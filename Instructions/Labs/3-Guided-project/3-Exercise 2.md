---
lab:
  title: Criar um webhook de entrada
  module: Exercise 2
---

# Exercício 2: Criar um webhook de entrada

## Cenário

Suponha que a equipe de suporte de TI use um serviço de notificação de terceiros para gerenciar alertas e mensagens. Recentemente, a equipe decidiu automatizar o processo de postagem de mensagens em um canal do Teams que é usado para atualizações críticas.  O serviço de terceiros está sendo projetado para postar mensagens por meio de um webhook.  

## Tarefas de exercício

Sua tarefa é criar um novo Webhook de entrada, chamado **Alertas**, para receber essas mensagens.  Você também deve testar o webhook para garantir que ele possa aceitar e exibir uma mensagem com a cadeia de caracteres `"Testing the Alerts endpoint."` corretamente. A equipe atualizará o serviço com a URL do ponto de extremidade do webhook quando você concluir suas tarefas.

Execute as seguintes tarefas para concluir o exercício:

1. Registre um webhook de entrada.
2. Poste uma mensagem para testar o webhook.

**Tempo de conclusão estimado:** 8 minutos

## Tarefa 1: Registrar um webhook de entrada

Primeiro, registre um webhook de entrada.

**Observação:** Se a conta do Teams que você está usando para este exercício ainda não tiver uma equipe com um canal, crie um novo canal antes de concluir as etapas a seguir.

1. No Microsoft Teams, navegue até um canal onde você possa configurar o webhook.
2. No canal, selecione o menu **Mais opções** e selecione **Conectores**.  (Observação: use o menu dentro do canal, não o menu da lista de canais.)
3. Pesquise por `"webhook"` e selecione **Webhook de entrada**.

   ![Captura de tela do webhook na barra de pesquisa.](../../media/add-incoming-webhook.png)

4. Selecione **Adicionar**.
5. Na página de visão geral, selecione **Adicionar**.
6. No canal, selecione o menu **Mais opções** novamente e selecione **Conectores**.
7. Ao lado do **Webhook de entrada**, selecione **Configurar**.
8. Para o nome, insira **Alertas**.
9. Selecione **Criar**.  Deixe esta janela aberta para que você possa copiar a URL durante a próxima tarefa.

Você configurou um webhook de entrada no canal.

## Tarefa 2: Postar uma mensagem para testar o webhook

Para testar o webhook, use o PowerShell para enviar uma mensagem para o ponto de extremidade do webhook.

1. Abra o **PowerShell**.
2. Execute o comando a seguir para enviar a mensagem.  Substitua <YOUR WEBHOOK URL> pela URL da janela de configuração do webhook no Teams da tarefa anterior:

     ```powershell
     Invoke-RestMethod -Method post -ContentType 'Application/Json' -Body '{"text":"Testing the Alerts endpoint."}' -Uri <YOUR WEBHOOK URL>
    ```

## Verifique seu trabalho

1. No cliente do Microsoft Teams, navegue até a guia **Conversas** do canal configurado.
2. Verifique a presença de uma mensagem no canal de `Alerts` em que se lê `"Testing the Alerts endpoint"`.

 ![Captura de tela da exibição Permissões Configuradas no Portal do Azure.](../../media/final-alert-message.png)
