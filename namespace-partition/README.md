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

#### 3A-mem-limit-range

Since setting the limit on all pods can be inconvenient, the best solution is to set a reasonable limit that is assigned by default to all pods.
Only the pods that have special needs will have to be modified then.


#### 3B-cpu-limit-range
Same of 3A but with cpu.

#### 4-limit-range
Mix of 3A and 3B.


