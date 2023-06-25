# Docker Ghost Starter

this is self-hosted docker starter template for ghost

- SSL Certification issue & automatic renew (Certbot, using Cloudflare dns challenge)
- Nginx Reverse proxy
- Ghost & MariaDB

# Directory Structures

- config
  - every configuration data
- data
  - every persistant data will be stored in here
- docker-compose.yml
  - nginx, ghost, database compose file
- certbot.docker-compose.yml
  - certbot test/issue/renew compose file
- .env.example
  - environment that used everywhere

# Pre-Requirement

1. Clone this repository in some place
```bash
git clone https://github.com/winetree94/docker-ghost-starter.git
```

2. Clone .env.example to .env and fill your infos
```bash
cp .env.example .env && nano .env
```

4. Replace cloudflare api token
```
nano config/cloudflare/credentials
```

# Installation

## 1. SSL Certification working test

I recommend that you test it before you start.

this command will test your dns challenging process

```bash
docker compose -f certbot.docker-compose.yml run --rm certbot-test
```

If the result is like this, it's ready to go

```text
Simulating renewal of an existing certificate for winetree94.com
Waiting 10 seconds for DNS changes to propagate
The dry run was successful.
```

## 2. Issue SSL Certification

```bash
docker compose up -f certbot.docker-compose.yml run --rm certbot-issue
```

## 3. Start ghost

```bash
docker compose up -d
```

## 4. Register crontab for automatic renewal ssl certification

```bash
crontab -e
```

```bash
* * 5 * * /REPOSITORY_PATH/config/crontab/renew.sh >> /PATH_TO_STORE_LOG/crontab.log
```
