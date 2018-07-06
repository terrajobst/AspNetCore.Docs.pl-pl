---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC (3 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f5ce5f926021a53a5dd96e578b26b8c186c9a83c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841037"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC (3 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku wdrożono zestaw stron sieci web w przypadku podstawowych operacji CRUD dla `Student` jednostek. W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcje **studentów** strony indeksu. Utworzysz też strony, która wykonuje prostą grupowania.

Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu. Nagłówki kolumn odnoszą się łącza, które użytkownik może kliknąć, aby posortować według tej kolumny. Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Dodaj kolumnę sortowania łącza do strony indeksu uczniów

Aby dodać sortowanie do strony indeksu dla uczniów, zmienisz `Index` metody `Student` kontrolera i Dodaj kod do `Student` indeksować widoku.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodaj sortowanie funkcji Index — metoda

W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu zapytania jest dostarczane przez platformę ASP.NET MVC jako parametr do metody akcji. Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia i ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Przy pierwszym żądaniu strony indeksu nie jest Brak ciągu zapytania. Uczniów są wyświetlane w porządku rosnącym według `LastName`, co jest ustawieniem domyślnym, zgodnie z ustaleniami w należą do przypadku `switch` instrukcji. Kiedy użytkownik kliknie hiperlink nagłówek kolumny, odpowiednie `sortOrder` podana w ciągu zapytania.

Dwa `ViewBag` zmienne są używane, tak aby widok można skonfigurować hiperłącza nagłówków kolumn za pomocą wartości ciągu zapytania odpowiednie:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Są to trójargumentowy instrukcji. Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub jest pusta, `ViewBag.NameSortParm` powinna być ustawiona na "nazwa\_desc"; w przeciwnym razie należy ją ustawić na pusty ciąg. Te dwie instrukcje Włącz widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

| Bieżący kolejność sortowania | Ostatnia nazwa hiperłącza | Data hiperłącza |
| --- | --- | --- |
| Ostatnie nazwy, rosnąco | descending | ascending |
| Ostatnie nazwy, malejąco | ascending | ascending |
| Data, w kolejności rosnącej | ascending | descending |
| Data, malejąco | ascending | ascending |

Metoda używa [składnik LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) określić kolumnę sortowania. Ten kod tworzy [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) zmiennej przed `switch` instrukcji, modyfikuje je w `switch` instrukcji i wywołania `ToList` metody `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, bez określenia zapytania są wysyłane do bazy danych. Zapytanie nie jest wykonywane, dopóki nie można przekonwertować `IQueryable` obiektu w kolekcji przez wywołanie metody, takie jak `ToList`. W związku z tym, ten kod powoduje pojedyncze zapytanie, które nie jest wykonywane, dopóki `return View` instrukcji.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodaj kolumnę nagłówka hiperłącza do widoku indeksu dla uczniów

W *Views\Student\Index.cshtml*, Zastąp `<tr>` i `<th>` elementy wiersz nagłówka z wyróżniony kod:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Ten kod używa tych informacji w `ViewBag` właściwości, aby skonfigurować hiperlinki z zapytaniem odpowiednie wartości ciągu.

Uruchom stronę, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** nagłówków kolumn, aby zweryfikować, że sortowanie działa.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po kliknięciu **nazwisko** nagłówka, studentów są wyświetlane w ostatniej malejącej nazwy.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów

Aby dodać filtrowanie do strony indeksu studentów, dodasz pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem (zmiany zostały wyróżnione):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Po dodaniu `searchString` parametr `Index` metody. Możesz również zostały dodane do instrukcji LINQ `where` clausethat wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, które należy dodać do widoku indeksu. Instrukcja, która dodaje [gdzie](https://msdn.microsoft.com/library/bb535040.aspx) klauzula jest wykonywany tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tej samej metody, zestaw jednostek platformy Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci. Wyniki są zazwyczaj takie same, ale w niektórych przypadkach może być inna. Na przykład implementacji .NET Framework z `Contains` metoda zwraca wszystkie wiersze, w przypadku przekazania pustego ciągu do niego, ale dostawcy środowiska Entity Framework dla programu SQL Server Compact 4.0 zwraca zero wierszy obecność pustych ciągów. W związku z tym kodem w przykładzie (umieszczanie `Where` instrukcji wewnątrz `if` instrukcji) zapewnia, że, Pobierz te same wyniki dla wszystkich wersji programu SQL Server. Ponadto wdrożenia programu .NET Framework z `Contains` metoda wykonuje porównania uwzględniającego wielkość liter, domyślnie, ale dostawcy programu Entity Framework SQL Server wykonania porównania bez uwzględniania wielkości liter, domyślnie. Dlatego wywołanie `ToUpper` metody testu jawnie bez uwzględniania wielkości liter zapewnia wyniki nie należy zmieniać po zmianie kodu później, aby korzystać z repozytorium, które zwróci `IEnumerable` zbiór zamiast `IQueryable` obiektu. (Gdy wywołujesz `Contains` metody `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; Jeśli wywołasz ją na `IQueryable` obiektu, możesz uzyskać implementację dostawcy bazy danych.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla uczniów

W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwierającym `table` tag, aby można było utworzyć podpis, pola tekstowego, a **wyszukiwania** przycisku.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Uruchom strony, wprowadź wyszukiwany ciąg, a następnie kliknij przycisk **wyszukiwania** Aby sprawdzić, czy działa filtrowanie.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Należy zauważyć, że adres URL nie zawiera "" ciąg wyszukiwania, co oznacza, że jeśli Oznacz tę stronę zakładką nie otrzymasz filtrowana lista korzystając z zakładki. Poznasz, jak zmienić **wyszukiwania** przycisk, aby użyć ciągów zapytań do kryteriów filtrowania w dalszej części tego samouczka.

## <a name="add-paging-to-the-students-index-page"></a>Dodawanie stronicowania do strony indeksu uczniów

Dodawanie stronicowania do strony indeksu studentów, zostanie najpierw należy zainstalować **PagedList.Mvc** pakietu NuGet. Będzie wprowadzić dodatkowe zmiany w `Index` metody i dodawanie stronicowania łącza do `Index` widoku. **PagedList.Mvc** jest jednym z wielu dobre stronicowania i sortowania pakietów dla platformy ASP.NET MVC i jego użycia w tym miejscu jest przeznaczona tylko na potrzeby przykładu nie jako zalecenie dla niego inne opcje. Poniższa ilustracja przedstawia łącza stronicowania.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet PagedList.MVC NuGet

NuGet **PagedList.Mvc** pakiet automatycznie instaluje **PagedList** pakietu jako zależność. **PagedList** pakiet instaluje `PagedList` kolekcji typu i rozszerzenie metody `IQueryable` i `IEnumerable` kolekcji. Metody rozszerzające tworzenia pojedynczej strony danych w `PagedList` kolekcji z Twojej `IQueryable` lub `IEnumerable`i `PagedList` kolekcji zawiera kilka właściwości i metod, które ułatwiają stronicowania. **PagedList.Mvc** pakiet Instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** i następnie **Zarządzaj pakietami NuGet dla rozwiązania**.

W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **Online** karcie po lewej stronie, a następnie wprowadź "stronicowanej" w polu wyszukiwania. Po wyświetleniu **PagedList.Mvc** pakietu, kliknij przycisk **zainstalować**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

W **Wybierz projekty** kliknij **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *Controllers\StudentController.cs*, Dodaj `using` poufności informacji dotyczące `PagedList` przestrzeni nazw:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ten kod dodaje `page` parametr, bieżący parametr kolejność sortowania i bieżącego parametru filtru w podpisie metody, jak pokazano poniżej:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknie, stronicowanie i sortowanie łącza, wszystkich parametrów będzie pusta. Po kliknięciu łącza stronicowania `page` zmienna będzie zawierać numer strony, aby wyświetlić.

`A ViewBag` dostarcza widok z bieżącej kolejności sortowania, ponieważ ten musi być uwzględniona w łącza stronicowania Aby zachować porządek sortowania, taki sam, podczas stronicowania:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Inna właściwość `ViewBag.CurrentFilter`, zawiera widok bieżący ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i musi on zostać przywrócony do pola tekstowego po stronie zostanie wyświetlony ponownie. Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony musi być ustawiony na 1, nowy filtr może powodować różne dane do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i naciśnięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie ma wartości null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na końcu metody `ToPagedList` metody rozszerzenia na uczniów `IQueryable` obiektu konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie. Tego jednostronicowej studentów są następnie przekazywane do widoku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda przyjmuje numeru strony. Dwa znaki zapytania reprezentują [operatorem łączenia wartości null](https://msdn.microsoft.com/library/ms173224.aspx). Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza, że zwracają wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawanie stronicowania łączy do widoku indeksu dla uczniów

W *Views\Student\Index.cshtml*, Zastąp istniejący kod następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model` Instrukcji w górnej części strony określa widok teraz pobiera `PagedList` zamiast obiektu `List` obiektu.

`using` Poufności informacji dotyczące `PagedList.Mvc` daje dostęp do pomocy MVC przycisków stronicowania.

Kod używa przeciążenia [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) umożliwiająca, aby określić [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Wartość domyślna [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) przesyła dane formularza z WPISEM, co oznacza, że parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, dane formularza jest przekazywany w adresie URL jako ciągi zapytań, która umożliwia użytkownikom utworzyć zakładkę ma adres URL. [W3C wytyczne dotyczące użytkowania HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) Określ, czy powinien być używany GET akcja nie powoduje aktualizacji.

Pole tekstowe jest inicjowany z aktualnie wyszukiwanego ciągu, dzięki czemu po kliknięciu nowej strony można przeglądać aktualnie wyszukiwanego ciągu.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Linki nagłówek kolumny, użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do kontrolera, dzięki czemu użytkownik może sortować w ramach wyników filtrowania:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Bieżącej strony i łączna liczba stron są wyświetlane.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

W przypadku stron do wyświetlania jest wyświetlany "Page 0 0". (W tym przypadku jest większa niż liczba stron na numer strony ponieważ `Model.PageNumber` wynosi 1, a `Model.PageCount` wynosi 0.)

Przyciski stronicowania są wyświetlane przez `PagedListPager` pomocy:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Pomocnik zapewnia kilka opcji, które można dostosować, w tym adresy URL i ustawianie stylów. Aby uzyskać więcej informacji, zobacz [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.

Uruchom stronę.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Po kliknięciu łączy stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź wyszukiwany ciąg i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie przy użyciu sortowania i filtrowania.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Utwórz informacje o stronie, które znajdują się dane statystyczne dla uczniów

Dla firmy Contoso University witryny internetowej informacje o stronie będą wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to obliczeń grupowania i prostych grup. Aby to osiągnąć, wykonasz następujące czynności:

- Utwórz klasę modelu widoku danych potrzebnych do przekazania do widoku.
- Modyfikowanie `About` method in Class metoda `Home` kontrolera.
- Modyfikowanie `About` widoku.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Tworzenie *modele widoków* folderu. W tym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie kontrolera głównego

W *HomeController.cs*, Dodaj następujący kod `using` instrukcji w górnej części pliku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Zastąp `About` metoda następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.

Dodaj `Dispose` metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

Zastąp kod w *Views\Home\About.cshtml* pliku następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uruchom aplikację, a następnie kliknij przycisk **o** łącza. Liczba studentów każdej daty rejestracji jest wyświetlany w tabeli.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcjonalnie: Wdrażanie aplikacji na platformie Windows Azure

Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić innym osobom korzystanie przez Internet, należy je wdrożyć do dostawcy usług hosta sieci web. Ta opcjonalna sekcja samouczka przedstawiono wdrażania go do witryny sieci Web Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Wdrażanie bazy danych przy użyciu migracje Code First

Aby wdrożyć bazy danych będzie użyć migracje Code First. Gdy tworzysz profil publikowania, który umożliwia konfigurowanie ustawień dla wdrażania z programu Visual Studio, należy wybrać pole wyboru, która jest oznaczona etykietą **wykonaj migracje Code First (uruchomieniu aplikacji)**. To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować aplikację *Web.config* plików na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.

Program Visual Studio nie działa z bazą danych w procesie wdrażania. Gdy wdrożonej aplikacji uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First automatycznie tworzy bazę danych lub aktualizuje schemat bazy danych do najnowszej wersji. Jeśli aplikacja implementuje migracje `Seed` metody, metoda uruchomienia po utworzeniu bazy danych lub schemat jest aktualizowany.

Migracji `Seed` metoda wstawia dane z badań. Jeśli były wdrażane w środowisku produkcyjnym, musisz zmienić `Seed` metodę, tak aby tylko wstawia dane, który ma zostać wstawiony do produkcyjnej bazy danych. Na przykład w bieżącym modelu danych warto mieć rzeczywistych kursów, ale fikcyjnej studentów w bazie danych rozwoju. Można napisać `Seed` metodę, aby załadować zarówno w rozwoju, a następnie przekształcić w komentarz fikcyjnej studentów przed wdrożeniem w środowisku produkcyjnym. Lub możesz napisać `Seed` metodę, aby załadować tylko kursów, a następnie wprowadź fikcyjnej studentów w bazie danych testu ręcznie przy użyciu interfejsu użytkownika aplikacji.

### <a name="get-a-windows-azure-account"></a>Załóż konto usługi Windows Azure

Musisz mieć konto usługi Windows Azure. Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Tworzenie witryny sieci web i bazy danych SQL na platformie Windows Azure

Witryny Windows Azure uruchomi we współużytkowanym środowisku hostingu, co oznacza, że działa na maszynach wirtualnych (VM), które są współużytkowane z innymi klientami usługi Windows Azure. Udostępnione Środowisko hostingu jest sposób niskie koszty, aby rozpocząć pracę w chmurze. Później Jeśli powoduje zwiększenie ruchu w sieci web, aplikację można skalować do spełnia potrzeby, uruchamiając na dedykowanych maszynach wirtualnych. Jeśli potrzebujesz bardziej złożone architektury, można migrować do usługi Windows Azure w chmurze. Uruchamianie usług w chmurze na dedykowanych maszynach wirtualnych, które można skonfigurować zgodnie z potrzebami.

Windows Azure SQL Database to usługa relacyjnej bazy danych opartej na chmurze, która jest oparta na technologiach programu SQL Server. Narzędzia i aplikacje, które działają z programem SQL Server również działać z usługą SQL Database.

1. W [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/), kliknij przycisk **witryn sieci Web** w karcie po lewej stronie, a następnie kliknij przycisk **New**.

    ![Przycisk Nowy w portalu zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Kliknij przycisk **tworzenia NIESTANDARDOWEGO**.

    ![Tworzenie za pomocą łącza bazy danych w portalu zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **Nową witrynę sieci Web — tworzenie niestandardowe** zostanie otwarty Kreator.
3. W **nową witrynę sieci Web** kroku kreatora wprowadź ciąg w **adresu URL** pole do użycia jako unikatowy adres URL aplikacji. Pełny adres URL będzie składać się w tym miejscu wprowadź oraz sufiks, który zostanie wyświetlony obok pola tekstowego. Na ilustracji przedstawiono "ConU", ale prawdopodobnie pochodzi tego adresu URL, więc będzie musiał wybrać inny.

    ![Tworzenie za pomocą łącza bazy danych w portalu zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. W **Region** listę rozwijaną, wybierz region bliską. To ustawienie określa, które centrum danych witryny sieci web jest uruchamiana w.
5. W **bazy danych** listy rozwijanej wybierz **tworzenie bezpłatnej bazy danych SQL o rozmiarze 20 MB**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. W **nazwa parametrów połączenia bazy danych**, wprowadź *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Kliknij strzałkę po prawej stronie u dołu okna. Kreator prowadzi do **ustawienia bazy danych** kroku.
8. W **nazwa** wprowadź *ContosoUniversityDB*.
9. W **serwera** wybierz opcję **nowy serwer SQL Database**. Alternatywnie Jeśli wcześniej utworzonego serwera, można wybrać tego serwera z listy rozwijanej.
10. Wprowadź administrator **nazwa logowania** i **hasło**. W przypadku wybrania **nowy serwer SQL Database** nie wprowadzasz istniejącej nazwy i hasła w tym miejscu, podajesz nową nazwę i hasło, które definiujesz teraz do użycia w przyszłości, gdy uzyskujesz dostęp do bazy danych. Jeśli został wybrany serwer, który został utworzony wcześniej, wprowadzisz poświadczenia dla tego serwera. W tym samouczku nie możesz wybrać ***zaawansowane*** pole wyboru. ***Zaawansowane*** opcje umożliwiają skonfigurowanie bazy danych [sortowania](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Wybierz taką samą **Region** wybranej witryny sieci web.
12. Kliknij znacznik wyboru w prawym dolnym rogu pola, aby wskazać, że jesteś gotowy.   
  
    ![Krok ustawienia bazy danych dla nowej witryny sieci Web — tworzenie za pomocą Kreatora bazy danych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Na poniższej ilustracji przedstawiono korzystanie z istniejącego serwera SQL Server oraz identyfikator logowania.   
  
    ![Krok ustawienia bazy danych dla nowej witryny sieci Web — tworzenie za pomocą Kreatora bazy danych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Portal zarządzania zwróci do strony witryny sieci Web i **stan** kolumna pokazuje, że witryna jest tworzona. Po chwili (zazwyczaj mniej niż minutę) **stan** kolumna pokazuje, czy witryna została pomyślnie utworzona. Na pasku nawigacyjnym po lewej stronie liczby lokacji na swoim koncie możesz mieć pojawia się obok **witryn sieci Web** ikonę i liczby baz danych pojawia się obok **baz danych SQL** ikony.

## <a name="deploy-the-application-to-windows-azure"></a>Wdrażanie aplikacji na platformie Windows Azure

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.  
  
    ![Opublikuj w menu kontekstowego projektu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. W **profilu** karcie **publikowanie w sieci Web** kreatora, kliknij przycisk **importu**.  
  
    ![Importowanie ustawień publikowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Jeśli Twoja subskrypcja Windows Azure nie zostały wcześniej dodane w programie Visual Studio, wykonaj następujące kroki. W ramach tej procedury dodasz subskrypcję tak, aby na liście rozwijanej listy w obszarze **Import z witryny sieci web usługi Windows Azure** będzie zawierać witryny sieci web.

    a. W **Importuj profil publikowania** okno dialogowe, kliknij przycisk **Import z witryny sieci web usługi Windows Azure**, a następnie kliknij przycisk **subskrypcji Dodaj platformy Windows Azure**.

    ![Dodaj subskrypcję usługi Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. W **subskrypcje platformy Azure Windows importu** okno dialogowe, kliknij przycisk **pobierania pliku subskrypcji**.

    ![Pobierz plik subskrypcji](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. W oknie przeglądarki, Zapisz *.publishsettings* pliku.

    ![Pobierz plik .publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Zabezpieczenia — *publishsettings* plik zawiera swoje poświadczenia (Niezakodowane:), które są używane do administrowania subskrypcji Windows Azure i usługi. Najlepszym rozwiązaniem bezpieczeństwa dla tego pliku jest tymczasowo przechowywane poza katalogów źródła (na przykład w *Libraries\Documents* folderu) i usuniesz go po zakończeniu importowania. Złośliwy użytkownik, który uzyskuje dostęp do `.publishsettings` plik można edytować, tworzenia i usuwania usługi Windows Azure.

    d. W **subskrypcje platformy Azure Windows importu** okno dialogowe, kliknij przycisk **Przeglądaj** i przejdź do *.publishsettings* pliku.

    ![Pobierz sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Kliknij przycisk **importu**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. W **Importuj profil publikowania** okno dialogowe, wybierz opcję **Import z witryny sieci web usługi Windows Azure**, wybierz witrynę sieci web z listy rozwijanej, a następnie kliknij **OK**.  
  
    ![Importowanie profilu publikowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. W **połączenia** kliknij pozycję **Waliduj połączenie** aby upewnić się, że ustawienia są poprawne.  
  
    ![Weryfikowanie połączenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Po zweryfikowaniu połączenia, zielony znacznik wyboru jest wyświetlany obok pozycji **Waliduj połączenie** przycisku. Kliknij przycisk **Dalej**.  
  
    ![Pomyślnie zweryfikowane połączenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Otwórz **parametry połączenia zdalnego** listy rozwijanej w obszarze **SchoolContext** i wybierz parametry połączenia dla bazy danych został utworzony.
8. Wybierz **wykonaj migracje Code First (uruchomieniu aplikacji)**.
9. Usuń zaznaczenie pola wyboru **Użyj tych parametrów połączenia w czasie wykonywania** dla **UserContext (DefaultConnection)**, ponieważ ta aplikacja nie używa bazy danych członkostwa.   
  
    ![Karta Ustawienia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Kliknij przycisk **Dalej**.
11. W **Podgląd** kliknij pozycję **Uruchom Podgląd**.  
  
    ![Przycisk StartPreview w karcie podglądu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Karta zawiera listę plików, które zostaną skopiowane na serwer. Wyświetlanie korzystania z wersji zapoznawczej nie jest wymagane, aby opublikować aplikację, ale jest przydatna funkcja wiedzieć. W takim przypadku nie trzeba podejmować żadnych działań, z listy plików, który jest wyświetlany. Przy następnym wdrożeniem tej aplikacji będzie tylko te pliki, które zostały zmienione na tej liście.  
  
    ![Dane wyjściowe pliku StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Kliknij przycisk **publikowania**.  
    Visual Studio rozpoczyna proces kopiowania plików na serwerze usługi Windows Azure.
13. **Dane wyjściowe** okno pokazuje, jakie akcje wdrażania zostały wykonane i raporty pomyślne ukończenie wdrożenia.  
  
    ![Okno danych wyjściowych potwierdzającym pomyślne ukończenie wdrożenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Po pomyślnym wdrożeniu przeglądarka domyślna automatycznie otwiera adres URL witryny sieci web, wdrożone.  
    Teraz działa utworzonej aplikacji w chmurze. Kliknij kartę studentów.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

W tym momencie usługi *SchoolContext* bazy danych została utworzona w Windows Azure SQL Database, ponieważ wybrano **wykonaj migracje Code First (uruchomieniu aplikacji)**. *Web.config* plik w witrynie sieci web wdrożonej został zmieniony tak, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjatora może działać po raz pierwszy kod odczytuje lub zapisuje dane w bazie danych (które wystąpiły w przypadku wybrania **studentów** kartę):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Proces wdrażania jest tworzona w nowe parametry połączenia *(SchoolContext\_DatabasePublish*) dla migracje Code First na potrzeby Aktualizowanie schematu bazy danych i wstępne wypełnianie bazy danych.

![Parametry połączenia Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection* parametry połączenia są bazy danych członkostwa, (które firma Microsoft nie używa w ramach tego samouczka). *SchoolContext* ciąg połączenia jest ContosoUniversity bazy danych.

Można znaleźć wdrożonej wersji pliku Web.config na komputerze w *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dostęp do wdrożonych *Web.config* samego pliku przy użyciu protokołu FTP. Aby uzyskać instrukcje, zobacz [wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Postępuj zgodnie z instrukcjami, które zaczyna się "Aby użyć narzędzia FTP, potrzebne są trzy rzeczy: adres URL protokołu FTP, nazwę użytkownika i hasło."

> [!NOTE]
> Aplikacja sieci web nie implementuje zabezpieczeń, dzięki czemu każda osoba, która znajdzie adres URL można zmieniać dane. Aby uzyskać instrukcje na temat sposobu zabezpieczenie witryny sieci web, zobacz [wdrażanie bezpiecznej aplikacji ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazy danych SQL Database to a Windows Azure Web Site](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Możesz uniemożliwić innym osobom za pomocą witryny przy użyciu portalu zarządzania pakietu Windows Azure lub **Eksploratora serwera** w programie Visual Studio, aby zatrzymać witrynę.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicjatory pierwszy kodu

W sekcji Wdrażanie dostrzegła [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjatora używane. Kod najpierw zapewnia innych inicjatorów, które są dostępne, w tym [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (ustawienie domyślne), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) i [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicjatora może być przydatne w przypadku konfigurowania warunki dla testów jednostkowych. Można także napisać własny inicjatory i można wywołać inicjatora jawnie, jeśli nie chcesz czekać aż aplikacja odczytuje lub zapisuje w bazie danych. Wyczerpujący opis inicjatory, zobacz rozdział 6 książki [programowania programu Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman i Rowan Miller.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia modelu danych i wdrażania podstawowych operacji CRUD, sortowanie, filtrowanie, stronicowanie i grupowanie funkcji. W następnym samouczku rozpocznie się spojrzenie na bardziej zaawansowanych tematów, rozwijając modelu danych.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [dalej](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
