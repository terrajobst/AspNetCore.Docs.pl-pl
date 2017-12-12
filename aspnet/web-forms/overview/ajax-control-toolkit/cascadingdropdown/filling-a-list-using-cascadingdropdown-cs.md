---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: "Wypełnianie listy przy użyciu CascadingDropDown (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: e5e0f11a815632aff9e17dc0f783f7eba2753995
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Wypełnianie listy przy użyciu CascadingDropDown (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Pierwszego wyzwania rozwiązania jest rzeczywiście wypełnienia listy rozwijanej, w tym formancie.


## <a name="overview"></a>Omówienie

Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Pierwszego wyzwania rozwiązania jest rzeczywiście wypełnienia listy rozwijanej, w tym formancie.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Następnie wymagana jest kontrola DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Dla tej listy rozszerzający CascadingDropDown jest dodawany. Wysyła żądanie asynchroniczne z usługą sieci web, które będą zwracać listy wpisów do wyświetlenia na liście. Aby to zrobić trzeba ustawić CascadingDropDown następujące atrybuty:

- `ServicePath`: Adres URL usługi sieci web dostarczania pozycji na liście
- `ServiceMethod`: Dostarczanie pozycji na liście metody sieci web
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: Kategoria informacje przesyłane podczas wywoływania metody sieci web
- `PromptText`: Tekst wyświetlany, gdy asynchronicznie podczas ładowania danych listy z serwera

Oto kod znaczników dla `CascadingDropDown` elementu. Jedyną różnicą między C# i VB jest nazwą usługi skojarzone sieci web:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Kod JavaScript pochodzący z `CascadingDropDown` rozszerzeń wywołuje metody usługi sieci web z następującą sygnaturą:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Dlatego jest istotnym elementem metoda musi zwracać tablicy typu `CascadingDropDownNameValue` (zdefiniowany w zestawie narzędzi programu ASP.NET AJAX kontroli). W `CascadingDropDownNameValue` contructor, najpierw należy podać hasło listy, a następnie jego wartość, podobnie jak `<option value="VALUE">NAME</option>` zrobić w formacie HTML. Poniżej przedstawiono niektóre przykładowe dane:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Ładowanie strony w przeglądarce wyzwoli listy należy podać trzy dostawców.


[![Lista jest wypełniane automatycznie](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Dalej](using-cascadingdropdown-with-a-database-cs.md)
