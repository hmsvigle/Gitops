# Gitops


1. # Create argocd namespace
````sh
kubectl create ns argocd
````
2. # install argocd crd
````sh 
kubectl apply -f argocd-crd.yaml -n argocd
````
3. # Deploy LB Svc (Patch clusterip svc to LB)
````sh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
````

4. # initial Password for login
````sh
kubectl.exe get secrets -n argocd argocd-initial-admin-secret -ojsonpath='{.data.password}' | base64 -d 
````
5. # deploy nginx to app namespace 
````sh 
kubectl.exe create deploy nginx --image=dockerhub.com/nginx-unprivileged/nginx-unprivileged:v1 -n app --replicas=2

kubectl.exe expose deploy nginx --name=nginx-svc -n app --port=8080 --target-port=8080 --type=ClusterIP
service/nginx-svc exposed
````
6. # Create an Application (argocd crd) & link git source & kubernetes target destination with sync policies

````sh
 kubectl apply argocd/nginx-application.yaml
````
  * Sync policies can be referred from [here](https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/#selective-sync) 


7. Login to Argocd Portal & Validate the configurations further 
