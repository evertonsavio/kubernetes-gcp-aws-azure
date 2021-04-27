### Kubernetes   GCP-GKE AWS-EKS AZURE-AKS
---
* Criar conta: https://cloud.google.com/  

> Kubernetes gerencia recursos, servidores. 
> Na nuvem esses servidores são "virtual servers e são chamados": 1. AMAZON: EC2 2. Virtual machines 3. Google: Compute Engines;
> Kubernetes chama eles de NODES.
> Para gerenciar multiplos Worker Nodes, você precisa de manages, os Master Node(s).
---
* No google console, ativar Kubernetes Engine Api e criar um novo Cluster.
* Ativar Google Cloud Shell na aba superior direita.
* Na aba cluster ativar _CONNECT copie o comando e cole no shell
#### No client Shell:
```
kubectl version
kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
```
* Ao executar = kubectl create deployment... -> São criadas as entidades = deployment, replicaset & pod
* kubectl expose deployment... -> service
---
#### Resizing Clusters
```
gcloud container clusters resize --zone <name_of_zone> <name_of_your_cluster> --num-nodes=0
gcloud container clusters resize --zone <name_of_zone> <name_of_your_cluster> --num-nodes=3
```
---
#### Numero de Containers em um POD e IP do POD (kubectl explain pods)
* 1/1 significa que há um containers nesse pod. no pod IP 10.68.0.6, todos os containers dentro do pod podem user localhost.
```
kubectl get pods -i wide
hello-world-rest-api-6d5479ddb6-vbxk5   1/1     Running   0          33m   10.68.0.6   gke-cluster-padotec-default-pool-b9f34c8c-z3s8   <none>           <none>
kubectl describe pod hello-world-rest-api-6d5479ddb6-vbxk5
```
---
### Installing GCloud & Kubectl https://cloud.google.com/sdk/docs/install#deb
#### GCloud
```
sudo apt-get install kubectl
gcloud init
gcloud auth login
```
#### kubectl https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(<kubectl.sha256) kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```
* Para conectar com o Cluster, vá no google console e na aba console clicar em connect, copy link.
