<properties
 pageTitle="Tamanhos de máquinas virtuais | Azure da Microsoft"
 description="Lista os diferentes tamanhos de máquinas virtuais e suas capacidades."
 services="virtual-machines"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>

<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="12/11/2015"
 ms.author="cynthn"/>

# Tamanhos das máquinas virtuais

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Visão geral

Este artigo descreve os tamanhos e as opções disponíveis para os recursos de computação baseados em máquina virtual que você pode usar para executar seus aplicativos e cargas de trabalho. Ele também fornece considerações de implantação a serem observadas ao planejar o uso desses recursos. Para obter informações sobre os preços dos vários tamanhos, consulte [Preços de máquinas virtuais](https://azure.microsoft.com/pricing/details/virtual-machines/).

Para ver os limites gerais em VMs do Azure, consulte [Limites de assinatura e serviços do Azure, cotas e restrições](../azure-subscription-service-limits.md).

Os tamanhos padrão consistem em várias séries: A, D, DS, G, e GS. Entre as considerações sobre algumas dessas dimensões estão:

*   As VMs da série D são projetadas para executar aplicativos que exigem maior capacidade de computação e de desempenho de disco temporário. As VMs da série D fornecem processadores mais rápidos, uma maior taxa de memória por núcleo e uma unidade de estado sólido (SSD) para o disco temporário. Para obter detalhes, confira o anúncio no blog do Azure, [Novos tamanhos de máquina virtual da série D](http://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

*   A série Dv2, uma continuação da série D original, apresenta uma CPU mais potente. A CPU da série Dv2 é aproximadamente 35% mais rápida do que a CPU da série D. Ela se baseia na última geração do processador Intel Xeon® E5-2673 v3 (Haswell) de 2,4 GHz e, com a Intel Turbo Boost Technology 2.0, pode chegar a até 3,2 GHz. A série Dv2 tem as mesmas configurações de memória e disco que a série D.

    A disponibilidade regional da série Dv2 se baseará nesse cronograma: outubro de 2015: Leste dos EUA 2, Centro dos EUA, Centro-Norte dos EUA, Oeste dos EUA; novembro de 2015: Leste dos EUA, Norte da Europa, Oeste da Europa; janeiro de 2016: Centro-Sul dos EUA, Leste do Pacífico Asiático, Sudeste do Pacífico Asiático, Leste do Japão, Oeste do Japão, Leste da Austrália, Sudeste da Austrália e Sul do Brasil


*   As VMs da série G oferecem o melhor desempenho e maior tamanho e são executadas em hosts com processadores da família Intel Xeon E5 V3.

*   As VMs das séries DS e GS podem usar o Armazenamento Premium, que fornece armazenamento de alto desempenho e baixa latência para cargas de trabalho com uso intenso de E/S. Essas VMs usam SSDs (unidades de estado sólido) para hospedar os discos da máquina virtual e também oferecem um cache de disco SSD local. O Armazenamento Premium está disponível em determinadas regiões. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho das máquinas virtuais do Azure](../storage-premium-storage-preview-portal.md).

O tamanho da máquina virtual afeta os preços. O tamanho também afeta a capacidade de processamento, memória e armazenamento da máquina virtual. Os custos de armazenamento são calculados separadamente com base nas páginas usadas na conta de armazenamento. Para obter detalhes, confira [Detalhes de preços de máquinas virtuais](http://azure.microsoft.com/pricing/details/virtual-machines/) e [Preços de armazenamento do Azure](http://azure.microsoft.com/pricing/details/storage/). Para obter mais detalhes sobre o armazenamento em VMs, veja [Sobre discos e VHDs para máquinas virtuais](virtual-machines-disks-vhds.md).

As considerações a seguir podem ajudá-lo a escolher um tamanho:


*   Alguns dos hosts físicos em data centers do Azure podem não aceitar tamanhos de máquina virtual maiores, como A5 – A11. Como resultado, você poderá ver a mensagem de erro **Falha ao configurar a máquina virtual <machine name>** ou **Falha ao criar a máquina virtual <machine name>** ao redimensionar uma máquina virtual existente para um novo tamanho, criar uma nova máquina virtual em uma rede virtual criada antes de 16 de abril de 2013 ou adicionar uma nova máquina virtual a um serviço de nuvem. Confira [Erro: "Falha ao configurar a máquina virtual"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) no fórum de suporte de soluções alternativas para cada cenário de implantação.  

*   Os tamanhos de máquina virtual A8/A10 e A9/A11 têm as mesmas capacidades. As instâncias de máquina virtual A8 e A9 incluem um adaptador de rede adicional que é conectado a uma rede de Acesso Remoto Direto à Memória (RDMA) para uma comunicação rápida entre máquinas virtuais. As instâncias A8 e A9 destinam-se a aplicativos de computação de alto desempenho que exigem comunicação constante e de baixa latência entre os nós durante a execução como, por exemplo, os aplicativos que usam MPI (Message Passing Interface). As instâncias de máquina virtual A10 e A11 não incluem o adaptador de rede adicional. As instâncias A10 e A11 destinam-se a aplicativos de computação de alto desempenho que não exigem comunicação constante e de baixa latência entre os nós, também conhecidos como aplicativos paramétricos ou totalmente paralelos.

*	As séries Dv2, D e G e as equivalentes DS/GS são ideais para aplicativos que exigem CPUs mais rápidas, melhor desempenho de disco local ou que têm maior demanda de memória. Elas oferecem uma combinação poderosa para vários aplicativos de nível empresarial.





## Tabelas de tamanho

As tabelas a seguir mostram os tamanhos e as capacidades oferecidas.

>[AZURE.NOTE]A capacidade de armazenamento é representada usando-se 1024^3 bytes como a unidade de medida para GB. Isso às vezes é chamado de gibibyte ou definição de base 2. Ao comparar tamanhos que usam diferentes sistemas de base, lembre-se de que os tamanhos de base 2 podem parecer menores do que os de base 10, mas para qualquer tamanho específico (por exemplo, 1 GB), um sistema de base 2 fornece mais capacidade do que um sistema de base 10, porque 1024^3 é maior que 1000^3.



## Camada Standard: série A

No modelo de implantação clássico, alguns tamanhos de VM são ligeiramente diferentes no Powershell e na CLI.

* Standard\_A0 é ExtraSmall 
* Standard\_A1 é pequeno
* Standard\_A2 é médio
* Standard\_A3 é grande
* Standard\_A4 é ExtraLarge

<br>

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)|
|---|---|---|---|---|---|---|
|Standard\_A0\\ExtraSmall |1|768 MB|1| Temporário = 20 GB |1|1 x 500|
|Standard\_A1\\Pequeno|1|1,75 GB|1|Temporário = 70 GB |2|2x500|
|Standard\_A2\\Médio|2|3,5 GB|1|Temporário = 135 GB |4|4x500|
|Standard\_A3\\Grande|4|7 GB|2|Temporário = 285 GB |8|8 x 500|
|Standard\_A4\\ExtraLarge|8|14 GB|4|Temporário = 605 GB |16|16 x 500|
|Standard\_A5|2|14 GB|1|Temporário = 135 GB |4|4X500|
|Standard\_A6|4|28 GB|2|Temporário = 285 GB |8|8 x 500|
|Standard\_A7|8|56 GB|4|Temporário = 605 GB |16|16 x 500|
|Standard\_A8|8|56 GB|2| Temporário = 382 GB Observação: Para informações e considerações sobre o uso desse tamanho, veja [Sobre as instâncias A8, A9, A10, e A11 de computação intensiva](http://go.microsoft.com/fwlink/p/?linkid=328042). |16|16 x 500|
|Standard\_A9|16|112 GB|4| Temporário = 382 GB Observação: Para informações e considerações sobre o uso desse tamanho, veja [Sobre as instâncias A8, A9, A10, e A11 de computação intensiva](http://go.microsoft.com/fwlink/p/?linkid=328042). |16|16 x 500|
|Standard\_A10|8|56 GB|2| Temporário = 382 GB Observação: Para informações e considerações sobre o uso desse tamanho, veja [Sobre as instâncias A8, A9, A10, e A11 de computação intensiva](http://go.microsoft.com/fwlink/p/?linkid=328042). |16|16 x 500|
|Standard\_A11|16|112 GB|4| Temporário = 382 GB Observação: Para informações e considerações sobre o uso desse tamanho, veja [Sobre as instâncias A8, A9, A10, e A11 de computação intensiva](http://go.microsoft.com/fwlink/p/?linkid=328042). |16|16 x 500|

## Camada Standard: série D

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)|
|---|---|---|---|---|---|---|
|Standard\_D1 |1|3,5 GB|1|Temporário (SSD) =50 GB |2|2x500|
|Standard\_D2 |2|7 GB|2|Temporário (SSD) =100 GB |4|4x500|
|Standard\_D3 |4|14 GB|4|Temporário (SSD) =200 GB |8|8 x 500|
|Standard\_D4 |8|28 GB|8|Temporário (SSD) =400 GB |16|16 x 500|
|Standard\_D11 |2|14 GB|2|Temporário (SSD) =100 GB |4|4x500|
|Standard\_D12 |4|28 GB|4|Temporário (SSD) =200 GB |8|8 x 500|
|Standard\_D13 |8|56 GB|8|Temporário (SSD) =400 GB |16|16 x 500|
|Standard\_D14 |16|112 GB|8|Temporário (SSD) =800 GB |32|32 x 500|

## Camada Standard: série Dv2

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)|
|---|---|---|---|---|---|---|
|Standard\_D1\_v2 |1|3,5 GB|1|Temporário (SSD) =50 GB |2|2x500|
|Standard\_D2\_v2 |2|7 GB|2|Temporário (SSD) =100 GB |4|4x500|
|Standard\_D3\_v2 |4|14 GB|4|Temporário (SSD) =200 GB |8|8 x 500|
|Standard\_D4\_v2 |8|28 GB|8|Temporário (SSD) =400 GB |16|16 x 500|
|Standard\_D5\_v2 |16|56 GB|8|Temporário (SSD) =800 GB |32|32 x 500|
|Standard\_D11\_v2 |2|14 GB|2|Temporário (SSD) =100 GB |4|4x500|
|Standard\_D12\_v2 |4|28 GB|4|Temporário (SSD) =200 GB |8|8 x 500|
|Standard\_D13\_v2 |8|56 GB|8|Temporário (SSD) =400 GB |16|16 x 500|
|Standard\_D14\_v2 |16|112 GB|8|Temporário (SSD) =800 GB |32|32 x 500|

## Camada Standard: série DS*

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Tamanho do cache (GB)|Máx. de IOPS de disco &amp; largura de banda|
|---|---|---|---|---|---|---|---|
|Standard\_DS1 |1|3,5|1|Disco SSD local = 7 GB |2|43| 3\.200 32 MB por segundo |
|Standard\_DS2 |2|7|2|Disco SSD local = 14 GB |4|86| 6\.400 64 MB por segundo |
|Standard\_DS3 |4|14|4|Disco SSD local = 28 GB |8|172| 12\.800 128 MB por segundo |
|Standard\_DS4 |8|28|8|Disco SSD local = 56 GB |16|344| 25\.600 256 MB por segundo |
|Standard\_DS11 |2|14|2|Disco SSD local = 28 GB |4|72| 6\.400 64 MB por segundo |
|Standard\_DS12 |4|28|4|Disco SSD local = 56 GB |8|144| 12\.800 128 MB por segundo |
|Standard\_DS13 |8|56|8|Disco SSD local = 112 GB |16|288| 25\.600 256 MB por segundo |
|Standard\_DS14 |16|112|8|Disco SSD local = 224 GB |32|576| 50\.000 512 MB por segundo |

**O máximo de operações de entrada/saída por segundo (IOPS) e de taxa de transferência (largura de banda) possível com uma VM da série DS é afetado pelo tamanho do disco. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho de máquinas virtuais do Azure](../storage-premium-storage-preview-portal.md).

## Camada Standard: série G

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)|
|---|---|---|---|---|---|---|
|Standard\_G1 |2|28 GB|1|Disco SSD local = 384 GB |4|4 x 500|
|Standard\_G2 |4|56 GB|2|Disco SSD local = 768 GB |8|8 x 500|
|Standard\_G3 |8|112 GB|4|Disco SSD local = 1.536 GB |16|16 x 500|
|Standard\_G4 |16|224 GB|8|Disco SSD local = 3.072 GB |32|32 x 500|
|Standard\_G5 |32|448 GB|8|Disco SSD local = 6.144 GB |64| 64 x 500 |

## Camada Standard: Série GS

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Tamanho do cache (GB)|Máx. de IOPS de disco &amp; largura de banda|
|---|---|---|---|---|---|---|---|
|Standard\_GS1|2|28|1|Disco SSD local = 56 GB |4|264| 5\.000 125 MB por segundo |
|Standard\_GS2|4|56|2|Disco SSD local = 112 GB |8|528| 10\.000 250 MB por segundo |
|Standard\_GS3|8|112|4|Disco SSD local = 224 GB |16|1056| 20\.000 500 MB por segundo |
|Standard\_GS4|16|224|8|Disco SSD local = 448 GB |32|2112| 40\.000 1.000 MB por segundo |
|Standard\_GS5|32|448|8|Disco SSD local = 896 GB |64|4224| 80\.000 2.000 MB por segundo |


### Consulte também

[Assinatura do Azure e limite de serviços, cotas e restrições](../azure-subscription-service-limits.md)

[Sobre as instâncias A8, A9, A10 e A11 com uso intensivo de computação](virtual-machines-a8-a9-a10-a11-specs.md)

<!---HONumber=AcomDC_1217_2015-->