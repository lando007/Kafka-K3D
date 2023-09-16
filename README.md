# Kafka-K3D

# Project Overview

This project is a rough draft for the application. It can be deployed locally at this time but might have issues with a load balancer depending on the user's environment.

## Platform Used

- Ubuntu WSL Version 0.2.1
  - [WSL Installation Guide](https://learn.microsoft.com/en-us/windows/wsl/install-manual)
- k3d version v5.4.9
  - [k3d Installation](https://k3d.io/v5.6.0/)

## Purpose

Apache Kafka is a distributed event store and stream-processing platform that is open-source, allowing for easy production and consumption of messages. This application helps deploy these services locally and test the production and consumption of each topic created.

## Installation

### Prerequisites

1. Install WSL on a Windows machine and configure it to your desired specifications.
2. Install k3d on the WSL installation and configure it as needed.

### Deployment

To create a Kubernetes environment and deploy Kafka, follow these steps:

1. Run this command to create a Kubernetes environment with k3d:

   ```shell
   k3d cluster create --api-port 6550 -p "9092:9092@loadbalancer" --agents 3. Depending on what the user wants, they can choose the value for the API port (6550) or load balancer values (9092). I have chosen these values as they were examples many other people have used, and normally Kafka uses 9092 as the port it communicates on. By running this command, it will create a lightweight Kubernetes environment for the Kafka deployment to run on.
2: Run the command kubectl apply -f zookeeper-service.yml. This will deploy the services for the ZooKeeper pod.
3: Run the command kubectl apply -f zookeeper-deployment.yml. This will deploy the ZooKeeper deployment.
4: Run the command kubectl apply -f kafka-multi-lb.yml. This will create the load balancers for each of the 3 Kafka brokers created.
5: Run the command kubectl describe service zookeeper-service and look for the IP number. This will need to be saved and used in the kafka-deployment.yml file.
6: Run the command kubectl get service. Look for the cluster IP for the service with an external-IP value and save the Cluster-IP value. This will need to be saved and used in the kafka-deployment.yml file.
7: Open the kafka-deployment file to edit and look for the line "value: {zookeeper-service}:2181" take the value you saved for the ZooKeeper IP number and replace this value. Also, replace the value load-balancer-ip in the line value: "PLAINTEXT://{load-balancer-ip}:9092" with the Cluster-IP value you saved from the Kafka LoadBalancer.
8: Deploy the Kafka brokers by running the command kubectl apply -f kafka-deployment.yml.

Produce to Brokers:
1: Download the Apache Kafka CLI https://kafka.apache.org/downloads
2: Run the command kubectl describe services and take the value in EXTERNAL-IP
3: Run the command ./kafka_2.12-3.4.0/bin/kafka-console-producer.sh --topic Test-Topic --bootstrap-server 172.20.0.3:9092 but replace the Kafka version with the one downloaded and the IP with the EXTERNAL-IP you got from step 2.
4: Open another terminal window of WSL and run the command ./kafka_2.12-3.4.0/bin/kafka-console-consumer.sh --topic test-topic --bootstrap-server 172.20.0.3:9092 --from-beginning making sure to replace the Kafka version with the one downloaded and the IP with the EXTERNAL-IP you got from step 2.
5: Start writing values into the producer terminal and see the values show up in the consumer terminal.

Steps to Debug Issues:
Run commands that have to do with kubectl describe pod or kubectl describe services.
There might also be issues with how your networking is within Windows and the k3d environment."

Produce to Brokers:
  1: Download the Apache Kafka CLI https://kafka.apache.org/downloads
  2: Run the command kubectl describe services and take the value one in EXTERNAL-IP
  3: Run the command ./kafka_2.12-3.4.0/bin/kafka-console-producer.sh --topic Test-Topic --bootstrap-server 172.20.0.3:9092 but replace the kafka version with the one downloaded and the IP with the EXTERNAL-IP you got from step 2.
  4: Open Another terminal window of WSL and run the command ./kafka_2.12-3.4.0/bin/kafka-console-consumer.sh --topic test-topic --bootstrap-server 172.20.0.3:9092 --from-beginning making sure to reaplce the kafka version with the one downloaded and the IP with the EXTERNAL-IP you got from step 2
  5: start righting values into the producer terminal and see the values show up in the consumer terminal.

Steps to Debug Issues:
  Run commands that have to do with kubectl describe pod or kubectl describe services.
  There might also be issues with how your networking is within windows and the k3d envionment. 
    
    
        


