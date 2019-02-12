---
title: Biblioteki klas składników razor
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Razor składników z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: f5073337db88f05f3911fdfb1f29e7302c7cddec
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102908"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="5c5d5-103">Biblioteki klas składników razor</span><span class="sxs-lookup"><span data-stu-id="5c5d5-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="5c5d5-104">Przez [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="5c5d5-104">By [Simon Timms](https://github.com/stimms)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

> [!NOTE]
> <span data-ttu-id="5c5d5-105">Zestaw SDK platformy .NET Core 3.0 w wersji zapoznawczej 2 nie zawiera szablon projektu dla bibliotek klas składników Razor, ale oczekuje się dodać szablon w przyszłej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="5c5d5-106">W międzyczasie można użyć szablonu biblioteki klas składników Blazor szczegółowo opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="5c5d5-107">Składniki mogą być udostępniane w bibliotekach składnika w projektach.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="5c5d5-108">Składniki można uwzględnić od:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-108">Components can be included from:</span></span>

* <span data-ttu-id="5c5d5-109">Innego projektu w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-109">Another project in the solution.</span></span>
* <span data-ttu-id="5c5d5-110">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-110">A NuGet package.</span></span>
* <span data-ttu-id="5c5d5-111">Odwołania biblioteki .NET.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-111">A referenced .NET library.</span></span>

<span data-ttu-id="5c5d5-112">Tak, jak składniki są regularnie typów .NET, składnik biblioteki są normalne zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="5c5d5-113">Aby utworzyć nową bibliotekę składników, należy użyć `blazorlib` szablon [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="5c5d5-114">Szablon jest częścią szablonów zainstalowanych podczas [Konfigurowanie składników Razor](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="5c5d5-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="5c5d5-115">Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet sln](/dotnet/core/tools/dotnet-sln) polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="5c5d5-116">Biblioteki składników może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="5c5d5-117">W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego (*.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="5c5d5-118">Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="5c5d5-119">Używanie składnik biblioteki</span><span class="sxs-lookup"><span data-stu-id="5c5d5-119">Consume a library component</span></span>

<span data-ttu-id="5c5d5-120">Aby można było korzystających ze składników zdefiniowane w bibliotece w innym projekcie [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) dyrektywa musi być używana.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="5c5d5-121">Poszczególne składniki mogą być dodawane według nazwy.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-121">Individual components may be added by name.</span></span> <span data-ttu-id="5c5d5-122">Na przykład następująca dyrektywa dodaje `Component1` z `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="5c5d5-123">Ogólny format dyrektywy jest:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="5c5d5-124">Jednak jest wspólne dla wszystkich składników z zestawu przy użyciu symboli wieloznacznych zawierają:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="5c5d5-125">`@addTagHelper` Mogą być dołączane dyrektywą *_ViewImport.cshtml* dokonać składników dostępne dla całego projektu lub zastosowane pojedynczej strony lub zbiór stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="5c5d5-126">Za pomocą `@addTagHelper` dyrektywy w miejscu, składniki biblioteki składników mogą być używane tak, jakby znajdowały się w tym samym zestawie co aplikacja.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="5c5d5-127">Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="5c5d5-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="5c5d5-128">Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="5c5d5-129">Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="5c5d5-130">Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c5d5-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="5c5d5-131">Wszystkie dołączone zasoby statyczne znajdują się w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="5c5d5-132">Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.</span><span class="sxs-lookup"><span data-stu-id="5c5d5-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
