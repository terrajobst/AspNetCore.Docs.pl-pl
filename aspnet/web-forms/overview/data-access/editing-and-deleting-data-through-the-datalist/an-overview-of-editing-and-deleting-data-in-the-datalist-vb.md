---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Omówienie edytowania i usuwania danych w kontrolce DataList (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Gdy kontrolki DataList nie ma wbudowanego edytowania i usuwania funkcji, w tym samouczku zobaczymy, jak utworzyć DataList, który obsługuje edytowania i usuwania o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5e2a0f672a9be074abd3ab92eb5b36c18d64d1b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364018"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Omówienie edytowania i usuwania danych w kontrolce DataList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) lub [Pobierz plik PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Gdy kontrolki DataList nie ma wbudowanego edytowania i usuwania funkcji, w tym samouczku zobaczymy sposób tworzenia DataList, obsługującego edytowania i usuwania swoich danych bazowych.


## <a name="introduction"></a>Wprowadzenie

W [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczek zobaczyliśmy, jak wstawianie, aktualizowanie i usuwanie danych przy użyciu architektury aplikacji, ObjectDataSource i GridView, DetailsView i FormView kontrolki. Za pomocą kontrolki ObjectDataSource i te kontrolki internetowe danych trzy implementacja interfejsy modyfikacji danych proste było bardzo proste i związane jedynie zaznaczenie pola wyboru z tagu inteligentnego. Żaden kod nie jest potrzebny do zapisania.

Niestety kontrolki DataList nie ma wbudowanego edytowania i usuwania możliwości w kontrolce GridView. Brak jest to termin w całości na fakt, że kontrolki DataList dane deklaratywne kontrolki źródła i stron modyfikacji danych niekorzystające z kodu były dostępne, jest relic z poprzedniej wersji programu ASP.NET. Gdy DataList programu ASP.NET 2.0 nie oferuje takie same, gotowych funkcji modyfikacji danych jako widoku GridView, możemy użyć technik 1.x ASP.NET oferować takie funkcje. Takie podejście wymaga fragmentem kodu, ale jak opisano w tym samouczku, kontrolki DataList ma niektóre zdarzenia i właściwości w celu pomocy w ramach tego procesu.

W tym samouczku opisano sposób tworzenia DataList, obsługującego edytowania i usuwania swoich danych bazowych. Sprawdzać przyszłych samouczki bardziej zaawansowane edytowanie i usuwanie scenariuszy, w tym poprawności pola wejściowego, bez problemu zmieniała obsługi wyjątki wywoływane z dostępu do danych lub warstwy logiki biznesowej i tak dalej.

> [!NOTE]
> Jak DataList kontrolce elementu powtarzanego brakuje poza funkcjonalność pola do wstawiania, aktualizowania lub usuwania. Chociaż można dodawać funkcjonalność, kontrolki DataList obejmuje właściwości i zdarzenia nie można odnaleźć w elemencie powtarzanym, które upraszczają dodawanie tych funkcji. W związku z tym w tym samouczku i wszystkie następne, które Przyjrzyj się edytowania i usuwania koncentruje się wyłącznie na elemencie DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Krok 1: Tworzenie stron sieci Web samouczki edytowania i usuwania

Zanim zaczniemy, eksplorowanie jak aktualizować i usuwać dane z kontrolką DataList, umożliwiają najpierw Poświęć chwilę, do tworzenia stron ASP.NET w naszym projektu witryny sieci Web, który będziemy potrzebować dla tego samouczka i dalej te kilka s. Rozpocznij od dodania nowy folder o nazwie `EditDeleteDataList`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Dodawanie stron ASP.NET samouczki](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki


Podobnie jak w przypadku innych folderów `Default.aspx` w `EditDeleteDataList` folder zawiera listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Na koniec Dodaj strony jako wpisy, aby `Web.sitemap` pliku. Ściślej mówiąc, Dodaj następujący kod po raporty wzorzec/szczegół za pomocą kontrolek DataList i Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementów dla DataList, edytowania i usuwania samouczków.


![Mapa witryny zawiera teraz wpisy dla DataList, edytowania i usuwania samouczki](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Rysunek 3**: mapy witryny zawiera teraz wpisy dla DataList, edytowania i usuwania samouczki


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Krok 2: Badanie techniki, aktualizowanie i usuwanie danych

Edytowanie i usuwanie danych za pomocą widoku GridView jest tak proste, ponieważ w obiekcie nadrzędnym kontrolkami GridView i kontrolki ObjectDataSource pracować wspólnie z optymalizacją. Zgodnie z opisem w [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) samouczek, gdy kliknięto przycisk Aktualizuj wiersz s, widoku GridView automatycznie przypisuje jej pola, które używane powiązanie dwukierunkowe danych z `UpdateParameters` zbiór jego ObjectDataSource, a następnie wywołuje tego s ObjectDataSource `Update()` metody.

Niestety kontrolki DataList nie zapewnia tej funkcji wbudowanych. Upewnij się, że użytkownika s są przypisane wartości parametrów elementu ObjectDataSource s i że naszych odpowiada jego `Update()` metoda jest wywoływana. Aby pomóc nam w ten pozwala, kontrolki DataList zawiera poniższe właściwości i zdarzenia:

- **[ `DataKeyField` Właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  podczas aktualizowania lub usuwania, musimy mieć możliwość identyfikują każdego elementu w elemencie DataList. Ustaw tę właściwość na klucz podstawowy wyświetlanych danych. Ten sposób spowoduje wypełnienie DataList s [ `DataKeys` kolekcji](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) z określonym `DataKeyField` wartość dla każdego elementu DataList.
- **[ `EditCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  generowane, gdy przycisk, element LinkButton lub ImageButton którego `CommandName` właściwość jest ustawiona na kliknięciu edycji.
- **[ `CancelCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  generowane, gdy przycisk, element LinkButton lub ImageButton którego `CommandName` właściwość jest ustawiona na kliknięciu anulowania.
- **[ `UpdateCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  generowane, gdy przycisk, element LinkButton lub ImageButton którego `CommandName` właściwość jest ustawiona na kliknięciu aktualizacji.
- **[ `DeleteCommand` Zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  generowane, gdy przycisk, element LinkButton lub ImageButton którego `CommandName` właściwość jest ustawiona na kliknięciu Delete.

Korzystając z tych właściwości i zdarzenia, istnieją cztery metody, których używamy do aktualizowania i usuwania danych w kontrolce DataList:

1. **Przy użyciu technik 1.x ASP.NET** kontrolki DataList istniały przed ASP.NET 2.0 i ObjectDataSources i był w stanie aktualizowanie i usuwanie danych wyłącznie w sposób programowy. Ta technika rowów kontrolki ObjectDataSource całkowicie i wymaga, że firma Microsoft powiązanie danych kontrolki DataList bezpośrednio z warstwy logiki biznesowej, zarówno podczas pobierania danych do wyświetlenia i w przypadku aktualizowania lub usuwania rekordu.
2. **Za pomocą jednego formantu ObjectDataSource na stronie wybranie pozycji, aktualizowanie i usuwanie** podczas kontrolki DataList GridView s nieprzerwaną pracę edytowanie i usuwanie możliwości, brakuje tam s bez powodu, możemy t dodane w określić główną przyczynę. W przypadku tej metody, możemy użyć kontrolki ObjectDataSource, podobnie jak w przykładach GridView, ale należy utworzyć procedurę obsługi zdarzeń dla kontrolek DataList s `UpdateCommand` zdarzeń, gdzie możemy ustawić parametry s ObjectDataSource i wywołanie jego `Update()` metody.
3. **Używanie formantu ObjectDataSource wybranie, ale aktualizowanie i usuwanie bezpośrednio przed LOGIKI** przy użyciu opcji 2, należy napisać ilość kodu w `UpdateCommand` zdarzeń, przypisywanie wartości parametrów i tak dalej. Zamiast tego firma Microsoft trzymaj się za pomocą kontrolki ObjectDataSource służąca do wybierania, ale wykonywanie wywołań aktualizowanie i usuwanie bezpośrednio w odniesieniu do LOGIKI (np. za pomocą opcji 1). W mojej opinii, aktualizowanie danych przez sprzężenie bezpośrednio za pomocą LOGIKI prowadzi do bardziej czytelny kod niż przypisywanie ObjectDataSource s `UpdateParameters` i wywoływania jego `Update()` metody.
4. **Za pomocą Deklaratywnych oznacza, że za pomocą wielu ObjectDataSources** poprzednich podejść trzy wszystkie wymagają fragmentem kodu. Jeśli d zamiast Zachowaj używane jako znacznie deklaratywne składni, jak to możliwe, Ostatnia opcja jest obejmujący wiele ObjectDataSources na stronie. Pierwszy elementu ObjectDataSource pobiera dane z LOGIKI i wiąże ją w elemencie DataList. Do aktualizacji, innej kontrolki ObjectDataSource dodane, ale dodany bezpośrednio w elemencie DataList s `EditItemTemplate`. Aby uwzględnić usuwanie pomocy technicznej, jeszcze innego elementu ObjectDataSource będzie potrzebna w `ItemTemplate`. W przypadku tej metody, są osadzone ObjectDataSource s Użyj `ControlParameters` deklaratywne parametrów elementu ObjectDataSource s można powiązać kontrolki wejściowe użytkownika (zamiast konieczności określania je programowo w elemencie DataList s `UpdateCommand` program obsługi zdarzeń). Takie podejście wymaga nadal ilość kodu, należy wywołać osadzony s ObjectDataSource `Update()` lub `Delete()` polecenia, ponieważ wymaga znacznie mniejsza niż razem z innymi trzy zbliża się. W tym miejscu minusem jest to, że wiele ObjectDataSources zaśmiecać stronie szkody dla czytelności ogólnej.

Wymuszenie tylko użyj jednej z tych metod, I d wybierz opcję 1, ponieważ zapewnia największą elastyczność i kontrolki DataList pierwotnie został zaprojektowany w celu uwzględnienia tego wzorca. Gdy kontrolki DataList został rozszerzony do pracy z kontrolki źródła danych programu ASP.NET 2.0, nie ma wszystkie punkty rozszerzeń lub funkcje oficjalne danych ASP.NET 2.0 kontrolki sieci Web (GridView, DetailsView i FormView). Opcje 2 do 4 nie są bez osiągnięć, mimo że.

To przyszłych edytowanie i usuwanie samouczki będzie używać kontrolki ObjectDataSource pobierania danych do wyświetlania i bezpośrednie wywołania LOGIKI do aktualizowania i usuwania danych (opcja 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Krok 3: Dodawanie kontrolki DataList i konfigurowania jej ObjectDataSource

W tym samouczku utworzymy DataList, który zawiera informacje o produkcie i dla każdego produktu umożliwia użytkownikowi możliwości, aby edytować nazwę i ceny i aby całkowicie usunąć produkt. W szczególności firma Microsoft pobierze rekordów do wyświetlenia za pomocą kontrolki ObjectDataSource, ale przeprowadzić aktualizację i akcje usuwania korzystając bezpośrednio z LOGIKI. Zanim firma martwienia się o wdrażanie funkcji edytowania i usuwania do elementu DataList, chętnie s najpierw pobrać strony, aby wyświetlić produktów w interfejsie tylko do odczytu. Ponieważ firma Microsoft ve zbadać te kroki w poprzednich samouczkach I będzie kontynuować przetwarza je szybko.

Zacznij od otwarcia `Basics.aspx` stronie `EditDeleteDataList` folder i w widoku Projekt, dodać kontrolką DataList do strony. Następnie należy utworzyć nowe kontrolki ObjectDataSource z tagu inteligentnego s DataList. Ponieważ pracujemy z danymi produktów, należy skonfigurować tak, aby użyć `ProductsBLL` klasy. Można pobrać *wszystkich* wybierz produkty, `GetProducts()` metody w karcie Wybierz.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Zwraca informacje o produkcie przy użyciu metody GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Rysunek 5**: zwraca informacje o produkt za pomocą `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


DataList, takich jak GridView, nie jest przeznaczona do wstawiania nowych danych; w związku z tym, wybierz opcję (Brak) z listy rozwijanej na karcie Wstawianie. Też (Brak) dla karty aktualizacji i usuwania, ponieważ aktualizacji i usuwania będą wykonywane programowo przy użyciu LOGIKI.


[![Upewnij się, że listy rozwijane w s ObjectDataSource WSTAWIANIA, aktualizacji i usuwania karty są ustawione na (Brak)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Rysunek 6**: Upewnij się, że listy rozwijane w s WSTAWIANIA, aktualizacji i usuwania karty ObjectDataSource są ustawione na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Po skonfigurowaniu kontrolki ObjectDataSource, kliknij przycisk Zakończ, wracając do projektanta. Ponieważ ve widoczne w poprzednich przykładach podczas kończenia konfiguracji elementu ObjectDataSource, Visual Studio automatycznie tworzy `ItemTemplate` DropDownList, wyświetlanie wszystkich pól danych. Zastąp to `ItemTemplate` o taki, który wyświetla tylko nazwę produktu s i ceny. Ponadto należy ustawić `RepeatColumns` właściwość 2.

> [!NOTE]
> Zgodnie z opisem w *Przegląd Wstawianie, aktualizowanie i usuwanie danych* samouczek, w przypadku modyfikowania danych za pomocą kontrolki ObjectDataSource Nasza architektura wymaga, że możemy usunąć `OldValuesParameterFormatString` właściwość z elementu ObjectDataSource s oznaczeniu deklaracyjnym (lub zresetować je do swojej wartości domyślnej `{0}`). W tym samouczku jednak użyto kontrolki ObjectDataSource tylko w przypadku pobierania danych. W związku z tym, nie należy modyfikować ObjectDataSource s `OldValuesParameterFormatString` wartość właściwości (mimo że jego t pogarszać Aby to zrobić).


Po zastąpieniu domyślne DataList `ItemTemplate` przy użyciu dostosowanych jeden oznaczeniu deklaracyjnym na stronie powinien wyglądać podobnie do następujących:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Poświęć chwilę, aby wyświetlić postępach za pośrednictwem przeglądarki. Jak pokazano na rysunku 7, kontrolki DataList wyświetlana jest cena nazwy i jednostki produktu dla każdego produktu w dwóch kolumnach.


[![Produkty, nazwy i ceny są wyświetlane w DataList dwie kolumny](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Rysunek 7**: nazwy produktów i ceny są wyświetlane w DataList dwie kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> Kontrolki DataList ma wiele właściwości, które są wymagane przez proces aktualizowania i usuwania, a te wartości są przechowywane w widoku stanu. W związku z tym gdy kompilowanie kontrolką DataList, która obsługuje edytowania lub usuwania danych, jest niezbędne, można włączyć stan widoku s DataList.  
>   
>  Uważni czytnika może pamiętasz, że możemy wyłączyć stan widoku, tworząc edytowalne GridViews DetailsViews i FormViews. Jest to spowodowane formantów sieci Web ASP.NET 2.0 mogą obejmować *kontrolowany stan*, który jest stan zachowywany ogłaszania zwrotnego, takich jak stan widoku, ale zakładany niezbędne.


Wyłączanie widok stanu, w widoku GridView jedynie pomija informacje o stanie prosta, ale zachowuje stan formantu (w tym stanie niezbędne do edytowania i usuwania). DataList, jakby został utworzony w przedziale czasu 1.x ASP.NET, nie używa stan formantu i dlatego musi mieć stan widoku włączone. Zobacz [vs stan kontrolki. Wyświetl stan](https://msdn.microsoft.com/library/1whwt1k7.aspx) uzyskać więcej informacji dotyczących przeznaczenia stan formantu i czym różni się ona od stanu widoku.

## <a name="step-4-adding-an-editing-user-interface"></a>Krok 4. Dodawanie edycji interfejsu użytkownika

W kontrolce GridView składa się z kolekcji pól (BoundFields CheckBoxFields, kontrolek TemplateField i tak dalej). Te pola można dostosować ich renderowanego kodu znaczników w zależności od ich trybu. Na przykład w trybie tylko do odczytu elementu BoundField spowoduje wyświetlenie jej wartość pola danych jako tekst. w trybie edycji renderowaniu sieci Web w polu tekstowym formant, którego `Text` właściwość jest przypisywana wartość pola danych.

DataList, z drugiej strony, powoduje wyświetlenie jego elementów za pomocą szablonów. Elementy tylko do odczytu są renderowane przy użyciu `ItemTemplate` elementów w trybie edycji są uznane za pośrednictwem `EditItemTemplate`. W tym momencie nasze DataList ma tylko `ItemTemplate`. Do obsługi funkcji edycji schodzić do poziomu, musimy dodać `EditItemTemplate` zawierający znaczników, który ma być wyświetlany dla elementu można edytować. W tym samouczku użyjemy kontrolki sieci Web w polu tekstowym do edycji cena produktu s nazwę i jednostkę.

`EditItemTemplate` Można utworzyć deklaratywne lub za pomocą projektanta (przez wybranie opcji Edytuj szablony z tagu inteligentnego DataList s). Aby użyć opcji Edytuj szablony, najpierw kliknij link Edytuj szablony w tagu inteligentnego, a następnie wybierz pozycję `EditItemTemplate` elementu z listy rozwijanej.


[![Zoptymalizowany pod kątem do pracy za pomocą kontrolek DataList EditItemTemplate s](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Rysunek 8**: zdecydować się na pracy za pomocą kontrolek DataList s `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


Następnie wpisz w polu Nazwa produktu: i cena:, a następnie przeciągnij dwie kontrolki TextBox z przybornika do `EditItemTemplate` interfejsu w projektancie. Ustawianie pól tekstowych `ID` właściwości `ProductName` i `UnitPrice`.


[![Dodaj pole tekstowe Nazwa s produktu i cenę](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Rysunek 9**: Dodaj pole tekstowe dla produktu s Nazwa i cena ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


Należy powiązać odpowiedniego produktu pola wartości danych `Text` właściwości dwóch pól tekstowych. Z tagów inteligentnych pól tekstowych, kliknij link Edytuj powiązania danych, a następnie skojarzyć pola odpowiednie dane za pomocą `Text` właściwości, jak pokazano na rysunku nr 10.

> [!NOTE]
> Podczas tworzenia powiązania `UnitPrice` pola danych do ceny TextBox s `Text` pola, można formatować je jako wartość waluty (`{0:C}`), ogólne numer (`{0:N}`), lub pozostaw to pole niesformatowany.


![Powiązywanie właściwości Text pól tekstowych ProductName i pola danych UnitPrice](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Na rysunku nr 10**: powiązywanie `ProductName` i `UnitPrice` polom danych `Text` właściwości pól tekstowych


Zwróć uwagę, jak odbywa się okno dialogowe Edytuj powiązania danych na rysunku nr 10 *nie* obejmują wyboru dwukierunkowego wiązania danych, który jest obecny podczas edytowania TemplateField w kontrolce GridView lub DetailsView lub szablonu w widoku FormView. Funkcja dwukierunkowego wiązania danych dozwolona wartość wprowadzoną w danych wejściowych kontrolki sieci Web ma być automatycznie przypisane do odpowiednich s ObjectDataSource `InsertParameters` lub `UpdateParameters` podczas wstawiania lub aktualizowania danych. Kontrolki DataList nie obsługuje dwukierunkowego wiązania danych, ponieważ zobaczymy później w tym samouczku po sprawia, że użytkownik jej zmiany i jest gotowy do aktualizacji danych, trzeba uzyskać programowy dostęp do tych pól tekstowych `Text` właściwości i przekaż ich wartości odpowiednie `UpdateProduct` method in Class metoda `ProductsBLL` klasy.

Na koniec musimy dodać aktualizacji i przycisków, aby anulować `EditItemTemplate`. Jak widzieliśmy w [Master/szczegółów przy użyciu punktowanej listy z rekordów głównych z kontrolką DataList szczegółów](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) samouczek, gdy przycisk, element LinkButton lub ImageButton którego `CommandName` właściwość jest ustawiona po kliknięciu z w obrębie elementu powtarzanego lub DataList, S Repeater lub DataList `ItemCommand` zdarzenie jest wywoływane. Dla elementu DataList Jeśli `CommandName` właściwości jest równa określonej wartości, mogą być również wywoływane dodatkowego zdarzenia. Specjalne `CommandName` wartości właściwości należą między innymi:

- Anuluj zgłasza `CancelCommand` zdarzeń
- Edytuj zgłasza `EditCommand` zdarzeń
- Aktualizuj zgłasza `UpdateCommand` zdarzeń

Należy pamiętać, że te zdarzenia są wywoływane *oprócz* `ItemCommand` zdarzeń.

Dodaj do `EditItemTemplate` dwie kontrolki przycisku w sieci Web, po jednym którego `CommandName` jest ustawiona na aktualizację i inne zasoby, ustaw opcję Anuluj. Po dodaniu tych dwóch formantów Web przycisk projektanta powinien wyglądać podobnie do poniższej:


[![Dodawanie aktualizacji i przyciski do EditItemTemplate "Anuluj"](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Rysunek 11**: Dodawanie aktualizacji i przyciski Anuluj, aby `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Za pomocą `EditItemTemplate` pełną znaczników deklaratywne DataList s powinien wyglądać podobnie do następującego:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Krok 5. Dodawanie żmudne procesy, aby przejść do trybu edycji

W tym momencie nasze DataList ma interfejs edycji zdefiniowanych za pomocą jego `EditItemTemplate`; jednak miejsca s aktualnie żaden sposób dla użytkownika, odwiedź naszą stronę wskazują, że chce edytować informacje o produkcie s. Musimy dodać przycisk edycji do każdego produktu, po kliknięciu renderuje DataList tego elementu w trybie edycji. Rozpocznij od dodania przycisk Edytuj, aby `ItemTemplate`, za pomocą projektanta lub deklaratywnie. Aby ustawić przycisk Edytuj s `CommandName` właściwość do edycji.

Po dodaniu tego przycisku edycji potrwać chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. Dodając każda lista produktów powinny zawierać dostępny przycisk Edytuj.


[![Dodawanie aktualizacji i przyciski do EditItemTemplate "Anuluj"](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Rysunek 12**: Dodawanie aktualizacji i przyciski Anuluj, aby `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Kliknięcie przycisku powoduje odświeżenie strony, ale *nie* Przenieś produkty do trybu edycji. Umożliwiają produktu można edytować, należy:

1. Ustaw DataList s [ `EditItemIndex` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) na indeks `DataListItem` przycisk Edytuj, której właśnie został kliknięty.
2. Ponownie powiązać dane do kontrolki DataList. Po ponownym renderowanych kontrolki DataList `DataListItem` którego `ItemIndex` odpowiada za pomocą kontrolek DataList s `EditItemIndex` będą renderowane przy użyciu jego `EditItemTemplate`.

Ponieważ DataList s `EditCommand` zdarzenie jest wywoływane, gdy kliknięto przycisk Edytuj, Utwórz `EditCommand` programu obsługi zdarzeń z następującym kodem:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand` Programu obsługi zdarzeń jest przekazywany obiekt typu `DataListCommandEventArgs` jako drugi parametr wejściowy, który zawiera odwołanie do `DataListItem` został kliknięty przycisk Edytuj, którego (`e.Item`). Program obsługi zdarzeń najpierw ustawia DataList s `EditItemIndex` do `ItemIndex` z edytowalne `DataListItem` i następnie rebinds danych do kontrolki DataList przez wywołanie metody DataList s `DataBind()` metody.

Po dodaniu tej obsługi zdarzeń, ponownie stronę w przeglądarce. Teraz kliknięcie przycisku Edytuj sprawia, że można edytować kliknięto produktu (patrz rysunek 13).


[![Klikając przycisk edycji przycisk sprawia, że można edytować produktu](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Rysunek 13**: kliknięcie przycisku Edytuj sprawia, że edytowalne produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Krok 6: Zapisywanie zmian s użytkownika

Kliknięcie produktu edytowanych s aktualizacji lub przyciski Anuluj nie robi nic, w tym momencie; Aby dodać tę funkcję, należy utworzyć procedury obsługi zdarzeń dla kontrolek DataList s `UpdateCommand` i `CancelCommand` zdarzenia. Rozpocznij od utworzenia `CancelCommand` program obsługi zdarzeń, które będą wykonywane, gdy kliknięto przycisk Anuluj edytowanych produkt s nadzorowania go ze zwracaniem kontrolki DataList stan wstępnie edycji.

Aby DataList renderowania, wszystkie jego elementy w trybie tylko do odczytu, należy:

1. Ustaw DataList s [ `EditItemIndex` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) do indeksu nieistniejącej `DataListItem` indeksu. `-1` ponieważ jest bezpiecznym wyborem `DataListItem` indeksy rozpoczynają się od `0`.
2. Ponownie powiązać dane do kontrolki DataList. Ponieważ nie `DataListItem` `ItemIndex` es odpowiadają DataList s `EditItemIndex`, cały DataList będą renderowane w trybie tylko do odczytu.

Te kroki można osiągnąć przy użyciu następującego kodu programu obsługi zdarzeń:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Dodając klikając zwraca kontrolki DataList przycisku Anuluj do stanu wstępnie edycji.

Ostatnia procedura obsługi zdarzeń, musimy wykonać jest `UpdateCommand` programu obsługi zdarzeń. Ta procedura obsługi zdarzeń musi:

1. Programowy dostęp do nazwy produktu wprowadzonych przez użytkownika i ceny, tak jak edytowanych produkt s `ProductID`.
2. Inicjuje proces aktualizacji, przez wywołanie odpowiedniej `UpdateProduct` przeciążenia w `ProductsBLL` klasy.
3. Ustaw DataList s [ `EditItemIndex` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) do indeksu nieistniejącej `DataListItem` indeksu. `-1` ponieważ jest bezpiecznym wyborem `DataListItem` indeksy rozpoczynają się od `0`.
4. Ponownie powiązać dane do kontrolki DataList. Ponieważ nie `DataListItem` `ItemIndex` es odpowiadają DataList s `EditItemIndex`, cały DataList będą renderowane w trybie tylko do odczytu.

Kroki 1 i 2 są odpowiedzialne za zapisywanie użytkownika zmiany s. kroki 3 i 4 przywrócić kontrolki DataList wstępnie edycji stan po zmiany zostały zapisane i są takie same jak kroki wykonywane w `CancelCommand` programu obsługi zdarzeń.

Aby uzyskać nazwę zaktualizowanego produktu i cenę, musimy użyć `FindControl` metodę, aby programowo odwołania sieci Web pola tekstowego kontrolki w ramach `EditItemTemplate`. Musimy również uzyskać produkt edytowanych s `ProductID` wartość. Gdy firma Microsoft początkowo powiązana kontrolki ObjectDataSource z kontrolki DataList, Visual Studio przypisane DataList s `DataKeyField` właściwości wartość klucza podstawowego źródła danych (`ProductID`). Następnie można pobrać tę wartość w elemencie DataList s `DataKeys` kolekcji. Poświęć chwilę, aby upewnić się, że `DataKeyField` jest w rzeczywistości właściwością `ProductID`.

Poniższy kod implementuje cztery kroki:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Program obsługi zdarzeń uruchamia, czytając w produkcie edytowanych s `ProductID` z `DataKeys` kolekcji. Następnie dwóch pól tekstowych w `EditItemTemplate` są określone i ich `Text` właściwości przechowywane w zmiennych lokalnych `productNameValue` i `unitPriceValue`. Używamy `Decimal.Parse()` metodę, aby odczytać wartości z `UnitPrice` pola tekstowego, tak że jeśli wartość wprowadzona zawiera symbol waluty, nadal poprawnie konwersji do `Decimal` wartości.

> [!NOTE]
> Wartości z `ProductName` i `UnitPrice` pola tekstowe są przypisane tylko do zmiennych productNameValue i unitPriceValue, jeśli określona wartość właściwości tekstu pola tekstowe. W przeciwnym razie wartość `Nothing` służy do zmiennych, która powoduje aktualizowanie danych za pomocą bazy danych `NULL` wartość. Oznacza to, że nasz kod traktuje konwertuje puste ciągi do bazy danych `NULL` wartości, które jest to domyślne zachowanie interfejsu edycji, z kontrolki GridView DetailsView i FormView.


Po przeczytaniu wartości, `ProductsBLL` klasy s `UpdateProduct` metoda jest wywoływana, przekazując nazwę produktu s, ceny i `ProductID`. Program obsługi zdarzeń kończy się, zwracając kontrolki DataList stan wstępnie edycji przy użyciu dokładnie tę samą logikę, podobnie jak w `CancelCommand` programu obsługi zdarzeń.

Za pomocą `EditCommand`, `CancelCommand`, i `UpdateCommand` zakończenia procedury obsługi zdarzeń, obiekt odwiedzający można edytować nazwę i cena produktu. Rysunki 14-16 wyświetlenie tego edycji przepływu pracy w działaniu.


[![Po pierwsze, odwiedzając stronę, wszystkie produkty są w trybie tylko do odczytu](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Rysunek 14**: po pierwsze, odwiedzając stronę, wszystkie produkty są w trybie tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Aktualizacja produktu, nazwa lub cen, kliknij przycisk Edytuj](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Rysunek 15**: Aby zaktualizować produkt s nazwy ani ceny, kliknij przycisk Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Po zmianie wartości, kliknij przycisk Aktualizuj, aby powrócić do trybu tylko do odczytu](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Rysunek 16**: po zmianie wartości, kliknij polecenie Aktualizuj, aby powrócić do trybu tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Krok 7: Dodawanie funkcji Delete

Dodawanie funkcji usuwania do elementu DataList kroki są podobne do dodawania możliwość edycji. Krótko mówiąc, musimy dodać przycisk Usuń, aby `ItemTemplate` , po kliknięciu:

1. Odczytuje odpowiedniego produktu s `ProductID` za pośrednictwem `DataKeys` kolekcji.
2. Wykonuje delete, wywołując `ProductsBLL` klasy s `DeleteProduct` metody.
3. Rebinds danych do kontrolki DataList.

S, Rozpocznij od dodania przycisk Usuń, aby umożliwić `ItemTemplate`.

Po kliknięciu przycisku którego `CommandName` jest Edycja, Update, lub Anuluj zgłasza DataList s `ItemCommand` zdarzeń wraz ze zdarzeniem dodatkowe (na przykład w przypadku, gdy za pomocą edycji `EditCommand` zdarzenie jest zgłaszane w także). Podobnie dowolnego przycisku, element LinkButton lub ImageButton w elemencie DataList którego `CommandName` właściwość jest ustawiona na Usuń przyczyny `DeleteCommand` wyzwolenie zdarzenia (wraz z `ItemCommand`).

Dodaj przycisk usuwania obok przycisku edycji w `ItemTemplate`, ustawiając jego `CommandName` właściwości do usunięcia. Po dodaniu tego przycisku sterowania listy DataList s `ItemTemplate` składni deklaratywnej powinien wyglądać następująco:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Następnie należy utworzyć procedurę obsługi zdarzeń dla kontrolek DataList s `DeleteCommand` zdarzeń, używając następującego kodu:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Kliknięcie przycisku Usuń powoduje odświeżenie strony, a następnie uruchamia DataList s `DeleteCommand` zdarzeń. W obsłudze zdarzeń kliknięto produkt s `ProductID` wartość są dostępne z `DataKeys` kolekcji. Następnie produkt zostanie usunięty, wywołując `ProductsBLL` klasy s `DeleteProduct` metody.

Po usunięciu produktu, jego ważne, możemy ponownie powiązać dane do kontrolki DataList (`DataList1.DataBind()`), w przeciwnym razie kontrolki DataList będą nadal istnieć produktu, który właśnie został usunięty.

## <a name="summary"></a>Podsumowanie

Kontrolki DataList nie posiada punkt i kliknij przycisk edytowania i usuwania dla widoku GridView pomocy technicznej, z krótkim fragmentem kodu go może zostać poprawione obejmujący te funkcje. Jak utworzyć dwie kolumny listę produktów, można go usunąć i którego nazwa i cena może być edytowany widzieliśmy w ramach tego samouczka. Dodawanie, edytowanie i usuwanie pomocy technicznej jest kwestią odpowiednie kontrolki sieci Web, w tym `ItemTemplate` i `EditItemTemplate`, tworzenie odpowiednich procedur obsługi zdarzeń, odczytując wartości wprowadzone przez użytkownika i podstawowego klucza i komunikuje się z usługą firmy Warstwy logiki.

Dodaliśmy podstawowe edytowania i usuwania możliwości kontrolki DataList, nie ma bardziej zaawansowane funkcje. Na przykład, nie ma żadnych weryfikacji pola wejściowego —, jeśli użytkownik wprowadzi cenie za kosztowne, zostanie zgłoszony wyjątek przez `Decimal.Parse` w trakcie próby konwersji zbyt drogie do `Decimal`. Podobnie jeśli występuje problem podczas aktualizowania danych w logice biznesowej i warstwy dostępu do danych użytkownik zobaczy na ekranie błędu standardowego. Bez dowolny rodzaj potwierdzenie przycisk usuwania przypadkowego usunięcia produktu prawdopodobnie wszystkie zbyt.

Samouczki, zobaczymy, jak poprawić edycji użytkownika środowiska w przyszłości.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Zack Jones, Krzysztof Pespisa i Randy Schmidt. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](customizing-the-datalist-s-editing-interface-cs.md)
> [dalej](performing-batch-updates-vb.md)
