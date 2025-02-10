# Anonymity and Security

### Network Status and Interface Management

###### Important: In the example, "wlan0" is specified. Depending on the network interface, you may need to change this. 
###### I already show you how to find it in my guide.
#
#### [+] 1. Check network status. 
###### view and note active network interface
```bash
iwconfig
```
#### [+] 2. Close network interface.
###### You may see "wlan0" as a different network interface name. For example: "eth0"
```bash
ip link set wlan0 down
```
#### [+] 3. Automatic MAC Address Change Script (recommended)

######  If the tool is not installed:
```bash
sudo apt install macchanger
```

######  A script that randomly changes the MAC address of the network interface every 10 minutes.
```bash
nohup bash -c 'while true; do
    ip link set wlan0 down
    macchanger -r wlan0 > /dev/null 2>&1
    ip link set wlan0 up
    sleep 600
done' > /dev/null 2>&1 &
```
######  To check if the script is working:
```bash
ps aux | grep macchanger
```
#### To stop the script:
###### Replace Process ID with the process ID you viewed with the "ps aux | grep macchanger" command.
```bash
kill process_id (e.g 1234)
```
###### - More information: https://www.kali.org/tools/macchanger/

#### [+] 4. Switch to Monitor Mode
###### Switch Network Distribution to Monitor Mode
```bash
iw dev wlan0 set type monitor
```
###### Re-open Network Distribution
```bash
ip link set wlan0 up
```
#

#### Connecting to the Tor
##### To connect to the Tor network, you must first install and start the Tor service.
1. Install Tor (if not installed):
```bash
sudo apt update
sudo apt install tor -y
```
2. Start the Tor service:
```bash
sudo service tor start
```
3. To check that Tor is working properly:
```bash
sudo service tor status
```
### Redirecting All Traffic to the Tor Network
##### We will configure a system-wide routing using iptables to route all your internet traffic through the Tor network.
###### 1. Edit Tor's configuration file:
```bash
sudo nano /etc/tor/torrc
```
###### At the end of this file, add the following line:
```bash
AutomapHostsOnResolve 1
TransPort 9040
DNSPort 53
```
###### - This will redirect Tor's DNS requests and general network traffic. Save the file and exit (CTRL+O, Enter, CTRL+X).

###### 2. Set up routing with iptables
###### - Now we will create iptables rules to direct all traffic to the Tor network.
###### -- First, apply iptables rules as follows to route your traffic through the Tor network:
```bash
sudo iptables -t nat -A OUTPUT -m owner ! --uid-owner debian-tor -p tcp --dport 9050 -j REDIRECT --to-port 9040
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 9040
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 9040
sudo iptables -A OUTPUT -m owner ! --uid-owner debian-tor -d 127.0.0.0/8 -j REJECT
sudo iptables -A OUTPUT -m owner ! --uid-owner debian-tor -d 0.0.0.0/8 -j REJECT
```
###### - These rules will direct all TCP traffic to Tor's forwarded ports (9040) and block traffic outside of Tor.

###### 3. Save iptables rules:
###### - You need to save iptables rules so that these settings persist after a reboot.
```bash
sudo sh -c 'iptables-save > /etc/iptables/rules.v4'
```
### Checking Connection to Tor Network
```bash
curl --socks5 127.0.0.1:9050 https://check.torproject.org
```
###### - If you are connecting through Tor, you should see the message "Congratulations. This browser is configured to use Tor."

### Get Alerts When Tor Connection Disconnects
```bash
sudo iptables -A OUTPUT -m owner ! --uid-owner debian-tor -d 127.0.0.1 -j REJECT
```
###### - This command will prevent your system from connecting to the internet when the Tor service is not running.

#

### Assigning a Tor connection to an application with the "proxychains" tool in Kali Linux.

###### Steps for Proxychains Installation and Usage:

1. Install Proxychains:
```bash
sudo apt install proxychains -y
```
2. Configure Proxychains settings:
```bash
sudo nano /etc/proxychains.conf
```
3. In the configuration file, make sure the following line is set correctly:
```bash
socks4 127.0.0.1 9050
```
- Save and exit the file (use CTRL + O, press Enter, and CTRL + X).

4. To run an application through the Tor network using Proxychains, use the following command:
```bash
proxychains firefox
```
### To shut down the Tor network:

###### - Stop the Tor service: sudo service tor stop
```bash
sudo service tor stop
```
###### - flush iptables forwarding rules: sudo iptables -t nat -F and sudo iptables -F
```bash
sudo iptables -t nat -F
sudo iptables -F
```
###### - Remove proxychains configuration (if used).
```bash
sudo nano /etc/proxychains.conf
```
###### - Remove the line about Tor (if any):

- Delete or comment out the line socks4 127.0.0.1 9050.

- Save the file and exit.

###### - Check Tor connection.
```bash
curl https://check.torproject.org
```
###### If you get the message "Sorry. You are not using Tor", Tor has been successfully disabled.

### Important Notes for TOR:
- Route all traffic through the Tor network may cause problems for some applications, as Tor may not be suitable for some services (for example, tasks that require very high speeds).
Tor is very effective for providing anonymity and privacy, but it is not 100% secure. You should be careful when using Tor and take additional precautions for anonymity.
The Tor network may be restricted in some countries, in which case you can use alternative tools such as VPN to access Tor.
