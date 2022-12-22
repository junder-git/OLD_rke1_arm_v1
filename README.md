> === PI BACKEND SECTION ===

RKE INSTALL HAS BEEN DONE ON Ubuntu 20.10:::

INSTALL DOCKER::: 

> DONT SNAP INSTALL DOCKER (((https://stackoverflow.com/questions/52526219/docker-mkdir-read-only-file-system)))
sudo usermod -aG docker pi (((THEN RESTART PI)))
chmod +x rke_linux-arm64 (((COPY FILE ACROSS FIRST USING WINSCP WITH SIMPLE CLUSTER.YML TOO)))
"./rke_linux-arm64 up" but prerequisite with ssh login without pass by running ssh-keygen and ssh-copy-id -i /home/pi/.ssh/id_rsa -p 22 pi@192.168.50.89:::

INSTALL KUBECTL USE SNAP::: 
sudo snap install kubectl --classic
mkdir .kube
cp kube_config_cluster.yml .kube/config
(now can freely kubectl from sudo apt install kubectl)

INSTALL HELM PLUS CERT MANAGER AND THEN RANCHER BOTH THROUGH HELM USE SNAP::: 
(((https://helm.sh/docs/intro/install/)))
sudo snap install helm --classic
helm repo add jetstack https://charts.jetstack.io
helm repo update
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.10.1/cert-manager.crds.yaml

helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.10.1 \
  # --set installCRDs=true 

RANCHER INSTALL:::

helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
helm repo update
helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --create-namespace \
  --set hostname=rancher.junder.ddns.net \
  --set ingress.tls.source=letsEncrypt \
  --set letsEncrypt.email=james.witts.92@gmail.com \
  --set letsEncrypt.ingress.class=nginx \
  --set replicas=1

kubectl -n cattle-system rollout status deploy/rancher

UNINSTALL:::

./rke remove
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker system prune
docker volume prune



> === PI USEFUL COMMANDS ===

kubectl apply -f paintapp || kubectl delete -f paintapp

kubectl logs deployment/paintapp # logs of deployment
kubectl logs -f deployment/paintapp # follow logs

kubectl rollout restart deployments/paintapp

kubectl exec -it deployment/paintapp -- /bin/sh

kubectl get service/nginx-service -o jsonpath='{.spec.clusterIP}'

curl <clusterip>||<ingressip>

kubectl describe ing nginx-junder

kubectl get ingress

kubectl cluster-info

curl -vLk https://127.0.0.1

kubectl get endpoints

>=== DNS === 

(((https://www.suse.com/support/kb/doc/?id=000020174)))

