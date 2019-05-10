---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure przy użyciu programu Visual Studio Code
author: ricardoserradas
description: Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio Code
ms.author: ricardoserradas
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 64d82835f6a47a458802692c99658b964c07f807
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901978"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a><span data-ttu-id="19ccd-103">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure przy użyciu programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19ccd-103">Publish an ASP.NET Core app to Azure with Visual Studio Code</span></span>

<span data-ttu-id="19ccd-104">Przez [Ricardo Serradas](https://twitter.com/ricardoserradas)</span><span class="sxs-lookup"><span data-stu-id="19ccd-104">By [Ricardo Serradas](https://twitter.com/ricardoserradas)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="19ccd-105">Aby rozwiązać problem wdrożenia usługi App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="19ccd-105">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="intro"></a><span data-ttu-id="19ccd-106">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="19ccd-106">Intro</span></span>

<span data-ttu-id="19ccd-107">W tym samouczku dowiesz się, jak utworzyć aplikację ASP.Net Core MVC i wdrożyć ją w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="19ccd-107">With this tutorial, you'll learn how to create an ASP.Net Core MVC Application and deploy it within Visual Studio Code.</span></span>

## <a name="set-up"></a><span data-ttu-id="19ccd-108">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="19ccd-108">Set up</span></span>

- <span data-ttu-id="19ccd-109">Otwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/dotnet/) Jeśli nie masz.</span><span class="sxs-lookup"><span data-stu-id="19ccd-109">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span>
- <span data-ttu-id="19ccd-110">Zainstaluj [platformy .NET Core SDK](https://dotnet.microsoft.com/download)</span><span class="sxs-lookup"><span data-stu-id="19ccd-110">Install [.NET Core SDK](https://dotnet.microsoft.com/download)</span></span>
- <span data-ttu-id="19ccd-111">Zainstaluj [programu Visual Studio Code](https://code.visualstudio.com/Download)</span><span class="sxs-lookup"><span data-stu-id="19ccd-111">Install [Visual Studio Code](https://code.visualstudio.com/Download)</span></span>
  - <span data-ttu-id="19ccd-112">Zainstaluj [ C# rozszerzenia](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19ccd-112">Install the [C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to Visual Studio Code</span></span>
  - <span data-ttu-id="19ccd-113">Instalowanie [rozszerzenia usługi aplikacji Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) programu Visual Studio Code i skonfiguruj ją przed kontynuowaniem</span><span class="sxs-lookup"><span data-stu-id="19ccd-113">Instal the [Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) to Visual Studio Code and configure it before proceeding</span></span>

## <a name="create-an-aspnet-core-mvc-project"></a><span data-ttu-id="19ccd-114">Tworzenie projektu programu ASP.Net Core MVC</span><span class="sxs-lookup"><span data-stu-id="19ccd-114">Create an ASP.Net Core MVC project</span></span>

<span data-ttu-id="19ccd-115">Za pomocą terminalu przejdź do folderu, chcesz, aby projekt można tworzyć na i użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="19ccd-115">Using a terminal, navigate to the folder you want the project to be created on and use the following command:</span></span>

```cmd
> dotnet new mvc
```

<span data-ttu-id="19ccd-116">Będziesz mieć strukturę folderów, które jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="19ccd-116">You'll have a folder structure similar to the following:</span></span>

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

## <a name="open-it-with-visual-studio-code"></a><span data-ttu-id="19ccd-117">Otwórz go za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19ccd-117">Open it with Visual Studio Code</span></span>

<span data-ttu-id="19ccd-118">Po utworzeniu projektu, możesz go otworzyć przy użyciu programu Visual Studio Code przy użyciu jednej z poniższych opcji:</span><span class="sxs-lookup"><span data-stu-id="19ccd-118">After your project is created, you can open it with Visual Studio Code by using one of the options below:</span></span>

### <a name="through-the-command-line"></a><span data-ttu-id="19ccd-119">Za pomocą wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="19ccd-119">Through the command line</span></span>

<span data-ttu-id="19ccd-120">Użyj następującego polecenia w folderze, że utworzono projekt:</span><span class="sxs-lookup"><span data-stu-id="19ccd-120">Use the following command within the folder you created the project:</span></span>

```cmd
> code .
```

<span data-ttu-id="19ccd-121">Jeśli poniższe polecenie nie działa, sprawdź, jeśli instalacja jest prawidłowo skonfigurowane, odwołując się do [ten link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span><span class="sxs-lookup"><span data-stu-id="19ccd-121">If the command below does not work, check if your installation is configured properly by referencing [this link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span></span>

### <a name="through-visual-studio-code-interface"></a><span data-ttu-id="19ccd-122">Za pomocą interfejsu programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19ccd-122">Through Visual Studio Code interface</span></span>

- <span data-ttu-id="19ccd-123">Open Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19ccd-123">Open Visual Studio Code</span></span>
- <span data-ttu-id="19ccd-124">W menu wybierz pozycję `File > Open Folder`</span><span class="sxs-lookup"><span data-stu-id="19ccd-124">On the menu, select `File > Open Folder`</span></span>
- <span data-ttu-id="19ccd-125">Wybierz katalog główny folderu utworzonego projektu MVC</span><span class="sxs-lookup"><span data-stu-id="19ccd-125">Select the root of the folder you created the MVC Project</span></span>

<span data-ttu-id="19ccd-126">Po otwarciu folderu projektu, otrzymasz komunikat z informacją, że wymagane zasoby do tworzenia i debugowania są nieobecne.</span><span class="sxs-lookup"><span data-stu-id="19ccd-126">When you open the project folder, you'll receive a message saying that required assets to build and debug are missing.</span></span> <span data-ttu-id="19ccd-127">Zaakceptuj pomocy, aby dodać je.</span><span class="sxs-lookup"><span data-stu-id="19ccd-127">Accept the help to add them.</span></span>

![Załadowano interfejs graficzny Studio kodu z projektem](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

<span data-ttu-id="19ccd-129">A `.vscode` zostanie utworzony folder, w ramach struktury projektu.</span><span class="sxs-lookup"><span data-stu-id="19ccd-129">A `.vscode` folder will be created under the project structure.</span></span> <span data-ttu-id="19ccd-130">Będzie zawierać następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="19ccd-130">It will contain the following files:</span></span>

```cmd
launch.json
tasks.json
```

<span data-ttu-id="19ccd-131">Są to pliki narzędzia ułatwiają kompilowanie i debugowanie aplikacji sieci Web platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="19ccd-131">These are utility files to help you build and debug your .NET Core Web App.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="19ccd-132">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="19ccd-132">Run the app</span></span>

<span data-ttu-id="19ccd-133">Zanim wdrożymy aplikację na platformie Azure, upewnij się, że działa on poprawnie na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="19ccd-133">Before we deploy the app to Azure, make sure it is running properly on your local machine.</span></span>

- <span data-ttu-id="19ccd-134">Naciśnij klawisz F5, aby uruchomić projekt</span><span class="sxs-lookup"><span data-stu-id="19ccd-134">Press F5 to run the project</span></span>

<span data-ttu-id="19ccd-135">Twoja aplikacja sieci web zostanie uruchomione na nowej karcie domyślnej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="19ccd-135">Your web app will start running on a new tab of your default browser.</span></span> <span data-ttu-id="19ccd-136">Ostrzeżenie dotyczące prywatności można zauważyć, jak najszybciej po jego uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="19ccd-136">You may notice a privacy warning as soon as it starts.</span></span> <span data-ttu-id="19ccd-137">Jest to spowodowane aplikacji rozpocznie się albo przy użyciu protokołu HTTP i HTTPS, a przechodzi do punktu końcowego HTTPS domyślnie.</span><span class="sxs-lookup"><span data-stu-id="19ccd-137">This is because your app will start either using HTTP and HTTPS, and it navigates to the HTTPS endpoint by default.</span></span>

![Ostrzeżenie dotyczące prywatności podczas debugowania aplikacji w środowisku lokalnym](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

<span data-ttu-id="19ccd-139">Aby zachować sesję debugowania, kliknij przycisk `Advanced` i następnie `Continue to localhost (unsafe)`.</span><span class="sxs-lookup"><span data-stu-id="19ccd-139">To keep the debugging session, click `Advanced` and then `Continue to localhost (unsafe)`.</span></span>

## <a name="generate-the-deployment-package-locally"></a><span data-ttu-id="19ccd-140">Wygeneruj pakiet wdrażania lokalnie</span><span class="sxs-lookup"><span data-stu-id="19ccd-140">Generate the deployment package locally</span></span>

- <span data-ttu-id="19ccd-141">Otwórz terminal programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19ccd-141">Open Visual Studio Code terminal</span></span>
- <span data-ttu-id="19ccd-142">Użyj następującego polecenia, aby wygenerować `Release` pakiet folder podrzędny o nazwie `publish`:</span><span class="sxs-lookup"><span data-stu-id="19ccd-142">Use the following command to generate a `Release` package to a sub folder called `publish`:</span></span>
  - `dotnet publish -c Release -o ./publish`
- <span data-ttu-id="19ccd-143">Nowy `publish` zostanie utworzony folder, w ramach struktury projektu</span><span class="sxs-lookup"><span data-stu-id="19ccd-143">A new `publish` folder will be created under the project structure</span></span>

![Struktura folderu publikowania](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a><span data-ttu-id="19ccd-145">Publikowanie w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="19ccd-145">Publish to Azure App Service</span></span>

<span data-ttu-id="19ccd-146">Korzystanie z rozszerzenia Azure App Service dla programu Visual Studio Code, wykonaj poniższe kroki, aby opublikować witryny sieci Web bezpośrednio w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="19ccd-146">Leveraging the Azure App Service extension for Visual Studio Code, follow the steps below to publish the website directly to the Azure App Service.</span></span>

### <a name="if-youre-creating-a-new-web-app"></a><span data-ttu-id="19ccd-147">Jeśli tworzysz nową aplikację sieci Web</span><span class="sxs-lookup"><span data-stu-id="19ccd-147">If you're creating a new Web App</span></span>

- <span data-ttu-id="19ccd-148">Kliknij prawym przyciskiem myszy `publish` i wybierz polecenie `Deploy to Web App...`</span><span class="sxs-lookup"><span data-stu-id="19ccd-148">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="19ccd-149">Wybierz subskrypcję, którą chcesz utworzyć aplikację sieci Web</span><span class="sxs-lookup"><span data-stu-id="19ccd-149">Select the subscription you want to create the Web App</span></span>
- <span data-ttu-id="19ccd-150">Wybierz pozycję `Create New Web App`</span><span class="sxs-lookup"><span data-stu-id="19ccd-150">Select `Create New Web App`</span></span>
- <span data-ttu-id="19ccd-151">Wprowadź nazwę aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="19ccd-151">Enter a name for the Web App</span></span>

<span data-ttu-id="19ccd-152">Rozszerzenie spowoduje utworzenie nowej aplikacji sieci Web i automatycznie rozpocznie się wdrażanie pakietu.</span><span class="sxs-lookup"><span data-stu-id="19ccd-152">The extension will create the new Web App and will automatically start deploying the package to it.</span></span> <span data-ttu-id="19ccd-153">Po zakończeniu wdrożenia kliknij `Browse Website` na potrzeby weryfikacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="19ccd-153">Once the deployment is finished, click `Browse Website` to validate the deployment.</span></span>

![Wdrażanie zakończyło się pomyślnie wiadomości](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

<span data-ttu-id="19ccd-155">Po kliknięciu `Browse Website`, będzie łączyć się z nim za pomocą domyślnej przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="19ccd-155">Once you click `Browse Website`, you'll navigate to it using your default browser:</span></span>

![Nowa aplikacja sieci Web pomyślnie wdrożona](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a><span data-ttu-id="19ccd-157">Jeśli wdrażasz do istniejącej aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="19ccd-157">If you're deploying to an existing Web App</span></span>

- <span data-ttu-id="19ccd-158">Kliknij prawym przyciskiem myszy `publish` i wybierz polecenie `Deploy to Web App...`</span><span class="sxs-lookup"><span data-stu-id="19ccd-158">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="19ccd-159">Wybierz subskrypcję, w której znajduje się w istniejącej aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="19ccd-159">Select the subscription the existing Web App resides</span></span>
- <span data-ttu-id="19ccd-160">Wybierz aplikację sieci Web z listy</span><span class="sxs-lookup"><span data-stu-id="19ccd-160">Select the Web App from the list</span></span>
- <span data-ttu-id="19ccd-161">Visual Studio Code zapyta, czy chcesz zastąpić istniejącą zawartość.</span><span class="sxs-lookup"><span data-stu-id="19ccd-161">Visual Studio Code will ask you if you want to overwrite the existing content.</span></span> <span data-ttu-id="19ccd-162">Kliknij przycisk `Deploy` o potwierdzenie</span><span class="sxs-lookup"><span data-stu-id="19ccd-162">Click `Deploy` to confirm</span></span>

<span data-ttu-id="19ccd-163">Rozszerzenie zostanie wdrożona zaktualizowanej zawartości w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="19ccd-163">The extension will deploy the updated content to the Web App.</span></span> <span data-ttu-id="19ccd-164">Gdy wszystko będzie gotowe, kliknij przycisk `Browse Website` na potrzeby weryfikacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="19ccd-164">Once it's done, click `Browse Website` to validate the deployment.</span></span>

![Pomyślnie wdrożono jest istniejącej aplikacji sieci Web](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a><span data-ttu-id="19ccd-166">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="19ccd-166">Next steps</span></span>

- [<span data-ttu-id="19ccd-167">Tworzenie pierwszego potoku DevOps platformy Azure</span><span class="sxs-lookup"><span data-stu-id="19ccd-167">Create your first Azure DevOps pipeline</span></span>](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a><span data-ttu-id="19ccd-168">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="19ccd-168">Additional resources</span></span>

- [<span data-ttu-id="19ccd-169">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="19ccd-169">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
- [<span data-ttu-id="19ccd-170">Grupy zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="19ccd-170">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)