
<properties
	pageTitle="Solução de problemas de associação dinâmica para grupos| Microsoft Azure"
	description="Um tópico que lista dicas de solução de problemas para a associação dinâmica para grupos no AD do Azure."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""
	/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="11/17/2015"
	ms.author="curtand"/>


# Solução de problemas de associações dinâmicas para grupos

| Sintoma | Ação |
|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurei uma regra em um grupo, mas nenhuma associação é atualizada no grupo | Verifique se a configuração **Habilitar gerenciamento de grupos delegados** está definida como Sim na guia Configurar Diretório. Você verá essa configuração somente se estiver conectado como um usuário que recebe uma licença do Azure Active Directory Premium. Verifique os valores dos atributos de usuário na regra: há usuários que atendem à regra? |
| Configurei uma regra, mas agora os membros existentes da regra foram removidos | Esse comportamento é esperado. Membros existentes do grupo são removidos quando uma regra é habilitada ou alterada. Os usuários retornados da avaliação da regra são adicionados como membros ao grupo. |
| Não vejo as alterações de associação instantaneamente quando adiciono ou altero uma regra; por quê? | A avaliação de associação dedicada é realizada periodicamente em um processo assíncrono em segundo plano. O tempo que o processo leva é determinado pelo número de usuários no diretório e pelo tamanho do grupo criado como resultado da regra. Normalmente, os diretórios com um pequeno número de usuários verão as alterações de associação de grupo em poucos minutos. Os diretórios com um grande número de usuários podem levar até 30 minutos ou mais para serem populados. |

Esses artigos fornecem mais informações sobre o Active Directory do Azure.

* [Gerenciamento de acesso a recursos com grupos do Active Directory do Azure](active-directory-manage-groups.md)
* [O que é o Active Directory do Azure?](active-directory-whatis.md)
* [Integração das identidades locais com o Active Directory do Azure](active-directory-aadconnect.md)

<!---HONumber=Nov15_HO4-->