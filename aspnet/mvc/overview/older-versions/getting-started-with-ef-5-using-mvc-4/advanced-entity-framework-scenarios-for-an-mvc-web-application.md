---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Zaawansowanych scenariuszy struktury jednostek dla aplikacji sieci Web MVC (10 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d58a745896b29317c1d1049e3bf1a5ec2e628820
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Zaawansowane Entity Framework scenariusze dla aplikacji sieci Web MVC (10 10)
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich instrukcji zaimplementowana repozytorium i jednostki pracy. Ten samouczek obejmuje następujące tematy:

- Wykonywanie raw zapytania SQL.
- Wykonywanie zapytania dotyczące śledzenia nie.
- Badanie zapytań wysłanych do bazy danych.
- Praca z klasy serwera proxy.
- Wyłączenie automatycznego wykrywania zmian.
- Wyłączenie sprawdzania poprawności podczas zapisywania zmian.
- [Błędy i obejścia](#errors)

Większość tych tematów będzie współpracować z stron, które zostały już utworzone. Na potrzeby raw SQL zbiorcze aktualizacje utworzysz nową stronę, która aktualizuje liczba kredytów wszystkich kursów w bazie danych:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

I używać kwerendy śledzenia nie dodasz nowe logikę weryfikacji do działu edycji strony:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Wykonanie kwerendy SQL pierwotnych

Interfejsu API z pierwszego kodu Entity Framework zawiera metody, które umożliwiają przekazywania poleceń SQL bezpośrednio do bazy danych. Do wyboru są następujące opcje:

- Użyj `DbSet.SqlQuery` metodę dla zapytań zwracających typów jednostek. Zwracane obiekty muszą mieć typ oczekiwany przez `DbSet` obiektów i ich automatycznie są śledzone przez kontekst bazy danych, chyba że wyłączyć śledzenie. (Zobacz następującą sekcję `AsNoTracking` metody.)
- Użyj `Database.SqlQuery` metodę dla zapytań zwracających typy, które nie są jednostek. Zwrócone dane nie jest śledzony przez kontekst bazy danych, nawet w przypadku użycia tej metody można pobrać typów jednostek.
- Użyj [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456(v=vs.103).aspx) -query poleceń.

Jedną z zalet funkcji programu Entity Framework jest, że unika wiązanie kodu zbytnio do określonej metody przechowywania danych. Robi to przez generowanie zapytań SQL i poleceń, które zwalnia z konieczności pisania samodzielnie. Ale są wyjątkowych scenariusze, w należy uruchomić określonego zapytania SQL, które zostały utworzone ręcznie, a te metody umożliwiają obsługę tych wyjątków.

Podobnie jak zawsze podczas wykonywania polecenia SQL w aplikacji sieci web, należy wykonać środki ostrożności, aby chronić witrynę przed atakami opartymi na wstrzyknięciu kodu SQL. Jest jednym ze sposobów to zrobić na potrzeby zapytań sparametryzowanych upewnij się, że ciągi przesłane przez stronę sieci web nie można zinterpretować jako polecenia SQL. W tym samouczku użyjesz zapytań sparametryzowanych składników podczas integrowania danych wejściowych użytkownika na kwerendę.

### <a name="calling-a-query-that-returns-entities"></a>Wywoływanie kwerendę, która zwraca jednostki

Załóżmy, że chcesz `GenericRepository` klasy zapewnienie dodatkowych filtrowanie i sortowanie elastyczność bez konieczności tworzenia klasy pochodnej z dodatkowych metod. Jednym ze sposobów osiągnięcia, który jest dodanie metody, która akceptuje zapytanie SQL. Można określić dowolny filtrowania lub sortowania można mają w kontrolerze, takich jak `Where` klauzuli, która jest zależna od sprzężenia ani podzapytań. W tej sekcji zobaczysz implementowania taka metoda.

Utwórz `GetWithRawSql` metody, dodając następujący kod, aby *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

W *CourseController.cs*, wywołaj metodę nowego z `Details` metody, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

W takim przypadku można użyto `GetByID` korzystania z metody, ale `GetWithRawSql` metody do sprawdzenia, czy `GetWithRawSQL` działania metody.

Uruchom na stronie Szczegóły, aby sprawdzić, czy działa kwerenda wyboru (wybierz **kursu** kartę, a następnie **szczegóły** dla porach jeden).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Wywoływanie kwerendę, która zwraca inne typy obiektów

Wcześniej utworzono siatka uczniów statystyki dla strony informacje wskazujące, liczba studentów dla każdego dnia rejestracji. Kod, który obsługuje to w *HomeController.cs* używa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Załóżmy, że chcesz pisania kodu, który pobiera dane bezpośrednio w SQL, a nie za pomocą LINQ. Aby zrobić, należy uruchomić kwerendę, która zwraca wartość inną niż obiekty obiektów, co oznacza, że należy użyć `Database.SqlQuery` metody.

W *HomeController.cs*, zastąp instrukcję LINQ w `About` metodę z następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Uruchom strony informacje. Wyświetla dane, które jak poprzednio.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Wywoływanie zapytanie Update

Załóżmy, że chcesz mogą wykonywać zbiorcze zmiany w bazie danych, takich jak zmiana liczby środków dla porach co pracowników firmy Contoso administracyjnych. Jeśli uniwersyteckie ma dużą liczbę kursów, byłoby nieefektywne je odzyskać wszystkie jako jednostek i zmień je pojedynczo. W tej sekcji będziesz zaimplementować stronę sieci web, który umożliwia użytkownikowi określenie czynnikiem, o którą należy zmienić numer środków dla wszystkich kursów i będzie wprowadzić zmiany, wykonując SQL `UPDATE` instrukcji. Strony sieci web będzie wyglądać jak na poniższej ilustracji:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

W poprzednich instrukcji używane ogólnego repozytorium na odczytywanie i aktualizowanie `Course` jednostek `Course` kontrolera. Dla tej operacji aktualizacji zbiorczej należy utworzyć nową metodę repozytorium, który nie znajduje się w repozytorium ogólnego. Aby to zrobić, należy utworzyć dedykowana `CourseRepository` klasą pochodzącą z `GenericRepository` klasy.

W *DAL* folderu, Utwórz *CourseRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

W *UnitOfWork.cs*, zmień `Course` typu repozytorium z `GenericRepository<Course>` do`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

W *CourseContoller.cs*, Dodaj `UpdateCourseCredits` metody:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Ta metoda będzie używana zarówno dla `HttpGet` i `HttpPost`. Gdy `HttpGet` `UpdateCourseCredits` uruchamia — metoda `multiplier` zmienna będzie mieć wartość null i zostaną wyświetlone puste pole tekstowe i przycisk Prześlij, jak pokazano na powyższej ilustracji.

Gdy **aktualizacji** przycisku i `HttpPost` uruchamia — metoda `multiplier` będzie mieć wartość wprowadzona w polu tekstowym. Kod wywołuje repozytorium `UpdateCourseCredits` spowodowała metodę, która zwraca liczbę i jest przechowywana w `ViewBag` obiektu. Gdy widok odbiera liczbę wierszy wykorzystywanych w `ViewBag` obiektów, wyświetla ten numer zamiast pola tekstowego, a przycisk, Prześlij, jak pokazano na poniższej ilustracji:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Tworzenie widoku w *Views\Course* folderu dla strony środków kursu aktualizacji:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

W *Views\Course\UpdateCourseCredits.cshtml*, Zastąp kod szablonu z następującym kodem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Uruchom `UpdateCourseCredits` metody wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL na pasku adresu przeglądarki (na przykład: `http://localhost:50205/Course/UpdateCourseCredits`). Wprowadź liczbę w polu tekstowym:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Kliknij przycisk **aktualizacji**. Zostanie wyświetlony liczba zmodyfikowanych wierszy:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Kliknij przycisk **powrót do listy** Lista kursów poprawione numer środków.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Aby uzyskać więcej informacji o raw zapytania SQL, zobacz [Raw zapytania SQL](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) na blogu zespołu programu Entity Framework.

## <a name="no-tracking-queries"></a>Zapytania dotyczące śledzenia nie

Jeśli kontekst bazy danych pobiera wiersze z bazy danych i tworzy obiekty jednostki, które reprezentują je, domyślnie go przechowuje informacje o czy jednostek w pamięci są zsynchronizowane z nowości w bazie danych. Dane w pamięci działa jako pamięci podręcznej i jest używany podczas aktualizacji jednostki. Często jest wykorzystywana w aplikacji sieci web to buforowanie, ponieważ kontekst wystąpienia są zwykle krótkotrwałą (nowy jest utworzony i usunięty dla każdego żądania) oraz kontekst które odczytuje jednostki zazwyczaj zostanie usunięty, zanim będzie można ponownie użyć tej jednostki.

Można określić, czy kontekst śledzi obiektów jednostek dla zapytania przy użyciu `AsNoTracking` metody. Następujące typowe scenariusze, w których można to zrobić:

- Zapytanie pobiera takie dużą ilość danych, które wyłączenie śledzenia może znacznie zwiększyć wydajność.
- Aby dołączyć jednostki, aby można było zaktualizować go, ale wcześniej pobrać tej samej jednostki w innym celu. Ponieważ jednostka jest już śledzony przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniony. Jednym ze sposobów temu zapobiec, jest użycie `AsNoTracking` opcji wcześniejsze zapytanie.

W tej sekcji będziesz wdrożyć logikę biznesową, która ilustruje drugi z tych scenariuszy. W szczególności będzie wymuszać reguły biznesowej z informacją, że instruktora nie może być administratorem działu więcej niż jeden.

W *DepartmentController.cs*, Dodaj nową metodę można wywołać z `Edit` i `Create` metody, aby upewnić się, że nie dwa działów mają tego samego konta administratora:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Dodaj kod w `try` zablokować z `HttpPost` `Edit` metodę do wywołania tej nowej metody, jeśli nie ma żadnych błędów sprawdzania poprawności. `try` Bloku teraz wygląda następująco:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Uruchom działu Edytuj stronę i spróbuj zmienić administratora działu do instruktora, który jest już administrator innego działu. Komunikat oczekiwany błąd:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Teraz uruchomić ponownie działu edytowanie strony, a zmiana czasu **budżetu** wielkość. Po kliknięciu **zapisać**, zobacz stronę błędu:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Komunikat o błędzie wyjątku "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" to się to zdarzyć z powodu następująca sekwencja zdarzeń:

- `Edit` Wywołania metody `ValidateOneAdministratorAssignmentPerInstructor` metodę, która pobiera wszystkie działy, które mają osoby Kim Abercrombie jako administrator z działu. Która powoduje angielskiej wersji językowej działu do odczytu. Ponieważ jest to działu edytowany, nie będzie zgłaszany błąd. W wyniku tej operacji odczytu jednak jednostki angielskiej wersji językowej działu, która została odczytana z bazy danych jest teraz śledzona przez kontekst bazy danych.
- `Edit` Metoda próbuje ustawić `Modified` Flaga w języku angielskim jednostki działu utworzony przez obiekt wiążący modelu MVC, ale się nie powiedzie, ponieważ kontekst śledzi już jednostkę działu angielskiej wersji językowej.

Jedno rozwiązanie tego problemu jest zapobiec śledzenia jednostek w pamięci działu pobierane przez zapytanie weryfikacji kontekstu. Nie ma żadnych wadą temu, ponieważ nie będzie aktualizację tej jednostki lub odczytywania go ponownie w sposób, który będzie z niego korzystać w pamięci podręcznej w pamięci.

W *DepartmentController.cs*w `ValidateOneAdministratorAssignmentPerInstructor` metody, określ Brak śledzenia, jak pokazano poniżej:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Powtórz próba edycji **budżetu** ilość działu. Tym razem operacja się powiodła i lokacji zwraca zgodnie z oczekiwaniami do strony indeksu działów, pokazujący wartość skorygowana.

## <a name="examining-queries-sent-to-the-database"></a>Badanie zapytań wysłanych do bazy danych

Czasami jest przydatne można było wyświetlić rzeczywiste zapytania SQL, które są wysyłane do bazy danych. Aby to zrobić, można przejrzeć zmienną zapytania w debugerze lub wywołać kwerendy `ToString` metody. Aby wypróbować tę możliwość, możesz przyjrzeć się prostego zapytania i przyjrzyj się co się dzieje do niej dodać opcje takie eager ładowania, filtrowanie i sortowanie.

W *kontrolerów/CourseController*, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Teraz Ustaw punkt przerwania *GenericRepository.cs* na `return query.ToList();` i `return orderBy(query).ToList();` instrukcje `Get` metody. Uruchom projekt w trybie debugowania, a następnie wybierz stronę indeksu ciągu. W przypadku kod osiąga punkt przerwania, sprawdzić `query` zmiennej. Zostanie wyświetlony zapytania, które są wysyłane do programu SQL Server. Jest prostą `Select` instrukcji:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Zapytań może być zbyt długi, aby wyświetlić w oknach debugowania w programie Visual Studio. Aby wyświetlić całą zapytania, możesz skopiować wartość zmiennej i wklej go w edytorze tekstu:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Teraz dodasz listy rozwijanej do strony indeksu kursu tak, aby filtrować dla danego działu. Kursy będzie sortowania według tytułów i określisz wczesny ładowania dla `Department` właściwości nawigacji. W *CourseController.cs*, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Metoda odbiera wybranej wartości z listy rozwijanej w `SelectedDepartment` parametru. Jeśli nic nie zostanie wybrane, ten parametr będzie równy null.

A `SelectList` kolekcja zawierająca wszystkie działy jest przekazywane do widoku listy rozwijanej. Parametry przekazywane do `SelectList` Konstruktor Określ nazwę pola wartość, nazwy pola tekstowego i wybranego elementu.

Aby uzyskać `Get` metody `Course` repozytorium kodu określa wyrażenie filtru, kolejności sortowania i eager ładowania dla `Department` właściwości nawigacji. Zwraca wyrażenie filtru zawsze `true` Jeśli nic nie zostanie wybrane na liście rozwijanej (to znaczy `SelectedDepartment` ma wartość null).

W *Views\Course\Index.cshtml*, bezpośrednio przed rozpoczęciem `table` tagów, Dodaj następujący kod do tworzenia listy rozwijanej i przycisk przesyłania:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Punkty nadal jest ustawione `GenericRepository` uruchomienia strony indeksu kursu klasy. Kontynuuj pierwsze dwa razy, trafienia punktu przerwania w kodzie, aby ta strona jest wyświetlana w przeglądarce. Z listy rozwijanej wybierz działu, a następnie kliknij przycisk **filtru**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Teraz będzie pierwszy punkt przerwania dla działów zapytania do listy rozwijanej. Pomiń który i wyświetlić `query` zmiennej przy następnym kod osiąga punkt przerwania, aby sprawdzić, jakie `Course` zapytania teraz wygląda jak. Zostanie wyświetlony ekran podobny do następującego:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Widać, że zapytanie jest teraz `JOIN` zapytania, który ładuje `Department` danych wraz z `Course` danych i że zawiera on `WHERE` klauzuli.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Praca z klasy serwera Proxy

Kiedy programu Entity Framework utworzy wystąpień jednostek (na przykład podczas wykonywania kwerendy), często tworzy je jako wystąpienia typu pochodnego dynamicznie generowanym, który działa jako serwer proxy dla jednostki. Ten serwer proxy przesłania niektórych właściwości wirtualnych jednostki do wstawienia punkty zaczepienia dla operacji wykonywanych automatycznie podczas uzyskiwania dostępu do właściwości. Na przykład ten mechanizm jest używana do obsługi opóźnionego ładowania relacji.

W większości przypadków nie trzeba znać to korzystanie z serwerów proxy, ale istnieją wyjątki:

- W niektórych scenariuszach można zapobiec tworzenia wystąpień serwera proxy programu Entity Framework. Na przykład serializacji wystąpień z systemem innym niż serwer proxy może być skuteczniejsza niż serializacji wystąpień serwera proxy.
- Gdy wystąpienia klasy jednostki przy użyciu `new` operator nie pobieraj wystąpienia serwera proxy. Oznacza to, że nie pobieraj funkcje, takie jak opóźnionego ładowania i automatyczne śledzenie zmian. Jest to zazwyczaj zgoda; Zazwyczaj nie trzeba opóźnionego ładowania, ponieważ tworzysz nowy obiekt, który nie znajduje się w bazie danych i zazwyczaj nie trzeba zmian, jeśli jest jawnie oznaczenie jednostki jako `Added`. Jednak jeśli trzeba opóźnionego ładowania i trzeba śledzenia zmian, możesz utworzyć nowe wystąpienia jednostki z serwerów proxy przy użyciu `Create` metody `DbSet` klasy.
- Możesz pobrać z typ obiektu pośredniczącego rzeczywistego typu jednostki. Można użyć `GetObjectType` metody `ObjectContext` klasy można pobrać rzeczywistego typu jednostki wystąpienia typu serwera proxy.

Aby uzyskać więcej informacji, zobacz [Praca z serwerów proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) na blogu zespołu programu Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Wyłączenie automatycznego wykrywania zmian

Entity Framework określa zmian jednostki (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych), porównując bieżące wartości jednostki z oryginalnych wartości. Oryginalne wartości są przechowywane usunięcie jednostki zbadać lub został dołączony. Niektóre metody, które powodują zmiany automatycznego wykrywania są następujące:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Jeśli podczas śledzenia dużą liczbę jednostek i wywołania jednej z tych metod wiele razy w pętlę, można otrzymać znaczną poprawę wydajności wyłączając tymczasowo zmień automatycznego wykrywania przy użyciu [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) właściwości. Aby uzyskać więcej informacji, zobacz [automatyczne wykrywanie zmian](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Wyłączenie sprawdzania poprawności podczas zapisywania zmian

Podczas wywoływania `SaveChanges` metody, domyślnie programu Entity Framework sprawdza poprawność danych we właściwościach wszystkich wszystkich jednostek zmienione przed zaktualizowaniem bazy danych. Jeśli użytkownik zaktualizował dużą liczbę jednostek i możesz już upewnieniu się, dane, tej pracy jest niepotrzebne można utworzyć procesu zapisywania zmian zająć mniej czasu tymczasowe wyłączenie sprawdzania poprawności. Możesz zrobić tego za pomocą [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) właściwości. Aby uzyskać więcej informacji, zobacz [weryfikacji](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Podsumowanie

Na tym kończy się tej serii samouczków przy użyciu programu Entity Framework w aplikacji platformy ASP.NET MVC. Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji na temat wdrażania aplikacji sieci web po jego powstanie zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/en-us/library/bb386521.aspx) w bibliotece MSDN.

Informacje o innych tematów dotyczących MVC, takie jak uwierzytelniania i autoryzacji, zobacz [zasoby programu MVC zalecane](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Potwierdzenia

- Tomasz Dykstra napisane z oryginalną wersją tego samouczka i jest starszy programowania składnika zapisywania na platformy Microsoft Web i narzędzia Team zawartości.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) wspólnie utworzone w tym samouczku i czy większość pracy aktualizacją EF 5 i MVC 4. Rick jest starszy programowania modułu zapisującego dla Microsoft koncentrujących się na platformie Azure i MVC.
- [Tomaszewski rowan](http://www.romiller.com) i innych członków zespołu programu Entity Framework wspierana z przeglądy kodu i pomogła debugowania wiele problemów z migracji, które powstały podczas możemy aktualizowany samouczka EF 5.

## <a name="vb"></a>VB

Gdy samouczka został opracowany, podaliśmy zarówno C# i VB wersji projektu Pobieranie ukończone. Dzięki tej aktualizacji zapewniamy projektu C# do pobrania dla każdego działu ułatwiające rozpoczęcie pracy dowolne miejsce w serii, ale z powodu ograniczenia czasu i innych priorytetów, firma Microsoft nie została który dla VB. Jeśli kompilacji projektu VB te samouczki i będzie chce który udostępniać innym osobom, prosimy o kontakt.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Błędy i rozwiązania

### <a name="cannot-createshadow-copy"></a>Nie można utworzyć/tle kopiowania

Komunikat o błędzie:

*Nie można utworzyć/tle kopii "DotNetOpenAuth.OpenId" Jeśli ten plik już istnieje.*

Rozwiązanie:

Poczekaj kilka sekund i Odśwież stronę.

### <a name="update-database-not-recognized"></a>Nie rozpoznano Update-Database

Komunikat o błędzie:

*Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżki został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.* (Z  *`Update-Database`*  w PMC.)

Rozwiązanie:

Zamknij program Visual Studio. Ponownie otwórz projekt i spróbuj ponownie.

### <a name="validation-failed"></a>Weryfikacja nie powiodła się

Komunikat o błędzie:

*Weryfikacja nie powiodła się dla co najmniej jedna jednostka. Zobacz właściwości "EntityValidationErrors", aby uzyskać więcej informacji.* (Z  *`Update-Database`*  w PMC.)

Rozwiązanie:

Jedną z przyczyn tego problemu jest błędy sprawdzania poprawności podczas `Seed` metody działa. Zobacz [wstępnego wypełniania i bazami danych debugowanie Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) dla wskazówki dotyczące debugowanie `Seed` metody.

### <a name="http-50019-error"></a>HTTP 500.19 błąd

Komunikat o błędzie:

*Nie można uzyskać dostępu do błąd HTTP 500.19 — wewnętrzny błąd serwera żądanej strony, ponieważ jej odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.*

Rozwiązanie:

Jednym ze sposobów tym błędzie można uzyskać jest o wiele kopii rozwiązania, każde z nich przy użyciu tego samego numeru portu. Zazwyczaj można rozwiązać ten problem, zamykanie wszystkich wystąpień programu Visual Studio, a następnie ponowne uruchomienie pracy w projekcie. Jeśli to nie zadziała, spróbuj zmienić numer portu. Kliknij prawym przyciskiem myszy plik projektu, a następnie kliknij przycisk Właściwości. Wybierz **Web** karcie, a następnie zmień numer portu w **adres Url projektu** pola tekstowego.

### <a name="error-locating-sql-server-instance"></a>Błąd podczas lokalizowania wystąpienia programu SQL Server

Komunikat o błędzie:

*Wystąpił błąd związany z siecią lub wystąpieniem podczas ustanawiania połączenia z programem SQL Server. Serwer nie został znaleziony lub był niedostępny. Sprawdź, czy nazwa wystąpienia jest poprawna i czy programu SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. (Dostawca: interfejsy sieciowe programu SQL, błąd: 26 - błąd podczas lokalizowania określonego serwera/wystąpienia)*

Rozwiązanie:

Sprawdź parametry połączenia. Jeśli bazy danych został ręcznie usunięty, Zmień nazwę bazy danych w ciągu konstrukcji.

>[!div class="step-by-step"]
[Poprzednie](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[dalej](building-the-ef5-mvc4-chapter-downloads.md)
