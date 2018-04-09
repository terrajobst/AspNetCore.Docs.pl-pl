---
title: Platformy ASP.NET Core MVC podstawowych EF - współbieżności - 8, 10
author: tdykstra
description: Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 99c4872719a4e46aa27eb7138eb914dc5954c219
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-ef-core---concurrency---8-of-10"></a><span data-ttu-id="8892b-103">Platformy ASP.NET Core MVC podstawowych EF - współbieżności - 8, 10</span><span class="sxs-lookup"><span data-stu-id="8892b-103">ASP.NET Core MVC with EF Core - Concurrency - 8 of 10</span></span>

<span data-ttu-id="8892b-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8892b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8892b-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8892b-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="8892b-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="8892b-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="8892b-107">W starszych samouczkach przedstawiono sposób aktualizowania danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-107">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="8892b-108">Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="8892b-108">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="8892b-109">Utworzysz stron sieci web, działające z jednostką działu i obsługi błędów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-109">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="8892b-110">Na poniższych ilustracjach przedstawiono edytowanie i usuwanie stron, tym komunikaty, które są wyświetlane, gdy wystąpi konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-110">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Dział edycji strony](concurrency/_static/edit-error.png)

![Strona usuwania działu](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="8892b-113">Konfliktom współbieżności</span><span class="sxs-lookup"><span data-stu-id="8892b-113">Concurrency conflicts</span></span>

<span data-ttu-id="8892b-114">Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki celu jego edycji, a następnie inny użytkownik aktualizuje dane tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-114">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="8892b-115">Nie włączaj wykrywania takie konflikty, ostatnio kto aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8892b-115">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="8892b-116">W wielu aplikacjach to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie są naprawdę krytyczne, jeśli pewne zmiany zostaną zastąpione, kosztów programowania dla współbieżności może przeważają korzyści.</span><span class="sxs-lookup"><span data-stu-id="8892b-116">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="8892b-117">W takim przypadku nie trzeba skonfigurować aplikację do obsługi konfliktom współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-117">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="8892b-118">Pesymistyczne współbieżności (blokowanie)</span><span class="sxs-lookup"><span data-stu-id="8892b-118">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="8892b-119">Jeśli aplikacja potrzeba uniknąć przypadkowej utraty danych w scenariuszach współbieżności, jeden sposób jest użycie blokady bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-119">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="8892b-120">Jest to pesymistyczne współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-120">This is called pessimistic concurrency.</span></span> <span data-ttu-id="8892b-121">Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub aktualizacji dostępu.</span><span class="sxs-lookup"><span data-stu-id="8892b-121">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="8892b-122">Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub aktualizacji dostępu, ponieważ uzyskują kopii danych, która jest w trakcie zmieniane.</span><span class="sxs-lookup"><span data-stu-id="8892b-122">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="8892b-123">Jeśli zablokujesz wiersz dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="8892b-123">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="8892b-124">Zarządzanie blokad ma wady.</span><span class="sxs-lookup"><span data-stu-id="8892b-124">Managing locks has disadvantages.</span></span> <span data-ttu-id="8892b-125">Może być skomplikowane, aby program.</span><span class="sxs-lookup"><span data-stu-id="8892b-125">It can be complex to program.</span></span> <span data-ttu-id="8892b-126">Wymaga to znaczące bazy danych zarządzania zasobami i może spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa.</span><span class="sxs-lookup"><span data-stu-id="8892b-126">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="8892b-127">Z tego względu nie wszystkie systemy zarządzania bazy danych obsługują pesymistyczne współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-127">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="8892b-128">Program Entity Framework Core nie zapewnia wbudowanej obsługi dla niego, a w tym samouczku nie pokazuje, jak ją wdrożyć.</span><span class="sxs-lookup"><span data-stu-id="8892b-128">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="8892b-129">Optymistycznej współbieżności</span><span class="sxs-lookup"><span data-stu-id="8892b-129">Optimistic Concurrency</span></span>

<span data-ttu-id="8892b-130">Alternatywą dla pesymistyczne współbieżności jest optymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-130">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="8892b-131">Optymistycznej współbieżności oznacza stosowanie konfliktom współbieżności mieć miejsce, a następnie reaguje odpowiednio Jeśli.</span><span class="sxs-lookup"><span data-stu-id="8892b-131">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="8892b-132">Na przykład Magdalena odwiedzających stronę Edytuj działu i zmienia wielkość budżetu dla angielskiej wersji językowej działu z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="8892b-132">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="8892b-134">Przed kliknie Magdalena **zapisać**, Jan odwiedza tej samej stronie i zmienia pole Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="8892b-134">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia 2013](concurrency/_static/change-date.png)

<span data-ttu-id="8892b-136">Magdalena kliknie **zapisać** pierwszy i widzi jej zmienić po powrocie do strony indeksu z przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8892b-136">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Budżet zmienione od zera.](concurrency/_static/budget-zero.png)

<span data-ttu-id="8892b-138">A następnie klika przycisk Jan **zapisać** na stronie edycji nadal pokazujący budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="8892b-138">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="8892b-139">Co dalej zależy od sposobu obsługi konfliktom współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-139">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="8892b-140">Niektóre opcje są następujące:</span><span class="sxs-lookup"><span data-stu-id="8892b-140">Some of the options include the following:</span></span>

* <span data-ttu-id="8892b-141">Można zachować informacje o właściwości, które zostało zmodyfikowane przez użytkownika i aktualizować tylko odpowiednie kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-141">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="8892b-142">W przykładowym scenariuszu żadne dane nie byłoby utracone, ponieważ inne właściwości zostały zaktualizowane przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8892b-142">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="8892b-143">Przy następnym ktoś przegląda w angielskiej wersji językowej działu, zobaczą zmiany zarówno Joanny i jego Jan — Data początkowa 9/1/2013 i budżetu dolarów zero.</span><span class="sxs-lookup"><span data-stu-id="8892b-143">The next time someone browses the English department, they will see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="8892b-144">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurują zmian z tą samą właściwością jednostki.</span><span class="sxs-lookup"><span data-stu-id="8892b-144">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="8892b-145">Czy programu Entity Framework działa w ten sposób zależy od sposobu implementacji kodu aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="8892b-145">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="8892b-146">Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu celu śledzenia wszystkich oryginalnej wartości właściwości dla obiektu, a także nowe wartości.</span><span class="sxs-lookup"><span data-stu-id="8892b-146">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="8892b-147">Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji ponieważ go wymaga zasobów serwera lub muszą być zawarte w strony sieci web (na przykład w pola ukryte) lub w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8892b-147">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="8892b-148">Możesz pozwolić, aby zmiany w Jan zastąpić zmiany nazwy.</span><span class="sxs-lookup"><span data-stu-id="8892b-148">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="8892b-149">Przy następnym ktoś przegląda w angielskiej wersji językowej działu, zobaczą 9/1/2013 i przywrócone wartości $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="8892b-149">The next time someone browses the English department, they will see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="8892b-150">Ta metoda jest wywoływana *klienta Wins* lub *ostatniego w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="8892b-150">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="8892b-151">(Wszystkie wartości z klienta wyższy priorytet niż co znajduje się w magazynie danych). Zgodnie z opisem w wprowadzenie do tej sekcji, w przeciwnym razie pisania kodu do obsługi współbieżności, nastąpi to automatycznie.</span><span class="sxs-lookup"><span data-stu-id="8892b-151">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="8892b-152">Aby uniemożliwić zmianę jego Jan aktualizację w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-152">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="8892b-153">Zazwyczaj będzie wyświetlony komunikat o błędzie, Pokaż jego bieżący stan danych i Zezwalaj, aby ponownie zastosować jego zmiany, jeśli chce nadal były.</span><span class="sxs-lookup"><span data-stu-id="8892b-153">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="8892b-154">Ta metoda jest wywoływana *Wins magazynu* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="8892b-154">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="8892b-155">(Wartości magazynu danych mają priorytet nad wartości przesłany przez klienta). W tym samouczku będziesz wdrożyć scenariusz dla magazynu usługi Wins.</span><span class="sxs-lookup"><span data-stu-id="8892b-155">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="8892b-156">Ta metoda gwarantuje, że żadne zmiany nie zostaną zastąpione bez użytkownika są alerty o tym, co dzieje.</span><span class="sxs-lookup"><span data-stu-id="8892b-156">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="8892b-157">Wykrywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="8892b-157">Detecting concurrency conflicts</span></span>

<span data-ttu-id="8892b-158">Obsługa może rozwiązać konflikty `DbConcurrencyException` wyjątki, które generuje programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8892b-158">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="8892b-159">Aby wiedzieć, kiedy throw te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów.</span><span class="sxs-lookup"><span data-stu-id="8892b-159">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="8892b-160">W związku z tym musisz skonfigurować bazę danych i modelu danych odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="8892b-160">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="8892b-161">Niektóre opcje umożliwiających wykrywanie konfliktów są następujące:</span><span class="sxs-lookup"><span data-stu-id="8892b-161">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="8892b-162">W tabeli bazy danych należy dołączyć kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="8892b-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="8892b-163">Następnie można skonfigurować programu Entity Framework w celu uwzględnienia tej kolumny w klauzuli Where klauzuli SQL Update lub Delete poleceń.</span><span class="sxs-lookup"><span data-stu-id="8892b-163">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="8892b-164">Typ danych kolumny śledzenia jest zwykle `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="8892b-164">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="8892b-165">`rowversion` Wartość jest liczbą sekwencyjnych, który jest zwiększany po każdej zaktualizować wiersza.</span><span class="sxs-lookup"><span data-stu-id="8892b-165">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="8892b-166">W poleceniu Update lub Delete gdzie klauzula zawiera oryginalnej wartości kolumny śledzenia (oryginalnej wersji wierszy).</span><span class="sxs-lookup"><span data-stu-id="8892b-166">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="8892b-167">Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc instrukcji Update lub Delete nie można odnaleźć wiersza do aktualizacji z powodu Where klauzuli.</span><span class="sxs-lookup"><span data-stu-id="8892b-167">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="8892b-168">Gdy znajdzie Entity Framework, że żadne wiersze nie zostały zaktualizowane przez aktualizacji lub usunięcia polecenia (Jeśli liczba wierszy, których dotyczy to zero), interpretuje który jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-168">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="8892b-169">Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości wszystkich kolumn w tabeli w klauzuli Where klauzuli poleceń Update i Delete.</span><span class="sxs-lookup"><span data-stu-id="8892b-169">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="8892b-170">Jak pierwsza opcja, jeśli dowolny wiersz zmieniła się od najpierw odczytano wiersza gdzie klauzuli nie zwrócą wiersza do aktualizacji, które programu Entity Framework interpretowane jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-170">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="8892b-171">Dla tabel bazy danych, które mają wiele kolumn, ta metoda może spowodować bardzo dużych Where klauzule i może wymagać, aby zachować stan dużych ilości.</span><span class="sxs-lookup"><span data-stu-id="8892b-171">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="8892b-172">Jak wspomniano wcześniej, obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8892b-172">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="8892b-173">W związku z tym tej metody zwykle nie jest zalecane, a nie jest ona metodę używaną w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="8892b-173">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="8892b-174">Jeśli chcesz wdrożyć takie podejście do współbieżności, masz Oznacz wszystkie właściwości bez klucza podstawowego w jednostce chcesz śledzić concurrency przez dodanie `ConcurrencyCheck` atrybutu do nich.</span><span class="sxs-lookup"><span data-stu-id="8892b-174">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="8892b-175">Taka zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w klauzuli SQL Where w instrukcji Update i Delete.</span><span class="sxs-lookup"><span data-stu-id="8892b-175">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="8892b-176">W pozostałej części tego samouczka zostanie dodana `rowversion` śledzenia właściwości jednostki działu, Tworzenie kontrolera i widoków i sprawdzenie, czy wszystko działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="8892b-176">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="8892b-177">Dodaj właściwość śledzenia do działu jednostki</span><span class="sxs-lookup"><span data-stu-id="8892b-177">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="8892b-178">W *Models/Department.cs*, Dodaj właściwość śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="8892b-178">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="8892b-179">`Timestamp` Atrybut określa, że w tej kolumnie będą uwzględniane w Where klauzuli Update i Delete polecenia wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-179">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="8892b-180">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server SQL `timestamp` — typ danych przed SQL `rowversion` on zastąpiony.</span><span class="sxs-lookup"><span data-stu-id="8892b-180">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="8892b-181">Typ architektury .NET dla `rowversion` jest tablicą bajtów.</span><span class="sxs-lookup"><span data-stu-id="8892b-181">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="8892b-182">Jeśli wolisz korzystać z interfejsu API fluent, możesz użyć `IsConcurrencyToken` — metoda (w *Data/SchoolContext.cs*) do określenia właściwości śledzenia, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8892b-182">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="8892b-183">Przez dodanie właściwości po zmianie modelu bazy danych, więc należy przeprowadzić migrację z innej.</span><span class="sxs-lookup"><span data-stu-id="8892b-183">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="8892b-184">Zapisz zmiany i skompiluj projekt, a następnie wprowadź następujące polecenia w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="8892b-184">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="8892b-185">Tworzenie kontrolera działów i widoków</span><span class="sxs-lookup"><span data-stu-id="8892b-185">Create a Departments controller and views</span></span>

<span data-ttu-id="8892b-186">Tak jak wcześniej dla uczniów lub studentów, szkoleń i instruktorów szkieletu kontrolera działów i widoków.</span><span class="sxs-lookup"><span data-stu-id="8892b-186">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Dział szkieletu](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="8892b-188">W *DepartmentsController.cs* pliku, zmienić wszystkie cztery wystąpienia "FirstMidName" na "Pełna nazwa", tak aby list rozwijanych administratora działu będzie zawierać pełną nazwę instruktora, a nie tylko nazwisko.</span><span class="sxs-lookup"><span data-stu-id="8892b-188">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="8892b-189">Aktualizowanie widoku indeksu działów</span><span class="sxs-lookup"><span data-stu-id="8892b-189">Update the Departments Index view</span></span>

<span data-ttu-id="8892b-190">Aparat szkieletów utworzył RowVersion kolumny w widoku indeksu, ale nie powinny być wyświetlane tego pola.</span><span class="sxs-lookup"><span data-stu-id="8892b-190">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="8892b-191">Zastąp kod w *Views/Departments/Index.cshtml* następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="8892b-191">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="8892b-192">Zmiany pozycji do "Działów", usuwa kolumnę RowVersion i zawiera pełną nazwę zamiast imię dla administratora.</span><span class="sxs-lookup"><span data-stu-id="8892b-192">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="8892b-193">Metody edycji w kontrolerze działów aktualizacji</span><span class="sxs-lookup"><span data-stu-id="8892b-193">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="8892b-194">W obu HttpGet `Edit` — metoda i `Details` metody, Dodaj `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="8892b-194">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="8892b-195">W HttpGet `Edit` metody, Dodaj wczesny ładowania dla administratora.</span><span class="sxs-lookup"><span data-stu-id="8892b-195">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="8892b-196">Zastąp istniejący kod httppost `Edit` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8892b-196">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="8892b-197">Na początku kodu próby odczytu z działu do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="8892b-197">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="8892b-198">Jeśli `SingleOrDefaultAsync` metoda zwraca wartość null, dział został usunięty przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8892b-198">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="8892b-199">W takim przypadku ten kod używa wartości przesłanego formularza utworzyć jednostki działu, tak aby edycji strony mogą być wyświetlane ponownie z komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="8892b-199">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="8892b-200">Alternatywnie nie trzeba ponownie utworzyć jednostki działu, jeśli wyświetla komunikat o błędzie bez ponowne wyświetlanie pola działu.</span><span class="sxs-lookup"><span data-stu-id="8892b-200">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="8892b-201">Widok przechowuje oryginalnej `RowVersion` wartość w ukrytym polu i ta metoda odbiera tę wartość w `rowVersion` parametru.</span><span class="sxs-lookup"><span data-stu-id="8892b-201">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="8892b-202">Przed wywołaniem `SaveChanges`, trzeba umieścić który oryginalnego `RowVersion` wartości właściwości w `OriginalValues` kolekcji jednostki.</span><span class="sxs-lookup"><span data-stu-id="8892b-202">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="8892b-203">Następnie podczas programu Entity Framework utworzy polecenia aktualizacji SQL, to polecenie będzie zawierać klauzuli WHERE, sprawdzający wiersza, który ma oryginalną `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="8892b-203">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="8892b-204">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (żadnych wierszy ma oryginalną `RowVersion` wartość), programu Entity Framework zgłasza `DbUpdateConcurrencyException` wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8892b-204">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="8892b-205">Kod w bloku catch dla tego wyjątku pobiera dotyczy jednostki działu, która ma zaktualizowanej wartości z `Entries` właściwości w obiekcie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8892b-205">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="8892b-206">`Entries` Kolekcja będzie mieć tylko jedno `EntityEntry` obiektu.</span><span class="sxs-lookup"><span data-stu-id="8892b-206">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="8892b-207">Ten obiekt służy do nowych wartości wprowadzonej przez użytkownika i wartości bieżącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-207">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="8892b-208">Ten kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma różne wartości bazy danych z wprowadzoną na edycję użytkownika strony (tylko jedno pole znajduje się tutaj w celu jego skrócenia).</span><span class="sxs-lookup"><span data-stu-id="8892b-208">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="8892b-209">Na koniec kod ustawia `RowVersion` wartość `departmentToUpdate` do nowej wartości pobrane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-209">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="8892b-210">Nowy `RowVersion` wartości będą przechowywane w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następne czasu użytkownik klika polecenie **zapisać**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie edycji strony zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="8892b-210">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="8892b-211">`ModelState.Remove` Instrukcja jest wymagane, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="8892b-211">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="8892b-212">W widoku `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości w modelu, gdy istnieją obie.</span><span class="sxs-lookup"><span data-stu-id="8892b-212">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="8892b-213">Aktualizacja działu Edytuj widok</span><span class="sxs-lookup"><span data-stu-id="8892b-213">Update the Department Edit view</span></span>

<span data-ttu-id="8892b-214">W *Views/Departments/Edit.cshtml*, wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="8892b-214">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="8892b-215">Dodaj ukryte pole, aby zapisać `RowVersion` wartość właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości.</span><span class="sxs-lookup"><span data-stu-id="8892b-215">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="8892b-216">Dodaj opcję "Wybierz administratora" do listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="8892b-216">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="8892b-217">Testowanie konfliktom współbieżności na stronie edycji</span><span class="sxs-lookup"><span data-stu-id="8892b-217">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="8892b-218">Uruchom aplikację i przejdź do strony indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="8892b-218">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="8892b-219">Kliknij prawym przyciskiem myszy **Edytuj** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie**, następnie kliknij przycisk **Edytuj** hyperlink działu angielskiej wersji językowej.</span><span class="sxs-lookup"><span data-stu-id="8892b-219">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="8892b-220">Karty przeglądarki dwóch jest teraz wyświetlany tych samych informacji.</span><span class="sxs-lookup"><span data-stu-id="8892b-220">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="8892b-221">Zmień pola w pierwszej karcie przeglądarki i kliknij **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="8892b-221">Change a field in the first browser tab and click **Save**.</span></span>

![Edytowanie działu strony 1 po zmianie](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="8892b-223">Przeglądarka wyświetla stronę indeksu o wartości zmienione.</span><span class="sxs-lookup"><span data-stu-id="8892b-223">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="8892b-224">Zmień pola w drugiej karty przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8892b-224">Change a field in the second browser tab.</span></span>

![Edytowanie działu strony 2 po zmianie](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="8892b-226">Kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="8892b-226">Click **Save**.</span></span> <span data-ttu-id="8892b-227">Zostanie wyświetlony komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="8892b-227">You see an error message:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="8892b-229">Kliknij przycisk **zapisać** ponownie.</span><span class="sxs-lookup"><span data-stu-id="8892b-229">Click **Save** again.</span></span> <span data-ttu-id="8892b-230">Wartość wprowadzona w drugiej karty przeglądarki jest zapisywana.</span><span class="sxs-lookup"><span data-stu-id="8892b-230">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="8892b-231">Zostanie wyświetlony zapisanych wartości, gdy zostanie wyświetlona strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="8892b-231">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="8892b-232">Zaktualizuj strony usuwania</span><span class="sxs-lookup"><span data-stu-id="8892b-232">Update the Delete page</span></span>

<span data-ttu-id="8892b-233">Na stronie usuwania programu Entity Framework wykrywa konfliktom współbieżności spowodowane przez kogoś else edycji dział w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="8892b-233">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="8892b-234">Gdy HttpGet `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w polu ukrytym.</span><span class="sxs-lookup"><span data-stu-id="8892b-234">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="8892b-235">Czy wartość jest następnie udostępniana HttpPost `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdza usunięcie.</span><span class="sxs-lookup"><span data-stu-id="8892b-235">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="8892b-236">Entity Framework utworzy polecenia SQL DELETE, zawiera klauzulę WHERE z oryginalną `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="8892b-236">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="8892b-237">Jeśli wyniki poleceń w żadnych wierszy dotyczy (znaczenie wiersz został zmieniony po stronie potwierdzenia usunięcia została wyświetlona), zwracany jest wyjątek współbieżności, a HttpGet `Delete` metoda jest wywoływana z ustawioną flagą błąd na true, aby ponownie wyświetlić Strona potwierdzenia z komunikatem o błędzie.</span><span class="sxs-lookup"><span data-stu-id="8892b-237">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="8892b-238">Istnieje również możliwość, że zero wierszy została zmieniona, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku jest wyświetlany żaden komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="8892b-238">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="8892b-239">Metody Delete w kontrolerze działów aktualizacji</span><span class="sxs-lookup"><span data-stu-id="8892b-239">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="8892b-240">W *DepartmentsController.cs*, Zastąp HttpGet `Delete` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8892b-240">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="8892b-241">Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-241">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="8892b-242">Jeśli ta flaga ma wartość true, a dział określono już nie istnieje, został usunięty przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8892b-242">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="8892b-243">W takim przypadku kod przekierowuje do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="8892b-243">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="8892b-244">Jeśli ta flaga ma wartość true, a dział istnieje, został zmieniony przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8892b-244">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="8892b-245">W takim przypadku kod wysyła komunikat o błędzie do widoku przy użyciu `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="8892b-245">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="8892b-246">Zastąp kod w HttpPost `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8892b-246">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="8892b-247">W kodzie szkieletu po prostu zastąpić ta metoda zaakceptowane identyfikator rekordu:</span><span class="sxs-lookup"><span data-stu-id="8892b-247">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="8892b-248">Ten parametr zmieniono do wystąpienia jednostki działu utworzone przez integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="8892b-248">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="8892b-249">To zapewnia EF dostęp do wartości właściwości RowVersion oprócz klucza rekordu.</span><span class="sxs-lookup"><span data-stu-id="8892b-249">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="8892b-250">Również zmieniono nazwę metody akcji `DeleteConfirmed` do `Delete`.</span><span class="sxs-lookup"><span data-stu-id="8892b-250">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="8892b-251">Kod z utworzonym szkieletem użyta nazwa `DeleteConfirmed` umożliwiają metody HttpPost unikatowego podpisu.</span><span class="sxs-lookup"><span data-stu-id="8892b-251">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="8892b-252">(CLR wymaga przeciążonej metody mają parametry innej metody). Teraz, czy podpisy są unikatowe, można przestrzegaj Konwencji MVC i używać tej samej nazwy dla metody delete HttpPost i HttpGet.</span><span class="sxs-lookup"><span data-stu-id="8892b-252">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="8892b-253">Jeśli dział został już usunięty, `AnyAsync` metoda zwraca wartość false i aplikacji po prostu wraca do metody indeksu.</span><span class="sxs-lookup"><span data-stu-id="8892b-253">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="8892b-254">Jeśli zostanie przechwycony błąd współbieżności, kod zostanie ponownie stronę potwierdzenia Delete i udostępnia Flaga, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-254">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="8892b-255">Aktualizowanie widoku Delete</span><span class="sxs-lookup"><span data-stu-id="8892b-255">Update the Delete view</span></span>

<span data-ttu-id="8892b-256">W *Views/Departments/Delete.cshtml*, Zastąp następujący kod, który dodaje błąd pola wiadomości i ukryte pola dla właściwości DepartmentID i RowVersion szkieletu kodu.</span><span class="sxs-lookup"><span data-stu-id="8892b-256">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="8892b-257">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="8892b-257">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="8892b-258">Dzięki temu następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="8892b-258">This makes the following changes:</span></span>

* <span data-ttu-id="8892b-259">Dodaje komunikat o błędzie między `h2` i `h3` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="8892b-259">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="8892b-260">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="8892b-260">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="8892b-261">Usuwa pole RowVersion.</span><span class="sxs-lookup"><span data-stu-id="8892b-261">Removes the RowVersion field.</span></span>

* <span data-ttu-id="8892b-262">Dodaje ukryte pole umożliwiające `RowVersion` właściwości.</span><span class="sxs-lookup"><span data-stu-id="8892b-262">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="8892b-263">Uruchom aplikację i przejdź do strony indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="8892b-263">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="8892b-264">Kliknij prawym przyciskiem myszy **usunąć** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie**, następnie na karcie pierwszy kliknij **Edytuj** hyperlink działu angielskiej wersji językowej.</span><span class="sxs-lookup"><span data-stu-id="8892b-264">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="8892b-265">W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **zapisać**:</span><span class="sxs-lookup"><span data-stu-id="8892b-265">In the first window, change one of the values, and click **Save**:</span></span>

![Dział edycji strony po zmianie przed usunięciem](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="8892b-267">Na karcie drugi kliknij **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="8892b-267">In the second tab, click **Delete**.</span></span> <span data-ttu-id="8892b-268">Zostanie wyświetlony komunikat o błędzie współbieżności, a dział wartości są odświeżane co to jest obecnie w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8892b-268">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Strona potwierdzenia usunięcia działu błąd współbieżności](concurrency/_static/delete-error.png)

<span data-ttu-id="8892b-270">Jeśli klikniesz przycisk **usunąć** ponownie, że przekierowanie do strony indeksu, który pokazuje, czy dział została usunięta.</span><span class="sxs-lookup"><span data-stu-id="8892b-270">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="8892b-271">Szczegółowe informacje dotyczące aktualizacji i tworzyć widoki</span><span class="sxs-lookup"><span data-stu-id="8892b-271">Update Details and Create views</span></span>

<span data-ttu-id="8892b-272">Opcjonalnie można wyczyścić szkieletu kodu w szczegółach i tworzyć widoki.</span><span class="sxs-lookup"><span data-stu-id="8892b-272">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="8892b-273">Zastąp kod w *Views/Departments/Details.cshtml* Aby usunąć kolumnę RowVersion i wyświetlić pełną nazwę administratora.</span><span class="sxs-lookup"><span data-stu-id="8892b-273">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="8892b-274">Zastąp kod w *Views/Departments/Create.cshtml* do dodania do listy rozwijanej wybierz opcję.</span><span class="sxs-lookup"><span data-stu-id="8892b-274">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="8892b-275">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8892b-275">Summary</span></span>

<span data-ttu-id="8892b-276">Na tym kończy się wprowadzenie do obsługi konfliktom współbieżności.</span><span class="sxs-lookup"><span data-stu-id="8892b-276">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="8892b-277">Aby uzyskać więcej informacji na temat obsługi współbieżność w EF Core, zobacz [konfliktom współbieżności](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="8892b-277">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="8892b-278">Następny samouczek pokazuje, jak do zaimplementowania tabeli na hierarchii dziedziczenia dla jednostek instruktora i uczniów.</span><span class="sxs-lookup"><span data-stu-id="8892b-278">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8892b-279">[Poprzednie](update-related-data.md)
> [dalej](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="8892b-279">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
