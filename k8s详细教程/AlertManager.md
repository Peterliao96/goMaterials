## Alert Manager with Grafana, Prometheus and Kubernetes

#### 1. Deployment of Prometheus Service

```powershell
# create namespace
[root@master ~]# git clone https://github.com/iKubernetes/k8s-prom.git && cd k8s-prom
[root@master k8s-prom]# kubectl apply -f namespace.yaml
namespace/prom created

# deploy node_exporter
[root@master k8s-prom]# kubectl apply -f node_exporter/
daemonset.apps/prometheus-node-exporter created
service/prometheus-node-exporter created

# checkout the pods
[root@master k8s-prom]# kubectl get pods -n prom
NAME								READY		STATUS		RESTARTS		AGES
prometheus-node-exporter-q6fm5		 1/1		Running			0			3m43s

# deploy Prometheus server, edit rbac.authorization.k8s.io/prometheus/v1beta1 to rbac.authorization.k8s.io/prometheus/v1 if so
[root@master k8s-prom]# kubetctl apply -f prometheus/
configmap/prometheus-config created
deployment.apps/prometheus-server created
serviceaccount/prometheus created
service/prometheus created
clusterrole.rbac.authorization.k8s.io/prometheus created
clusterrolebinding.rbac.authorization.k8s.io/promethus created

# deploy kube-sate-metrics
[root@master k8s-prom]# kubectl apply -f kube-state-metrics
deployment.apps/kube-state-metrics created
serviceaccount/kube-state-metrics created
clusterrole.rbac.authorization.k8s.io/kube-state-metrics created
clusterrolebinding.rbac.authorization.k8s.io/kube-state-metrics created
service/kube-state-metrics created

# generate serving cert
[root@master k8s-prom]# cd /etc/kubernetes/pki/
[root@master pki]# (umask 077; openssl genrsa -out serving.key 2048)
.....................+++
....+++
e is 65537 (0x10001)
[root@master pki]# openssl req -new -key serving.key -out serving.csr -subj "CN=serving"
[root@master pki]# openssl x509 -req -in serving.csr -CA ./ca.crt -CAkey ./ca.key -CAcreateserial -out serving.crt -days 3650
Signature ok
subject=/CN=serving
Getting CA Private Key
[root@master pki]# kubectl create secret generic cm-adapter-serving-certs --from-file=serving.crt=./serving.crt --from-file=serving.key -n prom
secret/cm-adpter-serving-certs created
[root@master pki]# kubectl get secret -n prom
NAME								TYPE										DATA		AGE
cm-adapter-serving-certs			Opaque		 								2			 2m
kube-state-metrics-tokens-zvqg7		kubernetes.io/service-account-token			3			 17m
prometheus-token-k5fls				kubernetes.io/service-account-token			3			 43m

```

