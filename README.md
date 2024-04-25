### Overview
---
In this exercise, you use ACR Tasks to perform the following actions:

- Create an Azure Container Registry
- Build and push an image using a Dockerfile
- Verify the results
- Run the image in the ACR

### Key Aspects
---
Not applicable 

### Environment
---
- Microsoft Azure Portal
  - Valid Subscription

### Actions
---
Create an Azure Container Registry

- Sign in to the Azure portal and open the Cloud Shell

- When the shell opens select the Bash environment

- Create a resource group for the resources needed for this exercise
```
az account list-locations --output table
DEFAULTLOCATION="eastus2"
CURRENTMOMENT="20240425154500"
RESOURCEGROUPNAME="myresourcegroup${CURRENTMOMENT}"
az group create --location ${DEFAULTLOCATION} --name ${RESOURCEGROUPNAME}
```

- Create the Azure Container Registry
```
ACRNAME="myacr${CURRENTMOMENT}"
az acr create --name ${ACRNAME} --resource-group ${RESOURCEGROUPNAME} --sku basic
```


Build and push an image from a Dockerfile

- Create a Dockerfile
```
echo FROM mcr.microsoft.com/hello-world > Dockerfile
```

- Builds the image and pushes it to the ACR tasks
```
az acr build --registry ${ACRNAME} --image sample/hello-world:v1 --file Dockerfile .
```

Verify the results

- List all images in the ACR repository
```
az acr repository list --name ${ACRNAME} --output table
```

- List all tags in the ACR repository
```
az acr repository show-tags --name ${ACRNAME} --repository sample/hello-world --output table
```

Run the image in the ACR
```
az acr run --registry ${ACRNAME} --cmd '$Registry/sample/hello-world:v1' /dev/null
```

Clean up resources

- Delete the Resource Groups and its content
```
az group delete --name ${RESOURCEGROUPNAME} --no-wait
```

### Media
---
![image-20240425-152721](https://github.com/ViCunha/Lab-Azure-AzureContainerRegistry-TasksPipeline-OnAzureCloudShell/assets/65992033/f4ae1590-a680-44da-839e-25e1f783df4f)
---
![image-20240425-152121](https://github.com/ViCunha/Lab-Azure-AzureContainerRegistry-TasksPipeline-OnAzureCloudShell/assets/65992033/70741561-6a1a-40f1-bd36-ec7a5247fb22)
---
![image-20240425-152108](https://github.com/ViCunha/Lab-Azure-AzureContainerRegistry-TasksPipeline-OnAzureCloudShell/assets/65992033/420508fb-978e-4f65-a995-065806038d88)
---
![image-20240425-151517](https://github.com/ViCunha/Lab-Azure-AzureContainerRegistry-TasksPipeline-OnAzureCloudShell/assets/65992033/8069bb34-8b41-4c04-b941-d7f559ab8f1f)
---
![image-20240425-150936](https://github.com/ViCunha/Lab-Azure-AzureContainerRegistry-TasksPipeline-OnAzureCloudShell/assets/65992033/852c26f9-ac60-4f74-90bb-c9754cae344c)

### References
---
(Exercise: Build and run a container image by using Azure Container Registry Tasks)[https://learn.microsoft.com/en-us/training/modules/publish-container-image-to-azure-container-registry/6-build-run-image-azure-container-registry]
