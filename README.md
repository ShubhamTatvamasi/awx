# awx

https://github.com/ansible/awx-operator

clone the awx-operator repo:
```bash
git clone https://github.com/ansible/awx-operator.git
```

Checkout to the latest version:
```bash
git checkout 0.16.0
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
  name: awx-demo
spec:
  service_type: nodeport
EOF
```

ID: `admin`, get password:
```bash
kubectl get secret awx-demo-admin-password -o jsonpath='{.data.password}' | base64 -d
```

ingress:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: awx-ingress-2
spec:
  rules:
  - host: awx.k8s.shubhamtatvamasi.com
    http:
      paths:
      - backend:
          service:
            name: awx-service
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
EOF
```
