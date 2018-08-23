---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikowanie aplikacji w usłudze Azure usługa Azure App Service | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 5c1a70ceded85681046065881f62c5c95c5d8740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752265"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="af1ba-102">Publikowanie aplikacji w usłudze Azure App Service platformy Azure</span><span class="sxs-lookup"><span data-stu-id="af1ba-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="af1ba-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="af1ba-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="af1ba-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="af1ba-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="af1ba-105">W ostatnim kroku opublikujesz aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="af1ba-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="af1ba-106">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="af1ba-107">Klikając **Publikuj** wywołuje **publikowanie w sieci Web** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="af1ba-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="af1ba-108">Jeśli zaznaczono **Host w chmurze** podczas pierwszego utworzenia projektu, a następnie połączenie i ustawienia zostały już skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="af1ba-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="af1ba-109">W takim przypadku należy po prostu kliknij **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="af1ba-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="af1ba-110">(Jeśli nie została sprawdzona **Host w chmurze** na początku, a następnie wykonaj kroki opisane w [następnej sekcji](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="af1ba-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="af1ba-111">Aby wdrożyć aplikację, kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="af1ba-112">Możesz wyświetlić postęp publikowania **działania publikowania internetowego** okna.</span><span class="sxs-lookup"><span data-stu-id="af1ba-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="af1ba-113">(Z **widoku** menu, wybierz opcję **Windows inne**, a następnie wybierz **działania publikowania internetowego**.)</span><span class="sxs-lookup"><span data-stu-id="af1ba-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="af1ba-114">Po zakończeniu wdrażania aplikacji programu Visual Studio przeglądarka domyślna automatycznie otwiera adres URL wdrożonych witryn sieci Web i aplikacji, który został utworzony, jest teraz uruchomiona w chmurze.</span><span class="sxs-lookup"><span data-stu-id="af1ba-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="af1ba-115">Adres URL w pasku adresu przeglądarki pokazuje, że witryna jest ładowany z Internetu.</span><span class="sxs-lookup"><span data-stu-id="af1ba-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="af1ba-116">Wdrożenie nowej witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="af1ba-116">Deploying to a New Website</span></span>

<span data-ttu-id="af1ba-117">Jeśli nie zaznaczono **Host w chmurze** podczas tworzenia projektu, możesz teraz skonfigurować nową aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="af1ba-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="af1ba-118">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="af1ba-119">Wybierz **profilu** kartę, a następnie kliknij przycisk **Microsoft Azure Websites**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="af1ba-120">Nie są obecnie zalogowano do platformy Azure, zostanie wyświetlony monit Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="af1ba-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="af1ba-121">W **istniejących witryn sieci Web** okno dialogowe, kliknij przycisk **New**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="af1ba-122">Wprowadź nazwę lokacji.</span><span class="sxs-lookup"><span data-stu-id="af1ba-122">Enter a site name.</span></span> <span data-ttu-id="af1ba-123">Wybierz subskrypcję platformy Azure i region.</span><span class="sxs-lookup"><span data-stu-id="af1ba-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="af1ba-124">W obszarze **serwera bazy danych**, wybierz opcję **Utwórz nowy serwer**, lub wybierz istniejący serwer.</span><span class="sxs-lookup"><span data-stu-id="af1ba-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="af1ba-125">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="af1ba-126">Kliknij przycisk **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="af1ba-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="af1ba-127">Następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="af1ba-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="af1ba-128">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="af1ba-128">Previous</span></span>](part-9.md)
