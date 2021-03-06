<properties
   pageTitle="Visão geral de comunicação do Reliable Service | Microsoft Azure"
   description="Visão geral do modelo de comunicação do Reliable Service, incluindo a abertura de ouvintes nos serviços, solução de pontos de extremidade e comunicação entre serviços."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="11/17/2015"
   ms.author="vturecek@microsoft.com"/>

# O modelo de comunicação Reliable Service

O Service Fabric como uma plataforma é totalmente independente para comunicações entre serviços. Os protocolos e pilhas são bastante equilibrados, de UDP a HTTP. Cabe ao desenvolvedor determinar a forma de comunicação entre os serviços. A estrutura do aplicativo Reliable Services fornece algumas pilhas de comunicação predefinidas, bem como ferramentas para implantar uma pilha de comunicação personalizada, como abstrações que os clientes podem usar para descobrir e se comunicar com os Pontos de Extremidade de Serviço.

## Configurando a comunicação de serviço

A API do Reliable Services usa uma interface simples para comunicação de serviço. Para abrir um ponto de extremidade para o serviço, basta implementar essa interface:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Em seguida, adicione a implementação do ouvinte de comunicação, retornando-a em uma substituição do método de classe base do serviço.

Sem monitoração de estado:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
```

Com monitoração de estado:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
```

Em ambos os casos,você retorna uma coleção de ouvintes, permitindo que o serviço use facilmente vários ouvintes; por exemplo, você pode ter um ouvinte HTTP e um ouvinte WebSocket separado. Cada ouvinte recebe um nome e a coleção resultante do nome: os pares de endereços são representados como um objeto JSON quando um cliente solicita os endereços de ouvintes para uma instância ou partição de serviço.

Em um serviço sem estado, a substituição retorna uma coleção de ServiceInstanceListeners. Um ServiceInstanceListener contém apenas uma função para criar e nomear um ICommunicationListener. Para os serviços com monitoração de estado, a substituição retorna uma coleção de ServiceReplicaListeners. Esse serviço é um pouco diferente do equivalente sem estado, pois o ServiceReplicaListener tem uma opção para abrir um ICommunicationListener em réplicas secundárias. Isso lhe permite usar vários ouvintes de comunicação em um serviço, além de determinar aqueles que aceitam solicitações em réplicas secundárias e os que ouvem apenas em réplicas primárias.

Por exemplo, temos um ServiceRemotingListener para fazer chamadas RPC apenas em réplicas primárias e um ouvinte personalizado que faz solicitações de leitura em réplicas secundárias:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(initParams =>
            new MyCustomListener(initParams),
            "customReadonlyEndpoint",
            true),

        new ServiceReplicaListener(initParams =>
            new ServiceRemotingListener<IMyStatefulInterface2>(initParams, this),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

Por fim, descreva os pontos de extremidade necessários para o serviço no [manifesto do serviço](service-fabric-application-model.md), na seção Pontos de extremidade.

```xml

<Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" Protocol="http" Type="Input" />
    <Endpoints>
</Resources>

```

O ouvinte de comunicação pode acessar os recursos do ponto de extremidade alocados para ele no `CodePackageActivationContext`, no `ServiceInitializationParameters`, e começar a escutar solicitações quando ele é aberto.

```csharp

var codePackageActivationContext = this.serviceInitializationParameters.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE]Os recursos dos pontos de extremidade são comuns a todo o pacote de serviço e são alocados pela Service Fabric quando o pacote de serviço é ativado. Assim, todas as réplicas hospedadas no mesmo ServiceHost compartilham a mesma porta. Isso significa que o ouvinte de comunicação deve oferecer suporte ao compartilhamento de porta. A maneira recomendada de fazer isso é que o ouvinte de comunicação use a ID da partição e a ID de réplica/instância ao gerar o endereço de escuta.

```csharp

var replicaOrInstanceId = 0;
var parameters = this.serviceInitializationParameters as StatelessServiceInitializationParameters;
if (parameters != null)
{
   replicaOrInstanceId = parameters.InstanceId;
}
else
{
   replicaOrInstanceId = ((StatefulServiceInitializationParameters) this.serviceInitializationParameters).ReplicaId;
}

var nodeContext = FabricRuntime.GetNodeContext();

var listenAddress = new Uri(
    string.Format(
        CultureInfo.InvariantCulture,
        "{0}://{1}:{2}/{5}/{3}-{4}",
        scheme,
        nodeContext.IPAddressOrFQDN,
        port,
        this.serviceInitializationParameters.PartitionId,
        replicaOrInstanceId,
        Guid.NewGuid().ToString()));

```

Para obter uma explicação mais completa sobre a criação de um `ICommunicationListener`, confira [Introdução aos serviços da API Web do Service Fabric do Microsoft Azure com host automático OWIN](service-fabric-reliable-services-communication-webapi.md)

## Comunicação do cliente com o serviço
A API do Reliable Services fornece as abstrações a seguir, que facilitam a criação de clientes para comunicação com serviços.

### ServicePartitionResolver
Essa classe do utilitário ajuda o cliente a determinar o ponto de extremidade de um serviço Service Fabric no tempo de execução. O processo de determinar o ponto de extremidade de um serviço é chamado de Resolução do Ponto de Extremidade de Serviço na terminologia da Malha do Serviço.

```csharp

public delegate FabricClient CreateFabricClientDelegate();

// ServicePartitionResolver methods

public ServicePartitionResolver(CreateFabricClientDelegate createFabricClient);

Task<ResolvedServicePartition> ResolveAsync(Uri serviceName,
   long partitionKey,
   CancellationToken cancellationToken);

Task<ResolvedServicePartition> ResolveAsync(ResolvedServicePartition previousRsp,
   CancellationToken cancellationToken);


```
> [AZURE.NOTE]FabricClient é o objeto usado para se comunicar com o cluster da Malha do Serviço em várias operações de gerenciamento no cluster da Malha do Serviço.

Normalmente, o código do cliente não precisa trabalhar com `ServicePartitionResolver` diretamente. Ele é criado e passado para outras classes auxiliares na API do Reliable Services, que usam o resolvedor internamente e ajudam o cliente a se comunicar com o serviço.

### CommunicationClientFactory
`ICommunicationClientFactory` define a interface base implementada por uma fábrica de cliente de comunicação que produz clientes que podem se comunicar com um serviço ServiceFabric. A implementação de CommunicationClientFactory dependerá da pilha de comunicação usada pelo serviço da Malha do Serviço com o qual o cliente deseja se comunicar. A API do Reliable Services fornece um CommunicationClientFactoryBase<TCommunicationClient> que fornece uma implementação básica dessa interface `ICommunicationClientFactory` e executa tarefas que são comuns a todas as pilhas de comunicação (como usar um `ServicePartitionResolver` para determinar o ponto de extremidade de serviço). Os clientes geralmente implementam a classe abstrata CommunicationClientFactoryBase para lidar com lógica específica da pilha de comunicação.

```csharp

protected CommunicationClientFactoryBase(
   ServicePartitionResolver servicePartitionResolver = null,
   IEnumerable<IExceptionHandler> exceptionHandlers = null,
   IEnumerable<Type> doNotRetryExceptionTypes = null);


public class MyCommunicationClient : ICommunicationClient
{
   public MyCommunicationClient(MyCommunicationChannel communicationChannel)
   {
      this.CommunicationChannel = communicationChannel;
   }
   public MyCommunicationChannel CommunicationChannel { get; private set; }
   public ResolvedServicePartition ResolvedServicePartition;
}

public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
        throw new NotImplementedException();
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
        throw new NotImplementedException();
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
        throw new NotImplementedException();
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
        throw new NotImplementedException();
    }
}

```

### ServicePartitionClient
`ServicePartitionClient` usa CommunicationClientFactory (e internamente o ServicePartitionResolver) e ajuda na comunicação com o serviço ao lidar com repetições para exceções transitórias da Malha do Serviço e comunicação comum.

```csharp

public ServicePartitionClient(
    ICommunicationClientFactory<TCommunicationClient> factory,
    Uri serviceName,
    long partitionKey);

public async Task<TResult> InvokeWithRetryAsync<TResult>(
    Func<TCommunicationClient, Task<TResult>> func,
    CancellationToken cancellationToken,
    params Type[] doNotRetryExceptionTypes)

```

Um padrão de uso típico seria assim:

```csharp

public MyCommunicationClientFactory myCommunicationClientFactory;
public Uri myServiceUri;

... other client code ..

// Call the service corresponding to the partitionKey myKey.

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(
   client =>
   {
      // Communicate with the service using the client.
      throw new NotImplementedException();
   },
   CancellationToken.None);


... other client code ...

```

## Próximas etapas
* [Chamada de procedimento remoto com Reliable Services remoto](service-fabric-reliable-services-communication-remoting.md)

* [API Web com OWIN no Reliable Services](service-fabric-reliable-services-communication-webapi.md)

* [Comunicação WCF com o Reliable Services](service-fabric-reliable-services-communication-wcf.md)

 

<!---HONumber=Nov15_HO4-->