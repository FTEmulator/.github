# Requisitos
- Ubuntu Server 24.04  
- Mínimo 2 GB de RAM  
- Al menos 2 núcleos de CPU  
- Conexión a Internet

# Instalar Docker
- ref: https://docs.docker.com/engine/install/ubuntu/

Tendremos que ir al apartado de "Install using apt repository": https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

Después de seguir la guía y haber instalado Docker:  
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Y para comprobar la versión y que esté todo bien:  
```bash
sudo docker version
```

# Instalar cri-docked  
Es un adaptador que permite utilizar Docker en Kubernetes  
- ref: https://mirantis.github.io/cri-dockerd/usage/install/

Después de instalarlo:  
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now cri-docker.socket
```

# Instalación de herramientas de Kubernetes  
- ref: https://kubernetes.io/docs/tasks/tools/

**kubectl**: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/  
**kind**: https://kind.sigs.k8s.io/docs/user/quick-start/#installation  
**minikube**: https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download  
**kubeadm y kubelet**: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

# Creación del cluster de Kubernetes  
- ref: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

sudo kubeadm init      --apiserver-advertise-address=192.168.1.200      --pod-network-cidr=192.168.0.0/16      --cri-socket=unix:///var/run/cri-dockerd.sock
```

# Archivo de conexión  
Lo que haremos a continuación es indicarle a kubectl qué archivo de configuración debe usar para conectarse al cluster.

- Usuario no root:  
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

- Usuario root:  
```bash
export KUBECONFIG=/etc/kubernetes/admin.conf
```

# Instalar Calico  
Calico es un plugin de red el cual usaremos para que los pods se puedan comunicar.

- ref: https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises

```bash
curl https://raw.githubusercontent.com/projectcalico/calico/v3.30.0/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

## Abrir puertos necesarios del firewall  
```bash
sudo ufw allow 6443/tcp                 # API server
sudo ufw allow 2379:2380/tcp            # etcd
sudo ufw allow 10250/tcp                # kubelet
sudo ufw allow 10259/tcp                # kube-scheduler
sudo ufw allow 10257/tcp                # kube-controller-manager
sudo ufw allow 179/tcp                  # Calico BGP
sudo ufw allow 4789/udp                 # Calico VXLAN
sudo ufw allow from 192.168.0.0/16
```

## Aplicar cambios del firewall  
```bash
sudo ufw reload
```

## Verificar si está todo correcto  
```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

# ¡¡¡ IMPORTANTE !!!  
En caso de hacer algún fallo y querer repetir el cluster poner lo siguiente:  
```bash
kubeadm reset -f
rm -rf ~/.kube /etc/kubernetes /var/lib/etcd /var/lib/kubelet /etc/cni/net.d /var/lib/cni
systemctl restart kubelet
```
