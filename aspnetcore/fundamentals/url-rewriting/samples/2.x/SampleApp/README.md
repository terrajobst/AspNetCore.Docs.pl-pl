# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Adresu URL platformy ASP.NET Core ponowne napisanie przykładowe (ASP.NET Core 2.x)

W tym przykładzie pokazano sposób użycia platformy ASP.NET Core 2.x oprogramowanie pośredniczące ponownego zapisywania adresów URL. Aplikacja pokazuje adres URL przekierowania i adres URL ponowne napisanie opcje.

Podczas uruchamiania przykładu, innego niż plik odpowiedzi zwraca nowych lub przekierowanego adresu URL po zastosowaniu jednej reguły do adresu URL żądania. Aby uzyskać przykłady plików XML i tekst oprogramowanie pośredniczące plików statycznych służy pliku po adresie URL żądania jest przepisany przez oprogramowanie pośredniczące.

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Kod stanu powodzenia: 302 (Found)
  - Przykład (przekierowanie): **/redirect-rule / {capture_group}** do **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Kod stanu powodzenia: 200 (OK)
  - Przykład (Edycja): **/rewrite-rule / {capture_group_1} / {capture_group_2}** do **/ przepisany? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Kod stanu powodzenia: 302 (Found)
  - Przykład (przekierowanie): **/apache-mod-rules-redirect / {capture_group}** do **/ przekierowanie? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Kod stanu powodzenia: 200 (OK)
  - Przykład (Edycja): **/iis-rules-rewrite / {capture_group}** do **/ przepisany? id = {capture_group}**
* `Add(RedirectXmlFileRequests)`
  - Kod stanu powodzenia: 301 (trwale przeniesiona)
  - Przykład (przekierowanie): **/file.xml** do **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - Kod stanu powodzenia: 200 (OK)
  - Przykład (Edycja): **/some_file.txt** do **/file.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Kod stanu powodzenia: 301 (trwale przeniesiona)
  - Przykład (przekierowanie): **/image.png** do **/png-images/image.png**
  - Przykład (przekierowanie): **/image.jpg** do **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>Użyj PhysicalFileProvider

Możesz również uzyskać `IFileProvider` , tworząc `PhysicalFileProvider` do przekazania do `AddApacheModRewrite()` i `AddIISUrlRewrite()` metody:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Zabezpieczanie rozszerzenia przekierowania

Ten przykład obejmuje `WebHostBuilder` konfiguracji dla aplikacji, które chcesz użyć adresów URL (`https://localhost:5001`, `https://localhost`) i certyfikat testowy (*testCert.pfx*) ułatwiają Eksplorowanie metod bezpiecznego przekierowania. Jeśli serwer ma już portu 443 przypisane lub jest w użyciu, `https://localhost` przykład nie zadziała&mdash;Usuń `ListenOptions` dla portu 443 w `CreateWebHostBuilder` metody *Program.cs* pliku lub Usuń powiązanie portu 443 na serwer, tak aby usługa Kestrel można użyć portu.

| Metoda                           | Kod stanu: |    Port    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | wartość null (465) |
| `.AddRedirectToHttps()`          |     302     | wartość null (465) |
| `.AddRedirectToHttps(301)`       |     301     | wartość null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
