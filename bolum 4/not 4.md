terminal1
kubectl apply -f podinitcontainer.yaml
kubectl apply -f service1.yaml
kubectl logs -f initcontainerpod -c initcontainer

terminal2
kubectl describe pod initcontainerpod
watch -n 2 kubectl describe pod initcontainerpod
//initcontainerpod isimli podun durumunu anlık olarak listeler. -n 2 parametresi ile 2 saniyede bir günceller.




----------------------------------------------------------------------------------------------

kubectl apply -f mypodlabel.yaml
//mypodlabel.yaml dosyası içeriğindeki özelliklere uygun bir pod oluşturur.

kubectl get pods
//oluşturulmuş podları listeler.

<b><mark>kubectl get pods -l "hi" --show-labels</mark></b><br>
//hi adlı bir labela sahip olan tüm podları labellarıyla birlikte görüntüler.s

<b><mark>kubectl get pods -l "hi=Merhaba" --show-labels</mark></b><br>
//hi adlı etiketi olan ve etiket değeri Merhaba olan podları labellarıyla birlikte listeler.

<b><mark>kubectl get pods -l "hi=Merhaba,world=Dunya" --show-labels</mark></b><br>
//birden fazla etiketi olan podları etiketleriyle birlikte görüntüler. etiketler virgülle ayrılır.

<b><mark>kubectl get pods -l "hi=Merhaba,world!=Dunya" --show-labels</mark></b><br>
// hi etiket değeri Merhaba olan ve world etiket değeri Dunya olmayan podları listeler.s

<b><mark>kubectl get pods -l "hi,world=Dunya" --show-labels</mark></b><br>
// hi ve world etiketleri olan podları listeler.
//bu podlardan sadece world etiket değeri World olan podları listeler.s

<b><mark>kubectl get pods -l "hi=Merhaba,hi=Hola" --show-labels</mark></b><br>
//hi etiket değeri Merhaba ya da Hola olan podları listeler.

<b><mark>kubectl get pods -l "app in (myfirstapp)" --show-labels</mark></b><br>
//app etiketi içinde myfirstapp değerine sahip olan podları listeler.

<b><mark>kubectl get pods -l "app in (myfirstapp,mysecondapp)" --show-labels</mark></b><br>
//app etiketi içinde myfirstapp ve mysecondapp değerine sahip olan podları listeler.

<b><mark>kubectl get pods -l "app notin (firstapp,secondapp)" --show-labels</mark></b><br>
//app etiketi içinde myfirstapp ve mysecondapp değerine sahip olmayan podları listeler.

<b><mark>kubectl get pods -l "app,app notin (firstapp)" --show-labels</mark></b><br>
//app'i tarar app içinde firstapp olmayanı getir

<b><mark>kubectl get pods -l "!hi" --show-labels</mark></b><br>
//"hi" etiketine sahip olmayan podları listeler.

<b><mark>kubectl get pods -l "app in (firstapp),team notin (frontend)" --show-labels</mark></b><br>
//app etiketi içinde firstapp değerine sahip olan ve team etiketi içinde frontend değerine sahip olmayan podları görüntüler.

****************************************************************************************************


kubectl get pods --show-labels
//oluşturulmuş podları ve bu podlara ait labelleri listeler.

<b><mark>kubectl label pods mypod app=thirdapp</mark></b><br>
//mypod isimli poda app=thirdapp etiket ve değerini ekler.

<b><mark>kubectl label pods mypod app-</mark></b><br>
//mypod isimli podun app isimli etiketinin değerini siler.

<b><mark>kubectl label --overwrite pods mypod app=fourthapp</mark></b><br>
//mypod isimli podun app isimli etiketinin değerini fourthapp olarak günceller.

<b><mark>kubectl label pods --all foo=bar</mark></b><br>
//namespaceteki tüm podlara foo=bar etiket ve değeri eklendi.

kubectl get nodes --show-labels
//tüm node nesnelerini labellarıyla birlikte listeler.

kubectl label nodes minikube hddtype=ssd
//minikube adlı düğüme hddtype=ssd etiket ve değerini atar

kubectl get nodes --show-labels
//bu komut bir düğümün üstündeki tüm etiketleri görüntüler

kubectl delete -f pod
//"pod" adlı bir YAML dosyasında tanımlanan bir Kubernetes pod'unu siler.
kubectl delete -f podlabel.yaml
//podlabel.yaml dosyasında tanımlanan objeleri siler.















