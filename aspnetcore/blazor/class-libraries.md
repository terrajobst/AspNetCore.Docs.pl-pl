---
title: ASP.NET Core biblioteki klas składników Razor
author: guardrex
description: Odkryj, jak składniki mogą być dołączane do aplikacji Blazor z zewnętrznej biblioteki składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/class-libraries
ms.openlocfilehash: 6e93d48bbc684845952c3db8935ccc8b190044b7
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030342"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="d6f1b-103">ASP.NET Core biblioteki klas składników Razor</span><span class="sxs-lookup"><span data-stu-id="d6f1b-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="d6f1b-104">Autor [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="d6f1b-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="d6f1b-105">Składniki mogą być współużytkowane w [bibliotece klas Razor (RCL)](xref:razor-pages/ui-class) w różnych projektach.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="d6f1b-106">*Biblioteka klas składników Razor* może być dołączona z:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="d6f1b-107">Inny projekt w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-107">Another project in the solution.</span></span>
* <span data-ttu-id="d6f1b-108">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-108">A NuGet package.</span></span>
* <span data-ttu-id="d6f1b-109">Przywoływana Biblioteka platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-109">A referenced .NET library.</span></span>

<span data-ttu-id="d6f1b-110">Podobnie jak składniki są zwykłymi typami .NET, składniki udostępniane przez RCL są normalnymi zestawami platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="d6f1b-111">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="d6f1b-111">Create an RCL</span></span>

<span data-ttu-id="d6f1b-112">Postępuj zgodnie ze wskazówkami zawartymi w <xref:blazor/get-started> artykule, aby skonfigurować środowisko dla programu Blazor.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6f1b-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6f1b-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d6f1b-114">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-114">Create a new project.</span></span>
1. <span data-ttu-id="d6f1b-115">Wybierz **bibliotekę klas Razor**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="d6f1b-116">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-116">Select **Next**.</span></span>
1. <span data-ttu-id="d6f1b-117">W oknie dialogowym **Tworzenie nowej biblioteki klas Razor** wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="d6f1b-118">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d6f1b-119">W przykładach w tym temacie użyto nazwy `MyComponentLib1`projektu.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="d6f1b-120">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-120">Select **Create**.</span></span>
1. <span data-ttu-id="d6f1b-121">Dodaj RCL do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="d6f1b-122">Kliknij prawym przyciskiem myszy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-122">Right-click the solution.</span></span> <span data-ttu-id="d6f1b-123">Wybierz pozycję **Dodaj** > **istniejący projekt**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="d6f1b-124">Przejdź do pliku projektu RCL.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="d6f1b-125">Wybierz plik projektu RCL ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="d6f1b-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="d6f1b-126">Dodaj odwołanie RCL z aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="d6f1b-127">Kliknij prawym przyciskiem myszy projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-127">Right-click the app project.</span></span> <span data-ttu-id="d6f1b-128">Wybierz pozycję **Dodaj** > **odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="d6f1b-129">Wybierz projekt RCL.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-129">Select the RCL project.</span></span> <span data-ttu-id="d6f1b-130">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d6f1b-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d6f1b-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="d6f1b-132">Użyj szablonu **biblioteki klas Razor** (`razorclasslib`) z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="d6f1b-133">W poniższym przykładzie jest tworzony RCL o nazwie `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="d6f1b-134">Folder, który `MyComponentLib1` ma zostać utworzony, jest tworzony automatycznie podczas wykonywania polecenia:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="d6f1b-135">Aby dodać bibliotekę do istniejącego projektu, użyj polecenia [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="d6f1b-136">W poniższym przykładzie RCL jest dodawany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="d6f1b-137">Wykonaj następujące polecenie z folderu projektu aplikacji z ścieżką do biblioteki:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="d6f1b-138">RCLs nie są obsługiwane w przypadku aplikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="d6f1b-138">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="d6f1b-139">W bieżącej wersji zapoznawczej ASP.NET Core 3,0 biblioteki klas Razor nie są zgodne z Blazor aplikacjami po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-139">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="d6f1b-140">W przypadku aplikacji po stronie klienta Blazor należy użyć biblioteki składników Blazor utworzonej przez `blazorlib` szablon w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-140">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="d6f1b-141">Biblioteki składników korzystające `blazorlib` z szablonu mogą zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-141">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="d6f1b-142">W czasie kompilacji pliki statyczne są osadzane w skompilowanym pliku zestawu ( *. dll*), co pozwala na użycie składników bez konieczności zajmowania się sposobem uwzględniania zasobów.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-142">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="d6f1b-143">Wszystkie pliki znajdujące się `content` w katalogu są oznaczone jako zasób osadzony.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-143">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="d6f1b-144">Korzystanie ze składnika biblioteki</span><span class="sxs-lookup"><span data-stu-id="d6f1b-144">Consume a library component</span></span>

<span data-ttu-id="d6f1b-145">Aby można było korzystać ze składników zdefiniowanych w bibliotece w innym projekcie, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-145">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="d6f1b-146">Użyj pełnej nazwy typu z przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-146">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="d6f1b-147">Użyj dyrektywy [ \@using przy użyciu](xref:mvc/views/razor#using) składni Razor.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-147">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="d6f1b-148">Poszczególne składniki można dodawać według nazwy.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-148">Individual components can be added by name.</span></span>

<span data-ttu-id="d6f1b-149">W poniższych przykładach `MyComponentLib1` jest biblioteka składników `SalesReport` zawierająca składnik.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-149">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="d6f1b-150">Do `SalesReport` składnika można odwoływać się za pomocą jego pełnej nazwy typu z przestrzenią nazw:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-150">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="d6f1b-151">Składnik może być również przywoływany, jeśli biblioteka została wprowadzona do zakresu przy `@using` użyciu dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-151">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="d6f1b-152">Uwzględnij dyrektywę w pliku *_Import. Razor* najwyższego poziomu, aby udostępnić składniki biblioteki dla całego projektu. `@using MyComponentLib1`</span><span class="sxs-lookup"><span data-stu-id="d6f1b-152">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="d6f1b-153">Dodaj dyrektywę do pliku *_Import. Razor* na dowolnym poziomie, aby zastosować przestrzeń nazw do pojedynczej strony lub zestawu stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-153">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="d6f1b-154">Kompilowanie, pakowanie i dostarczanie do narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="d6f1b-154">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="d6f1b-155">Ponieważ biblioteki składników są standardowymi bibliotekami .NET, pakowanie i dostarczanie ich do narzędzia NuGet nie różni się od pakowania i wysyłania żadnej biblioteki do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-155">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="d6f1b-156">Pakowanie jest wykonywane przy użyciu polecenia [pakietu dotnet](/dotnet/core/tools/dotnet-pack) w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-156">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="d6f1b-157">Przekaż pakiet do narzędzia NuGet przy użyciu polecenia [publikowania NuGet programu dotnet](/dotnet/core/tools/dotnet-nuget-push) w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d6f1b-157">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="d6f1b-158">W przypadku korzystania `blazorlib` z szablonu zasoby statyczne są zawarte w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-158">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="d6f1b-159">Klienci biblioteki automatycznie odbierają skrypty i arkusze stylów, dlatego klienci nie muszą ręcznie instalować zasobów.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-159">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="d6f1b-160">Tworzenie biblioteki klas składników Razor ze statycznymi zasobami</span><span class="sxs-lookup"><span data-stu-id="d6f1b-160">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="d6f1b-161">RCL może zawierać statyczne zasoby.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-161">An RCL can include static assets.</span></span> <span data-ttu-id="d6f1b-162">Zasoby statyczne są dostępne dla każdej aplikacji, która korzysta z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-162">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="d6f1b-163">Aby uzyskać więcej informacji, zobacz <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="d6f1b-163">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6f1b-164">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d6f1b-164">Additional resources</span></span>

* <xref:razor-pages/ui-class>
