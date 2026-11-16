# Observability Stack - Prometheus and Grafana on Amazon EKS

In this lab, you will deploy the complete observability stack (Prometheus, Grafana, and Alertmanager) onto an Amazon EKS cluster using the official kube-prometheus-stack Helm chart. You will also configure persistent storage so metrics survive pod restarts, and expose both web UIs securely to the internet.
- Role: DevOps / Platform Engineer
- Estimated Time: 30–40 Minutes

## Prerequisites & Cluster Dependencies

Before deploying the stack, ensure your environment and EKS cluster meet these requirements. In production EKS clusters, missing storage or ingress controllers are the two most common reasons monitoring deployments fail.

### Client Workstation Prerequisites

- AWS CLI (v2): Configured with IAM permissions to manage EKS, EC2, ELB, and EBS (aws sts get-caller-identity verified).
- kubectl: Matching your EKS Kubernetes version.
- Helm (v3+): Installed locally (helm version).
- eksctl: Installed to simplify IAM Role for Service Accounts (IRSA) creation.

### EKS Cluster Dependencies

1. Active Amazon EKS Cluster: At least 2 worker nodes (recommended t3.medium or larger) with public and private subnets tagged properly for load balancers (kubernetes.io/role/elb = 1 on public subnets).
2. OIDC Provider Enabled: Required for IAM Roles for Service Accounts (IRSA) so cluster add-ons can talk to AWS APIs securely.
   - eksctl utils associate-iam-oidc-provider --region us-east-1 --cluster labekscluster --approve 
4. Amazon EBS CSI Driver (Critical Storage Dependency):
   - Prometheus and Grafana require PersistentVolumeClaims (PVCs) to store time-series metrics and dashboards.
   - On modern EKS clusters, EBS volumes will not provision automatically unless the Amazon EBS CSI Driver add-on is installed. Without it, Prometheus pods remain stuck in Pending state.
   - eksctl create iamserviceaccount --name ebs-csi-controller-sa --namespace kube-system --cluster labekscluster --role-name AmazonEKS_EBS_CSI_DriverRole --role-only  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy --approve
   - eksctl create addon --cluster labekscluster --name aws-ebs-csi-driver --version latest --service-account-role-arn arn:aws:iam::154511248558:role/AmazonEKS_EBS_CSI_DriverRole --force

## Step-1: Verify Cluster Access & Storage Class

Connect kubectl to your EKS cluster and verify that a default StorageClass exists.

```bash
# Update local kubeconfig
aws eks update-kubeconfig --region <your-region> --name <your-cluster-name>

# Verify nodes are Ready
kubectl get nodes

# Verify StorageClass exists (should show gp2 or gp3)
kubectl get sc
```

(Note: Ensure the aws-ebs-csi-driver add-on is installed in your EKS Console under Add-ons before proceeding.)

## Step-2: Add the Prometheus Community Helm Repository 

Add the official CNCF prometheus-community repository, which bundles Prometheus Operator, Prometheus, Alertmanager, Grafana, node-exporter, and kube-state-metrics into a single chart.

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## Step-3: Create a Dedicated Namespace 

Isolate all monitoring components inside a dedicated monitoring namespace.

```bash
kubectl create namespace monitoring
```

## Step-4: Create a Custom values.yaml for Production Persistence

- Never deploy production monitoring with default ephemeral storage—if a pod restarts, all historical metrics and custom Grafana dashboards are lost.
- Create a file named `monitoring-values.yaml` with persistent volume claims configured for both Prometheus and Grafana:

```yaml
grafana:
  adminPassword: "ChangeMeToStrongPassword123!"
  persistence:
    enabled: true
    type: pvc
    storageClassName: gp2
    size: 10Gi

prometheus:
  prometheusSpec:
    retention: 15d
    storageSpec:
      volumeClaimTemplate:
        spec:
          storageClassName: gp2
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 50Gi
```

## Step-5: Install the kube-prometheus-stack Chart

Deploy the stack into the monitoring namespace using your custom values file:

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --values monitoring-values.yaml
```

## Step-6: Verify Pods and Persistent Volumes

Watch the pods spin up and verify that AWS dynamically provisions the EBS volumes:

```bash
# Check pod status (wait until all show Running/Ready)
kubectl get pods -n monitoring

# Verify PVCs are Bound (not Pending)
kubectl get pvc -n monitoring
```

## Step-7: (optional) Initial Local Verification (Port-Forwarding)

Before exposing anything to the internet, confirm both applications work internally via kubectl port-forward:

```bash
# Test Grafana
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80

# Open http://localhost:3000 in your browser. Log in with username admin and the password set in monitoring-values.yaml
```

```bash
# Test Prometheus
kubectl port-forward svc/monitoring-kube-prometheus-prometheus -n monitoring 9090:9090

# Open http://localhost:9090 in your browser and navigate to Status > Targets to confirm cluster metrics are being scraped.
```

## Step-8: Publishing Both Web UIs on the Internet (using AWS Classic Load Balancer | not recommended for Production)

## Step-9: Production Security Hardening Reminders


