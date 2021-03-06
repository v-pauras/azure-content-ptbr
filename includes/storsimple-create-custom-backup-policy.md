<!--author=SharS last changed: 9/15/15-->

#### Para criar uma política de backup personalizada

1. Na página **Dispositivos**, clique em **Políticas de Backup** e, em seguida, clique em **Adicionar**.

2. Na caixa de diálogo **Adicionar uma política de backup**, em **Definir sua política de backup**:

    1. Especifique o nome da política de backup.

    2. Selecione os volumes a serem adicionados a essa política. Você pode optar por adicionar vários volumes selecionando-os na lista suspensa.

    3. Clique no ícone de verificação ![ícone de verificação](./media/storsimple-add-backup-policy/HCS_CheckIcon-include.png).

     Você será notificado depois que a política tiver sido criada com êxito. A página de políticas de backup também será atualizada para mostrar a política recém-criada.

4. Clique no nome da política (primeira coluna) para fazer o detalhamento da política que você acabou de criar.

5. Clique em **Gerenciar agendas**.

6. Na caixa de diálogo **Gerenciar agendas**:

    1. Selecione **Criar novo** para adicionar outro agendamento.

    2. Na lista suspensa, escolha o tipo de backup como instantâneo **local** ou **de nuvem**.

    3. Especifique a frequência de backup em minutos, em horas, em dias ou em semanas.

    4. Selecione uma retenção. As opções de retenção dependem da frequência do backup. Por exemplo, para uma política diária, a retenção pode ser especificada em semanas, enquanto a retenção para uma política mensal é especificada em meses.
 
    5. Selecione a hora e a data de início da política.

    6. Marque a caixa de seleção para habilitar a política.

7. Clique no ícone de seleção ![ícone de verificação](./media/storsimple-add-backup-policy/HCS_CheckIcon-include.png) para concluir.

8. Você será retornado para os detalhes da política. Clique em **Salvar** para salvar as alterações feitas nessa política. Você será notificado quando a política tiver sido salva.

9. Retorne para a página **Políticas de Backup**. A listagem tabular de políticas de backup será atualizada para exibir a política modificada.

    ![Política de backup personalizada](./media/storsimple-create-custom-backup-policy/HCS_CustomBackupPolicyM-include.png).

<!---HONumber=AcomDC_1203_2015-->