---
title: Publikowanie platformy ASP.NET Core aplikacji SignalR w usłudze Azure App Service
author: bradygaster
description: Dowiedz się, jak opublikować aplikację biblioteki SignalR platformy ASP.NET Core w usłudze Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406110"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a><span data-ttu-id="73901-103">Publikowanie platformy ASP.NET Core aplikacji SignalR w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="73901-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="73901-104">Przez [Brady'ego Gastera](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="73901-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="73901-105">[Usługa Azure App Service](/azure/app-service/app-service-web-overview) jest [przetwarzanie w chmurze firmy Microsoft](https://azure.microsoft.com/) usługa platformy do hostowania aplikacji sieci web, w tym platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73901-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="73901-106">Ten artykuł odnosi się do publikowania aplikacji biblioteki SignalR platformy ASP.NET Core w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73901-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="73901-107">Aby uzyskać więcej informacji, zobacz [usługi SignalR platformy Azure](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="73901-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="73901-108">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="73901-108">Publish the app</span></span>

<span data-ttu-id="73901-109">W tym artykule opisano publikowania za pomocą narzędzi w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73901-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="73901-110">Visual Studio Code użytkownicy mogą używać [wiersza polecenia platformy Azure](/cli/azure) polecenia, aby opublikować aplikacje na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="73901-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="73901-111">Aby uzyskać więcej informacji, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="73901-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="73901-112">Kliknij prawym przyciskiem myszy nad projektem w **Eksploratora rozwiązań** i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="73901-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="73901-113">Upewnij się, że **usługi App Service** i **Utwórz nową** są zaznaczone w **wybierz lokalizację docelową publikowania** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="73901-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="73901-114">Wybierz **Utwórz profil** z **Publikuj** przycisk listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="73901-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="73901-115">Wprowadź informacje opisane w poniższej tabeli w **Tworzenie usługi App Service** okna dialogowego, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="73901-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="73901-116">Element</span><span class="sxs-lookup"><span data-stu-id="73901-116">Item</span></span>               | <span data-ttu-id="73901-117">Opis</span><span class="sxs-lookup"><span data-stu-id="73901-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="73901-118">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="73901-118">**Name**</span></span>           | <span data-ttu-id="73901-119">Unikatowa nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="73901-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="73901-120">**Subskrypcja**</span><span class="sxs-lookup"><span data-stu-id="73901-120">**Subscription**</span></span>   | <span data-ttu-id="73901-121">Subskrypcja platformy Azure przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="73901-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="73901-122">**Grupa zasobów**</span><span class="sxs-lookup"><span data-stu-id="73901-122">**Resource Group**</span></span> | <span data-ttu-id="73901-123">Grupa powiązane zasoby, do których należy aplikacja.</span><span class="sxs-lookup"><span data-stu-id="73901-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="73901-124">**Plan hostingu**</span><span class="sxs-lookup"><span data-stu-id="73901-124">**Hosting Plan**</span></span>   | <span data-ttu-id="73901-125">Plan cenowy dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="73901-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="73901-126">Wybierz **usługi Azure SignalR Service** w **zależności** > **Dodaj** listy rozwijanej:</span><span class="sxs-lookup"><span data-stu-id="73901-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Obszar zależności przedstawiający Wybieranie usługi Azure SignalR Service na liście rozwijanej Dodaj](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="73901-128">W **usługi Azure SignalR Service** okno dialogowe, wybierz opcję **Utwórz nowe wystąpienie usługi Azure SignalR Service**.</span><span class="sxs-lookup"><span data-stu-id="73901-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="73901-129">Podaj **nazwa**, **grupy zasobów**, i **lokalizacji**.</span><span class="sxs-lookup"><span data-stu-id="73901-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="73901-130">Wróć do **usługi Azure SignalR Service** okna dialogowego, a następnie wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="73901-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="73901-131">Program Visual Studio wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="73901-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="73901-132">Tworzy profil publikowania zawierający ustawienia publikowania.</span><span class="sxs-lookup"><span data-stu-id="73901-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="73901-133">Tworzy *aplikacji sieci Web platformy Azure* przy użyciu podanych szczegółów.</span><span class="sxs-lookup"><span data-stu-id="73901-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="73901-134">Publikuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="73901-134">Publishes the app.</span></span>
* <span data-ttu-id="73901-135">Otworzy w przeglądarce, która ładuje aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="73901-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="73901-136">Format adresu URL aplikacji jest `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="73901-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="73901-137">Na przykład aplikacji o nazwie `SignalRChatApp` ma adres URL z `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="73901-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="73901-138">Jeśli HTTP *502.2 — Zła brama* błąd występuje, gdy wdrażanie aplikacji, który jest przeznaczony dla wersji platformy .NET Core w wersji zapoznawczej, zobacz [wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) go rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="73901-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="73901-139">Skonfiguruj aplikację w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="73901-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="73901-140">*Ta sekcja dotyczy tylko aplikacji nie korzystających z usługi Azure SignalR Service.*</span><span class="sxs-lookup"><span data-stu-id="73901-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="73901-141">Jeśli aplikacja korzysta z usługi Azure SignalR Service, App Service nie wymaga konfiguracji koligacji Routing żądań aplikacji (ARR) i elementy Web Socket opisane w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="73901-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="73901-142">Klienci łączą się ich gniazda sieci Web do usługi Azure SignalR Service, nie są bezpośrednio do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="73901-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="73901-143">W przypadku aplikacji hostowanych bez usługi Azure SignalR Service należy włączyć:</span><span class="sxs-lookup"><span data-stu-id="73901-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="73901-144">[Koligacja ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) rozsyłanie żądań od użytkownika do tego samego wystąpienia usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="73901-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="73901-145">Ustawieniem domyślnym jest **na**.</span><span class="sxs-lookup"><span data-stu-id="73901-145">The default setting is **On**.</span></span>
* <span data-ttu-id="73901-146">[Web Sockets](xref:fundamentals/websockets) umożliwiające transport gniazda sieci Web do funkcji.</span><span class="sxs-lookup"><span data-stu-id="73901-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="73901-147">Ustawieniem domyślnym jest **poza**.</span><span class="sxs-lookup"><span data-stu-id="73901-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="73901-148">W witrynie Azure portal przejdź do aplikacji sieci web w **App Services**.</span><span class="sxs-lookup"><span data-stu-id="73901-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="73901-149">Otwórz **konfiguracji** > **ustawienia ogólne**.</span><span class="sxs-lookup"><span data-stu-id="73901-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="73901-150">Ustaw **Web sockets** do **na**.</span><span class="sxs-lookup"><span data-stu-id="73901-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="73901-151">Upewnij się, że **koligacja ARR** ustawiono **na**.</span><span class="sxs-lookup"><span data-stu-id="73901-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="73901-152">Limity planu usługi App Service</span><span class="sxs-lookup"><span data-stu-id="73901-152">App Service Plan limits</span></span>

<span data-ttu-id="73901-153">Gniazda sieci Web i innych rodzajów transportu są ograniczone oparte na planie usługi App Service wybrane.</span><span class="sxs-lookup"><span data-stu-id="73901-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="73901-154">Aby uzyskać więcej informacji, zobacz *usług Azure Cloud Services ogranicza* i *limity usługi App Service* sekcje [subskrypcji platformy Azure i limity, przydziały i ograniczenia](/azure/azure-subscription-service-limits#app-service-limits) artykuł.</span><span class="sxs-lookup"><span data-stu-id="73901-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73901-155">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="73901-155">Additional resources</span></span>

* [<span data-ttu-id="73901-156">Co to jest usługa Azure SignalR Service?</span><span class="sxs-lookup"><span data-stu-id="73901-156">What is Azure SignalR Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="73901-157">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="73901-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="73901-158">Hostowanie i wdrażanie aplikacji ASP.NET Core w wersji zapoznawczej na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="73901-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
