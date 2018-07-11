---
title: Platforma ASP.NET Core MVC z programem EF Core — współbieżności - 8, 10
author: rick-anderson
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 9bf65621213c9657232dfff1701c9937d5105a9c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186640"
---
# <a name="aspnet-core-mvc-with-ef-core---concurrency---8-of-10"></a><span data-ttu-id="71ed3-103">Platforma ASP.NET Core MVC z programem EF Core — współbieżności - 8, 10</span><span class="sxs-lookup"><span data-stu-id="71ed3-103">ASP.NET Core MVC with EF Core - Concurrency - 8 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="71ed3-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71ed3-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71ed3-105">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71ed3-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="71ed3-106">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="71ed3-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="71ed3-107">W samouczkach wcześniej przedstawiono sposób zaktualizować dane.</span><span class="sxs-lookup"><span data-stu-id="71ed3-107">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="71ed3-108">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-108">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="71ed3-109">Instrukcje dotyczące tworzenia stron sieci web, które współpracują z jednostką działu i obsługi błędów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-109">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="71ed3-110">Na poniższych ilustracjach przedstawiono edytowanie i usuwanie stron, w tym niektóre komunikaty, które są wyświetlane, jeśli wystąpi konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-110">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Strona edytowania działu](concurrency/_static/edit-error.png)

![Strona usuwanie działu](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="71ed3-113">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="71ed3-113">Concurrency conflicts</span></span>

<span data-ttu-id="71ed3-114">Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki w celu edycji, a następnie inny użytkownik aktualizuje dane w tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="71ed3-114">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="71ed3-115">Jeśli nie włączysz wykrywania takie konflikty, ostatnia spojrzenia aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="71ed3-115">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="71ed3-116">W wielu aplikacjach, to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie jest tak naprawdę krytycznego, jeśli niektóre zmiany są zastępowane, koszt programowania współbieżność może przeważają korzyści.</span><span class="sxs-lookup"><span data-stu-id="71ed3-116">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="71ed3-117">W takiej sytuacji nie trzeba skonfigurować aplikację do obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-117">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="71ed3-118">Współbieżność pesymistyczna (blokowanie)</span><span class="sxs-lookup"><span data-stu-id="71ed3-118">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="71ed3-119">Jeśli Twoja aplikacja wymaga uniknąć przypadkowej utraty danych w scenariuszach współbieżności, używają blokady bazy danych jest jednym ze sposobów, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="71ed3-119">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="71ed3-120">Jest to nazywane pesymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-120">This is called pessimistic concurrency.</span></span> <span data-ttu-id="71ed3-121">Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub uzyskać dostęp do aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="71ed3-121">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="71ed3-122">Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub zaktualizować dostępu, ponieważ jaką uzyskują kopii danych, która jest w trakcie zmieniany.</span><span class="sxs-lookup"><span data-stu-id="71ed3-122">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="71ed3-123">Jeśli zablokujesz wiersz, aby uzyskać dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="71ed3-123">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="71ed3-124">Zarządzanie blokady ma wady.</span><span class="sxs-lookup"><span data-stu-id="71ed3-124">Managing locks has disadvantages.</span></span> <span data-ttu-id="71ed3-125">Może być skomplikowane, aby program.</span><span class="sxs-lookup"><span data-stu-id="71ed3-125">It can be complex to program.</span></span> <span data-ttu-id="71ed3-126">Wymaga znaczących bazy danych zarządzania zasobami, a może to spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa się.</span><span class="sxs-lookup"><span data-stu-id="71ed3-126">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="71ed3-127">Z tego względu nie wszystkie systemy zarządzania bazami danych obsługuje pesymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-127">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="71ed3-128">Entity Framework Core nie obsługuje wbudowanej go, a w tym samouczku nie pokazano, jak ją wdrożyć.</span><span class="sxs-lookup"><span data-stu-id="71ed3-128">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="71ed3-129">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="71ed3-129">Optimistic Concurrency</span></span>

<span data-ttu-id="71ed3-130">Alternatywa dla Współbieżność pesymistyczna jest optymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-130">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="71ed3-131">Optymistyczna współbieżność oznacza, umożliwiając konfliktów współbieżności do wykonania, a następnie reagowaniu odpowiednio Jeśli tak jest.</span><span class="sxs-lookup"><span data-stu-id="71ed3-131">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="71ed3-132">Na przykład Magdalena odwiedzin strony Edytuj działu i zmienia kwota budżetu dla angielskiego działu z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="71ed3-132">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="71ed3-134">Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="71ed3-134">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

<span data-ttu-id="71ed3-136">Magdalena kliknie **Zapisz** pierwszy i widzi jej zmieniać, gdy przeglądarka powróci do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-136">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Budżet na zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="71ed3-138">A następnie kliknie John **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="71ed3-138">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="71ed3-139">Co dzieje się potem określają sposób obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-139">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="71ed3-140">Niektóre opcje obejmują następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="71ed3-140">Some of the options include the following:</span></span>

* <span data-ttu-id="71ed3-141">Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="71ed3-141">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="71ed3-142">W przykładowym scenariuszu żadne dane nie zostałyby utracone, ponieważ inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="71ed3-142">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="71ed3-143">Przy następnym ktoś przegląda angielskiej działu, zobaczą zmiany nazwy i John's — datę rozpoczęcia 9/1/2013 i budżetu, zerowego dolarów.</span><span class="sxs-lookup"><span data-stu-id="71ed3-143">The next time someone browses the English department, they will see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="71ed3-144">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostały wprowadzone w tej samej właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="71ed3-144">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="71ed3-145">Czy platformy Entity Framework działa w ten sposób zależy od tego, jak zaimplementować kod aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="71ed3-145">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="71ed3-146">Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu w celu śledzenia oryginalnych wartości właściwości dla jednostki, a także nowe wartości.</span><span class="sxs-lookup"><span data-stu-id="71ed3-146">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="71ed3-147">Obsługa dużych ilości stanu może mieć wpływ na wydajność aplikacji, ponieważ jego wymaga zasobów serwera albo muszą być zawarte na stronie sieci web (na przykład w ukrytych polach) lub w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-147">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="71ed3-148">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="71ed3-148">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="71ed3-149">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i przywrócone wartości $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="71ed3-149">The next time someone browses the English department, they will see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="71ed3-150">Jest to nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="71ed3-150">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="71ed3-151">(Wszystkie wartości z klienta pierwszeństwo co znajduje się w magazynie danych.) Jak wspomniano we wprowadzeniu do tej sekcji, w przeciwnym razie pisania obsługi współbieżności, nastąpi to automatycznie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-151">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="71ed3-152">Aby uniemożliwić zmiany John's aktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="71ed3-152">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="71ed3-153">Zazwyczaj będzie wyświetlony komunikat o błędzie, pokazują, jak bieżący stan danych i umożliwiające podszycie się ponownie zastosuj swoje zmiany, jeśli chce nadal były.</span><span class="sxs-lookup"><span data-stu-id="71ed3-153">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="71ed3-154">Jest to nazywane *Store Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="71ed3-154">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="71ed3-155">(Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) Będzie implementować scenariusza Store Wins w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="71ed3-155">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="71ed3-156">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu z wydarzeniami.</span><span class="sxs-lookup"><span data-stu-id="71ed3-156">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="71ed3-157">Wykrywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="71ed3-157">Detecting concurrency conflicts</span></span>

<span data-ttu-id="71ed3-158">Należy rozwiązać konflikty, obsługując `DbConcurrencyException` wyjątki, które zgłasza Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="71ed3-158">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="71ed3-159">Aby wiedzieć, kiedy trzeba generować te wyjątki, platformy Entity Framework musi umożliwiać wykrywanie konfliktów.</span><span class="sxs-lookup"><span data-stu-id="71ed3-159">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="71ed3-160">W związku z tym należy skonfigurować bazy danych oraz model danych odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="71ed3-160">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="71ed3-161">Niektóre opcje umożliwiające wykrywanie konfliktów są następujące:</span><span class="sxs-lookup"><span data-stu-id="71ed3-161">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="71ed3-162">W tabeli bazy danych zawierają kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="71ed3-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="71ed3-163">Następnie można skonfigurować programu Entity Framework, aby uwzględnić tej kolumny w klauzuli Where klauzuli SQL Update lub Delete poleceń.</span><span class="sxs-lookup"><span data-stu-id="71ed3-163">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="71ed3-164">Typ danych kolumny śledzenia jest zazwyczaj `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="71ed3-164">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="71ed3-165">`rowversion` Wartość numeru sekwencyjnego, który jest zwiększana za każdym razem, zaktualizować wiersza.</span><span class="sxs-lookup"><span data-stu-id="71ed3-165">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="71ed3-166">W poleceniu Update lub Delete gdzie klauzula zawiera oryginalna wartość kolumny śledzenia (oryginalna wersja wiersza).</span><span class="sxs-lookup"><span data-stu-id="71ed3-166">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="71ed3-167">Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc instrukcji Update lub Delete nie można odnaleźć wiersza do zaktualizowania ze względu na miejsce klauzuli.</span><span class="sxs-lookup"><span data-stu-id="71ed3-167">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="71ed3-168">Gdy znajdzie Entity Framework, że żadne wiersze nie zostały zaktualizowane, Update lub Delete polecenie (to znaczy, gdy liczba wierszy, których to dotyczy, jest równy zero), interpretuje, jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-168">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="71ed3-169">Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości każdej kolumny w tabeli w klauzuli Where klauzuli polecenia Update i Delete.</span><span class="sxs-lookup"><span data-stu-id="71ed3-169">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="71ed3-170">Tak jak w pierwszej opcji, jeśli nic w wierszu zmieniła się od wiersza została najpierw przeczytać artykuł gdzie klauzula nie zwraca wiersz, aby zaktualizować, której platformy Entity Framework interpretuje jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-170">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="71ed3-171">Dla tabel bazy danych, które mają wiele kolumn, to podejście może doprowadzić do bardzo dużych gdzie zdań i może wymagać obsługi dużych ilości stanu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-171">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="71ed3-172">Jak wspomniano wcześniej, obsługi dużych ilości stanu mogą wpływać na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71ed3-172">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="71ed3-173">W związku z tym to podejście zazwyczaj nie zaleca się i nie można go metodę używaną w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="71ed3-173">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="71ed3-174">Jeśli chcesz zaimplementować to podejście do współbieżności, musisz oznaczyć wszystkie właściwości bez klucza podstawowego w jednostce, które chcesz śledzić concurrency, dodając `ConcurrencyCheck` atrybutu do nich.</span><span class="sxs-lookup"><span data-stu-id="71ed3-174">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="71ed3-175">Ta zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w klauzuli SQL Where w instrukcji Update i Delete.</span><span class="sxs-lookup"><span data-stu-id="71ed3-175">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="71ed3-176">W pozostałej części tego samouczka dodasz `rowversion` śledzenie właściwości do jednostki działu, Utwórz kontrolera i widoki, a test, aby sprawdzić, czy wszystko działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-176">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="71ed3-177">Dodawanie właściwości śledzenia do jednostki działu</span><span class="sxs-lookup"><span data-stu-id="71ed3-177">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="71ed3-178">W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="71ed3-178">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="71ed3-179">`Timestamp` Atrybut określa, że ta kolumna będzie zawierać w Where klauzuli Update i Delete polecenia wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="71ed3-179">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="71ed3-180">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` zastąpiono ją.</span><span class="sxs-lookup"><span data-stu-id="71ed3-180">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="71ed3-181">Typ architektury .NET dla `rowversion` jest tablicą bajtów.</span><span class="sxs-lookup"><span data-stu-id="71ed3-181">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="71ed3-182">Jeśli wolisz używać interfejsu API fluent, możesz użyć `IsConcurrencyToken` — metoda (w *Data/SchoolContext.cs*) można określić właściwości śledzenia, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="71ed3-182">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="71ed3-183">Przez dodanie właściwości po zmianie modelu bazy danych, więc należy przeprowadzić migrację z innej.</span><span class="sxs-lookup"><span data-stu-id="71ed3-183">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="71ed3-184">Zapisz zmiany i skompiluj projekt, a następnie wprowadź następujące polecenia w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="71ed3-184">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="71ed3-185">Tworzenie kontrolera działów i widoków</span><span class="sxs-lookup"><span data-stu-id="71ed3-185">Create a Departments controller and views</span></span>

<span data-ttu-id="71ed3-186">Jak wcześniej dla uczniów, kursy i instruktorów, tworzenia szkieletu kontrolera działów i widoków.</span><span class="sxs-lookup"><span data-stu-id="71ed3-186">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Tworzenie szkieletu działu](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="71ed3-188">W *DepartmentsController.cs* plików, zmienić wszystkie cztery wystąpienia "FirstMidName" na "Imię i nazwisko", tak, aby list rozwijanych administratora działu będzie zawierać imię i nazwisko instruktora, a nie po prostu nazwisko.</span><span class="sxs-lookup"><span data-stu-id="71ed3-188">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="71ed3-189">Aktualizacja widoku indeks działów</span><span class="sxs-lookup"><span data-stu-id="71ed3-189">Update the Departments Index view</span></span>

<span data-ttu-id="71ed3-190">Aparat tworzenia szkieletów utworzył RowVersion kolumny w widoku indeksu, ale nie powinny być wyświetlane to pole.</span><span class="sxs-lookup"><span data-stu-id="71ed3-190">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="71ed3-191">Zastąp kod w *Views/Departments/Index.cshtml* następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="71ed3-191">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="71ed3-192">Zmiany pozycji "Wydziałom", usuwa kolumnę RowVersion i pokazuje pełną nazwę zamiast imię administratora.</span><span class="sxs-lookup"><span data-stu-id="71ed3-192">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="71ed3-193">Aktualizacja metod edycji w kontrolerze działów</span><span class="sxs-lookup"><span data-stu-id="71ed3-193">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="71ed3-194">W obu narzędzia HttpGet `Edit` metody i `Details` metody, Dodaj `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="71ed3-194">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="71ed3-195">W HttpGet `Edit` metody, Dodaj wczesne ładowanie dla administratora.</span><span class="sxs-lookup"><span data-stu-id="71ed3-195">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="71ed3-196">Zastąp istniejący kod httppost `Edit` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="71ed3-196">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="71ed3-197">Na początku kodu próby odczytu z działu do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="71ed3-197">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="71ed3-198">Jeśli `SingleOrDefaultAsync` metoda zwraca wartość null, dział został usunięty przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="71ed3-198">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="71ed3-199">W takiej sytuacji kod używa wartości przesłanego formularza, aby utworzyć jednostki dział strony edytowania mogą być wyświetlane ponownie komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-199">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="71ed3-200">Jako alternatywę nie trzeba ponownie utworzyć jednostki działu, jeśli wyświetla komunikat o błędzie bez wyświetlania pól działu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-200">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="71ed3-201">Widok przechowuje oryginalny `RowVersion` wartości w ukrytym polu, a ta metoda odbiera tę wartość w `rowVersion` parametru.</span><span class="sxs-lookup"><span data-stu-id="71ed3-201">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="71ed3-202">Przed wywołaniem `SaveChanges`, musisz umieścić te oryginalnego `RowVersion` wartości właściwości w `OriginalValues` kolekcji jednostki.</span><span class="sxs-lookup"><span data-stu-id="71ed3-202">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="71ed3-203">Następnie podczas Entity Framework tworzy polecenie pliki aktualizacji programu SQL, to polecenie będzie zawierać klauzuli WHERE, który wyszukuje dla wiersza zawierającego oryginalne `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="71ed3-203">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="71ed3-204">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (żadnych wierszy ma oryginalny `RowVersion` wartość), platformy Entity Framework zgłasza `DbUpdateConcurrencyException` wyjątek.</span><span class="sxs-lookup"><span data-stu-id="71ed3-204">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="71ed3-205">Kod w bloku catch dla tego wyjątku pobiera dotyczy jednostki działu, która ma zaktualizowanymi wartościami z `Entries` właściwość obiektu wyjątku.</span><span class="sxs-lookup"><span data-stu-id="71ed3-205">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="71ed3-206">`Entries` Kolekcja będzie mieć tylko jeden `EntityEntry` obiektu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-206">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="71ed3-207">Aby uzyskać nowe wartości wprowadzonej przez użytkownika i wartości bieżącej bazy danych, można użyć tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-207">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="71ed3-208">Ten kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma różne wartości bazy danych, z jakiego które użytkownik wprowadził w edycji strony (tylko jedno pole znajduje się w tym miejscu dla zwięzłości).</span><span class="sxs-lookup"><span data-stu-id="71ed3-208">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="71ed3-209">Na koniec kod ustawia `RowVersion` wartość `departmentToUpdate` na nową wartość pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="71ed3-209">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="71ed3-210">Ta nowa `RowVersion` wartość zostanie zapisana w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następnie czasu użytkownik klika polecenie **Zapisz**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie strony edycji zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="71ed3-210">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="71ed3-211">`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="71ed3-211">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="71ed3-212">W widoku `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.</span><span class="sxs-lookup"><span data-stu-id="71ed3-212">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="71ed3-213">Aktualizacja widoku edycji działu</span><span class="sxs-lookup"><span data-stu-id="71ed3-213">Update the Department Edit view</span></span>

<span data-ttu-id="71ed3-214">W *Views/Departments/Edit.cshtml*, wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="71ed3-214">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="71ed3-215">Dodaj pole ukryte, aby zapisać `RowVersion` wartości właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości.</span><span class="sxs-lookup"><span data-stu-id="71ed3-215">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="71ed3-216">Dodaj opcję "Zaznacz administratora" do listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="71ed3-216">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="71ed3-217">Badanie konfliktów współbieżności na stronie edycji</span><span class="sxs-lookup"><span data-stu-id="71ed3-217">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="71ed3-218">Uruchom aplikację i przejdź do strony indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="71ed3-218">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="71ed3-219">Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**, następnie kliknij przycisk **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-219">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="71ed3-220">Karty przeglądarki dwa są teraz wyświetlane w tych samych informacji.</span><span class="sxs-lookup"><span data-stu-id="71ed3-220">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="71ed3-221">Zmień pole na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="71ed3-221">Change a field in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="71ed3-223">Przeglądarka wyświetla stronę indeksu wartością zmienione.</span><span class="sxs-lookup"><span data-stu-id="71ed3-223">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="71ed3-224">Zmień pole na drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="71ed3-224">Change a field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="71ed3-226">Kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="71ed3-226">Click **Save**.</span></span> <span data-ttu-id="71ed3-227">Zobaczysz komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="71ed3-227">You see an error message:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="71ed3-229">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-229">Click **Save** again.</span></span> <span data-ttu-id="71ed3-230">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="71ed3-230">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="71ed3-231">Zobaczysz zapisane wartości, gdy zostanie wyświetlona strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-231">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="71ed3-232">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="71ed3-232">Update the Delete page</span></span>

<span data-ttu-id="71ed3-233">Na stronie usuwania programu Entity Framework wykrywa konfliktów współbieżności spowodowanych przez osoby z działu inne do edycji w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="71ed3-233">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="71ed3-234">Gdy HttpGet `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w ukrytym polu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-234">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="71ed3-235">Czy wartość jest następnie udostępniana HttpPost `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdzi usunięcie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-235">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="71ed3-236">Entity Framework tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z oryginalnym `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="71ed3-236">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="71ed3-237">Jeśli wyniki polecenia zero wierszy dotyczy (wiersz został zmieniony po stronie potwierdzenia usunięcia został wyświetlony przesyłane), zwracany jest wyjątek współbieżności i narzędzia HttpGet `Delete` metoda jest wywoływana z ustawioną flagą błąd wartość true, aby ponownie wyświetlić Strona potwierdzenia komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-237">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="71ed3-238">Istnieje również możliwość, że zero wierszy została zmieniona, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku jest wyświetlany żaden komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="71ed3-238">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="71ed3-239">Aktualizacja metod usuwania w kontrolerze działów</span><span class="sxs-lookup"><span data-stu-id="71ed3-239">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="71ed3-240">W *DepartmentsController.cs*, Zastąp HttpGet `Delete` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="71ed3-240">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="71ed3-241">Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-241">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="71ed3-242">Jeśli ta flaga ma wartość true, a dział określono już istnieje, zostało usunięte przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="71ed3-242">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="71ed3-243">W takiej sytuacji kod wykonuje przekierowanie do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-243">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="71ed3-244">Jeśli ta flaga ma wartość true, a dział istnieje, został zmieniony przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="71ed3-244">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="71ed3-245">W takiej sytuacji kod wysyła komunikat o błędzie na widok za pomocą `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="71ed3-245">In that case, the code sends an error message to the view using `ViewData`.</span></span>

<span data-ttu-id="71ed3-246">Zastąp kod w HttpPost `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="71ed3-246">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="71ed3-247">W utworzony szkielet kodu, który został zastąpiony ta metoda akceptowane tylko identyfikator rekordu:</span><span class="sxs-lookup"><span data-stu-id="71ed3-247">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="71ed3-248">Tego parametru zostało zmienione na wystąpienie jednostki działu utworzone przez integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-248">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="71ed3-249">Daje to EF dostęp do wartości właściwości RowVersion oprócz klucza rekordu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-249">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="71ed3-250">Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`.</span><span class="sxs-lookup"><span data-stu-id="71ed3-250">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="71ed3-251">Utworzony szkielet kodu użyto nazwy `DeleteConfirmed` zapewnienie metody HttpPost unikatowy podpis.</span><span class="sxs-lookup"><span data-stu-id="71ed3-251">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="71ed3-252">(Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyć tej samej nazwy dla metody usuwania HttpPost i narzędzia HttpGet.</span><span class="sxs-lookup"><span data-stu-id="71ed3-252">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="71ed3-253">Jeśli dział został już usunięty, `AnyAsync` metoda zwraca wartość false, oraz aplikacji po prostu powraca do metody indeksu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-253">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="71ed3-254">Jeżeli zostanie przechwycony błąd współbieżności, kod zostanie ponownie strona potwierdzenia usunięcia i zapewnia, że flagę, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-254">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="71ed3-255">Aktualizacja widoku Delete</span><span class="sxs-lookup"><span data-stu-id="71ed3-255">Update the Delete view</span></span>

<span data-ttu-id="71ed3-256">W *Views/Departments/Delete.cshtml*, Zastąp następujący kod, który dodaje błąd pola komunikatu i ukrytych pól właściwości DepartmentID i RowVersion utworzony szkielet kodu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-256">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="71ed3-257">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="71ed3-257">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="71ed3-258">To sprawia, że następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="71ed3-258">This makes the following changes:</span></span>

* <span data-ttu-id="71ed3-259">Dodaje komunikat o błędzie między `h2` i `h3` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="71ed3-259">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="71ed3-260">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="71ed3-260">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="71ed3-261">Usuwa pole RowVersion.</span><span class="sxs-lookup"><span data-stu-id="71ed3-261">Removes the RowVersion field.</span></span>

* <span data-ttu-id="71ed3-262">Dodaje pole ukryte służące `RowVersion` właściwości.</span><span class="sxs-lookup"><span data-stu-id="71ed3-262">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="71ed3-263">Uruchom aplikację i przejdź do strony indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="71ed3-263">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="71ed3-264">Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**, na pierwszej karcie kliknięcie **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="71ed3-264">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="71ed3-265">W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **Zapisz**:</span><span class="sxs-lookup"><span data-stu-id="71ed3-265">In the first window, change one of the values, and click **Save**:</span></span>

![Strona edytowania działu po zmianie przed delete](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="71ed3-267">W drugiej karcie kliknij **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="71ed3-267">In the second tab, click **Delete**.</span></span> <span data-ttu-id="71ed3-268">Zostanie wyświetlony komunikat o błędzie współbieżności, a wartości Dział zostaną odświeżone przy użyciu co to jest obecnie dostępna w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="71ed3-268">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Strona potwierdzenia usunięcia działu błąd współbieżności](concurrency/_static/delete-error.png)

<span data-ttu-id="71ed3-270">Jeśli klikniesz **Usuń** ponownie, użytkownik jest przekierowany do strony indeksu, który pokazuje, że dział został usunięty.</span><span class="sxs-lookup"><span data-stu-id="71ed3-270">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="71ed3-271">Aktualizowanie szczegółów i tworzenia widoków</span><span class="sxs-lookup"><span data-stu-id="71ed3-271">Update Details and Create views</span></span>

<span data-ttu-id="71ed3-272">Opcjonalnie można wyczyścić utworzony szkielet kodu w szczegółach i tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="71ed3-272">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="71ed3-273">Zastąp kod w *Views/Departments/Details.cshtml* Usuń kolumnę RowVersion i wyświetlić pełną nazwę administratora.</span><span class="sxs-lookup"><span data-stu-id="71ed3-273">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="71ed3-274">Zastąp kod w *Views/Departments/Create.cshtml* do dodania zaznacz opcję do listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="71ed3-274">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="71ed3-275">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="71ed3-275">Summary</span></span>

<span data-ttu-id="71ed3-276">Na tym kończy się wprowadzenie do obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="71ed3-276">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="71ed3-277">Aby uzyskać więcej informacji na temat obsługi współbieżności w programie EF Core, zobacz [konfliktów współbieżności](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="71ed3-277">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="71ed3-278">Następny samouczek pokazuje, jak zaimplementować Tabela wg hierarchii dziedziczenia dla jednostek przez instruktorów i uczniów.</span><span class="sxs-lookup"><span data-stu-id="71ed3-278">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="71ed3-279">[Poprzednie](update-related-data.md)
> [dalej](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="71ed3-279">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>
