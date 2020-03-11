---
title: 'Samouczek: obsługa współbieżności ASP.NET MVC z EF Core'
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 6839e383093b993ff55095f26cf88cd68708f001
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657396"
---
# <a name="tutorial-handle-concurrency---aspnet-mvc-with-ef-core"></a>Samouczek: obsługa współbieżności ASP.NET MVC z EF Core

W poprzednich samouczkach przedstawiono sposób aktualizowania danych. W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.

Utworzysz strony sieci Web, które współpracują z jednostką działu i obsługują błędy współbieżności. Na poniższych ilustracjach przedstawiono strony edycji i usuwania, w tym niektóre komunikaty, które są wyświetlane w przypadku wystąpienia konfliktu współbieżności.

![Strona edycji działu](concurrency/_static/edit-error.png)

![Strona usuwania działu](concurrency/_static/delete-error.png)

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje o konfliktach współbieżności
> * Dodaj właściwość śledzenia
> * Tworzenie kontrolera i widoków działów
> * Aktualizuj widok indeksu
> * Aktualizowanie metod edycji
> * Aktualizuj widok edycji
> * Testuj konflikty współbieżności
> * Aktualizuj stronę Delete
> * Aktualizowanie szczegółów i Tworzenie widoków

## <a name="prerequisites"></a>Wymagania wstępne

* [Aktualizowanie powiązanych danych](update-related-data.md)

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Konflikt współbieżności występuje, gdy jeden użytkownik wyświetla dane jednostki w celu ich edycji, a następnie inny użytkownik aktualizuje dane tej samej jednostki przed zapisaniem w bazie danych pierwszego użytkownika. Jeśli nie włączysz wykrywania takich konfliktów, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników. W wielu aplikacjach to ryzyko jest akceptowalne: w przypadku kilku użytkowników lub kilku aktualizacji lub jeśli nie jest to naprawdę krytyczne, jeśli niektóre zmiany zostaną nadpisywane, koszt programowania współbieżności może wznieść korzyści. W takim przypadku nie trzeba konfigurować aplikacji do obsługi konfliktów współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli aplikacja musi zapobiegać przypadkowej utracie danych w scenariuszach współbieżności, jeden ze sposobów jest używany do blokowania baz danych. Jest to nazywane pesymistyczną współbieżnością. Na przykład przed odczytaniem wiersza z bazy danych należy zażądać blokady dla dostępu tylko do odczytu lub do aktualizacji. Jeśli zablokujesz wiersz na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie będzie mógł zablokować wiersza dla dostępu tylko do odczytu lub aktualizacji, ponieważ spowodowałoby to skopiowanie danych w procesie. Jeśli zablokujesz wiersz dla dostępu tylko do odczytu, inne osoby mogą także zablokować dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokadami ma wady. Może być skomplikowany dla programu. Wymaga to znaczących zasobów zarządzania bazami danych. może to spowodować problemy z wydajnością w miarę zwiększania się liczby użytkowników aplikacji. Z tego względu nie wszystkie systemy zarządzania bazami danych obsługują pesymistyczne współbieżności. Entity Framework Core nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak wdrożyć go.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Alternatywą dla pesymistycznej współbieżności jest Optymistyczna współbieżność. Współbieżność optymistyczna pozwala na wykonywanie konfliktów współbieżności, a następnie podejmowanie odpowiednich działań. Na przykład Joanna odwiedzi stronę Edycja działu i zmieni kwotę budżetu dla działu angielskiego z $350 000,00 na $0,00.

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

Jan klika pozycję **Zapisz** jako pierwszy i widzi zmiany, gdy przeglądarka powraca do strony indeks.

![Budżet na zero](concurrency/_static/budget-zero.png)

Następnie Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00. Co dzieje się potem określają sposób obsługi konfliktów współbieżności.

Dostępne są następujące opcje:

* Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych.

     W przykładowym scenariuszu żadne dane nie zostaną utracone, ponieważ różne właściwości zostały zaktualizowane przez dwóch użytkowników. Następnym razem, gdy ktoś przegląda ten dział w języku angielskim, zobaczy zmiany w kategorii Janina i Jan — datę początkową 9/1/2013 i budżet zerowych dolarów. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które mogłyby spowodować utratę danych, ale nie może uniknąć utraty danych, jeśli wprowadzono konkurencyjne zmiany w tej samej właściwości jednostki. Czy Entity Framework działa w ten sposób, zależy od sposobu implementacji kodu aktualizacji. Często nie jest to praktyczne w aplikacji sieci Web, ponieważ może wymagać utrzymania dużej ilości danych w celu śledzenia wszystkich oryginalnych wartości właściwości dla jednostki, a także nowych wartości. Obsługa dużych ilości Stanów może wpłynąć na wydajność aplikacji, ponieważ wymaga zasobów serwera lub musi być uwzględniona na stronie sieci Web (na przykład w ukrytych polach) lub w pliku cookie.

* Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.

     Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy 9/1/2013 i przywrócona wartość $350 000,00. Jest to tzw. *klient WINS* lub *ostatni w scenariuszu usługi WINS* . (Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jak zostało to opisane we wprowadzeniu do tej sekcji, jeśli nie wykonasz kodowania na potrzeby obsługi współbieżności, zostanie to wykonane automatycznie.

* Można zapobiec aktualizacji firmy Jan ze zmian w bazie danych.

     Zwykle zostanie wyświetlony komunikat o błędzie, wyświetlenie go w bieżącym stanie danych i umożliwienie mu ponownego zastosowania zmian w przypadku, gdy nadal chce je wprowadzić. Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zostanie wdrożony scenariusz sklepu WINS. Ta metoda zapewnia, że żadne zmiany nie są zastępowane bez alertu użytkownika o tym, co się dzieje.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Konflikty można rozwiązać przez obsługę `DbConcurrencyException` wyjątków zgłaszanych przez Entity Framework. Aby dowiedzieć się, kiedy należy zgłosić te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym należy odpowiednio skonfigurować bazę danych i model danych. Dostępne są następujące opcje włączania wykrywania konfliktów:

* W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony. Następnie można skonfigurować Entity Framework, aby uwzględnić tę kolumnę w klauzuli WHERE polecenia SQL Update lub DELETE.

     Typ danych kolumny śledzenia jest zwykle `rowversion`. Wartość `rowversion` jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W przypadku polecenia Update lub DELETE klauzula WHERE zawiera oryginalną wartość kolumny śledzenia (oryginalną wersję wiersza). Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości, dlatego instrukcja UPDATE lub DELETE nie może znaleźć wiersza do zaktualizowania z powodu klauzuli WHERE. Gdy Entity Framework stwierdzi, że żadne wiersze nie zostały zaktualizowane przez polecenie Update lub Delete (oznacza to, że gdy liczba odnośnych wierszy wynosi zero), interpretuje to jako konflikt współbieżności.

* Skonfiguruj Entity Framework, aby uwzględnić oryginalne wartości każdej kolumny w tabeli w klauzuli WHERE poleceń Update i DELETE.

     Jak w pierwszej opcji, jeśli coś w wierszu uległo zmianie od momentu pierwszego odczytu wiersza, klauzula WHERE nie zwraca wiersza do zaktualizowania, który Entity Framework interpretuje jako konflikt współbieżności. W przypadku tabel bazy danych z wieloma kolumnami takie podejście może skutkować bardzo dużymi klauzulami WHERE i może wymagać utrzymania dużej ilości danych. Jak wspomniano wcześniej, utrzymanie dużej ilości stanu może wpłynąć na wydajność aplikacji. W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.

     Jeśli chcesz zaimplementować te podejście do współbieżności, musisz oznaczyć wszystkie właściwości klucza niepodstawowego w jednostce, dla której chcesz śledzić współbieżność, dodając do nich atrybut `ConcurrencyCheck`. Ta zmiana umożliwia Entity Framework uwzględnienie wszystkich kolumn w klauzuli SQL WHERE instrukcji UPDATE i DELETE.

W pozostałej części tego samouczka dodasz Właściwość śledzenia `rowversion` do jednostki działu, utworzysz kontroler i widoki i testujesz, aby sprawdzić, czy wszystko działa prawidłowo.

## <a name="add-a-tracking-property"></a>Dodaj właściwość śledzenia

W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

Atrybut `Timestamp` określa, że ta kolumna zostanie uwzględniona w klauzuli WHERE poleceń Update i DELETE wysyłanych do bazy danych. Ten atrybut jest nazywany `Timestamp`, ponieważ poprzednie wersje SQL Server używały typu danych SQL `timestamp` przed zapisaniem przez program SQL `rowversion`. Typ .NET dla `rowversion` jest tablicą bajtów.

Jeśli wolisz używać interfejsu API Fluent, możesz użyć metody `IsConcurrencyToken` (w *danych/SchoolContext. cs*), aby określić właściwość śledzenia, jak pokazano w następującym przykładzie:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Przez dodanie właściwości, która zmieniła model bazy danych, należy wykonać kolejną migrację.

Zapisz zmiany i skompiluj projekt, a następnie wprowadź następujące polecenia w oknie polecenia:

```dotnetcli
dotnet ef migrations add RowVersion
```

```dotnetcli
dotnet ef database update
```

## <a name="create-departments-controller-and-views"></a>Tworzenie kontrolera i widoków działów

Tworzy szkielet na kontrolerze i widokach działów jak wcześniej dla studentów, kursów i instruktorów.

![Dział szkieletu](concurrency/_static/add-departments-controller.png)

W pliku *DepartmentsController.cs* Zmień wszystkie cztery wystąpienia elementu "FirstMidName" na "FullName", dzięki czemu listy rozwijane administrator działu będą zawierać pełną nazwę instruktora, a nie tylko nazwisko.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-index-view"></a>Aktualizuj widok indeksu

Aparat tworzenia szkieletów utworzył kolumnę RowVersion w widoku indeks, ale to pole nie powinno być wyświetlane.

Zastąp kod w *widokach/działach/index. cshtml* następującym kodem.

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Spowoduje to zmianę nagłówka na "działy", usunięcie kolumny RowVersion i wyświetlenie pełnej nazwy zamiast imienia administratora.

## <a name="update-edit-methods"></a>Aktualizowanie metod edycji

W metodzie `Edit` narzędzia HttpGet i `Details`, Dodaj `AsNoTracking`. W metodzie `Edit` narzędzia HttpGet Dodaj eager ładowanie dla administratora.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading)]

Zastąp istniejący kod metody HttpPost `Edit` następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Kod rozpoczyna się od próby odczytu działu do zaktualizowania. Jeśli metoda `FirstOrDefaultAsync` zwraca wartość null, dział został usunięty przez innego użytkownika. W takim przypadku kod używa opublikowanych wartości formularza do utworzenia jednostki działu, aby można było ponownie wyświetlić stronę edytowania z komunikatem o błędzie. Alternatywnie nie trzeba ponownie tworzyć jednostki działu, jeśli zostanie wyświetlony komunikat o błędzie bez ponownego wyświetlania pól działu.

Widok przechowuje oryginalną wartość `RowVersion` w ukrytym polu, a ta metoda otrzymuje tę wartość w parametrze `rowVersion`. Przed wywołaniem `SaveChanges`należy umieścić tę oryginalną wartość właściwości `RowVersion` w kolekcji `OriginalValues` dla jednostki.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Następnie, gdy Entity Framework tworzy polecenie SQL UPDATE, to polecenie będzie zawierać klauzulę WHERE, która szuka wiersza, który ma oryginalną wartość `RowVersion`. Jeśli nie ma żadnych wierszy, których dotyczy polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), Entity Framework zgłasza wyjątek `DbUpdateConcurrencyException`.

Kod w bloku catch dla tego wyjątku pobiera jednostkę zależnego działu, która ma zaktualizowane wartości właściwości `Entries` w obiekcie Exception.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

Kolekcja `Entries` będzie zawierać tylko jeden obiekt `EntityEntry`.  Można użyć tego obiektu, aby pobrać nowe wartości wprowadzone przez użytkownika i bieżące wartości bazy danych.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych inne niż wprowadzone przez użytkownika na stronie edytowania (w tym miejscu jest wyświetlane tylko jedno pole dla zwięzłości).

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Na koniec kod ustawia wartość `RowVersion` `departmentToUpdate` do nowej wartości pobranej z bazy danych. Ta nowa `RowVersion` wartość będzie przechowywana w ukrytym polu po ponownym wyświetleniu strony edycji, a przy następnym kliknięciu przycisku **Zapisz**zostaną przechwycone tylko błędy współbieżności, które zachodzą od momentu wyświetlenia ekranu edycji.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`. W widoku wartość `ModelState` pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.

## <a name="update-edit-view"></a>Aktualizuj widok edycji

W obszarze *widoki/działy/Edit. cshtml*wprowadź następujące zmiany:

* Dodaj ukryte pole, aby zapisać wartość właściwości `RowVersion`, bezpośrednio po ukrytym polu właściwości `DepartmentID`.

* Dodaj opcję "Wybierz administratora" do listy rozwijanej.

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts"></a>Testuj konflikty współbieżności

Uruchom aplikację i przejdź do strony indeks działów. Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**, a następnie kliknij hiperłącze **Edytuj** dla działu angielskiego. Dwie karty przeglądarki zawierają teraz te same informacje.

Zmień pole na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

W przeglądarce zostanie wyświetlona strona indeks o zmienionej wartości.

Zmień pole na drugiej karcie przeglądarki.

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

Kliknij przycisk **Save** (Zapisz). Zostanie wyświetlony komunikat o błędzie:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

Kliknij przycisk **Zapisz** ponownie. Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany. Zapisane wartości są wyświetlane, gdy zostanie wyświetlona strona indeks.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Na stronie Usuwanie Entity Framework wykrywa konflikty współbieżności spowodowane przez inną osobę edytującą dział w podobny sposób. Gdy metoda `Delete` narzędzia HttpGet wyświetla widok potwierdzenia, widok zawiera oryginalną wartość `RowVersion` w ukrytym polu. Ta wartość jest następnie dostępna dla metody HttpPost `Delete`, która jest wywoływana, gdy użytkownik potwierdzi usunięcie. Gdy Entity Framework tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z oryginalną wartością `RowVersion`. Jeśli w poleceniu wyniki są równe zero (oznacza to, że wiersz został zmieniony po wyświetleniu strony potwierdzenia usunięcia), zostanie zgłoszony wyjątek współbieżności, a metoda narzędzia HttpGet `Delete` jest wywoływana z flagą błędu ustawioną na wartość true, aby ponownie wyświetlić stronę potwierdzenia z komunikatem o błędzie. Istnieje również możliwość, że zero wierszy miało wpływ, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku nie jest wyświetlany komunikat o błędzie.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Aktualizowanie metod DELETE w kontrolerze działu

W *DepartmentsController.cs*Zastąp metodę narzędzia HttpGet `Delete` następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest ponownie wyświetlana po wystąpieniu błędu współbieżności. Jeśli flaga ma wartość true, a określony dział już nie istnieje, został usunięty przez innego użytkownika. W takim przypadku kod przekieruje się do strony indeksu.  Jeśli ta flaga ma wartość true, a dział istnieje, został zmieniony przez innego użytkownika. W takim przypadku kod wysyła komunikat o błędzie do widoku przy użyciu `ViewData`.

Zastąp kod w metodzie `Delete` HttpPost (o nazwie `DeleteConfirmed`) następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

W kodzie szkieletowym, który właśnie został zastąpiony, ta metoda akceptuje tylko identyfikator rekordu:

```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Ten parametr został zmieniony do wystąpienia jednostki działu utworzonego przez spinacz modelu. Zapewnia to EF dostęp do wartości właściwości RowVersion oprócz klucza rekordu.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Zmieniono także nazwę metody akcji z `DeleteConfirmed` na `Delete`. Kod szkieletu użył nazwy `DeleteConfirmed`, aby nadać metodzie HttpPost unikatową sygnaturę. (Środowisko CLR wymaga przeciążonych metod, aby mieć inne parametry metody). Teraz, gdy podpisy są unikatowe, można naklejić do Konwencji MVC i używać tej samej nazwy dla metod Delete HttpPost i narzędzia HttpGet.

Jeśli dział został już usunięty, Metoda `AnyAsync` zwraca wartość false, a aplikacja po prostu wraca do metody index.

W przypadku przechwyconego błędu współbieżności kod ponownie wyświetla stronę potwierdzenia usuwania i zawiera flagę wskazującą, że powinien zostać wyświetlony komunikat o błędzie współbieżności.

### <a name="update-the-delete-view"></a>Aktualizowanie widoku usuwania

W obszarze *widoki/działy/Delete. cshtml*Zamień kod szkieletowy na następujący kod, który dodaje pole komunikatu o błędzie i ukryte pola dla właściwości DepartmentID i rowversion. Zmiany są wyróżnione.

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Powoduje to wprowadzenie następujących zmian:

* Dodaje komunikat o błędzie między nagłówkami `h2` i `h3`.

* Zastępuje FirstMidName z FullName w polu **administrator** .

* Usuwa pole RowVersion.

* Dodaje ukryte pole dla właściwości `RowVersion`.

Uruchom aplikację i przejdź do strony indeks działów. Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**, a następnie na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu angielskiego.

W pierwszym oknie Zmień jedną z wartości, a następnie kliknij przycisk **Zapisz**:

![Strona Edycja działu po zmianie przed usunięciem](concurrency/_static/edit-after-change-for-delete.png)

Na drugiej karcie kliknij pozycję **Usuń**. Zobaczysz komunikat o błędzie współbieżności, a wartości działu są odświeżane z aktualną wartością w bazie danych.

![Strona potwierdzenia usunięcia działu z błędem współbieżności](concurrency/_static/delete-error.png)

Jeśli klikniesz przycisk **Usuń** ponownie, nastąpi przekierowanie do strony indeks, która pokazuje, że dział został usunięty.

## <a name="update-details-and-create-views"></a>Aktualizowanie szczegółów i Tworzenie widoków

Opcjonalnie można oczyścić kod szkieletowy w szczegółach i w widokach.

Zastąp kod w *widokach/działach/details. cshtml* , aby usunąć kolumnę rowversion i wyświetlić pełną nazwę administratora.

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Zastąp kod w *widokach/działach/Create. cshtml* , aby dodać opcję Select do listy rozwijanej.

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a>Dodatkowe zasoby

 Aby uzyskać więcej informacji o sposobie obsługi współbieżności w EF Core, zobacz [konflikty współbieżności](/ef/core/saving/concurrency).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Informacje o konfliktach współbieżności
> * Dodano Właściwość śledzenia
> * Utworzono kontroler i widok wydziałów
> * Zaktualizowany widok indeksu
> * Zaktualizowane metody edycji
> * Zaktualizowano widok edycji
> * Przetestowane konflikty współbieżności
> * Zaktualizowano stronę usuwania
> * Zaktualizowane szczegóły i Tworzenie widoków

Przejdź do następnego samouczka, aby dowiedzieć się, jak zaimplementować dziedziczenie na poziomie tabeli dla instruktorów i jednostek ucznia.

> [!div class="nextstepaction"]
> [Dalej: Implementuj dziedziczenie w hierarchii na poziomie tabeli](inheritance.md)
