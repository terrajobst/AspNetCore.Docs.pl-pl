---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamicznie danymi formantu (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant DynamicPopulate w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wartość wynikową w formancie docelowym t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="16469-103">Dynamicznie danymi formantu (VB)</span><span class="sxs-lookup"><span data-stu-id="16469-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="16469-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="16469-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="16469-105">[Pobierz kod](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="16469-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="16469-106">DynamicPopulate formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołanie usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="16469-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="16469-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="16469-107">Overview</span></span>

<span data-ttu-id="16469-108">`DynamicPopulate` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="16469-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="16469-109">W tym samouczku przedstawiono sposób tej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="16469-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="16469-110">Kroki</span><span class="sxs-lookup"><span data-stu-id="16469-110">Steps</span></span>

<span data-ttu-id="16469-111">Przede wszystkim należy usługi sieci Web ASP.NET, która implementuje metodę do wywołania przez `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="16469-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="16469-112">Klasa usługi sieci web wymaga `ScriptService` atrybut, który jest zdefiniowany w ramach `Microsoft.Web.Script.Services`; w przeciwnym razie ASP.NET AJAX nie można utworzyć serwera proxy JavaScript po stronie klienta dla usługi sieci web, która z kolei jest wymagana przez `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="16469-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="16469-113">Metoda sieci web należy spodziewać się jeden argument typu String, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jednej kontekstu informacji z każdego wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="16469-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="16469-114">Następująca usługa sieci web zwraca bieżącą datę w formacie reprezentowany przez `contextKey` argumentu:</span><span class="sxs-lookup"><span data-stu-id="16469-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="16469-115">Usługi sieci web jest następnie zapisywana jako `DynamicPopulate.vb.asmx`.</span><span class="sxs-lookup"><span data-stu-id="16469-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="16469-116">Alternatywnie można zaimplementować `getDate()` metody jako metodę strony w rzeczywistych strony ASP.NET z `DynamicPopulate` formantu.</span><span class="sxs-lookup"><span data-stu-id="16469-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="16469-117">W następnym kroku utwórz nowy plik programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="16469-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="16469-118">Zawsze, pierwszym krokiem jest uwzględnienie `ScriptManager` na bieżącej stronie można załadować biblioteki ASP.NET AJAX i dla zapewnienia działania kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="16469-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="16469-119">Następnie należy dodać formantu etykiety (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub &lt; `asp:Label`  / &gt; formant sieci web) wykazujących później wynik wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="16469-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="16469-120">Przycisk HTML (jako kontrolkę HTML, ponieważ nie wymaga ogłaszania zwrotnego na serwerze) zostanie następnie użyte do wyzwolenia dynamiczne wypełniania:</span><span class="sxs-lookup"><span data-stu-id="16469-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="16469-121">Na koniec należy `DynamicPopulateExtender` formantu do elementów danych przesyłanych w sieci w górę.</span><span class="sxs-lookup"><span data-stu-id="16469-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="16469-122">Następujące atrybuty zostanie skonfigurowany (oprócz oczywiste te `ID` i `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="16469-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="16469-123">`TargetControlID` gdzie umieścić wyniki z wywołaniem usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="16469-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="16469-124">`ServicePath` Ścieżka do usługi sieci web (pominąć, jeśli chcesz użyć metody strony)</span><span class="sxs-lookup"><span data-stu-id="16469-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="16469-125">`ServiceMethod` Nazwa metody sieci web lub strona — Metoda</span><span class="sxs-lookup"><span data-stu-id="16469-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="16469-126">`ContextKey` informacje o kontekście do wysłania do usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="16469-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="16469-127">`PopulateTriggerControlID` element, który wyzwala wywołania usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="16469-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="16469-128">`ClearContentsDuringUpdate` Czy pusty element docelowy podczas wywołania usługi sieci web</span><span class="sxs-lookup"><span data-stu-id="16469-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="16469-129">Jak widać, formantu wymaga pewne informacje, ale przygotowania wszystko, co jest dość proste.</span><span class="sxs-lookup"><span data-stu-id="16469-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="16469-130">Oto kod znaczników dla `DynamicPopulateExtender` formantu w tym scenariuszu:</span><span class="sxs-lookup"><span data-stu-id="16469-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="16469-131">Uruchom strony ASP.NET w przeglądarce, a następnie kliknij przycisk; otrzymasz bieżącą datę w formacie dzień miesiąc rok.</span><span class="sxs-lookup"><span data-stu-id="16469-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="16469-132">[![Kliknięcie przycisku pobiera daty z serwera](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="16469-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="16469-133">Kliknięcie przycisku pobiera daty z serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="16469-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16469-134">[Poprzednie](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [dalej](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="16469-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
