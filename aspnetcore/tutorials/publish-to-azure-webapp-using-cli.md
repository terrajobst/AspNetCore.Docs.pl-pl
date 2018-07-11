---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia
author: camsoper
description: Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service za pomocą klienta wiersza polecenia usługi Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126963"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia

Przez [Soper kamery](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

W tym samouczku będzie pokazują, jak tworzenie i wdrażanie aplikacji platformy ASP.NET Core w usłudze Microsoft Azure App Service przy użyciu narzędzia wiersza polecenia. Po zakończeniu będziesz mieć stron Razor, aplikację sieci web w programie ASP.NET Core jest hostowany jako aplikacja sieci Web usługi Azure App Service. W tym samouczku są zapisywane przy użyciu narzędzia wiersza polecenia Windows, ale mogą być stosowane do systemu macOS i Linux środowisk, jak również.

W tym samouczku dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie witryny sieci Web usługi Azure App Service przy użyciu wiersza polecenia platformy Azure
> * Wdrażanie aplikacji ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia usługi Git

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka będą potrzebne:

* A [subskrypcji Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/) klienta wiersza polecenia

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

Utwórz nowy katalog dla aplikacji sieci web, Utwórz nową aplikację ASP.NET Core Razor strony, a następnie uruchom lokalnie witryny sieci Web.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

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

# <a name="othertabother"></a>[Inne](#tab/other)

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

Testowanie aplikacji, przechodząc do `http://localhost:5000`.

![Witryny sieci Web uruchomionej na komputerze lokalnym](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Tworzenie wystąpienia usługi Azure App Service

Za pomocą [usługi Azure Cloud Shell](/azure/cloud-shell/quickstart), Utwórz grupę zasobów, plan usługi App Service i aplikację sieci web usługi App Service.

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

Przed przystąpieniem do wdrożenia należy ustawić poświadczenia wdrażania na poziomie konta, za pomocą następującego polecenia:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Adres URL wdrożenia jest wymagane do wdrażania aplikacji przy użyciu narzędzia Git. Pobierz adres URL następująco.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Zanotuj adres URL wyświetlany w `.git`. Jest on używany w następnym kroku.

## <a name="deploy-the-app-using-git"></a>Wdrażanie aplikacji przy użyciu narzędzia Git

Wszystko gotowe do wdrożenia na maszynie lokalnej przy użyciu narzędzia Git.

> [!NOTE]
> Jest to bezpieczne Zignoruj wszelkie ostrzeżenia z repozytorium Git o zakończeniu linii.

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

Git wyświetla monit o poświadczenia wdrażania, które zostały ustawione wcześniej. Po uwierzytelnieniu, aplikacja będzie można wypchnięte do lokalizacji zdalnej, utworzone i wdrożone.

![Dane wyjściowe wdrożenia narzędzia Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Testowanie aplikacji

Testowanie aplikacji, przechodząc do `https://<web app name>.azurewebsites.net`. Aby wyświetlić adres w usłudze Cloud Shell (lub wiersza polecenia platformy Azure), użyj następujących opcji:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikacji działających na platformie Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Czyszczenie

Po zakończeniu testowania aplikacji i zapoznanie się z kodu i zasobów aplikacji internetowej i planu można usunąć przez usunięcie grupy zasobów.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób:

> [!div class="checklist"]
> * Tworzenie witryny sieci Web usługi Azure App Service przy użyciu wiersza polecenia platformy Azure
> * Wdrażanie aplikacji ASP.NET Core w usłudze Azure App Service przy użyciu narzędzia wiersza polecenia usługi Git

Dowiedz się używać wiersza polecenia do wdrażania istniejącej aplikacji sieci web, która używa bazy danych cosmos DB.

> [!div class="nextstepaction"]
> [Wdrażanie na platformie Azure z poziomu wiersza polecenia za pomocą programu .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
