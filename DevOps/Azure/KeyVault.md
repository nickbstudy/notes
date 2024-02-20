'Service Principals' are used to share key vaults.

On the portal open the cloud shell, `az ad sp create-for-fbac -n keyvaultdemonamehere`

Copy all info for later use.

Get Azure subscription ID with command `az account show` and copy as well

Create a 'Key Vault' resource from the portal

Back in project settings under Pipelines create a new Service Connection