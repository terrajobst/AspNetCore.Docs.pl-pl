---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: "Zezwalanie tylko niektórych znaków w polu tekstowym (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formanty weryfikacji platformy ASP.NET można upewnij się, czy tylko niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal uniemożliwia użytkownikom wpisywania nieprawidłowy..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 246c3b5dd55ceb0f47ad1f4982ae5b3bf855e747
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="292e3-104">Zezwalanie tylko niektórych znaków w polu tekstowym (C#)</span><span class="sxs-lookup"><span data-stu-id="292e3-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="292e3-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="292e3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="292e3-106">[Pobierz kod](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="292e3-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="292e3-107">Formanty weryfikacji platformy ASP.NET można upewnij się, czy tylko niektóre znaki są dozwolone w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="292e3-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="292e3-108">Jednak to nadal nie uniemożliwić użytkownikom wpisując nieprawidłowych znaków, a w trakcie przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="292e3-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="292e3-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="292e3-109">Overview</span></span>

<span data-ttu-id="292e3-110">Formanty weryfikacji platformy ASP.NET można upewnij się, czy tylko niektóre znaki są dozwolone w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="292e3-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="292e3-111">Jednak to nadal nie uniemożliwić użytkownikom wpisując nieprawidłowych znaków, a w trakcie przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="292e3-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="292e3-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="292e3-112">Steps</span></span>

<span data-ttu-id="292e3-113">Zawiera zestawie narzędzi programu ASP.NET AJAX kontroli `FilteredTextBox` kontrolują, które rozszerza pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="292e3-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="292e3-114">Po uaktywnieniu w polu można wprowadzać tylko w określonym zestawie znaków.</span><span class="sxs-lookup"><span data-stu-id="292e3-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="292e3-115">Aby to zrobić, najpierw należy w zwykły sposób ASP.NET AJAX `ScriptManager` który ładuje bibliotekach JavaScript, które są również używane w zestawie narzędzi programu ASP.NET AJAX sterowania:</span><span class="sxs-lookup"><span data-stu-id="292e3-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="292e3-116">Następnie potrzebujemy pole tekstowe:</span><span class="sxs-lookup"><span data-stu-id="292e3-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="292e3-117">Na koniec `FilteredTextBoxExtender` kontroli zajmuje się ograniczenie znaków, użytkownik może wpisać.</span><span class="sxs-lookup"><span data-stu-id="292e3-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="292e3-118">Najpierw należy ustawić `TargetControlID` atrybutu `ID` z `TextBox` formantu.</span><span class="sxs-lookup"><span data-stu-id="292e3-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="292e3-119">Następnie wybierz jedną z dostępnych `FilterType` wartości:</span><span class="sxs-lookup"><span data-stu-id="292e3-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="292e3-120">`Custom`Domyślnie; należy podać listę prawidłowe znaki</span><span class="sxs-lookup"><span data-stu-id="292e3-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="292e3-121">`LowercaseLetters`małe litery</span><span class="sxs-lookup"><span data-stu-id="292e3-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="292e3-122">`Numbers`tylko cyfr</span><span class="sxs-lookup"><span data-stu-id="292e3-122">`Numbers` digits only</span></span>
- <span data-ttu-id="292e3-123">`UppercaseLetters`tylko wielkie litery</span><span class="sxs-lookup"><span data-stu-id="292e3-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="292e3-124">Jeśli `Custom FilterType` jest używana, `ValidChars` właściwości musi być ustawione i zawierają listę znaków, które mogą zostać wpisany.</span><span class="sxs-lookup"><span data-stu-id="292e3-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="292e3-125">Sposobem: próba Wklej tekst w polu tekstowym zostaną usunięte wszystkie nieprawidłowe znaki.</span><span class="sxs-lookup"><span data-stu-id="292e3-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="292e3-126">Oto kod znaczników dla `FilteredTextBoxExtender` formant, który zezwala tylko cyfry (coś, co także byłoby możliwe za pomocą `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="292e3-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="292e3-127">Uruchomić stronę i spróbuj wprowadzić literę, czy włączyć obsługę języka JavaScript, nie będzie on działał; cyfr jednak wyświetlane na stronie.</span><span class="sxs-lookup"><span data-stu-id="292e3-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="292e3-128">Jednak należy pamiętać, że ochrony `FilteredTextBox` zapewnia nie jest potwierdzenie punktor: Jeśli JavaScript jest włączony, wszelkie dane mogą być wprowadzane w polu tekstowym, należy użyć oznacza, że dodatkowe sprawdzenie poprawności, tj. ASP. Formanty walidacji w sieci.</span><span class="sxs-lookup"><span data-stu-id="292e3-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="292e3-129">[![Można wprowadzać tylko cyfry](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="292e3-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="292e3-130">Można wprowadzać tylko cyfry ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="292e3-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="292e3-131">Dalej</span><span class="sxs-lookup"><span data-stu-id="292e3-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
