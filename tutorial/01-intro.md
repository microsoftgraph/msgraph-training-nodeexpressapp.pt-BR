<!-- markdownlint-disable MD002 MD041 -->

Este tutorial ensina como criar um aplicativo Web Node.js Express que usa a API do Microsoft Graph para recuperar informações de calendário para um usuário.

> [!TIP]
> Se você preferir apenas baixar o tutorial concluído, poderá baixá-lo de duas maneiras.
>
> - Baixe o [Node.js início rápido](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obter o código de trabalho em minutos.
> - Baixe ou clone o [GitHub repositório](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar essa demonstração, você deve [ ter ](https://nodejs.org)Node.jsinstalado em sua máquina de desenvolvimento. Se você não tiver Node.js, visite o link anterior para opções de download.

> [!NOTE]
> Windows usuários podem precisar instalar o Python e Ferramentas de Build do Visual Studio para dar suporte a módulos NPM que precisam ser compilados a partir de C/C++. O Node.js instalador no Windows oferece uma opção para instalar automaticamente essas ferramentas. Como alternativa, você pode seguir instruções em [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) .

Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio em Outlook.com, ou uma conta de trabalho ou de estudante da Microsoft. Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever em uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Você pode [se inscrever no programa Office 365 desenvolvedor para](https://developer.microsoft.com/office/dev-program) obter uma assinatura Office 365 gratuita.

> [!NOTE]
> Este tutorial foi escrito com o Nó versão 14.15.0. As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.

## <a name="feedback"></a>Comentários

Forneça qualquer comentário sobre este tutorial no [repositório GitHub .](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp)
