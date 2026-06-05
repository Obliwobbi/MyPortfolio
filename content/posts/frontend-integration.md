+++
date = '2026-06-05T12:30:04+02:00'
draft = false
title = 'Frontend integration - API, authentication, roles and deployment'
tags = ["react", "api", "jwt", "routing", "deployment", "caddy"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Frontend integration - API, authentication, roles and deployment

This part of the frontend project focused on connecting my React application to the backend API.

The goal was no longer just to make static pages. The frontend had to communicate with the backend, handle login, store a JWT token, fetch protected data, show different UI depending on the logged-in user and finally be deployed behind my domain.

This was the part of the project where the frontend and backend really started working together.

## Connecting React to the backend API

In the beginning, I made API calls directly inside components using `fetch`.

For example, the login page originally sent a request directly to the backend URL.

That worked, but it quickly became messy because every page would need to repeat:

* base URL
* headers
* JSON parsing
* error handling
* authorization token

To avoid that, I created an `apiClient.js`.

The idea was to have one central place for API communication.

Instead of writing `fetch` directly in every page, I could write:

```jsx
const data = await apiGet('/users')
```

or:

```jsx
const data = await apiPost('/login', {
  email,
  password,
}, false)
```

This made the frontend cleaner and made it easier to change the API base URL later.

## Handling API responses

In the API client, I added a helper for handling responses.

The goal was to avoid repeating the same error handling in every component.

The response handler checks if the response is okay. If not, it tries to read the error message from the backend.

```js
async function handleResponse(response) {
  if (!response.ok) {
    let errorMessage = `API error: ${response.status}`

    try {
      const errorData = await response.json()
      errorMessage = errorData.message ?? errorMessage
    } catch {
      // fallback if the API does not return JSON
    }

    throw new Error(errorMessage)
  }

  if (response.status === 204) {
    return null
  }

  return await response.json()
}
```

This was useful because the pages could use normal `try/catch` blocks.

For example:

```jsx
try {
  const data = await apiGet('/users')
  setUsers(data)
} catch (error) {
  setErrorMessage(error.message)
} finally {
  setIsLoading(false)
}
```

This gave me a better understanding of how frontend error handling should work with HTTP errors.

## Async and await

Fetching data from an API is asynchronous because the frontend has to wait for the backend to respond.

I used `async` and `await` in several places.

For example, the users page fetches users inside `useEffect`:

```jsx
useEffect(() => {
  async function fetchUsers() {
    try {
      const data = await apiGet('/users')
      setUsers(data)
    } catch (error) {
      setErrorMessage(error.message)
    } finally {
      setIsLoading(false)
    }
  }

  fetchUsers()
}, [])
```

This taught me that React itself does not wait for data automatically. I have to handle loading state, error state and the final data state.

The empty dependency array means the effect runs once when the component is mounted.

```jsx
}, [])
```

I also used a dependency array with route changes in the header, so the header could refetch the logged-in user when navigation changed.

## Login with JWT

The login flow uses JWT authentication.

The flow is:

```text
1. User enters email and password
2. Frontend sends POST /login
3. Backend validates the credentials
4. Backend returns a JWT token
5. Frontend stores the token
6. Future API requests include the token in the Authorization header
```

In the login page, after a successful login, the token is stored in `localStorage`:

```js
localStorage.setItem('token', data.token)
```

Then the API client adds the token to protected requests:

```js
headers.Authorization = `Bearer ${token}`
```

This made it possible to call protected endpoints like:

```text
GET /api/v1/users
GET /api/v1/companies
GET /api/v1/me
```

## localStorage

I used `localStorage` to store the JWT token.

The benefit of `localStorage` is that the token remains after a page refresh.

That means the user does not get logged out just because the page reloads.

A simplified example:

```js
localStorage.setItem('token', data.token)
const token = localStorage.getItem('token')
localStorage.removeItem('token')
```

I also used `localStorage.removeItem('token')` when the user logs out.

One important reflection is that storing tokens in the browser always has security considerations. For this project, localStorage was a simple and practical solution, but I understand that in larger production systems, token handling can be more advanced.

## The /me endpoint and current user state

After login, the frontend needs to know who is logged in.

For that, I used the `/me` endpoint.

The `/me` endpoint returns information about the logged-in user, such as:

```json
{
  "id": 1,
  "email": "admin@obli.dk",
  "firstname": "Admin",
  "lastname": "User",
  "role": "SYSTEM_ADMIN",
  "companyId": 1,
  "companyName": "Membersystem Bootstrap Company"
}
```

The header uses this endpoint to decide whether to show the login button or the user menu.

If there is no token, the header shows public navigation.

If there is a valid token, the header shows user-specific navigation.

## Role-based UI

The application has three main roles:

```text
SYSTEM_ADMIN
COMPANY_ADMIN
MEMBER
```

I used the logged-in user's role to change the frontend navigation.

For example:

```text
Logged out:
Home | Features | How it works | Login

SYSTEM_ADMIN:
Users | Companies | UserMenu

COMPANY_ADMIN:
My Users | My Company | UserMenu

MEMBER:
My Profile | My Company | UserMenu
```

The function for generating menu items checks the current user:

```js
function getMenuItems(user) {
  if (!user) {
    return [
      { label: 'Home', to: '/' },
      { label: 'Features', to: '/features' },
      { label: 'How it works', to: '/how-it-works' },
    ]
  }

  if (user.role === 'SYSTEM_ADMIN') {
    return [
      { label: 'Users', to: '/users' },
      { label: 'Companies', to: '/companies' },
    ]
  }

  if (user.role === 'COMPANY_ADMIN') {
    return [
      { label: 'My Users', to: '/users' },
      { label: 'My Company', to: '/companies' },
    ]
  }

  return [
    { label: 'My Profile', to: `/users/${user.id}` },
    { label: 'My Company', to: '/companies' },
  ]
}
```

This made the UI feel more relevant to the user.

A member does not need to see the same menu as a system admin.

## Frontend role checks are not security

One of the most important reflections I made during this part was that frontend role checks are only for user experience.

They are not real security.

Even if I hide a button in React, a user could still manually send a request with curl or through browser developer tools.

Therefore, the backend must still check the user's role and company before returning or changing data.

For example, the frontend can hide `SYSTEM_ADMIN` from a company admin's role dropdown, but the backend must still reject the request if a company admin tries to promote someone to system admin.

This helped me understand the difference between:

```text
Frontend role-based UI = user experience
Backend authorization = actual security
```

## Protected routes

I also created a `ProtectedRoute` component.

The idea is simple: if there is no token, the user should not access protected pages in the frontend.

```jsx
function ProtectedRoute({ children }) {
  const token = localStorage.getItem('token')

  if (!token) {
    return <Navigate to="/login" replace />
  }

  return children
}
```

This is used around protected pages like users and companies.

```jsx
<ProtectedRoute>
  <UsersPage />
</ProtectedRoute>
```

This improves the user experience, because unauthenticated users are redirected to login.

But again, the backend still needs to protect the data.

## React Router and navigation

React Router was used throughout the frontend.

I used:

* `BrowserRouter`
* `Routes`
* `Route`
* `NavLink`
* `Outlet`
* `useNavigate`
* `useParams`
* `useLocation`
* `useSearchParams`

Each of these solved a specific problem.

`NavLink` helped style active menu links.

`useNavigate` helped redirect users after login and after saving edit forms.

`useParams` helped details pages read ids from the URL.

`useLocation` helped remember where a user came from.

`useSearchParams` helped preserve filtering on the users page.

This made me understand that routing is not just about changing pages. It also controls navigation flow and how state can be represented in the URL.

## Register flow

I also added a register page.

The register page lets a new user select a company from a dropdown and create an account.

At first, this caused a problem because `/companies` became protected. The register page could no longer fetch companies without a token.

The solution was to create a separate public endpoint for the register dropdown:

```text
GET /api/v1/companies/public
```

That endpoint only returns the company id and name, which is enough for registration.

This taught me that not all endpoints have the same purpose.

A protected `/companies` endpoint can be role-filtered, while a public `/companies/public` endpoint can be used for a limited signup flow.

## CORS issues during development

While developing locally, my React frontend ran on:

```text
http://localhost:5173
```

and my backend ran on:

```text
http://localhost:7070
```

Even though both are localhost, they are different origins because they use different ports.

The browser blocked requests because of CORS.

This was confusing at first, because curl worked, but the browser did not.

The reason is that CORS is enforced by browsers.

I fixed this by allowing the frontend origin in the backend CORS configuration.

This taught me the difference between:

```text
curl request
browser request
```

and why CORS only appears as a frontend/browser issue.

## Deployment setup

For deployment, I built the frontend as a Docker image and served it behind Caddy.

The deployment flow is:

```text
Push code to GitHub
GitHub Actions builds Docker image
Docker image is pushed to Docker Hub
Droplet pulls the new image
Caddy serves the frontend and proxies API requests
```

The React app is built with Vite.

When building the frontend, Vite creates static files in the `dist` folder.

These files can be served by a web server.

In deployment, the frontend runs in its own container, while the backend runs in another container.

Caddy handles the public domain.

## Caddy and routing

Caddy is used as the public entry point.

The desired structure is:

```text
https://membersystem.obli.dk
    -> React frontend

https://membersystem.obli.dk/api/v1
    -> Java/Javalin backend
```

This means the frontend and backend can share the same domain.

That is useful because it avoids many CORS problems in production.

The important part was making sure that Caddy did not remove `/api/v1` from the request before sending it to the backend.

Earlier, I had used `handle_path`, which strips the path prefix. But after moving API versioning into the codebase, the backend expected `/api/v1`.

So the Caddy setup had to forward the full path to the backend.

This was a useful deployment lesson: reverse proxy configuration and backend routing must match.

## HTTPS

Caddy also handles HTTPS certificates.

This means users can access the deployed site through:

```text
https://membersystem.obli.dk
```

instead of using an IP address and port.

HTTPS is important because login credentials and JWT tokens should not be sent over plain HTTP.

Caddy made this easier because it can automatically handle certificates for the domain.

## Reflections

This integration phase was one of the most important parts of the frontend project.

It connected many concepts:

* React components
* API calls
* async/await
* error handling
* JWT authentication
* localStorage
* protected routes
* role-based UI
* CORS
* deployment
* Caddy reverse proxy
* HTTPS

I also learned that many frontend problems are actually full-stack problems.

For example:

* a login issue can be caused by wrong API base URL
* a CORS issue can be caused by backend configuration
* a broken deployed route can be caused by Caddy rewriting paths
* a missing role check can look like a frontend issue but must be fixed in backend
* a register dropdown can fail because the endpoint requires authentication

The biggest takeaway from this part is that a frontend application is not isolated.

It depends heavily on how the backend, API routes, authentication, deployment and server configuration are designed.

I now have a much better understanding of how a React frontend works together with a backend API in a deployed full-stack application.
