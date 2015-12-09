Vagrant.configure("2") do |config|

  use_sensu_enterprise = (ENV['SENSU_ENTERPRISE_USER'] && ENV['SENSU_ENTERPRISE_PASSWORD'])

  config.vm.provision 'ansible' do |ansible|

    sensu_groups = {
      "sensu_server_client" => ["sensu-server-client"],
      "sensu_client"        => ["sensu-client"]
    }

    if use_sensu_enterprise
      sensu_groups["sensu_enterprise"] = ["sensu-server"]
    else
      sensu_groups["sensu_server"] = ["sensu-server"]
    end

    ansible.playbook = 'vagrant/site.yml'
    ansible.sudo = true
    ansible.host_key_checking = false
    ansible.groups = sensu_groups
  end

  config.vm.define 'sensu-server-client' do |a|
    a.vm.box = "debian/jessie64"
    a.vm.network :private_network, ip: "192.168.60.2"
    a.vm.hostname = 'sensu-server-client'
    a.vm.synced_folder ".", "/vagrant", :disabled => true
  end

  config.vm.define 'sensu-server' do |a|
    a.vm.box = "debian/jessie64"
    a.vm.network :private_network, ip: "192.168.60.3"
    a.vm.hostname = 'sensu-server'
    a.vm.synced_folder ".", "/vagrant", :disabled => true

    a.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--memory", 2048] if use_sensu_enterprise
    end
  end

  config.vm.define 'sensu-client' do |a|
    a.vm.box = "debian/jessie64"
    a.vm.network :private_network, ip: "192.168.60.4"
    a.vm.hostname = 'sensu-client'
    a.vm.synced_folder ".", "/vagrant", :disabled => true
  end
end
