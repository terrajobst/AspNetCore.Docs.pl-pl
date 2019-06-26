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
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Wywoływanie interfejsu API sieci web z platformy ASP.NET Core Blazor

Przez [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

Aplikacje klienta Blazor wywołać przy użyciu wstępnie skonfigurowanych interfejsów API sieci web `HttpClient` usługi. Tworzą żądania, które mogą obejmować JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) opcji za pomocą pomocników Blazor JSON lub <xref:System.Net.Http.HttpRequestMessage>.

Blazor aplikacji po stronie serwera wywołania sieci web interfejsów API przy użyciu <xref:System.Net.Http.HttpClient> zwykle tworzony przy użyciu wystąpienia <xref:System.Net.Http.IHttpClientFactory>. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Aby uzyskać Blazor przykłady po stronie klienta, zobacz następujące składniki w przykładowej aplikacji:

* Wywoływanie internetowego interfejsu API (*Pages/CallWebAPI.razor*)
* Tester żądania HTTP (*Components/HTTPRequestTester.razor*)

## <a name="httpclient-and-json-helpers"></a>Pomocnicy klasy HttpClient i JSON

W aplikacjach klienta Blazor [HttpClient](xref:fundamentals/http-requests) jest dostępne jako wstępnie skonfigurowane usługi w celu wysyłania żądań do serwera pochodzenia. `HttpClient` i pomocników JSON są również używane w celu wywołania punktów końcowych interfejsu API sieci web innych firm. `HttpClient` jest implementowana przy użyciu przeglądarki [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) i podlega swoje ograniczenia, w tym wyegzekwowania te same zasady pochodzenia.

Podstawowy adres klienta jest ustawiony adres serwer źródłowy. Wstrzykiwanie `HttpClient` przy użyciu `@inject` dyrektywy:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

W poniższych przykładach procesy interfejsu API sieci web Todo tworzenia, odczytu, aktualizowanie i usuwanie operacji (CRUD). Przykłady są oparte na `TodoItem` klasy, która przechowuje:

* Identyfikator (`Id`, `long`) &ndash; Unikatowy identyfikator elementu.
* Nazwa (`Name`, `string`) &ndash; nazwy elementu.
* Stan (`IsComplete`, `bool`) &ndash; wskazuje, czy element Todo zostało zakończone.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Metody pomocników JSON wysyłania żądań do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetworzenie odpowiedzi:

* `GetJsonAsync` &ndash; Wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON do tworzenia obiektu.

  W poniższym kodzie `_todoItems` są wyświetlane przez składnik. `GetTodoItems` Metody jest wyzwalana po zakończeniu składnika renderowania ([OnInitAsync](xref:blazor/components#lifecycle-methods)). Zobacz przykładową aplikację, aby uzyskać kompletny przykład.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync` &ndash; Wysyła żądanie HTTP POST, w tym w przypadku zawartości zakodowane w formacie JSON i analizuje treść odpowiedzi JSON do tworzenia obiektu.

  W poniższym kodzie `_newItemName` są dostarczane przez element powiązania składnika. `AddItem` Metody jest wyzwalany przez wybranie `<button>` elementu. Zobacz przykładową aplikację, aby uzyskać kompletny przykład.

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

* `PutJsonAsync` &ndash; Wysyła żądanie PUT protokołu HTTP, w tym zawartości zakodowane w formacie JSON.

  W poniższym kodzie `_editItem` wartości `Name` i `IsCompleted` są dostarczane przez elementów powiązania składnika. Element `Id` zostaje ustawiona po wybraniu elementu w innej części interfejsu użytkownika i `EditItem` jest wywoływana. `SaveItem` Metoda zostanie wywołany, wybierając pozycję Zapisz `<button>` elementu. Zobacz przykładową aplikację, aby uzyskać kompletny przykład.

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

<xref:System.Net.Http> zawiera metody dodatkowe rozszerzenia dla wysyłania żądań HTTP i odbierania odpowiedzi HTTP. [HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania wysłanie żądania HTTP DELETE do internetowego interfejsu API.

W poniższym kodzie, Usuń `<button>` wywołania elementu `DeleteItem` metody. Granica `<input>` dostarcza element `id` elementu do usunięcia. Zobacz przykładową aplikację, aby uzyskać kompletny przykład.

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

## <a name="cross-origin-resource-sharing-cors"></a>Współużytkowanie zasobów między źródłami (cors)

Poziom zabezpieczeń przeglądarki zapobiega wysyłania żądań do innej domeny niż obsługiwana strona sieci Web strony sieci Web. To ograniczenie jest nazywany *zasadami tego samego źródła*. Zasada tego samego źródła uniemożliwia złośliwych witryn odczytywanie poufnych danych z innej lokacji. Aby wysyłać żądania z przeglądarki do punktu końcowego z innego źródła, *punktu końcowego* należy włączyć [współużytkowanie zasobów między źródłami (cors)](https://www.w3.org/TR/cors/).

Przykładowa aplikacja demonstruje użycie mechanizmu CORS w składniku Wywołaj interfejs API sieci Web (*Pages/CallWebAPI.razor*).

Aby zezwolić na innych witryn, które umożliwiają współużytkowanie zasobów żądania (między źródłami CORS) do aplikacji udostępniania, zobacz <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient i HttpRequestMessage z opcjami żądanie pobrania interfejsu API

W przypadku uruchamiania na format WebAssembly w aplikacji po stronie klienta Blazor, użyć [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage> dostosować żądań. Na przykład można określić identyfikatora URI żądania, metodę HTTP oraz wszystkie nagłówki żądania żądaną.

Podanie opcji żądania do podstawowego języka JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) przy użyciu `WebAssemblyHttpMessageHandler.FetchArgs` właściwości dla żądania. Jak pokazano w poniższym przykładzie `credentials` właściwość jest ustawiona na jedną z następujących wartości:

* `FetchCredentialsOption.Include` ("include") &ndash; Powiadamia przeglądarkę, aby wysyłała poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP), nawet w przypadku żądań cross-origin. Ta opcja jest dozwolone tylko, gdy zasady CORS jest skonfigurowana do Zezwalaj na stosowanie poświadczeń.
* `FetchCredentialsOption.Omit` ("Pomiń") &ndash; Zaleceniem przeglądarki nigdy do wysyłania poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP).
* `FetchCredentialsOption.SameOrigin` ("tego samego źródła") &ndash; Zaleceniem przeglądarki do wysyłania poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP), tylko wtedy, gdy docelowy adres URL w tym samym źródle jako aplikacji wywołującej.

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

Aby uzyskać więcej informacji na temat opcji pobrania interfejsu API, zobacz [MDN dokumentów sieci web: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Podczas wysyłania poświadczeń (autoryzacji plików cookie. / nagłówki) w odpowiedzi na żądania CORS `Authorization` nagłówka muszą być dozwolone przez zasady CORS.

Następujące zasady obejmuje konfigurację dla:

* Żądanie źródła (`http://localhost:5000`, `https://localhost:5001`).
* Każda metoda (zlecenie).
* `Content-Type` i `Authorization` nagłówków. Aby umożliwić niestandardowego nagłówka (na przykład `x-custom-header`), lista nagłówka podczas wywoływania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.
* Poświadczenia zestaw przez kod języka JavaScript po stronie klienta (`credentials` właściwością `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Aby uzyskać więcej informacji, zobacz <xref:security/cors> , jak i składnika Tester żądania HTTP przykładową aplikację (*Components/HTTPRequestTester.razor*).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/http-requests>
* [Krzyżowe Origin Resource Sharing (CORS) w formacie W3C](https://www.w3.org/TR/cors/)
