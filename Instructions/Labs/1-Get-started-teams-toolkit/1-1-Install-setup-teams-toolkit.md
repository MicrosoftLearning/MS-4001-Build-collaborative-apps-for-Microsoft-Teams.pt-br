# Exercício 1: Instalar e configurar o Kit de Ferramentas do Teams para Visual Studio Code

Neste exercício, você instalará o Kit de Ferramentas do Teams para Visual Studio Code e configurará seu ambiente.

## Tarefa 1: Instalar o Kit de Ferramentas do Teams para Visual Studio Code

1. Abra o **Visual Studio Code**.
2. Selecione o ícone **Extensões** na barra lateral.
3. Procure por "Kit de Ferramentas do Teams" na seção **Extensões** usando a barra de pesquisa. Em seguida, escolha **Instalar**.

:::image type="content" source="../../media/teams-toolkit-install.png" alt-text="Captura de tela da instalação do Kit de Ferramentas do Teams para Visual Studio Code.":::

**Observação**:  Os exercícios deste módulo usam o Kit de Ferramentas do Teams v5.0.0.

Você também pode instalar o Kit de Ferramentas do Teams a partir do [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension).

## Tarefa 2: Preparar sua conta corporativa ou de estudante do Microsoft 365

Se você já tiver acesso de administrador a uma conta corporativa ou de estudante do Microsoft 365 que seja adequada para desenvolvimento e teste, poderá usar essa conta para executar e depurar seu aplicativo. Certifique-se de usar um locatário que seja seguro para realizar as operações sem afetar os usuários reais.

Caso contrário, você pode criar uma conta de teste gratuita usando o [Programa para Desenvolvedores do Microsoft 365](https://aka.ms/m365developers).  Depois que a configuração for concluída, o Programa para Desenvolvedores do Microsoft 365 fornecerá a você acesso de administrador a um locatário que poderá ser usado para criar aplicativos do Teams.

## Tarefa 3: Configurar um locatário do Microsoft 365 para carregar aplicativos para o Teams

Ative o sideload de aplicativos personalizados para seu locatário seguindo estas etapas:

1. Entre no [Centro de administração do Microsoft Teams](https://admin.teams.microsoft.com) com suas credenciais de administrador do Microsoft 365.

2. Na barra lateral, selecione **Aplicativos do Teams** e, em seguida, **Políticas de Instalação**.

3. Selecione a política **Global (padrão em toda a organização)** e, em seguida, ative a opção **Carregar aplicativos personalizados**. 
   :::image type="content" source="../../media/configure-upload-apps.png" alt-text="Captura de tela da configuração do upload de aplicativos personalizados.":::

4. Selecione o botão **Salvar** para salvar suas alterações. Seu locatário agora está configurado para permitir o sideload de aplicativos personalizados.

Na próxima unidade, você aprenderá a criar um aplicativo do Teams e a executá-lo localmente no Teams.
