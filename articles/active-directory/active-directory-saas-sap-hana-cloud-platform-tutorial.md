<properties 
    pageTitle="Tutorial: Integração do Active Directory do Azure com a Plataforma de Nuvem HANA SAP | Microsoft Azure" 
    description="Saiba como usar a SAP HANA Cloud Platform com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!" 
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

#Tutorial: Integração do Active Directory do Azure com a Plataforma de Nuvem HANA SAP
  
O objetivo deste tutorial é mostrar a integração do Azure com a Plataforma de Nuvem HANA SAP. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

-   Uma assinatura válida do Azure
-   Uma conta da Plataforma de Nuvem HANA SAP
  
Depois de concluir este tutorial, os usuários do Azure AD que você atribuiu à SAP HANA Cloud Platform poderão fazer logon único no aplicativo usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

>[AZURE.IMPORTANT]Você precisa implantar seu próprio aplicativo ou assinar um aplicativo em sua conta da Plataforma de Nuvem HANA SAP para testar o logon único. Neste tutorial, um aplicativo é implantado na conta.
  
O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1.  Habilitando a integração de aplicativos para a Plataforma de Nuvem HANA SAP
2.  Configurando o logon único
3.  Atribuindo uma função a um usuário
4.  Atribuindo usuários

![Cenário](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Cenário")
##Habilitando a integração de aplicativos para a Plataforma de Nuvem HANA SAP
  
O objetivo desta seção é descrever como habilitar a integração de aplicativos com a Plataforma de Nuvem HANA SAP.

###Para habilitar a integração de aplicativos com a Plataforma de Nuvem HANA SAP, execute as seguintes etapas:

1.  No Portal de Gerenciamento do Azure, no painel navegação à esquerda, clique em **Active Directory**.

    ![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3.  Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Aplicativos")

4.  Clique em **Adicionar** na parte inferior da página.

    ![Adicionar aplicativo](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Adicionar aplicativo")

5.  Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6.  Na **caixa de pesquisa**, digite **SAP HANA Cloud Platform**.

    ![Galeria de Aplicativos](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Galeria de Aplicativos")

7.  No painel de resultados, selecione **SAP HANA Cloud Platform** e clique em **Concluir** para adicionar o aplicativo.

    ![Hana SAP](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "Hana SAP")
##Configurando o logon único
  
O objetivo desta seção é descrever como permitir que os usuários se autentiquem na Plataforma de Nuvem HANA SAP com sua conta do AD do Azure usando federação baseada no protocolo SAML. Como parte deste procedimento, será necessário carregar um certificado codificado de base 64 no locatário da Plataforma de Nuvem HANA SAP. Se você não estiver familiarizado com esse procedimento, veja [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)

###Para configurar o logon único, execute as seguintes etapas:

1.  No portal do Azure AD, na página de integração de aplicativos da **SAP HANA Cloud Platform**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.

    ![Configurar o logon único](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configurar o logon único")

2.  Na página **Como você deseja que os usuários façam logon na SAP HANA Cloud Platform**, selecione **Logon Único do AD do Microsoft Azure** e clique em **Avançar**.

    ![Configurar o logon único](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configurar o logon único")

3.  Em outra janela do navegador da Web, faça logon na ferramenta Cockpit da SAP HANA Cloud Platform em https://account.\<landscape host>.ondemand.com/cockpit (por exemplo: **https://account.hanatrial.ondemand.com/cockpit*)).

4.  Clique na guia **Confiar**.

    ![Confiança](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Confiança")

5.  Na seção de gerenciamento de confiança, execute as seguintes etapas:

    ![Obter Metadados](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Obter Metadados")

    1.  Clique na guia **Provedor de Serviços Local**.
    2.  Para baixar o arquivo de metadados da SAP HANA Cloud Platform, clique em **Obter Metadados**.

6.  No portal do Active Directory do Azure, na página **Configurar URL do Aplicativo**, realize as etapas a seguir e clique em **Avançar**.

    ![Configurar a URL do Aplicativo](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configurar a URL do Aplicativo")

    1.  Na caixa de texto **URL de Logon**, digite a URL usada pelos usuários para entrar no aplicativo **SAP HANA Cloud Platform**. Esta é a URL específica da conta de um recurso protegido em seu aplicativo Plataforma de Nuvem HANA SAP. A URL baseia-se no seguinte padrão: *https://\<nomeAplicativo><nomeConta>.<host paisagem>.ondemand.com/<caminho\_para\_recurso\_protegido>* (por exemplo: **https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

		>[AZURE.NOTE]Esta é a URL em seu aplicativo Plataforma de Nuvem HANA SAP que exige que o usuário seja autenticado.

    2.  Abra o arquivo de metadados da SAP HANA Cloud Platform baixado e localize a marca **ns3:AssertionConsumerService**.
    3.  Copie o valor do atributo **Location** e cole-o na caixa de texto **URL de Resposta da SAP HANA Cloud Platform**.

7.  Na página **Configurar logon único na SAP HANA Cloud Platform**, para baixar os metadados, clique em **Baixar metadados** e salve o arquivo de certificado no computador.

    ![Configurar o logon único](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configurar o logon único")

8.  Na ferramenta Cockpit da SAP HANA Cloud Platform, na seção **Provedor de Serviços Local**, realize as seguintes etapas:

    ![Gerenciamento de Confiança](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Gerenciamento de Confiança")

    1.  Clique em **Editar**.
    2.  Para **Tipo de Configuração**, selecione **Personalizado**.
    3.  Para **Nome do Provedor Local**, deixe o valor padrão.
    4.  Para gerar um par de chaves **Chave de Assinatura** e um **Certificado de Assinatura**, clique em **Gerar Par de Chaves**.
    5.  Para **Propagação de Entidade**, selecione **Desabilitado**.
    6.  Para **Forçar Autenticação**, selecione **Desabilitado**.
    7.  Clique em **Salvar**.

9.  Clique na guia **Provedor de Identidade Confiável** e em **Adicionar Provedor de Identidade Confiável**.

    ![Gerenciamento de Confiança](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Gerenciamento de Confiança")

    >[AZURE.NOTE]Para gerenciar a lista de provedores de identidade confiáveis, será necessário ter escolhido o Tipo de configuração personalizado na seção Provedor de Serviço Local. Para o tipo de configuração Padrão, você terá uma confiança implícita e não editável para o Serviço de ID do SAP. Para Nenhum, não há configurações de confiança.

10. Clique na guia **Geral** e em **Procurar** para carregar o arquivo de metadados baixado.

    ![Gerenciamento de Confiança](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Gerenciamento de Confiança")

    >[AZURE.NOTE]Depois de carregar o arquivo de metadados, os valores de **URL de Logon Único**, **URL de Logoff Único** e o **Certificado de Assinatura** serão automaticamente populados.

11. Clique na guia **Atributos**.

12. Na guia **Atributos**, realize as seguintes etapas:

    ![Atributos](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Atributos")

    1.  Ao clicar em **Adicionar Atributo Baseado em Declaração**, adicione os seguintes atributos baseados em declaração:

        |Atributo de Asserção| Atributo de Entidade|
		|-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname| nome|--------------------|--------------------| |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname| sobrenome|-----------| |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress|email|

    >[AZURE.NOTE]A configuração dos Atributos depende de como os aplicativos na HCP são desenvolvidos, isto é, quais atributos eles esperam ter na resposta do SAML e por qual nome (Atributo de Entidade) eles acessam esse atributo no código.
    >  
    >a. O **Atributo Padrão** na captura de tela serve apenas para fins de ilustração. Ele não é necessário para que o cenário funcione.
    >
    >b. Os nomes e valores do **Atributo de Entidade** mostrados na captura de tela dependerão de como o aplicativo foi desenvolvido. É possível que o seu aplicativo precise de mapeamentos diferentes.

13. No portal do Azure, na página do diálogo **Configurar logon único na SAP HANA Cloud Platform**, selecione a confirmação de configuração de logon único e clique em **Concluir**.

    ![Configurar o logon único](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configurar o logon único")
  
Como uma etapa opcional, você pode configurar grupos com base na asserção para seu Provedor de Identidade do Active Directory do Azure

>[AZURE.NOTE]Usar grupos na Plataforma de Nuvem HANA SAP permite que você atribua dinamicamente um ou mais usuários a uma ou mais funções em seus aplicativos da Plataforma de Nuvem HANA SAP, determinados pelos valores de atributos na asserção SAML 2.0. Por exemplo, se a declaração contém o atributo “*contract=temporary*”, talvez seja conveniente que todos os usuários afetados sejam adicionados ao grupo “*TEMPORARY*”. O grupo “*TEMPORARY*” pode conter uma ou mais funções de um ou mais aplicativos implantados em sua conta da SAP HANA Cloud Platform.
>  
>Use os grupos com base em asserção se desejar atribuir em massa vários usuários para uma ou mais funções de aplicativos em sua conta da Plataforma de Nuvem HANA SAP. Se você quiser atribuir um único usuário ou um pequeno número deles a funções específicas, é recomendável atribuí-los diretamente na guia “**Autorizações**” da ferramenta cockpit da Plataforma de Nuvem HANA SAP.

##Atribuindo uma função a um usuário
  
Para permitir que os usuários do AD do Azure façam logon na Plataforma de Nuvem HANA SAP, atribua funções a eles na Plataforma de Nuvem HANA SAP.

###Para atribuir uma função a um usuário, execute as seguintes etapas:

1.  Faça logon em sua ferramenta cockpit da **SAP HANA Cloud Platform**.

2.  Execute as seguintes etapas:

    ![Autorizações](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Autorizações")

    1.  Clique em **Autorização**.
    2.  Clique na guia **Usuários**.
    3.  Na caixa de texto **Usuário**, digite o endereço de email do usuário.
    4.  Clique em **Atribuir** para atribuir uma função ao usuário.
    5.  Clique em **Salvar**.

##Atribuindo usuários
  
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do AD do Azure que você deseja que usem seu aplicativo.

###Para atribuir usuários à Plataforma de Nuvem HANA SAP, execute as seguintes etapas:

1.  No portal do AD do Azure, crie uma conta de teste.

2.  Na página de integração de aplicativos da **SAP HANA Cloud Platform**, clique em **Atribuir usuários**.

    ![Atribuir Usuários](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Atribuir Usuários")

3.  Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.

    ![Sim](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Sim")
  
Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=Nov15_HO1-->