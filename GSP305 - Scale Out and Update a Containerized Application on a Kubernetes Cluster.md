# GSP305 - Scale Out and Update a Containerized Application on a Kubernetes Cluster
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

* Copies the `echo-web-v2.tar.gz` file from a Google Cloud Storage bucket to the current working directory.
```
gsutil cp gs://sureskills-ql/challenge-labs/ch05-k8s-scale-and-update/echo-web-v2.tar.gz .
```

* Extracts the contents of the echo-web-v2.tar.gz archive file.
```
tar xvzf echo-web-v2.tar.gz
```

* Submits the current directory as a build to Google Cloud Build, which creates a Docker container image and tags it with the specified name (gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v2). The resulting image is stored in Google Container Registry.
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v2 .
```  

* Retrieves the cluster credentials for the echo-cluster Kubernetes cluster in the us-central1-a zone. This allows you to interact with the cluster using the kubectl command-line tool
```
gcloud container clusters get-credentials echo-cluster --zone us-central1-a
```

* Creates a new Kubernetes deployment named echo-web using the specified container image (gcr.io/qwiklabs-resources/echo-app:v1).
```
kubectl create deployment echo-web --image=gcr.io/qwiklabs-resources/echo-app:v1
```

* Creates a new Kubernetes service to expose the `echo-web` deployment. The service is of type `LoadBalancer`, which means it will create an external load balancer to distribute traffic to the pods in the deployment. The service listens on port` 80` and forwards traffic to the target port `8000` on the pods
```
kubectl expose deployment echo-web --type=LoadBalancer --port 80 --target-port 8000
```

* Opens the echo-web deployment configuration in an editor, allowing you to make changes to the deployment.
```
kubectl edit deploy echo-web
```

* Pressing "i" to change image version from 'v1' to 'v2'
* Save by click Esc > type ":wq" > press enter.

* Scale the echo-web deployment to 2 replicas. In other words, it adjusts the number of running instances (pods) of the deployment to the specified number.
```
kubectl scale deploy echo-web --replicas=2
```

* wait for a minute then check
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## DONE
