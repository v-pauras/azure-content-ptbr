<properties 
	pageTitle="Configurar o SDK do Application Insights com ApplicationInsights.config ou .xml" 
	description="Habilitar ou desabilitar módulos de coleta de dados e adicionar contadores de desempenho e outros parâmetros" 
	services="application-insights"
    documentationCenter="" 
	authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
	manager="douge"/>
 
<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/05/2015" 
	ms.author="awills"/>

# Configurar o SDK do Application Insights com ApplicationInsights.config ou.xml

O SDK .NET do Application Insights consiste em vários pacotes NuGet. O [pacote principal](http://www.nuget.org/packages/Microsoft.ApplicationInsights) fornece a API para enviar telemetria ao Application Insights. Os [pacotes adicionais](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) fornecem _módulos_ e _inicializadores_ de telemetria para rastreamento automático de telemetria do seu aplicativo e respectivo contexto. Ajustando o arquivo de configuração, você pode habilitar ou desabilitar módulos e inicializadores de telemetria, bem como definir parâmetros para alguns deles.

O arquivo de configuração é chamado `ApplicationInsights.config` ou `ApplicationInsights.xml`, dependendo do tipo do seu aplicativo. Ele é adicionado automaticamente ao seu projeto quando você [instala a maioria das versões do SDK][start]. Ele também é adicionado a um aplicativo Web pelo [Status Monitor em um servidor IIS][redfield] ou quando você seleciona a [extensão do Application Insights para um site ou VM do Azure][azure].

Não há um arquivo equivalente para controlar o [SDK em uma página da Web][client].

Este documento descreve as seções que você vê no arquivo de configuração, como controlar os componentes do SDK, e quais pacotes NuGet carregar esses componentes.

## Módulos de telemetria (ASP.NET)

Cada módulo de telemetria coleta um tipo específico de dados e usa o API principal para enviar os dados. Os módulos são instalados por diferentes pacotes NuGet, que também adicionam as linhas necessárias para o arquivo. config.

Há um nó no arquivo de configuração para cada módulo. Para desabilitar um módulo, exclua o nó ou remova o comentário dele.



### Acompanhamento de dependência

O [Acompanhamento de dependência](app-insights-dependencies.md) coleta a telemetria sobre chamadas para bancos de dados e serviços externos e torna o seu aplicativo. Para permitir que esse módulo funcione em um servidor IIS, é necessário [instalar o Monitor de Status][redfield]. Para usá-lo em aplicativos Web do Azure ou VMs, [selecione a extensão do Application Insights][azure].

Você também pode escrever seu próprio código de rastreamento de dependência usando a [API TrackDependency](app-insights-api-custom-events-metrics.md#track-dependency).


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* Pacote NuGet [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector).

### Coletor de desempenho

[Coleta contadores de desempenho do sistema](app-insights-web-monitor-performance.md#system-performance-counters) como CPU, memória e carga de rede de instalações do IIS. Você pode especificar quais contadores coletar, incluindo contadores de desempenho que você configurou por si mesmo.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* Pacote NuGet [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector).


### Telemetria de diagnóstico do Application Insights

Os erros de `DiagnosticsTelemetryModule` relatórios na instrumentação do Application Insights se codificam sozinhos. Por exemplo, se o código não puder acessar contadores de desempenho ou se um `ITelemetryInitializer` lançar uma exceção. A telemetria de rastreamento rastreada por esse módulo aparece na [Pesquisa de Diagnóstico][diagnostic].
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* Pacote NuGet [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights). Se você instalar apenas esse pacote, o arquivo applicationinsights. config não é criado automaticamente. 

### Modo de desenvolvedor

`DeveloperModeWithDebuggerAttachedTelemetryModule` força `TelemetryChannel` do Application Insights a enviar dados imediatamente, um item de telemetria por vez, quando um depurador é conectado ao processo de aplicativo. Isso reduz a quantidade de tempo entre o momento em que seu aplicativo rastreia a telemetria e quando ele aparece no portal do Application Insights. Faz com uma sobrecarga significativa na largura de banda de CPU e rede.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* Pacote NuGet do [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/).

### Acompanhamento de solicitação da Web

Relatórios do [código de tempo e o resultado da resposta](app-insights-asp-net.md) de solicitações HTTP.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* Pacote NuGet [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)

### Acompanhamento de exceções

`ExceptionTrackingTelemetryModule` rastreia exceção sem tratamento no seu aplicativo Web. Consulte [Falhas e exceções][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* Pacote NuGet [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` -rastreia [exceções de tarefa não observada](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` -rastreia as exceções sem tratamento para funções de trabalho, serviços do Windows e aplicativos de console.
* Pacote NuGet do [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/).

### API principal

O pacote básico fornece o [API principal](https://msdn.microsoft.com/library/mt420197.aspx) do SDK. Usam os outros módulos de telemetria e você também pode [usá-lo para definir sua própria telemetria](app-insights-api-custom-events-metrics.md).

* Nenhuma entrada em ApplicationInsights.config.
* Pacote NuGet [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights). Se você acabou de instalar este NuGet, nenhum arquivo. config será gerado.

## Canal de telemetria

O canal de telemetria gerencia o armazenamento em buffer e transmissão de telemetria ao serviço Application Insights.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` é o canal padrão para serviços. Ele armazena em buffer dados na memória.
* `Microsoft.ApplicationInsights.PersistenceChannel` é uma alternativa para aplicativos de console. Pode salvar os dados não certificados para armazenamento persistente quando seu aplicativo estiver fechado e vai enviá-los quando o aplicativo for iniciado novamente.


## Inicializadores de telemetria (ASP.NET)

Os inicializadores de telemetria definem propriedades de contexto que são enviadas com cada item de telemetria.

Você pode [escrever seus próprios inicializadores](app-insights-api-filtering-sampling.md#add-properties) para definir as propriedades de contexto.

Os inicializadores padrão foram todos configurados pelos pacotes do WindowsServer NuGet ou Web:


* `AccountIdTelemetryInitializer` define a propriedade AccountId.
* `AuthenticatedUserIdTelemetryInitializer` define a propriedade AuthenticatedUserId como definido pelo SDK do JavaScript.
* `AzureRoleEnvironmentTelemetryInitializer` atualiza as propriedades `RoleName` e `RoleInstance` do contexto `Device` para todos os itens de telemetria com informações extraídas do ambiente de tempo de execução do Azure.
* `BuildInfoConfigComponentVersionTelemetryInitializer` atualiza a `Version` propriedade do `Component` contexto para todos os itens de telemetria com o valor extraído do arquivo `BuildInfo.config` produzido pelo MS Build.
* `ClientIpHeaderTelemetryInitializer` atualiza a propriedade `Ip` do contexto `Location` de todos os itens de telemetria baseados no cabeçalho HTTP `X-Forwarded-For` da solicitação.
* `DeviceTelemetryInitializer` atualiza as propriedades a seguir do contexto `Device` para todos os itens de telemetria.
 - `Type` é definido como "PC"
 - `Id` é definido para o nome de domínio do computador onde o aplicativo Web está em execução.
 - `OemName` é definido para o valor extraído do campo `Win32_ComputerSystem.Manufacturer` usando WMI.
 - `Model` é definido para o valor extraído do campo `Win32_ComputerSystem.Model` usando WMI.
 - `NetworkType` é definido para o valor extraído de `NetworkInterface`.
 - `Language` é definido para o nome da `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer` atualiza a propriedade `RoleInstance` do contexto `Device` para todos os itens de telemetria com o nome de domínio do computador onde o aplicativo Web está em execução.
* `OperationNameTelemetryInitializer` atualiza a propriedade `Name` das propriedades `RequestTelemetry` e `Name` do contexto `Operation` de todos os itens de telemetria baseados no método HTTP, bem como nomes do controlador MVC do ASP.NET e ação invocada para processar a solicitação.
* `OperationIdTelemetryInitializer` atualiza a `Operation.Id` propriedade de contexto de todos os itens de telemetria rastreadas ao manipular uma solicitação com o gerado automaticamente `RequestTelemetry.Id`.
* `SessionTelemetryInitializer` atualiza a propriedade `Id` do contexto `Session` para todos os itens de telemetria com valor extraído do cookie `ai_session` gerado pelo código de instrumentação do JavaScript do ApplicationInsights em execução no navegador do usuário. 
* `SyntheticTelemetryInitializer` atualiza `User`, `Session` as `Operation` propriedades de contexto de todos os itens de telemetria rastreados ao lidar com uma solicitação de uma fonte sintética, como uma disponibilidade de teste pesquisa de bot de mecanismo. Por padrão, o [Metrics Explorer](app-insights-metrics-explorer.md) não exibe telemetria sintética.
* `UserAgentTelemetryInitializer` atualiza a propriedade `UserAgent` do contexto `User` de todos os itens de telemetria baseados no cabeçalho HTTP `User-Agent` da solicitação.
* `UserTelemetryInitializer` atualiza as propriedades `Id` e `AcquisitionDate` do contexto `User` para todos os itens de telemetria com valores extraídos do cookie `ai_user` gerado pelo código de instrumentação do JavaScript do Application Insights em execução no navegador do usuário.


## Processadores de Telemetria (ASP.NET)

Processadores de telemetria podem filtrar e modificar cada item de telemetria antes de serem enviado do SDK para o portal.

Você pode [escrever seus próprios processadores de telemetria](app-insights-api-filtering-sampling.md#filtering).


#### Processador de telemetria de amostragem adaptável (da 2.0.0-beta3)

Isso é habilitado por padrão. Se o seu aplicativo envia muita telemetria, esse processador remove parte dela.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

O parâmetro fornece o destino que o algoritmo tenta obter. Cada instância do SDK funciona independentemente, portanto, se o servidor for um cluster de vários computadores, o volume real de telemetria será multiplicado adequadamente.

[Saiba mais sobre a amostragem](app-insights-sampling.md).



#### Processador de telemetria de amostragem de taxa fixa (da 2.0.0-beta1)

Também há um [processador de telemetria de amostra](app-insights-api-filtering-sampling.md#sampling) padrão (de 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## Parâmetros de canal (Java)

Esses parâmetros afetam como o SDK do Java deve armazenar e liberar os dados de telemetria coletados.

#### MaxTelemetryBufferCapacity

O número de itens de telemetria que podem ser armazenados na memória do SDK. Quando esse número for atingido, o buffer de telemetria é liberado, ou seja, os itens de telemetria são enviados para o servidor do Application Insights.

-	Mín.: 1
-	Máx.: 1.000
-	Padrão: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### FlushIntervalInSeconds 

Determina com que frequência os dados armazenados no armazenamento de memória devem ser liberados (enviada para o Application Insights).

-	Mín.: 1
-	Máx.: 300
-	Padrão: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### MaxTransmissionStorageCapacityInMB

Determina o tamanho máximo em MB alocado para o armazenamento persistente no disco local. Esse armazenamento é usado para itens de telemetria persistentes que falharam ao serem transmitidos para o ponto de extremidade do Application Insights. Quando o tamanho do armazenamento for atingido, novos itens de telemetria serão descartados.

-	Mín.: 1
-	Máx.: 100
-	Padrão: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## InstrumentationKey

Isso determina o recurso do Application Insights em que seus dados aparecem. Normalmente um recurso separado, com uma chave separada, é criado para cada um dos seus aplicativos.

Se você deseja definir a chave dinamicamente, por exemplo, se quiser enviar os resultados do seu aplicativo para outros recursos, é possível omitir a chave do arquivo de configuração e defini-lo no código.

Para definir a chave para todas as instâncias de TelemetryClient, incluindo módulos de telemetria padrão, defina a chave em TelemetryConfiguration.Active. Faça isso em um método de inicialização como global.aspx.cs em um serviço ASP.NET:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Se quiser enviar um conjunto específico de eventos para um recurso diferente, você pode definir a chave para um TelemetryClient específico:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Saiba mais sobre a API][api].

Para obter uma nova chave, [crie um novo recurso no portal do Application Insights][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

<!---HONumber=AcomDC_1203_2015-->