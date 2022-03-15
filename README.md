# This Article gives you overview
 - How to create Docker Image of Function App using DevOps CICD Pipeline
 - 
# PreRequisite 

- Visual Studio
- Azure Subscription

# Create Azure Infrastructure components

### Create Resource Group
```
az group create -g "$(ResourceGroupName)" -l "$(ResourceGroupLocation)"
```

### Create Azure Container Registry

``` 
az acr create -n "$(AcrRegistryName)" -g "$(ResourceGroupName)" --sku basic
```

### Create Azure Kubernetes Service

```       
az aks create `
            --resource-group "$(ResourceGroupName)" `
            --location "$(ResourceGroupLocation)"  `
            --name "$(AksClusterName)" `
            --network-plugin $(NetworkPlugin) `
            --kubernetes-version $(KubernetesVersion) `
            --node-vm-size Standard_B2s `
            --node-osdisk-size 0 `
            --node-count $(NodeCount)`
            --load-balancer-sku standard `
            --max-pods 110 `
            --dns-name-prefix microservice-aks-dns `
            --generate-ssh-keys `
            --enable-cluster-autoscaler `
            --min-count 1 `
            --max-count 3 `
            --cluster-autoscaler-profile scale-down-delay-after-add=30s `
            --cluster-autoscaler-profile scale-down-unneeded-time=1m 
 ```

# Create Service Connection for Azure Container Registry

![image](https://user-images.githubusercontent.com/6815990/158156325-c20de667-b4ef-4d3a-8a40-f4e2513383b2.png)

# attach the aks to the acr so it can grab the image from the acr

```
az aks update -n microservice-aks -g MicroserviceDemo --attach-acr acrmicroservicedemo
```
