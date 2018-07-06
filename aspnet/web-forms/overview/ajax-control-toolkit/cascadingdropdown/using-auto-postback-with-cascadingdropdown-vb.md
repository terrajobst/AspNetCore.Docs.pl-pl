---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Przy użyciu automatycznego odświeżania z CascadingDropDown (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f0ed4c88789504c09649254ea98a89b01bdd20c5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813844"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="aad4f-103">Przy użyciu automatycznego odświeżania z CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="aad4f-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="aad4f-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="aad4f-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="aad4f-105">[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="aad4f-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="aad4f-106">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="aad4f-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="aad4f-107">Jednak przy użyciu kontrolki CascadingDropDown, ASP. Funkcja automatycznego ogłaszania zwrotnego kontrolki DropDownList przez sieć działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) zwrotu, sam.</span><span class="sxs-lookup"><span data-stu-id="aad4f-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="aad4f-108">Za pomocą kodu JavaScript można uniknąć tego efektu.</span><span class="sxs-lookup"><span data-stu-id="aad4f-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="aad4f-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="aad4f-109">Overview</span></span>

<span data-ttu-id="aad4f-110">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="aad4f-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="aad4f-111">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Jednak przy użyciu kontrolki CascadingDropDown, ASP. Funkcja automatycznego ogłaszania zwrotnego kontrolki DropDownList przez sieć działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) zwrotu, sam.</span><span class="sxs-lookup"><span data-stu-id="aad4f-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="aad4f-112">Za pomocą kodu JavaScript można uniknąć tego efektu.</span><span class="sxs-lookup"><span data-stu-id="aad4f-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="aad4f-113">Kroki</span><span class="sxs-lookup"><span data-stu-id="aad4f-113">Steps</span></span>

<span data-ttu-id="aad4f-114">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu &lt; `form` &gt; elementu):</span><span class="sxs-lookup"><span data-stu-id="aad4f-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="aad4f-115">Następnie formant DropDownList jest wymagane:</span><span class="sxs-lookup"><span data-stu-id="aad4f-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="aad4f-116">Dla tej listy rozszerzeń kontrolki CascadingDropDown jest dodawany, dostarczania informacji adresu URL i metody usługi sieci web:</span><span class="sxs-lookup"><span data-stu-id="aad4f-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="aad4f-117">Rozszerzenie kontrolki CascadingDropDown asynchronicznie wywołuje usługę sieci web za pomocą następujący podpis metody:</span><span class="sxs-lookup"><span data-stu-id="aad4f-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="aad4f-118">Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość.</span><span class="sxs-lookup"><span data-stu-id="aad4f-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="aad4f-119">Konstruktor typu oczekuje, że najpierw podpis wpisu listy, a następnie wartość (HTML `value` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="aad4f-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="aad4f-120">Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej przy użyciu trzech dostawców drugi są wstępnie wybrane.</span><span class="sxs-lookup"><span data-stu-id="aad4f-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="aad4f-121">Ponadto definiuje ASP.NET `__doPostBack()` metodę JavaScript.</span><span class="sxs-lookup"><span data-stu-id="aad4f-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="aad4f-122">Po załadowaniu strony tego wywołania języka JavaScript jest dodawany do listy rozwijanej, ale tylko jeśli istnieją elementy w nim.</span><span class="sxs-lookup"><span data-stu-id="aad4f-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="aad4f-123">Jeśli nie ma elementów na liście, narzędzi kontroli jest Trwa ładowanie wideo, dzięki czemu kod JavaScript używa przekroczenie limitu czasu i próbuje ponownie za pół sekundy.</span><span class="sxs-lookup"><span data-stu-id="aad4f-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="aad4f-124">Dzięki temu tylko jest wykonywany odświeżenie strony, gdy istnieją faktycznie elementów na liście, a użytkownik wybierze wpis.</span><span class="sxs-lookup"><span data-stu-id="aad4f-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="aad4f-125">[![Wybieranie elementu listy powoduje odświeżenie strony](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aad4f-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="aad4f-126">Wybieranie elementu listy powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="aad4f-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="aad4f-127">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="aad4f-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
