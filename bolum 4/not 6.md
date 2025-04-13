deployment bu özelliklerde bir replicaset oluşturur ve podlar bunun tarafından oluşturulur.

replication controller birden fazla sayıda aynı tip pod oluşturmak için kullanılır. fakat deploy
ettiği podlarla ilgili değişiklik yapmak istediğimizde sorunlar çıkarıyordu.. bunu çözmek için
deployment ve replicaset adında iki objeye bölündü.

replicaset
belirlenen özlliklere göre belirlenen sayıda pod oluşturmak ve desired statete kalmasını sağlamak

deployment ise replicasetin bir üst objesi olarak dizayn edildi.
pod tanımında bir güncelleme yaparsak bu güncellemenin belirli kurallara ve sırayla uygulanmasını
sağlamış oldu.

<b><mark>deployment oluşturulduğunda kendi içinde replicaset oluşturur ve yönetir.
deployment tanımında değişiklik olursa deployment yeni tanıma göre yeni replicaset oluşturur. 
ilk oluşturulan replicaset kendi oluşturduğu podları silmeye başlar. yeni replicaset yeni podları yaratır
silme ve oluşturma işleminin hangi sırayla olacağını ve ne şekilde davranılacağını bizler belirleyebiliriz.</mark></b><br>

terminal1
kubectl get deployments
<b><mark>kubectl set image deployment/firstdeployment nginx:httpd</mark></b><br>
<b><mark>kubectl rollout undo deployment firstdeployment</mark></b><br>
//son değişikliği geri alır

terminal2
kubectl get replicaset
//replicasetleri görüntüler.

watch kubectl get replicaset

terminal3
watch kubectl get pods

--

kubectl delete rs firstrs


kubectl rollout undo deployment firstdeployment

***************************************************************************************************************************


Service
Bir dizi Pod üzerinde çalışan bir uygulamayı ağ hizmeti olarak göstermenin soyut bir yolu.

Kubernetes ile, uygulamanızı tanıdık olmayan bir hizmet bulma mekanizması kullanacak şekilde değiştirmeniz gerekmez. 
Kubernetes, Pod'lara kendi IP adreslerini ve bir dizi Pod için tek bir DNS adı verir ve bunlar arasında yük dengeleyebilir.

Type values and their behaviors are:
ClusterIP: 
NodePort: 
LoadBalancer: 
ExternalName: 






