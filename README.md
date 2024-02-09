# Rancher Desktop playground

Installation and sample use of rancher desktop

## What you need

* Azure subscription - most likely part of your Visual Studio subscription
* Github account - free to signup if you don't have one
* some time to tinker

## Setup

Create a new Virtual Machine in azure. Pay attention to these settings:

* __Instance details__
  * __Security type__ standard
  * __Image__ Windows 11 Pro
  * __Size__ Standard_D4s_v4
* __Inbound port rules__
  * __Public inbound ports__ Allow selected ports
  * __Select inbound ports__ RDP (3389)

## Install

After the VM is created make a remote desktop connection and run these installations with powershell.

```powershell
# install chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
# install openssl
choco install openssl -y
# install git
choco install git -y
# install base64 to decrypt secrets
choco install base64 -y
# install rancher desktop
choco install rancher-desktop -y
```

## Reboot

After installing rancher desktop a reboot is needed.

## Final steps

Clone this repository:

```bash
git clone https://github.com/erictummers/rancher-desktop-playground.git
```

Start Rancher desktop with the shortcut on the desktop. Accept the defaults for kubernetes and runtime. Now install [ingress-nginx](ingress-nginx) and [argocd](argocd) and you're ready for playing.
