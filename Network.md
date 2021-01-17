## Setup and requirements

$ gcloud auth list

$ gcloud config list project

## Task 1: Set the default region and zone for all resources

$ gcloud config set compute/zone us-central1-a

$ gcloud config set compute/region us-central1

## Task 2: Create multiple web server instances

1. Create three new virtual machines
   
$ gcloud compute instances create www1 \
  --image-family debian-9 \
  --image-project debian-cloud \
  --zone us-central1-a \
  --tags network-lb-tag \
  --metadata startup-script="#! /bin/bash
    sudo apt-get update
    sudo apt-get install apache2 -y
    sudo service apache2 restart
    echo '<!doctype html><html><body><h1>www1</h1></body></html>' | tee /var/www/html/index.html"

$ gcloud compute instances create www2 \
  --image-family debian-9 \
  --image-project debian-cloud \
  --zone us-central1-a \
  --tags network-lb-tag \
  --metadata startup-script="#! /bin/bash
    sudo apt-get update
    sudo apt-get install apache2 -y
    sudo service apache2 restart
    echo '<!doctype html><html><body><h1>www2</h1></body></html>' | tee /var/www/html/index.html"

$ gcloud compute instances create www3 \
  --image-family debian-9 \
  --image-project debian-cloud \
  --zone us-central1-a \
  --tags network-lb-tag \
  --metadata startup-script="#! /bin/bash
    sudo apt-get update
    sudo apt-get install apache2 -y
    sudo service apache2 restart
    echo '<!doctype html><html><body><h1>www3</h1></body></html>' | tee /var/www/html/index.html"

2. Create a firewall rule to allow external traffic to the VM instances:

$ gcloud compute firewall-rules create www-firewall-network-lb \
    --target-tags network-lb-tag --allow tcp:80

3. Run the following to list your instances. You'll see their IP addresses in the EXTERNAL_IP column:
   
$ gcloud compute instances list

4. Verify that each instance is running with curl, replacing [IP_ADDRESS] with the IP address for each of your VMs

$ curl http://[IP_ADDRESS]

