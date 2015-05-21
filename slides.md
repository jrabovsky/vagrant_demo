layout: true
class: middle

---

background-image: url(http://upload.wikimedia.org/wikipedia/commons/8/87/Vagrant.png)

---

# Vagrant?

--

A command line tool for creating, managing, and distributing portable portable sandbox environments.

---

Zero to VM in seconds

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

Use a simple DSL to describe your VMs

```ruby
  Vagrant.configure 2 do |config|
    config.vm.box = "ubuntu/trusty64"
  end
```

---

# Why bother?

You get "The Cloud"
but on your machine

---

* Self service
* Quick provisioning
* Cost effective
* Flexible

---

# Benefits

---

## Infrastructure
* Disposable and isolated test environments
* Repeatability
* Fast feedback

---

## Developers
* Isolate dependancies
* Consistent dev environments, no more "works on my machine" bugs
* Get new team members setup quickly

---

background-image: url(addition.png)

---

## Why not just use VirtualBox/Vmware/Hyper-V?

--

## Workflow

---

background-image: url(ui.png)

---

## Or with Vagrant:

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

### With Vagrant your environment is:

--

* Reproducible

--

* Versionable

--

* Sharable
