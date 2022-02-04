# Fortify AST of Azure Resource Manager (ARM) templates

This is an example project for the demonstration of Fortify Application Security Testing of Azure Resource Manager templates. It also includes a simple Java application so that both application code and infrastructure code can be security scanned simultaneously.

To use this demo you will need the following:

* Visual Studio Code
* Visual Studio Code [Azure Resource Manager tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) extension
* Fortify SCA and Tools (21.2 or later)
* Visual Studio Code [Fortify](https://marketplace.visualstudio.com/items?itemName=fortifyvsts.fortify-extension-for-vs-code) extension
* [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows) or the [Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps) installed and authenticated

Setup
-----

If you are going to deploy the application to Azure you will need to choose a unique name, it is recommend
you choose something along the line of `[your-initials]-fortify-java-arm`, e.g. `kal-fortify-java-arm`.

First create a file called `.env` in the project root directory with content similar to the following:

```
# The URL of Software Security Center
SSC_URL=http://ftfydemo:8080/ssc
SSC_USERNAME=admin
SSC_PASSWORD=admin
# SSC Authentication Token (recommended to use CIToken)
SSC_AUTH_TOKEN=XXXXX
# Name of the application in SSC
SSC_APP_NAME=JavaARM
# Name of the application version in SSC
SSC_APP_VER_NAME=main
# Azure (Resource Manager)
# Your Azure subscription id
AZURE_SUBSCRIPTION_ID=XXXXX
AZURE_RESOURCE_GROUP=xxx-fortify-java-arm
# The name of the App, replace "xxx-fortify-java-arm" with a unique name, e.g. "[your_initials]-fortify-java-arm"
AZURE_APP_NAME=fortify-java-armWeb
# Your desired Azure region, note not all regions allow MySQL databases to be created
AZURE_REGION=eastus
```

Make sure you set an appropriate value for `AZURE_SUBSCRIPTION_ID` and if you want to upload the results to SSC
`SSC_URL` and `SSC_AUTH_TOKEN`.

Next update the file `azuredeploy.parameters.json` with content similar to the following:

```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "appDnsPrefix": {
            "value": "kal-fortify-java-arm"
        },
        "mySqlAdminLogin": {
            "value": "mysql"
        },
        "mySqlAdminPassword": {
            "value": "Password123!"
        }
    }
}
```

Security Scan
-------------

To run a Fortify SCA scan (from PowerShell console) you can use the included script:

```
.\bin\fortify-sca.ps1
```

This will scan the Java applications source code and the Azure Resource Management infrastructure definition.

You can also use the Fortify VS Code plugin, for example if you just wanted to scan the Azure Resoure Manager template
`azuredeploy.json`.

To view results:

```
auditworkbench .\JavaARM.fpr
```

Deploy
------

If you want build the Azure infrstructure from the included template (and maybe run a WebInspect scan on it), carry out the following
(from PowerShell console):    

```
New-AzResourceGroup -Name fortify-java-arm -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName fortify-java-arm -TemplateFile ./azuredeploy.json -TemplateParameterFile ./azuredeploy.parameters.json
```

Replace `eastus` with your own region

Wait a few minutes...

To deploy the web application:

```
.\gradlew.bat clean build
.\gradlew.bat azureWebAppDeploy
```

The application should then be available at the URL listed when the template was deployed.

To clean up the resources (from PowerShell console):

```
Remove-AzResourceGroup -Name fortify-arm-demo
```

Kevin A. Lee - kevin.lee@microfocus.com
