# Known Caveats

---
## Networking
---

### HCX over Direct Connect Private VIF requires manual configuration by VMware
In order for HCX traffic to ride over Direct Connect Private VIF, the cloud-side appliances must bind to a private IP. For this, customer's must allocate an additional private IP range for HCX to use. The general practice is to allocate this block from the larger supernet allocated to the Compute Network of the SDDC. A /27 allocation is the most common. Today, the configuration for this setup must be performed manually by VMware.

The process for configuring HCX for Private VIF will be as follows:
1. Customer to activate HCX
2. Customer to allocate private IP range for cloud-side HCX appliances
3. Customer to contact VMware via live-chat and provide the private IP range
4. VMware to perform the configuration and contact the customer when finished
5. Customer to continue with on-premises installation

There is one additional change that the customer must make, and this is with the site-pairing. Normally, the site-pairing will be done to the EIP of the cloud-side HCX Manager. However, this is not desirable in the case of Private VIF. In this case, the customer will want to establish the pairing with the private IP of the HCX manager such that the traffic will ride over the Direct Connect. This IP may be found directly within vCenter by looking at the HCX Manager which has been deployed within the SDDC. The customer must then create a DNS entry for this IP on their DNS server (or create an entry within /etc/hosts of the on-prem HCX Manager). This entry should reflect the FQDN of the cloud-side HCX Manager (e.g. hcx-sddc.xx-xx-xx-xx.vmwarevmc.com).


### IP assignment for additional WAN Interconnect/Extension appliances requires VMware assistance.
When using HCX over the public internet, the cloud-side appliances will use public EIPs provided by AWS. There are 2 of these allocated by default for the WAN Interconnect/Extension appliances. If additional appliances are required by the customer, then they must contact VMware via live-chat and request additional EIPs be allocated to their SDDC for HCX. Once these have been allocated then the customer may continue with the deployment of the additional appliances.



---
## vMotion
---

### Virtual Machine Restrictions for vMotion
The following are summarized from the HCX user guide. Please refer to that document for details.
* must be hardware version 9+
* underlying architecture, regardless of OS, must be x86
* disk size cannot exceed 2 TB
* cannot have shared VMDK files
* cannot have any attached virtual media or ISOs



---
[Top](./README.md)
