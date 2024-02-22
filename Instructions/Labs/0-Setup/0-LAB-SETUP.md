# Configuração do Laboratório

Conclua as tarefas a seguir para preparar o seu ambiente de desenvolvimento antes de concluir os laboratórios.

## Pré-requisitos do laboratório

Você precisa das seguintes ferramentas para concluir os laboratórios deste curso:

- Acesso de administrador a um locatário do Microsoft 365.
- Uma assinatura do Azure.
- Visual Studio Code.
- Extensão do Visual Studio Code do Kit de Ferramentas do Teams:  Versão 5.0.0 ou superior. (Você realizará esta instalação durante o laboratório)
- O cliente do Microsoft Teams (corporativo ou de estudante), ou acesso ao Microsoft Teams através de um navegador.
- Versão 16.14.2 do Node.js.

## Instale o nvm-windows

Você usará essa ferramenta para instalar o Node.js e, se necessário, alternar as versões do Node para seus projetos.

1. Em um navegador da Web, acesse [https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases).
2. Localize a versão mais recente e selecione o arquivo **nvm-setup.zip** para baixar.  O arquivo será baixado no seu computador.
3. Abra a pasta do arquivo e **extraia** o conteúdo da pasta zip em um diretório no seu computador.
4. Na nova pasta, abra o arquivo **nvm-setup.exe** para iniciar a instalação.
5. Siga as solicitações do instalador para instalar a ferramenta usando as opções padrão.
6. O nvm para Windows será instalado no seu computador.

## Instalar o Node.js

Instale a versão 16.14.2 do Node.js, que é compatível com todas as soluções neste curso.

1. Abra o aplicativo **Prompt de Comando**.
2. Insira o comando `nvm install 16.14.2` para instalar o Node.js.
3. A saída do nvm deve confirmar a conclusão da instalação.
4. Execute o comando `nvs use 16.14.2` para usar esta versão do Node.js.
5. Execute o comando `node -v` para confirmar se você tem a versão 16.14.2 instalada.

Você instalou e configurou a versão 16.14.2 do Node.js

## Assinatura do Azure

Observe que, se você recebeu informações de login do Azure, um grupo de recursos já foi criado para você.  Nas tarefas de provisionamento dos laboratórios, quando for solicitado para "selecionar um grupo de recursos ou criar um novo", **escolha o grupo de recursos fornecido**.

## Depurar aplicativos do Teams

Ao depurar seu aplicativo do Teams localmente, talvez seja necessário instalar um certificado de desenvolvimento no localhost.  Você precisará fazer isso para depurar localmente.

Quando solicitado, selecione **Instalar**.

![Captura de tela da solicitação para instalar o certificado de desenvolvimento.](../../media/install-certificate.png)

Em seguida, selecione **Sim** na caixa de diálogo Aviso de Segurança.

![Captura de tela da caixa de diálogo de segurança.](../../media/development-certificate.png)

Acesse a documentação do Teams para saber mais: [Depurar seu aplicativo do Teams localmente](https://learn.microsoft.com/microsoftteams/platform/toolkit/debug-local?tabs=Windows&pivots=visual-studio-code-v5)
