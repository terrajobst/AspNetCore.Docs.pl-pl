---
title: Platforma ASP.NET Core MVC z programem EF Core — współbieżności - 8, 10
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 0ae566a76a2ef656843452ed537b8fdfbddaed22
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090904"
---
# <a name="aspnet-core-mvc-with-ef-core---concurrency---8-of-10"></a>Platforma ASP.NET Core MVC z programem EF Core — współbieżności - 8, 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).

W samouczkach wcześniej przedstawiono sposób zaktualizować dane. W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.

Instrukcje dotyczące tworzenia stron sieci web, które współpracują z jednostką działu i obsługi błędów współbieżności. Na poniższych ilustracjach przedstawiono edytowanie i usuwanie stron, w tym niektóre komunikaty, które są wyświetlane, jeśli wystąpi konflikt współbieżności.

![Strona edytowania działu](concurrency/_static/edit-error.png)

![Strona usuwanie działu](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki w celu edycji, a następnie inny użytkownik aktualizuje dane w tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych. Jeśli nie włączysz wykrywania takie konflikty, ostatnia spojrzenia aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika. W wielu aplikacjach, to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie jest tak naprawdę krytycznego, jeśli niektóre zmiany są zastępowane, koszt programowania współbieżność może przeważają korzyści. W takiej sytuacji nie trzeba skonfigurować aplikację do obsługi konfliktów współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jeśli Twoja aplikacja wymaga uniknąć przypadkowej utraty danych w scenariuszach współbieżności, używają blokady bazy danych jest jednym ze sposobów, aby to zrobić. Jest to nazywane pesymistycznej współbieżności. Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub uzyskać dostęp do aktualizacji. Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub zaktualizować dostępu, ponieważ jaką uzyskują kopii danych, która jest w trakcie zmieniany. Jeśli zablokujesz wiersz, aby uzyskać dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokady ma wady. Może być skomplikowane, aby program. Wymaga znaczących bazy danych zarządzania zasobami, a może to spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa się. Z tego względu nie wszystkie systemy zarządzania bazami danych obsługuje pesymistycznej współbieżności. Entity Framework Core nie obsługuje wbudowanej go, a w tym samouczku nie pokazano, jak ją wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Alternatywa dla Współbieżność pesymistyczna jest optymistycznej współbieżności. Optymistyczna współbieżność oznacza, umożliwiając konfliktów współbieżności do wykonania, a następnie reagowaniu odpowiednio Jeśli tak jest. Na przykład Magdalena odwiedzin strony Edytuj działu i zmienia kwota budżetu dla angielskiego działu z $350,000.00 na 0,00 USD.

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

Magdalena kliknie **Zapisz** pierwszy i widzi jej zmieniać, gdy przeglądarka powróci do strony indeksu.

![Budżet na zero](concurrency/_static/budget-zero.png)

A następnie kliknie John **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $. Co dzieje się potem określają sposób obsługi konfliktów współbieżności.

Niektóre opcje obejmują następujące czynności:

* Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny w bazie danych.

     W przykładowym scenariuszu żadne dane nie zostałyby utracone, ponieważ inne właściwości zostały zaktualizowane przez dwóch użytkowników. Przy następnym ktoś przegląda angielskiej działu, zobaczą zmiany nazwy i John's — datę rozpoczęcia 9/1/2013 i budżetu, zerowego dolarów. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostały wprowadzone w tej samej właściwości jednostki. Czy platformy Entity Framework działa w ten sposób zależy od tego, jak zaimplementować kod aktualizacji. Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu w celu śledzenia oryginalnych wartości właściwości dla jednostki, a także nowe wartości. Obsługa dużych ilości stanu może mieć wpływ na wydajność aplikacji, ponieważ jego wymaga zasobów serwera albo muszą być zawarte na stronie sieci web (na przykład w ukrytych polach) lub w pliku cookie.

* Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.

     Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i przywrócone wartości $350,000.00. Jest to nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza. (Wszystkie wartości z klienta pierwszeństwo co znajduje się w magazynie danych.) Jak wspomniano we wprowadzeniu do tej sekcji, w przeciwnym razie pisania obsługi współbieżności, nastąpi to automatycznie.

* Aby uniemożliwić zmiany John's aktualizowane w bazie danych.

     Zazwyczaj będzie wyświetlony komunikat o błędzie, pokazują, jak bieżący stan danych i umożliwiające podszycie się ponownie zastosuj swoje zmiany, jeśli chce nadal były. Jest to nazywane *Store Wins* scenariusza. (Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) Będzie implementować scenariusza Store Wins w ramach tego samouczka. Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu z wydarzeniami.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Należy rozwiązać konflikty, obsługując `DbConcurrencyException` wyjątki, które zgłasza Entity Framework. Aby wiedzieć, kiedy trzeba generować te wyjątki, platformy Entity Framework musi umożliwiać wykrywanie konfliktów. W związku z tym należy skonfigurować bazy danych oraz model danych odpowiednio. Niektóre opcje umożliwiające wykrywanie konfliktów są następujące:

* W tabeli bazy danych zawierają kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony. Następnie można skonfigurować programu Entity Framework, aby uwzględnić tej kolumny w klauzuli Where klauzuli SQL Update lub Delete poleceń.

     Typ danych kolumny śledzenia jest zazwyczaj `rowversion`. `rowversion` Wartość numeru sekwencyjnego, który jest zwiększana za każdym razem, zaktualizować wiersza. W poleceniu Update lub Delete gdzie klauzula zawiera oryginalna wartość kolumny śledzenia (oryginalna wersja wiersza). Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc instrukcji Update lub Delete nie można odnaleźć wiersza do zaktualizowania ze względu na miejsce klauzuli. Gdy znajdzie Entity Framework, że żadne wiersze nie zostały zaktualizowane, Update lub Delete polecenie (to znaczy, gdy liczba wierszy, których to dotyczy, jest równy zero), interpretuje, jako konflikt współbieżności.

* Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości każdej kolumny w tabeli w klauzuli Where klauzuli polecenia Update i Delete.

     Tak jak w pierwszej opcji, jeśli nic w wierszu zmieniła się od wiersza została najpierw przeczytać artykuł gdzie klauzula nie zwraca wiersz, aby zaktualizować, której platformy Entity Framework interpretuje jako konflikt współbieżności. Dla tabel bazy danych, które mają wiele kolumn, to podejście może doprowadzić do bardzo dużych gdzie zdań i może wymagać obsługi dużych ilości stanu. Jak wspomniano wcześniej, obsługi dużych ilości stanu mogą wpływać na wydajność aplikacji. W związku z tym to podejście zazwyczaj nie zaleca się i nie można go metodę używaną w ramach tego samouczka.

     Jeśli chcesz zaimplementować to podejście do współbieżności, musisz oznaczyć wszystkie właściwości bez klucza podstawowego w jednostce, które chcesz śledzić concurrency, dodając `ConcurrencyCheck` atrybutu do nich. Ta zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w klauzuli SQL Where w instrukcji Update i Delete.

W pozostałej części tego samouczka dodasz `rowversion` śledzenie właściwości do jednostki działu, Utwórz kontrolera i widoki, a test, aby sprawdzić, czy wszystko działa poprawnie.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Dodawanie właściwości śledzenia do jednostki działu

W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` Atrybut określa, że ta kolumna będzie zawierać w Where klauzuli Update i Delete polecenia wysyłane do bazy danych. Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` zastąpiono ją. Typ architektury .NET dla `rowversion` jest tablicą bajtów.

Jeśli wolisz używać interfejsu API fluent, możesz użyć `IsConcurrencyToken` — metoda (w *Data/SchoolContext.cs*) można określić właściwości śledzenia, jak pokazano w poniższym przykładzie:

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

Jak wcześniej dla uczniów, kursy i instruktorów, tworzenia szkieletu kontrolera działów i widoków.

![Tworzenie szkieletu działu](concurrency/_static/add-departments-controller.png)

W *DepartmentsController.cs* plików, zmienić wszystkie cztery wystąpienia "FirstMidName" na "Imię i nazwisko", tak, aby list rozwijanych administratora działu będzie zawierać imię i nazwisko instruktora, a nie po prostu nazwisko.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Aktualizacja widoku indeks działów

Aparat tworzenia szkieletów utworzył RowVersion kolumny w widoku indeksu, ale nie powinny być wyświetlane to pole.

Zastąp kod w *Views/Departments/Index.cshtml* następującym kodem.

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Zmiany pozycji "Wydziałom", usuwa kolumnę RowVersion i pokazuje pełną nazwę zamiast imię administratora.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Aktualizacja metod edycji w kontrolerze działów

W obu narzędzia HttpGet `Edit` metody i `Details` metody, Dodaj `AsNoTracking`. W HttpGet `Edit` metody, Dodaj wczesne ładowanie dla administratora.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Zastąp istniejący kod httppost `Edit` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Na początku kodu próby odczytu z działu do zaktualizowania. Jeśli `SingleOrDefaultAsync` metoda zwraca wartość null, dział został usunięty przez innego użytkownika. W takiej sytuacji kod używa wartości przesłanego formularza, aby utworzyć jednostki dział strony edytowania mogą być wyświetlane ponownie komunikatu o błędzie. Jako alternatywę nie trzeba ponownie utworzyć jednostki działu, jeśli wyświetla komunikat o błędzie bez wyświetlania pól działu.

Widok przechowuje oryginalny `RowVersion` wartości w ukrytym polu, a ta metoda odbiera tę wartość w `rowVersion` parametru. Przed wywołaniem `SaveChanges`, musisz umieścić te oryginalnego `RowVersion` wartości właściwości w `OriginalValues` kolekcji jednostki.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Następnie podczas Entity Framework tworzy polecenie pliki aktualizacji programu SQL, to polecenie będzie zawierać klauzuli WHERE, który wyszukuje dla wiersza zawierającego oryginalne `RowVersion` wartość. Jeśli żadne wiersze nie dotyczy polecenia UPDATE (żadnych wierszy ma oryginalny `RowVersion` wartość), platformy Entity Framework zgłasza `DbUpdateConcurrencyException` wyjątek.

Kod w bloku catch dla tego wyjątku pobiera dotyczy jednostki działu, która ma zaktualizowanymi wartościami z `Entries` właściwość obiektu wyjątku.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` Kolekcja będzie mieć tylko jeden `EntityEntry` obiektu.  Aby uzyskać nowe wartości wprowadzonej przez użytkownika i wartości bieżącej bazy danych, można użyć tego obiektu.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Ten kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma różne wartości bazy danych, z jakiego które użytkownik wprowadził w edycji strony (tylko jedno pole znajduje się w tym miejscu dla zwięzłości).

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Na koniec kod ustawia `RowVersion` wartość `departmentToUpdate` na nową wartość pobierane z bazy danych. Ta nowa `RowVersion` wartość zostanie zapisana w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następnie czasu użytkownik klika polecenie **Zapisz**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie strony edycji zostanie przechwycony.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość. W widoku `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.

## <a name="update-the-department-edit-view"></a>Aktualizacja widoku edycji działu

W *Views/Departments/Edit.cshtml*, wprowadź następujące zmiany:

* Dodaj pole ukryte, aby zapisać `RowVersion` wartości właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości.

* Dodaj opcję "Zaznacz administratora" do listy rozwijanej.

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Badanie konfliktów współbieżności na stronie edycji

Uruchom aplikację i przejdź do strony indeksu działów. Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**, następnie kliknij przycisk **Edytuj** hiperłącze dla angielskiego działu. Karty przeglądarki dwa są teraz wyświetlane w tych samych informacji.

Zmień pole na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

Przeglądarka wyświetla stronę indeksu wartością zmienione.

Zmień pole na drugiej karcie przeglądarki.

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

Kliknij przycisk **Zapisz**. Zobaczysz komunikat o błędzie:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

Kliknij przycisk **Zapisz** ponownie. Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany. Zobaczysz zapisane wartości, gdy zostanie wyświetlona strona indeksu.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Na stronie usuwania programu Entity Framework wykrywa konfliktów współbieżności spowodowanych przez osoby z działu inne do edycji w podobny sposób. Gdy HttpGet `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w ukrytym polu. Czy wartość jest następnie udostępniana HttpPost `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdzi usunięcie. Entity Framework tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z oryginalnym `RowVersion` wartość. Jeśli wyniki polecenia zero wierszy dotyczy (wiersz został zmieniony po stronie potwierdzenia usunięcia został wyświetlony przesyłane), zwracany jest wyjątek współbieżności i narzędzia HttpGet `Delete` metoda jest wywoływana z ustawioną flagą błąd wartość true, aby ponownie wyświetlić Strona potwierdzenia komunikatu o błędzie. Istnieje również możliwość, że zero wierszy została zmieniona, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku jest wyświetlany żaden komunikat o błędzie.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Aktualizacja metod usuwania w kontrolerze działów

W *DepartmentsController.cs*, Zastąp HttpGet `Delete` metoda następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności. Jeśli ta flaga ma wartość true, a dział określono już istnieje, zostało usunięte przez innego użytkownika. W takiej sytuacji kod wykonuje przekierowanie do strony indeksu.  Jeśli ta flaga ma wartość true, a dział istnieje, został zmieniony przez innego użytkownika. W takiej sytuacji kod wysyła komunikat o błędzie na widok za pomocą `ViewData`.

Zastąp kod w HttpPost `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

W utworzony szkielet kodu, który został zastąpiony ta metoda akceptowane tylko identyfikator rekordu:

```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Tego parametru zostało zmienione na wystąpienie jednostki działu utworzone przez integratora modelu. Daje to EF dostęp do wartości właściwości RowVersion oprócz klucza rekordu.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`. Utworzony szkielet kodu użyto nazwy `DeleteConfirmed` zapewnienie metody HttpPost unikatowy podpis. (Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyć tej samej nazwy dla metody usuwania HttpPost i narzędzia HttpGet.

Jeśli dział został już usunięty, `AnyAsync` metoda zwraca wartość false, oraz aplikacji po prostu powraca do metody indeksu.

Jeżeli zostanie przechwycony błąd współbieżności, kod zostanie ponownie strona potwierdzenia usunięcia i zapewnia, że flagę, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.

### <a name="update-the-delete-view"></a>Aktualizacja widoku Delete

W *Views/Departments/Delete.cshtml*, Zastąp następujący kod, który dodaje błąd pola komunikatu i ukrytych pól właściwości DepartmentID i RowVersion utworzony szkielet kodu. Zmiany są wyróżnione.

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

To sprawia, że następujące zmiany:

* Dodaje komunikat o błędzie między `h2` i `h3` nagłówków.

* Zamienia FirstMidName imię i nazwisko w **administratora** pola.

* Usuwa pole RowVersion.

* Dodaje pole ukryte służące `RowVersion` właściwości.

Uruchom aplikację i przejdź do strony indeksu działów. Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**, na pierwszej karcie kliknięcie **Edytuj** hiperłącze dla angielskiego działu.

W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **Zapisz**:

![Strona edytowania działu po zmianie przed delete](concurrency/_static/edit-after-change-for-delete.png)

W drugiej karcie kliknij **Usuń**. Zostanie wyświetlony komunikat o błędzie współbieżności, a wartości Dział zostaną odświeżone przy użyciu co to jest obecnie dostępna w bazie danych.

![Strona potwierdzenia usunięcia działu błąd współbieżności](concurrency/_static/delete-error.png)

Jeśli klikniesz **Usuń** ponownie, użytkownik jest przekierowany do strony indeksu, który pokazuje, że dział został usunięty.

## <a name="update-details-and-create-views"></a>Aktualizowanie szczegółów i tworzenia widoków

Opcjonalnie można wyczyścić utworzony szkielet kodu w szczegółach i tworzenia widoków.

Zastąp kod w *Views/Departments/Details.cshtml* Usuń kolumnę RowVersion i wyświetlić pełną nazwę administratora.

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Zastąp kod w *Views/Departments/Create.cshtml* do dodania zaznacz opcję do listy rozwijanej.

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Podsumowanie

Na tym kończy się wprowadzenie do obsługi konfliktów współbieżności. Aby uzyskać więcej informacji na temat obsługi współbieżności w programie EF Core, zobacz [konfliktów współbieżności](/ef/core/saving/concurrency). Następny samouczek pokazuje, jak zaimplementować Tabela wg hierarchii dziedziczenia dla jednostek przez instruktorów i uczniów.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](update-related-data.md)
> [dalej](inheritance.md)
