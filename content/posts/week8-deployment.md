+++
date = '2026-04-08T01:00:00+01:00'
draft = false
title = 'Deployment - From Local Development to Production'
tags = ["java", "docker", "deployment", "ci/cd", "devops"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Deployment - From Local Development to Production

This week was where everything came together, and also where everything broke.

Up until this point, the system had only existed locally on my machine.  
This week was about getting it running in a real environment, accessible through a domain.

That meant dealing with:
- Docker
- CI/CD (Continious Integration / Continious Deployment)
- environment variables
- networking
- and a lot of debugging

## From local to deployed system

Locally, everything worked:
- database connection
- API endpoints
- authentication
- testing

But deployment introduced a completely new layer of complexity.

Suddenly, I had to think about:
- how the application is packaged
- how it runs in a container
- how it connects to other services
- how it is accessed externally

## Deployment setup

The deployment process consisted of several steps:

1. **Build the application**
   - Maven builds a fat JAR using the shade plugin

2. **Create a Docker image**
   - The JAR is packaged inside a container
   - The application is configured to run on a specific port

3. **Push the image to Docker Hub**
   - GitHub Actions builds and pushes the image automatically on push

4. **Run the container on a Digital Ocean droplet**
   - Docker Compose is used to run the application together with PostgreSQL

5. **Expose the application through a domain**
   - Caddy is used as a reverse proxy
   - The API is available at:
     ```
     /api/v1
     ```

6. **Automatic updates**
   - Watchtower monitors for new Docker images
   - Automatically pulls and restarts the container when a new version is available

This pipeline allowed me to go from:
> pushing code > to having a running system online

## Challenges and debugging

This week involved a lot of trial and error.

### Application not starting

At one point, the application kept restarting in a loop.

The issue turned out to be:
- a database constraint violation
- caused by my bootstrap logic inserting duplicate data

Because the container restarted automatically, the error repeated continuously.

This made it harder to debug, but also taught me how important it is to:
- read logs carefully
- understand startup behavior

---

### Configuration issues

One of the biggest challenges was configuration.

Problems included:
- environment variables missing in Docker
- config files not being loaded inside the container
- differences between local and deployed setup

A specific issue was:
- password not loading correctly from config
- which caused login to fail even though the user existed

This forced me to rethink how configuration should be handled.

The solution was to rely on environment variables instead of local files.

---

### CI/CD and failing tests

Before deployment could work, the CI pipeline had to pass.

I ran into issues where:
- tests failed in GitHub Actions but worked locally
- configuration was missing in the CI environment

This required:
- setting up a test database in the workflow
- ensuring environment variables were available during tests

This was one of the more frustrating parts, but also one of the most valuable.

---

### Private Docker repository issues

Another issue was related to Docker Hub.

Since the repository was private:
- the server could not pull the image initially
- resulting in errors like:
  - access denied
  - repository not found

The solution was to:
- log in to Docker Hub on the server
- ensure credentials were available for pulling images

---

### Watchtower and automatic updates

Setting up Watchtower also introduced some issues.

At first:
- it could not pull images due to missing authentication
- it attempted to update containers that were not accessible

This was due to not having the correct root folder in the watchtower config. <br> Documentation said "/root/.docker/...", but mine should be "/home/{user}"

Once configured correctly, it worked as intended:
- detected new images
- restarted the container automatically

This made deployment much smoother.

---

### Networking and reverse proxy

Another challenge was getting the application accessible through the domain.

Issues included:
- connection refused errors
- incorrect ports
- containers not being reachable

This required:
- matching internal container ports with external ports
- configuring Caddy correctly
- verifying that the application was actually running

---

## What I learned

This week taught me more than any other part of the project.

Some key lessons:

- A system that works locally can still fail completely in production  
- Configuration is one of the hardest parts of deployment  
- Logs are essential for debugging  
- Docker simplifies deployment, but introduces its own complexity  
- CI/CD pipelines require a stable and testable setup  
- Small mistakes ("*cough*" .. *like missing environment variables* .. "*cough*") can break everything  

## Final result

At the end of this process, I had:

- a running backend deployed on a server  
- a domain pointing to the application  
- a CI/CD pipeline building and pushing images  
- automatic updates using Watchtower  

The API is now accessible externally and behaves as expected.

## Key takeaways

This week taught me that:
- deployment is not just a final step, it is a separate discipline  
- real-world systems require more than just code  
- debugging in production is very different from local debugging  
- automation (CI/CD) is extremely powerful when it works  

This was the week where the project went from being “just code”  
to becoming a **real deployed system**.