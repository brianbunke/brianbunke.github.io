---
layout:     post
title:      vCenter Simulator Docker Container
date:       2018-12-31 00:00:00 -0800
summary:    No vCenter, no problem
categories: blog
series:     vcsim
thumbnail:  docker
author:     brianbunke
tags:
 - powershell
 - powercli
 - vmware
 - vcsim
 - docker
---

One of the primary issues with developing tools focused on VMware is the lack of a readily available vCenter lab to test against. At times, I have had a functional and up-to-date [home lab], and right now it sits in neglectful disrepair. When you develop open-source tooling, this problem is uniquely inflicted upon every potential contributor.

The original incarnation of vCenter Simulator (VCSIM) was a useful tool to help bridge this gap, but it last emulated vCenter 5.5.

[govcsim] is a new version of VCSIM, written in the Go language (Golang), intended to mock the API endpoints presented by vCenter Server and ESXi. While it is written in Golang, once it's running, any language capable of interacting with an API can consume it. _(\*cough\* PowerShell)_

In addition, it can be packaged as a Docker container, which makes end-user consumption even easier. This walkthrough uses [nimmis/vcsim].

> PowerCLI can connect to a VCSIM Docker container and run commands against it.

I am using Docker Desktop on Windows 10 Enterprise. ([Install Docker for Windows]) Docker must be running in Linux mode, as VCSIM is a Linux-based container.

That said: Docker, PowerShell v6+ (pwsh), and PowerCLI v10+ are all cross-platform, so you can run and interact with VCSIM from pretty much whereever you want. (Consider this a teaser for my next blog post.)

This code will:

1. Run a new VCSIM container.
    - If you don't have the image locally, it will download from Docker Hub.
2. On Windows, get the local IP address of your DockerNAT network adapter.
    - tl;dr - On Win10, Hyper-V + Docker + NAT doesn't work the way you'd expect.
    - You will be connecting to a local IP address like `10.0.75.1`, _not_ localhost.
3. After starting the container, test that port 443 is responding.
    - The `-TCPPort` parameter was introduced in PowerShell v6.
    - If you're using Windows PowerShell and still want to test port connectivity, [see Stack OverFlow].
4. Use the [VMware.PowerCLI] PowerShell module to connect to vCenter Simulator and test drive it.

```powershell
docker run --rm -d -p 443:443/tcp nimmis/vcsim:latest

If ($IsMacOS -or $IsLinux) {
    $Server = 'localhost'
} Else {
    $Server = (Get-NetIPAddress -InterfaceAlias *docker* -AddressFamily IPv4).IPAddress
}

If ($PSVersionTable.PSVersion.Major -ge 6) {
    Test-Connection $Server -TCPPort 443
}

Connect-VIServer -Server $Server -Port 443 -User u -Password p
# Did it work?
Get-VM
# Or Get-VMHost, or Get-Datastore, etc.
```

The simulator currently supports a subset of API methods. Once the container is running, you can view the list of supported methods and types:

```powershell
$About = Invoke-RestMethod -Uri "https://$($Server):443/about" -Method Get
$About.Methods
$About.Types
```

For example, some of the methods include `PowerOffVM_Task`, `PowerOnMultiVM_Task`, and `PowerOnVM_Task`. From PowerCLI, try out your trusty `Stop-VM` and `Start-VM` commands _(after you've disconnected from your production environment)_!

If you'd like VCSIM to do something it currently doesn't support, open an issue on the open-source [govmomi] library.

Coming up: My next post will detail how you can incorporate this Docker image into an [Azure DevOps build pipeline], providing an avenue to run integration tests with your code in a CI pipeline without connecting to an existing vSphere environment.



[home lab]: https://twitter.com/brianbunke/status/619970996708093952

[govcsim]: https://github.com/vmware/govmomi/tree/master/vcsim
[nimmis/vcsim]: https://hub.docker.com/r/nimmis/vcsim

[Install Docker for Windows]: https://docs.docker.com/docker-for-windows/install/

[see Stack Overflow]: https://stackoverflow.com/questions/9566052/how-to-check-network-port-access-and-display-useful-message
[VMware.PowerCLI]: https://www.powershellgallery.com/packages/VMware.PowerCLI

[govmomi]: https://github.com/vmware/govmomi
[Azure DevOps build pipeline]: https://azure.microsoft.com/en-us/services/devops/pipelines/
