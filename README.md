## EFK Setup



## Setup Contains
 - Curator to clear logs after some interval
 - Elasticsearch for database
 - Enterprise search 
 - Fluentd and Fluentbit for logs prosessor and logs collector
 - Kibana for visualization
 - scalable setup 


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


## Generate ssl certificate for enterprise search:

## step 1: generate keystore.jks

https://support.globalsign.com/digital-certificates/digital-certificate-installation/java-keytool-create-keystore

## step 2: self sign certificate


https://www.elastic.co/guide/en/enterprise-search/current/configure-ssl-tls.html#configure-ssl-tls-custom-ca-server-requests

## Deploy Manifest

1. kubectl apply -k .
 

## Clear Elasticsearch index 

 curl -X DELETE 'http://localhost:9200/_all'

 
## Elastic operator CRD
 
https://www.elastic.co/guide/en/cloud-on-k8s/2.5/k8s-deploy-eck.html
https://github.com/elastic/cloud-on-k8s/tree/2.5/config/samples
https://www.elastic.co/guide/en/cloud-on-k8s/2.5/k8s-deploy-eck.html
## Restart fluentbit daemonsets  

 curl -X DELETE 'http://localhost:9200/_all'

## Quick Guide everything

1. https://banzaicloud.com/docs/one-eye/logging-operator/quickstarts/es-nginx/


## Useful Command

1. kubectl get secret quickstart-es-elastic-user  -n logging -o go-template='{{.data.elastic | base64decode}}'
2. kubectl port-forward service/quickstart-es-http -n logging-test 9200
3. kubectl port-forward service/quickstart-kb-http -n logging-test 5601



## Custom Resource Definition

You can define `outputs` (destinations where you want to send your log messages, for example, Elasticsearch, or and Amazon S3 bucket), and `flows` that use filters and selectors to route log messages to the appropriate outputs. You can also define cluster-wide outputs and flows, for example, to use a centralized output that namespaced users cannot modify.

You can configure the Logging operator using the following Custom Resource Definitions.

- [logging](https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/logging_types/) - Represents a logging system. Includes `Fluentd` and `Fluent-bit` configuration. Specifies the `controlNamespace`. Fluentd and Fluent-bit will be deployed in the `controlNamespace`
- [output](https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/output_types/) - Defines an Output for a logging flow. This is a namespaced resource. See also `clusteroutput`.
- [flow](https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/flow_types/) - Defines a logging flow with `filters` and `outputs`. You can specify `selectors` to filter logs by labels. Outputs can be `output` or `clusteroutput`.  This is a namespaced resource. See also `clusterflow`.
- [clusteroutput](https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/clusteroutput_types/) - Defines an output without namespace restriction. Only effective in `controlNamespace`.
- [clusterflow](https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/output_types/) - Defines a logging flow without namespace restriction.

## Enriching the logs

  For extra filters and log formatting please use below filters
 
1. https://banzaicloud.com/docs/one-eye/logging-operator/configuration/plugins/filters/


## Troubleshooting fluentd

   1.  https://banzaicloud.com/docs/one-eye/logging-operator/operation/troubleshooting/fluentd/

## Troubleshooting fluentbit

   1. https://banzaicloud.com/docs/one-eye/logging-operator/operation/troubleshooting/fluentbit/

## Interacting with elasticsearch

https://logs.sariska.io/app/dev_tools#/console


## Please keep monitoring ES pvc , otherwise we need to relaunch cluster

https://logs.sariska.io/app/dev_tools#/console



## Important resources
* https://logz.io/blog/fluentd-vs-fluent-bit/
* https://github.com/srinisbook/kubernetes-efk-stack/tree/master/manifests
* https://banzaicloud.com/docs/one-eye/logging-operator/configuration/crds/v1beta1/common_types/
* https://github.com/banzaicloud/logging-operator/blob/master/config/crd/bases/logging.banzaicloud.io_loggings.yaml
* https://stackoverflow.com/questions/52009124/not-able-to-completely-remove-kubernetes-customresource
* https://techblog.cisco.com/blog/k8s-logging-tls/

