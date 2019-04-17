---
title: Biblioteki klas składników razor
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/class-libraries
ms.openlocfilehash: 6c0b741de1e3b9ad2b226cc376f06ad8365542e8
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614872"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="99ece-103">Biblioteki klas składników razor</span><span class="sxs-lookup"><span data-stu-id="99ece-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="99ece-104">Przez [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="99ece-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="99ece-105">Składniki mogą być udostępniane w bibliotekach klas Razor w projektach.</span><span class="sxs-lookup"><span data-stu-id="99ece-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="99ece-106">Składniki można uwzględnić od:</span><span class="sxs-lookup"><span data-stu-id="99ece-106">Components can be included from:</span></span>

* <span data-ttu-id="99ece-107">Innego projektu w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="99ece-107">Another project in the solution.</span></span>
* <span data-ttu-id="99ece-108">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="99ece-108">A NuGet package.</span></span>
* <span data-ttu-id="99ece-109">Odwołania biblioteki .NET.</span><span class="sxs-lookup"><span data-stu-id="99ece-109">A referenced .NET library.</span></span>

<span data-ttu-id="99ece-110">Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez biblioteki klas Razor są normalne zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="99ece-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="99ece-111">Użyj `razorclasslib` szablonu (biblioteki klas Razor) za pomocą [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia:</span><span class="sxs-lookup"><span data-stu-id="99ece-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="99ece-112">Dodaj pliki Razor składników (*.razor*) do biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="99ece-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="99ece-113">Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet sln](/dotnet/core/tools/dotnet-sln) polecenia:</span><span class="sxs-lookup"><span data-stu-id="99ece-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99ece-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99ece-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="99ece-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="99ece-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="99ece-116">Biblioteki klas razor nie są zgodne z aplikacjami Blazor w ASP.NET Core w wersji zapoznawczej 3.</span><span class="sxs-lookup"><span data-stu-id="99ece-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="99ece-117">Do tworzenia składników w bibliotece, które mogą być udostępniane innym Blazor po stronie klienta i Razor składniki po stronie serwera aplikacji, użyj utworzonych przez bibliotekę klas Blazor `blazorlib` szablonu.</span><span class="sxs-lookup"><span data-stu-id="99ece-117">To create components in a library that can be shared with Blazor client-side and Razor Components server-side apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="99ece-118">Biblioteki klas razor statycznych zasobów w ASP.NET Core w wersji zapoznawczej 3 nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="99ece-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="99ece-119">Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="99ece-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="99ece-120">W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego (*.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów.</span><span class="sxs-lookup"><span data-stu-id="99ece-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="99ece-121">Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="99ece-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="99ece-122">Używanie składnik biblioteki</span><span class="sxs-lookup"><span data-stu-id="99ece-122">Consume a library component</span></span>

<span data-ttu-id="99ece-123">Aby można było korzystających ze składników zdefiniowane w bibliotece w innym projekcie [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) dyrektywa musi być używana.</span><span class="sxs-lookup"><span data-stu-id="99ece-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="99ece-124">Poszczególne składniki mogą być dodawane według nazwy.</span><span class="sxs-lookup"><span data-stu-id="99ece-124">Individual components may be added by name.</span></span>

<span data-ttu-id="99ece-125">Ogólny format dyrektywy jest:</span><span class="sxs-lookup"><span data-stu-id="99ece-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="99ece-126">Na przykład następująca dyrektywa dodaje `Component1` z `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="99ece-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="99ece-127">Jednak jest często obejmują wszystkie elementy z zestawu przy użyciu symboli wieloznacznych (`*`):</span><span class="sxs-lookup"><span data-stu-id="99ece-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="99ece-128">`@addTagHelper` Mogą być dołączane dyrektywą *_ViewImport.cshtml* dokonać składników dostępne dla całego projektu lub zastosowane pojedynczej strony lub zbiór stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="99ece-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="99ece-129">Za pomocą `@addTagHelper` dyrektywy w miejscu, składniki biblioteki składników mogą być używane tak, jakby znajdowały się w tym samym zestawie co aplikacja.</span><span class="sxs-lookup"><span data-stu-id="99ece-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="99ece-130">Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="99ece-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="99ece-131">Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="99ece-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="99ece-132">Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia:</span><span class="sxs-lookup"><span data-stu-id="99ece-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="99ece-133">Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia:</span><span class="sxs-lookup"><span data-stu-id="99ece-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="99ece-134">Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="99ece-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="99ece-135">Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.</span><span class="sxs-lookup"><span data-stu-id="99ece-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
