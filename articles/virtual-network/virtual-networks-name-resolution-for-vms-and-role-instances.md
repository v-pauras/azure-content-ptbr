<properties 
   pageTitle="Resolução para máquinas virtuais e instâncias de função"
   description="Cenários de resolução de nomes para o Azure IaaS, soluções híbridas, entre diferentes serviços de nuvem, Active Directory e usando o seu próprio servidor DNS"
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="jdial"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2015"
   ms.author="joaoma" />

# Resolução de nomes de máquinas virtuais e instâncias de função

Dependendo de como você usar o Azure para hospedar o IaaS, o PaaS e as soluções híbridas, pode ser necessário permitir que as VMs e as instâncias de função criadas se comuniquem entre si. Embora essa comunicação possa ser feita usando endereços IP, é muito mais simples usar nomes que possam ser facilmente lembrados e que não sejam alterados.

Quando as instâncias de função e as VMs hospedadas no Azure precisam resolver nomes de domínio para endereços IP internos, elas podem usar um dos dois métodos:

- [Resolução de nomes fornecida pelo Azure](#azure-provided-name-resolution)

- [Resolução de nome usando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server)

O tipo de resolução de nomes usado depende de como as VMs e as instâncias de função precisam se comunicar entre si.

**A tabela a seguir ilustra os cenários e as soluções de resolução de nome correspondentes:**

| **Cenário** | **Resolução de nomes fornecida por:** | **Para obter mais informações, consulte:** |
|--------------|----------------------------------|-------------------------------|
| Resolução de nomes entre as instâncias de função ou as máquinas virtuais no mesmo serviço de nuvem | Resolução de nomes fornecida pelo Azure | [Resolução de nomes fornecida pelo Azure](#azure-provided-name-resolution)|
| Resolução de nomes entre máquinas virtuais e instâncias de função localizadas na mesma rede virtual | Resolução de nomes fornecida pelo Azure (somente implantações baseadas em ARM) ou Resolução de nomes usando o seu próprio servidor DNS | [Resolução de nomes fornecida pelo Azure](#azure-provided-name-resolution), [Resolução de nomes usando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server) e [requisitos do servidor DNS](#dns-server-requirements) |
| Resolução de nomes entre máquinas virtuais e instâncias de função localizadas em redes virtuais diferentes | Resolução de nome usando o seu próprio servidor DNS| [Resolução de nomes usando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server) e [requisitos do servidor DNS](#dns-server-requirements)|
| Entre instalações: Resolução de nomes entre instâncias de função ou máquinas virtuais no Azure e em computadores locais| Resolução de nome usando o seu próprio servidor DNS| [Resolução de nomes usando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server) e [requisitos do servidor DNS](#dns-server-requirements)|
| Pesquisa inversa de IPs internos| Resolução de nome usando o seu próprio servidor DNS| [Resolução de nomes usando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server) e [requisitos do servidor DNS](#dns-server-requirements)|
| Resolução de nomes para domínios não públicos (por exemplo, domínios do Active Directory)| Resolução de nome usando o seu próprio servidor DNS| [Resolução de nomes usando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server) e [requisitos do servidor DNS](#dns-server-requirements)|
| Resolução de nomes entre instâncias de função localizadas em serviços de nuvem diferente, não em uma rede virtual| Não aplicável. Não há suporte para a conectividade entre máquinas virtuais e instâncias de função em diferentes serviços de nuvem fora de uma rede virtual.| Não aplicável.|


## Resolução de nomes fornecida pelo Azure

Junto com a resolução de nomes DNS públicos, o Azure fornece uma resolução de nomes interna para VMs e instâncias de função que residem na mesma rede virtual ou serviço de nuvem. Como as VMs/instâncias em um serviço de nuvem compartilham o mesmo sufixo DNS, apenas o nome do host é suficiente. Em redes virtuais clássicas, diferentes serviços de nuvem contêm diferentes sufixos DNS, por isso o FQDN é necessário. Em redes virtuais baseadas em ARM, o sufixo DNS é comum em toda a rede virtual, por isso o FQDN não é necessário e o nome DNS pode ser atribuído à NIC ou à máquina virtual. Embora a resolução de nomes fornecida pelo Azure não requeira qualquer configuração, ela não é a escolha apropriada para todos os cenários de implantação, como mostrado na tabela acima.

> [AZURE.NOTE]No caso das funções web e de trabalho, você também pode acessar os endereços IP internos das instâncias de função, conforme o número de instâncias e o nome da função usando a API REST do Gerenciamento de Serviços do Azure. Para obter mais informações, veja [Referência da API REST de Gerenciamento de Serviços](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### Recursos e considerações

**Recursos:**

- Facilidade de uso: não é necessária nenhuma configuração para usar a resolução de nomes fornecida pelo Azure.

- O serviço de resolução de nomes fornecido pelo Azure é altamente disponível, economizando a necessidade de criar e gerenciar clusters de seus próprios servidores DNS.

- A resolução de nomes é fornecida entre as instâncias de função ou as VMs no mesmo serviço de nuvem, sem a necessidade de um FQDN.

- A resolução de nomes é fornecida entre as VMs em redes virtuais baseadas em ARM, sem a necessidade do FQDN; no entanto, as redes vrituais clássicas exigem o FQDN durante a resolução de nomes em diferentes serviços de nuvem.

- Você pode criar os nomes de host que melhor descreverão as suas implantações, em vez de trabalhar com nomes gerados automaticamente.

**Considerações:**

- Resolução de nomes entre redes virtuais e entre computadores locais e o Azure não está disponível.

- O sufixo DNS criado pelo Azure não pode ser modificado.

- Não é possível registrar manualmente seus próprios registros.

- Não há suporte para o WINS e NetBIOS. (Você não pode listar suas máquinas virtuais no navegador de rede no Windows Explorer).

- Os nomes de host devem ser compatíveis com DNS (eles devem usar somente 0-9, a-z e '-' e não podem começar ou terminar com um '-'. Consulte o RFC 3696, seção 2).

- O tráfego de consultas DNS é limitado pela VM. Isso não deve afetar a maioria dos aplicativos. Se a limitação da solicitação for observada, certifique-se de que o armazenamento em cache do cliente está habilitado. Para obter mais detalhes, consulte [Obtendo o máximo de resolução de nomes fornecida pelo Azure](#Getting-the-most-from-Azure-provided-name-resolution).

- Apenas as VMs nos primeiros 180 serviços de nuvem primeiro são registrados para cada rede virtual clássica. Isso não se aplica às redes virtuais baseadas em ARM.


### Obtendo o máximo de resolução de nomes fornecida pelo Azure
**Armazenamento em cache no lado cliente:**

Nem todas as consultas DNS precisam ser enviada pela rede. O armazenamento em cache do lado do cliente ajuda a reduzir a latência e melhorar a resistência a pontos de luz de rede resolvendo consultas de DNS recorrentes a partir de um cache local. Os registros DNS contêm um Time-To-Live (TTL) que permite que o cache armazene o registro o maior tempo possível sem afetar a atualização de registro para que o armazenamento em cache do lado do cliente seja adequado para a maioria das situações.

O cliente DNS padrão do Windows tem um cache DNS interno. Algumas distribuições do Linux não incluem o armazenamento em cache por padrão, é recomendável que um seja adicionado a cada VM do Linux. Há uma série de pacotes de armazenamento em cache de DNS diferentes disponíveis, por exemplo, dnsmasq, aqui estão as etapas para instalar o dnsmasq nas distribuições mais comuns:

- **Ubuntu (usa resolvconf)**:
	- apenas instale o pacote de dnsmasq ("sudo apt-get install dnsmasq").
- **SUSE (usa netconf)**:
	- instale o pacote de dnsmasq ("sudo zypper install dnsmasq"). 
	- habilite o serviço dnsmasq ("systemctl enable dnsmasq.service") 
	- inicie o serviço dnsmasq ("systemctl enable dnsmasq.service") 
	- edite "/etc/sysconfig/network/config" e altere NETCONFIG\_DNS\_FORWARDER = "" para "dnsmasq"
	- atualize resolv. conf ("netconfig update") para definir o cache do resolvedor DNS local
- **OpenLogic (usa NetworkManager)**:
	- instale o pacote de dnsmasq ("sudo yum install dnsmasq").
	- habilite o serviço dnsmasq ("systemctl enable dnsmasq.service")
	- inicie o serviço dnsmasq ("systemctl enable dnsmasq.service")
	- adicione "prepend domain-name-servers 127.0.0.1;" a "/etc/dhclient-eth0.conf"
	- reinicie o serviço de rede ("service network restart") para definir o cache como o resolvedor DNS local

[AZURE.NOTE]\: O pacote 'dnsmasq' é apenas um dos vários caches DNS disponíveis para o Linux. Antes de usá-lo, verifique sua adequação para suas necessidades específicas e se nenhum outro cache está instalado.

**Tentativa no lado do cliente:**

O DNS é principalmente um protocolo UDP. Como o protocolo UDP não garante a entrega de mensagens, a lógica de repetição é manipulada no próprio protocolo DNS. Cada cliente DNS (sistema operacional) pode apresentar uma lógica de repetição diferente dependendo da preferência dos criadores:

 - Sistemas operacionais Windows tentam novamente depois de 1 segundo e, em seguida, novamente após outros 2, 4 e outros 4 segundos. 
 - As tentativas de instalação do Linux ocorrem, por padrão, depois de 5 segundos. É recomendável alterar para repetir 5 vezes em intervalos de 1 segundo.  

Para verificar as configurações atuais em uma VM do Linux, 'cat /etc/resolv.conf' e examine a linha 'options', por exemplo:

	options timeout:1 attempts:5

O arquivo resolv.conf é normalmente gerado automaticamente e não deve ser editado. As etapas específicas para adicionar a linha 'options' variam de acordo com a distribuição:

- **Ubuntu** (usa resolvconf):
	- adicione a linha de opções para '/etc/resolveconf/resolv.conf.d/head' 
	- execute 'resolvconf -u' para atualizar
- **SUSE** (usa netconf):
	- adicione 'timeout:1 tentativas: 5' ao parâmetro NETCONFIG\_DNS\_RESOLVER\_OPTIONS = "" em '/ etc/sysconfig/rede/config' 
	- execute 'netconfig update' para atualizar
- **OpenLogic** (usa NetworkManager):
	- adicione 'echo "options timeout:1 attempts:5"' em '/etc/NetworkManager/dispatcher.d/11-dhclient' 
	- execute 'service network restart' para atualizar

## Resolução de nome usando o seu próprio servidor DNS

Se os seus requisitos de resolução de nomes forem além dos recursos fornecidos pelo Azure, você tem a opção de usar seu(s) próprio(s) servidor(es) DNS. Quando você usa seu(s) próprio(s) servidor(es) DNS, você é responsável por gerenciar os registros de DNS.

> [AZURE.NOTE]É recomendável evitar o uso de um servidor DNS externo, a menos que o cenário de implantação exija.

## Requisitos do servidor DNS

Se você planeja usar a resolução de nomes que não é fornecida pelo Azure, o servidor DNS que você especificar deverá dar suporte ao seguinte:

- O servidor DNS deve aceitar o registro de DNS dinâmico por meio do protocolo de DNS Dinâmico (DDNS), ou você deve criar os registros necessários.

- Se depender do DNS dinâmico, o servidor DNS deve ter a limpeza de registro desativada. No Azure, os endereços IP têm concessões de DHCP longas, que podem resultar na remoção de registros do servidor DNS durante a limpeza.

- O servidor DNS deve estar com a recursão habilitada para permitir a resolução de nomes de domínio externo.

- O servidor DNS deve ser acessível (na TCP/UDP, porta 53) por clientes que solicitarem a resolução de nomes e pelos serviços e máquinas virtuais que registrarão seus nomes.

- Também é recomendável proteger o servidor DNS contra o acesso pela Internet, pois vários robôs procuram resolvedores DNS recursivo abertos.

- Para obter um melhor desempenho, ao usar as VMs do Azure como servidores DNS, o IPv6 deve ser desabilitado e um [IP Público em Nível de Instância](virtual-networks-instance-level-public-ip.mp) deve ser atribuído a cada VM do servidor DNS.

## Especificar servidores DNS

Você pode especificar vários servidores DNS a serem usados por suas VMs e instâncias de função. Para cada consulta DNS, o cliente tentará primeiro o servidor DNS preferencial e apenas tentará os servidores alternativos se o servidor preferencial não responder, ou seja, as consultas DNS não têm balanceamento de carga entre os diferentes servidores DNS. Por esse motivo, verifique se os servidores DNS listados estão na ordem correta para o seu ambiente.

> [AZURE.NOTE]Se alterar as configurações DNS em um arquivo de configuração de rede para rede virtual que já foi implantado, você precisará reiniciar cada máquina virtual para que as alterações entrem em vigor.

### Especificando um servidor DNS no Portal de Gerenciamento

Ao criar sua rede virtual no Portal de Gerenciamento, você pode especificar o endereço IP e o nome do(s) servidor(es) DNS que deseja usar. Depois de criar a rede virtual, as máquinas virtuais e as instâncias de função implantadas na rede virtual são configuradas automaticamente com as configurações DNS especificadas. Os servidores DNS especificados para um serviço de nuvem específico (clássico do Azure) ou uma placa de interface de rede (implantações baseadas em ARM) têm precedência sobre aqueles especificados para a rede virtual. Veja [Sobre como configurar uma rede virtual no Portal de Gerenciamento](virtual-networks-settings.md).

### Especificando um servidor DNS usando arquivos de configuração (clássico do Azure)

Para redes virtuais clássicas, é possível especificar as configurações DNS usando dois arquivos de configuração diferentes: o arquivo de *Configuração de rede* e o arquivo de *Configuração de serviço*.

O arquivo de configuração de rede descreve as redes virtuais em sua assinatura. Quando você adiciona instâncias de função ou VMs a um serviço de nuvem em uma rede virtual, as configurações DNS do arquivo de configuração de rede são aplicadas a cada instância de função ou VM, a menos que servidores DNS específicos ao serviço de nuvem tenham sido especificados.

O arquivo de configuração de serviço é criado para cada serviço de nuvem adicionado ao Azure. Quando você adiciona instâncias de função ou VMs ao serviço de nuvem, as configurações DNS do arquivo de configuração de serviço são aplicadas a cada instância de função ou VM.

> [AZURE.NOTE]Os servidores DNS no arquivo de configuração de serviço substituem as configurações no arquivo de configuração de rede.


## Próximas etapas

[Esquema de configuração de serviço do Azure](https://msdn.microsoft.com/library/azure/ee758710)

[Esquema de configuração de Rede Virtual](https://msdn.microsoft.com/library/azure/jj157100)

[Sobre definir as configurações de rede virtual no Portal de Gerenciamento](virtual-networks-settings.md)

[Configurar uma rede virtual usando um arquivo de configuração de rede](virtual-networks-using-network-configuration-file.md)

<!---HONumber=Oct15_HO3-->