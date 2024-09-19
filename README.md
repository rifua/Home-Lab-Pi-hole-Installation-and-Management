# Pi-hole Home Lab

## Introduction

Pi-hole is designed to act as a network-wide ad blocker for your home. It's a DNS sinkhole: it will reject any requests to known ad-serving domains, thus ensuring that no device on your network actually has ads served to it. Commonly installed on a Raspberry Pi, but there are many hardware devices that can run this. This project was done on a local home lab environment and Pi-hole was installed on an Ubuntu Server. 

#### Note: Raspberry Pi Not Used for Pi-hole Installation

## How Pi-hole Works?

- DNS Filtering: Pi-hole does look at the request of any device against the blocklist the moment any request comes for a website. It blocks resolution when found on the list and returns a blank page, or any address specified.Because Pi-hole works at the DNS level, thus blocking advertisements across your network for all devices, whether smartphones, tablets, or even smart TVs, without requiring you to install an ad-blocking software on each device.

- Custom Blocklists: Users can whitelist or blacklist domains using a blocklist, giving them the capability to create custom ad-blocking experiences.

- Dashboard: Pi-hole has a web interface that shows the statistics of blocked queries and the total number of requests, among many other things that can be used to monitor performance quite easily.

- Improved Privacy: Pi-hole will block tracking domains, which goes a long way in enhancing your online privacy by reducing the amount of data collected by advertisers.

## Objectives

- Install and Manage Pi-hole on a Home Lab
- Monitor Network Traffic
- Block Unwanted Ads

## Skills Learned

- DNS Management: Understand general DNS principles, Pi-hole inner workings that include the function of a DNS sinkhole.

- Network Configuration: Setup and configuration skills to apply for network settings, including but not limited to DHCP and DNS forwarding.

- Virtualization: Experience working in VirtualBox, including creation, management, and optimization of resource allocation of virtual machines.

- Ad and Tracker Blocking: General understanding of how ad-blocking technologies work, including how custom block lists work.

- Pi-hole Web Interface Management: Understand how to navigate and use the Pi-hole web dashboard to perform basic monitoring and analytics.

- Security Best Practices: Understanding of privacy-related concerns and how DNS filtering must be used to minimize tracking. Documentation and Reporting: Document processes, configurations, and findings for future use or knowledge sharing.

## Tools Used

- Host OS: Windows 11
- VirtualBox
- Ubuntu Server 24.04
- Pi-hole

## Steps

### Step 1 : Download & Install Ubuntu Server
![Ubuntu Server](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/a680ec46-14fa-4fe9-9f00-c41a39a96a55)
We proceed by installing Ubuntu Server 24.04 on our virtual box. Please refer to guides on how to download and install Ubuntu Server on a VirtualBox if you do are not familiar with it. 

### Step 2 : Providing Static IP to Ubuntu Server
![Network Settings](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/4a681269-994f-4e65-bab6-3961d94d218b)
We'll need to setup a Static IP for our Ubuntu Server as it will be used as our DNS Server after we install Pi-hole on it. 

#### Note: We will forward DNS requests from all the devices to Pi-hole for Ad-blocking by adding the Pi-hole Static IP as DNS Server for every devices.

To add a static IP for Ubuntu Server, we will modify a configuration file. As seen on the screenshot above, there is no static IP provided. 

Now, we will change the configuration file. The configuration file is stored in /etc/netplan/50-cloud-init.yaml

#### Command Used: sudo nano /etc/netplan/50-cloud-init.yaml

![Changed to Static IP](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/0d499961-2f16-4aa2-a27a-bcab2e6a0b9b)
Now we will be making the following changes: 
- dhcp: false
- addresses: - 192.168.1.101/24 (Which is the STATIC IP for Ubuntu Server along with Subnet Mask)
- route: - to: default
- via: 192.168.1.101 (Gateway)
- nameservers:
- addresses: [8.8.8.8] (DNS Server for Ubuntu Server)
- Save the File
- sudo netplay apply (Ignore the Warnings)

#### Note: Make Sure the New Static IP Has Been Applied
#### Command: ip -a
### Step 3 : Installing Pi-hole 
![Installing Pi-hole](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/ed4e08f7-3c34-43de-8fd8-16cc8a14c436)
After conforming that the Static IP has been applied, we now move-on towards installing Pi-hole on the Ubuntu Server by downloading it.

![Installing Pi-hole 2](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/eb31fb43-4f5a-40a5-a472-c5611bbf593f)
#### Command Used: wget -O pi-installer.sh https://install.pi-hole.net

![Alert - Installing Pi-hole ](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/8c6a5043-0179-41bf-9ecf-3eca3f01d819)
You will get a message just go along with it, and make sure you install the Admin-Web Interface

![DNS Selection - Installing Pi-hole ](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/5c2e0501-be4c-4319-995b-428d0d25acf0)
Select any DNS you want to or add your custom DNS. In my case I have selected Cloudflare for additional security.

![Privacy Mode - Installing Pi-hole 2](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/59dc2d55-a2a9-4026-b0c4-6d9038bd2c42)
Leave the Privacy Mode as Show everything.

![Installing Pi-hole - Final Step ](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/7d85472e-d262-41c3-988b-54303aa2df34)
#### Note: Make sure that you note down the URL for Pi-hole web-interface and the Password.

### Step 4 : Changing Automatic DNS to Manual
![image](https://github.com/user-attachments/assets/8e3b1c11-6add-442a-8512-a18f495c1ffa)
Now we change the network setting of devices that we want to block ads on. Simply add Pi-hole IP as the DNS in the network settings. We can do the same for mobile devices as well. 

### Step 5 : Pi-hole Web Admin Portal
![Pi-hole Web Admin Login](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/fef30d03-2028-4bfe-8790-c506985ab17f)
After successfully adding DNS server manually on different devices, we will now move on to the Pi-hole Web Admin Portal. 
#### URL Used:http://192.168.1.101/admin/login.php
#### Note: Use the URL, and Login Credentials from that was provided at the end of Pi-hole installation.

![Pi-hole Logged In Dashboard](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/1d4c3c5b-6a23-4a1b-9412-9b2af050d723)
Pi-hole Web-Interface dashboard with Total Queries, Queries Blocked, and different other information can be seen. 

![Pi-hole Logged In Dashboard 2](https://github.com/rifua/Home-Lab-Pi-hole-Installation-and-Management/assets/160899842/dd8aff8a-7c61-46fd-b918-bd396b510f0d)
Pi-hole Dashboard

### Step 6 : Additional Contents

- We can add different sites that we want to block from the Pi-hole web-interface.
- Query Log can let us view domains blocked and allowed. 


