🚀 OpenVPN Installation Guide

This guide explains how to install and set up OpenVPN Access Server on an AWS EC2 instance inside a VPC.

⚙️ Prerequisites

An AWS account with permissions to create VPCs, subnets, EC2 instances, and security groups

Basic knowledge of Linux commands and SSH

OpenVPN installation script

🏗️ Step 1: Create a VPC

Create a VPC with these settings:

VPC: 1

Public Subnets: 2 🌐

Private Subnets: 2 🔒

Internet Gateway (IGW): 1

NAT Gateway: 1

Make sure your route tables are set so public subnets can access the Internet through IGW and private subnets through NAT.

💻 Step 2: Launch an EC2 Instance

Launch a public EC2 instance in one of the public subnets.

Assign a public IP to the instance 🌍.

Connect to the instance using SSH 🔑.

🛠️ Step 3: Install OpenVPN Access Server

Run these commands on your EC2 instance:

sudo apt update
sudo bash -c 'bash <(curl -fsS https://packages.openvpn.net/as/install.sh) --yes'


This will install OpenVPN Access Server from the official website ✅.

📄 Step 4: Check the Installation

After installation, you should see a message like this:

Access Server 3.0.2 has been installed in /usr/local/openvpn_as
Admin UI: https://10.0.1.221:943/admin
Client UI: https://10.0.1.221:943/
Use "openvpn" account with the provided password


Open the Admin UI in your browser using your EC2 public IP:
https://<EC2-PUBLIC-IP>:943/admin 🌐

🔧 Step 5: Configure the Server

Log in to the OpenVPN Admin UI.

Change the VPN server from the private IP to your public IP 🌍.

🛣️ Step 6: Set Up Routing

Run these commands to allow your subnets to use the VPN:

sudo /usr/local/openvpn_as/scripts/sacli --key "vpn.server.routing.private_network.0" --value "10.0.0.0/16" ConfigPut
sudo /usr/local/openvpn_as/scripts/sacli start
sudo systemctl restart openvpnas

🔑 Step 7: Connect to the VPN

Install the OpenVPN client on your device.

Download the configuration file from the client UI and connect ✅.
