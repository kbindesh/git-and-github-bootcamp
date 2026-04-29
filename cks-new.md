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

# Supply Chain Security

## Key Concepts

- Explain with the Analogy - Thief wants to enter house
  
- Vulnerabilities - A flaw, gap or weakness in the software design or architecture that leaves it open to attack. E.g. Small hole in the backside of house
  
- Exploit - A specific method or a tool or a piece of code used to take advantage of the vulnerability. It is an act of breaking-in. E.g. Using Jackhammer
  
- Payload and Post exploitation - the malicious action after the exploit has successfully breached. E.g. Steal money

## Hands-on lab for  exploiting vulnerabilities present on target machine packages

```bash
sudo apt update
sudo apt install nmap

# List packages which are listening on the target server
nmap -sV xx.xx.xx.xx
nmap -sV -Pn 34.233.133.199

[check the s/w version]

# NIST (National Institute of Standards and Technology)
# https://nvd.nist.gov/vuln/detail/CVE-2021-30047

Check on rapid7 website

# Install and Use msfconsole
sudo apt update
sudo apt install metasploit-framework

# To verify the installation
msfconsole

# Search for the exploit related to the package installed on the target machine
search OpenSSH

use exploit/multi/persistence/ssh_key

show options

set PAYLOAD <any-script>
```

# Monitoring, Logging and Runtime Security

## Install Falco
-------------------

https://falco.org/docs/setup/packages/#install-with-yum

falco --version

## Configuration and Rule Files and it's purpose
/etc/falco

## What are `FALCO rules`?
---------------------------
- https://falco.org/docs/concepts/rules/

=> Structure of Falco rules
- https://falco.org/docs/concepts/rules/basic-elements/ 

=> Priority - https://falco.org/docs/concepts/rules/basic-elements/#priority

## Lab-01: Invoking Falco rules (built-in)
============================================

```bash
sudo systemctl status falco

# get alerts based on the falco rules 
journalctl -u falco-modern-bpf.service -f


# Example-01
# Now try to access /etc/shadow to trigger notifications | on a new tab
cat /etc/shadow


# Example-02
# Create a Pod and exec into it with a bash
kubectl run nginx-pod --image=nginx:latest

kubectl get pods

kubectl exec -it nginx-pod -- bash

[Should have trigger a notification with the details]
```

## CREATING custom FALCO RULES 
/etc/falco/falco_rules.yaml | /etc/falco/falco_rules.local.yaml 
------------------------------------------------------------------------------------------

Falco rules are written in YAML format. They define security policies by structuring rules, macros, and lists, which allows for monitoring system calls and container activity.

=> Key Aspects of Falco Rules:

- YAML Syntax: Rules are highly flexible and readable, organized into rules, lists, and macros.

- Components: Each rule contains a rule name, desc (description), condition (boolean logic), output (alert message), and priority.

- Condition Language: The condition field uses a specific filtering syntax to analyze events, such as container.id != host and proc.name = bash.

- Files: Rules are typically defined in files ending in .yaml or .yml, often located in /etc/falco/falco_rules.yaml.


## Lab-02: Create a custom Falco rule that detects the use of curl inside a container

- Edit `/etc/falco/falco_rules.local.yaml` and add the following rule:

```
- rule: Curl Used in Container
  desc: Detects when curl is executed inside a container, which could indicate a web fetch or data exfiltration.
  condition: spawned_process and container and proc.name = curl
  output: "Suspicious activity detected: curl executed in container (user=%user.name container_id=%container.id container_name=%container.name command=%proc.cmdline image=%container.image.repository)"
  priority: WARNING
  tags: [network, container, mitre_exfiltration]
```

### Test the created Falco Rule
  - After adding the rule, restart the Falco service (sudo systemctl restart falco) and then execute a curl command inside any running container to verify the alert appears in your Falco logs.

```bash
# On tab-1
systemctl restart falco
systemctl status falco
journalctl -u falco-modern-bpf.service -f


# On tab-2
kubectl exec -it nginx-pod -- bash
curl google.com

[Should see the alerts getting generated on Tab-1]
```


