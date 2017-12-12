---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: "Przy użyciu automatycznego odświeżania z CascadingDropDown (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: cd103283f46223d5158e58227bb53c00c74bc7d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Przy użyciu automatycznego odświeżania z CascadingDropDown (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. Jednak przy korzystaniu z formantu CascadingDropDown ASP. Funkcja AutoPostBack kontroli DropDownList przez sieć nie działa, ponieważ asynchronicznie ładowania danych do listy generuje (niepotrzebnych) ogłaszania zwrotnego sam. Z kodu JavaScript można uniknąć tego efektu.


## <a name="overview"></a>Omówienie

Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Jednak przy korzystaniu z formantu CascadingDropDown ASP. Funkcja AutoPostBack kontroli DropDownList przez sieć nie działa, ponieważ asynchronicznie ładowania danych do listy generuje (niepotrzebnych) ogłaszania zwrotnego sam. Z kodu JavaScript można uniknąć tego efektu.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu &lt; `form` &gt; element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie wymagana jest kontrola DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy rozszerzający CascadingDropDown został dodany, dostarczania informacji adresu URL i metody usługi sieci web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Rozszerzenie CascadingDropDown asynchronicznie wywołuje usługi sieci web za pomocą następujących podpis metody:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda zwraca tablicę typu CascadingDropDown wartości. Konstruktor typów najpierw oczekuje podpis wpisu listy, a następnie wartość (HTML `value` atrybutu).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej z trzech dostawców drugi jest wstępnie wybrany. Ponadto definiuje ASP.NET `__doPostBack()` metodę JavaScript. Po załadowaniu strony tego wywołania języka JavaScript jest dodane do listy rozwijanej, ale tylko jeśli istnieją elementy w nim. Jeśli w liście nie ma elementów, Toolkit kontroli obecnie ładuje je, dzięki czemu kod JavaScript używa przekroczenie limitu czasu i spróbuje ponownie w pół sekundy.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

W ten sposób odświeżania strony tylko zostanie wykonany po są faktycznie elementów na liście, a użytkownik wybierze wpis.


[![Wybieranie elementu listy powoduje odświeżenie strony](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Wybieranie elementu listy powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](presetting-list-entries-with-cascadingdropdown-cs.md)
[dalej](filling-a-list-using-cascadingdropdown-vb.md)
