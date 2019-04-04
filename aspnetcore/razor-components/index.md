---
title: Wprowadzenie do składników Razor programu ASP.NET Core
author: guardrex
description: Poznaj składniki programu ASP.NET Core Razor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 03/27/2019
uid: razor-components/index
ms.openlocfilehash: 43d5cf1d752b66a531c8d974deeb5a5fc8e94b43
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012659"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="08d1f-103">Wprowadzenie do składników Razor</span><span class="sxs-lookup"><span data-stu-id="08d1f-103">Introduction to Razor Components</span></span>

<span data-ttu-id="08d1f-104">Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="08d1f-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="08d1f-105">*Składniki razor*, wprowadzone w programie ASP.NET Core 3.0 (wersja zapoznawcza), jest sposób tworzenia interaktywnego po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="08d1f-105">*Razor Components*, introduced in ASP.NET Core 3.0 (Preview), is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="08d1f-106">Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08d1f-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="08d1f-107">Udostępnianie aplikacji po stronie serwera i klienta logiki napisane przy użyciu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="08d1f-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="08d1f-108">Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.</span><span class="sxs-lookup"><span data-stu-id="08d1f-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="08d1f-109">Składniki razor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:</span><span class="sxs-lookup"><span data-stu-id="08d1f-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="08d1f-110">Parametry</span><span class="sxs-lookup"><span data-stu-id="08d1f-110">Parameters</span></span>
* <span data-ttu-id="08d1f-111">Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="08d1f-111">Event handling</span></span>
* <span data-ttu-id="08d1f-112">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="08d1f-112">Data binding</span></span>
* <span data-ttu-id="08d1f-113">Routing</span><span class="sxs-lookup"><span data-stu-id="08d1f-113">Routing</span></span>
* <span data-ttu-id="08d1f-114">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="08d1f-114">Dependency injection</span></span>
* <span data-ttu-id="08d1f-115">Układy</span><span class="sxs-lookup"><span data-stu-id="08d1f-115">Layouts</span></span>
* <span data-ttu-id="08d1f-116">Tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="08d1f-116">Templating</span></span>
* <span data-ttu-id="08d1f-117">Kaskadowe wartości</span><span class="sxs-lookup"><span data-stu-id="08d1f-117">Cascading values</span></span>

<span data-ttu-id="08d1f-118">Składniki razor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="08d1f-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="08d1f-119">Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="08d1f-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="08d1f-120">Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="08d1f-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="08d1f-121">Środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="08d1f-121">The runtime:</span></span>

* <span data-ttu-id="08d1f-122">Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.</span><span class="sxs-lookup"><span data-stu-id="08d1f-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="08d1f-123">Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.</span><span class="sxs-lookup"><span data-stu-id="08d1f-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="08d1f-124">Połączenie używane przez składniki Razor do komunikowania się za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08d1f-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="08d1f-126">Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="08d1f-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="08d1f-127">*Blazor* jest eksperymentalna modelu hostingu po stronie klienta, składników Razor.</span><span class="sxs-lookup"><span data-stu-id="08d1f-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="08d1f-128">Blazor działające na platformie .NET w przeglądarce, za pomocą standardów sieci web Otwórz bez transpilation wtyczki lub kodu.</span><span class="sxs-lookup"><span data-stu-id="08d1f-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="08d1f-129">Aby uzyskać więcej informacji, zobacz <xref:spa/blazor/index> i <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="08d1f-129">For more information, see <xref:spa/blazor/index> and <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="08d1f-130">Składniki</span><span class="sxs-lookup"><span data-stu-id="08d1f-130">Components</span></span>

<span data-ttu-id="08d1f-131">A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="08d1f-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="08d1f-132">Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="08d1f-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="08d1f-133">Składniki może zagnieżdżone i ponownie używane.</span><span class="sxs-lookup"><span data-stu-id="08d1f-133">Components can be nested and reused.</span></span>

<span data-ttu-id="08d1f-134">Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="08d1f-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="08d1f-135">Klasy zwykle są zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="08d1f-135">The class is normally written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="08d1f-136">[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu.</span><span class="sxs-lookup"><span data-stu-id="08d1f-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="08d1f-137">Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="08d1f-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="08d1f-138">Strony razor i MVC widoki również używają Razor.</span><span class="sxs-lookup"><span data-stu-id="08d1f-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="08d1f-139">W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="08d1f-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="08d1f-140">Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.</span><span class="sxs-lookup"><span data-stu-id="08d1f-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="08d1f-141">Następujący kod jest przykładem składnika niestandardowy dialog w pliku Razor (*DialogComponent.razor*):</span><span class="sxs-lookup"><span data-stu-id="08d1f-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.razor*):</span></span>

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

<span data-ttu-id="08d1f-142">Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.</span><span class="sxs-lookup"><span data-stu-id="08d1f-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="08d1f-143">Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.</span><span class="sxs-lookup"><span data-stu-id="08d1f-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="08d1f-144">Międzyoperacyjność w języku JavaScript</span><span class="sxs-lookup"><span data-stu-id="08d1f-144">JavaScript interop</span></span>

<span data-ttu-id="08d1f-145">W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08d1f-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="08d1f-146">Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać.</span><span class="sxs-lookup"><span data-stu-id="08d1f-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="08d1f-147">C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu.</span><span class="sxs-lookup"><span data-stu-id="08d1f-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="08d1f-148">Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="08d1f-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08d1f-149">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="08d1f-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="08d1f-150">Format WebAssembly</span><span class="sxs-lookup"><span data-stu-id="08d1f-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="08d1f-151">Przewodnik po języku C#</span><span class="sxs-lookup"><span data-stu-id="08d1f-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="08d1f-152">HTML</span><span class="sxs-lookup"><span data-stu-id="08d1f-152">HTML</span></span>](https://www.w3.org/html/)
