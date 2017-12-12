---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 3: filtrowanie i sortowanie | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez wprowadzenie do samouczka serii Entity Framework 4.0. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 3: filtrowanie i sortowanie
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) samouczka serii. Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednich instrukcji zaimplementowana wzorca repozytorium w aplikacji sieci web n warstwowa korzystającej z programu Entity Framework i `ObjectDataSource` formantu. W tym samouczku pokazano, jak wykonać sortowania i filtrowania i obsługiwać scenariusze główny szczegółowy. Należy dodać następujące rozszerzenia *Departments.aspx* strony:

- Pole tekstowe, aby zezwolić użytkownikom na wybór działów według nazwy.
- Lista szkoleń dla każdego działu, który jest widoczny w siatce.
- Możliwość sortować, klikając nagłówki kolumn.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Dodawanie funkcji do sortowania kolumn widoku GridView

Otwórz *Departments.aspx* i dodać `SortParameterName="sortExpression"` atrybutu `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`. (Później utworzysz `GetDepartments` metody pobierającej parametr o nazwie `sortExpression`.) Kod znaczników dla otwierający tag formantu teraz podobnego do następującego.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Dodaj `AllowSorting="true"` atrybut do tagu otwierającego `GridView` formantu. Kod znaczników dla otwierający tag formantu teraz podobnego do następującego.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

W *Departments.aspx.cs*, Ustaw domyślną kolejność sortowania, wywołując `GridView` formantu `Sort` metody z `Page_Load` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

W klasie logiki biznesowej lub klasę repozytorium można dodać kodu sortuje lub filtrów. Jeśli chcesz w klasie logiki biznesowej, sortowanie lub filtrowanie zostanie wykonane po dane są pobierane z bazy danych, ponieważ klasa logika biznesowa jest praca z `IEnumerable` obiektu zwróconego przez repozytorium. Jeśli dodasz sortowania i filtrowania kod w klasie repozytorium i to zrobić przez wyrażenie LINQ lub zapytanie obiektu został przekonwertowany na `IEnumerable` obiekt poleceniach będą przekazywane do bazy danych do przetwarzania, który jest zazwyczaj bardziej wydajne. W tym samouczku można zaimplementować sortowanie i filtrowanie w taki sposób, który powoduje, że przetwarzanie do wykonania przez bazę danych, to znaczy w repozytorium.

Można dodać możliwości sortowania, możesz dodać nową metodę interfejsu repozytorium i klasy repozytorium, jak również dotyczące klasy logiki biznesowej. W *ISchoolRepository.cs* plików, Dodaj nową `GetDepartments` metody pobierającej `sortExpression` parametr, który będzie służyć do sortowania Lista działów, która jest zwracana:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Parametr będzie określać kolumnę do sortowania i kierunek sortowania.

Dodaj kod dla nowej metody, aby *SchoolRepository.cs* pliku:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Zmień istniejące bez parametrów `GetDepartments` metodę do wywołania nowej metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

W projekcie testowym, dodaj następującą metodę nowe do *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Jeśli zostały chce utworzyć żadnych testów, które są zależne od ta metoda zwraca posortowaną listę, konieczne będzie listę można sortować przed zwróceniem. Nie można utworzeniem testy tak jak w tym samouczku, metoda może zwracać tylko nieposortowane listę działów.

W *SchoolBL.cs* plików, dodaj następującą metodę nowej klasy logiki biznesowej:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Ten kod przekazuje sortowania parametru do metody repozytorium.

Uruchom *Departments.aspx* strony.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Można teraz kliknij nagłówek dowolnej kolumny, aby sortować według tej kolumny. Jeśli już jest sortowana kolumna, klikając nagłówek odwraca kierunek sortowania.

## <a name="adding-a-search-box"></a>Dodawanie pola wyszukiwania

W tej sekcji będzie dodanie pola tekstowego wyszukiwania, połącz go do `ObjectDataSource` kontrolować za pomocą parametru kontroli i Dodawanie metody do klasy logiki biznesowej w celu obsługuje filtrowania.

Otwórz *Departments.aspx* strony i Dodaj następujący kod między nagłówek i pierwszy `ObjectDataSource` sterowania:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

W `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`, wykonaj następujące czynności:

- Dodaj `SelectParameters` elementu dla parametru o nazwie `nameSearchString` który pobiera wartość wprowadzona w `SearchTextBox` formantu.
- Zmień `SelectMethod` wartość do atrybutu `GetDepartmentsByName`. (Utworzysz tej metody później.)

Kod znaczników dla `ObjectDataSource` kontroli teraz podobnego do następującego:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

W *ISchoolRepository.cs*, Dodaj `GetDepartmentsByName` metody pobierającej zarówno `sortExpression` i `nameSearchString` parametry:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

W *SchoolRepository.cs*, dodaj następującą metodę nowe:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Ten kod zawiera `Where` metody, aby zaznaczyć elementy, które zawierają ciąg wyszukiwania. Jeśli ciąg wyszukiwania jest pusta, zostanie wybrana wszystkie rekordy. Należy pamiętać, że po określeniu wywołania metod razem w jednej instrukcji następująco (`Include`, następnie `OrderBy`, następnie `Where`), `Where` metody musi zawsze występować na końcu.

Zmień istniejące `GetDepartments` metody pobierającej `sortExpression` parametr, aby wywołać metodę nowe:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

W *MockSchoolRepository.cs* projektu testowego, dodaj następującą metodę nowe:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

W *SchoolBL.cs*, dodaj następującą metodę nowe:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Uruchom *Departments.aspx* strony, a następnie wprowadź ciąg wyszukiwania, aby upewnić się, że logika wybór działa. Pozostaw puste pole tekstowe, a następnie spróbuj wyszukiwania, aby upewnić się, że zwracane są wszystkie rekordy.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Dodawanie kolumny szczegółów dla każdego wiersza siatki

Następnie chcesz zobaczyć wszystkie szkoleń dla każdego działu wyświetlany w prawej komórki siatki. Aby to zrobić, użyjesz zagnieżdżoną `GridView` databind i kontroli do danych z `Courses` właściwość nawigacji `Department` jednostki.

Otwórz *Departments.aspx* i w znaczniku dla `GridView` kontrolować, określ obsługi dla `RowDataBound` zdarzeń. Kod znaczników dla otwierający tag formantu teraz podobnego do następującego.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Dodaj nową `TemplateField` elementu po `Administrator` pole szablonu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Ten kod znaczników tworzy zagnieżdżoną `GridView` formant, który zawiera kursu numer i tytuł listę kursów. Nie określa źródło danych, ponieważ będzie databind w kodzie w `RowDataBound` programu obsługi.

Otwórz *Departments.aspx.cs* i dodaj następujące obsługę `RowDataBound` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Ten kod pobiera `Department` konwertuje jednostki w argumentach zdarzenia `Courses` właściwości nawigacji do `List` kolekcji, a zagnieżdżone databinds `GridView` do kolekcji.

Otwórz *SchoolRepository.cs* pliku i określ wczesny ładowania dla `Courses` właściwość nawigacji, wywołując `Include` metody w zapytaniu obiektów, które są tworzone w `GetDepartmentsByName` — metoda. `return` Instrukcji w `GetDepartmentsByName` metody teraz podobnego do następującego.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Uruchom strony. Oprócz sortowania i filtrowania możliwości dodanego wcześniej kontrolki widoku siatki zawiera szczegóły dotyczące zagnieżdżonych kursu dla każdego działu.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Na tym kończy się wprowadzenie do filtrowania, sortowania i wzorzec szczegół scenariuszy. W następnym samouczku zobaczysz sposób obsługi współbieżności.

>[!div class="step-by-step"]
[Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
