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
ms.openlocfilehash: 0462a4cf18bba23643ed3b1b4e6b76bdbceb24a8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="5f3e2-103">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="5f3e2-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="5f3e2-104">Przez [Soper kamery](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="5f3e2-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="5f3e2-105">Ten samouczek przedstawia sposób tworzenia i wdrażania aplikacji platformy ASP.NET Core w usłudze Microsoft Azure App Service przy użyciu narzędzia wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="5f3e2-106">Po zakończeniu będziesz mieć aplikację sieci web wbudowanych w platformy ASP.NET Core MVC hostowanej jako aplikacji sieci Web platformy Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="5f3e2-107">W tym samouczku zostały utworzone za pomocą narzędzia wiersza polecenia systemu Windows, ale można zastosować do macOS i środowisk Linux, jak również.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="5f3e2-108">Z tego samouczka, dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="5f3e2-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f3e2-109">Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5f3e2-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="5f3e2-110">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git</span><span class="sxs-lookup"><span data-stu-id="5f3e2-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f3e2-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5f3e2-111">Prerequisites</span></span>

<span data-ttu-id="5f3e2-112">Do ukończenia tego samouczka będą potrzebne:</span><span class="sxs-lookup"><span data-stu-id="5f3e2-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="5f3e2-113">A [subskrypcji Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="5f3e2-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="5f3e2-114">[Git](https://www.git-scm.com/) klient wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="5f3e2-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="5f3e2-115">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="5f3e2-115">Create a web application</span></span>

<span data-ttu-id="5f3e2-116">Utwórz nowy katalog dla aplikacji sieci web, Utwórz nową aplikację ASP.NET Core MVC, a następnie uruchom witrynę sieci Web lokalnie.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-116">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5f3e2-117">Windows</span><span class="sxs-lookup"><span data-stu-id="5f3e2-117">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="5f3e2-118">Inne</span><span class="sxs-lookup"><span data-stu-id="5f3e2-118">Other</span></span>](#tab/other)
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

<span data-ttu-id="5f3e2-120">Przetestuj aplikację, przechodząc do http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-120">Test the application by browsing to http://localhost:5000.</span></span>

![Witryny sieci Web uruchomionej na komputerze lokalnym](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="5f3e2-122">Utwórz wystąpienie usługi aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="5f3e2-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="5f3e2-123">Przy użyciu [powłoki chmury Azure](/azure/cloud-shell/quickstart), Utwórz grupę zasobów, plan usługi aplikacji i aplikacji sieci web usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="5f3e2-124">Przed przystąpieniem do wdrożenia należy ustawić poświadczenia wdrażania na poziomie konta przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5f3e2-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="5f3e2-125">Adres URL wdrożenia jest wymagany do wdrażania aplikacji przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-125">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="5f3e2-126">Pobiera adres URL podobny do tego.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="5f3e2-127">Zanotuj adres URL wyświetlany w `.git`.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="5f3e2-128">Jest on używany w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-128">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="5f3e2-129">Wdrażanie aplikacji przy użyciu narzędzia Git</span><span class="sxs-lookup"><span data-stu-id="5f3e2-129">Deploy the application using Git</span></span>

<span data-ttu-id="5f3e2-130">Wszystko jest gotowe do wdrożenia z komputera lokalnego przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="5f3e2-131">Jest bezpiecznie zignorować ostrzeżenia z repozytorium Git o zakończenia wierszy.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5f3e2-132">Windows</span><span class="sxs-lookup"><span data-stu-id="5f3e2-132">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="5f3e2-133">Inne</span><span class="sxs-lookup"><span data-stu-id="5f3e2-133">Other</span></span>](#tab/other)
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

<span data-ttu-id="5f3e2-134">Git wyświetla monit o poświadczenia wdrażania, które zostały określone wcześniej.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="5f3e2-135">Po uwierzytelnieniu aplikacji będzie można przeniesiony do lokalizacji zdalnej, wbudowane i wdrożone.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-135">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Dane wyjściowe wdrażanie Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="5f3e2-137">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5f3e2-137">Test the application</span></span>

<span data-ttu-id="5f3e2-138">Przetestuj aplikację, przechodząc do `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-138">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="5f3e2-139">Aby wyświetlić adres w chmurze powłoki (lub wiersza polecenia platformy Azure), użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="5f3e2-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikacji działających na platformie Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="5f3e2-141">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="5f3e2-141">Clean up</span></span>

<span data-ttu-id="5f3e2-142">Po zakończeniu testowania aplikacji i zapoznanie się z kodu i zasobów, należy usunąć aplikacji sieci web i planu poprzez usunięcie grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="5f3e2-143">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5f3e2-143">Next steps</span></span>

<span data-ttu-id="5f3e2-144">W tym samouczku przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="5f3e2-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f3e2-145">Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure</span><span class="sxs-lookup"><span data-stu-id="5f3e2-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="5f3e2-146">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git</span><span class="sxs-lookup"><span data-stu-id="5f3e2-146">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="5f3e2-147">Następnie Dowiedz się, jak wdrożyć istniejącej aplikacji sieci web, która używa CosmosDB za pomocą wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5f3e2-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5f3e2-148">Wdrażanie na platformie Azure z poziomu wiersza polecenia z platformą .NET Core</span><span class="sxs-lookup"><span data-stu-id="5f3e2-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
