[Paperless NGX](https://github.com/paperless-ngx/paperless-ngx)
=============

Self‑hosted document manager with Redis sharing the webserver’s network namespace.

Components
----------

*   **Paperless Webserver** – web UI, API, document consumer, and task workers.
*   **Redis** – message broker and task queue (ephemeral, no persistence).

Access
------

*   Web UI: `http://<your‑server>:8000`

How networking works
--------------------

Redis uses `Network=container:paperless-webserver`. It appears to the webserver at `localhost:6379`. No separate port is published for Redis.

Setup
-----

1.  **Adjust the hostname**:  
    In `paperless-webserver.container`, replace `YOUR_HOSTNAME` with your server’s IP or domain (e.g. `192.168.1.10` or `paperless.yourdomain.com`).
2.  **Start the webserver** (Redis will start automatically):
    
        systemctl --user enable --now paperless-webserver.service
    
3.  Open `http://your-ip:8000` and complete the first‑run setup.

Document consumption
--------------------

Place files in the `~/paperless/consume` directory and they will be processed automatically. You can also use the web UI to upload documents.

Hardening
---------

All hardening directives are commented out. Uncomment the blocks labeled “Hardening” and the corresponding tmpfs mounts to lock down the containers. The tmpfs mounts are required if you set `ReadOnly=true`.

Resource limits
---------------

The webserver’s memory and CPU limits are commented out. Uncomment and adjust them according to your system’s capacity.

Volumes
-------

*   **Data:** `~/paperless/data`
*   **Media:** `~/paperless/media`
*   **Export:** `~/paperless/export`
*   **Consume:** `~/paperless/consume`

Important notes
---------------

*   The `SecurityLabelDisable=true` option is commented out by default. Uncomment it only if you experience permission problems with SELinux.
*   Redis is configured with `Exec=valkey-server --save "" --appendonly no` to disable persistence. Data is held in memory only – if the container restarts, queued tasks will be lost but will be re‑created by the webserver.
