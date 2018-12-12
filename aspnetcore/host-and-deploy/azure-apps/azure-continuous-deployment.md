---
title: Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git na platformie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio, a następnie wdrożyć ją w usłudze Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: e12c2ee0b78db105b431770e8644e7d19d915765
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284446"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="48f9e-103">Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48f9e-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="48f9e-104">przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="48f9e-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="48f9e-105">Ten samouczek pokazuje, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio i wdrożyć ją w programie Visual Studio w usłudze Azure App Service za pomocą ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="48f9e-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="48f9e-106">Zobacz też [Tworzenie pierwszego potoku za pomocą potoków usługi Azure](/azure/devops/pipelines/get-started-yaml), który pokazuje, jak skonfigurować ciągłe dostarczanie (CD) przepływ pracy dla [usługi Azure App Service](/azure/app-service/app-service-web-overview) przy użyciu usługi DevOps platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-106">See also [Create your first pipeline with Azure Pipelines](/azure/devops/pipelines/get-started-yaml), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Azure DevOps Services.</span></span> <span data-ttu-id="48f9e-107">Potoki usługi Azure (usługa usługom DevOps platformy Azure) upraszcza konfigurowanie niezawodnego potoku wdrażania aktualizacji dla aplikacji hostowanych w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="48f9e-107">Azure Pipelines (an Azure DevOps Services service) simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="48f9e-108">Potok można skonfigurować w witrynie Azure portal do kompilacji, uruchom testy, wdrożyć w miejscu przejściowym i następnie wdrażania w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="48f9e-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="48f9e-109">Do ukończenia tego samouczka, wymagane jest konto Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="48f9e-110">Aby utworzyć konta [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="48f9e-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48f9e-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="48f9e-111">Prerequisites</span></span>

<span data-ttu-id="48f9e-112">W tym samouczku przyjęto założenie, że zainstalowano następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="48f9e-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="48f9e-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48f9e-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="48f9e-114">[Git](https://git-scm.com/downloads) dla Windows</span><span class="sxs-lookup"><span data-stu-id="48f9e-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="48f9e-115">Tworzenie aplikacji internetowej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48f9e-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="48f9e-116">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48f9e-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="48f9e-117">Z **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="48f9e-118">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="48f9e-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="48f9e-119">Wygląda na to, w obszarze **zainstalowane** > **szablony** > **Visual C#** > **platformy .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="48f9e-120">Nadaj projektowi nazwę `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="48f9e-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="48f9e-121">Wybierz **Utwórz nowe repozytorium Git** opcji, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Okno dialogowe nowego projektu](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="48f9e-123">W **nowy projekt programu ASP.NET Core** okno dialogowe, wybierz pozycję ASP.NET Core **pusty** szablonu, następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Okno dialogowe nowego projektu programu ASP.NET Core](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="48f9e-125">Najbardziej aktualną wersją platformy .NET Core jest w wersji 2.0.</span><span class="sxs-lookup"><span data-stu-id="48f9e-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="48f9e-126">Uruchamianie aplikacji sieci web lokalnie</span><span class="sxs-lookup"><span data-stu-id="48f9e-126">Running the web app locally</span></span>

1. <span data-ttu-id="48f9e-127">Po zakończeniu tworzenia aplikacji programu Visual Studio Uruchom aplikację, wybierając **debugowania** > **Rozpocznij debugowanie**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="48f9e-128">Alternatywnie, naciśnij klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="48f9e-129">Może potrwać do zainicjowania programu Visual Studio i nową aplikację.</span><span class="sxs-lookup"><span data-stu-id="48f9e-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="48f9e-130">Po zakończeniu przeglądarce pokazuje uruchomionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48f9e-130">Once it's complete, the browser shows the running app.</span></span>

   ![Wyświetlanie okna przeglądarki uruchomiona aplikacja, która wyświetla "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="48f9e-132">Po zapoznaniu się z działającą aplikacją sieci Web, zamknij przeglądarkę, a następnie wybierz ikonę "Zatrzymaj debugowanie" na pasku narzędzi programu Visual Studio, aby zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="48f9e-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="48f9e-133">Tworzenie aplikacji internetowej w witrynie Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48f9e-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="48f9e-134">Poniższe kroki umożliwiają utworzenie aplikacji sieci web w witrynie Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="48f9e-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="48f9e-135">Zaloguj się do [witryny Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48f9e-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="48f9e-136">Wybierz **NEW** w lewym górnym rogu portalu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="48f9e-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="48f9e-137">Wybierz **sieci Web i mobilność** > **aplikacja sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portal Microsoft Azure: Nowy przycisk: Sieć Web i mobilność w portalu Marketplace: Przycisk aplikacji sieci Web w obszarze polecane aplikacje](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="48f9e-139">W **aplikacji sieci Web** bloku, należy wprowadzić unikatową wartość dla **nazwa usługi App Service**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Bloku aplikacji sieci Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="48f9e-141">**Nazwa usługi App Service** nazwa musi być unikatowa.</span><span class="sxs-lookup"><span data-stu-id="48f9e-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="48f9e-142">Portal wymusza tę regułę, gdy została podana nazwa.</span><span class="sxs-lookup"><span data-stu-id="48f9e-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="48f9e-143">Jeśli podając inną wartość, Zastąp tę wartość dla każdego wystąpienia **SampleWebAppDemo** w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="48f9e-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="48f9e-144">Również w **aplikacji sieci Web** bloku, wybierz istniejącą **Plan App Service/lokalizacja** lub utworzyć nowy.</span><span class="sxs-lookup"><span data-stu-id="48f9e-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="48f9e-145">W przypadku tworzenia nowego planu, wybierz warstwę cenową, lokalizacja i inne opcje.</span><span class="sxs-lookup"><span data-stu-id="48f9e-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="48f9e-146">Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [szczegółowe omówienie planów usługi Azure App Service](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="48f9e-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="48f9e-147">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-147">Select **Create**.</span></span> <span data-ttu-id="48f9e-148">Platforma Azure będzie aprowizować i uruchomić aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="48f9e-148">Azure will provision and start the web app.</span></span>

   ![Witryna Azure Portal: Blok podstawy 01 wersji demonstracyjnej aplikacji sieci Web próbki](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="48f9e-150">Włączanie publikowania usługi Git dla nowej aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="48f9e-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="48f9e-151">Git to Rozproszony system kontroli wersji, który może służyć do wdrażania aplikacji sieci web usługi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="48f9e-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="48f9e-152">Kod aplikacji internetowej jest przechowywany w lokalnym repozytorium Git, a kod jest wdrażany na platformie Azure przez wypchnięcie do repozytorium zdalnego.</span><span class="sxs-lookup"><span data-stu-id="48f9e-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="48f9e-153">Zaloguj się do [witryny Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48f9e-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="48f9e-154">Wybierz **App Services** Aby wyświetlić listę usług aplikacji skojarzonych z subskrypcją platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="48f9e-155">Wybierz aplikację internetową utworzoną w poprzedniej sekcji tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="48f9e-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="48f9e-156">W **wdrożenia** bloku wybierz **opcje wdrażania** > **wybierz źródło** > **lokalnego repozytorium Git**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Blok ustawień: Blok źródła wdrożenia: Wybierz blok źródła](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="48f9e-158">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-158">Select **OK**.</span></span>

1. <span data-ttu-id="48f9e-159">Jeśli poświadczenia wdrażania na potrzeby publikowania aplikacji sieci web lub innych aplikacji usługi app Service nie zostało jeszcze skonfigurowane, skonfiguruj je teraz:</span><span class="sxs-lookup"><span data-stu-id="48f9e-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="48f9e-160">Wybierz **ustawienia** > **poświadczenia wdrożenia**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="48f9e-161">**Ustaw poświadczenia wdrażania** zostanie wyświetlony blok.</span><span class="sxs-lookup"><span data-stu-id="48f9e-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="48f9e-162">Utwórz nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="48f9e-162">Create a user name and password.</span></span> <span data-ttu-id="48f9e-163">Zapisz hasło do późniejszego użycia podczas konfigurowania usługi Git.</span><span class="sxs-lookup"><span data-stu-id="48f9e-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="48f9e-164">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-164">Select **Save**.</span></span>

1. <span data-ttu-id="48f9e-165">W **aplikacji sieci Web** bloku wybierz **ustawienia** > **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="48f9e-166">Adres URL zdalnego repozytorium Git do wdrożenia jest wyświetlany w obszarze **adres URL GIT**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="48f9e-167">Kopiuj **adres URL GIT** wartości do późniejszego użycia w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="48f9e-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Witrynie Azure Portal: blok właściwości aplikacji](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="48f9e-169">Publikowanie aplikacji sieci web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="48f9e-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="48f9e-170">W tej sekcji należy utworzyć lokalne repozytorium Git przy użyciu programu Visual Studio i wypychania z tego repozytorium na platformie Azure do wdrożenia aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="48f9e-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="48f9e-171">Następujące etapy:</span><span class="sxs-lookup"><span data-stu-id="48f9e-171">The steps involved include the following:</span></span>

* <span data-ttu-id="48f9e-172">Dodaj ustawienie repozytorium zdalnego przy użyciu wartości adres URL usługi GIT, aby można było wdrożyć lokalne repozytorium na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="48f9e-173">Zatwierdź zmiany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="48f9e-173">Commit project changes.</span></span>
* <span data-ttu-id="48f9e-174">Wypchnij zmiany projektu z repozytorium lokalnego do repozytorium zdalnego na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="48f9e-175">W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="48f9e-176">**Team Explorer** jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="48f9e-176">The **Team Explorer** is displayed.</span></span>

   ![Team Explorer, Połącz kartę](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="48f9e-178">W **Team Explorer**, wybierz opcję **macierzystego** (ikona macierzystego) > **ustawienia** > **ustawienia repozytorium**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="48f9e-179">W **źródłami zdalnymi** części **ustawienia repozytorium**, wybierz opcję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="48f9e-180">**Dodawanie elementu zdalnego** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="48f9e-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="48f9e-181">Ustaw **nazwa** zdalnego do **Przykładowa aplikacja Azure**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="48f9e-182">Ustaw wartość **pobrać** do **adres URL Git** , skopiowane z platformy Azure we wcześniejszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="48f9e-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="48f9e-183">Należy pamiętać, że ten adres URL, który kończy się **.git**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Zdalne okno dialogowe Edycja](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="48f9e-185">Jako alternatywę, można określić zdalnego repozytorium z **okna polecenia** , otwierając **okna polecenia**zmiany do katalogu projektu i wprowadzając polecenie.</span><span class="sxs-lookup"><span data-stu-id="48f9e-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="48f9e-186">Przykład:</span><span class="sxs-lookup"><span data-stu-id="48f9e-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="48f9e-187">Wybierz **macierzystego** (ikona macierzystego) > **ustawienia** > **ustawienia globalne**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="48f9e-188">Upewnij się, że nazwa i adres e-mail są ustawione.</span><span class="sxs-lookup"><span data-stu-id="48f9e-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="48f9e-189">Wybierz **aktualizacji** w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="48f9e-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="48f9e-190">Wybierz **Home** > **zmiany** aby powrócić do **zmiany** widoku.</span><span class="sxs-lookup"><span data-stu-id="48f9e-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="48f9e-191">Wprowadź wiadomość dotyczącą zatwierdzenia, taką jak **początkowej wypychania nr 1** i wybierz **zatwierdzenia**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="48f9e-192">Ta akcja powoduje utworzenie *zatwierdzenia* lokalnie.</span><span class="sxs-lookup"><span data-stu-id="48f9e-192">This action creates a *commit* locally.</span></span>

   ![Team Explorer, Połącz kartę](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="48f9e-194">Jako alternatywę zatwierdzenia zmieni się z **okna polecenia** , otwierając **okna polecenia**zmiany do katalogu projektu i wprowadzania poleceń usługi git.</span><span class="sxs-lookup"><span data-stu-id="48f9e-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="48f9e-195">Przykład:</span><span class="sxs-lookup"><span data-stu-id="48f9e-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="48f9e-196">Wybierz **Home** > **synchronizacji** > **akcje** > **Otwórz wiersz polecenia**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="48f9e-197">Wiersz polecenia otwiera się w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="48f9e-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="48f9e-198">Wprowadź następujące polecenie w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="48f9e-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="48f9e-199">Wprowadź Azure **poświadczenia wdrożenia** hasło utworzone wcześniej na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="48f9e-200">To polecenie uruchamia proces wypychania pliki projektu lokalnych na platformę Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="48f9e-201">Dane wyjściowe z powyższego polecenia kończy się z komunikatem, że wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="48f9e-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="48f9e-202">Jeśli wymagana jest współpracy nad projektem, należy wziąć pod uwagę Wypychanie do [GitHub](https://github.com) przed wypchnięciem do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="48f9e-203">Sprawdź aktywne wdrożenie</span><span class="sxs-lookup"><span data-stu-id="48f9e-203">Verify the Active Deployment</span></span>

<span data-ttu-id="48f9e-204">Sprawdź, czy powiodła się przesyłanie aplikacji sieci web ze środowiska lokalnego do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="48f9e-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="48f9e-205">W [witryny Azure Portal](https://portal.azure.com), wybierz aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="48f9e-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="48f9e-206">Wybierz **wdrożenia** > **opcje wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-206">Select **Deployment** > **Deployment options**.</span></span>

![Witryna Azure Portal: Blok ustawień: Pomyślne wdrożenie przedstawiający bloku wdrożenia](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="48f9e-208">Uruchom aplikację na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="48f9e-208">Run the app in Azure</span></span>

<span data-ttu-id="48f9e-209">Teraz, gdy aplikacja sieci web jest wdrażana na platformie Azure, uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="48f9e-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="48f9e-210">Można to zrobić na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="48f9e-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="48f9e-211">W witrynie Azure Portal zlokalizuj bloku aplikacji sieci web dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="48f9e-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="48f9e-212">Wybierz **Przeglądaj** do wyświetlania aplikacji w domyślnej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="48f9e-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="48f9e-213">Otwórz przeglądarkę i wprowadź adres URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="48f9e-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="48f9e-214">Przykład: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="48f9e-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="48f9e-215">Aktualizowanie aplikacji internetowej i ponowne opublikowanie</span><span class="sxs-lookup"><span data-stu-id="48f9e-215">Update the web app and republish</span></span>

<span data-ttu-id="48f9e-216">Po wprowadzeniu zmian w kodzie lokalnych należy opublikować ponownie:</span><span class="sxs-lookup"><span data-stu-id="48f9e-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="48f9e-217">W **Eksploratora rozwiązań** programu Visual Studio, otwórz *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="48f9e-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="48f9e-218">W `Configure` metody, zmodyfikuj `Response.WriteAsync` metody, aby była ona wyświetlana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="48f9e-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="48f9e-219">Czy zapisać zmiany *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="48f9e-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="48f9e-220">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="48f9e-221">**Team Explorer** jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="48f9e-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="48f9e-222">Wprowadź wiadomość dotyczącą zatwierdzenia, taką jak `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="48f9e-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="48f9e-223">Naciśnij klawisz **zatwierdzenia** przycisk, aby zatwierdzić zmiany projektu.</span><span class="sxs-lookup"><span data-stu-id="48f9e-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="48f9e-224">Wybierz **Home** > **synchronizacji** > **akcje** > **wypychania**.</span><span class="sxs-lookup"><span data-stu-id="48f9e-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="48f9e-225">Jako alternatywę, Wypchnij zmiany z **okna polecenia** , otwierając **okna polecenia**zmiany do katalogu projektu i wprowadzając polecenie git.</span><span class="sxs-lookup"><span data-stu-id="48f9e-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="48f9e-226">Przykład:</span><span class="sxs-lookup"><span data-stu-id="48f9e-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="48f9e-227">Wyświetlanie zaktualizowanej aplikacji internetowej na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="48f9e-227">View the updated web app in Azure</span></span>

<span data-ttu-id="48f9e-228">Wyświetlanie zaktualizowanej aplikacji internetowej, wybierając **Przeglądaj** z bloku aplikacji sieci web w witrynie Azure Portal lub otwierając przeglądarkę i wprowadź adres URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="48f9e-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="48f9e-229">Przykład: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="48f9e-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48f9e-230">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48f9e-230">Additional resources</span></span>

* [<span data-ttu-id="48f9e-231">Tworzenie pierwszego potoku za pomocą potoków usługi Azure</span><span class="sxs-lookup"><span data-stu-id="48f9e-231">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)
* [<span data-ttu-id="48f9e-232">Projekt Kudu</span><span class="sxs-lookup"><span data-stu-id="48f9e-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* <xref:host-and-deploy/visual-studio-publish-profiles>
