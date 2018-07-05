---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 3 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 51964f86e66e21d63105f1043641c0096d1d41b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391150"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 3
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtrowanie, porządkowanie i grupowanie danych

W poprzednim samouczku użyto `EntityDataSource` formantu, aby wyświetlić i edytować dane. W tym samouczku będziesz filtrować, kolejność i grupowania danych. Gdy to zrobisz, ustawiając właściwości `EntityDataSource` kontrolki, składnia różni się od innych formantów źródła danych. Jak można zauważyć, jednak można użyć `QueryExtender` kontroli w celu zminimalizowania tych różnic.

Poznasz, jak zmienić *Students.aspx* strony, aby filtrować dla uczniów i studentów, sortowanie według nazwy i nazwy funkcji przeszukiwania. Zostanie również zmienić *Courses.aspx* na stronie kursy dla wybranego działu i wyszukaj kursów według nazwy. Na koniec należy dodać statystyki dla uczniów, aby *About.aspx* strony.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Przy użyciu EntityDataSource "Gdzie" właściwości do filtrowania danych

Otwórz *Students.aspx* strony, który został utworzony w poprzednim samouczku. Zgodnie z obecną konfiguracją `GridView` kontrolki na stronie wyświetla wszystkie nazwy z `People` zestawu jednostek. Jednak mają być wyświetlane tylko uczniowie, który można znaleźć, wybierając `Person` obiektów, które mają daty rejestracji inną niż null.

Przełącz się do **projektowania** wyświetlać i wybierać `EntityDataSource` kontroli. W **właściwości** oknie `Where` właściwość `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Składnia, możesz użyć w `Where` właściwość `EntityDataSource` formant jest jednostki SQL. Jednostka SQL jest podobna do instrukcji języka Transact-SQL, ale jest ono dostosowane do użytku z jednostki, a nie obiekty bazy danych. W wyrażeniu `it.EnrollmentDate is not null`, wyraz `it` reprezentuje odwołanie do jednostek zwróconych przez zapytanie. W związku z tym `it.EnrollmentDate` odwołuje się do `EnrollmentDate` właściwość `Person` jednostki, `EntityDataSource` kontrolować zwraca.

Uruchom stronę. Listy studentów zawiera teraz tylko uczniowie. (Nie ma żadnych wierszy, w którym wyświetlana jest Brak daty rejestracji.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Przy użyciu właściwości "OrderBy" EntityDataSource, aby dane dotyczące zamówień

Możesz też tej listy, aby być w kolejności nazwa pojawi się najpierw. Za pomocą *Students.aspx* strony jest wciąż otwarty w **projektowania** widoku i `EntityDataSource` kontroli wybrana, w **właściwości** zestaw okna  **OrderBy** właściwość `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Uruchom stronę. Listy studentów jest teraz w kolejności według nazwiska.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Za pomocą parametru kontroli można ustawić właściwości "Gdzie"

Zgodnie z innymi formantami źródła danych można przekazać wartości parametru, aby `Where` właściwości. Na *Courses.aspx* strony, czy został utworzony w części 2 samouczka, ta metoda służy do wyświetlania kursy, które są skojarzone z działu, który użytkownik wybiera z listy rozwijanej.

Otwórz *Courses.aspx* i przełącz się do **projektowania** widoku. Dodaj drugą `EntityDataSource` do strony i nadaj mu nazwę `CoursesEntityDataSource`. Połącz go `SchoolEntities` modelu, a następnie wybierz `Courses` jako **Nazwa zestawu jednostek** wartość.

W **właściwości** okna, kliknij przycisk wielokropka w **gdzie** okno właściwości. (Upewnij się, że `CoursesEntityDataSource` kontroli nadal zaznaczone przed użyciem **właściwości** okna.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**Edytora wyrażeń** zostanie wyświetlone okno dialogowe. W tym oknie Wybierz **automatycznego generowania gdzie wyrażenia na podstawie podanych parametrów**, a następnie kliknij przycisk **Dodaj parametr**. Nazwa parametru `DepartmentID`, wybierz opcję **kontroli** jako **źródło parametru** wartości, a następnie wybierz **DepartmentsDropDownList** jako **ControlID**  wartość.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Kliknij przycisk **Pokaż zaawansowane właściwości**, a następnie w **właściwości** okna **edytora wyrażeń** okno dialogowe, zmiana `Type` właściwość `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Gdy wszystko będzie gotowe, kliknij przycisk **OK**.

Poniżej listy rozwijanej Dodaj `GridView` do strony i nadaj mu nazwę `CoursesGridView`. Połącz go `CoursesEntityDataSource` kontroli źródła danych kliknij **odświeżania schematu**, kliknij przycisk **Edytowanie kolumn**i Usuń `DepartmentID` kolumny. `GridView` Kodu znaczników kontrolki przypomina poniższy przykład.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Gdy użytkownik zmieni wybranego działu, na liście rozwijanej, ma listę skojarzone kursy, aby zmienić automatycznie. Aby to zrobić, wybierz z listy rozwijanej i **właściwości** zestaw okna `AutoPostBack` właściwość `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Teraz, gdy jesteś gotowy, za pomocą projektanta, przełącz się do **źródła** wyświetlanie i Zastąp `ConnectionString` i `DefaultContainer` nazwy właściwości `CoursesEntityDataSource` kontrolką `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atrybutu. Po wykonaniu tych czynności kodu znaczników kontrolki będzie wyglądać jak w poniższym przykładzie.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Uruchom strony i użyj listy rozwijanej, aby wybrać różne działy. Tylko kursy, które są oferowane przez dział wybrane są wyświetlane w `GridView` kontroli.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Przy użyciu właściwości "GroupBy" EntityDataSource do grupowania danych

Załóżmy, że Contoso University chce umieścić statystykami treści uczniów na jego temat strony. W szczególności chce wyświetlić podział liczby studentów według daty, które są zarejestrowane.

Otwórz *About.aspx*i w **źródła** wyświetlanie, zastąpić istniejącą zawartość elementu `BodyContent` kontrolką "Student treści statystyki" między `h2` tagi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Po nagłówku, Dodaj `EntityDataSource` kontroli i nadaj mu nazwę `StudentStatisticsEntityDataSource`. Połącz go `SchoolEntities`, wybierz opcję `People` jednostki zestawu i pozostawić **wybierz** pola kreatora bez zmian. Ustaw następujące właściwości **właściwości** okna:

- Aby filtrować tylko uczniowie, ustaw `Where` właściwość `it.EnrollmentDate is not null`.
- Aby pogrupować wyniki według daty rejestracji, należy ustawić `GroupBy` właściwość `it.EnrollmentDate`.
- Aby wybrać datę rejestracji i liczby studentów, należy ustawić `Select` właściwość `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Aby uporządkować wyniki według daty rejestracji, należy ustawić `OrderBy` właściwość `it.EnrollmentDate`.

W **źródła** wyświetlanie, Zastąp `ConnectionString` i `DefaultContainer` nazwy właściwości `ContextTypeName` właściwości. `EntityDataSource` Kodu znaczników kontrolki teraz przypomina poniższy przykład.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Składnia `Select`, `GroupBy`, i `Where` właściwości przypomina język Transact-SQL, z wyjątkiem `it` — słowo kluczowe, który określa bieżąca jednostka.

Dodaj następujący kod, aby utworzyć `GridView` formantu, aby wyświetlić dane.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Uruchom stronę, aby zobaczyć listę, na którym wyświetlana jest liczba uczniów według daty rejestracji.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Używanie formantu QueryExtender filtrowania i kolejność

`QueryExtender` Kontrola zapewnia możliwość określenia filtrowania i sortowania w znacznikach. Składnia jest niezależny od system zarządzania bazy danych (DBMS) jest używany. Jest również zwykle niezależnie od programu Entity Framework, z wyjątkiem, że składnia, których używasz dla właściwości nawigacji jest unikatowy w programie Entity Framework.

W tej części samouczka użyjesz `QueryExtender` formantu do filtrowania i określania kolejności danych, a jedno z pól w klauzuli order by pojawią się właściwości nawigacji.

(Jeśli wolisz używać kodu zamiast znaczników rozszerzyć zapytania, które są automatycznie generowane przez `EntityDataSource` kontrolki, możesz to zrobić, obsługa `QueryCreated` zdarzeń. Jest to sposób, w jaki `QueryExtender` rozszerza formant `EntityDataSource` również kontrolować zapytań.)

Otwórz *Courses.aspx* strony, a poniżej znaczników, wcześniej dodany, Wstaw następujące znaczniki, aby utworzyć nagłówek, pole tekstowe do wprowadzania wyszukiwania ciągów, przycisk Wyszukaj, a `EntityDataSource` formant, który jest powiązany z `Courses` zestawu jednostek.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Należy zauważyć, że `EntityDataSource` kontrolki `Include` właściwość jest ustawiona na `Department`. W bazie danych `Course` tabela nie zawiera nazwy działu; zawiera on `DepartmentID` kolumna klucza obcego. Jeśli zostały zapytanie do bazy danych bezpośrednio, aby uzyskać nazwę działu wraz z danymi kurs, trzeba dołączyć `Course` i `Department` tabel. Ustawiając `Include` właściwości `Department`, należy określić, że platformy Entity Framework powinien wykonywać pracę wprowadzenie powiązane `Department` jednostki, gdy otrzyma `Course` jednostki. `Department` Jednostki są następnie zapisywane w `Department` właściwość nawigacji `Course` jednostki. (Domyślnie `SchoolEntities` klasy, który został wygenerowany przez projektanta modeli danych pobiera powiązanych danych, jest to konieczne, gdy kontrola źródła danych został powiązany z tej klasy, dlatego ustawiając `Include` właściwość nie jest konieczne. Jednak ustawienie zwiększa wydajność strony, ponieważ w przeciwnym razie Entity Framework byłaby oddzielne wywołania do bazy danych do pobierania danych dla `Course` jednostek i powiązane `Department` jednostek.)

Po `EntityDataSource` formant został właśnie utworzony, Wstaw następujące znaczniki, aby utworzyć `QueryExtender` formant, który jest powiązany z tym, że `EntityDataSource` kontroli.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Element określa, że do wybranych kursów o tytułach zgodnych wartość wprowadzona w polu tekstowym. Tylko liczby znaków są wprowadzane w polu tekstowym zostaną porównane, ponieważ `SearchType` właściwość określa `StartsWith`.

`OrderByExpression` Element określa, że zestaw wyników będzie uporządkowane według tytułu kursów w ciągu nazwy działu. Zwróć uwagę, jak określono nazwę działu: `Department.Name`. Ponieważ skojarzenie między `Course` jednostki i `Department` jednostka jest jeden do jednego, `Department` zawiera właściwość nawigacji `Department` jednostki. (Jeśli były to relacji jeden do wielu, właściwość będzie zawierać kolekcji). Aby uzyskać nazwę działu, należy określić `Name` właściwość `Department` jednostki.

Na koniec należy dodać `GridView` formantu, aby wyświetlić listę kursów:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Pierwsza kolumna jest pole szablonu, który wyświetla nazwę działu. Określa wyrażenie wiążące dane `Department.Name`tak samo jak opisany w `QueryExtender` kontroli.

Uruchom stronę. Ekran początkowy to lista wszystkie kursy w kolejności według działu, a następnie według tytułu kursu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Wprowadź "m", a następnie kliknij przycisk **wyszukiwania** aby zobaczyć wszystkie kursy o tytułach zaczynają się od "m" (wyszukiwanie jest nie zamierzone, Zapisz wielkość liter).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Filtrowanie danych przy użyciu operatora "Like"

Podobnie jak efekt możesz osiągnąć `QueryExtender` kontrolki `StartsWith`, `Contains`, i `EndsWith` wyszukiwania typów przy użyciu `Like` operatora w `EntityDataSource` kontrolki `Where` właściwości. W tej części samouczka pokazano, jak używać `Like` operator wyszukiwania dla uczniów lub studentów według nazwy.

Otwórz *Students.aspx* w **źródła** widoku. Po `GridView` i Dodaj następujący kod:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Ten kod znaczników jest podobny do opisane wcześniej z wyjątkiem `Where` wartości właściwości. Druga część `Where` wyrażenie definiuje wyszukiwania podciągu (`LIKE %FirstMidName% or LIKE %LastName%`) który wyszukuje zarówno imię i nazwisko w nazwach niezależnie od rodzaju został wprowadzony w polu tekstowym.

Uruchom stronę. Początkowo będzie widoczny wszystkich uczniów, ponieważ wartość domyślna `StudentName` parametr jest "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Wprowadź literę "g" w polu tekstowym, a następnie kliknij przycisk **wyszukiwania**. Zobaczysz listę uczniów, które mają "g", albo imię lub nazwisko.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Już teraz wyświetlany, zaktualizować, filtrowane, uporządkowane i pogrupowanych danych z poszczególnych tabel. W następnym samouczku będzie rozpocząć pracę z danymi powiązanych (wzorzec / szczegół scenariusze).

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-4.md)
