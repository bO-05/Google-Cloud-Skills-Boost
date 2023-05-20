GSP318 - Deploy to Kubernetes in Google Cloud: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1: Create a Docker image and store the Dockerfile

```
gcloud auth list
gsutil cat gs://cloud-training/gsp318/marking/setup_marking_v2.sh | bash
gcloud source repos clone valkyrie-app
cd valkyrie-app
cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF
docker build -t <DOCKER_IMAGE>:<TAG_NAME> .
cd ..
cd marking
./step1_v2.sh
```

#### Before check, wait for: 
"Image exists"  
"Go ahead and check the activity tracking on the lab page"

#### If it isn't, run again:
```
./step1_v2.sh
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Test the created Docker image

```
cd ..
cd valkyrie-app
docker run -p 8080:8080 <DOCKER_IMAGE>:<TAG_NAME> &
cd ..
cd marking
./step2_v2.sh

```

#### Before check, wait for: 
"Container running and visible on port 8080, good job!"  
"Go ahead and check the activity tracking on the lab page"

#### If it isn't, run again:
```
./step2_v2.sh
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Push the Docker image to the Artifact Registry

```
cd ..
cd valkyrie-app

gcloud artifacts repositories create <REPOSITORY_NAME> \
    --repository-format=docker \
    --location=us-central1 \
    --description="Deploy to Kubernetes in Google Cloud Challenge Lab" \
    --async 

gcloud auth configure-docker us-central1-docker.pkg.dev
```
#### Check and copy <IMAGE_ID> 
```
docker images
```
#### Paste <IMAGE_ID> from output above
```
docker tag <IMAGE_ID> us-central1-docker.pkg.dev/<PROJECT_ID>/<REPOSITORY_NAME>/<DOCKER_IMAGE>:<TAG_NAME>

docker push us-central1-docker.pkg.dev/<PROJECT_ID>/<REPOSITORY_NAME>/<DOCKER_IMAGE>:<TAG_NAME>
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Create and expose a deployment in Kubernetes

```
sed -i s#IMAGE_HERE#us-central1-docker.pkg.dev/<PROJECT_ID>/<REPOSITORY_NAME>/<DOCKER_IMAGE>:<TAG_NAME>#g k8s/deployment.yaml

gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
kubectl create -f k8s/deployment.yaml
kubectl create -f k8s/service.yaml
```

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## DONE
