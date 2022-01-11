## EFK Setup

 1. logs can be accessed at: https://logs.sariska.io
   

## Create a Namespace logging
    1. kubectl create ns logging

## Install helm
    * Install: (brew install helm) https://helm.sh/docs/intro/install/ 
    * Configure KUBECONFIG evn variable into your env variable: https://stackoverflow.com/a/45276283 

## Deploy loggin-operator
    
    overwrite default values of values.yaml
    1. change values.yaml
    2. helm package .
    3. helm install logging-operator -f values.yaml .
    4. helm repo add banzaicloud-stable https://kubernetes-charts.banzaicloud.com
    5. helm repo update
    6. helm upgrade --install --wait --create-namespace --namespace logging logging-operator banzaicloud-stable/logging-operator
Note:- Deleting a helm chart command (loggin-operator is the release name, Also use -n command)

## Deploy Manifest

1. kubectl apply -k .
 
## Quick Guide

1. https://banzaicloud.com/docs/one-eye/logging-operator/quickstarts/es-nginx/


## Useful Command

1. kubectl get secret quickstart-es-elastic-user -n logging-test -o go-template='{{.data.elastic | base64decode}}'
2. kubectl port-forward service/quickstart-es-http -n logging-test 9200
3. kubectl port-forward service/quickstart-kb-http -n logging-test 5601
4. kubectl get secret quickstart-es-elastic-user  -n logging-test o=jsonpath='{.data.elastic}' | base64 --decode; echo

## Enriching the logs

  For extra filters and log formatting please use below filters
 
1. https://banzaicloud.com/docs/one-eye/logging-operator/configuration/plugins/filters/


## Important resources
* https://logz.io/blog/fluentd-vs-fluent-bit/
* https://github.com/srinisbook/kubernetes-efk-stack/tree/master/manifests
* https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/common_types/
* https://github.com/banzaicloud/logging-operator/blob/master/config/crd/bases/logging.banzaicloud.io_loggings.yaml
* https://stackoverflow.com/questions/52009124/not-able-to-completely-remove-kubernetes-customresource
* https://techblog.cisco.com/blog/k8s-logging-tls/

