<properties
   pageTitle="Exemplos de configuração de roteador de cliente Rota Expressa | Microsoft Azure"
   description="Esta página fornece exemplos de configuração do roteador para os roteadores da série Cisco ASA e Juniper."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/12/2015"
   ms.author="cherylmc"/>

# Exemplos de configuração do roteador para configurar e gerenciar o NAT

Esta página fornece exemplos de configuração do NAT para roteadores da série Cisco ASA e Juniper MX. Devem ser exemplos para obter orientação apenas e não devem ser usados como estão. Você pode trabalhar com o fornecedor para exibir as configurações apropriadas para sua rede.

>[AZURE.IMPORTANT]Exemplos nesta página devem ser exclusivamente para obter orientação. Trabalhe com a equipe de vendas / equipe técnica e sua equipe de rede para exibir as configurações adequadas para atendar às suas necessidades. A Microsoft não oferecerá suporte a problemas relacionados a configurações listadas nesta página. Você deve entrar em contato com o fornecedor do dispositivo para problemas de suporte.

Os exemplos de configuração de roteador abaixo se aplicam a emparelhamentos do Azure Public e Microsoft. Você não deve configurar o NAT para emparelhamento privado do Azure. Examine[emparelhamentos Rota Expressa](expressroute-circuit-peerings.md) e [requisitos de NAT Rota Expressa](expressroute-nat.md) para obter mais detalhes.

**Observação:** você DEVE usar pools de IP de NAT separados para conectividade à Internet e Rota Expressa. Usando o mesmo pool de IP de NAT através da Internet e Rota Expressa vai resultar em roteamento assimétrico e perda de conectividade.

## Firewalls do Cisco ASA

### Configuração de PAT para o tráfego de rede de cliente para a Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### Configuração PAT do tráfego da Microsoft para a rede do cliente

#### Interfaces e Direção:
	Source Interface (where the traffic enters the ASA): inside
	Destination Interface (where the traffic exits the ASA): outside

#### Configuração:
Pool de NAT:

	object network outbound-PAT
		host <NAT-IP>

Servidor de destino:

	object network Customer-Network
		network-object <IP> <Subnet-Mask>

Grupo de objetos para endereços IP do cliente

	object-group network MSFT-Network-1
		network-object <MSFT-IP> <Subnet-Mask>
	
	object-group network MSFT-PAT-Networks
		network-object object MSFT-Network-1

Comandos de NAT

	nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## Roteadores da série Juniper MX 

### 1\. Criar interfaces de Ethernet redundantes para o cluster

	interfaces {
	    reth0 {
	        description "To Internal Network";
	        vlan-tagging;
	        redundant-ether-options {
	            redundancy-group 1;
	        }
	        unit 100 {
	            vlan-id 100;
	            family inet {
	                address <IP-Address/Subnet-mask>;
	            }
	        }
	    }
	    reth1 {
	        description "To Microsoft via Edge Router";
	        vlan-tagging;
	        redundant-ether-options {
	            redundancy-group 2;
	        }
	        unit 100 {
	            description "To Microsoft via Edge Router";
	            vlan-id 100;
	            family inet {
	                address <IP-Address/Subnet-mask>;
	            }
	        }
	    }
	}


### 2\. Criar duas zonas de segurança

 - A Zona de Confiança para a rede interna e Zona Não Confiável para rede externa de frente para os Roteadores de Extremidade
 - Atribuir as interfaces apropriadas para as zonas
 - Permitir os serviços nas interfaces


	security { zones { security-zone Trust { host-inbound-traffic { system-services { ping; } protocols { bgp; } } interfaces { reth0.100; } } security-zone Untrust { host-inbound-traffic { system-services { ping; } protocols { bgp; } } interfaces { reth1.100; } } } }


### 3\. Criar políticas de segurança entre zonas
 
	security {
	    policies {
	        from-zone Trust to-zone Untrust {
	            policy allow-any {
	                match {
	                    source-address any;
	                    destination-address any;
	                    application any;
	                }
	                then {
	                    permit;
	                }
	            }
	        }
	        from-zone Untrust to-zone Trust {
	            policy allow-any {
	                match {
	                    source-address any;
	                    destination-address any;
	                    application any;
	                }
	                then {
	                    permit;
	                }
	            }
	        }
	    }
	}


### 4\. Configurar políticas NAT
 - Criar dois pools NAT. Um será usado para a saída de tráfego NAT para a Microsoft e outro da Microsoft para o cliente.
 - Criar o respectivo tráfego de regras de NAT

		security {
		    nat {
		        source {
		            pool SNAT-To-ExpressRoute {
		                routing-instance {
		                    External-ExpressRoute;
		                }
		                address {
		                    <NAT-IP-address/Subnet-mask>;
		                }
		            }
		            pool SNAT-From-ExpressRoute {
		                routing-instance {
		                    Internal;
		                }
		                address {
		                    <NAT-IP-address/Subnet-mask>;
		                }
		            }
		            rule-set Outbound_NAT {
		                from routing-instance Internal;
		                to routing-instance External-ExpressRoute;
		                rule SNAT-Out {
		                    match {
		                        source-address 0.0.0.0/0;
		                    }
		                    then {
		                        source-nat {
		                            pool {
		                                SNAT-To-ExpressRoute;
		                            }
		                        }
		                    }
		                }
		            }
		            rule-set Inbound-NAT {
		                from routing-instance External-ExpressRoute;
		                to routing-instance Internal;
		                rule SNAT-In {
		                    match {
		                        source-address 0.0.0.0/0;
		                    }
		                    then {
		                        source-nat {
		                            pool {
		                                SNAT-From-ExpressRoute;
		                            }
		                        }
		                    }
		                }
		            }
		        }
		    }
		}


### 5\. Configurar o BGP para anunciar prefixos seletivos em cada direção

Veja os exemplos na página [Exemplos de configuração de roteamento](expressroute-config-samples-routing.md).

### 6\. Criar políticas

	routing-options {
	    	      autonomous-system <Customer-ASN>;
	}
	policy-options {
	    prefix-list Microsoft-Prefixes {
	        <IP-Address/Subnet-Mask;
	        <IP-Address/Subnet-Mask;
	    }
	    prefix-list private-ranges {
	        10.0.0.0/8;
	        172.16.0.0/12;
	        192.168.0.0/16;
	        100.64.0.0/10;
	    }
	    policy-statement Advertise-NAT-Pools {
	        from {
	            protocol static;
	            route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
	        }
	        then accept;
	    }
	    policy-statement Accept-from-Microsoft {
	        term 1 {
	            from {
	                instance External-ExpressRoute;
	                prefix-list-filter Microsoft-Prefixes orlonger;
	            }
	            then accept;
	        }
	        term deny {
	            then reject;
	        }
	    }
	    policy-statement Accept-from-Internal {
	        term no-private {
	            from {
	                instance Internal;
	                prefix-list-filter private-ranges orlonger;
	            }
	            then reject;
	        }
	        term bgp {
	            from {
	                instance Internal;
	                protocol bgp;
	            }
	            then accept;
	        }
	        term deny {
	            then reject;
	        }
	    }
	}
	routing-instances {
	    Internal {
	        instance-type virtual-router;
	        interface reth0.100;
	        routing-options {
	            static {
	                route <NAT-Pool-IP-Address/Subnet-mask> discard;
	            }
	            instance-import Accept-from-Microsoft;
	        }
	        protocols {
	            bgp {
	                group customer {
	                    export <Advertise-NAT-Pools>;
	                    peer-as <Customer-ASN-1>;
	                    neighbor <BGP-Neighbor-IP-Address>;
	                }
	            }
	        }
	    }
	    External-ExpressRoute {
	        instance-type virtual-router;
	        interface reth1.100;
	        routing-options {
	            static {
	                route <NAT-Pool-IP-Address/Subnet-mask> discard;
	            }
	            instance-import Accept-from-Internal;
	        }
	        protocols {
	            bgp {
	                group edge-router {
	                    export <Advertise-NAT-Pools>;
	                    peer-as <Customer-Public-ASN>;
	                    neighbor <BGP-Neighbor-IP-Address>;
	                }
	            }
	        }
	    }
	}

## Próximas etapas

Consulte as [Perguntas Frequentes sobre Rota Expressa](expressroute-faqs.md) para obter mais detalhes.

<!---HONumber=Nov15_HO1-->