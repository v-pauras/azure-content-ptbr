<properties 
   pageTitle="Introdução ao Repositório Data Lake | Azure" 
   description="Usar o Azure PowerShell para criar uma conta do Repositório Data Lake e executar operações básicas" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/04/2015"
   ms.author="nitinme"/>

# Introdução ao Repositório Azure Data Lake usando o Azure PowerShell

> [AZURE.SELECTOR]
- [Using Portal](data-lake-store-get-started-portal.md)
- [Using PowerShell](data-lake-store-get-started-powershell.md)
- [Using .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Using Azure CLI](data-lake-store-get-started-cli.md)
- [Using Node.js](data-lake-store-manage-use-nodejs.md)

Saiba como usar o Azure PowerShell para criar uma conta do Repositório Azure Data Lake e executar operações básicas, como criar pastas, carregar e baixar arquivos de dados, excluir sua conta, etc. Para obter mais informações sobre o Repositório Data Lake, veja [Visão geral do Repositório Data Lake](data-lake-store-overview.md).

## Pré-requisitos

Antes de começar este tutorial, você deve ter o seguinte:

- **Uma assinatura do Azure**. Consulte [Obter avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Habilite sua assinatura do Azure** para a visualização pública do Repositório Data Lake. Veja [instruções](data-lake-store-get-started-portal.md#signup).


##Instalar o Azure PowerShell 1.0 e superior

Para começar, você deve desinstalar as versões 0.9x do Azure PowerShell. Para verificar a versão do PowerShell instalado, execute o seguinte comando em uma janela do PowerShell:

	Get-Module *azure*
	
Para desinstalar a versão mais antiga, execute **Programas e Recursos** no painel de controle e remove a versão instalada se ela for anterior ao PowerShell 1.0.

Há duas opções principais para a instalação do Azure PowerShell.

- [Galeria do PowerShell](https://www.powershellgallery.com/). Execute os seguintes comandos no PowerShell ISE elevado ou no console Windows PowerShell elevado:

		# Install the Azure Resource Manager modules from PowerShell Gallery
		Install-Module AzureRM
		Install-AzureRM
		
		# Install the Azure Service Management module from PowerShell Gallery
		Install-Module Azure
		
		# Import AzureRM modules for the given version manifest in the AzureRM module
		Import-AzureRM
		
		# Import Azure Service Management module
		Import-Module Azure

	Para obter mais informações, veja [Galeria do PowerShell](https://www.powershellgallery.com/).

- [Microsoft Web Platform Installer (WebPI)](http://aka.ms/webpi-azps). Se você tem o Azure PowerShell 0.9.x instalado, será solicitado a desinstalar o 0.9.x. Se tiver instalado os módulos do Azure PowerShell na Galeria do PowerShell, o instalador exigirá que os módulos sejam removidos antes da instalação para assegurar a consistência do Ambiente do Azure PowerShell. Para obter instruções, veja [Instalar o Azure PowerShell 1.0 via WebPI](https://azure.microsoft.com/blog/azps-1-0/).

A WebPI receberá atualizações mensais. A Galeria do PowerShell receberá atualizações continuamente. Se você estiver familiarizado com a instalação a partir da Galeria do PowerShell, esse será o primeiro canal para os melhores e mais recentes recursos do Azure PowerShell.

## Criar uma conta do Repositório Azure Data Lake

1. Na área de trabalho, abra uma nova janela do Azure PowerShell e insira o trecho de código a seguir para fazer logon em sua conta do Azure, definir a assinatura e registrar o provedor do Repositório Data Lake. Quando solicitado a fazer logon, lembre-se de fazer logon como o proprietário ou um dos administradores da assinatura:

        # Log in to your Azure account
		Login-AzureRmAccount
        
		# List all the subscriptions associated to your account
		Get-AzureRmSubscription
		
		# Select a subscription 
		Set-AzureRmContext -SubscriptionId <subscription ID>
        
		# Register for Azure Data Lake Store
		Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore" 


2. Uma conta do Repositório Azure Data Lake está associada a um Grupo de Recursos do Azure. Comece criando um Grupo de Recursos do Azure.

		$resourceGroupName = "<your new resource group name>"
    	New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

	![Criar um grupo de recursos do Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Criar um grupo de recursos do Azure")

2. Crie uma conta do Repositório Azure Data Lake. O nome especificado deve conter apenas letras minúsculas e números.

		$dataLakeStoreName = "<your new Data Lake Store name>"
    	New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

	![Criar uma conta do Repositório Azure Data Lake](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Criar uma conta do Repositório Azure Data Lake")

3. Verifique se a conta foi criada com êxito.

		Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

	A saída para isso deve ser **True**.

## Criar estruturas de diretório no Repositório Azure Data Lake

Você pode criar diretórios em sua conta do Repositório Azure Data Lake para gerenciar e armazenar dados.

1. Especifique um diretório raiz.

		$myrootdir = "/"

2. Crie um novo diretório chamado **mynewdirectory** na raiz especificada.

		New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Verifique se o novo diretório foi criado com êxito.

		Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

	Ele deve mostrar uma saída semelhante à seguinte:

	![Verificar diretório](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verificar diretório")


## Carregar dados no Repositório Azure Data Lake

É possível carregar seus dados no Repositório Data Lake diretamente no nível da raiz ou em um diretório que você criou na conta. Os trechos de código abaixo demonstram como carregar alguns dados de exemplo no diretório (**mynewdirectory**) criado na seção anterior.

Se estiver procurando alguns dados de exemplo para carregar, é possível obter a pasta **Dados da Ambulância** no [Repositório Git do Azure Data Lake](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData). Baixe o arquivo e armazene-o em um diretório local no computador, como C:\\sampledata.

	Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## Renomear, baixar e excluir dados do Repositório Data Lake

Para renomear um arquivo, use o seguinte comando:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Para baixar um arquivo, use o seguinte comando:

	Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Para excluir um arquivo, use o seguinte comando:

	Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv 
	
Quando solicitado, insira **Y** para excluir o item. Se você tiver mais de um arquivo para excluir, você pode fornecer todos os caminhos separados por vírgula.

	Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## Excluir sua conta do Repositório Azure Data Lake

Use o comando a seguir para excluir sua conta do Repositório Data Lake.

	Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Quando solicitado, insira **Y** para excluir a conta.


## Outras maneiras de criar uma conta do Repositório Data Lake

- [Introdução ao Repositório Data Lake usando o Portal](data-lake-store-get-started-portal.md)
- [Introdução ao Repositório Data Lake usando o SDK do .NET](data-lake-store-get-started-net-sdk.md)
- [Introdução ao Repositório Data Lake usando o SDK do CLI](data-lake-store-get-started-cli.md)


## Próximas etapas

- [Proteger dados no Repositório Data Lake](data-lake-store-secure-data.md)
- [Usar a Análise Data Lake do Azure com o Repositório Data Lake](data-lake-analytics-get-started-portal.md)
- [Usar o Azure HDInsight com o Repositório Data Lake](data-lake-store-hdinsight-hadoop-use-portal.md)

<!---HONumber=AcomDC_1217_2015-->