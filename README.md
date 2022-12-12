Personal repository hosting helm charts
- [Charts available to istall](#charts-available-to-istall)
- [Prerequisites](#prerequisites)
- [How to install](#how-to-install)


# Charts available to istall

| Name        | Version | ArtifactHub link                                                            | Supported ubernetes version | Helm version |
|-------------|---------|-----------------------------------------------------------------------------|-----------------------------|--------------|
| devops-demo | 1.0.0  | https://artifacthub.io/packages/helm/mungari-development-charts/devops-demo | 1.25.2                      | 3.10.2       |

# Prerequisites

1. Minikube, KIND or a kubernetes cluster and kubectl installed (Quick guide on how to set up minikube: https://minikube.sigs.k8s.io/docs/start/)
to install kubectl:
```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

chmod +x kubectl

sudo mv kubectl /usr/local/bin
```
2. Helm installed (https://helm.sh/docs/intro/install/)

# How to install

1. Add repository to helm
    ```bash
    helm repo add myrepo https://mungari.github.io/mungari-helm-repo/ 
    ```
    change myrepo as needed

2. Install helm chart
    ```bash
    helm install -f myvalues.yaml chart-name myrepo/chart-name  
    ```
    populate myvalues.yaml according to the helm chart README.md file (you can find it either on artifact hub if it has updated -takes a while- or under charts/chart-name) to ovverride settings
