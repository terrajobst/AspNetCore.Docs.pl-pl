---
title: Rozpoczynanie pracy z NSwag
author: zuckerthoben
description: "Ten samouczek zawiera wskazówki dodawania NSwag do generowania dokumentacji i strony dla aplikacji interfejsu API sieci Web pomocy."
keywords: Platformy ASP.NET Core, programu Swagger, NSwag, strony, interfejsu API sieci Web pomocy
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 70923210ef8f5b23ddd3cc74efceb05cbd90a179
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="get-started-with-nswag"></a>Rozpoczynanie pracy z NSwag

Przez [Christoph Nienaber](https://twitter.com/zuckerthoben) i [Rico Suter](https://rsuter.com)

Przy użyciu [NSwag](https://github.com/RSuter/NSwag) z platformy ASP.NET Core wymaga oprogramowania pośredniczącego [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) pakietu NuGet. Pakiet składa się z generator Swagger, interfejs użytkownika programu Swagger (v2 i v3), i [interfejsu użytkownika ReDoc](https://github.com/Rebilly/ReDoc).

Zdecydowanie zaleca się korzystać z jego NSwag możliwości generowania kodu. Podczas generowania kodu, wybierz jedną z następujących opcji:

* Użyj [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), aplikacji pulpitu systemu Windows podczas generowania kodu klienta w języku C# i TypeScript do interfejsu API
* Użyj [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) lub [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pakietów NuGet do kodu generowania wewnątrz projektu
* Użyj NSwag z [wiersza polecenia](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Użyj [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) pakietu NuGet

## <a name="features"></a>Funkcje

Głównym celem używania NSwag jest możliwość wprowadzenia nie tylko interfejs użytkownika programu Swagger i struktury Swagger generator, jak używać funkcji generowania kodu elastyczne. Nie ma potrzeby istniejącego interfejsu API&mdash;można użyć interfejsów API innych firm, które włączają Swagger i umożliwić NSwag Generowanie implementacja klienta. W obu przypadkach jest przyspieszonego cyklu programowanie i łatwiej można dostosować do zmian interfejsu API.

## <a name="package-installation"></a>Instalacja pakietu

Pakiet NSwag NuGet można dodać z następujących metod:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konsola Menedżera pakietów** okno:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Z **Zarządzaj pakietami NuGet** okna dialogowego:
  * Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
  * Ustaw **źródła pakietu** do "nuget.org"
  * W polu wyszukiwania wprowadź "NSwag.AspNetCore"
  * Wybierz pakiet "NSwag.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"
* W polu wyszukiwania wprowadź NSwag.AspNetCore
* Wybierz pakiet NSwag.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowane Terminal**:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

# <a name="add-and-configure-swagger-middleware"></a>Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger

Importowanie następujących przestrzeni nazw w `Info` klasy:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

W `Startup.Configure` metody, włącza oprogramowanie pośredniczące dla obsługująca specyfikację struktury Swagger generowane i interfejsu użytkownika programu Swagger:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Uruchom aplikację. Przejdź do `/swagger` do wyświetlania interfejsu użytkownika programu Swagger. Przejdź do `/swagger/v1/swagger.json` Aby wyświetlić specyfikację struktury Swagger.

## <a name="code-generation"></a>Generowanie kodu

### <a name="via-nswagstudio"></a>Via NSwagStudio

* Zainstaluj `NSwagStudio` z oficjalnego [repozytorium GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio).

* Launch NSwagStudio. Wprowadź lokalizację użytkownika *swagger.json* lub skopiuj go bezpośrednio:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Wskazuje typ danych wyjściowych żądaną klienta. Dostępne są następujące opcje **klienta TypeScript**, **klienta CSharp**, lub **Kontroler interfejsu API sieci Web CSharp**. Przy użyciu kontrolera interfejsu API sieci Web jest zasadniczo reverse generacji. Specyfikacja usługi używa odbudować usługi.

* Kliknij przycisk **Generowanie danych wyjściowych**.

* W tym miejscu wyświetlić implementację klienta pełną *TodoApi.NSwag* przykład w języku C#:

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Umieść plik do projektu klienta (na przykład [platformy Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/) aplikacji). Rozpocznij korzystanie z interfejsu API:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Podstawowy adres URL i/lub klient HTTP można wprowadzić do klienta interfejsu API. Najlepszym rozwiązaniem jest zawsze [ponowne użycie HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

Można teraz uruchomić łatwe Implementowanie interfejsu API do projektów klienckich.

### <a name="other-ways-to-generate-client-code"></a>Inne sposoby generowanie kodu klienta

Można wygenerować kod w inny sposób więcej nadaje się do przepływu pracy:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [W kodzie](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Szablony T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Dostosowywanie

### <a name="xml-comments"></a>komentarze XML

Komentarze XML są włączone z następujących metod:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml)

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**
* Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml)

* Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**
* Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml)

Ręcznie dodaj poniższy fragment do *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

---

## <a name="data-annotations"></a>Adnotacje danych

Używa NSwag [odbicia](/dotnet/csharp/programming-guide/concepts/reflection), i jest najlepszym rozwiązaniem dla interfejsu API sieci Web akcji do zwrócenia [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). W rezultacie NSwag nie można wywnioskować czynności akcję i ją zwraca. Rozważmy następujący przykład:

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

Poprzedni zwraca akcji `IActionResult`, ale wewnątrz działania aplikacja zwraca albo [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) lub [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Adnotacje danych służą do Poinformuj klientów odpowiedzi HTTP, które zwraca tej akcji. Dekoracji działanie z następującymi atrybutami:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Swagger generator teraz można dokładnie opisują tę akcję, a wygenerowanego klienci wiedzieli, co otrzymują podczas wywoływania metody punktu końcowego. Zdecydowanie zalecane jest dekoracji wszystkie akcje te atrybuty. Aby uzyskać wskazówki dotyczące odpowiedzi HTTP, jakie powinna zwrócić operacji interfejsu API, zobacz [specyfikacji RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
