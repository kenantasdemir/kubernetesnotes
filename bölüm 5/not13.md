Service account

kubectl config use-context minikube
kubectl apply -f serviceaccount.yaml

Kullanıcı hesapları insanlar içindir.
<b><mark>service accounts podlarda çalışan processler tarafından kullanılmak üzere tasarlanmıştır.</mark></b><br>


kubectl get sa
//serivceaccount objelerini görüntüler.

<b><mark>kubernetes her namespace için bir adet default isimli service account oluşturulur.</mark></b><br>
<b><mark>ve her poda aksi belirtilmedikçe bu service account bağlanır. hiçbir yetkisi yoktur.</mark></b><br>
<b><mark>istersek bunlara role ya da clusterrole bind ederek gerekli yetkiyi verebiliriz.</mark></b><br>

kubectl get pods
kubectl exec -it testpod -- bash
cd /var/run/secrets/kubernetes.io/serviceaccount/


oluşturulan serviceaccount için bir secret oluşturulur bu secretta 3 bilgi bulunur
<ul>
  <li>serviceaccountun oluşturulduğu namespace adı</li>
  <li>sertifika bilgisi</li>
  <li>kimlik doğrulaması için JWT bilgisi</li>
</ul>

her kubernetes cluster kurulumunda kubernetes içerisinden api servera erişilebilsin diye
kubernetes isimli bir service yaratılır

curl --insecure https://kubernetes


TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
curl --insecure https://kubernetes --header "Authorization:Bearer $TOKEN"
curl --insecure https://kubernetes/api/v1/namespaces/default/pods --header "Authorization:Bearer $TOKEN"

curl --insecure https://kubernetes/api/v1/namespaces/default/pods --header "Authorization:Bearer $TOKEN" | jq '.items[].metadata.name'


terminal2
kubectl get svc

her servis başına bir load balancer

************************************************************************

Ingress controller L7 Application Loadbalancer kavramının Kubernetes spesifikasyonlarına göre çalışan ve Kubernetese deploy ederek kullanabildiğimiz türüdür. Nginx, HAproxy, Traefik en bilinen ingress controller uygulamalarıdır.

Ingress
Genellikle HTTP olmak üzere bir clusterdaki servislere harici erişimi yöneten bir API nesnesidir.

Yük dengeleme, SSL sonlandırması ve path-name tabanlı yönlendirme özelliklerini destekler.






