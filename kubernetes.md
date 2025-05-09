kubectl, kuberenetes clusterlarla iletişim kurmamızı sağlyan temel cli aracıdır.

kubectl aracı, bağalanacağı cluster bilgisini config dosylarından okur.

kubectl default olarak home klasörü altında .kube altında config adlı bir bdosyaya bakar. ayarları buradan okur

aynı cluster üzerine birden fazla yetkiye sahip kullanıcılarla bağlanabiliriz.

kubectl config get-contexts

birden fazla context tanımı yapılabiliyor olsa da sadece bir tanesi current context konumunda olabilir.

kubectl ile girdiğimiz tüm komutlar bu context altında işlenecektir.

kubectl config current-context

kubectl config use-context docker-desktop

kubectl get nodes

kubectl cluster-info

kubectl cp --help

kubectl apply --help

kubectl get pods
kubectl create deployment nginx --image=nginx


kubectl get pod my-pod -o yaml 

kubectl top pod podName
//poda ait kaynak kullanım bilgilerini verir.

<b><mark>kubectl get services --sort-by=.metadata.name</mark></b>

<b><mark>kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'</mark></b><br>


<b><mark>kubectl get pv --sort-by=.spec.capacity.storage</mark></b><br>


<b><mark>kubectl get node --selector='!node-role.kubernetes.io/master'</mark></b><br>


<b><mark>kubectl get pods --field-selector=status.phase=Running</mark></b><br>

kubectl get pods --show-labels

kubectl get events --sort-by=.metadata.creationTimestamp

# Compares the current state of the cluster against the state that the cluster would be in if the manifest was applied.
kubectl diff -f ./my-manifest.yaml

Scaling Resources
kubectl scale --replicas=3 rs/foo                                 # Scale a replicaset named 'foo' to 3
kubectl scale --replicas=3 -f foo.yaml                            # Scale a resource specified in "foo.yaml" to 3
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql  # If the deployment named mysql's current size is 2, scale mysql to 3
kubectl scale --replicas=5 rc/foo rc/bar rc/baz                   # Scale multiple replication controllers



kubectl get deployments

kubectl delete pods testpod

kubectl varsayılan default isimli namespace'i kullanır

kubectl get pods -n kube-system

kubectl get pods --all-namespaces === kubectl get pods -A

kubectl get pods -A -o wide
kubectl get pods -A -o yaml
kubectl get pods -A -o json

kubectl get pods -A -o json | jq -r ".items[].spec.containers[].name"
kubectl explain pod

kubectl run firstpod --image=nginx --restart=Never

kubectl describe pods firstpod

kubectl describe deployments denemedeploy

kubectl logs firstpod

kubectl logs -f firstpod

kubectl exec firstpod -- hostname

kubectl run secondpod --image=nginx --port=80 --labels="app=frontend" --restart=Never

kubectl apply -f pod1.yaml

kubectl edit pods firstpod

kubectl get pods -w

minikube delete

minikube start

docker exet -it multicontainer -c sidecarcontainer -- /bin/bash

kubectl logs -f multicontainer -c sidecarcontainer

watch -n 2 kubectl describe pod initcontainerpod

kubectl logs -f initcontainerpod -c initcontainer


kubectl get pods -l "app" --show-labels
kubectl get pods -l "app=firstapp" --show-labels
kubectl get pods -l "app=firstapp,tier!=frontend" --show-labels
kubectl get pods -l "app,tier=frontend" --show-labels

kubectl get pods -l 'app in (firstapp,secondapp)' --show-labels
kubectl get pods -l 'app notin (firstapp,secondapp)' --show-labels

kubectl get pods -l '!app' --show-labels

kubectl get pods --show-labels

kubectl label pods pod9 app=thirdapp

kubectl label pods pod9 app-

kubectl label --overwrite pods pod9 team=team3

kubectl label pods --all foo=bar

kubectl get nodes --show-labels

kubectl label nodes minikube hddtype=ssd

kubectl delete -f podlabel.yaml

kubectl annotate pods annotationpodfirst foo=bar

kubectl get namespaces

kubectl get pods -n development

kubectl config set-context --current --namespace=development

kubectl delete namespaces development

kubectl create deployment firstdeployment --image=nginx:latest --replicas=2

watch kubectl get pods

kubectl set image deployment/firstdeployment nginx=httpd

kubectl scale deployment firstdeployment --replicas=5
kubectl scale deployment firstdeployment --replicas=2

kubectl get rs

kubectl apply -f deploymentrolling.yaml --record=true

kubectl rollout history deployment rolldeployment

kubectl rollout history deployment rolldeployment --revision=2

kubectl rollout undo deployment rolldeployment --to-revision=1

kubectl rollout status deployment rolldeployment -w

kubectl rollout pause deployment rolldeployment 

kubectl get service

kubectl get events -A

minikube addons enable dashboard
minikube addons enable metrics-server
minikube dashboard

lens
headlamp
helm
artifacthub
grafana


*************************************************************************************************

Kubernetes'te statik pod (statik podlar), Kubernetes API server’ı tarafından otomatik olarak yönetilmeyen ve doğrudan node’lar üzerinde çalışan pod'lardır. Bu pod'lar, Kubernetes’in normal işleyişine (örn. ReplicaSets, Deployments) dahil edilmezler ve genellikle sadece belirli bir node üzerinde çalışacak şekilde yapılandırılır. Statik pod’lar, genellikle özel bir gereksinim veya node seviyesinde çalışan bileşenler için kullanılır.
Statik Pod'ların Özellikleri:
Kubernetes API tarafından yönetilmez: Statik pod'lar, Kubernetes'in merkezi kontrol düzlemi (API server) tarafından yönetilmez. Bu pod'lar, doğrudan node üzerinde, kubelet tarafından başlatılır ve izlenir.
Kubelet tarafından oluşturulur: Statik pod’lar, node üzerinde çalışan kubelet tarafından yönetilir. Kubelet, belirli bir dizin (genellikle /etc/kubernetes/manifests/) içindeki pod tanımlarını kontrol eder. Eğer bu dizinde bir YAML veya JSON formatında pod tanımı varsa, kubelet bu pod’u otomatik olarak başlatır.
Yük dengelemesi ve auto-healing yok: Statik pod’lar, replica set veya deployment gibi yüksek erişilebilirlik ve otomatik yenileme özelliklerinden yararlanmaz. Eğer bir statik pod arızalanırsa, kubelet otomatik olarak yeniden başlatır, ancak bu süreç dışarıdan bir kontrol düzlemi tarafından yapılmaz.
Node’a özel: Statik pod'lar, belirli bir node'da çalışacak şekilde yapılandırıldığından, bir pod başka bir node'a taşınamaz. Bir node’un arızalanması durumunda, statik pod’lar başka bir node’a taşınamaz.
Statik Pod'ların Kullanım Alanları:
Cluster bileşenleri: Kubernetes kontrol düzlemi bileşenleri (API server, scheduler, controller-manager gibi) genellikle statik pod'lar olarak çalıştırılır. Özellikle kube-apiserver, kube-controller-manager, kube-scheduler ve etcd gibi bileşenler, her zaman belirli bir node üzerinde çalışacak şekilde yapılandırılabilir.
Özel node konfigürasyonları: Belirli bir node üzerinde çalışan ve yalnızca o node’un özelliklerine bağlı olan özel uygulamalar veya araçlar için kullanılabilir.
Yüksek güvenlik gereksinimleri: Kubernetes'in kontrol düzlemi dışındaki önemli sistemler, çok özel yapılandırmalarla çalışacaksa statik pod kullanımı tercih edilebilir.
Statik Pod Nasıl Çalışır?
Pod manifest dosyasını oluşturma: Öncelikle, node üzerinde çalışan kubelet’in gözlemleyeceği bir pod manifest dosyası oluşturulmalıdır. Bu dosya genellikle YAML formatında yazılır.
Manifesti doğru dizine koyma: Pod manifest dosyasını, kubelet’in gözlemlemesi için uygun bir dizine koymanız gerekir. Genellikle bu dizin /etc/kubernetes/manifests/’dir.
Kubelet pod’u başlatır: Kubelet, bu dizindeki manifest dosyalarını kontrol eder ve statik pod'ları başlatır. Bu pod'lar, node’un özelliklerine ve kubelet’in yapılandırmasına bağlı olarak başlatılır.
Pod yönetimi: Kubelet, statik pod’ları sürekli olarak izler. Eğer pod bir şekilde durursa veya ölürse, kubelet onu yeniden başlatır. Ancak, pod’u daha yüksek seviyede yöneten bir controller (replica set, deployment vb.) yoktur.
Statik Pod’ları Yönetmek:
Pod Güncellemeleri: Statik pod’ların güncellenmesi, manifest dosyasındaki değişiklikle yapılır. Yeni bir sürüm için pod manifestini günceller ve dosyayı yeniden node’daki uygun dizine koyarsınız. Kubelet, bu değişikliği algılar ve pod'u yeniden başlatır.
Loglar: Statik pod’ların logları, diğer pod’lar gibi kubectl logs komutu ile alınabilir.
Pod Silme: Statik pod'lar, doğrudan kubectl delete komutu ile silinemez. Pod’u silmek için ilgili manifest dosyasını kaldırmak gerekir ve kubelet bu pod'u sonlandırır.
Özet:
Statik pod’lar, Kubernetes içinde genellikle yönetilen pod'lardan farklı bir şekilde çalışır. API server tarafından izlenmezler ve yalnızca kubelet tarafından node üzerinde kontrol edilirler. Bu özellikleri, onları özellikle özel gereksinimler veya sistem seviyesindeki bileşenler için kullanışlı hale getirir. Ancak, yönetim açısından daha az esneklik ve otomatik iyileştirme (auto-healing) sağlarlar.






