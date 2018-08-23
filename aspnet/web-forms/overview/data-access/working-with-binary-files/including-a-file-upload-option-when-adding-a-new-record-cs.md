---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: W tym pliku przekazywania opcji podczas dodawania nowego rekordu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku pokazano, jak utworzyć interfejs sieci Web, który umożliwia użytkownikowi wprowadzanie tekstu danych i przekaż pliki binarne. Aby zilustrować t dostępne opcje...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: c3887f920126d70b300de5a0d6e09474fd33c332
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756811"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Dołączanie opcji przekazywania pliku podczas dodawania nowego rekordu (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) lub [Pobierz plik PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> W tym samouczku pokazano, jak utworzyć interfejs sieci Web, który umożliwia użytkownikowi wprowadzanie tekstu danych i przekaż pliki binarne. Aby zilustrować dostępne opcje aby przechowywać dane binarne, jeden plik zostanie zapisany w bazie danych, podczas gdy druga jest przechowywany w systemie plików.


## <a name="introduction"></a>Wprowadzenie

W poprzednich samouczkach dwóch rozważyliśmy technik do przechowywania danych binarnych, który jest skojarzony z modelem danych aplikacji s, przyjrzano się jak formant FileUpload służy do wysyłania plików z klienta do serwera sieci web i poznaliśmy sposób przedstawiania tych danych binarnych w danych W Kontrolka EB. Firma Microsoft ve jeszcze, aby porozmawiać na temat sposobu kojarzenia przekazane dane z modelu danych, mimo że.

W tym samouczku utworzymy strony sieci web, aby dodać nową kategorię. Oprócz pól tekstowych dla kategorii s nazwę i opis ta strona muszą zawierać dwóch kontrolek FileUpload dla nowego obrazu kategorii s i jeden dla broszurę. Przekazany obraz będą przechowywane bezpośrednio w nowym rekordzie s `Picture` kolumny, natomiast broszurę zostanie zapisany w `~/Brochures` folderu ze ścieżką do pliku, który został zapisany w nowym rekordzie s `BrochurePath` kolumny.

Przed utworzeniem tej nowej strony sieci web, musimy zaktualizować architekturę. `CategoriesTableAdapter` s główne zapytanie nie pobrać `Picture` kolumny. W związku z tym, wygenerowaną automatycznie `Insert` metoda zawiera tylko dane wejściowe dla `CategoryName`, `Description`, i `BrochurePath` pola. W związku z tym, należy utworzyć dodatkową metodę w TableAdapter, zawierająca monit o podanie wszystkie cztery `Categories` pola. `CategoriesBLL` Klasy w warstwy logiki biznesowej będzie również trzeba być aktualizowane.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Krok 1. Dodawanie`InsertWithPicture`metody`CategoriesTableAdapter`

Po utworzeniu `CategoriesTableAdapter` w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczka został skonfigurowany do automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcji na podstawie głównego zapytania. Ponadto firma Microsoft zgodnie z instrukcjami otrzymywanymi TableAdapter można wykorzystywać podejście bezpośredniego bazy danych, który metody `Insert`, `Update`, i `Delete`. Te metody wykonywania wygenerowany automatycznie `INSERT`, `UPDATE`, i `DELETE` instrukcji i w związku z tym, Zaakceptuj parametrów wejściowych, na podstawie kolumn zwrócony przez główne zapytanie. W [przesyłanie plików](uploading-files-cs.md) samouczka będziemy rozszerzone `CategoriesTableAdapter` s główne zapytanie do użycia `BrochurePath` kolumny.

Ponieważ `CategoriesTableAdapter` s główne zapytanie nie odwołuje się `Picture` kolumny, firma Microsoft nie można dodać nowy rekord ani zaktualizować istniejący rekord z wartością dla `Picture` kolumny. Aby można było przechwytywać te informacje, możemy albo utworzyć nową metodę w TableAdapter, który jest używany do wstawienia rekordu z danymi binarnymi, lub można dostosować, wygenerowany automatycznie `INSERT` instrukcji. Problem dotyczący Dostosowywanie generowanych automatycznie `INSERT` instrukcja jest firma Microsoft ryzyka o naszych dostosowania zastąpione przez kreatora. Załóżmy, że możemy dostosować `INSERT` instrukcję, aby uwzględnić użycie `Picture` kolumny. Spowoduje to zaktualizowanie TableAdapter s `Insert` metodę, aby uwzględnić dodatkowy parametr wejściowy dla danych binarnych obraz s kategorii s. Następnie możemy utworzyć metodę w warstwy logiki biznesowej, ta metoda warstwy DAL do wywołania tej metody LOGIKI przez warstwę prezentacji i wszystko będzie działać bajeczną. Oznacza to aż do następnego skonfigurowaliśmy TableAdapter za pomocą Kreatora konfiguracji TableAdapter. Tak szybko, jak ukończono działanie kreatora, nasze dostosowania `INSERT` instrukcji zostaną zastąpione, `Insert` metoda będzie powracać do jego starego formularza i naszego kodu będzie już kompilacji!

> [!NOTE]
> Określenia tego dokuczliwości jest bez problemu, korzystając z procedur składowanych zamiast instrukcji SQL zapytań ad-hoc. Samouczek przyszłych przedstawimy korzystanie z procedur składowanych, zamiast instrukcji SQL zapytań ad-hoc w warstwie dostępu do danych.


Aby uniknąć tego potencjalnych kłopotliwe, zamiast Dostosowywanie instrukcji SQL generowanych automatycznie umożliwiają s zamiast utworzenie nowej metody TableAdapter. Ta metoda o nazwie `InsertWithPicture`, będzie akceptować wartości `CategoryName`, `Description`, `BrochurePath`, i `Picture` kolumny, a następnie wykonaj `INSERT` instrukcji, która przechowuje wszystkie cztery wartości w nowym rekordzie.

Otwórz zestaw danych wpisane, a przy użyciu projektanta, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` nagłówka s i z menu kontekstowego wybierz polecenie Dodaj zapytanie. Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter, który rozpoczyna się od pytaniem jak zapytania TableAdapter powinien uzyskiwać dostęp do bazy danych. Wybierz opcję Użyj instrukcji SQL, a następnie kliknij przycisk Dalej. Następnym krokiem monituje o podanie typu zapytania do wygenerowania. Ponieważ firma Microsoft, ponownie tworząc zapytanie, aby dodać nowy rekord do `Categories` tabeli, wybierz opcję Wstaw i kliknij przycisk Dalej.


[![Wybierz opcję Wstaw](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Rysunek 1**: wybierz opcję Wstaw ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Teraz musisz określić `INSERT` instrukcji SQL. Kreator automatycznie sugeruje `INSERT` instrukcji odpowiadający główne zapytanie TableAdapter s. W tym przypadku go s `INSERT` instrukcję, która wstawia `CategoryName`, `Description`, i `BrochurePath` wartości. Aktualizacja instrukcji, aby `Picture` kolumny jest dołączany wraz `@Picture` parametru, w następujący sposób:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Na ostatnim ekranie kreatora pyta, czy nam nadaj nazwę nowej metody TableAdapter. Wprowadź `InsertWithPicture` i kliknij przycisk Zakończ.


[![Nazwa nowego InsertWithPicture metody TableAdapter](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Rysunek 2**: nadaj nazwę nowej metody TableAdapter `InsertWithPicture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Krok 2: Trwa aktualizowanie warstwy logiki biznesowej

Ponieważ warstwa prezentacji tylko powinny współpracować z warstwy logiki biznesowej, a nie z pominięciem jej spowoduje przejście bezpośrednio do warstwy dostępu do danych, należy utworzyć metodę LOGIKI, która wywołuje metodę DAL, którą właśnie utworzyliśmy (`InsertWithPicture`). W tym samouczku należy utworzyć metodę w `CategoriesBLL` klasę o nazwie `InsertWithPicture` akceptujący jako danych wejściowych trzech `string` s i `byte` tablicy. `string` Parametry wejściowe są nazwa kategorii s, opis i ścieżkę pliku broszura, podczas gdy `byte` tablica jest binarny zawartości obrazu kategorii s. Jak pokazano na poniższym kodem, ta metoda LOGIKI wywołuje odpowiednia metoda DAL:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Upewnij się, został zapisany wpisany zestaw danych przed dodaniem `InsertWithPicture` metody LOGIKI. Ponieważ `CategoriesTableAdapter` kod klasy został wygenerowany automatycznie na podstawie wpisana zestawu danych, jeśli don t najpierw zapisać zmiany do zestawu danych wpisane `Adapter` właściwość wygrał t wiedzieć o `InsertWithPicture` metody.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Krok 3: Wyświetlanie istniejących kategorii i ich dane binarne

W tym samouczku zostanie utworzona strona, która pozwala użytkownikowi końcowemu dodać nową kategorię do systemu, podając obrazu i broszura nowej kategorii. W [poprzedni Samouczek](displaying-binary-data-in-the-data-web-controls-cs.md) użyliśmy GridView TemplateField i ImageField, aby wyświetlić nazwę każdej kategorii s, opis, obraz i link umożliwiający pobranie jej broszura. Pozwól s replikowanie tej funkcji na potrzeby tego samouczka tworzenia strona, która zawiera listę wszystkich istniejących kategorii i pozwala na nowe ma zostać utworzony.

Zacznij od otwarcia `DisplayOrDownload.aspx` strony `BinaryData` folderu. Przejdź do widoku źródła i skopiuj kontrolkami GridView i kontrolki ObjectDataSource s składni deklaratywnej, wklejając je w ramach `<asp:Content>` element `UploadInDetailsView.aspx`. Ponadto don t zapomnij Skopiuj `GenerateBrochureLink` metody z klasy CodeBehind `DisplayOrDownload.aspx` do `UploadInDetailsView.aspx`.


[![Skopiuj i Wklej składni deklaratywnej z DisplayOrDownload.aspx do UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Rysunek 3**: kopiowanie i wklejanie składni deklaratywnej z `DisplayOrDownload.aspx` do `UploadInDetailsView.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Po skopiowaniu składni deklaratywnej i `GenerateBrochureLink` metodę za pośrednictwem `UploadInDetailsView.aspx` stron, wyświetlenia strony za pośrednictwem przeglądarki, aby upewnić się, że wszystko zostało skopiowano za pośrednictwem. Powinien zostać wyświetlony GridView listę osiem kategorii zawiera link do pobierania broszurę oraz obraz kategorii s.


[![Powinien zostać wyświetlony w każdej kategorii, wraz z jego dane binarne](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Rysunek 4**: możesz powinny teraz Zobacz każdej kategorii wraz z jego danymi binarnymi ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Krok 4: Konfigurowanie`CategoriesDataSource`do wstawiania pomocy technicznej

`CategoriesDataSource` Posługują się ObjectDataSource `Categories` GridView aktualnie nie zapewniają możliwość wstawiania danych. Aby zapewnić obsługę wstawiania za pomocą tego formantu źródła danych, należy mapowanie jego `Insert` metody do metody w jego obiektu źródłowego `CategoriesBLL`. W szczególności, chcemy zamapować na `CategoriesBLL` metoda dodaliśmy w kroku 2 `InsertWithPicture`.

Rozpocznij, klikając link Konfiguruj źródła danych za pomocą kontrolki ObjectDataSource tagu inteligentnego s. Pierwszy ekran przedstawia obiektu źródła danych jest skonfigurowane do pracy z `CategoriesBLL`. Pozostaw to ustawienie jako — jest i kliknij przycisk Dalej, przejdź do ekranu definiowanie metody dotyczące danych. Przenieś na kartę Wstaw, a następnie wybierz `InsertWithPicture` metodę z listy rozwijanej. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Rysunek 5**: Konfigurowanie kontrolki ObjectDataSource używać `InsertWithPicture` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Po ukończeniu kreatora, Visual Studio może wystąpić, jeśli chcesz odświeżyć pola i klucze, który spowoduje to ponowne wygenerowanie danych sieci Web kontroluje pola. Wybierz opcję nie, ponieważ wybranie opcji Tak spowoduje zastąpienie wszelkich dostosowań pola, które mogły zostać wprowadzone.


Po ukończeniu kreatora, kontrolki ObjectDataSource będzie teraz obejmować wartości jego `InsertMethod` właściwość także `InsertParameters` do kategorii czterech kolumn, jako następujących oznaczeniu deklaracyjnym przedstawia:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Krok 5: Tworzenie interfejsu Wstawianie

Jako pierwsze objęte [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), kontrolce DetailsView udostępnia wbudowane, wstawianie interfejs, który może być wykorzystywany podczas pracy z kontrolą źródła danych, który obsługuje wstawiania. Pozwól s dodać kontrolki widoku szczegółów do tej strony powyżej GridView zostanie trwale renderowanie jego Wstawianie interfejs użytkownika szybko dodać nową kategorię. Po dodaniu nowej kategorii w DetailsView, GridView znajdujące się poniżej zostanie automatycznie odświeżania i wyświetlać nową kategorię.

Start, przeciągając DetailsView z przybornika w Projektancie powyżej GridView, ustawiając jego `ID` właściwości `NewCategory` i wyczyszczenie `Height` i `Width` wartości właściwości. Za pomocą tagu inteligentnego s DetailsView powiązać go do istniejących `CategoriesDataSource` , a następnie zaznacz pole wyboru Włącz wstawianie.


[![Powiązywanie DetailsView CategoriesDataSource i umożliwić wstawianie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Rysunek 6**: powiązywanie DetailsView do `CategoriesDataSource` i umożliwić wstawianie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Aby renderować trwale DetailsView w jego interfejsie Wstawianie, ustaw jego `DefaultMode` właściwość `Insert`.

Należy zauważyć, że DetailsView ma pięć BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, i `BrochurePath` mimo że `CategoryID` elementu BoundField nie jest wyświetlana w interfejsie Wstawianie, ponieważ jego `InsertVisible` Właściwość jest ustawiona na `false`. Te BoundFields istnieje, ponieważ są one kolumny zwracane przez `GetCategories()` metoda, która jest kontrolki ObjectDataSource wywołuje można pobrać danych. Do wstawiania, jednak firma Microsoft don t chcesz, aby umożliwić użytkownikowi określić wartość dla `NumberOfProducts`. Ponadto należy zezwolić im na przekazywanie obrazu dla nowej kategorii, a także przekazać plik PDF broszurę.

Usuń `NumberOfProducts` elementu BoundField z całkowicie DetailsView, a następnie zaktualizuj `HeaderText` właściwości `CategoryName` i `BrochurePath` BoundFields do kategorii i Brochure, odpowiednio. Następnie przekonwertować `BrochurePath` elementu BoundField do TemplateField i dodać nowe TemplateField obrazu, dzięki czemu ten nowy TemplateField `HeaderText` wartość obrazu. Przenieś `Picture` TemplateField, tak aby można ją między `BrochurePath` TemplateField i CommandField.


![Powiązywanie DetailsView CategoriesDataSource i umożliwić wstawianie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Rysunek 7**: powiązywanie DetailsView do `CategoriesDataSource` i umożliwić wstawianie


Jeśli po konwersji `BrochurePath` obejmuje TemplateField elementu BoundField do TemplateField za pomocą okna dialogowego Edytowanie pól, `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate`. Tylko `InsertItemTemplate` jest wymagane, jednak, więc możesz usunąć dwa szablony. W tym momencie DetailsView składni deklaratywnej s powinna wyglądać następująco:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Dodawanie formantów FileUpload broszura i pola obrazu

Obecnie `BrochurePath` TemplateField s `InsertItemTemplate` zawiera pole tekstowe, podczas gdy `Picture` TemplateField nie zawiera żadnych szablonów. Należy zaktualizować te dwa s TemplateField `InsertItemTemplate` s, aby użyć kontrolek FileUpload.

W tagu inteligentnego s DetailsView opcję Edytuj szablony, a następnie wybierz `BrochurePath` TemplateField s `InsertItemTemplate` z listy rozwijanej. Usuń pole tekstowe, a następnie przeciągnij formant FileUpload z przybornika, do szablonu. Ustawianie kontroli FileUpload s `ID` do `BrochureUpload`. Podobnie, Dodaj kontrolkę FileUpload do `Picture` TemplateField s `InsertItemTemplate`. Ustaw ten formant FileUpload s `ID` do `PictureUpload`.


[![Dodawanie kontrolki FileUpload do InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Rysunek 8**: Dodawanie formantu z przekazywaniem plików `InsertItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Po wprowadzeniu tych dodatków, będą dwa składni deklaratywnej s TemplateField:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Gdy użytkownik doda nową kategorię, chcemy upewnić się, że broszura i obraz typu odpowiedniego pliku. Dla brochure że użytkownik musi podać plik PDF. Obraz, firma Microsoft na potrzeby użytkownika można przekazać pliku obrazu, ale możemy zezwalają na *wszelkie* obrazu pliku lub tylko pliki obrazów określonego typu, na przykład plików GIF lub jpg? Aby umożliwić dla różnych typów plików, d należy rozszerzyć `Categories` schematu, aby zawierała kolumnę, która przechwytuje typ pliku, więc, że tego typu mogą być wysyłane do klienta za pośrednictwem `Response.ContentType` w `DisplayCategoryPicture.aspx`. Ponieważ firma Microsoft don t kolumnie jest, byłoby rozsądne ograniczyć użytkowników do zapewniania tylko typu pliku określonego obrazu. `Categories` Tabeli s istniejących obrazów są mapy bitowe, ale jpg są bardziej odpowiednie format pliku obrazy obsługiwane za pośrednictwem sieci web.

Jeśli użytkownik prześle niepoprawny typ pliku, należy anulować insert i wyświetlić komunikat informujący o problemie. Dodaj kontrolkę etykieta w sieci Web pod DetailsView. Ustaw jego `ID` właściwości `UploadWarning`, wyczyść się jego `Text` , właściwością `CssClass` właściwości na ostrzegawczy i `Visible` i `EnableViewState` właściwości w celu `false`. `Warning` CSS, klasa jest zdefiniowana w `Styles.css` i renderuje tekstu duże czerwony, kursywą, pogrubioną czcionką.

> [!NOTE]
> W idealnym przypadku `CategoryName` i `Description` BoundFields zostanie przekonwertowana na kontrolek TemplateField i ich interfejsy Wstawianie dostosowane. `Description` Wstawianie interfejsu, na przykład będzie prawdopodobnie jej lepszego dostosowania za pośrednictwem wielowierszowym polu tekstowym. A ponieważ `CategoryName` kolumny nie akceptuje `NULL` wartości RequiredFieldValidator powinny zostać dodane do zapewnienia użytkownika zawiera wartość dla nowej nazwy kategorii s. Te kroki są pozostawiane w charakterze ćwiczenia do czytnika. Odwołaj się do [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) dla przyjrzeć się przestarzałe interfejsy modyfikacji danych.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Krok 6: Zapisywanie broszura przekazanego do systemu plików s serwera sieci Web

Gdy użytkownik wprowadza wartości dla nowej kategorii i kliknie przycisk Wstaw, występuje odświeżenie strony i rozwoju Wstawianie przepływu pracy. Po pierwsze, DetailsView s [ `ItemInserting` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) uruchamiany. Następnie, ObjectDataSource s `Insert()` metoda jest wywoływana, co powoduje, że w nowym rekordzie dodawane do `Categories` tabeli. Po zakończeniu DetailsView s [ `ItemInserted` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) uruchamiany.

Przed ObjectDataSource s `Insert()` metoda jest wywoływana, firma Microsoft musi najpierw upewnij się, że typy odpowiedniego pliku zostały przekazane przez użytkownika, a następnie zapisz broszura PDF w systemie plików serwera s sieci web. Utwórz procedurę obsługi zdarzeń dla DetailsView s `ItemInserting` zdarzeń i Dodaj następujący kod:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Program obsługi zdarzeń, który rozpoczyna się przez utworzenie odwołań do `BrochureUpload` FileUpload sterowania za pomocą szablonów s DetailsView. Następnie jeśli został przekazany broszurę rozszerzenie s przekazany plik jest sprawdzany pod. Jeśli rozszerzenie nie jest. Pliki PDF, następnie ostrzeżenie jest wyświetlane, Wstaw zostało anulowane i kończy wykonywanie programu obsługi zdarzeń.

> [!NOTE]
> Opierając się na rozszerzenie s przekazany plik nie jest to technika stosowane za zapewnienie, że przekazany plik jest dokument PDF. Użytkownik może mieć prawidłowy dokument PDF z rozszerzeniem `.Brochure`, lub można podjąć dokument PDF nie i biorąc pod uwagę jej `.pdf` rozszerzenia. Zawartość binarna s pliku należałoby programowo należy zbadać w celu bardziej ostatecznie Sprawdź typ pliku. Takie dokładnego podejścia, chociaż często są obszerne; Sprawdzanie rozszerzenie jest wystarczające dla większości scenariuszy.


Zgodnie z opisem w [przesyłanie plików](uploading-files-cs.md) samouczek, należy zachować ostrożność podczas zapisywania plików do systemu plików, więc przekazywania tego jednego użytkownika s nie zastąpi inną s. W tym samouczku firma Microsoft spróbuje użyć takiej samej nazwie jako przekazanego pliku. Jeśli istnieje już plik w `~/Brochures` katalog o tej samej nazwie pliku, jednak firma Microsoft będzie dodają liczbę na końcu aż do znalezienia unikatową nazwę. Na przykład, jeśli użytkownik prześle broszura plik o nazwie `Meats.pdf`, ale istnieje już plik o nazwie `Meats.pdf` w `~/Brochures` folderu, zmienimy nazwę zapisanego pliku do `Meats-1.pdf`. Jeśli, istnieje, zostanie podjęta `Meats-2.pdf`i tak dalej, aż do znalezienia unikatową nazwę pliku.

Poniższy kod używa [ `File.Exists(path)` metoda](https://msdn.microsoft.com/library/system.io.file.exists.aspx) do ustalenia, czy plik istnieje już przy użyciu określonej nazwy pliku. Jeśli tak, będzie kontynuowane do wypróbowania nowej nazwy plików dla broszurę, aż do znalezienia żadnych konfliktów.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Po znalezieniu prawidłową nazwę pliku pliku musi można zapisać w systemie plików i ObjectDataSource s `brochurePath``InsertParameter` wartość musi zostać zaktualizowany, aby ta nazwa pliku jest zapisywany w bazie danych. Jak widzieliśmy w *przesyłanie plików* samouczku plik można zapisać za pomocą formantu FileUpload s `SaveAs(path)` metody. Aby zaktualizować ObjectDataSource s `brochurePath` parametru, użyj `e.Values` kolekcji.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Krok 7: Zapisywania przekazanego obrazu z bazą danych

Do przechowywania przekazanego obrazu w nowym `Categories` rekord, należy przypisać przekazanych zawartość binarną do ObjectDataSource s `picture` parametr DetailsView s `ItemInserting` zdarzeń. Przed to przypisanie, jednak należy najpierw upewnić się, że przekazany obraz JPG i nie innego obraz typu. Jak w poniższym kroku 6 umożliwiają s stwierdzenia, jego typ za pomocą rozszerzenia pliku s przekazanego obrazu.

Gdy `Categories` umożliwia tabeli `NULL` wartości `Picture` kolumna, wszystkie kategorie obecnie ma obrazu. Pozwól s zmusza użytkownika do obrazu podczas dodawania nowej kategorii za pośrednictwem tej strony. Poniższy kod sprawdza, aby upewnić się, że obraz został przekazany i ma właściwe rozszerzenie.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Ten kod będzie umieszczona *przed* kod z kroku 6 tak, aby w przypadku problemu z przekazywaniem obrazu programu obsługi zdarzeń przestanie obowiązywać przed zapisaniem pliku broszura do systemu plików.

Przy założeniu, że odpowiedni plik został przekazany, należy przypisać przekazanych zawartość binarna do wartości s parametru obraz przy użyciu następującego kodu:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Pełne`ItemInserting`program obsługi zdarzeń

Aby informacje były kompletne, Oto `ItemInserting` program obsługi zdarzeń w całości:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Krok 8: Rozwiązywanie`DisplayCategoryPicture.aspx`strony

Umożliwiają s Poświęć chwilę, w celu przetestowania Wstawianie interfejsu i `ItemInserting` program obsługi zdarzeń, który został utworzony w ciągu ostatnich kilku kroków. Odwiedź stronę `UploadInDetailsView.aspx` strony za pośrednictwem przeglądarki i podjęcie próby dodać kategorię, ale Pomiń obraz lub obrazu innego niż JPG lub broszura niż PDF. W dowolnym z tych przypadków pojawi się komunikat o błędzie i anulowane insert przepływu pracy.


[![Ostrzeżenie jest wyświetlane, jeśli został przekazany nieprawidłowy typ pliku](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Rysunek 9**: ostrzeżenie jest wyświetlane, jeśli został przekazany nieprawidłowy typ pliku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Po sprawdzeniu strona wymaga obraz do przekazania i uzyskany t akceptuje plików innych niż PDF lub innych JPG, Dodaj nową kategorię prawidłowy obraz JPG, jeśli pole broszura pozostanie puste. Po kliknięciu przycisku Wstaw będzie ogłaszanie zwrotne strony i nowy rekord zostanie dodany do `Categories` tabelę zawierającą binarne treści przekazany obraz s bezpośrednio w bazie danych. Kontrolki GridView zostanie zaktualizowana i zawiera wiersz dla nowo dodanego kategorii, ale, jak pokazano na rysunku nr 10, nowy obraz s kategorii nie jest poprawnie renderowana.


[![Nowa kategoria s, który nie jest wyświetlany obraz](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Na rysunku nr 10**: s nowej kategorii nie jest wyświetlany obraz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Przyczyną nowy obraz nie jest wyświetlany jest `DisplayCategoryPicture.aspx` strona, która zwraca wartość określonej kategorii s obrazu jest skonfigurowany do przetworzenia mapy bitowe, które mają nagłówek OLE. Ten nagłówek 78 bajtów jest pozbawiony `Picture` kolumny s binarne treści przed wysłaniem ich z powrotem do klienta. Ale JPG plik, który właśnie został przekazany do nowej kategorii nie ma tego pliku nagłówkowego OLE; w związku z tym bajty prawidłowy, niezbędne są usuwane z danych binarnych s obrazu.

Ponieważ teraz są oba map bitowych z nagłówkami OLE i JPG w `Categories` tabeli, należy zaktualizować `DisplayCategoryPicture.aspx` tak, aby jest nagłówek OLE obcięcie dla oryginalnego osiem kategorii i pomija ten obcięcie nowszej rekordów kategorii. W naszym samouczku dalej zajmiemy się jak zaktualizować istniejący obraz rekordu s, a dodamy wszystkie obrazy stare kategorii, aby były jpg. Teraz, mimo że, użyj następującego kodu, w `DisplayCategoryPicture.aspx` oddzielić nagłówki OLE tylko dla tych oryginalnego osiem kategorii:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Dzięki tej zmianie obraz JPG teraz poprawnie renderowana w widoku GridView.


[![Obrazów JPG dla nowych kategorii są poprawnie renderowane](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Rysunek 11**: obrazów JPG dla nowych kategorii są poprawnie renderowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Krok 9: Usuwanie broszura także w przypadku wyjątku

Jednym z wyzwań dotyczących przechowywania danych binarnych w systemie plików s serwera sieci web jest wprowadzenie rozłączenia od modelu danych i jego danych binarnych. W związku z tym gdy rekord zostanie usunięty, odpowiadające im dane binarne w systemie plików należy także usunąć. Można to dochodzą do głosu przy wstawianiu, jak również. Rozważmy następujący scenariusz: użytkownik dodaje nową kategorię, określając prawidłowy obraz i broszura. Kliknięcie przycisku Wstaw występuje odświeżenie strony i DetailsView s `ItemInserting` generowane zdarzenie, zapisywanie broszurę w systemie plików serwera s sieci web. Następnie, s kontrolki ObjectDataSource `Insert()` wywoływana jest metoda, która wywołuje `CategoriesBLL` klasy s `InsertWithPicture` metody, która wywołuje `CategoriesTableAdapter` s `InsertWithPicture` metody.

Teraz, co się stanie, jeśli baza danych jest w trybie offline lub jeśli jest to błąd w `INSERT` instrukcji SQL? Wyraźnie Wstaw zakończy się niepowodzeniem, więc nie nowy wiersz kategorii, które zostaną dodane do bazy danych. Ale mamy jeszcze plików przekazanych broszura znajdują się w systemie plików s serwera sieci web! Ten plik wymaga do usunięcia w przypadku wystąpienia wyjątku podczas Wstawianie przepływu pracy.

Jak wspomniano wcześniej w [obsługi LOGIKI i wyjątki DAL na poziomie strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczek, gdy wyjątek jest zgłaszany w ramach głębokości architektury, jest ona przepuszcza się za pomocą różnych warstw. W warstwie prezentacji, możemy określić, jeśli wystąpił wyjątek z DetailsView s `ItemInserted` zdarzeń. Ta procedura obsługi zdarzeń są także wartości ObjectDataSource s `InsertParameters`. W związku z tym, można utworzyć program obsługi zdarzeń dla `ItemInserted` zdarzenia, które sprawdza w przypadku, gdy wystąpił wyjątek, a jeśli tak, usuwa plik określony przez ObjectDataSource s `brochurePath` parametru:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Podsumowanie

Istnieje kilka kroków, które należy wykonać w celu zapewnienia interfejsu opartego na sieci web do dodawania rekordów, które zawierają dane binarne. Jeśli bezpośrednio do bazy danych przechowywane są dane binarne, jest szansa, że musisz zaktualizować architektury, dodawania konkretnych metod, aby obsłużyć przypadek, w którym wstawiono dane binarne. Gdy architektura została zaktualizowana, następnym krokiem jest utworzenie Wstawianie interfejs, który można osiągnąć za pomocą DetailsView, który został dostosowany do uwzględnienia kontrolki FileUpload dla każdego pola danych binarnych. Można następnie przekazane dane zapisane w systemie plików serwera s sieci web lub przypisane do parametru źródła danych, w tym s DetailsView `ItemInserting` programu obsługi zdarzeń.

Zapisywanie danych binarnych w systemie plików wymaga planowania więcej niż zapisywanie danych bezpośrednio do bazy danych. Schemat nazewnictwa musi być wybrana w celu uniknięcia jeden przekazywania użytkownika s, zastępując inny s. Ponadto dodatkowe kroki należy usunąć przekazany plik, jeśli wstawienie bazy danych nie powiedzie się.

Teraz mamy możliwość dodawania nowych kategorii do systemu za pomocą broszurę i obraz, ale ve jeszcze, aby przyjrzeć się sposobie aktualizowania istniejących danych binarnych s kategorii lub jak poprawnie usunąć dane binarne usunięto kategorię. W następnym samouczku przyjrzymy się dwa następujące tematy.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Dave Gardner Teresa Murphy i Bernadette Leigh. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-binary-data-in-the-data-web-controls-cs.md)
> [dalej](updating-and-deleting-existing-binary-data-cs.md)
