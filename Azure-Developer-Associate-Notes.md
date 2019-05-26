# Microsoft Certified: Azure Developer Associate Notes
- Exam AZ-203: Developing Solutions for Microsoft Azure


## Azure Learning Resources
- Azure Code Samples: https://azure.microsoft.com/en-us/resources/samples/?sort=0
- Official Azure Documentation: https://docs.microsoft.com/en-us/azure/
- Azure Citadel - Labs and Workshops: https://azurecitadel.github.io/labs/
- Azure Hands on Labs: https://www.microsoft.com/handsonlabs/SelfPacedLabs
- Official Microsoft Azure YouTube Channel: https://www.youtube.com/user/windowsazure
- Official Microsoft Developer YouTube Channel: https://www.youtube.com/channel/UCsMica-v34Irf9KVTh6xx-g
- Azure REST API Browser: https://docs.microsoft.com/en-us/rest/api/?view=Azure
- Azure Hands on labs for AZ-203: https://microsoftlearning.github.io/AZ-203-DevelopingSolutionsforMicrosoftAzure/

## Virtual Machines
- Can SSH or RDP into the VM to perform tasks
- Can use the VM for a server etc
- To allow for public internet access - enable the publi inbound ports i.e. 80, 443 if it is going to be a web server, or other ports for SSH or RDP (3389)
  - security concerns i.e. put some blocks on RDP, close ports after use etc
- Infrastructure that you are responsible for, i.e. security, updates, OS etc.
- Admin account is for RDP
- Enable Auto-shutdown when creating VM (useful)
- Can use a free domain instead of a IP address for ease of use
- Azure charges you for when you run your VM, as well as storage etc. If you stop your VM, charges will stop but you will still have to pay for Storage costs
- By default, Azure recommends using the default VM size of **Standard DS1**

### Encrypt a OS/Datadisk in VM
- Azure by default encrypts files using Secure Storage Encryption but does not encrypt the VM itself by default except the files (VHD is not encrypted, Go to Disks, look for OS disks and see that the Encryption is not enabled)
- Can use BitLocker to encrypt the Virtual Disk. The keys will be stored in Azure Key Vault
- Create a Key Vault if you don't have one, search for it
  - Stores secrets, application keys, certificates
  - To encrypt the key (needed to encrypt the VHD) using Azure Vault for a VM it has to exist in the same region as the VM
  - Use powershell to encrypt the VM (see microsoft links online)

### Connect to VM using RDP/SSH
- Go to Overtab of VM
- Go to Connect tab then you can see instructions for RDP or SSH
- **Troubleshooting RDP**
  - Restart machine in Azure Portal
  - Check the Firewall on the following:
    - Virtual Network/Subnets (Go to overview and click Virtual network/subnets, check for security groups)
    - Network security groups in Network Interface Card (Go to VM, Networking tab, Network interface card)
      - RDP should be an inbound port rule (port 3389), otherwise you will have to add the rule
    - Worse case is Recreate VM if firewall is on Windows...

## Azure Batch Services
- Allows to upload batch jobs and have azure manage the execution
  - Batched jobs: identical tasks that need to be run multiple times
  - Can run in parrallel etc
- Separate from Azure Subscription, need to create a batch account for the batch services
- See Azure Batch examples on github: https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts, https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/GettingStarted/01_HelloWorld
- Can use Batch explorer to see insights on your batch 

## Containers
- Midway between an IaaS and PaaS
- A way to package code and deploy it to your environment
- Easy to get up and running (with environment replicated etc)

## Docker Containers
- Create our own containers using docker
- Can create our own containers and package it so that it is deployed in Kubernetes cluster
- Download docker - https://hub.docker.com/editions/community/docker-ce-desktop-windows or Docker toolbox https://docs.docker.com/toolbox/toolbox_install_windows/
- docker-compose.yaml contains instructions on how to create the container
- Use command to create containers using the docker-compose file: ``docker-compose up -d``
- See the created containers: ``docker container ls``
- Docker runs in a virtual box (which has an IP address).
  - See ip address: ``docker-machine ip Default``
  - Can now navigate to that ip address and port to see local machine with the copy of the container running
- Use command to stop containers: ``docker-compose down``
- Use command to see images: ``docker images``

### Azure Container Registry (ACR)
- Location where you can store container images for others to use
- Create a container registry in Azure
  - SKU: different types but all cost money (defaults to standard)
- To add docker image to register you tag the image with the ACR name for the registry: ``docker tag {image-name} {registry-name}:v1`` 
  - example: ``docker tag azure-vote-front azsj.azurecr.io/azure-vote-front:v1`` 
- Then push the image: ``docker push {registry-name}:v1`` i.e. ``docker push azsj.azurecr.io/azure-vote-front:v1``

## Azure Kubernetes Service (AKS)
- Azure Kubernetes is a managed Kubernetes service for Azure
- Originally created by Google
- Kubernetes container is a way to package your code and all the dependencies and everything needed to run. 
- Open source
- Supports multiple different cloud providers which allows deployment to Azure, AWS, Google Cloud etc
- Be aware that this is a cluster so there are multiple nodes - which may incurr higher fees
- The Yaml file is the settings and it tells Kubernetes where to get the code to deploy it to the cluster. It has two types of programs: back-end and front-end services.
- Install the Azure CLI to perform local tasks for azure using command line or you can use PowerShell
- **AKS Dashboard**
  - Only available for use locally
  - For local setup, download azure cli or use powerShell
  - View kubernetes dashboard in Azure Portal and copy the commands
- Practise: https://github.com/Azure-Samples/aks-dotnet-manage-kubernetes-cluster

### Create Kubernetes cluster
- DNS name prefix - used for fully qualified domain name
- Node size defaults to Standard DS2 v2 (2 cpus, 7 GB memory), 
- Defaults to 3 nodes
- Service principal: the accounts in which the nodes are going to run
- Minimum nodes is 3

### Deploy Kubernetes cluster
- Using cloud shell cli
- Use command to set up context and connect to cluster: ``az aks get-credentials --resource-group {resource-group-name} --name {cluster-name}``
- Use command to see the nodes: ``kubectl get nodes``
- Deploy code to cluster using a YAML file (see Kubernetes notes): ``kubectl apply -f {yaml-file-name}.yaml``
  - Provision of an external-ip address takes a few minutes
- See services running with the command: ``kubectl get service``
- See more at: https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough

## Azure Concepts
- **Resource group** 
  - A folder that stores resources that are related
  - A way of organising resources
- **Network Security Groups (NSG)**

## Azure Resource Management (ARM) Templates
- Json files that contain all configuration made to create resources
- Automation, can recreate it exactly as it was by redeploying
- Can modify the json files and deploy to create new resources with similar infrastructure i.e. changing location of data center etc
- Can Add to library if want it reusable by other projects, give it a name and save it to the library.
- Add to source control, and you will have a snapshot of the environment that is reproducable
