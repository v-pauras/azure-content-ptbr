<properties 
	pageTitle="Data Factory - Log de alterações de API do .NET | Microsoft Azure" 
	description="Descreve alterações significativas, novos recursos e correções de bugs etc... em uma versão específica de API do .NET para o Azure Data Factory." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/10/2015" 
	ms.author="spelluru"/>

# Azure Data Factory - Log de alterações SDK do .NET 
Este artigo fornece informações sobre alterações no SDK do Azure Data Factory em uma versão específica. Você pode baixar o mais recente pacote Nuget para o Azure Data Factory [aqui](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories).

## Versão 4.2.0
Data do lançamento: 10-11-2015

### Adições de recursos

- O novo tipo de atividade a seguir foi adicionado: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Para obter detalhes sobre a atividade, consulte [Atualização dos modelos do ML do Azure usando a Atividade de Recurso de Atualização](https://azure.microsoft.com/documentation/articles/data-factory-azure-ml-batch-execution-activity/#updating-azure-ml-models-using-the-update-resource-activity).
- Uma nova propriedade opcional [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) foi adicionada ao [AzureMLLinkedService classe](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) e [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) propriedades foram adicionadas para a classe [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx). 
- Permitir a configuração de tempos de limite para chamadas de cliente para o serviço de dados de fábrica. 


## Versão 4.1.0
Data de lançamento: 28/10/2015

### Adições de recursos
* Os seguintes tipos de serviço vinculados foram adicionados: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Os seguintes tipos de atividades foram adicionados: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Os seguintes tipos de conjunto de dados foram adicionados: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Os seguintes tipos de fonte e coletor de atividade para Atividade de Cópia foram adicionados:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## Versão 4.0.1
Data do lançamento: 13-10-2015

### Alterações de última hora
As classes a seguir foram renomeadas. Os novos nomes foram os nomes originais de classes antes da liberação 4.0.0.
 
Nome no 4.0.0 | Nome no 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## Versão 4.0.0
Data do lançamento: 02-10-2015

### Alterações de última hora



- As seguintes classes/interfaces foram renomeadas.

| Nome antigo | Novo nome |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabela | [Conjunto de dados](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    
- A **versão da API** para esta versão é: **2015-10-01**.

- Os métodos de **Lista** retornam resultados paginados agora. Se a resposta contém uma propriedade não vazia **NextLink**, o aplicativo cliente precisa continuar a busca da próxima página até que todas as páginas são retornadas. Aqui está um exemplo:

		PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
	    var pipelines = new List<Pipeline>(response.Pipelines);
	
	    string nextLink = response.NextLink;
	    while (string.IsNullOrEmpty(response.NextLink))
	    {
	        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
	        pipelines.AddRange(nextResponse.Pipelines);
	
	        nextLink = nextResponse.NextLink;
	    }
	
- O API da **Lista** retorna apenas o resumo de um pipeline, em vez de todos os detalhes. Por exemplo, as atividades em um resumo de pipeline contém apenas nome e tipo.

### Adições de recursos
- A classe [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) dá suporte a duas novas propriedades, **SliceIdentifierColumnName** e **SqlWriterCleanupScript**, para dar suporte a cópia idempotente para o Azure SQL Data Warehouse. Consulte o artigo [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md), especificamente, as seções [Mecanismo 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) e [Mecanismo 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) para obter detalhes sobre essas propriedades.

- Agora há suporte para executar o procedimento armazenado nas fontes do Banco de Dados SQL do Azure e Azure SQL Data Warehouse como parte da Atividade de Cópia. As classes [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) e [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) têm as seguintes propriedades para dar suporte: **SqlReaderStoredProcedureName** e **StoredProcedureParameters**. Consulte os artigos [Banco de Dados SQL do Azure](data-factory-azure-sql-connector.md#sqlsource) e [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) sobre Azure.com para obter detalhes sobre essas propriedades.

<!---HONumber=Nov15_HO3-->