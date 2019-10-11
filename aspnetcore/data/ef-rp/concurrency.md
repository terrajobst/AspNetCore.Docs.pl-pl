---
title: Razor Pages z EF Core w ASP.NET Core-concurrency-8 z 8
author: rick-anderson
description: W tym samouczku pokazano, jak obsłużyć konflikty, gdy wielu użytkowników jednocześnie zaktualizuje tę samą jednostkę.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: 944e746624bf5fe7c586a521059fa4eb34b0f1e7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259380"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>Razor Pages z EF Core w ASP.NET Core-concurrency-8 z 8

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)i [Jan P Kowalski](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

W tym samouczku pokazano, jak obsłużyć konflikty, gdy wielu użytkowników aktualizuje jednostkę współbieżnie (w tym samym czasie).

## <a name="concurrency-conflicts"></a>Konflikty współbieżności

Konflikt współbieżności występuje, gdy:

* Użytkownik przechodzi do strony Edycja dla jednostki.
* Inny użytkownik aktualizuje tę samą jednostkę przed zapisaniem zmiany pierwszego użytkownika w bazie danych.

Jeśli wykrywanie współbieżności nie jest włączone, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników. Jeśli to ryzyko jest akceptowalne, koszt programowania dla współbieżności może być korzystny.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jednym ze sposobów zapobiegania konfliktom współbieżności jest użycie blokad bazy danych. Jest to nazywane pesymistyczną współbieżnością. Zanim aplikacja odczyta wiersz bazy danych, który zamierza zaktualizować, żąda blokady. Gdy wiersz jest zablokowany na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie może zablokować wiersza do momentu zwolnienia pierwszej blokady.

Zarządzanie blokadami ma wady. Może być skomplikowany dla programu i może spowodować problemy z wydajnością w miarę zwiększania się liczby użytkowników. Entity Framework Core nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak wdrożyć go.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Optymistyczna współbieżność umożliwia konflikty współbieżności, a następnie ponownie wykonuje odpowiednie działania. Na przykład Joanna odwiedzi stronę Edycja działu i zmieni budżet działu angielskiego z $350 000,00 na $0,00.

![Zmiana wartości budżetu na 0](concurrency/_static/change-budget30.png)

Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.

![Zmiana daty rozpoczęcia na 2013](concurrency/_static/change-date30.png)

Janina klika pozycję **Zapisz** pierwszy i widzimy, że zmiany zaczęły obowiązywać, ponieważ przeglądarka wyświetla stronę indeksu z zerem jako kwotą budżetu.

Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00. Co się stanie dalej, zależy od sposobu obsługi konfliktów współbieżności:

* Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych.

  W tym scenariuszu żadne dane nie zostaną utracone. Różne właściwości zostały zaktualizowane przez dwóch użytkowników. Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy zmiany w kategorii Janina i Jan. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które mogłyby spowodować utratę danych. Takie podejście ma pewne wady:
 
  * Nie można uniknąć utraty danych, jeśli wprowadzono konkurencyjne zmiany w tej samej właściwości.
  * Zwykle nie jest to praktyczne w aplikacji sieci Web. Wymaga utrzymywania znaczących stanów w celu śledzenia wszystkich pobranych wartości i nowych wartości. Utrzymywanie dużej ilości stanu może wpłynąć na wydajność aplikacji.
  * Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.

* Możesz pozwolić, aby zmiana została zastąpiona przez Jan.

  Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy 9/1/2013 i pobraną wartość $350 000,00. Ta metoda jest nazywana *klientem WINS* lub *ostatnim scenariuszem usługi WINS* . (Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.

* Można zapobiec aktualizacji firmy Jan ze zmian w bazie danych. Zazwyczaj aplikacja będzie:

  * Wyświetla komunikat o błędzie.
  * Pokaż bieżący stan danych.
  * Zezwalaj użytkownikowi na ponowne stosowanie zmian.

  Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS. Ta metoda zapewnia, że żadne zmiany nie są zastępowane bez alertu użytkownika.

## <a name="conflict-detection-in-ef-core"></a>Wykrywanie konfliktów w EF Core

EF Core zgłasza wyjątki `DbConcurrencyException` w przypadku wykrycia konfliktów. Model danych musi być skonfigurowany, aby umożliwić wykrywanie konfliktów. Opcje włączania wykrywania konfliktów obejmują następujące elementy:

* Skonfiguruj EF Core, aby uwzględnić oryginalne wartości kolumn skonfigurowanych jako [tokeny współbieżności](/ef/core/modeling/concurrency) w klauzuli WHERE poleceń Update i DELETE.

  Gdy zostanie wywołana `SaveChanges`, klauzula WHERE szuka oryginalnych wartości wszelkich właściwości adnotacji z atrybutem [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) . Instrukcja Update nie znajdzie wiersza do zaktualizowania, jeśli którykolwiek z właściwości tokenu współbieżności został zmieniony od momentu pierwszego odczytu wiersza. EF Core interpretuje ten sposób jako konflikt współbieżności. W przypadku tabel bazy danych z wieloma kolumnami takie podejście może skutkować bardzo dużymi klauzulami WHERE i może wymagać dużej ilości danych. W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.

* W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony.

  W SQL Server bazie danych typem danych kolumny śledzenia jest `rowversion`. Wartość `rowversion` jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W przypadku polecenia Update lub DELETE klauzula WHERE zawiera oryginalną wartość kolumny śledzenia (numer wersji pierwszego wiersza). Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości. W takim przypadku instrukcja UPDATE lub DELETE nie może znaleźć wiersza do zaktualizowania z powodu klauzuli WHERE. EF Core zgłasza wyjątek współbieżności, gdy polecenie Update lub DELETE nie ma na nich żadnych wierszy.

## <a name="add-a-tracking-property"></a>Dodaj właściwość śledzenia

W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) wskazuje, co identyfikuje kolumnę jako kolumnę śledzenia współbieżności. Interfejs API Fluent jest alternatywnym sposobem określenia właściwości śledzenia:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W przypadku bazy danych SQL Server atrybut `[Timestamp]` właściwości Entity został zdefiniowany jako tablica bajtowa:

* Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.
* Ustawia typ kolumny w bazie danych na [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).

Baza danych generuje sekwencyjny numer wersji wiersza, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete` klauzula `Where` zawiera wartość wersji wiersza do pobrania. Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:

* Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.
* Polecenia `Update` lub `Delete` nie szukają wiersza, ponieważ klauzula `Where` szuka wartości wersji wiersza pobrania.
* Zostanie zgłoszony `DbUpdateConcurrencyException`.

Poniższy kod przedstawia część języka T-SQL wygenerowanego przez EF Core w przypadku zaktualizowania nazwy działu:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

Poprzedni wyróżniony kod pokazuje klauzulę `WHERE` zawierającą `RowVersion`. Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.

Następujący wyróżniony kod pokazuje T-SQL, który sprawdza dokładnie jeden wiersz został zaktualizowany:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

[@ @ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy, na które miało wpływ Ostatnia instrukcja. Jeśli żadne wiersze nie są aktualizowane, EF Core zgłasza `DbUpdateConcurrencyException`.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W przypadku bazy danych programu SQLite atrybut `[Timestamp]` właściwości Entity został zdefiniowany jako tablica bajtowa:

* Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.
* Mapuje do typu kolumny obiektu BLOB.

Wyzwalacze bazy danych aktualizują kolumnę RowVersion za pomocą nowej losowej tablicy bajtów za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete` klauzula `Where` zawiera pobraną wartość kolumny RowVersion. Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:

* Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.
* Polecenie `Update` lub `Delete` nie znajduje wiersza, ponieważ klauzula `Where` szuka pierwotnej wartości wersji wiersza.
* Zostanie zgłoszony `DbUpdateConcurrencyException`.

---

### <a name="update-the-database"></a>Aktualizowanie bazy danych

Dodanie właściwości `RowVersion` zmienia model danych, który wymaga migracji.

Skompiluj projekt. 

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uruchom następujące polecenie w obszarze PMC:

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Uruchom następujące polecenie w terminalu:

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

To polecenie:

* Tworzy plik migracji *_RowVersion. cs migracji/{Time Datownik}* .
* Aktualizuje plik *migrations/SchoolContextModelSnapshot. cs* . Aktualizacja dodaje następujący wyróżniony kod do metody `BuildModel`:

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uruchom następujące polecenie w obszarze PMC:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz plik `Migrations/<timestamp>_RowVersion.cs` i Dodaj wyróżniony kod:

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  Poprzedni kod:

  * Aktualizuje istniejące wiersze z losowymi wartościami obiektów BLOB.
  * Dodaje wyzwalacze bazy danych, które ustawiają kolumnę RowVersion na losową wartość obiektu BLOB za każdym razem, gdy wiersz zostanie zaktualizowany.

* Uruchom następujące polecenie w terminalu:

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a>Strony działu szkieletu

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Postępuj zgodnie z instrukcjami na [stronach uczniów tworzenia szkieletów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:

* Utwórz folder *strony/działy* .  
* Użyj `Department` dla klasy modelu.
  * Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Utwórz folder *strony/działy* .

* Uruchom następujące polecenie, aby uzyskać szkielet na stronach działu.

  **W systemie Windows:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  **W systemie Linux lub macOS:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

Skompiluj projekt.

## <a name="update-the-index-page"></a>Aktualizowanie strony indeksu

Narzędzie tworzenia szkieletów utworzyło kolumnę `RowVersion` dla strony indeks, ale to pole nie będzie wyświetlane w aplikacji produkcyjnej. W tym samouczku zostanie wyświetlony ostatni bajt `RowVersion`, aby zobaczyć, jak działa obsługa współbieżności. Ostatni bajt nie gwarantuje, że jest unikatowy.

Strona aktualizacji *Pages\Departments\Index.cshtml* :

* Zamień indeks na działy.
* Zmień kod zawierający `RowVersion`, aby pokazać tylko ostatni bajt tablicy bajtów.
* Zamień FirstMidName na FullName.

Poniższy kod przedstawia zaktualizowaną stronę:

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a>Aktualizowanie modelu strony edycji

Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) jest aktualizowany przy użyciu wartości `rowVersion` z jednostki, gdy została ona pobrana w metodzie `OnGet`. EF Core generuje polecenie SQL UPDATE z klauzulą WHERE zawierającą oryginalną wartość `RowVersion`. Jeśli nie ma żadnych wierszy, których dotyczy polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), zostanie zgłoszony wyjątek `DbUpdateConcurrencyException`.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

W poprzednim wyróżnionym kodzie:

* Wartość w `Department.RowVersion` wskazuje, co było w jednostce, gdy została pierwotnie pobrana w żądaniu pobrania dla strony edycji. Wartość jest dostarczana do metody `OnPost` przez ukryte pole na stronie Razor, które wyświetla jednostkę do edycji. Wartość pola ukrytego jest kopiowana do `Department.RowVersion` przez spinacz modelu.
* `OriginalValue` to jakie EF Core będą używane w klauzuli WHERE. Przed wykonaniem wyróżnionego wiersza kodu `OriginalValue` ma wartość znajdującą się w bazie danych, gdy w tej metodzie została wywołana funkcja `FirstOrDefaultAsync`, która może się różnić od tego, co zostało wyświetlone na stronie Edycja.
* Wyróżniony kod sprawdza, czy EF Core używa oryginalnej wartości `RowVersion` z wyświetlonej jednostki `Department` w klauzuli WHERE instrukcji SQL UPDATE.

Gdy wystąpi błąd współbieżności, poniższy wyróżniony kod pobiera wartości klienta (wartości ogłoszone w tej metodzie) oraz wartości bazy danych.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

Następujący wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych. Następnym razem, gdy użytkownik kliknie przycisk **Zapisz**, zostanie przechwycony tylko błąd współbieżności występujący od momentu ostatniego wyświetlenia strony edycji.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`. Na stronie Razor wartość `ModelState` dla pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.

### <a name="update-the-razor-page"></a>Aktualizowanie strony Razor

Zaktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego kodu:

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Poprzedni kod:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje ukrytą wersję wiersza. należy dodać `RowVersion`, aby po zwrotze ponownie powiązać wartość.
* Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.
* Zastępuje `ViewData` z silnie wpisaną `InstructorNameSL`.

### <a name="test-concurrency-conflicts-with-the-edit-page"></a>Konflikty współbieżności testów ze stroną edycji

Otwórz dwa wystąpienia przeglądarki edycji dla działu angielskiego:

* Uruchom aplikację i wybierz pozycję działy.
* Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**.
* Na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu w języku angielskim.

Dwie karty przeglądarki wyświetlają te same informacje.

Zmień nazwę na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edycja działu Strona 1 po zmianie](concurrency/_static/edit-after-change-130.png)

W przeglądarce zostanie wyświetlona strona indeks z wartością zmieniona i zaktualizowanym wskaźnikiem rowVersion. Zanotuj zaktualizowany wskaźnik rowVersion, który jest wyświetlany na drugim serwerze ogłaszania zwrotnego na drugiej karcie.

Zmień inne pole w drugiej karcie przeglądarki.

![Edycja działu Strona 2 po zmianie](concurrency/_static/edit-after-change-230.png)

Kliknij przycisk **Save** (Zapisz). Komunikaty o błędach są wyświetlane dla wszystkich pól, które nie pasują do wartości bazy danych:

![Komunikat o błędzie strony edytowania działu](concurrency/_static/edit-error30.png)

To okno przeglądarki nie zamiało zmiany pola Nazwa. Skopiuj i wklej bieżącą wartość (języki) do pola Nazwa. Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.

Kliknij przycisk **Zapisz** ponownie. Wartość wprowadzona na drugiej karcie przeglądarki jest zapisywana. Zapisane wartości są widoczne na stronie indeks.

## <a name="update-the-delete-page"></a>Aktualizowanie strony usuwania

Zaktualizuj *strony/działy/Delete. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

Strona usuwania wykrywa konflikty współbieżności, gdy jednostka została zmieniona po pobraniu. `Department.RowVersion` to wersja wiersza, kiedy jednostka została pobrana. Gdy EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`. Jeśli polecenie SQL DELETE skutkuje niezerowymi wierszami:

* @No__t-0 w poleceniu SQL DELETE nie jest zgodny z `RowVersion` w bazie danych.
* Zgłaszany jest wyjątek DbUpdateConcurrencyException.
* `OnGetAsync` jest wywoływana z `concurrencyError`.

### <a name="update-the-delete-razor-page"></a>Aktualizowanie strony usuwania Razor

Zaktualizuj *strony/działy/Delete. cshtml* przy użyciu następującego kodu:

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Poprzedni kod wprowadza następujące zmiany:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje komunikat o błędzie.
* Zastępuje FirstMidName z FullName w polu **administrator** .
* Zmiany `RowVersion` w celu wyświetlenia ostatniego bajtu.
* Dodaje ukrytą wersję wiersza. należy dodać `RowVersion`, aby postgit dodać ponownie wiąże wartość.

### <a name="test-concurrency-conflicts"></a>Testuj konflikty współbieżności

Tworzenie działu testowego.

Otwórz dwa wystąpienia przeglądarki usuwania dla działu testowego:

* Uruchom aplikację i wybierz pozycję działy.
* Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu testowego i wybierz polecenie **Otwórz na nowej karcie**.
* Kliknij hiperłącze **Edytuj** dla działu testowego.

Dwie karty przeglądarki wyświetlają te same informacje.

Zmień budżet na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

W przeglądarce zostanie wyświetlona strona indeks z wartością zmieniona i zaktualizowanym wskaźnikiem rowVersion. Zanotuj zaktualizowany wskaźnik rowVersion, który jest wyświetlany na drugim serwerze ogłaszania zwrotnego na drugiej karcie.

Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych. Kliknięcie przycisku **Usuń** powoduje usunięcie jednostki, chyba że `RowVersion` została zaktualizowana. dział został usunięty.

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Tokeny współbieżności w EF Core](/ef/core/modeling/concurrency)
* [Obsługa współbieżności w EF Core](/ef/core/saving/concurrency)
* [Debugowanie ASP.NET Core 2. x](https://github.com/aspnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a>Następne kroki

Jest to ostatni samouczek z serii. Dodatkowe tematy zostały omówione w [wersji MVC tej serii samouczków](xref:data/ef-mvc/index).

> [!div class="step-by-step"]
> [Poprzedni samouczek](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym samouczku pokazano, jak obsłużyć konflikty, gdy wielu użytkowników aktualizuje jednostkę współbieżnie (w tym samym czasie). Jeśli występują problemy, których nie można rozwiązać, [Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

## <a name="concurrency-conflicts"></a>Konflikty współbieżności

Konflikt współbieżności występuje, gdy:

* Użytkownik przechodzi do strony Edycja dla jednostki.
* Inny użytkownik aktualizuje tę samą jednostkę przed zapisaniem zmiany pierwszego użytkownika w bazie danych.

Jeśli wykrywanie współbieżności nie jest włączone, gdy wystąpią współbieżne aktualizacje:

* Ostatnia aktualizacja usługi WINS. Oznacza to, że ostatnie wartości aktualizacji są zapisywane w bazie danych.
* Pierwszy z bieżących aktualizacji zostanie utracony.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Optymistyczna współbieżność umożliwia konflikty współbieżności, a następnie ponownie wykonuje odpowiednie działania. Na przykład Joanna odwiedzi stronę Edycja działu i zmieni budżet działu angielskiego z $350 000,00 na $0,00.

![Zmiana wartości budżetu na 0](concurrency/_static/change-budget.png)

Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.

![Zmiana daty rozpoczęcia na 2013](concurrency/_static/change-date.png)

Jan klika pozycję **Zapisz** jako pierwszy i widzi zmiany, gdy przeglądarka wyświetli stronę indeksu.

![Budżet zmieniony na zero](concurrency/_static/budget-zero.png)

Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00. Co się stanie dalej, zależy od sposobu obsługi konfliktów współbieżności.

Współbieżność optymistyczna obejmuje następujące opcje:

* Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych.

  W tym scenariuszu żadne dane nie zostaną utracone. Różne właściwości zostały zaktualizowane przez dwóch użytkowników. Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy zmiany w kategorii Janina i Jan. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które mogłyby spowodować utratę danych. Takie podejście:
 
  * Nie można uniknąć utraty danych, jeśli wprowadzono konkurencyjne zmiany w tej samej właściwości.
  * Zwykle nie jest to praktyczne w aplikacji sieci Web. Wymaga utrzymywania znaczących stanów w celu śledzenia wszystkich pobranych wartości i nowych wartości. Utrzymywanie dużej ilości stanu może wpłynąć na wydajność aplikacji.
  * Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.

* Możesz pozwolić, aby zmiana została zastąpiona przez Jan.

  Następnym razem, gdy ktoś przegląda dział w języku angielskim, zobaczy 9/1/2013 i pobraną wartość $350 000,00. Ta metoda jest nazywana *klientem WINS* lub *ostatnim scenariuszem usługi WINS* . (Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.

* Można zapobiec aktualizacji firmy Jan ze zmian w bazie danych. Zazwyczaj aplikacja będzie:

  * Wyświetla komunikat o błędzie.
  * Pokaż bieżący stan danych.
  * Zezwalaj użytkownikowi na ponowne stosowanie zmian.

  Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS. Ta metoda zapewnia, że żadne zmiany nie są zastępowane bez alertu użytkownika.

## <a name="handling-concurrency"></a>Obsługa współbieżności 

Gdy właściwość jest skonfigurowana jako [Token współbieżności](/ef/core/modeling/concurrency):

* EF Core sprawdza, czy właściwość nie została zmodyfikowana po pobraniu. Sprawdzanie występuje, gdy wywoływana jest [metody SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) .
* Jeśli właściwość została zmieniona po pobraniu, zgłaszany jest [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) . 

Baza danych i model danych muszą być skonfigurowane tak, aby obsługiwały generowanie `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Wykrywanie konfliktów współbieżności dla właściwości

Konflikty współbieżności mogą być wykrywane na poziomie właściwości przy użyciu atrybutu [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) . Ten atrybut może być stosowany do wielu właściwości w modelu. Aby uzyskać więcej informacji, zobacz [Adnotacje danych — ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).

Atrybut `[ConcurrencyCheck]` nie jest używany w tym samouczku.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Wykrywanie konfliktów współbieżności w wierszu

Aby wykrywać konflikty współbieżności, do modelu dodawana jest kolumna śledzenia [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) .  `rowversion`:

* SQL Server specyficzny. Inne bazy danych mogą nie zapewniać podobnej funkcji.
* Służy do określenia, że jednostka nie została zmieniona od czasu pobrania z bazy danych. 

Baza danych generuje numer sekwencyjny `rowversion`, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete` klauzula `Where` zawiera pobraną wartość `rowversion`. Jeśli aktualizowany wiersz został zmieniony:

* `rowversion` nie pasuje do pobranej wartości.
* Polecenia `Update` lub `Delete` nie wyszukują wiersza, ponieważ klauzula `Where` zawiera pobrane `rowversion`.
* Zostanie zgłoszony `DbUpdateConcurrencyException`.

W EF Core, gdy żadne wiersze nie zostały zaktualizowane przez polecenie `Update` lub `Delete`, zostanie zgłoszony wyjątek współbieżności.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Dodawanie właściwości śledzenia do jednostki działu

W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) określa, że ta kolumna jest uwzględniona w @no__t klauzuli `Where` i `Delete` poleceń. Ten atrybut jest wywoływany `Timestamp`, ponieważ poprzednie wersje SQL Server używały typu danych SQL `timestamp` przed zastąpieniem typu SQL `rowversion`.

Interfejs API Fluent może również określać Właściwość śledzenia:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Poniższy kod przedstawia część języka T-SQL wygenerowanego przez EF Core w przypadku zaktualizowania nazwy działu:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

Poprzedni wyróżniony kod pokazuje klauzulę `WHERE` zawierającą `RowVersion`. Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.

Następujący wyróżniony kod pokazuje T-SQL, który sprawdza dokładnie jeden wiersz został zaktualizowany:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

[@ @ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy, na które miało wpływ Ostatnia instrukcja. W żadnym wierszu nie są aktualizowane EF Core zgłasza `DbUpdateConcurrencyException`.

W oknie danych wyjściowych programu Visual Studio można wyświetlić EF Core T-SQL.

### <a name="update-the-db"></a>Aktualizowanie bazy danych

Dodanie właściwości `RowVersion` zmienia model bazy danych, który wymaga migracji.

Skompiluj projekt. Wprowadź następujące polecenie w oknie polecenia:

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

Poprzednie polecenia:

* Dodaje plik migracji *_RowVersion. cs migracji/{Time Datownik}* .
* Aktualizuje plik *migrations/SchoolContextModelSnapshot. cs* . Aktualizacja dodaje następujący wyróżniony kod do metody `BuildModel`:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* Uruchamia migracje, aby zaktualizować bazę danych.

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>Tworzenie szkieletu modelu działów

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w obszarze [szkieletu model studenta](xref:data/ef-rp/intro#scaffold-student-pages) i użyj `Department` dla klasy modelu.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

Poprzednie polecenie szkieletuje model `Department`. Otwórz projekt w programie Visual Studio.

Skompiluj projekt.

### <a name="update-the-departments-index-page"></a>Aktualizowanie strony indeksu działów

Aparat tworzenia szkieletów utworzył kolumnę `RowVersion` dla strony indeks, ale to pole nie powinno być wyświetlane. W tym samouczku zostanie wyświetlony ostatni bajt `RowVersion`, aby ułatwić zrozumienie współbieżności. Ostatni bajt nie gwarantuje unikalności. Rzeczywista aplikacja nie zostanie wyświetlona `RowVersion` lub ostatniego bajtu `RowVersion`.

Aktualizowanie strony indeksu:

* Zamień indeks na działy.
* Zastąp znaczniki zawierające `RowVersion` ostatnim bajtem `RowVersion`.
* Zamień FirstMidName na FullName.

Następujące znaczniki pokazują zaktualizowaną stronę:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aktualizowanie modelu strony edycji

Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) zostaje zaktualizowany przy użyciu wartości `rowVersion` z jednostki, która została pobrana. EF Core generuje polecenie SQL UPDATE z klauzulą WHERE zawierającą oryginalną wartość `RowVersion`. Jeśli nie ma żadnych wierszy, których dotyczy polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), zostanie zgłoszony wyjątek `DbUpdateConcurrencyException`.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

W poprzednim kodzie `Department.RowVersion` jest wartością, gdy jednostka została pobrana. `OriginalValue` jest wartością w bazie danych, gdy w tej metodzie została wywołana `FirstOrDefaultAsync`.

Poniższy kod pobiera wartości klienta (wartości ogłoszone w tej metodzie) i wartości bazy danych:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Następujący wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych. Następnym razem, gdy użytkownik kliknie przycisk **Zapisz**, zostanie przechwycony tylko błąd współbieżności występujący od momentu ostatniego wyświetlenia strony edycji.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`. Na stronie Razor wartość `ModelState` dla pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.

## <a name="update-the-edit-page"></a>Aktualizowanie strony edytowania

Aktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego znacznika:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Poprzedzające znaczniki:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje ukrytą wersję wiersza. należy dodać `RowVersion`, aby po zwrotze ponownie powiązać wartość.
* Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.
* Zastępuje `ViewData` z silnie wpisaną `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Konflikty współbieżności testów ze stroną edycji

Otwórz dwa wystąpienia przeglądarki edycji dla działu angielskiego:

* Uruchom aplikację i wybierz pozycję działy.
* Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**.
* Na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu w języku angielskim.

Dwie karty przeglądarki wyświetlają te same informacje.

Zmień nazwę na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edycja działu Strona 1 po zmianie](concurrency/_static/edit-after-change-1.png)

W przeglądarce zostanie wyświetlona strona indeks z wartością zmieniona i zaktualizowanym wskaźnikiem rowVersion. Zanotuj zaktualizowany wskaźnik rowVersion, który jest wyświetlany na drugim serwerze ogłaszania zwrotnego na drugiej karcie.

Zmień inne pole w drugiej karcie przeglądarki.

![Edycja działu Strona 2 po zmianie](concurrency/_static/edit-after-change-2.png)

Kliknij przycisk **Save** (Zapisz). Komunikaty o błędach są wyświetlane dla wszystkich pól, które nie pasują do wartości bazy danych:

![Komunikat o błędzie strony edytowania działu](concurrency/_static/edit-error.png)

To okno przeglądarki nie zamiało zmiany pola Nazwa. Skopiuj i wklej bieżącą wartość (języki) do pola Nazwa. Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.

![Komunikat o błędzie strony edytowania działu](concurrency/_static/cv.png)

Kliknij przycisk **Zapisz** ponownie. Wartość wprowadzona na drugiej karcie przeglądarki jest zapisywana. Zapisane wartości są widoczne na stronie indeks.

## <a name="update-the-delete-page"></a>Aktualizowanie strony usuwania

Zaktualizuj model usuwania stron przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Strona usuwania wykrywa konflikty współbieżności, gdy jednostka została zmieniona po pobraniu. `Department.RowVersion` to wersja wiersza, kiedy jednostka została pobrana. Gdy EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`. Jeśli polecenie SQL DELETE skutkuje niezerowymi wierszami:

* @No__t-0 w poleceniu SQL DELETE nie jest zgodny z `RowVersion` w bazie danych.
* Zgłaszany jest wyjątek DbUpdateConcurrencyException.
* `OnGetAsync` jest wywoływana z `concurrencyError`.

### <a name="update-the-delete-page"></a>Aktualizowanie strony usuwania

Zaktualizuj *strony/działy/Delete. cshtml* przy użyciu następującego kodu:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Poprzedni kod wprowadza następujące zmiany:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje komunikat o błędzie.
* Zastępuje FirstMidName z FullName w polu **administrator** .
* Zmiany `RowVersion` w celu wyświetlenia ostatniego bajtu.
* Dodaje ukrytą wersję wiersza. należy dodać `RowVersion`, aby po zwrotze ponownie powiązać wartość.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Konflikty współbieżności testów ze stroną usuwania

Tworzenie działu testowego.

Otwórz dwa wystąpienia przeglądarki usuwania dla działu testowego:

* Uruchom aplikację i wybierz pozycję działy.
* Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu testowego i wybierz polecenie **Otwórz na nowej karcie**.
* Kliknij hiperłącze **Edytuj** dla działu testowego.

Dwie karty przeglądarki wyświetlają te same informacje.

Zmień budżet na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

W przeglądarce zostanie wyświetlona strona indeks z wartością zmieniona i zaktualizowanym wskaźnikiem rowVersion. Zanotuj zaktualizowany wskaźnik rowVersion, który jest wyświetlany na drugim serwerze ogłaszania zwrotnego na drugiej karcie.

Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych. Kliknięcie przycisku **Usuń** powoduje usunięcie jednostki, chyba że `RowVersion` została zaktualizowana. dział został usunięty.

Zobacz [dziedziczenie](xref:data/ef-mvc/inheritance) sposobu dziedziczenia modelu danych.

### <a name="additional-resources"></a>Zasoby dodatkowe

* [Tokeny współbieżności w EF Core](/ef/core/modeling/concurrency)
* [Obsługa współbieżności w EF Core](/ef/core/saving/concurrency)
* [Wersja tego samouczka usługi YouTube (obsługa konfliktów współbieżności)](https://youtu.be/EosxHTFgYps)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [Wersja usługi YouTube w tym samouczku (część 3)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [Wstecz](xref:data/ef-rp/update-related-data)

::: moniker-end

