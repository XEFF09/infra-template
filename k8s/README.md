# To complete the setup

- create: ingress as router & load-balancer

```.yml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: indiv-ingress
    namespace: backend-space
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/rewrite-target: /$2
  spec:
    ingressClassName: nginx
    rules:
      - host: localhost
        http:
          paths:
            - path: /api(/|$)(.*)
              pathType: ImplementationSpecific
              backend:
                service:
                  name: indiv-be-cluster
                  port:
                    number: 9090

```

- create: end-kustomization file in order to execute resources

```.yml

  apiVersion: kustomize.config.k8s.io/v1beta1
  kind: Kustomization

  # optional: if you want to patch any
  patches:
    - path: ./patcher/backend-cluster.yml
      target:
        kind: Service
        name: BACKEND_CLUSTER_PLACEHOLDER

  resources:
    - "./overlays/gofiber/"
    - "./overlays/postgres/"
```
