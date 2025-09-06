# Quick VPN with Wireguard on your VPS

![AI Generated](https://img.shields.io/badge/🤖_AI-Generated-orange)

One-command WireGuard VPN setup with instant QR code for mobile and config for desktop

## Prerequisites

- A VPS or server with a public IP address
- Ubuntu 20.04+ or Debian 11+ (this guide uses Ubuntu)
- Root or sudo access
- Basic command line knowledge

## Quick Setup

## Configuration

First time setup doesn't require any config changes. Feel free to edit `wireguard_init.conf` before running.

1. Start Wireguard VPN on the VPS

   There are two ways to start the VPN:

   ### i. Start from your Laptop (Modify based on your setup):
      ```bash
      git clone https://github.com/baymac/quick-vpn.git
      cd quick-vpn   
      scp -i ~/.ssh/id_ecdsa wireguard* root@YOUR_SERVER_IP:/root/
      ssh root@YOUR_SERVER_IP
      chmod +x wireguard_init
      ./wireguard_init
      ```

   OR

   ### ii. Start from your VPS:
      ```bash
      git clone https://github.com/baymac/quick-vpn.git
      cd quick-vpn
      # Edit the configuration file with your settings
      sudo ./wireguard_init
      ```
   
   Note: we are defaulting to root user, you can use a different user.

2. **Scan QR code** with WireGuard app on your phone

   ### Mobile Setup

   1. Install WireGuard app
   2. Tap "+" → "Scan from QR code"
   3. Scan the displayed QR code
   4. Enable tunnel

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

Each client gets:
- Unique VPN IP address (auto-assigned)
- Individual QR code and config file
- Separate key pair for security

Do not connect same client to multiple devices, it will break the connection.

## List and Get Existing Clients

### Commands

```bash
chmod +x wireguard_client

# List all active clients (reads from server config first)
./wireguard_client list

# Get QR code, config, and regenerate PNG for a specific client
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

