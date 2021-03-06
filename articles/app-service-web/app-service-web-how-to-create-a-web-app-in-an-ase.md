<properties
	pageTitle="Criar um aplicativo Web em um Ambiente de Serviço de Aplicativo"
	description="Saiba como criar aplicativos Web e planos de serviço de aplicativo em um Ambiente do Serviço de Aplicativo."
	services="app-service"
	documentationCenter=""
	authors="ccompy"
	manager="stefsch"
	editor=""/>

<tags
	ms.service="app-service"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article" 
	ms.date="10/26/2015"
	ms.author="ccompy"/>

# Criar um aplicativo Web em um Ambiente de Serviço de Aplicativo

## Visão geral

Este tutorial mostra como criar aplicativos Web e planos do Serviço de Aplicativo em um [Ambiente do Serviço de Aplicativo](app-service-app-service-environment-intro.md) (ASE).

> [AZURE.NOTE]Se você quiser saber como criar um aplicativo Web mas se não precisar fazê-lo em um Ambiente de Serviço de Aplicativo, consulte [Criar um aplicativo Web .NET](web-sites-dotnet-get-started.md) ou um dos tutoriais relacionados para outras linguagens e estruturas.

## Pré-requisitos

Este tutorial supõe que você tenha criado um Ambiente de Serviço de Aplicativo. Se você ainda não tiver feito isso, consulte [Criar um Ambiente de Serviço de Aplicativo](app-service-web-how-to-create-an-app-service-environment.md).

## Criar um aplicativo Web

1. No [Portal do Azure](https://portal.azure.com/), clique em **Novo > Web + Móvel > Aplicativo Web**. 

	![][1]

2. Selecione sua assinatura.

	Se você tiver várias assinaturas, lembre-se de que, para criar um aplicativo no Ambiente de Serviço de Aplicativo, precisará usar a mesma assinatura que usou ao criar o ambiente.

3. Selecione ou crie um grupo de recursos.

	Os *grupos de recursos* permitem gerenciar recursos do Azure relacionados como uma unidade e são úteis durante o estabelecimento de regras de *controle de acesso baseado em função* (RBAC) para seus aplicativos. Para saber mais, consulte [Gerenciando seus recursos do Azure][ResourceGroups].

4. Selecione ou crie um plano do Serviço de Aplicativo.

	Os *planos do Serviço de Aplicativo* são conjuntos gerenciados de aplicativos Web. Quando você seleciona os preços, o preço cobrado é aplicado a um plano do serviço de aplicativo em vez de aplicado aos aplicativos individuais. Para aumentar o número de instâncias de um aplicativo Web, você aumenta as instâncias do seu plano de Serviço de Aplicativo e isso afeta todos os aplicativos Web nesse plano. Alguns recursos, como slots de site ou integração de VNET, também têm restrições de quantidade dentro do plano. Para saber mais, consulte [Visão geral de planos do Serviço de Aplicativo do Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

	Você pode identificar os planos do Serviço de Aplicativo no ASE examinando o local indicado abaixo do nome do plano.

	![][5]

	Se você quiser usar um plano de serviço do Serviço de Aplicativo que já exista no seu Ambiente de Serviço de Aplicativo, selecione-o. Se você quiser criar um novo plano do Serviço de Aplicativo, consulte a seção seguinte deste tutorial, [Criar um plano do Serviço de Aplicativo em um Ambiente de Serviço de Aplicativo](#createplan).

5. Insira o nome do aplicativo Web e clique em **Criar**.

	O nome de seu aplicativo Web precisa ser exclusivo em todo o Serviço de Aplicativo do Azure. Isso significa que, se você quiser criar um aplicativo Web chamado "meuaplicativoweb", então atualmente não pode existir nenhum outro aplicativo Web chamado "meuaplicativoweb" no Serviço de Aplicativo do Azure.

	A URL de um aplicativo Web em um ASE é:

	[*nomedosite*].[*nome do seu ambiente de Serviço de Aplicativo*].p.azurewebsites.net

	em vez de

	[*nomedosite*]. azurewebsites.net

## <a name="createplan"></a>Criar um plano do Serviço de Aplicativo

Quando você cria um plano do Serviço de Aplicativo em um Ambiente do Serviço de Aplicativo, as opções do seu trabalhador serão diferentes caso não haja trabalhadores compartilhados em um ASE. Os trabalhadores que você precisa usar são aqueles que foram alocados para o ASE pelo administrador. Isso significa que, para criar um novo plano, você precisa ter mais trabalhadores alocados para o pool de trabalho ASE do que o número total de instâncias em todos os seus planos já nesse pool de trabalho. Se não tiver trabalhadores suficientes em seu pool de trabalho ASE para criar o plano, você precisará trabalhar com seu administrador do ASE para adicioná-los.

Outra diferença em relação aos planos do Serviço de Aplicativo hospedados por um Ambiente do Serviço de Aplicativo é a ausência de seleção de preços. Quando você tem um ambiente do serviço de aplicativo, você está pagando por recursos de computação usados pelo sistema e não tem encargos adicionais para os planos nesse ambiente. Normalmente, quando você cria um plano do Serviço de Aplicativo, você seleciona um plano de preços que determina sua cobrança. Um ambiente do serviço de aplicativo é essencialmente um local privado onde você pode criar conteúdo. Você paga pelo ambiente e não para hospedar seu conteúdo.

As instruções a seguir mostram como criar um plano do Serviço de Aplicativo enquanto você estiver criando um aplicativo Web, como explicado na seção anterior do tutorial.

1. Clique em **Criar Novo** na interface do usuário de seleção de planos e forneça um nome para seu plano como normalmente faria fora de um ASE.

2. Selecione o ASE que você deseja usar em sua seleção de local.

	Como um Ambiente do Serviço de Aplicativo é, essencialmente, um local de implantação particular, ele é mostrado no Local.

	![][2]

	Após a seleção de um ASE no seletor de local, a interface do usuário de criação de Serviço de Aplicativo é atualizada. O local agora mostra o nome do sistema ASE e a região em que ele está, e o seletor de plano de preços será substituído por um seletor de pool de trabalho.

	![][3]

### Selecionando o pool de trabalho

Normalmente no Serviço de Aplicativo do Azure e fora de um Ambiente do Serviço de Aplicativo, há três tamanhos de computação disponíveis com a seleção de um plano de preços dedicado. De maneira semelhante, para um ASE, você pode definir até três pools de trabalho e especificar o tamanho de computação que é usado para esse pool de trabalho. O que isso significa para locatários do ASE é que, em vez de selecionar um plano de preços com tamanho de computação para o plano do Serviço de Aplicativo, selecione o que chamamos de um *pool de trabalho*.

A interface do usuário de seleção de pool de trabalho mostra o tamanho de computação usado para esse pool de trabalho abaixo do nome. A quantidade disponível se refere a quantas instâncias de computação estão disponíveis para uso no pool. O pool pode, na realidade, ter mais instâncias que esse número, mas esse valor se refere simplesmente a quantas não estão em uso. Se você precisar ajustar o seu ambiente de serviço de aplicativo para adicionar mais recursos de computação, consulte [Configurando seu ambiente de serviço de aplicativo](app-service-web-configure-an-app-service-environment.md).

![][4]

Neste exemplo, você verá apenas dois pools de trabalho disponíveis. Isso ocorre porque o administrador do ASE alocou hosts apenas nesses dois pools de trabalho. O terceiro aparecerá quando houver VMs alocadas para ele.

## Após a criação do aplicativo Web

Há algumas considerações sobre a execução de aplicativos Web e o gerenciamento de planos do Serviço de Aplicativo em um ASE que precisam ser levadas em conta.

Como observado anteriormente, o proprietário do ASE é responsável pelo tamanho do sistema e, como resultado, responsável também por garantir que haja capacidade suficiente para hospedar os planos do Serviço de Aplicativo desejados. Se não houver nenhum processador disponível, você não poderá criar o plano de Serviço de Aplicativo. Isso também é verdadeiro ao dimensionar seu aplicativo Web. Se você precisar de mais instâncias, será necessário que o administrador de seu ambiente do serviço de aplicativo adicione mais processadores.

Depois de criar seu aplicativo Web e o plano do Serviço de Aplicativo, será uma boa ideia escalá-lo verticalmente. Em um ASE, é sempre necessário ter pelo menos 2 instâncias do seu plano de Serviço de Aplicativo para proporcionar tolerância a falhas para seus aplicativos. O dimensionamento de um plano do Serviço de Aplicativo em um ASE é igual ao normal por meio da interface do usuário do plano de Serviço de Aplicativo. Para obter mais informações sobre o dimensionamento, [Como dimensionar um aplicativo Web em um Ambiente de Serviço de Aplicativo](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/

<!---HONumber=AcomDC_1203_2015-->