---
title: Wywoływanie interfejsu API sieci web z platformy ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak wywoływać internetowy interfejs API z aplikacji Blazor przy użyciu pomocników JSON, w tym wprowadzania współużytkowanie zasobów (między źródłami CORS) żądania udostępniania.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/25/2019
uid: blazor/call-web-api
ms.openlocfilehash: 1a13f9f1f9e660b39a1df584e49198c4bbb61533
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399180"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="ff343-103">Wywoływanie interfejsu API sieci web z platformy ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="ff343-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="ff343-104">Przez [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ff343-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="ff343-105">Aplikacje klienta Blazor wywołać przy użyciu wstępnie skonfigurowanych interfejsów API sieci web `HttpClient` usługi.</span><span class="sxs-lookup"><span data-stu-id="ff343-105">Blazor client-side apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="ff343-106">Tworzą żądania, które mogą obejmować JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) opcji za pomocą pomocników Blazor JSON lub <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="ff343-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="ff343-107">Blazor aplikacji po stronie serwera wywołania sieci web interfejsów API przy użyciu <xref:System.Net.Http.HttpClient> zwykle tworzony przy użyciu wystąpienia <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="ff343-107">Blazor server-side apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="ff343-108">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="ff343-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="ff343-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ff343-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ff343-110">Aby uzyskać Blazor przykłady po stronie klienta, zobacz następujące składniki w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ff343-110">For Blazor client-side examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="ff343-111">Wywoływanie internetowego interfejsu API (*Pages/CallWebAPI.razor*)</span><span class="sxs-lookup"><span data-stu-id="ff343-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="ff343-112">Tester żądania HTTP (*Components/HTTPRequestTester.razor*)</span><span class="sxs-lookup"><span data-stu-id="ff343-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="ff343-113">Pomocnicy klasy HttpClient i JSON</span><span class="sxs-lookup"><span data-stu-id="ff343-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="ff343-114">W aplikacjach klienta Blazor [HttpClient](xref:fundamentals/http-requests) jest dostępne jako wstępnie skonfigurowane usługi w celu wysyłania żądań do serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="ff343-114">In Blazor client-side apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="ff343-115">`HttpClient` i pomocników JSON są również używane w celu wywołania punktów końcowych interfejsu API sieci web innych firm.</span><span class="sxs-lookup"><span data-stu-id="ff343-115">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="ff343-116">`HttpClient` jest implementowana przy użyciu przeglądarki [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) i podlega swoje ograniczenia, w tym wyegzekwowania te same zasady pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="ff343-116">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="ff343-117">Podstawowy adres klienta jest ustawiony adres serwer źródłowy.</span><span class="sxs-lookup"><span data-stu-id="ff343-117">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="ff343-118">Wstrzykiwanie `HttpClient` przy użyciu `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="ff343-118">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="ff343-119">W poniższych przykładach procesy interfejsu API sieci web Todo tworzenia, odczytu, aktualizowanie i usuwanie operacji (CRUD).</span><span class="sxs-lookup"><span data-stu-id="ff343-119">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="ff343-120">Przykłady są oparte na `TodoItem` klasy, która przechowuje:</span><span class="sxs-lookup"><span data-stu-id="ff343-120">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="ff343-121">Identyfikator (`Id`, `long`) &ndash; Unikatowy identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="ff343-121">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="ff343-122">Nazwa (`Name`, `string`) &ndash; nazwy elementu.</span><span class="sxs-lookup"><span data-stu-id="ff343-122">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="ff343-123">Stan (`IsComplete`, `bool`) &ndash; wskazuje, czy element Todo zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="ff343-123">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="ff343-124">Metody pomocników JSON wysyłania żądań do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetworzenie odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="ff343-124">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="ff343-125">`GetJsonAsync` &ndash; Wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON do tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="ff343-125">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ff343-126">W poniższym kodzie `_todoItems` są wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="ff343-126">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="ff343-127">`GetTodoItems` Metody jest wyzwalana po zakończeniu składnika renderowania ([OnInitAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="ff343-127">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="ff343-128">Zobacz przykładową aplikację, aby uzyskać kompletny przykład.</span><span class="sxs-lookup"><span data-stu-id="ff343-128">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="ff343-129">`PostJsonAsync` &ndash; Wysyła żądanie HTTP POST, w tym w przypadku zawartości zakodowane w formacie JSON i analizuje treść odpowiedzi JSON do tworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="ff343-129">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ff343-130">W poniższym kodzie `_newItemName` są dostarczane przez element powiązania składnika.</span><span class="sxs-lookup"><span data-stu-id="ff343-130">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="ff343-131">`AddItem` Metody jest wyzwalany przez wybranie `<button>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ff343-131">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="ff343-132">Zobacz przykładową aplikację, aby uzyskać kompletny przykład.</span><span class="sxs-lookup"><span data-stu-id="ff343-132">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* <span data-ttu-id="ff343-133">`PutJsonAsync` &ndash; Wysyła żądanie PUT protokołu HTTP, w tym zawartości zakodowane w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="ff343-133">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="ff343-134">W poniższym kodzie `_editItem` wartości `Name` i `IsCompleted` są dostarczane przez elementów powiązania składnika.</span><span class="sxs-lookup"><span data-stu-id="ff343-134">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="ff343-135">Element `Id` zostaje ustawiona po wybraniu elementu w innej części interfejsu użytkownika i `EditItem` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ff343-135">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="ff343-136">`SaveItem` Metoda zostanie wywołany, wybierając pozycję Zapisz `<button>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ff343-136">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="ff343-137">Zobacz przykładową aplikację, aby uzyskać kompletny przykład.</span><span class="sxs-lookup"><span data-stu-id="ff343-137">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="ff343-138"><xref:System.Net.Http> zawiera metody dodatkowe rozszerzenia dla wysyłania żądań HTTP i odbierania odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="ff343-138"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="ff343-139">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania wysłanie żądania HTTP DELETE do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ff343-139">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="ff343-140">W poniższym kodzie, Usuń `<button>` wywołania elementu `DeleteItem` metody.</span><span class="sxs-lookup"><span data-stu-id="ff343-140">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="ff343-141">Granica `<input>` dostarcza element `id` elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="ff343-141">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="ff343-142">Zobacz przykładową aplikację, aby uzyskać kompletny przykład.</span><span class="sxs-lookup"><span data-stu-id="ff343-142">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="ff343-143">Współużytkowanie zasobów między źródłami (cors)</span><span class="sxs-lookup"><span data-stu-id="ff343-143">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="ff343-144">Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż obsługiwana strona sieci Web strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ff343-144">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="ff343-145">To ograniczenie jest nazywany *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="ff343-145">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="ff343-146">Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="ff343-146">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ff343-147">Aby wysyłać żądania z przeglądarki do punktu końcowego z innego źródła, *punktu końcowego* należy włączyć [współużytkowanie zasobów między źródłami (cors)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="ff343-147">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="ff343-148">Przykładowa aplikacja demonstruje użycie mechanizmu CORS w składniku Wywołaj interfejs API sieci Web (*Pages/CallWebAPI.razor*).</span><span class="sxs-lookup"><span data-stu-id="ff343-148">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="ff343-149">Aby zezwolić na innych witryn, które umożliwiają współużytkowanie zasobów żądania (między źródłami CORS) do aplikacji udostępniania, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="ff343-149">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="ff343-150">HttpClient i HttpRequestMessage z opcjami żądanie pobrania interfejsu API</span><span class="sxs-lookup"><span data-stu-id="ff343-150">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="ff343-151">W przypadku uruchamiania na format WebAssembly w aplikacji po stronie klienta Blazor, użyć [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage> dostosować żądań.</span><span class="sxs-lookup"><span data-stu-id="ff343-151">When running on WebAssembly in a Blazor client-side app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="ff343-152">Na przykład można określić identyfikatora URI żądania, metodę HTTP oraz wszystkie nagłówki żądania żądaną.</span><span class="sxs-lookup"><span data-stu-id="ff343-152">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="ff343-153">Podanie opcji żądania do podstawowego języka JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) przy użyciu `WebAssemblyHttpMessageHandler.FetchArgs` właściwości dla żądania.</span><span class="sxs-lookup"><span data-stu-id="ff343-153">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="ff343-154">Jak pokazano w poniższym przykładzie `credentials` właściwość jest ustawiona na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="ff343-154">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="ff343-155">`FetchCredentialsOption.Include` ("include") &ndash; Powiadamia przeglądarkę, aby wysyłała poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP), nawet w przypadku żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="ff343-155">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="ff343-156">Ta opcja jest dozwolone tylko, gdy zasady CORS jest skonfigurowana do Zezwalaj na stosowanie poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="ff343-156">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="ff343-157">`FetchCredentialsOption.Omit` ("Pomiń") &ndash; Zaleceniem przeglądarki nigdy do wysyłania poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP).</span><span class="sxs-lookup"><span data-stu-id="ff343-157">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="ff343-158">`FetchCredentialsOption.SameOrigin` ("tego samego źródła") &ndash; Zaleceniem przeglądarki do wysyłania poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP), tylko wtedy, gdy docelowy adres URL w tym samym źródle jako aplikacji wywołującej.</span><span class="sxs-lookup"><span data-stu-id="ff343-158">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

```cshtml
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/todo"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="ff343-159">Aby uzyskać więcej informacji na temat opcji pobrania interfejsu API, zobacz [MDN dokumentów sieci web: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="ff343-159">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="ff343-160">Podczas wysyłania poświadczeń (autoryzacji plików cookie. / nagłówki) w odpowiedzi na żądania CORS `Authorization` nagłówka muszą być dozwolone przez zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="ff343-160">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="ff343-161">Następujące zasady obejmuje konfigurację dla:</span><span class="sxs-lookup"><span data-stu-id="ff343-161">The following policy includes configuration for:</span></span>

* <span data-ttu-id="ff343-162">Żądanie źródła (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="ff343-162">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="ff343-163">Każda metoda (zlecenie).</span><span class="sxs-lookup"><span data-stu-id="ff343-163">Any method (verb).</span></span>
* <span data-ttu-id="ff343-164">`Content-Type` i `Authorization` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="ff343-164">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="ff343-165">Aby umożliwić niestandardowego nagłówka (na przykład `x-custom-header`), lista nagłówka podczas wywoływania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="ff343-165">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="ff343-166">Poświadczenia zestaw przez kod języka JavaScript po stronie klienta (`credentials` właściwością `include`).</span><span class="sxs-lookup"><span data-stu-id="ff343-166">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="ff343-167">Aby uzyskać więcej informacji, zobacz <xref:security/cors> , jak i składnika Tester żądania HTTP przykładową aplikację (*Components/HTTPRequestTester.razor*).</span><span class="sxs-lookup"><span data-stu-id="ff343-167">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff343-168">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ff343-168">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="ff343-169">Krzyżowe Origin Resource Sharing (CORS) w formacie W3C</span><span class="sxs-lookup"><span data-stu-id="ff343-169">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
