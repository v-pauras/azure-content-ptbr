<properties
	pageTitle="Tutorial - Introdução à Biblioteca .NET do Lote do Azure | Microsoft Azure"
	description="Aprenda os conceitos básicos sobre o Azure Batch e a desenvolver para o serviço Batch com um cenário simples"
	services="batch"
	documentationCenter=".net"
	authors="yidingzhou"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.tgt_pltfrm="na"
	ms.workload="big-compute"
	ms.date="09/23/2015"
	ms.author="yidingz"/>

# Introdução à Biblioteca do Azure Batch para .NET  

Comece a trabalhar com a Biblioteca .NET do Lote do Azure criando um aplicativo de console que configura arquivos de suporte e um programa que pode ser executado em vários nós de computação em um pool do Lote do Azure. As tarefas criadas neste tutorial avaliam o texto dos arquivos carregados no Armazenamento do Azure e retornam as palavras que mais aparecem nesses arquivos. Os exemplos são escritos em código C# e usam a [Biblioteca .NET do Lote do Azure](https://msdn.microsoft.com/library/azure/mt348682.aspx).

## Pré-requisitos

- As contas:

	- **Conta do Azure** - você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

	- **Conta do Lote** -consulte [Criar e gerenciar uma conta do Lote do Azure](batch-account-create-portal.md).

	- **Conta de armazenamento** - consulte a seção **Criar uma conta de armazenamento** em [Sobre as contas de armazenamento do Azure](../storage-create-storage-account.md). Neste tutorial, você criará um contêiner na conta denominada **testcon1**.

- Um projeto de aplicativo de console do Visual Studio:

	1.  Abra o Visual Studio e, no menu **Arquivo**, clique em **Novo** e clique em **Projeto**.

	2.	No **Windows**, em **Visual C#**, clique em **Aplicativo de Console**, nomeie o projeto como **GettingStarted**, nomeie a solução como **AzureBatch** e, em seguida, clique em **OK**.

- Os assemblies do NuGet:

	1. Depois de criar seu projeto no Visual Studio, clique com botão direito do mouse no projeto no **Gerenciador de Soluções** e escolha **Gerenciar Pacotes NuGet**. Procure online **Azure.Batch** e clique em **Instalar** para instalar o pacote do Lote do Microsoft Azure e suas dependências.

	2. Pesquise online **WindowsAzure.Storage** e clique em **Instalar** para instalar o pacote Armazenamento do Azure suas dependências.

> [AZURE.TIP]Este tutorial usa alguns dos principais conceitos do Lote discutidos em [Visão geral do recurso Lote do Azure](batch-api-basics.md), leitura altamente recomendada para aqueles que ainda não conhecem o Lote.

## Etapa 1: Criar e carregar os arquivos de suporte

Para dar suporte ao aplicativo, é criado um contêiner no Armazenamento do Azure, os arquivos de texto são criados e, depois, os arquivos de texto e de suporte são carregados para o contêiner.

### Configurar a cadeia de conexão do armazenamento

1. Abra o arquivo App.config para o projeto GettingStarted e adicione o elemento *&lt;appSettings&gt;* a *&lt;configuration&gt;*.

		<?xml version="1.0" encoding="utf-8" ?>
		<configuration>
			<appSettings>
				<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key]"/>
			</appSettings>
		</configuration>

	Substitua esses valores:

	- **[nome-conta]** - o nome da conta de armazenamento que você criou anteriormente.

	- **[chave-conta]** - a chave primária da conta de armazenamento. Você pode encontrar a chave primária da página Armazenamento no portal do Azure.

2. Salve o arquivo App.config.

Para saber mais sobre cadeias de conexão do Armazenamento do Azure, consulte [Configurar cadeias de conexão do Armazenamento do Azure](../storage/storage-configure-connection-string.md).

### Criar o contêiner de armazenamento

1. Adicione-as usando diretivas à parte superior de Program.cs no projeto GettingStarted:

		using System.Configuration;
		using System.IO;
		using Microsoft.WindowsAzure.Storage;
		using Microsoft.WindowsAzure.Storage.Blob;

2. Adicione *System.Configuration* a **Referências** no **Gerenciador de Soluções** para o projeto GettingStarted

3. Adicione este método à classe do Programa que obtém a cadeia de conexão do armazenamento, cria o contêiner e define as permissões:

		static void CreateStorage()
		{
			// Get the storage connection string
			CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
				ConfigurationManager.AppSettings["StorageConnectionString"]);

			// Create the container
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
			CloudBlobContainer container = blobClient.GetContainerReference("testcon1");
			container.CreateIfNotExists();

			// Set permissions on the container
			BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
			containerPermissions.PublicAccess = BlobContainerPublicAccessType.Blob;
			container.SetPermissions(containerPermissions);
			Console.WriteLine("Created the container. Press Enter to continue.");
			Console.ReadLine();
		}

4. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		CreateStorage();

5. Salve o arquivo Program.cs.

	> [AZURE.NOTE]Em um ambiente de produção, é recomendável usar uma [assinatura de acesso compartilhado](https://msdn.microsoft.com/library/azure/ee395415.aspx).

Para saber mais sobre armazenamento de Blob, consulte [Como usar o armazenamento de Blob do .NET](../storage/storage-dotnet-how-to-use-blobs.md)

### Criar o programa de processamento

1. No **Gerenciador de Soluções**, crie um novo projeto de aplicativo de console chamado **ProcessTaskData**.

2. Depois de criar seu projeto no Visual Studio, clique com botão direito do mouse no projeto no **Gerenciador de Soluções** e escolha **Gerenciar Pacotes NuGet**. Procure online **WindowsAzure.Storage** e clique em **Instalar** para instalar o pacote Armazenamento do Azure suas dependências.

3. Adicione o seguinte usando a diretiva à parte superior de Program.cs.

		using Microsoft.WindowsAzure.Storage.Blob;

4. Adicione este código a Principal, que processa o texto dos arquivos:

		string blobName = args[0];
		Uri blobUri = new Uri(blobName);
		int numTopN = int.Parse(args[1]);

		CloudBlockBlob blob = new CloudBlockBlob(blobUri);
		string content = blob.DownloadText();
		string[] words = content.Split(' ');
		var topNWords =
		  words.
			Where(word => word.Length > 0).
			GroupBy(word => word, (key, group) => new KeyValuePair<String, long>(key, group.LongCount())).
			OrderByDescending(x => x.Value).
			Take(numTopN).
			ToList();

		foreach (var pair in topNWords)
		{
			Console.WriteLine("{0} {1}", pair.Key, pair.Value);
		}

5. Salve e compile o projeto ProcessTaskData.

### Criar os arquivos de dados

1. No projeto GettingStarted, crie um novo arquivo de texto chamado **taskdata1.txt**, copie o texto a seguir para ele e salve o arquivo.

	Você poderá usar as Máquinas Virtuais do Azure para provisionar a infraestrutura de computação dimensionável por demanda quando precisar de recursos flexíveis para suas necessidades comerciais. Na galeria, é possível criar máquinas virtuais que executam aplicativos Windows, Linux e corporativos, como o SharePoint e o SQL Server. Ou você pode capturar e usar suas próprias imagens para criar máquinas virtuais personalizadas.

2. Crie um novo arquivo de texto chamado **taskdata2.txt**, copie o texto a seguir para ele e salve o arquivo.

	Implante e gerencie rapidamente aplicativos e serviços poderosos com os serviços de nuvem do Azure. Basta carregar seu aplicativo e o Azure lidará com os detalhes da implantação, desde o provisionamento e o balanceamento de carga até o monitoramento para fornecer disponibilidade contínua. Seu aplicativo é garantido por um Contrato de Nível de Serviço mensal de 99,95% líder do setor. Mantenha seu foco no aplicativo, não na infraestrutura.

3. Crie um novo arquivo de texto chamado **taskdata3.txt**, copie o texto a seguir para ele e salve o arquivo.

	Os Sites do Azure oferecem um ambiente dimensionável, confiável e de fácil utilização para hospedagem de aplicativos Web. Selecione de um intervalo de estruturas e modelos para criar um site em segundos. Use qualquer ferramenta ou sistema operacional para desenvolver seu site com .NET, PHP, Node.js ou Python. Escolha de uma variedade de opções de controle de fonte, incluindo TFS, GitHub e BitBucket para configurar a integração contínua e o desenvolvimento como uma equipe. Expanda sua funcionalidade de site com o tempo aproveitando outros serviços gerenciados do Azure, como armazenamento, CDN e Banco de Dados SQL.

### Carregar os arquivos no contêiner Armazenamento

1. Abra o arquivo Program.cs do projeto **GettingStarted** e adicione este método que carrega os arquivos:

		static void CreateFiles()
		{
		  CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
			ConfigurationManager.AppSettings["StorageConnectionString"]);
		  CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
		  CloudBlobContainer container = blobClient.GetContainerReference("testcon1");

		  CloudBlockBlob taskData1 = container.GetBlockBlobReference("taskdata1");
		  CloudBlockBlob taskData2 = container.GetBlockBlobReference("taskdata2");
		  CloudBlockBlob taskData3 = container.GetBlockBlobReference("taskdata3");
		  taskData1.UploadFromFile("..\\..\\taskdata1.txt", FileMode.Open);
		  taskData2.UploadFromFile("..\\..\\taskdata2.txt", FileMode.Open);
	  	taskData3.UploadFromFile("..\\..\\taskdata3.txt", FileMode.Open);

			CloudBlockBlob storageassembly = container.GetBlockBlobReference("Microsoft.WindowsAzure.Storage.dll");
			storageassembly.UploadFromFile("Microsoft.WindowsAzure.Storage.dll", FileMode.Open);

			CloudBlockBlob dataprocessor = container.GetBlockBlobReference("ProcessTaskData.exe");
		  dataprocessor.UploadFromFile("..\\..\\..\\ProcessTaskData\\bin\\debug\\ProcessTaskData.exe", FileMode.Open);

		  Console.WriteLine("Uploaded the files. Press Enter to continue.");
		  Console.ReadLine();
		}

2. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		CreateFiles();

3. Salve o arquivo Program.cs.

## Etapa 2. Adicionar um pool à sua conta do Lote

Um pool de nós de computação é o primeiro conjunto de recursos que você deve criar para executar tarefas.

1.	Adicione-as usando diretivas à parte superior de Program.cs no projeto GettingStarted:

			using Microsoft.Azure.Batch;
			using Microsoft.Azure.Batch.Auth;
			using Microsoft.Azure.Batch.Common;

2. Adicione este código a Principal, que define as credenciais para fazer chamadas para o serviço de Lote do Azure:

			BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials("[account-url]", "[account-name]", "[account-key]");
			BatchClient client = BatchClient.Open(cred);

	Substitua os valores entre colchetes por aqueles associados à sua conta do Lote, cada um deles pode ser encontrado no [portal do Azure](https://portal.azure.com). Para localizar esses valores, faça logon no [portal do Azure](https://portal.azure.com) e:

	- **[nome-conta]** - clique em **Contas do Lote**, selecione a conta do Lote criada anteriormente
	- **[url-conta]** - na folha Conta do Lote, clique em **Propriedades** > **URL**
	- **[chave-conta]** - na folha Conta do Lote, Clique em **Propriedades** > **Chaves** > **Chave de Acesso Primária**

3.	Adicione esse método à classe Programa, que cria o pool:

			static void CreatePool(BatchClient client)
			{
			  CloudPool newPool = client.PoolOperations.CreatePool(
			    "testpool1",
			    "3",
			    "small",
			    3);
			  newPool.Commit();
			  Console.WriteLine("Created the pool. Press Enter to continue.");
			  Console.ReadLine();
		  	}

4. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		CreatePool(client);

5. Adicione este método à classe Programa, que lista os pools na conta. Isto ajuda a verificar se o pool foi criado:

			static void ListPools(BatchClient client)
			{
				IPagedEnumerable<CloudPool> pools = client.PoolOperations.ListPools();
				foreach (CloudPool pool in pools)
				{
					Console.WriteLine("Pool name: " + pool.Id);
					Console.WriteLine("   Pool status: " + pool.State);
				}
				Console.WriteLine("Press enter to continue.");
				Console.ReadLine();
			}

6. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		ListPools(client);

7. Salve o arquivo Program.cs.

## Etapa 3: Adicionar um trabalho a uma conta

Crie um trabalho que é usado para gerenciar as tarefas executadas no pool. Todas as tarefas devem ser associadas a um trabalho.

1. Adicione esse método à classe Programa, que cria o trabalho:

		static CloudJob CreateJob (BatchClient client)
		{
			CloudJob newJob = client.JobOperations.CreateJob();
			newJob.Id = "testjob1";
			newJob.PoolInformation = new PoolInformation() { PoolId = "testpool1" };
			newJob.Commit();
			Console.WriteLine("Created the job. Press Enter to continue.");
			Console.ReadLine();

			return newJob;
		}

2. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		CreateJob(client);

3. Adicione este método à classe Programa, que lista os trabalhos na conta. Isto ajuda a verificar se o trabalho foi criado:

		static void ListJobs (BatchClient client)
		{
			IPagedEnumerable<CloudJob> jobs = client.JobOperations.ListJobs();
			foreach (CloudJob job in jobs)
			{
				Console.WriteLine("Job id: " + job.Id);
				Console.WriteLine("   Job status: " + job.State);
			}
			Console.WriteLine("Press Enter to continue.");
			Console.ReadLine();
		}

4. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		ListJobs(client);

5. Salve o arquivo Program.cs.

## Etapa 4: Adicionar tarefas ao trabalho

Após o trabalho ser criado, podem ser adicionadas tarefas a ele. Cada tarefa é executada em um nó de computação e processa um arquivo de texto. Para este tutorial, você adiciona três tarefas ao trabalho.

1. Adicione este método à classe Programa, que adiciona as três tarefas ao trabalho:

		static void AddTasks(BatchClient client)
		{
			CloudJob job = client.JobOperations.GetJob("testjob1");
			ResourceFile programFile = new ResourceFile(
				"https://[account-name].blob.core.windows.net/testcon1/ProcessTaskData.exe",
				"ProcessTaskData.exe");
      	  ResourceFile assemblyFile = new ResourceFile(
				"https://[account-name].blob.core.windows.net/testcon1/Microsoft.WindowsAzure.Storage.dll",
				"Microsoft.WindowsAzure.Storage.dll");
			for (int i = 1; i < 4; ++i)
			{
				string blobName = "taskdata" + i;
				string taskName = "mytask" + i;
				ResourceFile taskData = new ResourceFile("https://[account-name].blob.core.windows.net/testcon1/" +
				  blobName, blobName);
				CloudTask task = new CloudTask(taskName, "ProcessTaskData.exe https://[account-name].blob.core.windows.net/testcon1/" +
				  blobName + " 3");
				List<ResourceFile> taskFiles = new List<ResourceFile>();
				taskFiles.Add(taskData);
				taskFiles.Add(programFile);
				taskFiles.Add(assemblyFile);
				task.ResourceFiles = taskFiles;
				job.AddTask(task);
				job.Commit();
				job.Refresh();
			}

			client.Utilities.CreateTaskStateMonitor().WaitAll(job.ListTasks(),
        TaskState.Completed, new TimeSpan(0, 30, 0));
			Console.WriteLine("The tasks completed successfully.");
			foreach (CloudTask task in job.ListTasks())
			{
				Console.WriteLine("Task " + task.Id + " says:\n" + task.GetNodeFile(Constants.StandardOutFileName).ReadAsString());
			}
			Console.WriteLine("Press Enter to continue.");
			Console.ReadLine();
		}


	**[nome-conta]** precisa ser substituído pelo nome da conta de armazenamento criada anteriormente. No exemplo anterior, atualize todas as quatro instâncias de **[nome-conta]**.


2. Adicione este código a Principal, que chama o método que você acabou de adicionar:

			AddTasks(client);

3. Adicione este método à classe Programa, que lista as tarefas que são associadas ao trabalho:

		static void ListTasks(BatchClient client)
		{
			IPagedEnumerable<CloudTask> tasks = client.JobOperations.ListTasks("testjob1");
			foreach (CloudTask task in tasks)
			{
				Console.WriteLine("Task id: " + task.Id);
				Console.WriteLine("   Task status: " + task.State);
			  Console.WriteLine("   Task start: " + task.ExecutionInformation.StartTime);
			}
			Console.ReadLine();
		}

4. Adicione este código a Principal para chamar o método que você adicionou:

		ListTasks(client);

5. Salve o arquivo Program.cs.

## Etapa 5: Excluir os recursos

Como você é cobrado pelos recursos no Azure, sempre será uma boa ideia excluir recursos caso eles não sejam mais necessários.

### Excluir as tarefas

1.	Adicione este método à classe Programa, que exclui as tarefas:

			static void DeleteTasks(BatchClient client)
			{
				CloudJob job = client.JobOperations.GetJob("testjob1");
				foreach (CloudTask task in job.ListTasks())
				{
					task.Delete();
				}
				Console.WriteLine("All tasks deleted.");
				Console.ReadLine();
			}

2. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		DeleteTasks(client);

3. Salve o arquivo Program.cs.

### Excluir o trabalho

1.	Adicione este método à classe Programa, que exclui o trabalho:

			static void DeleteJob(BatchClient client)
			{
				client.JobOperations.DeleteJob("testjob1");
				Console.WriteLine("Job was deleted.");
				Console.ReadLine();
			}

2. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		DeleteJob(client);

3. Salve o arquivo Program.cs.

### Excluir o pool

1. Adicione este método à classe Program, que exclui o pool:

		static void DeletePool (BatchClient client)
		{
			client.PoolOperations.DeletePool("testpool1");
			Console.WriteLine("Pool was deleted.");
			Console.ReadLine();
		}

2. Adicione este código a Principal, que chama o método que você acabou de adicionar:

		DeletePool(client);

3. Salve o arquivo Program.cs.

## Etapa 6: Executar o aplicativo

1. Inicie o projeto GettingStarted e você verá isto na janela do console após o contêiner ser criado:

		Created the container. Press Enter to continue.

2. Pressione Enter e os arquivos serão criados e carregados. Agora, você deve ver uma nova linha na janela:

		Uploaded the files. Press Enter to continue.

3. Pressione Enter e o pool será criado:

		Created the pool. Press Enter to continue.

4. Pressione Enter e você verá a listagem do novo pool:

		Pool name: testpool1
			Pool status: Active
		Press Enter to continue.

5. Pressione Enter e o trabalho será criado:

		Created the job. Press Enter to continue.

6. Pressione Enter e você verá a listagem do novo trabalho:

		Job id: testjob1
			Job status: Active
		Press Enter to continue.

7. Pressione Enter e as tarefas serão adicionadas ao trabalho. Quando as tarefas são adicionadas, elas são executadas automaticamente:

		The tasks completed successfully.
		Task mytask1 says:
		can 3
		you 3
		and 3

		Task mytask2 says:
		and 5
		application 3
		the 3

		Task mytask3 says:
		a 5
		and 5
		to 3

		Press Enter to continue.

7. Pressione Enter e você verá a lista de tarefas e seus status:

		Task id: mytask1
			Task status: Completed
			Task start: 7/17/2015 8:31:58 PM
		Task id: mytask2
			Task status: Completed
			Task start: 7/17/2015 8:31:57 PM
		Task id: mytask3
			Task status: Completed
			Task start: 7/17/2015 8:31:57 PM

8. Neste ponto, você pode ir para o portal do Azure para examinar os recursos que foram criados. Para excluir os recursos, pressione Enter até que o programa seja concluído.

## Próximas etapas

1. Agora que aprendeu os conceitos básicos da execução de tarefas, poderá saber mais sobre como dimensionar automaticamente nós de computação quando a demanda por seu aplicativo for alterada. Para fazer isso, consulte [Dimensionar automaticamente Nós de Computação em um Pool do Lote do Azure](batch-automatic-scaling.md)

2. Alguns aplicativos geram grandes quantidades de dados que podem ser difíceis de processar. Um modo de resolver isso é por meio da [consulta de lista eficiente](batch-efficient-list-queries.md).

<!---HONumber=AcomDC_1203_2015-->