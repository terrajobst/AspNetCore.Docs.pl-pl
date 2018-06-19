---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikowanie aplikacji w usłudze Azure Azure App Service | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867817"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="d98e4-102">Publikowanie aplikacji w usłudze Azure Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d98e4-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="d98e4-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d98e4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d98e4-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="d98e4-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d98e4-105">Jako ostatni krok opublikuje aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="d98e4-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="d98e4-106">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="d98e4-107">Kliknięcie przycisku **publikowania** wywołuje **publikowanie w sieci Web** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="d98e4-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="d98e4-108">Jeśli zaznaczono **Host w chmurze** po pierwszym tworzeniu projektu, a następnie połączenie i ustawienia są już skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="d98e4-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="d98e4-109">W takim przypadku wystarczy kliknąć **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="d98e4-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="d98e4-110">(Jeśli nie została sprawdzona **Host w chmurze** na początku, a następnie wykonaj kroki opisane w [następnej sekcji](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="d98e4-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="d98e4-111">Aby wdrożyć aplikację, kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="d98e4-112">Możesz wyświetlić postęp publikowania w **aktywności publikowania w sieci Web** okna.</span><span class="sxs-lookup"><span data-stu-id="d98e4-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="d98e4-113">(Z **widoku** menu, wybierz opcję **inne okna**, a następnie wybierz pozycję **aktywności publikowania w sieci Web**.)</span><span class="sxs-lookup"><span data-stu-id="d98e4-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="d98e4-114">Po zakończeniu pracy programu Visual Studio, wdrażania aplikacji, przeglądarka domyślna automatycznie otwiera adres URL wdrożonej witryny sieci Web, a utworzona aplikacja działa w chmurze.</span><span class="sxs-lookup"><span data-stu-id="d98e4-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="d98e4-115">Adres URL na pasku adresu przeglądarki pokazuje, że lokacja jest ładowany z Internetu.</span><span class="sxs-lookup"><span data-stu-id="d98e4-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="d98e4-116">Wdrożenie nowej witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d98e4-116">Deploying to a New Website</span></span>

<span data-ttu-id="d98e4-117">Jeśli nie zaznaczono **Host w chmurze** podczas tworzenia projektu, możesz teraz skonfigurować nową aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="d98e4-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="d98e4-118">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="d98e4-119">Wybierz **profilu** i kliknij polecenie **witryn sieci Web Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="d98e4-120">Jeśli użytkownik nie jest obecnie zalogowany na platformie Azure, pojawi się monit do logowania.</span><span class="sxs-lookup"><span data-stu-id="d98e4-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="d98e4-121">W **istniejących witryn sieci Web** okna dialogowego, kliknij przycisk **nowy**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="d98e4-122">Wprowadź nazwę lokacji.</span><span class="sxs-lookup"><span data-stu-id="d98e4-122">Enter a site name.</span></span> <span data-ttu-id="d98e4-123">Wybierz subskrypcję platformy Azure i region.</span><span class="sxs-lookup"><span data-stu-id="d98e4-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="d98e4-124">W obszarze **serwera bazy danych**, wybierz pozycję **Utwórz nowy serwer**, lub wybierz istniejący serwer.</span><span class="sxs-lookup"><span data-stu-id="d98e4-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="d98e4-125">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="d98e4-126">Kliknij przycisk **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="d98e4-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="d98e4-127">Następnie kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d98e4-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d98e4-128">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="d98e4-128">Previous</span></span>](part-9.md)
