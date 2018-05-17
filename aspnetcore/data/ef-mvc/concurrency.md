---
title: Platformy ASP.NET Core MVC podstawowych EF - współbieżności - 8, 10
author: rick-anderson
description: Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 48aa5a2d47c9d60ab8ac5a25f8f29ce8e462dd61
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---concurrency---8-of-10"></a>Platformy ASP.NET Core MVC podstawowych EF - współbieżności - 8, 10

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W starszych samouczkach przedstawiono sposób aktualizowania danych. Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.

Utworzysz stron sieci web, działające z jednostką działu i obsługi błędów współbieżności. Na poniższych ilustracjach przedstawiono edytowanie i usuwanie stron, tym komunikaty, które są wyświetlane, gdy wystąpi konflikt współbieżności.

![Dział edycji strony](concurrency/_static/edit-error.png)

![Strona usuwania działu](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Konfliktom współbieżności

Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki celu jego edycji, a następnie inny użytkownik aktualizuje dane tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych. Nie włączaj wykrywania takie konflikty, ostatnio kto aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika. W wielu aplikacjach to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie są naprawdę krytyczne, jeśli pewne zmiany zostaną zastąpione, kosztów programowania dla współbieżności może przeważają korzyści. W takim przypadku nie trzeba skonfigurować aplikację do obsługi konfliktom współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Pesymistyczne współbieżności (blokowanie)

Jeśli aplikacja potrzeba uniknąć przypadkowej utraty danych w scenariuszach współbieżności, jeden sposób jest użycie blokady bazy danych. Jest to pesymistyczne współbieżności. Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub aktualizacji dostępu. Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub aktualizacji dostępu, ponieważ uzyskują kopii danych, która jest w trakcie zmieniane. Jeśli zablokujesz wiersz dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokad ma wady. Może być skomplikowane, aby program. Wymaga to znaczące bazy danych zarządzania zasobami i może spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa. Z tego względu nie wszystkie systemy zarządzania bazy danych obsługują pesymistyczne współbieżności. Program Entity Framework Core nie zapewnia wbudowanej obsługi dla niego, a w tym samouczku nie pokazuje, jak ją wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistycznej współbieżności

Alternatywą dla pesymistyczne współbieżności jest optymistycznej współbieżności. Optymistycznej współbieżności oznacza stosowanie konfliktom współbieżności mieć miejsce, a następnie reaguje odpowiednio Jeśli. Na przykład Magdalena odwiedzających stronę Edytuj działu i zmienia wielkość budżetu dla angielskiej wersji językowej działu z $350,000.00 na 0,00 USD.

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

Przed kliknie Magdalena **zapisać**, Jan odwiedza tej samej stronie i zmienia pole Data rozpoczęcia z 2007-9-1 do 9/1/2013.

![Zmiana daty rozpoczęcia 2013](concurrency/_static/change-date.png)

Magdalena kliknie **zapisać** pierwszy i widzi jej zmienić po powrocie do strony indeksu z przeglądarki.

![Budżet zmienione od zera.](concurrency/_static/budget-zero.png)

A następnie klika przycisk Jan **zapisać** na stronie edycji nadal pokazujący budżetu 350,000.00 $. Co dalej zależy od sposobu obsługi konfliktom współbieżności.

Niektóre opcje są następujące:

* Można zachować informacje o właściwości, które zostało zmodyfikowane przez użytkownika i aktualizować tylko odpowiednie kolumny w bazie danych.

     W przykładowym scenariuszu żadne dane nie byłoby utracone, ponieważ inne właściwości zostały zaktualizowane przez użytkowników. Przy następnym ktoś przegląda w angielskiej wersji językowej działu, zobaczą zmiany zarówno Joanny i jego Jan — Data początkowa 9/1/2013 i budżetu dolarów zero. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurują zmian z tą samą właściwością jednostki. Czy programu Entity Framework działa w ten sposób zależy od sposobu implementacji kodu aktualizacji. Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu celu śledzenia wszystkich oryginalnej wartości właściwości dla obiektu, a także nowe wartości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji ponieważ go wymaga zasobów serwera lub muszą być zawarte w strony sieci web (na przykład w pola ukryte) lub w pliku cookie.

* Możesz pozwolić, aby zmiany w Jan zastąpić zmiany nazwy.

     Przy następnym ktoś przegląda w angielskiej wersji językowej działu, zobaczą 9/1/2013 i przywrócone wartości $350,000.00. Ta metoda jest wywoływana *klienta Wins* lub *ostatniego w usłudze Wins* scenariusza. (Wszystkie wartości z klienta wyższy priorytet niż co znajduje się w magazynie danych). Zgodnie z opisem w wprowadzenie do tej sekcji, w przeciwnym razie pisania kodu do obsługi współbieżności, nastąpi to automatycznie.

* Aby uniemożliwić zmianę jego Jan aktualizację w bazie danych.

     Zazwyczaj będzie wyświetlony komunikat o błędzie, Pokaż jego bieżący stan danych i Zezwalaj, aby ponownie zastosować jego zmiany, jeśli chce nadal były. Ta metoda jest wywoływana *Wins magazynu* scenariusza. (Wartości magazynu danych mają priorytet nad wartości przesłany przez klienta). W tym samouczku będziesz wdrożyć scenariusz dla magazynu usługi Wins. Ta metoda gwarantuje, że żadne zmiany nie zostaną zastąpione bez użytkownika są alerty o tym, co dzieje.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Obsługa może rozwiązać konflikty `DbConcurrencyException` wyjątki, które generuje programu Entity Framework. Aby wiedzieć, kiedy throw te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym musisz skonfigurować bazę danych i modelu danych odpowiednio. Niektóre opcje umożliwiających wykrywanie konfliktów są następujące:

* W tabeli bazy danych należy dołączyć kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony. Następnie można skonfigurować programu Entity Framework w celu uwzględnienia tej kolumny w klauzuli Where klauzuli SQL Update lub Delete poleceń.

     Typ danych kolumny śledzenia jest zwykle `rowversion`. `rowversion` Wartość jest liczbą sekwencyjnych, który jest zwiększany po każdej zaktualizować wiersza. W poleceniu Update lub Delete gdzie klauzula zawiera oryginalnej wartości kolumny śledzenia (oryginalnej wersji wierszy). Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc instrukcji Update lub Delete nie można odnaleźć wiersza do aktualizacji z powodu Where klauzuli. Gdy znajdzie Entity Framework, że żadne wiersze nie zostały zaktualizowane przez aktualizacji lub usunięcia polecenia (Jeśli liczba wierszy, których dotyczy to zero), interpretuje który jako konflikt współbieżności.

* Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości wszystkich kolumn w tabeli w klauzuli Where klauzuli poleceń Update i Delete.

     Jak pierwsza opcja, jeśli dowolny wiersz zmieniła się od najpierw odczytano wiersza gdzie klauzuli nie zwrócą wiersza do aktualizacji, które programu Entity Framework interpretowane jako konflikt współbieżności. Dla tabel bazy danych, które mają wiele kolumn, ta metoda może spowodować bardzo dużych Where klauzule i może wymagać, aby zachować stan dużych ilości. Jak wspomniano wcześniej, obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji. W związku z tym tej metody zwykle nie jest zalecane, a nie jest ona metodę używaną w tym samouczku.

     Jeśli chcesz wdrożyć takie podejście do współbieżności, masz Oznacz wszystkie właściwości bez klucza podstawowego w jednostce chcesz śledzić concurrency przez dodanie `ConcurrencyCheck` atrybutu do nich. Taka zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w klauzuli SQL Where w instrukcji Update i Delete.

W pozostałej części tego samouczka zostanie dodana `rowversion` śledzenia właściwości jednostki działu, Tworzenie kontrolera i widoków i sprawdzenie, czy wszystko działa prawidłowo.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Dodaj właściwość śledzenia do działu jednostki

W *Models/Department.cs*, Dodaj właściwość śledzenia o nazwie RowVersion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` Atrybut określa, że w tej kolumnie będą uwzględniane w Where klauzuli Update i Delete polecenia wysyłane do bazy danych. Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server SQL `timestamp` — typ danych przed SQL `rowversion` on zastąpiony. Typ architektury .NET dla `rowversion` jest tablicą bajtów.

Jeśli wolisz korzystać z interfejsu API fluent, możesz użyć `IsConcurrencyToken` — metoda (w *Data/SchoolContext.cs*) do określenia właściwości śledzenia, jak pokazano w poniższym przykładzie:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Przez dodanie właściwości po zmianie modelu bazy danych, więc należy przeprowadzić migrację z innej.

Zapisz zmiany i skompiluj projekt, a następnie wprowadź następujące polecenia w oknie wiersza polecenia:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Tworzenie kontrolera działów i widoków

Tak jak wcześniej dla uczniów lub studentów, szkoleń i instruktorów szkieletu kontrolera działów i widoków.

![Dział szkieletu](concurrency/_static/add-departments-controller.png)

W *DepartmentsController.cs* pliku, zmienić wszystkie cztery wystąpienia "FirstMidName" na "Pełna nazwa", tak aby list rozwijanych administratora działu będzie zawierać pełną nazwę instruktora, a nie tylko nazwisko.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Aktualizowanie widoku indeksu działów

Aparat szkieletów utworzył RowVersion kolumny w widoku indeksu, ale nie powinny być wyświetlane tego pola.

Zastąp kod w *Views/Departments/Index.cshtml* następującym kodem.

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Zmiany pozycji do "Działów", usuwa kolumnę RowVersion i zawiera pełną nazwę zamiast imię dla administratora.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Metody edycji w kontrolerze działów aktualizacji

W obu HttpGet `Edit` — metoda i `Details` metody, Dodaj `AsNoTracking`. W HttpGet `Edit` metody, Dodaj wczesny ładowania dla administratora.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Zastąp istniejący kod httppost `Edit` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Na początku kodu próby odczytu z działu do zaktualizowania. Jeśli `SingleOrDefaultAsync` metoda zwraca wartość null, dział został usunięty przez innego użytkownika. W takim przypadku ten kod używa wartości przesłanego formularza utworzyć jednostki działu, tak aby edycji strony mogą być wyświetlane ponownie z komunikatem o błędzie. Alternatywnie nie trzeba ponownie utworzyć jednostki działu, jeśli wyświetla komunikat o błędzie bez ponowne wyświetlanie pola działu.

Widok przechowuje oryginalnej `RowVersion` wartość w ukrytym polu i ta metoda odbiera tę wartość w `rowVersion` parametru. Przed wywołaniem `SaveChanges`, trzeba umieścić który oryginalnego `RowVersion` wartości właściwości w `OriginalValues` kolekcji jednostki.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Następnie podczas programu Entity Framework utworzy polecenia aktualizacji SQL, to polecenie będzie zawierać klauzuli WHERE, sprawdzający wiersza, który ma oryginalną `RowVersion` wartość. Jeśli żadne wiersze nie dotyczy polecenia UPDATE (żadnych wierszy ma oryginalną `RowVersion` wartość), programu Entity Framework zgłasza `DbUpdateConcurrencyException` wyjątek.

Kod w bloku catch dla tego wyjątku pobiera dotyczy jednostki działu, która ma zaktualizowanej wartości z `Entries` właściwości w obiekcie wyjątku.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` Kolekcja będzie mieć tylko jedno `EntityEntry` obiektu.  Ten obiekt służy do nowych wartości wprowadzonej przez użytkownika i wartości bieżącej bazy danych.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Ten kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma różne wartości bazy danych z wprowadzoną na edycję użytkownika strony (tylko jedno pole znajduje się tutaj w celu jego skrócenia).

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Na koniec kod ustawia `RowVersion` wartość `departmentToUpdate` do nowej wartości pobrane z bazy danych. Nowy `RowVersion` wartości będą przechowywane w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następne czasu użytkownik klika polecenie **zapisać**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie edycji strony zostanie przechwycony.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove` Instrukcja jest wymagane, ponieważ `ModelState` ma stary `RowVersion` wartość. W widoku `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości w modelu, gdy istnieją obie.

## <a name="update-the-department-edit-view"></a>Aktualizacja działu Edytuj widok

W *Views/Departments/Edit.cshtml*, wprowadź następujące zmiany:

* Dodaj ukryte pole, aby zapisać `RowVersion` wartość właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości.

* Dodaj opcję "Wybierz administratora" do listy rozwijanej.

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Testowanie konfliktom współbieżności na stronie edycji

Uruchom aplikację i przejdź do strony indeksu działów. Kliknij prawym przyciskiem myszy **Edytuj** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie**, następnie kliknij przycisk **Edytuj** hyperlink działu angielskiej wersji językowej. Karty przeglądarki dwóch jest teraz wyświetlany tych samych informacji.

Zmień pola w pierwszej karcie przeglądarki i kliknij **zapisać**.

![Edytowanie działu strony 1 po zmianie](concurrency/_static/edit-after-change-1.png)

Przeglądarka wyświetla stronę indeksu o wartości zmienione.

Zmień pola w drugiej karty przeglądarki.

![Edytowanie działu strony 2 po zmianie](concurrency/_static/edit-after-change-2.png)

Kliknij przycisk **zapisać**. Zostanie wyświetlony komunikat o błędzie:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

Kliknij przycisk **zapisać** ponownie. Wartość wprowadzona w drugiej karty przeglądarki jest zapisywana. Zostanie wyświetlony zapisanych wartości, gdy zostanie wyświetlona strona indeksu.

## <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

Na stronie usuwania programu Entity Framework wykrywa konfliktom współbieżności spowodowane przez kogoś else edycji dział w podobny sposób. Gdy HttpGet `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w polu ukrytym. Czy wartość jest następnie udostępniana HttpPost `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdza usunięcie. Entity Framework utworzy polecenia SQL DELETE, zawiera klauzulę WHERE z oryginalną `RowVersion` wartość. Jeśli wyniki poleceń w żadnych wierszy dotyczy (znaczenie wiersz został zmieniony po stronie potwierdzenia usunięcia została wyświetlona), zwracany jest wyjątek współbieżności, a HttpGet `Delete` metoda jest wywoływana z ustawioną flagą błąd na true, aby ponownie wyświetlić Strona potwierdzenia z komunikatem o błędzie. Istnieje również możliwość, że zero wierszy została zmieniona, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku jest wyświetlany żaden komunikat o błędzie.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Metody Delete w kontrolerze działów aktualizacji

W *DepartmentsController.cs*, Zastąp HttpGet `Delete` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności. Jeśli ta flaga ma wartość true, a dział określono już nie istnieje, został usunięty przez innego użytkownika. W takim przypadku kod przekierowuje do strony indeksu.  Jeśli ta flaga ma wartość true, a dział istnieje, został zmieniony przez innego użytkownika. W takim przypadku kod wysyła komunikat o błędzie do widoku przy użyciu `ViewData`.  

Zastąp kod w HttpPost `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

W kodzie szkieletu po prostu zastąpić ta metoda zaakceptowane identyfikator rekordu:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Ten parametr zmieniono do wystąpienia jednostki działu utworzone przez integratora modelu. To zapewnia EF dostęp do wartości właściwości RowVersion oprócz klucza rekordu.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Również zmieniono nazwę metody akcji `DeleteConfirmed` do `Delete`. Kod z utworzonym szkieletem użyta nazwa `DeleteConfirmed` umożliwiają metody HttpPost unikatowego podpisu. (CLR wymaga przeciążonej metody mają parametry innej metody). Teraz, czy podpisy są unikatowe, można przestrzegaj Konwencji MVC i używać tej samej nazwy dla metody delete HttpPost i HttpGet.

Jeśli dział został już usunięty, `AnyAsync` metoda zwraca wartość false i aplikacji po prostu wraca do metody indeksu.

Jeśli zostanie przechwycony błąd współbieżności, kod zostanie ponownie stronę potwierdzenia Delete i udostępnia Flaga, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.

### <a name="update-the-delete-view"></a>Aktualizowanie widoku Delete

W *Views/Departments/Delete.cshtml*, Zastąp następujący kod, który dodaje błąd pola wiadomości i ukryte pola dla właściwości DepartmentID i RowVersion szkieletu kodu. Zmiany zostały wyróżnione.

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Dzięki temu następujące zmiany:

* Dodaje komunikat o błędzie między `h2` i `h3` nagłówków.

* Zamienia FirstMidName imię i nazwisko w **administratora** pola.

* Usuwa pole RowVersion.

* Dodaje ukryte pole umożliwiające `RowVersion` właściwości.

Uruchom aplikację i przejdź do strony indeksu działów. Kliknij prawym przyciskiem myszy **usunąć** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie**, następnie na karcie pierwszy kliknij **Edytuj** hyperlink działu angielskiej wersji językowej.

W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **zapisać**:

![Dział edycji strony po zmianie przed usunięciem](concurrency/_static/edit-after-change-for-delete.png)

Na karcie drugi kliknij **usunąć**. Zostanie wyświetlony komunikat o błędzie współbieżności, a dział wartości są odświeżane co to jest obecnie w bazie danych.

![Strona potwierdzenia usunięcia działu błąd współbieżności](concurrency/_static/delete-error.png)

Jeśli klikniesz przycisk **usunąć** ponownie, że przekierowanie do strony indeksu, który pokazuje, czy dział została usunięta.

## <a name="update-details-and-create-views"></a>Szczegółowe informacje dotyczące aktualizacji i tworzyć widoki

Opcjonalnie można wyczyścić szkieletu kodu w szczegółach i tworzyć widoki.

Zastąp kod w *Views/Departments/Details.cshtml* Aby usunąć kolumnę RowVersion i wyświetlić pełną nazwę administratora.

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Zastąp kod w *Views/Departments/Create.cshtml* do dodania do listy rozwijanej wybierz opcję.

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Podsumowanie

Na tym kończy się wprowadzenie do obsługi konfliktom współbieżności. Aby uzyskać więcej informacji na temat obsługi współbieżność w EF Core, zobacz [konfliktom współbieżności](https://docs.microsoft.com/ef/core/saving/concurrency). Następny samouczek pokazuje, jak do zaimplementowania tabeli na hierarchii dziedziczenia dla jednostek instruktora i uczniów.

> [!div class="step-by-step"]
> [Poprzednie](update-related-data.md)
> [dalej](inheritance.md)  
