---
uid: web-api/overview/advanced/dependency-injection
title: Wstrzykiwanie zależności we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak wstrzykiwanie zależności do poziomu kontrolera interfejsu API sieci Web platformy ASP.NET. Wersje oprogramowania używanych w samouczek zablokowanych witryn sieci Web API 2 Unity aplikacji...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 318a2f1c587feb360212a390bb5de7bdc127513d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341829"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="ec3f2-104">Wstrzykiwanie zależności we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ec3f2-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="ec3f2-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ec3f2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ec3f2-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="ec3f2-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="ec3f2-107">W tym samouczku pokazano, jak wstrzykiwanie zależności do poziomu kontrolera interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ec3f2-108">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="ec3f2-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ec3f2-109">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="ec3f2-109">Web API 2</span></span>
> - [<span data-ttu-id="ec3f2-110">Blok aplikacji Unity</span><span class="sxs-lookup"><span data-stu-id="ec3f2-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="ec3f2-111">Entity Framework 6 (działa także w wersji 5)</span><span class="sxs-lookup"><span data-stu-id="ec3f2-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="ec3f2-112">Co to jest wstrzykiwanie zależności?</span><span class="sxs-lookup"><span data-stu-id="ec3f2-112">What is Dependency Injection?</span></span>

<span data-ttu-id="ec3f2-113">A *zależności* jest dowolny obiekt, który wymaga innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="ec3f2-114">Na przykład, jest często zdefiniować [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) obsługująca dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="ec3f2-115">Teraz pokazują z przykładem.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-115">Let's illustrate with an example.</span></span> <span data-ttu-id="ec3f2-116">Po pierwsze zdefiniujemy modelu domeny:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="ec3f2-117">W tym miejscu jest klasą prostego repozytorium, która przechowuje elementy w bazie danych przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="ec3f2-118">Teraz czynnością jest zdefiniowanie kontroler Web API, który obsługuje żądania GET `Product` jednostek.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="ec3f2-119">(Czy mam pomijając POST i innych metod dla uproszczenia.) W tym miejscu jest pierwsza próba:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="ec3f2-120">Należy zauważyć, że klasa kontrolera jest zależna od `ProductRepository`, i zawiadamiamy kontroler tworzenia `ProductRepository` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="ec3f2-121">Jednak jest to zły pomysł kodowi twardych zależności w ten sposób z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="ec3f2-122">Jeśli chcesz zastąpić `ProductRepository` z inną implementacją, trzeba będzie również zmodyfikować klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="ec3f2-123">Jeśli `ProductRepository` ma zależności, należy skonfigurować w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="ec3f2-124">Dla dużego projektu za pomocą wielu kontrolerów kodu konfiguracji staje się rozproszone w projekcie.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="ec3f2-125">Ciężko jest testy jednostkowe, ponieważ kontroler jest ustalony do wykonywania zapytań w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="ec3f2-126">Dla testu jednostkowego należy użyć pozorny ani klas zastępczych repozytorium, które nie jest możliwe z projektem currect.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="ec3f2-127">Można rozwiązać te problemy, zapewniając *wprowadza* repozytorium do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="ec3f2-128">Po pierwsze, Refaktoryzuj `ProductRepository` klasy w interfejsie:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="ec3f2-129">Następnie podaj `IProductRepository` jako parametr konstruktora:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="ec3f2-130">W tym przykładzie użyto [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="ec3f2-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="ec3f2-131">Można również użyć *iniekcji metody ustawiającej*, gdzie ustawiasz zależności za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="ec3f2-132">Ale teraz występuje problem, ponieważ aplikacja nie tworzy kontroler bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="ec3f2-133">Interfejs API sieci Web tworzy kontroler, kieruje żądanie, gdy interfejs API sieci Web nie nic wiedzieć o `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="ec3f2-134">Jest to, gdzie mechanizmu rozpoznawania zależności interfejsu API sieci Web jest dostępna w.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="ec3f2-135">Mechanizm rozpoznawania zależności interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="ec3f2-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="ec3f2-136">Definiuje interfejs API sieci Web **elementu IDependencyResolver** interfejs służący do rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="ec3f2-137">Oto definicja interfejsu:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="ec3f2-138">**IDependencyScope** interfejs ma dwie metody:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="ec3f2-139">**GetService** tworzy jedno wystąpienie typu.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="ec3f2-140">**Funkcji GetServices** tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="ec3f2-141">**Elementu IDependencyResolver** dziedziczy metodę **IDependencyScope** i dodaje **BeginScope** metody.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="ec3f2-142">Omówię zakresów w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="ec3f2-143">Kiedy internetowy interfejs API tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver.GetService**, przekazując typ kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="ec3f2-144">Tego punktu zaczepienia rozszerzalności umożliwia tworzenie kontrolera, rozwiązywanie wszelkich zależności.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="ec3f2-145">Jeśli **GetService** zwraca wartość null, internetowy interfejs API szuka konstruktorem klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="ec3f2-146">Rozpoznawanie zależności za pomocą kontenera aparatu Unity</span><span class="sxs-lookup"><span data-stu-id="ec3f2-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="ec3f2-147">Mimo że można napisać kompletna **elementu IDependencyResolver** wdrożenia od zera, interfejs naprawdę jest przeznaczony do działania jako Most między interfejsu API sieci Web i istniejące kontenery IoC.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="ec3f2-148">Kontenera IoC jest składnikiem oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="ec3f2-149">Rejestrowanie typów z kontenerem, a następnie użyć do tworzenia obiektów kontenera.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="ec3f2-150">Kontener automatycznie wpadł na pomysł relacji zależności.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="ec3f2-151">Wiele kontenerów IoC umożliwiają także kontrolowanie elementów, takich jak okres istnienia obiektów i zakresu.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="ec3f2-152">"IoC" oznacza "Inwersja kontroli", który jest ogólny wzorzec gdzie struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="ec3f2-153">Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".</span><span class="sxs-lookup"><span data-stu-id="ec3f2-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="ec3f2-154">W tym samouczku użyjemy [Unity](https://msdn.microsoft.com/library/ff647202.aspx) z Microsoft Patterns &amp; rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="ec3f2-155">(Obejmują innych popularnych bibliotek [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), i [StructureMap ](http://structuremap.github.io/documentation/).) Menedżer pakietów NuGet można użyć do zainstalowania aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="ec3f2-156">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-156">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ec3f2-157">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="ec3f2-158">Oto implementacja **elementu IDependencyResolver** wszystko w kontenerze aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="ec3f2-159">Jeśli **GetService** metody nie można rozpoznać typu, powinna zwrócić **null**.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="ec3f2-160">Jeśli **funkcji GetServices** metody nie można rozpoznać typu, powinien zostać zwrócony obiekt pustą kolekcję.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="ec3f2-161">Nie zgłaszają wyjątki dla nieznanego typu.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="ec3f2-162">Konfigurowanie mechanizmu rozpoznawania zależności</span><span class="sxs-lookup"><span data-stu-id="ec3f2-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="ec3f2-163">Nastavit mechanizmu rozpoznawania zależności **klasy DependencyResolver** właściwości globalnego **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="ec3f2-164">Poniższy kod rejestruje `IProductRepository` interfejs za pomocą aparatu Unity, a następnie tworzy `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="ec3f2-165">Zakres zależności i okresem istnienia kontrolera</span><span class="sxs-lookup"><span data-stu-id="ec3f2-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="ec3f2-166">Kontrolery są tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-166">Controllers are created per request.</span></span> <span data-ttu-id="ec3f2-167">Do zarządzania okresy istnienia obiektu, **elementu IDependencyResolver** korzysta z koncepcji *zakres*.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="ec3f2-168">Mechanizm rozpoznawania zależności dołączone do **HttpConfiguration** obiekt ma zakres globalny.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="ec3f2-169">Kiedy internetowy interfejs API tworzy kontroler, wywołuje **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="ec3f2-170">Ta metoda zwraca **IDependencyScope** reprezentujący zakresem podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="ec3f2-171">Następnie wywołuje internetowy interfejs API **GetService** w zakresie podrzędnym, aby utworzyć kontroler.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="ec3f2-172">Po zakończeniu żądania wywołania interfejsu API sieci Web **Dispose** w zakresie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="ec3f2-173">Użyj **Dispose** metodę dispose zależności kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="ec3f2-174">Jak zaimplementować **BeginScope** zależy od kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="ec3f2-175">Zakres dla aparatu Unity, odnosi się do kontenerów podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="ec3f2-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="ec3f2-176">Większość kontenery IoC mają podobne odpowiedniki.</span><span class="sxs-lookup"><span data-stu-id="ec3f2-176">Most IoC containers have similar equivalents.</span></span>
