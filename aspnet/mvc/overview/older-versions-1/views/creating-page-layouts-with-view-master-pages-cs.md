---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Tworzenie układów strony za pomocą stron wzorcowych widoku (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku dowiesz się, jak utworzyć wspólne układ strony dla wielu stron w aplikacji, korzystając z widoku strony wzorcowe. Możesz użyć...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: acf304d0f1cf349f7e9c30acb9271e602a47d3d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755064"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Tworzenie układów strony za pomocą stron wzorcowych widoku (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> W tym samouczku dowiesz się, jak utworzyć wspólne układ strony dla wielu stron w aplikacji, korzystając z widoku strony wzorcowe. Wyświetl stronę wzorcową, można użyć na przykład, aby zdefiniować układ strony dwie kolumny i użyj układu dwie kolumny dla wszystkich stron w aplikacji sieci web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Tworzenie układów strony za pomocą stron wzorcowych widoku

W tym samouczku dowiesz się, jak utworzyć wspólne układ strony dla wielu stron w aplikacji, korzystając z widoku strony wzorcowe. Wyświetl stronę wzorcową, można użyć na przykład, aby zdefiniować układ strony dwie kolumny i użyj układu dwie kolumny dla wszystkich stron w aplikacji sieci web.

Możesz również korzystać z zalet widoku strony wzorcowe, aby udostępnić zawartość, typowe na wielu stronach w aplikacji. Na przykład można umieścić swoje logo witryny sieci Web, łączy nawigacji i anonse transparent w widoku strony wzorcowej. W ten sposób każdej strony w aplikacji będą automatycznie wyświetlenia tej zawartości.

W tym samouczku dowiesz się, jak tworzyć nowe strony wzorcowej widoku i utworzyć nową stronę zawartości widoku na podstawie strony wzorcowej.

### <a name="creating-a-view-master-page"></a>Tworzenie stron wzorcowych widoku

Zacznijmy od utworzenia widoku strony wzorcowej, który definiuje układ dwie kolumny. Można dodać nowej strony wzorcowej widoku do projektu MVC przez kliknięcie prawym przyciskiem myszy Views\Shared folder wybranie opcji menu **Dodaj, nowy element**i wybierając polecenie **strony wzorcowej widoku MVC** szablonu (patrz rysunek 1).


[![Dodawanie widoku strony wzorcowej](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Rysunek 01**: Dodawanie strony wzorcowej widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


W aplikacji, można utworzyć więcej niż jednej strony wzorcowej widoku. Każdej strony wzorcowej widoku można zdefiniować układ innej strony. Można na przykład, niektóre strony, aby układ dwie kolumny i innych stron na układ trzy kolumny.

Na stronie wzorcowej widoku wygląda bardzo podobnie standardowy widok ASP.NET MVC. Jednak w przeciwieństwie do normalnego widoku strony wzorcowej widoku zawiera jeden lub więcej `<asp:ContentPlaceHolder>` tagów. `<contentplaceholder>` Znaczniki są używane do oznaczania obszarów stronę wzorcową, którą można zastąpić w poszczególnych stron zawartości.

Na przykład strony wzorcowej widoku w ofercie 1 definiuje układ dwie kolumny. Zawiera dwa `<contentplaceholder>` tagów. Jeden `<ContentPlaceHolder>` dla każdej kolumny.

**1 — Lista `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Treść widoku strony wzorcowej w ofercie 1 zawiera dwa `<div>` tagi, które odpowiadają dwie kolumny. Klasa kolumny kaskadowy arkusz stylów jest stosowana do obu `<div>` tagów. Ta klasa jest zdefiniowana w arkuszu stylów zadeklarować w górnej części strony wzorcowej. Możesz obejrzeć, jak strony wzorcowej widoku będzie renderowany przez przełączanie do widoku projektu. Kliknij kartę projekt, w lewym dolnym rogu Edytor kodu źródłowego (patrz rysunek 2).


[![Podgląd strony wzorcowej w Projektancie](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Rysunek 02**: Podgląd strony wzorcowej w Projektancie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Tworzenie widoku strony zawartości

Po utworzeniu strony wzorcowej widoku, można utworzyć co najmniej jeden widok zawartości stron w oparciu o strony wzorcowej widoku. Na przykład można utworzyć stronę zawartości widoku indeksu dla kontrolera głównego, klikając prawym przyciskiem myszy Views\Home folder, wybierając **Dodaj, nowy element**, wybierając opcję **strona zawartości widoku składnika MVC** szablonu, wprowadzając Nazwa Index.aspx i klikając **Dodaj** przycisku (zobacz rysunek 3).


[![Dodawanie strony zawartości widoku](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Rysunek 03**: Dodawanie strony zawartości widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Po kliknięciu przycisku Dodaj nowe zostanie wyświetlone okno dialogowe umożliwia wybranie do skojarzenia z poziomu strony zawartości widoku strony wzorcowej widoku (zobacz rysunek 4). Możesz przejść do strony wzorcowej widoku Site.master, które zostały utworzone w poprzedniej sekcji.


[![Wybieranie strony wzorcowej](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Rysunek 04**: Wybieranie strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Po utworzeniu nowej strony zawartości widoku na podstawie strony wzorcowej Site.master, można pobrać pliku w ofercie 2.

**2 — Lista `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Należy zauważyć, że ten widok zawiera `<asp:Content>` tag, który odnosi się do każdego z `<asp:ContentPlaceHolder>` tagów w widoku strony wzorcowej. Każdy `<asp:Content>` znacznik obejmuje atribut ContentPlaceHolderID, który wskazuje na określonych `<asp:ContentPlaceHolder>` , zastępuje ona.

Zwróć uwagę, oprócz tego, czy strona Widok zawartości w ofercie 2 nie zawiera żadnego z normalnym otwierającym i zamykającym HTML. Na przykład, nie zawiera otwierającym i zamykającym `<html>` lub `<head>` tagów. Wszystkie normalne otwierającym i zamykającym znajdują się w widoku strony wzorcowej.

Zawartość, która ma być wyświetlany w widoku strony zawartości musi być umieszczony w `<asp:Content>` tagu. Jeśli umieścisz kod HTML lub inną zawartość poza te znaczniki, następnie otrzymasz błąd przy próbie wyświetlenia strony.

Nie trzeba zastąpić co `<asp:ContentPlaceHolder>` tagu z strony wzorcowej na stronie widok zawartości. Wystarczy zastąpić `<asp:ContentPlaceHolder>` tag zastąpić znacznik przy użyciu określonej zawartości.

Na przykład zmodyfikowanego widoku indeksu w ofercie 3 zawiera tylko dwa `<asp:Content>` tagów. Każdy z `<asp:Content>` tagi zawiera jakiś tekst.

**3 — lista `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Zażądano widoku w ofercie 3 powoduje wyświetlenie strony na rysunku 5. Należy zauważyć, że widok renderuje stronę zawierającą dwie kolumny. Zwróć uwagę, co więcej, że zawartość z poziomu strony zawartości widoku jest scalany z zawartości z strony wzorcowej widoku


[![Strona zawartości widoku indeksu](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Rysunek 05**: stronę zawartości widoku indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modyfikowanie zawartości stron wzorcowych widoku

Jednym problemem, który wystąpi niemal natychmiast, podczas pracy ze stron wzorcowych widoku jest problem modyfikowania zawartości strony wzorcowej widoku zleconą inny widok strony z zawartością. Na przykład chcesz każdej strony w aplikacji sieci web, aby mieć unikatowy tytuł. Jednak tytuł jest zadeklarowany w widoku strony wzorcowej a nie w zawartości strony widoku. Tak jak możesz dostosować tytuł strony dla każdej strony zawartości widoku?

Istnieją dwa sposoby, które można modyfikować tytuł wyświetlanych przez stronę zawartości widoku. Po pierwsze można przypisać tytuł strony, aby atrybut tytułu `<%@ page %>` dyrektywy zadeklarowane w górnej części strony zawartości widoku. Na przykład jeśli chcesz przypisać tytuł strony "Super doskonałe witryny sieci Web" w widoku indeksu, można dołączyć następującą dyrektywę w górnej części widoku indeksu:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Podczas renderowania widoku indeksu do przeglądarki, żądany tytuł jest wyświetlany na pasku tytułu w przeglądarce:


[![Pasek tytułu w przeglądarce](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Istnieje jeden ważny zapotrzebowania, które strony wzorcowej widoku musi spełniać, aby atrybut tytułu do pracy. Musi zawierać strony wzorcowej widoku `<head runat="server">` tag zamiast normalnego `<head>` tag w przypadku jej nagłówek. Jeśli `<head>` tagu nie zawiera runat = atrybut "server", a następnie tytuł nie będą wyświetlane. Domyślny widok strony wzorcowej obejmuje wymagana `<head runat="server">` tagu.

Alternatywnym podejściem do modyfikowania zawartości strony wzorcowej ze strony zawartość poszczególnych widok jest opakowanie region, który chcesz zmodyfikować w `<asp:ContentPlaceHolder>` tagu. Na przykład Wyobraź sobie, że chcesz zmienić, nie tylko tytuł, ale także metatagów renderowany przez strony wzorcowej widoku. Strona widoku głównego w ofercie 4 zawiera `<asp:ContentPlaceHolder>` tagów w ramach jego `<head>` tagu.

**4 — lista `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Należy zauważyć, że `<asp:ContentPlaceHolder>` znacznik w ofercie 4 zawiera zawartość domyślna: tytuł domyślny i tagi meta domyślne. Jeśli nie zastąpisz to `<asp:ContentPlaceHolder>` tagu na stronie zawartości poszczególnych widoku, a następnie wyświetli zawartość domyślna.

Strona Widok zawartości w ofercie 5 zastępuje `<asp:ContentPlaceHolder>` tag, aby wyświetlić niestandardowy tytuł i niestandardowe tagi meta.

**5 — lista `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Podsumowanie

W tym samouczku został udostępniony wstęp można wyświetlić strony wzorcowej oraz strony z zawartością. Pokazaliśmy ci, jak utworzyć nowy widok stron wzorcowych i utworzyć widok strony zawartości na ich podstawie. Także zbadać, jak możesz zmodyfikować zawartość strony wzorcowej widoku z konkretnym widoku strony zawartości.

> [!div class="step-by-step"]
> [Poprzednie](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [dalej](passing-data-to-view-master-pages-cs.md)
