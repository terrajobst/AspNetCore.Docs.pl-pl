---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Jak używać formantu edytora HTML (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor jest kontrolka AJAX ASP.NET, która pozwala na łatwe tworzenie i edytowanie zawartość HTML za pomocą przycisków na pasku narzędzi.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fca18948c0e4f1323f214dc0033f19fa44efad47
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879780"
---
<a name="how-do-i-use-the-html-editor-control-c"></a>Jak używać formantu edytora HTML (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> HTMLEditor jest kontrolka AJAX ASP.NET, która pozwala na łatwe tworzenie i edytowanie zawartość HTML za pomocą przycisków na pasku narzędzi.


Celem tego samouczka jest dostarczają Omówienie formantu edytora HTML dołączone do zestawu narzędzi kontroli AJAX. Edytor HTML zawiera opcje Zmienianie rozmiaru czcionki, wybierając czcionki, zmiana koloru tła, modyfikowanie kolor pierwszego planu dodawania łączy, obrazy, zmiana wyrównania tekstu i wykonywania wycinania, kopiowania i wklejania operacji (zobacz rysunek 1).


[![Edytor HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Rysunek 01**: edytor HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


Edytor HTML umożliwia wprowadzanie zawartości przy użyciu trybu projektowania lub HTML można wprowadzić bezpośrednio. Można również uzyskać umożliwia podgląd zawartości HTML (patrz rysunek 2).


[![Projekt, HTML i Podgląd przycisków](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Rysunek 02**: projekt, HTML i Podgląd przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


Z tego samouczka dowiesz się, jak wyświetlić edytora HTML, dostosowywanie przycisków paska narzędzi, które są wyświetlane w edytorze HTML i zapobiegania atakom skryptów między witrynami.

## <a name="displaying-the-html-editor"></a>Wyświetlanie edytora HTML

Przed użyciem edytora HTML strony ASP.NET, najpierw należy dodać formantu ScriptManager do strony. Formantu ScriptManager znajduje się poniżej karcie rozszerzenia AJAX w Visual Studio/Visual Web Developer Express przybornika.

W górnej części strony przed wszystkie inne formanty na stronie należy umieścić formantu ScriptManager. Na przykład możesz umieścić go bezpośrednio poniżej serwerowe otwierania &lt;formularza&gt; tagu.

Formant edytora HTML znajduje się w przybornika z resztą formanty Toolkit kontroli AJAX. Nazwie kontrolce edytora (patrz rysunek 3).


[![Formant edytora HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Rysunek 03**: kontrolce edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


Po przeciągnięciu edytora HTML na stronie można ustawić właściwości w arkuszu właściwości. Na przykład zwykle chcesz ustawić właściwości Width i Height. Wyświetlanie listy 1 zawiera źródło dla strony platformy ASP.NET, który zawiera edytor HTML.

**Wyświetlanie listy 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

Strona wyświetlania 1 zawiera kontrolkę edytora HTML, kontrolkę przycisku i formancie Literal. Po kliknięciu przycisku zawartość edytora HTML jest wyświetlana w formancie Literal (patrz rysunek 4).


[![Przesyłanie formularza z edytora HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Rysunek 04**: przesyłanie formularza z edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


Właściwość Content edytora HTML służy do pobierania zawartości HTML wprowadzone w edytorze HTML. Należy pamiętać, że ta zawartość HTML może zawierać JavaScript. W następnej sekcji omówiono sposób można zapobiec atakom iniekcji JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Dostosowywanie paska narzędzi edytora HTML

Można dostosować dokładnie przyciski, które są wyświetlane w edytorze. Na przykład można usunąć kartę HTML, aby uniemożliwić użytkownikom przełączanie w tryb HTML edytora HTML. Lub, można usunąć z listy rozwijanej rozmiar czcionki, aby uniemożliwić użytkownikom tworzenie zbyt duży w forum komunikatów post (patrz rysunek 5).


[![Dostosowane edytora HTML](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Rysunek 05**: A dostosowane edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Przyciski paska narzędzi można dostosować przez wyprowadzanie nowy edytor HTML z klasy podstawowej edytora. Na przykład edytora niestandardowego wyświetlania 2 zawiera przycisków paska narzędzi dla pogrubionego oraz pochylonego. Usunięto wszystkich innych przycisków paska narzędzi. Ponadto karta HTML został usunięty w dolnej części edytora (ale karty Projekt i w wersji zapoznawczej nadal występują).

**Wyświetlanie listy 2 - App\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Musisz dodać klasę wyświetlania 2 do aplikacji\_kodu folderu, dzięki czemu klasa zostanie skompilowany, automatycznie. Jeśli aplikacja\_kodu folder nie istnieje w witrynie sieci Web, a następnie można po prostu Dodaj folder.

Po utworzeniu niestandardowego edytora możesz można dodać go do strony platformy ASP.NET w taki sam sposób jak podczas dodawania normalne edytora HTML (patrz lista 3).

**Wyświetlanie listy 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Unikanie atakami skryptów między witrynami (XSS)

Zawsze, gdy akceptowanie danych wejściowych od użytkownika, a następnie ponownie wyświetlić te dane wejściowe w witrynie sieci Web, możesz potencjalnie otworzyć witryny sieci Web na ataki skryptów między witrynami (XSS). Teoretycznie złośliwym hakerom można przesłać kodu JavaScript, który jest wykonywany, gdy dane wejściowe zostanie wyświetlony ponownie. Kod JavaScript może służyć do kradzieży hasła użytkowników lub inne poufne informacje.

Zazwyczaj może pokonać atakom XSS przez HTML kodowanie danych wejściowych, niezależnie od pobrania od użytkownika przed wyświetleniem go na stronie sieci web. Jednak kodowania danych wyjściowych edytora HTML może nie tylko kodowanie HTML &lt;skryptu&gt; tagów, będzie on również kodowania wszystkie tagi HTML. Innymi słowy spowoduje utratę wszystkich formatowania, takie jak typ czcionki, rozmiaru czcionki i kolor tła.

Jeśli poufne informacje są zbierane od użytkowników — takie jak hasła, numery kart kredytowych i numery ubezpieczenia społecznego - powinien nie zawiera cofanie zakodowanego zawartość, która pobrać z użytkownikiem z edytora HTML. Edytor HTML należy używać tylko w sytuacji, w których są nie ponowne wyświetlanie zawartości HTML, lub zawartość HTML jest przesyłane do witryny sieci Web przez zaufane firmy.

Załóżmy, na przykład tworzysz aplikację blogu. W takim przypadku warto użyć edytora HTML, tworząc wpisy na blogu. Jesteś jedyną osobą, która przesyła w blogu i prawdopodobnie, można ufać sobie nie można przesłać złośliwego kodu JavaScript. Jednak nie mieć sensu przypadku zezwalania użytkownikom anonimowym publikować komentarze za pomocą edytora HTML. Należy zachować szczególną ostrożność w sytuacjach, w których użytkownicy przesłać poufne informacje, takie jak hasła. Złośliwy użytkownik może ewentualnie po komentarz, który zawiera prawo JavaScript w przypadku kradzieży hasła.

## <a name="summary"></a>Podsumowanie

W tym samouczku zostały podane krótki przegląd formantu edytora HTML zawarte w zestawie narzędzi kontroli AJAX. Przedstawiono sposób, aby zaakceptować bogatej zawartości z danych użytkownika i przesyłania zawartości na serwerze za pomocą edytora HTML. Omówiono również, jak można dostosować przycisków paska narzędzi, które są wyświetlane w edytorze HTML. Ponadto przedstawiono sposób uniknąć skryptów przed atakami opartymi na Cross-Site, używając edytora HTML do przyjmowania danych wejściowych mogą okazać się złośliwe.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
