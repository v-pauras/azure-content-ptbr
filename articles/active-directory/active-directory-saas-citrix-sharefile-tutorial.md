<properties 
    pageTitle="Tutorial: integração do Active Directory do Azure ao Citrix ShareFile | Microsoft Azure" 
    description="Saiba como usar o Citrix ShareFile com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#Tutorial: integração do Active Directory do Azure ao Citrix ShareFile

O objetivo deste tutorial é mostrar a integração do Azure ao Citrix ShareFile. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Um locatário do Citrix ShareFile

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Citrix ShareFile poderão fazer logon único no aplicativo em seu site de empresa do Citrix ShareFile (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos com o Citrix ShareFile
2.  Configurando o logon único
3.  Configurando o provisionamento de usuários
4.  Atribuindo usuários

![Cenário](./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Cenário")
##Habilitando a integração de aplicativos com o Citrix ShareFile

O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Citrix ShareFile.

###Para habilitar a integração de aplicativos para o Citrix ShareFile, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Adicionar aplicativo")

5.  No diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **Citrix ShareFile**.

    ![Galeria de aplicativos](./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Galeria de aplicativos")

7.  No painel de resultados, selecione **Citrix ShareFile** e clique em **Concluir** para adicionar o aplicativo.

    ![Citrix ShareFile](./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários autentiquem no Citrix ShareFile com sua própria conta do Azure AD usando federação baseada no protocolo SAML.

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do Azure AD, na página de integração de aplicativos do **Citrix ShareFile**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.

    ![Habilitar logon único](./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Habilitar logon único")

2.  Na página **Como você deseja que os usuários façam logon no Citrix ShareFile**, selecione **Logon Único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar o logon único](./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Configurar o logon único")

3.  Na página **Configurar URL do Aplicativo**, na caixa de texto **URL de Logon do Citrix ShareFile**, digite a URL usando o padrão `https://<tenant-name>.shareFile.com` e clique em **Avançar**.

    ![Configurar a URL do Aplicativo](./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Configurar a URL do Aplicativo")

4.  Na página **Configurar logon único no Citrix ShareFile**, para baixar seu certificado, clique em **Baixar certificado** e salve o arquivo de certificado no computador.

    ![Configurar o Logon Único](./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "Configurar o Logon Único")

5.  Em outra janela do navegador da Web, faça logon em seu site de empresa do **Citrix ShareFile** como administrador.

6.  Na barra de ferramentas na parte superior, clique em **Administrador**.

7.  No painel de navegação à esquerda, selecione **Configurar Logon Único**.

    ![Administração de Conta](./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Administração de Conta")

8.  Na página **Configuração de Logon Único/SAML 2.0** em **Configurações Básicas**, realize as seguintes etapas:

    ![Logon único](./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Logon único")

    1.  Clique em **Habilitar SAML**.
    2.  No portal do Azure, na página do diálogo **Configurar logon único no Citrix ShareFile**, copie o valor da **ID de Entidade** e cole-o na caixa de texto **Sua ID de Entidade/Emissor de IDP**.
    3.  No portal do Azure, na página do diálogo **Configurar logon único no Citrix ShareFile**, copie o valor da **URL de Logon Remoto** e cole-o na caixa de texto **URL de Logon**.
    4.  No portal do Azure, na página do diálogo **Configurar logon único no Citrix ShareFile**, copie o valor da **URL de Logoff Remoto** e cole-o na caixa de texto **URL de Logoff**.
    5.  Clique em **Alterar** ao lado do campo **Certificado X.509** e carregue o certificado baixado no portal do Azure AD. ![Configurações Básicas](./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Configurações Básicas")

9.  Clique em **Salvar** no portal de gerenciamento do Citrix ShareFile.

10. No portal do AD do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar Logon Único**.

    ![Configurar logon único](./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Configurar logon único")
##Configurando o provisionamento de usuários

Para permitir que os usuários do Azure AD façam logon no Citrix ShareFile, eles devem ser provisionados no Citrix ShareFile. No caso do Citrix ShareFile, o provisionamento é uma tarefa manual.

###Para provisionar contas de usuário, execute as seguintes etapas:

1.  Faça logon em seu locatário do **Citrix ShareFile**.

2.  Clique em **Gerenciar Usuários > Gerenciar Página Inicial dos Usuários > + Criar Funcionário**.

    ![Criar Funcionário](./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Criar Funcionário")

3.  Insira o **Email**, **Nome** e **Sobrenome** de uma conta válida do Azure AD que você deseja provisionar.

    ![Informações Básicas](./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Informações Básicas")

4.  Clique em **Adicionar Usuário**.

    >[AZURE.NOTE]O titular da conta do AAD receberá um email e clicará em um link para confirmar sua conta antes que ela se torne ativa.

>[AZURE.NOTE]É possível usar qualquer outra ferramenta de criação da conta de usuário do Citrix ShareFile ou APIs fornecidas pelo Citrix ShareFile para provisionar as contas de usuário do AAD.

##Atribuindo usuários

Para testar sua configuração, será necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que o utilizem.

###Para atribuir usuários ao Citrix ShareFile, execute as seguintes etapas:

1.  No portal do Azure AD, crie uma conta de teste.

2.  Na página de integração de aplicativos do **Citrix ShareFile**, clique em **Atribuir usuários**.

    ![Atribuir usuários](./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Atribuir usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e em **Sim** para confirmar a atribuição.

    ![Sim](./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Sim")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=Nov15_HO1-->