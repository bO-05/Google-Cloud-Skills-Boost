# GSP322 - Build and Secure Networks in Google Cloud: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

* Navigate to Compute Engine > VM Instances 
* Check zone and copy value, note it as <YOUR_ZONE>
* Fill all value below accordingly and paste to cloud shell

```
export ZONE=<YOUR_ZONE>
export IAP_NETWORK_TAG=<SSH_IAP_NETWORK_TAG>
export INTERNAL_NETWORK_TAG=<SSH_INTERNAL_NETWORK_TAG>
export HTTP_NETWORK_TAG=<HTTP_NETWORK_TAG>
```
* Copy and paste all code below to cloud shell
```
gcloud compute firewall-rules delete open-access
 
gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags $IAP_NETWORK_TAG --network acme-vpc
 
gcloud compute instances add-tags bastion --tags=$IAP_NETWORK_TAG --zone=$ZONE
 
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags $HTTP_NETWORK_TAG --network acme-vpc
 
gcloud compute instances add-tags juice-shop --tags=$HTTP_NETWORK_TAG --zone=$ZONE
 
gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags $INTERNAL_NETWORK_TAG --network acme-vpc
 
gcloud compute instances add-tags juice-shop --tags=$INTERNAL_NETWORK_TAG --zone=$ZONE
```


* Navigate to Compute Engine > VM Instances
* Click the SSH button for the bastion host. 
* Paste code below to the SSH connection

```
gcloud compute ssh juice-shop --internal-ip
```

--------------------------------------------------------------------------------------------------------------------------------------------------------------

DONE
