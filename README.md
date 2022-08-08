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

