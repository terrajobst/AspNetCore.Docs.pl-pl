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
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a>Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor

[Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)i [Juan de la Cruz](https://github.com/juandelacruz23)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor aplikacje webassembly wywołują interfejsy API sieci Web przy użyciu wstępnie skonfigurowanej usługi `HttpClient`. Twórz żądania, które mogą obejmować opcje [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) języka JavaScript, przy użyciu Blazor pomocy JSON lub <xref:System.Net.Http.HttpRequestMessage>.

Blazor aplikacje serwera wywołują interfejsy API sieci Web przy użyciu wystąpień <xref:System.Net.Http.HttpClient> zwykle utworzonych przy użyciu <xref:System.Net.Http.IHttpClientFactory>. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

Aby uzyskać Blazor przykładów zestawu webassembly, zobacz następujące składniki w przykładowej aplikacji:

* Wywoływanie interfejsu API sieci Web (*strony/CallWebAPI. Razor*)
* Tester żądania HTTP (*Components/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>HttpClient i pomocnicy JSON

W Blazor aplikacje webassembly [HttpClient](xref:fundamentals/http-requests) jest dostępny jako usługa wstępnie skonfigurowana do wykonywania żądań z powrotem do serwera pochodzenia. Aby korzystać z pomocników JSON `HttpClient`, Dodaj odwołanie do pakietu do `Microsoft.AspNetCore.Blazor.HttpClient`. `HttpClient` i pomocników JSON są również używane do wywoływania punktów końcowych interfejsu API sieci Web innych firm. `HttpClient` jest implementowana przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) przeglądarki i podlega jego ograniczeniom, w tym wymuszania tych samych zasad pochodzenia.

Adres podstawowy klienta jest ustawiany na adres serwera źródłowego. Wstrzyknąć wystąpienie `HttpClient` przy użyciu dyrektywy `@inject`:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

W poniższych przykładach przebieg internetowy interfejs API sieci Web przetwarza operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD). Przykłady są oparte na klasie `TodoItem`, która przechowuje:

* Identyfikator (`Id`, `long`) &ndash; unikatowy identyfikator elementu.
* Nazwa (`Name`, `string`) &ndash; Nazwa elementu.
* Status (`IsComplete`, `bool`) &ndash; wskazuje, czy zadanie do wykonania zostało zakończone.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Metody pomocnika JSON wysyłają żądania do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetwarzają odpowiedzi:

* `GetJsonAsync` &ndash; wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.

  W poniższym kodzie `_todoItems` są wyświetlane przez składnik. Metoda `GetTodoItems` jest wyzwalana, gdy składnik jest gotowy do renderowania ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)). Pełny przykład można znaleźć w przykładowej aplikacji.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* `PostJsonAsync` &ndash; wysyła żądanie HTTP POST, w tym zawartość zakodowaną w formacie JSON, i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.

  W poniższym kodzie `_newItemName` jest udostępniany przez powiązany element składnika. Metoda `AddItem` jest wyzwalana przez wybranie elementu `<button>`. Pełny przykład można znaleźć w przykładowej aplikacji.

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

* `PutJsonAsync` &ndash; wysyła żądanie HTTP PUT, w tym zawartość zakodowaną w formacie JSON.

  W poniższym kodzie `_editItem` wartości dla `Name` i `IsCompleted` są udostępniane przez powiązane elementy składnika. Element `Id` jest ustawiany, gdy element jest wybrany w innej części interfejsu użytkownika i wywoływana jest `EditItem`. Metoda `SaveItem` jest wyzwalana przez wybranie elementu Save `<button>`. Pełny przykład można znaleźć w przykładowej aplikacji.

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

<xref:System.Net.Http> zawiera dodatkowe metody rozszerzające do wysyłania żądań HTTP i otrzymywania odpowiedzi HTTP. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania żądania HTTP Delete do internetowego interfejsu API.

W poniższym kodzie, element Delete `<button>` wywołuje metodę `DeleteItem`. Powiązany `<input>` element dostarcza `id` elementu do usunięcia. Pełny przykład można znaleźć w przykładowej aplikacji.

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

## <a name="cross-origin-resource-sharing-cors"></a>Współużytkowanie zasobów między źródłami (CORS)

Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która jest obsługiwana przez stronę sieci Web. To ograniczenie jest nazywane *zasadami tego samego źródła*. Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji. Aby żądania z przeglądarki były wysyłane do punktu końcowego z innym źródłem, *punkt końcowy* musi włączyć [udostępnianie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/).

Przykładowa aplikacja demonstruje użycie mechanizmu CORS w składniku API wywołania (*strony/CallWebAPI. Razor*).

Aby umożliwić innym lokacjom wykonywanie żądań funkcji udostępniania zasobów między źródłami (CORS) w aplikacji, zobacz <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient i HttpRequestMessage za pomocą opcji żądania interfejsu API pobierania

W przypadku uruchamiania w zestawie webassembly w aplikacji Blazor webassembly Użyj [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage>, aby dostosować żądania. Na przykład można określić identyfikator URI żądania, metodę HTTP i wszystkie żądane nagłówki żądania.

Opcje żądania dostarczenia do źródłowego [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript przy użyciu właściwości `WebAssemblyHttpMessageHandler.FetchArgs` w żądaniu. Jak pokazano w poniższym przykładzie, właściwość `credentials` jest ustawiana na jedną z następujących wartości:

* `FetchCredentialsOption.Include` ("include") &ndash; zaleca przeglądarce wysyłanie poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) nawet w przypadku żądań między źródłami. Dozwolone tylko w przypadku, gdy zasady CORS są skonfigurowane tak, aby zezwalały na poświadczenia.
* `FetchCredentialsOption.Omit` ("Pomiń") &ndash; zaleca przeglądarki nigdy nie wysyłaj poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP).
* `FetchCredentialsOption.SameOrigin` ("ten sam-Origin") &ndash; zaleca przeglądarce wysyłanie poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) tylko wtedy, gdy docelowy adres URL znajduje się na tym samym źródle co aplikacja wywołująca.

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

Aby uzyskać więcej informacji na temat opcji interfejsu API pobierania, zobacz [powiadomienia MDN Web docs: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Podczas wysyłania poświadczeń (plików cookie/nagłówki autoryzacji) w żądaniach CORS nagłówek `Authorization` musi być dozwolony przez zasady CORS.

Następujące zasady obejmują konfigurację programu:

* Pochodzenie żądania (`http://localhost:5000`, `https://localhost:5001`).
* Dowolna Metoda (czasownik).
* `Content-Type` i `Authorization` nagłówków. Aby zezwolić na nagłówek niestandardowy (na przykład `x-custom-header`), Wyświetl nagłówek podczas wywoływania <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.
* Poświadczenia ustawione przez kod JavaScript po stronie klienta (`credentials` Właściwość ustawiona na `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Aby uzyskać więcej informacji, zobacz <xref:security/cors> i składnik Tester żądania HTTP aplikacji przykładowej (*Components/HTTPRequestTester. Razor*).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Konfiguracja punktu końcowego HTTPS Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Współużytkowanie zasobów między źródłami (CORS) w formacie W3C](https://www.w3.org/TR/cors/)
