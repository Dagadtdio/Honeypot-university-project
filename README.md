## Documentation language

The main project documentation PDF is written in Hungarian because it was prepared as part of a Hungarian university course submission. The repository README and supporting project files are written in English to make the technical setup easier to understand for a wider audience and to support the improvement of my English writing skills.

# Cowrie SSH Honeypot Lab

This is a Proxmox-based SSH honeypot lab using **Cowrie proxy mode**, isolated Linux bridge networking, UFW firewall rules, `authbind` for port 22, and Python-based JSON log analysis exported to Excel.

> This repository contains a sanitized university project lab. It does **not** include real credentials, private keys, or full raw honeypot logs.

## Project overview

The goal of the project was to build a controlled SSH honeypot environment where SSH login attempts and post-login commands could be simulated, logged, exported, and analyzed.

The lab used three virtual machines:

| VM | Role | IP |
|---|---|---|
| `test-client` | Attacker simulation and analysis machine | `10.50.0.20` |
| `cowrie-proxy` | Cowrie SSH honeypot in proxy mode | `10.50.0.10` |
| `backend-ubuntu` | Restricted backend machine behind the proxy | `10.50.0.30` |

## Architecture

```text
Test-client (10.50.0.20)
        |
        | SSH 22
        v
Cowrie proxy (10.50.0.10)
        |
        | SSH 22
        v
Backend VM (10.50.0.30)
```

Direct access from the test-client to the backend is blocked. The backend only accepts SSH from the Cowrie proxy VM.

## Why proxy mode?

Cowrie can be used with a shell-emulated environment or in proxy mode.

In shell emulation, Cowrie shows a simulated shell to the attacker. This is easier to set up, but less realistic.

In proxy mode, Cowrie acts as a middle layer between the attacker and a real backend Linux VM. The attacker sees a more believable environment, while Cowrie logs authentication attempts, sessions, and commands.

## Key security decisions

- The lab was isolated on an internal Proxmox Linux bridge.
- There was no public IP and no port forwarding.
- The backend SSH service was reachable only from the Cowrie VM.
- Cowrie was run as the dedicated `cowrie` user, not as root.
- `authbind` was used so Cowrie could listen on port 22 without running as root.
- Fake honeypot credentials were stored in `userdb`.
- Real backend credentials were stored separately in Cowrie proxy config and are not included here.

## Log processing

The script reads Cowrie's JSON Lines log file (`cowrie.json`) and creates an Excel workbook with separate sheets for:

- summary statistics
- login attempts
- commands
- connection/session events
- all raw events

## Disclaimer

This project is for educational and lab use only. It should not be deployed on a public IP without additional hardening and monitoring.
