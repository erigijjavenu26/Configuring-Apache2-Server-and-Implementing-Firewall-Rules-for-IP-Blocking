# Configuring Apache2 Server and Implementing Firewall Rules for IP Blocking

## Goal
Set up networking between a Windows PC and a Kali Linux VM, install and configure Apache2 on Kali, host a test HTML page, access it from Windows using the Kali IP address, identify the Windows IP address from Apache logs, and finally block that Windows IP using the firewall on Kali Linux.

## Objectives
- Establish network communication between Windows and Kali
- Install and configure Apache2 web server in Kali Linux
- Host a custom HTML test page on the server
- Access the web page from the Windows system using Kali's IP
- Identify the Windows client IP from Apache logs
- Block the Windows IP using firewall rules

## Step-by-Step Procedure

### Step 1: Check and Configure Network Connection
1. Open VirtualBox / VMware settings for your Kali Linux VM.
2. Under **Network**, select **Bridged Adapter** (recommended) so Kali gets an IP from the router.
3. Start Kali Linux.

Verify IP address:
```bash
ip a
```

### Step 2: Check IP Address on Windows
```powershell
ipconfig
```
Note down the IPv4 address (e.g. `192.168.1.9`).

### Step 3: Verify Communication Between Kali and Windows
On Kali:
```bash
ping -c 4 192.168.1.9
```
On Windows:
```powershell
ping 192.168.1.13
```
If ping works, both systems are connected correctly.

### Step 4: Install Apache Web Server on Kali
```bash
sudo apt update
sudo apt install apache2 -y
```
Start and enable Apache:
```bash
sudo systemctl enable apache2
sudo systemctl status apache2
```
Expected output: `Active: active (running)`

### Step 5: Create a Simple HTML Web Page
```bash
cd /var/www/html
sudo nano index.html
```
Add the following HTML:
```html
<!DOCTYPE html>
<html>
<head>
  <title>My Web Page</title>
</head>
<body>
  <h1>Hello, Welcome!</h1>
  <p>This is a basic HTML page.</p>
</body>
</html>
```
Save using: `Ctrl + O` → `Enter` → `Ctrl + X`

### Step 6: Access the Web Page from Windows
Open a browser on the Windows machine and navigate to:
```
http://192.168.1.13
```
You should see the hosted web page.

### Step 7: Check Access Logs to Identify Windows IP
```bash
cd /var/log/apache2
sudo tail -n 20 /var/log/apache2/access.log
```
This shows recent client requests, including the Windows machine's IP address.

### Step 8: Enable Firewall and Allow Web Traffic
```bash
sudo apt install ufw -y
sudo ufw enable
sudo ufw allow 80/tcp
sudo ufw status
```

### Step 9: Block the Windows IP
```bash
sudo ufw deny from 192.168.1.9
```
Check the rules:
```bash
sudo ufw status numbered
```

### Step 10: Verify the Block Worked
On Windows, refresh the browser. The web page should fail to load (connection refused).

## Conclusion
In this project, we learned how to connect Windows and Kali Linux and make them communicate. We set up a web server on Kali and accessed the web page from Windows using the IP address. Finally, we saw how to check access logs and block a client IP using the firewall for security.
