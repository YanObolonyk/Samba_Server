# Samba_Server <em>(Currently being processed)</em>
Showcase project for creating and configuring a Samba server for file transfer.

## Objective of the project
Create a network of machines and enable the possibility to share files between all of them, regardless of OS.

## Lab environment description:
3 VMs hosted on Oracle VirtualBox (v. 7.1.12 at the moment of project creation):
- Ubuntu Server 24.04
- Ubuntu Desktop 24.04
- Windows 10 22H2

## Realisation
### 1. Place all the machines in the same network

In Oracle VirtualBox it is done by choosing the Internal Network in the Network Adapter options. Naming a new network "labnet" and assigning all machines to it. The network settings should be same for 3 VMs and look as follows:

<img width="371" height="263" alt="image" src="https://github.com/user-attachments/assets/78b79dbb-e66e-43cd-b617-111d7b14bbc5" />

With this done, VMs are now within the same network.

### 2. Assign static IPv4 addresses 

#### Ubuntu Server
Navigate to Netplan configuration:

cd /etc/netplan/

Check contents with:

ls 

<img width="315" height="36" alt="image" src="https://github.com/user-attachments/assets/c8a1f9ff-8def-4ac0-896d-d4e4dc765eb0" />

Then open the config file by its name with editor:

sudo nano 50-cloud-init.yaml

Edit file to the following:

<img width="244" height="133" alt="image" src="https://github.com/user-attachments/assets/d2ccabe2-f9c8-493d-bed2-becd8e51ee5b" />

Getaway is not used now, so it is not a necessity to assign it. Save changes and exit the editor. Nextly, apply changes with:

sudo netplan apply

After that check if the changes came into effect:

ip a

<img width="843" height="230" alt="image" src="https://github.com/user-attachments/assets/0220ef55-c385-4ba1-8f84-a7f4c9bee7ac" />

"inet" at enp0s3 interface should be of recently assigned address.

#### Ubuntu Desktop
Go to Settings and then to the Network. Find the enp0s3 interface:

<img width="602" height="127" alt="image" src="https://github.com/user-attachments/assets/fbd3cbc3-b5cf-43ca-b795-851ff4114dd6" />

Go to its settings and to IPv4 tab. Change method to Manual and assign needed address and subnet mask:

<img width="757" height="373" alt="image" src="https://github.com/user-attachments/assets/3310d616-5f97-4b33-96e3-84ecd3a9fc16" />

Apply changes. As previously, getaway is not used.

#### Windows 10

### 3. Ensure bidirectional connection between clients and server
### 4. Install and configure Samba on Ubuntu Server machine
### 5. Test the operability
