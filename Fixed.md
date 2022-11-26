# Fixed

## Get error kubectl after successfully install everything.
    #if you show message "the server dosen't have a resource type "something"
    #try check config to your $HOME/.kube/config file
    
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

## CGroup Memory Missing Issue
    sudo nano /boot/firmware/cmdline.txt
    
    # Add cgroup in kernel command file (saperated by space)
    cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
    
    sudo reboot
    
## Container runtime network not ready Issue
    # Install pod network add-on
    kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml
    
## When intialize using by kubeadm come out container runtime error
    # Remove config file
    sudo rm /etc/containerd/config.toml
    sudo systemctl restart containerd.service

## CoreDNS Stop Status on CreateContainer
    #Install Container Pod Network
    
    kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
    #Must be check init pod network id in kube-flannel.yml
    #When initalize kubeadm --pod-network-cidr=10.244.0.0/16
    
    
