---
title: Rozpoczynanie pracy z NSwag i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak używać NSwag Generowanie dokumentacji do strony sieci Web platformy ASP.NET Core API pomocy.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 0f0aa0eeaa174ef40f03aab2500a8b3ce37e9448
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094895"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Rozpoczynanie pracy z NSwag i ASP.NET Core

Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](https://rsuter.com)

::: moniker range="<= aspnetcore-2.0"
[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Przy użyciu [NSwag](https://github.com/RSuter/NSwag) z platformy ASP.NET Core wymaga oprogramowania pośredniczącego [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet. Pakiet składa się z generator Swagger, interfejs użytkownika programu Swagger (v2 i v3), i [interfejsu użytkownika ReDoc](https://github.com/Rebilly/ReDoc).

Zdecydowanie zaleca się korzystać z jego NSwag możliwości generowania kodu. Podczas generowania kodu, wybierz jedną z następujących opcji:

* Użyj [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikacji pulpitu systemu Windows podczas generowania kodu klienta w języku C# i TypeScript do interfejsu API.
* Użyj [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakietów NuGet do kodu generowania wewnątrz projektu.
* Użyj NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Użyj [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet.

## <a name="features"></a>Funkcje

Głównym celem używania NSwag jest możliwość wprowadzenia nie tylko interfejs użytkownika programu Swagger i struktury Swagger generator, jak używać funkcji generowania kodu elastyczne. Nie ma potrzeby istniejącego interfejsu API&mdash;można użyć interfejsów API innych firm, które włączają Swagger i umożliwić NSwag Generowanie implementacja klienta. W obu przypadkach jest przyspieszonego cyklu programowanie i łatwiej można dostosować do zmian interfejsu API.

## <a name="package-installation"></a>Instalacja pakietu

Pakiet NSwag NuGet można dodać z następujących metod:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konsola Menedżera pakietów** okno:
  * Przejdź do **widoku** > **innych okien** > **Konsola Menedżera pakietów**
  * Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje
  * Uruchom następujące polecenie:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Z **Zarządzaj pakietami NuGet** okna dialogowego:
  * Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
  * Ustaw **źródła pakietu** do "nuget.org"
  * W polu wyszukiwania wprowadź "NSwag.AspNetCore"
  * Wybierz pakiet "NSwag.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"
* W polu wyszukiwania wprowadź "NSwag.AspNetCore"
* Wybierz pakiet "NSwag.AspNetCore" w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowane Terminal**:

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

Importowanie następujących przestrzeni nazw w `Startup` klasy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

W `Startup.Configure` metody, włącza oprogramowanie pośredniczące dla obsługująca specyfikację struktury Swagger generowane i interfejsu użytkownika programu Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Uruchom aplikację. Przejdź do `http://localhost:<port>/swagger` do wyświetlania interfejsu użytkownika programu Swagger. Przejdź do `http://localhost:<port>/swagger/v1/swagger.json` Aby wyświetlić specyfikację struktury Swagger.

## <a name="code-generation"></a>Generowanie kodu

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Zainstaluj NSwagStudio z oficjalnego [repozytorium GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
* Launch NSwagStudio. Wprowadź *swagger.json* adres URL w pliku **Swagger URL specyfikacji** polem tekstowym i kliknij przycisk **utworzyć lokalną kopię** przycisku.
* Wybierz **klienta CSharp** typ danych wyjściowych klienta. Inne opcje obejmują **klienta TypeScript** i **Kontroler interfejsu API sieci Web CSharp**. Przy użyciu kontrolera interfejsu API sieci Web jest zasadniczo reverse generacji. Specyfikacja usługi używa odbudować usługi.
* Kliknij przycisk **Generowanie danych wyjściowych** przycisku. Pełna C# klienta implementacja elementu *TodoApi.NSwag* projektu jest generowany. Kliknij przycisk **klienta CSharp** karcie **dane wyjściowe** sekcję, aby wyświetlić kod wygenerowanego klienta:

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
> Kod klienta C# jest generowany na podstawie ustawień zdefiniowanych w **ustawienia** karcie **klienta CSharp** kartę. Zmodyfikuj ustawienia w celu wykonywania zadań takich jak zmiana nazwy przestrzeni nazw domyślne i generowania metoda synchroniczna.

* Skopiuj wygenerowany kod C# do pliku w projekcie klienta (na przykład [platformy Xamarin.Forms](/xamarin/xamarin-forms/) aplikacji).
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
> Podstawowy adres URL i/lub klient HTTP można wprowadzić do klienta interfejsu API. Najlepszym rozwiązaniem jest zawsze [ponowne użycie HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Inne sposoby generowanie kodu klienta

Istnieje możliwość wygenerowania kodu klienta w inny sposób więcej, nadaje się do przepływu pracy:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [W kodzie](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Szablony T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Dostosuj

Struktury swagger udostępnia opcje dla dokumentowanie model obiektów do jej obsługi ułatwiają przez interfejs API sieci web.

### <a name="api-info-and-description"></a>Informacje o interfejsu API i opis

W `Startup.Configure` metody akcji konfiguracji przekazany do `UseSwagger` metoda dodaje informacje, takie jak autor, licencji i opis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Interfejs użytkownika programu Swagger Wyświetla informacje o wersji:

![Interfejs użytkownika struktury swagger z informacjami o wersji](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>komentarze XML

Komentarze XML są włączone z następujących metod:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**
* Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** kartę

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)

* Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**
* Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Ręcznie dodaj poniższy fragment do *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a>Adnotacje danych

::: moniker range="<= aspnetcore-2.0"
Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i zalecanym typ zwracany dla akcji interfejsu API sieci web jest [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). W rezultacie NSwag nie można wywnioskować czynności akcję i ją zwraca. Rozważmy następujący przykład:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Poprzedni zwraca akcji `IActionResult`, ale wewnątrz działania aplikacja zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) lub [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Adnotacje danych służą do Poinformuj klientów kodów stanu HTTP ta akcja jest znany do zwrócenia. Dekoracji działanie z następującymi atrybutami:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i zalecanym typ zwracany dla akcji interfejsu API sieci web jest [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). W rezultacie NSwag można tylko wywnioskować zwracanego typu określone przez `T`. Nie można wywnioskować innych możliwych typów zwracanych w akcji. Rozważmy następujący przykład:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

Poprzedni zwraca akcji `ActionResult<T>`, ale wewnątrz działania aplikacja zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Ponieważ kontrolerowi zostanie nadany [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu, [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) zbyt odpowiedzi jest możliwe. Zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses) Aby uzyskać więcej informacji. Adnotacje danych służą do Poinformuj klientów kodów stanu HTTP ta akcja jest znany do zwrócenia. Dekoracji działanie z następującymi atrybutami:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

Swagger generator teraz można dokładnie opisują tę akcję, a wygenerowanego klienci wiedzieli, co otrzymują podczas wywoływania metody punktu końcowego. Zdecydowanie zalecane jest dekoracji wszystkie akcje te atrybuty. Aby uzyskać wskazówki dotyczące odpowiedzi HTTP, jakie powinna zwrócić operacji interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
