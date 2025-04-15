
# Job
<b><mark>Bir Job objesi, bir veya daha fazla pod oluşturur ve 
belirli bir sayıda pod başarıyla sonlandırılana kadar pod yürütmeyi yeniden denemeye devam eder.</mark></b><br>
<b><mark>Job belirtilen sayıda pod'un başarıyla tamamlanma durumunu izler. </mark></b><br>
<b><mark>Belirtilen sayıda başarılı tamamlamaya ulaşıldığında, Job (yani görev) tamamlanır. </mark></b><br>
<b><mark>Bir Job'un silinmesi, oluşturduğu podları temizleyecektir.</mark></b><br>



terminal1
kubectl apply -f job.yaml
kubectl logs pi-4jfd6
kubectl delete jobs.batch pi

terminal2
kubectl get pods -w

terminal3
kubectl get jobs


**************************************************************************************


# CronJOB

Bir cronjob nesnesi, bir crontab dosyasının bir satırı gibidir.

“Bir CronJob nesnesi, bir crontab dosyasının bir satırı gibidir.”

CronJob, klasik Linux crontab satırları gibi zamanlama bilgisi içerir.

Cron formatı (* * * * *) kullanılır: dakika, saat, gün, ay, hafta günü şeklinde.

“Belirli bir zamanlamaya göre cron formatında yazılmış bir job’u periyodik olarak çalıştırır.”

Belirtilen zamanlama aralığında Job nesneleri oluşturur.

Bu Job nesnesi pod’ları yaratır ve çalıştırır.

Başarısız çalıştırmaları, geçmiş kayıtları ve eş zamanlı çalıştırma ayarlarını kontrol etmek için şu parametreler kullanılır:

<ul>
	<li>successfulJobsHistoryLimit</li>
	<li>failedJobsHistoryLimit</li>
	<li>
		concurrencyPolicy
	</li>
	<li>
		startingDeadlineSeconds
	</li>
</ul>









terminal1
kubectl apply -f cronjob.yaml
kubectl get cronjobs.batch
kubectl delete cronjobs.batch hello

terminal2
kubectl get pods -w
