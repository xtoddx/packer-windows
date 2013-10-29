# Windows Templates for Packer

### Introduction

This repository contains Packer [templates][templates] to create
[Vagrant][vagrant] boxes running different versions of Windows.

This repository is a repurposing of some [VeeWee templates][veewee] templates to
be usable by Packer and introduce VMWare Fusion.

### Workflow

In general, there are few steps to get started.

* Select which template file to use
* Download the appropraite version of Windows from MSDN or TechNet
* Run `packer build TEMPLATE.json`

### Windows Versions

#### 2008 R2 SP1 Evaluation

The free (if you don't mind registering with your personal information) path to
evaluating Windows is to use the evaluation ISO provided for that purpose.

Download it at: http://technet.microsoft.com/en-us/evalcenter/ee175713.aspx

Make sure to copy it into
isos/7601.17514.101119-1850_x64fre_server_eval_en-us-GRMSXEVAL_EN_DVD.iso

#### 2008 R2 SP1 Full

Available for [MSDN][msdn] Subscribers.

#### 2012

Available for [MSDN][msdn] Subscribers.

### How does this work?

Windows has an unattended installation mechanism that is invoked to begin the
installation process. This process is triggered by having an Autounattend.xml
file mounted as part of the setup process.

During this unattended installation:

* disks are configured
* windows is installed to the local disk
* after the system restarts, a user named "vagrant" is added as an admin
* the system is set to auto-login the vagrant user
* at the first sign-in, the vagrant user runs some scripts that are mounted into
  the instance
* WinRM is enabled (this will enable chef to run later), including firewall
  rules
* Usability settings are tweaked to enable a more powerful experience
* Install OpenSSH to get remote access to the machine, used to start chef

Afer that process completes, which packer will figure out by having the ability
to SSH into the machine, the "provisioners" step of the packer template runs.
This step runs a few batch files to continue setup:

* update virtualbox guest additions
* run chef
* add the vagrant public key for SSH

### Contributing

Pull requests welcomed.

[templates]: http://www.packer.io/docs/templates/introduction.html
[vagrant]: http://www.vagrantup.com/
[veewee]: https://github.com/jedi4ever/veewee/tree/master/templates
[msdn]: http://msdn.microsoft.com/
