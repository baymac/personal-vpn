# WireGuard VPN with QR Code Setup

One-command WireGuard VPN setup with instant QR code for mobile devices.

## Quick Setup

Note: we are defaulting to root user, you can use a different user.

1. Start Wireguard VPN on the VPS

   There are two ways to start the VPN:

   ### i. Start from your Laptop:
      ```bash
      git clone https://github.com/baymac/wireguard-qr.git
      cd wireguard-qr
      scp -i ~/.ssh/id_ecdsa wireguard_setup* root@YOUR_SERVER_IP:/root/
      ssh root@YOUR_SERVER_IP
      chmod +x wireguard_setup
      ./wireguard_setup
      ```

   OR

   ### ii. Start from your VPS:
      ```bash
      git clone https://github.com/baymac/wireguard-qr.git
      cd wireguard-qr
      chmod +x wireguard_setup
      ./wireguard_setup
      ```

2. **Scan QR code** with WireGuard app on your phone

## What it does

- ✅ Installs WireGuard server
- ✅ Generates keys automatically  
- ✅ Configures firewall
- ✅ Displays QR code in terminal
- ✅ Saves client config files

## Configuration

First time setup doesn't require any config changes. Feel free to do so.

Edit `wireguard_setup.conf` before running:

```bash
CLIENT_NAME="phone" # unique name for new client
CLIENT_VPN_IP="10.0.0.2" # increment by 1 per new client
```

## Mobile Setup

1. Install WireGuard app
2. Tap "+" → "Scan from QR code"
3. Scan the displayed QR code
4. Enable tunnel

Done! All traffic routes through your VPS.

## Management

```bash
# Check status
sudo wg show

# Restart service  
sudo systemctl restart wg-quick@wg0

# View logs
sudo journalctl -u wg-quick@wg0
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
