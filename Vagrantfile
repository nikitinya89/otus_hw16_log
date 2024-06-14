Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

  ssh_pub_key = File.readlines("../id_rsa.pub").first.strip
  common_provision_script = <<-SHELL
    echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
    sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL

  config.vm.define "web" do |web|
    web.vm.hostname = "web"
    web.vm.network "private_network", ip: "192.168.56.10"
    
    web.vm.provider :virtualbox do |v|
      v.memory = 1512
      v.cpus = 2
    end
    
    web.vm.provision "shell", inline: common_provision_script
  end

  config.vm.define "log" do |log|
    log.vm.hostname = "log"
    log.vm.network "private_network", ip: "192.168.56.15"
    
    log.vm.provider :virtualbox do |v|
      v.memory = 1512
      v.cpus = 2
    end

    log.vm.provision "shell", inline: common_provision_script
  end

  config.vm.define "elk" do |elk|
    elk.vm.hostname = "elk"
    elk.vm.network "private_network", ip: "192.168.56.20"
    
    elk.vm.provider :virtualbox do |v|
      v.memory = 4096
      v.cpus = 2
    end

    elk.vm.provision "shell", inline: common_provision_script
  end
end
