<div align="center" width="100%">
    <h1>SecureTheJuice</h1>
    <p>OWASP Juice Shop hosted by Traefik SSL Reverse Proxy and Authelia Single-Sign-On (SSO) provider.</p><p>
    <a target="_blank" href="https://github.com/l4rm4nd"><img src="https://img.shields.io/badge/maintainer-LRVT-orange" /></a>
    <a target="_blank" href="https://GitHub.com/l4rm4nd/SecureTheJuice/graphs/contributors/"><img src="https://img.shields.io/github/contributors/l4rm4nd/SecureTheJuice.svg" /></a><br>
    <a target="_blank" href="https://GitHub.com/l4rm4nd/SecureTheJuice/commits/"><img src="https://img.shields.io/github/last-commit/l4rm4nd/SecureTheJuice.svg" /></a>
    <a target="_blank" href="https://GitHub.com/l4rm4nd/SecureTheJuice/issues/"><img src="https://img.shields.io/github/issues/l4rm4nd/SecureTheJuice.svg" /></a>
    <a target="_blank" href="https://github.com/l4rm4nd/SecureTheJuice/issues?q=is%3Aissue+is%3Aclosed"><img src="https://img.shields.io/github/issues-closed/l4rm4nd/SecureTheJuice.svg" /></a><br>
        <a target="_blank" href="https://github.com/l4rm4nd/SecureTheJuice/stargazers"><img src="https://img.shields.io/github/stars/l4rm4nd/SecureTheJuice.svg?style=social&label=Star" /></a>
    <a target="_blank" href="https://github.com/l4rm4nd/SecureTheJuice/network/members"><img src="https://img.shields.io/github/forks/l4rm4nd/SecureTheJuice.svg?style=social&label=Fork" /></a>
    <a target="_blank" href="https://github.com/l4rm4nd/SecureTheJuice/watchers"><img src="https://img.shields.io/github/watchers/l4rm4nd/SecureTheJuice.svg?style=social&label=Watch" /></a><p>
    <a href="https://www.buymeacoffee.com/LRVT" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>
</div>

## ✨ Requirements
- Docker for Linux
- Docker Compose for Linux
- Valid domain or proper `/etc/hosts` setup for fictive domain

## 🎓 Configuration

1. Adjust the `docker-compose.yml` file to your needs. Especially adjust the traefik labels and example domain `fictive.local` to your valid domain, if available.
2. Adjust the `traefik/fileConfig.yml` to your needs. Especially adjust the Authelia example domain `fictive.local` to your valid domain, if available.
3. Adjust the `authelia/config/configuration.yml` to your needs. Especially adjust the Authelia example domain `fictive.local` to your valid domain, if available and all default secrets.
4. Adjust the `authelia/config/user_database.yml` to your needs. Especially adjust the default users and secrets.

If you do not have an own domain and registrar for DNS setup, you may keep using the `fictive.local` domain as is. If so, please ensure to properly setup your Linux's `/etc/hosts` file. I recommend the following entries:

````
127.0.0.1       fictive.local auth.fictive.local juice.fictive.local traefik.fictive.local
````

## 💎 SSL Certificates

Traefik is configured to use HTTP challenge. You will obtain valid Let's Encrypt SSL certificates if:

- You use your own domain with proper DNS entries setup
- You run this project on your server, which has the IP address that your domain is publicly resolved to
- You expose TCP/80 of the Traefik reverse proxy to the public Internet

As an alternative, you may adjust the Traefik configuration to use DNS challenge. This setup is not part of this GitHub repo though.

If the HTTP challenge fails, Traefik will issue self-signed SSL certificates.

## 🏃 Running

````
docker network create proxy
docker compose up -d
````

The OWASP Juice Shop web application is run behind Traefik + Authelia. Only TCP/80 (HTTP) and TCP/443 (HTTPS) of the Traefik container are mapped onto the Docker host.

If you haven't changed the project files and ensured proper `/etc/hosts` entries, you will be able to access:

- Authelia Login page at https://auth.fictive.local
- Juice Shop at https://juice.fictive.local
    - after Authelia login with default creds `SecureTheJuice:SecureTheJuice` 
- Traefik API dashboard at https://traefik.fictive.local
    - from private class A networks only

## 🔑 Authentication via Authelia

In order to access the Juice Shop, you will have to authenticate against Authelia first.

The default Authelia users are:

| Username | Password |
| :---------- | :--------- |
| SecureTheJuice  | SecureTheJuice  |

You can freely adjust users and groups at `authelia/config/users_database.yml`.

## 🔏 Authorization via Authelia

In order to access the Juice Shop, you will have to authenticate against Authelia first.

The access controls are defined in Authelia's configuration file `authelia/config/configuration.yml`.

The default user group `fruitlovers` is allowed to gain access. The user `SecureTheJuice` is member of this group.
