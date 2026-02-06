+++
date = '2026-02-05'
draft = false
title = 'What? Why? How? - My First Post!'
tags = ["hugo", "mana", "github-pages", "cloudflare-dns", "how-to", "show'n'tell"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# My first blog post!

I thought I would tell a little bit about my short journey on how I set up this page, what it is for and then a little bit about my project!

### What is this page for?

First of all, this site is a personal blog where I'll be documenting my journey through my _3rd term_ as and _AP in Computer Sciene_, along with various other projects and experiments I'm working on along the way!

As part of out term project, we were encouraged to create a blog-style log of our weekly progress, not just to show _what_ we built, but _how_ we approached problems, made decisions and learned from the process. This site is my take on that idea.

Alongside updates from my main project, I'll also use this space to write about side projects, technical challenges, tools I'm learning to use, and the occasional deep dive when something interesting (or frustrating..) comes up.

In short: this page exists to track my progress, refelct on my work, and keep everything in one place! Both for myself and for anyone curious enough to follow along!

### Why?

The short answer: **structure**, **accountability** and **visibility**.

During previous terms I've noticed that it's very easy to do a lot of work without really stopping to reflect on _why_ certain decisions were made or what I actually learned along the way.
Writing things down forces me to slow down, think and relfect, both to myself, and to others.

Another reason is that I want this site to work as a kind of **living portfolio**, not just a list of finished projects, but a place that shows my personal _progress_, _mistakes_, _refactors_, and _learning moments_. I think that tells a more honest story, showing we all come from somewhere, learn along the way as we make mistakes and can acknowledge them! Much rather than a polished "look what i build!" page.

And finally: **motivation!** <br>
Knowing that I'll write a short post about each milestone makes me want to keep things moving and finish features properly instead of half-parking them in my head and moving on to the next step!

### How did I create this site?

This site is built using **[Hugo](https://gohugo.io/)**, a static site-generator, together with the **[Mana](https://github.com/Livour/hugo-mana-theme)** theme. I wanted something that was both _fast_, _simple_, _easy to host_ and _deploy automatically_.<br>
Hugo turned out to be a really good fit for that. Content is written in plain Markdown files, which means I can focus on writing instead of fighting a CMS or a heavy frontend framework.

The Mana theme gave me a clean and minimal starting point with support for:

- Blog posts
- Tags
- An about page
- And a nice overall layout without too much visual noise

The site itself is hosted using **GitHub Pages**, and it's connected to my personal custom domain.
That means every time i push a new commit to GitHub, for example when I add a new blog post, the site _automatically_ rebuilds and deploys. No manual uploads, no servers to maintain, no hassle.

This setup also fits really well with how I already work on projects: _Git_, _commits_, _small changes_, _repeat_.

#### The steps I went through (with commands)

To make the setup as smooth as possible, I followed a fairly simple workflow and relied mostly on the terminal and Visual Studio Code.

**1. Installing Hugo**

I worked on both macOS and Windows, so I installed Hugo slightly differently on each system.

On **macOS**, I used Homebrew:

> brew install hugo

On **Windows**, I installed Hugo using winget:

> winget install Hugo.Hugo.extended

> [!TIP] Hugo Extended version
> Make sure you install the extended version in both cases, if not using these commands, as some themes (including Mana) rely on features like SCSS processing

To verify the installation, I ran

> hugo version

**2. Creating a new Hugo project**

Once Hugo was installed, I created a new site using Hugo's built-in command, making sure I was in the correct directory:

> hugo new site portfolio

This generated the basic folder structure needed for the site.

I then opened the project in **Visual Studio Code**, this was just my preffered code editor, you can use any you are familiar with!

From here on, most of the work was done directly inside VS code!

**3. Adding the Mana theme as a Git submodule**

Instead of copying the them files directly into the project, I added the Mana theme as a **Git submodule** as the Mana documentation recommends! This keeps the theme separate from my own content and makes updates easier later on.

From the project root in terminal:

> git submodule add https://github.com/Livour/hugo-mana-theme.git themes/mana

After cloning the project on another machine, I had to run:

> git submodule update --init --recursive

to make sure the theme files were actually pulled in.. something I learned the hard way..

**4. Configuring Hugo**

With the theme in place, I updated the Hugo configuration file to set things like:

- Site title
- Theme name
- Menus
- Tags
- Author information
- Base URL for deployment
  This tells Hugo how to structure and render the site.

**5. Creating pages and blog posts**

Pages and posts are created using Hugo's built-in commands.

For example, creating an About page:

> hugo new about/index.md

And creating a new blog post:

> hugo new posts/my-first-post.md

All content is written in **Markdown**, which makes it quick and free of distractions to write and edit.

**6. Running the site locally**

To preview the site while working on it, I ran:

> hugo server -D

This starts a local development server and includes draft posts so I can see everything as I work.

**7. Version control and deployment**

Once everything was set up, I initalized a Git repository, pushed it to Github, and configured GitHub Pages with GitHub Actions. This means the site automatically builds and deploys whenever I push changes, including new blog posts.<br>
This was done using the official Hugo documentation, found under _Docs -> Host and Deploy -> Host on GitHub Pages_ .. or [click here](https://gohugo.io/host-and-deploy/host-on-github-pages/) for direct link.

After the site was deployed using GitHub Pages, I connected a custom domain using **Cloudflare DNS**, so the site is avaiable under my own domain. This can take some time, before the DNS updates through the internet, so patience was needed!

Just make a **DNS record** where the *type* is **CNAME** where ever you have your DNS, I'm using Cloudflare, here, make sure you uncheck the orange proxy button so its **DNS Only**

*Name* should be what ever you want infront of your domain, here i used *portfolio*, and then **Target** is your GitHub page: *<user_name>.github.io* eg.: toby.github.io

Last thing to do is to set baseUrl in the *hugo.toml* ! Mine looks like this:
>baseURL = 'https://portfolio.obli.dk/'

Its in the very top of the file.

Remember to commit and push your changes.

Thats it! If you had patience, your site should now be up and running, and looking fiiiine!

Thanks for reading!