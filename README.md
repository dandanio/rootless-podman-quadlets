Podman Rootless Containers
==========================

A collection of Podman my rootless container stacks, tested and running on **Ubuntu 26.04** with **Podman 5.7.0**, with optional security additions that do not break the container.  
All services use Quadlet files (`~/.config/containers/systemd/`) so they integrate natively with systemd and start automatically under your user.

* * *

Why this repo?
--------------

*   **Rootless by default** – every container runs without any root privileges, even during setup.
*   **Arguably safer than Docker** – Podman’s daemonless architecture and user‑namespace isolation reduce the attack surface. No privileged `dockerd` socket, no “root equivalent” container escapes.
*   **Quadlet for systemd** – no need for `podman-compose`; just drop a `.container` file and enable with `systemctl --user`.
*   Podman is gaining traction, and rootless mode is finally production‑ready for most workloads.

* * *

Repository structure
--------------------

Each directory below contains a **Quadlet `.container` file** and optional notes.

*   Hardening options (like `no-new-privileges`, `read-only` rootfs, `security-opt`) are **commented out** by default – uncomment what you need.
*   Temporary drives (`tmpfs`, `:Z` volume flags) are also commented out for clarity but should be strongly considered with containers with frequent HDD/SSD read/writes.
*   All volumes map into `~/containers/<service>/` by default – no system directories touched.
   
* * *

Quick start (for any container in this repo)
--------------------------------------------

### 1\. Install Podman (Ubuntu 26.04)

    sudo apt update
    sudo apt install podman

No Docker or `docker-compose` needed. Podman includes all the tools.

### 2\. Enable linger for your user _(critical for autostart at boot)_

This allows your user’s systemd session to run even when you are not logged in, so containers start automatically at system boot.

    sudo loginctl enable-linger $USER

Verify with `loginctl show-user $USER | grep Linger` – should show `Linger=yes`.

### 3\. Set up the Quadlet directory

    mkdir -p ~/.config/containers/systemd

_If the directory already exists, that’s fine._

### 4\. Grab a container definition

Navigate to the service directory you want, then copy the `.container` file:

    cp ~/this-repo/adguard/adguard.container ~/.config/containers/systemd/

**Edit the file with `nano` (without `sudo` – it’s your user dir)**:

    nano ~/.config/containers/systemd/adguard.container

Adjust volumes, environment variables, or uncomment hardening options as needed.

### 5\. Pull the image and generate the systemd unit

Quadlet automatically creates a systemd service from the `.container` file:

    systemctl --user daemon-reload

Then pull the image (optional but gives you immediate feedback):

    podman pull <image-name>   
    # or let systemd pull it on start

### 6\. Enable and start the container

    systemctl --user start vert

Check status:

    systemctl --user status vert
    podman ps

The service will survive reboots and log to the journal (`journalctl --user -u vert` or `podman logs vert`).

* * *

Saving / backing up your containers
-----------------------------------

Your persistent data lives in the volumes you defined (e.g., `~/containers/adguard/`).  
To back up a container’s state:

    podman stop vert
    tar -czf vert ~/containers/adguard/

The Quadlet file itself is your “infrastructure as code” – keep it safe in this git repo.

* * *

Common troubleshooting
----------------------

*   **`systemctl --user` commands fail with `Failed to connect to bus`**  
    Make sure your user session is running `systemd --user`. Usually it’s automatic on login, but if not: `systemctl --user start default.target`.
*   **Linger is not enabled / containers don’t start at boot**  
    Run `sudo loginctl enable-linger $USER` to allow the user manager to run at boot without a login. This is mandatory for unattended servers.
*   **Ports below 1024**  
    Rootless Podman can’t bind to privileged ports by default. Add `net.ipv4.ip_unprivileged_port_start=80` to `/etc/sysctl.d/99-rootless.conf` and reboot, or use a high port with a reverse proxy.
*   **`WARN[0000] Failed to load cached network config`**  
    Harmless; happens when networks are recreated. Ignore unless connectivity breaks.

* * *

Useful resources
----------------

*   [Podman rootless tutorial](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md)
*   [Quadlet (systemd generator) guide](https://www.redhat.com/sysadmin/quadlet-podman)
*   [Awesome Podman](https://github.com/containers/awesome-podman)

* * *

_These configurations were tested on Ubuntu 26.04 with Podman 5.7.0 (apt installed) on an Intel NUC 13i7 with 64 gb memory and a 2 tb NVME SSD. They should work on any recent systemd distro with a Podman version ≥ 4.4, but your mileage may vary._
