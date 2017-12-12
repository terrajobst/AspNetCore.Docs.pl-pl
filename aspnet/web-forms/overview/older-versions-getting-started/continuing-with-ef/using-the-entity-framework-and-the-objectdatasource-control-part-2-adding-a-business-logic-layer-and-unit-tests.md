---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: "Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 2: Dodawanie warstwy logiki biznesowej i testów jednostkowych | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez wprowadzenie do samouczka serii Entity Framework 4.0. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 0440f807c7baa7b92e5f05590eca9cc237b5aef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 2: Dodawanie warstwy logiki biznesowej i testów jednostkowych
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) samouczka serii. Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednich samouczek utworzono aplikację sieci web n warstwowa przy użyciu programu Entity Framework i `ObjectDataSource` formantu. W tym samouczku przedstawiono sposób dodawania logiki biznesowej podczas oddzieleniu warstwy logiki biznesowej (logiki warstwy Biznesowej) oraz warstwa dostępu do danych (DAL), a widoczny jest sposób tworzenia testów jednostkowych automatycznych dla logiki warstwy Biznesowej.

W tym samouczku będziesz wykonywanie następujących zadań:

- Tworzenie interfejsu repozytorium, który deklaruje metody dostępu do danych, które są potrzebne.
- Zaimplementuj interfejs repozytorium w klasie repozytorium.
- Utwórz klasę logiki biznesowej, który wywoła klasę repozytorium do wykonywania funkcji dostępu do danych.
- Połącz `ObjectDataSource` formantu do klasy logiki biznesowej, a nie klasę repozytorium.
- Tworzenie projektu testu jednostkowego i klasę repozytorium, która używa kolekcje w pamięci, do jego magazynu danych.
- Tworzenie testu jednostkowego dla logiki biznesowej, które mają zostać dodane do klasy logiki biznesowej, a następnie uruchom test i zobaczyć ją zakończyć się niepowodzeniem.
- Wdrożyć logikę biznesową w klasie logiki biznesowej, a następnie ponownie uruchom jednostki testu i zobaczyć ją przekazać.

Będzie współpracować *Departments.aspx* i *DepartmentsAdd.aspx* stron, które zostały utworzone w poprzednich instrukcji.

## <a name="creating-a-repository-interface"></a>Tworzenie interfejsu w repozytorium

Zostaną przez tworzenie interfejsu w repozytorium.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

W *DAL* folder, Utwórz nowy plik klasy, nadaj jej nazwę *ISchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Interfejs definiuje jedną metodę dla każdego CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) metod, które zostały utworzone w klasie repozytorium.

W `SchoolRepository` klasy w *SchoolRepository.cs*, wskazuje, że ta klasa implementuje `ISchoolRepository` interfejsu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Tworzenie klasy logika biznesowa

Następnie utworzysz klasy logiki biznesowej. Można to zrobić, w którym można dodawać logiki biznesowej, które będą wykonywane przez `ObjectDataSource` kontrolować, mimo że nie będzie który jeszcze zrobić. Teraz nową klasę logiki biznesowej tylko będzie wykonywać operacji CRUD, które jest repozytorium.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Utwórz nowy folder i nadaj mu nazwę *logiki warstwy Biznesowej*. (W rzeczywistych aplikacjach, warstwy logiki biznesowej zazwyczaj są realizowane jako biblioteki klas — oddzielny projekt, ale do tego samouczka Zachowaj proste, klas logiki warstwy Biznesowej będą znajdować się w folderze projektu.)

W *logiki warstwy Biznesowej* folder, Utwórz nowy plik klasy, nadaj jej nazwę *SchoolBL.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Ten kod tworzy tych samych metod CRUD, który został wyświetlony wcześniej w klasie repozytorium, ale zamiast bezpośrednio dostęp do metody Entity Framework, wywołuje repozytorium metody klasy.

Zmienna klasy, która zawiera odwołanie do klasy repozytorium jest zdefiniowany jako typ interfejsu i kod, który tworzy wystąpienie klasy repozytorium znajduje się w dwóch konstruktorów. Konstruktor bez parametrów, które będą używane przez `ObjectDataSource` formantu. Tworzy wystąpienie `SchoolRepository` klasy, który został utworzony wcześniej. Inne Konstruktor umożliwia niezależnie od kodu, który tworzy wystąpienie klasy logiki biznesowej do przekazania w każdym obiekcie, który implementuje interfejs repozytorium.

Metody CRUD, które wywołują klasę repozytorium i dwa konstruktory umożliwiają użycie klasy logiki biznesowej z dowolnego magazynu danych zaplecza, możesz wybrać. Klasa logiki biznesowej nie musi wiedzieć, jak klasy, która wywołuje ona będzie się powtarzał danych. (Jest to często nazywane *nieznajomości trwałości*.) Ułatwia testowanie, jednostki, ponieważ klasa logiki biznesowej można nawiązać implementację repozytorium, która używa coś jako prosty jako w pamięci `List` kolekcje do przechowywania danych.

> [!NOTE]
> Z technicznego punktu widzenia obiekt jednostki są nadal nie trwałości ignorujących, ponieważ są one utworzone z klasy, które dziedziczą z programu Entity Framework `EntityObject` klasy. Nieznajomości pełną trwałości, można użyć *zwykły stare obiekty CLR*, lub *POCOs*, zamiast obiektów, które dziedziczą z `EntityObject` klasy. Przy użyciu POCOs wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji, zobacz [testowania i Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx) w witrynie MSDN.)


Teraz możesz połączyć `ObjectDataSource` formantów z logiką biznesową klasa zamiast do repozytorium i sprawdź, czy wszystko działa tak jak poprzednio.

W *Departments.aspx* i *DepartmentsAdd.aspx*, zmienić każde wystąpienie `TypeName="ContosoUniversity.DAL.SchoolRepository"` do `TypeName="ContosoUniversity.BLL.SchoolBL`". (Istnieją cztery wystąpienia we wszystkich.)

Uruchom *Departments.aspx* i *DepartmentsAdd.aspx* strony, aby zweryfikować, że nadal działają tak samo jak przed.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Tworzenie projektu testu jednostkowego i implementacja repozytorium

Dodawanie nowego projektu do rozwiązania przy użyciu **projekt testowy** szablonu i nadaj mu nazwę `ContosoUniversity.Tests`.

W projekcie testowym Dodaj odwołanie do `System.Data.Entity` i Dodaj odwołanie projektu do `ContosoUniversity` projektu.

Można teraz utworzyć klasę repozytorium, które będzie używane przy użyciu testów jednostkowych. Magazyn danych to repozytorium będzie należące do klasy.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

W projekcie testowym, Utwórz nowy plik klasy, nadaj jej nazwę *MockSchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Ta klasa repozytorium ma te same metody CRUD, która uzyskuje bezpośredni dostęp do programu Entity Framework, ale pracować z `List` kolekcji w pamięci, a nie z bazy danych. Ułatwia dla klasy testowej, konfigurowania i sprawdzania poprawności testów jednostkowych dla klasy logiki biznesowej.

## <a name="creating-unit-tests"></a>Tworzenie testów jednostkowych

**Test** szablonu projektu utworzony stub klasę testów jednostkowych i kolejnego zadania się modyfikowanie tej klasy, dodając do niego metody testowe jednostki dla logiki biznesowej, które mają zostać dodane do klasy logiki biznesowej.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Na uniwersytecie Contoso wszystkie poszczególne instruktora może być tylko administrator jednego działu i konieczne jest dodanie logiki biznesowej, aby wymusić tę regułę. Rozpoczęcia przez dodanie testy i uruchamiania testów, aby zobaczyć je zakończyć się niepowodzeniem. Następnie będzie Dodaj kod i ponownie uruchom testy, oczekiwana ich przekazać.

Otwórz *UnitTest1.cs* plik i dodać `using` instrukcje firm logikę i dane — dostęp warstw utworzonych w projekcie ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Zastąp `TestMethod1` metody za pomocą następujących metod:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Metoda tworzy wystąpienie klasy repozytorium utworzonej dla projektu, który następnie przekazuje do nowego wystąpienia klasy logiki biznesowej testu jednostkowego. Metoda następnie używa do klasy logiki biznesowej Wstaw trzy działów, których można użyć w metodach.

Sprawdź metod klasy logiki biznesowej zgłasza wyjątek, jeśli ktoś spróbuje Wstaw nowy dział z tego samego konta administratora, jak istniejące działu lub jeśli ktoś spróbuje zaktualizować administratora działu ustawiając go na identyfikator osoby kto jest już administrator innego działu.

Klasy wyjątków nie utworzono jeszcze, dlatego ten kod nie zostanie skompilowany. Aby pobrać go skompilować, kliknij prawym przyciskiem myszy `DuplicateAdministratorException` i wybierz **Generuj**, a następnie **klasy**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

To tworzy klasę w projekcie testowym, które można usunąć po utworzeniu klasy wyjątków w projekcie głównym. i implementacji logiki biznesowej.

Uruchom projekt testowy. Zgodnie z oczekiwaniami, testy zostaną zakończone niepowodzeniem.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Dodawanie logiki biznesowej, aby przebiegu testowego

Następnie będzie wdrożyć logikę biznesową, uniemożliwiający można ustawić jako administratora działu osoby, która jest już administratora innego działu. Zostanie zgłoszenia wyjątku z warstwy logiki biznesowej oraz catch go w warstwie prezentacji, jeśli użytkownik edytuje działu i klika **aktualizacji** po wybraniu osoby, która jest już uprawnienia administratora. (Można również usunąć instruktorów z listy rozwijanej, którzy są już Administratorzy przed renderowanie strony, ale w tym miejscu ma na celu pracy z warstwy logiki biznesowej).

Rozpocznij od utworzenia klasy wyjątków, który będzie zgłaszać, gdy użytkownik próbuje nawiązać instruktora administratora działu więcej niż jeden. W projekcie głównym, Utwórz nowy plik klasy w *logiki warstwy Biznesowej* folderu, nadaj jej nazwę *DuplicateAdministratorException.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Teraz usunąć tymczasowy *DuplicateAdministratorException.cs* utworzony plik w projekcie testowym wcześniej aby można było skompilować.

W projekcie głównym, należy otworzyć *SchoolBL.cs* pliku i dodaj następującą metodę, która zawiera logikę weryfikacji. (Kod odwołuje się do metody, która będzie utworzyć później).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Ta metoda będzie jest wywoływana podczas wstawiania lub aktualizowania `Department` jednostek w celu sprawdzenia, czy innego działu ma już tego samego konta administratora.

Kod wywołuje metodę wyszukiwania bazy danych dla `Department` jednostki, który ma taką samą `Administrator` wartości właściwości jako jednostki są wstawiane lub aktualizowane. Jeśli został znaleziony, kod zgłasza wyjątek. Bez sprawdzania poprawności jest wymagany, jeśli nie ma jednostki są wstawiane lub aktualizowane `Administrator` wartość i żaden wyjątek jest zgłaszany, jeśli metoda jest wywoływana podczas aktualizacji i `Department` znaleziono dopasowań jednostki `Department` jednostki aktualizowana.

Wywołaj metodę nowego z `Insert` i `Update` metod:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

W *ISchoolRepository.cs*, Dodaj następujące oświadczenie dla nowej metody dostępu do danych:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

W *SchoolRepository.cs*, Dodaj następujący `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

W *SchoolRepository.cs*, dodaj następującą metodę dostępu do danych nowego:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Ten kod pobiera `Department` obiektów, które mają określony administratora. Tylko jeden dział powinien można znaleźć (jeśli istnieje). Jednak bez ograniczenia są wbudowane w bazie danych, typ zwrotny jest kolekcją w przypadku znalezienia wielu działów.

Domyślnie gdy kontekst pobiera jednostki z bazy danych, jej przechowuje informacje o ich w jego Menedżer stanu obiektu. `MergeOption.NoTracking` Parametr określa, że to śledzenie nie zostanie wykonane dla tego zapytania. Jest to konieczne, ponieważ zapytanie może zwrócić dokładne jednostki, który próbujesz zaktualizować, a następnie nie będzie mógł dołączyć jednostkę. Na przykład edytować dział historii *Departments.aspx* strony i pozostawić bez zmian przez administratora, to zapytanie spowoduje zwrócenie działu historii. Jeśli `NoTracking` nie jest ustawione, kontekst już miałoby jednostki działu historii w jego Menedżer stanu obiektu. Po dołączeniu jednostki działu historii ponownie utworzona na podstawie stanu widoku, kontekst spowoduje zgłoszenie wyjątku z informacją, a następnie `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Zamiast określania `MergeOption.NoTracking`, można utworzyć nowy kontekst obiektu tylko dla tego zapytania. Ponieważ nowy kontekst byłyby własną Menedżer stanu obiektu, nie byłoby nie było konfliktu podczas wywoływania `Attach` metody. Nowy kontekst czy udostępnianie metadanych i bazy danych połączenia oryginalnego kontekst, więc spadek wydajności tego podejścia alternatywnego jest niewielka. Podejście pokazane, jednak wprowadza `NoTracking` opcję znajdującą się przydatne w innych kontekstach. `NoTracking` Opcja została szczegółowo opisana w późniejszym samouczku z tej serii.)

W projekcie testowym Dodawanie nowej metody dostępu do danych do *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Ten kod zawiera LINQ do wykonania tego samego wyboru danych który `ContosoUniversity` repozytorium projekt używa składnika LINQ to Entities dla.

Uruchom ponownie projekt testowy. Teraz testy zostały zaliczone pomyślnie.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Obsługa wyjątków w elemencie ObjectDataSource

W `ContosoUniversity` projektu, uruchom *Departments.aspx* strony, a następnie spróbuj zmienić administratora dla działu do osoby, która jest już administratora do innego działu. (Należy pamiętać, że można edytować tylko działów, które zostały dodane w tym samouczku, ponieważ w bazie danych jest wstępnie zainstalowany z nieprawidłowych danych). Można uzyskać stronie błąd serwera:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Nie chcesz, aby użytkownicy widzieli tego rodzaju strony błędu, należy dodać kodu obsługi błędu. Otwórz *Departments.aspx* i określ obsługi dla `OnUpdated` zdarzenie `DepartmentsObjectDataSource`. `ObjectDataSource` Tagu początkowego teraz podobnego do następującego.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

W *Departments.aspx.cs*, Dodaj następujący `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Dodaj następujące obsługę `Updated` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Jeśli `ObjectDataSource` kontroli przechwytuje Wystąpił wyjątek podczas próby przeprowadzenia aktualizacji, przekazuje wyjątek w argumencie zdarzenia (`e`) do tego programu obsługi. Kod obsługi sprawdza, jeśli wyjątek jest wyjątek zduplikowane administratora. Jeśli tak jest, kod tworzy kontrolkę modułu sprawdzania poprawności, która zawiera komunikat o błędzie `ValidationSummary` kontrolka do wyświetlenia.

Uruchom strony i nawiązania ktoś administrator dwóch działów ponownie. Teraz `ValidationSummary` kontroli wyświetla komunikat o błędzie.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Podobne zmiany do *DepartmentsAdd.aspx* strony. W *DepartmentsAdd.aspx*, określ obsługi dla `OnInserted` zdarzenie `DepartmentsObjectDataSource`. Wynikowa znaczników będą podobne do następującego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

W *DepartmentsAdd.aspx.cs*, dodania tego samego `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Dodaj następujące programu obsługi zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Teraz możesz przetestować *DepartmentsAdd.aspx.cs* stronę, aby zweryfikować również poprawnie obsługi prób wprowadzenia jedną osobę administratora działu więcej niż jeden.

Na tym kończy się wprowadzenie do implementacji wzorca repozytorium dla przy użyciu `ObjectDataSource` kontrolki z programu Entity Framework. Aby uzyskać więcej informacji na temat wzorca repozytorium i testowania, zobacz oficjalny dokument MSDN [testowania i Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx).

W samouczku następujące zobaczysz sposób dodawania sortowania i filtrowania funkcje do aplikacji.

>[!div class="step-by-step"]
[Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[dalej](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
