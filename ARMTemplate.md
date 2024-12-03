# Ejemplo de una Plantilla de Azure Resource Manager (ARM)

Las fuentes proporcionan información detallada sobre las Plantillas de Azure Resource Manager (ARM) y su uso en la gestión de la infraestructura en Azure. Aunque no ofrecen un ejemplo específico de una plantilla ARM completa, se pueden extraer elementos y conceptos clave para entender su estructura y funcionamiento.

## Ejemplo de una plantilla ARM simple para desplegar una cuenta de almacenamiento en Azure

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Nombre de la cuenta de almacenamiento a crear"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Ubicación de la cuenta de almacenamiento"
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {}
}
