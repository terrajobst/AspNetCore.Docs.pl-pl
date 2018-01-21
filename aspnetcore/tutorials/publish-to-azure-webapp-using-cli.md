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
ms.openlocfilehash: 4797260f95443954e86aae1614140c0caa5ca8bd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>Wdrażanie aplikacji platformy ASP.NET Core usłudze Azure App Service z wiersza polecenia

Przez [Soper kamery](https://twitter.com/camsoper)

Ten samouczek przedstawia sposób tworzenia i wdrażania aplikacji platformy ASP.NET Core w usłudze Microsoft Azure App Service przy użyciu narzędzia wiersza polecenia.  Po zakończeniu będziesz mieć aplikację sieci web wbudowanych w platformy ASP.NET Core MVC hostowanej jako aplikacji sieci Web platformy Azure App Service.  W tym samouczku zostały utworzone za pomocą narzędzia wiersza polecenia systemu Windows, ale można zastosować do macOS i środowisk Linux, jak również.  

Z tego samouczka, dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure
> * Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka będą potrzebne:

* A [subskrypcji Microsoft Azure](https://azure.microsoft.com/free/)
* [.NET Core](https://www.microsoft.com/net/download/core)
* [Git](https://www.git-scm.com/) klient wiersza polecenia

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci web

Utwórz nowy katalog dla aplikacji sieci web, Utwórz nową aplikację ASP.NET Core MVC, a następnie uruchom witrynę sieci Web lokalnie.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[Inne](#tab/other)
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

Przetestuj aplikację, przechodząc do http://localhost: 5000.

![Witryny sieci Web uruchomionej na komputerze lokalnym](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a>Utwórz wystąpienie usługi aplikacji Azure

Przy użyciu [powłoki chmury Azure](/azure/cloud-shell/quickstart), Utwórz grupę zasobów, plan usługi aplikacji i aplikacji sieci web usługi aplikacji.

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

Przed przystąpieniem do wdrożenia należy ustawić poświadczenia wdrażania na poziomie konta przy użyciu następującego polecenia:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Adres URL wdrożenia jest wymagany do wdrażania aplikacji przy użyciu narzędzia Git.  Pobiera adres URL podobny do tego.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
Zanotuj adres URL wyświetlany w `.git`. Jest on używany w następnym kroku.

## <a name="deploy-the-application-using-git"></a>Wdrażanie aplikacji przy użyciu narzędzia Git

Wszystko jest gotowe do wdrożenia z komputera lokalnego przy użyciu narzędzia Git.

> [!NOTE]
> Jest bezpiecznie zignorować ostrzeżenia z repozytorium Git o zakończenia wierszy.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
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

# <a name="othertabother"></a>[Inne](#tab/other)
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

Git wyświetli monit o poświadczenia wdrażania, które zostały określone wcześniej.  Po uwierzytelnieniu aplikacji będzie można przeniesiony do lokalizacji zdalnej, wbudowane i wdrożone.

![Dane wyjściowe wdrażanie Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>Testowanie aplikacji

Przetestuj aplikację, przechodząc do `https://<web app name>.azurewebsites.net`.  Aby wyświetlić adres w chmurze powłoki (lub wiersza polecenia platformy Azure), użyj następującego polecenia:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikacji działających na platformie Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Czyszczenie

Po zakończeniu testowania aplikacji i zapoznanie się z kodu i zasobów, należy usunąć aplikacji sieci web i planu poprzez usunięcie grupy zasobów.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób:

> [!div class="checklist"]
> * Utwórz witrynę sieci Web Azure App Service przy użyciu wiersza polecenia platformy Azure
> * Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia Git

Następnie Dowiedz się, jak wdrożyć istniejącej aplikacji sieci web, która używa CosmosDB za pomocą wiersza polecenia.

> [!div class="nextstepaction"]
> [Wdrażanie na platformie Azure z poziomu wiersza polecenia z platformą .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
