---
title: Obsługa błędów w ASP.NET Core interfejsów API sieci Web
author: pranavkm
description: Dowiedz się więcej o obsłudze błędów przy użyciu ASP.NET Core interfejsów API sieci Web.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/18/2019
uid: web-api/handle-errors
ms.openlocfilehash: bf8229d37ef15c42b5d7bb4a12737ac65d7b753c
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150909"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="cde1f-103">Obsługa błędów w ASP.NET Core interfejsów API sieci Web</span><span class="sxs-lookup"><span data-stu-id="cde1f-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="cde1f-104">W tym artykule opisano sposób obsługi i dostosowywania obsługi błędów przy użyciu ASP.NET Core interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cde1f-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="cde1f-105">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="cde1f-105">Developer Exception Page</span></span>

<span data-ttu-id="cde1f-106">[Strona wyjątków dla deweloperów](xref:fundamentals/error-handling) to przydatne narzędzie do uzyskiwania szczegółowych śladów stosu dla błędów serwera.</span><span class="sxs-lookup"><span data-stu-id="cde1f-106">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cde1f-107">Na stronie wyjątku dewelopera jest wyświetlana odpowiedź w postaci zwykłego tekstu, jeśli klient nie akceptuje danych wyjściowych w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="cde1f-107">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="cde1f-108">Stronę wyjątku dla deweloperów należy włączyć tylko wtedy, **gdy aplikacja jest uruchomiona w środowisku deweloperskim**.</span><span class="sxs-lookup"><span data-stu-id="cde1f-108">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="cde1f-109">Nie chcesz udostępniać szczegółowych informacji o wyjątku publicznie, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="cde1f-109">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="cde1f-110">Aby uzyskać więcej informacji na temat konfigurowania środowisk <xref:fundamentals/environments>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="cde1f-110">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="cde1f-111">Procedura obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="cde1f-111">Exception handler</span></span>

<span data-ttu-id="cde1f-112">W środowiskach innych niż programowanie [wyjątek obsługujący oprogramowanie pośredniczące](xref:fundamentals/error-handling) może być używany do tworzenia ładunku błędu:</span><span class="sxs-lookup"><span data-stu-id="cde1f-112">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="cde1f-113">W `Startup.Configure`programie Wywołaj <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> , aby użyć oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="cde1f-113">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

1. <span data-ttu-id="cde1f-114">Skonfiguruj akcję kontrolera, aby odpowiedzieć na `/error` trasę:</span><span class="sxs-lookup"><span data-stu-id="cde1f-114">Configure a controller action to respond to the `/error` route:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

<span data-ttu-id="cde1f-115">Poprzednia `Error` akcja powoduje wysłanie do klienta ładunku zgodnego z [RFC7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="cde1f-115">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="cde1f-116">Wyjątek obsługujący oprogramowanie pośredniczące może również dostarczyć bardziej szczegółowe dane wyjściowe negocjowane z zawartością w lokalnym środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="cde1f-116">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="cde1f-117">Wykonaj następujące kroki, aby utworzyć spójny format ładunku w środowisku deweloperskim i produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="cde1f-117">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="cde1f-118">W `Startup.Configure`programie Zarejestruj wyjątek specyficzny dla środowiska obsługi oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="cde1f-118">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    <span data-ttu-id="cde1f-119">W poprzednim kodzie oprogramowanie pośredniczące jest zarejestrowane w:</span><span class="sxs-lookup"><span data-stu-id="cde1f-119">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="cde1f-120">Trasa `/error-local-development` w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="cde1f-120">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="cde1f-121">Trasa `/error` w środowiskach, które nie są przeznaczone do programowania.</span><span class="sxs-lookup"><span data-stu-id="cde1f-121">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="cde1f-122">Zastosuj Routing atrybutów do akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cde1f-122">Apply attribute routing to controller actions:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error-local-development")]
        public IActionResult ErrorLocalDevelopment(
            [FromServices] IWebHostEnvironment webHostEnvironment)
        {
            if (!webHostEnvironment.IsDevelopment())
            {
                throw new InvalidOperationException(
                    "This shouldn't be invoked in non-development environments.");
            }
    
            var context = HttpContext.Features.Get<IExceptionHandlerFeature>();

            return Problem(
                detail: context.Error.StackTrace,
                title: context.Error.Message);
        }
    
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="cde1f-123">Modyfikowanie odpowiedzi przy użyciu wyjątków</span><span class="sxs-lookup"><span data-stu-id="cde1f-123">Use exceptions to modify the response</span></span>

<span data-ttu-id="cde1f-124">Zawartość odpowiedzi można modyfikować poza kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="cde1f-124">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="cde1f-125">W przypadku interfejsu API sieci Web ASP.NET 4. x jeden ze sposobów na to zrobić <xref:System.Web.Http.HttpResponseException> przy użyciu typu.</span><span class="sxs-lookup"><span data-stu-id="cde1f-125">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="cde1f-126">ASP.NET Core nie zawiera równoważnego typu.</span><span class="sxs-lookup"><span data-stu-id="cde1f-126">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="cde1f-127">Pomoc techniczną dla programu `HttpResponseException` można dodać, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="cde1f-127">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="cde1f-128">Utwórz dobrze znany typ wyjątku o nazwie `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="cde1f-128">Create a well-known exception type named `HttpResponseException`:</span></span>

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. <span data-ttu-id="cde1f-129">Utwórz filtr akcji o nazwie `HttpResponseExceptionFilter`:</span><span class="sxs-lookup"><span data-stu-id="cde1f-129">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    ```csharp
    public class HttpResponseExceptionFilter : IActionFilter, IOrderedFilter
    {
        public int Order { get; set; } = int.MaxValue - 10;
    
        public void OnActionExecuting(ActionExecutingContext context) {}
    
        public void OnActionExecuted(ActionExecutedContext context)
        {
            if (context.Exception is HttpResponseException exception)
            {
                context.Result = new ObjectResult(exception.Value)
                {
                    Status = exception.Status,
                };
                context.ExceptionHandled = true;
            }
        }
    }
    ```

1. <span data-ttu-id="cde1f-130">W `Startup.ConfigureServices`programie Dodaj filtr akcji do kolekcji filters:</span><span class="sxs-lookup"><span data-stu-id="cde1f-130">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a><span data-ttu-id="cde1f-131">Odpowiedź na błąd niepowodzenia weryfikacji</span><span class="sxs-lookup"><span data-stu-id="cde1f-131">Validation failure error response</span></span>

<span data-ttu-id="cde1f-132">W przypadku kontrolerów interfejsu API sieci Web MVC reaguje <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> na typ odpowiedzi, gdy Walidacja modelu kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="cde1f-132">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="cde1f-133">MVC używa wyników <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> w celu skonstruowania odpowiedzi na błąd w przypadku niepowodzenia walidacji.</span><span class="sxs-lookup"><span data-stu-id="cde1f-133">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="cde1f-134">Poniższy przykład używa fabryki do zmiany domyślnego typu odpowiedzi na <xref:Microsoft.AspNetCore.Mvc.SerializableError> wartość w: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="cde1f-134">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

services.Configure<ApiBehaviorOptions>(options =>
{
    options.InvalidModelStateResponseFactory = context =>
    {
        var result = new BadRequestObjectResult(context.ModelState);

        // TODO: add `using using System.Net.Mime;` to resolve MediaTypeNames
        result.ContentTypes.Add(MediaTypeNames.Application.Json);
        result.ContentTypes.Add(MediaTypeNames.Application.Xml);

        return result;
    };
});
```

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="cde1f-135">Odpowiedź na błąd klienta</span><span class="sxs-lookup"><span data-stu-id="cde1f-135">Client error response</span></span>

<span data-ttu-id="cde1f-136">*Wynik błędu* jest definiowany w wyniku przy użyciu kodu stanu HTTP 400 lub wyższego.</span><span class="sxs-lookup"><span data-stu-id="cde1f-136">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="cde1f-137">W przypadku kontrolerów interfejsu API sieci Web MVC przekształca wynik błędu z wynikiem <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="cde1f-137">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cde1f-138">Odpowiedź na błąd można skonfigurować w jeden z następujących sposobów:</span><span class="sxs-lookup"><span data-stu-id="cde1f-138">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="cde1f-139">Implementuj ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="cde1f-139">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="cde1f-140">Użyj ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="cde1f-140">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="cde1f-141">Implementuj ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="cde1f-141">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="cde1f-142">MVC używa `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` do tworzenia wszystkich <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> wystąpień i <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="cde1f-142">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="cde1f-143">Obejmuje to odpowiedzi na błędy klientów, odpowiedzi na błędy walidacji i `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="cde1f-143">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="cde1f-144">Aby dostosować odpowiedź dotyczącą szczegółów problemu, zarejestruj niestandardową `ProblemDetailsFactory` implementację programu w programie `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cde1f-144">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="cde1f-145">Odpowiedź na błąd można skonfigurować zgodnie z opisem w sekcji [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .</span><span class="sxs-lookup"><span data-stu-id="cde1f-145">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="cde1f-146">Użyj ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="cde1f-146">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="cde1f-147">Użyj właściwości <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> , aby skonfigurować zawartość `ProblemDetails` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="cde1f-147">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="cde1f-148">Na przykład poniższy kod aktualizuje `type` właściwość dla 404 odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="cde1f-148">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
