---
title: Wprowadzenie do pakietu Swashbuckle i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak dodać pakiet Swashbuckle do swojego projektu interfejsu API sieci web platformy ASP.NET Core, aby zintegrować interfejs użytkownika struktury Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/04/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: ebbdfa4cfeb3013cd961167439dd261f2e058a4b
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087613"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a>Wprowadzenie do pakietu Swashbuckle i ASP.NET Core

Przez [Shayne Boyer](https://twitter.com/spboyer) i [Scott Addie](https://twitter.com/Scott_Addie)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Istnieją trzy główne składniki do narzędzia Swashbuckle:

* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów programu Swagger i oprogramowaniu pośredniczącym, aby udostępnić `SwaggerDocument` obiektów jako punkty końcowe w formacie JSON.

* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): generatora struktury Swagger, który tworzy `SwaggerDocument` obiektów bezpośrednio z tras, kontrolerów i modeli. Zazwyczaj jest połączona z oprogramowaniem pośredniczącym punktu końcowego struktury Swagger można automatycznie udostępnić JSON programu Swagger.

* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejs użytkownika struktury Swagger. Interpretuje JSON programu Swagger do tworzenia rozbudowanych, możliwych do dostosowania środowisko do opisywania funkcje interfejsu API sieci web. Obejmuje ona wiązka testów wbudowanych dla metody publiczne.

## <a name="package-installation"></a>Instalacja pakietu

Pakiet Swashbuckle mogą być dodawane przy użyciu następujących metod:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konsola Menedżera pakietów** okna:
  * Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**
  * Przejdź do katalogu, w którym *TodoApi.csproj* plik istnieje
  * Wykonaj następujące polecenie:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Z **Zarządzaj pakietami NuGet** okno dialogowe:
  * Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**
  * Ustaw **źródła pakietu** na stronie "nuget.org"
  * W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"
  * Wybierz pakiet "Swashbuckle.AspNetCore" z **Przeglądaj** kartę, a następnie kliknij przycisk **instalacji**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...**
* Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org"
* W polu wyszukiwania wprowadź "Swashbuckle.AspNetCore"
* Wybierz pakiet "Swashbuckle.AspNetCore" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowany Terminal**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Dodawanie i konfigurowanie oprogramowania pośredniczącego struktury Swagger

Importowanie następująca przestrzeń nazw do użycia `Info` klasy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

Dodaj generatora struktury Swagger do kolekcji usługi w `Startup.ConfigureServices` metody:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

W `Startup.Configure` metody włącza oprogramowanie pośredniczące dla obsługująca wygenerowane dokumentów JSON i interfejs użytkownika struktury Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

Poprzedni `UseSwaggerUI` umożliwia wywołanie metody [oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files). Jeśli przeznaczony dla .NET Framework lub .NET Core 1.x, należy dodać [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakiet NuGet do projektu.

Uruchom aplikację i przejdź do `http://localhost:<port>/swagger/v1/swagger.json`. Wygenerowany opisujący punktów końcowych, które pojawia się, jak pokazano na [(swagger.json) specyfikacjami struktury Swagger](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).

Interfejs użytkownika struktury Swagger, można znaleźć w folderze `http://localhost:<port>/swagger`. Poznaj interfejs API poprzez interfejs użytkownika struktury Swagger, a następnie Uwzględnij ją w innych programach.

> [!TIP]
> Aby obsługiwać interfejs użytkownika struktury Swagger w katalogu głównym aplikacji (`http://localhost:<port>/`) ustaw `RoutePrefix` właściwości na pusty ciąg:
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

Jeśli przy użyciu katalogów w usługach IIS lub zwrotny serwer proxy punktu końcowego struktury Swagger za pomocą ścieżki względnej `./` prefiks. Na przykład `./swagger/v1/swagger.json`. Za pomocą `/swagger/v1/swagger.json` instruuje aplikację, aby wyszukać plik JSON w katalogu głównym PRAWDA adres URL (plus prefiks trasy, jeśli używane). Na przykład użyć `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` zamiast `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.

## <a name="customize-and-extend"></a>Dostosowywanie i rozszerzanie

Struktury swagger zawiera opcje dokumentowanie model obiektu i dostosowywanie interfejsu użytkownika do dopasowania Twój wybrany motyw.

### <a name="api-info-and-description"></a>Informacje o interfejsie API i opis

Akcja konfiguracji są przekazywane do `AddSwaggerGen` metoda dodaje informacje, takie jak tworzenie, licencji i opis:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

Interfejs użytkownika struktury Swagger Wyświetla informacje o wersji:

![Interfejs użytkownika struktury swagger z informacjami o wersji: opis, autorem i zobacz link więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>komentarze XML

Komentarze XML można włączyć za pomocą następujących metod:

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **edytowanie pliku .csproj < project_name >**.
* Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**.
* Sprawdź **pliku dokumentacji XML** pole w obszarze **dane wyjściowe** części **kompilacji** kartę.

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* Z *konsoli rozwiązania*, naciśnij klawisz **kontroli** i kliknij nazwę projektu. Przejdź do **narzędzia** > **Edytuj plik**.
* Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Otwórz **opcje projektu** okna dialogowego > **kompilacji** > **kompilatora**
* Sprawdź **Generuj dokumentację xml** pole w obszarze **ogólne opcje** sekcji

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Ręcznie Dodaj wyróżnione wiersze w celu *.csproj* pliku:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

Włączanie komentarzy XML zawiera informacje o debugowaniu nieudokumentowane typy publiczne i elementy członkowskie. Nieudokumentowany typy i elementy członkowskie są wskazywane przez komunikat ostrzegawczy. Na przykład następujący komunikat wskazuje naruszenie kod ostrzegawczy 1591:

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

Aby pominąć ostrzeżenia całego projektu, należy zdefiniować rozdzieloną średnikami listę kodów błędów do zignorowania w pliku projektu. Dołączanie kodów `$(NoWarn);` stosuje [ C# wartości domyślne](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) zbyt.

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

Aby pominąć ostrzeżenia tylko dla określonych członków, należy wpisać kod w [ostrzeżenie #pragma](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) dyrektywy preprocesora. To podejście jest przydatne w przypadku kodu, który nie powinien być udostępniane za pośrednictwem dokumentacji interfejsu API. W poniższym przykładzie, kod ostrzegawczy CS1591 jest ignorowany dla całego `Program` klasy. Wymuszanie kod ostrzegawczy zostanie przywrócony na koniec definicji klasy. Określ wiele kodów ostrzeżenie listę rozdzielonych przecinkami.

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

Skonfiguruj strukturę Swagger, aby użyć pliku XML, który jest generowany przy użyciu poprzednich instrukcji. Dla systemu Linux lub w systemach operacyjnych innych niż Windows może być uwzględniana wielkość liter nazw plików oraz ścieżek. Na przykład *TodoApi.XML* plik jest prawidłowy w Windows, ale nie CentOS.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

W poprzednim kodzie [odbicia](/dotnet/csharp/programming-guide/concepts/reflection) jest używany do tworzenia nazwę pliku XML dopasowania, projekt interfejsu API sieci web. [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) właściwość jest używana do konstruowania ścieżkę do pliku XML. Niektóre funkcje programu Swagger (na przykład wypełniana parametrów wejściowych lub metody HTTP i kodów odpowiedzi z odpowiednich atrybutów) działa bez użycia pliku dokumentacji XML. W przypadku większości funkcji, mianowicie podsumowania metody i opisy parametrów i kodów odpowiedzi korzystanie z pliku XML jest obowiązkowe.

Dodawanie komentarze z potrójnym ukośnikiem akcję zwiększa interfejsu użytkownika programu Swagger, dodając opis do nagłówku sekcji. Dodaj [ \<podsumowania >](/dotnet/csharp/programming-guide/xmldoc/summary) element powyżej `Delete` akcji:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

Interfejs użytkownika struktury Swagger zawiera tekst wewnętrzny dla poprzedniego kodu `<summary>` elementu:

![Wyświetlanie komentarza XML "Usuwa określone TodoItem". interfejs użytkownika struktury swagger w przypadku metody DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

Interfejs użytkownika jest wymuszany przez wygenerowany schemat JSON:

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

Dodaj [ \<Uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu `Create` dokumentacji metody akcji. Uzupełnia artykuł informacji o określonych w `<summary>` elementu i zapewnia bardziej niezawodny interfejs użytkownika struktury Swagger. `<remarks>` Element zawartości może zawierać tekstu, JSON lub XML.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

Zwróć uwagę, ulepszenia interfejsu użytkownika za pomocą te dodatkowe uwagi:

![Interfejs użytkownika struktury swagger z pokazane dodatkowe komentarze](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Adnotacje danych

Dekoracji modelu z atrybutów, w [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) przestrzeni nazw, oferuje składniki interfejs użytkownika struktury Swagger.

Dodaj `[Required]` atrybutu `Name` właściwość `TodoItem` klasy:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

Obecność tego atrybutu zmienia zachowanie interfejsu użytkownika i zmienia schemat JSON źródłowy:

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

Dodaj `[Produces("application/json")]` atrybutu Kontroler interfejsu API. Jej celem jest, aby zadeklarować, że akcji kontrolera obsługuje typ zawartości odpowiedzi *application/json*:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

**Typ zawartości odpowiedzi** listy rozwijanej wybiera tego typu zawartości jako wartość domyślna dla akcji Pobierz kontrolera:

![Interfejs użytkownika struktury swagger z domyślny typ zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

W miarę zwiększania użycia adnotacje danych w interfejsie API sieci web interfejsu API i interfejsu użytkownika strony, stają się bardziej opisowy i przydatne pomocy.

### <a name="describe-response-types"></a>Opis typów odpowiedzi

Deweloperom korzystanie z interfejsu API sieci web są najbardziej interesujących co to jest zwracany&mdash;specjalnie typów odpowiedzi i kody błędów (o ile nie standard). Typy odpowiedzi i kody błędów są wskazywane w adnotacjach komentarze i dane XML.

`Create` Akcji zwraca kod stanu 201 protokołu HTTP w przypadku powodzenia. Treść żądania przesłane ma wartość null zwracany jest kod stanu HTTP 400. Bez prawidłowego dokumentacji w Interfejsie użytkownika programu Swagger użytkownik nie ma wiedzy na temat tych oczekiwanych wyników. Rozwiązać ten problem, dodając wyróżnione wiersze w następującym przykładzie:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

Teraz interfejs użytkownika struktury Swagger dokumenty wyraźnie oczekiwanego kody odpowiedzi HTTP:

![Wyświetlanie opis klasy odpowiedzi WPIS "Zwraca nowo utworzony element Todo" interfejs użytkownika struktury swagger i "400 - Jeśli element ma wartość null" dla kodu stan i przyczyna w komunikatach odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

W programie ASP.NET Core 2.2 lub nowszej, konwencje mogą być używane jako alternatywa jawnie urządzanie poszczególne akcje za pomocą `[ProducesResponseType]`. Aby uzyskać więcej informacji, zobacz <xref:web-api/advanced/conventions>.

::: moniker-end

### <a name="customize-the-ui"></a>Dostosowywanie interfejsu użytkownika

Zasoby interfejsu użytkownika jest funkcjonalności i zawartości. Jednak strony dokumentacji interfejsu API powinno reprezentować Twojej marki lub motywu. Składniki pakietu Swashbuckle znakowania wymaga dodawania zasobów do obsługi plików statycznych i tworzenia struktury folderów do obsługi tych plików.

Jeśli przeznaczony dla .NET Framework lub .NET Core 1.x, należy dodać [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) pakiet NuGet do projektu:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Poprzedni pakiet NuGet jest już zainstalowany, jeśli przeznaczony dla platformy .NET Core 2.x i przy użyciu [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).

Włącz oprogramowanie pośredniczące plików statycznych:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

Uzyskiwanie zawartość *dist* z folderu [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist). Ten folder zawiera zasoby wymagane dla strony interfejs użytkownika struktury Swagger.

Tworzenie *wwwroot/swagger/ui* folderu i skopiuj do niego zawartość *dist* folderu.

Tworzenie *custome.CSS* pliku w *wwwroot/swagger/ui*, za pomocą następujących CSS, aby dostosować nagłówek strony:

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

Odwołanie *custome.CSS* w *index.html* pliku po wszystkich innych plików CSS:

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

Przejdź do *index.html* pod `http://localhost:<port>/swagger/ui/index.html`. Wprowadź `http://localhost:<port>/swagger/v1/swagger.json` w polu tekstowym w nagłówku i kliknij **Eksploruj** przycisku. Wynikowa strona wygląda następująco:

![Interfejs użytkownika struktury swagger z tytułem niestandardowego nagłówka](web-api-help-pages-using-swagger/_static/custom-header.png)

Jest znacznie więcej można zrobić ze stroną. Zobacz pełne możliwości zasoby interfejsu użytkownika na [repozytorium GitHub interfejsu użytkownika programu Swagger](https://github.com/swagger-api/swagger-ui).
