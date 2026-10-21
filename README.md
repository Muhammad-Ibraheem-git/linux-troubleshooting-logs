# SSH from Termux to Linux Mint: UFW Timeout Fix

## Symptom
I was trying to connect from the OpenSSH client on Termux (Android) to the SSH server on my Linux Mint desktop. Termux kept returning a "connection timed out" error, even though the SSH client was installed and the IP address was correct.

## Investigation
1. Checked the SSH service status on the Mint PC using `systemctl status ssh`. 
2. The output showed `ssh.service` was `inactive (dead)`.
3. However, checking `systemctl status ssh.socket` showed the socket was `active (listening)` on port 22. This meant the server was using systemd socket activation. The service wasn't dead; it was just waiting for a connection to wake it up.
4. Since the server was listening, the timeout meant packets were being dropped before reaching the socket. This pointed to the firewall.

## Root Cause
Checked UFW (Uncomplicated Firewall) using `sudo ufw status verbose`. 
The default incoming policy was set to `deny`, and there were zero allow rules configured. The firewall was silently dropping the incoming SSH SYN packets from Termux.

## Fix
Created a scoped UFW rule to allow SSH traffic only from my local home network (192.168.0.0/24), rather than opening port 22 to the entire internet.

Command used:
```bash
sudo ufw allow from 192.168.0.0/24 to any port 22 proto tcp
