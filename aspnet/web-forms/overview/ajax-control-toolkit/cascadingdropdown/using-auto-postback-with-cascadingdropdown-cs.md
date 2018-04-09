---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Przy użyciu automatycznego odświeżania z CascadingDropDown (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 04e0914dd1057f9ce490f68ae3fa9c56766beafb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="b0b16-103">Przy użyciu automatycznego odświeżania z CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="b0b16-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="b0b16-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b0b16-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b0b16-105">[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0b16-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="b0b16-106">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b0b16-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b0b16-107">Jednak przy korzystaniu z formantu CascadingDropDown ASP. Funkcja AutoPostBack kontroli DropDownList przez sieć nie działa, ponieważ asynchronicznie ładowania danych do listy generuje (niepotrzebnych) ogłaszania zwrotnego sam.</span><span class="sxs-lookup"><span data-stu-id="b0b16-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="b0b16-108">Z kodu JavaScript można uniknąć tego efektu.</span><span class="sxs-lookup"><span data-stu-id="b0b16-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="b0b16-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b0b16-109">Overview</span></span>

<span data-ttu-id="b0b16-110">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b0b16-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b0b16-111">(Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Jednak przy korzystaniu z formantu CascadingDropDown ASP. Funkcja AutoPostBack kontroli DropDownList przez sieć nie działa, ponieważ asynchronicznie ładowania danych do listy generuje (niepotrzebnych) ogłaszania zwrotnego sam.</span><span class="sxs-lookup"><span data-stu-id="b0b16-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="b0b16-112">Z kodu JavaScript można uniknąć tego efektu.</span><span class="sxs-lookup"><span data-stu-id="b0b16-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="b0b16-113">Kroki</span><span class="sxs-lookup"><span data-stu-id="b0b16-113">Steps</span></span>

<span data-ttu-id="b0b16-114">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu &lt; `form` &gt; element):</span><span class="sxs-lookup"><span data-stu-id="b0b16-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="b0b16-115">Następnie wymagana jest kontrola DropDownList:</span><span class="sxs-lookup"><span data-stu-id="b0b16-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="b0b16-116">Dla tej listy rozszerzający CascadingDropDown został dodany, dostarczania informacji adresu URL i metody usługi sieci web:</span><span class="sxs-lookup"><span data-stu-id="b0b16-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="b0b16-117">Rozszerzenie CascadingDropDown asynchronicznie wywołuje usługi sieci web za pomocą następujących podpis metody:</span><span class="sxs-lookup"><span data-stu-id="b0b16-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="b0b16-118">Metoda zwraca tablicę typu CascadingDropDown wartości.</span><span class="sxs-lookup"><span data-stu-id="b0b16-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="b0b16-119">Konstruktor typów najpierw oczekuje podpis wpisu listy, a następnie wartość (HTML `value` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="b0b16-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="b0b16-120">Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej z trzech dostawców drugi jest wstępnie wybrany.</span><span class="sxs-lookup"><span data-stu-id="b0b16-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="b0b16-121">Ponadto definiuje ASP.NET `__doPostBack()` metodę JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b0b16-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="b0b16-122">Po załadowaniu strony tego wywołania języka JavaScript jest dodane do listy rozwijanej, ale tylko jeśli istnieją elementy w nim.</span><span class="sxs-lookup"><span data-stu-id="b0b16-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="b0b16-123">Jeśli w liście nie ma elementów, Toolkit kontroli obecnie ładuje je, dzięki czemu kod JavaScript używa przekroczenie limitu czasu i spróbuje ponownie w pół sekundy.</span><span class="sxs-lookup"><span data-stu-id="b0b16-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="b0b16-124">W ten sposób odświeżania strony tylko zostanie wykonany po są faktycznie elementów na liście, a użytkownik wybierze wpis.</span><span class="sxs-lookup"><span data-stu-id="b0b16-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="b0b16-125">[![Wybieranie elementu listy powoduje odświeżenie strony](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0b16-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="b0b16-126">Wybieranie elementu listy powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b0b16-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0b16-127">[Poprzednie](presetting-list-entries-with-cascadingdropdown-cs.md)
> [dalej](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b0b16-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
