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
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="182c9-103">Publikowanie platformy ASP.NET Core aplikacji SignalR do aplikacji sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="182c9-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="182c9-104">[Azure Web App](/azure/app-service/app-service-web-overview) jest [firmy Microsoft w chmurze obliczeniowych](https://azure.microsoft.com/) usługi platforma do obsługi aplikacji sieci web, w tym platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="182c9-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="182c9-105">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="182c9-105">Publish the app</span></span>

<span data-ttu-id="182c9-106">Program Visual Studio oferuje wbudowane narzędzia do publikowania aplikacji sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="182c9-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="182c9-107">Visual Studio Code użytkownik może użyć [interfejsu wiersza polecenia Azure](/cli/azure) poleceń do publikowania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="182c9-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="182c9-108">W tym artykule omówiono publikowania za pomocą narzędzi w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="182c9-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="182c9-109">Aby opublikować aplikację przy użyciu interfejsu wiersza polecenia Azure, zobacz [publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="182c9-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="182c9-110">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="182c9-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="182c9-111">Upewnij się, że **Utwórz nowy** zaewidencjonowania **wybierz element docelowy publikowania** okna dialogowego, a następnie wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="182c9-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Pobranie publikowania docelowego](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="182c9-113">Wprowadź następujące informacje w **Tworzenie usługi App Service** okno dialogowe i wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="182c9-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="182c9-114">Element</span><span class="sxs-lookup"><span data-stu-id="182c9-114">Item</span></span> | <span data-ttu-id="182c9-115">Opis</span><span class="sxs-lookup"><span data-stu-id="182c9-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="182c9-116">**Nazwa aplikacji**</span><span class="sxs-lookup"><span data-stu-id="182c9-116">**App name**</span></span> | <span data-ttu-id="182c9-117">Unikatowa nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="182c9-117">A unique name of the app.</span></span> |
| <span data-ttu-id="182c9-118">**Subskrypcji**</span><span class="sxs-lookup"><span data-stu-id="182c9-118">**Subscription**</span></span> | <span data-ttu-id="182c9-119">Subskrypcja platformy Azure, który korzysta z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="182c9-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="182c9-120">**Grupy zasobów**</span><span class="sxs-lookup"><span data-stu-id="182c9-120">**Resource Group**</span></span> | <span data-ttu-id="182c9-121">Grupa powiązane zasoby, do której należy aplikacja.</span><span class="sxs-lookup"><span data-stu-id="182c9-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="182c9-122">**Plan hostingu**</span><span class="sxs-lookup"><span data-stu-id="182c9-122">**Hosting Plan**</span></span> | <span data-ttu-id="182c9-123">Planu cenowego dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="182c9-123">The pricing plan for the web app.</span></span> |

![Tworzenie usługi app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="182c9-125">Visual Studio wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="182c9-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="182c9-126">Tworzy profil publikowania zawierający ustawienia publikowania.</span><span class="sxs-lookup"><span data-stu-id="182c9-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="182c9-127">Tworzy lub wykorzystuje istniejące *aplikacji sieci Web Azure* z podanych szczegółów.</span><span class="sxs-lookup"><span data-stu-id="182c9-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="182c9-128">Publikowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="182c9-128">Publishes the app.</span></span>
* <span data-ttu-id="182c9-129">Uruchamia przeglądarce do aplikacji opublikowanych w sieci web załadowane.</span><span class="sxs-lookup"><span data-stu-id="182c9-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="182c9-130">Zwróć uwagę format adresu URL aplikacji jest *{nazwa} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="182c9-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="182c9-131">Na przykład aplikacji o nazwie `SignalRChattR` ma adres URL, który wygląda jak `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="182c9-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="182c9-132">Jeśli wystąpi błąd HTTP 502.2, zobacz [wersji zapoznawczej wdrażania platformy ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/index) go rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="182c9-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="182c9-133">Konfigurowanie aplikacji sieci web SignalR</span><span class="sxs-lookup"><span data-stu-id="182c9-133">Configure SignalR web app</span></span>

<span data-ttu-id="182c9-134">ASP.NET Core SignalR aplikacje, które są publikowane jako aplikacji sieci Web platformy Azure musi mieć [koligacji ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) włączone.</span><span class="sxs-lookup"><span data-stu-id="182c9-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="182c9-135">[Protokół WebSockets](xref:fundamentals/websockets) powinno być włączone, aby umożliwić transportu Websocket do funkcji.</span><span class="sxs-lookup"><span data-stu-id="182c9-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="182c9-136">W portalu Azure, przejdź do **ustawień aplikacji** dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="182c9-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="182c9-137">Ustaw **Websocket** do **na**i sprawdź **koligacji ARR** jest **na**.</span><span class="sxs-lookup"><span data-stu-id="182c9-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure ustawień aplikacji sieci Web w portalu Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="182c9-139">Protokół WebSockets i innych transportów [są ograniczone w oparciu o Plan usługi aplikacji](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="182c9-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="182c9-140">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="182c9-140">Related resources</span></span>

* [<span data-ttu-id="182c9-141">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure za pomocą narzędzia wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="182c9-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="182c9-142">Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="182c9-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="182c9-143">Host i wdrażanie aplikacji platformy ASP.NET Core Preview na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="182c9-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
