# How to Add a New User in Kubernetes with Certificates

```
USER_NAME="cluster-admin"

# Generate a Private Key
openssl genrsa -out $USER_NAME.key 2048

# Create an X.509 Certificate Signing Request (CSR)
openssl req -new -key $USER_NAME.key -subj "/CN=cluster-admin/O=system:masters" -out $USER_NAME.csr

# Generate a self-signed certificate for the user
openssl x509 -req -in $USER_NAME.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -out $USER_NAME.crt

# To get the kube-api-server URL (e.g. https://10.0.0.10:6443)
kubectl config view --minify | grep server

# Add the new credentials to the kubeconfig file
kubectl config set-credentials $USER_NAME --client-certificate=$USER_NAME.crt --client-key=$USER_NAME.key --embed-certs=true

# Create a new context for the cluster-admin user
kubectl config set-context clusteradmin-context --cluster=kubernetes --user=$USER_NAME --namespace=default

# You can now generate a minimal, dedicated kubeconfig file for this user
kubectl config view --context=clusteradmin-context --minify --raw > clusteradmin.kubeconfig

# Test the Authentication
kubectl --context clusteradmin-context auth whoami
OR
kubectl auth whoami

# (optional) Switch to a particular context
kubectl config use-context clusteradmin-context
kubectl auth whoami

# Verify if you are able to query k8s objects as a cluster-admin user
kubectl --kubeconfig=clusteradmin.kubeconfig get pods
```

# How to Add a New User in Kubernetes with Certificates & RBAC

```
USER_NAME="rbac-labuser"

# Generate a Private Key
openssl genrsa -out $USER_NAME.key 2048

# Create an x509 Certificate Signing Request (CSR)
openssl req -new -key $USER_NAME.key -subj "/CN=rbac-labuser/O=developers" -out $USER_NAME.csr

# Encode the CSR in Base64 | Will need it for CertificateSigningRequest Kubernetes resource
cat $USER_NAME.csr | base64 | tr -d "\n"

# Create a Kubernetes CSR Resource (rbac-labuser-csr.yaml)
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: rbac-labuser
spec:
  request: <paste-one-line-base64-here>
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 864000  # 10 days
  usages:
  - client auth

# Apply the above k8s manifest
kubectl apply -f mary-csr.yaml

# review and approve the csr
kubectl get csr
kubectl certificate approve rbac-labuser

# Retrieve the Certificate
kubectl get csr/rbac-labuser -o yaml

# Export the issued certificate to a .crt file
kubectl get csr rbac-labuser -o jsonpath='{.status.certificate}'| base64 -d > rbac-labuser.crt

# Configure the rbac-labuser User in Kubeconfig
kubectl config set-credentials $USER_NAME --client-certificate=$USER_NAME.crt --client-key=$USER_NAME.key --embed-certs=true

# Create a new context for the rbac-labuser user
kubectl config set-context $USER_NAME-context --cluster=kubernetes --user=$USER_NAME --namespace=default

# Switch to a particular context and test
kubectl config use-context $USER_NAME-context
kubectl auth whoami

# Create a Role
kubectl create role developer --verb=create --verb=get --verb=list --verb=update --verb=delete --resource=pods

# Create a RoleBinding
kubectl create rolebinding dev-binding-rbaclu --role=developer --user=$USER_NAME

# Test the Authorization
kubectl --context $USER_NAME-context get pods
```

## ROLE & Rolebinding manifests (with user & with groupOfUsers)

- ROLE
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-manager-role
  # Replace 'default' with the namespace where this role should apply
  namespace: default 
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["create", "update", "delete"]
```

- ROLEBINDING WITH USER
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-jane
  namespace: default # The namespace where the permissions are granted
subjects:
- kind: User # Specifies the subject type (User, Group, or ServiceAccount)
  name: jane # The name of the user (case-sensitive)
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role # Must be Role or ClusterRole
  name: pod-reader # The name of the Role (or ClusterRole) to bind to the user
  apiGroup: rbac.authorization.k8s.io
```

- ROLEBINDING WITH A GROUP 
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: view-rolebinding-to-group # Name of the RoleBinding resource
  namespace: default               # Namespace where this binding is effective
subjects:
- kind: Group                      # Specifies the subject is a group
  name: view-only-group            # Name of your group (as recognized by your auth provider)
  apiGroup: rbac.authorization.k8s.io # Required for Group subjects
roleRef:
  kind: ClusterRole                # The type of role being referenced (can be 'Role' or 'ClusterRole')
  name: view                       # Name of the Role or ClusterRole to bind
  apiGroup: rbac.authorization.k8s.io # API group the role belongs to
```
