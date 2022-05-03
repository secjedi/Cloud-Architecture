Nodes are like servers; they can be local or remote.
Each nodes consists of apods that have one container running in each of them. The conatiners share same ip address and vokume. So keep this in mind. <br />
Start kubernetes node <br />

`minikube start --driver=virtualbox`

Get node ip
`minikube ip`

ssh into the node
`ssh docker@[192.xxx.xx.x]`

To check pods in a cluster
`kubectl get pods`

To create alias `alias k=kubectl`

Check pods in a particular namespace
`kubectl get pods --namespace=kube-system`

To create a pod
`kubectl run nginx --image=nginx`

To get the details about a pod
`kubectl describe pod nginx`

Create a deployement (this is better for load balancing amongst pods and elasticity)
`kubectl create deployment nginx-dep --image=nginx`

Scale deployments 
`k scale deployment nginx-dep --replicas=5`

To scale down
`k scale deployment nginx-dep --replicas=3`

Now we have 3 pods running in our deployment <br />
`$ k get pods -o wide`
````
NAME                         READY   STATUS    RESTARTS   AGE    IP           NODE       NOMINATED NODE   READINESS GATES
nginx-dep-845cc48b5d-25t8j   1/1     Running   0          27m    1xx.xx.xx.x   minikube   <none>           <none>
nginx-dep-845cc48b5d-8d9kt   1/1     Running   0          113s   1xx.xx.xx.x   minikube   <none>           <none>
nginx-dep-845cc48b5d-pxmrn   1/1     Running   0          113s   1xx.xx.xx.x   minikube   <none> 
````

We can connect to individual pods within the the node (ssh into the node using ssh docker@ip-addr), using 
`curl 1xx.xx.xx.x`. We should get the nginx default webserver output.
But we cannot obviously cannot connect from out main machine to the pods because it is external of course. <br />
To connect to these pods externally, we create services.


To connect to specific deployments using specific ip address, use services; cluster Ip; virtual ip addresses. they  will be single ip addresses for all pods.
Using service, we can expose specific pods from the deployment. Since nginx servers are inside the pods, we can expose port 80 so it can be reached from the outside via port 8080 `kubectl expose deployment nginx-dep --port=8080 --target-port=80`

To check the created service: <br />
```
$ kubectl get services
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.xx.x.x      <none>        443/TCP    2d21h
nginx-dep    ClusterIP   10.xxx.xx.xxx   <none>        8080/TCP   29m
```
The first is a custom service. The second is the nginx service we just created.

If we have a database deployment and we dont want it to be reached from the outside, the best thing to do is to create a cluster IP so other deployment in the cluster would access the database deployment from this newly created cluster IP. cluster IP is only available inside the cluster. <br />

We can connect to the ip address in the cluster after (ssh docker@minikub-ip) then `curl 10.xx.xx.xxx:8080`

The response provided would be from one of the pods in the deplyment.

```
$ ssh docker@192.xx.xx.xxx
docker@192.xx.xx.xxx's password: 
                         _             _            
            _         _ ( )           ( )           
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ curl 10.xx.x.xx:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
To describe service `kubectl describe service nginx-dep`

###Create a webserver, dockerize it using docker file, create deployments...
https://github.com/bstashchuk/k8s

To build docker image
`docker build . -t secjedi/k8s-web-hello`

Push to repo in dockerhub
- First login `sudo docker login`
- Then push to remote hub `sudo docker push secjedi/k8s-web-hello`
- Now image is ready and available on docker hub ![alt text](https://github.com/secjedi/CyberDefense/blob/main/Images/zerologon/docker.png)
- Create deplyement with this image as the base image `kubectl create deployment k8s-web-hello --image=secjedi/k8s-web-hello`
- `$ kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
k8s-web-hello-749d77f744-hc9m9   1/1     Running   0          5m5s`
- Create service; cluster ip `$ kubectl expose deployment k8s-web-hello --port=3000
service/k8s-web-hello exposed`
- View the cluster ip `$ kubectl get svc
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
k8s-web-hello   ClusterIP   10.x.xx.xxx   <none>        xxx/TCP   34s
kubernetes      ClusterIP   10.xx.xx.xx       <none>        443/TCP    2d23h`
- view webserver in cluster using cluster ip <br />
```
ssh docker@192.xx.xx.xx
docker@192.xx.xx.xx's password: 
                         _             _            
            _         _ ( )           ( )           
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ curl 10.xx.xx.xxx:3000
VERSION 2: Hello from the k8s-web-hello-749d77f744-hc9m9
```
sclae deployment `k scale deployment k8s-web-hello --replicas=4`

Now we see 4 pods
```
$ k get pods
NAME                             READY   STATUS    RESTARTS   AGE
k8s-web-hello-749d77f744-7p2wd   1/1     Running   0          26s
k8s-web-hello-749d77f744-cczvv   1/1     Running   0          26s
k8s-web-hello-749d77f744-d8bcv   1/1     Running   0          26s
k8s-web-hello-749d77f744-hc9m9   1/1     Running   0          17m
```
If we connect back again to our cluster and try reaching the cluster ip, we would get responses from different pods at different times. So, load is baalnced across different pods.  <br/>
```
docker@192.xx.xx.xx's password: 
                         _             _            
            _         _ ( )           ( )           
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ curl 10.xx.xx.xx:3000
VERSION 2: Hello from the k8s-web-hello-749d77f744-hc9m9$ curl 10.xx.xx.xx:3000
VERSION 2: Hello from the k8s-web-hello-749d77f744-d8bcv$ curl 10.xx.xx.xx:3000
VERSION 2: Hello from the k8s-web-hello-749d77f744-7p2wd$ curl 10.xx.xx.xx:3000;echo
VERSION 2: Hello from the k8s-web-hello-749d77f744-7p2wd
$ curl 10.xx.xx.xx:3000;echo
VERSION 2: Hello from the k8s-web-hello-749d77f744-cczvv
```
<br />
Now to create node ip `$ k expose deployment k8s-web-hello --type=NodePort --port=3000
service/k8s-web-hello exposed`

```
$ k get svc
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
k8s-web-hello   NodePort    10.xx.xx.xx   <none>        3000:31496/TCP   44s
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP          2d23h
```
Now we can connect to the node externally using the minikube ip and port 31496
![alt text](https://github.com/secjedi/CyberDefense/blob/main/Images/zerologon/connect.png) <br />


We can also connect using the command `$ minikube service k8s-web-hello`

To see only url: `minikube service k8s-web-hello --url`

Create a load balancer service `k expose deployment k8s-web-hello --type=LoadBalancer --port=3000` <br />
For kubernetes on cloud providers, load baalancers are automatically created.

Roll update to your image: Pserhaps you made changes to your app, tehn you have to rebuild the image using docker build.
`sudo docker build . -t secjedi/k8s-web-hello:2.0.0`
<br />
`sudo docker push secjedi/k8s-web-hello:2.0.0`
NOw we will have teh latest image onn dockerhub <br />

On the terminal:` k set image deployment k8s-web-hello k8s-web-hello=secjedi/k8s-web-hello:2.0.0` <br />
Then immediately paste:
`k rollout status deploy k8s-web-hello` <br />
```
k get pods
NAME                             READY   STATUS    RESTARTS   AGE
k8s-web-hello-6ff48dd8cb-2x4l8   1/1     Running   0          22s
k8s-web-hello-6ff48dd8cb-ndx4t   1/1     Running   0          33s
k8s-web-hello-6ff48dd8cb-zbwrk   1/1     Running   0          26s
k8s-web-hello-6ff48dd8cb-zm656   1/1     Running   0          33s

```
Seeing the timing, we can see that new pods were created immediately. we can also check using curl. <br />
If we delete one of the pods, minikube restarts another one immediately. Because we has initially stated that we need 4 replicas running.


### Visualizing DashBoards in Kubernetes
`minikube dashboard`








