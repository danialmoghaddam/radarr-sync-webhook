# Radarr Sync Webhook

Radarr Sync Webhook adds downloaded movies from a Radarr instance to another Radarr instance automatically..

## Requirements

- Two Radarr instances
- Node.js / Docker

## Usage

### Webhook Setup

On your main Radarr instance, create a new webhook:

1. Run "On Download" and "On Upgrade"
1. URL should point to `/import` and specify the following query parameters:
    1. `resolutions`: A comma-separated whitelist of resolutions to sync. 
    Current valid resolutions: `r2160P`, `r1080P`, `r720P`, `r480P`, `unknown`  
    1. `profile`: Quality profile id to use. Get a list of profile ids from the `/api/profile` endpoint on the secondary instance.
    1. Example URL: `http://localhost:3000/import?resolutions=r2160P,r1080P&profile=1`. 
1. Method: `POST`

### Manual methods

In addition to the `/import` webhook, you can also trigger syncs manually. The manual methods use the same URL parameters as the webhook.

#### `/import/:id`

Imports movie `id`. You can get a list of movie ids using the [API](https://github.com/Radarr/Radarr/wiki/API:Movie#get).

Example: `curl -XPOST http://localhost:3000/import/1?resolutions=r2160P&profile=1`

#### `/import/all` 

Imports all movies.

Example: `curl -XPOST http://localhost:3000/import/all?resolutions=r2160P&profile=1`

## Installation

### Node.js

Install node modules: `npm install`

### Docker

Create Docker image:
```
docker create \
--name=radarr-sync \
-p 3000:3000 \
-e SRC_APIKEY=apikey \
-e DST_APIKEY=apikey \
-e SRC_ROOT="/my/UHD/Movies" \
-e DST_ROOT="/my/HD/Movies" \
-e SRC_HOST=http://localhost:7878 \
-e DST_HOST=http://localhost:9090 \
--restart unless-stopped \
radarr-sync:latest
```

## Running

### Node.js

```
PORT=3000 \
SRC_APIKEY=apikey \
DST_APIKEY=apikey \
SRC_ROOT="/my/UHD/Movies" \
DST_ROOT="/my/HD/Movies" \
SRC_HOST=http://localhost:7878 \
DST_HOST=http://localhost:9090 \
npm start
```

### Docker

```
docker start radarr-sync
```
