---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Przy użyciu rozszerzeń formantu ColorPicker (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: ColorPicker jest extender ASP.NET AJAX, która udostępnia funkcje pobrania kolor po stronie klienta przy użyciu interfejsu użytkownika w formancie menu podręczne. Będzie można dołączyć do dowolnego ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d44fc81305e668b545246cf044dce275563d81a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-c"></a>Przy użyciu rozszerzeń formantu ColorPicker (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> ColorPicker jest extender ASP.NET AJAX, która udostępnia funkcje pobrania kolor po stronie klienta przy użyciu interfejsu użytkownika w formancie menu podręczne. Może zostać dołączona do żadnego formantu ASP.NET TextBox. Go.


Celem tego samouczka jest wyjaśnienie, jak używasz AJAX kontroli zestawu narzędzi ColorPicker rozszerzeń formantu. Rozszerzeń formantu ColorPicker wyświetla okno dialogowe menu podręczne, które można wybrać kolor. ColorPicker jest przydatne, gdy chcesz zapewnić intuicyjnego interfejsu użytkownika dla użytkownika wybrać kolor.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Rozszerzenie kontrolki pola tekstowego z rozszerzeń formantu ColorPicker

Załóżmy, że chcesz utworzyć witrynę sieci Web, który umożliwia tworzenie dostosowanych kart biznesowej osoby odwiedzające. Osoby odwiedzające można wprowadzić tekst kart biznesowej i wybierz kolor. Strony ASP.NET w 1 Lista zawiera dwa kontrolki TextBox o nazwie txtCardText i txtCardColor. Po przesłaniu formularza, wybranych wartości są wyświetlane (zobacz rysunek 1).


[![Prosty formularz służący do tworzenia kart biznesowej](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Rysunek 01**: prosty formularz służący do tworzenia kart biznesowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Wyświetlanie listy 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Formularz działa wyświetlania 1, ale nie zapewnia obsługi użytkowników. Użytkownik będzie musiał wpisać koloru w polu tekstowym. Jeśli użytkownik chce specjalne kolor — na przykład tylko prawo odcień zielony pea -, a następnie użytkownik musi ustalić kod HTML koloru bez pomocy.

Rozszerzenie kontrolki ColorPicker służy do tworzenia lepsze środowisko pracy użytkownika. ColorPicker wyświetla okno dialogowe kolorów, gdy Przenieś fokus do formantu TextBox (patrz rysunek 2).


[![Rozszerzenie kontrolki ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Rysunek 02**: rozszerzeń formantu ColorPicker ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image4.png))


Trzeba wykonać rozszerzeń formantu ColorPicker za pomocą formularza w 1 wyświetlania wykonać dwie czynności:

1. Dodawanie formantu ScriptManager na stronie
2. Na stronie Dodaj ColorPicker rozszerzeń formantu

Przed użyciem ColorPicker, należy dodać element ScriptManager do strony. Dobrym miejscem, aby dodać element ScriptManager jest bezpośrednio pod serwerowe otwierania &lt;formularza&gt; tagu. Z przybornika (ScriptManager znajduje się na karcie rozszerzenia AJAX) można przeciągnąć element ScriptManager na stronę. Alternatywnie możesz wpisać następującego tagu w widoku źródła poniżej otwierający tag formularza po stronie serwera:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

Najprostszym sposobem na stronie Dodaj ColorPicker rozszerzeń formantu jest w widoku Projekt. Jeśli umieść kursor nad txtCardColor pole tekstowe, opcja inteligentne zadań pojawi się zapewniającą można dodać moduł rozszerzający (patrz rysunek 3). W przypadku wybrania tej opcji, zostanie wyświetlony Kreator rozszerzeń, (patrz rysunek 4).


[![Dodawanie rozszerzeń](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Rysunek 03**: Dodawanie rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Wybieranie rozszerzeń formantu przy użyciu kreatora rozszerzeń](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Rysunek 04**: Wybieranie rozszerzeń formantu przy użyciu kreatora rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image8.png))


Można wybrać rozszerzeń ColorPicker rozszerzenie txtCardColor pole tekstowe z rozszerzeń ColorPicker. Kliknij przycisk OK, aby zamknąć okno dialogowe.

Po wprowadzeniu tych zmian źródła dla strony wygląda jak lista 2.

Wyświetlanie listy 2 - CreateCard.aspx (z ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Zwróć uwagę, czy strona zawiera teraz ColorPickerExtender formant, który pojawi się bezpośrednio pod txtCardColor formantu TextBox. Formant ColorPickerExtender rozszerza kontroli txtCardColor tak, aby go Wyświetla okno dialogowe selektora kolorów.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Aby uruchomić okno dialogowe selektora kolorów za pomocą przycisku

Rozszerzenie ColorPicker obsługuje następujące właściwości:

- PopupButtonId — identyfikator przycisku na stronie powoduje wyświetlać okno dialogowe selektora kolorów.
- PopupPosition - pozycji, względem formantu docelowego okna dialogowego selektora kolorów. Możliwe wartości to bezwzględne, Centrum, BottomLeft, BottomRight, TopLeft, TopRight, prawo i lewej strony (wartość domyślna to BottomLeft).
- SampleControlId — identyfikator formantu, który wyświetla wybranego koloru.
- SelectedColor - początkowy kolor wybrany przez ColorPicker.

Aby dostosować sposób wyświetlania okna dialogowego selektora kolorów i sposób wyświetlania wybranego koloru, można użyć tych właściwości. Strona wyświetlania 3 ilustruje sposób korzystania z niektóre z tych właściwości.

**Wyświetlanie listy 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

Strona wyświetlania 3 zawiera wybierz kolor przycisku (patrz rysunek 5). Po kliknięciu tego przycisku okna dialogowego selektora kolorów pojawia się powyżej pola tekstowego. W przypadku wybrania koloru z okna dialogowego kolorów pojawia się jako kolor tła lblSample formantu etykiety.

Właściwość ColorPicker PopupButtonID jest używana do skojarzenia z rozszerzeń ColorPicker przycisku Wybierz kolor. Jeśli zostanie podana wartość właściwości PopupButtonID okna dialogowego selektora kolorów nie będzie widoczny po aktywowaniu formantu docelowego. Musi kliknąć przycisk, aby wyświetlić okno dialogowe.

Właściwość SampleControlID jest używana do skojarzenia kontrolkę wyświetlającą wybranego koloru z ColorPicker. ColorPicker zmienia kolor tła tego formantu do aktualnie wybranego koloru.


[![Wyświetlanie okna dialogowego selektora kolorów z przyciskiem](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Rysunek 05**: wyświetlanie okna dialogowego selektora kolorów z przyciskiem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób rozszerzeń formantu ColorPicker umożliwia wyświetlanie podręcznego okna dialogowego selektora kolorów. Po pierwsze firma Microsoft zbadane, sposób wyświetlania okna dialogowego, gdy fokus zostanie przeniesiony do kontrolki pola tekstowego. Następnie przedstawiono sposób tworzenia przycisku, który powoduje wyświetlenie okna dialogowego selektora kolorów, po kliknięciu przycisku.

> [!div class="step-by-step"]
> [Next](using-the-colorpicker-control-extender-vb.md)
