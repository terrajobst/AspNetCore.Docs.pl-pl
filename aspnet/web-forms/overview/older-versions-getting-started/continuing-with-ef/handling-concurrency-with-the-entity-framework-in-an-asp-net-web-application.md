---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Obsługa współbieżności przy użyciu programu Entity Framework 4.0 w aplikacji ASP.NET 4 Web | Dokumentacja firmy Microsoft
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web firmy Contoso University, utworzony przez wprowadzenie do serii samouczków Entity Framework 4.0. CZY MOGĘ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6b10aca944a322cae252666218aee4d5a2d6ed35
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754978"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Obsługa współbieżności przy użyciu programu Entity Framework 4.0 w aplikacji ASP.NET sieci Web 4
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serii samouczków. Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednim samouczku przedstawiono sposób sortowania i filtrowania danych przy użyciu `ObjectDataSource` kontroli i Entity Framework. Ten samouczek przedstawia opcje obsługi współbieżności w aplikacji sieci web ASP.NET, który używa programu Entity Framework. Utworzysz nową stronę sieci web, przeznaczoną do aktualizowania przypisań office instruktora. Zajmiemy się problemy z współbieżności na tej stronie i na stronie działów, który został utworzony wcześniej.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Występuje konflikt współbieżności, gdy jeden użytkownik edytuje rekord i inny użytkownik edytuje ten sam rekord, przed zapisaniem zmian pierwszego użytkownika do bazy danych. Jeśli nie skonfigurowano programu Entity Framework, aby wykryć takie konflikty, spojrzenia aktualizuje bazę danych ostatniego zastępuje zmiany wprowadzone przez użytkownika. W wielu aplikacjach to zagrożenie jest dopuszczalne, a nie trzeba skonfigurować aplikację do obsługi konfliktów współbieżności możliwe. (Jeśli istnieją kilku użytkowników lub kilka aktualizacji lub nie jest tak naprawdę krytycznego, jeśli niektóre zmiany są zastępowane, koszt programowania współbieżność może przeważają korzyści.) Jeśli nie potrzebujesz już martwić się o konfliktów współbieżności, można pominąć ten samouczek; pozostałe dwie samouczków w serii nie należy polegać na coś, co tworzysz w tym obiekcie.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli Twoja aplikacja wymaga uniknąć przypadkowej utraty danych w scenariuszach współbieżności, używają blokady bazy danych jest jednym ze sposobów, aby to zrobić. Jest to nazywane *Współbieżność pesymistyczna*. Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub uzyskać dostęp do aktualizacji. Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub zaktualizować dostępu, ponieważ jaką uzyskują kopii danych, która jest w trakcie zmieniany. Jeśli zablokujesz wiersz, aby uzyskać dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokady ma pewne wady. Może być skomplikowane, aby program. Wymaga znaczących bazy danych zarządzania zasobami, a jako liczbę użytkowników aplikacji może spowodować problemy z wydajnością zwiększa (czyli go nie jest dobrze skalowalna). Z tego względu nie wszystkie systemy zarządzania bazami danych obsługuje pesymistycznej współbieżności. Entity Framework nie obsługuje wbudowanej go, a w tym samouczku nie pokazano, jak ją wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Jest alternatywą do Współbieżność pesymistyczna *optymistycznej współbieżności*. Optymistyczna współbieżność oznacza, umożliwiając konfliktów współbieżności do wykonania, a następnie reagowaniu odpowiednio Jeśli tak jest. Na przykład Jan uruchamia *Department.aspx* strony, kliknięć **Edytuj** łączy dla działu historii, a także zmniejsza **budżetu** kwotę od 1,000,000.00 $ $ 125,000.00. (Jan zarządza dział konkurencyjnych i chce, aby zwolnić pieniądze własnej dział).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Zanim Jan kliknie **aktualizacji**, Magdalena uruchamia tę samą stronę, kliknie **Edytuj** link dla działu historii, a następnie zmiany **Data rozpoczęcia** pola do 1/1/1/10/2011 1999. (Magdalena zarządza dział historii i chce dać jej więcej związanych ze stażem pracy).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John kliknie **aktualizacji** najpierw Magdalena klika **aktualizacji**. Wyświetla teraz przeglądarki Joanny **budżetu** jak $1,000,000.00, ale jest niepoprawna, ponieważ ilość zostało zmienione, Jan do 125,000.00 $.

Akcje, które można wykonać w tym scenariuszu między innymi następujące:

- Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie zostałyby utracone, ponieważ inne właściwości zostały zaktualizowane przez dwóch użytkowników. Przy następnym przegląda ktoś z działu historii, zobaczy 1/1/1999 i 125,000.00 $. 

    Jest to domyślne zachowanie platformy Entity Framework, a jej znacznie zmniejszyć liczbę konfliktów, które może spowodować utratę danych. Jednak to zachowanie nie uniknąć utraty danych, jeśli konkurencyjnych zmiany zostały wprowadzone w tej samej właściwości jednostki. Ponadto to zachowanie nie zawsze jest możliwe; Kiedy mapujesz procedur składowanych dla typu jednostki wszystkich właściwości obiektu są aktualizowane, po dokonaniu zmiany do jednostki w bazie danych.
- Można pozwolić, aby zmiana Joanny zastąpienie John's zmian. Po kliknięciu Magdalena **aktualizacji**, **budżetu** kwota powraca do 1,000,000.00 $. Jest to nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza. (Wartości klienta pierwszeństwo co znajduje się w magazynie danych.)
- Aby uniemożliwić zmiany Joanny aktualizowane w bazie danych. Zazwyczaj będzie wyświetlony komunikat o błędzie, pokazywać bieżący stan danych i umożliwienia jej uzyskania ponownie wprowadzić swoje zmiany, jeśli chce nadal były. Można dodatkowo zautomatyzować ten proces, zapisując jej danych wejściowych i zapewniając jej możliwość Zastosuj je ponownie bez konieczności ponownego wprowadzania go. Jest to nazywane *Store Wins* scenariusza. (Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.)

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

W ramach jednostki może rozwiązać konflikty, obsługując `OptimisticConcurrencyException` wyjątki, które zgłasza Entity Framework. Aby wiedzieć, kiedy trzeba generować te wyjątki, platformy Entity Framework musi umożliwiać wykrywanie konfliktów. W związku z tym należy skonfigurować bazy danych oraz model danych odpowiednio. Niektóre opcje umożliwiające wykrywanie konfliktów są następujące:

- W bazie danych zawierają kolumny tabeli, który może służyć do określania, kiedy wiersz został zmieniony. Następnie można skonfigurować programu Entity Framework, aby uwzględnić tej kolumny w `Where` klauzuli SQL `Update` lub `Delete` poleceń.

    Jest celem `Timestamp` kolumny w `OfficeAssignment` tabeli.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Typ danych `Timestamp` kolumny jest również nazywany `Timestamp`. Jednak kolumna faktycznie nie zawiera wartości daty i godziny. Zamiast tego wartość numeru sekwencyjnego, który jest zwiększana za każdym razem, wiersz jest aktualizowany. W `Update` lub `Delete` polecenia `Where` klauzula zawiera oryginalny `Timestamp` wartość. Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `Timestamp` różni się od oryginalnej wartości, więc `Where` klauzula zwraca nie wiersza do zaktualizowania. Gdy Entity Framework wykryje, czy żadne wiersze nie zostały zaktualizowane przez bieżącą `Update` lub `Delete` polecenie (to znaczy, gdy liczba wierszy, których to dotyczy, jest równy zero), który interpretuje jako konflikt współbieżności.
- Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości każdej kolumny w tabeli w `Where` klauzuli `Update` i `Delete` poleceń.

    Tak jak w pierwszej opcji, jeśli nic w wierszu zmieniła się od wiersza została najpierw przeczytać artykuł `Where` klauzuli nie zwraca wiersz, aby zaktualizować, której platformy Entity Framework interpretuje jako konflikt współbieżności. Ta metoda jest tak skuteczne, jak za pomocą `Timestamp` pola, ale może być mało wydajne. W przypadku tabel bazy danych, które mają wiele kolumn, może to skutkować bardzo dużych `Where` zdań, i w aplikacji sieci web go wymagać obsługi dużych ilości stanu. Obsługa dużych ilości stanu może wpłynąć na wydajność aplikacji ponieważ go wymaga zasobów serwera (na przykład stan sesji) albo muszą być zawarte w tej strony (na przykład stan widoku).

W tym samouczku dodasz obsługę błędów dla konfliktów optymistycznej współbieżności dla jednostki, która nie ma właściwości śledzenia ( `Department` jednostek) i jednostki, która zawiera właściwości śledzenia ( `OfficeAssignment` jednostek).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Obsługa optymistycznej współbieżności bez właściwości śledzenia

Na potrzeby optymistycznej współbieżności dla `Department` jednostki, która nie ma śledzenia (`Timestamp`) właściwość, zostaną wykonane następujące zadania:

- Zmień model danych, aby włączyć śledzenie współbieżności `Department` jednostek.
- W `SchoolRepository` klasy uchwyt wyjątki współbieżności w `SaveChanges` metody.
- W *Departments.aspx* strony uchwytu wyjątki współbieżności, poprzez wyświetlenie komunikatu do użytkownika, ostrzeżenie, że próba zmiany zakończyły się niepowodzeniem. Użytkownik może następnie wyświetlić bieżące wartości i ponów próbę zmiany, jeśli są one nadal potrzebne.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Włączanie śledzenia w modelu danych współbieżności

W programie Visual Studio Otwórz aplikację sieci web firmy Contoso University, która pracowano w poprzednim samouczku z tej serii.

Otwórz *SchoolModel.edmx*i w Projektancie modeli danych, kliknij prawym przyciskiem myszy `Name` właściwość `Department` jednostki, a następnie kliknij przycisk **właściwości**. W **właściwości** oknie zmiany `ConcurrencyMode` właściwość `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Zrób to samo dla innych właściwości skalarne bez klucza podstawowego (`Budget`, `StartDate`, i `Administrator`.) (Nie możesz tego zrobić dla właściwości nawigacji.) Określa, że zawsze, gdy platforma Entity Framework generuje `Update` lub `Delete` polecenie SQL aktualizujące `Department` jednostek w bazie danych, te kolumny (z oryginalnych wartości) musi być uwzględniona w `Where` klauzuli. Jeśli żaden wiersz nie zostanie znaleziony, kiedy `Update` lub `Delete` polecenie wykonuje, platformy Entity Framework spowoduje zgłoszenie wyjątku optymistycznej współbieżności.

Zapisz i zamknij modelu danych.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Obsługa wyjątków współbieżności na warstwy DAL

Otwórz *SchoolRepository.cs* i dodaj następującą `using` poufności informacji dotyczące `System.Data` przestrzeni nazw:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Dodaj następujące nowe `SaveChanges` metody, która obsługuje wyjątki optymistycznej współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Jeśli wystąpi błąd współbieżności, gdy ta metoda jest wywoływana, wartości właściwości obiektu w pamięci są zastępowane wartościami znajdujących się obecnie w bazie danych. Wyjątku współbieżności jest zgłaszany ponownie, tak aby można go obsłużyć, strony sieci web.

W `DeleteDepartment` i `UpdateDepartment` metod, Zastąp wywołanie istniejących `context.SaveChanges()` wywołaniem `SaveChanges()` aby wywołać nową metodę.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Obsługa wyjątków współbieżności w warstwie prezentacji

Otwórz *Departments.aspx* i Dodaj `OnDeleted="DepartmentsObjectDataSource_Deleted"` atrybutu `DepartmentsObjectDataSource` kontroli. Otwierający tag dla formantu będzie teraz wyglądać następująco.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

W `DepartmentsGridView` sterowania, wybierz opcję Wszystkie kolumny tabeli w `DataKeyNames` atrybutu, jak pokazano w poniższym przykładzie. Należy pamiętać, że spowoduje to utworzenie widoku bardzo dużych pól stanu który jest jednym z powodów dlaczego za pomocą pola śledzenia ogólnie jest preferowanym sposobem śledzenia konfliktów współbieżności.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Otwórz *Departments.aspx.cs* i dodaj następującą `using` poufności informacji dotyczące `System.Data` przestrzeni nazw:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Dodaj następującą nową metodę, która będzie wywoływać z poziomu kontroli źródła danych `Updated` i `Deleted` programy obsługi zdarzeń dla obsługi wyjątków współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Ten kod sprawdza, czy typ wyjątku, a jeśli wyjątku współbieżności, ten kod tworzy dynamicznie `CustomValidator` formant, który z kolei wyświetla komunikat w `ValidationSummary` kontroli.

Wywołaj metodę nowe z `Updated` program obsługi zdarzeń, który dodano wcześniej. Ponadto, należy utworzyć nową `Deleted` obsługi zdarzeń, który wywołuje tej samej metody (ale nie robi nic innego):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Testowanie optymistycznej współbieżności na stronie działów

Uruchom *Departments.aspx* strony.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Kliknij przycisk **Edytuj** w wierszu i zmień wartość w **budżetu** kolumny. (Należy pamiętać, że możesz edytować tylko rekordy utworzone na potrzeby tego samouczka, ponieważ istniejące `School` rekordów bazy danych zawiera niektóre nieprawidłowe dane. Rekord dla działu ekonomii jest bezpieczne, aby eksperymentować z).

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Otwórz nowe okno przeglądarki i uruchom ponownie strony (Kopiuj adres URL z pola adresu pierwsze okno przeglądarki, aby drugie okno przeglądarki).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Kliknij przycisk **Edytuj** w tym samym wierszu, możesz edytować wcześniej i zmień **budżetu** wartości na coś innego.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

W drugim oknie przeglądarki, kliknij przycisk **aktualizacji**. **Budżetu** kwota jest pomyślnie zmieniono na tę nową wartość.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

W pierwszym oknie przeglądarki, kliknij przycisk **aktualizacji**. Aktualizacja nie powiodła się. **Budżetu** kwota zostanie wyświetlony ponownie przy użyciu wartość ustawiona w drugim oknie przeglądarki, a następnie zostanie wyświetlony komunikat o błędzie.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Obsługa optymistycznej współbieżności przy użyciu właściwości śledzenia

Aby obsłużyć optymistycznej współbieżności dla jednostki, która ma właściwości śledzenia, wykonasz następujące zadania:

- Dodawaj procedury składowane do modelu danych, aby zarządzać `OfficeAssignment` jednostek. (Właściwości śledzenia i procedur składowanych, które nie mają być używane razem; są one po prostu grupowane razem w tym miejscu do celów ilustracyjnych).
- Dodaj metody CRUD warstwy DAL i LOGIKI dla `OfficeAssignment` jednostkami, w tym kod służący do obsługi wyjątków optymistycznej współbieżności w warstwy DAL.
- Utwórz stronę sieci web pakietu office przypisania.
- Przetestuj optymistycznej współbieżności w nową stronę sieci web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Dodawanie OfficeAssignment procedury składowane do modelu danych

Otwórz *SchoolModel.edmx* pliku w Projektancie modeli, kliknij prawym przyciskiem myszy powierzchnię projektu, a następnie kliknij przycisk **Model aktualizacji z bazy danych**. W **Dodaj** karcie **wybierz obiekty bazy danych** okna dialogowego rozwiń **procedur składowanych** i wybrać trzy `OfficeAssignment` procedur składowanych (zobacz Po zrzut ekranu), a następnie kliknij przycisk **Zakończ**. (Tych procedur przechowywanych już w bazie danych podczas zostały pobrane lub utworzone za pomocą skryptu.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Kliknij prawym przyciskiem myszy `OfficeAssignment` jednostki, a następnie wybierz pozycję **mapowanie procedur przechowywanych**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Ustaw **Wstaw**, **aktualizacji**, i **Usuń** funkcje do użycia, odpowiadające im procedur składowanych. Dla `OrigTimestamp` parametru `Update` funkcji, należy ustawić **właściwość** do `Timestamp` i wybierz **Użyj oryginalnej wartości** opcji.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Kiedy wywołuje Entity Framework `UpdateOfficeAssignment` procedury składowanej, zostaną przetworzone oryginalnej wartości elementu `Timestamp` kolumny w `OrigTimestamp` parametru. Procedura składowana używa tego parametru w jego `Where` klauzuli:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Procedura składowana zaznacza nowa wartość `Timestamp` kolumny po aktualizacji tak, aby zachować Entity Framework `OfficeAssignment` jednostki, który znajduje się w pamięci w synchronizacji z odpowiednim bazy danych.

(Należy pamiętać, że procedura składowana usuwania przypisania pakietu office nie ma `OrigTimestamp` parametru. W związku z tym Entity Framework nie może sprawdzić czy jednostki są takie same jak w przed jego usunięciem.)

Zapisz i zamknij modelu danych.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Dodawanie metod OfficeAssignment z warstwą dal

Otwórz *ISchoolRepository.cs* i dodaj następujące metody CRUD `OfficeAssignment` zestaw jednostek:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Dodaj następujące metody nowe *SchoolRepository.cs*. W `UpdateOfficeAssignment` metody, należy wywołać lokalnej `SaveChanges` zamiast metody `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

W projekcie test Otwórz *MockSchoolRepository.cs* i dodaj następującą `OfficeAssignment` zbierania i metody CRUD. (Makiety repozytorium musi implementować interfejs repozytorium lub nie będzie skompilować rozwiązanie.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Dodawanie metody OfficeAssignment LOGIKI

W głównym projektu, otwórz *SchoolBL.cs* i dodaj następujące metody CRUD `OfficeAssignment` zestawu jednostek do niego:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Tworzenie strony sieci Web OfficeAssignments

Tworzenie nowej strony internetowej, która używa *Site.Master* strony wzorcowej i nadaj mu nazwę *OfficeAssignments.aspx*. Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Należy zauważyć, że w `DataKeyNames` atrybutu znaczników Określa `Timestamp` właściwości, a także klucz rekordu (`InstructorID`). Określanie właściwości w `DataKeyNames` atrybutu powoduje, że formant Aby zapisać je w stan formantu, (co przypomina, aby wyświetlić stan), aby oryginalne wartości są dostępne podczas przetwarzania zwrotu.

Jeśli nie został zapisany `Timestamp` wartości, platformy Entity Framework nie musi je `Where` klauzuli SQL `Update` polecenia. W związku z tym nic nie będzie można znaleźć do aktualizacji. W rezultacie Entity Framework spowoduje zgłoszenie wyjątku optymistycznej współbieżności każdym razem, gdy `OfficeAssignment` jednostki są aktualizowane.

Otwórz *OfficeAssignments.aspx.cs* i dodaj następującą `using` poufności informacji dotyczące warstwy dostępu do danych:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Dodaj następujący kod `Page_Init` metody, która włącza funkcję danych dynamicznych. Również dodać następujące Obsługa `ObjectDataSource` kontrolki `Updated` zdarzeń, aby sprawdzić błędy współbieżności:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Testowanie optymistycznej współbieżności na stronie OfficeAssignments

Uruchom *OfficeAssignments.aspx* strony.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Kliknij przycisk **Edytuj** w wierszu i zmień wartość w **lokalizacji** kolumny.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Otwórz nowe okno przeglądarki i uruchom ponownie strony (Kopiuj adres URL w pierwszym oknie przeglądarki, aby drugie okno przeglądarki).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Kliknij przycisk **Edytuj** w tym samym wierszu, możesz edytować wcześniej i zmień **lokalizacji** wartości na coś innego.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

W drugim oknie przeglądarki, kliknij przycisk **aktualizacji**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Przejdź do pierwszego okna przeglądarki, a następnie kliknij przycisk **aktualizacji**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Zostanie wyświetlony komunikat o błędzie i **lokalizacji** wartość został zaktualizowany do wyświetlenia wartości został zmieniony w drugim oknie przeglądarki.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Obsługa współbieżności przy użyciu kontrolki EntityDataSource

`EntityDataSource` Kontroli zawiera wbudowaną logikę, która rozpoznaje ustawieniami współbieżności w modelu danych i obsługuje operacje aktualizowania i usuwania odpowiednio. Jednakże, podobnie jak w przypadku wszystkich wyjątków, musi obsługiwać `OptimisticConcurrencyException` wyjątki samodzielnie, aby zapewnić przyjazny dla użytkownika komunikat.

Następnie należy skonfigurować *Courses.aspx* strony (wykonującemu `EntityDataSource` kontroli) zezwala na aktualizowanie i usuwanie operacji i wyświetlić komunikat o błędzie, jeśli wystąpi konflikt współbieżności. `Course` Jednostki nie istnieje współbieżności, śledzenia kolumn, dzięki czemu będą używać tej samej metody, która została zastosowana za pomocą `Department` jednostki: śledzenie wartości wszystkich właściwości klucza.

Otwórz *SchoolModel.edmx* pliku. Dla właściwości-key `Course` jednostki (`Title`, `Credits`, i `DepartmentID`) ustaw **tryb współbieżności** właściwość `Fixed`. Następnie zapisz i zamknij modelu danych.

Otwórz *Courses.aspx* strony, a następnie dokonaj następujących zmian:

- W `CoursesEntityDataSource` Dodaj `EnableUpdate="true"` i `EnableDelete="true"` atrybutów. Otwierający tag dla tej kontrolki teraz podobnego do następującego:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- W `CoursesGridView` kontrolować, zmień `DataKeyNames` wartość do atrybutu `"CourseID,Title,Credits,DepartmentID"`. Następnie dodaj `CommandField` elementu `Columns` element, który pokazuje **Edytuj** i **Usuń** przycisków (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Kontroli teraz podobnego do następującego:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Uruchom stronę i Utwórz sytuacji konflikt, jak poprzednio na stronie działów. Uruchom stronę w dwa okna przeglądarki, kliknij przycisk **Edytuj** w tej samej linii każdego okna i wprowadzić różne zmiany w każdej z nich. Kliknij przycisk **aktualizacji** w jednym oknie, a następnie kliknij przycisk **aktualizacji** w innym oknie. Po kliknięciu **aktualizacji** po raz drugi, zostanie wyświetlona strona błędu, będącą wynikiem współbieżności nieobsługiwany wyjątek.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Obsługa ten błąd wystąpił w sposób bardzo podobny do sposobu obsługi dla `ObjectDataSource` kontroli. Otwórz *Courses.aspx* strony, a następnie w `CoursesEntityDataSource` kontrolki, określa programy obsługi dla `Deleted` i `Updated` zdarzenia. Otwierający tag formantu teraz podobnego do następującego:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Przed `CoursesGridView` sterowania, Dodaj następujący kod `ValidationSummary` sterowania:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

W *Courses.aspx.cs*, Dodaj `using` poufności informacji dotyczące `System.Data` przestrzeni nazw, Dodaj metodę, która sprawdza współbieżności wyjątków i programy obsługi dla `EntityDataSource` kontrolki `Updated` i `Deleted`programów obsługi. Ten kod będzie wyglądać następująco:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Jedyną różnicą między tym kodem i co zostało zrobione `ObjectDataSource` kontrolka jest w tym przypadku wyjątku współbieżności w `Exception` właściwości obiektu w argumenty zdarzenia, a nie w ten wyjątek `InnerException` właściwości.

Uruchom stronę i ponownie utwórz konfliktów współbieżności. Teraz, zobaczysz komunikat o błędzie:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Na tym kończy się wprowadzenie do obsługi konfliktów współbieżności. Następnym samouczku przedstawiono wskazówki dotyczące zwiększania wydajności w aplikacji sieci web, która używa programu Entity Framework.

> [!div class="step-by-step"]
> [Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [dalej](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
