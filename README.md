<div align="center">

```text
   ██████╗   ██████╗  ██╗      ███████╗ ███╗   ███╗
  ██╔════╝  ██╔═══██╗ ██║      ██╔════╝ ████╗ ████║
  ██║  ███╗ ██║   ██║ ██║      █████╗   ██╔████╔██║
  ██║   ██║ ██║   ██║ ██║      ██╔══╝   ██║╚██╔╝██║
  ╚██████╔╝ ╚██████╔╝ ███████╗ ███████╗ ██║ ╚═╝ ██║
   ╚═════╝   ╚═════╝  ╚══════╝ ╚══════╝ ╚═╝     ╚═╝
```

# 🗿 GOLEM

**A blazing-fast, lightweight CLI tool written in Rust to spin up, manage, and monitor Dockerized Minecraft servers in seconds.**

[![Rust](https://img.shields.io/badge/rust-%23000000.svg?style=for-the-badge&logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

*Zero complex server setup. One command to launch. Automatic LAN IP discovery. Fully persistent.*

</div>

---

## ⚡ Highlights

- 🚀 **Instant Setup**: Spawns production-grade Minecraft servers (PaperMC, Fabric, Forge, Vanilla) powered by [`itzg/minecraft-server`](https://github.com/itzg/docker-minecraft-server).
- 🌐 **Smart Network UX**: Instantly calculates and displays your **Local connection** (`localhost:<port>`) and your **LAN Wi-Fi IP** (`<lan_ip>:<port>`) so friends on your network can join right away.
- 🛡️ **Pre-Flight Safety Checks**: Proactively inspects Docker daemon readiness, detects name collisions, and validates port availability before running.
- 📊 **Simple Stats View (`golem stats` / `golem dash`)**: Fast, non-interactive stats report showing live CPU, memory consumption, connection endpoints, and recent logs.
- 💾 **Persistent Data Storage**: Automatically provisions isolated Docker named volumes (`golem_<name>_data`) to safeguard your worlds, player inventories, and server configs across restarts.
- 🎮 **Offline & Launcher Friendly**: Configured by default with `ONLINE_MODE=FALSE` so custom launchers (like TLauncher) and friends can join without auth hurdles.

---

## 📥 Installation

Ensure you have [Rust](https://www.rust-lang.org/tools/install) and [Docker](https://docs.docker.com/get-docker/) installed and running.

```bash
# Clone the repository
git clone https://github.com/parameters-tools/golem.git
cd golem

# Build and install to ~/.cargo/bin
cargo install --path .
```

`golem` is now ready to use anywhere in your terminal!

---

## 🚀 Quick Start

### 1. Create a server on a custom port
```bash
golem create survival --port 8000
```

```text
🗿 Golem - Minecraft Server Launcher

• Checking Docker status... OK
• Checking container name availability... OK
• Checking port 8000 availability... OK

▶ Spinning up Minecraft server 'survival'...

✔ Server container created successfully!
──────────────────────────────────────────────────────
  Server Name:       survival
  Container ID:      8418091402f4
  Server Engine:     PAPER
  Allocated RAM:     2G
  Persistent Data:   golem_survival_data
  Online Mode:       False (Offline/Custom)
──────────────────────────────────────────────────────

🎮 How to connect in Minecraft (Direct Connection / Add Server):
  • Local (this PC):     localhost:8000
  • LAN (same Wi-Fi):    192.168.1.45:8000

📋 Server management:
  • View stats:          golem stats survival
  • Stop server:         golem stop survival
  • Run/Start server:    golem run survival
  • Follow raw logs:     docker logs -f survival
```

### 2. List All Servers (`golem ls`)
List all your Minecraft servers with statuses, ports, engine types, and addresses:
```bash
golem ls
# or alias: golem list
```

```text
🗿 Golem Minecraft Servers
Found 2 server(s):

  NAME               STATUS          PORT      ENGINE    RAM     LOCAL ADDRESS
  ─────────────────────────────────────────────────────────────────────────
  survival           🟢 Running       8000      PAPER     2G      localhost:8000
  lobby              🔴 Stopped       25565     PAPER     4G      localhost
  ─────────────────────────────────────────────────────────────────────────
```

### 3. View Server Stats & Health
Check container status, resource usage, addresses, and live server logs:
```bash
golem stats survival
# or alias: golem dash survival
```

```text
🗿 Golem Server Status: survival
──────────────────────────────────────────────────────
  • Status:          🟢 RUNNING (running)
  • Host Port:       8000
  • Local Connect:   localhost:8000
  • LAN Connect:     192.168.1.45:8000
  • Storage Volume:  golem_survival_data
  • CPU Usage:       1.85%
  • Memory Usage:    2.31GiB / 16.00GiB
──────────────────────────────────────────────────────

📜 Recent Logs (last few lines):
  [04:26:06 INFO]: Done (6.848s)! For help, type "help"
  [04:26:11 INFO]: UUID of player Kevin is a26cc14e-b3ab-3612-b513-f9cf0a976ae0
  [04:26:12 INFO]: System chat: Kevin joined the game
  [04:26:12 INFO]: Kevin logged in with entity id 66 at ([world]61.07, 66.0, 51.30)
```

### 4. Stop Server
Gracefully stop the server container:
```bash
golem stop survival
```

### 5. Resume / Run Server
Start up an existing stopped server anytime:
```bash
golem run survival
# or alias: golem start survival
```

### 6. Remove a Server (`golem rm`)
Remove a container safely with interactive confirmation:
```bash
# Deletes container but keeps world data volume safely
golem rm survival

# Delete container AND permanently purge world data volume
golem rm survival --purge

# Skip confirmation prompt (-f or -y)
golem rm survival --purge -y
```

### 7. Stream Server Logs (`golem logs`)
View recent lines or follow the console in real time:
```bash
# View last 50 lines with color syntax highlighting
golem logs survival

# View last 100 lines
golem logs survival -n 100

# Stream / follow logs live (press Ctrl+C to detach)
golem logs survival -f
```

### 8. World Backups & Restore (`golem backup` / `golem restore`)
Snapshot and protect your worlds:
```bash
# Create a compressed timestamped snapshot in ~/.golem/backups/
golem backup survival

# List all available saved backups
golem backup --list

# Restore world from a snapshot
golem restore survival ~/.golem/backups/survival_backup_2026-10-24_142601.tar.gz

# Restore without confirmation prompt
golem restore survival ~/.golem/backups/survival_backup_2026-10-24_142601.tar.gz -y
```

---

## 🕹️ Connecting to Your Server

| Connection Type | Who is connecting? | Address to enter in Minecraft |
|-----------------|-------------------|--------------------------------|
| **Local** | You (on the same machine running Golem) | `localhost:8000` *(or `127.0.0.1:8000`)* |
| **LAN / Wi-Fi** | Friends connected to the same home Wi-Fi | `<YOUR_LAN_IP>:8000` *(e.g. `192.168.1.45:8000`)* |
| **Internet** | Friends outside your home network | `<PUBLIC_IP>:8000` *(via router port-forwarding or tunnels like Playit.gg / Ngrok)* |

> **Note**: If running on the standard default port (`25565`), you do not need to specify `:25565` in Minecraft. For any custom port (such as `8000`), the colon and port number must be included.

---

## ⚙️ Advanced `golem create` Options

You can customize the server engine, memory allocation, and authentication mode:

```bash
golem create skyblock \
  --port 25566 \
  --server-type PAPER \
  --memory 4G \
  --max-players 10 \
  --view-distance 12 \
  --online-mode
```

### Flags & Options

| Flag | Short | Default | Description |
|------|:-----:|:-------:|-------------|
| `--port` | `-p` | `25565` | Host port to bind to the container |
| `--server-type` | | `PAPER` | Server type (`PAPER`, `PURPUR`, `FABRIC`, `FORGE`, `SPIGOT`, `VANILLA`) |
| `--memory` | `-m` | `2G` | Memory RAM limit (`2G`, `4G`, `8G`, etc.) |
| `--max-players` | | `20` | Max simultaneous players allowed on server (1-10,000) |
| `--view-distance` | | `10` | Render/view distance in chunks (3-32) |
| `--online-mode` | | `false` | Enable official Mojang authentication (`true` for official accounts only, `false` for offline/custom launchers) |

---

## 🗄️ World Data & Volumes

All worlds, player data, and configurations are stored in an isolated Docker volume named `golem_<server_name>_data`.

- **Backup world**:
  ```bash
  docker cp survival:/data ./survival-backup
  ```
- **Install plugins or mods**:
  ```bash
  docker cp MyPlugin.jar survival:/data/plugins/
  ```
- **Delete server and clean up data**:
  ```bash
  docker rm -f -v survival && docker volume rm golem_survival_data
  ```

---

## 🛠️ Tech Stack & Architecture

- **Language**: [Rust](https://www.rust-lang.org/) (2024 Edition)
- **CLI Framework**: [`clap`](https://crates.io/crates/clap) with derive macros
- **Formatting & Colors**: [`colored`](https://crates.io/crates/colored)
- **Docker Engine**: Invoked directly via `std::process::Command` for zero async runtime overhead, tiny binary size, and instant execution.
- **Network Resolution**: Zero-packet UDP socket kernel route discovery for instantaneous LAN IP detection.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
