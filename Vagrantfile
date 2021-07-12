Vagrant.configure("2") do |config|

  vms = [
    { :name => "titan", :ip => "192.168.15.101", :playbook => "web-setup.yml", :group => "web" },
    { :name => "enceladus", :ip => "192.168.15.102", :playbook => "db-setup.yml", :group => "db" }
  ]

  vms.each do |info|

    name, ip, playbook, group = info.values_at(:name, :ip, :playbook)

    config.vm.define name, :primary => name == "titan" do |vconf|
      vconf.vm.box = "centos/8"
      vconf.vm.network "private_network", ip: ip
      vconf.vm.hostname = "#{name}.local"

      vconf.vm.provider :virtualvconf do |v|
        v.name = name
        v.memory = 512
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
      end

      vconf.vm.synced_folder "share/#{name}", "/vagrant", type: "nfs"

      vconf.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioners/ansible/#{playbook}"
        ansible.groups = vms.each_with_object(Hash.new { |h,k| h[k] = [] }) do |vm, o|
          o[vm[:group]] << vm[:name]
        end
      end
    end

  end

end