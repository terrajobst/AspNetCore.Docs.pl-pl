---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Tworzenie układów stron z strony wzorcowej widoku (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Z tego samouczka dowiesz się sposobu tworzenia typowych układ strony dla wielu stronach w aplikacji korzystając z widoku strony wzorcowe. Można użyć...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 5208cedd8d24a290a0227bdcbaa84ae6210cd969
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>Tworzenie układów stron z strony wzorcowej widoku (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> Z tego samouczka dowiesz się sposobu tworzenia typowych układ strony dla wielu stronach w aplikacji korzystając z widoku strony wzorcowe. Strony wzorcowej widoku, można użyć na przykład do definiowania układu strony dwie kolumny i używanie układu dwie kolumny dla wszystkich stron w aplikacji sieci web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Tworzenie układów stron z strony wzorcowej widoku

Z tego samouczka dowiesz się sposobu tworzenia typowych układ strony dla wielu stronach w aplikacji korzystając z widoku strony wzorcowe. Strony wzorcowej widoku, można użyć na przykład do definiowania układu strony dwie kolumny i używanie układu dwie kolumny dla wszystkich stron w aplikacji sieci web.

Możesz również mogą korzystać z widoku strony wzorcowe, aby udostępnić zawartość wspólnej na wielu stronach w aplikacji. Na przykład można umieścić Twojego logo witryny sieci Web, łącza nawigacji i transparent anonsów w widoku strony wzorcowej. W ten sposób każdej strony w aplikacji będą automatycznie wyświetlenia tej zawartości.

Z tego samouczka dowiesz się tworzenie nowej strony wzorcowej widoku i Utwórz nowy widok zawartości strony na podstawie strony wzorcowej.

### <a name="creating-a-view-master-page"></a>Tworzenie strony wzorcowej widoku

Zacznijmy od utworzenia widoku strony wzorcowej, który definiuje układu dwie kolumny. Można dodać nowej strony wzorcowej widoku do projektu MVC klikając prawym przyciskiem myszy Views\Shared folder, wybranie opcji menu **Dodaj, nowy element**i wybór szablonu strony wzorcowej widoku MVC (zobacz rysunek 1).


[![Dodawanie widoku strony wzorcowej](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Rysunek 01**: Dodawanie widoku strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


W aplikacji można utworzyć więcej niż jeden widok strony wzorcowej. Każdej strony wzorcowej widoku można zdefiniować układu innej strony. Można na przykład określonych stron do układ dwie kolumny i innych stron układ trzy kolumny.

Widok strony wzorcowej wygląda bardzo podobnie standardowe widoku aplikacji ASP.NET MVC. Jednak w przeciwieństwie do normalnego widoku strony wzorcowej widoku zawiera jeden lub więcej `<asp:ContentPlaceHolder>` tagów. `<contentplaceholder>` Tagi są używane do oznaczania obszary strony głównej, która może zostać zastąpiona w pojedynczej strony zawartości.

Na przykład strony wzorcowej widoku w wyświetlania 1 definiuje układ dwie kolumny. Zawiera dwa `<contentplaceholder>` tagów. Jeden `<ContentPlaceHolder>` dla każdej kolumny.

**1 — Lista `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

Treść widoku strony wzorcowej 1 Lista zawiera dwa `<div>` tagi, które odpowiadają dwóch kolumn. Klasa kolumny kaskadowy arkusz stylów jest stosowany do obu `<div>` tagów. Ta klasa jest zdefiniowana w arkuszu stylów zadeklarowany w górnej części strony wzorcowej. Można wyświetlić podgląd jak strony wzorcowej widoku będzie renderowany przełączyć do widoku projektu. Kliknij kartę projekt w lewym dolnym Edytor kodu źródłowego (patrz rysunek 2).


[![Wyświetlanie podglądu strony wzorcowej w Projektancie](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Rysunek 02**: wyświetlanie podglądu strony wzorcowej w Projektancie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Tworzenie widoku strony zawartości

Po utworzeniu strony wzorcowej widoku, można utworzyć co najmniej jeden widok strony zawartości na podstawie strony wzorcowej widoku. Na przykład można utworzyć indeksu widoku strony zawartości dla kontrolera głównej klikając prawym przyciskiem myszy Views\Home folder, wybierając **Dodaj, nowy element**, wybierając żądane **MVC — Wyświetl stronę zawartości** szablon wprowadzania Nazwa Index.aspx, a następnie klikając przycisk Dodaj przycisk (patrz rysunek 3).


[![Dodawanie widoku strony zawartości](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Rysunek 03**: Dodawanie widoku strony zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


Po kliknięciu przycisku Dodaj nowe okno dialogowe wyświetlany jest umożliwia wybranie strony wzorcowej widoku do skojarzenia z widoku strony zawartość (patrz rysunek 4). Można przejść do strony wzorcowej widoku Site.master utworzonej w poprzedniej sekcji.


[![Wybieranie strony wzorcowej](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Rysunek 04**: Wybieranie strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


Po utworzeniu nowej strony zawartości widoku oparte na stronie głównej Site.master otrzymasz plik wyświetlania 2.

**2 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Należy zauważyć, że ten widok zawiera `<asp:Content>` tag, który odpowiada na każdy `<asp:ContentPlaceHolder>` tagów w widoku strony wzorcowej. Każdy `<asp:Content>` znacznik zawiera atrybut ContentPlaceHolderID, który wskazuje danej `<asp:ContentPlaceHolder>` zastępujący go.

Zwróć uwagę, ponadto, czy strona Widok zawartości w wyświetlania 2 nie zawiera żadnego z normalnym tagów otwierających i zamykających HTML. Na przykład nie zawiera otwarcia i zamknięcia `<html>` lub `<head>` tagów. Wszystkie normalne otwierające i zamykające znaczniki są zawarte w widoku strony wzorcowej.

Zawartość, która ma być wyświetlany w widoku strony zawartości musi być umieszczony w `<asp:Content>` tagu. Jeśli umieścisz kod HTML lub innej zawartości poza tymi tagi, następnie wystąpi błąd przy próbie wyświetlenia strony.

Nie trzeba zastąpić co `<asp:ContentPlaceHolder>` tag ze strony wzorcowej na stronie widok zawartości. Należy zastąpić `<asp:ContentPlaceHolder>` tagów, aby zastąpić określonej zawartości tagu.

Na przykład zmodyfikowany widok indeksu w wyświetlania 3 zawiera tylko dwóch `<asp:Content>` tagów. Każdy z `<asp:Content>` tagi zawiera tekst.

**3 — lista `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Zażądano widoku w 3 wyświetlania powoduje renderowanie strony na rysunku 5. Należy zauważyć, że widok renderuje stronę z dwiema kolumnami. Zwróć uwagę, ponadto, czy zawartość ze strony zawartość widoku jest scalany z zawartości z strony wzorcowej widoku.


[![Strona zawartości widoku indeksu](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Rysunek 05**: strona zawartości widoku indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modyfikowanie zawartości strony widoku głównego

Jednego problemu, które wystąpią prawie natychmiast, podczas pracy z strony wzorcowej widoku jest problem modyfikowania zawartości strony wzorcowej widoku, gdy żądane są inny widok zawartości strony. Na przykład chcesz każdej strony w aplikacji sieci web ma unikatowy tytuł. Jednak tytuł jest zadeklarowany w widoku strony wzorcowej, a nie w widoku strony zawartość. Tak jak można dostosować dla każdej strony zawartości widoku tytuł strony?

Istnieją dwa sposoby, które można modyfikować tytuł wyświetlanych przez strony zawartości widoku. Po pierwsze można przypisać tytuł strony z atrybutem tytuł `<%@ page %>` dyrektywy zadeklarowany w górnej części strony zawartość widoku. Na przykład jeśli chcesz przypisać tytuł strony "Super doskonałe witryny sieci Web" w widoku indeksu, można dołączyć następujące dyrektywy w górnej części widoku indeksu:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Podczas renderowania widoku indeksu w przeglądarce, żądany tytuł jest wyświetlany na pasku tytułu przeglądarki:


[![Pasek tytułu w przeglądarce](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


Istnieje jedno wymaganie ważne, które muszą spełniać strony wzorcowej widoku, aby atrybut tytułu pracować. Widok strony wzorcowej musi zawierać `<head runat="server">` tag zamiast zwykłym `<head>` tag dla jej nagłówek. Jeśli `<head>` tag nie obejmuje runat = "server" atrybutu, a następnie tytuł nie będą wyświetlane. Domyślny widok strony głównej zawiera wymagane `<head runat="server">` tagu.

O innym podejściu do modyfikowania zawartości strony wzorcowej ze strony zawartości poszczególnych widoku jest opakowywać regionu, który chcesz zmodyfikować w `<asp:ContentPlaceHolder>` tagu. Załóżmy na przykład chcesz zmienić, nie tylko tytuł, ale również metatagów renderowana przez strony wzorcowej widoku. Strona widoku głównego, w przypadku wyświetlania 4 zawiera `<asp:ContentPlaceHolder>` tagów w jego `<head>` tagu.

**Wyświetlanie listy 4. `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Zwróć uwagę, że `<asp:ContentPlaceHolder>` znacznik na listę 4 zawiera zawartości domyślnej: domyślny tytuł i tagi meta domyślne. Jeśli nie można zastąpić `<asp:ContentPlaceHolder>` tagu w poszczególnych widoku strony zawartości, a następnie zawartość domyślna będzie wyświetlana.

Strona Widok zawartości w listę 5 zastępuje `<asp:ContentPlaceHolder>` tag, aby wyświetlić niestandardowe tytuł i niestandardowe tagi meta.

**Wyświetlanie listy 5 — `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Podsumowanie

W tym samouczku wyposażone podstawowe wprowadzenie można wyświetlić strony wzorcowej oraz strony z zawartością. Przedstawiono sposób tworzenia nowego widoku strony wzorcowe i utworzyć widok strony zawartości na ich podstawie. Również zbadane, jak możesz zmodyfikować zawartość strony wzorcowej widoku z konkretnym widoku strony zawartość.

> [!div class="step-by-step"]
> [Poprzednie](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [dalej](passing-data-to-view-master-pages-vb.md)
