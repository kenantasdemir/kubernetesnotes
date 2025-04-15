# ConfigMap (gizli olmayan konfigurasyon verileri)
ConfigMap’ler Secret objectleriyle tamamen aynı mantıkta çalışır. 
Tek farkı; Secret’lar etcd üzerinde base64 ile encode edilerek encoded bir şekilde saklanır. 
ConfigMap’ler ise encode edilmez ve bu sebeple hassas datalar içermemelidir.
Pod içerisine Volume veya Env. Variables olarak tanımlayabiliriz.
Gizli olmayan verileri anahtar-değer eşlenikleri olarak depolamak için kullanılan bir API nesnesidir.
Podlar, configmapi environment variable, komut satırı argümanları veya bir volume olarak bağlanan apılandırma dosyaları olarak kullanabilir.

configmapler oluştururlen tip belirtmemize gerek yoktur.

<b><mark>kubectl create configmap myconfigmap --from-literal=background=blue --from-file=a.txt</mark></b><br>
//myconfigmap adlı configmap nesnesi oluşturur.<br>
//ekleyeceğiniz tek bir değer varsa --from-literal ile ekleyebilirsiniz.<br>
//ya da gerekli configurasyonları bir dosyada belirtip --from-file ile bu dosyayı belirtebilirsiniz. <br>

kubectl get configmaps
//tüm configmapleri listeler.

kubectl delete configmaps myconfigmap
//myconfigmap adlı configmap nesnesini siler.

Kubectl aracılığıyla config.json dosyamızdan ConfigMap object’i oluşturalım.
Eğer bir CI/CD üzerinde çalıştırıyor ve logları takip edeceksek;

# --dry-run normalde deprecated, fakat hala eski versionlarda kullanılıyor.
kubectl create configmap xyzconfig --from-file ${configFile} -o yaml --dry-run | kubectl apply -f -

# yeni hali --dry-run="client"
kubectl create configmap berkconfig --from-file config.json -o yaml --dry-run="client" | kubectl apply -f -

# Sondaki "-" (dash) çıkan pipe'ın ilk tarafından çıkan output'u alır.

Yukarıdaki komutu çalıştırdığımızda kubectl config.json dosyasını alır; bize kubectl apply komutunu kullanabileceğimiz bir ConfigMap YAML dosyası içeriğini oluşturur ve bu içeriği –dry-run seçeneğinden dolayı output olarak ekrana basar.
Gelen output’u alıp (bash “pipe | ” ile) kubectl apply komutu ile cluster’ımıza gönderiyoruz ve ConfigMap tanıtımını yapıyoruz.

Eğer logları okumadan direkt config.json dosyasından bir ConfigMap yaratmak istiyorsak
# config.json yerine bir çok formatta dosya kullanabiliriz: ÖR: yaml
kubectl create configmap <configName> --from-file config.json


Şimdi Pod’u oluştururken ConfigMap’imizdeki değerleri Pod içerisinde yer alan bir 
klasördeki bir dosyaya aktarmamız ve bunu “volume” mantığında bir dosya içerisinde saklamamız gerekiyor.
“volumes” kısmında volume’ümüzü ve configMap’imizi tanımlayalım.
Pod içerisindeki “volumeMounts” kısmında ise tanımladığımız bu volume’ü pod’umuza tanıtalım.
mountPath kısmında configMap içerisinde yer alan dosyanın, Pod içerisinde hangi klasör altına 
kopyalanacağı ve yeni isminin ne olacağını belirtebiliriz.
subPath ile configMap içerisindeki dosyanın (ÖR: config.json) ismini vermemiz gerekiyor. 
Böylelikle Pod yaratılırken “Bu dosyanın ismi değişecek.” demiş oluyoruz.


<b><mark>kubectl delete secrets mysecret1 --now=true</mark></b><br>

<b><mark>kubectl delete secrets mysecret2 --grade-period=1</mark></b><br>
//mysecret2 adlı secret nesnesini siler.

terminal1
kubectl apply -f configmap.yaml
kubectl get pods
kubectl exec -it configmappod -- bash
printenv
cd config
ls


****************************************************************************************************************

kubectl config current-context
//o anki context bilgisini görüntüler.

kubectl config get-users
//o anki kubeconfig içinde bulunan kullanıcıları listeler.

