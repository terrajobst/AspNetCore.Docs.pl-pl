---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Tworzenie niestandardowych pomocników HTML (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC. Dzięki wykorzystaniu pomocnika kodu HTML...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839412"
---
<a name="creating-custom-html-helpers-c"></a>Tworzenie niestandardowych pomocników HTML (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC. Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość tedious wpisywania tagów HTML, czy należy wykonać, aby utworzyć standardowej strony HTML.


Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC. Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość tedious wpisywania tagów HTML, czy należy wykonać, aby utworzyć standardowej strony HTML.

W pierwszej części tego samouczka I opisano niektóre istniejące pomocników HTML dołączone do struktury ASP.NET MVC. Następnie na opisują jest tworzenie niestandardowych pomocników HTML na dwa sposoby: opisano sposób tworzenia niestandardowych pomocników HTML, tworząc metody statycznej i tworząc metody rozszerzenia.

## <a name="understanding-html-helpers"></a>Opis pomocników HTML

Pomocnik kodu HTML jest po prostu metody, która zwraca wartość typu ciąg. Ciąg może reprezentować dowolnego typu zawartości. Na przykład, można użyć pomocników HTML do renderowania standardowych znaczników HTML, takich jak HTML `<input>` i `<img>` tagów. Możesz również użyć pomocników HTML do renderowania zawartości bardziej złożonych, takie jak pasek kart lub tabeli HTML danych bazy danych.

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

Rozważmy na przykład formularz w ofercie 1. Ta forma jest renderowany przy pomocy dwóch standardowa pomocników HTML (patrz rysunek 1). Ten formularz używa `Html.BeginForm()` i `Html.TextBox()` metody pomocnika do renderowania prostego formularza HTML.


[![Strony renderowane przy użyciu pomocników HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Rysunek 01**: strony renderowane przy użyciu pomocników HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image3.png))


**1 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Metoda pomocnika Html.BeginForm() służy do utworzenia pliku HTML otwierający i zamykający `<form>` tagów. Należy zauważyć, że `Html.BeginForm()` metoda jest wywoływana w obrębie za pomocą instrukcji. Za pomocą instrukcji zapewnia, że `<form>` tag zostanie zamknięty na końcu używając bloku.

Jeśli wolisz, zamiast tworzyć za pomocą bloku, można wywołać metodę pomocnika Html.EndForm(), aby zamknąć `<form>` tagu. Użyj jednego z tych podejście do tworzenia otwierający i zamykający `<form>` tag, który wydaje się być najbardziej intuicyjnym do Ciebie.

`Html.TextBox()` Metody pomocnika służą do renderowania elementów HTML w ofercie 1 `<input>` tagów. Jeśli wybierzesz Wyświetl źródło w przeglądarce zobaczysz do źródła HTML w ofercie 2. Należy zauważyć, że źródłowa zawiera standardowych znaczników HTML.

> [!IMPORTANT]
> Należy zauważyć, że `Html.TextBox()`— HTML pomocnika jest renderowany przy użyciu `<%= %>` tagów zamiast `<% %>` tagów. Jeśli nie dołączysz znaku równości, następnie nic nie pobiera renderowane w przeglądarce.

Platforma ASP.NET MVC zawiera niewielki zestaw pomocników. Prawdopodobnie należy rozszerzyć platformę MVC za pomocą niestandardowych pomocników HTML. W pozostałej części tego samouczka dowiesz się, tworzenie niestandardowych pomocników HTML na dwa sposoby.

**2 — Lista `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Tworzenie pomocników HTML za pomocą metod statycznych

Najprostszym sposobem utworzenia nowego pomocnika kodu HTML jest Tworzenie statycznej metody, która zwraca wartość typu ciąg. Wyobraź sobie, na przykład zdecydujesz utworzyć nowego pomocnika HTML, który powoduje wyświetlenie kodu HTML `<label>` tagu. Można użyć klasy w ofercie 2 do renderowania `<label>` .

**2 — Lista `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Nie ma nic specjalnego informacje o klasie w ofercie 2. `Label()` Metoda po prostu zwraca ciąg.

Używa zmodyfikowanego widoku indeksu w ofercie 3 `LabelHelper` do renderowania elementów HTML `<label>` tagów. Należy zauważyć, że widok zawiera `<%@ imports %>` dyrektywę, który importuje `Application1.Helpers` przestrzeni nazw.

**2 — Lista `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Tworzenie pomocników HTML przy użyciu metody rozszerzenia

Jeśli chcesz tworzyć pomocników HTML, które działają podobnie, takich jak standardowy pomocników HTML zawarte w platformę ASP.NET MVC, a następnie należy utworzyć metody rozszerzenia. Metody rozszerzające umożliwiają dodawanie nowych metod do istniejącej klasy. Podczas tworzenia metodę pomocnika kodu HTML, należy dodać nowe metody do klasy HtmlHelper, reprezentowane przez właściwości Html widoku.

Ta klasa w ofercie 3 dodaje metodę rozszerzenia, aby `HtmlHelper` klasę o nazwie `Label()`. Istnieje kilka rzeczy, które użytkownik zauważy o tej klasie. Najpierw zwróć uwagę, że klasa jest klasą statyczną. Należy zdefiniować metodę rozszerzającą o klasie statycznej.

Po drugie, zwróć uwagę, że pierwszy parametr `Label()` metody jest poprzedzone słowem kluczowym `this`. Pierwszy parametr metody rozszerzenia wskazuje klasę, która rozszerza metoda rozszerzenia.

**3 — lista `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Po utworzeniu metodę rozszerzenia, a następnie skompilować aplikację pomyślnym, metoda rozszerzenia pojawia się w Visual Studio technologii Intellisense, podobnie jak wszystkie inne metody klasy (patrz rysunek 2). Jedyną różnicą jest to rozszerzenie, które metody są wyświetlane z symbolem specjalne obok nich (ikona strzałki w dół).


[![Przy użyciu metody rozszerzenia Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Rysunek 02**: przy użyciu metody rozszerzenia Html.Label() ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-cs/_static/image6.png))


Zmodyfikowany widok indeksu w ofercie 4 używa Html.Label() metody rozszerzenia do renderowania, wszystkie jego `<label>` tagów.

**4 — lista `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono tworzenie niestandardowych pomocników HTML na dwa sposoby. Po pierwsze, wiesz, jak utworzyć niestandardową `Label()` pomocnika kodu HTML, tworząc statycznej metody, która zwraca wartość typu ciąg. Następnie pokazano, jak utworzyć niestandardową `Label()` metody pomocnika kodu HTML, tworząc metodę rozszerzającą o `HtmlHelper` klasy.

W tym samouczku I koncentruje się na tworzeniu bardzo prostą metodę pomocnika kodu HTML. Należy pamiętać, że pomocnika kodu HTML, może być tak skomplikowane, jak chcesz. Można tworzyć pomocników HTML, renderowanie sformatowanej zawartości, takich jak widoki drzewa i menu tabel bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-views-overview-cs.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
