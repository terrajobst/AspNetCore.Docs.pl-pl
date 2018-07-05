---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Wypełnianie listy przy użyciu kontrolki CascadingDropDown (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a4271b2697df8e24fca5f7ff30797b1e4e077a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385424"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Wypełnianie listy przy użyciu kontrolki CascadingDropDown (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. (Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Pierwszy wyzwaniem wymagającym rozwiązania jest faktycznie wypełnienia listy rozwijanej, za pomocą tego formantu.


## <a name="overview"></a>Omówienie

Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList. (Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Pierwszy wyzwaniem wymagającym rozwiązania jest faktycznie wypełnienia listy rozwijanej, za pomocą tego formantu.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie formant DropDownList jest wymagane:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy rozszerzeń kontrolki CascadingDropDown zostanie dodany. Asynchroniczne żądanie zostanie wysłane do usługi sieci web, które są następnie przekazywane ponownie listę wpisów do wyświetlenia na liście. Aby to zrobić, należy skonfigurować następujące atrybuty kontrolki CascadingDropDown:

- `ServicePath`: Adres URL usługi sieci web, zapewniając pozycji na liście
- `ServiceMethod`: Metoda sieci web dostarczanie pozycji na liście
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: Informacje kategorii jest przesyłany do usługi po wywołaniu metody sieci web
- `PromptText`: Tekstu wyświetlanego, gdy asynchronicznie ładowania listy danych z serwera

Oto znaczniki dla `CascadingDropDown` elementu. Jedyną różnicą między C# i VB jest nazwą usługi skojarzone sieci web:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Kod JavaScript pochodzący z `CascadingDropDown` extender wywołań metody usługi sieci web z następującą sygnaturą:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Dlatego metoda musi zwracać tablicę typu ważnym aspektem `CascadingDropDownNameValue` (zdefiniowanej przez program ASP.NET AJAX Control Toolkit). W `CascadingDropDownNameValue` konstruktora, pierwszy tekst wpisu listy, a następnie jego wartość musi być podana, podobnie jak `<option value="VALUE">NAME</option>` sposób analogiczny jak w formacie HTML. Poniżej przedstawiono przykładowe dane:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Ładowanie strony w przeglądarce wyzwoli listy ma zostać wypełniony przy użyciu trzech dostawców.


[![Lista jest wypełniana automatycznie](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-cascadingdropdown-with-a-database-cs.md)
