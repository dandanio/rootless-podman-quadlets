[MinusPod](https://github.com/ttlequals0/minuspod) – Podcast Ad Remover
=============================

Self‑hosted, rootless deployment of MinusPod using Podman Quadlets. The engine can use **OpenRouter** (free tier) for ad‑removal or an optional local **OpenVINO Whisper** transcriber.

Components
----------

*   **MinusPod CPU Engine** – web UI, API, and ad‑removal logic. This is for non-Nvidia image.
*   **OpenVINO Model Server** (optional) – local Whisper transcription for completely offline processing. Shares the engine’s network namespace.

Access
------

*   MinusPod web UI: `http://<your‑ip>:9998`

Prerequisites
-------------

1.  **Copy and edit the environment file**:
    
        cp minuspod.env.example minuspod.env
        nano minuspod.env
    
    Fill in your master passphrase, OpenRouter API key, and base URL.
2.  **(Optional) Prepare the OpenVINO model**:
    
    If you plan to use the local transcriber, create the directory `~/openvino/models` and add a valid `config.json` plus the required Whisper models. Refer to the [OpenVINO Model Server documentation](https://github.com/openvinotoolkit/model_server) for details.
    

Setup
-----

1.  Start the engine:
    
        systemctl --user enable --now minuspod.service
    
2.  (Optional) Start the transcriber:
    
        systemctl --user enable --now minuspod-transcriber.service
    
    _The transcriber will start automatically a few seconds after the engine._

How the transcriber works
-------------------------

The OpenVINO model server uses `Network=container:minuspod`, so it appears to the engine as `localhost:8001`. You do not need to publish any extra ports.

Hardening
---------

All hardening directives are commented out. Uncomment the blocks labeled “Hardening” and the tmpfs mounts to lock down each container.

Resource limits
---------------

The transcriber’s resource limits are commented out. Adjust `CPUQuota` and `MemoryMax` according to your hardware.

Volumes
-------

*   **MinusPod data:** `~/minuspod/data`
*   **OpenVINO models:** `~/openvino/models` (optional)
