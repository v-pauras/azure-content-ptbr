<!--author=alkohli last changed: 9/17/15-->

#### Para concluir a configuração mínima do dispositivo StorSimple

1. Na página de **Dispositivos**, selecione o dispositivo, clique na seta no nome do dispositivo para ir até a página específica do dispositivo. 

	![Página de dispositivos com o dispositivo online](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png)

2. Clique no início rápido do ícone ![Ícone de Inicialização Rápida](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) para acessar à página de início rápido do dispositivo. Clique em **Concluir a instalação do dispositivo** para iniciar o assistente de **Configuração do dispositivo**.

	![Página de início rápido do dispositivo](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)

2. Na página **Configurações Básicas**, faça o seguinte:
  1. Forneça um **nome amigável** para seu dispositivo. O nome do dispositivo padrão reflete as informações, tais como, o número de série e o modelo do dispositivo. Você pode atribuir um nome amigável de até 64 caracteres para gerenciar o dispositivo.
  2. Defina o **fuso horário** com base na localização geográfica na qual o dispositivo está sendo implantado. Seu dispositivo usará esse fuso horário para todas as operações agendadas.
  3. Em **Configurações DNS**, forneça um endereço para o **Servidor DNS Secundário**. Se você estiver usando o IPv6, o campo será preenchido com base no prefixo IPv6 fornecido na interface do Windows PowerShell. Se o servidor DNS secundário não estiver configurado, você não poderá salvar a configuração do dispositivo.
  4. Em interfaces habilitadas para o iSCSI, habilite pelo menos uma rede para o iSCSI. Pelo menos uma interface de rede deve ser habilitada para a nuvem e uma interface precisa ser habilitada para o iSCSI. Os DADOS 0 são automaticamente habilitados para a nuvem.
 
      ![Definições básicas da instalação mínima do dispositivo do StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)

3. Clique no ícone de seta. ![Ícone de seta do StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. Na página **Interfaces de Rede**, forneça endereços IP fixos para os Controladores 0 e 1. Se a interface DADOS 0 foi configurada para o IPv4, os endereços IP fixos precisam ser fornecidos no formato IPv4. Se você forneceu um prefixo para a configuração do IPv6, os endereços IP fixos serão preenchidos automaticamente nesses campos.


	> [AZURE.NOTE]
 	> 
 	> - Os endereços IP fixos do controlador precisam ser IPs gratuitos dentro da sub-rede acessível pelo endereço IP do dispositivo.
 	> - Os endereços IP fixos do controlador são usados para atender às atualizações para o dispositivo, portanto, os IPs fixos devem ser roteáveis e conseguir conectar-se à Internet.

    ![Interfaces de rede da instalação mínima do dispositivo do StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

5. Clique no ícone de verificação ![Ícone de verificação do StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png). Você será retornado para a página **Início Rápido** do dispositivo.

 >[AZURE.NOTE]Você pode modificar todas as outras configurações do dispositivo a qualquer momento acessando a página **Configurar**.

![Vídeo disponível](./media/storsimple-complete-minimum-device-setup/Video_icon.png) **Vídeo disponível**

Para assistir a um vídeo que demonstra como concluir a configuração mínima do dispositivo, clique [aqui](http://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/).

<!---HONumber=Oct15_HO3-->