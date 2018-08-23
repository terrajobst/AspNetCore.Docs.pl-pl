---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Wyświetlanie wielu rekordów w wierszu za pomocą kontrolki DataList (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Temu krótkiemu samouczkowi Przedstawimy sposób dostosowywania układu DataList za pośrednictwem jego właściwości RepeatColumns i RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 7a23e8897e4fbdf84c4d6916c22bad9687fa5ee6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753487"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Wyświetlanie wielu rekordów w wierszu za pomocą kontrolki DataList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) lub [Pobierz plik PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Temu krótkiemu samouczkowi Przedstawimy sposób dostosowywania układu DataList za pośrednictwem jego właściwości RepeatColumns i RepeatDirection.


## <a name="introduction"></a>Wprowadzenie

W przykładach DataList możemy ve występuje w ciągu ostatnich dwóch samouczkach spowodował, że każdy rekord ze źródła danych jako wiersz w HTML jednokolumnową `<table>`. Chociaż jest to domyślne zachowanie DataList, jest bardzo proste dostosować wyświetlanie DataList w taki sposób, że elementy źródeł danych są dystrybuowane między tabeli wielokolumnowej, wielowierszowych. Ponadto on możliwość wszystkie dane źródło elementów wyświetlanych w DataList jednowierszowy, wielokolumnowych.

Możemy dostosować układ DataList s za pośrednictwem jego `RepeatColumns` i `RepeatDirection` właściwości, które odpowiednio wskazać liczbę kolumn są renderowane i czy te elementy są ułożone w pionie lub poziomie. Rysunek 1, na przykład pokazuje DataList, który wyświetla informacje o produkcie w tabeli zawierającej trzy kolumny.


[![Kontrolki DataList pokazuje trzy produkty na wiersz](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Rysunek 1**: DataList pokazuje trzy produkty poszczególnych wierszy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Poprzez wyświetlanie wielu elementów źródła danych w wierszu, miejsce na ekranie poziomy bardziej efektywne może korzystać z kontrolki DataList. Te dwie właściwości DataList przyjrzymy się temu krótkiemu samouczkowi.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Krok 1: Wyświetlanie informacji o produkcie w kontrolkach DataList

Zanim analizujemy `RepeatColumns` i `RepeatDirection` właściwości umożliwiają s najpierw utworzyć kontrolką DataList na naszej stronie zawierającego informacje o produkcie przy użyciu standardowej tabeli jednokolumnowej, wielowierszowych układu. W tym przykładzie umożliwiają s wyświetlanie nazwy s produktu, kategorii i cenę za pomocą następujących znaczników:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Firma Microsoft ve już, jak powiązać dane z kontrolką DataList w poprzednich przykładach, dzięki czemu można szybko przeniesienie tych kroków. Zacznij od otwarcia `RepeatColumnAndDirection.aspx` stronie `DataListRepeaterBasics` folder i przeciągnij kontrolką DataList z przybornika do projektanta. Za pomocą kontrolek DataList s tagu inteligentnego, zoptymalizowany pod kątem do tworzenia nowego elementu ObjectDataSource i skonfigurować go do ściągania danych z `ProductsBLL` klasy s `GetProducts` metody, wybierając (Brak) opcji za pomocą Kreatora s INSERT, UPDATE i usuwanie kart.

Po utworzeniu i powiązanie nowego elementu ObjectDataSource z kontrolki DataList, Visual Studio będzie automatycznie tworzył `ItemTemplate` który wyświetla nazwę i wartość dla każdego pola danych produktu. Dostosuj `ItemTemplate` bezpośrednio w oznaczeniu deklaracyjnym lub Edytuj szablony opcję w tagu inteligentnego DataList s, tak aby używał znaczników, pokazana powyżej, zastępując *nazwa produktu*, *nazwa kategorii* , i *cena* tekstu za pomocą formantów etykiet, korzystających ze składni odpowiednie powiązanie danych można przypisać wartości do ich `Text` właściwości. Po zaktualizowaniu `ItemTemplate`, Twoje oznaczeniu deklaracyjnym strony s powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Zwróć uwagę, ve uwzględnione specyfikatora formatu w `Eval` składnia wiązania danych `UnitPrice`, zwrócona wartość jako walutę — formatowanie `Eval("UnitPrice", "{0:C}").`

Poświęć chwilę, aby odwiedzić stronę w przeglądarce. Jak pokazano na rysunku 2, kontrolki DataList renderowany jako tabelę jednokolumnową, wielowierszowych produktów.


[![Domyślnie renderuje DataList jako tabelę jednokolumnową, wielowierszowych](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Rysunek 2**: Domyślnie, DataList renderowany jako jedną kolumną, wielowierszowych tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Krok 2: Zmiana Kierunek układu kontrolek DataList s

Podczas domyślne zachowanie dla kontrolki DataList jest układ elementy w pionie w tabelę jednokolumnową, wielowierszowych to zachowanie można łatwo zmienić za pomocą kontrolek DataList s [ `RepeatDirection` właściwość](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Właściwości można zaakceptować jedną z dwóch wartości: `Horizontal` lub `Vertical` (ustawienie domyślne).

Zmieniając `RepeatDirection` właściwość `Vertical` do `Horizontal`, jego rekordów w jednym wierszu, powoduje wyświetlenie elementu DataList tworzenia jedną kolumnę na element źródła danych. Aby zilustrować ten efekt, kliknij DataList w projektancie, a następnie w oknie Właściwości zmień `RepeatDirection` właściwość `Vertical` do `Horiztonal`. Natychmiast po wykonaniu tej czynności projektanta dostosowuje układ DataList s tworzenia z pojedynczy wiersz tabeli wielokolumnowej interfejsu (zobacz rysunek 3).


[![Elementy RepeatDirection właściwości połączenia z opisywanym jak kierunek DataList s są określone w poziomie](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Rysunek 3**: `RepeatDirection` właściwość określa, jak elementy kierunku DataList s są określone w poziomie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Podczas wyświetlania małe ilości danych, pojedynczy wiersz wielokolumnowej tabeli może być to idealny sposób na powierzchnię ekranu. Dla większych ilości danych jednak pojedynczego wiersza będzie wymagać wiele kolumn, które wypchnięć te elementy tego t może mieści się na ekranie wyłączona po prawej stronie. Rysunek 4 przedstawia produktów podczas renderowania w DataList pojedynczy wiersz tabeli. Ponieważ wiele produktów (ponad 80), użytkownik będzie musiał przewiń daleko w prawo, aby wyświetlić informacje o poszczególnych produktów.


[![W przypadku źródeł danych wystarczająco dużą DataList jednokolumnową będzie wymagać przewijanie w poziomie](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Rysunek 4**: dla wystarczająco duże źródła danych, jednej kolumny DataList będzie wymagać przewijanie w poziomie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Krok 3: Wyświetlanie danych w tabeli wielokolumnowej, wielowierszowych

Aby utworzyć DataList wielokolumnowych, wielowierszowych, musimy [ `RepeatColumns` właściwość](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) do liczby kolumn do wyświetlenia. Domyślnie `RepeatColumns` właściwość jest ustawiona na 0, co spowoduje, że DataList wyświetlić wszystkie jego elementy w pojedynczy wiersz lub kolumnę (w zależności od wartości `RepeatDirection` właściwości).

W tym przykładzie umożliwiają s wyświetlane trzy produkty każdego wiersza tabeli. W związku z tym, ustaw `RepeatColumns` właściwości do 3. Po wprowadzeniu tej zmiany, Poświęć chwilę, aby wyświetlić wyniki w przeglądarce. Jak pokazano na rysunku 5, produkty są teraz wymienione w tabeli trzy kolumny, z wieloma wierszami.


[![Trzy produkty są wyświetlane w wierszu](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Rysunek 5**: trzy produkty są wyświetlane w wierszu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection` Właściwość ma wpływ na sposób są układane elementów w kontrolce DataList. Rysunek 5. pokazuje wyniki z `RepeatDirection` właściwością `Horizontal`. Należy zauważyć, że pierwsze trzy produkty Chai, zmian centralnych i Syrop anyżowy są ułożone od lewej do prawej i od góry do dołu. Następne trzy produkty (począwszy od programu Chef Jacka s Cajun Seasoning) są wyświetlane w wiersz pod pierwsze trzy. Zmiana `RepeatDirection` właściwości z powrotem do `Vertical`, jednak decydujących o tych produktów, od góry do dołu i od lewej do prawej, tak jak pokazano na rysunku 6.


[![W tym miejscu produkty są ustanowione Out w pionie](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Rysunek 6**: w tym miejscu produkty są ustanowione się pionowo ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Liczba wierszy wyświetlanych w tabeli wynikowej zależy od liczby całkowitej rekordów powiązany z kontrolki DataList. Mówiąc, jego s Zaokrąglenie w górę łączna liczba elementów źródła danych podzielona przez `RepeatColumns` wartości właściwości. Ponieważ `Products` tabela ma obecnie 84 produktów, które można podzielić przez 3, 28 wierszy. Jeśli liczba elementów w źródle danych i `RepeatColumns` wartość właściwości nie są podzielna, a następnie ostatni wiersz lub kolumna ma puste komórki. Jeśli `RepeatDirection` ustawiono `Vertical`, a następnie w ostatniej kolumnie będą miały puste komórki; Jeśli `RepeatDirection` jest `Horizontal`, ostatni wiersz będzie znajdować się puste komórki, a następnie.

## <a name="summary"></a>Podsumowanie

DataList, domyślnie wyświetla jego elementów w jednokolumnowej, wielowierszowych tabeli, która naśladuje układ GridView za pomocą pojedynczego TemplateField. Ten układ domyślny jest dopuszczalne, firma Microsoft może powierzchnię ekranu, wyświetlając wiele elementów źródła danych na wiersz. To jest po prostu kwestią ustawienie DataList s `RepeatColumns` liczbę kolumn w celu wyświetlenia każdego wiersza. Ponadto DataList s `RepeatDirection` właściwość może służyć do wskazania, czy zawartość tabeli wielokolumnowej, wielowierszowych powinny być rozmieszczone poziomo od lewej do prawej i od góry do dołu, czy w pionie od góry do dołu i od lewej do prawej.

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Suru Jan. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [dalej](nested-data-web-controls-vb.md)
