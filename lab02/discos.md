# Desplegar Máquina Virtual en Azure

En este ejemplo, aprenderás a crear un grupo de recursos, desplegar una máquina virtual y adjuntar un disco de datos adicional utilizando Azure CLI.

```bash
# Definir el nombre de usuario y la contraseña de administrador.
username="demoadmin"
password="Tecsup0000@@"
```
Crear un grupo de recursos en la región central de India.
```bash
az group create --name mv02_group --location centralindia
```

Crear una máquina virtual con la siguiente configuración:
- Nombre: MVL2WS2019DC00
- Imagen: Windows Server 2019 Datacenter
- Tamaño: Standard_B2s
- Dirección IP pública: SKU Standard
- Almacenamiento: SKU Standard_LRS
  
```bash
az vm create \
  --resource-group mv02_group \
  --name MVL2WS2019DC00 \
  --image win2019datacenter \
  --size Standard_B2s \
  --public-ip-sku Standard \
  --admin-username $username \
  --admin-password $password \
  --storage-sku Standard_LRS
```

Adjuntar un nuevo disco de datos de 10 GB con SKU Standard_LRS.
- El disco se llamará 'data' y tendrá un tamaño de 10 GB.
- Usará el SKU de almacenamiento 'Standard_LRS'.
```bash
az vm disk attach \
  --resource-group mv02_group \
  --vm-name MVL2WS2019DC00 \
  --name data \
  --size-gb 10 \
  --sku Standard_LRS \
  --new
```
