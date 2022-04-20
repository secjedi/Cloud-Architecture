Nodes are like servers; they can be local or remote.
Each nodes consists of apods that have one container running in each of them. The conatiners share same ip address and vokume. SO keep this in mind. <br />
Start kubernetes node <br />

minikube start --driver=virtualbox 

Get node ip

minikube ip
