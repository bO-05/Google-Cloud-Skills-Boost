# GSP101 - Google Cloud Essential Skills: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1 : Create a Compute Engine instance, add necessary firewall rules

Navigate to **Compute Engine** > **VM Instances** > **Create Instance**.
* Name : <INSTANCE_NAME>
* Zone : <COMPUTE_ZONE>
* Series : N1, f1-micro
* Boot Disk : Default

* Tick : `Allow HTTP Traffic`
* Tick : `Allow HTTPS Traffic`

Click Create.

## Task 2 : Configure Apache2 Web Server in your instance

SSH to the above vm instance you created and run
```
sudo su -
```
```
apt-get update
apt-get install apache2 -y
```
```
service --status-all
```

## Task 3 : Test your server

Click on External IP of your vm instance, check that your URL is `http:// EXTERNAL_IP` and ***not*** `https:// EXTERNAL_IP`.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## DONE
