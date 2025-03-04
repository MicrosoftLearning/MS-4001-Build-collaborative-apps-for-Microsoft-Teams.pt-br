# Exercício 1: Criar recursos do Azure para hospedar um aplicativo de guia do Teams

Neste exercício, primeiro você criará e provisionará um aplicativo de guia do Teams usando o Kit de Ferramentas do Teams para Visual Studio Code. Em um exercício posterior, você configurará o aplicativo a ser hospedado no Azure.

**Observação**: os exercícios neste módulo de treinamento usam o Kit de Ferramentas do Teams v5.8.0. As etapas a seguir pressupõem que a extensão do Kit de Ferramentas do Teams esteja instalada.

## Tarefa 1: Criar um novo aplicativo de guias

1. No Visual Studio Code, na Barra de atividades, selecione o ícone do **Microsoft Teams**.

1. No painel do Kit de Ferramentas do Teams, selecione o botão **Criar um novo aplicativo**.

1. Nas opções, selecione **Guia**.

    ![Captura de tela da opção Kit de Ferramentas do Teams para o aplicativo de guia.](../../media/create-teams-tab-app.png)

1. Em seguida, selecione **React com a interface do usuário Fluent**.

    ![Captura de tela do modelo de aplicativo do Kit de Ferramentas do Teams com a guia selecionada.](../../media/create-teams-tab-react.png)

1. Nas opções de linguagem de programação, selecione **JavaScript**.

1. Selecione um **local** para a pasta de projeto do aplicativo de guias e todos os seus arquivos.

1. Para o nome do aplicativo, insira **hello-tab** e selecione Enter.

1. O scaffolding do projeto começa. Quando o projeto é scaffolded, uma nova janela do Visual Studio Code é aberta com o novo projeto carregado.

    ![Captura de tela do novo projeto de guia do Teams Toolkit depois que a sua estrutura é criada (scaffolding).](../../media/new-tab-project.png)

1. No Visual Studio Code, selecione **Executar > Iniciar depuração** ou selecione a chave **F5** para iniciar a sessão de depuração.

1. O Visual Studio Code cria e inicia o aplicativo. Execute a sessão de depuração antes de começar a provisionar os recursos do Azure.

1. Quando o aplicativo for testado com êxito, pare de executar o aplicativo localmente.

    ![Captura de tela do aplicativo de guia de depuração](../../media/launch-debug-tab-app.png)

1. Para encerrar a sessão de depuração e parar de executar o aplicativo, você pode fechar o navegador, selecionar **Executar > Parar depuração** ou selecionar **Shift+F5**.

## Tarefa 2: Entrar no Azure no Kit de Ferramentas do Teams

Entre no Azure usando sua conta da **assinatura do Azure**.

1. Na Barra de atividades, selecione o ícone do **Microsoft Teams**.

1. No painel do Kit de Ferramentas do Teams, em **Contas**, selecione **Entrar no Azure**.

    ![Captura de tela do painel do Kit de Ferramentas do Teams com o botão para entrar no Azure.](../../media/sign-into-azure.png)

1. Na caixa de diálogo exibida, selecione **Entrar**.

    ![Captura de tela de uma caixa de diálogo para confirmar a entrada no Azure.](../../media/sign-into-azure-alert.png)
    ![Captura de tela de usuário logado no Azure](../../media/signed-into-azure.png)

## Tarefa 3: Provisionar os recursos

Agora você pode provisionar os recursos de que o seu aplicativo de guias do Teams precisa.

1. No painel do Kit de Ferramentas do Teams, em **Ciclo de vida**, selecione **Provisionar**.

    ![Captura de tela que realça o provisionamento na opção de nuvem na seção de implantação.](../../media/provision-start.png)

1. Em seguida, você precisa selecionar um grupo de recursos em que possa provisionar os recursos ou criar um novo grupo de recursos selecionando a opção **Novo grupo de recursos** no menu **Selecionar um grupo de recursos**.

    ![Captura de tela que mostra como criar um novo grupo de recursos.](../../media/resource-group.png)

1. A ferramenta sugere automaticamente o nome do grupo de recursos, como rg-hello-tab0989fd-dev. Selecione **Enter**.

1. Em seguida, selecione a **localização do Leste dos EUA** para o novo grupo de recursos e selecione **Enter**.

1. Em uma caixa de diálogo para confirmar a sua seleção, selecione **Provisionar**.

    ![Captura de tela da caixa de diálogo para confirmar o provisionamento.](../../media/provision-confirm.png)

1. O provisionamento começa para todos os recursos necessários para hospedar o aplicativo de guias do Teams no Azure. O provisionamento pode demorar um pouco.

    ![Captura de tela do sucesso da provisão.](../../media/provision-success.png)
Agora você provisionou com êxito todos os recursos necessários para hospedar o aplicativo de guias do Teams.

Em seguida, você implantará o código-fonte do aplicativo nesses recursos.
