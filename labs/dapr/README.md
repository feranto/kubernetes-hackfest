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

    ```bash
    #set variables for docker authentication
    DOCKER_USERNAME=<YOUR_USERNAME>
    DOCKER_PASSWORD=<YOUR_PASSWORD>
    #login with your docker credentials
    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD

    #build the images locally
    docker build -t $DOCKER_USERNAME/data-api:1.0 app/data-api
    docker build -t $DOCKER_USERNAME/flights-api:1.0 app/flights-api
    docker build -t $DOCKER_USERNAME/quakes-api:1.0 app/quakes-api
    docker build -t $DOCKER_USERNAME/weather-api:1.0 app/weather-api
    docker build -t $DOCKER_USERNAME/service-tracker-ui:1.0 app/service-tracker-ui

    #push the images to your dockerhub repo
    docker push $DOCKER_USERNAME/data-api:1.0
    docker push $DOCKER_USERNAME/flights-api:1.0
    docker push $DOCKER_USERNAME/quakes-api:1.0 
    docker push $DOCKER_USERNAME/weather-api:1.0 
    docker push $DOCKER_USERNAME/service-tracker-ui:1.0
    ```
2.  Then we need to add the cosmosdb dapr component to our application

    ```bash
    apiVersion: dapr.io/v1alpha1
    kind: Component
    metadata:
    name: <name>
    spec:
    type: state.azure.cosmosdb
    metadata:
    - name: url
        value: <REPLACE-WITH-URL>
    - name: username
        secretKeyRef:
        name: redis-secret
        key:  redis-password
    - name: password
        secretKeyRef:
        name: redis-secret
        key:  redis-password
    - name: masterKey
        value: <REPLACE-WITH-MASTER-KEY>
    - name: database
        value: <REPLACE-WITH-DATABASE>
    - name: collection
        value: <REPLACE-WITH-COLLECTION>
    ```

3.  Now we modify our data-api helm chart in order for it to work with dapr
