---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Zaawansowane Entity Framework 6 scenariusze dla aplikacji MVC 5 sieci Web (12 12) | Dokumentacja firmy Microsoft
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 85276377671b96e65406639c8584d9ebf8d77ff7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Zaawansowane Entity Framework 6 scenariusze dla aplikacji MVC 5 sieci Web (12 12)
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednich instrukcji zaimplementowano tabeli na hierarchię dziedziczenia. W tym samouczku opisano wprowadzono kilka tematów, które są przydatne pod uwagę przed przejściem do innych niż podstawowe informacje dotyczące projektowania aplikacji sieci web programu ASP.NET korzystających z programu Entity Framework Code First. Instrukcje krok po kroku pomagają użytkownikowi kod i korzystania z programu Visual Studio na następujące tematy:

- [Wykonywanie raw zapytania SQL](#rawsql)
- [Wykonywanie zapytania dotyczące śledzenia nie](#notracking)
- [Badanie SQL wysyłane do bazy danych](#sql)

Samouczek przedstawia kilka tematów z krótkie instrukcje, a następnie łączy do zasobów, aby uzyskać więcej informacji:

- [Repozytorium i jednostki pracy](#repo)
- [Klasy serwera proxy](#proxies)
- [Zmiana automatycznego wykrywania](#changedetection)
- [Automatycznego sprawdzania poprawności](#validation)
- [EF narzędzi dla programu Visual Studio](#tools)
- [Kod źródłowy Entity Framework](#source)

Samouczek zawiera także następujące sekcje:

- [Podsumowanie](#summary)
- [Potwierdzenia](#acknowledgments)
- [Uwagi dotyczące VB](#vb)
- [Typowe błędy i rozwiązania lub obejścia dla nich](#errors)

Większość tych tematów będzie współpracować z stron, które zostały już utworzone. Na potrzeby raw SQL zbiorcze aktualizacje utworzysz nową stronę, która aktualizuje liczba kredytów wszystkich kursów w bazie danych:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Wykonanie kwerendy SQL pierwotnych

Interfejsu API z pierwszego kodu Entity Framework zawiera metody, które umożliwiają przekazywania poleceń SQL bezpośrednio do bazy danych. Do wyboru są następujące opcje:

- Użyj [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) metodę dla zapytań zwracających typów jednostek. Zwracane obiekty muszą mieć typ oczekiwany przez `DbSet` obiektów i ich automatycznie są śledzone przez kontekst bazy danych, chyba że wyłączyć śledzenie. (Zobacz następującą sekcję [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) metody.)
- Użyj [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) metodę dla zapytań zwracających typy, które nie są jednostek. Zwrócone dane nie jest śledzony przez kontekst bazy danych, nawet w przypadku użycia tej metody można pobrać typów jednostek.
- Użyj [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) -query poleceń.

Jedną z zalet funkcji programu Entity Framework jest, że unika wiązanie kodu zbytnio do określonej metody przechowywania danych. Robi to przez generowanie zapytań SQL i poleceń, które zwalnia z konieczności pisania samodzielnie. Ale są wyjątkowych scenariusze, w należy uruchomić określonego zapytania SQL, które zostały utworzone ręcznie, a te metody umożliwiają obsługę tych wyjątków.

Podobnie jak zawsze podczas wykonywania polecenia SQL w aplikacji sieci web, należy wykonać środki ostrożności, aby chronić witrynę przed atakami opartymi na wstrzyknięciu kodu SQL. Jest jednym ze sposobów to zrobić na potrzeby zapytań sparametryzowanych upewnij się, że ciągi przesłane przez stronę sieci web nie można zinterpretować jako polecenia SQL. W tym samouczku użyjesz zapytań sparametryzowanych składników podczas integrowania danych wejściowych użytkownika na kwerendę.

### <a name="calling-a-query-that-returns-entities"></a>Wywoływanie kwerendę, która zwraca jednostki

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) klasa udostępnia metodę, która służy do wykonywania zapytania, które zwraca jednostki typu `TEntity`. Aby zobaczyć, jak to działa użytkownik będzie Zmień kod w `Details` metody `Department` kontrolera.

W *DepartmentController.cs*w `Details` metody, Zastąp `db.Departments.FindAsync` wywołanie metody z `db.Departments.SqlQuery` wywołania metody, jak pokazano w poniższym kodzie wyróżnione:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Aby sprawdzić, czy nowy kod działa poprawnie, wybierz **działów** kartę, a następnie **szczegóły** dla jednego z działów.

![Szczegóły działu](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Wywoływanie kwerendę, która zwraca inne typy obiektów

Wcześniej utworzono siatka uczniów statystyki dla strony informacje wskazujące, liczba studentów dla każdego dnia rejestracji. Kod, który obsługuje to w *HomeController.cs* używa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Załóżmy, że chcesz pisania kodu, który pobiera dane bezpośrednio w SQL, a nie za pomocą LINQ. Aby zrobić, należy uruchomić kwerendę, która zwraca wartość inną niż obiekty obiektów, co oznacza, że należy użyć [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) metody.

W *HomeController.cs*, zastąp instrukcję LINQ w `About` metody za pomocą instrukcji SQL, jak pokazano w poniższym kodzie wyróżnione:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Uruchom strony informacje. Wyświetla dane, które jak poprzednio.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Wywoływanie zapytanie Update

Załóżmy, że chcesz mogą wykonywać zbiorcze zmiany w bazie danych, takich jak zmiana liczby środków dla porach co pracowników firmy Contoso administracyjnych. Jeśli uniwersyteckie ma dużą liczbę kursów, byłoby nieefektywne je odzyskać wszystkie jako jednostek i zmień je pojedynczo. W tej sekcji będziesz zaimplementować stronę sieci web, która umożliwia użytkownikowi określenie czynnikiem, o którą należy zmienić numer środków dla wszystkich kursów i będzie wprowadzić zmiany, wykonując SQL `UPDATE` instrukcji. Strony sieci web będzie wyglądać jak na poniższej ilustracji:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

W *CourseContoller.cs*, Dodaj `UpdateCourseCredits` metody `HttpGet` i `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Gdy kontroler przetworzy `HttpGet` żądania, nic nie zostanie zwrócone w `ViewBag.RowsAffected` zmiennej, a następnie Wyświetl wyświetla puste pole tekstowe i przycisk Prześlij, jak pokazano na powyższej ilustracji.

Gdy **aktualizacji** kliknięciu przycisku `HttpPost` metoda jest wywoływana, i `multiplier` ma wartość wprowadzona w polu tekstowym. Kod wykonywany następnie SQL Server, który aktualizuje kursy i zwraca liczbę wierszy wykorzystywanych do widoku w `ViewBag.RowsAffected` zmiennej. Jeśli widok pobiera wartość, w tym zmiennej, w nim wyświetlane liczba zaktualizowanych zamiast pola tekstowego oraz przycisk, Prześlij, jak pokazano na poniższej ilustracji:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

W *CourseController.cs*, kliknij prawym przyciskiem myszy `UpdateCourseCredits` metod, a następnie kliknij przycisk **Dodaj widok**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

W *Views\Course\UpdateCourseCredits.cshtml*, Zastąp kod szablonu z następującym kodem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Uruchom `UpdateCourseCredits` metody wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL na pasku adresu przeglądarki (na przykład: `http://localhost:50205/Course/UpdateCourseCredits`). Wprowadź liczbę w polu tekstowym:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Kliknij przycisk **aktualizacji**. Zostanie wyświetlony liczba zmodyfikowanych wierszy:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Kliknij przycisk **powrót do listy** Lista kursów poprawione numer środków.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Aby uzyskać więcej informacji o raw zapytania SQL, zobacz [Raw zapytania SQL](https://msdn.microsoft.com/data/jj592907) w witrynie MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Zapytania dotyczące śledzenia nie

Jeśli kontekst bazy danych pobiera wiersze tabeli i tworzy obiekty jednostki, które reprezentują je, domyślnie go przechowuje informacje o czy jednostek w pamięci są zsynchronizowane z nowości w bazie danych. Dane w pamięci działa jako pamięci podręcznej i jest używany podczas aktualizacji jednostki. Często jest wykorzystywana w aplikacji sieci web to buforowanie, ponieważ kontekst wystąpienia są zwykle krótkotrwałą (nowy jest utworzony i usunięty dla każdego żądania) oraz kontekst które odczytuje jednostki zazwyczaj zostanie usunięty, zanim będzie można ponownie użyć tej jednostki.

Śledzenie obiektów jednostek w pamięci można wyłączyć za pomocą [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metody. Następujące typowe scenariusze, w których można to zrobić:

- Zapytanie pobiera takie dużą ilość danych, które wyłączenie śledzenia może znacznie zwiększyć wydajność.
- Aby dołączyć jednostki, aby można było zaktualizować go, ale wcześniej pobrać tej samej jednostki w innym celu. Ponieważ jednostka jest już śledzony przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniony. Jednym ze sposobów obsłużyć taką sytuację jest użycie `AsNoTracking` opcji wcześniejsze zapytanie.

Na przykład, który demonstruje sposób użycia [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metody, zobacz [starszą wersję tego samouczka](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Ta wersja samouczka nie Ustaw flagę zmodyfikowane w jednostce utworzyć integratora modelu w metodzie edycji, więc nie musi `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Badanie SQL wysyłane do bazy danych

Czasami jest przydatne można było wyświetlić rzeczywiste zapytania SQL, które są wysyłane do bazy danych. W starszych samouczku pokazano, jak to zrobić w kodzie interceptora; teraz zostanie wyświetlony bez pisania kodu interceptora niektórych rozwiązań. Aby wypróbować tę możliwość, możesz przyjrzeć się prostego zapytania i przyjrzyj się co się dzieje do niej dodać opcje takie eager ładowania, filtrowanie i sortowanie.

W *kontrolerów/CourseController*, Zastąp `Index` metodę z następującym kodem, aby tymczasowo zatrzymać ładowanie wczesny:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Teraz Ustaw punkt przerwania na `return` instrukcji (F9 kursor znajduje się w tym wierszu). Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania, a następnie wybierz stronę indeksu ciągu. W przypadku kod osiąga punkt przerwania, sprawdzić `sql` zmiennej. Zostanie wyświetlony zapytania, które są wysyłane do programu SQL Server. Jest prostą `Select` instrukcji.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Kliknij przycisk powiększającego klasę, aby zobaczyć zapytania w **narzędzia Text Visualizer**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Teraz dodasz listy rozwijanej do strony indeksu kursy tak, aby filtrować dla danego działu. Kursy będzie sortowania według tytułów i określisz wczesny ładowania dla `Department` właściwości nawigacji.

W *CourseController.cs*, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Przywracanie punktu przerwania `return` instrukcji.

Metoda odbiera wybranej wartości z listy rozwijanej w `SelectedDepartment` parametru. Jeśli nic nie zostanie wybrane, ten parametr będzie równy null.

A `SelectList` kolekcja zawierająca wszystkie działy jest przekazywane do widoku listy rozwijanej. Parametry przekazywane do `SelectList` Konstruktor Określ nazwę pola wartość, nazwy pola tekstowego i wybranego elementu.

Aby uzyskać `Get` metody `Course` repozytorium kodu określa wyrażenie filtru, kolejności sortowania i eager ładowania dla `Department` właściwości nawigacji. Zwraca wyrażenie filtru zawsze `true` Jeśli nic nie zostanie wybrane na liście rozwijanej (to znaczy `SelectedDepartment` ma wartość null).

W *Views\Course\Index.cshtml*, bezpośrednio przed rozpoczęciem `table` tagów, Dodaj następujący kod do tworzenia listy rozwijanej i przycisk przesyłania:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Punkt przerwania nadal ustawiony, uruchom strony indeksu ciągu. Kontynuuj pierwszy godziny trafienia punktu przerwania w kodzie, aby ta strona jest wyświetlana w przeglądarce. Z listy rozwijanej wybierz działu, a następnie kliknij przycisk **filtru**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Teraz będzie pierwszy punkt przerwania dla działów zapytania do listy rozwijanej. Pomiń który i wyświetlić `query` zmiennej przy następnym kod osiąga punkt przerwania, aby sprawdzić, jakie `Course` zapytania teraz wygląda jak. Zostanie wyświetlony ekran podobny do następującego:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Widać, że zapytanie jest teraz `JOIN` zapytania, który ładuje `Department` danych wraz z `Course` danych i że zawiera on `WHERE` klauzuli.

Usuń `var sql = courses.ToString()` wiersza.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repozytorium i jednostki pracy

Wielu deweloperów napisać kod do implementacji repozytorium i jednostki pracy jako otokę kod, który działa z programu Entity Framework. Te wzorce są przeznaczone do tworzenia warstwy abstrakcji między Warstwa dostępu do danych i warstwy logiki biznesowej aplikacji. Implementacja tych wzorców może pomóc zabezpieczyć aplikacji od zmian w magazynie danych i może ułatwić automatyczne testy jednostkowe lub projektowanie oparte na (testach TDD). Jednak zapisywania dodatkowy kod w celu wdrożenia tych wzorców nie zawsze jest najlepszym wyborem aplikacje używające funkcji EF, z kilku powodów:

- Ta sama klasa kontekstu EF powoduje kodu z kodu określonych w przypadku magazynu danych.
- Klasy kontekstu EF może działać jako klasę jednostki pracy dla bazy danych aktualizacje, czy przy użyciu EF.
- Funkcje wprowadzone w programie Entity Framework 6 należy zaimplementować TDD bez pisania kodu repozytorium.

Aby uzyskać więcej informacji dotyczących sposobu wdrażania repozytorium i jednostki pracy, zobacz [wersji programu Entity Framework 5 tego samouczka serii](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Aby uzyskać informacje na temat implementacji TDD w programie Entity Framework 6 zobacz następujące zasoby:

- [Jak EF6 umożliwia Mocking DbSets łatwiej](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testowanie za pomocą mocking framework](https://msdn.microsoft.com/data/dn314429)
- [Testowanie za pomocą własnych symulacyjnych testu](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Klasy serwera proxy

Kiedy programu Entity Framework utworzy wystąpień jednostek (na przykład podczas wykonywania kwerendy), często tworzy je jako wystąpienia typu pochodnego dynamicznie generowanym, który działa jako serwer proxy dla jednostki. Na przykład zobacz następujące dwa obrazy debugera. W pierwszym obrazu, zobacz który `student` zmienna jest oczekiwana `Student` wpisz natychmiast po wystąpienia jednostki. W drugim obraz po EF został użyty do odczytu jednostki uczniów bazy danych, można zobaczyć klasy serwera proxy.

![Przed klasy serwera proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Po klasy serwera proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Ta klasa proxy przesłania niektórych właściwości wirtualnych jednostki do wstawienia punkty zaczepienia dla operacji wykonywanych automatycznie podczas uzyskiwania dostępu do właściwości. Jedna funkcja mechanizm ten jest używany w przypadku jest opóźnionego ładowania.

W większości przypadków nie trzeba znać to korzystanie z serwerów proxy, ale istnieją wyjątki:

- W niektórych scenariuszach można zapobiec tworzenia wystąpień serwera proxy programu Entity Framework. Na przykład gdy w przypadku serializacji jednostek zazwyczaj mają klasy POCO, a nie klasy serwera proxy. Jeden sposób na uniknięcie problemów serializacji jest do serializacji obiektów transfer danych (DTOs) zamiast obiektów jednostek, jak pokazano w [przy użyciu interfejsu API sieci Web z programu Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) samouczka. Innym sposobem jest [wyłączyć tworzenie serwera proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Gdy wystąpienia klasy jednostki przy użyciu `new` operator nie pobieraj wystąpienia serwera proxy. Oznacza to, że nie pobieraj funkcje, takie jak opóźnionego ładowania i automatyczne śledzenie zmian. Jest to zazwyczaj zgoda; Zazwyczaj nie trzeba opóźnionego ładowania, ponieważ tworzysz nowy obiekt, który nie znajduje się w bazie danych i zazwyczaj nie trzeba zmian, jeśli jest jawnie oznaczenie jednostki jako `Added`. Jednak jeśli trzeba opóźnionego ładowania i trzeba śledzenia zmian, możesz utworzyć nowe wystąpienia jednostki z serwerów proxy przy użyciu [Utwórz](https://msdn.microsoft.com/library/gg679504.aspx) metody `DbSet` klasy.
- Możesz pobrać z typ obiektu pośredniczącego rzeczywistego typu jednostki. Można użyć [Element GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) metody `ObjectContext` klasy można pobrać rzeczywistego typu jednostki wystąpienia typu serwera proxy.

Aby uzyskać więcej informacji, zobacz [Praca z serwerów proxy](https://msdn.microsoft.com/data/JJ592886.aspx) w witrynie MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Zmiana automatycznego wykrywania

Entity Framework określa zmian jednostki (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych), porównując bieżące wartości jednostki z oryginalnych wartości. Oryginalne wartości są przechowywane po jednostek jest poddawany kwerendzie lub dołączony. Niektóre metody, które powodują zmiany automatycznego wykrywania są następujące:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Jeśli podczas śledzenia dużą liczbę jednostek i wywołania jednej z tych metod wiele razy w pętlę, można otrzymać znaczną poprawę wydajności wyłączając tymczasowo zmień automatycznego wykrywania przy użyciu [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) właściwości. Aby uzyskać więcej informacji, zobacz [automatyczne wykrywanie zmian](https://msdn.microsoft.com/data/jj556205) w witrynie MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Automatycznego sprawdzania poprawności

Podczas wywoływania `SaveChanges` metody, domyślnie programu Entity Framework sprawdza poprawność danych we właściwościach wszystkich wszystkich jednostek zmienione przed zaktualizowaniem bazy danych. Jeśli użytkownik zaktualizował dużą liczbę jednostek i możesz już upewnieniu się, dane, tej pracy jest niepotrzebne można utworzyć procesu zapisywania zmian zająć mniej czasu tymczasowe wyłączenie sprawdzania poprawności. Możesz zrobić tego za pomocą [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) właściwości. Aby uzyskać więcej informacji, zobacz [weryfikacji](https://msdn.microsoft.com/data/gg193959) w witrynie MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework zaawansowanych narzędzi

[Entity Framework zaawansowanych narzędzi](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) dodatku Visual Studio, który został użyty do tworzenia diagramów modelu danych jest wyświetlany w tych samouczkach. Narzędzia można również wykonać innych funkcji, takich jak Generowanie klas jednostek na podstawie tabeli w istniejącej bazy danych tak, aby można było używać bazy danych z Code First. Po zainstalowaniu narzędzi, niektóre dodatkowe opcje dostępne w menu kontekstowe. Na przykład po kliknięciu prawym przyciskiem myszy klasy kontekstu w **Eksploratora rozwiązań**, Pobierz opcję, aby wygenerować diagramu. Podczas korzystania z Code First nie można zmienić modelu danych w schemacie, ale użytkownik może przenosić elementy w celu ułatwienia zrozumienia.

![EF w menu kontekstowym](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF diagram](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Kod źródłowy Entity Framework

Kod źródłowy Entity Framework 6 są dostępne pod adresem [GitHub](https://github.com/aspnet/EntityFramework6). Zgłoś błędy, a może przyczynić się własnych rozszerzeń do kodu źródłowego EF.

Mimo że kod źródłowy jest otwarty, Entity Framework jest w pełni obsługiwany jako produkt firmy Microsoft. Zespół Microsoft Entity Framework temu formantu, w którym są akceptowane udziały i sprawdza wszystkie zmiany kodu w celu zapewnienia jakości każdej wersji.

<a id="summary"></a>
## <a name="summary"></a>Podsumowanie

Na tym kończy się tej serii samouczków przy użyciu programu Entity Framework w aplikacji platformy ASP.NET MVC. Aby uzyskać więcej informacji na temat pracy z danymi przy użyciu programu Entity Framework, zobacz [EF stronę dokumentacji w witrynie MSDN](https://msdn.microsoft.com/data/ee712907) i [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji na temat wdrażania aplikacji sieci web po jego powstanie zobacz [wdrożenie sieci Web programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-web-deployment-content-map.md) w bibliotece MSDN.

Informacje o innych tematów dotyczących MVC, takie jak uwierzytelniania i autoryzacji, zobacz [ASP.NET MVC — zalecane zasobów](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Potwierdzenia

- Tomasz Dykstra napisane z oryginalną wersją tego samouczka utworzone wspólnie EF 5 aktualizacji i zapisano aktualizacji EF 6. Tomasz jest starszy programowania składnika zapisywania na platformy Microsoft Web i narzędzia Team zawartości.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) czy większość pracy aktualizacji EF 5 i MVC 4 samouczka i utworzone wspólnie EF 6 aktualizacji. Rick jest starszy programowania modułu zapisującego dla Microsoft koncentrujących się na platformie Azure i MVC.
- [Tomaszewski rowan](http://www.romiller.com) i innych członków zespołu programu Entity Framework wspierana z przeglądy kodu i pomogła wiele problemów z migracji, które powstały podczas możemy aktualizowany samouczka EF 5 i EF 6 debugowania.

<a id="vb"></a>
## <a name="vb"></a>VB

Gdy samouczka został opracowany dla EF 4.1, podaliśmy zarówno C# i VB wersji projektu Pobieranie ukończone. Ze względu na ograniczenia czasu i innych priorytetów firma Microsoft nie zostało zrobione który dla tej wersji. Jeśli kompilacji projektu VB te samouczki i będzie chce który udostępniać innym osobom, prosimy o kontakt.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Typowe błędy i rozwiązania lub obejścia dla nich

### <a name="cannot-createshadow-copy"></a>Nie można utworzyć/tle kopiowania

Komunikat o błędzie:

> Nie można utworzyć/tle kopii "&lt;filename&gt;" gdy ten plik już istnieje.


Rozwiązanie

Poczekaj kilka sekund i Odśwież stronę.

### <a name="update-database-not-recognized"></a>Nie rozpoznano Update-Database

Komunikat o błędzie (z `Update-Database` w PMC):

> Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżki został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.


Rozwiązanie

Zamknij program Visual Studio. Ponownie otwórz projekt i spróbuj ponownie.

### <a name="validation-failed"></a>Weryfikacja nie powiodła się

Komunikat o błędzie (z `Update-Database` w PMC):

> Weryfikacja nie powiodła się dla co najmniej jedna jednostka. Zobacz właściwości "EntityValidationErrors", aby uzyskać więcej informacji.


Rozwiązanie

Jedną z przyczyn tego problemu jest błędy sprawdzania poprawności podczas `Seed` metody działa. Zobacz [wstępnego wypełniania i bazami danych debugowanie Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) dla wskazówki dotyczące debugowanie `Seed` metody.

### <a name="http-50019-error"></a>HTTP 500.19 błąd

Komunikat o błędzie:

> Błąd HTTP 500.19 — wewnętrzny błąd serwera  
> Nie można uzyskać dostępu do żądanej strony, ponieważ jej odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.


Rozwiązanie

Jednym ze sposobów tym błędzie można uzyskać jest o wiele kopii rozwiązania, każde z nich przy użyciu tego samego numeru portu. Zamykanie wszystkich wystąpień programu Visual Studio, a następnie ponowne uruchamianie projektu, nad którymi pracuje, mogą zazwyczaj rozwiązać ten problem. Jeśli to nie zadziała, spróbuj zmienić numer portu. Kliknij prawym przyciskiem myszy plik projektu, a następnie kliknij przycisk Właściwości. Wybierz **Web** karcie, a następnie zmień numer portu w **adres Url projektu** pola tekstowego.

### <a name="error-locating-sql-server-instance"></a>Błąd podczas lokalizowania wystąpienia programu SQL Server

Komunikat o błędzie:

> Wystąpił błąd związany z siecią lub wystąpieniem podczas ustanawiania połączenia z programem SQL Server. Serwer nie został znaleziony lub był niedostępny. Sprawdź, czy nazwa wystąpienia jest poprawna i czy programu SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. (Dostawca: interfejsy sieciowe programu SQL, błąd: 26 - błąd podczas lokalizowania określonego serwera/wystąpienia)


Rozwiązanie

Sprawdź parametry połączenia. Jeśli bazy danych został ręcznie usunięty, Zmień nazwę bazy danych w ciągu konstrukcji.

>[!div class="step-by-step"]
[Poprzednie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
