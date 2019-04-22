---
title: Pliki statyczne z platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obsługiwać oraz zabezpieczanie plików statycznych i konfigurowanie plików statycznych hostingu zachowania oprogramowania pośredniczącego w aplikacji sieci web platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: fundamentals/static-files
ms.openlocfilehash: 12c7b39bee462ff83188a5a0f10b133ca273863b
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59425065"
---
# <a name="static-files-in-aspnet-core"></a>Pliki statyczne z platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Scott Addie](https://twitter.com/Scott_Addie)

Pliki statyczne, takich jak HTML, CSS, obrazów i JavaScript, to zasoby, których aplikacji ASP.NET Core służy bezpośrednio do klientów. Niektóre konfiguracja jest wymagana do włączenia obsługi tych plików.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Obsługa plików statycznych

Pliki statyczne są przechowywane w katalogu głównym projektu sieci web. Domyślny katalog jest  *\<content_root > / wwwroot*, ale można ją zmienić za pomocą [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metody. Zobacz [zawartości głównego](xref:fundamentals/index#content-root) i [katalog główny sieci Web](xref:fundamentals/index#web-root) Aby uzyskać więcej informacji.

Hosta sieci web aplikacji należy pamiętać o zawartości katalogu.

::: moniker range=">= aspnetcore-2.0"

`WebHost.CreateDefaultBuilder` Metoda ustawia zawartość katalogu głównego w bieżącym katalogu:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ustaw zawartość katalogu głównego w bieżącym katalogu, wywołując [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) wewnątrz `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

Pliki statyczne są dostępne za pośrednictwem ścieżki względem katalogu głównego sieci web. Na przykład **aplikacji sieci Web** szablonu projektu zawiera kilka folderów w ramach *wwwroot* folderu:

* **wwwroot**
  * **css**
  * **images**
  * **js**

Format identyfikatora URI, aby uzyskać dostęp do pliku w *obrazów* podfolder jest *http://\<server_address > /images/\<image_file_name >*. Na przykład *http://localhost:9189/images/banner3.svg*.

::: moniker range=">= aspnetcore-2.1"

Jeśli przeznaczony dla .NET Framework, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu. Jeśli przeznaczony dla platformy .NET Core [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) obejmuje tego pakietu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Jeśli przeznaczony dla .NET Framework, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu. Jeśli przeznaczony dla platformy .NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) obejmuje tego pakietu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu.

::: moniker-end

Konfigurowanie [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) umożliwiająca obsługi plików statycznych.

### <a name="serve-files-inside-of-web-root"></a>Udostępniać pliki wewnątrz katalog główny sieci web

Wywoływanie [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodę w ramach `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Bez parametrów `UseStaticFiles` przeciążenie metody oznacza pliki w katalogu głównym sieci web, jako servable. Następujące odwołania do znaczników *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

W poprzednim kodzie znak tyldy `~/` wskazuje webroot. Aby uzyskać więcej informacji, zobacz [katalog główny sieci Web](xref:fundamentals/index#web-root).

### <a name="serve-files-outside-of-web-root"></a>Udostępniania plików poza katalogiem głównym sieci web

Należy wziąć pod uwagę hierarchii katalogów, w którym znajdują się pliki statyczne, które ma zostać dostarczony poza katalog główny sieci web:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
    * *banner1.svg*

Żądanie mogą uzyskiwać dostęp do *banner1.svg* pliku przez skonfigurowanie statycznych oprogramowanie pośredniczące plików w następujący sposób:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

W poprzednim kodzie *MyStaticFiles* hierarchii katalogów jest udostępniane publicznie *StaticFiles* segmentem identyfikatora URI. Żądanie *http://\<server_address > /StaticFiles/images/banner1.svg* służy *banner1.svg* pliku.

Następujące odwołania do znaczników *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Ustawianie nagłówków odpowiedzi HTTP

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) obiekt może służyć do ustawiania nagłówków odpowiedzi HTTP. Oprócz konfigurowania dostarczanie plików statycznych z katalogu głównego sieci web, poniższy kod ustawia `Cache-Control` nagłówka:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) istnieje metoda w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.

Pliki zostały wprowadzone publicznie podlega buforowaniu, na 10 minut (600 sekund) w środowisku deweloperskim:

![Nagłówki odpowiedzi, wyświetlanie nagłówek Cache-Control został dodany.](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statyczny plik autoryzacji

Statyczne oprogramowanie pośredniczące plików nie zapewnia sprawdzeń autoryzacji. Wszystkich plików obsługiwanych przez, łącznie z tymi w ramach *wwwroot*, są dostępne publicznie. Aby udostępniać pliki na podstawie autoryzacji:

* Store je poza *wwwroot* i z dowolnego katalogu, które są dostępne dla statycznych oprogramowanie pośredniczące plików.
* Obsługujących je za pośrednictwem metody akcji, do którego zastosowano autoryzacji. Zwróć [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) obiektu:

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Umożliwia przeglądanie katalogów

Przeglądanie katalogów umożliwia użytkownikom aplikacji sieci web wyświetlić listę katalogów i plików w określonym katalogu. Przeglądanie katalogów jest domyślnie wyłączona, ze względów bezpieczeństwa (zobacz [zagadnienia](#considerations)). Przeglądanie za pomocą wywołania katalogów Włącz [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in Class metoda `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Dodaj wymagane usługi, wywołując [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metody z `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Powyższy kod pozwala na przeglądanie katalogu *wwwroot/obrazów* folderu przy użyciu adresu URL *http://\<server_address > / Mojeobrazy*, wraz z łączami do poszczególnych plików i folderów:

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.

Należy pamiętać, dwa `UseStaticFiles` wywołuje w poniższym przykładzie. Pierwsze wywołanie umożliwia obsługi plików statycznych w *wwwroot* folderu. Drugie wywołanie umożliwia przeglądanie katalogów dla *wwwroot/obrazów* folderu przy użyciu adresu URL *http://\<server_address > / Mojeobrazy*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Udostępniania dokumentu domyślnego

Ustawianie domyślnej strony głównej zawiera odwiedzających logiczne punkt początkowy podczas odwiedzania Twojej witryny. Aby obsługiwać domyślną stronę bez żadnego użytkownika, w pełni kwalifikujących się identyfikator URI, należy wywołać [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metody z `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` musi zostać wywołana przed `UseStaticFiles` do obsługi domyślnego pliku. `UseDefaultFiles` jest dysków adresu URL, który faktycznie nie obsługuje pliku. Włącza oprogramowanie pośredniczące plików statycznych przy użyciu `UseStaticFiles` do udostępniania plików.

Za pomocą `UseDefaultFiles`, żądania do folderu wyszukiwania:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Pierwszy plik znaleziono z listy jest obsługiwany, tak, jakby żądania zostały w pełni kwalifikowanego identyfikatora URI. Adres URL przeglądarki w dalszym ciągu odzwierciedla żądanego identyfikatora URI.

Poniższy kod zmienia domyślna nazwa pliku do *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) łączy w sobie funkcje `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`.

Poniższy kod umożliwia obsługujących pliki statyczne i domyślnego pliku. Przeglądanie katalogów nie jest włączone.

```csharp
app.UseFileServer();
```

Poniższy kod tworzy po przeciążenie bez parametrów, należy włączyć, przeglądanie katalogów:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Należy wziąć pod uwagę następujące hierarchii katalogów:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
    * *banner1.svg*
  * *default.html*

Poniższy kod umożliwia pliki statyczne, domyślne pliki i przeglądanie katalogów dla `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser` musi być wywoływana, gdy `EnableDirectoryBrowsing` wartość właściwości jest `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

Za pomocą hierarchii plików i poprzedni kod, adresy URL rozwiązać problem w następujący sposób:

| Identyfikator URI            |                             Odpowiedź  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Jeśli plik o nazwie domyślnej, nie istnieje w *MyStaticFiles* katalogu *http://\<server_address > / StaticFiles* zwraca listę wraz z łączami możesz klikać katalogów:

![Lista plików statycznych](static-files/_static/db2.png)

> [!NOTE]
> <xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> i <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> wykonania przekierowania klienta z `http://{SERVER ADDRESS}/StaticFiles` (bez znaku ukośnika na końcu) do `http://{SERVER ADDRESS}/StaticFiles/` (z ukośnikiem końcowym). Względnych adresów URL w ramach *StaticFiles* katalogu jest nieprawidłowa bez znaku ukośnika na końcu.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) klasa zawiera `Mappings` właściwość służy jako mapowanie rozszerzenia plików do typu MIME. W poniższym przykładzie kilka rozszerzeń plików są zarejestrowane do znanych typów MIME. *.Rtf* rozszerzenia są zastępowane, oraz *MP4* zostanie usunięty.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Zobacz [typu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Niestandardowe typy zawartości

Oprogramowanie pośredniczące plików statycznych rozumie prawie 400 znanych typów zawartości. Jeśli użytkownik żąda pliku z nieznany typ pliku, oprogramowanie pośredniczące plików statycznych przekazuje żądanie do następnego oprogramowania pośredniczącego w potoku. Jeśli nie oprogramowanie pośredniczące obsługuje żądania, *404 Nie znaleziono* zwróceniem odpowiedzi. Jeśli przeglądanie katalogów jest włączona, łącze do pliku jest wyświetlany na liście katalogów.

Poniższy kod umożliwia obsługująca nieznane typy i renderuje nieznanym pliku jako obrazu:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Przy użyciu poprzedniego kodu żądanie dotyczące pliku z nieznanym typem zawartości jest zwracana jako obraz.

> [!WARNING]
> Włączanie [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) stanowi zagrożenie bezpieczeństwa. Jest domyślnie wyłączona, a jego użycie nie jest zalecane. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) bezpieczniejsze alternatywą do obsługujących pliki z rozszerzeniami niestandardowych.

### <a name="considerations"></a>Uwagi

> [!WARNING]
> `UseDirectoryBrowser` i `UseStaticFiles` można przecieku wpisów tajnych. Zdecydowanie zaleca się wyłączenie przeglądanie katalogów w środowisku produkcyjnym. Dokładnie przejrzyj katalogi, które są włączone za pomocą `UseStaticFiles` lub `UseDirectoryBrowser`. Cały katalog i jego podkatalogi stają się publicznie dostępny. Store plików odpowiednie dla obsługująca publicznie w dedykowanym katalogu, takie jak  *\<content_root > / wwwroot*. Te pliki należy oddzielić od widoków MVC, stron Razor (tylko 2.x), pliki konfiguracji itp.

* Adresy URL zawartość jest uwidaczniana z `UseDirectoryBrowser` i `UseStaticFiles` podlegają rozróżnianie wielkości liter i ograniczenia dotyczące znaków bazowego systemu plików. Na przykład Windows jest uwzględniana wielkość liter&mdash;systemach macOS i Linux nie są.

* Aplikacje platformy ASP.NET Core hostowanych w użyciu IIS [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) do przekazywania wszystkich żądań do aplikacji, w tym realizowania żądań plików statycznych. Obsługa plików statycznych usług IIS nie jest używany. Ma on możliwość obsługi żądań przed są one obsługiwane przez moduł.

* Wykonaj następujące czynności w Menedżerze usług IIS, aby usunąć program obsługi plików statycznych usług IIS na poziomie serwera lub witryny sieci Web:
    1. Przejdź do **modułów** funkcji.
    1. Wybierz **StaticFileModule** na liście.
    1. Kliknij przycisk **Usuń** w **akcje** pasku bocznym.

> [!WARNING]
> Po włączeniu obsługi plików statycznych IIS **i** modułu ASP.NET Core jest nieprawidłowo skonfigurowany, pliki statyczne są obsługiwane. Dzieje się tak, na przykład, jeśli *web.config* plik nie jest wdrożony.

* Umieść pliki kodu (w tym *.cs* i *.cshtml*) poza katalogiem głównym pakietu projektu aplikacji sieci web. W związku z tym powstaje separacji logicznej między zawartości po stronie klienta i kod na serwerze aplikacji. Zapobiega to wyciek kodu po stronie serwera.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Wprowadzenie do platformy ASP.NET Core](xref:index)
