---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 2: Dodawanie warstwy logiki biznesowej i testów jednostkowych | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web firmy Contoso University, utworzony przez wprowadzenie do serii samouczków Entity Framework 4.0. CZY MOGĘ...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6adeb0e49fa754c42bdff6e0bb058f7b73a3f68f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832561"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 2: Dodawanie warstwy logiki biznesowej i testów jednostkowych
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serii samouczków. Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednim samouczku utworzono n warstwową aplikację internetową przy użyciu platformy Entity Framework i `ObjectDataSource` kontroli. W tym samouczku pokazano, jak dodawać logikę biznesową, przy zachowaniu oddzielne warstwy logiki biznesowej (LOGIKI) i warstwy dostępu do danych (DAL) i przedstawia sposób tworzenia zautomatyzowanych testów jednostek dla LOGIKI.

W tym samouczku wykonasz następujące zadania:

- Utwórz interfejs repozytorium, która deklaruje metody dostępu do danych, których potrzebujesz.
- Zaimplementuj interfejs repozytorium w klasie repozytorium.
- Utwórz klasę logikę biznesową, która wywołuje klasę repozytorium do wykonywania funkcji dostępu do danych.
- Połącz `ObjectDataSource` formantu do klasy logikę biznesową, zamiast klasę repozytorium.
- Tworzenie projektu testu jednostkowego i klasa repozytorium, która korzysta z kolekcji w pamięci dla jego magazynu danych.
- Tworzenie testu jednostkowego dla logiki biznesowej, które chcesz dodać do klasy logiki biznesowej, a następnie uruchom test i zobaczyć, jak to się nie powieść.
- Implementują logikę biznesową w klasie logiki biznesowej, a następnie ponownie uruchom jednostki testowania i zobaczyć ją przekazać.

Będziesz pracować *Departments.aspx* i *DepartmentsAdd.aspx* stron, które zostały utworzone w poprzednim samouczku.

## <a name="creating-a-repository-interface"></a>Tworzenie interfejsu repozytorium

Rozpocznie się przez utworzenie interfejsu repozytorium.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

W *DAL* folderu, Utwórz nowy plik klasy, nadaj jej nazwę *ISchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Interfejs definiuje jedną metodę dla każdej operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) metody, które zostały utworzone w klasie repozytorium.

W `SchoolRepository` klasy w *SchoolRepository.cs*, wskazują, że ta klasa implementuje `ISchoolRepository` interfejsu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Tworzenie klasy logiki biznesowej

Następnie utworzysz klasy logiki biznesowej. Można to zrobić, aby dodać logikę biznesową, która będzie wykonywana przez `ObjectDataSource` kontrolować, mimo że nie zrobimy to jeszcze. Na razie nowa klasa logikę biznesową wykona tylko tych samych operacji CRUD, które obsługuje repozytorium.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Utwórz nowy folder i nadaj mu nazwę *LOGIKI*. (W przypadku aplikacji rzeczywistych warstwy logiki biznesowej zazwyczaj będzie można zaimplementować jako bibliotekę klas — oddzielnego projektu — ale w celu uproszczenia w tym samouczku klasy LOGIKI będą znajdować się w folderze projektu.)

W *LOGIKI* folderu, Utwórz nowy plik klasy, nadaj jej nazwę *SchoolBL.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Ten kod tworzy te same metody CRUD, które zostały użyte wcześniej w klasie repozytorium, ale zamiast bezpośrednio dostęp do metod programu Entity Framework, wywoływanych przez nią repozytorium metody klasy.

Zmienna klasy, która zawiera odwołanie do klasy repozytorium jest zdefiniowany jako typ interfejsu i kod, który tworzy wystąpienie klasy repozytorium znajduje się w dwa konstruktory. Konstruktora bez parametrów, które będą używane przez `ObjectDataSource` kontroli. Tworzy wystąpienie `SchoolRepository` klasę, która została utworzona wcześniej. Innego konstruktora umożliwia niezależnie od kodu, która tworzy wystąpienie klasy logikę biznesową, aby przekazać dowolny obiekt, który implementuje interfejs repozytorium.

Metody CRUD, które wywołują, klasę repozytorium i dwa konstruktory umożliwiają korzystanie z klasy logika biznesowa z magazynem danych zaplecza, niezależnie od wybranej. Klasa logiki biznesowej nie musi wiedzieć, jak klasa, która je wywołuje będzie się powtarzał danych. (Jest to często nazywane *nieznajomości trwałości*.) To ułatwia testy jednostkowe, połączyć ze klasy logikę biznesową do implementacji repozytorium, która używa coś prostego w pamięci `List` kolekcji do przechowywania danych.

> [!NOTE]
> Technicznie rzecz biorąc, obiekty jednostki są nadal nie trwałości zakresu, ponieważ są one tworzone z klas dziedziczących z programu Entity Framework `EntityObject` klasy. Nieznajomości pełną trwałości, można użyć *zwykłych starych obiektów CLR*, lub *POCOs*, zamiast obiektów, które dziedziczą z `EntityObject` klasy. Za pomocą POCOs wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji, zobacz [testowania i Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) w witrynie MSDN.)


Teraz możesz łączyć `ObjectDataSource` kontrolek z logiką biznesową klasy zamiast do repozytorium i sprawdź, czy wszystko działa tak jak poprzednio.

W *Departments.aspx* i *DepartmentsAdd.aspx*, zmienić każde wystąpienie `TypeName="ContosoUniversity.DAL.SchoolRepository"` do `TypeName="ContosoUniversity.BLL.SchoolBL`". (Dostępne są cztery wystąpienia we wszystkich).

Uruchom *Departments.aspx* i *DepartmentsAdd.aspx* strony, aby zweryfikować, że nadal działają tak samo jak przed.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Tworzenie projektu testu jednostkowego i implementacji repozytorium

Dodaj nowy projekt do rozwiązania przy użyciu **projekt testowy** szablonu i nadaj mu nazwę `ContosoUniversity.Tests`.

W projekcie testowym Dodaj odwołanie do `System.Data.Entity` i Dodaj odwołanie do `ContosoUniversity` projektu.

Można teraz utworzyć klasę repozytorium, które będzie używane z testami jednostkowymi. Magazyn danych dla tego repozytorium będzie w klasie.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

W projekcie testowym, Utwórz nowy plik klasy, nadaj jej nazwę *MockSchoolRepository.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Ta klasa repozytorium ma te same metody CRUD, która uzyskuje dostęp do programu Entity Framework bezpośrednio, ale pracują z `List` kolekcji w pamięci, a nie z bazą danych. Ułatwia klasy testowej skonfigurować i zweryfikować testów jednostkowych dla klasy logiki biznesowej.

## <a name="creating-unit-tests"></a>Tworzenie testów jednostkowych

**Test** szablonu projektu utworzony klasę testu jednostkowego klasy zastępczej dla Ciebie i kolejnego zadania do modyfikowania tej klasy, dodając do niego metody testów jednostkowych dla logiki biznesowej, które chcesz dodać do klasy logiki biznesowej.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Na uniwersytecie Contoso wszystkie poszczególne przez instruktorów, może być tylko administrator jednego działu, a następnie należy dodać logikę biznesową w celu wymuszają tę regułę. Rozpoczniesz Dodawanie testów, a następnie uruchamiając testy, aby zobaczyć je zakończyć się niepowodzeniem. Następnie dodaj kod i ponownie uruchom testy, oczekiwany będzie je przekazać.

Otwórz *UnitTest1.cs* pliku i Dodaj `using` instrukcji business Logic Apps i dostęp do danych warstw, które utworzono w projekcie ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Zastąp `TestMethod1` metody za pomocą następujących metod:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Metoda tworzy wystąpienie klasy repozytorium, utworzonej dla projektu, który następnie przekazuje do nowego wystąpienia klasy logikę biznesową testów jednostkowych. Metody następnie używa klasy logikę biznesową, aby wstawić trzy działów, których można użyć metod testowych.

Metody testowe Sprawdź klasy logikę biznesową zgłasza wyjątek, jeśli ktoś inny wstawić nowy dział przy użyciu tego samego konta administratora jako istniejące dział lub ktoś próbuje zaktualizować administratora działu, ustawiając dla niej identyfikator osoby kto jest już administratorem innego działu.

Klasy wyjątków nie utworzono jeszcze, dlatego ten kod nie zostanie skompilowany. Aby uzyskać go skompilować, kliknij prawym przyciskiem myszy `DuplicateAdministratorException` i wybierz **Generuj**, a następnie **klasy**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

To tworzy klasę w projekcie testowym, którego można usunąć po utworzeniu klasy wyjątku w głównym projektu. i zaimplementować logikę biznesową.

Uruchamianie projektu testowego. Zgodnie z oczekiwaniami, testy zostaną zakończone niepowodzeniem.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Dodawanie logiki biznesowej, aby przebieg testu

Następnie będzie implementować logikę biznesową, która uniemożliwia ustawiony jako administrator działu osobę, która jest już administratorem innego działu. Będzie zgłoszenie wyjątku z warstwy logiki biznesowej i przechwytywać go w warstwie prezentacji, jeśli użytkownik edytuje dział i klika **aktualizacji** po wybraniu osobą, która jest już administratorem. (Można również usunąć Instruktorzy z listy rozwijanej, którzy są już administratorami, zanim renderowania strony, ale w tym miejscu ma na celu pracy z warstwy logiki biznesowej).

Rozpocznij od utworzenia klasy wyjątku, który będzie throw, gdy użytkownik próbuje się z administratorem działu więcej niż jeden pod kierunkiem instruktora. W głównym projektu Utwórz nowy plik klasy w *LOGIKI* folderu, nadaj jej nazwę *DuplicateAdministratorException.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Teraz usunąć tymczasowy *DuplicateAdministratorException.cs* pliku utworzonego w projekcie testowym wcześniej aby można było skompilować.

W głównym projektu, otwórz *SchoolBL.cs* pliku i dodaj następującą metodę, która zawiera logikę weryfikacji. (Kod odwołuje się do metody, którą utworzysz później).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Wywołasz tę metodę podczas wstawiania lub aktualizowania `Department` jednostki w celu sprawdzenia, czy innego działu, ma już tego samego konta administratora.

Kod wywołuje metodę wyszukiwania bazy danych dla `Department` jednostki, który ma taką samą `Administrator` wartości właściwości jako jednostki są wstawiane lub aktualizowane. Jeśli nie zostanie znalezione, kod zgłasza wyjątek. Sprawdzanie poprawności nie jest wymagany w przypadku, jeśli nie ma jednostki są wstawiane lub aktualizowane `Administrator` wartość i żaden wyjątek jest generowany, jeśli metoda jest wywoływana podczas aktualizacji i `Department` znaleziono dopasowania jednostki `Department` jednostki aktualizowana.

Wywołaj metodę nowe z `Insert` i `Update` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

W *ISchoolRepository.cs*, dodaj następującą deklarację dla nowej metody dostępu do danych:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

W *SchoolRepository.cs*, Dodaj następujący kod `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

W *SchoolRepository.cs*, dodaj następującą nową metodę dostępu do danych:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Ten kod pobiera `Department` obiektów, które mają określoną administratora. Tylko jednego działu powinien można znaleźć (jeśli istnieje). Jednak ponieważ określono ograniczenia jest wbudowana w bazie danych, typ zwracany to kolekcja, w przypadku, gdy znajdują się wiele działów.

Domyślnie jeśli kontekst pobiera jednostki z bazy danych, go przechowuje informacje o ich w jego menedżera stanu obiektu. `MergeOption.NoTracking` Parametr określa, że to śledzenie nie zostanie wykonane dla tego zapytania. Jest to konieczne, ponieważ zapytanie może zwrócić dokładnie jednostki, który próbujesz zaktualizować, a użytkownik nie będzie mógł dołączyć do tej jednostki. Na przykład, jeśli edytujesz dziale historii *Departments.aspx* strony i pozostaw bez zmian przez administratora, to zapytanie spowoduje zwrócenie działu historii. Jeśli `NoTracking` nie jest ustawiona, kontekst będzie już jednostki działu historii w jego menedżera stanu obiektów. Po dołączeniu jednostki działu historii, ponownie utworzona na podstawie stanu widoku kontekst zgłasza wyjątek, który jest wyświetlany komunikat, a następnie `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Zamiast określania `MergeOption.NoTracking`, można utworzyć nowy kontekst obiektu tylko dla tego zapytania. Ponieważ nowy kontekst obiektu będzie mieć swój własny Menedżer stanu obiektu, nie będzie żadnych konfliktów podczas wywoływania `Attach` metody. Nowy kontekst obiektu będzie Udostępnij połączenia metadanych i baza danych z oryginalnego kontekstu obiektu, aby spadek wydajności o tym alternatywnym podejściu będzie niewielki. To podejście pokazano poniżej, jednak wprowadza `NoTracking` opcję znajdującą się przydatne w innych kontekstach. `NoTracking` Opcji zostało omówione w dalszych samouczków w tej serii.)

W projekcie testowym Dodaj nową metodę dostępu do danych w celu *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Ten kod używa LINQ do wykonania tych samych wybór danych, `ContosoUniversity` składnik LINQ to Entities dla korzysta z repozytorium projektu.

Uruchom ponownie projekt testowy. Teraz kod przechodzi testy.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Obsługa wyjątków ObjectDataSource

W `ContosoUniversity` projektu, uruchom *Departments.aspx* strony, a następnie spróbuj zmienić administratora dla działu do osoby, która jest już administratorem innego działu. (Należy pamiętać, że możesz edytować tylko działów dodanych w ramach tego samouczka, ponieważ baza danych zawiera wstępnie załadowane z nieprawidłowe dane). Otrzymasz następującą stronę błąd serwera:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Nie chcesz, aby użytkownikom na wyświetlanie tego rodzaju strony błędu, więc należy dodać kod obsługi błędów. Otwórz *Departments.aspx* i określ program obsługi `OnUpdated` zdarzenia `DepartmentsObjectDataSource`. `ObjectDataSource` Tagu początkowego teraz przypomina poniższy przykład.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

W *Departments.aspx.cs*, Dodaj następujący kod `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Dodaj następujący program obsługi dla `Updated` zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Jeśli `ObjectDataSource` kontroli wyłapuje wyjątek, gdy próbuje wykonać aktualizację, przekazuje wyjątek w argumencie zdarzeń (`e`) do tego programu obsługi. Kod obsługi sprawdza, jeśli wyjątek jest wyjątkiem zduplikowanych administratora. Jeśli tak jest, kod tworzy formant modułu sprawdzania poprawności, który zawiera komunikat o błędzie `ValidationSummary` kontrolka do wyświetlenia.

Uruchom stronę i próbować wykonać ktoś administrator dwóch działów ponownie. Tym razem `ValidationSummary` kontrolka wyświetla komunikat o błędzie.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Wprowadzić zmiany podobne do *DepartmentsAdd.aspx* strony. W *DepartmentsAdd.aspx*, określ program obsługi `OnInserted` zdarzenia `DepartmentsObjectDataSource`. Wynikowy kod znaczników będą podobne do następującego przykładu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

W *DepartmentsAdd.aspx.cs*, dodania tego samego `using` instrukcji:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Dodaj następującą obsługę zdarzeń:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Teraz możesz przetestować *DepartmentsAdd.aspx.cs* stronę, aby sprawdzić, czy również poprawnie obsługuje prób wprowadzenia jedna osoba administrator więcej niż jednego działu.

Na tym kończy się wprowadzenie do implementowania wzorca repozytorium dla przy użyciu `ObjectDataSource` kontrola przy użyciu platformy Entity Framework. Aby uzyskać więcej informacji na temat wzorca repozytorium i testowania, zobacz oficjalny dokument dotyczący MSDN [testowania i Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

W tym samouczku poniższy pokazano, jak dodać sortowanie i filtrowanie funkcji do aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [dalej](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
