# Lab: Migrating application to run with dapr

This lab walks you through migrating the services tracker microservices app to the dapr runtime in kubernetes. Instead of using the direct cosmosdb sdk's we will use dapr components to communicate with cosmosdb. 

## Prerequisites

* Complete previous labs:
    * [Azure Kubernetes Service](../../create-aks-cluster/README.md)
    * [Build Application Components in Azure Container Registry](../../build-application/README.md)
    * [Helm Setup and Deploy Application](../../helm-setup-deploy/README.md)

* Visual Studio Code (NOT IN AZURE CLOUD SHELL). 

    * This lab is not intended for Azure Cloud Shell. You may need to configure your local machine to match the cloud shell environment. 

## Instructions

1. Push all the service tracker docker images to Dockerhub