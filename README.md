# k8s-demo-app-rocketchat  

## What's this?
Sample kubernetes manifests of "Rocket.Chat".  
https://rocket.chat/  
## How to use
- **rocketchat-app-nodeport.yaml & rocketchat-db-emptydir.yaml**  
For "eqipmentless" k8s clusters, or no persistent volume and loadbalancer.  
You access to the worker node where the rocketchat app Pod is running, with nodePort 31000:  
[http://*$YOUR_WORKER_NODE_IP*:31000](http://*$YOUR_WORKER_NODE_IP*:31000)  
  
- **rocketchat-app-loadbalancer.yaml & rocketchat-db-pvc.yaml**  
If your cluster supports dynamic volume provisioning and loadbalancer services, you use these manifests.  
Please make appropriate changes to your environment before applying the manifests.  
  - "storageClassame" of rocketchat-db-pvc.yaml  
  - "loadBalancerIP" and "ROOT_URL" of rocketchat-app-loadbalancer.yaml  

In either case, please chanege the namespaces of rocketchat-app deployment and mongo-rsinit job as follows.  
rocketchat-db-0.rocketchat-db.*$YOUR_NAMESPACE*.svc.cluster.local
  
## Note
- The number of replicas of mongodb is limited to 1.  
- mongo-rsinit job executes mongo rs.initiate() command to the mongodb deployment from outside.  
It's a lillte bit tricky so if you know a better way please reach out to me.  
Of course, you can do this manually.  

  ```
  $ kubectl exec -it rocketchat-db-0 -- bash    
  $ mongo  
  $ > rs.initiate()
  ```

  For more information, please read the official document below.  
  https://docs.mongodb.com/manual/tutorial/convert-standalone-to-replica-set/
