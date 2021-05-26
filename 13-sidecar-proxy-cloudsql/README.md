### Kubernetes   
---
   
### SIDE CAR PATTERN
* https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine
### GITLAB CONTAINER REGISTRY K8S
* https://vix.digital/blog/technology/how-get-kubernetes-pulling-private-gitlab-container-registry/
* https://chris-vermeulen.com/using-gitlab-registry-with-kubernetes/
* https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
### Google SQL connect
* https://cloud.google.com/sdk/gcloud/reference/sql/connect
---
### Secret for Gitlab on K8s
```
kubectl create secret docker-registry regcredgitlab --docker-server=https://<DNS_REGISTRY_CONTAINER_PATH> \
--docker-username=<EMAIL> --docker-password=<PASS> --docker-email=<EMAIL>
```

```
sudo apt-get install xclip
cat file.txt | xclip -selection clipboard
paste...

ex: cat db-password.md | base64 --decode | xclip -selection clipboard
```

### BASE64 TERMINAL
* https://askubuntu.com/questions/178521/how-can-i-decode-a-base64-string-from-the-command-line
### MANAGING USERS DEBIAN TERMINAL
* https://devconnected.com/how-to-add-and-delete-users-on-debian-10-buster/
```
git checkout --track origin/newsletter
```