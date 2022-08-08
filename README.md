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



