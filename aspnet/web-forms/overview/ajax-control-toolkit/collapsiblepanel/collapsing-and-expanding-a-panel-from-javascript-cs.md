---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go kontrolki CollapsiblePanel w ASP.NET AJAX Control Toolkit...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c7fc2e2f5e4bc74efb7bf22afbc776cd6aee2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813155"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Kontrolka CollapsiblePanel w ASP.NET AJAX Control Toolkit rozszerza panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go ponownie. Te dwie akcje mogą być też wywoływane z niestandardowego kodu języka JavaScript.


## <a name="overview"></a>Omówienie

Kontrolka CollapsiblePanel w ASP.NET AJAX Control Toolkit rozszerza panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go ponownie. Te dwie akcje mogą być też wywoływane z niestandardowego kodu języka JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, tworzenie nowej strony programu ASP.NET i zawierają `ScriptManager` w ramach jednego `<form>` elementu. Spowoduje to załadowanie biblioteki ASP.NET AJAX, która jest wymagana przez zestaw narzędzi kontroli:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Następnie należy utworzyć panel zawierający tekst tak, aby były widoczne Zwiń/Rozwiń wpływu:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Jak widać, w panelu odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (zasadniczo Określa kolor tła i szerokość panelu):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender` Formant wymaga `TargetControlID` atrybutu, tak aby wie zestawu narzędzi, które panelu, aby zwinąć lub rozwinąć na żądanie:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Niestety urządzenia extender aktualnie nie ujawnia konkretnego interfejsu API dla zwijanie i rozwijanie panelu, ale niektóre metody nieudokumentowane będzie wykonywać. Po pierwsze Dodaj trzy przyciski HTML do strony, który następnie wyzwoli JavaScript po stronie klienta, aby zwinąć lub rozwinąć panelu zawartości:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

W przypadku kodu JavaScript po stronie klienta (wprowadzenie `<script type="text/javascript">`), `$find()` metoda musi zostać użyte do dostępu `CollapsiblePanelExtender`. `$find("cpe")` Zwraca odwołanie do niej. Z tego miejsca na określonych metod rozwiąże wykonywanego zadania.

Metoda otwierania (Rozszerzanie) nosi nazwę panelu `_doOpen()`; poniższy kod implementuje `doOpen()` funkcja wywoływana, gdy kliknięto przycisk pierwszy:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Zamykanie lub zwijanie panelu `_doClose()` metody musi być wykonywane. Zatem gdy użytkownik kliknie drugi przycisk, następujący kod JavaScript jest wywoływana:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Trzeci przycisk Włącza/wyłącza stan panelu: od zwinięte do rozwinięta i na odwrót. `CollapsiblePanelExtender` Udostępnia `toggle()` metodę, która dokładnie tak: odwraca stan panelu. Jednak istnieje również inna metoda (który wewnętrznie jest używany przez `toggle()` metoda): `get_Collapsed()` metody `CollapsiblePanelExtender()` informuje NAS, czy panel jest zwinięty. W zależności od wartość zwracaną z tej funkcji jest albo rozwinięte panelu (`_doOpen()` metoda) lub zwinięte (`_doClose()`) metody:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Trzeci przycisk zmienia stan panelu: od zwinięte rozwinięte i Wstecz](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Trzeci przycisk zmienia stan panelu: od zwinięte rozwinięte i Wstecz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](collapsing-and-expanding-a-panel-from-javascript-vb.md)
