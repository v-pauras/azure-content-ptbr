<properties 
   pageTitle="Políticas de backup do StorSimple Snapshot Manager | Microsoft Azure"
   description="Descreve como usar o snap-in StorSimple Snapshot Manager MMC para criar e gerenciar as políticas de backup que controlam os backups agendados."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/15/2015"
   ms.author="v-sharos" />

# Usar o StorSimple Snapshot Manager para criar e gerenciar políticas de backup

## Visão geral

Uma política de backup cria um cronograma para o backup de dados de volumes localmente ou na nuvem. Quando cria uma política de backup, você também pode especificar uma política de retenção. (Você pode manter no máximo 64 instantâneos). Para obter mais informações sobre políticas de backup, consulte [Tipos de backup e políticas de backup](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies).

Este tutorial explica como:

- Criar uma política de backup 
- Editar uma política de backup 
- Excluir uma política de backup 

## Criar uma política de backup

Use o procedimento a seguir para criar uma nova política de backup.

#### Para criar uma política de backup

1. Clique no ícone da área de trabalho para iniciar o StorSimple Snapshot Manager.

2. No painel **Escopo**, clique com o botão direito em **Políticas de Backup** e clique em **Criar Política de Backup**.

    ![Criar uma política de backup](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    A caixa de diálogo **Criar uma Política** é exibida.

    ![Criar uma Política - guia Geral](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Na guia **Geral**, preencha as seguintes informações:

   1. Na caixa de texto **Nome**, digite um nome para a política.

   2. Na caixa de texto** Grupo de volumes **, digite o nome do grupo de volumes associado à política.

   3. Selecione **Instantâneo Local** ou **Instantâneo em Nuvem**.

   4. Selecione o número de instantâneos a serem retidos. Se você selecionar **Todos**, 64 instantâneos serão retidos (máximo).

4. Clique na guia **Agenda**.

    ![Criar uma política - guia Agenda](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Na guia **Agenda**, preencha as seguintes informações:

   1. Marque a caixa de seleção **Habilitar** para agendar o próximo backup.

   2. Nas **Configurações**, selecione **Uma vez**, **Diariamente**, **Semanalmente** ou **Mensalmente**.

   3. Na caixa de texto **Iniciar**, clique no ícone de calendário e selecione uma data de início.

   4. Em **Configurações Avançadas**, você pode definir cronogramas opcionais de repetição e uma data de término.

   5. Clique em **OK**.

Após você criar uma política de backup, as seguintes informações aparecem no painel **Resultados**:

- **Nome** – o nome da política de backup.

- **Tipo** – instantâneo local ou instantâneo de nuvem.

- **Grupo de Volumes** – o grupo de volumes associado à política.

- **Retenção** – o número de instantâneos retidos; o máximo é 64.

- **Criado** – a data em que esta política foi criada.

- **Habilitado** – se a política está em vigor. **Verdadeiro** indica que está em vigor e **Falso** indica que não está.

## Editar uma política de backup

Use o procedimento a seguir para editar uma política de backup existente.

#### Para editar uma política de backup

1. Clique no ícone da área de trabalho para iniciar o StorSimple Snapshot Manager. 

2. No painel **Escopo**, clique no nó **Políticas de Backup**. Todas as políticas de backup aparecem no painel **Resultados**.

3. Clique com o botão direito na política que você deseja editar e clique em **Editar**.

    ![Editar uma política de backup](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)

4. Quando a janela **Criar uma Política** for exibida, insira suas alterações e clique em **OK**.

## Excluir uma política de backup

Use o procedimento a seguir para excluir uma política de backup.

#### Para excluir uma política de backup

1. Clique no ícone da área de trabalho para iniciar o StorSimple Snapshot Manager. 

2. No painel **Escopo**, clique no nó **Políticas de Backup**. Todas as políticas de backup aparecem no painel **Resultados**.

3. Clique com o botão direito na política de backup que você deseja excluir e clique em **Excluir**. e
4. Quando a mensagem de confirmação aparecer, clique em **Sim**.

    ![Excluir confirmação da política de backup](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## Próximas etapas

- Saiba como [usar o StorSimple Snapshot Manager para administrar sua solução do StorSimple](storsimple-snapshot-manager-admin.md).
- Saiba como [Usar o StorSimple Snapshot Manager para exibir e gerenciar trabalhos de backup](storsimple-snapshot-manager-manage-backup-jobs.md).

<!---HONumber=Oct15_HO3-->