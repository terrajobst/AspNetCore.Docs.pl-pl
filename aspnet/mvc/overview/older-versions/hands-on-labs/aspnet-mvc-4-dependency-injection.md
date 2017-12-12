---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Iniekcji zależności platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Uwaga: W tym laboratorium Hands-on przyjęto założenie, że masz podstawową wiedzę na temat filtrów platformy ASP.NET MVC i platformy ASP.NET MVC 4. Jeśli nie użyto filtrów platformy ASP.NET MVC 4 przed możemy rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="62a0b-104">Iniekcji zależności platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="62a0b-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="62a0b-105">przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="62a0b-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="62a0b-106">W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC** i **filtrów platformy ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="62a0b-107">Jeśli nie używasz **filtrów platformy ASP.NET MVC 4** przed, zalecamy zapoznać się z **filtry akcji niestandardowych MVC ASP.NET** Hands-on laboratorium.</span><span class="sxs-lookup"><span data-stu-id="62a0b-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="62a0b-108">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="62a0b-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="62a0b-109">W **programowanie zorientowane na obiekt** modelu, obiekty współdziałają ze sobą w modelu współpracy w przypadku, gdy istnieją współautorzy i konsumentów.</span><span class="sxs-lookup"><span data-stu-id="62a0b-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="62a0b-110">Oczywiście ten model komunikacji generuje zależności między obiektami i składników, staje się trudno było zarządzać podczas zwiększa złożoności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="62a0b-111">![Klasa zależności i modelu złożoności](aspnet-mvc-4-dependency-injection/_static/image1.png "klasy zależności i modelu złożoności")</span><span class="sxs-lookup"><span data-stu-id="62a0b-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="62a0b-112">*Klasa zależności i złożoność modelu*</span><span class="sxs-lookup"><span data-stu-id="62a0b-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="62a0b-113">Prawdopodobnie ma wiesz o **wzorzec fabryki** i rozdzielenie interfejs i wdrożenia przy użyciu usług, gdy obiekty klienta często są odpowiedzialne za lokalizacji usługi.</span><span class="sxs-lookup"><span data-stu-id="62a0b-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="62a0b-114">Wzorzec iniekcji zależności jest konkretnej implementacji Inwersja kontroli.</span><span class="sxs-lookup"><span data-stu-id="62a0b-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="62a0b-115">**Inwersja kontroli (IoC)** oznacza, że obiektów należy tworzyć inne obiekty, które opierają się ich w pracy.</span><span class="sxs-lookup"><span data-stu-id="62a0b-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="62a0b-116">Zamiast tego otrzymują obiektów, które potrzebują ze źródła zewnętrznego (na przykład plik konfiguracyjny xml).</span><span class="sxs-lookup"><span data-stu-id="62a0b-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="62a0b-117">**Zależności Iniekcja** oznacza, że jest to zrobić bez interwencji obiektu zwykle przez składnik framework, która przekazuje parametry konstruktora i ustaw właściwości.</span><span class="sxs-lookup"><span data-stu-id="62a0b-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="62a0b-118">Wzorzec projektowy (Podpisane) iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="62a0b-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="62a0b-119">Na wysokim poziomie celem iniekcji zależności jest to, że klasa klienta (np. *golfer*) wymaga czegoś, która obsługuje interfejs (np. *IClub*).</span><span class="sxs-lookup"><span data-stu-id="62a0b-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="62a0b-120">Nowe zależy, co to jest konkretnego typu (np. *WedgeClub WoodClub, IronClub,* lub *PutterClub*), ktoś inny do obsługi, który chce (np. jest dobrą *caddy*).</span><span class="sxs-lookup"><span data-stu-id="62a0b-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="62a0b-121">Mechanizm rozpoznawania zależności na platformie ASP.NET MVC umożliwia zarejestrować gdzieś logiki zależności (np. kontener lub *zbioru klubów*).</span><span class="sxs-lookup"><span data-stu-id="62a0b-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="62a0b-122">![Diagram iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustracji iniekcji zależności")</span><span class="sxs-lookup"><span data-stu-id="62a0b-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="62a0b-123">*Iniekcji zależności - odpowiednio pól*</span><span class="sxs-lookup"><span data-stu-id="62a0b-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="62a0b-124">Zalety korzystania z wzorca iniekcji zależności oraz Inwersja kontroli są następujące:</span><span class="sxs-lookup"><span data-stu-id="62a0b-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="62a0b-125">Zmniejsza sprzężenia klas</span><span class="sxs-lookup"><span data-stu-id="62a0b-125">Reduces class coupling</span></span>
- <span data-ttu-id="62a0b-126">Zwiększa ponowne wykorzystywanie kodu</span><span class="sxs-lookup"><span data-stu-id="62a0b-126">Increases code reusing</span></span>
- <span data-ttu-id="62a0b-127">Zwiększa utrzymanie kodu</span><span class="sxs-lookup"><span data-stu-id="62a0b-127">Improves code maintainability</span></span>
- <span data-ttu-id="62a0b-128">Zwiększa testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="62a0b-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="62a0b-129">Iniekcji zależności czasami jest porównywany ze wzorca projektowego fabryki abstrakcyjny, ale istnieje niewielka różnica między obu podejść.</span><span class="sxs-lookup"><span data-stu-id="62a0b-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="62a0b-130">Podpisane ma środowisko pracy za rozwiązać zależności w wywołaniu fabryk i zarejestrowane usługi.</span><span class="sxs-lookup"><span data-stu-id="62a0b-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="62a0b-131">Znasz wzorzec iniekcji zależności dowiesz w tym laboratorium sposób stosowania go w programie ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="62a0b-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="62a0b-132">Zostanie uruchomiona, przy użyciu iniekcji zależności w **kontrolerów** uwzględnienie usługi dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="62a0b-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="62a0b-133">Następnie należy zastosować iniekcji zależności do **widoków** do wykorzystania usługi i zawierają informacje.</span><span class="sxs-lookup"><span data-stu-id="62a0b-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="62a0b-134">Na koniec rozszerzenie Podpisane do platformy ASP.NET MVC 4 filtrów, zostanie wstrzyknięta filtr akcji niestandardowej w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="62a0b-135">W tym laboratorium Hands-on przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="62a0b-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="62a0b-136">Integracja programu ASP.NET MVC 4 z Unity iniekcji zależności za pomocą pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="62a0b-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="62a0b-137">Iniekcji zależności w obrębie kontrolera ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="62a0b-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="62a0b-138">Iniekcji zależności w widoku programu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="62a0b-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="62a0b-139">Iniekcji zależności wewnątrz filtr akcji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="62a0b-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="62a0b-140">W tym laboratorium jest używany pakiet NuGet Unity.Mvc3 rozpoznawania zależności, ale istnieje możliwość dostosowania dowolnej architektury iniekcji zależności do pracy z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="62a0b-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="62a0b-141">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="62a0b-141">Prerequisites</span></span>

<span data-ttu-id="62a0b-142">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="62a0b-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="62a0b-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="62a0b-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="62a0b-144">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="62a0b-144">Setup</span></span>

<span data-ttu-id="62a0b-145">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="62a0b-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="62a0b-146">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="62a0b-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="62a0b-147">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="62a0b-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="62a0b-148">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="62a0b-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="62a0b-149">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="62a0b-149">Exercises</span></span>

<span data-ttu-id="62a0b-150">W tym laboratorium Hands-On składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="62a0b-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="62a0b-151">Ćwiczenie 1: Wstrzyknięcie kontrolera</span><span class="sxs-lookup"><span data-stu-id="62a0b-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="62a0b-152">Ćwiczenie 2: Wstrzyknięcie widoku</span><span class="sxs-lookup"><span data-stu-id="62a0b-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="62a0b-153">Ćwiczenie 3: Wstrzyknięcie filtry</span><span class="sxs-lookup"><span data-stu-id="62a0b-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="62a0b-154">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="62a0b-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="62a0b-155">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="62a0b-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="62a0b-156">Szacowany czas trwania tego laboratorium: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="62a0b-157">Ćwiczenie 1: Wstrzyknięcie kontrolera</span><span class="sxs-lookup"><span data-stu-id="62a0b-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="62a0b-158">W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w kontrolery ASP.NET MVC, integrując Unity przy użyciu pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="62a0b-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="62a0b-159">Z tego powodu będzie dołączać usług do kontrolerów MvcMusicStore do oddzielania logiki z dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="62a0b-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="62a0b-160">Usługi spowoduje utworzenie nowej zależności w Konstruktorze kontrolera, który zostanie rozwiązany przy użyciu iniekcji zależności za pomocą **Unity**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="62a0b-161">Takie podejście opisano sposób generowania mniej sprzężonego aplikacji, które są bardziej elastyczne i łatwiejsze do utrzymywania i testowania.</span><span class="sxs-lookup"><span data-stu-id="62a0b-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="62a0b-162">Zostanie również sposób integracji ASP.NET MVC z Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="62a0b-163">Usługa StoreManager informacje</span><span class="sxs-lookup"><span data-stu-id="62a0b-163">About StoreManager Service</span></span>

<span data-ttu-id="62a0b-164">Magazyn utworów muzycznych MVC podane w rozwiązaniu Rozpocznij teraz obejmują usługę, która zarządza danymi kontrolera magazynu o nazwie **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="62a0b-165">Poniżej przedstawiono implementacji usługi magazynu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="62a0b-166">Należy pamiętać, że wszystkie metody zwraca jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="62a0b-167">**StoreController** BEGIN teraz zużywa rozwiązania **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="62a0b-168">Usunięto wszystkie odwołania do danych z **StoreController**i teraz można modyfikować bieżącego dostawcę dostępu do danych bez zmieniania dowolnej metody, który wykorzystuje **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="62a0b-169">Znajdują się poniżej **StoreController** implementacji ma zależność o **StoreService** wewnątrz konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="62a0b-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="62a0b-170">Zależności wprowadzone w tym ćwiczeniu jest powiązany z **Inwersja kontroli** (IoC).</span><span class="sxs-lookup"><span data-stu-id="62a0b-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="62a0b-171">**StoreController** odbiera konstruktora klasy **IStoreService** parametr typu, które są niezbędne do wykonania wywołania usługi z wewnątrz klasy.</span><span class="sxs-lookup"><span data-stu-id="62a0b-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="62a0b-172">Jednak **StoreController** nie implementuje konstruktora domyślnego (z żadnych parametrów), że każdy kontroler musi mieć do pracy z platformą ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="62a0b-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="62a0b-173">Aby rozwiązać zależności, kontrolera musi być utworzony przez fabrykę abstrakcyjny (Klasa zwraca dowolnego obiektu określonego typu).</span><span class="sxs-lookup"><span data-stu-id="62a0b-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="62a0b-174">Wystąpił błąd wystąpi po klasie próbuje utworzyć StoreController bez wysyłania obiektu usługi, ponieważ nie istnieje żaden konstruktor bez parametrów zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="62a0b-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="62a0b-175">Zadanie 1 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="62a0b-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="62a0b-176">To zadanie uruchomi aplikacji Begin, która obejmuje usługę do kontrolera magazynu, która oddziela dostęp do danych z aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="62a0b-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="62a0b-177">Podczas uruchamiania aplikacji, jak usługa kontrolera nie zostanie przekazany jako parametr domyślnie zostanie wyświetlony Wystąpił wyjątek:</span><span class="sxs-lookup"><span data-stu-id="62a0b-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="62a0b-178">Otwórz **rozpocząć** rozwiązania, znajdujących się w **wstrzyknięcie Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="62a0b-179">Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="62a0b-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="62a0b-180">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="62a0b-181">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="62a0b-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="62a0b-182">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="62a0b-183">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="62a0b-184">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="62a0b-185">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="62a0b-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="62a0b-186">Naciśnij klawisz **Ctrl + F5** do uruchomienia aplikacji bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="62a0b-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="62a0b-187">Otrzymasz komunikat o błędzie &quot; **nie konstruktora bez parametrów zdefiniowanych dla tego obiektu**&quot;:</span><span class="sxs-lookup"><span data-stu-id="62a0b-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="62a0b-188">![Błąd podczas uruchamiania rozpocząć aplikacji programu ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="62a0b-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="62a0b-189">*Błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="62a0b-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="62a0b-190">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-190">Close the browser.</span></span>

<span data-ttu-id="62a0b-191">W poniższych krokach będą działać w ramach rozwiązania magazynu utworów muzycznych iniekcję zależności, potrzebne w tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="62a0b-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="62a0b-192">Zadanie 2 — w tym Unity do rozwiązania MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="62a0b-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="62a0b-193">To zadanie będzie zawierać **Unity.Mvc3** pakietu NuGet w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="62a0b-194">Pakiet Unity.Mvc3 był przeznaczony dla platformy ASP.NET MVC 3, ale jest w pełni zgodny z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="62a0b-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="62a0b-195">Unity jest na przykład kontener iniekcji zależności niewielka, rozszerzalne z obsługą opcjonalne i wpisz zatrzymania.</span><span class="sxs-lookup"><span data-stu-id="62a0b-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="62a0b-196">Jest kontenerem ogólnego przeznaczenia do użycia w dowolnego typu aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="62a0b-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="62a0b-197">Zapewnia wspólne funkcje, w tym mechanizmów iniekcji zależności: Tworzenie obiektów, abstrakcji wymagania, określając zależności środowiska uruchomieniowego i elastyczność, przez odkładanie konfiguracji składnika do kontenera.</span><span class="sxs-lookup"><span data-stu-id="62a0b-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="62a0b-198">Zainstaluj **Unity.Mvc3** pakietu NuGet w **MvcMusicStore** projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="62a0b-199">Aby to zrobić, otwórz **Konsola Menedżera pakietów** z **widoku** | **inne okna**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="62a0b-200">Uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="62a0b-200">Run the following command.</span></span>

    <span data-ttu-id="62a0b-201">PMC</span><span class="sxs-lookup"><span data-stu-id="62a0b-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="62a0b-202">![Instalowanie pakietu NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalowanie pakietu NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="62a0b-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="62a0b-203">*Instalowanie pakietu NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="62a0b-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="62a0b-204">Raz **Unity.Mvc3** pakiet jest zainstalowany, Analizuj pliki i foldery, które automatycznie dodaje Aby uprościć konfigurację Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="62a0b-205">![Zainstalowanym pakietem Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "zainstalowanym pakietem Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="62a0b-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="62a0b-206">*Pakiet Unity.Mvc3 zainstalowany*</span><span class="sxs-lookup"><span data-stu-id="62a0b-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="62a0b-207">Zadanie 3 — rejestrowanie Unity w aplikacji Global.asax.cs\_Start</span><span class="sxs-lookup"><span data-stu-id="62a0b-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="62a0b-208">To zadanie zaktualizuje **aplikacji\_Start** metoda znajduje się w **Global.asax.cs** można wywołać inicjatora programu inicjującego Unity, a następnie, zaktualizuj rejestrowanie pliku inicjującego Usługi i kontrolera użyjesz iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="62a0b-209">Teraz zostanie Podłączanie programu inicjującego, czyli plik, który inicjuje kontenera Unity i rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="62a0b-210">Aby to zrobić, otwórz **Global.asax.cs** i Dodaj następujący wyróżniony kod w **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="62a0b-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="62a0b-211">(Fragment - kodu *laboratorium iniekcji zależności ASP.NET - Ex01 - zainicjować Unity*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="62a0b-212">Otwórz **Bootstrapper.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="62a0b-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="62a0b-213">Obejmują następujących przestrzeni nazw: **MvcMusicStore.Services** i **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="62a0b-214">(Fragment - kodu *Dodawanie przestrzeni nazw programu ASP.NET zależności iniekcji laboratorium - Ex01 - inicjującego*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="62a0b-215">Zastąp **BuildUnityContainer** metoda jego zawartość następującym kodem, który rejestruje kontrolera magazynu i usługi magazynu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="62a0b-216">(Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex01 - magazynu rejestru kontrolera i usługi*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="62a0b-217">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="62a0b-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="62a0b-218">W ramach tego zadania należy uruchomić aplikację, aby sprawdzić, czy teraz może być załadowany po dołączeniu Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="62a0b-219">Naciśnij klawisz **F5** do uruchomienia aplikacji, aplikacja powinna teraz załadować bez wyświetlania wszelkie komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="62a0b-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="62a0b-220">![Aplikacja uruchamiana za pomocą iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image6.png "aplikacja uruchamiana za pomocą iniekcji zależności")</span><span class="sxs-lookup"><span data-stu-id="62a0b-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="62a0b-221">*Aplikacja uruchamiana za pomocą iniekcji zależności*</span><span class="sxs-lookup"><span data-stu-id="62a0b-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="62a0b-222">Przejdź do **/magazynu**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-222">Browse to **/Store**.</span></span> <span data-ttu-id="62a0b-223">To spowoduje wywołanie **StoreController**, który został utworzony przy użyciu **Unity**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="62a0b-224">![Magazyn utworów muzycznych MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC utworów muzycznych magazynu")</span><span class="sxs-lookup"><span data-stu-id="62a0b-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="62a0b-225">*Magazyn utworów muzycznych MVC*</span><span class="sxs-lookup"><span data-stu-id="62a0b-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="62a0b-226">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-226">Close the browser.</span></span>

<span data-ttu-id="62a0b-227">W poniższych ćwiczeniach dowiesz się, jak rozszerzyć zakresu iniekcji zależności, aby użyć go w widoków MVC ASP.NET i filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="62a0b-228">Ćwiczenie 2: Wstrzyknięcie widoku</span><span class="sxs-lookup"><span data-stu-id="62a0b-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="62a0b-229">W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w widoku z nowych funkcji programu ASP.NET MVC 4 integracji Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="62a0b-230">Aby to zrobić, spowodują wywołanie usługi niestandardowej wewnątrz widok Przeglądaj magazynu, który będzie widoczny komunikat i obraz poniżej.</span><span class="sxs-lookup"><span data-stu-id="62a0b-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="62a0b-231">Następnie zostanie integracji projektu z Unity i utworzyć niestandardowy mechanizm rozpoznawania zależności wstrzyknięcia zależności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="62a0b-232">Zadanie 1 — Tworzenie widoku, który wykorzystuje usługę</span><span class="sxs-lookup"><span data-stu-id="62a0b-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="62a0b-233">W ramach tego zadania spowoduje utworzenie widoku, który wykonuje wywołanie usługi, aby wygenerować nową zależność.</span><span class="sxs-lookup"><span data-stu-id="62a0b-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="62a0b-234">Usługa składa się proste usługą obsługi wiadomości zawartych w tym rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="62a0b-235">Otwórz **rozpocząć** rozwiązania, znajdujących się w **wstrzyknięcie Source\Ex02 View\Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="62a0b-236">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="62a0b-237">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="62a0b-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="62a0b-238">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="62a0b-239">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="62a0b-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="62a0b-240">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="62a0b-241">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="62a0b-242">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="62a0b-243">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="62a0b-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="62a0b-244">Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="62a0b-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="62a0b-245">Obejmują **MessageService.cs** i **IMessageService.cs** klas znajdujących się w **źródła \Assets** folderu w **/usług**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="62a0b-246">Aby to zrobić, kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="62a0b-247">Przejdź do lokalizacji plików i włączyć je.</span><span class="sxs-lookup"><span data-stu-id="62a0b-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="62a0b-248">![Dodawanie usługi wiadomości i interfejs usługi](aspnet-mvc-4-dependency-injection/_static/image8.png "Dodawanie usługi wiadomości i interfejs usługi")</span><span class="sxs-lookup"><span data-stu-id="62a0b-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="62a0b-249">*Dodawanie usługi wiadomości i interfejs usługi*</span><span class="sxs-lookup"><span data-stu-id="62a0b-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="62a0b-250">**IMessageService** interfejs definiuje dwie właściwości implementowane przez **MessageService** klasy.</span><span class="sxs-lookup"><span data-stu-id="62a0b-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="62a0b-251">Te właściwości —**komunikat** i **ImageUrl**-przechowywać wiadomość i adres URL obrazu, który będzie wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="62a0b-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="62a0b-252">Utwórz folder **/strony** w projekcie głównym folderu, a następnie dodaj istniejącej klasy **MyBasePage.cs** z **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="62a0b-253">Strony podstawowej, który będzie dziedziczyć ma następującą strukturę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="62a0b-254">![Folder stron](aspnet-mvc-4-dependency-injection/_static/image9.png "folder stron")</span><span class="sxs-lookup"><span data-stu-id="62a0b-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="62a0b-255">Otwórz **Browse.cshtml** wyświetlić z **/widoków/Store** folderu i przydzielić mu dziedziczyć **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="62a0b-256">W **Przeglądaj** wyświetlić, dodaj wywołanie do **MessageService** do wyświetlania obrazu i wiadomości pobierane przez usługę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="62a0b-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="62a0b-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="62a0b-258">Zadanie 2 — w tym moduł rozpoznawania zależności niestandardowych i aktywatora strony widoku niestandardowego</span><span class="sxs-lookup"><span data-stu-id="62a0b-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="62a0b-259">W poprzednim zadaniu możesz wprowadzić nowe zależności w widoku do wykonywania w nim wywołanie usługi.</span><span class="sxs-lookup"><span data-stu-id="62a0b-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="62a0b-260">Teraz, rozwiąże ten zależności dzięki implementacji interfejsów iniekcji zależności MVC ASP.NET **IViewPageActivator** i **elementu IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="62a0b-261">W rozwiązaniu będzie zawierać implementację **elementu IDependencyResolver** który zajmuje się pobieranie usługi przy użyciu platformy Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="62a0b-262">Następnie będzie zawierać innej implementacji niestandardowych **IViewPageActivator** interfejs, który rozwiąże tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="62a0b-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="62a0b-263">Od platformy ASP.NET MVC 3 implementacja iniekcji zależności miał uproszczone interfejsy rejestrowania usług.</span><span class="sxs-lookup"><span data-stu-id="62a0b-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="62a0b-264">**Elementu IDependencyResolver** i **IViewPageActivator** są częścią funkcje platformy ASP.NET MVC 3 iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="62a0b-265">**-Elementu IDependencyResolver** poprzedniej IMvcServiceLocator zastępuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="62a0b-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="62a0b-266">Implementacje elementu IDependencyResolver musi zwrócić wystąpienie usługi lub kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="62a0b-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="62a0b-267">**-IViewPageActivator** interfejs zapewnia bardziej precyzyjną kontrolę nad jak utworzyć widok strony wystąpienia za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="62a0b-268">Klasy, które implementują **IViewPageActivator** interfejsu można utworzyć wystąpienia widoku przy użyciu informacji o kontekście.</span><span class="sxs-lookup"><span data-stu-id="62a0b-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="62a0b-269">Utwórz /**fabryki** folderu w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="62a0b-270">Obejmują **CustomViewPageActivator.cs** do rozwiązania z **/źródeł/zasoby/** do **fabryki** folderu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="62a0b-271">W tym celu kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz opcję **Dodaj | Istniejący element** , a następnie wybierz **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="62a0b-272">Ta klasa implementuje **IViewPageActivator** interfejs do przechowywania kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="62a0b-273">**CustomViewPageActivator** jest odpowiedzialny za zarządzanie tworzenia widoku przy użyciu kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="62a0b-274">Obejmują **UnityDependencyResolver.cs** plik z **/źródeł/zasoby** do **/Factories** folderu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="62a0b-275">W tym celu kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz opcję **Dodaj | Istniejący element** , a następnie wybierz **UnityDependencyResolver.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="62a0b-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="62a0b-276">**UnityDependencyResolver** klasy jest niestandardowej klasy DependencyResolver for Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="62a0b-277">Jeśli nie można znaleźć usługi kontenera platformy Unity, jest invocated podstawowego rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="62a0b-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="62a0b-278">W poniższych zadań zarówno implementacje zostanie zarejestrowany umożliwi poznanie lokalizacji usług i widoki modelu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="62a0b-279">Zadanie 3 — rejestrowanie iniekcji zależności w kontenerze Unity</span><span class="sxs-lookup"><span data-stu-id="62a0b-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="62a0b-280">To zadanie należy umieścić wszystkie poprzednie rzeczy, dzięki czemu będą działać iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="62a0b-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="62a0b-281">Do chwili rozwiązania zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="62a0b-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="62a0b-282">A **Przeglądaj** widoku, który dziedziczy z **MyBaseClass** i wykorzystuje **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="62a0b-283">Klasa pośrednicząca -**MyBaseClass**-mający iniekcji zależności zgłoszonych do interfejsu usługi.</span><span class="sxs-lookup"><span data-stu-id="62a0b-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="62a0b-284">Usługa - **MessageService** - i jego interfejs **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="62a0b-285">Moduł rozpoznawania zależności niestandardowych dla Unity - **UnityDependencyResolver** — która zajmuje się usługa pobierania.</span><span class="sxs-lookup"><span data-stu-id="62a0b-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="62a0b-286">Aktywatora strony widoku - **CustomViewPageActivator** -który tworzy stronę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="62a0b-287">Iniekcję **Przeglądaj** widoku, możesz teraz zarejestruje mechanizmu rozpoznawania zależności niestandardowych w kontenerze Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="62a0b-288">Otwórz **Bootstrapper.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="62a0b-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="62a0b-289">Zarejestrowanie wystąpienia **MessageService** w kontenerze Unity zainicjować usługi:</span><span class="sxs-lookup"><span data-stu-id="62a0b-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="62a0b-290">(Fragment - kodu *usługi wiadomości rejestru laboratorium - Ex02 - iniekcji zależności ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="62a0b-291">Dodaj odwołanie do **MvcMusicStore.Factories** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="62a0b-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="62a0b-292">(Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex02 - fabryki Namespace*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="62a0b-293">Zarejestruj **CustomViewPageActivator** jako aktywatora strony widoku w kontenerze Unity:</span><span class="sxs-lookup"><span data-stu-id="62a0b-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="62a0b-294">(Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex02 - rejestru CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="62a0b-295">Zamień na wystąpienie ASP.NET MVC 4 mechanizmem rozpoznawania zależności **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="62a0b-296">Aby to zrobić, Zamień **Initialise** metody zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="62a0b-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="62a0b-297">(Fragment - kodu *rozpoznawania zależności aktualizacji laboratorium - Ex02 - iniekcji zależności ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="62a0b-298">ASP.NET MVC udostępnia klasę mechanizmu rozpoznawania zależności domyślne.</span><span class="sxs-lookup"><span data-stu-id="62a0b-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="62a0b-299">Aby pracować z mechanizmów rozpoznawania zależności niestandardowego, który został utworzony dla unity, ten mechanizm rozpoznawania ma zostać zamieniona.</span><span class="sxs-lookup"><span data-stu-id="62a0b-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="62a0b-300">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="62a0b-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="62a0b-301">To zadanie uruchomi aplikację, aby sprawdzić, czy przeglądarka magazynu wykorzystuje usługę i przedstawia obrazu i pobrać komunikat:</span><span class="sxs-lookup"><span data-stu-id="62a0b-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="62a0b-302">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="62a0b-303">Kliknij przycisk **skale** w Genres Menu i zobacz, jak **MessageService** zostało dodane do widoku i załadować komunikat powitalny i obrazu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="62a0b-304">W tym przykładzie mamy wprowadzania do &quot; **skale**&quot;:</span><span class="sxs-lookup"><span data-stu-id="62a0b-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="62a0b-305">![Magazynu utworów muzycznych MVC — Wyświetl iniekcji](aspnet-mvc-4-dependency-injection/_static/image10.png "magazynu utworów muzycznych MVC — Wyświetl iniekcji")</span><span class="sxs-lookup"><span data-stu-id="62a0b-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="62a0b-306">*Magazynu utworów muzycznych MVC — Wyświetl iniekcji*</span><span class="sxs-lookup"><span data-stu-id="62a0b-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="62a0b-307">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="62a0b-308">Ćwiczenie 3: Wstrzyknięcie filtry akcji</span><span class="sxs-lookup"><span data-stu-id="62a0b-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="62a0b-309">W poprzednich laboratorium praktycznego **filtry akcji niestandardowych** mający doświadczenie z filtrów, dostosowywanie i iniekcji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="62a0b-310">W tym ćwiczeniu dowiesz się, jak wstrzyknięcie filtrów z iniekcji zależności za pomocą kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="62a0b-311">W tym celu zostaną dodane do rozwiązania magazynu utworów muzycznych filtr akcji niestandardowej, który będzie śledzić działania witryny.</span><span class="sxs-lookup"><span data-stu-id="62a0b-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="62a0b-312">Zadanie 1 — w tym filtru śledzenia w rozwiązaniu</span><span class="sxs-lookup"><span data-stu-id="62a0b-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="62a0b-313">W tym zadaniu będą zawarte w magazynie utworów muzycznych filtr akcji niestandardowych w celu śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="62a0b-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="62a0b-314">Jako filtr akcji niestandardowej pojęcia już są traktowane w poprzednim laboratorium &quot;filtry akcji niestandardowych&quot;, będą tylko zawiera klasy filtr z folderu zasoby tego laboratorium, a następnie utworzyć dostawcę filtru dla Unity:</span><span class="sxs-lookup"><span data-stu-id="62a0b-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="62a0b-315">Otwórz **rozpocząć** rozwiązania, znajdujących się w **Source\Ex03 - wstrzyknięcie Filter\Begin akcji** folderu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="62a0b-316">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="62a0b-317">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="62a0b-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="62a0b-318">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="62a0b-319">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="62a0b-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="62a0b-320">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="62a0b-321">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="62a0b-322">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="62a0b-323">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="62a0b-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="62a0b-324">Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="62a0b-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="62a0b-325">Obejmują **TraceActionFilter.cs** plik z **/źródeł/zasoby** do **/filtry** folderu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="62a0b-326">Ten filtr akcji niestandardowej wykonuje śledzenie na platformie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62a0b-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="62a0b-327">Możesz sprawdzić &quot;filtrów dynamicznych akcji i ASP.NET MVC 4 lokalne&quot; laboratorium więcej odwołania.</span><span class="sxs-lookup"><span data-stu-id="62a0b-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="62a0b-328">Dodaj pustą klasę **FilterProvider.cs** do projektu w folderze   **/filtry.**</span><span class="sxs-lookup"><span data-stu-id="62a0b-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="62a0b-329">Dodaj **System.Web.Mvc** i **Microsoft.Practices.Unity** przestrzeni nazw w **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="62a0b-330">(Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex03 - Filtr dostawcy Dodawanie obszarów nazw*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="62a0b-331">Zmień klasę dziedziczyć **IFilterProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="62a0b-332">Dodaj **IUnityContainer** właściwości w **FilterProvider** klasy, a następnie utwórz konstruktora klasy, aby przypisać kontenera.</span><span class="sxs-lookup"><span data-stu-id="62a0b-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="62a0b-333">(Fragment - kodu *Konstruktor dostawcy filtru laboratorium - Ex03 - iniekcji zależności ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="62a0b-334">Konstruktor klasy dostawcy filtru nie jest tworzone **nowe** obiektów wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="62a0b-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="62a0b-335">Kontener jest przekazywana jako parametr, a zależność jest obliczany przez Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="62a0b-336">W **FilterProvider** klasy, zaimplementuj metodę **GetFilters** z **IFilterProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="62a0b-337">(Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex03 - Filtr dostawcy GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="62a0b-338">Zadanie 2 — rejestrowania i włączanie filtru</span><span class="sxs-lookup"><span data-stu-id="62a0b-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="62a0b-339">To zadanie spowoduje włączenie śledzenia lokacji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="62a0b-340">Aby to zrobić, zarejestruj filtr w **Bootstrapper.cs BuildUnityContainer** metodę, aby rozpocząć śledzenie:</span><span class="sxs-lookup"><span data-stu-id="62a0b-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="62a0b-341">Otwórz **Web.config** znajdujący się w głównym projektu i Włącz śledzenie śledzenia w grupie System.Web.</span><span class="sxs-lookup"><span data-stu-id="62a0b-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="62a0b-342">Otwórz **Bootstrapper.cs** w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="62a0b-343">Dodaj odwołanie do **MvcMusicStore.Filters** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="62a0b-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="62a0b-344">(Fragment - kodu *Dodawanie przestrzeni nazw programu ASP.NET zależności iniekcji laboratorium - Ex03 - inicjującego*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="62a0b-345">Wybierz **BuildUnityContainer** — metoda i rejestrowanie filtru w kontenerze Unity.</span><span class="sxs-lookup"><span data-stu-id="62a0b-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="62a0b-346">Należy zarejestrować dostawcę filtru, a także filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="62a0b-347">(Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex03 - rejestru FilterProvider i ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="62a0b-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="62a0b-348">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="62a0b-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="62a0b-349">W tym zadaniu zostanie Uruchom aplikację i przetestować, czy filtr akcji niestandardowej jest śledzenie działania:</span><span class="sxs-lookup"><span data-stu-id="62a0b-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="62a0b-350">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="62a0b-351">Kliknij przycisk **skale** w Genres Menu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="62a0b-352">Jeśli chcesz, można przejść do genres więcej.</span><span class="sxs-lookup"><span data-stu-id="62a0b-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="62a0b-353">![Muzyka magazynu](aspnet-mvc-4-dependency-injection/_static/image11.png "magazynu utworów muzycznych")</span><span class="sxs-lookup"><span data-stu-id="62a0b-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="62a0b-354">*Muzyka magazynu*</span><span class="sxs-lookup"><span data-stu-id="62a0b-354">*Music Store*</span></span>
3. <span data-ttu-id="62a0b-355">Przejdź do **/Trace.axd** aby zobaczyć śledzenie aplikacji, a następnie kliknij przycisk **Wyświetl szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="62a0b-356">![Dziennik śledzenia aplikacji](aspnet-mvc-4-dependency-injection/_static/image12.png "dziennik śledzenia aplikacji")</span><span class="sxs-lookup"><span data-stu-id="62a0b-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="62a0b-357">*Dziennik śledzenia aplikacji*</span><span class="sxs-lookup"><span data-stu-id="62a0b-357">*Application Trace Log*</span></span>

    <span data-ttu-id="62a0b-358">![Aplikacja śledzenia — informacje dotyczące żądania](aspnet-mvc-4-dependency-injection/_static/image13.png "śledzenia aplikacji — szczegóły żądania")</span><span class="sxs-lookup"><span data-stu-id="62a0b-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="62a0b-359">*Aplikacja śledzenia — informacje dotyczące żądania*</span><span class="sxs-lookup"><span data-stu-id="62a0b-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="62a0b-360">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="62a0b-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="62a0b-361">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="62a0b-361">Summary</span></span>

<span data-ttu-id="62a0b-362">Wykonując tego laboratorium Hands-On ma przedstawiono sposób używania iniekcji zależności w programie ASP.NET MVC 4 integrując Unity przy użyciu pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="62a0b-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="62a0b-363">Aby to osiągnąć, użyto iniekcji zależności wewnątrz kontrolerów, widoków i filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="62a0b-364">Zostały uwzględnione następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="62a0b-364">The following concepts were covered:</span></span>

- <span data-ttu-id="62a0b-365">Funkcje iniekcji zależności programu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="62a0b-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="62a0b-366">Integracja platformy Unity za pośrednictwem pakietu NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="62a0b-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="62a0b-367">Iniekcji zależności w kontrolerów</span><span class="sxs-lookup"><span data-stu-id="62a0b-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="62a0b-368">Iniekcji zależności w widokach</span><span class="sxs-lookup"><span data-stu-id="62a0b-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="62a0b-369">Iniekcja zależności filtry akcji</span><span class="sxs-lookup"><span data-stu-id="62a0b-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="62a0b-370">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="62a0b-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="62a0b-371">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="62a0b-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="62a0b-372">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="62a0b-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="62a0b-373">Przejdź do [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="62a0b-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="62a0b-374">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="62a0b-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="62a0b-375">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-375">Click on **Install Now**.</span></span> <span data-ttu-id="62a0b-376">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="62a0b-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="62a0b-377">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="62a0b-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="62a0b-378">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="62a0b-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="62a0b-379">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="62a0b-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="62a0b-380">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="62a0b-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="62a0b-382">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="62a0b-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="62a0b-383">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-383">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="62a0b-385">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="62a0b-385">*Installation progress*</span></span>
6. <span data-ttu-id="62a0b-386">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-386">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="62a0b-388">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="62a0b-388">*Installation completed*</span></span>
7. <span data-ttu-id="62a0b-389">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="62a0b-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="62a0b-390">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="62a0b-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="62a0b-392">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="62a0b-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="62a0b-393">Dodatek B: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="62a0b-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="62a0b-394">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="62a0b-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="62a0b-395">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="62a0b-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="62a0b-396">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-dependency-injection/_static/image19.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="62a0b-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="62a0b-397">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="62a0b-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="62a0b-398">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="62a0b-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="62a0b-399">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="62a0b-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="62a0b-400">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="62a0b-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="62a0b-401">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="62a0b-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="62a0b-402">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="62a0b-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="62a0b-403">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="62a0b-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="62a0b-404">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-dependency-injection/_static/image20.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="62a0b-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="62a0b-405">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="62a0b-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="62a0b-406">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-dependency-injection/_static/image21.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="62a0b-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="62a0b-407">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="62a0b-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="62a0b-408">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-dependency-injection/_static/image22.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="62a0b-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="62a0b-409">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="62a0b-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="62a0b-410">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="62a0b-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="62a0b-411">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="62a0b-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="62a0b-412">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="62a0b-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="62a0b-413">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="62a0b-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="62a0b-414">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="62a0b-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="62a0b-415">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="62a0b-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="62a0b-416">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-dependency-injection/_static/image24.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="62a0b-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="62a0b-417">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="62a0b-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
