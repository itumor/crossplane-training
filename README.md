# Resources for Crossplane Training

## Setup KIND

Create kind cluster

    kind create cluster

Check if cluster is running

    kubectl get nodes

Should be something like

```
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   60s   v1.24.0
```

See https://kind.sigs.k8s.io/docs/user/quick-start/ for more info

## Setup Crossplane

See https://crossplane.io/docs/v1.9/getting-started/install-configure.html#install-crossplane

## Setup provider-aws

Prerequisite: [Crossplane CLI](https://crossplane.io/docs/v1.9/getting-started/install-configure.html#install-crossplane-cli)

Install provider using Crossplane CLI

    kubectl crossplane install provider crossplane/provider-aws:v0.29.0

Check deployment

    kubectl get deploy -n upbound-system –w


Should be something like
```
NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
crossplane-provider-aws-a2e16ca2fc1a   1/1     1            1           2m46s
```

# Create a Kubernetes secret for AWS
The provider requires credentials to create and manage AWS resources.

# Generate an AWS key-pair file
Create a text file containing the AWS account aws_access_key_id and aws_secret_access_key. The AWS documentation provides information on how to generate these keys.
```
[default]
aws_access_key_id = <aws_access_key>
aws_secret_access_key = <aws_secret_key>
```
Save this text file as aws-credentials.txt.

# Create a Kubernetes secret with AWS credentials
Use kubectl create secret -n upbound-system to generate a Kubernetes secret object inside the Kubernetes cluster.

```
kubectl create secret \
generic aws-secret \
-n upbound-system \
--from-file=creds=./aws-credentials.txt
```
