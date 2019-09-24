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
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Wywoływanie interfejsu API sieci Web z ASP.NET Core Blazor

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor aplikacje webassembly wywołują interfejsy API sieci Web przy użyciu `HttpClient` wstępnie skonfigurowanej usługi. Twórz żądania, które mogą obejmować opcje [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) języka JavaScript, za pomocą pomocników JSON Blazor <xref:System.Net.Http.HttpRequestMessage>lub za pomocą polecenia.

Aplikacje serwera Blazor wywołują interfejsy API <xref:System.Net.Http.HttpClient> sieci Web przy użyciu <xref:System.Net.Http.IHttpClientFactory>wystąpień zwykle utworzonych przy użyciu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykłady Blazor webassembly można znaleźć w następujących składnikach w przykładowej aplikacji:

* Wywoływanie interfejsu API sieci Web (*strony/CallWebAPI. Razor*)
* Tester żądania HTTP (*Components/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>HttpClient i pomocnicy JSON

W aplikacjach Blazor webassembly [HttpClient](xref:fundamentals/http-requests) jest dostępny jako usługa wstępnie skonfigurowana do wykonywania żądań z powrotem do serwera pochodzenia. Aby skorzystać `HttpClient` z pomocników JSON, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.Blazor.HttpClient`do. `HttpClient`i pomocniki JSON są również używane do wywoływania punktów końcowych interfejsu API sieci Web innych firm. `HttpClient`Program jest implementowany przy użyciu [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) przeglądarki i podlega jego ograniczeniom, w tym wymuszania tych samych zasad pochodzenia.

Adres podstawowy klienta jest ustawiany na adres serwera źródłowego. `HttpClient` Wstrzyknąć wystąpienie `@inject` przy użyciu dyrektywy:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

W poniższych przykładach przebieg internetowy interfejs API sieci Web przetwarza operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD). Przykłady są oparte na `TodoItem` klasie, która przechowuje:

* Identyfikator (`Id`, `long`) &ndash; unikatowy identyfikator elementu.
* Nazwa (`Name`, `string`) &ndash; nazwa elementu.
* Stan (`IsComplete`, `bool` )&ndash; wskazujący, czy zadanie do wykonania zostało zakończone.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Metody pomocnika JSON wysyłają żądania do identyfikatora URI (internetowego interfejsu API w poniższych przykładach) i przetwarzają odpowiedzi:

* `GetJsonAsync`&ndash; Wysyła żądanie HTTP GET i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.

  W poniższym kodzie, `_todoItems` są wyświetlane przez składnik. Metoda jest wyzwalana, gdy składnik jest gotowy do renderowania (OnInitializedAsync).[](xref:blazor/components#lifecycle-methods) `GetTodoItems` Pełny przykład można znaleźć w przykładowej aplikacji.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash; Wysyła żądanie HTTP POST, w tym zawartość zakodowaną w formacie JSON, i analizuje treść odpowiedzi JSON w celu utworzenia obiektu.

  W poniższym kodzie `_newItemName` jest dostarczany przez powiązany element składnika. Metoda jest wyzwalana przez `<button>` wybranie elementu. `AddItem` Pełny przykład można znaleźć w przykładowej aplikacji.

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

* `PutJsonAsync`&ndash; Wysyła żądanie HTTP Put, w tym zawartość zakodowaną w formacie JSON.

  W poniższym kodzie `_editItem` wartości dla `Name` i `IsCompleted` są dostarczane przez powiązane elementy składnika. Element `Id` jest ustawiany, gdy element jest wybrany w innej części interfejsu użytkownika i `EditItem` jest wywoływany. Metoda jest wyzwalana przez wybranie elementu Save `<button>`. `SaveItem` Pełny przykład można znaleźć w przykładowej aplikacji.

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

<xref:System.Net.Http>zawiera dodatkowe metody rozszerzające do wysyłania żądań HTTP i otrzymywania odpowiedzi HTTP. [HttpClient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) służy do wysyłania żądania HTTP Delete do internetowego interfejsu API.

W poniższym kodzie element Delete `<button>` `DeleteItem` wywołuje metodę. `<input>` Element`id` powiązany zawiera element do usunięcia. Pełny przykład można znaleźć w przykładowej aplikacji.

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

## <a name="cross-origin-resource-sharing-cors"></a>Współużytkowanie zasobów między źródłami (CORS)

Zabezpieczenia przeglądarki uniemożliwiają stronom sieci Web wykonywanie żądań do innej domeny niż ta, która jest obsługiwana przez stronę sieci Web. To ograniczenie jest nazywane *zasadami tego samego źródła*. Zasady tego samego źródła uniemożliwiają złośliwej lokacji odczytywanie poufnych danych z innej lokacji. Aby żądania z przeglądarki były wysyłane do punktu końcowego z innym źródłem, *punkt końcowy* musi włączyć [udostępnianie zasobów między źródłami (CORS)](https://www.w3.org/TR/cors/).

Przykładowa aplikacja demonstruje użycie mechanizmu CORS w składniku API wywołania (*strony/CallWebAPI. Razor*).

Aby umożliwić innym lokacjom wykonywanie żądań funkcji udostępniania zasobów między źródłami (CORS) w aplikacji, zobacz <xref:security/cors>.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient i HttpRequestMessage za pomocą opcji żądania interfejsu API pobierania

W przypadku uruchamiania w zestawie webassembly w aplikacji Blazor webassembly Użyj [HttpClient](xref:fundamentals/http-requests) i <xref:System.Net.Http.HttpRequestMessage> aby dostosować żądania. Na przykład można określić identyfikator URI żądania, metodę HTTP i wszystkie żądane nagłówki żądania.

Opcje żądania dostarczenia do źródłowego [interfejsu API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript przy użyciu `WebAssemblyHttpMessageHandler.FetchArgs` właściwości w żądaniu. Jak pokazano w poniższym przykładzie, `credentials` właściwość jest ustawiana na jedną z następujących wartości:

* `FetchCredentialsOption.Include`("Dołącz") &ndash; Zaleca przeglądarce wysyłanie poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) nawet w przypadku żądań między źródłami. Dozwolone tylko w przypadku, gdy zasady CORS są skonfigurowane tak, aby zezwalały na poświadczenia.
* `FetchCredentialsOption.Omit`("Pomiń") &ndash; Zaleca przeglądarce nigdy nie wysyłać poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP).
* `FetchCredentialsOption.SameOrigin`("to samo-Origin") &ndash; Doradza przeglądarce do wysyłania poświadczeń (takich jak pliki cookie lub nagłówki uwierzytelniania HTTP) tylko wtedy, gdy docelowy adres URL znajduje się na tym samym źródle co aplikacja wywołująca.

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

Aby uzyskać więcej informacji na temat opcji interfejsu API [pobierania, zobacz powiadomienia MDN Web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Podczas wysyłania poświadczeń (plików cookie/nagłówki autoryzacji) w żądaniach `Authorization` CORS nagłówek musi być dozwolony przez zasady CORS.

Następujące zasady obejmują konfigurację programu:

* Pochodzenie żądania (`http://localhost:5000`, `https://localhost:5001`).
* Dowolna Metoda (czasownik).
* `Content-Type`i `Authorization` nagłówki. Aby zezwolić na nagłówek niestandardowy (na przykład `x-custom-header`), Wyświetl nagłówek podczas wywoływania. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>
* Poświadczenia ustawione przez kod JavaScript po stronie klienta (`credentials` Właściwość ustawiona na `include`).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Aby uzyskać więcej informacji, <xref:security/cors> Zobacz i składnik Tester żądania HTTP aplikacji przykładowej (*Components/HTTPRequestTester. Razor*).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/http-requests>
* [Współużytkowanie zasobów między źródłami (CORS) w formacie W3C](https://www.w3.org/TR/cors/)
