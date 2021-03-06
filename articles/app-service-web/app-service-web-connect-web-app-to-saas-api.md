<properties 
	pageTitle="Conectar um aplicativo Web a um aplicativo de API no Serviço de Aplicativo do Azure" 
	description="Este tutorial mostra como consumir um aplicativo de API de um aplicativo Web ASP.NET hospedado no Serviço de Aplicativo do Azure." 
	services="app-service\web" 
	documentationCenter=".net" 
	authors="syntaxc4" 
	manager="yochayk" 
	editor="jimbe"/>

<tags
	ms.service="app-service-web"
	ms.devlang="dotnet"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="na" 
	ms.date="12/24/2015"
	ms.author="cfowler"/>

# Conectar um aplicativo Web a um aplicativo de API no Serviço de Aplicativo do Azure

Este tutorial mostra como consumir um aplicativo de API de um aplicativo Web ASP.NET hospedado em [Serviço de Aplicativo](https://azure.microsoft.com/services/app-service/).

## Pré-requisitos

Este tutorial se baseia na série de tutoriais de Aplicativos de API:

1. [Criar um aplicativo de API do Azure](../app-service-dotnet-create-api-app)
3. [Implantar um aplicativo de API do Azure](../app-service-dotnet-deploy-api-app)
4. [Depurar um aplicativo de API do Azure](../app-service-dotnet-remotely-debug-api-app)


## Criar um Aplicativo MVC ASP.NET no Visual Studio

1. Abra o Visual Studio. Use a caixa de diálogo **Novo Projeto** para adicionar um novo **Aplicativo Web ASP.NET**. Clique em **OK**.

	![Arquivo > Novo > Web > Aplicativo Web ASP.NET](./media/app-service-web-connect-web-app-to-saas-api/1-Create-New-MVC-App-For-Consumption.png)

1. Selecione o modelo **MVC**. Clique em **Alterar Autenticação**, selecione **Sem Autenticação** e, em seguida, clique em **OK** duas vezes.

	![Novo aplicativo ASP.NET](./media/app-service-web-connect-web-app-to-saas-api/2-Change-Auth-To-No-Auth.png)

1. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto de aplicativo Web recém-criado e selecione **Adicionar** > **Cliente API REST...**.

	![Adicione referência de aplicativo de API do Azure...](./media/app-service-web-connect-web-app-to-saas-api/3-Add-Azure-API-App-SDK.png)

1. Em **Adicionar cliente API REST**, selecione o download do aplicativo de API do Microsoft Azure e clique em Procurar. Selecione o aplicativo de API ao qual você gostaria de se conectar.

	![Selecione o aplicativo de API existente](./media/app-service-web-connect-web-app-to-saas-api/4-Add-Azure-API-App-SDK-Dialog.png)

	>[AZURE.NOTE]O código do cliente para se conectar ao aplicativo de API será gerado automaticamente de um ponto de extremidade de API Swagger.

1. Para aproveitar o código de API gerado, abra o arquivo HomeController.cs e substitua a ação `Contact` pelo seguinte:

	    public async Task<ActionResult> Contact()
        {
            ViewBag.Message = "Your contact page.";

            var contacts = new ContactsList12242015();
            var contactList = await contacts.Contacts.GetAsync();
            
            return View(contactList);
        }

	![Atualizações de código HomeController.cs](./media/app-service-web-connect-web-app-to-saas-api/5-Write-Code-Which-Leverages-Swagger-Generated-Code.png)

1. Atualize a exibição `Contact` para refletir a lista dinâmica de contatos com o seguinte código:
	<pre>// Add to the very top of the view file
	@model IList&lt;MyContactsList.Web.Models.Contact&gt;
	
	// Replace the default email addresses with the following
    &lt;h3&gt;Public Contacts&lt;/h3&gt;
    &lt;ul&gt;
        @foreach (var contact in Model)
        {
            &lt;li&gt;&lt;a href=&quot;mailto:@contact.EmailAddress&quot;&gt;@contact.Name &amp;lt;@contact.EmailAddress&amp;gt;&lt;/a&gt;&lt;/li&gt;
        }
    &lt;/ul&gt; 
	</pre>
	![Atualizações de código Contact.cshtml](./media/app-service-web-connect-web-app-to-saas-api/6-Update-View-To-Reflect-Changes.png)

## Implantar o aplicativo Web para Aplicativos Web no Serviço de Aplicativo

Siga as instruções disponíveis em [Como Implantar um Aplicativo Web do Azure](web-sites-deploy.md).

>[AZURE.NOTE]Se você deseja começar a usar o Serviço de Aplicativo do Azure antes de se inscrever em uma conta do Azure, vá até [Experimentar o Serviço de Aplicativo](http://go.microsoft.com/fwlink/?LinkId=523751), em que você pode criar imediatamente um aplicativo Web inicial de curta duração no Serviço de Aplicativo. Nenhum cartão de crédito é exigido, sem compromissos.

## O que mudou
* Para obter um guia sobre a alteração de Sites para o Serviço de Aplicativo, consulte: [Serviço de Aplicativo do Azure e seu impacto sobre os serviços do Azure existentes](http://go.microsoft.com/fwlink/?LinkId=529714)
 

<!---HONumber=AcomDC_0107_2016-->