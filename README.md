# Berry-Secure
Raspberry Pi 4 Wireguard VPN Setup with personal GUI

## Objective
The Berry-Secure project aimed to establish a way to bypass the schools restricted wifi rule of blocking VPN sites. This was done with no bad intention, just as a matter of saftey and education purposes, since VPNs help keep our personal information and data protected when roaming the internet. The primary focus of this project was to set up and configure a Raspberry Pi 4 (RPi4) to act as a VPN server which we can connect to whenever we are connected to public and unsecure wifi networks. The secondary goal was to build and manually configure Wireguard on our server to have more control over our data, as some VPN services are known to log user activity and/or sell our data to generate revenue. 

## Skills Learned
- Configuring network settings and services on a Linux-based operating system.
- Configuring firewall rules and network interfaces to manage VPN traffic.
- Understanding how to manage client-server VPN configurations, routing, and permissions.
- Configuring VPN security and implementing secure network design principles.
- Understanding how to interface Python with system-level commands.
- Automating VPN connection tasks with Python scripts including start/stop services and more.
- Scripting configurations for WireGuard on a Linux-based system.
- Diagnosing and resolving issues in VPN connectivity, networking and system processes.
- Managing a project lifecycle from concept to implementation.

## Tools Used
- WireGuard (free open-source VPN service).
- Wireshark (for capturing and examining our network traffic).
- Python IDLE (interactive environment for writing, running and debugging Python code).
- Two Raspberry Pi 4's.
- Home router.

## Steps
[If you are going to try this out it must be noted that as of right now these steps ONLY work for devices running a Linux-based operating system.]

### RPi4 Server Setup
1. Download Wireguard
    - `sudo apt install wireguard`
2. Generate keys for the server
    - `cd /etc/wireguard`
    - `umask 077`
    - `wg genkey | tee privatekey | wg pubkey > publickey`
    - `wg genkey | tee client-privatekey | wg pubkey > client-publickey`
3. Create Wireguard configuration file
     - `nano wg0.conf`
     - (will add the contents inside of the config file soon)
4. Enable IP forwarding
     - `nano /etc/sysctl.conf`
     - uncomment line with "net.ipv4 ..." then exit (ctrl+ x)
     - `sudo sysctl -p` OR `sysctl net.ipv4.ip_forward`
5. Setup Wireguard to start on boot
    - `sudo systemctl enable wg-quick@wg0`

### Enable Port Forwarding on Router/Firewall
- protocol: UDP
- Port: 51820
- IP: enter RPi4 server ip address

### Client Setup
1. Install Wireguard
     - `sudo apt install wireguard`
2. Create Wireguard configuration file
     - `nano wg0.conf`
     - insert the client private key and server public key
     - insert the vpn servers address and listening port (51820)
3. Enable ip forwarding
     - `sudo nano /etc/sysctl.conf`
     - uncomment "net.ipv4.ip_forward=1" then exit (ctrl+ x)
4. Start up Wireguard
     - `sudo wg-quick up wg0`
5. To disconnect
     - `sudo wg-quick down wg0`
