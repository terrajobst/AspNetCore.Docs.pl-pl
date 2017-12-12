---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: "Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 1: wprowadzenie | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ten samouczek serii kompilacje w aplikacji sieci web Contoso University utworzony przez wprowadzenie do samouczka serii Entity Framework. Jeśli yo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6f93d6033b68773507d624125936f0a69777e2b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 1: wprowadzenie
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) samouczka serii. Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii.
> 
> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Przykładowa aplikacja jest witryną internetową uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania.
> 
> Samouczek zawiera przykłady w języku C#. [Pobrania](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) zawiera kod w języku C# i Visual Basic.
> 
> ## <a name="database-first"></a>Najpierw bazy danych
> 
> Istnieją trzy sposoby pracy z danymi programu Entity Framework: *Database First*, *Model First*, i *Code First*. Ten samouczek jest przeznaczony dla pierwszej bazy danych. Aby uzyskać informacji o różnicach między te przepływy pracy i wskazówki na temat wybierania najlepszy dla danego scenariusza, zobacz [przepływów pracy programu Entity Framework programowanie](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formularze sieci Web
> 
> Podobnie jak seria wprowadzenie tego samouczka serii korzysta z modelu formularzy sieci Web ASP.NET i przyjęto założenie, że wiesz, jak pracować z formularzy sieci Web ASP.NET w programie Visual Studio. Jeśli nie zobacz [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Jeśli wolisz pracować z platformę ASP.NET MVC, zobacz [wprowadzenie do korzystania z programu Entity Framework za pomocą platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Wyświetlany w samouczku** | **Współdziała również z** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio Express 2010 for Web. Samouczek nie był testowany z nowszej wersji programu Visual Studio. Różnią się wiele opcji menu, okna dialogowe i szablony. |
> | .NET 4 | .NET 4.5 jest zgodna z .NET 4, ale nie została przetestowana samouczek z .NET 4.5. |
> | Entity Framework w wersji 4 | Samouczek nie był testowany z nowszej wersji programu Entity Framework. Począwszy od programu Entity Framework 5, domyślnie używa EF `DbContext API` wprowadzono w systemie EF 4.1. Formant EntityDataSource został zaprojektowany do użycia `ObjectContext` interfejsu API. Aby uzyskać informacje o sposobie używania obiektu EntityDataSource sterować za pomocą `DbContext` interfejsu API, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Pytania
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i składnika LINQ to Entities forum](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Kontroli umożliwia szybkie tworzenie aplikacji, ale zwykle wymaga zachować dużą logika biznesowa i logika dostępu do danych w sieci *.aspx* stron. Jeśli aplikacja zwiększa się złożoność i wymaganie rutynowej konserwacji, więcej czasu programowanie można zainwestować góry aby można było utworzyć *n warstwowa* lub *warstwie* struktury aplikacji to jest bardziej łatwy w obsłudze. Do wdrożenia tej architektury, można oddzielić od warstwy logiki biznesowej (logiki warstwy Biznesowej) oraz warstwa dostępu do danych (DAL) warstwy prezentacji. Jednym ze sposobów implementuje ta struktura jest użycie `ObjectDataSource` kontrolować zamiast `EntityDataSource` formantu. Jeśli używasz `ObjectDataSource` kontroli, wykonania kodu dostępu do danych, a następnie wywołaj w *.aspx* strony za pomocą formantu, który ma wiele takich samych funkcji innych formantów źródła danych. Dzięki temu można łączyć zalety podejścia n warstwowa z zalet dotyczących dostępu do danych za pomocą kontrolki formularzy sieci Web.

`ObjectDataSource` Kontroli zapewnia większą elastyczność w inny sposób, jak również. Ponieważ można napisać własny kod dostępu do danych, łatwiej jest więcej niż tylko do odczytu, wstawiania, aktualizowania lub usuwania określonego typu, które są zadania który `EntityDataSource` kontroli służy do wykonywania. Na przykład można wykonać rejestrowania każdej aktualizacji jednostki, archiwizowanie danych zawsze po usunięciu jednostki lub automatycznie sprawdzanie i aktualizacji danych związanych z zgodnie z potrzebami podczas wstawiania wiersza o wartości klucza obcego.

## <a name="business-logic-and-repository-classes"></a>Logika biznesowa i klasy repozytorium

`ObjectDataSource` Działania kontroli przez klasę, która tworzenia. Klasa zawiera metody, które pobierania i aktualizowania danych, i podanie nazwy tych metod, które `ObjectDataSource` kontroli w znaczniku. Podczas renderowania lub odświeżania strony przetwarzania `ObjectDataSource` wywołuje metody, które zostały określone.

Oprócz podstawowych operacji CRUD, tworzona za pomocą klasy `ObjectDataSource` formant może być konieczne wykonanie logiki biznesowej po `ObjectDataSource` odczytuje lub aktualizuje dane. Na przykład po zaktualizowaniu działu, może być konieczne Sprawdź, czy nie inne działy mają tego samego konta administratora, ponieważ jedna osoba nie może być administratorem działu więcej niż jeden.

W niektórych `ObjectDataSource` dokumentacji, takich jak [Przegląd klasy ObjectDataSource](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.aspx), kontrolka wywołuje klasy nazywane *obiektu biznesowego* zawierającą zarówno logika biznesowa i logika dostępu do danych . W tym samouczku utworzysz osobnych klas logika biznesowa i logika dostępu do danych. Klasa, która hermetyzuje logika dostępu do danych jest nazywany *repozytorium*. Klasa logika biznesowa zawiera metody logiki biznesowej i metod dostępu do danych, ale metody dostępu do danych mogą wywoływać repozytorium, aby wykonywać zadania dostępu do danych.

Warstwa abstrakcji między logiki warstwy Biznesowej i warstwy DAL umożliwiającą automatyczne jednostki spowoduje również utworzenie testowania logiki warstwy Biznesowej. Ta warstwa abstrakcji jest implementowany przez tworzenie interfejsu i przy użyciu interfejsu w przypadku wystąpienia repozytorium w klasie logiki biznesowej. Dzięki temu można podać klasy logiki biznesowej w odniesieniu do dowolnego obiektu, który implementuje interfejs repozytorium. Do normalnego działania musisz podać obiektu repozytorium, który działa z programu Entity Framework. Do testowania, musisz podać obiekt repozytorium, który współpracuje z danych przechowywanych w taki sposób, który można łatwo manipulować, takich jak klasy zmienne zdefiniowane jako kolekcji.

Na poniższej ilustracji przedstawiono różnica między klasy logiki biznesowej, która zawiera logikę dostępu do danych bez repozytorium i korzystającą z repozytorium.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Rozpocznie się przez utworzenie strony sieci web, w którym `ObjectDataSource` kontrolka jest powiązana bezpośrednio do repozytorium, ponieważ spełnia on tylko podstawowego dostępu do danych zadania. W następnym samouczku utworzysz klasy logiki biznesowej z logikę weryfikacji i powiązać `ObjectDataSource` kontrolki do tej klasy, a nie klasę repozytorium. Zostanie również tworzenia testów jednostkowych dla logiki sprawdzania poprawności. W trzecim samouczku tej serii doda sortowania i filtrowania funkcje do aplikacji.

Strony tworzenia w tym samouczku pracować z `Departments` zestawu jednostek w modelu danych, który został utworzony w [Wprowadzenie samouczek serii](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualizowanie bazy danych i modelu danych

W tym samouczku zostanie rozpoczęta, wprowadzając dwie zmiany w bazie danych, które wymaga odpowiednich zmian w modelu danych, który został utworzony w [wprowadzenie do korzystania z programu Entity Framework i formularzy sieci Web](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) samouczki. W jednym z tych samouczkach zmiany zostały wprowadzone w Projektancie ręcznie, aby zsynchronizować modelu danych z bazy danych po zmianie bazy danych. W tym samouczku użyjesz projektanta **zaktualizować z bazy danych modelu** narzędzie automatyczne aktualizowanie modelu danych.

### <a name="adding-a-relationship-to-the-database"></a>Dodawanie relacji z bazą danych

W programie Visual Studio Otwórz utworzony w aplikacji sieci web Contoso University [wprowadzenie do korzystania z programu Entity Framework i formularzy sieci Web](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) samouczka serii, a następnie otwórz `SchoolDiagram` schemat bazy danych.

Jeśli przyjrzymy się `Department` tabeli w diagramie bazy danych, zobaczysz, że ma `Administrator` kolumny. W tej kolumnie jest kluczem obcym do `Person` tabeli, ale nie relacji klucza obcego jest zdefiniowany w bazie danych. Należy utworzyć relację, a Aktualizacja modelu danych, dzięki czemu programu Entity Framework automatycznie może obsłużyć tej relacji.

Na diagramie bazy danych, kliknij prawym przyciskiem myszy `Department` tabeli, a następnie wybierz **relacje**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

W **relacje klucza obcego** kliknij pozycję **Dodaj**, następnie kliknij przycisk wielokropka dla **Specyfikacja tabel i kolumn**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

W **tabel i kolumn** okno dialogowe pola, ustaw tabeli klucza podstawowego i pola do `Person` i `PersonID`, ustaw tabeli klucza obcego i pola do `Department` i `Administrator`. (Jeśli to zrobisz, Nazwa relacji ulegnie zmianie z `FK_Department_Department` do `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Kliknij przycisk **OK** w **tabel i kolumn** kliknij **Zamknij** w **relacje klucza obcego** i Zapisz zmiany. Jeśli pojawi się monit, aby zapisać `Person` i `Department` tabele, kliknij przycisk **tak**.

> [!NOTE]
> Jeśli została usunięta `Person` wiersze odpowiadające danych, który jest już w `Administrator` kolumny, nie będzie można zapisać tej zmiany. W takim przypadku za pomocą edytora tabeli w **Eksploratora serwera** do upewnij się, że `Administrator` wartości w każdym `Department` wiersz zawiera identyfikator rekordu, który istnieje w `Person` tabeli.
> 
> Po zapisaniu zmian, nie będzie można usunąć wiersza z `Person` tabeli, jeśli osoba ma uprawnienia administratora działu. W aplikacji produkcyjnej czy Podaj komunikat o błędzie, usunięcie uniemożliwia ograniczenie bazy danych lub należy określić usuwania kaskadowego. Na przykład sposobu określania usuwania kaskadowego zobacz [program Entity Framework i platformy ASP.NET — wprowadzenie Rozpoczęto część 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Dodawanie widoku do bazy danych

W nowym *Departments.aspx* strony, który zostanie utworzony, chcesz zapewnić nazwy w formacie "ostatni, najpierw" listy rozwijanej instruktorów, dzięki czemu użytkownicy mogą wybrać Administratorzy działów. Aby ułatwić to zrobić, spowoduje utworzenie widoku w bazie danych. Widok będzie zawierać tylko te dane, które są wymagane przez listy rozwijanej: Pełna nazwa (poprawnie sformatowana) i klucza rekordu.

W **Eksploratora serwera**, rozwiń węzeł *School.mdf*, kliknij prawym przyciskiem myszy **widoków** folder, a następnie wybierz **Dodaj nowy widok**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Kliknij przycisk **Zamknij** podczas **Dodaj tabelę** pojawi się okno dialogowe i wklej następującą instrukcję SQL w okienku SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Zapisz widok jako `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualizacja modelu danych

W *DAL* folder, otwórz *SchoolModel.edmx* , kliknij prawym przyciskiem myszy powierzchnię projektu, a następnie wybierz **modelu aktualizacji z bazy danych**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

W **wybierz obiekty bazy danych użytkownika** okno dialogowe, wybierz opcję **Dodaj** i wybierz utworzony widok.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Kliknij przycisk **Zakończ**.

W projektancie, zobaczysz, że narzędzie utworzone `vInstructorName` jednostki i nowe skojarzenie między `Department` i `Person` jednostek.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> W **dane wyjściowe** i **listy błędów** systemu windows można napotkać komunikat ostrzegawczy informujący o tym, że narzędzie automatycznie utworzone podstawowego klucza dla nowego `vInstructorName` widoku. Jest to oczekiwane zachowanie.


Odwołań do nowego `vInstructorName` jednostki w kodzie, nie chcesz użyć prefiksu małymi literami "v" do niego z Konwencją bazy danych. W związku z tym spowoduje zmianę nazwy podmiotu i zestawu jednostek w modelu.

Otwórz **modelu przeglądarki**. Zostanie wyświetlony `vInstructorName` wymieniony jako typu jednostki i widokiem.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

W obszarze **SchoolModel** (nie **SchoolModel.Store**), kliknij prawym przyciskiem myszy **vInstructorName** i wybierz **właściwości**. W **właściwości** Zmień **nazwa** dla właściwości "InstructorName" i zmień **Nazwa zestawu jednostek** dla właściwości "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Zapisz i zamknij modelu danych, a następnie Odbuduj projekt.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Przy użyciu klasy repozytorium i kontrolki ObjectDataSource

Utwórz nowy plik klasy w *DAL* folderu, nadaj jej nazwę *SchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Ten kod zawiera jeden `GetDepartments` metodę zwracającą wszystkie jednostki w `Departments` zestawu jednostek. Ponieważ wiadomo, że możesz będą uzyskiwać dostęp do `Person` właściwości nawigacji dla każdego wiersza zwrócone, możesz określić wczesny ładowania dla tej właściwości przy użyciu `Include` metody. Klasa implementuje również `IDisposable` interfejs, aby upewnić się, że połączenie z bazą danych jest zwolnione, gdy obiekt jest usunięty.

> [!NOTE]
> Popularną praktyką jest utworzenie klasy repozytorium dla poszczególnych typów jednostek. W tym samouczku jest używany jedną klasę repozytorium dla wielu typów jednostek. Aby uzyskać więcej informacji na temat wzorca repozytorium, zobacz wpisów w [blog zespołu programu Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) i [blogu Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Metoda zwraca `IEnumerable` obiektu zamiast `IQueryable` obiektu w celu zapewnienia użytecznego zwracana kolekcja nawet po usunięciu obiektu repozytorium. `IQueryable` Obiektu może spowodować dostęp do bazy danych, gdy jest on dostępny, ale obiekt repozytorium może być usunięty w czasie próby renderowania danych formantu z danymi. Może zwrócić inny typ kolekcji, takie jak `IList` obiekt zamiast `IEnumerable` obiektu. Jednak zwracanie `IEnumerable` obiektu gwarantuje, że można wykonywać zadania przetwarzania typowych listy tylko do odczytu takie jak `foreach` pętli i zapytań LINQ, ale nie można dodać do lub usuwanie elementów w kolekcji, co może oznacza, że będą takie zmiany utrwalone w bazie danych.

Utwórz *Departments.aspx* strona używa *Site.Master* strony wzorcowej i Dodaj następujący kod w `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Tworzy tego znacznika `ObjectDataSource` formant, który używa klasy repozytorium nowo utworzone i `GridView` formantu, aby wyświetlić dane. `GridView` Kontroli określa **Edytuj** i **usunąć** polecenia, ale nie dodano kod, aby jeszcze obsługiwane.

Użyj kilka kolumn `DynamicField` steruje tak, aby można było korzystać z funkcji danych automatycznego formatowania i walidacji. W tym do pracy, należy wywołać `EnableDynamicData` metoda `Page_Init` obsługi zdarzeń. (`DynamicControl` formanty nie są używane w `Administrator` pola, ponieważ one nie działać z właściwości nawigacji.)

`Vertical-Align="Top"` Atrybuty staną się ważne później podczas dodawania kolumny, która ma zagnieżdżoną `GridView` formant do siatki.

Otwórz *Departments.aspx.cs* pliku i dodaj następującą `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Następnie dodaj poniższe obsługę strony `Init` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

W *DAL* folderu, Utwórz nowy plik klasy o nazwie *Department.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Ten kod dodaje metadane do modelu danych. Określa, że `Budget` właściwość `Department` jednostki faktycznie reprezentuje walutę, mimo że jego typu danych `Decimal`, i określa, że wartość musi wynosić od 0 do 1,000,000.00 $. Określa również który `StartDate` właściwości powinien być sformatowany jako datę w formacie mm/dd/rrrr.

Uruchom *Departments.aspx* strony.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Zwróć uwagę, że mimo że nie określono ciągu formatu w *Departments.aspx* znaczników dla **budżetu** lub **Data rozpoczęcia** kolumn, domyślne waluty i daty Formatowanie stosowane na im przez `DynamicField` kontrolki, przy użyciu metadanych, który został dostarczony w *Department.cs* pliku.

## <a name="adding-insert-and-delete-functionality"></a>Wstaw dodawania i usuwania funkcji

Otwórz *SchoolRepository.cs*, Dodaj następujący kod, aby można było utworzyć `Insert` — metoda i `Delete` metody. Ten kod zawiera także metodę o nazwie `GenerateDepartmentID` obliczającej następnej wartości klucza rekordu dostępne do użycia przez `Insert` metody. Jest to wymagane, ponieważ nie skonfigurowano bazę danych do obliczenia to automatycznie dla `Department` tabeli.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach — metoda

`DeleteDepartment` Wywołania metody `Attach` metody, aby ponownie ustanowić łącze, które są przechowywane w Menedżerze stanu obiektu kontekstu obiektów między jednostki w pamięci i bazę danych wiersz reprezentuje. To musi być wcześniejsza niż wywołania metody `SaveChanges` metody.

Termin *kontekst* odwołuje się do klasy Entity Framework, która jest pochodną `ObjectContext` klasy, która umożliwia dostęp do zestawów jednostek i obiektów. W kodzie dla tego projektu, nosi nazwę klasy `SchoolEntities`, i zawsze jest nazwane wystąpienie `context`. Kontekst obiektu *Menedżer stanu obiektu* jest klasą pochodną `ObjectStateManager` klasy. Skontaktuj się z obiektu używa obiektu Menedżer stanu do przechowywania obiektów jednostek i czy każdy z nich jest zsynchronizowana z jego odpowiedni wiersz tabeli lub wierszy w bazie danych.

Po przeczytaniu jednostki kontekst zapisuje go w obiekcie Menedżer stanu i śledzi czy reprezentacja tego obiektu jest zsynchronizowana z bazy danych. Na przykład zmiana wartości właściwości, Flaga jest ustawiona na wskazują, że można zmienić właściwości nie jest już w bazie danych. Następnie podczas wywoływania `SaveChanges` metody, kontekst wie, co należy zrobić w bazie danych, ponieważ Menedżer stanu obiektu zna dokładnie co różni się od bieżącego stanu jednostki i stan bazy danych.

Jednak ten proces zwykle nie działa w aplikacji sieci web, ponieważ wystąpienie obiektu kontekstu, odczytujący jednostki, wraz z wszystkich elementów w jego menedżera stanu obiektów zostanie usunięty po renderowania strony. Wystąpienie obiektu kontekstu, które należy zastosować zmiany jest nowy, który zostanie uruchomiony do przetwarzania odświeżania strony. W przypadku programu `DeleteDepartment` metody, `ObjectDataSource` kontroli ponownie tworzy oryginalnej wersji jednostki z wartości w widoku stanu, ale to utworzony ponownie `Department` jednostka nie istnieje w Menedżerze stanu obiektu. Jeśli zostanie wywołany `DeleteObject` metody w tej jednostce ponownego utworzenia, połączenie nie powiedzie się, ponieważ kontekst nie może określić, czy jednostka jest zsynchronizowana z bazy danych. Jednak podczas wywoływania `Attach` metody ponownie nawiązuje tego samego śledzenia między ponownie utworzonej jednostki i wartościami w bazie danych, która pierwotnie została wykonana automatycznie, gdy jednostka została odczytana w starszych wystąpienia kontekstu obiektów.

Brak czasu nie mają kontekst śledzenia jednostki w obiekcie Menedżer stanu i można ustawić flagi, aby zapobiec operacją. Przykładem tego są wyświetlane w kolejnych samouczkach z tej serii.

### <a name="the-savechanges-method"></a>Metody SaveChanges

Ta klasa prostego repozytorium przedstawiono podstawowe zasady sposób wykonywania operacji CRUD. W tym przykładzie `SaveChanges` metoda jest wywoływana bezpośrednio po każdej aktualizacji. W aplikacji produkcyjnej warto wywołać `SaveChanges` metody z oddzielnych metodach, aby mieć lepszą kontrolę nad po zaktualizowaniu bazy danych. (Na końcu następnym samouczku znajdziesz łącza do dokumentacji omówiono jednostki pracy wzorzec, który jest jednym z podejść koordynowanie odpowiednich aktualizacji.) Zwróć uwagę, że w tym przykładzie `DeleteDepartment` — metoda nie ma kodu do obsługi konfliktom współbieżności; zostanie dodany kod w tym w późniejszym samouczku z tej serii.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Podczas pobierania nazwy instruktora do wybrania przy wstawianiu

Użytkownicy musi mieć należy wybrać z listy instruktorów na liście rozwijanej administrator, podczas tworzenia nowych działów. W związku z tym, Dodaj następujący kod do *SchoolRepository.cs* utworzyć metody, aby pobrać listę instruktorów przy użyciu widoku, który został utworzony wcześniej:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Tworzenie strony do wstawiania działów

Utwórz *DepartmentsAdd.aspx* strona używa *Site.Master* strony i Dodaj następujący kod w `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Tego znacznika tworzy dwa `ObjectDataSource` kontroluje jedno wstawiania nowych `Department` jednostek i jeden do pobierania nazw instruktora `DropDownList` formant, który służy do wybierania Administratorzy działów. Tworzy kod znaczników `DetailsView` kontrolować wprowadzanie nowych działów który określa obsługi dla formantu `ItemInserting` zdarzeń, dzięki czemu można ustawić `Administrator` wartości klucza obcego. Na końcu jest `ValidationSummary` formantu, aby wyświetlić komunikaty o błędach.

Otwórz *DepartmentsAdd.aspx.cs* i dodaj następującą `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Dodaj następującą zmienną klasy i metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Metody umożliwia korzystanie z funkcji danych dynamicznych. Program obsługi dla `DropDownList` formantu `Init` zdarzeń zapisuje odwołanie do formantu i obsługi dla `DetailsView` formantu `Inserting` zdarzeń używa tego odwołania, aby uzyskać `PersonID` wartość wybranym instruktorze i aktualizacji `Administrator` właściwości kluczy obcych z `Department` jednostki.

Uruchom strony, dodawania informacji działu nowe, a następnie kliknij **Wstaw** łącza.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Wprowadź wartości dla innego działu nowe. Wprowadź liczbę większą niż 1,000,000.00 w **budżetu** pola i tab do następnego pola. W polu pojawi się znak gwiazdki, a jeśli kursora myszy nad nim, zobaczysz komunikat o błędzie, wprowadzone w metadanych dla tego pola.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Kliknij przycisk **Wstaw**, i zostanie wyświetlony komunikat o błędzie wyświetlany przez `ValidationSummary` kontroli w dolnej części strony.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Następnie zamknij przeglądarkę i Otwórz *Departments.aspx* strony. Dodaj możliwość usuwania *Departments.aspx* dodając `DeleteMethod` atrybutu `ObjectDataSource` kontroli, a `DataKeyNames` atrybutu `GridView` formantu. Znaczniki otwarcia tych kontrolek będzie teraz podobne do następujących:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Uruchom strony.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Usuń działu dodane po uruchomieniu *DepartmentsAdd.aspx* strony.

## <a name="adding-update-functionality"></a>Dodawanie funkcji aktualizacji

Otwórz *SchoolRepository.cs* i dodaj następującą `Update` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Po kliknięciu **aktualizacji** w *Departments.aspx* strony, `ObjectDataSource` kontroli tworzy dwa `Department` jednostek do przekazania do `UpdateDepartment` metody. Jedno zawiera oryginalnych wartości, które były przechowywane w widoku stanu, a drugi zawiera nowe wartości, które zostały wprowadzone w `GridView` formantu. Kod w `UpdateDepartment` metoda przekazuje `Department` jednostki, która ma oryginalnych wartości do `Attach` metody w celu ustanowienia śledzenia między jednostki i o nowościach w bazie danych. Następnie kod `Department` jednostki, która zawiera nowe wartości, aby `ApplyCurrentValues` metody. Kontekst porównanie starych i nowych wartości. Jeśli nowa wartość jest inna niż poprzednia wartość, kontekst zmieni się wartość właściwości. `SaveChanges` Metody następnie aktualizuje tylko zmienione kolumny w bazie danych. (Jednak jeśli ta funkcja aktualizacji dla tej jednostki zostały zamapowane do procedury składowanej, cały wiersz będzie można zaktualizować niezależnie od tego, które zostały zmienione kolumn.)

Otwórz *Departments.aspx* i dodaj następujące atrybuty `DepartmentsObjectDataSource` sterowania:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Ta powoduje, że stary wartości były przechowywane w widoku stanu, dzięki czemu można porównać przy użyciu nowych wartości w `Update` metody.
- `OldValuesParameterFormatString="orig{0}"`   
 Informuje formant, który jest nazwa oryginalnego parametru wartości `origDepartment` .

Kod znaczników dla tagu otwierającego `ObjectDataSource` kontroli teraz podobnego do następującego:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Dodaj `OnRowUpdating="DepartmentsGridView_RowUpdating"` atrybutu `GridView` formantu. Zostanie ona użyta do ustawienia `Administrator` wartość właściwości na podstawie wiersza użytkownik wybiera z listy rozwijanej. `GridView` Tagu początkowego teraz podobnego do następującego:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Dodaj `EditItemTemplate` sterować dla `Administrator` kolumny `GridView` kontrolować, natychmiast po `ItemTemplate` kontroli dla tej kolumny:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

To `EditItemTemplate` formant jest podobny do `InsertItemTemplate` kontroli w *DepartmentsAdd.aspx* strony. Różnica polega na wartość początkowa formantu jest ustawiona, przy użyciu `SelectedValue` atrybutu.

Przed `GridView` kontrolować, Dodaj `ValidationSummary` sterować tak samo jak *DepartmentsAdd.aspx* strony.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Otwórz *Departments.aspx.cs* i bezpośrednio po deklaracji klasy częściowe, Dodaj następujący kod, aby utworzyć pole prywatne odwołanie do `DropDownList` sterowania:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Następnie dodaj obsługę `DropDownList` formantu `Init` zdarzeń i `GridView` formantu `RowUpdating` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Program obsługi dla `Init` odwołanie do zapisuje zdarzenia `DropDownList` kontrolki pola klasy. Program obsługi dla `RowUpdating` zdarzeń używa odwołania można pobrać wartości wprowadzonych przez użytkownika i zastosować je do `Administrator` właściwość `Department` jednostki.

Użyj *DepartmentsAdd.aspx* strony, aby dodać nowy dział, a następnie uruchom *Departments.aspx* i kliknij przycisk **Edytuj** na wiersz, który został dodany.

> [!NOTE]
> Nie można edytować wierszy, które nie zostały dodane (oznacza to, że już zostały w bazie danych), ze względu na nieprawidłowe dane w bazie danych. Administratorzy dla wierszy, które zostały utworzone w bazie danych są studenta. Jeśli próbujesz edytować jeden z nich, otrzyma strony błędu, który zgłasza błąd, takich jak`'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Jeśli wprowadzisz nieprawidłowy **budżetu** ilość, a następnie kliknij przycisk **aktualizacji**, zapoznaj się z tego samego gwiazdki i komunikat o błędzie, który był wyświetlany w *Departments.aspx* strony.

Zmień wartość pola lub wybierz innego administratora i kliknij przycisk **aktualizacji**. Zmiana jest wyświetlany.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Na tym kończy się wprowadzenie do korzystania z `ObjectDataSource` kontrola basic CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacje przy użyciu programu Entity Framework. Powstanie prostej aplikacji warstwowych, ale warstwy logiki biznesowej jest nadal ściśle powiązane do warstwy dostępu do danych, co stwarza automatyczne testy jednostkowe. W samouczku następujące zobaczysz sposobu implementacji wzorca repozytorium w celu ułatwienia testowania jednostki.

>[!div class="step-by-step"]
[Dalej](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
