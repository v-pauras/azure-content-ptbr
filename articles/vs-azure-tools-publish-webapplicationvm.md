<properties
   pageTitle="Publicar WebApplicationVM | Microsoft Azure"
   description="Saiba como implantar um aplicativo Web para uma máquina virtual. Se os recursos necessários não existirem, este script criará tais recursos em sua assinatura do Azure."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="09/30/2015"
   ms.author="tarcher" />

# Publish-WebApplicationVM (script do Windows PowerShell)

Implanta um aplicativo Web em uma máquina virtual. Se os recursos necessários não existirem, o script criará tais recursos em sua assinatura do Azure.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### Configuração

O caminho para o arquivo de configuração de JSON que descreve os detalhes da implantação.

|Aliases|nenhum|
|---|---|
|Obrigatório?|verdadeiro|
|Position|nomeado|
|Valor padrão|nenhum|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

### SubscriptionName

O nome da assinatura do Azure no qual você deseja criar a máquina virtual.

|Aliases|nenhum|
|---|---|
|Obrigatório?|false|
|Position|nomeado|
|Valor padrão|Usa a primeira assinatura no arquivo de assinatura|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

### WebDeployPackage

O caminho para o pacote de implantação da web para publicar na máquina virtual. Você pode criar este pacote usando o assistente Publicar Web no Visual Studio. Consulte [Como: criar um pacote de implantação da Web no Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliases|nenhum|
|---|---|
|Obrigatório?|false|
|Position|nomeado|
|Valor padrão|nenhum|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

### AllowUntrusted

Se for verdadeiro, permite o uso de certificados que não está assinado por uma autoridade raiz confiável.

|Aliases|nenhum|
|---|---|
|Obrigatório?|false|
|Position|nomeado|
|Valor padrão|false|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

### VMPassword

As credenciais da conta de máquina virtual. Exemplo: -VMPassword @{Name = "admin"; Password = "password"}

|Aliases|nenhum|
|---|---|
|Obrigatório?|false|
|Position|nomeado|
|Valor padrão|nenhum|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

### DatabaseServerPassword

As credenciais do banco de dados SQL no Azure. Exemplo:-DatabaseServerPassword @{Name = "admin"; Password = "password"}

|Aliases|nenhum|
|---|---|
|Obrigatório?|false|
|Position|nomeado|
|Valor padrão|nenhum|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

### SendHostMessagesToOutput

Se seu valor for true, imprimir mensagens do script para o fluxo de saída.

|Aliases|nenhum|
|---|---|
|Obrigatório?|false|
|Position|nomeado|
|Valor padrão|false|
|Aceitar entrada do Pipeline?|false|
|Aceitar caracteres curinga?|false|

## Comentários

Para obter uma explicação completa de como usar o script para criar ambientes de desenvolvimento e teste, consulte [usando Scripts do Windows PowerShell para publicar em ambientes de teste e desenvolvimento](vs-azure-tools-publishing-using-powershell-scripts.md).

O arquivo de configuração JSON especifica os detalhes daquilo que está para ser implantado. Ele inclui as informações que você especificou quando criou o projeto, como o nome, grupo de afinidades, imagem de VHD e tamanho da máquina virtual. Também inclui os pontos de extremidade na máquina virtual, os bancos de dados a provisionar, se houver, e os parâmetros de implantação de web. O código a seguir mostra um exemplo de arquivo de configuração JSON:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Você pode editar o arquivo de configuração do JSON para alterar o que é provisionado. Uma máquina virtual e um serviço de nuvem são necessários, mas a seção de banco de dados é opcional.

<!---HONumber=AcomDC_1217_2015-->