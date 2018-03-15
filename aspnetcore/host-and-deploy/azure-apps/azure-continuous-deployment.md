---
title: "Ciągłe wdrażanie na platformie Azure z programem Visual Studio oraz Git z platformy ASP.NET Core"
author: rick-anderson
description: "Informacje o sposobie tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w usłudze Azure App Service przy użyciu usługi Git do ciągłego wdrażania."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="97a29-103">Ciągłe wdrażanie na platformie Azure z programem Visual Studio oraz Git z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97a29-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="97a29-104">Przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="97a29-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="97a29-105">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w programie Visual Studio w usłudze Azure App Service przy użyciu ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="97a29-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="97a29-106">Zobacz też [VSTS używany do tworzenia i publikowania w ciągłego wdrażania aplikacji sieci Web Azure](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), który wskazuje, jak skonfigurować ciągłego dostarczania (CD) przepływ pracy dla [usłudze Azure App Service](/azure/app-service/app-service-web-overview) przy użyciu programu Visual Studio Team Usługi.</span><span class="sxs-lookup"><span data-stu-id="97a29-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="97a29-107">Azure ciągłego dostarczania w Team Services upraszcza proces konfigurowania potoku niezawodne wdrożenia publikować aktualizacje dla aplikacji hostowanych w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="97a29-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="97a29-108">Potok można skonfigurować w portalu Azure do kompilacji, uruchom testy wdrożyć miejsce wystawiania i następnie wdrożyć do środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="97a29-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="97a29-109">Do ukończenia tego samouczka, wymagane jest konto Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="97a29-110">Aby utworzyć konta [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="97a29-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97a29-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="97a29-111">Prerequisites</span></span>

<span data-ttu-id="97a29-112">W tym samouczku przyjęto założenie, że zainstalowano następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="97a29-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="97a29-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97a29-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="97a29-114">[Zestaw SDK programu .NET core](https://www.microsoft.com/net/download/core) (środowisko uruchomieniowe i narzędzi)</span><span class="sxs-lookup"><span data-stu-id="97a29-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="97a29-115">[Git](https://git-scm.com/downloads) dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="97a29-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="97a29-116">Tworzenie aplikacji sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97a29-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="97a29-117">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97a29-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="97a29-118">Z **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="97a29-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="97a29-119">Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="97a29-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="97a29-120">Wygląda na to, w obszarze **zainstalowana** > **szablony** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="97a29-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="97a29-121">Nazwij projekt `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="97a29-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="97a29-122">Wybierz **Tworzenie nowego repozytorium Git** opcję i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="97a29-122">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Okno dialogowe nowego projektu](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="97a29-124">W **nowy projekt programu ASP.NET Core** okno dialogowe, wybierz platformy ASP.NET Core **pusty** szablonu, następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="97a29-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Okno dialogowe nowego projektu platformy ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="97a29-126">Najbardziej aktualną wersją .NET Core jest wersja 2.0.</span><span class="sxs-lookup"><span data-stu-id="97a29-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="97a29-127">Uruchomienie aplikacji sieci web lokalnie</span><span class="sxs-lookup"><span data-stu-id="97a29-127">Running the web app locally</span></span>

1. <span data-ttu-id="97a29-128">Po zakończeniu tworzenia aplikacji programu Visual Studio Uruchom aplikację wybierając **debugowania** > **Rozpocznij debugowanie**.</span><span class="sxs-lookup"><span data-stu-id="97a29-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="97a29-129">Alternatywnie, naciśnij klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="97a29-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="97a29-130">Może potrwać do zainicjowania programu Visual Studio i nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97a29-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="97a29-131">Po zakończeniu przeglądarki zawiera uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97a29-131">Once it's complete, the browser shows the running app.</span></span>

   ![Wyświetlanie okna przeglądarki uruchomiona aplikacja, która wyświetla "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="97a29-133">Po przejrzeniu uruchomionej aplikacji sieci Web, zamknij przeglądarkę i wybierz ikonę "Zatrzymaj debugowanie" na pasku narzędzi programu Visual Studio, aby zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="97a29-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="97a29-134">Tworzenie aplikacji sieci web w portalu Azure</span><span class="sxs-lookup"><span data-stu-id="97a29-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="97a29-135">Poniższe kroki tworzenia aplikacji sieci web w portalu Azure:</span><span class="sxs-lookup"><span data-stu-id="97a29-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="97a29-136">Zaloguj się do [portalu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="97a29-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="97a29-137">Wybierz **nowy** w lewym górnym rogu portalu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="97a29-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="97a29-138">Wybierz **sieci Web i mobilność** > **sieci Web aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="97a29-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portalu Microsoft Azure: Przycisk nowego: sieci Web i mobilność w witrynie Marketplace: przycisku aplikacji sieci Web w obszarze polecane aplikacje](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="97a29-140">W **aplikacji sieci Web** bloku, wprowadź unikatową wartość **nazwę usługi aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="97a29-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Bloku aplikacja sieci Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="97a29-142">**Nazwę usługi aplikacji** nazwa musi być unikatowa.</span><span class="sxs-lookup"><span data-stu-id="97a29-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="97a29-143">Portalu wymusza tej reguły, gdy została podana nazwa.</span><span class="sxs-lookup"><span data-stu-id="97a29-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="97a29-144">Jeśli podając inną wartość, należy zastąpić tę wartość dla każdego wystąpienia **SampleWebAppDemo** w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="97a29-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="97a29-145">Również w **aplikacji sieci Web** bloku, wybierz istniejący **lokalizacja planu usługi aplikacji** lub Utwórz nową.</span><span class="sxs-lookup"><span data-stu-id="97a29-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="97a29-146">W przypadku tworzenia nowego planu, wybierz warstwę cenową, lokalizacji i innych opcji.</span><span class="sxs-lookup"><span data-stu-id="97a29-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="97a29-147">Aby uzyskać więcej informacji na temat planów usługi aplikacji, zobacz [szczegółowe omówienie planów usługi aplikacji Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="97a29-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="97a29-148">Wybierz **utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="97a29-148">Select **Create**.</span></span> <span data-ttu-id="97a29-149">Azure obsługi administracyjnej, a następnie uruchomić aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-149">Azure will provision and start the web app.</span></span>

   ![Portalu Azure: Blok Essentials pokaz aplikacji sieci Web przykładowej 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="97a29-151">Włączanie publikowania Git dla nowej aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="97a29-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="97a29-152">Git to system kontroli wersji rozproszonej, który może służyć do wdrażania aplikacji sieci web usługi aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="97a29-153">Kod aplikacji sieci Web jest przechowywany w lokalnym repozytorium Git i kod jest wdrożony na platformie Azure przez wypchnięcie do repozytorium zdalnego.</span><span class="sxs-lookup"><span data-stu-id="97a29-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="97a29-154">Zaloguj się do [portalu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="97a29-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="97a29-155">Wybierz **usługi aplikacji** Aby wyświetlić listę usług aplikacji skojarzone z subskrypcją platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="97a29-156">Wybierz utworzony w poprzedniej sekcji w tym samouczku aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="97a29-157">W **wdrożenia** bloku, wybierz opcję **opcje wdrażania** > **wybierz źródło** > **lokalnego repozytorium Git**.</span><span class="sxs-lookup"><span data-stu-id="97a29-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blok ustawień: bloku źródła wdrożenia: Wybierz źródło bloku](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="97a29-159">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="97a29-159">Select **OK**.</span></span>

1. <span data-ttu-id="97a29-160">Jeśli wcześniej nie został ustawiony poświadczenia wdrażania dla publikowania aplikacji sieci web lub innych aplikacji usługi app Service, skonfiguruj je teraz:</span><span class="sxs-lookup"><span data-stu-id="97a29-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="97a29-161">Wybierz **ustawienia** > **poświadczenia wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="97a29-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="97a29-162">**Ustawić poświadczenia wdrażania** bloku jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="97a29-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="97a29-163">Utwórz nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="97a29-163">Create a user name and password.</span></span> <span data-ttu-id="97a29-164">Zapisz hasło do późniejszego użytku podczas konfigurowania Git.</span><span class="sxs-lookup"><span data-stu-id="97a29-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="97a29-165">Wybierz **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="97a29-165">Select **Save**.</span></span>

1. <span data-ttu-id="97a29-166">W **aplikacji sieci Web** bloku, wybierz opcję **ustawienia** > **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="97a29-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="97a29-167">Adres URL zdalnego repozytorium Git do wdrożenia jest wyświetlany w obszarze **adres URL GIT**.</span><span class="sxs-lookup"><span data-stu-id="97a29-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="97a29-168">Kopiuj **adres URL GIT** wartość do wykorzystania później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="97a29-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portalu Azure: bloku właściwości aplikacji](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="97a29-170">Publikowanie aplikacji sieci web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="97a29-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="97a29-171">W tej sekcji należy utworzyć lokalnego repozytorium Git przy użyciu programu Visual Studio i wypychania z tego repozytorium na platformie Azure do wdrażania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="97a29-172">Następujące etapy:</span><span class="sxs-lookup"><span data-stu-id="97a29-172">The steps involved include the following:</span></span>

* <span data-ttu-id="97a29-173">Dodaj ustawienie zdalnego repozytorium, używając wartość adresu URL GIT lokalnego repozytorium można wdrożyć na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="97a29-174">Zatwierdź zmiany projektu.</span><span class="sxs-lookup"><span data-stu-id="97a29-174">Commit project changes.</span></span>
* <span data-ttu-id="97a29-175">Wypchnij zmiany projektu z repozytorium lokalnego do zdalnego repozytorium na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="97a29-176">W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**.</span><span class="sxs-lookup"><span data-stu-id="97a29-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="97a29-177">**Team Explorer** jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="97a29-177">The **Team Explorer** is displayed.</span></span>

   ![Team Explorer połączyć karty](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="97a29-179">W **Team Explorer**, wybierz pozycję **macierzystego** (ikona domową) > **ustawienia** > **ustawienia repozytorium**.</span><span class="sxs-lookup"><span data-stu-id="97a29-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="97a29-180">W **element zdalny** sekcji **ustawienia repozytorium**, wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="97a29-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="97a29-181">**Dodaj zdalny** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="97a29-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="97a29-182">Ustaw **nazwa** zdalnego do **aplikacja Azure**.</span><span class="sxs-lookup"><span data-stu-id="97a29-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="97a29-183">Ustaw wartość **pobrać** do **adres URL Git** który skopiowano Azure we wcześniejszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="97a29-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="97a29-184">Należy pamiętać, że ten adres URL, który kończy się wyrazem **.git**.</span><span class="sxs-lookup"><span data-stu-id="97a29-184">Note that this is the URL that ends with **.git**.</span></span>

   ![Zdalne okno dialogowe Edycja](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="97a29-186">Alternatywnie, można określić zdalnego repozytorium, z **okno polecenia** otwierając **okno polecenia**, zmiana w katalogu projektu i wprowadzając polecenie.</span><span class="sxs-lookup"><span data-stu-id="97a29-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="97a29-187">Przykład:</span><span class="sxs-lookup"><span data-stu-id="97a29-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="97a29-188">Wybierz **macierzystego** (ikona domową) > **ustawienia** > **ustawienia globalne**.</span><span class="sxs-lookup"><span data-stu-id="97a29-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="97a29-189">Upewnij się, że nazwa i adres e-mail są ustawione.</span><span class="sxs-lookup"><span data-stu-id="97a29-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="97a29-190">Wybierz **aktualizacji** w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="97a29-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="97a29-191">Wybierz **Home** > **zmiany** aby powrócić do **zmiany** widoku.</span><span class="sxs-lookup"><span data-stu-id="97a29-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="97a29-192">Wpisz wiadomość, zatwierdzania, takich jak **początkowej Push #1** i wybierz **zatwierdzania**.</span><span class="sxs-lookup"><span data-stu-id="97a29-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="97a29-193">Ta akcja tworzy *zatwierdzania* lokalnie.</span><span class="sxs-lookup"><span data-stu-id="97a29-193">This action creates a *commit* locally.</span></span>

   ![Team Explorer połączyć karty](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="97a29-195">Alternatywnie, zatwierdzania zmieni się z **okno polecenia** otwierając **okno polecenia**, zmiana w katalogu projektu i wprowadzania poleceń git.</span><span class="sxs-lookup"><span data-stu-id="97a29-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="97a29-196">Przykład:</span><span class="sxs-lookup"><span data-stu-id="97a29-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="97a29-197">Wybierz **Home** > **synchronizacji** > **akcje** > **Otwórz wiersz polecenia**.</span><span class="sxs-lookup"><span data-stu-id="97a29-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="97a29-198">Wiersz polecenia otworzy się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="97a29-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="97a29-199">Wprowadź następujące polecenie w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="97a29-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="97a29-200">Wprowadź Azure **poświadczenia wdrażania** hasło utworzone wcześniej w usłudze Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="97a29-201">To polecenie uruchamia proces wypychanie pliki projektu lokalnego do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="97a29-202">Dane wyjściowe z powyższego polecenia kończy się komunikat o pomyślnym wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="97a29-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="97a29-203">Jeśli wymagana jest współpraca w projekcie, należy wziąć pod uwagę Wypychanie do [GitHub](https://github.com) przed wypchnięciem na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="97a29-204">Sprawdź aktywnych wdrożeń</span><span class="sxs-lookup"><span data-stu-id="97a29-204">Verify the Active Deployment</span></span>

<span data-ttu-id="97a29-205">Sprawdź, czy pomyślnie transfer aplikacji sieci web ze środowiska lokalnego do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="97a29-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="97a29-206">W [Azure Portal](https://portal.azure.com), wybierz aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="97a29-207">Wybierz **wdrożenia** > **opcje wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="97a29-207">Select **Deployment** > **Deployment options**.</span></span>

![Portalu Azure: Blok ustawień: blok wdrożeń pomyślnego wdrożenia](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="97a29-209">Uruchom aplikację na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="97a29-209">Run the app in Azure</span></span>

<span data-ttu-id="97a29-210">Teraz, gdy aplikacja sieci web jest wdrażana na platformie Azure, uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="97a29-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="97a29-211">Można to zrobić na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="97a29-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="97a29-212">W portalu Azure Znajdź bloku aplikacja sieci web dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="97a29-213">Wybierz **Przeglądaj** do wyświetlania aplikacji w domyślnej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="97a29-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="97a29-214">Otwórz przeglądarkę i wprowadź adres URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="97a29-215">Przykład: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="97a29-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="97a29-216">Aktualizowanie aplikacji sieci web i opublikować</span><span class="sxs-lookup"><span data-stu-id="97a29-216">Update the web app and republish</span></span>

<span data-ttu-id="97a29-217">Po wprowadzeniu zmian w kodzie lokalnego, należy opublikować:</span><span class="sxs-lookup"><span data-stu-id="97a29-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="97a29-218">W **Eksploratora rozwiązań** programu Visual Studio Otwórz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="97a29-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="97a29-219">W `Configure` metody, zmodyfikuj `Response.WriteAsync` metody, dzięki czemu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="97a29-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="97a29-220">Zapisać zmiany w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="97a29-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="97a29-221">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**.</span><span class="sxs-lookup"><span data-stu-id="97a29-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="97a29-222">**Team Explorer** jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="97a29-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="97a29-223">Wpisz wiadomość, zatwierdzania, takich jak `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="97a29-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="97a29-224">Naciśnij klawisz **zatwierdzania** przycisk, aby zatwierdzić zmiany projektu.</span><span class="sxs-lookup"><span data-stu-id="97a29-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="97a29-225">Wybierz **Home** > **synchronizacji** > **akcje** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="97a29-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="97a29-226">Alternatywnie, Wypchnij zmiany z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzając polecenie git.</span><span class="sxs-lookup"><span data-stu-id="97a29-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="97a29-227">Przykład:</span><span class="sxs-lookup"><span data-stu-id="97a29-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="97a29-228">Widok zaktualizowano aplikację sieci web na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="97a29-228">View the updated web app in Azure</span></span>

<span data-ttu-id="97a29-229">Zaktualizowano aplikację sieci web wyświetlić, wybierając **Przeglądaj** z bloku aplikacja sieci web w portalu Azure lub przez otwarcie przeglądarki i wprowadzić adres URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97a29-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="97a29-230">Przykład: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="97a29-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97a29-231">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="97a29-231">Additional resources</span></span>

* [<span data-ttu-id="97a29-232">VSTS umożliwia tworzenie i publikowanie aplikacji sieci Web platformy Azure przy użyciu ciągłe wdrażanie</span><span class="sxs-lookup"><span data-stu-id="97a29-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="97a29-233">Program Kudu projektu</span><span class="sxs-lookup"><span data-stu-id="97a29-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
