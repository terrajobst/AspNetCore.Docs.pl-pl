---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Omówienie edytowanie i usuwanie danych DataList (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Gdy elementu DataList nie ma wbudowanej edytowanie i usuwanie funkcji, w tym samouczku zajmiemy się tym, jak utworzyć DataList, który obsługuje edytowanie i usuwanie o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: be86707980b11453ef78fdbddead73ab9808b54d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888997"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Omówienie edytowanie i usuwanie danych DataList (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) lub [pobierania plików PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Podczas elementu DataList nie ma wbudowanej edytowanie i usuwanie funkcji, w tym samouczku będzie przedstawiono sposób tworzenia DataList, obsługujący edytowanie i usuwanie z jej odpowiednie dane.


## <a name="introduction"></a>Wprowadzenie

W [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczek analizujemy jak wstawianie, aktualizowanie i usuwanie danych przy użyciu architektury aplikacji, ObjectDataSource i GridView, widoku DetailsView i FormView formanty. ObjectDataSource i formantów sieci Web tych trzech danych implementowanie interfejsów modyfikacji proste danych było bardzo proste i zaangażowany jedynie zaznaczenie pola wyboru z tagów inteligentnych. Brak kodu niezbędne do zapisania.

Niestety DataList nie ma wbudowanej, edytowanie i usuwanie możliwości dostępu do właściwych w kontrolce GridView. Ta funkcja brak wynika w części fakt, że elementu DataList jest relic z poprzedniej wersji programu ASP.NET, gdy dane deklaratywne kontrolki źródła i stron modyfikacji kodu dane były niedostępne. Gdy DataList w programie ASP.NET 2.0 nie oferuje takie same, bez możliwości zmiany danych jako widoku GridView, możemy użyć techniki 1.x ASP.NET uwzględnienie tych funkcji. Ta metoda wymaga fragmentem kodu, ale zajmiemy się tym, w tym samouczku, DataList ma niektórych zdarzeń i właściwości w celu pomocy w tym procesie.

W tym samouczku będzie przedstawiono sposób tworzenia DataList, obsługujący edytowanie i usuwanie z jej odpowiednie dane. Samouczki przyszłych zbada bardziej zaawansowane edytowanie i usuwanie scenariuszy, włącznie z weryfikacji pola wejściowego, bezpiecznie obsługi wyjątków zgłoszonych z dostępu do danych i warstwy logiki biznesowej i tak dalej.

> [!NOTE]
> Jak DataList kontrolce elementu powtarzanego brakuje poza funkcjonalność pola do wstawiania, aktualizowania lub usuwania. Podczas dodawania takich funkcji, DataList zawiera właściwości i zdarzeń nie znaleziono w elemencie powtarzanym, które ułatwiają dodawanie takich funkcji. W związku z tym w tym samouczku, a następne, które wyglądają na edytowanie i usuwanie dotyczą wyłącznie elementu DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Krok 1: Tworzenie stron sieci Web samouczki edytowania i usuwania

Zanim Rozpoczniemy eksploracji jak aktualizować i usuwać dane z DataList, chętnie s najpierw Poświęć chwilę, aby utworzyć w naszym projekt witryny sieci Web, które będą potrzebne dla tego samouczka i dalej widocznych kilka stron ASP.NET. Rozpocznij od dodania nowy folder o nazwie `EditDeleteDataList`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Dodawanie stron ASP.NET dla samouczków](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Rysunek 1**: Dodawanie stron ASP.NET dla samouczków


Podobnie jak w innych folderach `Default.aspx` w `EditDeleteDataList` folder zawiera listę samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Ponadto dodanie stron jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po raporty wzorzec/szczegół z DataList i powtarzanego `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie teraz obejmuje elementy DataList, edytowanie i usuwanie samouczki.


![Obecnie mapy witryny zawiera wpisy dla DataList, edytowanie i usuwanie samouczki](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Rysunek 3**: mapy witryny teraz zawiera wpisy dla DataList, edytowanie i usuwanie samouczki


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Krok 2: Badanie techniki, aktualizowanie i usuwanie danych

Edytowanie i usuwanie danych z widoku GridView jest tak proste, ponieważ poniżej obejmuje GridView i ObjectDataSource działają w połączeniu. Zgodnie z opisem w [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) samouczek, po kliknięciu przycisku Aktualizuj wiersza s, widoku GridView automatycznie przypisuje jej pola używane powiązany dwukierunkowo z danymi`UpdateParameters` kolekcji jego ObjectDataSource, a następnie wywołuje ten element ObjectDataSource s `Update()` metody.

Sadly DataList nie ma żadnych z tej funkcji wbudowanych. Nasze odpowiedzialność za upewnij się, że wartości użytkownika s są przypisane ObjectDataSource parametry s, a jego `Update()` metoda jest wywoływana. Aby pomóc nam w tym pozwala, DataList zawiera poniższe właściwości i zdarzenia:

- **[ `DataKeyField` Właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  podczas aktualizowania lub usuwania, musimy można jednoznacznie zidentyfikować każdego elementu w elementu DataList. Ustaw tą właściwość na klucz podstawowy wyświetlanych danych. To spowoduje wypełnienie DataList s [ `DataKeys` kolekcji](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) z określonym `DataKeyField` wartość dla każdego elementu DataList.
- **[ `EditCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  generowane, gdy przycisk, LinkButton lub ImageButton których `CommandName` właściwości ustawiono edycji zostanie kliknięty.
- **[ `CancelCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  generowane, gdy przycisk, LinkButton lub ImageButton których `CommandName` właściwości ustawiono Anuluj zostanie kliknięty.
- **[ `UpdateCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  generowane, gdy przycisk, LinkButton lub ImageButton których `CommandName` właściwości ustawiono aktualizacji zostanie kliknięty.
- **[ `DeleteCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  generowane, gdy przycisk, LinkButton lub ImageButton których `CommandName` właściwości ustawiono Delete zostanie kliknięty.

Przy użyciu tych właściwości i zdarzenia, istnieją cztery metody, których firma Microsoft może używać do aktualizować i usuwać dane z elementu DataList:

1. **Przy użyciu technik 1.x ASP.NET** elementu DataList sprzed ASP.NET 2.0 i ObjectDataSources i nie może zaktualizować i usuwania danych wyłącznie w sposób programowy. Ta technika całkowicie rowów ObjectDataSource i wymaga, aby firma Microsoft powiązać do elementu DataList dane bezpośrednio z warstwy logiki biznesowej, zarówno podczas pobierania danych do wyświetlenia i w przypadku aktualizowania lub usuwania rekordu.
2. **Za pomocą pojedynczego kontrolki ObjectDataSource na stronie dla wybranie opcji, aktualizowania i usuwania** podczas elementu DataList nie ma w widoku GridView s związanego z używaniem edytowania i usuwania możliwości, s nie ma powodu możemy t dodane w nad. Z tej metody został Użyj ObjectDataSource tak jak w przykładach GridView, ale należy utworzyć program obsługi zdarzeń dla elementu DataList s `UpdateCommand` zdarzeń, w którym możemy ustawić parametry s ObjectDataSource i wywołanie jego `Update()` metody.
3. **Wybranie opcji, ale aktualizowanie i usuwanie bezpośrednio przed logiki warstwy Biznesowej za pomocą kontrolki ObjectDataSource** podczas używania opcji 2, musimy zapisu z bitowego kodu w `UpdateCommand` zdarzeń, przypisywanie wartości parametrów i tak dalej. Firma Microsoft zamiast tego przestrzegaj przy użyciu elementu ObjectDataSource wyboru, ale wykonywanie wywołań aktualizowania i usuwania bezpośrednio przed logiki warstwy Biznesowej (np. z opcja 1). W mojej opinii, aktualizowanie danych przez sprzężenie bezpośrednio z logiki warstwy Biznesowej prowadzi do bardziej czytelny kodu niż przypisywanie ObjectDataSource s `UpdateParameters` i wywoływanie jej `Update()` metody.
4. **Przy użyciu środków deklaratywne za pomocą wielu ObjectDataSources** trzech poprzednich podejść wszystkie wymagają fragmentem kodu. Jeśli możesz d raczej nadal używać jako znacznie deklaratywne składni, jak to możliwe, Ostatnia opcja polega na obejmują wiele ObjectDataSources na stronie. Pierwszy element ObjectDataSource pobiera dane z logiki warstwy Biznesowej i wiąże go z elementu DataList. Do aktualizacji, inny element ObjectDataSource jest dodany, ale dodany bezpośrednio wewnątrz elementu DataList s `EditItemTemplate`. Dołączenie, usunięcie pomocy technicznej, jeszcze inny element ObjectDataSource byłaby potrzebna w `ItemTemplate`. Z tej metody są osadzone ObjectDataSource s Użyj `ControlParameters` deklaratywnie powiązać parametry s ObjectDataSource kontrolki wejściowe użytkownika (zamiast je określić programowo w DataList s `UpdateCommand` program obsługi zdarzeń). Takie podejście nadal wymaga nieco kodu, należy wywołać osadzony s ObjectDataSource `Update()` lub `Delete()` polecenia, ponieważ wymaga znacznie mniej niż z innym trzy zbliża się. Wadą interfejsu w tym miejscu jest, że wiele ObjectDataSources zajmowały stronę, szkody dla czytelności ogólnej.

Wymuszenie tylko użyj jednej z tych metod, I d wybierz opcję 1, ponieważ zapewnia ona największą elastyczność i elementu DataList pierwotnie została zaprojektowana w celu uwzględnienia tego wzorca. Gdy elementu DataList został rozszerzony do pracy z kontrolki źródła danych programu ASP.NET 2.0, nie ma wszystkich punktów rozszerzalności lub funkcje oficjalnego danych ASP.NET 2.0 formantów sieci Web (GridView, widoku DetailsView i FormView). Opcje 2 do 4 nie są bez wartości, jednak.

To przyszłych edytowanie i usuwanie samouczki będzie używać ObjectDataSource pobierania danych do wyświetlania i bezpośrednie wywołania logiki warstwy Biznesowej do aktualizowania i usuwania danych (opcja 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Krok 3: Dodawanie elementu DataList i konfigurowania jej ObjectDataSource

W tym samouczku utworzymy DataList, wyświetla informacje o produktach, a następnie dla każdego produktu umożliwia użytkownika można edytować nazwę i ceny i aby całkowicie usunąć produkt. W szczególności firma Microsoft pobieranie rekordów do wyświetlenia przy użyciu ObjectDataSource, ale przeprowadzi aktualizację i akcje usuwania korzystając bezpośrednio z logiki warstwy Biznesowej. Zanim firma Microsoft martwić się o implementacji możliwości edytowania i usuwania do elementu DataList, chętnie s najpierw uzyskać strony, aby wyświetlić produktów w interfejsie tylko do odczytu. Od nas stawienia zbadać następujące kroki w poprzednim samouczki I będzie kontynuować za ich pośrednictwem szybko.

Uruchamianie przez otwarcie `Basics.aspx` strony `EditDeleteDataList` folderu i w widoku Projekt, należy dodać DataList do strony. Następnie należy utworzyć nowy element ObjectDataSource z tagów inteligentnych s DataList. Ponieważ pracujemy z danymi produktu, należy skonfigurować go do używania `ProductsBLL` klasy. Można pobrać *wszystkie* wybierz produkty, `GetProducts()` Metoda wybierz kartę.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Zwraca informacje produktu przy użyciu metody GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Rysunek 5**: zwraca informacje o produkt za pomocą `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList, takich jak GridView, nie jest przeznaczony do wstawiania nowych danych; w związku z tym, wybierz opcję (Brak) z listy rozwijanej na karcie Wstawianie. Też (Brak) dla kart UPDATE i DELETE, ponieważ aktualizacji i usunięć będzie można wykonać programowo przy użyciu logiki warstwy Biznesowej.


[![Upewnij się, że listy rozwijane w elemencie ObjectDataSource s INSERT, UPDATE i usuwanie kart są ustawione na (Brak)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Rysunek 6**: Upewnij się, że listy rozwijane w s WSTAWIANIA, aktualizacji i usunąć karty ObjectDataSource są ustawione na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


Po skonfigurowaniu ObjectDataSource, kliknij przycisk Zakończ, wracając do projektanta. Jak możemy kolejnych widoczna w poprzednich przykładach, po zakończeniu konfiguracji ObjectDataSource, Visual Studio automatycznie tworzy `ItemTemplate` DropDownList, wyświetlania każdego pola danych. Zastąp to `ItemTemplate` z jednym, zawierający tylko nazwę produktu s i cenę. Ponadto należy ustawić `RepeatColumns` właściwości do 2.

> [!NOTE]
> Zgodnie z opisem w *omówienie Wstawianie, aktualizowanie i usuwanie danych* samouczek, modyfikując danych przy użyciu ObjectDataSource Nasza architektura wymaga możemy usunąć `OldValuesParameterFormatString` właściwości z ObjectDataSource s deklaracyjne znaczników (lub zresetować go do wartości domyślnej `{0}`). W tym samouczku jednak użyto ObjectDataSource tylko do pobierania danych. W związku z tym nie należy modyfikować ObjectDataSource s `OldValuesParameterFormatString` wartość właściwości (mimo że jego pogarszać t, aby to zrobić).


Po wymianie domyślne DataList `ItemTemplate` z jednego dostosowane, deklaratywne znaczników na stronie powinien wyglądać podobnie do następujących:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Poświęć chwilę, aby wyświetlić postęp naszych za pośrednictwem przeglądarki. Jak pokazano na rysunku 7, DataList wyświetlana cena nazwę i jednostkę produktu dla każdego produktu w dwóch kolumnach.


[![Nazwy produktów i ceny są wyświetlane w DataList dwie kolumny](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Rysunek 7**: nazwy produktów i ceny są wyświetlane w DataList dwie kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> Elementu DataList ma wiele właściwości, które są wymagane dla procesu aktualizowania i usuwania, a te wartości są przechowywane w widoku stanu. W związku z tym podczas kompilowania DataList, który obsługuje edytowania lub usuwania danych, jest ważne, czy stan widoku s DataList można włączyć.  
>   
>  Czytnik astute może odwołać, czy udało się wyłączyć stan widoku, podczas tworzenia, edycji GridViews DetailsViews i FormViews. Jest to spowodowane mogą zawierać formantów sieci Web ASP.NET 2.0 *kontrolować stan*, która jest stan zachowywany po ogłaszania zwrotnego, takie jak stan widoku, ale zakładany istotne.


Wyłączanie widoku stanu w widoku GridView jedynie pomija informacji o stanie trivial, ale zachowuje stan formantu (w tym stanie niezbędne do edytowania i usuwania). DataList, jakby został utworzony w przedziale czasu 1.x ASP.NET, nie korzysta z kontrolki stanu i dlatego musi być włączony stan widoku. Zobacz [stanu kontroli wersji programu vs. Stan widoku](https://msdn.microsoft.com/library/1whwt1k7.aspx) uzyskać więcej informacji dotyczących przeznaczenia stan kontrolki i sposób różni się od stanu widoku.

## <a name="step-4-adding-an-editing-user-interface"></a>Krok 4: Dodawanie edycji interfejsu użytkownika

Kontrolki widoku siatki składa się z kolekcji pól (BoundFields, CheckBoxFields TemplateFields i tak dalej). Te pola można dostosować ich renderowanego kodu znaczników w zależności od ich trybu. Na przykład w trybie tylko do odczytu elementu BoundField wyświetla jego wartości pola danych jako tekst. w trybie edycji renderowania Web pole tekstowe kontroli, których `Text` właściwości jest przypisywana wartość pola danych.

DataList renderuje z drugiej strony, jego elementów za pomocą szablonów. Elementy tylko do odczytu są renderowane przy użyciu `ItemTemplate` elementów w trybie edycji są uznane za pośrednictwem `EditItemTemplate`. W tym momencie nasze DataList ma tylko `ItemTemplate`. Do obsługi funkcji edytowania poziomie elementu, należy dodać `EditItemTemplate` zawierający kod znaczników, który będzie wyświetlany dla edytowalnego elementu. W tym samouczku użyjemy kontrolki TextBox w sieci Web do edycji cena nazwę i jednostkę produktu s.

`EditItemTemplate` Mogą być tworzone deklaratywnie lub poprzez projektanta (przez wybranie opcji Edytuj szablony z tagów inteligentnych DataList s). Aby użyć opcji Edytuj szablony, najpierw kliknij łącze Edytuj szablony w tagu inteligentnego, a następnie wybierz `EditItemTemplate` elementu z listy rozwijanej.


[![OPT pracować z DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Rysunek 8**: zdecydować się na pracę z DataList s `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Następnie wpisz w polu Nazwa produktu: i cen:, a następnie przeciągnij z przybornika do dwóch pól tekstowych `EditItemTemplate` interfejsu w projektancie. Ustawianie pól tekstowych `ID` właściwości `ProductName` i `UnitPrice`.


[![Dodaj pole tekstowe Nazwa s produktu i cen](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Rysunek 9**: Dodaj pole tekstowe s Nazwa produktu i cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


Należy powiązać odpowiedniego produktu pola wartości danych `Text` właściwości dwóch pól tekstowych. Z tagami inteligentnymi pól tekstowych, kliknij łącze Edytuj powiązania danych, a następnie skojarzyć pole odpowiednie dane o `Text` właściwości, jak pokazano na rysunku nr 10.

> [!NOTE]
> Podczas tworzenia wiązania `UnitPrice` pola danych do ceny s pole tekstowe `Text` pola, użytkownik może go sformatować jako wartość walutową (`{0:C}`), ogólne numer (`{0:N}`), lub pozostaw niesformatowany.


![Powiązania ProductName i pola danych UnitPrice właściwości tekstu, pól tekstowych](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Na rysunku nr 10**: powiązać `ProductName` i `UnitPrice` pól danych do `Text` właściwości pól tekstowych


Zwróć uwagę, jak okno dialogowe Edytuj powiązania DataBindings na rysunku nr 10 *nie* obejmują wyboru dwukierunkowego wiązania danych, która występuje podczas edytowania TemplateField w widoku GridView lub widoku DetailsView lub szablonu w widoku FormView. Funkcja dwukierunkowego wiązania z danymi dozwolona wartość wprowadzone do kontrolki wprowadzania sieci Web ma być automatycznie przypisany do odpowiedniego s ObjectDataSource `InsertParameters` lub `UpdateParameters` podczas wstawiania lub aktualizowania danych. Jak zajmiemy się później w tym samouczku po użytkownik dokona swoje zmiany i jest gotowy do aktualizacji danych elementu DataList nie obsługuje dwukierunkowego wiązania z danymi, potrzebujemy do uzyskania programowego dostępu do tych pól tekstowych `Text` właściwości i przekazywania ich wartości do odpowiednie `UpdateProduct` metoda `ProductsBLL` klasy.

Na koniec należy dodać aktualizacji i przyciski, aby anulować `EditItemTemplate`. Jak widzieliśmy w [wzorca/Detail, za pomocą listy punktowanej z rekordu głównego z DataList szczegóły](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) samouczek, gdy przycisk, LinkButton lub ImageButton którego `CommandName` ustawiono właściwość zostanie kliknięty z wewnątrz elementu powtarzanego lub DataList, S elementu powtarzanego lub DataList `ItemCommand` zdarzenia. Dla elementu DataList Jeśli `CommandName` właściwość jest ustawiona na określoną wartość, może zostać również zgłoszone zdarzenie dodatkowe. Specjalne `CommandName` wartości właściwości należą między innymi:

- Anuluj generuje `CancelCommand` zdarzeń
- Edytuj zgłasza `EditCommand` zdarzeń
- Zaktualizuj zgłasza `UpdateCommand` zdarzeń

Należy pamiętać, że te zdarzenia są generowane *oprócz* `ItemCommand` zdarzeń.

Dodaj do `EditItemTemplate` dwóch formantów sieci Web przycisk, którego `CommandName` ustawiono aktualizacji i innych s ustawioną Anuluj. Po dodaniu tych dwóch formantów sieci Web przycisk projektanta powinien wyglądać podobnie do poniższej:


[![Dodawanie aktualizacji i anulować przycisków EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Rysunek 11**: aktualizacji dodawać i przyciski Anuluj, aby `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


Z `EditItemTemplate` pełną Twojej znaczników deklaratywne DataList s powinien wyglądać podobnie do następujących:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Krok 5: Dodawanie żmudne procesy do trybu edycji

W tym momencie nasze DataList ma interfejs edycji zdefiniowany za pomocą jego `EditItemTemplate`, jednak istnieje s aktualnie żaden sposób dla użytkownika, odwiedzając stronę dotyczącą wskazująca, czy chce edytować informacje o produkcie s. Musimy dodać dostępny przycisk Edytuj do każdego produktu, że po kliknięciu renderuje DataList tego elementu w trybie edycji. Rozpocznij od dodania przycisk Edytuj, aby `ItemTemplate`, za pomocą projektanta lub deklaratywnie. Aby ustawić przycisk Edytuj s `CommandName` właściwości do edycji.

Po dodaniu tego przycisku Edytuj Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. Z tym dodatkiem każdego produktu lista powinna zawierać przycisk Edytuj.


[![Dodawanie aktualizacji i anulować przycisków EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Rysunek 12**: aktualizacji dodawać i przyciski Anuluj, aby `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Kliknięcie przycisku powoduje odświeżenie strony, ale *nie* Przełącz do trybu edycji produktu. Aby dokonać edycji produktu, musimy:

1. Ustaw DataList s [ `EditItemIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) na indeks `DataListItem` właśnie kliknięto przycisk Edytuj, którego.
2. Ponownie powiązać dane do elementu DataList. Gdy jest ponownie renderowanego elementu DataList `DataListItem` których `ItemIndex` odpowiada DataList s `EditItemIndex` spowoduje, że przy użyciu jego `EditItemTemplate`.

Ponieważ DataList s `EditCommand` zdarzenie jest wywoływane, gdy zostanie kliknięty przycisk Edytuj, Utwórz `EditCommand` obsługi zdarzeń z następującym kodem:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

`EditCommand` Program obsługi zdarzeń jest przekazano obiekt typu `DataListCommandEventArgs` jako drugiego parametru wejściowego, który zawiera odwołanie do `DataListItem` kliknięto przycisk Edytuj, którego (`e.Item`). Obsługi zdarzeń ustawia najpierw DataList s `EditItemIndex` do `ItemIndex` z można edytować `DataListItem` , a następnie rebinds danych do elementu DataList przez wywołanie elementu DataList s `DataBind()` metody.

Po dodaniu ten program obsługi zdarzeń, ponownie strony w przeglądarce. Teraz kliknij przycisk Edytuj sprawia, że można edytować klikniętej produktu (patrz rysunek 13).


[![Klikając przycisk edycji przycisku powoduje, że można edytować produktu](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Rysunek 13**: kliknięcie przycisku Edytuj sprawia, że edytowalne produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Krok 6: Trwa zapisywanie zmian s użytkownika

Kliknięcie przycisku s edytowanych produktu aktualizacji lub przyciski Anuluj nie działają na tym etapie; Aby dodać tę funkcję, należy utworzyć procedury obsługi zdarzeń dla elementu DataList s `UpdateCommand` i `CancelCommand` zdarzenia. Rozpocznij od utworzenia `CancelCommand` obsługi zdarzeń, który zostanie wykonany, gdy kliknięto przycisk Anuluj edytowanych produktu s i go zlecił ze zwracaniem do stanu przed edycji elementu DataList.

Aby DataList renderowania wszystkich jego elementów w trybie tylko do odczytu, musimy:

1. Ustaw DataList s [ `EditItemIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) do indeksu do nieistniejącej `DataListItem` indeksu. `-1` ponieważ jest bezpiecznym wyborem `DataListItem` indeksy liczone od `0`.
2. Ponownie powiązać dane do elementu DataList. Ponieważ nie `DataListItem` `ItemIndex` es odpowiadają DataList s `EditItemIndex`, cała DataList będzie renderowany w trybie tylko do odczytu.

Następującym kodem obsługi zdarzeń można wykonać następujące kroki:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Z tym dodatkiem kliknięcie przycisku Anuluj zwraca elementu DataList do stanu edytowania wstępnie.

Ostatnia procedura obsługi zdarzeń, musimy wykonać jest `UpdateCommand` obsługi zdarzeń. Ten program obsługi zdarzeń musi:

1. Uzyskania programowego dostępu do nazwy produktu wprowadzonych przez użytkownika i cen, a także edytowanych produktu s `ProductID`.
2. Inicjuje proces aktualizacji, wywołując odpowiednie `UpdateProduct` przeciążenia w `ProductsBLL` klasy.
3. Ustaw DataList s [ `EditItemIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) do indeksu do nieistniejącej `DataListItem` indeksu. `-1` ponieważ jest bezpiecznym wyborem `DataListItem` indeksy liczone od `0`.
4. Ponownie powiązać dane do elementu DataList. Ponieważ nie `DataListItem` `ItemIndex` es odpowiadają DataList s `EditItemIndex`, cała DataList będzie renderowany w trybie tylko do odczytu.

Kroki 1 i 2 są odpowiedzialne za zapisywanie użytkownika zmian s; kroki 3 i 4 przywrócić elementu DataList wstępnie edycji stan po zmiany zostały zapisane i są takie same jak kroki wykonywane w `CancelCommand` obsługi zdarzeń.

Aby uzyskać nazwę produktu zaktualizowane i cen, należy użyć `FindControl` metody programowo odwołania do kontrolki TextBox w sieci Web w obrębie `EditItemTemplate`. Musimy również pobrać edytowanych produktu s `ProductID` wartość. Gdy element ObjectDataSource możemy początkowo powiązana z elementu DataList, Visual Studio przypisane DataList s `DataKeyField` właściwości na wartość klucza podstawowego ze źródła danych (`ProductID`). Następnie można pobrać wartości z DataList s `DataKeys` kolekcji. Poświęć chwilę, aby upewnić się, że `DataKeyField` rzeczywiście ustawioną właściwość `ProductID`.

Poniższy kod implementuje cztery kroki:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Program obsługi zdarzeń, który rozpoczyna się od odczytu w produkcie edytowanych s `ProductID` z `DataKeys` kolekcji. Następnie dwóch pól tekstowych w `EditItemTemplate` odwołuje się i ich `Text` właściwości przechowywanych w zmiennych lokalnych, `productNameValue` i `unitPriceValue`. Używamy `Decimal.Parse()` metody można odczytać wartości z `UnitPrice` pole tekstowe, że jeśli wartość wprowadzona ma symbol waluty, można nadal ją poprawnie przekonwertować do `Decimal` wartości.

> [!NOTE]
> Wartości z `ProductName` i `UnitPrice` pola tekstowe są przypisane tylko do zmiennych productNameValue i unitPriceValue, jeśli właściwości tekst pola tekstowe mają określoną wartość. W przeciwnym razie wartość `Nothing` służy do zmiennych, które powoduje aktualizowanie danych z bazy danych `NULL` wartość. Oznacza to, naszego kodu traktuje konwertuje puste ciągi do bazy danych `NULL` wartości, które jest zachowaniem domyślnym edycji interfejsu w formantach GridView widoku DetailsView i FormView.


Po przeczytaniu wartości, `ProductsBLL` klasy s `UpdateProduct` metoda jest wywoływana, przekazując nazwy produktu s, ceny i `ProductID`. Program obsługi zdarzeń kończy się zwracając elementu DataList do stanu edytowania wstępnie przy użyciu dokładnie samej logiki, podobnie jak w `CancelCommand` programu obsługi zdarzeń.

Z `EditCommand`, `CancelCommand`, i `UpdateCommand` ukończyć procedury obsługi zdarzeń, obiekt odwiedzający można edytować nazwy i cena produktu. Rysunki 14-16 Pokaż ten przepływ edycji w akcji.


[![Po pierwszym odwiedzając stronę, wszystkie produkty są w trybie tylko do odczytu](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Rysunek 14**: podczas odwiedzania najpierw stronę, wszystkie produkty nie są w trybie tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Aby zaktualizować produkt s nazwy lub cen, kliknij przycisk Edytuj](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Rysunek 15**: Aby zaktualizować produkt s nazwy lub cen, kliknij przycisk Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Po zmianie wartości, kliknij przycisk Aktualizuj, aby wrócić do trybu tylko do odczytu](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Rysunek 16**: po zmianie wartości, kliknij polecenie Aktualizuj, aby wrócić do trybu tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Krok 7: Dodawanie funkcji usuwania

Dodawanie funkcji usuwania do elementu DataList kroki są podobne do tych dodawania możliwości edycji. Krótko mówiąc, należy dodać przycisk Usuń, aby `ItemTemplate` , po kliknięciu:

1. Odczytuje w produktu s `ProductID` za pośrednictwem `DataKeys` kolekcji.
2. Wykonuje delete wywołując `ProductsBLL` klasy s `DeleteProduct` metody.
3. Rebinds danych do elementu DataList.

S, Rozpocznij od dodania przycisk Usuń, aby umożliwić `ItemTemplate`.

Po kliknięciu przycisku których `CommandName` jest Edycja, Update, lub Anuluj zgłasza DataList s `ItemCommand` zdarzeń oraz dodatkowe zdarzenia (na przykład w przypadku, gdy za pomocą edycji `EditCommand` zdarzenie jest zgłaszane, jak również). Podobnie dowolnego przycisku, LinkButton lub ImageButton w elementu DataList których `CommandName` właściwość jest ustawiona na usunięcie przyczyny `DeleteCommand` zdarzenia (wraz z `ItemCommand`).

Dodawanie przycisku Usuń obok przycisku edycji w `ItemTemplate`, ustawienie jej `CommandName` właściwości do usunięcia. Po dodaniu tego przycisku kontrolować listy DataList s `ItemTemplate` składni deklaratywnej powinien wyglądać następująco:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla elementu DataList s `DeleteCommand` zdarzeń, używając następującego kodu:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Kliknięcie przycisku Usuń powoduje odświeżenie strony i jest uruchamiany DataList s `DeleteCommand` zdarzeń. W obsłudze zdarzeń klikniętej produktu s `ProductID` wartość jest dostępny z `DataKeys` kolekcji. Następnie produktu są usuwane przez wywołanie `ProductsBLL` klasy s `DeleteProduct` metody.

Po usunięciu produktu, jego s ważne, możemy ponownie powiązać dane do elementu DataList (`DataList1.DataBind()`), w przeciwnym razie elementu DataList będzie Pokaż produkt, który właśnie został usunięty.

## <a name="summary"></a>Podsumowanie

Elementu DataList nie ma punktu i kliknij przycisk edytowanie i usuwanie pomocy technicznej dla widoku GridView, z krótkim fragmentem kodu go może zostać poprawione uwzględnienie tych funkcji. Jak utworzyć dwie kolumny listę produktów, można go usunąć i którego nazwa i cen można edytowane widzieliśmy w tym samouczku. Dodawanie, edytowanie i usuwanie obsługi polega na tym odpowiednie formantów sieci Web w `ItemTemplate` i `EditItemTemplate`, tworzenie odpowiednich programów obsługi zdarzeń odczytywania wartości klucza podstawowego i wprowadzonych przez użytkownika i relacje z firmy Warstwy logiki.

Gdy dodaliśmy podstawowe edytowanie i usuwanie funkcji elementu DataList nie ma bardziej zaawansowane funkcje. Na przykład istnieje Weryfikacja nie pola wejściowego -, jeśli użytkownik wprowadzi cena za kosztowne, wyjątek zostanie zgłoszony przez `Decimal.Parse` w trakcie próby konwersji zbyt drogie w `Decimal`. Podobnie jeśli występuje problem podczas aktualizowania danych w logiki biznesowej i warstwy dostępu do danych użytkownik zobaczy komunikat o błędzie standard. Bez dowolny rodzaj potwierdzenia na przycisku Usuń wszystkie zbyt prawdopodobne jest przypadkowego usunięcia produktu.

W przyszłości wystąpi samouczki zajmiemy się tym, jak poprawić edycji użytkownika.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Nowak Zack, Krzysztof Pespisa i Randy Schmidt. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](performing-batch-updates-cs.md)
