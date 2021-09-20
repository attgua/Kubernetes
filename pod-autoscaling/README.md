# pod-autoscaling
 
**Here the various *.yaml* are briefly presented with their purpose.**

In this section it is assumed that the metric server has already been set up and is working correctly.
** The steps to implement and test an autoscaler will then be presented. **

#### Create an nginx deploy for testing

```
k create deployment nginx --image=nginx

```

#### Expose deployment

```
k expose deployment nginx --port 80 --type NodePort

```

#### Check it works

```
k get all -o wide

go with your favourite browser on: https://worker-adress:NodePort

```

#### Set a limit on resources

Edit the nginx deploy 

```

k edit deploy nginx

```


with a modification of resources like:

```
resources:
  limits:
    cpu: "100m" #m means millicore
  requests:
    cpu: "100m"

```

#### Create an autoscaler with 

```
k autoscale deploy nginx --min 1 --max 5 --cpu-percent 20

```

control it works:

```
k describe hpa nginx

```
#### Aumenting the request

```
siege -q -c 5 -t 2m http://"worker_node_adress":"NodePort"

```

Now the replica will scale up.
After two minutes the will scale down again.
Observe the evolution with:

```
k get pod -w 

k get all

k describe deploy nginx 

```

