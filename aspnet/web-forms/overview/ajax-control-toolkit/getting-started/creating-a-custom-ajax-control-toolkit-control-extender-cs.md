---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Tworzenie niestandardowych AJAX formantu rozszerzeń formantu Toolkit (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Niestandardowych rozszerzeń umożliwiają dostosowywanie i rozszerzanie możliwości kontrolki ASP.NET, bez konieczności tworzenia nowych klas.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: dc058d1d19df880109352caf2dc7d1860121a104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Tworzenie rozszerzeń formantu Toolkit kontroli AJAX niestandardowych (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Niestandardowych rozszerzeń umożliwiają dostosowywanie i rozszerzanie możliwości kontrolki ASP.NET, bez konieczności tworzenia nowych klas.


W tym samouczku Dowiedz się tworzenie niestandardowych rozszerzeń formantu Toolkit kontroli AJAX. Utworzymy prostą, ale przydatne, nowych rozszerzeń, który zmienia stan przycisku wyłączonego na włączony podczas wpisywania tekstu w pole tekstowe. Po przeczytaniu tego samouczka, można rozszerzyć zestawie narzędzi programu ASP.NET AJAX z własnych rozszerzeń formantu.

Można utworzyć przy użyciu programu Visual Studio lub Visual Web Developer rozszerzeń formantu niestandardowego (Upewnij się, że masz najnowszą wersję programu Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Omówienie rozszerzeń DisabledButton

Nasze nowych rozszerzeń formantu nosi nazwę DisabledButton rozszerzeń. Tego rozszerzenia ma trzy właściwości:

- Targetcontrolid równa - pole tekstowe, rozszerzający formantu.
- TargetButtonIID - przycisku, który jest wyłączone lub włączone.
- DisabledText - tekst wyświetlany na przycisku. Po rozpoczęciu wprowadzania, przycisk wyświetla wartość właściwości tekst przycisku.

Utworzenie punktu zaczepienia rozszerzeń DisabledButton do formantu TextBox i przycisk. Przed wprowadzeniem tekst przycisku jest wyłączona, a pole tekstowe i przycisk wyglądać następująco:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Po rozpoczęciu wpisywania tekstu, ten przycisk jest włączony i pole tekstowe i przycisk wyglądać następująco:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Aby utworzyć naszych rozszerzeń formantu, należy utworzyć trzy następujące pliki:

- DisabledButtonExtender.cs — ten plik jest klasy formantu po stronie serwera, która będzie zarządzać tworzenia urządzenia extender i umożliwiają skonfigurowanie właściwości w czasie projektowania. Definiuje właściwości, które można ustawić na Twoje rozszerzenie. Te właściwości są dostępne za pośrednictwem kodu i w czasie projektowania i odpowiada właściwości zdefiniowane w pliku DisableButtonBehavior.js.
- DisabledButtonBehavior.js — Ten plik jest, gdzie zostaną dodane wszystkie logiki skryptu klienta.
- DisabledButtonDesigner.cs — ta klasa umożliwia funkcjonalność czasu projektowania. Ta klasa jest konieczne, jeśli chcesz, rozszerzający formantu do poprawnego działania z programem Visual Studio/Visual Web Developer Designer.

Dlatego rozszerzeń formantu składa się z formantu po stronie serwera, zachowanie po stronie klienta i po stronie serwera klasy projektanta. Jak utworzyć wszystkie trzy pliki w poniższych sekcjach.

## <a name="creating-the-custom-extender-website-and-project"></a>Tworzenie rozszerzeń niestandardową witrynę sieci Web i projektu

Pierwszym krokiem jest utworzenie projektu biblioteki klas i witryny sieci Web w programie Visual Studio/Visual Web Developer. Firma Microsoft ll tworzenia niestandardowych rozszerzeń w projektu biblioteki klas i testowania niestandardowych rozszerzeń w witrynie internetowej.

Let s uruchomić z poziomu witryny sieci Web. Wykonaj następujące kroki, aby utworzyć witrynę sieci Web:

1. Wybierz opcję menu **plik, nowej witryny sieci Web**.
2. Wybierz **witryny sieci Web ASP.NET** szablonu.
3. Nazwa nowej witryny sieci Web *Website1*.
4. Kliknij przycisk **OK** przycisku.

Następnie należy utworzyć projektu biblioteki klas, zawierające kod dla rozszerzeń formantu:

1. Wybierz opcję menu **pliku, Dodaj nowy projekt**.
2. Wybierz **biblioteki klas** szablonu.
3. Nazwa nowej biblioteki klasy o nazwie **CustomExtenders**.
4. Kliknij przycisk **OK** przycisku.

Po wykonaniu tych kroków okna Eksploratora rozwiązań powinien wyglądać rysunek 1.


[![Rozwiązania z witryny sieci Web i klasa projektu biblioteki](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Rysunek 01**: rozwiązania z witryny sieci Web i klasa projektu biblioteki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Następnie należy dodać wszystkie niezbędne odwołania zestawów do projektu biblioteki klas:

1. Kliknij prawym przyciskiem myszy projekt CustomExtenders i wybierz opcję menu **Dodaj odwołanie**.
2. Wybierz kartę .NET.
3. Dodaj odwołania do następujących zestawów:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Wybierz kartę przeglądania.
5. Dodaj odwołanie do zestawu AjaxControlToolkit.dll. Ten zestaw znajduje się w folderze, którego pobrano Toolkit kontroli AJAX.

Po wykonaniu tych kroków folderze odwołania do projektu biblioteki klas powinien wyglądać na rysunku 2.


[![Odwołania do folderu z odwołania wymagane](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Rysunek 02**: folder odwołań z odwołania wymagane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Tworzenie rozszerzeń formantu niestandardowego

Teraz, gdy mamy naszych biblioteki klas, możemy rozpocząć tworzenie naszych formant rozszerzający. Let s rozpoczynać kości bare niestandardowe rozszerzenie klasy formantu (patrz lista 1).

**Wyświetlanie listy 1 - MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Istnieje kilka kwestii, które można zauważyć, że informacje o klasie rozszerzeń formantu w wyświetlania 1. Najpierw należy zauważyć, że klasa dziedziczy z klasy podstawowej ExtenderControlBase. Wszystkie formanty rozszerzające AJAX Toolkit formant pochodzi od tej klasy podstawowej. Na przykład klasa podstawowa zawiera właściwość TargetID, która jest wymagana właściwość co rozszerzeń formantu.

Następnie należy zauważyć, że klasa zawiera następujące atrybuty powiązane z skrypt po stronie klienta:

- Widok — powoduje, że plik do załączenia jako osadzonego zasobu w zestawie.
- ClientScriptResource — powoduje, że zasób Skrypt można pobrać z zestawu.

Atrybut widok jest używany do osadzania plik MyControlBehavior.js JavaScript w zestawie podczas kompilowania rozszerzeń niestandardowych. Atrybut ClientScriptResource służy do pobierania skryptu MyControlBehavior.js z zestawu, gdy niestandardowych rozszerzeń jest używany na stronie sieci web.


Aby atrybutów zasobów sieci Web i ClientScriptResource działała należy skompilować plik JavaScript jako osadzony zasób. Wybierz plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisuje wartość *osadzonego zasobu* do **Akcja kompilacji** właściwości.


Zwróć uwagę, że formant rozszerzający obejmuje również atrybutu element TargetControlType. Ten atrybut służy do określania typu formantu, który zostanie przedłużony rozszerzeń formantu. W przypadku wyświetlania 1 rozszerzający kontroli służy do rozszerzyć pole tekstowe.

Warto zauważyć, że niestandardowe rozszerzenie zawiera właściwość o nazwie MyProperty. Właściwość jest oznaczona atrybutem ExtenderControlProperty. Metody GetPropertyValue() i SetPropertyValue() są używane do przekazywania wartości właściwości z rozszerzeń po stronie serwera kontroli zachowania po stronie klienta.

Let s Przejdź dalej i implementowania kodu dla naszych DisabledButton rozszerzenia. Kod dla tego rozszerzenia można znaleźć w wyświetlania 2.

**Wyświetlanie listy 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Extender DisabledButton wyświetlania 2 ma dwie właściwości o nazwie TargetButtonID i DisabledText. IDReferenceProperty zastosować do właściwości TargetButtonID uniemożliwia innym niż identyfikator formantu przycisku przypisanie do tej właściwości.

Atrybuty widok i ClientScriptResource skojarzyć zachowanie klienta znajduje się w pliku o nazwie DisabledButtonBehavior.js z tego rozszerzenia. Omówiono ten plik JavaScript w następnej sekcji.

## <a name="creating-the-custom-extender-behavior"></a>Tworzenie niestandardowych rozszerzeń zachowanie

Składnik po stronie klienta rozszerzeń formantu jest nazywany zachowanie. Rzeczywiste logikę wyłączenie i włączenie przycisku znajduje się w zachowaniu DisabledButton. Kod JavaScript zachowanie jest dołączona do wyświetlania 3.

**Wyświetlanie listy 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Plik JavaScript w 3 lista zawiera klasę klienta o nazwie DisabledButtonBehavior. Ta klasa, takie jak jego dwie po stronie serwera zawiera dwie właściwości o nazwie TargetButtonID i uzyskać DisabledText, którego można uzyskiwać dostęp za pomocą\_TargetButtonID/set\_TargetButtonID i uzyskać\_DisabledText/set\_ DisabledText.

Metodę initialize() kojarzy obsługi zdarzeń keyup z elementem docelowym zachowania. Wykonuje obsługi keyup zawsze można wpisać w pole tekstowe skojarzone z tym działaniem literą. Program obsługi keyup Włącza lub wyłącza przycisk w zależności od tego, czy pole tekstowe skojarzonych z zachowaniem zawiera dowolny tekst.

Należy pamiętać, że należy skompilować plik JavaScript w wyświetlania 3 jako osadzony zasób. Wybierz plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisuje wartość *osadzonego zasobu* do **Akcja kompilacji** właściwości (patrz rysunek 3). Ta opcja jest dostępna w programie Visual Studio i Visual Web Developer.


[![Dodawanie pliku JavaScript jako osadzony zasób](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Rysunek 03**: Dodawanie pliku JavaScript jako osadzony zasób ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Tworzenie niestandardowych rozszerzeń projektanta

Brak jednego ostatniego klasy, która należy utworzyć w celu ukończenia naszego rozszerzeń. Należy utworzyć designer klasy w listę 4. Ta klasa jest wymagana do udostępnienia, rozszerzający zadziała poprawnie przy użyciu projektanta Visual Studio/Visual Web Developer.

**Wyświetlanie listy 4 - DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Projektant w listę 4 należy skojarzyć z rozszerzeń DisabledButton z atrybutem projektanta. Należy zastosować atrybut projektanta do klasy DisabledButtonExtender następująco:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Przy użyciu niestandardowych rozszerzeń

Teraz, gdy Tworzenie rozszerzeń formantu DisabledButton została zakończona, nadszedł czas na używany w naszej witrynie sieci Web programu ASP.NET. Najpierw należy dodać niestandardowe rozszerzenie do przybornika. Wykonaj następujące kroki:

1. Otwórz stronę ASP.NET, klikając dwukrotnie strony w oknie Eksploratora rozwiązań.
2. Kliknij prawym przyciskiem myszy przybornika i wybierz opcję menu **wybierz elementy**.
3. W oknie dialogowym Wybierz elementy przybornika przejdź do zestawu CustomExtenders.dll.
4. Kliknij przycisk **OK** przycisk, aby zamknąć okno dialogowe.

Po wykonaniu tych kroków rozszerzeń formantu DisabledButton powinny być wyświetlane w przyborniku (patrz rysunek 4).


[![DisabledButton w przyborniku](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Rysunek 04**: DisabledButton w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Następnie należy utworzyć nową stronę ASP.NET. Wykonaj następujące kroki:

1. Utwórz nową stronę ASP.NET o nazwie ShowDisabledButton.aspx.
2. Przeciągnij element ScriptManager na stronie.
3. Przeciągnij kontrolki pola tekstowego na stronie.
4. Przeciągnij formant przycisku na stronie.
5. W oknie Właściwości zmień wartość właściwości przycisk identyfikator na wartość <em>btnSave</em> i wartość właściwości Text *zapisać\**.
  

Utworzono stronę z formantu standardowego pola tekstowego ASP.NET i przycisk.

Następnie należy rozszerzyć formantu TextBox z rozszerzeń DisabledButton:

1. Wybierz **Dodawanie rozszerzeń** zadań opcję, aby otworzyć okno dialogowe Kreator Extender (patrz rysunek 5). Zwróć uwagę, że okno dialogowe zawiera naszych niestandardowych rozszerzeń DisabledButton.
2. Wybierz rozszerzenie DisabledButton i kliknij przycisk **OK** przycisku.


[![Okno dialogowe Kreator rozszerzeń](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Rysunek 05**: okno dialogowe Kreator rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Firma Microsoft wreszcie, ustaw właściwości rozszerzeń DisabledButton. Właściwości rozszerzeń DisabledButton można modyfikować, zmieniając właściwości formantu TextBox:

1. Wybierz pole tekstowe w projektancie.
2. W oknie właściwości rozwiń węzeł Extender (patrz rysunek 6).
3. Przypisuje wartość *zapisać* DisabledText właściwości i wartość *btnSave* TargetButtonID właściwości.


[![Ustawianie właściwości rozszerzeń](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Rysunek 06**: Ustawianie właściwości rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Po uruchomieniu strony (za pomocą F5) formantu przycisku początkowo jest wyłączona. Jak w przypadku uruchomienia wprowadzanie tekstu w polu tekstowym, przycisk formant jest włączony (patrz rysunek 7).


[![Rozszerzenie DisabledButton w akcji](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Rysunek 07**: DisabledButton rozszerzeń w akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wyjaśniają, jak można rozszerzyć Toolkit kontroli AJAX z formanty rozszerzające niestandardowych. W tym samouczku utworzyliśmy proste rozszerzeń formantu DisabledButton. Tworząc klasę DisabledButtonExtender, zachowanie DisabledButtonBehavior JavaScript i klasa DisabledButtonDesigner zaimplementowano tego rozszerzenia. Zbiór podobne kroki należy wykonać zawsze, gdy Tworzenie rozszerzeń kontrolki niestandardowej.

> [!div class="step-by-step"]
> [Poprzednie](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [dalej](get-started-with-the-ajax-control-toolkit-vb.md)
