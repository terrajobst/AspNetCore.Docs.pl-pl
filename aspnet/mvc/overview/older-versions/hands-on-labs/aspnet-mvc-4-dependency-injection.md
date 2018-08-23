---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Wstrzykiwanie zależności platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: 'Uwaga: W tym laboratorium praktyczne przyjęto założenie, że masz podstawową wiedzę na temat filtrów platformy ASP.NET MVC i platformy ASP.NET MVC 4. Jeśli nie używasz filtrów platformy ASP.NET MVC 4 przed możemy "rec"...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755773"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="5b80a-104">Wstrzykiwanie zależności platformy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="5b80a-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="5b80a-105">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5b80a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5b80a-106">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="5b80a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="5b80a-107">W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC** i **filtrów platformy ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="5b80a-108">Jeśli nie używasz **filtrów platformy ASP.NET MVC 4** , zalecamy zapoznać się z **programu ASP.NET MVC niestandardowe filtry akcji** warsztatów.</span><span class="sxs-lookup"><span data-stu-id="5b80a-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="5b80a-109">Wszystkie przykładowy kod i fragmenty są uwzględnione w sieci Web Camp zestaw szkoleniowy, pod [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="5b80a-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="5b80a-110">Projekt specyficzne dla tego laboratorium znajduje się w temacie [wstrzykiwanie zależności programu ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="5b80a-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="5b80a-111">W **programowanie zorientowane na obiekt** paradigm, obiekty współpracują ze sobą w model współpracy w przypadku, gdy istnieją współautorów i konsumentów.</span><span class="sxs-lookup"><span data-stu-id="5b80a-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="5b80a-112">Oczywiście ten model komunikacji generuje zależności między obiektami i składników, staje się trudne do zarządzania, gdy zwiększa złożonością.</span><span class="sxs-lookup"><span data-stu-id="5b80a-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="5b80a-113">![Klasa zależności i modelowanie złożoności](aspnet-mvc-4-dependency-injection/_static/image1.png "klasy zależności i modelowanie złożoności")</span><span class="sxs-lookup"><span data-stu-id="5b80a-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="5b80a-114">*Klasa zależności i złożoność modelu*</span><span class="sxs-lookup"><span data-stu-id="5b80a-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="5b80a-115">Prawdopodobnie wiesz o **wzorzec fabryki** i oddzielenie interfejs i wdrożenia przy użyciu usług, których obiekty klienta często są odpowiedzialne za lokalizacji usługi.</span><span class="sxs-lookup"><span data-stu-id="5b80a-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="5b80a-116">Wzorzec wstrzykiwanie zależności jest określonej implementacji Inwersja kontroli.</span><span class="sxs-lookup"><span data-stu-id="5b80a-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="5b80a-117">**Inwersja kontroli (IoC)** oznacza, że obiektów nie należy tworzyć inne obiekty, które korzystają z do wykonania swojej pracy.</span><span class="sxs-lookup"><span data-stu-id="5b80a-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="5b80a-118">Zamiast tego otrzymują obiektów, które są im niezbędne ze źródła zewnętrznego (na przykład plik konfiguracyjny xml).</span><span class="sxs-lookup"><span data-stu-id="5b80a-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="5b80a-119">**Wstrzykiwanie zależności (DI)** oznacza, że jest zazwyczaj wykonywane bez interwencji obiektu przez składnik framework, który przekazuje parametry konstruktora i ustaw właściwości.</span><span class="sxs-lookup"><span data-stu-id="5b80a-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="5b80a-120">Wzorzec projektowy (DI) wstrzykiwania zależności</span><span class="sxs-lookup"><span data-stu-id="5b80a-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="5b80a-121">Na wysokim poziomie celem wstrzykiwanie zależności jest to, że klasa klienta (np. *golfer*) wymaga coś, co spełnia interfejsu (np. *IClub*).</span><span class="sxs-lookup"><span data-stu-id="5b80a-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="5b80a-122">Zależy to konkretny typ (np. *WedgeClub WoodClub, IronClub,* lub *PutterClub*), ktoś inny do obsługi, który chce (np. jest dobrą *caddy*).</span><span class="sxs-lookup"><span data-stu-id="5b80a-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="5b80a-123">Mechanizm rozpoznawania zależności na platformie ASP.NET MVC umożliwia rejestrowanie logikę zależności gdzie indziej (np. kontenerze lub *zbioru kluby*).</span><span class="sxs-lookup"><span data-stu-id="5b80a-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="5b80a-124">![Diagram iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustracji wstrzykiwanie zależności")</span><span class="sxs-lookup"><span data-stu-id="5b80a-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="5b80a-125">*Wstrzykiwanie zależności - odpowiednio Golf*</span><span class="sxs-lookup"><span data-stu-id="5b80a-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="5b80a-126">Zalety korzystania z wzorca wstrzykiwanie zależności oraz Inwersja kontroli są następujące:</span><span class="sxs-lookup"><span data-stu-id="5b80a-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="5b80a-127">Zmniejsza sprzężenia klas</span><span class="sxs-lookup"><span data-stu-id="5b80a-127">Reduces class coupling</span></span>
- <span data-ttu-id="5b80a-128">Zwiększa, ponowne użycie kodu</span><span class="sxs-lookup"><span data-stu-id="5b80a-128">Increases code reusing</span></span>
- <span data-ttu-id="5b80a-129">Zwiększa łatwości utrzymania kodu</span><span class="sxs-lookup"><span data-stu-id="5b80a-129">Improves code maintainability</span></span>
- <span data-ttu-id="5b80a-130">Zwiększa testowania aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b80a-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="5b80a-131">Wstrzykiwanie zależności czasem jest porównywana z abstrakcyjne wzorzec projektowy fabryki, ale istnieje niewielka różnica między oba podejścia.</span><span class="sxs-lookup"><span data-stu-id="5b80a-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="5b80a-132">DI ma strukturę pracę za modułem rozwiązać zależności w wywołaniu fabryk i zarejestrowane usługi.</span><span class="sxs-lookup"><span data-stu-id="5b80a-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="5b80a-133">Teraz, gdy już rozumiesz wzorzec iniekcji zależności, nauczysz w całym tym środowisku laboratoryjnym ją zastosować w technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5b80a-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="5b80a-134">Zostanie uruchomiona, przy użyciu iniekcji zależności w **kontrolerów** obejmujący usługi dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5b80a-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="5b80a-135">Następnie zostaną zastosowane wstrzykiwanie zależności do **widoków** do korzystania z usługi i wyświetlania informacji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="5b80a-136">Na koniec rozszerzenie DI do platformy ASP.NET MVC 4 filtrów, wprowadzanie filtru akcji niestandardowej w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="5b80a-137">W tym laboratorium praktyczne dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="5b80a-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="5b80a-138">Integracja platformy ASP.NET MVC 4 przy użyciu aparatu Unity do wstrzykiwania zależności za pomocą pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="5b80a-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="5b80a-139">Iniekcji zależności wewnątrz kontroler składnika ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5b80a-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="5b80a-140">Iniekcji zależności w widoku programu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5b80a-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="5b80a-141">Iniekcji zależności wewnątrz filtru akcji programu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5b80a-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="5b80a-142">W tym laboratorium to przy użyciu pakietu NuGet Unity.Mvc3 do rozpoznawania zależności, ale istnieje możliwość dostosowania dowolnej architektury wstrzykiwanie zależności do pracy z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5b80a-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5b80a-143">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5b80a-143">Prerequisites</span></span>

<span data-ttu-id="5b80a-144">Należy dysponować następującymi elementami do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="5b80a-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5b80a-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="5b80a-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5b80a-146">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="5b80a-146">Setup</span></span>

<span data-ttu-id="5b80a-147">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="5b80a-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="5b80a-148">Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b80a-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5b80a-149">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="5b80a-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5b80a-150">Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek B: za pomocą wstawek kodu programu](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b80a-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5b80a-151">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="5b80a-151">Exercises</span></span>

<span data-ttu-id="5b80a-152">W tym laboratorium praktyczne składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="5b80a-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="5b80a-153">Ćwiczenie 1: Wstawianie kontrolera</span><span class="sxs-lookup"><span data-stu-id="5b80a-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="5b80a-154">Ćwiczenie 2: Wstawianie widoku</span><span class="sxs-lookup"><span data-stu-id="5b80a-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="5b80a-155">Ćwiczenie 3: Wstawianie filtry</span><span class="sxs-lookup"><span data-stu-id="5b80a-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="5b80a-156">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="5b80a-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="5b80a-157">Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="5b80a-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="5b80a-158">Szacowany czas do ukończenia tego laboratorium: **30 minut**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="5b80a-159">Ćwiczenie 1: Wstawianie kontrolera</span><span class="sxs-lookup"><span data-stu-id="5b80a-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="5b80a-160">W tym ćwiczeniu dowiesz się, jak używać wstrzykiwanie zależności w kontrolery ASP.NET MVC, integrując Unity przy użyciu pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="5b80a-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="5b80a-161">Z tego powodu będzie zawierać usług na kontrolerach MvcMusicStore do oddzielania logika dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="5b80a-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="5b80a-162">Usługi spowoduje utworzenie nowych zależności w Konstruktorze kontrolera, który zostanie rozwiązany przy użyciu iniekcji zależności za pomocą **Unity**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="5b80a-163">To podejście pokazano sposób generowania mniej sprzężone aplikacje, które są bardziej elastyczne i ułatwia utrzymywanie i testowanie.</span><span class="sxs-lookup"><span data-stu-id="5b80a-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="5b80a-164">Ponadto dowiesz sposobu integrowania programu ASP.NET MVC przy użyciu aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="5b80a-165">O usłudze StoreManager</span><span class="sxs-lookup"><span data-stu-id="5b80a-165">About StoreManager Service</span></span>

<span data-ttu-id="5b80a-166">MVC Music Store, podany w rozwiązaniu Rozpocznij teraz obejmuje usługi, która zarządza danymi Store kontroler o nazwie **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="5b80a-167">Poniżej można znaleźć implementacji usługi Store.</span><span class="sxs-lookup"><span data-stu-id="5b80a-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="5b80a-168">Należy pamiętać, że wszystkie metody zwracają modelowania jednostek.</span><span class="sxs-lookup"><span data-stu-id="5b80a-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="5b80a-169">**StoreController** BEGIN teraz wykorzystuje rozwiązanie **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="5b80a-170">Wszystkie odwołania zostały usunięte z **StoreController**, a teraz możliwość modyfikowania bieżącego dostawcy dostępu do danych bez wprowadzania zmian w dowolnej metody, która zużywa **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="5b80a-171">Można znaleźć poniżej **StoreController** implementacja ma zależność **StoreService** wewnątrz konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="5b80a-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="5b80a-172">Zależność wprowadzone w tym ćwiczeniu jest powiązany z **Inwersja kontroli** (IoC).</span><span class="sxs-lookup"><span data-stu-id="5b80a-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="5b80a-173">**StoreController** odbiera konstruktora klasy **IStoreService** parametr typu, który jest niezbędne do wykonania wywołania usługi wewnątrz klasy.</span><span class="sxs-lookup"><span data-stu-id="5b80a-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="5b80a-174">Jednak **StoreController** nie zawiera implementacji konstruktora domyślnego (z bez parametrów), że każdy kontroler musi mieć do pracy z platformą ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b80a-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="5b80a-175">Można rozpoznać zależności, kontroler ma zostać utworzona przez fabrykę abstrakcyjny (klasę, która zwraca dowolnego obiektu określonego typu).</span><span class="sxs-lookup"><span data-stu-id="5b80a-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="5b80a-176">Gdy klasa próbuje utworzyć StoreController bez wysyłania obiektu usługi, ponieważ nie ma żadnych konstruktora bez parametrów, które zostały zgłoszone, otrzymają komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="5b80a-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="5b80a-177">Zadanie 1. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b80a-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="5b80a-178">W tym zadaniu uruchomisz aplikację Begin, która obejmuje usługę do kontrolera Store, która oddziela dostęp do danych od logiki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="5b80a-179">Po uruchomieniu aplikacji, zostanie wyświetlony wyjątek, ponieważ usługa kontrolera nie jest przekazywany jako parametr domyślny:</span><span class="sxs-lookup"><span data-stu-id="5b80a-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="5b80a-180">Otwórz **rozpocząć** rozwiązanie znajduje się w **wprowadza Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="5b80a-181">Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="5b80a-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5b80a-182">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="5b80a-183">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="5b80a-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="5b80a-184">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5b80a-185">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5b80a-186">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5b80a-187">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="5b80a-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5b80a-188">Naciśnij klawisz **klawiszy Ctrl + F5** Aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="5b80a-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="5b80a-189">Otrzymasz komunikat o błędzie &quot; **nie konstruktora bez parametrów, które zostały zdefiniowane dla tego obiektu**&quot;:</span><span class="sxs-lookup"><span data-stu-id="5b80a-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="5b80a-190">![Wystąpił błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET](aspnet-mvc-4-dependency-injection/_static/image3.png "wystąpił błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="5b80a-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="5b80a-191">*Wystąpił błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="5b80a-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="5b80a-192">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-192">Close the browser.</span></span>

<span data-ttu-id="5b80a-193">W poniższych krokach będzie działać na rozwiązanie Store utworów muzycznych na iniekcji zależności, czego potrzebuje tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5b80a-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="5b80a-194">Zadanie 2 — w tym aparatu Unity w rozwiązaniu MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="5b80a-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="5b80a-195">W ramach tego zadania będzie zawierać **Unity.Mvc3** pakietu NuGet z rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="5b80a-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="5b80a-196">Pakiet Unity.Mvc3 był przeznaczony dla platformy ASP.NET MVC 3, ale jest w pełni zgodny z platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5b80a-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="5b80a-197">Unity to kontener iniekcji zależności niewielka, rozszerzalna opcjonalna pomoc techniczna dla wystąpienia i wpisz przejmowanie.</span><span class="sxs-lookup"><span data-stu-id="5b80a-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="5b80a-198">Jest kontener ogólnego zastosowania do użycia w aplikacji .NET dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="5b80a-199">Umożliwia wspólne funkcje w mechanizmy iniekcji zależności, w tym: Tworzenie obiektów, abstrakcji wymagania za pośrednictwem zależności środowiska uruchomieniowego i elastyczność, poprzez opóźnienie Konfiguracja składnika, do kontenera.</span><span class="sxs-lookup"><span data-stu-id="5b80a-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="5b80a-200">Zainstaluj **Unity.Mvc3** pakietu NuGet w **MvcMusicStore** projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="5b80a-201">Aby to zrobić, otwórz **Konsola Menedżera pakietów** z **widoku** | **inne Windows**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="5b80a-202">Uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="5b80a-202">Run the following command.</span></span>

    <span data-ttu-id="5b80a-203">PMC</span><span class="sxs-lookup"><span data-stu-id="5b80a-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="5b80a-204">![Instalowanie pakietu NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalowanie pakietu NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="5b80a-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="5b80a-205">*Instalowanie pakietu NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="5b80a-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="5b80a-206">Gdy **Unity.Mvc3** pakiet jest zainstalowany, Eksploruj pliki i foldery, które automatycznie dodaje w celu uproszczenia konfiguracji aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="5b80a-207">![Zainstalowany pakiet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "zainstalowany pakiet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="5b80a-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="5b80a-208">*Zainstalowany pakiet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="5b80a-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="5b80a-209">Zadanie 3. rejestrowanie Unity w aplikacji Global.asax.cs\_Start</span><span class="sxs-lookup"><span data-stu-id="5b80a-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="5b80a-210">To zadanie zaktualizuje **aplikacji\_Start** metoda znajduje się w **Global.asax.cs** wywołać inicjatora programu inicjującego Unity, a następnie, zaktualizować, rejestrowanie pliku inicjującego Usługi i kontroler użyje do wstrzykiwania zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="5b80a-211">Teraz będzie podpiąć program inicjujący, czyli pliku który inicjuje kontenera aparatu Unity i mechanizmu rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="5b80a-212">Aby to zrobić, otwórz **Global.asax.cs** i Dodaj następujący wyróżniony kod w ramach **aplikacji\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="5b80a-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="5b80a-213">(Code Snippet — *laboratorium iniekcji zależności platformy ASP.NET — Ex01 — inicjowanie aparatu Unity*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="5b80a-214">Otwórz **Bootstrapper.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="5b80a-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="5b80a-215">Uwzględnić następujące przestrzenie nazw: **MvcMusicStore.Services** i **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="5b80a-216">(Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex01 — program inicjujący Instalatora programu Dodawanie przestrzenie nazw*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="5b80a-217">Zastąp **BuildUnityContainer** metoda jego zawartości za pomocą następujący kod, który rejestruje Store kontrolera i Store Service.</span><span class="sxs-lookup"><span data-stu-id="5b80a-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="5b80a-218">(Code Snippet — *ASP.NET zależności iniekcji laboratorium - Ex01 - Store Zarejestruj kontroler i usługa*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="5b80a-219">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b80a-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="5b80a-220">To zadanie uruchomi aplikację, aby sprawdzić, czy teraz może być załadowany po tym aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="5b80a-221">Naciśnij klawisz **F5** do uruchamiania aplikacji, aplikacja powinna teraz załadować bez pokazywania wszelkie komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="5b80a-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="5b80a-222">![Uruchomienie aplikacji przy użyciu iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image6.png "uruchamiania aplikacji przy użyciu iniekcji zależności")</span><span class="sxs-lookup"><span data-stu-id="5b80a-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="5b80a-223">*Uruchomienie aplikacji przy użyciu iniekcji zależności*</span><span class="sxs-lookup"><span data-stu-id="5b80a-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="5b80a-224">Przejdź do **/Store**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-224">Browse to **/Store**.</span></span> <span data-ttu-id="5b80a-225">Spowoduje to wywołanie **StoreController**, który został utworzony przy użyciu **Unity**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="5b80a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="5b80a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="5b80a-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="5b80a-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="5b80a-228">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-228">Close the browser.</span></span>

<span data-ttu-id="5b80a-229">W poniższym ćwiczeniach dowiesz się, jak rozszerzyć zakres wstrzykiwanie zależności z niej korzystać w widoków MVC platformy ASP.NET i filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="5b80a-230">Ćwiczenie 2: Wstawianie widoku</span><span class="sxs-lookup"><span data-stu-id="5b80a-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="5b80a-231">W tym ćwiczeniu dowiesz się, jak używać wstrzykiwanie zależności w widoku z nowych funkcji programu ASP.NET MVC 4 do integracji środowiska Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="5b80a-232">Aby to zrobić, będzie wywoływać to usługa niestandardowa wewnątrz Store Przeglądaj widoku, który wyświetli komunikat i obraz poniżej.</span><span class="sxs-lookup"><span data-stu-id="5b80a-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="5b80a-233">Następnie zostanie zintegrować przy użyciu aparatu Unity i utworzyć niestandardowy mechanizm rozpoznawania zależności iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="5b80a-234">Zadanie 1 — Tworzenie widoku, który korzysta z usługi</span><span class="sxs-lookup"><span data-stu-id="5b80a-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="5b80a-235">W tym zadaniu utworzysz widok, który wykonuje wywołanie usługi, aby wygenerować nowy zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="5b80a-236">Usługi polega na prostą usługę komunikatów zawartych w tym rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="5b80a-237">Otwórz **rozpocząć** rozwiązanie znajduje się w **wprowadza Source\Ex02 View\Begin** folderu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="5b80a-238">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="5b80a-239">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="5b80a-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5b80a-240">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="5b80a-241">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="5b80a-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="5b80a-242">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5b80a-243">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5b80a-244">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5b80a-245">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="5b80a-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="5b80a-246">Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="5b80a-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="5b80a-247">Obejmują **MessageService.cs** i **IMessageService.cs** klas znajdujących się w **źródła \Assets** folderu w **/usług**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="5b80a-248">Aby to zrobić, kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="5b80a-249">Przejdź do lokalizacji plików i je uwzględnić.</span><span class="sxs-lookup"><span data-stu-id="5b80a-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="5b80a-250">![Dodawanie wiadomości SMS i interfejsu usługi](aspnet-mvc-4-dependency-injection/_static/image8.png "dodawanie wiadomości SMS i interfejsu usługi")</span><span class="sxs-lookup"><span data-stu-id="5b80a-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="5b80a-251">*Dodawanie usługi wiadomości i interfejsu usługi*</span><span class="sxs-lookup"><span data-stu-id="5b80a-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b80a-252">**IMessageService** interfejs definiuje dwie właściwości implementowane przez **MessageService** klasy.</span><span class="sxs-lookup"><span data-stu-id="5b80a-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="5b80a-253">Te właściwości -**komunikat** i **ImageUrl**— przechowywanie komunikat i adres URL obrazu, który będzie wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="5b80a-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="5b80a-254">Utwórz folder **/strony** w projekcie folder główny, a następnie Dodaj istniejącą klasę **MyBasePage.cs** z **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="5b80a-255">Strony podstawowej, który będzie dziedziczyć ma następującą strukturę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="5b80a-256">![Folder stron](aspnet-mvc-4-dependency-injection/_static/image9.png "folder stron")</span><span class="sxs-lookup"><span data-stu-id="5b80a-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="5b80a-257">Otwórz **Browse.cshtml** z widoku **/widoków/Store** folderu i przypisz ją dziedziczyć **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="5b80a-258">W **Przeglądaj** wyświetlić, dodać wywołanie **MessageService** do wyświetlania obrazu i komunikat, który został pobrany przez usługę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="5b80a-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="5b80a-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="5b80a-260">Zadanie 2 — w tym mechanizm rozpoznawania zależności niestandardowej i aktywatora strony widoku niestandardowego</span><span class="sxs-lookup"><span data-stu-id="5b80a-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="5b80a-261">W poprzednim zadaniu wprowadzony się nowych zależności wewnątrz widoku, aby wykonać wywołanie usługi wewnątrz niego.</span><span class="sxs-lookup"><span data-stu-id="5b80a-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="5b80a-262">Teraz, zostanie rozwiązany tej zależności poprzez implementację interfejsów wstrzykiwanie zależności MVC ASP.NET **IViewPageActivator** i **elementu IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="5b80a-263">W rozwiązaniu będzie zawierać implementację **elementu IDependencyResolver** który poradzi sobie z pobierania usługi za pomocą aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="5b80a-264">Następnie będzie zawierać inny niestandardową implementację **IViewPageActivator** interfejs, który rozwiąże tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="5b80a-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="5b80a-265">Od platformy ASP.NET MVC 3 wykonania na iniekcji zależności miał uproszczone interfejsów, które można zarejestrować usługi.</span><span class="sxs-lookup"><span data-stu-id="5b80a-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="5b80a-266">**Elementu IDependencyResolver** i **IViewPageActivator** wchodzą w skład funkcji ASP.NET MVC 3 do wstrzykiwania zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="5b80a-267">**-Elementu IDependencyResolver** interfejsu zastępuje IMvcServiceLocator poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="5b80a-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="5b80a-268">Implementacje elementu IDependencyResolver musi zwrócić wystąpienie usługi lub kolekcję usługi.</span><span class="sxs-lookup"><span data-stu-id="5b80a-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="5b80a-269">**-IViewPageActivator** interfejs zapewnia bardziej precyzyjną kontrolę nad jak widoku strony są tworzone za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="5b80a-270">Klasy, które implementują **IViewPageActivator** interfejsu można utworzyć wystąpienia widoku przy użyciu informacji o kontekście.</span><span class="sxs-lookup"><span data-stu-id="5b80a-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="5b80a-271">Utwórz /**fabryk** folderu w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="5b80a-272">Obejmują **CustomViewPageActivator.cs** do rozwiązania z **/źródła/zasobów/** do **fabryk** folderu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="5b80a-273">Aby to zrobić, kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz **Dodaj | Istniejący element** , a następnie wybierz **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="5b80a-274">Ta klasa implementuje **IViewPageActivator** interfejs do przechowywania kontenera aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b80a-275">**CustomViewPageActivator** jest odpowiedzialny za zarządzanie tworzenia widoku przy użyciu kontenerów aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="5b80a-276">Obejmują **UnityDependencyResolver.cs** plik wchodzącej w skład **/źródła/zasoby** do **/Factories** folderu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="5b80a-277">Aby to zrobić, kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz **Dodaj | Istniejący element** , a następnie wybierz **UnityDependencyResolver.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="5b80a-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b80a-278">**UnityDependencyResolver** klasa jest niestandardowej klasy DependencyResolver dla aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="5b80a-279">Jeśli usługa nie zostanie znaleziony w kontenerze aparatu Unity, jest invocated podstawowego rozpoznawania nazw.</span><span class="sxs-lookup"><span data-stu-id="5b80a-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="5b80a-280">Następujące zadanie zostanie zarejestrowana obu implementacjach umożliwiające modelu znać lokalizację usługi i widoki.</span><span class="sxs-lookup"><span data-stu-id="5b80a-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="5b80a-281">Zadanie 3 — rejestrowanie na potrzeby wstrzykiwanie zależności w kontenerze aparatu Unity</span><span class="sxs-lookup"><span data-stu-id="5b80a-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="5b80a-282">W tym zadaniu należy umieścić wszystkie poprzednie elementy razem w celu zapewnienia wstrzykiwanie zależności w pracy.</span><span class="sxs-lookup"><span data-stu-id="5b80a-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="5b80a-283">Do chwili obecnej rozwiązanie ma następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="5b80a-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="5b80a-284">A **Przeglądaj** widoku, który dziedziczy z **MyBaseClass** i wykorzystuje **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="5b80a-285">Klasa pośrednicząca -**MyBaseClass**-zawierający wstrzykiwanie zależności zadeklarowany dla interfejsu usługi.</span><span class="sxs-lookup"><span data-stu-id="5b80a-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="5b80a-286">Usługa - **MessageService** — i jego interfejs **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="5b80a-287">Mechanizm rozpoznawania zależności niestandardowej dla aparatu Unity — **UnityDependencyResolver** — która zajmuje się pobieranie usługi.</span><span class="sxs-lookup"><span data-stu-id="5b80a-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="5b80a-288">Aktywatora strony widoku - **CustomViewPageActivator** -tworząca stronę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="5b80a-289">Aby wstawić **Przeglądaj** widoku teraz zarejestrujesz mechanizmu rozpoznawania zależności niestandardowej w kontenerze aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="5b80a-290">Otwórz **Bootstrapper.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="5b80a-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="5b80a-291">Rejestrowanie wystąpienia **MessageService** w kontenerze aparatu Unity zainicjować usługi:</span><span class="sxs-lookup"><span data-stu-id="5b80a-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="5b80a-292">(Code Snippet — *usługi wiadomości rejestru laboratorium — Ex02 — iniekcji zależności ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="5b80a-293">Dodaj odwołanie do **MvcMusicStore.Factories** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="5b80a-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="5b80a-294">(Code Snippet — *ASP.NET zależności iniekcji laboratorium - Ex02 - fabryk Namespace*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="5b80a-295">Zarejestruj **CustomViewPageActivator** jako aktywatora strony widoku w kontenerze aparatu Unity:</span><span class="sxs-lookup"><span data-stu-id="5b80a-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="5b80a-296">(Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex02 — Zarejestruj CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="5b80a-297">Zamień na wystąpienie platformy ASP.NET MVC 4 mechanizmem rozpoznawania zależności **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="5b80a-298">Aby to zrobić, Zastąp **Initialise** metoda zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5b80a-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="5b80a-299">(Code Snippet — *rozpoznawania zależności aktualizacji laboratorium — Ex02 — wstrzykiwanie zależności ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b80a-300">ASP.NET MVC udostępnia domyślną klasę mechanizmu rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="5b80a-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="5b80a-301">Aby pracować z mechanizmów rozpoznawania zależności niestandardowego, który został utworzony dla aparatu unity, ten mechanizm rozpoznawania ma zostać zastąpiony.</span><span class="sxs-lookup"><span data-stu-id="5b80a-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="5b80a-302">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b80a-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="5b80a-303">To zadanie uruchomi aplikację, aby sprawdzić, czy przeglądarka Store wykorzystuje usługę i pokazuje obraz i komunikat pobrać:</span><span class="sxs-lookup"><span data-stu-id="5b80a-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="5b80a-304">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5b80a-305">Kliknij przycisk **Rock** gatunki Menu i zobacz jak **MessageService** został wprowadzony w widoku, a następnie ładowane wiadomość powitalna i obraz.</span><span class="sxs-lookup"><span data-stu-id="5b80a-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="5b80a-306">W tym przykładzie mamy wprowadzenie do &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="5b80a-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="5b80a-307">![MVC Music Store — widok iniekcji](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store — iniekcji widoku")</span><span class="sxs-lookup"><span data-stu-id="5b80a-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="5b80a-308">*MVC Music Store — iniekcji widoku*</span><span class="sxs-lookup"><span data-stu-id="5b80a-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="5b80a-309">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="5b80a-310">Ćwiczenie 3: Wstawianie filtry akcji</span><span class="sxs-lookup"><span data-stu-id="5b80a-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="5b80a-311">W poprzednim laboratorium praktycznego **niestandardowe filtry akcji** mający doświadczenie z filtrów dostosowywania i iniekcji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="5b80a-312">W tym ćwiczeniu dowiesz się, jak wstawić filtry przy użyciu iniekcji zależności za pomocą kontenera aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="5b80a-313">Aby to zrobić, dodasz do rozwiązania Music Store filtr akcji niestandardowej, który będzie śledzić działania lokacji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="5b80a-314">Zadanie 1 — w tym filtru śledzenia w rozwiązaniu</span><span class="sxs-lookup"><span data-stu-id="5b80a-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="5b80a-315">W tym zadaniu w Store utworów muzycznych będzie zawierać filtr akcję niestandardową do śledzenia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="5b80a-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="5b80a-316">Jako filtr akcji niestandardowej pojęcia są już traktowane w poprzednim środowisku laboratoryjnym &quot;niestandardowe filtry akcji&quot;, będą po prostu Dołącz klasy filtr z folderu zasobów w tym laboratorium, a następnie utworzyć dostawcę filtru dla aparatu Unity:</span><span class="sxs-lookup"><span data-stu-id="5b80a-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="5b80a-317">Otwórz **rozpocząć** rozwiązanie znajduje się w **Source\Ex03 — wprowadzanie Filter\Begin akcji** folderu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="5b80a-318">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="5b80a-319">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="5b80a-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5b80a-320">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="5b80a-321">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="5b80a-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="5b80a-322">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5b80a-323">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5b80a-324">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5b80a-325">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="5b80a-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="5b80a-326">Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="5b80a-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="5b80a-327">Obejmują **TraceActionFilter.cs** plik wchodzącej w skład **/źródła/zasoby** do **/filtry** folderu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b80a-328">Ten filtr akcji niestandardowej wykonuje śledzenie na platformie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b80a-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="5b80a-329">Możesz sprawdzić &quot;lokalnej platformy ASP.NET MVC 4 i filtrów dynamicznych akcji&quot; laboratorium na potrzeby więcej odwołań.</span><span class="sxs-lookup"><span data-stu-id="5b80a-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="5b80a-330">Dodać pustą klasę **FilterProvider.cs** do projektu w folderze   **/filtrów.**</span><span class="sxs-lookup"><span data-stu-id="5b80a-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="5b80a-331">Dodaj **System.Web.Mvc** i **Microsoft.Practices.Unity** przestrzeniach nazw **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="5b80a-332">(Code Snippet — *dostawcy filtru rozszerzeń ASP.NET zależności iniekcji laboratorium — Ex03 — Dodawanie przestrzenie nazw*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="5b80a-333">Wprowadź dziedziczyć z klasy **IFilterProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="5b80a-334">Dodaj **IUnityContainer** właściwość **FilterProvider** klasy, a następnie utwórz konstruktora klasy, aby przypisać kontenera.</span><span class="sxs-lookup"><span data-stu-id="5b80a-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="5b80a-335">(Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — Filtr dostawcy Konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b80a-336">Konstruktora klasy dostawcy filtru są nietworzenie **nowe** wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="5b80a-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="5b80a-337">Kontener jest przekazywany jako parametr i zależność jest obliczany przez aparat Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="5b80a-338">W **FilterProvider** klasy, implementować metody **GetFilters** z **IFilterProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="5b80a-339">(Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — Filtr dostawcy GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="5b80a-340">Zadanie 2 — rejestrowania i włączanie filtru</span><span class="sxs-lookup"><span data-stu-id="5b80a-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="5b80a-341">W ramach tego zadania spowoduje włączenie śledzenia witryny.</span><span class="sxs-lookup"><span data-stu-id="5b80a-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="5b80a-342">Aby to zrobić, zarejestruje filtru **Bootstrapper.cs BuildUnityContainer** metodę, aby rozpocząć śledzenie:</span><span class="sxs-lookup"><span data-stu-id="5b80a-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="5b80a-343">Otwórz **Web.config** znajduje się w katalogu głównym projektu i włączanie śledzenia śledzenia w grupie System.Web.</span><span class="sxs-lookup"><span data-stu-id="5b80a-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="5b80a-344">Otwórz **Bootstrapper.cs** w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="5b80a-345">Dodaj odwołanie do **MvcMusicStore.Filters** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="5b80a-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="5b80a-346">(Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — program inicjujący Instalatora programu Dodawanie przestrzenie nazw*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="5b80a-347">Wybierz **BuildUnityContainer** metody i rejestrowanie filtru w kontenerze aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="5b80a-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="5b80a-348">Należy zarejestrować dostawcę filtru, a także filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="5b80a-349">(Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — Zarejestruj FilterProvider i ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="5b80a-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5b80a-350">Zadanie 3. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b80a-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="5b80a-351">W tym zadaniu zostanie Uruchom aplikację i przetestować, czy filtr akcji niestandardowej jest śledzenie aktywności:</span><span class="sxs-lookup"><span data-stu-id="5b80a-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="5b80a-352">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5b80a-353">Kliknij przycisk **Rock** w Menu gatunki.</span><span class="sxs-lookup"><span data-stu-id="5b80a-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="5b80a-354">Jeśli chcesz, można przeglądać gatunki więcej.</span><span class="sxs-lookup"><span data-stu-id="5b80a-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="5b80a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="5b80a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="5b80a-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="5b80a-356">*Music Store*</span></span>
3. <span data-ttu-id="5b80a-357">Przejdź do **/Trace.axd** aby zobaczyć śledzenie aplikacji strony, a następnie kliknij przycisk **Wyświetl szczegóły**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="5b80a-358">![Dziennik śledzenia aplikacji](aspnet-mvc-4-dependency-injection/_static/image12.png "dziennika śledzenia aplikacji")</span><span class="sxs-lookup"><span data-stu-id="5b80a-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="5b80a-359">*Dziennik śledzenia aplikacji*</span><span class="sxs-lookup"><span data-stu-id="5b80a-359">*Application Trace Log*</span></span>

    <span data-ttu-id="5b80a-360">![Śledzenie aplikacji — szczegóły żądania](aspnet-mvc-4-dependency-injection/_static/image13.png "śledzenia aplikacji — szczegóły żądania")</span><span class="sxs-lookup"><span data-stu-id="5b80a-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="5b80a-361">*Śledzenie aplikacji — szczegóły żądania*</span><span class="sxs-lookup"><span data-stu-id="5b80a-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="5b80a-362">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="5b80a-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5b80a-363">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="5b80a-363">Summary</span></span>

<span data-ttu-id="5b80a-364">Przez ukończenie tego laboratorium praktyczne mają przedstawiono sposób korzystania wstrzykiwanie zależności w technologii ASP.NET MVC 4, integrując Unity przy użyciu pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="5b80a-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="5b80a-365">Aby to osiągnąć, użyto wstrzykiwanie zależności wewnątrz kontrolerów, widoki i filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="5b80a-366">Następujące pojęcia zostały objęte pomocą techniczną:</span><span class="sxs-lookup"><span data-stu-id="5b80a-366">The following concepts were covered:</span></span>

- <span data-ttu-id="5b80a-367">Funkcje programu ASP.NET MVC 4 wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="5b80a-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="5b80a-368">Integracja aparatu Unity przy użyciu pakietu NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="5b80a-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="5b80a-369">Wstrzykiwanie zależności w kontrolerów</span><span class="sxs-lookup"><span data-stu-id="5b80a-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="5b80a-370">Wstrzykiwanie zależności w widokach</span><span class="sxs-lookup"><span data-stu-id="5b80a-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="5b80a-371">Wstrzykiwanie zależności filtrów akcji</span><span class="sxs-lookup"><span data-stu-id="5b80a-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5b80a-372">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="5b80a-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5b80a-373">Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5b80a-374">Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="5b80a-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5b80a-375">Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5b80a-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5b80a-376">Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b80a-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="5b80a-377">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-377">Click on **Install Now**.</span></span> <span data-ttu-id="5b80a-378">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="5b80a-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5b80a-379">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="5b80a-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5b80a-380">![Zainstaluj program Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "zainstalować program Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5b80a-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5b80a-381">*Zainstaluj program Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5b80a-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5b80a-382">Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5b80a-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="5b80a-384">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="5b80a-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5b80a-385">Zaczekaj, aż do zakończenia procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-385">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="5b80a-387">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="5b80a-387">*Installation progress*</span></span>
6. <span data-ttu-id="5b80a-388">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-388">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="5b80a-390">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="5b80a-390">*Installation completed*</span></span>
7. <span data-ttu-id="5b80a-391">Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5b80a-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5b80a-392">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="5b80a-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="5b80a-394">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="5b80a-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="5b80a-395">Dodatek B: korzystania z fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="5b80a-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="5b80a-396">Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="5b80a-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5b80a-397">Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="5b80a-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5b80a-398">![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="5b80a-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5b80a-399">*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*</span><span class="sxs-lookup"><span data-stu-id="5b80a-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5b80a-400">***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***</span><span class="sxs-lookup"><span data-stu-id="5b80a-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5b80a-401">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="5b80a-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5b80a-402">Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="5b80a-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5b80a-403">Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5b80a-404">Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5b80a-405">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="5b80a-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5b80a-406">![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-dependency-injection/_static/image20.png "Rozpocznij wpisywanie nazwy fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="5b80a-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5b80a-407">*Rozpocznij wpisywanie nazwy fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="5b80a-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="5b80a-408">![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-dependency-injection/_static/image21.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="5b80a-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5b80a-409">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="5b80a-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5b80a-410">![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-dependency-injection/_static/image22.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="5b80a-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5b80a-411">*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*</span><span class="sxs-lookup"><span data-stu-id="5b80a-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5b80a-412">***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5b80a-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5b80a-413">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="5b80a-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5b80a-414">Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.</span><span class="sxs-lookup"><span data-stu-id="5b80a-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5b80a-415">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="5b80a-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5b80a-416">![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-dependency-injection/_static/image23.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="5b80a-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5b80a-417">*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="5b80a-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5b80a-418">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-dependency-injection/_static/image24.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="5b80a-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5b80a-419">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="5b80a-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
