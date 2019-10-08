---
title: Pliki statyczne w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak udostępniać i zabezpieczać pliki statyczne oraz skonfigurować zachowania oprogramowania pośredniczącego do obsługi plików statycznych w aplikacji internetowej ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/static-files
ms.openlocfilehash: 2f153551a86860616469200862723528e4a8cc1c
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007329"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="c7be2-103">Pliki statyczne w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="c7be2-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="c7be2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="c7be2-105">Pliki statyczne, takie jak HTML, CSS, images i JavaScript, są zasobami, które aplikacja ASP.NET Core może bezpośrednio obsługiwać klientów.</span><span class="sxs-lookup"><span data-stu-id="c7be2-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="c7be2-106">Aby umożliwić obsługę tych plików, wymagana jest pewna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="c7be2-106">Some configuration is required to enable serving of these files.</span></span>

<span data-ttu-id="c7be2-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7be2-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="c7be2-108">Obsługuj pliki statyczne</span><span class="sxs-lookup"><span data-stu-id="c7be2-108">Serve static files</span></span>

<span data-ttu-id="c7be2-109">Pliki statyczne są przechowywane w katalogu [głównym sieci Web](xref:fundamentals/index#web-root) projektu.</span><span class="sxs-lookup"><span data-stu-id="c7be2-109">Static files are stored within the project's [web root](xref:fundamentals/index#web-root) directory.</span></span> <span data-ttu-id="c7be2-110">Domyślnym katalogiem jest *{Content root}/wwwroot*, ale można go zmienić za pomocą metody [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) .</span><span class="sxs-lookup"><span data-stu-id="c7be2-110">The default directory is *{content root}/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="c7be2-111">Aby uzyskać więcej informacji, zobacz [katalog główny zawartości](xref:fundamentals/index#content-root) i [katalog główny sieci Web](xref:fundamentals/index#web-root) .</span><span class="sxs-lookup"><span data-stu-id="c7be2-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="c7be2-112">Host sieci Web aplikacji musi być świadomy katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="c7be2-112">The app's web host must be made aware of the content root directory.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c7be2-113">Metoda `WebHost.CreateDefaultBuilder` ustawia katalog główny zawartości dla bieżącego katalogu:</span><span class="sxs-lookup"><span data-stu-id="c7be2-113">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c7be2-114">Ustaw katalog główny zawartości w bieżącym katalogu, wywołując [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) wewnątrz `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-114">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

<span data-ttu-id="c7be2-115">Pliki statyczne są dostępne za pośrednictwem ścieżki względnej dla [katalogu głównego sieci Web](xref:fundamentals/index#web-root).</span><span class="sxs-lookup"><span data-stu-id="c7be2-115">Static files are accessible via a path relative to the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="c7be2-116">Na przykład szablon projektu **aplikacji sieci Web** zawiera kilka folderów w folderze *wwwroot* :</span><span class="sxs-lookup"><span data-stu-id="c7be2-116">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="c7be2-117">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c7be2-117">**wwwroot**</span></span>
  * <span data-ttu-id="c7be2-118">**kaskad**</span><span class="sxs-lookup"><span data-stu-id="c7be2-118">**css**</span></span>
  * <span data-ttu-id="c7be2-119">**images**</span><span class="sxs-lookup"><span data-stu-id="c7be2-119">**images**</span></span>
  * <span data-ttu-id="c7be2-120">**js**</span><span class="sxs-lookup"><span data-stu-id="c7be2-120">**js**</span></span>

<span data-ttu-id="c7be2-121">Format identyfikatora URI służący do uzyskiwania dostępu do pliku w podfolderze *images* to *http://\<server_address >/images/\<image_file_name >* .</span><span class="sxs-lookup"><span data-stu-id="c7be2-121">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="c7be2-122">Na przykład *http://localhost:9189/images/banner3.svg* .</span><span class="sxs-lookup"><span data-stu-id="c7be2-122">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c7be2-123">Jeśli .NET Framework określania wartości docelowej, Dodaj pakiet [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="c7be2-123">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="c7be2-124">W przypadku określania platformy .NET Core [pakiet Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) zawiera ten pakiet.</span><span class="sxs-lookup"><span data-stu-id="c7be2-124">If targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) includes this package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c7be2-125">Jeśli .NET Framework określania wartości docelowej, Dodaj pakiet [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="c7be2-125">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="c7be2-126">W przypadku określania platformy .NET Core pakiet [Microsoft. AspNetCore. All](xref:fundamentals/metapackage) zawiera ten pakiet.</span><span class="sxs-lookup"><span data-stu-id="c7be2-126">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c7be2-127">Dodaj pakiet [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="c7be2-127">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span>

::: moniker-end

<span data-ttu-id="c7be2-128">Skonfiguruj [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) , które umożliwia obsługę plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="c7be2-128">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="c7be2-129">Obsługuj pliki wewnątrz katalogu głównego sieci Web</span><span class="sxs-lookup"><span data-stu-id="c7be2-129">Serve files inside of web root</span></span>

<span data-ttu-id="c7be2-130">Wywołaj metodę [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-130">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="c7be2-131">Bezparametrowe przeciążanie metody `UseStaticFiles` oznacza pliki w [katalogu głównym sieci Web](xref:fundamentals/index#web-root) jako z możliwością.</span><span class="sxs-lookup"><span data-stu-id="c7be2-131">The parameterless `UseStaticFiles` method overload marks the files in [web root](xref:fundamentals/index#web-root) as servable.</span></span> <span data-ttu-id="c7be2-132">Następujące znaczniki odwołują się do *wwwroot/images/banner1. SVG*:</span><span class="sxs-lookup"><span data-stu-id="c7be2-132">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

<span data-ttu-id="c7be2-133">W poprzednim kodzie znak tyldy `~/` wskazuje na [katalog główny sieci Web](xref:fundamentals/index#web-root).</span><span class="sxs-lookup"><span data-stu-id="c7be2-133">In the preceding code, the tilde character `~/` points to the [web root](xref:fundamentals/index#web-root).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="c7be2-134">Obsługiwanie plików poza katalogiem głównym sieci Web</span><span class="sxs-lookup"><span data-stu-id="c7be2-134">Serve files outside of web root</span></span>

<span data-ttu-id="c7be2-135">Rozważ hierarchię katalogów, w której pliki statyczne mają być obsługiwane poza [katalogiem głównym sieci Web](xref:fundamentals/index#web-root):</span><span class="sxs-lookup"><span data-stu-id="c7be2-135">Consider a directory hierarchy in which the static files to be served reside outside of the [web root](xref:fundamentals/index#web-root):</span></span>

* <span data-ttu-id="c7be2-136">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c7be2-136">**wwwroot**</span></span>
  * <span data-ttu-id="c7be2-137">**kaskad**</span><span class="sxs-lookup"><span data-stu-id="c7be2-137">**css**</span></span>
  * <span data-ttu-id="c7be2-138">**images**</span><span class="sxs-lookup"><span data-stu-id="c7be2-138">**images**</span></span>
  * <span data-ttu-id="c7be2-139">**js**</span><span class="sxs-lookup"><span data-stu-id="c7be2-139">**js**</span></span>
* <span data-ttu-id="c7be2-140">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="c7be2-140">**MyStaticFiles**</span></span>
  * <span data-ttu-id="c7be2-141">**images**</span><span class="sxs-lookup"><span data-stu-id="c7be2-141">**images**</span></span>
    * <span data-ttu-id="c7be2-142">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="c7be2-142">*banner1.svg*</span></span>

<span data-ttu-id="c7be2-143">Żądanie może uzyskać dostęp do pliku *banner1. SVG* przez skonfigurowanie pliku statycznego pośredniczącego w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c7be2-143">A request can access the *banner1.svg* file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="c7be2-144">W powyższym kodzie hierarchia katalogów *MyStaticFiles* jest udostępniana publicznie za pośrednictwem segmentu identyfikatora URI *StaticFiles* .</span><span class="sxs-lookup"><span data-stu-id="c7be2-144">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="c7be2-145">Żądanie do *http://\<server_address >/StaticFiles/images/banner1.SVG* obsługuje plik *banner1. SVG* .</span><span class="sxs-lookup"><span data-stu-id="c7be2-145">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="c7be2-146">Następujące znaczniki odwołują się do *MyStaticFiles/images/banner1. SVG*:</span><span class="sxs-lookup"><span data-stu-id="c7be2-146">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="c7be2-147">Ustawianie nagłówków odpowiedzi HTTP</span><span class="sxs-lookup"><span data-stu-id="c7be2-147">Set HTTP response headers</span></span>

<span data-ttu-id="c7be2-148">Obiekt [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) może służyć do ustawiania nagłówków odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7be2-148">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="c7be2-149">Oprócz konfigurowania pliku statycznego z poziomu [katalogu głównego sieci Web](xref:fundamentals/index#web-root), poniższy kod ustawia nagłówek `Cache-Control`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-149">In addition to configuring static file serving from the [web root](xref:fundamentals/index#web-root), the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="c7be2-150">Metoda [HeaderDictionaryExtensions. Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) istnieje w pakiecie [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) .</span><span class="sxs-lookup"><span data-stu-id="c7be2-150">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="c7be2-151">Pliki zostały publicznie przetworzone w pamięci podręcznej przez 10 minut (600 sekund) w środowisku deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="c7be2-151">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![Nagłówki odpowiedzi pokazujące nagłówek Cache-Control został dodany](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="c7be2-153">Autoryzacja pliku statycznego</span><span class="sxs-lookup"><span data-stu-id="c7be2-153">Static file authorization</span></span>

<span data-ttu-id="c7be2-154">Oprogramowanie pośredniczące plików statycznych nie zapewnia kontroli autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c7be2-154">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="c7be2-155">Wszystkie pliki obsługiwane przez ten program, w tym w katalogu *wwwroot*, są publicznie dostępne.</span><span class="sxs-lookup"><span data-stu-id="c7be2-155">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="c7be2-156">Aby obpracować pliki na podstawie autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="c7be2-156">To serve files based on authorization:</span></span>

* <span data-ttu-id="c7be2-157">Przechowaj je poza katalogiem *wwwroot* i dowolnym katalogu dostępnym dla oprogramowania pośredniczącego plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="c7be2-157">Store them outside of *wwwroot* and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="c7be2-158">Obsłużyć je za pomocą metody akcji, do której zastosowano autoryzację.</span><span class="sxs-lookup"><span data-stu-id="c7be2-158">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="c7be2-159">Zwróć obiekt [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) :</span><span class="sxs-lookup"><span data-stu-id="c7be2-159">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="c7be2-160">Włącz przeglądanie katalogów</span><span class="sxs-lookup"><span data-stu-id="c7be2-160">Enable directory browsing</span></span>

<span data-ttu-id="c7be2-161">Przeglądanie katalogów umożliwia użytkownikom aplikacji sieci Web Wyświetlanie listy katalogów i plików w określonym katalogu.</span><span class="sxs-lookup"><span data-stu-id="c7be2-161">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="c7be2-162">Przeglądanie katalogów jest domyślnie wyłączone ze względów bezpieczeństwa (zobacz [uwagi](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="c7be2-162">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="c7be2-163">Włącz przeglądanie katalogów przez wywołanie metody [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-163">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="c7be2-164">Dodaj wymagane usługi, wywołując metodę [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-164">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="c7be2-165">Poprzedni kod umożliwia przeglądanie katalogów folderu *wwwroot/images* przy użyciu adresu URL *http://\<server_address >/myimages*, z linkami do każdego pliku i folderu:</span><span class="sxs-lookup"><span data-stu-id="c7be2-165">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

<span data-ttu-id="c7be2-167">Zapoznaj się z [uwagami](#considerations) dotyczącymi zagrożeń bezpieczeństwa podczas włączania przeglądania.</span><span class="sxs-lookup"><span data-stu-id="c7be2-167">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="c7be2-168">Zwróć uwagę na dwa wywołania `UseStaticFiles` w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c7be2-168">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="c7be2-169">Pierwsze wywołanie umożliwia obsługę plików statycznych w folderze *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="c7be2-169">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="c7be2-170">Drugie wywołanie umożliwia przeglądanie katalogów folderu *wwwroot/images* przy użyciu adresu URL *http://\<server_address >/myimages*:</span><span class="sxs-lookup"><span data-stu-id="c7be2-170">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="c7be2-171">Obsługuj dokument domyślny</span><span class="sxs-lookup"><span data-stu-id="c7be2-171">Serve a default document</span></span>

<span data-ttu-id="c7be2-172">Ustawienie domyślnej strony głównej zapewnia odwiedzającym logiczny punkt wyjścia podczas odwiedzania witryny.</span><span class="sxs-lookup"><span data-stu-id="c7be2-172">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="c7be2-173">Aby obtworzyć stronę domyślną bez zakwalifikowania identyfikatora URI użytkownika, wywołaj metodę [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-173">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="c7be2-174">`UseDefaultFiles` musi zostać wywołana przed `UseStaticFiles`, aby można było obmieścić plik domyślny.</span><span class="sxs-lookup"><span data-stu-id="c7be2-174">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="c7be2-175">`UseDefaultFiles` to obiekt zapisujący adresu URL, który faktycznie nie obsługuje pliku.</span><span class="sxs-lookup"><span data-stu-id="c7be2-175">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="c7be2-176">Włącz oprogramowanie pośredniczące plików statycznych za pośrednictwem `UseStaticFiles`, aby obpracować plik.</span><span class="sxs-lookup"><span data-stu-id="c7be2-176">Enable Static File Middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="c7be2-177">W przypadku `UseDefaultFiles` żądania kierowane do folderu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="c7be2-177">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="c7be2-178">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="c7be2-178">*default.htm*</span></span>
* <span data-ttu-id="c7be2-179">*default.html*</span><span class="sxs-lookup"><span data-stu-id="c7be2-179">*default.html*</span></span>
* <span data-ttu-id="c7be2-180">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="c7be2-180">*index.htm*</span></span>
* <span data-ttu-id="c7be2-181">*index.html*</span><span class="sxs-lookup"><span data-stu-id="c7be2-181">*index.html*</span></span>

<span data-ttu-id="c7be2-182">Pierwszy plik znaleziony z listy jest obsługiwany tak, jakby żądanie było w pełni kwalifikowanym identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="c7be2-182">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="c7be2-183">Adres URL przeglądarki nadal odzwierciedla żądany identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="c7be2-183">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="c7be2-184">Poniższy kod zmienia domyślną nazwę pliku na *default. html*:</span><span class="sxs-lookup"><span data-stu-id="c7be2-184">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="c7be2-185">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="c7be2-185">UseFileServer</span></span>

<span data-ttu-id="c7be2-186">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) łączy funkcje `UseStaticFiles`, `UseDefaultFiles` i `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="c7be2-186">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="c7be2-187">Poniższy kod umożliwia obsługę plików statycznych i pliku domyślnego.</span><span class="sxs-lookup"><span data-stu-id="c7be2-187">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="c7be2-188">Przeglądanie katalogów nie jest włączone.</span><span class="sxs-lookup"><span data-stu-id="c7be2-188">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="c7be2-189">Poniższy kod kompiluje na Przeciążenie bez parametrów przez włączenie przeglądania katalogów:</span><span class="sxs-lookup"><span data-stu-id="c7be2-189">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="c7be2-190">Weź pod uwagę następującą hierarchię katalogów:</span><span class="sxs-lookup"><span data-stu-id="c7be2-190">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="c7be2-191">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c7be2-191">**wwwroot**</span></span>
  * <span data-ttu-id="c7be2-192">**kaskad**</span><span class="sxs-lookup"><span data-stu-id="c7be2-192">**css**</span></span>
  * <span data-ttu-id="c7be2-193">**images**</span><span class="sxs-lookup"><span data-stu-id="c7be2-193">**images**</span></span>
  * <span data-ttu-id="c7be2-194">**js**</span><span class="sxs-lookup"><span data-stu-id="c7be2-194">**js**</span></span>
* <span data-ttu-id="c7be2-195">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="c7be2-195">**MyStaticFiles**</span></span>
  * <span data-ttu-id="c7be2-196">**images**</span><span class="sxs-lookup"><span data-stu-id="c7be2-196">**images**</span></span>
    * <span data-ttu-id="c7be2-197">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="c7be2-197">*banner1.svg*</span></span>
  * <span data-ttu-id="c7be2-198">*default.html*</span><span class="sxs-lookup"><span data-stu-id="c7be2-198">*default.html*</span></span>

<span data-ttu-id="c7be2-199">Poniższy kod umożliwia włączenie plików statycznych, plików domyślnych i przeglądanie katalogów `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-199">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="c7be2-200">`AddDirectoryBrowser` musi być wywoływana, gdy wartość właściwości `EnableDirectoryBrowsing` jest `true`:</span><span class="sxs-lookup"><span data-stu-id="c7be2-200">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="c7be2-201">Przy użyciu hierarchii plików i poprzedniego kodu adresy URL są rozpoznawane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c7be2-201">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="c7be2-202">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="c7be2-202">URI</span></span>            |                             <span data-ttu-id="c7be2-203">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="c7be2-203">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="c7be2-204">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="c7be2-204">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="c7be2-205">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="c7be2-205">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="c7be2-206">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="c7be2-206">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="c7be2-207">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="c7be2-207">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="c7be2-208">Jeśli w katalogu *MyStaticFiles* nie istnieje plik o nazwie default, *http://\<server_address >/StaticFiles* zwraca listę katalogów z linkami, które można kliknąć:</span><span class="sxs-lookup"><span data-stu-id="c7be2-208">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Lista plików statycznych](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="c7be2-210"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> i <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> przekierować po stronie klienta z `http://{SERVER ADDRESS}/StaticFiles` (bez końcowego ukośnika) do `http://{SERVER ADDRESS}/StaticFiles/` (z końcowym ukośnikiem).</span><span class="sxs-lookup"><span data-stu-id="c7be2-210"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> and <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> perform a client-side redirect from `http://{SERVER ADDRESS}/StaticFiles` (without a trailing slash) to `http://{SERVER ADDRESS}/StaticFiles/` (with a trailing slash).</span></span> <span data-ttu-id="c7be2-211">Względne adresy URL w katalogu *StaticFiles* są nieprawidłowe bez końcowej kreski ułamkowej.</span><span class="sxs-lookup"><span data-stu-id="c7be2-211">Relative URLs within the *StaticFiles* directory are invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="c7be2-212">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="c7be2-212">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="c7be2-213">Klasa [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) zawiera właściwość `Mappings` służącą jako mapowanie rozszerzeń plików na typy zawartości MIME.</span><span class="sxs-lookup"><span data-stu-id="c7be2-213">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="c7be2-214">W poniższym przykładzie kilka rozszerzeń plików są zarejestrowane na znanych typach MIME.</span><span class="sxs-lookup"><span data-stu-id="c7be2-214">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="c7be2-215">Rozszerzenie *. rtf* jest zastępowane, a plik *MP4* jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="c7be2-215">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="c7be2-216">Zobacz [typy zawartości MIME](https://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="c7be2-216">See [MIME content types](https://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="c7be2-217">Niestandardowe typy zawartości</span><span class="sxs-lookup"><span data-stu-id="c7be2-217">Non-standard content types</span></span>

<span data-ttu-id="c7be2-218">Oprogramowanie pośredniczące plików static rozpoznaje niemal 400 znanych typów zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="c7be2-218">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="c7be2-219">Jeśli użytkownik zażąda pliku z nieznanym typem pliku, oprogramowanie pośredniczące plików przesyła żądanie do następnego oprogramowania pośredniczącego w potoku.</span><span class="sxs-lookup"><span data-stu-id="c7be2-219">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="c7be2-220">Jeśli żadne oprogramowanie pośredniczące nie obsługuje żądania, zwracana jest odpowiedź *404* .</span><span class="sxs-lookup"><span data-stu-id="c7be2-220">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="c7be2-221">Jeśli przeglądanie katalogów jest włączone, link do pliku zostanie wyświetlony na liście katalogów.</span><span class="sxs-lookup"><span data-stu-id="c7be2-221">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="c7be2-222">Poniższy kod umożliwia obsługę nieznanych typów i renderuje nieznany plik jako obraz:</span><span class="sxs-lookup"><span data-stu-id="c7be2-222">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="c7be2-223">W powyższym kodzie żądanie dotyczące pliku z nieznanym typem zawartości jest zwracane jako obraz.</span><span class="sxs-lookup"><span data-stu-id="c7be2-223">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="c7be2-224">Włączenie [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="c7be2-224">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="c7be2-225">Jest on domyślnie wyłączony i nie jest zalecane jego użycie.</span><span class="sxs-lookup"><span data-stu-id="c7be2-225">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="c7be2-226">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) zapewnia bezpieczniejsze rozwiązanie do obsługi plików z rozszerzeniami niestandardowymi.</span><span class="sxs-lookup"><span data-stu-id="c7be2-226">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="c7be2-227">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="c7be2-227">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="c7be2-228">`UseDirectoryBrowser` i `UseStaticFiles` mogą wyciekać klucze tajne.</span><span class="sxs-lookup"><span data-stu-id="c7be2-228">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="c7be2-229">Wyłączenie przeglądania katalogów w środowisku produkcyjnym jest zdecydowanie zalecane.</span><span class="sxs-lookup"><span data-stu-id="c7be2-229">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="c7be2-230">Uważnie Sprawdź, które katalogi są włączone za pośrednictwem `UseStaticFiles` lub `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="c7be2-230">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="c7be2-231">Cały katalog i jego katalogi podrzędne stają się publicznie dostępne.</span><span class="sxs-lookup"><span data-stu-id="c7be2-231">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="c7be2-232">Pliki magazynu odpowiednie do obsłużenia publicznie w dedykowanym katalogu, takie jak *\<content_root >/wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="c7be2-232">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="c7be2-233">Oddziel te pliki od widoków MVC, Razor Pages (tylko 2. x), plików konfiguracji itp.</span><span class="sxs-lookup"><span data-stu-id="c7be2-233">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="c7be2-234">Adresy URL dla zawartości uwidocznionej za pomocą `UseDirectoryBrowser` i `UseStaticFiles` podlegają uwzględnieniu wielkości liter i ograniczeniach znaków w źródłowym systemie plików.</span><span class="sxs-lookup"><span data-stu-id="c7be2-234">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="c7be2-235">Na przykład w systemie Windows nie jest rozróżniana wielkość liter, no__t-0macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="c7be2-235">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="c7be2-236">ASP.NET Core aplikacje hostowane w usługach IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) do przesyłania dalej wszystkich żądań do aplikacji, w tym żądań plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="c7be2-236">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="c7be2-237">Procedura obsługi pliku statycznego usług IIS nie jest używana.</span><span class="sxs-lookup"><span data-stu-id="c7be2-237">The IIS static file handler isn't used.</span></span> <span data-ttu-id="c7be2-238">Nie ma możliwości obsługi żądań, zanim są one obsługiwane przez moduł.</span><span class="sxs-lookup"><span data-stu-id="c7be2-238">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="c7be2-239">Wykonaj następujące kroki w Menedżerze usług IIS, aby usunąć program obsługi plików statycznych usług IIS na poziomie serwera lub witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="c7be2-239">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="c7be2-240">Przejdź do funkcji **moduły** .</span><span class="sxs-lookup"><span data-stu-id="c7be2-240">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="c7be2-241">Na liście wybierz pozycję **StaticFileModule** .</span><span class="sxs-lookup"><span data-stu-id="c7be2-241">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="c7be2-242">Kliknij przycisk **Usuń** na pasku bocznym **Akcje** .</span><span class="sxs-lookup"><span data-stu-id="c7be2-242">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="c7be2-243">Jeśli program obsługi plików statycznych usług IIS jest włączony **i** moduł ASP.NET Core jest niepoprawnie skonfigurowany, obsługiwane są pliki statyczne.</span><span class="sxs-lookup"><span data-stu-id="c7be2-243">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="c7be2-244">Dzieje się tak na przykład wtedy, gdy plik *Web. config* nie jest wdrożony.</span><span class="sxs-lookup"><span data-stu-id="c7be2-244">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="c7be2-245">Umieść pliki kodu (w tym *. cs* i *. cshtml*) poza [katalogiem głównym sieci Web](xref:fundamentals/index#web-root)projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7be2-245">Place code files (including *.cs* and *.cshtml*) outside of the app project's [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="c7be2-246">W związku z tym tworzone jest podział logiczny między zawartością po stronie klienta a kodem opartym na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c7be2-246">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="c7be2-247">Zapobiega to wyciekowi kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="c7be2-247">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7be2-248">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c7be2-248">Additional resources</span></span>

* [<span data-ttu-id="c7be2-249">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="c7be2-249">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="c7be2-250">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7be2-250">Introduction to ASP.NET Core</span></span>](xref:index)
