###Install the RabbitMQ operator 

kubectl apply -f https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml

###Check if the components are healthy in the rabbitmq-system namespace

kubectl get all -o wide -n rabbitmq-system

create rabbitmqcluster.yaml
    apiVersion: rabbitmq.com/v1beta1
kind: RabbitmqCluster
metadata:
  name: production-rabbitmqcluster
spec:
  replicas: 3
  resources:
    requests:
      cpu: 500m
      memory: 0.5Gi
    limits:
      cpu: 1
      memory: 1Gi
  rabbitmq:
          additionalConfig: |
                  log.console.level = info
                  channel_max = 1700
                  default_user= guest 
                  default_pass = guest
                  default_user_tags.administrator = true
  service:
    type: LoadBalancer
------------------------------------------------------------------------    
If the above configuration file does't spin up use below

apiVersion: rabbitmq.com/v1beta1
kind: RabbitmqCluster
metadata:
  name: production-rabbitmqcluster
spec:
  replicas: 1
  rabbitmq:
    additionalConfig: "log.console.level = info\nchannel_max = 700\ndefault_user= guest \ndefault_pass = guest\ndefault_user_tags.administrator = true\n"
  service:
    type: LoadBalancer
 ------------------------------------------------------   
 
kubectl describe RabbitmqCluster production-rabbitmqcluster 

###let us explore the resources created by the RabbitMqCluster

kubectl get all -l app.kubernetes.io/part-of=rabbitmq

### get the load balancer IP

kubectl get svc production-rabbitmqcluster -o jsonpath='{.status.loadBalancer.ingress[0].ip}'

copy the ip and add port number 15762

User name and password are : guest , guest

![image](https://user-images.githubusercontent.com/82759393/196940501-93531497-9dfa-4338-9579-86ba874f9d61.png)

















# Introduction to RabbitMQ in Kubernetes
RabbitMQ is an open-source message-broker. It is the most widely deployed open-source message broker. RabbitMQ is lightweight and easy to deploy on-premises and in the cloud. Lets us look at the various features that RabbitMQ has to offer as mentioned on the official page of RabbitMQ
Supports multiple messaging Protocols like ( AMQP, STOMP, MQTT, etc ).
Distributed Development for high availability and throughput. 
Supports multiple tools and Plugins. 
An Interactive UI and an HTTP-API for managing and monitoring. 
Rich support for a variety of languages like ( Python, Java, Go, Ruby, C#, etc ).





