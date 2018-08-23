---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Aktualizowanie i usuwanie istniejących danych binarnych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W samouczkach wcześniej widzieliśmy, jak w kontrolce GridView umożliwia łatwe edytowanie i usuwanie danych tekstowych. W tym samouczku zobaczymy, jak również wprowadzić w kontrolce GridView...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a0f9e5ebb8bc8227e869f2af085394262a31663
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752897"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualizowanie i usuwanie istniejących danych binarnych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) lub [Pobierz plik PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> W samouczkach wcześniej widzieliśmy, jak w kontrolce GridView umożliwia łatwe edytowanie i usuwanie danych tekstowych. W tym samouczku zobaczymy, jak w kontrolce GridView umożliwia także do edytowania i usuwania danych binarnych, czy te dane binarne są zapisywane w bazie danych ani przechowywane w systemie plików.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich trzech samouczków firma dodano znacznej liczby funkcji do pracy z danymi binarnymi. Rozpoczęliśmy, dodając `BrochurePath` kolumny `Categories` tabeli, a następnie odpowiednio aktualizowana architektury. Dodaliśmy również metody warstwy dostępu do danych i warstwy logiki biznesowej do pracy z istniejącymi kategoriami tabeli s `Picture` kolumny, która przechowuje binarne s zawartość pliku obrazu. Utworzyliśmy stron sieci web do prezentowania danych binarnych w GridView link pobierania dla brochure, za pomocą kategorii s obrazie w `<img>` elementu i mieć dodane DetailsView, aby użytkownicy mogli dodać nową kategorię i przekazać swoje dane broszura i obraz.

Do zaimplementowania pozostaje możliwość edytowania i usuwania istniejących kategorii, które opisano w tym samouczku przy użyciu GridView s wbudowanych edycji i usuwania funkcji. Podczas edytowania kategorii, użytkownik będzie mógł opcjonalnie Przekaż nowy obraz lub należą do kategorii, w dalszym ciągu używać istniejącej. Dla brochure ich albo można używać istniejących brochure, można przekazać nowe broszura lub aby wskazać, że kategorii nie ma już broszurę skojarzonych z nim. Rozpocznij pracę dzięki s!

## <a name="step-1-updating-the-data-access-layer"></a>Krok 1: Trwa aktualizowanie warstwy dostępu do danych

Warstwy DAL został wygenerowany automatycznie `Insert`, `Update`, i `Delete` metody, ale te metody zostały wygenerowane na podstawie `CategoriesTableAdapter` główne zapytanie s, która nie obejmuje `Picture` kolumny. W związku z tym `Insert` i `Update` metody nie dołączaj parametrów do określenia danych binarnych dla obrazu kategorii s. Tak jak Robiliśmy to [poprzedni Samouczek](including-a-file-upload-option-when-adding-a-new-record-vb.md), należy utworzyć nowe metody TableAdapter aktualizowanie `Categories` tabeli podczas określania danych binarnych.

Otwórz zestaw danych wpisane, a przy użyciu projektanta, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` nagłówka s i wybierz polecenie Dodaj zapytanie z menu kontekstowego do launche Kreatora konfiguracji zapytań TableAdapter. Ten Kreator uruchamia się z pytaniem jak zapytania TableAdapter powinien uzyskiwać dostęp do bazy danych. Wybierz opcję Użyj instrukcji SQL, a następnie kliknij przycisk Dalej. Następnym krokiem monituje o podanie typu zapytania do wygenerowania. Ponieważ firma Microsoft, ponownie tworząc zapytanie, aby dodać nowy rekord do `Categories` tabeli, wybierz AKTUALIZACJĘ i kliknij przycisk Dalej.


[![Wybierz opcję aktualizacji](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Rysunek 1**: wybierz opcję aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Teraz musisz określić `UPDATE` instrukcji SQL. Kreator automatycznie sugeruje `UPDATE` instrukcji odpowiadający główne zapytanie TableAdapter s (jedną, która aktualizuje `CategoryName`, `Description`, i `BrochurePath` wartości). Zmień instrukcję, aby `Picture` kolumny jest dołączana do wzdłuż `@Picture` parametru, w następujący sposób:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Na ostatnim ekranie kreatora pyta, czy nam nadaj nazwę nowej metody TableAdapter. Wprowadź `UpdateWithPicture` i kliknij przycisk Zakończ.


[![Nazwa nowego UpdateWithPicture metody TableAdapter](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Rysunek 2**: nadaj nazwę nowej metody TableAdapter `UpdateWithPicture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Krok 2: Dodawanie metod warstwy logiki biznesowej

Oprócz aktualizacji warstwy DAL, musimy zaktualizować LOGIKI obejmujący metody, aktualizowania i usuwania kategorii. Są to metody, które zostaną wywołane z warstwy prezentacji.

Do usuwania kategorii, możemy użyć `CategoriesTableAdapter` s generowanych automatycznie `Delete` metody. Dodaj następującą metodę do `CategoriesBLL` klasy:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

W tym samouczku umożliwiają s utworzyć dwie metody aktualizacji kategorii — taki, który oczekuje, że dane binarne obrazu, a następnie wywołuje `UpdateWithPicture` metoda właśnie dodaliśmy, aby `CategoriesTableAdapter` i drugi akceptujący tylko `CategoryName`, `Description`i `BrochurePath`wartości i używa `CategoriesTableAdapter` klasy s generowanych automatycznie `Update` instrukcji. Uzasadnieniem przy użyciu dwóch metod jest, że w niektórych sytuacjach użytkownika mogą zostać zaktualizowane obrazu s kategorii wraz z jego innych pól, w których przypadku użytkownik musi przekazać nowy obraz. Dane binarne przekazany obraz s mogą następnie służyć w `UPDATE` instrukcji. W innych przypadkach użytkownik może być tylko zainteresowana aktualizacji i powiedz, nazwę i opis. Ale w tym przypadku `UPDATE` instrukcji oczekuje, że dane binarne `Picture` także kolumny, a następnie możemy d należy podać te informacje, jak również. To wymaga dodatkowej podróży do bazy danych, aby przywrócić dane obrazu dla rekordu edytowany. W związku z tym, chcemy, aby dwa `UPDATE` metody. Warstwy logiki biznesowej określi, co umożliwia oparte na tego, czy obraz podano danych podczas aktualizacji kategorii.

Aby ułatwić to zadanie, należy dodać dwie metody `CategoriesBLL` klasy zarówno o nazwie `UpdateCategory`. Pierwsza z nich powinna obsługiwać trzy `String` s, `Byte` tablicy, a `Integer` jako dane wejściowe parametry; druga Strona, po prostu trzy `String` s i `Integer`. `String` Parametry wejściowe są nazwa kategorii s, opis i ścieżkę pliku broszura, `Byte` tablica jest binarny zawartości obrazu kategorii s i `Integer` identyfikuje `CategoryID` rekordu do zaktualizowania. Zwróć uwagę, że pierwsze przeciążenie wywołuje drugi jeśli przekazany do `Byte` tablica jest `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Krok 3: Skopiowanie Insert i funkcji widoku

W [poprzedni Samouczek](including-a-file-upload-option-when-adding-a-new-record-vb.md) utworzyliśmy stronę o nazwie `UploadInDetailsView.aspx` wyświetlane wszystkie kategorie GridView i podano DetailsView do dodawania nowych kategorii do systemu. W tym samouczku rozszerzmy GridView obejmujący edytowania i usuwania pomocy technicznej. Zamiast dalszą pracę z `UploadInDetailsView.aspx`, umożliwiają s zamiast tego umieść ten samouczek s zmiany `UpdatingAndDeleting.aspx` strony z tego samego folderu `~/BinaryData`. Skopiuj i Wklej oznaczeniu deklaracyjnym i kod od `UploadInDetailsView.aspx` do `UpdatingAndDeleting.aspx`.

Zacznij od otwarcia `UploadInDetailsView.aspx` strony. Kopiuj wszystko składni deklaratywnej w ramach `<asp:Content>` elementu, jak pokazano na rysunku 3. Następnie otwórz `UpdatingAndDeleting.aspx` i wklej ten kod znaczników w ramach jego `<asp:Content>` elementu. Podobnie, skopiować kod z `UploadInDetailsView.aspx` stronie klasy CodeBehind s `UpdatingAndDeleting.aspx`.


[![Skopiuj oznaczeniu deklaracyjnym z UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Rysunek 3**: Skopiuj oznaczeniu deklaracyjnym z `UploadInDetailsView.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Po skopiowaniu ciągu deklaratywne znaczników i kodu, odwiedź stronę `UpdatingAndDeleting.aspx`. Powinna być wyświetlana takie same dane wyjściowe oraz ma tego samego środowiska użytkownika podobnie jak w przypadku `UploadInDetailsView.aspx` stronę z poprzednim samouczku.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Krok 4: Dodawanie, usuwanie pomoc techniczną do kontrolki ObjectDataSource i widoku GridView

Tak jak Omówiliśmy to w [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczku widoku GridView zapewnia wbudowane możliwości usuwania i te funkcje można włączyć w znaczników pola wyboru, gdy podstawowy siatki s źródło danych obsługuje usuwania. Obecnie kontrolki ObjectDataSource widoku GridView jest powiązany z (`CategoriesDataSource`) nie obsługuje usuwania.

Aby rozwiązać ten problem, należy kliknąć opcję konfigurowania źródła danych za pomocą kontrolki ObjectDataSource tagu inteligentnego s, aby uruchomić kreatora. Pierwszy ekran pokazuje, że kontrolki ObjectDataSource jest skonfigurowany do pracy z `CategoriesBLL` klasy. Wprowadzam polecenie dalej. Obecnie tylko kontrolki ObjectDataSource s `InsertMethod` i `SelectMethod` są określone właściwości. Jednak Kreator automatycznie wypełniony list rozwijanych w AKTUALIZOWANIE i usuwanie kart za pomocą `UpdateCategory` i `DeleteCategory` metod, odpowiednio. Jest to spowodowane w `CategoriesBLL` możemy oznaczone te metody przy użyciu klasy `DataObjectMethodAttribute` jako domyślnych metod aktualizowania i usuwania.

Teraz Zmień wartość na liście rozwijanej aktualizacji karty s (Brak), ale pozostawić listy rozwijanej kartę s DELETE równa `DeleteCategory`. Wrócimy do tego kreatora, w kroku 6 w celu dodania obsługi aktualizacji.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource do użycia `DeleteCategory` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Po ukończeniu kreatora, Visual Studio może wystąpić, jeśli chcesz odświeżyć pola i klucze, który spowoduje to ponowne wygenerowanie danych sieci Web kontroluje pola. Wybierz opcję nie, ponieważ wybranie opcji Tak spowoduje zastąpienie wszelkich dostosowań pola, które mogły zostać wprowadzone.


Kontrolki ObjectDataSource będzie teraz obejmować wartości jego `DeleteMethod` właściwości, a także `DeleteParameter`. Pamiętaj, że podczas korzystania z kreatora, aby określić metody, program Visual Studio ustawia ObjectDataSource s `OldValuesParameterFormatString` właściwość `original_{0}`, która powoduje występowanie problemów z aktualizacją i Usuń wywołania metody. W związku z tym, całkowicie wyczyszczenie tej właściwości lub je zresetować do ustawień domyślnych `{0}`. Jeśli musisz odświeżyć sobie pamięć dla tej właściwości kontrolki ObjectDataSource, zobacz [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczka.

Po zakończeniu pracy kreatora i naprawianie `OldValuesParameterFormatString`, s ObjectDataSource o oznaczeniu deklaracyjnym zawartość okna powinna wyglądać podobnie do poniższego:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Po skonfigurowaniu kontrolki ObjectDataSource, należy dodać możliwości usuwania do kontrolki GridView, zaznaczając pole wyboru Włącz usuwanie z tagu inteligentnego s GridView. Spowoduje to dodanie do widoku GridView CommandField którego `ShowDeleteButton` właściwość jest ustawiona na `True`.


[![Włącz obsługę usuwania w widoku GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Rysunek 5**: Włączanie obsługi usuwanie w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Poświęć chwilę, w celu przetestowania funkcji usuwania. Brak klucza obcego między `Products` tabeli s `CategoryID` i `Categories` tabeli s `CategoryID`, dzięki czemu otrzymasz wyjątek naruszenie ograniczenia klucza obcego, przy próbie usunięcia znajdujących się w pierwszych osiem kategorii. Aby przetestować tę funkcję w poziomie, Dodaj nową kategorię, podawania broszura i obraz. Moje kategorii testów, przedstawiono na rysunku 6 zawiera plik broszura testu o nazwie `Test.pdf` i obraz testowy. Rysunek nr 7 przedstawia widoku GridView po dodaniu kategorii testów.


[![Dodaj kategorię testów przy użyciu broszura i obrazu](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Rysunek 6**: Dodaj kategorię testów przy użyciu broszura i obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Po wstawieniu kategorii testów, jest wyświetlany w widoku GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Rysunek 7**: po wstawieniu kategorii testów, jest wyświetlany w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


W programie Visual Studio należy odświeżyć Eksploratora rozwiązań. Powinien zostać wyświetlony nowy plik w `~/Brochures` folderze `Test.pdf` (zobacz rysunek 8).

Następnie kliknij łącze Usuń, w wierszu kategorii testów, powodując strony aby ogłaszanie zwrotne i `CategoriesBLL` klasy s `DeleteCategory` metoda ognia. Spowoduje to wywołanie DAL s `Delete` metody, powodując odpowiednie `DELETE` instrukcji do wysłania do bazy danych. Dane są następnie odbitych do kontrolki GridView i znaczników jest wysyłane z powrotem do klienta za pomocą kategorii testów nie jest już obecny.

Podczas usuwania przepływu pracy został pomyślnie usunięty rekord kategorii testów z `Categories` tabeli nie został usunięty jego pliku broszura w systemie plików serwera s sieci web. Odśwież Eksploratora rozwiązań, a zobaczysz, że `Test.pdf` nadal znajdują się `~/Brochures` folderu.


![Plik Test.pdf nie został usunięty z systemu plików s serwera sieci Web](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Rysunek 8**: `Test.pdf` plik nie został usunięty z systemu plików w sieci Web serwera s


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Krok 5: Usuwanie pliku broszura s usunięto kategorię

Jest jedną z wad przechowywania danych binarnych zewnętrzne w stosunku do bazy danych, czy dodatkowe kroki należy wyczyścić te pliki po usunięciu rekordu skojarzonej bazy danych. GridView i kontrolki ObjectDataSource zapewniają zdarzenia, które są uruchamiane przed i po wykonał polecenie Usuń. Faktycznie musimy utworzyć procedury obsługi zdarzeń dla zdarzeń przed i po działaniu. Przed `Categories` rekord zostanie usunięty należy określić jego ścieżki do pliku s PDF, ale możemy don t chcesz usunąć plik PDF, przed usunięciem tej kategorii, w przypadku, gdy istnieje kilka wyjątków, a kategorii nie jest usuwany.

GridView s [ `RowDeleting` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) uruchamiany przed polecenie Usuń s ObjectDataSource zostało wywołane, podczas gdy jej [ `RowDeleted` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) uruchamia się po. Utwórz obsługę zdarzeń dla tych dwóch zdarzeń, używając następującego kodu:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

W `RowDeleting` programu obsługi zdarzeń `CategoryID` wiersza, usuwany jest pobierany z GridView s `DataKeys` kolekcji, która może być dostępne w tej obsługi zdarzeń za pośrednictwem `e.Keys` kolekcji. Następnie `CategoriesBLL` klasy s `GetCategoryByCategoryID(categoryID)` wywoływana w celu zwracania informacji o rekordzie usuwany. Jeśli zwrócony `CategoriesDataRow` obiekt ma innej niż`NULL``BrochurePath` wartości, a następnie jest on przechowywany w zmiennej strony `deletedCategorysPdfPath` , dzięki czemu można usunąć pliku `RowDeleted` programu obsługi zdarzeń.

> [!NOTE]
> Zamiast pobierania `BrochurePath` szczegóły `Categories` rejestrowania usuwane w `RowDeleting` programu obsługi zdarzeń można też dodaliśmy `BrochurePath` s GridView `DataKeyNames` właściwości i dostępne wartości rekordu s za pomocą `e.Keys` kolekcji. To może nieco zwiększyć rozmiar stanu widoku s GridView, ale będzie zmniejszenie ilości kodu i Zapisz podróż do bazy danych.


Po wywołaniu bazowe polecenie Usuń s kontrolki ObjectDataSource GridView s `RowDeleted` uruchamiany program obsługi zdarzeń. Brak wyjątków podczas usuwania danych, jeśli ma wartości `deletedCategorysPdfPath`, a następnie plik PDF jest usuwany z systemu plików. Należy pamiętać, że ten dodatkowy kod nie jest potrzebna do czyszczenia danych binarnych s kategorii, skojarzone z jego obraz. Tego s, ponieważ dane obrazu są przechowywane bezpośrednio w bazie danych, dlatego usunięcie `Categories` wierszy spowoduje również usunięcie tych kategorii s danych obrazu.

Po dodaniu obsługi dwóch zdarzeń, należy ponownie uruchomić tego przypadku testowego. Podczas usuwania kategorii, jego skojarzony plik PDF są także usuwane.

Aktualizowanie istniejących danych binarnych s rekordów skojarzonych udostępnia kilka ciekawych wyzwań. W pozostałej części tego samouczka pokażcie Dodawanie możliwości aktualizacji do broszura i obraz. Krok 6 odkrywa technik aktualizacji informacji broszura podczas kroku 7 patrzy na aktualizowanie obrazu.

## <a name="step-6-updating-a-category-s-brochure"></a>Krok 6: Aktualizowanie broszura s kategorii

Zgodnie z opisem w tym samouczku Przegląd Wstawianie, aktualizowanie i usuwanie danych, widoku GridView oferuje wbudowaną obsługą edycji poziomie wiersza, który może być implementowany przez znaczników pola wyboru, jeśli źródła danych jest odpowiednio skonfigurowany. Obecnie `CategoriesDataSource` ObjectDataSource nie został jeszcze skonfigurowany do uwzględnienia, aktualizowanie pomocy technicznej, dlatego umożliwiają s dodać w.

Kliknij łącze Konfiguruj źródła danych za pomocą kontrolki ObjectDataSource kreatora s i przejdź do kroku drugim. Z powodu `DataObjectMethodAttribute` używane w `CategoriesBLL`, listy aktualizacji jest wypełniane automatycznie przy użyciu `UpdateCategory` przeciążenia, które przyjmuje cztery parametry wejściowe (dla wszystkich kolumn, ale `Picture`). Zmiany, tak aby używał przeciążenia z pięciu parametrami.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody UpdateCategory zawierającego parametr dla obrazu](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Rysunek 9**: Konfigurowanie kontrolki ObjectDataSource do użycia `UpdateCategory` metodę, która zawiera parametr `Picture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


Kontrolki ObjectDataSource będzie teraz obejmować wartości jego `UpdateMethod` właściwości, a także odpowiadającej `UpdateParameter` s. Jak wspomniano w kroku 4, program Visual Studio ustawia ObjectDataSource s `OldValuesParameterFormatString` właściwość `original_{0}` podczas korzystania z kreatora Konfigurowanie źródła danych. Spowoduje to spowodować problemy z aktualizacją i usuwanie wywołań metody. W związku z tym, całkowicie wyczyszczenie tej właściwości lub je zresetować do ustawień domyślnych `{0}`.

Po zakończeniu pracy kreatora i naprawianie `OldValuesParameterFormatString`, s ObjectDataSource o oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Aby włączyć kontrolki GridView s wbudowane funkcje edycji, zaznacz opcję Włącz edytowanie, za pomocą tagu inteligentnego s GridView. To ustawienie ustawi CommandField s `ShowEditButton` właściwości `True`, co dodatkowo przycisk Edytuj (i przyciski aktualizacji i Anuluj dla wiersza, edytowany).


[![Konfigurowanie kontrolki GridView do edycji pomocy technicznej](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Na rysunku nr 10**: Konfigurowanie GridView do edycji pomocy technicznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Odwiedź stronę za pośrednictwem przeglądarki i kliknij jeden z wierszy s przyciski Edytuj. `CategoryName` i `Description` BoundFields są renderowane jako pola tekstowe. `BrochurePath` Brakuje TemplateField `EditItemTemplate`, więc kontynuuje wyświetlić jego `ItemTemplate` łącze do broszurę. `Picture` ImageField renderowany jako pole tekstowe, którego `Text` właściwość jest przypisywana wartość s ImageField `DataImageUrlField` wartości w tym przypadku `CategoryID`.


[![Kontrolki GridView brakuje interfejsu edycji dla BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Rysunek 11**: widoku GridView nie ma interfejsu edycji kontrolki dla `BrochurePath` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Dostosowywanie`BrochurePath`interfejsu edycji kontrolki s

Musimy utworzyć interfejsu edycji `BrochurePath` TemplateField, taki, który umożliwia użytkownikowi albo:

- Pozostaw broszura kategorii s jako —,
- Zaktualizuj broszura kategorii s przez przekazanie nowego brochure, lub
- Całkowicie usunąć broszura kategorii s (w przypadku, że kategorii nie ma już skojarzone broszura).

Musimy też zaktualizować `Picture` ImageField s edycji interfejs, ale uzyskać do tego w kroku 7.

W widoku GridView s tagu inteligentnego, kliknij link Edytuj szablony i wybierz pozycję `BrochurePath` TemplateField s `EditItemTemplate` z listy rozwijanej. Dodawanie kontrolki RadioButtonList w sieci Web do tego szablonu, ustawiając jego `ID` właściwości `BrochureOptions` i jego `AutoPostBack` właściwość `True`. W oknie dialogowym właściwości kliknij wielokropek w `Items` właściwość, która zostanie wyświetlone okno `ListItem` — Edytor kolekcji. Dodaj następujące trzy opcje z `Value` s, 1, 2 i 3, odpowiednio:

- Użyj bieżącego broszura
- Usuń bieżący broszura
- Przekaż nowe broszura

Ustawianie pierwszego `ListItem` s `Selected` właściwość `True`.


![Dodaj trzy ListItems do RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Rysunek 12**: Dodaj trzy `ListItem` s RadioButtonList


Poniżej RadioButtonList, Dodaj formant FileUpload o nazwie `BrochureUpload`. Ustaw jego `Visible` właściwość `False`.


[![Dodaj do EditItemTemplate RadioButtonList i FileUpload kontroli](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Rysunek 13**: Dodaj RadioButtonList i kontroli FileUpload `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Ta RadioButtonList zawiera trzy opcje dla użytkownika. Chodzi o to, że formant FileUpload będą wyświetlane tylko wtedy, gdy wybrano Ostatnia opcja przekazywania nowego brochure. W tym celu należy utworzyć program obsługi zdarzeń dla RadioButtonList s `SelectedIndexChanged` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Ponieważ kontrolki RadioButtonList i przekazywaniem plików znajdują się w szablonie, należy napisać ilość kodu, aby uzyskać programowy dostęp do tych kontrolek. `SelectedIndexChanged` Programu obsługi zdarzeń jest przekazywany odwołanie RadioButtonList w `sender` parametr wejściowy. Aby uzyskać kontrolę FileUpload, musimy rodzica s RadioButtonList, kontroli i użyj `FindControl("controlID")` metody, w tym miejscu. Gdy będziemy już mieć odwołania do obu RadioButtonList FileUpload formantów i, FileUpload kontrolować s `Visible` właściwość jest ustawiona na `True` tylko wtedy, gdy RadioButtonList s `SelectedValue` 3, jest równe `Value` dla broszura nowe przekazywania `ListItem`.

Przy użyciu tego kodu w miejscu Poświęć chwilę, przetestowaniu interfejsu edycji. Kliknij przycisk Edytuj, aby wiersz. Początkowo należy wybrać opcję Użyj bieżącego broszura. Zmiana wybranego indeksu powoduje odświeżenie strony. Jeśli trzecia opcja jest zaznaczona, jest wyświetlany formantu FileUpload, w przeciwnym razie jest ukryty. Rysunek 14 pokazuje interfejsu edycji, gdy najpierw kliknięto przycisk Edytuj; Rysunek 15 pokazuje interfejs, po wybraniu nowej opcji broszura przekazywania.


[![Początkowo wybranej opcji bieżącego broszura użycia](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Rysunek 14**: początkowo, Użyj bieżącego broszurę wybrano opcję ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Wybieranie nowego broszura przekazywania opcji wyświetla kontroli FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Rysunek 15**: Wybieranie nowego broszura przekazywania opcji wyświetla kontroli FileUpload ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Zapisywanie broszurę plików i aktualizowanie`BrochurePath`kolumny

Po kliknięciu przycisku Aktualizuj s GridView jego `RowUpdating` generowane zdarzenie. Kontrolki ObjectDataSource s polecenia update jest wywoływana, a następnie GridView s `RowUpdated` generowane zdarzenie. Jak usunąć przepływu pracy, musimy utworzyć procedury obsługi zdarzeń dla obu tych zdarzeń. W `RowUpdating` procedura obsługi zdarzeń, należy ustalić, jaka akcja ma być oparty na `SelectedValue` z `BrochureOptions` RadioButtonList:

- Jeśli `SelectedValue` wynosi 1, chcemy nadal korzystać z takie same `BrochurePath` ustawienie. Dlatego musimy ObjectDataSource s `brochurePath` parametru do istniejących `BrochurePath` wartość rekordu aktualizowana. ObjectDataSource s `brochurePath` parametr można ustawić za pomocą `e.NewValues["brochurePath"] = value`.
- Jeśli `SelectedValue` wynosi 2, a następnie chcemy ustawić rekordu s `BrochurePath` wartość `NULL`. Można to zrobić, ustawiając ObjectDataSource s `brochurePath` parametr `Nothing`, co powoduje, że w bazie danych `NULL` używany w `UPDATE` instrukcji. W przypadku istniejącego pliku broszura, która jest usuwana, należy usunąć istniejącego pliku. Jednak tylko chcemy to zrobić, jeśli aktualizacja zakończy się bez zgłaszania wyjątku.
- Jeśli `SelectedValue` jest 3, a następnie chcemy upewnić się, że użytkownik został przekazany plik PDF, a następnie zapisz go na system plików i zaktualizuj rekord s `BrochurePath` wartości w kolumnie. Ponadto w przypadku istniejącego pliku broszura, który jest zastępowany, należy usunąć poprzedni plik. Jednak tylko chcemy to zrobić, jeśli aktualizacja zakończy się bez zgłaszania wyjątku.

Czynnościach, które należy wykonać po RadioButtonList s `SelectedValue` jest 3 są niemal identyczne do tych używanych przez DetailsView s `ItemInserting` programu obsługi zdarzeń. Ta procedura obsługi zdarzeń jest wykonywana po dodaniu nowego rekordu kategorii z dodaliśmy w kontrolce DetailsView [poprzedniego samouczka](including-a-file-upload-option-when-adding-a-new-record-vb.md). W związku z tym jego behooves nam Refaktoryzuj tę funkcję się w oddzielnych metodach. W szczególności została przeniesiona funkcja wspólne do dwóch metod:

- `ProcessBrochureUpload(FileUpload, out bool)` przyjmuje jako dane wejściowe, wystąpienie kontrolki przekazywaniem plików i danych wyjściowych wartość logiczna, która określa, czy należy kontynuować operację usuwania lub edycji, lub jeśli powinna zostać anulowana z powodu błędu sprawdzania poprawności. Ta metoda zwraca ścieżkę do pliku zapisanego lub `null` Jeśli plik nie został zapisany.
- `DeleteRememberedBrochurePath` Usuwa plik określony przez ścieżkę w zmiennej strony `deletedCategorysPdfPath` Jeśli `deletedCategorysPdfPath` nie `null`.

Kod dla tych dwóch metod poniżej. Należy pamiętać, podobieństwa między `ProcessBrochureUpload` i DetailsView s `ItemInserting` programu obsługi zdarzeń z poprzednim samouczku. W tym samouczku zaktualizowano I procedury obsługi zdarzeń s DetailsView używania tych nowych metod. Pobierz kod skojarzony z tym samouczkiem, aby zobaczyć zmiany do obsługi zdarzeń DetailsView s.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s `RowUpdating` i `RowUpdated` użyć procedury obsługi zdarzeń `ProcessBrochureUpload` i `DeleteRememberedBrochurePath` metod, co ilustruje poniższy kod:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Uwaga jak `RowUpdating` programu obsługi zdarzeń korzysta z szeregu instrukcji warunkowych do wykonania odpowiednich akcji na podstawie `BrochureOptions` RadioButtonList s `SelectedValue` wartości właściwości.

Przy użyciu tego kodu w miejscu możesz edytować kategorię i jego użycia jego bieżący broszura, korzystaj z nie broszura lub Prześlij nowy. Przejdź dalej i wypróbuj jej działanie. Ustawianie punktów przerwania w `RowUpdating` i `RowUpdated` procedury obsługi zdarzeń, aby uzyskać ogólny obraz przepływu pracy.

## <a name="step-7-uploading-a-new-picture"></a>Krok 7: Przekazywania nowego obrazu

`Picture` S ImageField edycji renderuje interfejsu jako pole tekstowe wypełnione wartością z jego `DataImageUrlField` właściwości. Podczas edytowania przepływu pracy, widoku GridView przekazuje parametr do elementu ObjectDataSource z nazwą s parametru wartość ImageField s `DataImageUrlField` właściwości i s parametru wartość podana w polu tekstowym w interfejsie edycji. To zachowanie jest odpowiednia w przypadku, gdy obraz zostanie zapisany jako plik w systemie plików i `DataImageUrlField` zawiera pełny adres URL obrazu. Dzięki takiej sytuacji interfejsu edycji wyświetla s adres URL obrazu w polu tekstowym, które użytkownik może zmienić i zostały zapisane w bazie danych. Udzielenia tego domyślnego interfejsu t umożliwia użytkownikowi Przekaż nowy obraz, ale informacją zmienić adres URL obrazu z bieżącą wartość do innego. W tym samouczku, jednak domyślne s ImageField edytowanie interfejsu nie wystarcza, ponieważ `Picture` przechowywanych danych binarnych bezpośrednio w bazie danych i `DataImageUrlField` właściwości przechowują po prostu z `CategoryID`.

Aby lepiej zrozumieć, co dzieje się w naszym samouczku, gdy użytkownik edytuje wiersz, w którym ImageField, rozważmy następujący przykład: użytkownik edytuje wiersz z `CategoryID` 10, powodując `Picture` ImageField być renderowany jako pole tekstowe z wartością 10. Wyobraź sobie, że użytkownik zmieni wartość w tym polu tekstowym do 50, a następnie klika przycisk Aktualizuj. Występuje odświeżenie strony i kontrolki GridView wstępnie tworzy parametr o nazwie `CategoryID` wartość 50. Jednak przed widoku GridView wysyła tego parametru (i `CategoryName` i `Description` parametrów), dodaje wartości z `DataKeys` kolekcji. W związku z tym, zastępuje `CategoryID` parametrem bazowej bieżącego wiersza s `CategoryID` wartość 10. Krótko mówiąc, s ImageField edytowanie interfejsu nie ma wpływu na edytowanie przepływu pracy na potrzeby tego samouczka ponieważ nazwy ImageField s `DataImageUrlField` właściwości i siatki s `DataKey` wartość są w taki sam.

Gdy ImageField ułatwia wyświetlania obrazu na podstawie danych z bazy danych, firma Microsoft don t chcesz, aby zapewnić pole tekstowe w interfejsie edycji. Przeciwnie chcemy oferować formant FileUpload, który użytkownik końcowy może używać do zmiany obrazu s kategorii. W odróżnieniu od `BrochurePath` wartość, dla tych samouczków firma ve decyzję, aby wymagać, że każda kategoria musi mieć obraz. W związku z tym, firma Microsoft don t trzeba umożliwić użytkownikowi oznacza, że żadnego skojarzonego obrazu użytkownika może być albo Przekaż nowy obraz lub pozostaw bieżącego obrazu jako-to.

Dostosowywanie interfejsu edycji s ImageField, musimy przekonwertować go na TemplateField. Za pomocą tagu inteligentnego s GridView kliknij link Edytuj kolumny, zaznacz ImageField, a następnie kliknij Convert to pole na łącze TemplateField.


![Konwertowanie ImageField TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Rysunek 16**: konwertowanie ImageField TemplateField


Konwertowanie ImageField na TemplateField w ten sposób generuje TemplateField dwa szablony. Jak pokazano na poniższej składni deklaratywnej, `ItemTemplate` obrazu w sieci Web zawiera formant, którego `ImageUrl` właściwość jest przypisana, przy użyciu składni wiązania danych oparte na ImageField s `DataImageUrlField` i `DataImageUrlFormatString` właściwości. `EditItemTemplate` Zawiera pole tekstowe którego `Text` właściwość jest powiązana wartość określoną przez `DataImageUrlField` właściwości.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Musimy zaktualizować `EditItemTemplate` używać kontrolki FileUpload. Z GridView tagu inteligentnego s kliknij pozycję Edytuj szablony łącze, a następnie wybierz pozycję `Picture` TemplateField s `EditItemTemplate` z listy rozwijanej. W szablonie powinny być widoczne pole tekstowe, Usuń ten element. Następnie przeciągnij formant FileUpload z przybornika do szablonu, ustawiając jego `ID` do `PictureUpload`. Również dodać tekst, aby zmienić obraz kategorii s, określ nowy obraz. Aby zachować obraz kategorii s takie same, pole puste do szablonu, jak również.


[![Dodawanie kontrolki FileUpload do EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Rysunek 17**: Dodawanie formantu z przekazywaniem plików `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Po Dostosowywanie interfejsu edycji, należy wyświetlić postęp w przeglądarce. Podczas przeglądania wierszy w trybie tylko do odczytu, image s kategorii jest wyświetlany jako sprzed, ale kliknięcie na przycisk Edytuj pozwala kolumny obrazu jako tekst z kontrolką FileUpload.


[![Edytowanie interfejsu zawiera kontrolkę FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Rysunek 18**: interfejsu edycji zawiera kontrolkę FileUpload ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Pamiętaj, że kontrolki ObjectDataSource jest skonfigurowany do wywołania `CategoriesBLL` klasy s `UpdateCategory` metodę, która przyjmuje jako dane wejściowe dane binarne obraz jako `Byte` tablicy. Jeśli ta tablica jest `Nothing`, jednak alternatywna `UpdateCategory` przeciążenie jest wywoływana, problemy, które `UPDATE` instrukcję SQL, która nie powoduje modyfikacji `Picture` kolumny, w tym samym opuszczeniem bieżącej kategorii s obraz opublikowane. W związku z tym, w tym s GridView `RowUpdating` musimy programowo odwoływać się do programu obsługi zdarzeń `PictureUpload` FileUpload kontroli i określić, jeśli plik został przekazany. Jeśli jeden nie został załadowany, a następnie robimy *nie* chcesz określić wartość dla `picture` parametru. Z drugiej strony, jeśli plik został przekazany w `PictureUpload` kontroli FileUpload chcemy się upewnić, że jest to plik JPG. Jeśli jest, a następnie wysyłamy zawartością binarne do kontrolki ObjectDataSource za pośrednictwem `picture` parametru.

Tak, jak kod używany w kroku 6, większość kodu tu potrzebne już istnieje w DetailsView s `ItemInserting` programu obsługi zdarzeń. W związku z tym, czy ve zaprojektowane od nowa typowych funkcji do nowej metody `ValidPictureUpload`i zaktualizować `ItemInserting` program obsługi zdarzeń do używania tej metody.

Dodaj następujący kod na początku GridView s `RowUpdating` programu obsługi zdarzeń. Jego s pamiętać, że ten kod występować przed kod, który zapisuje plik broszura, ponieważ firma Microsoft don t ma zostać zapisany broszurę w systemie plików serwera s sieci web, jeśli plik nieprawidłowy obraz zostanie przekazany.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)` Metoda przyjmuje w kontrolce FileUpload jako jedyny parametr wejściowy i sprawdza, czy rozszerzenie przekazany plik s, upewnij się, że przekazany plik JPG; jest on wywoływany tylko, jeśli przekazany plik obrazu. Jeśli plik nie zostanie przekazany, a następnie parametru obrazu nie ustawiono i w związku z tym używa domyślnej wartości `Nothing`. Jeśli obraz został przekazany i `ValidPictureUpload` zwraca `True`, `picture` parametr jest przypisany dane binarne przekazanego obrazu; Jeśli metoda zwraca `False`, przepływ pracy aktualizacji została anulowana i program obsługi zdarzeń został zakończony.

`ValidPictureUpload(FileUpload)` Metoda kod, który został wycofany z DetailsView s `ItemInserting` obsługi zdarzeń:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Krok 8: Zastępując oryginalne obrazy kategorie jpg

Pamiętaj, że oryginalne obrazy osiem kategorii są pliki map bitowych opakowane w nagłówku OLE. Teraz dodaliśmy możliwość edycji istniejący obraz rekordu s przyjrzeć zastąpić te mapy bitowe jpg. Jeśli chcesz nadal używać bieżącej obrazy kategorii, możesz przekształcić je do jpg, wykonując następujące czynności:

1. Zapisać obrazy mapy bitowej na dysku twardym. Odwiedź stronę `UpdatingAndDeleting.aspx` strony w przeglądarce i dla każdej z pierwszych osiem kategorii, kliknij prawym przyciskiem myszy na obrazie, a następnie Zapisz obraz.
2. Otwórz obraz w edytorze obrazu wybranego. Na przykład możesz użyć programu Microsoft Paint.
3. Zapisz mapy bitowej jako obraz JPG.
4. Aktualizowanie obrazu kategorii s za pośrednictwem interfejsu edycji, przy użyciu pliku JPG.

Po zakończeniu edycji kategorii i przekazanie obrazu JPG, obraz, który nie będą renderowane w przeglądarce ponieważ `DisplayCategoryPicture.aspx` strony jest obcięcie pierwszych 78 bajtów z obrazów pierwszych osiem kategorii. Rozwiązać ten problem przez usunięcie kodu, który wykonuje obcięcie nagłówka OLE. Po wykonaniu tego, `DisplayCategoryPicture.aspx``Page_Load` program obsługi zdarzeń powinny mieć następujący kod:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` Strony Wstawianie i Edycja interfejsów można używać trochę więcej pracy. `CategoryName` i `Description` BoundFields DetailsView i GridView powinny być konwertowane do kontrolek TemplateField. Ponieważ `CategoryName` nie zezwala na `NULL` wartości RequiredFieldValidator powinny zostać dodane. I `Description` TextBox prawdopodobnie powinny być konwertowane w wielowierszowym polu tekstowym. Opuścić tych poprawek w charakterze ćwiczenia dla Ciebie.


## <a name="summary"></a>Podsumowanie

W tym samouczku kończy naszych się pracy z danymi binarnymi. W tym samouczku i poprzednie trzy widzieliśmy dane binarne jak mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych. Użytkownik udostępnia dane binarne do systemu przez wybranie pliku z dysku twardego i przekazać go do serwera sieci web, w którym może być przechowywany w systemie plików lub wstawione do bazy danych. Platforma ASP.NET 2.0 obejmuje kontroli FileUpload, który ułatwia zapewnienie taki interfejs tak łatwe przeciąganie i upuszczanie. Jednak jak zanotowane w [przesyłanie plików](uploading-files-vb.md) samouczku FileUpload znajduje się kontrolka tylko nadają się do przekazywania plików stosunkowo mały, najlepiej nieprzekraczającej megabajt. Rozważyliśmy się również, jak skojarzyć przekazane dane z bazowy model danych, a także jak edytowanie i usuwanie danych binarnych z istniejących rekordów.

Nasz kolejny zbiór samouczki przedstawiono różnych technik przetwarzania w pamięci podręcznej. Pamięć podręczna oferuje sposób poprawić s aplikacji ogólną wydajność przez pobranie wyniki z kosztownych operacji i przechowywanie ich w lokalizacji, do której można szybciej uzyskać dostęp.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](including-a-file-upload-option-when-adding-a-new-record-vb.md)
