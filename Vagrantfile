Vagrant.configure("2") do |config|

  config.vm.define "titan" do |web|
    web.vm.box = "centos/7"
    web.vm.network "private_network", ip: "192.168.15.101"
    web.vm.hostname = "titan.local"
  end

  config.vm.define "enceladus" do |db|
    db.vm.box = "centos/7"
    db.vm.network "private_network", ip: "192.168.15.102"
    db.vm.hostname = "enceladus.local"
  end
  
end