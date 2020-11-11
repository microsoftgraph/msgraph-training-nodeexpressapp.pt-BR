<!-- markdownlint-disable MD002 MD041 -->

Este tutorial ensina como criar um aplicativo Web do Node.js Express que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.

> [!TIP]
> Se preferir baixar o tutorial concluído, você pode baixá-lo de duas maneiras.
>
> - Baixe o [Node.js início rápido](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obter o código de trabalho em minutos.
> - Baixe ou clone o [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar esta demonstração, você deve ter o [Node.js](https://nodejs.org) instalado em sua máquina de desenvolvimento. Se você não tiver Node.js, visite o link anterior para opções de download.

> [!NOTE]
> Os usuários do Windows podem precisar instalar as ferramentas de desenvolvimento Python e Visual Studio para suportar módulos NPM que precisam ser compilados do C/C++. O Node.js Installer no Windows oferece uma opção para instalar essas ferramentas automaticamente. Como alternativa, você pode seguir as instruções em [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) .

Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft. Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

> [!NOTE]
> Este tutorial foi escrito com o nó versão 12.18.4. As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.

## <a name="feedback"></a>Comentários

Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).
