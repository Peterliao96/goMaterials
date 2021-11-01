## K8s 命令 Exercises

#### 1. Core Command

1.1 Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace.

```powershell
[root@master ~]# kubectl create ns mynamespace
namespace/mynamespace created 
```

1.2 Create the pod that was just described using YAML

```powershell
[root@master ~]# kubectl run nginx --image=nginx --restart=Never --dry-run=client -n mynamespace -o yaml > pod.yaml
```

this will create a pod.yaml file like this:

```yaml
apiVersion: v1
kind: Pod
metadata: 
 creationTimestamp: null
 labels:
   run: nginx
 name: nginx
 namespace: mynamespace
spec:
 containers:
 - image: nginx
   imagePullPolicy: IfNotPresent
   name: nginx
   resources: {}
 dnsPolicy: ClusterFirst
 restartPolicy: Never
status: {}
```

Now we create the pod,

```powershell
[root@master ~]# kubectl create -f pod.yaml
pod/nginx created
```

1.3 Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output.

```powershell
[root@master ~]# kubectl run busybox --image=busybox --comand --restart=Never --env
[root@master ~]# kubectl logs busybox
```

1.4 Get the YAML for a new namespace called "myns" without creating it.

```powershell
[root@master ~]# kubectl create namespace myns -o yaml --dry-run=client
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  namespace: myns
spec: {}
status: {}  
```

1.5 Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1CPU, 1G memory and 2 pods without creating it

```powershell
[root@master ~]# kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run=client -o yaml
```

1.6 Get pods on all namespaces

```powershell
[root@master ~]# kubectl get po --all-namespaces
```

1.7 Create a pod with image nginx called nginx and expose traffic on port 80

```powershell
[root@master ~]# kubectl run nginx --image=nginx --restart=Never --port=80
```

1.8 Change pod's image to nginx: 1.7.1. Observe that the container will be restarted as soon as the image gets pulled

```powershell
[root@master ~]# kubectl set image pod/nginx nginx=nginx:1.7.1
[root@master ~]# kubectl describe pod nginx
[root@master ~]# kubectl get pod nginx -w
```

1.9 Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'

```powershell
[root@master ~]#
```

1.10 Get pod's YAML

```powershell
[root@master ~]# kubectl get pod -o yaml
```

1.11 Get information about the pod, including details about potential issues (e.g, pod hasn't started)

```powershell
[root@master ~]# 
```

1.12 Get pod logs

```powershell
[root@master ~]# kubectl logs nginx 
```

1.13 If pod crashed and restarted, get logs about the previous instance

```powershell
[root@master ~]# 
```

1.14 Execute a simple shell on the nginx pod

```powershell
[root@master ~]# kubectl exec -it nginx -- /bin/bash
```

1.15 Create a busybox pod that echoes "hello world" and then exits

```powershell
[root@master ~]# kubectl run busybox --image=busybox -it --restart=Never -- echo 'hello world'
```

1.16 Do the same, but have the pod deleted automatically when it's completed

```powershell
[root@master ~]# kubectl run busybox --image=busybox -it --rm --restart=Never -- /bin/sh -c 'echo hello world'
```

1.17 Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod.

```powershell
[root@master ~]# kubectl run nginx --image=nginx --restart=Never --env=var1=val1
[root@master ~]# kubectl exec -it nginx -- env
```



#### 2. Multi-Container Pods

2.1 Create a Pod with two containers, both with image busybox and command "echo hello; sleep 3600". Connect to the second container and run "ls"

```powershell
[root@master ~]# kubectl run busybox --image=busybox --restart=Never -o yaml --dry-run=client -- /bin/sh -c 'echo hello;sleep 3600' > pod.yaml
[root@master ~]# vim pod.yaml
# 复制args以及之后的，修改一下name为busybox2即可
containers:
  - args:
    - /bin/sh
    - -c
    - echo hello;sleep 3600
    image: busybox
    imagePullPolicy: IfNotPresent
    name: busybox
    resources: {}
  - args:
    - /bin/sh
    - -c
    - echo hello;sleep 3600
    image: busybox
    name: busybox2
[root@master ~]# kubectl create -f pod.yaml
pod/busybox created
[root@master ~]# kubectl exec -it busybox -c busybox2 -- /bin/sh
ls
exit
```

2.2 Create pod with nginx container exposed at port 80. Add a busybox init container which downloads a page using "wget -O /work-dir/index.html http://neverssl.com/online". Make a volume of type emptyDir and mount it in both containers. For the nginx container, mount it on "/usr/share/nginx/html" and for the initcontainer, mount it on "/work-dir". When done, get the IP of the created pod and create a busybox pod and run "wget -O- IP"

```powershell
[root@master ~]# 
```



#### 3. Pod Design

Labels and annotations

3.1 Create 3 pods with name nginx1, nginx2, nginx3. All of them should have the label app=v1

```powershell
[root@master ~]# 
```

3.2 Show all labels of the pods

```powershell
[root@master ~]# 
```

3.3 Change the labels of pod 'nginx2' to be app=v2

```powershell
[root@master ~]# 
```

3.4 Get the label 'app' for the pods (show a column with APP labels)

```powershell
[root@master ~]# 
```

3.5 Get only the 'app=v2' pods

```powershell
[root@master ~]# 
```

3.6 Remove the 'app' label from the pods we created before

```powershell
[root@master ~]# 
```

3.7 Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'

```powershell
[root@master ~]# 
```

3.8 Check the annotations for pod nginx1

```powershell
[root@master ~]# 
```

3.9 Remove the annotations for these three pods

```powershell
[root@master ~]# 
```

3.10 Remove these pods to have a clean state in your cluster

```powershell
[root@master ~]# 
```

Deployments

3.11 Create a deployment with image nginx:1.18.0, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

```powershell
[root@master ~]# 
```

3.12 View the YAML of this deployment

```powershell
[root@master ~]# 
```

3.13 View the YAML of the replica set that was created by this deployment

```powershell
[root@master ~]# 
```

3.14 Get the YAML for one of the pods

```powershell
[root@master ~]# 
```

3.15 Check how the deployment rollout is going

```powershell
[root@master ~]# 
```

3.16 Update the nginx image to nginx:1.19.8

```powershell
[root@master ~]# 
```

3.17 Check the rollout history and confirm that replicas are OK

```powershell
[root@master ~]# 
```

3.18 Undo the latest rollout and verify that new pods have the old image (nginx:1.18.0)

```powershell
[root@master ~]# 
```

3.19 Do an on purpose update of the deployment with a wrong image nginx:1.91

```powershell
[root@master ~]# 
```

3.20 Verify that something's wrong with the rollout

```powershell
[root@master ~]# 
```

3.21 Return the deployment to the second revision (number 2) and verify the image is nginx:1.19.8

```powershell
[root@master ~]# 
```

3.22 Check the details of the fourth revision (number 4)

```powershell
[root@master ~]# 
```

3.23 Scale the deployment to 5 replicas

```powershell
[root@master ~]# 
```

3.24 Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%

```powershell
[root@master ~]# 
```

3.25 Pause the rollout of the deployment

```powershell
[root@master ~]# 
```

3.26 Update the image to nginx:1.19.9 and check that there's nothing going on, since we paused the rollout

```powershell
[root@master ~]# 
```

3.27 Resume the rollout and check that the nginx:1.19.9 image has been applied

```powershell
[root@master ~]# 
```

3.28 Delete the deployment and the horizontal pod autoscaler you created

```powershell
[root@master ~]# 
```

Jobs

3.29 Create a job named pi with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

```powershell
[root@master ~]# 
```

3.30 Wait till it's done, get the output

```powershell
[root@master ~]# 
```

3.31 Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'

```powershell
[root@master ~]# 
```

3.32 Follow the logs for the pod(you'll wait for 30 seconds)

```powershell
[root@master ~]# 
```

3.33 See the status of the job, describe it and see the logs

```powershell
[root@master ~]# 
```

3.34 Delete the job

```powershell
[root@master ~]# 
```

3.35 Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

```powershell
[root@master ~]# 
```

3.36 Create the same job, make it run 5 times, one after the other. Verify its status and delete it.

```powershell
[root@master ~]# 
```

3.37 Create the same job, but make it run 5 parallel times

```powershell
[root@master ~]# 
```

Cron jobs

3.38 Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from Kubernetes cluster' to standard output

```powershell
[root@master ~]# 
```

3.39 See its logs and delete it

```powershell
[root@master ~]# 
```

3.40 Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to start execution after its scheduled time (i.e, the job missed its scheduled time).

```powershell
[root@master ~]# 
```

3.41 Create a cron job with image busybox that runs every minute and writes 'date;echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it successfully starts but it takes more than 12 seconds to complete execution.

```powershell
[root@master ~]# 
```



#### 4. Configuration

ConfigMaps

4.1 Create a configmap named config with values foo=lala, foo2=lolo

```powershell
[root@master ~]# 
```

4.2 Display its values

```powershell
[root@master ~]# 
```

4.3 Create and display a configMap from a file

```powershell
[root@master ~]# 
```

4.4 Create and display a configMap from a .env file

```powershell
[root@master ~]# 
```

4.5 Create and display a configMap from a file, giving the key 'special'

```powershell
[root@master ~]# 
```

4.6 Create a configMap called 'options' with the value var5=val5. Create a new nginx pod that loads the value from variable 'var5' in an env variable called 'option'

```powershell
[root@master ~]# 
```

4,7 Create a configMap 'anotherone' with values 'var6=val6', 'var7=val7'. Load this configMap as env variables into a new nginx pod

```powershell
[root@master ~]# 
```

4.8 Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.

```powershell
[root@master ~]# 
```

SecurityContext

4.9 Create the YAML for an nginx pod that runs with the user ID 101. No need to create the pod.

```powershell
[root@master ~]# 
```

4.10 Create the YAML for an nginx pod that has the capabilities "NET_ADMIN", "SYS_TIME" added on its single container

```powershell
[root@master ~]# 
```

Requests and limits

4.11 Create an nginx pod with requests cpu=100m, memory=256Mi and limits cpu=200m, memory=512Mi

```powershell
[root@master ~]# 
```

Secrets

4.12 Create a secret called mysecret with the values password=mypass

```powershell
[root@master ~]# 
```

4.13 Create a secret called mysecret2 that gets key/value from file

```powershell
[root@master ~]# 
```

4.14 Get the value of mysecret2

```powershell
[root@master ~]# 
```

4.15 Create an nginx pod that mounts the secret mysecret2 in a volume on path /etc/foo

```powershell
[root@master ~]# 
```

4.16 Delete the pod you just created and mount the variable 'username' from secret mysecret2 onto a new nginx pod in env variable called 'USERNAME'

```powershell
[root@master ~]# 
```

ServiceAccounts

4.17 See all the service accounts of the cluster in all namespaces

```powershell
[root@master ~]# 
```

4.18 Create a new serviceaccount called 'myuser'

```powershell
[root@master ~]# 
```

4.19 Create an nginx pod that uses 'myuser' as a service account

```powershell
[root@master ~]# 
```

#### 5. Observablity

5.1 Create an nginx pod with a liveness probe that just runs the command 'ls'. Save its YAML in pod.yaml. Run it, check its probe status, delete it.

```powershell
[root@master ~]# kubectl run nginx --image=nginx --restart=Never --dry-run -o yaml > pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    livenessProbe: # our probe
      exec: # add this line
        command: # command definition
        - ls # ls command
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```powershell
[root@master ~]# kubectl create -f pod.yaml
[root@master ~]# kubectl describe pod nginx | grep -i liveness
[root@master ~]# kubectl delete -f pod.yaml
```

5.2 Modify the pod.yaml file so that liveness probe starts kicking in after 5 seconds whereas the interval between probes would be 5 seconds. Run it, check the probe, delete it.

```powershell
[root@master ~]# kubectl explain pod.spec.containers.livenessProbe # get exact name of livenessProbe
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    livenessProbe: 
      initialDelaySeconds: 5 # add this line
      periodSeconds: 5 # add this line as well
      exec:
        command:
        - ls
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```powershell
[root@master ~]# kubectl create -f pod.yaml
[root@master ~]# kubectl describe pod nginx | grep -i liveness
[root@master ~]# kubectl delete -f pod.yaml
```

5.3 Create an nginx pod (that includes port 80) with an HTTP readinessProbe on path '/' on port 80. Again, run it, check the readinessProbe, delete it.

```powershell
[root@master ~]# kubectl run nginx --image=nginx --restart=Never --dry-run -o yaml > pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    ports:
      - containerPort: 80 # Note: Readiness probes runs on the container during its whole lifecycle. Since nginx exposes 80, containerPort: 80 is not required for readiness to work.
    readinessProbe: # declare the readiness probe
      httpGet: # add this line
        path: / #
        port: 80 #
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```powershell
[root@master ~]# kubectl create -f pod.yaml
[root@master ~]# kubectl describe pod nginx | grep -i readiness
[root@master ~]# kubectl delete -f pod.yaml
```

5.4 Lots of pods are running in qa, alan, test, production namespaces. All of these pods are configured with liveness probe. Please list all pods whose liveness probe are failed in the format of <namespace>/<pod name> per line.

```powershell
[root@master ~]# 
```

Logging

5.5 Create a busybox pod that runs 'i=0;while true;do echo "$i:$(date)"'; i=$((i+1)); sleep 1; done'. Check its logs.

```powershell
[root@master ~]# 
```

Debugging

5.6 Create a busybox pod that runs 'ls /notexist'. Determine if there's an error (of course there is), see it. In the end, delete the pod.

```powershell
[root@master ~]# 
```

5.7 Create a busybox pod that runs 'notexist'. Determine if there's an error (of course there is), see it. In the end, delete the pod forcefully with a 0 grace period.

```powershell
[root@master ~]# 
```

5.8 Get CPU/memory utilization for nodes (metrics-server must be running)

```powershell
[root@master ~]# 
```



#### 6. Service and Networking

6.1 Create a pod with image nginx called nginx and expose its port 80

```powershell
[root@master ~]# 
```

6.2 Confirm that ClusterIP has been created. Also check endpoints

```powershell
[root@master ~]# 
```

6.3 Get service's ClusterIP, create a temp busybox pod and 'hit' that IP with wget

```powershell
[root@master ~]# 
```

6.4 Convert the ClusterIP to NodePort for the same service and find the NodePort port. Hit service using Node's IP. Delete the service and the pod at the end.

```powershell
[root@master ~]# 
```

6.5 Create a deployment called foo using image 'dgkanatsios/simpleapp' (a simple server that returns hostname) and 3 replicas. Label it as 'app=foo'. Declare that containers in this pod will accept traffic on port 8080 (do NOT create a service yet)

```powershell
[root@master ~]# 
```

6.6 Get the pod IPs. Create a temp busybox pod and try hitting them on port 8080.

```powershell
[root@master ~]# 
```

6.7 Create a service that exposes the deployment on port 6262. Verify its existence, check the endpoints.

```powershell
[root@master ~]# 
```

6.8 Create a temp busybox pod and connect via wget to foo service. Verify that each time there's a different hostname returned. Delete deployment and services to cleanup the cluster

```powershell
[root@master ~]# 
```

6.9 Create an nginx deployment of 2 replicas, expose it via a ClusterIP service on port 80. Create a NetworkPolicy so that only pods with labels 'access:granted' can access the deployment and apply it.

```powershell
[root@master ~]# 
```



#### 7. State Persistence

7.1 Create busybox pod with two containers, each one will have the image busybox and will run the 'sleep 3600' command. Make both containers mount an emptyDir at '/etc/foo'. Connect to the second busybox, write the first column of '/etc/passwd' file to '/etc/foo/passwd'. Connect to the first busybox and write '/etc/foo/passwd' file to standard output. Delete pod.

```powershell
[root@master ~]# 
```

7.2 Create a PersistentVolume of 10Gi, called 'myvolume'. Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'normal', mounted on hostPath '/etc/foo'. Save it on pv.yaml, add it to the cluster. Show the PersistentVolumes that exist on the cluster.

```powershell
[root@master ~]# 
```

7.3 Create a PersistentVolumeClaim for this storage class, called 'mypvc', a request of 4Gi and an accessMode of ReadWriteOnce, with the storageClassName of normal, and save it on pvc.yaml. Create it on the cluster. Show the PersistentVolumeClaims of the cluster. Show the PersistentVolumes of the cluster.

```powershell
[root@master ~]# 
```

7.4 Create a busybox pod with command 'sleep 3600', save it on pod.yaml. Mount the PersistentVolumeClaim to '/etc/foo'. Connect to the 'busybox' pod, and copy the '/etc/passwd' file to '/etc/foo/passwd'

```powershell
[root@master ~]# 
```

7.5 Create a second pod which is identical with the one you just created (you can easily do it by changing the 'name' property on pod.yaml). Connect to it and verify that '/etc/foo' contains the 'passwd' file. Delete pods to cleanup. Note: If you can't see the file from the second pod, can you figure out why? What would you do to fix that?

```powershell
[root@master ~]# 
```

7.6 Create a busybox pod with 'sleep 3600' as arguments. Copy '/etc/passwd' from pod to your local folder

```powershell
[root@master ~]# 
```

