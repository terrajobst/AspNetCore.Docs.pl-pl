---
title: Publikowanie platformy ASP.NET Core app SignalR do aplikacji sieci Web platformy Azure
author: bradygaster
description: Publikowanie platformy ASP.NET Core app SignalR do aplikacji sieci Web platformy Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837679"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="4880e-103">Publikowanie platformy ASP.NET Core aplikacji SignalR na aplikację internetową platformy Azure</span><span class="sxs-lookup"><span data-stu-id="4880e-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="4880e-104">[Usługa Azure Web Apps](/azure/app-service/app-service-web-overview) jest [przetwarzanie w chmurze firmy Microsoft](https://azure.microsoft.com/) usługa platformy do hostowania aplikacji sieci web, w tym platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4880e-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="4880e-105">Ten artykuł odnosi się do publikowania aplikacji biblioteki SignalR platformy ASP.NET Core w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4880e-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="4880e-106">Odwiedź stronę [usługi SignalR platformy Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) Aby uzyskać więcej informacji na temat przy użyciu biblioteki SignalR na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="4880e-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="4880e-107">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4880e-107">Publish the app</span></span>

<span data-ttu-id="4880e-108">Visual Studio udostępnia wbudowane narzędzia do publikowania aplikacji sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4880e-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="4880e-109">Visual Studio Code użytkownik może używać [wiersza polecenia platformy Azure](/cli/azure) polecenia, aby opublikować aplikacje na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="4880e-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="4880e-110">W tym artykule opisano publikowania za pomocą narzędzi w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4880e-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="4880e-111">Aby opublikować aplikację przy użyciu wiersza polecenia platformy Azure, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="4880e-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="4880e-112">Kliknij prawym przyciskiem myszy nad projektem w **Eksploratora rozwiązań** i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="4880e-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="4880e-113">Upewnij się, że **Utwórz nową** jest zaewidencjonowany **wybierz lokalizację docelową publikowania** okna dialogowego, a następnie wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="4880e-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Pobranie publikowania docelowej](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="4880e-115">Wprowadź następujące informacje w **Tworzenie usługi App Service** okna dialogowego, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4880e-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="4880e-116">Element</span><span class="sxs-lookup"><span data-stu-id="4880e-116">Item</span></span> | <span data-ttu-id="4880e-117">Opis</span><span class="sxs-lookup"><span data-stu-id="4880e-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="4880e-118">**Nazwa aplikacji**</span><span class="sxs-lookup"><span data-stu-id="4880e-118">**App name**</span></span> | <span data-ttu-id="4880e-119">Unikatowa nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4880e-119">A unique name of the app.</span></span> |
| <span data-ttu-id="4880e-120">**Subskrypcja**</span><span class="sxs-lookup"><span data-stu-id="4880e-120">**Subscription**</span></span> | <span data-ttu-id="4880e-121">Subskrypcja platformy Azure, przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="4880e-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="4880e-122">**Grupa zasobów**</span><span class="sxs-lookup"><span data-stu-id="4880e-122">**Resource Group**</span></span> | <span data-ttu-id="4880e-123">Grupa powiązane zasoby, do których należy aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4880e-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="4880e-124">**Plan hostingu**</span><span class="sxs-lookup"><span data-stu-id="4880e-124">**Hosting Plan**</span></span> | <span data-ttu-id="4880e-125">Plan taryfowy aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4880e-125">The pricing plan for the web app.</span></span> |

![Utwórz usługę app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="4880e-127">Program Visual Studio wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="4880e-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="4880e-128">Tworzy profil publikowania zawierający ustawienia publikowania.</span><span class="sxs-lookup"><span data-stu-id="4880e-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="4880e-129">Tworzy i wykorzystuje istniejące *aplikacji sieci Web platformy Azure* przy użyciu podanych szczegółów.</span><span class="sxs-lookup"><span data-stu-id="4880e-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="4880e-130">Publikuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="4880e-130">Publishes the app.</span></span>
* <span data-ttu-id="4880e-131">Otworzy w przeglądarce, za pomocą aplikacji sieci web opublikowanych załadowane.</span><span class="sxs-lookup"><span data-stu-id="4880e-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="4880e-132">Zwróć uwagę format adresu URL aplikacji jest *{Nazwa aplikacji} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="4880e-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="4880e-133">Na przykład aplikacji o nazwie `SignalRChattR` ma adres URL, który wygląda jak `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="4880e-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="4880e-134">Jeśli wystąpi błąd HTTP 502.2, zobacz [wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index) go rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="4880e-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="4880e-135">Konfigurowanie aplikacji sieci web SignalR</span><span class="sxs-lookup"><span data-stu-id="4880e-135">Configure SignalR web app</span></span>

<span data-ttu-id="4880e-136">ASP.NET Core SignalR aplikacje, które są publikowane jako aplikację internetową platformy Azure musi mieć [koligacja ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) włączone.</span><span class="sxs-lookup"><span data-stu-id="4880e-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="4880e-137">[Gniazda Websocket](xref:fundamentals/websockets) powinna mieć możliwość, Zezwalaj na transport gniazda Websocket funkcji.</span><span class="sxs-lookup"><span data-stu-id="4880e-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="4880e-138">W witrynie Azure portal przejdź do **ustawienia aplikacji** dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4880e-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="4880e-139">Ustaw **WebSockets** do **na**i sprawdź **koligacja ARR** jest **na**.</span><span class="sxs-lookup"><span data-stu-id="4880e-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Ustawienia aplikacji sieci Web usługi Azure w witrynie Azure portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="4880e-141">Gniazda Websocket i innych rodzajów transportu [są ograniczone w oparciu o Plan usługi App Service](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="4880e-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="4880e-142">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="4880e-142">Related resources</span></span>

* [<span data-ttu-id="4880e-143">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="4880e-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="4880e-144">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4880e-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="4880e-145">Hostowanie i wdrażanie aplikacji ASP.NET Core w wersji zapoznawczej na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="4880e-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
