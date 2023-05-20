# GSP304 - Build and Deploy a Docker Image to a Kubernetes Cluster
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 1 : Create a Kubernetes Cluster
```
gcloud container clusters create echo-cluster --num-nodes 2 --zone us-central1-a --machine-type n1-standard-2
```

## Task 2 : Build a tagged Docker Image
```
gsutil cp gs://sureskills-ql/challenge-labs/ch04-kubernetes-app-deployment/echo-web.tar.gz .

tar -xvf echo-web.tar.gz
```
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 .
```

## Task 3 : Push the image to the Google Container Registry
```
kubectl create deployment echo-web --image=gcr.io/qwiklabs-resources/echo-app:v1
```

## Task 4 : Deploy the application to the Kubernetes Cluster
```
kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
```
check external IP
```
kubectl get svc
```

## DONE
