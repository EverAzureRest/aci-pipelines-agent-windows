# aci-pipelines-agent-Windows

Azure DevOps Pipelines Windows Agent docker build and templates to deploy in Azure Container Instance with basic private VNET connectivity when that functionality exists.

Currently Windows support is only still in the planning phase for Azure Container Instances.  This project still allows you to build and push the image to a Container Registry, or use ACR to build the image.

## About

This project will build a docker container for an Azure Pipelines Windows Agent.  Once Azure Container Instances have Windows support in a private VNET, the agent can be deployed using the provided template.

## How to use

From a Windows computer with Docker installed, build the dockerfile in the ./docker folder and push it to a container registry.

OR if Azure-Cli is installed and you are using an Azure Container Registry, from the docker folder run ```az acr build -t <registryserver.fqdn/imagename:tag> --platform windows -r <registryuser> .```
This will build and push the image directly to the registry.

## To Deploy when Azure Supports Windows ACI containers in a VNET

These templates assume Admin is enabled on the ACR registry or you have a direct login to your registry.  The given parameters file assumes using Azure Keyvault to store the secrets for both the Registry login and the AzureDevops PAT.  

A subnet in a VNET needs to be delegated to the Azure Container Instance service for the VNET integration to work.  This can be done via the subnet view in the Portal, or any other method, but the subnet must be empty, so I recommend creating a new subnet in an existing VNET, or an entirely new VNET.  /

This deployment also assumes you have a VNET and Subnet already defined.

To deploy the agent container to Azure, from the root of the project run: ```az deployment group create -n <deploymentName> -g <resourceGroup> --template-file ./azuredeploy.json --parameters ./yourparametersfile.json```

