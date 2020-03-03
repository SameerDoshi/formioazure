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
3. Azure Container Instance
4. Azure Storage Account
5. Azure Redis Cache
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
3. Azure Container Instance
4. Azure Storage Account
5. Azure Redis Cache

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
TODO ***** ENDED HERE*****
4. Publish to Azure Container Instance
Add Azure Storage
Add Azure Redis


Not Covered (todo)
Networking
Work is provided as is, without warranty. 
