---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Za pomocą rozszerzenie kontrolki ColorPicker (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: ColorPicker jest rozszerzeń ASP.NET AJAX, który udostępnia funkcjonalność pobrania kolor po stronie klienta za pomocą interfejsu użytkownika w kontrolce popup. Będzie można dołączyć do dowolnej platformy ASP.NET...
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: e7375dcfc354e931f30d2250081f424bd2149953
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828134"
---
<a name="using-the-colorpicker-control-extender-vb"></a>Za pomocą rozszerzenie kontrolki ColorPicker (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> ColorPicker jest rozszerzeń ASP.NET AJAX, który udostępnia funkcjonalność pobrania kolor po stronie klienta za pomocą interfejsu użytkownika w kontrolce popup. Będzie można dołączyć do dowolnej kontrolki ASP.NET TextBox. Go.


Celem tego samouczka jest wyjaśniają, jak można użyć rozszerzenie kontrolki Toolkit ColorPicker kontrolka AJAX. Rozszerzenie kontrolki ColorPicker wyświetla menu podręczne okno dialogowe, umożliwiające wybierz kolor. ColorPicker jest przydatne, gdy chcesz zapewnić intuicyjnego interfejsu użytkownika dla użytkownika, aby wybrać kolor.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Rozszerzanie Formant TextBox z rozszerzenie kontrolki ColorPicker

Wyobraź sobie, na przykład chcesz utworzyć witrynę sieci Web, który umożliwia tworzenie dostosowanych wizytówki przez osoby odwiedzające. Osoby odwiedzające można wprowadzić tekst wizytówki i wybierz kolor. Strony ASP.NET w ofercie 1 zawiera dwie kontrolki TextBox o nazwie txtCardText i txtCardColor. Gdy prześlesz formularz, są wyświetlane wybrane wartości (patrz rysunek 1).


[![Prosty formularz do tworzenia wizytówkę](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Rysunek 01**: prosty formularz do tworzenia wizytówki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image2.png))


**Wyświetlanie listy 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Formularz w ofercie 1 działa, ale nie udostępnia doskonałe środowisko użytkownika. Użytkownik będzie musiał wpisać koloru w polu tekstowym. Jeśli użytkownik chce wyspecjalizowane kolor — na przykład tylko odpowiednie odcień zielony PLA -, a następnie użytkownik musi ustalić kod HTML koloru bez pomocy.

Rozszerzenie kontrolki ColorPicker służy do tworzenia lepszego środowiska użytkownika. ColorPicker wyświetla okno dialogowe kolorów, po przeniesieniu fokusu do kontrolki TextBox (patrz rysunek 2).


[![Rozszerzenie kontrolki ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Rysunek 02**: rozszerzenie kontrolki ColorPicker ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image4.png))


Należy wykonać dwa kroki, aby rozszerzenie kontrolki ColorPicker za pomocą formularza w ofercie 1:

1. Dodawanie formantu ScriptManager do strony
2. Dodaj rozszerzenie kontrolki ColorPicker do strony

Zanim użyjesz ColorPicker, należy dodać Menedżera skryptów do strony. Dobrym miejscem do dodania funkcja ScriptManager jest bezpośrednio poniżej po stronie serwera otwierania &lt;formularza&gt; tagu. Funkcja ScriptManager można przeciągnąć na stronę z przybornika (funkcja ScriptManager znajduje się na karcie rozszerzenia AJAX). Alternatywnie można wpisać następującego tagu w widoku źródła pod otwierający tag formularza po stronie serwera:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

Najprostszym sposobem na stronie Dodaj rozszerzenie kontrolki ColorPicker jest w widoku Projekt. Jeśli wskaźnik myszy nad txtCardColor pola tekstowego, opcja inteligentne zadań jest wyświetlana, zapewniającą można dodać urządzenia extender (zobacz rysunek 3). W przypadku wybrania tej opcji, zostanie wyświetlony Kreator urządzenia Extender, (zobacz rysunek 4).


[![Dodawanie urządzenia extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Rysunek 03**: Dodawanie rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Wybieranie rozszerzenia kontrolki zestawu narzędzi za pomocą kreatora rozszerzeń](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Rysunek 04**: Wybieranie rozszerzenia kontrolki zestawu narzędzi za pomocą kreatora rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image8.png))


Można wybrać rozszerzenie ColorPicker rozszerzenie txtCardColor pola tekstowego przy użyciu rozszerzeń ColorPicker. Kliknij przycisk OK, aby zamknąć okno dialogowe.

Po wprowadzeniu tych zmian, źródła dla strony wyglądają jak lista 2.

**Wyświetlanie listy 2 - CreateCard.aspx (z ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Należy zauważyć, że strona zawiera teraz formant ColorPickerExtender, który pojawia się bezpośrednio pod txtCardColor formant pola tekstowego. Kontrolka ColorPickerExtender rozszerza formant txtCardColor tak, aby wyświetlał okna dialogowego selektora kolorów.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Za pomocą przycisku można uruchomić okna dialogowego selektora kolorów

Rozszerzenie ColorPicker obsługuje następujące właściwości:

- PopupButtonId — identyfikator przycisk na stronie, która powoduje, że są wyświetlane okno dialogowe próbnika kolorów.
- PopupPosition — pozycja, względem formantu docelowego, okna dialogowego selektora kolorów. Możliwe wartości to bezwzględną, Centrum, BottomLeft, BottomRight, TopLeft, TopRight i po lewej stronie (wartość domyślna to BottomLeft).
- SampleControlId — identyfikator formantu, który wyświetla wybrany kolor.
- SelectedColor — kolor początkowy wybranych przez ColorPicker.

Aby dostosować sposób wyświetlania okna dialogowego selektora kolorów i sposób wyświetlania kolorów, można użyć tych właściwości. Na stronie w ofercie 3 ilustruje, jak skorzystać z kilku z tych właściwości.

**Wyświetlanie listy 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

Na stronie w ofercie 3 obejmuje wybierz kolor przycisku (zobacz rysunek 5). Po kliknięciu tego przycisku, powyżej pola tekstowego pojawi się okno dialogowe selektora kolorów. Jeśli wybierzesz kolor z poziomu okna dialogowego wybrany kolor pojawia się jako kolor tła lblSample formant etykiety.

Właściwość ColorPicker PopupButtonID jest używana do kojarzenia przycisk wyboru koloru z rozszerzeń ColorPicker. Podczas podawania wartości dla właściwości PopupButtonID okna dialogowego selektora kolorów nie jest już wyświetlany formantu docelowego po ustawieniu fokusu. Należy kliknąć przycisk aby wyświetlić okno dialogowe.

Właściwość SampleControlID jest używana do kojarzenia kontrolkę wyświetlającą wybranego koloru z ColorPicker. ColorPicker zmienia kolor tła tego formantu do aktualnie wybranego koloru.


[![Wyświetlanie okna dialogowego selektora kolorów przy użyciu przycisku](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Rysunek 05**: wyświetlanie okna dialogowego selektora kolorów przy użyciu przycisku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób użyć rozszerzenie kontrolki ColorPicker, aby wyświetlić okno podręczne okno dialogowe selektora kolorów. Najpierw zbadaliśmy się, jak można wyświetlić okno dialogowe, gdy fokus zostanie przeniesiony do kontrolki TextBox. Następnie pokazaliśmy ci, jak utworzyć przycisk, który wyświetla okno dialogowe selektora kolorów, po kliknięciu przycisku.

> [!div class="step-by-step"]
> [Poprzednie](using-the-colorpicker-control-extender-cs.md)
