---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamiczne dodawanie okienka kontrolki Accordion (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są deklarowane w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dd82fab03e06aa5dd3baba7dd24734fa964b350
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378965"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamiczne dodawanie okienka kontrolki Accordion (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale kod po stronie serwera może służyć do ten sam efekt.


## <a name="overview"></a>Omówienie

Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są zadeklarowane w obrębie sama strona, ale kod po stronie serwera może służyć do ten sam efekt.

## <a name="steps"></a>Kroki

Kontrolka właściwości Accordion uwidacznia wszystkie ważne właściwości do kodu po stronie serwera. Między innymi `Panes` właściwość udziela dostępu do kolekcja okienek, które tworzą właściwości Accordion. Okienka, co jest typu `AccordionPane`. W związku z tym jest prosta, aby utworzyć takie okienka:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Właściwość `AccordionPane` zapewnia dostęp do kontrolek ASP.NET, w ramach sekcji nagłówka okienka; `ContentContainer` właściwość `AccordionPane` działa tak samo dla zawartości sekcji okienka. Dzięki temu kod platformy ASP.NET do dodawania zawartości do okienka:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Na koniec pane(s) muszą zostać dodane do `Panes` zbiór właściwości Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki właściwości Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Jedynym elementem brak jest właściwości Accordion, która zależy od obecności ASP.NET `ScriptManager` sterowania:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Aby zakończyć w przykładzie, dwóch klas CSS, do którego odwołuje się kontroli właściwości Accordion zawierają informacje dotyczące stylu przeglądarki:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Dane w właściwości accordion dynamicznie został dodany przez kod po stronie serwera](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Dane w właściwości accordion dynamicznie został dodany przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](databinding-to-an-accordion-vb.md)
