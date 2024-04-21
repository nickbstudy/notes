#### Building a swarm

Docker desktop will only create 1 container.  To create many, you can use Multipass (requires VirtualBox).  If using a windows installation give it file system access too using `

---

#### Troubleshooting multipass

- Can't access local file system:

Run this command: `multipass set local.privileged-mounts=Yes`

Only 10.x.x.x IP Addresses: 
 
> Unfortunately the default switch is known to be quirky and Windows updates often put it in a weird state. This may result in new instances failing to launch, and existing ones timing out to start.

> The broken state also persists over reboots. The one approach that has helped is removing the network sharing from the default switch and rebooting:

**In Powershell:**
`Get-HNSNetwork | ? Name -Like "Default Switch" | Remove-HNSNetwork`
`Restart-Computer`

> Hyper-V will recreate it on next boot.



---
