## Prerequisite

 A running k8s cluster
- Minikube
- Docker desktop
- Kind
- Rancher desktop
- Full cluster

## Installation options

- Non High availablity setup
  - Suitable for evaluation or dev/test environments
- High availablity setup
  - Recommended for production
  - You need atleast 3 worker nodes
- Light installation "Core"
  - Suitable if ArgoCD is used by administrators only. UI and API server is not installed for end users.
  - By default its installed as Non-HA
 
## Privilege options

ArgoCD provides two options for in-cluster privileges:
- Cluster-admin privileges: Use this if you want to deploy into the same cluster where ArgoCD runs in.
- Namespace level privileges: Use this if you are planning to use ArgoCD without deploying in the same cluster where ArgoCD runs in.

You need to create a namespace for argocd ```kubectl create ns argocd``` and then choose one of the below options :

1. Non-HA:

   a. cluster-admin privileges: https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   
   b. namespace level privileges: https://github.com/argoproj/argo-cd/raw/stable/manifests/namespace-install.yaml
2. HA:
   
   a. cluster-admin privileges: https://github.com/argoproj/argo-cd/raw/stable/manifests/ha/install.yaml

   b. namespace level privileges: https://github.com/argoproj/argo-cd/raw/stable/manifests/ha/namespace-install.yaml
3. Light installation "Core"

   https://github.com/argoproj/argo-cd/raw/stable/manifests/core-install.yaml

4. Helm chart:

   https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd

Initial admin password is stored as a secret in argocd namespace:

`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo`

## Accessing ArgoCD server (API + UI)

By default ArgoCD server is not exposed with external endpoint. But we can expose it by using:

- Service: LoadBalancer - Change the ArgoCD service type to LoadBalancer and this will create LB if we're using a managed service like EKS, AKS etc
- Ingress: Use your preferred ingress controller - Create an Ingress resource that point to argocd-server service.
- Port-forward: Use this to access locally on your machine - ```kubectl port-forward svc/argocd-server -n argocd 8080:443```

## Installing ArgoCD CLI

- CLI is useful when you need to interact with ArgoCD in CI pipelines
- You manage everything using CLI
  - Manage applications
  - Manage repos
  - Manage clusters
  - Admin tasks
  - Manage projects
  - And more
- You need to log into ArgoCD server before using any commands

  ```argocd login <ARGOCD_SERVER>```
- Follow the guide in this link https://argo-cd.readthedocs.io/en/stable/cli_installation/
   

