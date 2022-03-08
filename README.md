# Kustomize Manifests for Kubeflow on ppc64le
A repository for Kustomize manifests for Kubeflow on IBM Power (ppc64le).
These manifests base on the [official Kubeflow manifests](http://www.github.com/kubeflow/manifests/).

## Supported Distributions
- Red Hat OpenShift v4.5-v4.8 on ppc64le
- Vanilla Kubernetes v1.17-v1.21 on ppc64le

### Pre-Requisites for Vanilla Kubernetes
Ensure that your ```kube-apiserver``` is initialized with settings for certificates to work, e.g.:
```
service-account-issuer: kubernetes.default.svc,
service-account-signing-key-file: /etc/kubernetes/ssl/sa.key
```
(see https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

Depending on your k8s distributions, paths can be different, e.g., for minikube set:
```
--extra-config=apiserver.service-account-signing-key-file=/var/lib/minikube/certs/sa.key
--extra-config=apiserver.service-account-issuer=kubernetes.default.svc 
```

## Supported Kubeflow Versions
Please select appropriate tag:
- v1.3.0
- main (v1.4.1 preview)

## Install
```
# select [main|v1.3.0]
KUBEFLOW_VERSION=main
wget https://raw.githubusercontent.com/lehrig/kubeflow-ppc64le-manifests/${KUBEFLOW_VERSION}/install_kubeflow.sh
source install_kubeflow.sh
```
## Uninstall
```
# select [main|v1.3.0]
KUBEFLOW_VERSION=main
wget https://raw.githubusercontent.com/lehrig/kubeflow-ppc64le-manifests/${KUBEFLOW_VERSION}/uninstall_kubeflow.sh
source uninstall_kubeflow.sh
```

## Release
1. Get relevant image updates from [official Kubeflow manifests](http://www.github.com/kubeflow/manifests/):
```
kustomize build example | yq eval '.. | select(has("image")) | ."image"'
```
2. Update these files accordingly:
- base/kustomization.yaml
- overlays/k8s/kustomization.yaml
- overlays/openshift/kustomization.yaml
3. Manually update:
- base/rewire-images-in-katib-config.yaml
- base/pipeline-params.env
- base/workflow-controller-deployment-patch.yaml
- base/rewire-inference-cm.yaml
- overlays/k8s/rewire-istio-images.yaml

