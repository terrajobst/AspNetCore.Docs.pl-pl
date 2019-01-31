---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Używanie składnika Web API 2 z platformą Entity Framework 6 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku nauczy Cię, że zaplecze podstawy tworzenia aplikacji sieci web za pomocą interfejsu API sieci Web platformy ASP.NET. W tym samouczku użyto programu Entity Framework 6 dla układ danych...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236526"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="b5280-104">Używanie wzorca Web API 2 z programem Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b5280-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="b5280-105">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="b5280-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="b5280-106">W tym samouczku dowiesz się, że zaplecze podstawy tworzenia aplikacji sieci web za pomocą interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b5280-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="b5280-107">W samouczku Entity Framework 6 dla warstwy danych i użyciem Knockout.js dla aplikacji JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b5280-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="b5280-108">Samouczek przedstawia również sposób wdrażania aplikacji w usłudze Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="b5280-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5280-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="b5280-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="b5280-110">Składnik Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="b5280-110">Web API 2.1</span></span>
> - <span data-ttu-id="b5280-111">Program Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="b5280-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="b5280-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b5280-112">Entity Framework 6</span></span>
> - <span data-ttu-id="b5280-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="b5280-113">.NET 4.7</span></span>
> - <span data-ttu-id="b5280-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="b5280-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="b5280-115">W tym samouczku za pomocą wzorca ASP.NET Web API 2 platformy Entity Framework 6 do tworzenia aplikacji sieci web, która manipuluje wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b5280-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="b5280-116">Poniżej przedstawiono zrzut ekranu aplikacji, która zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="b5280-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="b5280-117">Aplikacja używa projektowania aplikacji jednostronicowej (SPA).</span><span class="sxs-lookup"><span data-stu-id="b5280-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="b5280-118">"Aplikacja jednostronicowa" jest ogólnym terminem dla aplikacji sieci web, która ładuje z pojedynczą stroną HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowanie nowych stron.</span><span class="sxs-lookup"><span data-stu-id="b5280-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="b5280-119">Po załadowaniu strony początkowej aplikacji komunikuje się z serwerem za pośrednictwem żądań AJAX.</span><span class="sxs-lookup"><span data-stu-id="b5280-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="b5280-120">AJAX żądań zwracany danych JSON, których aplikacje używają do aktualizacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b5280-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="b5280-121">AJAX nie jest nowy, ale obecnie ma platformy JavaScript, które ułatwiają tworzenie i zarządzanie nimi dużej zaawansowanych aplikacji SPA.</span><span class="sxs-lookup"><span data-stu-id="b5280-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="b5280-122">W tym samouczku [struktura Knockout.js](http://knockoutjs.com/), ale można użyć dowolnej architektury klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b5280-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="b5280-123">Poniżej przedstawiono główne bloki konstrukcyjne dla tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b5280-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="b5280-124">ASP.NET MVC tworzy stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="b5280-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="b5280-125">ASP.NET Web API obsługuje żądania AJAX i zwraca dane JSON.</span><span class="sxs-lookup"><span data-stu-id="b5280-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="b5280-126">Struktura Knockout.js danych — tworzy powiązanie elementów HTML dane JSON.</span><span class="sxs-lookup"><span data-stu-id="b5280-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="b5280-127">Entity Framework komunikuje się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="b5280-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="b5280-128">Zobacz tej aplikacji działających na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="b5280-128">See this app running on Azure</span></span>

<span data-ttu-id="b5280-129">Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo?</span><span class="sxs-lookup"><span data-stu-id="b5280-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="b5280-130">Pełną wersję aplikacji można wdrożyć do konta platformy Azure, wybierając poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="b5280-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="b5280-131">Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b5280-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="b5280-132">Jeśli nie masz jeszcze konta, możesz:</span><span class="sxs-lookup"><span data-stu-id="b5280-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="b5280-133">[Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymujesz środki , które możesz użyć do wypróbowania płatnych usług platformy Azure, a potem nawet w przypadku ich wyczerpania możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b5280-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="b5280-134">[Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - w ramach subskrypcji MSDN każdego miesiąca dostajesz środki, dzięki którym możesz korzystać z płatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b5280-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="b5280-135">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="b5280-135">Create the project</span></span>

<span data-ttu-id="b5280-136">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5280-136">Open Visual Studio.</span></span> <span data-ttu-id="b5280-137">Z **pliku** menu, wybierz opcję **New**, a następnie wybierz **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b5280-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="b5280-138">(Lub wybierz **nowy projekt** na stronie początkowej.)</span><span class="sxs-lookup"><span data-stu-id="b5280-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="b5280-139">W **nowy projekt** okno dialogowe, wybierz opcję **Web** w okienku po lewej stronie i **aplikacji sieci Web platformy ASP.NET (.NET Framework)** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="b5280-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="b5280-140">Nadaj projektowi nazwę **BookService** i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5280-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="b5280-141">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="b5280-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="b5280-142">Wybierz **OK** do tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="b5280-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="b5280-143">Konfigurowanie ustawień platformy Azure (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="b5280-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="b5280-144">Po utworzeniu projektu, można wdrożyć do usługi Azure App Service Web Apps w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="b5280-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="b5280-145">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b5280-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="b5280-146">W wyświetlonym oknie Wybierz **Start**.</span><span class="sxs-lookup"><span data-stu-id="b5280-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="b5280-147">**Wybierz lokalizację docelową publikowania** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="b5280-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="b5280-148">Wybierz **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="b5280-148">Select **Create Profile**.</span></span> <span data-ttu-id="b5280-149">**Tworzenie usługi App Service** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="b5280-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="b5280-150">Zaakceptuj wartości domyślne lub wprowadzić inne wartości dla nazwy aplikacji w grupie zasobów, hosting plan subskrypcji platformy Azure i regionu geograficznego.</span><span class="sxs-lookup"><span data-stu-id="b5280-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="b5280-151">Wybierz **utworzyć bazę danych SQL**.</span><span class="sxs-lookup"><span data-stu-id="b5280-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="b5280-152">**Konfiguruj serwer SQL** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="b5280-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="b5280-153">Zaakceptuj wartości domyślne lub wprowadzić inne wartości.</span><span class="sxs-lookup"><span data-stu-id="b5280-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="b5280-154">Wprowadź **nazwa użytkownika administratora** i **hasło administratora** nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b5280-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="b5280-155">Wybierz **OK** po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="b5280-155">Select **OK** when you're done.</span></span> <span data-ttu-id="b5280-156">**Tworzenie usługi App Service** pojawi się strona.</span><span class="sxs-lookup"><span data-stu-id="b5280-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="b5280-157">Wybierz **Utwórz** w celu utworzenia profilu.</span><span class="sxs-lookup"><span data-stu-id="b5280-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="b5280-158">Pojawi się w prawym dolnym rogu, co oznacza, że wdrożenie w toku.</span><span class="sxs-lookup"><span data-stu-id="b5280-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="b5280-159">Po krótkiej chwili **Publikuj** okna pojawi się ponownie.</span><span class="sxs-lookup"><span data-stu-id="b5280-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="b5280-160">Profil został utworzony w celu wdrożenia aplikacji jest teraz dostępna.</span><span class="sxs-lookup"><span data-stu-id="b5280-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="b5280-161">Next</span><span class="sxs-lookup"><span data-stu-id="b5280-161">Next</span></span>](part-2.md)
