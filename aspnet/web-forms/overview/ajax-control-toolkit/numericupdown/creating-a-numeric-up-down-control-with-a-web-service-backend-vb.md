---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Tworzenie Numeric w górę/dół kontrolki z zapleczem usługi internetowej (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, kontrolki numeric up/down (znajdującą się na Windows i innych systemów operacyjnych) może okazać się c ponieważ coraz więcej...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bf69b3e6932df528e8ccd2348ffa6f13132f99f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753207"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="a11a1-103">Tworzenie kontrolki Numeric Up/Down z zapleczem usługi internetowej (VB)</span><span class="sxs-lookup"><span data-stu-id="a11a1-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="a11a1-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a11a1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a11a1-105">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a11a1-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="a11a1-106">Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, liczbowych w górę/dół kontroli (co oznacza, że istnieje na Windows i innych systemów operacyjnych) można udowodnić, że ponieważ coraz więcej doświadczenia.</span><span class="sxs-lookup"><span data-stu-id="a11a1-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="a11a1-107">Domyślnie formant NumericUpDown zawsze zwiększa lub zmniejsza wartość 1, ale usługi sieci web okaże bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="a11a1-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="a11a1-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a11a1-108">Overview</span></span>

<span data-ttu-id="a11a1-109">Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, liczbowych w górę/dół kontroli (co oznacza, że istnieje na Windows i innych systemów operacyjnych) można udowodnić, że ponieważ coraz więcej doświadczenia.</span><span class="sxs-lookup"><span data-stu-id="a11a1-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="a11a1-110">Domyślnie `NumericUpDown` kontroli zawsze zwiększa lub zmniejsza wartość 1, ale usługi sieci web okaże bardziej elastyczne.</span><span class="sxs-lookup"><span data-stu-id="a11a1-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="a11a1-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="a11a1-111">Steps</span></span>

<span data-ttu-id="a11a1-112">ASP.NET AJAX Control Toolkit zawiera `NumericUpDown` rozszerzeń, który automatycznie dodaje dwa przyciski do pola tekstowego: jedną zwiększenia jego wartość, jedną dla zmniejsza się go.</span><span class="sxs-lookup"><span data-stu-id="a11a1-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="a11a1-113">Jednak formant obsługuje również wywołanie usługi sieci web (lub wywołanie metody strony).</span><span class="sxs-lookup"><span data-stu-id="a11a1-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="a11a1-114">Zawsze, gdy w górę lub dół przycisk po kliknięciu JavaScript kod łączy się z serwerem sieci web i wykonuje metodę istnieje.</span><span class="sxs-lookup"><span data-stu-id="a11a1-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="a11a1-115">Podpis metody jest następujące:</span><span class="sxs-lookup"><span data-stu-id="a11a1-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="a11a1-116">`current` Argument jest bieżąca wartość w polu tekstowym; `tag` atrybut jest dodatkowy kontekst danych, którą można określić jako właściwość `NumericUpDown` rozszerzeń (ale nie jest wymagane).</span><span class="sxs-lookup"><span data-stu-id="a11a1-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="a11a1-117">W tym przykładzie liczbowe w górę/dół kontroli tylko Zezwalaj na wartości, które są potęgi liczb: 1, 2, 4, 8, 16, 32, 64 i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="a11a1-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="a11a1-118">W związku z tym metoda wykonania, gdy użytkownik chce, aby zwiększyć wartość należy double stara wartość; inna metoda należy podzielić wartość przez dwa.</span><span class="sxs-lookup"><span data-stu-id="a11a1-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="a11a1-119">Oto więc usługę sieci web zakończenie:</span><span class="sxs-lookup"><span data-stu-id="a11a1-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="a11a1-120">Na koniec Utwórz nową stronę programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a11a1-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="a11a1-121">Należy w zwykły sposób, `ScriptManager` kontroli `TextBox` kontroli i `NumericUpDownExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="a11a1-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="a11a1-122">W przypadku drugiego nagłówka musisz podać informacje o usłudze sieci web:</span><span class="sxs-lookup"><span data-stu-id="a11a1-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="a11a1-123">`ServiceDownMethod` Nazwa w dół metodę sieci web lub strony — metoda</span><span class="sxs-lookup"><span data-stu-id="a11a1-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="a11a1-124">`ServiceDownPath` Ścieżka do usługi sieci web za pomocą metody dół usługi; pominąć, jeśli używana jest metoda strony</span><span class="sxs-lookup"><span data-stu-id="a11a1-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="a11a1-125">`ServiceUpMethod` Nazwa pracy metodę sieci web lub strony — metoda</span><span class="sxs-lookup"><span data-stu-id="a11a1-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="a11a1-126">`ServiceUpPath` Ścieżka do usługi sieci web za pomocą metody pracy usługi; pominąć, jeśli używana jest metoda strony</span><span class="sxs-lookup"><span data-stu-id="a11a1-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="a11a1-127">Oto kompletny kod znaczników dla strony:</span><span class="sxs-lookup"><span data-stu-id="a11a1-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="a11a1-128">Po uruchomieniu strony, zwróć uwagę, jak wartość w polu tekstowym zawsze podwaja się po kliknięciu przycisk w prawym górnym i filtrach, po kliknięciu przycisku niższe.</span><span class="sxs-lookup"><span data-stu-id="a11a1-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="a11a1-129">[![Są wyświetlane tylko cyfry, które są wartością potęgi liczby 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a11a1-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="a11a1-130">Są wyświetlane tylko cyfry, które są wartością potęgi liczby 2 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a11a1-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a11a1-131">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="a11a1-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
