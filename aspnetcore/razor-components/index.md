---
title: Wprowadzenie do składników Razor
author: guardrex
description: Poznaj składniki Razor programu ASP.NET Core, .NET sieci web framework przy użyciu C#/Razor, jak i HTML.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/index
ms.openlocfilehash: e8d75a0647704f1ff05e70a96abbb2e448868165
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102932"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="e8254-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="e8254-103">Introduction to Razor Components</span></span>

<span data-ttu-id="e8254-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e8254-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="e8254-105">*Składniki razor* to nowy sposób tworzyć interaktywne po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="e8254-105">*Razor Components* is a new way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="e8254-106">Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8254-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="e8254-107">Udostępnianie aplikacji po stronie serwera i klienta logiki napisane przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e8254-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="e8254-108">Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="e8254-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

## <a name="components"></a><span data-ttu-id="e8254-109">Składniki</span><span class="sxs-lookup"><span data-stu-id="e8254-109">Components</span></span>

<span data-ttu-id="e8254-110">A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="e8254-110">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="e8254-111">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e8254-111">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="e8254-112">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="e8254-112">Components can be nested and reused.</span></span>

<span data-ttu-id="e8254-113">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="e8254-113">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="e8254-114">Klasa, albo mogą być napisane w postaci znaczników strony Razor (*.cshtml*) lub jako C# klasy (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="e8254-114">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="e8254-115">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu.</span><span class="sxs-lookup"><span data-stu-id="e8254-115">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="e8254-116">Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="e8254-116">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="e8254-117">Strony razor i MVC widoki również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="e8254-117">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="e8254-118">W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e8254-118">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="e8254-119">Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="e8254-119">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="e8254-120">Następujący kod jest przykładem składnika niestandardowy dialog w pliku Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="e8254-120">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

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

<span data-ttu-id="e8254-121">Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="e8254-121">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="e8254-122">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.</span><span class="sxs-lookup"><span data-stu-id="e8254-122">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="e8254-123">Składniki razor obsługują urządzenia podstawowe wymagane przez większość aplikacji, w tym:</span><span class="sxs-lookup"><span data-stu-id="e8254-123">Razor Components support core facilities required by most apps, including:</span></span>

* <span data-ttu-id="e8254-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="e8254-124">Parameters</span></span>
* <span data-ttu-id="e8254-125">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="e8254-125">Event handling</span></span>
* <span data-ttu-id="e8254-126">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="e8254-126">Data binding</span></span>
* <span data-ttu-id="e8254-127">Routing</span><span class="sxs-lookup"><span data-stu-id="e8254-127">Routing</span></span>
* <span data-ttu-id="e8254-128">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="e8254-128">Dependency injection</span></span>
* <span data-ttu-id="e8254-129">Układy</span><span class="sxs-lookup"><span data-stu-id="e8254-129">Layouts</span></span>
* <span data-ttu-id="e8254-130">Tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="e8254-130">Templating</span></span>
* <span data-ttu-id="e8254-131">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="e8254-131">Cascading values</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="e8254-132">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8254-132">JavaScript interop</span></span>

<span data-ttu-id="e8254-133">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarce interfejsów API składniki Razor współdziałać z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8254-133">For apps that require third-party JavaScript libraries and browser APIs, Razor Components interoperate with JavaScript.</span></span> <span data-ttu-id="e8254-134">Składniki razor są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="e8254-134">Razor Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="e8254-135">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="e8254-135">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="e8254-136">Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="e8254-136">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="e8254-137">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="e8254-137">Hosting models</span></span>

### <a name="server-side-hosting-model"></a><span data-ttu-id="e8254-138">Model hostingu po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="e8254-138">Server-side hosting model</span></span>

<span data-ttu-id="e8254-139">Ponieważ składniki Razor oddzielenie logiki renderowania składnika w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jak Razor składniki mogą być hostowane jest elastyczność.</span><span class="sxs-lookup"><span data-stu-id="e8254-139">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="e8254-140">Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="e8254-140">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="e8254-141">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="e8254-141">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="e8254-142">Środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="e8254-142">The runtime:</span></span>

* <span data-ttu-id="e8254-143">Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.</span><span class="sxs-lookup"><span data-stu-id="e8254-143">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="e8254-144">Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.</span><span class="sxs-lookup"><span data-stu-id="e8254-144">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="e8254-145">Połączenie używane przez składniki Razor do komunikowania się za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8254-145">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="e8254-147">Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="e8254-147">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

### <a name="client-side-hosting-model"></a><span data-ttu-id="e8254-148">Model hostingu po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="e8254-148">Client-side hosting model</span></span>

<span data-ttu-id="e8254-149">*Blazor* jest eksperymentalna modelu hostingu po stronie klienta, składników Razor.</span><span class="sxs-lookup"><span data-stu-id="e8254-149">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="e8254-150">Blazor działające na platformie .NET w przeglądarce, za pomocą standardów sieci web Otwórz bez transpilation wtyczki lub kodu.</span><span class="sxs-lookup"><span data-stu-id="e8254-150">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="e8254-151">Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="e8254-151">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="e8254-152">Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*).</span><span class="sxs-lookup"><span data-stu-id="e8254-152">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="e8254-153">Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek.</span><span class="sxs-lookup"><span data-stu-id="e8254-153">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="e8254-154">Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.</span><span class="sxs-lookup"><span data-stu-id="e8254-154">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="e8254-155">Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8254-155">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="e8254-156">W tym samym czasie format WebAssembly kod jest uruchamiany w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="e8254-156">At the same time, WebAssembly code runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor uruchamia kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="e8254-158">Przy aplikacji Blazor jest wbudowana i uruchomić w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="e8254-158">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="e8254-159">C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e8254-159">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="e8254-160">Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e8254-160">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="e8254-161">Blazor używa języka JavaScript do uruchamiania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy.</span><span class="sxs-lookup"><span data-stu-id="e8254-161">Blazor uses JavaScript to bootstrap the .NET runtime and configures the runtime to load assemblies.</span></span> <span data-ttu-id="e8254-162">Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe Blazor za pomocą międzyoperacyjności języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8254-162">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="e8254-163">Aby zapewnić obsługę starszych przeglądarkach, które nie obsługują format WebAssembly, użyj [po stronie serwera modelu hostingu](#server-side-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="e8254-163">To support older browsers that don't support WebAssembly, use the [server-side hosting model](#server-side-hosting-model).</span></span>

<span data-ttu-id="e8254-164">Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="e8254-164">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8254-165">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e8254-165">Additional resources</span></span>

* [<span data-ttu-id="e8254-166">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="e8254-166">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="e8254-167">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="e8254-167">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="e8254-168">HTML</span><span class="sxs-lookup"><span data-stu-id="e8254-168">HTML</span></span>](https://www.w3.org/html/)
