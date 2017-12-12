# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>Adres URL platformy ASP.NET Core ponowne zapisywanie próbki (1.x platformy ASP.NET Core)

Ten przykład przedstawia użycie platformy ASP.NET Core 1.x ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym. Aplikacja pokazano adres URL przekierowania i ponowne zapisywanie opcje adresu URL. Dla przykładu 2.x platformy ASP.NET Core, zobacz [platformy ASP.NET Core adresu URL ponowne zapisywanie próbki (platformy ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

Podczas uruchamiania próbki, odpowiedź zostanie obsłużona pokazujący nowych lub przekierowany adres URL, gdy jedna z zasad jest stosowany do adresu URL żądania.

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - Kod stanu powodzenia: 302 (Found)
  - Przykład (przekierowanie): **/redirect-rule / {capture_group}** do **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Kod stanu powodzenia: 200 (OK)
  - Przykład (Edycja): **/rewrite-rule / {capture_group_1} / {capture_group_2}** do **/ napisany od nowa? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Kod stanu powodzenia: 302 (Found)
  - Przykład (przekierowanie): **/apache-mod-rules-redirect / {capture_group}** do **/ przekierowanie? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Kod stanu powodzenia: 200 (OK)
  - Przykład (Edycja): **/iis-rules-rewrite / {capture_group}** do **/ napisany od nowa? id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Kod stanu powodzenia: 301 (trwale przeniesiona)
  - Przykład (przekierowanie): **/file.xml** do **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Kod stanu powodzenia: 301 (trwale przeniesiona)
  - Przykład (przekierowanie): **/image.png** do **/png-images/image.png**
  - Przykład (przekierowanie): **/image.jpg** do **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Za pomocą`PhysicalFileProvider`
Możesz również uzyskać `IFileProvider` tworząc `PhysicalFileProvider` do przekazania do `AddApacheModRewrite()` i `AddIISUrlRewrite()` metod:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Zabezpieczanie rozszerzenia przekierowania
Ten przykład zawiera `WebHostBuilder` konfiguracji dla aplikacji użyć adresów URL (**https://localhost:5001**, **https://localhost**) i certyfikatu testowego (**testCert.pfx**) do pomocy w one poznawanie przekierowywania metod. Dodawanie ich do `RewriteOptions()` w **Startup.cs** badanie ich zachowanie.

Metoda | Kod stanu | Port
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | wartość null (465)
`.AddRedirectToHttps()` | 302 | wartość null (465)
`.AddRedirectToHttps(301)` | 301 | wartość null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
