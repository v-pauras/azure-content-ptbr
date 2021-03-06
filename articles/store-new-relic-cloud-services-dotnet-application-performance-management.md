<properties 
	pageTitle="Usando o New Relic com o Azure | Microsoft Azure" 
	description="Saiba como usar o serviço New Relic para gerenciar e monitorar seu aplicativo do Azure." 
	services="" 
	documentationCenter=".net" 
	authors="nickfloyd" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="cloud-services" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="03/16/2015" 
	ms.author="nickfloyd@newrelic.com"/>



# Gerenciamento de desempenho do aplicativo New Relic no Azure

Este guia descreve como adicionar o monitoramento de desempenho de classe mundial do New Relic aos seus aplicativos hospedados no Microsoft Azure. Abordaremos o processo simples e rápido para adicionar New Relic ao seu aplicativo e apresentar alguns dos recursos do New Relic. Para obter mais informações sobre como usar o novo Relíquia, consulte [usando o novo Relíquia](#using-new-relic)

## O que há de novo Relíquia?

O New Relic é uma ferramenta focada no desenvolvedor que monitora seus aplicativos de produção e fornece informações detalhadas sobre seu desempenho e confiabilidade. Ele foi projetado para economizar tempo ao identificar e diagnosticar problemas de desempenho, e ele coloca as informações necessárias para solucionar esses problemas em suas mãos.

O New Relic controla o tempo de carregamento e taxa de transferência da sua transação da web, tanto do servidor quanto dos navegadores dos seus usuários. Ele mostra quanto tempo você gasta no banco de dados, analisa consultas lentas e solicitações da web, fornece monitoramento de tempo de atividade e alertas, faixas exceções de aplicativos e muito mais.

## Nova Relíquia especial de preços por meio do armazenamento do Azure


O New Relic Standard é oferecido gratuitamente aos usuários do Azure, o New Relic Pro é oferecido com base no tamanho da instância para os Serviços de Nuvem do Azure

Para informações sobre preços, consulte o [página nova Relíquia no armazenamento do Azure](http://azure.microsoft.com/marketplace/partners/newrelic/newrelic)

> [AZURE.NOTE]Preços somente é listada para até 10 instâncias de computação. Para um número maior que 10, entre em contato com o New Relic (sales@newrelic.com) para obter o preço por volume.

Azure clientes recebem uma assinatura de avaliação de 2 semanas do nova Relíquia Pro ao implantar o agente Relíquia de novo.

## Inscreva-se no novo Relíquia usando o armazenamento do Azure

Relíquia nova integra-se perfeitamente com funções de funções do Azure da Web e de trabalho.

Para inscrever-se para nova Relíquia diretamente do armazenamento do Azure, siga estas três etapas simples.

### Etapa 1. Inscrever-se por meio do armazenamento do Azure

1. Faça logon no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com).
2. No painel inferior do portal de gerenciamento, clique em **Novo**.
3. Clique em **Repositório**.
4. Na caixa de diálogo **Escolher um complemento**, selecione **New Relic** e clique em **Próximo**.
5. Na caixa de diálogo **Personalizar complemento**, selecione o plano New Relic que você deseja.
6. Digite um código de promoção, se aplicável.
7. Digite um nome para como o serviço New Relic aparecerá nas configurações do Azure ou use o valor padrão **NewRelic**. O nome deve ser exclusivo na sua lista de itens inscritos da Azure Store.
8. Escolha um valor para a região; por exemplo, **oeste dos Estados Unidos**.
9. Clique em **Próximo**.
10. Na caixa de diálogo **Revisar Compra**, revise o plano e as informações sobre preços, bem como os termos legais. Se você concordar com os termos, clique em **Comprar**
11. Depois que você clicar em **Comprar**, a sua conta New Relic iniciará o processo de criação. Você pode monitorar o status no portal de gerenciamento do Azure.
12. Para recuperar a chave de licença Relíquia de novo, clique em **valores de saída**. 
13. Copie a chave de licença é exibida. Você precisará inseri-la quando você instala o pacote novo Relíquia Nuget.

### Etapa 2. Instalar o pacote NuGet.

1. Abrir sua solução do Visual Studio ou criar uma nova selecionando **Arquivo > Novo > Projeto**.

	![Visual Studio](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget01.png)

2. Se você não tiver um projeto de serviço de nuvem do Azure em sua solução, adicione um clicando com o botão direito do mouse em seu aplicativo no Gerenciador de Soluções e selecionando **Adicionar projeto de serviço de nuvem do Azure**.

	![Criar serviço de nuvem](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget02.png)

3. Abra o console do Gerenciador de Pacotes selecionando **Ferramentas > Gerenciador de Pacotes da Biblioteca > Console do Gerenciador de Pacotes**. Defina seu projeto como o projeto padrão na parte superior da janela do Console do Gerenciador de Pacotes.

	![Console do Gerenciador de Pacotes](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget04.png)

4. No prompt de comando do Gerenciador de Pacotes, digite `Install-Package
   NewRelicWindowsAzure` e pressione **Enter**.

	![instalar o Gerenciador de pacotes](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget06.png)

5. No prompt de chave de licença, insira a chave de licença que você recebeu da Azure Store.

	![Insira a chave de licença](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget07.png)

6. (Opcional) No prompt de nome do aplicativo, digite o nome de seu aplicativo da maneira como ele será exibido no painel do New Relic. Ou, use o nome da solução como o padrão.

	![Digite o nome do aplicativo](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget08.png)

7. A partir do Gerenciador de Soluções, clique com o botão direito do mouse no Projeto de Serviço de Nuvem e selecione **Publicar**.

	![o projeto de nuvem pública](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget09.png)


**Observação:** se esta for a primeira vez que você está implantando esse aplicativo no Azure, será solicitado que você insira suas credenciais do Azure. Para obter mais informações, veja [Implantando um aplicativo Web ASP.NET em um site do Azure](app-service-web\web-sites-dotnet-get-started.md)

![configurações de publicação](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget10.png)

### Etapa 3. Conferir o desempenho do seu aplicativo no New Relic.

Para exibir o painel novo Relíquia:

1. No portal do Azure, clique o **gerenciar** botão.
2. Entrar com seu e-mail do Relíquia nova conta e senha.
3. Na barra de menu Novo Relíquia, selecione **aplicativos > (nome do aplicativo)**.

	O **Monitoring > Visão geral** painel aparece automaticamente.

	![Painel de monitoramento do New Relic](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelic_app.png)

	Depois de selecionar um aplicativo a partir da lista no seu **aplicativos** menu, o **visão geral** painel mostra informações atuais de servidor e o navegador do aplicativo.

### <a id="using-new-relic"></a>Usando o New Relic

Depois de selecionar um aplicativo a partir da lista no menu Aplicativos, o painel Visão Geral mostra informações atuais de servidor de aplicativos e do navegador. Para alternar entre dois modos de exibição, clique o **o servidor de aplicativo** ou **navegador** botão.

Além da [Relíquia novo padrão de interface de usuário](https://newrelic.com/docs/site/the-new-relic-ui#functions") e [painel drill-down](https://newrelic.com/docs/site/the-new-relic-ui#drilldown) funções, o painel de visão geral de aplicativos tem funções adicionais.

| Se você quiser... | Faça isso... |
| ----------------- | ---------- |
| Mostre informações do painel para o servidor ou navegador do aplicativo selecionado. | Clique o **Aplicativo servidor** ou **navegador** botão. |
| Exiba os níveis de limite da pontuação [Apdex](https://newrelic.com/docs/site/apdex) do seu aplicativo | Aponte para o ícone **?** da pontuação Apdex. |
| Exiba detalhes do Apdex em todo o mundo. | Na exibição **Navegador** da Visão Geral, aponte em qualquer lugar no mapa de Apdex Global. **Dica:** para ir diretamente para o painel [Geografia](https://docs.newrelic.com/docs/new-relic-browser/geography-dashboard") do aplicativo selecionado, clique no título **Apdex Global** ou clique em qualquer lugar no mapa do Apdex Global. |
| Exiba o painel [Transações da Web](https://newrelic.com/docs/applications-dashboards/web-transactions). | Clique na tabela de transações na Web no painel Visão geral de aplicativos. Ou, para exibir detalhes sobre uma transação da Web específica (incluindo [Principais Transações](https://newrelic.com/docs/site/key-transactions")), clique em seu nome. |
| Exiba o painel [Erros](https://newrelic.com/docs/site/errors). | Clique no título do gráfico de taxa de Erro no painel Visão geral dos aplicativos. **Dica:** Você também pode exibir o painel de Erros em **Aplicativos** > (seu aplicativo) > Eventos > Erros. |


Além disso, se desejar exibir os detalhes do servidor do aplicativo, faça o seguinte:

- Alternar entre um modo de exibição de tabela dos hosts ou detalhes de métrica de análise de cada host.
- Clique no nome de um servidor individual.
- Aponte para pontuação de Apdex do servidor individual da lista.
- Clique em um servidor individual da CPU ou memória.

Abaixo está um exemplo do painel de visão geral de aplicativos quando você selecionar o modo de exibição do navegador.

![Console do Gerenciador de Pacotes](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelic_app_browser.png)

## Próximas etapas

Confira estes recursos adicionais para obter mais informações:

 * [A instalação do agente do .NET no Azure](https://newrelic.com/docs/dotnet/installing-the-net-agent-on-azure): os procedimentos de instalação do agente do .NET do New Relic 
 * [Interface do usuário do New Relic](https://newrelic.com/docs/site/the-new-relic-ui): Visão geral da IU do New Relic, definindo direitos de usuário e perfis usando as funções padrão e os detalhes de busca detalhada do painel
 * [Visão geral sobre aplicativos](https://newrelic.com/docs/site/applications-overview): Recursos e funções ao usar o painel de visão geral de aplicativos do New Relic
 * [Apdex](https://newrelic.com/docs/site/apdex): Visão geral de como o Apdex mede a satisfação dos usuários finais com seu aplicativo
 * [Monitoramento real de usuários](https://newrelic.com/docs/features/real-user-monitoring): Visão geral de como o RUM detalha o tempo que leva para os navegadores de seus usuários carregarem as suas páginas da Web, de onde eles vêm e quais navegadores usam
 * [Encontrar Ajuda](https://newrelic.com/docs/site/finding-help): recursos disponíveis por meio do Centro de ajuda online do New Relic

<!---HONumber=Nov15_HO1-->