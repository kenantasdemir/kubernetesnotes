
Kubernetes ağ kuralları

kubernetes kurulmda podlara ip dağıtılması için bir ip adres aralığı ya da 
kubernetes terminoljisinde --pod-network-cidr belirlenir
kubernetesde her pod bu cidr bloğundan atanacak bir eşsiz ip adresine sahip olur.
aynı cluster içerisindeki tüm podlar varsayılan olarak birbirleriyle herhangi bir kısıtlama olmadan 
ve NAT yani network address translation olmadan haberleşebiliriler.


---------------------------------------------------------------------------------------------------

<b><mark>kubectl run firstpod --image=nginx --restart=Never</mark></b><br>
//nginx imajıdan bir pod yaratıldı. <br>
--restart=Never ifadesiyle bu pod durduğunda tekrar çalışmaması gerektiği belirtildi.<br>

kubectl get pods -o wide
//oluşturulmuş podları geniş formatta listeler.

<b><mark>kubectl explain serviceaccount</mark></b><br>
//serviceaccount hakkında detaylı bilgi görüntüler.<br>

kubectl describe pods firstpod
//firstpod isimli pod hakkında detaylı bilgi listeler.

Pod'u silmek: <b><mark>kubectl delete pod <pod-adı></mark></b><br>
Deployment pod sayısını sıfırlamak: <b><mark>kubectl scale deployment <deployment-adı> --replicas=0</mark></b><br>
Deployment'ı yeniden başlatmak: <b><mark>kubectl rollout restart deployment <deployment-adı></mark></b><br>

<b><mark>kubectl describe deployments denemedeployment</mark></b><br>
//denemedeployment nesnesi hakkında detaylı bilgi görüntüler.

kubectl logs firstpod
//firstpod isimli poda ait logları görüntüler.

<b><mark>kubectl logs -f firstpod</mark></b><br>
//anlık olarak eklenen logları listeler<br>

<b><mark>kubectl exec firstpod -- hostname</mark></b><br>
//firstpod isimli pod üzerinde hostname komutu çalıştırıldı

kubectl exec firstpod -- ls /
//firstpod isimli pod üzerinde ls / komutu çalıştırıldı.

kubectl exec -it firstpod -- /bin/sh
//firstpod isimli pod üzerinde /bin/sh komutu interaktif olarak çalıştırıldı.(yani firstpod isimli pod üzerinde shell açıldı.)
//exit yazıp enterladıktan sonra shellden çıkarsınız.

kubectl delete pods firstpod
//firstpod isimli pod silindi.

YAML dosyalarını kubernetes api'ya deklare etmek için apply kullanırız.

kubectl apply -f pod1.yaml
//hangi objenin oluşturulacağı ve bu objenin hangi özellikleri kullanacağı yaml dosyası içinde belirtilir.

kubectl describe pods firstpod

<b><mark>kubectl run mysecondpod --image=nginx --port=80 --labels="hi=Merhaba,world=Dunya" --restart=Never</mark></b><br>
//mysecondpod isimli yeni bir pod oluşturuldu. bu pod nginx imajını kullanacak. 80 portundan çalışacak restart policy ise Never olarak atandı.
//hi:Merhaba ve world:Dunya olmak üzere iki adet label eklendi.

<b><mark>kubectl edit pods mysecondpod</mark></b><br>
//daha önce oluşturulan mysecondpod isimi pod öğesinin konfigurasynlarını terminalde görüntüler
//burada poda ait konfigurasyonları değiştirilebilir.

<b><mark>kubectl get pods --show-labels</mark></b><br>

--------------------------------------------------------------------------------------------------------

POD YAŞAM DÖNGÜSÜ

yaml dosyasını clustera göndeririz.
kubernetes api server bu dosyayı alır diğer ayarları da default olan değerlerle birleştirir.
poda bir id atar tüm bunları etcdye kaydeder. tüm bunlardan sonra 
yaşam döngüsünün ilk aşaması olan PENDİNG'e geçer

kube-scheduler api server aracılığıyla etcdyi sürekli gözler
eğer burada yeni oluşturulmuş herhangi bir node ataması yapılmamış bir pod görürse çalışmaya başlar
algoritması ve seçme kriterlerine göre podun çalışmasının en uygun olacağı nodeu seçer ve 
veritabanındaki pod objesine node bilgisini ekler. bu noktadan itibaren pod CREATING aşamasındadır.

<b><mark>cluster içerisinde bulunan tüm nodelarda kubelet adında bir servis bulunur.</mark></b><br>
bu servis etcd veritabanını gözler. bulunduğunu nodea atanmış podları tespit eder
ilk olarak pod tanımında oluşturulması istenen containerlara bakar ve bu containerların
oluşturulacağı imageları sisteme indirir. eğer bir şekilde image indirilmezse pod öncelikle
image pull ardından da image pull backoff aşamasına geçer

ImagePullBackOff görürseniz bu nodun imageı repodan çekemediği ve bunu tekrar tekrar denediği
anlamına gelir. en sık karşılaşılan neden image isminin pod tanımında yanlış yazılması
diğer neden imageı çekmek için registry'e authentike olması gerekirken
kullanıcı adı ve şifre bilgisinin hatalı olması

image düzgün bir şekilde çekilmiş olsun. o noktada kubelet container engine ile haberleşir ve
ilgili containerların oluşturulması sağlanır. containerlar çalışınca pod RUNNING durumuna geçer.


bir podun içerisindeki container sürekli durup çalışıyorsa CRASHLOOPBACKOFF durumuna geçer
10 saniye sonra restart eder tekrar kapanırsa 20 saniye sonra restart eder 40 80 160 
en son 5 dakika aralıklarla dener. container çökmeyi bırakır ve 10 dk sorunsuz çalışırsa tekrar
RUNNING durumuna geçer.

kubectl get pods -w
//-w parametresi watch anlamına gelir. yani sistemdeki podları anlık olarak izler

best practiceler
bir poda bir container
bir containera bir uygulama

























