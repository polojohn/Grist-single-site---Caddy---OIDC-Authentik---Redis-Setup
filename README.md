# Grist-single-site | Caddy | OIDC-Authentik | Redis-Setup

## Introduction
These are my notes on setting up **Grist** with **Caddy** as a reverse proxy, **OIDC** for authentication, and **Redis** for API calls and webhooks, all managed using **Portainer**. The goal is to document what worked for me in achieving a setup that passes all self-checks in the Grist admin panel and maintains healthy Docker containers.

---

## Why These Notes?

I faced challenges setting up Grist, particularly getting the admin panel self-checks to pass and integrating it with **N8n** for webhooks.

### Links to some other guides that i used
   

   - This video helpled me with authentik settings https://www.youtube.com/watch?v=N5unsATNpJk
   -    -[Community Grist](https://community.getgrist.com/t/questions-about-self-hosted-grist-and-authentik-with-oidc/5250/8) 
   - [Detailed Grist Deployment Setup](https://github.com/hectorm/grist-deployment)

You could also use Google OpenID Connect authentication through the Google Identity Platform but i dont have notes on that:
  [Google Identity OAuth & OIDC Docs](https://developers.google.com/identity/protocols/oauth2)  
  [Set Up OAuth Consent & Credentials](https://console.cloud.google.com/apis/credentials)

- **Troubleshooting Tip:** Always use a **new private window** (or clear cookies) when testing setups to avoid misleading cache issues.
- **Volumes & Permissions:** Setting up persistence was straightforward, but **correcting file permissions** was necessary and caused some issues along the way.
- **Networking:** My setup includes a **backend network for internal communication** and a **VLAN front-end with a static IP**—your configuration might differ.

---

## What's Here

You will find 2 Docker Compose YAML files, supporting two different setups. Both have over the top explanations and hints that you can remove.
  
Currently, environment variables are included directly in the Docker Compose files instead of using a separate `.env` file. While not best practice, this keeps the setup self-contained. If preferred, you can refactor the variables into an `.env` file for better management and security.

There is also a snippet for a **Caddy file** that assumes you already have automatic HTTPS configured in the global options.


### **Example 1: Core Setup** (Recommended to get this working first)
Before deploying, create the required directory and set proper permissions:
Something like:
```sh
mkdir -p /home/john/docker/grist
chmod -R 755 /home/john/docker/grist
```

If things go wrong, remove all the folders and start again:
```sh
rm -rf /home/john/docker/grist/*
```
- Deploy **Grist** with **OIDC single sign-on** via **Authentik**.
- Use the **inbuilt SQLite database** for persistence.
- Configure **Caddy as a reverse proxy**.
- Enable **health checks** (this is especially useful for the next example).
---
### **Example 2: Add Redis**
Before deploying, create one extra folder and set proper permissions:
Something like:-
If things go wrong, remove all the folders and start again:

```sh
mkdir -p /home/john/docker/grist/redis
chown -R 999:999 /home/john/docker/grist/redis
chmod -R 770 /home/john/docker/grist/redis
```
- **Integrate Redis** to enable API calls and webhooks.

## What's Not Covered

These notes do **not** cover:
- **Setting up Authentik**
- **Integrating Miro**.
- **Using a PostgreSQL database**.
- **Multi-team site configurations**.

These features are not required for this specific setup but may be useful in other scenarios.

---

## Prerequisites

Before you begin, ensure you have:

- **Docker & Docker Compose installed (or Portainer)**.
- **Caddy installed** (as a service or container).
- **OIDC Provider configured** (e.g., Authentik, Google, etc.).
- **A domain with proper DNS settings**.

---

## About Me

I'm new to home lab setups and happy to follow guides from others. I decided to document what I did to get to this point, which involved a lot of asking chatbots to redo my Compose YAMLs, checking log errors, and searching the web. Just like the bot would caveat any answer—I'm not an expert. Any flaws in the setup are not intentional but may well be there.

