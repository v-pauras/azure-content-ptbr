<properties
 pageTitle="Tamanhos dos serviços de nuvem"
 description="Lista os tamanhos diferentes para funções de web e de trabalho do serviço de nuvem do Azure."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="09/14/2015"
 ms.author="adegeo"/>

# Tamanhos dos serviços de nuvem

Este tópico descreve os tamanhos disponíveis e as opções para instâncias de função de Serviço de Nuvem (funções web e funções de trabalho). Ele também fornece considerações de implantação a serem observadas ao planejar o uso desses recursos.

As Máquinas Virtuais e os Serviços de Nuvem do Azure são dois dos vários tipos de recursos de computação oferecidos pelo Azure. Para obter explicações, confira [Computar opções de hospedagem fornecidas pelo Azure](fundamentals-application-models.md)

> [AZURE.NOTE]Para ver os limites relacionados do Azure, confira [Assinatura do Azure e limites de serviços, cotas e restrições](../azure-subscription-service-limits.md).

## Tamanhos de instâncias de função web e de trabalho

As considerações a seguir podem ajudá-lo a escolher um tamanho:

* As instâncias das VMs da série D são projetadas para executar aplicativos que exigem maior capacidade de computação e de desempenho de disco temporário. As VMs da série D fornecem processadores mais rápidos, uma maior taxa de memória por núcleo e uma unidade de estado sólido (SSD) para o disco temporário. Para obter detalhes, consulte o anúncio no blog do Azure, [Novos tamanhos de máquina virtual da série D](http://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).  

*   A série Dv2, uma continuação da série D original, apresenta uma CPU mais potente. A CPU da série Dv2 é aproximadamente 35% mais rápida do que a CPU da série D. Ela se baseia na última geração do processador Intel Xeon® E5-2673 v3 (Haswell) de 2,4 GHz e, com a Intel Turbo Boost Technology 2.0, pode chegar a até 3,2 GHz. A série Dv2 tem as mesmas configurações de memória e disco que a série D.

    A disponibilidade regional da série Dv2 se baseará nesse cronograma: outubro de 2015: Leste dos EUA 2, Centro dos EUA, Centro-Norte dos EUA, Oeste dos EUA; novembro de 2015: Leste dos EUA, Norte da Europa, Oeste da Europa; janeiro de 2016: Centro-Sul dos EUA, Leste do Pacífico Asiático, Sudeste do Pacífico Asiático, Leste do Japão, Oeste do Japão, Leste da Austrália, Sudeste da Austrália e Sul do Brasil

* Funções web e funções de trabalho exigem mais espaço em disco temporário que as máquinas virtuais do Azure devido aos requisitos de sistema. Os arquivos do sistema reservam 4 GB de espaço para o arquivo de paginação do Windows e 2 GB de espaço para o arquivo de despejo de memória do Windows.

* O disco do sistema operacional contém o SO Windows convidado e inclui a pasta Arquivos de Programas (incluindo as instalações feitas por meio de tarefas de inicialização, a menos que você especifique outro disco), as alterações no Registro, a pasta System32 e o .NET Framework.

* O disco de recursos local contém logs e arquivos de configuração do Azure, diagnóstico do Azure (que inclui os logs do IIS) e quaisquer recursos de armazenamento local que você definir.

* O disco de aplicativos é onde o .cspkg é extraído e inclui seu site, binários, processo de host de função, tarefas de inicialização, web.config e assim por diante.

* Os tamanhos de máquina virtual A8/A10 e A9/A11 têm as mesmas capacidades. As instâncias de máquina virtual A8 e A9 incluem um adaptador de rede adicional que é conectado a uma rede de Acesso Remoto Direto à Memória (RDMA) para uma comunicação rápida entre máquinas virtuais. As instâncias A8 e A9 destinam-se a aplicativos de computação de alto desempenho que exigem comunicação constante e de baixa latência entre os nós durante a execução como, por exemplo, os aplicativos que usam MPI (Message Passing Interface). As instâncias de máquina virtual A10 e A11 não incluem o adaptador de rede adicional. As instâncias A10 e A11 destinam-se a aplicativos de computação de alto desempenho que não exigem comunicação constante e de baixa latência entre os nós, também conhecidos como aplicativos paramétricos ou totalmente paralelos.

|Tamanho|CPU<br>núcleos|Memória|Tamanhos do disco|
|---|---|---|---|
|ExtraSmall|1|768 MB|SO = tamanho do SO convidado<br/>Recurso local = 15384 MB<br/>Aplicativos = aprox. 1,5 GB|
|Pequena|1|1,75 GB|SO = tamanho do SO convidado<br/>Recurso local = 225304 MB<br/>Aplicativos = aprox. 1,5 GB|
|Média|2|3,5 GB|SO = tamanho do SO convidado<br/>Recurso local = 496664 MB<br/>Aplicativos = aprox. 1,5 GB|
|Grande|4|7 GB|SO = tamanho do SO convidado<br/>Recurso local = 1018904 MB<br/>Aplicativos = aprox. 1,5 GB|
|ExtraLarge|8|14 GB|SO = tamanho do SO convidado<br/>Recurso local = 2083864 MB<br/>Aplicativos = aprox. 1,5 GB|
|A5|2|14 GB|SO = tamanho do SO convidado<br/>Recurso local = 496664 MB<br/>Aplicativos = aprox. 1,5 GB|
|A6|4|28 GB|SO = tamanho do SO convidado<br/>Recurso local = 1018904 MB<br/>Aplicativos = aprox. 1,5 GB|
|A7|8|56 GB|SO = tamanho do SO convidado<br/>Recurso local = 2083864 MB<br/>Aplicativos = aprox. 1,5 GB
|A8|8|56 GB|SO = tamanho do SO convidado<br/>Recurso local = 1856172 TB<br/>Aplicativos = aprox. 1,5 GB<blockquote> Observação: para obter informações e considerações sobre o uso desse tamanho, consulte <a href="http://go.microsoft.com/fwlink/p/?linkid=328042">Sobre as instâncias intensivas de computação A8, A9, A10 e A11</a>.</blockquote>|
|A9|16|112 GB|SO = tamanho do SO convidado<br/>Recurso local = 1856172 TB<br/>Aplicativos = aprox. 1,5 GB<blockquote> Observação: para obter informações e considerações sobre o uso desse tamanho, consulte <a href="http://go.microsoft.com/fwlink/p/?linkid=328042">Sobre as instâncias intensivas de computação A8, A9, A10 e A11</a>.</blockquote>|
|A10|8|56 GB|SO = tamanho do SO convidado<br/>Recurso local = 1856172 TB<br/>Aplicativos = aprox. 1,5 GB<blockquote> Observação: para obter informações e considerações sobre o uso desse tamanho, consulte <a href="http://go.microsoft.com/fwlink/p/?linkid=328042">Sobre as instâncias intensivas de computação A8, A9, A10 e A11</a>.</blockquote>|
|A11|16|112 GB|SO = tamanho do SO convidado<br/>Recurso local = 1856172 TB<br/>Aplicativos = aprox. 1,5 GB<blockquote> Observação: para obter informações e considerações sobre o uso desse tamanho, consulte <a href="http://go.microsoft.com/fwlink/p/?linkid=328042">Sobre as instâncias intensivas de computação A8, A9, A10 e A11</a>.</blockquote>|
|Standard\_D1|1|3,5 GB|SO = tamanho do SO convidado<br/>Recurso local = 46104 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D2|2|7 GB|SO = tamanho do SO convidado<br/>Recurso local = 97304 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D3|4|14 GB|SO = tamanho do SO convidado<br/>Recurso local = 199704 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D4|8|28 GB|SO = tamanho do SO convidado<br/>Recurso local = 404504 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D11|2|14 GB|SO = tamanho do SO convidado<br/>Recurso local = 97304 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D12|4|28 GB|SO = tamanho do SO convidado<br/>Recurso local = 199704 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D13|8|56 GB|SO = tamanho do SO convidado<br/>Recurso local = 404504 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D14|16|112 GB|SO = tamanho do SO convidado<br/>Recurso local = 814104 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D1\_v2|1|3,5 GB|SO = tamanho do SO convidado<br/>Recurso local = 46104 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D2\_v2|2|7 GB|SO = tamanho do SO convidado<br/>Recurso local = 97304 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D3\_v2|4|14 GB|SO = tamanho do SO convidado<br/>Recurso local = 199704 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D4\_v2|8|28 GB|SO = tamanho do SO convidado<br/>Recurso local = 404504 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D5\_v2|16|56 GB|SO = tamanho do SO convidado<br/>Recurso local = 814104 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D11\_v2|2|14 GB|SO = tamanho do SO convidado<br/>Recurso local = 97304 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D12\_v2|4|28 GB|SO = tamanho do SO convidado<br/>Recurso local = 199704 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D13\_v2|8|56 GB|SO = tamanho do SO convidado<br/>Recurso local = 404504 MB<br/>Aplicativos = aprox. 1,5 GB|
|Standard\_D14\_v2|16|112 GB|SO = tamanho do SO convidado<br/>Recurso local = 814104 MB<br/>Aplicativos = aprox. 1,5 GB|
## Configurar tamanhos para os Serviços de Nuvem

Você pode especificar o tamanho da Máquina Virtual de uma instância de função como parte do modelo de serviço descrito pelo arquivo de definição de serviço. O tamanho da função determina o número de núcleos de CPU, a capacidade de memória e o tamanho do sistema de arquivos local alocados para uma instância em execução. Escolha o tamanho da função com base em requisitos de recurso do aplicativo.

Veja um exemplo de definição do tamanho da função como pequeno para uma instância de Função Web:


    <WebRole name="WebRole1" vmsize="Small">
    …
    </WebRole>

Assegure-se de que o tamanho do recurso local especificado seja menor ou igual ao tamanho do recurso local máximo na tabela acima.
## Próximas etapas

[Configurar um serviço de nuvem para o Azure](https://msdn.microsoft.com/library/hh124108)

<!---HONumber=Oct15_HO3-->