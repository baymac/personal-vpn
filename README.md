# Quick VPN with Wireguard on your VPS

![AI Generated](https://img.shields.io/badge/🤖_AI-Generated-orange)

One-command WireGuard VPN setup with instant QR code for mobile and config for desktop

## Prerequisites

- A VPS or server with a public IP address
- Ubuntu 20.04+ or Debian 11+ (this guide uses Ubuntu)
- Root or sudo access
- Basic command line knowledge

## Quick Setup

First time setup doesn't necessarily require any config changes. Feel free to edit `wireguard_init.conf` before running.

1. Start Wireguard VPN on the VPS

   ### Option A: Deploy from Local Machine
   
   ```bash
   # Clone the repository
   git clone https://github.com/baymac/quick-vpn.git
   cd quick-vpn
   
   # Upload scripts to your VPS (modify SSH key path and server IP)
   scp -i ~/.ssh/id_ecdsa wireguard* root@YOUR_SERVER_IP:/root/
   
   # Connect to VPS and run setup
   ssh root@YOUR_SERVER_IP
   chmod +x wireguard_init
   ./wireguard_init
   ```

   ### Option B: Deploy Directly on VPS
   
   ```bash
   # Clone and run on the VPS directly
   git clone https://github.com/baymac/quick-vpn.git
   cd quick-vpn
   chmod +x wireguard_init
   ./wireguard_init
   ```
   
   > **Note:** These commands use root user by default. You can use a different user with sudo privileges.

2. Connect your device to the VPN

   ### Option A: Mobile Setup (QR Code)

   1. Install WireGuard app on your phone
   2. Tap "+" → "Scan from QR code"
   3. Scan the displayed QR code
   4. Enable tunnel

   ### Option B: Desktop Setup (Config File)

   1. Install WireGuard app on your desktop
   2. Open WireGuard → "Manage Tunnels"
   3. Click "Add" button → "Add empty tunnel"
   4. Paste the config text from terminal
   5. Give it a name (e.g. "us", "eu")
   6. Click "Save" → Enable the tunnel

   Done! All traffic routes through your VPS.

## Adding More Clients

To add additional clients to your existing WireGuard setup:

1. Edit `wireguard_client_add.conf` with client settings

2. Add client:
   ```bash
   # Edit the configuration file with client settings
   chmod +x wireguard_client_add
   ./wireguard_client_add
   ```

> **Note:** Do not connect same client to multiple devices, it will break the connection.

## List and Get Existing Clients

```bash
chmod +x wireguard_client

# List all active clients (reads from server config first)
./wireguard_client list

# Get QR code, config
./wireguard_client get <client_name> # Replace <client_name> with the name of the client
```

## Teardown

To completely remove WireGuard setup:

```bash
chmod +x wireguard_teardown
./wireguard_teardown
```

This will remove:
- WireGuard
- All client keys and certificates  
- Firewall rules
- Network interface


## Management

```bash
# Check status
sudo wg show

# Restart service  
sudo systemctl restart wg-quick@wg0

# View logs
sudo journalctl -u wg-quick@wg0
```

