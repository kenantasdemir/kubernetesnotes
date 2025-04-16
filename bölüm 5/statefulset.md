# Singleton Pod

Singleton pod, yalnızca bir örneği çalıştırılan bir Kubernetes pod'udur. 
Yani, bir Singleton pod yalnızca bir pod örneğinden oluşur ve bu pod'un 
birden fazla çoğaltması yoktur.

Singleton pod'lar, kritik sistem bileşenlerinin çalıştırılması veya yalnızca
bir örneğin çalıştırılmasının gerektiği özel durumlar gibi belirli durumlarda
kullanılabilir. Örneğin, bir yedekleme işlemi sırasında yalnızca bir pod'un 
çalıştırılması gerekebilir. Bir Singleton pod, bir Deployment, ReplicaSet 
veya StatefulSet aracılığıyla oluşturulabilir.


Kubernetes, Singleton pod'ların tek örneğinin herhangi bir sebeple durması
durumunda otomatik olarak yeniden başlatılmasını sağlayabilir. Ayrıca, bir 
Singleton pod'un yedeklenmesi ve kurtarılması için tasarlanmış mekanizmalar 
da mevcuttur.

Singleton pod'lar, Kubernetes'te yaygın bir kullanıma sahip değildir, ancak 
belirli senaryolarda yararlı olabilirler. Bu nedenle, kullanımları ihtiyaca 
bağlı olarak farklılık gösterebilir ve doğru senaryoda doğru şekilde 
kullanıldığında avantajlı olabilirler.


İşlemler manuel ve zahmetli
singleton podların fail durumunu kontrol eden bir mekanizma yok.
yeni bir pod eklemek ya da çıkarmak istenilirse tüm süreci siz yönetmelisiniz.

# StatefullSet

StatefulSet, Kubernetes'te durum bilgisi olan (stateful) uygulamaları yönetmek için kullanılan bir nesnedir. Özellikle her pod’un kendine özgü kimliğe (hostname, storage, IP vs.) sahip olması gerektiği durumlar için tasarlanmıştır.

Kısaca söylemek gerekirse:

Deployment, stateless (durumsuz) uygulamalar içindir.
StatefulSet, stateful (durumlu) uygulamalar içindir.

📛 Sabit Pod İsimleri	Her pod sıralı ve sabit isimle oluşturulur: app-0, app-1, app-2, ...
🧾 Sabit Volume	Her pod'a özgü PersistentVolumeClaim (PVC) oluşturulur ve bu volume ona ait kalır.
🔁 Sıralı Başlatma & Silme	Pod’lar sıralı olarak başlatılır (app-0 → app-1 → ...) ve yine sıralı olarak silinir.
♻️ Yeniden başlatmada aynı kimlik	Pod silinip yeniden başlatılsa bile aynı adı, IP’yi ve volume’u kullanır.

🧱 Ne zaman StatefulSet kullanmalıyım?
Veritabanları: MySQL, PostgreSQL, MongoDB, Cassandra...

Zookeeper, Kafka gibi quorum tabanlı sistemler

Her pod'un kendi persistent storage’ına sahip olması gerekiyorsa

Network üzerinden her pod’un sabit bir isme/IP’ye ihtiyacı varsa


terminal1
kubectl apply -f statefulset.yaml
kubectl exec cassandra-0 -- nodetool status
kubectl scale statefulset cassandra --replicas=5
kubectl delete pod cassandra-2
kubectl scale statefulset cassandra --replicas=3

kubectl get svc

kubectl run -it test --image=busybox -- sh
ping cassandra
ping cassandra-1.cassandra


terminal2
kubectl get pods -w


terminal3
kubectl get pvc -w


statefulset podları sırasıyla oluşturur.
1.pod oluşturulmadan 2.oluşturulmaz
her pod oluşturulmadan önce bir pvc oluşturulur ve bu pvc pv'ye bağlanır.

