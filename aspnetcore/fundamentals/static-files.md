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
# <a name="working-with-static-files-in-aspnet-core"></a>Praca z plików statycznych w ASP.NET Core

<a name="fundamentals-static-files"></a>

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Pliki statyczne, takich jak HTML, CSS, obrazu i JavaScript, są zasoby, które aplikacji platformy ASP.NET Core mogą służyć bezpośrednio do klientów.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="serving-static-files"></a>Dostarczanie plików statycznych

Pliki statyczne są zazwyczaj znajduje się w `web root` (*\<zawartości główny > / wwwroot*) folderu. Zobacz [zawartości głównego](xref:fundamentals/index#content-root) i [głównego sieci Web](xref:fundamentals/index#web-root) Aby uzyskać więcej informacji. Zwykle ustawić zawartości głównego jako bieżący katalog, aby projektu `web root` zostaną znalezione w rozwoju.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Pliki statyczne mogą być przechowywane w dowolnym folderze `web root` i uzyskanie dostępu do ścieżki względnej z elementem głównym. Na przykład podczas tworzenia domyślny projekt aplikacji sieci Web przy użyciu programu Visual Studio istnieje kilka folderami utworzonych w ramach *wwwroot* folderu - *css*, *obrazów*, i *js*. Identyfikator URI uzyskać dostępu do obrazu w *obrazów* podfolderów:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Aby pliki statyczne z zabezpieczeniami, należy skonfigurować [oprogramowanie pośredniczące](middleware.md) Aby dodać pliki statyczne z potokiem. Oprogramowanie pośredniczące plików statycznych można skonfigurować przez dodanie zależności na *Microsoft.AspNetCore.StaticFiles* pakietu do projektu, a następnie wywołania `UseStaticFiles` — metoda rozszerzenia z `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`powoduje, że pliki w `web root` (*wwwroot* domyślnie) servable. Później zostanie omówiony sposób wprowadzania servable z innych zawartość katalogu `UseStaticFiles`.

Musi zawierać pakiet NuGet "Microsoft.AspNetCore.StaticFiles".

> [!NOTE]
> `web root`Domyślnie *wwwroot* katalogu, ale można ustawić `web root` katalogu z `UseWebRoot`.

Załóżmy, że masz hierarchii projektu, w którym chcesz obsługiwać pliki statyczne są poza `web root`. Na przykład:

* Wwwroot
  * CSS
  * obrazy
  * ...
* MyStaticFiles
  * Test.PNG

Dla żądania dostępu do *test.png*, skonfiguruj oprogramowanie pośredniczące plików statycznych w następujący sposób:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Żądanie `http://<app>/StaticFiles/test.png` posłuży *test.png* pliku.

`StaticFileOptions()`można ustawić nagłówków odpowiedzi. Na przykład poniższy kod konfiguruje dostarczanie z plików statycznych *wwwroot* folderu i zestawy `Cache-Control` nagłówka, aby były publicznie buforowalnej przez 10 minut (600 sekund):

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metoda jest dostępna z [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu. Dodaj `using Microsoft.AspNetCore.Http;` do Twojej *csharp* plik, jeśli metoda jest niedostępny.

![Dodano przedstawiający nagłówek Cache-Control nagłówki odpowiedzi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Plik statyczny autoryzacji

Udostępnia moduł plików statycznych **nie** sprawdzeń autoryzacji. Wszystkie pliki obsługiwane przez, włącznie z zawartymi w obszarze *wwwroot* publicznie dostępne. Do obsługi plików na podstawie autoryzacji:

* Przechowywania ich poza *wwwroot* i dowolnego katalogu, które są dostępne dla oprogramowania pośredniczącego plików statycznych **i**

* Obsługujących je za pomocą akcji kontrolera, zwracając `FileResult` których stosowane jest autoryzacji

## <a name="enabling-directory-browsing"></a>Włączanie przeglądania katalogów

Przeglądanie katalogów umożliwia użytkownikowi wyświetlenie listy katalogów i plików w określonym katalogu aplikacji sieci web. Przeglądanie katalogów jest domyślnie wyłączona, ze względów bezpieczeństwa (zobacz [zagadnienia](#considerations)). Aby umożliwić przeglądanie katalogów, należy wywołać `UseDirectoryBrowser` — metoda rozszerzenia z `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

I Dodaj wymagane usługi, wywołując `AddDirectoryBrowser` — metoda rozszerzenia z `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

Powyższy kod umożliwia przeglądanie katalogów z *wwwroot/obrazy* folder przy użyciu adresu URL http://\<aplikacji > / MyImages wraz z łączami do poszczególnych plików i folderów:

![Przeglądanie katalogów](static-files/_static/dir-browse.png)

Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania.

Należy zwrócić uwagę dwa `app.UseStaticFiles` wywołania. Pierwsza z nich jest wymagany do obsługi obrazów, CSS i JavaScript w *wwwroot* folderu, a drugie wywołanie przeglądanie katalogu *wwwroot/obrazy* folder przy użyciu adresu URL http://\<aplikacji > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Obsługująca dokument domyślny

Ustawianie domyślnej strony głównej zapewnia odwiedzający zacząć podczas odwiedzania witryny. Dla aplikacji sieci Web do obsługi domyślną stronę, użytkownik nie do pełnej kwalifikacji identyfikator URI, wywołać `UseDefaultFiles` — metoda rozszerzenia z `Startup.Configure` w następujący sposób.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`musi zostać wywołana przed `UseStaticFiles` do obsługi domyślnego pliku. `UseDefaultFiles`to edytor powrotnego adresu URL, który faktycznie nie mógł być pliku. Należy włączyć oprogramowanie pośredniczące plików statycznych (`UseStaticFiles`) do obsługi pliku.

Z `UseDefaultFiles`, wyszuka żądań do folderu:

* default.htm
* default.HTML
* index.htm
* index.HTML

Pierwszy plik znaleziono na liście zostanie obsłużona, ponieważ, jeśli żądanie zostało pełni kwalifikowanego identyfikatora URI (mimo że adres URL przeglądarki będą w dalszym ciągu Pokaż żądanego identyfikatora URI).

Poniższy kod przedstawia sposób zmiany domyślnej nazwy pliku do *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`łączą funkcjonalność `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`.

Poniższy kod umożliwia pliki statyczne oraz domyślny plik ma być obsługiwana, ale nie jest możliwe przeglądanie katalogów:

```csharp
app.UseFileServer();
   ```

Poniższy kod umożliwia pliki statyczne i pliki domyślne, przeglądanie katalogów:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

Zobacz [zagadnienia](#considerations) na zagrożenia bezpieczeństwa, podczas włączania przeglądania. Jak `UseStaticFiles`, `UseDefaultFiles`, i `UseDirectoryBrowser`, jeśli chcesz udostępniać pliki znajdujące się poza `web root`, wystąpienia i skonfigurować `FileServerOptions` obiektu, który jest przekazywany jako parametr `UseFileServer`. Na przykład podane następujące hierarchii katalogów w aplikacji sieci Web:

* Wwwroot

  * CSS

  * obrazy

  * ...

* MyStaticFiles

  * Test.PNG

  * default.HTML

Przy użyciu powyższego przykładu hierarchii, warto włączyć pliki statyczne, domyślne pliki i przeglądanie dla `MyStaticFiles` katalogu. W poniższy fragment kodu, który odbywa się przy użyciu jednego wywołania do `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Jeśli `enableDirectoryBrowsing` ustawiono `true` należy wywołać `AddDirectoryBrowser` — metoda rozszerzenia z `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Przy użyciu pliku hierarchii i powyższym kodzie:

| Identyfikator URI            |                             Odpowiedź  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Jeśli nie domyślne pliki znajdują się w *MyStaticFiles* katalogu, http://\<aplikacji > / StaticFiles zwraca listę z aktywne katalogów:

![Lista plików statycznych](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`i `UseDirectoryBrowser` potrwa adresu url http://\<aplikacji > / StaticFiles bez ukośników i Przyczyna przekierowywania po stronie klienta http://\<aplikacji > /StaticFiles/ (Dodawanie wiodący ukośnik). Bez końcowych ukośnika względnych adresów URL w dokumentach będą niepoprawne.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

`FileExtensionContentTypeProvider` Klasa zawiera kolekcję, która mapuje rozszerzenia plików do typu MIME. W poniższym przykładzie kilka rozszerzeń plików jest zarejestrowany na znane typy MIME, zastępuje ".rtf" i "plik MP4" zostanie usunięta.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

Zobacz [typu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Niestandardowe typy zawartości

Oprogramowanie pośredniczące plików statycznych ASP.NET rozumie prawie 400 znanych typów zawartości. Jeśli użytkownik żąda pliku nieznany typ pliku, oprogramowanie pośredniczące plików statycznych zwraca odpowiedź HTTP 404 (nie znaleziono). Jeśli przeglądanie katalogów jest włączona, zostanie wyświetlony link do pliku, ale identyfikator URI zwróci błąd HTTP 404.

Poniższy kod umożliwia obsługująca nieznanych typów i będą zawierały nieznanej pliku jako obrazu.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Z powyższym kodzie żądanie dla pliku z nieznanego typu zawartości, zostanie zwrócony jako obraz.

>[!WARNING]
> Włączanie `ServeUnknownFileTypes` stanowi zagrożenie bezpieczeństwa i korzystania z niego nie jest zalecane.  `FileExtensionContentTypeProvider`(przedstawionych powyżej) jest bezpieczniejsze alternatywą dla obsługujących pliki z rozszerzeniami niestandardowym.

### <a name="considerations"></a>Uwagi

>[!WARNING]
> `UseDirectoryBrowser`i `UseStaticFiles` można wyciek kluczy tajnych. Zalecamy, aby użytkownik **nie** Włącz przeglądanie katalogów w środowisku produkcyjnym. Uważaj, o które katalogi Włącz z `UseStaticFiles` lub `UseDirectoryBrowser` jako cały katalog i wszystkie podkatalogi będą dostępne. Zaleca się pozostawienie zawartości publicznej w jego własnej katalogu, takie jak  *\<zawartości główny > / wwwroot*, od widoki aplikacji, plików konfiguracji itp.

* Adresy URL zawartość jest uwidaczniana z `UseDirectoryBrowser` i `UseStaticFiles` uwzględniana wielkość liter i znaków ograniczenia ich źródłowy system plików. Na przykład systemu Windows nie uwzględnia wielkości liter, ale Mac i Linux nie są.

* Aplikacje platformy ASP.NET Core hostowane w usługach IIS umożliwia moduł platformy ASP.NET Core przekazywania wszystkich żądań do aplikacji, w tym żądań dotyczących plików statycznych. Obsługa plików statycznych usług IIS nie jest używana, ponieważ nie uzyskać możliwość obsługi żądań przed są obsługiwane przez moduł platformy ASP.NET Core.

* Aby usunąć usług IIS obsługę plików statycznych (na poziomie serwera lub witryny sieci Web):

     * Przejdź do **modułów** funkcji

     * Wybierz **StaticFileModule** na liście

     * Wybierz **Usuń** w **akcje** paska bocznego

>[!WARNING]
> Jeśli jest włączona obsługa plików statycznych usług IIS **i** modułu podstawowej platformy ASP.NET (ANCM) nie jest poprawnie skonfigurowana (na przykład jeśli *web.config* nie został wdrożony), pliki statyczne będą obsługiwane.

* Pliki kodu (w tym c# i Razor) należy umieścić poza projektem aplikacji `web root` (*wwwroot* domyślnie). Spowoduje to utworzenie czyste rozdzielenie zawartości po stronie klienta dla aplikacji i kodu źródłowego po stronie serwera, które uniemożliwiają wyciek kod po stronie serwera.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](middleware.md)

* [Wprowadzenie do platformy ASP.NET Core](../index.md)
