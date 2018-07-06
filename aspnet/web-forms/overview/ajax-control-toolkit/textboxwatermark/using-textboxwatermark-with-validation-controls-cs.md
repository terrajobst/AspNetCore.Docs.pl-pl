---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, go czy...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 144e4ee8f8096076b4e498844429ebf96e6ea402
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836197"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="bface-104">Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (C#)</span><span class="sxs-lookup"><span data-stu-id="bface-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="bface-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bface-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bface-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bface-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="bface-107">Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu.</span><span class="sxs-lookup"><span data-stu-id="bface-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="bface-108">Gdy użytkownik kliknie w polu, będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="bface-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="bface-109">Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie.</span><span class="sxs-lookup"><span data-stu-id="bface-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="bface-110">Może to kolidować z kontrolkami walidacji platformy ASP.NET na tej samej stronie, ale może być można rozwiązać te problemy.</span><span class="sxs-lookup"><span data-stu-id="bface-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="bface-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="bface-111">Overview</span></span>

<span data-ttu-id="bface-112">`TextBoxWatermark` Formantu w zestawu narzędzi AJAX Control Toolkit rozszerza pola tekstowego, więc, że tekst jest wyświetlany w polu.</span><span class="sxs-lookup"><span data-stu-id="bface-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="bface-113">Gdy użytkownik kliknie w polu, będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="bface-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="bface-114">Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie.</span><span class="sxs-lookup"><span data-stu-id="bface-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="bface-115">Może to kolidować z kontrolkami walidacji platformy ASP.NET na tej samej stronie, ale może być można rozwiązać te problemy.</span><span class="sxs-lookup"><span data-stu-id="bface-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="bface-116">Kroki</span><span class="sxs-lookup"><span data-stu-id="bface-116">Steps</span></span>

<span data-ttu-id="bface-117">Konfiguracja podstawowa próbki jest następująca: `TextBox` kontroli jest znakiem wodnym przy użyciu `TextBoxWatermarkExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="bface-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="bface-118">Przycisk wyzwala odświeżenie strony i później będzie służyć do wyzwolenia formanty sprawdzania poprawności na stronie.</span><span class="sxs-lookup"><span data-stu-id="bface-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="bface-119">Ponadto `ScriptManager` sterowania jest wymagany do zainicjowania ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="bface-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="bface-120">Teraz Dodaj `RequiredFieldValidator` formant, który sprawdza, czy znajduje się tekst w polu po przesłaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="bface-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="bface-121">`InitialValue` Właściwości modułu sprawdzania poprawności, należy określić tę samą wartość, która jest używana w `TextBoxWatermarkExtender` kontroli: po przesłaniu formularza, wartości bez zmian pole tekstowe jest wartość limitu znajdujący się w nim:</span><span class="sxs-lookup"><span data-stu-id="bface-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="bface-122">Jednak jeden problem występuje w przypadku tej metody: Jeśli klient wyłącza JavaScript, pole tekstowe jest nie wstępnie przy użyciu tekstu znaku wodnego, w związku z tym `RequiredFieldValidator` nie spowoduje wyzwolenia komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="bface-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="bface-123">W związku z tym, sekundy `RequiredFieldValidator` kontroli jest wymagana, która sprawdza, czy puste pole tekstowe (z pominięciem `InitialValue` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="bface-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="bface-124">Ponieważ korzystać z obu modułów sprawdzania poprawności `Display` = `"Dynamic"`, użytkownik końcowy nie można odróżnić od wygląd, które z dwóch modułów sprawdzania poprawności zostało wywołane; zamiast tego należy prawdopodobnie wystąpił tylko jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="bface-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="bface-125">Na koniec należy dodać kod po stronie serwera w danych wyjściowych tekst w polu, jeśli żadnego modułu weryfikacji wydany komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="bface-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="bface-126">[![Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bface-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="bface-127">Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bface-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bface-128">[Poprzednie](using-textboxwatermark-in-a-formview-cs.md)
> [dalej](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bface-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
