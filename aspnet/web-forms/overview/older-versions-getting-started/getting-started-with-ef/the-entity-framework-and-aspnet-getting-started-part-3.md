---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 3 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889257"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET — część 3
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtrowanie, kolejność i grupowanie danych

W poprzednich samouczku użyto `EntityDataSource` formantu, aby wyświetlić i edytować dane. W tym samouczku będzie filtru, kolejności i grupowania danych. Po wykonaniu tej przez ustawienie właściwości `EntityDataSource` formantu, składnia różni się od innych kontrolki źródła danych. Jak można zauważyć, jednak można użyć `QueryExtender` kontroli w celu zminimalizowania tych różnic.

Użytkownik zmieni *Students.aspx* strony do filtrowania dla uczniów lub studentów, sortowanie według nazwy i wyszukiwania dla nazwy. Zostanie również zmienić *Courses.aspx* stronę, aby wyświetlić szkoleń dla wybranego działu i wyszukaj kursów według nazwy. Na koniec zostanie dodana statystyki uczniowie *About.aspx* strony.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Przy użyciu obiektu EntityDataSource "Gdzie" właściwości do filtrowania danych

Otwórz *Students.aspx* strony utworzoną w poprzedniej samouczka. Zgodnie z obecną konfiguracją `GridView` kontrolki na stronie wyświetla wszystkie nazwy z `People` zestawu jednostek. Jednak chcesz pokazać tylko studentów, który można znaleźć po wybraniu `Person` obiektów, które mają daty rejestracji inną niż null.

Przełącz się do **projekt** wyświetlać i wybierać `EntityDataSource` formantu. W **właściwości** ustaw `Where` właściwości `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Składnia w `Where` właściwość `EntityDataSource` formant jest SQL jednostki. Jednostka SQL jest podobny do języka Transact-SQL, ale jest dostosowana do użycia z jednostek, a nie obiekty bazy danych. W wyrażeniu `it.EnrollmentDate is not null`, słowa `it` reprezentuje odwołanie do jednostki zwróconych przez kwerendę. W związku z tym `it.EnrollmentDate` odwołuje się do `EnrollmentDate` właściwość `Person` jednostki który `EntityDataSource` kontrolować zwraca.

Uruchom strony. Lista studentów zawiera teraz tylko studentów. (Nie ma żadnych wierszy, w którym wyświetlana jest data nie rejestracji.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Za pomocą właściwości "OrderBy" obiektu EntityDataSource kolejność danych

Należy zawsze być według nazwy, gdy jest najpierw wyświetlany dla tej listy. Z *Students.aspx* strony jest wciąż otwarty w **projektu** widoku i `EntityDataSource` kontroli jest wybrana, w **właściwości** zestaw okna  **OrderBy** właściwości `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Uruchom strony. Lista studentów jest teraz w kolejności według nazwiska.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Za pomocą parametru formantu można ustawić właściwości "Gdzie"

Zgodnie z innymi kontrolki źródła danych, można podać wartości parametrów do `Where` właściwości. Na *Courses.aspx* strony, że został utworzony w część 2 samouczka, ta metoda służy do wyświetlenia kursów, które są skojarzone z działu, które użytkownik wybiera z listy rozwijanej.

Otwórz *Courses.aspx* i przejdź do **projekt** widoku. Dodaj drugi `EntityDataSource` do strony i nadaj mu nazwę `CoursesEntityDataSource`. Połącz go z `SchoolEntities` modelu, a następnie wybierz `Courses` jako **EntitySetName** wartości.

W **właściwości** okna, kliknij przycisk wielokropka w **gdzie** pole właściwości. (Upewnij się, że `CoursesEntityDataSource` przed użyciem nadal jest wybrana kontrolka **właściwości** okna.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**Edytora wyrażeń** zostanie wyświetlone okno dialogowe. W tym oknie dialogowym wybierz **automatycznego generowania klauzuli Where wyrażenia na podstawie podanych parametrów**, a następnie kliknij przycisk **Dodaj parametr**. Nazwa parametru `DepartmentID`, wybierz pozycję **kontroli** jako **źródło parametru** wartość, a następnie wybierz **DepartmentsDropDownList** jako **ControlID**  wartość.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Kliknij przycisk **Pokaż zaawansowane właściwości**i w **właściwości** okna **edytora wyrażeń** okno dialogowe, zmień `Type` właściwości `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Gdy wszystko będzie gotowe, kliknij przycisk **OK**.

Poniżej listy rozwijanej, Dodaj `GridView` do strony i nadaj mu nazwę `CoursesGridView`. Połącz go z `CoursesEntityDataSource` źródła danych formantu, kliknij przycisk **Odśwież schemat**, kliknij przycisk **Edytowanie kolumn**i Usuń `DepartmentID` kolumny. `GridView` Kontroli znaczników podobnego do następującego.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Gdy użytkownik zmieni się na liście rozwijanej wybranego działu, ma listę skojarzone kursy zmienić automatycznie. Aby to zrobić, wybierz z listy rozwijanej i **właściwości** zestaw okna `AutoPostBack` właściwości `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Teraz, gdy wszystko jest gotowe, przy użyciu narzędzia Projektant, przełącz się do **źródła** wyświetlić i Zastąp `ConnectionString` i `DefaultContainer` nazwy właściwości `CoursesEntityDataSource` sterować za pomocą `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atrybutu. Gdy wszystko będzie gotowe, znaczników dla formantu będzie wyglądać jak w następującym przykładzie.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Uruchom strony i użyj listy rozwijanej, aby wybrać różnych działów. Są wyświetlane tylko kursów dostępnych w ramach wybranego działu `GridView` formantu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Za pomocą właściwości "GroupBy" obiektu EntityDataSource do grupowania danych

Załóżmy, że Contoso University chce wstrzymać statystykami uczniów treści na stronie o. W szczególności potrzebuje do pokazywania podział liczby studentów według daty, które są zarejestrowane.

Otwórz *About.aspx*i w **źródła** wyświetlić, Zastąp istniejącą zawartość elementu `BodyContent` sterować za pomocą "Uczniów treści statystyki" między `h2` tagów:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Po pozycji, Dodaj `EntityDataSource` kontroli i nadaj mu nazwę `StudentStatisticsEntityDataSource`. Połącz go z `SchoolEntities`, wybierz pozycję `People` jednostki ustawić i pozostawić **wybierz** pola kreatora bez zmian. Ustaw następujące właściwości **właściwości** okno:

- Aby filtrować dla uczniów lub studentów tylko, ustaw `Where` właściwości `it.EnrollmentDate is not null`.
- Aby pogrupować wyniki według daty rejestracji, ustaw `GroupBy` właściwości `it.EnrollmentDate`.
- Aby wybrać datę rejestracji i liczby studentów, ustaw `Select` właściwości `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Aby zamówić wyniki według daty rejestracji, ustaw `OrderBy` właściwości `it.EnrollmentDate`.

W **źródła** wyświetlić, Zastąp `ConnectionString` i `DefaultContainer` nazwy właściwości, do których `ContextTypeName` właściwości. `EntityDataSource` Znacznika kontrolki teraz podobnego do następującego.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Składnia `Select`, `GroupBy`, i `Where` właściwości podobny języka Transact-SQL, z wyjątkiem `it` — słowo kluczowe, który określa bieżącego obiektu.

Dodaj następujący kod, aby utworzyć `GridView` formantu, aby wyświetlić dane.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Uruchomić stronę, aby wyświetlić listę zawierającą liczbę studentów według daty rejestracji.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Używanie formantu klasą QueryExtender filtrowania i kolejności

`QueryExtender` Kontrola zapewnia możliwość określenia filtrowanie i sortowanie w znaczniku. Składnia jest niezależna od używany system zarządzania bazami (danych DBMS). Jest również zwykle niezależnie od programu Entity Framework, z wyjątkiem, że składnia używanych w przypadku właściwości nawigacji jest unikatowy dla programu Entity Framework.

W tej części samouczka użyjesz `QueryExtender` formantu do filtrowania i określania kolejności danych, a jedno z pól według będzie właściwości nawigacji.

(Jeśli wolisz użyć kodu zamiast znaczników, aby rozszerzyć zapytań, które są automatycznie generowane przez `EntityDataSource` kontroli, możesz to zrobić obsługi `QueryCreated` zdarzeń. Jest to sposób `QueryExtender` rozszerza kontroli `EntityDataSource` także kontrola zapytań.)

Otwórz *Courses.aspx* strony i poniżej znaczników dodaniu wcześniej, Wstaw następujący kod w celu utworzenia nagłówka, pole tekstowe do wprowadzania ciągów wyszukiwania, przycisk wyszukiwania i `EntityDataSource` formant, który jest powiązany z `Courses` zestawu jednostek.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Zwróć uwagę, że `EntityDataSource` formantu `Include` właściwość jest ustawiona na `Department`. W bazie danych `Course` tabela nie zawiera nazwę działu; zawiera `DepartmentID` kolumny klucza obcego. Jeśli zostały kwerendy bazy danych bezpośrednio, można uzyskać nazwy działu wraz z danymi kursu, trzeba będzie sprzęgać `Course` i `Department` tabel. Przez ustawienie `Include` właściwości `Department`, możesz określić, że programu Entity Framework powinien wykonać pracy pobierania pokrewny `Department` jednostki, gdy otrzymuje `Course` jednostki. `Department` Jednostki są następnie przechowywane w `Department` właściwość nawigacji `Course` jednostki. (Domyślnie `SchoolEntities` klasy, który został wygenerowany przez projektanta modelu danych pobiera dane dotyczące po jest wymagana i kontroli źródła danych został powiązany z tej klasy, więc ustawienie `Include` właściwość nie jest konieczne. Jednak ustawieniem dla niego zwiększa wydajność strony, ponieważ w przeciwnym razie programu Entity Framework spowodowałoby, oddziel wywołania do bazy danych do pobierania danych dla `Course` jednostek i pokrewny `Department` jednostek.)

Po `EntityDataSource` formant został właśnie utworzony, Wstaw następujący kod, aby utworzyć `QueryExtender` formant, który jest powiązany z tym, że `EntityDataSource` formantu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Element określa, czy chcesz wybrać kursy o tytułach zgodnych wartości wprowadzonej w polu tekstowym. Tylko liczby znaków są wprowadzane w polu tekstowym zostaną porównane, ponieważ `SearchType` określa właściwości `StartsWith`.

`OrderByExpression` Element określa, że w skład zestawu wyników będzie uporządkowany według tytuł kursu w dziale nazwa. Zwróć uwagę, jak określono nazwę działu: `Department.Name`. Ponieważ skojarzenie między `Course` jednostki i `Department` jednostka jest jeden do jednego, `Department` zawiera właściwość nawigacji `Department` jednostki. (Jeśli były to relacji jeden do wielu, właściwości czy zawiera kolekcję). Aby uzyskać nazwę działu, należy określić `Name` właściwość `Department` jednostki.

Na koniec należy dodać `GridView` formantu, aby wyświetlić listę kursów:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Pierwsza kolumna jest pole szablonu, który wyświetla nazwę działu. Określa wyrażenie wiązania z danymi `Department.Name`, tak jak przedstawiono w `QueryExtender` formantu.

Uruchom strony. Początkowej wyświetlania zawiera listę wszystkich kursów w kolejności według działów, a następnie według tytuł kursu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Wprowadź "m", a następnie kliknij przycisk **wyszukiwania** aby zobaczyć wszystkie kursy o tytułach rozpoczynać się od "m" (wyszukiwanie jest nie przypadek poufne).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Filtrowanie danych za pomocą operatora "Like"

Podobnie jak efekt można uzyskać `QueryExtender` formantu `StartsWith`, `Contains`, i `EndsWith` wyszukiwania typów przy użyciu `Like` operatora w `EntityDataSource` formantu `Where` właściwości. W tej części samouczka zobaczysz sposób użycia `Like` operatora, aby wyszukać studenta według nazwy.

Otwórz *Students.aspx* w **źródła** widoku. Po `GridView` kontrolować, Dodaj następujący kod:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Ten kod znaczników jest podobny do opisane wcześniej z wyjątkiem `Where` wartości właściwości. Druga część `Where` wyrażenie definiuje wyszukiwania podciągu (`LIKE %FirstMidName% or LIKE %LastName%`) który wyszukuje zarówno imiona i nazwiska na dowolnym została wprowadzona w polu tekstowym.

Uruchom strony. Początkowo dostępne dla Ciebie studentów, ponieważ wartość domyślna dla `StudentName` parametr jest "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

W polu tekstowym wprowadź litery "g", a następnie kliknij przycisk **wyszukiwania**. Zostanie wyświetlona lista studentów, które mają "g" albo imię lub nazwisko.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Możesz już teraz wyświetlane, zaktualizowane filtrowane, uporządkowanych i grupowane dane z poszczególnych tabel. W następnym samouczku będzie rozpocząć pracę z danymi powiązane (scenariusze główny szczegółowy).

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-4.md)
