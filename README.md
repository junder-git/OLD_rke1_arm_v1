> === PI BACKEND SECTION ===

RKE INSTALL MUST BE ON UBUNTU-22.04 NOT RASPBERRY-OS:::

INSTALL DOCKER::: 

> DONT SNAP INSTALL DOCKER (((https://stackoverflow.com/questions/52526219/docker-mkdir-read-only-file-system)))
sudo groupadd docker
sudo usermod -aG docker pi
chmod +x rke_linux-arm64
"./rke_linux-arm64 up" but prerequisite create a simple cluster.yml with ssh login without pass by running ssh-keygen and ssh-copy-id -i /home/<user>/.ssh/id_rsa -p <port> <user>@<ssh_server>:::

INSTALL KUBECTL::: 

sudo snap install kubectl --classic
mkdir .kube
cp kube_config_cluster.yml .kube/config
(now can freely kubectl from sudo apt install kubectl)

INSTALL HELM PLUS CERT MANAGER AND THEN RANCHER BOTH THROUGH HELM::: 

(https://cert-manager.io/docs/installation/helm/)
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
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
  --set hostname=rancher.junder.ddns.net \
  --set ingress.tls.source=letsEncrypt \
  --set letsEncrypt.email=james.witts.92@gmail.com \
  --set letsEncrypt.ingress.class=nginx

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

docker logs rancher 2>&1