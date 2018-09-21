---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia
author: camsoper
description: Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service za pomocą klienta wiersza polecenia usługi Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 63a313de786b1f89e84c594cbd665d1b230e4ba3
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523249"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="b579c-103">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="b579c-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="b579c-104">Przez [Soper kamery](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b579c-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="b579c-105">W tym samouczku będzie pokazują, jak tworzenie i wdrażanie aplikacji platformy ASP.NET Core w usłudze Microsoft Azure App Service przy użyciu narzędzia wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b579c-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="b579c-106">Po zakończeniu będziesz mieć stron Razor, aplikację sieci web w programie ASP.NET Core jest hostowany jako aplikacja sieci Web usługi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b579c-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="b579c-107">W tym samouczku są zapisywane przy użyciu narzędzia wiersza polecenia Windows, ale mogą być stosowane do systemu macOS i Linux środowisk, jak również.</span><span class="sxs-lookup"><span data-stu-id="b579c-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="b579c-108">W tym samouczku dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="b579c-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b579c-109">Tworzenie witryny sieci Web usługi Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="b579c-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b579c-110">Wdrażanie aplikacji ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia usługi Git</span><span class="sxs-lookup"><span data-stu-id="b579c-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b579c-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b579c-111">Prerequisites</span></span>

<span data-ttu-id="b579c-112">Do ukończenia tego samouczka będą potrzebne:</span><span class="sxs-lookup"><span data-stu-id="b579c-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="b579c-113">A [subskrypcji Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="b579c-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="b579c-114">[Git](https://www.git-scm.com/) klienta wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="b579c-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="b579c-115">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="b579c-115">Create a web app</span></span>

<span data-ttu-id="b579c-116">Utwórz nowy katalog dla aplikacji sieci web, Utwórz nową aplikację ASP.NET Core Razor strony, a następnie uruchom lokalnie witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b579c-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b579c-117">Windows</span><span class="sxs-lookup"><span data-stu-id="b579c-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="b579c-118">Inne</span><span class="sxs-lookup"><span data-stu-id="b579c-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Dane wyjściowe wiersza polecenia](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="b579c-120">Testowanie aplikacji, przechodząc do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b579c-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Witryny sieci Web uruchomionej na komputerze lokalnym](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="b579c-122">Tworzenie wystąpienia usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b579c-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="b579c-123">Za pomocą [usługi Azure Cloud Shell](/azure/cloud-shell/quickstart), Utwórz grupę zasobów, plan usługi App Service i aplikację sieci web usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="b579c-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="b579c-124">Przed przystąpieniem do wdrożenia należy ustawić poświadczenia wdrażania na poziomie konta, za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b579c-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="b579c-125">Adres URL wdrożenia jest wymagane do wdrażania aplikacji przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="b579c-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="b579c-126">Pobierz adres URL następująco.</span><span class="sxs-lookup"><span data-stu-id="b579c-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="b579c-127">Zanotuj adres URL wyświetlany w `.git`.</span><span class="sxs-lookup"><span data-stu-id="b579c-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="b579c-128">Jest on używany w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="b579c-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="b579c-129">Wdrażanie aplikacji przy użyciu narzędzia Git</span><span class="sxs-lookup"><span data-stu-id="b579c-129">Deploy the app using Git</span></span>

<span data-ttu-id="b579c-130">Wszystko gotowe do wdrożenia na maszynie lokalnej przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="b579c-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="b579c-131">Jest to bezpieczne Zignoruj wszelkie ostrzeżenia z repozytorium Git o zakończeniu linii.</span><span class="sxs-lookup"><span data-stu-id="b579c-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b579c-132">Windows</span><span class="sxs-lookup"><span data-stu-id="b579c-132">Windows</span></span>](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="b579c-133">Inne</span><span class="sxs-lookup"><span data-stu-id="b579c-133">Other</span></span>](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

<span data-ttu-id="b579c-134">Git wyświetla monit o poświadczenia wdrażania, które zostały ustawione wcześniej.</span><span class="sxs-lookup"><span data-stu-id="b579c-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="b579c-135">Po uwierzytelnieniu, aplikacja będzie można wypchnięte do lokalizacji zdalnej, utworzone i wdrożone.</span><span class="sxs-lookup"><span data-stu-id="b579c-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Dane wyjściowe wdrożenia narzędzia Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="b579c-137">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b579c-137">Test the app</span></span>

<span data-ttu-id="b579c-138">Testowanie aplikacji, przechodząc do `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="b579c-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="b579c-139">Aby wyświetlić adres w usłudze Cloud Shell (lub wiersza polecenia platformy Azure), użyj następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="b579c-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikacji działających na platformie Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="b579c-141">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="b579c-141">Clean up</span></span>

<span data-ttu-id="b579c-142">Po zakończeniu testowania aplikacji i zapoznanie się z kodu i zasobów aplikacji internetowej i planu można usunąć przez usunięcie grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="b579c-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="b579c-143">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b579c-143">Next steps</span></span>

<span data-ttu-id="b579c-144">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="b579c-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b579c-145">Tworzenie witryny sieci Web usługi Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="b579c-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b579c-146">Wdrażanie aplikacji ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia usługi Git</span><span class="sxs-lookup"><span data-stu-id="b579c-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="b579c-147">Dowiedz się używać wiersza polecenia do wdrażania istniejącej aplikacji sieci web, która używa bazy danych cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b579c-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b579c-148">Wdrażanie na platformie Azure z poziomu wiersza polecenia za pomocą programu .NET Core</span><span class="sxs-lookup"><span data-stu-id="b579c-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
