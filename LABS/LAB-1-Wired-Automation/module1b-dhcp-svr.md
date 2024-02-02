# DHCP Discovery using Windows DHCP Service

As you may recall, for a device to discover Cisco Catalyst Center, the device uses a discovery method to help it find Cisco Catalyst Center. 

The PnP components are as follows:

![json](../../ASSETS/pnp-workflows.png?raw=true "Import JSON")

There are three automated methods to make that occur and in this section we will use DHCP Discovery. To aide in that we are goiing to utilize Windows DHCP services. The Target switch, commonly called Access switch will need to be offered option 43 with a string in order to find Cisco Catalyst Center.

That string is offered as a scope option typically referred to as **Option 43** where the string **`5A1D;B2;K4;I198.18.129.100;J80`** can be explained as follows:

```shell
Option 43 format 
 The option 43 string has the following components, delimited by semicolons:
 
PnP string: 5A1D;B2;K4;I198.18.129.100;J80 
 
Description - dhcpc receiving DHCP Offer with option 43 info, pass the info to PNPA 
 *     5A = PnP DHCP ID
 *     1D = PnP DHCP debug on
 *     1N = PnP DHCP debug off
 *     token.B = <address type> 1:Host; 2:IPv4; 3:IPv6
 *     token.K = <protocol> 1: XMPP-starttls; 2: XMPP-socket; 3:: XMPP-tls; 4: HTTP; 5: HTTPS
 *     token.I = <remote server ip add / hostname>
 *     token.J = <remote server port> valid ports are 80 or 443
 
Further explanation of PnP string and how to read:

5A1N;             Specifies the DHCP suboption for Plug and Play, active operation, version 1, 
                  no debug information. It is not necessary to change this part of the string.
                  
B2;               IP address type: in this case IPv4 format
                  Alternates would be: 
                            B1 = hostname
                            B2 = IPv4 (default)

K4;               Transport protocol to be used between the device and the controller: In 
                  this example HTTP transport
                  K4 = HTTP (default)
                  K5 = HTTPS

Ixxx.xxx.xxx.xxx; IP address or hostname of the Cisco Catalyst Center controller (following a 
                  capital letter i). In this example, the IP address is 198.18.129.100.
                  
Jxxxx             Port number to use to connect to the Cisco Catalyst Center controller. 
                  In this example, the port number is 80. The default is port 80 for HTTP 
                  and port 443 for HTTPS.
```

#### Step 3.2a - Windows DHCP and Option 43 Discovery Configuration

If we want to use the Windows DHCP service, connect to the windows **AD1** server. On the windows server, you have two options to deploy DHCP scopes the UI or PowerShell. We will deploy the scope via PowerShell. Paste the following into PowerShell to create the required DHCP scope:

```ps
Add-DhcpServerv4Scope -Name "DNAC-Templates-Lab" -StartRange 192.168.5.1 -EndRange 192.168.5.254 -SubnetMask 255.255.255.0 -LeaseDuration 6.00:00:00 -SuperScope "PnP Onboarding"
Set-DhcpServerv4OptionValue -ScopeId 192.168.5.0 -Router 192.168.5.1 
Add-Dhcpserverv4ExclusionRange -ScopeId 192.168.5.0 -StartRange 192.168.5.1 -EndRange 192.168.5.1
```

The DHCP scope will look like this in Windows DHCP Administrative tool:

![json](./images/WindowsDHCPscoperouteronly.png?raw=true "Import JSON")

To deploy Option 43 on the Windows DHCP Server Scope, paste the following configuration into PowerShell:

```ps
Set-DhcpServerv4OptionValue -ScopeId 192.168.5.0 -OptionId 43 -Value ([System.Text.Encoding]::ASCII.GetBytes("5A1N;B2;K4;I198.18.129.100;J80"))
```

The DHCP scope modification will resemble the following image of the Windows DHCP Administrative tool:

![json](./images/DNACDHCPoption43.png?raw=true "Import JSON")

Configure the **helper-address** statement on the management VLAN's SVI to point to the Windows DHCP server. Connect to switch **c9300-2** and paste the following configuration:

```vtl
!
conf t
!
  interface Vlan 5                         
     ip helper-address 198.18.133.1                  
     end
!
wr
!
```

## Verification and Testing

To test the environment to ensure it's ready, we need to try a few things.

First, lets check the configuration on the **9300-2**:

```bash
sh vlan id 5

sh run int vlan 5

ping 192.168.5.1
```

The test results should look similar to this:

![json](blank.png?raw=true "Import JSON")

Second, we need to ensure the Cisco Catalyst Center responds on the VIP, lets test that from the **9300-2**:

```bash
ping 198.18.129.100 repeat 2
ping 198.18.129.100 source vlan 5 repeat 2
```
The test results should look similar to this:

![json](./images/blank.png?raw=true "Import JSON")

At this point, the environment should be set up to onboard devices within VLAN 5 using the network address **192.168.5.0/24** utilizing **DNS discovery**

> [**Return to PnP Preparation Lab**](./module1e-reset.md#step-6---reset-eem-script-or-pnp-service-reset)