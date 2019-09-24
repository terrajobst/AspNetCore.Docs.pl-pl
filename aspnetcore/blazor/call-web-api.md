---
title: Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak wywoływać interfejs API sieci Web z aplikacji Blazor przy użyciu pomocników JSON, w tym do tworzenia żądań wymiany zasobów między źródłami (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/call-web-api
ms.openlocfilehash: 2e79c81dc2371ef5b00f9251be47433a141c8ab7
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207138"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="ae93a-103">Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="ae93a-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="ae93a-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ae93a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ae93a-105">Blazor aplikacje webassembly wywołują interfejsy API sieci Web przy użyciu `HttpClient` wstępnie skonfigurowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="ae93a-105">Blazor WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="ae93a-106">Twórz żądania, które mogą obejmować opcje [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) języka JavaScript, za pomocą pomocników JSON Blazor <xref:System.Net.Http.HttpRequestMessage>lub za pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="ae93a-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="ae93a-107">Aplikacje serwera Blazor wywołują interfejsy API <xref:System.Net.Http.HttpClient> sieci Web przy użyciu <xref:System.Net.Http.IHttpClientFactory>wystąpień zwykle utworzonych przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="ae93a-107">Blazor Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="ae93a-108">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="ae93a-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="ae93a-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae93a-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ae93a-110">Przykłady Blazor webassembly można znaleźć w następujących składnikach w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ae93a-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="ae93a-111">Wywoływanie interfejsu API sieci Web (*strony/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="ae93a-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="ae93a-112">Tester żądania HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="ae93a-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="ae93a-113">HttpClient i pomocnicy JSON</span><span class="sxs-lookup"><span data-stu-id="ae93a-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="ae93a-114">W aplikacjach Blazor webassembly [HttpClient](xref:fundamentals/http-requests) jest dostępny jako usługa wstępnie skonfigurowana do wykonywania żądań z powrotem do serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="ae93a-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="ae93a-115">Aby skorzystać `HttpClient` z pomocników JSON, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.Blazor.HttpClient`do.</span><span class="sxs-lookup"><span data-stu-id="ae93a-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="ae93a-116">`HttpClient`i pomocniki JSON są również używane do wywoływania punktów końcowych interfejsu API sieci Web innych firm.</span><span class="sxs-lookup"><span data-stu-id="ae93a-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="ae93a-117">`HttpClient`Program jest implementowany przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) przeglądarki i podlega jego ograniczeniom, w tym wymuszania tych samych zasad pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="ae93a-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="ae93a-118">Adres podstawowy klienta jest ustawiany na adres serwera źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ae93a-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="ae93a-119">`HttpClient` Wstrzyknąć wystąpienie `@inject` przy użyciu dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="ae93a-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="ae93a-120">W poniższych przykładach przebieg internetowy interfejs API sieci Web przetwarza operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD).</span><span class="sxs-lookup"><span data-stu-id="ae93a-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="ae93a-121">Przykłady są oparte na `TodoItem` klasie, która przechowuje:</span><span class="sxs-lookup"><span data-stu-id="ae93a-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="ae93a-122">Identyfikator (`Id`, `long`) &ndash; unikatowy identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="ae93a-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="ae93a-123">Nazwa (`Name`, `string`) &ndash; nazwa elementu.</span><span class="sxs-lookup"><span data-stu-id="ae93a-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="ae93a-124">Stan (`IsComplete`, `bool` )&ndash; wskazujący, czy zadanie do wykonania zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="ae93a-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="ae93a-125">Metody pomocnika JSON wysyłają żądania do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetwarzają odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="ae93a-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="ae93a-126">`GetJsonAsync`&ndash; Wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="ae93a-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ae93a-127">W poniższym kodzie, `_todoItems` są wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="ae93a-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="ae93a-128">Metoda jest wyzwalana, gdy składnik jest gotowy do renderowania (OnInitializedAsync).[](xref:blazor/components#lifecycle-methods) `GetTodoItems`</span><span class="sxs-lookup"><span data-stu-id="ae93a-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="ae93a-129">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae93a-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="ae93a-130">`PostJsonAsync`&ndash; Wysyła żądanie HTTP POST, w tym zawartość zakodowaną w formacie JSON, i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="ae93a-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ae93a-131">W poniższym kodzie `_newItemName` jest dostarczany przez powiązany element składnika.</span><span class="sxs-lookup"><span data-stu-id="ae93a-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="ae93a-132">Metoda jest wyzwalana przez `<button>` wybranie elementu. `AddItem`</span><span class="sxs-lookup"><span data-stu-id="ae93a-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="ae93a-133">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae93a-133">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="ae93a-134">`PutJsonAsync`&ndash; Wysyła żądanie HTTP Put, w tym zawartość zakodowaną w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="ae93a-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="ae93a-135">W poniższym kodzie `_editItem` wartości dla `Name` i `IsCompleted` są dostarczane przez powiązane elementy składnika.</span><span class="sxs-lookup"><span data-stu-id="ae93a-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="ae93a-136">Element `Id` jest ustawiany, gdy element jest wybrany w innej części interfejsu użytkownika i `EditItem` jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="ae93a-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="ae93a-137">Metoda jest wyzwalana przez wybranie elementu Save `<button>`. `SaveItem`</span><span class="sxs-lookup"><span data-stu-id="ae93a-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="ae93a-138">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae93a-138">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="ae93a-139"><xref:System.Net.Http>zawiera dodatkowe metody rozszerzające do wysyłania żądań HTTP i otrzymywania odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae93a-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="ae93a-140">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania żądania HTTP Delete do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ae93a-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="ae93a-141">W poniższym kodzie element Delete `<button>` `DeleteItem` wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="ae93a-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="ae93a-142">`<input>` Element`id` powiązany zawiera element do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="ae93a-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="ae93a-143">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae93a-143">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="ae93a-144">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="ae93a-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="ae93a-145">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która jest obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ae93a-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="ae93a-146">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="ae93a-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="ae93a-147">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="ae93a-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ae93a-148">Aby żądania z przeglądarki były wysyłane do punktu końcowego z innym źródłem, *punkt końcowy* musi włączyć [udostępnianie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="ae93a-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="ae93a-149">Przykładowa aplikacja demonstruje użycie mechanizmu CORS w składniku API wywołania (*strony/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="ae93a-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="ae93a-150">Aby umożliwić innym lokacjom wykonywanie żądań funkcji udostępniania zasobów między źródłami (CORS) w aplikacji, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="ae93a-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="ae93a-151">HttpClient i HttpRequestMessage za pomocą opcji żądania interfejsu API pobierania</span><span class="sxs-lookup"><span data-stu-id="ae93a-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="ae93a-152">W przypadku uruchamiania w zestawie webassembly w aplikacji Blazor webassembly Użyj [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage> aby dostosować żądania.</span><span class="sxs-lookup"><span data-stu-id="ae93a-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="ae93a-153">Na przykład można określić identyfikator URI żądania, metodę HTTP i wszystkie żądane nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="ae93a-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="ae93a-154">Opcje żądania dostarczenia do źródłowego [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript przy użyciu `WebAssemblyHttpMessageHandler.FetchArgs` właściwości w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="ae93a-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="ae93a-155">Jak pokazano w poniższym przykładzie, `credentials` właściwość jest ustawiana na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="ae93a-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="ae93a-156">`FetchCredentialsOption.Include`("Dołącz") &ndash; Zaleca przeglądarce wysyłanie poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) nawet w przypadku żądań między źródłami.</span><span class="sxs-lookup"><span data-stu-id="ae93a-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="ae93a-157">Dozwolone tylko w przypadku, gdy zasady CORS są skonfigurowane tak, aby zezwalały na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="ae93a-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="ae93a-158">`FetchCredentialsOption.Omit`("Pomiń") &ndash; Zaleca przeglądarce nigdy nie wysyłać poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP).</span><span class="sxs-lookup"><span data-stu-id="ae93a-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="ae93a-159">`FetchCredentialsOption.SameOrigin`("to samo-Origin") &ndash; Doradza przeglądarce do wysyłania poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) tylko wtedy, gdy docelowy adres URL znajduje się na tym samym źródle co aplikacja wywołująca.</span><span class="sxs-lookup"><span data-stu-id="ae93a-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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

<span data-ttu-id="ae93a-160">Aby uzyskać więcej informacji na temat opcji interfejsu API [pobierania, zobacz powiadomienia MDN Web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="ae93a-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="ae93a-161">Podczas wysyłania poświadczeń (plików cookie/nagłówki autoryzacji) w żądaniach `Authorization` CORS nagłówek musi być dozwolony przez zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="ae93a-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="ae93a-162">Następujące zasady obejmują konfigurację programu:</span><span class="sxs-lookup"><span data-stu-id="ae93a-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="ae93a-163">Pochodzenie żądania (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="ae93a-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="ae93a-164">Dowolna Metoda (czasownik).</span><span class="sxs-lookup"><span data-stu-id="ae93a-164">Any method (verb).</span></span>
* <span data-ttu-id="ae93a-165">`Content-Type`i `Authorization` nagłówki.</span><span class="sxs-lookup"><span data-stu-id="ae93a-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="ae93a-166">Aby zezwolić na nagłówek niestandardowy (na przykład `x-custom-header`), Wyświetl nagłówek podczas wywoływania. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*></span><span class="sxs-lookup"><span data-stu-id="ae93a-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="ae93a-167">Poświadczenia ustawione przez kod JavaScript po stronie klienta (`credentials` Właściwość ustawiona na `include`).</span><span class="sxs-lookup"><span data-stu-id="ae93a-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="ae93a-168">Aby uzyskać więcej informacji, <xref:security/cors> Zobacz i składnik Tester żądania HTTP aplikacji przykładowej (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="ae93a-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae93a-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ae93a-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="ae93a-170">Współużytkowanie zasobów między źródłami (CORS) w formacie W3C</span><span class="sxs-lookup"><span data-stu-id="ae93a-170">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
