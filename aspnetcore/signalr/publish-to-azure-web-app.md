---
title: Opublikuj aplikację SignalR ASP.NET Core w Azure App Service
author: bradygaster
description: Dowiedz się, jak opublikować aplikację SignalR ASP.NET Core w Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: d03a007ca883b3d0391b848e3e92c90469ee640a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963931"
---
# <a name="publish-an-aspnet-core-opno-locsignalr-app-to-azure-app-service"></a><span data-ttu-id="25552-103">Opublikuj aplikację SignalR ASP.NET Core w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="25552-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="25552-104">Autor [Brady gastera](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="25552-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="25552-105">[Azure App Service](/azure/app-service/app-service-web-overview) to usługa platformy [obliczeniowej w chmurze firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, w tym ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25552-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="25552-106">Ten artykuł odnosi się do publikowania aplikacji ASP.NET Core SignalR z poziomu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25552-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="25552-107">Aby uzyskać więcej informacji, zobacz [SignalR Service for Azure](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="25552-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="25552-108">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="25552-108">Publish the app</span></span>

<span data-ttu-id="25552-109">W tym artykule opisano Publikowanie przy użyciu narzędzi w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25552-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="25552-110">Visual Studio Code użytkownicy mogą używać poleceń [interfejsu wiersza polecenia platformy Azure](/cli/azure) do publikowania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="25552-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="25552-111">Aby uzyskać więcej informacji, zobacz temat [publikowanie aplikacji ASP.NET Core na platformie Azure przy użyciu narzędzi wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="25552-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="25552-112">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="25552-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="25552-113">Upewnij się, że w oknie dialogowym **Wybieranie elementu docelowego publikowania** są wybrane **App Service** i **Utwórz nowe** .</span><span class="sxs-lookup"><span data-stu-id="25552-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="25552-114">Wybierz pozycję **Utwórz profil** na liście rozwijanej przycisk **Publikuj** .</span><span class="sxs-lookup"><span data-stu-id="25552-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="25552-115">Wprowadź informacje opisane w poniższej tabeli w oknie dialogowym **tworzenie App Service** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="25552-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="25552-116">Element</span><span class="sxs-lookup"><span data-stu-id="25552-116">Item</span></span>               | <span data-ttu-id="25552-117">Opis</span><span class="sxs-lookup"><span data-stu-id="25552-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="25552-118">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="25552-118">**Name**</span></span>           | <span data-ttu-id="25552-119">Unikatowa nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25552-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="25552-120">**Ramach**</span><span class="sxs-lookup"><span data-stu-id="25552-120">**Subscription**</span></span>   | <span data-ttu-id="25552-121">Subskrypcja platformy Azure, której używa aplikacja.</span><span class="sxs-lookup"><span data-stu-id="25552-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="25552-122">**Grupa zasobów**</span><span class="sxs-lookup"><span data-stu-id="25552-122">**Resource Group**</span></span> | <span data-ttu-id="25552-123">Grupa powiązanych zasobów, do której należy aplikacja.</span><span class="sxs-lookup"><span data-stu-id="25552-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="25552-124">**Plan hostingu**</span><span class="sxs-lookup"><span data-stu-id="25552-124">**Hosting Plan**</span></span>   | <span data-ttu-id="25552-125">Plan cenowy dla aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="25552-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="25552-126">Wybierz **usługę Azure SignalR** w obszarze **zależności** > **Dodaj** listę rozwijaną:</span><span class="sxs-lookup"><span data-stu-id="25552-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Obszar zależności pokazujący wybór platformy Azure [! OP. Usługa NO-LOC (Signaler)] na liście rozwijanej dodawania](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="25552-128">W oknie dialogowym **usługa SignalR Azure** wybierz pozycję **Utwórz nowe wystąpienie usługi Azure SignalR** .</span><span class="sxs-lookup"><span data-stu-id="25552-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="25552-129">Podaj **nazwę**, **grupę zasobów**i **lokalizację**.</span><span class="sxs-lookup"><span data-stu-id="25552-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="25552-130">Wróć do okna dialogowego **usługi Azure SignalR** i wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="25552-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="25552-131">Program Visual Studio wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="25552-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="25552-132">Tworzy profil publikacji zawierający ustawienia publikowania.</span><span class="sxs-lookup"><span data-stu-id="25552-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="25552-133">Tworzy *aplikację internetową platformy Azure* z podanymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="25552-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="25552-134">Publikuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="25552-134">Publishes the app.</span></span>
* <span data-ttu-id="25552-135">Uruchamia przeglądarkę, która ładuje aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="25552-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="25552-136">Format adresu URL aplikacji jest `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="25552-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="25552-137">Na przykład aplikacja o nazwie `SignalRChatApp` ma adres URL `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="25552-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="25552-138">502,2 Jeśli podczas wdrażania aplikacji przeznaczonej dla wersji zapoznawczej programu .NET Core wystąpi błąd *nieprawidłowej bramy* , zapoznaj się z artykułem [Wdróż ASP.NET Core wersja zapoznawcza, aby Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="25552-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="25552-139">Skonfiguruj aplikację w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="25552-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="25552-140">*Ta sekcja dotyczy tylko aplikacji, które nie korzystają z usługi Azure SignalR.*</span><span class="sxs-lookup"><span data-stu-id="25552-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="25552-141">Jeśli aplikacja używa usługi Azure SignalR, App Service nie wymaga konfiguracji koligacji i sieci Web usługi Routing żądań aplikacji opisanych w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="25552-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="25552-142">Klienci łączą swoje gniazda internetowe z usługą SignalR platformy Azure, nie bezpośrednio z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="25552-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="25552-143">W przypadku aplikacji hostowanych bez usługi Azure SignalR należy włączyć:</span><span class="sxs-lookup"><span data-stu-id="25552-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="25552-144">[Koligacja ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) do kierowania żądań od użytkownika z powrotem do tego samego wystąpienia App Service.</span><span class="sxs-lookup"><span data-stu-id="25552-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="25552-145">Ustawieniem domyślnym jest **włączone**.</span><span class="sxs-lookup"><span data-stu-id="25552-145">The default setting is **On**.</span></span>
* <span data-ttu-id="25552-146">[Gniazda sieci Web](xref:fundamentals/websockets) , aby umożliwić transport gniazd sieci Web.</span><span class="sxs-lookup"><span data-stu-id="25552-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="25552-147">Ustawienie domyślne jest **wyłączone**.</span><span class="sxs-lookup"><span data-stu-id="25552-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="25552-148">W Azure Portal przejdź do aplikacji sieci Web w **App Services**.</span><span class="sxs-lookup"><span data-stu-id="25552-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="25552-149">Otwórz > konfiguracja **Ustawienia ogólne**.</span><span class="sxs-lookup"><span data-stu-id="25552-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="25552-150">Ustaw dla opcji **gniazda sieci Web** wartość **włączone**.</span><span class="sxs-lookup"><span data-stu-id="25552-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="25552-151">Sprawdź, czy **koligacja ARR** jest ustawiona na wartość **włączone**.</span><span class="sxs-lookup"><span data-stu-id="25552-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="25552-152">Limity planu App Service</span><span class="sxs-lookup"><span data-stu-id="25552-152">App Service Plan limits</span></span>

<span data-ttu-id="25552-153">Gniazda sieci Web i inne transporty są ograniczone w zależności od wybranego planu App Service.</span><span class="sxs-lookup"><span data-stu-id="25552-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="25552-154">Aby uzyskać więcej informacji, zapoznaj się z sekcją *limity Cloud Services platformy Azure* i *App Service limity* dotyczące [subskrypcji, limitów, przydziałów i ograniczeń usługi](/azure/azure-subscription-service-limits#app-service-limits) Azure.</span><span class="sxs-lookup"><span data-stu-id="25552-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25552-155">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="25552-155">Additional resources</span></span>

* <span data-ttu-id="25552-156">[Co to jest usługa Azure SignalR Service?](/azure/azure-signalr/signalr-overview)</span><span class="sxs-lookup"><span data-stu-id="25552-156">[What is Azure SignalR Service?](/azure/azure-signalr/signalr-overview)</span></span>
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="25552-157">Publikowanie aplikacji ASP.NET Core na platformie Azure przy użyciu narzędzi wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="25552-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="25552-158">Hostowanie i wdrażanie aplikacji ASP.NET Core w wersji zapoznawczej na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="25552-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
