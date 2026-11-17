
# ClawBox 硬件盒子- OpenClaw Hardware Dev

> 来源：[https://gotoaiworld.feishu.cn/docx/GzgIdGdDzoAqBixb8U8c6JC2nwg](https://gotoaiworld.feishu.cn/docx/GzgIdGdDzoAqBixb8U8c6JC2nwg)

## ClawBox

`https://openclawhardware.dev/mac-mini-alternative`

![](img/18b87838a450e5223210c54fb99ab33b.png)

![](img/ea2d2711acabc280f3321ebed1e669ca.png)

### GitHub Source Code

ClawBox - AI Hardware Setup and Configuration Code

`https://github.com/ID-Robots/clawbox`

Setup wizard and dashboard for NVIDIA Jetson devices. Provides a browser-based interface for configuring OpenClaw — connect to the device's WiFi hotspot, open `http://clawbox.local/`, and walk through the guided setup.

### Quick Start

```
sudo bash install.sh
```

Connect to the `ClawBox-Setup` WiFi network (open, no password) and navigate to:

- `http://clawbox.local/`
- `http://10.42.0.1/`

### Setup Wizard

The wizard guides you through 7 steps:

1. Welcome — Introduction
1. Security — Set device password
1. WiFi — Connect to your home/office network
1. Update — System packages, JetPack, OpenClaw
1. AI Models — API key or Claude subscription (OAuth)
1. Telegram — Optional bot integration
1. Done — System dashboard with status and factory reset

After setup completes, the root URL serves the OpenClaw gateway Control UI with a ClawBox navigation bar.

### Architecture

```
Browser (http://clawbox.local)
  │
  ├── Port 80: Next.js (production-server.js)
  │     ├── /setup          → Setup wizard (React SPA)
  │     ├── /setup-api/*    → Setup API routes
  │     ├── /api/*          → Proxy to gateway
  │     ├── /               → Gateway HTML + ClawBox bar
  │     └── WebSocket       → Proxy to gateway (upgrade handler)
  │
  └── Port 18789: OpenClaw Gateway (localhost only)
        ├── Control UI
        ├── WebSocket (real-time)
        └── REST API
```

Node.js is used instead of Bun for the production server because Bun doesn't fire `http.Server` upgrade events, which are required for WebSocket proxying.

### Tech Stack

- Next.js 16 / React 19 / TypeScript 5
- Tailwind CSS 4
- Bun — package management and builds
- Node.js 22 — production runtime
- NetworkManager — WiFi AP and client connections
- Avahi — mDNS (`clawbox.local`)

### Project Structure

```
├── config/                 Systemd services, dnsmasq config
├── scripts/                WiFi AP start/stop scripts
├── src/
│   ├── app/                Next.js App Router (pages + API routes)
│   ├── components/         React components (setup steps)
│   ├── lib/                Utilities (config store, network, updater, gateway proxy)
│   └── middleware.ts       Captive portal detection
├── production-server.js    Node.js wrapper with WebSocket proxy
└── install.sh              Full installer script
```

### Installation Details

The installer (`install.sh`) performs 13 steps:

1. Verify `clawbox` system user exists
1. Install system packages (`git`, `curl`, `NetworkManager`, `avahi`, `iptables`)
1. Install Node.js 22 via NodeSource
1. Set hostname to `clawbox`, configure mDNS
1. Clone or update the repository
1. Install Bun runtime
1. Build the Next.js app (`bun install && bun run build`)
1. Install OpenClaw via `npm`
1. Patch OpenClaw gateway (insecure auth + scope preservation)
1. Deploy captive portal DNS config
1. Set up directories and permissions
1. Install and enable systemd services
1. Start services

The script is idempotent — safe to run multiple times.

### Services

```
sudo systemctl status clawbox-ap
sudo systemctl status clawbox-setup
sudo systemctl restart clawbox-setup
```

### Development

```
bun install
bun run dev           # Port 3000
bun run dev:privileged # Port 80 (requires root)
bun run build
bun run lint
```

### Environment Variables
