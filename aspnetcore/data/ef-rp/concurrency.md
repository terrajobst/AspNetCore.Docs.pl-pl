---
title: Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: c6ec07eb7bf484490bd7730edc44bf2d89e8fb2a
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38150486"
---
<span data-ttu-id="2d0ca-103">en-us /</span><span class="sxs-lookup"><span data-stu-id="2d0ca-103">en-us/</span></span>

# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="2d0ca-104">Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8</span><span class="sxs-lookup"><span data-stu-id="2d0ca-104">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="2d0ca-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), i [Jan Kowalski P](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="2d0ca-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="2d0ca-106">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="2d0ca-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="2d0ca-107">Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="2d0ca-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="2d0ca-108">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="2d0ca-108">Concurrency conflicts</span></span>

<span data-ttu-id="2d0ca-109">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="2d0ca-110">Użytkownik przejdzie do strony edytowania dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="2d0ca-111">Inny użytkownik aktualizuje tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="2d0ca-112">Jeśli wykrycie współbieżności nie jest włączone, gdy wystąpią równoczesne aktualizacje:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="2d0ca-113">Ostatnia aktualizacja usługi wins.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-113">The last update wins.</span></span> <span data-ttu-id="2d0ca-114">Oznacza to, że ostatnie wartości aktualizacji są zapisywane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="2d0ca-115">Pierwszego dnia bieżące aktualizacje zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="2d0ca-116">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="2d0ca-116">Optimistic concurrency</span></span>

<span data-ttu-id="2d0ca-117">Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="2d0ca-118">Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="2d0ca-120">Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

<span data-ttu-id="2d0ca-122">Magdalena kliknie **Zapisz** pierwszy i widzi jej zmienić, gdy w przeglądarce pojawi się strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budżet na zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="2d0ca-124">John kliknie **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="2d0ca-125">Co dzieje się potem określają sposób obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="2d0ca-126">Optymistyczna współbieżność obejmuje następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="2d0ca-127">Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="2d0ca-128">W tym scenariuszu zostałyby utracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="2d0ca-129">Inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="2d0ca-130">Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="2d0ca-131">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="2d0ca-132">To podejście: \* nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="2d0ca-133">\* Jest zwykle nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="2d0ca-134">Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="2d0ca-135">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="2d0ca-136">\* Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="2d0ca-137">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="2d0ca-138">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="2d0ca-139">To podejście jest nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="2d0ca-140">(Wszystkie wartości z klienta pierwszeństwo co znajduje się w magazynie danych.) Jeśli nie możesz tworzyć jakiegokolwiek kodu do obsługi współbieżności, Wins klienta odbywa się automatycznie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="2d0ca-141">Aby uniemożliwić zmiany John's aktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="2d0ca-142">Zwykle, czy aplikacja: \* wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="2d0ca-143">\* Umożliwia wyświetlenie bieżącego stanu danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="2d0ca-144">\* Umożliwia użytkownikowi ponownie zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-144">\* Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="2d0ca-145">Jest to nazywane *Store Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="2d0ca-146">(Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) W tym samouczku jest zaimplementowania scenariusza Store Wins.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="2d0ca-147">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="2d0ca-148">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="2d0ca-148">Handling concurrency</span></span> 

<span data-ttu-id="2d0ca-149">Gdy właściwość jest skonfigurowany jako [tokenu współbieżności](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="2d0ca-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="2d0ca-150">EF Core sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="2d0ca-151">Sprawdzanie jest wykonywane podczas [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-151">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="2d0ca-152">Jeśli właściwość została zmieniona po jego pobrania, [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="2d0ca-153">Model danych i bazy danych musi być skonfigurowany do obsługi zgłaszanie `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="2d0ca-154">Wykrywanie konfliktów współbieżności we właściwości</span><span class="sxs-lookup"><span data-stu-id="2d0ca-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="2d0ca-155">Konfliktów współbieżności może zostać wykryte przy użyciu na poziomie właściwość [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="2d0ca-156">Ten atrybut można zastosować na wiele właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="2d0ca-157">Aby uzyskać więcej informacji, zobacz [danych adnotacje-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="2d0ca-157">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="2d0ca-158">`[ConcurrencyCheck]` Atrybut nie jest używany w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="2d0ca-159">Wykrywanie konfliktów współbieżności wiersz</span><span class="sxs-lookup"><span data-stu-id="2d0ca-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="2d0ca-160">Do wykrywania konfliktów współbieżności [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) kolumny śledzenia jest dodawany do modelu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-160">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="2d0ca-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="2d0ca-161">`rowversion` :</span></span>

* <span data-ttu-id="2d0ca-162">Dotyczy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-162">Is SQL Server specific.</span></span> <span data-ttu-id="2d0ca-163">Inne bazy danych nie mogą zawierać podobnych funkcji.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="2d0ca-164">Służy do określenia, czy jednostka nie została zmieniona, ponieważ została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="2d0ca-165">Baza danych generuje kolejna `rowversion` numer jest zwiększany po każdej wiersz jest aktualizowany.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="2d0ca-166">W `Update` lub `Delete` polecenia `Where` klauzula zawiera wartość pobrano `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="2d0ca-167">Jeśli zmieniono aktualizacji wiersza:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="2d0ca-168">`rowversion` nie pasuje do wartości pobrano.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="2d0ca-169">`Update` Lub `Delete` poleceń nie znajdziesz wiersza, ponieważ `Where` klauzula zawiera pobrano `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="2d0ca-170">A `DbUpdateConcurrencyException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="2d0ca-171">W programie EF Core, gdy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia, jest zgłaszany wyjątek współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="2d0ca-172">Dodawanie właściwości śledzenia do jednostki działu</span><span class="sxs-lookup"><span data-stu-id="2d0ca-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="2d0ca-173">W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="2d0ca-174">[Sygnatura czasowa](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atrybut określa, czy ta kolumna znajduje się w `Where` klauzuli `Update` i `Delete` poleceń.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-174">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="2d0ca-175">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` typ zastąpiono ją.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="2d0ca-176">Interfejs fluent API można również określić właściwości śledzenia:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="2d0ca-177">Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="2d0ca-178">Poprzednie wyróżnione przedstawia kod `WHERE` zawierających klauzulę `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="2d0ca-179">Jeśli bazy danych `RowVersion` nie równa się `RowVersion` parametru (`@p2`), wiersze nie zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="2d0ca-180">Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="2d0ca-181">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy na ostatniej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-181">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="2d0ca-182">W żadnym wiersze są aktualizowane, zgłasza programu EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="2d0ca-183">Widać, że T-SQL programu EF Core generuje w oknie danych wyjściowych programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="2d0ca-184">Aktualizacja bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d0ca-184">Update the DB</span></span>

<span data-ttu-id="2d0ca-185">Dodawanie `RowVersion` zmienia właściwość modelu bazy danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="2d0ca-186">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-186">Build the project.</span></span> <span data-ttu-id="2d0ca-187">W oknie polecenia, należy wprowadzić następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="2d0ca-188">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-188">The preceding commands:</span></span>

* <span data-ttu-id="2d0ca-189">Dodaje *migracje / {stamp}_RowVersion.cs czasu* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="2d0ca-190">Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="2d0ca-191">Aktualizacja dodaje następujący wyróżniony kod do `BuildModel` metody:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="2d0ca-192">Uruchamia migracji można zaktualizować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="2d0ca-193">Tworzenie szkieletu modelu działów</span><span class="sxs-lookup"><span data-stu-id="2d0ca-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="2d0ca-194">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="2d0ca-195">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="2d0ca-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="2d0ca-196">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-196">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="2d0ca-197">Poprzedni szkielety mechanizmów polecenia `Department` modelu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="2d0ca-198">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="2d0ca-199">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-199">Build the project.</span></span> <span data-ttu-id="2d0ca-200">Kompilacja generuje błędy podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="2d0ca-201">Globalnie zmienić `_context.Department` do `_context.Departments` (oznacza to, Dodaj znak "s" do `Department`).</span><span class="sxs-lookup"><span data-stu-id="2d0ca-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="2d0ca-202">7 wystąpień są znaleźć i zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="2d0ca-203">Zaktualizuj strony działy indeksu</span><span class="sxs-lookup"><span data-stu-id="2d0ca-203">Update the Departments Index page</span></span>

<span data-ttu-id="2d0ca-204">Aparat tworzenia szkieletów utworzone `RowVersion` kolumny do strony indeksu, ale tego pola nie powinna wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="2d0ca-205">W tym samouczku, ostatni bajt `RowVersion` jest wyświetlana, aby lepiej zrozumieć współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="2d0ca-206">Ostatni bajt nie musi być unikatowy.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="2d0ca-207">Rzeczywistej aplikacji nie wyświetlał `RowVersion` lub ostatni bajt `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="2d0ca-208">Zaktualizuj strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-208">Update the Index page:</span></span>

* <span data-ttu-id="2d0ca-209">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="2d0ca-210">Zastąp kod znaczników zawierający `RowVersion` z ostatniego bajtu `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="2d0ca-211">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="2d0ca-212">Następujący kod przedstawia zaktualizowaną stronę:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="2d0ca-213">Aktualizowanie modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="2d0ca-213">Update the Edit page model</span></span>

<span data-ttu-id="2d0ca-214">Aktualizacja *pages\departments\edit.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="2d0ca-215">Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) będą aktualizowane przy użyciu `rowVersion` wartości z obiektu została ona pobrana.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="2d0ca-216">EF Core generuje polecenia aktualizacji programu SQL z klauzulą WHERE zawiera oryginał `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="2d0ca-217">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (nie wiersze mają oryginalny `RowVersion` wartość), `DbUpdateConcurrencyException` jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="2d0ca-218">W poprzednim kodzie `Department.RowVersion` jest wartością, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="2d0ca-219">`OriginalValue` jest to wartość w bazie danych podczas `FirstOrDefaultAsync` została wywołana w przypadku tej metody.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="2d0ca-220">Poniższy kod umożliwia pobranie ustawienia klienta (wartości do tej metody) oraz bazy danych wartości:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="2d0ca-221">Kod follwing dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma bazy danych wartości inne niż co opublikowano `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="2d0ca-222">Następujące wyróżniony kod ustawia `RowVersion` wartość do nowej wartości są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="2d0ca-223">Przy następnym kliknięciu **Zapisz**, tylko błędy współbieżności, które odbywa się od czasu ostatniego wyświetlania strony edytowania zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="2d0ca-224">`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="2d0ca-225">Strony Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="2d0ca-226">Zaktualizuj strony edytowania</span><span class="sxs-lookup"><span data-stu-id="2d0ca-226">Update the Edit page</span></span>

<span data-ttu-id="2d0ca-227">Aktualizacja *Pages/Departments/Edit.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="2d0ca-228">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-228">The preceding markup:</span></span>

* <span data-ttu-id="2d0ca-229">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2d0ca-230">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-230">Adds a hidden row version.</span></span> <span data-ttu-id="2d0ca-231">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="2d0ca-232">Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="2d0ca-233">Zastępuje `ViewData` z silnie typizowanych `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="2d0ca-234">Badanie konfliktów współbieżności przy użyciu strony edytowania</span><span class="sxs-lookup"><span data-stu-id="2d0ca-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="2d0ca-235">Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="2d0ca-236">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="2d0ca-237">Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2d0ca-238">Na pierwszej karcie kliknij **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="2d0ca-239">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2d0ca-240">Zmień nazwę w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-240">Change the name in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="2d0ca-242">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2d0ca-243">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2d0ca-244">Zmień inne pole w drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-244">Change a different field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="2d0ca-246">Kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-246">Click **Save**.</span></span> <span data-ttu-id="2d0ca-247">Zostaną wyświetlone komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-247">You see error messages for all fields that don't match the DB values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="2d0ca-249">To okno przeglądarki przypadkowo zmienić pole Nazwa.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="2d0ca-250">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="2d0ca-251">Karta. Weryfikacja po stronie klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-251">Tab out. Client-side validation removes the error message.</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

<span data-ttu-id="2d0ca-253">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-253">Click **Save** again.</span></span> <span data-ttu-id="2d0ca-254">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="2d0ca-255">Zobaczysz zapisane wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="2d0ca-256">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="2d0ca-256">Update the Delete page</span></span>

<span data-ttu-id="2d0ca-257">Aktualizowanie modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="2d0ca-258">Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="2d0ca-259">`Department.RowVersion` jest to wersja wiersza, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="2d0ca-260">EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="2d0ca-261">Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="2d0ca-262">`RowVersion` W SQL DELETE polecenia nie jest zgodna `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="2d0ca-263">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="2d0ca-264">`OnGetAsync` jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="2d0ca-265">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="2d0ca-265">Update the Delete page</span></span>

<span data-ttu-id="2d0ca-266">Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="2d0ca-267">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="2d0ca-268">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2d0ca-269">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-269">Adds an error message.</span></span>
* <span data-ttu-id="2d0ca-270">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="2d0ca-271">Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="2d0ca-272">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-272">Adds a hidden row version.</span></span> <span data-ttu-id="2d0ca-273">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="2d0ca-274">Badanie konfliktów współbieżności ze stroną Delete</span><span class="sxs-lookup"><span data-stu-id="2d0ca-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="2d0ca-275">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-275">Create a test department.</span></span>

<span data-ttu-id="2d0ca-276">Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:</span><span class="sxs-lookup"><span data-stu-id="2d0ca-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="2d0ca-277">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="2d0ca-278">Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu test i wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2d0ca-279">Kliknij przycisk **Edytuj** hiperłącze dla działu testu.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="2d0ca-280">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2d0ca-281">Zmień budżetu w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="2d0ca-282">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2d0ca-283">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2d0ca-284">Usuń z działu testu z drugiej karcie. Błąd współbieżności jest wyświetlana przy użyciu bieżących wartości z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="2d0ca-285">Klikając **Usuń** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="2d0ca-286">Zobacz [dziedziczenia](xref:data/ef-mvc/inheritance) na temat sposobu dziedziczą modelu danych.</span><span class="sxs-lookup"><span data-stu-id="2d0ca-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2d0ca-287">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2d0ca-287">Additional resources</span></span>

* [<span data-ttu-id="2d0ca-288">Tokeny współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="2d0ca-288">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="2d0ca-289">Obsługa współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="2d0ca-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="2d0ca-290">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="2d0ca-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
