---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Uaktualnianie projektów SignalR 1.x do wersji 2 | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano sposób uaktualniania istniejący projekt SignalR 1.x do SignalR 2.x oraz sposób rozwiązywania problemów, które mogą wystąpić podczas procesu uaktualniania...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: 393beb1ef696bd2dfae25789f79a67157780a219
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824166"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="32f3e-103">Uaktualnianie projektów SignalR 1.x do wersji 2</span><span class="sxs-lookup"><span data-stu-id="32f3e-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="32f3e-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="32f3e-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="32f3e-105">W tym temacie opisano sposób uaktualniania istniejący projekt SignalR 1.x do SignalR 2.x oraz sposób rozwiązywania problemów, które mogą wystąpić podczas procesu uaktualniania.</span><span class="sxs-lookup"><span data-stu-id="32f3e-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="32f3e-106">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="32f3e-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="32f3e-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="32f3e-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="32f3e-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="32f3e-108">.NET 4.5</span></span>
> - <span data-ttu-id="32f3e-109">SignalR w wersji 1 i 2</span><span class="sxs-lookup"><span data-stu-id="32f3e-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="32f3e-110">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="32f3e-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="32f3e-111">Aby użyć programu Visual Studio 2012 za pomocą tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="32f3e-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="32f3e-112">Aktualizacja usługi [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="32f3e-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="32f3e-113">Zainstaluj [Instalator platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="32f3e-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="32f3e-114">Instalator platformy sieci Web, wyszukiwanie i instalowanie **platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="32f3e-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="32f3e-115">Szablony programu Visual Studio dla klas SignalR spowoduje to zainstalowanie takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="32f3e-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="32f3e-116">Niektóre szablony (takie jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku użyj pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="32f3e-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="32f3e-117">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="32f3e-117">Questions and comments</span></span>
> 
> <span data-ttu-id="32f3e-118">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="32f3e-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="32f3e-119">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="32f3e-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="32f3e-120">SignalR 2 oferuje w jednolitym środowisku projektowym na platformach serwera przy użyciu [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="32f3e-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="32f3e-121">W tym artykule opisano kilka kroków, które są potrzebne, aby zaktualizować aplikację SignalR 1.x do wersji 2.</span><span class="sxs-lookup"><span data-stu-id="32f3e-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="32f3e-122">Chociaż zaleca się uaktualnienie aplikacji SignalR 2, SignalR 1.x będą nadal obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="32f3e-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="32f3e-123">W tym samouczku opisano sposób uaktualniania aplikacji hostowanej w sieci web z SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="32f3e-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="32f3e-124">Własne aplikacje, (te, które będzie hostować serwer w aplikacji konsoli, usługa Windows lub inny proces) są teraz obsługiwane zgodnie z SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="32f3e-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="32f3e-125">Aby uzyskać informacje na temat sposobu Rozpocznij tworzenie aplikacji samodzielnie hostowany przy użyciu SignalR 2, zobacz [samouczek: Host samodzielny SignalR](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="32f3e-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="32f3e-126">Spis treści</span><span class="sxs-lookup"><span data-stu-id="32f3e-126">Contents</span></span>

<span data-ttu-id="32f3e-127">W poniższych sekcjach opisano zadania związane z uaktualnianiem projektów SignalR i jak rozwiązywać problemy, które mogą wystąpić.</span><span class="sxs-lookup"><span data-stu-id="32f3e-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="32f3e-128">Przykład: Uaktualnienie samouczka Wprowadzenie do SignalR 2</span><span class="sxs-lookup"><span data-stu-id="32f3e-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="32f3e-129">Rozwiązywanie problemów z błędami podczas uaktualniania</span><span class="sxs-lookup"><span data-stu-id="32f3e-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="32f3e-130">Przykład: Uaktualnianie aplikacji samouczka Wprowadzenie do SignalR 2</span><span class="sxs-lookup"><span data-stu-id="32f3e-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="32f3e-131">W tej sekcji zostaną zaktualizowane aplikacja utworzona w [wersji biblioteki SignalR 1.x Samouczek wprowadzający](../older-versions/index.md) do korzystania z SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="32f3e-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="32f3e-132">Po zakończeniu samouczka Wprowadzenie, kliknij prawym przyciskiem myszy nad projektem i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="32f3e-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="32f3e-133">Upewnij się, że **platformę docelową** ustawiono **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="32f3e-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="32f3e-134">Otwórz konsolę Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="32f3e-134">Open the Package Manager Console.</span></span> <span data-ttu-id="32f3e-135">Usuń SignalR 1.x z projektu, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="32f3e-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="32f3e-136">Zainstaluj signalr2 na użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="32f3e-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="32f3e-137">Na stronie HTML zaktualizuj odwołanie do skryptu dla elementu SignalR w celu dopasowania do wersji skryptu teraz zawarty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="32f3e-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="32f3e-138">W klasie globalnej aplikacji Usuń wywołanie funkcji MapHubs.</span><span class="sxs-lookup"><span data-stu-id="32f3e-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="32f3e-139">Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj**, **nowy element...** . W oknie dialogowym wybierz **klasy początkowej Owin**.</span><span class="sxs-lookup"><span data-stu-id="32f3e-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="32f3e-140">Nadaj nowej klasie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="32f3e-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="32f3e-141">Zastąp zawartość pliku Startup.cs następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="32f3e-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="32f3e-142">Atrybutu zestawu Dodaje klasę do procesu uruchamiania firmy Owin, które wykonuje `Configuration` metoda podczas uruchamiania Owin.</span><span class="sxs-lookup"><span data-stu-id="32f3e-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="32f3e-143">To z kolei wywołuje `MapSignalR` metody, która tworzy trasy do wszystkich centrów SignalR w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32f3e-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="32f3e-144">Uruchom projekt, a następnie skopiuj adres URL strony głównej do innej przeglądarki lub okienku przeglądarki, jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="32f3e-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="32f3e-145">Każda strona będzie monitować o nazwę użytkownika i komunikatów wysyłanych z każdej strony powinny być widoczne w okienkami przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="32f3e-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="32f3e-146">Rozwiązywanie problemów z błędami podczas uaktualniania</span><span class="sxs-lookup"><span data-stu-id="32f3e-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="32f3e-147">W tej sekcji opisano problemy, które mogą wystąpić podczas uaktualniania.</span><span class="sxs-lookup"><span data-stu-id="32f3e-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="32f3e-148">Aby bardziej kompleksowe listę błędów i problemów, które mogą wystąpić w przypadku aplikacji SignalR, zobacz [Rozwiązywanie problemów z SignalR](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="32f3e-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="32f3e-149">"Wywołanie jest niejednoznaczne między następujące metody lub właściwości"</span><span class="sxs-lookup"><span data-stu-id="32f3e-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="32f3e-150">Ten błąd wystąpi, jeśli odwołanie do `Microsoft.AspNet.SignalR.Owin` nie został usunięty.</span><span class="sxs-lookup"><span data-stu-id="32f3e-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="32f3e-151">Ten pakiet jest przestarzały; Odwołanie muszą zostać usunięte i wersji 1.x host własny pakiet musi zostać odinstalowane.</span><span class="sxs-lookup"><span data-stu-id="32f3e-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="32f3e-152">Metod koncentratora zakończyć się niepowodzeniem dyskretnie</span><span class="sxs-lookup"><span data-stu-id="32f3e-152">Hub methods fail silently</span></span>

<span data-ttu-id="32f3e-153">Sprawdź, czy odwołania do skryptu w swoim kliencie aktualny i że `OwinStartup` atrybutu dla klasy uruchamiania ma poprawne klasy i nazwy zestawu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="32f3e-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="32f3e-154">Ponadto spróbuj otworzyć adres koncentratorów (/ signalr/koncentratory) w przeglądarce; wszelkie błędy, który pojawia się zaoferuje dowiedzieć się więcej o tym, co będzie problem.</span><span class="sxs-lookup"><span data-stu-id="32f3e-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
