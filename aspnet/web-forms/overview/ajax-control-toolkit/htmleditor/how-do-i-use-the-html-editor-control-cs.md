---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Jak używać kontrolka edytora HTML? (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: HTMLEditor to kontrolka AJAX programu ASP.NET, która pozwala na łatwe tworzenie i edytowanie zawartości HTML za pomocą przycisków na pasku narzędzi.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 775cb113ea11e582806b0bd9397778cff6761940
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809410"
---
<a name="how-do-i-use-the-html-editor-control-c"></a>Jak używać kontrolka edytora HTML? (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> HTMLEditor to kontrolka AJAX programu ASP.NET, która pozwala na łatwe tworzenie i edytowanie zawartości HTML za pomocą przycisków na pasku narzędzi.


Celem tego samouczka jest zapewnienie Przegląd kontrolka edytora HTML dołączone do zestawu narzędzi AJAX Control Toolkit. Edytor HTML zawiera opcje zmiany rozmiaru czcionki, wybierając czcionki, zmiana koloru tła, modyfikując kolor pierwszego planu dodawania łączy, dodawanie obrazów, zmiana wyrównania tekstu i wykonywanie wycinania, kopiowania i wklejania operacje (zobacz rysunek 1).


[![Edytor HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Rysunek 01**: edytor HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


Edytor HTML umożliwia wprowadzanie zawartości przy użyciu trybu projektowania lub możesz wprowadzić HTML bezpośrednio. Możesz również znajdują się z opcją zawartości HTML w wersji zapoznawczej (patrz rysunek 2).


[![Projektowanie, HTML i w wersji zapoznawczej przycisków](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Rysunek 02**: przyciski projektowania, HTML i w wersji zapoznawczej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


W tym samouczku dowiesz się, sposób wyświetlania edytora HTML, jak dostosować przyciski paska narzędzi, które pojawiają się w edytorze HTML i sposoby unikania atakami skryptów między witrynami.

## <a name="displaying-the-html-editor"></a>Wyświetlanie edytora HTML

Zanim użyjesz edytora HTML na stronie ASP.NET, należy najpierw dodać formantu ScriptManager do strony. Formantu ScriptManager znajduje się poniżej na karcie rozszerzenia AJAX w przyborniku Visual Studio/Visual Web Developer Express.

W górnej części strony przed wszystkie inne formanty na stronie należy umieścić formantu ScriptManager. Na przykład, należy go umieścić bezpośrednio poniżej po stronie serwera otwierania &lt;formularza&gt; tagu.

Kontrolka edytora HTML znajduje się w przyborniku z pozostałą częścią kontrolki zestawu narzędzi AJAX Control Toolkit. Jest on nazwany kontrolka edytora (zobacz rysunek 3).


[![Kontrolka edytora HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Rysunek 03**: kontrolka edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


Po przeciągnięciu edytora HTML na stronie można ustawić jego właściwości w arkuszu właściwości. Na przykład zazwyczaj chcesz ustawić właściwości Width i Height. Wyświetlanie listy 1 zawiera źródła dla strony ASP.NET, która zawiera edytor HTML.

**Wyświetlanie listy 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

Na stronie w ofercie 1 zawiera kontrolka edytora HTML, formant przycisku i formant literału. Po kliknięciu przycisku, zawartość edytora HTML jest wyświetlana w formancie Literal (zobacz rysunek 4).


[![Przesyłanie formularza za pomocą edytora HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Rysunek 04**: przesyłanie formularza za pomocą edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


Zawartość edytora HTML jest używana do pobierania zawartości HTML wprowadzone w edytorze HTML. Należy pamiętać, że ta zawartość HTML może zawierać języka JavaScript. W następnej sekcji omówiono, jak zapobiegać atakom iniekcji JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Dostosowywanie paska narzędzi edytora HTML

Można dostosować dokładnie przyciski, które pojawiają się w edytorze. Na przykład można usunąć karta HTML, aby uniemożliwić użytkownikom przełączanie edytora HTML w trybie HTML. Lub możesz chcieć usunąć listę rozwijaną rozmiar czcionki, tak aby uniemożliwić użytkownikom tworzenie zbyt duże pole tekstowe wśród wątków forum Księguj message, (zobacz rysunek 5).


[![Niestandardowy edytor HTML](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Rysunek 05**: A dostosowany edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Przyciski paska narzędzi można dostosować przez wyprowadzanie nowy edytor HTML z klasy bazowej edytora. Niestandardowy Edytor w ofercie 2 zawiera tylko przyciski paska narzędzi dla pogrubiony i kursywę. Inne przyciski paska narzędzi, zostały usunięte. Ponadto karta HTML został usunięty w dolnej części edytora (ale karty projektowania i w wersji zapoznawczej nadal istnieją).

**Wyświetlanie listy 2 - App\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Klasa w ofercie 2 należy dodać do swojej aplikacji\_kodu folderu, tak aby klasy, zostanie automatycznie kompilowane. Jeśli aplikacja\_katalogu z kodem nie istnieje w witrynie sieci Web, a następnie można po prostu dodać folder.

Po utworzeniu niestandardowego edytora, dodaniem go do strony ASP.NET w taki sam sposób w miarę dodawania normalne edytora HTML (patrz lista 3).

**Wyświetlanie listy 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Unikanie atakami skryptów między witrynami (XSS)

Za każdym razem akceptują dane wejściowe od użytkownika i ponownie wyświetlić te dane wejściowe w witrynie sieci Web, możesz potencjalnie Otwórz witrynę sieci Web z atakami skryptów między witrynami (XSS). Teoretycznie złośliwym hakerom można przesłać kodu JavaScript, który pobiera wykonania, gdy dane wejściowe, zostanie wyświetlony ponownie. Kod JavaScript może służyć do kradzieży haseł użytkowników ani innych informacji poufnych.

Zazwyczaj może zniweczyć cały atakom XSS, kodowania danych wejściowych, niezależnie od pobrania od użytkownika przed wyświetleniem go na stronie sieci web HTML. Jednak kodowania danych wyjściowych edytora HTML może nie tylko kodowanie HTML &lt;skryptu&gt; tagów, również będzie ona kodowanie wszystkie tagi HTML. Innymi słowy spowoduje utratę wszystkich formatowania, takie jak typ czcionki, rozmiaru czcionki i kolor tła.

Jeśli poufne informacje są zbierane od użytkowników — takie jak hasła, numerów kart kredytowych i numery ubezpieczenia społecznego — należy nie zawiera usunięcie zakodowanym zawartość, która możesz pobrać z użytkownikiem za pomocą edytora HTML. Edytor HTML należy używać tylko w sytuacji, w których nie są wyświetlania zawartości HTML, lub zawartość HTML jest przesyłane do witryny sieci Web przez zaufany.

Wyobraź sobie, na przykład tworzysz aplikację blogu. W takiej sytuacji warto użyć edytora HTML, tworząc wpisy w blogu. Jesteś jedyną osobą, która przesyła wpis w blogu i prawdopodobnie, mogą ufać sobie nie, aby przesłać złośliwego kodu JavaScript. Jednak go nie ma sensu używać edytora HTML w przypadku zezwalania użytkownikom anonimowym na komentarze. Należy zachować szczególną ostrożność w sytuacjach, w których użytkownicy przesyłać poufne informacje, takie jak hasła. Złośliwy użytkownik może potencjalnie, Opublikuj komentarz, który zawiera odpowiednie JavaScript dla kradzież hasła.

## <a name="summary"></a>Podsumowanie

W tym samouczku podano krótki przegląd kontrolka edytora HTML objęte zestawu narzędzi AJAX Control Toolkit. Przedstawiono sposób akceptowania sformatowanej zawartości od użytkownika i przesyłanie zawartości do serwera przy użyciu edytora HTML. Omówiono również, w jaki sposób dostosować przyciski paska narzędzi, które są wyświetlane w edytorze HTML. Na koniec pokazaliśmy ci, jak należy unikać atakami skryptów między witrynami, podczas używania edytora HTML do przyjmowania danych wejściowych mogą okazać się złośliwe.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
