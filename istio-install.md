echo 'export ISTIO_VERSION="1.17"' >> ${HOME}/.bash_profile
source ${HOME}/.bash_profile

cd ~/environment
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=${ISTIO_VERSION} sh -

cd ${HOME}/environment/istio-${ISTIO_VERSION}

sudo cp -v bin/istioctl /usr/local/bin/

istioctl version --remote=false


yes | istioctl install --set profile=demo

kubectl -n istio-system get svc
kubectl -n istio-system get pods


kubectl create namespace bookinfo
kubectl label namespace bookinfo istio-injection=enabled
kubectl get ns bookinfo --show-labels

kubectl -n bookinfo apply \
  -f ${HOME}/environment/istio-${ISTIO_VERSION}/samples/bookinfo/platform/kube/bookinfo.yaml

kubectl -n bookinfo get pod,svc

kubectl -n bookinfo \
 apply -f ${HOME}/environment/istio-${ISTIO_VERSION}/samples/bookinfo/networking/bookinfo-gateway.yaml

export GATEWAY_URL=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo "http://${GATEWAY_URL}/productpage"

# Create the default destination rules
kubectl -n bookinfo apply \
  -f ${HOME}/environment/istio-${ISTIO_VERSION}/samples/bookinfo/networking/destination-rule-all.yaml
kubectl -n bookinfo get destinationrules -o yaml

# Route traffic to one version of a service
kubectl -n bookinfo \
  apply -f ${HOME}/environment/istio-${ISTIO_VERSION}/samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl -n bookinfo get virtualservices reviews -o yaml


