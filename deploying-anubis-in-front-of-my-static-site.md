+++
title = "Deploying Anubis in Front of My Static Site"
description = "I walk through adding Anubis to the static website stack, and change the file server from Caddy to Static Web Server"
authors = ["Brandon Phillips"]
template = "blog_post.html"
date = "2025-07-07"
draft = false
[taxonomies]
tags = ["devops", "anubis", "static-site"]
+++

So I just put out a [post](@/blog/new-site-deployment-and-domain.md) talking about my new site deployment setup, but lo and behold, I've got some new stuff added to it with [Anubis](https://anubis.techaro.lol/) and [Static Web Server](https://static-web-server.net/)!
<!-- more -->

Anubis is a neat little Go tool that sits in front of your website and weighs incoming requests to block unwanted bot and scraper traffic. It's super configurable, fast, and [getting really popular](https://www.404media.co/the-open-source-software-saving-the-internet-from-ai-bot-scrapers/) in the wake of all the AI scrapers plaguing the internet. I've decided to give it a try in front of my public-facing static site.

The setup was really simple, First I added the following to my `compose.yml`:
```
# compose.yml
...
anubis:
  image: ghcr.io/techarohq/anubis:latest
  pull_policy: always
  environment:
    BIND: ":3000"
    TARGET: http://static-web-server:80
#    TARGET: http://httpdebug:3000

# This is for troubleshooting and debugging the requests and headers
# httpdebug:
#   image: ghcr.io/xe/x/httpdebug
#   pull_policy: always

static-web-server:
  build:
    dockerfile: ./conf/Containerfile
  container_name: "static-web-server"
  ports:
    - :80
  restart: unless-stopped
  environment:
    - SERVER_ROOT=/path/to/public/files
    - SERVER_CONFIG_FILE=/etc/config.toml
  volumes:
    - ./server.toml:/etc/config.toml
```

Most of this speaks for itself; the compose file pulls the Anubis image and we set it to listen on port `3000`. The `TARGET` variable here is where we want to point Anubis to after the challenges are completed, this is our Static Web Server container serving the website. The Anubis docs also include a `httpdebug` image for debugging the requests and headers.

The Static Web Server container points to the same `Containerfile` that the Caddy container was *previously* building from. We only changed the second piece there to pull the SwS image instead of Caddy, then give the Zola build files to it.
```
# Containerfile
FROM ghcr.io/getzola/zola:v0.20.0 AS zola

USER 0
COPY ../src /build
WORKDIR /build
RUN ["zola", "--config", "zola.toml", "build"]

FROM joseluisq/static-web-server:2-alpine
WORKDIR /
COPY --from=zola /build/public /public
```
Lastly, there's a few new changes needed in the Caddyfile.
```
# Caddyfile
{
	acme_ca {$ACME_CA_URL}
}

https://btp.dev {
	tls email@example.com

	reverse_proxy http://anubis:3000 {
		header_up X-Real-Ip {remote_host}
		header_up X-Http-Version {http.request.proto}
	}
}
```
So Caddy is now functioning as a stand-alone reverse proxy, instead of serving the static files, and is terminating the TLS. For Anubis to pass the traffic properly, we need to tell caddy in the `reverse_proxy` block here to also pass the headers `X-Real-Ip` and `X-Http-Version`. Anubis does the rest of the work directing traffic back to the Static Web Server.

I thought Anubis was a neat piece of software and wanted to give it a shot. I haven't noticed any unwanted traffic on the site, like web scrapers, but just because I haven't noticed it, doesn't mean it's not happening!
