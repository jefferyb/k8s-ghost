# Ghost on Kubernetes

Install Ghost on Kubernetes using [kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/), or just copy `ghost.yaml` and edit it before applying it

# Deploy Ghost with kustomize | example

Create/Update `kustomization.yaml` file with ( replace 'blog.example.com' with your own url ):

```yaml
# Create a kustomization.yaml file
cat <<EOF >./kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: ghost
resources:
  - https://github.com/jefferyb/k8s-ghost.git

patches:
### Update host value
  - patch: |-
      - op: replace
        path: /spec/rules/0/host
        value: blog.example.com # Replace with your url
    target:
      group: networking.k8s.io
      version: v1
      kind: Ingress
      name: ghost

### Update url value
  - patch: |-
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: ghost
        namespace: ghost
      spec:
        template:
          spec:
            containers:
              - name: ghost
                env:
                  - name: url
                    value: 'http://blog.example.com' # Replace with your url

# ### Add TLS
#   - patch: |-
#       apiVersion: networking.k8s.io/v1
#       kind: Ingress
#       metadata:
#         name: ghost
#         namespace: ghost
#         annotations:
#           cert-manager.io/cluster-issuer: lets-encrypt
#       spec:
#         tls:
#           - secretName: ghost-ingress-tls
#             hosts:
#               - blog.example.com # Replace with your url

EOF
```
review the deployment with:

```bash
kubectl kustomize .
```

apply/deploy with run:

```bash
kubectl kustomize . | kubectl apply -f -
#  OR
kubectl apply --kustomize .
```

ref: 
  * https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/
