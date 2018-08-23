---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Używanie kontrolki DynamicPopulate z kontrolką użytkownika i kodu JavaScript (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wypełnia wynikowej wartości do formantu docelowego t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 110f6dd05d038438bc061d3ee907a5e2da8968c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754761"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="4faa2-103">Używanie kontrolki DynamicPopulate z kontrolką użytkownika i kodu JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="4faa2-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="4faa2-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4faa2-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4faa2-105">[Pobierz program Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4faa2-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="4faa2-106">Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="4faa2-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="4faa2-107">Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4faa2-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="4faa2-108">Jednak szczególną uwagę należy podjąć, gdy urządzenie extender znajduje się w kontrolce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4faa2-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="4faa2-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4faa2-109">Overview</span></span>

<span data-ttu-id="4faa2-110">`DynamicPopulate` Kontrola w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="4faa2-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="4faa2-111">Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4faa2-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="4faa2-112">Jednak szczególną uwagę należy podjąć, gdy urządzenie extender znajduje się w kontrolce użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4faa2-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="4faa2-113">Kroki</span><span class="sxs-lookup"><span data-stu-id="4faa2-113">Steps</span></span>

<span data-ttu-id="4faa2-114">Po pierwsze, potrzebujesz usługi sieci Web ASP.NET, która implementuje metodę można wywoływać za pomocą `DynamicPopulateExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="4faa2-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="4faa2-115">Usługa sieci web implementuje metodę `getDate()` który oczekuje jednego argumentu typu ciąg, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jeden fragment informacje o kontekście przy każdym wywołaniem usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="4faa2-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="4faa2-116">Oto kod (plik `DynamicPopulate.cs.asmx`) które pobiera bieżącą datę w jednym z trzech formatów:</span><span class="sxs-lookup"><span data-stu-id="4faa2-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="4faa2-117">W następnym kroku, Utwórz nową kontrolkę użytkownika (`.ascx` pliku), są oznaczane przez następującą deklarację w jej pierwszy wiersz:</span><span class="sxs-lookup"><span data-stu-id="4faa2-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="4faa2-118">A &lt; `label` &gt; element będzie używany do wyświetlania danych z serwera.</span><span class="sxs-lookup"><span data-stu-id="4faa2-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="4faa2-119">Również w pliku kontrolki użytkownika, firma Microsoft użyje trzy przyciski radiowe, każdy z nich reprezentuje jeden z trzech formatów możliwych Data obsługiwana przez usługę sieci web.</span><span class="sxs-lookup"><span data-stu-id="4faa2-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="4faa2-120">Gdy użytkownik kliknie na jeden z przycisków radiowych, przeglądarka będzie wykonywać kod JavaScript, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="4faa2-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="4faa2-121">Ten kod uzyskuje dostęp do `DynamicPopulateExtender` (nie martw się o identyfikatorze dziwne, ale, zostanie to opisane w dalszej) i wyzwala dynamiczne wypełnianie danymi.</span><span class="sxs-lookup"><span data-stu-id="4faa2-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="4faa2-122">W kontekście bieżącego przycisk radiowy `this.value` odwołuje się do jego wartość, która jest `format1`, `format2` lub `format3` dokładnie metody sieci web oczekiwaniom.</span><span class="sxs-lookup"><span data-stu-id="4faa2-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="4faa2-123">Jedyną czynnością, brakuje w kontrolce użytkownika jeszcze jest `DynamicPopulateExtender` kontrolowanie linki przycisków radiowych do usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="4faa2-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="4faa2-124">Ponownie należy zaznaczyć otrzymano nieoczekiwany identyfikator używany w kontrolce: `mcd1$myDate` zamiast `myDate`.</span><span class="sxs-lookup"><span data-stu-id="4faa2-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="4faa2-125">Wcześniej kod JavaScript używany `mcd1_dpe1` dostęp do `DynamicPopulateExtender` zamiast `dpe1`. Ta strategia nazewnictwa jest specjalnych wymagań, korzystając z `DynamicPopulateExtender` znajdujących się pod kontrolą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4faa2-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="4faa2-126">Ponadto należy osadzić kontroli użytkownika w określony sposób, aby umożliwić jej pracę.</span><span class="sxs-lookup"><span data-stu-id="4faa2-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="4faa2-127">Tworzenie nowej strony programu ASP.NET i zarejestrować prefiksu tagu kontrolki użytkownika, które zostało zaimplementowane:</span><span class="sxs-lookup"><span data-stu-id="4faa2-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="4faa2-128">Następnie wprowadzić ASP.NET AJAX `ScriptManager` kontroli na nowej stronie:</span><span class="sxs-lookup"><span data-stu-id="4faa2-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="4faa2-129">Na koniec Dodaj formant użytkownika do strony.</span><span class="sxs-lookup"><span data-stu-id="4faa2-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="4faa2-130">Musisz ustawić jego `ID` atrybutu (i `runat="server"`, oczywiście), ale należy również ustawić ją na określonej nazwy: `mcd1` to prefiks używany w kontrolce użytkownika dostępu do niego przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4faa2-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="4faa2-131">I to wszystko!</span><span class="sxs-lookup"><span data-stu-id="4faa2-131">And that's it!</span></span> <span data-ttu-id="4faa2-132">Strona zachowuje się zgodnie z oczekiwaniami: użytkownik kliknie na jeden z przycisków radiowych, formant w zestawie narzędzi wywołuje usługę sieci web i wyświetla bieżącą datę w wybranym formacie.</span><span class="sxs-lookup"><span data-stu-id="4faa2-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="4faa2-133">[![Przyciski radiowe znajdują się w kontrolce użytkownika](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4faa2-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="4faa2-134">Przyciski radiowe znajdują się w kontrolce użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4faa2-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4faa2-135">[Poprzednie](dynamically-populating-a-control-using-javascript-code-cs.md)
> [dalej](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4faa2-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
