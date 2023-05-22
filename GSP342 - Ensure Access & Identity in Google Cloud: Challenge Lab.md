# GSP342 - Ensure Access & Identity in Google Cloud: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 1: Create a custom security role

```
gcloud config set compute/zone us-east1-b
nano role-definition.yaml
```

Paste below code to the editor  
//note: replace <CUSTOM_SECURITY_ROLE> accordingly

```
title: "<CUSTOM_SECURITY_ROLE>"
description: "Permissions"
stage: "ALPHA"
includedPermissions:
- storage.buckets.get
- storage.objects.get
- storage.objects.list
- storage.objects.update
- storage.objects.create
```
`Ctrl + X` + "Y" + `Enter`

//note: replace <CUSTOM_SECURITY_ROLE> accordingly

```
gcloud iam service-accounts create orca-private-cluster-sa --display-name "Orca Private Cluster Service Account"
gcloud iam roles create <CUSTOM_SECURITY_ROLE> --project $DEVSHELL_PROJECT_ID --file role-definition.yaml
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Create a service account

//note: replace <SERVICE_ACCOUNT> accordingly

```
gcloud iam service-accounts create <SERVICE_ACCOUNT> --display-name "Orca Private Cluster Service Account"
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Bind a custom security role to a service account

//note: replace <SERVICE_ACCOUNT> and <CUSTOM_SECURITY_ROLE> accordingly

```
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:<SERVICE_ACCOUNT>@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/monitoring.viewer

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:<SERVICE_ACCOUNT>@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/monitoring.metricWriter

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:<SERVICE_ACCOUNT>@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role roles/logging.logWriter

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member serviceAccount:<SERVICE_ACCOUNT>@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role projects/$DEVSHELL_PROJECT_ID/roles/<CUSTOM_SECURITY_ROLE>
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Create and configure a new Kubernetes Engine private cluster

//note: replace <CLUSTER_NAME> and <SERVICE_ACCOUNT> accordingly

```
gcloud container clusters create <CLUSTER_NAME> --num-nodes 1 --master-ipv4-cidr=172.16.0.64/28 --network orca-build-vpc --subnetwork orca-build-subnet --enable-master-authorized-networks  --master-authorized-networks 192.168.10.2/32 --enable-ip-alias --enable-private-nodes --enable-private-endpoint --service-account <SERVICE_ACCOUNT>@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --zone us-east1-b
```

This take a couple of minutes, we can directly go to task 5

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Deploy an application to a private Kubernetes Engine cluster

SSH to `orca-jumphost` VM Instance  

//note: replace <CLUSTER_NAME> accordingly

```
gcloud config set compute/zone us-east1-b

gcloud container clusters get-credentials <CLUSTER_NAME> --internal-ip

sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin

kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0

kubectl expose deployment hello-server --name orca-hello-service --type LoadBalancer --port 80 --target-port 8080
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## DONE
