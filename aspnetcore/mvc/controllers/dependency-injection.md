---
title: "Iniekcji zależności do kontrolerów"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 946d695c572379c3ebc2eda1569f186f25ab9bfc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="064ec-102">Iniekcji zależności do kontrolerów</span><span class="sxs-lookup"><span data-stu-id="064ec-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="064ec-103">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="064ec-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="064ec-104">Kontrolerów MVC ASP.NET Core należy zażądać ich zależności jawnie za pośrednictwem ich konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="064ec-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="064ec-105">W niektórych przypadkach akcji kontrolera poszczególnych może wymagać usługi, a jego może nie mieć sensu żądanie na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="064ec-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="064ec-106">W takim przypadku również można wstrzyknąć usługi jako parametr metody akcji.</span><span class="sxs-lookup"><span data-stu-id="064ec-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="064ec-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="064ec-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="064ec-108">Iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="064ec-108">Dependency Injection</span></span>

<span data-ttu-id="064ec-109">Iniekcji zależności to technika, który następuje [zasady odwracanie zależności](http://deviq.com/dependency-inversion-principle/), dzięki czemu aplikacje mogą składa się z modułów luźno powiązanych.</span><span class="sxs-lookup"><span data-stu-id="064ec-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="064ec-110">Platformy ASP.NET Core ma wbudowaną obsługę [iniekcji zależności](../../fundamentals/dependency-injection.md), która ułatwia aplikacji do testowania i obsługi.</span><span class="sxs-lookup"><span data-stu-id="064ec-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="064ec-111">Iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="064ec-111">Constructor Injection</span></span>

<span data-ttu-id="064ec-112">Rozszerzenie platformy ASP.NET Core wbudowaną obsługę iniekcji zależności na podstawie konstruktora do kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="064ec-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="064ec-113">Po prostu dodając typ usługi do kontrolera jako parametru konstruktora, platformy ASP.NET Core podejmie próbę rozpoznania tego typu za pomocą swojej wbudowanej w kontenerze usług.</span><span class="sxs-lookup"><span data-stu-id="064ec-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="064ec-114">Usługi są zwykle, ale nie zawsze zdefiniowane, za pomocą interfejsów.</span><span class="sxs-lookup"><span data-stu-id="064ec-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="064ec-115">Na przykład jeśli aplikacja ma logiki biznesowej, które jest zależny od bieżącego czasu, można wstrzyknąć to usługa, która pobiera czas (zamiast kodować je), co pozwoliłoby testów do przekazania w implementacjach korzystających z określonym czasie.</span><span class="sxs-lookup"><span data-stu-id="064ec-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="064ec-116">Implementowanie interfejsu podobny do tego, tak aby były używane w czasie wykonywania zegar systemowy jest prosta:</span><span class="sxs-lookup"><span data-stu-id="064ec-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="064ec-117">Dzięki temu w miejscu możemy korzystać z usługi w kontrolera.</span><span class="sxs-lookup"><span data-stu-id="064ec-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="064ec-118">W takim przypadku dodano logikę do `HomeController` `Index` metodę w celu wyświetlenia pozdrowienia użytkownikowi na podstawie czasu dnia.</span><span class="sxs-lookup"><span data-stu-id="064ec-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="064ec-119">Czy możemy uruchomić aplikację teraz, firma Microsoft najprawdopodobniej wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="064ec-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="064ec-120">Ten błąd występuje, gdy firma Microsoft nie zostały skonfigurowane w usługach `ConfigureServices` metody w naszym `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="064ec-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="064ec-121">Aby określić, która zażąda `IDateTime` mają zostać rozwiązane, za pomocą wystąpienia `SystemDateTime`, Dodaj wiersz wyróżnione na liście poniżej, aby Twoje `ConfigureServices` — metoda:</span><span class="sxs-lookup"><span data-stu-id="064ec-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="064ec-122">Tej konkretnej usługi można zaimplementować za pomocą dowolnej z kilku opcji istnienia różnych (`Transient`, `Scoped`, lub `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="064ec-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="064ec-123">Zobacz [iniekcji zależności](../../fundamentals/dependency-injection.md) zrozumieć, jak każdej z tych opcji zakresu wpływają na zachowanie usługi.</span><span class="sxs-lookup"><span data-stu-id="064ec-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="064ec-124">Po skonfigurowaniu usługi uruchamiania aplikacji i przechodząc na stronę główną powinien być wyświetlany komunikat na podstawie czasu zgodnie z oczekiwaniami:</span><span class="sxs-lookup"><span data-stu-id="064ec-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Serwer pozdrowienia](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="064ec-126">Zobacz [logiką kontrolera testów](testing.md) informacje na temat jawne żądanie zależności [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) kontrolery ułatwia kod do testowania.</span><span class="sxs-lookup"><span data-stu-id="064ec-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="064ec-127">Iniekcji zależności wbudowanych platformy ASP.NET Core obsługuje posiadanie tylko jednego konstruktora dla klasy żądanie usługi.</span><span class="sxs-lookup"><span data-stu-id="064ec-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="064ec-128">Jeśli masz więcej niż jeden konstruktor może wystąpić wyjątek z informacją:</span><span class="sxs-lookup"><span data-stu-id="064ec-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="064ec-129">Jak Stany komunikat o błędzie, należy rozwiązać ten problem, w posiadanie jednego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="064ec-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="064ec-130">Możesz również [Zamień obsługi iniekcji zależności Domyślna implementacja innej](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), duża liczba obsługują wiele konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="064ec-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="064ec-131">Akcja iniekcja FromServices</span><span class="sxs-lookup"><span data-stu-id="064ec-131">Action Injection with FromServices</span></span>

<span data-ttu-id="064ec-132">Czasami niepotrzebne usługi dla więcej niż jednej akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="064ec-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="064ec-133">W takim przypadku rozsądne może okazać się do dodania usługi jako parametr do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="064ec-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="064ec-134">Jest to zrobić przez oznaczenie parametr z atrybutem `[FromServices]` w sposób pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="064ec-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="064ec-135">Uzyskiwanie dostępu do ustawień z kontrolera</span><span class="sxs-lookup"><span data-stu-id="064ec-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="064ec-136">Uzyskiwanie dostępu do aplikacji lub Konfiguracja ustawień z poziomu kontrolera jest wspólnym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="064ec-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="064ec-137">Tego dostępu należy używać wzorzec opcje opisane w [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="064ec-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="064ec-138">Zazwyczaj nie powinny żądania ustawienia bezpośrednio w kontrolerze przy użyciu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="064ec-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="064ec-139">Lepszym rozwiązaniem jest żądanie `IOptions<T>` wystąpienia, gdzie `T` jest klasa konfiguracji potrzebne.</span><span class="sxs-lookup"><span data-stu-id="064ec-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="064ec-140">Aby pracować z wzorcem opcji, należy utworzyć klasę, która reprezentuje opcje, takie jak ta:</span><span class="sxs-lookup"><span data-stu-id="064ec-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="064ec-141">Następnie należy skonfigurować aplikację do korzystania z modelu opcje i dodać do kolekcji usług w klasie konfiguracji `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="064ec-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="064ec-142">Na liście powyżej, możemy konfigurowania aplikacji na odczytywanie ustawień z pliku w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="064ec-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="064ec-143">Można również skonfigurować ustawienia całkowicie w kodzie, jak w powyższym kodzie komentarze.</span><span class="sxs-lookup"><span data-stu-id="064ec-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="064ec-144">Zobacz [konfiguracji](xref:fundamentals/configuration/index) dla dalszego opcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="064ec-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="064ec-145">Po określeniu obiekt jednoznacznie konfiguracji (w tym przypadku `SampleWebSettings`) i dodaniu go do kolekcji usług, możesz poprosić go z dowolnego kontrolera lub metody akcji przez zażądanie wystąpienia `IOptions<T>` (w tym przypadku `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="064ec-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="064ec-146">Poniższy kod przedstawia, jak jeden z monitem ustawień z kontrolera:</span><span class="sxs-lookup"><span data-stu-id="064ec-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="064ec-147">Następujące opcje wzorzec umożliwia ustawienia i konfigurację być odłączona od siebie i zapewnia następujące jest kontrolerem [separacji](http://deviq.com/separation-of-concerns/), ponieważ nie musi wiedzieć, jak i gdzie można znaleźć ustawienia informacje.</span><span class="sxs-lookup"><span data-stu-id="064ec-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="064ec-148">On również ułatwia kontrolera testu jednostkowego [logiką kontrolera testów](testing.md), ponieważ istnieje nie [statycznych przylepna](http://deviq.com/static-cling/) lub bezpośrednie tworzenie wystąpień klas ustawienia należące do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="064ec-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
