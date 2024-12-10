username="demoadmin"
password="Tecsup0000@@"
az group create --name mv02_group --location centralindia
az vm create --resource-group mv02_group --name MVL2WS2019DC00 --image win2019datacenter --size Standard_B2s --public-ip-sku Standard --admin-username $username --admin-password $password --storage-sku Standard_LRS
az vm disk attach --resource-group mv02_group --vm-name MVL2WS2019DC00 --name data --size-gb 10 --sku Standard_LRS --new
