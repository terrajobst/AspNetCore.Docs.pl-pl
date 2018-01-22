---
title: "Omówienie platformy ASP.NET Core MVC"
author: ardalis
description: "Dowiedz się, jak platformy ASP.NET Core MVC jest sformatowany framework do tworzenia aplikacji sieci web i interfejsów API przy użyciu Model-View-Controller projektowanie wzorca."
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: ad8a1dfae89a7ecd5573c16ba70d7d12216b4c57
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="e69af-103">Omówienie platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e69af-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="e69af-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e69af-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e69af-105">Platformy ASP.NET Core MVC jest sformatowany framework do tworzenia aplikacji sieci web i interfejsów API przy użyciu Model-View-Controller projektowanie wzorca.</span><span class="sxs-lookup"><span data-stu-id="e69af-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="e69af-106">Co to jest wzorzec MVC?</span><span class="sxs-lookup"><span data-stu-id="e69af-106">What is the MVC pattern?</span></span>

<span data-ttu-id="e69af-107">Wzorzec architektury Model-widok-kontroler (MVC) dzieli aplikację na trzy główne grupy składników: modele, widoki i kontrolery.</span><span class="sxs-lookup"><span data-stu-id="e69af-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="e69af-108">Ten wzorzec umożliwia uzyskanie [separacji](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="e69af-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="e69af-109">Za pomocą tego wzorca, żądań użytkowników są kierowane do kontrolera, który jest odpowiedzialny za pracy z modelem akcje użytkownika i/lub pobrać wyniki zapytania.</span><span class="sxs-lookup"><span data-stu-id="e69af-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="e69af-110">Kontroler wybierze widok, aby wyświetlić dla użytkownika i zapewnia Model danych, które wymaga.</span><span class="sxs-lookup"><span data-stu-id="e69af-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="e69af-111">Na poniższym diagramie przedstawiono trzy główne składniki i te, które odwołują się inne:</span><span class="sxs-lookup"><span data-stu-id="e69af-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Wzorzec MVC](overview/_static/mvc.png)

<span data-ttu-id="e69af-113">Ta nakreślenia obowiązki pomaga skalowanie aplikacji pod względem stopnia złożoności, ponieważ ułatwia kodu, debugowania i testowania coś (model, widok lub kontrolera) z jednym zadaniu (i jest zgodna z [jednej zasady odpowiedzialności ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="e69af-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="e69af-114">Jest trudne do aktualizacji, badanie i debugowania kodu, który ma zależności rozłożyć na co najmniej dwa z tych trzech obszarach.</span><span class="sxs-lookup"><span data-stu-id="e69af-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="e69af-115">Na przykład logika interfejsu użytkownika zwykle zmieniać częściej niż logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="e69af-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="e69af-116">Prezentacja kodu i logiki biznesowej są łączone w pojedynczy obiekt, należy zmodyfikować obiekt zawierający logikę biznesową, przy każdej zmianie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e69af-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="e69af-117">To może powodować błędy i wymagają ponowne wszystkich logiki biznesowej po zmianie co minimalnym interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e69af-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="e69af-118">Zarówno widok i kontroler zależą od modelu.</span><span class="sxs-lookup"><span data-stu-id="e69af-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="e69af-119">Jednak model zależy od widoku ani kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e69af-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="e69af-120">To jest jednym z kluczowych zalet separacji.</span><span class="sxs-lookup"><span data-stu-id="e69af-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="e69af-121">Ta separacja umożliwia niezależne od wizualną prezentację modelu, który ma zostać utworzony i przetestowane.</span><span class="sxs-lookup"><span data-stu-id="e69af-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="e69af-122">Obowiązki modelu</span><span class="sxs-lookup"><span data-stu-id="e69af-122">Model Responsibilities</span></span>

<span data-ttu-id="e69af-123">Model w aplikacji MVC reprezentuje stan aplikacji i wszelka logika biznesowa lub operacje, które powinny zostać wykonane przez nią.</span><span class="sxs-lookup"><span data-stu-id="e69af-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="e69af-124">Logika biznesowa powinna hermetyzowany w modelu, wraz z wszelka logika implementacji dla utrwalanie stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e69af-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="e69af-125">Widoki jednoznacznie zazwyczaj używają typów ViewModel przeznaczone do wyświetlania danych do wyświetlenia w tym widoku.</span><span class="sxs-lookup"><span data-stu-id="e69af-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="e69af-126">Kontroler tworzy i wypełnia te instancje ViewModel z modelu.</span><span class="sxs-lookup"><span data-stu-id="e69af-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="e69af-127">Istnieje wiele sposobów w celu zorganizowania modelu w aplikacji, która używa wzorzec architektury MVC.</span><span class="sxs-lookup"><span data-stu-id="e69af-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="e69af-128">Dowiedz się więcej o niektórych [różnych typów modeli](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="e69af-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="e69af-129">Obowiązki widoku</span><span class="sxs-lookup"><span data-stu-id="e69af-129">View Responsibilities</span></span>

<span data-ttu-id="e69af-130">Widoki są zobowiązani do prezentowania zawartości za pomocą interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e69af-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="e69af-131">Używają [aparatu widoku Razor](#razor-view-engine) osadzanie kodu platformy .NET w kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="e69af-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="e69af-132">Powinien być minimalny logikę w obrębie widoków i wszelka logika w nich odnoszą się do przedstawiania zawartości.</span><span class="sxs-lookup"><span data-stu-id="e69af-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="e69af-133">Jeśli trzeba wykonać dużą logikę w widoku pliki, aby wyświetlić dane z modelu złożonego, należy rozważyć użycie [składnika widoku](views/view-components.md), ViewModel, lub szablon widoku, aby uprościć widoku.</span><span class="sxs-lookup"><span data-stu-id="e69af-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="e69af-134">Obowiązki kontrolera</span><span class="sxs-lookup"><span data-stu-id="e69af-134">Controller Responsibilities</span></span>

<span data-ttu-id="e69af-135">Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, pracy z modelem i ostatecznie wybierają widok do renderowania.</span><span class="sxs-lookup"><span data-stu-id="e69af-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="e69af-136">W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja.</span><span class="sxs-lookup"><span data-stu-id="e69af-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="e69af-137">We wzorcu MVC kontroler jest punkt wejścia początkowej i jest odpowiedzialny za wybierania który model typy do pracy z i do renderowania widoku (stąd jego nazwa - kontroluje sposób odpowiadania przez aplikację do danego żądania).</span><span class="sxs-lookup"><span data-stu-id="e69af-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="e69af-138">Kontrolerów powinny nie zbyt skomplikowane przez zbyt wiele obowiązki.</span><span class="sxs-lookup"><span data-stu-id="e69af-138">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="e69af-139">Aby zachować logiką kontrolera staje się zbyt skomplikowane, użyj [jednej zasady odpowiedzialności](http://deviq.com/single-responsibility-principle/) do wypychania logiki biznesowej z kontrolerem z do modelu domeny.</span><span class="sxs-lookup"><span data-stu-id="e69af-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="e69af-140">Jeśli okaże się, że Twoje akcji kontrolera często wykonują te same czynności, można wykonać [nie powtarzaj samodzielnie zasady](http://deviq.com/don-t-repeat-yourself/) przez przeniesienie tych typowych akcji do [filtry](#filters).</span><span class="sxs-lookup"><span data-stu-id="e69af-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="e69af-141">Co to jest program ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e69af-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="e69af-142">Platforma ASP.NET Core MVC jest lekki typu open source, wysoce testowalna presentation framework zoptymalizowana dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e69af-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="e69af-143">Podstawowe ASP.NET MVC umożliwia na podstawie wzorców do tworzenia dynamicznych witryn sieci Web umożliwia czyste rozdzielenie problemy.</span><span class="sxs-lookup"><span data-stu-id="e69af-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="e69af-144">Umożliwia pełną kontrolę nad znaczników, obsługuje programowanie TDD przyjaznych i korzysta z najnowszych standardów sieci web.</span><span class="sxs-lookup"><span data-stu-id="e69af-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="e69af-145">Funkcje</span><span class="sxs-lookup"><span data-stu-id="e69af-145">Features</span></span>

<span data-ttu-id="e69af-146">Platformy ASP.NET Core MVC obejmuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="e69af-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="e69af-147">Routing</span><span class="sxs-lookup"><span data-stu-id="e69af-147">Routing</span></span>](#routing)
* [<span data-ttu-id="e69af-148">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="e69af-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="e69af-149">Walidacja modelu</span><span class="sxs-lookup"><span data-stu-id="e69af-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="e69af-150">Iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="e69af-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="e69af-151">Filtry</span><span class="sxs-lookup"><span data-stu-id="e69af-151">Filters</span></span>](#filters)
* [<span data-ttu-id="e69af-152">Obszary</span><span class="sxs-lookup"><span data-stu-id="e69af-152">Areas</span></span>](#areas)
* [<span data-ttu-id="e69af-153">Interfejsy API sieci Web</span><span class="sxs-lookup"><span data-stu-id="e69af-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="e69af-154">Testability</span><span class="sxs-lookup"><span data-stu-id="e69af-154">Testability</span></span>](#testability)
* [<span data-ttu-id="e69af-155">Aparat widoku razor</span><span class="sxs-lookup"><span data-stu-id="e69af-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="e69af-156">Jednoznacznie widoków</span><span class="sxs-lookup"><span data-stu-id="e69af-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="e69af-157">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="e69af-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="e69af-158">Składniki w widoku</span><span class="sxs-lookup"><span data-stu-id="e69af-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="e69af-159">Routing</span><span class="sxs-lookup"><span data-stu-id="e69af-159">Routing</span></span>

<span data-ttu-id="e69af-160">ASP.NET Core MVC jest oparty na [routingu platformy ASP.NET Core](../fundamentals/routing.md), wydajny składnik Mapowanie adresu URL, który pozwala tworzyć aplikacje, które mają zrozumiałe i można wyszukiwać adresów URL.</span><span class="sxs-lookup"><span data-stu-id="e69af-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="e69af-161">Dzięki temu można zdefiniować aplikacji wzorców nazewnictwa adresów URL współpracujących dla optymalizacji dla aparatów wyszukiwania (SEO) oraz do generowania łącza, bez względu na sposób organizowania są pliki na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="e69af-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="e69af-162">Można zdefiniować przy użyciu składni szablonu trasy wygodny obsługującego ograniczenia wartości trasy, wartości domyślne i opcjonalne wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="e69af-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="e69af-163">*Na podstawie Konwencji routingu* umożliwia zdefiniowanie globalny adres URL formaty, w których aplikacja akceptuje i jak każdego z tych formatów mapy do metody akcji określonych w podanego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e69af-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="e69af-164">Po odebraniu żądania przychodzącego aparat routingu analizuje adres URL i dopasowuje je do jednej z określonych formatów adresów URL, a następnie wywołuje metodę akcji skojarzonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e69af-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e69af-165">*Atrybut routingu* można określić informacji o routingu przez dekoracji z kontrolerów i akcji z atrybutami, które definiowania tras dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e69af-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="e69af-166">Oznacza to, że definicji trasy są umieszczone obok kontrolera i akcji, z którym są powiązane.</span><span class="sxs-lookup"><span data-stu-id="e69af-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="e69af-167">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="e69af-167">Model binding</span></span>

<span data-ttu-id="e69af-168">Platformy ASP.NET Core MVC [modelu powiązania](models/model-binding.md) konwertuje danych o żądaniach klientów (wartości formularza, danych trasy, parametrów ciągu zapytania, nagłówki HTTP) na obiekty, które może obsłużyć kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e69af-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="e69af-169">W związku z tym logiki kontrolera nie ma w pracy z ustaleniem, dane żądanie przychodzące; po prostu ma danych jako parametry metody akcji.</span><span class="sxs-lookup"><span data-stu-id="e69af-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="e69af-170">Weryfikacja modelu</span><span class="sxs-lookup"><span data-stu-id="e69af-170">Model validation</span></span>

<span data-ttu-id="e69af-171">Obsługuje platformy ASP.NET Core MVC [weryfikacji](models/validation.md) przez dekoracji obiektu modelu z atrybutów sprawdzania poprawności adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="e69af-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="e69af-172">Atrybuty weryfikacji są sprawdzane po stronie klienta, zanim wartości są przesłane do serwera, jak również na serwerze przed akcji kontrolera jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="e69af-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="e69af-173">Akcja kontrolera:</span><span class="sxs-lookup"><span data-stu-id="e69af-173">A controller action:</span></span>

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

<span data-ttu-id="e69af-174">Platformę obsługi sprawdzania poprawności danych żądania zarówno na kliencie, jak i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e69af-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="e69af-175">Logikę weryfikacji określoną w typach modelu jest dodawany do widoków renderowanych jako adnotacje dyskretnego kodu i jest wymuszana w przeglądarce z [jQuery weryfikacji](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="e69af-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="e69af-176">Iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="e69af-176">Dependency injection</span></span>

<span data-ttu-id="e69af-177">Platformy ASP.NET Core ma wbudowaną obsługę [iniekcji zależności (Podpisane)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e69af-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="e69af-178">W nazwie wzorca MVC ASP.NET Core [kontrolerów](controllers/dependency-injection.md) można żądania potrzebne usług za pośrednictwem ich konstruktorów, umożliwiając wykonaj [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="e69af-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="e69af-179">Aplikację można również użyć [iniekcji zależności w widoku pliki](views/dependency-injection.md)za pomocą `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="e69af-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="e69af-180">Filtry</span><span class="sxs-lookup"><span data-stu-id="e69af-180">Filters</span></span>

<span data-ttu-id="e69af-181">[Filtry](controllers/filters.md) pomóc deweloperom Hermetyzowanie kompleksowymi problemy, takie jak obsługa wyjątków lub autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e69af-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="e69af-182">Filtry włączyć uruchamianie niestandardowej logiki przed i przetwarzania końcowego dla metody akcji i może być skonfigurowana do uruchamiania w niektórych punktach w potoku wykonywania dla danego żądania.</span><span class="sxs-lookup"><span data-stu-id="e69af-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="e69af-183">Filtry można zastosować do kontrolerów i akcji jako atrybuty (lub mogą być uruchamiane globalny).</span><span class="sxs-lookup"><span data-stu-id="e69af-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="e69af-184">Kilka filtrów (takie jak `Authorize`) znajdują się w ramach.</span><span class="sxs-lookup"><span data-stu-id="e69af-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="e69af-185">Obszary</span><span class="sxs-lookup"><span data-stu-id="e69af-185">Areas</span></span>

<span data-ttu-id="e69af-186">[Obszary](controllers/areas.md) umożliwiają partycji dużych aplikacji sieci Web platformy ASP.NET Core MVC w mniejszych funkcjonalności grupowania.</span><span class="sxs-lookup"><span data-stu-id="e69af-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="e69af-187">Obszar to struktura MVC wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e69af-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="e69af-188">W projekcie MVC składników logicznych, takich jak Model, kontrolera i widoku są przechowywane w różnych folderach i MVC używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="e69af-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="e69af-189">W przypadku dużych aplikacji może być korzystne partycji aplikacji na oddzielnych wysokiej obszary poziomu funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="e69af-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="e69af-190">Na przykład aplikacji handlu elektronicznego z wiele jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania itd. Każdy z tych jednostek ma własne logiczny składnik widoki, kontrolery i modeli.</span><span class="sxs-lookup"><span data-stu-id="e69af-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="e69af-191">Interfejsy Web API</span><span class="sxs-lookup"><span data-stu-id="e69af-191">Web APIs</span></span>

<span data-ttu-id="e69af-192">Oprócz stanowi doskonałe platformę do tworzenia witryn sieci web, ASP.NET Core MVC ma dużą obsługę tworzenia interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e69af-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="e69af-193">Można utworzyć usługi, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne.</span><span class="sxs-lookup"><span data-stu-id="e69af-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="e69af-194">Platformę obsługuje negocjowanie zawartości HTTP z wbudowaną obsługę [formatowania danych](models/formatting.md) jako JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="e69af-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="e69af-195">Zapis [niestandardowe elementy formatujące](advanced/custom-formatters.md) Aby dodać obsługę własnych formatów.</span><span class="sxs-lookup"><span data-stu-id="e69af-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="e69af-196">Użyj generowania łącze, aby włączyć obsługę hipermedialnych.</span><span class="sxs-lookup"><span data-stu-id="e69af-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="e69af-197">Łatwo włączyć obsługę [współużytkowanie zasobów między źródłami (CORS) do udostępniania](http://www.w3.org/TR/cors/) tak, aby interfejsów API sieci Web może być współużytkowana przez wiele aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e69af-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="e69af-198">Pola</span><span class="sxs-lookup"><span data-stu-id="e69af-198">Testability</span></span>

<span data-ttu-id="e69af-199">W ramach korzystanie z interfejsów i iniekcji zależności był dobrze nadaje się do przeprowadzania testów jednostkowych i ramach zawiera funkcje (na przykład TestHost i InMemory dostawcy programu Entity Framework) [testów integracji](../testing/integration-testing.md) szybki i łatwe również.</span><span class="sxs-lookup"><span data-stu-id="e69af-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="e69af-200">Dowiedz się więcej o [testowania logiką kontrolera](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="e69af-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="e69af-201">Aparat widoku razor</span><span class="sxs-lookup"><span data-stu-id="e69af-201">Razor view engine</span></span>

<span data-ttu-id="e69af-202">[ASP.NET Core MVC widoków](views/overview.md) użyj [aparatu widoku Razor](views/razor.md) do renderowania widoków.</span><span class="sxs-lookup"><span data-stu-id="e69af-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="e69af-203">Razor to język compact, obszerne i płynne szablonu do definiowania widoków przy użyciu osadzonego kodu C#.</span><span class="sxs-lookup"><span data-stu-id="e69af-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="e69af-204">Razor jest używana do dynamicznego generowania zawartości sieci web na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e69af-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="e69af-205">Kod serwera prawidłowo można łączyć z zawartością po stronie klienta i kod.</span><span class="sxs-lookup"><span data-stu-id="e69af-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="e69af-206">Przy użyciu aparatu widoku Razor można zdefiniować [układów](views/layout.md), [widoki częściowe](views/partial.md) i wymiennych sekcji.</span><span class="sxs-lookup"><span data-stu-id="e69af-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="e69af-207">Jednoznacznie widoków</span><span class="sxs-lookup"><span data-stu-id="e69af-207">Strongly typed views</span></span>

<span data-ttu-id="e69af-208">Widoków MVC razor można zdecydowanie wpisywać oparte na modelu.</span><span class="sxs-lookup"><span data-stu-id="e69af-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="e69af-209">Kontrolery można przekazać jednoznacznie modelu z widokami włączanie widoków Sprawdzanie typu i obsługę funkcji IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e69af-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="e69af-210">Na przykład następujący widok renderuje modelu typu `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="e69af-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="e69af-211">Pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="e69af-211">Tag Helpers</span></span>

<span data-ttu-id="e69af-212">[Pomocników tagów](views/tag-helpers/intro.md) Włącz kod po stronie serwera do tworzenia i renderowania elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="e69af-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="e69af-213">Pomocników tagów umożliwia definiowanie niestandardowych tagów (na przykład `<environment>`) lub aby zmodyfikować zachowanie znaczników (na przykład `<label>`).</span><span class="sxs-lookup"><span data-stu-id="e69af-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="e69af-214">Pomocników tagów powiązać z określonych elementów na podstawie nazwy elementu i jego atrybuty.</span><span class="sxs-lookup"><span data-stu-id="e69af-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="e69af-215">Zapewniają korzyści wynikające z renderowania po stronie serwera, zachowując nadal edytowania kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="e69af-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="e69af-216">Istnieje wiele wbudowanych pomocników tagów do wykonywania typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakietów więcej — i jeszcze bardziej dostępne w publicznych repozytoriach usługi GitHub i NuGet.</span><span class="sxs-lookup"><span data-stu-id="e69af-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="e69af-217">Pomocników tagów są tworzone w języku C# i ich elementami docelowymi na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="e69af-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="e69af-218">Na przykład wbudowana LinkTagHelper można utworzyć łącza do `Login` akcji `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="e69af-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="e69af-219">`EnvironmentTagHelper` Można uwzględnić skrypty różnych w widoków (na przykład raw lub zminimalizowany) oparte na środowisko uruchomieniowe, takich jak projektowanie, tymczasowym czy produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="e69af-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="e69af-220">Pomocników tagów zapewnić środowisko programistyczne przyjaznego HTML i bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor.</span><span class="sxs-lookup"><span data-stu-id="e69af-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="e69af-221">Większość wbudowanych pomocników tagów target istniejących elementów HTML i udostępniania atrybutów po stronie serwera dla elementu.</span><span class="sxs-lookup"><span data-stu-id="e69af-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="e69af-222">Składniki w widoku</span><span class="sxs-lookup"><span data-stu-id="e69af-222">View Components</span></span>

<span data-ttu-id="e69af-223">[Wyświetlanie składników](views/view-components.md) umożliwiają pakietu logiki renderowania i użyć go ponownie w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e69af-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="e69af-224">Są one podobne do [widoki częściowe](views/partial.md), ale z skojarzonej logiki.</span><span class="sxs-lookup"><span data-stu-id="e69af-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
