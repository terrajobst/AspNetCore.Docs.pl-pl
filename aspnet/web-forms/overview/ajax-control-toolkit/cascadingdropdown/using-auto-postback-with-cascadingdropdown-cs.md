---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Przy użyciu automatycznego odświeżania z CascadingDropDown (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: bece4f78b46c44db988e04e0e987450d94c37aab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819383"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Przy użyciu automatycznego odświeżania z CascadingDropDown (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. Jednak przy użyciu kontrolki CascadingDropDown, ASP. Funkcja automatycznego ogłaszania zwrotnego kontrolki DropDownList przez sieć działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) zwrotu, sam. Za pomocą kodu JavaScript można uniknąć tego efektu.


## <a name="overview"></a>Omówienie

Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. (Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Jednak przy użyciu kontrolki CascadingDropDown, ASP. Funkcja automatycznego ogłaszania zwrotnego kontrolki DropDownList przez sieć działa, ponieważ asynchroniczne ładowanie danych do listy generuje (niepotrzebne) zwrotu, sam. Za pomocą kodu JavaScript można uniknąć tego efektu.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu &lt; `form` &gt; elementu):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie formant DropDownList jest wymagane:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy rozszerzeń kontrolki CascadingDropDown jest dodawany, dostarczania informacji adresu URL i metody usługi sieci web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Rozszerzenie kontrolki CascadingDropDown asynchronicznie wywołuje usługę sieci web za pomocą następujący podpis metody:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość. Konstruktor typu oczekuje, że najpierw podpis wpisu listy, a następnie wartość (HTML `value` atrybutu).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej przy użyciu trzech dostawców drugi są wstępnie wybrane. Ponadto definiuje ASP.NET `__doPostBack()` metodę JavaScript. Po załadowaniu strony tego wywołania języka JavaScript jest dodawany do listy rozwijanej, ale tylko jeśli istnieją elementy w nim. Jeśli nie ma elementów na liście, narzędzi kontroli jest Trwa ładowanie wideo, dzięki czemu kod JavaScript używa przekroczenie limitu czasu i próbuje ponownie za pół sekundy.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Dzięki temu tylko jest wykonywany odświeżenie strony, gdy istnieją faktycznie elementów na liście, a użytkownik wybierze wpis.


[![Wybieranie elementu listy powoduje odświeżenie strony](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Wybieranie elementu listy powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](presetting-list-entries-with-cascadingdropdown-cs.md)
> [dalej](filling-a-list-using-cascadingdropdown-vb.md)
