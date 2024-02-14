---
lab:
  title: Implementar uma extensão de mensagem que recupera dados do Microsoft Graph
  module: Exercise 1
---

# Exercício 1: Implementar uma extensão de mensagem que recupera dados do Microsoft Graph

## Cenário

Suponha que você tenha sido solicitado a ajudar a equipe de Suporte de TI a criar uma extensão de mensagem que permita aos membros da equipe recuperar informações de contato para os usuários e inserir os detalhes do contato em mensagens no Teams usando cartões.  Nesse exercício, você implementará uma extensão de mensagem que recupera os dados do usuário do Microsoft Graph.  A solução já foi estruturada usando o Kit de Ferramentas do Teams, mas você precisará fazer alterações para implementar a funcionalidade.

## Tarefas de exercício

Sua meta é garantir que o aplicativo tenha a seguinte funcionalidade:

:::image type="content" source="../../media/contact-details-demo1.png" alt-text="Captura de tela da extensão de mensagem de detalhes do contato no Teams.":::

- Os usuários do aplicativo inserem o nome de um usuário na interface do usuário da extensão de mensagem.
- O aplicativo usa o ponto de extremidade `users` da API do Graph para localizar usuários por nome de exibição e lista os resultados.
- Quando o usuário do aplicativo seleciona o usuário desejado nos resultados da pesquisa, ele pode inserir o cartão desejado em uma mensagem no Teams.
- O cartão exibe o nome de exibição, o endereço de email e o número de telefone do usuário

:::image type="content" source="../../media/contact-details-demo2.png" alt-text="Captura de tela do aplicativo de detalhes de contato no Teams.":::

:::image type="content" source="../../media/contact-details-demo3.png" alt-text="Captura de tela do aplicativo de detalhes de contato no Teams: inserindo cartão na mensagem.":::

Execute as seguintes tarefas para concluir o exercício:

1. Acesse e examine o projeto.
2. Conclua a funcionalidade de pesquisa.
3. Adicione as consultas do Graph.
4. Provisione recursos para a extensão de mensagem.
5. Configure permissões para acessar o Microsoft Graph.
6. Implantar no Azure.
7. Execute e teste o aplicativo.

**Tempo de conclusão estimado:** 25 minutos

## Tarefa 1: Acessar e examinar o projeto

O aplicativo de extensão de mensagem foi estruturada usando o Kit de Ferramentas do Teams.  O aplicativo também foi atualizado para recuperar dados do usuário do Microsoft Graph usando a biblioteca de clientes JavaScript do Microsoft Graph.  Parte do código está incompleta.

1. Baixe o projeto [ContactDetails.zip](https://github.com/MicrosoftLearning/APL-4001-Build-collaborative-apps-for-Microsoft-Teams/raw/master/Allfiles/Labs/Starter/ContactDetails.zip) da pasta [Starter](https://github.com/MicrosoftLearning/APL-4001-Build-collaborative-apps-for-Microsoft-Teams/tree/master/Allfiles/Labs/Starter).
2. Extraia o conteúdo do arquivo zip para uma pasta chamada **ContactDetails** em seu computador e abra a pasta no Visual Studio Code.  
3. Examine os diretórios e arquivos do projeto na área do Explorer do Visual Studio Code para se familiarizar com o código-fonte.  Os arquivos e pastas de chave incluem:

| Pasta/Arquivo | Contents |
| --- | --- |
| `teamsapp.yml` | O arquivo de projeto principal descreve a configuração do aplicativo e define o conjunto de ações a serem executadas em cada fase do ciclo de vida. |
| `teamsapp.local.yml` | Isso substitui `teamsapp.yml` com ações que permitem a execução e a depuração locais. |
| `.vscode/` | Arquivos VSCode para depuração local. |
| `appPackage/` | Os arquivos do pacote do aplicativo, incluindo o manifesto do aplicativo Teams. |
| `infra/` | Modelos para provisionar recursos do Azure. |
| `index.js` | Ponto de entrada e manipulador `restify` do aplicativo. |
| `teamsBot.js` | Manipulador de atividades do Teams.  |

## Tarefa 2: Concluir a funcionalidade de pesquisa

A solução está sem o código para armazenar o valor da cadeia de caracteres de consulta de pesquisa para uso na consulta do Graph.  Atualize o código para armazenar esse valor em uma variável chamada `searchQuery`.

1. Navegue até o arquivo **TeamsBot.ts**.
2. No método `handleTeamsMessagingExtensionQuery`, localize o comentário **// Obtenha o contexto de pesquisa dos parâmetros de consulta.** na linha 81 e adicione a seguinte linha de código na próxima linha:

    ```JavaScript
    const searchQuery = query.parameters[0].value;
    ```

## Tarefa 3: Atualizar a consulta do Graph

A solução não tem o caminho da API para a consulta do Graph que usa a cadeia de caracteres de pesquisa.  Atualize a consulta para usar `$search` para pesquisar usuários pelo nome de exibição.

1. Na função `handleTeamsMessagingExtensionQuery`, localize o seguinte comentário na linha 84:

      `// Use the Graph API to search for users by their display name.`

2. Na próxima linha de código, substitua `path` pelo seguinte caminho de API:

     ```TypeScript
     /users?$search="displayName:${searchQuery}"&$count=true
     ```

O código deve atender aos requisitos de funcionalidade agora.

## Tarefa 4: Provisionar recursos para a extensão de mensagem

Em seguida, use o Kit de Ferramentas do Teams para provisionar os recursos necessários para a extensão de mensagem.

> Observação: Provisionar recursos de nuvem do Azure e implantar no Azure pode causar encargos em sua assinatura do Azure.

1. No código do Visual Studio, selecione o **Kit de Ferramentas do Teams** na barra lateral.
2. Em **CONTAS**, entre em seu locatário do Microsoft 365 e em sua conta do Azure.
3. Em **CICLO DE VIDA**, selecione **Provisionar**.
    :::image type="content" source="../../media/toolkit-provision.png" alt-text="Captura de tela da extensão do Kit de Ferramentas do Teams no Visual Studio Code.":::
4. Selecione um grupo de recursos em que você pode provisionar os recursos ou criar um novo grupo de recursos selecionando a opção **Novo grupo de recursos** e seguindo os prompts.  
    :::image type="content" source="../../media/new-resource-group.png" alt-text="Captura de tela do menu Selecionar um grupo de recursos no Kit de Ferramentas do Teams.":::
5. Na caixa de diálogo final para confirmar sua escolha, selecione **Provisionar**.

    Quando o provisionamento for concluído, um novo registro de aplicativo deverá ser criado em seu locatário do Microsoft 365 usando o ambiente `dev` no Kit de Ferramentas do Teams. O provisionamento pode demorar um pouco.

    :::image type="content" source="../../media/provisioned-resources-dev.png" alt-text="Captura de tela do Kit de Ferramentas do Teams para Visual Studio Code mostrando um ambiente de desenvolvimento provisionado.":::

## Tarefa 5: Configurar permissões para recuperar dados do Microsoft Graph

1. Entre no portal do Azure [portal.azure.com](portal.azure.com) usando sua conta de administrador do **Microsoft 365**.
2. No menu de navegação à esquerda, navegue até **Microsoft Entra ID**.
3. Navegue até **Gerenciar > Registros de aplicativo > Todos os registros**.
4. Selecione o registro do aplicativo **ContactDetails** criado durante o provisionamento.
5. Navegue até **Gerenciar > Permissões de API.**
6. Selecione **+ Adicionar uma permissão**.
7. Selecione **Microsoft Graph**.
8. Selecione **Permissões delegadas**.
9. Localize as permissões listadas em **Usuário** e selecione a permissão **User.Read.All**.
    :::image type="content" source="../../media/user-permissions.png" alt-text="Captura de tela das permissões do Graph no portal do Azure.":::
10. Selecione o botão **Adicionar permissões**.
11. A permissão está configurada, mas requer consentimento do administrador.
    :::image type="content" source="../../media/configured-permissions-consent.png" alt-text="Captura de tela da exibição Permissões Configuradas no Portal do Azure.":::
12. Selecione **Conceder consentimento do administrador para [locatário]** e selecione **Sim** para confirmar.

A permissão foi configurada e consentida.

## Tarefa 6: Implantar no Azure

Implante o aplicativo nos recursos provisionados no ambiente `dev`.

1. No painel do Kit de Ferramentas do Teams, em **Ciclo de Vida**, selecione **Implantar**.
2. Na caixa de diálogo de confirmação de implantação, selecione **Implantar**.
3. Verifique se há confirmação da implantação bem-sucedida no editor do Visual Studio Code.

A extensão da mensagem é hospedada no Azure.

## Verifique seu trabalho

Visualize seu aplicativo no cliente do Teams para testar a funcionalidade.

1. No painel do Kit de Ferramentas do Teams, em **Desenvolvimento**, selecione **Visualizar seu Aplicativo do Teams (F5)**.
2. No menu suspenso, selecione a opção desejada para **Iniciar Remoto** com seu navegador preferido.

    :::image type="content" source="../../media/launch-remote.png" alt-text="Captura de tela da opção para iniciar remotamente usando o Kit de Ferramentas do Teams.":::

3. Ao executar o aplicativo pela primeira vez, todas as dependências serão baixadas e o aplicativo será compilado. Uma janela do navegador abrirá ao concluir a compilação. Esse processo pode levar de três a cinco minutos para ser concluído.
4. O Teams exibe uma janela com a descrição e os requisitos de permissão do aplicativo.  Selecione **Adicionar** para adicionar o aplicativo.

    :::image type="content" source="../../media/add-contact-details-app.png" alt-text="Captura de tela do cliente do Teams com a opção de instalar o aplicativo de detalhes de contato.":::

5. Quando a extensão de mensagem for carregada no cliente do Teams, insira uma letra para pesquisar os usuários pelo nome de exibição.  Selecione um resultado para inserir um cartão na conversa.

Observação: Se, por algum motivo, a extensão da mensagem não for invocada automaticamente, você poderá acessá-la inserindo "Detalhes de desenvolvimento de @Contact" na barra de comandos na parte superior do cliente do Teams ou na área de mensagem de redação.  Você também pode usar o botão **Ações e aplicativos** da área de Redigir mensagem para localizar o aplicativo.
