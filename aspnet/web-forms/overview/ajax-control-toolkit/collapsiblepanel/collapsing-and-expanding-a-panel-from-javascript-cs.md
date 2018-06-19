---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant CollapsiblePanel w zestawie narzędzi programu ASP.NET AJAX kontroli rozszerza panelu i zapewnia możliwość zwijanie zawartością i rozwiń go...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 7baa3be7144946bde7d11afd9b1cb5f14ad9dede
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875542"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Formant CollapsiblePanel w zestawie narzędzi programu ASP.NET AJAX kontroli rozszerza panelu i zapewnia możliwość zwijanie zawartością i rozwiń go ponownie. Te dwie akcje można również uruchomić z niestandardowego kodu JavaScript.


## <a name="overview"></a>Omówienie

Formant CollapsiblePanel w zestawie narzędzi programu ASP.NET AJAX kontroli rozszerza panelu i zapewnia możliwość zwijanie zawartością i rozwiń go ponownie. Te dwie akcje można również uruchomić z niestandardowego kodu JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, Utwórz nową stronę ASP.NET i obejmują `ScriptManager` w ramach jednej `<form>` elementu. Spowoduje to załadowanie biblioteki ASP.NET AJAX, co jest wymagane przez zestaw narzędzi do sterowania:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Następnie należy utworzyć panelu z tekstem tak, aby były widoczne efekt Rozwiń/Zwiń:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Jak widać, panelu odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (i zasadniczo definiuje kolor tła i szerokość panelu):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender` Formant wymaga `TargetControlID` atrybutu, aby zestaw narzędzi znała panelu, które można zwijać i rozwijać na żądanie:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Niestety extender aktualnie nie uwidacznia interfejsu API dla zwijanie lub rozwijanie panelu, ale niektóre metody nieudokumentowanej będzie wykonywać. Po pierwsze Dodaj trzy przyciski HTML do strony, które następnie wyzwoli JavaScript po stronie klienta, które można zwijać i rozwijać zawartość panelu:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

W kodzie JavaScript po stronie klienta (wprowadzenie do `<script type="text/javascript">`), `$find()` metoda musi zostać użyte do dostępu `CollapsiblePanelExtender`. `$find("cpe")` Zwraca odwołanie do niej. Stamtąd na konkretnych metod rozwiąże wykonywanego zadania.

Metoda otwierania (rozszerzenie) jest nazywany panelu `_doOpen()`; poniższy kod implementuje `doOpen()` funkcja wywoływana po kliknięciu przycisku pierwszej:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Zamykanie lub zwijanie panelu `_doClose()` metody ma zostać wykonana. Tak, gdy użytkownik kliknie przycisk drugiego, następujący kod JavaScript jest wywoływana:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Trzeci przycisk Przełącza stan panelu: z zwinięte do rozwinięta i na odwrót. `CollapsiblePanelExtender` Przedstawia `toggle()` metodę, która jest dokładnie który: odwraca stan panelu. Jednak istnieje również inna metoda (wewnętrznie używany przez `toggle()` metody): `get_Collapsed()` metody `CollapsiblePanelExtender()` informuje NAS, czy panel jest zwinięty. W zależności od zwracanej wartości tej funkcji, panelu jest następnie albo rozwinięty (`_doOpen()` metody) lub zwinięte (`_doClose()`) metody:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Trzeci przycisk zmieni stan panelu: z zwinięte rozwinięte i zapasowego](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Trzeci przycisk zmieni stan panelu: z zwinięte rozwinięte i zapasowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](collapsing-and-expanding-a-panel-from-javascript-vb.md)
