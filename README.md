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
