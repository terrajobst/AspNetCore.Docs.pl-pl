---
title: Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak wywoływać interfejs API sieci Web z aplikacji Blazor za pomocą pomocników JSON, w tym do tworzenia żądań wymiany zasobów między źródłami (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: e6996f0e6731b05038d0a9329152b8afd5f6796d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660147"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="69d6f-103">Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="69d6f-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="69d6f-104">[Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)i [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="69d6f-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="69d6f-105">[Blazor aplikacje Webassembly](xref:blazor/hosting-models#blazor-webassembly) wywołują interfejsy API sieci Web przy użyciu wstępnie skonfigurowanej usługi `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="69d6f-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="69d6f-106">Twórz żądania, które mogą obejmować opcje [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) języka JavaScript, przy użyciu Blazor pomocy JSON lub <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="69d6f-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span> <span data-ttu-id="69d6f-107">Usługa `HttpClient` w aplikacjach Blazor webassembly koncentruje się na przesyłaniu żądań z powrotem do serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="69d6f-107">The `HttpClient` service in Blazor WebAssembly apps is focused on making requests back to the server of origin.</span></span> <span data-ttu-id="69d6f-108">Wskazówki zawarte w tym temacie dotyczą tylko Blazor aplikacji webassembly.</span><span class="sxs-lookup"><span data-stu-id="69d6f-108">The guidance in this topic only pertains to Blazor WebAssembly apps.</span></span>

<span data-ttu-id="69d6f-109">[Blazor aplikacje serwera](xref:blazor/hosting-models#blazor-server) wywołują interfejsy API sieci Web przy użyciu wystąpień <xref:System.Net.Http.HttpClient>, zwykle utworzonych przy użyciu <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="69d6f-109">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances, typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="69d6f-110">Wskazówki zawarte w tym temacie nie odnoszą się do aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="69d6f-110">The guidance in this topic doesn't pertain to Blazor Server apps.</span></span> <span data-ttu-id="69d6f-111">Podczas opracowywania aplikacji Blazor Server postępuj zgodnie ze wskazówkami w <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="69d6f-111">When developing Blazor Server apps, follow the guidance in <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="69d6f-112">[Wyświetl lub Pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample)) &ndash; wybierz aplikację *BlazorWebAssemblySample* .</span><span class="sxs-lookup"><span data-stu-id="69d6f-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="69d6f-113">Zobacz następujące składniki w przykładowej aplikacji *BlazorWebAssemblySample* :</span><span class="sxs-lookup"><span data-stu-id="69d6f-113">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="69d6f-114">Wywoływanie interfejsu API sieci Web (*strony/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="69d6f-114">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="69d6f-115">Tester żądania HTTP (*Components/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="69d6f-115">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="69d6f-116">Pakiety</span><span class="sxs-lookup"><span data-stu-id="69d6f-116">Packages</span></span>

<span data-ttu-id="69d6f-117">Odwołuje się do *eksperymentalnej* [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)Pakiet NuGet HttpClient w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="69d6f-117">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="69d6f-118">`Microsoft.AspNetCore.Blazor.HttpClient` jest oparty na `HttpClient` i [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="69d6f-118">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="69d6f-119">Aby użyć stabilnego interfejsu API, Użyj pakietu [Microsoft. ASPNET. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , który używa [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="69d6f-119">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="69d6f-120">Korzystanie z stabilnego interfejsu API w `Microsoft.AspNet.WebApi.Client` nie zapewnia pomocników JSON opisanych w tym temacie, które są unikatowe dla eksperymentalnego `Microsoft.AspNetCore.Blazor.HttpClient` pakietu.</span><span class="sxs-lookup"><span data-stu-id="69d6f-120">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="69d6f-121">HttpClient i pomocnicy JSON</span><span class="sxs-lookup"><span data-stu-id="69d6f-121">HttpClient and JSON helpers</span></span>

<span data-ttu-id="69d6f-122">W aplikacji Blazor webassembly [HttpClient](xref:fundamentals/http-requests) jest dostępna jako usługa wstępnie skonfigurowana do wykonywania żądań z powrotem do serwera pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="69d6f-122">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="69d6f-123">Aplikacja serwera Blazor nie obejmuje domyślnie usługi `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="69d6f-123">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="69d6f-124">Udostępnienie `HttpClient` aplikacji przy użyciu [infrastruktury fabryki HttpClient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="69d6f-124">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="69d6f-125">`HttpClient` i pomocnicy JSON są również używane do wywoływania punktów końcowych interfejsu API sieci Web innych firm.</span><span class="sxs-lookup"><span data-stu-id="69d6f-125">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="69d6f-126">`HttpClient` jest implementowana przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) przeglądarki i podlega jego ograniczeniom, w tym wymuszania tych samych zasad pochodzenia.</span><span class="sxs-lookup"><span data-stu-id="69d6f-126">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="69d6f-127">Adres podstawowy klienta jest ustawiany na adres serwera źródłowego.</span><span class="sxs-lookup"><span data-stu-id="69d6f-127">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="69d6f-128">Wstrzyknąć wystąpienie `HttpClient` przy użyciu dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="69d6f-128">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="69d6f-129">W poniższych przykładach przebieg internetowy interfejs API sieci Web przetwarza operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD).</span><span class="sxs-lookup"><span data-stu-id="69d6f-129">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="69d6f-130">Przykłady są oparte na klasie `TodoItem`, która przechowuje:</span><span class="sxs-lookup"><span data-stu-id="69d6f-130">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="69d6f-131">Identyfikator (`Id`, `long`) &ndash; unikatowy identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="69d6f-131">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="69d6f-132">Nazwa (`Name`, `string`) &ndash; nazwa elementu.</span><span class="sxs-lookup"><span data-stu-id="69d6f-132">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="69d6f-133">Stan (`IsComplete`, `bool`) &ndash; wskazują, czy zadanie do wykonania zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="69d6f-133">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="69d6f-134">Metody pomocnika JSON wysyłają żądania do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetwarzają odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="69d6f-134">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="69d6f-135">`GetJsonAsync` &ndash; wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="69d6f-135">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="69d6f-136">W poniższym kodzie `_todoItems` są wyświetlane przez składnik.</span><span class="sxs-lookup"><span data-stu-id="69d6f-136">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="69d6f-137">Metoda `GetTodoItems` jest wyzwalana, gdy składnik jest gotowy do renderowania ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="69d6f-137">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="69d6f-138">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69d6f-138">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="69d6f-139">`PostJsonAsync` &ndash; wysyła żądanie HTTP POST, w tym zawartość zakodowaną w formacie JSON, i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="69d6f-139">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="69d6f-140">W poniższym kodzie `_newItemName` jest udostępniany przez powiązany element składnika.</span><span class="sxs-lookup"><span data-stu-id="69d6f-140">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="69d6f-141">Metoda `AddItem` jest wyzwalana przez wybranie elementu `<button>`.</span><span class="sxs-lookup"><span data-stu-id="69d6f-141">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="69d6f-142">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69d6f-142">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="69d6f-143">`PutJsonAsync` &ndash; wysyła żądanie HTTP PUT, w tym zawartość zakodowaną w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="69d6f-143">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="69d6f-144">W poniższym kodzie `_editItem` wartości dla `Name` i `IsCompleted` są udostępniane przez powiązane elementy składnika.</span><span class="sxs-lookup"><span data-stu-id="69d6f-144">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="69d6f-145">`Id` elementu jest ustawiana, gdy element jest wybrany w innej części interfejsu użytkownika i jest wywoływany `EditItem`.</span><span class="sxs-lookup"><span data-stu-id="69d6f-145">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="69d6f-146">Metoda `SaveItem` jest wyzwalana przez wybranie elementu `<button>` Zapisz.</span><span class="sxs-lookup"><span data-stu-id="69d6f-146">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="69d6f-147">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69d6f-147">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="69d6f-148"><xref:System.Net.Http> zawiera dodatkowe metody rozszerzające do wysyłania żądań HTTP i otrzymywania odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="69d6f-148"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="69d6f-149">[HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania żądania HTTP Delete do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="69d6f-149">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="69d6f-150">W poniższym kodzie element Delete `<button>` wywołuje metodę `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="69d6f-150">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="69d6f-151">Powiązany element `<input>` dostarcza `id` elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="69d6f-151">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="69d6f-152">Pełny przykład można znaleźć w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69d6f-152">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="69d6f-153">Współużytkowanie zasobów między źródłami (CORS)</span><span class="sxs-lookup"><span data-stu-id="69d6f-153">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="69d6f-154">Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która jest obsługiwana przez stronę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="69d6f-154">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="69d6f-155">To ograniczenie jest nazywane *zasadami tego samego źródła*.</span><span class="sxs-lookup"><span data-stu-id="69d6f-155">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="69d6f-156">Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="69d6f-156">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="69d6f-157">Aby żądania z przeglądarki były wysyłane do punktu końcowego z innym źródłem, *punkt końcowy* musi włączyć [udostępnianie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="69d6f-157">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="69d6f-158">[Przykładowa aplikacjaBlazor webassembly (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstruje użycie mechanizmu CORS w składniku API wywołania (*Pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="69d6f-158">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="69d6f-159">Aby umożliwić innym lokacjom wykonywanie żądań funkcji udostępniania zasobów między źródłami (CORS) w aplikacji, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="69d6f-159">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="69d6f-160">HttpClient i HttpRequestMessage za pomocą opcji żądania interfejsu API pobierania</span><span class="sxs-lookup"><span data-stu-id="69d6f-160">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="69d6f-161">W przypadku uruchamiania w zestawie webassembly w aplikacji Blazor webassembly Użyj [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage>, aby dostosować żądania.</span><span class="sxs-lookup"><span data-stu-id="69d6f-161">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="69d6f-162">Na przykład można określić identyfikator URI żądania, metodę HTTP i wszystkie żądane nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="69d6f-162">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

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

<span data-ttu-id="69d6f-163">Aby uzyskać więcej informacji na temat opcji interfejsu API pobierania, zobacz [powiadomienia MDN Web docs: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="69d6f-163">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="69d6f-164">Podczas wysyłania poświadczeń (plików cookie/nagłówki autoryzacji) w żądaniach CORS nagłówek `Authorization` musi być dozwolony przez zasady CORS.</span><span class="sxs-lookup"><span data-stu-id="69d6f-164">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="69d6f-165">Następujące zasady obejmują konfigurację programu:</span><span class="sxs-lookup"><span data-stu-id="69d6f-165">The following policy includes configuration for:</span></span>

* <span data-ttu-id="69d6f-166">Pochodzenie żądania (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="69d6f-166">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="69d6f-167">Dowolna Metoda (czasownik).</span><span class="sxs-lookup"><span data-stu-id="69d6f-167">Any method (verb).</span></span>
* <span data-ttu-id="69d6f-168">nagłówki `Content-Type` i `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="69d6f-168">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="69d6f-169">Aby zezwolić na nagłówek niestandardowy (na przykład `x-custom-header`), Wyświetl nagłówek podczas wywoływania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="69d6f-169">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="69d6f-170">Poświadczenia ustawione przez kod JavaScript po stronie klienta (`credentials` Właściwość ustawiona na `include`).</span><span class="sxs-lookup"><span data-stu-id="69d6f-170">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="69d6f-171">Aby uzyskać więcej informacji, zobacz <xref:security/cors> i składnik Tester żądania HTTP aplikacji przykładowej (*Components/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="69d6f-171">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69d6f-172">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="69d6f-172">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="69d6f-173">Konfiguracja punktu końcowego HTTPS Kestrel</span><span class="sxs-lookup"><span data-stu-id="69d6f-173">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="69d6f-174">Współużytkowanie zasobów między źródłami (CORS) w formacie W3C</span><span class="sxs-lookup"><span data-stu-id="69d6f-174">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
