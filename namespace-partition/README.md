# Namespace-partition
 
**Here the various *.yaml* are briefly presented with their purpose.**

#### 1A-basic-quota

Set contraints on the numbers of resources directly from their type.

This file could be directly created with:

```
kubectl create quota my-quota --hard=pods=2,services=3,replicationcontrollers=2,resourcequotas=1,secrets=5,persistentvolumeclaims=10
```

#### 1B-basic-quota

Set constraints on the resources based on memory/cpu limits.

This file could be directly created with:

```
kubectl create quota my-quota --hard=cpu=1,memory=1G
```

N.B. 1A and 1B could be mixed. 

#### 2-pod-nginx-limited

When a resource quota like ***1B-basic-quota.yaml*** is used you'll need to set the resources on the pod (or deploy) to have it works.




