---
TOCTitle: Como Implementar o cliente RMS
Title: Como Implementar o cliente RMS
ms:assetid: 'c84f1724-cf71-4385-9003-ff68bc23c927'
ms:contentKeyID: 18124175
ms:mtpsurl: 'https://technet.microsoft.com/pt-pt/library/Cc747749(v=WS.10)'
---

Como Implementar o cliente RMS
==============================

Se estiver a utilizar o Microsoft Windows XP ou o Microsoft Windows 2000, o software cliente RMS (Rights Management Services) deve poder utilizar quaisquer funcionalidades do RMS, tais como a Gestão de Direitos de Informação do Microsoft® Office System 2003 e o Suplemento de Gestão de Direitos para o Internet Explorer. O cliente RMS está incorporado no Windows Vista®.

Um grande número de organizações opta por controlar a implementação do software de cliente. O Systems Management Server (SMS) ou a Política de Grupo podem ser utilizados para implementar o RMS com o cliente Service Pack 2 (SP2).

Antes de iniciar a implementação, aceda ao site [http://go.microsoft.com/fwlink/?LinkId=67736](http://go.microsoft.com/fwlink/?linkid=67736) para transferir o cliente RMS.

| ![](images/Cc747749.Important(WS.10).gif)Importante                    |
|-----------------------------------------------------------------------------------------------------|
| O cliente RMS foi incorporado no Windows Vista. Assim, já não é necessária uma instalação separada. |

Extrair os Ficheiros de Instalação
----------------------------------

Após transferir o ficheiro WindowsRightsManagementServicesSP2-KB917275-Client-ENU.exe, deve extrair os ficheiros do Microsoft® Windows® Installer a partir do pacote executável.

Pode utilizar o comando seguinte na linha de comandos para efectuar a extracção:

`WindowsRightsManagementServicesSP2-KB917275-Client-ENU.exe /x <path>`

em que &lt;caminho&gt; é o directório de destino onde os ficheiros extraídos são colocados.

A execução deste comando extrai os ficheiros indicados a seguir para o directório de destino especificado:

-   Bootstrap.exe
    É um ficheiro wrapper utilizado pelo ficheiro executável para instalar outros ficheiros incluídos. Não é utilizado quando instala o cliente RMS com SP2 através do SMS ou da Política de Grupo.
-   MSDrmClient.msi
    É o ficheiro de instalação do cliente RMS com SP2. Esta instalação desinstala qualquer versão anterior do cliente RMS existente no computador. Este programa deve ser instalado primeiro nos computadores clientes.
-   RMClientBackCompat.msi
    É o ficheiro de instalação que identifica o novo cliente RMS com SP2 para aplicações activadas pelo RMS (tais como o Microsoft Office Professional 2003 ou o Microsoft Office System de 2007) que são dependentes da versão anterior do cliente RMS, de forma a que o cliente RMS com SP2 possa ser utilizado em seu lugar. Este programa deve ser instalado nos computadores clientes após a instalação com êxito do MSDrmClient.msi.

| ![](images/Cc747749.note(WS.10).gif)Nota                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Independentemente do método de instalação escolhido para a implementação, deve certificar-se de que ambos os ficheiros do Windows Installer são instalados com êxito. Se ocorrer um erro que evite a instalação do MSDrmClient.msi, não instale o RMClientBackCompat.msi. |

Implemente o cliente RMS efectuando uma instalação automática
-------------------------------------------------------------

A extracção dos ficheiros para instalar os ficheiros do Windows Installer é um procedimento opcional. Também pode implementar o cliente RMS efectuando uma instalação automática. Pode utilizar o comando seguinte na linha de comandos para efectuar a extracção:

`WindowsRightsManagementServicesSP2-KB917275-Client-ENU.exe -override 1 /I MsDrmClient.msi REBOOT=ReallySuppress /q -override 2 /I RmClientBackCompat.msi REBOOT=ReallySuppress /q`

Este comando inicia a instalação automática do cliente RMS.

| ![](images/Cc747749.note(WS.10).gif)Nota                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uma vez que é uma instalação automática, o Windows Installer não o informa quando está concluída. As instalações automáticas são normalmente executadas num ficheiro batch ou script. |

Implementar o cliente RMS Através do SMS
----------------------------------------

**Para implementar o cliente RMS através do SMS**
1.  Abra o SMS Administrator Console.

2.  Expanda a base de dados do local que pretende utilizar.

3.  No painel esquerdo, clique com o botão direito do rato em **Packages**, escolha **New** e, em seguida, clique em **Package From Definition**.

4.  Crie os pacotes a partir dos ficheiros MSDRMClient.msi e RMClientBackCompat.msi. Os pacotes devem ter as seguintes propriedades:

    **Gerais**:

    -   Para **Command line**, escreva o seguinte:
        `msiexec.exe /q ALLUSERS=2 /m MSIDGHOG /i "<file_name>.msi"`
        | ![](images/Cc747749.note(WS.10).gif)Nota                                                                    |
        |------------------------------------------------------------------------------------------------------------------------------------------|
        | MSIDGHOG é um valor aleatório. Substitua &lt;nome\_ficheiro&gt; pelo nome do ficheiro do Windows Installer que este pacote irá instalar. |

    -   Para **Run**, seleccione a opção **Hidden**.
    -   Para **After running**, seleccione a opção **No action required**.
    -   Para **Category**, seleccione a opção **Administrative Software**.

    **Requisitos:**

    -   Para **Estimated disk space**, escreva **445 KB**.
    -   Para **Maximum allowed run time**, seleccione **Unknown**.
    -   Seleccione a caixa de verificação **This program can run on any platform**.

    **Ambiente:**

    -   Para **Program can run**, seleccione a opção **Whether or not a user is logged on**.
    -   Para **Run mode**, seleccione a opção **Run with administrative rights**.
    -   Para **Drive mode**, seleccione a opção **Runs with UNC name**.

    **Avançadas:**

    -   Desmarque a caixa de verificação **Run another program first**.
    -   Desmarque a caixa de verificação **Suppress program notification** em **When the program is assigned to a computer**.
    -   Desmarque a caixa de verificação **Disable this program on computers where it is advertised**.

5.  Defina **Access Accounts and Distribution Points** conforme as necessidades da organização.

6.  Crie um anúncio para a colecção adequada. Recomenda-se a utilização do programa **Per-system unattended** numa implementação com o SMS.

7.  Agende este anúncio de acordo com as necessidades da organização.

Implementar o cliente RMS através da Política de Grupo
------------------------------------------------------

Pode utilizar a funcionalidade Instalação e Manutenção de Software da Política de Grupo para implementar o cliente RMS nos computadores de destino.

A Política de Grupo é o método recomendado para gerir de forma activa a implementação dos clientes RMS em pequenas e médias empresas, ou em empresas que ainda não utilizam uma solução de actualização empresarial, tal como o Systems Management Server 2003.

Quando utiliza a Política de Grupo para distribuir um programa, pode atribuí-lo aos computadores. O programa é instalado quando o computador é iniciado, ficando disponível para todos os utilizadores que iniciem sessão no computador. Para mais informações sobre a Política de Grupo, consulte Conceber uma infra-estrutura de Política de Grupo (<http://go.microsoft.com/fwlink/?linkid=24328>). Este procedimento assume que está a utilizar a Consola de Gestão de Política de Grupo (GPMC, Group Policy Management Console). Para transferir a GPMC, consulte Consola de Gestão de Política de Grupo com o Service Pack 1 (<http://go.microsoft.com/fwlink/?linkid=21813>).

O procedimento seguinte fornece um manual de consulta rápida para administradores não familiarizados com a distribuição de software baseada na Política de Grupo. Pode modificar estes passos conforme as necessidades da organização.

**Para implementar o cliente RMS através da Política de Grupo**
1.  Num controlador de domínio, abra o snap-in **Utilizadores e Computadores do Active Directory** da Consola de Gestão da Microsoft (MMC, Microsoft Management Console).

2.  Crie uma unidade organizacional (UO) nova ou seleccione uma UO existente.

    Se criar uma UO nova, adicione os computadores nos quais pretende instalar o cliente RMS.

3.  Clique com o botão direito do rato na UO e seleccione **Propriedades**.

4.  Seleccione o separador **Política de Grupo**.

5.  Clique em **Nova** para criar um Objecto de Política de Grupo (GPO, Group Policy Object) novo.

6.  Clique em **Editar** para editar o GPO novo.

7.  Na árvore da consola, expanda **Configuração do Computador, Definições de Software** e, em seguida, seleccione **Instalação de Software**.

8.  Clique com o botão direito no painel de detalhes, clique em **Novo** e, de seguida, clique em **Pacote**.

9.  Especifique um caminho para o ficheiro MSDRMclient.msi numa pasta de rede partilhada à qual os computadores clientes têm acesso.

10. Clique em **OK** para atribuir o pacote.

11. Repita os passos de 5 a 10 para criar um GPO que instale o ficheiro RMClientBackCompat.msi.

| ![](images/Cc747749.note(WS.10).gif)Nota                                                                                                                                                                                                                                                                                                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Estes passos são fornecidos apenas como referência para utilizadores que não estão familiarizados com a utilização da Política de Grupo. Se for um administrador experiente da Política de Grupo, pode aplicar procedimentos operacionais próprios para distribuir o pacote MSDrmClient.msi. Estes passos destinam-se a um controlador de domínio com o Windows Server 2003 — o processo e a terminologia podem ser diferentes num domínio do Windows 2000. |

Actualizar a partir de uma versão anterior
------------------------------------------

É possível utilizar um método de instalação automático a partir de um script que detectará se o cliente RMS com SP2 está instalado. Se o cliente não estiver instalado, o script actualizará o actual cliente ou instalará o cliente RMS com SP2. O script é o seguinte:


```
    Set objShell = Wscript.CreateObject("Wscript.Shell")
    Set objWindowsInstaller = Wscript.CreateObject("WindowsInstaller.Installer") 
    Set colProducts = objWindowsInstaller.Products 

    For Each product In colProducts 
    strProductName = objWindowsInstaller.ProductInfo (product, "ProductName")

    if strProductName = "Windows Rights Management Client with Service Pack 2" then
    strInstallFlag = "False"
    Exit For
    else
    strInstallFlag = "True"
    end if
    Seguinte

    if strInstallFlag = "True" then
    objShell.run "WindowsRightsManagementServicesSP2-KB917275-Client-ENU.exe -override 1 /I MsDrmClient.msi REBOOT=ReallySuppress /q -override 2 /I RmClientBackCompat.msi REBOOT=ReallySuppress /q "
    else
    wscript.echo "No installation required"
    end if
```

| ![](images/Cc747749.note(WS.10).gif)Nota                                         |
|---------------------------------------------------------------------------------------------------------------|
| Este script não funciona com o Windows Vista uma vez que o cliente RMS está incorporado no sistema operativo. |
