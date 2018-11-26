# SDDC Interconnectivity

### The Importance of Proper IP Administration
At minimum, each SDDC will be cross-linked to a VPC within the customer's AWS account. Typically, however, it will also be connected to other networks as well (such as an on-premises environment). In order to ensure that the SDDC can communicate with other interconnect networks, it is vital that IP addressing be properly planned.

Though not required, it is a good practice to allocate IP address space in large, contiguous chunks. The following table provides an example IP Administration plan.

Supernet     | Subnet1      | Subnet2      | Description
-------------|--------------|--------------|------------
10.1.0.0/19  |              |              | on-premises networks
10.1.32.0/19 |              |              | AWS native
10.1.32.0/19 | 10.1.32.0/22 |              | AWS Services VPC
10.1.32.0/19 | 10.1.32.0/22 | 10.1.32.0/26 | AWS Services VPC SDDC x-link
10.1.64.0/19 |              |              | SDDC1
10.1.64.0/19 | 10.1.64.0/20 |              | SDDC1 Management
10.1.64.0/19 | 10.1.80.0/20 |              | SDDC1 Compute
10.1.64.0/19 | 10.1.80.0/20 | 10.1.80.0/24 | SDDC1 Compute Servers



### VPC Cross-Linking
As mentioned previously, SDDCs are given access to AWS services by cross-linking them to a VPC within a customer-owned AWS account. As indicated by the diagram, cross-linking is made possible by ENIs which have been attached to a dedicated subnet within that VPC.

![vpcCrossLink.png](./illustrations/vpcCrossLink.png "VPC Cross-Linking")

It should be noted that the first cluster of the SDDC will be deployed within the same availability zone as the cross-link subnet.

Routing between the SDDC and the VPC is enabled by using static routes which are created on-demand as networks are added to the SDDC.



### IPSec VPN
In the majority of setups, customers wish to maintain some sort of permanent means of direct connectivity between the SDDC and their on-prem environment.

![ipsecVPN.png](./illustrations/ipsecVPN.png "IPSec VPN")

IPSec VPN provides secure connectivity to the private IP address ranges of the SDDC, and is implemented with a tunnel to the edge router.
There are 2 flavors of IPSec VPN available: policy-based VPN, and route-based VPN.

Policy-based VPN is typically the easiest solution to implement, but requires that the network administrator manually configure the tunnel to permit specific source and destination IP ranges through.

Route-based VPN is a bit more complex, involving the creation of virtual tunnel interfaces and BGP routing configurations, but is also much more flexible.

One note on redundancy. 



### Direct Connect
Under normal circumstances, customers will access their SDDC via the public internet; either directly to VM public IP addresses, or to private addresses via IPSec VPN. Often times, however, customers wish to avoid using their public internet provider for connectivity to the SDDC. For these cases, AWS offers [Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html), which provides direct connectivity into an AWS region via private leased lines.

Public VIF enables the Direct Connect to be used for accessing the AWS public network.

Private VIF, on the other hand, enables Direct Connect to be used for accessing the private IP address space of a VPC.

Lets explore this in more detail.


#### Public VIF
Public VIF enables a Direct Connect to be used for accessing the public IP address space of  the AWS network.

Normally, customers will have one or more public internet circuits over which they will receive either default routes, or specific BGP prefixes.

![dxPublicVIF.png](./illustrations/dxPublicVIF.png "Direct Connect Public VIF")

When Direct Connect is enabled, and a Public VIF created, AWS will begin to announce all of their public IP prefixes, via BGP, over the Direct Connect.

One important consideration when using Public VIF is to keep in mind that the on-prem network must also be reachable via its own public IP address space.

For these cases, customers may submit a request to AWS for a public IP. 



#### Private VIF
The standard means of accessing the private address space of an SDDC is via IPSec VPN.

![dxPrivateVIF.png](./illustrations/dxPrivateVIF.png "Direct Connect Private VIF")

As mentioned previously, each SDDC resides within a dedicated VPC which is owned by a master VMware account. 

Once the VIF has been terminated, the SDDC begins advertising routes through the Private VIF, via BGP.

As seen in the diagram, the edge routers of the SDDC are in-path for Direct Connect. 



#### [Top](./README.md) | [Back <- SDDC Networking](./03_sddcNetworking.md)