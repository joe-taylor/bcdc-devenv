Vagrant.configure("2") do |config|

  config.vm.define "titan" do |web|
    web.vm.box = "centos/8"
    web.vm.network "private_network", ip: "192.168.15.101"
    web.vm.hostname = "titan.local"

    web.vm.synced_folder "share/titan", "/vagrant", type: "nfs"

    web.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioners/ansible/web-setup.yml"
      ansible.groups = {
        "web": ["titan"]
      }
    end
  end

  config.vm.define "enceladus" do |db|
    db.vm.box = "centos/8"
    db.vm.network "private_network", ip: "192.168.15.102"
    db.vm.hostname = "enceladus.local"

    db.vm.synced_folder "share/enceladus", "/vagrant", type: "nfs"    

    db.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioners/ansible/db-setup.yml"
      ansible.groups = {
        "db": ["enceladus"]
      }
    end
  end

end