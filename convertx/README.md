# ConvertX

File conversion service (images, documents, etc.) accessible via web UI.

## Access
Open `http://<your-ip>:3002` after starting.

## Volumes
Data is stored in `~/convertx/data` by default. Change the left‑side path in the
`.container` file if you want a different location.

## Secrets / API keys
If you need to pass secrets, create a file `~/.config/containers/systemd/convertx.env`
and uncomment the `EnvironmentFile` line in the Quadlet file.

## Hardening
The Quadlet file includes commented‑out hardening options. ConvertX can run with
`ReadOnly=true` as long as the tmpfs mounts are enabled alongside it.
