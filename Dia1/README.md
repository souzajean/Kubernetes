# Criando nossa máquina com multipass com 2 kubernetes


#Instalando Microk8s

multipass launch -n k8s -c 2 -m 4G -d 20GB
multipass exec k8s -- sudo snap install microk8s --classic --channel=1.25/stable
multipass exec k8s -- sudo usermod -a -G microk8s ubuntu
multipass exec k8s -- sudo chown -f -R ubuntu ~/.kube
multipass restart k8s
multipass exec k8s -- /snap/bin/microk8s.kubectl create deployment nginx --image=nginx
multipass exec k8s -- /snap/bin/microk8s.kubectl get pods
multipass exec k8s -- /snap/bin/microk8s.kubectl config view --raw
multipass list #anotar ip adress


#Maquina local (Instalar também o Microk8s)
su - $USER
microk8s status --wait-ready #Verificar status do serviço
kubectl get pods
kubectl get nodes


#Instalando nossa 2 máquina com Microk8s
multipass launch -n k8s2 -c 2 -m 4G -d 20GB
multipass exec k8s2 -- sudo snap install microk8s --classic --channel=1.25/stable
multipass exec k8s2 -- sudo usermod -a -G microk8s ubuntu
multipass exec k8s2 -- sudo chown -f -R ubuntu ~/.kube
multipass restart k8s2

#Maquina local
sudo iptables -P FORWARD ACCEPT
multipass list

#Adicionando o node
multipass shell k8s
microk8s add-node
#Copiar join

#Entrar no K8s2
multipass shell k8s2
#colar join

#Maquina local
kubctl get nodes

#Configurando chaves ssh



ssh-keygen -t rsa -b 2048

cat ~/.ssh/id_rsa_k8s.pub
cat ~/.ssh/id_rsa_k8s2.pub











