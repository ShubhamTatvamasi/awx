# awx

https://github.com/ansible/awx-operator

clone the awx-operator repo:
```bash
git clone https://github.com/ansible/awx-operator.git
cd awx-operator
```

Checkout to the latest version:
```bash
git checkout 0.20.0
```

Deploy awx-operator:
```bash
make deploy
```

change default namespace to awx:
```bash
kubectl config set-context --current --namespace=awx
```

install awx:
```bash
kubectl apply -f - << EOF
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
spec:
  service_type: nodeport
EOF
```

Set service as `LoadBalancer`:
```bash
kubectl patch svc awx-service \
  --patch='{"spec": {"type": "LoadBalancer"}}'
```

ID: `admin`, get password:
```bash
kubectl get secret awx-admin-password -o jsonpath='{.data.password}' | base64 -d ; echo
```

Get service:
```bash
kubectl get svc
```

### CRD

get awx deployment:
```bash
kubectl get awx
```

delete awx deployment:
```bash
kubectl delete awx awx
```
