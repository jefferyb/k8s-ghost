# Ghost on Kubernetes

Install Ghost on Kubernetes using [kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/), or just copy `ghost.yaml` and edit it before applying it

# Deploy Ghost with kustomize | example

Create/Update `kustomization.yaml` file and view the Deployment with:

```bash
kubectl kustomize .
```

To apply/deploy, run:

```bash
kubectl kustomize . | kubectl apply -f -
#  OR
kubectl apply --kustomize .
```

ref: 
  * https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/
