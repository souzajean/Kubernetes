# Criando nossa máquina com multipass com 2 kubernetes


# Instalando Microk8s

multipass launch -n k8s -c 2 -m 4G -d 20GB <br>
multipass exec k8s -- sudo snap install microk8s --classic --channel=1.25/stable <br>
multipass exec k8s -- sudo usermod -a -G microk8s ubuntu <br>
multipass exec k8s -- sudo chown -f -R ubuntu ~/.kube <br>
multipass restart k8s <br>
multipass exec k8s -- /snap/bin/microk8s.kubectl create deployment nginx --image=nginx <br>
multipass exec k8s -- /snap/bin/microk8s.kubectl get pods <br>
multipass exec k8s -- /snap/bin/microk8s.kubectl config view --raw <br>
multipass list #anotar ip adress

# Maquina local (Instalar também o Microk8s)
su - $USER<br>
microk8s status --wait-ready #Verificar status do serviço<br>
kubectl get pods<br>
kubectl get nodes

# Instalando nossa 2 máquina com Microk8s
multipass launch -n k8s2 -c 2 -m 4G -d 20GB<br>
multipass exec k8s2 -- sudo snap install microk8s --classic --channel=1.25/stable<br>
multipass exec k8s2 -- sudo usermod -a -G microk8s ubuntu <br>
multipass exec k8s2 -- sudo chown -f -R ubuntu ~/.kube<br>
multipass restart k8s2

# Maquina local
sudo iptables -P FORWARD ACCEPT <br>
multipass list

# Adicionando o node
multipass shell k8s <br>
multipass exec k8s -- sudo microk8s.add-node <br> 
microk8s add-node <br>
Copiar join

# Entrar no K8s2
multipass shell k8s2 <br>
colar join

# Maquina local
kubctl get nodes

# Criar minha aplicação
/snap/bin/microk8s.kubectl expose deployment nginx --type NodePort --port 80
/snap/bin/microk8s.kubectl get services
# Criar MicroK8s User
sudo usermod -a -g microk8s $USER <br>
sudo chown -f -R $USER ~/.kube <br>
su - $USER

# MicroK8s comandos
microk8s kubectl get nodes
microk8s kubectl get services
microk8s kubectl get all -all-namespace

# MicroK8s alias (~/.bash_aliases)
alias kubctl='microk8s kubectl'


# Instalação dos módulos do kernel
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
<br>
overlay <br>
br_netfilter <br>
EOF

# Configuração dos parâmetros do sysctl, fica mantido mesmo com reebot da máquina.
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf 
<br>
net.bridge.bridge-nf-call-iptables = 1 <br>
net.ipv4.ip_forward <br>
= 1 <br>
net.bridge.bridge-nf-call-ip6tables = 1 <br>
EOF

# Aplica as definições do sysctl sem reiniciar a máquina
sudo sysctl --system

# Instalação Containerd
sudo apt update && sudo apt install -y containerd

# Configuração padrão do Containerd
sudo mkdir -p /etc/containerd <br>
containerd config default | sudo tee /etc/containerd/config.toml

# Reiniciar o container
sudo systemctl restart containerd

# Atualizo os pacotes necessários pra instalação
sudo apt-get update && \ <br>
sudo apt-get install -y apt-transport-https ca-certificates curl

# Download da chave pública do Google Cloud
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# Adiciono o repositório apt do Kubernetes
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/

# Atualização do repositório apt e instalação das ferramentas
sudo apt-get update && \ <br>
sudo apt-get install -y kubelet kubeadm kubectl

# Agora eu garanto que eles não sejam atualizados automaticamente.
sudo apt-mark hold kubelet kubeadm kubectl