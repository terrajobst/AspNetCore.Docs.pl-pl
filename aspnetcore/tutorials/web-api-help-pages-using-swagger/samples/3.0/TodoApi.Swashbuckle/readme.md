---
page_type: sample
description: Dowiedz się, jak dodać Swashbuckle do projektu interfejsu API sieci Web ASP.NET Core, aby zintegrować interfejs użytkownika struktury Swagger.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
- vs-code
- vs-mac
urlFragment: getstarted-swashbuckle-aspnetcore
ms.openlocfilehash: e02247325f430b0ce23dbb3f5bc344a60a1a164a
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879727"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="b5a3a-102">Wprowadzenie do Swashbuckle i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5a3a-102">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="b5a3a-103">Podczas korzystania z internetowego interfejsu API, informacje o jego różne metody może stanowić wyzwanie dla dewelopera.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-103">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="b5a3a-104">[Struktury swagger](https://swagger.io/), znane również jako [OpenAPI](https://www.openapis.org/), rozwiązuje problem Generowanie przydatne stron pomocy i dokumentacji dla interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-104">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="b5a3a-105">Zapewnia korzyści, takich jak dokumentacja interaktywne, generowanie zestawów SDK klienta i odnajdywania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-105">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="b5a3a-106">W tym przykładzie pokazano [Swashbuckle. AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) implementacja platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-106">In this sample, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) the .NET implementation is shown.</span></span>

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="b5a3a-107">Dodawanie i Konfigurowanie oprogramowania pośredniczącego programu Swagger</span><span class="sxs-lookup"><span data-stu-id="b5a3a-107">Add and configure Swagger middleware</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
    });
}
```

<span data-ttu-id="b5a3a-108">W metodzie `Startup.Configure` Włącz oprogramowanie pośredniczące do obsługi wygenerowanego dokumentu JSON i interfejsu użytkownika programu Swagger:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-108">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();

    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1"); 
    });

    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

<span data-ttu-id="b5a3a-109">Poprzednie wywołanie metody `UseSwaggerUI` włącza [oprogramowanie pośredniczące pliku statycznego](https://docs.microsoft.com/aspnet/core/fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b5a3a-109">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/static-files).</span></span> <span data-ttu-id="b5a3a-110">Jeśli obiektem docelowym jest .NET Framework lub .NET Core 1. x, Dodaj pakiet NuGet [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-110">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="b5a3a-111">Uruchom aplikację i przejdź do `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-111">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="b5a3a-112">Wygenerowany dokument opisujący punkty końcowe pojawia się, jak pokazano w [specyfikacji Swagger (Swagger. JSON)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="b5a3a-112">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="b5a3a-113">Interfejs użytkownika struktury Swagger można znaleźć pod adresem `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-113">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="b5a3a-114">Eksploruj interfejs API za pośrednictwem interfejsu użytkownika struktury Swagger i Uwzględnij go w innych programach.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-114">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="b5a3a-115">Aby obpracować interfejs użytkownika struktury Swagger w katalogu głównym aplikacji (`http://localhost:<port>/`), ustaw właściwość `RoutePrefix` na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-115">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> ```csharp
>app.UseSwaggerUI(c =>
>{
>    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
>    c.RoutePrefix = string.Empty;
>});
>```

<span data-ttu-id="b5a3a-116">W przypadku używania katalogów z usługami IIS lub zwrotnego serwera proxy Ustaw punkt końcowy struktury Swagger na ścieżkę względną przy użyciu prefiksu `./`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-116">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="b5a3a-117">Na przykład `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-117">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="b5a3a-118">Przy użyciu `/swagger/v1/swagger.json` nakazuje aplikacji wyszukanie pliku JSON w prawdziwym katalogu głównym adresu URL (plus prefiks trasy, jeśli jest używany).</span><span class="sxs-lookup"><span data-stu-id="b5a3a-118">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="b5a3a-119">Na przykład użyj `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json`, a nie `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-119">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="b5a3a-120">Dostosuj i rozwiń</span><span class="sxs-lookup"><span data-stu-id="b5a3a-120">Customize and extend</span></span>

<span data-ttu-id="b5a3a-121">Struktura Swagger zawiera opcje dokumentowania modelu obiektów i dostosowywania interfejsu użytkownika w celu dopasowania go do motywu.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-121">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

<span data-ttu-id="b5a3a-122">W klasie `Startup` Dodaj następujące przestrzenie nazw:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-122">In the `Startup` class, add the following namespaces:</span></span>
```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a><span data-ttu-id="b5a3a-123">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="b5a3a-123">API info and description</span></span>

<span data-ttu-id="b5a3a-124">Akcja konfiguracji przeniesiona do metody `AddSwaggerGen` dodaje informacje takie jak autor, licencja i opis:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-124">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

```csharp
// Register the Swagger generator, defining 1 or more Swagger documents
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Version = "v1",
        Title = "ToDo API",
        Description = "A simple example ASP.NET Core Web API",
        TermsOfService = new Uri("https://example.com/terms"),
        Contact = new OpenApiContact
        {
            Name = "Shayne Boyer",
            Email = string.Empty,
            Url = new Uri("https://twitter.com/spboyer"),
        },
        License = new OpenApiLicense
        {
            Name = "Use under LICX",
            Url = new Uri("https://example.com/license"),
        }
    });
});
```

<span data-ttu-id="b5a3a-125">Interfejs użytkownika struktury Swagger wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-125">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury Swagger z informacjami o wersji: Description, Author i linku więcej](sample_images/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="b5a3a-127">komentarze XML</span><span class="sxs-lookup"><span data-stu-id="b5a3a-127">XML comments</span></span>

<span data-ttu-id="b5a3a-128">Komentarze XML można włączyć przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-128">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5a3a-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5a3a-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b5a3a-130">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** a następnie wybierz polecenie **edytuj < Project_Name >. csproj**.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-130">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="b5a3a-131">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="b5a3a-131">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b5a3a-132">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="b5a3a-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b5a3a-133">W *okienko rozwiązania*naciśnij klawisz **Control** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-133">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="b5a3a-134">Przejdź do **menu narzędzia** > **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-134">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="b5a3a-135">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="b5a3a-135">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b5a3a-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b5a3a-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b5a3a-137">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="b5a3a-137">Manually add the highlighted lines to the *.csproj* file:</span></span>

```xml
<PropertyGroup>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

---

<span data-ttu-id="b5a3a-138">Włączenie komentarzy XML zapewnia informacje debugowania dla nieudokumentowanych typów publicznych i członków.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-138">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="b5a3a-139">Nieudokumentowane typy i elementy członkowskie są wskazywane przez komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-139">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="b5a3a-140">Na przykład następujący komunikat oznacza naruszenie kodu ostrzegawczego 1591:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-140">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="b5a3a-141">Aby pominąć ostrzeżenia dla całego projektu, należy zdefiniować rozdzielaną średnikami listę kodów ostrzeżeń do ignorowania w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-141">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="b5a3a-142">Dołączanie kodów ostrzeżeń do `$(NoWarn);` powoduje zastosowanie [ C# wartości domyślnych](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) .</span><span class="sxs-lookup"><span data-stu-id="b5a3a-142">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

```xml
<NoWarn>$(NoWarn);1591</NoWarn>
```

<span data-ttu-id="b5a3a-143">Aby pominąć ostrzeżenia tylko dla określonych elementów członkowskich, należy ująć kod w [#pragma ostrzeżenia](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocesora.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-143">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="b5a3a-144">Takie podejście jest przydatne w przypadku kodu, który nie powinien być ujawniony za pośrednictwem dokumentacji interfejsu API. W poniższym przykładzie kod ostrzegawczy CS1591 jest ignorowany dla całej klasy `Program`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-144">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="b5a3a-145">Wymuszanie kodu ostrzegawczego jest przywracane po zamknięciu definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-145">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="b5a3a-146">Określ wiele kodów ostrzeżeń z listą rozdzielaną przecinkami.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-146">Specify multiple warning codes with a comma-delimited list.</span></span>

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

<span data-ttu-id="b5a3a-147">Skonfiguruj strukturę Swagger, aby używała pliku XML, który jest generowany z poprzednimi instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-147">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="b5a3a-148">W przypadku systemów operacyjnych Linux lub innych niż Windows nazwy plików i ścieżki mogą być rozróżniane wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-148">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="b5a3a-149">Na przykład plik *TodoApi. XML* jest prawidłowy w systemie Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-149">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

```csharp
/// NOTE LAST 3 LINES IN THIS SNIPPET
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<TodoContext>(opt =>
        opt.UseInMemoryDatabase("TodoList"));
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo
        {
            Version = "v1",
            Title = "ToDo API",
            Description = "A simple example ASP.NET Core Web API",
            TermsOfService = new Uri("https://example.com/terms"),
            Contact = new OpenApiContact
            {
                Name = "Shayne Boyer",
                Email = string.Empty,
                Url = new Uri("https://twitter.com/spboyer"),
            },
            License = new OpenApiLicense
            {
                Name = "Use under LICX",
                Url = new Uri("https://example.com/license"),
            }
        });

        // Set the comments path for the Swagger JSON and UI.
        var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
        var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
        c.IncludeXmlComments(xmlPath);
    });
}
```

<span data-ttu-id="b5a3a-150">W poprzednim kodzie [odbicie](/dotnet/csharp/programming-guide/concepts/reflection) jest używane do kompilowania nazwy pliku XML pasującego do projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-150">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="b5a3a-151">Właściwość [AppContext. BaseDirectory](/dotnet/api/system.appcontext.basedirectory) służy do konstruowania ścieżki do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-151">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="b5a3a-152">Niektóre funkcje struktury Swagger (na przykład schematu parametrów wejściowych lub metod HTTP i kodów odpowiedzi z odpowiednich atrybutów) działają bez użycia pliku dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-152">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="b5a3a-153">W przypadku większości funkcji, a mianowicie podsumowania metod i opisów parametrów i kodów odpowiedzi, użycie pliku XML jest obowiązkowe.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-153">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="b5a3a-154">Dodawanie do akcji komentarzy z potrójnym ukośnikiem ulepsza wyniki narzędzia Swagger UI przez dodanie opisu do nagłówka sekcji.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-154">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="b5a3a-155">Dodaj element `Delete` powyżej akcji: [\<podsumowania>](/dotnet/csharp/programming-guide/xmldoc/summary)</span><span class="sxs-lookup"><span data-stu-id="b5a3a-155">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

```csharp
/// <summary>
/// Deletes a specific TodoItem.
/// </summary>
/// <param name="id"></param>        
[HttpDelete("{id}")]
public IActionResult Delete(long id)
{
    var todo = _context.TodoItems.Find(id);

    if (todo == null)
    {
        return NotFound();
    }

    _context.TodoItems.Remove(todo);
    _context.SaveChanges();

    return NoContent();
}
```
<span data-ttu-id="b5a3a-156">Interfejs użytkownika struktury Swagger wyświetla tekst wewnętrzny elementu `<summary>` poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-156">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interfejs użytkownika struktury Swagger pokazujący komentarz XML "Usuwa określony TodoItem".](sample_images/triple-slash-comments.png)

<span data-ttu-id="b5a3a-159">Interfejs użytkownika jest oparty na wygenerowanym schemacie JSON:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-159">The UI is driven by the generated JSON schema:</span></span>

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
<span data-ttu-id="b5a3a-160">Dodaj [\<uwagi >](/dotnet/csharp/programming-guide/xmldoc/remarks) elementu do dokumentacji metody akcji `Create`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-160">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="b5a3a-161">Uzupełnia informacje określone w `<summary>` elemencie i zapewnia bardziej niezawodny interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-161">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="b5a3a-162">Zawartość elementu `<remarks>` może składać się z tekstu, JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-162">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

```csharp
/// <summary>
/// Creates a TodoItem.
/// </summary>
/// <remarks>
/// Sample request:
///
///     POST /Todo
///     {
///        "id": 1,
///        "name": "Item1",
///        "isComplete": true
///     }
///
/// </remarks>
/// <param name="item"></param>
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
{
    _context.TodoItems.Add(item);
    _context.SaveChanges();

    return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```
<span data-ttu-id="b5a3a-163">Zwróć uwagę na ulepszenia interfejsu użytkownika z następującymi dodatkowymi komentarzami:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-163">Notice the UI enhancements with these additional comments:</span></span>

![Interfejs użytkownika struktury Swagger z pokazanymi dodatkowymi komentarzami](sample_images/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="b5a3a-165">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="b5a3a-165">Data annotations</span></span>

<span data-ttu-id="b5a3a-166">Oznacz model atrybutami, które znajdują się w przestrzeni nazw [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) , aby ułatwić DYSKOM interfejsu użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-166">Mark the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="b5a3a-167">Dodaj atrybut `[Required]` do właściwości `Name` klasy `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-167">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

```csharp
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;

namespace TodoApi.Models
{
    public class TodoItem
    {
        public long Id { get; set; }

        [Required]
        public string Name { get; set; }

        [DefaultValue(false)]
        public bool IsComplete { get; set; }
    }
}
```

<span data-ttu-id="b5a3a-168">Obecność tego atrybutu zmienia zachowanie interfejsu użytkownika i zmienia źródłowy schemat JSON:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-168">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="b5a3a-169">Dodaj atrybut `[Produces("application/json")]` do kontrolera interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-169">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="b5a3a-170">Celem jest zadeklarowanie, że działania kontrolera obsługują typ zawartości odpowiedzi dla *aplikacji/JSON*:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-170">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

```csharp
[Produces("application/json")]
[Route("api/[controller]")]
[ApiController]
public class TodoController : ControllerBase
{
    private readonly TodoContext _context;
```
<span data-ttu-id="b5a3a-171">Lista rozwijana **Typ zawartości odpowiedzi** wybiera ten typ zawartości jako domyślny dla akcji Get kontrolera:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-171">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury Swagger z domyślnym typem zawartości odpowiedzi](sample_images/json-response-content-type.png)

<span data-ttu-id="b5a3a-173">W miarę wzrostu użycia adnotacji danych w interfejsie API sieci Web strony interfejsu użytkownika i interfejsu API stają się bardziej opisowe i przydatne.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-173">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="b5a3a-174">Opisz typy odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b5a3a-174">Describe response types</span></span>

<span data-ttu-id="b5a3a-175">Deweloperzy korzystający z interfejsu API sieci Web mają największe znaczenie w odróżnieniu od typów odpowiedzi i kodów błędów (jeśli nie&mdash;są standardem).</span><span class="sxs-lookup"><span data-stu-id="b5a3a-175">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="b5a3a-176">Typy odpowiedzi i kody błędów są oznaczane w komentarzach XML i adnotacjach danych.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-176">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="b5a3a-177">Akcja `Create` zwraca kod stanu HTTP 201 na powodzeniu.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-177">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="b5a3a-178">Kod stanu HTTP 400 jest zwracany, gdy treść ogłoszonego żądania ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-178">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="b5a3a-179">Bez odpowiedniej dokumentacji w interfejsie użytkownika programu Swagger klient nie ma wiedzy na temat oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-179">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="b5a3a-180">Rozwiąż ten problem, dodając wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-180">Fix that problem by adding the highlighted lines in the following example:</span></span>

```csharp
/// <returns>A newly created TodoItem</returns>
/// <response code="201">Returns the newly created item</response>
/// <response code="400">If the item is null</response>            
[HttpPost]
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<TodoItem> Create(TodoItem item)
```

<span data-ttu-id="b5a3a-181">Interfejs użytkownika struktury Swagger teraz jasno dokumentuje oczekiwane kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="b5a3a-181">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfejs użytkownika struktury Swagger pokazujący opis klasy odpowiedzi "zwraca nowo utworzony element zadania" i "400-Jeśli element ma wartość null" dla kodu stanu i przyczyny w komunikatach odpowiedzi](sample_images/data-annotations-response-types.png)

<span data-ttu-id="b5a3a-183">W ASP.NET Core 2,2 lub nowszych Konwencji mogą służyć jako alternatywa dla jawnego dekorowania nazwy poszczególnych akcji z `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="b5a3a-183">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="b5a3a-184">Aby uzyskać więcej informacji, zobacz [Korzystanie z Konwencji interfejsu API sieci Web](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="b5a3a-184">For more information, see [Use web API conventions](https://docs.microsoft.com/aspnet/core/web-api/advanced/conventions).</span></span>

<span data-ttu-id="b5a3a-185">Aby uzyskać informacje na temat dostosowywania interfejsu użytkownika, zobacz: [Dostosowywanie interfejsu użytkownika](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend)</span><span class="sxs-lookup"><span data-stu-id="b5a3a-185">For information on customizing the UI see: [Customize the UI](/aspnet/core/tutorials/getting-started-with-swashbuckle?#customize-and-extend)</span></span>
