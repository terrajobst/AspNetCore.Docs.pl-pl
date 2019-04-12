---
title: Omówienie platformy ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak ASP.NET Core MVC zaawansowaną strukturę do tworzenia aplikacji sieci web i interfejsów API za pomocą Model-View-Controller wzorca projektowego.
ms.author: riande
ms.date: 01/08/2018
uid: mvc/overview
ms.openlocfilehash: eb636616ab952f506993ae5f4692e5e966bb20f4
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516303"
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="7c66f-103">Omówienie platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7c66f-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="7c66f-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7c66f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7c66f-105">Platforma ASP.NET Core MVC jest zaawansowaną strukturę do tworzenia aplikacji sieci web i interfejsów API za pomocą Model-View-Controller wzorca projektowego.</span><span class="sxs-lookup"><span data-stu-id="7c66f-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="7c66f-106">Co to jest wzorzec MVC?</span><span class="sxs-lookup"><span data-stu-id="7c66f-106">What is the MVC pattern?</span></span>

<span data-ttu-id="7c66f-107">Wzorzec architektury Model-View-Controller (MVC) dzieli aplikację na trzy główne grupy składników: Modeli, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7c66f-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="7c66f-108">Ten wzorzec pomaga osiągnąć [separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="7c66f-108">This pattern helps to achieve [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span> <span data-ttu-id="7c66f-109">Korzystając z tego wzorca, żądań użytkowników są kierowane do kontrolera, który jest odpowiedzialny za współpracę z modelu, który ma wykonywać akcje użytkownika i/lub pobrać wyniki zapytania.</span><span class="sxs-lookup"><span data-stu-id="7c66f-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="7c66f-110">Kontroler wybiera widok, aby wyświetlić użytkownikowi i dostarcza mu żadnych danych modelu, który wymaga.</span><span class="sxs-lookup"><span data-stu-id="7c66f-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="7c66f-111">Na poniższym diagramie przedstawiono trzy główne składniki i te, które odwołują się inne:</span><span class="sxs-lookup"><span data-stu-id="7c66f-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Wzorzec MVC](overview/_static/mvc.png)

<span data-ttu-id="7c66f-113">Ta nakreślenia obowiązki ułatwia skalowanie aplikacji w sensie złożoność, ponieważ jest łatwiejsza do kodu, debugowania i testowania (model, widok lub kontroler) coś, co ma pojedynczego zadania.</span><span class="sxs-lookup"><span data-stu-id="7c66f-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job.</span></span> <span data-ttu-id="7c66f-114">Jest trudniejsze do aktualizacji, testowania i debugowania kodu, obejmującego zależności rozkładają się na dwóch lub więcej z tych trzech obszarów.</span><span class="sxs-lookup"><span data-stu-id="7c66f-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="7c66f-115">Na przykład logika interfejsu użytkownika zwykle zmieniać częściej niż logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="7c66f-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="7c66f-116">Jeśli prezentacji kodu i logiki biznesowej są łączone w pojedynczy obiekt, można zmodyfikować obiekt zawierający logikę biznesową, za każdym razem, gdy interfejs użytkownika zostanie zmieniony.</span><span class="sxs-lookup"><span data-stu-id="7c66f-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="7c66f-117">To często wprowadza błędy i wymaga przetestowanie logiki biznesowej po każdej zmianie interfejsu użytkownika minimalnej.</span><span class="sxs-lookup"><span data-stu-id="7c66f-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="7c66f-118">Widok i kontroler zależeć od modelu.</span><span class="sxs-lookup"><span data-stu-id="7c66f-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="7c66f-119">Jednak model jest zależna od widoku ani kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7c66f-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="7c66f-120">Jest to jedna z najważniejszych korzyści zapewnianych przez oddzielenie.</span><span class="sxs-lookup"><span data-stu-id="7c66f-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="7c66f-121">Ta separacja pozwala modelu, który ma być tworzone i testowane na niezależne od wizualnej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="7c66f-122">Obowiązki modelu</span><span class="sxs-lookup"><span data-stu-id="7c66f-122">Model Responsibilities</span></span>

<span data-ttu-id="7c66f-123">Modelu w aplikacji MVC reprezentuje stan aplikacji, wszelka logika biznesowa i operacje, które powinny być wykonywane przez nią.</span><span class="sxs-lookup"><span data-stu-id="7c66f-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="7c66f-124">Logika biznesowa powinna hermetyzowane w modelu wraz z wszelka logika implementacji na przechowywanie stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="7c66f-125">Silnie typizowane widoki zwykle użyć typów ViewModel przeznaczone do wyświetlania danych do wyświetlenia w tym widoku.</span><span class="sxs-lookup"><span data-stu-id="7c66f-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="7c66f-126">Kontroler tworzy i wypełnia te wystąpienia ViewModel z modelu.</span><span class="sxs-lookup"><span data-stu-id="7c66f-126">The controller creates and populates these ViewModel instances from the model.</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="7c66f-127">Wyświetl zakres odpowiedzialności</span><span class="sxs-lookup"><span data-stu-id="7c66f-127">View Responsibilities</span></span>

<span data-ttu-id="7c66f-128">Widoki są odpowiedzialny za prezentowanie zawartości za pomocą interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7c66f-128">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="7c66f-129">Używają [aparatu widoku Razor](#razor-view-engine) osadzanie kodu .NET w kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="7c66f-129">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="7c66f-130">Powinien być minimalny logiki w obrębie widoków i wszelka logika w nich odnoszą się do prezentowania zawartości.</span><span class="sxs-lookup"><span data-stu-id="7c66f-130">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="7c66f-131">Jeśli okaże się, trzeba wykonać dużym stopniem logiki w widoku plików w celu wyświetlania danych z modelu złożonego, należy wziąć pod uwagę przy użyciu [widoku składnika](views/view-components.md), ViewModel, lub Wyświetl szablon, aby uprościć widok.</span><span class="sxs-lookup"><span data-stu-id="7c66f-131">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="7c66f-132">Obowiązki kontrolera</span><span class="sxs-lookup"><span data-stu-id="7c66f-132">Controller Responsibilities</span></span>

<span data-ttu-id="7c66f-133">Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, pracy z modelem i ostatecznie wybierają widok do renderowania.</span><span class="sxs-lookup"><span data-stu-id="7c66f-133">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="7c66f-134">W aplikacji MVC widok zawiera tylko informacje; Kontroler obsługuje i reaguje na dane wejściowe użytkownika i interakcji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-134">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="7c66f-135">We wzorcu MVC kontroler jest punktem wejścia początkowej i jest odpowiedzialny za wybranie model, który typy do pracy z i widok do renderowania (dlatego jego nazwa - kontroluje sposób aplikacja reaguje na dane żądanie).</span><span class="sxs-lookup"><span data-stu-id="7c66f-135">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="7c66f-136">Kontrolery nie powinien zbyt skomplikowane, przez zbyt wiele obowiązki.</span><span class="sxs-lookup"><span data-stu-id="7c66f-136">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="7c66f-137">Aby zapobiec logiką kontrolera staje się zbyt skomplikowana, Wypchnij logika biznesowa z kontrolerem z do modelu domeny.</span><span class="sxs-lookup"><span data-stu-id="7c66f-137">To keep controller logic from becoming overly complex, push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="7c66f-138">Jeśli okaże się, że akcje kontrolera często wykonują te same czynności, Przenieś następujące typowe akcje do [filtry](#filters).</span><span class="sxs-lookup"><span data-stu-id="7c66f-138">If you find that your controller actions frequently perform the same kinds of actions, move these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="7c66f-139">What is ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7c66f-139">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="7c66f-140">Platforma ASP.NET Core MVC jest uproszczone, typu open source, wysoce testowalna presentation framework zoptymalizowana do użytku z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c66f-140">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="7c66f-141">Platforma ASP.NET Core MVC udostępnia bazujący na wzorcach sposób tworzenia dynamicznych witryn internetowych, który umożliwia czyste rozdzielenie problemów.</span><span class="sxs-lookup"><span data-stu-id="7c66f-141">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="7c66f-142">Zapewnia pełną kontrolę nad znacznikami, obsługuje programowanie TDD przyjaznego i korzysta z najnowszych standardów sieci web.</span><span class="sxs-lookup"><span data-stu-id="7c66f-142">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="7c66f-143">Funkcje</span><span class="sxs-lookup"><span data-stu-id="7c66f-143">Features</span></span>

<span data-ttu-id="7c66f-144">Platforma ASP.NET Core MVC obejmuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="7c66f-144">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="7c66f-145">Routing</span><span class="sxs-lookup"><span data-stu-id="7c66f-145">Routing</span></span>](#routing)
* [<span data-ttu-id="7c66f-146">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="7c66f-146">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="7c66f-147">Walidacja modelu</span><span class="sxs-lookup"><span data-stu-id="7c66f-147">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="7c66f-148">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="7c66f-148">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="7c66f-149">Filtry</span><span class="sxs-lookup"><span data-stu-id="7c66f-149">Filters</span></span>](#filters)
* [<span data-ttu-id="7c66f-150">Obszary</span><span class="sxs-lookup"><span data-stu-id="7c66f-150">Areas</span></span>](#areas)
* [<span data-ttu-id="7c66f-151">Interfejsy API sieci Web</span><span class="sxs-lookup"><span data-stu-id="7c66f-151">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="7c66f-152">Testowalności</span><span class="sxs-lookup"><span data-stu-id="7c66f-152">Testability</span></span>](#testability)
* [<span data-ttu-id="7c66f-153">Aparat widoku razor</span><span class="sxs-lookup"><span data-stu-id="7c66f-153">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="7c66f-154">Silnie typizowane widoki</span><span class="sxs-lookup"><span data-stu-id="7c66f-154">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="7c66f-155">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="7c66f-155">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="7c66f-156">Składniki widoków</span><span class="sxs-lookup"><span data-stu-id="7c66f-156">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="7c66f-157">Routing</span><span class="sxs-lookup"><span data-stu-id="7c66f-157">Routing</span></span>

<span data-ttu-id="7c66f-158">ASP.NET Core MVC jest wbudowana w górnej części [routingu platformy ASP.NET Core](../fundamentals/routing.md), zaawansowane składnik Mapowanie adresu URL, który pozwala tworzyć aplikacje, które mają adresy URL zrozumiałe i którą można przeszukiwać.</span><span class="sxs-lookup"><span data-stu-id="7c66f-158">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="7c66f-159">Dzięki temu można zdefiniować aplikacji wzorców nazewnictwa adresów URL, które działa dobrze w przypadku optymalizacji dla aparatów wyszukiwania (SEO) oraz do generowania łącza, bez względu na sposób organizowania są pliki na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="7c66f-159">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="7c66f-160">Można zdefiniować przy użyciu składni szablonu dogodnym tras, które obsługuje ograniczenia wartości trasy, wartości domyślne i opcjonalne wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="7c66f-160">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="7c66f-161">*Routing oparty na Konwencji* formatuje można zdefiniować globalny adres URL, który umożliwia aplikacji akceptuje i jak każdy z tych formatów mapuje do metody akcji określonych w podanego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7c66f-161">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="7c66f-162">Po odebraniu żądania przychodzącego aparat routingu analizuje adres URL i dopasowuje je do jednej z określonych formatów adresów URL, a następnie wywołuje metodę akcji kontrolera skojarzone.</span><span class="sxs-lookup"><span data-stu-id="7c66f-162">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="7c66f-163">*Atrybut routingu* umożliwia określenie informacji o routingu przez urządzanie kontrolerów i akcji przy użyciu atrybutów, które definiują trasy Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-163">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="7c66f-164">Oznacza to, że definicji trasy są umieszczane obok kontrolera i akcji, z którymi są powiązane.</span><span class="sxs-lookup"><span data-stu-id="7c66f-164">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="7c66f-165">Powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="7c66f-165">Model binding</span></span>

<span data-ttu-id="7c66f-166">Platforma ASP.NET Core MVC [wiązanie modelu](models/model-binding.md) konwertuje dane z żądania klienta (wartości formularza, danych trasy, parametry ciągu zapytania, nagłówki HTTP) na obiekty, które może obsłużyć kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7c66f-166">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="7c66f-167">Co w efekcie logikę kontrolera nie musi wykonywać pracę ustalenie dane przychodzące żądania; po prostu ma danych jako parametry do jego metod akcji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-167">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="7c66f-168">Walidacja modelu</span><span class="sxs-lookup"><span data-stu-id="7c66f-168">Model validation</span></span>

<span data-ttu-id="7c66f-169">Obsługuje platformy ASP.NET Core MVC [weryfikacji](models/validation.md) przez urządzanie obiektu modelu przy użyciu atrybutów weryfikacji adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="7c66f-169">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="7c66f-170">Atrybutów sprawdzania poprawności są sprawdzane po stronie klienta, zanim wartości są ogłaszane na serwerze, a także jest wywoływana na serwerze przed akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7c66f-170">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="7c66f-171">Akcja kontrolera:</span><span class="sxs-lookup"><span data-stu-id="7c66f-171">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="7c66f-172">Struktura obsługuje sprawdzanie poprawności danych żądania zarówno na kliencie, jak i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7c66f-172">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="7c66f-173">Logikę weryfikacji określony dla typów modelu jest dodawana do widoków renderowany jako adnotacje dyskretnego kodu i są wymuszane w przeglądarce z [jQuery weryfikacji](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="7c66f-173">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="7c66f-174">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="7c66f-174">Dependency injection</span></span>

<span data-ttu-id="7c66f-175">Platforma ASP.NET Core ma wbudowaną obsługę [wstrzykiwanie zależności (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7c66f-175">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="7c66f-176">W programie ASP.NET Core MVC [kontrolerów](controllers/dependency-injection.md) można żądania potrzebnych usług za pośrednictwem ich konstruktory, umożliwiając im postępuj zgodnie z [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="7c66f-176">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

<span data-ttu-id="7c66f-177">Aplikację można również użyć [wstrzykiwanie zależności w widoku plików](views/dependency-injection.md)przy użyciu `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="7c66f-177">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="7c66f-178">Filtry</span><span class="sxs-lookup"><span data-stu-id="7c66f-178">Filters</span></span>

<span data-ttu-id="7c66f-179">[Filtry](controllers/filters.md) ułatwiają deweloperom hermetyzacji odciąż przekrojowe zagadnienia, takie jak obsługa wyjątków i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-179">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="7c66f-180">Filtry Włącz logiki niestandardowej uruchomiona przed i przetwarzanie końcowe dla metody akcji i mogą być skonfigurowane do uruchamiania w określonych punktach w potoku wykonywania dla danego żądania.</span><span class="sxs-lookup"><span data-stu-id="7c66f-180">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="7c66f-181">Filtry mogą być stosowane do kontrolerów lub akcje jako atrybuty (lub mogą być uruchamiane globalnie).</span><span class="sxs-lookup"><span data-stu-id="7c66f-181">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="7c66f-182">Kilka filtrów (takie jak `Authorize`) znajdują się w ramach.</span><span class="sxs-lookup"><span data-stu-id="7c66f-182">Several filters (such as `Authorize`) are included in the framework.</span></span> <span data-ttu-id="7c66f-183">`[Authorize]` to atrybut, który jest używany do tworzenia filtrów autoryzacji platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="7c66f-183">`[Authorize]` is the attribute that is used to create MVC authorization filters.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
```

### <a name="areas"></a><span data-ttu-id="7c66f-184">Obszary</span><span class="sxs-lookup"><span data-stu-id="7c66f-184">Areas</span></span>

<span data-ttu-id="7c66f-185">[Obszary](controllers/areas.md) umożliwiają do dzielenia dużych aplikacji sieci Web platformy ASP.NET Core MVC na mniejsze grupy funkcjonalnej.</span><span class="sxs-lookup"><span data-stu-id="7c66f-185">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="7c66f-186">Obszar to struktura MVC wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-186">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="7c66f-187">W projekcie MVC logiczne składniki, takie jak Model, kontroler i Widok są przechowywane w różnych folderach, i są używane konwencje nazewnictwa do utworzenia relacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="7c66f-187">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="7c66f-188">W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-188">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="7c66f-189">Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyszukiwanie itp wyewidencjonowanie i rozliczeniami. Każda z tych jednostek ma swoje własne widoki logiczny składnik, kontrolery i modeli.</span><span class="sxs-lookup"><span data-stu-id="7c66f-189">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="7c66f-190">Interfejsy Web API</span><span class="sxs-lookup"><span data-stu-id="7c66f-190">Web APIs</span></span>

<span data-ttu-id="7c66f-191">Oprócz znakomitą platformę dla tworzenia witryn sieci web, ASP.NET Core MVC obsługuje doskonałe do tworzenia interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7c66f-191">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="7c66f-192">Można tworzyć usługi, którzy osiągną szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="7c66f-192">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="7c66f-193">Struktura obsługuje negocjowanie zawartości HTTP z wbudowaną obsługą do [formatowania danych](xref:web-api/advanced/formatting) jak JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="7c66f-193">The framework includes support for HTTP content-negotiation with built-in support to [format data](xref:web-api/advanced/formatting) as JSON or XML.</span></span> <span data-ttu-id="7c66f-194">Zapis [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters) dodanie obsługi własnych formatów.</span><span class="sxs-lookup"><span data-stu-id="7c66f-194">Write [custom formatters](xref:web-api/advanced/custom-formatters) to add support for your own formats.</span></span>

<span data-ttu-id="7c66f-195">Użyj generowania łącza, aby włączyć obsługę hipermediach.</span><span class="sxs-lookup"><span data-stu-id="7c66f-195">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="7c66f-196">Łatwo włączyć obsługę [współużytkowanie zasobów między źródłami (cors)](http://www.w3.org/TR/cors/) tak, aby interfejsy API sieci Web mogą być współużytkowane przez wiele aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7c66f-196">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="7c66f-197">Testowalności</span><span class="sxs-lookup"><span data-stu-id="7c66f-197">Testability</span></span>

<span data-ttu-id="7c66f-198">Użyj struktury interfejsów i wstrzykiwanie zależności przekształcić ją w odpowiednie do testowania jednostkowego i struktura zawiera funkcje (na przykład TestHost i InMemory dostawca programu Entity Framework) [testy integracji](xref:test/integration-tests) szybki i łatwe jak również.</span><span class="sxs-lookup"><span data-stu-id="7c66f-198">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration tests](xref:test/integration-tests) quick and easy as well.</span></span> <span data-ttu-id="7c66f-199">Dowiedz się więcej o [testowanie logiką kontrolera](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="7c66f-199">Learn more about [how to test controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="7c66f-200">Aparat widoku razor</span><span class="sxs-lookup"><span data-stu-id="7c66f-200">Razor view engine</span></span>

<span data-ttu-id="7c66f-201">[ASP.NET Core MVC widoków](views/overview.md) użyj [aparatu widoku Razor](views/razor.md) do renderowania widoków.</span><span class="sxs-lookup"><span data-stu-id="7c66f-201">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="7c66f-202">Razor jest językiem znaczników compact, ekspresyjny i płynnych szablonu służącego do definiowania widoków przy użyciu osadzonego kodu C#.</span><span class="sxs-lookup"><span data-stu-id="7c66f-202">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="7c66f-203">Razor jest używana do dynamicznego generowania zawartości sieci web na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7c66f-203">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="7c66f-204">Kod serwera można prawidłowo łączyć za pomocą kodu i zawartości po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7c66f-204">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="7c66f-205">Za pomocą aparatu widoku Razor, można zdefiniować [układy](views/layout.md), [widoki częściowe](views/partial.md) i wymienne sekcji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-205">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="7c66f-206">Silnie typizowane widoki</span><span class="sxs-lookup"><span data-stu-id="7c66f-206">Strongly typed views</span></span>

<span data-ttu-id="7c66f-207">Widokami razor, MVC może być silnie typizowane oparte na modelu.</span><span class="sxs-lookup"><span data-stu-id="7c66f-207">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="7c66f-208">Kontrolery można przekazać silnie typizowany model z widokami włączanie widoków sprawdzania typu i obsługuje funkcję IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="7c66f-208">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="7c66f-209">Na przykład następujący widok renderuje model typu `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="7c66f-209">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="7c66f-210">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="7c66f-210">Tag Helpers</span></span>

<span data-ttu-id="7c66f-211">[Pomocników tagów](views/tag-helpers/intro.md) włączyć kodu po stronie serwera wziąć udział w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="7c66f-211">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="7c66f-212">Pomocnicy tagów umożliwia definiowanie tagów niestandardowych (na przykład `<environment>`) lub do zmodyfikowania zachowania istniejących tagów (na przykład `<label>`).</span><span class="sxs-lookup"><span data-stu-id="7c66f-212">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="7c66f-213">Pomocnicy tagów powiązać z określonych elementów na podstawie nazwy elementu i jego atrybuty.</span><span class="sxs-lookup"><span data-stu-id="7c66f-213">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="7c66f-214">Zapewniają korzyści wynikające z renderowania po stronie serwera, zachowując przy tym nadal środowisko edytowania plików języka HTML.</span><span class="sxs-lookup"><span data-stu-id="7c66f-214">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="7c66f-215">Istnieje wiele wbudowanych pomocników tagów dla typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakiety więcej — i jeszcze bardziej dostępne w publicznych repozytoriach GitHub oraz jak NuGet.</span><span class="sxs-lookup"><span data-stu-id="7c66f-215">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="7c66f-216">Pomocnicy tagów są tworzone w języku C#, a ich celem są elementy HTML na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym.</span><span class="sxs-lookup"><span data-stu-id="7c66f-216">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="7c66f-217">Na przykład wbudowane LinkTagHelper można utworzyć łącze do `Login` akcji `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="7c66f-217">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="7c66f-218">`EnvironmentTagHelper` Może służyć do uwzględnienia różnych skryptów w widoków (na przykład raw lub zminimalizowana) oparte na środowisko uruchomieniowe, takich jak deweloperskie, przejściowe lub produkcyjne:</span><span class="sxs-lookup"><span data-stu-id="7c66f-218">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="7c66f-219">Pomocnicy tagów zapewniają środowisko programistyczne przyjaznego dla kodu HTML i bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor.</span><span class="sxs-lookup"><span data-stu-id="7c66f-219">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="7c66f-220">Większość wbudowanych pomocników tagów docelowe istniejące elementy HTML i udostępniania atrybutów po stronie serwera dla elementu.</span><span class="sxs-lookup"><span data-stu-id="7c66f-220">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="7c66f-221">Składniki widoków</span><span class="sxs-lookup"><span data-stu-id="7c66f-221">View Components</span></span>

<span data-ttu-id="7c66f-222">[Wyświetlanie składników](views/view-components.md) umożliwiają pakietu logiki renderowania i użyć go ponownie w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c66f-222">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="7c66f-223">Są one podobne do [widoki częściowe](views/partial.md), ale przy użyciu skojarzonej logiki.</span><span class="sxs-lookup"><span data-stu-id="7c66f-223">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>

## <a name="compatibility-version"></a><span data-ttu-id="7c66f-224">Wersja zgodności</span><span class="sxs-lookup"><span data-stu-id="7c66f-224">Compatibility version</span></span>

<span data-ttu-id="7c66f-225">Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> umożliwia aplikacji włączenie wykorzystania lub rezygnację ze zmian zachowania wprowadzanych w programie ASP.NET Core MVC w wersji 2.1 lub nowszej, które potencjalnie mogą prowadzić do awarii.</span><span class="sxs-lookup"><span data-stu-id="7c66f-225">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="7c66f-226">Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="7c66f-226">For more information, see <xref:mvc/compatibility-version>.</span></span>
