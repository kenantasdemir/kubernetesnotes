Livenessprobe
kubelet, bir containerın ne zaman yeniden başlatılacağını bilmek için liveness probe kullanır.
Örneğin liveness probe bir uygulamanın çalıştığı ancak ilerleme kaydedemediği bir kilitlenmeyi
yakalayabilir. böyle bir durumda bir containerı yeniden başlatmak, 
hatalara rağmen uygulamayı daha kullanılabilir hale getirmeye yardımcı olabilir.

Liveness Probes sayesinde containera bir request göndererek, TCP connection açarak veya container içerisinde
bir komut çalıştırarak doğru çalışıp çalışmadığını anlayabiliriz.

Readinessprobe
kubelet bir containerın ne zaman trafiği kabul etmeye hazır olduğunu bilmek için readiness probeları kullanır.
bir pod tüm containerlar hazır olduğunda hazır kabul edilir.
readiness probe sayesinde bir pod hazır olana kadar service arkasına eklenmez

Liveness probe'ta olduğu gibi get isteği göndererek, TCP connection açarak veya container içerisinde bir komut çalıştırarak
doğru çalışıp çalışmadığını anlayabiliriz.

 Readiness ile Liveness arasındaki fark, 
 Readiness ilk çalışma anını baz alırken, 
 Liveness sürekli çalışıp çalışmadığını kontrol eder.


---------------------------------------------------------------------------------------------------------------

terminal1
kubectl apply -f readiness.yaml
kubectl exec frontend-5f56af56 -- rm -rf ready
//Bu komut, frontend-5f56af56 adlı Pod üzerinde çalışan konteynerin içinde, ready adlı klasorü ve alt klasörleri içerikleriyle beraber siler.

kubectl exec frontend-5f56af56 -- touch ready
//Bu komut, frontend-5f56af56 adlı Pod üzerinde çalışan konteynerin içinde, ready adlı klasor oluşturur.

terminal2
kubectl get pod -w

terminal3
kubectl describe endpoints frontend -w



---------------------------------------------------------------------------------------------------------------

terminal1
kubectl apply -f requestlimit.yaml
kubectl describe pod requestlimit
kubectl delete -f requestlimit.yaml

terminal2
kubectl get pods -w


######################################################################################################################################################################

kubectl apply -f podenv.yaml
kubectl exec envpod -- printenv
//envpod içinde printenv komutunu çalıştırır.

terminal1
<b><mark>kubectl port-forward pod/envpod 8080:80</mark></b><br>
//envpod adlı pod'un 80 numaralı hedef portunu yerel 8080 numaralı portuna yönlendirir.

terminal2
curl 127.0.0.1:8080

----------------------------------------

######################################################################################################################################################################

CPU kaynağı, CPU birimleri olarak ölçülür.
Kuberneteste bir CPU şuna eşdeğerdir.

1 AWS vCPU
1 GCP Core
1 Azure vCore
1 HyperThread bare-metal


Kesirli değerlere izin verilir. 0,5 CPU talep eden bir container, 1 CPU talep eden bir containerin yarısı kadar CPU alır.
Mili anlamında m son ekini kullanabilirsiniz. 
Örneğin 100m CPU, 100 milliCPU ve 0.1 CPU aynıdır. 1m'den daha ince hassasiyete izin verilmez.

CPU her zaman mutlak bir miktar olarak talep edilir, 
asla göreli bir miktar olarak talep edilmez; 0.1, tek çekirdekli, 
çift çekirdekli veya 48 çekirdekli bir makinede aynı miktarda CPU'dur.


		1 cpu core kullansın
	cpu: "1" = cpu: "1000" = cpu: "1000m"

	1 cpu core'unun %10'unu kullansın cpu: 
		"0.1" = cpu: "100" cpu: "100m"
resources: 
  limits:
    cpu: "100" 
  requests:
    cpu: "100"


######################################################################################################################################################################

Memory

Cluster'da mevcut durumda kullanılabilir bellek varsa, 
bir container belirlenen bellek isteğini aşabilir. 
Ancak bir container bellek sınırından fazlasını kullanmasına 
izin verilmez. Bir container, sınırından daha fazla bellek ayırırsa, 
container sonlandırma için aday olur. 
Container, limitinin ötesinde bellek tüketmeye devam ederse,
 container sonlandırılır. 
 Sonlandırılmış bir container yeniden başlatılabiliyorsa, 
 diğer herhangi bir runtime hatası türünde olduğu gibi kubelet onu 
 yeniden başlatır.


1000 YB yottabyte
Orders of magnitude of data
resources:
  requests:
    memory: "50Mi"
  limits:
    memory: "100Mi"























































