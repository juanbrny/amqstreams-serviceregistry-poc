# AMQ Streams Service Registry deployment 

Deploy AMQ Streams Service Registry (Apicurio) on an AMQ Streams (Kafka) provided cluster using OpenShift Operators. 

## Prerequisites
We'll need a Kafka cluster. The storage backend for our Service Registry  is Kafka as it's the supported scenario in version 1.1. Future versions will add support for aditional storage backends.

In our example, both the Registry and the Kafka cluster used for storage, will share the same namespace so we won't have to deal with external access routes.

Once the cluster is ready:

 - Install AMQ-Streams operator
 - Install Service Registry operator

## Kafka cluster configuration
Use the values provided only as reference, they do work for a testing scenario but may not fit others. The number of brokers, zookeeper nodes, partitions per topics, ... should be reviewed if a more robuts deployment is needed.

## Monitoring
Monitoring will also be enabled so we can scrape metrics from Kafka. If the metrics block is present the operator will configure everything and create a service on port 9094 to access the /metrics service. 
If you are not interested on gathering metrics fell free to remove the metrics block from the yaml containing the Kafka cluster definition.

## Resources
We won't do any resource management for CPU and Memory and will just include some reference values for the Java Virtual Machine used on Kafka pods. Fell free to adjust that to your scenario. You have examples of resources block for Kafka and Zookeeper on the documentation

As this is a test, storage request are adjusted to the minimun with 10G for Kafka brokers and 5G for Zookeeper nodes. The storage can be extended later on if needed.
Having a storage class is not mandatory. Both Kafka and Zookeeper can consume ephemereal storage. Check appendix X for instructions to use ephemereal storage when deploying the cluster.

## Security
Our cluster need to support either TLS or SCRAM authentication. The example is configured to use SCRAM over the TLS endpoint.
Service Registry doesn't currently support using SCRAM over the Plain endpoint.

All required user ACLs are included. In case you deploy a small Kafka cluster just for the registry, not shared with anyone else, we can create a simple ACL with full access to all topics and groups. In our example we'll be sharing an existing cluster so we'll go with the full ACL list.


## Deployment

Main data variables used in the deployment files
- Namespace: redhat-lab
- Cluster name: event-bus
- Storage class: managed-premium
- User for registry: service-registry-kafka-user-scram
- Registry name: service-registry-scram

They can be replaced to fit other scenarios.




