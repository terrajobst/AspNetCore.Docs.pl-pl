---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego przez formant okna podręcznego o UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Szczególną uwagę należy zwrócić...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="41ee4-104">Obsługa ogłaszania zwrotnego przez formant okna podręcznego o UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="41ee4-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="41ee4-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="41ee4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="41ee4-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="41ee4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="41ee4-107">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="41ee4-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="41ee4-108">Szczególną uwagę brane odświeżania strony występuje w menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="41ee4-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="41ee4-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="41ee4-109">Overview</span></span>

<span data-ttu-id="41ee4-110">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="41ee4-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="41ee4-111">Szczególną uwagę brane odświeżania strony występuje w menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="41ee4-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="41ee4-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="41ee4-112">Steps</span></span>

<span data-ttu-id="41ee4-113">Korzystając z `PopupControl` z odświeżenie strony, `UpdatePanel` może uniemożliwić odświeżania strony spowodowane ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="41ee4-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="41ee4-114">Następujący kod definiuje kilka istotnych elementów:</span><span class="sxs-lookup"><span data-stu-id="41ee4-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="41ee4-115">A `ScriptManager` , aby działa w zestawie narzędzi programu ASP.NET AJAX formantu</span><span class="sxs-lookup"><span data-stu-id="41ee4-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="41ee4-116">Dwa `TextBox` formantów, które wyzwoli oba okna podręcznego</span><span class="sxs-lookup"><span data-stu-id="41ee4-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="41ee4-117">A `Panel` formant, który będzie służyć jako menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="41ee4-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="41ee4-118">W panelu `Calendar` kontroli jest osadzony w `UpdatePanel` formantu</span><span class="sxs-lookup"><span data-stu-id="41ee4-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="41ee4-119">Dwa `PopupControlExtender` formantów, które przypisać panelu do pól tekstowych</span><span class="sxs-lookup"><span data-stu-id="41ee4-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="41ee4-120">Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrola została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="41ee4-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="41ee4-121">Tak, gdy użytkownik zaznaczy datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="41ee4-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="41ee4-122">W ramach tej metody należy pobrać i zapisywane w polu tekstowym bieżącą datę.</span><span class="sxs-lookup"><span data-stu-id="41ee4-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="41ee4-123">Poniżej przedstawiono składnię tego: obiekt przede wszystkim, serwer proxy dla `PopupControlExtender` na stronie musi zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="41ee4-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="41ee4-124">Oferuje zestawie narzędzi programu ASP.NET AJAX kontroli `GetProxyForCurrentPopup()` metody.</span><span class="sxs-lookup"><span data-stu-id="41ee4-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="41ee4-125">Ta metoda zwraca obiekt obsługuje `Commit()` metodę, która wysyła wartość do kontrolki, która wyzwoliła podręcznego (nie formantu wyzwalająca wywołanie metody!).</span><span class="sxs-lookup"><span data-stu-id="41ee4-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="41ee4-126">Następujący kod stanowi wybranej daty jako argument dla `Commit()` metody powodują kod do zapisu zwrotnego wybrana data w polu tekstowym:</span><span class="sxs-lookup"><span data-stu-id="41ee4-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="41ee4-127">Teraz przy każdym kliknięciu na datę w kalendarzu, wybrana data jest wyświetlana w polu tekstowym skojarzone tworzenie formant wyboru daty, które aktualnie znajdują się na wiele witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="41ee4-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="41ee4-128">[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="41ee4-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="41ee4-129">Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="41ee4-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="41ee4-130">[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="41ee4-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="41ee4-131">Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="41ee4-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="41ee4-132">[Poprzednie](using-multiple-popup-controls-vb.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="41ee4-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
