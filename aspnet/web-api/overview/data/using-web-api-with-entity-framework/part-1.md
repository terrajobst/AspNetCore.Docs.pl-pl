---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: "Używanie składnika Web API 2 z programu Entity Framework 6 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym samouczku zostanie nauczyć się, że tworzenie aplikacji sieci web ze składnika ASP.NET Web API zaplecza. W samouczku Entity Framework 6 dla układ dane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: cceefa128f90b4c3e23dd31119f44e6ffc55f46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="4c16f-104">Używanie składnika Web API 2 z programu Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4c16f-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="4c16f-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4c16f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4c16f-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="4c16f-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="4c16f-107">W tym samouczku zostanie nauczyć się, że tworzenie aplikacji sieci web ze składnika ASP.NET Web API zaplecza.</span><span class="sxs-lookup"><span data-stu-id="4c16f-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="4c16f-108">W samouczku Entity Framework 6 dla warstwy danych i Knockout.js dla aplikacji JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4c16f-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="4c16f-109">Samouczek również pokazuje, jak wdrożyć aplikację do aplikacji sieci Web usługi aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="4c16f-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4c16f-110">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="4c16f-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4c16f-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="4c16f-111">Web API 2.1</span></span>
> - [<span data-ttu-id="4c16f-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="4c16f-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="4c16f-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4c16f-113">Entity Framework 6</span></span>
> - <span data-ttu-id="4c16f-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4c16f-114">.NET 4.5</span></span>
> - <span data-ttu-id="4c16f-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="4c16f-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="4c16f-116">Ten samouczek używa ASP.NET Web API 2 z programu Entity Framework 6 do utworzenia aplikacji sieci web, która obsługuje wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4c16f-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="4c16f-117">Poniżej przedstawiono zrzut ekranu aplikacji, która zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="4c16f-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="4c16f-118">Projekt jednostronicowej aplikacji JEDNOSTRONICOWEJ korzysta z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c16f-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="4c16f-119">"Aplikacji jednostronicowej" to ogólny termin dla aplikacji sieci web, który ładuje pojedynczej strony HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowania nowych stron.</span><span class="sxs-lookup"><span data-stu-id="4c16f-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="4c16f-120">Po załadowaniu strony początkowej aplikacji komunikuje się z serwerem za pośrednictwem żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="4c16f-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="4c16f-121">AJAX żądań zwracanych danych JSON, których aplikacje używają do aktualizacji w Interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4c16f-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="4c16f-122">AJAX nie jest nowy, ale obecnie jest struktury języka JavaScript, ułatwiające tworzenie i obsługa dużej aplikacji JEDNOSTRONICOWEJ zaawansowane.</span><span class="sxs-lookup"><span data-stu-id="4c16f-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="4c16f-123">W tym samouczku używana [Knockout.js](http://knockoutjs.com/), ale możesz użyć dowolnej architektury JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="4c16f-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="4c16f-124">Poniżej przedstawiono główne bloków konstrukcyjnych dla tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4c16f-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="4c16f-125">ASP.NET MVC tworzy stronę HTML.</span><span class="sxs-lookup"><span data-stu-id="4c16f-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="4c16f-126">Interfejs API sieci Web platformy ASP.NET obsługuje żądania AJAX i zwraca dane JSON.</span><span class="sxs-lookup"><span data-stu-id="4c16f-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="4c16f-127">Knockout.js danych wiązania elementów HTML w danych JSON.</span><span class="sxs-lookup"><span data-stu-id="4c16f-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="4c16f-128">Entity Framework komunikuje się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4c16f-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="4c16f-129">Zobacz tej aplikacji działających na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="4c16f-129">See this App Running on Azure</span></span>

<span data-ttu-id="4c16f-130">Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web?</span><span class="sxs-lookup"><span data-stu-id="4c16f-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="4c16f-131">Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="4c16f-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="4c16f-132">Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4c16f-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="4c16f-133">Jeśli nie masz już konto, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="4c16f-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="4c16f-134">[Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4c16f-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="4c16f-135">[Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.</span><span class="sxs-lookup"><span data-stu-id="4c16f-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="4c16f-136">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="4c16f-136">Create the Project</span></span>

<span data-ttu-id="4c16f-137">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c16f-137">Open Visual Studio.</span></span> <span data-ttu-id="4c16f-138">Z **pliku** menu, wybierz opcję **nowy**, a następnie wybierz pozycję **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4c16f-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="4c16f-139">(Lub kliknij przycisk **nowy projekt** na stronie początkowej.)</span><span class="sxs-lookup"><span data-stu-id="4c16f-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="4c16f-140">W **nowy projekt** okna dialogowego, kliknij przycisk **Web** w okienku po lewej stronie i **aplikacji sieci Web ASP.NET** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="4c16f-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="4c16f-141">Nazwij projekt BookService, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c16f-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="4c16f-142">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="4c16f-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="4c16f-143">Jeśli chcesz udostępnić projekt w usłudze Azure App Service, pozostaw **Hostuj w chmurze** zaznaczonym polem.</span><span class="sxs-lookup"><span data-stu-id="4c16f-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="4c16f-144">Kliknij przycisk **OK** Aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="4c16f-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="4c16f-145">Konfigurowanie ustawień platformy Azure (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="4c16f-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="4c16f-146">Jeśli **Host w chmurze** zaznaczeniu opcji programu Visual Studio spowoduje wyświetlenie monitu do logowania do systemu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4c16f-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="4c16f-147">Po zalogowaniu się do platformy Azure, programu Visual Studio monituje do skonfigurowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4c16f-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="4c16f-148">Wprowadź nazwę lokacji, wybierz subskrypcję platformy Azure, a następnie wybierz region geograficzny.</span><span class="sxs-lookup"><span data-stu-id="4c16f-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="4c16f-149">W obszarze **serwera bazy danych**, wybierz pozycję **Utwórz nowy serwer**.</span><span class="sxs-lookup"><span data-stu-id="4c16f-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="4c16f-150">Wprowadź nazwę użytkownika administratora i hasło.</span><span class="sxs-lookup"><span data-stu-id="4c16f-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[<span data-ttu-id="4c16f-151">Next</span><span class="sxs-lookup"><span data-stu-id="4c16f-151">Next</span></span>](part-2.md)
