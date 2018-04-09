---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Tworzenie niestandardowych HTML wątków (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest aby zademonstrować, jak utworzyć niestandardowe pomocników HTML używanej w ramach widoków MVC. Dzięki wykorzystaniu pomocnika kodu HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-html-helpers-c"></a>Tworzenie niestandardowych HTML wątków (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Celem tego samouczka jest aby zademonstrować, jak utworzyć niestandardowe pomocników HTML używanej w ramach widoków MVC. Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość wpisywania niewygodny tagów HTML, że należy wykonać, aby utworzyć standardowej strony HTML.


Celem tego samouczka jest aby zademonstrować, jak utworzyć niestandardowe pomocników HTML używanej w ramach widoków MVC. Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość wpisywania niewygodny tagów HTML, że należy wykonać, aby utworzyć standardowej strony HTML.

W pierwszej części tego samouczka I opisano niektóre z istniejących pomocników HTML dołączonego platformę ASP.NET MVC. Następnie I opisano dwie metody tworzenia niestandardowych pomocników HTML: opisano sposób tworzenia niestandardowych pomocników HTML, tworząc metody statycznej i tworząc — metoda rozszerzenia.

## <a name="understanding-html-helpers"></a>Opis pomocników HTML

Pomocnik kodu HTML jest po prostu metodę, która zwraca wartość typu ciąg. Ten ciąg może reprezentować dowolnego typu zawartości, którą chcesz. Na przykład można użyć pomocników HTML do renderowania standardowych znaczników HTML, takich jak HTML `<input>` i `<img>` tagów. Możesz również użyć pomocników HTML do renderowania zawartości bardziej złożonych, takie jak paska karty lub tabeli HTML danych bazy danych.

Platforma ASP.NET MVC zawiera następujący zestaw standardowych pomocników HTML (nie jest to pełna lista):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Rozważmy na przykład formularz wyświetlania 1. Ten formularz jest renderowany przy użyciu dwóch standardowe pomocników HTML (zobacz rysunek 1). Ten formularz używa `Html.BeginForm()` i `Html.TextBox()` metody pomocnicze do renderowania prostego formularza HTML.


[![Strona jest odwzorowywany z pomocników HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Rysunek 01**: strona odwzorowywany z pomocników HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image3.png))


**1 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Metoda pomocnicza Html.BeginForm() służy do tworzenia HTML otwierający i zamykający `<form>` tagów. Zwróć uwagę, że `Html.BeginForm()` metoda jest wywoływana w za pomocą instrukcji. Za pomocą instrukcji upewnia się, że `<form>` tag zostanie zamknięty na końcu przy użyciu bloku.

Jeśli wolisz, zamiast tworzyć przy użyciu bloku, należy wywołać metodę pomocnika Html.EndForm(), aby zamknąć `<form>` tagu. Użyj innego podejścia do tworzenia, otwierania i zamykania `<form>` tag, który wydaje się najbardziej intuicyjny.

`Html.TextBox()` Metody pomocnicze są używane w wyświetlania 1 do renderowania elementów HTML `<input>` tagów. W przypadku wybrania Wyświetl źródło w przeglądarce, a następnie wyświetlić źródło HTML w wyświetlania 2. Zwróć uwagę, że źródło zawiera standardowych znaczników HTML.

> [!IMPORTANT]
> Zwróć uwagę, że `Html.TextBox()`-HTML pomocnika jest odwzorowywany z `<%= %>` znaczniki zamiast `<% %>` tagów. Jeśli nie zawiera znaku równości, następnie nic nie pobiera renderowane w przeglądarce.

Platforma ASP.NET MVC zawiera niewielki zestaw pomocników. Prawdopodobnie należy rozszerzyć struktura MVC z niestandardowych pomocników HTML. W pozostałej części tego samouczka dowiesz się dwie metody tworzenia niestandardowych pomocników HTML.

**2 — Lista `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Tworzenie pomocników HTML za pomocą metod statycznych

Najprostszym sposobem tworzenia nowego pomocnika kodu HTML jest Tworzenie statycznej metody, która zwraca wartość typu ciąg. Załóżmy na przykład użytkownik chce utworzyć nowe pomocnika kodu HTML, który renderuje HTML `<label>` tagu. Klasa w 2 wyświetlania służy do renderowania `<label>` .

**2 — Lista `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Nie ma nic specjalne informacje o klasie wyświetlania 2. `Label()` Metoda po prostu zwraca ciąg.

Używa zmodyfikowany widok indeksu w 3 wyświetlania `LabelHelper` do renderowania elementów HTML `<label>` tagów. Należy zauważyć, że zawiera on `<%@ imports %>` dyrektywy, który importuje `Application1.Helpers` przestrzeni nazw.

**2 — Lista `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Tworzenie pomocników HTML za pomocą metody rozszerzenia

Jeśli chcesz utworzyć pomocników HTML, które działają podobnie jak standardowy pomocników HTML dołączony platformy ASP.NET MVC, a następnie musisz utworzyć metody rozszerzenia. Metody rozszerzenia umożliwiają dodanie nowych metod do istniejącej klasy. Podczas tworzenia metody pomocnika kodu HTML, możesz dodać nowe metody klasy HtmlHelper reprezentowany przez właściwości Html widoku.

Klasa w 3 wyświetlania dodaje metodę rozszerzenia, aby `HtmlHelper` klasy o nazwie `Label()`. Istnieje kilka rzeczy, które powinny uwagi dotyczące tej klasy. Należy zauważyć, że klasa jest klasie statycznej. Zdefiniuj metodę rozszerzenia o klasie statycznej.

Po drugie, zwróć uwagę, że pierwszy parametr `Label()` metody jest poprzedzony słowa kluczowego `this`. Pierwszy parametr metody rozszerzenia wskazuje klasę, która rozszerza — metoda rozszerzenia.

**3 — lista `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Po utworzeniu metodę rozszerzenia i pomyślnie skompilować aplikację, metody rozszerzenia pojawia się w programie Visual Studio Intellisense podobnie jak wszystkie inne metody klasy (patrz rysunek 2). Jedyna różnica polega na rozszerzenia metod są wyświetlane przy użyciu specjalnego symbolu obok nich (ikonę strzałki w dół).


[![Przy użyciu metody rozszerzenia Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Rysunek 02**: przy użyciu metody rozszerzenia Html.Label() ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image6.png))


Zmodyfikowany widok indeksu listę 4 używa metody rozszerzenia Html.Label() do renderowania, wszystkie jego `<label>` tagów.

**Wyświetlanie listy 4. `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono dwie metody tworzenia niestandardowych pomocników HTML. Po pierwsze, przedstawiono sposób tworzenia niestandardowego `Label()` pomocnika kodu HTML, tworząc statycznej metody, która zwraca wartość typu ciąg. Następnie przedstawiono sposób tworzenia niestandardowego `Label()` metody pomocnika kodu HTML, tworząc metody rozszerzenia na `HtmlHelper` klasy.

W tym samouczku I koncentruje się na tworzeniu bardzo prosta metoda pomocnika kodu HTML. Należy pamiętać, że pomocnika kodu HTML, może być jako skomplikowane, można dowolnie. Można tworzyć pomocników HTML, który renderowania zawartości zaawansowanych, takich jak widok drzewa, menu lub tabele bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-views-overview-cs.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
