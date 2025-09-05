# Samba_Server <em>(Step 4 + formatting + mb change ukr)</em>
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
Go to Network & Internet - Change Adapter Settings. In the list of properties choose IPv4. In the drop-down menu, change Automatic (DHCP) to Manual. It should be changed to the following:

<img width="400" height="454" alt="image" src="https://github.com/user-attachments/assets/c4f765e0-c78c-4658-a413-519b2376f7b2" />

Save changes.

### 3. Ensure bidirectional connection between clients and server
To check whether both the client and server sides are visible, the ping command will be used.

#### Important note
If you have a firewall set up on any of the machines, ensure firtsly that in/outbound traffic for ping command and Samba service is allowed. If firewall is not used, [skip this step](https://github.com/YanObolonyk/Samba_Server/blob/main/README.md#windows-10-client-to-server-and-back).

#### Ubuntu
Check the firewall status:

sudo ufw status

If enabled, add:

sudo ufw allow samba

Recheck the allowed services.

#### Windows
For ping to work, go to Windows firewall settings. Choose Inbound Rules. Find and ensure to enable File and Printer Sharing (Echo Request - ICMPv4-In) for the Private profile (or for the one where the network is).

#### Windows 10 client to server and back
Successful ping TO server:

<img width="574" height="194" alt="image" src="https://github.com/user-attachments/assets/84ee8af8-400a-43ed-b832-08b1e9cc8588" />

Successful ping FROM server:

<img width="544" height="259" alt="image" src="https://github.com/user-attachments/assets/9557b6d8-7a13-4767-8d1d-72eb0872b69d" />

#### Ubuntu Desktop client to server and back
Successful ping TO server:

<img width="663" height="317" alt="image" src="https://github.com/user-attachments/assets/e19f9d08-10f3-41b3-859b-a91bee8cf6ef" />

Successful ping FROM server:

<img width="520" height="220" alt="image" src="https://github.com/user-attachments/assets/e2902d39-2866-4753-94f3-5d9370c4bdcc" />

### 4. Install and configure Samba on Ubuntu Server machine
### 5. Test the operability
#### Create a test file in shared folder
Go to share folder:

cd /srv/samba/share

Create a test file:

sudo touch test.txt

Edit it to add some contents:

sudo nano test.txt

<img width="147" height="53" alt="image" src="https://github.com/user-attachments/assets/5f126e0d-70c5-487a-990f-a0c602a4e54e" />

Save the changes and exit.

#### Connect from Windows machine
Right-click on This PC and choose Add network location. Then choose to add the address and write the IP address of the server and the name of the shared folder:

<img width="304" height="53" alt="image" src="https://github.com/user-attachments/assets/e5190aed-13b7-49c7-9c4f-8f329474724b" />

Enter the login and password created for the Samba user. After that choose the location name or leave the defaults.

<img width="295" height="124" alt="image" src="https://github.com/user-attachments/assets/893c55fe-3ebe-41b3-89d8-a9c15225ec75" />

Now the location is added and the test file created on server is there. Checking the contents:

<img width="188" height="96" alt="image" src="https://github.com/user-attachments/assets/3b4bee6a-f16a-43b8-a626-b5248aec3489" />

#### Connect from Linux machine
In file explorer click:

<img width="203" height="94" alt="image" src="https://github.com/user-attachments/assets/315a13f4-14df-473a-a8db-af738d354ce0" />

In search field write the address of the server in the following form:

smb://192.168.10.10/share

<img width="511" height="86" alt="image" src="https://github.com/user-attachments/assets/b8b02176-e5dc-4076-9618-2f0a2bbd8764" />

Enter the credentials:

<img width="514" height="492" alt="image" src="https://github.com/user-attachments/assets/64ef0d0a-03e3-4530-adb0-7821e043a104" />

Connect. The location is added, with the test file inside:

<img width="903" height="553" alt="image" src="https://github.com/user-attachments/assets/4d018137-68a8-4168-9a1c-58dabd81a178" />

Checking the contents:

<img width="492" height="129" alt="image" src="https://github.com/user-attachments/assets/0a5af93f-5040-437f-9328-4ab973182794" />

## Results
During the deployment of this laboratory environment, three virtual machines were created in VirtualBox and connected to a shared internal network. Static IP addresses were configured, a Samba server was installed and configured, a separate user account was created, and a shared directory was organized for access from both Windows and Linux clients. To verify functionality, the connections were tested, access rights were configured, and configuration errors were eliminated (in particular, those related to YAML syntax in Netplan, firewall operation, and Samba binding to the network interface).

The main difficulties that could be encountered during setting the server up were:
- errors in formatting the Netplan configuration, which blocked the network from coming up;
- incorrect binding of the Samba service to the required interface (the service was running but not listening to external connections);
- configuring access rights and creating Samba accounts to avoid “Access denied” when connecting from Windows;
- possible firewall restrictions and blocking of ports 445/139.

The result is a working environment with the ability to exchange files between Linux and Windows. Requires basic skills in system administration, working with network services, and troubleshooting.


