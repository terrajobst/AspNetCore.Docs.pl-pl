---
uid: signalr/overview/advanced/dependency-injection
title: Wstrzykiwanie zależności w SignalR | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 2f9b8eeb87882a686df5f35b2e7048a8518c8d4b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753740"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="5e073-103">Wstrzykiwanie zależności w SignalR</span><span class="sxs-lookup"><span data-stu-id="5e073-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="5e073-104">przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5e073-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5e073-105">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="5e073-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5e073-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5e073-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5e073-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5e073-107">.NET 4.5</span></span>
> - <span data-ttu-id="5e073-108">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="5e073-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5e073-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="5e073-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5e073-110">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5e073-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5e073-111">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="5e073-111">Questions and comments</span></span>
> 
> <span data-ttu-id="5e073-112">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="5e073-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5e073-113">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5e073-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5e073-114">Wstrzykiwanie zależności jest sposobem usunięcia ustalonych zależności między obiektami, ułatwiając Zastąp zależności obiektu do testowania (przy użyciu obiektów makiety) lub zmienić zachowanie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="5e073-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="5e073-115">W tym samouczku pokazano, jak wykonać wstrzykiwanie zależności na koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e073-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="5e073-116">Pokazano również, jak używać kontenerów IoC za pomocą biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e073-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="5e073-117">Kontenera IoC to ogólne umożliwiająca iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="5e073-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="5e073-118">Co to jest wstrzykiwanie zależności?</span><span class="sxs-lookup"><span data-stu-id="5e073-118">What is Dependency Injection?</span></span>

<span data-ttu-id="5e073-119">Pomiń tę sekcję, jeśli już znasz iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="5e073-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="5e073-120">*Wstrzykiwanie zależności* (DI) jest wzorcem, w którym obiekty nie są odpowiedzialne za tworzenie własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="5e073-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="5e073-121">Poniżej przedstawiono prosty przykład bodziec DI.</span><span class="sxs-lookup"><span data-stu-id="5e073-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="5e073-122">Załóżmy, że obiekt, który trzeba komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="5e073-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="5e073-123">Można zdefiniować interfejs rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="5e073-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="5e073-124">Obiekt, umożliwia tworzenie `ILogger` na komunikaty dziennika:</span><span class="sxs-lookup"><span data-stu-id="5e073-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="5e073-125">Ta funkcja działa, ale nie jest to najlepszy projekt.</span><span class="sxs-lookup"><span data-stu-id="5e073-125">This works, but it's not the best design.</span></span> <span data-ttu-id="5e073-126">Jeśli chcesz zastąpić `FileLogger` z inną `ILogger` implementacji, trzeba będzie zmodyfikować `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="5e073-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="5e073-127">Przypuszczenia, że wiele innych obiektów użyj `FileLogger`, musisz zmienić ich wszystkich.</span><span class="sxs-lookup"><span data-stu-id="5e073-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="5e073-128">Lub jeśli użytkownik zdecyduje się wprowadzić `FileLogger` pojedynczych, należy także wprowadzić zmiany w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e073-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="5e073-129">Lepszym rozwiązaniem jest "wstrzyknąć" `ILogger` do obiektu — na przykład przy użyciu argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="5e073-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="5e073-130">Teraz obiekt nie jest odpowiedzialny za wybierając, które `ILogger` do użycia.</span><span class="sxs-lookup"><span data-stu-id="5e073-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="5e073-131">Możesz swich `ILogger` implementacje bez wprowadzania zmian obiektów, które od niego zależne.</span><span class="sxs-lookup"><span data-stu-id="5e073-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="5e073-132">Ten wzorzec jest nazywany [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="5e073-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="5e073-133">Inny wzorzec jest iniekcji setter, gdzie ustawiasz zależności za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e073-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="5e073-134">Wstrzykiwanie zależności proste w SignalR</span><span class="sxs-lookup"><span data-stu-id="5e073-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="5e073-135">Należy wziąć pod uwagę aplikacji rozmów z tego samouczka [wprowadzenie do SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5e073-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="5e073-136">Poniżej przedstawiono klasy koncentratora z tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="5e073-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="5e073-137">Załóżmy, że chcesz przechowywanie wiadomości rozmów na serwerze przed ich wysłaniem.</span><span class="sxs-lookup"><span data-stu-id="5e073-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="5e073-138">Może być zdefiniuj interfejs zapewniający abstrakcję tej funkcji i umożliwia DI wstrzyknięcie interfejsu do `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="5e073-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="5e073-139">Problem polega na to, że aplikacji SignalR nie tworzy bezpośrednio koncentratory; SignalR utworzy je.</span><span class="sxs-lookup"><span data-stu-id="5e073-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="5e073-140">Domyślnie SignalR oczekuje, że ma domyślnego konstruktora klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5e073-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="5e073-141">Jednak można łatwo zarejestrowaniu funkcję, aby tworzyć wystąpienia koncentratora i użyć tej funkcji do wykonywania DI.</span><span class="sxs-lookup"><span data-stu-id="5e073-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="5e073-142">Rejestrowanie funkcji przez wywołanie metody **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="5e073-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="5e073-143">Teraz SignalR wywoła tę funkcję anonimowe zawsze wtedy, gdy wymagane do utworzenia `ChatHub` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="5e073-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="5e073-144">Kontenery IoC</span><span class="sxs-lookup"><span data-stu-id="5e073-144">IoC Containers</span></span>

<span data-ttu-id="5e073-145">Powyższy kod jest w dobrym stanie, w przypadku prostych.</span><span class="sxs-lookup"><span data-stu-id="5e073-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="5e073-146">Jednak nadal musiały zapisu to:</span><span class="sxs-lookup"><span data-stu-id="5e073-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="5e073-147">W aplikacji złożonych z wielu zależnościami może być konieczne zapisu mnóstwo ten kod "zespół".</span><span class="sxs-lookup"><span data-stu-id="5e073-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="5e073-148">Ten kod może być trudne do utrzymania, zwłaszcza w przypadku, gdy są zagnieżdżone zależności.</span><span class="sxs-lookup"><span data-stu-id="5e073-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="5e073-149">Jest również trudne do testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="5e073-149">It is also hard to unit test.</span></span>

<span data-ttu-id="5e073-150">Jednym rozwiązaniem jest do używania kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="5e073-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="5e073-151">Kontenera IoC jest składnikiem oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami. Rejestrowanie typów z kontenerem, a następnie użyć do tworzenia obiektów kontenera.</span><span class="sxs-lookup"><span data-stu-id="5e073-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="5e073-152">Kontener automatycznie wpadł na pomysł relacji zależności.</span><span class="sxs-lookup"><span data-stu-id="5e073-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="5e073-153">Wiele kontenerów IoC umożliwiają także kontrolowanie elementów, takich jak okres istnienia obiektów i zakresu.</span><span class="sxs-lookup"><span data-stu-id="5e073-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="5e073-154">"IoC" oznacza "Inwersja kontroli", który jest ogólny wzorzec gdzie struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e073-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="5e073-155">Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".</span><span class="sxs-lookup"><span data-stu-id="5e073-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="5e073-156">Używanie kontenerów IoC w SignalR</span><span class="sxs-lookup"><span data-stu-id="5e073-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="5e073-157">Aplikacja rozmowy prawdopodobnie jest zbyt proste do korzystania z kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="5e073-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="5e073-158">Zamiast tego, Przyjrzyjmy się [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) próbki.</span><span class="sxs-lookup"><span data-stu-id="5e073-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="5e073-159">Przykładowe StockTicker definiuje dwie klasy głównej:</span><span class="sxs-lookup"><span data-stu-id="5e073-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="5e073-160">`StockTickerHub`: Klasy koncentratora, który zarządza połączeniami klienta.</span><span class="sxs-lookup"><span data-stu-id="5e073-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="5e073-161">`StockTicker`: Wzorzec singleton przechowuje cen akcji, która okresowo aktualizuje je.</span><span class="sxs-lookup"><span data-stu-id="5e073-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="5e073-162">`StockTickerHub` zawiera odwołanie do `StockTicker` singleton, podczas gdy `StockTicker` zawiera odwołanie do **IHubConnectionContext** dla `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="5e073-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="5e073-163">Ten interfejs używa do komunikowania się z `StockTickerHub` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="5e073-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="5e073-164">(Aby uzyskać więcej informacji, zobacz [emisje serwera z użyciem ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="5e073-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="5e073-165">Możemy użyć kontenera IoC związanych z porządkowaniem nieco te zależności.</span><span class="sxs-lookup"><span data-stu-id="5e073-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="5e073-166">Najpierw Przyjrzyjmy uprościć `StockTickerHub` i `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="5e073-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="5e073-167">W poniższym kodzie I zostały oznaczone jako komentarz części, nie są nam potrzebne.</span><span class="sxs-lookup"><span data-stu-id="5e073-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="5e073-168">Usuń konstruktora bez parametrów z `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="5e073-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="5e073-169">Zamiast tego należy zawsze używamy DI do Utwórz koncentrator.</span><span class="sxs-lookup"><span data-stu-id="5e073-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="5e073-170">W przypadku StockTicker należy usunąć pojedyncze wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="5e073-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="5e073-171">Później użyjemy kontenera IoC kontrolować okres istnienia StockTicker.</span><span class="sxs-lookup"><span data-stu-id="5e073-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="5e073-172">Ponadto upublicznić konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5e073-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="5e073-173">Następnie możemy refaktoryzować kod, tworząc interfejsu `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5e073-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="5e073-174">Użyjemy tego interfejsu do oddzielenia `StockTickerHub` z `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="5e073-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="5e073-175">Program Visual Studio sprawia, że ten rodzaj refaktoryzacji proste.</span><span class="sxs-lookup"><span data-stu-id="5e073-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="5e073-176">Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy `StockTicker` deklaracji klasy, a następnie wybierz **Refaktoryzuj** ... **Wyodrębnij interfejs**.</span><span class="sxs-lookup"><span data-stu-id="5e073-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="5e073-177">W **Wyodrębnij interfejs** okno dialogowe, kliknij przycisk **Zaznacz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="5e073-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="5e073-178">Pozostaw inne wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="5e073-178">Leave the other defaults.</span></span> <span data-ttu-id="5e073-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e073-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="5e073-180">Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`i zmienia także `StockTicker` wyprowadzenia z `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5e073-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="5e073-181">Otwórz plik IStockTicker.cs i zmień interfejs służący do **publicznych**.</span><span class="sxs-lookup"><span data-stu-id="5e073-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="5e073-182">W `StockTickerHub` klasy, należy zastąpić dwa wystąpienia `StockTicker` do `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="5e073-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="5e073-183">Tworzenie `IStockTicker` interfejsu nie jest to niezbędne, ale chciałem pokazać, jak DI może pomóc zmniejszyć sprzężenia między składnikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e073-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="5e073-184">Dodaj bibliotekę Ninject</span><span class="sxs-lookup"><span data-stu-id="5e073-184">Add the Ninject Library</span></span>

<span data-ttu-id="5e073-185">Istnieje wiele kontenerów IoC typu open source dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="5e073-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="5e073-186">Na potrzeby tego samouczka przy użyciu [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="5e073-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="5e073-187">(Obejmują innych popularnych bibliotek [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), i [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="5e073-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="5e073-188">Menedżer pakietów NuGet Użyj, aby zainstalować [biblioteki Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="5e073-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="5e073-189">W programie Visual Studio z **narzędzia** menu wybierz opcję **Menedżer pakietów biblioteki** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5e073-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="5e073-190">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5e073-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="5e073-191">Zastąp mechanizmu rozpoznawania zależności biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="5e073-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="5e073-192">Aby użyć Ninject w SignalR, należy utworzyć klasę, która pochodzi od klasy **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="5e073-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="5e073-193">Ta klasa zastępuje **GetService** i **funkcji GetServices** metody **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="5e073-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="5e073-194">SignalR wywołuje te metody do tworzenia różnych obiektów w czasie wykonywania, w tym Centrum wystąpień, a także różne usługi używane wewnętrznie przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e073-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="5e073-195">**GetService** metody tworzy pojedyncze wystąpienie typu.</span><span class="sxs-lookup"><span data-stu-id="5e073-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="5e073-196">Przesłonić tę metodę do wywołania jądra Ninject **TryGet** metody.</span><span class="sxs-lookup"><span data-stu-id="5e073-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="5e073-197">Jeśli ta metoda zwraca wartość null, wrócić do domyślnego programu rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="5e073-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="5e073-198">**Funkcji GetServices** metoda tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="5e073-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="5e073-199">Przesłoń tę metodę, aby łączyć wyniki Ninject z wynikami domyślny mechanizm rozwiązywania konfliktów.</span><span class="sxs-lookup"><span data-stu-id="5e073-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="5e073-200">Skonfiguruj Ninject powiązania</span><span class="sxs-lookup"><span data-stu-id="5e073-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="5e073-201">Teraz użyjemy Ninject do deklarowania typu powiązania.</span><span class="sxs-lookup"><span data-stu-id="5e073-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="5e073-202">Otwórz klasę Startup.cs aplikacji (albo utworzony ręcznie zgodnie z instrukcjami pakietu `readme.txt`, lub który został utworzony przez dodawanie uwierzytelniania do projektu).</span><span class="sxs-lookup"><span data-stu-id="5e073-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="5e073-203">W `Startup.Configuration` metody, Tworzenie kontenera Ninject, która wywołuje Ninject *jądra*.</span><span class="sxs-lookup"><span data-stu-id="5e073-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="5e073-204">Utwórz wystąpienie obiektu nasz mechanizm rozpoznawania zależności niestandardowej:</span><span class="sxs-lookup"><span data-stu-id="5e073-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="5e073-205">Utwórz powiązanie dla `IStockTicker` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5e073-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="5e073-206">Ten kod jest informacją dwie rzeczy.</span><span class="sxs-lookup"><span data-stu-id="5e073-206">This code is saying two things.</span></span> <span data-ttu-id="5e073-207">Pierwszy, zawsze wtedy, gdy aplikacja potrzebuje `IStockTicker`, jądra należy utworzyć wystąpienie `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5e073-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="5e073-208">Drugi `StockTicker` klasa mają zostać utworzone jako pojedynczego obiektu.</span><span class="sxs-lookup"><span data-stu-id="5e073-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="5e073-209">Ninject utworzyć jedno wystąpienie obiektu i zwracane to samo wystąpienie dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="5e073-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="5e073-210">Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5e073-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="5e073-211">Ten kod creatres funkcja anonimowa, która zwraca **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="5e073-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="5e073-212">**WhenInjectedInto** metoda informuje Ninject, aby użyć tej funkcji, tylko wtedy, gdy tworzenie `IStockTicker` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="5e073-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="5e073-213">Przyczyną jest to, że SignalR tworzy **IHubConnectionContext** wystąpień wewnętrznie i nie chcemy do zastąpienia, jak SignalR ich tworzenia.</span><span class="sxs-lookup"><span data-stu-id="5e073-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="5e073-214">Ta funkcja ma zastosowanie tylko do naszych `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="5e073-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="5e073-215">Mechanizm rozpoznawania zależności do przekazania **MapSignalR** metody, dodając konfiguracji Centrum:</span><span class="sxs-lookup"><span data-stu-id="5e073-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="5e073-216">Zaktualizuj metodę Startup.ConfigureSignalR w klasie uruchamiania przykładowej nowy parametr:</span><span class="sxs-lookup"><span data-stu-id="5e073-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="5e073-217">Teraz SignalR będzie używać programu rozpoznawania nazw określonych w **MapSignalR**, zamiast domyślny mechanizm rozwiązywania konfliktów.</span><span class="sxs-lookup"><span data-stu-id="5e073-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="5e073-218">Oto kompletny kod dla `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="5e073-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="5e073-219">Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="5e073-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="5e073-220">W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="5e073-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="5e073-221">Aplikacja ma dokładnie taką samą funkcjonalność jak przed.</span><span class="sxs-lookup"><span data-stu-id="5e073-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="5e073-222">(Aby uzyskać opis, zobacz [emisje serwera z użyciem ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Firma Microsoft nie zostały zmienione zachowanie; po prostu ułatwiono kod do testowania, obsługa i rozwijać.</span><span class="sxs-lookup"><span data-stu-id="5e073-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
