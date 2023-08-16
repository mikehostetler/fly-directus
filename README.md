# fly-directus

This repo contains a template to run [Directus](https://directus.io/) on [Fly.io](https://fly.io/) using the Apps V2 platform.  The Directus machine scales to zero and wakes up upon access.

## Setup

You will need an available Postgres database to store your config.  You can set one up using the [Fly Postgres](https://fly.io/docs/postgres/).

*Fly Postgres Speedrun*
```
fly pg create
```

## Deploy

To host minio on Fly.io clone this repo, enter the folder and run:

```
fly launch \
  --copy-config \
  --no-public-ips \
  --no-deploy \
  --ha=false

fly pg attach -a <YOUR_APP> <YOUR_PG_APP>

fly secrets set \
  KEY=$(openssl rand -base64 32) \
  SECRET=$(openssl rand -base64 32) \
  ADMIN_EMAIL=<YOUR_ADMIN_EMAIL> \
  ADMIN_PASSWORD=<YOUR_ADMIN_PW> \
  DB_CLIENT=pg \
  DB_CONNECTION_STRING=<YOUR_CONNECTION_STRING> \
  WEBSOCKETS_ENABLED=true

fly deploy --ha=false
```

You will be asked an app name and organization. A 1GB volume will be created on deploy.

## Feedback?

https://community.fly.io/ ...TODO

## Wishlist

I'd love to scale this out to a High Availability solution using clustering across multiple regions on Fly. If you have any ideas on how to do this, please let me know! PR's welcome!