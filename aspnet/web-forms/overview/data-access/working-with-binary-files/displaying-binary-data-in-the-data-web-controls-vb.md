---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Wyświetlanie danych binarnych w sieci Web danych kontrolki (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W ramach tego samouczka przyjrzymy się opcji do prezentowania danych binarnych na stronie sieci Web, w tym wyświetlania pliku obrazu i dostarczanie łącze "Pobieranie" f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 006a4d014b610f3079d7f25e9420f687447a1b26
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Wyświetlanie danych binarnych w formantach sieci Web danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) lub [pobierania plików PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> W ramach tego samouczka przyjrzymy się opcji do prezentowania danych binarnych na stronie sieci Web, w tym wyświetlania pliku obrazu i zapewnienie łącze "Pobieranie" dla pliku PDF.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczek możemy przedstawione dwie metody kojarzenia danych binarnych z właściwego modelu danych aplikacji s i użyć formantu przekazywaniem plików do przekazywania plików z przeglądarką w systemie plików s serwera sieci web. Firma Microsoft kolejnych jeszcze, aby zobaczyć, jak skojarzyć przekazywane dane binarne z modelu danych. Oznacza to po ukończeniu przekazać plik i zapisać w systemie plików, ścieżka do pliku muszą być przechowywane w rekordzie odpowiednią bazę danych. Jeśli przechowywania danych bezpośrednio w bazie danych, następnie przekazywane dane binarne nie należy zapisać w systemie plików, ale muszą zostać dodane do bazy danych.

Zanim przyjrzymy kojarzenie danych z modelu danych, jednak umożliwiają s Pierwsze spojrzenie na jak zapewnić danych binarnych dla użytkownika końcowego. Prezentacja danych tekstowych jest prosta, ale jak dane binarne przedstawiane? Jest ono zależne, oczywiście od typu danych binarnych. W przypadku obrazów prawdopodobnie chcemy do wyświetlania obrazu. dla plików PDF dokumentów Microsoft Word, pliki ZIP i innymi typami danych binarnych, udostępnienie łącza pobierania jest prawdopodobnie bardziej odpowiednie.

W tym samouczku przedstawiono sposób przedstawia dane binarne obok tekstu skojarzone dane przy użyciu danych sieci Web kontrolki GridView i widoku DetailsView. W następnym samouczku firma Microsoft będzie Włącz wymagające uwagi do kojarzenia przekazany plik z bazą danych.

## <a name="step-1-providingbrochurepathvalues"></a>Krok 1: Zapewnianie`BrochurePath`wartości

`Picture` Kolumny w `Categories` tabela już zawiera dane binarne dla różnych kategorii obrazów. W szczególności `Picture` kolumny dla każdego rekordu znajduje się zawartość binarne mapy bitowej ziarnisty, niskiej jakości, 16 kolorów. Obraz każdej kategorii jest 172 pikseli szerokości i 120 pikseli wysokości i wykorzystuje około 11 KB. Jakie więcej s, zawartość binarną w `Picture` kolumna zawiera 78-bajtowych [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) nagłówek, który musi być usunięte przed wyświetlania obrazu. Te informacje nagłówka występuje, ponieważ dla bazy danych Northwind jest jego katalogów głównych programu Microsoft Access. W programie Access danych binarnych jest przechowywany przy użyciu typu danych obiektu OLE uwzględnia tego nagłówka. Obecnie firma Microsoft będzie Zobacz jak usuwanie nagłówków z tych obrazów w niskiej jakości, aby wyświetlić obraz. W samouczku przyszłych mamy utworzyć interfejsu do aktualizowania kategorii s `Picture` kolumny i Zastąp te obrazy mapy bitowej, używające nagłówki OLE z równoważną obrazów JPG bez niepotrzebnego nagłówków OLE.

W poprzednim samouczek widzieliśmy jak używać kontrolki przekazywaniem plików. W związku z tym można teraz i Dodaj broszura pliki do systemu plików s serwera sieci web. Dzięki temu, jednak nie aktualizuje `BrochurePath` kolumny w `Categories` tabeli. W następnym samouczku zajmiemy się tym, jak to zrobić, ale teraz musimy ręcznie Podaj wartości dla tej kolumny.

W tym samouczku s pobieranie znajdziesz siedmiu broszura pliki PDF `~/Brochures` folder, dla każdej kategorii, z wyjątkiem ryby. I celowo całkowicie pominięty, dodawanie broszurę ryby ilustrujący sposób obsługi scenariuszy, w którym nie wszystkie rekordy są skojarzone dane binarne. Aby zaktualizować `Categories` tabeli z tymi wartościami, kliknij prawym przyciskiem myszy `Categories` węzła z Eksploratora serwera i wybierz polecenie Pokaż dane tabeli. Następnie wprowadź ścieżek wirtualnych w plikach broszura dla każdej kategorii, której broszurę, jak przedstawiono na rysunku 1. Ponieważ nie ma żadnych innej kategorii Seafood, pozostaw jego `BrochurePath` wartość w kolumnie s jako `NULL`.


[![Ręcznie wprowadź wartości dla kolumny BrochurePath tabeli s kategorii](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Rysunek 1**: ręcznie wprowadzić wartości `Categories` tabeli s `BrochurePath` kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Krok 2: Gwarantuje to łącze pobierania broszury w widoku GridView

Z `BrochurePath` podane wartości `Categories` tabeli, możemy re gotowe, aby utworzyć element GridView z listą każdej kategorii, wraz z linkiem do pobrania broszura kategorii s. W kroku 4 będziemy rozszerzać ten GridView można również wyświetlić obraz kategorii s.

Uruchom przeciągnąć element GridView z przybornika do projektanta `DisplayOrDownloadData.aspx` strony `BinaryData` folderu. Ustaw GridView s `ID` do `Categories` i za pomocą tagów inteligentnych s GridView chcesz powiązać go z nowego źródła danych. W szczególności powiązać go z ObjectDataSource o nazwie `CategoriesDataSource` , który pobiera danych przy użyciu `CategoriesBLL` obiektu s `GetCategories()` metody.


[![Utwórz nowy element ObjectDataSource o nazwie CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Rysunek 2**: Utwórz nowy składnik o nazwie ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Skonfiguruj ObjectDataSource do użycia klasy CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Rysunek 3**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Pobieranie listy kategorii przy użyciu metody GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Rysunek 4**: pobrać listy z kategorii za pomocą `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio automatycznie doda do elementu BoundField `Categories` GridView dla `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, i `BrochurePath` `DataColumn` s. Przejdź dalej i Usuń `NumberOfProducts` elementu BoundField od `GetCategories()` zapytania metody s nie pobrać tych informacji. Również usunięcie `CategoryID` elementu BoundField i zmiana nazwy `CategoryName` i `BrochurePath` BoundFields `HeaderText` właściwości do kategorii i Brochure, odpowiednio. Po wprowadzeniu tych zmian, z widoku GridView i ObjectDataSource s deklaratywne znaczników powinna wyglądać następująco:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Wyświetlić tę stronę za pośrednictwem przeglądarki (patrz rysunek 5). Każda z tych kategorii osiem jest wyświetlana. Siedem kategorii z `BrochurePath` wartości mają `BrochurePath` wartości wyświetlane w odpowiednich elementu BoundField. Seafood, który ma `NULL` wartość jego `BrochurePath`, wyświetla pustej komórki.


[![Każda kategoria s nazwa, opis i wartość BrochurePath ma na liście](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Rysunek 5**: s każda kategoria Nazwa, opis, a `BrochurePath` znajduje się wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Zamiast tekstu `BrochurePath` kolumny, jeśli chcesz utworzyć łącze do broszury. Aby to zrobić, należy usunąć `BrochurePath` elementu BoundField i Zastąp pole hiperłącza HyperLinkField. Ustaw nowe s pole hiperłącza HyperLinkField `HeaderText` właściwości Brochure, jego `Text` właściwości broszura widoku i jego `DataNavigateUrlFields` właściwości `BrochurePath`.


![Dodaj pole hiperłącza HyperLinkField BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Rysunek 6**: Dodaj pole hiperłącza HyperLinkField dla `BrochurePath`


Spowoduje to dodanie kolumny łącza do widoku GridView, jak pokazano na rysunku 7. Klikając łącze Wyświetl broszura zostanie wyświetlony plik PDF bezpośrednio w przeglądarce, albo Monituj użytkownika, aby pobrać plik, w zależności od tego, czy zainstalowano czytnik plików PDF oraz ustawienia przeglądarki s.


[![Broszurę s kategorii można wyświetlić, klikając łącze Wyświetl broszura](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Rysunek 7**: s A kategorii broszura można wyświetlić, klikając łącze Wyświetl broszura ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Kategoria s broszura PDF jest wyświetlana](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Rysunek 8**: s kategorii broszura PDF jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ukrywanie tekstu broszura widoku dla kategorii bez broszurę

Jak pokazano na rysunku 7, `BrochurePath` Wyświetla pole hiperłącza HyperLinkField jego `Text` wartość właściwości (Widok broszura) dla wszystkich rekordów, niezależnie od tego, czy istnieje s niż`NULL` wartość `BrochurePath`. Oczywiście jeśli `BrochurePath` jest `NULL`, link jest wyświetlana jako tekst, a następnie tak jak w przypadku z kategorią ryby (odwołują się do na rysunku nr 7). Zamiast wyświetlanie tekstu broszura widoku, może być uwzględniona te kategorie bez `BrochurePath` wartość wyświetlana niektórych tekst alternatywny, takich jak nr broszura dostępne.

W celu zapewnienia tego zachowania, należy użyć TemplateField, których zawartość jest generowana przez wywołanie do metody strony, który emituje odpowiednie dane wyjściowe na podstawie `BrochurePath` wartości. Firma Microsoft najpierw przedstawione to technika formatowania w [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) samouczka.

Włącz pole hiperłącza HyperLinkField na pole TemplateField, wybierając `BrochurePath` pole hiperłącza HyperLinkField i klikając Konwertuj to pole na pole TemplateField łącze w oknie dialogowym Edytowanie kolumn.


![Konwertuj pole hiperłącza HyperLinkField na pole TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Rysunek 9**: Konwertuj pole hiperłącza HyperLinkField na pole TemplateField


Spowoduje to utworzenie TemplateField z `ItemTemplate` zawierający HyperLink Web kontroli, których `NavigateUrl` właściwość jest powiązana z `BrochurePath` wartości. Zamień ten kod znaczników wywołanie do metody `GenerateBrochureLink`, przekazując wartość `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Następnie należy utworzyć `Protected` metody w ASP.NET strony s związane z kodem klasę o nazwie `GenerateBrochureLink` zwracającą `String` i akceptuje `Object` jako parametr wejściowy.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Ta metoda określa, czy przekazywany do `Object` wartość jest bazą danych `NULL` i jeśli tak, zwraca komunikat wskazujący, że kategoria brakuje broszurę. W przeciwnym razie, jeśli istnieje `BrochurePath` wartość on s wyświetlane w hiperłączu. Należy pamiętać, że jeśli `BrochurePath` wartość jest prezentować przekazanych do [ `ResolveUrl(url)` metody](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Ta metoda usuwa przekazany do *adres url*, zastępując `~` znak z odpowiednią ścieżką wirtualną. Na przykład, jeśli aplikacja w `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` zwróci `/Tutorial55/Brochures/Meat.pdf`.

Rysunek nr 10 przedstawia strony, po zastosowaniu tych zmian. Należy pamiętać, że kategoria ryby s `BrochurePath` tekst nie broszura dostępne są obecnie wyświetlane pola.


[![Dostępne broszura nr tekst jest wyświetlany dla tych kategorii bez broszura](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Na rysunku nr 10**: tekst nie broszura dostępne są wyświetlane dla tych kategorii bez broszura ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Krok 3: Dodawanie strony sieci Web, aby wyświetlić obraz kategorii s

Gdy użytkownik odwiedza strony platformy ASP.NET, otrzymają ASP.NET strony HTML. Odebrano kod HTML jest tylko tekst i nie zawiera żadnych danych binarnych. Dodatkowe dane binarne, takich jak obrazy, pliki dźwiękowe, aplikacje Macromedia Flash, osadzonego wideo Windows Media Player i tak dalej, istnieje jako oddzielne zasoby na serwerze sieci web. Kod HTML zawiera odwołania do tych plików, ale nie ma rzeczywistej zawartości plików.

Na przykład w języku HTML `<img>` element jest używany w celu obrazu z `src` atrybut wskazuje plik obrazu w następujący sposób:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Gdy przeglądarką odbiera kod HTML, ułatwia innego żądania do serwera sieci web, aby pobrać binarne zawartość pliku obrazu, który następnie wyświetla w przeglądarce. Wszystkie dane binarne dotyczy tego samego pojęcia. W kroku 2 broszury nie została wysłana w dół w przeglądarce jako część kod znaczników HTML strony s. Zamiast HTML renderowanych ile hiperłącza, po kliknięciu spowodował przeglądarkę, aby bezpośrednio dokumentu PDF.

Aby wyświetlić lub Zezwalaj użytkownikom na pobieranie danych binarnych, które znajdują się w bazie danych, należy utworzyć oddzielne strony sieci web, która zwraca dane. Dla aplikacji, że s tylko jedno pole dane binarne przechowywane bezpośrednio w obraz s kategorii bazy danych. W związku z tym należy strona, która po wywołaniu zwraca dane obrazu dla określonej kategorii.

Dodaj nową stronę ASP.NET do `BinaryData` folder o nazwie `DisplayCategoryPicture.aspx`. Po tej czynności nie zaznaczaj pola wyboru Wybierz strony wzorcowej wyboru. Ta strona oczekuje `CategoryID` wartości querystring i zwraca dane binarne tej kategorii s `Picture` kolumny. Ponieważ ta strona zwraca dane binarne i nic, nie wymaga żadnych znaczników w sekcji HTML. W związku z tym kliknij na karcie Źródło w lewym dolnym rogu i usunięcie wszystkich znaczników strony s, z wyjątkiem `<%@ Page %>` dyrektywy. Oznacza to, że `DisplayCategoryPicture.aspx` znaczników deklaratywne s powinien zawierać pojedynczy wiersz:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Jeśli widzisz `MasterPageFile` atrybutu w `<%@ Page %>` dyrektywy, usuń go.

W klasie związanej z kodem strony s, Dodaj następujący kod do `Page_Load` obsługi zdarzeń:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Ten kod rozpoczyna się od odczytu w `CategoryID` wartości querystring w zmiennej o nazwie `categoryID`. Następnie dane obrazu są pobierane za pośrednictwem wywołania `CategoriesBLL` klasy s `GetCategoryWithBinaryDataByCategoryID(categoryID)` metody. Te dane, jest zwracana do klienta przy użyciu `Response.BinaryWrite(data)` metody, ale przed jest to nazywane `Picture` nagłówka OLE wartość s kolumny muszą zostać usunięte. Jest to osiągane przez utworzenie `Byte` tablicy o nazwie `strippedImageData` który wstrzymuje dokładnie 78 znaków mniejszej niż podana w `Picture` kolumny. [ `Array.Copy` Metody](https://msdn.microsoft.com/library/z50k9bft.aspx) służy do kopiowania danych z `category.Picture` zaczynając od pozycji 78 za pośrednictwem do `strippedImageData`.

`Response.ContentType` Właściwość określa [typ MIME](http://en.wikipedia.org/wiki/MIME) zwracanych tak, aby przeglądarka potrafi do renderowania jej zawartości. Ponieważ `Categories` tabeli s `Picture` kolumna jest obraz mapy bitowej, mapy bitowej typ MIME jest używany tutaj (obraz/bmp). W przypadku pominięcia typ MIME, w większości przeglądarek nadal wyświetli obrazu poprawnie, ponieważ ich można wnioskować o typie, na podstawie zawartości danych binarnych s pliku obrazu. Jednak go s, aby uwzględnić MIME typu, jeśli to możliwe. Zobacz [witryny sieci Web Internet Assigned Numbers Authority s](http://www.iana.org/) Pełna lista [typów nośników MIME](http://www.iana.org/assignments/media-types/).

Z tą stroną utworzony obraz określonej kategorii s można wyświetlić, odwiedzając `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Rysunek 11 zawiera obraz kategorii s napoje, które mogą być wyświetlane z `DisplayCategoryPicture.aspx?CategoryID=1`.


[![S należące do kategorii, wyświetlania obrazu](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Rysunek 11**: s należące do kategorii jest wyświetlany obraz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


W przypadku, gdy użytkownik odwiedzi `DisplayCategoryPicture.aspx?CategoryID=categoryID`dwie rzeczy, które mogą być przyczyną tego, otrzymasz wyjątek odczytujący można rzutować obiektu typu stałej "System.DBNull" na typ "[System.Byte]". Najpierw `Categories` tabeli s `Picture` kolumna zezwala `NULL` wartości. `DisplayCategoryPicture.aspx` Strony, jednak przyjęto założenie, ma wartość inną niż`NULL` wartość istnieje. `Picture` Właściwość `CategoriesDataTable` nie są bezpośrednio dostępne, jeśli ma ona `NULL` wartość. Jeśli chcesz umożliwić `NULL` wartości `Picture` kolumny, d chcesz dołączyć następujący warunek:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Powyższy kod przyjęto założenie, ponieważ s niektóre obrazu pliku o nazwie `NoPictureAvailable.gif` w `Images` folderu, który chcesz wyświetlić tych kategorii bez obrazu.

Ten wyjątek może być również spowodowane Jeśli `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metody s `SELECT` instrukcji przywrócono listy kolumn główne zapytanie s może nastąpić, jeśli używasz ad hoc instrukcji SQL i był ponownie uruchom Kreatora s TableAdapter główne zapytanie. Sprawdź, upewnij się, że `GetCategoryWithBinaryDataByCategoryID` metody s `SELECT` instrukcja nadal zawiera `Picture` kolumny.

> [!NOTE]
> Za każdym razem, gdy `DisplayCategoryPicture.aspx` jest odwiedzoną, bazy danych jest dostępne i określonej kategorii s dane obrazu są zwracane. Jeśli t nie zostały obraz s kategorii zmieniona, ponieważ użytkownik ma ostatnio go, jednak jest nieużywanego wysiłku. Na szczęście usługa HTTP umożliwia *pobiera warunkowego*. Warunkowe operacje GET, tworzenia żądania HTTP klienta wysyła wzdłuż [ `If-Modified-Since` nagłówka HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) zapewnia datę i godzinę ostatniego klienta pobrania tego zasobu na serwerze sieci web. Jeśli zawartość nie została zmieniona, ponieważ to określona data, serwer sieci web może odpowiadać, podając [niezmodyfikowany kod stanu (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) i zrezygnujesz z odsyła zawartości żądanego zasobu s. Krótko mówiąc ta metoda zwalnia z konieczności wysyłania zawartości dla zasobu, jeśli nie został on zmodyfikowany od czasu ostatniego dostęp klienta serwera sieci web.


Aby zaimplementować to zachowanie, jednak wymaga dodania `PictureLastModified` kolumny, która ma `Categories` tabeli do przechwycenia, kiedy `Picture` kolumny ostatniej aktualizacji oraz kod, aby sprawdzić `If-Modified-Since` nagłówka. Aby uzyskać więcej informacji na temat `If-Modified-Since` nagłówka i warunkowego GET przepływu pracy, zobacz [GET protokołu HTTP warunkowego hakerom RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) i [A głębiej przyjrzeć się wykonywania żądań HTTP na stronie ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Krok 4: Wyświetlanie obrazów kategorii w widoku GridView

Teraz, gdy mamy strony sieci web, aby wyświetlić obraz określonej kategorii s można wyświetlić za pomocą [kontrolka sieci Web obrazu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) lub HTML `<img>` element wskazujący `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Obrazy, którego adres URL jest określany przez dane z bazy danych mogą być wyświetlane w widoku GridView lub przy użyciu elementu ImageField widoku DetailsView. Element ImageField zawiera `DataImageUrlField` i `DataImageUrlFormatString` właściwości, które działają podobnie s pole hiperłącza HyperLinkField `DataNavigateUrlFields` i `DataNavigateUrlFormatString` właściwości.

Let s rozszerzyć `Categories` GridView w `DisplayOrDownloadData.aspx` przez dodanie elementu ImageField wyświetlania obrazu kategorii s. Po prostu Dodaj element ImageField i ustaw jej `DataImageUrlField` i `DataImageUrlFormatString` właściwości `CategoryID` i `DisplayCategoryPicture.aspx?CategoryID={0}`odpowiednio. Spowoduje to utworzenie kolumny widoku GridView, który renderuje `<img>` element których `src` atrybut odwołania `DisplayCategoryPicture.aspx?CategoryID={0}`, gdzie {0} zostanie zastąpiony wiersza elementu GridView s `CategoryID` wartość.


![Dodaj element ImageField do widoku GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Rysunek 12**: Dodaj element ImageField do widoku GridView


Po dodaniu elementu ImageField, GridView składni deklaratywnej s powinna wyglądać tak jak soothe następujące:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Należy zwrócić uwagę, jak każdy rekord zawiera teraz obrazu dla kategorii.


[![Kategoria s obrazu jest wyświetlana dla każdego wiersza.](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Rysunek 13**: s kategorii obrazu jest wyświetlana dla każdego wiersza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy zbadać jak do prezentowania danych binarnych. Sposób prezentowania danych zależy od typu danych. Broszura plików PDF, firma Microsoft oferowanych użytkownika broszurę widoku łączem, po kliknięciu trwało użytkownika bezpośrednio do pliku PDF. Obrazu s kategorii firma Microsoft utworzenia strony, aby pobrać i zwracać danych binarnych z bazy danych, a następnie używany do wyświetlania każdego obrazu s kategorii w widoku GridView tej strony.

Teraz tego możemy kolejnych przeglądał sposób wyświetlania danych binarnych, możemy re gotowe, aby sprawdzić, jak wykonać wstawiania, aktualizacji i usunięć w bazie danych z danych binarnych. W następnym samouczku wyjaśniono, jak skojarzyć przekazanego pliku z jego odpowiedniego rekordu bazy danych. W samouczku po tym firma Microsoft będzie widoczny jak zaktualizować istniejące dane binarne, jak również sposób usuwania danych binarnych, po usunięciu jego skojarzony rekord.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Teresa Murphy i Dave Gardner. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](uploading-files-vb.md)
> [dalej](including-a-file-upload-option-when-adding-a-new-record-vb.md)
