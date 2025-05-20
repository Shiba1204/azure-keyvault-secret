Hello everyone,

In this hands-on lab, I’ll show you where and when to use variables and variable groups in an Azure DevOps YAML pipeline. Later, I’ll demonstrate how to integrate Azure Key Vault secrets with variable groups and use the same variable group in the DevOps pipeline.

Let’s walk through the step-by-step implementation in the lab.

I’m logged into the Azure DevOps portal and will be using the Lab Shell organization, working within the Demo DevOps project. If you’ve seen my previous videos, you’ll know I use this project for all Azure DevOps demonstrations.

Step 1: Create Azure Key Vault and Secrets
We’ll begin by deploying a Key Vault, where we’ll create two secrets: a username and a password. These secrets will be used to deploy a virtual machine in the Azure environment.

To create the VM, I’ll use ARM templates that I’ve already prepared and stored in the Azure DevOps repository, which I’ll show you shortly.

Step 2: Configure the Pipeline
In the pipeline, I’ll demonstrate how to use both pipeline variables and variable groups, and explain when and how to use each.

We’ll start by creating the Key Vault in the Azure portal:

Search for Key Vaults and create a new one.
Create a new resource group named RG-KV.
Name the Key Vault lab-key-00001.
Choose the Australia East region and standard pricing.
Set the retention period to 7 days.
Enable Role-Based Access Control (RBAC).
Once the Key Vault is created, we’ll add the secrets. If you don’t have permission to create or view secrets, assign yourself the Key Vault Secrets Officer role.

Create two secrets:

username
password
These will be used for the virtual machine login.

Step 3: Use ARM Templates from Azure DevOps Repo
Navigate to the Azure DevOps portal and go to Repositories. I’ll use a repo named Secrets, which contains the ARM templates.

The template:

Accepts parameters like VM name, admin credentials, location, VNet, subnet, etc.
Creates the VNet, subnet, public IP, NIC, and finally the VM.
The parameters file includes all values except the admin username and password, which we’ll pull from Key Vault.

Step 4: Create the Pipeline
Create a new pipeline using the Secrets repo.
Set the trigger to none to prevent automatic runs.
Use the windows-latest Microsoft-hosted agent.
Add a PowerShell task to create a resource group using pipeline variables:
resourceGroupName = RG-variable-group-test
location = Australia East
You can define variables directly in the pipeline or use a variable group.

Step 5: Create and Link Variable Group to Key Vault
Go to Library and create a new variable group.
Link it to the Azure Key Vault using the service connection.
If the service connection lacks access, assign it the Key Vault Secrets User role.
Import the username and password secrets into the variable group.
Grant access to the specific pipeline.
Step 6: Deploy the Virtual Machine
Add an ARM Template Deployment task.
Use linked artifacts to reference the template and parameters files.
Override the adminUsername and adminPassword parameters using the secrets from the variable group.
Define the variable group in the pipeline YAML:

Step 7: Run and Verify
Run the pipeline.
It will:
Download secrets from Key Vault.
Set up the environment.
Deploy the virtual machine using the ARM template.
Verify the deployment in the Azure portal. Use the public IP and credentials to RDP into the VM.

Summary
In this lab, we:

Created a Key Vault and stored secrets.
Linked those secrets to a variable group.
Used both pipeline variables and variable groups in a YAML pipeline.
Deployed a virtual machine securely using secrets from Azure Key Vault.
This approach ensures secure, reusable, and scalable DevOps practices.

Thank you for watching! Please like and subscribe if you found this helpful.
