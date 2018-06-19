---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortowanie, filtrowanie i stronicowania Entity Framework w aplikacji platformy ASP.NET MVC (3 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878233"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sortowanie, filtrowanie i stronicowania Entity Framework w aplikacji platformy ASP.NET MVC (3 10)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich instrukcji zaimplementowana zestaw stron sieci web dla podstawowe operacje CRUD na `Student` jednostek. W tym samouczku można dodać sortowanie, filtrowanie i funkcje stronicowania **studentów** strony indeksu. Utworzysz także strona, która jest prosta grupa.

Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu. Nagłówki kolumn są łącza, które użytkownik może kliknąć, aby sortować według tej kolumny. Kliknięcie nagłówka wielokrotnie kolumny przełączanie się między kolumny sortowania.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Dodaj kolumny sortowania łącza do strony indeksu uczniów lub studentów

Aby dodać sortowanie do strony indeksu dla użytkowników domowych, zostanie zmieniona `Index` metody `Student` kontrolera i Dodaj kod, aby `Student` Indeksuj zawartości widoku.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dodawanie funkcji do metody indeksu sortowania

W *Controllers\StudentController.cs*, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL. Wartość ciągu kwerendy są udostępniane przez program ASP.NET MVC jako parametr do metody akcji. Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia, a ciąg "desc", aby określić w kolejności malejącej. Domyślna kolejność sortowania jest rosnąca.

Żądana strona indeksu, po raz pierwszy nie jest Brak ciągu zapytania. Studenci są wyświetlane w rosnącej kolejności przez `LastName`, co jest ustawieniem domyślnym, zgodnie z ustaleniami w przypadku fall-through `switch` instrukcji. Gdy użytkownik klika hiperłącze nagłówek kolumny, odpowiedni `sortOrder` wartość jest podana w ciągu zapytania.

Dwa `ViewBag` zmienne są używane, aby widok było skonfigurowanie hiperłącza nagłówek kolumny z wartości typu QueryString odpowiednie:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Są to trójargumentowy instrukcje. Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub pusty, `ViewBag.NameSortParm` powinien być ustawiony na "nazwa\_desc"; w przeciwnym razie należy ustawić pustego ciągu. Te dwie instrukcje włączyć widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:

| Bieżącej kolejności sortowania | Ostatnia nazwa hiperłącza | Data hiperłącza |
| --- | --- | --- |
| Ostatni nazwy, rosnąco | descending | ascending |
| Ostatni nazwy, malejąco | ascending | ascending |
| Data w kolejności rosnącej | ascending | descending |
| Data, malejąco | ascending | ascending |

W metodzie [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) określić kolumnę sortowania. Kod tworzy [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) zmiennej przed `switch` instrukcji, modyfikuje go w `switch` instrukcji i wywołania `ToList` metody po `switch` instrukcji. Podczas tworzenia i modyfikowania `IQueryable` zmiennych, nie zapytanie jest wysyłane do bazy danych. Kwerenda nie została wykonana, do momentu konwersji `IQueryable` obiektu do kolekcji, wywołując metodę, takich jak `ToList`. W związku z tym powoduje ten kod w jednym zapytaniu, która nie jest wykonywana do czasu `return View` instrukcji.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Dodawanie hiperłącza do widoku indeksu uczniów nagłówek kolumny

W *Views\Student\Index.cshtml*, Zastąp `<tr>` i `<th>` elementy wiersz nagłówka z wyróżniony kod:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Ten kod używa tych informacji w `ViewBag` właściwości do skonfigurowania hiperłącza z zapytaniem odpowiednie ciągu wartości.

Uruchom strony, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** działa nagłówki kolumn, aby zweryfikować, że sortowanie.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po kliknięciu **nazwisko** nagłówek, studentów są wyświetlane w porządku malejącym ostatnia nazwa.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Dodaj pole wyszukiwania do strony indeksu uczniów lub studentów

Aby dodać filtrowanie do strony indeksu studentów, będzie Dodaj pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody. Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dodawanie funkcji filtrowania do Index — metoda

W *Controllers\StudentController.cs*, Zastąp `Index` metodę z następującym kodem (zmiany zostały wyróżnione):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Dodano `searchString` parametr `Index` metody. Również dodane do instrukcji LINQ `where` clausethat wybiera tylko studentów, w których imię lub nazwisko zawiera ciąg wyszukiwania. Wartość ciągu wyszukiwania są odebrane z pola tekstowego, która zostanie dodana do widoku indeksu. Instrukcja, która dodaje [gdzie](https://msdn.microsoft.com/library/bb535040.aspx) klauzuli jest wykonywane tylko wtedy, gdy wartość do wyszukania.

> [!NOTE]
> W wielu przypadkach można wywołać tej samej metody zestaw jednostek Entity Framework lub jako metodę rozszerzenie w kolekcji w pamięci. Wyniki są zazwyczaj takie same, ale w niektórych przypadkach może się różnić. Na przykład, .NET Framework wykonania `Contains` metoda zwraca wszystkie wiersze, gdy przekazać pusty ciąg do niego, ale dostawcy programu Entity Framework dla programu SQL Server Compact 4.0 nie zwraca żadnych wierszy obecność pustych ciągów. W związku z tym kod w przykładzie (umieszczanie `Where` instrukcja wewnątrz `if` instrukcji) zapewnia uzyskać ten sam rezultat dla wszystkich wersji programu SQL Server. Ponadto wdrożenia programu .NET Framework z `Contains` metoda wykonuje porównania z uwzględnieniem wielkości liter domyślnie, ale dostawcy programu Entity Framework SQL Server wykonania porównania bez uwzględniania wielkości liter, domyślnie. W związku z tym wywołaniem `ToUpper` metody, aby jawnie bez uwzględniania wielkości liter testu zapewnia wyników nie należy zmieniać po zmianie kod później do korzystania z repozytorium, która będzie zwracać `IEnumerable` kolekcji zamiast `IQueryable` obiektu. (Podczas wywoływania `Contains` metoda `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; podczas wywoływania go na `IQueryable` obiektu, możesz uzyskać implementacji dostawcy bazy danych.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Dodaj pole wyszukiwania do widoku indeksu dla użytkowników domowych

W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed rozpoczęciem `table` tag, aby można było utworzyć podpis pola tekstowego, a **wyszukiwania** przycisku.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Uruchom strony, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk **wyszukiwania** Aby sprawdzić, czy działa filtrowania.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Należy zauważyć, że adres URL nie zawiera "" ciąg wyszukiwania, co oznacza zakładki tej strony, nie będzie uzyskanie wyfiltrowanej listy używania zakładki. Użytkownik zmieni **wyszukiwania** przycisk, aby użyć ciągów zapytania dla kryteriów filtru później w samouczku.

## <a name="add-paging-to-the-students-index-page"></a>Dodaj stronicowania do strony indeksu uczniów lub studentów

Aby dodać stronicowania do strony indeksu studentów, rozpocznie instalując **PagedList.Mvc** pakietu NuGet. Następnie będzie można wprowadzić dodatkowe zmiany w `Index` — metoda i Dodaj stronicowania łącza do `Index` widoku. **PagedList.Mvc** jest jednym z wielu dobrej stronicowania i sortowania pakietów dla platformy ASP.NET MVC i użyć w tym polu jest przeznaczona tylko na przykład nie jako zalecenie dla niego za pośrednictwem innych opcji. Na poniższej ilustracji przedstawiono łącza stronicowania.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Zainstaluj pakiet PagedList.MVC NuGet

NuGet **PagedList.Mvc** pakiet automatycznie instaluje **PagedList** pakietu jako zależność. **PagedList** pakiet instaluje `PagedList` metody typu i rozszerzenia dla kolekcji `IQueryable` i `IEnumerable` kolekcji. Metody rozszerzenia tworzenia pojedynczej strony danych w `PagedList` kolekcji z Twojej `IQueryable` lub `IEnumerable`i `PagedList` kolekcji zawiera kilka właściwości i metody, które ułatwiają stronicowania. **PagedList.Mvc** pakiet Instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** , a następnie **Zarządzaj pakietami NuGet dla rozwiązania**.

W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **Online** karcie po lewej stronie, a następnie wprowadź "stronicowanej" w polu wyszukiwania. Po wyświetleniu **PagedList.Mvc** pakiet, kliknij przycisk **zainstalować**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

W **Wybierz projekty** kliknij **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Dodawanie funkcji stronicowania do Index — metoda

W *Controllers\StudentController.cs*, Dodaj `using` instrukcji dla `PagedList` przestrzeni nazw:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ten kod dodaje `page` parametru bieżącego parametru kolejność sortowania i bieżącego parametru filtru w podpisie metody, jak pokazano poniżej:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknął stronicowania i sortowania łącza, wszystkie parametry będzie miała wartość null. Po kliknięciu łącza stronicowania `page` zmienna będzie zawierać numer strony do wyświetlenia.

`A ViewBag` Właściwość zapewnia widok z bieżącej kolejności sortowania, ponieważ to musi być uwzględniona w łącza stronicowania aby zapewnić porządek sortowania, taka sama podczas stronicowania:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Inna właściwość `ViewBag.CurrentFilter`, zapewnia widok bieżący ciąg filtru. Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i go muszą być przywrócone do pola tekstowego, gdy strona zostanie wyświetlony ponownie. Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strona musi zresetowana do wartości 1, nowy filtr może powodować różnych danych do wyświetlenia. Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i kliknięciu przycisku Prześlij. W takim przypadku `searchString` parametru nie jest zerowa.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na końcu metody `ToPagedList` metody rozszerzenia dla uczniów lub studentów `IQueryable` obiektu konwertuje zapytań uczniów do pojedynczej strony studentów w typ kolekcji, który obsługuje stronicowanie. Tej stronie studentów są następnie przekazywane do widoku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda przyjmuje numer strony. Reprezentuje dwa znaki zapytania [łączenie null operator](https://msdn.microsoft.com/library/ms173224.aspx). Wartość domyślna dla typu dopuszczającego wartość null; definiuje operator łączenia wartości null wyrażenie `(page ?? 1)` oznacza zwrócić wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.

### <a name="add-paging-links-to-the-student-index-view"></a>Dodawania łączy stronicowania w widoku indeksu dla użytkowników domowych

W *Views\Student\Index.cshtml*, Zastąp istniejący kod następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model` Instrukcji w górnej części strony określa, czy widok teraz pobiera `PagedList` obiekt zamiast `List` obiektu.

`using` Instrukcji dla `PagedList.Mvc` zapewnia dostęp do pomocniczego MVC przycisków stronicowania.

Kod używane jest przeciążenie [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) umożliwiająca, aby określić [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Wartość domyślna [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) przesyła dane formularza przy użyciu metody POST, co oznacza, że parametry są przekazywane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań. Po określeniu HTTP GET, formularza dane są przekazywane w adresie URL jako ciągi zapytania, który umożliwia użytkownikom zakładki adres URL. [W3C wytyczne dotyczące stosowania HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) określić, że GET należy używać w przypadku akcji nie powoduje aktualizacji.

Pole tekstowe jest inicjowany z aktualnie wyszukiwanego ciągu, więc po kliknięciu nowej strony można zobaczyć aktualnie wyszukiwanego ciągu.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Łącza nagłówka kolumny Użyj ciągu zapytania do przekazania do kontrolera aktualnie wyszukiwanego ciągu, dzięki czemu użytkownik może sortować w wynikach filtrowania:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Bieżącej strony i łączna liczba stron wyświetlanych.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Jeśli nie ma żadnych stron do wyświetlenia, wyświetlany jest "Page 0 0". (W takim przypadku numer strony jest większa niż liczba stron ponieważ `Model.PageNumber` 1, a `Model.PageCount` to 0.)

Wyświetlane przyciski stronicowania przez `PagedListPager` pomocy:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Pomocnika udostępnia wiele opcji, które można dostosować, w tym adresy URL i style. Aby uzyskać więcej informacji, zobacz [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.

Uruchom strony.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknij łącza stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania. Następnie wprowadź ciąg wyszukiwania i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie z sortowania i filtrowania.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Utwórz o stronie, które znajdują się dane statystyczne uczniów

Contoso University witryny sieci Web przez strony będzie wyświetlane liczby studentów zostały zarejestrowane dla każdego dnia rejestracji. Wymaga to prosty i grupowania obliczeń grup. W tym celu będzie wykonaj następujące czynności:

- Utwórz klasę modelu widoku danych, która należy do przekazania do widoku.
- Modyfikowanie `About` metoda `Home` kontrolera.
- Modyfikowanie `About` widoku.

### <a name="create-the-view-model"></a>Tworzenie modelu widoku

Utwórz *ViewModels* folderu. W tym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modyfikowanie macierzystego kontrolera

W *HomeController.cs*, Dodaj następujący `using` instrukcje w górnej części pliku:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym klasy:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Zastąp `About` metodę z następującym kodem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Instrukcja LINQ grupy jednostek uczniowie według daty rejestracji oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w kolekcji z `EnrollmentDateGroup` wyświetlić obiekty modelu.

Dodaj `Dispose` metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Zmodyfikuj widok — informacje

Zastąp kod w *Views\Home\About.cshtml* pliku następującym kodem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Uruchom aplikację i kliknij przycisk **o** łącza. Liczba studentów dla każdego dnia rejestracji jest wyświetlane w formie tabeli.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcjonalnie: Wdrażanie aplikacji w systemie Windows Azure

Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić innym użytkownikom za pośrednictwem Internetu, należy wdrożyć ją do usługi hosta sieci web. W tej sekcji opcjonalnej samouczka przedstawiono wdrażania go do witryny sieci Web systemu Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Wdrażanie bazy danych za pomocą migracje Code First

Aby wdrożyć bazy danych będzie wykonaj migracje Code First. Gdy tworzysz profil publikowania, który służy do konfigurowania ustawień wdrażania w programie Visual Studio, należy wybrać pola wyboru o nazwie **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**. To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować aplikację *Web.config* plików na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.

Program Visual Studio nie ma wpływu z bazą danych podczas procesu wdrażania. W przypadku wdrożonej aplikacji uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First automatycznie utworzy bazę danych lub aktualizuje schemat bazy danych do najnowszej wersji. Jeśli aplikacja korzysta migracje `Seed` metody, uruchamia metody po utworzeniu bazy danych lub schemat jest aktualizowany.

Migracji `Seed` metody wstawia dane testowe. Jeśli są wdrażane w środowisku produkcyjnym, należy zmienić `Seed` metodę, tak że tylko wstawia dane, które ma zostać wstawiony do bazy danych produkcyjnych. Na przykład w bieżącym modelu danych możesz mieć rzeczywistych kursów, ale fikcyjnej studentów w projektowej bazie danych. Można napisać `Seed` metodę, aby załadować zarówno w rozwoju, a następnie Oznacz jako komentarz fikcyjnej studentów przed wdrożeniem w środowisku produkcyjnym. Lub może zapisywać `Seed` metodę, aby załadować tylko kursy, a następnie wprowadź fikcyjnej studentów w bazie danych testu ręcznie przy użyciu interfejsu użytkownika aplikacji.

### <a name="get-a-windows-azure-account"></a>Uzyskaj konto systemu Windows Azure

Konieczne będzie konto systemu Windows Azure. Jeśli nie masz jeszcze jeden, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatna wersja próbna systemu Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Tworzenie witryny sieci web i bazy danych SQL w systemie Windows Azure

Witryny sieci Web systemu Windows Azure zostanie uruchomiony w środowisku macierzystym udostępnionego, co oznacza, że jest uruchamiany na maszynach wirtualnych (VM), które są współużytkowane z innymi klientami systemu Windows Azure. Udostępniony środowisko macierzyste to sposób ekonomicznych wprowadzenie w chmurze. Później Jeśli powoduje zwiększenie ruchu w sieci web, aplikację można skalować spełnia potrzeby, uruchamiając na dedykowanych maszynach wirtualnych. Jeśli potrzebujesz bardziej złożoną architekturę, można przeprowadzić migrację do usługi Windows Azure w chmurze. Usługi w chmurze, uruchom na dedykowanych maszynach wirtualnych, które można skonfigurować zgodnie z potrzebami.

Baza danych SQL Azure z systemem Windows jest oparta na chmurze usługą relacyjnych baz danych, który jest oparty na technologii programu SQL Server. Narzędzia i aplikacje, które współpracują z programem SQL Server również współpracować z bazy danych SQL.

1. W [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/), kliknij przycisk **witryn sieci Web** karcie po lewej stronie, a następnie kliknij polecenie **nowy**.

    ![Przycisk Nowy w portalu zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Kliknij przycisk **tworzenie NIESTANDARDOWYCH**.

    ![Utwórz łącze bazy danych w portalu zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **Nowej witryny sieci Web — Utwórz niestandardowy** zostanie otwarty Kreator.
3. W **nowej witryny sieci Web** kroku kreatora, wprowadź ciąg w **adres URL** pole ma być używana jako unikatowy adres URL aplikacji. Pełny adres URL będzie składać się z tutaj wprowadź oraz sufiks, który pojawi się obok pola tekstowego. Na ilustracji przedstawiono "ConU", ale prawdopodobnie pochodzi ten adres URL, dlatego trzeba wybrać inny.

    ![Utwórz łącze bazy danych w portalu zarządzania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. W **Region** listy rozwijanej wybierz region blisko Ciebie. To ustawienie określa centrum danych, które witryny sieci web jest uruchamiana w.
5. W **bazy danych** listy rozwijanej wybierz pozycję **Utwórz bezpłatną bazę danych SQL 20 MB**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. W **Nazwa ciągu połączenia bazy danych**, wprowadź *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Kliknij przycisk Strzałka w prawo w dolnej części pola. Kreator przechodzi do **ustawienia bazy danych** kroku.
8. W **nazwa** wprowadź *ContosoUniversityDB*.
9. W **serwera** wybierz opcję **nowej bazy danych SQL server**. Alternatywnie wcześniej utworzonego serwera, można wybrać tego serwera z listy rozwijanej.
10. Wprowadź administrator **nazwa logowania** i **hasło**. W przypadku wybrania **nowej bazy danych SQL server** nie wprowadzasz istniejącej nazwy i hasła w tym miejscu, podajesz nową nazwę i hasło definiowane w tej chwili do użycia w przyszłości podczas dostępu do bazy danych. W przypadku wybrania wcześniej utworzonego serwera zostanie wprowadź poświadczenia dla tego serwera. W tym samouczku nie wybierz ***zaawansowane*** pole wyboru. ***Zaawansowane*** opcje umożliwiają skonfigurowanie bazy danych [sortowania](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Wybierz taki sam **Region** wybranej witryny sieci web.
12. Kliknij znacznik wyboru w prawej dolnej części pola, aby wskazać, że wszystko jest gotowe.   
  
    ![Krok ustawienia bazy danych dla nowej witryny sieci Web — tworzenie przy użyciu Kreatora bazy danych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Na poniższej ilustracji przedstawiono przy użyciu istniejącego serwera SQL i logowania.   
  
    ![Krok ustawienia bazy danych dla nowej witryny sieci Web — tworzenie przy użyciu Kreatora bazy danych](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Portal zarządzania zwróci do strony witryny sieci Web i **stan** kolumnie jest wyświetlana jest tworzony lokacji. Po chwili (zazwyczaj mniej niż minutę) **stan** kolumnie jest wyświetlana lokacja została pomyślnie utworzona. Na pasku nawigacyjnym po lewej stronie liczby witryn, które masz na koncie jest wyświetlany obok pozycji **witryn sieci Web** ikonę i baz danych jest wyświetlany obok pozycji **baz danych SQL** ikony.

## <a name="deploy-the-application-to-windows-azure"></a>Wdrażanie aplikacji w systemie Windows Azure

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania** z menu kontekstowego.  
  
    ![Opublikuj w menu kontekstowego projektu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. W **profilu** karcie **publikowanie w sieci Web** kreatora, kliknij przycisk **importu**.  
  
    ![Importowanie ustawień publikowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Jeśli nie zostały wcześniej dodane subskrypcji systemu Windows Azure w programie Visual Studio, wykonaj następujące kroki. W tych czynności można dodać subskrypcji listy rozwijanej w obszarze **Importuj z witryny sieci web systemu Windows Azure** uwzględni witryny sieci web.

    a. W **importowania profilu publikowania** okno dialogowe, kliknij przycisk **Importuj z witryny sieci web systemu Windows Azure**, a następnie kliknij przycisk **subskrypcji platformy Microsoft Azure Dodaj**.

    ![Dodawanie subskrypcji systemu Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. W **subskrypcji Import systemu Windows Azure** okno dialogowe, kliknij przycisk **pobierania pliku subskrypcji**.

    ![Pobierz plik subskrypcji](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. W oknie przeglądarki, Zapisz *.publishsettings* pliku.

    ![Pobierz plik .publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Zabezpieczenia — *publishsettings* plik zawiera swoje poświadczenia (Niezakodowane), które są używane do administrowania subskrypcji systemu Windows Azure i usługi. Najlepszym rozwiązaniem bezpieczeństwa dla tego pliku jest tymczasowo przechowywane poza katalogów źródła (na przykład w *Libraries\Documents* folderu), a następnie usuń, po zakończeniu importowania. Złośliwy użytkownik, który uzyskuje dostęp do `.publishsettings` plik można edytować, tworzenie i usuwanie usług systemu Windows Azure.

    d. W **subskrypcji Import systemu Windows Azure** okno dialogowe, kliknij przycisk **Przeglądaj** i przejdź do *.publishsettings* pliku.

    ![Pobierz sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Kliknij przycisk **importu**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. W **importowania profilu publikowania** okno dialogowe, wybierz opcję **Importuj z witryny sieci web systemu Windows Azure**, wybierz witrynę sieci web z listy rozwijanej, a następnie kliknij przycisk **OK**.  
  
    ![Importowanie profilu publikowania](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. W **połączenia** , kliknij pozycję **sprawdzania poprawności połączenia** aby upewnić się, że ustawienia są poprawne.  
  
    ![Weryfikacja połączenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Po sprawdzeniu poprawności połączenia zielony znacznik wyboru jest wyświetlany obok pozycji **sprawdzania poprawności połączenia** przycisku. Kliknij przycisk **Dalej**.  
  
    ![Pomyślnie zweryfikowane połączenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Otwórz **ciąg połączenia zdalnego** listy rozwijanej w obszarze **SchoolContext** i wybierz parametry połączenia dla bazy danych został utworzony.
8. Wybierz **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**.
9. Usuń zaznaczenie pola wyboru **Użyj tych parametrów połączenia w czasie wykonywania** dla **UserContext (parametru DefaultConnection)**, ponieważ ta aplikacja nie używa bazy danych członkostwa.   
  
    ![Karta Ustawienia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Kliknij przycisk **Dalej**.
11. W **Podgląd** , kliknij pozycję **Uruchom Podgląd**.  
  
    ![Przycisk StartPreview w karcie podglądu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Karcie zostanie wyświetlona lista plików, które zostaną skopiowane na serwer. Wyświetlanie podglądu nie jest wymagane, aby opublikować aplikację, ale jest to funkcja przydatna pod uwagę. W takim przypadku nie trzeba wykonywać żadnych czynności z listy plików, który jest wyświetlany. Przy następnym wdrażania tej aplikacji będzie tylko te pliki, które zostały zmienione na tej liście.  
  
    ![Dane wyjściowe pliku StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Kliknij przycisk **publikowania**.  
    Visual Studio rozpoczyna proces kopiowania plików na serwerze systemu Windows Azure.
13. **Dane wyjściowe** okna pokazuje, jakie akcje wdrażania zostały pobrane i raporty pomyślnego wdrożenia.  
  
    ![Okno danych wyjściowych raportowania pomyślnego wdrożenia](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Po pomyślnym wdrożeniu przeglądarka domyślna automatycznie otwiera adres URL wdrożonej witryny sieci web.  
    Utworzoną aplikację jest teraz uruchomiona w chmurze. Kliknij kartę studenta.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

W tym momencie z *SchoolContext* utworzono bazę danych w bazie danych SQL Azure z systemem Windows, ponieważ wybrano **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**. *Web.config* plik wdrożonej witryny sieci web został zmieniony, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjatora może działać po raz pierwszy kod odczytuje i zapisuje dane w bazie danych (który się to zdarzyć w przypadku wybrania **studentów** kartę):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Proces wdrażania również utworzyć nowe parametry połączenia *(SchoolContext\_DatabasePublish*) dla migracje Code First na potrzeby aktualizacji schematu bazy danych i wstępne wypełnianie bazy danych.

![Parametry połączenia Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*Połączenia DefaultConnection* ciągu połączenia jest przeznaczony dla bazy danych członkostwa (w którym firma Microsoft nie jest używana w tym samouczku). *SchoolContext* parametry połączenia są ContosoUniversity bazy danych.

Można znaleźć wdrożoną wersję pliku Web.config na komputerze w *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dostęp do wdrożonych *Web.config* samego pliku przy użyciu protokołu FTP. Aby uzyskać instrukcje, zobacz [wdrożenia sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Postępuj zgodnie z instrukcjami rozpoczynających się od "Aby użyć narzędzia FTP, niezbędne są trzy elementy: adres URL FTP, nazwę użytkownika i hasło."

> [!NOTE]
> Aplikacja sieci web nie zawiera implementacji zabezpieczeń, więc każda osoba, która znajdzie adres URL Zmień dane. Aby uzyskać instrukcje na temat sposobu zabezpieczenie witryny sieci web, zobacz [wdrażanie aplikacji bezpiecznego ASP.NET MVC z członkostwa, OAuth i bazy danych SQL do witryny sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Można zapobiec użyciu lokacji za pomocą portalu zarządzania pakietu Windows Azure przez inne osoby lub **Eksploratora serwera** w programie Visual Studio można zatrzymać witryny.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicjatory pierwszy kodu

W sekcji wdrożenia widać [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjatora używane. Kod najpierw zawiera również inne inicjatory, które są dostępne, w tym [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (ustawienie domyślne), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) i [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicjatora może być przydatne w przypadku konfigurowania warunki dla testów jednostkowych. Można również napisać własny inicjatory i można wywołać inicjatora jawnie, jeśli nie chcesz czekać, aż do aplikacji odczytuje z lub zapisuje w bazie danych. Opis kompleksowe inicjatorów, zobacz rozdział 6 książki [programowania Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman i Tomaszewski Rowan.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia modelu danych i implementować basic CRUD, sortowanie, filtrowanie, stronicowania i funkcja grupowania. W następnym samouczku zostaną patrzeć bardziej zaawansowanych tematów, rozwijając modelu danych.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [dalej](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
