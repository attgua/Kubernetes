# RBAC

**What a user can do in a cluster depen only from the certificate the user have.**

#### Create Certifications

Suppose there is a user  `john` that should have access to the `prod` namespace.

* Create john.key

`openssl genrsa -out john.key 2048` 

* Create certificate (.csr)

`openssl req -new -key john.key -out john.csr -subj "/CN=john/O=prod"`

* Take ca.crt and ca.key from master node

if you are on master

`mv /etc/kubernetes/pki/ca.{crt,key} .`

otherwise 

`scp "user"@"masted_ip" /etc/kubernetes/pki/ca.{crt,key}  .` 

* Create John certificate

`sudo  openssl x509 -req -in john.csr -CA ca.crt -CAkey ca.key -CAcreateserial  -out john.crt -days 365`


#### Two possibilities to create the kubernetes configuratiton file

If you are the admin you could 

##### * Give to John certificate and key and he could create his certificate

We have to send to John:

```
ca.crt
john.crt
john.key
```

and he could use:

`kubectl --kubeconfig john.kubeconfig config set-cluster kubernetes --server https://192.168.0.3:6443 --certificate-authority=ca.crt`

to know the cluster name and server ip address --> *kubectl config view*

```kubectl --kubeconfig john.kubeconfig config set-credentials john --client-certificate full_path/john.crt --client-key full_path/john.key  ```

now set the context

`kubectl --kubeconfig john.kubeconfig config set-context john-kubernetes --cluster kubernetes --namespace prod --user john`

modify the *john.kubeconfig*

```
vi john.kubeconfig
current-context: john-kubernetes 
```

finally john have to copy the certificate in his .kube

```
cp john.kubeconfig ~/.kube.config
```

##### * Create the certificate for John and give to him only it  

Better solution so you have not to share all those private file to him.



##### Create a Role inside the cluster


```
k create role john-prod --verb=get,list --resource=pods --namespace=prod
```

##### Use Role-Binding

```
kubectl create rolebinding john-prod-rolebinding --role=john-prod  --user=john --namespace=prod
```



