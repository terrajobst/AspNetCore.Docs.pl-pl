---
title: "Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure przy użyciu narzędzia wiersza polecenia | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu klienta wiersza polecenia Git."
services: multiple
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: de05c1688d7de6126434395042103d803ee3064e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="5ceb0-103">Wdrażanie aplikacji platformy ASP.NET Core usłudze Azure App Service z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="5ceb0-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="5ceb0-104">Przez [Soper kamery](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="5ceb0-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="5ceb0-105">Ten samouczek przedstawia sposób tworzenia i wdrażania aplikacji platformy ASP.NET Core w usłudze Microsoft Azure App Service przy użyciu narzędzia wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="5ceb0-106">Po zakończeniu będziesz mieć aplikację sieci web wbudowanych w platformy ASP.NET Core MVC hostowanej jako aplikacji sieci Web platformy Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="5ceb0-107">W tym samouczku zostały utworzone za pomocą narzędzia wiersza polecenia systemu Windows, ale można zastosować do macOS i środowisk Linux, jak również.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="5ceb0-108">Z tego samouczka, dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="5ceb0-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5ceb0-109">Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5ceb0-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="5ceb0-110">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git</span><span class="sxs-lookup"><span data-stu-id="5ceb0-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ceb0-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5ceb0-111">Prerequisites</span></span>

<span data-ttu-id="5ceb0-112">Do ukończenia tego samouczka będą potrzebne:</span><span class="sxs-lookup"><span data-stu-id="5ceb0-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="5ceb0-113">A [subskrypcji Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="5ceb0-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="5ceb0-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ceb0-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="5ceb0-115">[Git](https://www.git-scm.com/) klient wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="5ceb0-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="5ceb0-116">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="5ceb0-116">Create a web application</span></span>

<span data-ttu-id="5ceb0-117">Utwórz nowy katalog dla aplikacji sieci web, Utwórz nową aplikację ASP.NET Core MVC, a następnie uruchom witrynę sieci Web lokalnie.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5ceb0-118">Windows</span><span class="sxs-lookup"><span data-stu-id="5ceb0-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="5ceb0-119">Inne</span><span class="sxs-lookup"><span data-stu-id="5ceb0-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Dane wyjściowe wiersza polecenia](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="5ceb0-121">Przetestuj aplikację, przechodząc do http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-121">Test the application by browsing to http://localhost:5000.</span></span>

![Witryny sieci Web uruchomionej na komputerze lokalnym](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="5ceb0-123">Utwórz wystąpienie usługi aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="5ceb0-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="5ceb0-124">Przy użyciu [powłoki chmury Azure](/azure/cloud-shell/quickstart), Utwórz grupę zasobów, plan usługi aplikacji i aplikacji sieci web usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="5ceb0-125">Przed przystąpieniem do wdrożenia należy ustawić poświadczenia wdrażania na poziomie konta przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5ceb0-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="5ceb0-126">Adres URL wdrożenia jest wymagany do wdrażania aplikacji przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="5ceb0-127">Pobiera adres URL podobny do tego.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="5ceb0-128">Zanotuj adres URL wyświetlany w `.git`.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="5ceb0-129">Jest on używany w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="5ceb0-130">Wdrażanie aplikacji przy użyciu narzędzia Git</span><span class="sxs-lookup"><span data-stu-id="5ceb0-130">Deploy the application using Git</span></span>

<span data-ttu-id="5ceb0-131">Wszystko jest gotowe do wdrożenia z komputera lokalnego przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="5ceb0-132">Jest bezpiecznie zignorować ostrzeżenia z repozytorium Git o zakończenia wierszy.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5ceb0-133">Windows</span><span class="sxs-lookup"><span data-stu-id="5ceb0-133">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="5ceb0-134">Inne</span><span class="sxs-lookup"><span data-stu-id="5ceb0-134">Other</span></span>](#tab/other)
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

<span data-ttu-id="5ceb0-135">Git wyświetla monit o poświadczenia wdrażania, które zostały określone wcześniej.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-135">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="5ceb0-136">Po uwierzytelnieniu aplikacji będzie można przeniesiony do lokalizacji zdalnej, wbudowane i wdrożone.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Dane wyjściowe wdrażanie Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="5ceb0-138">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5ceb0-138">Test the application</span></span>

<span data-ttu-id="5ceb0-139">Przetestuj aplikację, przechodząc do `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="5ceb0-140">Aby wyświetlić adres w chmurze powłoki (lub wiersza polecenia platformy Azure), użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5ceb0-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikacji działających na platformie Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="5ceb0-142">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="5ceb0-142">Clean up</span></span>

<span data-ttu-id="5ceb0-143">Po zakończeniu testowania aplikacji i zapoznanie się z kodu i zasobów, należy usunąć aplikacji sieci web i planu poprzez usunięcie grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="5ceb0-144">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5ceb0-144">Next steps</span></span>

<span data-ttu-id="5ceb0-145">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="5ceb0-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5ceb0-146">Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5ceb0-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="5ceb0-147">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git</span><span class="sxs-lookup"><span data-stu-id="5ceb0-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="5ceb0-148">Następnie Dowiedz się, jak wdrożyć istniejącej aplikacji sieci web, która używa CosmosDB za pomocą wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5ceb0-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5ceb0-149">Wdrażanie na platformie Azure z poziomu wiersza polecenia z platformą .NET Core</span><span class="sxs-lookup"><span data-stu-id="5ceb0-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
