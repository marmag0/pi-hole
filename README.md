# Pi-hole with Cloudflare

This repository contains a Pi-hole deployment using `cloudflared` (Cloudflare Tunnel) for easy access to the Pi-hole dashboard.

## Safe Deployment: Step-by-Step

1. Make sure Docker is installed.
2. Create a Cloudflare Tunnel in Cloudflare Zero Trust and obtain the tunnel token (it will be used in `.env` shortly). The tunnel should only expose port `80`/`HTTP`; use it only for the Pi-hole web UI.
3. Create a Cloudflare Application with an access control policy for the domain specified during tunnel configuration. This limits access to only the users you specify.
4. Set up environment variables:

- `API_PASSWORD` - password for accessing Pi-hole configuration; make it hard to guess
- `LOCAL_IP` - the IP address of the device on which Pi-hole is hosted (used to avoid conflicts with Cloudflare Tunnel's DNS setting)
- `CLOUDFLARE_TOKEN` - token used to connect the tunnel to the app

```bash
API_PASSWORD=superHardPasswd
LOCAL_IP=yourPiHoleIpInHomeNetwork
CLOUDFLARE_TOKEN=yourSecretToken
```

5. Run the app (add `-d` to run in detached mode):

```bash
docker compose up
```

## Deployment without Cloudflare

1. Make sure Docker is installed.
2. Comment out the entire `cloudflared` service in `docker-compose.yml` (add `#` before each line).
3. Set up environment variables:

- `API_PASSWORD` - password for accessing Pi-hole configuration; make it hard to guess
- `LOCAL_IP` - the IP address of the device on which Pi-hole is hosted, but if deployed without Cloudflare integration it can be left blank

```bash
API_PASSWORD=superHardPasswd
LOCAL_IP=yourPiHoleIpInHomeNetwork
```

4. Run the app (add `-d` to run in detached mode):

```bash
docker compose up
```

## Stopping and Deleting the App

- Stop the application while preserving data stored in volumes. After another `docker compose up`, it will resume in the same state as before you ran `docker compose down`.

```bash
docker compose down
```

- Stop the application and remove all saved data volumes on the server:

```bash
docker compose down -v
```
