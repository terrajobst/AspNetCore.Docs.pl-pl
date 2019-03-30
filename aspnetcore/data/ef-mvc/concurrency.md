---
title: 'Samouczek: Obsługa współbieżności — ASP.NET MVC z programem EF Core'
description: W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 668cdafc078091b65035ecad854d2ecc62555721
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750861"
---
# <a name="tutorial-handle-concurrency---aspnet-mvc-with-ef-core"></a><span data-ttu-id="97c47-103">Samouczek: Obsługa współbieżności — ASP.NET MVC z programem EF Core</span><span class="sxs-lookup"><span data-stu-id="97c47-103">Tutorial: Handle concurrency - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="97c47-104">W samouczkach wcześniej przedstawiono sposób zaktualizować dane.</span><span class="sxs-lookup"><span data-stu-id="97c47-104">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="97c47-105">W tym samouczku przedstawiono sposób obsługi konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="97c47-105">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="97c47-106">Instrukcje dotyczące tworzenia stron sieci web, które współpracują z jednostką działu i obsługi błędów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-106">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="97c47-107">Na poniższych ilustracjach przedstawiono edytowanie i usuwanie stron, w tym niektóre komunikaty, które są wyświetlane, jeśli wystąpi konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-107">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Strona edytowania działu](concurrency/_static/edit-error.png)

![Strona usuwanie działu](concurrency/_static/delete-error.png)

<span data-ttu-id="97c47-110">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="97c47-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="97c47-111">Dowiedz się więcej o konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="97c47-111">Learn about concurrency conflicts</span></span>
> * <span data-ttu-id="97c47-112">Dodawanie właściwości śledzenia</span><span class="sxs-lookup"><span data-stu-id="97c47-112">Add a tracking property</span></span>
> * <span data-ttu-id="97c47-113">Tworzenie kontrolera działów i widoków</span><span class="sxs-lookup"><span data-stu-id="97c47-113">Create Departments controller and views</span></span>
> * <span data-ttu-id="97c47-114">Aktualizacja widoku indeks</span><span class="sxs-lookup"><span data-stu-id="97c47-114">Update Index view</span></span>
> * <span data-ttu-id="97c47-115">Aktualizacja metod edycji</span><span class="sxs-lookup"><span data-stu-id="97c47-115">Update Edit methods</span></span>
> * <span data-ttu-id="97c47-116">Aktualizowanie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="97c47-116">Update Edit view</span></span>
> * <span data-ttu-id="97c47-117">Testowanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="97c47-117">Test concurrency conflicts</span></span>
> * <span data-ttu-id="97c47-118">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="97c47-118">Update the Delete page</span></span>
> * <span data-ttu-id="97c47-119">Aktualizowanie szczegółów i tworzenia widoków</span><span class="sxs-lookup"><span data-stu-id="97c47-119">Update Details and Create views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97c47-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="97c47-120">Prerequisites</span></span>

* [<span data-ttu-id="97c47-121">Aktualizowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="97c47-121">Update related data</span></span>](update-related-data.md)

## <a name="concurrency-conflicts"></a><span data-ttu-id="97c47-122">Konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="97c47-122">Concurrency conflicts</span></span>

<span data-ttu-id="97c47-123">Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki w celu edycji, a następnie inny użytkownik aktualizuje dane w tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="97c47-123">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="97c47-124">Jeśli nie włączysz wykrywania takie konflikty, ostatnia spojrzenia aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="97c47-124">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="97c47-125">W wielu aplikacjach, to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie jest tak naprawdę krytycznego, jeśli niektóre zmiany są zastępowane, koszt programowania współbieżność może przeważają korzyści.</span><span class="sxs-lookup"><span data-stu-id="97c47-125">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="97c47-126">W takiej sytuacji nie trzeba skonfigurować aplikację do obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-126">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="97c47-127">Współbieżność pesymistyczna (blokowanie)</span><span class="sxs-lookup"><span data-stu-id="97c47-127">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="97c47-128">Jeśli Twoja aplikacja wymaga uniknąć przypadkowej utraty danych w scenariuszach współbieżności, używają blokady bazy danych jest jednym ze sposobów, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="97c47-128">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="97c47-129">Jest to nazywane pesymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-129">This is called pessimistic concurrency.</span></span> <span data-ttu-id="97c47-130">Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub uzyskać dostęp do aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="97c47-130">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="97c47-131">Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub zaktualizować dostępu, ponieważ jaką uzyskują kopii danych, która jest w trakcie zmieniany.</span><span class="sxs-lookup"><span data-stu-id="97c47-131">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="97c47-132">Jeśli zablokujesz wiersz, aby uzyskać dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="97c47-132">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="97c47-133">Zarządzanie blokady ma wady.</span><span class="sxs-lookup"><span data-stu-id="97c47-133">Managing locks has disadvantages.</span></span> <span data-ttu-id="97c47-134">Może być skomplikowane, aby program.</span><span class="sxs-lookup"><span data-stu-id="97c47-134">It can be complex to program.</span></span> <span data-ttu-id="97c47-135">Wymaga znaczących bazy danych zarządzania zasobami, a może to spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa się.</span><span class="sxs-lookup"><span data-stu-id="97c47-135">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="97c47-136">Z tego względu nie wszystkie systemy zarządzania bazami danych obsługuje pesymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-136">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="97c47-137">Entity Framework Core nie obsługuje wbudowanej go, a w tym samouczku nie pokazano, jak ją wdrożyć.</span><span class="sxs-lookup"><span data-stu-id="97c47-137">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="97c47-138">Optymistyczna współbieżność</span><span class="sxs-lookup"><span data-stu-id="97c47-138">Optimistic Concurrency</span></span>

<span data-ttu-id="97c47-139">Alternatywa dla Współbieżność pesymistyczna jest optymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-139">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="97c47-140">Optymistyczna współbieżność oznacza, umożliwiając konfliktów współbieżności do wykonania, a następnie reagowaniu odpowiednio Jeśli tak jest.</span><span class="sxs-lookup"><span data-stu-id="97c47-140">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="97c47-141">Na przykład Magdalena odwiedzin strony Edytuj działu i zmienia kwota budżetu dla angielskiego działu z $350,000.00 na 0,00 USD.</span><span class="sxs-lookup"><span data-stu-id="97c47-141">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Zmiana budżetu na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="97c47-143">Zanim kliknie Magdalena **Zapisz**, Jan odwiedzi tę samą stronę i zmiany pola Data rozpoczęcia z 2007-9-1 do 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="97c47-143">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Zmiana daty rozpoczęcia do 2013](concurrency/_static/change-date.png)

<span data-ttu-id="97c47-145">Magdalena kliknie **Zapisz** pierwszy i widzi jej zmieniać, gdy przeglądarka powróci do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="97c47-145">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Budżet na zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="97c47-147">A następnie kliknie John **Zapisz** na stronie edycji, który nadal pokazuje budżetu 350,000.00 $.</span><span class="sxs-lookup"><span data-stu-id="97c47-147">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="97c47-148">Co dzieje się potem określają sposób obsługi konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-148">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="97c47-149">Niektóre opcje obejmują następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="97c47-149">Some of the options include the following:</span></span>

* <span data-ttu-id="97c47-150">Można zachować informacje o właściwości, które użytkownik zmodyfikował i aktualizować tylko odpowiednie kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="97c47-150">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="97c47-151">W przykładowym scenariuszu żadne dane nie zostałyby utracone, ponieważ inne właściwości zostały zaktualizowane przez dwóch użytkowników.</span><span class="sxs-lookup"><span data-stu-id="97c47-151">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="97c47-152">Przy następnym ktoś przegląda angielskiej działu, zobaczą zmiany nazwy i John's — datę rozpoczęcia 9/1/2013 i budżetu, zerowego dolarów.</span><span class="sxs-lookup"><span data-stu-id="97c47-152">The next time someone browses the English department, they will see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="97c47-153">Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurencyjnych zmiany zostały wprowadzone w tej samej właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="97c47-153">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="97c47-154">Czy platformy Entity Framework działa w ten sposób zależy od tego, jak zaimplementować kod aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="97c47-154">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="97c47-155">Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu w celu śledzenia oryginalnych wartości właściwości dla jednostki, a także nowe wartości.</span><span class="sxs-lookup"><span data-stu-id="97c47-155">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="97c47-156">Obsługa dużych ilości stanu może mieć wpływ na wydajność aplikacji, ponieważ jego wymaga zasobów serwera albo muszą być zawarte na stronie sieci web (na przykład w ukrytych polach) lub w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="97c47-156">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="97c47-157">Można pozwolić, aby zmiana John's zastąpienie Joanny zmian.</span><span class="sxs-lookup"><span data-stu-id="97c47-157">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="97c47-158">Przy następnym ktoś przegląda angielskiej działu, zobaczy 9/1/2013 i przywrócone wartości $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="97c47-158">The next time someone browses the English department, they will see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="97c47-159">Jest to nazywane *Wins klienta* lub *ostatnie w usłudze Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="97c47-159">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="97c47-160">(Wszystkie wartości z klienta pierwszeństwo co znajduje się w magazynie danych.) Jak wspomniano we wprowadzeniu do tej sekcji, w przeciwnym razie pisania obsługi współbieżności, nastąpi to automatycznie.</span><span class="sxs-lookup"><span data-stu-id="97c47-160">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="97c47-161">Aby uniemożliwić zmiany John's aktualizowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="97c47-161">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="97c47-162">Zazwyczaj będzie wyświetlony komunikat o błędzie, pokazują, jak bieżący stan danych i umożliwiające podszycie się ponownie zastosuj swoje zmiany, jeśli chce nadal były.</span><span class="sxs-lookup"><span data-stu-id="97c47-162">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="97c47-163">Jest to nazywane *Store Wins* scenariusza.</span><span class="sxs-lookup"><span data-stu-id="97c47-163">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="97c47-164">(Wartości magazynu danych pierwszeństwo wartości przesłany przez klienta.) Będzie implementować scenariusza Store Wins w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="97c47-164">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="97c47-165">Ta metoda zapewnia, że żadne zmiany nie zostaną zastąpione bez użytkownika, w tym celu z wydarzeniami.</span><span class="sxs-lookup"><span data-stu-id="97c47-165">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="97c47-166">Wykrywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="97c47-166">Detecting concurrency conflicts</span></span>

<span data-ttu-id="97c47-167">Należy rozwiązać konflikty, obsługując `DbConcurrencyException` wyjątki, które zgłasza Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="97c47-167">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="97c47-168">Aby wiedzieć, kiedy trzeba generować te wyjątki, platformy Entity Framework musi umożliwiać wykrywanie konfliktów.</span><span class="sxs-lookup"><span data-stu-id="97c47-168">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="97c47-169">W związku z tym należy skonfigurować bazy danych oraz model danych odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="97c47-169">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="97c47-170">Niektóre opcje umożliwiające wykrywanie konfliktów są następujące:</span><span class="sxs-lookup"><span data-stu-id="97c47-170">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="97c47-171">W tabeli bazy danych zawierają kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="97c47-171">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="97c47-172">Następnie można skonfigurować programu Entity Framework, aby uwzględnić tej kolumny w klauzuli Where klauzuli SQL Update lub Delete poleceń.</span><span class="sxs-lookup"><span data-stu-id="97c47-172">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="97c47-173">Typ danych kolumny śledzenia jest zazwyczaj `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="97c47-173">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="97c47-174">`rowversion` Wartość numeru sekwencyjnego, który jest zwiększana za każdym razem, zaktualizować wiersza.</span><span class="sxs-lookup"><span data-stu-id="97c47-174">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="97c47-175">W poleceniu Update lub Delete gdzie klauzula zawiera oryginalna wartość kolumny śledzenia (oryginalna wersja wiersza).</span><span class="sxs-lookup"><span data-stu-id="97c47-175">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="97c47-176">Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc instrukcji Update lub Delete nie można odnaleźć wiersza do zaktualizowania ze względu na miejsce klauzuli.</span><span class="sxs-lookup"><span data-stu-id="97c47-176">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="97c47-177">Gdy znajdzie Entity Framework, że żadne wiersze nie zostały zaktualizowane, Update lub Delete polecenie (to znaczy, gdy liczba wierszy, których to dotyczy, jest równy zero), interpretuje, jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-177">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="97c47-178">Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości każdej kolumny w tabeli w klauzuli Where klauzuli polecenia Update i Delete.</span><span class="sxs-lookup"><span data-stu-id="97c47-178">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="97c47-179">Tak jak w pierwszej opcji, jeśli nic w wierszu zmieniła się od wiersza została najpierw przeczytać artykuł gdzie klauzula nie zwraca wiersz, aby zaktualizować, której platformy Entity Framework interpretuje jako konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-179">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="97c47-180">Dla tabel bazy danych, które mają wiele kolumn, to podejście może doprowadzić do bardzo dużych gdzie zdań i może wymagać obsługi dużych ilości stanu.</span><span class="sxs-lookup"><span data-stu-id="97c47-180">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="97c47-181">Jak wspomniano wcześniej, obsługi dużych ilości stanu mogą wpływać na wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97c47-181">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="97c47-182">W związku z tym to podejście zazwyczaj nie zaleca się i nie można go metodę używaną w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="97c47-182">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="97c47-183">Jeśli chcesz zaimplementować to podejście do współbieżności, musisz oznaczyć wszystkie właściwości bez klucza podstawowego w jednostce, które chcesz śledzić concurrency, dodając `ConcurrencyCheck` atrybutu do nich.</span><span class="sxs-lookup"><span data-stu-id="97c47-183">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="97c47-184">Ta zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w klauzuli SQL Where w instrukcji Update i Delete.</span><span class="sxs-lookup"><span data-stu-id="97c47-184">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="97c47-185">W pozostałej części tego samouczka dodasz `rowversion` śledzenie właściwości do jednostki działu, Utwórz kontrolera i widoki, a test, aby sprawdzić, czy wszystko działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="97c47-185">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property"></a><span data-ttu-id="97c47-186">Dodawanie właściwości śledzenia</span><span class="sxs-lookup"><span data-stu-id="97c47-186">Add a tracking property</span></span>

<span data-ttu-id="97c47-187">W *Models/Department.cs*, dodawanie właściwości śledzenia o nazwie RowVersion:</span><span class="sxs-lookup"><span data-stu-id="97c47-187">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="97c47-188">`Timestamp` Atrybut określa, że ta kolumna będzie zawierać w Where klauzuli Update i Delete polecenia wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="97c47-188">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="97c47-189">Ten atrybut jest nazywany `Timestamp` ponieważ poprzednie wersje programu SQL Server używane SQL `timestamp` typu danych, zanim SQL `rowversion` zastąpiono ją.</span><span class="sxs-lookup"><span data-stu-id="97c47-189">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="97c47-190">Typ architektury .NET dla `rowversion` jest tablicą bajtów.</span><span class="sxs-lookup"><span data-stu-id="97c47-190">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="97c47-191">Jeśli wolisz używać interfejsu API fluent, możesz użyć `IsConcurrencyToken` — metoda (w *Data/SchoolContext.cs*) można określić właściwości śledzenia, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="97c47-191">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="97c47-192">Przez dodanie właściwości po zmianie modelu bazy danych, więc należy przeprowadzić migrację z innej.</span><span class="sxs-lookup"><span data-stu-id="97c47-192">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="97c47-193">Zapisz zmiany i skompiluj projekt, a następnie wprowadź następujące polecenia w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="97c47-193">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-departments-controller-and-views"></a><span data-ttu-id="97c47-194">Tworzenie kontrolera działów i widoków</span><span class="sxs-lookup"><span data-stu-id="97c47-194">Create Departments controller and views</span></span>

<span data-ttu-id="97c47-195">Jak wcześniej dla uczniów, kursy i instruktorów, tworzenia szkieletu kontrolera działów i widoków.</span><span class="sxs-lookup"><span data-stu-id="97c47-195">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Tworzenie szkieletu działu](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="97c47-197">W *DepartmentsController.cs* plików, zmienić wszystkie cztery wystąpienia "FirstMidName" na "Imię i nazwisko", tak, aby list rozwijanych administratora działu będzie zawierać imię i nazwisko instruktora, a nie po prostu nazwisko.</span><span class="sxs-lookup"><span data-stu-id="97c47-197">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-index-view"></a><span data-ttu-id="97c47-198">Aktualizacja widoku indeks</span><span class="sxs-lookup"><span data-stu-id="97c47-198">Update Index view</span></span>

<span data-ttu-id="97c47-199">Aparat tworzenia szkieletów utworzył RowVersion kolumny w widoku indeksu, ale nie powinny być wyświetlane to pole.</span><span class="sxs-lookup"><span data-stu-id="97c47-199">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="97c47-200">Zastąp kod w *Views/Departments/Index.cshtml* następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="97c47-200">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="97c47-201">Zmiany pozycji "Wydziałom", usuwa kolumnę RowVersion i pokazuje pełną nazwę zamiast imię administratora.</span><span class="sxs-lookup"><span data-stu-id="97c47-201">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-edit-methods"></a><span data-ttu-id="97c47-202">Aktualizacja metod edycji</span><span class="sxs-lookup"><span data-stu-id="97c47-202">Update Edit methods</span></span>

<span data-ttu-id="97c47-203">W obu narzędzia HttpGet `Edit` metody i `Details` metody, Dodaj `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="97c47-203">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="97c47-204">W HttpGet `Edit` metody, Dodaj wczesne ładowanie dla administratora.</span><span class="sxs-lookup"><span data-stu-id="97c47-204">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="97c47-205">Zastąp istniejący kod httppost `Edit` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="97c47-205">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="97c47-206">Na początku kodu próby odczytu z działu do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="97c47-206">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="97c47-207">Jeśli `SingleOrDefaultAsync` metoda zwraca wartość null, dział został usunięty przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="97c47-207">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="97c47-208">W takiej sytuacji kod używa wartości przesłanego formularza, aby utworzyć jednostki dział strony edytowania mogą być wyświetlane ponownie komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="97c47-208">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="97c47-209">Jako alternatywę nie trzeba ponownie utworzyć jednostki działu, jeśli wyświetla komunikat o błędzie bez wyświetlania pól działu.</span><span class="sxs-lookup"><span data-stu-id="97c47-209">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="97c47-210">Widok przechowuje oryginalny `RowVersion` wartości w ukrytym polu, a ta metoda odbiera tę wartość w `rowVersion` parametru.</span><span class="sxs-lookup"><span data-stu-id="97c47-210">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="97c47-211">Przed wywołaniem `SaveChanges`, musisz umieścić te oryginalnego `RowVersion` wartości właściwości w `OriginalValues` kolekcji jednostki.</span><span class="sxs-lookup"><span data-stu-id="97c47-211">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="97c47-212">Następnie podczas Entity Framework tworzy polecenie pliki aktualizacji programu SQL, to polecenie będzie zawierać klauzuli WHERE, który wyszukuje dla wiersza zawierającego oryginalne `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="97c47-212">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="97c47-213">Jeśli żadne wiersze nie dotyczy polecenia UPDATE (żadnych wierszy ma oryginalny `RowVersion` wartość), platformy Entity Framework zgłasza `DbUpdateConcurrencyException` wyjątek.</span><span class="sxs-lookup"><span data-stu-id="97c47-213">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="97c47-214">Kod w bloku catch dla tego wyjątku pobiera dotyczy jednostki działu, która ma zaktualizowanymi wartościami z `Entries` właściwość obiektu wyjątku.</span><span class="sxs-lookup"><span data-stu-id="97c47-214">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="97c47-215">`Entries` Kolekcja będzie mieć tylko jeden `EntityEntry` obiektu.</span><span class="sxs-lookup"><span data-stu-id="97c47-215">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="97c47-216">Aby uzyskać nowe wartości wprowadzonej przez użytkownika i wartości bieżącej bazy danych, można użyć tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="97c47-216">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="97c47-217">Ten kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny, która ma różne wartości bazy danych, z jakiego które użytkownik wprowadził w edycji strony (tylko jedno pole znajduje się w tym miejscu dla zwięzłości).</span><span class="sxs-lookup"><span data-stu-id="97c47-217">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="97c47-218">Na koniec kod ustawia `RowVersion` wartość `departmentToUpdate` na nową wartość pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="97c47-218">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="97c47-219">Ta nowa `RowVersion` wartość zostanie zapisana w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następnie czasu użytkownik klika polecenie **Zapisz**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie strony edycji zostanie przechwycony.</span><span class="sxs-lookup"><span data-stu-id="97c47-219">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="97c47-220">`ModelState.Remove` Instrukcji jest wymagana, ponieważ `ModelState` ma stary `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="97c47-220">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="97c47-221">W widoku `ModelState` wartość dla pola ma pierwszeństwo przed wartości właściwości modelu, jeśli obie są podane.</span><span class="sxs-lookup"><span data-stu-id="97c47-221">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-edit-view"></a><span data-ttu-id="97c47-222">Aktualizowanie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="97c47-222">Update Edit view</span></span>

<span data-ttu-id="97c47-223">W *Views/Departments/Edit.cshtml*, wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="97c47-223">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="97c47-224">Dodaj pole ukryte, aby zapisać `RowVersion` wartości właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości.</span><span class="sxs-lookup"><span data-stu-id="97c47-224">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="97c47-225">Dodaj opcję "Zaznacz administratora" do listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="97c47-225">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts"></a><span data-ttu-id="97c47-226">Testowanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="97c47-226">Test concurrency conflicts</span></span>

<span data-ttu-id="97c47-227">Uruchom aplikację i przejdź do strony indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="97c47-227">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="97c47-228">Kliknij prawym przyciskiem myszy **Edytuj** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**, następnie kliknij przycisk **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="97c47-228">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="97c47-229">Karty przeglądarki dwa są teraz wyświetlane w tych samych informacji.</span><span class="sxs-lookup"><span data-stu-id="97c47-229">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="97c47-230">Zmień pole na pierwszej karcie przeglądarki, a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="97c47-230">Change a field in the first browser tab and click **Save**.</span></span>

![Edytuj działu po zmianie — strona 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="97c47-232">Przeglądarka wyświetla stronę indeksu wartością zmienione.</span><span class="sxs-lookup"><span data-stu-id="97c47-232">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="97c47-233">Zmień pole na drugiej karcie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="97c47-233">Change a field in the second browser tab.</span></span>

![Edytuj działu po zmianie — strona 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="97c47-235">Kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="97c47-235">Click **Save**.</span></span> <span data-ttu-id="97c47-236">Zobaczysz komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="97c47-236">You see an error message:</span></span>

![Komunikat o błędzie działu edycji strony](concurrency/_static/edit-error.png)

<span data-ttu-id="97c47-238">Kliknij przycisk **Zapisz** ponownie.</span><span class="sxs-lookup"><span data-stu-id="97c47-238">Click **Save** again.</span></span> <span data-ttu-id="97c47-239">Wartość, która została wprowadzona w drugiej karcie przeglądarki jest zapisywany.</span><span class="sxs-lookup"><span data-stu-id="97c47-239">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="97c47-240">Zobaczysz zapisane wartości, gdy zostanie wyświetlona strona indeksu.</span><span class="sxs-lookup"><span data-stu-id="97c47-240">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="97c47-241">Aktualizuj stronę Delete</span><span class="sxs-lookup"><span data-stu-id="97c47-241">Update the Delete page</span></span>

<span data-ttu-id="97c47-242">Na stronie usuwania programu Entity Framework wykrywa konfliktów współbieżności spowodowanych przez osoby z działu inne do edycji w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="97c47-242">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="97c47-243">Gdy HttpGet `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w ukrytym polu.</span><span class="sxs-lookup"><span data-stu-id="97c47-243">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="97c47-244">Czy wartość jest następnie udostępniana HttpPost `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdzi usunięcie.</span><span class="sxs-lookup"><span data-stu-id="97c47-244">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="97c47-245">Entity Framework tworzy polecenie SQL DELETE, zawiera klauzulę WHERE z oryginalnym `RowVersion` wartość.</span><span class="sxs-lookup"><span data-stu-id="97c47-245">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="97c47-246">Jeśli wyniki polecenia zero wierszy dotyczy (wiersz został zmieniony po stronie potwierdzenia usunięcia został wyświetlony przesyłane), zwracany jest wyjątek współbieżności i narzędzia HttpGet `Delete` metoda jest wywoływana z ustawioną flagą błąd wartość true, aby ponownie wyświetlić Strona potwierdzenia komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="97c47-246">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="97c47-247">Istnieje również możliwość, że zero wierszy została zmieniona, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku jest wyświetlany żaden komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="97c47-247">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="97c47-248">Aktualizacja metod usuwania w kontrolerze działów</span><span class="sxs-lookup"><span data-stu-id="97c47-248">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="97c47-249">W *DepartmentsController.cs*, Zastąp HttpGet `Delete` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="97c47-249">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="97c47-250">Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-250">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="97c47-251">Jeśli ta flaga ma wartość true, a dział określono już istnieje, zostało usunięte przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="97c47-251">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="97c47-252">W takiej sytuacji kod wykonuje przekierowanie do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="97c47-252">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="97c47-253">Jeśli ta flaga ma wartość true, a dział istnieje, został zmieniony przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="97c47-253">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="97c47-254">W takiej sytuacji kod wysyła komunikat o błędzie na widok za pomocą `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="97c47-254">In that case, the code sends an error message to the view using `ViewData`.</span></span>

<span data-ttu-id="97c47-255">Zastąp kod w HttpPost `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="97c47-255">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="97c47-256">W utworzony szkielet kodu, który został zastąpiony ta metoda akceptowane tylko identyfikator rekordu:</span><span class="sxs-lookup"><span data-stu-id="97c47-256">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>

```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="97c47-257">Tego parametru zostało zmienione na wystąpienie jednostki działu utworzone przez integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="97c47-257">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="97c47-258">Daje to EF dostęp do wartości właściwości RowVersion oprócz klucza rekordu.</span><span class="sxs-lookup"><span data-stu-id="97c47-258">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="97c47-259">Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`.</span><span class="sxs-lookup"><span data-stu-id="97c47-259">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="97c47-260">Utworzony szkielet kodu użyto nazwy `DeleteConfirmed` zapewnienie metody HttpPost unikatowy podpis.</span><span class="sxs-lookup"><span data-stu-id="97c47-260">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="97c47-261">(Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyć tej samej nazwy dla metody usuwania HttpPost i narzędzia HttpGet.</span><span class="sxs-lookup"><span data-stu-id="97c47-261">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="97c47-262">Jeśli dział został już usunięty, `AnyAsync` metoda zwraca wartość false, oraz aplikacji po prostu powraca do metody indeksu.</span><span class="sxs-lookup"><span data-stu-id="97c47-262">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="97c47-263">Jeżeli zostanie przechwycony błąd współbieżności, kod zostanie ponownie strona potwierdzenia usunięcia i zapewnia, że flagę, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.</span><span class="sxs-lookup"><span data-stu-id="97c47-263">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="97c47-264">Aktualizacja widoku Delete</span><span class="sxs-lookup"><span data-stu-id="97c47-264">Update the Delete view</span></span>

<span data-ttu-id="97c47-265">W *Views/Departments/Delete.cshtml*, Zastąp następujący kod, który dodaje błąd pola komunikatu i ukrytych pól właściwości DepartmentID i RowVersion utworzony szkielet kodu.</span><span class="sxs-lookup"><span data-stu-id="97c47-265">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="97c47-266">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="97c47-266">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="97c47-267">To sprawia, że następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="97c47-267">This makes the following changes:</span></span>

* <span data-ttu-id="97c47-268">Dodaje komunikat o błędzie między `h2` i `h3` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="97c47-268">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="97c47-269">Zamienia FirstMidName imię i nazwisko w **administratora** pola.</span><span class="sxs-lookup"><span data-stu-id="97c47-269">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="97c47-270">Usuwa pole RowVersion.</span><span class="sxs-lookup"><span data-stu-id="97c47-270">Removes the RowVersion field.</span></span>

* <span data-ttu-id="97c47-271">Dodaje pole ukryte służące `RowVersion` właściwości.</span><span class="sxs-lookup"><span data-stu-id="97c47-271">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="97c47-272">Uruchom aplikację i przejdź do strony indeksu działów.</span><span class="sxs-lookup"><span data-stu-id="97c47-272">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="97c47-273">Kliknij prawym przyciskiem myszy **Usuń** hiperlink do działu w języku angielskim, a następnie wybierz pozycję **Otwórz na nowej karcie**, na pierwszej karcie kliknięcie **Edytuj** hiperłącze dla angielskiego działu.</span><span class="sxs-lookup"><span data-stu-id="97c47-273">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="97c47-274">W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **Zapisz**:</span><span class="sxs-lookup"><span data-stu-id="97c47-274">In the first window, change one of the values, and click **Save**:</span></span>

![Strona edytowania działu po zmianie przed delete](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="97c47-276">W drugiej karcie kliknij **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="97c47-276">In the second tab, click **Delete**.</span></span> <span data-ttu-id="97c47-277">Zostanie wyświetlony komunikat o błędzie współbieżności, a wartości Dział zostaną odświeżone przy użyciu co to jest obecnie dostępna w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="97c47-277">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Strona potwierdzenia usunięcia działu błąd współbieżności](concurrency/_static/delete-error.png)

<span data-ttu-id="97c47-279">Jeśli klikniesz **Usuń** ponownie, użytkownik jest przekierowany do strony indeksu, który pokazuje, że dział został usunięty.</span><span class="sxs-lookup"><span data-stu-id="97c47-279">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="97c47-280">Aktualizowanie szczegółów i tworzenia widoków</span><span class="sxs-lookup"><span data-stu-id="97c47-280">Update Details and Create views</span></span>

<span data-ttu-id="97c47-281">Opcjonalnie można wyczyścić utworzony szkielet kodu w szczegółach i tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="97c47-281">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="97c47-282">Zastąp kod w *Views/Departments/Details.cshtml* Usuń kolumnę RowVersion i wyświetlić pełną nazwę administratora.</span><span class="sxs-lookup"><span data-stu-id="97c47-282">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="97c47-283">Zastąp kod w *Views/Departments/Create.cshtml* do dodania zaznacz opcję do listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="97c47-283">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="get-the-code"></a><span data-ttu-id="97c47-284">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="97c47-284">Get the code</span></span>

[<span data-ttu-id="97c47-285">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97c47-285">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a><span data-ttu-id="97c47-286">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="97c47-286">Additional resources</span></span>

 <span data-ttu-id="97c47-287">Aby uzyskać więcej informacji na temat obsługi współbieżności w programie EF Core, zobacz [konfliktów współbieżności](/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="97c47-287">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](/ef/core/saving/concurrency).</span></span>

## <a name="next-steps"></a><span data-ttu-id="97c47-288">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="97c47-288">Next steps</span></span>

<span data-ttu-id="97c47-289">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="97c47-289">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="97c47-290">Przedstawia informacje na temat konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="97c47-290">Learned about concurrency conflicts</span></span>
> * <span data-ttu-id="97c47-291">Dodano właściwości śledzenia</span><span class="sxs-lookup"><span data-stu-id="97c47-291">Added a tracking property</span></span>
> * <span data-ttu-id="97c47-292">Utworzony kontroler działów i widoków</span><span class="sxs-lookup"><span data-stu-id="97c47-292">Created Departments controller and views</span></span>
> * <span data-ttu-id="97c47-293">Zaktualizowano widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="97c47-293">Updated Index view</span></span>
> * <span data-ttu-id="97c47-294">Zaktualizowano metod edycji</span><span class="sxs-lookup"><span data-stu-id="97c47-294">Updated Edit methods</span></span>
> * <span data-ttu-id="97c47-295">Zaktualizowano widoku do edycji</span><span class="sxs-lookup"><span data-stu-id="97c47-295">Updated Edit view</span></span>
> * <span data-ttu-id="97c47-296">Konflikty współbieżności przetestowane</span><span class="sxs-lookup"><span data-stu-id="97c47-296">Tested concurrency conflicts</span></span>
> * <span data-ttu-id="97c47-297">Zaktualizowane strony usuwania</span><span class="sxs-lookup"><span data-stu-id="97c47-297">Updated the Delete page</span></span>
> * <span data-ttu-id="97c47-298">Zaktualizowano szczegółowe informacje i tworzyć widoki</span><span class="sxs-lookup"><span data-stu-id="97c47-298">Updated Details and Create views</span></span>

<span data-ttu-id="97c47-299">Przejdź do następnego samouczka, aby dowiedzieć się, jak zaimplementować Tabela wg hierarchii dziedziczenia dla jednostek przez instruktorów i uczniów.</span><span class="sxs-lookup"><span data-stu-id="97c47-299">Advance to the next tutorial to learn how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="97c47-300">Dalej: Implementowanie Tabela wg hierarchii dziedziczenia</span><span class="sxs-lookup"><span data-stu-id="97c47-300">Next: Implement table-per-hierarchy inheritance</span></span>](inheritance.md)
