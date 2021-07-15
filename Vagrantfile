Vagrant.configure("2") do |config|

  vms = [
    { :name => "enceladus", :ip => "192.168.15.102", :playbook => "db-setup.yml", :group => "db" },
    { :name => "titan", :ip => "192.168.15.101", :playbook => "web-setup.yml", :group => "web" }
  ]

  config.vagrant.plugins = ["vagrant-disksize"]

  vms.each do |info|

    name, ip, playbook, group = info.values_at(:name, :ip, :playbook, :group)

    config.vm.define name, :primary => name == "titan" do |vconf|
      vconf.vm.box = "centos/8"
      vconf.vm.network "private_network", ip: ip
      vconf.vm.hostname = "#{name}.local"

      vconf.vm.provider :virtualvconf do |v|
        v.name = name
        v.memory = 4080
      end

      if name == "enceladus"
        vconf.disksize.size = '20GB'
      end

      # vconf.vm.synced_folder "share/#{name}", "/vagrant", type: "nfs"

      vconf.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/#{playbook}"
        ansible.groups = vms.each_with_object(Hash.new { |h,k| h[k] = [] }) do |vm, o|
          o[vm[:group]] << vm[:name]
        end

        ansible.galaxy_role_file = "provisioning/#{group}-ansible-requirements.yml"
      end
    end

  end

end