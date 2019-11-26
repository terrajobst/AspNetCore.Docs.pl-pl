---
title: Make HTTP requests using IHttpClientFactory in ASP.NET Core
author: stevejgordon
description: Learn about using the IHttpClientFactory interface to manage logical HttpClient instances in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 7a5b5c84775ea2482034ef9f3e8a2376036e66cb
ms.sourcegitcommit: a104ba258ae7c0b3ee7c6fa7eaea1ddeb8b6eb73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478732"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="d51b8-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d51b8-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d51b8-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="d51b8-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="d51b8-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="d51b8-106">`IHttpClientFactory` offers the following benefits:</span><span class="sxs-lookup"><span data-stu-id="d51b8-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="d51b8-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="d51b8-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="d51b8-109">A default client can be registered for general access.</span><span class="sxs-lookup"><span data-stu-id="d51b8-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="d51b8-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="d51b8-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="d51b8-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="d51b8-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d51b8-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span><span class="sxs-lookup"><span data-stu-id="d51b8-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d51b8-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d51b8-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="d51b8-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span><span class="sxs-lookup"><span data-stu-id="d51b8-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="d51b8-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span><span class="sxs-lookup"><span data-stu-id="d51b8-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="d51b8-118">Consumption patterns</span><span class="sxs-lookup"><span data-stu-id="d51b8-118">Consumption patterns</span></span>

<span data-ttu-id="d51b8-119">There are several ways `IHttpClientFactory` can be used in an app:</span><span class="sxs-lookup"><span data-stu-id="d51b8-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="d51b8-120">Basic usage</span><span class="sxs-lookup"><span data-stu-id="d51b8-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="d51b8-121">Named clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="d51b8-122">Typed clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="d51b8-123">Generated clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="d51b8-124">The best approach depends upon the app's requirements.</span><span class="sxs-lookup"><span data-stu-id="d51b8-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="d51b8-125">Basic usage</span><span class="sxs-lookup"><span data-stu-id="d51b8-125">Basic usage</span></span>

<span data-ttu-id="d51b8-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="d51b8-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="d51b8-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d51b8-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d51b8-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="d51b8-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="d51b8-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="d51b8-130">It has no impact on how `HttpClient` is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="d51b8-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="d51b8-132">Named clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-132">Named clients</span></span>

<span data-ttu-id="d51b8-133">Named clients are a good choice when:</span><span class="sxs-lookup"><span data-stu-id="d51b8-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="d51b8-134">The app requires many distinct uses of `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="d51b8-135">Many `HttpClient`s have different configuration.</span><span class="sxs-lookup"><span data-stu-id="d51b8-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="d51b8-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d51b8-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="d51b8-137">In the preceding code the client is configured with:</span><span class="sxs-lookup"><span data-stu-id="d51b8-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="d51b8-138">The base address `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="d51b8-139">Two headers required to work with the GitHub API.</span><span class="sxs-lookup"><span data-stu-id="d51b8-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="d51b8-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="d51b8-140">CreateClient</span></span>

<span data-ttu-id="d51b8-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span><span class="sxs-lookup"><span data-stu-id="d51b8-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="d51b8-142">A new instance of `HttpClient` is created.</span><span class="sxs-lookup"><span data-stu-id="d51b8-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="d51b8-143">The configuration action is called.</span><span class="sxs-lookup"><span data-stu-id="d51b8-143">The configuration action is called.</span></span>

<span data-ttu-id="d51b8-144">To create a named client, pass its name into `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="d51b8-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="d51b8-145">In the preceding code, the request doesn't need to specify a hostname.</span><span class="sxs-lookup"><span data-stu-id="d51b8-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="d51b8-146">The code can pass just the path, since the base address configured for the client is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="d51b8-147">Typed clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-147">Typed clients</span></span>

<span data-ttu-id="d51b8-148">Typed clients:</span><span class="sxs-lookup"><span data-stu-id="d51b8-148">Typed clients:</span></span>

* <span data-ttu-id="d51b8-149">Provide the same capabilities as named clients without the need to use strings as keys.</span><span class="sxs-lookup"><span data-stu-id="d51b8-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="d51b8-150">Provides IntelliSense and compiler help when consuming clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="d51b8-151">Provide a single location to configure and interact with a particular `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="d51b8-152">For example, a single typed client might be used:</span><span class="sxs-lookup"><span data-stu-id="d51b8-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="d51b8-153">For a single backend endpoint.</span><span class="sxs-lookup"><span data-stu-id="d51b8-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="d51b8-154">To encapsulate all logic dealing with the endpoint.</span><span class="sxs-lookup"><span data-stu-id="d51b8-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="d51b8-155">Work with DI and can be injected where required in the app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="d51b8-156">A typed client accepts a `HttpClient` parameter in its constructor:</span><span class="sxs-lookup"><span data-stu-id="d51b8-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d51b8-157">In the preceding code:</span><span class="sxs-lookup"><span data-stu-id="d51b8-157">In the preceding code:</span></span>

* <span data-ttu-id="d51b8-158">The configuration is moved into the typed client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="d51b8-159">The `HttpClient` object is exposed as a public property.</span><span class="sxs-lookup"><span data-stu-id="d51b8-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="d51b8-160">API-specific methods can be created that expose `HttpClient` functionality.</span><span class="sxs-lookup"><span data-stu-id="d51b8-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="d51b8-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span><span class="sxs-lookup"><span data-stu-id="d51b8-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="d51b8-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span><span class="sxs-lookup"><span data-stu-id="d51b8-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="d51b8-163">The typed client is registered as transient with DI.</span><span class="sxs-lookup"><span data-stu-id="d51b8-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="d51b8-164">The typed client can be injected and consumed directly:</span><span class="sxs-lookup"><span data-stu-id="d51b8-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="d51b8-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span><span class="sxs-lookup"><span data-stu-id="d51b8-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="d51b8-166">The `HttpClient` can be encapsulated within a typed client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="d51b8-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span><span class="sxs-lookup"><span data-stu-id="d51b8-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="d51b8-168">In the preceding code, the `HttpClient` is stored in a private field.</span><span class="sxs-lookup"><span data-stu-id="d51b8-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="d51b8-169">Access to the `HttpClient` is by the public `GetRepos` method.</span><span class="sxs-lookup"><span data-stu-id="d51b8-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="d51b8-170">Generated clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-170">Generated clients</span></span>

<span data-ttu-id="d51b8-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="d51b8-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="d51b8-172">Refit is a REST library for .NET.</span><span class="sxs-lookup"><span data-stu-id="d51b8-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="d51b8-173">It converts REST APIs into live interfaces.</span><span class="sxs-lookup"><span data-stu-id="d51b8-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="d51b8-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span><span class="sxs-lookup"><span data-stu-id="d51b8-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="d51b8-175">An interface and a reply are defined to represent the external API and its response:</span><span class="sxs-lookup"><span data-stu-id="d51b8-175">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="d51b8-176">A typed client can be added, using Refit to generate the implementation:</span><span class="sxs-lookup"><span data-stu-id="d51b8-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="d51b8-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span><span class="sxs-lookup"><span data-stu-id="d51b8-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="d51b8-178">Outgoing request middleware</span><span class="sxs-lookup"><span data-stu-id="d51b8-178">Outgoing request middleware</span></span>

<span data-ttu-id="d51b8-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span><span class="sxs-lookup"><span data-stu-id="d51b8-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="d51b8-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="d51b8-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="d51b8-181">Simplifies defining the handlers to apply for each named client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="d51b8-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d51b8-183">Each of these handlers is able to perform work before and after the outgoing request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="d51b8-184">This pattern:</span><span class="sxs-lookup"><span data-stu-id="d51b8-184">This pattern:</span></span>

  * <span data-ttu-id="d51b8-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d51b8-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="d51b8-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span><span class="sxs-lookup"><span data-stu-id="d51b8-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="d51b8-187">buforowanie</span><span class="sxs-lookup"><span data-stu-id="d51b8-187">caching</span></span>
    * <span data-ttu-id="d51b8-188">obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="d51b8-188">error handling</span></span>
    * <span data-ttu-id="d51b8-189">serializacja</span><span class="sxs-lookup"><span data-stu-id="d51b8-189">serialization</span></span>
    * <span data-ttu-id="d51b8-190">rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="d51b8-190">logging</span></span>

<span data-ttu-id="d51b8-191">To create a delegating handler:</span><span class="sxs-lookup"><span data-stu-id="d51b8-191">To create a delegating handler:</span></span>

* <span data-ttu-id="d51b8-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="d51b8-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="d51b8-194">Execute code before passing the request to the next handler in the pipeline:</span><span class="sxs-lookup"><span data-stu-id="d51b8-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="d51b8-195">The preceding code checks if the `X-API-KEY` header is in the request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="d51b8-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span><span class="sxs-lookup"><span data-stu-id="d51b8-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="d51b8-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="d51b8-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="d51b8-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span><span class="sxs-lookup"><span data-stu-id="d51b8-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="d51b8-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span><span class="sxs-lookup"><span data-stu-id="d51b8-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="d51b8-200">Handlers can depend upon services of any scope.</span><span class="sxs-lookup"><span data-stu-id="d51b8-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="d51b8-201">Services that handlers depend upon are disposed when the handler is disposed.</span><span class="sxs-lookup"><span data-stu-id="d51b8-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="d51b8-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span><span class="sxs-lookup"><span data-stu-id="d51b8-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="d51b8-203">Multiple handlers can be registered in the order that they should execute.</span><span class="sxs-lookup"><span data-stu-id="d51b8-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="d51b8-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span><span class="sxs-lookup"><span data-stu-id="d51b8-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="d51b8-205">Use one of the following approaches to share per-request state with message handlers:</span><span class="sxs-lookup"><span data-stu-id="d51b8-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="d51b8-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="d51b8-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="d51b8-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="d51b8-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span><span class="sxs-lookup"><span data-stu-id="d51b8-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="d51b8-209">Use Polly-based handlers</span><span class="sxs-lookup"><span data-stu-id="d51b8-209">Use Polly-based handlers</span></span>

<span data-ttu-id="d51b8-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="d51b8-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="d51b8-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span><span class="sxs-lookup"><span data-stu-id="d51b8-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="d51b8-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span><span class="sxs-lookup"><span data-stu-id="d51b8-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="d51b8-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-214">The Polly extensions support adding Polly-based handlers to clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="d51b8-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span><span class="sxs-lookup"><span data-stu-id="d51b8-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="d51b8-216">Handle transient faults</span><span class="sxs-lookup"><span data-stu-id="d51b8-216">Handle transient faults</span></span>

<span data-ttu-id="d51b8-217">Faults typically occur when external HTTP calls are transient.</span><span class="sxs-lookup"><span data-stu-id="d51b8-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="d51b8-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span><span class="sxs-lookup"><span data-stu-id="d51b8-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="d51b8-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span><span class="sxs-lookup"><span data-stu-id="d51b8-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="d51b8-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="d51b8-220">HTTP 5xx</span></span>
* <span data-ttu-id="d51b8-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="d51b8-221">HTTP 408</span></span>

<span data-ttu-id="d51b8-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span><span class="sxs-lookup"><span data-stu-id="d51b8-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="d51b8-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span><span class="sxs-lookup"><span data-stu-id="d51b8-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="d51b8-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span><span class="sxs-lookup"><span data-stu-id="d51b8-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="d51b8-225">Dynamically select policies</span><span class="sxs-lookup"><span data-stu-id="d51b8-225">Dynamically select policies</span></span>

<span data-ttu-id="d51b8-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="d51b8-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span><span class="sxs-lookup"><span data-stu-id="d51b8-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="d51b8-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span><span class="sxs-lookup"><span data-stu-id="d51b8-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="d51b8-229">For any other HTTP method, a 30-second timeout is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="d51b8-230">Add multiple Polly handlers</span><span class="sxs-lookup"><span data-stu-id="d51b8-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="d51b8-231">It's common to nest Polly policies:</span><span class="sxs-lookup"><span data-stu-id="d51b8-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="d51b8-232">In the preceding example:</span><span class="sxs-lookup"><span data-stu-id="d51b8-232">In the preceding example:</span></span>

* <span data-ttu-id="d51b8-233">Two handlers are added.</span><span class="sxs-lookup"><span data-stu-id="d51b8-233">Two handlers are added.</span></span>
* <span data-ttu-id="d51b8-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span><span class="sxs-lookup"><span data-stu-id="d51b8-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="d51b8-235">Failed requests are retried up to three times.</span><span class="sxs-lookup"><span data-stu-id="d51b8-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="d51b8-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span><span class="sxs-lookup"><span data-stu-id="d51b8-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="d51b8-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span><span class="sxs-lookup"><span data-stu-id="d51b8-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="d51b8-238">Circuit breaker policies are stateful.</span><span class="sxs-lookup"><span data-stu-id="d51b8-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="d51b8-239">All calls through this client share the same circuit state.</span><span class="sxs-lookup"><span data-stu-id="d51b8-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="d51b8-240">Add policies from the Polly registry</span><span class="sxs-lookup"><span data-stu-id="d51b8-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="d51b8-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="d51b8-242">In the following code:</span><span class="sxs-lookup"><span data-stu-id="d51b8-242">In the following code:</span></span>

* <span data-ttu-id="d51b8-243">The "regular" and "long" polices are added.</span><span class="sxs-lookup"><span data-stu-id="d51b8-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="d51b8-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span><span class="sxs-lookup"><span data-stu-id="d51b8-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="d51b8-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="d51b8-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="d51b8-246">HttpClient and lifetime management</span><span class="sxs-lookup"><span data-stu-id="d51b8-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="d51b8-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="d51b8-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="d51b8-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="d51b8-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span><span class="sxs-lookup"><span data-stu-id="d51b8-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="d51b8-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span><span class="sxs-lookup"><span data-stu-id="d51b8-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="d51b8-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span><span class="sxs-lookup"><span data-stu-id="d51b8-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="d51b8-253">Creating more handlers than necessary can result in connection delays.</span><span class="sxs-lookup"><span data-stu-id="d51b8-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="d51b8-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="d51b8-255">The default handler lifetime is two minutes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="d51b8-256">The default value can be overridden on a per named client basis:</span><span class="sxs-lookup"><span data-stu-id="d51b8-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="d51b8-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span><span class="sxs-lookup"><span data-stu-id="d51b8-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="d51b8-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="d51b8-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="d51b8-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="d51b8-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="d51b8-262">Alternatives to IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="d51b8-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="d51b8-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span><span class="sxs-lookup"><span data-stu-id="d51b8-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="d51b8-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="d51b8-265">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-265">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="d51b8-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span><span class="sxs-lookup"><span data-stu-id="d51b8-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="d51b8-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="d51b8-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span><span class="sxs-lookup"><span data-stu-id="d51b8-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="d51b8-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span><span class="sxs-lookup"><span data-stu-id="d51b8-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="d51b8-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span><span class="sxs-lookup"><span data-stu-id="d51b8-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="d51b8-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-272">This sharing prevents socket exhaustion.</span><span class="sxs-lookup"><span data-stu-id="d51b8-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="d51b8-273">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span><span class="sxs-lookup"><span data-stu-id="d51b8-273">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="d51b8-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="d51b8-274">Cookies</span></span>

<span data-ttu-id="d51b8-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span><span class="sxs-lookup"><span data-stu-id="d51b8-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="d51b8-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span><span class="sxs-lookup"><span data-stu-id="d51b8-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="d51b8-277">For apps that require cookies, consider either:</span><span class="sxs-lookup"><span data-stu-id="d51b8-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="d51b8-278">Disabling automatic cookie handling</span><span class="sxs-lookup"><span data-stu-id="d51b8-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="d51b8-279">Avoiding `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="d51b8-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="d51b8-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span><span class="sxs-lookup"><span data-stu-id="d51b8-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="d51b8-281">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="d51b8-281">Logging</span></span>

<span data-ttu-id="d51b8-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span><span class="sxs-lookup"><span data-stu-id="d51b8-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="d51b8-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span><span class="sxs-lookup"><span data-stu-id="d51b8-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="d51b8-284">Additional logging, such as the logging of request headers, is only included at trace level.</span><span class="sxs-lookup"><span data-stu-id="d51b8-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="d51b8-285">The log category used for each client includes the name of the client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="d51b8-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="d51b8-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="d51b8-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="d51b8-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span><span class="sxs-lookup"><span data-stu-id="d51b8-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="d51b8-289">On the response, messages are logged after any other pipeline handlers have received the response.</span><span class="sxs-lookup"><span data-stu-id="d51b8-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="d51b8-290">Logging also occurs inside the request handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="d51b8-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="d51b8-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="d51b8-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span><span class="sxs-lookup"><span data-stu-id="d51b8-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="d51b8-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="d51b8-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span><span class="sxs-lookup"><span data-stu-id="d51b8-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="d51b8-295">This may include changes to request headers or to the response status code.</span><span class="sxs-lookup"><span data-stu-id="d51b8-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="d51b8-296">Including the name of the client in the log category enables log filtering for specific named clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="d51b8-297">Configure the HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="d51b8-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="d51b8-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="d51b8-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="d51b8-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span><span class="sxs-lookup"><span data-stu-id="d51b8-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="d51b8-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span><span class="sxs-lookup"><span data-stu-id="d51b8-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="d51b8-302">Use IHttpClientFactory in a console app</span><span class="sxs-lookup"><span data-stu-id="d51b8-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="d51b8-303">In a console app, add the following package references to the project:</span><span class="sxs-lookup"><span data-stu-id="d51b8-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="d51b8-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="d51b8-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="d51b8-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="d51b8-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="d51b8-306">In the following example:</span><span class="sxs-lookup"><span data-stu-id="d51b8-306">In the following example:</span></span>

* <span data-ttu-id="d51b8-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span><span class="sxs-lookup"><span data-stu-id="d51b8-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="d51b8-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="d51b8-309">`HttpClient` is used to retrieve a webpage.</span><span class="sxs-lookup"><span data-stu-id="d51b8-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="d51b8-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span><span class="sxs-lookup"><span data-stu-id="d51b8-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="d51b8-311">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d51b8-311">Additional resources</span></span>

* [<span data-ttu-id="d51b8-312">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="d51b8-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="d51b8-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span><span class="sxs-lookup"><span data-stu-id="d51b8-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="d51b8-314">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="d51b8-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d51b8-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="d51b8-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="d51b8-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="d51b8-317">It offers the following benefits:</span><span class="sxs-lookup"><span data-stu-id="d51b8-317">It offers the following benefits:</span></span>

* <span data-ttu-id="d51b8-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="d51b8-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="d51b8-320">A default client can be registered for other purposes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d51b8-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span><span class="sxs-lookup"><span data-stu-id="d51b8-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="d51b8-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d51b8-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span><span class="sxs-lookup"><span data-stu-id="d51b8-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d51b8-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d51b8-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="d51b8-325">Consumption patterns</span><span class="sxs-lookup"><span data-stu-id="d51b8-325">Consumption patterns</span></span>

<span data-ttu-id="d51b8-326">There are several ways `IHttpClientFactory` can be used in an app:</span><span class="sxs-lookup"><span data-stu-id="d51b8-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="d51b8-327">Basic usage</span><span class="sxs-lookup"><span data-stu-id="d51b8-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="d51b8-328">Named clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="d51b8-329">Typed clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="d51b8-330">Generated clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="d51b8-331">None of them are strictly superior to another.</span><span class="sxs-lookup"><span data-stu-id="d51b8-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="d51b8-332">The best approach depends upon the app's constraints.</span><span class="sxs-lookup"><span data-stu-id="d51b8-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="d51b8-333">Basic usage</span><span class="sxs-lookup"><span data-stu-id="d51b8-333">Basic usage</span></span>

<span data-ttu-id="d51b8-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span><span class="sxs-lookup"><span data-stu-id="d51b8-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="d51b8-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d51b8-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d51b8-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="d51b8-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="d51b8-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="d51b8-338">It has no impact on the way `HttpClient` is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="d51b8-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="d51b8-340">Named clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-340">Named clients</span></span>

<span data-ttu-id="d51b8-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span><span class="sxs-lookup"><span data-stu-id="d51b8-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="d51b8-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="d51b8-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span><span class="sxs-lookup"><span data-stu-id="d51b8-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="d51b8-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span><span class="sxs-lookup"><span data-stu-id="d51b8-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="d51b8-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span><span class="sxs-lookup"><span data-stu-id="d51b8-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="d51b8-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="d51b8-347">Specify the name of the client to be created:</span><span class="sxs-lookup"><span data-stu-id="d51b8-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="d51b8-348">In the preceding code, the request doesn't need to specify a hostname.</span><span class="sxs-lookup"><span data-stu-id="d51b8-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="d51b8-349">It can pass just the path, since the base address configured for the client is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="d51b8-350">Typed clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-350">Typed clients</span></span>

<span data-ttu-id="d51b8-351">Typed clients:</span><span class="sxs-lookup"><span data-stu-id="d51b8-351">Typed clients:</span></span>

* <span data-ttu-id="d51b8-352">Provide the same capabilities as named clients without the need to use strings as keys.</span><span class="sxs-lookup"><span data-stu-id="d51b8-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="d51b8-353">Provides IntelliSense and compiler help when consuming clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="d51b8-354">Provide a single location to configure and interact with a particular `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="d51b8-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span><span class="sxs-lookup"><span data-stu-id="d51b8-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="d51b8-356">Work with DI and can be injected where required in your app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="d51b8-357">A typed client accepts a `HttpClient` parameter in its constructor:</span><span class="sxs-lookup"><span data-stu-id="d51b8-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d51b8-358">In the preceding code, the configuration is moved into the typed client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="d51b8-359">The `HttpClient` object is exposed as a public property.</span><span class="sxs-lookup"><span data-stu-id="d51b8-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="d51b8-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span><span class="sxs-lookup"><span data-stu-id="d51b8-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="d51b8-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span><span class="sxs-lookup"><span data-stu-id="d51b8-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="d51b8-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span><span class="sxs-lookup"><span data-stu-id="d51b8-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="d51b8-363">The typed client is registered as transient with DI.</span><span class="sxs-lookup"><span data-stu-id="d51b8-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="d51b8-364">The typed client can be injected and consumed directly:</span><span class="sxs-lookup"><span data-stu-id="d51b8-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="d51b8-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span><span class="sxs-lookup"><span data-stu-id="d51b8-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="d51b8-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="d51b8-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span><span class="sxs-lookup"><span data-stu-id="d51b8-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="d51b8-368">In the preceding code, the `HttpClient` is stored as a private field.</span><span class="sxs-lookup"><span data-stu-id="d51b8-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="d51b8-369">All access to make external calls goes through the `GetRepos` method.</span><span class="sxs-lookup"><span data-stu-id="d51b8-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="d51b8-370">Generated clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-370">Generated clients</span></span>

<span data-ttu-id="d51b8-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="d51b8-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="d51b8-372">Refit is a REST library for .NET.</span><span class="sxs-lookup"><span data-stu-id="d51b8-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="d51b8-373">It converts REST APIs into live interfaces.</span><span class="sxs-lookup"><span data-stu-id="d51b8-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="d51b8-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span><span class="sxs-lookup"><span data-stu-id="d51b8-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="d51b8-375">An interface and a reply are defined to represent the external API and its response:</span><span class="sxs-lookup"><span data-stu-id="d51b8-375">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="d51b8-376">A typed client can be added, using Refit to generate the implementation:</span><span class="sxs-lookup"><span data-stu-id="d51b8-376">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="d51b8-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span><span class="sxs-lookup"><span data-stu-id="d51b8-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="d51b8-378">Outgoing request middleware</span><span class="sxs-lookup"><span data-stu-id="d51b8-378">Outgoing request middleware</span></span>

<span data-ttu-id="d51b8-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span><span class="sxs-lookup"><span data-stu-id="d51b8-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="d51b8-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="d51b8-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d51b8-382">Each of these handlers is able to perform work before and after the outgoing request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="d51b8-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d51b8-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d51b8-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span><span class="sxs-lookup"><span data-stu-id="d51b8-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="d51b8-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="d51b8-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span><span class="sxs-lookup"><span data-stu-id="d51b8-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="d51b8-387">The preceding code defines a basic handler.</span><span class="sxs-lookup"><span data-stu-id="d51b8-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="d51b8-388">It checks to see if an `X-API-KEY` header has been included on the request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="d51b8-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span><span class="sxs-lookup"><span data-stu-id="d51b8-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="d51b8-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="d51b8-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="d51b8-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span><span class="sxs-lookup"><span data-stu-id="d51b8-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="d51b8-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span><span class="sxs-lookup"><span data-stu-id="d51b8-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="d51b8-394">Handlers are free to depend upon services of any scope.</span><span class="sxs-lookup"><span data-stu-id="d51b8-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="d51b8-395">Services that handlers depend upon are disposed when the handler is disposed.</span><span class="sxs-lookup"><span data-stu-id="d51b8-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="d51b8-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span><span class="sxs-lookup"><span data-stu-id="d51b8-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="d51b8-397">Multiple handlers can be registered in the order that they should execute.</span><span class="sxs-lookup"><span data-stu-id="d51b8-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="d51b8-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span><span class="sxs-lookup"><span data-stu-id="d51b8-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="d51b8-399">Use one of the following approaches to share per-request state with message handlers:</span><span class="sxs-lookup"><span data-stu-id="d51b8-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="d51b8-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="d51b8-401">Use `IHttpContextAccessor` to access the current request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="d51b8-402">Create a custom `AsyncLocal` storage object to pass the data.</span><span class="sxs-lookup"><span data-stu-id="d51b8-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="d51b8-403">Use Polly-based handlers</span><span class="sxs-lookup"><span data-stu-id="d51b8-403">Use Polly-based handlers</span></span>

<span data-ttu-id="d51b8-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="d51b8-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="d51b8-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span><span class="sxs-lookup"><span data-stu-id="d51b8-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="d51b8-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span><span class="sxs-lookup"><span data-stu-id="d51b8-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="d51b8-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-408">The Polly extensions:</span><span class="sxs-lookup"><span data-stu-id="d51b8-408">The Polly extensions:</span></span>

* <span data-ttu-id="d51b8-409">Support adding Polly-based handlers to clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="d51b8-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span><span class="sxs-lookup"><span data-stu-id="d51b8-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="d51b8-411">The package isn't included in the ASP.NET Core shared framework.</span><span class="sxs-lookup"><span data-stu-id="d51b8-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="d51b8-412">Handle transient faults</span><span class="sxs-lookup"><span data-stu-id="d51b8-412">Handle transient faults</span></span>

<span data-ttu-id="d51b8-413">Most common faults occur when external HTTP calls are transient.</span><span class="sxs-lookup"><span data-stu-id="d51b8-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="d51b8-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span><span class="sxs-lookup"><span data-stu-id="d51b8-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="d51b8-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span><span class="sxs-lookup"><span data-stu-id="d51b8-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="d51b8-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d51b8-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span><span class="sxs-lookup"><span data-stu-id="d51b8-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="d51b8-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span><span class="sxs-lookup"><span data-stu-id="d51b8-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="d51b8-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span><span class="sxs-lookup"><span data-stu-id="d51b8-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="d51b8-420">Dynamically select policies</span><span class="sxs-lookup"><span data-stu-id="d51b8-420">Dynamically select policies</span></span>

<span data-ttu-id="d51b8-421">Additional extension methods exist which can be used to add Polly-based handlers.</span><span class="sxs-lookup"><span data-stu-id="d51b8-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="d51b8-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span><span class="sxs-lookup"><span data-stu-id="d51b8-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="d51b8-423">One overload allows the request to be inspected when defining which policy to apply:</span><span class="sxs-lookup"><span data-stu-id="d51b8-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="d51b8-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span><span class="sxs-lookup"><span data-stu-id="d51b8-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="d51b8-425">For any other HTTP method, a 30-second timeout is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="d51b8-426">Add multiple Polly handlers</span><span class="sxs-lookup"><span data-stu-id="d51b8-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="d51b8-427">It's common to nest Polly policies to provide enhanced functionality:</span><span class="sxs-lookup"><span data-stu-id="d51b8-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="d51b8-428">In the preceding example, two handlers are added.</span><span class="sxs-lookup"><span data-stu-id="d51b8-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="d51b8-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span><span class="sxs-lookup"><span data-stu-id="d51b8-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="d51b8-430">Failed requests are retried up to three times.</span><span class="sxs-lookup"><span data-stu-id="d51b8-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="d51b8-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span><span class="sxs-lookup"><span data-stu-id="d51b8-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="d51b8-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span><span class="sxs-lookup"><span data-stu-id="d51b8-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="d51b8-433">Circuit breaker policies are stateful.</span><span class="sxs-lookup"><span data-stu-id="d51b8-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="d51b8-434">All calls through this client share the same circuit state.</span><span class="sxs-lookup"><span data-stu-id="d51b8-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="d51b8-435">Add policies from the Polly registry</span><span class="sxs-lookup"><span data-stu-id="d51b8-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="d51b8-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="d51b8-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span><span class="sxs-lookup"><span data-stu-id="d51b8-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="d51b8-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="d51b8-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span><span class="sxs-lookup"><span data-stu-id="d51b8-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="d51b8-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="d51b8-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="d51b8-441">HttpClient and lifetime management</span><span class="sxs-lookup"><span data-stu-id="d51b8-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="d51b8-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="d51b8-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="d51b8-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="d51b8-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span><span class="sxs-lookup"><span data-stu-id="d51b8-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="d51b8-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span><span class="sxs-lookup"><span data-stu-id="d51b8-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="d51b8-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span><span class="sxs-lookup"><span data-stu-id="d51b8-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="d51b8-448">Creating more handlers than necessary can result in connection delays.</span><span class="sxs-lookup"><span data-stu-id="d51b8-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="d51b8-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="d51b8-450">The default handler lifetime is two minutes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="d51b8-451">The default value can be overridden on a per named client basis.</span><span class="sxs-lookup"><span data-stu-id="d51b8-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="d51b8-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span><span class="sxs-lookup"><span data-stu-id="d51b8-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="d51b8-453">Disposal of the client isn't required.</span><span class="sxs-lookup"><span data-stu-id="d51b8-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="d51b8-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="d51b8-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span><span class="sxs-lookup"><span data-stu-id="d51b8-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="d51b8-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="d51b8-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="d51b8-459">Alternatives to IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="d51b8-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="d51b8-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span><span class="sxs-lookup"><span data-stu-id="d51b8-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="d51b8-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="d51b8-462">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-462">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="d51b8-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span><span class="sxs-lookup"><span data-stu-id="d51b8-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="d51b8-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="d51b8-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span><span class="sxs-lookup"><span data-stu-id="d51b8-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="d51b8-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span><span class="sxs-lookup"><span data-stu-id="d51b8-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="d51b8-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span><span class="sxs-lookup"><span data-stu-id="d51b8-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="d51b8-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-469">This sharing prevents socket exhaustion.</span><span class="sxs-lookup"><span data-stu-id="d51b8-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="d51b8-470">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span><span class="sxs-lookup"><span data-stu-id="d51b8-470">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="d51b8-471">Cookies</span><span class="sxs-lookup"><span data-stu-id="d51b8-471">Cookies</span></span>

<span data-ttu-id="d51b8-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span><span class="sxs-lookup"><span data-stu-id="d51b8-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="d51b8-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span><span class="sxs-lookup"><span data-stu-id="d51b8-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="d51b8-474">For apps that require cookies, consider either:</span><span class="sxs-lookup"><span data-stu-id="d51b8-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="d51b8-475">Disabling automatic cookie handling</span><span class="sxs-lookup"><span data-stu-id="d51b8-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="d51b8-476">Avoiding `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="d51b8-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="d51b8-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span><span class="sxs-lookup"><span data-stu-id="d51b8-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="d51b8-478">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="d51b8-478">Logging</span></span>

<span data-ttu-id="d51b8-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span><span class="sxs-lookup"><span data-stu-id="d51b8-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="d51b8-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span><span class="sxs-lookup"><span data-stu-id="d51b8-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="d51b8-481">Additional logging, such as the logging of request headers, is only included at trace level.</span><span class="sxs-lookup"><span data-stu-id="d51b8-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="d51b8-482">The log category used for each client includes the name of the client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="d51b8-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="d51b8-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="d51b8-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span><span class="sxs-lookup"><span data-stu-id="d51b8-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="d51b8-486">On the response, messages are logged after any other pipeline handlers have received the response.</span><span class="sxs-lookup"><span data-stu-id="d51b8-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="d51b8-487">Logging also occurs inside the request handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="d51b8-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="d51b8-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span><span class="sxs-lookup"><span data-stu-id="d51b8-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="d51b8-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="d51b8-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span><span class="sxs-lookup"><span data-stu-id="d51b8-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="d51b8-492">This may include changes to request headers, for example, or to the response status code.</span><span class="sxs-lookup"><span data-stu-id="d51b8-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="d51b8-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span><span class="sxs-lookup"><span data-stu-id="d51b8-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="d51b8-494">Configure the HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="d51b8-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="d51b8-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="d51b8-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="d51b8-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span><span class="sxs-lookup"><span data-stu-id="d51b8-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="d51b8-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span><span class="sxs-lookup"><span data-stu-id="d51b8-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="d51b8-499">Use IHttpClientFactory in a console app</span><span class="sxs-lookup"><span data-stu-id="d51b8-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="d51b8-500">In a console app, add the following package references to the project:</span><span class="sxs-lookup"><span data-stu-id="d51b8-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="d51b8-501">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="d51b8-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="d51b8-502">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="d51b8-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="d51b8-503">In the following example:</span><span class="sxs-lookup"><span data-stu-id="d51b8-503">In the following example:</span></span>

* <span data-ttu-id="d51b8-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span><span class="sxs-lookup"><span data-stu-id="d51b8-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="d51b8-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="d51b8-506">`HttpClient` is used to retrieve a webpage.</span><span class="sxs-lookup"><span data-stu-id="d51b8-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="d51b8-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span><span class="sxs-lookup"><span data-stu-id="d51b8-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="d51b8-508">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d51b8-508">Additional resources</span></span>

* [<span data-ttu-id="d51b8-509">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="d51b8-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="d51b8-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span><span class="sxs-lookup"><span data-stu-id="d51b8-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="d51b8-511">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="d51b8-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="d51b8-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="d51b8-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="d51b8-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="d51b8-514">It offers the following benefits:</span><span class="sxs-lookup"><span data-stu-id="d51b8-514">It offers the following benefits:</span></span>

* <span data-ttu-id="d51b8-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="d51b8-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="d51b8-517">A default client can be registered for other purposes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d51b8-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span><span class="sxs-lookup"><span data-stu-id="d51b8-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="d51b8-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d51b8-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span><span class="sxs-lookup"><span data-stu-id="d51b8-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d51b8-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d51b8-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d51b8-522">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d51b8-522">Prerequisites</span></span>

<span data-ttu-id="d51b8-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span><span class="sxs-lookup"><span data-stu-id="d51b8-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="d51b8-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span><span class="sxs-lookup"><span data-stu-id="d51b8-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="d51b8-525">Consumption patterns</span><span class="sxs-lookup"><span data-stu-id="d51b8-525">Consumption patterns</span></span>

<span data-ttu-id="d51b8-526">There are several ways `IHttpClientFactory` can be used in an app:</span><span class="sxs-lookup"><span data-stu-id="d51b8-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="d51b8-527">Basic usage</span><span class="sxs-lookup"><span data-stu-id="d51b8-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="d51b8-528">Named clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="d51b8-529">Typed clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="d51b8-530">Generated clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="d51b8-531">None of them are strictly superior to another.</span><span class="sxs-lookup"><span data-stu-id="d51b8-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="d51b8-532">The best approach depends upon the app's constraints.</span><span class="sxs-lookup"><span data-stu-id="d51b8-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="d51b8-533">Basic usage</span><span class="sxs-lookup"><span data-stu-id="d51b8-533">Basic usage</span></span>

<span data-ttu-id="d51b8-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span><span class="sxs-lookup"><span data-stu-id="d51b8-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="d51b8-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d51b8-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d51b8-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="d51b8-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="d51b8-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="d51b8-538">It has no impact on the way `HttpClient` is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="d51b8-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="d51b8-540">Named clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-540">Named clients</span></span>

<span data-ttu-id="d51b8-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span><span class="sxs-lookup"><span data-stu-id="d51b8-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="d51b8-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="d51b8-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span><span class="sxs-lookup"><span data-stu-id="d51b8-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="d51b8-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span><span class="sxs-lookup"><span data-stu-id="d51b8-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="d51b8-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span><span class="sxs-lookup"><span data-stu-id="d51b8-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="d51b8-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="d51b8-547">Specify the name of the client to be created:</span><span class="sxs-lookup"><span data-stu-id="d51b8-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="d51b8-548">In the preceding code, the request doesn't need to specify a hostname.</span><span class="sxs-lookup"><span data-stu-id="d51b8-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="d51b8-549">It can pass just the path, since the base address configured for the client is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="d51b8-550">Typed clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-550">Typed clients</span></span>

<span data-ttu-id="d51b8-551">Typed clients:</span><span class="sxs-lookup"><span data-stu-id="d51b8-551">Typed clients:</span></span>

* <span data-ttu-id="d51b8-552">Provide the same capabilities as named clients without the need to use strings as keys.</span><span class="sxs-lookup"><span data-stu-id="d51b8-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="d51b8-553">Provides IntelliSense and compiler help when consuming clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="d51b8-554">Provide a single location to configure and interact with a particular `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="d51b8-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span><span class="sxs-lookup"><span data-stu-id="d51b8-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="d51b8-556">Work with DI and can be injected where required in your app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="d51b8-557">A typed client accepts a `HttpClient` parameter in its constructor:</span><span class="sxs-lookup"><span data-stu-id="d51b8-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d51b8-558">In the preceding code, the configuration is moved into the typed client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="d51b8-559">The `HttpClient` object is exposed as a public property.</span><span class="sxs-lookup"><span data-stu-id="d51b8-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="d51b8-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span><span class="sxs-lookup"><span data-stu-id="d51b8-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="d51b8-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span><span class="sxs-lookup"><span data-stu-id="d51b8-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="d51b8-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span><span class="sxs-lookup"><span data-stu-id="d51b8-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="d51b8-563">The typed client is registered as transient with DI.</span><span class="sxs-lookup"><span data-stu-id="d51b8-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="d51b8-564">The typed client can be injected and consumed directly:</span><span class="sxs-lookup"><span data-stu-id="d51b8-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="d51b8-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span><span class="sxs-lookup"><span data-stu-id="d51b8-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="d51b8-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="d51b8-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span><span class="sxs-lookup"><span data-stu-id="d51b8-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="d51b8-568">In the preceding code, the `HttpClient` is stored as a private field.</span><span class="sxs-lookup"><span data-stu-id="d51b8-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="d51b8-569">All access to make external calls goes through the `GetRepos` method.</span><span class="sxs-lookup"><span data-stu-id="d51b8-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="d51b8-570">Generated clients</span><span class="sxs-lookup"><span data-stu-id="d51b8-570">Generated clients</span></span>

<span data-ttu-id="d51b8-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="d51b8-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="d51b8-572">Refit is a REST library for .NET.</span><span class="sxs-lookup"><span data-stu-id="d51b8-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="d51b8-573">It converts REST APIs into live interfaces.</span><span class="sxs-lookup"><span data-stu-id="d51b8-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="d51b8-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span><span class="sxs-lookup"><span data-stu-id="d51b8-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="d51b8-575">An interface and a reply are defined to represent the external API and its response:</span><span class="sxs-lookup"><span data-stu-id="d51b8-575">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="d51b8-576">A typed client can be added, using Refit to generate the implementation:</span><span class="sxs-lookup"><span data-stu-id="d51b8-576">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="d51b8-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span><span class="sxs-lookup"><span data-stu-id="d51b8-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="d51b8-578">Outgoing request middleware</span><span class="sxs-lookup"><span data-stu-id="d51b8-578">Outgoing request middleware</span></span>

<span data-ttu-id="d51b8-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span><span class="sxs-lookup"><span data-stu-id="d51b8-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="d51b8-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="d51b8-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d51b8-582">Each of these handlers is able to perform work before and after the outgoing request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="d51b8-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d51b8-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d51b8-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span><span class="sxs-lookup"><span data-stu-id="d51b8-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="d51b8-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="d51b8-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span><span class="sxs-lookup"><span data-stu-id="d51b8-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="d51b8-587">The preceding code defines a basic handler.</span><span class="sxs-lookup"><span data-stu-id="d51b8-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="d51b8-588">It checks to see if an `X-API-KEY` header has been included on the request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="d51b8-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span><span class="sxs-lookup"><span data-stu-id="d51b8-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="d51b8-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="d51b8-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="d51b8-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span><span class="sxs-lookup"><span data-stu-id="d51b8-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="d51b8-593">The handler **must** be registered in DI as a transient service, never scoped.</span><span class="sxs-lookup"><span data-stu-id="d51b8-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="d51b8-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span><span class="sxs-lookup"><span data-stu-id="d51b8-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="d51b8-595">The handler's services could be disposed before the handler goes out of scope.</span><span class="sxs-lookup"><span data-stu-id="d51b8-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="d51b8-596">The disposed handler services causes the handler to fail.</span><span class="sxs-lookup"><span data-stu-id="d51b8-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="d51b8-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span><span class="sxs-lookup"><span data-stu-id="d51b8-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="d51b8-598">Multiple handlers can be registered in the order that they should execute.</span><span class="sxs-lookup"><span data-stu-id="d51b8-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="d51b8-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span><span class="sxs-lookup"><span data-stu-id="d51b8-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="d51b8-600">Use one of the following approaches to share per-request state with message handlers:</span><span class="sxs-lookup"><span data-stu-id="d51b8-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="d51b8-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="d51b8-602">Use `IHttpContextAccessor` to access the current request.</span><span class="sxs-lookup"><span data-stu-id="d51b8-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="d51b8-603">Create a custom `AsyncLocal` storage object to pass the data.</span><span class="sxs-lookup"><span data-stu-id="d51b8-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="d51b8-604">Use Polly-based handlers</span><span class="sxs-lookup"><span data-stu-id="d51b8-604">Use Polly-based handlers</span></span>

<span data-ttu-id="d51b8-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="d51b8-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="d51b8-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span><span class="sxs-lookup"><span data-stu-id="d51b8-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="d51b8-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span><span class="sxs-lookup"><span data-stu-id="d51b8-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="d51b8-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-609">The Polly extensions:</span><span class="sxs-lookup"><span data-stu-id="d51b8-609">The Polly extensions:</span></span>

* <span data-ttu-id="d51b8-610">Support adding Polly-based handlers to clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="d51b8-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span><span class="sxs-lookup"><span data-stu-id="d51b8-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="d51b8-612">The package isn't included in the ASP.NET Core shared framework.</span><span class="sxs-lookup"><span data-stu-id="d51b8-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="d51b8-613">Handle transient faults</span><span class="sxs-lookup"><span data-stu-id="d51b8-613">Handle transient faults</span></span>

<span data-ttu-id="d51b8-614">Most common faults occur when external HTTP calls are transient.</span><span class="sxs-lookup"><span data-stu-id="d51b8-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="d51b8-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span><span class="sxs-lookup"><span data-stu-id="d51b8-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="d51b8-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span><span class="sxs-lookup"><span data-stu-id="d51b8-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="d51b8-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d51b8-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span><span class="sxs-lookup"><span data-stu-id="d51b8-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="d51b8-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span><span class="sxs-lookup"><span data-stu-id="d51b8-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="d51b8-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span><span class="sxs-lookup"><span data-stu-id="d51b8-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="d51b8-621">Dynamically select policies</span><span class="sxs-lookup"><span data-stu-id="d51b8-621">Dynamically select policies</span></span>

<span data-ttu-id="d51b8-622">Additional extension methods exist which can be used to add Polly-based handlers.</span><span class="sxs-lookup"><span data-stu-id="d51b8-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="d51b8-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span><span class="sxs-lookup"><span data-stu-id="d51b8-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="d51b8-624">One overload allows the request to be inspected when defining which policy to apply:</span><span class="sxs-lookup"><span data-stu-id="d51b8-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="d51b8-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span><span class="sxs-lookup"><span data-stu-id="d51b8-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="d51b8-626">For any other HTTP method, a 30-second timeout is used.</span><span class="sxs-lookup"><span data-stu-id="d51b8-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="d51b8-627">Add multiple Polly handlers</span><span class="sxs-lookup"><span data-stu-id="d51b8-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="d51b8-628">It's common to nest Polly policies to provide enhanced functionality:</span><span class="sxs-lookup"><span data-stu-id="d51b8-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="d51b8-629">In the preceding example, two handlers are added.</span><span class="sxs-lookup"><span data-stu-id="d51b8-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="d51b8-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span><span class="sxs-lookup"><span data-stu-id="d51b8-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="d51b8-631">Failed requests are retried up to three times.</span><span class="sxs-lookup"><span data-stu-id="d51b8-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="d51b8-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span><span class="sxs-lookup"><span data-stu-id="d51b8-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="d51b8-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span><span class="sxs-lookup"><span data-stu-id="d51b8-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="d51b8-634">Circuit breaker policies are stateful.</span><span class="sxs-lookup"><span data-stu-id="d51b8-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="d51b8-635">All calls through this client share the same circuit state.</span><span class="sxs-lookup"><span data-stu-id="d51b8-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="d51b8-636">Add policies from the Polly registry</span><span class="sxs-lookup"><span data-stu-id="d51b8-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="d51b8-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="d51b8-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span><span class="sxs-lookup"><span data-stu-id="d51b8-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="d51b8-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="d51b8-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span><span class="sxs-lookup"><span data-stu-id="d51b8-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="d51b8-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="d51b8-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="d51b8-642">HttpClient and lifetime management</span><span class="sxs-lookup"><span data-stu-id="d51b8-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="d51b8-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="d51b8-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="d51b8-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="d51b8-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span><span class="sxs-lookup"><span data-stu-id="d51b8-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="d51b8-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span><span class="sxs-lookup"><span data-stu-id="d51b8-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="d51b8-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span><span class="sxs-lookup"><span data-stu-id="d51b8-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="d51b8-649">Creating more handlers than necessary can result in connection delays.</span><span class="sxs-lookup"><span data-stu-id="d51b8-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="d51b8-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="d51b8-651">The default handler lifetime is two minutes.</span><span class="sxs-lookup"><span data-stu-id="d51b8-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="d51b8-652">The default value can be overridden on a per named client basis.</span><span class="sxs-lookup"><span data-stu-id="d51b8-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="d51b8-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span><span class="sxs-lookup"><span data-stu-id="d51b8-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="d51b8-654">Disposal of the client isn't required.</span><span class="sxs-lookup"><span data-stu-id="d51b8-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="d51b8-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="d51b8-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="d51b8-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span><span class="sxs-lookup"><span data-stu-id="d51b8-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="d51b8-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="d51b8-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="d51b8-660">Alternatives to IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="d51b8-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="d51b8-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span><span class="sxs-lookup"><span data-stu-id="d51b8-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="d51b8-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="d51b8-663">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-663">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="d51b8-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span><span class="sxs-lookup"><span data-stu-id="d51b8-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="d51b8-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span><span class="sxs-lookup"><span data-stu-id="d51b8-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="d51b8-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span><span class="sxs-lookup"><span data-stu-id="d51b8-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="d51b8-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span><span class="sxs-lookup"><span data-stu-id="d51b8-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="d51b8-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span><span class="sxs-lookup"><span data-stu-id="d51b8-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="d51b8-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="d51b8-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="d51b8-670">This sharing prevents socket exhaustion.</span><span class="sxs-lookup"><span data-stu-id="d51b8-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="d51b8-671">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span><span class="sxs-lookup"><span data-stu-id="d51b8-671">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="d51b8-672">Cookies</span><span class="sxs-lookup"><span data-stu-id="d51b8-672">Cookies</span></span>

<span data-ttu-id="d51b8-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span><span class="sxs-lookup"><span data-stu-id="d51b8-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="d51b8-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span><span class="sxs-lookup"><span data-stu-id="d51b8-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="d51b8-675">For apps that require cookies, consider either:</span><span class="sxs-lookup"><span data-stu-id="d51b8-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="d51b8-676">Disabling automatic cookie handling</span><span class="sxs-lookup"><span data-stu-id="d51b8-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="d51b8-677">Avoiding `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="d51b8-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="d51b8-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span><span class="sxs-lookup"><span data-stu-id="d51b8-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="d51b8-679">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="d51b8-679">Logging</span></span>

<span data-ttu-id="d51b8-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span><span class="sxs-lookup"><span data-stu-id="d51b8-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="d51b8-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span><span class="sxs-lookup"><span data-stu-id="d51b8-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="d51b8-682">Additional logging, such as the logging of request headers, is only included at trace level.</span><span class="sxs-lookup"><span data-stu-id="d51b8-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="d51b8-683">The log category used for each client includes the name of the client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="d51b8-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="d51b8-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="d51b8-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span><span class="sxs-lookup"><span data-stu-id="d51b8-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="d51b8-687">On the response, messages are logged after any other pipeline handlers have received the response.</span><span class="sxs-lookup"><span data-stu-id="d51b8-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="d51b8-688">Logging also occurs inside the request handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="d51b8-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="d51b8-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span><span class="sxs-lookup"><span data-stu-id="d51b8-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="d51b8-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span><span class="sxs-lookup"><span data-stu-id="d51b8-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="d51b8-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span><span class="sxs-lookup"><span data-stu-id="d51b8-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="d51b8-693">This may include changes to request headers, for example, or to the response status code.</span><span class="sxs-lookup"><span data-stu-id="d51b8-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="d51b8-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span><span class="sxs-lookup"><span data-stu-id="d51b8-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="d51b8-695">Configure the HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="d51b8-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="d51b8-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span><span class="sxs-lookup"><span data-stu-id="d51b8-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="d51b8-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span><span class="sxs-lookup"><span data-stu-id="d51b8-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="d51b8-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span><span class="sxs-lookup"><span data-stu-id="d51b8-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="d51b8-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span><span class="sxs-lookup"><span data-stu-id="d51b8-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="d51b8-700">Use IHttpClientFactory in a console app</span><span class="sxs-lookup"><span data-stu-id="d51b8-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="d51b8-701">In a console app, add the following package references to the project:</span><span class="sxs-lookup"><span data-stu-id="d51b8-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="d51b8-702">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="d51b8-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="d51b8-703">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="d51b8-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="d51b8-704">In the following example:</span><span class="sxs-lookup"><span data-stu-id="d51b8-704">In the following example:</span></span>

* <span data-ttu-id="d51b8-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span><span class="sxs-lookup"><span data-stu-id="d51b8-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="d51b8-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="d51b8-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="d51b8-707">`HttpClient` is used to retrieve a webpage.</span><span class="sxs-lookup"><span data-stu-id="d51b8-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="d51b8-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span><span class="sxs-lookup"><span data-stu-id="d51b8-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="d51b8-709">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d51b8-709">Additional resources</span></span>

* [<span data-ttu-id="d51b8-710">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="d51b8-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="d51b8-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span><span class="sxs-lookup"><span data-stu-id="d51b8-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="d51b8-712">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="d51b8-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
