---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: "Obsługa ogłaszania zwrotnego przez formant okna podręcznego o UpdatePanel (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Szczególną uwagę należy zwrócić..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 2abcf381aa95ad10d2f36e72d1be64c70fb7d9e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="1a389-104">Obsługa ogłaszania zwrotnego przez formant okna podręcznego o UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="1a389-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="1a389-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1a389-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1a389-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1a389-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="1a389-107">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="1a389-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="1a389-108">Szczególną uwagę brane odświeżania strony występuje w menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="1a389-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="1a389-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1a389-109">Overview</span></span>

<span data-ttu-id="1a389-110">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="1a389-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="1a389-111">Szczególną uwagę brane odświeżania strony występuje w menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="1a389-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="1a389-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="1a389-112">Steps</span></span>

<span data-ttu-id="1a389-113">Korzystając z `PopupControl` z odświeżenie strony, `UpdatePanel` może uniemożliwić odświeżania strony spowodowane ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="1a389-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="1a389-114">Następujący kod definiuje kilka istotnych elementów:</span><span class="sxs-lookup"><span data-stu-id="1a389-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="1a389-115">A `ScriptManager` , aby działa w zestawie narzędzi programu ASP.NET AJAX formantu</span><span class="sxs-lookup"><span data-stu-id="1a389-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="1a389-116">Dwa `TextBox` formantów, które wyzwoli oba okna podręcznego</span><span class="sxs-lookup"><span data-stu-id="1a389-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="1a389-117">A `Panel` formant, który będzie służyć jako menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="1a389-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="1a389-118">W panelu `Calendar` kontroli jest osadzony w `UpdatePanel` formantu</span><span class="sxs-lookup"><span data-stu-id="1a389-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="1a389-119">Dwa `PopupControlExtender` formantów, które przypisać panelu do pól tekstowych</span><span class="sxs-lookup"><span data-stu-id="1a389-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="1a389-120">Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrola została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="1a389-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="1a389-121">Tak, gdy użytkownik zaznaczy datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="1a389-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="1a389-122">W ramach tej metody należy pobrać i zapisywane w polu tekstowym bieżącą datę.</span><span class="sxs-lookup"><span data-stu-id="1a389-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="1a389-123">Poniżej przedstawiono składnię tego: obiekt przede wszystkim, serwer proxy dla `PopupControlExtender` na stronie musi zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="1a389-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="1a389-124">Oferuje zestawie narzędzi programu ASP.NET AJAX kontroli `GetProxyForCurrentPopup()` metody.</span><span class="sxs-lookup"><span data-stu-id="1a389-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="1a389-125">Ta metoda zwraca obiekt obsługuje `Commit()` metodę, która wysyła wartość do kontrolki, która wyzwoliła podręcznego (nie formantu wyzwalająca wywołanie metody!).</span><span class="sxs-lookup"><span data-stu-id="1a389-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="1a389-126">Następujący kod stanowi wybranej daty jako argument dla `Commit()` metody powodują kod do zapisu zwrotnego wybrana data w polu tekstowym:</span><span class="sxs-lookup"><span data-stu-id="1a389-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="1a389-127">Teraz przy każdym kliknięciu na datę w kalendarzu, wybrana data jest wyświetlana w polu tekstowym skojarzone tworzenie formant wyboru daty, które aktualnie znajdują się na wiele witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1a389-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="1a389-128">[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1a389-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="1a389-129">Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1a389-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="1a389-130">[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1a389-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="1a389-131">Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1a389-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1a389-132">[Poprzednie](using-multiple-popup-controls-cs.md)
[dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1a389-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
