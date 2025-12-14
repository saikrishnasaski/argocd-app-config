# Argocd

This repository holds kubernetes resources for one of the service which will be monitored by Argocd agent running on Kubernetes.

## Screenshot / Demo
![Demo Image](https://redhat-scholars.github.io/argocd-tutorial/argocd-tutorial/_images/argocd-sync-flow.png)

### Installing Argocd agent on k8s

Create argocd namespace with below command

kubectl create namespace argocd

Apply argo resource yaml file with following command which installs argocd in your k8s cluster.

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

### Access Argocd UI from local machine

To access argocd UI from local machine we need to do port forwarding with kubectl command. Argocd service object is reachable on port 80 and 443.

kubectl port-forward -n argocd svc/argocd-server 8089:443

Access argocd on https://localhost:8089       (username=admin)

Password is stored in this secret argocd-initial-admin-secret

To retrieve the password, execute following command

kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode

### Configure argo app config

Now we need to do tell argo for what are the source (github url) and destination (kubernetes API server url).
Once we have this config ready in yaml file (assuming file name is application.yaml)

kubectl apply -f application.yaml

In Argo UI you should be able to see application details like (deployment, pods for the configured application)

Argo agent will be polling configured git url for every 3 minutes. Any change to the app resource file in github, Argo agent will snychronize those changes in kubernetes cluster.
