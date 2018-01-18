---
title: Praca z plikami statycznych w ASP.NET Core
author: rick-anderson
description: "Dowiedz się, jak obsługiwać i zabezpieczania pliki statyczne oraz skonfigurować plik statyczny hosting zachowania oprogramowania pośredniczącego w aplikacji sieci web platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, pliki statyczne, zasoby statyczne, HTML, CSS, JavaScript
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="dba75-104">Praca z plikami statycznych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dba75-104">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="dba75-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="dba75-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="dba75-106">Pliki statyczne, takich jak HTML, CSS, obrazy i JavaScript, są trwałe, które służy aplikacji platformy ASP.NET Core bezpośrednio do klientów.</span><span class="sxs-lookup"><span data-stu-id="dba75-106">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="dba75-107">Niektóre konfiguracja jest wymagana, aby włączyć do obsługi tych plików.</span><span class="sxs-lookup"><span data-stu-id="dba75-107">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="dba75-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dba75-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="dba75-109">Obsługiwać pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="dba75-109">Serve static files</span></span>

<span data-ttu-id="dba75-110">Pliki statyczne są przechowywane w katalogu głównym projektu sieci web.</span><span class="sxs-lookup"><span data-stu-id="dba75-110">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="dba75-111">Domyślny katalog jest  *\<content_root > / wwwroot*, ale można ją zmienić za pomocą [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metody.</span><span class="sxs-lookup"><span data-stu-id="dba75-111">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="dba75-112">Zobacz [zawartości głównego](xref:fundamentals/index#content-root) i [głównego sieci Web](xref:fundamentals/index#web-root) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="dba75-112">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="dba75-113">Host sieci web aplikacji należy pamiętać o zawartości katalogu.</span><span class="sxs-lookup"><span data-stu-id="dba75-113">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dba75-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dba75-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dba75-115">`WebHost.CreateDefaultBuilder` Metoda ustawia zawartości katalogu głównego w bieżącym katalogu:</span><span class="sxs-lookup"><span data-stu-id="dba75-115">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dba75-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dba75-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dba75-117">Ustaw główny zawartości do bieżącego katalogu, wywołując [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) wewnątrz `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="dba75-117">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="dba75-118">Pliki statyczne są dostępne za pośrednictwem ścieżka względem katalogu głównego sieci web.</span><span class="sxs-lookup"><span data-stu-id="dba75-118">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="dba75-119">Na przykład **aplikacji sieci Web** szablon projektu zawiera kilka folderów w ramach *wwwroot* folderu:</span><span class="sxs-lookup"><span data-stu-id="dba75-119">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="dba75-120">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="dba75-120">**wwwroot**</span></span>
  * <span data-ttu-id="dba75-121">**CSS**</span><span class="sxs-lookup"><span data-stu-id="dba75-121">**css**</span></span>
  * <span data-ttu-id="dba75-122">**images**</span><span class="sxs-lookup"><span data-stu-id="dba75-122">**images**</span></span>
  * <span data-ttu-id="dba75-123">**js**</span><span class="sxs-lookup"><span data-stu-id="dba75-123">**js**</span></span>

<span data-ttu-id="dba75-124">Dostęp do pliku w formacie identyfikatora URI *obrazów* podfolder jest *http://\<server_address > /images/\<image_file_name >*.</span><span class="sxs-lookup"><span data-stu-id="dba75-124">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="dba75-125">Na przykład *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="dba75-125">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dba75-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dba75-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dba75-127">Jeśli przeznaczonych dla platformy .NET Framework, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="dba75-127">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="dba75-128">Jeśli przeznaczonych dla platformy .NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) obejmuje tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="dba75-128">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dba75-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dba75-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dba75-130">Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="dba75-130">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="dba75-131">Skonfiguruj [oprogramowanie pośredniczące](xref:fundamentals/middleware) co pozwala obsługi plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="dba75-131">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="dba75-132">Udostępniać pliki wewnątrz głównego sieci web</span><span class="sxs-lookup"><span data-stu-id="dba75-132">Serve files inside of web root</span></span>

<span data-ttu-id="dba75-133">Wywołanie [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodę `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="dba75-133">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="dba75-134">Bez parametrów `UseStaticFiles` pliki w katalogu głównym sieci web jako servable oznacza przeciążenie metody.</span><span class="sxs-lookup"><span data-stu-id="dba75-134">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="dba75-135">Następujące odwołania znaczników *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="dba75-135">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="dba75-136">Rozpoznawanie plików poza katalogiem głównym sieci web</span><span class="sxs-lookup"><span data-stu-id="dba75-136">Serve files outside of web root</span></span>

<span data-ttu-id="dba75-137">Należy wziąć pod uwagę hierarchii katalogów, w którym znajdują się pliki statyczne, który ma zostać dostarczony poza katalogiem głównym sieci web:</span><span class="sxs-lookup"><span data-stu-id="dba75-137">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="dba75-138">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="dba75-138">**wwwroot**</span></span>
  * <span data-ttu-id="dba75-139">**CSS**</span><span class="sxs-lookup"><span data-stu-id="dba75-139">**css**</span></span>
  * <span data-ttu-id="dba75-140">**images**</span><span class="sxs-lookup"><span data-stu-id="dba75-140">**images**</span></span>
  * <span data-ttu-id="dba75-141">**js**</span><span class="sxs-lookup"><span data-stu-id="dba75-141">**js**</span></span>
* <span data-ttu-id="dba75-142">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="dba75-142">**MyStaticFiles**</span></span>
  * <span data-ttu-id="dba75-143">**images**</span><span class="sxs-lookup"><span data-stu-id="dba75-143">**images**</span></span>
      * <span data-ttu-id="dba75-144">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="dba75-144">*banner1.svg*</span></span>

<span data-ttu-id="dba75-145">Żądanie mogą uzyskiwać dostęp do *banner1.svg* pliku przez skonfigurowanie oprogramowanie pośredniczące plików statycznych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dba75-145">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="dba75-146">W powyższym kodzie *MyStaticFiles* hierarchii katalogów jest udostępniane publicznie za pośrednictwem funkcji *StaticFiles* segmentem identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="dba75-146">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="dba75-147">Żądanie *http://\<server_address > /StaticFiles/images/banner1.svg* służy *banner1.svg* pliku.</span><span class="sxs-lookup"><span data-stu-id="dba75-147">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="dba75-148">Następujące odwołania znaczników *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="dba75-148">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="dba75-149">Ustawianie nagłówków odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="dba75-149">Set HTTP response headers</span></span>

<span data-ttu-id="dba75-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) obiekt może służyć do ustawienia nagłówków odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="dba75-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="dba75-151">Oprócz konfigurowania obsługi plików statycznych z katalogu głównego sieci web, poniższy kod ustawia `Cache-Control` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="dba75-151">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="dba75-152">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) istnieje metoda w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="dba75-152">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="dba75-153">Pliki zostały wprowadzone publicznie buforowalnej przez 10 minut (600 sekund):</span><span class="sxs-lookup"><span data-stu-id="dba75-153">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Dodano przedstawiający nagłówek Cache-Control nagłówki odpowiedzi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="dba75-155">Plik statyczny autoryzacji</span><span class="sxs-lookup"><span data-stu-id="dba75-155">Static file authorization</span></span>

<span data-ttu-id="dba75-156">Oprogramowanie pośredniczące plików statycznych nie zapewnia sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="dba75-156">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="dba75-157">Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot*, są dostępne publicznie.</span><span class="sxs-lookup"><span data-stu-id="dba75-157">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="dba75-158">Do obsługi plików na podstawie autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="dba75-158">To serve files based on authorization:</span></span>

* <span data-ttu-id="dba75-159">Przechowywania ich poza *wwwroot* i dowolnego katalogu, które są dostępne dla oprogramowania pośredniczącego plików statycznych **i**</span><span class="sxs-lookup"><span data-stu-id="dba75-159">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="dba75-160">Obsługujących je za pomocą metody akcji, do którego zastosowano autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="dba75-160">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="dba75-161">Zwraca [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) obiektu:</span><span class="sxs-lookup"><span data-stu-id="dba75-161">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="dba75-162">Umożliwia przeglądanie katalogów</span><span class="sxs-lookup"><span data-stu-id="dba75-162">Enable directory browsing</span></span>

<span data-ttu-id="dba75-163">Przeglądanie katalogów umożliwia użytkownikom aplikacji sieci web wyświetlić listę katalogów i plików w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="dba75-163">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="dba75-164">Przeglądanie katalogów jest domyślnie wyłączona, ze względów bezpieczeństwa (zobacz [zagadnienia](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="dba75-164">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="dba75-165">Przeglądanie wywołując katalogów Włącz [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metody w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="dba75-165">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="dba75-166">Dodaj wymagane usługi, wywołując [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metody z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="dba75-166">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="dba75-167">Poprzedni kod umożliwia przeglądanie katalogów z *wwwroot/obrazy* folder przy użyciu adresu URL *http://\<server_address > / MyImages*, wraz z łączami do poszczególnych plików i folderów:</span><span class="sxs-lookup"><span data-stu-id="dba75-167">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

<span data-ttu-id="dba75-169">Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.</span><span class="sxs-lookup"><span data-stu-id="dba75-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="dba75-170">Należy zwrócić uwagę dwa `UseStaticFiles` wywołuje w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="dba75-170">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="dba75-171">Pierwsze wywołanie umożliwia obsługi plików statycznych w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="dba75-171">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="dba75-172">Drugie wywołanie umożliwia przeglądanie katalogów z *wwwroot/obrazy* folder przy użyciu adresu URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="dba75-172">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="dba75-173">Udostępniania dokumentu domyślnego</span><span class="sxs-lookup"><span data-stu-id="dba75-173">Serve a default document</span></span>

<span data-ttu-id="dba75-174">Ustawianie domyślnej strony głównej stanowi odwiedzających logicznej punkt początkowy podczas odwiedzania witryny.</span><span class="sxs-lookup"><span data-stu-id="dba75-174">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="dba75-175">Aby obsługiwać domyślną stronę bez użytkownika pełni kwalifikujących się identyfikator URI, należy wywołać [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metody z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="dba75-175">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="dba75-176">`UseDefaultFiles`musi zostać wywołana przed `UseStaticFiles` do obsługi domyślnego pliku.</span><span class="sxs-lookup"><span data-stu-id="dba75-176">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="dba75-177">`UseDefaultFiles`jest funkcji ponownego zapisu adresu URL, który faktycznie nie mógł być pliku.</span><span class="sxs-lookup"><span data-stu-id="dba75-177">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="dba75-178">Włącz oprogramowanie pośredniczące plików statycznych przy użyciu `UseStaticFiles` do obsługi pliku.</span><span class="sxs-lookup"><span data-stu-id="dba75-178">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="dba75-179">Z `UseDefaultFiles`, żądania do folderu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="dba75-179">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="dba75-180">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="dba75-180">*default.htm*</span></span>
* <span data-ttu-id="dba75-181">*default.html*</span><span class="sxs-lookup"><span data-stu-id="dba75-181">*default.html*</span></span>
* <span data-ttu-id="dba75-182">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="dba75-182">*index.htm*</span></span>
* <span data-ttu-id="dba75-183">*index.html*</span><span class="sxs-lookup"><span data-stu-id="dba75-183">*index.html*</span></span>

<span data-ttu-id="dba75-184">Pierwszy plik znaleziono na liście jest wyświetlona tak, jakby żądania zostały w pełni kwalifikowanego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="dba75-184">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="dba75-185">Adres URL przeglądarki w dalszym ciągu odzwierciedlał żądanego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="dba75-185">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="dba75-186">Poniższy kod zmienia domyślnej nazwy pliku do *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="dba75-186">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="dba75-187">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="dba75-187">UseFileServer</span></span>

<span data-ttu-id="dba75-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) łączą funkcjonalność `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="dba75-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="dba75-189">Poniższy kod umożliwia obsługujących pliki statyczne i domyślnego pliku.</span><span class="sxs-lookup"><span data-stu-id="dba75-189">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="dba75-190">Przeglądanie katalogów nie jest włączone.</span><span class="sxs-lookup"><span data-stu-id="dba75-190">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="dba75-191">Następujący kod, bazując na przeciążenia bez parametrów, należy włączyć, przeglądanie katalogów:</span><span class="sxs-lookup"><span data-stu-id="dba75-191">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="dba75-192">Należy wziąć pod uwagę następujące hierarchii katalogów:</span><span class="sxs-lookup"><span data-stu-id="dba75-192">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="dba75-193">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="dba75-193">**wwwroot**</span></span>
  * <span data-ttu-id="dba75-194">**CSS**</span><span class="sxs-lookup"><span data-stu-id="dba75-194">**css**</span></span>
  * <span data-ttu-id="dba75-195">**images**</span><span class="sxs-lookup"><span data-stu-id="dba75-195">**images**</span></span>
  * <span data-ttu-id="dba75-196">**js**</span><span class="sxs-lookup"><span data-stu-id="dba75-196">**js**</span></span>
* <span data-ttu-id="dba75-197">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="dba75-197">**MyStaticFiles**</span></span>
  * <span data-ttu-id="dba75-198">**images**</span><span class="sxs-lookup"><span data-stu-id="dba75-198">**images**</span></span>
      * <span data-ttu-id="dba75-199">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="dba75-199">*banner1.svg*</span></span>
  * <span data-ttu-id="dba75-200">*default.html*</span><span class="sxs-lookup"><span data-stu-id="dba75-200">*default.html*</span></span>

<span data-ttu-id="dba75-201">Poniższy kod umożliwia pliki statyczne, domyślne pliki i przeglądanie katalogów z `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="dba75-201">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="dba75-202">`AddDirectoryBrowser`musi być wywoływane, gdy `EnableDirectoryBrowsing` wartość właściwości jest `true`:</span><span class="sxs-lookup"><span data-stu-id="dba75-202">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="dba75-203">Za pomocą hierarchii plików i poprzedzających kodu, adresy URL rozwiązania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dba75-203">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="dba75-204">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="dba75-204">URI</span></span>            |                             <span data-ttu-id="dba75-205">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="dba75-205">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="dba75-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="dba75-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="dba75-207">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="dba75-207">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="dba75-208">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="dba75-208">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="dba75-209">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="dba75-209">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="dba75-210">Jeśli nie o nazwie domyślnej istnieje plik w *MyStaticFiles* katalogu, *http://\<server_address > / StaticFiles* zwraca listę z aktywne katalogów:</span><span class="sxs-lookup"><span data-stu-id="dba75-210">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Lista plików statycznych](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="dba75-212">`UseDefaultFiles`i `UseDirectoryBrowser` Użyj adresu URL *http://\<server_address > / StaticFiles* bez ukośników do wyzwolenia klienta przekierowania do *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="dba75-212">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="dba75-213">Zwróć uwagę, dodanie wiodący ukośnik.</span><span class="sxs-lookup"><span data-stu-id="dba75-213">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="dba75-214">Względnych adresów URL w dokumentach jest uznany za nieprawidłowy bez ukośnika.</span><span class="sxs-lookup"><span data-stu-id="dba75-214">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="dba75-215">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="dba75-215">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="dba75-216">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) klasa zawiera `Mappings` właściwości służy jako mapowanie rozszerzenia plików do typu MIME.</span><span class="sxs-lookup"><span data-stu-id="dba75-216">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="dba75-217">W poniższym przykładzie kilka rozszerzeń plików jest zarejestrowany na znanych typów MIME.</span><span class="sxs-lookup"><span data-stu-id="dba75-217">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="dba75-218">*.Rtf* rozszerzenia są zastępowane, oraz *plik MP4* zostanie usunięta.</span><span class="sxs-lookup"><span data-stu-id="dba75-218">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="dba75-219">Zobacz [typu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="dba75-219">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="dba75-220">Niestandardowe typy zawartości</span><span class="sxs-lookup"><span data-stu-id="dba75-220">Non-standard content types</span></span>

<span data-ttu-id="dba75-221">Oprogramowanie pośredniczące plików statycznych rozumie prawie 400 znanych typów zawartości.</span><span class="sxs-lookup"><span data-stu-id="dba75-221">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="dba75-222">Jeśli użytkownik żąda pliku nieznany typ pliku, oprogramowanie pośredniczące plików statycznych zwraca odpowiedź HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="dba75-222">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="dba75-223">Jeśli przeglądanie katalogów jest włączona, zostanie wyświetlony link do pliku.</span><span class="sxs-lookup"><span data-stu-id="dba75-223">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="dba75-224">Identyfikator URI zwraca błąd HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="dba75-224">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="dba75-225">Poniższy kod umożliwia obsługująca nieznanych typów i renderuje nieznany pliku jako obrazu:</span><span class="sxs-lookup"><span data-stu-id="dba75-225">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="dba75-226">Z poprzednim kodzie żądanie dla pliku z nieznanego typu zawartości jest zwracana jako obraz.</span><span class="sxs-lookup"><span data-stu-id="dba75-226">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="dba75-227">Włączanie [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="dba75-227">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="dba75-228">Jest domyślnie wyłączona, a jego użycie nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="dba75-228">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="dba75-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) bezpieczniejsze alternatywą do obsługujących pliki z rozszerzeniami niestandardowym.</span><span class="sxs-lookup"><span data-stu-id="dba75-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="dba75-230">Uwagi</span><span class="sxs-lookup"><span data-stu-id="dba75-230">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="dba75-231">`UseDirectoryBrowser`i `UseStaticFiles` można wyciek kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="dba75-231">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="dba75-232">Zdecydowanie zaleca się wyłączenie przeglądanie katalogów w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="dba75-232">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="dba75-233">Uważnie przejrzyj katalogi, które są włączone za pośrednictwem `UseStaticFiles` lub `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="dba75-233">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="dba75-234">Całego katalogu i jego podkatalogów stać się publicznie.</span><span class="sxs-lookup"><span data-stu-id="dba75-234">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="dba75-235">Magazyn plików odpowiednie do użycia w publicznie dedykowanej katalog, takich jak  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="dba75-235">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="dba75-236">Te pliki należy oddzielić od widoków MVC, stron Razor (tylko 2.x), pliki konfiguracji itp.</span><span class="sxs-lookup"><span data-stu-id="dba75-236">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="dba75-237">Adresy URL zawartość jest uwidaczniana z `UseDirectoryBrowser` i `UseStaticFiles` uwzględniana wielkość liter i znaków ograniczenia źródłowy system plików.</span><span class="sxs-lookup"><span data-stu-id="dba75-237">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="dba75-238">Na przykład systemu Windows nie uwzględnia wielkości liter&mdash;nie Mac i Linux.</span><span class="sxs-lookup"><span data-stu-id="dba75-238">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="dba75-239">Aplikacji platformy ASP.NET Core hostowanych w użyciu IIS [platformy ASP.NET Core modułu (ANCM)](xref:fundamentals/servers/aspnet-core-module) do przekazywania wszystkich żądań do aplikacji, włącznie z żądaniami plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="dba75-239">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="dba75-240">Obsługa plików statycznych usług IIS nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="dba75-240">The IIS static file handler isn't used.</span></span> <span data-ttu-id="dba75-241">Nie ma możliwość obsługi żądań przed są one obsługiwane przez ANCM.</span><span class="sxs-lookup"><span data-stu-id="dba75-241">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="dba75-242">Wykonaj następujące kroki w Menedżerze usług IIS do obsługi plików statycznych usług IIS na poziomie serwera lub witryny sieci Web do usunięcia:</span><span class="sxs-lookup"><span data-stu-id="dba75-242">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="dba75-243">Przejdź do **modułów** funkcji.</span><span class="sxs-lookup"><span data-stu-id="dba75-243">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="dba75-244">Wybierz **StaticFileModule** na liście.</span><span class="sxs-lookup"><span data-stu-id="dba75-244">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="dba75-245">Kliknij przycisk **Usuń** w **akcje** paska bocznego.</span><span class="sxs-lookup"><span data-stu-id="dba75-245">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="dba75-246">Jeśli jest włączona obsługa plików statycznych IIS **i** ANCM jest niepoprawnie skonfigurowana, pliki statyczne są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="dba75-246">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="dba75-247">Dzieje się tak, na przykład, jeśli *web.config* plik nie jest wdrożony.</span><span class="sxs-lookup"><span data-stu-id="dba75-247">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="dba75-248">Umieść kod plików (w tym *.cs* i *.cshtml*) poza katalogiem głównym projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="dba75-248">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="dba75-249">W związku z tym utworzeniu separacji logicznej między zawartości po stronie klienta i kodu na serwerze aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dba75-249">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="dba75-250">Zapobiega to wycieku kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="dba75-250">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dba75-251">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="dba75-251">Additional resources</span></span>

* [<span data-ttu-id="dba75-252">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="dba75-252">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="dba75-253">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dba75-253">Introduction to ASP.NET Core</span></span>](xref:index)