# Connect to Your Home PC from Anywhere Using DuckDNS and Port Forwarding

This guide will walk you through the steps to set up a secure connection to your home PC from an external network. The setup uses a TOTOLINK router as an example.

## Prerequisites
- A router that supports Port Forwarding (e.g., TOTOLINK).
- A Linux system running on your home PC (e.g., Ubuntu).
- A DuckDNS account to bind your dynamic IP to a fixed domain name.

## Step 1: Configure Port Forwarding on Your Router

First, configure Port Forwarding on your router to allow external access to your home PC.

1. Open your browser and access the router's admin page (usually [192.168.0.1](http://192.168.0.1)).
2. Log in and navigate to the **Port Forwarding** settings.
3. Add the following rule:
   - **External Port**: 2222
   - **Internal IP**: 192.168.1.100
   - **Internal Port**: 22
   - **Protocol**: TCP

### How to Find Your Internal IP
Run the following command on your home PC to find its internal IP address:
```bash
ip addr
```
Look for an IP address in the format `192.168.x.x`. This is your Internal IP.

## Step 2: Install and Enable SSH

Next, install and enable the SSH service on your home PC to allow remote access.

1. Update your system packages:
   ```bash
   sudo apt update
   ```
2. Install the OpenSSH server:
   ```bash
   sudo apt install openssh-server
   ```
3. Enable and start the SSH service:
   ```bash
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```

## Step 3: Use DuckDNS to Bind Your Dynamic IP

If your home network uses a dynamic IP (which changes whenever your router reconnects), use DuckDNS to bind it to a fixed domain name.

1. Go to the [DuckDNS website](https://www.duckdns.org/) and create an account.
2. Log in and create a subdomain (e.g., `myhomepc`). Afterward, you will have a domain like `myhomepc.duckdns.org`.
3. Note down your DuckDNS token and set up a cron job to automatically update your IP:
   ```bash
   echo "*/5 * * * * curl -k 'https://www.duckdns.org/update?domains=myhomepc&token=YOUR_TOKEN&ip='" | crontab -
   ```
   Replace `YOUR_TOKEN` with your DuckDNS token.

## Step 4: Test Your Remote Connection

After completing all the steps, test the remote connection from an external network.

Use the following command to connect:
```bash
ssh username@myhomepc.duckdns.org -p 2222
```
- Replace `username` with your home PC's username.
- If everything is set up correctly, you should be able to connect to your home PC successfully!

## Notes
- Ensure that your router and computer firewalls allow connections on port 2222.
- For security, use a strong password or other security measures to protect your SSH service.

## Conclusion

By following these steps, you should now be able to securely connect to your home PC from anywhere. If you encounter any issues, double-check each step or review error logs for troubleshooting.
