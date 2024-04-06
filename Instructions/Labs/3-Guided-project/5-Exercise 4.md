---
lab:
  title: Criar um bot
  module: Exercise 4
---

# Exercício 4: Criar um bot

## Cenário

Suponha que a equipe de suporte de TI para a qual você está prestando suporte receba um alto volume de consultas comuns e repetitivas de funcionários em toda a organização. Essas consultas geralmente envolvem problemas simples, como redefinições de senha, instruções de instalação de software ou solução de problemas de erros comuns.

Para simplificar o processo e reduzir a carga de trabalho em sua equipe, você decide criar um bot que possa lidar com essas consultas comuns no Microsoft Teams.

Você decide adicionar um comando inicial ao bot chamado "resetPassword". Quando um usuário digita esse comando, o bot responde com instruções passo a passo sobre como redefinir a senha. Isso permite que os usuários resolvam seus problemas sem precisar entrar em contato diretamente com a equipe de Suporte de TI, liberando a sua equipe para lidar com problemas mais complexos.

Além do comando "resetPassword", você planeja adicionar mais comandos para lidar com outras consultas comuns, transformando o bot em uma ferramenta de autoatendimento abrangente para os funcionários da organização.

## Tarefas de exercício

Você precisa concluir as seguintes tarefas para concluir o exercício:

1. Criar o bot usando o Kit de Ferramentas do Teams
2. Configurar o manifesto
3. Criar um cartão adaptável
4. Manipular o comando

**Tempo de conclusão estimado:** 17 minutos

## Tarefa 1: Criar um bot usando o Kit de Ferramentas do Teams

Usar o modelo de Bot de Comando para criar um novo bot:

1. Abra o Visual Studio Code.
1. Na barra lateral, selecione o ícone **Microsoft Teams** para abrir o painel **Kit de Ferramenta do Teams**.
1. Clique no botão **Criar um Aplicativo**.
1. No menu **Novo projeto**, selecione **Bot** e, em seguida, selecione **Comando de chat** para criar um bot de comando.
1. Em Linguagem de programação, selecione **TypeScript**.
1. Em **Pasta do workspace**, selecione ou crie uma pasta para armazenar seus arquivos de projeto em seu computador.
1. Em **Nome do aplicativo**, insira **SupportCommandBot** e pressione **Enter**. O Kit de Ferramentas do Teams fará o scaffold de um novo aplicativo e abrirá a pasta do projeto no Visual Studio Code.
1. Você pode receber uma mensagem do Visual Studio Code perguntando se você confia nos autores dos arquivos nesta pasta. Selecione o botão **Sim, confio nos autores** para continuar.
1. Examinar os diretórios e arquivos do projeto usando o Explorer no Visual Studio Code para se familiarizar com o código-fonte.

## Tarefa 2: Configurar o manifesto

Defina um comando `ResetPassword` no manifesto do aplicativo:

1. Abra o arquivo `manifest.json` da pasta `appPackage`.
2. No objeto `bots`, localize `commandLists`.  Atualmente, há um único comando intitulado `helloWorld`.
3. Substitua `commands` pelo código a seguir, para que ele inclua um novo comando `ResetPassword` da seguinte maneira:

    ```typescript
            "commands": [
                {
                    "title": "helloWorld",
                    "description": "A helloworld command to send a welcome message"
                },
                {
                    "title": "resetPassword",
                    "description": "Request instructions to reset your password"
                }
            ]
    ```

## Tarefa 3: Criar um cartão adaptável

Crie um cartão adaptável a ser enviado em resposta ao comando `ResetPassword`:

1. Em `src`/`adaptiveCards`, crie um novo arquivo chamado `resetPassword.json`.
2. Adicione o seguinte conteúdo ao arquivo e salve-o:

   ```json
        {
            "type": "AdaptiveCard",
            "body": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Reset Password Instructions"
                },
                {
                    "type": "TextBlock",
                    "text": "1. Navigate to the login page and select 'Forgot Password'.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "2. Enter your email or username and select 'Submit'.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "3. Check your email for a password reset link and select it.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "4. Enter and confirm your new password, then select 'Save'.",
                    "wrap": true
                },
                {
                    "type": "TextBlock",
                    "text": "5. Log in with your new password.",
                    "wrap": true
                }
            ],
            "actions": [
                {
                    "type": "Action.OpenUrl",
                    "title": "Go to Login Page",
                    "url": "https://www.adaptivecards.io/designer/"
                }
            ],
            "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
            "version": "1.4"
        }
   ```

## Tarefa 4: Manipular o comando

Em seguida, manipule o comando no código-fonte do bot usando a classe `TeamsFxBotCommandHandler`.  Importe o `resetPasswordCard` do arquivo JSON e renderize-o quando o comando for invocado:

1. Na pasta `src`, crie um novo arquivo chamado `resetPasswordCommandHandler.ts`.
2. Adicione as seguintes instruções de importação ao arquivo, incluindo uma instrução para importar o cartão bruto `resetPassword` que você criou:

   ```typescript
   import { Activity, CardFactory, MessageFactory, TurnContext } from "botbuilder";
    import { CommandMessage, TeamsFxBotCommandHandler, TriggerPatterns } from "@microsoft/teamsfx";
    import { AdaptiveCards } from "@microsoft/adaptivecards-tools";
    import rawResetPasswordCard from "./adaptiveCards/resetPassword.json";
   ```

3. Abaixo das instruções de importação, adicione o seguinte código para implementar o manipulador de comandos e salve o arquivo:

   ```typescript
       export class ResetPasswordCommandHandler implements TeamsFxBotCommandHandler {
          triggerPatterns: TriggerPatterns = "resetPassword";
        
          async handleCommandReceived(
            context: TurnContext,
            message: CommandMessage
          ): Promise<string | Partial<Activity> | void> {
            console.log(`App received message: ${message.text}`);
        
            const resetPasswordCard = AdaptiveCards.declareWithoutData(rawResetPasswordCard).render();
            await context.sendActivity({ attachments: [CardFactory.adaptiveCard(resetPasswordCard)] });
          }
        }
   ```

## Tarefa 5: Registrar o novo comando

Cada novo comando precisa ser configurado no `ConversationBot`, que alimenta o fluxo de conversação do modelo de bot de comando.

1. Navegue até o arquivo `src/internal/initialize.ts`.
2. Adicione a seguinte instrução de importação na linha 2:

    `import { ResetPasswordCommandHandler } from "../resetPasswordCommandHandler";`
3. Na linha 20, atualize a matriz `commands` da propriedade `command` para incluir uma instrução para inicializar o novo manipulador: o objeto `new ResetPasswordCommandHandler().  The updated `command` deve ser o seguinte:

   ```json
   command: {    enabled: true,    commands: [new HelloWorldCommandHandler(), new ResetPasswordCommandHandler()],  },
    ```

## Tarefa 6: Alternar para ngrok do túnel do desenvolvedor (opcional)

Se o ambiente de desenvolvimento não der suporte ao túnel do desenvolvedor do Kit de Ferramentas do Teams, você poderá substituir o túnel do desenvolvedor por ngrok.

1. Para instalar o ngrok, siga estas etapas:
   1. Acesse o [site do ngrok](https://ngrok.com/) e inscreva-se em uma conta.
   1. Baixe o executável ngrok para seu sistema operacional.
   1. Extraia o arquivo baixado para um diretório de sua escolha.
   1. Para o ambiente do Windows, adicione o diretório onde `ngrok.exe` está localizado à variável de ambiente PATH do sistema 
      ```powershell
      setx PATH "$Env:path;<ngrok_full_path>"
      ```
      _No ambiente do PowerShell, substitua `<ngrok_full_path>` pelo caminho localizado `ngrok.exe`._
      > Para aplicar essa alteração de variável de ambiente, você precisa reiniciar os terminais e o **Visual Studio Code** para o projeto atual.

   1. Abra um terminal ou prompt de comando e execute o seguinte comando para autenticar sua conta do ngrok:
      ```shell
      ngrok config add-authtoken <your_auth_token>
      ```
      _Substitua `<your_auth_token>` pelo token de autenticação fornecido no site do ngrok._
   1. Para iniciar um túnel na porta 3978, execute o seguinte comando:
      ```shell
      ngrok http 3978
      ```
   1. O ngrok gerará uma URL de encaminhamento que você pode usar para acessar seu aplicativo pela Internet.
      ```shell
      Forwarding      http://<random_string>.ngrok-free.app -> http://localhost:3978
      ```
   1. Clique em `Ctrl + C` para desconectar o túnel ngrok.
1. Navegue até a pasta `.vscode` e abra o arquivo `task.json`. Atualize a tarefa `Start local tunnel`:
   ```json
    {
        "label": "Start local tunnel",
        "type": "shell",
        "command": "ngrok http 3978 --log=stdout --log-format=logfmt",
        "isBackground": true,
        "problemMatcher": {
            "pattern": [
                {
                    "regexp": "^.*$",
                    "file": 0,
                    "location": 1,
                    "message": 2
                }
            ],
            "background": {
                "activeOnStart": true,
                "beginsPattern": "starting web service",
                "endsPattern": "started tunnel|failed to reconnect session"
            }
        }
    }
   ```
1. Navegue até o arquivo `teamsapp.local.yml` na pasta raiz. Adicionar a ação a seguir na primeira etapa do ciclo de vida de provisionamento
   - Windows
     ```yml
     provision:
       - uses: script
         with:
           shell: powershell
           run: |
                for ($i = 1; $i -le 10; $i++) {
                    $endpoint = (Invoke-WebRequest -Uri "http://localhost:4040/api/tunnels" | Select-String -Pattern 'https://[a-zA-Z0-9 -\.]*\.ngrok-free\.app').Matches.Value
                    if ($endpoint) {
                        break
                    }
                    sleep 10
                }
                if (-not $endpoint) {
                    echo "ERROR: Failed to find tunnel endpoint after 10 attempts."
                    exit 1
                } else {
                    echo "::set-teamsfx-env BOT_ENDPOINT=$endpoint"
                    echo "::set-teamsfx-env BOT_DOMAIN=$($endpoint.Substring(8))"
                }
     ```
   - Linux e macOS
     ```yml
     provision:
        - uses: script
            with:
            run: |
                for i in {1..10}; do
                    endpoint=$(curl -s localhost:4040/api/tunnels | grep -o 'https://[a-zA-Z0-9 -\.]*\.ngrok-free\.app')
                    if [ -n "$endpoint" ]; then
                        break
                    fi
                    sleep 10
                done
                if [ -z "$endpoint" ]; then
                    echo "ERROR: Failed to find tunnel endpoint after 10 attempts."
                    exit 1
                else
                    echo "::set-teamsfx-env BOT_ENDPOINT=$endpoint"
                    echo "::set-teamsfx-env BOT_DOMAIN=${endpoint:8}"
                fi
     ```
## Verifique seu trabalho

Execute o seu aplicativo localmente para testar a funcionalidade:

1. Abra o painel do **KIT DE FERRAMENTAS DO TEAMS**. No menu **DESENVOLVIMENTO**, selecione **Pré-visualizar o seu aplicativo do Teams** (ou use a chave `F5`) e, em seguida, selecione **Depurar no Teams ()** com o navegador de sua preferência.  
2. O Kit de Ferramentas do Teams provisionará e executará o seu aplicativo localmente em um navegador.
3. Na caixa de diálogo de instalação do aplicativo no navegador, selecione **Adicionar** para instalar o seu aplicativo do Teams.  O Teams abre uma conversa com o bot instalado.
4. Insira ou selecione o comando `resetPassword`.
5. Verifique se o bot responde com um cartão adaptável que contém instruções de redefinição de senha.
