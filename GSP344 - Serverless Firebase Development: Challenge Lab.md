# GSP344 - Serverless Firebase Development: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 1: Create a Firestore database

* Navigate to Cloud Firestore Database
* Use Firestore Native Mode
* Add location Nam5 (United States)

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Populate the Database
```
cd pet-theory/lab06/firebase-import-csv/solution
npm install
node index.js netflix_titles_original.csv
```

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Create a REST API
```
cd ~/pet-theory/lab06/firebase-rest-api/solution-01
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1
gcloud beta run deploy <DATASET_SERVICE_NAME> --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1 --allow-unauthenticated
```
* Choose number that represent `us-central-1`

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Firestore API access
```
cd ~/pet-theory/lab06/firebase-rest-api/solution-02
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2
gcloud beta run deploy <DATASET_SERVICE_NAME> --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2 --allow-unauthenticated
```
* Choose number that represent `us-central-1`
* Go to cloud run and click <DATASET_SERVICE_NAME> then copy the url.
```
SERVICE_URL=<copy url from your <DATASET_SERVICE_NAME> >
curl -X GET $SERVICE_URL/2019
```

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Deploy the Staging Frontend
```
cd ~/pet-theory/lab06/firebase-frontend/public
nano app.js 
```
* Comment line 3 and uncomment line 4, insert your netflix-dataset-service url
```
npm install
cd ~/pet-theory/lab06/firebase-frontend
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
gcloud beta run deploy <FRONTEND_STAGING_SERVICE_NAME> --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
```
* Choose number that represent `us-central-1`
* Allow unauthenticated invocation to <FRONTEND_STAGING_SERVICE_NAME>? Y

--------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 6: Deploy the Production Frontend
```
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
gcloud beta run deploy <FRONTEND_PRODUCTION_SERVICE_NAME> --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
```
* Choose number that represent `us-central-1`
* Allow unauthenticated invocation to <FRONTEND_PRODUCTION_SERVICE_NAME>? Y

--------------------------------------------------------------------------------------------------------------------------------------------------------------

## DONE
