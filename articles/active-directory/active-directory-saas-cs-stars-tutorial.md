<properties
	pageTitle="Tutorial: integração do Active Directory do Azure ao CS Stars | Microsoft Azure"
	description="Saiba como configurar o logon único entre o Active Directory do Azure e o CS Stars."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="12/18/2015"
	ms.author="jeedes"/>


# Tutorial: integração do Active Directory do Azure ao CS Stars

O objetivo deste tutorial é mostrar a você como integrar o Alcumus Info Exchange ao Active Directory do Azure (Azure AD).<br>A integração do Alcumus Info Exchange com o Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao CS Stars 
- Você pode habilitar seus usuários a fazerem logon automaticamente no CS Stars (logon único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um local central – o Portal do Active Directory do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## Pré-requisitos 

Para configurar a integração do Azure AD ao CS Stars, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do CS Stars


> [AZURE.NOTE]Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/). 

 
## Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do AD do Azure em um ambiente de teste. <br> O cenário descrito neste tutorial consiste em três blocos de construção principais:

1. Adicionando CS Stars da galeria 
2. Configurar e testar o logon único do Azure AD


## Adicionando CS Stars da galeria
Para configurar a integração do CS Stars ao Azure AD, você precisa adicionar o CS Stars da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar o CS Stars da galeria, execute as seguintes etapas:**

1. No **Portal de Gerenciamento do Azure**, no painel de navegação esquerdo, clique em **Active Directory**. <br><br>![Active Directory][1]<br>

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior. <br><br>![Aplicativos][2]<br>

4. Clique em **Adicionar** na parte inferior da página. <br><br>![Aplicativos][3]<br>

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**. <br><br>![Aplicativos][4]<br>

6. Na caixa de pesquisa, digite **CS Stars**. <br><br>![Aplicativos][5]<br>

7. No painel de resultados, selecione **CS Stars** e clique em **Concluir** para adicionar o aplicativo. <br><br>![Aplicativos][400]<br>



##  Configurar e testar o logon único do Azure AD
O objetivo desta seção é mostrar como configurar e testar logon único do Azure AD com o CS Stars, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do CS Stars é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do CS Stars.<br> Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** no Azure AD ao valor de **nome de usuário** no CS Stars.
 
Para configurar e testar o logon único do Azure AD com o CS Stars, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o Logon único do Azure AD](#configuring-azure-ad-single-single-sign-on)**: para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#creating-an-azure-ad-test-user)**: para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Criação de um usuário de teste do CS Stars](#creating-a-cs-stars-test-user)**: para ter um equivalente de Brenda Fernandes no CS Stars que esteja vinculado à representação dela no Azure AD.
5. **[Atribuição do usuário de teste do Azure AD](#assigning-the-azure-ad-test-user)**: habilitar Brenda Fernandes a usar o logon único do Azure AD.
5. **[Teste do logon único](#testing-single-sign-on)**: para verificar se a configuração funciona.

### Configuração do logon único do AD do Azure

O objetivo desta seção é habilitar o logon único do Azure AD no portal do Azure AD e configurar o logon único em seu aplicativo do CS Stars.<br>

**Para configurar o logon único do Azure AD com o CS Stars, execute as seguintes etapas:**

1. No portal do Azure AD, na página de integração de aplicativos do **CS Stars**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**. <br><br>![Configurar o logon único][6]<br>

2. Na página **Como você deseja que os usuários façam logon no CS Stars**, selecione **Logon único do Azure AD** e clique em **Próximo**.<br><br>![Logon único do AD do Azure][7]<br>

3. Na página de caixa de diálogo **Definir configurações de aplicativo**, execute as seguintes etapas: <br><br>![Definir configurações de aplicativo][8]<br>
 
     3\.1 na caixa de texto **URL de Logon Único**, digite a URL utilizada pelos usuários para entrar no aplicativo CS Stars (por exemplo, **https://uat.csstars.com/enterprise/default.cmdx?ssoclient=C234UAT2*).

     >[AZURE.NOTE]Se você não souber qual é o valor correto, contate o seu representante Marsh ClearSight.

     3\.2. Clique em **Próximo**.
 
4. Na página **Configurar logon único no CS Stars**, clique em **Baixar metadados** e salve o arquivo de metadados localmente no computador. <br><br>![O que é o Azure AD Connect][9]<br>

5. Para habilitar o logon único para o CS Stars, entre em contato com seu representante Marsh ClearSight e envie o arquivo de metadados.


6. No portal do Azure AD, selecione a confirmação de configuração de logon único e clique em **Próximo**. <br><br>![O que é o Azure AD Connect][10]<br>

7. Na página **Confirmação de logon único**, clique em **Concluir**.<br><br>![O que é o Azure AD Connect][11]<br>




### Criação de um usuário de teste do Azure AD
O objetivo desta seção é criar um usuário de teste no portal do Azure chamado Brenda Fernandes.<br> Na lista de usuários, selecione **Brenda Fernandes**. <br><br>![Criar um usuário do AD do Azure][20]<br>

**Para criar um usuário de teste no Azure AD, execute as seguintes etapas:**

1. No **Portal de Gerenciamento do Azure**, no painel de navegação esquerdo, clique em **Active Directory**. <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png)<br> 

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**. <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png)<br>
 
4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**. <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png)<br>

5. Na página de diálogo **Conte-nos sobre este usuário**, execute as seguintes etapas: <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png)<br>
  1. Em Tipo de Usuário, selecione Novo usuário na organização.
  2. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.
  3. Clique em Avançar.

6.  Na caixa de diálogo **Perfil do Usuário**, execute as seguintes etapas: <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png)<br>
  1. Na caixa de texto **Nome**, digite **Brenda**.  
  2. Na caixa de texto **Sobrenome**, digite **Fernandes**.
  3. Na caixa de texto **Nome de exibição**, digite **Brenda Fernandes**.
  4. Na lista **Função**, selecione **Usuário**.
  5. Clique em **Próximo**.

7. Na página de diálogo **Obter senha temporária**, clique em **criar**. <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png)<br>
 
8. Na página de diálogo **Obter senha temporária**, execute as seguintes etapas: <br><br>![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png)<br>
  1. Anote o valor da **Nova senha**.
  2. Clique em **Concluído**.   

  
 
### Criando um usuário de teste CS Stars

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no CS Stars.

**Para criar um usuário chamado Brenda Fernandes no CS Stars, execute as seguintes etapas:**

1. Entre em contato com seu representante Marsh ClearSight.


### Atribuição do usuário de teste do Azure AD

O objetivo desta seção é habilitar que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao CS Stars.<br><br>![Atribuir usuário][200]<br>

**Para atribuir Brenda Fernandes ao CS Stars, execute as seguintes etapas:**

1. No portal do Azure, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **aplicativos** no menu superior. <br><br>![Atribuir usuário][201]<br>
2. Na lista de aplicativos, selecione **CS Stars**. <br><br>![Atribuir usuário][202]<br>
1. No menu na parte superior, clique em **Usuários**. <br><br><br>![Atribuir usuário][203]<br>
1. Na lista de usuários, selecione **Brenda Fernandes**.

2. Na barra de ferramentas na parte inferior, clique em **Atribuir**. <br><br>![Atribuir usuário][205]<br>



### Teste do logon único

O objetivo desta seção é testar a sua configuração de logon único do Azure AD usando o Painel de Acesso.<br> Quando você clica no bloco CS Stars no Painel de Acesso, deve fazer logon automaticamente no seu aplicativo CS Stars.


## Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_01.png
[6]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_02.png
[7]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_03.png
[8]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_04.png
[9]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_05.png
[10]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_06.png
[11]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_07.png
[20]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_202.png
[203]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_403.png

<!---HONumber=AcomDC_1223_2015-->