---
title: Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak kontrolerów platformy ASP.NET Core MVC zażądać ich zależności, które jawnie za pomocą ich konstruktory mogę przy użyciu iniekcji zależności w programie ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 12247dbbbb6de3f8feb7bc37caec4ecf4bd21719
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206345"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="fd3d3-103">Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd3d3-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="fd3d3-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fd3d3-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fd3d3-105">Kontrolerów MVC platformy ASP.NET Core powinien zażądać ich zależności, które jawnie za pomocą ich konstruktory.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="fd3d3-106">W niektórych przypadkach akcji kontrolera indywidualne mogą wymagać usługi, i może nie mieć sensu żądań na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="fd3d3-107">W takim przypadku możesz również iniekcję usługi jako parametr metody akcji.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="fd3d3-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd3d3-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="fd3d3-109">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="fd3d3-109">Dependency Injection</span></span>

<span data-ttu-id="fd3d3-110">Wstrzykiwanie zależności jest techniką, która następuje po [zasady odwrócenie zależności](http://deviq.com/dependency-inversion-principle/), dzięki czemu aplikacje mogą się składać z luźno powiązanych modułów.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="fd3d3-111">Platforma ASP.NET Core ma wbudowaną obsługę [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md), która ułatwia aplikacji do testowania i obsługi.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="fd3d3-112">Iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="fd3d3-112">Constructor Injection</span></span>

<span data-ttu-id="fd3d3-113">Platforma ASP.NET Core wbudowaną obsługę wstrzykiwania zależności w oparciu o konstruktory rozszerza kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="fd3d3-114">Po prostu dodając typ usługi do kontrolera jako parametr konstruktora, ASP.NET Core będzie próbował rozpoznać typu przy użyciu jego wbudowanych w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="fd3d3-115">Usługi są zwykle, ale nie zawsze zdefiniowane, przy użyciu interfejsów.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="fd3d3-116">Na przykład jeśli aplikacja ma logikę biznesową, która jest zależna od bieżącego czasu, należy wstrzyknąć to usługa, która pobiera czas (zamiast kodować je), co pozwoliłoby testów do przekazania w implementacji, korzystających z określonym czasie.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="fd3d3-117">Implementowanie interfejsu podobny do tego, tak aby używał zegar systemowy w czasie wykonywania jest proste:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="fd3d3-118">Dzięki temu w miejscu możemy użyć usługi w naszym kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="fd3d3-119">W tym przypadku dodaliśmy logikę do `HomeController` `Index` metodę w celu wyświetlenia pozdrowienia dla użytkownika na podstawie czasu dnia.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="fd3d3-120">Jeśli możemy uruchomić aplikację teraz, firma Microsoft będzie prawdopodobnie wystąpi błąd:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="fd3d3-121">Ten błąd występuje, gdy firma Microsoft nie zostały skonfigurowane w usługach `ConfigureServices` method in Class metoda naszych `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="fd3d3-122">Aby określić, że żądania dla `IDateTime` mają zostać rozwiązane za pomocą wystąpienia `SystemDateTime`, Dodaj wyróżniony wiersz na liście poniżej, aby Twoje `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="fd3d3-123">Tej konkretnej usługi można zaimplementować przy użyciu dowolnego z kilku opcji inny okres istnienia (`Transient`, `Scoped`, lub `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="fd3d3-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="fd3d3-124">Zobacz [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) Aby zrozumieć, jak każdą z tych opcji zakresu będą wpływać na działanie usługi.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="fd3d3-125">Po skonfigurowaniu usługi działania aplikacji i przejdź do strony głównej powinien wyświetlić komunikat na podstawie czasu zgodnie z oczekiwaniami:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Powitanie serwera](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="fd3d3-127">Zobacz [logikę kontrolera testu](testing.md) informacje na temat jawne żądanie zależności [ http://deviq.com/explicit-dependencies-principle/ ](http://deviq.com/explicit-dependencies-principle/) kontrolery ułatwia kodu do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-127">See [Test controller logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="fd3d3-128">Wstrzykiwanie zależności wbudowanych w platformy ASP.NET Core obsługuje posiadanie tylko jednego konstruktora dla klas żądania usługi.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="fd3d3-129">Jeśli masz więcej niż jeden konstruktor, może wystąpić wyjątek z informacją:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="fd3d3-130">Jak komunikat o błędzie stwierdzający, możesz rozwiązać ten problem, przy użyciu jednego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-130">As the error message states, you can correct this problem with the use of a single constructor.</span></span> <span data-ttu-id="fd3d3-131">Możesz również [zastąpić domyślny kontener iniekcji zależności z implementacją firm](xref:fundamentals/dependency-injection#default-service-container-replacement), liczby, które obsługują wiele konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-131">You can also [replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="fd3d3-132">Akcja iniekcja FromServices</span><span class="sxs-lookup"><span data-stu-id="fd3d3-132">Action Injection with FromServices</span></span>

<span data-ttu-id="fd3d3-133">Czasami nie potrzebujesz usługi dla więcej niż jednej akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="fd3d3-134">W tym przypadku rozsądne może okazać się do dodania usługi jako parametr do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="fd3d3-135">Jest to realizowane przez oznaczenie parametr z atrybutem `[FromServices]` jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="fd3d3-136">Uzyskiwanie dostępu do ustawień za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="fd3d3-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="fd3d3-137">Uzyskiwanie dostępu do aplikacji lub konfiguracji ustawień z poziomu kontrolera jest typowy wzorzec.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="fd3d3-138">Ten dostęp, należy używać wzorca opcje opisane w [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="fd3d3-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="fd3d3-139">Ogólnie nie powinno żądania ustawień bezpośrednio z poziomu kontrolera, przy użyciu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-139">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="fd3d3-140">Lepszym rozwiązaniem jest żądanie `IOptions<T>` wystąpienia, gdzie `T` jest klasą konfiguracji potrzebne.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="fd3d3-141">Aby pracować z wzorcem opcji, należy utworzyć klasę, która reprezentuje opcje, taką jak ta:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="fd3d3-142">Następnie należy skonfigurować aplikację do używania tego modelu opcje i Dodaj klasy konfiguracji do kolekcji usługi w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="fd3d3-143">Na powyższej liście będziemy konfigurować aplikacji na odczytywanie ustawień z pliku w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="fd3d3-144">Można również skonfigurować ustawienia całkowicie w kodzie, jak pokazano w powyższym kodzie komentarzem.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="fd3d3-145">Zobacz [konfiguracji](xref:fundamentals/configuration/index) dalszych opcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="fd3d3-146">Po określeniu obiektu silnie typizowane konfiguracji (w tym przypadku `SampleWebSettings`) i dodaniu go do kolekcji usługi, możesz poprosić go dowolnego kontrolera lub metody akcji przez zażądanie wystąpienie `IOptions<T>` (w tym przypadku `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="fd3d3-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="fd3d3-147">Poniższy kod pokazuje, jak jeden z monitem ustawienia za pomocą kontrolera:</span><span class="sxs-lookup"><span data-stu-id="fd3d3-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="fd3d3-148">Wzorzec opcje umożliwia ustawienia i konfigurację być całkowicie niezależni od siebie nawzajem i zapewnia obserwowanych kontrolera [separacji](http://deviq.com/separation-of-concerns/), ponieważ nie musi wiedzieć, jak i gdzie można znaleźć ustawienia informacje.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="fd3d3-149">On również ułatwia kontrolera testu jednostkowego [logikę kontrolera testu](testing.md), ponieważ istnieje nie [statyczne przylepna](http://deviq.com/static-cling/) lub bezpośrednie wystąpienia klasy ustawienia w obrębie klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="fd3d3-149">It also makes the controller easier to unit test [Test controller logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
