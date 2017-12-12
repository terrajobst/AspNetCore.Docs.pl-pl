---
title: Niestandardowe autoryzacji opartych na zasadach
author: rick-anderson
description: "Tym dokumencie opisano sposób tworzenia i używania programów obsługi zasad autoryzacji niestandardowej w aplikacji platformy ASP.NET Core."
keywords: Platformy ASP.NET Core autoryzacji, niestandardowych zasad, zasady autoryzacji
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="39619-104">Niestandardowe autoryzacji opartych na zasadach</span><span class="sxs-lookup"><span data-stu-id="39619-104">Custom policy-based authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="39619-105">Poniżej obejmuje [autoryzacji ról](roles.md) i [oświadczeń autoryzacji](claims.md) wprowadzić korzystanie z wymaganiem, obsługa wymagania i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="39619-105">Underneath the covers, the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement, and a pre-configured policy.</span></span> <span data-ttu-id="39619-106">Te bloki konstrukcyjne umożliwiają express ocen autoryzacji w kodzie, co pozwala na pełniejsze, wielokrotnego użytku i struktury łatwością testować autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="39619-106">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="39619-107">Zasady autoryzacji jest składają się z co najmniej jednego wymagania i zarejestrowane podczas uruchamiania aplikacji w ramach konfiguracji usługi autoryzacji, w `ConfigureServices` w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="39619-107">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

<span data-ttu-id="39619-108">W tym miejscu można zauważyć, że z jednym wymaganie, że o minimalnym wieku, która jest przekazywana jako parametr wymogiem tworzone są zasady "Over21".</span><span class="sxs-lookup"><span data-stu-id="39619-108">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="39619-109">Zasady są stosowane przy użyciu `Authorize` atrybutu, określając nazwę zasady, na przykład;</span><span class="sxs-lookup"><span data-stu-id="39619-109">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a><span data-ttu-id="39619-110">Wymagania</span><span class="sxs-lookup"><span data-stu-id="39619-110">Requirements</span></span>

<span data-ttu-id="39619-111">Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="39619-111">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="39619-112">W zasadach minimalnego wieku wymaganie, który mamy jest jeden parametr, minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="39619-112">In our Minimum Age policy, the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="39619-113">Wymaganie musi implementować `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="39619-113">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="39619-114">To jest pusta, interfejsem znacznika.</span><span class="sxs-lookup"><span data-stu-id="39619-114">This is an empty, marker interface.</span></span> <span data-ttu-id="39619-115">Sparametryzowane minimalny wymagany wiek mogą zostać zaimplementowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="39619-115">A parameterized minimum age requirement might be implemented as follows;</span></span>

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

<span data-ttu-id="39619-116">Wymagania nie musi być danych lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="39619-116">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="39619-117">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="39619-117">Authorization handlers</span></span>

<span data-ttu-id="39619-118">Program obsługi autoryzacji jest odpowiedzialny za oceny wszystkie właściwości wymagane.</span><span class="sxs-lookup"><span data-stu-id="39619-118">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="39619-119">Program obsługi autoryzacji musi ich oceny na podstawie podanych `AuthorizationHandlerContext` zdecydować, czy jest dozwolone autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="39619-119">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="39619-120">Wymóg może mieć [wielu obsług](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="39619-120">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="39619-121">Programy obsługi musi dziedziczyć `AuthorizationHandler<T>` gdzie T jest konieczność obsługi.</span><span class="sxs-lookup"><span data-stu-id="39619-121">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="39619-122">Program obsługi minimalnego wieku może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="39619-122">The minimum age handler might look like this:</span></span>

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="39619-123">W powyższym kodzie najpierw szukamy aby zobaczyć, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez wystawcę wiemy i zaufania.</span><span class="sxs-lookup"><span data-stu-id="39619-123">In the code above, we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="39619-124">Jeśli brakuje oświadczenie tak, aby firma Microsoft nie można autoryzować, dlatego zostanie zwrócona.</span><span class="sxs-lookup"><span data-stu-id="39619-124">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="39619-125">Jeśli mamy oświadczenia, możemy ustalić przyczynę wiek użytkownika i, jeżeli spełniają minimalnego wieku przekazany przez wymaganie następnie autoryzacji przebiegło pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="39619-125">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="39619-126">Po pomyślnej autoryzacji nazywamy `context.Succeed()` przekazywanie wymaganie, która została pomyślnie jako parametr.</span><span class="sxs-lookup"><span data-stu-id="39619-126">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="39619-127">Rejestracja programu obsługi</span><span class="sxs-lookup"><span data-stu-id="39619-127">Handler registration</span></span>
<span data-ttu-id="39619-128">Programy obsługi musi być zarejestrowany w kolekcji usługi podczas konfiguracji, na przykład;</span><span class="sxs-lookup"><span data-stu-id="39619-128">Handlers must be registered in the services collection during configuration, for example;</span></span>

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

<span data-ttu-id="39619-129">Każdy program obsługi jest dodawany do kolekcji usług przy użyciu `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` przekazywanie w klasie obsługi.</span><span class="sxs-lookup"><span data-stu-id="39619-129">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="39619-130">Co powinna zwracać obsługi?</span><span class="sxs-lookup"><span data-stu-id="39619-130">What should a handler return?</span></span>

<span data-ttu-id="39619-131">Można zobaczyć w naszym [przykład obsługi](policies.md#security-authorization-handler-example) który `Handle()` — metoda nie ma zwracanych wartości, w jaki sposób możemy oznaczają powodzenie lub niepowodzenie?</span><span class="sxs-lookup"><span data-stu-id="39619-131">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="39619-132">Program obsługi oznacza Powodzenie przez wywołanie metody `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, która została pomyślnie zweryfikowana.</span><span class="sxs-lookup"><span data-stu-id="39619-132">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="39619-133">Program obsługi nie musi do obsługi błędów ogólnie rzecz biorąc, zgodnie z innych programów obsługi na ten sam wymóg może się powieść.</span><span class="sxs-lookup"><span data-stu-id="39619-133">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="39619-134">Aby zagwarantować awarii, nawet w przypadku pomyślnego innych programów obsługi dla wymagania, należy wywołać `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="39619-134">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="39619-135">Niezależnie od wywołana wewnątrz obsługi sieci obsługi wszystkie wymagane będzie wywoływany, gdy zasady wymaga to wymaganie.</span><span class="sxs-lookup"><span data-stu-id="39619-135">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="39619-136">Dzięki temu wymagania dotyczące efekty uboczne, takich jak rejestrowanie, które będą zawsze miały miejsce nawet wtedy, gdy `context.Fail()` została wywołana w innego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="39619-136">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="39619-137">Dlaczego może być wielu obsług dla wymagania?</span><span class="sxs-lookup"><span data-stu-id="39619-137">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="39619-138">W przypadkach, w którym ma oceny na **lub** podstawy wdrożenia wielu obsług dla pojedynczego wymagania.</span><span class="sxs-lookup"><span data-stu-id="39619-138">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="39619-139">Na przykład firma Microsoft ma drzwi, które mogły być otwierane tylko w przypadku kart klucza.</span><span class="sxs-lookup"><span data-stu-id="39619-139">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="39619-140">Jeśli karta klucza pozostanie w domu recepcjonista drukuje tymczasowego naklejce i otwiera drzwi.</span><span class="sxs-lookup"><span data-stu-id="39619-140">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="39619-141">W tym scenariuszu należy wymaganiem pojedynczego *EnterBuilding*, ale wielu obsług, każda z nich badanie pojedynczego wymaganie.</span><span class="sxs-lookup"><span data-stu-id="39619-141">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="39619-142">Teraz, zakładając, że oba programy obsługi są [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) po ocenia zasady `EnterBuildingRequirement` Jeśli albo obsługi zakończy się powodzeniem oceny zasad powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="39619-142">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="39619-143">Przy użyciu func do zrealizowania zasady</span><span class="sxs-lookup"><span data-stu-id="39619-143">Using a func to fulfill a policy</span></span>

<span data-ttu-id="39619-144">Może być zmieniana spełniających zasady w przypadku prostego można wyrazić w kodzie.</span><span class="sxs-lookup"><span data-stu-id="39619-144">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="39619-145">Można podać po prostu `Func<AuthorizationHandlerContext, bool>` przy konfigurowaniu zasad z `RequireAssertion` konstruktora zasad.</span><span class="sxs-lookup"><span data-stu-id="39619-145">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="39619-146">Na przykład poprzedniej `BadgeEntryHandler` można ponownie zapisać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="39619-146">For example the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="39619-147">Uzyskiwanie dostępu do kontekstu żądania MVC w obsłudze</span><span class="sxs-lookup"><span data-stu-id="39619-147">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="39619-148">`Handle` Metoda musi implementować obsługi autoryzacji ma dwa parametry `AuthorizationContext` i `Requirement` są obsługi.</span><span class="sxs-lookup"><span data-stu-id="39619-148">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="39619-149">Struktury, np. MVC lub Jabbr są także dodać dowolny obiekt, aby `Resource` właściwość `AuthorizationContext` do przekazywania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="39619-149">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="39619-150">Na przykład MVC przekazuje wystąpienie `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` we właściwości zasobu, którego uzyskano dostęp element HttpContext RouteData i wszystko, co zapewnia else MVC.</span><span class="sxs-lookup"><span data-stu-id="39619-150">For example, MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="39619-151">Korzystanie z `Resource` właściwość jest tylko dla struktury.</span><span class="sxs-lookup"><span data-stu-id="39619-151">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="39619-152">Korzystając z informacji w `Resource` właściwości ograniczy zasad autoryzacji do określonej struktury.</span><span class="sxs-lookup"><span data-stu-id="39619-152">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="39619-153">Należy rzutować `Resource` za pomocą właściwości `as` — słowo kluczowe i sprawdź rzutowanie ma się powieść, aby upewnić się, kod nie awarii z `InvalidCastExceptions` uruchomienia na innych platformach;</span><span class="sxs-lookup"><span data-stu-id="39619-153">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
