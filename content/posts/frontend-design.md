+++
date = '2026-06-03T12:18:03+02:00'
draft = false
title = 'Frontend design - From mockup to React'
tags = ["react", "frontend", "components", "ui"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Frontend development - From mockup to React application

This part of the project focused on turning my backend API into an actual frontend application.

Before this phase, most of my project was backend-focused. I had built REST endpoints, authentication, user management, company management and database logic, but the system was mainly something I could test through curl requests.

The goal of the frontend was to build a React application that made the system feel more like a real product.

I wanted the frontend to:

* show a proper landing page
* allow users to log in and register
* display users and companies
* show details pages
* support editing users and companies
* react differently depending on the logged-in user's role
* work both on desktop and mobile

This meant that I had to think more about user experience, component structure, routing, state and styling.

## Starting with a visual direction

I started by creating a visual idea for the application.

The first mockups were not meant to be perfect finished designs. They were more like a starting point, so I could get a feeling for what the system should look like.

I chose a dark theme with purple accents because I wanted the application to feel modern, clean and a little more polished than a plain admin interface.

The main visual direction became:

* dark background
* purple highlights
* card-based layouts
* rounded corners
* clear action buttons
* readable user and company cards

This helped a lot, because I was not just coding random pages. I had a direction to follow.

## Setting up the React structure

One of the first decisions was how to structure the React project.

I chose to split the project into **pages** and **components**.

Pages are full views connected to routes:

```text
FrontPage
LoginPage
RegisterPage
UsersPage
UserDetailsPage
UserEditPage
CompaniesPage
CompanyDetailsPage
CompanyEditPage
FeaturesPage
HowItWorksPage
```

Components are smaller reusable UI parts:

```text
Header
UserMenu
UserCard
RoleBadge
FeatureCard
ProtectedRoute
```

This separation made the project easier to understand.

A page is responsible for a full route, while a component is responsible for a smaller piece of UI.

For example, the `UsersPage` does not need to know exactly how every user card is styled. It can simply render a `UserCard` for each user:

```jsx
{filteredUsers.map((user) => (
  <UserCard key={user.id} user={user} />
))}
```

That made the code easier to read and easier to change later.

## Thinking in components

A big part of learning React was learning to think in components.

In backend development, I am used to thinking in layers like controllers, services, DAOs and entities. In React, I had to think more in terms of user interface blocks and data flow.

A component is basically a reusable function that returns JSX.

For example, my `UserMenu` component receives a user and a logout function:

```jsx
<UserMenu user={user} onLogout={handleLogout} />
```

This means the component does not need to know how login works or where the user came from. It just displays the user information and calls `onLogout` when the logout button is clicked.

That was an important learning point for me: components become easier to reuse when they receive what they need through props instead of handling everything themselves.

## Props and reusable UI

Props are used to pass data into components.

I used props in several places, for example:

```jsx
<UserCard user={user} />
<RoleBadge role={user.role} />
<UserMenu user={user} onLogout={handleLogout} />
```

This helped me avoid repeating the same UI logic.

One example is the role badge. The backend sends roles like:

```text
SYSTEM_ADMIN
COMPANY_ADMIN
MEMBER
```

That is fine for the backend and database, but it does not look very user-friendly in the frontend.

So I created a `RoleBadge` component that formats the role text and styles it visually.

Instead of showing:

```text
SYSTEM_ADMIN
```

the frontend can show:

```text
System Admin
```

This gave me a better understanding of how the frontend can make backend data more readable without changing the backend model.

## State in React

Another major part of the frontend was learning how state works.

I used `useState` for values that can change while the user interacts with the application.

For example, on the login page I used state for:

```jsx
const [email, setEmail] = useState('')
const [password, setPassword] = useState('')
const [errorMessage, setErrorMessage] = useState('')
const [isLoading, setIsLoading] = useState(false)
```

This made the form reactive. When the user types in an input field, React updates the state, and the UI updates automatically.

I also used state on pages that fetch data, such as the users page:

```jsx
const [users, setUsers] = useState([])
const [isLoading, setIsLoading] = useState(true)
const [errorMessage, setErrorMessage] = useState('')
```

This made it possible to show different UI states:

* loading
* error
* empty result
* data loaded successfully

That was one of the first places where React started to make more sense to me. The page is basically a result of the current state.

## Controlled forms

The login, register and edit pages use controlled form inputs.

For example:

```jsx
<input
  value={email}
  onChange={(event) => setEmail(event.target.value)}
/>
```

This means React controls the value of the input.

The input value is stored in state, and every time the user types, the state is updated.

This made forms easier to work with because I could collect all form values from state when submitting.

I used the same concept on the edit pages, where the form is first filled with data from the API and then updated through state.

## Routing and pages

I used React Router to make the frontend work like a multi-page application while still being a single page application.

The main structure uses routes like:

```text
/
 /login
 /register
 /users
 /users/:id
 /users/:id/edit
 /companies
 /companies/:id
 /companies/:id/edit
 /features
 /how-it-works
```

The `App.jsx` file acts as a layout:

```jsx
<Header />
<main>
  <Outlet />
</main>
```

The `Outlet` is where the active page is rendered.

This means the header stays visible, while the content inside the page changes depending on the route.

React Router also allowed me to use route parameters. For example, the user details page can read the user id from the URL:

```jsx
const { id } = useParams()
```

Then it can fetch:

```jsx
apiGet(`/users/${id}`)
```

This made it possible to build details and edit pages for users and companies.

## Preserving filter state with query parameters

On the users page, I added search and filtering.

The user can filter by:

* search text
* role
* company

At first, these values were stored only in React state. That worked until I navigated to a details page and then went back. The filters were lost because the page was reloaded.

To fix this, I used query parameters with React Router.

For example:

```text
/users?search=toby&role=MEMBER&company=ALL
```

This means the filter state is stored in the URL instead of only inside the component.

That was a useful learning point because it made the page more stable. The filter can survive navigation and refreshes, and the URL actually describes what the user is looking at.

## Conditional rendering

I used conditional rendering several places in the project.

For loading and errors:

```jsx
if (isLoading) {
  return <p>Loading users...</p>
}

if (errorMessage) {
  return <p>{errorMessage}</p>
}
```

For login state:

```jsx
{user ? (
  <UserMenu user={user} onLogout={handleLogout} />
) : (
  <NavLink to="/login">Login</NavLink>
)}
```

And for role-based UI:

```jsx
const canEditCompany =
  currentUser?.role === 'SYSTEM_ADMIN' ||
  currentUser?.role === 'COMPANY_ADMIN'
```

This helped me understand that React components can return different UI depending on state, props or user role.

## Styling the application

I used regular CSS files for each page and component.

For example:

```text
Header.jsx
Header.css

UsersPage.jsx
UsersPage.css

UserCard.jsx
UserCard.css
```

This made the CSS easier to connect to the component it belongs to.

One issue I ran into was that some headings appeared white in my browser, but black on another computer.

The reason was that the global CSS used `prefers-color-scheme`, which changed variables depending on whether the system was in light or dark mode.

This taught me that global CSS can affect the whole application in unexpected ways. I fixed it by making the dark theme explicit, so text and headings stay consistent for everyone.

## Responsive design

Towards the end of the frontend work, I also made the site more mobile-friendly.

The biggest change was the header.

On desktop, the header shows:

```text
Logo | Navigation links | User menu
```

On mobile, I changed it to a burger menu.

The mobile menu slides down over the page instead of pushing the content down. This made the mobile experience feel smoother and more like a real application.

I also adjusted cards, grids and forms so they work better on smaller screens.

This involved using media queries and changing layouts from multiple columns to one column.

For example, a user card can be shown in multiple columns on desktop, but stacked vertically on mobile.

## Reflections

This frontend phase made the project feel much more complete.

Before the frontend, I could show that the backend worked, but it was not easy for a normal user to interact with. After building the React frontend, the project became something that looked and felt like an actual application.

The biggest things I learned were:

* how to split an application into pages and components
* how props make components reusable
* how state controls dynamic UI
* how controlled forms work
* how React Router handles pages and nested routes
* how query parameters can preserve search/filter state
* how conditional rendering can change UI based on login state and roles
* how important global CSS decisions are
* how responsive design changes the way components should be structured

I also learned that frontend development is not just about making things look nice.

It is also about structure, data flow, user experience, error handling, routing and making sure the interface matches what the backend allows.

This phase gave me a much better understanding of how a frontend application is built on top of a backend API.
