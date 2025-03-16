# Helm

- Official website: https://helm.sh/
  
### 01. Introduction

- Helm is written in Go language
- Bitnami Repo: https://bitnami.com/stacks/helm
- GitHub Repo for charts: https://github.com/bitnami/charts
- 
### 02. Install Helm

- https://helm.sh >> Docs >> Introduction >> Installing Helm
- [As a pre-requisite, you can download and install chocolatey package manager] from https://chocolatey.org/install#individual

```
# If you’re using Windows with Chocolatey, install the binaries with the following command.
choco install kubernetes-helm

# If you’re using Linux, install the binaries with the following commands
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

- *NOTE*: If you get a message that openssl must first be installed, you can install it with the following command.
  ```
  sudo yum install openssl
  ```

### 03. Initialize a Helm Chart Repository

- Once you have Helm ready, you can add a chart repository. Check Artifact Hub for available Helm chart repositories.
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

- Once this is installed, you will be able to list the charts you can install:
```
helm search repo bitnami
```
