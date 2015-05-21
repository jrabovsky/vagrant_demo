Vagrant.configure(2) do |config|
  config.vm.box = "mwrock/Windows2012R2"
  config.vm.box_download_insecure = true
  config.vm.communicator = "winrm"
  config.vm.network "forwarded_port", host: 3389, guest: 3389, auto_correct: true
  config.vm.network "forwarded_port", host: 9000, guest: 9000

  config.vm.provision "file", source: "config.ru", destination: "/tmp/config.ru"
  config.vm.provision "file", source: ".gemrc", destination: 'C:\Users\Administrator\.gemrc'
  config.vm.provision "shell", inline: "netsh advfirewall set allprofiles state off"
  config.vm.provision "shell", path: "choco_setup.ps1"
  config.vm.provision "shell", path: "ruby_setup.ps1"
end
