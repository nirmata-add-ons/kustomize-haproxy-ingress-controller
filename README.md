# Haproxy Ingress Controller

[Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) controller
implementation for [HAProxy](http://www.haproxy.org/) loadbalancer.


HAProxy Ingress is a Kubernetes ingress controller: it configures a HAProxy instance
to route incoming requests from an external network to the in-cluster applications.
The routing configurations are built reading specs from the Kubernetes cluster.
Updates made to the cluster are applied on the fly to the HAProxy instance.

## Use HAProxy Ingress

**Documentation:**

* Getting started guide: [/docs/getting-started/](https://haproxy-ingress.github.io/docs/getting-started/)
* Global and per ingress/service configuration keys: [/docs/configuration/keys/](https://haproxy-ingress.github.io/docs/configuration/keys/)
* Command-line options: [/docs/configuration/command-line/](https://haproxy-ingress.github.io/docs/configuration/command-line/)

**Supported versions:**

| HAProxy Ingress   | Supported<br/>Kubernetes | 
|-------------------|--------------------------|
| [`v0.13.6`]       |       `1.19+`            |                 

**Steps to Backup existing haproxy:**
<br />
mkdir backup-ingrees <br />
cd backup-ingrees <br />
<br />
kubectl get cm haproxy-configmap -oyaml -n ingress-haproxy > haproxy-configmap.yaml <br />
kubectl get sa haproxy-ingress-serviceaccount -oyaml -n ingress-haproxy > haproxy-ingress-serviceaccount.yaml <br />
kubectl get clusterrole haproxy-ingress-clusterrole -oyaml -n ingress-haproxy > haproxy-ingress-clusterrole.yaml <br />
kubectl get role haproxy-ingress-role -oyaml -n ingress-haproxy > haproxy-ingress-role.yaml <br />
kubectl get rolebinding haproxy-ingress-role-nisa-binding -oyaml -n ingress-haproxy > haproxy-ingress-role-nisa-binding.yaml <br />
kubectl get clusterrolebinding haproxy-ingress-clusterrole-nisa-binding -oyaml -n ingress-haproxy > haproxy-ingress-clusterrole-nisa-binding.yaml <br />
kubectl get deploy haproxy-ingress -oyaml -n ingress-haproxy > haproxy-ingress-deploy.yaml <br />
kubectl get deploy ingress-default-backend -n ingress-haproxy -oyaml > ingress-default-backend.yaml <br />
kubectl get svc ingress-default-backend -n ingress-haproxy -oyaml > ingress-default-backend-svc.yaml <br />
kubectl get svc haproxy-ingress -n ingress-haproxy -oyaml > haproxy-ingress-svc.yaml <br />
<br />

**Steps to delete haproxy:**

kubeconfig=$1 <br />
name=$2 <br />
namespace=$3 <br />
kubectl --kubeconfig=$kubeconfig  get crd | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig delete crd <br />
kubectl --kubeconfig=$kubeconfig  get clusterrole | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig delete clusterrole <br />
kubectl --kubeconfig=$kubeconfig get clusterrolebinding | grep -i $2 |  awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig delete clusterrolebinding <br />
kubectl --kubeconfig=$kubeconfig get mutatingwebhookconfigurations | grep -i $2 |  awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig delete mutatingwebhookconfigurations <br />
kubectl --kubeconfig=$kubeconfig get validatingwebhookconfigurations | grep -i $2 |  awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig delete validatingwebhookconfigurations <br />
kubectl --kubeconfig=$kubeconfig get sa -n $namespace | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete sa <br />
kubectl --kubeconfig=$kubeconfig get role -n $namespace | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete role <br />
kubectl --kubeconfig=$kubeconfig get rolebinding -n $namespace | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete rolebinding <br />
kubectl --kubeconfig=$kubeconfig get cm -n $namespace | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete cm <br />
kubectl --kubeconfig=$kubeconfig get deploy -n $namespace | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete deploy <br />
kubectl --kubeconfig=$kubeconfig get svc -n $namespace | grep -i $2 | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete svc <br />
kubectl --kubeconfig=$kubeconfig get deploy -n $namespace | grep -i ingress-default-backend | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete deploy <br />
kubectl --kubeconfig=$kubeconfig get svc -n $namespace | grep -i ingress-default-backend | awk {'print $1'} | xargs kubectl --kubeconfig=$kubeconfig -n $namespace delete svc <br />

<br>

**You can set values as below:** <br>
name=haproxy <br>
namespace=ingress-haproxy <br>
