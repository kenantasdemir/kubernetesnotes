Persistent Volume

Kubernetes, verilerin kalıcı bir şekilde depolanması için Persistent Volume 
(PV) ve Persistent Volume Claim (PVC) kavramlarını kullanır.

Persistent Volume (PV), Kubernetes kümesindeki bir depolama kaynağıdır. PV,
verilerin kalıcı olarak depolanması için fiziksel bir disk, bulut depolama 
veya ağa bağlı bir depolama kaynağı olabilir. PV, Kubernetes clusterındaki bir 
kaynak olarak tanımlanır ve diğer Kubernetes nesneleri, özellikle PVC'ler 
tarafından talep edilir.


Persistent Volume Claim (PVC), PV'ye talepte bulunmak için kullanılan bir 
objedir. PVC'ler, PV'lerin kapasitesini, erişim modunu ve diğer özelliklerini
tanımlar. PVC'ler, Kubernetes kullanıcıları tarafından oluşturulur ve 
Kubernetes tarafından otomatik olarak bir PV ile eşleştirilir. <b><mark>Bir PVC 
oluşturulduğunda, PVC, mevcut bir PV ile eşleşebilirse, mevcut PV'yi talep 
eder veya yoksa yeni bir PV oluşturur.</mark></b><br>


PV ve PVC, Kubernetes kümesindeki uygulamaların verilerini kalıcı olarak saklamak için kullanılır. Örneğin, bir web uygulaması için kullanılan veritabanı, bir PV'ye bağlanabilir ve uygulama yeniden başlatıldığında veya yeni bir pod oluşturulduğunda bile veriler korunabilir. Bu sayede, uygulama hizmetlerinin kesintiye uğramadan çalışmasına olanak sağlar.

bir volume direkt olarak pod ile eşlenemez
pvc, pvler arasında işimize uygun olab bir tanesini seçmemize imkan verir.


terminal1
docker volume create nfsvol
docker network create --driver=bridge --subnet=10.255.255.0/24 --ip-range=10.255.255.0/24 --gateway=10.255.255.10 nfsnet
docker run -dit --privileged --restart unless-stopped -e SHARED_DIRECTORY=/data -v nfsvol:/data --network nfsnet -p 2049:2049 --name nfssrv ozgurozturknet/nfs:latest

--privileged Seçeneğinin İşlevi
Docker container'ları, güvenlik amacıyla genellikle sandbox (kapsayıcı) ortamında çalıştırılır ve sistem düzeyindeki birçok kaynağa, donanıma veya çekirdek özelliklerine erişimleri sınırlıdır. --privileged bayrağı, container'a aşağıdaki ek yetkileri sağlar:

Cihaz Erişimi (Device Access): Container, host makinedeki cihazlara doğrudan erişebilir. Bu, örneğin USB cihazlarına veya özel donanımlara (GPU, ağ kartları vb.) erişimi içerir.
Çekirdek Modüllerine Erişim: Container, host çekirdeğinde çalışan modüllere erişebilir ve hatta bunları değiştirebilir. Bu, bazı kernel seviyesinde işlemler gerçekleştirmek için gereklidir.
/dev Dizinine Erişim: Container, host makinenin /dev dizinindeki dosyaları okuyabilir ve yazabilir, bu da donanım cihazlarına erişim sağlar.
Yüksek Sistem Yetkileri: Container, bazı çekirdek özelliklerine erişebilir (örneğin, ağ ayarlarını değiştirme, kernel parametrelerini okuma ve yazma).
Docker-in-Docker gibi

minikube delete
minikube start

kubectl apply -f pv.yaml
kubectl get pv

/*
hangi pvyi tercih ettiğimizi pvc.yaml dosyası içinde label selector attributeü içinde belirtiriz.
*/

kubectl apply -f pvc.yaml
kubectl get pvc

kubectl apply -f deploy.yaml
kubectl describe pod mysqldeploymentAdi

minikube node add
kubectl get nodes

<b><mark>kubectl taint node minikube a=b:NoExecute</mark></b><br>
kubectl describe pod mysqldeploymentAdi

minikube delete
docker rm -f nfssrv
docker volume rm nfsvol
docker network rm nfsnet



terminal2
kubectl get pods -w -o wide
