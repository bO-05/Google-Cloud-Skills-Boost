# GSP330 - Implement DevOps in Google Cloud: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1: Create the lab resources

```
gcloud services enable container.googleapis.com \
    cloudbuild.googleapis.com \
    sourcerepo.googleapis.com
```
```    
export PROJECT_ID=$(gcloud config get-value project)
gcloud projects add-iam-policy-binding $PROJECT_ID \
--member=serviceAccount:$(gcloud projects describe $PROJECT_ID \
--format="value(projectNumber)")@cloudbuild.gserviceaccount.com --role="roles/container.developer"
```
```
git config --global user.email <email> #student-xx-xxxxxxxxxxxx@qwiklabs.net
git config --global user.name <name> #user,qwiklabs,student,etc
```
```
export PROJECT_ID=<YOUR_PROJECT_ID>
export CLUSTER_NAME=<YOUR_CLUSTER_NAME>
export ZONE=<YOUR_ZONE>
export REGION=<YOUR_REGION>
export REPO=<YOUR_REPO_NAME>
```
```
gcloud artifacts repositories create $REPO \
    --repository-format=docker \
    --location=$REGION \
    --description="Implement DevOps in Google Cloud Challenge Lab"
```
```
gcloud beta container --project "$PROJECT_ID" clusters create "$CLUSTER_NAME" --zone "$ZONE" --no-enable-basic-auth --cluster-version "1.25.8-gke.500" --release-channel "regular" --machine-type "e2-medium" --image-type "COS_CONTAINERD" --disk-type "pd-balanced" --disk-size "100" --metadata disable-legacy-endpoints=true  --logging=SYSTEM,WORKLOAD --monitoring=SYSTEM --enable-ip-alias --network "projects/$PROJECT_ID/global/networks/default" --subnetwork "projects/$PROJECT_ID/regions/$REGION/subnetworks/default" --no-enable-intra-node-visibility --default-max-pods-per-node "110" --enable-autoscaling --min-nodes "2" --max-nodes "6" --location-policy "BALANCED" --no-enable-master-authorized-networks --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --enable-shielded-nodes --node-locations "$ZONE"
```
```
kubectl create namespace prod && kubectl create namespace dev
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Create a repository in Cloud Source Repositories

```
gcloud source repos create sample-app
```
```
git clone https://source.developers.google.com/p/$PROJECT_ID/r/sample-app
```
```
cd ~
gsutil cp -r gs://spls/gsp330/sample-app/* sample-app
```
```
git init
cd sample-app/
git add .
git commit -m "Implement DevOps in Google Cloud Challenge Lab" 
git push -u origin master
```
```
git branch dev
git checkout dev
git push -u origin dev
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Create the Cloud Build Triggers

In the Cloud console, go to Cloud Build > Triggers.

Click `+ CREATE TRIGGER`  
* Name: `sample-app-prod-deploy`  
* Event: `Push to a branch`  
* Source Repository: `sample-app`  
* Branch: `^master$`  
* Cloud Build Configuration File: `cloudbuild.yaml`  
* Click `CREATE`  

Click `+ CREATE TRIGGER` again  
* Name: `sample-app-dev-deploy`  
* Event: `Push to a branch`  
* Source Repository: `sample-app`  
* Branch: `^dev$`  
* Cloud Build Configuration File: `cloudbuild-dev.yaml`  
* Click `CREATE`  
--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Deploy the first versions of the application

### Build the first ***development*** deployment

01. In Cloud Shell editor, inspect the `cloudbuild-dev.yaml` file to see the steps in the build process. Fill in the <version> on lines 10 and 15 with `v1.0`  
02. Run command below and note the value of `IMAGE`:  
```
COMMIT_ID="$(git rev-parse --short=7 HEAD)"
gcloud builds submit --tag="${REGION}-docker.pkg.dev/${PROJECT_ID}/$REPO/hello-cloudbuild:${COMMIT_ID}" .
```
Navigate to the `dev/deployment.yaml` file and replace the <todo> on line 17 with the value of `IMAGE` above.

```
git add .
git commit -m "Implement DevOps in Google Cloud Challenge Lab" 
git push -u origin dev
```
  
  
```
git checkout master
```

  
### Build the first ***production*** deployment

01. Inspect the `cloudbuild.yaml` file to see the steps in the build process. Fill in the <version> on lines 10 and 15 with `v1.0`  
02. Navigate to the `prod/deployment.yaml` file and replace the <todo> on line 17 with the value of `IMAGE` same as before.

```
git add .
git commit -m "Implement DevOps in Google Cloud Challenge Lab" 
git push -u origin master
```

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Deploy the second versions of the application

### Build the second ***development*** deployment
01. Switch back to the dev branch.
```
git checkout dev
```
02. Open Cloud Shell editor, In the `main.go` file, update the `main()` function 
```
func main() {
	http.HandleFunc("/blue", blueHandler)
	http.HandleFunc("/red", redHandler)
	http.ListenAndServe(":8080", nil)
}
```
03. Add this function in the end of `main.go`
```
func redHandler(w http.ResponseWriter, r *http.Request) {
	img := image.NewRGBA(image.Rect(0, 0, 100, 100))
	draw.Draw(img, img.Bounds(), &image.Uniform{color.RGBA{255, 0, 0, 255}}, image.ZP, draw.Src)
	w.Header().Set("Content-Type", "image/png")
	png.Encode(w, img)
}
```
04. Inspect the `cloudbuild-dev.yaml` file to see the steps in the build process. Update the version of the Docker image to `v2.0`  
05. Navigate to the `dev/deployment.yaml`, change image version in the end to `v1.0`
06. Commit and push to ***dev*** branch
```
git add .
git commit -m "Implement DevOps in Google Cloud Challenge Lab" 
git push -u origin dev
```
  
### Build the second ***production*** deployment
01. Switch to the main branch.
```
git checkout master
```
02. Open Cloud Shell editor, In the `main.go` file, update the `main()` function
```
func main() {
	http.HandleFunc("/blue", blueHandler)
	http.HandleFunc("/red", redHandler)
	http.ListenAndServe(":8080", nil)
}
```
03. Add this function in the end of `main.go`
```
func redHandler(w http.ResponseWriter, r *http.Request) {
	img := image.NewRGBA(image.Rect(0, 0, 100, 100))
	draw.Draw(img, img.Bounds(), &image.Uniform{color.RGBA{255, 0, 0, 255}}, image.ZP, draw.Src)
	w.Header().Set("Content-Type", "image/png")
	png.Encode(w, img)
}
```
04. Inspect the `cloudbuild.yaml` file to see the steps in the build process. Update the version of the Docker image to `v2.0`  
05. Navigate to the `prod/deployment.yaml`, change image version in the end to `v1.0`
06. Commit and push to ***master*** branch
```
git add .
git commit -m "Implement DevOps in Google Cloud Challenge Lab" 
git push -u origin master
```

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 6: Roll back the production deployment

In the Cloud console, go to Cloud Build > History  

Choose the second options from the top  

click `REBUILD`  

--------------------------------------------------------------------------------------------------------------------------------------------------------------
### DONE
