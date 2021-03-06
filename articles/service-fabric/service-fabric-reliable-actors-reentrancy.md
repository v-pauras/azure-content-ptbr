<properties
   pageTitle="Reentrada de Atores Confiáveis"
   description="Introdução à reentrada para Atores Confiáveis do Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/14/2015"
   ms.author="amanbha"/>


# Reentrada de Ator Confiável
Por padrão, os Atores de Malha permitem reentrância baseada no contexto de chamada lógica. Isso possibilita que os atores sejam reentrantes se estiverem na mesma cadeia de contexto de chamada. Por exemplo, se um ator A envia a mensagem para o ator B, que envia a mensagem para o ator C. Como parte do processamento da mensagem no caso de o ator C chamar o ator A, a mensagem é reentrante e será permitida. Todas as outras mensagens que fazem parte de um contexto de chamada diferente serão bloqueadas no Ator A até a conclusão do processamento.

Os atores que não querem permitir a reentrância baseada no contexto de chamada lógica podem desabilitá-la decorando a classe do ator com `ReentrantAttribute(ReentrancyMode.Disallowed)`.

```csharp
public enum ReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

O código a seguir mostra a classe de ator que define o modo de reentrância como `ReentrancyMode.Disallowed`. Nesse caso, se um ator envia uma mensagem reentrante para outro ator, será lançada uma exceção do tipo `FabricException`.

```csharp
[Reentrant(ReentrancyMode.Disallowed)]
class VoicemailBoxActor : StatefulActor<VoicemailBox>, IVoicemailBoxActor
{
    ...
}
```

<!---HONumber=Nov15_HO4-->