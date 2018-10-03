---
title: Pliki statyczne z platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obsługiwać oraz zabezpieczanie plików statycznych i konfigurowanie plików statycznych hostingu zachowania oprogramowania pośredniczącego w aplikacji sieci web platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
uid: fundamentals/static-files
ms.openlocfilehash: 52c7916b9fc55c875d56acd49c01f76dd2053817
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861008"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="e7af0-103">Pliki statyczne z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7af0-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="e7af0-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e7af0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e7af0-105">Pliki statyczne, takich jak HTML, CSS, obrazów i JavaScript, to zasoby, których aplikacji ASP.NET Core służy bezpośrednio do klientów.</span><span class="sxs-lookup"><span data-stu-id="e7af0-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="e7af0-106">Niektóre konfiguracja jest wymagana do włączenia do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="e7af0-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="e7af0-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e7af0-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="e7af0-108">Obsługa plików statycznych</span><span class="sxs-lookup"><span data-stu-id="e7af0-108">Serve static files</span></span>

<span data-ttu-id="e7af0-109">Pliki statyczne są przechowywane w katalogu głównym projektu sieci web.</span><span class="sxs-lookup"><span data-stu-id="e7af0-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="e7af0-110">Domyślny katalog jest  *\<content_root > / wwwroot*, ale można ją zmienić za pomocą [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metody.</span><span class="sxs-lookup"><span data-stu-id="e7af0-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="e7af0-111">Zobacz [zawartości głównego](xref:fundamentals/index#content-root) i [katalog główny sieci Web](xref:fundamentals/index#web-root) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e7af0-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="e7af0-112">Hosta sieci web aplikacji należy pamiętać o zawartości katalogu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7af0-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7af0-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="e7af0-114">`WebHost.CreateDefaultBuilder` Metoda ustawia zawartość katalogu głównego w bieżącym katalogu:</span><span class="sxs-lookup"><span data-stu-id="e7af0-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7af0-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7af0-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e7af0-116">Ustaw zawartość katalogu głównego w bieżącym katalogu, wywołując [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) wewnątrz `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="e7af0-117">Pliki statyczne są dostępne za pośrednictwem ścieżki względem katalogu głównego sieci web.</span><span class="sxs-lookup"><span data-stu-id="e7af0-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="e7af0-118">Na przykład **aplikacji sieci Web** szablonu projektu zawiera kilka folderów w ramach *wwwroot* folderu:</span><span class="sxs-lookup"><span data-stu-id="e7af0-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="e7af0-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="e7af0-119">**wwwroot**</span></span>
  * <span data-ttu-id="e7af0-120">**CSS**</span><span class="sxs-lookup"><span data-stu-id="e7af0-120">**css**</span></span>
  * <span data-ttu-id="e7af0-121">**images**</span><span class="sxs-lookup"><span data-stu-id="e7af0-121">**images**</span></span>
  * <span data-ttu-id="e7af0-122">**js**</span><span class="sxs-lookup"><span data-stu-id="e7af0-122">**js**</span></span>

<span data-ttu-id="e7af0-123">Format identyfikatora URI, aby uzyskać dostęp do pliku w *obrazów* podfolder jest *http://\<server_address > /images/\<image_file_name >*.</span><span class="sxs-lookup"><span data-stu-id="e7af0-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="e7af0-124">Na przykład *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="e7af0-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7af0-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7af0-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e7af0-126">Jeśli przeznaczony dla .NET Framework, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="e7af0-127">Jeśli przeznaczony dla platformy .NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) obejmuje tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7af0-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7af0-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e7af0-129">Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="e7af0-130">Konfigurowanie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) umożliwiająca obsługi plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="e7af0-130">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="e7af0-131">Udostępniać pliki wewnątrz katalog główny sieci web</span><span class="sxs-lookup"><span data-stu-id="e7af0-131">Serve files inside of web root</span></span>

<span data-ttu-id="e7af0-132">Wywoływanie [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodę w ramach `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="e7af0-133">Bez parametrów `UseStaticFiles` przeciążenie metody oznacza pliki w katalogu głównym sieci web, jako servable.</span><span class="sxs-lookup"><span data-stu-id="e7af0-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="e7af0-134">Następujące odwołania do znaczników *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="e7af0-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="e7af0-135">Udostępniania plików poza katalogiem głównym sieci web</span><span class="sxs-lookup"><span data-stu-id="e7af0-135">Serve files outside of web root</span></span>

<span data-ttu-id="e7af0-136">Należy wziąć pod uwagę hierarchii katalogów, w którym znajdują się pliki statyczne, które ma zostać dostarczony poza katalog główny sieci web:</span><span class="sxs-lookup"><span data-stu-id="e7af0-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="e7af0-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="e7af0-137">**wwwroot**</span></span>
  * <span data-ttu-id="e7af0-138">**CSS**</span><span class="sxs-lookup"><span data-stu-id="e7af0-138">**css**</span></span>
  * <span data-ttu-id="e7af0-139">**images**</span><span class="sxs-lookup"><span data-stu-id="e7af0-139">**images**</span></span>
  * <span data-ttu-id="e7af0-140">**js**</span><span class="sxs-lookup"><span data-stu-id="e7af0-140">**js**</span></span>
* <span data-ttu-id="e7af0-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="e7af0-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="e7af0-142">**images**</span><span class="sxs-lookup"><span data-stu-id="e7af0-142">**images**</span></span>
      * <span data-ttu-id="e7af0-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="e7af0-143">*banner1.svg*</span></span>

<span data-ttu-id="e7af0-144">Żądanie mogą uzyskiwać dostęp do *banner1.svg* pliku, konfigurując oprogramowanie pośredniczące plików statycznych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e7af0-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="e7af0-145">W poprzednim kodzie *MyStaticFiles* hierarchii katalogów jest udostępniane publicznie *StaticFiles* segmentem identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="e7af0-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="e7af0-146">Żądanie *http://\<server_address > /StaticFiles/images/banner1.svg* służy *banner1.svg* pliku.</span><span class="sxs-lookup"><span data-stu-id="e7af0-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="e7af0-147">Następujące odwołania do znaczników *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="e7af0-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="e7af0-148">Ustawianie nagłówków odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="e7af0-148">Set HTTP response headers</span></span>

<span data-ttu-id="e7af0-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) obiekt może służyć do ustawiania nagłówków odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7af0-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="e7af0-150">Oprócz konfigurowania dostarczanie plików statycznych z katalogu głównego sieci web, poniższy kod ustawia `Cache-Control` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="e7af0-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="e7af0-151">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) istnieje metoda w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="e7af0-152">Pliki zostały wprowadzone publicznie podlega buforowaniu, na 10 minut (600 sekund) w środowisku deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="e7af0-152">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![Nagłówki odpowiedzi, wyświetlanie nagłówek Cache-Control został dodany.](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="e7af0-154">Statyczny plik autoryzacji</span><span class="sxs-lookup"><span data-stu-id="e7af0-154">Static file authorization</span></span>

<span data-ttu-id="e7af0-155">Oprogramowanie pośredniczące plików statycznych nie zapewnia sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e7af0-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="e7af0-156">Wszystkich plików obsługiwanych przez, łącznie z tymi w ramach *wwwroot*, są dostępne publicznie.</span><span class="sxs-lookup"><span data-stu-id="e7af0-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="e7af0-157">Aby udostępniać pliki na podstawie autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="e7af0-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="e7af0-158">Store je poza *wwwroot* i z dowolnego katalogu, które są dostępne dla oprogramowanie pośredniczące plików statycznych **i**</span><span class="sxs-lookup"><span data-stu-id="e7af0-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="e7af0-159">Obsługujących je za pośrednictwem metody akcji, do którego zastosowano autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e7af0-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="e7af0-160">Zwróć [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) obiektu:</span><span class="sxs-lookup"><span data-stu-id="e7af0-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="e7af0-161">Umożliwia przeglądanie katalogów</span><span class="sxs-lookup"><span data-stu-id="e7af0-161">Enable directory browsing</span></span>

<span data-ttu-id="e7af0-162">Przeglądanie katalogów umożliwia użytkownikom aplikacji sieci web wyświetlić listę katalogów i plików w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="e7af0-163">Przeglądanie katalogów jest domyślnie wyłączona, ze względów bezpieczeństwa (zobacz [zagadnienia](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="e7af0-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="e7af0-164">Przeglądanie za pomocą wywołania katalogów Włącz [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in Class metoda `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="e7af0-165">Dodaj wymagane usługi, wywołując [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metody z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="e7af0-166">Powyższy kod pozwala na przeglądanie katalogu *wwwroot/obrazów* folderu przy użyciu adresu URL *http://\<server_address > / Mojeobrazy*, wraz z łączami do poszczególnych plików i folderów:</span><span class="sxs-lookup"><span data-stu-id="e7af0-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

<span data-ttu-id="e7af0-168">Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.</span><span class="sxs-lookup"><span data-stu-id="e7af0-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="e7af0-169">Należy pamiętać, dwa `UseStaticFiles` wywołuje w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e7af0-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="e7af0-170">Pierwsze wywołanie umożliwia obsługi plików statycznych w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="e7af0-171">Drugie wywołanie umożliwia przeglądanie katalogów dla *wwwroot/obrazów* folderu przy użyciu adresu URL *http://\<server_address > / Mojeobrazy*:</span><span class="sxs-lookup"><span data-stu-id="e7af0-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="e7af0-172">Udostępniania dokumentu domyślnego</span><span class="sxs-lookup"><span data-stu-id="e7af0-172">Serve a default document</span></span>

<span data-ttu-id="e7af0-173">Ustawianie domyślnej strony głównej zawiera odwiedzających logiczne punkt początkowy podczas odwiedzania Twojej witryny.</span><span class="sxs-lookup"><span data-stu-id="e7af0-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="e7af0-174">Aby obsługiwać domyślną stronę bez żadnego użytkownika, w pełni kwalifikujących się identyfikator URI, należy wywołać [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metody z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="e7af0-175">`UseDefaultFiles` musi zostać wywołana przed `UseStaticFiles` do obsługi domyślnego pliku.</span><span class="sxs-lookup"><span data-stu-id="e7af0-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="e7af0-176">`UseDefaultFiles` jest dysków adresu URL, który faktycznie nie obsługuje pliku.</span><span class="sxs-lookup"><span data-stu-id="e7af0-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="e7af0-177">Włącz oprogramowanie pośredniczące plików statycznych przy użyciu `UseStaticFiles` do udostępniania plików.</span><span class="sxs-lookup"><span data-stu-id="e7af0-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="e7af0-178">Za pomocą `UseDefaultFiles`, żądania do folderu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="e7af0-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="e7af0-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="e7af0-179">*default.htm*</span></span>
* <span data-ttu-id="e7af0-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="e7af0-180">*default.html*</span></span>
* <span data-ttu-id="e7af0-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="e7af0-181">*index.htm*</span></span>
* <span data-ttu-id="e7af0-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="e7af0-182">*index.html*</span></span>

<span data-ttu-id="e7af0-183">Pierwszy plik znaleziono z listy jest obsługiwany, tak, jakby żądania zostały w pełni kwalifikowanego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="e7af0-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="e7af0-184">Adres URL przeglądarki w dalszym ciągu odzwierciedla żądanego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="e7af0-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="e7af0-185">Poniższy kod zmienia domyślna nazwa pliku do *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="e7af0-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="e7af0-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="e7af0-186">UseFileServer</span></span>

<span data-ttu-id="e7af0-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) łączy w sobie funkcje `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="e7af0-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="e7af0-188">Poniższy kod umożliwia obsługujących pliki statyczne i domyślnego pliku.</span><span class="sxs-lookup"><span data-stu-id="e7af0-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="e7af0-189">Przeglądanie katalogów nie jest włączone.</span><span class="sxs-lookup"><span data-stu-id="e7af0-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="e7af0-190">Poniższy kod tworzy po przeciążenie bez parametrów, należy włączyć, przeglądanie katalogów:</span><span class="sxs-lookup"><span data-stu-id="e7af0-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="e7af0-191">Należy wziąć pod uwagę następujące hierarchii katalogów:</span><span class="sxs-lookup"><span data-stu-id="e7af0-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="e7af0-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="e7af0-192">**wwwroot**</span></span>
  * <span data-ttu-id="e7af0-193">**CSS**</span><span class="sxs-lookup"><span data-stu-id="e7af0-193">**css**</span></span>
  * <span data-ttu-id="e7af0-194">**images**</span><span class="sxs-lookup"><span data-stu-id="e7af0-194">**images**</span></span>
  * <span data-ttu-id="e7af0-195">**js**</span><span class="sxs-lookup"><span data-stu-id="e7af0-195">**js**</span></span>
* <span data-ttu-id="e7af0-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="e7af0-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="e7af0-197">**images**</span><span class="sxs-lookup"><span data-stu-id="e7af0-197">**images**</span></span>
      * <span data-ttu-id="e7af0-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="e7af0-198">*banner1.svg*</span></span>
  * <span data-ttu-id="e7af0-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="e7af0-199">*default.html*</span></span>

<span data-ttu-id="e7af0-200">Poniższy kod umożliwia pliki statyczne, domyślne pliki i przeglądanie katalogów dla `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="e7af0-201">`AddDirectoryBrowser` musi być wywoływana, gdy `EnableDirectoryBrowsing` wartość właściwości jest `true`:</span><span class="sxs-lookup"><span data-stu-id="e7af0-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="e7af0-202">Za pomocą hierarchii plików i poprzedni kod, adresy URL rozwiązać problem w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e7af0-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="e7af0-203">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="e7af0-203">URI</span></span>            |                             <span data-ttu-id="e7af0-204">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="e7af0-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="e7af0-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="e7af0-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="e7af0-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="e7af0-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="e7af0-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="e7af0-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="e7af0-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="e7af0-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="e7af0-209">Jeśli plik o nazwie domyślnej, nie istnieje w *MyStaticFiles* katalogu *http://\<server_address > / StaticFiles* zwraca listę wraz z łączami możesz klikać katalogów:</span><span class="sxs-lookup"><span data-stu-id="e7af0-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Lista plików statycznych](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="e7af0-211">`UseDefaultFiles` i `UseDirectoryBrowser` Użyj adresu URL *http://\<server_address > / StaticFiles* bez ukośnika, aby wyzwolić po stronie klienta przekierowania do *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="e7af0-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="e7af0-212">Zwróć uwagę, dodanie końcowy ukośnik.</span><span class="sxs-lookup"><span data-stu-id="e7af0-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="e7af0-213">Względnych adresów URL w ramach dokumenty zostaną uznane za nieprawidłowe bez znaku ukośnika na końcu.</span><span class="sxs-lookup"><span data-stu-id="e7af0-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="e7af0-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="e7af0-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="e7af0-215">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) klasa zawiera `Mappings` właściwość służy jako mapowanie rozszerzenia plików do typu MIME.</span><span class="sxs-lookup"><span data-stu-id="e7af0-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="e7af0-216">W poniższym przykładzie kilka rozszerzeń plików są zarejestrowane do znanych typów MIME.</span><span class="sxs-lookup"><span data-stu-id="e7af0-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="e7af0-217">*.Rtf* rozszerzenia są zastępowane, oraz *MP4* zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="e7af0-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="e7af0-218">Zobacz [typu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="e7af0-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="e7af0-219">Niestandardowe typy zawartości</span><span class="sxs-lookup"><span data-stu-id="e7af0-219">Non-standard content types</span></span>

<span data-ttu-id="e7af0-220">Oprogramowanie pośredniczące plików statycznych rozumie prawie 400 znanych typów zawartości.</span><span class="sxs-lookup"><span data-stu-id="e7af0-220">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="e7af0-221">Jeśli użytkownik żąda pliku z nieznany typ pliku, oprogramowanie pośredniczące plików statycznych przekazuje żądanie do następnego oprogramowania pośredniczącego w potoku.</span><span class="sxs-lookup"><span data-stu-id="e7af0-221">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="e7af0-222">Jeśli nie oprogramowanie pośredniczące obsługuje żądania, *404 Nie znaleziono* zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e7af0-222">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="e7af0-223">Jeśli przeglądanie katalogów jest włączona, łącze do pliku jest wyświetlany na liście katalogów.</span><span class="sxs-lookup"><span data-stu-id="e7af0-223">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="e7af0-224">Poniższy kod umożliwia obsługująca nieznane typy i renderuje nieznanym pliku jako obrazu:</span><span class="sxs-lookup"><span data-stu-id="e7af0-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="e7af0-225">Przy użyciu poprzedniego kodu żądanie dotyczące pliku z nieznanym typem zawartości jest zwracana jako obraz.</span><span class="sxs-lookup"><span data-stu-id="e7af0-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="e7af0-226">Włączanie [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="e7af0-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="e7af0-227">Jest domyślnie wyłączona, a jego użycie nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="e7af0-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="e7af0-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) bezpieczniejsze alternatywą do obsługujących pliki z rozszerzeniami niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="e7af0-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="e7af0-229">Uwagi</span><span class="sxs-lookup"><span data-stu-id="e7af0-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="e7af0-230">`UseDirectoryBrowser` i `UseStaticFiles` można przecieku wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="e7af0-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="e7af0-231">Zdecydowanie zaleca się wyłączenie przeglądanie katalogów w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="e7af0-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="e7af0-232">Dokładnie przejrzyj katalogi, które są włączone za pomocą `UseStaticFiles` lub `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="e7af0-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="e7af0-233">Cały katalog i jego podkatalogi stają się publicznie dostępny.</span><span class="sxs-lookup"><span data-stu-id="e7af0-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="e7af0-234">Store plików odpowiednie dla obsługująca publicznie w dedykowanym katalogu, takie jak  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e7af0-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="e7af0-235">Te pliki należy oddzielić od widoków MVC, stron Razor (tylko 2.x), pliki konfiguracji itp.</span><span class="sxs-lookup"><span data-stu-id="e7af0-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="e7af0-236">Adresy URL zawartość jest uwidaczniana z `UseDirectoryBrowser` i `UseStaticFiles` podlegają rozróżnianie wielkości liter i ograniczenia dotyczące znaków bazowego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="e7af0-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="e7af0-237">Na przykład Windows jest uwzględniana wielkość liter&mdash;systemach macOS i Linux nie są.</span><span class="sxs-lookup"><span data-stu-id="e7af0-237">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="e7af0-238">Aplikacje platformy ASP.NET Core hostowanych w użyciu IIS [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do przekazywania wszystkich żądań do aplikacji, w tym realizowania żądań plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="e7af0-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="e7af0-239">Obsługa plików statycznych usług IIS nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="e7af0-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="e7af0-240">Ma on możliwość obsługi żądań przed są one obsługiwane przez moduł.</span><span class="sxs-lookup"><span data-stu-id="e7af0-240">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="e7af0-241">Wykonaj następujące czynności w Menedżerze usług IIS, aby usunąć program obsługi plików statycznych usług IIS na poziomie serwera lub witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="e7af0-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="e7af0-242">Przejdź do **modułów** funkcji.</span><span class="sxs-lookup"><span data-stu-id="e7af0-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="e7af0-243">Wybierz **StaticFileModule** na liście.</span><span class="sxs-lookup"><span data-stu-id="e7af0-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="e7af0-244">Kliknij przycisk **Usuń** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="e7af0-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="e7af0-245">Po włączeniu obsługi plików statycznych IIS **i** modułu ASP.NET Core jest nieprawidłowo skonfigurowany, pliki statyczne są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e7af0-245">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="e7af0-246">Dzieje się tak, na przykład, jeśli *web.config* plik nie jest wdrożony.</span><span class="sxs-lookup"><span data-stu-id="e7af0-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="e7af0-247">Umieść pliki kodu (w tym *.cs* i *.cshtml*) poza katalogiem głównym pakietu projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="e7af0-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="e7af0-248">W związku z tym powstaje separacji logicznej między zawartości po stronie klienta i kod na serwerze aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e7af0-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="e7af0-249">Zapobiega to wyciek kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="e7af0-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7af0-250">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e7af0-250">Additional resources</span></span>

* [<span data-ttu-id="e7af0-251">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="e7af0-251">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="e7af0-252">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7af0-252">Introduction to ASP.NET Core</span></span>](xref:index)
