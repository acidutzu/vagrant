Vagrant.configure("2") do |config|
    config.vm.box = "generic/alpine318"
  
    # Forward all ports from 1 to 65535
    # (8000..9999).each do |port|
    config.vm.network "forwarded_port", guest: 5000, host: 5000
    config.vm.network "private_network", type: "dhcp"
  
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  
    # Provisioning section
    config.vm.provision "shell", inline: <<-SHELL
      apk update > /var/log/provision.log 2>&1
      apk add --no-cache python3 py3-pip net-tools netcat-openbsd openssh-client busybox-extras tmux socat git vim nano curl wget bind-tools jq zip tar mc >> /var/log/provision.log 2>&1
  
      # Install Docker
      apk add --no-cache docker
      rc-update add docker boot
      service docker start
  
      # Install Helm
      curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
      chmod +x get_helm.sh
      ./get_helm.sh
  
      # Add public key for Alpine
    #   wget -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
  
      # Install kubectl from the official Kubernetes repository
      curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
      mv kubectl /usr/local/bin/kubectl
      chmod +x /usr/local/bin/kubectl
    SHELL
  end
  