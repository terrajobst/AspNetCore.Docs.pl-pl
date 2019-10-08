---
title: ASP.NET Core biblioteki klas składników Razor
author: guardrex
description: Odkryj, jak składniki mogą być dołączane do aplikacji Blazor z zewnętrznej biblioteki składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/class-libraries
ms.openlocfilehash: 2e042b43c6db24e0ecac727be100575fe1275e17
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999774"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="bda4f-103">ASP.NET Core biblioteki klas składników Razor</span><span class="sxs-lookup"><span data-stu-id="bda4f-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="bda4f-104">Autor [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="bda4f-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="bda4f-105">Składniki mogą być współużytkowane w [bibliotece klas Razor (RCL)](xref:razor-pages/ui-class) w różnych projektach.</span><span class="sxs-lookup"><span data-stu-id="bda4f-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="bda4f-106">*Biblioteka klas składników Razor* może być dołączona z:</span><span class="sxs-lookup"><span data-stu-id="bda4f-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="bda4f-107">Inny projekt w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="bda4f-107">Another project in the solution.</span></span>
* <span data-ttu-id="bda4f-108">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="bda4f-108">A NuGet package.</span></span>
* <span data-ttu-id="bda4f-109">Przywoływana Biblioteka platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="bda4f-109">A referenced .NET library.</span></span>

<span data-ttu-id="bda4f-110">Podobnie jak składniki są zwykłymi typami .NET, składniki udostępniane przez RCL są normalnymi zestawami platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="bda4f-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="bda4f-111">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="bda4f-111">Create an RCL</span></span>

<span data-ttu-id="bda4f-112">Postępuj zgodnie ze wskazówkami w artykule <xref:blazor/get-started>, aby skonfigurować środowisko dla Blazor.</span><span class="sxs-lookup"><span data-stu-id="bda4f-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bda4f-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bda4f-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="bda4f-114">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="bda4f-114">Create a new project.</span></span>
1. <span data-ttu-id="bda4f-115">Wybierz **bibliotekę klas Razor**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="bda4f-116">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-116">Select **Next**.</span></span>
1. <span data-ttu-id="bda4f-117">W oknie dialogowym **Tworzenie nowej biblioteki klas Razor** wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="bda4f-118">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="bda4f-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="bda4f-119">W przykładach w tym temacie użyto nazwy projektu `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="bda4f-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="bda4f-120">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-120">Select **Create**.</span></span>
1. <span data-ttu-id="bda4f-121">Dodaj RCL do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="bda4f-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="bda4f-122">Kliknij prawym przyciskiem myszy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="bda4f-122">Right-click the solution.</span></span> <span data-ttu-id="bda4f-123">Wybierz pozycję **dodaj** > **istniejący projekt**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="bda4f-124">Przejdź do pliku projektu RCL.</span><span class="sxs-lookup"><span data-stu-id="bda4f-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="bda4f-125">Wybierz plik projektu RCL ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="bda4f-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="bda4f-126">Dodaj odwołanie RCL z aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bda4f-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="bda4f-127">Kliknij prawym przyciskiem myszy projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bda4f-127">Right-click the app project.</span></span> <span data-ttu-id="bda4f-128">Wybierz pozycję **dodaj** > **odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="bda4f-129">Wybierz projekt RCL.</span><span class="sxs-lookup"><span data-stu-id="bda4f-129">Select the RCL project.</span></span> <span data-ttu-id="bda4f-130">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bda4f-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bda4f-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="bda4f-132">Użyj szablonu **biblioteki klas Razor** (`razorclasslib`) z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="bda4f-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="bda4f-133">W poniższym przykładzie jest tworzony RCL o nazwie `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="bda4f-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="bda4f-134">Folder, który zawiera `MyComponentLib1` jest tworzony automatycznie podczas wykonywania polecenia:</span><span class="sxs-lookup"><span data-stu-id="bda4f-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="bda4f-135">Aby dodać bibliotekę do istniejącego projektu, użyj polecenia [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="bda4f-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="bda4f-136">W poniższym przykładzie RCL jest dodawany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bda4f-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="bda4f-137">Wykonaj następujące polecenie z folderu projektu aplikacji z ścieżką do biblioteki:</span><span class="sxs-lookup"><span data-stu-id="bda4f-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="bda4f-138">Korzystanie ze składnika biblioteki</span><span class="sxs-lookup"><span data-stu-id="bda4f-138">Consume a library component</span></span>

<span data-ttu-id="bda4f-139">Aby można było korzystać ze składników zdefiniowanych w bibliotece w innym projekcie, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="bda4f-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="bda4f-140">Użyj pełnej nazwy typu z przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="bda4f-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="bda4f-141">Użyj składni [\@Using](xref:mvc/views/razor#using) Razor.</span><span class="sxs-lookup"><span data-stu-id="bda4f-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="bda4f-142">Poszczególne składniki można dodawać według nazwy.</span><span class="sxs-lookup"><span data-stu-id="bda4f-142">Individual components can be added by name.</span></span>

<span data-ttu-id="bda4f-143">W poniższych przykładach `MyComponentLib1` jest biblioteką składników zawierającą składnik `SalesReport`.</span><span class="sxs-lookup"><span data-stu-id="bda4f-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="bda4f-144">Do składnika `SalesReport` można odwoływać się za pomocą jego pełnej nazwy typu z przestrzenią nazw:</span><span class="sxs-lookup"><span data-stu-id="bda4f-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="bda4f-145">Składnik może być również przywoływany, jeśli biblioteka została wprowadzona do zakresu za pomocą dyrektywy `@using`:</span><span class="sxs-lookup"><span data-stu-id="bda4f-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="bda4f-146">Uwzględnij dyrektywę `@using MyComponentLib1` w pliku *_Import. Razor* najwyższego poziomu, aby udostępnić składniki biblioteki dla całego projektu.</span><span class="sxs-lookup"><span data-stu-id="bda4f-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="bda4f-147">Dodaj dyrektywę do pliku *_Import. Razor* na dowolnym poziomie, aby zastosować przestrzeń nazw do pojedynczej strony lub zestawu stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="bda4f-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="bda4f-148">Kompilowanie, pakowanie i dostarczanie do narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="bda4f-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="bda4f-149">Ponieważ biblioteki składników są standardowymi bibliotekami .NET, pakowanie i dostarczanie ich do narzędzia NuGet nie różni się od pakowania i wysyłania żadnej biblioteki do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="bda4f-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="bda4f-150">Pakowanie jest wykonywane przy użyciu polecenia [pakietu dotnet](/dotnet/core/tools/dotnet-pack) w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bda4f-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="bda4f-151">Przekaż pakiet do narzędzia NuGet przy użyciu polecenia [push NuGet w trybie wypychania](/dotnet/core/tools/dotnet-nuget-push) w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="bda4f-151">Upload the package to NuGet using the [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push) command in a command shell.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="bda4f-152">Tworzenie biblioteki klas składników Razor ze statycznymi zasobami</span><span class="sxs-lookup"><span data-stu-id="bda4f-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="bda4f-153">RCL może zawierać statyczne zasoby.</span><span class="sxs-lookup"><span data-stu-id="bda4f-153">An RCL can include static assets.</span></span> <span data-ttu-id="bda4f-154">Zasoby statyczne są dostępne dla każdej aplikacji, która korzysta z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="bda4f-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="bda4f-155">Aby uzyskać więcej informacji, zobacz <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="bda4f-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bda4f-156">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bda4f-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
