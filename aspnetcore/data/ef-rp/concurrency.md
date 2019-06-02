---
title: Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: 8430f8e720870a7b541655ea8bcfe2f67c942bb3
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458430"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), i [Jan Kowalski P](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie). Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrukcje pobierania](xref:index#how-to-download-a-sample).

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

Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

Magdalena kliknie **Zapisz** pierwszy i widzi jej zmienić, gdy w przeglądarce pojawi się strona indeksu.

![Budżet na zero](concurrency/_static/budget-zero.png)

John kliknie **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $. Co dzieje się potem określają sposób obsługi konfliktów współbieżności.

Optymistyczna współbieżność obejmuje następujące opcje:

* Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny bazy danych.

  W tym scenariuszu zostałyby utracone żadne dane. Inne właściwości zostały zaktualizowane przez dwóch użytkowników. Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych. To podejście:
 
  * Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.
  * Zwykle nie jest praktyczne w aplikacji sieci web. Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.
  * Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.

* Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.

  Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00. To podejście jest nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza. (Wszystkie wartości z klienta pierwszeństwo co znajduje się w magazynie danych.) Jeśli nie możesz tworzyć jakiegokolwiek kodu do obsługi współbieżności, Wins klienta odbywa się automatycznie.

* Aby uniemożliwić zmiany John's aktualizowane w bazie danych. Zazwyczaj aplikacja będzie:

  * Wyświetl komunikat o błędzie.
  * Umożliwia wyświetlenie bieżącego stanu danych.
  * Zezwalaj użytkownikowi ponownie zastosować zmiany.

  Jest to nazywane *Store Wins* scenariusza. (Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) W tym samouczku jest zaimplementowania scenariusza Store Wins. Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.

## <a name="handling-concurrency"></a>Obsługa współbieżności 

Gdy właściwość jest skonfigurowany jako [tokenu współbieżności](/ef/core/modeling/concurrency):

* EF Core sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania. Sprawdzanie jest wykonywane podczas [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.
* Jeśli właściwość została zmieniona po jego pobrania, [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) zgłaszany. 

Model danych i bazy danych musi być skonfigurowany do obsługi zgłaszanie `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Wykrywanie konfliktów współbieżności we właściwości

Konfliktów współbieżności może zostać wykryte przy użyciu na poziomie właściwość [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atrybutu. Ten atrybut można zastosować na wiele właściwości w modelu. Aby uzyskać więcej informacji, zobacz [danych adnotacje-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` Atrybut nie jest używany w ramach tego samouczka.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Wykrywanie konfliktów współbieżności wiersz

Do wykrywania konfliktów współbieżności [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) kolumny śledzenia jest dodawany do modelu.  `rowversion` :

* Dotyczy programu SQL Server. Inne bazy danych nie mogą zawierać podobnych funkcji.
* Służy do określenia, czy jednostka nie została zmieniona, ponieważ została pobrana z bazy danych. 

Baza danych generuje kolejna `rowversion` numer jest zwiększany po każdej wiersz jest aktualizowany. W `Update` lub `Delete` polecenia `Where` klauzula zawiera wartość pobrano `rowversion`. Jeśli zmieniono aktualizacji wiersza:

* `rowversion` nie pasuje do wartości pobrano.
* `Update` Lub `Delete` poleceń nie znajdziesz wiersza, ponieważ `Where` klauzula zawiera pobrano `rowversion`.
* A `DbUpdateConcurrencyException` zgłaszany.

W programie EF Core, gdy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia, jest zgłaszany wyjątek współbieżności.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Dodawanie właściwości śledzenia do jednostki działu

W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Sygnatura czasowa](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atrybut określa, czy ta kolumna znajduje się w `Where` klauzuli `Update` i `Delete` poleceń. Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` typ zastąpiono ją.

Interfejs fluent API można również określić właściwości śledzenia:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Poprzednie wyróżnione przedstawia kod `WHERE` zawierających klauzulę `RowVersion`. Jeśli bazy danych `RowVersion` nie równa się `RowVersion` parametru (`@p2`), wiersze nie zostały zaktualizowane.

Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy na ostatniej instrukcji. W żadnym wiersze są aktualizowane, zgłasza programu EF Core `DbUpdateConcurrencyException`.

Widać, że T-SQL programu EF Core generuje w oknie danych wyjściowych programu Visual Studio.

### <a name="update-the-db"></a>Aktualizacja bazy danych

Dodawanie `RowVersion` zmienia właściwość modelu bazy danych, który wymaga migracji.

Skompiluj projekt. W oknie polecenia, należy wprowadzić następujące czynności:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Poprzedniego polecenia:

* Dodaje *migracje / {stamp}_RowVersion.cs czasu* pliku migracji.
* Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku. Aktualizacja dodaje następujący wyróżniony kod do `BuildModel` metody:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Uruchamia migracji można zaktualizować bazy danych.

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>Tworzenie szkieletu modelu działów

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Department` dla klasy modelu.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

Poprzedni szkielety mechanizmów polecenia `Department` modelu. Otwórz projekt w programie Visual Studio.

Skompiluj projekt.

### <a name="update-the-departments-index-page"></a>Zaktualizuj strony działy indeksu

Aparat tworzenia szkieletów utworzone `RowVersion` kolumny do strony indeksu, ale tego pola nie powinna wyświetlane. W tym samouczku, ostatni bajt `RowVersion` jest wyświetlana, aby lepiej zrozumieć współbieżności. Ostatni bajt nie musi być unikatowy. Rzeczywistej aplikacji nie wyświetlał `RowVersion` lub ostatni bajt `RowVersion`.

Zaktualizuj strony indeksu:

* Zastąp indeksu działów.
* Zastąp kod znaczników zawierający `RowVersion` z ostatniego bajtu `RowVersion`.
* Zastąp FirstMidName imię i nazwisko.

Następujący kod przedstawia zaktualizowaną stronę:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aktualizowanie modelu strony edycji

Aktualizacja *Pages\Departments\Edit.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) będą aktualizowane przy użyciu `rowVersion` wartości z obiektu została ona pobrana. EF Core generuje polecenia aktualizacji programu SQL z klauzulą WHERE zawiera oryginał `RowVersion` wartość. Jeśli żadne wiersze nie dotyczy polecenia UPDATE (nie wiersze mają oryginalny `RowVersion` wartość), `DbUpdateConcurrencyException` jest zgłaszany wyjątek.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

W poprzednim kodzie `Department.RowVersion` jest wartością, jeśli jednostka została pobrana. `OriginalValue` jest to wartość w bazie danych podczas `FirstOrDefaultAsync` została wywołana w przypadku tej metody.

Poniższy kod umożliwia pobranie ustawienia klienta (wartości do tej metody) oraz bazy danych wartości:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma bazy danych wartości inne niż co opublikowano `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Następujące wyróżniony kod ustawia `RowVersion` wartość do nowej wartości są pobierane z bazy danych. Przy następnym kliknięciu **Zapisz**, tylko błędy współbieżności, które odbywa się od czasu ostatniego wyświetlania strony edytowania zostanie przechwycony.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość. Strony Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

Aktualizacja *Pages/Departments/Edit.cshtml* następującym kodem:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Poprzedni kod znaczników:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.
* Dodaje wersji ukrytego wiersza. `RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.
* Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.
* Zastępuje `ViewData` z silnie typizowanych `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Badanie konfliktów współbieżności przy użyciu strony edytowania

Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**.
* Na pierwszej karcie kliknij **Edytuj** hiperłącze dla angielskiego działu.

Na kartach przeglądarki dwa są wyświetlane te same informacje.

Zmień nazwę w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika. Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.

Zmień inne pole w drugiej karcie przeglądarki.

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

Kliknij pozycję **Zapisz**. Zostaną wyświetlone komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

To okno przeglądarki przypadkowo zmienić pole Nazwa. Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa. Karta. Weryfikacja po stronie klienta usuwa komunikat o błędzie.

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

Kliknij przycisk **Zapisz** ponownie. Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany. Zobaczysz zapisane wartości na stronie indeksu.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Aktualizowanie modelu strony Usuń z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania. `Department.RowVersion` jest to wersja wiersza, jeśli jednostka została pobrana. EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`. Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:

* `RowVersion` W SQL DELETE polecenia nie jest zgodna `RowVersion` w bazie danych.
* DbUpdateConcurrencyException wyjątku.
* `OnGetAsync` jest wywoływana z `concurrencyError`.

### <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Poprzedni kod znaczników wprowadza następujące zmiany:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.
* Dodaje komunikat o błędzie.
* Zamienia FirstMidName imię i nazwisko w **administratora** pola.
* Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.
* Dodaje wersji ukrytego wiersza. `RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Badanie konfliktów współbieżności ze stroną Delete

Utwórz działu testu.

Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu test i wybierz pozycję **Otwórz na nowej karcie**.
* Kliknij przycisk **Edytuj** hiperłącze dla działu testu.

Na kartach przeglądarki dwa są wyświetlane te same informacje.

Zmień budżetu w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.

Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika. Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.

Usuń z działu testu z drugiej karcie. Błąd współbieżności jest wyświetlana przy użyciu bieżących wartości z bazy danych. Klikając **Usuń** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.

Zobacz [dziedziczenia](xref:data/ef-mvc/inheritance) na temat sposobu dziedziczą modelu danych.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Tokeny współbieżności w programie EF Core](/ef/core/modeling/concurrency)
* [Obsługa współbieżności w programie EF Core](/ef/core/saving/concurrency)
* [Wersja usługi YouTube w tym samouczku (Obsługa konfliktów współbieżności)](https://youtu.be/EosxHTFgYps)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [Wersja usługi YouTube w tym samouczku (część 3)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/update-related-data)
