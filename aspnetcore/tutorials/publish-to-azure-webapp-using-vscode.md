---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure przy użyciu programu Visual Studio Code
author: ricardoserradas
description: Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio Code
ms.author: riserrad
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 2eaae62af97927fbe22e7f5d4fadfc2265c5a5cd
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538750"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a>Publikowanie aplikacji platformy ASP.NET Core na platformie Azure przy użyciu programu Visual Studio Code

Przez [Ricardo Serradas](https://twitter.com/ricardoserradas)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Aby rozwiązać problem wdrożenia usługi App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="intro"></a>Wprowadzenie

W tym samouczku dowiesz się, jak utworzyć aplikację ASP.Net Core MVC i wdrożyć ją w programie Visual Studio Code.

## <a name="set-up"></a>Konfigurowanie

- Otwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/dotnet/) Jeśli nie masz.
- Zainstaluj [platformy .NET Core SDK](https://dotnet.microsoft.com/download)
- Zainstaluj [programu Visual Studio Code](https://code.visualstudio.com/Download)
  - Zainstaluj [ C# rozszerzenia](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do programu Visual Studio Code
  - Instalowanie [rozszerzenia usługi aplikacji Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) programu Visual Studio Code i skonfiguruj ją przed kontynuowaniem

## <a name="create-an-aspnet-core-mvc-project"></a>Tworzenie projektu programu ASP.Net Core MVC

Za pomocą terminalu przejdź do folderu, chcesz, aby projekt można tworzyć na i użyj następującego polecenia:

```cmd
> dotnet new mvc
```

Będziesz mieć strukturę folderów, które jest podobny do następującego:

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a>Otwórz go za pomocą programu Visual Studio Code

Po utworzeniu projektu, możesz go otworzyć przy użyciu programu Visual Studio Code przy użyciu jednej z poniższych opcji:

### <a name="through-the-command-line"></a>Za pomocą wiersza polecenia

Użyj następującego polecenia w folderze, że utworzono projekt:

```cmd
> code .
```

Jeśli poniższe polecenie nie działa, sprawdź, jeśli instalacja jest prawidłowo skonfigurowane, odwołując się do [ten link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).

### <a name="through-visual-studio-code-interface"></a>Za pomocą interfejsu programu Visual Studio Code

- Open Visual Studio Code
- W menu wybierz pozycję `File > Open Folder`
- Wybierz katalog główny folderu utworzonego projektu MVC

Po otwarciu folderu projektu, otrzymasz komunikat z informacją, że wymagane zasoby do tworzenia i debugowania są nieobecne. Zaakceptuj pomocy, aby dodać je.

![Załadowano interfejs graficzny Studio kodu z projektem](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

A `.vscode` zostanie utworzony folder, w ramach struktury projektu. Będzie zawierać następujące pliki:

```cmd
launch.json
tasks.json
```

Są to pliki narzędzia ułatwiają kompilowanie i debugowanie aplikacji sieci Web platformy .NET Core.

## <a name="run-the-app"></a>Uruchamianie aplikacji

Zanim wdrożymy aplikację na platformie Azure, upewnij się, że działa on poprawnie na komputerze lokalnym.

- Naciśnij klawisz F5, aby uruchomić projekt

Twoja aplikacja sieci web zostanie uruchomione na nowej karcie domyślnej przeglądarki. Ostrzeżenie dotyczące prywatności można zauważyć, jak najszybciej po jego uruchomieniu. Jest to spowodowane aplikacji rozpocznie się albo przy użyciu protokołu HTTP i HTTPS, a przechodzi do punktu końcowego HTTPS domyślnie.

![Ostrzeżenie dotyczące prywatności podczas debugowania aplikacji w środowisku lokalnym](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

Aby zachować sesję debugowania, kliknij przycisk `Advanced` i następnie `Continue to localhost (unsafe)`.

## <a name="generate-the-deployment-package-locally"></a>Wygeneruj pakiet wdrażania lokalnie

- Otwórz terminal programu Visual Studio Code
- Użyj następującego polecenia, aby wygenerować `Release` pakiet folder podrzędny o nazwie `publish`:
  - `dotnet publish -c Release -o ./publish`
- Nowy `publish` zostanie utworzony folder, w ramach struktury projektu

![Struktura folderu publikowania](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a>Publikowanie w usłudze Azure App Service

Korzystanie z rozszerzenia Azure App Service dla programu Visual Studio Code, wykonaj poniższe kroki, aby opublikować witryny sieci Web bezpośrednio w usłudze Azure App Service.

### <a name="if-youre-creating-a-new-web-app"></a>Jeśli tworzysz nową aplikację sieci Web

- Kliknij prawym przyciskiem myszy `publish` i wybierz polecenie `Deploy to Web App...`
- Wybierz subskrypcję, którą chcesz utworzyć aplikację sieci Web
- Wybierz pozycję `Create New Web App`
- Wprowadź nazwę aplikacji sieci Web

Rozszerzenie spowoduje utworzenie nowej aplikacji sieci Web i automatycznie rozpocznie się wdrażanie pakietu. Po zakończeniu wdrożenia kliknij `Browse Website` na potrzeby weryfikacji wdrożenia.

![Wdrażanie zakończyło się pomyślnie wiadomości](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

Po kliknięciu `Browse Website`, będzie łączyć się z nim za pomocą domyślnej przeglądarki:

![Nowa aplikacja sieci Web pomyślnie wdrożona](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a>Jeśli wdrażasz do istniejącej aplikacji sieci Web

- Kliknij prawym przyciskiem myszy `publish` i wybierz polecenie `Deploy to Web App...`
- Wybierz subskrypcję, w której znajduje się w istniejącej aplikacji sieci Web
- Wybierz aplikację sieci Web z listy
- Visual Studio Code zapyta, czy chcesz zastąpić istniejącą zawartość. Kliknij przycisk `Deploy` o potwierdzenie

Rozszerzenie zostanie wdrożona zaktualizowanej zawartości w aplikacji sieci Web. Gdy wszystko będzie gotowe, kliknij przycisk `Browse Website` na potrzeby weryfikacji wdrożenia.

![Pomyślnie wdrożono jest istniejącej aplikacji sieci Web](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a>Następne kroki

- [Tworzenie pierwszego potoku DevOps platformy Azure](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Usługa Azure App Service](/azure/app-service/app-service-web-overview)
- [Grupy zasobów platformy Azure](/azure/azure-resource-manager/resource-group-overview#resource-groups)