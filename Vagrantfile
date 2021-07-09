Vagrant.configure("2") do |config|

  vms = [
    { :name => "titan", :ip => "192.168.15.101", :playbook => "web-setup.yml", :group => "web" },
    { :name => "enceladus", :ip => "192.168.15.102", :playbook => "db-setup.yml", :group => "db" }
  ]

  vms.each do |info|

    name, ip, playbook, group = info.values_at(:name, :ip, :playbook)

    config.vm.define name do |web|
      web.vm.box = "centos/8"
      web.vm.network "private_network", ip: ip
      web.vm.hostname = "#{name}.local"

      web.vm.synced_folder "share/#{name}", "/vagrant", type: "nfs"

      web.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioners/ansible/#{playbook}"
        ansible.groups = vms.each_with_object(Hash.new { |h,k| h[k] = [] }) do |vm, o|
          o[vm[:group]] << vm[:name]
        end
      end

    end

  end

end