---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy zwrócić...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 758ed5eef32be0fb596d8b9dbf903729816746bd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377976"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="35663-104">Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="35663-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="35663-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="35663-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="35663-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="35663-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="35663-107">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="35663-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="35663-108">Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="35663-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="35663-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="35663-109">Overview</span></span>

<span data-ttu-id="35663-110">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="35663-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="35663-111">Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="35663-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="35663-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="35663-112">Steps</span></span>

<span data-ttu-id="35663-113">Korzystając z `PopupControl` z zwrotu `UpdatePanel` może uniemożliwić odświeżania strony spowodowane zwrotu.</span><span class="sxs-lookup"><span data-stu-id="35663-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="35663-114">Następujące znaczniki definiuje kilka ważnych elementów:</span><span class="sxs-lookup"><span data-stu-id="35663-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="35663-115">A `ScriptManager` kontrolki ASP.NET AJAX Control Toolkit działa</span><span class="sxs-lookup"><span data-stu-id="35663-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="35663-116">Dwa `TextBox` formanty, które wyzwolą oba okna podręcznego</span><span class="sxs-lookup"><span data-stu-id="35663-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="35663-117">A `Panel` formant, który będzie służyć jako menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="35663-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="35663-118">W panelu `Calendar` kontroli jest osadzony w ramach `UpdatePanel` kontroli</span><span class="sxs-lookup"><span data-stu-id="35663-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="35663-119">Dwa `PopupControlExtender` mechanizmy przypisać panelu do pól tekstowych</span><span class="sxs-lookup"><span data-stu-id="35663-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="35663-120">Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrolki jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="35663-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="35663-121">Tak, gdy użytkownik wybierze datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="35663-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="35663-122">W ramach tej metody należy pobrać i zapisywane z powrotem do pola tekstowego bieżącą datę.</span><span class="sxs-lookup"><span data-stu-id="35663-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="35663-123">Ma następującą składnię, w tym: po pierwsze, serwer proxy dla obiektu `PopupControlExtender` na stronie musi zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="35663-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="35663-124">ASP.NET AJAX Control Toolkit oferuje `GetProxyForCurrentPopup()` metody.</span><span class="sxs-lookup"><span data-stu-id="35663-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="35663-125">Obiekt, Metoda ta zwraca obsługuje `Commit()` metodę, która wysyła wartość do formantu, który wyzwolił menu podręczne (nie formant który wyzwolił wywołanie metody!).</span><span class="sxs-lookup"><span data-stu-id="35663-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="35663-126">Poniższy kod zawiera wybranej daty jako argument dla `Commit()` metody, powodując kod w celu zapisania wybranej daty z powrotem do pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="35663-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="35663-127">Teraz po każdym kliknięciu datę w kalendarzu wybrana data pojawia się w polu tekstowym skojarzone tworzenie kontrolki selektora daty, które aktualnie znajdują się na wiele witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="35663-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="35663-128">[![Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="35663-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="35663-129">Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="35663-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="35663-130">[![Klikając pozycję na wartość typu date umieszcza go w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="35663-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="35663-131">Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="35663-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35663-132">[Poprzednie](using-multiple-popup-controls-cs.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="35663-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
