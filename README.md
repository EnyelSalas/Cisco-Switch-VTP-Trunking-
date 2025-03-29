# Cisco-Switch-VTP-Trunking-

Configuring VLANs and VTP on Cisco Switches: A Step-by-Step Guide
This guide outlines the process for configuring VLANs and VLAN Trunking Protocol (VTP) on a network of five Cisco switches. We'll include definitions, step-by-step instructions, and examples for clarity.

What are VLANs and VTP?
VLAN (Virtual Local Area Network)
A VLAN divides a physical network into multiple logical networks, improving segmentation and security. Devices in the same VLAN communicate as if they were on the same physical network.

Imagine you're managing a network with 50 switches distributed across various floors of a large office building. You need to set up 10 VLANs (e.g., VLAN 10 for Accounting, VLAN 20 for Marketing, etc.) across all these switches. Without VTP, you'd have to manually configure each switch with these VLANs, which is time-consuming and prone to errors.

With VTP:

You designate one switch as the VTP server. On this switch, you configure the VLANs and set a VTP domain name (e.g., "OfficeNetwork").

The other switches are configured as VTP clients. They automatically receive the VLAN configurations from the VTP server.

If you make a change, like adding VLAN 30 for HR, it only needs to be configured on the VTP server. This change is automatically propagated to all the VTP client switches.

Example VLANs:

VLAN 10: IT Department

VLAN 20: Finance

VLAN 30: Human Resources

VLAN 40: Administration

VLAN 50: Production

VTP (VLAN Trunking Protocol)
VTP manages VLAN information across switches within the same VTP domain, ensuring consistency. There are three VTP modes:

Server: Allows VLAN creation, deletion, and propagation to clients.

Client: Inherits VLAN configuration from the server.

Transparent: Forwards VTP messages but does not apply or propagate them.

Configuration Steps
Step 1: Assign Hostnames to Switches
Set distinct hostnames for the switches to improve network management.

VTP Server (Switch1):
plaintext
Switch> enable
Switch# configure terminal
Switch(config)# hostname VTP_Server
VTP Clients (Switch2-5):
plaintext
Switch> enable
Switch# configure terminal
Switch(config)# hostname VTP_Client_1
Switch(config)# hostname VTP_Client_2
Switch(config)# hostname VTP_Client_3
Switch(config)# hostname VTP_Client_4
Step 2: Configure VTP Server
Enter configuration mode:

plaintext
VTP_Server> enable
VTP_Server# configure terminal
Set VTP mode to server:

plaintext
VTP_Server(config)# vtp mode server
Assign the domain name:

plaintext
VTP_Server(config)# vtp domain cisco.com
Set the VTP password:

plaintext
VTP_Server(config)# vtp password cisco
Specify the VTP version:

plaintext
VTP_Server(config)# vtp version 2
Step 3: Configure VTP Clients
On VTP_Client_1 to VTP_Client_4:

Enter configuration mode:

plaintext
Switch> enable
Switch# configure terminal
Set VTP mode to client:

plaintext
Switch(config)# vtp mode client
Assign the same domain name as the server:

plaintext
Switch(config)# vtp domain cisco.com
Set the same VTP password as the server:

plaintext
Switch(config)# vtp password cisco
Step 4: Create VLANs
On VTP_Server:

Enter VLAN configuration mode:

plaintext
VTP_Server(config)# vlan 10
VTP_Server(config-vlan)# name IT
VTP_Server(config-vlan)# exit

VTP_Server(config)# vlan 20
VTP_Server(config-vlan)# name FIN
VTP_Server(config-vlan)# exit

VTP_Server(config)# vlan 30
VTP_Server(config-vlan)# name HR
VTP_Server(config-vlan)# exit

VTP_Server(config)# vlan 40
VTP_Server(config-vlan)# name AD
VTP_Server(config-vlan)# exit

VTP_Server(config)# vlan 50
VTP_Server(config-vlan)# name PRO
VTP_Server(config-vlan)# exit
Save the configuration:

plaintext
VTP_Server# write memory
Step 5: Assign VLANs to Ports
On all switches:

Enter the interface configuration mode for a port:

plaintext
Switch(config)# interface FastEthernet0/3
Set the port to access mode and assign it to a VLAN:

plaintext
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Repeat for other ports and VLANs:

plaintext
Switch(config-if)# switchport access vlan 20
Step 6: Configure Trunk Ports
On all switches, configure the links between them as trunk ports:

Enter the trunk interface configuration:

plaintext
Switch(config)# interface range FastEthernet0/1 - 2
Set the ports to trunk mode:

plaintext
Switch(config-if)# switchport mode trunk
Restrict trunk traffic to specific VLANs:

plaintext
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Step 7: Verify the Configuration
Check VLANs:

plaintext
Switch# show vlan brief
Check VTP status:

plaintext
Switch# show vtp status
Check trunk configurations:

plaintext
Switch# show interface trunk
Troubleshooting Tips
Domain Mismatch: Ensure all switches share the same VTP domain and password.

VLAN Visibility: Verify VLANs are active and allowed on trunk links.

Port Blocking: Resolve Spanning Tree Protocol (STP) inconsistencies.

With these steps, you can configure VLANs and VTP for a consistent and organized network. Feel free to raise issues or contribute enhancements to this guide!
