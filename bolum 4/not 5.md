
						Kubernetes annotation
				example.com/notification-email:admin@k8sfundamentals.com
			   Prefix "Opsiyonel"  	  Anahtar		Değer


Objeye onu tanımlayan fakat label olarak eklememizin sakıncalı olacağı bilgileri annotation "açıklama” olarak ekleriz.
Prefix "önek" kısmı opsiyoneldir. Zorunlu değildir.
Anahtar alanı 63 karakter veya daha az olmalıdır.
Alfanumerik bir karakterle ([a-z0-9 A-Z]) başlamalı ve bitmelidir
Tire (-), alt çizgi (_), noktalar (.) ve arasında alfanumerik değerler içerebilir.
Değer alanında bu kurallar geçerli değildir. Alfanümerik olmayan karakter de alabilir.


kubectl apply -f podannotation.yaml
kubectl describe pod annotationpod

<b><mark>kubectl annotate pods myfirstpod foo=bar</mark></b><br>
//myfirstpod isimli pod nesnesine foo=bar anotasyonu eklendi.

kubectl annotate pods mysecondpod foo=
//mysecondpod isimli poda foo adlı anotasyon eklendi.


kubectl annotate pods mysecondpod foo-
//foo isimli anotasyonu siler.s


-------------------------------------------------------------------------------------------------

<b><mark>kube-system isimli namespace kubernetes tarafından oluşturulan nesnelerin tutulduğu namespacetir.</mark></b><br>
<b><mark>kube-public kimliği doğrulanmamış kullanıcılar dahil tüm kullanıcılar tarafından erişilmesine
ihtiyaç duyulan objelerin oluşturulacağı yerdir.</mark></b><br>
<b><mark>kube-node-lease nodehardbit işlemleri için gereken özel bir namespacetir.</mark></b><br>

-------------------------------------------------------------------------------------------------
				
Namespecler adlar için bir kapsam sağlar.
Kaynak adlarının bir namespace içinde benzersiz olması gerekir.
Namepace birbirinin içine yerleştirilemez ve her 
Kubernetes kaynağı yalnızca bir namespace içinde olabilir.

Namespaceler cluster kaynaklarını birden çok kullanıcı arasında bölmenin bir yoludur.
			
Kubernetes'te, default olarak aynı cluster içerisindeki pod'lar arasında herhangi bir ağ izolasyonu yoktur. 
Bu, bir pod'un içindeki konteynerlerin başka bir pod'daki konteynerlere erişebileceği anlamına gelir.

kubectl get pods --namespace kube-system
//kube-system isimli namespaceteki podları listeler.

kubectl get pods -n kube-system
//yukarıdaki ile aynı işi yapar. tek fark --namespace keywordü -n olarak kısaltıldı.

kubectl get pods --all-namespaces
//bütün namespaceler altında bulunan pod objelerini listeler.


<b><mark>kubectl create namespace app1</mark></b><br>
//app1 isimli namespace oluşturuldu.

<b><mark>kubectl config set-context --current --namespace=app1</mark></b><br>
//varsayılan namespace değiştirildi.

kubectl get namespaces
//sistemdeki tüm namespaceleri döndürür.

kubectl exec -it namespacedpod -n development -- /bin/sh

<b><mark>kubectl config set-context --current --namespace=development</mark></b><br>
//development isimli namespacei aktif olarak ayarlar.

<b><mark>kubectl delete namespaces development</mark></b><br>
//development isimli namespacesi siler.

-------------------------------------------------------------------------------------------------

Bir deployment'da istenen durumu tanımlarsınız ve deployment controller, mevcut durumu istenilen durumla karşılaştırıp gerekli aksiyonları alır.

<b><mark>Her deployment’ta en az bir tane selector tanımı olmalıdır.</mark></b><br>
Birden fazla deployment yaratacaksanız, farklı label’lar kullanmak zorundasınız. 
Yoksa deploymentlar hangi podların kendine ait olduğunu karıştırabilir. 
Ayrıca, aynı labelları kullanan singleton bir pod’da yaratmak sakıncalıdır!

Bir Replicasetin amacı, herhangi bir zamanda çalışan kararlı bir replika pod setini sürdürmektir.
Bu nedenle, genellikle belirli sayıda özdeş podun kullanılabilirliğini garanti etmek için kullanılır.



terminal1
kubectl create deployment firstdeployment --image=nginx:latest --replicas=2
//nginx imajından bir deployment oluşturur. 2 tane replika oluşturur.

kubectl delete deployment firstdeployment
//firstdeployment isimli deployment nesnesini siler.


kubectl get deployments watch
//deploymentları listeler ve anlık olarak izler.


-----------------------------------

kubectl edit pods myfirstpod
//myfirstpod isimli nesnenin özelliklerini terminalde yaml formatında görüntüler.


-----------------------------------

terminal1
<b><mark>kubectl set image deployments/firstdeployment myCon:httpd</mark><b></br>
//myCon isimli containerdaki imajı httpd olarak günceller.

terminal2
kubectl get pods --watch

---------------------------------

terminal1
kubectl scale deployment firstdeployment --replicas=5
//firstdeployment nesnesinin replika sayısını 5 olarak günceller..

---------------------------------

terminal1
kubectl delete deployments firstdeployment

terminal2
kubectl get pods --watch

-------------------------------------------------------------------------------------------------



















