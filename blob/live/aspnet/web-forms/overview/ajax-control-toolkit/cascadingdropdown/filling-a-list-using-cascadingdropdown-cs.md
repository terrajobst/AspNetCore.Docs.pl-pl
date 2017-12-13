---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: "Wypełnianie listy przy użyciu CascadingDropDown (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: e5e0f11a815632aff9e17dc0f783f7eba2753995
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="945a8-103">Wypełnianie listy przy użyciu CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="945a8-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="945a8-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="945a8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="945a8-105">[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="945a8-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="945a8-106">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="945a8-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="945a8-107">(Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Pierwszego wyzwania rozwiązania jest rzeczywiście wypełnienia listy rozwijanej, w tym formancie.</span><span class="sxs-lookup"><span data-stu-id="945a8-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="945a8-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="945a8-108">Overview</span></span>

<span data-ttu-id="945a8-109">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="945a8-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="945a8-110">(Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Pierwszego wyzwania rozwiązania jest rzeczywiście wypełnienia listy rozwijanej, w tym formancie.</span><span class="sxs-lookup"><span data-stu-id="945a8-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="945a8-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="945a8-111">Steps</span></span>

<span data-ttu-id="945a8-112">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="945a8-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="945a8-113">Następnie wymagana jest kontrola DropDownList:</span><span class="sxs-lookup"><span data-stu-id="945a8-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="945a8-114">Dla tej listy rozszerzający CascadingDropDown jest dodawany.</span><span class="sxs-lookup"><span data-stu-id="945a8-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="945a8-115">Wysyła żądanie asynchroniczne z usługą sieci web, które będą zwracać listy wpisów do wyświetlenia na liście.</span><span class="sxs-lookup"><span data-stu-id="945a8-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="945a8-116">Aby to zrobić trzeba ustawić CascadingDropDown następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="945a8-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="945a8-117">`ServicePath`: Adres URL usługi sieci web dostarczania pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="945a8-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="945a8-118">`ServiceMethod`: Dostarczanie pozycji na liście metody sieci web</span><span class="sxs-lookup"><span data-stu-id="945a8-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="945a8-119">`TargetControlID`: Identyfikator listy rozwijanej</span><span class="sxs-lookup"><span data-stu-id="945a8-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="945a8-120">`Category`: Kategoria informacje przesyłane podczas wywoływania metody sieci web</span><span class="sxs-lookup"><span data-stu-id="945a8-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="945a8-121">`PromptText`: Tekst wyświetlany, gdy asynchronicznie podczas ładowania danych listy z serwera</span><span class="sxs-lookup"><span data-stu-id="945a8-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="945a8-122">Oto kod znaczników dla `CascadingDropDown` elementu.</span><span class="sxs-lookup"><span data-stu-id="945a8-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="945a8-123">Jedyną różnicą między C# i VB jest nazwą usługi skojarzone sieci web:</span><span class="sxs-lookup"><span data-stu-id="945a8-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="945a8-124">Kod JavaScript pochodzący z `CascadingDropDown` rozszerzeń wywołuje metody usługi sieci web z następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="945a8-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="945a8-125">Dlatego jest istotnym elementem metoda musi zwracać tablicy typu `CascadingDropDownNameValue` (zdefiniowany w zestawie narzędzi programu ASP.NET AJAX kontroli).</span><span class="sxs-lookup"><span data-stu-id="945a8-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="945a8-126">W `CascadingDropDownNameValue` contructor, najpierw należy podać hasło listy, a następnie jego wartość, podobnie jak `<option value="VALUE">NAME</option>` zrobić w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="945a8-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="945a8-127">Poniżej przedstawiono niektóre przykładowe dane:</span><span class="sxs-lookup"><span data-stu-id="945a8-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="945a8-128">Ładowanie strony w przeglądarce wyzwoli listy należy podać trzy dostawców.</span><span class="sxs-lookup"><span data-stu-id="945a8-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="945a8-129">[![Lista jest wypełniane automatycznie](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="945a8-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="945a8-130">Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="945a8-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="945a8-131">Dalej</span><span class="sxs-lookup"><span data-stu-id="945a8-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
