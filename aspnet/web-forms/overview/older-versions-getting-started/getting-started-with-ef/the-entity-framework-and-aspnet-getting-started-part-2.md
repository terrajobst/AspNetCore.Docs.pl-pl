---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 2 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a549bd62bd78573c368784fd1529a830e009b0d4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET — część 2
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Formant obiektu EntityDataSource

W poprzednich instrukcji utworzono witrynę sieci web, bazy danych i modelu danych. W tym samouczku pracować z `EntityDataSource` kontrolkę udostępniającą ASP.NET w celu ułatwienia pracy z modelem danych programu Entity Framework. Utworzysz `GridView` formantu do wyświetlania i edytowania danych uczniowie `DetailsView` sterowania do dodawania nowych studentów i `DropDownList` formant wyboru działu (który zostanie użyty później do wyświetlania kursy skojarzone).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Należy pamiętać, że w tej aplikacji nie można dodać sprawdzania poprawności danych wejściowych do stron, które aktualizują bazę danych, a niektóre obsługi błędów nie są równie niezawodny, będą wymagane w aplikacji produkcyjnej. Który temu samouczek koncentruje się na platformie Entity Framework i przechowuje ją przed pobraniem zbyt długa. Aby uzyskać więcej informacji o sposobie dodawania tych funkcji do aplikacji, zobacz [sprawdzanie poprawności danych wejściowych użytkownika w programie ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) i [obsługi błędu w stron ASP.NET i aplikacji](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Dodawanie i konfigurowanie sterowania obiektu EntityDataSource

Konfigurując zostaną `EntityDataSource` kontroli odczytać `Person` jednostek z `People` zestawu jednostek.

Upewnij się, że masz program Visual Studio Otwórz i pracujesz z projekt został utworzony w część 1. Jeśli nie zostało to jeszcze skompilować ten projekt, ponieważ utworzono modelu danych lub od momentu ostatniej zmiany wprowadzone do niego, należy teraz skompilować projekt. Zmiany w modelu danych nie są udostępniane do projektanta aż projekt jest budowany.

Tworzenie nowej strony sieci web przy użyciu **formularza sieci Web używający strony wzorcowej** szablonu i nadaj mu nazwę *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Określ *Site.Master* jako strony wzorcowej. Wszystkie strony utworzone następujące samouczki użyje tej strony wzorcowej.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

W **źródła** wyświetlić, dodać `h2` nagłówek do `Content` formantu o nazwie `Content2`, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Z **danych** karcie **przybornika**, przeciągnij `EntityDataSource` do strony, Porzuć go pod nagłówkiem i zmienić identyfikator na `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Przełącz się do **projekt** wyświetlić, kliknij przycisk tagów inteligentnych kontroli źródła danych, a następnie kliknij **skonfiguruj źródło danych** można uruchomić **skonfiguruj źródło danych** kreatora.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

W **skonfigurować ObjectContext** kroku kreatora wybierz **SchoolEntities** jako wartość **połączenia o nazwie**i wybierz **SchoolEntities**jako **DefaultContainerName** wartości. Następnie kliknij przycisk **Dalej**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Uwaga: Jeśli w tym momencie otrzymasz następujące okno dialogowe, należy skompilować projekt przed kontynuowaniem.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

W **Konfigurowanie wyboru danych** krok, wybierz opcję **osób** jako wartość **EntitySetName**. W obszarze **wybierz**, upewnij się, że **wybierz A** ll pole wyboru jest zaznaczone. Następnie wybierz opcje, aby włączyć aktualizacji i usuwania. Gdy wszystko będzie gotowe, kliknij przycisk **Zakończ**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurowanie reguł bazy danych, aby zezwolić na usunięcie

Trzeba utworzyć strony, który pozwala użytkownikom na usuwanie studentów z `Person` tabeli, która ma trzy relacje z innych tabel (`Course`, `StudentGrade`, i `OfficeAssignment`). Domyślnie bazy danych mogą uniemożliwić usunięcie wiersza w `Person` Jeśli istnieją powiązane wiersze w jednej z innych tabel. Można ręcznie usunąć powiązane wiersze najpierw lub można skonfigurować bazy danych do ich usunięcia automatycznie po usunięciu `Person` wiersza. Rekordy uczniów w tym samouczku skonfigurujesz bazy danych, aby automatycznie usuwać powiązanych danych. Ponieważ studentów może mieć powiązanych wiersze tylko w `StudentGrade` tabeli, należy skonfigurować tylko jeden z trzech relacji.

Jeśli używasz *School.mdf* pliku pobranego z projektu, która łączy się z tego samouczka, można pominąć tę sekcję, ponieważ te zmiany w konfiguracji nie zostało jeszcze utworzone. Jeśli bazy danych została utworzona przez uruchomienie skryptu, skonfiguruj bazę danych, wykonując poniższe procedury.

W **Eksploratora serwera**, Otwórz diagram bazy danych, który został utworzony w część 1. Kliknij prawym przyciskiem myszy relację między `Person` i `StudentGrade` (wiersz między tabelami), a następnie wybierz **właściwości**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

W **właściwości** okna, rozwiń węzeł **INSERT i UPDATE specyfikacji** i ustaw **DeleteRule** właściwości **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Zapisz i zamknij diagram. Jeśli zostanie wyświetlone pytanie, czy chcesz zaktualizować bazę danych, kliknij przycisk **tak**.

Aby upewnić się, że model śledzi jednostek, które znajdują się w pamięci w synchronizacji z czynności bazy danych, należy ustawić odpowiednie zasady w modelu danych. Otwórz *SchoolModel.edmx*, kliknij prawym przyciskiem myszy linię skojarzenia między `Person` i `StudentGrade`, a następnie wybierz **właściwości**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

W **właściwości** ustaw **End1 OnDelete** do **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Zapisz i Zamknij *SchoolModel.edmx* pliku, a następnie ponownie skompilować projekt.

Ogólnie rzecz biorąc gdy zmieni się bazy danych, można wybrać kilka opcji jak zsynchronizować modelu:

- Dla niektórych rodzaju zmian (np. Dodawanie lub odświeżanie tabel, widoków lub procedur składowanych), kliknij prawym przyciskiem myszy w Projektancie i wybierz **modelu aktualizacji z bazy danych** do projektanta upewnij te zmiany zostały automatycznie.
- Ponowne generowanie modelu danych.
- Ręczne aktualizowanie podobne do następującego.

W takim przypadku można generowane modelu lub odświeżenia tabel, który wpływa zmiana relacji, ale może następnie będzie musiał ponownie wprowadzić zmianę nazwy pola (z `FirstName` do `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Używanie formantu widoku GridView na odczytywanie i aktualizowanie jednostek

W tej sekcji użyjesz `GridView` formantu, aby wyświetlić, aktualizować lub usuwać studentów.

Otwieranie programu lub przełączanie do *Students.aspx* i przejdź do **projekt** widoku. Z **danych** karcie **przybornika**, przeciągnij `GridView` formantu z prawej strony `EntityDataSource` kontrolować, nadaj jej nazwę `StudentsGridView`kliknij tagów inteligentnych, a następnie wybierz  **StudentsEntityDataSource** jako źródła danych.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Kliknij przycisk **Odśwież schemat** (kliknij **tak** Jeśli zostanie wyświetlony monit o potwierdzenie), następnie kliknij przycisk **włączyć stronicowanie**, **włączyć sortowanie**, **Włącz edytowanie**, i **Włącz usuwanie**.

Kliknij przycisk **Edytuj kolumny**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

W **wybrane pola** pozycję Usuń **PersonID**, **nazwisko**, i **DataZatrudnienia**. Zwykle nie wyświetlasz klucz rekordu dla użytkowników, data zatrudnienia nie jest ważna dla uczniów lub studentów i obie części nazwy będzie umieścić w jednym polu, wystarczy tylko jeden z pola Nazwa.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Wybierz **FirstMidName** pola, a następnie kliknij przycisk **Konwertuj to pole na pole TemplateField**.

Tym samym **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Kliknij przycisk **OK** , a następnie przejdź do **źródła** widoku. Pozostałe zmiany będą prostsze bezpośrednio w znaczniku. `GridView` Kontrolować znaczników teraz wygląda jak w następującym przykładzie.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Pierwsza kolumna po pole polecenia jest polem szablonu, który aktualnie Wyświetla imię. Zmień kod znaczników dla tego pola szablonu wyglądały jak w poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

W trybie wyświetlania dwóch `Label` formanty wyświetlania imię i nazwisko. W trybie edycji dwa pola tekstowe są dostarczane, co może zmienić imię i nazwisko. Jak `Label` trybu wyświetlania formantów, możesz użyć `Bind` i `Eval` wyrażenia dokładnie jako czy z kontrolki źródła danych programu ASP.NET, które łączą się bezpośrednio do bazy danych. Jedyna różnica polega na tym, że określasz właściwości jednostki zamiast kolumnach bazy danych.

Ostatnia kolumna jest polem szablonu jest wyświetlana data rejestracji. Zmień kod znaczników dla tego pola wyglądały jak w poniższym przykładzie:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

W obu wyświetlić i edytować trybie data będzie wyświetlana w formacie "Data krótka" powoduje, że ciąg formatu "{0, d}". (Ten komputer może być skonfigurowany do wyświetlenia tego formatu inaczej niż obrazy ekranu przedstawiona w tym samouczku.)

Zwróć uwagę, że w każdym z tych pól szablonu użyto `Bind` wyrażenia przez domyślny, ale zmieniono do `Eval` wyrażenie w `ItemTemplate` elementów. `Bind` Wyrażenie sprawia, że dane są dostępne w `GridView` właściwości formantu, w razie potrzeby dostępu do danych w kodzie. Na tej stronie nie trzeba dostęp do tych danych w kodzie, dzięki czemu można używać `Eval`, które jest bardziej wydajny. Aby uzyskać więcej informacji, zobacz [pobierania danych z kontroli danych](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Zmiana sterowania obiektu EntityDataSource znaczników do zwiększenia wydajności

W znaczniku dla `EntityDataSource` kontrolować, Usuń `ConnectionString` i `DefaultContainerName` atrybutów i zastąp je za pomocą `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atrybutu. Jest to zmiana należy za każdym razem, gdy utworzysz `EntityDataSource` kontrolować, o ile nie trzeba użyć połączenia, które jest inny niż ten, który jest ustalony w klasie kontekstu obiektu. Przy użyciu `ContextTypeName` atrybut zapewnia następujące korzyści:

- Lepszą wydajność. Gdy `EntityDataSource` formant inicjuje model danych przy użyciu `ConnectionString` i `DefaultContainerName` atrybuty, wykonuje dodatkowej pracy można załadować metadanych na każde żądanie. Nie jest to konieczne, jeśli określono `ContextTypeName` atrybutu.
- Opóźnionego ładowania jest włączona domyślnie w wygenerowanym obiektu kontekstu klasy (takich jak `SchoolEntities` w tym samouczku) w programie Entity Framework 4.0. Oznacza to, że właściwości nawigacji są załadowane dane dotyczące automatycznie po prawej, jeśli zajdzie taka potrzeba. Powolne ładowanie jest szczegółowo w dalszej części tego samouczka.
- Wszystkie dostosowania, które zostały zastosowane do obiektu kontekstu klasy (w tym przypadku `SchoolEntities` klasy) będzie miał dostęp do formantów, które używają `EntityDataSource` formantu. Dostosowywanie klasę obiektu kontekstu jest temat zaawansowany, które nie są ujęte w tym samouczku. Aby uzyskać więcej informacji, zobacz [rozszerzanie typy generowane Framework jednostek](https://msdn.microsoft.com/library/dd456844.aspx).

Kod znaczników będzie teraz wyglądać poniższy przykład (kolejność właściwości mogą być różne):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Odnosi się do funkcji, który był konieczny we wcześniejszych wersjach programu Entity Framework, ponieważ kolumny klucza obcego nie były widoczne jako właściwości jednostki. Bieżąca wersja sprawia, że można użyć *skojarzenia klucza obcego*, co oznacza, że właściwości klucza obcego są widoczne wszystkie elementy oprócz wiele do wielu skojarzeń. Jeśli obiekty mają właściwości klucza obcego i nie [typów złożonych](https://msdn.microsoft.com/library/bb738472.aspx), może narazić ten atrybut na wartość `False`. Nie usunąć atrybut znaczników, ponieważ wartość domyślna to `True`. Aby uzyskać więcej informacji, zobacz [spłaszczanie obiektów (obiektu EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Uruchom strony i wyświetlić listę studentów i pracowników (będzie filtrować dla uczniów lub studentów tylko w następnym samouczku). Imię i nazwisko są wyświetlane razem.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Aby posortować ekran, kliknij nazwę kolumny.

Kliknij przycisk **Edytuj** w dowolnym wierszu. Pola tekstowe są wyświetlane, którym można zmienić imię i nazwisko.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Usunąć** działania również przycisku. Kliknij przycisk Usuń wiersza, który ma datę rejestracji i znika wiersza. (Wierszy bez daty rejestracji reprezentują instruktorów i może wystąpić błąd integralności referencyjnej. W następnym samouczku będzie filtrować tej listy, aby uwzględnić studentów tylko.)

## <a name="displaying-data-from-a-navigation-property"></a>Wyświetlanie danych z właściwości nawigacji

Teraz załóżmy, że chcesz wiedzieć, ile kursów użytkowników jest zarejestrowane w. Entity Framework udostępnia te informacje w `StudentGrades` właściwość nawigacji `Person` jednostki. Ponieważ projektu bazy danych nie zezwalają na uczniów powinny być rejestrowane w kursu bez kategorii, przypisane, w tym samouczku można założyć, że o wiersz `StudentGrade` wiersza tabeli, która jest skojarzona z kursu jest taka sama jak rejestrowane w toku. ( `Courses` Właściwość nawigacji jest przeznaczona tylko dla instruktorów.)

Jeśli używasz `ContextTypeName` atrybutu `EntityDataSource` kontroli programu Entity Framework automatycznie pobiera informacje dla właściwości nawigacji gdy uzyskujesz dostęp do tej właściwości. Ta metoda jest wywoływana *opóźnionego ładowania*. Jednak może to być mało wydajne, ponieważ jej wynikiem wymaga oddzielnego wywołania do bazy danych jest wymagana w każdej chwili dodatkowe informacje. Jeśli potrzebne są dane z właściwości nawigacji dla każdej jednostki zwrócony przez `EntityDataSource` kontroli, jest bardziej wydajne, można pobrać powiązanych danych wraz z jednostki w pojedynczym wywołaniu elementu w bazie danych. Ta metoda jest wywoływana *wczesny ładowania*, i określ wczesny ładowania dla właściwości nawigacji, ustawiając `Include` właściwość `EntityDataSource` formantu.

W *Students.aspx*, ma być wyświetlana liczba kursów dla wszyscy uczniowie, najlepszym rozwiązaniem jest więc wczesny ładowania. Jeśli zostały wyświetlanie wszystkich studentów, ale wyświetlana jest liczba kursy tylko dla niektórych z nich, (które wymagają pisanie kodu oprócz znaczników), opóźnionego ładowania może być lepszym rozwiązaniem.

Otwieranie programu lub przełączanie do *Students.aspx*, przełącz się do **projektu** widok, wybierz opcję `StudentsEntityDataSource`i w **właściwości** zestaw okna **Include**właściwości **StudentGrades**. (Jeśli chcesz pobrać kilka właściwości nawigacji, można określić ich nazwy przecinkami — na przykład **StudentGrades, kursy**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Przełącz się do **źródła** widoku. W `StudentsGridView` formantu po ostatniej `asp:TemplateField` elementu, Dodaj następujące nowe pole szablonu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

W `Eval` wyrażenia mogą odwoływać się właściwość nawigacji `StudentGrades`. Ponieważ ta właściwość zawiera kolekcję, ma `Count` właściwość, która służy do wyświetlenia liczba kursy, w których jest zarejestrowany studenta. W samouczku nowsze zobaczysz sposób wyświetlania danych z właściwości nawigacji, które zawierają pojedyncze jednostki zamiast kolekcji. (Należy pamiętać, że nie można użyć `BoundField` elementy, aby wyświetlić dane z właściwości nawigacji.)

Uruchom strony i pojawi się ile szkoleń dla użytkowników domowych jest zarejestrowane w.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Używanie formantu widoku DetailsView do wstawiania jednostek

Następnym krokiem jest utworzenie strony, która ma `DetailsView` formant, który pozwala dodać nowy studentów. Zamknij przeglądarkę, a następnie utworzyć nowe strony sieci web przy użyciu *Site.Master* strony wzorcowej. Nazwa strony *StudentsAdd.aspx*, a następnie przejdź do **źródła** widoku.

Dodaj następujący kod, aby zastąpić istniejący kod znaczników dla `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Tworzy tego znacznika `EntityDataSource` formant, który jest podobny do tego, który został utworzony w *Students.aspx*, z wyjątkiem umożliwia wstawiania. Jak `GridView` kontrolować pól związanych z `DetailsView` formantu są zakodowane, dokładnie tak jak powinny kontrolki danych, która łączy się bezpośrednio z bazą danych, z wyjątkiem tego, że odwołujących się do właściwości obiektu. W takim przypadku `DetailsView` formant jest używany tylko w przypadku wstawiania wierszy, więc wybrano domyślny tryb `Insert`.

Uruchom strony, a następnie dodaj nowe uczniów.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nic się nie wydarzy po wstawieniu nowych studentów, ale teraz uruchomić *Students.aspx*, pojawi się nowe informacje dla użytkowników domowych.

## <a name="displaying-data-in-a-drop-down-list"></a>Wyświetlanie danych na liście rozwijanej

W poniższych krokach będą databind `DropDownList` formantu, aby ustawić za pomocą jednostki `EntityDataSource` formantu. W tej części samouczka nie są bardzo z tej listy. W kolejnych częściach jednak użyjesz listy umożliwi użytkownikom dział, aby wyświetlić kursy skojarzone z działu.

Tworzenie nowej strony sieci web o nazwie *Courses.aspx*. W **źródła** wyświetlić, dodać nagłówek do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

W **projekt** wyświetlić, dodać `EntityDataSource` formantu do strony tak samo jak przed, z wyjątkiem tym momencie nadaj mu nazwę `DepartmentsEntityDataSource`. Wybierz **działów** jako **EntitySetName** wartość, a następnie wybierz tylko **DepartmentID** i **nazwa** właściwości.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Z **standardowe** karcie **przybornika**, przeciągnij `DropDownList` do strony, nadaj jej nazwę `DepartmentsDropDownList`kliknij tagów inteligentnych i wybierz **wybierz źródło danych** do Uruchom **Kreator konfiguracji źródła danych**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

W **wybierz źródło danych** krok, wybierz opcję **DepartmentsEntityDataSource** jako źródło danych, kliknij przycisk **Odśwież schemat**, a następnie wybierz **nazwa** jako pole danych, aby wyświetlić i **DepartmentID** jako pola wartości danych. Kliknij przycisk **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Metoda dla operacji wiązania danych formantu przy użyciu programu Entity Framework jest taki sam, jak z innymi danymi ASP.NET kontrolki źródła, chyba że zostanie określone, jednostki i właściwości obiektu.

Przełącz się do **źródła** wyświetlanie i dodawanie "Wybierz dział:" bezpośrednio przed `DropDownList` formantu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Dla przypomnienia, Zmień kod znaczników dla `EntityDataSource` formantu w tym momencie, zastępując `ConnectionString` i `DefaultContainerName` atrybutów z `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atrybutu. Często najlepiej czekać po utworzeniu formantu powiązane z danymi, który jest połączony z kontroli źródła danych, przed wprowadzeniem zmian w `EntityDataSource` kontrolować kod znaczników, ponieważ po wprowadzeniu zmiany projektanta nie będą umożliwiać użytkownikowi **odświeżania Schemat** opcji w formancie powiązane z danymi.

Uruchom strony i dział można wybrać z listy rozwijanej.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Na tym kończy się wprowadzenie do korzystania z `EntityDataSource` formantu. Praca z tego formantu zazwyczaj nie różni się od pracy z innymi danymi ASP.NET kontroli źródła, z tą różnicą, że odwołania jednostki i właściwości zamiast tabel i kolumn. Jedynym wyjątkiem jest, aby uzyskać dostęp do właściwości nawigacji. W następnym samouczku zobaczysz, że składnia, można użyć z `EntityDataSource` formant może również różnią się od innych kontrolki źródła danych podczas filtrowanie, grupowanie i kolejność danych.

>[!div class="step-by-step"]
[Poprzednie](the-entity-framework-and-aspnet-getting-started-part-1.md)
[dalej](the-entity-framework-and-aspnet-getting-started-part-3.md)
