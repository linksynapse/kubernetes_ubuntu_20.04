# Activate Master Node

## Initalize with kubeadm
    # initialize with pod network id
    sudo kubeadm init --pod-network-cidr=92.68.0.0/24
    
## Access kubernetes by kubectl
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    
    # Next time access at remotly using $HOME/.kube/config file
    # ex) get nodes
    kubectl --kubeconfig $HOME/.kube/config get nodes
    
## Show Token
    kubeadm token list
    
## Show Hash
    openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
    
    
# Join Master Node

## Join
    sudo kubeadm join <MASTER HOST>:<MASTER PORT> --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>
