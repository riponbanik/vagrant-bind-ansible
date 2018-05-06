# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.ssh.insert_key = false

  # Cache
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
  end

  # Landrush DNS
  if Vagrant.has_plugin?("landrush")   
      # config.landrush_ip.auto_install = true      
      config.landrush.enabled = true            
      config.landrush.guest_redirect_dns = false
      # config.landrush.host_redirect_dns= false
      # config.landrush.upstream '127.0.0.1', 53, :tcp
  end
  
  # set host dns
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]      
  end 

  config.vm.define "ns01" do |machine|
    machine.vm.hostname = "ns01.lab.local"
    machine.vm.network "private_network", ip: "192.168.56.27"        
  end

  config.vm.define "ns02" do |machine|    
    machine.vm.hostname = "ns02.lab.local"
    machine.vm.network "private_network", ip: "192.168.56.28"        
  end

  # Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "vv"
    ansible.playbook = "provisioning/playbook.yml"
    ansible.become = true        
  end

  # Enable Password Auth  
  config.vm.provision "auth", type: "shell", inline: <<-EOC
    sudo sed -i -e "\\#PasswordAuthentication no# s#PasswordAuthentication no#PasswordAuthentication yes#g" /etc/ssh/sshd_config
    sudo service sshd restart
  EOC

  # Shell
  config.vm.provision "shell", inline: <<-SHELL
    sudo yum -y install nano net-tools bind-utils   
    grep -q -F 'NM_CONTROLED=yes' /etc/sysconfig/network-scripts/ifcfg-eth0 || (echo 'NM_CONTROLED=yes' | sudo tee -a /etc/sysconfig/network-scripts/ifcfg-eth0)
    grep -q -F 'PEERDNS=no' /etc/sysconfig/network-scripts/ifcfg-eth0 || (echo 'PEERDNS=no' | sudo tee -a /etc/sysconfig/network-scripts/ifcfg-eth0)
    sudo nmcli con mod "System eth0" ipv4.dns "192.168.56.13 10.0.2.3"    
    sudo sed -i "s/127\.0\.0\.1/10.0.2.15/g" /etc/hosts
    sudo systemctl restart NetworkManager.service
  SHELL

end
