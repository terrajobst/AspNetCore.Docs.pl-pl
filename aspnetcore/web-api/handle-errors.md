---
title: Obsługa błędów w ASP.NET Core interfejsów API sieci Web
author: pranavkm
description: Dowiedz się więcej o obsłudze błędów przy użyciu ASP.NET Core interfejsów API sieci Web.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 12/10/2019
uid: web-api/handle-errors
ms.openlocfilehash: e445fb3d50973643c9cea60395d1ed02c2f5f675
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660889"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a>Obsługa błędów w ASP.NET Core interfejsów API sieci Web

W tym artykule opisano sposób obsługi i dostosowywania obsługi błędów przy użyciu ASP.NET Core interfejsów API sieci Web.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Strona wyjątków dla deweloperów

[Strona wyjątków dla deweloperów](xref:fundamentals/error-handling) to przydatne narzędzie do uzyskiwania szczegółowych śladów stosu dla błędów serwera. Używa <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> do przechwytywania wyjątków synchronicznych i asynchronicznych z potoku HTTP oraz do generowania odpowiedzi na błędy. Aby zilustrować, należy wziąć pod uwagę następujące działania kontrolera:

[!code-csharp[](handle-errors/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_GetByCity)]

Uruchom następujące polecenie `curl`, aby przetestować poprzednią akcję:

```bash
curl -i https://localhost:5001/weatherforecast/chicago
```

::: moniker range=">= aspnetcore-3.0"

W ASP.NET Core 3,0 i nowszych na stronie wyjątków dla deweloperów zostanie wyświetlona odpowiedź w postaci zwykłego tekstu, jeśli klient nie zażąda danych wyjściowych w formacie HTML. Wyświetlane są następujące dane wyjściowe:

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/plain
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:13:16 GMT

System.ArgumentException: We don't offer a weather forecast for chicago. (Parameter 'city')
   at WebApiSample.Controllers.WeatherForecastController.Get(String city) in C:\working_folder\aspnet\AspNetCore.Docs\aspnetcore\web-api\handle-errors\samples\3.x\Controllers\WeatherForecastController.cs:line 34
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.SyncObjectResultExecutor.Execute(IActionResultTypeMapper mapper, ObjectMethodExecutor executor, Object controller, Object[] arguments)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeActionMethodAsync>g__Logged|12_1(ControllerActionInvoker invoker)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeNextActionFilterAsync>g__Awaited|10_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Rethrow(ActionExecutedContextSealed context)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.InvokeInnerFilterAsync()
--- End of stack trace from previous location where exception was thrown ---
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeFilterPipelineAsync>g__Awaited|19_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>g__Logged|17_1(ResourceInvoker invoker)
   at Microsoft.AspNetCore.Routing.EndpointMiddleware.<Invoke>g__AwaitRequestTask|6_0(Endpoint endpoint, Task requestTask, ILogger logger)
   at Microsoft.AspNetCore.Authorization.AuthorizationMiddleware.Invoke(HttpContext context)
   at Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware.Invoke(HttpContext context)

HEADERS
=======
Accept: */*
Host: localhost:44312
User-Agent: curl/7.55.1
```

Aby zamiast tego wyświetlić odpowiedź sformatowaną w formacie HTML, należy ustawić `Accept` nagłówek żądania HTTP na typ nośnika `text/html`. Na przykład:

```bash
curl -i -H "Accept: text/html" https://localhost:5001/weatherforecast/chicago
```

Rozważmy następujący fragment z odpowiedzi HTTP:

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

W ASP.NET Core 2,2 i wcześniejszych na stronie wyjątku dla deweloperów zostanie wyświetlona odpowiedź w formacie HTML. Rozważmy na przykład następujący fragment z odpowiedzi HTTP:

::: moniker-end

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/html; charset=utf-8
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:55:37 GMT

<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta charset="utf-8" />
        <title>Internal Server Error</title>
        <style>
            body {
    font-family: 'Segoe UI', Tahoma, Arial, Helvetica, sans-serif;
    font-size: .813em;
    color: #222;
    background-color: #fff;
}
```

::: moniker range=">= aspnetcore-3.0"

Odpowiedź w formacie HTML jest przydatna podczas testowania za pomocą narzędzi, takich jak Poster. Na poniższym zrzucie ekranu są widoczne odpowiedzi zarówno w formacie zwykłego tekstu, jak i w formacie HTML.

![Testowanie stron wyjątków dla deweloperów w programie Poster](handle-errors/_static/developer-exception-page-postman.gif)

::: moniker-end

> [!WARNING]
> Stronę wyjątku dla deweloperów należy włączyć tylko wtedy, **gdy aplikacja jest uruchomiona w środowisku deweloperskim**. Nie chcesz udostępniać szczegółowych informacji o wyjątku publicznie, gdy aplikacja jest uruchamiana w środowisku produkcyjnym. Aby uzyskać więcej informacji na temat konfigurowania środowisk, zobacz <xref:fundamentals/environments>.

## <a name="exception-handler"></a>Procedura obsługi wyjątków

W środowiskach innych niż programowanie [wyjątek obsługujący oprogramowanie pośredniczące](xref:fundamentals/error-handling) może być używany do tworzenia ładunku błędu:

1. W `Startup.Configure`Wywołaj <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A>, aby użyć oprogramowania pośredniczącego:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. Skonfiguruj akcję kontrolera, aby odpowiedzieć na trasę `/error`:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

Poprzednia akcja `Error` wysyła do klienta ładunek zgodny ze standardem [RFC 7807](https://tools.ietf.org/html/rfc7807).

Wyjątek obsługujący oprogramowanie pośredniczące może również dostarczyć bardziej szczegółowe dane wyjściowe negocjowane z zawartością w lokalnym środowisku programistycznym. Wykonaj następujące kroki, aby utworzyć spójny format ładunku w środowisku deweloperskim i produkcyjnym:

1. W `Startup.Configure`należy zarejestrować wyjątek specyficzny dla środowiska obsługi oprogramowania pośredniczącego:

    ::: moniker range=">= aspnetcore-3.0"

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

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
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

    ::: moniker-end

    W poprzednim kodzie oprogramowanie pośredniczące jest zarejestrowane w:

    * Trasa `/error-local-development` w środowisku deweloperskim.
    * Trasa `/error` w środowiskach, które nie są programowaniem.
    
1. Zastosuj Routing atrybutów do akcji kontrolera:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a>Modyfikowanie odpowiedzi przy użyciu wyjątków

Zawartość odpowiedzi można modyfikować poza kontrolerem. W interfejsie API sieci Web ASP.NET 4. x jeden ze sposobów na to zrobić przy użyciu typu <xref:System.Web.Http.HttpResponseException>. ASP.NET Core nie zawiera równoważnego typu. Pomoc techniczną dla `HttpResponseException` można dodać, wykonując następujące czynności:

1. Utwórz dobrze znany typ wyjątku o nazwie `HttpResponseException`:

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. Utwórz filtr akcji o nazwie `HttpResponseExceptionFilter`:

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. W `Startup.ConfigureServices`Dodaj filtr akcji do kolekcji filters:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a>Odpowiedź na błąd niepowodzenia weryfikacji

W przypadku kontrolerów interfejsu API sieci Web MVC reaguje na <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> typ odpowiedzi, gdy Walidacja modelu zakończy się niepowodzeniem. MVC używa wyników <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> do konstruowania odpowiedzi na błąd w przypadku niepowodzenia walidacji. Poniższy przykład używa fabryki do zmiany domyślnego typu odpowiedzi na <xref:Microsoft.AspNetCore.Mvc.SerializableError> w `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a>Odpowiedź na błąd klienta

*Wynik błędu* jest definiowany w wyniku przy użyciu kodu stanu HTTP 400 lub wyższego. W przypadku kontrolerów interfejsu API sieci Web MVC przekształca wynik błędu z wynikiem <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.

::: moniker range="= aspnetcore-2.1"

> [!IMPORTANT]
> ASP.NET Core 2,1 generuje odpowiedź na szczegóły problemu, która jest niemal zgodna ze specyfikacją RFC 7807. Jeśli 100 procent zgodności jest ważne, Uaktualnij projekt do ASP.NET Core 2,2 lub nowszego.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Odpowiedź na błąd można skonfigurować w jeden z następujących sposobów:

1. [Implementuj ProblemDetailsFactory](#implement-problemdetailsfactory)
1. [Użyj ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a>Implementuj ProblemDetailsFactory

MVC używa `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`, aby generować wszystkie wystąpienia <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> i <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Obejmuje to odpowiedzi na błędy klientów, odpowiedzi na błędy walidacji i metody pomocnika `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem>.

Aby dostosować odpowiedź dotyczącą szczegółów problemu, należy zarejestrować niestandardową implementację `ProblemDetailsFactory` w `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Odpowiedź na błąd można skonfigurować zgodnie z opisem w sekcji [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a>Użyj ApiBehaviorOptions. ClientErrorMapping

Użyj właściwości <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping%2A>, aby skonfigurować zawartość `ProblemDetails` odpowiedzi. Na przykład poniższy kod w `Startup.ConfigureServices` aktualizuje właściwość `type` dla 404 odpowiedzi:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
