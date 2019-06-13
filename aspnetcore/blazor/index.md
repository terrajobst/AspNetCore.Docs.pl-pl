---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034423"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="b8358-103">Wprowadzenie do Blazor</span><span class="sxs-lookup"><span data-stu-id="b8358-103">Introduction to Blazor</span></span>

<span data-ttu-id="b8358-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b8358-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b8358-105">*Blazor — Zapraszamy!*</span><span class="sxs-lookup"><span data-stu-id="b8358-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="b8358-106">Blazor to architektura służąca do tworzenia interaktywnego po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="b8358-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="b8358-107">Tworzyć zaawansowane interaktywne interfejsy, za pomocą C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b8358-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="b8358-108">Udostępnij logiki po stronie serwera i klienta aplikacji napisanych przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b8358-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="b8358-109">Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="b8358-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="b8358-110">Do tworzenia aplikacji internetowych po stronie klienta przy użyciu platformy .NET ma następujące zalety:</span><span class="sxs-lookup"><span data-stu-id="b8358-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="b8358-111">Pisanie kodu w C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b8358-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="b8358-112">Wykorzystaj istniejące ekosystemu .NET bibliotek platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b8358-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="b8358-113">Logika aplikacji może być współużytkowane przez serwer i klienta.</span><span class="sxs-lookup"><span data-stu-id="b8358-113">Share app logic across the server and client.</span></span>
* <span data-ttu-id="b8358-114">Korzystaj z. Wydajność, niezawodność i bezpieczeństwo przez sieć.</span><span class="sxs-lookup"><span data-stu-id="b8358-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="b8358-115">Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="b8358-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="b8358-116">Twórz wspólny zbiór języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.</span><span class="sxs-lookup"><span data-stu-id="b8358-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="b8358-117">Składniki</span><span class="sxs-lookup"><span data-stu-id="b8358-117">Components</span></span>

<span data-ttu-id="b8358-118">Aplikacje Blazor opierają się na *składniki*.</span><span class="sxs-lookup"><span data-stu-id="b8358-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="b8358-119">Składnik Blazor jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="b8358-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="b8358-120">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b8358-120">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="b8358-121">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="b8358-121">Components can be nested and reused.</span></span>

<span data-ttu-id="b8358-122">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako [pakiety NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="b8358-122">Components are .NET classes built into .NET assemblies that can be shared and distributed as [NuGet packages](/nuget/what-is-nuget).</span></span> <span data-ttu-id="b8358-123">Klasa składników są zwykle zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="b8358-123">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="b8358-124">Składniki Blazor są czasami określane jako *składniki Razor*.</span><span class="sxs-lookup"><span data-stu-id="b8358-124">Components in Blazor are sometimes referred to as *Razor components*.</span></span> <span data-ttu-id="b8358-125">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu zapewniające produktywność deweloperów.</span><span class="sxs-lookup"><span data-stu-id="b8358-125">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="b8358-126">Razor pozwala na przełączanie między kod znaczników HTML i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="b8358-126">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="b8358-127">Strony razor i MVC również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="b8358-127">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="b8358-128">W przeciwieństwie do stron Razor i MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="b8358-128">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="b8358-129">Następujący kod Razor pokazuje składnika (*Dialog.razor*), który może być zagnieżdżona w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="b8358-129">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

<span data-ttu-id="b8358-130">Okno dialogowe treść (`ChildContent`) i tytuł (`Title`) są dostarczane przez składnik, który używa tego składnika w jego interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b8358-130">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="b8358-131">`OnYes` jest C# metoda wyzwalane za pomocą przycisku `onclick` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="b8358-131">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="b8358-132">Blazor używa naturalnych znaczników HTML dla kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b8358-132">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="b8358-133">Elementy HTML określ składniki, a atrybuty znacznika przekazania wartości do właściwości tego składnika.</span><span class="sxs-lookup"><span data-stu-id="b8358-133">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span> <span data-ttu-id="b8358-134">`ChildContent` i `Title` ustawionych przez składnik, który używa składnika okna dialogowego (*Index.razor*):</span><span class="sxs-lookup"><span data-stu-id="b8358-134">`ChildContent` and `Title` are set by the component that uses the Dialog component (*Index.razor*):</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="b8358-135">Okno dialogowe jest renderowana, gdy element nadrzędny (*Index.razor*) jest dostępny w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b8358-135">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Okno dialogowe składnika w przeglądarce](index/_static/dialog.png)

<span data-ttu-id="b8358-137">Gdy ten składnik jest używany w aplikacji, funkcji IntelliSense w [programu Visual Studio](/visualstudio/ide/using-intellisense) i [programu Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="b8358-137">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="b8358-138">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* służący do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="b8358-138">Components render into an in-memory representation of the browser DOM called a *render tree* that's used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="b8358-139">Blazor po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="b8358-139">Blazor client-side</span></span>

<span data-ttu-id="b8358-140">Blazor po stronie klienta jest aplikacja jednostronicowa umożliwiająca tworzenie aplikacji sieci web po stronie klienta interactive na platformie .NET.</span><span class="sxs-lookup"><span data-stu-id="b8358-140">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="b8358-141">Blazor po stronie klienta używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu i działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="b8358-141">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="b8358-142">Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*).</span><span class="sxs-lookup"><span data-stu-id="b8358-142">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="b8358-143">Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek.</span><span class="sxs-lookup"><span data-stu-id="b8358-143">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="b8358-144">Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.</span><span class="sxs-lookup"><span data-stu-id="b8358-144">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="b8358-145">Format WebAssembly kod może uzyskiwać dostęp do pełnej funkcjonalności przeglądarki przy użyciu języka JavaScript, o nazwie *współdziałanie JavaScript* (lub *międzyoperacyjnego JavaScript*).</span><span class="sxs-lookup"><span data-stu-id="b8358-145">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="b8358-146">Kod platformy .NET, wykonywane przy użyciu format WebAssembly w przeglądarce jest uruchamiany w tym samym piaskownicy zaufanych jako kodu JavaScript, który wirtualnie eliminuje możliwości dla aplikacji w celu wykonania złośliwych akcji na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b8358-146">.NET code executed via WebAssembly in the browser runs in the same trusted sandbox as JavaScript, which virtually eliminates the opportunity for an app to perform malicious actions on the client machine.</span></span>

![Blazor po stronie klienta działa kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="b8358-148">Przy aplikacji po stronie klienta Blazor jest wbudowana i uruchomić w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="b8358-148">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="b8358-149">C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b8358-149">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="b8358-150">Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b8358-150">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="b8358-151">Blazor po stronie klienta używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b8358-151">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="b8358-152">Środowisko uruchomieniowe Blazor po stronie klienta używa międzyoperacyjnego JavaScript do obsługi wywołań interfejsu API manipulacji i przeglądarka modelu DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="b8358-152">The Blazor client-side runtime uses JavaScript interop to handle Document Object Model (DOM) manipulation and browser API calls.</span></span>

<span data-ttu-id="b8358-153">Rozmiar opublikowanej aplikacji, jego *rozmiar ładunku*, jest czynnikiem wydajność krytycznych dla użyteczności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b8358-153">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="b8358-154">Aplikacja dużych względnie długi czas pobierania do przeglądarki, która dobrze jest środowisko użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b8358-154">A large app takes a relatively long time to download to a browser, which hurts the user experience.</span></span> <span data-ttu-id="b8358-155">Po stronie klienta Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:</span><span class="sxs-lookup"><span data-stu-id="b8358-155">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="b8358-156">Nieużywany kod jest usunięte z aplikacji, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="b8358-156">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="b8358-157">Odpowiedzi HTTP są kompresowane.</span><span class="sxs-lookup"><span data-stu-id="b8358-157">HTTP responses are compressed.</span></span>
* <span data-ttu-id="b8358-158">Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b8358-158">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="b8358-159">Aby uzyskać więcej informacji i wskazówek na temat wybierania modelu hostingu, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="b8358-159">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="b8358-160">Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="b8358-160">Blazor server-side</span></span>

<span data-ttu-id="b8358-161">Blazor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b8358-161">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="b8358-162">Po stronie serwera Blazor zapewnia obsługę hostującego składniki Razor w aplikacji ASP.NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b8358-162">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="b8358-163">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="b8358-163">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="b8358-164">Środowisko wykonawcze obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera i stosuje aktualizacje interfejsu użytkownika wysyłane przez serwer z powrotem do przeglądarki po uruchomieniu składników.</span><span class="sxs-lookup"><span data-stu-id="b8358-164">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="b8358-165">Połączenie używane przez Blazor po stronie serwera do komunikacji za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b8358-165">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Po stronie serwera Blazor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/blazor-server-side.png)

<span data-ttu-id="b8358-167">Aby uzyskać więcej informacji i wskazówek na temat wybierania modelu hostingu, zobacz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="b8358-167">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="b8358-168">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="b8358-168">JavaScript interop</span></span>

<span data-ttu-id="b8358-169">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b8358-169">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="b8358-170">Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="b8358-170">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="b8358-171">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="b8358-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="b8358-172">Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="b8358-172">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="b8358-173">Udostępnianie kodu i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="b8358-173">Code sharing and .NET Standard</span></span>

<span data-ttu-id="b8358-174">Implementuje Blazor [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="b8358-174">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="b8358-175">.NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b8358-175">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="b8358-176">Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.</span><span class="sxs-lookup"><span data-stu-id="b8358-176">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="b8358-177">Throw interfejsów API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo i wątkowość) <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="b8358-177">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8358-178">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b8358-178">Additional resources</span></span>

* [<span data-ttu-id="b8358-179">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="b8358-179">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="b8358-180">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="b8358-180">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="b8358-181">HTML</span><span class="sxs-lookup"><span data-stu-id="b8358-181">HTML</span></span>](https://www.w3.org/html/)
