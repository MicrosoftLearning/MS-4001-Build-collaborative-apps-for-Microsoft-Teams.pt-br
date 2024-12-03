---
lab:
  title: Crie um bot com IA
  module: Exercise 4
---

# Exercício 4: Criar um bot com IA

## Cenário

Imagine que você é um membro da equipe de suporte de TI. Você percebe que compilar o Relatório Semanal é um processo muito manual e demorado. Você deseja criar um bot de IA no MS Teams. Simplesmente ao discutir os itens de trabalho semanais e as tarefas para a próxima semana com o bot em uma conversa, ele pode gerar um relatório semanal bem formatado. Isso pode melhorar significativamente a eficiência do trabalho.

## Tarefas de exercício

Você precisa concluir as seguintes tarefas para concluir o exercício:

1. Criar um bot usando a biblioteca de IA do Teams
1. Conectar-se ao Serviço OpenAI
1. Implementar a funcionalidade de código
1. Atualizar prompts

**Tempo estimado de conclusão:** 20 minutos

## Pré-requisitos

Para executar o modelo do bot de chat de IA em seu computador de desenvolvimento local, você não só precisa atender aos requisitos de recursos mencionados no laboratório anterior, mas isso também exige uma conta OpenAI. Esta conta pode ser da [OpenAI](https://platform.openai.com/) ou do [OpenAI do Azure](https://aka.ms/oai/access).

## Tarefa 1: Criar um bot usando a biblioteca de IA do Teams

**Meta:** configurar um novo projeto de bot usando a biblioteca de IA do Teams e familiarizar-se com a estrutura e os arquivos do projeto.  

Usar o modelo de bot de chat de IA para criar um novo bot:

1. Abra o Visual Studio Code.
1. Na barra lateral, selecione o ícone **Microsoft Teams** para abrir o painel **Kit de Ferramenta do Teams**.
1. Selecione o botão **Criar Novo Aplicativo**.
1. No menu **Novo Projeto**, selecione **Copilot Personalizado** e, em seguida, selecione **Chatbot de IA Básico** para criar um bot de comando.
1. Em Linguagem de programação, selecione **TypeScript**.
1. Para **Serviço de LLMs (Grande Modelo de Linguagem),** escolha **Azure OpenAI** ou **OpenAI** com base em sua conta de LLM.
1. Pressione **Enter** para pular a configuração do LLM (Grande Modelo de Linguagem) primeiro. A chave do OpenAI será configurada na próxima etapa.
1. Em **Pasta do workspace**, selecione ou crie uma pasta para armazenar seus arquivos de projeto em seu computador.
1. Em **Application name**, insira **WeeklyReportChatBot** e pressione **Enter**. O Kit de Ferramentas do Teams fará o scaffold de um novo aplicativo e abrirá a pasta do projeto no Visual Studio Code.
1. Você pode receber uma mensagem do Visual Studio Code perguntando se você confia nos autores dos arquivos nesta pasta. Selecione o botão **Sim, confio nos autores** para continuar.
1. Examinar os diretórios e arquivos do projeto usando o Explorer no Visual Studio Code para se familiarizar com o código-fonte.

## Tarefa 2: Conectar-se ao Serviço OpenAI

**Meta:** configurar seu bot para se conectar a um Serviço OpenAI, seja por meio do OpenAI diretamente ou usando o OpenAI do Azure, configurando as chaves de API e os pontos de extremidade necessários.  

### Usar a conta do OpenAI
1. Abra o arquivo `.env.local.user` da pasta `env`.
1. No arquivo *env/.env.local.user*, preencha sua chave `SECRET_OPENAI_API_KEY=<your-key>` do OpenAI.
1. Abra o arquivo `app.ts` da pasta `src`.
1. (Opcional) Como esta demonstração usa os recursos de planejamento do modelo, o gpt-4 oferece uma melhoria significativa em relação à saída padrão do gpt-3.5. No arquivo *src/config.ts*, atualize o valor da propriedade *openAIModelName* de `"gpt-3.5-turbo"` para `"gpt-4"` ou `"gpt-4-turbo"`.

### Usar o OpenAI do Azure
1. Abra o arquivo `.env.local.user` da pasta `env`.
1. No arquivo *env/.env.local.user*, preencha sua chave `SECRET_AZURE_OPENAI_API_KEY=<azure-openai-api-key>` do OpenAI do Azure e o nome do ponto de extremidade `AZURE_OPENAI_ENDPOINT=<azure-openai-endpoint>` do OpenAI do Azure e o nome `AZURE_OPENAI_DEPLOYMENT_NAME=<azure-openai-deployment-name>` da Implantação do OpenAI do Azure.

## Tarefa 3: Implementar a funcionalidade de código

**Meta:** desenvolver as principais funcionalidades do bot modificando o arquivo app.ts, adicionando as importações necessárias e implementando a lógica de resposta para lidar com as interações do usuário.  

1. Abra o arquivo *src/app/app.ts*. Vamos modificar este arquivo de acordo com as etapas a seguir. O arquivo final pode ser referenciado em [app.ts](../../../Allfiles/Labs/Guided-Exercise5/app.ts).
1. Adicionar a importação `TurnContext` de `botbuilder` 
    ```typescript
    import { MemoryStorage, TurnContext } from "botbuilder";
    ```
1. Adicionar as importações `DefaultConversationState` e `TurnState` de `@microsoft/teams-ai`
    ```typescript
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application, ActionPlanner, OpenAIModel, PromptManager, DefaultConversationState, TurnState } from "@microsoft/teams-ai";
    ```
1. No arquivo *src/app.ts* antes da seção *Criar componentes de AI*, adicione a interface `ProjectInformation` e a definição `ApplicationTurnState`.
    ```typescript
    // Register project information item related handlers
    interface ProjectInformation {
      projectName: string;
      tasksAccomplished: string;
      tasksComing: string;
      blockingIssues: string;
    }

    // Strongly type the applications turn state
    interface ConversationState extends DefaultConversationState {
      greeted: boolean;
      projectInformation: ProjectInformation;
    }
    type ApplicationTurnState = TurnState<ConversationState>;

    // Create AI components
    ```
1. No arquivo *src/app.ts* após a seção *Define storage and application*, adicione a resposta do bot às mensagens.
    ```typescript
    // List for /reset command and then delete the conversation state
    app.message('/reset', async (context: TurnContext, state: ApplicationTurnState) => {
      state.deleteConversationState();
      await context.sendActivity("Your project information has been cleared.");
    });

    // Define the method for updating project information
    app.ai.action("updateProjectInformation", async (context: TurnContext, state: ApplicationTurnState, parameters: ProjectInformation) => {
      const conversation = ensureStateInitialized(state);
      if (parameters){
        if (parameters.projectName) {
          conversation.projectInformation.projectName = parameters.projectName;
        }
        if (parameters.tasksAccomplished) {
          conversation.projectInformation.tasksAccomplished = parameters.tasksAccomplished;
        } 
        if (parameters.tasksComing) {
          conversation.projectInformation.tasksComing = parameters.tasksComing;
        }
        if (parameters.blockingIssues) {
          conversation.projectInformation.blockingIssues = parameters.blockingIssues;
        }
        return `Project information was updated. Think about your next action`;
      }
    });

    // This method is used to make sure that the conversation state is initialized.
    function ensureStateInitialized(state: ApplicationTurnState): ConversationState {
      if (state.conversation.projectInformation == undefined) {
        state.conversation.projectInformation = {
          projectName: "",
          tasksAccomplished: "",
          tasksComing: "",
          blockingIssues: "",
        };
      }
      return state.conversation;
    }
    ```

## Tarefa 4: Atualizações de prompts

**Meta:** refinar os prompts de conversação do bot e configurar a chamada de função para melhorar a qualidade da interação e a eficácia na geração de relatórios semanais.

1. Atualize o arquivo `skprompt.txt` na pasta *src/prompts/chat*.  O arquivo final pode ser referenciado em [skprompt.txt](../../../Allfiles/Labs/Guided-Exercise5/skprompt.txt)
    ```txt
    You are a Teams Bot. Here is how you will act.
    Team Bot will adopt an encouraging and positive tone in all its interactions. This will be reflected in the creation of status report emails, ensuring that they are not only informative but also boost morale and foster a joyous team spirit. The language used will be engaging and supportive, aiming to excite and inspire the team while maintaining a professional undercurrent appropriate for the communication between a project manager and their team and stakeholders in a professional corporate setting. The Teams Bot will always ask for information from the user when it is not provided. 
    # Teams Bot will ask for the following project information to make status report emails:
    1. project name
    2. tasks accomplished this week
    3. tasks coming next week
    4. blocking issues

    # Status report task description like SCRUM style summary

    # Then, the users will type in the parameters and the bot will make the email.

    # For the first time, users are informed that they can clear the entered ProjectInformation with /reset command.

    # THE SUMMARY MUST BE:
    - G RATED
    - WORKPLACE / FAMILY SAFE
    NO SEXISM, RACISM OR OTHER BIAS/BIGOTRY.

    project information:
    {{$conversation.projectInfomation}}

    Typescript Interfaces:
    interface ProjectInformation {
        projectName: string;
        tasksAccomplished: string;
        tasksComing: string;
        blockingIssues: string;
    }
    ```
1. Atualize os parâmetros `max_tokens` e `temperature` no arquivo `src/prompts/chat/config.json`. Além disso, adicione um parâmetro de nó de aumento. O resultado será similar ao a seguir.  E pode ser referenciado em [config.json](../../../Allfiles/Labs/Guided-Exercise5/config.json)
    ```json
    {
      "schema": 1,
      "description": "AI Bot",
      "type": "completion",
      "completion": {
        "max_tokens": 2500,
        "temperature": 0.1,
        "top_p": 0.0,
        "presence_penalty": 0.6,
        "frequency_penalty": 0.0
      },
      "augmentation": {
        "augmentation_type": "monologue"
      }
    }
    ```
1. Crie um novo arquivo chamado `actions.json` na pasta *src/prompts/chat*. O conteúdo do arquivo é o seguinte. Este arquivo define os métodos para a IA usar. E pode ser referenciado em [actions.json](../../../Allfiles/Labs/Guided-Exercise5/actions.json)
    ```json
    [
        {
            "name": "updateProjectInformation",
            "description": "updates the information for the existing project",
            "parameters": {
                "type": "object",
                "properties": {
                    "projectName": {
                        "type": "string",
                        "description": "The name of the project"
                    },
                    "tasksAccomplished": {
                        "type": "string",
                        "description": "tasks that have been accomplished"
                    },
                    "tasksComing": {
                        "type": "string",
                        "description": "tasks that are coming up"
                    },
                    "blockingIssues": {
                        "type": "string",
                        "description": "any blocking issues"
                    }
                }
            }
        }
    ]
    ```

## Verifique seu trabalho

Execute o seu aplicativo localmente para testar a funcionalidade:

1. Abra o painel do **KIT DE FERRAMENTAS DO TEAMS**. No menu **DESENVOLVIMENTO**, selecione **Pré-visualizar o seu aplicativo do Teams** (ou use a chave `F5`) e, em seguida, selecione **Depurar no Teams ()** com o navegador de sua preferência.  
2. O Kit de Ferramentas do Teams provisionará e executará o seu aplicativo localmente em um navegador.
3. Na caixa de diálogo de instalação do aplicativo no navegador, selecione **Adicionar** para instalar o seu aplicativo do Teams.  O Teams abre uma conversa com o bot instalado.
4. Depois de inserir as informações de saudação, siga os prompts para inserir o nome do projeto, as tarefas concluídas, as tarefas não concluídas e as tarefas de bloqueio. O bot de IA gerará um Relatório Semanal semelhante à captura de tela abaixo.
5. Verifique se o bot responde a resposta correta da seguinte maneira. ![Captura de tela de um relatório semanal gerado por IA.](../../media/ai-weekly-report.png)
