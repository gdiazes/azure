### Comando para ejecutar en la consola (CLI de Azure)
# Definir el nombre de usuario y la contraseña de administrador.
```bash
username="demoadmin"
password="Tecsup0000@@"
```

Ejecuta el siguiente bloque de comandos. Este script crea primero el grupo de recursos y luego la máquina virtual:

```bash
# 1. Definir variables para mantener el código ordenado
rgName="mv01_group"
vmName="MVL2WS2025DC01"
location="chilecentral"

# 2. Crear el grupo de recursos (si no existe)
az group create --name $rgName --location $location

# 3. Crear la máquina virtual con Windows Server 2025
az vm create \
  --resource-group $rgName \
  --name $vmName \
  --image MicrosoftWindowsServer:windowsserver:2025-datacenter-g2:latest \
  --size Standard_D2s_v3 \
  --admin-username $username \
  --admin-password $password \
  --nsg-rule RDP HTTP HTTPS --verbose \
  --location $location
```

### Explicación de los parámetros clave:

*   **`--image`**: Utilizamos el URN `MicrosoftWindowsServer:windowsserver:2025-datacenter-g2:latest`. Esta es la imagen oficial de Windows Server 2025 Datacenter optimizada para Azure de 2da generación.
*   **`--size`**: He configurado `Standard_D2s_v3`. Si en tu región específica (`chilecentral`) este tamaño también presenta problemas de capacidad, puedes cambiarlo por `Standard_DS1_v2` o verificar disponibilidad con `az vm list-sizes --location chilecentral --output table`.
*   **`--admin-username` y `--admin-password`**: Asegúrate de que las variables `$username` y `$password` estén definidas en tu terminal antes de ejecutarlo. Si prefieres no usar variables, puedes reemplazar `$username` por tu nombre de usuario y `$password` por tu contraseña entre comillas.
*   **`--location`**: Está configurado para `chilecentral` según tus pruebas anteriores. Si el error de `SkuNotAvailable` persiste, intenta cambiar esta variable por `eastus`.

**Nota de seguridad:** Si el comando te solicita términos de la imagen, Azure te indicará que ejecutes `az vm image terms accept --urn ...` antes de poder completar la creación.
