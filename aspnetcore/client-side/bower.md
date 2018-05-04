---
title: Zarządzaj pakietami po stronie klienta z Bower w ASP.NET Core
author: rick-anderson
description: Zarządzanie pakietami po stronie klienta z Bower.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 2d6cc526b5a0890103e2856a0ca4b58c5f162c79
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="2fb69-103">Zarządzaj pakietami po stronie klienta z Bower w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fb69-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="2fb69-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2fb69-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2fb69-105">Jednocześnie jest Bower, jego maintainers zaleca się użycie innego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="2fb69-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="2fb69-106">[Menedżer biblioteki](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan skrócie) jest nowy system zarządzania zawartością statycznych po stronie klienta programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2fb69-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side static content management system.</span></span> <span data-ttu-id="2fb69-107">Yarn z Webpack jest jeden popularną alternatywę dla którego [instrukcje migracji](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) są dostępne.</span><span class="sxs-lookup"><span data-stu-id="2fb69-107">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="2fb69-108">[Bower](https://bower.io/) wywołuje sam siebie "Menedżer pakietów dla sieci web".</span><span class="sxs-lookup"><span data-stu-id="2fb69-108">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="2fb69-109">W ekosystemie .NET umieszcza void pozostawionego przez brakiem NuGet do dostarczania zawartości plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="2fb69-109">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="2fb69-110">Dla projektów platformy ASP.NET Core, te pliki statyczne są wbudowane w bibliotekach po stronie klienta, takich jak [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="2fb69-110">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="2fb69-111">W przypadku bibliotek .NET, możesz nadal używać [NuGet](https://www.nuget.org/) Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="2fb69-111">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="2fb69-112">Proces kompilacji nowych projektów utworzonych za pomocą szablonów projektu platformy ASP.NET Core — konfiguracja po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="2fb69-112">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="2fb69-113">[jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/) są zainstalowane i Bower jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="2fb69-113">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="2fb69-114">Pakiety po stronie klienta są wyświetlane w *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="2fb69-114">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="2fb69-115">Szablony projektów platformy ASP.NET Core konfiguruje *bower.json* jQuery, weryfikacji jQuery i ładowania początkowego.</span><span class="sxs-lookup"><span data-stu-id="2fb69-115">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="2fb69-116">W tym samouczku dodamy obsługę [czcionki świetny](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="2fb69-116">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="2fb69-117">Można je zainstalować pakiety bower **Zarządzaj pakietami Bower** interfejsu użytkownika lub ręcznie w *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="2fb69-117">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="2fb69-118">Instalacja za pomocą pakietów Bower Zarządzaj interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="2fb69-118">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="2fb69-119">Utwórz nową aplikację sieci Web platformy ASP.NET Core z **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-119">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="2fb69-120">Wybierz **aplikacji sieci Web** i **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="2fb69-120">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="2fb69-121">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzaj pakietami Bower** (również w menu głównym **projektu** > **Zarządzaj pakietami Bower**).</span><span class="sxs-lookup"><span data-stu-id="2fb69-121">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="2fb69-122">W **Bower: \<Nazwa projektu\>**  , kliknij kartę "Przeglądaj", a następnie przeprowadź filtrowanie listy pakietów, wprowadzając `font-awesome` w polu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="2fb69-122">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![Zarządzaj pakietami bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="2fb69-124">Upewnij się, że "zapisać zmiany w *bower.json*" jest zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="2fb69-124">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="2fb69-125">Wybierz z listy rozwijanej wersję, a następnie kliknij przycisk **zainstalować** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2fb69-125">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="2fb69-126">**Dane wyjściowe** okno zawiera szczegółowe informacje dotyczące instalacji.</span><span class="sxs-lookup"><span data-stu-id="2fb69-126">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="2fb69-127">Ręczna instalacja w pliku bower.json</span><span class="sxs-lookup"><span data-stu-id="2fb69-127">Manual installation in bower.json</span></span>

<span data-ttu-id="2fb69-128">Otwórz *bower.json* pliku, a następnie dodaj "font świetny" do zależności.</span><span class="sxs-lookup"><span data-stu-id="2fb69-128">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="2fb69-129">IntelliSense zawiera dostępnych pakietów.</span><span class="sxs-lookup"><span data-stu-id="2fb69-129">IntelliSense shows the available packages.</span></span> <span data-ttu-id="2fb69-130">Po wybraniu pakietu dostępne wersje są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2fb69-130">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="2fb69-131">Poniżej obrazy są starsze i nie będzie zgodne, zostanie wyświetlony.</span><span class="sxs-lookup"><span data-stu-id="2fb69-131">The images below are older and won't match what you see.</span></span>

![IntelliSense bower Eksploratora pakietów](bower/_static/add-package.png)

![Wersja bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="2fb69-134">Bower używa [wersjonowania semantycznego](http://semver.org/) do organizowania zależności.</span><span class="sxs-lookup"><span data-stu-id="2fb69-134">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="2fb69-135">Wersjonowania semantycznego, znanej także jako programu SemVer identyfikuje pakiety ze schematu numerowania \<głównych >.\< drobne >. \<poprawki >.</span><span class="sxs-lookup"><span data-stu-id="2fb69-135">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="2fb69-136">Przedstawiający kilka typowe opcje IntelliSense upraszcza wersjonowania semantycznego.</span><span class="sxs-lookup"><span data-stu-id="2fb69-136">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="2fb69-137">Pierwszy element na liście IntelliSense (4.6.3 w powyższym przykładzie) jest uznawany za najnowsza stabilna wersja pakietu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-137">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="2fb69-138">Symbol daszek (^) najnowszą wersją główną i tyldy (~) najnowszą wersją pomocniczą.</span><span class="sxs-lookup"><span data-stu-id="2fb69-138">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="2fb69-139">Zapisz *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="2fb69-139">Save the *bower.json* file.</span></span> <span data-ttu-id="2fb69-140">Visual Studio Obserwujący *bower.json* zmiany w pliku.</span><span class="sxs-lookup"><span data-stu-id="2fb69-140">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="2fb69-141">Przy zapisywaniu *instalacji bower* polecenie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="2fb69-141">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="2fb69-142">Zobacz okno dane wyjściowe **Bower/npm** widok pełne polecenie wykonane.</span><span class="sxs-lookup"><span data-stu-id="2fb69-142">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="2fb69-143">Otwórz *.bowerrc* plików w obszarze *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="2fb69-143">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="2fb69-144">`directory` Właściwość jest ustawiona na *wwwroot/lib* który wskazuje lokalizację Bower zainstaluje zasoby pakietu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-144">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="2fb69-145">Pole wyszukiwania w Eksploratorze rozwiązań umożliwia znaleźć i wyświetlić świetny czcionki pakietu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-145">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="2fb69-146">Otwórz *Views\Shared\_Layout.cshtml* plik i dodać świetny czcionki pliku CSS do środowiska [pomocnika tagów](xref:mvc/views/tag-helpers/intro) dla `Development`.</span><span class="sxs-lookup"><span data-stu-id="2fb69-146">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="2fb69-147">W Eksploratorze rozwiązań, przeciągnij i upuść *awesome.css czcionki* wewnątrz `<environment names="Development">` elementu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-147">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="2fb69-148">W aplikacji produkcyjnej należy dodać *awesome.min.css czcionki* do pomocniczego znacznika środowiska dla `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="2fb69-148">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="2fb69-149">Zastąp zawartość *Views\Home\About.cshtml* pliku Razor z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2fb69-149">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="2fb69-150">Uruchom aplikację i przejdź do widoku informacje weryfikowanie działania pakietu świetny czcionki.</span><span class="sxs-lookup"><span data-stu-id="2fb69-150">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="2fb69-151">Eksploracja proces kompilacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="2fb69-151">Exploring the client-side build process</span></span>

<span data-ttu-id="2fb69-152">Większość szablonów projektu platformy ASP.NET Core są już skonfigurowane do używania Bower.</span><span class="sxs-lookup"><span data-stu-id="2fb69-152">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="2fb69-153">Ten przewodnik dalej rozpoczyna się od pustego projektu platformy ASP.NET Core i dodaje każdego z nich ręcznie, dzięki czemu możesz uzyskać pewne pojęcie dotyczące sposobu używania Bower w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2fb69-153">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="2fb69-154">Widać, co się dzieje z struktury projektu i środowiska uruchomieniowego output, ponieważ każda zmiana konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2fb69-154">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="2fb69-155">Ogólne kroki procesu kompilacji po stronie klienta za pomocą rozwiązania Bower są:</span><span class="sxs-lookup"><span data-stu-id="2fb69-155">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="2fb69-156">Definiowanie pakietów używany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2fb69-156">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="2fb69-157">Odwołanie pakiety ze stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="2fb69-157">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="2fb69-158">Definiowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="2fb69-158">Define packages</span></span>

<span data-ttu-id="2fb69-159">Po listy pakietów *bower.json* pliku, Visual Studio będzie je pobrać.</span><span class="sxs-lookup"><span data-stu-id="2fb69-159">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="2fb69-160">W poniższym przykładzie użyto Bower załadować jQuery i ładowania początkowego do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-160">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="2fb69-161">Utwórz nową aplikację sieci Web platformy ASP.NET Core z **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-161">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="2fb69-162">Wybierz **pusty** szablonu projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2fb69-162">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="2fb69-163">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy Projekt > **Dodaj nowy element** i wybierz **pliku konfiguracyjnego Bower**.</span><span class="sxs-lookup"><span data-stu-id="2fb69-163">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="2fb69-164">Uwaga: A *.bowerrc* również zostanie dodany plik.</span><span class="sxs-lookup"><span data-stu-id="2fb69-164">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="2fb69-165">Otwórz *bower.json*, Dodaj jquery i bootstrap do `dependencies` sekcji.</span><span class="sxs-lookup"><span data-stu-id="2fb69-165">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="2fb69-166">Powstałe w ten sposób *bower.json* pliku będzie wyglądać jak w następującym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="2fb69-166">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="2fb69-167">Wersje zmieni się wraz z upływem czasu i może nie odpowiadać na poniższym obrazie.</span><span class="sxs-lookup"><span data-stu-id="2fb69-167">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="2fb69-168">Zapisz *bower.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="2fb69-168">Save the *bower.json* file.</span></span>

  <span data-ttu-id="2fb69-169">Sprawdź projekt zawiera *bootstrap* i *jQuery* katalogów w *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="2fb69-169">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="2fb69-170">Bower używa *.bowerrc* plik, aby zainstalować zasoby w *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="2fb69-170">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="2fb69-171">Uwaga: Interfejsu użytkownika "Zarządzaj pakietami Bower" stanowi alternatywę do edycji plik ręcznie.</span><span class="sxs-lookup"><span data-stu-id="2fb69-171">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="2fb69-172">Włącz pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="2fb69-172">Enable static files</span></span>

* <span data-ttu-id="2fb69-173">Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-173">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="2fb69-174">Włącz pliki statyczne z [oprogramowanie pośredniczące plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="2fb69-174">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="2fb69-175">Dodaj wywołanie do [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) do `Configure` metody `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2fb69-175">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="2fb69-176">Pakietów odniesienia</span><span class="sxs-lookup"><span data-stu-id="2fb69-176">Reference packages</span></span>

<span data-ttu-id="2fb69-177">W tej sekcji utworzysz stronę HTML, aby sprawdzić, czy można uzyskać dostępu do wdrożone pakiety.</span><span class="sxs-lookup"><span data-stu-id="2fb69-177">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="2fb69-178">Dodaj nową stronę HTML o nazwie *Index.html* do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-178">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="2fb69-179">Uwaga: Należy dodać do pliku w formacie HTML *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="2fb69-179">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="2fb69-180">Domyślnie funkcja zawartość statyczna nie może zostać wyświetlona poza *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2fb69-180">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="2fb69-181">Zobacz [pracować z plikami statycznych](xref:fundamentals/static-files) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2fb69-181">See [Work with static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="2fb69-182">Zastąp zawartość *Index.html* z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2fb69-182">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="2fb69-183">Uruchom aplikację i przejdź do `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="2fb69-183">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="2fb69-184">Alternatywnie z *Index.html* otwarty, naciśnij klawisz `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="2fb69-184">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="2fb69-185">Sprawdź stosowanie stylów jumbotron, kodu jQuery odpowiada po kliknięciu przycisku i że Bootstrap przycisku zmienia stan.</span><span class="sxs-lookup"><span data-stu-id="2fb69-185">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![Styl jumbotron](bower/_static/jumbotron.png)
