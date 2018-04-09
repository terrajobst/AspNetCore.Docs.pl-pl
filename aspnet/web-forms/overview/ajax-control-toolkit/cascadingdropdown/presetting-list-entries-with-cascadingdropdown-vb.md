---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Ustawianie wstępne listy wpisów z CascadingDropDown (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="6f13a-103">Ustawianie wstępne listy wpisów z CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="6f13a-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="6f13a-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6f13a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6f13a-105">[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f13a-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="6f13a-106">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="6f13a-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="6f13a-107">Z niewielki kod jest możliwe, że element listy jest wstępnie wybrana po dynamicznie załadować danych.</span><span class="sxs-lookup"><span data-stu-id="6f13a-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="6f13a-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6f13a-108">Overview</span></span>

<span data-ttu-id="6f13a-109">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="6f13a-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="6f13a-110">(Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Z niewielki kod jest możliwe, że element listy jest wstępnie wybrana po dynamicznie załadować danych.</span><span class="sxs-lookup"><span data-stu-id="6f13a-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="6f13a-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="6f13a-111">Steps</span></span>

<span data-ttu-id="6f13a-112">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="6f13a-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="6f13a-113">Następnie wymagana jest kontrola DropDownList:</span><span class="sxs-lookup"><span data-stu-id="6f13a-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="6f13a-114">Dla tej listy rozszerzający CascadingDropDown został dodany, dostarczania informacji adresu URL i metody usługi sieci web:</span><span class="sxs-lookup"><span data-stu-id="6f13a-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="6f13a-115">Rozszerzenie CascadingDropDown asynchronicznie wywołuje usługi sieci web za pomocą następujących podpis metody:</span><span class="sxs-lookup"><span data-stu-id="6f13a-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="6f13a-116">Metoda zwraca tablicę typu CascadingDropDown wartości.</span><span class="sxs-lookup"><span data-stu-id="6f13a-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="6f13a-117">Konstruktor typów najpierw oczekuje podpis wpisu listy, a następnie wartość (HTML `value` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="6f13a-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="6f13a-118">Jeśli trzeci argument ma wartość PRAWDA, z listy element jest automatycznie wybierany w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="6f13a-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="6f13a-119">Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej z trzech dostawców drugi jest wstępnie wybrany.</span><span class="sxs-lookup"><span data-stu-id="6f13a-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="6f13a-120">[![Listy są wypełnione i wstępnie wybrana automatycznie](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f13a-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="6f13a-121">Listy są wypełnione i wstępnie wybrana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6f13a-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f13a-122">[Poprzednie](using-cascadingdropdown-with-a-database-vb.md)
> [dalej](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6f13a-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
