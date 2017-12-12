---
title: "Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i Git"
author: rick-anderson
description: "Informacje o sposobie tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w usłudze Azure App Service przy użyciu usługi Git do ciągłego wdrażania."
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="e4bf0-104">Ciągłe wdrażanie na platformie Azure dla platformy ASP.NET Core, z programu Visual Studio i Git</span><span class="sxs-lookup"><span data-stu-id="e4bf0-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="e4bf0-105">Przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="e4bf0-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="e4bf0-106">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w programie Visual Studio w usłudze Azure App Service przy użyciu ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="e4bf0-107">Zobacz też [VSTS używany do tworzenia i publikowania w ciągłego wdrażania aplikacji sieci Web Azure](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), który wskazuje, jak skonfigurować ciągłego dostarczania (CD) przepływ pracy dla [usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) przy użyciu programu Visual Studio Team Usługi.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="e4bf0-108">Azure ciągłego dostarczania w Team Services upraszcza proces konfigurowania potoku niezawodne wdrożenia do publikowania aktualizacji dla aplikacji w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="e4bf0-109">Potok można skonfigurować w portalu Azure do kompilacji, uruchom testy wdrożyć miejsce wystawiania i następnie wdrożyć do środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bf0-110">Do ukończenia tego samouczka potrzebne jest konto Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="e4bf0-111">Jeśli nie masz konta, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e4bf0-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4bf0-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e4bf0-112">Prerequisites</span></span>

<span data-ttu-id="e4bf0-113">W tym samouczku przyjęto założenie, że zainstalowano już następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="e4bf0-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4bf0-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="e4bf0-115">[Platformy ASP.NET Core](https://www.microsoft.com/net/download/core) (środowisko uruchomieniowe i narzędzi)</span><span class="sxs-lookup"><span data-stu-id="e4bf0-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>

* <span data-ttu-id="e4bf0-116">[Git](https://git-scm.com/downloads) dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="e4bf0-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="e4bf0-117">Tworzenie aplikacji sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4bf0-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="e4bf0-118">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="e4bf0-119">Z **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="e4bf0-120">Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-120">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="e4bf0-121">Wygląda na to, w obszarze **zainstalowana** > **szablony** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-121">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="e4bf0-122">Nazwij projekt `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="e4bf0-123">Wybierz **utworzyć nowe repozytorium Git** opcję i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Okno dialogowe nowego projektu](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="e4bf0-125">W **nowy projekt programu ASP.NET Core** okno dialogowe, wybierz platformy ASP.NET Core **pusty** szablonu, następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-125">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Okno dialogowe nowego projektu platformy ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    ><span data-ttu-id="e4bf0-127">Najbardziej aktualną wersją .NET Core jest 2.0</span><span class="sxs-lookup"><span data-stu-id="e4bf0-127">Most recent release of .NET Core is 2.0</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="e4bf0-128">Uruchomienie aplikacji sieci web lokalnie</span><span class="sxs-lookup"><span data-stu-id="e4bf0-128">Running the web app locally</span></span>

1. <span data-ttu-id="e4bf0-129">Po zakończeniu tworzenia aplikacji programu Visual Studio Uruchom aplikację wybierając **debugowania** -> **Rozpocznij debugowanie**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-129">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="e4bf0-130">Alternatywnie, można nacisnąć klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-130">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="e4bf0-131">Może potrwać do zainicjowania programu Visual Studio i nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-131">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="e4bf0-132">Po zakończeniu przeglądarki zostaną wyświetlone uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-132">Once it is complete, the browser will show the running app.</span></span>

   ![Wyświetlanie okna przeglądarki uruchomiona aplikacja, która wyświetla "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="e4bf0-134">Po przejrzeniu uruchomionej aplikacji sieci Web, zamknij przeglądarkę, a następnie kliknij przycisk "Zatrzymaj debugowanie" ikony na pasku narzędzi programu Visual Studio, aby zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-134">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="e4bf0-135">Tworzenie aplikacji sieci web w portalu Azure</span><span class="sxs-lookup"><span data-stu-id="e4bf0-135">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="e4bf0-136">Poniższe kroki przeprowadzi Cię przez proces tworzenia aplikacji sieci web w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-136">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="e4bf0-137">Zaloguj się do [portalu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="e4bf0-137">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="e4bf0-138">Wybierz **nowy** u góry po lewej portalu</span><span class="sxs-lookup"><span data-stu-id="e4bf0-138">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="e4bf0-139">Wybierz **sieci Web i mobilność** > **sieci Web aplikacji**</span><span class="sxs-lookup"><span data-stu-id="e4bf0-139">TAP **Web + Mobile** > **Web App**</span></span>

    ![Portalu Microsoft Azure: Przycisk nowego: sieci Web i mobilność w witrynie Marketplace: przycisku aplikacji sieci Web w obszarze polecane aplikacje](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="e4bf0-141">W **aplikacji sieci Web** bloku, wprowadź unikatową wartość **nazwę usługi aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-141">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Bloku aplikacja sieci Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="e4bf0-143">**Nazwę usługi aplikacji** nazwa musi być unikatowa.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-143">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="e4bf0-144">Podczas próby wprowadź nazwę, portalu będzie wymuszać tej reguły.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-144">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="e4bf0-145">Po wprowadzeniu inną wartość, należy zastąpić tę wartość dla każdego wystąpienia **SampleWebAppDemo** widoczny w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-145">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="e4bf0-146">Również w **aplikacji sieci Web** bloku, wybierz istniejący **lokalizacja planu usługi aplikacji** lub Utwórz nową.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-146">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="e4bf0-147">Jeśli tworzysz nowy plan, wybierz warstwę cenową, lokalizacji i innych opcji.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-147">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="e4bf0-148">Aby uzyskać więcej informacji na temat planów usługi aplikacji [szczegółowe omówienie planów usługi aplikacji Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="e4bf0-148">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="e4bf0-149">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-149">Click **Create**.</span></span> <span data-ttu-id="e4bf0-150">Azure obsługi administracyjnej, a następnie uruchom aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-150">Azure will provision and start your web app.</span></span>

    ![Portalu Azure: Blok Essentials pokaz aplikacji sieci Web przykładowej 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="e4bf0-152">Włączanie publikowania Git dla nowej aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="e4bf0-152">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="e4bf0-153">Git to system kontroli wersji rozproszonej, który służy do wdrażania aplikacji sieci web w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-153">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="e4bf0-154">Będą przechowywane kod dla aplikacji sieci web w lokalnym repozytorium Git i wdrożony kodu na platformie Azure przez wypchnięcie do repozytorium zdalnego.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-154">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="e4bf0-155">Zaloguj się do [Azure Portal](https://portal.azure.com), jeśli jeszcze nie jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-155">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="e4bf0-156">Kliknij przycisk **usługi aplikacji** Aby wyświetlić listę usług aplikacji skojarzone z subskrypcją platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-156">Click **App Services** to view a list of the app services associated with your Azure subscription.</span></span>

3. <span data-ttu-id="e4bf0-157">Wybierz aplikację sieci web utworzonej w poprzedniej sekcji tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-157">Select the web app you created in the previous section of this tutorial.</span></span>

4. <span data-ttu-id="e4bf0-158">W **wdrożenia** bloku, wybierz opcję **opcje wdrażania** > **wybierz źródło** > **lokalnego repozytorium Git**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-158">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blok ustawień: bloku źródła wdrożenia: Wybierz źródło bloku](azure-continuous-deployment/_static/deployment-options.png)

5. <span data-ttu-id="e4bf0-160">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-160">Click **OK**.</span></span>

6. <span data-ttu-id="e4bf0-161">Jeśli nie ma poświadczeń wdrożenia do publikowania aplikacji sieci web lub innych aplikacji usługi app Service, skonfiguruj je teraz:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-161">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="e4bf0-162">Kliknij przycisk **ustawienia** > **poświadczenia wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-162">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="e4bf0-163">**Ustawić poświadczenia wdrażania** zostanie wyświetlony blok.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-163">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="e4bf0-164">Utwórz nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-164">Create a user name and password.</span></span>  <span data-ttu-id="e4bf0-165">To hasło będzie potrzebne później podczas konfigurowania Git.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-165">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="e4bf0-166">Kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-166">Click **Save**.</span></span>

7. <span data-ttu-id="e4bf0-167">W **aplikacji sieci Web** bloku, kliknij przycisk **ustawienia** > **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-167">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="e4bf0-168">Adres URL zdalnego repozytorium Git, który będzie można wdrożyć do jest wyświetlany w obszarze **adres URL GIT**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-168">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

8. <span data-ttu-id="e4bf0-169">Kopiuj **adres URL GIT** wartość do wykorzystania później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-169">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portalu Azure: bloku właściwości aplikacji](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="e4bf0-171">Publikowanie aplikacji sieci web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e4bf0-171">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="e4bf0-172">W tej sekcji utworzysz lokalne repozytorium Git przy użyciu programu Visual Studio i wypychania z tego repozytorium na platformie Azure do wdrożenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-172">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="e4bf0-173">Następujące etapy:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-173">The steps involved include the following:</span></span>

   * <span data-ttu-id="e4bf0-174">Dodaj odpowiednie ustawienie repozytorium zdalnego przy użyciu wartość adres URL GIT, aby umożliwić wdrażanie lokalnym repozytorium na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-174">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="e4bf0-175">Zatwierdź zmiany projektu.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-175">Commit your project changes.</span></span>

   * <span data-ttu-id="e4bf0-176">Wypchnij zmiany projektu z lokalnym repozytorium do zdalnego repozytorium na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-176">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="e4bf0-177">W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-177">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="e4bf0-178">**Team Explorer** będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-178">The **Team Explorer** will be displayed.</span></span>

    ![Team Explorer połączyć karty](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="e4bf0-180">W **Team Explorer**, wybierz pozycję **macierzystego** (ikona domową) > **ustawienia** > **ustawienia repozytorium**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-180">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="e4bf0-181">W **element zdalny** sekcji **ustawienia repozytorium** wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-181">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="e4bf0-182">**Dodaj zdalny** wyświetli się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-182">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="e4bf0-183">Ustaw **nazwa** zdalnego do **aplikacja Azure**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-183">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="e4bf0-184">Ustaw wartość **pobrać** do **adres URL Git** skopiowany z platformy Azure we wcześniejszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-184">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="e4bf0-185">Należy pamiętać, że ten adres URL, który kończy się wyrazem **.git**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-185">Note that this is the URL that ends with **.git**.</span></span>

    ![Zdalne okno dialogowe Edycja](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="e4bf0-187">Alternatywnie, można określić zdalnego repozytorium z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzając polecenie.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-187">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="e4bf0-188">Na przykład:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="e4bf0-188">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="e4bf0-189">Wybierz **macierzystego** (ikona domową) > **ustawienia** > **ustawienia globalne**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-189">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="e4bf0-190">Upewnij się, że masz swoją nazwę i adres e-mail, ustaw.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-190">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="e4bf0-191">Może być również konieczne wybierz **aktualizacji**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-191">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="e4bf0-192">Wybierz **Home** > **zmiany** aby powrócić do **zmiany** widoku.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-192">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="e4bf0-193">Wpisz wiadomość, zatwierdzania, takich jak **początkowej Push #1** i kliknij przycisk **zatwierdzania**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-193">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="e4bf0-194">Ta akcja spowoduje utworzenie *zatwierdzania* lokalnie.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-194">This action will create a *commit* locally.</span></span> <span data-ttu-id="e4bf0-195">Następnie należy do *synchronizacji* z platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-195">Next, you need to *sync* with Azure.</span></span>

    ![Team Explorer połączyć karty](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="e4bf0-197">Alternatywnie, można zatwierdzić zmiany z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzania poleceń git.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-197">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="e4bf0-198">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-198">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="e4bf0-199">Wybierz **Home** > **synchronizacji** > **akcje** > **Otwórz wiersz polecenia**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-199">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="e4bf0-200">Wiersz polecenia otworzy się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-200">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="e4bf0-201">Wprowadź następujące polecenie w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-201">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="e4bf0-202">Wprowadź Azure **poświadczenia wdrażania** hasła utworzonego wcześniej w usłudze Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-202">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="e4bf0-203">Podczas wprowadzania hasła nie będą widoczne.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-203">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="e4bf0-204">To polecenie spowoduje uruchomienie procesu pchać plików lokalnych projektu na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-204">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="e4bf0-205">Dane wyjściowe z powyższego polecenia kończy się komunikat, że wdrożenie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-205">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="e4bf0-206">Jeśli potrzebujesz współpracować nad projektem, należy rozważyć Wypychanie do [GitHub](https://github.com) między Wypychanie do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-206">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="e4bf0-207">Sprawdź aktywnych wdrożeń</span><span class="sxs-lookup"><span data-stu-id="e4bf0-207">Verify the Active Deployment</span></span>

<span data-ttu-id="e4bf0-208">Aby sprawdzić, czy pomyślnie przeniesiono aplikacji sieci web ze środowiska lokalnego do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-208">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="e4bf0-209">Zobaczysz wymienionych pomyślne wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-209">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="e4bf0-210">W [Azure Portal](https://portal.azure.com), wybierz aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-210">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="e4bf0-211">Następnie wybierz opcję **wdrożenia** > **opcje wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-211">Then, select **Deployment** > **Deployment options**.</span></span>

   ![Portalu Azure: Blok ustawień: blok wdrożeń pomyślnego wdrożenia](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="e4bf0-213">Uruchom aplikację na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="e4bf0-213">Run the app in Azure</span></span>

<span data-ttu-id="e4bf0-214">Teraz, wdrożono aplikację sieci web na platformie Azure, możesz uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-214">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="e4bf0-215">Można to zrobić na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-215">This can be done in two ways:</span></span>

* <span data-ttu-id="e4bf0-216">W portalu Azure Znajdź bloku aplikacja sieci web dla aplikacji sieci web, a następnie kliknij przycisk **Przeglądaj** do wyświetlania aplikacji w domyślnej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-216">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="e4bf0-217">Otwórz przeglądarkę i wprowadź adres URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-217">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="e4bf0-218">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-218">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="e4bf0-219">Aktualizowanie aplikacji sieci web i opublikować</span><span class="sxs-lookup"><span data-stu-id="e4bf0-219">Update your web app and republish</span></span>

<span data-ttu-id="e4bf0-220">Po wprowadzeniu zmian w kodzie lokalnego można ponownie opublikować.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-220">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="e4bf0-221">W **Eksploratora rozwiązań** programu Visual Studio Otwórz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-221">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="e4bf0-222">W `Configure` metody, zmodyfikuj `Response.WriteAsync` metody, dzięki czemu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-222">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="e4bf0-223">Zapisać zmiany w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-223">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="e4bf0-224">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-224">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="e4bf0-225">**Team Explorer** będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-225">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="e4bf0-226">Wprowadź komunikat zatwierdzenia, takich jak:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-226">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="e4bf0-227">Naciśnij klawisz **zatwierdzania** przycisk, aby zatwierdzić zmiany projektu.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-227">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="e4bf0-228">Wybierz **Home** > **synchronizacji** > **akcje** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-228">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="e4bf0-229">Alternatywnie możesz wypchnąć zmiany z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzając polecenie git.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-229">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="e4bf0-230">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-230">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="e4bf0-231">Widok zaktualizowano aplikację sieci web na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="e4bf0-231">View the updated web app in Azure</span></span>

<span data-ttu-id="e4bf0-232">Wyświetlenie aplikacji sieci web zaktualizowane przez wybranie **Przeglądaj** z bloku aplikacja sieci web w portalu Azure lub przez otwarcie przeglądarki i wprowadzić adres URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="e4bf0-232">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="e4bf0-233">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e4bf0-233">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="e4bf0-234">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e4bf0-234">Additional Resources</span></span>

* [<span data-ttu-id="e4bf0-235">Publikowanie i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="e4bf0-235">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="e4bf0-236">Program Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="e4bf0-236">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
