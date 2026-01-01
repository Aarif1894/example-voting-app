

Deploy Example Voting App on Azure Kubernetes Service (AKS)

This guide explains how to deploy the Docker Example Voting App to Azure Kubernetes Service (AKS) using the Kubernetes manifests provided in the repository.

Repository:
https://github.com/dockersamples/example-voting-app


Architecture Overview
Vote UI → Redis → Worker → PostgreSQL → Result UI

All components are deployed as Kubernetes pods and exposed via Kubernetes services.




Login to Azure:

az login

1: Clone the Repository
git clone https://github.com/dockersamples/example-voting-app.git
cd example-voting-app


Kubernetes manifests are located in:

k8s-specifications/

2: Create an Azure Resource Group
az group create \
  --name votingapp-rg \
  --location eastus

3: Create an AKS Cluster
az aks create \
  --resource-group votingapp-rg \
  --name votingapp-aks \
  --node-count 2 \
  --enable-addons monitoring \
  --generate-ssh-keys


4: Configure kubectl Access
az aks get-credentials \
  --resource-group votingapp-rg \
  --name votingapp-aks


5: Verify connectivity:

kubectl get nodes


6: Deploy the Application

Apply all Kubernetes manifests:

kubectl apply -f k8s-specifications/


Verify pods and services:

kubectl get pods
kubectl get svc

7: Access the Application

Retrieve the external IP addresses for the frontend services:

kubectl get svc vote
kubectl get svc result


8: Once the EXTERNAL-IP is assigned, open the following in your browser:

Vote App: http://<vote-external-ip>

Result App: http://<result-external-ip>


9: Verify Deployment

Open the Vote App and submit votes.

Open the Result App and confirm votes are updated in real time.


10: Cleanup

To delete all resources and avoid ongoing charges:

az group delete --name votingapp-rg --yes --no-wait

