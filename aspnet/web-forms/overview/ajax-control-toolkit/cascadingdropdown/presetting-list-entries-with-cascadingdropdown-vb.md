---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Wstępne Ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5133516311478d0a4faab45721c6b1d0a251b4b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817755"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Wstępne Ustawianie pozycji listy przy użyciu kontrolki CascadingDropDown (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. Za pomocą niewielki kod jest możliwe, że element listy wyświetlana jest wartość po dynamicznie załadowane dane.


## <a name="overview"></a>Omówienie

Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. (Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Za pomocą niewielki kod jest możliwe, że element listy wyświetlana jest wartość po dynamicznie załadowane dane.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Następnie formant DropDownList jest wymagane:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Dla tej listy rozszerzeń kontrolki CascadingDropDown jest dodawany, dostarczania informacji adresu URL i metody usługi sieci web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Rozszerzenie kontrolki CascadingDropDown asynchronicznie wywołuje usługę sieci web za pomocą następujący podpis metody:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Metoda zwraca tablicę typu kontrolki CascadingDropDown wartość. Konstruktor typu oczekuje, że najpierw podpis wpisu listy, a następnie wartość (HTML `value` atrybutu). Jeśli trzeci argument jest ustawiona na wartość true, na liście elementu jest automatycznie wybierany w przeglądarce.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej przy użyciu trzech dostawców drugi są wstępnie wybrane.


[![Lista jest wypełniana i wstępnie wybrane automatycznie](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

Lista jest wypełniana i wstępnie wybrane automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-cascadingdropdown-with-a-database-vb.md)
> [dalej](using-auto-postback-with-cascadingdropdown-vb.md)
