# Disaster Recovery
The term "disaster recovery" is somewhat generic, and may mean different things to different people. For the purposes of discussion, we will define it as the ability to recreate the workloads of a production data center in a backup site.


### Disaster Recovery Services
Currently, VMware offers two services for configuring an SDDC as a DR site: Site Recovery and HCX.

[Site Recovery](https://www.vmware.com/products/site-recovery-manager.html) is a specialized tool which has been designed specifically for disaster recovery.

[HCX](https://hcx.vmware.com) is a tool which has been designed specifically for workload migration between sites.



### Preparation and Initial Setup
Disaster recovery in VMware Cloud on AWS may be accomplished using either the HCX or the Site Recovery services.

1. Select and activate the service - As with all cloud services, both HCX and Site Recovery must be activated within the SDDC before they may be used.

2. Adjust security policy of the SDDC - For both HCX and Site Recovery, the on-premises components must be able to communicate with the cloud-side components.

3. Install critical services within the SDDC - It is a good practice to provide the SDDC local access to certain critical services such as DNS and Active Directory.

4. Develop a recovery plan - Once the DR site is online and disaster recovery services have been configured, the next step will be to develop a recovery plan.
    * What level of failures do I need to account for? Application-level, VM-level, or site-level?
    * Do VMs need to keep their IP addresses in the recovery site, or should their IP addresses change?
    * How will I trigger and execute a recovery plan?
    * What critical services must be updated as part of the recovery plan?
    * What portions of the recovery plan will be automated vs manual?
    * How will users access the recovery site?


    
#### [Top](./README.md) | [Back <- Workload On-Boarding](./05_workloadOnBoarding.md)