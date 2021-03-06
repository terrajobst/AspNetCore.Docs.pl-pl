---
title: Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: c4d43f26ba80e7922c3cbd37d9a5f8e1561b11ad
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656913"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)i [Jan P Kowalski](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Występuje konflikt współbieżności, gdy:

* Użytkownik przejdzie do strony edytowania dla jednostki.
* Inny użytkownik aktualizuje tę samą jednostkę przed zapisaniem zmiany pierwszego użytkownika w bazie danych.

Jeśli wykrywanie współbieżności nie jest włączone, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników. Jeśli to ryzyko jest akceptowalne, koszt programowania dla współbieżności może być korzystny.

### <a name="pessimistic-concurrency-locking"></a>Współbieżność pesymistyczna (blokowanie)

Jednym ze sposobów zapobiegania konfliktom współbieżności jest użycie blokad bazy danych. Jest to nazywane pesymistyczną współbieżnością. Zanim aplikacja odczyta wiersz bazy danych, który zamierza zaktualizować, żąda blokady. Gdy wiersz jest zablokowany na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie może zablokować wiersza do momentu zwolnienia pierwszej blokady.

Zarządzanie blokadami ma wady. Może być skomplikowany dla programu i może spowodować problemy z wydajnością w miarę zwiększania się liczby użytkowników. Entity Framework Core nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak wdrożyć go.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj. Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.

![Zmiana budżetu na 0](concurrency/_static/change-budget30.png)

Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date30.png)

Janina klika pozycję **Zapisz** pierwszy i widzimy, że zmiany zaczęły obowiązywać, ponieważ przeglądarka wyświetla stronę indeksu z zerem jako kwotą budżetu.

Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00. Co się stanie dalej, zależy od sposobu obsługi konfliktów współbieżności:

* Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych.

  W tym scenariuszu zostałyby utracone żadne dane. Inne właściwości zostały zaktualizowane przez dwóch użytkowników. Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych. Takie podejście ma pewne wady:
 
  * Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.
  * Zwykle nie jest praktyczne w aplikacji sieci web. Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.
  * Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.

* Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.

  Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00. Ta metoda jest nazywana *klientem WINS* lub *ostatnim scenariuszem usługi WINS* . (Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.

* Można zapobiec aktualizacji firmy Jan ze zmian w bazie danych. Zazwyczaj aplikacja będzie:

  * Wyświetl komunikat o błędzie.
  * Umożliwia wyświetlenie bieżącego stanu danych.
  * Zezwalaj użytkownikowi ponownie zastosować zmiany.

  Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS. Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.

## <a name="conflict-detection-in-ef-core"></a>Wykrywanie konfliktów w EF Core

EF Core generuje wyjątki `DbConcurrencyException` w przypadku wykrycia konfliktów. Model danych musi być skonfigurowany, aby umożliwić wykrywanie konfliktów. Opcje włączania wykrywania konfliktów obejmują następujące elementy:

* Skonfiguruj EF Core, aby uwzględnić oryginalne wartości kolumn skonfigurowanych jako [tokeny współbieżności](/ef/core/modeling/concurrency) w klauzuli WHERE poleceń Update i DELETE.

  Gdy `SaveChanges` jest wywoływana, klauzula WHERE szuka oryginalnych wartości wszelkich właściwości, które mają adnotację z atrybutem [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) . Instrukcja Update nie znajdzie wiersza do zaktualizowania, jeśli którykolwiek z właściwości tokenu współbieżności został zmieniony od momentu pierwszego odczytu wiersza. EF Core interpretuje ten sposób jako konflikt współbieżności. W przypadku tabel bazy danych z wieloma kolumnami takie podejście może skutkować bardzo dużymi klauzulami WHERE i może wymagać dużej ilości danych. W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.

* W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony.

  W bazie danych SQL Server typ danych kolumny śledzenia jest `rowversion`. Wartość `rowversion` jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W przypadku polecenia Update lub DELETE klauzula WHERE zawiera oryginalną wartość kolumny śledzenia (numer wersji pierwszego wiersza). Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości. W takim przypadku instrukcja UPDATE lub DELETE nie może znaleźć wiersza do zaktualizowania z powodu klauzuli WHERE. EF Core zgłasza wyjątek współbieżności, gdy polecenie Update lub DELETE nie ma na nich żadnych wierszy.

## <a name="add-a-tracking-property"></a>Dodaj właściwość śledzenia

W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) wskazuje, co identyfikuje kolumnę jako kolumnę śledzenia współbieżności. Interfejs API Fluent jest alternatywnym sposobem określenia właściwości śledzenia:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

W przypadku bazy danych SQL Server atrybut `[Timestamp]` właściwości Entity zdefiniowany jako tablica bajtowa:

* Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.
* Ustawia typ kolumny w bazie danych na [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).

Baza danych generuje sekwencyjny numer wersji wiersza, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete`, klauzula `Where` zawiera wartość wersji wiersza do pobrania. Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:

* Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.
* Polecenia `Update` lub `Delete` nie znalazły wiersza, ponieważ klauzula `Where` szuka wartości wersji wiersza pobrania.
* Zostanie zgłoszony `DbUpdateConcurrencyException`.

Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

Poprzedni wyróżniony kod pokazuje klauzulę `WHERE` zawierającą `RowVersion`. Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.

Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy, na które miało wpływ Ostatnia instrukcja. Jeśli żadne wiersze nie są aktualizowane, EF Core zgłasza `DbUpdateConcurrencyException`.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W przypadku bazy danych programu SQLite atrybut `[Timestamp]` właściwości Entity został zdefiniowany jako tablica bajtowa:

* Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.
* Mapuje do typu kolumny obiektu BLOB.

Wyzwalacze bazy danych aktualizują kolumnę RowVersion za pomocą nowej losowej tablicy bajtów za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete`, klauzula `Where` zawiera pobraną wartość kolumny RowVersion. Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:

* Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.
* Polecenie `Update` lub `Delete` nie znajduje wiersza, ponieważ klauzula `Where` szuka pierwotnej wartości wersji wiersza.
* Zostanie zgłoszony `DbUpdateConcurrencyException`.

---

### <a name="update-the-database"></a>Aktualizowanie bazy danych

Dodanie właściwości `RowVersion` powoduje zmianę modelu danych, który wymaga migracji.

Skompiluj projekt. 

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uruchom następujące polecenie w obszarze PMC:

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Uruchom następujące polecenie w terminalu:

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

To polecenie:

* Tworzy plik migracji *_RowVersion. cs migracji/sygnatur czasowych}* .
* Aktualizuje plik *migrations/SchoolContextModelSnapshot. cs* . Aktualizacja dodaje następujący wyróżniony kod do metody `BuildModel`:

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uruchom następujące polecenie w obszarze PMC:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz plik `Migrations/<timestamp>_RowVersion.cs` i Dodaj wyróżniony kod:

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  Powyższy kod:

  * Aktualizuje istniejące wiersze z losowymi wartościami obiektów BLOB.
  * Dodaje wyzwalacze bazy danych, które ustawiają kolumnę RowVersion na losową wartość obiektu BLOB za każdym razem, gdy wiersz zostanie zaktualizowany.

* Uruchom następujące polecenie w terminalu:

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a>Strony działu szkieletu

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Postępuj zgodnie z instrukcjami na [stronach uczniów tworzenia szkieletów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:

* Utwórz folder *strony/działy* .  
* Użyj `Department` dla klasy modelu.
  * Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

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

* Zastąp indeksu działów.
* Zmień kod zawierający `RowVersion`, aby pokazać tylko ostatni bajt tablicy bajtów.
* Zastąp FirstMidName imię i nazwisko.

Poniższy kod przedstawia zaktualizowaną stronę:

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a>Aktualizowanie modelu strony edycji

Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) jest aktualizowana przy użyciu wartości `rowVersion` z jednostki, gdy została ona pobrana w metodzie `OnGet`. EF Core generuje polecenie SQL UPDATE z klauzulą WHERE zawierającą oryginalną wartość `RowVersion`. Jeśli nie ma żadnych wierszy, na które ma wpływ polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), zostanie zgłoszony wyjątek `DbUpdateConcurrencyException`.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

W poprzednim wyróżnionym kodzie:

* Wartość w `Department.RowVersion` to jaka znajdowała się w jednostce, gdy została pierwotnie pobrana w żądaniu pobrania dla strony edycji. Wartość jest dostarczana do metody `OnPost` przez ukryte pole na stronie Razor, które wyświetla jednostkę do edycji. Wartość pola ukrytego jest kopiowana do `Department.RowVersion` przez spinacz modelu.
* `OriginalValue` jest tym, czego EF Core będzie używać w klauzuli WHERE. Przed wykonaniem wyróżnionego wiersza kodu `OriginalValue` ma wartość znajdującą się w bazie danych, gdy `FirstOrDefaultAsync` została wywołana w tej metodzie, która może się różnić od tego, co zostało wyświetlone na stronie Edycja.
* Wyróżniony kod sprawdza, czy EF Core używa oryginalnej wartości `RowVersion` z wyświetlonej jednostki `Department` w klauzuli WHERE instrukcji SQL UPDATE.

Gdy wystąpi błąd współbieżności, poniższy wyróżniony kod pobiera wartości klienta (wartości ogłoszone w tej metodzie) oraz wartości bazy danych.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

Poniższy wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych. Następnym razem, gdy użytkownik kliknie przycisk **Zapisz**, zostanie przechwycony tylko błąd współbieżności występujący od momentu ostatniego wyświetlenia strony edycji.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`. Na stronie Razor wartość `ModelState` pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.

### <a name="update-the-razor-page"></a>Aktualizowanie strony Razor

Zaktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego kodu:

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Powyższy kod:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje wersji ukrytego wiersza. należy dodać `RowVersion` tak, aby zwroty z powrotem powiązać wartość.
* Wyświetla ostatni bajt `RowVersion` do celów debugowania.
* Zastępuje `ViewData` ze silnie wpisaną `InstructorNameSL`.

### <a name="test-concurrency-conflicts-with-the-edit-page"></a>Badanie konfliktów współbieżności przy użyciu strony edytowania

Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**.
* Na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu w języku angielskim.

Na kartach przeglądarki dwa są wyświetlane te same informacje.

Zmień nazwę na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-130.png)

Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika. Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.

Zmień inne pole w drugiej karcie przeglądarki.

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-230.png)

Kliknij przycisk **Save** (Zapisz). Komunikaty o błędach są wyświetlane dla wszystkich pól, które nie pasują do wartości bazy danych:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error30.png)

To okno przeglądarki przypadkowo zmienić pole Nazwa. Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa. Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.

Kliknij przycisk **Zapisz** ponownie. Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany. Zobaczysz zapisane wartości na stronie indeksu.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Zaktualizuj *strony/działy/Delete. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania. `Department.RowVersion` to wersja wiersza, kiedy jednostka została pobrana. Gdy EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`. Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:

* `RowVersion` w poleceniu SQL DELETE nie jest zgodny `RowVersion` w bazie danych.
* DbUpdateConcurrencyException wyjątku.
* `OnGetAsync` jest wywoływana z `concurrencyError`.

### <a name="update-the-delete-razor-page"></a>Aktualizowanie strony usuwania Razor

Zaktualizuj *strony/działy/Delete. cshtml* przy użyciu następującego kodu:

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Poprzedni kod wprowadza następujące zmiany:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje komunikat o błędzie.
* Zastępuje FirstMidName z FullName w polu **administrator** .
* Zmienia `RowVersion` w celu wyświetlenia ostatniego bajtu.
* Dodaje wersji ukrytego wiersza. należy dodać `RowVersion`, aby postgit dodać ponownie powiązania wartości.

### <a name="test-concurrency-conflicts"></a>Testuj konflikty współbieżności

Utwórz działu testu.

Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu testowego i wybierz polecenie **Otwórz na nowej karcie**.
* Kliknij hiperłącze **Edytuj** dla działu testowego.

Na kartach przeglądarki dwa są wyświetlane te same informacje.

Zmień budżet na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika. Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.

Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych. Kliknięcie przycisku **Usuń** powoduje usunięcie jednostki, o ile `RowVersion` nie została zaktualizowana. dział został usunięty.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Tokeny współbieżności w EF Core](/ef/core/modeling/concurrency)
* [Obsługa współbieżności w EF Core](/ef/core/saving/concurrency)
* [Debugowanie ASP.NET Core 2. x](https://github.com/dotnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a>Następne kroki

Jest to ostatni samouczek z serii. Dodatkowe tematy zostały omówione w [wersji MVC tej serii samouczków](xref:data/ef-mvc/index).

> [!div class="step-by-step"]
> [Poprzedni samouczek](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie). Jeśli występują problemy, których nie można rozwiązać, [Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

## <a name="concurrency-conflicts"></a>Konfliktów współbieżności

Występuje konflikt współbieżności, gdy:

* Użytkownik przejdzie do strony edytowania dla jednostki.
* Inny użytkownik aktualizuje tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.

Jeśli wykrycie współbieżności nie jest włączone, gdy wystąpią równoczesne aktualizacje:

* Ostatnia aktualizacja usługi wins. Oznacza to, że ostatnie wartości aktualizacji są zapisywane do bazy danych.
* Pierwszego dnia bieżące aktualizacje zostaną utracone.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj. Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

Jan klika pozycję **Zapisz** jako pierwszy i widzi zmiany, gdy przeglądarka wyświetli stronę indeksu.

![Budżet na zero](concurrency/_static/budget-zero.png)

Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00. Co dzieje się potem określają sposób obsługi konfliktów współbieżności.

Optymistyczna współbieżność obejmuje następujące opcje:

* Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny bazy danych.

  W tym scenariuszu zostałyby utracone żadne dane. Inne właściwości zostały zaktualizowane przez dwóch użytkowników. Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych. To podejście:
 
  * Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.
  * Zwykle nie jest praktyczne w aplikacji sieci web. Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.
  * Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.

* Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.

  Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00. Ta metoda jest nazywana *klientem WINS* lub *ostatnim scenariuszem usługi WINS* . (Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.

* Aby uniemożliwić zmiany John's aktualizowane w bazie danych. Zazwyczaj aplikacja będzie:

  * Wyświetl komunikat o błędzie.
  * Umożliwia wyświetlenie bieżącego stanu danych.
  * Zezwalaj użytkownikowi ponownie zastosować zmiany.

  Jest to tzw. scenariusz *magazynu usługi WINS* . (Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS. Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.

## <a name="handling-concurrency"></a>Obsługa współbieżności 

Gdy właściwość jest skonfigurowana jako [Token współbieżności](/ef/core/modeling/concurrency):

* EF Core sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania. Sprawdzanie występuje, gdy wywoływana jest [metody SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) .
* Jeśli właściwość została zmieniona po pobraniu, zgłaszany jest [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) . 

Baza danych i model danych muszą być skonfigurowane do obsługi zgłaszania `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Wykrywanie konfliktów współbieżności we właściwości

Konflikty współbieżności mogą być wykrywane na poziomie właściwości przy użyciu atrybutu [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) . Ten atrybut można zastosować na wiele właściwości w modelu. Aby uzyskać więcej informacji, zobacz [Adnotacje danych — ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).

Atrybut `[ConcurrencyCheck]` nie jest używany w tym samouczku.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Wykrywanie konfliktów współbieżności wiersz

Aby wykrywać konflikty współbieżności, do modelu dodawana jest kolumna śledzenia [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) .  `rowversion`:

* Dotyczy programu SQL Server. Inne bazy danych nie mogą zawierać podobnych funkcji.
* Służy do określenia, czy jednostka nie została zmieniona, ponieważ została pobrana z bazy danych. 

Baza danych generuje numer sekwencyjny `rowversion`, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany. W `Update` lub `Delete`, klauzula `Where` zawiera pobraną wartość `rowversion`. Jeśli zmieniono aktualizacji wiersza:

* `rowversion` nie jest zgodna z pobraną wartością.
* Polecenia `Update` lub `Delete` nie znalazły wiersza, ponieważ klauzula `Where` zawiera pobrane `rowversion`.
* Zostanie zgłoszony `DbUpdateConcurrencyException`.

W EF Core, gdy żadne wiersze nie zostały zaktualizowane przez polecenie `Update` lub `Delete`, zostanie zgłoszony wyjątek współbieżności.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Dodawanie właściwości śledzenia do jednostki działu

W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) określa, że ta kolumna jest uwzględniona w klauzuli `Where` poleceń `Update` i `Delete`. Ten atrybut jest nazywany `Timestamp`, ponieważ poprzednie wersje SQL Server używały typu danych SQL `timestamp` przed zastąpieniem typu `rowversion` SQL.

Interfejs fluent API można również określić właściwości śledzenia:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

Poprzedni wyróżniony kod pokazuje klauzulę `WHERE` zawierającą `RowVersion`. Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.

Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy, na które miało wpływ Ostatnia instrukcja. Nie zaktualizowano żadnych wierszy, EF Core zgłasza `DbUpdateConcurrencyException`.

Widać, że T-SQL programu EF Core generuje w oknie danych wyjściowych programu Visual Studio.

### <a name="update-the-db"></a>Aktualizacja bazy danych

Dodanie właściwości `RowVersion` powoduje zmianę modelu bazy danych, który wymaga migracji.

Skompiluj projekt. W oknie polecenia, należy wprowadzić następujące czynności:

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

Poprzedniego polecenia:

* Dodaje *migrację/{Time sygnaturę} _RowVersion. cs* pliku migracji.
* Aktualizuje plik *migrations/SchoolContextModelSnapshot. cs* . Aktualizacja dodaje następujący wyróżniony kod do metody `BuildModel`:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* Uruchamia migracji można zaktualizować bazy danych.

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>Tworzenie szkieletu modelu działów

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w obszarze [szkieletem model studenta](xref:data/ef-rp/intro#scaffold-student-pages) i użyj `Department` dla klasy model.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

Poprzednie polecenie szkieletuje model `Department`. Otwórz projekt w programie Visual Studio.

Skompiluj projekt.

### <a name="update-the-departments-index-page"></a>Zaktualizuj strony działy indeksu

Aparat tworzenia szkieletów utworzył kolumnę `RowVersion` dla strony index, ale to pole nie powinno być wyświetlane. W tym samouczku zostanie wyświetlony ostatni bajt `RowVersion`, aby ułatwić zrozumienie współbieżności. Ostatni bajt nie musi być unikatowy. Rzeczywista aplikacja nie zostanie wyświetlona `RowVersion` lub ostatniego bajtu `RowVersion`.

Zaktualizuj strony indeksu:

* Zastąp indeksu działów.
* Zastąp znaczniki zawierające `RowVersion` z ostatnim bajtem `RowVersion`.
* Zastąp FirstMidName imię i nazwisko.

Następujący kod przedstawia zaktualizowaną stronę:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aktualizowanie modelu strony edycji

Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) zostaje zaktualizowany przy użyciu wartości `rowVersion` z jednostki, która została pobrana. EF Core generuje polecenie SQL UPDATE z klauzulą WHERE zawierającą oryginalną wartość `RowVersion`. Jeśli nie ma żadnych wierszy, na które ma wpływ polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), zostanie zgłoszony wyjątek `DbUpdateConcurrencyException`.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

W poprzednim kodzie `Department.RowVersion` jest wartością, gdy jednostka została pobrana. `OriginalValue` jest wartością w bazie danych, gdy `FirstOrDefaultAsync` została wywołana w tej metodzie.

Poniższy kod umożliwia pobranie ustawienia klienta (wartości do tej metody) oraz bazy danych wartości:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Poniższy wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych. Następnym razem, gdy użytkownik kliknie przycisk **Zapisz**, zostanie przechwycony tylko błąd współbieżności występujący od momentu ostatniego wyświetlenia strony edycji.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`. Na stronie Razor wartość `ModelState` pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

Aktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego znacznika:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Poprzedni kod znaczników:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje wersji ukrytego wiersza. należy dodać `RowVersion` tak, aby zwroty z powrotem powiązać wartość.
* Wyświetla ostatni bajt `RowVersion` do celów debugowania.
* Zastępuje `ViewData` ze silnie wpisaną `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Badanie konfliktów współbieżności przy użyciu strony edytowania

Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**.
* Na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu w języku angielskim.

Na kartach przeglądarki dwa są wyświetlane te same informacje.

Zmień nazwę na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika. Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.

Zmień inne pole w drugiej karcie przeglądarki.

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

Kliknij przycisk **Save** (Zapisz). Zostaną wyświetlone komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

To okno przeglądarki przypadkowo zmienić pole Nazwa. Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa. Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

Kliknij przycisk **Zapisz** ponownie. Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany. Zobaczysz zapisane wartości na stronie indeksu.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Aktualizowanie modelu strony Usuń z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania. `Department.RowVersion` to wersja wiersza, kiedy jednostka została pobrana. Gdy EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`. Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:

* `RowVersion` w poleceniu SQL DELETE nie jest zgodny `RowVersion` w bazie danych.
* DbUpdateConcurrencyException wyjątku.
* `OnGetAsync` jest wywoływana z `concurrencyError`.

### <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Zaktualizuj *strony/działy/Delete. cshtml* przy użyciu następującego kodu:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Poprzedni kod wprowadza następujące zmiany:

* Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.
* Dodaje komunikat o błędzie.
* Zastępuje FirstMidName z FullName w polu **administrator** .
* Zmienia `RowVersion` w celu wyświetlenia ostatniego bajtu.
* Dodaje wersji ukrytego wiersza. należy dodać `RowVersion` tak, aby zwroty z powrotem powiązać wartość.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Badanie konfliktów współbieżności ze stroną Delete

Utwórz działu testu.

Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu testowego i wybierz polecenie **Otwórz na nowej karcie**.
* Kliknij hiperłącze **Edytuj** dla działu testowego.

Na kartach przeglądarki dwa są wyświetlane te same informacje.

Zmień budżet na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.

Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika. Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.

Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych. Kliknięcie przycisku **Usuń** powoduje usunięcie jednostki, o ile `RowVersion` nie została zaktualizowana. dział został usunięty.

Zobacz [dziedziczenie](xref:data/ef-mvc/inheritance) sposobu dziedziczenia modelu danych.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Tokeny współbieżności w EF Core](/ef/core/modeling/concurrency)
* [Obsługa współbieżności w EF Core](/ef/core/saving/concurrency)
* [Wersja tego samouczka usługi YouTube (obsługa konfliktów współbieżności)](https://youtu.be/EosxHTFgYps)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [Wersja usługi YouTube w tym samouczku (część 3)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [Wstecz](xref:data/ef-rp/update-related-data)

::: moniker-end

