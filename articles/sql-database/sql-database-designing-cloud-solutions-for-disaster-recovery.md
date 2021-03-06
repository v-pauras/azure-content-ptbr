<properties 
   pageTitle="Criando soluções de nuvem para recuperação de desastres usando replicação geográfica do banco de dados SQL"
   description="Aprenda a projetar sua solução de nuvem para recuperação de desastres escolhendo o padrão de failover correto."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="10/07/2015"
   ms.author="sashan"/>

# Projetando aplicativos de nuvem para continuidade de negócios usando a Replicação Geográfica

Há várias maneiras de tirar proveito da replicação geográfica no Banco de Dados SQL ao criar um aplicativo para continuidade dos negócios. Os fatores que orientam o design incluem a topologia de implantação do aplicativo, o contrato de nível de serviço que você está buscando, a latência de tráfego e os custos. Neste artigo, examinamos os padrões comuns de aplicativos e abordamos as vantagens e as desvantagens de cada opção.

## Padrão de design 1: implantação ativa-passiva para recuperação de desastre com banco de dados colocalizado

Essa opção é mais adequada para aplicativos com as seguintes características:

+ instância ativa em uma única região do Azure
+ alta dependência do acesso de RW (leitura/gravação) aos dados 
+ a conectividade entre regiões entre a lógica do aplicativo e o banco de dados não é aceitável devido ao custo de tráfego e à latência    

Nesse caso, a topologia de implantação do aplicativo é otimizada para lidar com desastres regionais quando todos os componentes do aplicativo são afetados e precisam fazer failover como uma unidade. Para redundância geográfica, a lógica do aplicativo e o banco de dados são replicados para outra região, mas não são usados para a carga de trabalho de aplicativo sob as condições normais. O aplicativo na região secundária deve ser configurado para usar uma cadeia de conexão SQL para o banco de dados secundário. O gerenciador de tráfego é configurado para usar o [método de roteamento de failover](traffic-manager-configure-failover-load-balancing.md).

> [AZURE.NOTE] [Azure traffic manager](traffic-manager-overview.md) é usado em todo este artigo apenas para fins ilustrativos. Você pode usar qualquer solução de balanceamento de carga que dê suporte ao método de roteamento de failover.
 
Além das instâncias do aplicativo principal, você deve considerar a implantação de um pequeno [aplicativo de função de trabalho](cloud-services-choose-me.md#tellmecs) que monitora o banco de dados primário emitindo comandos RO (somente leitura) periódicos do T-SQL. Você pode usá-lo para disparar automaticamente o failover, para gerar um alerta no console de administração do aplicativo ou ambos. Para garantir que a monitoração não seja afetada por paralisações em toda a região, você deve implantar as instâncias do aplicativo de monitoramento para cada região e conectá-las ao banco de dados em outra região, mas apenas a instância na região secundária precisa estar ativa.

> [AZURE.NOTE]Se estiver usando a [replicação geográfica](https://msdn.microsoft.com/library/azure/dn741339.aspx), você poderá ter aplicativos de monitoramento ativos e sondar bancos de dados primários e secundários. Essa última opção pode ser usada para detectar uma falha na região secundária e alertar quando o aplicativo não estiver protegido.

O diagrama a seguir mostra essa configuração antes de uma interrupção.

![A figura 1](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Após uma interrupção na região primária, o aplicativo de monitoramento detecta que o banco de dados primário não está acessível e registra um alerta. Dependendo do SLA do aplicativo, você pode decidir quantas sondagens de monitoramento consecutivas devem falhar antes de ser declarada uma interrupção do banco de dados. Para obter um failover coordenado do ponto de extremidade do aplicativo e do banco de dados, o aplicativo de monitoramento deve executar as seguintes etapas:

1. [atualizar o status de ponto de extremidade primário](https://msdn.microsoft.com/library/hh758250.aspx) para disparar o failover do ponto de extremidade.
2. chamar o banco de dados secundário [iniciar o failover de banco de dados](https://msdn.microsoft.com/library/azure/dn509573.aspx) 

Após o failover, o aplicativo processará as solicitações do usuário na região secundária, mas permanecerá colocalizado com o banco de dados porque o banco de dados primário agora estará na região secundária. Isso é ilustrado pelo diagrama a seguir. Em todos os diagramas, linhas sólidas indicam conexões ativas, linhas pontilhadas indicam conexões suspensas e pontos indicam gatilhos de ação.


![Figura 2](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Se ocorrer uma interrupção na região secundária, o link de replicação entre os bancos de dados primário e secundário será suspenso, e o aplicativo de monitoramento registrará um alerta de que o banco de dados primário está exposto. Isso não afetará o desempenho do aplicativo, mas ele funcionará exposto e, assim, correrá um risco mais elevado, caso ambas as regiões falhem em sucessão.

> [AZURE.NOTE]Só recomendamos configurações de implantação com uma única região de DR. Isso ocorre porque a maioria das regiões geográficas do Azure tem duas regiões. Essas configurações não protegerão seu aplicativo contra uma falha catastrófica de ambas as regiões. Caso ocorra essa falha improvável, você poderá recuperar seus bancos de dados em uma terceira região usando a [operação de restauração geográfica](sql-database-disaster-recovery.md#recovery-using-geo-restore).
 
Depois que a interrupção for atenuada, o banco de dados secundário será sincronizado automaticamente com o primário. Durante a sincronização, o desempenho do primário poderá ser ligeiramente afetado, dependendo da quantidade de dados que precisam ser sincronizados. O diagrama a seguir ilustra uma interrupção na região secundária.

![A figura 3](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


As principais **vantagens** desse padrão de design são:

+ a cadeia de conexão SQL é definida durante a implantação do aplicativo em cada região e não muda depois do failover
+ o desempenho do aplicativo não é afetado pelo failover, pois o aplicativo e o banco de dados sempre estão colocalizados 

A principal **desvantagem** é que a instância do aplicativo redundante na região secundária é usada apenas para recuperação de desastres.

## Padrão de design 2: implantação ativa-ativa para balanceamento de carga de aplicativo
Essa opção é mais adequada para aplicativos com as seguintes características:

+ alta taxa de leituras de banco de dados em relação às gravações
+ a latência de gravação do banco de dados não afeta a experiência do usuário final  
+ a lógica somente leitura pode ser separada da lógica de leitura/gravação usando uma cadeia de conexão diferente 
+ a lógica somente leitura não requer que os dados sejam totalmente sincronizados com as últimas atualizações  

Se seus aplicativos tiverem essas características, o balanceamento de carga de conexões de usuário final em várias instâncias do aplicativo em regiões diferentes poderá melhorar o desempenho e a experiência do usuário final. Para conseguir isso, cada região deve ter uma instância ativa do aplicativo com a lógica de RW (leitura/gravação) conectada ao banco de dados primário na região primária. A lógica RO (somente leitura) deve ser conectada a um banco de dados secundário na mesma região que a instância do aplicativo. O gerenciador de tráfego deve ser configurado para usar o [roteamento round robin](traffic-manager-configure-round-robin-load-balancing.md) ou o [roteamento de desempenho](traffic-manager-configure-performance-load-balancing.md) com o [monitoramento de ponto de extremidade](traffic-manager-monitoring.md) habilitado para cada instância do aplicativo.

Como no padrão 1, considere a implantação de um aplicativo de monitoramento semelhante. Porém, diferentemente do padrão 1, ele não será responsável por disparar o failover de ponto de extremidade.

> [AZURE.NOTE]Embora esse padrão use mais de um banco de dados secundário, apenas um dos secundários seria usado para failover, pelos motivos observados anteriormente. Como esse padrão requer acesso somente leitura ao secundário, ele requer a [Replicação geográfica ativa](https://msdn.microsoft.com/library/azure/dn741339.aspx).

O gerenciador de tráfego deve ser configurado para roteamento de desempenho para direcionar as conexões de usuário à instância do aplicativo mais próxima da localização geográfica do usuário. O diagrama a seguir ilustra essa configuração antes de uma interrupção. ![Figura 4](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Se for detectada uma falha de banco de dados na região primária, você iniciará o failover do banco de dados primário para uma das regiões secundárias, que alterará o local do banco de dados primário. O gerenciador de tráfego excluirá automaticamente o ponto de extremidade offline da tabela de roteamento, mas continuará a rotear o tráfego de usuário final para as instâncias online restantes. Como o banco de dados primário está em uma região diferente, todas as instâncias online devem alterar sua cadeia de conexão SQL de leitura/gravação para se conectar ao novo primário. É importante que você faça essa alteração antes de iniciar o failover de banco de dados. As cadeias de conexão SQL somente leitura devem permanecer inalteradas, pois sempre apontam para o banco de dados na mesma região. As etapas de failover são:

1. alterar as cadeias de conexão SQL de leitura/gravação para apontar para o novo primário
2. chamar o banco de dados secundário designado para [iniciar o failover de banco de dados](https://msdn.microsoft.com/library/azure/dn509573.aspx) 

O diagrama a seguir ilustra a nova configuração após o failover. ![Figura 5](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

No caso de uma interrupção em uma das regiões secundários, o gerenciador de tráfego removerá automaticamente da tabela de roteamento o ponto de extremidade offline nessa região. O canal de replicação para o banco de dados secundário nessa região será suspenso. Como as demais regiões receberão tráfego de usuário adicional, o desempenho do aplicativo poderá ser afetado durante a interrupção. Depois que a interrupção for atenuada, o banco de dados secundário na região afetada será imediatamente sincronizado com o primário. Durante a sincronização, o desempenho do primário poderá ser ligeiramente afetado, dependendo da quantidade de dados que precisam ser sincronizados. O diagrama a seguir ilustra uma interrupção em uma das regiões secundárias.

![Figura 6](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

A principal **vantagem** desse padrão de design é que você pode dimensionar a carga de trabalho do aplicativo em vários secundários para obter o desempenho ideal para o usuário final. As **desvantagens** dessa opção são:

+ conexões de leitura e gravação entre as instâncias do aplicativo e o banco de dados tem latência e custos variáveis
+ o desempenho do aplicativo é afetado durante a interrupção
+ as instâncias do aplicativo precisam alterar dinamicamente a cadeia de conexão SQL após o failover de banco de dados.  

> [AZURE.NOTE]Uma abordagem semelhante pode ser usada para descarregar cargas de trabalho especializadas, como trabalhos de geração de relatórios, ferramentas de business intelligence ou backups. Normalmente, essas cargas de trabalho consomem recursos significativos do banco de dados. Portanto, é recomendável que você designe um dos bancos de dados secundários para elas com o nível de desempenho correspondente à carga de trabalho antecipada.

## Padrão de design 3: implantação ativa-passiva para preservação dos dados  
Essa opção é mais adequada para aplicativos com as seguintes características:

+ qualquer perda de dados é um alto risco de negócios; o failover de banco de dados só poderá ser usado como um último recurso, se a interrupção for permanente
+ o aplicativo pode operar em "modo somente leitura" por um período de tempo 

Nesse padrão, o aplicativo alterna para o modo somente leitura quando conectado ao banco de dados secundário. A lógica do aplicativo na região primária está localizada no mesmo banco de dados primário e opera em modo de RW (leitura/gravação); a lógica do aplicativo na região secundária está colocalizada com o mesmo banco de dados secundário e está pronta para operar no modo RO (somente leitura). O gerenciador de tráfego deve ser configurado para usar o [roteamento de failover](traffic-manager-configure-failover-load-balancing.md) com o [monitoramento do ponto de extremidade](traffic-manager-monitoring.md) habilitado para ambas as instâncias do aplicativo.

O diagrama a seguir ilustra essa configuração antes de uma interrupção. ![Figura 7](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Quando o gerenciador de tráfego detecta uma falha de conectividade para a região primária, ele alterna automaticamente o tráfego do usuário para a instância do aplicativo na região secundária. Com esse padrão, é importante que você **não** inicie o failover de banco de dados após a interrupção ser detectada. O aplicativo na região secundária é ativado e opera em modo somente leitura usando o banco de dados secundário. Isso é ilustrado pelo diagrama a seguir.

![Figura 8](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Depois que a interrupção na região primária for atenuada, o gerenciador de tráfego detectará a restauração da conectividade na região primária e alternará o tráfego do usuário de volta para a instância do aplicativo na região primária. Essa instância de aplicativo será retomada e operará no modo de leitura/gravação usando o banco de dados primário.

> [AZURE.NOTE]Como esse padrão requer acesso somente leitura ao secundário, ele requer a [Replicação geográfica ativa](https://msdn.microsoft.com/library/azure/dn741339.aspx).

No caso de uma interrupção na região secundária, o gerenciador de tráfego marcará o ponto de extremidade do aplicativo na região primária como degradado, e o canal de replicação será suspenso. No entanto, isso não afetará o desempenho do aplicativo durante a interrupção. Depois que a interrupção for atenuada, o banco de dados secundário será sincronizado imediatamente com o primário. Durante a sincronização, o desempenho do primário poderá ser ligeiramente afetado, dependendo da quantidade de dados que precisam ser sincronizados.

![Figura 8](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Esse padrão de design apresenta várias **vantagens**:

+ ele evita a perda de dados durante as interrupções temporárias
+ ele não exige que você implante um aplicativo de monitoramento, pois a recuperação é disparada pelo gerenciador de tráfego 
+ o tempo de inatividade depende somente da velocidade com a qual o gerenciador de tráfego detecta a falha de conectividade, o que pode ser configurado.

As **desvantagens** são:

+ o aplicativo deve ser capaz de operar no modo somente leitura
+ é necessário alternar dinamicamente para ele quando ele é conectado a um banco de dados somente leitura
+ a retomada das operações de leitura/gravação exige a recuperação da região primária, o que significa que o acesso a dados completo pode ser desabilitado por horas ou até mesmo dias. 

> [AZURE.NOTE]No caso de uma interrupção de serviço permanente na região, você precisará ativar manualmente o failover de banco de dados e aceitar a perda de dados. O aplicativo será funcional na região secundária com acesso de leitura e gravação ao banco de dados.

## Resumo

A estratégia de DR específica pode combinar ou estender esses padrões para atender da melhor forma às necessidades do aplicativo. Como mencionado anteriormente, a estratégia escolhida será baseada no SLA que você deseja oferecer a seus clientes e na topologia de implantação do aplicativo. Para ajudar a orientar sua decisão, a tabela a seguir compara as opções com base na perda de dados estimada ou no RPO (objetivo de ponto de recuperação) e no ERT (tempo de recuperação estimado\_.

| Padrão | RPO | ERT 
| :--- |:--- | :--- 
| Implantação ativa-passiva para recuperação de desastres com acesso de banco de dados colocalizado | Acesso de leitura/gravação < 5 s | Tempo de detecção de falha + chamada à API de failover + teste de verificação de aplicativo 
| Implantação ativa-ativa para balanceamento de carga de aplicativo | Acesso de leitura/gravação < 5 s | Tempo de detecção de falha + chamada à API de failover + alteração da cadeia de conexão SQL + teste de verificação de aplicativo
| Implantação ativa-passiva para preservação de dados | Acesso somente leitura < 5 s Acesso de leitura/gravação = zero | Acesso somente leitura = tempo de detecção de falha de conectividade + teste de verificação de aplicativo <br>Acesso de leitura/gravação = tempo para atenuar a interrupção 

<!---HONumber=Nov15_HO1-->