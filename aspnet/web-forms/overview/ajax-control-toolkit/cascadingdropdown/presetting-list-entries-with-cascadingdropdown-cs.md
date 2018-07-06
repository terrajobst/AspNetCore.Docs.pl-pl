---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Wstępne Ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 7797aa91452bfbed2695fd26e3b9d2a5783dd216
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825092"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Wstępne Ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. Za pomocą niewielki kod jest możliwe, że element listy wyświetlana jest wartość po dynamicznie załadowane dane.


## <a name="overview"></a>Omówienie

Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. (Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Za pomocą niewielki kod jest możliwe, że element listy wyświetlana jest wartość po dynamicznie załadowane dane.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie formant DropDownList jest wymagane:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy rozszerzeń kontrolki CascadingDropDown jest dodawany, dostarczania informacji adresu URL i metody usługi sieci web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

Rozszerzenie kontrolki CascadingDropDown asynchronicznie wywołuje usługę sieci web za pomocą następujący podpis metody:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość. Konstruktor typu oczekuje, że najpierw podpis wpisu listy, a następnie wartość (HTML `value` atrybutu). Jeśli trzeci argument jest ustawiona na wartość true, na liście elementu jest automatycznie wybierany w przeglądarce.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej przy użyciu trzech dostawców drugi są wstępnie wybrane.


[![Lista jest wypełniana i wstępnie wybrane automatycznie](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

Lista jest wypełniana i wstępnie wybrane automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-cascadingdropdown-with-a-database-cs.md)
> [dalej](using-auto-postback-with-cascadingdropdown-cs.md)
