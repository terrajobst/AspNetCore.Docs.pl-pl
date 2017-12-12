---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: "Przy użyciu DynamicPopulate z formantu użytkownika i kodu JavaScript (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant DynamicPopulate w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wartość wynikową w formancie docelowym t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f78da3898d44cc2cf1db6623ebcaf6a73ebbf3e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="c11dd-103">Przy użyciu DynamicPopulate z formantu użytkownika i kodu JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="c11dd-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="c11dd-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c11dd-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c11dd-105">[Pobierz kod](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c11dd-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="c11dd-106">DynamicPopulate formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołanie usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="c11dd-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="c11dd-107">Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c11dd-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="c11dd-108">Jednak szczególną uwagę brane rozszerzeń znajduje się w formancie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c11dd-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="c11dd-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c11dd-109">Overview</span></span>

<span data-ttu-id="c11dd-110">`DynamicPopulate` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="c11dd-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="c11dd-111">Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c11dd-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="c11dd-112">Jednak szczególną uwagę brane rozszerzeń znajduje się w formancie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c11dd-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="c11dd-113">Kroki</span><span class="sxs-lookup"><span data-stu-id="c11dd-113">Steps</span></span>

<span data-ttu-id="c11dd-114">Przede wszystkim należy usługi sieci Web ASP.NET, która implementuje metodę do wywołania przez `DynamicPopulateExtender` formantu.</span><span class="sxs-lookup"><span data-stu-id="c11dd-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="c11dd-115">Usługa sieci web implementuje metody `getDate()` spodziewa się jeden argument typu String, nazywany `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jednej kontekstu informacji z każdego wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="c11dd-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="c11dd-116">Oto kod (plik `DynamicPopulate.cs.asmx`) która pobiera bieżącą datę w jednym z trzech formatów:</span><span class="sxs-lookup"><span data-stu-id="c11dd-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="c11dd-117">W następnym kroku należy utworzyć nowy formant użytkownika (`.ascx` pliku), są oznaczane przez następujące oświadczenie w jego pierwszego wiersza:</span><span class="sxs-lookup"><span data-stu-id="c11dd-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="c11dd-118">A &lt; `label` &gt; element będzie używany do wyświetlania danych z serwera.</span><span class="sxs-lookup"><span data-stu-id="c11dd-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="c11dd-119">Również w pliku sterowania użytkownika, użyjemy trzy przyciski radiowe, każdą z nich reprezentujące jedną z trzech formatów daty możliwe obsługiwane przez usługę sieci web.</span><span class="sxs-lookup"><span data-stu-id="c11dd-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="c11dd-120">Gdy użytkownik kliknie jeden z przycisków radiowych, przeglądarka będzie wykonywał kodu JavaScript, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c11dd-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="c11dd-121">Ten kod uzyskuje dostęp do `DynamicPopulateExtender` (nie martw się o identyfikatorze dziwne jeszcze, to będzie uwzględnione w przyszłości) i wyzwala dynamiczne populacji z danymi.</span><span class="sxs-lookup"><span data-stu-id="c11dd-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="c11dd-122">W kontekście bieżącego przycisku radiowego `this.value` odwołuje się do jego wartość, która jest `format1`, `format2` lub `format3` dokładnie co metoda sieci web oczekuje.</span><span class="sxs-lookup"><span data-stu-id="c11dd-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="c11dd-123">Jedyną operacją, brak jeszcze w formancie użytkownika jest `DynamicPopulateExtender` kontroli, która łączy przyciski radiowe z usługą sieci web.</span><span class="sxs-lookup"><span data-stu-id="c11dd-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="c11dd-124">Ponownie należy zaznaczyć dziwne identyfikator używany w formancie: `mcd1$myDate` zamiast `myDate`.</span><span class="sxs-lookup"><span data-stu-id="c11dd-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="c11dd-125">Wcześniej, kod JavaScript używane `mcd1_dpe1` dostępu `DynamicPopulateExtender` zamiast `dpe1`. Ta strategia nazewnictwa jest szczególne wymagania w przypadku używania `DynamicPopulateExtender` w formancie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c11dd-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="c11dd-126">Ponadto należy osadzić kontroli użytkownika w określony sposób, aby było to działa.</span><span class="sxs-lookup"><span data-stu-id="c11dd-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="c11dd-127">Tworzenie nowej strony ASP.NET i zarejestrować prefiksu tagu kontrolki użytkownika, które zostały zaimplementowane:</span><span class="sxs-lookup"><span data-stu-id="c11dd-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="c11dd-128">Następnie wprowadzić ASP.NET AJAX `ScriptManager` kontrolki w nowej strony:</span><span class="sxs-lookup"><span data-stu-id="c11dd-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="c11dd-129">Na koniec Dodaj kontrolkę użytkownika do strony.</span><span class="sxs-lookup"><span data-stu-id="c11dd-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="c11dd-130">Należy ustawić jego `ID` atrybutu (i `runat="server"`, oczywiście), ale również ma ustawioną nazwę: `mcd1` ponieważ jest to prefiks używany w formancie użytkownika dostępu do niej przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c11dd-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="c11dd-131">I to już wszystko!</span><span class="sxs-lookup"><span data-stu-id="c11dd-131">And that's it!</span></span> <span data-ttu-id="c11dd-132">Strona działa zgodnie z oczekiwaniami: użytkownik kliknie na przyciski radiowe, formantu w zestawie narzędzi programu wywołania usługi sieci web i wyświetla bieżącą datę w wybranym formacie.</span><span class="sxs-lookup"><span data-stu-id="c11dd-132">The page behaves as expected: A user clicks on on of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="c11dd-133">[![Przyciski radiowe znajdują się w formancie użytkownika](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c11dd-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="c11dd-134">Przyciski radiowe znajdują się w formancie użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c11dd-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c11dd-135">[Poprzednie](dynamically-populating-a-control-using-javascript-code-cs.md)
[dalej](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c11dd-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
