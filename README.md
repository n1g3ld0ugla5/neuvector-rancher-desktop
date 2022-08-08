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

All newly-installed components of Neuvector should show-up in the Rancher Desktop UI:


<img width="1047" alt="Screenshot 2022-08-08 at 14 15 13" src="https://user-images.githubusercontent.com/109959738/183426856-e51da609-9542-4a94-820d-5fd4ce5e8977.png">


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

Now I can access the Web UI from ```https://localhost:8443``` <br/>
Tip: Make sure your request is for ```https``` and not ```http```.

## Step 5: Using NeuVector:
The default username is ```admin``` and default password is ```admin``` <br/>
However, the controller is not working - so I cannot log into the web UI yet.

<img width="1030" alt="Screenshot 2022-08-08 at 14 03 34" src="https://user-images.githubusercontent.com/109959738/183424557-a29d0f4a-a7cc-4fad-a453-6b048f3d7eea.png">

Check the health of the controller pods in the ```neuvector``` network namespace:

```
kubectl get pods -A
```

<img width="756" alt="Screenshot 2022-08-08 at 14 06 41" src="https://user-images.githubusercontent.com/109959738/183424990-3cf68004-a4fc-4c50-9de4-45d12f45ee7f.png">

Describing the pod in the ```CrashLoopBackOff``` state informs us of...

```
kubectl describe pod neuvector-controller-pod-54c588dc6c-wvgf4 -n neuvector
```

<img width="1155" alt="Screenshot 2022-08-08 at 14 12 00" src="https://user-images.githubusercontent.com/109959738/183425993-9494c948-a212-4397-8e0f-2c4f61354538.png">


## Step 6: End User License Agreements

Accept the license agreement, and you're off to the races! Happy NeuVectoring! <br/>
We will cover the process of locking down and securing our environment in future articles.

