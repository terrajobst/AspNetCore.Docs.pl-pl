---
title: Platforma ASP.NET Core Razor składniki klasy biblioteki
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/class-libraries
ms.openlocfilehash: 40dc029b35e0997e526fdfc02c275da5a84450b9
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152882"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="f9f52-103">Platforma ASP.NET Core Razor składniki klasy biblioteki</span><span class="sxs-lookup"><span data-stu-id="f9f52-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="f9f52-104">Przez [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="f9f52-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="f9f52-105">Składniki mogą być udostępniane w bibliotekach klas Razor w projektach.</span><span class="sxs-lookup"><span data-stu-id="f9f52-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="f9f52-106">Składniki można uwzględnić od:</span><span class="sxs-lookup"><span data-stu-id="f9f52-106">Components can be included from:</span></span>

* <span data-ttu-id="f9f52-107">Innego projektu w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="f9f52-107">Another project in the solution.</span></span>
* <span data-ttu-id="f9f52-108">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9f52-108">A NuGet package.</span></span>
* <span data-ttu-id="f9f52-109">Odwołania biblioteki .NET.</span><span class="sxs-lookup"><span data-stu-id="f9f52-109">A referenced .NET library.</span></span>

<span data-ttu-id="f9f52-110">Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez biblioteki klas Razor są normalne zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f9f52-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="f9f52-111">Tworzenie biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="f9f52-111">Create a Razor class library</span></span>

<span data-ttu-id="f9f52-112">Postępuj zgodnie ze wskazówkami w <xref:blazor/get-started> artykuł, aby skonfigurować środowisko dla Blazor.</span><span class="sxs-lookup"><span data-stu-id="f9f52-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f9f52-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9f52-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f9f52-114">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="f9f52-114">Create a new project.</span></span>
1. <span data-ttu-id="f9f52-115">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f9f52-116">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-116">Select **Next**.</span></span>
1. <span data-ttu-id="f9f52-117">Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="f9f52-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="f9f52-118">Przykłady w tym temacie użyje nazwy projektu `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="f9f52-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="f9f52-119">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-119">Select **Create**.</span></span>
1. <span data-ttu-id="f9f52-120">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="f9f52-120">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="f9f52-121">Wybierz **biblioteki klas Razor** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f9f52-121">Select the **Razor Class Library** template.</span></span> <span data-ttu-id="f9f52-122">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-122">Select **Create**.</span></span>
1. <span data-ttu-id="f9f52-123">Dodawanie biblioteki klas Razor do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="f9f52-123">Add the Razor class library to a solution:</span></span>
   1. <span data-ttu-id="f9f52-124">Kliknij prawym przyciskiem myszy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="f9f52-124">Right-click the solution.</span></span> <span data-ttu-id="f9f52-125">Wybierz **Dodaj** > **istniejący projekt**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-125">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="f9f52-126">Przejdź do pliku projektu biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="f9f52-126">Navigate to the Razor class library's project file.</span></span>
   1. <span data-ttu-id="f9f52-127">Wybierz plik projektu biblioteki klas Razor ( *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f9f52-127">Select the Razor class library's project file (*.csproj*).</span></span>
1. <span data-ttu-id="f9f52-128">Dodaj odwołanie do biblioteki klas Razor z poziomu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f9f52-128">Add a reference the Razor class library from the app:</span></span>
   1. <span data-ttu-id="f9f52-129">Kliknij prawym przyciskiem myszy projekt aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9f52-129">Right-click the app project.</span></span> <span data-ttu-id="f9f52-130">Wybierz **Dodaj** > **odwołania**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-130">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="f9f52-131">Wybierz projekt biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="f9f52-131">Select the Razor class library project.</span></span> <span data-ttu-id="f9f52-132">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9f52-132">Select **OK**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="f9f52-133">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="f9f52-133">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

1. <span data-ttu-id="f9f52-134">Użyj biblioteki klas Razor (`razorclasslib`) szablon [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="f9f52-134">Use the Razor class library (`razorclasslib`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="f9f52-135">W poniższym przykładzie jest tworzony o nazwie biblioteki klas Razor `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="f9f52-135">In the following example, a Razor class library is created named `MyComponentLib1`.</span></span> <span data-ttu-id="f9f52-136">Folder, który przechowuje `MyComponentLib1` jest tworzona automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="f9f52-136">The folder that holds `MyComponentLib1` is created automatically when the command is executed.</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="f9f52-137">Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference) polecenia powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="f9f52-137">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command from a command shell.</span></span> <span data-ttu-id="f9f52-138">W poniższym przykładzie biblioteki klas Razor jest dodawany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9f52-138">In the following example, the Razor class library is added to the app.</span></span> <span data-ttu-id="f9f52-139">Wykonaj następujące polecenie z folderu projektu aplikacji ze ścieżką do biblioteki:</span><span class="sxs-lookup"><span data-stu-id="f9f52-139">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

<span data-ttu-id="f9f52-140">Dodaj pliki składników Razor ( *.razor*) do biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="f9f52-140">Add Razor component files (*.razor*) to the Razor class library.</span></span>

## <a name="razor-class-libraries-not-supported-for-client-side-apps"></a><span data-ttu-id="f9f52-141">Biblioteki klas razor nie jest obsługiwane dla aplikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="f9f52-141">Razor class libraries not supported for client-side apps</span></span>

<span data-ttu-id="f9f52-142">W programie ASP.NET Core 3.0 (wersja zapoznawcza) biblioteki klas Razor nie są zgodne z aplikacji po stronie klienta Blazor.</span><span class="sxs-lookup"><span data-stu-id="f9f52-142">In ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span>

<span data-ttu-id="f9f52-143">W przypadku aplikacji po stronie klienta Blazor używać biblioteki składników Blazor utworzone przez `blazorlib` szablonu z powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="f9f52-143">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template from a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="f9f52-144">Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="f9f52-144">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="f9f52-145">W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego ( *.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów.</span><span class="sxs-lookup"><span data-stu-id="f9f52-145">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="f9f52-146">Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="f9f52-146">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="static-assets-not-supported-for-server-side-apps"></a><span data-ttu-id="f9f52-147">Statyczne elementy zawartości, które nie są obsługiwane w przypadku aplikacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="f9f52-147">Static assets not supported for server-side apps</span></span>

<span data-ttu-id="f9f52-148">W programie ASP.NET Core 3.0 (wersja zapoznawcza), Blazor po stronie serwera aplikacji nie może zużywać zasoby statyczne z obu biblioteki klas Razor (`razorclasslib`) lub bibliotekę Blazor (`blazorlib`).</span><span class="sxs-lookup"><span data-stu-id="f9f52-148">In ASP.NET Core 3.0 Preview, Blazor server-side apps can't consume static assets from either a Razor class library (`razorclasslib`) or a Blazor library (`blazorlib`).</span></span>

<span data-ttu-id="f9f52-149">Jako rozwiązanie tymczasowe, możesz spróbować [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/).</span><span class="sxs-lookup"><span data-stu-id="f9f52-149">As a temporary workaround, you can try [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/).</span></span>

> [!NOTE]
> <span data-ttu-id="f9f52-150">[BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f9f52-150">[BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/) isn't maintained or supported by Microsoft.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="f9f52-151">Używanie składnik biblioteki</span><span class="sxs-lookup"><span data-stu-id="f9f52-151">Consume a library component</span></span>

<span data-ttu-id="f9f52-152">Aby można było używać składników zdefiniowane w bibliotece w innym projekcie, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f9f52-152">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="f9f52-153">Pełna nazwa typu za pomocą przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f9f52-153">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="f9f52-154">Użyj firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="f9f52-154">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="f9f52-155">Poszczególne składniki można dodać według nazwy.</span><span class="sxs-lookup"><span data-stu-id="f9f52-155">Individual components can be added by name.</span></span>

<span data-ttu-id="f9f52-156">W poniższych przykładach `MyComponentLib1` to biblioteka składnika zawierająca raport sprzedaży (`SalesReport`) składnika.</span><span class="sxs-lookup"><span data-stu-id="f9f52-156">In the following examples, `MyComponentLib1` is a component library containing a Sales Report (`SalesReport`) component.</span></span>

<span data-ttu-id="f9f52-157">Składnik raport sprzedaży mogą być przywoływane z przestrzenią nazw przy użyciu jego pełna nazwa typu:</span><span class="sxs-lookup"><span data-stu-id="f9f52-157">The Sales Report component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="f9f52-158">Składnik również mogą być przywoływane, jeśli biblioteka zostanie przełączony w tryb do zakresu za pomocą `@using` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="f9f52-158">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="f9f52-159">Obejmują `@using MyComponentLib1` dyrektywy w najwyższego poziomu *_Import.razor* pliku udostępniania biblioteki składników dla całego projektu.</span><span class="sxs-lookup"><span data-stu-id="f9f52-159">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="f9f52-160">Dodaj dyrektywę do *_Import.razor* plik na dowolnym poziomie, aby zastosować przestrzeń nazw pojedynczej strony lub zbiór stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="f9f52-160">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="f9f52-161">Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="f9f52-161">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="f9f52-162">Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9f52-162">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="f9f52-163">Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="f9f52-163">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command from a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="f9f52-164">Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia powłoki poleceń:</span><span class="sxs-lookup"><span data-stu-id="f9f52-164">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command from a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="f9f52-165">Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9f52-165">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="f9f52-166">Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.</span><span class="sxs-lookup"><span data-stu-id="f9f52-166">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span> <span data-ttu-id="f9f52-167">Należy pamiętać, że [statycznych zasobów nie są obsługiwane w przypadku aplikacji po stronie serwera](#static-assets-not-supported-for-server-side-apps), w tym przypadku biblioteki Blazor (`blazorlib`) odwołuje się do aplikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="f9f52-167">Note that [static assets aren't supported for server-side apps](#static-assets-not-supported-for-server-side-apps), including when a Blazor library (`blazorlib`) is referenced by a server-side app.</span></span>

## <a name="create-a-razor-class-library-with-static-assets"></a><span data-ttu-id="f9f52-168">Tworzenie biblioteki klas Razor przy użyciu statycznych zasobów</span><span class="sxs-lookup"><span data-stu-id="f9f52-168">Create a Razor class library with static assets</span></span>

<span data-ttu-id="f9f52-169">Biblioteki klas razor (RCL) często wymagają pomocnika statycznych zasobów, które mogą być przywoływane przez aplikację odbierającą RCL.</span><span class="sxs-lookup"><span data-stu-id="f9f52-169">Razor class libraries (RCL) frequently require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="f9f52-170">Platforma ASP.NET Core umożliwia tworzenie RCLs, które obejmują zasoby statyczne, które są dostępne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9f52-170">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="f9f52-171">Aby uwzględnić zasoby pomocnika jako część biblioteki klas Razor, utworzyć *wwwroot* folder w ułatwieniach i zawierać wszystkie wymagane pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="f9f52-171">To include companion assets as part of a Razor class library, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="f9f52-172">Podczas pakowania, biblioteki klas Razor, wszystkie pomocnika zasobów w *wwwroot* folderów są automatycznie uwzględnione w pakiecie i są dostępne dla aplikacji, które odwołuje się do pakietu.</span><span class="sxs-lookup"><span data-stu-id="f9f52-172">When packing a Razor class library, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-razor-class-library"></a><span data-ttu-id="f9f52-173">Korzystanie z zawartości z przywoływanych biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="f9f52-173">Consume content from a referenced Razor class library</span></span>

<span data-ttu-id="f9f52-174">Pliki zawarte w *wwwroot* folder biblioteki klas Razor są widoczne dla aplikacji w ramach prefiksu `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="f9f52-174">The files included in the *wwwroot* folder of the Razor class library are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="f9f52-175">Aplikacja odbierająca komunikaty odwołuje się do tych zasobów za pomocą `<script>`, `<style>`, `<img>`, a inne tagi HTML.</span><span class="sxs-lookup"><span data-stu-id="f9f52-175">The consuming app references these assets via `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="f9f52-176">Przepływ rozwoju wielu projektów</span><span class="sxs-lookup"><span data-stu-id="f9f52-176">Multi-project development flow</span></span>

<span data-ttu-id="f9f52-177">Po uruchomieniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f9f52-177">When the app runs:</span></span>

* <span data-ttu-id="f9f52-178">Zasoby pozostają w ich oryginalnych folderów.</span><span class="sxs-lookup"><span data-stu-id="f9f52-178">The assets stay in their original folders.</span></span>
* <span data-ttu-id="f9f52-179">Wszelkie zmiany w bibliotece klas *wwwroot* folder jest widoczny w aplikacji bez ponownie skompilować.</span><span class="sxs-lookup"><span data-stu-id="f9f52-179">Any change within the class library *wwwroot* folder is reflected in the app without rebuilding.</span></span>

<span data-ttu-id="f9f52-180">W czasie kompilacji manifest jest generowany ze wszystkich lokalizacji zasobów statyczną sieci web.</span><span class="sxs-lookup"><span data-stu-id="f9f52-180">At build time, a manifest is produced with all the static web asset locations.</span></span> <span data-ttu-id="f9f52-181">Manifest jest do odczytu w czasie wykonywania i umożliwia aplikacji korzystanie z zasobów w projektach odwołania i pakietów.</span><span class="sxs-lookup"><span data-stu-id="f9f52-181">The manifest is read at runtime and allows the app to consume the assets from referenced projects and packages.</span></span>

### <a name="publish"></a><span data-ttu-id="f9f52-182">Publikowanie</span><span class="sxs-lookup"><span data-stu-id="f9f52-182">Publish</span></span>

<span data-ttu-id="f9f52-183">Po opublikowaniu aplikacji zasoby pomocnika z wszystkie przywoływane projekty i pakiety są kopiowane do *wwwroot* folderu opublikowanej aplikacji, w obszarze `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="f9f52-183">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>
