---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983000"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="2808a-103">Wprowadzenie do Blazor</span><span class="sxs-lookup"><span data-stu-id="2808a-103">Introduction to Blazor</span></span>

<span data-ttu-id="2808a-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2808a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2808a-105">Blazor — Zapraszamy!</span><span class="sxs-lookup"><span data-stu-id="2808a-105">Welcome to Blazor!</span></span>

<span data-ttu-id="2808a-106">Twórz interaktywne po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="2808a-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="2808a-107">Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2808a-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="2808a-108">Udostępnij logiki po stronie serwera i klienta aplikacji napisanych przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2808a-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="2808a-109">Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="2808a-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="2808a-110">Blazor obsługuje podstawowe scenariusze wymagane przez większość aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2808a-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="2808a-111">Parametry</span><span class="sxs-lookup"><span data-stu-id="2808a-111">Parameters</span></span>
* <span data-ttu-id="2808a-112">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="2808a-112">Event handling</span></span>
* <span data-ttu-id="2808a-113">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="2808a-113">Data binding</span></span>
* <span data-ttu-id="2808a-114">Routing</span><span class="sxs-lookup"><span data-stu-id="2808a-114">Routing</span></span>
* <span data-ttu-id="2808a-115">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="2808a-115">Dependency injection</span></span>
* <span data-ttu-id="2808a-116">Układy</span><span class="sxs-lookup"><span data-stu-id="2808a-116">Layouts</span></span>
* <span data-ttu-id="2808a-117">Szablony</span><span class="sxs-lookup"><span data-stu-id="2808a-117">Templates</span></span>
* <span data-ttu-id="2808a-118">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="2808a-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="2808a-119">Składniki</span><span class="sxs-lookup"><span data-stu-id="2808a-119">Components</span></span>

<span data-ttu-id="2808a-120">A *składnika* Blazor jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="2808a-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="2808a-121">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2808a-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="2808a-122">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="2808a-122">Components can be nested and reused.</span></span>

<span data-ttu-id="2808a-123">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="2808a-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="2808a-124">Klasa składników są zwykle zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="2808a-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="2808a-125">Składniki Blazor są czasami określane jako składniki Razor.</span><span class="sxs-lookup"><span data-stu-id="2808a-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="2808a-126">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu.</span><span class="sxs-lookup"><span data-stu-id="2808a-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="2808a-127">Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="2808a-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="2808a-128">Strony razor i MVC widoki również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="2808a-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="2808a-129">W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2808a-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="2808a-130">Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="2808a-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="2808a-131">Następujący kod jest przykładem składnika niestandardowe okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="2808a-131">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="2808a-132">Podczas tego składnika jest używany w aplikacji, funkcji IntelliSense w [programu Visual Studio](https://visualstudio.microsoft.com/vs/) przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="2808a-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="2808a-133">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.</span><span class="sxs-lookup"><span data-stu-id="2808a-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="2808a-134">Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="2808a-134">Blazor server-side</span></span>

<span data-ttu-id="2808a-135">Blazor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2808a-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="2808a-136">Po stronie serwera Blazor zapewnia obsługę hostującego składniki Razor w aplikacji ASP.NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2808a-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="2808a-137">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="2808a-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="2808a-138">Środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="2808a-138">The runtime:</span></span>

* <span data-ttu-id="2808a-139">Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.</span><span class="sxs-lookup"><span data-stu-id="2808a-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="2808a-140">Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.</span><span class="sxs-lookup"><span data-stu-id="2808a-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="2808a-141">Połączenie używane przez Blazor po stronie serwera do komunikacji za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2808a-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Po stronie serwera Blazor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/blazor-server-side.png)

<span data-ttu-id="2808a-143">Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="2808a-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="2808a-144">Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="2808a-144">Blazor client-side</span></span>

<span data-ttu-id="2808a-145">Blazor po stronie klienta jest aplikacja jednostronicowa umożliwiająca tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2808a-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="2808a-146">Blazor po stronie klienta używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu.</span><span class="sxs-lookup"><span data-stu-id="2808a-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="2808a-147">Blazor po stronie klienta działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="2808a-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="2808a-148">Przy użyciu platformy .NET w przeglądarce do tworzenia aplikacji internetowych po stronie klienta ma wiele zalet:</span><span class="sxs-lookup"><span data-stu-id="2808a-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="2808a-149">**C#język**: Pisanie kodu w C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2808a-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="2808a-150">**Ekosystemu .NET**: Wykorzystaj istniejące ekosystemu bibliotek platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2808a-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="2808a-151">**Tworzenie pełnych**: Udostępnij logiki po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="2808a-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="2808a-152">**Szybkość i skalowalność**: .NET została skompilowana wydajność, niezawodność i bezpieczeństwo.</span><span class="sxs-lookup"><span data-stu-id="2808a-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="2808a-153">**Wiodące w branży narzędzi**: Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="2808a-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="2808a-154">**Stabilność i spójność**:  Twórz wspólny zbiór języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.</span><span class="sxs-lookup"><span data-stu-id="2808a-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="2808a-155">Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*).</span><span class="sxs-lookup"><span data-stu-id="2808a-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="2808a-156">Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek.</span><span class="sxs-lookup"><span data-stu-id="2808a-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="2808a-157">Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.</span><span class="sxs-lookup"><span data-stu-id="2808a-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="2808a-158">Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2808a-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="2808a-159">W tym samym czasie kodu platformy .NET, wykonywane przy użyciu format WebAssembly działa w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="2808a-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor po stronie klienta działa kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="2808a-161">Przy aplikacji po stronie klienta Blazor jest wbudowana i uruchomić w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="2808a-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="2808a-162">C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2808a-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="2808a-163">Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2808a-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="2808a-164">Blazor po stronie klienta używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2808a-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="2808a-165">Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe po stronie klienta Blazor za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2808a-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="2808a-166">Aby zmniejszyć rozmiar pobranej aplikacji, nieużywany kod jest usuwany poza aplikacją, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="2808a-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="2808a-167">Blazor po stronie klienta jest modelu hostingu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="2808a-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="2808a-168">Ponieważ Blazor oddziela składnika renderowania logiki, w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jaki może być hostowana Blazor jest elastyczność.</span><span class="sxs-lookup"><span data-stu-id="2808a-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="2808a-169">Użyj [Blazor po stronie serwera](#blazor-server-side) hosta Blazor na serwerze w aplikacji ASP.NET Core, w którym aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="2808a-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="2808a-170">Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="2808a-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="2808a-171">Rozmiar ładunku jest czynnikiem wydajność krytycznych dla użyteczności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2808a-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="2808a-172">Po stronie klienta Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:</span><span class="sxs-lookup"><span data-stu-id="2808a-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="2808a-173">Nieużywane części zestawów .NET są usuwane podczas procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2808a-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="2808a-174">Odpowiedzi HTTP są kompresowane.</span><span class="sxs-lookup"><span data-stu-id="2808a-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="2808a-175">Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2808a-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="2808a-176">[Po stronie serwera Blazor](#blazor-server-side) zapewnia mniejszy rozmiar ładunku niż Blazor po stronie klienta przy zachowaniu zestawy .NET, zestawu aplikacji i po stronie serwera środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="2808a-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="2808a-177">Blazor po stronie serwera aplikacji służyć tylko pliki znaczników i statyczne elementy zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="2808a-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="2808a-178">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="2808a-178">JavaScript interop</span></span>

<span data-ttu-id="2808a-179">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2808a-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="2808a-180">Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="2808a-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="2808a-181">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="2808a-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="2808a-182">Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="2808a-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="2808a-183">Udostępnianie kodu i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="2808a-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="2808a-184">Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="2808a-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="2808a-185">.NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2808a-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="2808a-186">Blazor implementuje .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2808a-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="2808a-187">Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="2808a-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="2808a-188">Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.</span><span class="sxs-lookup"><span data-stu-id="2808a-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2808a-189">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2808a-189">Additional resources</span></span>

* [<span data-ttu-id="2808a-190">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="2808a-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="2808a-191">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="2808a-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="2808a-192">HTML</span><span class="sxs-lookup"><span data-stu-id="2808a-192">HTML</span></span>](https://www.w3.org/html/)
