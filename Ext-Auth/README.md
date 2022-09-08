For any filter that calls an external service, requires to define a control plane extension CRD in envoy gateway to define the external service. Then from the related filter, the external services can be referred.

```yaml
kind: HTTPRoute
apiVersion: gateway.networking.k8s.io/v1alpha2
metadata:
  name: bookinfo
spec:
  parentRefs:
    - name: eg-gw
  hostnames:
    - "bookinfo.com"
  rules:
    - filters:
        - type: ExtensionRef
          extensionRef:
            - group: gateway.envoy.io
              kind: ExtAuth
              name: enforcerAuthFilter
      backendRefs:
        - name: productpage
          port: 8080
---
# Envoy external auth filter
apiVersion: gateway.envoy.io/v1alpha1
kind: ExtAuth
metadata:
  name: enforcerAuthFilter
spec:
  grpc_service:
    group: gateway.envoy.io
    kind: ControlPlane
    name: enforcer
  transport_api_version: v3
  failure_mode_allow: true
  clear_route_cache: true
  status_on_error: 401
---
# Reference to the external service via Control plane extension CRD
apiVersion: gateway.envoyproxy.io/v1alpha1
kind: ControlPlane
metadata:
  name: enforcer
spec:
  hosts:
  - enforcer.prod.svc.cluster.local
  protocol: GRPC
  trafficPolicy:
    loadBalancer:
      simple: ROUND_ROBIN
```