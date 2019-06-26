---
title: Platforma ASP.NET Core Razor składniki klasy biblioteki
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/24/2019
uid: blazor/class-libraries
ms.openlocfilehash: 8676e0fd660b7d281c80d06d24d5593c2df6348b
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394620"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="3b6db-103">Platforma ASP.NET Core Razor składniki klasy biblioteki</span><span class="sxs-lookup"><span data-stu-id="3b6db-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="3b6db-104">Przez [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="3b6db-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="3b6db-105">Składniki mogą być współużytkowane w [biblioteki klas Razor (RCL)](xref:razor-pages/ui-class) w projektach.</span><span class="sxs-lookup"><span data-stu-id="3b6db-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="3b6db-106">A *Biblioteka klas składników Razor* można uwzględnić od:</span><span class="sxs-lookup"><span data-stu-id="3b6db-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="3b6db-107">Innego projektu w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="3b6db-107">Another project in the solution.</span></span>
* <span data-ttu-id="3b6db-108">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b6db-108">A NuGet package.</span></span>
* <span data-ttu-id="3b6db-109">Odwołania biblioteki .NET.</span><span class="sxs-lookup"><span data-stu-id="3b6db-109">A referenced .NET library.</span></span>

<span data-ttu-id="3b6db-110">Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez RCL są normalne zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3b6db-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="3b6db-111">Utwórz RCL</span><span class="sxs-lookup"><span data-stu-id="3b6db-111">Create an RCL</span></span>

<span data-ttu-id="3b6db-112">Postępuj zgodnie ze wskazówkami w <xref:blazor/get-started> artykuł, aby skonfigurować środowisko dla Blazor.</span><span class="sxs-lookup"><span data-stu-id="3b6db-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3b6db-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b6db-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="3b6db-114">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="3b6db-114">Create a new project.</span></span>
1. <span data-ttu-id="3b6db-115">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="3b6db-116">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-116">Select **Next**.</span></span>
1. <span data-ttu-id="3b6db-117">Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="3b6db-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="3b6db-118">Przykłady w tym temacie użyje nazwy projektu `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="3b6db-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="3b6db-119">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-119">Select **Create**.</span></span>
1. <span data-ttu-id="3b6db-120">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="3b6db-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="3b6db-121">Wybierz **biblioteki klas Razor** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3b6db-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="3b6db-122">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-122">Select **Create**.</span></span>
1. <span data-ttu-id="3b6db-123">Dodawanie RCL do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="3b6db-123">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="3b6db-124">Kliknij prawym przyciskiem myszy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="3b6db-124">Right-click the solution.</span></span> <span data-ttu-id="3b6db-125">Wybierz **Dodaj** > **istniejący projekt**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="3b6db-126">Przejdź do pliku projektu RCL.</span><span class="sxs-lookup"><span data-stu-id="3b6db-126">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="3b6db-127">Wybierz plik projektu RCL ( *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="3b6db-127">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="3b6db-128">Dodaj odwołanie RCL z poziomu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3b6db-128">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="3b6db-129">Kliknij prawym przyciskiem myszy projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b6db-129">Right-click the app project.</span></span> <span data-ttu-id="3b6db-130">Wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="3b6db-131">Wybierz projekt RCL.</span><span class="sxs-lookup"><span data-stu-id="3b6db-131">Select the RCL project.</span></span> <span data-ttu-id="3b6db-132">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b6db-132">Select **OK**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="3b6db-133">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="3b6db-133">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

1. <span data-ttu-id="3b6db-134">Szablon biblioteki klas Razor (`razorclasslib`) za pomocą [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="3b6db-134">Use the Razor Class Library template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="3b6db-135">W poniższym przykładzie jest tworzony o nazwie RCL `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="3b6db-135">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="3b6db-136">Folder, który przechowuje `MyComponentLib1` jest tworzona automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="3b6db-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed.</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="3b6db-137">Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference) polecenia powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="3b6db-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command from a command shell.</span></span> <span data-ttu-id="3b6db-138">W poniższym przykładzie RCL zostanie dodany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b6db-138">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="3b6db-139">Wykonaj następujące polecenie z folderu projektu aplikacji ze ścieżką do biblioteki:</span><span class="sxs-lookup"><span data-stu-id="3b6db-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

<span data-ttu-id="3b6db-140">Dodaj pliki składników Razor ( *.razor*) do RCL.</span><span class="sxs-lookup"><span data-stu-id="3b6db-140">Add Razor component files (*.razor*) to the RCL.</span></span>

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="3b6db-141">RCLs nie jest obsługiwane dla aplikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="3b6db-141">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="3b6db-142">W programie ASP.NET Core 3.0 (wersja zapoznawcza) biblioteki klas Razor nie są zgodne z aplikacji po stronie klienta Blazor.</span><span class="sxs-lookup"><span data-stu-id="3b6db-142">In ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span>

<span data-ttu-id="3b6db-143">W przypadku aplikacji po stronie klienta Blazor używać biblioteki składników Blazor utworzone przez `blazorlib` szablonu z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3b6db-143">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template from a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="3b6db-144">Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="3b6db-144">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="3b6db-145">W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego ( *.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów.</span><span class="sxs-lookup"><span data-stu-id="3b6db-145">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="3b6db-146">Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="3b6db-146">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="3b6db-147">Używanie składnik biblioteki</span><span class="sxs-lookup"><span data-stu-id="3b6db-147">Consume a library component</span></span>

<span data-ttu-id="3b6db-148">Aby można było używać składników zdefiniowane w bibliotece w innym projekcie, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3b6db-148">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="3b6db-149">Pełna nazwa typu za pomocą przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="3b6db-149">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="3b6db-150">Użyj firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="3b6db-150">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="3b6db-151">Poszczególne składniki można dodać według nazwy.</span><span class="sxs-lookup"><span data-stu-id="3b6db-151">Individual components can be added by name.</span></span>

<span data-ttu-id="3b6db-152">W poniższych przykładach `MyComponentLib1` to biblioteka składnika zawierająca raport sprzedaży (`SalesReport`) składnika.</span><span class="sxs-lookup"><span data-stu-id="3b6db-152">In the following examples, `MyComponentLib1` is a component library containing a Sales Report (`SalesReport`) component.</span></span>

<span data-ttu-id="3b6db-153">Składnik raport sprzedaży mogą być przywoływane z przestrzenią nazw przy użyciu jego pełna nazwa typu:</span><span class="sxs-lookup"><span data-stu-id="3b6db-153">The Sales Report component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="3b6db-154">Składnik również mogą być przywoływane, jeśli biblioteka zostanie przełączony w tryb do zakresu za pomocą `@using` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="3b6db-154">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="3b6db-155">Obejmują `@using MyComponentLib1` dyrektywy w najwyższego poziomu *_Import.razor* pliku udostępniania biblioteki składników dla całego projektu.</span><span class="sxs-lookup"><span data-stu-id="3b6db-155">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="3b6db-156">Dodaj dyrektywę do *_Import.razor* plik na dowolnym poziomie, aby zastosować przestrzeń nazw pojedynczej strony lub zbiór stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="3b6db-156">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="3b6db-157">Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="3b6db-157">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="3b6db-158">Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b6db-158">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="3b6db-159">Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3b6db-159">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command from a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="3b6db-160">Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="3b6db-160">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command from a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="3b6db-161">Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b6db-161">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="3b6db-162">Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.</span><span class="sxs-lookup"><span data-stu-id="3b6db-162">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="3b6db-163">Tworzenie biblioteki klas Razor składników za pomocą statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="3b6db-163">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="3b6db-164">RCL mogą obejmować zasoby statyczne.</span><span class="sxs-lookup"><span data-stu-id="3b6db-164">An RCL can include static assets.</span></span> <span data-ttu-id="3b6db-165">Statyczne elementy zawartości są dostępne dla żadnej aplikacji, która korzysta z biblioteki.</span><span class="sxs-lookup"><span data-stu-id="3b6db-165">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="3b6db-166">Aby uzyskać więcej informacji, zobacz <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="3b6db-166">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b6db-167">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3b6db-167">Additional resources</span></span>

* <xref:razor-pages/ui-class>
