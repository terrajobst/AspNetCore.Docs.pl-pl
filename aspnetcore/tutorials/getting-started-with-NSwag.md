---
title: Rozpoczynanie pracy z usługą NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak za pomocą NSwag generować dokumentację i Pomóż strony dla internetowego interfejsu API platformy ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/30/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: af07ad771c582cfad80f297748c3c1049ff4a7d6
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419410"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Rozpoczynanie pracy z usługą NSwag i ASP.NET Core

Przez [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), i [firmy Dave Brock](https://twitter.com/daveabrock)

::: moniker range=">= aspnetcore-2.1"

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker-end

NSwag oferuje następujące możliwości:

* Umożliwia korzystanie z programu Swagger interfejsu użytkownika i programu Swagger generator.
* Możliwości generowania kodu elastyczne.

Nswag, nie ma potrzeby istniejącego interfejsu API&mdash;możesz użyć interfejsów API innych firm, które włączają struktury Swagger i wygenerować implementacji klienta. NSwag pozwala przyspieszyć cykl tworzenia oprogramowania i łatwo przystosować do zmiany interfejsu API.

## <a name="register-the-nswag-middleware"></a>Zarejestruj oprogramowanie pośredniczące NSwag

Zarejestruj NSwag oprogramowaniu pośredniczącym, aby:

* Generowanie specyfikacją struktury Swagger dla interfejsu API wdrożony w sieci web.
* Obsługiwać interfejs użytkownika struktury Swagger do przeglądania i testowanie interfejsu API sieci web.

Aby użyć [NSwag](https://github.com/RSuter/NSwag) oprogramowanie pośredniczące platformy ASP.NET Core, zainstaluj [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet. Ten pakiet zawiera oprogramowaniu pośredniczącym, aby wygenerować i obsługiwać specyfikacją struktury Swagger interfejs użytkownika struktury Swagger (v2 i v3), a [ReDoc UI](https://github.com/Rebilly/ReDoc).

Aby zainstalować pakiet NSwag NuGet, użyj jednej z następujących metod:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konsola Menedżera pakietów** okna:
  * Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**
  * Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje
  * Wykonaj następujące polecenie:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Z **Zarządzaj pakietami NuGet** okno dialogowe:
  * Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
  * Ustaw **źródła pakietu** na stronie "nuget.org"
  * W polu wyszukiwania wprowadź "NSwag.AspNetCore"
  * Wybierz pakiet "NSwag.AspNetCore" z **Przeglądaj** kartę, a następnie kliknij przycisk **instalacji**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"
* W polu wyszukiwania wprowadź "NSwag.AspNetCore"
* Wybierz pakiet "NSwag.AspNetCore" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowany Terminal**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger

 Dodawanie i konfigurowanie programu Swagger w aplikacji platformy ASP.NET Core, wykonując następujące kroki w `Startup` klasy:

* Zaimportuj następujące przestrzenie nazw:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

* W `ConfigureServices` metodę rejestrowania wymagane usługi struktury Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* W `Configure` metody włącza oprogramowanie pośredniczące dla obsługująca wygenerowane Specyfikacja Swagger i interfejs użytkownika struktury Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* Uruchom aplikację. Przejdź do:
  * `http://localhost:<port>/swagger` Aby wyświetlić interfejs użytkownika struktury Swagger.
  * `http://localhost:<port>/swagger/v1/swagger.json` Aby zapoznać się ze specyfikacją struktury Swagger.

## <a name="code-generation"></a>Generowanie kodu

Korzystać z zalet możliwości generowania kodu NSwag firmy, wybierając jedną z następujących opcji:

* [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; podczas generowania kodu klienta interfejsu API w aplikacji pulpitu Windows C# lub TypeScript.
* [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakietów NuGet dla generowania kodu wewnątrz projektu.
* NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine).
* [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet.

### <a name="generate-code-with-nswagstudio"></a>Generowanie kodu za pomocą NSwagStudio

* Zainstaluj NSwagStudio, postępując zgodnie z instrukcjami w artykule [repozytorium NSwagStudio GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* Uruchom NSwagStudio, a następnie wprowadź *swagger.json* adresu URL w pliku **Swagger URL specyfikacji** pola tekstowego. Na przykład *http://localhost:44354/swagger/v1/swagger.json*.
* Kliknij przycisk **utworzyć lokalną kopię** przycisk, aby wygenerować reprezentacja JSON usługi specyfikacją struktury Swagger.

  ![Utwórz lokalną kopię specyfikacją struktury Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* W **dane wyjściowe** obszaru, kliknij przycisk **klienta CSharp** pole wyboru. W zależności od projektu, można także **klienta TypeScript** lub **Kontroler interfejsu API sieci Web języka CSharp**. Jeśli wybierzesz **Kontroler interfejsu API sieci Web języka CSharp**, Specyfikacja usługi odtwarza usługi, która służy jako zwrotny generacji.
* Kliknij przycisk **Generowanie danych wyjściowych** do produkcji kompletna C# implementacji klienta *TodoApi.NSwag* projektu. Aby wyświetlić kod wygenerowanego klienta, kliknij przycisk **klienta CSharp** karty:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient;
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> C# Kod klienta jest generowany na podstawie wyborów w **ustawienia** kartę. Zmodyfikuj ustawienia, aby wykonywać zadania takie jak zmiana nazwy przestrzeni nazw domyślne i generowanie metody synchronicznej.

* Skopiuj wygenerowany C# kod do pliku w projekcie klienta, które będą korzystać z interfejsu API.
* Rozpocznij korzystanie z interfejsu API sieci web:

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a>Dostosowywanie dokumentacji interfejsu API

Struktury swagger zawiera opcje dokumentowanie modelu obiektów do jej obsługi ułatwiają realizację użycia interfejsu API sieci web.

### <a name="api-info-and-description"></a>Informacje o interfejsie API i opis

W `Startup.ConfigureServices` metody akcji konfiguracji przekazywane do `AddSwaggerDocument` metoda dodaje informacje, takie jak tworzenie, licencji i opis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

Interfejs użytkownika struktury Swagger Wyświetla informacje o wersji:

![Interfejs użytkownika struktury swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>komentarze XML

Aby włączyć komentarze XML, wykonaj następujące czynności:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **edytowanie pliku .csproj < project_name >**.
* Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**
* Sprawdź **pliku dokumentacji XML** pole w obszarze **dane wyjściowe** części **kompilacji** kartę

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu. Przejdź do **narzędzia** > **Edytuj plik**.
* Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**
* Sprawdź **Generuj dokumentację xml** pole w obszarze **ogólne opcje** sekcji

::: moniker-end

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Program Visual Studio Code / .NET Core interfejsu wiersza polecenia](#tab/visual-studio-code+netcore-cli)

Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a>Adnotacje danych

::: moniker range="<= aspnetcore-2.0"

Ponieważ używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i zalecanym typ zwracany dla akcji internetowego interfejsu API jest [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), nie można wywnioskować, co robi Twoja Akcja i ją zwraca.

Rozważmy następujący przykład:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Poprzedni zwraca akcji `IActionResult`, ale wewnątrz akcji go zwraca albo [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) lub [element BadRequest](xref:System.Web.Http.ApiController.BadRequest*). Adnotacje danych umożliwia Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia. Dekoracji działanie z następującymi atrybutami:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 Ponieważ NSwag używa [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecany typ zwracany dla akcji internetowego interfejsu API jest [ActionResult\<T >](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), tylko można go wywnioskować zwracany typ zdefiniowany przez `T`. Nie można automatycznie wywnioskować inne możliwe zwracane typy. 

Rozważmy następujący przykład:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Poprzedni zwraca akcji `ActionResult<T>`. W akcji, zwraca [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*). Ponieważ kontrolerowi zostanie nadany [[klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu [element BadRequest](xref:System.Web.Http.ApiController.BadRequest*) odpowiedzi jest to możliwe, zbyt. Aby uzyskać więcej informacji, zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses). Adnotacje danych umożliwia Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia. Dekoracji działanie z następującymi atrybutami:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

W programie ASP.NET Core 2.2 lub nowszej, można użyć konwencji zamiast jawnie urządzanie poszczególne akcje za pomocą `[ProducesResponseType]`. Aby uzyskać więcej informacji, zobacz <xref:web-api/advanced/conventions>.

::: moniker-end

Generatora struktury Swagger teraz można dokładnie opisują tej akcji i wygenerowanego klienci wiedzieli, czego otrzymują podczas wywoływania punktu końcowego. Jako zalecenie dekoracji wszystkie akcje za pomocą tych atrybutów.

Aby uzyskać wskazówki, jakie odpowiedzi HTTP, powinien zwrócić swoje działania interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
