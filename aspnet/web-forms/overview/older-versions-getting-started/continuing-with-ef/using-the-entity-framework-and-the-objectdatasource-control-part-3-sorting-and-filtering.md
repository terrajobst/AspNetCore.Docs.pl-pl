---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 3: sortowanie i filtrowanie | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web firmy Contoso University, utworzony przez wprowadzenie do serii samouczków Entity Framework 4.0. CZY MOGĘ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378913"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 3: sortowanie i filtrowanie
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serii samouczków. Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednim samouczku zaimplementowano wzorzec repozytorium w aplikacji n warstwowej w sieci web, która używa programu Entity Framework i `ObjectDataSource` kontroli. W tym samouczku pokazano, jak wykonać sortowanie i filtrowanie i obsługiwać scenariusze wzorzec / szczegół. Należy dodać następujące rozszerzenia *Departments.aspx* strony:

- Pole tekstowe, aby umożliwić użytkownikom na wybór działów według nazwy.
- Lista kursów dla każdego działu, która jest wyświetlana w siatce.
- Możliwość sortowania, klikając nagłówki kolumn.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Dodanie możliwości do sortowanie kolumn GridView

Otwórz *Departments.aspx* strony, a następnie dodaj `SortParameterName="sortExpression"` atrybutu `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`. (Później utworzymy `GetDepartments` metody, która przyjmuje parametr o nazwie `sortExpression`.) Kod znaczników dla znacznika otwierającego formantu teraz przypomina poniższy przykład.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Dodaj `AllowSorting="true"` atrybutu znaczniku otwierającym elementu `GridView` kontroli. Kod znaczników dla znacznika otwierającego formantu teraz przypomina poniższy przykład.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

W *Departments.aspx.cs*, ustaw kolejność sortowania domyślnego, wywołując `GridView` kontrolki `Sort` metody z `Page_Load` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Możesz dodać kod, która sortuje i filtruje klasy logiki biznesowej lub klasę repozytorium. Jeśli możesz zrobić w klasie logiki biznesowej, sortowanie lub filtrowanie pracy zostaną wykonane po pobraniu danych z bazy danych, ponieważ klasa logika biznesowa jest praca z `IEnumerable` obiektu zwróconego przez repozytorium. Jeśli dodasz, sortowanie i filtrowanie kod w klasie repozytorium i zrobisz to przed wyrażenie LINQ lub zapytanie obiektów został przekonwertowany na `IEnumerable` obiektu poleceń będą przekazywane do bazy danych do przetwarzania, co jest zazwyczaj bardziej wydajne. W tym samouczku możesz wdrożyć sortowanie i filtrowanie w sposób, który powoduje, że przetwarzanie do wykonania przez bazę danych — czyli w repozytorium.

Aby dodać możliwość sortowania, należy dodać nową metodę do klasy repozytorium, jak również co klasa logika biznesowa i repozytorium interfejsu. W *ISchoolRepository.cs* Dodaj nową `GetDepartments` metody, która przyjmuje `sortExpression` parametr, który będzie używany do sortować listę działów, który jest zwracany:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Parametr będzie określać kolumnę do sortowania i kierunek sortowania.

Dodaj kod do nowej metody do *SchoolRepository.cs* pliku:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Zmień istniejące bez parametrów `GetDepartments` metodę, aby wywołać nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

W projekcie testowym Dodaj następującą nową metodę do *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Jeśli zostały zamierza utworzyć wszelkie testy jednostkowe, które zależą od ta metoda zwraca posortowaną listę, będziesz potrzebować listę można sortować przed zwróceniem. Użytkownik nie będzie można tworzenie testów Krewny, który w ramach tego samouczka, więc metoda po prostu może zwrócić listę nieposortowane działów.

W *SchoolBL.cs* plików, dodaj następującą nową metodę do klasy logiki biznesowej:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Ten kod przekazuje parametr sortowania do metody repozytorium.

Uruchom *Departments.aspx* strony.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Teraz możesz kliknąć dowolny nagłówek kolumny, aby posortować według tej kolumny. Jeśli kolumna jest już sortowana, klikając nagłówek odwraca kierunek sortowania.

## <a name="adding-a-search-box"></a>Dodawanie pola wyszukiwania

W tej sekcji będzie dodać pole tekstowe wyszukiwania, połączenie jej z `ObjectDataSource` sterować za pomocą parametru formant i dodać metodę do klasy logiki biznesowej, aby obsługiwać filtrowanie.

Otwórz *Departments.aspx* strony, a następnie dodaj następujący kod między nagłówkiem i pierwszy `ObjectDataSource` sterowania:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

W `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`, wykonaj następujące czynności:

- Dodaj `SelectParameters` elementu dla parametru o nazwie `nameSearchString` , które pobiera wartość wprowadzona w `SearchTextBox` kontroli.
- Zmiana `SelectMethod` wartość do atrybutu `GetDepartmentsByName`. (W dalszej części opisano tworzenie tej metody.)

Kod znaczników dla `ObjectDataSource` kontroli teraz podobnego do następującego:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

W *ISchoolRepository.cs*, Dodaj `GetDepartmentsByName` metody, która przyjmuje zarówno `sortExpression` i `nameSearchString` parametry:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

W *SchoolRepository.cs*, dodaj następującą nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Ten kod używa `Where` metodę, aby wybrać elementy zawierające ciąg wyszukiwania. Jeśli ciąg wyszukiwania jest pusta, zostanie wybrana wszystkie rekordy. Należy pamiętać, że po określeniu wywołania metod razem w jednej instrukcji w następujący sposób (`Include`, następnie `OrderBy`, następnie `Where`), `Where` metoda zawsze musi być ostatni.

Zmień istniejące `GetDepartments` metody, która przyjmuje `sortExpression` parametru, aby wywołać nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

W *MockSchoolRepository.cs* w projekcie testowym Dodaj następującą nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

W *SchoolBL.cs*, dodaj następującą nową metodę:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Uruchom *Departments.aspx* stronie, a następnie wprowadź ciąg wyszukiwania, aby upewnić się, że działa logikę wyboru. Pozostaw puste pole tekstowe, a następnie spróbuj wyszukiwania, aby upewnić się, że zwracane są wszystkie rekordy.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Dodawanie kolumny szczegółów dla każdego wiersza siatki

Następnie użytkownik chce zobaczyć wszystkie kursy dla każdego działu wyświetlana w komórce po prawej stronie siatki. Aby to zrobić, użyjesz zagnieżdżoną `GridView` kontroli i powiązać z danymi do danych z `Courses` właściwość nawigacji `Department` jednostki.

Otwórz *Departments.aspx* w znaczniki dla `GridView` kontrolować, określ program obsługi dla `RowDataBound` zdarzeń. Kod znaczników dla znacznika otwierającego formantu teraz przypomina poniższy przykład.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Dodaj nową `TemplateField` elementu po `Administrator` pole szablonu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Ten kod znaczników tworzy zagnieżdżoną `GridView` kontrolka, która pokazuje liczbę kurs i tytuły listę kursów. Nie określa źródło danych, ponieważ należy powiązać z danymi w kodzie w `RowDataBound` programu obsługi.

Otwórz *Departments.aspx.cs* i Dodaj następujący program obsługi dla `RowDataBound` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Ten kod pobiera `Department` jednostki na podstawie argumentów zdarzeń, konwertuje `Courses` właściwość nawigacji do `List` zbierania i databinds zagnieżdżonego `GridView` do kolekcji.

Otwórz *SchoolRepository.cs* pliku i określić wczesne ładowanie dla `Courses` właściwość nawigacji, wywołując `Include` metody w zapytaniu obiektu, który zostanie utworzony w `GetDepartmentsByName` metody. `return` Instrukcji w `GetDepartmentsByName` metoda teraz przypomina poniższy przykład.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Uruchom stronę. Oprócz sortowania i filtrowania możliwości, który dodano wcześniej kontrolki GridView zawiera szczegóły dotyczące zagnieżdżonych kursu dla każdego działu.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Na tym kończy się wprowadzenie do filtrowania, sortowania i wzorzec / szczegół scenariuszy. W następnym samouczku pojawi się, jak obsługiwać współbieżności.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
