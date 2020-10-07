# Jupyterhub_on_GCP 
October 2020 Tutorial Deploying a Kubernetes cluster and Jupyterhub on Google Cloud using OSX and command line.

Cloud providers have made deploying clusters and applications incredibily easy.
I have seen other tutorials, but because of updates to software those directions do not work.

So these directions are "good" as of Oct. 2020.

Step 1: Have a Google Account. I created a new account on my MAC called gcp, just so I can keep this project seperate from my AWS projects.

Step 2: Download the SDK and extract it. (Complete directions here: https://cloud.google.com/sdk/docs/how-to)
        I am on OSX, so I used this link: https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-313.0.0-darwin-x86_64.tar.gz
        Entered the following commands: 
        wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-313.0.0-darwin-x86_64.tar.gz
        tar xzf google-cloud-sdk-313.0.0-darwin-x86_64.tar.gz

Step 3: Install and Initialize software, the command is gcloud which is in the folder extracted from the SDK, issue the following commands:
        cd google-cloud-sdk
        ./google-cloud-sdk/install.sh
        ./google-cloud-sdk/bin/gcloud init
        At this point, the cli program will attempt to open a browser to connect to your google account.  I had lynx installed, so it was a impossible to login, once I deleted it, it opened by default browser.  At that point, you will see a screen where you can "Enable Google Kubernetes Engine API"  Click on it and you are good to go.
        
Step 4: Install kubectl:        gcloud components install kubectl

Step 5: Create kubernetes cluster: N.B. research your zones and machine-type since costs vary deplending on your choices.
        gcloud container clusters create notebook-test  --num-nodes=5  --machine-type=n1-highmem-2 --zone=us-central1-b
        
Step 6: Install Helm:           brew install kubernetes-helm

Step 7: Add Jupyterhub repository:
        helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/
        helm repo update
        
Step 8: Create your YAML file

Step 9: Initialize your Jupyterhub
        kubectl  create namespace jup
        helm upgrade --install jup jupyterhub/jupyterhub --namespace jup  --version=0.8.2  --values config.yaml

Step 10: Find public IP address of the Jupyterhub instance:
         kube --namespace=rah get svc proxy-public
         Enter that IP address in your browser and login.
