---
title: Zarządzanie pakietami po stronie klienta za pomocą narzędzi Bower w programie ASP.NET Core
author: rick-anderson
description: Zarządzanie pakietami po stronie klienta za pomocą narzędzi Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 8606c21596a5d9d6ada9c60b55b2f54da21c601b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902722"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="d42af-103">Zarządzanie pakietami po stronie klienta za pomocą narzędzi Bower w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d42af-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="d42af-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="d42af-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d42af-105">Jednocześnie jest Bower, zaleca się jego maintainers przy użyciu innego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="d42af-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="d42af-106">[Menedżer biblioteki](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan w skrócie) to narzędzie programu Visual Studio do pozyskiwania nowych biblioteki po stronie klienta (Visual Studio, należy zachować 15,8 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="d42af-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side library acquisition tool (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="d42af-107">Aby uzyskać więcej informacji, zobacz <xref:client-side/libman/index>.</span><span class="sxs-lookup"><span data-stu-id="d42af-107">For more information, see <xref:client-side/libman/index>.</span></span> <span data-ttu-id="d42af-108">Bower jest świadczona w programie Visual Studio w wersji 15.5.</span><span class="sxs-lookup"><span data-stu-id="d42af-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="d42af-109">Yarn z Webpack jest jeden popularną alternatywę dla którego [instrukcjach migracji](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) są dostępne.</span><span class="sxs-lookup"><span data-stu-id="d42af-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="d42af-110">[Program bower](https://bower.io/) wywołuje sam siebie "Menedżer pakietów dla sieci web".</span><span class="sxs-lookup"><span data-stu-id="d42af-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="d42af-111">W ramach ekosystemu .NET umieszcza void pozostawiony przez brakiem NuGet, aby dostarczać plików zawartości statycznej.</span><span class="sxs-lookup"><span data-stu-id="d42af-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="d42af-112">Dla projektów ASP.NET Core, te pliki statyczne są wbudowane w bibliotek po stronie klienta, takich jak [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="d42af-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="d42af-113">Dla bibliotek .NET, możesz nadal używać [NuGet](https://www.nuget.org/) Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d42af-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="d42af-114">Proces kompilacji nowe projekty utworzone za pomocą szablonów projektu ASP.NET Core, skonfiguruj po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d42af-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="d42af-115">[jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/) są zainstalowane, i Bower jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="d42af-115">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="d42af-116">Pakiety po stronie klienta są wymienione w *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d42af-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="d42af-117">Szablony projektów programu ASP.NET Core konfiguruje *bower.json* przy użyciu jQuery i dotyczącą weryfikacji jQuery, Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="d42af-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="d42af-118">W tym samouczku dodamy obsługę [Font Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="d42af-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="d42af-119">Można zainstalować za pomocą pakietów bower **Zarządzanie pakietami programu Bower** interfejsu użytkownika lub ręcznie w *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d42af-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="d42af-120">Instalację za pomocą Zarządzanie pakietami programu Bower interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="d42af-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="d42af-121">Utwórz nową aplikację sieci Web platformy ASP.NET Core za pomocą **aplikacja sieci Web programu ASP.NET Core (.NET Core)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d42af-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="d42af-122">Wybierz **aplikacji sieci Web** i **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="d42af-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="d42af-123">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie pakietami programu Bower** (również w menu głównym **projektu** > **Zarządzanie pakietami programu Bower**).</span><span class="sxs-lookup"><span data-stu-id="d42af-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="d42af-124">W **Bower: \<Nazwa projektu\>**  , kliknij kartę "Przeglądaj", a następnie Przefiltruj listę pakietów, wprowadzając `font-awesome` w polu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="d42af-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![Zarządzaj pakietami programu bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="d42af-126">Upewnij się, że "zapisać zmiany w *bower.json*" jest zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="d42af-126">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="d42af-127">Wybierz wersję z listy rozwijanej, a następnie kliknij przycisk **zainstalować** przycisku.</span><span class="sxs-lookup"><span data-stu-id="d42af-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="d42af-128">**Dane wyjściowe** okno zawiera szczegółowe informacje dotyczące instalacji.</span><span class="sxs-lookup"><span data-stu-id="d42af-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="d42af-129">Instalacja ręczna, w pliku bower.json</span><span class="sxs-lookup"><span data-stu-id="d42af-129">Manual installation in bower.json</span></span>

<span data-ttu-id="d42af-130">Otwórz *bower.json* pliku i Dodaj "font-awesome" do zależności.</span><span class="sxs-lookup"><span data-stu-id="d42af-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="d42af-131">Funkcja IntelliSense wyświetla dostępne pakiety.</span><span class="sxs-lookup"><span data-stu-id="d42af-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="d42af-132">Po wybraniu pakietu dostępnych wersji są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="d42af-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="d42af-133">Poniższe obrazy są starsze i nie będzie zgodne, zostanie wyświetlony.</span><span class="sxs-lookup"><span data-stu-id="d42af-133">The images below are older and won't match what you see.</span></span>

![IntelliSense Eksplorator pakietów bower](bower/_static/add-package.png)

![wersja programu bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="d42af-136">Bower używa [wersji semantycznej](http://semver.org/) do organizowania zależności.</span><span class="sxs-lookup"><span data-stu-id="d42af-136">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="d42af-137">Semantyczne przechowywania wersji, znany także jako SemVer identyfikuje pakiety ze schematu numerowania \<główna >.\< pomocnicza >. \<poprawki >.</span><span class="sxs-lookup"><span data-stu-id="d42af-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="d42af-138">IntelliSense ułatwia semantycznego versioning przedstawiający kilka typowe opcje.</span><span class="sxs-lookup"><span data-stu-id="d42af-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="d42af-139">Pierwszy element na liście funkcji IntelliSense (4.6.3 w powyższym przykładzie), jest uznawana za stabilną najnowszą wersję pakietu.</span><span class="sxs-lookup"><span data-stu-id="d42af-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="d42af-140">Symbolu daszka (^) odpowiada najbardziej aktualną wersję główną i tyldy (~) dopasowuje najbardziej aktualną wersję pomocniczą.</span><span class="sxs-lookup"><span data-stu-id="d42af-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="d42af-141">Zapisz *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d42af-141">Save the *bower.json* file.</span></span> <span data-ttu-id="d42af-142">Program Visual Studio obserwuje *bower.json* zmiany w pliku.</span><span class="sxs-lookup"><span data-stu-id="d42af-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="d42af-143">Po zapisaniu, *Zainstaluj program bower* polecenie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="d42af-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="d42af-144">Zobacz okno dane wyjściowe **Bower/npm** widok pełne polecenie wykonane.</span><span class="sxs-lookup"><span data-stu-id="d42af-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="d42af-145">Otwórz *.bowerrc* plik *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="d42af-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="d42af-146">`directory` Właściwość jest ustawiona na *wwwroot/lib* która wskazuje lokalizację Bower zainstaluje zasobów pakietu.</span><span class="sxs-lookup"><span data-stu-id="d42af-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="d42af-147">Aby znaleźć i wyświetlić pakiet font awesome, można użyć pola wyszukiwania w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="d42af-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="d42af-148">Otwórz *Views\Shared\_Layout.cshtml* pliku i Dodaj font awesome pliku CSS w środowisku [Pomocnik tagu](xref:mvc/views/tag-helpers/intro) dla `Development`.</span><span class="sxs-lookup"><span data-stu-id="d42af-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="d42af-149">Za pomocą Eksploratora rozwiązań przeciągnij i upuść *awesome.css czcionki* wewnątrz `<environment names="Development">` elementu.</span><span class="sxs-lookup"><span data-stu-id="d42af-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="d42af-150">W aplikacji produkcyjnych należy dodać *awesome.min.css czcionki* do Pomocnik tagu środowiska dla `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="d42af-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="d42af-151">Zastąp zawartość *Views\Home\About.cshtml* Razor pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d42af-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="d42af-152">Uruchom aplikację i przejdź do widoku informacje, aby Sprawdź, czy działa font awesome pakietu.</span><span class="sxs-lookup"><span data-stu-id="d42af-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="d42af-153">Eksplorowanie procesu kompilacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="d42af-153">Exploring the client-side build process</span></span>

<span data-ttu-id="d42af-154">Większość szablonów projektów ASP.NET Core są już skonfigurowane do użycia rozwiązania Bower.</span><span class="sxs-lookup"><span data-stu-id="d42af-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="d42af-155">Ten przewodnik dalej rozpoczyna się od pustego projektu platformy ASP.NET Core i dodaje każdy element ręcznie, dzięki czemu można uzyskać pewne pojęcie dotyczące sposobu używania rozwiązania Bower w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d42af-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="d42af-156">Możesz zobaczyć, co się dzieje z struktury projektu i środowiska uruchomieniowego, zgodnie z każdej zmiany konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d42af-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="d42af-157">Ogólne kroki procesu kompilacji po stronie klienta za pomocą rozwiązania Bower są:</span><span class="sxs-lookup"><span data-stu-id="d42af-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="d42af-158">Definiowanie pakietów używanych w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d42af-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="d42af-159">Pakiety odwołania ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="d42af-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="d42af-160">Definiowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="d42af-160">Define packages</span></span>

<span data-ttu-id="d42af-161">Po wyświetleniu listy pakietów w *bower.json* plików, programu Visual Studio pobierze je.</span><span class="sxs-lookup"><span data-stu-id="d42af-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="d42af-162">W poniższym przykładzie użyto narzędzia Bower do załadowania, jQuery i Bootstrap do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="d42af-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="d42af-163">Utwórz nową aplikację sieci Web platformy ASP.NET Core za pomocą **aplikacja sieci Web programu ASP.NET Core (.NET Core)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="d42af-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="d42af-164">Wybierz **pusty** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d42af-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="d42af-165">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy Projekt > **Dodaj nowy element** i wybierz **plik konfiguracji programu Bower**.</span><span class="sxs-lookup"><span data-stu-id="d42af-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="d42af-166">Uwaga: Element *.bowerrc* również zostanie dodany plik.</span><span class="sxs-lookup"><span data-stu-id="d42af-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="d42af-167">Otwórz *bower.json*, Dodaj jquery i uruchamiania na `dependencies` sekcji.</span><span class="sxs-lookup"><span data-stu-id="d42af-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="d42af-168">Wartość wynikowa *bower.json* plik będzie wyglądać podobnie jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="d42af-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="d42af-169">Wersje zmieni się wraz z upływem czasu i może nie odpowiadać na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="d42af-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="d42af-170">Zapisz *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d42af-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="d42af-171">Sprawdź projekt obejmuje *bootstrap* i *jQuery* katalogi *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="d42af-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="d42af-172">Zastosowań bower *.bowerrc* plik, aby zainstalować zasoby w *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="d42af-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="d42af-173">Uwaga: Interfejsu użytkownika "Zarządzaj pakietami Bower" jest alternatywą dla ręcznego plik edycji.</span><span class="sxs-lookup"><span data-stu-id="d42af-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="d42af-174">Włącz pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="d42af-174">Enable static files</span></span>

* <span data-ttu-id="d42af-175">Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="d42af-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="d42af-176">Włącz pliki statyczne z [oprogramowanie pośredniczące plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="d42af-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="d42af-177">Dodaj wywołanie do [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) do `Configure` metody `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d42af-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="d42af-178">Odwołanie do pakietów</span><span class="sxs-lookup"><span data-stu-id="d42af-178">Reference packages</span></span>

<span data-ttu-id="d42af-179">W tej sekcji utworzysz stronę HTML, aby sprawdzić, czy będzie miał dostęp do wdrożonych pakietów.</span><span class="sxs-lookup"><span data-stu-id="d42af-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="d42af-180">Dodaj nową stronę HTML o nazwie *Index.html* do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="d42af-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="d42af-181">Uwaga: Należy dodać plik HTML *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="d42af-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="d42af-182">Domyślnie nie może zostać wyświetlona zawartość statyczną, poza *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="d42af-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="d42af-183">Zobacz [pliki statyczne](xref:fundamentals/static-files) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d42af-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="d42af-184">Zastąp zawartość *Index.html* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d42af-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="d42af-185">Uruchom aplikację i przejdź do `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="d42af-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="d42af-186">Alternatywnie za pomocą *Index.html* otwarte, naciśnij klawisz `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="d42af-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="d42af-187">Sprawdź stosowanie stylów jumbotron, kodu jQuery odpowiada po kliknięciu przycisku i ładowania przycisku zmienia się stan.</span><span class="sxs-lookup"><span data-stu-id="d42af-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![Styl jumbotron stosowany](bower/_static/jumbotron.png)
