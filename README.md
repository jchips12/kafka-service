Create zookeeper deployment and service
$ kb create -f zookeeper-deployment.yaml

Create kafka deployment and service
$ kb create -f kafka-deployment.yaml

Take note of kafka service IP address
$ kb get service | grep kafka-service

Update host machine /etc/hosts
$ sudo vi /etc/hosts
<kafka-service IP> kafka-service

Enable nginx-ingress controller
$ microk8s enable ingress

Patch nginx-ingress daemonset
$ kb -n ingress patch daemonset nginx-ingress-microk8s-controller --patch "$(cat kafka-patch-ingress-daemonset.yaml)"

Patch nginx-ingress configmap
$ kb -n ingress patch configmap nginx-ingress-tcp-microk8s-conf --patch "$(cat kafka-patch-ingress-tcp-configmap.yaml)"

## Testing
Install kafkacat

Create Consumer
$ kafkacat -C -b localhost:9092 -t test-topic

Create Producer
$ kafkacat -P -b localhost:9092 -t test-topic

Produce message
send message from producer

Consume message should be displayed from consumer terminal