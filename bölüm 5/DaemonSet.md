DaemonSet
<b><mark>tüm veya bazı nodeların bir podun bir kopyaasını çalıştırmasını sağlar.</mark></b><br>
clustera yeni node eklendikçe, onlara podlar da eklenir.
clusterdan node kaldırıldığında bu podlar da kaldırılır.
bir daemonsetin silinmesi oluşturduğu podları da temizleyecektir.

DaemonSet, Kubernetes'teki bir controller türüdür ve belirli bir pod'un, cluster'daki her bir node'da (veya belirli node'larda) çalışmasını sağlamak için kullanılır. DaemonSet, bir pod'un her bir node'da yalnızca bir kez çalışmasını garanti eder. Bu özellik, genellikle log toplama, monitoring (izleme), ağ proxy'leri veya sistem yönetimi gibi node'a özel görevleri gerçekleştirmek için kullanılır.

DaemonSet'in Temel Özellikleri
Her Node'da Pod Çalıştırma: <b><mark>DaemonSet, cluster'daki her node üzerinde bir pod çalıştırır. </mark></b><br>.Eğer yeni bir node cluster'a eklenirse, DaemonSet otomatik olarak o node'da da pod oluşturur.
Otomatik Pod Ekleme: Cluster'a yeni node eklendiğinde, DaemonSet'e bağlı pod, bu yeni node'a da otomatik olarak dağıtılır.
Pod Yönetimi: DaemonSet, cluster'daki tüm node'larda aynı pod'un çalışmasını sağlayarak, pod'ları yönetir. DaemonSet, bu pod'ları gerektiğinde günceller, yeniden başlatır ve yönetir.
Kullanım Senaryoları
DaemonSet genellikle şu durumlarda kullanılır:

Log Toplama: Cluster'daki her node'dan log toplamak için DaemonSet kullanılır. Örneğin, Fluentd veya Logstash gibi araçlar kullanılarak loglar toplanabilir.
Monitoring: Her node'dan performans verisi toplamak için monitoring agent'ları (örneğin Prometheus Node Exporter) dağıtılabilir.
Ağ Proxy'leri: Ağ proxy'leri (örneğin, Envoy veya Istio Proxy) her node üzerinde çalıştırılabilir.
Sistem Yönetimi: Sistemle ilgili görevlerin her node'da çalışmasını sağlamak için.




terminal1
kubectl get nodes
kubectl apply -f daemonset.yaml
minikube node add
minikube node add --worker
//ikinci node ekler

kubectl get nodes
kubectl get pods


kubectl delete pod podismi



terminal2
kubectl get daemonset -w




*******************************************************************************************************


Container Storage Interface (CSI)

CSI, isteğe bağlı blok ve dosya depolama depolama sistemlerini Kubernetes gibi 
Container Orchestration Systems (CO'ler). üzerindeki containerized iş yüklerine 
maruz bırakmak için bir standart olarak geliştirilmiştir. Container Storage Interface'in
benimsenmesiyle Kubernetes volume katmanı gerçekten genişletilebilir hale geldi.
Üçüncü taraf depolama sağlayıcıları, CSI kullanarak, 
çekirdek Kubernetes koduna dokunmak zorunda kalmadan Kubernetes'te yeni depolama 
sistemlerini açığa çıkaran eklentiler yazabilme imkanına kavuştu.
