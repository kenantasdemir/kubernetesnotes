# Taint and Toleration

Taint ve toleration
NoSchedule, PreferNoSchedule, NoExecute

<b><mark>Taint, bir node'a uygulanan etiket veya işarettir. Taint, o node'un belirli pod'lar tarafından kullanılmasını engeller, ancak bu engellemeyi esnek hale getirmek için toleration eklenebilir.</mark></b><br>

<b><mark>Taint, genellikle bir node'a belirli bir koşul ya da özellik eklemek için kullanılır. Örneğin, bir node bakımda olduğunda o node'a bir taint ekleyerek, o node'daki pod'ların çalışmasını engelleyebilirsiniz.</mark></b><br>

Taint yapısının genel formatı:

<key>=<value>:<effect>
key: Taint'in anahtarı, genellikle tanımlayıcı bir string.
value: Taint ile ilişkilendirilen değer (opsiyonel).
effect: Taint'in etkisi, üç farklı türü vardır:

NoSchedule: Bu node'a taint uygulanmışsa, sadece toleration'a sahip pod'lar bu node'a schedule edilir. Diğer pod'lar bu node üzerinde çalıştırılamaz. <br>
PreferNoSchedule: Bu etki, mümkünse pod'ların bu node'a schedule edilmesini engeller, ancak zorunlu değildir. Yani, bu node'a toleration olan pod'lar yine çalıştırılabilir. <br>
NoExecute: Taint uygulanmış node üzerinde zaten çalışan pod'lar, taint etkisi altındaysa çalıştırılmaya devam etmez ve pod'lar hemen dışlanır. <br>

Örnek:
Bir node'a NoSchedule taint uygulamak için şu komut kullanılır:

kubectl taint nodes <node-name> key=value:NoSchedule
Bu komut, belirtilen node üzerinde key=value şeklinde bir taint uygular ve bu node'a sadece ilgili toleration'a sahip pod'lar yerleştirilebilir.

2. Toleration 
Toleration, bir pod'un taint'lere karşı duyarlı olup olmadığını belirtir. Yani, bir pod'un node'da taint varsa, bu pod'un o node'a yerleştirilebilmesi için bir toleration'a sahip olması gerekir.

Toleration, pod tanımında yer alır ve taint'e karşı tolerans sağlar. Bir pod, taint'e sahip bir node'a yerleştirilmek isteniyorsa, bu pod’un toleration’a sahip olması gerekir.

Toleration, taint'in etkisini kabul eden bir "izin" gibidir. Bir pod, bir node'a yerleşmeden önce, o node'da taint olup olmadığını kontrol eder. Eğer node'da taint varsa ve pod toleration'a sahipse, pod o node'a yerleşebilir.

Örnek:
Bir pod'un taint'e karşı toleration eklemek için aşağıdaki gibi bir pod tanımı yapılır:

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

Yukarıdaki örnekte, mypod adlı pod, key=value taint'i ile işaretlenmiş node'lara NoSchedule etkisiyle yerleştirilebilir. Yani, pod'un toleration'ı olduğu için taint'e sahip node'a schedule edilmesine izin verilir.

Taint ve Toleration Kullanım Senaryoları
Node Bakımı: Node üzerinde bakım yapılacaksa, bakım sırasında o node üzerinde pod'ların çalışmasını engellemek için taint kullanılabilir. Bakım tamamlandıktan sonra taint kaldırılır ve pod'lar yeniden bu node'a schedule edilebilir.


terminal1
kubectl get nodes
kubectl describe nodes minikube
kubectl taint node minikube platform=production:NoSchedule
kubectl taint node minikube platform-
kubectl taint node minikube platform=production:NoSchedule

kubectl run test --image:nginx --restart=Never
kubectl describe pod test

kubectl apply -f podtoleration.yaml

kubectl taint node minikube color=blue:NoExecute
color=blue anahtarını tolere edemeyen podları bunun üzerinde schedule etme
aynı zamanda bunların üzerinde çalışan podlar varsa ve bu tainti tolere edemiyorlarsa
bunları bu podun üzerinden kaldır anlamına gelir.


terminal2
kubectl get pods -w


ephemeral volumeler podun yaşam süresi boyunca geçerli

