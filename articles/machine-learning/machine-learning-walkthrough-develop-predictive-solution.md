<properties
	pageTitle="Uma solução preditiva para o risco de crédito com o Aprendizado de Máquina | Microsoft Azure"
	description="Um passo a passo detalhado mostrando como criar uma solução de análise preditiva para avaliação de risco de crédito no Estúdio de Aprendizado de Máquina do Azure."
	keywords="credit risk, predictive analytics solution,risk assessment"
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="10/13/2015"
	ms.author="garye"/>


# Passo a passo: Desenvolver uma solução de análise preditiva para avaliação de risco de crédito no Aprendizado de Máquina do Azure

Suponha que você precise prever o risco de crédito de uma pessoa com base nas informações dadas em um aplicativo de crédito.

A avaliação de risco de crédito é um problema complexo, mas vamos simplificar um pouco os parâmetros da pergunta. Em seguida, podemos usá-lo como um exemplo de como você pode usar o Aprendizado de Máquina do Microsoft Azure com o Estúdio de Aprendizado de Máquina e o serviço Web de Aprendizado de Máquina para criar essa solução de análise preditiva.

Neste passo a passo detalhado, seguiremos o processo de desenvolvimento de um modelo de análise preditiva no Estúdio de Aprendizado de Máquina e, em seguida, implantá-lo como um serviço Web do Aprendizado de Máquina do Azure. Começaremos com os dados de risco de crédito disponíveis publicamente, desenvolveremos e treinaremos um modelo preditivo baseado nesses dados e implantaremos o modelo como um serviço Web que pode ser usado por outros para a avaliação de risco de crédito.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Abra o Estúdio de Aprendizado de Máquina: [https://studio.azureml.net/Home](https://studio.azureml.net/Home). Para obter mais informações sobre os primeiros passos no Estúdio de Aprendizado de Máquina, consulte [Página Inicial do Estúdio de Aprendizado de Máquina do Microsoft Azure](https://studio.azureml.net/).

>[AZURE.TIP]Para baixar e imprimir um diagrama que fornece uma visão geral dos recursos do Estúdio de Aprendizado de Máquina, consulte [Diagrama de visão geral dos recursos do Estúdio de Aprendizado de Máquina do Azure](machine-learning-studio-overview-diagram.md).

Para criar uma solução de avaliação de risco de crédito, vamos acompanhar estas etapas:

1.	[Criar um espaço de trabalho do Aprendizado de Máquina](machine-learning-walkthrough-1-create-ml-workspace.md)
2.	[Carregar dados existentes](machine-learning-walkthrough-2-upload-data.md)
3.	[Criar um novo teste](machine-learning-walkthrough-3-create-new-experiment.md)
4.	[Treinar e avaliar os modelos](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.	[Implantar o serviço Web](machine-learning-walkthrough-5-publish-web-service.md)
6.	[Acessar o serviço Web](machine-learning-walkthrough-6-access-web-service.md)

Este passo a passo é baseado em uma versão simplificada do [Teste de exemplo de previsão de risco de crédito](../machine-learning-sample-credit-risk-prediction.md) incluído no Estúdio de Aprendizado de Máquina.

<!---HONumber=Oct15_HO3-->