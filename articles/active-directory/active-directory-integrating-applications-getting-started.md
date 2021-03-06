<properties
   pageTitle="Guia de introdução: integrando o Active Directory do Azure com aplicativos | Microsoft Azure"
   description="Este artigo é um guia de introdução para a integração do AD do Azure (Active Directory do Azure) com aplicativos locais e aplicativos em nuvem."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="stevenpo"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/16/2015"
      ms.author="inhenk"/>

# Guia de introdução: integrando o Active Directory do Azure com aplicativos
## Visão geral
Este tópico destina-se a fornecer um roteiro para integrar aplicativos com o AD do Azure (Active Directory do Azure). Cada uma das seções abaixo contém um breve resumo de um tópico mais detalhado, para que você possa identificar quais partes deste guia de introdução são relevantes para você. Siga os links para se aprofundar em cada assunto.

## Antes de começar, faça o inventário
Antes de ir para a integração de aplicativos com o AD do Azure, é importante saber onde você está e onde deseja ir. As perguntas a seguir destinam-se a ajudá-lo a pensar em seu projeto de integração de aplicativos do AD do Azure.

### Inventário de aplicativos
- Onde estão todos os seus aplicativos? Quem é seu proprietário?
- Que tipo de autenticação os aplicativos exigem?
- Quem precisa de acesso a quais aplicativos?
- Você deseja implantar um novo aplicativo?
  - Ele será criado internamente e implantado em uma instância de computação do Azure?
  - Você usará uma que está disponível na Galeria de Aplicativos do Azure?

### Inventário de usuários e grupos
- Onde residem suas contas de usuário?
 - Active Directory local
 - AD do Azure
 - Em um banco de dados de aplicativo separado que você possui
 - Em aplicativos não autorizados
 - Todos os itens acima
- Quais permissões e atribuições de função os usuários individuais têm atualmente? Você precisa examinar seu acesso ou tem certeza de que o acesso do usuário e as atribuições de função são apropriadas agora?
- Os grupos já estão estabelecidos em seu Active Directory local?
 - Como os grupos são organizados?
 - Quem são os membros do grupo?
 - Quais permissões/atribuições de função os grupos têm atualmente?
- Você precisará limpar os bancos de dados de usuários/grupos antes da integração? (Essa é uma pergunta muito importante. Entrada e saída de lixo.)

### Inventário de gerenciamento de acesso
- Atualmente, como você gerencia o acesso do usuário aos aplicativos? Isso precisa ser alterado? Você considerou outras maneiras de gerenciar o acesso, como com o [RBAC](role-based-access-control-configure.md), por exemplo?
- Quem precisa de acesso ao quê?

Talvez você não tenha as respostas a todas essas perguntas com antecedência, mas tudo bem. Este guia pode ajudá-lo a responder a algumas dessas perguntas e tomar algumas decisões informadas.

## Pré-requisitos
- Uma assinatura do Azure e um diretório do Active Directory do Azure. Se você ainda não tem uma assinatura do Azure, você pode experimentar o Azure gratuitamente por 30 dias. [Experimente!](https://azure.microsoft.com/trial/get-started-active-directory/)

## Integração de aplicativos com o AD do Azure
### Encontrando aplicativos em nuvem não autorizados com o Cloud App Discovery
Como mencionado acima, pode haver aplicativos que ainda não foram gerenciados pela sua organização até agora. Como parte do processo de inventário, é possível encontrar aplicativos em nuvem não autorizados. Veja [Encontrando aplicativos em nuvem não autorizados com o Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

### Tipos de autenticação
Cada um dos seus aplicativos pode ter requisitos de autenticação diferentes. Com o AD do Azure, pode-se usar certificados de autenticação com aplicativos que usam os Protocolos SAML 2.0, WS-Federation ou OpenID Connect, bem como Logon Único com Senha. Para obter mais informações sobre os tipos de autenticação de aplicativo para uso com o AD do Azure, veja [Gerenciando certificados para Logon Único Federado no Active Directory do Azure](active-directory-sso-certs.md) e [Logon único baseado em senha](active-directory-appssoaccess-whatis.md).

### Habilitando o SSO com o Proxy de Aplicativo do AD do Azure
Com o Proxy de Aplicativo do AD do Microsoft Azure, você pode fornecer acesso a aplicativos localizados em sua rede privada com segurança, de qualquer lugar e em qualquer dispositivo. Depois de instalar um conector de proxy de aplicativo em seu ambiente, ele pode ser facilmente configurado com o AD do Azure. Veja [Habilitando o SSO com o Proxy de Aplicativo do AD do Azure](active-directory-appssoaccess-enable-hybrid-access.md) e [Publicando novos aplicativos com o Proxy de Aplicativo do AD do Azure](active-directory-application-proxy-configure.md).

### Integrando aplicativos com o AD do Azure
Os artigos a seguir abordam as diferentes maneiras pelas quais os aplicativos são integrados com o AD do Azure, além de fornecer uma orientação.

- [Determinando qual Active Directory será usado](active-directory-administer.md)
- [Integrando com aplicativos existentes](active-directory-sso-integrate-existing-apps.md)
- [Publicando novos aplicativos com o Proxy de Aplicativo do AD do Azure](active-directory-application-proxy-configure.md)
- [Usando aplicativos na galeria de aplicativos do Azure](active-directory-appssoaccess-whatis.md/#get-started-with-the-azure-ad-application-gallery.md)
- [Integrando a lista de tutoriais de aplicativos SaaS](active-directory-saas-tutorial-list.md)

## Gerenciando o acesso a aplicativos
Os artigos a seguir descrevem as maneiras pelas quais você pode gerenciar o acesso aos aplicativos depois de serem integrados ao AD do Azure usando os Conectores do AD do Azure e o AD do Azure no portal do Azure.

- [Gerenciando o acesso a aplicativos com o AD do Azure](active-directory-managing-access-to-apps.md)
- [Automatizando com os Conectores do AD do Azure](active-directory-saas-app-provisioning.md)
- [Atribuindo usuários a um aplicativo](active-directory-applications-guiding-developers-assigning-users.md) com o portal do Azure.
- [Atribuindo grupos a um aplicativo](active-directory-applications-guiding-developers-assigning-groups.md) com o portal do Azure.
- [Compartilhando contas](active-directory-sharing-accounts.md)

## Integrando aplicativos personalizados
Se você estiver escrevendo um novo aplicativo e desejar ajudar os desenvolvedores a aproveitar o potencial do AD do Azure, veja [Orientando desenvolvedores](active-directory-applications-guiding-developers-for-lob-applications.md).

Se desejar adicionar seu aplicativo personalizado à Galeria de Aplicativos do Azure, veja [“Traga seu próprio aplicativo” com a configuração do SAML por autoatendimento do AD do Azure](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

<!---HONumber=Oct15_HO4-->