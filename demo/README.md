# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="42332-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="42332-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42332-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="42332-102">Prerequisites</span></span>

<span data-ttu-id="42332-103">Para executar o projeto concluído nesta pasta, você precisa do seguinte:</span><span class="sxs-lookup"><span data-stu-id="42332-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="42332-104">[Node.js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="42332-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="42332-105">Se você não tiver Node.js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="42332-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="42332-106">(**Observação:** este tutorial foi escrito com o Nó versão 14.15.0.</span><span class="sxs-lookup"><span data-stu-id="42332-106">(**Note:** This tutorial was written with Node version 14.15.0.</span></span> <span data-ttu-id="42332-107">As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)</span><span class="sxs-lookup"><span data-stu-id="42332-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="42332-108">Uma conta pessoal da Microsoft com uma caixa de correio em Outlook.com ou uma conta de estudante ou de trabalho da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="42332-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="42332-109">Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="42332-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="42332-110">Você pode [se inscrever em uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="42332-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="42332-111">Você pode [se inscrever no programa Office 365 desenvolvedor para](https://developer.microsoft.com/office/dev-program) obter uma assinatura Office 365 gratuita.</span><span class="sxs-lookup"><span data-stu-id="42332-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="42332-112">Registrar um aplicativo Web com o Azure Active Directory de administração</span><span class="sxs-lookup"><span data-stu-id="42332-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="42332-113">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="42332-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="42332-114">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="42332-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="42332-115">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="42332-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="42332-116">Uma captura de tela dos registros do aplicativo</span><span class="sxs-lookup"><span data-stu-id="42332-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="42332-117">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="42332-117">Select **New registration**.</span></span> <span data-ttu-id="42332-118">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="42332-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="42332-119">Defina **Nome** para `Node.js Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="42332-119">Set **Name** to `Node.js Graph Tutorial`.</span></span>
    - <span data-ttu-id="42332-120">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="42332-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="42332-121">Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Web` e defina o valor como `http://localhost:3000/auth/callback`.</span><span class="sxs-lookup"><span data-stu-id="42332-121">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000/auth/callback`.</span></span>

    ![Captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="42332-123">Escolha **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="42332-123">Choose **Register**.</span></span> <span data-ttu-id="42332-124">Na página **Node.js Graph Tutorial,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dela na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="42332-124">On the **Node.js Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="42332-126">Selecione **Certificados e segredos** sob **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="42332-126">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="42332-127">Selecione o botão **Novo segredo do cliente**.</span><span class="sxs-lookup"><span data-stu-id="42332-127">Select the **New client secret** button.</span></span> <span data-ttu-id="42332-128">Insira um valor em **Descrição**, selecione uma das opções para **Expira** e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="42332-128">Enter a value in **Description** and select one of the options for **Expires** and choose **Add**.</span></span>

    ![Uma captura de tela da caixa de diálogo Adicionar um segredo do cliente](/tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="42332-130">Copie o valor de segredo do cliente antes de sair desta página.</span><span class="sxs-lookup"><span data-stu-id="42332-130">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="42332-131">Você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="42332-131">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="42332-132">Este segredo do cliente nunca é mostrado novamente, portanto, certifique-se de copiá-lo agora.</span><span class="sxs-lookup"><span data-stu-id="42332-132">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Uma captura de tela do segredo do cliente recém-adicionado](/tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="42332-134">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="42332-134">Configure the sample</span></span>

1. <span data-ttu-id="42332-135">Renomeie o `example.env` arquivo para `.env` .</span><span class="sxs-lookup"><span data-stu-id="42332-135">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="42332-136">Edite `.env` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="42332-136">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="42332-137">Substitua `YOUR_CLIENT_SECRET_HERE` pela **ID do aplicativo que** você recebeu do Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="42332-137">Replace `YOUR_CLIENT_SECRET_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="42332-138">Substitua `YOUR_APP_PASSWORD_HERE` pela senha que você recebeu do Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="42332-138">Replace `YOUR_APP_PASSWORD_HERE` with the password you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="42332-139">Em sua interface de linha de comando (CLI), navegue até esse diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="42332-139">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="42332-140">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="42332-140">Run the sample</span></span>

1. <span data-ttu-id="42332-141">Execute o seguinte comando em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="42332-141">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="42332-142">Abra um navegador e navegue até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="42332-142">Open a browser and browse to `http://localhost:3000`.</span></span>
