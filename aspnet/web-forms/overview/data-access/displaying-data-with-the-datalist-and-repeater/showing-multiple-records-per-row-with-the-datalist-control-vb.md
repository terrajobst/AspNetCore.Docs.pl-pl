---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: "Wyświetlanie wielu rekordów w wierszu o formant DataList (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku krótkich firma Microsoft będzie Poznaj sposób dostosowywania układu DataList za pośrednictwem jego właściwości RepeatColumns i RepeatDirection."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 416178533f022f2a262799e6f042d6009bb9d999
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Wyświetlanie wielu rekordów w wierszu o formant DataList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) lub [pobierania plików PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> W tym samouczku krótkich firma Microsoft będzie Poznaj sposób dostosowywania układu DataList za pośrednictwem jego właściwości RepeatColumns i RepeatDirection.


## <a name="introduction"></a>Wprowadzenie

Przykłady DataList możemy Zapisz w ciągu ostatnich dwóch samouczki spowodował, że każdy rekord od źródła danych jako wiersz w HTML pojedynczej kolumny `<table>`. Jest to domyślne zachowanie DataList, jest bardzo proste dostosować wyświetlanie DataList w taki sposób, że elementy źródeł danych są rozkładane między wieloma kolumnami, wielu wierszy tabeli. Ponadto go s możliwe wszystkie dane źródłowe elementy wyświetlane w pojedynczy wiersz, wielokolumnowego elementu DataList.

Możemy dostosować układ DataList s za pośrednictwem jego `RepeatColumns` i `RepeatDirection` właściwości, które odpowiednio oznacza liczbę kolumn są renderowane i określa, czy te elementy zostały przedstawione w pionie lub poziomie. Przedstawiono na rysunku 1, na przykład DataList, który wyświetla informacje o produkcie w tabeli zawierającej trzy kolumny.


[![Elementu DataList zawiera trzy produkty w wierszu](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Rysunek 1**: DataList zawiera trzy produkty wierszu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Pokazując wiele elementów źródła danych na wiersz, miejsca na ekranie poziome efektywniejsze może korzystać z elementu DataList. W tym samouczku krótkich firma Microsoft będzie Poznaj te dwie właściwości elementu DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Krok 1: Wyświetlanie informacji o produkcie w DataList.

Przed omówione `RepeatColumns` i `RepeatDirection` właściwości let s najpierw utworzyć DataList na naszą stronę, która zawiera informacje o produkcie przy użyciu standardowej tabeli pojedynczej kolumny, wielowierszowych układu. Na przykład zezwolić s są wyświetlane nazwy s produktu, kategorii i cen za pomocą następujących znaczników:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Firma Microsoft kolejnych przedstawiono sposób powiązać danych DataList w poprzednich przykładach, można szybko przeniesienie tych kroków. Uruchamianie przez otwarcie `RepeatColumnAndDirection.aspx` strony `DataListRepeaterBasics` folder i przeciągnij DataList z przybornika do projektanta. Z tagów inteligentnych DataList s zdecydować się na tworzenie nowego elementu ObjectDataSource i skonfigurować go do swoich danych z `ProductsBLL` klasy s `GetProducts` metody (Brak) opcja z Kreatora s INSERT, UPDATE i usuwanie kart.

Po utworzeniu i powiązanie ObjectDataSource nowego elementu DataList, Visual Studio automatycznie utworzy `ItemTemplate` który wyświetla nazwę i wartość dla każdego pola danych produktu. Dostosuj `ItemTemplate` bezpośrednio za pomocą deklaratywne znaczników lub Edytuj szablony w przystawce tagów inteligentnych DataList s tak, aby korzystała znaczników pokazano powyżej, zastępując *nazwa produktu*, *nazwa kategorii* , i *cen* tekst za pomocą formantów etykiet, korzystających ze składni odpowiednie wiązania z danymi można przypisać wartości do ich `Text` właściwości. Po zaktualizowaniu `ItemTemplate`, Twoje s deklaratywne znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Powiadomienie tej I kolejnych uwzględnione specyfikatora formatu w `Eval` składnia wiązania z danymi `UnitPrice`, formatowanie zwrócona wartość jako walutę -`Eval("UnitPrice", "{0:C}").`

Poświęć chwilę, aby odwiedzić stronę w przeglądarce. Jak pokazano na rysunku 2, jako pojedynczej kolumny, wielowierszowych spis produktów powoduje renderowanie elementu DataList.


[![Domyślnie renderuje DataList jako pojedynczej kolumny, wielu wiersza tabeli](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Rysunek 2**: Domyślnie, DataList renderuje jako pojedynczej kolumny, tabeli wielowierszowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Krok 2: Zmiana kierunku układu DataList s

Podczas domyślne zachowanie dla elementu DataList jest układ jego elementy w pionie w tabeli pojedynczej kolumny, wielowierszowych to zachowanie można łatwo zmienić za pośrednictwem DataList s [ `RepeatDirection` właściwości](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Właściwości może zaakceptować jedną z dwóch wartości: `Horizontal` lub `Vertical` (ustawienie domyślne).

Zmieniając `RepeatDirection` właściwość z `Vertical` do `Horizontal`, DataList renderuje jego rekordów w jednym wierszu, tworzenie jedną kolumnę na element źródła danych. Aby zilustrować ten efekt, kliknij na liście DataList w projektancie, a następnie w oknie Właściwości zmień `RepeatDirection` właściwość z `Vertical` do `Horiztonal`. Natychmiast po zrobieniu tego, Projektant dostosowuje układ DataList s Tworzenie interfejsu w jednym wierszu, wielokolumnowego (patrz rysunek 3).


[![Elementy RepeatDirection właściwość określa, jak kierunek DataList s są określone limit](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Rysunek 3**: `RepeatDirection` właściwość decyduje, jak są elementy kierunek DataList s, którego układ określa się ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Podczas wyświetlania małe ilości danych w jednym wierszu tabeli wielokolumnowego może być idealny w celu zmaksymalizowania wykorzystania nieruchomości ekranu. Dla większych ilości danych jednak pojedynczy wiersz będzie wymagać wiele kolumn, które umieszcza te elementy tego t może mieści się na ekranie poza z prawej strony. Rysunek 4 przedstawia produkty, gdy są renderowane w DataList pojedynczy wiersz. Ponieważ wiele produktów (ponad 80), użytkownik będzie musiał daleko przewiń w prawo, aby wyświetlić informacje dotyczące każdego produktu.


[![Dla źródeł danych wystarczająco duże DataList pojedyncza kolumna będzie wymagać przewijanie w poziomie](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Rysunek 4**: dla wystarczająco dużą źródeł danych, jednej kolumny DataList będzie wymagać przewijanie w poziomie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Krok 3: Wyświetlanie danych w tabeli wielokolumnowego, wielowierszowych

Aby utworzyć DataList wielokolumnowego, wielowierszowych, musimy ustawić [ `RepeatColumns` właściwości](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) do liczby kolumn do wyświetlenia. Domyślnie `RepeatColumns` właściwość jest ustawiona na 0, co spowoduje, że DataList wyświetlić wszystkie jego elementy w jeden wiersz lub kolumnę (w zależności od wartości `RepeatDirection` właściwości).

W naszym przykładzie umożliwiają s wyświetlane trzy produktów dla każdego wiersza tabeli. W związku z tym ustawić `RepeatColumns` właściwości do 3. Po wprowadzeniu tej zmiany należy Poświęć chwilę, aby wyświetlić wyniki w przeglądarce. Jak pokazano na rysunku nr 5, produkty są teraz wymienione w tabeli trzy kolumny, wielu wierszy.


[![Trzy produkty są wyświetlane w wierszu](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Rysunek 5**: trzy produkty są wyświetlane w wierszach ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection` Właściwość ma wpływ na sposób układ elementów w elementu DataList. Rysunek 5. pokazuje wyniki z `RepeatDirection` ustawioną właściwość `Horizontal`. Należy pamiętać, że pierwsze trzy produkty Chai zmian i syropu anyżem układ od lewej do prawej strony, od góry do dołu. Produkty trzech kolejnych (począwszy od Jacka Chef s Cajun Seasoning) są wyświetlane w wierszu poniżej pierwsze trzy. Zmiana `RepeatDirection` właściwości z powrotem do `Vertical`, jednak wychodzi poza tymi produktami od góry do dołu, od lewej do prawej, jak pokazano na rysunku 6.


[![Te produkty są w tym miejscu, którego układ określa się pionowo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Rysunek 6**: w tym miejscu, którego układ określa się pionowo produkty są ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Liczba wierszy wyświetlanych w tabeli wynikowej zależy od liczby całkowitej rekordów powiązany z elementu DataList. Mówiąc, jego s limitu całkowitej liczby elementów źródła danych podzielonej przez `RepeatColumns` wartości właściwości. Ponieważ `Products` tabeli ma obecnie 84 produktów, która jest podzielnych przez 3, 28 wierszy. Jeśli liczba elementów w źródle danych i `RepeatColumns` wartość właściwości nie są podzielić, a następnie ostatni wiersz lub kolumnę mają puste komórki. Jeśli `RepeatDirection` ma ustawioną wartość `Vertical`, a następnie w ostatniej kolumnie będą miały puste komórki; Jeśli `RepeatDirection` jest `Horizontal`, ostatni wiersz będzie mieć puste komórki.

## <a name="summary"></a>Podsumowanie

DataList, domyślnie zawiera elementy w pojedynczej kolumny, wielu wierszy tabeli, która naśladuje układ widoku GridView z jednym TemplateField. Ten układ domyślny jest dopuszczalne, możemy zmaksymalizować nieruchomości ekranu za pomocą wyświetlania wielu elementów źródła danych w wierszu. To jest po prostu ustawienie DataList s `RepeatColumns` właściwości Liczba kolumn do wyświetlenia w każdym wierszu. Ponadto DataList s `RepeatDirection` właściwości można wskazać, czy zawartość tabeli wielokolumnowego, wielowierszowych należy określić poziomo od lewej do prawej, od góry do dołu lub pionowo z góry do dołu, od lewej do prawej.

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Suru Jan. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
[dalej](nested-data-web-controls-vb.md)
