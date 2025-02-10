# VPN and DNS Leak Test

## VPN
#### Start VPN if you do not have one, below is the installation guide for ProtonVPN.

##### - ProtonVPN setup guide for Kali Linux:
###### To create an account on the ProtonVPN website, you first need to sign up. Visit ProtonVPN's official website and go to the "Pricing" section. ProtonVPN offers four main plans: Free, Basic, Plus, and Visionary. In this guide, we’ll focus on the "Free" plan, which provides free VPN access for 7 days. Follow the steps on their website to get started with your account.

##### - For creating an account on ProtonVPN you just need to follow the following steps:

###### [+] 1. Visit the ProtonVPN’s Official Website
###### - https://protonvpn.com/

###### [+] 2. If you don’t already have a ProtonVPN account, click on the "Sign Up" option and select your preferred pricing plan. If you already have an account, simply choose the "Login" option to proceed.

###### [+] 3.  Fill in the required details, such as your username, password, and email address, to create your ProtonVPN account.

###### [+] 4. Make sure to save your username and password by copying and pasting them somewhere secure. You’ll need these credentials later during the ProtonVPN installation and configuration process.

##### - Steps to Install ProtonVPN on Kali Linux Operating System:

###### [+] 1. Open a terminal and run the following command to install OpenVPN:
```bash
sudo apt update
sudo apt install openvpn dialog python3-pip python3-setuptools
```

###### [+] 2. To install ProtonVPN CLI, use the pip package manager by running the following command.
```bash
sudo pip3 install protonvpn-cli
```
###### - Now, both OpenVPN and ProtonVPN CLI have been successfully installed on your Linux system, and you are ready to proceed with configuring and using ProtonVPN.
##### - Now we can move on to installing ProtonVPN on Kali Linux.
###### - After successfully installing the ProtonVPN packages, the next step is to configure ProtonVPN for your system. This involves logging into your account, setting up the necessary preferences, and ensuring everything is ready for secure VPN connections. Follow the configuration steps carefully to complete the setup process.

###### [+] 1. Run the following command to begin the configuration process:
```bash
sudo protonvpn init
```

###### - This command will prompt you through some configuration steps. The first thing you need to do is provide your ProtonVPN username and password for authentication. Once entered, the system will authenticate your credentials and move on to the next configuration steps.

###### [+] 2.  After authentication, you will be prompted to select your ProtonVPN plan. Choose the plan you are subscribed to (e.g., Free, Basic, Plus, or Visionary). This will configure the VPN to use the servers and features available with your selected plan.

###### [+] 3. Next, you will be prompted to choose your preferred VPN protocol. You can select from options such as UDP, TCP, or others. In this case, choose UDP for faster speeds.

###### [+] 4.  After selecting your preferred protocol and reviewing the configuration settings, you will be asked to confirm that all the information is correct. To confirm, press Y and then hit Enter. This will finalize your configuration.

##### - Disable IPv6 Permanently
###### [+] 1. To avoid IPv6 leaks and ensure that ProtonVPN works without issues, you can disable IPv6 by modifying the /etc/sysctl.conf configuration file.
```bash
sudo vim  /etc/sysctl.conf
```
###### [+] 2. After executing the command to open the /etc/sysctl.conf file, you need to append the following lines to the very end of the file to fully disable IPv6 and prevent any leaks
```bash
net.ipv6.conf.all.disable_ipv6 = 1 
net.ipv6.conf.default.disable_ipv6 = 1 
net.ipv6.conf.lo.disable_ipv6 = 1 
net.ipv6.conf.tun0.disable_ipv6 = 1
```

###### [+] 3. To save the changes, after editing the /etc/sysctl.conf file

###### - Save the changes:

###### - Press CTRL + X to exit the editor.
###### - Press Y to confirm saving the changes.
###### - Press Enter to finalize the save.

###### Apply the changes: To apply the changes you made to the sysctl file, run the following command in the terminal:
```bash
sudo sysctl -p
```

###### To establish the ProtonVPN connection after disabling IPv6 and completing the configuration steps, run the following command:
```bash
sudo protonvpn connect
```
###### - Then Select the Country to which you want to connect your IP.
###### - Then Pick the Server location. choose the server which has a minimum load for better speed.
###### - Then select a Protocol (TCP, UDP).
###### - After all these steps you are now connected to the ProtonVPN.

###### For Disabling ProtonVPN, enter the following command:
```bash
sudo protonvpn disconnect
```

#### You have completed the installation and connection processes, now your ProtonVPN is ready to work securely. Disabling IPv6, preventing potential leaks and ensuring a secure internet connection. If you encounter any issues, you can review the steps or get help from ProtonVPN support resources.

## DNS Leak Test

###### [+] 1. Visit the Website:
- https://dnsleaktest.com

###### [+] 2. Select a Test Mode:
######  - On the homepage, you’ll see two types of DNS leak tests:

###### - Standard Test: A quick test to check for DNS leaks.
###### - Extended Test: A more comprehensive test that shows the DNS servers used by your device in more detail.
###### - Click Standard Test for a quick check or Extended Test for a deeper look.

###### [+] 3. Start the Test:
###### - Once you click your preferred test, the website will start checking your DNS servers. This may take a few seconds.

###### [+] 4. View the Results:
###### - After the test completes, it will show you the DNS servers that are being used. If you're using a VPN or proxy, the test should ideally show the DNS servers associated with that service.

###### - If it shows DNS servers from your ISP (Internet Service Provider), it means there is a DNS leak, meaning your real IP is potentially exposed.
###### - If the DNS servers correspond to your VPN or proxy provider, your DNS is secure, and there is no leak.

###### [+] 5. Analyze the Results:

###### - No Leak: If the DNS server addresses match your VPN or private DNS, then you are safe, and there is no leak.
###### - Leak Detected: If your real ISP's DNS servers are shown, you should consider changing your DNS settings, or ensuring your VPN’s DNS is configured correctly.

###### [+] 6. Fix DNS Leaks (If Detected):
###### - If you detect a leak, you can:

###### - Use a VPN with DNS leak protection: Most reliable VPNs have built-in DNS leak protection features.
###### - Manually change your DNS settings: Use third-party DNS servers like Google DNS or Cloudflare DNS.
###### - Check VPN settings: Ensure that your VPN is correctly configured to route DNS requests through the VPN.

###### [+] 7. Test Again:
###### - After making adjustments, rerun the test to make sure your DNS is secure.


### This website is useful for confirming that your DNS queries are being routed properly and that your privacy is maintained when using services like a VPN.

