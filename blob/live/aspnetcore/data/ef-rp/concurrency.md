---
title: "Stron razor podstawowych EF - współbieżności - 8 8"
author: rick-anderson
description: "Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie."
keywords: "Współbieżność platformy ASP.NET Core Entity Framework Core,"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 841c638b2cacaab7970f2b173fee488972957b63
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
<span data-ttu-id="8dbe0-104">en-us /</span><span class="sxs-lookup"><span data-stu-id="8dbe0-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="8dbe0-105">Obsługa konfliktom współbieżności - Core EF Razor strony (8 8)</span><span class="sxs-lookup"><span data-stu-id="8dbe0-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="8dbe0-106">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra), i [Jan Kowalski P](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="8dbe0-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="8dbe0-107">Ten samouczek pokazuje sposób obsługi konflikty, gdy wielu użytkowników zaktualizować jednostki, jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="8dbe0-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="8dbe0-108">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="8dbe0-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="8dbe0-109">Konfliktom współbieżności</span><span class="sxs-lookup"><span data-stu-id="8dbe0-109">Concurrency conflicts</span></span>

<span data-ttu-id="8dbe0-110">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="8dbe0-111">Użytkownik przechodzi do edycji strony dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="8dbe0-112">Inny użytkownik aktualizacje tej samej jednostki przed zapisaniem zmian pierwszy użytkownik z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="8dbe0-113">Jeśli wykrywanie współbieżności nie jest włączone, gdy występują równoczesnych aktualizacji:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="8dbe0-114">Ostatnia aktualizacja usługi wins.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-114">The last update wins.</span></span> <span data-ttu-id="8dbe0-115">Oznacza to ostatnie wartości aktualizacji są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="8dbe0-116">Pierwszy bieżące aktualizacje zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="8dbe0-117">Optymistycznej współbieżności</span><span class="sxs-lookup"><span data-stu-id="8dbe0-117">Optimistic concurrency</span></span>

<span data-ttu-id="8dbe0-118">Optymistycznej współbieżności umożliwia konfliktom współbieżności mieć miejsce, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="8dbe0-119">Na przykład Magdalena odwiedza edycji strony działu i zmienia budżet dla angielskiej wersji językowej działu z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="8dbe0-121">Przed kliknie Magdalena **zapisać**, Jan odwiedza tej samej stronie i zmienia pole Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia 2013](concurrency/_static/change-date.png)

<span data-ttu-id="8dbe0-123">Magdalena kliknie **zapisać** pierwszy i widzi jej zmienić, gdy w przeglądarce pojawi się strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budżet zmienione od zera.](concurrency/_static/budget-zero.png)

<span data-ttu-id="8dbe0-125">Jan klika **zapisać** na stronie edycji nadal pokazujący budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="8dbe0-126">Co dalej zależy od sposobu obsługi konfliktom współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="8dbe0-127">Optymistycznej współbieżności zawiera następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="8dbe0-128">Można zachować informacje o właściwości, które zostało zmodyfikowane przez użytkownika i aktualizować tylko odpowiednie kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="8dbe0-129">W tym scenariuszu żadne dane nie może zostać utracone.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="8dbe0-130">Inne właściwości zostały zaktualizowane przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="8dbe0-131">Przy następnym ktoś przegląda w angielskiej wersji językowej działu, ich zmiany będą widoczne zarówno Joanny i jego Jan.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="8dbe0-132">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="8dbe0-133">Takie podejście: \* nie można uniknąć utraty danych, jeśli konkurują zmian z tą samą właściwością.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-133">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="8dbe0-134">\* Jest zazwyczaj nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-134">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="8dbe0-135">Wymaga to zachowanie znaczących stanu w celu śledzenia wszystkich pobranych oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="8dbe0-136">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="8dbe0-137">\* Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności na jednostkę.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-137">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="8dbe0-138">Możesz pozwolić, aby zmiany w Jan zastąpić zmiany nazwy.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="8dbe0-139">Przy następnym ktoś przegląda w angielskiej wersji językowej działu, będzie zobaczy 9/1/2013 i pobranych wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="8dbe0-140">Ta metoda jest wywoływana *klienta Wins* lub *ostatniego w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="8dbe0-141">(Wszystkie wartości z klienta wyższy priorytet niż co znajduje się w magazynie danych). Jeśli nie ma żadnych kodowania obsługi współbieżności, Wins klienta odbywa się automatycznie.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="8dbe0-142">Aby uniemożliwić zmianę jego Jan aktualizację w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="8dbe0-143">Zwykle, czy aplikacja: \* wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-143">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="8dbe0-144">\* Wyświetlić bieżący stan danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-144">\* Show the current state of the data.</span></span>
        <span data-ttu-id="8dbe0-145">\* Umożliwia użytkownikowi ponownie zastosuj zmiany.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-145">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="8dbe0-146">Ta metoda jest wywoływana *Wins magazynu* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="8dbe0-147">(Wartości magazynu danych mają priorytet nad wartości przesłany przez klienta). W tym samouczku należy wdrożyć scenariusz dla magazynu usługi Wins.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="8dbe0-148">Ta metoda gwarantuje, że żadne zmiany nie zostaną zastąpione bez użytkownika w tym celu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="8dbe0-149">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="8dbe0-149">Handling concurrency</span></span> 

<span data-ttu-id="8dbe0-150">Jeśli właściwość została skonfigurowana jako [tokenu współbieżności](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="8dbe0-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="8dbe0-151">Podstawowe EF sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="8dbe0-152">Sprawdzanie jest wykonywane podczas [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="8dbe0-153">Jeśli właściwość została zmieniona po pobrano, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) jest generowany.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="8dbe0-154">Model danych i bazy danych musi być skonfigurowany do obsługi zgłaszanie `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="8dbe0-155">Wykrywanie konfliktów współbieżności we właściwości</span><span class="sxs-lookup"><span data-stu-id="8dbe0-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="8dbe0-156">Mogą być wykrywane konfliktom współbieżności na poziomie właściwości z [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="8dbe0-157">Ten atrybut można zastosować na wiele właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="8dbe0-158">Aby uzyskać więcej informacji, zobacz [danych adnotacje-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="8dbe0-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="8dbe0-159">`[ConcurrencyCheck]` Atrybut nie jest używana w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="8dbe0-160">Wykrywanie konfliktów współbieżności na wiersz</span><span class="sxs-lookup"><span data-stu-id="8dbe0-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="8dbe0-161">Aby wykryć konfliktom współbieżności [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) kolumny śledzenia jest dodawane do modelu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="8dbe0-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="8dbe0-162">`rowversion` :</span></span>

* <span data-ttu-id="8dbe0-163">Dotyczy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-163">Is SQL Server specific.</span></span> <span data-ttu-id="8dbe0-164">Innych baz danych może nie zapewniać podobnych funkcji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="8dbe0-165">Służy do określania, czy jednostka nie ma została zmieniona, ponieważ została ona pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="8dbe0-166">Bazy danych generuje kolejna `rowversion` numer jest zwiększany po każdej wiersz jest aktualizowany.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="8dbe0-167">W `Update` lub `Delete` polecenia `Where` klauzuli obejmuje pobranych wartość `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="8dbe0-168">Zmiana aktualizacji wiersza:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="8dbe0-169">`rowversion`nie pasuje do wartości pobranych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="8dbe0-170">`Update` Lub `Delete` poleceń nie odnaleźć wiersza, ponieważ `Where` klauzula zawiera pobranych `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="8dbe0-171">A `DbUpdateConcurrencyException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="8dbe0-172">W podstawowej EF, gdy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia, jest zgłaszany wyjątek współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="8dbe0-173">Dodaj właściwość śledzenia do działu jednostki</span><span class="sxs-lookup"><span data-stu-id="8dbe0-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="8dbe0-174">W *Models/Department.cs*, Dodaj właściwość śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="8dbe0-175">[Sygnatury czasowej](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atrybut określa, czy ta kolumna jest uwzględniona w `Where` klauzuli `Update` i `Delete` poleceń.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="8dbe0-176">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używany SQL `timestamp` — typ danych przed SQL `rowversion` zamieniony typu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="8dbe0-177">Interfejsu API fluent można również określić właściwość śledzenia:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="8dbe0-178">Poniższy kod przedstawia część generowane przez podstawowe EF po zaktualizowaniu nazwę działu T-SQL:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="8dbe0-179">Poprzedni wyróżniony kod pokazuje `WHERE` zawierające klauzuli `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="8dbe0-180">Jeśli bazy danych `RowVersion` nie jest równa `RowVersion` parametr (`@p2`), żadne wiersze nie zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="8dbe0-181">Następujący wyróżniony kod T-SQL, który sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="8dbe0-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy objętych ostatniej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="8dbe0-183">W żadnym wierszy są aktualizowane, zgłasza EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="8dbe0-184">Można zauważyć, że generuje rdzeń EF T-SQL w oknie danych wyjściowych programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="8dbe0-185">Zaktualizuj bazę danych</span><span class="sxs-lookup"><span data-stu-id="8dbe0-185">Update the DB</span></span>

<span data-ttu-id="8dbe0-186">Dodawanie `RowVersion` zmiany właściwości modelu bazy danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="8dbe0-187">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-187">Build the project.</span></span> <span data-ttu-id="8dbe0-188">W oknie polecenia wprowadź następujący:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="8dbe0-189">Poprzednie polecenia:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-189">The preceding commands:</span></span>

* <span data-ttu-id="8dbe0-190">Dodaje *migracje / {stamp}_RowVersion.cs czasu* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="8dbe0-191">Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="8dbe0-192">Aktualizacja dodaje następujące wyróżniony kod do `BuildModel` metody:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="8dbe0-193">Uruchamia migracji, aby zaktualizować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="8dbe0-194">Tworzenie szkieletu modelu działów</span><span class="sxs-lookup"><span data-stu-id="8dbe0-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="8dbe0-195">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="8dbe0-196">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="8dbe0-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="8dbe0-197">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="8dbe0-198">Poprzedni rusztowania polecenia `Department` modelu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="8dbe0-199">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="8dbe0-200">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-200">Build the project.</span></span> <span data-ttu-id="8dbe0-201">Kompilacja generuje błędy podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="8dbe0-202">Globalnie zmienić `_context.Department` do `_context.Departments` (to znaczy dodania "s" do `Department`).</span><span class="sxs-lookup"><span data-stu-id="8dbe0-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="8dbe0-203">7 wystąpienia są odnaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="8dbe0-204">Zaktualizuj strony indeksu działów</span><span class="sxs-lookup"><span data-stu-id="8dbe0-204">Update the Departments Index page</span></span>

<span data-ttu-id="8dbe0-205">Aparat szkieletów utworzony `RowVersion` nie można wyświetlić kolumny do strony indeksu, ale tego pola.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="8dbe0-206">W tym samouczku ostatniego bajtu `RowVersion` wyświetleniem ułatwi zrozumienie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="8dbe0-207">Ostatniego bajtu nie musi być unikatowa.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="8dbe0-208">Rzeczywiste aplikacji nie wyświetlić `RowVersion` lub ostatniego bajtu `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="8dbe0-209">Zaktualizuj strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-209">Update the Index page:</span></span>

* <span data-ttu-id="8dbe0-210">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="8dbe0-211">Zastąp kod znaczników zawierający `RowVersion` z ostatniego bajtu `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="8dbe0-212">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="8dbe0-213">Następujący kod przedstawia zaktualizowanej strony:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="8dbe0-214">Aktualizacja modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="8dbe0-214">Update the Edit page model</span></span>

<span data-ttu-id="8dbe0-215">Aktualizacja *pages\departments\edit.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="8dbe0-216">Aby wykryć problem współbieżności, [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) został zaktualizowany o `rowVersion` wartości z obiektu jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="8dbe0-217">EF Core generuje polecenia aktualizacji SQL z klauzula WHERE zawiera oryginał `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="8dbe0-218">Jeśli żadne wiersze nie dotyczy polecenia aktualizacji (żadnych wierszy ma oryginalną `RowVersion` wartość), `DbUpdateConcurrencyException` wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="8dbe0-219">W powyższym kodzie `Department.RowVersion` jest to wartość, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="8dbe0-220">`OriginalValue`jest to wartość w bazie danych podczas `FirstOrDefaultAsync` została wywołana w ramach tej metody.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="8dbe0-221">Poniższy kod pobiera wartości klienta (wartości do tej metody) i wartości bazy danych:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="8dbe0-222">Oto kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma DB wartości inne niż co zaksięgowania `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="8dbe0-223">Następujące wyróżniony kod zestawy `RowVersion` wartość do nowej wartości pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="8dbe0-224">Przy następnym kliknięciu **zapisać**, tylko błędy współbieżności zachodzące od czasu ostatniego wyświetlania edycji strony zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="8dbe0-225">`ModelState.Remove` Instrukcja jest wymagane, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="8dbe0-226">Na stronie aparatu Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości w modelu, gdy istnieją obie.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="8dbe0-227">Aktualizacja edycji strony</span><span class="sxs-lookup"><span data-stu-id="8dbe0-227">Update the Edit page</span></span>

<span data-ttu-id="8dbe0-228">Aktualizacja *Pages/Departments/Edit.cshtml* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="8dbe0-229">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-229">The preceding markup:</span></span>

* <span data-ttu-id="8dbe0-230">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="8dbe0-231">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-231">Adds a hidden row version.</span></span> <span data-ttu-id="8dbe0-232">`RowVersion`musi zostać dodany, ogłaszanie zwrotne wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="8dbe0-233">Wyświetla ostatniego bajtu `RowVersion` na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="8dbe0-234">Zastępuje `ViewData` z jednoznacznie `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="8dbe0-235">Testowanie konfliktom współbieżności na stronie edycji</span><span class="sxs-lookup"><span data-stu-id="8dbe0-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="8dbe0-236">Otwórz dwa wystąpienia przeglądarki edycji w angielskiej wersji językowej działu:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="8dbe0-237">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="8dbe0-238">Kliknij prawym przyciskiem myszy **Edytuj** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="8dbe0-239">Na pierwszej karcie, kliknij **Edytuj** hyperlink działu angielskiej wersji językowej.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="8dbe0-240">Karty przeglądarki dwóch wyświetlenia tych samych informacji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="8dbe0-241">Zmień nazwę na pierwszej karcie przeglądarki i kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-241">Change the name in the first browser tab and click **Save**.</span></span>

![Edytowanie działu strony 1 po zmianie](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="8dbe0-243">Przeglądarka wyświetla stronę indeksu z zmieniona wartość i zaktualizowane rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="8dbe0-244">Należy zwrócić uwagę wskaźnika zaktualizowane rowVersion jest wyświetlany na drugi ogłaszania zwrotnego na karcie inne.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="8dbe0-245">Zmień innego pola w drugiej karty przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-245">Change a different field in the second browser tab.</span></span>

![Edytowanie działu strony 2 po zmianie](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="8dbe0-247">Kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-247">Click **Save**.</span></span> <span data-ttu-id="8dbe0-248">Możesz wyświetlić komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-248">You see error messages for all fields that don't match the DB values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="8dbe0-250">To okno przeglądarki nie chcesz zmienić nazwę pola.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="8dbe0-251">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="8dbe0-252">Karta wychodzących. Sprawdzanie poprawności klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-252">Tab out. Client-side validation removes the error message.</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

<span data-ttu-id="8dbe0-254">Kliknij przycisk **zapisać** ponownie.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-254">Click **Save** again.</span></span> <span data-ttu-id="8dbe0-255">Wartość wprowadzona w drugiej karty przeglądarki jest zapisywana.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="8dbe0-256">Zostanie wyświetlony zapisanych wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="8dbe0-257">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="8dbe0-257">Update the Delete page</span></span>

<span data-ttu-id="8dbe0-258">Aktualizacja modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="8dbe0-259">Strona usuwania wykrywa konfliktom współbieżności jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="8dbe0-260">`Department.RowVersion`jest wersja wiersza, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="8dbe0-261">Podstawowe EF tworzy polecenia SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="8dbe0-262">Jeśli wpływ na wyniki polecenia SQL DELETE żadnych wierszy:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="8dbe0-263">`RowVersion` W SQL DELETE nie odpowiada polecenia `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="8dbe0-264">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="8dbe0-265">`OnGetAsync`jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="8dbe0-266">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="8dbe0-266">Update the Delete page</span></span>

<span data-ttu-id="8dbe0-267">Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="8dbe0-268">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="8dbe0-269">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="8dbe0-270">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-270">Adds an error message.</span></span>
* <span data-ttu-id="8dbe0-271">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="8dbe0-272">Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="8dbe0-273">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-273">Adds a hidden row version.</span></span> <span data-ttu-id="8dbe0-274">`RowVersion`musi zostać dodany, ogłaszanie zwrotne wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="8dbe0-275">Testowanie konfliktom współbieżności na stronie Delete</span><span class="sxs-lookup"><span data-stu-id="8dbe0-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="8dbe0-276">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-276">Create a test department.</span></span>

<span data-ttu-id="8dbe0-277">Otwórz dwa wystąpienia przeglądarki usuwania w dziale testu:</span><span class="sxs-lookup"><span data-stu-id="8dbe0-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="8dbe0-278">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="8dbe0-279">Kliknij prawym przyciskiem myszy **usunąć** hiperłącza dla działu testu oraz wybierz **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="8dbe0-280">Kliknij przycisk **Edytuj** hiperłącze dla działu testu.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="8dbe0-281">Karty przeglądarki dwóch wyświetlenia tych samych informacji.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="8dbe0-282">Zmień budżet pierwszej karcie przeglądarki i kliknij **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="8dbe0-283">Przeglądarka wyświetla stronę indeksu z zmieniona wartość i zaktualizowane rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="8dbe0-284">Należy zwrócić uwagę wskaźnika zaktualizowane rowVersion jest wyświetlany na drugi ogłaszania zwrotnego na karcie inne.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="8dbe0-285">Usuń działu testu z drugiej karty. Błąd współbieżności jest wyświetlana bieżącymi wartościami z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="8dbe0-286">Kliknięcie przycisku **usunąć** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="8dbe0-287">Zobacz [dziedziczenia](xref:data/ef-mvc/inheritance) na temat sposobu dziedziczą modelu danych.</span><span class="sxs-lookup"><span data-stu-id="8dbe0-287">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="8dbe0-288">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8dbe0-288">Additional resources</span></span>

* [<span data-ttu-id="8dbe0-289">Tokeny współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="8dbe0-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="8dbe0-290">Obsługa współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="8dbe0-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="8dbe0-291">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="8dbe0-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
