
Vagrant.configure("2") do |config|

    config.vm.define :mynode do |node|
      node.vm.box = "centos7_5"
      node.ssh.insert_key = false
      node.vm.network "private_network", :ip => "192.168.110.100"
      node.vm.synced_folder ".", "/home/vagrant/sources"
      node.vm.provision "shell", inline: "nmcli connection reload; systemctl restart network.service"
      node.vm.provider "libvirt" do |v|
        v.management_network_address = "192.168.121.0/24"
        v.memory = 2048
        v.cpus = 2
        v.storage_pool_name = 'default'
      end
      
      node.vm.provision "ansible" do |ansible|
        ansible.sudo_user = "vagrant"
        ansible.playbook = "site.yml"
        ansible.inventory_path = "sites/servers"
        ansible.limit = "dev"
        ansible.tags = ["common", "docker", "python3", "virtualenv", "postgresql"]
        ansible.verbose = "vvv"
      end
    end
end
