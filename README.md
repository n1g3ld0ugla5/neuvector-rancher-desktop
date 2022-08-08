# neuvector-rancher-desktop

Ensure Rancher Desktop nodes & pods are running in your local terminal:

```
kubectl get nodes -o wide && kubectl get pods -A
```

<img width="1167" alt="Screenshot 2022-08-08 at 13 48 10" src="https://user-images.githubusercontent.com/109959738/183421660-3e2d0034-f251-491b-a602-5b6be93ef32c.png">



## Step 1: Add the helm repository
Add the helm repo from NeuVector's Github Pages:

```
helm repo add neuvector https://neuvector.github.io/neuvector-helm/
```

## Step 2: Verify helm chart
Verify the helm repo

```
helm search repo neuvector
```

<img width="750" alt="Screenshot 2022-08-08 at 13 50 55" src="https://user-images.githubusercontent.com/109959738/183422202-cfc45b13-0a50-4a8f-9e2e-6dcb6630ac99.png">



Hint: To list chart versions, including Beta charts, add "-l --devel":

```
helm search repo neuvector -l --devel
```

<img width="750" alt="Screenshot 2022-08-08 at 13 51 55" src="https://user-images.githubusercontent.com/109959738/183422346-29bce978-f1e2-4187-8c21-3f55594fb45f.png">


## Step 3: Helm install
Compile your helm command with values overrides. <br/>
<br/>
We need to use the Beta chart version (currently 2.2.0). <br/>
Use the following which includes the version specified.

```
helm upgrade --install neuvector neuvector/core --version 2.2.0 \
--set tag=5.0.0 \
--set registry=docker.io \
--create-namespace \
--namespace neuvector
```

<img width="874" alt="Screenshot 2022-08-08 at 13 55 02" src="https://user-images.githubusercontent.com/109959738/183422957-e612a209-b1e0-4cae-9840-9ff6e8320ee5.png">

Run the commands provided to print the nodeport location of the NeuVector controller

```
NODE_PORT=$(kubectl get --namespace neuvector -o jsonpath="{.spec.ports[0].nodePort}" services neuvector-service-webui) && \
NODE_IP=$(kubectl get nodes --namespace neuvector -o jsonpath="{.items[0].status.addresses[0].address}") && \
echo https://$NODE_IP:$NODE_PORT
```

<img width="861" alt="Screenshot 2022-08-08 at 13 57 28" src="https://user-images.githubusercontent.com/109959738/183423402-2fda7953-c00b-4a73-b201-2ec5aa82aa76.png">


This endpoint (```https://192.168.5.15:30890```) will not be available to the internet unless you have specifically opened the port on your internet-connected nodes. <br/>
Use kubectl ```port-forward``` or other tool to proxy create an access point for you local machine, then access the Web UI.

## Step 4: Access the Web UI
I proxied the connect to my local machine via kubectl port-forward

```
kubectl port-forward --namespace neuvector service/neuvector-service-webui 8443
```

Now I can access the Web UI from ```https://localhost:8443```
Tip: Make sure your request is for ```https``` and not ```http```.

## Step 5: Using NeuVector:
The default username is ```admin``` and default password is ```admin``` <br/>

Accept the license agreement, and you're off to the races! Happy NeuVectoring! <br/>

We will cover the process of locking down and securing our environment in future articles.

