# HowTo install an Azure Provider for Crossplane

## Prerequisites
- Ensure you have a running Kubernetes cluster with Crossplane installed.
- You need an Azure account and the Azure CLI installed on your local machine.

## Create a Service Principal and Secret
First, create a service principal in Azure. This service principal will be used by Crossplane to manage resources in your Azure account.

#### Login to Azure
```bash
az login
```

#### Create a Service Principal
```bash
az ad sp create-for-rbac --sdk-auth --role Contributor --scopes /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId} > creds.json
```

## Create a Kubernetes Secret
Create a Kubernetes secret with the Azure credentials.

1. Create a Secret in Kubernetes:
Assuming you are in the directory where creds.json is located:
```bash
kubectl create secret generic azure-provider-secret --from-file=credentials=./creds.json -n crossplane-system
```

2. Verify that the secret was created:
```bash
kubectl get secret azure-provider-secret -n crossplane-system
```

## Install and configure the Azure Provider
Now, you'll install the Azure provider in Crossplane and configure it using the secret you just created.

1. Install the Azure provider:
Create a azure-provider.yaml file with the following content:
```yaml
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-azure
spec:
  package: xpkg.upbound.io/upbound/provider-azure:v0.38.2
```

Install the provider:
```bash
kubectl apply -f azure-provider.yaml
```


1. Create a ProviderConfig in Kubernetes:
Create a provider-config.yaml file with the following content:
```yaml
apiVersion: azure.crossplane.io/v1beta1
kind: ProviderConfig
metadata:
  name: azure-provider
spec:
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: azure-provider-secret
      key: credentials
```

1. Apply the ProviderConfig:
```bash
kubectl apply -f provider-config.yaml
```

## Verify Installation
After these steps, you can verify the installation by checking the status of the provider:
```bash
kubectl get providers.pkg.crossplane.io
```

Check the 'ProviderConfig':
```bash
kubectl get providerconfigs
```



