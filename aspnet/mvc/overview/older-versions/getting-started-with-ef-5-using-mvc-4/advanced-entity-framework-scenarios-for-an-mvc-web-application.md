---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Zaawansowane scenariusze platformy Entity Framework dla aplikacji sieci Web MVC (10 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: e4e0a754163ad6b44ca02678fe6a0407e71ec3e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398200"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scenariusze platformy zaawansowane Entity Framework dla aplikacji sieci Web MVC (10 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku wdrożono repozytorium i jednostki pracy. Ten samouczek obejmuje następujące tematy:

- Wykonywanie pierwotne zapytania SQL.
- Wykonywanie zapytania nie śledzenia.
- Badanie zapytań wysyłanych do bazy danych.
- Praca z klasy serwera proxy.
- Wyłączanie automatycznego wykrywania zmian.
- Wyłączenie sprawdzania poprawności podczas zapisywania zmian.
- [Błędy i obejścia](#errors)

Dla większości z tych tematów którą będziesz pracować stron, które zostały już utworzone. Aby zbiorczej aktualizacji przy użyciu surowego SQL należy utworzyć nowej strony, która aktualizuje środków wszystkie kursy w bazie danych:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

I korzystać z zapytania śledzenia nie dodasz nowe logikę walidacji do strony edytowania działu:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Przeprowadzanie pierwotne zapytania SQL

Interfejs API z pierwszego kodu Entity Framework zawiera metody, które umożliwiają przekazywanie poleceń SQL bezpośrednio w bazie danych. Do wyboru są następujące opcje:

- Użyj `DbSet.SqlQuery` metodę dla zapytań zwracających typów jednostek. Zwracanych obiektów musi być typ oczekiwany przez `DbSet` obiektu, dlatego są automatycznie śledzone przez kontekst bazy danych, chyba że wyłączyć śledzenie. (Zobacz poniższą sekcję `AsNoTracking` metody.)
- Użyj `Database.SqlQuery` metodę dla zapytań zwracających typy, które nie są jednostkami. Zwrócone dane nie są śledzone przez kontekst bazy danych, nawet w przypadku używania tej metody można pobrać typów jednostek.
- Użyj [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) poleceń niebędącą zapytaniem.

Jedną z zalet używający narzędzia Entity Framework jest, że takie rozwiązanie pomaga uniknąć wiązanie kodu zbyt ściśle do określonej metody przechowywania danych. Dzieje się tak, generując zapytań SQL i poleceń, która uwalnia użytkownika od konieczności pisania samodzielnie. Ale istnieją scenariusze, wyjątkowe, gdy trzeba uruchomić określonego zapytania SQL, które zostały utworzone ręcznie, a te metody umożliwiają służących do obsługi wyjątków.

Jak jest zawsze wartość true, gdy wykonywanie poleceń SQL w aplikacji sieci web, należy podjąć środki ostrożności, aby chronić witrynę przed atakami polegającymi na iniekcji SQL. Jednym sposobem wykonania tego zadania jest użyć sparametryzowanych zapytań, aby upewnić się, że ciągi przesłane przez stronę sieci web nie może być interpretowany jako polecenia SQL. W tym samouczku użyjesz zapytaniach parametrycznych podczas integrowania danych wejściowych użytkownika na kwerendę.

### <a name="calling-a-query-that-returns-entities"></a>Wywoływanie kwerendę, która zwraca jednostki

Załóżmy, że chcesz `GenericRepository` klasy, aby zapewnić dodatkowe filtrowania i sortowania elastyczności bez konieczności tworzenia klasy pochodnej o dodatkowe metody. Dodaj metodę, która akceptuje zapytanie SQL jest jednym ze sposobów, aby to osiągnąć. Można określić dowolnego rodzaju, filtrowanie lub sortowanie ma w kontrolerze, takich jak `Where` klauzula, która jest zależna od sprzężenia ani podzapytania. W tej sekcji pokazano, jak zaimplementować taką metodę.

Tworzenie `GetWithRawSql` metody, dodając następujący kod, aby *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

W *CourseController.cs*, wywołaj metodę nowe z `Details` metodzie, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

W takim przypadku można wykorzystano `GetByID` korzystania z metody, ale `GetWithRawSql` metodę, aby sprawdzić, czy `GetWithRawSQL` działania metody.

Uruchom strony szczegółów, aby sprawdzić, czy działa zapytanie select (wybierz **kurs** kartę i następnie **szczegóły** do jednego kursu).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Wywoływanie kwerendę, która zwraca inne typy obiektów

Wcześniej utworzono uczniów siatki statystyki dla strony informacje, które wykazało, że liczba studentów każdej daty rejestracji. Kod, który wykonuje to w *HomeController.cs* używa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Załóżmy, że chcesz napisać kod, który pobiera te dane bezpośrednio w SQL, a nie za pomocą LINQ. Aby zrobić, należy uruchomić zapytanie zwracające coś innego niż obiekty jednostki, co oznacza, że należy użyć `Database.SqlQuery` metody.

W *HomeController.cs*, zastąp instrukcję LINQ w `About` metoda następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Uruchom na stronie informacje. Wyświetla te same dane, które wcześniej.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Wywoływanie zapytanie aktualizujące

Załóżmy, że administratorzy Contoso University chcesz móc wykonywać zbiorcze zmiany w bazie danych, takich jak zmienianie liczby środki na korzystanie z każdego kursu. Jeśli uniwersytecie ma dużą liczbę kursów, będzie nieefektywne pobierać je wszystkie jako jednostki, a następnie zmianę ich osobno. W tej sekcji możesz wdrożyć strony sieci web, która umożliwia użytkownikowi określenie współczynnik za pomocą którego można zmienić ilość środków, aby uzyskać wszystkie kursy i wprowadzisz zmiany, wykonując SQL `UPDATE` instrukcji. Strony sieci web będzie wyglądał jak na poniższej ilustracji:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

W poprzednim samouczku użyto ogólnego repozytorium na odczytywanie i aktualizowanie `Course` jednostek w `Course` kontrolera. Dla tej operacji aktualizacji zbiorczej musisz utworzyć nową metodę repozytorium, która nie znajduje się w ogólnego repozytorium. Aby w tym celu należy utworzyć dedykowane `CourseRepository` klasę pochodzącą od `GenericRepository` klasy.

W *DAL* folderze utwórz *CourseRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

W *UnitOfWork.cs*, zmień `Course` typ repozytorium z `GenericRepository<Course>` do `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

W *CourseContoller.cs*, Dodaj `UpdateCourseCredits` metody:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Ta metoda będzie używana dla obu `HttpGet` i `HttpPost`. Gdy `HttpGet` `UpdateCourseCredits` uruchamia metodę `multiplier` zmienna będzie miała wartość null i zostaną wyświetlone puste pole tekstowe i przycisk przesyłania, jak pokazano na poprzedniej ilustracji.

Gdy **aktualizacji** przycisku i `HttpPost` uruchamia metody `multiplier` będzie miał wartość wprowadzona w polu tekstowym. Kod wywołuje następnie repozytorium `UpdateCourseCredits` metody, która zwraca liczbę wpływ na wiersze, a ta wartość jest przechowywany w `ViewBag` obiektu. Kiedy widoku otrzyma liczbę wierszy dotyczy `ViewBag` obiektów, wyświetla tę liczbę, a nie pole tekstowe i przycisk, Prześlij, jak pokazano na poniższej ilustracji:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Utwórz widok w *Views\Course* folderu dla strony aktualizacji kursu środki:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

W *Views\Course\UpdateCourseCredits.cshtml*, Zastąp kod szablonu poniższym kodem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Uruchom `UpdateCourseCredits` metody, wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL w pasku adresu przeglądarki (na przykład: `http://localhost:50205/Course/UpdateCourseCredits`). Wprowadź liczbę w polu tekstowym:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Kliknij przycisk **aktualizacji**. Zobaczysz liczbę uwzględnionych wierszy:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Kliknij przycisk **powrót do listy** Aby wyświetlić listę kursy z poprawione ilość środków.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Aby uzyskać więcej informacji na temat pierwotne zapytania SQL, zobacz [pierwotne zapytania SQL](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) na blogu zespołu programu Entity Framework.

## <a name="no-tracking-queries"></a>Bez śledzenia zapytań

Jeśli kontekst bazy danych pobiera wiersze z bazy danych i tworzy obiekty jednostki, które reprezentują je, domyślnie go przechowuje informacje o czy jednostki w pamięci są zsynchronizowane z tym, co w bazie danych. Dane w pamięci działa jak pamięć podręczna i jest używany podczas aktualizowania jednostki. Często jest wykorzystywana w aplikacji sieci web tej buforowanie, ponieważ kontekst wystąpienia są zwykle krótkotrwałe (nowy jeden jest tworzony i usuwane dla każdego żądania) i obiekt context, odczytuje jednostki zazwyczaj jest usuwane, zanim ponownie używane jest tej jednostki.

Można określić, czy kontekst śledzi obiekty jednostki dla zapytania za pomocą `AsNoTracking` metody. Następujące typowe scenariusze, w których warto to zrobić:

- Zapytanie pobiera dużą woluminu danych, które wyłączenie śledzenia może znacznie zwiększyć wydajność.
- Aby dołączyć jednostkę, aby można było zaktualizować go, ale wcześniej pobrane tej samej jednostki do różnych celów. Ponieważ jednostka jest już śledzony przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniony. Jednym ze sposobów, aby temu zapobiec, jest użycie `AsNoTracking` opcji wcześniejsze zapytanie względem.

W tej sekcji będziesz implementuje logikę biznesową, która przedstawia drugi z tych scenariuszy. W szczególności będzie wymuszać reguły biznesowej, informujący o tym, że pod kierunkiem instruktora nie może być administrator więcej niż jednego działu.

W *DepartmentController.cs*, Dodaj nową metodę, którą można wywołać z `Edit` i `Create` metody, aby upewnić się, że nie dwa działy mają tego samego konta administratora:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Dodaj kod w `try` bloku `HttpPost` `Edit` metodę do wywołania tej nowej metody, jeśli nie ma żadnych błędów sprawdzania poprawności. `try` Blok wygląda teraz następująco:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Uruchom na stronie edycji działu i spróbuj zmienić administratora działu do instruktora, który jest już administratorem innego działu. Zostanie wyświetlony komunikat o oczekiwanym błędem:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Teraz uruchomić ponownie działu edytowanie strony, a zmiana czasu **budżetu** kwoty. Po kliknięciu **Zapisz**, zostanie wyświetlona strona błędu:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Komunikat o błędzie wyjątek "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" to się stało z powodu następująca sekwencja zdarzeń:

- `Edit` Wywołania metody `ValidateOneAdministratorAssignmentPerInstructor` metody, która pobiera wszystkie działy, w których osoby Kim Abercrombie jako administrator. Dzięki któremu angielskiej działu do odczytu. Ponieważ jest to dział edytowanym, nie będzie zgłaszany błąd. W wyniku tego operacja odczytu jednak jednostka angielskiej działu, która została odczytana z bazy danych jest teraz śledzony przez kontekst bazy danych.
- `Edit` Metoda próbuje ustawić `Modified` flagi w języku angielskim jednostki działu utworzone przez integrator modelu MVC, ale się nie powiedzie, ponieważ kontekst jest już śledzony jednostki dla angielskiego działu.

Jedno rozwiązanie tego problemu jest zapobiec śledzenia jednostek w pamięci działu pobierane przez zapytanie weryfikacji kontekstu. Nie ma żadnych wadą, aby to zrobić, ponieważ nie będzie zaktualizować tę jednostkę lub wczytaniem go ponownie w taki sposób, że będą korzystać z niego w pamięci podręcznej jest.

W *DepartmentController.cs*w `ValidateOneAdministratorAssignmentPerInstructor` metody, określić Brak śledzenia, jak pokazano w następującym:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Powtórz próba edycji **budżetu** ilość działu. Tym razem operacja się powiedzie, a lokacji zwraca zgodnie z oczekiwaniami do strony indeksu działów, wyświetla wartość skorygowany budżet.

## <a name="examining-queries-sent-to-the-database"></a>Badanie zapytań wysłanych do bazy danych

Czasami warto będą mogli zobaczyć rzeczywiste zapytania SQL, które są wysyłane do bazy danych. Aby to zrobić, można przejrzeć zmiennej zapytania w debugerze lub wywołanie kwerendy `ToString` metody. Aby wypróbować tę możliwość, możesz przyjrzeć się proste zapytanie i przyjrzyj się co się dzieje do niego dodać opcje takie eager ładowania, filtrowania i sortowania.

W *kontrolerów/CourseController*, Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Teraz Ustaw punkt przerwania *GenericRepository.cs* na `return query.ToList();` i `return orderBy(query).ToList();` instrukcje `Get` metody. Uruchom projekt w trybie debugowania, a następnie wybierz stronę indeksu kursu. Gdy kod osiąga punkt przerwania, sprawdź `query` zmiennej. Zobaczysz zapytanie, które są wysyłane do programu SQL Server. Jest to prosty `Select` instrukcji:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Zapytania mogą być zbyt długa, do wyświetlenia w oknach debugowania w programie Visual Studio. Aby wyświetlić całe zapytanie, możesz skopiować wartość zmiennej i wklej go do edytora tekstu:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Teraz dodasz listy rozwijanej do strony indeksu kurs, aby filtrować dla danego działu. Kursy dostosować sortowanie według tytułu, a następnie wskazać wczesne ładowanie dla `Department` właściwości nawigacji. W *CourseController.cs*, Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Metoda odbiera wybranej wartości z listy rozwijanej w `SelectedDepartment` parametru. Jeśli nic nie jest zaznaczone, ten parametr będzie mieć wartości null.

A `SelectList` kolekcja zawierająca wszystkie działy jest przekazywane do widoku listy rozwijanej. Parametry przekazywane do `SelectList` konstruktora, określ nazwę pola wartość, nazwy pola tekstowe i wybranego elementu.

Aby uzyskać `Get` metody `Course` repozytorium, kod określa wyrażenie filtru, kolejności sortowania i eager ładowania dla `Department` właściwości nawigacji. Wyrażenie filtru zawsze zwraca `true` Jeśli nic nie zostanie wybrane na liście rozwijanej (czyli `SelectedDepartment` ma wartość null).

W *Views\Course\Index.cshtml*, bezpośrednio przed otwierającym `table` tag, Dodaj następujący kod do tworzenia listy rozwijanej i przycisk Prześlij:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Za pomocą punktów przerwania, nadal jest ustawione `GenericRepository` uruchomienia strony indeksu kurs klasy. Wykonaj pierwsze dwa razy, trafienia punktu przerwania w kodzie, aby ta strona jest wyświetlana w przeglądarce. Wybierz dział z listy rozwijanej, a następnie kliknij przycisk **filtru**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Tym razem będzie pierwszy punkt przerwania dla zapytania działy, na liście rozwijanej. Pominąć i wyświetlić `query` zmiennej przy następnym kodu osiągnie punkt przerwania, aby sprawdzić, jakie `Course` zapytania teraz wygląda podobnie. Zostanie wyświetlony podobny do poniższego:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Widać, że zapytanie jest teraz `JOIN` zapytania, który ładuje `Department` danych wraz z `Course` danych i że zawiera on `WHERE` klauzuli.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Praca z klasy serwera Proxy

Gdy platforma Entity Framework tworzy wystąpienia jednostki (np. podczas wykonywania zapytania), często tworzy je jako wystąpienia typu pochodnego dynamicznie generowanym, który działa jako serwer proxy dla jednostki. Ten serwer proxy zastępuje niektóre właściwości wirtualnego jednostka do wstawienia punkty zaczepienia do operacji wykonywanych automatycznie, gdy uzyskano dostęp do właściwości. Na przykład ten mechanizm jest używany do obsługi powolne ładowanie relacji.

W większości przypadków nie trzeba znać to wykorzystania serwerów proxy, ale istnieją wyjątki:

- W niektórych scenariuszach możesz chcieć uniemożliwić tworzenia wystąpień serwera proxy programu Entity Framework. Na przykład serializacji wystąpień-proxy może być bardziej efektywne niż serializacji wystąpień serwera proxy.
- Podczas tworzenia wystąpienia klasy jednostki przy użyciu `new` operatora, nie udostępnia wystąpienia serwera proxy. Oznacza to, że nie udostępnia funkcje, takie jak powolne ładowanie i automatyczne śledzenie zmian. Zazwyczaj jest to OK; Zazwyczaj nie ma potrzeby ładowania z opóźnieniem, ponieważ tworzysz nowy obiekt, który nie znajduje się w bazie danych i zazwyczaj nie trzeba śledzenie zmian, jeśli one jawnie oznaczanie jednostki jako `Added`. Jednak jeśli potrzebujesz powolne ładowanie i potrzebny śledzenia zmian, możesz utworzyć nowe wystąpienia jednostki z serwerami proxy przy użyciu `Create` metody `DbSet` klasy.
- Możesz chcieć uzyskać typ rzeczywistego jednostki z typ serwera proxy. Możesz użyć `GetObjectType` metody `ObjectContext` klasy można pobrać typu rzeczywistego jednostki wystąpienia typu serwera proxy.

Aby uzyskać więcej informacji, zobacz [pracy z serwerami proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) na blogu zespołu programu Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Wyłączenie automatycznego wykrywania zmian

Entity Framework Określa, jak jednostki został zmieniony (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych) przez porównanie bieżącej wartości jednostki przy użyciu oryginalnych wartości. Oryginalne wartości są przechowywane, jednostka zapytania lub został dołączony. Niektóre metody, które powodują wykrywania automatycznego zmian są następujące:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Jeśli prześledzić dużą liczbę jednostek można wywołać jedną z następujących metod wiele razy w pętli, może uzyskać znaczne ulepszenia wydajności, tymczasowo wyłączając przy użyciu wykrywania automatyczna zmiana [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) właściwości. Aby uzyskać więcej informacji, zobacz [automatyczne wykrywanie zmian](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Wyłączenie sprawdzania poprawności podczas zapisywania zmian

Gdy wywołujesz `SaveChanges` metody domyślnie platforma Entity Framework sprawdza poprawność danych wszystkich właściwości wszystkich jednostek zmienione przed zaktualizowaniem bazy danych. Jeśli użytkownik zaktualizował dużą liczbę jednostek i już zweryfikowaniu danych, to działanie jest niepotrzebne wprowadzeniu procesem zapisywania zmian zająć mniej czasu, tymczasowo wyłączając sprawdzania poprawności. Możesz zrobić, przy użyciu [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) właściwości. Aby uzyskać więcej informacji, zobacz [weryfikacji](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Podsumowanie

Na tym kończy się w tej serii samouczków na temat korzystania z programu Entity Framework w aplikacji ASP.NET MVC. Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji o sposobie wdrażania aplikacji sieci web po dołączeniu go, zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521.aspx) w bibliotece MSDN.

Aby uzyskać informacje o innych tematów związanych z MVC, takie jak uwierzytelnianie i autoryzacja, zobacz [zalecane zasoby programu MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Potwierdzenia

- Tom Dykstra napisał oryginalną wersję tego samouczka i jest starszym pisarzem programowania na platformy Microsoft Web i narzędzia do zespołu ds. zawartości.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) wspólnie utworzone w tym samouczku, i czy większość pracy aktualizowania programów EF 5 i MVC 4. Rick jest starszym pisarzem programowania dla firmy Microsoft, koncentrując się na platformie Azure i MVC.
- [Rowan Miller](http://www.romiller.com) i inni członkowie zespołu programu Entity Framework korzystającej z przeglądy kodu i brały udział w debugowaniu wiele problemów z migracji, które powstały podczas, gdy firma Microsoft była aktualizowanie samouczka dla programów EF 5.

## <a name="vb"></a>VB

W przypadku tego samouczka został opracowany, firma Microsoft określany kod C# i VB wersji projektu Pobieranie ukończone. Dzięki tej aktualizacji dostarczamy projektu C# do pobrania dla każdego działu ułatwić wprowadzenie do dowolnego miejsca w tej serii, ale ze względu na ograniczenia czasu i innych priorytetach, firma Microsoft nie została, dla języka VB. Jeśli kompilacja projektu VB przy użyciu tych samouczków i będzie gotowy do udostępnienia, z innymi osobami, Daj nam znać.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Błędy i rozwiązania

### <a name="cannot-createshadow-copy"></a>Nie można utworzyć/w tle kopii

Komunikat o błędzie:

*Nie można utworzyć/w tle kopii "DotNetOpenAuth.OpenId" ten plik już istnieje.*

Rozwiązanie:

Odczekaj kilka sekund, a następnie odśwież stronę.

### <a name="update-database-not-recognized"></a>Update-Database nie został rozpoznany

Komunikat o błędzie:

*Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżka został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.* (Z *`Update-Database`* polecenia w konsoli zarządzania Pakietami.)

Rozwiązanie:

Zamknij program Visual Studio. Ponownie otwórz projekt i spróbuj ponownie.

### <a name="validation-failed"></a>Walidacja nie powiodła się

Komunikat o błędzie:

*Weryfikacja nie powiodła się dla co najmniej jednej jednostki. Zobacz właściwość "EntityValidationErrors", aby uzyskać więcej informacji.* (Z *`Update-Database`* polecenia w konsoli zarządzania Pakietami.)

Rozwiązanie:

Jedną z przyczyn tego problemu jest błędy sprawdzania poprawności podczas `Seed` metoda przebiegów. Zobacz [wstępnego wypełniania i baz danych debugowania Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) porady dotyczące debugowania `Seed` metody.

### <a name="http-50019-error"></a>HTTP 500.19 błąd

Komunikat o błędzie:

*Błąd HTTP 500.19 — wewnętrzny błąd serwera żądana strona nie jest dostępny, ponieważ odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.*

Rozwiązanie:

Jednym ze sposobów ten błąd może wystąpić jest z konieczności wiele kopii rozwiązania każdego z nich przy użyciu tego samego numeru portu. Można zwykle rozwiązać ten problem, kończenie wszystkie wystąpienia programu Visual Studio, a następnie ponowne uruchomienie pracy w projekcie. Jeśli to nie zadziała, spróbuj zmienić numer portu. Kliknij prawym przyciskiem myszy plik projektu, a następnie kliknij polecenie Właściwości. Wybierz **Web** kartę, a następnie zmień numer portu w **adres Url projektu** pola tekstowego.

### <a name="error-locating-sql-server-instance"></a>Błąd podczas lokalizowania wystąpienia programu SQL Server

Komunikat o błędzie:

*Wystąpił błąd związany z siecią lub wystąpieniem podczas nawiązywania połączenia z programem SQL Server. Serwer nie został znaleziony lub jest on niedostępny. Sprawdź, czy nazwa wystąpienia jest prawidłowa i że program SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. (Dostawca: interfejsy sieciowe programu SQL, błąd: 26 — Błąd lokalizowania określonego Server/wystąpienie)*

Rozwiązanie:

Sprawdź parametry połączenia. Jeśli został ręcznie usunięty bazy danych, Zmień nazwę bazy danych w ciągu konstrukcji.

> [!div class="step-by-step"]
> [Poprzednie](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [dalej](building-the-ef5-mvc4-chapter-downloads.md)
