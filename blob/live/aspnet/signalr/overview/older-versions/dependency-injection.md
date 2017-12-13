---
uid: signalr/overview/older-versions/dependency-injection
title: "Iniekcji zależności w bibliotece SignalR 1.x | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="4c4c6-102">Iniekcji zależności w bibliotece SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="4c4c6-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="4c4c6-103">przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4c4c6-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="4c4c6-104">Iniekcji zależności jest sposobem usunięcia ustalonych zależności między obiektami, ułatwiając użytkownikom, aby zastąpić obiekt zależności, albo do testowania (przy użyciu obiektów zasymulować) lub zmienić zachowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="4c4c6-105">W tym samouczku przedstawiono sposób wykonywania iniekcji zależności na koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="4c4c6-106">Ponadto sposobu używania kontenery Inwersja kontroli z SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="4c4c6-107">Kontener Inwersja kontroli jest ogólnych ram iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="4c4c6-108">Co to jest iniekcji zależności?</span><span class="sxs-lookup"><span data-stu-id="4c4c6-108">What is Dependency Injection?</span></span>

<span data-ttu-id="4c4c6-109">Pomiń tę sekcję, jeśli już znasz iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="4c4c6-110">*Iniekcji zależności* (Podpisane) to wzorzec, gdy nie są odpowiedzialne za tworzenie własnych zależności obiektów.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="4c4c6-111">Poniżej przedstawiono prosty przykład do Motywuj Podpisane.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="4c4c6-112">Załóżmy, że został wybrany obiekt, który musi komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="4c4c6-113">Można zdefiniować interfejsu rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="4c4c6-114">Obiekt, umożliwia tworzenie `ILogger` rejestrować komunikatów:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="4c4c6-115">To działa, ale nie jest najlepszym projektu.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-115">This works, but it's not the best design.</span></span> <span data-ttu-id="4c4c6-116">Jeśli chcesz zamienić `FileLogger` z inną `ILogger` implementacji, należy zmodyfikować `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="4c4c6-117">Przypuszczenia, że wiele innych obiektów użyj `FileLogger`, musisz zmienić ich wszystkich.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="4c4c6-118">Lub jeśli użytkownik chce utworzyć `FileLogger` pojedynczą, również należy wprowadzić zmiany w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="4c4c6-119">Lepszym rozwiązaniem jest "wstrzyknąć" `ILogger` do obiektu — na przykład za pomocą argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="4c4c6-120">Teraz obiektu nie odpowiada wybierania który `ILogger` do użycia.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="4c4c6-121">Możesz swich `ILogger` implementacje bez zmieniania obiektów, które od niego zależne.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="4c4c6-122">Ten wzorzec jest nazywany [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="4c4c6-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="4c4c6-123">Inny wzorzec jest iniekcji setter, których wartość zależności za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="4c4c6-124">Iniekcji zależności proste w SignalR</span><span class="sxs-lookup"><span data-stu-id="4c4c6-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="4c4c6-125">Należy wziąć pod uwagę aplikacji czatu z samouczka [wprowadzenie SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4c4c6-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="4c4c6-126">Oto klasy koncentratora od tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="4c4c6-127">Załóżmy, że chcesz przechowywanie wiadomości rozmów na serwerze przed ich wysłaniem.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="4c4c6-128">Może zdefiniować interfejs, który abstracts tę funkcję i użyć Podpisane iniekcję interfejs do `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="4c4c6-129">Problem polega na to, że aplikacji SignalR nie tworzy bezpośrednio koncentratory; SignalR utworzy je.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="4c4c6-130">Domyślnie SignalR oczekuje ma bezparametrowego konstruktora klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="4c4c6-131">Jednak użytkownik może łatwo rejestrować funkcję do tworzenia wystąpień koncentratorów i ta funkcja służy do wykonywania Podpisane.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="4c4c6-132">Zarejestruj przez wywołanie funkcji **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="4c4c6-133">Teraz SignalR wywoła tej funkcji anonimowej zawsze, gdy trzeba utworzyć `ChatHub` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="4c4c6-134">Kontenery Inwersja kontroli</span><span class="sxs-lookup"><span data-stu-id="4c4c6-134">IoC Containers</span></span>

<span data-ttu-id="4c4c6-135">Poprzedni kod wystarcza do prostych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="4c4c6-136">Jednak nadal musiały zapisu to:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="4c4c6-137">W złożonych aplikacji z wiele zależności konieczne może być napisać dużą ten kod "okablowania".</span><span class="sxs-lookup"><span data-stu-id="4c4c6-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="4c4c6-138">Ten kod może być trudno będzie utrzymać, zwłaszcza, jeśli są zagnieżdżone zależności.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="4c4c6-139">Również jest trudna do testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-139">It is also hard to unit test.</span></span>

<span data-ttu-id="4c4c6-140">Jedynym rozwiązaniem jest umieszczenie w kontenerze Inwersja kontroli.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="4c4c6-141">Kontenera IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależności. Zarejestrować typy z kontenerem, a następnie użyć do tworzenia obiektów kontenera.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="4c4c6-142">Kontener danych liczbowych automatycznie limit relacji zależności.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="4c4c6-143">Również wiele kontenerów Inwersja kontroli pozwala na kontrolowanie okres istnienia obiektów i zakresu.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="4c4c6-144">"IoC" oznacza "Inwersja kontroli", która jest wzorzec ogólne gdzie to struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="4c4c6-145">Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".</span><span class="sxs-lookup"><span data-stu-id="4c4c6-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="4c4c6-146">Używanie kontenerów Inwersja kontroli w SignalR</span><span class="sxs-lookup"><span data-stu-id="4c4c6-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="4c4c6-147">Prawdopodobnie zbyt proste do korzystania z kontenera IoC aplikacji czatu.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="4c4c6-148">Zamiast tego, Przyjrzyjmy się [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) próbki.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="4c4c6-149">Przykładowe StockTicker definiuje dwie klasy głównym:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="4c4c6-150">`StockTickerHub`: Centrum klasy, która zarządza połączeń klientów.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="4c4c6-151">`StockTicker`: Jako pojedyncza, która zawiera giełdowych i okresowo aktualizuje je.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="4c4c6-152">`StockTickerHub`zawiera odwołanie do `StockTicker` jako pojedynczej, podczas gdy `StockTicker` zawiera odwołanie do **IHubConnectionContext** dla `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="4c4c6-153">Ten interfejs używa do komunikacji z `StockTickerHub` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="4c4c6-154">(Aby uzyskać więcej informacji, zobacz [serwera emisji z ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="4c4c6-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="4c4c6-155">Możemy użyć kontenera IoC do nieco untangle te zależności.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="4c4c6-156">Po pierwsze odrobinę uprościmy badane `StockTickerHub` i `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="4c4c6-157">W poniższym kodzie I zostały oznaczone jako komentarz części firma Microsoft nie wymagają.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="4c4c6-158">Usuń konstruktora bez parametrów z `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="4c4c6-159">Zamiast tego należy zawsze używamy Podpisane utworzyć koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="4c4c6-160">W przypadku StockTicker usunąć pojedyncze wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="4c4c6-161">Później użyjemy kontenera IoC do sterowania StockTicker przez czas ich istnienia.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="4c4c6-162">Sprawdź także, konstruktora publicznego.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="4c4c6-163">Następnie możemy zrefaktoryzuj kod przez utworzenie interfejsu `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="4c4c6-164">Użyjemy tego interfejsu rozdzielenie `StockTickerHub` z `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="4c4c6-165">Visual Studio sprawia, że to rodzaj refaktoryzacji łatwe.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="4c4c6-166">Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy `StockTicker` deklaracji klasy, a następnie wybierz **Refaktoryzuj** ... **Wyodrębnij Interface**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="4c4c6-167">W **wyodrębniania interfejsu** okna dialogowego, kliknij przycisk **Zaznacz wszystko**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="4c4c6-168">Pozostaw wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-168">Leave the other defaults.</span></span> <span data-ttu-id="4c4c6-169">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="4c4c6-170">Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`, a także zmianę `StockTicker` pochodzić z `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="4c4c6-171">Otwórz plik IStockTicker.cs i zmienić interfejsu **publicznego**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="4c4c6-172">W `StockTickerHub` klasy, zmień dwa wystąpienia `StockTicker` do `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="4c4c6-173">Tworzenie `IStockTicker` interfejsu nie jest to niezbędne, ale miała pokazanie, jak Podpisane może pomóc zmniejszyć sprzężenia między składnikami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="4c4c6-174">Dodaj bibliotekę Ninject</span><span class="sxs-lookup"><span data-stu-id="4c4c6-174">Add the Ninject Library</span></span>

<span data-ttu-id="4c4c6-175">Istnieje wiele kontenerów Inwersja kontroli open source dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="4c4c6-176">W tym samouczku będziesz używać [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="4c4c6-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="4c4c6-177">(Obejmują innych popularnych bibliotek [Windsor zamku](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), i [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="4c4c6-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="4c4c6-178">Użyj Menedżera pakietów NuGet do zainstalowania [Ninject biblioteki](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="4c4c6-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="4c4c6-179">W programie Visual Studio z **narzędzia** menu wybierz opcję **Menedżer pakietów biblioteki** | **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="4c4c6-180">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="4c4c6-181">Zastąp mechanizmu rozpoznawania zależności biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="4c4c6-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="4c4c6-182">Aby użyć Ninject w SignalR, Utwórz klasę, która jest pochodną **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="4c4c6-183">Ta klasa zastępuje **GetService** i **metodę GetServices** metody **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="4c4c6-184">SignalR wywołania tych metod, aby utworzyć różnych obiektów w czasie wykonywania, w tym wystąpień koncentratorów, a także różne usługi używana wewnętrznie przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="4c4c6-185">**GetService** metoda tworzy pojedynczego wystąpienia typu.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="4c4c6-186">Przesłonić tę metodę do wywołania jądra Ninject **TryGet** metody.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="4c4c6-187">Jeśli ta metoda zwraca wartość null, wrócić do domyślny program rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="4c4c6-188">**Metodę GetServices** metoda tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="4c4c6-189">Zastępuje tę metodę do łączenia wyników z Ninject z wyników z mechanizmem rozpoznawania.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="4c4c6-190">Skonfiguruj powiązania Ninject</span><span class="sxs-lookup"><span data-stu-id="4c4c6-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="4c4c6-191">Teraz użyjemy Ninject Aby zadeklarować typ powiązania.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="4c4c6-192">Otwórz plik RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="4c4c6-193">W `RegisterHubs.Start` metody tworzenia kontenera Ninject, który wywołuje Ninject *jądra*.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="4c4c6-194">Utwórz wystąpienie naszych mechanizmu rozpoznawania zależności niestandardowych:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="4c4c6-195">Utwórz powiązanie dla `IStockTicker` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="4c4c6-196">Ten kod jest informacją dwie czynności.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-196">This code is saying two things.</span></span> <span data-ttu-id="4c4c6-197">Pierwsza strona, gdy aplikacja musi `IStockTicker`, jądra należy utworzyć wystąpienia `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="4c4c6-198">Drugi, `StockTicker` klasy powinny być utworzone jako pojedynczego obiektu.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="4c4c6-199">Ninject utworzyć jedno wystąpienie obiektu i zwraca to samo wystąpienie dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="4c4c6-200">Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="4c4c6-201">Ten kod creatres funkcji anonimowej, która zwraca **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="4c4c6-202">**WhenInjectedInto** metody informuje Ninject, aby użyć tej funkcji tylko podczas tworzenia `IStockTicker` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="4c4c6-203">Przyczyną jest to, że tworzy SignalR **IHubConnectionContext** wewnętrznie, wystąpienia i nie chcemy zastąpić, jak SignalR tworzy je.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="4c4c6-204">Ta funkcja ma zastosowanie tylko do naszej `StockTicker` klasy.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="4c4c6-205">Przekaż do mechanizmu rozpoznawania zależności **MapHubs** metody:</span><span class="sxs-lookup"><span data-stu-id="4c4c6-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="4c4c6-206">Teraz SignalR będzie używać programu rozpoznawania nazw, określonych w **MapHubs**, zamiast domyślny program rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="4c4c6-207">W tym miejscu jest kompletny kod dla `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="4c4c6-208">Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="4c4c6-209">W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="4c4c6-210">Aplikacja ma dokładnie tak samo jak przed.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="4c4c6-211">(Aby uzyskać opis, zobacz [serwera emisji z ASP.NET SignalR](index.md).) Firma Microsoft nie zostały zmienione zachowanie; po prostu łatwiejsza kod do testowania, obsługa i rozwijać.</span><span class="sxs-lookup"><span data-stu-id="4c4c6-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
