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


