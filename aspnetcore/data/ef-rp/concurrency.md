---
title: "Stron razor podstawowych EF - współbieżności - 8 8"
author: rick-anderson
description: "Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
en-us /

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Obsługa konfliktom współbieżności - Core EF Razor strony (8 8)

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra), i [Jan Kowalski P](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Ten samouczek pokazuje sposób obsługi konflikty, gdy wielu użytkowników zaktualizować jednostki, jednocześnie (w tym samym czasie). Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Konfliktom współbieżności

Występuje konflikt współbieżności, gdy:

* Użytkownik przechodzi do edycji strony dla jednostki.
* Inny użytkownik aktualizacje tej samej jednostki przed zapisaniem zmian pierwszy użytkownik z bazą danych.

Jeśli wykrywania współbieżności nie jest włączone, gdy występują równoczesnych aktualizacji:

* Ostatnia aktualizacja usługi wins. Oznacza to ostatnie wartości aktualizacji są zapisywane w bazie danych.
* Pierwszy bieżące aktualizacje zostaną utracone.

### <a name="optimistic-concurrency"></a>Optymistycznej współbieżności

Optymistycznej współbieżności umożliwia konfliktom współbieżności mieć miejsce, a następnie reaguje odpowiednio po ich wykonaj. Na przykład Magdalena odwiedza edycji strony działu i zmienia budżet dla angielskiej wersji językowej działu z $350,000.00 na 0,00 USD.

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

Przed kliknie Magdalena **zapisać**, Jan odwiedza tej samej stronie i zmienia pole Data rozpoczęcia z 2007-9-1 do 9/1/2013.

![Zmiana daty rozpoczęcia 2013](concurrency/_static/change-date.png)

Magdalena kliknie **zapisać** pierwszy i widzi jej zmienić, gdy w przeglądarce pojawi się strona indeksu.

![Budżet zmienione od zera.](concurrency/_static/budget-zero.png)

Jan klika **zapisać** na stronie edycji nadal pokazujący budżetu 350,000.00 $. Co dalej zależy od sposobu obsługi konfliktom współbieżności.

Optymistycznej współbieżności zawiera następujące opcje:

* Można zachować informacje o właściwości, które zostało zmodyfikowane przez użytkownika i aktualizować tylko odpowiednie kolumny w bazie danych.

 W tym scenariuszu żadne dane nie może zostać utracone. Inne właściwości zostały zaktualizowane przez użytkowników. Przy następnym ktoś przegląda w angielskiej wersji językowej działu, zobaczą zmiany zarówno Joanny i jego Jan. Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych. Takie podejście: * nie można uniknąć utraty danych, jeśli konkurują zmian z tą samą właściwością.
        * Jest zazwyczaj nie jest praktyczne w aplikacji sieci web. Wymaga to zachowanie znaczących stanu w celu śledzenia wszystkich pobranych oraz nowych wartości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.
        * Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności na jednostkę.

* Możesz pozwolić, aby zmiany w Jan zastąpić zmiany nazwy.

 Przy następnym ktoś przegląda w angielskiej wersji językowej działu, zobaczą 9/1/2013 i pobranych wartość $350,000.00. Ta metoda jest wywoływana *klienta Wins* lub *ostatniego w usłudze Wins* scenariusza. (Wszystkie wartości z klienta wyższy priorytet niż co znajduje się w magazynie danych). Jeśli nie ma żadnych kodowania obsługi współbieżności, Wins klienta odbywa się automatycznie.

* Aby uniemożliwić zmianę jego Jan aktualizację w bazie danych. Zwykle, czy aplikacja: * wyświetlony komunikat o błędzie.
        * Wyświetlić bieżący stan danych.
        * Umożliwia użytkownikowi ponownie zastosuj zmiany.

 Ta metoda jest wywoływana *Wins magazynu* scenariusza. (Wartości magazynu danych mają priorytet nad wartości przesłany przez klienta). W tym samouczku należy wdrożyć scenariusz dla magazynu usługi Wins. Ta metoda gwarantuje, że żadne zmiany nie zostaną zastąpione bez użytkownika w tym celu.

## <a name="handling-concurrency"></a>Obsługa współbieżności 

Jeśli właściwość została skonfigurowana jako [tokenu współbieżności](https://docs.microsoft.com/ef/core/modeling/concurrency):

* Podstawowe EF sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania. Sprawdzanie jest wykonywane podczas [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.
* Jeśli właściwość została zmieniona po pobrano, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) jest generowany. 

Model danych i bazy danych musi być skonfigurowany do obsługi zgłaszanie `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Wykrywanie konfliktów współbieżności we właściwości

Mogą być wykrywane konfliktom współbieżności na poziomie właściwości z [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atrybutu. Ten atrybut można zastosować na wiele właściwości w modelu. Aby uzyskać więcej informacji, zobacz [danych adnotacje-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` Atrybut nie jest używana w tym samouczku.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Wykrywanie konfliktów współbieżności na wiersz

Aby wykryć konfliktom współbieżności [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) kolumny śledzenia jest dodawane do modelu.  `rowversion` :

* Dotyczy programu SQL Server. Innych baz danych może nie zapewniać podobnych funkcji.
* Służy do określania, czy jednostka nie ma została zmieniona, ponieważ została ona pobrana z bazy danych. 

Bazy danych generuje kolejna `rowversion` numer jest zwiększany po każdej wiersz jest aktualizowany. W `Update` lub `Delete` polecenia `Where` klauzuli obejmuje pobranych wartość `rowversion`. Zmiana aktualizacji wiersza:

 * `rowversion`nie pasuje do wartości pobranych.
 * `Update` Lub `Delete` poleceń nie odnaleźć wiersza, ponieważ `Where` klauzula zawiera pobranych `rowversion`.
 * A `DbUpdateConcurrencyException` jest generowany.

W podstawowej EF, gdy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia, jest zgłaszany wyjątek współbieżności.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Dodaj właściwość śledzenia do działu jednostki

W *Models/Department.cs*, Dodaj właściwość śledzenia o nazwie RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Sygnatury czasowej](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atrybut określa, czy ta kolumna jest uwzględniona w `Where` klauzuli `Update` i `Delete` poleceń. Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używany SQL `timestamp` — typ danych przed SQL `rowversion` zamieniony typu.

Interfejsu API fluent można również określić właściwość śledzenia:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Poniższy kod przedstawia część generowane przez podstawowe EF po zaktualizowaniu nazwę działu T-SQL:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Poprzedni wyróżniony kod pokazuje `WHERE` zawierające klauzuli `RowVersion`. Jeśli bazy danych `RowVersion` nie jest równa `RowVersion` parametr (`@p2`), żadne wiersze nie zostały zaktualizowane.

Następujący wyróżniony kod T-SQL, który sprawdza, czy dokładnie jeden wiersz został zaktualizowany:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy objętych ostatniej instrukcji. W żadnym wierszy są aktualizowane, zgłasza EF Core `DbUpdateConcurrencyException`.

Można zauważyć, że generuje rdzeń EF T-SQL w oknie danych wyjściowych programu Visual Studio.

### <a name="update-the-db"></a>Zaktualizuj bazę danych

Dodawanie `RowVersion` zmiany właściwości modelu bazy danych, który wymaga migracji.

Skompiluj projekt. W oknie polecenia wprowadź następujący:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Poprzednie polecenia:

* Dodaje *migracje / {stamp}_RowVersion.cs czasu* pliku migracji.
* Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku. Aktualizacja dodaje następujące wyróżniony kod do `BuildModel` metody:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Uruchamia migracji, aby zaktualizować bazę danych.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Tworzenie szkieletu modelu działów

* Zamknij program Visual Studio.
* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenie:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

Poprzedni rusztowania polecenia `Department` modelu. Otwórz projekt w programie Visual Studio.

Skompiluj projekt. Kompilacja generuje błędy podobne do następujących:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Globalnie zmienić `_context.Department` do `_context.Departments` (to znaczy dodania "s" do `Department`). 7 wystąpienia są odnaleźć i zaktualizować.

### <a name="update-the-departments-index-page"></a>Zaktualizuj strony indeksu działów

Aparat szkieletów utworzony `RowVersion` nie można wyświetlić kolumny do strony indeksu, ale tego pola. W tym samouczku ostatniego bajtu `RowVersion` wyświetleniem ułatwi zrozumienie współbieżności. Ostatniego bajtu nie musi być unikatowy. Rzeczywiste aplikacji nie wyświetlić `RowVersion` lub ostatniego bajtu `RowVersion`.

Zaktualizuj strony indeksu:

* Zastąp indeksu działów.
* Zastąp kod znaczników zawierający `RowVersion` z ostatniego bajtu `RowVersion`.
* Zastąp FirstMidName imię i nazwisko.

Następujący kod przedstawia zaktualizowanej strony:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aktualizacja modelu strony edycji

Aktualizacja *pages\departments\edit.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Aby wykryć problem współbieżności, [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) został zaktualizowany o `rowVersion` wartości z obiektu jego pobrania. EF Core generuje polecenia aktualizacji SQL z klauzula WHERE zawiera oryginał `RowVersion` wartość. Jeśli żadne wiersze nie dotyczy polecenia aktualizacji (żadnych wierszy ma oryginalną `RowVersion` wartość), `DbUpdateConcurrencyException` wyjątku.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

W powyższym kodzie `Department.RowVersion` jest to wartość, jeśli jednostka została pobrana. `OriginalValue`jest to wartość w bazie danych podczas `FirstOrDefaultAsync` została wywołana w ramach tej metody.

Poniższy kod pobiera wartości klienta (wartości do tej metody) i wartości bazy danych:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Oto kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma DB wartości inne niż co zaksięgowania `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Następujące wyróżniony kod zestawy `RowVersion` wartość do nowej wartości pobrana z bazy danych. Przy następnym kliknięciu **zapisać**, tylko błędy współbieżności zachodzące od czasu ostatniego wyświetlania edycji strony zostanie przechwycony.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` Instrukcja jest wymagane, ponieważ `ModelState` ma stary `RowVersion` wartość. Na stronie aparatu Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości w modelu, gdy istnieją obie.

## <a name="update-the-edit-page"></a>Aktualizacja edycji strony

Aktualizacja *Pages/Departments/Edit.cshtml* z następujący kod:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Poprzedni kod znaczników:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.
* Dodaje wersji ukrytego wiersza. `RowVersion`musi zostać dodany, ogłaszanie zwrotne wiąże wartość.
* Wyświetla ostatniego bajtu `RowVersion` na potrzeby debugowania.
* Zastępuje `ViewData` z jednoznacznie `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Testowanie konfliktom współbieżności na stronie edycji

Otwórz dwa wystąpienia przeglądarki edycji w angielskiej wersji językowej działu:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy **Edytuj** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie**.
* Na pierwszej karcie, kliknij **Edytuj** hyperlink działu angielskiej wersji językowej.

Karty przeglądarki dwóch wyświetlenia tych samych informacji.

Zmień nazwę na pierwszej karcie przeglądarki i kliknij przycisk **zapisać**.

![Edytowanie działu strony 1 po zmianie](concurrency/_static/edit-after-change-1.png)

Przeglądarka wyświetla stronę indeksu z zmieniona wartość i zaktualizowane rowVersion wskaźnika. Należy zwrócić uwagę wskaźnika zaktualizowane rowVersion jest wyświetlany na drugi ogłaszania zwrotnego na karcie inne.

Zmień innego pola w drugiej karty przeglądarki.

![Edytowanie działu strony 2 po zmianie](concurrency/_static/edit-after-change-2.png)

Kliknij przycisk **zapisać**. Możesz wyświetlić komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

Aby zmienić nazwę pola przypadkowo tego okna przeglądarki. Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa. Karta wychodzących. Sprawdzanie poprawności klienta usuwa komunikat o błędzie.

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

Kliknij przycisk **zapisać** ponownie. Wartość wprowadzona w drugiej karty przeglądarki jest zapisywana. Zostanie wyświetlony zapisanych wartości na stronie indeksu.

## <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

Aktualizacja modelu strony Usuń z następującym kodem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Strona usuwania wykrywa konfliktom współbieżności jednostka została zmieniona po jego pobrania. `Department.RowVersion`jest wersja wiersza, jeśli jednostka została pobrana. Podstawowe EF tworzy polecenia SQL DELETE, zawiera klauzulę WHERE z `RowVersion`. Jeśli wpływ na wyniki polecenia SQL DELETE żadnych wierszy:

* `RowVersion` W SQL DELETE nie odpowiada polecenia `RowVersion` w bazie danych.
* DbUpdateConcurrencyException wyjątku.
* `OnGetAsync`jest wywoływana z `concurrencyError`.

### <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Poprzedni kod znaczników wprowadza następujące zmiany:

* Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.
* Dodaje komunikat o błędzie.
* Zamienia FirstMidName imię i nazwisko w **administratora** pola.
* Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.
* Dodaje wersji ukrytego wiersza. `RowVersion`musi zostać dodany, ogłaszanie zwrotne wiąże wartość.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Testowanie konfliktom współbieżności na stronie Delete

Utwórz działu testu.

Otwórz dwa wystąpienia przeglądarki usuwania w dziale testu:

* Uruchom aplikację i wybierz działów.
* Kliknij prawym przyciskiem myszy **usunąć** hiperłącza dla działu testu oraz wybierz **Otwórz na nowej karcie**.
* Kliknij przycisk **Edytuj** hiperłącze dla działu testu.

Karty przeglądarki dwóch wyświetlenia tych samych informacji.

Zmień budżet pierwszej karcie przeglądarki i kliknij **zapisać**.

Przeglądarka wyświetla stronę indeksu z zmieniona wartość i zaktualizowane rowVersion wskaźnika. Należy zwrócić uwagę wskaźnika zaktualizowane rowVersion jest wyświetlany na drugi ogłaszania zwrotnego na karcie inne.

Usuń działu testu z drugiej karty. Błąd współbieżności jest wyświetlana bieżącymi wartościami z bazy danych. Kliknięcie przycisku **usunąć** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.

Zobacz [dziedziczenia](xref:data/ef-mvc/inheritance) na temat sposobu dziedziczą modelu danych.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Tokeny współbieżności w EF Core](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [Obsługa współbieżności w EF Core](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Poprzednie](xref:data/ef-rp/update-related-data)
