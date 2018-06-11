---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia
author: camsoper
description: Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu klienta wiersza polecenia Git.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 3fc068096a4b8696340787aa15120a2f97d10164
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252441"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="27a0d-103">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="27a0d-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="27a0d-104">Przez [Soper kamery](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="27a0d-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="27a0d-105">Ten samouczek przedstawia sposób tworzenia i wdrażania aplikacji platformy ASP.NET Core w usłudze Microsoft Azure App Service przy użyciu narzędzia wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="27a0d-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="27a0d-106">Po zakończeniu będziesz mieć stron Razor, aplikacji sieci web platformy ASP.NET Core wbudowane hostowany jako aplikacji sieci Web platformy Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="27a0d-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="27a0d-107">W tym samouczku zostały utworzone za pomocą narzędzia wiersza polecenia systemu Windows, ale można zastosować do macOS i środowisk Linux, jak również.</span><span class="sxs-lookup"><span data-stu-id="27a0d-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="27a0d-108">Z tego samouczka, dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="27a0d-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27a0d-109">Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="27a0d-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="27a0d-110">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git</span><span class="sxs-lookup"><span data-stu-id="27a0d-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27a0d-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="27a0d-111">Prerequisites</span></span>

<span data-ttu-id="27a0d-112">Do ukończenia tego samouczka będą potrzebne:</span><span class="sxs-lookup"><span data-stu-id="27a0d-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="27a0d-113">A [subskrypcji Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="27a0d-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="27a0d-114">[Git](https://www.git-scm.com/) klient wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="27a0d-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="27a0d-115">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="27a0d-115">Create a web app</span></span>

<span data-ttu-id="27a0d-116">Utwórz nowy katalog dla aplikacji sieci web, Utwórz nową aplikację ASP.NET Core Razor strony, a następnie uruchom witrynę sieci Web lokalnie.</span><span class="sxs-lookup"><span data-stu-id="27a0d-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="27a0d-117">Windows</span><span class="sxs-lookup"><span data-stu-id="27a0d-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

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

# <a name="othertabother"></a>[<span data-ttu-id="27a0d-119">Inne</span><span class="sxs-lookup"><span data-stu-id="27a0d-119">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

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

<span data-ttu-id="27a0d-122">Testowanie aplikacji, przechodząc do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="27a0d-122">Test the app by browsing to `http://localhost:5000`.</span></span>

![Witryny sieci Web uruchomionej na komputerze lokalnym](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="27a0d-124">Utwórz wystąpienie usługi aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="27a0d-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="27a0d-125">Przy użyciu [powłoki chmury Azure](/azure/cloud-shell/quickstart), Utwórz grupę zasobów, plan usługi aplikacji i aplikacji sieci web usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27a0d-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="27a0d-126">Przed przystąpieniem do wdrożenia należy ustawić poświadczenia wdrażania na poziomie konta przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="27a0d-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="27a0d-127">Adres URL wdrożenia jest wymagany do wdrażania aplikacji przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="27a0d-127">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="27a0d-128">Pobiera adres URL podobny do tego.</span><span class="sxs-lookup"><span data-stu-id="27a0d-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="27a0d-129">Zanotuj adres URL wyświetlany w `.git`.</span><span class="sxs-lookup"><span data-stu-id="27a0d-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="27a0d-130">Jest on używany w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="27a0d-130">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="27a0d-131">Wdrażanie aplikacji przy użyciu narzędzia Git</span><span class="sxs-lookup"><span data-stu-id="27a0d-131">Deploy the app using Git</span></span>

<span data-ttu-id="27a0d-132">Wszystko jest gotowe do wdrożenia z komputera lokalnego przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="27a0d-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="27a0d-133">Jest bezpiecznie zignorować ostrzeżenia z repozytorium Git o zakończenia wierszy.</span><span class="sxs-lookup"><span data-stu-id="27a0d-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="27a0d-134">Windows</span><span class="sxs-lookup"><span data-stu-id="27a0d-134">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="27a0d-135">Inne</span><span class="sxs-lookup"><span data-stu-id="27a0d-135">Other</span></span>](#tab/other)

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

<span data-ttu-id="27a0d-136">Git wyświetla monit o poświadczenia wdrażania, które zostały określone wcześniej.</span><span class="sxs-lookup"><span data-stu-id="27a0d-136">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="27a0d-137">Po uwierzytelnieniu aplikacji będzie można przeniesiony do lokalizacji zdalnej, wbudowane i wdrożone.</span><span class="sxs-lookup"><span data-stu-id="27a0d-137">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Dane wyjściowe wdrażanie Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="27a0d-139">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="27a0d-139">Test the app</span></span>

<span data-ttu-id="27a0d-140">Testowanie aplikacji, przechodząc do `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="27a0d-140">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="27a0d-141">Aby wyświetlić adres w chmurze powłoki (lub wiersza polecenia platformy Azure), użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="27a0d-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikacji działających na platformie Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="27a0d-143">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="27a0d-143">Clean up</span></span>

<span data-ttu-id="27a0d-144">Po zakończeniu testowania aplikacji i zapoznanie się z kodu i zasobów, należy usunąć aplikacji sieci web i planu poprzez usunięcie grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="27a0d-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="27a0d-145">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="27a0d-145">Next steps</span></span>

<span data-ttu-id="27a0d-146">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="27a0d-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27a0d-147">Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="27a0d-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="27a0d-148">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git</span><span class="sxs-lookup"><span data-stu-id="27a0d-148">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="27a0d-149">Następnie Dowiedz się, jak wdrożyć istniejącej aplikacji sieci web, która używa CosmosDB za pomocą wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="27a0d-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="27a0d-150">Wdrażanie na platformie Azure z poziomu wiersza polecenia z platformą .NET Core</span><span class="sxs-lookup"><span data-stu-id="27a0d-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
