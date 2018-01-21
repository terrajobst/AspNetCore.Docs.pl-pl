---
title: Praca z plikami statycznych w ASP.NET Core
author: rick-anderson
description: "Dowiedz się, jak obsługiwać i zabezpieczania pliki statyczne oraz skonfigurować plik statyczny hosting zachowania oprogramowania pośredniczącego w aplikacji sieci web platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Praca z plikami statycznych w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Scott Addie](https://twitter.com/Scott_Addie)

Pliki statyczne, takich jak HTML, CSS, obrazy i JavaScript, są trwałe, które służy aplikacji platformy ASP.NET Core bezpośrednio do klientów. Niektóre konfiguracja jest wymagana, aby włączyć do obsługi tych plików.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Obsługiwać pliki statyczne

Pliki statyczne są przechowywane w katalogu głównym projektu sieci web. Domyślny katalog jest  *\<content_root > / wwwroot*, ale można ją zmienić za pomocą [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metody. Zobacz [zawartości głównego](xref:fundamentals/index#content-root) i [głównego sieci Web](xref:fundamentals/index#web-root) Aby uzyskać więcej informacji.

Host sieci web aplikacji należy pamiętać o zawartości katalogu.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`WebHost.CreateDefaultBuilder` Metoda ustawia zawartości katalogu głównego w bieżącym katalogu:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ustaw główny zawartości do bieżącego katalogu, wywołując [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) wewnątrz `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Pliki statyczne są dostępne za pośrednictwem ścieżka względem katalogu głównego sieci web. Na przykład **aplikacji sieci Web** szablon projektu zawiera kilka folderów w ramach *wwwroot* folderu:

* **wwwroot**
  * **CSS**
  * **images**
  * **js**

Dostęp do pliku w formacie identyfikatora URI *obrazów* podfolder jest *http://\<server_address > /images/\<image_file_name >*. Na przykład *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli przeznaczonych dla platformy .NET Framework, Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu. Jeśli przeznaczonych dla platformy .NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) obejmuje tego pakietu.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dodaj [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pakietu do projektu.

---

Skonfiguruj [oprogramowanie pośredniczące](xref:fundamentals/middleware) co pozwala obsługi plików statycznych.

### <a name="serve-files-inside-of-web-root"></a>Udostępniać pliki wewnątrz głównego sieci web

Wywołanie [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodę `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Bez parametrów `UseStaticFiles` pliki w katalogu głównym sieci web jako servable oznacza przeciążenie metody. Następujące odwołania znaczników *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Rozpoznawanie plików poza katalogiem głównym sieci web

Należy wziąć pod uwagę hierarchii katalogów, w którym znajdują się pliki statyczne, który ma zostać dostarczony poza katalogiem głównym sieci web:

* **wwwroot**
  * **CSS**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

Żądanie mogą uzyskiwać dostęp do *banner1.svg* pliku przez skonfigurowanie oprogramowanie pośredniczące plików statycznych w następujący sposób:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

W powyższym kodzie *MyStaticFiles* hierarchii katalogów jest udostępniane publicznie za pośrednictwem funkcji *StaticFiles* segmentem identyfikatora URI. Żądanie *http://\<server_address > /StaticFiles/images/banner1.svg* służy *banner1.svg* pliku.

Następujące odwołania znaczników *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Ustawianie nagłówków odpowiedzi HTTP

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) obiekt może służyć do ustawienia nagłówków odpowiedzi HTTP. Oprócz konfigurowania obsługi plików statycznych z katalogu głównego sieci web, poniższy kod ustawia `Cache-Control` nagłówka:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) istnieje metoda w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.

Pliki zostały wprowadzone publicznie buforowalnej przez 10 minut (600 sekund):

![Dodano przedstawiający nagłówek Cache-Control nagłówki odpowiedzi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Plik statyczny autoryzacji

Oprogramowanie pośredniczące plików statycznych nie zapewnia sprawdzeń autoryzacji. Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot*, są dostępne publicznie. Do obsługi plików na podstawie autoryzacji:

* Przechowywania ich poza *wwwroot* i dowolnego katalogu, które są dostępne dla oprogramowania pośredniczącego plików statycznych **i**
* Obsługujących je za pomocą metody akcji, do którego zastosowano autoryzacji. Zwraca [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) obiektu:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Umożliwia przeglądanie katalogów

Przeglądanie katalogów umożliwia użytkownikom aplikacji sieci web wyświetlić listę katalogów i plików w określonym katalogu. Przeglądanie katalogów jest domyślnie wyłączona, ze względów bezpieczeństwa (zobacz [zagadnienia](#considerations)). Przeglądanie wywołując katalogów Włącz [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metody w `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Dodaj wymagane usługi, wywołując [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metody z `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Poprzedni kod umożliwia przeglądanie katalogów z *wwwroot/obrazy* folder przy użyciu adresu URL *http://\<server_address > / MyImages*, wraz z łączami do poszczególnych plików i folderów:

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.

Należy zwrócić uwagę dwa `UseStaticFiles` wywołuje w poniższym przykładzie. Pierwsze wywołanie umożliwia obsługi plików statycznych w *wwwroot* folderu. Drugie wywołanie umożliwia przeglądanie katalogów z *wwwroot/obrazy* folder przy użyciu adresu URL *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Udostępniania dokumentu domyślnego

Ustawianie domyślnej strony głównej stanowi odwiedzających logicznej punkt początkowy podczas odwiedzania witryny. Aby obsługiwać domyślną stronę bez użytkownika pełni kwalifikujących się identyfikator URI, należy wywołać [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metody z `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`musi zostać wywołana przed `UseStaticFiles` do obsługi domyślnego pliku. `UseDefaultFiles`jest funkcji ponownego zapisu adresu URL, który faktycznie nie mógł być pliku. Włącz oprogramowanie pośredniczące plików statycznych przy użyciu `UseStaticFiles` do obsługi pliku.

Z `UseDefaultFiles`, żądania do folderu wyszukiwania:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

Pierwszy plik znaleziono na liście jest wyświetlona tak, jakby żądania zostały w pełni kwalifikowanego identyfikatora URI. Adres URL przeglądarki w dalszym ciągu odzwierciedlał żądanego identyfikatora URI.

Poniższy kod zmienia domyślnej nazwy pliku do *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) łączą funkcjonalność `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`.

Poniższy kod umożliwia obsługujących pliki statyczne i domyślnego pliku. Przeglądanie katalogów nie jest włączone.

```csharp
app.UseFileServer();
```

Następujący kod, bazując na przeciążenia bez parametrów, należy włączyć, przeglądanie katalogów:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Należy wziąć pod uwagę następujące hierarchii katalogów:

* **wwwroot**
  * **CSS**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

Poniższy kod umożliwia pliki statyczne, domyślne pliki i przeglądanie katalogów z `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`musi być wywoływane, gdy `EnableDirectoryBrowsing` wartość właściwości jest `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

Za pomocą hierarchii plików i poprzedzających kodu, adresy URL rozwiązania w następujący sposób:

| Identyfikator URI            |                             Odpowiedź  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Jeśli nie o nazwie domyślnej istnieje plik w *MyStaticFiles* katalogu, *http://\<server_address > / StaticFiles* zwraca listę z aktywne katalogów:

![Lista plików statycznych](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`i `UseDirectoryBrowser` Użyj adresu URL *http://\<server_address > / StaticFiles* bez ukośników do wyzwolenia klienta przekierowania do *http://\<server_address > / StaticFiles /*. Zwróć uwagę, dodanie wiodący ukośnik. Względnych adresów URL w dokumentach jest uznany za nieprawidłowy bez ukośnika.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) klasa zawiera `Mappings` właściwości służy jako mapowanie rozszerzenia plików do typu MIME. W poniższym przykładzie kilka rozszerzeń plików jest zarejestrowany na znanych typów MIME. *.Rtf* rozszerzenia są zastępowane, oraz *plik MP4* zostanie usunięta.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Zobacz [typu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Niestandardowe typy zawartości

Oprogramowanie pośredniczące plików statycznych rozumie prawie 400 znanych typów zawartości. Jeśli użytkownik żąda pliku nieznany typ pliku, oprogramowanie pośredniczące plików statycznych zwraca odpowiedź HTTP 404 (nie znaleziono). Jeśli przeglądanie katalogów jest włączona, zostanie wyświetlony link do pliku. Identyfikator URI zwraca błąd HTTP 404.

Poniższy kod umożliwia obsługująca nieznanych typów i renderuje nieznany pliku jako obrazu:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Z poprzednim kodzie żądanie dla pliku z nieznanego typu zawartości jest zwracana jako obraz.

> [!WARNING]
> Włączanie [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) stanowi zagrożenie bezpieczeństwa. Jest domyślnie wyłączona, a jego użycie nie jest zalecane. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) bezpieczniejsze alternatywą do obsługujących pliki z rozszerzeniami niestandardowym.

### <a name="considerations"></a>Uwagi

> [!WARNING]
> `UseDirectoryBrowser`i `UseStaticFiles` można wyciek kluczy tajnych. Zdecydowanie zaleca się wyłączenie przeglądanie katalogów w środowisku produkcyjnym. Uważnie przejrzyj katalogi, które są włączone za pośrednictwem `UseStaticFiles` lub `UseDirectoryBrowser`. Całego katalogu i jego podkatalogów stać się publicznie. Magazyn plików odpowiednie do użycia w publicznie dedykowanej katalog, takich jak  *\<content_root > / wwwroot*. Te pliki należy oddzielić od widoków MVC, stron Razor (tylko 2.x), pliki konfiguracji itp.

* Adresy URL zawartość jest uwidaczniana z `UseDirectoryBrowser` i `UseStaticFiles` uwzględniana wielkość liter i znaków ograniczenia źródłowy system plików. Na przykład systemu Windows nie uwzględnia wielkości liter&mdash;nie Mac i Linux.

* Aplikacji platformy ASP.NET Core hostowanych w użyciu IIS [platformy ASP.NET Core modułu (ANCM)](xref:fundamentals/servers/aspnet-core-module) do przekazywania wszystkich żądań do aplikacji, włącznie z żądaniami plików statycznych. Obsługa plików statycznych usług IIS nie jest używany. Nie ma możliwość obsługi żądań przed są one obsługiwane przez ANCM.

* Wykonaj następujące kroki w Menedżerze usług IIS do obsługi plików statycznych usług IIS na poziomie serwera lub witryny sieci Web do usunięcia:
    1. Przejdź do **modułów** funkcji.
    1. Wybierz **StaticFileModule** na liście.
    1. Kliknij przycisk **Usuń** w **akcje** paska bocznego.

> [!WARNING]
> Jeśli jest włączona obsługa plików statycznych IIS **i** ANCM jest niepoprawnie skonfigurowana, pliki statyczne są obsługiwane. Dzieje się tak, na przykład, jeśli *web.config* plik nie jest wdrożony.

* Umieść kod plików (w tym *.cs* i *.cshtml*) poza katalogiem głównym projektu aplikacji sieci web. W związku z tym utworzeniu separacji logicznej między zawartości po stronie klienta i kodu na serwerze aplikacji. Zapobiega to wycieku kod po stronie serwera.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware)

* [Wprowadzenie do platformy ASP.NET Core](xref:index)
