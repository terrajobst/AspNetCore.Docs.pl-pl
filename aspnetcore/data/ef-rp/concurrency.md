---
title: Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
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
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="2dd0b-103">Strony razor z programem EF Core w programie ASP.NET Core — współbieżności — 8 8</span><span class="sxs-lookup"><span data-stu-id="2dd0b-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="2dd0b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), i [Jan Kowalski P](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="2dd0b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2dd0b-105">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="2dd0b-106">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="2dd0b-106">Concurrency conflicts</span></span>

<span data-ttu-id="2dd0b-107">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-107">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="2dd0b-108">Użytkownik przejdzie do strony edytowania dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-108">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="2dd0b-109">Inny użytkownik aktualizuje tę samą jednostkę przed zapisaniem zmiany pierwszego użytkownika w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-109">Another user updates the same entity before the first user's change is written to the database.</span></span>

<span data-ttu-id="2dd0b-110">Jeśli wykrywanie współbieżności nie jest włączone, osoba, która aktualizuje bazę danych, ostatnio zastępuje zmiany wprowadzone przez innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-110">If concurrency detection isn't enabled, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="2dd0b-111">Jeśli to ryzyko jest akceptowalne, koszt programowania dla współbieżności może być korzystny.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-111">If this risk is acceptable, the cost of programming for concurrency might outweigh the benefit.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="2dd0b-112">Współbieżność pesymistyczna (blokowanie)</span><span class="sxs-lookup"><span data-stu-id="2dd0b-112">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="2dd0b-113">Jednym ze sposobów zapobiegania konfliktom współbieżności jest użycie blokad bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-113">One way to prevent concurrency conflicts is to use database locks.</span></span> <span data-ttu-id="2dd0b-114">Jest to nazywane pesymistyczną współbieżnością.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-114">This is called pessimistic concurrency.</span></span> <span data-ttu-id="2dd0b-115">Zanim aplikacja odczyta wiersz bazy danych, który zamierza zaktualizować, żąda blokady.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-115">Before the app reads a database row that it intends to update, it requests a lock.</span></span> <span data-ttu-id="2dd0b-116">Gdy wiersz jest zablokowany na potrzeby dostępu do aktualizacji, żaden inny użytkownik nie może zablokować wiersza do momentu zwolnienia pierwszej blokady.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-116">Once a row is locked for update access, no other users are allowed to lock the row until the first lock is released.</span></span>

<span data-ttu-id="2dd0b-117">Zarządzanie blokadami ma wady.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-117">Managing locks has disadvantages.</span></span> <span data-ttu-id="2dd0b-118">Może być skomplikowany dla programu i może spowodować problemy z wydajnością w miarę zwiększania się liczby użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-118">It can be complex to program and can cause performance problems as the number of users increases.</span></span> <span data-ttu-id="2dd0b-119">Entity Framework Core nie zapewnia wbudowanej pomocy technicznej i ten samouczek nie pokazuje, jak wdrożyć go.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-119">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="2dd0b-120">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="2dd0b-120">Optimistic concurrency</span></span>

<span data-ttu-id="2dd0b-121">Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-121">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="2dd0b-122">Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-122">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget30.png)

<span data-ttu-id="2dd0b-124">Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-124">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date30.png)

<span data-ttu-id="2dd0b-126">Janina klika pozycję **Zapisz** pierwszy i widzimy, że zmiany zaczęły obowiązywać, ponieważ przeglądarka wyświetla stronę indeksu z zerem jako kwotą budżetu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-126">Jane clicks **Save** first and sees her change take effect, since the browser displays the Index page with zero as the Budget amount.</span></span>

<span data-ttu-id="2dd0b-127">John kliknie **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-127">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="2dd0b-128">Co się stanie dalej, zależy od sposobu obsługi konfliktów współbieżności:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-128">What happens next is determined by how you handle concurrency conflicts:</span></span>

* <span data-ttu-id="2dd0b-129">Można śledzić, która właściwość została zmodyfikowana przez użytkownika i zaktualizować tylko odpowiednie kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-129">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

  <span data-ttu-id="2dd0b-130">W tym scenariuszu zostałyby utracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-130">In the scenario, no data would be lost.</span></span> <span data-ttu-id="2dd0b-131">Inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-131">Different properties were updated by the two users.</span></span> <span data-ttu-id="2dd0b-132">Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-132">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="2dd0b-133">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-133">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="2dd0b-134">Takie podejście ma pewne wady:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-134">This approach has some disadvantages:</span></span>
 
  * <span data-ttu-id="2dd0b-135">Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-135">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="2dd0b-136">Zwykle nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-136">Is generally not practical in a web app.</span></span> <span data-ttu-id="2dd0b-137">Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-137">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="2dd0b-138">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-138">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="2dd0b-139">Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-139">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="2dd0b-140">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-140">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="2dd0b-141">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-141">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="2dd0b-142">To podejście jest nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-142">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="2dd0b-143">(Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-143">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="2dd0b-144">Można zapobiec aktualizacji firmy Jan ze zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-144">You can prevent John's change from being updated in the database.</span></span> <span data-ttu-id="2dd0b-145">Zazwyczaj aplikacja będzie:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-145">Typically, the app would:</span></span>

  * <span data-ttu-id="2dd0b-146">Wyświetl komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-146">Display an error message.</span></span>
  * <span data-ttu-id="2dd0b-147">Umożliwia wyświetlenie bieżącego stanu danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-147">Show the current state of the data.</span></span>
  * <span data-ttu-id="2dd0b-148">Zezwalaj użytkownikowi ponownie zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-148">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="2dd0b-149">Jest to nazywane *Store Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-149">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="2dd0b-150">(Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-150">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="2dd0b-151">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-151">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="conflict-detection-in-ef-core"></a><span data-ttu-id="2dd0b-152">Wykrywanie konfliktów w EF Core</span><span class="sxs-lookup"><span data-stu-id="2dd0b-152">Conflict detection in EF Core</span></span>

<span data-ttu-id="2dd0b-153">EF Core generuje wyjątki `DbConcurrencyException` w przypadku wykrycia konfliktów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-153">EF Core throws `DbConcurrencyException` exceptions when it detects conflicts.</span></span> <span data-ttu-id="2dd0b-154">Model danych musi być skonfigurowany, aby umożliwić wykrywanie konfliktów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-154">The data model has to be configured to enable conflict detection.</span></span> <span data-ttu-id="2dd0b-155">Opcje włączania wykrywania konfliktów obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-155">Options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="2dd0b-156">Skonfiguruj EF Core, aby uwzględnić oryginalne wartości kolumn skonfigurowanych jako [tokeny współbieżności](/ef/core/modeling/concurrency) w klauzuli WHERE poleceń Update i DELETE.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-156">Configure EF Core to include the original values of columns configured as [concurrency tokens](/ef/core/modeling/concurrency) in the Where clause of Update and Delete commands.</span></span>

  <span data-ttu-id="2dd0b-157">Gdy `SaveChanges` jest wywoływana, klauzula WHERE szuka oryginalnych wartości wszelkich właściwości, które mają adnotację z atrybutem [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) .</span><span class="sxs-lookup"><span data-stu-id="2dd0b-157">When `SaveChanges` is called, the Where clause looks for the original values of any properties annotated with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) attribute.</span></span> <span data-ttu-id="2dd0b-158">Instrukcja Update nie znajdzie wiersza do zaktualizowania, jeśli którykolwiek z właściwości tokenu współbieżności został zmieniony od momentu pierwszego odczytu wiersza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-158">The update statement won't find a row to update if any of the concurrency token properties changed since the row was first read.</span></span> <span data-ttu-id="2dd0b-159">EF Core interpretuje ten sposób jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-159">EF Core interprets that as a concurrency conflict.</span></span> <span data-ttu-id="2dd0b-160">W przypadku tabel bazy danych z wieloma kolumnami takie podejście może skutkować bardzo dużymi klauzulami WHERE i może wymagać dużej ilości danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-160">For database tables that have many columns, this approach can result in very large Where clauses, and can require large amounts of state.</span></span> <span data-ttu-id="2dd0b-161">W związku z tym takie podejście zwykle nie jest zalecane i nie jest to metoda używana w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-161">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

* <span data-ttu-id="2dd0b-162">W tabeli bazy danych Dołącz kolumnę śledzenia, której można użyć do określenia, kiedy wiersz został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span>

  <span data-ttu-id="2dd0b-163">W bazie danych SQL Server typ danych kolumny śledzenia jest `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-163">In a SQL Server database, the data type of the tracking column is `rowversion`.</span></span> <span data-ttu-id="2dd0b-164">Wartość `rowversion` jest kolejnym numerem, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-164">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="2dd0b-165">W przypadku polecenia Update lub DELETE klauzula WHERE zawiera oryginalną wartość kolumny śledzenia (numer wersji pierwszego wiersza).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-165">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version number).</span></span> <span data-ttu-id="2dd0b-166">Jeśli aktualizowany wiersz został zmieniony przez innego użytkownika, wartość w kolumnie `rowversion` różni się od oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-166">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value.</span></span> <span data-ttu-id="2dd0b-167">W takim przypadku instrukcja UPDATE lub DELETE nie może znaleźć wiersza do zaktualizowania z powodu klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-167">In that case, the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="2dd0b-168">EF Core zgłasza wyjątek współbieżności, gdy polecenie Update lub DELETE nie ma na nich żadnych wierszy.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-168">EF Core throws a concurrency exception when no rows are affected by an Update or Delete command.</span></span>

## <a name="add-a-tracking-property"></a><span data-ttu-id="2dd0b-169">Dodaj właściwość śledzenia</span><span class="sxs-lookup"><span data-stu-id="2dd0b-169">Add a tracking property</span></span>

<span data-ttu-id="2dd0b-170">W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-170">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

<span data-ttu-id="2dd0b-171">Atrybut [timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) wskazuje, co identyfikuje kolumnę jako kolumnę śledzenia współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-171">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute is what identifies the column as a concurrency tracking column.</span></span> <span data-ttu-id="2dd0b-172">Interfejs API Fluent jest alternatywnym sposobem określenia właściwości śledzenia:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-172">The fluent API is an alternative way to specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2dd0b-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dd0b-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2dd0b-174">W przypadku bazy danych SQL Server atrybut `[Timestamp]` właściwości Entity zdefiniowany jako tablica bajtowa:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-174">For a SQL Server database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="2dd0b-175">Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-175">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="2dd0b-176">Ustawia typ kolumny w bazie danych na [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-176">Sets the column type in the database to [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span></span>

<span data-ttu-id="2dd0b-177">Baza danych generuje sekwencyjny numer wersji wiersza, który jest zwiększany za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-177">The database generates a sequential row version number that's incremented each time the row is updated.</span></span> <span data-ttu-id="2dd0b-178">W `Update` lub `Delete`, klauzula `Where` zawiera wartość wersji wiersza do pobrania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-178">In an `Update` or `Delete` command, the `Where` clause includes the fetched row version value.</span></span> <span data-ttu-id="2dd0b-179">Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-179">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="2dd0b-180">Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-180">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="2dd0b-181">Polecenia `Update` lub `Delete` nie znalazły wiersza, ponieważ klauzula `Where` szuka wartości wersji wiersza pobrania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-181">The `Update` or `Delete` commands don't find a row because the `Where` clause looks for the fetched row version value.</span></span>
* <span data-ttu-id="2dd0b-182">A `DbUpdateConcurrencyException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-182">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="2dd0b-183">Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-183">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

<span data-ttu-id="2dd0b-184">Poprzednie wyróżnione przedstawia kod `WHERE` zawierających klauzulę `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-184">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="2dd0b-185">Jeśli baza danych `RowVersion` nie jest równa parametrowi `RowVersion` (`@p2`), żadne wiersze nie są aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-185">If the database `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="2dd0b-186">Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-186">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

<span data-ttu-id="2dd0b-187">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy na ostatniej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-187">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="2dd0b-188">Jeśli żadne wiersze nie są aktualizowane, EF Core zgłasza `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-188">If no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2dd0b-189">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2dd0b-189">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2dd0b-190">W przypadku bazy danych programu SQLite atrybut `[Timestamp]` właściwości Entity został zdefiniowany jako tablica bajtowa:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-190">For a SQLite database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="2dd0b-191">Powoduje, że kolumna powinna zostać uwzględniona w klauzulach DELETE i UPDATE WHERE.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-191">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="2dd0b-192">Mapuje do typu kolumny obiektu BLOB.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-192">Maps to a BLOB column type.</span></span>

<span data-ttu-id="2dd0b-193">Wyzwalacze bazy danych aktualizują kolumnę RowVersion za pomocą nowej losowej tablicy bajtów za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-193">Database triggers update the RowVersion column with a new random byte array whenever a row is updated.</span></span> <span data-ttu-id="2dd0b-194">W `Update` lub `Delete`, klauzula `Where` zawiera pobraną wartość kolumny RowVersion.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-194">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of the RowVersion column.</span></span> <span data-ttu-id="2dd0b-195">Jeśli aktualizowany wiersz został zmieniony od czasu pobrania:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-195">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="2dd0b-196">Wartość bieżącej wersji wiersza nie jest zgodna z pobraną wartością.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-196">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="2dd0b-197">Polecenie `Update` lub `Delete` nie znajduje wiersza, ponieważ klauzula `Where` szuka pierwotnej wartości wersji wiersza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-197">The `Update` or `Delete` command doesn't find a row because the `Where` clause looks for the original row version value.</span></span>
* <span data-ttu-id="2dd0b-198">A `DbUpdateConcurrencyException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-198">A `DbUpdateConcurrencyException` is thrown.</span></span>

---

### <a name="update-the-database"></a><span data-ttu-id="2dd0b-199">Aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="2dd0b-199">Update the database</span></span>

<span data-ttu-id="2dd0b-200">Dodanie właściwości `RowVersion` powoduje zmianę modelu danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-200">Adding the `RowVersion` property changes the data model, which requires a migration.</span></span>

<span data-ttu-id="2dd0b-201">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-201">Build the project.</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2dd0b-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dd0b-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2dd0b-203">Uruchom następujące polecenie w obszarze PMC:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-203">Run the following command in the PMC:</span></span>

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2dd0b-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2dd0b-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2dd0b-205">Uruchom następujące polecenie w terminalu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-205">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

<span data-ttu-id="2dd0b-206">To polecenie:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-206">This command:</span></span>

* <span data-ttu-id="2dd0b-207">Tworzy plik migracji *_RowVersion. cs migracji/sygnatur czasowych}* .</span><span class="sxs-lookup"><span data-stu-id="2dd0b-207">Creates the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="2dd0b-208">Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-208">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="2dd0b-209">Aktualizacja dodaje następujący wyróżniony kod do `BuildModel` metody:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-209">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2dd0b-210">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dd0b-210">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2dd0b-211">Uruchom następujące polecenie w obszarze PMC:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-211">Run the following command in the PMC:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2dd0b-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2dd0b-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2dd0b-213">Otwórz plik `Migrations/<timestamp>_RowVersion.cs` i Dodaj wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-213">Open the `Migrations/<timestamp>_RowVersion.cs` file and add the highlighted code:</span></span>

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  <span data-ttu-id="2dd0b-214">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-214">The preceding code:</span></span>

  * <span data-ttu-id="2dd0b-215">Aktualizuje istniejące wiersze z losowymi wartościami obiektów BLOB.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-215">Updates existing rows with random blob values.</span></span>
  * <span data-ttu-id="2dd0b-216">Dodaje wyzwalacze bazy danych, które ustawiają kolumnę RowVersion na losową wartość obiektu BLOB za każdym razem, gdy wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-216">Adds database triggers that set the RowVersion column to a random blob value whenever a row is updated.</span></span>

* <span data-ttu-id="2dd0b-217">Uruchom następujące polecenie w terminalu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-217">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a><span data-ttu-id="2dd0b-218">Strony działu szkieletu</span><span class="sxs-lookup"><span data-stu-id="2dd0b-218">Scaffold Department pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2dd0b-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dd0b-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2dd0b-220">Postępuj zgodnie z instrukcjami na [stronach uczniów tworzenia szkieletów](xref:data/ef-rp/intro#scaffold-student-pages) z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-220">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

* <span data-ttu-id="2dd0b-221">Utwórz folder *strony/działy* .</span><span class="sxs-lookup"><span data-stu-id="2dd0b-221">Create a *Pages/Departments* folder.</span></span>  
* <span data-ttu-id="2dd0b-222">Użyj `Department` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-222">Use `Department` for the model class.</span></span>
  * <span data-ttu-id="2dd0b-223">Użyj istniejącej klasy kontekstu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-223">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2dd0b-224">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2dd0b-224">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2dd0b-225">Utwórz folder *strony/działy* .</span><span class="sxs-lookup"><span data-stu-id="2dd0b-225">Create a *Pages/Departments* folder.</span></span>

* <span data-ttu-id="2dd0b-226">Uruchom następujące polecenie, aby uzyskać szkielet na stronach działu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-226">Run the following command to scaffold the Department pages.</span></span>

  <span data-ttu-id="2dd0b-227">**Na Windows:**</span><span class="sxs-lookup"><span data-stu-id="2dd0b-227">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  <span data-ttu-id="2dd0b-228">**W systemie Linux lub macOS:**</span><span class="sxs-lookup"><span data-stu-id="2dd0b-228">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="2dd0b-229">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-229">Build the project.</span></span>

## <a name="update-the-index-page"></a><span data-ttu-id="2dd0b-230">Aktualizowanie strony indeksu</span><span class="sxs-lookup"><span data-stu-id="2dd0b-230">Update the Index page</span></span>

<span data-ttu-id="2dd0b-231">Narzędzie tworzenia szkieletów utworzyło kolumnę `RowVersion` dla strony indeks, ale to pole nie będzie wyświetlane w aplikacji produkcyjnej.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-231">The scaffolding tool created a `RowVersion` column for the Index page, but that field wouldn't be displayed in a production app.</span></span> <span data-ttu-id="2dd0b-232">W tym samouczku zostanie wyświetlony ostatni bajt `RowVersion`, aby zobaczyć, jak działa obsługa współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-232">In this tutorial, the last byte of the `RowVersion` is displayed to help show how concurrency handling works.</span></span> <span data-ttu-id="2dd0b-233">Ostatni bajt nie gwarantuje, że jest unikatowy.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-233">The last byte isn't guaranteed to be unique by itself.</span></span>

<span data-ttu-id="2dd0b-234">Strona aktualizacji *Pages\Departments\Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dd0b-234">Update *Pages\Departments\Index.cshtml* page:</span></span>

* <span data-ttu-id="2dd0b-235">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-235">Replace Index with Departments.</span></span>
* <span data-ttu-id="2dd0b-236">Zmień kod zawierający `RowVersion`, aby pokazać tylko ostatni bajt tablicy bajtów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-236">Change the code containing `RowVersion` to show just the last byte of the byte array.</span></span>
* <span data-ttu-id="2dd0b-237">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-237">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="2dd0b-238">Poniższy kod przedstawia zaktualizowaną stronę:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-238">The following code shows the updated page:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a><span data-ttu-id="2dd0b-239">Aktualizowanie modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="2dd0b-239">Update the Edit page model</span></span>

<span data-ttu-id="2dd0b-240">Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-240">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

<span data-ttu-id="2dd0b-241">[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) jest aktualizowana przy użyciu wartości `rowVersion` z jednostki, gdy została ona pobrana w metodzie `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-241">The [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity when it was fetched in the `OnGet` method.</span></span> <span data-ttu-id="2dd0b-242">EF Core generuje polecenia aktualizacji programu SQL z klauzulą WHERE zawiera oryginał `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-242">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="2dd0b-243">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (nie wiersze mają oryginalny `RowVersion` wartość), `DbUpdateConcurrencyException` jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-243">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

<span data-ttu-id="2dd0b-244">W poprzednim wyróżnionym kodzie:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-244">In the preceding highlighted code:</span></span>

* <span data-ttu-id="2dd0b-245">Wartość w `Department.RowVersion` to jaka znajdowała się w jednostce, gdy została pierwotnie pobrana w żądaniu pobrania dla strony edycji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-245">The value in `Department.RowVersion` is what was in the entity when it was originally fetched in the Get request for the Edit page.</span></span> <span data-ttu-id="2dd0b-246">Wartość jest dostarczana do metody `OnPost` przez ukryte pole na stronie Razor, które wyświetla jednostkę do edycji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-246">The value is provided to the `OnPost` method by a hidden field in the Razor page that displays the entity to be edited.</span></span> <span data-ttu-id="2dd0b-247">Wartość pola ukrytego jest kopiowana do `Department.RowVersion` przez spinacz modelu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-247">The hidden field value is copied to `Department.RowVersion` by the model binder.</span></span>
* <span data-ttu-id="2dd0b-248">`OriginalValue` jest tym, czego EF Core będzie używać w klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-248">`OriginalValue` is what EF Core will use in the Where clause.</span></span> <span data-ttu-id="2dd0b-249">Przed wykonaniem wyróżnionego wiersza kodu `OriginalValue` ma wartość znajdującą się w bazie danych, gdy `FirstOrDefaultAsync` została wywołana w tej metodzie, która może się różnić od tego, co zostało wyświetlone na stronie Edycja.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-249">Before the highlighted line of code executes, `OriginalValue` has the value that was in the database when `FirstOrDefaultAsync` was called in this method, which might be different from what was displayed on the Edit page.</span></span>
* <span data-ttu-id="2dd0b-250">Wyróżniony kod sprawdza, czy EF Core używa oryginalnej wartości `RowVersion` z wyświetlonej jednostki `Department` w klauzuli WHERE instrukcji SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-250">The highlighted code makes sure that EF Core uses the original `RowVersion` value from the displayed `Department` entity in the SQL UPDATE statement's Where clause.</span></span>

<span data-ttu-id="2dd0b-251">Gdy wystąpi błąd współbieżności, poniższy wyróżniony kod pobiera wartości klienta (wartości ogłoszone w tej metodzie) oraz wartości bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-251">When a concurrency error happens, the following highlighted code gets the client values (the values posted to this method) and the database values.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

<span data-ttu-id="2dd0b-252">Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-252">The following code adds a custom error message for each column that has database values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

<span data-ttu-id="2dd0b-253">Poniższy wyróżniony kod ustawia wartość `RowVersion` na nową wartość pobraną z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-253">The following highlighted code sets the `RowVersion` value to the new value retrieved from the database.</span></span> <span data-ttu-id="2dd0b-254">Przy następnym kliknięciu **Zapisz**, tylko błędy współbieżności, które odbywa się od czasu ostatniego wyświetlania strony edytowania zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-254">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

<span data-ttu-id="2dd0b-255">`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-255">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="2dd0b-256">Strony Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-256">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

### <a name="update-the-razor-page"></a><span data-ttu-id="2dd0b-257">Aktualizowanie strony Razor</span><span class="sxs-lookup"><span data-stu-id="2dd0b-257">Update the Razor page</span></span>

<span data-ttu-id="2dd0b-258">Zaktualizuj *strony/działy/Edit. cshtml* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-258">Update *Pages/Departments/Edit.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="2dd0b-259">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-259">The preceding code:</span></span>

* <span data-ttu-id="2dd0b-260">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-260">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2dd0b-261">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-261">Adds a hidden row version.</span></span> <span data-ttu-id="2dd0b-262">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-262">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="2dd0b-263">Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-263">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="2dd0b-264">Zastępuje `ViewData` z silnie typizowanych `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-264">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

### <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="2dd0b-265">Badanie konfliktów współbieżności przy użyciu strony edytowania</span><span class="sxs-lookup"><span data-stu-id="2dd0b-265">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="2dd0b-266">Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-266">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="2dd0b-267">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-267">Run the app and select Departments.</span></span>
* <span data-ttu-id="2dd0b-268">Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-268">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2dd0b-269">Na pierwszej karcie kliknij **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-269">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="2dd0b-270">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-270">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2dd0b-271">Zmień nazwę w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-271">Change the name in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-130.png)

<span data-ttu-id="2dd0b-273">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-273">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2dd0b-274">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-274">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2dd0b-275">Zmień inne pole w drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-275">Change a different field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-230.png)

<span data-ttu-id="2dd0b-277">Kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-277">Click **Save**.</span></span> <span data-ttu-id="2dd0b-278">Komunikaty o błędach są wyświetlane dla wszystkich pól, które nie pasują do wartości bazy danych:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-278">You see error messages for all fields that don't match the database values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error30.png)

<span data-ttu-id="2dd0b-280">To okno przeglądarki przypadkowo zmienić pole Nazwa.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-280">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="2dd0b-281">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-281">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="2dd0b-282">Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-282">Tab out. Client-side validation removes the error message.</span></span>

<span data-ttu-id="2dd0b-283">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-283">Click **Save** again.</span></span> <span data-ttu-id="2dd0b-284">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-284">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="2dd0b-285">Zobaczysz zapisane wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-285">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="2dd0b-286">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="2dd0b-286">Update the Delete page</span></span>

<span data-ttu-id="2dd0b-287">Zaktualizuj *strony/działy/Delete. cshtml. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-287">Update *Pages/Departments/Delete.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="2dd0b-288">Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-288">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="2dd0b-289">`Department.RowVersion` jest to wersja wiersza, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-289">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="2dd0b-290">EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-290">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="2dd0b-291">Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-291">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="2dd0b-292">`RowVersion` w poleceniu SQL DELETE nie jest zgodny `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-292">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the database.</span></span>
* <span data-ttu-id="2dd0b-293">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-293">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="2dd0b-294">`OnGetAsync` jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-294">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="2dd0b-295">Aktualizowanie strony usuwania Razor</span><span class="sxs-lookup"><span data-stu-id="2dd0b-295">Update the Delete Razor page</span></span>

<span data-ttu-id="2dd0b-296">Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-296">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="2dd0b-297">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-297">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="2dd0b-298">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-298">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2dd0b-299">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-299">Adds an error message.</span></span>
* <span data-ttu-id="2dd0b-300">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-300">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="2dd0b-301">Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-301">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="2dd0b-302">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-302">Adds a hidden row version.</span></span> <span data-ttu-id="2dd0b-303">należy dodać `RowVersion`, aby postgit dodać ponownie powiązania wartości.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-303">`RowVersion` must be added so postgit add back binds the value.</span></span>

### <a name="test-concurrency-conflicts"></a><span data-ttu-id="2dd0b-304">Testuj konflikty współbieżności</span><span class="sxs-lookup"><span data-stu-id="2dd0b-304">Test concurrency conflicts</span></span>

<span data-ttu-id="2dd0b-305">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-305">Create a test department.</span></span>

<span data-ttu-id="2dd0b-306">Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-306">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="2dd0b-307">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-307">Run the app and select Departments.</span></span>
* <span data-ttu-id="2dd0b-308">Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu test i wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-308">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2dd0b-309">Kliknij przycisk **Edytuj** hiperłącze dla działu testu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-309">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="2dd0b-310">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-310">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2dd0b-311">Zmień budżetu w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-311">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="2dd0b-312">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-312">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2dd0b-313">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-313">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2dd0b-314">Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-314">Delete the test department from the second tab. A concurrency error is display with the current values from the database.</span></span> <span data-ttu-id="2dd0b-315">Klikając **Usuń** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-315">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2dd0b-316">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2dd0b-316">Additional resources</span></span>

* [<span data-ttu-id="2dd0b-317">Tokeny współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="2dd0b-317">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="2dd0b-318">Obsługa współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="2dd0b-318">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="2dd0b-319">Debugowanie ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="2dd0b-319">Debugging ASP.NET Core 2.x source</span></span>](https://github.com/aspnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a><span data-ttu-id="2dd0b-320">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="2dd0b-320">Next steps</span></span>

<span data-ttu-id="2dd0b-321">Jest to ostatni samouczek z serii.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-321">This is the last tutorial in the series.</span></span> <span data-ttu-id="2dd0b-322">Dodatkowe tematy zostały omówione w [wersji MVC tej serii samouczków](xref:data/ef-mvc/index).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-322">Additional topics are covered in the [MVC version of this tutorial series](xref:data/ef-mvc/index).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2dd0b-323">Poprzedni samouczek</span><span class="sxs-lookup"><span data-stu-id="2dd0b-323">Previous tutorial</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2dd0b-324">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników zaktualizowania jednostki jednocześnie (w tym samym czasie).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-324">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="2dd0b-325">Jeśli napotkasz problemy, nie można rozwiązać, [pobrania lub wyświetlenia ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="2dd0b-325">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="2dd0b-326">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-326">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="2dd0b-327">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="2dd0b-327">Concurrency conflicts</span></span>

<span data-ttu-id="2dd0b-328">Występuje konflikt współbieżności, gdy:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-328">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="2dd0b-329">Użytkownik przejdzie do strony edytowania dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-329">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="2dd0b-330">Inny użytkownik aktualizuje tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-330">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="2dd0b-331">Jeśli wykrycie współbieżności nie jest włączone, gdy wystąpią równoczesne aktualizacje:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-331">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="2dd0b-332">Ostatnia aktualizacja usługi wins.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-332">The last update wins.</span></span> <span data-ttu-id="2dd0b-333">Oznacza to, że ostatnie wartości aktualizacji są zapisywane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-333">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="2dd0b-334">Pierwszego dnia bieżące aktualizacje zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-334">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="2dd0b-335">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="2dd0b-335">Optimistic concurrency</span></span>

<span data-ttu-id="2dd0b-336">Optymistyczna współbieżność umożliwia konfliktów współbieżności do wykonania, a następnie reaguje odpowiednio po ich wykonaj.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-336">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="2dd0b-337">Na przykład Magdalena odwiedzin strony edytowania działu i zmienia budżetu działu angielskiego z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-337">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="2dd0b-339">Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-339">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

<span data-ttu-id="2dd0b-341">Magdalena kliknie **Zapisz** pierwszy i widzi jej zmienić, gdy w przeglądarce pojawi się strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-341">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budżet na zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="2dd0b-343">John kliknie **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-343">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="2dd0b-344">Co dzieje się potem określają sposób obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-344">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="2dd0b-345">Optymistyczna współbieżność obejmuje następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-345">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="2dd0b-346">Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-346">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="2dd0b-347">W tym scenariuszu zostałyby utracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-347">In the scenario, no data would be lost.</span></span> <span data-ttu-id="2dd0b-348">Inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-348">Different properties were updated by the two users.</span></span> <span data-ttu-id="2dd0b-349">Przy następnym ktoś przegląda angielskiej działu, zobaczy Joanny i John's zmiany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-349">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="2dd0b-350">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-350">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="2dd0b-351">To podejście:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-351">This approach:</span></span>
 
  * <span data-ttu-id="2dd0b-352">Nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostaną wprowadzone do tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-352">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="2dd0b-353">Zwykle nie jest praktyczne w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-353">Is generally not practical in a web app.</span></span> <span data-ttu-id="2dd0b-354">Wymaga to zachowanie stanu znaczne w celu śledzenia wszystkich pobrano oraz nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-354">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="2dd0b-355">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-355">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="2dd0b-356">Może zwiększyć złożoność aplikacji w porównaniu do wykrywania współbieżności w jednostce.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-356">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="2dd0b-357">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-357">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="2dd0b-358">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i pobrano wartość $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-358">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="2dd0b-359">To podejście jest nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-359">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="2dd0b-360">(Wszystkie wartości z klienta mają pierwszeństwo przed tym, co znajduje się w magazynie danych). Jeśli nie jest wykonywane żadne kodowanie dla obsługi współbieżności, klient WINS działa automatycznie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-360">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="2dd0b-361">Aby uniemożliwić zmiany John's aktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-361">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="2dd0b-362">Zazwyczaj aplikacja będzie:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-362">Typically, the app would:</span></span>

  * <span data-ttu-id="2dd0b-363">Wyświetl komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-363">Display an error message.</span></span>
  * <span data-ttu-id="2dd0b-364">Umożliwia wyświetlenie bieżącego stanu danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-364">Show the current state of the data.</span></span>
  * <span data-ttu-id="2dd0b-365">Zezwalaj użytkownikowi ponownie zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-365">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="2dd0b-366">Jest to nazywane *Store Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-366">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="2dd0b-367">(Wartości ze sklepu danych mają pierwszeństwo przed wartościami przesyłanymi przez klienta). W tym samouczku zaimplementowano scenariusz magazynu usługi WINS.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-367">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="2dd0b-368">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-368">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="2dd0b-369">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="2dd0b-369">Handling concurrency</span></span> 

<span data-ttu-id="2dd0b-370">Gdy właściwość jest skonfigurowany jako [tokenu współbieżności](/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="2dd0b-370">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="2dd0b-371">EF Core sprawdza, czy właściwości nie został zmodyfikowany po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-371">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="2dd0b-372">Sprawdzanie jest wykonywane podczas [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) lub [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-372">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="2dd0b-373">Jeśli właściwość została zmieniona po jego pobrania, [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-373">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="2dd0b-374">Model danych i bazy danych musi być skonfigurowany do obsługi zgłaszanie `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-374">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="2dd0b-375">Wykrywanie konfliktów współbieżności we właściwości</span><span class="sxs-lookup"><span data-stu-id="2dd0b-375">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="2dd0b-376">Konfliktów współbieżności może zostać wykryte przy użyciu na poziomie właściwość [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-376">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="2dd0b-377">Ten atrybut można zastosować na wiele właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-377">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="2dd0b-378">Aby uzyskać więcej informacji, zobacz [danych adnotacje-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="2dd0b-378">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="2dd0b-379">`[ConcurrencyCheck]` Atrybut nie jest używany w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-379">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="2dd0b-380">Wykrywanie konfliktów współbieżności wiersz</span><span class="sxs-lookup"><span data-stu-id="2dd0b-380">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="2dd0b-381">Do wykrywania konfliktów współbieżności [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) kolumny śledzenia jest dodawany do modelu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-381">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="2dd0b-382">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="2dd0b-382">`rowversion` :</span></span>

* <span data-ttu-id="2dd0b-383">Dotyczy programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-383">Is SQL Server specific.</span></span> <span data-ttu-id="2dd0b-384">Inne bazy danych nie mogą zawierać podobnych funkcji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-384">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="2dd0b-385">Służy do określenia, czy jednostka nie została zmieniona, ponieważ została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-385">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="2dd0b-386">Baza danych generuje kolejna `rowversion` numer jest zwiększany po każdej wiersz jest aktualizowany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-386">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="2dd0b-387">W `Update` lub `Delete` polecenia `Where` klauzula zawiera wartość pobrano `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-387">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="2dd0b-388">Jeśli zmieniono aktualizacji wiersza:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-388">If the row being updated has changed:</span></span>

* <span data-ttu-id="2dd0b-389">`rowversion` nie pasuje do wartości pobrano.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-389">`rowversion` doesn't match the fetched value.</span></span>
* <span data-ttu-id="2dd0b-390">`Update` Lub `Delete` poleceń nie znajdziesz wiersza, ponieważ `Where` klauzula zawiera pobrano `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-390">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
* <span data-ttu-id="2dd0b-391">A `DbUpdateConcurrencyException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-391">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="2dd0b-392">W programie EF Core, gdy żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia, jest zgłaszany wyjątek współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-392">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="2dd0b-393">Dodawanie właściwości śledzenia do jednostki działu</span><span class="sxs-lookup"><span data-stu-id="2dd0b-393">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="2dd0b-394">W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-394">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="2dd0b-395">[Sygnatura czasowa](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atrybut określa, czy ta kolumna znajduje się w `Where` klauzuli `Update` i `Delete` poleceń.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-395">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="2dd0b-396">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` typ zastąpiono ją.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-396">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="2dd0b-397">Interfejs fluent API można również określić właściwości śledzenia:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-397">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="2dd0b-398">Poniższy kod ilustruje część języka T-SQL, generowane przez platformę EF Core po zaktualizowaniu nazwy działu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-398">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

<span data-ttu-id="2dd0b-399">Poprzednie wyróżnione przedstawia kod `WHERE` zawierających klauzulę `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-399">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="2dd0b-400">Jeśli bazy danych `RowVersion` nie równa się `RowVersion` parametru (`@p2`), wiersze nie zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-400">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="2dd0b-401">Następujący wyróżniony kod przedstawia języka T-SQL sprawdza, czy dokładnie jeden wiersz został zaktualizowany:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-401">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

<span data-ttu-id="2dd0b-402">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) zwraca liczbę wierszy na ostatniej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-402">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="2dd0b-403">W żadnym wiersze są aktualizowane, zgłasza programu EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-403">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="2dd0b-404">Widać, że T-SQL programu EF Core generuje w oknie danych wyjściowych programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-404">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="2dd0b-405">Aktualizacja bazy danych</span><span class="sxs-lookup"><span data-stu-id="2dd0b-405">Update the DB</span></span>

<span data-ttu-id="2dd0b-406">Dodawanie `RowVersion` zmienia właściwość modelu bazy danych, który wymaga migracji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-406">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="2dd0b-407">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-407">Build the project.</span></span> <span data-ttu-id="2dd0b-408">W oknie polecenia, należy wprowadzić następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-408">Enter the following in a command window:</span></span>

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="2dd0b-409">Poprzedniego polecenia:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-409">The preceding commands:</span></span>

* <span data-ttu-id="2dd0b-410">Dodaje *migracje / {stamp}_RowVersion.cs czasu* pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-410">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="2dd0b-411">Aktualizacje *Migrations/SchoolContextModelSnapshot.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-411">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="2dd0b-412">Aktualizacja dodaje następujący wyróżniony kod do `BuildModel` metody:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-412">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* <span data-ttu-id="2dd0b-413">Uruchamia migracji można zaktualizować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-413">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a><span data-ttu-id="2dd0b-414">Tworzenie szkieletu modelu działów</span><span class="sxs-lookup"><span data-stu-id="2dd0b-414">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2dd0b-415">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dd0b-415">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="2dd0b-416">Postępuj zgodnie z instrukcjami w [tworzenia szkieletu modelu uczniów](xref:data/ef-rp/intro#scaffold-student-pages) i użyj `Department` dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-416">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Department` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2dd0b-417">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2dd0b-417">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="2dd0b-418">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-418">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="2dd0b-419">Poprzedni szkielety mechanizmów polecenia `Department` modelu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-419">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="2dd0b-420">Otwórz projekt w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-420">Open the project in Visual Studio.</span></span>

<span data-ttu-id="2dd0b-421">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-421">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="2dd0b-422">Zaktualizuj strony działy indeksu</span><span class="sxs-lookup"><span data-stu-id="2dd0b-422">Update the Departments Index page</span></span>

<span data-ttu-id="2dd0b-423">Aparat tworzenia szkieletów utworzone `RowVersion` kolumny do strony indeksu, ale tego pola nie powinna wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-423">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="2dd0b-424">W tym samouczku, ostatni bajt `RowVersion` jest wyświetlana, aby lepiej zrozumieć współbieżności.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-424">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="2dd0b-425">Ostatni bajt nie musi być unikatowy.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-425">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="2dd0b-426">Rzeczywistej aplikacji nie wyświetlał `RowVersion` lub ostatni bajt `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-426">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="2dd0b-427">Zaktualizuj strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-427">Update the Index page:</span></span>

* <span data-ttu-id="2dd0b-428">Zastąp indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-428">Replace Index with Departments.</span></span>
* <span data-ttu-id="2dd0b-429">Zastąp kod znaczników zawierający `RowVersion` z ostatniego bajtu `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-429">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="2dd0b-430">Zastąp FirstMidName imię i nazwisko.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-430">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="2dd0b-431">Następujący kod przedstawia zaktualizowaną stronę:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-431">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="2dd0b-432">Aktualizowanie modelu strony edycji</span><span class="sxs-lookup"><span data-stu-id="2dd0b-432">Update the Edit page model</span></span>

<span data-ttu-id="2dd0b-433">Zaktualizuj *Pages\Departments\Edit.cshtml.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-433">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="2dd0b-434">Aby wykryć problem współbieżności, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) będą aktualizowane przy użyciu `rowVersion` wartości z obiektu została ona pobrana.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-434">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="2dd0b-435">EF Core generuje polecenia aktualizacji programu SQL z klauzulą WHERE zawiera oryginał `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-435">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="2dd0b-436">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (nie wiersze mają oryginalny `RowVersion` wartość), `DbUpdateConcurrencyException` jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-436">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="2dd0b-437">W poprzednim kodzie `Department.RowVersion` jest wartością, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-437">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="2dd0b-438">`OriginalValue` jest to wartość w bazie danych podczas `FirstOrDefaultAsync` została wywołana w przypadku tej metody.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-438">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="2dd0b-439">Poniższy kod umożliwia pobranie ustawienia klienta (wartości do tej metody) oraz bazy danych wartości:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-439">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="2dd0b-440">Poniższy kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma wartości bazy danych różne od tego, co zostało opublikowane w `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-440">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="2dd0b-441">Następujące wyróżniony kod ustawia `RowVersion` wartość do nowej wartości są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-441">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="2dd0b-442">Przy następnym kliknięciu **Zapisz**, tylko błędy współbieżności, które odbywa się od czasu ostatniego wyświetlania strony edytowania zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-442">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="2dd0b-443">`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-443">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="2dd0b-444">Strony Razor `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-444">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="2dd0b-445">Zaktualizuj strony edytowania</span><span class="sxs-lookup"><span data-stu-id="2dd0b-445">Update the Edit page</span></span>

<span data-ttu-id="2dd0b-446">Aktualizacja *Pages/Departments/Edit.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-446">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="2dd0b-447">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-447">The preceding markup:</span></span>

* <span data-ttu-id="2dd0b-448">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-448">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2dd0b-449">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-449">Adds a hidden row version.</span></span> <span data-ttu-id="2dd0b-450">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-450">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="2dd0b-451">Wyświetla ostatni bajt `RowVersion` na potrzeby debugowania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-451">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="2dd0b-452">Zastępuje `ViewData` z silnie typizowanych `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-452">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="2dd0b-453">Badanie konfliktów współbieżności przy użyciu strony edytowania</span><span class="sxs-lookup"><span data-stu-id="2dd0b-453">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="2dd0b-454">Otwórz dwa wystąpienia przeglądarki edycji na angielski działu:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-454">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="2dd0b-455">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-455">Run the app and select Departments.</span></span>
* <span data-ttu-id="2dd0b-456">Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-456">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2dd0b-457">Na pierwszej karcie kliknij **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-457">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="2dd0b-458">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-458">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2dd0b-459">Zmień nazwę w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-459">Change the name in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="2dd0b-461">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-461">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2dd0b-462">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-462">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2dd0b-463">Zmień inne pole w drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-463">Change a different field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="2dd0b-465">Kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-465">Click **Save**.</span></span> <span data-ttu-id="2dd0b-466">Zostaną wyświetlone komunikaty o błędach dla wszystkich pól, które nie są zgodne z wartościami bazy danych:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-466">You see error messages for all fields that don't match the DB values:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="2dd0b-468">To okno przeglądarki przypadkowo zmienić pole Nazwa.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-468">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="2dd0b-469">Skopiuj i Wklej bieżącą wartość (języki) w polu Nazwa.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-469">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="2dd0b-470">Na zewnątrz. Walidacja po stronie klienta usuwa komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-470">Tab out. Client-side validation removes the error message.</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/cv.png)

<span data-ttu-id="2dd0b-472">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-472">Click **Save** again.</span></span> <span data-ttu-id="2dd0b-473">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-473">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="2dd0b-474">Zobaczysz zapisane wartości na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-474">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="2dd0b-475">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="2dd0b-475">Update the Delete page</span></span>

<span data-ttu-id="2dd0b-476">Aktualizowanie modelu strony Usuń z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-476">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="2dd0b-477">Strony usuwania wykrywa konfliktów współbieżności, jeśli jednostka została zmieniona po jego pobrania.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-477">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="2dd0b-478">`Department.RowVersion` jest to wersja wiersza, jeśli jednostka została pobrana.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-478">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="2dd0b-479">EF Core tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-479">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="2dd0b-480">Jeśli wpłynąć na wyniki polecenia SQL DELETE, zerowego wierszy:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-480">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="2dd0b-481">`RowVersion` W SQL DELETE polecenia nie jest zgodna `RowVersion` w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-481">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="2dd0b-482">DbUpdateConcurrencyException wyjątku.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-482">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="2dd0b-483">`OnGetAsync` jest wywoływana z `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-483">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="2dd0b-484">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="2dd0b-484">Update the Delete page</span></span>

<span data-ttu-id="2dd0b-485">Aktualizacja *Pages/Departments/Delete.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-485">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="2dd0b-486">Poprzedni kod wprowadza następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-486">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="2dd0b-487">Aktualizacje `page` dyrektywy z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-487">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="2dd0b-488">Dodaje komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-488">Adds an error message.</span></span>
* <span data-ttu-id="2dd0b-489">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-489">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="2dd0b-490">Zmiany `RowVersion` do wyświetlenia ostatniego bajtu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-490">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="2dd0b-491">Dodaje wersji ukrytego wiersza.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-491">Adds a hidden row version.</span></span> <span data-ttu-id="2dd0b-492">`RowVersion` musi zostać dodany, więc ponownie wpis wiąże wartość.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-492">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="2dd0b-493">Badanie konfliktów współbieżności ze stroną Delete</span><span class="sxs-lookup"><span data-stu-id="2dd0b-493">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="2dd0b-494">Utwórz działu testu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-494">Create a test department.</span></span>

<span data-ttu-id="2dd0b-495">Otwórz dwa wystąpienia przeglądarki Delete w dziale badań:</span><span class="sxs-lookup"><span data-stu-id="2dd0b-495">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="2dd0b-496">Uruchom aplikację i wybierz działów.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-496">Run the app and select Departments.</span></span>
* <span data-ttu-id="2dd0b-497">Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu test i wybierz pozycję **Otwórz na nowej karcie**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-497">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="2dd0b-498">Kliknij przycisk **Edytuj** hiperłącze dla działu testu.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-498">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="2dd0b-499">Na kartach przeglądarki dwa są wyświetlane te same informacje.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-499">The two browser tabs display the same information.</span></span>

<span data-ttu-id="2dd0b-500">Zmień budżetu w pierwszej karty przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-500">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="2dd0b-501">Przeglądarka wyświetla stronę indeksu zmieniona wartość i zaktualizowano rowVersion wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-501">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="2dd0b-502">Zaktualizowano rowVersion wskaźnik, należy pamiętać, jest wyświetlany na drugi odświeżenie strony na innej karcie.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-502">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="2dd0b-503">Usuń dział testów z drugiej karty. Błąd współbieżności jest wyświetlany z bieżącymi wartościami z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-503">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="2dd0b-504">Klikając **Usuń** usuwa jednostki, chyba że `RowVersion` został updated.department został usunięty.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-504">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="2dd0b-505">Zobacz [dziedziczenia](xref:data/ef-mvc/inheritance) na temat sposobu dziedziczą modelu danych.</span><span class="sxs-lookup"><span data-stu-id="2dd0b-505">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2dd0b-506">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2dd0b-506">Additional resources</span></span>

* [<span data-ttu-id="2dd0b-507">Tokeny współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="2dd0b-507">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="2dd0b-508">Obsługa współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="2dd0b-508">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="2dd0b-509">Wersja tego samouczka usługi YouTube (obsługa konfliktów współbieżności)</span><span class="sxs-lookup"><span data-stu-id="2dd0b-509">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="2dd0b-510">Wersja usługi YouTube w tym samouczku (część 2)</span><span class="sxs-lookup"><span data-stu-id="2dd0b-510">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="2dd0b-511">Wersja usługi YouTube w tym samouczku (część 3)</span><span class="sxs-lookup"><span data-stu-id="2dd0b-511">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [<span data-ttu-id="2dd0b-512">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="2dd0b-512">Previous</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

