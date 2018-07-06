---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wypełnia wynikowej wartości do formantu docelowego t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: fae87d87b0783dff6fc8f5f282a73b05ee64affa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812743"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="07ead-103">Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="07ead-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="07ead-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="07ead-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="07ead-105">[Pobierz program Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="07ead-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="07ead-106">Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="07ead-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="07ead-107">Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="07ead-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="07ead-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="07ead-108">Overview</span></span>

<span data-ttu-id="07ead-109">`DynamicPopulate` Kontrola w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="07ead-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="07ead-110">Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="07ead-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="07ead-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="07ead-111">Steps</span></span>

<span data-ttu-id="07ead-112">Po pierwsze, potrzebujesz usługi sieci Web ASP.NET, która implementuje metodę można wywoływać za pomocą `DynamicPopulateExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="07ead-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="07ead-113">Usługa sieci web implementuje metodę `getDate()` który oczekuje jednego argumentu typu ciąg, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jeden fragment informacje o kontekście przy każdym wywołaniem usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="07ead-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="07ead-114">Oto kod (plik `DynamicPopulate.vb.asmx`) które pobiera bieżącą datę w jednym z trzech formatów:</span><span class="sxs-lookup"><span data-stu-id="07ead-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="07ead-115">W następnym kroku utwórz nową witrynę programu ASP.NET i rozpoczynać formantu ScriptManager AJAX programu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="07ead-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="07ead-116">Następnie dodaj kontrolkę typu etykieta (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub `<asp:Label />` formant sieci web) będzie później przedstawiono w nich wynik wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="07ead-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="07ead-117">Następnie uwzględnić `DynamicPopulateExtender` kontroli i podaj informacje o usłudze sieci web, formantu docelowego, ale nie nazwę kontrolki, co powoduje wyzwolenie wypełniania, zostanie to wykonane w późniejszym czasie na przy użyciu niestandardowych skryptów JavaScript!</span><span class="sxs-lookup"><span data-stu-id="07ead-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="07ead-118">Teraz, aby część języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="07ead-118">Now to the JavaScript part.</span></span> <span data-ttu-id="07ead-119">`$find()` Funkcji, zdefiniowane przez bibliotekę ASP.NET AJAX zwraca odwołanie do obiektów po stronie serwera ASP.NET AJAX Control Toolkit, takich jak `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="07ead-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="07ead-120">W bieżącym pliku `$find("dpe")` zwraca odwołanie do tego `DynamicPopulateExtender` kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="07ead-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="07ead-121">Udostępnia metodę o nazwie `populate()` który wyzwala procesu dynamicznego populacji.</span><span class="sxs-lookup"><span data-stu-id="07ead-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="07ead-122">`populate()` Metoda wymaga jednego argumentu: klucz kontekstu, która będzie służyć jako argument `getDate()` metodę sieci web.</span><span class="sxs-lookup"><span data-stu-id="07ead-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="07ead-123">Dlatego na przykład `$find("dpe").populate("format1")` może wypełnić etykiety z bieżącą datę w formacie dzień miesiąc rok.</span><span class="sxs-lookup"><span data-stu-id="07ead-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="07ead-124">W celu próbki nieco bardziej elastyczne, użytkownik może teraz wybierać między kilka formatów daty.</span><span class="sxs-lookup"><span data-stu-id="07ead-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="07ead-125">Dla każdego z nich jest wyświetlany przycisk radiowy.</span><span class="sxs-lookup"><span data-stu-id="07ead-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="07ead-126">Po użytkownik kliknie przycisk radiowy kod JavaScript dynamicznie wypełnienie etykiety z wybranego formatu daty.</span><span class="sxs-lookup"><span data-stu-id="07ead-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="07ead-127">Poniżej przedstawiono tych przycisków radiowych:</span><span class="sxs-lookup"><span data-stu-id="07ead-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="07ead-128">Należy pamiętać, że w kontekście przycisk radiowy, wyrażenie JavaScript `this.value` odnosi się do wartości bieżącego przycisku, które akurat jest dokładnie te same informacje `getDate()` metoda może pracować.</span><span class="sxs-lookup"><span data-stu-id="07ead-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="07ead-129">[![Kliknięcie przycisku pobiera datę z serwera, w formacie określonym](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="07ead-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="07ead-130">Kliknięcie przycisku pobiera datę z serwera, w formacie określonym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="07ead-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="07ead-131">[Poprzednie](dynamically-populating-a-control-vb.md)
> [dalej](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="07ead-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
