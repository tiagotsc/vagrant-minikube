# -*- mode: ruby -*-
# vi: set ft=ruby  :

machines = {
  "minikube"   => {"memory" => "6144", "cpu" => "4", "ip" => "150", "image" => "centos/7"}
}

Vagrant.configure("2") do |config|
  #config.ssh.username = 'vagrant'
  #config.ssh.password = 'vagrant'
  #config.ssh.insert_key = false

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}"
      machine.vm.network "public_network", ip: "192.168.0.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
      end
 	  machine.vm.provision "shell", inline: "hostnamectl set-hostname #{name}"
	  config.vm.provision "shell", inline: <<-SHELL
	  HOSTS=$(head -n7 /etc/hosts)
	  echo -e "$HOSTS" > /etc/hosts
	  echo '192.168.0.#{conf["ip"]} #{name}' >> /etc/hosts
	  # Habilita acesso SSH remoto
	  sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
	  systemctl restart sshd
	  SHELL
    end
  end
  config.vm.provision "shell", path: "script.sh"
end