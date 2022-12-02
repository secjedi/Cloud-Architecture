# Cloud-Architecture
Simple steps to architecting in cloud

### Some Security mechanisms for Kubernetes
- Network Security: To secure pod-to-pod communication, we need to use network policies that accepts or rejects network traffic at pod level
- For Pod Security PSP Replacement Policy; that checks if a pod satisfies the minimum conditions before running in a cluster
- Make sure sensitive information like auth token, passwords, SSH keys are stored using Kubernetes secrets
- Kubernetes RBAC; to make sure each user can perform specific roles in a cluster
- Make sure TLS is enabled for Kubernetes ingress; that is to allow services in the cluster to be accessed from the outside.


We also need to make sure there are some ongoing monitoring.
-	Automated image scanning at every stage in the CI/CD pipeline
-	Host OS hardening: to ensure that the containers have least privileged access to the host
-	Build up base container images from scratch to minimize attack surface
-	Cluster hardening; regular review of RBAC and TLS for data transit
-	We can also do micro-segmentation for pods and services
-	Make sure data in transit is encrypted


