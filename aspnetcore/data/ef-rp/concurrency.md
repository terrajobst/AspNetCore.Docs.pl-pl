---
title: Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: a010e2ed660bea56b112799e850f2fb0ff37579e
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219397"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="7faec-103">Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8</span><span class="sxs-lookup"><span data-stu-id="7faec-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="7faec-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), i [Jan Kowalski P](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="7faec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="7faec-105">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="7faec-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="7faec-106">Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="7faec-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="7faec-107">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="7faec-107">Concurrency conflicts</span></span>

<span data-ttu-id="7faec-108">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="7faec-108">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="7faec-109">Użytkownik przejdzie do strony edytowania dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="7faec-109">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="7faec-110">Inny użytkownik aktualizuje tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-110">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="7faec-111">Jeśli wykrycie współbieżności nie jest włączone, gdy wystąpią równoczesne aktualizacje:</span><span class="sxs-lookup"><span data-stu-id="7faec-111">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="7faec-112">Ostatnia aktualizacja usługi wins.</span><span class="sxs-lookup"><span data-stu-id="7faec-112">The last update wins.</span></span> <span data-ttu-id="7faec-113">Oznacza to, że ostatnie wartości aktualizacji są zapisywane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-113">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="7faec-114">Pierwszego dnia bieżące aktualizacje zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="7faec-114">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="7faec-115">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="7faec-115">Optimistic concurrency</span></span>

<span data-ttu-id="7faec-116">Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="7faec-116">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="7faec-117">Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="7faec-117">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="7faec-119">Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="7faec-119">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

<span data-ttu-id="7faec-121">Magdalena kliknie **Zapisz** pierwszy i widzi jej zmienić, gdy w przeglądarce pojawi się strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="7faec-121">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budżet na zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="7faec-123">John kliknie **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="7faec-123">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="7faec-124">Co dzieje się potem określają sposób obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="7faec-124">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="7faec-125">Optymistyczna współbieżność obejmuje następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="7faec-125">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="7faec-126">Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-126">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="7faec-127">W tym scenariuszu zostałyby utracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="7faec-127">In the scenario, no data would be lost.</span></span> <span data-ttu-id="7faec-128">Inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7faec-128">Different properties were updated by the two users.</span></span> <span data-ttu-id="7faec-129">Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany.</span><span class="sxs-lookup"><span data-stu-id="7faec-129">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="7faec-130">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-130">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="7faec-131">To podejście:</span><span class="sxs-lookup"><span data-stu-id="7faec-131">This approach:</span></span>
 
  * <span data-ttu-id="7faec-132">Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="7faec-132">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="7faec-133">Zwykle nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7faec-133">Is generally not practical in a web app.</span></span> <span data-ttu-id="7faec-134">Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="7faec-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="7faec-135">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7faec-135">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="7faec-136">Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.</span><span class="sxs-lookup"><span data-stu-id="7faec-136">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="7faec-137">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="7faec-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="7faec-138">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="7faec-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="7faec-139">To podejście jest nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="7faec-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="7faec-140">(Wszystkie wartości z klienta pierwszeństwo co znajduje się w magazynie danych.) Jeśli nie możesz tworzyć jakiegokolwiek kodu do obsługi współbieżności, Wins klienta odbywa się automatycznie.</span><span class="sxs-lookup"><span data-stu-id="7faec-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="7faec-141">Aby uniemożliwić zmiany John's aktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="7faec-142">Zazwyczaj aplikacja będzie:</span><span class="sxs-lookup"><span data-stu-id="7faec-142">Typically, the app would:</span></span>

  * <span data-ttu-id="7faec-143">Wyświetl komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7faec-143">Display an error message.</span></span>
  * <span data-ttu-id="7faec-144">Umożliwia wyświetlenie bieżącego stanu danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-144">Show the current state of the data.</span></span>
  * <span data-ttu-id="7faec-145">Zezwalaj użytkownikowi ponownie zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="7faec-145">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="7faec-146">Jest to nazywane *Store Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="7faec-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="7faec-147">(Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) W tym samouczku jest zaimplementowania scenariusza Store Wins.</span><span class="sxs-lookup"><span data-stu-id="7faec-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="7faec-148">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="7faec-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="7faec-149">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="7faec-149">Handling concurrency</span></span> 

<span data-ttu-id="7faec-150">Gdy właściwość jest skonfigurowany jako [tokenu współbieżności](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="7faec-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="7faec-151">EF Core sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="7faec-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="7faec-152">Sprawdzanie jest wykonywane podczas [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="7faec-152">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="7faec-153">Jeśli właściwość została zmieniona po jego pobrania, [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="7faec-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="7faec-154">Model danych i bazy danych musi być skonfigurowany do obsługi zgłaszanie `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="7faec-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="7faec-155">Wykrywanie konfliktów współbieżności we właściwości</span><span class="sxs-lookup"><span data-stu-id="7faec-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="7faec-156">Konfliktów współbieżności może zostać wykryte przy użyciu na poziomie właściwość [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7faec-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="7faec-157">Ten atrybut można zastosować na wiele właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="7faec-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="7faec-158">Aby uzyskać więcej informacji, zobacz [danych adnotacje-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="7faec-158">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="7faec-159">`[ConcurrencyCheck]` Atrybut nie jest używany w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7faec-159">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="7faec-160">Wykrywanie konfliktów współbieżności wiersz</span><span class="sxs-lookup"><span data-stu-id="7faec-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="7faec-161">Do wykrywania konfliktów współbieżności [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) kolumny śledzenia jest dodawany do modelu.</span><span class="sxs-lookup"><span data-stu-id="7faec-161">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="7faec-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="7faec-162">`rowversion` :</span></span>

* <span data-ttu-id="7faec-163">Dotyczy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7faec-163">Is SQL Server specific.</span></span> <span data-ttu-id="7faec-164">Inne bazy danych nie mogą zawierać podobnych funkcji.</span><span class="sxs-lookup"><span data-stu-id="7faec-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="7faec-165">Służy do określenia, czy jednostka nie została zmieniona, ponieważ została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="7faec-166">Baza danych generuje kolejna `rowversion` numer jest zwiększany po każdej wiersz jest aktualizowany.</span><span class="sxs-lookup"><span data-stu-id="7faec-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="7faec-167">W `Update` lub `Delete` polecenia `Where` klauzula zawiera wartość pobrano `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="7faec-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="7faec-168">Jeśli zmieniono aktualizacji wiersza:</span><span class="sxs-lookup"><span data-stu-id="7faec-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="7faec-169">`rowversion` nie pasuje do wartości pobrano.</span><span class="sxs-lookup"><span data-stu-id="7faec-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="7faec-170">`Update` Lub `Delete` poleceń nie znajdziesz wiersza, ponieważ `Where` klauzula zawiera pobrano `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="7faec-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="7faec-171">A `DbUpdateConcurrencyException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="7faec-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="7faec-172">W programie EF Core, gdy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia, jest zgłaszany wyjątek współbieżności.</span><span class="sxs-lookup"><span data-stu-id="7faec-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="7faec-173">Dodawanie właściwości śledzenia do jednostki działu</span><span class="sxs-lookup"><span data-stu-id="7faec-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="7faec-174">W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="7faec-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="7faec-175">[Sygnatura czasowa](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atrybut określa, czy ta kolumna znajduje się w `Where` klauzuli `Update` i `Delete` poleceń.</span><span class="sxs-lookup"><span data-stu-id="7faec-175">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="7faec-176">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` typ zastąpiono ją.</span><span class="sxs-lookup"><span data-stu-id="7faec-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="7faec-177">Interfejs fluent API można również określić właściwości śledzenia:</span><span class="sxs-lookup"><span data-stu-id="7faec-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="7faec-178">Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:</span><span class="sxs-lookup"><span data-stu-id="7faec-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="7faec-179">Poprzednie wyróżnione przedstawia kod `WHERE` zawierających klauzulę `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="7faec-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="7faec-180">Jeśli bazy danych `RowVersion` nie równa się `RowVersion` parametru (`@p2`), wiersze nie zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="7faec-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="7faec-181">Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="7faec-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="7faec-182">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy na ostatniej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="7faec-182">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="7faec-183">W żadnym wiersze są aktualizowane, zgłasza programu EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="7faec-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="7faec-184">Widać, że T-SQL programu EF Core generuje w oknie danych wyjściowych programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7faec-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="7faec-185">Aktualizacja bazy danych</span><span class="sxs-lookup"><span data-stu-id="7faec-185">Update the DB</span></span>

<span data-ttu-id="7faec-186">Dodawanie `RowVersion` zmienia właściwość modelu bazy danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="7faec-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="7faec-187">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7faec-187">Build the project.</span></span> <span data-ttu-id="7faec-188">W oknie polecenia, należy wprowadzić następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7faec-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="7faec-189">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="7faec-189">The preceding commands:</span></span>

* <span data-ttu-id="7faec-190">Dodaje *migracje / {stamp}_RowVersion.cs czasu* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="7faec-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="7faec-191">Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="7faec-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="7faec-192">Aktualizacja dodaje następujący wyróżniony kod do `BuildModel` metody:</span><span class="sxs-lookup"><span data-stu-id="7faec-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="7faec-193">Uruchamia migracji można zaktualizować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="7faec-194">Tworzenie szkieletu modelu działów</span><span class="sxs-lookup"><span data-stu-id="7faec-194">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7faec-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7faec-195">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="7faec-196">Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-the-student-model) i użyj `Department` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="7faec-196">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Department` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7faec-197">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7faec-197">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="7faec-198">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7faec-198">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

------

<span data-ttu-id="7faec-199">Poprzedni szkielety mechanizmów polecenia `Department` modelu.</span><span class="sxs-lookup"><span data-stu-id="7faec-199">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="7faec-200">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7faec-200">Open the project in Visual Studio.</span></span>

<span data-ttu-id="7faec-201">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7faec-201">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="7faec-202">Zaktualizuj strony działy indeksu</span><span class="sxs-lookup"><span data-stu-id="7faec-202">Update the Departments Index page</span></span>

<span data-ttu-id="7faec-203">Aparat tworzenia szkieletów utworzone `RowVersion` kolumny do strony indeksu, ale tego pola nie powinna wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="7faec-203">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="7faec-204">W tym samouczku, ostatni bajt `RowVersion` jest wyświetlana, aby lepiej zrozumieć współbieżności.</span><span class="sxs-lookup"><span data-stu-id="7faec-204">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="7faec-205">Ostatni bajt nie musi być unikatowy.</span><span class="sxs-lookup"><span data-stu-id="7faec-205">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="7faec-206">Rzeczywistej aplikacji nie wyświetlał `RowVersion` lub ostatni bajt `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="7faec-206">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="7faec-207">Zaktualizuj strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="7faec-207">Update the Index page:</span></span>

* <span data-ttu-id="7faec-208">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="7faec-208">Replace Index with Departments.</span></span>
* <span data-ttu-id="7faec-209">Zastąp kod znaczników zawierający `RowVersion` z ostatniego bajtu `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="7faec-209">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="7faec-210">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="7faec-210">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="7faec-211">Następujący kod przedstawia zaktualizowaną stronę:</span><span class="sxs-lookup"><span data-stu-id="7faec-211">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="7faec-212">Aktualizowanie modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="7faec-212">Update the Edit page model</span></span>

<span data-ttu-id="7faec-213">Aktualizacja *pages\departments\edit.cshtml.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7faec-213">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="7faec-214">Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) będą aktualizowane przy użyciu `rowVersion` wartości z obiektu została ona pobrana.</span><span class="sxs-lookup"><span data-stu-id="7faec-214">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="7faec-215">EF Core generuje polecenia aktualizacji programu SQL z klauzulą WHERE zawiera oryginał `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="7faec-215">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="7faec-216">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (nie wiersze mają oryginalny `RowVersion` wartość), `DbUpdateConcurrencyException` jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="7faec-216">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="7faec-217">W poprzednim kodzie `Department.RowVersion` jest wartością, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="7faec-217">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="7faec-218">`OriginalValue` jest to wartość w bazie danych podczas `FirstOrDefaultAsync` została wywołana w przypadku tej metody.</span><span class="sxs-lookup"><span data-stu-id="7faec-218">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="7faec-219">Poniższy kod umożliwia pobranie ustawienia klienta (wartości do tej metody) oraz bazy danych wartości:</span><span class="sxs-lookup"><span data-stu-id="7faec-219">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="7faec-220">Kod follwing dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma bazy danych wartości inne niż co opublikowano `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="7faec-220">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="7faec-221">Następujące wyróżniony kod ustawia `RowVersion` wartość do nowej wartości są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-221">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="7faec-222">Przy następnym kliknięciu **Zapisz**, tylko błędy współbieżności, które odbywa się od czasu ostatniego wyświetlania strony edytowania zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="7faec-222">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="7faec-223">`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="7faec-223">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="7faec-224">Strony Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.</span><span class="sxs-lookup"><span data-stu-id="7faec-224">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="7faec-225">Zaktualizuj strony edytowania</span><span class="sxs-lookup"><span data-stu-id="7faec-225">Update the Edit page</span></span>

<span data-ttu-id="7faec-226">Aktualizacja *Pages/Departments/Edit.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7faec-226">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="7faec-227">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="7faec-227">The preceding markup:</span></span>

* <span data-ttu-id="7faec-228">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="7faec-228">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="7faec-229">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="7faec-229">Adds a hidden row version.</span></span> <span data-ttu-id="7faec-230">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="7faec-230">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="7faec-231">Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="7faec-231">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="7faec-232">Zastępuje `ViewData` z silnie typizowanych `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="7faec-232">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="7faec-233">Badanie konfliktów współbieżności przy użyciu strony edytowania</span><span class="sxs-lookup"><span data-stu-id="7faec-233">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="7faec-234">Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:</span><span class="sxs-lookup"><span data-stu-id="7faec-234">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="7faec-235">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="7faec-235">Run the app and select Departments.</span></span>
* <span data-ttu-id="7faec-236">Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="7faec-236">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="7faec-237">Na pierwszej karcie kliknij **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="7faec-237">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="7faec-238">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="7faec-238">The two browser tabs display the same information.</span></span>

<span data-ttu-id="7faec-239">Zmień nazwę w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="7faec-239">Change the name in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="7faec-241">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="7faec-241">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="7faec-242">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="7faec-242">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="7faec-243">Zmień inne pole w drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7faec-243">Change a different field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="7faec-245">Kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="7faec-245">Click **Save**.</span></span> <span data-ttu-id="7faec-246">Zostaną wyświetlone komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7faec-246">You see error messages for all fields that don't match the DB values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="7faec-248">To okno przeglądarki przypadkowo zmienić pole Nazwa.</span><span class="sxs-lookup"><span data-stu-id="7faec-248">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="7faec-249">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="7faec-249">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="7faec-250">Karta. Weryfikacja po stronie klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7faec-250">Tab out. Client-side validation removes the error message.</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

<span data-ttu-id="7faec-252">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="7faec-252">Click **Save** again.</span></span> <span data-ttu-id="7faec-253">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="7faec-253">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="7faec-254">Zobaczysz zapisane wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="7faec-254">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="7faec-255">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="7faec-255">Update the Delete page</span></span>

<span data-ttu-id="7faec-256">Aktualizowanie modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7faec-256">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="7faec-257">Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="7faec-257">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="7faec-258">`Department.RowVersion` jest to wersja wiersza, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="7faec-258">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="7faec-259">EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="7faec-259">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="7faec-260">Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:</span><span class="sxs-lookup"><span data-stu-id="7faec-260">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="7faec-261">`RowVersion` W SQL DELETE polecenia nie jest zgodna `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-261">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="7faec-262">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="7faec-262">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="7faec-263">`OnGetAsync` jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="7faec-263">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="7faec-264">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="7faec-264">Update the Delete page</span></span>

<span data-ttu-id="7faec-265">Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7faec-265">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="7faec-266">Poprzedni kod znaczników wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="7faec-266">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="7faec-267">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="7faec-267">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="7faec-268">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7faec-268">Adds an error message.</span></span>
* <span data-ttu-id="7faec-269">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="7faec-269">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="7faec-270">Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="7faec-270">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="7faec-271">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="7faec-271">Adds a hidden row version.</span></span> <span data-ttu-id="7faec-272">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="7faec-272">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="7faec-273">Badanie konfliktów współbieżności ze stroną Delete</span><span class="sxs-lookup"><span data-stu-id="7faec-273">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="7faec-274">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="7faec-274">Create a test department.</span></span>

<span data-ttu-id="7faec-275">Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:</span><span class="sxs-lookup"><span data-stu-id="7faec-275">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="7faec-276">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="7faec-276">Run the app and select Departments.</span></span>
* <span data-ttu-id="7faec-277">Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu test i wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="7faec-277">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="7faec-278">Kliknij przycisk **Edytuj** hiperłącze dla działu testu.</span><span class="sxs-lookup"><span data-stu-id="7faec-278">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="7faec-279">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="7faec-279">The two browser tabs display the same information.</span></span>

<span data-ttu-id="7faec-280">Zmień budżetu w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="7faec-280">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="7faec-281">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="7faec-281">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="7faec-282">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="7faec-282">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="7faec-283">Usuń z działu testu z drugiej karcie. Błąd współbieżności jest wyświetlana przy użyciu bieżących wartości z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-283">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="7faec-284">Klikając **Usuń** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.</span><span class="sxs-lookup"><span data-stu-id="7faec-284">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="7faec-285">Zobacz [dziedziczenia](xref:data/ef-mvc/inheritance) na temat sposobu dziedziczą modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7faec-285">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="7faec-286">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7faec-286">Additional resources</span></span>

* [<span data-ttu-id="7faec-287">Tokeny współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="7faec-287">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="7faec-288">Obsługa współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="7faec-288">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="7faec-289">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="7faec-289">Previous</span></span>](xref:data/ef-rp/update-related-data)
