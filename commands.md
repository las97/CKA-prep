# Commands
```
Kubectl get pods —selector label=app
kubectl get pods --show-labels
kubectl get all --show-labels
Kubectl get pods —no-headers
Kubectl get all —selector label=app --no-headers
Kubectl api-resources
Kubectl replace  —force -f 1.yaml	
Kubectl create -f . #all files from folder
Kubectl get pods —watch

Kubectl top node
Kubectl top pod
kubectl top nodes --sort-by=cpu
kubectl top node --sort-by=memory  --no-headers

kube-apiserver --help | grep -i admission
kubectl config view --kubeconfig=/root/my-kube-config

kubectl get pv -o json | jq '.items[0]'
kubectl get pv -o json | jq '.items[0].spec.capacity'
kubectl get pv --sort-by=kubectl get pv --sort-by=.spec.capacity.storage
kubectl get pv --sort-by=.spec.capacity.storage  -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage > /opt/outputs/pv-and-capacity-sorted.txt
kubectl config view --kubeconfig=my-kube-config -o jsonpath="{.contexts[?(@.context.user=='aws-user')].name}" > /opt/outputs/aws-context-name

Kubectl rollout status deployment 
Kubectl rollout history deployment 
Kubectl rollout undo deployment

kubectl create deployment blue --image=nginx --replicas=3
kubectl label nodes node01 color=blue
kubectl get nodes --show-labels | grep blue
```

# CKA
```
kubectl set resources deployment wordpress --limits=cpu=200m,memory=512Mi --requests=cpu=100m,memory=256Mi

kubectl label namespace calico-system \
  pod-security.kubernetes.io/enforce=privileged \
  pod-security.kubernetes.io/warn=privileged \
  pod-security.kubernetes.io/audit=privileged \
  --overwrite
```
# Custom columns
```
kubectl -n admin2406 get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name > /opt/admin2406_data
```

# Show rollout history
```
kubectl rollout history deployment myapp -n production
```

# Annotate last deployment - add comments. change-cause
```
kubectl annotate deployment myapp kubernetes.io/change-cause="updated image to nginx:1.17" -n production
```

# Get resource quotas
```
Kubectl describe resourcequota
```

# patching
```
kubectl get deployment resource-app -n patch-ns -o json
```

# Run nslookup from pod ot another pod
```
cluster1-controlplane ~ ➜   kubectl get pod nginx-resolver-cka06-svcn -o wide
cluster1-controlplane ~ ➜   IP=`kubectl get pod nginx-resolver-cka06-svcn -o wide --no-headers | awk '{print $6}' | tr '.' '-'`
cluster1-controlplane ~ ➜   kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup $IP.default.pod > /root/CKA/nginx.pod.cka06.svcn
```

# Node Info
```
student-node ~ ➜  ssh cluster1-controlplane "kubectl top node --no-headers | sort -nr -k2 | head -1"
 cluster1-controlplane   127m   1%    703Mi   1%    
student-node ~ ➜  ssh cluster3-controlplane "kubectl top node --no-headers | sort -nr -k2 | head -1"
cluster3-controlplane   577m   7%    1081Mi   1%    
student-node ~ ➜  ssh cluster4-controlplane "kubectl top node --no-headers | sort -nr -k2 | head -1"
cluster4-controlplane   130m   1%    679Mi   1%    
```

# Nginx ssl-redirect false
```
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false”
```

# envFrom/cm
    spec:
      containers:
      - image: kodekloud/webapp-color
        name: webapp-color-wl10
        envFrom:
        - configMapRef: 
            name: webapp-wl10-config-map
# RBAC
```
kubectl -n project-hamster create role processor --verb=create --resource=secret --resource=configmap
```

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: processor
  namespace: project-hamster
rules:
- apiGroups:
  - ""
  resources:
  - secrets
  - configmaps
  verbs:
  - create
```

# CRD
```
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: course-issuer
spec:
  selfSigned:
    crlDistributionPoints:               # ADD
    - http://example.com/crl             # ADD
```

# Create namespace
```
kubectl create namespace argocd
```

# Add repo and template manifests (CRDs not installed)
```
helm get manifest
helm repo add argocd https://argoproj.github.io/argo-helm
helm repo update

helm search repo argocd
```

# To get the crd value to change, you can run: 
```
helm show values argocd/argo-cd --version 7.7.3 | grep -i crd -C 4

helm install argocd argocd/argo-cd --version 7.7.3 --set crds.install=false --namespace argocd
helm template argocd argocd/argo-cd --version 7.7.3 --set crds.install=false --namespace argocd > /root/argo-helm.yaml
Prod
```

# Show pods that will be destroyed first. Besteffort QoS
```
kubectl get pods -o custom-columns="NAME:.metadata.name,QOS:.status.qosClass" --no-headers | grep BestEffort | awk '{print $1}'
```
