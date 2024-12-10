# Definir el nombre de usuario y la contraseña para el administrador de la máquina virtual.
username="demoadmin"
password="Tecsup0000@@"

# Crear un grupo de recursos llamado 'mv02_group' en la región 'centralindia'.
az group create --name mv02_group --location centralindia

# Crear una máquina virtual llamada 'MVL2WS2019DC00' en el grupo de recursos 'mv02_group'.
# - La máquina virtual usará la imagen de 'Windows Server 2019 Datacenter'.
# - Tendrá un tamaño 'Standard_B2s'.
# - Se creará y asociará una dirección IP pública con SKU 'Standard'.
# - Se establecerán el nombre de usuario y la contraseña del administrador definidos anteriormente.
# - El tipo de almacenamiento para el disco del sistema operativo será 'Standard_LRS'.
az vm create \
  --resource-group mv02_group \
  --name MVL2WS2019DC00 \
  --image win2019datacenter \
  --size Standard_B2s \
  --public-ip-sku Standard \
  --admin-username $username \
  --admin-password $password \
  --storage-sku Standard_LRS

# Adjuntar un nuevo disco de datos a la máquina virtual.
# - El disco se llamará 'data' y tendrá un tamaño de 10 GB.
# - Usará el SKU de almacenamiento 'Standard_LRS'.
az vm disk attach \
  --resource-group mv02_group \
  --vm-name MVL2WS2019DC00 \
  --name data \
  --size-gb 10 \
  --sku Standard_LRS \
  --new
