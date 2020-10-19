# knative-kind
Running Knative in Kind

# How to
* Install Knative serving component
```shell
kind create cluster --name knative
kubectl apply -f https://github.com/knative/serving/releases/download/v0.18.0/serving-crds.yaml
kubectl apply -f https://github.com/knative/serving/releases/download/v0.18.0/serving-core.yaml
```

* Install Istio
```shell
yes | istioctl install

export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')

echo $INGRESS_HOST:$INGRESS_PORT
```
* Install Knative CLI
```shell
if [ ! -f ./kn ]; then
    curl -L https://storage.googleapis.com/knative-nightly/client/latest/kn-linux-amd64 --output kn
    chmox +x kn
    ./kn version
fi
```

* Create a new app with Knative
```shell
./kn service create helloworld-go --image gcr.io/knative-samples/helloworld-go --env TARGET="Go Sample v1"

./kn service describe helloworld-go
kubectl get ksvc
```
