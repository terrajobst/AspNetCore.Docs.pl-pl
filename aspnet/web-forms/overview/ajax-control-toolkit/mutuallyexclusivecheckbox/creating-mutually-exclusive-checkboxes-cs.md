---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Tworzenie wykluczają się wzajemnie pola wyboru (C#) | Dokumentacja firmy Microsoft
author: wenz
description: 'Jeżeli można wybrać tylko jeden zestaw opcji, przycisków radiowych zwykle są używane. Brak zwrotnych, choć: po wybraniu jednego przycisku radiowego w grupie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Tworzenie wykluczają się wzajemnie pola wyboru (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Jeżeli można wybrać tylko jeden zestaw opcji, przycisków radiowych zwykle są używane. Brak zwrotnych, choć: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych. Pola wyboru można usunąć zaznaczenia w dowolnym momencie, ale nie wykluczają się wzajemnie. Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.


## <a name="overview"></a>Omówienie

Jeżeli można wybrać tylko jeden zestaw opcji, przycisków radiowych zwykle są używane. Brak zwrotnych, choć: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych. Pola wyboru można usunąć zaznaczenia w dowolnym momencie, ale nie wykluczają się wzajemnie. Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.

## <a name="steps"></a>Kroki

Zestawie narzędzi programu ASP.NET AJAX formant zawiera MutuallyExclusiveCheckBox rozszerzeń. Dzięki temu programiści można przypisać wszystkie pola wyboru o nazwie grupy (`Key` atrybutu). Z wszystkich pól wyboru w ramach tej samej grupy w tym samym czasie można wybrać tylko jeden z nich.

Zacznijmy od umieszczania dwa pola wyboru na nowej strony ASP.NET. Może istnieć więcej, ale dwa z nich wystarczające, aby zademonstrować zasady:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Dla oba pola wyboru formantu MutuallyExclusiveCheckBoxExtender musi znajdować się na stronie. Oba atrybuty klucza muszą mieć taką samą wartość, podobnie jak wartość atrybutów elementów HTML przycisku radiowego musi być taka sama jak określenia grupy, do których należą. Właściwość targetcontrolid równa punkty rozszerzeń dla identyfikatora pola wyboru.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Ponadto obejmować ASP.NET AJAX `ScriptManager` wymaganych przez wszystkie elementy zestawu ASP.NET AJAX kontroli narzędzi:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Zapisz i uruchom strony: można sprawdzić i usuń zaznaczenie pola wyboru, ale nie oba pola wyboru można sprawdzić.


[![Jednocześnie można sprawdzić tylko jedno pole wyboru](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Jednocześnie można sprawdzić tylko jedno pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-mutually-exclusive-checkboxes-vb.md)
