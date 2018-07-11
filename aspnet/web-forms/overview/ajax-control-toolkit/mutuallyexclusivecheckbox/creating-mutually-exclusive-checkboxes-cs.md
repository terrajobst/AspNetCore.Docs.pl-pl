---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Tworzenie wzajemnie wykluczających się pól wyboru (C#) | Dokumentacja firmy Microsoft
author: wenz
description: 'Jeżeli można wybrać tylko jeden zestaw opcji, przyciski radiowe zwykle są używane. Istnieje jednak wadą: po wybraniu jednego przycisku radiowego w grupie...'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bce8cd94ed11551f75e48b19cd9cc50b9d023c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818070"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Tworzenie wzajemnie wykluczających się pól wyboru (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Jeżeli można wybrać tylko jeden zestaw opcji, przyciski radiowe zwykle są używane. Istnieje jednak wadą: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych. Pola wyboru mogą być zaznaczone w dowolnym momencie, jednak nie są wzajemnie się wykluczają. Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.


## <a name="overview"></a>Omówienie

Jeżeli można wybrać tylko jeden zestaw opcji, przyciski radiowe zwykle są używane. Istnieje jednak wadą: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych. Pola wyboru mogą być zaznaczone w dowolnym momencie, jednak nie są wzajemnie się wykluczają. Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.

## <a name="steps"></a>Kroki

ASP.NET AJAX Control Toolkit zawiera MutuallyExclusiveCheckBox urządzenia extender. Dzięki temu programiści przypisać wszystkie pola wyboru do nazwy grupy (`Key` atrybutu). Z wszystkich pól wyboru w ramach tej samej grupy w tym samym czasie można wybrać tylko jeden.

Zacznijmy od umieszczenie dwa pola wyboru na nowej stronie ASP.NET. Może istnieć więcej, ale dwa z nich wystarczające, aby zademonstrować zasady:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Oba pola wyboru formantu MutuallyExclusiveCheckBoxExtender musi być wprowadzane na stronie. Obu atrybutów klucza muszą mieć taką samą wartość, podobnie jak wartość atrybutów elementów HTML przycisku radiowego musi być taka sama jak oznaczenia grupy, do których należą. Właściwość TargetControlID punktów rozszerzeń identyfikator pola wyboru.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Na koniec obejmują ASP.NET AJAX `ScriptManager` co jest wymagane przez wszystkie elementy ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Zapisz i uruchom strony: sprawdza, czy i usuń zaznaczenie pola wyboru, jednak w żadnym momencie oba pola wyboru można sprawdzić.


[![Można sprawdzić w danym momencie tylko jedno pole wyboru](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Można sprawdzić w danym momencie tylko jedno pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-mutually-exclusive-checkboxes-vb.md)
