---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Ustawianie wstępne listy wpisów z CascadingDropDown (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d87da6c19f6dbdff70eff410ba7573c3e26884fb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Ustawianie wstępne listy wpisów z CascadingDropDown (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. Z niewielki kod jest możliwe, że element listy jest wstępnie wybrana po dynamicznie załadować danych.


## <a name="overview"></a>Omówienie

Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Z niewielki kod jest możliwe, że element listy jest wstępnie wybrana po dynamicznie załadować danych.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie wymagana jest kontrola DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy rozszerzający CascadingDropDown został dodany, dostarczania informacji adresu URL i metody usługi sieci web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

Rozszerzenie CascadingDropDown asynchronicznie wywołuje usługi sieci web za pomocą następujących podpis metody:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Metoda zwraca tablicę typu CascadingDropDown wartości. Konstruktor typów najpierw oczekuje podpis wpisu listy, a następnie wartość (HTML `value` atrybutu). Jeśli trzeci argument ma wartość PRAWDA, z listy element jest automatycznie wybierany w przeglądarce.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Ładowanie strony w przeglądarce spowoduje wypełnienie listy rozwijanej z trzech dostawców drugi jest wstępnie wybrany.


[![Listy są wypełnione i wstępnie wybrana automatycznie](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

Listy są wypełnione i wstępnie wybrana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-cascadingdropdown-with-a-database-cs.md)
> [dalej](using-auto-postback-with-cascadingdropdown-cs.md)
