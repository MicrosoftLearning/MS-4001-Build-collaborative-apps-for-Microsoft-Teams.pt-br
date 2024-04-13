---
lab:
  title: "Criar uma guia\_do Teams"
  module: Exercise 3
---

# Exercício 3: Criar uma guia do Teams

## Cenário

Suponha que a equipe de suporte de TI queira criar uma guia do Teams para ajudar os usuários a acessar as informações necessárias ao enviar tíquetes de suporte. Por exemplo, a equipe precisa exibir os códigos de localidade dos usuários para processamento de tíquetes e relatórios. Sua tarefa atual é criar essa guia para exibir o código de localidade de um usuário. Mais informações serão adicionadas ao guia numa data posterior.

## Tarefas de exercícios

Sua tarefa é criar um aplicativo de guia do Teams que recupere a localidade do usuário, usando o contexto do Teams, e a exibe em uma guia pessoal.

Você precisará realizar as seguintes tarefas para concluir o exercício:

1. Crie um aplicativo de guia usando o Kit de Ferramenta do Teams para Visual Studio Code.
1. Atualize o aplicativo para recuperar e exibir a localidade do usuário.

**Tempo estimado de conclusão:** 10 minutos

## Tarefa 1: Criar um aplicativo de guia usando o Kit de Ferramenta do Teams para Visual Studio Code

1. Abra o Visual Studio Code.
1. Na barra lateral, selecione o ícone **Microsoft Teams** para abrir o painel **Kit de Ferramenta do Teams**.
1. Selecione **Criar aplicativo** e, em seguida, **Tabela**.
1. Selecione **Guia Básico** na lista de opções disponíveis.
1. Para a linguagem de programação, selecione **TypeScript**.
1. Em **Pasta do espaço de trabalho**, selecione **Pasta padrão**.
1. Em **Nome do aplicativo**, insira **UserInfoApp**.

Uma notificação será exibida quando todas as pastas e arquivos tiverem sido colocados em scaffolding com êxito, e uma nova instância do Visual Studio Code abrirá a nova pasta do projeto.

No painel **GERENCIADOR**, a pasta *src* contém o código-fonte do seu aplicativo. Os arquivos fora da pasta *src* são relacionados ao servidor, como o bot.

## Tarefa 2: Atualize o aplicativo para recuperar e exibir a localidade do usuário

Agora, você pode adicionar a funcionalidade desejada ao aplicativo de guia.

1. Navegue até a pasta `src` > `views` e abra o arquivo `hello.html`.
1. Localize o elemento `<div>` e atualize-o para conter os seguintes elementos entre as marcas `<div>` e `</div>`:

    ```html
        <h1>Hello, User</h1>
      <span>
        <h2>Review your locale code:</h2>
        <p id="locale"></p>
      </span>
    ```

1. Navegue até a pasta `src` > `static` > `scripts` e abra o arquivo `teamsapp.js`.
1. Substitua o conteúdo do arquivo  pelo seguinte código:

    ```typescript
        (function () {
          "use strict";
        
          // Call the initialize API first
          microsoftTeams.app.initialize().then(function () {
            microsoftTeams.app.getContext().then(function (context) {
              if (context?.app?.locale) {
                updateLocale(context.app.locale);
              }
              else{
                updateLocale("unknown");
              }
            });
          });
        
          function updateLocale(locale) {
            if(locale){
              document.getElementById("locale").innerHTML = locale;
            }
          }
        })();
    ```

    Esse código usa o objeto **contexto** para recuperar a localidade do usuário e atualiza o HTML para exibir o código da localidade.

## Verifique seu trabalho

Execute seu aplicativo no modo de depuração para testar a funcionalidade.

1. No Visual Studio Code, selecione o ícone **Microsoft Teams** para abrir o painel **Kit de Ferramenta do Teams**.

2. Comece a executar seu aplicativo com o depurador conectado usando um destes métodos:

   - Selecione a tecla F5.
   - No Visual Studio Code, navegue até o menu **Executar e depurar**.  Selecione **Depurar no Teams** com a opção de navegador desejada e, em seguida, selecione o botão **Iniciar a depuração**.
   - Na seção **AMBIENTE** do Kit de Ferramenta do Teams, abra a pasta *local* e selecione o navegador de sua preferência.

3. Depois que o Visual Studio Code executar algumas verificações, com ações visíveis na guia **Console**, uma nova janela do navegador será aberta. Na caixa de diálogo **UserInfoApplocal**, selecione o botão **Adicionar** para instalar o aplicativo no Teams para pré-visualização.

O aplicativo agora pode ser visualizado na barra lateral. O aplicativo está pré-configurado com duas guias: **Guia Pessoal** e **Sobre**. Verifique se o código da localidade está sendo exibido na guia.
