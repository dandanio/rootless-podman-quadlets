# [Uptime Kuma](https://github.com/louislam/uptime-kuma)

Fancy self‑hosted monitoring tool.

## Access
`http://<your-ip>:3001`

## Data
All monitor configurations and statuses are stored in `~/uptime-kuma/data/`.  
Back up this directory before major updates.

## Hardening
Uncomment the `NoNewPrivileges`, `ReadOnly`, `DropCapability=all`, and all three tmpfs mounts together.  
The container works well with a read‑only root filesystem.
