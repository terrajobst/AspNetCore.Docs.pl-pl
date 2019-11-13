---
title: Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak wywoływać interfejs API sieci Web z aplikacji Blazor za pomocą pomocników JSON, w tym do tworzenia żądań wymiany zasobów między źródłami (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/call-web-api
ms.openlocfilehash: b5c57317005d0072410542bad322458b1cb3f5ee
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962723"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="10e99-103">Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="10e99-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="10e99-104">[Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)i [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="10e99-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="10e99-105"> aplikacje webassembly wywołują interfejsy API sieci Web przy użyciu wstępnie skonfigurowanej usługi `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="10e99-105"> WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="10e99-106">Twórz żądania, które mogą obejmować opcje [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) języka JavaScript, przy użyciu Blazor pomocy JSON lub <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="10e99-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

Blazor<span data-ttu-id="10e99-107"> aplikacje serwera wywołują interfejsy API sieci Web przy użyciu wystąpień <xref:System.Net.Http.HttpClient> zwykle utworzonych przy użyciu <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="10e99-107"> Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="10e99-108">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="10e99-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="10e99-109">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10e99-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10e99-110">Aby uzyskać Blazor przykładów zestawu webassembly, zobacz następujące składniki w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="10e99-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="10e99-111">Wywoływanie interfejsu API sieci Web (*strony/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="10e99-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="10e99-112">Tester żądania HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="10e99-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="10e99-113">HttpClient i pomocnicy JSON</span><span class="sxs-lookup"><span data-stu-id="10e99-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="10e99-114">W Blazor aplikacje webassembly [HttpClient](xref:fundamentals/http-requests) jest dostępny jako usługa wstępnie skonfigurowana do wykonywania żądań z powrotem do serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="10e99-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="10e99-115">Aby korzystać z pomocników JSON `HttpClient`, Dodaj odwołanie do pakietu do `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="10e99-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="10e99-116">`HttpClient` i pomocników JSON są również używane do wywoływania punktów końcowych interfejsu API sieci Web innych firm.</span><span class="sxs-lookup"><span data-stu-id="10e99-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="10e99-117">`HttpClient` jest implementowana przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) przeglądarki i podlega jego ograniczeniom, w tym wymuszania tych samych zasad pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="10e99-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="10e99-118">Adres podstawowy klienta jest ustawiany na adres serwera źródłowego.</span><span class="sxs-lookup"><span data-stu-id="10e99-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="10e99-119">Wstrzyknąć wystąpienie `HttpClient` przy użyciu dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="10e99-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="10e99-120">W poniższych przykładach przebieg internetowy interfejs API sieci Web przetwarza operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD).</span><span class="sxs-lookup"><span data-stu-id="10e99-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="10e99-121">Przykłady są oparte na klasie `TodoItem`, która przechowuje:</span><span class="sxs-lookup"><span data-stu-id="10e99-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="10e99-122">Identyfikator (`Id`, `long`) &ndash; unikatowy identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="10e99-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="10e99-123">Nazwa (`Name`, `string`) &ndash; Nazwa elementu.</span><span class="sxs-lookup"><span data-stu-id="10e99-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="10e99-124">Status (`IsComplete`, `bool`) &ndash; wskazuje, czy zadanie do wykonania zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="10e99-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="10e99-125">Metody pomocnika JSON wysyłają żądania do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetwarzają odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="10e99-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="10e99-126">`GetJsonAsync` &ndash; wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="10e99-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="10e99-127">W poniższym kodzie `_todoItems` są wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="10e99-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="10e99-128">Metoda `GetTodoItems` jest wyzwalana, gdy składnik jest gotowy do renderowania ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="10e99-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="10e99-129">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10e99-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="10e99-130">`PostJsonAsync` &ndash; wysyła żądanie HTTP POST, w tym zawartość zakodowaną w formacie JSON, i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="10e99-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="10e99-131">W poniższym kodzie `_newItemName` jest udostępniany przez powiązany element składnika.</span><span class="sxs-lookup"><span data-stu-id="10e99-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="10e99-132">Metoda `AddItem` jest wyzwalana przez wybranie elementu `<button>`.</span><span class="sxs-lookup"><span data-stu-id="10e99-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="10e99-133">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10e99-133">See the sample app for a complete example.</span></span>

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
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="10e99-134">`PutJsonAsync` &ndash; wysyła żądanie HTTP PUT, w tym zawartość zakodowaną w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="10e99-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="10e99-135">W poniższym kodzie `_editItem` wartości dla `Name` i `IsCompleted` są udostępniane przez powiązane elementy składnika.</span><span class="sxs-lookup"><span data-stu-id="10e99-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="10e99-136">Element `Id` jest ustawiany, gdy element jest wybrany w innej części interfejsu użytkownika i wywoływana jest `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="10e99-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="10e99-137">Metoda `SaveItem` jest wyzwalana przez wybranie elementu Save `<button>`.</span><span class="sxs-lookup"><span data-stu-id="10e99-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="10e99-138">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10e99-138">See the sample app for a complete example.</span></span>

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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="10e99-139"><xref:System.Net.Http> zawiera dodatkowe metody rozszerzające do wysyłania żądań HTTP i otrzymywania odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="10e99-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="10e99-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania żądania HTTP Delete do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="10e99-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="10e99-141">W poniższym kodzie, element Delete `<button>` wywołuje metodę `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="10e99-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="10e99-142">Powiązany `<input>` element dostarcza `id` elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="10e99-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="10e99-143">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10e99-143">See the sample app for a complete example.</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="10e99-144">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="10e99-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="10e99-145">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która jest obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="10e99-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="10e99-146">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="10e99-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="10e99-147">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="10e99-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="10e99-148">Aby żądania z przeglądarki były wysyłane do punktu końcowego z innym źródłem, *punkt końcowy* musi włączyć [udostępnianie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="10e99-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="10e99-149">Przykładowa aplikacja demonstruje użycie mechanizmu CORS w składniku API wywołania (*strony/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="10e99-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="10e99-150">Aby umożliwić innym lokacjom wykonywanie żądań funkcji udostępniania zasobów między źródłami (CORS) w aplikacji, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="10e99-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="10e99-151">HttpClient i HttpRequestMessage za pomocą opcji żądania interfejsu API pobierania</span><span class="sxs-lookup"><span data-stu-id="10e99-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="10e99-152">W przypadku uruchamiania w zestawie webassembly w aplikacji Blazor webassembly Użyj [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage>, aby dostosować żądania.</span><span class="sxs-lookup"><span data-stu-id="10e99-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="10e99-153">Na przykład można określić identyfikator URI żądania, metodę HTTP i wszystkie żądane nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="10e99-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="10e99-154">Opcje żądania dostarczenia do źródłowego [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript przy użyciu właściwości `WebAssemblyHttpMessageHandler.FetchArgs` w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="10e99-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="10e99-155">Jak pokazano w poniższym przykładzie, właściwość `credentials` jest ustawiana na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="10e99-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="10e99-156">`FetchCredentialsOption.Include` ("include") &ndash; zaleca przeglądarce wysyłanie poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) nawet w przypadku żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="10e99-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="10e99-157">Dozwolone tylko w przypadku, gdy zasady CORS są skonfigurowane tak, aby zezwalały na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="10e99-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="10e99-158">`FetchCredentialsOption.Omit` ("Pomiń") &ndash; zaleca przeglądarki nigdy nie wysyłaj poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP).</span><span class="sxs-lookup"><span data-stu-id="10e99-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="10e99-159">`FetchCredentialsOption.SameOrigin` ("ten sam-Origin") &ndash; zaleca przeglądarce wysyłanie poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) tylko wtedy, gdy docelowy adres URL znajduje się na tym samym źródle co aplikacja wywołująca.</span><span class="sxs-lookup"><span data-stu-id="10e99-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
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

<span data-ttu-id="10e99-160">Aby uzyskać więcej informacji na temat opcji interfejsu API pobierania, zobacz [powiadomienia MDN Web docs: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="10e99-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="10e99-161">Podczas wysyłania poświadczeń (plików cookie/nagłówki autoryzacji) w żądaniach CORS nagłówek `Authorization` musi być dozwolony przez zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="10e99-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="10e99-162">Następujące zasady obejmują konfigurację programu:</span><span class="sxs-lookup"><span data-stu-id="10e99-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="10e99-163">Pochodzenie żądania (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="10e99-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="10e99-164">Dowolna Metoda (czasownik).</span><span class="sxs-lookup"><span data-stu-id="10e99-164">Any method (verb).</span></span>
* <span data-ttu-id="10e99-165">`Content-Type` i `Authorization` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="10e99-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="10e99-166">Aby zezwolić na nagłówek niestandardowy (na przykład `x-custom-header`), Wyświetl nagłówek podczas wywoływania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="10e99-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="10e99-167">Poświadczenia ustawione przez kod JavaScript po stronie klienta (`credentials` Właściwość ustawiona na `include`).</span><span class="sxs-lookup"><span data-stu-id="10e99-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="10e99-168">Aby uzyskać więcej informacji, zobacz <xref:security/cors> i składnik Tester żądania HTTP aplikacji przykładowej (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="10e99-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10e99-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="10e99-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="10e99-170">Konfiguracja punktu końcowego HTTPS Kestrel</span><span class="sxs-lookup"><span data-stu-id="10e99-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="10e99-171">Współużytkowanie zasobów między źródłami (CORS) w formacie W3C</span><span class="sxs-lookup"><span data-stu-id="10e99-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
