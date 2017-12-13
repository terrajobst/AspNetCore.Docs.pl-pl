---
uid: signalr/overview/advanced/dependency-injection
title: "Iniekcji zależności w bibliotece SignalR | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Wersje oprogramowania używane w tym temacie Visual Studio 2013 .NET 4.5 SignalR w wersji 2 poprzednie wersje tego tematu informacji o wcześniejszych wersji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="daee1-103">Iniekcji zależności w bibliotece SignalR</span><span class="sxs-lookup"><span data-stu-id="daee1-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="daee1-104">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="daee1-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="daee1-105">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="daee1-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="daee1-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="daee1-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="daee1-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="daee1-107">.NET 4.5</span></span>
> - <span data-ttu-id="daee1-108">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="daee1-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="daee1-109">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="daee1-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="daee1-110">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="daee1-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="daee1-111">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="daee1-111">Questions and comments</span></span>
> 
> <span data-ttu-id="daee1-112">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="daee1-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="daee1-113">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="daee1-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="daee1-114">Iniekcji zależności jest sposobem usunięcia ustalonych zależności między obiektami, ułatwiając użytkownikom, aby zastąpić obiekt zależności, albo do testowania (przy użyciu obiektów zasymulować) lub zmienić zachowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="daee1-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="daee1-115">W tym samouczku przedstawiono sposób wykonywania iniekcji zależności na koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="daee1-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="daee1-116">Ponadto sposobu używania kontenery Inwersja kontroli z SignalR.</span><span class="sxs-lookup"><span data-stu-id="daee1-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="daee1-117">Kontener Inwersja kontroli jest ogólnych ram iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="daee1-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="daee1-118">Co to jest iniekcji zależności?</span><span class="sxs-lookup"><span data-stu-id="daee1-118">What is Dependency Injection?</span></span>

<span data-ttu-id="daee1-119">Pomiń tę sekcję, jeśli już znasz iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="daee1-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="daee1-120">*Iniekcji zależności* (Podpisane) to wzorzec, gdy nie są odpowiedzialne za tworzenie własnych zależności obiektów.</span><span class="sxs-lookup"><span data-stu-id="daee1-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="daee1-121">Poniżej przedstawiono prosty przykład do Motywuj Podpisane.</span><span class="sxs-lookup"><span data-stu-id="daee1-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="daee1-122">Załóżmy, że został wybrany obiekt, który musi komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="daee1-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="daee1-123">Można zdefiniować interfejsu rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="daee1-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="daee1-124">Obiekt, umożliwia tworzenie `ILogger` rejestrować komunikatów:</span><span class="sxs-lookup"><span data-stu-id="daee1-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="daee1-125">To działa, ale nie jest najlepszym projektu.</span><span class="sxs-lookup"><span data-stu-id="daee1-125">This works, but it's not the best design.</span></span> <span data-ttu-id="daee1-126">Jeśli chcesz zamienić `FileLogger` z inną `ILogger` implementacji, należy zmodyfikować `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="daee1-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="daee1-127">Przypuszczenia, że wiele innych obiektów użyj `FileLogger`, musisz zmienić ich wszystkich.</span><span class="sxs-lookup"><span data-stu-id="daee1-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="daee1-128">Lub jeśli użytkownik chce utworzyć `FileLogger` pojedynczą, również należy wprowadzić zmiany w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="daee1-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="daee1-129">Lepszym rozwiązaniem jest "wstrzyknąć" `ILogger` do obiektu — na przykład za pomocą argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="daee1-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="daee1-130">Teraz obiektu nie odpowiada wybierania który `ILogger` do użycia.</span><span class="sxs-lookup"><span data-stu-id="daee1-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="daee1-131">Możesz swich `ILogger` implementacje bez zmieniania obiektów, które od niego zależne.</span><span class="sxs-lookup"><span data-stu-id="daee1-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="daee1-132">Ten wzorzec jest nazywany [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="daee1-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="daee1-133">Inny wzorzec jest iniekcji setter, których wartość zależności za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="daee1-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="daee1-134">Iniekcji zależności proste w SignalR</span><span class="sxs-lookup"><span data-stu-id="daee1-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="daee1-135">Należy wziąć pod uwagę aplikacji czatu z samouczka [wprowadzenie SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="daee1-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="daee1-136">Oto klasy koncentratora od tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="daee1-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="daee1-137">Załóżmy, że chcesz przechowywanie wiadomości rozmów na serwerze przed ich wysłaniem.</span><span class="sxs-lookup"><span data-stu-id="daee1-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="daee1-138">Może zdefiniować interfejs, który abstracts tę funkcję i użyć Podpisane iniekcję interfejs do `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="daee1-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="daee1-139">Problem polega na to, że aplikacji SignalR nie tworzy bezpośrednio koncentratory; SignalR utworzy je.</span><span class="sxs-lookup"><span data-stu-id="daee1-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="daee1-140">Domyślnie SignalR oczekuje ma bezparametrowego konstruktora klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="daee1-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="daee1-141">Jednak użytkownik może łatwo rejestrować funkcję do tworzenia wystąpień koncentratorów i ta funkcja służy do wykonywania Podpisane.</span><span class="sxs-lookup"><span data-stu-id="daee1-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="daee1-142">Zarejestruj przez wywołanie funkcji **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="daee1-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="daee1-143">Teraz SignalR wywoła tej funkcji anonimowej zawsze, gdy trzeba utworzyć `ChatHub` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="daee1-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="daee1-144">Kontenery Inwersja kontroli</span><span class="sxs-lookup"><span data-stu-id="daee1-144">IoC Containers</span></span>

<span data-ttu-id="daee1-145">Poprzedni kod wystarcza do prostych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="daee1-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="daee1-146">Jednak nadal musiały zapisu to:</span><span class="sxs-lookup"><span data-stu-id="daee1-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="daee1-147">W złożonych aplikacji z wiele zależności konieczne może być napisać dużą ten kod "okablowania".</span><span class="sxs-lookup"><span data-stu-id="daee1-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="daee1-148">Ten kod może być trudno będzie utrzymać, zwłaszcza, jeśli są zagnieżdżone zależności.</span><span class="sxs-lookup"><span data-stu-id="daee1-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="daee1-149">Również jest trudna do testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="daee1-149">It is also hard to unit test.</span></span>

<span data-ttu-id="daee1-150">Jedynym rozwiązaniem jest umieszczenie w kontenerze Inwersja kontroli.</span><span class="sxs-lookup"><span data-stu-id="daee1-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="daee1-151">Kontenera IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależności. Zarejestrować typy z kontenerem, a następnie użyć do tworzenia obiektów kontenera.</span><span class="sxs-lookup"><span data-stu-id="daee1-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="daee1-152">Kontener danych liczbowych automatycznie limit relacji zależności.</span><span class="sxs-lookup"><span data-stu-id="daee1-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="daee1-153">Również wiele kontenerów Inwersja kontroli pozwala na kontrolowanie okres istnienia obiektów i zakresu.</span><span class="sxs-lookup"><span data-stu-id="daee1-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="daee1-154">"IoC" oznacza "Inwersja kontroli", która jest wzorzec ogólne gdzie to struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="daee1-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="daee1-155">Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".</span><span class="sxs-lookup"><span data-stu-id="daee1-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="daee1-156">Używanie kontenerów Inwersja kontroli w SignalR</span><span class="sxs-lookup"><span data-stu-id="daee1-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="daee1-157">Prawdopodobnie zbyt proste do korzystania z kontenera IoC aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="daee1-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="daee1-158">Zamiast tego, Przyjrzyjmy się [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) próbki.</span><span class="sxs-lookup"><span data-stu-id="daee1-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="daee1-159">Przykładowe StockTicker definiuje dwie klasy głównym:</span><span class="sxs-lookup"><span data-stu-id="daee1-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="daee1-160">`StockTickerHub`: Centrum klasy, która zarządza połączeń klientów.</span><span class="sxs-lookup"><span data-stu-id="daee1-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="daee1-161">`StockTicker`: Jako pojedyncza, która zawiera giełdowych i okresowo aktualizuje je.</span><span class="sxs-lookup"><span data-stu-id="daee1-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="daee1-162">`StockTickerHub`zawiera odwołanie do `StockTicker` jako pojedynczej, podczas gdy `StockTicker` zawiera odwołanie do **IHubConnectionContext** dla `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="daee1-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="daee1-163">Ten interfejs używa do komunikacji z `StockTickerHub` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="daee1-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="daee1-164">(Aby uzyskać więcej informacji, zobacz [serwera emisji z ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="daee1-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="daee1-165">Możemy użyć kontenera IoC do nieco untangle te zależności.</span><span class="sxs-lookup"><span data-stu-id="daee1-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="daee1-166">Po pierwsze odrobinę uprościmy badane `StockTickerHub` i `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="daee1-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="daee1-167">W poniższym kodzie I zostały oznaczone jako komentarz części firma Microsoft nie wymagają.</span><span class="sxs-lookup"><span data-stu-id="daee1-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="daee1-168">Usuń konstruktora bez parametrów z `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="daee1-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="daee1-169">Zamiast tego należy zawsze używamy Podpisane utworzyć koncentratora.</span><span class="sxs-lookup"><span data-stu-id="daee1-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="daee1-170">W przypadku StockTicker usunąć pojedyncze wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="daee1-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="daee1-171">Później użyjemy kontenera IoC do sterowania StockTicker przez czas ich istnienia.</span><span class="sxs-lookup"><span data-stu-id="daee1-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="daee1-172">Sprawdź także, konstruktora publicznego.</span><span class="sxs-lookup"><span data-stu-id="daee1-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="daee1-173">Następnie możemy zrefaktoryzuj kod przez utworzenie interfejsu `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="daee1-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="daee1-174">Użyjemy tego interfejsu rozdzielenie `StockTickerHub` z `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="daee1-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="daee1-175">Visual Studio sprawia, że to rodzaj refaktoryzacji łatwe.</span><span class="sxs-lookup"><span data-stu-id="daee1-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="daee1-176">Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy `StockTicker` deklaracji klasy, a następnie wybierz **Refaktoryzuj** ... **Wyodrębnij Interface**.</span><span class="sxs-lookup"><span data-stu-id="daee1-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="daee1-177">W **wyodrębniania interfejsu** okna dialogowego, kliknij przycisk **Zaznacz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="daee1-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="daee1-178">Pozostaw wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="daee1-178">Leave the other defaults.</span></span> <span data-ttu-id="daee1-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="daee1-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="daee1-180">Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`, a także zmianę `StockTicker` pochodzić z `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="daee1-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="daee1-181">Otwórz plik IStockTicker.cs i zmienić interfejsu **publicznego**.</span><span class="sxs-lookup"><span data-stu-id="daee1-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="daee1-182">W `StockTickerHub` klasy, zmień dwa wystąpienia `StockTicker` do `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="daee1-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="daee1-183">Tworzenie `IStockTicker` interfejsu nie jest to niezbędne, ale miała pokazanie, jak Podpisane może pomóc zmniejszyć sprzężenia między składnikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="daee1-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="daee1-184">Dodaj bibliotekę Ninject</span><span class="sxs-lookup"><span data-stu-id="daee1-184">Add the Ninject Library</span></span>

<span data-ttu-id="daee1-185">Istnieje wiele kontenerów Inwersja kontroli open source dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="daee1-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="daee1-186">W tym samouczku będziesz używać [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="daee1-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="daee1-187">(Obejmują innych popularnych bibliotek [Windsor zamku](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), i [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="daee1-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="daee1-188">Użyj Menedżera pakietów NuGet do zainstalowania [Ninject biblioteki](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="daee1-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="daee1-189">W programie Visual Studio z **narzędzia** menu wybierz opcję **Menedżer pakietów biblioteki** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="daee1-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="daee1-190">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="daee1-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="daee1-191">Zastąp mechanizmu rozpoznawania zależności biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="daee1-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="daee1-192">Aby użyć Ninject w SignalR, Utwórz klasę, która jest pochodną **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="daee1-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="daee1-193">Ta klasa zastępuje **GetService** i **metodę GetServices** metody **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="daee1-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="daee1-194">SignalR wywołania tych metod, aby utworzyć różnych obiektów w czasie wykonywania, w tym wystąpień koncentratorów, a także różne usługi używana wewnętrznie przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="daee1-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="daee1-195">**GetService** metoda tworzy pojedynczego wystąpienia typu.</span><span class="sxs-lookup"><span data-stu-id="daee1-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="daee1-196">Przesłonić tę metodę do wywołania jądra Ninject **TryGet** metody.</span><span class="sxs-lookup"><span data-stu-id="daee1-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="daee1-197">Jeśli ta metoda zwraca wartość null, wrócić do domyślny program rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="daee1-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="daee1-198">**Metodę GetServices** metoda tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="daee1-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="daee1-199">Zastępuje tę metodę do łączenia wyników z Ninject z wyników z mechanizmem rozpoznawania.</span><span class="sxs-lookup"><span data-stu-id="daee1-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="daee1-200">Skonfiguruj powiązania Ninject</span><span class="sxs-lookup"><span data-stu-id="daee1-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="daee1-201">Teraz użyjemy Ninject Aby zadeklarować typ powiązania.</span><span class="sxs-lookup"><span data-stu-id="daee1-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="daee1-202">Otwórz klasy Startup.cs aplikacji (albo utworzony ręcznie zgodnie z instrukcjami pakietu `readme.txt`, i który został utworzony przez dodawanie uwierzytelniania do projektu).</span><span class="sxs-lookup"><span data-stu-id="daee1-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="daee1-203">W `Startup.Configuration` metody tworzenia kontenera Ninject, który wywołuje Ninject *jądra*.</span><span class="sxs-lookup"><span data-stu-id="daee1-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="daee1-204">Utwórz wystąpienie naszych mechanizmu rozpoznawania zależności niestandardowych:</span><span class="sxs-lookup"><span data-stu-id="daee1-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="daee1-205">Utwórz powiązanie dla `IStockTicker` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="daee1-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="daee1-206">Ten kod jest informacją dwie czynności.</span><span class="sxs-lookup"><span data-stu-id="daee1-206">This code is saying two things.</span></span> <span data-ttu-id="daee1-207">Pierwsza strona, gdy aplikacja musi `IStockTicker`, jądra należy utworzyć wystąpienia `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="daee1-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="daee1-208">Drugi, `StockTicker` klasy powinny być utworzone jako pojedynczego obiektu.</span><span class="sxs-lookup"><span data-stu-id="daee1-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="daee1-209">Ninject utworzyć jedno wystąpienie obiektu i zwraca to samo wystąpienie dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="daee1-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="daee1-210">Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="daee1-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="daee1-211">Ten kod creatres funkcji anonimowej, która zwraca **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="daee1-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="daee1-212">**WhenInjectedInto** metody informuje Ninject, aby użyć tej funkcji tylko podczas tworzenia `IStockTicker` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="daee1-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="daee1-213">Przyczyną jest to, że tworzy SignalR **IHubConnectionContext** wewnętrznie, wystąpienia i nie chcemy zastąpić, jak SignalR tworzy je.</span><span class="sxs-lookup"><span data-stu-id="daee1-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="daee1-214">Ta funkcja ma zastosowanie tylko do naszej `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="daee1-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="daee1-215">Przekaż do mechanizmu rozpoznawania zależności **MapSignalR** metody przez dodanie konfiguracji Centrum:</span><span class="sxs-lookup"><span data-stu-id="daee1-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="daee1-216">Zaktualizuj metodę Startup.ConfigureSignalR w klasie uruchamiania próbki o nowy parametr:</span><span class="sxs-lookup"><span data-stu-id="daee1-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="daee1-217">Teraz SignalR będzie używać programu rozpoznawania nazw, określonych w **MapSignalR**, zamiast domyślny program rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="daee1-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="daee1-218">W tym miejscu jest kompletny kod dla `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="daee1-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="daee1-219">Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="daee1-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="daee1-220">W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="daee1-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="daee1-221">Aplikacja ma dokładnie tak samo jak przed.</span><span class="sxs-lookup"><span data-stu-id="daee1-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="daee1-222">(Aby uzyskać opis, zobacz [serwera emisji z ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Firma Microsoft nie zostały zmienione zachowanie; po prostu łatwiejsza kod do testowania, obsługa i rozwijać.</span><span class="sxs-lookup"><span data-stu-id="daee1-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
