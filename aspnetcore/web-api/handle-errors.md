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
# <a name="handle-errors-in-aspnet-core-web-apis"></a>Obsługa błędów w ASP.NET Core interfejsów API sieci Web

W tym artykule opisano sposób obsługi i dostosowywania obsługi błędów przy użyciu ASP.NET Core interfejsów API sieci Web.

## <a name="developer-exception-page"></a>Strona wyjątków dla deweloperów

[Strona wyjątków dla deweloperów](xref:fundamentals/error-handling) to przydatne narzędzie do uzyskiwania szczegółowych śladów stosu dla błędów serwera.

::: moniker range=">= aspnetcore-3.0"

Na stronie wyjątku dewelopera jest wyświetlana odpowiedź w postaci zwykłego tekstu, jeśli klient nie akceptuje danych wyjściowych w formacie HTML:

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
> Stronę wyjątku dla deweloperów należy włączyć tylko wtedy, **gdy aplikacja jest uruchomiona w środowisku deweloperskim**. Nie chcesz udostępniać szczegółowych informacji o wyjątku publicznie, gdy aplikacja jest uruchamiana w środowisku produkcyjnym. Aby uzyskać więcej informacji na temat konfigurowania środowisk <xref:fundamentals/environments>, zobacz.

## <a name="exception-handler"></a>Procedura obsługi wyjątków

W środowiskach innych niż programowanie [wyjątek obsługujący oprogramowanie pośredniczące](xref:fundamentals/error-handling) może być używany do tworzenia ładunku błędu:

1. W `Startup.Configure`programie Wywołaj <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> , aby użyć oprogramowania pośredniczącego:

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

1. Skonfiguruj akcję kontrolera, aby odpowiedzieć na `/error` trasę:

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

Poprzednia `Error` akcja powoduje wysłanie do klienta ładunku zgodnego z [RFC7807](https://tools.ietf.org/html/rfc7807).

Wyjątek obsługujący oprogramowanie pośredniczące może również dostarczyć bardziej szczegółowe dane wyjściowe negocjowane z zawartością w lokalnym środowisku programistycznym. Wykonaj następujące kroki, aby utworzyć spójny format ładunku w środowisku deweloperskim i produkcyjnym:

1. W `Startup.Configure`programie Zarejestruj wyjątek specyficzny dla środowiska obsługi oprogramowania pośredniczącego:

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

    W poprzednim kodzie oprogramowanie pośredniczące jest zarejestrowane w:

    * Trasa `/error-local-development` w środowisku deweloperskim.
    * Trasa `/error` w środowiskach, które nie są przeznaczone do programowania.
    
1. Zastosuj Routing atrybutów do akcji kontrolera:

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

## <a name="use-exceptions-to-modify-the-response"></a>Modyfikowanie odpowiedzi przy użyciu wyjątków

Zawartość odpowiedzi można modyfikować poza kontrolerem. W przypadku interfejsu API sieci Web ASP.NET 4. x jeden ze sposobów na to zrobić <xref:System.Web.Http.HttpResponseException> przy użyciu typu. ASP.NET Core nie zawiera równoważnego typu. Pomoc techniczną dla programu `HttpResponseException` można dodać, wykonując następujące czynności:

1. Utwórz dobrze znany typ wyjątku o nazwie `HttpResponseException`:

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. Utwórz filtr akcji o nazwie `HttpResponseExceptionFilter`:

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

1. W `Startup.ConfigureServices`programie Dodaj filtr akcji do kolekcji filters:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a>Odpowiedź na błąd niepowodzenia weryfikacji

W przypadku kontrolerów interfejsu API sieci Web MVC reaguje <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> na typ odpowiedzi, gdy Walidacja modelu kończy się niepowodzeniem. MVC używa wyników <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> w celu skonstruowania odpowiedzi na błąd w przypadku niepowodzenia walidacji. Poniższy przykład używa fabryki do zmiany domyślnego typu odpowiedzi na <xref:Microsoft.AspNetCore.Mvc.SerializableError> wartość w: `Startup.ConfigureServices`

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

## <a name="client-error-response"></a>Odpowiedź na błąd klienta

*Wynik błędu* jest definiowany w wyniku przy użyciu kodu stanu HTTP 400 lub wyższego. W przypadku kontrolerów interfejsu API sieci Web MVC przekształca wynik błędu z wynikiem <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.

::: moniker range=">= aspnetcore-3.0"

Odpowiedź na błąd można skonfigurować w jeden z następujących sposobów:

1. [Implementuj ProblemDetailsFactory](#implement-problemdetailsfactory)
1. [Użyj ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a>Implementuj ProblemDetailsFactory

MVC używa `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` do tworzenia wszystkich <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> wystąpień i <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Obejmuje to odpowiedzi na błędy klientów, odpowiedzi na błędy walidacji i `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> metody pomocnika.

Aby dostosować odpowiedź dotyczącą szczegółów problemu, zarejestruj niestandardową `ProblemDetailsFactory` implementację programu w programie `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Odpowiedź na błąd można skonfigurować zgodnie z opisem w sekcji [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a>Użyj ApiBehaviorOptions. ClientErrorMapping

Użyj właściwości <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> , aby skonfigurować zawartość `ProblemDetails` odpowiedzi. Na przykład poniższy kod aktualizuje `type` właściwość dla 404 odpowiedzi:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
