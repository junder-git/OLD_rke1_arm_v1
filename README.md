> === PI BACKEND SECTION ===

INSTALL:::

"./rke-arch64 up" but prerequisite create a simple cluster.yml with ssh login without pass by running ssh-keygen and ssh-copy-id -i /home/<user>/.ssh/id_rsa -p <port> <user>@<ssh_server>:::

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

kubectl logs --selector=run=paintapp --tail 1