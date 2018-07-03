---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 2 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 476f3e45608bf79a6d2665424eba09cbfccd78fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371106"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 2
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Kontrolka EntityDataSource

W poprzednim samouczku utworzono witrynę sieci web, bazy danych i modelu danych. W tym samouczku możesz pracować `EntityDataSource` kontrolkę udostępniającą platformy ASP.NET w celu ułatwienia pracy z modelem danych platformy Entity Framework. Utworzysz `GridView` sterowania do wyświetlania i edytowania danych student `DetailsView` formantu do dodawania nowych studentów i `DropDownList` kontrolka służąca do wybierania działu, (który zostanie użyty później do wyświetlania kursy skojarzone).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Należy pamiętać, że ta aplikacja nie będzie można Dodawanie sprawdzania poprawności danych wejściowych do stron, które aktualizują bazę danych, a niektóre z obsługą błędów nie będzie równie niezawodny, może być wymagane w aplikacji produkcyjnej. Który utrzymuje samouczek koncentruje się na platformie Entity Framework i utrzymuje je przed pobraniem zbyt długa. Aby uzyskać szczegółowe informacje dotyczące sposobu dodawania tych funkcji do aplikacji, zobacz [sprawdzanie poprawności danych wejściowych użytkownika w produkcie ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) i [obsługę błędów w aplikacjach i stron ASP.NET](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Dodawanie i konfigurowanie kontrolki EntityDataSource

Rozpocznie się, konfigurując `EntityDataSource` kontroli można odczytać `Person` jednostek z `People` zestawu jednostek.

Upewnij się, że masz program Visual Studio, Otwórz i że pracujesz z projektem utworzony w części 1. Jeśli nie został utworzony projekt, ponieważ został utworzony w modelu danych lub od momentu ostatniej zmiany wprowadzone do niego, skompiluj teraz projekt. Zmiany w modelu danych nie są udostępniane do projektanta do momentu projekt jest kompilowany.

Utwórz nową stronę sieci web, używając **formularz sieci Web używający strony wzorcowej** szablonu i nadaj mu nazwę *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Określ *Site.Master* jako strony wzorcowej. Wszystkie strony, tworzone na potrzeby tych samouczków użyje tej strony wzorcowej.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

W **źródła** wyświetlać, dodawać `h2` nagłówek do `Content` formantu o nazwie `Content2`, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Z **danych** karcie **przybornika**, przeciągnij `EntityDataSource` do strony, upuść go pod nagłówkiem i zmień identyfikator, który ma `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Przełącz się do **projektowania** wyświetlić, kliknij tag inteligentny formant źródła danych, a następnie kliknij przycisk **skonfigurować źródło danych** można uruchomić **skonfigurować źródło danych** kreatora.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

W **konfigurowania obiektu ObjectContext** kroku kreatora, wybierz pozycję **SchoolEntities** jako wartość pozycji **połączenia o nazwie**i wybierz **SchoolEntities**jako **DefaultContainerName** wartości. Następnie kliknij przycisk **Dalej**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Uwaga: Jeśli w tym momencie pojawi się następujące okno dialogowe, należy skompilować projekt przed kontynuowaniem.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

W **Konfigurowanie wyboru danych** kroku, wybierz pozycję **osób** jako wartość pozycji **Nazwa zestawu jednostek**. W obszarze **wybierz**, upewnij się, że **wybierz A** ll pole wyboru jest zaznaczone. Następnie wybierz opcje, aby włączyć aktualizację i usuwanie. Gdy wszystko będzie gotowe, kliknij przycisk **Zakończ**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurowanie reguł bazy danych, aby zezwolić na usunięcie

Zostanie utworzona strona, która pozwala użytkownikom na usuwanie uczniów z `Person` tabeli, która ma trzy relacje z innymi tabelami (`Course`, `StudentGrade`, i `OfficeAssignment`). Domyślnie baza danych uniemożliwi usuwanie wierszy w `Person` Jeśli istnieją powiązane wiersze w jednej z innych tabel. Można ręcznie usunąć powiązane wiersze najpierw lub można skonfigurować bazy danych, aby usunąć je automatycznie, gdy usuniesz `Person` wiersza. Dla rekordów dla uczniów w ramach tego samouczka należy skonfigurować bazy danych, aby automatycznie usuwać powiązanych danych. Ponieważ uczniowie mogą powiązanych wierszy tylko w `StudentGrade` tabeli, należy skonfigurować tylko jeden z trzech relacji.

Jeśli używasz *School.mdf* plik został pobrany z projektu, który łączy się z tego samouczka, możesz pominąć tę sekcję, ponieważ te zmiany konfiguracji zostały już wykonane. Jeśli utworzono bazę danych przez uruchomienie skryptu Skonfiguruj bazę danych przy użyciu poniższej procedury.

W **Eksploratora serwera**, Otwórz diagram bazy danych, który został utworzony w części 1. Kliknij prawym przyciskiem myszy relację między `Person` i `StudentGrade` (linię między tabelami), a następnie wybierz pozycję **właściwości**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

W **właściwości** okna, rozwiń węzeł **INSERT i UPDATE specyfikacji** i ustaw **DeleteRule** właściwości **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Zapisz i zamknij diagram. Jeśli zostanie wyświetlony monit, czy chcesz zaktualizować bazę danych, kliknij przycisk **tak**.

Aby upewnić się, że model utrzymuje jednostek, które znajdują się w pamięci jest zsynchronizowany z działania bazy danych, należy ustawić odpowiednie zasady w modelu danych. Otwórz *SchoolModel.edmx*, kliknij prawym przyciskiem myszy linię skojarzenia między `Person` i `StudentGrade`, a następnie wybierz pozycję **właściwości**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

W **właściwości** oknie **End1 OnDelete** do **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Zapisz i Zamknij *SchoolModel.edmx* pliku, a następnie ponownie skompilować projekt.

Ogólnie rzecz biorąc po zmianie bazy danych, masz kilka opcji sposobu zsynchronizować modelu:

- Dla niektórych rodzajów zmian (np. dodanie lub odświeżenie tabel, widoków i procedur składowanych), kliknij prawym przyciskiem myszy projektanta i wybierz pozycję **Model aktualizacji z bazy danych** scalać zmian projektanta wykonującego automatycznie.
- Ponowne generowanie modelu danych.
- Dokonanie ręcznych aktualizacji podobny do poniższego.

W takim przypadku może zostać wygenerowane ponownie modelu lub odświeżenia tabel dotyczy zmiana relacji, ale następnie trzeba ponownie wprowadzić zmianę nazwy pola (z `FirstName` do `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Używanie kontrolki GridView na odczytywanie i aktualizowanie jednostek

W tej sekcji użyjesz `GridView` kontrolki na wyświetlanie, aktualizowanie lub usuwanie studentów.

Otwieranie programu lub przełączanie na *Students.aspx* i przełącz się do **projektowania** widoku. Z **danych** karcie **przybornika**, przeciągnij `GridView` kontroli po prawej stronie `EntityDataSource` , nazwij ją `StudentsGridView`, kliknij tag inteligentny, a następnie wybierz  **StudentsEntityDataSource** jako źródło danych.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Kliknij przycisk **odświeżania schematu** (kliknij **tak** po wyświetleniu monitu o potwierdzenie), następnie kliknij przycisk **Włączanie stronicowania**, **włączyć sortowanie**, **Włącz edytowanie**, i **Włącz usuwanie**.

Kliknij przycisk **Upravit Sloupce**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

W **wybrane pola** polu, Usuń **PersonID**, **LastName**, i **DataZatrudnienia**. Użytkownik zwykle nie są wyświetlane klucz rekordu dla użytkowników, data zatrudnienia nie jest ważna dla uczniów i studentów i obie części nazwy zostanie umieszczony w jednym polu, wystarczy tylko jedno z pól nazwy.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Wybierz **FirstMidName** pola, a następnie kliknij przycisk **Konwertuj to pole na TemplateField**.

Tym samym **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Kliknij przycisk **OK** a następnie przejdź na **źródła** widoku. Pozostałe zmiany będzie można łatwiej wykonać bezpośrednio w znacznikach. `GridView` Kontrolować znaczników teraz wygląda następująco.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Pierwsza kolumna po pole polecenia jest polem szablon, który aktualnie Wyświetla imię. Zmień adiustację tak, dla tego pola szablonu wyglądał jak na poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

W trybie wyświetlania dwóch `Label` formanty wyświetlania imię i nazwisko. W trybie edycji dwóch pól tekstowych są udostępniane, dzięki czemu można zmienić imię i nazwisko. Podobnie jak w przypadku `Label` kontrolek w tryb wyświetlania, należy użyć `Bind` i `Eval` wyrażeń dokładnie w przypadku usługi ASP.NET kontrolki źródła danych, które łączą się bezpośrednio do bazy danych. Jedyna różnica polega na określać właściwości jednostki zamiast kolumny bazy danych.

Ostatnia kolumna jest pole szablonu, który wyświetla datę rejestracji. Zmień adiustację tak, dla tego pola wyglądał jak na poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

W obu należy wyświetlić i edytować tryb, w formacie ciągu "{0, d}" powoduje, że data jest wyświetlana w formacie "Data krótka". (Komputer może być skonfigurowany do wyświetlania tego formatu, inaczej niż obrazy ekranu wyświetlane w ramach tego samouczka).

Należy zauważyć, że w każdym z tych pól szablonu, projektanta używane `Bind` wyrażenia przez domyślne, ale zostało zmienione, aby `Eval` wyrażenia w `ItemTemplate` elementów. `Bind` Wyrażenie sprawia, że dane są dostępne w `GridView` właściwości formantu, w razie potrzeby dostęp do danych w kodzie. Na tej stronie nie trzeba dostęp do tych danych w kodzie, aby można było używać `Eval`, co jest bardziej wydajne. Aby uzyskać więcej informacji, zobacz [Trwa pobieranie danych z formantów danych](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Zmiana sterowania EntityDataSource znaczników w celu zwiększenia wydajności

W znaczniku dla `EntityDataSource` kontrolować, Usuń `ConnectionString` i `DefaultContainerName` atrybutów, a zastępuje je `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atrybutu. Jest to zmiana, należy za każdym razem, gdy tworzysz `EntityDataSource` kontrolować, o ile nie trzeba używać połączenia innego niż ten, który jest ustalony w klasie kontekstu obiektu. Za pomocą `ContextTypeName` atrybut zapewnia następujące korzyści:

- Lepsza wydajność. Gdy `EntityDataSource` kontroli jest inicjowana przy użyciu modelu danych `ConnectionString` i `DefaultContainerName` atrybutów, wykonuje dodatkowej pracy można załadować metadanych na każde żądanie. Nie jest to konieczne, jeśli określisz `ContextTypeName` atrybutu.
- Powolne ładowanie jest włączona domyślnie w klasach kontekstu generowanych obiektów (takich jak `SchoolEntities` w ramach tego samouczka) w programie Entity Framework 4.0. Oznacza to, że właściwości nawigacji są ładowane z powiązanych danych automatycznie bezpośrednio w przypadku, gdy ich potrzebujesz. Powolne ładowanie jest omówione bardziej szczegółowo w dalszej części tego samouczka.
- Wszelkie dostosowania, które zostały zastosowane do obiektu klasy kontekstu (w tym przypadku `SchoolEntities` klasy) będzie dostępna dla kontrolki używające `EntityDataSource` kontroli. Dostosowywanie obiektu klasy kontekstu jest zaawansowane tematu, który nie pasuje do w tej serii samouczków. Aby uzyskać więcej informacji, zobacz [rozszerzanie typy generowane Framework jednostek](https://msdn.microsoft.com/library/dd456844.aspx).

Znaczniki teraz będą podobne do następującego przykładu, (kolejność właściwości mogą się różnić):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Atrybut odwołuje się do funkcji, który był konieczny we wcześniejszych wersjach programu Entity Framework, ponieważ kolumny klucza obcego nie były widoczne jako właściwości jednostki. Bieżąca wersja sprawia, że można użyć *skojarzenia klucza obcego*, co oznacza, że właściwości klucza obcego są widoczne wszystkie elementy oprócz wiele do wielu skojarzeń. Jeśli obiekty mają właściwości klucza obcego i nie [typy złożone](https://msdn.microsoft.com/library/bb738472.aspx), możesz pozostawić ten atrybut ustawiony na `False`. Nie usunąć atrybut znaczników, ponieważ wartość domyślna to `True`. Aby uzyskać więcej informacji, zobacz [spłaszczanie obiektów (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Uruchom stronę i wyświetlić listę uczniów i pracowników (będzie filtrowania dla uczniów i studentów tylko w następnym samouczku). Imię i nazwisko są wyświetlane razem.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Aby posortować ekran, kliknij nazwę kolumny.

Kliknij przycisk **Edytuj** w dowolnym wierszu. Pola tekstowe są wyświetlane, w którym można zmienić imię i nazwisko.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Usuń** działania również przycisku. Kliknij przycisk Usuń, dla wiersza zawierającego Data rejestracji i wiersza zniknie. (Wiersze bez daty rejestracji reprezentują instruktorów i może wystąpić błąd integralności referencyjnej. W następnym samouczku będziesz filtrowania tej listy w celu uwzględnienia studentów po prostu.)

## <a name="displaying-data-from-a-navigation-property"></a>Wyświetlanie danych za pomocą właściwości nawigacji

Teraz załóżmy, że chcesz wiedzieć, ile kursów każdego ucznia jest zarejestrowane w. Entity Framework udostępnia te informacje w `StudentGrades` właściwość nawigacji `Person` jednostki. Projekt bazy danych nie jest dozwolone uczniów do rejestracji w szkoleniu bez potrzeby przedsiębiorstw, przypisane, w tym samouczku można założyć, że o wiersz w `StudentGrade` wiersza tabeli, który jest skojarzony z kurs jest taka sama jak rejestrowane w ramach tego kursu. ( `Courses` Właściwość nawigacji jest przeznaczony tylko dla instruktorów.)

Kiedy używasz `ContextTypeName` atrybutu `EntityDataSource` kontrolki, platformy Entity Framework automatycznie pobiera informacje dla właściwości nawigacji podczas dostępu do tej właściwości. Jest to nazywane *powolne ładowanie*. Jednak może to być mało wydajne, ponieważ powoduje to oddzielne wywołania do bazy danych, potrzebne są dodatkowe informacje o każdej czasie. Jeśli potrzebne są dane z właściwości nawigacji dla każdej jednostki zwrócony przez `EntityDataSource` formantu, jest bardziej wydajne, można pobrać powiązanych danych, oraz jednostki w pojedynczym wywołaniu do bazy danych. Jest to *wczesne ładowanie*, a następnie określ wczesne ładowanie dla właściwości nawigacji, ustawiając `Include` właściwość `EntityDataSource` kontroli.

W *Students.aspx*, aby wyświetlić liczbę kursy dla każdego ucznia, więc wczesne ładowanie jest najlepszym wyborem. Jeśli zostały wyświetlanie wszystkich uczniów, ale którym wyświetlana jest liczba kursy tylko w przypadku niektórych z nich, (które wymagałyby pisanie kodu oprócz znaczników), powolne ładowanie może być lepszym rozwiązaniem.

Otwieranie programu lub przełączanie na *Students.aspx*, przełącz się do **projektu** widoku, wybierz opcję `StudentsEntityDataSource`i w **właściwości** zestaw okna **Include**właściwość **StudentGrades**. (Jeśli trzeba pobrać kilka właściwości nawigacji, można określić ich nazwy przecinkami — na przykład **StudentGrades, kursy**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Przełącz się do **źródła** widoku. W `StudentsGridView` kontroli po ostatnim `asp:TemplateField` elementu, Dodaj następujące nowe pole szablonu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

W `Eval` wyrażenia, możesz odwoływać się do właściwości nawigacji `StudentGrades`. Ponieważ ta właściwość zawiera kolekcję, ma `Count` właściwość, która służy do wyświetlania liczby kursów, w których jest zarejestrowany dla uczniów. Później w samouczku pokazano, jak do wyświetlania danych z właściwości nawigacji, które zawierają pojedyncze jednostki zamiast kolekcji. (Należy pamiętać, że nie można użyć `BoundField` elementy, aby wyświetlić dane z właściwości nawigacji.)

Uruchomienia strony, a teraz widać, ile kursy dla uczniów jest zarejestrowane w.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Wstaw jednostki za pomocą kontrolki widoku szczegółów

Następnym krokiem jest utworzenie strony, która ma `DetailsView` formant, który umożliwia dodawanie nowych studentów. Zamknij przeglądarkę, a następnie utwórz nową stronę sieci web, używając *Site.Master* strony wzorcowej. Nazwij stronę *StudentsAdd.aspx*, a następnie przejdź na **źródła** widoku.

Dodaj następujący kod, aby zastąpić istniejący kod znaczników dla `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Ten kod znaczników tworzy `EntityDataSource` formant, który jest podobny do tego, który został utworzony w *Students.aspx*, z wyjątkiem umożliwia wstawiania. Podobnie jak w przypadku `GridView` kontrolować powiązanych pól `DetailsView` kontrolki są kodowane dokładnie w takiej postaci, w jaki byłyby dla kontrolki danych, która łączy się bezpośrednio z bazą danych, z tą różnicą, że odwołują się do właściwości jednostki. W tym przypadku `DetailsView` formant jest używany tylko w przypadku wstawiania wierszy, dlatego ustawiono na domyślny tryb `Insert`.

Uruchom stronę i dodać nowego studenta.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nic się nie stanie po wstawieniu nowego Studenta, ale teraz uruchomić *Students.aspx*, zostaną wyświetlone nowe informacje dla uczniów.

## <a name="displaying-data-in-a-drop-down-list"></a>Wyświetlanie danych na liście rozwijanej

W poniższych krokach będziesz elementu databind `DropDownList` formantu do jednostki można ustawić przy użyciu `EntityDataSource` kontroli. W tej części samouczka nie zrobi, bardzo z tej listy. W kolejnych częściach jednak użyjesz listy umożliwiające użytkownikom wybór działu do wyświetlenia kursy skojarzony z działu.

Utwórz nową stronę sieci web o nazwie *Courses.aspx*. W **źródła** wyświetlać, dodawać nagłówek, aby `Content` formant, który nosi nazwę `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

W **projektowania** wyświetlać, dodawać `EntityDataSource` kontrolki na stronie jak poprzednio, jednak tym razem nadaj mu nazwę `DepartmentsEntityDataSource`. Wybierz **działów** jako **Nazwa zestawu jednostek** wartości, a następnie wybierz tylko **DepartmentID** i **nazwa** właściwości.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Z **standardowa** karcie **przybornika**, przeciągnij `DropDownList` do strony, nadaj jej nazwę `DepartmentsDropDownList`kliknij tag inteligentny i wybierz **wybierz źródło danych** do Rozpocznij **Kreatora konfiguracji źródła danych**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

W **wybierz źródło danych** kroku, wybierz pozycję **DepartmentsEntityDataSource** jako źródło danych, kliknij przycisk **odświeżania schematu**, a następnie wybierz pozycję **nazwa** jako pole danych, aby wyświetlić i **DepartmentID** jako wartość pola danych. Kliknij przycisk **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Metoda formant jest używany do elementu databind używający narzędzia Entity Framework jest taki sam, jak z innymi danymi ASP.NET kontrolki źródła, z wyjątkiem możesz określać jednostki i właściwości jednostki.

Przełącz się do **źródła** wyświetlić i dodać "Wybierz dział:" bezpośrednio przed `DropDownList` kontroli.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Przypominamy, zmień adiustację tak, aby uzyskać `EntityDataSource` kontrolki, w tym momencie, zastępując `ConnectionString` i `DefaultContainerName` atrybuty atrybutem `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atrybutu. Często najlepiej poczekaj, aż po jej utworzeniu formant powiązany z danymi, który jest połączony z kontroli źródła danych, zanim będzie można zmienić `EntityDataSource` kontrolować znaczników, ponieważ po wprowadzeniu zmiany, Projektant nie udostępni Ci **odświeżania Schemat** opcji w kontrolce powiązanych z danymi.

Uruchom stronę i dział można wybrać z listy rozwijanej.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Na tym kończy się wprowadzenie do korzystania z `EntityDataSource` kontroli. Praca z tej kontrolki jest zazwyczaj nie różni się od pracy z innymi danymi ASP.NET kontroli źródła, z tą różnicą, że odwołania jednostki i właściwości zamiast tabel i kolumn. Jedynym wyjątkiem jest, gdy chcesz uzyskać dostęp do właściwości nawigacji. W następnym samouczku zobaczysz, czy składnia możesz za pomocą `EntityDataSource` kontroli może też różnią się od innych formantów źródła danych podczas filtrowania, grupowania i kolejność danych.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-3.md)
