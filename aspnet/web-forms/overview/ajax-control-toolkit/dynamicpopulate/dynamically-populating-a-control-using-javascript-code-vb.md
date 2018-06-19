---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamicznie danymi formantu przy użyciu kodu JavaScript (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant DynamicPopulate w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wartość wynikową w formancie docelowym t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869988"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="5cf0d-103">Dynamicznie danymi formantu przy użyciu kodu JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="5cf0d-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="5cf0d-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5cf0d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5cf0d-105">[Pobierz kod](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5cf0d-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="5cf0d-106">DynamicPopulate formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołanie usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="5cf0d-107">Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="5cf0d-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5cf0d-108">Overview</span></span>

<span data-ttu-id="5cf0d-109">`DynamicPopulate` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="5cf0d-110">Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="5cf0d-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="5cf0d-111">Steps</span></span>

<span data-ttu-id="5cf0d-112">Przede wszystkim należy usługi sieci Web ASP.NET, która implementuje metodę do wywołania przez `DynamicPopulateExtender` formantu.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="5cf0d-113">Usługa sieci web implementuje metody `getDate()` spodziewa się jeden argument typu String, nazywany `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jednej kontekstu informacji z każdego wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="5cf0d-114">Oto kod (plik `DynamicPopulate.vb.asmx`) która pobiera bieżącą datę w jednym z trzech formatów:</span><span class="sxs-lookup"><span data-stu-id="5cf0d-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="5cf0d-115">W następnym kroku utwórz nową witrynę programu ASP.NET i uruchomić za pomocą formantu ASP.NET AJAX ScriptManager:</span><span class="sxs-lookup"><span data-stu-id="5cf0d-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="5cf0d-116">Następnie należy dodać formantu etykiety (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub `<asp:Label />` formant sieci web) wykazujących później wynik wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="5cf0d-117">Następnie należy uwzględnić `DynamicPopulateExtender` kontroli i podaj informacje o usługi sieci web, formantu docelowego, ale nie nazwę formantu, który wyzwala populacji będzie to później za pomocą niestandardowych skryptów JavaScript!</span><span class="sxs-lookup"><span data-stu-id="5cf0d-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="5cf0d-118">Teraz, aby części JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-118">Now to the JavaScript part.</span></span> <span data-ttu-id="5cf0d-119">`$find()` Funkcja zdefiniowana przez biblioteki ASP.NET AJAX, zwraca odwołanie do obiektów po stronie serwera zestawu ASP.NET AJAX kontroli narzędzi, takich jak `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="5cf0d-120">W bieżącym pliku `$find("dpe")` zwraca odwołanie do tego `DynamicPopulateExtender` kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="5cf0d-121">Udostępnia ona metodę o nazwie `populate()` która wyzwala procesu dynamicznego populacji.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="5cf0d-122">`populate()` Metoda wymaga jednego argumentu: klucz kontekstu, która będzie służyć jako argument `getDate()` metody w sieci web.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="5cf0d-123">Tak na przykład `$find("dpe").populate("format1")` czy wypełnić etykiety z bieżącą datę w formacie dzień miesiąc rok.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="5cf0d-124">Aby ustawić próbki bardziej elastyczne, użytkownik może teraz wybierać między kilka formatów daty.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="5cf0d-125">Dla każdego z nich jest wyświetlany przycisk radiowy.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="5cf0d-126">Po kliknięciu przycisku radiowego, kod JavaScript dynamicznie wypełnia etykiety z wybranego formatu daty.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="5cf0d-127">Poniżej przedstawiono tych przycisków radiowych:</span><span class="sxs-lookup"><span data-stu-id="5cf0d-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="5cf0d-128">Należy pamiętać, że w kontekście przycisku radiowego, wyrażenie JavaScript `this.value` odnosi się do wartości bieżącego przycisku do tych samych informacji `getDate()` metody może współpracować z.</span><span class="sxs-lookup"><span data-stu-id="5cf0d-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="5cf0d-129">[![Kliknięcie przycisku pobiera daty z serwera, w formacie określonym](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5cf0d-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="5cf0d-130">Kliknięcie przycisku pobiera daty z serwera, w formacie określonym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5cf0d-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5cf0d-131">[Poprzednie](dynamically-populating-a-control-vb.md)
> [dalej](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5cf0d-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
