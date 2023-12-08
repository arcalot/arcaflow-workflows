# Working with a Local Kubernetes Cluster

## Get a local cluster

* [OpenShift Local](https://console.redhat.com/openshift/create/local) (formerly Code Ready Containers)
* [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)

## Issue a Certificate

*This step is not needed for a Kind cluster.*

At the time of writing, 2023-11-20, OpenShift Local (refrenced as `crc`) installation does **not** issue a TLS certificate for any of its default external users. Arcaflow only authenticates with the cluster using mutual TLS, so we need to issue a certificate to a user.

[Issue a certificate](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user) to the `kubeadmin` user with the [cluster-admin role](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles). The subject's, `subj`, Common Name denotes the user and the Organization denotes the user's permissions group.


```shell
openssl genrsa -out myuser.key 2048
openssl req -new -key myuser.key -out myuser.csr -subj "/CN=kubeadmin/O=cluster-admin
```

Create a Certificate Signing Request using a shell heredoc.

```shell
cat <<EOF | oc apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata: 
  name: kubeadmin
spec:
  request: $(base64 myuser.csr | tr -d '\n' )
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth  
EOF
```
Approve the CSR named `kubeadmin`.

```shell
oc adm certificate approve kubeadmin
```

Get your user's signed certificate.

```shell
oc get csr kubeadmin -o jsonpath='{.status.certificate}'| base64 -d > myuser.crt
```

Add the TLS certificate and private key credentials to your kubeconfig.

```shell
oc config set-credentials kubeadmin --client-key=myuser.key --client-certificate=myuser.crt --embed-certs=true
```

## Add API Client Credentials to your Arcaflow Configuration

Looking at your `kubeconfig`,
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: ...
```

Base 64 decode the value pointed to by `certificate-authority-data`. This is your certificate authority certificate bundle.

Given a Kubernetes connection configuration, `connection`, copy and paste the certificate authority certificate bundle, user's certificate, and user's private key.
```yaml
connections:
  host: api.crc.testing:6443
  cacert: |
    < copy and paste the decoded certificate authority certificate bundle here >
  cert: |
    < copy and paste your user's certificate here >
  key: |
    < copy and paste your user's private key here >
```

Then your Arcaflow configuration file could look like this.

```yaml
deployers:
  image:
    deployer_name: kubernetes
    connection:
      host: api.crc.testing:6443
      cacert: |
        < certificate authority certificate bundle >
      cert: |
        < user certificate >
      key: |
        < user private key >  
log:
  level: debug
logged_outputs:
  error:
    level: error
  success:
    level: debug           
```