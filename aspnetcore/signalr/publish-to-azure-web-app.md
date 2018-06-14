---
title: Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure
author: rachelappel
description: Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 8612824cc9d4a9ace1c214411c44754350100f06
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341811"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="dfaa9-103">Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="dfaa9-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="dfaa9-104">[Azure Web App](/azure/app-service/app-service-web-overview) jest [firmy Microsoft w chmurze obliczeniowych](https://azure.microsoft.com/) usługi platforma do obsługi aplikacji sieci web, w tym platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="dfaa9-105">Ten artykuł dotyczy publikowania aplikacji ASP.NET Core SignalR z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="dfaa9-106">Odwiedź stronę [usługi SignalR platformy Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) Aby uzyskać więcej informacji o korzystaniu z SignalR na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="dfaa9-107">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="dfaa9-107">Publish the app</span></span>

<span data-ttu-id="dfaa9-108">Program Visual Studio oferuje wbudowane narzędzia do publikowania aplikacji sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="dfaa9-109">Visual Studio Code użytkownik może użyć [interfejsu wiersza polecenia Azure](/cli/azure) poleceń do publikowania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="dfaa9-110">W tym artykule omówiono publikowania za pomocą narzędzi w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="dfaa9-111">Aby opublikować aplikację przy użyciu interfejsu wiersza polecenia Azure, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="dfaa9-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="dfaa9-112">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="dfaa9-113">Upewnij się, że **Utwórz nowy** zaewidencjonowania **wybierz element docelowy publikowania** okna dialogowego, a następnie wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Pobranie publikowania docelowego](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="dfaa9-115">Wprowadź następujące informacje w **Tworzenie usługi App Service** okno dialogowe i wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="dfaa9-116">Element</span><span class="sxs-lookup"><span data-stu-id="dfaa9-116">Item</span></span> | <span data-ttu-id="dfaa9-117">Opis</span><span class="sxs-lookup"><span data-stu-id="dfaa9-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="dfaa9-118">**Nazwa aplikacji**</span><span class="sxs-lookup"><span data-stu-id="dfaa9-118">**App name**</span></span> | <span data-ttu-id="dfaa9-119">Unikatowa nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-119">A unique name of the app.</span></span> |
| <span data-ttu-id="dfaa9-120">**Subskrypcji**</span><span class="sxs-lookup"><span data-stu-id="dfaa9-120">**Subscription**</span></span> | <span data-ttu-id="dfaa9-121">Subskrypcja platformy Azure, który korzysta z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="dfaa9-122">**Grupy zasobów**</span><span class="sxs-lookup"><span data-stu-id="dfaa9-122">**Resource Group**</span></span> | <span data-ttu-id="dfaa9-123">Grupa powiązane zasoby, do której należy aplikacja.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="dfaa9-124">**Plan hostingu**</span><span class="sxs-lookup"><span data-stu-id="dfaa9-124">**Hosting Plan**</span></span> | <span data-ttu-id="dfaa9-125">Planu cenowego dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-125">The pricing plan for the web app.</span></span> |

![Tworzenie usługi app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="dfaa9-127">Visual Studio wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="dfaa9-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="dfaa9-128">Tworzy profil publikowania zawierający ustawienia publikowania.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="dfaa9-129">Tworzy lub wykorzystuje istniejące *aplikacji sieci Web Azure* z podanych szczegółów.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="dfaa9-130">Publikowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-130">Publishes the app.</span></span>
* <span data-ttu-id="dfaa9-131">Uruchamia przeglądarce do aplikacji opublikowanych w sieci web załadowane.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="dfaa9-132">Zwróć uwagę format adresu URL aplikacji jest *{nazwa} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="dfaa9-133">Na przykład aplikacji o nazwie `SignalRChattR` ma adres URL, który wygląda jak `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="dfaa9-134">Jeśli wystąpi błąd HTTP 502.2, zobacz [wersji zapoznawczej wdrażania platformy ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index) go rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="dfaa9-135">Konfigurowanie aplikacji sieci web SignalR</span><span class="sxs-lookup"><span data-stu-id="dfaa9-135">Configure SignalR web app</span></span>

<span data-ttu-id="dfaa9-136">ASP.NET Core SignalR aplikacje, które są publikowane jako aplikacji sieci Web platformy Azure musi mieć [koligacji ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) włączone.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="dfaa9-137">[Protokół WebSockets](xref:fundamentals/websockets) powinno być włączone, aby umożliwić transportu Websocket do funkcji.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="dfaa9-138">W portalu Azure, przejdź do **ustawień aplikacji** dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="dfaa9-139">Ustaw **Websocket** do **na**i sprawdź **koligacji ARR** jest **na**.</span><span class="sxs-lookup"><span data-stu-id="dfaa9-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure ustawień aplikacji sieci Web w portalu Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="dfaa9-141">Protokół WebSockets i innych transportów [są ograniczone w oparciu o Plan usługi aplikacji](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="dfaa9-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="dfaa9-142">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="dfaa9-142">Related resources</span></span>

* [<span data-ttu-id="dfaa9-143">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="dfaa9-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="dfaa9-144">Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfaa9-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="dfaa9-145">Host i wdrażanie aplikacji platformy ASP.NET Core Preview na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="dfaa9-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
