Nodes are like servers; they can be local or remote.
Each nodes consists of apods that have one container running in each of them. The conatiners share same ip address and vokume. SO keep this in mind. <br />
Start kubernetes node <br />

`minikube start --driver=virtualbox`

Get node ip
`minikube ip`

ssh into the node
`ssh docker@[192.xxx.xx.x]`

To check pods in a cluster
`kubectl get pods`

CHeck pods in a particular namespace
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






