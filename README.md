# CGLabs AVD Image - Labs #

## Notes ##

Verify VNET/Hosts has access to the following endpoints. <https://learn.microsoft.com/en-us/azure/virtual-desktop/safe-url-list?tabs=azure#session-host-virtual-machines>. Eventually I will include the Networking components as part of PoC.. But many large organizations treat networking as a seperate component from other infrastructure and thus I'm following that practice as well.  

## Resource Organization ##

The structure of your resource storage directly determines your options for implementing resource management and governance.

- Subscriptions
- Reesource Groups
- Landing Zones (collection of Subscriptions under a common MG)

### Things to Consider ###

- Number of Virtual Machines that will be required.
- Refrain from deploying more than 5000 VMs in a single Region and Subscription. If you require more desktops, then span multiple Subscriptions in the same Region.
- Deploy Hosts into HostPool in batches of no more than 100 VMs (I recommend 50) to avoid API Throttling and potential timeout/deploy errors.
- Resources and Regions should align. Keep things in the same Region
  - Metadata (ARM/API Deployment)
  - AVD Resources: Host Pools, Applications Groups, and Workspaces stay in region with Hosts and Network
  - Session Hosts (VMs) bound to Region with vNET
  - vNets are bound to a specific Region
  - Storage (goes without saying doesn't it) especially if you are using Private Endpoints!
  
Resources like Keyvault, Compute Gallery, Images, etc. Do not need to follow the Region. Just be sure to configure the Compute Gallery to replicate the Image to each target region (configured via IaC\Packer)

### Naming and Tagging ###