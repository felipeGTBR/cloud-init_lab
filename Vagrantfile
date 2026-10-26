Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "servidor-devops"
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.network "forwarded_port",
    guest: 80, host: 8080, auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 1
  end

  # Transfere o arquivo user-data para a VM
  config.vm.provision "file", source: "user-data", destination: "/tmp/user-data"

  if File.exist?("ssh_key.pub")
    ssh_key = File.read("ssh_key.pub").strip
  else
    abort "Erro: O arquivo ssh_key.pub não foi encontrado. Por favor, gere uma chave SSH e coloque o arquivo ssh_key.pub na pasta do projeto."
  end
  # 3. Substitui o marcador "CHAVE_SSH_AQUI" pela chave real dentro da VM antes do cloud-init rodar
  config.vm.provision "shell", inline: <<-SHELL
    sed -i "s|CHAVE_SSH_AQUI|#{ssh_key}|g" /tmp/user-data
  SHELL

    config.vm.provision "shell", path: "cloud-init_setup.sh"
end
