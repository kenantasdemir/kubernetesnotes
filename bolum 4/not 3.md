
<b><mark>iki poda aynı local volumün bağlanması için o iki podunda aynı worker node üstünde çalışması gerekir.</mark></b><br>
<b><mark>bir pod içinde ikinci container sidecar container olarak adlandırılır.</mark></b><br>
<b><mark>bu pod silinirse iki container birden silinir</mark></b><br>
<b><mark>bu containerlar arasında network izolasyonu bulunmaz</mark></b><br>
<b><mark>tek bir volume yaratılarak her iki containerada mount edilebilir.</mark></b><br>

Aynı pod içinde tanımlanmış tüm containerlar aynı worker node üstünde oluşturulur.
Containerlar ayrı birer ünitedir fakat pod’un lifecycle’i içinde yönetilir. Yani pod oluşturulunca iki container birden oluşturulur ve silinirse de iki container birden silinir. 
Bu containerlar arasında network izolasyonu bulunmaz Yani aynı pod içinde çalışan A ve B containerları birbirlerine localhost üstünden ulaşabilirler. Bunlar network bakımından sanki aynı makinede çalışan processler gibidir. 
Tek bir volüme yaratılarak her iki container’a da mount edilebilir. Böylelikle aynı dosyalar üstünde çalışabilirler.


----------------------------------------------------------------------------------------------------

kubectl apply -f podmulticontainer.yaml


kubectl exec -it multicontainer -c webcontainer -- /bin/sh
//multicontainer isimli container içinde bulunan containerlardan webcontainer isimli container içinde shell açar.

<b><mark>kubectl exec -it multicontainer -c sidecarcontainer -- /bin/sh</mark></b><br>
-----------------------------------------------------------------------------------------

kubectl logs -f multicontainer -c sidecarcontainer
//multicontainer içinde bulunan sidecarcontainer'a ait logları anlık olarak listeler.

<b><mark>kubectl port-forward pod/multicontainerpod 8080:80</mark></b><br>
//multicontainerpod içindeki containerların 80 portu ile makinemizin 8080 portunu eşler.<br>
//bu özelliğe port-forwarding denir.<br>
//bu özellik sayesinde pod'da çalışan bir uygulamaya dışarıdan erişmek 
//veya bu uygulamayla etkileşimde bulunmak mümkün hale gelir.



----------------------------------------------------------------------------------------------------
































