<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7deef-101">Este tutorial ensina como criar um aplicativo Web node. js Express que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="7deef-101">This tutorial teaches you how to build a Node.js Express web app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="7deef-102">Se preferir baixar o tutorial concluído, você pode baixá-lo de duas maneiras.</span><span class="sxs-lookup"><span data-stu-id="7deef-102">If you prefer to just download the completed tutorial, you can download it in two ways.</span></span>
>
> - <span data-ttu-id="7deef-103">Baixe o [início rápido do node. js](https://developer.microsoft.com/graph/quick-start?platform=option-node) para obter o código de trabalho em minutos.</span><span class="sxs-lookup"><span data-stu-id="7deef-103">Download the [Node.js quick start](https://developer.microsoft.com/graph/quick-start?platform=option-node) to get working code in minutes.</span></span>
> - <span data-ttu-id="7deef-104">Baixe ou clone o [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="7deef-104">Download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7deef-105">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7deef-105">Prerequisites</span></span>

<span data-ttu-id="7deef-106">Antes de iniciar esta demonstração, você deve ter o [node. js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="7deef-106">Before you start this demo, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="7deef-107">Se você não tiver o Node. js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="7deef-107">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="7deef-108">Este tutorial foi escrito com o nó versão 10.15.3.</span><span class="sxs-lookup"><span data-stu-id="7deef-108">This tutorial was written with Node version 10.15.3.</span></span> <span data-ttu-id="7deef-109">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="7deef-109">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="7deef-110">Comentários</span><span class="sxs-lookup"><span data-stu-id="7deef-110">Feedback</span></span>

<span data-ttu-id="7deef-111">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span><span class="sxs-lookup"><span data-stu-id="7deef-111">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-nodeexpressapp).</span></span>