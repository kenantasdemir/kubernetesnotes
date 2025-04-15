Node affinity
Node affinity kavramsal olarak nodeSelector'a benzer ve <b><mark>nodelara atanan etiketlere göre 
podunuzun hangi node üstünde schedule edilmeye uygun olduğunu kısıtlamanıza olanak tanır.</mark></b><br>

Pod affinity
<b><mark>pod affinity, podunuzun hangi node üstünde oluşturulmaya uygun olduğunu nodelardaki 
etiketlere göre değil halihazırda nodeda çalışmakta olan podlardaki etiketlere göre sınırlamanıza olanak tanır. benzer podları bir arada çalıştırır.</mark></b><br>


Pod Anti-Affinity (Uzak):
<b><mark>Tersi. Belirli pod’lardan uzakta yerleşmesini sağlar.</mark></b><br>

node selector sayesinde bir podu istediğimiz bir node üstünde oluşturabiliriz

terminal1
kubectl apply -f podnodeaffinity.yaml
kubectl get nodes
kubectl label node minikube app=blue

terminal2
kubectl get pods -w


