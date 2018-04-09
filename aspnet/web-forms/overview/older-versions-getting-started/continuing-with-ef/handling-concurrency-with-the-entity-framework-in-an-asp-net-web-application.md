---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Obsługa współbieżności Entity Framework 4.0 w aplikacji ASP.NET 4 Web | Dokumentacja firmy Microsoft
author: tdykstra
description: Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez wprowadzenie do samouczka serii Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Obsługa współbieżności Entity Framework 4.0 w aplikacji ASP.NET 4 sieci Web
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) samouczka serii. Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednich samouczka przedstawiono sposób sortowania i filtrowania danych przy użyciu `ObjectDataSource` kontroli i programu Entity Framework. W tym samouczku przedstawiono opcje obsługi współbieżność w aplikacji sieci web ASP.NET, która korzysta z programu Entity Framework. Spowoduje utworzenie nowej strony sieci web przeznaczonych do aktualizowania instruktora office przypisania. Problemy ze współbieżnością na tej stronie i na stronie działów, który został utworzony wcześniej będzie obsługiwać.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Konfliktom współbieżności

Występuje konflikt współbieżności, gdy jeden użytkownik edytuje rekordu, a inny użytkownik edytuje ten sam rekord przed zapisaniem zmian pierwszego użytkownika do bazy danych. Jeśli nie ustawisz programu Entity Framework wykryć takie konflikty, kto aktualizuje bazę danych ostatniego zastępuje zmiany wprowadzone przez użytkownika. W wielu aplikacjach dopuszczalny jest to zagrożenie, a nie trzeba skonfigurować aplikację do obsługi konfliktom współbieżności możliwe. (Jeśli istnieją w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie są naprawdę krytyczne, jeśli pewne zmiany zostaną zastąpione, kosztów programowania dla współbieżności może przeważają korzyści.) Jeśli nie trzeba martwić konfliktom współbieżności, możesz pominąć ten samouczek; pozostałe dwie samouczkach z tej serii nie zależą od coś kompilacji w tym obiekcie.

### <a name="pessimistic-concurrency-locking"></a>Pesymistyczne współbieżności (blokowanie)

Jeśli aplikacja potrzeba uniknąć przypadkowej utraty danych w scenariuszach współbieżności, jeden sposób jest użycie blokady bazy danych. Ta metoda jest wywoływana *współbieżności pesymistyczne*. Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub aktualizacji dostępu. Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub aktualizacji dostępu, ponieważ uzyskują kopii danych, która jest w trakcie zmieniane. Jeśli zablokujesz wiersz dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokad ma niektóre wady. Może być skomplikowane, aby program. Wymaga to znaczące bazy danych zarządzania zasobami i może spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa (to znaczy go nie jest dobrze skalowalna). Z tego względu nie wszystkie systemy zarządzania bazy danych obsługują pesymistyczne współbieżności. Entity Framework nie zapewnia wbudowanej obsługi dla niego, a w tym samouczku nie pokazuje, jak ją wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistycznej współbieżności

Alternatywą dla pesymistyczne współbieżności jest *optymistycznej współbieżności*. Optymistycznej współbieżności oznacza stosowanie konfliktom współbieżności mieć miejsce, a następnie reaguje odpowiednio Jeśli. Na przykład Jan uruchamia *Department.aspx* strony, kliknięć **Edytuj** łączy dla działu historii i zmniejsza **budżetu** kwota 1,000,000.00 $ $ 125,000.00. (Jan zarządza dział konkurencyjnych i chce zwolnić pieniędzy własnej działu).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Zanim Jan klika **aktualizacji**, Magdalena uruchamia tej samej stronie, klika **Edytuj** link historii działu, a następnie zmiany **Data rozpoczęcia** pola do 1/1/1/10/2011 1999. (Magdalena zarządza dział historii i chce nadaj starszeństwa więcej).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Jan klika **aktualizacji** najpierw, Magdalena klika przycisk **aktualizacji**. Wyświetla teraz przeglądarki Joanny **budżetu** kwota jako $1,000,000.00, ale jest niepoprawna, ponieważ ilość został zmieniony przez Jan do $125,000.00.

Niektóre akcje, które można wykonać w tym scenariuszu obejmują:

- Można zachować informacje o właściwości, które zostało zmodyfikowane przez użytkownika i aktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie byłoby utracone, ponieważ inne właściwości zostały zaktualizowane przez użytkowników. Przy następnym przegląda ktoś z działu historii, zobaczą 1/1/1999 i 125,000.00 $. 

    Jest to domyślne zachowanie w programie Entity Framework, a jego znacznie zmniejszyć liczbę konfliktów, które może spowodować utratę danych. Jednak to zachowanie nie uniknąć utraty danych, jeśli konkurują zmian z tą samą właściwością jednostki. Ponadto to zachowanie nie jest zawsze możliwe; Mapowanie procedur składowanych do typu jednostki, wszystkie właściwości obiektu są aktualizowane wszystkie jednostki zmiany w bazie danych.
- Możesz pozwolić, aby zastąpić zmiany w Jan zmiany nazwy. Po kliknięciu Magdalena **aktualizacji**, **budżetu** kwota wraca do 1,000,000.00 $. Ta metoda jest wywoływana *klienta Wins* lub *ostatniego w usłudze Wins* scenariusza. (Wartości klienta wyższy priorytet niż co znajduje się w magazynie danych).
- Aby uniemożliwić zmianę nazwy aktualizację w bazie danych. Zazwyczaj będzie wyświetlony komunikat o błędzie, wyświetlić jej bieżący stan danych i umożliwia jej ponownie wprowadzić swoje zmiany, jeśli chce nadal były. Dodatkowo można zautomatyzować proces przez zapisanie jej danych wejściowych i zapewnieniu jej możliwość Zastosuj je ponownie bez konieczności ponownego wprowadzania go. Ta metoda jest wywoływana *Wins magazynu* scenariusza. (Wartości magazynu danych mają priorytet nad wartości przesłany przez klienta).

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

W ramach jednostki można rozwiązać konflikty Obsługa `OptimisticConcurrencyException` wyjątki, które generuje programu Entity Framework. Aby wiedzieć, kiedy throw te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym musisz skonfigurować bazę danych i modelu danych odpowiednio. Niektóre opcje umożliwiających wykrywanie konfliktów są następujące:

- W bazie danych obejmują kolumnie tabeli, która może służyć do określania, kiedy wiersz został zmieniony. Następnie można skonfigurować programu Entity Framework w celu uwzględnienia tej kolumny `Where` klauzuli SQL `Update` lub `Delete` poleceń.

    Jest celem `Timestamp` kolumny w `OfficeAssignment` tabeli.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Typ danych miary `Timestamp` kolumny jest również nazywany `Timestamp`. Jednak kolumny faktycznie nie zawiera wartości daty i godziny. Zamiast tego wartość jest numerem sekwencyjnym, który jest zwiększany po każdej wiersz jest aktualizowany. W `Update` lub `Delete` polecenia `Where` klauzula zawiera oryginalny `Timestamp` wartość. Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `Timestamp` różni się od oryginalnej wartości, więc `Where` klauzula zwraca żadnego wiersza do aktualizacji. Gdy programu Entity Framework stwierdzi, że żadne wiersze nie zostały zaktualizowane przez bieżący `Update` lub `Delete` polecenia (Jeśli liczba wierszy, których dotyczy to zero), interpretuje który jako konflikt współbieżności.
- Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości wszystkich kolumn w tabeli w `Where` klauzuli `Update` i `Delete` poleceń.

    Jak pierwsza opcja, jeśli dowolny wiersz zmieniła się od najpierw odczytano wiersza `Where` klauzuli nie zwrócą wiersza do aktualizacji, które programu Entity Framework interpretowane jako konflikt współbieżności. Ta metoda jest tak skuteczne, jak za pomocą `Timestamp` pól, ale może być mało wydajne. Dla tabel bazy danych, które mają wiele kolumn, może to spowodować bardzo dużych `Where` klauzule, i w aplikacji sieci web ich wymaga, aby zachować stan dużych ilości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji ponieważ go wymaga zasoby serwera (na przykład stan sesji) lub muszą być zawarte w tej strony (na przykład stan widoku).

W tym samouczku spowoduje dodanie obsługi błędów dla konfliktów optymistycznej współbieżności dla jednostki, która nie ma właściwości śledzenia ( `Department` jednostek) oraz jednostki, która ma właściwość śledzenia ( `OfficeAssignment` jednostek).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Obsługa optymistycznej współbieżności bez właściwości śledzenia

Aby zaimplementować optymistycznej współbieżności dla `Department` jednostki, która nie ma śledzenia (`Timestamp`) właściwość spowoduje wykonanie następujących zadań:

- Zmień model danych, aby włączyć śledzenie współbieżności `Department` jednostek.
- W `SchoolRepository` klasy wyjątków współbieżności dojście w `SaveChanges` metody.
- W *Departments.aspx* strony obsługi wyjątków współbieżności poprzez wyświetlenie komunikatu do użytkownika, ostrzeżenie, że próba zmiany nie powiodły. Użytkownik może następnie zobacz bieżące wartości i spróbuj ponownie zmiany, jeśli są nadal potrzebne.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Włączanie śledzenia w modelu danych współbieżności

W programie Visual Studio Otwórz aplikację sieci web firmy Contoso University pracowano w poprzednich instrukcji w tej serii.

Otwórz *SchoolModel.edmx*i w Projektant modelu danych, kliknij prawym przyciskiem myszy `Name` właściwości w `Department` jednostki, a następnie kliknij przycisk **właściwości**. W **właściwości** Zmień `ConcurrencyMode` właściwości `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Wykonaj te same dla innych właściwości skalarnej z systemem innym niż klucza podstawowego (`Budget`, `StartDate`, i `Administrator`.) (Nie można w tym przypadku właściwości nawigacji.) Określa, że po każdej zmianie programu Entity Framework generuje `Update` lub `Delete` polecenie SQL aktualizujące `Department` jednostkę w bazie danych, te kolumny (z oryginalnych wartości) muszą być zawarte w `Where` klauzuli. Jeśli wiersz nie zostanie znaleziony podczas `Update` lub `Delete` polecenie zostanie wykonane, Entity Framework spowoduje zgłoszenie wyjątku optymistycznej współbieżności.

Zapisz i zamknij modelu danych.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Obsługa wyjątków współbieżności w warstwy DAL

Otwórz *SchoolRepository.cs* i dodaj następującą `using` instrukcji dla `System.Data` przestrzeni nazw:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Dodaj następujące nowe `SaveChanges` metodę, która obsługuje optymistycznych wyjątków współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Jeśli wystąpi błąd współbieżności, gdy ta metoda jest wywoływana, wartości właściwości jednostki w pamięci są zastępowane wartości obecnie w bazie danych. Wyjątku współbieżności jest zgłoszony, dzięki czemu może je obsłużyć strony sieci web.

W `DeleteDepartment` i `UpdateDepartment` metod, Zastąp wywołanie istniejących `context.SaveChanges()` wywołaniem `SaveChanges()` w celu wywołania metody nowe.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Obsługa wyjątków współbieżności warstwy prezentacji

Otwórz *Departments.aspx* i Dodaj `OnDeleted="DepartmentsObjectDataSource_Deleted"` atrybutu `DepartmentsObjectDataSource` formantu. Otwierający tag formantu zostanie teraz podobne do następującego przykładu.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

W `DepartmentsGridView` kontrolować, określ wszystkie kolumny tabeli w `DataKeyNames` atrybutu, jak pokazano w poniższym przykładzie. Należy pamiętać, że spowoduje to utworzenie widoku bardzo dużych pól stanu, która jest jedną z przyczyn Dlaczego za pomocą pola śledzenia jest zwykle preferowany sposób śledzenia konfliktom współbieżności.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Otwórz *Departments.aspx.cs* i dodaj następującą `using` instrukcji dla `System.Data` przestrzeni nazw:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Dodaj następującą metodę nowe będzie wywoływać z kontroli źródła danych `Updated` i `Deleted` programy obsługi zdarzeń dla obsługi wyjątków współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Ten kod sprawdza typ wyjątku, a jeśli wyjątku współbieżności, kod tworzy dynamicznie `CustomValidator` formant, który z kolei wyświetla komunikat w `ValidationSummary` formantu.

Wywołaj metodę nowego z `Updated` obsługi zdarzeń, który wcześniej został dodany. Ponadto, Utwórz nową `Deleted` obsługi zdarzeń, który wywołuje tej samej metody (ale nie wykonuje żadnych innych czynności):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Testowanie optymistycznej współbieżności na stronie działów

Uruchom *Departments.aspx* strony.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Kliknij przycisk **Edytuj** w wierszu i zmień wartość w **budżetu** kolumny. (Należy pamiętać, że można edytować tylko rekordy, które zostały utworzone w tym samouczku, ponieważ istniejący `School` rekordów bazy danych zawiera niektóre nieprawidłowe dane. Rekord dla działu ekonomii jest bezpieczne do wykonywania eksperymentów.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Otwórz nowe okno przeglądarki i uruchom ponownie strony (skopiuj adres URL z pola adresu w pierwszym oknie przeglądarki do drugiego okna przeglądarki).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Kliknij przycisk **Edytuj** w tym samym wierszu można edytowane wcześniej i zmień **budżetu** inną wartość.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Kliknij w oknie przeglądarki drugi **aktualizacji**. **Budżetu** kwota pomyślnie zostanie zmieniona do tej nowej wartości.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

W pierwszym oknie przeglądarki, kliknij przycisk **aktualizacji**. Aktualizacja nie powiedzie się. **Budżetu** kwota zostanie wyświetlony ponownie przy użyciu wartość ustawiona w drugie okno przeglądarki i zostanie wyświetlony komunikat o błędzie.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Obsługa przy użyciu właściwości śledzenia optymistycznej współbieżności.

Aby obsługiwać optymistycznej współbieżności dla obiektu, który ma właściwość śledzenia, będzie wykonać następujące zadania:

- Dodaj procedur składowanych do modelu danych, aby zarządzać `OfficeAssignment` jednostek. (Właściwości śledzenia i procedur składowanych nie mają być używane razem; są one tylko grupowane razem w tym miejscu ilustracyjną.)
- Dodaj metody CRUD warstwy DAL i logiki warstwy Biznesowej dla `OfficeAssignment` encje, w tym kod obsługujący optymistycznych wyjątków współbieżności w warstwy DAL.
- Utwórz stronę sieci web przypisania pakietu office.
- Test optymistycznej współbieżności nowej strony sieci web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Dodawanie OfficeAssignment przechowywane procedury do modelu danych

Otwórz *SchoolModel.edmx* w Projektancie modelu, kliknij prawym przyciskiem myszy powierzchnię projektu, a następnie kliknij przycisk **modelu aktualizacji z bazy danych**. W **Dodaj** karcie **wybierz obiekty bazy danych użytkownika** okna dialogowego rozwiń **procedur składowanych** i wybierz trzech `OfficeAssignment` procedur składowanych (zobacz Po zrzut ekranu), a następnie kliknij przycisk **Zakończ**. (Tych procedur przechowywanych już w bazie danych podczas zostały pobrane lub utworzone za pomocą skryptu.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Kliknij prawym przyciskiem myszy `OfficeAssignment` jednostki i wybierz **mapowania procedury przechowywane**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ustaw **Wstaw**, **aktualizacji**, i **usunąć** funkcje do użycia odpowiednie procedury składowane. Dla `OrigTimestamp` parametr `Update` funkcji, należy ustawić **właściwości** do `Timestamp` i wybierz **Użyj oryginalnej wartości** opcji.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Gdy programu Entity Framework wymaga `UpdateOfficeAssignment` procedury składowanej go przekazywać oryginalnej wartości elementu `Timestamp` kolumny w `OrigTimestamp` parametru. Procedura składowana używa parametru w jego `Where` klauzuli:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Procedura składowana wybiera również nową wartość `Timestamp` kolumny po aktualizacji, aby zachować programu Entity Framework `OfficeAssignment` jednostki, który znajduje się w pamięci w odpowiedni wiersz bazy danych.

(Należy pamiętać, że nie ma procedury składowanej usuwania przypisania office `OrigTimestamp` parametru. W związku z tym Entity Framework nie może sprawdzić czy obiekt jest taki sam jak w przed jego usunięciem.)

Zapisz i zamknij modelu danych.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Dodawanie metody OfficeAssignment do warstwy DAL

Otwórz *ISchoolRepository.cs* i dodaj następujące metody CRUD `OfficeAssignment` zestaw jednostek:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Dodaj następujące metody nowe *SchoolRepository.cs*. W `UpdateOfficeAssignment` metody należy wywołać lokalnej `SaveChanges` zamiast metody `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Otwórz projekt testowy *MockSchoolRepository.cs* i dodaj następującą `OfficeAssignment` kolekcji i metody CRUD do niego. (Zasymulować repozytorium musi implementować interfejs repozytorium, lub nie Kompiluj rozwiązanie).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Dodawanie metody OfficeAssignment logiki warstwy Biznesowej

W projekcie głównym, należy otworzyć *SchoolBL.cs* i dodaj następujące metody CRUD `OfficeAssignment` zestawu jednostek do niej:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Tworzenie stron sieci Web OfficeAssignments

Tworzenie nowej strony sieci web, która używa *Site.Master* strony wzorcowej i nadaj mu nazwę *OfficeAssignments.aspx*. Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Zwróć uwagę, że w `DataKeyNames` atrybutu kod znaczników Określa `Timestamp` właściwości, a także klucz rekordu (`InstructorID`). Określanie właściwości w `DataKeyNames` atrybutu powoduje, że formant, aby zapisać je w stanie kontroli, (która jest podobna do stanu widoku) dzięki czemu oryginalne wartości są dostępne podczas przetwarzania odświeżania strony.

Jeśli nie zapiszesz `Timestamp` wartość programu Entity Framework nie miałoby dla `Where` klauzuli SQL `Update` polecenia. W związku z tym nie będzie można znaleźć do aktualizacji. W związku z tym Entity Framework spowoduje zgłoszenie wyjątku optymistycznej współbieżności zawsze `OfficeAssignment` aktualizacji jednostki.

Otwórz *OfficeAssignments.aspx.cs* i dodaj następującą `using` instrukcji dla warstwy dostępu do danych:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Dodaj następujące `Page_Init` metodę, która umożliwia korzystanie z funkcji danych dynamicznych. Dodaj również następujące obsługę `ObjectDataSource` formantu `Updated` zdarzeń, aby sprawdzić błędy współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Testowanie optymistycznej współbieżności na stronie OfficeAssignments

Uruchom *OfficeAssignments.aspx* strony.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Kliknij przycisk **Edytuj** w wierszu i zmień wartość w **lokalizacji** kolumny.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Otwórz nowe okno przeglądarki i uruchom ponownie strony (skopiuj adres URL z pierwszego okna przeglądarki do drugiego okna przeglądarki).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Kliknij przycisk **Edytuj** w tym samym wierszu można edytowane wcześniej i zmień **lokalizacji** inną wartość.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Kliknij w oknie przeglądarki drugi **aktualizacji**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Przejście do pierwszego okna przeglądarki, a następnie kliknij przycisk **aktualizacji**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Zostanie wyświetlony komunikat o błędzie i **lokalizacji** wartość została zaktualizowana do wyświetlenia wartość można zmienić w oknie przeglądarki drugiego.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Obsługa współbieżności za pomocą formantu obiektu EntityDataSource

`EntityDataSource` Formant zawiera wbudowaną logikę, która rozpoznaje ustawień współbieżności w modelu danych i dojścia aktualizacja i usuwanie operacji odpowiednio. Jednakże, podobnie jak w przypadku wszystkich wyjątków musi obsługiwać `OptimisticConcurrencyException` wyjątki samodzielnie zapewnić przyjazny dla użytkownika komunikat.

Następnie należy skonfigurować *Courses.aspx* strony (który korzysta z `EntityDataSource` kontroli) umożliwia aktualizację i operacji usunięcia i wyświetlić komunikat o błędzie, jeśli występuje konflikt współbieżności. `Course` Jednostka nie ma wartość współbieżności to śledzenie kolumny, więc będzie używać tej samej metody, która jak w przypadku `Department` jednostki: śledzenie wartości wszystkich właściwości klucza.

Otwórz *SchoolModel.edmx* pliku. Dla właściwości niekluczowe `Course` jednostki (`Title`, `Credits`, i `DepartmentID`) ustaw **tryb współbieżności** właściwości `Fixed`. Następnie zapisz i zamknij modelu danych.

Otwórz *Courses.aspx* stronie, a następnie dokonaj następujących zmian:

- W `CoursesEntityDataSource` kontrolować, Dodaj `EnableUpdate="true"` i `EnableDelete="true"` atrybutów. Otwierający tag dla tego formantu teraz podobnego do następującego:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- W `CoursesGridView` kontrolować, zmień `DataKeyNames` wartość do atrybutu `"CourseID,Title,Credits,DepartmentID"`. Następnie dodaj `CommandField` elementu `Columns` element, który zawiera **Edytuj** i **usunąć** przycisków (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Kontroli teraz podobnego do następującego:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Uruchom strony i Utwórz sytuacji konflikt tak samo jak przed na stronie działów. Uruchomić strony w dwa okna przeglądarki, kliknij przycisk **Edytuj** w tym samym wiersz każdego okna i różne zmiany w każdej z nich. Kliknij przycisk **aktualizacji** w jednym oknie, a następnie kliknij przycisk **aktualizacji** w innym oknie. Po kliknięciu **aktualizacji** za drugim razem, zobacz stronę błędu, będącą wynikiem współbieżności nieobsługiwany wyjątek.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Obsługa ten błąd wystąpił w sposób bardzo podobny do sposobu obsługi dla `ObjectDataSource` formantu. Otwórz *Courses.aspx* stronę i w `CoursesEntityDataSource` kontroli, określ obsługę `Deleted` i `Updated` zdarzenia. Otwierający tag formantu teraz podobnego do następującego:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Przed `CoursesGridView` kontrolować, należy dodać następujące `ValidationSummary` sterowania:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

W *Courses.aspx.cs*, Dodaj `using` instrukcji dla `System.Data` przestrzeni nazw, Dodaj metodę, która sprawdza dla wyjątków współbieżności i Dodaj obsługę `EntityDataSource` formantu `Updated` i `Deleted`programów obsługi. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Jedyną różnicą między ten kod i jak w `ObjectDataSource` formant jest w tym przypadku wyjątku współbieżności w `Exception` właściwości obiektu argumentów zdarzenia, a nie w tym wyjątku `InnerException` właściwości.

Uruchom stronie i ponownie utwórz konflikt współbieżności. Teraz zostanie wyświetlony komunikat o błędzie:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Na tym kończy się wprowadzenie do obsługi konfliktom współbieżności. Następny samouczek zawierają wskazówki dotyczące poprawy wydajności w aplikacji sieci web, która używa programu Entity Framework.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [dalej](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
