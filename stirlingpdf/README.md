# [Stirling‑PDF](https://github.com/Stirling-Tools/stirling-pdf)

Powerful PDF manipulation tool with a web UI.

## Access
`http://<your-ip>:8180`

## Login
`SECURITY_ENABLELOGIN=true` requires a first‑run account creation.  
If you want open access, set it to `false`.

## Memory
Stirling‑PDF can be memory hungry. The `JAVA_TOOL_OPTIONS` limit the JVM to 4 GB max.  
A commented `--memory=8G` is also available if you need to cap the whole container.

## Volumes
All persistent data is kept under `~/stirlingpdf/`. Custom OCR training data, configs, and pipelines can be placed there.

## Hardening
Uncomment the entire hardening block (capabilities + tmpfs) to run a locked‑down container.
