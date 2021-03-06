<properties
   pageTitle="Visão geral da Service Fabric | Microsoft Azure"
   description="Uma visão geral do Service Fabric no qual os aplicativos são compostos por muitos microsserviços para fornecer dimensionamento e resiliência. O Service Fabric é uma plataforma de sistemas distribuídos usada para criar aplicativos escalonáveis, confiáveis e facilmente gerenciados para a nuvem"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/18/2015"
   ms.author="mfussell"/>

# Visão geral da Malha do Serviço
O Service Fabric do Azure é uma plataforma de sistemas distribuídos usada para criar aplicativos escalonáveis, confiáveis e facilmente gerenciados para a nuvem A Malha do Serviço resolve os desafios significativos do desenvolvimento e gerenciamento de aplicativos em nuvem. Ao usar o Service Fabric, desenvolvedores e administradores podem evitar a resolução de problemas complexos de infraestrutura e, em vez disso, se concentram na implementação de cargas de trabalho essenciais e exigentes sabendo que elas são escalonáveis, confiáveis e gerenciáveis. O Service Fabric representa a plataforma de middleware de última geração para criação e gerenciamento de serviços dimensionáveis de nuvem de Camada 1 e nível corporativo.

## Aplicativos compostos por microsserviços
A Malha do Serviço permite criar e gerenciar aplicativos escalonáveis e confiáveis compostos por microsserviços que são executados em densidade bastante alta em um pool compartilhado de computadores (geralmente conhecido como um cluster da Malha do Serviço). Ele fornece um tempo de execução sofisticado para criação de microsserviços distribuídos, escalonáveis, com e sem estado. Ele também fornece recursos abrangentes de gerenciamento de aplicativos para provisionamento, implantação, monitoramento, atualização/aplicação de patch e exclusão de aplicativos implantados.

O Service Fabric é a tecnologia de muitos serviços atuais da Microsoft, como Bancos de Dados SQL do Azure, Banco de Dados de Documentos do Azure, Cortana, Power BI, Microsoft Intune, Hubs de Eventos do Azure, muitos serviços de núcleo do Azure e Skype for Business, apenas para citar alguns.

A Malha do Serviço foi personalizada para a criação de serviços “nascidos na nuvem” que podem começar pequenos, conforme a necessidade, e se expandir em escala massiva com centenas ou milhares de computadores, criando clusters da Malha do Serviço entre conjuntos de disponibilidade em uma região ou entre regiões.

Os serviços em escala da Internet de hoje são criados usando microsserviços. Exemplos de microsserviços são gateways de protocolo, perfis de usuário, carrinhos de compra, processamento de inventário, filas e caches. O Service Fabric é uma plataforma de microsserviços que fornece a cada microsserviço um nome exclusivo que pode ser com ou sem estado.

A Malha do Serviço fornece recursos abrangentes de gerenciamento de ciclo de vida e tempo de execução para aplicativos compostos por esses microsserviços. Ela hospeda microsserviços em contêineres que são implantados e ativados no cluster da Malha do Serviço. Assim como um aumento gigantesco na densidade é possibilitado ao passar de VMs para contêineres, uma ordem de grandeza semelhante na densidade se torna possível ao passar de contêineres para microsserviços. Por exemplo, um único cluster de Banco de Dados SQL do Azure, que é criado no Service Fabric, engloba centenas de computadores que executam dezenas de milhares de contêineres que hospedam um total de centenas de milhares de bancos de dados. (Cada banco de dados é um microsserviço com estado do Service Fabric.) O mesmo se aplica aos Hubs de Eventos e a outros serviços mencionados acima. Por esse motivo o termo "hiperescala" pode ser usado para descrever os recursos do Service Fabric. Se os contêineres fornecem alta densidade, os microsserviços fornecem hiperescala.

![Plataforma Service Fabric][Image1]

## Microsserviços da Malha do Serviço com e sem estado

Os microsserviços sem estado (gateways de protocolo, proxies Web, etc.) não mantêm um estado mutável fora de qualquer solicitação e de sua resposta do serviço. As funções de trabalho dos Serviços de Nuvem do Azure são um exemplo de serviço sem estado. Os microsserviços com estado (contas de usuário, bancos de dados, dispositivos, carrinhos de compra, filas, etc.) mantêm um estado mutável e autoritativo além da solicitação e de sua resposta. Os aplicativos em escala da Internet de hoje consistem em uma combinação de microsserviços com e sem estado.

Por que os microsserviços com estado são importantes? Por que não usar serviços sem estado simplesmente para tudo? Por dois motivos:

1. Capacidade de criar serviços OLTP (transação online) tolerantes a falhas, de alta produtividade e baixa latência, como vitrines interativas, pesquisa, sistemas IoT (Internet das Coisas), sistemas comerciais, sistemas de processamento de cartão de crédito e de detecção de fraude, gerenciamento de registros pessoais, etc., mantendo código e dados encerrados no mesmo computador.

2. A simplificação de design de aplicativo, como microsserviços com estado, elimina a necessidade de filas e caches adicionais. Esses últimos têm sido tradicionalmente obrigatórios para atender aos requisitos de disponibilidade e latência de um aplicativo totalmente sem estado. Uma vez que os serviços com estado são, de forma natural, de alta disponibilidade e baixa latência, isso significa menos partes móveis para gerenciar no seu aplicativo como um todo.

Para obter mais informações sobre padrões e design de aplicativos usando o Service Fabric, confira [Cenários de aplicativos](service-fabric-application-scenarios.md).

## Gerenciamento do ciclo de vida de aplicativos
O Service Fabric oferece um excelente suporte ao ALM (gerenciamento do ciclo de vida do aplicativo) completo de aplicativos em nuvem, desde o desenvolvimento, passando por implantação, gerenciamento diário, manutenção até o possível encerramento.

Os recursos do ALM da Malha do Serviço permitem que os administradores de aplicativos/operadores de TI usem fluxos de trabalho simples com pouco envolvimento humano para provisionar, implantar e monitorar aplicativos, bem como aplicar patch neles. Esses fluxos de trabalho internos reduzem enormemente a carga em operações de TI para manter os aplicativos disponíveis continuamente.

A maioria dos aplicativos consiste em uma combinação de microsserviços com e sem estado e outros executáveis/tempos de execução que são implantados juntos. Tendo tipos fortes nos aplicativos e microsserviços empacotados, o Service Fabric permite a implantação de várias instâncias de aplicativo, e cada uma delas pode ser gerenciada e atualizada de maneira independente. Vale destacar que o Service Fabric é capaz de implantar *qualquer* executável ou tempo de execução e torná-los confiáveis. Por exemplo, ele pode ser usado para implantar ASP.NET 5, Node.js, scripts ou qualquer item que componha seu aplicativo.

Para obter mais informações sobre o gerenciamento do ciclo de vida do aplicativo, confira [Ciclo de vida do aplicativo](service-fabric-application-lifecycle.md)

## Principais recursos
Ao usar a Malha do Serviço, você pode:

- Desenvolver aplicativos massivamente escalonáveis, que são autorrecuperáveis.

- Desenvolver usando uma abordagem "datacenter no seu computador". O ambiente de desenvolvimento local usa o mesmo código que é executado nos datacenters do Azure.

- Desenvolver aplicativos compostos por microsserviços, executáveis e outras estruturas de aplicativo de sua escolha, como ASP.NET, Node.js, etc.

- Desenvolver serviços/microsserviços com e sem estado e torná-los altamente confiáveis.

- Simplificar o design do aplicativo usando serviços/microsserviços com estado no lugar de caches e filas.

- Implantar aplicativos em segundos.

- Implantar no Azure ou em nuvens locais executando o Windows Server sem mudanças no código. Escreva uma vez e implante em qualquer cluster da Malha do Serviço.

- Implantar aplicativos em densidade mais alta do que as máquinas virtuais, implantando centenas ou milhares de aplicativos por computador.

- Implantar diferentes versões do mesmo aplicativo lado a lado, cada um atualizável de modo independente.

- Gerenciar o ciclo de vida dos seus aplicativos com estado sem nenhum tempo de inatividade, incluindo atualizações imediatas ou não.

- Gerenciar aplicativos usando APIs .NET, PowerShell ou interfaces REST.

- Atualizar microsserviços e aplicar patch neles dentro dos aplicativos de modo independente.

- Monitorar e diagnosticar a integridade de seus aplicativos e definir políticas para realizar reparos automáticos.

- Escalar vertical ou horizontalmente o cluster do Service Fabric com facilidade, sabendo que os aplicativos são dimensionados de acordo com os recursos disponíveis.

- Observar o balanceador de recursos de autorrecuperação administrar a redistribuição de aplicativos entre os clusters do Service Fabric para se recuperar de falhas e otimizar a distribuição da carga com base nos recursos disponíveis.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Próximas etapas

* Para mais informações:
	* [Por que usar uma abordagem de microsserviço para construir aplicativos?](service-fabric-overview-microservices.md)
	* [Visão geral técnica](service-fabric-technical-overview.md)
* Configurando o [ambiente de desenvolvimento](service-fabric-get-started.md) do Service Fabric  
* Escolhendo uma [estrutura do modelo de programação](service-fabric-choose-framework.md) para o serviço


[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png

<!---HONumber=AcomDC_1223_2015-->