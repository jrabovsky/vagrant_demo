layout: true
class: middle

---

background-image: url(/vagrant_demo/Vagrant.png)

---

# Vagrant?

--

A command line tool for creating, managing, and distributing portable portable sandbox environments.

---

# Zero to VM in seconds

```bash
$ vagrant init ubuntu/trusty64
...
$ vagrant up
...
$ vagrant ssh
vagrant@vagrant-ubuntu-trusty-64:~$ echo hello world
hello world
```

---

# Use a simple DSL to describe your VMs

```ruby
  Vagrant.configure 2 do |config|
    config.vm.box = "ubuntu/trusty64"
  end
```

---

# Why bother?

--

### You get "The Cloud" but on your machine

--

* Self service
* Quick provisioning
* Cost effective
* Flexible

---

# Benefits

---

# Infrastructure
* Disposable and isolated test environments
* Repeatability
* Fast feedback

---

# Developers
* Isolate dependancies
* Consistent dev environments, no more "works on my machine" bugs
* Get new team members setup quickly

---

background-image: url(/vagrant_demo/addition.png)

---

## Why not just use VirtualBox/Vmware/Hyper-V?

--

# Workflow!

---

background-image: url(/vagrant_demo/ui.png)

---

# Or with Vagrant:

```ruby
Vagrant.configure(2) do |config|
  config.vm.define "zenoss" do |zenoss|
    zenoss.vm.box = "chef/centos-6.5"
    zenoss.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end
    zenoss.vm.network "private_network", ip: "192.168.50.2"
    zenoss.vm.provision "shell", path: "provisioner_scripts/zenoss.sh"
  end

  config.vm.define "win2008" do |win2008|
    win2008.vm.network "forwarded_port", host: 3389, guest: 3389
    win2008.vm.network "private_network", ip: "192.168.50.3"
    win2008.vm.box = "ferventcoder/win2008r2-x64-nocm"
    win2008.vm.provision "shell", path: "provisioner_scripts/win2008.ps1"
  end

  config.vm.define "web" do |web|
    config.vm.box = "ubuntu/trusty64"
    web.vm.network "private_network", ip: "192.168.50.4"
    web.vm.provision "shell", path: "provisioner_scripts/web.sh"
  end
end
```

---

## With Vagrant your environment is
* Reproducible
* Versionable
* Sharable

---

# Installation

* https://www.virtualbox.org/wiki/Downloads
* http://www.vagrantup.com/downloads

--

or use [Chocolatey](https://chocolatey.org/)

```bash
$ choco install virtualbox
$ choco install virtualbox.extensionpack
$ choco install vagrant
```

---

# How about a Windows VM?

```bash
$ vagrant init -m mwrock/Windows2012R2
$ cat Vagrantfile
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
end
$ vagrant up
...
$ vagrant rdp
...
```

---

# Cryptic SSL error?

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
end
```

---

```bash
$ vagrant up
==> default: Importing base box 'mwrock/Windows2012R2'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'mwrock/Windows2012R2' is up to date...
==> default: Setting the name of the VM: vagrant_demo_default_1432221976130_90128
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 => 2222 (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
    default: Warning: Connection timeout. Retrying...
```

---

background-image: url(/vagrant_demo/Windows-vs-Linux.jpg)

---

# Tell Vagrant to use WinRM

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
end
```

---

```bash
$ vagrant up
...
$ vagrant rdp
==> default: Detecting RDP info...
RDP connection information for this machine could not be
detected. This is typically caused when we can't find the IP
or port to connect to for RDP. Please verify you're forwarding
an RDP port and that your machine is accessible.
```

---

# Port forwarding

Access a port on your own machine, but actually have traffic go to a port on your VM

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
  config.vm.network "forwarded_port", host: 3389, guest: 3389, auto_correct: true
end
```

---

## By convention, the user name and password are...

.center[![rdp](/vagrant_demo/rdp.png)]

---

# Synced Folders

```bash
$ ls


    Directory: C:\Users\jrabovsky\git_repos\vagrant_demo


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        05/21/2015  10:12 AM            .vagrant
-a---        05/20/2015   4:17 PM     187455 addition.png
-a---        05/20/2015   2:35 PM        924 index.html
-a---        05/21/2015  11:04 AM      35455 rdp.png
-a---        05/20/2015   1:55 PM         52 README.md
-a---        05/21/2015  11:12 AM       4898 slides.md
-a---        05/21/2015  11:11 AM      36477 synced_folders.png
-a---        05/20/2015   6:03 PM     512982 ui.png
-a---        05/21/2015   9:14 AM     528977 Vagrant.png
-a---        05/21/2015  10:57 AM        242 Vagrantfile
-a---        05/21/2015  10:40 AM      97742 Windows-vs-Linux.jpg
```

---

background-image: url(/vagrant_demo/synced_folders.png)

---

# Provisioning

--

* File
* Shell (Bash, Batch, PowerShell)


* Ansible
* CFEngine
* Chef Solo
* Chef Zero
* Chef Client
* Chef Apply
* Docker
* Puppet Apply
* Puppet Agent
* Salt

---

# File provisioner
Copy a file from the host machine to the guest machine

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
  config.vm.network "forwarded_port", host: 3389, guest: 3389, auto_correct: true

  config.vm.provision "file", source: "config.ru", destination: "/tmp/config.ru"
end
```

---

background-image: url(/vagrant_demo/file.png)

---

# Shell provisioner
Execute a script on the guest machine

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
  config.vm.network "forwarded_port", host: 3389, guest: 3389, auto_correct: true

  config.vm.provision "file", source: "config.ru", destination: "/tmp/config.ru"
  config.vm.provision "shell",
    inline: "echo 'vagrant is awesome' > /tmp/hello.txt"
end
```

*Windows inline shell scripts an only be PowerShell*

---

background-image: url(/vagrant_demo/shell.png)

---

# Or use an external script

```bash
# choco_setup.ps1

iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
choco feature enable -n allowGlobalConfirmation
```

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
  config.vm.network "forwarded_port", host: 3389, guest: 3389, auto_correct: true

  config.vm.provision "file", source: "config.ru", destination: "/tmp/config.ru"
  config.vm.provision "shell", path: "choco_setup.ps1"
end
```

---

## Setup our Ruby app

```bash
# ruby_setup.ps1

netsh advfirewall set allprofiles state off
choco install ruby
C:\tools\ruby21\bin\gem install rack
```

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
  config.vm.network "forwarded_port", host: 3389, guest: 3389, auto_correct: true
  config.vm.network "forwarded_port", host: 9000, guest: 9000

  config.vm.provision "file", source: "config.ru", destination: "/tmp/config.ru"
  config.vm.provision "file", source: ".gemrc",
    destination: 'C:\Users\Administrator\.gemrc'
  config.vm.provision "shell", path: "choco_setup.ps1"
  config.vm.provision "shell", path: "ruby_setup.ps1"
end
```

```bash
$ vagrant rdp
...
$ rackup -p 9000 -o 0.0.0.0 /tmp/config.ru
```

---

# From the host machine...

.center[![web_server](/vagrant_demo/web_server.png)]

---

# But wait there's more!
https://docs.vagrantup.com/v2/

--

* Custom boxes
* Networking
* Multi-Machine
* Other provisioners
* Other providers
* Plugins
