# [Build and Secure Networks in Google Cloud: Challenge Lab](https://www.cloudskillsboost.google/focuses/12068?parent=catalog)

## Remove the overly permissive rules
```
gcloud compute firewall-rules delete open-access
```

## Start the bastion host instance
```
gcloud compute instances start bastion --zone=us-east1-b
```

## Create a firewall rule that allows SSH (tcp/22) from the IAP service and add network tag on bastion
```
gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges=35.235.240.0/20 \
  --target-tags=accept-ssh-iap-ingress-ql-232 --network=acme-vpc
gcloud compute instances add-tags bastion --tags=accept-ssh-iap-ingress-ql-232 --zone=us-east1-b
```

## Create a firewall rule that allows traffic on HTTP (tcp/80) to any address and add network tag on juice-shop
```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges=0.0.0.0/0 \
  --target-tags=accept-http-ingress-ql-557 --network=acme-vpc
gcloud compute instances add-tags juice-shop --tags=accept-http-ingress-ql-557 --zone=us-east1-b
```

## Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet
```
gcloud compute firewall-rules create ssh-internal-ingress --allow=tcp:22 --source-ranges=192.168.10.0/24 \
  --target-tags=accept-ssh-internal-ingress-ql-306 --network=acme-vpc
gcloud compute instances add-tags juice-shop --tags=accept-ssh-internal-ingress-ql-306 --zone=us-east1-b
```

## SSH to bastion host via IAP and juice-shop via bastion
