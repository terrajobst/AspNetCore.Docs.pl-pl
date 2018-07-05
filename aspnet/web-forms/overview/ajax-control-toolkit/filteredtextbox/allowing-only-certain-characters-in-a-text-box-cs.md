---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Zezwalanie tylko na niektóre znaki w polu tekstowym (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7203d7f367f275d2d80c86119edc9645c9d24c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400733"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="d45b4-104">Zezwalanie tylko na niektóre znaki w polu tekstowym (C#)</span><span class="sxs-lookup"><span data-stu-id="d45b4-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="d45b4-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d45b4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d45b4-106">[Pobierz program Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d45b4-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="d45b4-107">Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d45b4-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="d45b4-108">Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="d45b4-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="d45b4-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d45b4-109">Overview</span></span>

<span data-ttu-id="d45b4-110">Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d45b4-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="d45b4-111">Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="d45b4-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="d45b4-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d45b4-112">Steps</span></span>

<span data-ttu-id="d45b4-113">ASP.NET AJAX Control Toolkit zawiera `FilteredTextBox` kontrolki, która rozszerza pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="d45b4-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="d45b4-114">Po uaktywnieniu tylko w określonym zestawie znaków mogą być wprowadzane w polu.</span><span class="sxs-lookup"><span data-stu-id="d45b4-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="d45b4-115">Aby to zrobić, najpierw należy w zwykły sposób ASP.NET AJAX `ScriptManager` która ładuje bibliotek JavaScript, które są również używane przez program ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="d45b4-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="d45b4-116">W efekcie potrzebujemy pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="d45b4-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="d45b4-117">Na koniec `FilteredTextBoxExtender` kontrolka zajmuje się ograniczenie znaków, użytkownik może wpisać.</span><span class="sxs-lookup"><span data-stu-id="d45b4-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="d45b4-118">Najpierw ustaw `TargetControlID` atrybutu `ID` z `TextBox` kontroli.</span><span class="sxs-lookup"><span data-stu-id="d45b4-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="d45b4-119">Następnie wybierz jedną z dostępnych `FilterType` wartości:</span><span class="sxs-lookup"><span data-stu-id="d45b4-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="d45b4-120">`Custom` Domyślnie; Musisz podać listę prawidłowe znaki</span><span class="sxs-lookup"><span data-stu-id="d45b4-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="d45b4-121">`LowercaseLetters` małe litery</span><span class="sxs-lookup"><span data-stu-id="d45b4-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="d45b4-122">`Numbers` tylko cyfry</span><span class="sxs-lookup"><span data-stu-id="d45b4-122">`Numbers` digits only</span></span>
- <span data-ttu-id="d45b4-123">`UppercaseLetters` tylko wielkie litery</span><span class="sxs-lookup"><span data-stu-id="d45b4-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="d45b4-124">Jeśli `Custom FilterType` jest używany, `ValidChars` właściwość musi być ustawione i podać listę znaków, które mogą być wpisana.</span><span class="sxs-lookup"><span data-stu-id="d45b4-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="d45b4-125">Przy okazji: Jeśli użytkownik próbuje wkleić tekst w polu tekstowym, zostaną usunięte wszystkie nieprawidłowe znaki.</span><span class="sxs-lookup"><span data-stu-id="d45b4-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="d45b4-126">Oto znaczniki dla `FilteredTextBoxExtender` formant, który dopuszcza tylko cyfr (coś, który także byłby możliwego `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="d45b4-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="d45b4-127">Uruchomić stronę i spróbuj wprowadzić literą, jeśli włączona JavaScript, nie będzie on działał; cyfry są jednak wyświetlane na stronie.</span><span class="sxs-lookup"><span data-stu-id="d45b4-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="d45b4-128">Jednak należy pamiętać, że ochrona `FilteredTextBox` zapewnia nie jest dowód punktor: Jeśli JavaScript jest włączony, wszelkie dane mogą być wprowadzane w polu tekstowym, więc trzeba użyć oznacza, że dodatkowe sprawdzenie poprawności, czyli ASP. Formanty sprawdzania poprawności w sieci.</span><span class="sxs-lookup"><span data-stu-id="d45b4-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="d45b4-129">[![Mogą być wprowadzane tylko cyfry](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d45b4-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="d45b4-130">Mogą być wprowadzane tylko cyfry ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d45b4-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d45b4-131">Next</span><span class="sxs-lookup"><span data-stu-id="d45b4-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
