<properties 
   pageTitle="Criar um balanceador de carga voltado para a Internet no Gerenciador de Recursos usando um modelo | Microsoft Azure"
   description="Saiba como criar um balanceador de carga para a Internet no Gerenciador de Recursos usando um modelo ARM"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/18/2015"
   ms.author="joaoma" />

# Introdução à criação de um balanceador de carga para a Internet usando um modelo ARM

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Este artigo aborda o modelo de implantação do Gerenciador de Recursos. Também é possível [Saber como criar um balanceador de carga para a Internet usando o modelo de implantação clássica](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## Implantar o modelo ARM usando clique para implantar

O modelo de exemplo disponível no repositório público usa um arquivo de parâmetro que contém os valores padrão usados para gerar o cenário descrito acima. Para implantar esse modelo usando a opção de clique para implantar, acesse [este link](http://go.microsoft.com/fwlink/?LinkId=544801), clique em **Implantar no Azure**, substitua os valores de parâmetro padrão, se necessário, e siga as instruções no portal.

## Implantar o modelo ARM usando o PowerShell

Para implantar o modelo ARM baixado usando o PowerShell, siga as etapas abaixo.

1. Se você nunca usou o Azure PowerShell, consulte [Como Instalar e Configurar o Azure PowerShell](powershell-install-configure.md) e siga as instruções até o fim para entrar no Azure e selecionar sua assinatura.

2. Execute o cmdlet **New-AzureRmResourceGroup** para criar um grupo de recursos usando o modelo.

		New-AzureRmResourceGroup -Name TestRG -Location uswest `
		    -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
		    -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'	

## Implantar o modelo ARM usando a CLI do Azure

Para implantar o modelo ARM usando a CLI do Azure, siga as etapas abaixo.

1. Se você nunca usou a CLI do Azure, confira [Instalar e configurar a CLI do Azure](xplat-cli.md) e siga as instruções até o ponto em que você seleciona a conta e a assinatura do Azure.
2. Execute o comando **azure config mode** para alternar para o modo Gerenciador de Recursos, como mostrado abaixo.

		azure config mode arm

	Este é o resultado esperado para o comando descrito acima:

		info:    New mode is arm

3. No seu navegador, navegue até ****https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json**, copie o conteúdo do arquivo json e cole em um novo arquivo em seu computador. Para este cenário, copie os valores abaixo em um arquivo chamado **c:\\lb\\azuredeploy.parameters.json**.
4. Execute o cmdlet **azure group deployment create** para implantar o novo balanceador de carga usando o modelo e os arquivos de parâmetro que você baixou e modificou acima. A lista exibida após a saída explicar os parâmetros usados.

		azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## Próximas etapas

[Introdução à configuração de um balanceador de carga interno](load-balancer-internal-getstarted.md)

[Configurar um modo de distribuição do balanceador de carga](load-balancer-distribution-mode.md)

[Definir configurações de tempo limite de TCP ocioso para o balanceador de carga](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_1223_2015-->