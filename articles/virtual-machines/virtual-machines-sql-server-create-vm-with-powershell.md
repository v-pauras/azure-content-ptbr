<properties 
	pageTitle="Criar uma Máquina Virtual do SQL Server no PowerShell | Microsoft Azure"
	description="Fornece etapas e scripts do PowerShell para criar uma VM do Azure com imagens da galeria de máquinas virtuais do SQL Server."
	services="virtual-machines"
	documentationCenter="na"
	authors="rothja"
	manager="jeffreyg"
	editor="monicar" 
	tags="azure-service-management"
	 />
<tags 
	ms.service="virtual-machines"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="08/26/2015"
	ms.author="jroth" />

# Criar uma máquina virtual do SQL Server no Azure (PowerShell)

> [AZURE.SELECTOR]
- [Azure classic portal](virtual-machines-provision-sql-server.md)
- [PowerShell](virtual-machines-sql-server-create-vm-with-powershell.md)

## Visão geral

Este artigo fornece as etapas para a criação de uma máquina virtual do SQL Server no Azure usando os cmdlets do PowerShell.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Modelo do Gerenciador de Recursos.


## Instalar e configurar o Azure PowerShell

1. Se você não tiver uma conta do Azure, visite [Avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/). 
 
2. [Instale os cmdlets mais recentes do Azure PowerShell](../powershell-install-configure.md/#how-to-install-azure-powershell).

3. [Conecte o PowerShell com sua assinatura do Azure](../powershell-install-configure.md/#how-to-connect-to-your-subscription).

## Determinar o região de destino do Azure

A máquina virtual do SQL Server será hospedada em um serviço de nuvem que reside em uma região específica do Azure. As etapas a seguir ajudam a determinar sua região, a conta de armazenamento e o serviço de nuvem que será usado para o restante do tutorial.

1. Determine o data center que você deseja usar para hospedar a VM do SQL Server. Os comandos do PowerShell a seguir exibirão detalhadamente as regiões disponíveis com uma lista de resumo no final.

		Get-AzureLocation
		(Get-AzureLocation).Name

2.  Depois de identificar seu local preferido, defina uma variável chamada **$dcLocation** para essa região.

		$dcLocation = "<region name>"

## Definir a assinatura e a conta de armazenamento

1. Determine a assinatura do Azure que você usará para a nova máquina virtual.

		(Get-AzureSubscription).SubscriptionName

1. Atribua sua assinatura de destino do Azure à variável **$subscr**. Em seguida, defina isso como sua assinatura atual do Azure.

		$subscr="<subscription name>"
		Select-AzureSubscription -SubscriptionName $subscr –Current

1. Verifique se há contas de armazenamento existentes. O script a seguir exibe todas as contas de armazenamento existentes em sua região escolhida:

		(Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

	>[AZURE.NOTE]Se precisar de uma nova conta de armazenamento, primeiro crie um nome de conta de armazenamento com todas as letras minúsculas usando o comando New-AzureStorageAccount, como mostra o seguinte exemplo: **New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation**

1. Atribua o nome da conta de armazenamento de destino à **$staccount**. Em seguida, use **Set-AzureSubscription** para definir a assinatura e a conta de armazenamento atual.

		$staccount="<storage account name>"
		Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## Selecione uma imagem de máquina virtual do SQL Server

1. Descubra a lista de imagens de máquinas virtuais do SQL Server disponíveis na Galeria. Todas essas imagens têm uma propriedade **ImageFamily** que começa com “SQL”. A consulta a seguir exibe a família de imagens disponível com o SQL Server pré-instalado.

		Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Quando você encontrar a família de imagens da máquina virtual, talvez exista várias imagens publicadas nessa família. Use o script a seguir para localizar o nome da imagem de máquina virtual publicado mais recentemente para sua família de imagens selecionada (por exemplo, **SQL Server 2014 SP1 Enterprise no Windows Server 2012 R2**):

		$family="<ImageFamily value>"
		$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

		echo "Selected SQL Server image name:"
		echo "   $image"

## Criar a máquina virtual

Por fim, crie a máquina virtual com o PowerShell:

1. Crie um serviço de nuvem para hospedar a nova VM. Observe que também é possível usar um serviço de nuvem existente. Crie uma nova variável **$svcname** com o nome curto do serviço de nuvem.

		$svcname = "<cloud service name>"
		New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Especifique o nome e o tamanho da máquina virtual. Para obter mais informações sobre tamanhos de máquina virtual, veja [Tamanhos de máquina virtual para o Azure](virtual-machines-size-specs.md).

		$vmname="<machine name>"
		$vmsize="<Specify a valid machine size>" # see the link to virtual machine sizes
		$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Especifique a conta de administrador local e a senha.

		$cred=Get-Credential -Message "Type the name and password of the local administrator account."
		$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Execute o script a seguir para criar a máquina virtual.

		New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE]Para obter explicações adicionais e outras opções de configuração, veja a seção **Compilar o conjunto de comandos** em [Usar o Azure PowerShell para criar e pré-configurar Máquinas Virtuais baseadas em Windows](virtual-machines-ps-create-preconfigure-windows-vms.md).

## Exemplo de script do PowerShell

O script a seguir fornece um exemplo de um script completo que cria uma máquina virtual do **SQL Server 2014 SP1 Enterprise no Windows Server 2012 R2**. Se você usar esse script, personalize as variáveis iniciais com base nas etapas anteriores deste tópico.

	# Customize these variables based on your settings and requirements:
	$dcLocation = "East US"
	$subscr="mysubscription"
	$staccount="mystorageaccount"
	$family="SQL Server 2014 SP1 Enterprise on Windows Server 2012 R2"
	$svcname = "mycloudservice"
	$vmname="myvirtualmachine"
	$vmsize="A5" 
	
	# Set the current subscription and storage account
	# Comment out the New-AzureStorageAccount line if the account already exists
	Select-AzureSubscription -SubscriptionName $subscr –Current
	New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
	Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
	
	# Select the most recent VM image in this image family:
	$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
	
	# Create the new cloud service; comment out this line if cloud service exists already:
	New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
	
	# Create the VM config:
	$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
	
	# Set administrator credentials:
	$cred=Get-Credential -Message "Type the name and password of the local administrator account."
	$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
	
	# Create the SQL Server VM:
	New-AzureVM –ServiceName $svcname -VMs $vm1
	 

## Conectar-se à área de trabalho remota

1. Crie os arquivos RDP na pasta de documentos do usuário atual para iniciar essas máquinas virtuais e concluir a instalação:

		$documentspath = [environment]::getfolderpath("mydocuments")
		Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. No diretório de documentos, inicie o arquivo RDP. Conecte-se com o nome de usuário e a senha de administrador fornecidos anteriormente (por exemplo, se o nome de usuário era VMAdmin, especifique "\\VMAdmin" como o usuário e forneça a senha).

		.\vm1.rdp

## Concluir a configuração da Máquina do SQL Server para acesso remoto

Depois de fazer logon no computador com a área de trabalho remota, configure o SQL Server com base nas instruções em [Etapas para configurar a conectividade do SQL Server em uma VM do Azure](virtual-machines-sql-server-connectivity.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## Próximas etapas

Encontre mais instruções para o provisionamento de máquinas virtuais com o PowerShell na [documentação das máquinas virtuais](virtual-machines-ps-create-preconfigure-windows-vms.md). Para obter scripts adicionais relacionados ao SQL Server e ao Armazenamento Premium, veja [Usar o Armazenamento Premium do Azure com o SQL Server em Máquinas Virtuais](virtual-machines-sql-server-use-premium-storage.md).

Em muitos casos, a próxima etapa é migrar os bancos de dados para essa nova VM do SQL Server. Para obter orientações sobre a migração de banco de dados, veja [Migrando um banco de dados para o SQL Server em uma VM do Azure](virtual-machines-migrate-onpremises-database.md).

Se você também está interessado em ver como executar essas etapas no portal clássico do Azure, consulte [Provisionar uma máquina virtual do SQL Server no Azure](virtual-machines-provision-sql-server.md).

Além desses recursos, recomendamos ver [outros tópicos relacionados à execução do SQL Server em Máquinas Virtuais do Azure](virtual-machines-sql-server-infrastructure-services.md).

<!---HONumber=AcomDC_1203_2015-->