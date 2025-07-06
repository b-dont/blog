+++
title = "New Site Deployment and Domain"
description = "I've set up a new domain for the site, and a new static site deployment workflow."
authors = ["Brandon Phillips"]
template = "blog_post.html"
date = "2025-06-30"
draft = false
[taxonomies]
tags = ["docker", "zola", "git", "devops"]
+++

Hello, world! It's been a while since I've posted on this blog, but better late than never, right? This is coming to you from a new domain [btp.dev](https://btp.dev) and a new workflow for this static site. I've been really getting into build and deployment pipelines lately, so I figured I'd write up a little blurb about what I've done for this site.
<!-- more -->
### Caddy
My old config was a simple nginx service running on a Linux manchine that was serving static files. This time, I wanted to take advantage of some newer software to build and serve the site. [Caddy](https://caddyserver.com/) comes out of the box with some really kick-ass features, like auto-HTTPS, reverse proxies, an admin API, and it's super easy to get it all up and running in a container environment. I went with this over nginx for the server.

All you really need is the `compose.yml` file provided in the docs. So far, Caddy is the only service that the compose file is building, but there's room to add other stuff in here in the future, so I'm going to keep it under a `compose.yml` for now. I've made some small adjustments to the doc's `compose` I'll talk about next.

```
# compose.yml
services:
  caddy:
    build:
      dockerfile: ./conf/Containerfile
    restart: unless-stopped
    ports:
      - ${CADDY_HTTP}
      - ${CADDY_HTTPS}
    volumes:
      - ./conf:/etc/caddy
      - caddy_data:/data
      - caddy_config:/config

volumes:
  caddy_data:
  caddy_config:
```

### Zola
The static site generation is still [Zola](https://www.getzola.org/); I've been very happy with it since I started using it a couple of years ago, and I don't have any reason to stop using it. Zola itself is also running in a container, generating the site's public files and handing them over to the Caddy image to serve. You'll notice the `dockerfile` under the `build` directive in the `compose.yml` file; this is pointing to the following `Containerfile`:
```
# Containerfile
FROM ghcr.io/getzola/zola:v0.20.0 AS zola

USER 0
COPY ../src /build
WORKDIR /build
RUN ["zola", "--config", "zola.toml", "build"]

FROM caddy:latest
WORKDIR /
COPY --from=zola /build/public /public
```
This `Containerfile` builds our site's container. First, we use the `FROM` instruction to pull the `zola` image so we can build the static files. We're running this as a rootful container to allow Zola to cleanup directories afterwards. We'll then need to use `COPY` on my `src` directory so Zola has something to build on, set the working directory, then execute Zola's build command.

Once Zola builds the site, we pull the Caddy image, and copy the build files over to the Caddy image, set the `/public` directory, and that's it!

### The Caddyfile
Caddy's config is even smaller than the Docker stuff:
```
# Caddyfile
{
        # Staging acme_ca
        # acme_ca https://acme-staging-v02.api.letsencrypt.org/directory

        # Production acme_ca
        acme_ca https://acme-v02.api.letsencrypt.org/directory
}

# Local dev
http://localhost:8000 {
        root * /public
        file_server
}

# Production 
https://btp.dev {
        root * /public
        file_server
}
```
The top brackets are the `Caddyfile`'s Global Options; here we've set the `acme_ca` variable to point at the LetsEncrypt acme CA directory, so Caddy can obtain a SSL certificate and serve the site over HTTPS. My last server was using Certbot to obtain the certs, and that was fine with an nginx service, but giving Caddy a single line and having it automatically serve HTTPS, regardless of where I'm running it (granted that LetsEncrypt can successfully complete the acme challenge) is so much more conveinent.

I've added a `localhost` site block here for local development, and then the production block for the website. We just tell Caddy where the root of the site is, call the `file_server` directive, and it does the rest.

### Deployment
I've written about [deploying static sites with git before](@/blog/static-site-workflow-using-git.md) and I'll be using those basic concepts here as well, with some small changes. The basic setup is three repositories:
```
~/btp.dev # Local repository - development
me@production-server: /btp.dev-bare # bare repository on the production server - we push to this
me@production-server: /blog-bare # a second bare repo for the blog submodule
me@production-server: /btp.dev # Production repository - this is where the site files and container are built
```
In my previous post, I laid out what bare repositories are and their directory structure, so I'll skip that here. We'll be using a `post-receive` git hook in the bare repository to deploy the blog and the site itself. This hook can be place in the `/hooks` directory in both the site and blog bare repos:
```
#!/bin/bash

site_directory=/path/to/btp.dev
blog_submodule=$site_directory/src/content/blog

# Unset the git env variables
unset "$(git rev-parse --local-env-vars)"; git -C $site_directory/.git describe --always

# Pull the origin main branch for the blog repo
cd $blog_submodule || exit
git --git-dir=$blog_submodule/.git pull origin main

# Pull the origin main branch for the website repo
cd $site_directory || exit
git --git-dir=$site_directory/.git pull origin main

# Bring down the site container and rebuild it
docker compose down && docker compose up --build -d
```
We'll use the same `unset` call here to unset the git environment variables, pull the blog main branch, then pull the site main branch, then bring down the site container, rebuild it and bring it back up. Git will display the output from all of this when you run `git push <remote> <branch>` to the bare repo.
