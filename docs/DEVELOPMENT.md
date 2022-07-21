# Development

## Debug

### Debug Kustomize controller

From the [repository](https://github.com/fluxcd/kustomize-controller):

```
kubectl -n flux-system port-forward svc/source-controller 8082:80 >/dev/null 2>&1 &
export SOURCE_CONTROLLER_LOCALHOST=localhost:8082
kubectl -n flux-system scale deployment/kustomize-controller --replicas=0
go run main.go \
  --default-service-account=gitops-reconciler \
  --no-cross-namespace-refs=true \
  --watch-all-namespaces=true \
  --log-level=info \
  --log-encoding=json \
  --no-remote-bases=true \
  --metrics-addr=:8089
```

### Debug Capsule Proxy

From the [repository](https://github.com/clastix/capsule-proxy):

```
# Assuming Capsule Proxy is running in-cluster
kubectl get secret -n flux-system capsule-proxy -o=jsonpath='{.data.tls\.key}' | base64 -d > tls.key
kubectl get secret -n flux-system capsule-proxy -o=jsonpath='{.data.tls\.crt}' | base64 -d > tls.crt
kubectl -n flux-system scale deployment/capsule-proxy --replicas=0
go run main.go \
  --zap-log-level=10 \
  --listening-port=9001 \
  --capsule-configuration-name=default \
  --rolebindings-resync-period=10h \
  --enable-ssl=true \
  --ssl-cert-path=./tls.crt \
  --ssl-key-path=./tls.key \
  --oidc-username-claim=preferred_username
```
