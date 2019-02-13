---
title: Wprowadzenie do składników Razor
author: guardrex
description: Poznaj składniki programu ASP.NET Core Razor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: razor-components/index
ms.openlocfilehash: 04a73d33cee0deedaf3dc97395836a936b580fbd
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159529"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="f19ec-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="f19ec-103">Introduction to Razor Components</span></span>

<span data-ttu-id="f19ec-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f19ec-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f19ec-105">*Składniki razor* to sposób na tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="f19ec-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="f19ec-106">Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f19ec-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="f19ec-107">Udostępnianie aplikacji po stronie serwera i klienta logiki napisane przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f19ec-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="f19ec-108">Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="f19ec-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="f19ec-109">Składniki razor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:</span><span class="sxs-lookup"><span data-stu-id="f19ec-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="f19ec-110">Parametry</span><span class="sxs-lookup"><span data-stu-id="f19ec-110">Parameters</span></span>
* <span data-ttu-id="f19ec-111">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="f19ec-111">Event handling</span></span>
* <span data-ttu-id="f19ec-112">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="f19ec-112">Data binding</span></span>
* <span data-ttu-id="f19ec-113">Routing</span><span class="sxs-lookup"><span data-stu-id="f19ec-113">Routing</span></span>
* <span data-ttu-id="f19ec-114">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="f19ec-114">Dependency injection</span></span>
* <span data-ttu-id="f19ec-115">Układy</span><span class="sxs-lookup"><span data-stu-id="f19ec-115">Layouts</span></span>
* <span data-ttu-id="f19ec-116">Tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="f19ec-116">Templating</span></span>
* <span data-ttu-id="f19ec-117">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="f19ec-117">Cascading values</span></span>

<span data-ttu-id="f19ec-118">Składniki razor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f19ec-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="f19ec-119">Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f19ec-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="f19ec-120">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="f19ec-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="f19ec-121">Środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="f19ec-121">The runtime:</span></span>

* <span data-ttu-id="f19ec-122">Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.</span><span class="sxs-lookup"><span data-stu-id="f19ec-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="f19ec-123">Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.</span><span class="sxs-lookup"><span data-stu-id="f19ec-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="f19ec-124">Połączenie używane przez składniki Razor do komunikowania się za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f19ec-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="f19ec-126">Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="f19ec-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="f19ec-127">*Blazor* jest eksperymentalna modelu hostingu po stronie klienta, składników Razor.</span><span class="sxs-lookup"><span data-stu-id="f19ec-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="f19ec-128">Blazor działające na platformie .NET w przeglądarce, za pomocą standardów sieci web Otwórz bez transpilation wtyczki lub kodu.</span><span class="sxs-lookup"><span data-stu-id="f19ec-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="f19ec-129">Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="f19ec-129">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="f19ec-130">Składniki</span><span class="sxs-lookup"><span data-stu-id="f19ec-130">Components</span></span>

<span data-ttu-id="f19ec-131">A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="f19ec-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="f19ec-132">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f19ec-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="f19ec-133">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="f19ec-133">Components can be nested and reused.</span></span>

<span data-ttu-id="f19ec-134">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="f19ec-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="f19ec-135">Klasa, albo mogą być napisane w postaci znaczników strony Razor (*.cshtml*) lub jako C# klasy (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="f19ec-135">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="f19ec-136">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu.</span><span class="sxs-lookup"><span data-stu-id="f19ec-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="f19ec-137">Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="f19ec-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="f19ec-138">Strony razor i MVC widoki również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="f19ec-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="f19ec-139">W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f19ec-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="f19ec-140">Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="f19ec-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="f19ec-141">Następujący kod jest przykładem składnika niestandardowy dialog w pliku Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="f19ec-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

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

<span data-ttu-id="f19ec-142">Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="f19ec-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="f19ec-143">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.</span><span class="sxs-lookup"><span data-stu-id="f19ec-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="f19ec-144">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="f19ec-144">JavaScript interop</span></span>

<span data-ttu-id="f19ec-145">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f19ec-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="f19ec-146">Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="f19ec-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="f19ec-147">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="f19ec-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="f19ec-148">Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="f19ec-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f19ec-149">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f19ec-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="f19ec-150">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="f19ec-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="f19ec-151">Przewodnik dla języka C#</span><span class="sxs-lookup"><span data-stu-id="f19ec-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="f19ec-152">HTML</span><span class="sxs-lookup"><span data-stu-id="f19ec-152">HTML</span></span>](https://www.w3.org/html/)
