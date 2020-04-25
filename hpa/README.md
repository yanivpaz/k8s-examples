```
curl -o hey https://storage.googleapis.com/hey-release/hey_linux_amd64
sudo cp ./hey /usr/local/bin
```

#Assuming helm3
```
helm install stable/metrics-server  --set "args[0]=--kubelet-insecure-tls"   --generate-name
```

```
kubectl api-resources | grep -i metric
kubectl get --raw /apis/metrics.k8s.io/v1beta1  | jq

kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes  | jq
kubectl get --raw /apis/metrics.k8s.io/v1beta1/pods  | jq

helm install  prometheus-adapter stable/prometheus-adapter
kubectl get --raw /apis/custom.metrics.k8s.io/v1beta1
```

```
helm install prometheus  --set grafana.service.type=NodePort,grafana.service.nodePort=30030,grafana.service.port=80,grafana.adminPassword=admin stable/prometheus-operator

NODE_PORT=`kubectl get svc prometheus-grafana -o jsonpath='{.spec.ports[].nodePort}'`
```

```
localhost:30030
admin/admin

git clone https://github.com/arun-gupta/spring-boot-prometheus
cd spring-boot-prometheus
helm install myapp chart
kubectl get svc | grep myapp-greeting

curl http://localhost/hello
curl http://localhost/actuator/prometheus
```

Setup HPA
Create Horizontal Pod Autoscaler:

```
kubectl autoscale deployment myapp-greeting  --cpu-percent=2 --min=1 --max=10
kubectl  patch hpa myapp-greeting  --patch '{"spec":{"targetCPUUtilizationPercentage":80}}'
```

```
k get pods -l app=myapp-greeting -o wide
echo $((8/180.0*4))
k describe  hpa myapp-greeting
```


```
kubectl create -f hpa.yaml
```

HPA should read the metrics http_server_requests_seconds_max{uri="/hello"}. How would it?
---------------------







https://github.com/rakyll/hey

 watch -n 1 -d "curl http://$ENDPOINT/actuator/prometheus | grep http_server_requests_seconds_max | grep hello"

hey -c 100 -n 5000 -m GET http://localhost/hello  

# inspire by 
https://github.com/arun-gupta/arun-gupta.github.io/tree/master/hpa-app-metrics


