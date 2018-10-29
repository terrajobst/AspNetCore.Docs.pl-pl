---
title: Rozpoczynanie pracy z usługą NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak za pomocą NSwag generować dokumentację i Pomóż strony dla internetowego interfejsu API platformy ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 6c7d76e2202bf47c8d3e5d296e64e9e8c820e2a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207852"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Rozpoczynanie pracy z usługą NSwag i ASP.NET Core

Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](https://rsuter.com)

::: moniker range=">= aspnetcore-2.1"

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker-end

Zarejestruj middlewares NSwag do:

* Generowanie specyfikacją struktury Swagger dla interfejsu API wdrożony w sieci web.
* Obsługiwać interfejs użytkownika struktury Swagger do przeglądania i testowanie interfejsu API sieci web.

Aby użyć [NSwag](https://github.com/RSuter/NSwag) middlewares platformy ASP.NET Core, zainstaluj [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet. Ten pakiet zawiera middlewares do generowania i obsługiwać specyfikacją struktury Swagger interfejs użytkownika struktury Swagger (v2 i v3), a [ReDoc UI](https://github.com/Rebilly/ReDoc).

Ponadto zdecydowanie zaleca się korzystania z tych firmy NSwag możliwości generowania kodu. Wybierz jedną z poniższych opcji, aby korzystać z funkcji generowania kodu:

* Użyj [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikacji pulpitu Windows podczas generowania kodu klienta w języku C# i TypeScript dla interfejsu API.
* Użyj [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakiety NuGet umożliwiające code generation wewnątrz projektu.
* Użyj NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Użyj [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet.

## <a name="features"></a>Funkcje

Głównym celem użycia NSwag jest możliwość nie tylko interfejs użytkownika struktury Swagger i generatora struktury Swagger, ale również upewnij wykorzystanie możliwości generowania kodu elastyczne. Nie ma potrzeby istniejącego interfejsu API&mdash;możesz użyć interfejsów API innych firm, które włączają struktury Swagger i umożliwić NSwag Generowanie implementacji klienta. W obu przypadkach cyklu tworzenia oprogramowania jest przyspieszona i łatwiej można dostosować do zmian interfejsów API.

## <a name="package-installation"></a>Instalacja pakietu

Pakiet NSwag NuGet mogą być dodawane przy użyciu następujących metod:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

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

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"
* W polu wyszukiwania wprowadź "NSwag.AspNetCore"
* Wybierz pakiet "NSwag.AspNetCore" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowany Terminal**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger

Zaimportuj następujące przestrzenie nazw w `Startup` klasy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

W `Startup.ConfigureServices` metodę rejestrowania wymagane usługi struktury Swagger: 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

W `Startup.Configure` metody włącza oprogramowanie pośredniczące dla obsługująca wygenerowane Specyfikacja Swagger i v3 interfejs użytkownika struktury Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Uruchom aplikację. Przejdź do `http://localhost:<port>/swagger` Aby wyświetlić interfejs użytkownika struktury Swagger. Przejdź do `http://localhost:<port>/swagger/v1/swagger.json` można zapoznać się ze specyfikacją struktury Swagger.

## <a name="code-generation"></a>Generowanie kodu

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Zainstaluj NSwagStudio z oficjalnego [repozytorium GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* Launch NSwagStudio. Wprowadź *swagger.json* adresu URL w pliku **URL Specyfikacja Swagger** polu tekstowym i kliknij przycisk **utworzyć lokalną kopię** przycisku.
* Wybierz **klienta CSharp** klienta, typ danych wyjściowych. Inne opcje obejmują **klienta TypeScript** i **Kontroler interfejsu API sieci Web języka CSharp**. Za pomocą kontrolera interfejsu API sieci Web jest zasadniczo odwróconej generacji. Specyfikacja usługi używa odbudować usługi.
* Kliknij przycisk **Generowanie danych wyjściowych** przycisku. Pełne C# klienta wdrożenie *TodoApi.NSwag* projektu jest generowany. Kliknij przycisk **klienta CSharp** karcie **dane wyjściowe** sekcję, aby wyświetlić wygenerowanego kodu klienta:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
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
> Kod klienta języka C# jest generowany na podstawie ustawień zdefiniowanych w **ustawienia** karcie **klienta CSharp** kartę. Zmodyfikuj ustawienia, aby wykonywać zadania takie jak zmiana nazwy przestrzeni nazw domyślne i generowanie metody synchronicznej.

* Skopiuj wygenerowany kod C# do pliku w projekcie klienta (na przykład [Xamarin.Forms](/xamarin/xamarin-forms/) aplikacji).
* Rozpocznij korzystanie z interfejsu API sieci web:

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Podstawowy adres URL i/lub klienta HTTP można wstawić do klienta interfejsu API. Najlepszym rozwiązaniem jest zawsze [ponowne użycie HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Inne sposoby generowania kodu klienta

Można wygenerować kod klienta w inny sposób więcej dostosowane do przepływu pracy:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [W kodzie](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Szablony T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Dostosuj

Struktury swagger zawiera opcje dokumentowanie modelu obiektów do jej obsługi ułatwiają realizację użycia interfejsu API sieci web.

### <a name="api-info-and-description"></a>Informacje o interfejsie API i opis

W `Startup.Configure` metody akcji konfiguracji przekazywane do `UseSwagger` metoda dodaje informacje, takie jak tworzenie, licencji i opis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Interfejs użytkownika struktury Swagger Wyświetla informacje o wersji:

![Interfejs użytkownika struktury swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>komentarze XML

Komentarze XML są włączane wraz z następujących metod:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **edytowanie pliku .csproj < project_name >**.
* Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**
* Sprawdź **pliku dokumentacji XML** pole w obszarze **dane wyjściowe** części **kompilacji** kartę

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu. Przejdź do **narzędzia** > **Edytuj plik**.
* Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**
* Sprawdź **Generuj dokumentację xml** pole w obszarze **ogólne opcje** sekcji

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

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

Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecany typ zwracany dla akcji internetowego interfejsu API jest [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). W związku z tym nie można wywnioskować NSwag, co robi Twoja Akcja i ją zwraca. Rozważmy następujący przykład:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Poprzedni zwraca akcji `IActionResult`, ale wewnątrz akcji go zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) lub [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Adnotacje danych są używane do Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia. Dekoracji działanie z następującymi atrybutami:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), a zalecany typ zwracany dla akcji internetowego interfejsu API jest [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). W związku z tym, NSwag tylko może wywnioskować typ zwracany zdefiniowane przez `T`. Nie można wywnioskować inne możliwe zwracane typy akcji. Rozważmy następujący przykład:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Poprzedni zwraca akcji `ActionResult<T>`, ale wewnątrz akcji go zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Ponieważ kontrolerowi zostanie nadany [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) odpowiedzi możliwe jest również. Zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses) Aby uzyskać więcej informacji. Adnotacje danych są używane do Poinformuj klientów, który kodów stanu HTTP, ta akcja jest znany do zwrócenia. Dekoracji działanie z następującymi atrybutami:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

Generatora struktury Swagger teraz można dokładnie opisują tej akcji i wygenerowanego klienci wiedzieli, czego otrzymują podczas wywoływania punktu końcowego. Zdecydowanie zaleca się urządzanie wszystkie akcje za pomocą tych atrybutów. Aby uzyskać wskazówki, jakie odpowiedzi HTTP, powinien zwrócić swoje działania interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
