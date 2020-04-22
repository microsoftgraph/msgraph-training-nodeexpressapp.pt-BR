<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3117d-101">Este tutorial ensina como criar um aplicativo Web node. js Express que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="3117d-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="3117d-102">Se preferir baixar o tutorial concluído, você pode baixá-lo de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="3117d-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="3117d-103">Baixe o [início rápido do node. js](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obter o código de trabalho em minutos.</span><span class="sxs-lookup"><span data-stu-id="3117d-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="3117d-104">Baixe ou clone o [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="3117d-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3117d-105">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="3117d-105">Prerequisites</span></span>

<span data-ttu-id="3117d-106">Antes de iniciar esta demonstração, você deve ter o [node. js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="3117d-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="3117d-107">Se você não tiver o Node. js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="3117d-107">If you do not have Node.js, visit the previous link for download options.</span></span>

<span data-ttu-id="3117d-108">Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3117d-108">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="3117d-109">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="3117d-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="3117d-110">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="3117d-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="3117d-111">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="3117d-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="3117d-112">Este tutorial foi escrito com o nó versão 12.6.1.</span><span class="sxs-lookup"><span data-stu-id="3117d-112">This tutorial was written with Node version 12.6.1.</span></span> <span data-ttu-id="3117d-113">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="3117d-113">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="watch-the-tutorial"></a><span data-ttu-id="3117d-114">Assista ao tutorial</span><span class="sxs-lookup"><span data-stu-id="3117d-114">Watch the tutorial</span></span>

<span data-ttu-id="3117d-115">Este módulo foi gravado e está disponível no canal do Office Development YouTube.</span><span class="sxs-lookup"><span data-stu-id="3117d-115">This module has been recorded and is available in the Office Development YouTube channel.</span></span>

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/n6q8Cm-pTYY]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a><span data-ttu-id="3117d-116">Comentários</span><span class="sxs-lookup"><span data-stu-id="3117d-116">Feedback</span></span>

<span data-ttu-id="3117d-117">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="3117d-117">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>
