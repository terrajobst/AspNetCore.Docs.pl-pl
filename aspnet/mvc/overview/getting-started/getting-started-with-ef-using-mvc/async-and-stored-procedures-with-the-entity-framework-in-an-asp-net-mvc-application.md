---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: Użycie async i procedur składowanych z EF w aplikacji platformy ASP.NET MVC'
description: W tym samouczku zobaczysz sposobu implementacji asynchronicznego modelu programowania i Dowiedz się, jak używanie procedur składowanych.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9041167af076d80ebf294e054ffe51293d11e888
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889876"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="5b562-103">Samouczek: Użycie async i procedur składowanych z EF w aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5b562-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="5b562-104">W samouczkach wcześniej pokazaliśmy ci, jak odczytywanie i aktualizowanie danych za pomocą synchronicznego modelu programowania.</span><span class="sxs-lookup"><span data-stu-id="5b562-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="5b562-105">W tym samouczku dowiesz się jak zaimplementować asynchronicznego modelu programowania.</span><span class="sxs-lookup"><span data-stu-id="5b562-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="5b562-106">Kod asynchroniczny może pomóc aplikacji działać lepiej, ponieważ ułatwia lepsze wykorzystanie zasobów serwera.</span><span class="sxs-lookup"><span data-stu-id="5b562-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="5b562-107">W ramach tego samouczka możesz także Zobacz, jak używane procedury składowane insert, update i operacje usuwania w jednostce.</span><span class="sxs-lookup"><span data-stu-id="5b562-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="5b562-108">Na koniec ponownego wdrażania aplikacji na platformie Azure oraz wszystkich zmian w bazie danych, które zostały zaimplementowane od podczas pierwszego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="5b562-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="5b562-109">Na poniższych ilustracjach przedstawiono niektóre stron którymi będziesz pracować.</span><span class="sxs-lookup"><span data-stu-id="5b562-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Działy strony](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Utwórz działu](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="5b562-112">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="5b562-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b562-113">Dowiedz się więcej o kodzie asynchronicznym</span><span class="sxs-lookup"><span data-stu-id="5b562-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="5b562-114">Tworzenie kontrolera działu</span><span class="sxs-lookup"><span data-stu-id="5b562-114">Create a Department controller</span></span>
> * <span data-ttu-id="5b562-115">Używanie procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="5b562-115">Use stored procedures</span></span>
> * <span data-ttu-id="5b562-116">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="5b562-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b562-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5b562-117">Prerequisites</span></span>

* [<span data-ttu-id="5b562-118">Aktualizowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="5b562-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="5b562-119">Dlaczego warto korzystać z kodu asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="5b562-119">Why use asynchronous code</span></span>

<span data-ttu-id="5b562-120">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="5b562-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="5b562-121">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="5b562-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="5b562-122">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="5b562-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="5b562-123">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="5b562-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="5b562-124">W rezultacie kod asynchroniczny umożliwia zasobów serwera w celu można użyć bardziej wydajnie i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="5b562-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="5b562-125">We wcześniejszych wersjach programu .NET, pisanie i testowanie kodu asynchronicznego została skomplikowana, błąd podatne i trudne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="5b562-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="5b562-126">W .NET 4.5 jest znacznie łatwiejsze, że powinien ogólnie wpisywania kodu asynchronicznego, chyba że istnieje powód, aby nie były pisania, testowania i debugowania kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="5b562-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="5b562-127">Kod asynchroniczny wprowadzają niewielkiej ilości obciążenia, ale wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu sytuacjach o niewielkim ruchu, istotne jest potencjalna poprawa wydajności.</span><span class="sxs-lookup"><span data-stu-id="5b562-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="5b562-128">Aby uzyskać więcej informacji na temat programowania asynchronicznego, zobacz [Obsługa async Użyj .NET 4.5 w celu unikania blokowania połączeń](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="5b562-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="5b562-129">Tworzenie kontrolera działu</span><span class="sxs-lookup"><span data-stu-id="5b562-129">Create Department controller</span></span>

<span data-ttu-id="5b562-130">Tworzenie kontrolera działu, w taki sam sposób jak w przypadku starszych kontrolerów, jednak tym razem wybierz **używać asynchronicznych akcji kontrolera** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="5b562-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="5b562-131">Następującym głównym funkcjom Pokaż dodanych do kodu synchronicznego `Index` metodę, aby stał się asynchronicznego:</span><span class="sxs-lookup"><span data-stu-id="5b562-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="5b562-132">Cztery zmiany zostały zastosowane do Włączanie kwerenda bazy danych programu Entity Framework asynchroniczne wykonywanie:</span><span class="sxs-lookup"><span data-stu-id="5b562-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="5b562-133">Metoda jest oznaczona atrybutem `async` słowo kluczowe, które informuje kompilator, aby wygenerować wywołania zwrotne dla części treści metody, a także automatyczne tworzenie `Task<ActionResult>` obiekt, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="5b562-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="5b562-134">Zwracany typ został zmieniony z `ActionResult` do `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="5b562-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="5b562-135">`Task<T>` Typu reprezentuje pracę w toku z wyniku typu `T`.</span><span class="sxs-lookup"><span data-stu-id="5b562-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="5b562-136">`await` — Słowo kluczowe została zastosowana do wywołania usługi sieci web.</span><span class="sxs-lookup"><span data-stu-id="5b562-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="5b562-137">Gdy kompilator wykryje to słowo kluczowe, w tle dzieli metody na dwie części.</span><span class="sxs-lookup"><span data-stu-id="5b562-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="5b562-138">Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="5b562-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="5b562-139">Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="5b562-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="5b562-140">To wersja asynchroniczna elementu `ToList` wywołano metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5b562-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="5b562-141">Dlaczego jest `departments.ToList` instrukcji zmodyfikowany, ale nie `departments = db.Departments` instrukcji?</span><span class="sxs-lookup"><span data-stu-id="5b562-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="5b562-142">Przyczyną jest to, że tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="5b562-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="5b562-143">`departments = db.Departments` Instrukcja ustawia się zapytanie, ale zapytanie nie jest wykonywany do momentu `ToList` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="5b562-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="5b562-144">W związku z tym tylko `ToList` metoda jest wykonywana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="5b562-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="5b562-145">W `Details` metody i `HttpGet` `Edit` i `Delete` metod `Find` metodą jest ten, który powoduje, że zapytania wysyłane do bazy danych, to metody, która pobiera wykonywany asynchronicznie:</span><span class="sxs-lookup"><span data-stu-id="5b562-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="5b562-146">W `Create`, `HttpPost Edit`, i `DeleteConfirmed` metod jest `SaveChanges` wywołanie metody, która powoduje, że polecenie do wykonania, nie instrukcji takich jak `db.Departments.Add(department)` co spowodować tylko jednostki w pamięci do zmodyfikowania.</span><span class="sxs-lookup"><span data-stu-id="5b562-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="5b562-147">Otwórz *Views\Department\Index.cshtml*i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5b562-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="5b562-148">Ten kod zmienia się tytuł z indeksu działów, przenosi nazwy administratora po prawej stronie i zapewnia pełną nazwę administratora.</span><span class="sxs-lookup"><span data-stu-id="5b562-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="5b562-149">W przypadku tworzenia, usuwania, podawania szczegółów i edytowanie widoków, Zmień podpis `InstructorID` pole "Administrator" taki sam sposób, jak zmienić pole nazwy działu "Dział" w widokach kursu.</span><span class="sxs-lookup"><span data-stu-id="5b562-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="5b562-150">Tworzenie i edytowanie widoków, użyj następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="5b562-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="5b562-151">W widokach Delete i szczegółowe informacje, użyj następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="5b562-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="5b562-152">Uruchom aplikację, a następnie kliknij przycisk **działów** kartę.</span><span class="sxs-lookup"><span data-stu-id="5b562-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="5b562-153">Wszystko, co działa tak samo, jak w innych kontrolerów, ale w tym kontrolerze wszystkich zapytań SQL są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="5b562-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="5b562-154">Kilka rzeczy, aby wiedzieć, w którym używasz programowania asynchronicznego przy użyciu platformy Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="5b562-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="5b562-155">Kod asynchroniczny nie jest bezpieczny dla wątków.</span><span class="sxs-lookup"><span data-stu-id="5b562-155">The async code is not thread safe.</span></span> <span data-ttu-id="5b562-156">Innymi słowy innymi słowy, nie należy próbować wykonać wiele operacji równolegle za pomocą tego samego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="5b562-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="5b562-157">Jeśli chcesz skorzystać z zalet wydajności kod asynchroniczny, upewnij się, wszystkie biblioteki pakietami, które używasz (takie jak w przypadku stronicowania), jeśli wywołują wszystkie metody, że zapytania wysyłane do bazy danych programu Entity Framework użyć async.</span><span class="sxs-lookup"><span data-stu-id="5b562-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="5b562-158">Używanie procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="5b562-158">Use stored procedures</span></span>

<span data-ttu-id="5b562-159">Niektóre deweloperów i przetwarzający wolą używanie procedur składowanych, aby uzyskać dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5b562-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="5b562-160">We wcześniejszych wersjach programu Entity Framework można pobrać danych za pomocą procedury składowanej przez [wykonywanie pierwotne zapytania SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nie może nakazać EF procedur składowanych na potrzeby operacji aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="5b562-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="5b562-161">W programów EF 6 jest łatwa do skonfigurowania Code First na używanie procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="5b562-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="5b562-162">W *DAL\SchoolContext.cs*, Dodaj wyróżniony kod do `OnModelCreating` metody.</span><span class="sxs-lookup"><span data-stu-id="5b562-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="5b562-163">Ten kod powoduje, że platformy Entity Framework do użycia przechowywanych procedur Wstawianie, aktualizowanie i usuwanie operacji na `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="5b562-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="5b562-164">W konsoli Zarządzanie pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5b562-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="5b562-165">Otwórz *migracje\&lt; sygnatura czasowa&gt;\_DepartmentSP.cs* Aby wyświetlić kod w `Up` metodę umożliwiającą utworzenie wstawiania, aktualizowania i usuwania procedur składowanych:</span><span class="sxs-lookup"><span data-stu-id="5b562-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="5b562-166">W konsoli Zarządzanie pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5b562-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="5b562-167">Uruchom aplikację w trybie debugowania, kliknij przycisk **działów** kartę, a następnie kliknij przycisk **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="5b562-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="5b562-168">Wprowadź dane dla nowego wydziału, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5b562-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="5b562-169">W programie Visual Studio, sprawdź dzienniki w **dane wyjściowe** okno, aby wyświetlić, aby wstawić nowy wiersz działu użyto procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="5b562-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Dział Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="5b562-171">Kod najpierw tworzy domyślne przechowywane procedury nazwy.</span><span class="sxs-lookup"><span data-stu-id="5b562-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="5b562-172">Jeśli używasz istniejącej bazy danych może być konieczne Dostosowywanie nazw procedury składowanej, aby można było używać procedury składowane już zdefiniowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5b562-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="5b562-173">Aby uzyskać informacje o tym, jak to zrobić, zobacz [Entity Framework Code pierwszy wstawiania/aktualizowania/usuwania procedur składowanych](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="5b562-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="5b562-174">Jeśli chcesz dostosować, co generowane czy procedury składowane, można edytować utworzony szkielet kodu dla migracje `Up` metodę, która tworzy procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="5b562-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="5b562-175">W ten sposób zmiany są odzwierciedlane zawsze, gdy czy migracja jest uruchamiany i zostaną one zastosowane do produkcyjnej bazy danych, podczas migracji automatycznie uruchamia się w środowisku produkcyjnym po zakończeniu wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="5b562-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="5b562-176">Jeśli chcesz zmienić istniejącą procedurę składowaną, która została utworzona w poprzedniej migracji, można wygenerować pusty migracji za pomocą polecenia Add-migracji i ręcznie napisać kod, który wywołuje [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) — metoda .</span><span class="sxs-lookup"><span data-stu-id="5b562-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="5b562-177">Wdrażanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="5b562-177">Deploy to Azure</span></span>

<span data-ttu-id="5b562-178">W tej sekcji, musisz ukończyć opcjonalną **wdrażania aplikacji na platformie Azure** sekcji [migracje i wdrożenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek z tej serii.</span><span class="sxs-lookup"><span data-stu-id="5b562-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="5b562-179">Jeśli użytkownik ma błędy migracji, które rozwiązano problem, usuwając bazy danych w projekcie lokalnym, Pomiń tę sekcję.</span><span class="sxs-lookup"><span data-stu-id="5b562-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="5b562-180">W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="5b562-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="5b562-181">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="5b562-181">Click **Publish**.</span></span>

    <span data-ttu-id="5b562-182">Program Visual Studio wdroży aplikację na platformie Azure, a aplikacja zostanie otwarta w domyślnej przeglądarce, działające na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="5b562-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="5b562-183">Testowanie aplikacji w celu zweryfikowania działa.</span><span class="sxs-lookup"><span data-stu-id="5b562-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="5b562-184">Uruchom stronę po raz pierwszy uzyskuje dostęp do bazy danych, platformy Entity Framework umożliwia uruchamianie wszystkich migracjach `Up` metod wymaganych do przełączenia bazy danych na bieżąco z bieżącym modelem danych.</span><span class="sxs-lookup"><span data-stu-id="5b562-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="5b562-185">Można teraz używać wszystkich stron sieci web, które dodane od czasu ostatniego wdrożenia, w tym stron działu, które zostały dodane w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5b562-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="5b562-186">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="5b562-186">Get the code</span></span>

[<span data-ttu-id="5b562-187">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="5b562-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="5b562-188">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5b562-188">Additional resources</span></span>

<span data-ttu-id="5b562-189">Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="5b562-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b562-190">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5b562-190">Next steps</span></span>

<span data-ttu-id="5b562-191">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="5b562-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b562-192">Przedstawia informacje na temat kodu asynchronicznego</span><span class="sxs-lookup"><span data-stu-id="5b562-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="5b562-193">Utworzony kontroler działu</span><span class="sxs-lookup"><span data-stu-id="5b562-193">Created a Department controller</span></span>
> * <span data-ttu-id="5b562-194">Używane procedury składowane</span><span class="sxs-lookup"><span data-stu-id="5b562-194">Used stored procedures</span></span>
> * <span data-ttu-id="5b562-195">Wdrażane na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="5b562-195">Deployed to Azure</span></span>

<span data-ttu-id="5b562-196">Przejdź do następnego artykułu, aby dowiedzieć się, jak obsługa konfliktów, gdy wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="5b562-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="5b562-197">Obsługa współbieżności</span><span class="sxs-lookup"><span data-stu-id="5b562-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
