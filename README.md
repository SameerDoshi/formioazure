# formioazure
Deploying FormIO Open Source into Azure PaaS

## About
FormIO is a graphical, web, api driven solution to create, publish, and distribute forms.  An organization can write an app on top of formio and then easily create forms.  Read More on [form.io](https://form.io)

Formio comes in two offerings: open source and enterprise.  This document focuses on deploying the open source version.
Formio provides a guide to [deploying into Azure VMs](https://help.form.io/tutorials/deployment/azure/) but we want to deploy into the PaaS to avoid having to manage VMs.

#Overview
## Get it working locally
1. Clone formio repo
2. Update node package.json
3. npm install
4. npm start --dev
## Create Azure Resources
1. Cosmos DB
2. Azure Container Registry
3. Azure Storage Account
4. Azure Redis Cache
##
Modify our config to run in the PaaS
1. Modify mongo url
2. Run locally and finish setup
3. Push image to Azure Container Registry
4. Publish to Azure Container Instance
Add Azure Storage
Add Azure Redis

# Detailed instructions
## Get it working locally
1. Clone formio repo
git clone (TODO add url)
2. Update node package.json
Edit packages.json and remove version numbers behind mongodb and mongose
3. npm install
This will resolve and download all dependent packages
4. npm start --dev
This will start the app locally

## Create Azure Resources
TODO: Add link to quick starts on creating the below from Azure Portal
1. Cosmos DB
2. Azure Container Registry
3. Azure Storage Account
4. Azure Redis Cache

##Modify our config to run in the PaaS
1. Modify mongo url
go to azure portal and your cosmos instance.  Click Connection strings and copy the connection string.

In config\default.json:
a. Remove MONGO_SA line
b. REMOVE MONGO_PASSWORD line
c. replace MONGO url with the copy/paste connection string.
d. Modify the connection string to include formio before the ? and at teh tail end: &ssl_cert_reqs=CERT_NONE
the line will now look like: 
  "mongo": "mongodb://formsiocosmos:<password>@formsiocosmos.mongo.cosmos.azure.com:10255/formio?ssl=true&ssl_cert_reqs=CERT_NONE",

2. Run locally and finish setup
Now npm start --dev
This time you will be prompted to answer several questions to setup cosmos
Whoo hoo! your cosmos is setup
3. Push image to Azure Container Registry
Now lets create a container and push it to ACR
Go to the Azure Container Regisry in the portal and click admin user.  You'll need info from this page.
run: docker login <replace>.azurecr.io -u <username> -p <access key>

build the container:
docker-compose build

Tag the container
docker tag formio <replace>.azurecr.io/samples/formio
  
Push the container:
docker push <replace>.azurecr.io/samples/formio

After it pushes you can go into the portal and under your container registry under repositories you'll see a sample folder with a formio latest image in there

Reference: [https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli)

4. Publish to Azure Container Instance
Here is Azure CLI code you can use from inside cloud shell to publish
$image="<replace>.azurecr.io/sample/formio:latest"
$RES_GROUP="rg-formio-test"
$ACR_LOGIN_SERVER="<replace>.azurecr.io"
$ACR_User="<replace>"
$ACR_Password="<replace>"
Run this command to create teh ACI instance  
az container create --name formio-test --resource-group $RES_GROUP --image $image --registry-login-server $ACR_LOGIN_SERVER --registry-username $ACR_User --registry-password $ACR_Password --dns-name-label sam33rsamp13-M --query ipAddress.fqdn --ports 80 8080


5. Add Azure Storage


6. Add Azure Redis


Not Covered (todo)
Networking
Work is provided as is, without warranty. 
