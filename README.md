### Kubernetes   GCP-GKE AWS-EKS AZURE-AKS
* Project files from https://github.com/in28minutes
---

> Kubernetes gerencia recursos, servidores.   
> Na nuvem servidores são virtuais e são chamados": 1. AMAZON: EC2 2. Virtual machines 3. Google: Compute Engines;
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
* Deploy a partir da máquina local:
```
kubectl set image deployment hello-world-rest-api hello-world-rest-api=havyx/hello-world-rest-api --record
History of deplyments versions
kubectl rollout status deployment hello-world-rest-api
kubectl rollout undo deployment hello-world-rest-api --to-revision=1
kubectl rollout history deployment hello-world-rest-api
kubectl get pods -> kubectl logs POD_NAME -f
watch curl cloud_network_URL
```
* Get YAML FILE on Terminal
```
kubectl get deployment hello-world-rest-api -o yaml
kubectl get deployment hello-world-rest-api -o yaml > deployment.yaml,
kubectl get service hello-world-rest-api -o yaml
kubectl get service hello-world-rest-api -o yaml > sevice.yaml
```
---
#### Aplicar deployment, replicas:2  
```
kubectl apply -f deployment.yaml
kubectl delete all -l app=hello-world-rest-api
kubectl get all
```
---
#### Mostrar diferenças do seu arquivo yaml e o arquivo deployed.  
```
kubectl diff -f deployment-cleanup.yaml
```
## Kompose  
* Install: https://kompose.io/installation/
```
kompose convert
```
## Volumes  
#### kubectl get pv (Persistent volume, on clusters)
#### kubectl get pvc (persistent volume claim, access to the persistent volume)  
  
---
### Auto Scaling  
#### Cluster Auto Scaling  
```
gcloud container clusters create example-cluster \
--zone us-central1-a \
--node-locations us-central1-a,us-central1-b,us-central1-f \
--num-nodes 2 --enable-autoscaling --min-nodes 1 --max-nodes 4
```
#### Vertical Pod Auto Scaling  
- Available in version 1.14.7-gke.10 or higher and in 1.15.4-gke.15 or higher
```
gcloud container clusters create [CLUSTER_NAME] --enable-vertical-pod-autoscaling --cluster-version=1.14.7
gcloud container clusters update [CLUSTER-NAME] --enable-vertical-pod-autoscaling
```
* Configure VPA

```
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: currency-exchange-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       currency-exchange
  updatePolicy:
    updateMode: "Off"
```
* Get Recommendations off: recomendatios / auto: update pods
```
kubectl get vpa currency-exchange-vpa --output yaml
```  
#### HPA Horizontal Pod Auto Scaling  
```  
watch -n 0.1 curl http://INGRESS_IP/currency-exchange-cloud/from/USD/to/INR
kubectl top pods
kubectl autoscale deployment currency-exchange-cloud --min=1 max=3 --cpu-percent=70
kubectl get events
#Horizontal POD Auto-Scaling
kubectl hpa 
kubectl get hpa -o yaml
kubectl get hpa -o yaml > 01-hpa.yaml
kubectl get hpa currency-exchange -o yaml > 01-hpa.yaml
```
---
## Database Production Ready
* https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine?authuser=2#cloud-sql-auth-proxy-with-workload-identity
#### Secret  
```
padotec@PDO11869:~$ k create secret generic padotec-secret --from-literal=username=padotec-postgres --from-literal=password=iKiMhMC2iaBpogA1 --from-literal=database=padotec-postgres
secret/padotec-secret created
padotec@PDO11869:~$ k describe secret/padotec-secret
Name:         padotec-secret
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
database:  16 bytes
password:  16 bytes
username:  16 bytes
```
---
#### Workload Identity  
```
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ vim service_account.yaml
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ k apply -f service_account.yaml 
serviceaccount/ksa-cluster created
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ k get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.120.0.1   <none>        443/TCP   3h2m
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ k get all -o wide
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE    SELECTOR
service/kubernetes   ClusterIP   10.120.0.1   <none>        443/TCP   3h2m   <none>
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ k get serviceaccount
NAME          SECRETS   AGE
default       1         3h1m
ksa-cluster   1         27s
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ k get namespace
NAME              STATUS   AGE
default           Active   3h3m
kube-node-lease   Active   3h3m
kube-public       Active   3h3m
kube-system       Active   3h3m
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ gcloud iam service-accounts add-iam-policy-binding   --role roles/iam.workloadIdentityUser   --member "serviceAccount:euphoric-axiom-311918.svc.id.goog[cluster-1/ksa-cluster]"   padotec-app@euphoric-axiom-311918.iam.gserviceaccount.com
Updated IAM policy for serviceAccount [padotec-app@euphoric-axiom-311918.iam.gserviceaccount.com].
bindings:
- members:
  - serviceAccount:euphoric-axiom-311918.svc.id.goog[cluster-1/ksa-cluster]
  role: roles/iam.workloadIdentityUser
etag: BwXBhiUIa_M=
version: 1
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ kubectl annotate serviceaccount ksa-cluster iam.gke.io/gcp-service-account=padotec-app@euphoric-axiom-311918.iam.gserviceaccount.com
serviceaccount/ksa-cluster annotated
padotec@PDO11869:~/Github/Kubernetes_GPC-AWS-AZURE$ 

```