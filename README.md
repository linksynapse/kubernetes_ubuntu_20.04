# kubernetes_ubuntu_20.04

##Step 1: Server update
  sudo apt update
  sudo apt -y full-upgrade
  sudo reboot

##Step 2: Install kubelet, kubeadm, kubectl
Install package & check
  sudo apt -y install curl apt-transport-https
  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
  echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
  sudo apt update
  sudo apt -y install vim git curl wget kubelet kubeadm kubectl
  sudo apt-mark hold kubelet kubeadm kubectl
  kubectl version --client && kubeadm version
  
##Step 3: Disable Swap
  sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
  sudo swapoff -a
  
  sudo modprobe overlay
  sudo modprobe br_netfilter
  
  sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
  net.bridge.bridge-nf-call-ip6tables = 1
  net.bridge.bridge-nf-call-iptables = 1
  net.ipv4.ip_forward = 1
  EOF
  
  sudo sysctl --system
  
##Step 4: Uninstall old version (Optional)
  sudo apt-get remove docker docker-engine docker.io containerd runc
  
  
##Step 5: Set up the repository for Docker
  sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
  sudo mkdir -p /etc/apt/keyrings
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  
  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
##Step 6: Install Docker Engine
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
