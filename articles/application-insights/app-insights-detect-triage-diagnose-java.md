<properties
	pageTitle="Detectar, realizar triagem e diagnosticar aplicativos Web J2EE"
	description="Analise as falhas, detecte e diagnostique problemas de desempenho em seus aplicativos Web Java"
	authors="alancameronwills"
	services="application-insights"
    documentationCenter=""
	manager="douge"/>

<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article" 
	ms.date="09/09/2015"
	ms.author="awills"/>

# Detectar, realizar triagem e diagnosticar aplicativos J2EE com o Application Insights

*O Application Insights está em modo de visualização.*


Após ter publicado seu aplicativo, o Application Insights ajuda a garantir que ele esteja sendo executado do modo correto e apresentando um bom desempenho. Se houver um problema, você deseja saber sobre isso em breve e, em seguida, você deseja saber o que fazer a respeito.

* *"Alguns dias atrás, implantamos um hotfix “secundário”. Não executamos uma aprovação de teste ampla, mas infelizmente algumas alterações inesperadas foram mescladas à carga, gerando incompatibilidade entre o front-end e back-end. Imediatamente as exceções de servidor aumentaram drasticamente, nosso alerta disparou e fomos informados sobre a situação. Alguns cliques no portal do Application Insights, temos informações suficientes de pilhas de chamadas de exceção para restringir o problema. Revertemos imediatamente e limitamos os danos. O Application Insights tornou essa parte do ciclo das Operações de Desenvolvimento muito fácil e prática."*

Podemos pensar nessa parte do ciclo das Operações de Desenvolvimento como um pipeline:

![Detectar-Realizar a triagem-Diagnosticar](./media/app-insights-detect-triage-diagnose-java/01-pipe1.png)


Depois de você diagnosticar o problema, você sabe onde concentrar seus esforços - seja depurando seu código, alocando mais memória ou criando uma dependência subsequentemente. Finalmente, você pode verificar se a correção funcionou:



![Reparar-Validar](./media/app-insights-detect-triage-diagnose-java/02-pipe2.png)


Vamos ver como o Application Insights funciona em cada estágio do pipeline.

O Application Insights funciona para aplicativos de dispositivos e aplicativos Web. Neste passo a passo, vamos nos concentrar em um aplicativo Web. Vamos acompanhar a equipe OBS no Fabrikam Bank, que é responsável pelo sistema bancário online. Eles [adicionaram o Application Insights aos seus projetos da Web](app-insights-java-get-started.md).


![Exemplo de site da Web bancário](./media/app-insights-detect-triage-diagnose-java/03-bank.png)



## Detectar baixa disponibilidade


Marcela Markova é especialista em testes da equipe de OBS e assume a liderança no monitoramento de desempenho online. Ela define vários [testes da Web][availability]\:

* Um teste de URL única para a página de aterrissagem principal para o aplicativo, http://fabrikambank.com/onlinebanking/. Ela define os critérios de código HTTP 200 e o texto “Bem-vindo!”. Se esse teste falhar, há algum problema sério com a rede ou os servidores, ou talvez um problema de implantação. (Ou alguém alterou a mensagem “bem-vindo!” na página sem informá-la.)


* Um teste de várias etapas mais aprofundado, que faz logon e obtém uma listagem atual das contas, verificando alguns detalhes principais em cada página. Esse teste verifica se o link para o banco de dados de contas está funcionando. Ela usa uma ID do cliente fictícia: algumas delas são mantidas para fins de teste.


Com esses testes configurados, Marcela está confiante de que a equipe saberá rapidamente sobre qualquer interrupção.


Falhas aparecem como pontos vermelhos no gráfico de teste da Web:

![Exibição de testes da Web que foram executados durante o período anterior](./media/app-insights-detect-triage-diagnose-java/04-webtests.png)


Mas o mais importante, um alerta sobre qualquer falha será enviado por email à equipe de desenvolvimento. Desse modo, eles sabem a respeito antes de quase todos os clientes.


## Monitorar métricas de desempenho


Na mesma página de visão geral do Application Insights, há um gráfico que mostra as [principais métricas][perf].

![Métricas diversas](./media/app-insights-detect-triage-diagnose-java/05-perfMetrics.png)

O tempo de carregamento de página do navegador é derivado da telemetria enviada diretamente a partir de páginas da Web. Tempo de resposta do servidor, contagem de solicitação do servidor e contagem de solicitação com falha são todos medidos no servidor Web e enviadas ao Application Insights a partir daí.




A contagem de solicitações com falha indica casos em que os usuários já viram um erro - geralmente após uma exceção lançada no código. Talvez eles vejam uma mensagem dizendo "Desculpe, não foi possível atualizar os detalhes agora" ou, na hipótese mais vergonhosa possível, um despejo de pilha na tela do usuário, cortesia do servidor Web.


Marcela gosta de examinar esses gráficos de tempos em tempos. A ausência de solicitações com falha é animadora, apesar de que quando ela altera o intervalo do gráfico para cobrir a última semana, falhas ocasionais são exibidas. Esse é um nível aceitável em um servidor ocupado. Mas se houver um salto súbito em falhas ou em algumas das métricas, como tempo de resposta do servidor, Marcela quer saber imediatamente. Isso pode indicar um problema imprevisto causado por um lançamento de código, ou uma falha em uma dependência como um banco de dados, ou ainda uma má reação a uma carga alta de solicitações.


Marcela não fica apenas sentada esperando por alertas. Logo após cada reimplantação, ela examina [tempos de resposta][perf] -tanto o quadro geral e a tabela de solicitações mais lentas quanto as contagens de exceção.



![Gráfico de tempo de resposta e uma grade dos tempos de resposta do servidor.](./media/app-insights-detect-triage-diagnose-java/09-dependencies.png)

Ela pode avaliar o efeito no desempenho de cada implantação, geralmente comparando cada semana com a última. Se houver uma deterioração repentina do quadro, ela levanta a questão com os desenvolvedores relevantes.

Também há gráficos de dependências, contadores de desempenho, exceções, dados de uso e muitas outras métricas e eventos.

#### Alertas

Portanto, ela define dois [alertas][metrics]\: um para tempos de resposta maiores que um limite típico e outro para uma taxa de solicitações com falha maior do que aquela na tela de fundo atual.


Junto com o alerta de disponibilidade, eles dão a certeza de que, assim que qualquer coisa incomum acontecer, ela saberá a respeito.


Também é possível definir alertas em uma grande variedade de outras métricas. Por exemplo, você pode receber emails se a contagem de exceções aumentar excessivamente ou se a memória disponível tornar-se baixa demais, ou ainda se houver um pico em solicitações de cliente.



![Folha Adicionar alerta](./media/app-insights-detect-triage-diagnose-java/07-alerts.png)



## Detectando exceções


As exceções não capturadas são relatadas ao Application Insights automaticamente.


Além disso, você pode enviar relatórios de exceções que você capturar inserindo chamadas para [trackException()](app-insights-api-custom-events-metrics.md#track-exception) no manipulador:

``` Java

   TelemetryClient telemetry = new TelemetryClient();
   try {
    ...
   } catch (Exception ex) {
    // Set up some properties:
    Map<String, String> properties = new HashMap<String, String>();
    properties.put("Game", currentGame.getName());

    Map<String, Double> measurements = new HashMap<String, Double>();
    measurements.put("Users", currentGame.getUsers().getCount());

    // Send the exception telemetry:
    telemetry.trackException(ex, properties, measurements);
   }

```



A equipe do Fabrikam Bank evoluiu na prática de sempre enviar telemetria em uma exceção, a menos que haja uma óbvia recuperação.

Eles normalmente anexam as propriedades que podem ser úteis no diagnóstico.

Exceções e eventos aparecem na folha [Pesquisa de diagnóstico][diagnostic]. Você pode analisar para ver as propriedades adicionais e o rastreamento de pilha.

![Na Pesquisa de diagnóstico, use filtros para exibir tipos específicos de dados](./media/app-insights-detect-triage-diagnose-java/appinsights-333facets.png)




## Triagem


Triagem - avaliar a gravidade e a extensão de um problema - é a primeira etapa depois da detecção. Devemos chamar a equipe à meia-noite? Ou o problema pode esperar até o próximo intervalo conveniente na lista de pendências? Há algumas perguntas cruciais na triagem.


Que quantidade do problema em questão está acontecendo? Os gráficos na folha visão geral oferecem alguma perspectiva para um problema. Por exemplo, o aplicativo da Fabrikam gerou quatro alertas de teste da Web em uma noite. Examinando o gráfico pela manhã, a equipe pôde ver que existiam realmente alguns pontos vermelhos, embora a maioria dos testes tenham resultado em verde. Detalhando o gráfico de disponibilidade, ficou claro que todos esses problemas intermitentes eram de um local de teste. Isso, obviamente, foi um problema de rede que afetou somente uma rota e provavelmente se resolveria sozinho.


Por outro lado, um aumento drástico e estável no gráfico de tempos de resposta ou contagens de exceção é, obviamente, motivo para pânico.


Uma tática de triagem útil é “Experimente você mesmo”. Se você tiver o mesmo problema, saberá que ele é real.


Que fração dos usuários são afetados? Para obter uma resposta aproximada, divida a taxa de falha pela contagem de sessão.


![Gráficos de sessões e solicitações com falha](./media/app-insights-detect-triage-diagnose-java/10-failureRate.png)

No caso de resposta lenta, compare a tabela das solicitações com respostas mais lentas com a frequência de uso de cada página.


Quão importante é o cenário bloqueado? Se esse for um problema funcional bloqueando uma história de usuário específica, isso importa muito? Se os clientes não podem pagar suas contas, isso é sério; se eles não podem alterar suas preferências de cor da tela, talvez isso possa esperar. Os detalhes do evento ou da exceção, ou a identidade da página lenta, informam onde os clientes estão tendo problemas.


## Diagnóstico


O diagnóstico não é exatamente o mesmo que depuração. Antes de iniciar o rastreamento por meio do código, você deve ter uma ideia de porquê, de quando e de onde o problema está ocorrendo.


**Quando isso acontece?** A exibição do histórico fornecida pelos gráficos de evento e de métrica facilita correlacionar os efeitos e as possíveis causas. Se houver picos intermitentes em taxas de exceção ou de tempo de resposta, examine a contagem de solicitações: se eles atingirem o pico ao mesmo tempo, o cenário parece com o de um problema de recurso. Você precisa atribuir mais CPU ou memória? Ou é uma dependência que não é capaz de dar conta da carga?


**O problema é conosco?** Se você tiver uma queda repentina no desempenho de um determinado tipo de solicitação - por exemplo, quando o cliente deseja um demonstrativo de conta - há uma possibilidade de ser um subsistema externo em vez de seu aplicativo Web. No Metrics Explorer, selecione a taxa de falha de dependência e taxas de duração da dependência e compare seus históricos pelas últimas horas ou dias ao problema que é detectado. Se há alterações correlacionadas, um subsistema externo pode ser a causa.


![Gráficos de falha de dependência e a duração das chamadas para as dependências](./media/app-insights-detect-triage-diagnose-java/11-dependencies.png)

Alguns problemas de dependência de lentidão são problemas de localização geográfica. O Fabrikam Bank usa máquinas virtuais do Azure, e descobriu que eles tinham inadvertidamente localizado seu servidor Web e servidor de conta em diferentes países. Obtiveram uma melhoria expressiva migrando um deles.


**O que fizemos?** Se o problema não parece estar em uma dependência e se não esteve sempre lá, provavelmente é causado por uma alteração recente. A perspectiva histórica fornecida pelos gráficos de métrica e evento facilita correlacionar alterações repentinas com implantações. Isso reduz o escopo da busca pelo problema.


**O que está acontecendo?** Alguns problemas ocorrem apenas raramente e podem ser difíceis de rastrear por testes offline. Tudo o que podemos fazer é tentar capturar o bug quando ele ocorre, em tempo real. Você pode inspecionar os despejos de pilha em relatórios de exceção. Além disso, você pode escrever chamadas de rastreamento com sua estrutura de registros favorita ou com trackTrace() ou trackEvent().


A Fabrikam tinha um problema intermitente com transferências entre contas, mas apenas com determinados tipos de conta. Para entender melhor o que estava acontecendo, eles inseriram chamadas trackTrace() em pontos-chave do código, anexando o tipo de conta como uma propriedade a cada chamada. Isso facilitou filtrar apenas esses rastreamentos na pesquisa de diagnóstico. Eles também anexaram valores de parâmetro, como propriedades e medidas, às chamadas de rastreamento.


## Lidando com o problema


Depois de diagnosticar o problema, você pode fazer um plano para corrigi-lo. Talvez você precise reverter uma alteração recente, ou talvez você possa simplesmente seguir em frente e corrigir o problema. Quando a correção tiver sido realizada, o Application Insights dirá a você se o processo foi bem-sucedido.


A equipe de desenvolvimento do banco da Fabrikam utiliza uma abordagem mais estruturada para medição de desempenho do que costumava utilizar antes do Application Insights.

* Eles definem metas de desempenho em termos de medidas específicas na página de visão geral do Application Insights.

* Eles criam medidas de desempenho no aplicativo desde o início, como as métricas que medem o progresso do usuário por meio de “funis”.




## Uso

O Application Insights também pode ser usado para saber o que os usuários fazem com um aplicativo. Depois que ele é executado sem problemas, a equipe gostaria de saber quais recursos são os mais populares, do que os usuários gostam ou com o que têm dificuldade e com que frequência eles retornam. Isso os ajudará a definir as prioridades para seus futuros trabalhos. Além disso, eles podem planejar medir o sucesso de cada recurso como parte do ciclo de desenvolvimento. [Leia mais][usage].



## Monitorando a atividade do usuário

Quando o tempo de resposta é constantemente bom com poucas exceções, a equipe de desenvolvimento pode pensar sobre como melhorar a experiência dos usuários e como encorajar mais usuários a atingir as metas desejadas.


Por exemplo, uma jornada típica do usuário no site tem um 'funil' claro: muitos clientes examinam as taxas de diferentes tipos de empréstimos; alguns deles preenchem o formulário de cotação; e daqueles que conseguem uma cotação, alguns vão em frente e pegam o empréstimo.

![](./media/app-insights-detect-triage-diagnose-java/12-funnel.png)

Considerando onde os números maiores de clientes caem, a empresa pode trabalhar em como conseguir mais usuários na parte inferior do funil. Em alguns casos pode haver uma falha de experiência (UX) do usuário - por exemplo, o botão "Avançar" é difícil de encontrar ou as instruções não são óbvias. Provavelmente, existem motivos de negócio mais significativos para transferências de depósito: talvez as taxas de empréstimo são muito altas.

Sejam quais forem as razões, os dados ajudam a equipe a descobrir o que os usuários estão fazendo. Mais chamadas de acompanhamento podem ser inseridas para descobrir mais detalhes. trackEvent() pode ser usado para a contagem de quaisquer ações do usuário, que vão desde os detalhes refinados de cliques de botão individuais até realizações significativas como o pagamento de um empréstimo.

A equipe está acostumando-se a ter informações sobre a atividade do usuário. Hoje em dia, sempre que criam um novo recurso, eles pensam como irão receber comentários sobre seu uso. Eles criam chamadas de rastreamento para o recurso desde o início. Eles usam os comentários para melhorar o recurso em cada ciclo de desenvolvimento.




## Seus aplicativos

Portanto, é assim que uma equipe usa o Application Insights não apenas para corrigir problemas individuais, mas para melhorar seu ciclo de vida de desenvolvimento. Espero que isso tenha dado a você algumas ideias sobre como o Application Insights pode lhe ajudar a melhorar o desempenho dos seus próprios aplicativos.

## Vídeo

[AZURE.VIDEO performance-monitoring-application-insights]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[usage]: app-insights-web-track-usage.md
 

<!---HONumber=Oct15_HO3-->