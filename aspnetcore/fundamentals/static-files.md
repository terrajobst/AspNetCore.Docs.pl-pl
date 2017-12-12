---
title: "Praca z plików statycznych w ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak pracować z plików statycznych w ASP.NET Core."
keywords: Platformy ASP.NET Core, pliki statyczne, zasoby statyczne, HTML, CSS, JavaScript
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="4efff-104">Praca z plików statycznych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4efff-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="4efff-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4efff-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4efff-106">Pliki statyczne, takich jak HTML, CSS, obrazu i JavaScript, są zasoby, które aplikacji platformy ASP.NET Core mogą służyć bezpośrednio do klientów.</span><span class="sxs-lookup"><span data-stu-id="4efff-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="4efff-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4efff-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="4efff-108">Dostarczanie plików statycznych</span><span class="sxs-lookup"><span data-stu-id="4efff-108">Serving static files</span></span>

<span data-ttu-id="4efff-109">Pliki statyczne są zazwyczaj znajduje się w `web root` (*\<zawartości główny > / wwwroot*) folderu.</span><span class="sxs-lookup"><span data-stu-id="4efff-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="4efff-110">Zobacz [zawartości głównego](xref:fundamentals/index#content-root) i [głównego sieci Web](xref:fundamentals/index#web-root) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4efff-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="4efff-111">Zwykle ustawić zawartości głównego jako bieżący katalog, aby projektu `web root` zostaną znalezione w rozwoju.</span><span class="sxs-lookup"><span data-stu-id="4efff-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="4efff-112">Pliki statyczne mogą być przechowywane w dowolnym folderze `web root` i uzyskanie dostępu do ścieżki względnej z elementem głównym.</span><span class="sxs-lookup"><span data-stu-id="4efff-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="4efff-113">Na przykład podczas tworzenia domyślny projekt aplikacji sieci Web przy użyciu programu Visual Studio istnieje kilka folderami utworzonych w ramach *wwwroot* folderu - *css*, *obrazów*, i *js*.</span><span class="sxs-lookup"><span data-stu-id="4efff-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="4efff-114">Identyfikator URI uzyskać dostępu do obrazu w *obrazów* podfolderów:</span><span class="sxs-lookup"><span data-stu-id="4efff-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="4efff-115">Aby pliki statyczne z zabezpieczeniami, należy skonfigurować [oprogramowanie pośredniczące](middleware.md) Aby dodać pliki statyczne z potokiem.</span><span class="sxs-lookup"><span data-stu-id="4efff-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="4efff-116">Oprogramowanie pośredniczące plików statycznych można skonfigurować przez dodanie zależności na *Microsoft.AspNetCore.StaticFiles* pakietu do projektu, a następnie wywołania `UseStaticFiles` — metoda rozszerzenia z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4efff-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="4efff-117">`app.UseStaticFiles();`powoduje, że pliki w `web root` (*wwwroot* domyślnie) servable.</span><span class="sxs-lookup"><span data-stu-id="4efff-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="4efff-118">Później zostanie omówiony sposób wprowadzania servable z innych zawartość katalogu `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="4efff-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="4efff-119">Musi zawierać pakiet NuGet "Microsoft.AspNetCore.StaticFiles".</span><span class="sxs-lookup"><span data-stu-id="4efff-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="4efff-120">`web root`Domyślnie *wwwroot* katalogu, ale można ustawić `web root` katalogu z `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="4efff-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="4efff-121">Załóżmy, że masz hierarchii projektu, w którym chcesz obsługiwać pliki statyczne są poza `web root`.</span><span class="sxs-lookup"><span data-stu-id="4efff-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="4efff-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4efff-122">For example:</span></span>

* <span data-ttu-id="4efff-123">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="4efff-123">wwwroot</span></span>
  * <span data-ttu-id="4efff-124">CSS</span><span class="sxs-lookup"><span data-stu-id="4efff-124">css</span></span>
  * <span data-ttu-id="4efff-125">obrazy</span><span class="sxs-lookup"><span data-stu-id="4efff-125">images</span></span>
  * <span data-ttu-id="4efff-126">...</span><span class="sxs-lookup"><span data-stu-id="4efff-126">...</span></span>
* <span data-ttu-id="4efff-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="4efff-127">MyStaticFiles</span></span>
  * <span data-ttu-id="4efff-128">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="4efff-128">test.png</span></span>

<span data-ttu-id="4efff-129">Dla żądania dostępu do *test.png*, skonfiguruj oprogramowanie pośredniczące plików statycznych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4efff-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="4efff-130">Żądanie `http://<app>/StaticFiles/test.png` posłuży *test.png* pliku.</span><span class="sxs-lookup"><span data-stu-id="4efff-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="4efff-131">`StaticFileOptions()`można ustawić nagłówków odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4efff-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="4efff-132">Na przykład poniższy kod konfiguruje dostarczanie z plików statycznych *wwwroot* folderu i zestawy `Cache-Control` nagłówka, aby były publicznie buforowalnej przez 10 minut (600 sekund):</span><span class="sxs-lookup"><span data-stu-id="4efff-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

<span data-ttu-id="4efff-133">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metoda jest dostępna z [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4efff-133">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method is available from the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span> <span data-ttu-id="4efff-134">Dodaj `using Microsoft.AspNetCore.Http;` do Twojej *csharp* plik, jeśli metoda jest niedostępny.</span><span class="sxs-lookup"><span data-stu-id="4efff-134">Add `using Microsoft.AspNetCore.Http;` to your *csharp* file if the method is unavailable.</span></span>

![Dodano przedstawiający nagłówek Cache-Control nagłówki odpowiedzi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="4efff-136">Plik statyczny autoryzacji</span><span class="sxs-lookup"><span data-stu-id="4efff-136">Static file authorization</span></span>

<span data-ttu-id="4efff-137">Udostępnia moduł plików statycznych **nie** sprawdzeń autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4efff-137">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="4efff-138">Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot* publicznie dostępne.</span><span class="sxs-lookup"><span data-stu-id="4efff-138">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="4efff-139">Do obsługi plików na podstawie autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="4efff-139">To serve files based on authorization:</span></span>

* <span data-ttu-id="4efff-140">Przechowywania ich poza *wwwroot* i dowolnego katalogu, które są dostępne dla oprogramowania pośredniczącego plików statycznych **i**</span><span class="sxs-lookup"><span data-stu-id="4efff-140">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="4efff-141">Obsługujących je za pomocą akcji kontrolera, zwracając `FileResult` których stosowane jest autoryzacji</span><span class="sxs-lookup"><span data-stu-id="4efff-141">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="4efff-142">Włączanie przeglądania katalogów</span><span class="sxs-lookup"><span data-stu-id="4efff-142">Enabling directory browsing</span></span>

<span data-ttu-id="4efff-143">Przeglądanie katalogów umożliwia użytkownikowi wyświetlenie listy katalogów i plików w określonym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4efff-143">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="4efff-144">Przeglądanie katalogów jest domyślnie wyłączona, ze względów bezpieczeństwa (zobacz [zagadnienia](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="4efff-144">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="4efff-145">Aby umożliwić przeglądanie katalogów, należy wywołać `UseDirectoryBrowser` — metoda rozszerzenia z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4efff-145">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="4efff-146">I Dodaj wymagane usługi, wywołując `AddDirectoryBrowser` — metoda rozszerzenia z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4efff-146">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="4efff-147">Powyższy kod umożliwia przeglądanie katalogów z *wwwroot/obrazy* folder przy użyciu adresu URL http://\<aplikacji > / MyImages wraz z łączami do poszczególnych plików i folderów:</span><span class="sxs-lookup"><span data-stu-id="4efff-147">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

<span data-ttu-id="4efff-149">Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.</span><span class="sxs-lookup"><span data-stu-id="4efff-149">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="4efff-150">Należy zwrócić uwagę dwa `app.UseStaticFiles` wywołania.</span><span class="sxs-lookup"><span data-stu-id="4efff-150">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="4efff-151">Pierwsza z nich jest wymagany do obsługi obrazów, CSS i JavaScript w *wwwroot* folderu, a drugie wywołanie przeglądanie katalogu *wwwroot/obrazy* folder przy użyciu adresu URL http://\<aplikacji > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="4efff-151">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="4efff-152">Obsługująca dokument domyślny</span><span class="sxs-lookup"><span data-stu-id="4efff-152">Serving a default document</span></span>

<span data-ttu-id="4efff-153">Ustawianie domyślnej strony głównej zapewnia odwiedzający zacząć podczas odwiedzania witryny.</span><span class="sxs-lookup"><span data-stu-id="4efff-153">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="4efff-154">Dla aplikacji sieci Web do obsługi domyślną stronę, użytkownik nie do pełnej kwalifikacji identyfikator URI, wywołać `UseDefaultFiles` — metoda rozszerzenia z `Startup.Configure` w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="4efff-154">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="4efff-155">`UseDefaultFiles`musi zostać wywołana przed `UseStaticFiles` do obsługi domyślnego pliku.</span><span class="sxs-lookup"><span data-stu-id="4efff-155">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="4efff-156">`UseDefaultFiles`to edytor powrotnego adresu URL, który faktycznie nie mógł być pliku.</span><span class="sxs-lookup"><span data-stu-id="4efff-156">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="4efff-157">Należy włączyć oprogramowanie pośredniczące plików statycznych (`UseStaticFiles`) do obsługi pliku.</span><span class="sxs-lookup"><span data-stu-id="4efff-157">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="4efff-158">Z `UseDefaultFiles`, wyszuka żądań do folderu:</span><span class="sxs-lookup"><span data-stu-id="4efff-158">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="4efff-159">default.htm</span><span class="sxs-lookup"><span data-stu-id="4efff-159">default.htm</span></span>
* <span data-ttu-id="4efff-160">default.HTML</span><span class="sxs-lookup"><span data-stu-id="4efff-160">default.html</span></span>
* <span data-ttu-id="4efff-161">index.htm</span><span class="sxs-lookup"><span data-stu-id="4efff-161">index.htm</span></span>
* <span data-ttu-id="4efff-162">index.HTML</span><span class="sxs-lookup"><span data-stu-id="4efff-162">index.html</span></span>

<span data-ttu-id="4efff-163">Pierwszy plik znaleziono na liście zostanie obsłużona, ponieważ, jeśli żądanie zostało pełni kwalifikowanego identyfikatora URI (mimo że adres URL przeglądarki będą w dalszym ciągu Pokaż żądanego identyfikatora URI).</span><span class="sxs-lookup"><span data-stu-id="4efff-163">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="4efff-164">Poniższy kod przedstawia sposób zmiany domyślnej nazwy pliku do *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="4efff-164">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="4efff-165">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="4efff-165">UseFileServer</span></span>

<span data-ttu-id="4efff-166">`UseFileServer`łączą funkcjonalność `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="4efff-166">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="4efff-167">Poniższy kod umożliwia pliki statyczne oraz domyślny plik ma być obsługiwana, ale nie jest możliwe przeglądanie katalogów:</span><span class="sxs-lookup"><span data-stu-id="4efff-167">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="4efff-168">Poniższy kod umożliwia pliki statyczne i pliki domyślne, przeglądanie katalogów:</span><span class="sxs-lookup"><span data-stu-id="4efff-168">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="4efff-169">Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.</span><span class="sxs-lookup"><span data-stu-id="4efff-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="4efff-170">Jak `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`, jeśli chcesz udostępniać pliki znajdujące się poza `web root`, wystąpienia i skonfigurować `FileServerOptions` obiektu, który jest przekazywany jako parametr `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="4efff-170">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="4efff-171">Na przykład podane następujące hierarchii katalogów w aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="4efff-171">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="4efff-172">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="4efff-172">wwwroot</span></span>

  * <span data-ttu-id="4efff-173">CSS</span><span class="sxs-lookup"><span data-stu-id="4efff-173">css</span></span>

  * <span data-ttu-id="4efff-174">obrazy</span><span class="sxs-lookup"><span data-stu-id="4efff-174">images</span></span>

  * <span data-ttu-id="4efff-175">...</span><span class="sxs-lookup"><span data-stu-id="4efff-175">...</span></span>

* <span data-ttu-id="4efff-176">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="4efff-176">MyStaticFiles</span></span>

  * <span data-ttu-id="4efff-177">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="4efff-177">test.png</span></span>

  * <span data-ttu-id="4efff-178">default.HTML</span><span class="sxs-lookup"><span data-stu-id="4efff-178">default.html</span></span>

<span data-ttu-id="4efff-179">Przy użyciu powyższego przykładu hierarchii, warto włączyć pliki statyczne, domyślne pliki i przeglądanie dla `MyStaticFiles` katalogu.</span><span class="sxs-lookup"><span data-stu-id="4efff-179">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="4efff-180">W poniższy fragment kodu, który odbywa się przy użyciu jednego wywołania do `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="4efff-180">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="4efff-181">Jeśli `enableDirectoryBrowsing` ustawiono `true` należy wywołać `AddDirectoryBrowser` — metoda rozszerzenia z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4efff-181">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="4efff-182">Przy użyciu pliku hierarchii i powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="4efff-182">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="4efff-183">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="4efff-183">URI</span></span>            |                             <span data-ttu-id="4efff-184">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="4efff-184">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="4efff-185">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="4efff-185">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="4efff-186">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="4efff-186">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="4efff-187">Jeśli nie domyślne pliki znajdują się w *MyStaticFiles* katalogu, http://\<aplikacji > / StaticFiles zwraca listę z aktywne katalogów:</span><span class="sxs-lookup"><span data-stu-id="4efff-187">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Lista plików statycznych](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="4efff-189">`UseDefaultFiles`i `UseDirectoryBrowser` potrwa adresu url http://\<aplikacji > / StaticFiles bez ukośników i Przyczyna przekierowywania po stronie klienta http://\<aplikacji > /StaticFiles/ (Dodawanie wiodący ukośnik).</span><span class="sxs-lookup"><span data-stu-id="4efff-189">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="4efff-190">Bez końcowych ukośnika względnych adresów URL w dokumentach będą niepoprawne.</span><span class="sxs-lookup"><span data-stu-id="4efff-190">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="4efff-191">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="4efff-191">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="4efff-192">`FileExtensionContentTypeProvider` Klasa zawiera kolekcję, która mapuje rozszerzenia plików do typu MIME.</span><span class="sxs-lookup"><span data-stu-id="4efff-192">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="4efff-193">W poniższym przykładzie kilka rozszerzeń plików jest zarejestrowany na znane typy MIME, zastępuje ".rtf" i "plik MP4" zostanie usunięta.</span><span class="sxs-lookup"><span data-stu-id="4efff-193">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="4efff-194">Zobacz [typu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="4efff-194">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="4efff-195">Niestandardowe typy zawartości</span><span class="sxs-lookup"><span data-stu-id="4efff-195">Non-standard content types</span></span>

<span data-ttu-id="4efff-196">Oprogramowanie pośredniczące plików statycznych ASP.NET rozumie prawie 400 znanych typów zawartości.</span><span class="sxs-lookup"><span data-stu-id="4efff-196">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="4efff-197">Jeśli użytkownik żąda pliku nieznany typ pliku, oprogramowanie pośredniczące plików statycznych zwraca odpowiedź HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="4efff-197">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="4efff-198">Jeśli przeglądanie katalogów jest włączona, zostanie wyświetlony link do pliku, ale identyfikator URI zwróci błąd HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="4efff-198">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="4efff-199">Poniższy kod umożliwia obsługująca nieznanych typów i będą zawierały nieznanej pliku jako obrazu.</span><span class="sxs-lookup"><span data-stu-id="4efff-199">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="4efff-200">Z powyższym kodzie żądanie dla pliku z nieznanego typu zawartości, zostanie zwrócony jako obraz.</span><span class="sxs-lookup"><span data-stu-id="4efff-200">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="4efff-201">Włączanie `ServeUnknownFileTypes` stanowi zagrożenie bezpieczeństwa i korzystania z niego nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="4efff-201">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="4efff-202">`FileExtensionContentTypeProvider`(przedstawionych powyżej) jest bezpieczniejsze alternatywą dla obsługujących pliki z rozszerzeniami niestandardowym.</span><span class="sxs-lookup"><span data-stu-id="4efff-202">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="4efff-203">Uwagi</span><span class="sxs-lookup"><span data-stu-id="4efff-203">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="4efff-204">`UseDirectoryBrowser`i `UseStaticFiles` można wyciek kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="4efff-204">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="4efff-205">Zalecamy, aby użytkownik **nie** Włącz przeglądanie katalogów w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="4efff-205">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="4efff-206">Uważaj, o które katalogi Włącz z `UseStaticFiles` lub `UseDirectoryBrowser` jako cały katalog i wszystkie podkatalogi będą dostępne.</span><span class="sxs-lookup"><span data-stu-id="4efff-206">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="4efff-207">Zaleca się pozostawienie zawartości publicznej w jego własnej katalogu, takie jak  *\<zawartości główny > / wwwroot*, od widoki aplikacji, plików konfiguracji itp.</span><span class="sxs-lookup"><span data-stu-id="4efff-207">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="4efff-208">Adresy URL zawartość jest uwidaczniana z `UseDirectoryBrowser` i `UseStaticFiles` uwzględniana wielkość liter i znaków ograniczenia ich źródłowy system plików.</span><span class="sxs-lookup"><span data-stu-id="4efff-208">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="4efff-209">Na przykład systemu Windows nie uwzględnia wielkości liter, ale Mac i Linux nie są.</span><span class="sxs-lookup"><span data-stu-id="4efff-209">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="4efff-210">Aplikacje platformy ASP.NET Core hostowane w usługach IIS umożliwia moduł platformy ASP.NET Core przekazywania wszystkich żądań do aplikacji, w tym żądań dotyczących plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="4efff-210">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="4efff-211">Obsługa plików statycznych usług IIS nie jest używana, ponieważ nie uzyskać możliwość obsługi żądań przed są obsługiwane przez moduł platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4efff-211">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="4efff-212">Aby usunąć usług IIS obsługę plików statycznych (na poziomie serwera lub witryny sieci Web):</span><span class="sxs-lookup"><span data-stu-id="4efff-212">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="4efff-213">Przejdź do **modułów** funkcji</span><span class="sxs-lookup"><span data-stu-id="4efff-213">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="4efff-214">Wybierz **StaticFileModule** na liście</span><span class="sxs-lookup"><span data-stu-id="4efff-214">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="4efff-215">Wybierz **Usuń** w **akcje** paska bocznego</span><span class="sxs-lookup"><span data-stu-id="4efff-215">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="4efff-216">Jeśli jest włączona obsługa plików statycznych usług IIS **i** modułu podstawowej platformy ASP.NET (ANCM) nie jest poprawnie skonfigurowana (na przykład jeśli *web.config* nie został wdrożony), pliki statyczne będą obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4efff-216">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="4efff-217">Pliki kodu (w tym c# i Razor) należy umieścić poza projektem aplikacji `web root` (*wwwroot* domyślnie).</span><span class="sxs-lookup"><span data-stu-id="4efff-217">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="4efff-218">Spowoduje to utworzenie czyste rozdzielenie zawartości po stronie klienta dla aplikacji i kodu źródłowego po stronie serwera, które uniemożliwiają wyciek kod po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4efff-218">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4efff-219">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4efff-219">Additional Resources</span></span>

* [<span data-ttu-id="4efff-220">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="4efff-220">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="4efff-221">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4efff-221">Introduction to ASP.NET Core</span></span>](../index.md)
