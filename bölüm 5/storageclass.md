
# Storage Class
StorageClass, yöneticilerin sundukları depolama "sınıflarını" tanımlamaları 
için bir yol sağlar. Farklı sınıflar, hizmet kalitesi düzeylerine veya 
yedekleme ilkelerine veya cluster yöneticileri tarafından belirlenen isteğe 
bağlı ilkelere eşlenebilir. Kubernetes, sınıfların neyi temsil ettiği 
konusunda fikir sahibi değildir. Bu kavram bazen diğer depolama sistemlerinde 
"profiller" olarak adlandırılır.


StorageClass, Kubernetes'te persistent storage (kalıcı depolama) 
kaynaklarının nasıl sağlanacağını ve yönetileceğini tanımlayan bir kaynak 
türüdür. Bu nesne, Kubernetes cluster'ındaki depolama sistemlerine nasıl 
erişileceğini, hangi depolama türlerinin kullanılacağını ve bu depolama 
kaynaklarının nasıl yönetileceğini belirler.

---

# StorageClass'ın Temel Özellikleri
Depolama Yönetimini Otomatikleştirir: StorageClass, bir PersistentVolumeClaim
(PVC) oluşturulurken hangi depolama sağlayıcısının ve parametrelerinin 
kullanılacağını belirtir. Bu sayede, Kubernetes bir PVC oluşturduğunda doğru 
türde depolama kaynağını otomatik olarak oluşturabilir.


Dinamik Depolama Kaynakları: Kubernetes'teki Persistent Volumes (PV), 
genellikle manuel olarak oluşturulabilir. Ancak StorageClass, dinamik olarak
PVC talep edildiğinde PV oluşturulmasını sağlar. Böylece kullanıcılar 
depolama talep ettiklerinde, Kubernetes uygun depolama birimini otomatik 
olarak sağlayabilir.


Çeşitli Depolama Çözümleri Desteği: Farklı depolama sağlayıcıları için 
parametreler ve ayarlar tanımlanabilir. Örneğin, bir StorageClass, bir bulut 
sağlayıcısının (AWS, Google Cloud, Azure vb.) belirli bir disk türünü 
kullanacak şekilde yapılandırılabilir. Ayrıca yerel depolama veya NFS gibi 
farklı depolama çözümleri de desteklenebilir.


Performans ve Özellikler: StorageClass ile depolama birimlerinin özellikleri 
belirlenebilir. Örneğin, SSD veya HDD kullanımı, depolama boyutu, replikasyon 
stratejileri gibi özellikler ayarlanabilir.

---

# StorageClass'ın Yapısı ve Parametreler
Bir StorageClass nesnesi, çeşitli özelliklere sahip olabilir. Bu özellikler şunlardır:

<b><mark>Provisioner</mark></b>: Hangi depolama sağlayıcısının kullanılacağını belirler. Bu, 
genellikle bir bulut sağlayıcısının veya bir depolama sisteminin driver'ıdır.
Örneğin, AWS için kubernetes.io/aws-ebs, Google Cloud için kubernetes.io/gce-
pd olabilir.

<b><mark>Parameters</mark></b>: Depolama sağlayıcısının özelliklerini belirten parametrelerdir. 
Bu parametreler, örneğin disk türü (ssd, hdd), şifreleme özellikleri veya 
diğer sağlayıcıya özgü ayarları içerebilir.

<b><mark>ReclaimPolicy</mark></b>: PVC silindiğinde, PV'nin ne olacağını belirler. İki ana değer vardır:
Retain: PVC silindiğinde, ilgili PV korunur.
Delete: PVC silindiğinde, ilgili PV de silinir.

<b><mark>VolumeBindingMode</mark></b>: PVC ile PV'nin nasıl ilişkilendirileceğini belirtir. 
Bu değer Immediate veya WaitForFirstConsumer olabilir. 
WaitForFirstConsumer, pod talep edilene kadar depolama kaynağının oluşturulmasını erteleme seçeneğidir.

<b><mark>AllowVolumeExpansion</mark></b>: Bu seçenek, PV'nin boyutunun genişletilip genişletilemeyeceğini belirler.


terminal1
kubectl config use-context aks-k8s-test
kubectl get storageclass

kubectl apply -f sc.yaml
kubectl get storageclasses

kubectl apply -f pvc.yaml

kubectl get storageclasses.storage.k8s.io

kubectl apply -f deploy.yaml

VOLUMEBINDINGMODE
WaitForFirstConsumer => bu pvc kullanılana kadar volume oluşturulmayacak

terminal2
kubectl get pvc -w

terminal3
kubectl get pv -w


*************************************************************************************************************


StatefulSet
1: StatefulSet tarafından oluşturulan her pod, stateful set tanımında belirlediğiniz pvc'e göre oluşturulan bir pv'ye sahip olur. Yani her pod'un kendine ait bir pv'si olur.
2: StatefulSet altında podlar sırayla oluşturulup sırayla silinir. Örneğin 3 podlu bir stateful set oluşturduğunuz zaman öncelikle pod0 oluşturulur. Bu pod ayağa kalkıp readiness ve liveness checklerinden geçip işlemlerini tamamlamadan bir sonraki pod oluşturulmaz. Ne zaman ki bu pod hazır running durumuna geçer o zaman bir sonraki pod oluşturulur. Aynı şekilde 3. Pod da 2. Pod tamamlanmadan oluşturulmaz. Tam tersi durumda da bu geçerlidir. Siz 3 podlu bir statefulset'i 2. Pod'a indirirseniz Kubernetes random bir pod seçip onu silme yoluna gitmez. En son yaratılan pod hangisi ise ilk olarak o silinir.
3: StatefulSet tarafından oluşturulan her pod'a statefulsetin_adı-0 1 2 3 şeklinde devam eden sabit bir isim verilir. İsimler random seçilmez ve bu isimler aynı zamanda containerin hostname'i olarak da atandığı için her uygulama bu isimle ulaşılabilir.
