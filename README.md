# awx

deploy on default `namespace` only.

install operator:
```bash
export AWX_VERSION=0.9.0
kubectl apply -f https://github.com/ansible/awx-operator/raw/"${AWX_VERSION}"/deploy/awx-operator.yaml
```

install 
```bash
kubectl apply -f - << EOF
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
spec:
  tower_ingress_type: Ingress
EOF
```

get password:
```bash
kubectl get secret awx-admin-password -o jsonpath='{.data.password}' | base64 -d
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
