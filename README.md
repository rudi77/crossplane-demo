# Under Construction!!!!

# AKS setup with Crossplane

Setting up an Azure Kubernetes Service (AKS) cluster with Crossplane involves several steps, including configuring Azure resources, setting up Crossplane, and defining your cluster configuration

## Prerequisites
1. Azure Account and CLI Setup
   - Create an Azure Account: If you don't already have one, sign up for an Azure account.
   - Install Azure CLI: Download and install the Azure CLI on your local machine.
   - Login to Azure: Run az login and follow the prompts to log in to your Azure account.
2. Install Crossplane
   - Install Crossplane on your Kubernetes Cluster: You can use a local Kubernetes cluster like K3s, Rancher Desktop etc.. Follow the [Crossplane installation guide](https://docs.crossplane.io/v1.14/software/install/) to install Crossplane.
3. Configure Azure Provider in Crossplane
   - Create a Service Principal: Use Azure CLI to create a service principal for Crossplane with az ad sp create-for-rbac --sdk-auth --role Contributor.
   - Configure the Azure Provider: Save the output of the previous command as a JSON file. Apply this file to your Kubernetes cluster as a secret and then configure the Azure provider in Crossplane using this secret. Refer to the [Azure provider documentation](https://docs.crossplane.io/v1.14/concepts/providers/) for detailed steps.
4. Define Your AKS Cluster Configuration
   - Create Resource Classes: Define custom resource classes in Crossplane for your AKS cluster, including the specifications for master nodes, CPU-based worker nodes, and GPU-based worker nodes.
   - Set Up Networking: Define a Virtual Network (VNet) in your Crossplane configuration to ensure that your AKS cluster operates within a VNet.
   - Create Composition: Define a composition in Crossplane that includes all these resources, linking them together.
5. Deploy the AKS Cluster
   - Create a Resource Claim: Once your compositions and resource classes are defined, create a resource claim in Crossplane to provision the AKS cluster according to your configuration.
  - Monitor the Provisioning: Use Kubernetes commands (like kubectl get) to monitor the status of the resources being provisioned.
