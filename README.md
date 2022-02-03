# Fortify Azure Resource Manager Template Demo

Assets for demo of Fortify SCA vulnerability scanning of Azure Resource Manager templates

To use this demo you will need the following:

    - Visual Studio Code
    - Visual Studio Code [Azure Resource Manager tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) extension
    - Fortify SCA and Tools (21.2 or later)
    - Visual Studio Code [Fortify](https://marketplace.visualstudio.com/items?itemName=fortifyvsts.fortify-extension-for-vs-code) extension
    - [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows) or the [Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps) installed and authenticated

To deploy the template (from PowerShell console):    

```
New-AzResourceGroup -Name fortify-arm-demo -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName fortify-arm-demo -TemplateFile ./azuredeploy.json -TemplateParameterFile ./azuredeploy.parameters.json
```

Replace `eastus` with your own region

To clean up the resources (from PowerShell console):

```
Remove-AzResourceGroup -Name fortify-arm-demo
```

To run a Fortify SCA scan (from PowerShell console):

```
.\bin\fortify-sca.ps1
```

To view results:

```
auditworkbench .\AzureRMDemo.fpr
```