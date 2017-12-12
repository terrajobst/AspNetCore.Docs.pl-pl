---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: "Obsługa ogłaszania zwrotnego z ModalPopup (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy zachować ostrożność podczas pos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 096c32d6004474d3d5952ce12d7349b9ebda6003
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="31881-104">Obsługa ogłaszania zwrotnego z ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="31881-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="31881-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="31881-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="31881-106">[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="31881-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="31881-107">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="31881-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="31881-108">Specjalne należy uważać podczas odświeżania strony na podstawie w podręcznym.</span><span class="sxs-lookup"><span data-stu-id="31881-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="31881-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="31881-109">Overview</span></span>

<span data-ttu-id="31881-110">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="31881-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="31881-111">Specjalne należy uważać podczas odświeżania strony na podstawie w podręcznym.</span><span class="sxs-lookup"><span data-stu-id="31881-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="31881-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="31881-112">Steps</span></span>

<span data-ttu-id="31881-113">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="31881-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="31881-114">Następnie dodaj panel, która służy jako modalnych menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="31881-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="31881-115">Użytkownik może wprowadzić nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="31881-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="31881-116">Przycisk służy do menu podręcznego. Zamknij i Zapisz informacje.</span><span class="sxs-lookup"><span data-stu-id="31881-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="31881-117">Należy pamiętać, że `OnClick` atrybutu jest ustawiona, dzięki czemu odświeżania strony występuje po kliknięciu tego przycisku:</span><span class="sxs-lookup"><span data-stu-id="31881-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="31881-118">Sama strona zawiera dwie etykiety dla tych samych informacji: Nazwa i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="31881-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="31881-119">Przycisk służy do wyzwalania modalnych menu podręczne:</span><span class="sxs-lookup"><span data-stu-id="31881-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="31881-120">Aby wprowadzić podręcznego są wyświetlane, Dodaj `ModalPopupExtender` formantu.</span><span class="sxs-lookup"><span data-stu-id="31881-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="31881-121">Ustaw `PopupControlID` atrybutu ID panelu i `TargetControlID` przycisku ID:</span><span class="sxs-lookup"><span data-stu-id="31881-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="31881-122">Teraz przy każdym `Save` kliknięciu przycisku w podręcznym modalnej po stronie serwera `SaveData()` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="31881-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="31881-123">Istnieje można zapisać wprowadzonych danych w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="31881-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="31881-124">Dla uproszczenia nowych danych jest tylko dane wyjściowe w etykiecie:</span><span class="sxs-lookup"><span data-stu-id="31881-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="31881-125">Ponadto kontrolki textbox w podręcznym modalne powinno być wypełnione z bieżącym nazwę i adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="31881-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="31881-126">Jednak jest to konieczne tylko po wystąpieniu nie ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="31881-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="31881-127">W przypadku odświeżania strony, funkcja viewstate programu ASP.NET spowoduje automatyczne wypełnienie pola tekstowe z odpowiednimi wartościami.</span><span class="sxs-lookup"><span data-stu-id="31881-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="31881-128">[![Modalne podręcznego powoduje odświeżenie strony](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="31881-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="31881-129">Modalne podręcznego powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="31881-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="31881-130">[Poprzednie](using-modalpopup-with-a-repeater-control-cs.md)
[dalej](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="31881-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
