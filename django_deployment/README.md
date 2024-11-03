# Deploy Django locally with Kubernetes 

Deployment will include 2 pods:

* The first running a Gunicorn/Django container and an Nginx container
* The second running a Postgres database

Other K8s components used:

* initContainer (to collect Django static files)
* configmap (for environment variables)
* secrets (for sensitive variables)
* emptyDir (for static content)
* persistent volume and claim (for database storage)


## Deployment steps
1. Verify that the Kind cluster is running,
```
kind get clusters
```
    if there is none available, use
    ```
    kind create cluster1
    ```
    to create your first cluster with name cluster1

2. Create namespace **django** where all works will be done
```
kubectl create namespace django
```
3. Set namespace django as current so all works are done in there

```
kubectl config set-context --current --namespace=django
```

4. Encode all secrets with base64, using bash terminal
    * cd to **database** and encode all secrets
    * cd to **application** and do the same
```
echo -n postgresdb | base64
```

5. apply database templates
cd to **database** dir and apply the following

```
kubectl apply -f database/secret.yml
kubectl apply -f database/storage.yml
kubectl apply -f database/deployment.yml
kubectl apply -f database/service.yml
```

6. get the database local IP and place it in the application configmap, this is the IP of the NodePort

```
kubectl get services 
```

7. Apply application templates
cd to **application** dir and apply the following

```
kubectl apply -f application/secret.yaml
kubectl apply -f application/configmap.yaml
kubectl apply -f application/deployment.yaml
kubectl apply -f application/service.yaml
```

8. get the app pod name and exec into container to do database migration (optional)
```
kubectl get pods
kubectl exec -it <pod name> -- /bin/bash
python manage.py makemigrations
python manage.py migrate
```

10. port forwarding to reach the Django app from your browser
```
kubectl port-forward service/app-service 8000:80      
```

11. Once the port forward is running, you can open a browser and navigate to http://localhost:8000 to access your Django application.


12. Tear it all down
```
kubectl delete all --all
```

## Debugging


1. Check if the ConfigMap exists
```
kubectl get configmap nginx-conf -n django
```

2. Create the ConfigMap if it doesn't exist
```
kubectl create configmap nginx-conf --from-file=nginx.conf -n django
```

3. Verify the ConfigMap creation
```
kubectl get configmap nginx-conf -n django
```

4. Check the status of the pod
```
kubectl get pod -n django
```
