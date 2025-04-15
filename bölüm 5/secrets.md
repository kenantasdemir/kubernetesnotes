
# Secret
Hassas bilgileri (DB User, Pass vb.) Environment Variables içerisinde saklayabilsekte, bu yöntem ideal olmayabilir.
//gizli bilgileri pod tanımına veya bir container imajına koymaktan daha güvenli olacaktır.
Secret object’i sayesinde bu hassas bilgileri uygulamanın object tanımlarını yaptığımız YAML dosyalarından ayırıyoruz ve ayrı olarak yönetebiliyoruz.
<b><mark>Token, username, password vb. tüm bilgileri secret içerisinde saklamak her zaman daha güvenli ve esnektir.</mark></b><br>

oluşturulan secretlar ile atanacak podlar aynı namespacete olmalıdır.
8 değişik türde secret oluşturulabilir. 
opaque(generic) default seçenektir.
hemen hemen her hassas datamızı bu tipi kullanarak saklayabiliriz.

---

kubectl describe secrets firstSecretObject
//secret içerisindeki dataları görüntüler

✅ secretlar tls sertifikaları ve ssh anahtarları gibi hassas verileri saklamamıza yarar.

terminal 1
kubectl apply -f secret.yaml
kubectl get secrets
kubectl describe secrets mysecret

(imperative)
<b><mark>
kubectl create secret generic mysecret2 --from-literal=db_server=db.example.com --from-literal=db_username=admin --from-literal=db_password=12345P@ssword
</mark></b><br>
//mysecret2 adlı secret nesnesi oluşturuldu. db_server db_username db_password adlı secretlar oluşturuldu. bu secretlara değerler atandı.<br>

<b><mark>kubectl create secret generic mysecret3 --from-file=db_server=server.txt 
                                        --from-file=db_username=username.txt 
                                        --from-file=db_password=password.txt</mark></b><br>

//mysecret3 adlı secret oluşturuldu. 
//db_server adlı secret nesnesini server.txt adlı dosyadan 
//db_username secret nesnesinin değerini username.txt adlı dosyadan
//db_password secret nesnesinin değerini password.txt adlı dosyadan okur.


<b><mark>kubectl create secret generic mysecret4 --from-file=config.json</mark></b><br>
//mysecret4 adlı secret nesnesi oluşturur. gerekli konfigurasyonları config.json dosyasından okur.


ya bu değerleri poda volume aracılığyla aktarır
ya da bu değerleri environment variable olarak geçeriz



-----------------------------------

kubectl apply -f podsecret.yaml
kubectl get pods

kubectl exec -it secretpodvolume -- bash
exit

kubectl exec secretpodenv -- printenv

kubectl exec secretpodenvall -- printenv

bir secret oluşturulduğunda etcde base64 encoded olarak tutuluyor. yani şifreli değil
sonradan şifrelenebilir

bir secret oluşturduğunuzda default olarak tüm kubernetes kullanıcıları diğer objelerde de
olduğu gibi secretlarda da erişim hakkına sahiptir.


***********************************************************************************************************************************



Pod’dan Secret’ı Okuma
–> Oluşturduğumuz Secret’ları Pod’a aktarmak için 2 yöntem vardır:
​ Volume olarak aktarma ve Env variable olarak aktarma

kubectl exec podName -- printenv
//podName adlı podda printenv komutunu çalıştırır.

























