iki tip rollout stratejisi vardır.
RollingUpdate (default)(maxUnavailable ve maxUrge %25)
recreate 
tüm podlar silindikten sonra yenisi oluşturulur.

minikube delete
minikube start

terminal1
kubectl apply -f deploymentcreate.yaml
kubectl set image deployment rcdeployment nginx:httpd
//Bu komut, rcdeployment deployment'ının replica set'ini günceller ve yeni image'ın kullanılmasını sağlar.


watch kubectl get pods
//podları görüntüler. anlık olarak izler.

watch kubectl get rs
//replicaset nesnelerini anlık olarak izler.

-------------------------------------------------------

terminal1<br>
kubectl delete -f deploymentcreate.yaml<br>
kubectl apply -f deployrolling.yaml --record<br>
<b><mark>kubectl edit deployment rolldeployment --record</mark></b><br>
//rolldeployment objesini düzenlemeniz için terminalde yaml formatında görüntüler.<br>
//--record parametresi ile de revision history'ye kaydedilir.<br>
//kubectl rollout history ile yapılan düzenlemeye erişebilirsiniz.<br>

kubectl set image deployment rolldeployment nginx=httpd:alpine --record=true
//rolldeployment isimli deployment nesnesinin imajını nginx=httpd:altpine olarak günceller.
//--record=true ile de yapılan değişikliği revision history'ye kaydeder.

kubectl rollout history deployment rolldeployment
//revision history'deki daha önce yapılmış değişiklikler listelenir.

kubectl rollout history deployment rolldeployment --revision=2
//rolldeployment isimli deployment nesnesinin 2 numaralı revizyonu hakkında detaylı bilgi görüntüler.

kubectl rollout undo deployment rolldeployment --to-revision=1
//rolldeployment adlı deployment nesnesini 1 numaralı revizyondaki haline döndürür.

kubectl rollout status deployment rolldeployment -w
//bu komut ile deployment güncellemelerini canlı olarak izleriz.

kubectl rollout pause deployment myfirstdeployment
//Bu komut ile deployment güncellemesini durdurduk.

kubectl rollout resume deployment myfirstdeployment
//bu komut ile deployment güncellemelerini devam ettiriyoruz.

kubectl delete -f deployrolling.yaml
//deployrolling.yaml isimli dosyada tanımlanan özellikler ile eşleşen nesneyi siler.


terminal3
watch kubectl get rs
//replicaset nesnelerini listeler.

-------------------------------------------------------
terminal1
kubectl apply -f deployrolling.yaml

terminal2
kubectl rollout status deployment rolldeployment -w
//Bu komutu kullanarak, yeni bir replica set'in deployment'a başarılı bir şekilde uygulanıp uygulanmadığını kontrol edebilirsiniz. 


-------------------------------------------------------

terminal1
kubectl set image deployment rolldeployment nginx=httpd:alpine
//rolldeployment isimli deployment nesnesinin imajını nginx=httpd:alpine olarak günceller.

terminal2
kubectl rollout pause deployment rolldeployment
// belirtilen rolldeployment adlı deployment'ın otomatik güncelleme işlemlerini duraklatır. 
// Bu durumda deployment, yeni bir replica set oluşturmaz ve mevcut replica set ve pod'ları üzerindeki işlemler de durdurulur.


###########################################################################################################################################3

Kubernete kurulumunda podlara ip dağıtımı için --pod-network-cidr olarak adlandırılan bir IP adres aralığı belirlenir.

her pod bu cidr bloğundan atanacak eşsiz bir ip adresine sahip olur.

aynı cluster içindeki podlar herhangi bir kısıtlama yada NAT olmadan haberleşebilirler.(default)

Kubernetes, CoreDNS veya eski sürümlerde kube-dns gibi bir DNS çözümleyicisi kullanır. 
Bu DNS çözümleyicisi, her pod'un DNS taleplerini karşılar.
CoreDNS, Kubernetes kümelerinde varsayılan DNS çözümleyicisidir ve pod'lar arasında DNS çözümleme sağlar.

aynı node üstünde bulunan kubernetes objeleri(pod,container) arasında network izolasyonu yoktur

pod içindeki bir container dış dünyadan bir ip ile haberleşebilir.
aynı pod içindeki 2 container birbirleriyle haberleşebilir 
fakat bir container farklı bir node üstündeki başka bir containerla haberleşebilmesi
için Container Network Interface(CNI) kullanmak zorundadır.

Linux containerlarda ağ arabirimlerini yapılandırmak için eklentiler yazılabilmesini sağlayan
spesifikasyonları belirler. CNI, yalnızca containerların ağ bağlantısıyla ve containerlar silindiğinde ayrılan
kaynakların kaldırılmasıyla ilgilenir.
Bu odak nedeniyle, CNI geniş bir desteğe sahiptir ve spesifikasyonun uygulanması kolaydır.

istediğiniz CNI plug-in'ini kullanabilirsiniz.

<b><mark>Dış dünyadan containerlar ile haberleşmek isterseniz service objeleri işinizi görecektir.</mark></b><br>

herbir worker node üzerinde bir adet virtual bridge oluşturulur.

<b><mark>yeni bir pod olşturulduğunda o pod için linux kernelinde ayrı bir network namespacei oluşturulur
ve bu podun sanat ethernet0 interfaceine bu pod cidr adres bloğundan bir ip adres atanır 
bulunduğu host üzerindeki bridgee bağlanır labellar sayesinde objeler arasında bağlantı kurulur.</mark></b><br>

<b><mark>clusterIP virtual bir ip'ye sahiptir ve herhangi bir interface'a map edilmez</mark></b><br>

terminal1
kubectl apply -f deploy.yaml
kubectl apply -f serviceClusterIP.yaml
kubectl exec -it firstpod -- bash
nslookup backend

terminal2
kubectl get nodes -w

kubernetes içinde core dns tabanlı dns hizmeti bulunur
tüm podlar sorgularını buraya gönderir. bir servis oluşturulduğunda 
isim ve ip adresi bu dnste kaydedilir.

curl backend:5000
exit

------------------------------------------------------------------------------------

NodePort

terminal1
kubectl apply -f serviceNodePort.yaml
kubectl get service
minikube service --url frontend

curl localhost:portNumber


------------------------------------------------------------------------------------
		ENDPOINT oluşturma

kubectl create service clusterip my-service --tcp=80:80
kubectl create endpoint my-service --addresses=10.240.0.2 --port=80
kubectl get endpoints my-service

------------------------------------------------------------------------------------


terminal1
kubectl config use-context aks-k8s-test
kubectl get pods
kubectl get nodes
kubectl apply -f deploy.yaml
kubectl apply -f serviceLoadBalancer.yaml
kubectl get service

------------------------------------------------------------------------------------

kubectl config use-context minikube
kubectl get service
kubectl delete service backend
kubectl delete service frontend

@@kubectl expose deployment backend --type=ClusterIP --name=backend
Bu komut Kubernetes ortamında bir servis oluşturur ve 
belirtilen deployment (backend) için ClusterIP tipinde bir servis oluşturur. 
Bu servis, aynı Kubernetes clusterındaki diğer objelerin deployment'ı hedefleyebilmesi için bir endpoint sağlar.

--type parametresi ile servisin tipi belirlenir. 
ClusterIP tipi, yalnızca Kubernetes clusterındaki diğer objeler tarafından erişilebilen, 
cluster'ın içindeki bir IP adresine sahip bir servis oluşturur.

--name parametresi ile servisin ismi belirlenir. 
Bu isim daha sonra servise erişmek için kullanılabilir.

@@ kubectl expose deployment backend --type=NodePort --name=frontend
Bu komut Kubernetes ortamında bir servis oluşturur ve 
belirtilen deployment (backend) için NodePort tipinde bir servis oluşturur. 
Bu servis, cluster dışındaki kullanıcılara hizmet vermek için kullanılır.

--type parametresi ile servisin tipi belirlenir. 
NodePort tipi, servisin cluster dışındaki kullanıcılara bir port numarası atar ve 
servise erişmek için cluster IP'si ve NodePort kombinasyonunu kullanmalarını sağlar.

--name parametresi ile servisin ismi belirlenir.
Bu isim daha sonra servise erişmek için kullanılabilir.


kubectl get service

------------------------------------------------------------------------------------

<b><mark>bir servis oluşturduğumuzda servis endpoint denen bir obje oluşturur.
biizm belirlediğimiz selector tarafından seçilen podların ip adreslerini
üstünde barındırır.
servis trafiğini nereye yönlendireceğini bu listeye göre düzenler.
</mark></b><br>

terminal1
kubectl get endpoints
kubectl scale deployment frontend --replicas=5

terminal2
watch kubectl describe endpoint frontend



kubectl delete service myfirstservice

kubectl expose deployment myfirstdeployment --type=ClusterIP --name=firstservice
//myfirstdeployment adlı deployment nesnesinden firstservice isimli service oluşturur.
//bu servisin tipini de ClusterIP olarak atar.

kubectl expose deployment hellonodedeployment --type=LoadBalancer --port=8080
//hellonodedeployment adlı deployment nesnesini LoadBalancer tipinde bir servis olarak oluşturur.
//bu komut aynı zamanda hellonodedeployment adında bir servis oluşturur.

kubectl expose deployment myseconddeployment --type=NodePort --name=secondservice
//myseconddeployment adlı deployment nesnesinden secondservice isimli servis oluşturur.
//bu servisin tipini de NodePort olarak atar.

Note!
Deployment oluşturulduğu zaman arkaplanda replicaset oluşturur.
Service oluşturulduğu zaman arkaplanda endpoint oluşturur.

NodePort
<b><mark>
bu servis türü, clusterın dışından gelen bağlantıları çözmek için kullanılır.
NodePort servislerinin de ClusterIP'si mevcuttur.
Label selector aracılığıyla podlarla eşleştirilebilir.
</mark></b><br>

NodePort, Kubernetes'teki Service objelerinden biridir ve cluster içindeki herhangi bir node 
üzerinde özel bir port numarası atar ve bu port numarası üzerinden Service objesine gelen tüm trafiği 
bu node'a yönlendirir. Bu, cluster dışındaki kaynakların, 
Service objesi tarafından yönlendirilen trafiğe erişmesini sağlar.

Örneğin, bir web uygulaması çalıştırdığınızı ve web uygulamasının arkasında üç farklı pod olduğunu varsayalım.
 Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. 
<b><mark> NodePort Service objesi, bu pod'ları tek bir IP adresi altında gruplandırır ve </mark></b><br>
 belirli bir port numarası üzerinden erişilebilir hale getirir. 
 Ayrıca, bu Service objesi, her node'a özel bir port numarası atayarak, 
 cluster dışındaki kaynakların bu node'a erişmesine izin verir.

Örneğin, bir kullanıcı, node_ip:node_port şeklindeki bir URL ile web uygulamasına erişebilir. 
Bu, web uygulamasının, NodePort Service objesi aracılığıyla cluster 
dışındaki kaynaklarla haberleşebileceği anlamına gelir.

minikube service --url myfirstservice
//komutu ile tünel açılabilir.

ClusterIP
<b><mark>
Label selector ile podlarla ilişkilendirilebilir.
Bu obje Cluster içerisindeki tüm podların ve kaynakların çözebilecğei 
unique bir DNS ismine  sahip olur.</mark></b><br>

Bu Service türü, pod'ları tek bir IP adresi altında gruplandırır ve 
belirli bir port numarası üzerinden erişilebilir hale getirir.

Örneğin, bir web uygulaması çalıştırdığınızı ve web uygulamasının arkasında 
üç farklı pod olduğunu varsayalım. Bu pod'lar, uygulama sunucusu olarak hizmet vermektedir. 
ClusterIP Service objesi, bu pod'ları tek bir IP adresi altında 
gruplandırır ve belirli bir port numarası üzerinden erişilebilir hale getirir.

containerlar arası iletişimde kullanılır.
Load-Balancing ve service discovery görevlerini üstlenir
<b><mark>bir ClusterIP servisi oluşturup labellar sayesinde podlarla ilişkilendiririz.</mark></b><br>
Bu nesneyi yarattığmız zaman, Cluster içerisindeki tüm podların 
çözebileceği unique bir DNS adrese sahip olur.
Ayrıca her kubernetes kurulumunda sanal bir IP aralığına sahip olur.
Bu ip Adress sanaldır dolayısıyla herhangi bir interface'a map edilmez.

ClusterIP service nesnesi yaratıldığı zaman, bu nesneye yukarıda 
bahsedilen IP aralığından bir ip atanır ve servis ismi ile bu ip 
adresi Cluster'ın DNS mekanizmasına kaydedilir. 
Bu IP adresi sanal(virtual) bir ip adresidir.

<b><mark>Kube-Proxy tarafından tüm nodeların üstündeki IP tablosuna bu 
IP adresi eklenir. Bu ip adresine gelen istekler Round Tropping 
algoritmasıyla podlara yönlendirilir.</mark></b><br>


LoadBalancer
Sadece cloud service sağlayıcılarının manage Kubernetes servislerinde kullanılabilir.
Bu türde bir Servus oluşturulduğunda dışarıdan erişilebilecek bir load balancer oluşturulur.
Sonrasında bu lead balancer servis objesiyle ilişkilendirilir.
Dolayısıla bu public ip adresine Gelen paketier service içerisindeki podelara yönlendirilir.

LoadBalancer Service objesi, bu pod'ları tek bir IP adresi altında 
gruplandırır ve belirli bir port numarası üzerinden erişilebilir 
hale getirir. Ayrıca, cloud sağlayıcısı tarafından yönetilen bir yük 
dengeleyici oluşturur ve uygulama servislerini yük dengeleyici 
üzerinden dış dünyaya açar.

ExternalName
<b><mark>Uygulama servislerine DNS adları üzerinden erişmek için kullanılır.</mark></b><br> 
ExternalName Service objesi, cluster içinde bir DNS ismi olarak 
erişilebilen, farklı bir dış DNS adıyla eşleştirilir.

Örneğin, bir web uygulaması çalıştırdığınızı ve bu web uygulamasına 
erişmek için bir DNS adı kullanmak istediğinizi varsayalım. 
Ancak, web uygulaması bir başka cloud ortamındaki kaynakta çalışıyor 
ve erişilebilirliği sağlamak için bir DNS adı kullanıyor. 
ExternalName Service objesi, bu senaryoda kullanılabilir.

Dikkat Edilmesi Gerekenler
Pod ve Service objeleri arasındaki etiketleme tutarlılığına dikkat edin. 
<b><mark>Service objesi, etiketlerle belirlenen podları yönlendirecektir, 
bu nedenle Service ve pod etiketlerinin örtüşmesi gerekir.
</mark></b><br>

Service objesi türünü doğru şekilde belirleyin. 
ClusterIP, NodePort, LoadBalancer veya ExternalName türleri mevcuttur 
ve her birinin farklı kullanım durumları vardır. 
Bu nedenle, ihtiyacınıza en uygun olan Service türünü belirlemeniz 
önemlidir.

Hedef podların sağlığını kontrol edin. 
Service objesi, yönlendirdiği podların sağlığını kontrol etmez. 
Bu nedenle, hedef podlarının sağlığını kontrol eden bir 
readinessProbe veya livenessProbe belirlemek önemlidir.

<b><mark>Service objesi konfigürasyonunu güncellediğinizde, 
podların otomatik olarak yeniden başlatılmayacağına dikkat edin. 
</mark></b><br>
Bu nedenle, Service objesi konfigürasyonunu değiştirdiğinizde, 
ilgili podları da manuel olarak yeniden başlatmanız gerekebilir.

Service objelerinin trafik yönlendirmesi için kullanılan portları 
iyi bir şekilde belirleyin. Service objelerinin, diğer podlar veya 
hizmetler ile çakışmadığından emin olmak için benzersiz bir port 
numarası kullanın.
























