---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: "W tym pliku Przekaż opcja podczas dodawania nowego rekordu (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku pokazano, jak utworzyć interfejs sieci Web, który umożliwia użytkownikowi zarówno wprowadź dane tekstowe i przekazać pliki binarne. Aby zilustrować t dostępne opcje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: 384251e5d0d72c6d1cc014c929a5d504be11d1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Opcja przekazywania pliku w tym podczas dodawania nowego rekordu (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) lub [pobierania plików PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> W tym samouczku pokazano, jak utworzyć interfejs sieci Web, który umożliwia użytkownikowi zarówno wprowadź dane tekstowe i przekazać pliki binarne. Aby zilustrować opcje dostępne do przechowywania danych binarnych, jeden plik zostaną zapisane w bazie danych, podczas gdy druga jest przechowywany w systemie plików.


## <a name="introduction"></a>Wprowadzenie

W poprzednich dwóch samouczki możemy przedstawione techniki do przechowywania danych binarnych, który jest skojarzony z modelem danych aplikacji s, po zapoznaniu się jak używać kontrolki przekazywaniem plików do wysyłania plików z klienta do serwera sieci web i pokazano, jak do przedstawienia tych danych binarnych w danych W Formant EB. Firma Microsoft kolejnych jeszcze, aby porozmawiać na temat sposobu kojarzenia przekazywane dane z modelu danych, mimo że.

W tym samouczku utworzymy strony sieci web, aby dodać nową kategorię. Oprócz pól tekstowych dla kategorii s nazwę i opis ta strona należy włączyć dwa formanty przekazywaniem plików dla nowego obrazu kategorii s i jeden dla broszury. Obraz przekazany będą przechowywane bezpośrednio w nowym rekordzie s `Picture` kolumny, natomiast broszury zostanie zapisana na `~/Brochures` folderu o ścieżce do pliku zapisywane w nowym rekordzie s `BrochurePath` kolumny.

Przed utworzeniem tej nowej strony sieci web, musimy zaktualizować architekturę. `CategoriesTableAdapter` s główne zapytanie nie pobiera `Picture` kolumny. W rezultacie automatycznie generowanej `Insert` metoda zawiera tylko dane wejściowe dla `CategoryName`, `Description`, i `BrochurePath` pól. W związku z tym należy utworzyć dodatkową metodę w TableAdapter, które monituje wszystkie cztery `Categories` pola. `CategoriesBLL` Klasy warstwy logiki biznesowej będzie również muszą zostać zaktualizowane.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Krok 1: Dodawanie`InsertWithPicture`metody`CategoriesTableAdapter`

Gdy utworzyliśmy `CategoriesTableAdapter` w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczka został skonfigurowany do automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcje na podstawie głównego zapytania. Ponadto możemy instrukcją TableAdapter fragmentów DB bezpośredniego podejście, które tworzone metody `Insert`, `Update`, i `Delete`. Wykonanie tych metod generowanych automatycznie `INSERT`, `UPDATE`, i `DELETE` instrukcje i w związku z tym, Zaakceptuj parametrów wejściowych, na podstawie kolumn zwrócony przez główne zapytanie. W [przesyłanie plików](uploading-files-cs.md) samouczku będziemy rozszerzony `CategoriesTableAdapter` s główne zapytanie do używania `BrochurePath` kolumny.

Ponieważ `CategoriesTableAdapter` s główne zapytanie nie odwołuje się `Picture` kolumny, firma Microsoft nie można dodać nowego rekordu ani zaktualizować istniejący rekord z wartością dla `Picture` kolumny. Aby przechwytywać tych informacji, możemy utworzyć nowej metody w TableAdapter, używany jest przeznaczony do wstawienia rekordu z danych binarnych lub można dostosować, generowanych automatycznie `INSERT` instrukcji. Problem z Dostosowywanie generowanych automatycznie `INSERT` instrukcja jest firma Microsoft ryzyka o naszych dostosowania zastąpione przez kreatora. Załóżmy na przykład możemy dostosować `INSERT` instrukcji, aby uwzględnić użycie `Picture` kolumny. Ta operacja spowoduje zaktualizowanie TableAdapter s `Insert` metodę w celu uwzględnienia dodatkowe parametru wejściowego dla danych binarnych obraz s kategorii s. Następnie można utworzyć metody w warstwę logiki biznesowej, ta metoda DAL i wywoływać tej metody logiki warstwy Biznesowej przez warstwę prezentacji, a wszystko bajeczną będzie działać. Oznacza to po ponownym został skonfigurowany za pomocą Kreatora konfiguracji TableAdapter TableAdapter. Jak kreatora zakończona naszych dostosowania `INSERT` instrukcji zostaną zastąpione, `Insert` metody powodują do postaci stary i naszego kodu nie może skompilować!

> [!NOTE]
> Tego określenia dokuczliwości jest z systemem innym niż problem, korzystając z procedur składowanych zamiast instrukcji SQL ad hoc. Samouczek przyszłych może zapoznać się przy użyciu procedur składowanych zamiast instrukcji SQL ad hoc w warstwie dostępu do danych.


Aby uniknąć tego potencjalnego bólem głowy zamiast Dostosowywanie instrukcji SQL automatycznie generowanej umożliwiają s zamiast utworzenie nowej metody TableAdapter. Ta metoda o nazwie `InsertWithPicture`, będzie akceptować wartości `CategoryName`, `Description`, `BrochurePath`, i `Picture` kolumny, a następnie wykonaj `INSERT` instrukcji, która przechowuje wszystkie cztery wartości w nowym rekordzie.

Otwórz zestaw danych wpisany i przy użyciu projektanta, kliknij prawym przyciskiem myszy `CategoriesTableAdapter` nagłówka s i z menu kontekstowego wybierz polecenie Dodaj zapytanie. Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter, która rozpoczyna się od udzielenia sposób zapytanie TableAdapter powinno dostęp do bazy danych. Wybierz instrukcji SQL użyj, a następnie kliknij przycisk Dalej. Następnym krokiem wyświetli monit o typ zapytania do wygenerowania. Ponieważ firma Microsoft re Trwa tworzenie zapytania, aby dodać nowy rekord do `Categories` tabeli, wybierz polecenie Wstaw i kliknij przycisk Dalej.


[![Wybierz opcję WSTAWIANIA](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Rysunek 1**: wybierz opcję Wstaw ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Teraz musisz określić `INSERT` instrukcji SQL. Kreator automatycznie sugeruje `INSERT` instrukcji odpowiadający główne zapytanie TableAdapter s. W takim przypadku go s `INSERT` instrukcji, która wstawia `CategoryName`, `Description`, i `BrochurePath` wartości. Aktualizacja instrukcji, aby `Picture` kolumny jest dołączany wraz z `@Picture` parametru, w następujący sposób:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Na ekranie końcowym kreatora zapyta nam na nazwę nowej metody TableAdapter. Wprowadź `InsertWithPicture` i kliknij przycisk Zakończ.


[![Nazwa nowej InsertWithPicture metody TableAdapter](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Rysunek 2**: Nazwa nowej metody TableAdapter `InsertWithPicture` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Krok 2: Aktualizowanie warstwy logiki biznesowej

Ponieważ warstwę prezentacji tylko powinien współpracować z warstwy logiki biznesowej, a nie pomijanie, aby przejść bezpośrednio do Warstwa dostępu do danych, należy utworzyć metody logiki warstwy Biznesowej, która wywołuje metodę DAL właśnie utworzony (`InsertWithPicture`). W tym samouczku, można utworzyć metody w `CategoriesBLL` klasy o nazwie `InsertWithPicture` który akceptuje trzy jako nakłady `string` s i `byte` tablicy. `string` Parametry wejściowe są nazwy kategorii s, opis i ścieżka pliku broszura, podczas gdy `byte` tablica jest binarny zawartości obrazu kategorii s. Poniższy kod ilustruje tę metodę logiki warstwy Biznesowej wywołuje odpowiedniej metody DAL:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Upewnij się, że przed dodaniem zapisano DataSet wpisany `InsertWithPicture` metodę logiki warstwy Biznesowej. Ponieważ `CategoriesTableAdapter` kod klasy jest generowane automatycznie na podstawie zestawu typu danych ADAM t najpierw zapisania zmian do zestawu danych typu `Adapter` właściwości won t wiedzieć o `InsertWithPicture` metody.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Krok 3: Umieszczanie na liście istniejące kategorie oraz ich dane binarne

W tym samouczku utworzymy strony, który umożliwia użytkownikowi końcowemu dodać nową kategorię do systemu, podając obrazu i broszura nowej kategorii. W [poprzedniego samouczek](displaying-binary-data-in-the-data-web-controls-cs.md) użyliśmy Element GridView z elementu ImageField i TemplateField, aby umieścić nazwę kategorii s, opis, obrazu i łącze do pobrania jego broszura. Umożliwiają replikowanie te funkcje w tym samouczku, tworzenie strony, która zawiera listę wszystkich istniejących kategorii, a także zezwala na nowe, należy utworzyć s.

Uruchamianie przez otwarcie `DisplayOrDownload.aspx` strony z `BinaryData` folderu. Przejdź do widoku źródłowego i skopiuj GridView i ObjectDataSource s składni deklaratywnej, wklejając je w ramach `<asp:Content>` element `UploadInDetailsView.aspx`. Ponadto ADAM t Pamiętaj, aby skopiować `GenerateBrochureLink` metody z klasę CodeBehind `DisplayOrDownload.aspx` do `UploadInDetailsView.aspx`.


[![Skopiuj i Wklej składni deklaratywnej z DisplayOrDownload.aspx UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Rysunek 3**: kopiowanie i wklejanie składni deklaratywnej z `DisplayOrDownload.aspx` do `UploadInDetailsView.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Po skopiowaniu składni deklaratywnej i `GenerateBrochureLink` metodę za pośrednictwem `UploadInDetailsView.aspx` pozycję Wyświetl stronę za pośrednictwem przeglądarki, aby upewnić się, że wszystko zostało za pośrednictwem poprawnie skopiowany. Powinny pojawić się GridView osiem kategorie lista zawiera łącze do pobrania broszury, a także obraz kategorii s.


[![Powinien zostać wyświetlony każdej kategorii, wraz z jego dane binarne](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Rysunek 4**: możesz powinny teraz Zobacz każdy kategorii wraz z jego danych binarnych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Krok 4: Konfigurowanie`CategoriesDataSource`do obsługi Wstawianie

`CategoriesDataSource` ObjectDataSource używane przez `Categories` GridView aktualnie nie zapewnia możliwość wstawiania danych. Aby zapewnić obsługę wstawianie za pośrednictwem tego formantu źródła danych, należy zamapować jej `Insert` metody do metody w jego obiekt `CategoriesBLL`. W szczególności, jeśli chcesz mapowany do `CategoriesBLL` metody dodaliśmy w kroku 2 `InsertWithPicture`.

Uruchomić, klikając łącze Konfigurowanie źródła danych z elementu ObjectDataSource tag inteligentny s. Pierwszy ekran pokazuje obiekt źródła danych jest skonfigurowane do pracy z `CategoriesBLL`. Pozostaw to ustawienie jako — jest i kliknij przycisk Dalej, przejście do ekranu definiować metody dotyczące danych. Przenieś na kartę Wstawianie i wybierz `InsertWithPicture` metodę z listy rozwijanej. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Skonfiguruj element ObjectDataSource przy użyciu metody InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Rysunek 5**: Konfigurowanie ObjectDataSource do użycia `InsertWithPicture` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Po zakończeniu działania kreatora programu Visual Studio może wystąpić, jeśli chcesz Odśwież i klucze, który spowoduje ponowne wygenerowanie danych sieci Web kontrolki pola. Wybierz opcję nie, ponieważ wybranie opcji Tak spowoduje zastąpienie wszelkich dostosowań pola, które mogły zostać wprowadzone.


Po zakończeniu działania kreatora ObjectDataSource teraz będzie zawierać wartość dla jego `InsertMethod` właściwości oraz `InsertParameters` dla kategorii czterech kolumn, jako następujący kod deklaratywne przedstawiono:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Krok 5: Tworzenie interfejsu Wstawianie

Jak najpierw objęte [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), formantu widoku DetailsView udostępnia interfejs Wstawianie wbudowanych, które mogą zostać użyte podczas pracy z kontrolą źródła danych, który obsługuje wstawiania. Umożliwiają dodawanie formantu widoku DetailsView do tej strony powyżej widoku GridView zostanie trwale renderowanie interfejsie Wstawianie Umożliwianie użytkownikowi szybko dodać nową kategorię s. Po dodaniu nowej kategorii w widoku DetailsView, widoku GridView poniżej zostaną automatycznie Odśwież i Wyświetl nowej kategorii.

Start, wystarczy przeciągnąć element DetailsView z przybornika do projektanta powyżej widoku GridView ustawienie jej `ID` właściwości `NewCategory` i wyczyszczenie `Height` i `Width` wartości właściwości. Z tagów inteligentnych s widoku DetailsView powiązać go z istniejącą `CategoriesDataSource` , a następnie zaznacz pole wyboru Włącz wstawianie.


[![Powiązać widoku DetailsView CategoriesDataSource i Włącz wstawianie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Rysunek 6**: Bind do widoku DetailsView `CategoriesDataSource` i Włącz wstawianie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Aby trwale renderowania widoku DetailsView w interfejsie Wstawianie, ustaw jej `DefaultMode` właściwości `Insert`.

Należy zauważyć, że widoku DetailsView ma pięć BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, i `BrochurePath` chociaż `CategoryID` elementu BoundField nie jest wyświetlana w interfejsie Wstawianie, ponieważ jego `InsertVisible` Właściwość jest ustawiona na `false`. Te BoundFields nie istnieje, ponieważ są one kolumny zwracane przez `GetCategories()` metodę, która jest ObjectDataSource wywołuje można pobrać danych. Podczas wstawiania, jednak firma Microsoft ADAM chcesz t użytkownikowi należy określić wartość dla `NumberOfProducts`. Ponadto należy zezwolić im na przekazywanie obrazu dla nowej kategorii, a także Przekaż PDF dla broszury.

Usuń `NumberOfProducts` elementu BoundField z widoku DetailsView całkowicie i następnie zaktualizuj `HeaderText` właściwości `CategoryName` i `BrochurePath` BoundFields do kategorii i Brochure, odpowiednio. Następnie Konwertuj `BrochurePath` elementu BoundField na pole TemplateField i Dodaj nowe pole TemplateField obrazu, podając ten nowy TemplateField `HeaderText` wartość obrazu. Przenieś `Picture` TemplateField, tak aby była między `BrochurePath` TemplateField i CommandField.


![Powiązać widoku DetailsView CategoriesDataSource i Włącz wstawianie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Rysunek 7**: Bind do widoku DetailsView `CategoriesDataSource` i Włącz wstawianie


Jeśli można przekonwertować `BrochurePath` obejmuje TemplateField elementu BoundField na pole TemplateField za pomocą okna dialogowego Edytuj pola, `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate`. Tylko `InsertItemTemplate` jest wymagane, jednak, więc możesz usunąć innych szablonów. W tym momencie widoku DetailsView składni deklaratywnej s powinna wyglądać następująco:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Dodawanie formantów przekazywaniem plików broszura i pola obrazów

Obecnie usługa `BrochurePath` TemplateField s `InsertItemTemplate` zawiera pole tekstowe, podczas gdy `Picture` TemplateField nie zawiera żadnych szablonów. Należy zaktualizować te dwie s TemplateField `InsertItemTemplate` s w celu używania kontroli przekazywaniem plików.

W widoku DetailsView s tagu, wybierz opcję Edytuj szablony, a następnie wybierz `BrochurePath` TemplateField s `InsertItemTemplate` z listy rozwijanej. Usuń pole tekstowe, a następnie przeciągnij formant przekazywaniem plików z przybornika do szablonu. Ustawianie formantu przekazywaniem plików s `ID` do `BrochureUpload`. Podobnie, Dodaj formant przekazywaniem plików do `Picture` TemplateField s `InsertItemTemplate`. Ustaw ten formant przekazywaniem plików s `ID` do `PictureUpload`.


[![Dodawanie formantu przekazywaniem plików do InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Rysunek 8**: Dodawanie formantu przekazywaniem plików do `InsertItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Po wprowadzeniu tych dodatków, będą dwa składni deklaratywnej s TemplateField:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Gdy użytkownik dodaje nową kategorię, chcemy upewnij się, że broszura i obraz typu poprawny plik. Dla brochure użytkownik musi podać pliku PDF. Dla obrazu, potrzebujemy użytkownika, aby przekazać plik obrazu, ale możemy pozwalają *żadnych* obrazu pliku lub tylko pliki obrazów określonego typu, na przykład GIF lub jpg? Aby umożliwić dla różnych typów plików, d należy rozszerzyć `Categories` schemat w celu uwzględnienia kolumny, która przechwytuje typ pliku, dzięki czemu tego typu mogą być wysyłane do klienta za pośrednictwem `Response.ContentType` w `DisplayCategoryPicture.aspx`. Ponieważ firma Microsoft ADAM t kolumnie jest, byłoby rozsądne ograniczające użytkowników, aby udostępniać tylko typu pliku określonego obrazu. `Categories` Tabeli s istniejących obrazów są mapy bitowe, ale jpg są bardziej odpowiednie format pliku dla obrazów obsługiwanych w sieci web.

Jeśli użytkownik prześle niepoprawny typ pliku, należy usunąć insert i wyświetlić komunikat informujący o problem. Dodawanie formantu etykiety Web poniżej widoku DetailsView. Ustaw jego `ID` właściwości `UploadWarning`, wyczyść limit jego `Text` właściwość, ustaw `CssClass` właściwości na ostrzeżenie i `Visible` i `EnableViewState` właściwości `false`. `Warning` CSS klasa jest zdefiniowana w `Styles.css` i renderuje tekst w dużych, czerwony, kursywy pogrubioną czcionkę.

> [!NOTE]
> W idealnym przypadku `CategoryName` i `Description` BoundFields zostanie przekonwertowana na TemplateFields i ich wstawianie interfejsy dostosowane. `Description` Wstawianie interfejsu, na przykład czy prawdopodobnie można lepiej dostosowane przez wielowierszowego pola tekstowego. A ponieważ `CategoryName` kolumny nie akceptuje `NULL` wartości, RequiredFieldValidator powinny zostać dodane do zapewnienia użytkownika zawiera wartość dla nowej nazwy kategorii s. Te kroki są pozostawiane jako wykonywania do czytnika. Odwołaj się do [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) dla omówiono przestarzałe interfejsy modyfikacji danych.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Krok 6: Zapisywanie broszura przekazane do systemu plików s serwera sieci Web

Gdy użytkownik wprowadza wartości dla nowej kategorii i kliknie przycisk Wstaw, występuje odświeżania strony i rozwoju Wstawianie przepływu pracy. Po pierwsze, s widoku DetailsView [ `ItemInserting` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) uruchamiany. Następnie, ObjectDataSource s `Insert()` wywoływana jest metoda, która powoduje dodawany do nowego rekordu `Categories` tabeli. Po wykonaniu tej s widoku DetailsView [ `ItemInserted` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) uruchamiany.

Przed ObjectDataSource s `Insert()` wywołania metody, firma Microsoft musi najpierw upewnij się, że odpowiednie typy zostały przekazane przez użytkownika, a następnie zapisz broszura PDF w systemie plików serwera s sieci web. Tworzenie procedury obsługi zdarzeń dla widoku DetailsView s `ItemInserting` zdarzeń i Dodaj następujący kod:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Uruchamia program obsługi zdarzeń za pomocą odwołań do `BrochureUpload` kontroli przekazywaniem plików za pomocą szablonów s widoku DetailsView. Następnie jeśli broszurę został przekazany, rozszerzenia s przekazanego pliku jest badany. Jeśli nie jest rozszerzeniem. PDF, a następnie ostrzeżenie jest wyświetlane, Wstaw zostało anulowane i kończy wykonywanie programu obsługi zdarzeń.

> [!NOTE]
> Jednostki uzależnionej w rozszerzeniu s przesłanego pliku nie jest stosowane technika zapewnia, że przekazanego pliku jest dokument PDF. Użytkownik może mieć nieprawidłowy dokument PDF z rozszerzeniem `.Brochure`, lub można podjąć dokumentu PDF z systemem innym niż i jego podane `.pdf` rozszerzenia. Zawartość pliku binarnego s należy programowo należy zbadać w celu bardziej ostatecznie Sprawdź typ pliku. Takiego podejścia dokładnego, jednak są często zbyt obszerne; Sprawdzanie, czy rozszerzenie jest wystarczające dla większości scenariuszy.


Zgodnie z opisem w [przesyłanie plików](uploading-files-cs.md) samouczek, należy zachować ostrożność podczas zapisywania plików w systemie plików, tak że jeden użytkownik s przekazywania nie powoduje zastąpienia s innej. W tym samouczku firma Microsoft podejmie próbę użycia taką samą nazwę jak przekazanego pliku. Jeśli istnieje już plik w `~/Brochures` katalog o tej samej nazwie, jednak firma Microsoft będzie append numer na końcu aż do znalezienia unikatową nazwę. Na przykład, jeśli użytkownik prześle broszura plik o nazwie `Meats.pdf`, ale istnieje już plik o nazwie `Meats.pdf` w `~/Brochures` folderu, zmienimy nazwy zapisanego pliku `Meats-1.pdf`. Jeśli który istnieje, zostanie uruchomiony `Meats-2.pdf`i tak dalej, aż do znalezienia unikatową nazwę pliku.

Poniższy kod używa [ `File.Exists(path)` metody](https://msdn.microsoft.com/library/system.io.file.exists.aspx) do ustalenia, czy istnieje już plik o takiej samej nazwie. Jeśli tak, nadal spróbuj nowe nazwy pliku broszury, aż do znalezienia nie było konfliktu.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Po znalezieniu prawidłową nazwę pliku plik musi zostaną zapisane w systemie plików i ObjectDataSource s `brochurePath``InsertParameter` wartości musi zostać zaktualizowany, tak aby ta nazwa pliku jest zapisane w bazie danych. Jak widzieliśmy w *przesyłanie plików* samouczek, plik może być zapisany przy użyciu formantu przekazywaniem plików s `SaveAs(path)` metody. Aby zaktualizować ObjectDataSource s `brochurePath` parametru, użyj `e.Values` kolekcji.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Krok 7: Zapisywania przekazanego obrazu w bazie danych

Do przechowywania obrazu przekazanego w nowym `Categories` rekordów, należy przypisać przekazane zawartość binarną do ObjectDataSource s `picture` parametru w widoku DetailsView s `ItemInserting` zdarzeń. Przed pobraniem ten przydział, jednak należy najpierw upewnij się, że przekazane obrazów JPG i nie innego obrazu typu. W kroku 6 umożliwiają s rozszerzenie pliku s obraz przekazany służy do sprawdzenia jego typu.

Gdy `Categories` tabeli umożliwia `NULL` wartości `Picture` kolumna, w obecnie wszystkie kategorie ma obrazu. Let s zmusza użytkownika do zapewnienia obrazu podczas dodawania nowej kategorii za pośrednictwem tej strony. Poniższy kod sprawdza, aby upewnić się, że obraz został przekazany oraz ma właściwe rozszerzenie.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Ten kod należy umieścić *przed* kod z kroku 6, tak aby w przypadku problemu z przekazywanie obrazu programu obsługi zdarzeń zostanie zamknięty przed zapisaniem pliku broszura w systemie plików.

Przy założeniu, że odpowiedni plik został przekazany, przypisz przekazane zawartość binarną do wartości s parametru obraz o następujący wiersz kodu:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Pełną`ItemInserting`obsługi zdarzeń

Kompletności, Oto `ItemInserting` obsługi zdarzeń w całości:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Krok 8: Ustalania`DisplayCategoryPicture.aspx`strony

Let s Poświęć chwilę, aby przetestować interfejs Wstawianie i `ItemInserting` obsługi zdarzeń, który został utworzony w ciągu ostatnich kilku kroków. Odwiedź stronę `UploadInDetailsView.aspx` strony za pośrednictwem przeglądarki, a następnie spróbuj dodać kategorię, ale pominąć obraz lub Określ obraz z systemem innym niż JPG lub broszura PDF z systemem innym niż. W żadnym z tych przypadków zostanie wyświetlony komunikat o błędzie i przepływu pracy insert anulowane.


[![Ostrzeżenie jest wyświetlane, jeśli przekazano nieprawidłowy typ pliku](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Rysunek 9**: ostrzeżenie komunikat jest wyświetlany, jeśli przekazano nieprawidłowy typ pliku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Po sprawdzeniu strona wymaga obrazu do załadowania i wykorzystanej t Akceptuj pliki PDF z systemem innym niż lub z systemem innym niż JPG, Dodaj nową kategorię nieprawidłowy obraz JPG, jeśli broszura pole pozostanie puste. Po kliknięciu przycisku Wstaw strony będzie ogłaszanie i nowy rekord zostanie dodany do `Categories` tabelę z zawartości binarnej załadowanego obrazu s bezpośrednio w bazie danych. Widoku GridView zostanie zaktualizowana i zawiera wiersz dla nowo dodanego kategorii, ale, jak pokazano na rysunku nr 10, nowy obraz s kategorii nie są odtwarzane poprawnie.


[![Nową kategorię s obraz nie będzie wyświetlany.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Na rysunku nr 10**: s nową kategorię obraz nie jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Jest przyczyna nowy obraz nie jest wyświetlany, ponieważ `DisplayCategoryPicture.aspx` strona, która zwraca obraz określona kategoria s jest skonfigurowany do przetworzenia bitmap nagłówka OLE. Ten nagłówek 78 bajtów jest usuwany z `Picture` zawartości binarnej kolumn s przed wysłaniem ich z powrotem do klienta. Ale który możemy przekazać plik JPG nowej kategorii nie ma tego nagłówka OLE; w związku z tym bajtów nieprawidłowa, konieczne są usuwane z danych binarnych s obrazu.

Ponieważ teraz są obie map bitowych z nagłówkami OLE i JPG w `Categories` tabeli, należy zaktualizować `DisplayCategoryPicture.aspx` tak, aby nie nagłówka OLE usuwanie dla oryginalnego osiem kategorii i pomija to usuwanie nowszej rekordów kategorii. W naszym samouczku dalej zajmiemy się jak zaktualizować istniejący obraz rekordu s i będzie modyfikacjom wszystkie obrazy kategorii stary, aby były jpg. Na razie jednak użyć poniższego kodu w `DisplayCategoryPicture.aspx` na usuwanie nagłówków OLE tylko dla tych oryginalnego osiem kategorii:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Dzięki tej zmianie obrazu JPG jest teraz renderowane poprawnie w widoku GridView.


[![Obrazów JPG dla nowej kategorii są renderowane poprawnie](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Rysunek 11**: obrazów JPG dla nowej kategorii są renderowane poprawnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Krok 9: Usuwanie broszura w wypadku wyjątków

Jednym z wyzwań przechowywania danych binarnych w systemie plików s serwera sieci web jest wprowadzenie rozłączenia między modelu danych i jego dane binarne. W związku z tym gdy rekord zostanie usunięty, odpowiednie dane binarne w systemie plików należy także usunąć. To może wchodzić w grę podczas wstawiania, jak również. Rozważmy następujący scenariusz: użytkownik dodaje nową kategorię, określenie nieprawidłowy obraz i broszura. Po kliknięciu przycisku Wstaw występuje odświeżania strony i s widoku DetailsView `ItemInserting` generowane zdarzenie, zapisywanie broszury w systemie plików serwera s sieci web. Następnie, ObjectDataSource s `Insert()` wywoływana jest metoda, która wywołuje `CategoriesBLL` klasy s `InsertWithPicture` metodę, która wywołuje `CategoriesTableAdapter` s `InsertWithPicture` metody.

Teraz, co się stanie, jeśli baza danych jest w trybie offline lub jeśli jest błąd `INSERT` instrukcji SQL? Wyraźnie INSERT zakończy się niepowodzeniem, i ma nowego wiersza kategorii zostaną dodane do bazy danych. Ale wciąż istnieje plik broszura przekazane z systemu plików s serwera sieci web! Ten plik należy usunąć w wypadku Wystąpił wyjątek podczas Wstawianie przepływu pracy.

Jak wspomniano wcześniej w [obsługi logiki warstwy Biznesowej i wyjątków DAL na poziomie strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczek, gdy w ramach głębokości architektury, jest on przepuszcza się za pośrednictwem różnych warstw jest zgłaszany wyjątek. W warstwie prezentacji, możemy określić, jeśli wystąpił wyjątek z widoku DetailsView s `ItemInserted` zdarzeń. Ten program obsługi zdarzeń są także wartości ObjectDataSource s `InsertParameters`. W związku z tym można utworzyć programu obsługi zdarzeń dla `ItemInserted` zdarzenie, które sprawdza, czy wystąpił wyjątek, a jeśli tak, usuwa plik określony przez element ObjectDataSource s `brochurePath` parametru:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Podsumowanie

Istnieje kilka kroków, które należy wykonać w celu zapewnienia interfejs sieci web do dodawania rekordów, które zawierają dane binarne. Jeśli dane binarne jest magazynowana bezpośrednio do bazy danych, prawdopodobnie musisz zaktualizować architekturę, dodawanie określonych metod obsługi w przypadku, gdy wstawiono danych binarnych. Po zaktualizowaniu architekturę, następnym krokiem jest utworzenie Wstawianie interfejsu, co można zrobić za pomocą widoku DetailsView, który został dostosowany do uwzględnienia kontrolkę przekazywaniem plików do każdego pola danych binarnych. Można następnie przekazywane dane zapisane w systemie plików serwera s sieci web lub przypisane do parametru źródła danych w widoku DetailsView s `ItemInserting` program obsługi zdarzeń.

Zapisywanie danych binarnych w systemie plików wymaga planowania więcej niż zapisywania danych bezpośrednio do bazy danych. Schemat nazewnictwa musi być wybrana w celu uniknięcia jeden przekazywania użytkownika s zastępowanie s innej. Ponadto dodatkowe należy przedsięwziąć insert bazy danych nie powiodło się usunięcie przekazanego pliku.

Teraz mamy możliwość dodawania nowych kategorii do systemowi broszurę i obraz, ale Zapisz jeszcze aby przyjrzeć się jak zaktualizować istniejące dane binarne s kategorii lub jak poprawnie usunąć dane binarne dla usuniętych kategorii. Firma Microsoft będzie Poznaj tych dwóch tematów w następnym samouczku.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Dave Gardner, Teresa Murphy i Bernadette Leigh. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](displaying-binary-data-in-the-data-web-controls-cs.md)
[dalej](updating-and-deleting-existing-binary-data-cs.md)
