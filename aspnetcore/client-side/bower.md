---
title: Zarządzaj pakietami po stronie klienta z Bower w ASP.NET Core
author: rick-anderson
description: Zarządzanie pakietami po stronie klienta z Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
uid: client-side/bower
ms.openlocfilehash: 23f3dcd06f012f3cf8d9509280b91c4bd1dc84e1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272520"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="0deb5-103">Zarządzaj pakietami po stronie klienta z Bower w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0deb5-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="0deb5-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="0deb5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="0deb5-105">Jednocześnie jest Bower, jego maintainers zaleca się użycie innego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="0deb5-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="0deb5-106">[Menedżer biblioteki](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan skrócie) to system zarządzania zawartością statycznych po stronie klienta programu Visual Studio — nowy (Visual Studio 15,8 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="0deb5-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side static content management system (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="0deb5-107">Aby uzyskać więcej informacji, zobacz [Library Manager: Menedżer zawartości po stronie klienta dla aplikacji sieci web](https://blogs.msdn.microsoft.com/webdev/2018/04/17/library-manager-client-side-content-manager-for-web-apps/).</span><span class="sxs-lookup"><span data-stu-id="0deb5-107">For more information, see [Library Manager: Client-side content manager for web apps](https://blogs.msdn.microsoft.com/webdev/2018/04/17/library-manager-client-side-content-manager-for-web-apps/).</span></span> <span data-ttu-id="0deb5-108">Bower jest obsługiwana w programie Visual Studio za pomocą wersji 15,5 cala.</span><span class="sxs-lookup"><span data-stu-id="0deb5-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="0deb5-109">Yarn z Webpack jest jeden popularną alternatywę dla którego [instrukcje migracji](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) są dostępne.</span><span class="sxs-lookup"><span data-stu-id="0deb5-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span> 

<span data-ttu-id="0deb5-110">[Bower](https://bower.io/) wywołuje sam siebie "Menedżer pakietów dla sieci web".</span><span class="sxs-lookup"><span data-stu-id="0deb5-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="0deb5-111">W ekosystemie .NET umieszcza void pozostawionego przez brakiem NuGet do dostarczania zawartości plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="0deb5-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="0deb5-112">Dla projektów platformy ASP.NET Core, te pliki statyczne są wbudowane w bibliotekach po stronie klienta, takich jak [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="0deb5-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="0deb5-113">W przypadku bibliotek .NET, możesz nadal używać [NuGet](https://www.nuget.org/) Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="0deb5-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="0deb5-114">Proces kompilacji nowych projektów utworzonych za pomocą szablonów projektu platformy ASP.NET Core — konfiguracja po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0deb5-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="0deb5-115">[jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/) są zainstalowane i Bower jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="0deb5-115">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="0deb5-116">Pakiety po stronie klienta są wyświetlane w *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0deb5-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="0deb5-117">Szablony projektów platformy ASP.NET Core konfiguruje *bower.json* jQuery, weryfikacji jQuery i ładowania początkowego.</span><span class="sxs-lookup"><span data-stu-id="0deb5-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="0deb5-118">W tym samouczku dodamy obsługę [czcionki świetny](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="0deb5-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="0deb5-119">Można je zainstalować pakiety bower **Zarządzaj pakietami Bower** interfejsu użytkownika lub ręcznie w *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0deb5-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="0deb5-120">Instalacja za pomocą pakietów Bower Zarządzaj interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="0deb5-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="0deb5-121">Utwórz nową aplikację sieci Web platformy ASP.NET Core z **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="0deb5-122">Wybierz **aplikacji sieci Web** i **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="0deb5-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="0deb5-123">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzaj pakietami Bower** (również w menu głównym **projektu** > **Zarządzaj pakietami Bower**).</span><span class="sxs-lookup"><span data-stu-id="0deb5-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="0deb5-124">W **Bower: \<Nazwa projektu\>**  , kliknij kartę "Przeglądaj", a następnie przeprowadź filtrowanie listy pakietów, wprowadzając `font-awesome` w polu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="0deb5-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![Zarządzaj pakietami bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="0deb5-126">Upewnij się, że "zapisać zmiany w *bower.json*" jest zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="0deb5-126">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="0deb5-127">Wybierz z listy rozwijanej wersję, a następnie kliknij przycisk **zainstalować** przycisku.</span><span class="sxs-lookup"><span data-stu-id="0deb5-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="0deb5-128">**Dane wyjściowe** okno zawiera szczegółowe informacje dotyczące instalacji.</span><span class="sxs-lookup"><span data-stu-id="0deb5-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="0deb5-129">Ręczna instalacja w pliku bower.json</span><span class="sxs-lookup"><span data-stu-id="0deb5-129">Manual installation in bower.json</span></span>

<span data-ttu-id="0deb5-130">Otwórz *bower.json* pliku, a następnie dodaj "font świetny" do zależności.</span><span class="sxs-lookup"><span data-stu-id="0deb5-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="0deb5-131">IntelliSense zawiera dostępnych pakietów.</span><span class="sxs-lookup"><span data-stu-id="0deb5-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="0deb5-132">Po wybraniu pakietu dostępne wersje są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="0deb5-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="0deb5-133">Poniżej obrazy są starsze i nie będzie zgodne, zostanie wyświetlony.</span><span class="sxs-lookup"><span data-stu-id="0deb5-133">The images below are older and won't match what you see.</span></span>

![IntelliSense bower Eksploratora pakietów](bower/_static/add-package.png)

![Wersja bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="0deb5-136">Bower używa [wersjonowania semantycznego](http://semver.org/) do organizowania zależności.</span><span class="sxs-lookup"><span data-stu-id="0deb5-136">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="0deb5-137">Wersjonowania semantycznego, znanej także jako programu SemVer identyfikuje pakiety ze schematu numerowania \<głównych >.\< drobne >. \<poprawki >.</span><span class="sxs-lookup"><span data-stu-id="0deb5-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="0deb5-138">Przedstawiający kilka typowe opcje IntelliSense upraszcza wersjonowania semantycznego.</span><span class="sxs-lookup"><span data-stu-id="0deb5-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="0deb5-139">Pierwszy element na liście IntelliSense (4.6.3 w powyższym przykładzie) jest uznawany za najnowsza stabilna wersja pakietu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="0deb5-140">Symbol daszek (^) najnowszą wersją główną i tyldy (~) najnowszą wersją pomocniczą.</span><span class="sxs-lookup"><span data-stu-id="0deb5-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="0deb5-141">Zapisz *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0deb5-141">Save the *bower.json* file.</span></span> <span data-ttu-id="0deb5-142">Visual Studio Obserwujący *bower.json* zmiany w pliku.</span><span class="sxs-lookup"><span data-stu-id="0deb5-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="0deb5-143">Przy zapisywaniu *instalacji bower* polecenie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="0deb5-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="0deb5-144">Zobacz okno dane wyjściowe **Bower/npm** widok pełne polecenie wykonane.</span><span class="sxs-lookup"><span data-stu-id="0deb5-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="0deb5-145">Otwórz *.bowerrc* plików w obszarze *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="0deb5-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="0deb5-146">`directory` Właściwość jest ustawiona na *wwwroot/lib* który wskazuje lokalizację Bower zainstaluje zasoby pakietu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="0deb5-147">Pole wyszukiwania w Eksploratorze rozwiązań umożliwia znaleźć i wyświetlić świetny czcionki pakietu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="0deb5-148">Otwórz *Views\Shared\_Layout.cshtml* plik i dodać świetny czcionki pliku CSS do środowiska [pomocnika tagów](xref:mvc/views/tag-helpers/intro) dla `Development`.</span><span class="sxs-lookup"><span data-stu-id="0deb5-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="0deb5-149">W Eksploratorze rozwiązań, przeciągnij i upuść *awesome.css czcionki* wewnątrz `<environment names="Development">` elementu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="0deb5-150">W aplikacji produkcyjnej należy dodać *awesome.min.css czcionki* do pomocniczego znacznika środowiska dla `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="0deb5-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="0deb5-151">Zastąp zawartość *Views\Home\About.cshtml* pliku Razor z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="0deb5-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="0deb5-152">Uruchom aplikację i przejdź do widoku informacje weryfikowanie działania pakietu świetny czcionki.</span><span class="sxs-lookup"><span data-stu-id="0deb5-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="0deb5-153">Eksploracja proces kompilacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="0deb5-153">Exploring the client-side build process</span></span>

<span data-ttu-id="0deb5-154">Większość szablonów projektu platformy ASP.NET Core są już skonfigurowane do używania Bower.</span><span class="sxs-lookup"><span data-stu-id="0deb5-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="0deb5-155">Ten przewodnik dalej rozpoczyna się od pustego projektu platformy ASP.NET Core i dodaje każdego z nich ręcznie, dzięki czemu możesz uzyskać pewne pojęcie dotyczące sposobu używania Bower w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0deb5-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="0deb5-156">Widać, co się dzieje z struktury projektu i środowiska uruchomieniowego output, ponieważ każda zmiana konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0deb5-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="0deb5-157">Ogólne kroki procesu kompilacji po stronie klienta za pomocą rozwiązania Bower są:</span><span class="sxs-lookup"><span data-stu-id="0deb5-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="0deb5-158">Definiowanie pakietów używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0deb5-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="0deb5-159">Odwołanie pakiety ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="0deb5-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="0deb5-160">Definiowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="0deb5-160">Define packages</span></span>

<span data-ttu-id="0deb5-161">Po listy pakietów *bower.json* pliku, Visual Studio będzie je pobrać.</span><span class="sxs-lookup"><span data-stu-id="0deb5-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="0deb5-162">W poniższym przykładzie użyto Bower załadować jQuery i ładowania początkowego do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="0deb5-163">Utwórz nową aplikację sieci Web platformy ASP.NET Core z **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="0deb5-164">Wybierz **pusty** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="0deb5-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="0deb5-165">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy Projekt > **Dodaj nowy element** i wybierz **pliku konfiguracyjnego Bower**.</span><span class="sxs-lookup"><span data-stu-id="0deb5-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="0deb5-166">Uwaga: A *.bowerrc* również zostanie dodany plik.</span><span class="sxs-lookup"><span data-stu-id="0deb5-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="0deb5-167">Otwórz *bower.json*, Dodaj jquery i bootstrap do `dependencies` sekcji.</span><span class="sxs-lookup"><span data-stu-id="0deb5-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="0deb5-168">Powstałe w ten sposób *bower.json* pliku będzie wyglądać jak w następującym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="0deb5-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="0deb5-169">Wersje zmieni się wraz z upływem czasu i może nie odpowiadać na poniższym obrazie.</span><span class="sxs-lookup"><span data-stu-id="0deb5-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="0deb5-170">Zapisz *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0deb5-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="0deb5-171">Sprawdź projekt zawiera *bootstrap* i *jQuery* katalogów w *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="0deb5-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="0deb5-172">Bower używa *.bowerrc* plik, aby zainstalować zasoby w *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="0deb5-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="0deb5-173">Uwaga: Interfejsu użytkownika "Zarządzaj pakietami Bower" stanowi alternatywę do edycji plik ręcznie.</span><span class="sxs-lookup"><span data-stu-id="0deb5-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="0deb5-174">Włącz pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="0deb5-174">Enable static files</span></span>

* <span data-ttu-id="0deb5-175">Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="0deb5-176">Włącz pliki statyczne z [oprogramowanie pośredniczące plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="0deb5-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="0deb5-177">Dodaj wywołanie do [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) do `Configure` metody `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0deb5-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="0deb5-178">Pakietów odniesienia</span><span class="sxs-lookup"><span data-stu-id="0deb5-178">Reference packages</span></span>

<span data-ttu-id="0deb5-179">W tej sekcji utworzysz stronę HTML, aby sprawdzić, czy można uzyskać dostępu do wdrożone pakiety.</span><span class="sxs-lookup"><span data-stu-id="0deb5-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="0deb5-180">Dodaj nową stronę HTML o nazwie *Index.html* do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="0deb5-181">Uwaga: Należy dodać do pliku w formacie HTML *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="0deb5-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="0deb5-182">Domyślnie funkcja zawartość statyczna nie może zostać wyświetlona poza *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0deb5-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="0deb5-183">Zobacz [pliki statyczne](xref:fundamentals/static-files) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0deb5-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="0deb5-184">Zastąp zawartość *Index.html* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="0deb5-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="0deb5-185">Uruchom aplikację i przejdź do `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="0deb5-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="0deb5-186">Alternatywnie z *Index.html* otwarty, naciśnij klawisz `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="0deb5-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="0deb5-187">Sprawdź stosowanie stylów jumbotron, kodu jQuery odpowiada po kliknięciu przycisku i że Bootstrap przycisku zmienia stan.</span><span class="sxs-lookup"><span data-stu-id="0deb5-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![Styl jumbotron](bower/_static/jumbotron.png)
