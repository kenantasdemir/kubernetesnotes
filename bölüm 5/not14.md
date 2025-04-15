terminal1
minikube delete
minikube start --driver=hyperv
kubectl get nodes

<b><mark>minikube addons list</mark></b><br>
//tüm addon nesnelerinin listesini görüntüler

<b><mark>minikube addons enable ingress</mark></b><br>

kubectl get namespaces
//tüm namepspacleri görüntüler.

<b><mark>kubectl get all -n ingress-ngix</mark></b><br>
//ingress-nginx isimli namespace içindeki tüm nesneleri görüntüler.

kubectl apply -f .\deploy.yaml
kubectl get deployments<br>
kubectl get svc<br>
//kubectl get service
<br>

kubectl apply -f .
kubectl get ingress
kubectl get ingress -w



