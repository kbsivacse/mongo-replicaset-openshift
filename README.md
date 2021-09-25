# Creatre MongoDB replication across multiple OpenShift clusters

This project is to help creating and deployingo MongoDB cluster with replication across multiple OpenShift clusters.

## Prerequisites
1. OpenShift Cluster
2. OpenShift CLI

## Instructions
1. Create a primary MongoDB instance in cluster 1
```
# On OpenShift cluster1, run the below script
sh deploy-primary-mongo-1.sh 
```

2.Create a secondary MongoDB instance in cluster 1
```
# On OpenShift cluster1, run the below script
sh deploy-primary-mongo-2.sh 
```

3.Create a secondary MongoDB instance in cluster 2
```
# On OpenShift cluster2, run the below script
sh deploy-primary-mongo-3.sh 
```

4. Configure replica sets.
The mongo instances will be running as below,
cluster1:20017
cluster1:20018
cluster2:20017

```
#Run the following command to create the replica set:
kubectl exec $(k get po -o jsonpath='{.items[0].metadata.name}') -c mongo -- mongo --eval 'rs.initiate({_id: "rs0", version: 1, members: [ {_id: 0, host: "<cluster1>:20017"}, {_id: 1, host: "<cluster1>:20018"}, {_id: 2, host: "<cluster2>:20017"} ]});'
```

5. Now the replica set is created and you can connect it from any Mongo client
