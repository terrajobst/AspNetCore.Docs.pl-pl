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
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="0a0cf-103">Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8</span><span class="sxs-lookup"><span data-stu-id="0a0cf-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="0a0cf-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)i [Jan P Kowalski](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="0a0cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0a0cf-105">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="0a0cf-106">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="0a0cf-106">Concurrency conflicts</span></span>

<span data-ttu-id="0a0cf-107">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-107">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="0a0cf-108">Użytkownik przejdzie do strony edytowania dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-108">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="0a0cf-109">Inny użytkownik aktualizuje tę samą jednostkę przed zapisaniem zmiany pierwszego użytkownika w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-109">Another user updates the same entity before the first user's change is written to the database.</span></span>

<span data-ttu-id="0a0cf-110">Jeśli wykrywanie współbieżności nie jest włączone, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-110">If concurrency detection isn't enabled, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="0a0cf-111">Jeśli to ryzyko jest akceptowalne, koszt programowania dla współbieżności może być korzystny.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-111">If this risk is acceptable, the cost of programming for concurrency might outweigh the benefit.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="0a0cf-112">Współbieżność pesymistyczna (blokowanie)</span><span class="sxs-lookup"><span data-stu-id="0a0cf-112">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="0a0cf-113">Jednym ze sposobów zapobiegania konfliktom współbieżności jest użycie blokad bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-113">One way to prevent concurrency conflicts is to use database locks.</span></span> <span data-ttu-id="0a0cf-114">Jest to nazywane pesymistyczną współbieżnością.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-114">This is called pessimistic concurrency.</span></span> <span data-ttu-id="0a0cf-115">Zanim aplikacja odczyta wiersz bazy danych, który zamierza zaktualizować, żąda blokady.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-115">Before the app reads a database row that it intends to update, it requests a lock.</span></span> <span data-ttu-id="0a0cf-116">Gdy wiersz jest zablokowany na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie może zablokować wiersza do momentu zwolnienia pierwszej blokady.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-116">Once a row is locked for update access, no other users are allowed to lock the row until the first lock is released.</span></span>

<span data-ttu-id="0a0cf-117">Zarządzanie blokadami ma wady.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-117">Managing locks has disadvantages.</span></span> <span data-ttu-id="0a0cf-118">Może być skomplikowany dla programu i może spowodować problemy z wydajnością w miarę zwiększania się liczby użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-118">It can be complex to program and can cause performance problems as the number of users increases.</span></span> <span data-ttu-id="0a0cf-119">Entity Framework Core nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak wdrożyć go.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-119">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="0a0cf-120">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="0a0cf-120">Optimistic concurrency</span></span>

<span data-ttu-id="0a0cf-121">Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-121">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="0a0cf-122">Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-122">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget30.png)

<span data-ttu-id="0a0cf-124">Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-124">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date30.png)

<span data-ttu-id="0a0cf-126">Janina klika pozycję **Zapisz** pierwszy i widzimy, że zmiany zaczęły obowiązywać, ponieważ przeglądarka wyświetla stronę indeksu z zerem jako kwotą budżetu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-126">Jane clicks **Save** first and sees her change take effect, since the browser displays the Index page with zero as the Budget amount.</span></span>

<span data-ttu-id="0a0cf-127">Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-127">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="0a0cf-128">Co się stanie dalej, zależy od sposobu obsługi konfliktów współbieżności:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-128">What happens next is determined by how you handle concurrency conflicts:</span></span>

* <span data-ttu-id="0a0cf-129">Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-129">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

  <span data-ttu-id="0a0cf-130">W tym scenariuszu zostałyby utracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-130">In the scenario, no data would be lost.</span></span> <span data-ttu-id="0a0cf-131">Inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-131">Different properties were updated by the two users.</span></span> <span data-ttu-id="0a0cf-132">Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-132">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="0a0cf-133">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-133">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="0a0cf-134">Takie podejście ma pewne wady:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-134">This approach has some disadvantages:</span></span>
 
  * <span data-ttu-id="0a0cf-135">Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-135">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="0a0cf-136">Zwykle nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-136">Is generally not practical in a web app.</span></span> <span data-ttu-id="0a0cf-137">Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-137">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="0a0cf-138">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-138">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="0a0cf-139">Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-139">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="0a0cf-140">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-140">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="0a0cf-141">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-141">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="0a0cf-142">Ta metoda jest nazywana *klientem WINS* lub *ostatnim scenariuszem usługi WINS* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-142">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="0a0cf-143">(Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-143">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="0a0cf-144">Można zapobiec aktualizacji firmy Jan ze zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-144">You can prevent John's change from being updated in the database.</span></span> <span data-ttu-id="0a0cf-145">Zazwyczaj aplikacja będzie:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-145">Typically, the app would:</span></span>

  * <span data-ttu-id="0a0cf-146">Wyświetl komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-146">Display an error message.</span></span>
  * <span data-ttu-id="0a0cf-147">Umożliwia wyświetlenie bieżącego stanu danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-147">Show the current state of the data.</span></span>
  * <span data-ttu-id="0a0cf-148">Zezwalaj użytkownikowi ponownie zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-148">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="0a0cf-149">Jest to tzw. scenariusz *magazynu usługi WINS* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-149">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="0a0cf-150">(Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-150">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="0a0cf-151">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-151">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="conflict-detection-in-ef-core"></a><span data-ttu-id="0a0cf-152">Wykrywanie konfliktów w EF Core</span><span class="sxs-lookup"><span data-stu-id="0a0cf-152">Conflict detection in EF Core</span></span>

<span data-ttu-id="0a0cf-153">EF Core generuje wyjątki `DbConcurrencyException` w przypadku wykrycia konfliktów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-153">EF Core throws `DbConcurrencyException` exceptions when it detects conflicts.</span></span> <span data-ttu-id="0a0cf-154">Model danych musi być skonfigurowany, aby umożliwić wykrywanie konfliktów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-154">The data model has to be configured to enable conflict detection.</span></span> <span data-ttu-id="0a0cf-155">Opcje włączania wykrywania konfliktów obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-155">Options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="0a0cf-156">Skonfiguruj EF Core, aby uwzględnić oryginalne wartości kolumn skonfigurowanych jako [tokeny współbieżności](/ef/core/modeling/concurrency) w klauzuli WHERE poleceń Update i DELETE.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-156">Configure EF Core to include the original values of columns configured as [concurrency tokens](/ef/core/modeling/concurrency) in the Where clause of Update and Delete commands.</span></span>

  <span data-ttu-id="0a0cf-157">Gdy `SaveChanges` jest wywoływana, klauzula WHERE szuka oryginalnych wartości wszelkich właściwości, które mają adnotację z atrybutem [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-157">When `SaveChanges` is called, the Where clause looks for the original values of any properties annotated with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) attribute.</span></span> <span data-ttu-id="0a0cf-158">Instrukcja Update nie znajdzie wiersza do zaktualizowania, jeśli którykolwiek z właściwości tokenu współbieżności został zmieniony od momentu pierwszego odczytu wiersza.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-158">The update statement won't find a row to update if any of the concurrency token properties changed since the row was first read.</span></span> <span data-ttu-id="0a0cf-159">EF Core interpretuje ten sposób jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-159">EF Core interprets that as a concurrency conflict.</span></span> <span data-ttu-id="0a0cf-160">W przypadku tabel bazy danych z wieloma kolumnami takie podejście może skutkować bardzo dużymi klauzulami WHERE i może wymagać dużej ilości danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-160">For database tables that have many columns, this approach can result in very large Where clauses, and can require large amounts of state.</span></span> <span data-ttu-id="0a0cf-161">W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-161">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

* <span data-ttu-id="0a0cf-162">W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span>

  <span data-ttu-id="0a0cf-163">W bazie danych SQL Server typ danych kolumny śledzenia jest `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-163">In a SQL Server database, the data type of the tracking column is `rowversion`.</span></span> <span data-ttu-id="0a0cf-164">Wartość `rowversion` jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-164">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="0a0cf-165">W przypadku polecenia Update lub DELETE klauzula WHERE zawiera oryginalną wartość kolumny śledzenia (numer wersji pierwszego wiersza).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-165">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version number).</span></span> <span data-ttu-id="0a0cf-166">Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-166">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value.</span></span> <span data-ttu-id="0a0cf-167">W takim przypadku instrukcja UPDATE lub DELETE nie może znaleźć wiersza do zaktualizowania z powodu klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-167">In that case, the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="0a0cf-168">EF Core zgłasza wyjątek współbieżności, gdy polecenie Update lub DELETE nie ma na nich żadnych wierszy.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-168">EF Core throws a concurrency exception when no rows are affected by an Update or Delete command.</span></span>

## <a name="add-a-tracking-property"></a><span data-ttu-id="0a0cf-169">Dodaj właściwość śledzenia</span><span class="sxs-lookup"><span data-stu-id="0a0cf-169">Add a tracking property</span></span>

<span data-ttu-id="0a0cf-170">W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-170">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

<span data-ttu-id="0a0cf-171">Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) wskazuje, co identyfikuje kolumnę jako kolumnę śledzenia współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-171">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute is what identifies the column as a concurrency tracking column.</span></span> <span data-ttu-id="0a0cf-172">Interfejs API Fluent jest alternatywnym sposobem określenia właściwości śledzenia:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-172">The fluent API is an alternative way to specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studio"></a>[<span data-ttu-id="0a0cf-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a0cf-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a0cf-174">W przypadku bazy danych SQL Server atrybut `[Timestamp]` właściwości Entity zdefiniowany jako tablica bajtowa:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-174">For a SQL Server database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="0a0cf-175">Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-175">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="0a0cf-176">Ustawia typ kolumny w bazie danych na [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-176">Sets the column type in the database to [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span></span>

<span data-ttu-id="0a0cf-177">Baza danych generuje sekwencyjny numer wersji wiersza, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-177">The database generates a sequential row version number that's incremented each time the row is updated.</span></span> <span data-ttu-id="0a0cf-178">W `Update` lub `Delete`, klauzula `Where` zawiera wartość wersji wiersza do pobrania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-178">In an `Update` or `Delete` command, the `Where` clause includes the fetched row version value.</span></span> <span data-ttu-id="0a0cf-179">Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-179">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="0a0cf-180">Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-180">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="0a0cf-181">Polecenia `Update` lub `Delete` nie znalazły wiersza, ponieważ klauzula `Where` szuka wartości wersji wiersza pobrania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-181">The `Update` or `Delete` commands don't find a row because the `Where` clause looks for the fetched row version value.</span></span>
* <span data-ttu-id="0a0cf-182">Zostanie zgłoszony `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-182">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="0a0cf-183">Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-183">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

<span data-ttu-id="0a0cf-184">Poprzedni wyróżniony kod pokazuje klauzulę `WHERE` zawierającą `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-184">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="0a0cf-185">Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-185">If the database `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="0a0cf-186">Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-186">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

<span data-ttu-id="0a0cf-187">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy, na które miało wpływ Ostatnia instrukcja.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-187">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="0a0cf-188">Jeśli żadne wiersze nie są aktualizowane, EF Core zgłasza `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-188">If no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="0a0cf-189">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a0cf-189">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0a0cf-190">W przypadku bazy danych programu SQLite atrybut `[Timestamp]` właściwości Entity został zdefiniowany jako tablica bajtowa:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-190">For a SQLite database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="0a0cf-191">Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-191">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="0a0cf-192">Mapuje do typu kolumny obiektu BLOB.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-192">Maps to a BLOB column type.</span></span>

<span data-ttu-id="0a0cf-193">Wyzwalacze bazy danych aktualizują kolumnę RowVersion za pomocą nowej losowej tablicy bajtów za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-193">Database triggers update the RowVersion column with a new random byte array whenever a row is updated.</span></span> <span data-ttu-id="0a0cf-194">W `Update` lub `Delete`, klauzula `Where` zawiera pobraną wartość kolumny RowVersion.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-194">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of the RowVersion column.</span></span> <span data-ttu-id="0a0cf-195">Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-195">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="0a0cf-196">Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-196">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="0a0cf-197">Polecenie `Update` lub `Delete` nie znajduje wiersza, ponieważ klauzula `Where` szuka pierwotnej wartości wersji wiersza.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-197">The `Update` or `Delete` command doesn't find a row because the `Where` clause looks for the original row version value.</span></span>
* <span data-ttu-id="0a0cf-198">Zostanie zgłoszony `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-198">A `DbUpdateConcurrencyException` is thrown.</span></span>

---

### <a name="update-the-database"></a><span data-ttu-id="0a0cf-199">Aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="0a0cf-199">Update the database</span></span>

<span data-ttu-id="0a0cf-200">Dodanie właściwości `RowVersion` powoduje zmianę modelu danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-200">Adding the `RowVersion` property changes the data model, which requires a migration.</span></span>

<span data-ttu-id="0a0cf-201">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-201">Build the project.</span></span> 

# <a name="visual-studio"></a>[<span data-ttu-id="0a0cf-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a0cf-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a0cf-203">Uruchom następujące polecenie w obszarze PMC:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-203">Run the following command in the PMC:</span></span>

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="0a0cf-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a0cf-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a0cf-205">Uruchom następujące polecenie w terminalu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-205">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

<span data-ttu-id="0a0cf-206">To polecenie:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-206">This command:</span></span>

* <span data-ttu-id="0a0cf-207">Tworzy plik migracji *_RowVersion. cs migracji/sygnatur czasowych}* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-207">Creates the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="0a0cf-208">Aktualizuje plik *migrations/SchoolContextModelSnapshot. cs* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-208">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="0a0cf-209">Aktualizacja dodaje następujący wyróżniony kod do metody `BuildModel`:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-209">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studio"></a>[<span data-ttu-id="0a0cf-210">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a0cf-210">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a0cf-211">Uruchom następujące polecenie w obszarze PMC:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-211">Run the following command in the PMC:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="0a0cf-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a0cf-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a0cf-213">Otwórz plik `Migrations/<timestamp>_RowVersion.cs` i Dodaj wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-213">Open the `Migrations/<timestamp>_RowVersion.cs` file and add the highlighted code:</span></span>

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  <span data-ttu-id="0a0cf-214">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-214">The preceding code:</span></span>

  * <span data-ttu-id="0a0cf-215">Aktualizuje istniejące wiersze z losowymi wartościami obiektów BLOB.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-215">Updates existing rows with random blob values.</span></span>
  * <span data-ttu-id="0a0cf-216">Dodaje wyzwalacze bazy danych, które ustawiają kolumnę RowVersion na losową wartość obiektu BLOB za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-216">Adds database triggers that set the RowVersion column to a random blob value whenever a row is updated.</span></span>

* <span data-ttu-id="0a0cf-217">Uruchom następujące polecenie w terminalu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-217">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a><span data-ttu-id="0a0cf-218">Strony działu szkieletu</span><span class="sxs-lookup"><span data-stu-id="0a0cf-218">Scaffold Department pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0a0cf-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a0cf-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a0cf-220">Postępuj zgodnie z instrukcjami na [stronach uczniów tworzenia szkieletów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-220">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

* <span data-ttu-id="0a0cf-221">Utwórz folder *strony/działy* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-221">Create a *Pages/Departments* folder.</span></span>  
* <span data-ttu-id="0a0cf-222">Użyj `Department` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-222">Use `Department` for the model class.</span></span>
  * <span data-ttu-id="0a0cf-223">Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-223">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="0a0cf-224">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a0cf-224">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a0cf-225">Utwórz folder *strony/działy* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-225">Create a *Pages/Departments* folder.</span></span>

* <span data-ttu-id="0a0cf-226">Uruchom następujące polecenie, aby uzyskać szkielet na stronach działu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-226">Run the following command to scaffold the Department pages.</span></span>

  <span data-ttu-id="0a0cf-227">**W systemie Windows:**</span><span class="sxs-lookup"><span data-stu-id="0a0cf-227">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  <span data-ttu-id="0a0cf-228">**W systemie Linux lub macOS:**</span><span class="sxs-lookup"><span data-stu-id="0a0cf-228">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="0a0cf-229">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-229">Build the project.</span></span>

## <a name="update-the-index-page"></a><span data-ttu-id="0a0cf-230">Aktualizowanie strony indeksu</span><span class="sxs-lookup"><span data-stu-id="0a0cf-230">Update the Index page</span></span>

<span data-ttu-id="0a0cf-231">Narzędzie tworzenia szkieletów utworzyło kolumnę `RowVersion` dla strony indeks, ale to pole nie będzie wyświetlane w aplikacji produkcyjnej.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-231">The scaffolding tool created a `RowVersion` column for the Index page, but that field wouldn't be displayed in a production app.</span></span> <span data-ttu-id="0a0cf-232">W tym samouczku zostanie wyświetlony ostatni bajt `RowVersion`, aby zobaczyć, jak działa obsługa współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-232">In this tutorial, the last byte of the `RowVersion` is displayed to help show how concurrency handling works.</span></span> <span data-ttu-id="0a0cf-233">Ostatni bajt nie gwarantuje, że jest unikatowy.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-233">The last byte isn't guaranteed to be unique by itself.</span></span>

<span data-ttu-id="0a0cf-234">Strona aktualizacji *Pages\Departments\Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="0a0cf-234">Update *Pages\Departments\Index.cshtml* page:</span></span>

* <span data-ttu-id="0a0cf-235">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-235">Replace Index with Departments.</span></span>
* <span data-ttu-id="0a0cf-236">Zmień kod zawierający `RowVersion`, aby pokazać tylko ostatni bajt tablicy bajtów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-236">Change the code containing `RowVersion` to show just the last byte of the byte array.</span></span>
* <span data-ttu-id="0a0cf-237">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-237">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="0a0cf-238">Poniższy kod przedstawia zaktualizowaną stronę:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-238">The following code shows the updated page:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a><span data-ttu-id="0a0cf-239">Aktualizowanie modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="0a0cf-239">Update the Edit page model</span></span>

<span data-ttu-id="0a0cf-240">Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-240">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

<span data-ttu-id="0a0cf-241">[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) jest aktualizowana przy użyciu wartości `rowVersion` z jednostki, gdy została ona pobrana w metodzie `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-241">The [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity when it was fetched in the `OnGet` method.</span></span> <span data-ttu-id="0a0cf-242">EF Core generuje polecenie SQL UPDATE z klauzulą WHERE zawierającą oryginalną wartość `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-242">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="0a0cf-243">Jeśli nie ma żadnych wierszy, na które ma wpływ polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), zostanie zgłoszony wyjątek `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-243">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

<span data-ttu-id="0a0cf-244">W poprzednim wyróżnionym kodzie:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-244">In the preceding highlighted code:</span></span>

* <span data-ttu-id="0a0cf-245">Wartość w `Department.RowVersion` to jaka znajdowała się w jednostce, gdy została pierwotnie pobrana w żądaniu pobrania dla strony edycji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-245">The value in `Department.RowVersion` is what was in the entity when it was originally fetched in the Get request for the Edit page.</span></span> <span data-ttu-id="0a0cf-246">Wartość jest dostarczana do metody `OnPost` przez ukryte pole na stronie Razor, które wyświetla jednostkę do edycji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-246">The value is provided to the `OnPost` method by a hidden field in the Razor page that displays the entity to be edited.</span></span> <span data-ttu-id="0a0cf-247">Wartość pola ukrytego jest kopiowana do `Department.RowVersion` przez spinacz modelu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-247">The hidden field value is copied to `Department.RowVersion` by the model binder.</span></span>
* <span data-ttu-id="0a0cf-248">`OriginalValue` jest tym, czego EF Core będzie używać w klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-248">`OriginalValue` is what EF Core will use in the Where clause.</span></span> <span data-ttu-id="0a0cf-249">Przed wykonaniem wyróżnionego wiersza kodu `OriginalValue` ma wartość znajdującą się w bazie danych, gdy `FirstOrDefaultAsync` została wywołana w tej metodzie, która może się różnić od tego, co zostało wyświetlone na stronie Edycja.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-249">Before the highlighted line of code executes, `OriginalValue` has the value that was in the database when `FirstOrDefaultAsync` was called in this method, which might be different from what was displayed on the Edit page.</span></span>
* <span data-ttu-id="0a0cf-250">Wyróżniony kod sprawdza, czy EF Core używa oryginalnej wartości `RowVersion` z wyświetlonej jednostki `Department` w klauzuli WHERE instrukcji SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-250">The highlighted code makes sure that EF Core uses the original `RowVersion` value from the displayed `Department` entity in the SQL UPDATE statement's Where clause.</span></span>

<span data-ttu-id="0a0cf-251">Gdy wystąpi błąd współbieżności, poniższy wyróżniony kod pobiera wartości klienta (wartości ogłoszone w tej metodzie) oraz wartości bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-251">When a concurrency error happens, the following highlighted code gets the client values (the values posted to this method) and the database values.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

<span data-ttu-id="0a0cf-252">Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-252">The following code adds a custom error message for each column that has database values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

<span data-ttu-id="0a0cf-253">Poniższy wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-253">The following highlighted code sets the `RowVersion` value to the new value retrieved from the database.</span></span> <span data-ttu-id="0a0cf-254">Następnym razem, gdy użytkownik kliknie przycisk **Zapisz**, zostanie przechwycony tylko błąd współbieżności występujący od momentu ostatniego wyświetlenia strony edycji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-254">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

<span data-ttu-id="0a0cf-255">Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-255">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="0a0cf-256">Na stronie Razor wartość `ModelState` pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-256">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

### <a name="update-the-razor-page"></a><span data-ttu-id="0a0cf-257">Aktualizowanie strony Razor</span><span class="sxs-lookup"><span data-stu-id="0a0cf-257">Update the Razor page</span></span>

<span data-ttu-id="0a0cf-258">Zaktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-258">Update *Pages/Departments/Edit.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="0a0cf-259">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-259">The preceding code:</span></span>

* <span data-ttu-id="0a0cf-260">Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-260">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="0a0cf-261">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-261">Adds a hidden row version.</span></span> <span data-ttu-id="0a0cf-262">należy dodać `RowVersion` tak, aby zwroty z powrotem powiązać wartość.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-262">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="0a0cf-263">Wyświetla ostatni bajt `RowVersion` do celów debugowania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-263">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="0a0cf-264">Zastępuje `ViewData` ze silnie wpisaną `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-264">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

### <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="0a0cf-265">Badanie konfliktów współbieżności przy użyciu strony edytowania</span><span class="sxs-lookup"><span data-stu-id="0a0cf-265">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="0a0cf-266">Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-266">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="0a0cf-267">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-267">Run the app and select Departments.</span></span>
* <span data-ttu-id="0a0cf-268">Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-268">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="0a0cf-269">Na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu w języku angielskim.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-269">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="0a0cf-270">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-270">The two browser tabs display the same information.</span></span>

<span data-ttu-id="0a0cf-271">Zmień nazwę na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-271">Change the name in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-130.png)

<span data-ttu-id="0a0cf-273">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-273">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="0a0cf-274">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-274">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="0a0cf-275">Zmień inne pole w drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-275">Change a different field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-230.png)

<span data-ttu-id="0a0cf-277">Kliknij przycisk **Save** (Zapisz).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-277">Click **Save**.</span></span> <span data-ttu-id="0a0cf-278">Komunikaty o błędach są wyświetlane dla wszystkich pól, które nie pasują do wartości bazy danych:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-278">You see error messages for all fields that don't match the database values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error30.png)

<span data-ttu-id="0a0cf-280">To okno przeglądarki przypadkowo zmienić pole Nazwa.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-280">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="0a0cf-281">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-281">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="0a0cf-282">Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-282">Tab out. Client-side validation removes the error message.</span></span>

<span data-ttu-id="0a0cf-283">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-283">Click **Save** again.</span></span> <span data-ttu-id="0a0cf-284">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-284">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="0a0cf-285">Zobaczysz zapisane wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-285">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0a0cf-286">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="0a0cf-286">Update the Delete page</span></span>

<span data-ttu-id="0a0cf-287">Zaktualizuj *strony/działy/Delete. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-287">Update *Pages/Departments/Delete.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="0a0cf-288">Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-288">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="0a0cf-289">`Department.RowVersion` to wersja wiersza, kiedy jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-289">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="0a0cf-290">Gdy EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-290">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="0a0cf-291">Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-291">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="0a0cf-292">`RowVersion` w poleceniu SQL DELETE nie jest zgodny `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-292">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the database.</span></span>
* <span data-ttu-id="0a0cf-293">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-293">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="0a0cf-294">`OnGetAsync` jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-294">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="0a0cf-295">Aktualizowanie strony usuwania Razor</span><span class="sxs-lookup"><span data-stu-id="0a0cf-295">Update the Delete Razor page</span></span>

<span data-ttu-id="0a0cf-296">Zaktualizuj *strony/działy/Delete. cshtml* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-296">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="0a0cf-297">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-297">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="0a0cf-298">Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-298">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="0a0cf-299">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-299">Adds an error message.</span></span>
* <span data-ttu-id="0a0cf-300">Zastępuje FirstMidName z FullName w polu **administrator** .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-300">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="0a0cf-301">Zmienia `RowVersion` w celu wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-301">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="0a0cf-302">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-302">Adds a hidden row version.</span></span> <span data-ttu-id="0a0cf-303">należy dodać `RowVersion`, aby postgit dodać ponownie powiązania wartości.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-303">`RowVersion` must be added so postgit add back binds the value.</span></span>

### <a name="test-concurrency-conflicts"></a><span data-ttu-id="0a0cf-304">Testuj konflikty współbieżności</span><span class="sxs-lookup"><span data-stu-id="0a0cf-304">Test concurrency conflicts</span></span>

<span data-ttu-id="0a0cf-305">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-305">Create a test department.</span></span>

<span data-ttu-id="0a0cf-306">Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-306">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="0a0cf-307">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-307">Run the app and select Departments.</span></span>
* <span data-ttu-id="0a0cf-308">Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu testowego i wybierz polecenie **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-308">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="0a0cf-309">Kliknij hiperłącze **Edytuj** dla działu testowego.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-309">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="0a0cf-310">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-310">The two browser tabs display the same information.</span></span>

<span data-ttu-id="0a0cf-311">Zmień budżet na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-311">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="0a0cf-312">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-312">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="0a0cf-313">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-313">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="0a0cf-314">Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-314">Delete the test department from the second tab. A concurrency error is display with the current values from the database.</span></span> <span data-ttu-id="0a0cf-315">Kliknięcie przycisku **Usuń** powoduje usunięcie jednostki, o ile `RowVersion` nie została zaktualizowana. dział został usunięty.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-315">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a0cf-316">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0a0cf-316">Additional resources</span></span>

* [<span data-ttu-id="0a0cf-317">Tokeny współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="0a0cf-317">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="0a0cf-318">Obsługa współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="0a0cf-318">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="0a0cf-319">Debugowanie ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="0a0cf-319">Debugging ASP.NET Core 2.x source</span></span>](https://github.com/dotnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a><span data-ttu-id="0a0cf-320">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="0a0cf-320">Next steps</span></span>

<span data-ttu-id="0a0cf-321">Jest to ostatni samouczek z serii.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-321">This is the last tutorial in the series.</span></span> <span data-ttu-id="0a0cf-322">Dodatkowe tematy zostały omówione w [wersji MVC tej serii samouczków](xref:data/ef-mvc/index).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-322">Additional topics are covered in the [MVC version of this tutorial series](xref:data/ef-mvc/index).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0a0cf-323">Poprzedni samouczek</span><span class="sxs-lookup"><span data-stu-id="0a0cf-323">Previous tutorial</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0a0cf-324">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-324">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="0a0cf-325">Jeśli występują problemy, których nie można rozwiązać, [Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="0a0cf-325">If you run into problems you can't solve, [download or view the completed app.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="0a0cf-326">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-326">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="0a0cf-327">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="0a0cf-327">Concurrency conflicts</span></span>

<span data-ttu-id="0a0cf-328">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-328">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="0a0cf-329">Użytkownik przejdzie do strony edytowania dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-329">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="0a0cf-330">Inny użytkownik aktualizuje tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-330">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="0a0cf-331">Jeśli wykrycie współbieżności nie jest włączone, gdy wystąpią równoczesne aktualizacje:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-331">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="0a0cf-332">Ostatnia aktualizacja usługi wins.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-332">The last update wins.</span></span> <span data-ttu-id="0a0cf-333">Oznacza to, że ostatnie wartości aktualizacji są zapisywane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-333">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="0a0cf-334">Pierwszego dnia bieżące aktualizacje zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-334">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="0a0cf-335">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="0a0cf-335">Optimistic concurrency</span></span>

<span data-ttu-id="0a0cf-336">Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-336">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="0a0cf-337">Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-337">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="0a0cf-339">Przed Janem kliknie przycisk **Zapisz**, Jan odwiedzi tę samą stronę i zmieni pole Data rozpoczęcia z 9/1/2007 na 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-339">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

<span data-ttu-id="0a0cf-341">Jan klika pozycję **Zapisz** jako pierwszy i widzi zmiany, gdy przeglądarka wyświetli stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-341">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budżet na zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="0a0cf-343">Jan klika pozycję **Zapisz** na stronie edytowania, która nadal zawiera budżet $350 000,00.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-343">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="0a0cf-344">Co dzieje się potem określają sposób obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-344">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="0a0cf-345">Optymistyczna współbieżność obejmuje następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-345">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="0a0cf-346">Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-346">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="0a0cf-347">W tym scenariuszu zostałyby utracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-347">In the scenario, no data would be lost.</span></span> <span data-ttu-id="0a0cf-348">Inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-348">Different properties were updated by the two users.</span></span> <span data-ttu-id="0a0cf-349">Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-349">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="0a0cf-350">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-350">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="0a0cf-351">To podejście:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-351">This approach:</span></span>
 
  * <span data-ttu-id="0a0cf-352">Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-352">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="0a0cf-353">Zwykle nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-353">Is generally not practical in a web app.</span></span> <span data-ttu-id="0a0cf-354">Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-354">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="0a0cf-355">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-355">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="0a0cf-356">Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-356">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="0a0cf-357">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-357">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="0a0cf-358">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-358">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="0a0cf-359">Ta metoda jest nazywana *klientem WINS* lub *ostatnim scenariuszem usługi WINS* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-359">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="0a0cf-360">(Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-360">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="0a0cf-361">Aby uniemożliwić zmiany John's aktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-361">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="0a0cf-362">Zazwyczaj aplikacja będzie:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-362">Typically, the app would:</span></span>

  * <span data-ttu-id="0a0cf-363">Wyświetl komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-363">Display an error message.</span></span>
  * <span data-ttu-id="0a0cf-364">Umożliwia wyświetlenie bieżącego stanu danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-364">Show the current state of the data.</span></span>
  * <span data-ttu-id="0a0cf-365">Zezwalaj użytkownikowi ponownie zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-365">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="0a0cf-366">Jest to tzw. scenariusz *magazynu usługi WINS* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-366">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="0a0cf-367">(Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-367">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="0a0cf-368">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-368">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="0a0cf-369">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="0a0cf-369">Handling concurrency</span></span> 

<span data-ttu-id="0a0cf-370">Gdy właściwość jest skonfigurowana jako [Token współbieżności](/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="0a0cf-370">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="0a0cf-371">EF Core sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-371">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="0a0cf-372">Sprawdzanie występuje, gdy wywoływana jest [metody SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-372">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="0a0cf-373">Jeśli właściwość została zmieniona po pobraniu, zgłaszany jest [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-373">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="0a0cf-374">Baza danych i model danych muszą być skonfigurowane do obsługi zgłaszania `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-374">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="0a0cf-375">Wykrywanie konfliktów współbieżności we właściwości</span><span class="sxs-lookup"><span data-stu-id="0a0cf-375">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="0a0cf-376">Konflikty współbieżności mogą być wykrywane na poziomie właściwości przy użyciu atrybutu [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-376">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="0a0cf-377">Ten atrybut można zastosować na wiele właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-377">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="0a0cf-378">Aby uzyskać więcej informacji, zobacz [Adnotacje danych — ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-378">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="0a0cf-379">Atrybut `[ConcurrencyCheck]` nie jest używany w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-379">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="0a0cf-380">Wykrywanie konfliktów współbieżności wiersz</span><span class="sxs-lookup"><span data-stu-id="0a0cf-380">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="0a0cf-381">Aby wykrywać konflikty współbieżności, do modelu dodawana jest kolumna śledzenia [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-381">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="0a0cf-382">`rowversion`:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-382">`rowversion` :</span></span>

* <span data-ttu-id="0a0cf-383">Dotyczy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-383">Is SQL Server specific.</span></span> <span data-ttu-id="0a0cf-384">Inne bazy danych nie mogą zawierać podobnych funkcji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-384">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="0a0cf-385">Służy do określenia, czy jednostka nie została zmieniona, ponieważ została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-385">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="0a0cf-386">Baza danych generuje numer sekwencyjny `rowversion`, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-386">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="0a0cf-387">W `Update` lub `Delete`, klauzula `Where` zawiera pobraną wartość `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-387">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="0a0cf-388">Jeśli zmieniono aktualizacji wiersza:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-388">If the row being updated has changed:</span></span>

* <span data-ttu-id="0a0cf-389">`rowversion` nie jest zgodna z pobraną wartością.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-389">`rowversion` doesn't match the fetched value.</span></span>
* <span data-ttu-id="0a0cf-390">Polecenia `Update` lub `Delete` nie znalazły wiersza, ponieważ klauzula `Where` zawiera pobrane `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-390">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
* <span data-ttu-id="0a0cf-391">Zostanie zgłoszony `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-391">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="0a0cf-392">W EF Core, gdy żadne wiersze nie zostały zaktualizowane przez polecenie `Update` lub `Delete`, zostanie zgłoszony wyjątek współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-392">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="0a0cf-393">Dodawanie właściwości śledzenia do jednostki działu</span><span class="sxs-lookup"><span data-stu-id="0a0cf-393">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="0a0cf-394">W obszarze *modele/dział. cs*Dodaj właściwość śledzenia o nazwie rowversion:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-394">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="0a0cf-395">Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) określa, że ta kolumna jest uwzględniona w klauzuli `Where` poleceń `Update` i `Delete`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-395">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="0a0cf-396">Ten atrybut jest nazywany `Timestamp`, ponieważ poprzednie wersje SQL Server używały typu danych SQL `timestamp` przed zastąpieniem typu `rowversion` SQL.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-396">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="0a0cf-397">Interfejs fluent API można również określić właściwości śledzenia:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-397">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="0a0cf-398">Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-398">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

<span data-ttu-id="0a0cf-399">Poprzedni wyróżniony kod pokazuje klauzulę `WHERE` zawierającą `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-399">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="0a0cf-400">Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-400">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="0a0cf-401">Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-401">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

<span data-ttu-id="0a0cf-402">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy, na które miało wpływ Ostatnia instrukcja.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-402">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="0a0cf-403">Nie zaktualizowano żadnych wierszy, EF Core zgłasza `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-403">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="0a0cf-404">Widać, że T-SQL programu EF Core generuje w oknie danych wyjściowych programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-404">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="0a0cf-405">Aktualizacja bazy danych</span><span class="sxs-lookup"><span data-stu-id="0a0cf-405">Update the DB</span></span>

<span data-ttu-id="0a0cf-406">Dodanie właściwości `RowVersion` powoduje zmianę modelu bazy danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-406">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="0a0cf-407">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-407">Build the project.</span></span> <span data-ttu-id="0a0cf-408">W oknie polecenia, należy wprowadzić następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-408">Enter the following in a command window:</span></span>

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="0a0cf-409">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-409">The preceding commands:</span></span>

* <span data-ttu-id="0a0cf-410">Dodaje *migrację/{Time sygnaturę} _RowVersion. cs* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-410">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="0a0cf-411">Aktualizuje plik *migrations/SchoolContextModelSnapshot. cs* .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-411">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="0a0cf-412">Aktualizacja dodaje następujący wyróżniony kod do metody `BuildModel`:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-412">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* <span data-ttu-id="0a0cf-413">Uruchamia migracji można zaktualizować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-413">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a><span data-ttu-id="0a0cf-414">Tworzenie szkieletu modelu działów</span><span class="sxs-lookup"><span data-stu-id="0a0cf-414">Scaffold the Departments model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0a0cf-415">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a0cf-415">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0a0cf-416">Postępuj zgodnie z instrukcjami w obszarze [szkieletem model studenta](xref:data/ef-rp/intro#scaffold-student-pages) i użyj `Department` dla klasy model.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-416">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Department` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="0a0cf-417">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a0cf-417">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="0a0cf-418">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-418">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="0a0cf-419">Poprzednie polecenie szkieletuje model `Department`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-419">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="0a0cf-420">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-420">Open the project in Visual Studio.</span></span>

<span data-ttu-id="0a0cf-421">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-421">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="0a0cf-422">Zaktualizuj strony działy indeksu</span><span class="sxs-lookup"><span data-stu-id="0a0cf-422">Update the Departments Index page</span></span>

<span data-ttu-id="0a0cf-423">Aparat tworzenia szkieletów utworzył kolumnę `RowVersion` dla strony index, ale to pole nie powinno być wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-423">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="0a0cf-424">W tym samouczku zostanie wyświetlony ostatni bajt `RowVersion`, aby ułatwić zrozumienie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-424">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="0a0cf-425">Ostatni bajt nie musi być unikatowy.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-425">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="0a0cf-426">Rzeczywista aplikacja nie zostanie wyświetlona `RowVersion` lub ostatniego bajtu `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-426">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="0a0cf-427">Zaktualizuj strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-427">Update the Index page:</span></span>

* <span data-ttu-id="0a0cf-428">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-428">Replace Index with Departments.</span></span>
* <span data-ttu-id="0a0cf-429">Zastąp znaczniki zawierające `RowVersion` z ostatnim bajtem `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-429">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="0a0cf-430">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-430">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="0a0cf-431">Następujący kod przedstawia zaktualizowaną stronę:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-431">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="0a0cf-432">Aktualizowanie modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="0a0cf-432">Update the Edit page model</span></span>

<span data-ttu-id="0a0cf-433">Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-433">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="0a0cf-434">Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) zostaje zaktualizowany przy użyciu wartości `rowVersion` z jednostki, która została pobrana.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-434">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="0a0cf-435">EF Core generuje polecenie SQL UPDATE z klauzulą WHERE zawierającą oryginalną wartość `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-435">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="0a0cf-436">Jeśli nie ma żadnych wierszy, na które ma wpływ polecenie aktualizacji (żadne wiersze nie mają oryginalnej wartości `RowVersion`), zostanie zgłoszony wyjątek `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-436">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="0a0cf-437">W poprzednim kodzie `Department.RowVersion` jest wartością, gdy jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-437">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="0a0cf-438">`OriginalValue` jest wartością w bazie danych, gdy `FirstOrDefaultAsync` została wywołana w tej metodzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-438">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="0a0cf-439">Poniższy kod umożliwia pobranie ustawienia klienta (wartości do tej metody) oraz bazy danych wartości:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-439">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="0a0cf-440">Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-440">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="0a0cf-441">Poniższy wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-441">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="0a0cf-442">Następnym razem, gdy użytkownik kliknie przycisk **Zapisz**, zostanie przechwycony tylko błąd współbieżności występujący od momentu ostatniego wyświetlenia strony edycji.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-442">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="0a0cf-443">Instrukcja `ModelState.Remove` jest wymagana, ponieważ `ModelState` ma starą wartość `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-443">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="0a0cf-444">Na stronie Razor wartość `ModelState` pola ma pierwszeństwo przed wartościami właściwości modelu, gdy są obecne oba typy.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-444">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="0a0cf-445">Zaktualizuj strony edytowania</span><span class="sxs-lookup"><span data-stu-id="0a0cf-445">Update the Edit page</span></span>

<span data-ttu-id="0a0cf-446">Aktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego znacznika:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-446">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="0a0cf-447">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-447">The preceding markup:</span></span>

* <span data-ttu-id="0a0cf-448">Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-448">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="0a0cf-449">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-449">Adds a hidden row version.</span></span> <span data-ttu-id="0a0cf-450">należy dodać `RowVersion` tak, aby zwroty z powrotem powiązać wartość.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-450">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="0a0cf-451">Wyświetla ostatni bajt `RowVersion` do celów debugowania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-451">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="0a0cf-452">Zastępuje `ViewData` ze silnie wpisaną `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-452">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="0a0cf-453">Badanie konfliktów współbieżności przy użyciu strony edytowania</span><span class="sxs-lookup"><span data-stu-id="0a0cf-453">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="0a0cf-454">Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-454">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="0a0cf-455">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-455">Run the app and select Departments.</span></span>
* <span data-ttu-id="0a0cf-456">Kliknij prawym przyciskiem myszy hiperłącze **Edytuj** dla działu angielskiego i wybierz polecenie **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-456">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="0a0cf-457">Na pierwszej karcie kliknij hiperłącze **Edytuj** dla działu w języku angielskim.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-457">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="0a0cf-458">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-458">The two browser tabs display the same information.</span></span>

<span data-ttu-id="0a0cf-459">Zmień nazwę na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-459">Change the name in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="0a0cf-461">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-461">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="0a0cf-462">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-462">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="0a0cf-463">Zmień inne pole w drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-463">Change a different field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="0a0cf-465">Kliknij przycisk **Save** (Zapisz).</span><span class="sxs-lookup"><span data-stu-id="0a0cf-465">Click **Save**.</span></span> <span data-ttu-id="0a0cf-466">Zostaną wyświetlone komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-466">You see error messages for all fields that don't match the DB values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="0a0cf-468">To okno przeglądarki przypadkowo zmienić pole Nazwa.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-468">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="0a0cf-469">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-469">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="0a0cf-470">Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-470">Tab out. Client-side validation removes the error message.</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

<span data-ttu-id="0a0cf-472">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-472">Click **Save** again.</span></span> <span data-ttu-id="0a0cf-473">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-473">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="0a0cf-474">Zobaczysz zapisane wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-474">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0a0cf-475">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="0a0cf-475">Update the Delete page</span></span>

<span data-ttu-id="0a0cf-476">Aktualizowanie modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-476">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="0a0cf-477">Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-477">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="0a0cf-478">`Department.RowVersion` to wersja wiersza, kiedy jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-478">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="0a0cf-479">Gdy EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-479">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="0a0cf-480">Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-480">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="0a0cf-481">`RowVersion` w poleceniu SQL DELETE nie jest zgodny `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-481">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="0a0cf-482">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-482">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="0a0cf-483">`OnGetAsync` jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-483">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="0a0cf-484">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="0a0cf-484">Update the Delete page</span></span>

<span data-ttu-id="0a0cf-485">Zaktualizuj *strony/działy/Delete. cshtml* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-485">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="0a0cf-486">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-486">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="0a0cf-487">Aktualizuje dyrektywę `page` z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-487">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="0a0cf-488">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-488">Adds an error message.</span></span>
* <span data-ttu-id="0a0cf-489">Zastępuje FirstMidName z FullName w polu **administrator** .</span><span class="sxs-lookup"><span data-stu-id="0a0cf-489">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="0a0cf-490">Zmienia `RowVersion` w celu wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-490">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="0a0cf-491">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-491">Adds a hidden row version.</span></span> <span data-ttu-id="0a0cf-492">należy dodać `RowVersion` tak, aby zwroty z powrotem powiązać wartość.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-492">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="0a0cf-493">Badanie konfliktów współbieżności ze stroną Delete</span><span class="sxs-lookup"><span data-stu-id="0a0cf-493">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="0a0cf-494">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-494">Create a test department.</span></span>

<span data-ttu-id="0a0cf-495">Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:</span><span class="sxs-lookup"><span data-stu-id="0a0cf-495">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="0a0cf-496">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-496">Run the app and select Departments.</span></span>
* <span data-ttu-id="0a0cf-497">Kliknij prawym przyciskiem myszy hiperłącze **Usuń** dla działu testowego i wybierz polecenie **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-497">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="0a0cf-498">Kliknij hiperłącze **Edytuj** dla działu testowego.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-498">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="0a0cf-499">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-499">The two browser tabs display the same information.</span></span>

<span data-ttu-id="0a0cf-500">Zmień budżet na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-500">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="0a0cf-501">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-501">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="0a0cf-502">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-502">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="0a0cf-503">Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-503">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="0a0cf-504">Kliknięcie przycisku **Usuń** powoduje usunięcie jednostki, o ile `RowVersion` nie została zaktualizowana. dział został usunięty.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-504">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="0a0cf-505">Zobacz [dziedziczenie](xref:data/ef-mvc/inheritance) sposobu dziedziczenia modelu danych.</span><span class="sxs-lookup"><span data-stu-id="0a0cf-505">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="0a0cf-506">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0a0cf-506">Additional resources</span></span>

* [<span data-ttu-id="0a0cf-507">Tokeny współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="0a0cf-507">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="0a0cf-508">Obsługa współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="0a0cf-508">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="0a0cf-509">Wersja tego samouczka usługi YouTube (obsługa konfliktów współbieżności)</span><span class="sxs-lookup"><span data-stu-id="0a0cf-509">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="0a0cf-510">Wersja usługi YouTube w tym samouczku (część 2)</span><span class="sxs-lookup"><span data-stu-id="0a0cf-510">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="0a0cf-511">Wersja usługi YouTube w tym samouczku (część 3)</span><span class="sxs-lookup"><span data-stu-id="0a0cf-511">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [<span data-ttu-id="0a0cf-512">Wstecz</span><span class="sxs-lookup"><span data-stu-id="0a0cf-512">Previous</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

