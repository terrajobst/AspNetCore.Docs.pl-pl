---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Tworzenie niestandardowego interfejsu AJAX formantu rozszerzenia kontrolki Toolkit (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Niestandardowe rozszerzenia umożliwiają dostosowywanie i rozszerzanie możliwości kontrolek ASP.NET bez konieczności tworzenia nowych klas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f0cbee47b541e31f3e9f01e42afeabcd7b9769f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756335"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Tworzenie niestandardowego rozszerzenia AJAX Control Toolkit kontrolki (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Niestandardowe rozszerzenia umożliwiają dostosowywanie i rozszerzanie możliwości kontrolek ASP.NET bez konieczności tworzenia nowych klas.


W tym samouczku dowiesz się, jak utworzyć niestandardowego rozszerzenia kontrolki zestawu narzędzi AJAX Control Toolkit. Utworzymy prostą, ale przydatne, nowych rozszerzeń, który zmienia stan przycisku z wyłączonego na włączony podczas wpisywania tekstu w polu tekstowym. Po przeczytaniu tego samouczka, można rozszerzyć z zestawu narzędzi AJAX ASP.NET przy użyciu własnych rozszerzeń.

Można tworzyć niestandardowych kontrolek przy użyciu programu Visual Studio lub Visual Web Developer (Upewnij się, że masz najnowszą wersję programu Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Omówienie rozszerzeń DisabledButton

Nasze nowe rozszerzenie kontrolki nosi nazwę rozszerzenia DisabledButton. Tego rozszerzenia ma trzy właściwości:

- TargetControlID — pole tekstowe, które rozszerza formant.
- TargetButtonIID — przycisk, który zostało wyłączone lub włączone.
- DisabledText — tekst, który początkowo jest wyświetlana na przycisku. Gdy zaczniesz pisać, przycisk powoduje wyświetlenie wartości właściwości tekst przycisku.

Utworzenie punktu zaczepienia extender DisabledButton do kontrolki pola tekstowego i przycisku. Przed wpisaniem tekstu przycisk jest wyłączony i pole tekstowe i przycisk wyglądać następująco:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Po rozpoczęciu wpisywania tekstu, ten przycisk jest włączony, i pole tekstowe i przycisk wyglądać następująco:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


Aby utworzyć naszego rozszerzenia kontrolki zestawu narzędzi, musimy utworzyć trzy następujące pliki:

- DisabledButtonExtender.vb — ten plik jest klasa sterowania po stronie serwera, który będzie zarządzanie, tworzenie urządzenia extender i umożliwiają ustawianie właściwości w czasie projektowania. Definiuje właściwości, które można ustawić przy użyciu urządzenia extender. Te właściwości są dostępne za pośrednictwem kodu i w czasie projektowania i dopasowania właściwości zdefiniowane w pliku DisableButtonBehavior.js.
- DisabledButtonBehavior.js — Ten plik jest, gdy dodasz wszystkie logiki skryptu klienta.
- DisabledButtonDesigner.vb — ta klasa umożliwia funkcjonalność czasu projektowania. Ta klasa jest konieczne, jeśli chcesz, aby rozszerzenie kontrolki w celu poprawnego działania z Visual Studio/Visual Web Developer Designer.

Dlatego rozszerzenie kontrolki składa się z kontroli po stronie serwera, zachowanie po stronie klienta i po stronie serwera klasy projektanta. Dowiesz się, jak utworzyć wszystkie trzy tych plików w poniższych sekcjach.

## <a name="creating-the-custom-extender-website-and-project"></a>Tworzenie rozszerzeń niestandardową witrynę sieci Web i projektu

Pierwszym krokiem jest, aby utworzyć projekt biblioteki klas i witryny sieci Web w programie Visual Studio/Visual Web Developer. Firma Microsoft ll Tworzenie niestandardowych rozszerzeń w projekcie biblioteki klas i testowania niestandardowych rozszerzeń w witrynie sieci Web.

Pozwól s rozpoczynać witryny sieci Web. Wykonaj następujące kroki, aby utworzyć witrynę sieci Web:

1. Wybierz opcję menu **plik, nową witrynę sieci Web**.
2. Wybierz **witryny sieci Web platformy ASP.NET** szablonu.
3. Nadaj nazwę nowej witryny sieci Web *witryna "website1"*.
4. Kliknij przycisk **OK** przycisku.

Następnie należy utworzyć projekt biblioteki klas, który będzie zawierał kod rozszerzenia kontrolki zestawu narzędzi:

1. Wybierz opcję menu **plik i Dodaj nowy projekt**.
2. Wybierz **biblioteki klas** szablonu.
3. Nazwij nową bibliotekę klas o nazwie **CustomExtenders**.
4. Kliknij przycisk **OK** przycisku.

Po wykonaniu tych kroków, z okna Eksploratora rozwiązań powinien wyglądać jak rysunek 1.


[![Rozwiązanie z witryny sieci Web i klasy projektu biblioteki](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Rysunek 01**: rozwiązania z witryny sieci Web i klasy projektu biblioteki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


Następnie należy dodać wszystkich niezbędnych odwołań do zestawu do projektu biblioteki klas:

1. Kliknij prawym przyciskiem myszy projekt CustomExtenders i wybierz opcję menu **Dodaj odwołanie**.
2. Wybierz kartę .NET.
3. Dodaj odwołania do następujących zestawów:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Wybierz kartę przeglądania.
5. Dodaj odwołanie do zestawu AjaxControlToolkit.dll. Ten zestaw znajduje się w folderze, w której pobrano zestawu narzędzi AJAX Control Toolkit.

Możesz sprawdzić, że dodane zostały wszystkie odpowiednie odwołania, klikając prawym przyciskiem myszy projekt, wybierając właściwości i klikając kartę odwołania (zobacz rysunek 2).


[![Folder odwołania z odwołaniami wymagane](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Rysunek 02**: folder odwołań z odwołań wymagane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Tworzenie rozszerzeń kontrolki niestandardowej

Teraz, gdy mamy już naszej bibliotece klasy, Zaczniemy budowanie nasze rozszerzenie formantu. Pozwól s rozpoczynać kości bare klasy niestandardowego rozszerzenia kontrolki (patrz lista 1).

**Wyświetlanie listy 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Istnieje kilka rzeczy, które można zauważyć, że informacje o klasie rozszerzenia kontrolki w ofercie 1. Najpierw zwróć uwagę, że klasa dziedziczy z klasy bazowej ExtenderControlBase. Wszystkich kontrolek rozszerzeń typu AJAX Control Toolkit pochodzić z tej klasy bazowej. Na przykład klasa bazowa zawiera właściwość TargetID, która jest wymagana właściwość każdego rozszerzenia kontrolki zestawu narzędzi.

Następnie zwróć uwagę, że klasa zawiera następujące atrybuty dotyczą skrypt po stronie klienta:

- Widok — powoduje, że plik do uwzględnienia jako zasobu osadzonego w zestawie.
- ClientScriptResource - powoduje, że zasób skryptu do pobrania z zestawu.

Atrybut widok jest używany do osadzania plik MyControlBehavior.js JavaScript do zestawu podczas kompilowania rozszerzeń niestandardowych. Atrybut ClientScriptResource jest używany do pobierania skryptu MyControlBehavior.js z zestawu, stosowania niestandardowego rozszerzenia na stronie sieci web.


Aby widok i ClientScriptResource atrybuty do pracy należy skompilować pliku JavaScript jako zasobu osadzonego. Zaznacz ten plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisz wartość *zasób osadzony* do **Build Action** właściwości.


Zwróć uwagę, że rozszerzenie kontrolki także atrybutu element TargetControlType. Ten atrybut służy do określania typu formantu, który został rozszerzony przez rozszerzenie kontrolki. W przypadku wyświetlania listy 1 rozszerzenia kontrolki zestawu narzędzi umożliwia rozszerzanie pole tekstowe.

Na koniec Zauważ, że niestandardowego rozszerzenia zawiera właściwość o nazwie MyProperty. Właściwość jest oznaczona atrybutem ExtenderControlProperty. Metody GetPropertyValue() i SetPropertyValue() są używane do przekazywania wartości właściwości z rozszerzenia po stronie serwera kontrolki do zachowania po stronie klienta.

Pozwól s Przejdź dalej i zaimplementować kod dla naszego rozszerzenia DisabledButton. Kod dla tego rozszerzenia można znaleźć w ofercie 2.

**Wyświetlanie listy 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

Rozszerzenie DisabledButton w ofercie 2 ma dwie właściwości o nazwie TargetButtonID i DisabledText. IDReferenceProperty stosowany do właściwości TargetButtonID zapobiega przypisywanie coś innego niż identyfikator kontrolki przycisku do tej właściwości.

Atrybuty widok i ClientScriptResource kojarzenie zachowania klienta znajduje się w pliku o nazwie DisabledButtonBehavior.js z tego rozszerzenia. Omówimy ten plik JavaScript w następnej sekcji.

## <a name="creating-the-custom-extender-behavior"></a>Tworzenie urządzenia Extender niestandardowe zachowanie

Składnik po stronie klienta rozszerzenia kontrolki zestawu narzędzi nazywa się to zachowanie. Rzeczywiste logikę wyłączenie i włączenie przycisku znajduje się w zachowaniu DisabledButton. Kod JavaScript, zachowanie jest uwzględnione w ofercie 3.

**Wyświetlanie listy 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

Plik JavaScript w ofercie 3 zawiera klasę klienta o nazwie DisabledButtonBehavior. Ta klasa, takie jak jego twin po stronie serwera zawiera dwie właściwości o nazwie TargetButtonID i Uzyskaj DisabledText, którego można uzyskiwać dostęp za pomocą\_TargetButtonID/set\_TargetButtonID i Uzyskaj\_DisabledText/set\_ DisabledText.

Metodę initialize() kojarzy keyup obsługi zdarzenia z elementem docelowym zachowania. Wykonuje procedurę obsługi keyup każdorazowo wpisz literę w polu tekstowym skojarzony z tym działaniem. Program obsługi keyup Włącza lub wyłącza przycisk w zależności od tego, czy zawiera dowolny tekst w TextBox skojarzonych z zachowaniem.

Należy pamiętać, że należy skompilować pliku JavaScript w ofercie 3 jako zasobu osadzonego. Zaznacz ten plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisz wartość *zasób osadzony* do **Build Action** właściwości (zobacz rysunek 3). Ta opcja jest dostępna w programie Visual Studio i Visual Web Developer.


[![Dodawanie pliku JavaScript jako zasobu osadzonego](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Rysunek 03**: Dodawanie pliku JavaScript jako zasobu osadzonego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Tworzenie niestandardowego rozszerzenia projektanta

Istnieje jedna klasa ostatniego, która musimy utworzyć do ukończenia naszego rozszerzenia. Musimy utworzyć klasy projektanta w ofercie 4. Ta klasa jest wymagana do udostępnienia extender działają prawidłowo z Visual Studio/Visual Web Developer Designer.

**Wyświetlanie listy 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Projektant w ofercie 4 należy skojarzyć z rozszerzeń DisabledButton atrybutem projektanta. Należy zastosować atrybut Projektant klasy DisabledButtonExtender następująco:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Za pomocą niestandardowego rozszerzenia

Teraz, gdy firma Microsoft została zakończona, Tworzenie rozszerzeń kontrolki DisabledButton, nadszedł czas na ten jest używany w naszej witryny sieci Web platformy ASP.NET. Najpierw musimy dodać niestandardowe rozszerzenie do przybornika. Wykonaj następujące kroki:

1. Kliknij dwukrotnie strony w oknie Eksploratora rozwiązań, aby otworzyć stronę ASP.NET.
2. Kliknij prawym przyciskiem myszy przybornika, a następnie wybierz opcję menu **wybierz elementy**.
3. W oknie dialogowym Wybierz elementy paska narzędzi przejdź do zestawu CustomExtenders.dll.
4. Kliknij przycisk **OK** przycisk, aby zamknąć okno dialogowe.

Po wykonaniu tych kroków rozszerzenie kontrolki DisabledButton powinna zostać wyświetlona w przyborniku (zobacz rysunek 4).


[![DisabledButton w przyborniku](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Rysunek 04**: DisabledButton w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


Następnie należy utworzyć nową stronę programu ASP.NET. Wykonaj następujące kroki:

1. Tworzenie nowej strony programu ASP.NET o nazwie ShowDisabledButton.aspx.
2. Przeciągnij element ScriptManager na stronie.
3. Przeciągnij formant TextBox na stronie.
4. Przeciągnij formant przycisku na stronie.
5. W oknie Właściwości zmień wartość właściwości identyfikator przycisku na wartość <em>btnSave</em> i właściwość tekst na wartość *Zapisz\**.
  

Utworzyliśmy strony z formantu standardowego ASP.NET pole tekstowe i przycisk.

Następnie należy rozszerzyć formant pola tekstowego przy użyciu rozszerzeń DisabledButton:

1. Wybierz **Dodaj Extender** zadań opcję, aby otworzyć okno dialogowe Kreator rozszerzeń (zobacz rysunek 5). Należy zauważyć, że okno dialogowe zawiera nasz niestandardowego rozszerzenia DisabledButton.
2. Wybierz rozszerzenie DisabledButton, a następnie kliknij przycisk **OK** przycisku.


[![Okno dialogowe Kreator rozszerzeń](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Rysunek 05**: okno dialogowe Kreator rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Ponadto firma Microsoft można ustawić właściwości rozszerzenia, które ma DisabledButton. Można zmodyfikować właściwości rozszerzenia, które ma DisabledButton, modyfikując właściwości formant pola tekstowego:

1. Wybierz pole tekstowe w projektancie.
2. W oknie właściwości rozwiń węzeł rozszerzeń (patrz rysunek 6).
3. Przypisz wartość *Zapisz* DisabledText właściwości i wartość *btnSave* właściwości TargetButtonID.


[![Ustawianie właściwości rozszerzenia](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Rysunek 06**: Ustawianie właściwości rozszerzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Po uruchomieniu strony (przez naciskać klawisz F5), formant przycisku jest początkowo wyłączone. Zaraz po jego uruchomieniu, wprowadzając tekst w polu tekstowym, przycisk kontrolka jest włączona (zobacz rysunek 7).


[![Rozszerzenie DisabledButton w działaniu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Rysunek 07**: rozszerzenie DisabledButton w akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wyjaśniają, jak można je rozszerzyć AJAX Control Toolkit za pomocą niestandardowego rozszerzenia kontrolki. W tym samouczku utworzyliśmy proste rozszerzenie kontrolki DisabledButton. Wprowadziliśmy tego rozszerzenia, tworząc klasę DisabledButtonExtender, zachowanie DisabledButtonBehavior JavaScript i klasa DisabledButtonDesigner. Podobny zestaw kroków należy wykonać przy każdym utworzeniu rozszerzeń kontrolek niestandardowych.

> [!div class="step-by-step"]
> [Poprzednie](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
