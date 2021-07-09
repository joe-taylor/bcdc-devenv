Vagrant.configure("2") do |config|

  [
    { :name => "titan", :ip => "192.168.15.101", :playbook => "web-setup.yml" },
    { :name => "enceladus", :ip => "192.168.15.102", :playbook => "db-setup.yml" }

  ].each do |info|

    name, ip, playbook = info.values_at(:name, :ip, :playbook)

    config.vm.define name do |web|
      web.vm.box = "centos/8"
      web.vm.network "private_network", ip: ip
      web.vm.hostname = "#{name}.local"

      web.vm.synced_folder "share/#{name}", "/vagrant", type: "nfs"

      web.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioners/ansible/#{playbook}"
        ansible.groups = {
          "web": ["titan"],
          "db": ["enceladus"]
        }
      end
    end

  end

end