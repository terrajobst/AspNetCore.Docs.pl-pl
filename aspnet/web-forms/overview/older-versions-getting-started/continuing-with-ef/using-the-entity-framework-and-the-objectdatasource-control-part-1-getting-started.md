---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 1: wprowadzenie | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web University firmy Contoso, utworzony przez wprowadzenie do tej serii samouczka platformy Entity Framework. Jeśli yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 5eaeaa0aa474e1aed86954e6c10dd1703b938944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757270"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 1: wprowadzenie
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serii samouczków. Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków.
> 
> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Przykładowa aplikacja jest witryną internetową uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.
> 
> Samouczek zawiera przykłady w języku C#. [Przykładowe do pobrania](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) zawiera kod w językach C# i Visual Basic.
> 
> ## <a name="database-first"></a>Najpierw bazy danych
> 
> Istnieją trzy sposoby pracy z danymi w programie Entity Framework: *Database First*, *pierwszy Model*, i *Code First*. Niniejszy samouczek jest dla pierwszej bazy danych. Aby uzyskać informacje o różnicach między tymi przepływami pracy a wskazówki o tym, jak wybrać najlepszy dla danego scenariusza, zobacz [przepływów pracy programu Entity Framework programowania](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formularze sieci Web
> 
> Podobnie jak serii wprowadzenie tej serii samouczków używa modelu formularzy sieci Web ASP.NET i przyjęto założenie, że wiesz, jak pracować z wzorca ASP.NET Web Forms w programie Visual Studio. Jeśli nie widzisz [wprowadzenie do wzorca ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Jeśli wolisz pracować z platformę ASP.NET MVC, zobacz [rozpoczęcie korzystania z programu Entity Framework przy użyciu platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w tym samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Program Visual Studio 2010 Express for Web. Samouczek nie został przetestowany przy użyciu nowszej wersji programu Visual Studio. Istnieje wiele różnic w opcje menu, okna dialogowe i szablony. |
> | .NET 4 | .NET 4.5 jest zgodny z poprzednimi wersjami z .NET 4, ale nie przetestowano tego samouczka przy użyciu platformy .NET 4.5. |
> | Entity Framework 4 | Samouczek nie został przetestowany w nowszych wersjach platformy Entity Framework. Począwszy od programu Entity Framework 5, domyślnie używa EF `DbContext API` wprowadzono w systemie EF 4.1. Formant EntityDataSource został zaprojektowany do użycia `ObjectContext` interfejsu API. Aby uzyskać informacje o sposobie używania EntityDataSource kontrolką `DbContext` interfejsu API, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Pytania
> 
> Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Control umożliwia szybkie tworzenie aplikacji, ale zazwyczaj wymaga można zachować znacznej ilości logika biznesowa i logika dostępu do danych w Twojej *.aspx* stron. Jeśli oczekujesz, że aplikacja zwiększanie się stopnia skomplikowania i będą musieli rutynowej konserwacji, możesz inwestować więcej czasu na programowanie na początku aby można było utworzyć *n warstwowa* lub *warstwie* struktury aplikacji to będzie łatwiejszy w utrzymaniu. Aby wdrożyć tę architekturę, można oddzielić Warstwa prezentacji od warstwy logiki biznesowej (LOGIKI) i warstwy dostępu do danych (DAL). Jednym ze sposobów implementuje ta struktura jest użycie `ObjectDataSource` zamiast kontrolki `EntityDataSource` kontroli. Kiedy używasz `ObjectDataSource` kontrolki, zaimplementować własny kod dostępu do danych, a następnie wywołaj ją w *.aspx* strony za pomocą formantu, który ma wiele takich samych funkcje jak inne kontrolki źródła danych. Dzięki temu można połączyć korzyści wynikające z podejścia n warstwowa przy użyciu korzyści z używania formant formularzy sieci Web, aby uzyskać dostęp do danych.

`ObjectDataSource` Kontroli zapewnia większą elastyczność w inny sposób, jak również. Ponieważ pisania własnego kodu dostępu do danych, łatwiej jest więcej niż tylko odczytać, wstawiania, aktualizacji lub usuwania określonego typu, które są zadania, `EntityDataSource` kontroli zaprojektowano w celu wykonania. Na przykład można wykonać rejestrowania za każdym razem, gdy jednostka zostanie zaktualizowany, archiwizowanie danych zawsze wtedy, gdy została usunięta lub automatycznego wyboru i zaktualizuj dane dotyczące stosownie do potrzeb podczas wstawiania wierszy z wartością klucza obcego.

## <a name="business-logic-and-repository-classes"></a>Logika biznesowa i klasy repozytorium

`ObjectDataSource` Działania kontroli przez wywołanie klasy, którą tworzysz. Klasa zawiera metody, które pobierania i aktualizowania danych, a następnie podaj nazwy tych metod do `ObjectDataSource` kontroli w znacznikach. Podczas renderowania lub przetwarzania zwrotu `ObjectDataSource` wywołuje metody, które zostały określone.

Oprócz podstawowych operacji CRUD, klasa, która utworzyć za pomocą `ObjectDataSource` kontroli może zajść potrzeba wykonania logiki biznesowej po `ObjectDataSource` odczytuje lub aktualizuje dane. Na przykład po zaktualizowaniu dział może być konieczne Sprawdź, czy nie inne działy mają tego samego konta administratora, ponieważ jedna osoba nie może być administratorem więcej niż jednego działu.

W niektórych `ObjectDataSource` dokumentacji, takie jak [omówienie kontrolki ObjectDataSource klasy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), kontrolka wywołuje klasę nazywane *obiektem biznesowym* zawierającej logika biznesowa i logika dostępu do danych . W tym samouczku utworzysz osobnych klas, logika biznesowa i logika dostępu do danych. Klasa, która hermetyzuje logikę dostęp do danych jest wywoływana *repozytorium*. Klasa logika biznesowa zawiera metody logiki biznesowej i metod dostępu do danych, ale metody dostępu do danych mogą wywoływać repozytorium do wykonywania zadań dostęp do danych.

Zostanie również utworzony warstwę abstrakcji między LOGIKI i warstwy DAL, który ułatwia automatycznych jednostkowych testowania LOGIKI. Ta warstwa abstrakcji jest implementowany przez tworzenie interfejsu i przy użyciu interfejsu, podczas tworzenia wystąpienia repozytorium w klasie logiki biznesowej. Dzięki temu możliwa jest zapewnienie klasy logikę biznesową w odniesieniu do dowolnego obiektu, który implementuje interfejs repozytorium. Dla normalnych operacji należy podać obiekt repozytorium, który współdziała z platformą Entity Framework. W przypadku testowania należy podać obiekt repozytorium, który działa z danymi przechowywanymi w taki sposób, że można łatwo manipulować, takie jak zmienne klasy zdefiniowane jako kolekcji.

Na poniższej ilustracji przedstawiono różnice między klasą logikę biznesową, która zawiera logikę dostęp do danych bez repozytorium i jednego, który używa repozytorium.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Rozpocznie się, tworząc strony sieci web, w którym `ObjectDataSource` kontrolka jest powiązana bezpośrednio do repozytorium, ponieważ wykonuje tylko dostęp do danych podstawowych zadań. W następnym samouczku należy utworzyć klasę logiki biznesowej przy użyciu logiki weryfikacji i powiązać `ObjectDataSource` kontrolki do tej klasy, a nie klasę repozytorium. Zostanie również utworzyć testy jednostkowe dla logikę weryfikacji. W to trzeci samouczek z tej serii dodasz, sortowanie i filtrowanie funkcji do aplikacji.

Strony tworzony w tym samouczku pracować `Departments` zestawu jednostek w modelu danych, który został utworzony w [wprowadzenie serii samouczków](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualizowanie bazy danych i modelu danych

W tym samouczku rozpocznie się, wprowadzając dwie zmiany w bazie danych, które wymagają odpowiednie zmiany w modelu danych, który został utworzony w [rozpoczęcie korzystania z programu Entity Framework i formularzy sieci Web](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) samouczków. W jednym z tych samouczków zmiany zostały wprowadzone w Projektancie ręcznie, aby zsynchronizować modelu danych z bazy danych po zmianie bazy danych. W tym samouczku użyjesz projektanta **aktualizacji z bazy danych modelu** narzędzia na automatyczne aktualizowanie modelu danych.

### <a name="adding-a-relationship-to-the-database"></a>Dodawanie relacji w bazie danych

W programie Visual Studio, otwórz aplikacji sieci web firmy Contoso University został utworzony w [rozpoczęcie korzystania z programu Entity Framework i formularzy sieci Web](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serii samouczków, a następnie otwórz `SchoolDiagram` diagramu bazy danych.

Jeśli przyjrzymy się `Department` tabel w diagramie bazy danych, zobaczysz, że ma on `Administrator` kolumny. Ta kolumna znajduje się klucz obcy, aby `Person` tabeli, ale nie relacji klucza obcego jest zdefiniowana w bazie danych. Należy utworzyć relację i aktualizowanie modelu danych, dzięki czemu Entity Framework mogą automatycznie obsługiwać tę relację.

Na diagramie bazy danych, kliknij prawym przyciskiem myszy `Department` tabeli, a następnie wybierz **relacje**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

W **relacje klucza obcego** kliknij pozycję **Dodaj**, następnie kliknij przycisk wielokropka dla **Specyfikacja tabel i kolumn**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

W **tabele i kolumny** okna dialogowego pole, ustaw tabeli klucza podstawowego i pole `Person` i `PersonID`i ustaw Tabela klucza obcego i pole `Department` i `Administrator`. (Jeśli to zrobisz, Nazwa relacji ulegnie zmianie z `FK_Department_Department` do `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Kliknij przycisk **OK** w **tabele i kolumny** kliknij **Zamknij** w **relacje klucza obcego** polu, a następnie zapisz zmiany. Jeśli zostanie wyświetlony monit, jeśli chcesz zapisać `Person` i `Department` tabel, kliknij przycisk **tak**.

> [!NOTE]
> Jeśli została usunięta `Person` wierszy, które odnoszą się do danych, który znajduje się już w `Administrator` kolumny, nie będzie mógł zapisać tę zmianę. W takim przypadku należy użyć edytora tabeli w **Eksploratora serwera** do upewnij się, że `Administrator` wartość w każdej `Department` wiersz zawiera identyfikator rekordu, który w rzeczywistości istnieje `Person` tabeli.
> 
> Po zapisaniu zmian nie można usunąć wiersza z `Person` tabeli, jeśli osoba ta jest administratorem działu. W przypadku aplikacji produkcyjnej będzie zapewniają określony komunikat o błędzie, gdy ograniczenia database uniemożliwia usunięcie lub należy określić kaskadowego usuwania. Na przykład sposobu określania kaskadowego usuwania zobacz [platformy Entity Framework i ASP.NET — wprowadzenie do części 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Dodawanie widoku w bazie danych

W nowym *Departments.aspx* strony, która zostanie utworzona, chcesz udostępnić listy rozwijanej instruktorów, za pomocą nazwy w formacie "ostatni, najpierw", aby użytkownicy mogą wybierać Administratorzy działów. Aby ułatwić to zrobić, utworzysz widok w bazie danych. Widok będzie zawierać tylko dane wymagane przez listy rozwijanej: imię i nazwisko (prawidłowo sformatowaną) i klucza rekordu.

W **Eksploratora serwera**, rozwiń węzeł *School.mdf*, kliknij prawym przyciskiem myszy **widoków** folder, a następnie wybierz **Dodaj nowy widok**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Kliknij przycisk **Zamknij** podczas **Dodaj tabelę** pojawi się okno dialogowe i wklej następującą instrukcję SQL w okienku SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Widok jest zapisywany jako `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualizowanie modelu danych

W *DAL* folder, otwórz *SchoolModel.edmx* plików, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz **Model aktualizacji z bazy danych**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

W **wybierz obiekty bazy danych** okno dialogowe, wybierz opcję **Dodaj** karcie, a następnie wybierz widok został utworzony.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Kliknij przycisk **Zakończ**.

W projektancie, zobaczysz, że narzędzie utworzone `vInstructorName` jednostki i nowe skojarzenie między `Department` i `Person` jednostek.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> W **dane wyjściowe** i **lista błędów** systemu windows, można napotkać komunikat ostrzegawczy informujący o tym, że narzędzie automatycznie utworzony podstawowy klucz dla nowego `vInstructorName` widoku. Jest to oczekiwane zachowanie.


Gdy odwołasz się do nowego `vInstructorName` jednostki w kodzie, nie chcesz używać konwencji bazy danych z poprzedzania ich małe "v" do niego. W związku z tym spowoduje zmianę nazwy jednostek i zestawu jednostek w modelu.

Otwórz **modelu przeglądarki**. Zostanie wyświetlony `vInstructorName` wymienione jako typu jednostki, a widokiem.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

W obszarze **SchoolModel** (nie **SchoolModel.Store**), kliknij prawym przyciskiem myszy **vInstructorName** i wybierz **właściwości**. W **właściwości** oknie zmiany **nazwa** właściwość "InstructorName" i zmień **Nazwa zestawu jednostek** właściwość "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Zapisz i zamknij modelu danych, a następnie ponownie skompilować projekt.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Przy użyciu klasy repozytorium i kontrolka ObjectDataSource

Utwórz nowy plik klasy w *DAL* folderu, nadaj jej nazwę *SchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Ten kod zawiera pojedynczy `GetDepartments` metodę zwracającą wszystkie jednostki w `Departments` zestawu jednostek. Ponieważ wiadomo, że użytkownik będzie uzyskiwać dostęp do `Person` właściwości nawigacji dla każdego wiersza zwrócone, należy określić eager, ładowania dla tej właściwości przy użyciu `Include` metody. Klasa implementuje również `IDisposable` interfejsu, aby upewnić się, że połączenie z bazą danych jest zwalniany, gdy obiekt zostanie usunięty.

> [!NOTE]
> Powszechną praktyką jest, aby utworzyć klasę repozytorium dla każdego typu jednostki. W tym samouczku jest używany jedną klasę repozytorium dla wielu typów jednostek. Aby uzyskać więcej informacji na temat wzorca repozytorium zobacz wpisy w [blog zespołu programu Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) i [blogu Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Metoda zwraca `IEnumerable` obiektu, a nie `IQueryable` obiektu w celu zapewnienia, że zwrócona kolekcja jest użyteczne nawet w przypadku, po usunięciu sam obiekt repozytorium. `IQueryable` Obiektu może spowodować dostępu do bazy danych zawsze wtedy, gdy jest on dostępny, ale obiektu repozytorium może być usuwane przez czas kontrolkę powiązaną z danymi próbuje przedstawienia tych danych. Może zwrócić inny typ kolekcji, takie jak `IList` zamiast obiektu `IEnumerable` obiektu. Jednak zwracanie `IEnumerable` obiektu gwarantuje, że będzie możliwe wykonanie zadań przetwarzania typowej listy tylko do odczytu takich jak `foreach` pętli i zapytań LINQ, ale nie można dodać do lub usuwanie elementów w kolekcji, która może oznaczać, że takie zmiany będą utrwalone w bazie danych.

Tworzenie *Departments.aspx* strona używająca *Site.Master* strony wzorcowej, a następnie dodaj następujący kod znaczników w `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Ten kod znaczników tworzy `ObjectDataSource` formant, który używa klasy repozytorium właśnie utworzony, a `GridView` formantu, aby wyświetlić dane. `GridView` Kontroli określa **Edytuj** i **Usuń** poleceń, ale nie dodano kod, aby jeszcze obsługiwane.

Użyj kilku kolumn `DynamicField` kontroluje tak, że możesz korzystać z zalet funkcji formatowania i sprawdzania poprawności danych. Dla tych opcji do pracy, trzeba będzie wywołać `EnableDynamicData` method in Class metoda `Page_Init` programu obsługi zdarzeń. (`DynamicControl` formanty nie są używane w `Administrator` pola, ponieważ nie działają z właściwości nawigacji.)

`Vertical-Align="Top"` Atrybuty staną się ważne później podczas dodawania kolumny, która ma zagnieżdżoną `GridView` formant do siatki.

Otwórz *Departments.aspx.cs* pliku i Dodaj następujący kod `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Następnie dodaj następujący program obsługi dla strony `Init` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

W *DAL* folderu, Utwórz nowy plik klasy o nazwie *Department.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Ten kod dodaje metadanych do modelu danych. Określa, że `Budget` właściwość `Department` jednostki faktycznie reprezentuje walutę, mimo że jej typem danych jest `Decimal`, i określa, że wartość musi być pomiędzy 0 a 1,000,000.00 $. Określa również, `StartDate` właściwości powinny być sformatowane jako data w formacie mm/dd/rrrr.

Uruchom *Departments.aspx* strony.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Należy zauważyć, że mimo iż nie określiła ciąg formatu w *Departments.aspx* znaczników dla **budżetu** lub **Data rozpoczęcia** domyślne kolumny, waluty i daty Formatowanie zostały zastosowane do nich przez `DynamicField` kontroluje, przy użyciu metadanych, którą podano w *Department.cs* pliku.

## <a name="adding-insert-and-delete-functionality"></a>Dodawanie wstawiania i usuwania funkcji

Otwórz *SchoolRepository.cs*, Dodaj następujący kod, aby można było utworzyć `Insert` metody i `Delete` metody. Ten kod zawiera również metodę o nazwie `GenerateDepartmentID` obliczającej następnej wartości klucza rekordu dostępne do użycia przez `Insert` metody. Jest to wymagane, ponieważ baza danych nie jest skonfigurowany do obliczania to automatyczne `Department` tabeli.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach — metoda

`DeleteDepartment` Wywołania metody `Attach` metody, aby ponownie ustanowić łącze, które są przechowywane w Menedżer stanu obiektu kontekstu obiektów między jednostki w pamięci i bazy danych wiersz reprezentuje. To musi być wcześniejsza niż wywołania metody `SaveChanges` metody.

Termin *kontekst* odwołuje się do klasy Entity Framework, która pochodzi od klasy `ObjectContext` klasy, która umożliwia dostęp do zestawów encji i jednostek. W kodzie dla tego projektu o nazwie klasy `SchoolEntities`, i jego wystąpienie jest zawsze nazywać `context`. Kontekst obiektu *menedżera stanu obiektu* to klasa, która jest pochodną `ObjectStateManager` klasy. Skontaktuj się z obiektu używa Menedżera stanu obiektu, do przechowywania obiekty jednostki i czy każdy z nich jest zsynchronizowany z jego odpowiedni wiersz w tabeli lub wierszy w bazie danych.

Podczas odczytywania jednostki kontekst zapisze go w Menedżerze stanu obiektu i śledzi czy tę reprezentację obiektu jest zsynchronizowany z bazą danych. Na przykład w przypadku zmiany wartości właściwości flaga jest ustawiona na wskazują, że właściwości, który został zmieniony już nie jest zsynchronizowany z bazą danych. Następnie, gdy zostanie wywołana `SaveChanges` metody kontekst wie, co należy zrobić w bazie danych, ponieważ Menedżer stanu obiektów wie dokładnie, co to jest to różnica między bieżący stan jednostki i stan bazy danych.

Jednak ten proces zwykle nie działa w aplikacji sieci web, ponieważ wystąpienie kontekstu obiektu odczytujący jednostki, wraz z wszystkich elementów w jego menedżera stanu obiektów, zostanie usunięty po wyrenderowaniu strony. Wystąpienie kontekstu obiektu, który należy zastosować zmiany jest nowy, który zostanie uruchomiony w celu przetwarzania zwrotu. W przypadku właściwości `DeleteDepartment` metody `ObjectDataSource` kontroli ponownie tworzy oryginalnej wersji jednostki z wartości w widoku stanu, ale to ponowne utworzenie `Department` jednostka nie istnieje w Menedżerze stanu obiektu. Jeśli wywołujesz `DeleteObject` metody dla tej jednostki odtworzony wywołanie może zakończyć się niepowodzeniem, ponieważ kontekst nie wie, czy jednostka jest zsynchronizowany z bazą danych. Jednak podczas wywoływania `Attach` metoda ponownie nawiązuje tego samego śledzenia między ponownie utworzonej jednostki i wartości w bazie danych, która pierwotnie była generowane automatycznie, gdy jednostka została odczytana w wystąpieniu wcześniejszej kontekstu obiektów.

Istnieją terminy, gdy nie chcesz, aby kontekstu obiektów do śledzenia jednostek w menedżera stanu obiektu i można ustawić flagi, aby uniemożliwić tą operacją. Przykładem tego są wyświetlane w kolejnych samouczkach w tej serii.

### <a name="the-savechanges-method"></a>Metoda SaveChanges

Ta klasa prostego repozytorium przedstawia podstawowe zasady sposobu wykonywania operacji CRUD. W tym przykładzie `SaveChanges` metoda jest wywoływana bezpośrednio po każdej aktualizacji. W aplikacji produkcyjnej warto wywołać `SaveChanges` metody z oddzielnych metodach daje większą kontrolę nad po zaktualizowaniu bazy danych. (Na końcu następnego samouczka znajdziesz link do oficjalnego dokumentu, w tym artykule omówiono jednostki wzorzec pracy, który jest jednym z podejść do koordynowania odpowiednich aktualizacji.) Należy zauważyć, że w tym przykładzie `DeleteDepartment` metoda nie zawiera kodu Obsługa konfliktów współbieżności; kod, aby to zrobić, zostanie dodany później w samouczku z tej serii.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Podczas pobierania nazwy przez instruktorów, aby wybrać podczas wstawiania

Użytkownicy muszą być możliwe wybranie administratora z listy instruktorów na liście rozwijanej podczas tworzenia nowych działów. W związku z tym, Dodaj następujący kod do *SchoolRepository.cs* utworzyć metodę, która pobierze listę Instruktorzy przy użyciu widoku, który został utworzony wcześniej:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Tworzenie strony do wstawiania działów

Tworzenie *DepartmentsAdd.aspx* strona używająca *Site.Master* stronie, a następnie dodaj następujący kod znaczników w `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Ten kod znaczników umożliwia utworzenie dwóch `ObjectDataSource` kontroluje, jeden dla wstawianie nowych `Department` jednostek i jeden dla pobierania nazwy przez instruktorów `DropDownList` formant, który jest używany do wybierania Administratorzy działów. Tworzy znaczników `DetailsView` kontrolować wprowadzanie nowych działów i określa funkcję obsługi formantu `ItemInserting` zdarzenia dzięki czemu można ustawić `Administrator` wartości klucza obcego. Po zakończeniu będzie `ValidationSummary` formantu, aby wyświetlić komunikaty o błędach.

Otwórz *DepartmentsAdd.aspx.cs* i dodaj następującą `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Dodaj następującą zmienną klasy i metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Metody włączania funkcji danych dynamicznych. Obsługa `DropDownList` kontrolki `Init` zdarzeń zapisuje odwołanie do formantu i obsługa `DetailsView` kontrolki `Inserting` zdarzeń używa tego odwołania, aby uzyskać `PersonID` wartości wybranych przez instruktorów i aktualizacji `Administrator` właściwość klucza obcego z `Department` jednostki.

Uruchom stronę, Dodaj informacje dla nowego wydziału, a następnie kliknij **Wstaw** łącza.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Wprowadź wartości dla innego nowego działu. Wprowadź liczbę większą niż 1,000,000.00 w **budżetu** pola i karty do następnego pola. Gwiazdka jest wyświetlana w polu, a jeśli przytrzymasz wskaźnik myszy nad nim możesz zobaczyć komunikat o błędzie, który wprowadzono w metadanych dla tego pola.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Kliknij przycisk **Wstaw**, i zostanie wyświetlony komunikat o błędzie wyświetlany przez `ValidationSummary` u dołu strony.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Następnie zamknij przeglądarkę i Otwórz *Departments.aspx* strony. Dodaj możliwość usuwania *Departments.aspx* stronie przez dodanie `DeleteMethod` atrybutu `ObjectDataSource` kontroli i `DataKeyNames` atrybutu `GridView` kontroli. Znaczniki otwarcia tych formantów, będzie teraz podobne do następującego przykładu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Uruchom stronę.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Usuń działu dodane po uruchomieniu *DepartmentsAdd.aspx* strony.

## <a name="adding-update-functionality"></a>Dodawanie aktualizacji funkcji

Otwórz *SchoolRepository.cs* i dodaj następującą `Update` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Po kliknięciu **aktualizacji** w *Departments.aspx* stronie `ObjectDataSource` kontroli tworzy dwa `Department` jednostek do przekazania do `UpdateDepartment` metody. Jedna z nich zawiera oryginalnych wartości, które były przechowywane w widoku stanu, a drugi zawiera nowe wartości, które zostały wprowadzone w `GridView` kontroli. Kod w `UpdateDepartment` metoda przekazuje `Department` jednostki, która ma oryginalnych wartości, aby `Attach` metody, aby możliwe było nawiązanie śledzenia między jednostką a co znajduje się w bazie danych. Następnie kod przekazuje `Department` jednostki, która ma nowe wartości do `ApplyCurrentValues` metody. Kontekst porównuje stare i nowe wartości. Jeśli nowa wartość jest różna od starej wartości, kontekst zmieni się wartość właściwości. `SaveChanges` Metoda następnie aktualizuje tylko zmienione kolumny w bazie danych. (Jednak, jeśli funkcja aktualizacji dla tej jednostki zostały zmapowane do procedury składowanej, cały wiersz będzie można zaktualizować niezależnie od tego, które zostały zmienione kolumn.)

Otwórz *Departments.aspx* pliku i dodaj następujące atrybuty do `DepartmentsObjectDataSource` sterowania:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Ta powoduje, że stare wartości przechowywane w widoku stanu tak, aby mogą być porównywane z nowymi wartościami w `Update` metody.
- `OldValuesParameterFormatString="orig{0}"`   
 Informuje formant który jest nazwa oryginalnego parametru wartości `origDepartment` .

Kod znaczników dla znaczniku otwierającym elementu `ObjectDataSource` kontroli teraz podobnego do następującego:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Dodaj `OnRowUpdating="DepartmentsGridView_RowUpdating"` atrybutu `GridView` kontroli. Zostanie ona użyta do ustawiania `Administrator` wartość właściwości na podstawie wiersza, użytkownik wybierze się na liście rozwijanej. `GridView` Tagu początkowego teraz podobnego do następującego:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Dodaj `EditItemTemplate` kontrolki `Administrator` kolumny `GridView` kontrolować, natychmiast po `ItemTemplate` kontroli dla tej kolumny:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

To `EditItemTemplate` kontroli jest podobny do `InsertItemTemplate` w kontrolce *DepartmentsAdd.aspx* strony. Różnica polega na to, że początkowa wartość kontrolki jest ustawiona, za pomocą `SelectedValue` atrybutu.

Przed `GridView` Dodaj `ValidationSummary` kontroli, tak jak w *DepartmentsAdd.aspx* strony.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Otwórz *Departments.aspx.cs* a bezpośrednio po deklaracji klasy częściowe, Dodaj następujący kod, aby utworzyć odwołanie do pola prywatnego `DropDownList` sterowania:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Następnie dodaj programy obsługi dla `DropDownList` kontrolki `Init` zdarzeń i `GridView` kontrolki `RowUpdating` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Obsługa `Init` zdarzeń zapisuje odwołanie do `DropDownList` formant pola klasy. Obsługa `RowUpdating` zdarzeń używa odwołania, aby pobrać wartości które użytkownik wprowadził i zastosować je do `Administrator` właściwość `Department` jednostki.

Użyj *DepartmentsAdd.aspx* strony, aby dodać nowy dział, a następnie uruchom *Departments.aspx* strony, a następnie kliknij przycisk **Edytuj** w wierszu, który został dodany.

> [!NOTE]
> Nie można edytować wierszy, które nie zostały dodane (oznacza to, że zostały już w bazie danych), ze względu na nieprawidłowe dane w bazie danych. Administratorzy dla wierszy, które zostały utworzone z bazą danych są studentów. Jeśli spróbujesz edytować jeden z nich, zostanie wyświetlony strona błędu, który zgłasza błąd, taki jak `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Jeśli wprowadzisz nieprawidłowy **budżetu** kwota, a następnie kliknij przycisk **aktualizacji**, zostanie wyświetlony w tej samej gwiazdkę i komunikat o błędzie, który był wyświetlany w *Departments.aspx* strony.

Zmień wartość pola lub wybierz innego administratora i kliknij przycisk **aktualizacji**. Zmiana jest wyświetlany.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Na tym kończy się wprowadzenie do korzystania z `ObjectDataSource` kontroli dla podstawowych operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) operacji przy użyciu platformy Entity Framework. Gdy masz utworzoną prostej aplikacji n warstwowej, ale warstwy logiki biznesowej jest nadal ściśle powiązany z warstwą dostępu do danych, co komplikuje zautomatyzowanych testów jednostkowych. W tym samouczku poniższy zobaczysz sposób implementacji wzorca repozytorium w celu ułatwienia testów jednostkowych.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
