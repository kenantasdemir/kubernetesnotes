# Authentication

kubernetes kullanıcı oluşturma ve kullanıcı doğrulama işlemlerini kendi üzerinde yaptırmaz
bunu dışarıda halledebileceğiniz seçenekler olarak sunar.


Kubernetes authetication

Kubernetes, kimlik doğrulama eklentileri aracılığıyla API isteklerinin 
kimliğini doğrulamak için istemci sertifikaları, taşıyıcı belirteçleri, bir
kimlik doğrulama proxy'si veya HTTP temel kimlik doğrulaması kullanır.


Api'de bir kullanıcı objesi bulunmamasına rağmen, cluster'in certificate 
authority'si tarafından imzalanmış geçerli bir sertifika sunan herhangi bir
kullanici (CA) kimliği doğrulanmış olarak kabul edilir. Bu yapılandırmada 
Kubernetes, sertifikanın konu' kısmındaki ortak ad alanından kullanıcı adını
belirler (ör. "/CN=bob").

Oradan, rol tabanlı erişim denetimi (RBAC) alt sistemi, kullanıcının bir 
kaynak üzerinde belirli bir işlemi gerçekleştirme yetkisine sahip olup 
olmadığını belirler.

X509 Client Certs
Static Token File
OpenID Connect Tokens
Webhook Token Authentication 
Authenticating Proxy


terminal1
mkdir minikubecrt
cd minikubecrt

openssl genrsa -out keyadi.key 2048
//private key oluşturduk

openssl req -new -key keyadi.key -out keyadi.csr -subj "/CN=kenant42@hotmail.com/O=DevTeam"
authentication klasörü altındaki README.md dosyasındaki adımları uygula

kubectl config set-credentials kenant42@hotmail.com --client-certificate=ozgurozturk.crt --client-key=keyadi.key
kubectl config set-context ozgurozturk-context --cluster=minikube --user=kenant42@hotmail.com

kubectl config use-context ozgurozturk-context



terminal2
cd git/minikubecrt
kubectl get nodes
kubectl get csr

kubectl certificate approve ozgurozturk
kubectl get csr ozgurozturk -o yaml











