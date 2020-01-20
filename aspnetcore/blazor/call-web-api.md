---
title: Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak wywoływać interfejs API sieci Web z aplikacji Blazor za pomocą pomocników JSON, w tym do tworzenia żądań wymiany zasobów między źródłami (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 66605f38a6fcaedebc92b0946dca1e5f28b593c6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160070"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="b756d-103">Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="b756d-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="b756d-104">[Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)i [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="b756d-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="b756d-105">[Blazor aplikacje Webassembly](xref:blazor/hosting-models#blazor-webassembly) wywołują interfejsy API sieci Web przy użyciu wstępnie skonfigurowanej usługi `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b756d-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="b756d-106">Twórz żądania, które mogą obejmować opcje [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) języka JavaScript, przy użyciu Blazor pomocy JSON lub <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="b756d-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="b756d-107">[Blazor aplikacje serwera](xref:blazor/hosting-models#blazor-server) wywołują interfejsy API sieci Web przy użyciu wystąpień <xref:System.Net.Http.HttpClient> zwykle utworzonych przy użyciu <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="b756d-107">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="b756d-108">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="b756d-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="b756d-109">[Wyświetl lub Pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample)) &ndash; wybierz aplikację *BlazorWebAssemblySample* .</span><span class="sxs-lookup"><span data-stu-id="b756d-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="b756d-110">Zobacz następujące składniki w przykładowej aplikacji *BlazorWebAssemblySample* :</span><span class="sxs-lookup"><span data-stu-id="b756d-110">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="b756d-111">Wywoływanie interfejsu API sieci Web (*strony/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="b756d-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="b756d-112">Tester żądania HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="b756d-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="b756d-113">Pakiety</span><span class="sxs-lookup"><span data-stu-id="b756d-113">Packages</span></span>

<span data-ttu-id="b756d-114">Odwołuje się do *eksperymentalnej* [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)Pakiet NuGet HttpClient w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b756d-114">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="b756d-115">`Microsoft.AspNetCore.Blazor.HttpClient` jest oparty na `HttpClient` i [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="b756d-115">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="b756d-116">Aby użyć stabilnego interfejsu API, Użyj pakietu [Microsoft. ASPNET. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , który używa [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="b756d-116">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="b756d-117">Korzystanie z stabilnego interfejsu API w `Microsoft.AspNet.WebApi.Client` nie zapewnia pomocników JSON opisanych w tym temacie, które są unikatowe dla eksperymentalnego `Microsoft.AspNetCore.Blazor.HttpClient` pakietu.</span><span class="sxs-lookup"><span data-stu-id="b756d-117">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="b756d-118">HttpClient i pomocnicy JSON</span><span class="sxs-lookup"><span data-stu-id="b756d-118">HttpClient and JSON helpers</span></span>

<span data-ttu-id="b756d-119">W aplikacji Blazor webassembly [HttpClient](xref:fundamentals/http-requests) jest dostępna jako usługa wstępnie skonfigurowana do wykonywania żądań z powrotem do serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="b756d-119">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="b756d-120">Aplikacja serwera Blazor nie obejmuje domyślnie usługi `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b756d-120">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="b756d-121">Udostępnienie `HttpClient` aplikacji przy użyciu [infrastruktury fabryki HttpClient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="b756d-121">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="b756d-122">`HttpClient` i pomocnicy JSON są również używane do wywoływania punktów końcowych interfejsu API sieci Web innych firm.</span><span class="sxs-lookup"><span data-stu-id="b756d-122">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="b756d-123">`HttpClient` jest implementowana przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) przeglądarki i podlega jego ograniczeniom, w tym wymuszania tych samych zasad pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="b756d-123">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="b756d-124">Adres podstawowy klienta jest ustawiany na adres serwera źródłowego.</span><span class="sxs-lookup"><span data-stu-id="b756d-124">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="b756d-125">Wstrzyknąć wystąpienie `HttpClient` przy użyciu dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b756d-125">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="b756d-126">W poniższych przykładach przebieg internetowy interfejs API sieci Web przetwarza operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD).</span><span class="sxs-lookup"><span data-stu-id="b756d-126">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="b756d-127">Przykłady są oparte na klasie `TodoItem`, która przechowuje:</span><span class="sxs-lookup"><span data-stu-id="b756d-127">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="b756d-128">Identyfikator (`Id`, `long`) &ndash; unikatowy identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="b756d-128">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="b756d-129">Nazwa (`Name`, `string`) &ndash; nazwa elementu.</span><span class="sxs-lookup"><span data-stu-id="b756d-129">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="b756d-130">Stan (`IsComplete`, `bool`) &ndash; wskazują, czy zadanie do wykonania zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="b756d-130">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="b756d-131">Metody pomocnika JSON wysyłają żądania do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetwarzają odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="b756d-131">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="b756d-132">`GetJsonAsync` &ndash; wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="b756d-132">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="b756d-133">W poniższym kodzie `_todoItems` są wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="b756d-133">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="b756d-134">Metoda `GetTodoItems` jest wyzwalana, gdy składnik jest gotowy do renderowania ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="b756d-134">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="b756d-135">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b756d-135">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="b756d-136">`PostJsonAsync` &ndash; wysyła żądanie HTTP POST, w tym zawartość zakodowaną w formacie JSON, i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="b756d-136">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="b756d-137">W poniższym kodzie `_newItemName` jest udostępniany przez powiązany element składnika.</span><span class="sxs-lookup"><span data-stu-id="b756d-137">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="b756d-138">Metoda `AddItem` jest wyzwalana przez wybranie elementu `<button>`.</span><span class="sxs-lookup"><span data-stu-id="b756d-138">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="b756d-139">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b756d-139">See the sample app for a complete example.</span></span>

  ```razor
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

* <span data-ttu-id="b756d-140">`PutJsonAsync` &ndash; wysyła żądanie HTTP PUT, w tym zawartość zakodowaną w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="b756d-140">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="b756d-141">W poniższym kodzie `_editItem` wartości dla `Name` i `IsCompleted` są udostępniane przez powiązane elementy składnika.</span><span class="sxs-lookup"><span data-stu-id="b756d-141">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="b756d-142">`Id` elementu jest ustawiana, gdy element jest wybrany w innej części interfejsu użytkownika i jest wywoływany `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="b756d-142">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="b756d-143">Metoda `SaveItem` jest wyzwalana przez wybranie elementu `<button>` Zapisz.</span><span class="sxs-lookup"><span data-stu-id="b756d-143">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="b756d-144">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b756d-144">See the sample app for a complete example.</span></span>

  ```razor
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

<span data-ttu-id="b756d-145"><xref:System.Net.Http> zawiera dodatkowe metody rozszerzające do wysyłania żądań HTTP i otrzymywania odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b756d-145"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="b756d-146">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania żądania HTTP Delete do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b756d-146">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="b756d-147">W poniższym kodzie element Delete `<button>` wywołuje metodę `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="b756d-147">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="b756d-148">Powiązany element `<input>` dostarcza `id` elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="b756d-148">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="b756d-149">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b756d-149">See the sample app for a complete example.</span></span>

```razor
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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="b756d-150">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="b756d-150">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="b756d-151">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która jest obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b756d-151">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="b756d-152">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="b756d-152">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="b756d-153">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="b756d-153">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="b756d-154">Aby żądania z przeglądarki były wysyłane do punktu końcowego z innym źródłem, *punkt końcowy* musi włączyć [udostępnianie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="b756d-154">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="b756d-155">[Przykładowa aplikacjaBlazor webassembly (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstruje użycie mechanizmu CORS w składniku API wywołania (*Pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="b756d-155">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="b756d-156">Aby umożliwić innym lokacjom wykonywanie żądań funkcji udostępniania zasobów między źródłami (CORS) w aplikacji, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="b756d-156">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="b756d-157">HttpClient i HttpRequestMessage za pomocą opcji żądania interfejsu API pobierania</span><span class="sxs-lookup"><span data-stu-id="b756d-157">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="b756d-158">W przypadku uruchamiania w zestawie webassembly w aplikacji Blazor webassembly Użyj [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage>, aby dostosować żądania.</span><span class="sxs-lookup"><span data-stu-id="b756d-158">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="b756d-159">Na przykład można określić identyfikator URI żądania, metodę HTTP i wszystkie żądane nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="b756d-159">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

```razor
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

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="b756d-160">Aby uzyskać więcej informacji na temat opcji interfejsu API pobierania, zobacz [powiadomienia MDN Web docs: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="b756d-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="b756d-161">Podczas wysyłania poświadczeń (plików cookie/nagłówki autoryzacji) w żądaniach CORS nagłówek `Authorization` musi być dozwolony przez zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="b756d-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="b756d-162">Następujące zasady obejmują konfigurację programu:</span><span class="sxs-lookup"><span data-stu-id="b756d-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="b756d-163">Pochodzenie żądania (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="b756d-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="b756d-164">Dowolna Metoda (czasownik).</span><span class="sxs-lookup"><span data-stu-id="b756d-164">Any method (verb).</span></span>
* <span data-ttu-id="b756d-165">nagłówki `Content-Type` i `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="b756d-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="b756d-166">Aby zezwolić na nagłówek niestandardowy (na przykład `x-custom-header`), Wyświetl nagłówek podczas wywoływania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="b756d-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="b756d-167">Poświadczenia ustawione przez kod JavaScript po stronie klienta (`credentials` Właściwość ustawiona na `include`).</span><span class="sxs-lookup"><span data-stu-id="b756d-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="b756d-168">Aby uzyskać więcej informacji, zobacz <xref:security/cors> i składnik Tester żądania HTTP aplikacji przykładowej (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="b756d-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b756d-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b756d-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="b756d-170">Konfiguracja punktu końcowego HTTPS Kestrel</span><span class="sxs-lookup"><span data-stu-id="b756d-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="b756d-171">Współużytkowanie zasobów między źródłami (CORS) w formacie W3C</span><span class="sxs-lookup"><span data-stu-id="b756d-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
