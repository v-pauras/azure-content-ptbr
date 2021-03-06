<properties
	pageTitle="Usando dados offline nos Serviços Móveis (Xamarin iOS) | Microsoft Azure"
	description="Saiba como usar serviços móveis do Azure para cache e sincronização de dados offline no aplicativo iOS Xamarin"
	documentationCenter="xamarin"
	authors="lindydonna"
	editor="wesmc"
	manager="dwrede"
	services="mobile-services"/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="11/02/2015"
	ms.author="donnam"/>

# Usando sincronização de dados offline em Serviços Móveis

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

Este tópico apresenta os recursos de sincronização offline de Serviços Móveis do Azure no aplicativo de início rápido de lista de tarefas pendentes. A sincronização offline permite que você crie facilmente aplicativos que são utilizáveis mesmo quando o usuário final não tem acesso à rede.

A sincronização offline possui diversos usos possíveis:

* Melhora a capacidade de resposta do aplicativo armazenando em cache os dados do servidor localmente no dispositivo
* Torna os aplicativos resilientes em relação à conectividade da rede intermitente
* Permite que usuários finais criem e modifiquem dados mesmo quando não há acesso à rede, com suporte para cenários com pouca ou nenhuma conectividade
* Sincroniza dados entre vários dispositivos e detecta conflitos quando o mesmo registro é modificado por dois dispositivos

>[AZURE.NOTE]Para concluir este tutorial, será necessária uma conta do Azure. Se você não tem uma conta, você pode se inscrever para uma avaliação do Azure e obter até 10 serviços móveis gratuitos que você pode continuar usando mesmo depois do fim de sua avaliação. Para obter detalhes, consulte <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=AE564AB28" target="_blank">Avaliação Gratuita do Azure</a>.
>
> Se esta for sua primeira experiência com os Serviços Móveis, você deverá concluir a [Introdução aos Serviços Móveis].

Este tutorial apresenta e explica as seguintes etapas básicas:

1. [Examinar o código de sincronização de Serviços Móveis]
2. [Atualizar o comportamento de sincronização do aplicativo]
3. [Atualizar o aplicativo para reconectar seu serviço móvel]

Este tutorial exige o seguinte:

* Visual Studio com a [extensão Xamarin] **ou** [Xamarin Studio] no OS X
* XCode 4.5 e iOS 6.0 (ou versões posteriores)
* Conclusão do tutorial [Introdução aos Serviços Móveis]

## <a name="review-offline"></a>Examinar o código de sincronização dos Serviços Móveis

A Sincronização offline dos Serviços móveis do Azure permite aos usuários finais interagir com um banco de dados local quando a rede não está acessível. Para usar esses recursos em seu aplicativo, inicialize `MobileServiceClient.SyncContext` para um armazenamento local. Em seguida, faça referência à sua tabela por meio da interface da `IMobileServiceSyncTable`. Esta seção orienta você quanto ao código relacionado à sincronização offline em `QSTodoService.cs`.

1. No Visual Studio, abra o projeto que você concluiu no tutorial [Introdução aos Serviços Móveis]. Abra o arquivo `QSTodoService.cs`.

2. Observe que o tipo do membro `todoTable` é `IMobileServiceSyncTable`. A sincronização offline usa essa interface de tabela de sincronização em vez de `IMobileServiceTable`. Quando uma tabela de sincronização é usada, todas as operações vão para o armazenamento local e só são sincronizadas com o serviço remoto com operações de push e pull explícitas.

    Para obter uma referência a uma tabela de sincronização, utiliza-se o método `GetSyncTable()`. Para remover a funcionalidade de sincronização offline, use `GetTable()`.

3. Antes de qualquer operação de tabela poder ser executada, o armazenamento local deve ser inicializado. Isso é feito com o método `InitializeStoreAsync`:

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }

    Isso cria um repositório local usando a classe `MobileServiceSQLiteStore`, fornecida no SDK dos Serviços Móveis. Você também pode fornecer uma implementação de repositório local diferente ao implementar `IMobileServiceLocalStore`.

    O método `DefineTable` cria uma tabela no repositório local que corresponde aos campos no tipo fornecido, `ToDoItem`, neste caso. O tipo não precisa incluir todas as colunas que estão no banco de dados remoto - é possível armazenar apenas um subconjunto de colunas.

    Essa sobrecarga de `InitializeAsync` usa o manipulador de conflitos padrão, que falha sempre que há um conflito. Para fornecer um manipulador de conflitos personalizado, consulte o tutorial [Tratando conflitos com o suporte offline para os Serviços Móveis].

4. O método `SyncAsync` aciona a operação de sincronização real:

        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

    Primeiro, há uma chamada para `IMobileServiceSyncContext.PushAsync()`. Esse método é um membro de `IMobileServicesSyncContext` em vez da tabela de sincronização porque enviará as alterações a todas as tabelas. Somente os registros que foram modificados, de alguma forma, localmente (por meio de operações CUD) serão enviados ao servidor.

    Em seguida, o método chama `IMobileServiceSyncTable.PullAsync()` para extrair dados de uma tabela no servidor para o aplicativo. Observe que se houver quaisquer alterações pendentes no contexto de sincronização, um pull sempre emite um push primeiro. Isso serve para garantir que todas as tabelas no repositório local, juntamente com os relacionamentos, permaneçam consistentes. Neste caso, chamamos push explicitamente.

    Neste exemplo, recuperamos todos os registros na tabela remota `TodoItem`, mas também é possível filtrar os registros passando uma consulta. O primeiro parâmetro para `PullAsync()` é uma ID de consulta que é usada para sincronização incremental, que usa o carimbo de data/hora `UpdatedAt` para obter apenas os registros modificados desde a última sincronização. A ID da consulta deve ser uma cadeia de caracteres descritiva que é exclusiva para cada consulta lógica em seu aplicativo. Se desejar sair da sincronização incremental, passe `null` como a ID da consulta. Isso recuperará todos os registros de cada operação de pull, o que é potencialmente ineficiente.

    >[AZURE.NOTE]Para remover registros de armazenamento local do dispositivo quando eles tiverem sido excluídos do banco de dados do serviço móvel, você deve habilitar a [Exclusão Reversível]. Caso contrário, seu aplicativo deverá chamar `IMobileServiceSyncTable.PurgeAsync()` periodicamente para limpar o armazenamento local.

    Observe que a `MobileServicePushFailedException` pode ocorrer tanto por uma operação de push quanto de pull. O próximo tutorial, [Tratando conflitos com suporte offline para Serviços Móveis], mostra como tratar essas exceções relacionadas à sincronização.

5. Na classe `QSTodoService`, o método `SyncAsync()` é chamado após as operações que modificam dados, `InsertTodoItemAsync()` e `CompleteItemAsync`. Ele também é chamado por meio de `RefreshDataAsync()`, de modo que o usuário obtém os dados mais recentes sempre que executar o gesto de atualização. O aplicativo também realiza uma sincronização na inicialização, já que `QSTodoListViewController.ViewDidLoad()` chama `RefreshDataAsync()`.

    Como `SyncAsync()` é chamado sempre que dados são modificados, esse aplicativo presume que o usuário está online sempre que este usuário está editando dados. Na próxima seção, atualizaremos o aplicativo para que os usuários possam editar mesmo quando estiverem offline.

## <a name="update-sync"></a>Atualizar o comportamento de sincronização do aplicativo

Nesta seção, você modificará o aplicativo para ele não sincronizar na sua inicialização ou nas operações insert e update, mas somente quando o gesto de atualização for realizado. Em seguida, você interromperá a conexão do aplicativo com o serviço móvel para simular um cenário offline. Quando você adicionar itens de dados, eles serão mantidos no armazenamento local, mas não serão imediatamente sincronizados com o serviço móvel.

1. Abra `QSTodoService.cs`. Comente as chamadas a `SyncAsync()` nos métodos a seguir:

    - `InsertTodoItemAsync`
    - `CompleteItemAsync`
    - `RefreshAsync`

    Agora, `RefreshAsync()` só carregará dados do repositório local, mas não se conectará ao back-end do aplicativo.

2. Em `QSTodoService.cs`, comente as definições dos membros `applicationURL` e `applicationKey`. Adicione as linhas a seguir, que fazem referência a uma URL de serviço móvel inválida:

        const string applicationURL = @"https://your-mobile-service.azure-mobile.xxx/";
        const string applicationKey = @"AppKey";

3. Para garantir que os dados sejam sincronizados quando o gesto de atualização for executado, edite o método `QSTodoListViewController.RefreshAsync()`. Adicione uma chamada a `SyncAsync()` antes da chamada a `RefreshDataAsync()`:

        private async Task RefreshAsync ()
        {
            RefreshControl.BeginRefreshing ();

            await todoService.SyncAsync();
            await todoService.RefreshDataAsync (); // add this line

            RefreshControl.EndRefreshing ();

            TableView.ReloadData ();
        }

4. Compile e execute o aplicativo. Adicione alguns novos itens ToDo. Esses novos itens existem apenas no armazenamento local, até que possam ser enviados por push para o serviço móvel. O aplicativo cliente se comporta como se estivesse conectado ao serviço móvel, oferecendo suporte a todas as operações CRUD (criação, leitura, atualização, exclusão).

5. Feche o aplicativo e reinicie-o para verificar se os novos itens que você criou persistem no armazenamento local.

## <a name="update-online-app"></a>Atualizar o aplicativo para reconectar seu serviço móvel

Nesta seção você reconectará o aplicativo ao serviço móvel. Isso simula o aplicativo mudando de um estado offline para um estado online com o serviço móvel. Quando você executar o gesto de atualização, dados serão sincronizados com o serviço móvel.

1. Abra `QSTodoService.cs`. Remova a URL de serviço móvel inválida e adicione novamente a chave correta de aplicativo e URL.

2. Recompile e execute o aplicativo. Observe que os dados têm a mesma aparência do cenário offline mesmo que agora o aplicativo esteja conectado ao serviço móvel. Isso ocorre porque esse aplicativo sempre usa o `IMobileServiceSyncTable`, que está voltado ao repositório local.

3. Faça logon no [Portal clássico do Azure] e examine o banco de dados para o seu serviço móvel. Se o serviço usar o back-end do JavaScript, você poderá procurar os dados na guia **Dados** do serviço móvel.

    Se você estiver usando o back-end do .NET para o serviço móvel, no Visual Studio, acesse **Gerenciador de servidores** > **Azure** > **Bancos de dados SQL**. Clique com o botão direito do mouse em seu Banco de Dados e selecione **Abrir no Gerenciador de Objetos do SQL Server**.

    Observe que os dados *não* foram sincronizados entre o banco de dados e o repositório local.

4. No aplicativo, faça o gesto de atualização puxando a lista de itens para baixo. Isso faz com que o aplicativo chame `RefreshDataAsync()`, que, por sua vez, chama `SyncAsync()`. Isso executará as operações de push e pull, primeiro enviando os itens do armazenamento local para o serviço móvel e, em seguida, recuperando novos dados do serviço.

5. Verifique o banco de dados para o serviço móvel a fim de confirmar que as alterações foram sincronizadas.

##Resumo

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## Próximas etapas

* [Tratando conflitos com o suporte offline para Serviços Móveis]

* [Como usar a biblioteca de cliente Componente Xamarin para os Serviços Móveis do Azure]

<!-- Anchors. -->
[Examinar o código de sincronização de Serviços Móveis]: #review-offline
[Atualizar o comportamento de sincronização do aplicativo]: #update-sync
[Atualizar o aplicativo para reconectar seu serviço móvel]: #update-online-app

<!-- Images -->

<!-- URLs. -->
[Tratando conflitos com o suporte offline para Serviços Móveis]: ../mobile-services-xamarin-ios-handling-conflicts-offline-data.md
[Tratando conflitos com o suporte offline para os Serviços Móveis]: ../mobile-services-xamarin-ios-handling-conflicts-offline-data.md
[Tratando conflitos com suporte offline para Serviços Móveis]: ../mobile-services-xamarin-ios-handling-conflicts-offline-data.md
[Introdução aos Serviços Móveis]: mobile-services-ios-get-started.md
[Como usar a biblioteca de cliente Componente Xamarin para os Serviços Móveis do Azure]: partner-xamarin-mobile-services-how-to-use-client-library.md
[Exclusão Reversível]: mobile-services-using-soft-delete.md

[Xamarin Studio]: http://xamarin.com/download
[extensão Xamarin]: http://xamarin.com/visual-studio
[Portal clássico do Azure]: https://manage.windowsazure.com

<!---HONumber=AcomDC_1203_2015-->