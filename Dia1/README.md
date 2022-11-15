# Criando nossa máquina com multipass com 2 kubernetes


# Instalando Microk8s

multipass launch -n k8s -c 2 -m 4G -d 20GB<br>
multipass exec k8s -- sudo snap install microk8s --classic --channel=1.25/stable<br>
multipass exec k8s -- sudo usermod -a -G microk8s ubuntu<br>
multipass exec k8s -- sudo chown -f -R ubuntu ~/.kube<br>
multipass restart k8s<br>
multipass exec k8s -- /snap/bin/microk8s.kubectl create deployment nginx --image=nginx<br>
multipass exec k8s -- /snap/bin/microk8s.kubectl get pods<br>
multipass exec k8s -- /snap/bin/microk8s.kubectl config view --raw<br>
multipass list #anotar ip adress

# Maquina local (Instalar também o Microk8s)
su - $USER<br>
microk8s status --wait-ready #Verificar status do serviço<br>
kubectl get pods<br>
kubectl get nodes

# Instalando nossa 2 máquina com Microk8s
multipass launch -n k8s2 -c 2 -m 4G -d 20GB<br>
multipass exec k8s2 -- sudo snap install microk8s --classic --channel=1.25/stable<br>
multipass exec k8s2 -- sudo usermod -a -G microk8s ubuntu<br>
multipass exec k8s2 -- sudo chown -f -R ubuntu ~/.kube<br>
multipass restart k8s2

# Maquina local
sudo iptables -P FORWARD ACCEPT<br>
multipass list

# Adicionando o node
multipass shell k8s<br>
microk8s add-node<br>
Copiar join

# Entrar no K8s2
multipass shell k8s2<br>
colar join

# Maquina local
kubctl get nodes

# Instalação Kubeadm
cat << EOF | sudo tee /etc/modules-load.d/containerd.conf<br>
>overlay<br>
>br_netfilter<br>
>EOF