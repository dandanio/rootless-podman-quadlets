# Ente Auth (Self‑hosted)

A self‑hosted Ente Auth stack consisting of:
- **PostgreSQL** database
- **Museum** – the core Ente server (API)
- **Web** – the frontend serving Auth (port 3003) and Accounts (port 3001)

All containers run rootless and share an internal `ente.network`.

## Access
- Museum API: `http://<your‑ip>:8085`
- Web Auth UI: `http://<your‑ip>:3403`
- Web Accounts UI: `http://<your‑ip>:3401`

In production, you should put these behind a reverse proxy with SSL.

## Prerequisites
1. Create the shared network:  
   `cp network/ente.network ~/.config/containers/systemd/`  
   `systemctl --user daemon-reload`  
   `systemctl --user start ente-network.service`
2. Prepare the `museum.yaml` configuration file at `~/my-ente/museum.yaml`.  
   Refer to the [Ente server documentation](https://github.com/ente-io/ente) for required settings (S3/MinIO, JWT secrets, etc.).
3. Copy the `.env.example` files to `.env` and set your database credentials:
   ```bash
   cp ente-database.env.example ente-database.env
   cp ente-museum.env.example ente-museum.env
   # Edit them with your actual usernames and passwords
