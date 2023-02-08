# Kubernetes_cluster


# Installer containerd
   $ ./containerd.sh

# Installer kubeadm et ses acolytes: kubelet et kubectl
   $ ./kubeadm

----------------------------------MASTER-----------------------------------
# Créer le cluster Kubernetes
   $ sudo kubeadm init --pod-network-cidr=192.168.0.0/16

# Ici vous devez être connecté avec votre compte utilisateur et non pas en tant que `root`
   $ mkdir -p $HOME/.kube
   $ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   $ sudo chown $(id -u):$(id -g) $HOME/.kube/config

#  Deploy cni
   $ kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/tigera-operator.yaml

   $ curl https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/custom-resources.yaml -O

   $ kubectl create -f custom-resources.yaml

--------------------------------NODES----------------------------------------

# la commande à exécuter sur tous vos autres noeuds afin qu’ils rejoignent le cluster Kubernetes:
   $ sudo kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>

------------------------------------------------------------------------------

# vérifier que tout fonctionne
   $ kubectl cluster-info 