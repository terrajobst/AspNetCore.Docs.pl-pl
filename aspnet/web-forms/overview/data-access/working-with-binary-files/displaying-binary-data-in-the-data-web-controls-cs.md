---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Wyświetlanie danych binarnych w sieci Web Data kontrolki (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przyjrzymy się opcji do prezentowania danych binarnych na stronie sieci Web, w tym wyświetlania pliku obrazu i zapewnić link "Pobierania" f...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 62cc931b670931677b4e9632dccd6634715b3c71
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814857"
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>Wyświetlanie danych binarnych w kontrolkach internetowych danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) lub [Pobierz plik PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> W tym samouczku przyjrzymy się opcji do prezentowania danych binarnych na stronie sieci Web, w tym wyświetlania pliku obrazu i zapewnienie link "Pobierania" dla pliku PDF.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku możemy zbadano dwie techniki kojarzenia dane binarne z aplikacji s podstawowy model danych i kontrolki FileUpload umożliwia przekazywanie plików z przeglądarki do systemu plików s serwera sieci web. Firma Microsoft ve jeszcze, aby zobaczyć, jak skojarzyć przekazane dane binarne z modelu danych. Oznacza to po plik został przekazany i zapisane w systemie plików, ścieżka do pliku musi być przechowywany w rekordzie odpowiednią bazą danych. Jeśli danych jest magazynowana bezpośrednio w bazie danych, następnie przekazane dane binarne nie muszą zostać zapisane w system plików, ale muszą zostać wprowadzone do bazy danych.

Zanim przyjrzymy się kojarzenie danych z modelu danych, jednak chętnie s Pierwsze spojrzenie na sposób udostępniania danych binarnych dla użytkownika końcowego. Prezentacja danych tekstowych jest prosta, ale jak dane binarne przedstawiane? To zależy, oczywiście, typ danych binarnych. W przypadku obrazów prawdopodobnie chcemy wyświetlić obrazu. dla plików PDF dokumentów programu Microsoft Word, pliki ZIP i inne rodzaje danych binarnych, zapewniając link pobierania jest prawdopodobnie bardziej odpowiednie.

W tym samouczku przedstawiony zostanie sposób prezentowania danych binarnych wraz z jego danych skojarzony tekst przy użyciu danych w sieci Web kontroluje takich jak GridView i DetailsView. W następnym samouczku firma Microsoft będzie włączyć naszej uwagi do kojarzenia przekazanego pliku z bazą danych.

## <a name="step-1-providingbrochurepathvalues"></a>Krok 1: Zapewnianie`BrochurePath`wartości

`Picture` Kolumny w `Categories` tabela zawiera już dane binarne dla różnych obrazów kategorii. W szczególności `Picture` kolumny dla każdego rekordu znajduje się zawartość binarne mapy bitowej ziarnisty niskiej jakości, 16 kolorów. Każdy obraz kategorii jest 172 pikseli szerokości i 120 pikseli wysokości i zużywa około 11 KB. Jakie więcej s, zawartość binarną w `Picture` kolumna zawiera 78-bajtowych [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) nagłówek, który musi być usunięte przed wyświetleniem obrazu. Te informacje nagłówka występuje, ponieważ bazy danych Northwind ma korzenie jego programu Microsoft Access. W programie Access są przechowywane dane binarne, przy użyciu typu danych obiektu OLE, który gwoździe w nagłówku. Teraz zobaczymy jak usuwanie nagłówków z tych obrazów w niskiej jakości, aby wyświetlić obraz. W przyszłości samouczku utworzymy interfejsu do aktualizowania kategorii s `Picture` kolumny i zastąpić te obrazy mapy bitowej, korzystających z nagłówków OLE przy użyciu równoważne obrazów JPG bez niepotrzebnych nagłówków OLE.

Jak używać kontrolki FileUpload widzieliśmy w poprzednim samouczku. W związku z tym można Przejdź dalej i Dodaj broszura pliki do systemu plików s serwera sieci web. Ten sposób, jednak nie powoduje aktualizacji `BrochurePath` kolumny w `Categories` tabeli. W następnym samouczku zobaczymy, jak to zrobić, ale na razie będziemy musieli ręcznie Podaj wartości dla tej kolumny.

W tym samouczku s znajdziesz siedem broszura pliki PDF `~/Brochures` folderu, po jednym dla każdej kategorii, z wyjątkiem owoce morza. Czy mogę celowo pominięty, dodawanie broszura owoce morza, aby zilustrować procedurę do obsługi scenariuszy, w którym nie wszystkie rekordy są skojarzone dane binarne. Aby zaktualizować `Categories` tabeli z tymi wartościami, kliknij prawym przyciskiem myszy `Categories` węzła z poziomu Eksploratora serwera i wybierz polecenie Pokaż dane tabeli. Następnie wprowadź wirtualnej ścieżki do plików broszura dla każdej kategorii, która ma brochure, tak jak pokazano na rysunku 1. Ponieważ nie ma żadnych innej kategorii owoce morza, pozostaw swoją `BrochurePath` wartość kolumny s jako `NULL`.


[![Ręcznie wprowadź wartości dla kolumny BrochurePath tabeli s kategorie](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Rysunek 1**: ręcznie wprowadzić wartości `Categories` tabeli s `BrochurePath` kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Krok 2: Zapewnienie Link pobierania dla broszury w widoku GridView

Za pomocą `BrochurePath` wartości podanych dla `Categories` tabeli, możemy ponownie gotowe do utworzenia GridView zawierającego każdej kategorii oraz link umożliwiający pobranie broszura kategorii s. W kroku 4 będziemy rozszerzać ten GridView można również wyświetlić obraz kategorii s.

Rozpocznij, przeciągając je z przybornika do projektanta w kontrolce GridView `DisplayOrDownloadData.aspx` stronie `BinaryData` folderu. Ustaw GridView s `ID` do `Categories` i za pośrednictwem tagu inteligentnego s GridView wybierz powiązać go z nowego źródła danych. W szczególności powiązać kontrolki ObjectDataSource o nazwie `CategoriesDataSource` , który pobiera dane przy użyciu `CategoriesBLL` obiektu s `GetCategories()` metody.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Rysunek 2**: Utwórz nowy o nazwie elementu ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Rysunek 3**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![Pobieranie listy kategorii przy użyciu metody GetCategories()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Rysunek 4**: pobieranie listy z kategorii przy użyciu `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio automatycznie doda do elementu BoundField `Categories` GridView dla `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, i `BrochurePath` `DataColumn` s. Przejdź dalej i Usuń `NumberOfProducts` elementu BoundField od `GetCategories()` metoda s zapytanie nie pobrać tych informacji. To również usunięcie `CategoryID` elementu BoundField i Zmień nazwę `CategoryName` i `BrochurePath` BoundFields `HeaderText` właściwości do kategorii i Brochure, odpowiednio. Po wprowadzeniu tych zmian, z kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Wyświetl tę stronę za pośrednictwem przeglądarki (zobacz rysunek 5). Każda z tych kategorii osiem jest wymieniony. Siedem kategorii z `BrochurePath` wartości mają `BrochurePath` wartość wyświetloną w odpowiedniej elementu BoundField. Owoce morza, która ma `NULL` wartość jego `BrochurePath`, wyświetla pustą komórkę.


[![Każda kategoria s nazwa, opis i wartość BrochurePath znajduje się na liście](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Rysunek 5**: każdej kategorii s nazwa, opis, a `BrochurePath` wartość znajduje się na liście ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


Zamiast wyświetlania tekstu `BrochurePath` kolumny, chcemy, aby utworzyć łącze do broszurę. Aby to zrobić, należy usunąć `BrochurePath` elementu BoundField i Zastąp pole hiperłącza HyperLinkField. Ustaw nowe s pole hiperłącza HyperLinkField `HeaderText` właściwość Brochure, jego `Text` właściwość broszura widoku i jego `DataNavigateUrlFields` właściwość `BrochurePath`.


![Dodaj pole hiperłącza HyperLinkField dla BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Rysunek 6**: Dodaj pole hiperłącza HyperLinkField dla `BrochurePath`


Kolumna łącza spowoduje to dodanie do kontrolki GridView, jak pokazano na rysunku 7. Kliknięcie łącza broszura widok będzie wyświetlić pliku PDF bezpośrednio w przeglądarce lub monitować użytkownika o pobranie pliku, w zależności od tego, czy zainstalowano czytnik plików PDF oraz ustawienia przeglądarki s.


[![Broszura s kategorii można wyświetlić, klikając łącze Wyświetl broszura](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Rysunek 7**: s kategorii broszura można wyświetlić, klikając łącze Wyświetl broszura ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![Kategoria s broszura PDF jest wyświetlana.](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Rysunek 8**: s kategorii broszura PDF jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ukrywanie tekstu broszura widoku dla kategorii bez broszurę

Jak pokazano na rysunku 7, `BrochurePath` Wyświetla pole hiperłącza HyperLinkField jego `Text` wartość właściwości (Widok broszura) dla wszystkich rekordów, niezależnie od tego, czy tamtego s innej niż`NULL` wartość `BrochurePath`. Oczywiście jeśli `BrochurePath` jest `NULL`, a następnie łącze jest wyświetlane jako tekst, tak jak w przypadku kategorii owoce morza (odnoszą się do rysunek 7). Zamiast wyświetlania tekstu broszura widoku, może być przydatne tych kategorii bez `BrochurePath` wartość wyświetlana jakiś tekst alternatywny, takich jak brak broszura dostępne.

Aby zapewnić to zachowanie, należy wykonać TemplateField, w których zawartość jest generowana przez wywołanie metody strony, który emituje odpowiednie dane wyjściowe na podstawie `BrochurePath` wartości. Najpierw rozważyliśmy, to w formatowanie technika w [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) samouczka.

Przekształcając pole hiperłącza HyperLinkField TemplateField, wybierając `BrochurePath` pole hiperłącza HyperLinkField i klikając Konwertuj tego pola do TemplateField link w oknie dialogowym Edytowanie kolumn.


![Konwertuj pole hiperłącza HyperLinkField na TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Rysunek 9**: konwertowanie pole hiperłącza HyperLinkField TemplateField


Spowoduje to utworzenie TemplateField z `ItemTemplate` zawierający Web hiperłącze formant, którego `NavigateUrl` właściwość jest powiązana z `BrochurePath` wartości. Zamień ten kod znaczników wywołanie do metody `GenerateBrochureLink`, przekazując wartość `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Następnie należy utworzyć `protected` metody w ASP.NET stronie s związanym z kodem klasę o nazwie `GenerateBrochureLink` zwracającego `string` i akceptuje `object` jako parametr wejściowy.


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Ta metoda określa, czy przekazywany do `object` wartość jest bazą danych `NULL` i jeśli tak, zwraca komunikat wskazujący, że kategorii nie posiada broszurę. W przeciwnym razie, jeśli istnieje `BrochurePath` wartość, jej s wyświetlane w hiperlink. Należy pamiętać, że jeśli `BrochurePath` wartość prezentować przekazanych do [ `ResolveUrl(url)` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Ta metoda jest rozpoznawana jako przekazany do *adresu url*, zastępując `~` znak z odpowiednią ścieżką wirtualną. Na przykład, jeśli aplikacja w `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` zwróci `/Tutorial55/Brochures/Meat.pdf`.

Na rysunku nr 10 przedstawiono strony, po zastosowaniu tych zmian. Należy pamiętać, że kategorii owoce morza s `BrochurePath` pole zawiera teraz dostępne broszura bez tekstu.


[![Tekst nie broszura dostępne będą wyświetlane te kategorie bez broszura](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Na rysunku nr 10**: tekst nie broszura dostępne będą wyświetlane te kategorie bez broszura ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Krok 3: Dodawanie strony sieci Web, aby wyświetlić obraz kategorii s

Gdy użytkownik odwiedzi strony ASP.NET, otrzymują one ASP.NET, strony HTML. Odebrano HTML jest po prostu tekst i nie zawiera żadnych danych binarnych. Dodatkowe dane binarne, takie jak obrazy, pliki dźwiękowe, aplikacje Macromedia Flash, osadzone wideo Windows Media Player i tak dalej, istnieje jako oddzielne zasoby na serwerze sieci web. Kod HTML zawiera odwołania do tych plików, ale nie obejmuje rzeczywistej zawartości plików.

Na przykład w formacie HTML `<img>` element jest używany do odwoływać się do obrazu z `src` atrybut wskazuje plik obrazu w następujący sposób:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Gdy przeglądarka odbierze kod HTML, kieruje kolejne żądanie do serwera sieci web, aby pobrać zawartość binarny plik obrazu, który następnie wyświetla w przeglądarce. Tę samą koncepcję dotyczy dowolne dane binarne. W kroku 2 broszurę nie została wysłana w dół do przeglądarki jako część strony s Kod znaczników HTML. Zamiast renderowany kod HTML pod warunkiem hiperłącza, po kliknięciu, spowodowane przeglądarkę, aby bezpośrednio dokumentu PDF.

Aby wyświetlić lub Zezwalaj użytkownikom na pobieranie danych binarnych, które znajdują się w bazie danych, należy utworzyć oddzielne strony sieci web, która zwraca dane. Dla naszej aplikacji tam s przechowywane bezpośrednio na obrazie bazy danych s kategorii tylko jedno pole danych binarnych. W związku z tym, potrzebujemy strony, gdy zostanie wywołana, zwraca dane obrazu dla określonej kategorii.

Dodawanie nowej strony programu ASP.NET do `BinaryData` folder o nazwie `DisplayCategoryPicture.aspx`. Po wykonaniu tej czynności nie zaznaczaj pola wyboru Wybierz stronę wzorcową wyboru. Oczekuje, że ta strona `CategoryID` wartości querystring i zwraca dane binarne z tej kategorii s `Picture` kolumny. Ponieważ ta strona zwraca dane binarne i nic, nie wymaga żadnych znaczników w sekcji kodu HTML. W związku z tym, kliknij na karcie Źródło w lewym dolnym rogu i Usuń wszystkie znaczniki strony s, z wyjątkiem `<%@ Page %>` dyrektywy. Oznacza to, że `DisplayCategoryPicture.aspx` oznaczeniu deklaracyjnym s powinna składać się z jednego wiersza:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Jeśli widzisz `MasterPageFile` atrybutu w `<%@ Page %>` dyrektywy, usuń go.

W klasie CodeBehind strony s, Dodaj następujący kod do `Page_Load` program obsługi zdarzeń:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Ten kod uruchamia przez wczytanie `CategoryID` wartości querystring w zmiennej o nazwie `categoryID`. Następnie dane obrazu są pobierane przez wywołanie `CategoriesBLL` klasy s `GetCategoryWithBinaryDataByCategoryID(categoryID)` metody. Te dane są zwracane do klienta przy użyciu `Response.BinaryWrite(data)` metody, ale przed jest to nazywane `Picture` nagłówek OLE wartości s kolumny muszą zostać usunięte. Jest to osiągane przez utworzenie `byte` tablicy o nazwie `strippedImageData` który będzie przechowywać dokładnie 78 znaków mniejszej niż co `Picture` kolumny. [ `Array.Copy` Metoda](https://msdn.microsoft.com/library/z50k9bft.aspx) służy do kopiowania danych z `category.Picture` zaczynając od pozycji 78 za pośrednictwem do `strippedImageData`.

`Response.ContentType` Właściwość określa [typ MIME](http://en.wikipedia.org/wiki/MIME) zawartości zostały zwrócone, tak aby przeglądarki wie, jak renderować ją. Ponieważ `Categories` tabeli s `Picture` kolumna jest obraz mapy bitowej, mapy bitowej typ MIME jest używany tutaj (obraz/bmp). Jeżeli pominięto typu MIME w większości przeglądarek nadal wyświetli obraz poprawnie ponieważ one może wywnioskować typ na podstawie zawartości danych binarnych s pliku obrazu. Jednak jego typu s, aby uwzględnić MIME, gdy jest to możliwe. Zobacz [Internet Assigned Numbers Authority witryny](http://www.iana.org/) Pełna lista [typów nośników MIME](http://www.iana.org/assignments/media-types/).

Z tą stroną, utworzony obraz określoną kategorię s mogą być wyświetlane, odwiedzając stronę `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Na ilustracji 11 pokazano obrazu s kategorii Beverages, co mogą być wyświetlane z `DisplayCategoryPicture.aspx?CategoryID=1`.


[![S należące do kategorii, który jest wyświetlany obraz](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Rysunek 11**: s należące do kategorii jest wyświetlany obraz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


Jeśli podczas odwiedzania `DisplayCategoryPicture.aspx?CategoryID=categoryID`, Uzyskaj odczytujący można zrzutować obiektu typu "System.DBNull" na typ "[] System.Byte" wyjątek, istnieją dwie czynności, które mogą być przyczyną tego. Po pierwsze, `Categories` tabeli s `Picture` zezwala na kolumnę `NULL` wartości. `DisplayCategoryPicture.aspx` Strony, jednak zakłada, ma wartość inną niż`NULL` wartości. `Picture` Właściwość `CategoriesDataTable` nie są bezpośrednio dostępne, jeśli ma on `NULL` wartość. Jeśli chcesz zezwolić na `NULL` wartości `Picture` kolumny, d mają zostać uwzględnione następujący warunek:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

Powyższy kod przyjmuje s, ponieważ niektóre obrazu pliku o nazwie `NoPictureAvailable.gif` w `Images` folder, który chcesz wyświetlić kategorie bez obrazu.

Również być spowodowane ten wyjątek, jeśli `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metoda s `SELECT` instrukcji przywrócono listy kolumn główne zapytanie s może nastąpić, jeśli używasz instrukcji SQL zapytań ad-hoc i był ponownie uruchom Kreator TableAdapter s główne zapytanie. Zaznacz, aby upewnić się, że `GetCategoryWithBinaryDataByCategoryID` metoda s `SELECT` instrukcja nadal zawiera `Picture` kolumny.

> [!NOTE]
> Za każdym razem, gdy `DisplayCategoryPicture.aspx` jest odwiedzoną, baza danych jest dostępny i określonej kategorii s dane obrazu są zwracane. Jeśli t nie zostały obraz s kategorii zmieniona, ponieważ użytkownik ostatni zawiera przegląd, jednak to nieużywanego nakładu pracy. Na szczęście usługa HTTP umożliwia *warunkowego pobiera*. Za pomocą warunkowego GET, wysyła klienta wysyłającego żądanie HTTP [ `If-Modified-Since` nagłówka HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) zawierający datę i godzinę ostatniego pobrania tego zasobu z serwera sieci web w kliencie. Jeśli zawartość nie została zmieniona, ponieważ ta określona data, serwer sieci web mogą odpowiadać za pomocą [niezmodyfikowane kod stanu (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) i zrezygnujesz z powrotem przesyłania zawartości żądany zasób s. Krótko mówiąc ta metoda zwalnia serwer sieci web z konieczności wysyłania zawartości dla zasobu, jeśli go nie został zmodyfikowany od czasu ostatni dostęp klienta.


Aby zaimplementować to zachowanie, wymaga jednak sytuacja: dodajesz `PictureLastModified` kolumny `Categories` tabeli, aby przechwycić, gdy `Picture` kolumny był ostatnio aktualizowany, a także kod pod kątem `If-Modified-Since` nagłówka. Aby uzyskać więcej informacji na temat `If-Modified-Since` nagłówka i warunkowego przepływ pracy GET, zobacz [HTTP warunkowego GET dla hakerów RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) i [bardziej Przyjrzyj się wykonywanie żądań HTTP na stronie ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Krok 4: Wyświetlanie obrazów kategorii w widoku GridView

Teraz, gdy mamy już strony sieci web, aby wyświetlić obraz określoną kategorię s, możemy wyświetlić za pomocą [kontrolka obrazu w sieci Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) lub HTML `<img>` wskazuje element `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrazy, którego adres URL jest określany przez dane z bazy danych mogą być wyświetlane w widoku GridView lub DetailsView przy użyciu ImageField. Zawiera ImageField `DataImageUrlField` i `DataImageUrlFormatString` właściwości, które działają podobnie s pole hiperłącza HyperLinkField `DataNavigateUrlFields` i `DataNavigateUrlFormatString` właściwości.

Pozwól s rozszerzyć `Categories` GridView w `DisplayOrDownloadData.aspx` , dodając ImageField wyświetlania obrazu kategorii s. Po prostu Dodaj ImageField i ustaw jego `DataImageUrlField` i `DataImageUrlFormatString` właściwości `CategoryID` i `DisplayCategoryPicture.aspx?CategoryID={0}`, odpowiednio. Spowoduje to utworzenie kolumny kontrolki GridView, który renderuje `<img>` elementu którego `src` atrybutu odwołania `DisplayCategoryPicture.aspx?CategoryID={0}`, gdzie {0} zostaje zastąpiona opcją wiersza w widoku GridView s `CategoryID` wartość.


![Dodaj ImageField do widoku GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Rysunek 12**: Dodaj ImageField do widoku GridView


Po dodaniu ImageField, składni deklaratywnej s GridView powinien wyglądać soothe następujące:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Należy pamiętać o tym, jak każdy rekord zawiera teraz obraz dla kategorii.


[![Kategoria s obraz jest wyświetlany dla każdego wiersza](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Rysunek 13**: s kategorii obraz jest wyświetlany dla każdego wiersza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku zbadaliśmy sposoby przedstawiania danych binarnych. Sposób prezentowania danych zależy od typu danych. Broszura plików PDF, udostępniliśmy użytkownika broszura widoku łączem, po kliknięciu trwało użytkownika bezpośrednio do pliku PDF. Kategoria obrazu, s firma Microsoft pierwszego utworzenia strony, aby pobrać i zwrócić dane binarne z bazy danych i następnie używana tej stronie do wyświetlania obrazu s każdej kategorii w GridView.

Obecnie ten wykonujemy ve przyjrzano się sposób wyświetlania danych binarnych, możemy ponownie gotowe, aby sprawdzić, jak przeprowadzić wstawiania, aktualizacji i usuwania w bazie danych z danymi binarnymi. W następnym samouczku Zapoznamy się jak skojarzyć przekazanego pliku z jego odpowiedni rekord bazy danych. W tym samouczku później zobaczymy, jak zaktualizować istniejących danych binarnych, a także jak usunąć dane binarne, gdy jego skojarzony rekord zostanie usunięty.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Dave Gardner. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](uploading-files-cs.md)
> [dalej](including-a-file-upload-option-when-adding-a-new-record-cs.md)
