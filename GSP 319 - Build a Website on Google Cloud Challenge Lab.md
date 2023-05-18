# GSP319 - Build a Website on Google Cloud Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 1

```
git clone https://github.com/googlecodelabs/monolith-to-microservices.git
```
```
cd ~/monolith-to-microservices && ./setup.sh && nvm install --lts
```
```
cd ~/monolith-to-microservices/monolith && npm start
```
```
gcloud services enable cloudbuild.googleapis.com
```
```
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/<MONOLITH_IDENTIFIER>:1.0.0 .
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 2

```
gcloud config set compute/zone us-central1-a
```
```
gcloud services enable container.googleapis.com
```
```
gcloud container clusters create <CLUSTER_NAME> --num-nodes 3
```
```
kubectl create deployment <MONOLITH_IDENTIFIER> --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/<MONOLITH_IDENTIFIER>:1.0.0
```
```
kubectl expose deployment <MONOLITH_IDENTIFIER> --type=LoadBalancer --port 80 --target-port 8080
```
check external IP
```
kubectl get service <MONOLITH_IDENTIFIER>
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 3

```
cd ~/monolith-to-microservices/microservices/src/orders
```
```
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/<ORDERS_IDENTIFIER>:1.0.0 .
```
```
cd ~/monolith-to-microservices/microservices/src/products
```
```
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/<PRODUCTS_IDENTIFIER>:1.0.0 .
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 4

```
kubectl create deployment <ORDERS_IDENTIFIER> --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/<ORDERS_IDENTIFIER>:1.0.0
```
```
kubectl expose deployment <ORDERS_IDENTIFIER> --type=LoadBalancer --port 80 --target-port 8081
```
```
kubectl create deployment <PRODUCTS_IDENTIFIER> --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/<PRODUCTS_IDENTIFIER>:1.0.0
```
```
kubectl expose deployment <PRODUCTS_IDENTIFIER> --type=LoadBalancer --port 80 --target-port 8082
```
```
kubectl get all
```
wait until complete

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 5

check external IP, take note of <ORDERS_IP_ADDRESS> and <PRODUCTS_IP_ADDRESS>
```
kubectl get all 
```
```
cd ~/monolith-to-microservices/react-app && nano .env
```
change
```
REACT_APP_ORDERS_URL=http://<ORDERS_IP_ADDRESS>/api/orders
REACT_APP_PRODUCTS_URL=http://<PRODUCTS_IP_ADDRESS>/api/products
```
save + (ctrl X + Y + Enter)

```
npm run build
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 6

```
cd ~/monolith-to-microservices/microservices/src/frontend
```
```
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/<FRONTEND_IDENTIFIER>:1.0.0 .
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 7

```
kubectl create deployment <FRONTEND_IDENTIFIER> --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/<FRONTEND_IDENTIFIER>:1.0.0
```
```
kubectl expose deployment <FRONTEND_IDENTIFIER> --type=LoadBalancer --port 80 --target-port 8080
```
check frontend IP completion
```
kubectl get all
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
DONE
