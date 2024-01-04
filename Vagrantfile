servers = [
  {
    :name => "db",
    :type => "server",
    :box => "generic/rhel9",
    :eth1 => "192.168.56.12",
    :mem => "4096",
    :cpu => "2"
  },
  {
    :name => "pah",
    :type => "server",
    :box => "generic/rhel9",
    :eth1 => "192.168.56.11",
    :mem => "8192",
    :cpu => "2"
  },
  {
    :name => "hybrid",
    :type => "server",
    :box => "generic/rhel9",
    :eth1 => "192.168.56.10",
    :mem => "8192",
    :cpu => "2"
  }
]

Vagrant.configure("2") do |main_config|
  servers.each do |opts|
    main_config.vm.define opts[:name] do |config|
      config.vm.box = opts[:box]
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:eth1]

      # RHSM Login Information
      if Vagrant.has_plugin?("vagrant-registration")
        config.registration.username = ENV["SUB_USERNAME"]
        config.registration.password = ENV["SUB_PASSWORD"]
      end

      config.vm.provider "virtualbox" do |v|
        v.name = opts[:name]
        v.customize ["modifyvm", :id, "--groups", "/AAP Lab"]
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end

      # Ansible provisioning block
      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.extra_vars = {
          eth1_ip: opts[:eth1],
          server_type: opts[:type],
          root_password_hash: "$y$j9T$LW7X6afjthzqwdPVAjmid/$cOKer0RmUsC8mfWLuDsvYxyi18c1oKIz2s/Vyp2CrjC"
        }
      end

      if opts[:name] == "hybrid"
        # Allow passwordless SSH from hybrid VM
        config.vm.provision "shell", inline: "for server in hybrid pah db; do sshpass -p 'vagrant' ssh-copy-id root@$server; done"
        # Forward port 8443 to external to port 443 on the hybrid VM
        config.vm.network "forwarded_port", guest: 443, host: 8443, host_ip: "0.0.0.0"
      end

    end
  end
end