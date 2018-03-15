---
title: Rozpoczynanie pracy z Swashbuckle
author: zuckerthoben
description: "Ten samouczek zawiera wskazówki dodawania Swashbuckle do projektu do integracji interfejsu użytkownika programu Swagger."
keywords: Platformy ASP.NET Core, programu Swagger, Swashbuckle, strony, interfejsu API sieci Web pomocy
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: aaeaf9b776a5fd98900f2adf04468becec216da0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-swashbuckle"></a>Rozpoczynanie pracy z Swashbuckle

Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)

Istnieją trzy główne składniki do Swashbuckle:

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby ujawnić `SwaggerDocument` obiektów jako punkty końcowe JSON.

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generator Swagger, która tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli. Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejs użytkownika programu Swagger, które będą interpretowane przez JSON programu Swagger do tworzenia zaawansowanych, można dostosować środowisko dla opisu funkcji interfejsu API sieci Web. Obejmuje on przewodów wbudowanych testu dla metod publicznych.

## <a name="package-installation"></a>Instalacja pakietu

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

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakiety* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** listy rozwijanej "nuget.org"
* W polu wyszukiwania wprowadź Swashbuckle.AspNetCore
* Wybierz pakiet Swashbuckle.AspNetCore w okienku wyniki, a następnie kliknij przycisk **Dodawanie pakietu**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowane Terminal**:

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger

Dodaj Swagger generator kolekcji usług w `Startup.ConfigureServices` metody:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Importowanie następujących przestrzeni nazw w `Info` klasy:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

W `Startup.Configure` metody, włącza oprogramowanie pośredniczące do obsługi interfejsu użytkownika programu Swagger i wygenerowanego dokumentów JSON:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Uruchom aplikację i przejdź do `http://localhost:<random_port>/swagger/v1/swagger.json`. Wygenerowany opisujący punktów końcowych, które pojawia się, jak pokazano w [Swagger specyfikacji (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).

Interfejs użytkownika programu Swagger można znaleźć w folderze `http://localhost:<random_port>/swagger`. Teraz można eksplorować przez interfejs użytkownika programu Swagger interfejsu API i uwzględnić go w innych programów.

## <a name="customize--extend"></a>Dostosowywanie i rozszerzanie

Struktury swagger zawiera opcje dokumentowanie model obiektów i dostosowywanie interfejsu użytkownika do dopasowania motywu.

### <a name="api-info-and-description"></a>Informacje o interfejsu API i opis

Akcja konfiguracji przekazany do `AddSwaggerGen` metody można użyć do dodawania informacje, takie jak autor, licencji i opis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?range=20-30,36)]

Poniższa ilustracja przedstawia interfejsu użytkownika programu Swagger, wyświetlane informacje o wersji:

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autora i użyj łącza więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>komentarze XML

Komentarze XML można włączyć z następujących metod:

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

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?range=7-9)]

---

Włączanie komentarze XML zawiera informacje o debugowaniu nieudokumentowanej typy publiczne i elementów członkowskich. Nieudokumentowanej typy i składniki są oznaczone komunikat ostrzegawczy. Na przykład następujący komunikat wskazuje naruszenie kod ostrzeżenia 1591:

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

Pomijanie ostrzeżeń, definiując rozdzielaną średnikami listę kodów Ostrzeżenie można zignorować w *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?name=snippet_SuppressWarnings&highlight=3)]

Konfigurowanie programu Swagger, aby użyć wygenerowanego pliku XML. Dla systemu Linux lub systemów operacyjnych z systemem innym niż Windows może być uwzględniana wielkość liter nazwy pliku i ścieżki. Na przykład *TodoApi.Swashbuckle.XML* pliku jest prawidłowy dla systemu Windows, ale nie CentOS.

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

W powyższym kodzie `ApplicationBasePath` pobiera podstawowa ścieżka aplikacji. Podstawowa ścieżka jest używana do lokalizowania komentarze pliku XML. *TodoApi.Swashbuckle.xml* działa tylko w tym przykładzie, ponieważ nazwa wygenerowanego kodu XML komentarze pliku opiera się na nazwę aplikacji.

Dodawanie komentarzy potrójnym ukośnikiem do metody podnosi poziom interfejsu użytkownika programu Swagger, dodawanie opisu do Nagłówek sekcji:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

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

Dodaj [ \<Uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) znacznika `Create` dokumentacji metody akcji. Uzupełnia on informacje określone w [ \<podsumowania >](/dotnet/csharp/programming-guide/xmldoc/summary) tagu i zapewnia bardziej niezawodne interfejsu użytkownika programu Swagger. `<remarks>` Zawartości tagu może składać się tekst JSON i XML.

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Zwróć uwagę, rozszerzenia interfejsu użytkownika z te dodatkowe komentarze.

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Adnotacje danych

Dekoracji modelu z atrybutami, znalezione w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, ułatwiające dysków składniki interfejs użytkownika programu Swagger.

Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

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

Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API. Jej celem jest, aby zadeklarować, że akcje kontrolera obsługuje typ zawartości odpowiedzi *application/json*:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako domyślny dla akcji Pobierz kontrolera:

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Jak zwiększa użycie adnotacji danych w interfejsie API sieci Web, strony staną się bardziej opisowe i przydatne pomocy interfejsu użytkownika i interfejsu API.

### <a name="describe-response-types"></a>Opis typów odpowiedzi

Deweloperzy odbierająca komunikaty są najbardziej związane z wartością zwróconą&mdash;w szczególności typy odpowiedzi i kody błędów (o ile nie standard). Te są obsługiwane w adnotacjach komentarzy i danych XML.

`Create` Akcji zwraca `201 Created` w przypadku powodzenia lub `400 Bad Request` gdy treść żądania przesłanego ma wartość null. Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników. Ten problem został rozwiązany przez dodanie wyróżnione wiersze w następującym przykładzie:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Interfejs użytkownika programu Swagger teraz dokumentów wyraźnie oczekiwanego kody odpowiedzi HTTP:

![Swagger interfejsu użytkownika przedstawiający opis klasy odpowiedź POST "Zwraca nowo utworzony element Todo" i "400 — Jeśli element ma wartość null" dla kodu stanu i przyczyna w obszarze wiadomości odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a>Dostosowywanie interfejsu użytkownika

Zasoby interfejsu użytkownika jest funkcjonalności i presentable; Jednak podczas kompilowania stron z dokumentacją do interfejsu API, ma się reprezentować motywu lub marki, na których. Wykonywanie zadań tego zadania ze składnikami Swashbuckle wymaga Dodawanie zasobów do obsługi plików statycznych, a następnie tworzenie struktury folderów do obsługi tych plików.

Jeśli przeznaczonych dla platformy .NET Framework lub .NET Core 1.x, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) pakiet NuGet do projektu:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Poprzedni pakiet NuGet jest już zainstalowana, jeśli przeznaczonych dla platformy .NET Core 2.x i przy użyciu [metapackage](xref:fundamentals/metapackage).

Włącza oprogramowanie pośredniczące plików statycznych:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

Uzyskać zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Ten folder zawiera zasoby niezbędne dla strony interfejs użytkownika programu Swagger.

Utwórz *wwwroot/swagger/interfejsu użytkownika* folderu i skopiuj do niego zawartość *dist* folderu.

Utwórz *wwwroot/swagger/ui/css/custom.css* pliku z następujących CSS, aby dostosować nagłówka strony:

[!code-css[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/css/custom.css)]

Odwołanie *custome.CSS* w *index.html* pliku:

[!code-html[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?range=14)]

Przejdź do *index.html* pod `http://localhost:<random_port>/swagger/ui/index.html`. Wprowadź `http://localhost:<random_port>/swagger/v1/swagger.json` w nagłówku pola tekstowego i kliknij **Eksploruj** przycisku. Wynikowa strona wygląda następująco:

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

Jest znacznie więcej można zrobić za pomocą strony. Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).
