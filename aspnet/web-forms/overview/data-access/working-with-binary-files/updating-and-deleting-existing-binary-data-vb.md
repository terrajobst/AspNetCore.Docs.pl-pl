---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: "Aktualizowanie i usuwanie istniejących danych binarnych (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W samouczkach wcześniej widzieliśmy jak kontrolki widoku siatki ułatwia edytowania i usuwania danych tekstowych. W tym samouczku przedstawiono, jak również wprowadzić kontrolki widoku siatki..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e14b19f99e9f41c5a296d73ba689095a686794db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualizowanie i usuwanie istniejących danych binarnych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) lub [pobierania plików PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> W samouczkach wcześniej widzieliśmy jak kontrolki widoku siatki ułatwia edytowania i usuwania danych tekstowych. W tym samouczku przedstawiono sposób kontrolki widoku siatki umożliwia także do edytowania i usuwania danych binarnych, czy te dane binarne są zapisywane w bazie danych ani przechowywane w systemie plików.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich trzech samouczki możemy kolejnych dodane dość nieco funkcje do pracy z danych binarnych. Firma Microsoft uruchomiona przez dodanie `BrochurePath` kolumny `Categories` tabeli i odpowiednio aktualizowany architektury. Dodaliśmy również metody Warstwa dostępu do danych i warstwy logiki biznesowej do pracy z istniejących kategorii tabeli s `Picture` kolumny, która przechowuje binarne s zawartości pliku obrazu. Stworzyliśmy stron sieci web do prezentowania łącze pobierania brochure, z kategorii s obraz wyświetlany w danych binarnych w widoku GridView `<img>` element i dodano element DetailsView, aby zezwolić użytkownikom na dodawanie nowej kategorii i przekaż jej broszura i obraz danych.

Wszystkie opcje, które nadal wykonywane jest możliwość edytowania i usuwania istniejących kategorii, które firma Microsoft będzie wykonywać w tym samouczku przy użyciu widoku GridView s wbudowanych edycji i usuwania funkcji. Podczas edytowania kategorii, użytkownik będzie opcjonalnie Przekaż nowy obraz lub mają nadal używać istniejącą kategorię. Dla brochure ich można wybrać istniejący brochure, za pomocą można przekazać nowy broszura lub wskazują, że kategoria zawiera już broszurę skojarzonych z nim. Rozpoczynanie pracy dzięki s!

## <a name="step-1-updating-the-data-access-layer"></a>Krok 1: Aktualizowanie Warstwa dostępu do danych

Warstwa DAL został wygenerowany automatycznie `Insert`, `Update`, i `Delete` metody, ale te metody zostały wygenerowane na podstawie `CategoriesTableAdapter` główne zapytanie s, która nie obejmuje `Picture` kolumny. W związku z tym `Insert` i `Update` metody nie dołączaj parametrów służący do określania danych binarnych dla obrazu kategorii s. Jak robiliśmy [poprzedniego samouczek](including-a-file-upload-option-when-adding-a-new-record-vb.md), należy utworzyć nową metodę TableAdapter aktualizowanie `Categories` Tabela podczas określania danych binarnych.

Otwórz zestaw danych wpisany i przy użyciu projektanta, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` s nagłówka i wybierz polecenie Dodaj zapytanie z menu kontekstowego do launche Kreator konfiguracji zapytania TableAdapter. Ten Kreator uruchamia się z prośbą sposób zapytanie TableAdapter powinno dostęp do bazy danych. Wybierz instrukcji SQL użyj, a następnie kliknij przycisk Dalej. Następnym krokiem wyświetli monit o typ zapytania do wygenerowania. Ponieważ firma Microsoft re Trwa tworzenie zapytania, aby dodać nowy rekord do `Categories` tabeli, wybierz AKTUALIZACJĘ i kliknij przycisk Dalej.


[![Wybierz opcję aktualizacji](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Rysunek 1**: wybierz opcję aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Teraz musisz określić `UPDATE` instrukcji SQL. Kreator automatycznie sugeruje `UPDATE` instrukcji odpowiadający s zapytanietableadapter głównego (który aktualizuje `CategoryName`, `Description`, i `BrochurePath` wartości). Zmień instrukcję, aby `Picture` kolumny jest dołączany wraz z `@Picture` parametru, w następujący sposób:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Na ekranie końcowym kreatora zapyta nam na nazwę nowej metody TableAdapter. Wprowadź `UpdateWithPicture` i kliknij przycisk Zakończ.


[![Nazwa nowej UpdateWithPicture metody TableAdapter](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Rysunek 2**: Nazwa nowej metody TableAdapter `UpdateWithPicture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Krok 2: Dodawanie metod warstwy logiki biznesowej

Oprócz aktualizowanie warstwy DAL, należy zaktualizować logiki warstwy Biznesowej, aby uwzględnić metody aktualizowanie i usuwanie kategorii. Są to metody, które będą wywoływane z warstwy prezentacji.

Do usuwania kategorii, możemy użyć `CategoriesTableAdapter` s automatycznie generowanej `Delete` metody. Dodaj następującą metodę do `CategoriesBLL` klasy:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

W tym samouczku let s utworzyć dwie metody uaktualnienia kategorii - oczekuje dane binarne obrazu i wywołuje `UpdateWithPicture` dodano już do metody `CategoriesTableAdapter` i drugi akceptujący tylko `CategoryName`, `Description`i `BrochurePath`wartości i używa `CategoriesTableAdapter` s automatycznie generowanej klasy `Update` instrukcji. Argumentów przemawiających za pomocą dwóch metod jest, że w niektórych sytuacjach użytkownika mogą zostać zaktualizowane obrazu s kategorii wraz z jego innych pól, w których przypadku użytkownik będzie musiał Przekaż nowy obraz. Dane binarne obraz przekazany s mogą posłużyć w `UPDATE` instrukcji. W innych przypadkach użytkownik może tylko być zainteresowani, trwa aktualizowanie powiedzieć, nazwę i opis. Jeśli jednak `UPDATE` instrukcji oczekuje dane binarne `Picture` również kolumny, a następnie możemy d musisz podać te informacje, jak również. Jest wymagany dodatkowy podróży do bazy danych, aby przywrócić dane obrazu edytowanego rekordu. W związku z tym chcemy dwa `UPDATE` metody. Warstwy logiki biznesowej określi, który według tego, czy obraz podano danych podczas aktualizacji kategorii.

Aby to ułatwić, Dodaj dwie metody `CategoriesBLL` klasy zarówno o nazwie `UpdateCategory`. Pierwsza z nich powinien akceptować trzy `String` s, `Byte` tablicy i `Integer` jako dane wejściowe parametry; druga Strona, tylko trzech `String` s i `Integer`. `String` Parametry wejściowe są nazwy kategorii s, opis i ścieżka pliku broszura, `Byte` tablica jest binarny zawartości obrazu kategorii s i `Integer` identyfikuje `CategoryID` rekordu do zaktualizowania. Powiadomienie, że pierwszy przeciążenia wywołuje Jeśli drugi przekazany do `Byte` tablica jest `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Krok 3: Kopiowanie za pośrednictwem Insert i funkcje widoku

W [poprzedniego samouczek](including-a-file-upload-option-when-adding-a-new-record-vb.md) utworzyliśmy stronę o nazwie `UploadInDetailsView.aspx` wyświetlane wszystkie kategorie w widoku GridView i podać DetailsView do dodawania nowych kategorii do systemu. W tym samouczku rozbudowujemy widoku GridView do uwzględnienia, edytowanie i usuwanie pomocy technicznej. Zamiast kontynuuje współpracę z `UploadInDetailsView.aspx`, umożliwiają s umieścić zmiany tego samouczka s `UpdatingAndDeleting.aspx` strony z tego samego folderu `~/BinaryData`. Skopiuj i Wklej deklaratywne znaczników i kodu z `UploadInDetailsView.aspx` do `UpdatingAndDeleting.aspx`.

Uruchamianie przez otwarcie `UploadInDetailsView.aspx` strony. Skopiuj wszystkie składni deklaratywnej w `<asp:Content>` element, jak pokazano na rysunku 3. Następnie otwórz folder `UpdatingAndDeleting.aspx` i wklej ten kod znaczników w jego `<asp:Content>` elementu. Podobnie, skopiować kod z `UploadInDetailsView.aspx` strony klasy związane z kodem s `UpdatingAndDeleting.aspx`.


[![Skopiuj deklaratywne znaczników z UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Rysunek 3**: kopiowanie deklaratywne znaczników z `UploadInDetailsView.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Po skopiowaniu na deklaratywne znaczników i kodu, odwiedź stronę `UpdatingAndDeleting.aspx`. Powinny pojawić się takie same dane wyjściowe i mieć tego samego środowiska użytkownika za pomocą `UploadInDetailsView.aspx` strony z poprzedniej samouczka.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Krok 4: Dodawanie, usuwanie wsparcia ObjectDataSource i widoku GridView

Jak wspomniano w [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczka widoku GridView udostępnia wbudowane funkcje usuwania i możliwości może być włączona z znaczników jest pole wyboru, jeśli podstawowy siatki s źródło danych obsługuje usuwania. Obecnie ObjectDataSource widoku GridView jest powiązany (`CategoriesDataSource`) nie obsługuje usuwania.

Aby rozwiązać ten problem, kliknij tę opcję Konfigurowanie źródła danych z tagów inteligentnych s ObjectDataSource, aby uruchomić kreatora. Pierwszy ekran pokazuje, że element ObjectDataSource jest skonfigurowany do pracy z `CategoriesBLL` klasy. Kliknij przycisk Dalej. Obecnie tylko element ObjectDataSource s `InsertMethod` i `SelectMethod` właściwości są określone. Jednak Kreator automatycznie wypełnione list rozwijanych w kartach UPDATE i DELETE z `UpdateCategory` i `DeleteCategory` metod, odpowiednio. Wynika to z faktu w `CategoriesBLL` klasy oznaczonej możemy tych metod, za pomocą `DataObjectMethodAttribute` jako domyślnych metod aktualizowania i usuwania.

Teraz, ustaw listy rozwijanej kartę s aktualizacji (Brak), ale z listy rozwijanej Usuń kartę s ustawioną `DeleteCategory`. Wrócimy do tego kreatora w kroku 6, aby dodać obsługę aktualizacji.


[![Skonfiguruj element ObjectDataSource przy użyciu metody DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource użyć `DeleteCategory` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Po zakończeniu działania kreatora programu Visual Studio może wystąpić, jeśli chcesz Odśwież i klucze, który spowoduje ponowne wygenerowanie danych sieci Web kontrolki pola. Wybierz opcję nie, ponieważ wybranie opcji Tak spowoduje zastąpienie wszelkich dostosowań pola, które mogły zostać wprowadzone.


Element ObjectDataSource teraz będzie zawierać wartość dla jego `DeleteMethod` właściwości, a także `DeleteParameter`. Odwołaj, że podczas korzystania z kreatora, aby określić metod, Visual Studio ustawia ObjectDataSource s `OldValuesParameterFormatString` właściwości `original_{0}`, która powoduje występowanie problemów z aktualizacją i usunąć wywołań metod. W związku z tym wyczyszczenie tej właściwości w ogóle albo zresetować go do domyślnych, `{0}`. Jeśli potrzebujesz odświeżanie pamięci dla tej właściwości w elemencie ObjectDataSource, zobacz [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) samouczka.

Po zakończeniu pracy kreatora i rozwiązywania `OldValuesParameterFormatString`, znaczników deklaratywne s ObjectDataSource powinien wyglądać podobnie jak następujące:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Po skonfigurowaniu ObjectDataSource należy dodać możliwości usuwania do widoku GridView, zaznaczając pole wyboru Włącz usuwanie z tagów inteligentnych s widoku GridView. Spowoduje to dodanie CommandField do widoku GridView których `ShowDeleteButton` właściwość jest ustawiona na `True`.


[![Włącz obsługę usuwania w widoku GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Rysunek 5**: Włączanie obsługi usunięcie w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Poświęć chwilę, aby przetestować funkcje delete. Brak klucza obcego między `Products` tabeli s `CategoryID` i `Categories` tabeli s `CategoryID`, dlatego próba Usuń wszystkie kategorie pierwszych osiem wystąpi wyjątek naruszenie ograniczenia klucza obcego. Aby przetestować funkcje ten limit, dodać nową kategorię, podawania broszura i obraz. Moje kategorii testów, przedstawiono na rysunku 6, obejmuje plik broszura testu o nazwie `Test.pdf` i obraz testowy. Rysunek nr 7 przedstawia widoku GridView po dodaniu kategorii testu.


[![Dodaj kategorię testu broszura i obrazów](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Rysunek 6**: Dodaj kategorię testu broszura i obrazu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Po wstawieniu kategorii testów, jest wyświetlany w widoku GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Rysunek 7**: po wstawieniu kategorii testów, jest wyświetlany w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


W programie Visual Studio Odśwież Eksploratora rozwiązań. Powinien zostać wyświetlony nowy plik w `~/Brochures` folderu, `Test.pdf` (patrz rysunek 8).

Następnie kliknij łącze Usuń w wierszu kategorii testów, powodując strony odświeżania i `CategoriesBLL` klasy s `DeleteCategory` metody uruchomienie. To spowoduje wywołanie DAL s `Delete` metody powodują odpowiednie `DELETE` instrukcji do wysłania do bazy danych. Dane są następnie odbitych do widoku GridView i znaczniki są wysyłane do klienta z kategorii testu nie są już.

Podczas usuwania przepływu pracy pomyślnie usunął rekord kategorii testu z `Categories` tabeli nie spowodowało usunięcia jego pliku broszura z systemu plików s serwera sieci web. Odśwież Eksploratora rozwiązań i zostanie wyświetlone `Test.pdf` nadal znajduje się w `~/Brochures` folderu.


![Plik Test.pdf nie został usunięty z systemu plików s serwera sieci Web](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Rysunek 8**: `Test.pdf` plik nie został usunięty z systemu plików s serwera sieci Web


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Krok 5: Usuwanie pliku broszura s usuniętych kategorii

Jednym z downsides przechowywania danych binarnych zewnętrznych do bazy danych jest musi podjęcia dodatkowych czynności aby wyczyścić te pliki po usunięciu rekordu skojarzonej bazie danych. GridView i ObjectDataSource Podaj zdarzenia, które wyzwalać przed i po wykonaniu polecenia delete. Musimy faktycznie tworzenie obsługi zdarzeń dla zdarzenia przed i po akcji. Przed `Categories` jest usuwany należy określić ścieżki pliku s PDF, ale możemy ADAM t chcesz usunąć plik PDF, przed usunięciem tej kategorii w przypadku, gdy występuje wyjątek i kategoria nie została usunięta.

GridView s [ `RowDeleting` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) uruchamiany przed polecenia delete s ObjectDataSource została wywołana, podczas jej [ `RowDeleted` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) generowane po. Tworzenie obsługi zdarzeń dla tych dwóch zdarzeń, używając następującego kodu:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

W `RowDeleting` program obsługi zdarzeń, `CategoryID` wiersza usuwany jest pobierany z widoku GridView s `DataKeys` kolekcji, który jest dostępny w tej obsłudze zdarzeń za pomocą `e.Keys` kolekcji. Następnie `CategoriesBLL` klasy s `GetCategoryByCategoryID(categoryID)` wywołaniu zwraca informacje dotyczące rekordu usuwany. Jeśli zwróconego `CategoriesDataRow` obiekt ma niż`NULL``BrochurePath` wartości są przechowywane w zmiennej strony `deletedCategorysPdfPath` , dzięki czemu można usunąć pliku `RowDeleted` obsługi zdarzeń.

> [!NOTE]
> Zamiast pobierania `BrochurePath` szczegółów `Categories` rekord usuwany w `RowDeleting` obsługi zdarzeń można też dodaliśmy `BrochurePath` s GridView `DataKeyNames` właściwości i dostępne wartości rekordu s za pomocą `e.Keys` kolekcji. W ten sposób będzie nieco zwiększyć rozmiar widoku GridView s widok stanu, ale może zmniejszyć ilość kodu i Zapisz podróży do bazy danych.


Po s podstawowej polecenia delete została wywołana, element ObjectDataSource GridView s `RowDeleted` uruchamiany program obsługi zdarzeń. Jeśli nie było wyjątków w usuwania danych, a wartość `deletedCategorysPdfPath`, plik PDF jest usuwana z systemu plików. Należy pamiętać, ten dodatkowy kod nie jest potrzebna wyczyścić dane binarne s kategorii, skojarzone z jego obrazu. Tego s, ponieważ dane obrazu są przechowywane bezpośrednio w bazie danych, więc usunięcie `Categories` wiersza spowoduje również usunięcie tych kategorii s obraz danych.

Po dodaniu obsługi dwóch zdarzeń, uruchom ponownie tego przypadku testowego. Podczas usuwania kategorii, powoduje również usunięcie jego skojarzony plik PDF.

Aktualizowanie istniejących danych binarnych rekordów s skojarzone zawiera niektóre ciekawe wyzwania. W pozostałej części tego samouczka delves na dodawanie funkcji aktualizacji do broszura i obraz. Krok 6 Eksploruje techniki aktualizacji informacji broszura podczas kroku 7 analizuje aktualizowania obrazu.

## <a name="step-6-updating-a-category-s-brochure"></a>Krok 6: Aktualizowanie broszurę s kategorii

Zgodnie z opisem w samouczku omówienie Wstawianie, aktualizowanie i usuwanie danych, w widoku GridView oferuje wbudowana obsługa edycji poziomie wiersza, który może być zaimplementowany przez znaczników jest pole wyboru, jeśli źródła danych jest odpowiednio skonfigurowany. Obecnie `CategoriesDataSource` ObjectDataSource nie został jeszcze skonfigurowany do uwzględnienia aktualizacji pomocy technicznej, aby dodać let s, że w.

Kliknij łącze Konfigurowanie źródła danych z Kreatora s ObjectDataSource i przejdź do kroku drugiego. Z powodu `DataObjectMethodAttribute` używane w `CategoriesBLL`, aktualizacji listy rozwijanej powinny zostać wypełnione automatycznie z `UpdateCategory` przeciążenia, które przyjmuje cztery parametry wejściowe (dla wszystkich kolumn, ale `Picture`). Zmiany, tak aby były używane z parametrami pięć przeciążenia.


[![Skonfiguruj element ObjectDataSource przy użyciu metody UpdateCategory zawierającą parametr dla obrazu](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Rysunek 9**: Konfigurowanie ObjectDataSource użyć `UpdateCategory` metodę, która zawiera parametr `Picture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


Element ObjectDataSource teraz będzie zawierać wartość dla jego `UpdateMethod` właściwości, a także odpowiadającego `UpdateParameter` s. Zgodnie z opisem w kroku 4, Visual Studio ustawia ObjectDataSource s `OldValuesParameterFormatString` właściwości `original_{0}` podczas korzystania z Kreatora konfigurowania źródła danych. Będzie to powodować problemy z aktualizacją i Usuń wywołania metody. W związku z tym wyczyszczenie tej właściwości w ogóle albo zresetować go do domyślnych, `{0}`.

Po zakończeniu pracy kreatora i rozwiązywania `OldValuesParameterFormatString`, znaczników deklaratywne s ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Aby włączyć w widoku GridView s wbudowane funkcje edycji, zaznacz opcję Włącz edytowanie z tagów inteligentnych s widoku GridView. Spowoduje to ustawienie CommandField s `ShowEditButton` właściwości `True`, co dodatkowo przycisk Edytuj (i aktualizacji i Anuluj przycisków dla wiersza edytowanej).


[![Konfigurowanie widoku GridView do edycji pomocy technicznej](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Na rysunku nr 10**: konfigurowanie widoku GridView do obsługi edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Odwiedź stronę za pośrednictwem przeglądarki, a następnie kliknij jeden z wierszy s przycisków edycji. `CategoryName` i `Description` BoundFields są renderowane jako pól tekstowych. `BrochurePath` Brakuje TemplateField `EditItemTemplate`, więc nadal Pokaż jego `ItemTemplate` łącze do broszury. `Picture` Elementu ImageField renderuje jako pole tekstowe, którego `Text` właściwości jest przypisywana wartość s elementu ImageField `DataImageUrlField` wartość, w tym przypadku `CategoryID`.


[![Interfejs edycji w widoku GridView nie ma dla BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Rysunek 11**: widoku GridView nie ma interfejsu edycji `BrochurePath` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Dostosowywanie`BrochurePath`interfejsu edycji s

Należy utworzyć interfejsu edycji `BrochurePath` TemplateField, który umożliwia użytkownikowi albo:

- Pozostaw broszura kategorii s jako-,
- Zaktualizuj broszura kategorii s przekazując broszurę nowy lub
- Całkowicie usunąć broszura kategorii s (w przypadku czy kategoria zawiera już skojarzone broszura).

Ponadto należy zaktualizować `Picture` interfejs edytowania elementu ImageField s, ale otrzymają tej w kroku 7.

W widoku GridView s tagu, kliknij łącze Edytuj szablony i wybierz pozycję `BrochurePath` TemplateField s `EditItemTemplate` z listy rozwijanej. Dodawanie formantu RadioButtonList sieci Web do tego szablonu, ustawienie jej `ID` właściwości `BrochureOptions` i jego `AutoPostBack` właściwości `True`. W oknie właściwości, kliknij przycisk wielokropka w `Items` właściwość, która pojawi się `ListItem` edytora kolekcji. Dodaj następujące trzy opcje z `Value` s, 1, 2 i 3, odpowiednio:

- Użyj bieżącego broszura
- Usuń bieżący broszura
- Przekaż nowy broszura

Ustawianie pierwszego `ListItem` s `Selected` właściwości `True`.


![Dodaj trzy elementy ListItems do RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Rysunek 12**: Dodaj trzy `ListItem` s do RadioButtonList


Poniżej RadioButtonList, Dodaj formant przekazywaniem plików o nazwie `BrochureUpload`. Ustaw jego `Visible` właściwości `False`.


[![Dodaj do EditItemTemplate RadioButtonList i przekazywaniem plików sterowania](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Rysunek 13**: Dodaj RadioButtonList i przekazywaniem plików sterowania `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Ta RadioButtonList zawiera trzy opcje dla użytkownika. Pomysł jest, że formant przekazywaniem plików będą wyświetlane tylko po wybraniu opcji ostatni brochure nowe przekazywania. W tym celu należy utworzyć programu obsługi zdarzeń dla RadioButtonList s `SelectedIndexChanged` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Ponieważ formanty RadioButtonList i przekazywaniem plików w ramach szablonu, mamy zapisu z bitowego kodu do uzyskania programowego dostępu do tych kontrolek. `SelectedIndexChanged` Program obsługi zdarzeń jest przekazywany odwołanie RadioButtonList w `sender` parametru wejściowego. Uzyskanie kontroli przekazywaniem plików, musimy rodzica s RadioButtonList, sterowania i użyj `FindControl("controlID")` metodę z tego miejsca. Gdy mamy odwołania do obu RadioButtonList przekazywaniem plików formantów i przekazywaniem plików sterowania s `Visible` właściwość jest ustawiona na `True` tylko wtedy, gdy RadioButtonList s `SelectedValue` 3, który jest równe `Value` dla broszura nowe przekazywania `ListItem`.

Ten kod w miejscu Poświęć chwilę, aby przetestować interfejs edytowania. Kliknij przycisk Edytuj wiersza. Początkowo należy wybrać opcję Użyj bieżącego broszura. Zmiana wybranego indeksu powoduje odświeżenie strony. Jeśli trzecia opcja jest zaznaczona, jest wyświetlany formant przekazywaniem plików, w przeciwnym razie jest ukryty. Rysunek 14 udostępnia interfejsem edycji, gdy najpierw zostanie kliknięty przycisk Edytuj; Rysunek 15 pokazuje interfejsu po wybraniu nowej opcji broszura przekazywania.


[![Początkowo wybranej opcji bieżącego broszura użycia](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Rysunek 14**: początkowo, Użyj bieżącego broszury wybrano opcję ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Wybieranie przekazywania broszura nowych opcji wyświetla formant przekazywaniem plików](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Rysunek 15**: Wybieranie przekazywania broszura nowych opcji wyświetla formant przekazywaniem plików ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Zapisywanie broszury pliku i aktualizowanie`BrochurePath`kolumny

Po kliknięciu przycisku Aktualizuj s GridView jego `RowUpdating` generowane zdarzenie. ObjectDataSource wywołaniu polecenia update s, a następnie GridView s `RowUpdated` generowane zdarzenie. Podobnie jak z usuwania przepływu pracy, należy utworzyć procedury obsługi zdarzeń dla obu tych zdarzeń. W `RowUpdating` program obsługi zdarzeń, należy ustalić, jaka akcja ma być oparta na `SelectedValue` z `BrochureOptions` RadioButtonList:

- Jeśli `SelectedValue` 1, jeśli chcesz zachować korzystającej z tego samego `BrochurePath` ustawienie. W związku z tym należy ustawić element ObjectDataSource s `brochurePath` parametru do istniejącej `BrochurePath` wartości rekordu aktualizowana. Element ObjectDataSource s `brochurePath` parametru można ustawić za pomocą `e.NewValues["brochurePath"] = value`.
- Jeśli `SelectedValue` 2, a następnie chcemy, aby ustawić rekordu s `BrochurePath` do wartości `NULL`. Można to zrobić przez ustawienie ObjectDataSource s `brochurePath` parametr `Nothing`, które powoduje, że w bazie danych `NULL` używane w `UPDATE` instrukcji. W przypadku istniejącego pliku broszura, który jest usuwana, należy usunąć istniejący plik. Jednak tylko chcemy to zrobić po zakończeniu aktualizacji bez podnoszenia Wystąpił wyjątek.
- Jeśli `SelectedValue` to 3, a następnie chcemy upewnij się, że użytkownik został przekazany plik PDF, a następnie zapisz go w systemie plików i zaktualizować rekord s `BrochurePath` wartość w kolumnie. Ponadto w przypadku istniejącego pliku broszura zastępowanej figury geometrycznej, należy usunąć poprzednie pliku. Jednak tylko chcemy to zrobić po zakończeniu aktualizacji bez podnoszenia Wystąpił wyjątek.

Kroki niezbędne do można wykonać po RadioButtonList s `SelectedValue` jest 3 są niemal identyczne używanych przez s widoku DetailsView `ItemInserting` obsługi zdarzeń. Ten program obsługi zdarzeń jest wykonywany podczas dodawania nowego rekordu kategorii z formantu widoku DetailsView dodaliśmy w [samouczek poprzedniej](including-a-file-upload-option-when-adding-a-new-record-vb.md). W związku z tym go behooves nam Refaktoryzuj ta funkcja się w oddzielnych metodach. W szczególności została przeniesiona poza typowe funkcje do dwóch metod:

- `ProcessBrochureUpload(FileUpload, out bool)`przyjmuje jako dane wejściowe wystąpienie kontrolki przekazywaniem plików i danych wyjściowych wartość logiczna, która określa, czy ma być kontynuowane operację usuwania lub edycji lub jeśli powinien zostać anulowany z powodu błędu sprawdzania poprawności. Ta metoda zwraca ścieżkę do pliku zapisanego lub `null` Jeśli plik nie został zapisany.
- `DeleteRememberedBrochurePath`Usuwa plik określony przez ścieżkę w zmiennej strony `deletedCategorysPdfPath` Jeśli `deletedCategorysPdfPath` nie jest `null`.

Wykonuje kod dla tych dwóch metod. Należy zwrócić uwagę na podobieństwo `ProcessBrochureUpload` i s widoku DetailsView `ItemInserting` obsługi zdarzeń z poprzednich samouczka. W tym samouczku I zaktualizowano programów obsługi zdarzeń s widoku DetailsView tych nowych metod. Pobierz kod skojarzony z tym samouczkiem, aby zobaczyć zmiany do obsługi zdarzeń s widoku DetailsView.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s `RowUpdating` i `RowUpdated` użyć procedury obsługi zdarzeń `ProcessBrochureUpload` i `DeleteRememberedBrochurePath` metody, jak w poniższym kodzie:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Uwaga jak `RowUpdating` obsługi zdarzeń korzysta z szeregu warunkowe instrukcje, aby wykonać odpowiednie działania na podstawie `BrochureOptions` RadioButtonList s `SelectedValue` wartości właściwości.

Z tego kodu w miejscu można edytować kategorii i go użyć jego bieżący broszura, użyj nie broszura lub przekazać nowy. Przejdź dalej i wypróbować jej możliwości. Ustaw punkty przerwania w `RowUpdating` i `RowUpdated` procedury obsługi zdarzeń można zorientować przepływu pracy.

## <a name="step-7-uploading-a-new-picture"></a>Krok 7: Przekazywanie nowego obrazu

`Picture` s elementu ImageField edycji renderuje interfejsu jako pole tekstowe wypełniony wartość z jego `DataImageUrlField` właściwości. Podczas edytowania przepływu pracy, widoku GridView przekazuje parametr do elementu ObjectDataSource z nazwą parametru s wartość elementu ImageField s `DataImageUrlField` właściwości s parametru wartości wprowadzonej w polu tekstowym w interfejsie edycji wartości. To zachowanie jest odpowiednia w przypadku, gdy obraz jest zapisywany jako plik w systemie plików i `DataImageUrlField` zawiera pełny adres URL obrazu. Z takich sytuacji edycji interfejs zawiera adres URL s obrazu w polu tekstowym, które użytkownik może zmienić i zostały zapisane w bazie danych. Udzieleniu to domyślny interfejs zezwala na użytkownika przekazać nowy obraz, ale daj Zmień adres URL obrazu z bieżącą wartość. W tym samouczku, jednak domyślne s elementu ImageField edycji interfejsu nie wystarcza, ponieważ `Picture` dane binarne są przechowywane bezpośrednio w bazie danych i `DataImageUrlField` blokad właściwości właśnie `CategoryID`.

Aby lepiej zrozumieć, co się stanie w naszym samouczku, gdy użytkownik edytuje wiersz z elementu ImageField, rozważmy następujący przykład: użytkownik edytuje wiersz z `CategoryID` 10, co powoduje `Picture` elementu ImageField być renderowany jako pole tekstowe na wartość 10. Załóżmy, czy użytkownik zmieni wartość w tym polem tekstowym 50 i kliknie przycisk Aktualizuj. Występuje odświeżania strony i widoku GridView tworzy początkowo parametr o nazwie `CategoryID` na wartość 50. Jednak przed wysłaniem przez widoku GridView tego parametru (i `CategoryName` i `Description` parametrów), dodaje wartości z `DataKeys` kolekcji. W związku z tym zastępuje `CategoryID` parametru z bieżącego wiersza s odpowiadającego `CategoryID` wartość 10. Krótko mówiąc, s elementu ImageField edycji interfejsu nie ma wpływu na edytowania przepływu pracy w tym samouczku ponieważ nazwy s elementu ImageField `DataImageUrlField` właściwości i siatki s `DataKey` wartości są w taki sam.

Gdy element ImageField ułatwia wyświetlania obrazu na podstawie danych bazy danych, możemy ADAM chcesz t dostarcza element textbox w interfejsie edycji. Zamiast chcemy oferują formant przekazywaniem plików, który użytkownik końcowy służy do zmieniania obrazu s kategorii. W przeciwieństwie do `BrochurePath` wartości, te samouczki możemy kolejnych decyzję, aby wymagać, aby każda kategoria musi mieć obraz. W związku z tym możemy ADAM trzeba umożliwić użytkownikowi oznacza, że żadnego skojarzonego obrazu użytkownika może albo Przekaż nowy obraz lub pozostaw bieżącego obrazu jako t-jest.

Aby dostosować interfejs edycji elementu ImageField s, należy przekonwertować go na pole TemplateField. Z tagów inteligentnych s widoku GridView kliknij łącze edycji kolumn, zaznacz element ImageField, a następnie kliknij przycisk Konwertuj to pole na pole TemplateField łącze.


![Konwertuj element ImageField na pole TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Rysunek 16**: Konwertowanie elementu ImageField na pole TemplateField


Konwertowanie na pole TemplateField w ten sposób element ImageField generuje TemplateField z dwóch szablonów. Jak pokazano na poniższej składni deklaratywnej, `ItemTemplate` obrazu w sieci Web zawiera kontroli, których `ImageUrl` właściwości jest przypisany przy użyciu składni wiązania danych oparte na s elementu ImageField `DataImageUrlField` i `DataImageUrlFormatString` właściwości. `EditItemTemplate` Zawiera pole tekstowe których `Text` właściwość jest powiązana wartość określoną przez `DataImageUrlField` właściwości.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Należy zaktualizować `EditItemTemplate` można użyć formantu przekazywaniem plików. W widoku GridView tagów inteligentnych s polecenie Edytuj szablony łącza, a następnie wybierz `Picture` TemplateField s `EditItemTemplate` z listy rozwijanej. W szablonie powinny pojawić się usunąć to pole tekstowe. Następnie przeciągnij formant przekazywaniem plików z przybornika do szablonu, ustawienie jej `ID` do `PictureUpload`. Również dodać tekst, aby zmienić obraz kategorii s, określ nowy obraz. Aby zachować obraz kategorii s takie same, pole pozostanie puste do szablonu, a także.


[![Dodawanie formantu przekazywaniem plików do EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Rysunek 17**: Dodawanie formantu przekazywaniem plików do `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Po dostosowaniu interfejs edytowania, postęp jest wyświetlany w przeglądarce. Podczas przeglądania wiersza w trybie tylko do odczytu, obrazu s kategorii jest wyświetlana jako sprzed, ale kliknięcie przycisku Edytuj renderuje obraz kolumny jako tekst z formantem przekazywaniem plików.


[![Interfejs edycji zawiera formant przekazywaniem plików](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Rysunek 18**: interfejs edycji zawiera formant przekazywaniem plików ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Odwołaj, że element ObjectDataSource jest skonfigurowany do wywołania `CategoriesBLL` klasy s `UpdateCategory` metodę, która przyjmuje jako dane wejściowe dane binarne obraz jako `Byte` tablicy. Jeśli jest to tablica `Nothing`, jednak alternatywnego `UpdateCategory` wywołać przeciążenia, problemy, które `UPDATE` instrukcji SQL, który nie modyfikuje `Picture` kolumny, w tym samym opuszczeniem bieżącej kategorii s nienaruszonej obrazu. Dlatego w widoku GridView s `RowUpdating` obsługi zdarzeń należy odwoływać się programowo `PictureUpload` przekazywaniem plików kontroli i określić, czy plik został przekazany. Jeśli jeden nie został załadowany, a następnie przejdziemy *nie* chcesz określić wartość dla `picture` parametru. Z drugiej strony, jeśli plik został przekazany w `PictureUpload` kontroli przekazywaniem plików chcemy upewnić się, że jest to plik JPG. Jeśli jest, a następnie możemy wysłać zawartością binarnego do elementu ObjectDataSource za pośrednictwem `picture` parametru.

Jak z kod używany w kroku 6, większość kodu tu potrzebne już istnieje w widoku DetailsView s `ItemInserting` program obsługi zdarzeń. W związku z tym I Zapisz refaktoryzowane typowe funkcje do nowej metody, `ValidPictureUpload`i zaktualizować `ItemInserting` obsługi zdarzeń, aby użyć tej metody.

Dodaj następujący kod do początku GridView s `RowUpdating` obsługi zdarzeń. Go s ważne jest, że ten kod występować przed kod, który zapisuje plik broszura, ponieważ będziemy ADAM t chcesz zapisać broszury w systemie plików serwera s sieci web, jeśli plik nieprawidłowy obraz jest przekazywany.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)` Metoda pobiera w formancie przekazywaniem plików jako jedyny parametr wejściowy i sprawdza rozszerzenie przekazany plik s do zapewnienia, że przekazany plik JPG; jest on wywoływany tylko, jeśli jest przekazywany plik obrazu. Jeśli plik nie jest przekazywany, a następnie parametr obrazu jest nieustawiony, a w związku z tym używa domyślnej wartości `Nothing`. Jeśli obraz został załadowany i `ValidPictureUpload` zwraca `True`, `picture` parametru jest przypisany danymi binarnymi załadowanego obrazu; Jeśli metoda zwraca `False`, przepływ pracy aktualizacji zostało anulowane i program obsługi zdarzeń został zakończony.

`ValidPictureUpload(FileUpload)` Metody kod, który został zrefaktoryzowany z widoku DetailsView s `ItemInserting` sposób obsługi zdarzeń:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Krok 8: Zastępowanie jpg oryginalne obrazy kategorii

Odwołaj się, że oryginalne obrazy osiem kategorii są ujęte w nagłówku OLE pliki map bitowych. Teraz, dodano możliwość Edytuj istniejący obraz rekordu s, Poświęć chwilę, aby zastąpić te map bitowych jpg. Jeśli chcesz nadal używać obrazów bieżącej kategorii można przekonwertować na jpg, wykonując następujące czynności:

1. Zapisywanie obrazów mapy bitowej na dysku twardym. Odwiedź stronę `UpdatingAndDeleting.aspx` strony w przeglądarce i dla każdej z pierwszych osiem kategorii, kliknij prawym przyciskiem myszy na obrazie i zapisać obrazu.
2. Otwórz obraz w wybranym edytorze obrazu. Na przykład można użyć programu Microsoft Paint.
3. Zapisz mapy bitowej jako obraz JPG.
4. Aktualizowanie obrazu kategorii s za pośrednictwem interfejsu edycji, przy użyciu pliku JPG.

Po zakończeniu edycji kategorii i przekazywanie obrazów JPG, obraz nie będzie zwracał w przeglądarce ponieważ `DisplayCategoryPicture.aspx` strony jest usuwanie pierwszego 78 bajtów z obrazów pierwszych osiem kategorii. To naprawić przez usunięcie kodu, który wykonuje usuwanie nagłówka OLE. Po wykonaniu tego `DisplayCategoryPicture.aspx``Page_Load` obsługi zdarzeń powinny mieć następujący kod:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` Strony Wstawianie i Edycja interfejsów można używać więcej wysiłku. `CategoryName` i `Description` BoundFields w widoku DetailsView i GridView powinny być konwertowane do TemplateFields. Ponieważ `CategoryName` nie zezwala na `NULL` wartości, należy dodać RequiredFieldValidator. I `Description` pole tekstowe powinny być konwertowane prawdopodobnie w wielowierszowym polu tekstowym. I Pozostaw te ostateczne poprawki wykonywania dla Ciebie.


## <a name="summary"></a>Podsumowanie

W tym samouczku wykonuje naszych przyjrzeć się praca z danych binarnych. W tym samouczku i trzech poprzednich widzieliśmy dane binarne jak mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych. Użytkownik udostępnia dane binarne do systemu przez wybranie pliku z dysku twardego i przekazać go do serwera sieci web, w którym można przechowywanych w systemie plików, lub wstawione do bazy danych. Platforma ASP.NET 2.0 obejmuje umożliwiająca dostarczanie taki interfejs należycie łatwym przeciągania i upuszczania formantu przekazywaniem plików. Jednak, jakie zostały zanotowane w [przesyłanie plików](uploading-files-vb.md) samouczka formant przekazywaniem plików jest tylko nadają się do przekazywania plików stosunkowo mały, najlepiej nieprzekraczającej megabajt. Możemy również zbadane, jak skojarzyć dane przekazane z właściwego modelu danych, a także sposób edytowania i usuwania danych binarnych z istniejących rekordów.

Nasze dalej zbiór samouczki Eksploruje różnych technik buforowania. Buforowanie stanowi sposób poprawić s aplikacji ogólną wydajność przez pobranie wyniki operacji kosztowne i przechowywania ich w lokalizacji dostępnej dla szybciej.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](including-a-file-upload-option-when-adding-a-new-record-vb.md)
