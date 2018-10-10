---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Zaawansowane scenariusze platformy Entity Framework 6 dla aplikacji MVC 5 sieci Web (12, 12) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: riande
ms.date: 12/08/2014
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 0aa440e700c9bfb02aa5d55ebf481850a730febe
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912686"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Scenariusze platformy zaawansowane Entity Framework 6 dla aplikacji MVC 5 sieci Web (12, 12)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

W poprzednim samouczku wdrożono Tabela wg hierarchii dziedziczenia. Ten samouczek obejmuje wprowadza kilka tematów, które są przydatne pod uwagę podczas czegoś podstawy tworzenia aplikacji sieci web ASP.NET, które używają programu Entity Framework Code First. Instrukcje krok po kroku prowadzą użytkownika przez kod i za pomocą programu Visual Studio dla następujących tematach:

- [Pierwotne zapytania SQL](#rawsql)
- [Wykonywanie zapytania nie śledzenia](#notracking)
- [Badanie SQL wysyłane do bazy danych](#sql)

Samouczek przedstawia kilka tematów z krótkie instrukcje i linki do zasobów, aby uzyskać więcej informacji:

- [Repozytorium i jednostki pracy](#repo)
- [Klasy serwera proxy](#proxies)
- [Automatyczna zmiana wykrywania](#changedetection)
- [Automatyczna Walidacja](#validation)
- [EF narzędzia dla programu Visual Studio](#tools)
- [Kodu źródłowego programu Entity Framework](#source)

Samouczek zawiera również następujące sekcje:

- [Podsumowanie](#summary)
- [Potwierdzenia](#acknowledgments)
- [Uwaga dotycząca języka VB](#vb)
- [Typowe błędy i rozwiązania lub obejścia dla nich](#errors)

Dla większości z tych tematów którą będziesz pracować stron, które zostały już utworzone. Aby zbiorczej aktualizacji przy użyciu surowego SQL należy utworzyć nowej strony, która aktualizuje środków wszystkie kursy w bazie danych:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Przeprowadzanie pierwotne zapytania SQL

Interfejs API z pierwszego kodu Entity Framework zawiera metody, które umożliwiają przekazywanie poleceń SQL bezpośrednio w bazie danych. Do wyboru są następujące opcje:

- Użyj [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) metodę dla zapytań zwracających typów jednostek. Zwracanych obiektów musi być typ oczekiwany przez `DbSet` obiektu, dlatego są automatycznie śledzone przez kontekst bazy danych, chyba że wyłączyć śledzenie. (Zobacz poniższą sekcję [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) metody.)
- Użyj [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) metodę dla zapytań zwracających typy, które nie są jednostkami. Zwrócone dane nie są śledzone przez kontekst bazy danych, nawet w przypadku używania tej metody można pobrać typów jednostek.
- Użyj [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) poleceń niebędącą zapytaniem.

Jedną z zalet używający narzędzia Entity Framework jest, że takie rozwiązanie pomaga uniknąć wiązanie kodu zbyt ściśle do określonej metody przechowywania danych. Dzieje się tak, generując zapytań SQL i poleceń, która uwalnia użytkownika od konieczności pisania samodzielnie. Ale istnieją scenariusze, wyjątkowe, gdy trzeba uruchomić określonego zapytania SQL, które zostały utworzone ręcznie, a te metody umożliwiają służących do obsługi wyjątków.

Jak jest zawsze wartość true, gdy wykonywanie poleceń SQL w aplikacji sieci web, należy podjąć środki ostrożności, aby chronić witrynę przed atakami polegającymi na iniekcji SQL. Jednym sposobem wykonania tego zadania jest użyć sparametryzowanych zapytań, aby upewnić się, że ciągi przesłane przez stronę sieci web nie może być interpretowany jako polecenia SQL. W tym samouczku użyjesz zapytaniach parametrycznych podczas integrowania danych wejściowych użytkownika na kwerendę.

### <a name="calling-a-query-that-returns-entities"></a>Wywoływanie kwerendę, która zwraca jednostki

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) klasa dostarcza metody, która służy do wykonywania zapytania, które zwraca jednostki typu `TEntity`. Aby zobaczyć, jak to działa możesz poznasz, jak zmienić kod w `Details` metody `Department` kontrolera.

W *DepartmentController.cs*w `Details` metody, Zastąp `db.Departments.FindAsync` wywołanie metody z `db.Departments.SqlQuery` wywołania metody, jak pokazano na następujący wyróżniony kod:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Aby sprawdzić, czy nowy kod działa poprawnie, należy wybrać **działów** kartę i następnie **szczegóły** dla jednego z działów.

![Szczegóły działu](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Wywoływanie kwerendę, która zwraca inne typy obiektów

Wcześniej utworzono uczniów siatki statystyki dla strony informacje, które wykazało, że liczba studentów każdej daty rejestracji. Kod, który wykonuje to w *HomeController.cs* używa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Załóżmy, że chcesz napisać kod, który pobiera te dane bezpośrednio w SQL, a nie za pomocą LINQ. Aby zrobić, należy uruchomić zapytanie zwracające coś innego niż obiekty jednostki, co oznacza, że należy użyć [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) metody.

W *HomeController.cs*, zastąp instrukcję LINQ w `About` metody za pomocą instrukcji SQL, jak pokazano na następujący wyróżniony kod:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Uruchom na stronie informacje. Wyświetla te same dane, które wcześniej.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Wywoływanie zapytanie aktualizujące

Załóżmy, że administratorzy Contoso University chcesz móc wykonywać zbiorcze zmiany w bazie danych, takich jak zmienianie liczby środki na korzystanie z każdego kursu. Jeśli uniwersytecie ma dużą liczbę kursów, będzie nieefektywne pobierać je wszystkie jako jednostki, a następnie zmianę ich osobno. W tej sekcji możesz wdrożyć strony sieci web, która umożliwia użytkownikowi określenie współczynnik za pomocą którego można zmienić ilość środków, aby uzyskać wszystkie kursy i wprowadzisz zmiany, wykonując SQL `UPDATE` instrukcji. Strony sieci web będzie wyglądał jak na poniższej ilustracji:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

W *CourseContoller.cs*, Dodaj `UpdateCourseCredits` metody `HttpGet` i `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Gdy kontroler przetworzy `HttpGet` żądania, nic nie zostanie zwrócone w `ViewBag.RowsAffected` zmiennej, a następnie Wyświetl wyświetla puste pole tekstowe i przycisk przesyłania, jak pokazano na poprzedniej ilustracji.

Gdy **aktualizacji** kliknięto przycisk `HttpPost` metoda jest wywoływana, i `multiplier` został wprowadzony w polu tekstowym. Kod wykonuje następnie SQL Server, który aktualizuje kursów i zwraca liczbę wierszy dotyczy do widoku `ViewBag.RowsAffected` zmiennej. Jeśli widok pobiera wartość, w tym zmiennych, jest Wyświetla liczbę zaktualizowanych rekordów zamiast pole tekstowe i przycisk, Prześlij, jak pokazano na poniższej ilustracji:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

W *CourseController.cs*, kliknij prawym przyciskiem myszy jeden z `UpdateCourseCredits` metody, a następnie kliknij **Dodaj widok**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

W *Views\Course\UpdateCourseCredits.cshtml*, Zastąp kod szablonu poniższym kodem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Uruchom `UpdateCourseCredits` metody, wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL w pasku adresu przeglądarki (na przykład: `http://localhost:50205/Course/UpdateCourseCredits`). Wprowadź liczbę w polu tekstowym:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Kliknij przycisk **aktualizacji**. Zobaczysz liczbę uwzględnionych wierszy:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Kliknij przycisk **powrót do listy** Aby wyświetlić listę kursy z poprawione ilość środków.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Aby uzyskać więcej informacji na temat pierwotne zapytania SQL, zobacz [pierwotne zapytania SQL](https://msdn.microsoft.com/data/jj592907) w witrynie MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Bez śledzenia zapytań

Jeśli kontekst bazy danych pobiera wiersze z tabeli, tworzy obiekty jednostki, reprezentujących ich domyślnie go przechowuje informacje o czy jednostki w pamięci są zsynchronizowane z tym, co w bazie danych. Dane w pamięci działa jak pamięć podręczna i jest używany podczas aktualizowania jednostki. Często jest wykorzystywana w aplikacji sieci web tej buforowanie, ponieważ kontekst wystąpienia są zwykle krótkotrwałe (nowy jeden jest tworzony i usuwane dla każdego żądania) i obiekt context, odczytuje jednostki zazwyczaj jest usuwane, zanim ponownie używane jest tej jednostki.

Śledzenie obiektów jednostek w pamięci można wyłączyć za pomocą [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metody. Następujące typowe scenariusze, w których warto to zrobić:

- Zapytanie pobiera dużą woluminu danych, które wyłączenie śledzenia może znacznie zwiększyć wydajność.
- Aby dołączyć jednostkę, aby można było zaktualizować go, ale wcześniej pobrane tej samej jednostki do różnych celów. Ponieważ jednostka jest już śledzony przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniony. Jednym ze sposobów, aby obsłużyć taką sytuację, jest użycie `AsNoTracking` opcji wcześniejsze zapytanie względem.

Aby uzyskać przykład, który demonstruje sposób używania [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metody, zobacz [starszą wersję tego samouczka](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Ta wersja tego samouczka nie zostały ustawione flagi zmodyfikowane na jednostce utworzyć obiekt wiążący modelu w metodzie edycji, więc nie potrzebuje `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Badanie SQL wysyłane do bazy danych

Czasami warto będą mogli zobaczyć rzeczywiste zapytania SQL, które są wysyłane do bazy danych. W starszych samouczku pokazano, jak to zrobić w kodzie interceptor; teraz zobaczysz niektóre sposoby zrobić to bez konieczności pisania kodu interceptor. Aby wypróbować tę możliwość, możesz przyjrzeć się proste zapytanie i przyjrzyj się co się dzieje do niego dodać opcje takie eager ładowania, filtrowania i sortowania.

W *kontrolerów/CourseController*, Zastąp `Index` metody za pomocą następującego kodu, aby tymczasowo zatrzymać wczesne ładowanie:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Teraz Ustaw punkt przerwania na `return` — instrukcja (F9 kursor znajduje się w danym wierszu). Naciśnij klawisz **F5** Aby uruchomić projekt w trybie debugowania, a następnie wybierz stronę indeksu kursu. Gdy kod osiąga punkt przerwania, sprawdź `sql` zmiennej. Zobaczysz zapytanie, które są wysyłane do programu SQL Server. Jest to prosty `Select` instrukcji.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Kliknij przycisk lupy, aby wyświetlić zapytanie w **Wizualizator tekstu**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Teraz dodasz listy rozwijanej do strony indeksu kursów, aby filtrować dla danego działu. Kursy dostosować sortowanie według tytułu, a następnie wskazać wczesne ładowanie dla `Department` właściwości nawigacji.

W *CourseController.cs*, Zastąp `Index` metoda następującym kodem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Przywróć punkt przerwania na `return` instrukcji.

Metoda odbiera wybranej wartości z listy rozwijanej w `SelectedDepartment` parametru. Jeśli nic nie jest zaznaczone, ten parametr będzie mieć wartości null.

A `SelectList` kolekcja zawierająca wszystkie działy jest przekazywane do widoku listy rozwijanej. Parametry przekazywane do `SelectList` konstruktora, określ nazwę pola wartość, nazwy pola tekstowe i wybranego elementu.

Aby uzyskać `Get` metody `Course` repozytorium, kod określa wyrażenie filtru, kolejności sortowania i eager ładowania dla `Department` właściwości nawigacji. Wyrażenie filtru zawsze zwraca `true` Jeśli nic nie zostanie wybrane na liście rozwijanej (czyli `SelectedDepartment` ma wartość null).

W *Views\Course\Index.cshtml*, bezpośrednio przed otwierającym `table` tag, Dodaj następujący kod do tworzenia listy rozwijanej i przycisk Prześlij:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Punkt przerwania nadal ustawiony, Uruchom stronę indeksu kursu. Wykonaj pierwszy razy, trafienia punktu przerwania w kodzie, aby ta strona jest wyświetlana w przeglądarce. Wybierz dział z listy rozwijanej, a następnie kliknij przycisk **filtru**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Tym razem będzie pierwszy punkt przerwania dla zapytania działy, na liście rozwijanej. Pominąć i wyświetlić `query` zmiennej przy następnym kodu osiągnie punkt przerwania, aby sprawdzić, jakie `Course` zapytania teraz wygląda podobnie. Zostanie wyświetlony podobny do poniższego:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Widać, że zapytanie jest teraz `JOIN` zapytania, który ładuje `Department` danych wraz z `Course` danych i że zawiera on `WHERE` klauzuli.

Usuń `var sql = courses.ToString()` wiersza.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repozytorium i jednostki pracy

Wielu programistów napisać kod, aby zaimplementować repozytorium i jednostki pracy jako otoka wokół kodu, który współdziała z platformą Entity Framework. Te wzorce są przeznaczone do tworzenia warstwę abstrakcji od warstwy dostępu do danych i warstwy logiki biznesowej aplikacji. Implementacji tych wzorców może pomóc ochronić aplikację przed zmianami w magazynie danych i może ułatwić automatyczne testy jednostkowe i Programowanie oparte na testach (TDD). Jednak tworzenia dodatkowego kodu, aby zaimplementować te wzorce nie zawsze jest najlepszym wyborem, aplikacje używające funkcji EF, z kilku powodów:

- Samej klasy kontekstu EF powoduje, że kod w kodzie dotyczące magazynu danych.
- Klasy kontekstu EF może działać jako klasę jednostki pracy dla bazy danych aktualizacje, czy przy użyciu programu EF.
- Funkcje wprowadzone w programie Entity Framework 6 należy zaimplementować TDD bez konieczności pisania kodu w repozytorium.

Aby uzyskać więcej informacji na temat implementacji repozytorium i jednostki pracy, zobacz [Entity Framework 5 wersję tej serii samouczków](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Aby uzyskać informacje na temat zaimplementować projektowanie oparte na testach na platformie Entity Framework 6 zobacz następujące zasoby:

- [Jak EF6 umożliwia DbSets pozorowanie łatwiej](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testowanie za pomocą pozorowania framework](https://msdn.microsoft.com/data/dn314429)
- [Testowanie za pomocą własnych testów, wartości podwójnej precyzji](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Klasy serwera proxy

Gdy platforma Entity Framework tworzy wystąpienia jednostki (np. podczas wykonywania zapytania), często tworzy je jako wystąpienia typu pochodnego dynamicznie generowanym, który działa jako serwer proxy dla jednostki. Na przykład zobacz następujące dwa obrazy debugera. Na pierwszej ilustracji zobaczysz, że `student` zmienna jest oczekiwana `Student` wpisz natychmiast po zakończeniu tworzenia wystąpienia jednostki. W drugi obraz po EF został użyty do odczytu jednostki dla uczniów z bazy danych, zobaczysz klasy proxy.

![Przed klasy serwera proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Po klasy serwera proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Tej klasy proxy zastępuje niektóre właściwości wirtualnego jednostka do wstawienia punkty zaczepienia do operacji wykonywanych automatycznie, gdy uzyskano dostęp do właściwości. Jedna funkcja, ten mechanizm jest używana dla jest powolne ładowanie.

W większości przypadków nie trzeba znać to wykorzystania serwerów proxy, ale istnieją wyjątki:

- W niektórych scenariuszach możesz chcieć uniemożliwić tworzenia wystąpień serwera proxy programu Entity Framework. Na przykład gdy są serializacji jednostek zazwyczaj chcesz klas POCO, a nie klasy serwera proxy. Jednym ze sposobów, aby uniknąć problemów z serializacji jest do wykonywania serializacji obiektów transferu danych (dto) zamiast obiektów jednostek, jak pokazano w [przy użyciu interfejsu API sieci Web za pomocą platformy Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) samouczka. Innym sposobem jest [Wyłącz tworzenie serwera proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Podczas tworzenia wystąpienia klasy jednostki przy użyciu `new` operatora, nie udostępnia wystąpienia serwera proxy. Oznacza to, że nie udostępnia funkcje, takie jak powolne ładowanie i automatyczne śledzenie zmian. Zazwyczaj jest to OK; Zazwyczaj nie ma potrzeby ładowania z opóźnieniem, ponieważ tworzysz nowy obiekt, który nie znajduje się w bazie danych i zazwyczaj nie trzeba śledzenie zmian, jeśli one jawnie oznaczanie jednostki jako `Added`. Jednak jeśli potrzebujesz powolne ładowanie i potrzebny śledzenia zmian, możesz utworzyć nowe wystąpienia jednostki z serwerami proxy przy użyciu [Utwórz](https://msdn.microsoft.com/library/gg679504.aspx) metody `DbSet` klasy.
- Możesz chcieć uzyskać typ rzeczywistego jednostki z typ serwera proxy. Możesz użyć [Element GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) metody `ObjectContext` klasy można pobrać typu rzeczywistego jednostki wystąpienia typu serwera proxy.

Aby uzyskać więcej informacji, zobacz [pracy z serwerami proxy](https://msdn.microsoft.com/data/JJ592886.aspx) w witrynie MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Automatyczna zmiana wykrywania

Entity Framework Określa, jak jednostki został zmieniony (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych) przez porównanie bieżącej wartości jednostki przy użyciu oryginalnych wartości. Oryginalne wartości są przechowywane, gdy jednostka jest badane lub dołączone. Niektóre metody, które powodują wykrywania automatycznego zmian są następujące:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Jeśli prześledzić dużą liczbę jednostek można wywołać jedną z następujących metod wiele razy w pętli, może uzyskać znaczne ulepszenia wydajności, tymczasowo wyłączając przy użyciu wykrywania automatyczna zmiana [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) właściwości. Aby uzyskać więcej informacji, zobacz [automatyczne wykrywanie zmian](https://msdn.microsoft.com/data/jj556205) w witrynie MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Automatyczna Walidacja

Gdy wywołujesz `SaveChanges` metody domyślnie platforma Entity Framework sprawdza poprawność danych wszystkich właściwości wszystkich jednostek zmienione przed zaktualizowaniem bazy danych. Jeśli użytkownik zaktualizował dużą liczbę jednostek i już zweryfikowaniu danych, to działanie jest niepotrzebne wprowadzeniu procesem zapisywania zmian zająć mniej czasu, tymczasowo wyłączając sprawdzania poprawności. Możesz zrobić, przy użyciu [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) właściwości. Aby uzyskać więcej informacji, zobacz [weryfikacji](https://msdn.microsoft.com/data/gg193959) w witrynie MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) dodatku programu Visual Studio, który został użyty do tworzenia diagramów model danych jest wyświetlany w tych samouczkach. Narzędzia można również wykonać innych funkcji, takich jak Generowanie klas jednostek na podstawie tabel w istniejącej bazy danych tak, aby można było używać bazy danych za pomocą Code First. Po zainstalowaniu narzędzia, dodatkowe opcje są wyświetlane w menu kontekstowego. Na przykład po kliknięciu prawym przyciskiem myszy klasy kontekstu w **Eksploratora rozwiązań**, otrzymasz opcję, aby wygenerować diagram. Podczas korzystania z Code First, nie można zmienić modelu danych na diagramie, ale można przenosić elementy w celu ułatwienia zrozumienia.

![EF w menu kontekstowym](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF diagram](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Kodu źródłowego programu Entity Framework

Kod źródłowy platformy Entity Framework 6 znajduje się w temacie [GitHub](https://github.com/aspnet/EntityFramework6). Mogą zgłaszać usterki i może przyczynić się własne rozszerzenia do kodu źródłowego programu EF.

Mimo że kod źródłowy jest otwarty, platformy Entity Framework jest w pełni obsługiwany jest produktem firmy Microsoft. Zespół Microsoft Entity Framework zachowuje kontroli nad tym, którzy są akceptowane wkładu i testy wszystkich zmian w kodzie w celu zapewnienia jakości każdej wersji.

<a id="summary"></a>
## <a name="summary"></a>Podsumowanie

Na tym kończy się w tej serii samouczków na temat korzystania z programu Entity Framework w aplikacji ASP.NET MVC. Aby uzyskać więcej informacji na temat sposobu pracy z danymi przy użyciu platformy Entity Framework, zobacz [EF stronę dokumentacji w witrynie MSDN](https://msdn.microsoft.com/data/ee712907) i [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

Aby uzyskać więcej informacji o sposobie wdrażania aplikacji sieci web po dołączeniu go, zobacz [wdrażanie aplikacji internetowych ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-web-deployment-content-map.md) w bibliotece MSDN.

Aby uzyskać informacje o innych tematów związanych z MVC, takie jak uwierzytelnianie i autoryzacja, zobacz [MVC ASP.NET — zalecane zasoby](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Potwierdzenia

- Tom Dykstra napisał oryginalną wersję tego samouczka, wspólnie utworzone aktualizacja programów EF 5 i napisał aktualizacja programów EF 6. Tom jest starszym pisarzem programowania na platformy Microsoft Web i narzędzia do zespołu ds. zawartości.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) czy większość pracy aktualizowanie samouczek programu EF 5 i MVC 4 i współautorem aktualizacja programów EF 6. Rick jest starszym pisarzem programowania dla firmy Microsoft, koncentrując się na platformie Azure i MVC.
- [Rowan Miller](http://www.romiller.com) i inni członkowie zespołu programu Entity Framework korzystającej z przeglądy kodu i brały udział w debugowaniu wiele problemów z migracji, które powstały podczas, gdy firma Microsoft zostały aktualizowanie samouczek programu EF 5 i programów EF 6.

<a id="vb"></a>
## <a name="vb"></a>VB

W przypadku tego samouczka został opracowany dla platformy EF 4.1, zamieszczone kod C# i VB wersji projektu Pobieranie ukończone. Ze względu na ograniczenia czasu i innych priorytetach nie zrobić dla tej wersji. Jeśli kompilacja projektu VB przy użyciu tych samouczków i będzie gotowy do udostępnienia, z innymi osobami, Daj nam znać.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Typowe błędy i rozwiązania lub obejścia dla nich

### <a name="cannot-createshadow-copy"></a>Nie można utworzyć/w tle kopii

Komunikat o błędzie:

> Nie można utworzyć/w tle kopii "&lt;filename&gt;" gdy ten plik już istnieje.

Rozwiązanie

Odczekaj kilka sekund, a następnie odśwież stronę.

### <a name="update-database-not-recognized"></a>Update-Database nie został rozpoznany

Komunikat o błędzie (z `Update-Database` polecenia w konsoli zarządzania Pakietami):

> Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżka został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.

Rozwiązanie

Zamknij program Visual Studio. Ponownie otwórz projekt i spróbuj ponownie.

### <a name="validation-failed"></a>Walidacja nie powiodła się

Komunikat o błędzie (z `Update-Database` polecenia w konsoli zarządzania Pakietami):

> Weryfikacja nie powiodła się dla co najmniej jednej jednostki. Zobacz właściwość "EntityValidationErrors", aby uzyskać więcej informacji.

Rozwiązanie

Jedną z przyczyn tego problemu jest błędy sprawdzania poprawności podczas `Seed` metoda przebiegów. Zobacz [wstępnego wypełniania i baz danych debugowania Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) porady dotyczące debugowania `Seed` metody.

### <a name="http-50019-error"></a>HTTP 500.19 błąd

Komunikat o błędzie:

> Błąd HTTP 500.19 — wewnętrzny błąd serwera żądana strona nie jest dostępny, ponieważ odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.

Rozwiązanie

Jednym ze sposobów ten błąd może wystąpić jest z konieczności wiele kopii rozwiązania każdego z nich przy użyciu tego samego numeru portu. Zamykanie wszystkich wystąpień programu Visual Studio, a następnie ponownie uruchomić projekt, nad którymi pracuje można zwykle rozwiązać ten problem. Jeśli to nie zadziała, spróbuj zmienić numer portu. Kliknij prawym przyciskiem myszy plik projektu, a następnie kliknij polecenie Właściwości. Wybierz **Web** kartę, a następnie zmień numer portu w **adres Url projektu** pola tekstowego.

### <a name="error-locating-sql-server-instance"></a>Błąd podczas lokalizowania wystąpienia programu SQL Server

Komunikat o błędzie:

> Wystąpił błąd związany z siecią lub wystąpieniem podczas nawiązywania połączenia z programem SQL Server. Serwer nie został znaleziony lub jest on niedostępny. Sprawdź, czy nazwa wystąpienia jest prawidłowa i że program SQL Server jest skonfigurowany do zezwalania na połączenia zdalne. (Dostawca: interfejsy sieciowe programu SQL, błąd: 26 — Błąd lokalizowania określonego Server/wystąpienie)

Rozwiązanie

Sprawdź parametry połączenia. Jeśli został ręcznie usunięty bazy danych, Zmień nazwę bazy danych w ciągu konstrukcji.

> [!div class="step-by-step"]
> [Poprzednie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
