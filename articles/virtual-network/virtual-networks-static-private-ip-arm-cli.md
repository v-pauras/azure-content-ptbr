<properties 
   pageTitle="Como definir um IP estático e privado no modo ARM usando o CLI | Microsoft Azure"
   description="Noções básicas sobre DIPs (IPs estáticos) e como gerenciá-los no modo ARM usando o CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

# Como definir um endereço IP privado estático no CLI do Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Este artigo aborda o modelo de implantação do Gerenciador de Recursos. Você também pode [gerenciar o endereço IP privado estático no modelo de implantação clássico](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Os exemplos de comando abaixo do CLI do Azure esperam um ambiente simples já criado. Se você quiser executar os comandos da forma como eles aparecem neste documento, primeiro crie o ambiente de teste descrito em [criar uma vnet](virtual-networks-create-vnet-arm-cli.md).

## Como especificar um endereço IP privado estático ao criar uma VM
Para criar uma VM denominada *DNS01* na sub-rede *Front-end* de uma VNet chamada *TestVNet* com o endereço IP privado estático *192.168.1.101*, execute as etapas abaixo:

1. Se você nunca usou a CLI do Azure, consulte [Instalar e configurar a CLI do Azure](xplat-cli-install.md) e siga as instruções até o ponto em que você seleciona sua conta e assinatura do Azure.

2. Execute o comando **azure config mode** para alternar para o modo do Gerenciador de Recursos, como mostrado abaixo.

		azure config mode arm

	Saída esperada:

		info:    New mode is arm

3. Execute o **azure network public-ip create** para criar um IP público para a máquina virtual. A lista exibida após a saída explicar os parâmetros usados.

		azure network public-ip create -g TestRG -n TestPIP -l centralus
	
	Saída esperada:

		info:    Executing command network public-ip create
		+ Looking up the public ip "TestPIP"
		+ Creating public ip address "TestPIP"
		+ Looking up the public ip "TestPIP"
		data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
		data:    Name                            : TestPIP
		data:    Type                            : Microsoft.Network/publicIPAddresses
		data:    Location                        : centralus
		data:    Provisioning state              : Succeeded
		data:    Allocation method               : Dynamic
		data:    Idle timeout                    : 4
		info:    network public-ip create command OK

	- **-g (ou --resource-group)**. Nome do grupo de recursos em que o IP público será criado.
	- **-n (ou --name)**. Nome do IP público.
	- **-l (ou --location)**. Região do Azure em que o IP público será criado. Para o nosso cenário, *centralus*.

3. Execute o comando **azure network nic create** para criar uma NIC com um endereço IP estático privado. A lista exibida após a saída explicar os parâmetros usados.

		azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

	Saída esperada:

		+ Looking up the network interface "TestNIC"
		+ Looking up the subnet "FrontEnd"
		+ Creating network interface "TestNIC"
		+ Looking up the network interface "TestNIC"
		data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
		data:    Name                            : TestNIC
		data:    Type                            : Microsoft.Network/networkInterfaces
		data:    Location                        : centralus
		data:    Provisioning state              : Succeeded
		data:    Enable IP forwarding            : false
		data:    IP configurations:
		data:      Name                          : NIC-config
		data:      Provisioning state            : Succeeded
		data:      Private IP address            : 192.168.1.101
		data:      Private IP Allocation Method  : Static
		data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
		data:
		info:    network nic create command OK

	- **- a (ou --private-ip-address)**. Endereço IP privado estático para a NIC.
	- **-m (ou --subnet-vnet-name)**. Nome da VNet na qual a NIC será criada.
	- **-k (ou --subnet-name)**. Nome da sub-rede na qual a NIC será criada.

4. Execute o comando **azure vm create** para criar a VM usando o IP público e a NIC criada acima. A lista exibida após a saída explicar os parâmetros usados.

		azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

	Saída esperada:

		info:    Executing command vm create
		+ Looking up the VM "DNS01"
		info:    Using the VM Size "Standard_A1"
		warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
		info:    The [OS, Data] Disk or image configuration requires storage account
		+ Looking up the storage account vnetstorage
		+ Looking up the NIC "TestNIC"
		info:    Found an existing NIC "TestNIC"
		info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
		info:    Found public ip parameters, trying to setup PublicIP profile
		+ Looking up the public ip "TestPIP"
		info:    Found an existing PublicIP "TestPIP"
		info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
		+ Updating NIC "TestNIC"
		+ Looking up the NIC "TestNIC"
		+ Creating VM "DNS01"
		info:    vm create command OK

	- **-y (ou --os-type)**. Tipo de sistema operacional para a VM, que deve ser *Windows* ou *Linux*.
	- **-f (ou --nic-name)**. Nome da NIC que a VM usará.
	- **-i (ou --public-ip-name)**. Nome do IP público que a VM usará.
	- **-F (ou --vnet-name)**. Nome da VNet na qual a VM será criada.
	- **-j (ou --vnet-subnet-name)**. Nome da sub-rede em que a VM será criada.

## Como recuperar informações do endereço IP privado estático de uma VM

Para exibir as informações do endereço IP privado estático da VM criada com o script acima, execute o seguinte comando da CLI do Azure e observe os valores de *Private IP alloc-method* e *Private IP address*:

	azure vm show -g TestRG -n DNS01

Saída esperada:

	info:    Executing command vm show
	+ Looking up the VM "DNS01"
	+ Looking up the NIC "TestNIC"
	+ Looking up the public ip "TestPIP
	data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
	data:    ProvisioningState               :Succeeded
	data:    Name                            :DNS01
	data:    Location                        :centralus
	data:    Type                            :Microsoft.Compute/virtualMachines
	data:
	data:    Hardware Profile:
	data:      Size                          :Standard_A1
	data:
	data:    Storage Profile:
	data:      Source image:
	data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
	data:
	data:      OS Disk:
	data:        OSType                      :Windows
	data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
	data:        Caching                     :ReadWrite
	data:        CreateOption                :FromImage
	data:        Vhd:
	data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
	data:
	data:    OS Profile:
	data:      Computer Name                 :DNS01
	data:      User Name                     :adminuser
	data:      Windows Configuration:
	data:        Provision VM Agent          :true
	data:        Enable automatic updates    :true
	data:
	data:    Network Profile:
	data:      Network Interfaces:
	data:        Network Interface #1:
	data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
	data:          Primary                   :true
	data:          MAC Address               :00-0D-3A-90-1A-A8
	data:          Provisioning State        :Succeeded
	data:          Name                      :TestNIC
	data:          Location                  :centralus
	data:            Private IP alloc-method :Static
	data:            Private IP address      :192.168.1.101
	data:            Public IP address       :40.122.213.159
	info:    vm show command OK

## Como remover um endereço IP privado estático de uma VM
Você não pode remover um endereço IP privado estático de uma NIC no CLI do Azure para o Gerenciador de Recursos. Você deve criar uma nova NIC que usa um IP dinâmico, remover a NIC anterior da VM e, em seguida, adicionar a nova NIC à VM. Para alterar a NIC para a VM usada nos comandos acima, siga as etapas abaixo.
	
1. Execute o comando **azure network nic create** para criar uma nova NIC usando alocação de IP dinâmico. Observe que você não precisa especificar o endereço IP neste momento.

		azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

	Saída esperada:

		info:    Executing command network nic create
		+ Looking up the network interface "TestNIC2"
		+ Looking up the subnet "FrontEnd"
		+ Creating network interface "TestNIC2"
		+ Looking up the network interface "TestNIC2"
		data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
		data:    Name                            : TestNIC2
		data:    Type                            : Microsoft.Network/networkInterfaces
		data:    Location                        : centralus
		data:    Provisioning state              : Succeeded
		data:    Enable IP forwarding            : false
		data:    IP configurations:
		data:      Name                          : NIC-config
		data:      Provisioning state            : Succeeded
		data:      Private IP address            : 192.168.1.6
		data:      Private IP Allocation Method  : Dynamic
		data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
		data:
		info:    network nic create command OK

2. Execute o comando **azure vm set** para alterar a NIC usada pela VM.

		azure vm set -g TestRG -n DNS01 -N TestNIC2

	Saída esperada:

		info:    Executing command vm set
		+ Looking up the VM "DNS01"
		+ Looking up the NIC "TestNIC2"
		+ Updating VM "DNS01"
		info:    vm set command OK

3. Se desejar, execute o comando **azure network nic delete** para excluir o antigo NIC.

		azure network nic delete -g TestRG -n TestNIC --quiet

	Saída esperada:

		info:    Executing command network nic delete
		+ Looking up the network interface "TestNIC"
		+ Deleting network interface "TestNIC"
		info:    network nic delete command OK

## Como adicionar um endereço IP privado estático a uma VM existente
Para adicionar um endereço IP privado estático à NIC usada pela VM criada com o script acima, execute o comando a seguir:

	azure netwrok nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Saída esperada:

	info:    Executing command network nic set
	+ Looking up the network interface "TestNIC2"
	+ Updating network interface "TestNIC2"
	+ Looking up the network interface "TestNIC2"
	data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
	data:    Name                            : TestNIC2
	data:    Type                            : Microsoft.Network/networkInterfaces
	data:    Location                        : centralus
	data:    Provisioning state              : Succeeded
	data:    MAC address                     : 00-0D-3A-90-29-25
	data:    Enable IP forwarding            : false
	data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
	data:    IP configurations:
	data:      Name                          : NIC-config
	data:      Provisioning state            : Succeeded
	data:      Private IP address            : 192.168.1.101
	data:      Private IP Allocation Method  : Static
	data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
	data:
	info:    network nic set command OK

## Próximas etapas

- Saiba mais sobre endereços [IP públicos reservados](../virtual-networks-reserved-public-ip).
- Saiba mais sobre [endereços ILPIP (IP público em nível de instância)](../virtual-networks-instance-level-public-ip).
- Consulte as [APIs REST de IP reservado](https://msdn.microsoft.com/library/azure/dn722420.aspx).

<!---HONumber=AcomDC_1217_2015-->