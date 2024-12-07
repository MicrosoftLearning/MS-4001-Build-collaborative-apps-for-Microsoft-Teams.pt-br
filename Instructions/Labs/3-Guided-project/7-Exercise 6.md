---
lab:
  title: Criar um bot básico
  module: Exercise 6
---

# Exercício 6: Criar um bot básico

## Cenário
Suponha que você tenha sido solicitado a ajudar a equipe de Suporte de TI a criar um bot do Teams. Este bot terá a capacidade de recuperar a abreviação de um determinado estado e também buscar as condições climáticas atuais para uma área específica com base em seu CEP.

## Tarefas do exercício
Neste exercício, você usará o modelo do Kit de Ferramentas do Teams para criar um bot simples do Teams. Esse bot utilizará a biblioteca de IA do Teams para processar mensagens com usuários e incluir interações usando Cartões Adaptáveis. Observe que este exercício não envolve interações entre a biblioteca de IA do Teams e LLMs.

Você precisa concluir as seguintes tarefas para concluir o exercício:

1. Criar um bot do Teams usando o modelo de bot básico do kit de ferramentas do Teams.
1. Integrar a biblioteca de IA do Teams.
1. Criar um Cartão Adaptável.
1. Manipular mensagens.

**Tempo estimado de conclusão:** 15 minutos

## Tarefa 1: Criar um bot do Teams usando o modelo do kit de ferramentas do Teams

**Meta**: inicializar um projeto básico de bot do Teams usando o kit de ferramentas do Teams no Visual Studio Code.

Usar o modelo de Bot de Comando para criar um novo bot:

1. Abra o Visual Studio Code.
1. Na barra lateral, selecione o ícone **Microsoft Teams** para abrir o painel **Kit de Ferramenta do Teams**.
1. Selecione o botão **Criar Novo Aplicativo**.
1. No menu **Novo Projeto**, selecione **Bot** e, em seguida, selecione **Bot Básico** para criar um bot simples.
1. Em Linguagem de programação, selecione **TypeScript**.
1. Em **Pasta do workspace**, selecione ou crie uma pasta para armazenar seus arquivos de projeto em seu computador.
1. Em **Application name**, insira **TypeAheadBot** e pressione **Enter**. O Kit de Ferramentas do Teams fará o scaffold de um novo aplicativo e abrirá a pasta do projeto no Visual Studio Code.
1. Você pode receber uma mensagem do Visual Studio Code perguntando se você confia nos autores dos arquivos nesta pasta. Selecione o botão **Sim, confio nos autores** para continuar.
1. Examinar os diretórios e arquivos do projeto usando o Explorer no Visual Studio Code para se familiarizar com o código-fonte.

## Tarefa 2: Integrar a biblioteca de IA do Teams

**Meta**: adicionar a biblioteca de IA do Teams ao seu projeto para aprimorar os recursos do bot.

1. No Visual Code, pressione ``Ctrl + ` `` para abrir o Terminal.
1. No painel Terminal, execute o comando a seguir para instalar os pacotes axios e a biblioteca de IA do Teams. O pacote axios é um cliente HTTP baseado em promessa que usaremos neste exercício para chamar APIs da Web.
   ```shell
   npm install @microsoft/teams-ai axios --save
   ```
1. Crie um arquivo `app.ts` no diretório raiz do projeto. Adicione o código a seguir para definir e exportar o objeto `app`.
   ``` typescript
   // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
   import { Application } from "@microsoft/teams-ai";
   
   const app = new Application();
   
   export default app;
   ```
1. Abra o arquivo `index.ts` no diretório raiz do projeto. Substitua a referência por `TeamsBot` com o objeto `app`. O arquivo final de `index.ts` pode ser referenciado em [index.ts](../../../Allfiles/Labs/Guided-Exercise6/index.ts).
   1. Substitua a seção *This bot's main dialog.* com o seguinte código:
      
      Inserir snippet de código
      ``` typescript
      import app from "./app";
      ``` 
      Remover trecho de código
      ``` typescript
      // This bot's main dialog.
      import { TeamsBot } from "./teamsBot";
      ```
   1. Remover o trecho de código *Criar o bot que tratará as mensagens recebidas.*
      ``` typescript
      // Create the bot that will handle incoming messages.
      const bot = new TeamsBot();
      ```
   1. Modifique o trecho de código *Listen for incoming requests.* para usar o objeto `app` para responder a mensagens.
      ``` typescript
      // Listen for incoming requests.
      server.post("/api/messages", async (req, res) => {
        await adapter.process(req, res, async (context) => {
          await app.run(context); //replace bot with app object
        });
      });
      ```

## Tarefa 3: Criar um cartão adaptável

**Meta**: projetar e implementar Cartões Adaptáveis para interações de dados estáticas e dinâmicas dentro do bot.

1. Crie uma pasta chamada `cards` no diretório raiz do projeto.
1. Na pasta `cards`, crie um arquivo chamado `staticSearchCard.ts` com os seguintes conteúdos. O arquivo final de `staticSearchCard.ts` pode ser referenciado em [staticSearchCard.ts](../../../Allfiles/Labs/Guided-Exercise6/staticSearchCard.ts).
   ```typescript
   import { Attachment, CardFactory } from 'botbuilder';

    export function createStaticSearchCard(): Attachment {
        return CardFactory.adaptiveCard({
            $schema: 'http://adaptivecards.io/schemas/adaptive-card.json',
            version: '1.2',
            type: 'AdaptiveCard',
            body: [
                {
                    text: 'Please search for the list of state abbreviations from static list.',
                    wrap: true,
                    type: 'TextBlock'
                },
                {
                    columns: [
                        {
                            width: 'stretch',
                            items: [
                                {
                                    choices:
                                        [
                                            { title: 'Alabama', value: 'AL' },
                                            { title: 'Alaska', value: 'AK' },
                                            { title: 'Arizona', value: 'AZ' },
                                            { title: 'Arkansas', value: 'AR' },
                                            { title: 'California', value: 'CA' },
                                            { title: 'Colorado', value: 'CO' },
                                            { title: 'Connecticut', value: 'CT' },
                                            { title: 'Delaware', value: 'DE' },
                                            { title: 'Florida', value: 'FL' },
                                            { title: 'Georgia', value: 'GA' },
                                            { title: 'Hawaii', value: 'HI' },
                                            { title: 'Idaho', value: 'ID' },
                                            { title: 'Illinois', value: 'IL' },
                                            { title: 'Indiana', value: 'IN' },
                                            { title: 'Iowa', value: 'IA' },
                                            { title: 'Kansas', value: 'KS' },
                                            { title: 'Kentucky', value: 'KY' },
                                            { title: 'Louisiana', value: 'LA' },
                                            { title: 'Maine', value: 'ME' },
                                            { title: 'Maryland', value: 'MD' },
                                            { title: 'Massachusetts', value: 'MA' },
                                            { title: 'Michigan', value: 'MI' },
                                            { title: 'Minnesota', value: 'MN' },
                                            { title: 'Mississippi', value: 'MS' },
                                            { title: 'Missouri', value: 'MO' },
                                            { title: 'Montana', value: 'MT' },
                                            { title: 'Nebraska', value: 'NE' },
                                            { title: 'Nevada', value: 'NV' },
                                            { title: 'New Hampshire', value: 'NH' },
                                            { title: 'New Jersey', value: 'NJ' },
                                            { title: 'New Mexico', value: 'NM' },
                                            { title: 'New York', value: 'NY' },
                                            { title: 'North Carolina', value: 'NC' },
                                            { title: 'North Dakota', value: 'ND' },
                                            { title: 'Ohio', value: 'OH' },
                                            { title: 'Oklahoma', value: 'OK' },
                                            { title: 'Oregon', value: 'OR' },
                                            { title: 'Pennsylvania', value: 'PA' },
                                            { title: 'Rhode Island', value: 'RI' },
                                            { title: 'South Carolina', value: 'SC' },
                                            { title: 'South Dakota', value: 'SD' },
                                            { title: 'Tennessee', value: 'TN' },
                                            { title: 'Texas', value: 'TX' },
                                            { title: 'Utah', value: 'UT' },
                                            { title: 'Vermont', value: 'VT' },
                                            { title: 'Virginia', value: 'VA' },
                                            { title: 'Washington', value: 'WA' },
                                            { title: 'West Virginia', value: 'WV' },
                                            { title: 'Wisconsin', value: 'WI' },
                                            { title: 'Wyoming', value: 'WY' }
                                        ],
                                    style: 'filtered',
                                    placeholder: 'Search for a state abbreviation',
                                    id: 'choiceSelect',
                                    type: 'Input.ChoiceSet'
                                }
                            ],
                            type: 'Column'
                        }
                    ],
                    type: 'ColumnSet'
                }
            ],
            actions: [
                {
                    id: 'staticSubmit',
                    type: 'Action.Submit',
                    title: 'Submit',
                    data: {
                        verb: 'StaticSubmit'
                    }
                }
            ]
        });
    }
   ```

1. Na pasta `cards`, crie um arquivo chamado `dynamicSearchCard.ts` com o seguinte conteúdo. O arquivo final de `dynamicSearchCard.ts` pode ser referenciado em [dynamicSearchCard.ts](../../../Allfiles/Labs/Guided-Exercise6/dynamicSearchCard.ts).
   ```typescript
   import { Attachment, CardFactory } from 'botbuilder';

   export function createDynamicSearchCard(): Attachment {
        return CardFactory.adaptiveCard({
            $schema: 'http://adaptivecards.io/schemas/adaptive-card.json',
            version: '1.6',
            type: 'AdaptiveCard',
            body: [
                {
                    text: 'Please search for temperature using ZIP Code.',
                    wrap: true,
                    type: 'TextBlock'
                },
                {
                    columns: [
                        {
                            width: 'stretch',
                            items: [
                                {
                                    choices: [],
                                    'choices.data': {
                                        type: 'Data.Query',
                                        dataset: 'locations',
                                        value: 'value'
                                    },
                                    id: 'choiceSelect',
                                    type: 'Input.ChoiceSet',
                                    placeholder: 'ZIP Code',
                                    label: 'ZIP Code search',
                                    isRequired: false,
                                    errorMessage: 'There was an error test.',
                                    isMultiSelect: false,
                                    style: 'filtered'                                
                                }
                            ],
                            type: 'Column'
                        }
                    ],
                    type: 'ColumnSet'
                }
            ],
            actions: [
                {
                    id: 'submitdynamic',
                    type: 'Action.Submit',
                    title: 'Submit',
                    data: {
                        verb: 'DynamicSubmit'
                    }
                }
            ]
        });
    }

   ```

## Tarefa 4: Tratamento de mensagens

**Meta**: desenvolver a capacidade do bot de responder às entradas do usuário e interagir por meio de Cartões Adaptáveis.

Nesta etapa, adicionaremos a funcionalidade de resposta de mensagem ao objeto `app`.

1. Abra o arquivo `app.ts` .
1. Abaixo do código que cria o objeto do aplicativo `const app = new Application();`, adicione o código a seguir para lidar com mensagens diferentes.
    ``` typescript
    interface SubmitData {
        choiceSelect?: string;
    }

    app.message(/static/i, async (context, _state) => {
        const attachment = createStaticSearchCard();
        await context.sendActivity({ attachments: [attachment] });
    });

    app.message(/dynamic/i, async (context, _state) => {
        const attachment = createDynamicSearchCard();
        await context.sendActivity({ attachments: [attachment] });
    });

    // Listen for query from dynamic search card
    app.adaptiveCards.search('locations', async (context: TurnContext, state: TurnState, query) => {
        // Format search results
        const locations: AdaptiveCardSearchResult[] = [];
        // Execute query
        const searchQuery = query.parameters['queryText'] ?? '';
        if (searchQuery.length < 4) {
            return locations;
        }   

        const response = await axios.get(
            `https://zip-api.eu/api/v1/codes/postal_code=US-${searchQuery}*`
        );

        interface DataObject {
            state: string;
            place_name: string;
            postal_code: string;
            lat:string
            lng:string
        };

        //response data is an array of objects or an object
        response.data = Array.isArray(response.data) ? response.data : [response.data];
        response.data.forEach((obj: DataObject) => {
            const result: AdaptiveCardSearchResult = {
                title: `${obj.postal_code} - ${obj.place_name}, ${obj.state}`,
                value: `${obj.postal_code}|${obj.place_name}|${obj.lat}|${obj.lng}`
            };
            locations.push(result);
        });

        // Return search results
        return locations;
    });

    // Listen for submit buttons
    app.adaptiveCards.actionSubmit('DynamicSubmit', async (context, _state, data: SubmitData) => {
        let [postalCode, placeName, lat, lon] = data.choiceSelect?.split('|') ?? [];
        await context.sendActivity(`The dynamically selected place is: ${placeName}`);
        const weatherLocation = await axios.get(
            `https://api.weather.gov/points/${lat},${lon}`
        );
        const forcast = await axios.get(weatherLocation.data.properties.forecast);
        await context.sendActivity(`The weather in ${placeName}: ${forcast.data.properties.periods[0].detailedForecast}`);
        });

        app.adaptiveCards.actionSubmit('StaticSubmit', async (context, _state, data: SubmitData) => {
        await context.sendActivity(`The statically selected option is: ${data.choiceSelect}`);
        });

        // Listen for ANY message to be received. MUST BE AFTER ANY OTHER HANDLERS
        app.activity(ActivityTypes.Message, async (context, _state) => {
        await context.sendActivity(`Try saying "static search" or "dynamic search".`);
    });
    ```

1. Na parte superior do arquivo `app.ts`, atualize as referências relacionadas conforme o seguinte. O arquivo final de `app.ts` pode ser referenciado em [app.ts](../../../Allfiles/Labs/Guided-Exercise6/app.ts).
    
    Atualizado
    ```` typescript
    import { ActivityTypes, TurnContext } from "botbuilder";
    import { createStaticSearchCard } from "./cards/staticSearchCard";
    import { createDynamicSearchCard } from "./cards/dynamicSearchCard";
    import axios from "axios";
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application, TurnState, AdaptiveCardSearchResult } from "@microsoft/teams-ai";
    ````
    Anterior
    ```` typescript
    // See https://aka.ms/teams-ai-library to learn more about the Teams AI library.
    import { Application } from "@microsoft/teams-ai";
    ````    

## Verifique seu trabalho

Execute o seu aplicativo localmente para testar a funcionalidade:

1. Abra o painel do **KIT DE FERRAMENTAS DO TEAMS**. No menu **DESENVOLVIMENTO**, selecione **Pré-visualizar o seu aplicativo do Teams** (ou use a chave `F5`) e, em seguida, selecione **Depurar no Teams ()** com o navegador de sua preferência.  
1. O Kit de Ferramentas do Teams provisionará e executará o seu aplicativo localmente em um navegador.
1. Na caixa de diálogo de instalação do aplicativo no navegador, selecione **Adicionar** para instalar o seu aplicativo do Teams.  O Teams abre uma conversa com o bot instalado.
1. Na mensagem da caixa de diálogo, insira `static search` e pressione Enter. O bot retornará um Cartão Adaptável.
1. Na caixa de entrada, selecione ou insira um nome de estado e selecione o botão **Enviar**. O bot retornará a abreviação desse nome de estado. ![Captura de tela da pesquisa estátida do cartão adaptável](../../media/static-search.png)
1. Na mensagem da caixa de diálogo, insira `dynamic search` e pressione Enter.
1. Na caixa de diálogo de entrada, insira um CEP dos EUA e selecione o botão **Enviar**. O bot retornará o clima atual para essa área. ![Captura de tela da pesquisa dinâmica do cartão adaptável](../../media/dynamic-search.png)