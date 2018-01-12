---
title: "ASP.NET Core stron sieci Web interfejsu API pomocy przy użyciu programu Swagger"
author: spboyer
description: "Ten samouczek zawiera wskazówki dodawania programu Swagger do generowania dokumentacji i strony dla aplikacji interfejsu API sieci Web pomocy."
keywords: Platformy ASP.NET Core, programu Swagger, Swashbuckle, strony, interfejsu API sieci Web pomocy
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 08503b724aaea64ad2d32eaa710378ec77b9a1fe
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>ASP.NET Core stron sieci Web interfejsu API pomocy przy użyciu programu Swagger

<a name="web-api-help-pages-using-swagger"></a>

Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)

Opis różnych metod interfejsu API może być wyzwaniem dla dewelopera podczas kompilowania odbierającą aplikację.

Generowanie dobrej stron pomocy i dokumentacja dla interfejsu API sieci Web, przy użyciu [Swagger](https://swagger.io/) z implementacją .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), wystarczy kilka pakietów NuGet Dodawanie i modyfikowanie *Startup.cs*.

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) to projekt open source służący do generowania dokumentów programu Swagger dla interfejsów API platformy ASP.NET Core sieci Web.

* [Swagger](https://swagger.io/) czytelną reprezentacja interfejsu API RESTful, umożliwiającą obsługę interakcyjne dokumentacji, generowanie zestawów SDK klientów i odnajdywania.

W tym samouczku opiera się na przykład [budynku swój pierwszy interfejsu API sieci Web platformy ASP.NET Core MVC i Visual Studio](xref:tutorials/first-web-api). Jeśli chcesz z niego skorzystać, Pobierz przykład na [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Wprowadzenie

Istnieją trzy główne składniki do Swashbuckle:

* `Swashbuckle.AspNetCore.Swagger`: model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.

* `Swashbuckle.AspNetCore.SwaggerGen`: generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli. Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.

* `Swashbuckle.AspNetCore.SwaggerUI`: wbudowana wersja narzędzia interfejs użytkownika programu Swagger, które będą interpretowane przez JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web. Obejmuje on przewodów wbudowanych testu dla metod publicznych.

## <a name="nuget-packages"></a>Pakiety NuGet

Pakiet Swashbuckle można dodać z następujących metod:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konsola Menedżera pakietów** okno:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Z **Zarządzaj pakietami NuGet** okna dialogowego:

     * Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
     * Ustaw **źródła pakietu** do "nuget.org"
     * W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"
     * Wybierz pakiet "Swashbuckle.AspNetCore" **Przeglądaj** i kliknij polecenie **instalacji**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"
* W polu wyszukiwania wprowadź Swashbuckle.AspNetCore
* Wybierz pakiet Swashbuckle.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Kod Visual Studio](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowane Terminal**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Dodawanie i konfigurowanie programu Swagger do oprogramowania pośredniczącego

Dodaj Swagger generator kolekcji usług w `ConfigureServices` metody *Startup.cs*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Dodaj następujący kod za pomocą instrukcji dla `Info` klasy:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

W `Configure` metody *Startup.cs*, włącza oprogramowanie pośredniczące dla obsługująca wygenerowanego dokument JSON i SwaggerUI:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Uruchom aplikację i przejdź do `http://localhost:<random_port>/swagger/v1/swagger.json`. Pojawi się wygenerowanym opisujący punktów końcowych.

**Uwaga:** Microsoft Edge, Google Chrome i Firefox wyświetlanie dokumentów JSON natywnie. Brak rozszerzenia dla programu Chrome, które formatowania dokumentu, aby ułatwić czytanie. *Poniższy przykład, zostanie zmniejszona do skrócenia.*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

Ten dokument dyski interfejsu użytkownika programu Swagger, który można wyświetlić, przechodząc do `http://localhost:<random_port>/swagger`:

![Interfejs użytkownika struktury swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Każda metoda akcji publicznego w `TodoController` można przetestować w interfejsie użytkownika. Kliknij nazwę metody, aby rozwinąć sekcję. Dodaj wszystkie niezbędne parametry, a następnie kliknij przycisk "Wypróbuj ją!".

![Przykład struktury Swagger UZYSKAĆ testu](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Dostosowywanie & rozszerzalności

Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.

### <a name="api-info-and-description"></a>Informacje o interfejsu API i opis

Akcja konfiguracji przekazany do `AddSwaggerGen` metody można użyć do dodawania informacje, takie jak autor, licencji i opis:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

Poniższa ilustracja przedstawia interfejsu użytkownika programu Swagger, wyświetlane informacje o wersji:

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>Komentarze XML

Komentarze XML można włączyć z następujących metod:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**
* Sprawdź **pliku dokumentacji XML** obszarze **dane wyjściowe** sekcji **kompilacji** karty:

![Na karcie właściwości projektu kompilacji](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**
* Sprawdź **Generowanie dokumentacji xml** obszarze **Opcje ogólne** sekcji:

![Sekcja Opcje ogólne opcje projektu](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Kod Visual Studio](#tab/visual-studio-code)

Ręcznie dodaj poniższy fragment do *.csproj* pliku:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Zobacz kod programu Visual Studio.

---

Włączanie komentarze XML zawiera informacje o debugowaniu nieudokumentowanej typy publiczne i elementów członkowskich. Nieudokumentowanej typy i składniki są oznaczone komunikat ostrzegawczy: *komentarz Brak XML dla widocznego publicznie typu lub elementu członkowskiego*.

Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML. Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki. Na przykład *ToDoApi.XML* będzie można znaleźć pliku w systemie Windows, ale nie CentOS.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

W powyższym kodzie `ApplicationBasePath` pobiera podstawowa ścieżka aplikacji. Podstawowa ścieżka jest używana do lokalizowania komentarze pliku XML. *TodoApi.xml* działa tylko w tym przykładzie, ponieważ nazwa wygenerowanego kodu XML komentarze pliku opiera się na nazwę aplikacji.

Dodawanie komentarzy potrójnym ukośnikiem do metody podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do Nagłówek sekcji:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Wyświetlanie komentarza XML "Usuwa określonych TodoItem". interfejs użytkownika struktury swagger dla metody DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Interfejs użytkownika jest wymuszany przez wygenerowanego pliku JSON, który zawiera również te komentarze:

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

Dodaj [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) znacznika `Create` dokumentacji metody akcji. Uzupełnia on informacje określone w `<summary>` tagu i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger. `<remarks>` Zawartości tagu może składać się tekst JSON i XML.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe komentarze.

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Adnotacji danych

Dekoracji modelu z atrybutami, znalezione w `System.ComponentModel.DataAnnotations`, aby pomóc dysków składniki interfejs użytkownika programu Swagger.

Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

Obecność tego atrybutu zmiany zachowania interfejsu użytkownika i zmienia podstawowej schematu JSON:

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API. Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zwracany typ zawartości *application/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.

### <a name="describing-response-types"></a>Opisujący typy odpowiedzi

Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą &mdash; w szczególności typy odpowiedzi i kody błędów (o ile nie standard). Te są obsługiwane w adnotacjach komentarzy i danych XML.

`Create` Akcji zwraca `201 Created` w przypadku powodzenia lub `400 Bad Request` gdy treść żądania przesłanego ma wartość null. Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników. Ten problem został rozwiązany przez dodanie wyróżnione wiersze w następującym przykładzie:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Dostosowywanie interfejsu użytkownika

Zasoby interfejsu użytkownika jest funkcjonalności i presentable; Jednak podczas kompilowania stron z dokumentacją do interfejsu API, ma się reprezentować motywu lub marki, na których. Wykonywanie zadań tego zadania ze składnikami Swashbuckle wymaga Dodawanie zasobów do obsługi plików statycznych, a następnie tworzenie struktury folderów do obsługi tych plików.

Jeśli przeznaczonych dla platformy .NET Framework, Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Włącza oprogramowanie pośredniczące plików statycznych:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.

Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.

Utwórz *wwwroot/swagger/ui/css/custom.css* pliku z następujących CSS, aby dostosować nagłówka strony:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Odwołanie *custome.CSS* w *index.html* pliku:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Przejdź do *index.html* pod `http://localhost:<random_port>/swagger/ui/index.html`. Wprowadź `http://localhost:<random_port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku. Wynikowa strona wygląda następująco:

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

Jest znacznie więcej można zrobić za pomocą strony. Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).
