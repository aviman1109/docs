+++
date = "2015-12-05T16:00:21-08:00"
draft = false
title = "Reverse Proxy"
weight = 31
toc = true


[menu.main]
	parent="proxy"
+++

# Overview

This section of the documentation provides sample configurations for using Drone with reverse proxy servers such as nginx. When using a reverse proxy it is imperative you set the `X-Forwarded-Proto` and `X-Forwarded-For` header variables. You will also need to run the Drone container using an alternate exposed port, for example `--port-8000:8000`.

# Nginx

Example nginx reverse proxy configuration:

```
location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header Origin "";

    proxy_pass http://127.0.0.1:8000;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_buffering off;

    chunked_transfer_encoding off;
}
```

# Caddy

Example [caddy server](https://caddyserver.com/) reverse proxy configuration:

```
drone.mycomopany.com {
    proxy / localhost:8000 {
        websocket
        proxy_header X-Forwarded-Proto {scheme}
        proxy_header X-Forwarded-For {host}
        proxy_header Host {host}
    }
}
```