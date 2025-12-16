# EKS Logging Stack (EFK with ECK Operator)

This repository contains the Kubernetes manifests required to deploy a production-ready logging stack on Amazon EKS using **Elasticsearch, Fluent Bit, and Kibana**. It leverages the **Elastic Cloud on Kubernetes (ECK) Operator** for automated management and security.

---

## 📋 Prerequisites

Before deploying the manifests, you must have an EKS cluster running and the **ECK Operator** installed.

### 1. Install the ECK Operator
The Operator manages the lifecycle of Elasticsearch and Kibana resources cluster-wide.

* **Install Custom Resource Definitions (CRDs):** These teach your Kubernetes cluster what an Elasticsearch and Kibana resource looks like.
    ```bash
    kubectl create -f [https://download.elastic.co/downloads/eck/3.2.0/crds.yaml](https://download.elastic.co/downloads/eck/3.2.0/crds.yaml)
    ```
* **Install the Operator:** This deploys the actual Operator logic (the controller) into the `elastic-system` namespace.
    ```bash
    kubectl apply -f [https://download.elastic.co/downloads/eck/3.2.0/operator.yaml](https://download.elastic.co/downloads/eck/3.2.0/operator.yaml)
    ```
* **Verify Operator Status:** Wait until the Operator pod is running.
    ```bash
    kubectl get -n elastic-system pods
    ```

### 2. AWS EBS CSI Driver
Ensure the AWS EBS CSI Driver is installed and the IAM policy `AmazonEBSCSIDriverPolicy` is attached to your Node Instance Role so that Persistent Volumes can be provisioned.

---

## 🚀 Deployment Order

To ensure all dependencies are met, execute the files in the following order:

### Step 1: Core Infrastructure
Create the namespace and storage configuration.
```bash
kubectl apply -f namespace.yaml
kubectl apply -f storageClass.yaml
kubectl apply -f elasticsearch.yaml
kubectl apply -f kibanaService.yaml
kubectl apply -f kibanaDeployment.yaml
kubectl apply -f fluent-bit-RBAC.yaml
kubectl apply -f fluent-bit-config.yaml
kubectl apply -f fluent-bit-DaemonSet
