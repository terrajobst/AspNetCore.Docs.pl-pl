---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Wykonywanie prostej weryfikacji (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Informacje o sposobie przeprowadzania weryfikacji w aplikacji ASP.NET MVC. W tym samouczku Walther Autor: Stephen wprowadza do stanu modelu i pomocnika weryfikacji HTML...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 26900229630b25affe21a5bb801ef0247711d26b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399464"
---
<a name="performing-simple-validation-vb"></a>Wykonywanie prostej weryfikacji (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Informacje o sposobie przeprowadzania weryfikacji w aplikacji ASP.NET MVC. W tym samouczku Walther Autor: Stephen wprowadza do stanu modelu i pomocników HTML sprawdzania poprawności.


Celem tego samouczka jest wyjaśniają, jak można wykonywać sprawdzanie poprawności w aplikacji ASP.NET MVC. Na przykład dowiesz się, jak zapobiec ktoś przesyłania formularza, który nie zawiera wartości dla wymaganego pola. Dowiesz się, jak używać stan modelu i sprawdzania poprawności pomocników HTML.

## <a name="understanding-model-state"></a>Opis stanu modelu

Umożliwia stan modelu — lub dokładniej, ze słownika stanu modelu — reprezentują błędy sprawdzania poprawności. Na przykład akcja Create() w ofercie 1 sprawdza właściwości klasy produktu przed dodaniem klasy produktu do bazy danych.


Nie jestem I zalecania dodać logikę weryfikacji lub bazy danych do kontrolera. Kontroler może zawierać tylko logiki powiązane do sterowania działaniem aplikacji. Podejmujemy skrótów, aby zachować ich prostotę.


**Wyświetlanie listy 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

W ofercie 1 Nazwa, opis i UnitsInStock właściwości klasy produktu są weryfikowane. Jeśli dowolne z tych właściwości testu sprawdzania poprawności zakończy się niepowodzeniem błąd zostanie dodany do słownika stanu modelu (reprezentowane przez właściwość ModelState klasy kontrolera).

Jeśli istnieją błędy w stanie modelu ModelState.IsValid właściwość zwraca wartość false. W takim przypadku zostanie wyświetlony ponownie formularza HTML do tworzenia nowych produktów. W przeciwnym razie jeśli nie ma żadnych błędów sprawdzania poprawności, nowy produkt jest dodawany do bazy danych.

## <a name="using-the-validation-helpers"></a>Za pomocą pomocników sprawdzania poprawności

Platforma ASP.NET MVC zawiera dwa pomocników sprawdzania poprawności: pomocnika Html.ValidationMessage() i pomocnika Html.ValidationSummary(). Użyjesz tych dwóch wątków w widoku do wyświetlania komunikatów o błędach weryfikacji.

Pomocnicy Html.ValidationMessage() i Html.ValidationSummary() są używane w tworzenie i edytowanie widoków, które są generowane automatycznie przez szkieletu ASP.NET MVC. Wykonaj następujące kroki, aby wygenerować widok Utwórz:

1. Kliknij prawym przyciskiem myszy w akcji Create() kontroler produktu, a następnie wybierz opcję menu **Dodaj widok** (patrz rysunek 1).
2. W **Dodaj widok** okno dialogowe, zaznacz pola wyboru **utworzyć widok silnie typizowane** (patrz rysunek 2).
3. Z **wyświetlić klasy danych** listy rozwijanej wybierz klasy produktu.
4. Z **wyświetlanie zawartości** listy rozwijanej, wybierz pozycję Utwórz.
5. Kliknij przycisk **Dodaj** przycisku.


Upewnij się, czy kompilujesz aplikację przed dodaniem widoku. W przeciwnym razie lista klas nie będzie wyświetlane w **wyświetlić klasy danych** listy rozwijanej.


[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Rysunek 01**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image2.png))


[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Rysunek 02**: tworzenia silnie typizowane widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image4.png))


Po wykonaniu tych kroków, uzyskasz widoku Utwórz w ofercie 2.

**Wyświetlanie listy 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

W ofercie 2 pomocnika Html.ValidationSummary() jest wywoływana natychmiast powyżej formularza HTML. Tego pomocnika służy do wyświetlania listy komunikaty o błędach weryfikacji. Pomocnik Html.ValidationSummary() renderuje błędy na liście punktowanej.

Pomocnik Html.ValidationMessage() nazywa się obok każdego pola formularza HTML. Pomocnik ten jest używany do wyświetlania komunikatu o błędzie w prawo obok pola formularza. W przypadku wyświetlania listy 2 pomocnika Html.ValidationMessage() wyświetla gwiazdkę, gdy występuje błąd.

Strona na rysunku 3 przedstawiono komunikaty o błędach renderowany przez pomocników sprawdzania poprawności, gdy formularz zostanie przesłany z polami brakujących i nieprawidłowych wartości.


[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Rysunek 03**: Create view, które są przesyłane z problemami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image6.png))


Należy zauważyć, że pola również są modyfikowane, gdy występuje błąd weryfikacji danych wejściowych wygląd elementu HTML. Renderuje pomocnika Html.TextBox() *klasy = "danych wejściowych błędzie sprawdzania poprawności"* atrybutu, gdy występuje błąd weryfikacji skojarzony z właściwością renderowany przez pomocnika Html.TextBox().

Istnieją trzy kaskadowych klasy arkusz stylów używany, aby sterować wyglądem błędy sprawdzania poprawności:

- dane wejściowe błędzie sprawdzania poprawności — są stosowane do &lt;wejściowych&gt; renderowany przez Html.TextBox() pomocnika tagów.
- pole — błędzie sprawdzania poprawności — są stosowane do &lt;span&gt; renderowany przez pomocnika Html.ValidationMessage() tagu.
- błędy — sprawdzania poprawności podsumowanie — są stosowane do &lt;ul&gt; renderowany przez pomocnika Html.ValidationSumamry() tagu.

Możesz zmodyfikować te kaskadowych klasy arkusza stylów i w związku z tym zmodyfikować wygląd błędy sprawdzania poprawności, modyfikując plik Site.css znajduje się w folderze zawartości.

> [!NOTE] 
> 
> Klasa HtmlHelper obejmuje statycznych właściwości tylko do odczytu dla pobierania nazw weryfikacji powiązanych CSS klasy. Te właściwości statyczne są nazywane ValidationInputCssClassName ValidationFieldCssClassName i ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding weryfikacji i sprawdzania poprawności Postbinding

Jeśli przesyłanie formularza HTML do tworzenia produktu i wprowadź nieprawidłową wartość dla pola Cena i bez wartości dla pola UnitsInStock, następnie otrzymasz komunikatów weryfikacji wyświetlanych na rysunku 4. Skąd pochodzą te komunikaty o błędach weryfikacji?


[![Okno dialogowe Nowy projekt](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Rysunek 04**: błędy sprawdzania poprawności Prebinding ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-simple-validation-vb/_static/image8.png))


Istnieją faktycznie dwa typy komunikatów o błędach weryfikacji - wygenerowanymi przed pola formularza HTML są powiązane z klasą i te wygenerowane po pól formularza jest powiązana z tej klasy. Innymi słowy, istnieją prebinding błędy sprawdzania poprawności i postbinding błędy sprawdzania poprawności.

Akcja Create() udostępnianych przez kontroler produktu w ofercie 1 przyjmuje wystąpienie klasy produktu. Podpis metody Create wygląda następująco:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Wartości pól formularza HTML w formularzu tworzenia związane są z klasą productToCreate coś, co jest nazywane integratora modelu. Domyślny integrator modelu komunikat o błędzie do stanu modelu automatycznie dodaje gdy go nie można powiązać pole formularza z właściwością formularza.

Ciąg "apple" nie można powiązać właściwości cena klasy produktu domyślnego integratora modelu. Ciąg nie można przypisać do właściwości dziesiętnej. W związku z tym integratora modelu dodaje błąd do stanu modelu.

Domyślny integrator modelu również nie można przypisać wartość Nothing właściwość, która nie przyjmuje wartość Nothing. W szczególności integratora modelu nie można przypisać wartość Nothing właściwość UnitsInStock. Jeszcze raz integratora modelu rezygnuje i dodaje komunikat o błędzie do stanu modelu.

Jeśli chcesz dostosować wygląd te prebinding komunikaty o błędach następnie należy utworzyć ciągów zasobów dla tych komunikatów.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było opisują podstawowa mechanika sprawdzania poprawności w platformę ASP.NET MVC. Przedstawiono sposób używania stanu modelu i pomocników HTML sprawdzania poprawności. Omówiono również rozróżnienie między prebinding i postbinding sprawdzania poprawności. W innych samouczków omówimy różnych strategii przenoszenia kod sprawdzania poprawności poza kontrolerach i w klasach modeli.

> [!div class="step-by-step"]
> [Poprzednie](displaying-a-table-of-database-data-vb.md)
> [dalej](validating-with-the-idataerrorinfo-interface-vb.md)
