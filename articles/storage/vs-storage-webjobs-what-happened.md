<properties
	pageTitle="O que aconteceu ao meu projeto do WebJob (serviço conectado do Armazenamento do Azure do Visual Studio)? | Microsoft Azure"
	description="Descreve o que acontece em um projeto do Azure WebJob após a conexão a uma conta de armazenamento usando os serviços conectados do Visual Studio"
	services="storage"
	documentationCenter=""
	authors="TomArcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="storage"
	ms.workload="web"
	ms.tgt_pltfrm="vs-what-happened"
	ms.devlang="na"
	ms.topic="article"
	ms.date="12/16/2015"
	ms.author="tarcher"/>

# O que aconteceu ao meu projeto do WebJob (serviço conectado do Armazenamento do Azure do Visual Studio)?

## Referências adicionadas

O pacote do NuGet do Armazenamento do Azure foi adicionado ao seu projeto do Visual Studio. Esse pacote adiciona as referências de .NET a seguir:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## Cadeia de conexão do Armazenamento do Azure adicionada
No arquivo App.config do seu projeto, as entradas **AzureWebJobsStorage** e **AzureWebJobsDashboard** foram atualizadas com a cadeia de conexão e a chave da conta de armazenamento selecionada.

Para obter mais informações, consulte [Recursos Recomendados para Trabalhos Web do Azure](http://go.microsoft.com/fwlink/?linkid=390226).

<!---HONumber=AcomDC_1217_2015-->