# GSP323 - Perform Foundational Data, ML, and AI Tasks in Google Cloud: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 1: Run a simple Dataflow job

```
bq mk <BIGQUERY_DATASET_NAME>

gsutil mb gs://<CLOUD_STORAGE_BUCKET_NAME>

gsutil cp gs://cloud-training/gsp323/lab.csv  .

cat lab.csv 

gsutil cp gs://cloud-training/gsp323/lab.schema .
 
cat lab.schema
```

the rest is manual

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Run a simple Dataproc job
 
manual

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Use the Google Cloud Speech API


* Navigation menu > APIs & services > Credentials.
* Then click Create credentials.
* In the drop down menu, select API key.
* Copy the key you just generated and click Close.
* In cloud shell

```
gcloud auth login --no-launch-browser
```
```
export API_KEY=<YOUR_API_KEY>
```
```
cat > request.json <<EOF 
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-training/gsp323/task3.flac"
  }
}
EOF
```
```
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json

gsutil cp result.json <TASK_3_BUCKET_NAME>
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Use the Cloud Natural Language API
 
In cloud shell

```
export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
 
gcloud iam service-accounts create my-natlang-sa \
--display-name "my natural language service account"
 
gcloud iam service-accounts keys create ~/key.json \
--iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
 
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"
 
gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > result.json
 
gsutil cp result.json <TASK_4_BUCKET_NAME>
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

manual work i follow [this](https://www.youtube.com/watch?v=l5BZj3OXEgQ)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## DONE
