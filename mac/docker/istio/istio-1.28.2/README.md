# 基于docker部署istio
> 部署 istio-1.28.2

#### 部署
```shell

#参考文档
#https://istio.io/latest/zh/docs/setup/getting-started/

# Docker Desktop 4.46.0

# Kubernetes v1.32.2

curl -L https://istio.io/downloadIstio | sh -
cd istio-1.28.2
export PATH=$PWD/bin:$PATH

# demo-profile-no-gateways.yaml 需要修改，参考 ./demo-profile-no-gateways.yaml
istioctl install -f samples/bookinfo/demo-profile-no-gateways.yaml -y
kubectl label namespace default istio-injection=enabled

kubectl get crd gateways.gateway.networking.k8s.io &> /dev/null || \
{ kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.4.0" | kubectl apply -f -; }

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
kubectl get services
kubectl get pods
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"

kubectl apply -f samples/bookinfo/gateway-api/bookinfo-gateway.yaml
kubectl annotate gateway bookinfo-gateway networking.istio.io/service-type=ClusterIP --namespace=default
kubectl get gateway

kubectl port-forward svc/bookinfo-gateway-istio 8080:80
curl http://localhost:8080/productpage

kubectl apply -f samples/addons/prometheus.yaml
kubectl rollout status deployment/prometheus -n istio-system
kubectl apply -f samples/addons/grafana.yaml
kubectl rollout status deployment/grafana -n istio-system
kubectl apply -f samples/addons/kiali.yaml
kubectl rollout status deployment/kiali -n istio-system
istioctl dashboard kiali
for i in $(seq 1 100); do curl -s -o /dev/null "http://$GATEWAY_URL/productpage"; done


```

#### 卸载
```shell

kubectl delete -f samples/addons
istioctl uninstall -y --purge

kubectl delete namespace istio-system

kubectl label namespace default istio-injection-

kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.4.0" | kubectl delete -f -

```


