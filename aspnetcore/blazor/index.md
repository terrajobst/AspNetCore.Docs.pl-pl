---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614882"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="baf22-103">Wprowadzenie do Blazor</span><span class="sxs-lookup"><span data-stu-id="baf22-103">Introduction to Blazor</span></span>

<span data-ttu-id="baf22-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="baf22-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="baf22-105">Składniki Razor</span><span class="sxs-lookup"><span data-stu-id="baf22-105">Razor Components</span></span>

<span data-ttu-id="baf22-106">Model hostingu po stronie serwera Blazor, *składniki Razor*, jest sposób tworzenia interaktywnego po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="baf22-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="baf22-107">Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf22-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="baf22-108">Udostępnianie aplikacji po stronie serwera i klienta logiki napisane przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="baf22-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="baf22-109">Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="baf22-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="baf22-110">Składniki razor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:</span><span class="sxs-lookup"><span data-stu-id="baf22-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="baf22-111">Parametry</span><span class="sxs-lookup"><span data-stu-id="baf22-111">Parameters</span></span>
* <span data-ttu-id="baf22-112">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="baf22-112">Event handling</span></span>
* <span data-ttu-id="baf22-113">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="baf22-113">Data binding</span></span>
* <span data-ttu-id="baf22-114">Routing</span><span class="sxs-lookup"><span data-stu-id="baf22-114">Routing</span></span>
* <span data-ttu-id="baf22-115">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="baf22-115">Dependency injection</span></span>
* <span data-ttu-id="baf22-116">Układy</span><span class="sxs-lookup"><span data-stu-id="baf22-116">Layouts</span></span>
* <span data-ttu-id="baf22-117">Tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="baf22-117">Templating</span></span>
* <span data-ttu-id="baf22-118">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="baf22-118">Cascading values</span></span>

<span data-ttu-id="baf22-119">Składniki razor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="baf22-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="baf22-120">Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="baf22-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="baf22-121">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="baf22-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="baf22-122">Środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="baf22-122">The runtime:</span></span>

* <span data-ttu-id="baf22-123">Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.</span><span class="sxs-lookup"><span data-stu-id="baf22-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="baf22-124">Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.</span><span class="sxs-lookup"><span data-stu-id="baf22-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="baf22-125">Połączenie używane przez składniki Razor do komunikowania się za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf22-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="baf22-127">Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="baf22-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="baf22-128">Składniki</span><span class="sxs-lookup"><span data-stu-id="baf22-128">Components</span></span>

<span data-ttu-id="baf22-129">A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="baf22-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="baf22-130">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="baf22-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="baf22-131">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="baf22-131">Components can be nested and reused.</span></span>

<span data-ttu-id="baf22-132">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="baf22-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="baf22-133">Klasy zwykle są zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku (Razor składniki) lub strona znaczników Razor z *.cshtml* rozszerzenie (Blazor).</span><span class="sxs-lookup"><span data-stu-id="baf22-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="baf22-134">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu.</span><span class="sxs-lookup"><span data-stu-id="baf22-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="baf22-135">Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="baf22-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="baf22-136">Strony razor i MVC widoki również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="baf22-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="baf22-137">W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="baf22-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="baf22-138">Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="baf22-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="baf22-139">Następujący kod jest przykładem składnika niestandardowe okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="baf22-139">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="baf22-140">Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="baf22-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="baf22-141">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.</span><span class="sxs-lookup"><span data-stu-id="baf22-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="baf22-142">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="baf22-142">JavaScript interop</span></span>

<span data-ttu-id="baf22-143">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf22-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="baf22-144">Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="baf22-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="baf22-145">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="baf22-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="baf22-146">Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="baf22-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="baf22-147">Udostępnianie kodu i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="baf22-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="baf22-148">Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="baf22-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="baf22-149">.NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="baf22-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="baf22-150">Blazor implementuje .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="baf22-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="baf22-151">Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="baf22-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="baf22-152">Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.</span><span class="sxs-lookup"><span data-stu-id="baf22-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="baf22-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="baf22-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="baf22-154">Blazor to aplikacja jednostronicowa umożliwiająca tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="baf22-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="baf22-155">Blazor używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu.</span><span class="sxs-lookup"><span data-stu-id="baf22-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="baf22-156">Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="baf22-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="baf22-157">Przy użyciu platformy .NET w przeglądarce do tworzenia aplikacji internetowych po stronie klienta ma wiele zalet:</span><span class="sxs-lookup"><span data-stu-id="baf22-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="baf22-158">**C#język**: Pisanie kodu w C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf22-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="baf22-159">**Ekosystemu .NET**: Wykorzystaj istniejące ekosystemu bibliotek platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="baf22-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="baf22-160">**Tworzenie pełnych**: Udostępnij logiki po stronie klienta i serwera.</span><span class="sxs-lookup"><span data-stu-id="baf22-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="baf22-161">**Szybkość i skalowalność**: .NET została skompilowana wydajność, niezawodność i bezpieczeństwo.</span><span class="sxs-lookup"><span data-stu-id="baf22-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="baf22-162">**Wiodące w branży narzędzi**: Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="baf22-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="baf22-163">**Stabilność i spójność**:  Twórz commonset liczby języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.</span><span class="sxs-lookup"><span data-stu-id="baf22-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="baf22-164">Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*).</span><span class="sxs-lookup"><span data-stu-id="baf22-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="baf22-165">Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek.</span><span class="sxs-lookup"><span data-stu-id="baf22-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="baf22-166">Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.</span><span class="sxs-lookup"><span data-stu-id="baf22-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="baf22-167">Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf22-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="baf22-168">W tym samym czasie kodu platformy .NET, wykonywane przy użyciu format WebAssembly działa w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="baf22-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor uruchamia kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="baf22-170">Przy aplikacji Blazor jest wbudowana i uruchomić w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="baf22-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="baf22-171">C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="baf22-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="baf22-172">Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="baf22-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="baf22-173">Blazor używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="baf22-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="baf22-174">Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe Blazor za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf22-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="baf22-175">Blazor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:</span><span class="sxs-lookup"><span data-stu-id="baf22-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="baf22-176">Parametry</span><span class="sxs-lookup"><span data-stu-id="baf22-176">Parameters</span></span>
* <span data-ttu-id="baf22-177">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="baf22-177">Event handling</span></span>
* <span data-ttu-id="baf22-178">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="baf22-178">Data binding</span></span>
* <span data-ttu-id="baf22-179">Routing</span><span class="sxs-lookup"><span data-stu-id="baf22-179">Routing</span></span>
* <span data-ttu-id="baf22-180">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="baf22-180">Dependency injection</span></span>
* <span data-ttu-id="baf22-181">Układy</span><span class="sxs-lookup"><span data-stu-id="baf22-181">Layouts</span></span>
* <span data-ttu-id="baf22-182">Tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="baf22-182">Templating</span></span>
* <span data-ttu-id="baf22-183">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="baf22-183">Cascading values</span></span>

<span data-ttu-id="baf22-184">Aby zmniejszyć rozmiar pobranej aplikacji, nieużywany kod jest usuwany poza aplikacją, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="baf22-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="baf22-185">Blazor po stronie klienta jest modelu hostingu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="baf22-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="baf22-186">Ponieważ Blazor oddziela składnika renderowania logiki, w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jaki może być hostowana Blazor jest elastyczność.</span><span class="sxs-lookup"><span data-stu-id="baf22-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="baf22-187">Używanie platformy ASP.NET Core [składniki Razor](#razor-components) hosta Razor składników w aplikacji ASP.NET Core, w którym aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR na serwerze.</span><span class="sxs-lookup"><span data-stu-id="baf22-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="baf22-188">Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="baf22-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="baf22-189">Rozmiar ładunku jest czynnikiem wydajność krytycznych dla użyteczności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="baf22-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="baf22-190">Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:</span><span class="sxs-lookup"><span data-stu-id="baf22-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="baf22-191">Nieużywane części zestawów .NET są usuwane podczas procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="baf22-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="baf22-192">Odpowiedzi HTTP są kompresowane.</span><span class="sxs-lookup"><span data-stu-id="baf22-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="baf22-193">Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="baf22-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="baf22-194">[Składniki razor](#razor-components) zapewnia mniejszy rozmiar ładunku niż Blazor przy zachowaniu zestawy .NET, zestawu aplikacji i po stronie serwera środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="baf22-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="baf22-195">Razor składniki aplikacji służyć tylko plikami znaczników i statyczne elementy zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="baf22-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="baf22-196">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="baf22-196">Additional resources</span></span>

* [<span data-ttu-id="baf22-197">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="baf22-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="baf22-198">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="baf22-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="baf22-199">HTML</span><span class="sxs-lookup"><span data-stu-id="baf22-199">HTML</span></span>](https://www.w3.org/html/)
