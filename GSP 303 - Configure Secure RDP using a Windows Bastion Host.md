# GSP 303 - Configure Secure RDP using a Windows Bastion Host
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1 : A new non-default VPC has been created
```
gcloud compute networks create securenetwork --subnet-mode=custom
```

## Task 2 : The new VPC contains a new non-default subnet within it
```
gcloud compute networks subnets create securenetwork --network=securenetwork --region=us-central1 --range=192.168.1.0/24
```

## Task 3 : A firewall rule exists that allows TCP port 3389 traffic ( for RDP )
```
gcloud compute firewall-rules create myfirewalls --network securenetwork --allow=tcp:3389 --target-tags=rdp
```

## Task 4 : A Windows compute instance called vm-bastionhost exists that has a public ip-address to which the TCP port 3389 firewall rule applies.

```
gcloud compute instances create vm-bastionhost --zone=us-central1-a --machine-type=n1-standard-2 --subnet=securenetwork --network-tier=PREMIUM --maintenance-policy=MIGRATE --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=rdp --image=windows-server-2016-dc-v20200211 --image-project=windows-cloud --boot-disk-size=50GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-bastionhost --reservation-affinity=any
```

## Task 5 : A Windows compute instance called vm-securehost exists that does not have a public ip-address
```
gcloud compute instances create vm-securehost --zone=us-central1-a --machine-type=n1-standard-2 --subnet=securenetwork --no-address --maintenance-policy=MIGRATE --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=rdp --image=windows-server-2016-dc-v20200211 --image-project=windows-cloud --boot-disk-size=50GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-securehost --reservation-affinity=any
```
	
## Task 6 : The vm-securehost is running Microsoft IIS web server software.
* For `vm-bastionhost`
```
gcloud compute reset-windows-password vm-bastionhost --user app_admin --zone us-central1-a
```
(choose Y and copy the password)

* For `vm-securehost`
```
gcloud compute reset-windows-password vm-securehost --user app_admin --zone us-central1-a
```
(choose Y and copy the password)

## DONE
