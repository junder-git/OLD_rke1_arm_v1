> === PI BACKEND SECTION ===

RKE INSTALL MUST BE ON UBUNTU-22.04 NOT RASPBERRY-OS:::

sudo snap install docker
sudo groupadd docker
sudo usermod -aG docker pi
chmod +x rke_linux-arm64
"./rke_linux-arm64 up" but prerequisite create a simple cluster.yml with ssh login without pass by running ssh-keygen and ssh-copy-id -i /home/<user>/.ssh/id_rsa -p <port> <user>@<ssh_server>:::

mkdir .kube
cp kube_config_cluster.yml .kube/config
(now can freely kubectl from sudo apt install kubectl)

helm install cert manager see... (https://cert-manager.io/docs/installation/helm/)

SKIP RANCHER INSTALL:::
helm install rancher rancher-stable/rancher   --namespace cattle-system --create-namespace  --set hostname=rancher.junder.ddns.net   --set ingress.tls.source=letsEncrypt   --set letsEncrypt.email=james.witts.92@gmail.com   --set letsEncrypt.environment=prod   --set replicas=1

UNINSTALL:::

./rke remove
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker system prune
docker volume prune

> === PI USEFUL COMMANDS ===

kubectl apply -f https://raw.githubusercontent.com/Jaynericguy/pirke/main/deployments.yml
kubectl apply -f https://raw.githubusercontent.com/Jaynericguy/pirke/main/services.yml
kubectl apply -f https://raw.githubusercontent.com/Jaynericguy/pirke/main/ingresses.yml

kubectl delete -f ./deployments.yml <CAN-APPLY-A-FOLDER-TOO>

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