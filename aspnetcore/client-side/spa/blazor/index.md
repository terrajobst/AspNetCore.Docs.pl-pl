---
title: Wprowadzenie do Blazor
author: guardrex
description: 'Zapoznaj się z platformy ASP.NET Core Blazor, nowy sposób twórz interaktywne aplikacje po stronie klienta przy użyciu platformy .NET, które są uruchamiane w przeglądarce z format WebAssembly.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: spa/blazor/index
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="b157d-103">Wprowadzenie do Blazor</span><span class="sxs-lookup"><span data-stu-id="b157d-103">Introduction to Blazor</span></span>

<span data-ttu-id="b157d-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b157d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="b157d-105">Blazor to aplikacja jednostronicowa umożliwiająca tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b157d-105">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="b157d-106">Blazor używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu.</span><span class="sxs-lookup"><span data-stu-id="b157d-106">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="b157d-107">Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="b157d-107">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="b157d-108">Przy użyciu platformy .NET w przeglądarce do tworzenia aplikacji internetowych po stronie klienta ma wiele zalet:</span><span class="sxs-lookup"><span data-stu-id="b157d-108">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="b157d-109">**C#język**: Pisanie kodu w C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b157d-109">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="b157d-110">**Ekosystemu .NET**: Wykorzystaj istniejące ekosystemu bibliotek platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b157d-110">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="b157d-111">**Tworzenie pełnych**: Udostępnij logiki po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="b157d-111">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="b157d-112">**Szybkość i skalowalność**: .NET została skompilowana wydajność, niezawodność i bezpieczeństwo.</span><span class="sxs-lookup"><span data-stu-id="b157d-112">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="b157d-113">**Wiodące w branży narzędzi**: Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="b157d-113">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="b157d-114">**Stabilność i spójność**:  Twórz commonset liczby języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.</span><span class="sxs-lookup"><span data-stu-id="b157d-114">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="b157d-115">Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*).</span><span class="sxs-lookup"><span data-stu-id="b157d-115">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="b157d-116">Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek.</span><span class="sxs-lookup"><span data-stu-id="b157d-116">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="b157d-117">Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.</span><span class="sxs-lookup"><span data-stu-id="b157d-117">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="b157d-118">Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b157d-118">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="b157d-119">W tym samym czasie kodu platformy .NET, wykonywane przy użyciu format WebAssembly działa w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b157d-119">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor uruchamia kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="b157d-121">Przy aplikacji Blazor jest wbudowana i uruchomić w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b157d-121">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="b157d-122">C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b157d-122">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="b157d-123">Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b157d-123">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="b157d-124">Blazor używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b157d-124">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="b157d-125">Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe Blazor za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b157d-125">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="b157d-126">Blazor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:</span><span class="sxs-lookup"><span data-stu-id="b157d-126">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="b157d-127">Parametry</span><span class="sxs-lookup"><span data-stu-id="b157d-127">Parameters</span></span>
* <span data-ttu-id="b157d-128">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="b157d-128">Event handling</span></span>
* <span data-ttu-id="b157d-129">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="b157d-129">Data binding</span></span>
* <span data-ttu-id="b157d-130">Routing</span><span class="sxs-lookup"><span data-stu-id="b157d-130">Routing</span></span>
* <span data-ttu-id="b157d-131">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="b157d-131">Dependency injection</span></span>
* <span data-ttu-id="b157d-132">Układy</span><span class="sxs-lookup"><span data-stu-id="b157d-132">Layouts</span></span>
* <span data-ttu-id="b157d-133">Tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="b157d-133">Templating</span></span>
* <span data-ttu-id="b157d-134">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="b157d-134">Cascading values</span></span>

<span data-ttu-id="b157d-135">Aby zmniejszyć rozmiar aplikacji pobranych nieużywany kod usunięte z aplikacji publikowanych przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/razor-components/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="b157d-135">To reduce the size of the downloaded app unused code stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

<span data-ttu-id="b157d-136">Blazor jest modelu hostingu po stronie klienta dla składników Razor.</span><span class="sxs-lookup"><span data-stu-id="b157d-136">Blazor is the client-side hosting model for Razor Components.</span></span> <span data-ttu-id="b157d-137">Ponieważ składniki Razor oddzielenie logiki renderowania składnika w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jak Razor składniki mogą być hostowane jest elastyczność.</span><span class="sxs-lookup"><span data-stu-id="b157d-137">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="b157d-138">Umożliwia składniki programu ASP.NET Core Razor hosta Razor składników w aplikacji ASP.NET Core na serwerze gdzie aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="b157d-138">Use ASP.NET Core Razor Components to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="b157d-139">Aby uzyskać więcej informacji, zobacz <xref:razor-components/index> i <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="b157d-139">For more information, see <xref:razor-components/index> and <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span> 

## <a name="components"></a><span data-ttu-id="b157d-140">Składniki</span><span class="sxs-lookup"><span data-stu-id="b157d-140">Components</span></span>

<span data-ttu-id="b157d-141">A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="b157d-141">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="b157d-142">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b157d-142">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="b157d-143">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="b157d-143">Components can be nested and reused.</span></span>

<span data-ttu-id="b157d-144">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="b157d-144">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="b157d-145">Klasa, albo mogą być napisane w postaci znaczników strony Razor (*.cshtml*) lub jako C# klasy (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="b157d-145">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="b157d-146">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu.</span><span class="sxs-lookup"><span data-stu-id="b157d-146">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="b157d-147">Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="b157d-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="b157d-148">Strony razor i MVC widoki również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="b157d-148">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="b157d-149">W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b157d-149">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="b157d-150">Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="b157d-150">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="b157d-151">Następujący kod jest przykładem składnika niestandardowy dialog w pliku Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="b157d-151">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="b157d-152">Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="b157d-152">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="b157d-153">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.</span><span class="sxs-lookup"><span data-stu-id="b157d-153">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="b157d-154">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="b157d-154">JavaScript interop</span></span>

<span data-ttu-id="b157d-155">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API Blazor współdziała z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b157d-155">For apps that require third-party JavaScript libraries and browser APIs, Blazor interoperates with JavaScript.</span></span> <span data-ttu-id="b157d-156">Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="b157d-156">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="b157d-157">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="b157d-157">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="b157d-158">Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="b157d-158">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="b157d-159">Udostępnianie kodu i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="b157d-159">Code sharing and .NET Standard</span></span>

<span data-ttu-id="b157d-160">Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b157d-160">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="b157d-161">.NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b157d-161">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="b157d-162">.NET standard 2.0 lub nowszej jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="b157d-162">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="b157d-163">Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="b157d-163">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="b157d-164">Biblioteki klas .NET standard mogą być udostępniane w kodzie serwera oraz w aplikacjach opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b157d-164">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="optimization"></a><span data-ttu-id="b157d-165">Optymalizacja</span><span class="sxs-lookup"><span data-stu-id="b157d-165">Optimization</span></span>

<span data-ttu-id="b157d-166">Rozmiar ładunku jest czynnikiem wydajność krytycznych dla użyteczności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b157d-166">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="b157d-167">Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:</span><span class="sxs-lookup"><span data-stu-id="b157d-167">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="b157d-168">Nieużywane części zestawów .NET są usuwane podczas procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b157d-168">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="b157d-169">Odpowiedzi HTTP są kompresowane.</span><span class="sxs-lookup"><span data-stu-id="b157d-169">HTTP responses are compressed.</span></span>
* <span data-ttu-id="b157d-170">Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b157d-170">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="b157d-171">[Składniki Razor serwerowe](xref:razor-components/index) zapewnia jeszcze mniejszy rozmiar ładunku niż Blazor przy zachowaniu zestawy .NET, zestawu aplikacji i po stronie serwera środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="b157d-171">[Server-side Razor Components](xref:razor-components/index) provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="b157d-172">Razor składniki aplikacji służyć tylko plikami znaczników i statyczne elementy zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="b157d-172">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b157d-173">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b157d-173">Additional resources</span></span>

* <xref:razor-components/index>
* [<span data-ttu-id="b157d-174">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="b157d-174">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="b157d-175">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="b157d-175">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="b157d-176">HTML</span><span class="sxs-lookup"><span data-stu-id="b157d-176">HTML</span></span>](https://www.w3.org/html/)
