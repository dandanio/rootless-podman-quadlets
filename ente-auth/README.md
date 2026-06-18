Ente Auth (Self‑hosted)
=======================

A self‑hosted Ente Auth stack consisting of:

*   **PostgreSQL** database
*   **Museum** – the core Ente server (API)
*   **Web** – the frontend serving Auth (port 3003) and Accounts (port 3001)

All containers run rootless and share an internal `ente.network`.

Access
------

*   Museum API: `http://<your‑ip>:8085`
*   Web Auth UI: `http://<your‑ip>:3403`
*   Web Accounts UI: `http://<your‑ip>:3401`

In production, you should put these behind a reverse proxy with SSL.

Prerequisites
-------------

1.  Create the shared network:
    
        cp network/ente.network ~/.config/containers/systemd/
        systemctl --user daemon-reload
        systemctl --user start ente-network.service
    
2.  Prepare the `museum.yaml` configuration file at `~/my-ente/museum.yaml`.  
    Refer to the [Ente server documentation](https://github.com/ente-io/ente) for required settings (S3/MinIO, JWT secrets, etc.).
3.  Copy the `.env.example` files to `.env` and set your database credentials:
    
        cp ente-database.env.example ente-database.env
        cp ente-museum.env.example ente-museum.env
        # Edit them with your actual usernames and passwords
    

Setup
-----

1.  **Start the database:**  
    `systemctl --user enable --now ente-database.service`
2.  **Start the museum server:**  
    `systemctl --user enable --now ente-museum.service`
3.  **Start the web frontend:**  
    `systemctl --user enable --now ente-web.service`

Configuration details
---------------------

*   The web container’s `ENTE_API_ORIGIN` must point to your museum server’s public address.
*   `NEXT_PUBLIC_ENTE_ENDPOINT` is the URL your clients will use (must be HTTPS).
*   The web container exposes ports 3401 and 3403. If you run multiple instances, adjust the host ports.

Hardening
---------

All security directives are commented out. Uncomment the blocks labeled “Hardening” and the corresponding tmpfs mounts to lock down the containers.  
Resource limits (memory) are also commented out; uncomment and adjust to your system.

Volumes
-------

*   Database data: `~/my-ente/db-data`
*   Museum data & logs: `~/my-ente/data`, `~/my-ente/ente_logs`
*   Museum configuration: `~/my-ente/museum.yaml` (mounted read‑only)
