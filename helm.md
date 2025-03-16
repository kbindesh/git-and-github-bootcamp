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
- https://docs.aws.amazon.com/eks/latest/userguide/helm.html
  
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

### XX. See the version of Helm that you installed

```
helm version | cut -d + -f 1
```

### XX. Get Help

```
helm --help
OR
helm -h
```

### XX. How does Helm know about the K8s cluster?

- Helm uses the same kube-config file that kubectl uses

- In case you want to use another config file, update the KUBECONFIG environment variable:
  KUBECONFIG .kube/config

### 03. Initialize a Helm Chart Repository

- List all the Helm repo
```
helm repo list
```

- Once you have Helm ready, you can add a chart repository. Check Artifact Hub for available Helm chart repositories.
```
helm repo add bitnami https://charts.bitnami.com/bitnami

# List repos
helm repo list
```

- Once this is installed, you will be able to list the charts you can install:
```
helm search repo apache

helm search repo mysql
helm search repo mysql --versions

helm search repo bitnami
```

- Remove a repo
```
helm repo remove bitnami


# List repos
helm repo list
```
### 04. Install an Example Chart

```

```

### 05. List all the deployed Chart releases

```
helm list
```

### 06. Uninstall a Release

```
helm uninstall mysql-1612624192
```

- This will uninstall mysql-1612624192 from K8s, which will remove all resources associated with the release as well as the release history.
- If the flag *-keep-history* is provided, release history will be kept.
- You will be able to request information about that release:

```
helm status mysql-1612624192
```

- Because Helm tracks your releases even after you've uninstalled them, you can audit a cluster's history, and even undelete a release (with helm rollback).

### ASSINGMENT - Pushing a Helm chart to an Amazon ECR private repository


### Abbreviations

- OCI - Open Container Initiative
  
- 
