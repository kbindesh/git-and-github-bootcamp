# EKS

- Install eksctl, kubectl, aws cli

```
kubectl version --client

# If you ever need to update the eks cluster config
aws eks update-kubeconfig --region region-code --name my-cluster

# Create EKS cluster
eksctl create cluster --name=labekscluster --region=us-east-1 --zones=us-east-1a,us-east-1b --without-nodegroup

# Get List of clusters
eksctl get cluster

# Create IAM Open ID Connect provider for EKS cluster | Replace the region & cluster name value with specs
eksctl utils associate-iam-oidc-provider --region us-east-1 --cluster labekscluster --approve

# Create a Node group

eksctl create nodegroup --cluster=labekscluster --region=us-east-1 --name=eksdemo1-ng-public1 \
--node-type=t3.small --nodes=2 --nodes-min=2 --nodes-max=4 --node-volume-size=20 --ssh-access \
--ssh-public-key=binWinWebServerKey --managed --asg-access --external-dns-access --full-ecr-access \
--appmesh-access --alb-ingress-access
```
## After EKS Cluster creation

```
# List EKS clusters
eksctl get cluster

# List NodeGroups in a cluster
eksctl get nodegroup --cluster=<clusterName>

# List Nodes in current kubernetes cluster
kubectl get nodes -o wide

# The kubectl context should be automatically changed to new cluster
kubectl config view --minify
```

## Verify the default Namespaces and Resources

```
kubectl get ns - 4 namespaces will be there

kubectl get pods

# List all the resources present in kube-system
kubectl get all -n kube-system
```
