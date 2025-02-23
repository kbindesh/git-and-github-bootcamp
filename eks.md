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

kubectl get all

# List all the resources present in kube-system
kubectl get all -n kube-system
```

## Service

- ClusterIP
- NodePort
- LoadBalancer
- ExternalName
  
### Lab: Deploy sample application - 2 deployments, 1 clusterip svc, 1 nodeport svc 

- **Nginx Application Deployment**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
selector:
  matchLabels:
    app: nginx
replicas: 2
template:
  metadata:
    labels:
      app: nginx
  spec:
    containers:
      - name: nginx
        image: nginx
        ports:
          - containerPort: 80
```

- **ClusterIP Service for the above Pods**

```
apiVersion: v1
kind: Service
metadata:
  name: bin-cip-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
```

## STORAGE MANAGEMENT

- emptyDir

```
apiVersion: v1
kind: Pod
metadata:
  name: emptydir-vol-pod
spec:
  restartPolicy: Never
  volumes:
  - name: shared-data
    emptyDir: {}
  containers:
  - name: busybox-container
    image: busybox
    command: ["/bin/sh","-c","while true; do sleep 3600; done"]
    volumeMounts:
    - name: shared-data
      mountPath: /tmp/data
  - name: nginx-container
    image: nginx
    volumeMounts:
    - name: shared-data
      mountPath: /data
```

- CSI ephemeral storage

```
kind: Pod
apiVersion: v1
metadata:
  name: my-csi-pod
spec:
containers:
- name: my-frontend
  image: busybox
  volumeMounts:
  - mountPath: "/data"
    name: my-csi-vol
  command: ["sleep", "1000000"]
  volumes:
  - name: my-csi-vol
    csi:
      driver: inline.storage.kubernetes.io
      volumeAttributes:
        foo: bar
```  
