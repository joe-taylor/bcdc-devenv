require 'yaml'

Vagrant.configure("2") do |config|

  ansible_vars = open("./provisioning/vars/main.yml", "r") { |f| YAML.load(f) }
  vm_size_gb = [10, ansible_vars["db_vm_size_gb"].to_i].max

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

      vconf.vm.provider "virtualbox" do |v|
        v.name = name

        # give the database more cpus and memory to,
        # among other things, speed up solr indexing
        if name == "enceladus"
          v.cpus = 2
          v.memory = 3072
        end
      end

      if name == "enceladus"
        vconf.disksize.size = "#{vm_size_gb}GB"
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