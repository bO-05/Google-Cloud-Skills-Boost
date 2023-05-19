# GSP 313 - Create and Manage Cloud Resources: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 1: Create a project jumphost instance

### Navigation menu -> Compute engine -> VM Instance

* Instance name: <INSTANCE_NAME>
* Region and Zone: default values.
* Series: `N1`
* Machine type: `f1-micro`
* Default image type (Debian Linux)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Create a Kubernetes service cluster

### Create a zonal cluster using <YOUR_ZONE>
```
gcloud config set compute/zone <YOUR_ZONE>
```
```
gcloud container clusters create nucleus-backend \
         --num-nodes 1 \
         --network nucleus-vpc
```
```       
gcloud container clusters get-credentials nucleus-backend
```

### Use the Docker container hello-app (gcr.io/google-samples/hello-app:2.0) as a placeholder; the team will replace the container with their own work later
```
kubectl create deployment hello-server \
         --image=gcr.io/google-samples/hello-app:2.0
```

### Expose the app on port <APP_PORT_NUMBER>
```
kubectl expose deployment hello-server \
         --type=LoadBalancer \
         --port <APP_PORT_NUMBER>
```

### Using command below, if there's EXTERNAL_IP, then you can check for completion
```       
kubectl get service          
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Set up an HTTP load balancer

### Configure the web servers
```
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

### Create an instance template
```
gcloud compute instance-templates create web-server-template \
--metadata-from-file startup-script=startup.sh \
--network nucleus-vpc \
--machine-type g1-small \
--region us-east1
```

### Create a target pool
```
gcloud compute target-pools create nginx-pool
```

### Create a managed instance group
```
gcloud compute instance-groups managed create web-server-group \
      --base-instance-name web-server \
      --size 2 \
      --template web-server-template \
      --region us-east1
```

### Create a firewall rule named as <FIREWALL_RULE_NAME> to allow traffic (80/tcp)
```
gcloud compute firewall-rules create <FIREWALL_RULE_NAME> \
      --allow tcp:80 \
      --network nucleus-vpc
```

### Create a health check
```
gcloud compute http-health-checks create http-basic-check
```
```
gcloud compute instance-groups managed \
      set-named-ports web-server-group \
      --named-ports http:80 \
      --region us-east1
```

### Create a backend service, and attach the managed instance group with named port (http:80)
```
gcloud compute backend-services create web-server-backend \
      --protocol HTTP \
      --http-health-checks http-basic-check \
      --global
```
```
gcloud compute backend-services add-backend web-server-backend \
      --instance-group web-server-group \
      --instance-group-region us-east1 \
      --global
```

### Create a URL map, and target the HTTP proxy to route requests to your URL map
```
gcloud compute url-maps create web-server-map \
      --default-service web-server-backend
```
```
gcloud compute target-http-proxies create http-lb-proxy \
      --url-map web-server-map
```

### Create a forwarding rule
```
gcloud compute forwarding-rules create http-content-rule \
    --global \
    --target-http-proxy http-lb-proxy \
    --ports 80
```
    
### Wait 4-5 minutes, then you can check for completion

--------------------------------------------------------------------------------------------------------------------------------------------------------------

### DONE
