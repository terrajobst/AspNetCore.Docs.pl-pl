---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Sprawdzaniu poprawności proste (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Informacje o sposobie przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. W tym samouczku Stephen Walther wprowadza do stanu modelu i pomocnika weryfikacji HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: efb98d87106e332fffb158e5f382d57fea778957
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869754"
---
<a name="performing-simple-validation-vb"></a>Sprawdzaniu poprawności proste (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Informacje o sposobie przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. W tym samouczku Stephen Walther wprowadza do stanu modelu i pomocników HTML sprawdzania poprawności.


Celem tego samouczka jest wyjaśnienie sposobu wykonywania weryfikacji w aplikacji ASP.NET MVC. Na przykład zostanie przedstawiony sposób zapobiec ktoś przesyłania formularza, który nie zawiera wartości dla wymaganego pola. Jak używać stan modelu i sprawdzania poprawności pomocników HTML.

## <a name="understanding-model-state"></a>Opis stanu modelu

Używasz stan modelu — lub dokładniej ze słownika stanu modelu — do reprezentowania błędy sprawdzania poprawności. Na przykład w akcji Create() wyświetlania 1 sprawdza właściwości klasy produktu przed dodaniem klasy produktu do bazy danych.


Nie mam I rekomendowania Dodaj logikę sprawdzania poprawności lub bazy danych do kontrolera. Kontroler powinien zawierać tylko logiki powiązane do sterowania działaniem aplikacji. Firma Microsoft trwa skrót do zachowania prostych czynności.


**Wyświetlanie listy 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Wyświetlanie listy 1 Nazwa, opis i StanMagazynu właściwości klasy produktu są weryfikowane. Jeśli dowolne z tych właściwości test weryfikacji nie powiedzie się błędu jest dodawany do słownika stanu modelu (reprezentowane przez właściwość ModelState klasy Controller).

Jeśli wystąpią jakieś błędy w stanie modelu właściwość ModelState.IsValid zwraca wartość false. W takim przypadku formularza HTML służący do tworzenia nowego produktu zostanie wyświetlony ponownie. W przeciwnym razie jeśli nie ma żadnych błędów sprawdzania poprawności, nowego produktu jest dodawany do bazy danych.

## <a name="using-the-validation-helpers"></a>Przy użyciu pomocników sprawdzania poprawności

Platforma ASP.NET MVC zawiera dwa pomocników sprawdzania poprawności: pomocnika Html.ValidationMessage() i pomocnika Html.ValidationSummary(). Użyjesz tych dwóch pomocników w widoku wyświetlane komunikaty o błędach weryfikacji.

Pomocnicy Html.ValidationMessage() i Html.ValidationSummary() są używane w widokach tworzenie i edytowanie, które są generowane automatycznie przez funkcję szkieletów ASP.NET MVC. Wykonaj następujące kroki, aby wygenerować widok Utwórz:

1. Kliknij prawym przyciskiem myszy w akcji Create() kontroler produktu i wybierz opcję menu **Dodaj widok** (zobacz rysunek 1).
2. W **Dodaj widok** okno dialogowe, zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie** (patrz rysunek 2).
3. Z **wyświetlić klasy danych** listy rozwijanej wybierz klasy produktu.
4. Z **wyświetlania zawartości** listy rozwijanej, wybierz opcję Utwórz.
5. Kliknij przycisk **Dodaj** przycisku.


Upewnij się, że można skompilować aplikację przed dodaniem widoku. W przeciwnym razie nie będą wyświetlane na liście klas w **wyświetlić klasy danych** listy rozwijanej.


[![Okno dialogowe nowego projektu](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Rysunek 01**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image2.png))


[![Okno dialogowe nowego projektu](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Rysunek 02**: tworzenia widoku jednoznacznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image4.png))


Po wykonaniu tych kroków w wersji 2 wyświetlania jest wyświetlany widok Utwórz.

**Wyświetlanie listy 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Wyświetlanie listy 2 pomocnika Html.ValidationSummary() jest wywoływana natychmiast powyżej formularza HTML. Pomocnik ten służy do wyświetlania listę komunikatów o błędach weryfikacji. Pomocnik Html.ValidationSummary() renderuje błędy na liście punktowanej.

Pomocnik Html.ValidationMessage() nazywa się obok każdego pola formularza HTML. Pomocnik ten służy do wyświetlania komunikat o błędzie obok pola formularza. W przypadku wyświetlania 2 pomocnika Html.ValidationMessage() wyświetla gwiazdkę po wystąpieniu błędu.

Strony na rysunku 3 przedstawiono komunikaty o błędach renderowana przez pomocników weryfikacji po przesłaniu formularza z brakujące pola i nieprawidłowe wartości.


[![Okno dialogowe nowego projektu](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Rysunek 03**: Tworzenie widoku przesłane z problemami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image6.png))


Należy zauważyć, że pola są również zmodyfikowane w razie błędu sprawdzania poprawności danych wejściowych wygląd HTML. Renderuje pomocnika Html.TextBox() *klasy = "input błędzie sprawdzania poprawności"* atrybutu, gdy występuje błąd weryfikacji skojarzony z właściwością renderowana przez pomocnika Html.TextBox().

Istnieją trzy kaskadowych klasy arkusza stylów sterować wyglądem błędy sprawdzania poprawności:

- dane wejściowe błędzie sprawdzania poprawności — są stosowane do &lt;wejściowych&gt; renderowana przez Html.TextBox() pomocnika tagów.
- pole — błędzie sprawdzania poprawności — są stosowane do &lt;span&gt; renderowana przez pomocnika Html.ValidationMessage() tagu.
- błędy — sprawdzania poprawności — podsumowanie — są stosowane do &lt;ul&gt; renderowana przez pomocnika Html.ValidationSumamry() tagu.

Możesz zmodyfikować te kaskadowych klasy arkusza stylów i w związku z tym zmodyfikować wygląd błędy sprawdzania poprawności przez zmodyfikowanie pliku Site.css znajdujące się w folderze zawartości.

> [!NOTE] 
> 
> Klasa HtmlHelper zawiera statycznej właściwości tylko do odczytu podczas pobierania nazwy weryfikacji związanych z CSS klasy. Te właściwości statyczne są nazywane ValidationInputCssClassName, ValidationFieldCssClassName i ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding weryfikacji i sprawdzania poprawności Postbinding

Jeśli przesyłania formularza HTML służący do tworzenia produktu i wprowadź nieprawidłową wartość dla pola Cena i bez wartości dla pola StanMagazynu, następnie otrzymasz komunikatów dotyczących sprawdzania poprawności wyświetlane na rysunku 4. Skąd pochodzą te komunikaty o błędach weryfikacji?


[![Okno dialogowe nowego projektu](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Rysunek 04**: błędy sprawdzania poprawności Prebinding ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image8.png))


Brak faktycznie dwa typy komunikatów o błędach weryfikacji - wygenerowanymi przed pola formularza HTML są powiązane z klasą i te wygenerowane po pola formularza jest powiązana z tej klasy. Innymi słowy, występują błędy sprawdzania poprawności prebinding i postbinding błędy sprawdzania poprawności.

Akcja Create() udostępnianych przez kontroler produktu w 1 Lista akceptuje wystąpienia klasy produktu. Podpis metody Create wygląda następująco:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Wartości pól formularza HTML w formularzu Utwórz są powiązane z klasą productToCreate przez element zwany integratora modelu. Domyślny integrator modelu dodaje komunikat o błędzie do stanu modelu automatycznie, gdy nie można powiązać pole formularza właściwości formularza.

Ciąg "apple" nie można powiązać właściwości cen klasy produktu domyślnego integratora modelu. Ciąg nie można przypisać do właściwości typu decimal. W związku z tym integratora modelu dodaje błąd do stanu modelu.

Domyślny integrator modelu także nie można przypisać wartość Nothing do właściwości, która nie przyjmuje wartość Nothing. W szczególności integratora modelu nie można przypisać wartość Nothing StanMagazynu właściwości. Jeszcze raz integratora modelu zrezygnuje i dodaje komunikat o błędzie do stanu modelu.

Jeśli chcesz dostosować wygląd te komunikaty o błędach prebinding następnie musisz utworzyć ciągów zasobów dla tych wiadomości.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było do opisywania podstawowa mechanika weryfikacji platformy ASP.NET MVC. Przedstawiono sposób użycia stanu modelu i sprawdzania poprawności pomocników HTML. Omówiono także różnice między prebinding i postbinding sprawdzania poprawności. W innych samouczków omówiono różne strategie kod weryfikacji z kontrolerami poza i w klasach modeli.

> [!div class="step-by-step"]
> [Poprzednie](displaying-a-table-of-database-data-vb.md)
> [dalej](validating-with-the-idataerrorinfo-interface-vb.md)
