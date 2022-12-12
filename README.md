Personal repository hosting helm charts
- [Charts available to istall](#charts-available-to-istall)
- [How to install](#how-to-install)


# Charts available to istall

| Name        | Version | ArtifactHub link                                                            | Supported ubernetes version | Helm version |
|-------------|---------|-----------------------------------------------------------------------------|-----------------------------|--------------|
| devops-demo | 1.0.0  | https://artifacthub.io/packages/helm/mungari-development-charts/devops-demo | 1.25.2                      | 3.10.2       |

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
    populate myvalues.yaml according to the helm chart README.md file (you can find it either on artifact hub if it has updated -takes a while- or under charts/chart-name )
