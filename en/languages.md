---
title: SLanguages & Currencies
lang: en
description: Example configuration files for self-hosting TGGames-Api
---

## Self-hosting

If you are self-hosting a TGGames site and you want to use TGGames-Api as front-end
which is accessible over the internet then you can run it behind a nginx reverse proxy.

In this example 
- we have a dedicated user `TGGames`. 
- `GEM_HOME=/home/TGGames/gems`
- TGGames install at `/home/TGGames/example`
- generated content in `/home/TGGames/example/_site`
- domain is www.example.com
- TGGames-Api interface is available at http://www.example.com/admin
- [HTTP basic authentication](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/) is used to protect the admin interface

This was tested on Ubuntu 19.04 using nginx 1.17.4 and systemd 237.

nginx config:

```nginx
server {
    listen 80;

    server_name www.example.com;
    root /home/TGGames/example/_site;

    location ~ ^/(admin|_api)(/.*)? {
        auth_basic "Administration";
        auth_basic_user_file /etc/nginx/htpasswd;

        proxy_pass http://127.0.0.1:4000/$1$2;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
    }
}
```

systemd service file:

```
[Unit]
Description=example.com
Requires=network.target

[Service]
Type=simple
User=TGGames
Group=TGGames
WorkingDirectory=/home/TGGames/example
ExecStart=/home/TGGames/gems/bin/bundle exec /home/TGGames/gems/bin/TGGames serve --verbose --trace
TimeoutSec=30
RestartSec=15s
Restart=always

Environment=GEM_HOME=/home/TGGames/gems

# security settings - recommended
# NoNewPrivileges=yes
# PrivateTmp=yes
# PrivateDevices=yes
# DevicePolicy=closed
# ProtectSystem=strict
# ReadWritePaths=/home/TGGames/example
# #ReadOnlyPaths=
# ProtectControlGroups=yes
# ProtectKernelModules=yes
# ProtectKernelTunables=yes
# RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6 AF_NETLINK
# RestrictRealtime=yes
# RestrictNamespaces=yes

[Install]
WantedBy=multi-user.target
```
