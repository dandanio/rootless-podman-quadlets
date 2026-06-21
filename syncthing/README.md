# [Syncthing](https://github.com/syncthing/syncthing)

Continuous file synchronization between devices.

## Access
Web UI: `http://<your-ip>:8384`

## Volumes
- `~/syncthing/config` – Syncthing database and settings.
- `~` is mounted as `/home/rickined` inside the container.  
- External USB backup: `/mnt/sda1/backupfrom40/` is mounted directly.

Adjust the volume mappings if your home path differs.

## Firewall
Ensure ports `22000/tcp+udp` and `21027/udp` are open for device discovery and transfers.
