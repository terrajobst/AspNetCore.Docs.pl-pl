---
title: Obsługa błędów w ASP.NET Core interfejsów API sieci Web
author: pranavkm
description: Dowiedz się więcej o obsłudze błędów przy użyciu ASP.NET Core interfejsów API sieci Web.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/25/2019
uid: web-api/handle-errors
ms.openlocfilehash: 9c5dd2f89e7351f386d1f0633c831952dc58e568
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278733"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="41d30-103">Obsługa błędów w ASP.NET Core interfejsów API sieci Web</span><span class="sxs-lookup"><span data-stu-id="41d30-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="41d30-104">W tym artykule opisano sposób obsługi i dostosowywania obsługi błędów przy użyciu ASP.NET Core interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="41d30-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="41d30-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([Jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="41d30-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="41d30-106">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="41d30-106">Developer Exception Page</span></span>

<span data-ttu-id="41d30-107">[Strona wyjątków dla deweloperów](xref:fundamentals/error-handling) to przydatne narzędzie do uzyskiwania szczegółowych śladów stosu dla błędów serwera.</span><span class="sxs-lookup"><span data-stu-id="41d30-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

<span data-ttu-id="41d30-108">Na stronie wyjątku dewelopera jest wyświetlana odpowiedź w postaci zwykłego tekstu, jeśli klient nie akceptuje danych wyjściowych w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="41d30-108">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output.</span></span> <span data-ttu-id="41d30-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="41d30-109">For example:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> <span data-ttu-id="41d30-110">Stronę wyjątku dla deweloperów należy włączyć tylko wtedy, **gdy aplikacja jest uruchomiona w środowisku deweloperskim**.</span><span class="sxs-lookup"><span data-stu-id="41d30-110">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="41d30-111">Nie chcesz udostępniać szczegółowych informacji o wyjątku publicznie, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="41d30-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="41d30-112">Aby uzyskać więcej informacji na temat konfigurowania środowisk <xref:fundamentals/environments>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="41d30-112">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="41d30-113">Procedura obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="41d30-113">Exception handler</span></span>

<span data-ttu-id="41d30-114">W środowiskach innych niż programowanie [wyjątek obsługujący oprogramowanie pośredniczące](xref:fundamentals/error-handling) może być używany do tworzenia ładunku błędu:</span><span class="sxs-lookup"><span data-stu-id="41d30-114">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="41d30-115">W `Startup.Configure`programie Wywołaj <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> , aby użyć oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="41d30-115">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="41d30-116">Skonfiguruj akcję kontrolera, aby odpowiedzieć na `/error` trasę:</span><span class="sxs-lookup"><span data-stu-id="41d30-116">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="41d30-117">Poprzednia `Error` akcja powoduje wysłanie do klienta ładunku zgodnego z [RFC7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="41d30-117">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="41d30-118">Wyjątek obsługujący oprogramowanie pośredniczące może również dostarczyć bardziej szczegółowe dane wyjściowe negocjowane z zawartością w lokalnym środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="41d30-118">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="41d30-119">Wykonaj następujące kroki, aby utworzyć spójny format ładunku w środowisku deweloperskim i produkcyjnym:</span><span class="sxs-lookup"><span data-stu-id="41d30-119">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="41d30-120">W `Startup.Configure`programie Zarejestruj wyjątek specyficzny dla środowiska obsługi oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="41d30-120">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

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

    <span data-ttu-id="41d30-121">W poprzednim kodzie oprogramowanie pośredniczące jest zarejestrowane w:</span><span class="sxs-lookup"><span data-stu-id="41d30-121">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="41d30-122">Trasa `/error-local-development` w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="41d30-122">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="41d30-123">Trasa `/error` w środowiskach, które nie są przeznaczone do programowania.</span><span class="sxs-lookup"><span data-stu-id="41d30-123">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="41d30-124">Zastosuj Routing atrybutów do akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="41d30-124">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="41d30-125">Modyfikowanie odpowiedzi przy użyciu wyjątków</span><span class="sxs-lookup"><span data-stu-id="41d30-125">Use exceptions to modify the response</span></span>

<span data-ttu-id="41d30-126">Zawartość odpowiedzi można modyfikować poza kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="41d30-126">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="41d30-127">W przypadku interfejsu API sieci Web ASP.NET 4. x jeden ze sposobów na to zrobić <xref:System.Web.Http.HttpResponseException> przy użyciu typu.</span><span class="sxs-lookup"><span data-stu-id="41d30-127">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="41d30-128">ASP.NET Core nie zawiera równoważnego typu.</span><span class="sxs-lookup"><span data-stu-id="41d30-128">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="41d30-129">Pomoc techniczną dla programu `HttpResponseException` można dodać, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="41d30-129">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="41d30-130">Utwórz dobrze znany typ wyjątku o nazwie `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="41d30-130">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="41d30-131">Utwórz filtr akcji o nazwie `HttpResponseExceptionFilter`:</span><span class="sxs-lookup"><span data-stu-id="41d30-131">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="41d30-132">W `Startup.ConfigureServices`programie Dodaj filtr akcji do kolekcji filters:</span><span class="sxs-lookup"><span data-stu-id="41d30-132">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="41d30-133">Odpowiedź na błąd niepowodzenia weryfikacji</span><span class="sxs-lookup"><span data-stu-id="41d30-133">Validation failure error response</span></span>

<span data-ttu-id="41d30-134">W przypadku kontrolerów interfejsu API sieci Web MVC reaguje <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> na typ odpowiedzi, gdy Walidacja modelu kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="41d30-134">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="41d30-135">MVC używa wyników <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> w celu skonstruowania odpowiedzi na błąd w przypadku niepowodzenia walidacji.</span><span class="sxs-lookup"><span data-stu-id="41d30-135">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="41d30-136">Poniższy przykład używa fabryki do zmiany domyślnego typu odpowiedzi na <xref:Microsoft.AspNetCore.Mvc.SerializableError> wartość w: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="41d30-136">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="41d30-137">Odpowiedź na błąd klienta</span><span class="sxs-lookup"><span data-stu-id="41d30-137">Client error response</span></span>

<span data-ttu-id="41d30-138">*Wynik błędu* jest definiowany w wyniku przy użyciu kodu stanu HTTP 400 lub wyższego.</span><span class="sxs-lookup"><span data-stu-id="41d30-138">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="41d30-139">W przypadku kontrolerów interfejsu API sieci Web MVC przekształca wynik błędu z wynikiem <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="41d30-139">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="41d30-140">Odpowiedź na błąd można skonfigurować w jeden z następujących sposobów:</span><span class="sxs-lookup"><span data-stu-id="41d30-140">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="41d30-141">Implementuj ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="41d30-141">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="41d30-142">Użyj ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="41d30-142">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="41d30-143">Implementuj ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="41d30-143">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="41d30-144">MVC używa `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` do tworzenia wszystkich <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> wystąpień i <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="41d30-144">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="41d30-145">Obejmuje to odpowiedzi na błędy klientów, odpowiedzi na błędy walidacji i `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="41d30-145">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="41d30-146">Aby dostosować odpowiedź dotyczącą szczegółów problemu, zarejestruj niestandardową `ProblemDetailsFactory` implementację programu w programie `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="41d30-146">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="41d30-147">Odpowiedź na błąd można skonfigurować zgodnie z opisem w sekcji [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .</span><span class="sxs-lookup"><span data-stu-id="41d30-147">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="41d30-148">Użyj ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="41d30-148">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="41d30-149">Użyj właściwości <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> , aby skonfigurować zawartość `ProblemDetails` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="41d30-149">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="41d30-150">Na przykład poniższy kod w programie `Startup.ConfigureServices` `type` aktualizuje właściwość dla 404 odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="41d30-150">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
