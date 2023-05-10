# -*- mode: ruby -*-
# vi: set ft=ruby  :

machines = {
  "minikube"   => {"memory" => "6144", "cpu" => "4", "ip" => "150", "image" => "centos/7"}
}

Vagrant.configure("2") do |config|
  # Toda a configuração do Vagrant é feita aqui. A configuração mais comum
  # opções estão documentadas e comentadas abaixo. Para uma referência completa,
  # consulte a documentação online em vagrantup.com.
  
  # Define o usuário padrão SSH
  #config.ssh.username = 'vagrant'
  #config.ssh.password = 'vagrant'
  #config.ssh.insert_key = false
  
  # Compartilhamento de pasta do HOST para VM
  # É previso instalar o plugin no HOST "vagrant plugin install vagrant-vbguest"
  # Se tiver algum problema é possível desistalar no HOST através de "vagrant plugin uninstall vagrant-vbguest"
  # Habilita instalação do vbguest na VM do virtualbox
  config.vbguest.installer_options = { allow_kernel_upgrade: true }
  # Configura o compartilhamento da pasta raiz do vagrant do HOST com a VM
  config.vm.synced_folder ".", "/home/vagrant/projeto"

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
	  # Todo ambiente virtual Vagrant requer uma imagem para construir.
      machine.vm.box = "#{conf["image"]}"
	  # Definindo o hostname da VM
      machine.vm.hostname = "#{name}"
	  # Crie uma rede privada, que permite acesso somente de host à máquina
      # usando um IP específico.
      machine.vm.network "private_network", ip: "192.168.56.#{conf["ip"]}"
      # Crie uma rede pública, que geralmente corresponda à rede em ponte.
      # As redes em ponte fazem com que a máquina apareça como outro dispositivo físico na
      # sua rede.
      # machine.vm.network "public_network", ip: "192.168.0.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
	    # Nome VM
        vb.name = "#{name}"
		# Define a memória RAM VM
        vb.memory = conf["memory"]
		# Defina a quantidade de CPUs VM
        vb.cpus = conf["cpu"]
      end
	  # Executa shell script inline
      machine.vm.provision "shell", inline: "hostnamectl set-hostname #{name}"
      config.vm.provision "shell", inline: <<-SHELL
      HOSTS=$(head -n7 /etc/hosts)
      echo -e "$HOSTS" > /etc/hosts
      echo '192.168.0.#{conf["ip"]} #{name}' >> /etc/hosts
      # Habilita acesso SSH através de senha
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
      SHELL
    end
  end
  # Executa shell script externo
  config.vm.provision "shell", path: "script.sh"
end