# GSP 301 - Deploy a Compute Instance with a Remote Startup Script
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1: Check that a Google Cloud Storage bucket exists and contains a file
```
gsutil mb gs://$DEVSHELL_PROJECT_ID
gsutil cp gs://sureskills-ql/challenge-labs/ch01-startup-script/install-web.sh gs://$DEVSHELL_PROJECT_ID
```

## Task 2: Check that a compute instance was created with a startup script called `install-web.sh`
```
gcloud compute instances create example-instance --zone=us-central1-a --tags=http-server --metadata startup-script-url=gs://$DEVSHELL_PROJECT_ID/install-web.sh
```

## Task 3: Check that an HTTP firewall rule with a tag exists for that virtual machine
```
gcloud compute firewall-rules create allow-http --target-tags http-server --source-ranges 0.0.0.0/0 --allow tcp:80
```

## Task 4: Visit the server's external IP address over HTTP and get a successful response

## DONE
