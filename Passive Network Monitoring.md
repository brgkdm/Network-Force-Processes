# Passive Network Monitoring

##### [+] 1. Put the network interface in monitoring mode: 
###### - Before you start passive network monitoring, you need to put your wireless network interface in monitoring mode. 
###### - This will allow the interface to capture all wireless traffic, not just traffic to and from your device.

###### - To set it in monitoring mode, use the following command:
```bash
sudo ip link set wlan0 down
sudo iw dev wlan0 set type monitor
sudo ip link set wlan0 up
```
##### [+] 2. Run airodump-ng to monitor the network:
###### - Use the airodump-ng tool to passively monitor wireless networks. 
###### - This tool will capture the packets being transmitted between devices on the network. 
##### - The basic command is:
```bash
airodump-ng wlan0
```
###### - Replace wlan0 with the name of your wireless interface if it's different. This will display a list of nearby networks, including the SSIDs, BSSIDs, and other information.

##### [+] 3. Analyze and collect data:
###### - While airodump-ng is running, it will show information about the nearby wireless networks, including the clients connected to each network. You can use this data for further analysis, such as identifying weak security configurations or monitoring device activity.

This process allows you to passively capture traffic without interacting with the devices, making it a stealthy way to observe network activity.
