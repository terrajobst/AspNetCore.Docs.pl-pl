---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Eksploruj ASP.NET Core Blazor, sposób tworzenia interakcyjnego interfejsu użytkownika sieci Web po stronie klienta przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: blazor/index
ms.openlocfilehash: 02c95c19ebfb5ea6ad722f9d49f4cddc7471f8e1
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034088"
---
# <a name="introduction-to-aspnet-core-opno-locblazor"></a><span data-ttu-id="d4e57-103">Wprowadzenie do ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="d4e57-103">Introduction to ASP.NET Core Blazor</span></span>

<span data-ttu-id="d4e57-104">Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d4e57-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d4e57-105">*Witamy w Blazor!*</span><span class="sxs-lookup"><span data-stu-id="d4e57-105">*Welcome to Blazor!*</span></span>

Blazor<span data-ttu-id="d4e57-106"> to struktura służąca do kompilowania interakcyjnego interfejsu użytkownika sieci Web po stronie klienta przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="d4e57-106"> is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="d4e57-107">Utwórz rozbudowane interaktywne C# interfejsów użytkownika przy użyciu zamiast języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4e57-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="d4e57-108">Udostępnianie logiki aplikacji po stronie serwera i klienta zapisaną w środowisku .NET.</span><span class="sxs-lookup"><span data-stu-id="d4e57-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="d4e57-109">Renderuj interfejs użytkownika jako HTML i CSS, aby obsługiwał szeroką przeglądarkę, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="d4e57-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="d4e57-110">Korzystanie z programu .NET do tworzenia aplikacji sieci Web po stronie klienta zapewnia następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="d4e57-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="d4e57-111">Napisz kod C# zamiast języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4e57-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="d4e57-112">Skorzystaj z istniejącego ekosystemu .NET bibliotek platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d4e57-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="d4e57-113">Udostępnianie logiki aplikacji między serwerem i klientem.</span><span class="sxs-lookup"><span data-stu-id="d4e57-113">Share app logic across server and client.</span></span>
* <span data-ttu-id="d4e57-114">Skorzystaj z programu. Wydajność, niezawodność i bezpieczeństwo sieci.</span><span class="sxs-lookup"><span data-stu-id="d4e57-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="d4e57-115">Bądź na bieżąco z programem Visual Studio w systemach Windows, Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="d4e57-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="d4e57-116">Twórz w oparciu o wspólny zestaw języków, struktur i narzędzi, które są stabilne, bogate w funkcje i łatwe w użyciu.</span><span class="sxs-lookup"><span data-stu-id="d4e57-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="d4e57-117">Składniki</span><span class="sxs-lookup"><span data-stu-id="d4e57-117">Components</span></span>

<span data-ttu-id="d4e57-118">aplikacje Blazor są oparte na *składnikach*.</span><span class="sxs-lookup"><span data-stu-id="d4e57-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="d4e57-119">Składnik w Blazor to element interfejsu użytkownika, taki jak strona, okno dialogowe lub formularz wprowadzania danych.</span><span class="sxs-lookup"><span data-stu-id="d4e57-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="d4e57-120">Składniki to klasy .NET wbudowane w zestawy .NET, które:</span><span class="sxs-lookup"><span data-stu-id="d4e57-120">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="d4e57-121">Definiowanie elastycznej logiki renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-121">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="d4e57-122">Obsługa zdarzeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-122">Handle user events.</span></span>
* <span data-ttu-id="d4e57-123">Mogą być zagnieżdżane i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="d4e57-123">Can be nested and reused.</span></span>
* <span data-ttu-id="d4e57-124">Mogą być udostępniane i dystrybuowane jako [biblioteki klas Razor](xref:razor-pages/ui-class) lub [pakiety NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="d4e57-124">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="d4e57-125">Klasa składnika jest zwykle zapisywana w formie strony znaczników [Razor](xref:mvc/views/razor) z rozszerzeniem pliku *Razor* .</span><span class="sxs-lookup"><span data-stu-id="d4e57-125">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="d4e57-126">Składniki w Blazor są formalnie określane jako *składniki Razor*.</span><span class="sxs-lookup"><span data-stu-id="d4e57-126">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="d4e57-127">Razor to Składnia służąca do łączenia znaczników HTML C# z kodem zaprojektowanym na potrzeby produktywności dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="d4e57-127">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="d4e57-128">Razor pozwala przełączać się między znakami C# HTML i w tym samym pliku z obsługą [IntelliSense](/visualstudio/ide/using-intellisense) .</span><span class="sxs-lookup"><span data-stu-id="d4e57-128">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="d4e57-129">Razor Pages i MVC również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="d4e57-129">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="d4e57-130">W przeciwieństwie do Razor Pages i MVC, które są zbudowane wokół modelu żądania/odpowiedzi, składniki są używane specjalnie dla logiki interfejsu użytkownika po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d4e57-130">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="d4e57-131">Poniższy znacznik Razor ilustruje składnik (*dialog. Razor*), który może być zagnieżdżony w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="d4e57-131">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```razor
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="d4e57-132">Treść okna dialogowego (`ChildContent`) i tytuł (`Title`) są dostarczane przez składnik, który używa tego składnika w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-132">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="d4e57-133">`OnYes` jest C# metodą wyzwalaną przez zdarzenie `onclick` przycisku.</span><span class="sxs-lookup"><span data-stu-id="d4e57-133">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

Blazor<span data-ttu-id="d4e57-134"> używa naturalnych tagów HTML dla kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-134"> uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="d4e57-135">Elementy HTML określają składniki, a atrybuty znacznika przechodzą wartości do właściwości składnika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-135">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="d4e57-136">W poniższym przykładzie składnik `Index` używa składnika `Dialog`.</span><span class="sxs-lookup"><span data-stu-id="d4e57-136">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="d4e57-137">`ChildContent` i `Title` są ustawiane przez atrybuty i zawartość elementu `<Dialog>`.</span><span class="sxs-lookup"><span data-stu-id="d4e57-137">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="d4e57-138">*Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d4e57-138">*Index.razor*:</span></span>

```razor
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="d4e57-139">Okno dialogowe jest renderowane po uzyskaniu dostępu do elementu nadrzędnego (*index. Razor*) w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="d4e57-139">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Składnik okna dialogowego renderowany w przeglądarce](index/_static/dialog.png)

<span data-ttu-id="d4e57-141">Gdy ten składnik jest używany w aplikacji, funkcja IntelliSense w programie [Visual Studio](/visualstudio/ide/using-intellisense) i [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) przyspiesza Programowanie przy użyciu składni i uzupełniania parametrów.</span><span class="sxs-lookup"><span data-stu-id="d4e57-141">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="d4e57-142">Składniki są renderowane w pamięci podręcznej Document Object Model (DOM) przeglądarki o nazwie *drzewo renderowania*, która jest używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="d4e57-142">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="d4e57-143"> webassembly</span><span class="sxs-lookup"><span data-stu-id="d4e57-143"> WebAssembly</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="d4e57-144"> webassembly to jednostronicowa platforma aplikacji służąca do tworzenia interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d4e57-144"> WebAssembly is a single-page app framework for building interactive client-side web apps with .NET.</span></span> Blazor<span data-ttu-id="d4e57-145"> webassembly używa otwartych standardów sieci Web bez wtyczek lub kodu transpilation i działa we wszystkich nowoczesnych przeglądarkach sieci Web, w tym w przeglądarkach mobilnych.</span><span class="sxs-lookup"><span data-stu-id="d4e57-145"> WebAssembly uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="d4e57-146">Uruchamianie kodu platformy .NET wewnątrz przeglądarek sieci Web jest możliwe przez [zestaw webassembly](https://webassembly.org) (skrócony *wasm*).</span><span class="sxs-lookup"><span data-stu-id="d4e57-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="d4e57-147">Webassembly to kompaktowy format kodu bajtowego zoptymalizowany pod kątem szybkiego pobierania i maksymalnej szybkości wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d4e57-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="d4e57-148">Webassembly to otwarty standard sieci Web, który jest obsługiwany w przeglądarkach sieci Web bez wtyczek.</span><span class="sxs-lookup"><span data-stu-id="d4e57-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="d4e57-149">Kod webassembly może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pośrednictwem języka JavaScript, nazywanego *współdziałaniem JavaScript* (lub *międzyoperacyjną JavaScript*).</span><span class="sxs-lookup"><span data-stu-id="d4e57-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="d4e57-150">Kod .NET wykonywany za pośrednictwem webassembly w przeglądarce jest uruchamiany w piaskownicy języka JavaScript przeglądarki z ochroną, którą piaskownica zapewnia przed złośliwymi działaniami na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="d4e57-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![[! OP. NO-LOC (Blazor)]<span data-ttu-id="d4e57-151"> zestaw webassembly uruchamia kod platformy .NET w przeglądarce z zestawem webassembly.</span><span class="sxs-lookup"><span data-stu-id="d4e57-151"> WebAssembly runs .NET code in the browser with WebAssembly.</span></span>](index/_static/blazor-webassembly.png)

<span data-ttu-id="d4e57-152">Po skompilowaniu i uruchomieniu aplikacji Blazor webassembly w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="d4e57-152">When a Blazor WebAssembly app is built and run in a browser:</span></span>

* <span data-ttu-id="d4e57-153">C#pliki kodu i pliki Razor są kompilowane do zestawów .NET.</span><span class="sxs-lookup"><span data-stu-id="d4e57-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="d4e57-154">Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d4e57-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* Blazor<span data-ttu-id="d4e57-155"> uruchamiania zestawu webassembly w środowisku uruchomieniowym platformy .NET i konfiguruje środowisko uruchomieniowe w celu załadowania zestawów dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4e57-155"> WebAssembly bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="d4e57-156">Blazor środowisko uruchomieniowe webassembly używa interfejsu JavaScript Interop do obsługi operacji manipulowania DOM i interfejsów API przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="d4e57-156">The Blazor WebAssembly runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="d4e57-157">Rozmiar opublikowanej aplikacji, jej *rozmiaru ładunku*, jest krytycznym czynnikiem wydajności dla useability aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4e57-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="d4e57-158">Pobieranie dużej aplikacji do przeglądarki zajmuje stosunkowo dużo czasu, co zmniejsza środowisko użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> Blazor<span data-ttu-id="d4e57-159"> webassembly optymalizuje rozmiar ładunku, aby skrócić czas pobierania:</span><span class="sxs-lookup"><span data-stu-id="d4e57-159"> WebAssembly optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="d4e57-160">Nieużywany kod jest usuwany z aplikacji, gdy jest publikowany przez [konsolidator języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="d4e57-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="d4e57-161">Odpowiedzi HTTP są kompresowane.</span><span class="sxs-lookup"><span data-stu-id="d4e57-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="d4e57-162">Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d4e57-162">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="d4e57-163">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="d4e57-163">Blazor Server</span></span>

Blazor<span data-ttu-id="d4e57-164"> oddziela logikę renderowania składników od sposobu stosowania aktualizacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4e57-164"> decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="d4e57-165">Serwer Blazor zapewnia obsługę hostowania składników Razor na serwerze w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4e57-165">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="d4e57-166">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="d4e57-166">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="d4e57-167">Środowisko uruchomieniowe obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera i zastosowanie aktualizacji interfejsu użytkownika wysyłanych przez serwer z powrotem do przeglądarki po uruchomieniu składników programu.</span><span class="sxs-lookup"><span data-stu-id="d4e57-167">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="d4e57-168">Połączenie używane przez Blazor Server do komunikowania się z przeglądarką jest również używane do obsługi wywołań międzyoperacyjnych języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4e57-168">The connection used by Blazor Server to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![[! OP. Nie-LOC (Blazor)]<span data-ttu-id="d4e57-169"> serwer uruchamia kod platformy .NET na serwerze i współdziała z Document Object Model na kliencie za pośrednictwem [! OP. Nie-LOC (sygnalizujący)] połączenie</span><span class="sxs-lookup"><span data-stu-id="d4e57-169"> Server runs .NET code on the server and interacts with the Document Object Model on the client over a SignalR connection</span></span>](index/_static/blazor-server.png)

## <a name="javascript-interop"></a><span data-ttu-id="d4e57-170">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="d4e57-170">JavaScript interop</span></span>

<span data-ttu-id="d4e57-171">W przypadku aplikacji, które wymagają bibliotek JavaScript innych firm i dostępu do interfejsów API przeglądarki, składniki współdziałają z JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4e57-171">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="d4e57-172">Składniki mogą korzystać z dowolnej biblioteki lub interfejsu API, który może być używany przez język JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4e57-172">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="d4e57-173">C#kod może wywołać kod JavaScript, a kod JavaScript może wywołać C# kod.</span><span class="sxs-lookup"><span data-stu-id="d4e57-173">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="d4e57-174">Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="d4e57-174">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="d4e57-175">Udostępnianie kodu i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="d4e57-175">Code sharing and .NET Standard</span></span>

Blazor<span data-ttu-id="d4e57-176"> implementuje [.NET Standard 2,0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="d4e57-176"> implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="d4e57-177">.NET Standard jest formalną specyfikacją interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d4e57-177">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="d4e57-178">Biblioteki klas .NET Standard mogą być współużytkowane przez różne platformy .NET, takie jak Blazor, .NET Framework, .NET Core, Xamarin, mono i Unity.</span><span class="sxs-lookup"><span data-stu-id="d4e57-178">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="d4e57-179">Interfejsy API, które nie są stosowane w przeglądarce sieci Web (na przykład dostęp do systemu plików, otwieranie gniazda i wątkowość), zgłasza <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="d4e57-179">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4e57-180">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d4e57-180">Additional resources</span></span>

* [<span data-ttu-id="d4e57-181">Zestaw webassembly</span><span class="sxs-lookup"><span data-stu-id="d4e57-181">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* <xref:tutorials/signalr-blazor-webassembly>
* [<span data-ttu-id="d4e57-182">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="d4e57-182">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="d4e57-183">HTML</span><span class="sxs-lookup"><span data-stu-id="d4e57-183">HTML</span></span>](https://www.w3.org/html/)
* <span data-ttu-id="d4e57-184">[Firma Awesome Blazor](https://github.com/AdrienTorris/awesome-blazor) linki społecznościowe</span><span class="sxs-lookup"><span data-stu-id="d4e57-184">[Awesome Blazor](https://github.com/AdrienTorris/awesome-blazor) community links</span></span>
