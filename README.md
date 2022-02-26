## EFK Setup

 1. logs can be accessed at: https://logs.sariska.io

## Architecture:

![image description](https://camo.githubusercontent.com/1826e4f1b9b8107d7e7480137c767cbb9ef9689746bad30d8b0ce7ceef75b6d0/68747470733a2f2f62616e7a6169636c6f75642e636f6d2f646f63732f6f6e652d6579652f6c6f6767696e672d6f70657261746f722f696d672f6c6f6767696e675f6f70657261746f725f666c6f772e706e67)

   
## Create a Namespace logging
    1. kubectl create ns logging

## Install helm
    * Install: (brew install helm) https://helm.sh/docs/intro/install/ 
    * Configure KUBECONFIG evn variable into your env variable: https://stackoverflow.com/a/45276283 

## Deploy loggin-operator

    1. helm repo add banzaicloud-stable https://kubernetes-charts.banzaicloud.com
    2. helm repo update
    3. helm upgrade --install --wait --create-namespace --namespace logging logging-operator banzaicloud-stable/logging-operator
Note:- Deleting a helm chart command (loggin-operator is the release name, Also use -n command)

## Deploy Manifest

1. kubectl apply -k .
 
## Quick Guide everything

1. https://banzaicloud.com/docs/one-eye/logging-operator/quickstarts/es-nginx/


## Useful Command

1. kubectl get secret quickstart-es-elastic-user  -n logging -o go-template='{{.data.elastic | base64decode}}'
2. kubectl port-forward service/quickstart-es-http -n logging-test 9200
3. kubectl port-forward service/quickstart-kb-http -n logging-test 5601

## Enriching the logs

  For extra filters and log formatting please use below filters
 
1. https://banzaicloud.com/docs/one-eye/logging-operator/configuration/plugins/filters/


## Troubleshooting fluentd

   1.  https://banzaicloud.com/docs/one-eye/logging-operator/operation/troubleshooting/fluentd/

## Troubleshooting fluentbit

   1. https://banzaicloud.com/docs/one-eye/logging-operator/operation/troubleshooting/fluentbit/

## Interacting with elasticsearch

https://logs.sariska.io/app/dev_tools#/console


## Important resources
* https://logz.io/blog/fluentd-vs-fluent-bit/
* https://github.com/srinisbook/kubernetes-efk-stack/tree/master/manifests
* https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/common_types/
* https://github.com/banzaicloud/logging-operator/blob/master/config/crd/bases/logging.banzaicloud.io_loggings.yaml
* https://stackoverflow.com/questions/52009124/not-able-to-completely-remove-kubernetes-customresource
* https://techblog.cisco.com/blog/k8s-logging-tls/

