---
title: Adres URL ponowne napisanie oprogramowania pośredniczącego w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z adresem URL ponownego zapisywania adresów i przekierowywania z oprogramowanie pośredniczące ponownego zapisywania adresów URL w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326072"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Adres URL ponowne napisanie oprogramowania pośredniczącego w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Mikael Mengistu](https://github.com/mikaelm12)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Ponownego zapisywania adresów URL jest działaniem zmiany żądania, których adresy URL na podstawie co najmniej jeden wstępnie zdefiniowanych reguł. Ponownego zapisywania adresów URL tworzy abstrakcję między lokalizacje zasobów i ich adresów, tak, aby lokalizacje i adresy nie są ściśle powiązane. Istnieje kilka scenariuszy, w których jest przydatna ponownego zapisywania adresów URL:

* Przenoszenie lub zastępując tymczasowo lub trwale zasoby serwera przy zachowaniu stabilne lokalizatorów dla tych zasobów.
* Podział żądań w różnych aplikacjach lub w wielu obszarach jednej aplikacji.
* Usuwanie, dodawanie lub reorganizacja segmenty adresu URL na przychodzące żądania.
* Optymalizacja publiczne adresy URL do optymalizacji aparatu wyszukiwania (SEO).
* Umożliwiający użycie przyjaznych adresów URL publicznej, aby pomóc użytkownikom przewidzieć zawartość, którą znajdą, wykonując poniższe łącze.
* Przekierowywanie żądań niezabezpieczone bezpieczne punkty końcowe.
* Zapobieganie hotlinking obrazu.

Można zdefiniować reguły zmiana adresu URL na kilka sposobów, łącznie z wyrażeniem regularnym Apache mod_rewrite modułu zasad, zasady moduł ponowne zapisywanie adresów usług IIS i przy użyciu reguły niestandardowej logiki. Ten dokument wprowadza instrukcje dotyczące sposobu używania oprogramowanie pośredniczące ponownego zapisywania adresów URL w aplikacji platformy ASP.NET Core ponownego zapisywania adresów URL.

> [!NOTE]
> Ponownego zapisywania adresów URL może zmniejszyć wydajność aplikacji. Jeśli jest to możliwe, należy ograniczyć liczbę i złożoność reguł.

## <a name="url-redirect-and-url-rewrite"></a>Ponowne zapisywanie adresów URL przekierowania i adres URL

Różnica w treść między *adres URL przekierowania* i *ponowne zapisywanie adresów URL* może wydawać się subtelne na pierwszego, ale ma istotny wpływ na zapewniania zasobów dla klientów. Oprogramowanie pośredniczące ponownego zapisywania adresów URL platformy ASP.NET Core jest w stanie spełniające potrzeby dla obu.

A *adres URL przekierowania* jest operacją po stronie klienta, w których klient jest zobowiązany do uzyskania dostępu do zasobu na inny adres. Ta migracja wymaga przesłania danych do serwera. Adres URL przekierowania zwracana do klienta pojawia się w pasku adresu przeglądarki, gdy klient wysyła nowe żądanie dla zasobu. 

Jeśli `/resource` jest *przekierowanie* do `/different-resource`, żądań klientów `/resource`. Następnie serwer odpowiada, że klient powinien uzyskać zasób w `/different-resource` z kodem stanu wskazującym, przekierowania, który jest tymczasowy lub stały. Klient wykonuje żądanie nowego zasobu pod adresem URL przekierowania.

![Punkt końcowy usługi WebAPI tymczasowo zmieniono z wersją 1 (v1) do wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w wersji 1 /v1/api ścieżki. Serwer odsyła 302 odpowiedź (Found) z nowego, tymczasowego ścieżkę dla usługi w wersji 2 /v2/api. Klient wysyła drugie żądanie do usługi pod adresem URL przekierowania. Serwer odpowiada, zwracając kod stanu 200 (OK).](url-rewriting/_static/url_redirect.png)

Przekierowywanie żądań do innego adresu URL, wskazują, czy przekierowania, który jest stałych lub tymczasowych. 301 (trwale przeniesiona) kod stanu służy gdzie zasób ma nowy, stały adres URL i chcesz w celu poinstruowania klienta o tym, że wszystkie przyszłe żądania dotyczące zasobów powinien używać nowego adresu URL. *Klient może buforować odpowiedzi, po odebraniu kodu 301 stanu.* 302 kod stanu (Found) jest używana, gdzie przekierowania jest tymczasowy lub ogólnie podmiotu można zmienić w taki sposób, że klient nie należy przechowywać i ponowne użycie adres URL przekierowania w przyszłości. Aby uzyskać więcej informacji, zobacz [dokumencie RFC 2616: definicje kodów stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *ponowne zapisywanie adresów URL* jest operacją po stronie serwera w celu zapewnienia zasobów z adresu innego zasobu. Ponownego zapisywania adresów URL nie wymaga przesłania danych do serwera. Nowych adres URL nie jest zwracana do klienta i nie będzie wyświetlane na pasku adresu przeglądarki. Gdy `/resource` jest *przepisany* do `/different-resource`, żądań klientów `/resource`, a serwerem *wewnętrznie* pobiera zasób o `/different-resource`. Mimo, że klient może być możliwe do pobrania zasobu pod adresem URL nowych, klient nie będzie informacja, że zasób istnieje pod adresem URL nowych sprawia, że jego żądanie i odbiera odpowiedź.

![Punkt końcowy usługi WebAPI została zmieniona z wersją 1 (v1) do wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w wersji 1 /v1/api ścieżki. Adres URL żądania jest przepisane, aby uzyskać dostęp do usługi w wersji 2 /v2/api ścieżki. Usługa odnosi się do klienta z kodem stanu 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Przykładowa aplikacja ponownego zapisywania adresów URL

Możesz eksplorować funkcje pośredniczącym ponownego zapisywania adresów URL przy użyciu [ponownego zapisywania adresów URL przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Aplikacja stosuje ponownego zapisywania i Przekierowanie reguł i zawiera adres URL nowych lub przekierowanego.

## <a name="when-to-use-url-rewriting-middleware"></a>Kiedy należy używać oprogramowanie pośredniczące ponownego zapisywania adresów URL

Użyj oprogramowanie pośredniczące ponownego zapisywania adresów URL, gdy nie można używać [moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) z usługami IIS w systemie Windows Server [modułu mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) na serwerze Apache [na NginxponownegozapisywaniaadresówURL](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), lub aplikacja jest hostowana na [serwera HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)). Główne powody korzystania na serwerze adresu URL przebudowywania technologii w IIS, Apache i Nginx są, oprogramowanie pośredniczące nie obsługuje funkcji pełnego tych modułów i wydajność oprogramowania pośredniczącego prawdopodobnie nie będzie zgodne z modułów. Istnieją jednak pewne funkcje modułów serwera, która nie działa z projektami ASP.NET Core, takich jak `IsFile` i `IsDirectory` ograniczenia moduł ponowne zapisywanie adresów IIS. W tych scenariuszach Użyj oprogramowania pośredniczącego.

## <a name="package"></a>Package

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pakietu. Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.

## <a name="extension-and-options"></a>Opcje i rozszerzenia

Ustanowienia sieci ponowne zapisywanie adresów URL i przekierować reguły przez utworzenie wystąpienia [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) klasy przy użyciu metody rozszerzenia dla poszczególnych reguł. Utworzyć łańcuch wielu reguł w kolejności, że chcesz je przetworzyć. `RewriteOptions` Są przekazywane do oprogramowanie pośredniczące ponownego zapisywania adresów URL jest dodawany do potoku żądania za pomocą `app.UseRewriter(options);`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a>Przekierowanie nie www do www

Trzy opcje umożliwiają aplikacji, aby przekierować non -`www` żądania `www`:

* [AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; trwałe przekierowanie żądania do `www` poddomeny, jeśli żądanie ma wartość inną niż`www`. Przekierowuje z [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) kod stanu.
* [AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; przekierować żądania `www` poddomeny, jeśli żądanie przychodzące ma wartość inną niż`www`. Przekierowuje z [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) kod stanu.
* [AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; przekierować żądania `www` poddomeny, jeśli żądanie przychodzące ma wartość inną niż`www`. Umożliwia podanie kodu stanu odpowiedzi. Użyj pola [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) klasy do przypisania do `AddRedirectToWww`.

::: moniker-end

### <a name="url-redirect"></a>Adres URL przekierowania

Użyj `AddRedirect` Przekierowywanie żądań. Pierwszy parametr zawiera Twoje wyrażenia regularnego do dopasowania w ścieżce przychodzącego adresu URL. Drugi parametr jest ciąg zastępujący. Trzeci parametr, jeśli jest obecny, określa kod stanu. Jeśli nie określisz kod stanu, domyślnie 302 (Found), która wskazuje, że zasób tymczasowo przenieść lub zastąpiony.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

W przeglądarce za pomocą narzędzi dla deweloperów, włączone, należy wysłać żądanie do przykładowej aplikacji ze ścieżką `/redirect-rule/1234/5678`. Wyrażenie regularne dopasowuje ścieżki żądania w `redirect-rule/(.*)`, a ścieżka została zastąpiona `/redirected/1234/5678`. Przekieruj adres URL jest wysyłane z powrotem do klienta z kodem stanu 302 (Found). Przeglądarka sprawia, że nowe wezwanie pod adresem URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki. Ponieważ żadne reguły w przykładowej aplikacji odpowiada na adres URL przekierowania, drugie żądanie odbiera odpowiedź 200 (OK) z poziomu aplikacji, a treść odpowiedzi zawiera adres URL przekierowania. Komunikacja dwukierunkowa jest wysyłane do serwera, gdy adres URL jest *przekierowanie*.

> [!WARNING]
> Należy zachować ostrożność podczas ustanawiania reguł przekierowania. Twoje zasady przekierowania są obliczane na każde żądanie do aplikacji, nawet po przekierowania. Łatwo przypadkowo spowodować powstanie pętli nieskończonej przekierowania.

Oryginalne żądanie: `/redirect-rule/1234/5678`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

Część numerowanie wyrażenia zawartgoe w nawiasach jest nazywany *grupa przechwytywania*. Kropka (`.`) wyrażenie oznacza, że *dopasowuje dowolny znak*. Gwiazdka (`*`) wskazuje *Dopasuj poprzedni znak zero lub więcej razy*. W związku z tym, segmenty ostatnie dwie ścieżki adresu URL, `1234/5678`, są przechwytywane przez grupę przechwytywania `(.*)`. Dowolna wartość należy podać w adresie URL żądania po `redirect-rule/` są przechwytywane przez tę grupę przechwytywania jednego.

W ciągu zamiennym przechwyconych grupach są wstrzykiwane do ciągu znakiem dolara (`$`) wraz z numerem sekwencji przechwytywania. Wartość pierwszej grupy przechwytywania są uzyskiwane z `$1`, druga z `$2`, i kontynuują w sekwencji dla grup przechwytywania w swojej wyrażenia regularnego. Tylko jedna grupa przechwycone z wyrażeniem regularnym reguła przekierowania w jest przykładowej aplikacji, więc tylko jedna grupa wprowadzonego w ciągu zamiennym, który jest `$1`. Po zastosowaniu reguły staje się adres URL `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Adres URL przekierowania do bezpiecznego punktu końcowego

Użyj `AddRedirectToHttps` Przekierowywanie żądań HTTP do tego samego hosta i ścieżkę przy użyciu protokołu HTTPS (`https://`). Jeśli nie został dostarczony kod stanu, oprogramowanie pośredniczące domyślnie 302 (Found). Jeśli port nie jest podany, oprogramowanie pośredniczące, wartość domyślna to `null`, co oznacza, że protokół zmieni się na `https://` i klient uzyskuje dostęp do zasobów na porcie 443. W przykładzie pokazano, jak ustawić kod stanu 301 (trwale przeniesiona) i zmień numer portu na 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Użyj `AddRedirectToHttpsPermanent` Aby przekierować niezabezpieczone żądania do tego samego hosta i ścieżkę z bezpiecznego protokołu HTTPS (`https://` na porcie 443). Oprogramowanie pośredniczące ustawia kod stanu 301 (trwale przeniesiona).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, zaleca się za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS. Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl#require-https) tematu.

Przykładowa aplikacja jest w stanie pokazuje sposób użycia studia `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`. Dodaj metodę rozszerzenia, aby `RewriteOptions`. Przesyłania niezabezpieczone żądania do aplikacji na dowolny adres URL. Odrzuć zabezpieczeń przeglądarki, ostrzeżenie, że nie jest zaufany certyfikat z podpisem własnym, lub Utwórz wyjątek, aby ufać certyfikatowi.

Oryginalne żądanie, używając `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

Oryginalne żądanie, używając `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Ponowne zapisywanie adresów URL

Użyj `AddRewrite` można utworzyć regułę ponownego zapisywania adresów URL. Pierwszy parametr zawiera Twoje wyrażenia regularnego do dopasowania na przychodzące Ścieżka adresu URL. Drugi parametr jest ciąg zastępujący. Trzeci parametr `skipRemainingRules: {true|false}`, wskazuje, aby oprogramowanie pośredniczące umożliwia określenie, czy pominąć reguły ponownego zapisywania dodatkowe, jeśli zastosowano bieżącej reguły.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

Oryginalne żądanie: `/rewrite-rule/1234/5678`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_rewrite.png)

Pierwszą rzeczą, którą można zauważyć, wyrażenie regularne jest pojawia się znak daszka (`^`) na początku wyrażenia. Oznacza to, że dopasowanie rozpoczyna się od początku ścieżki adresu URL.

We wcześniejszym przykładzie z regułą przekierowania `redirect-rule/(.*)`, nie ma żadnych daszka, na początku wyrażenia regularnego; w związku z tym, mogą poprzedzać wszystkie znaki `redirect-rule/` w ścieżce dla pomyślnego dopasowania.

| Ścieżka                               | Dopasowanie |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Tak   |
| `/my-cool-redirect-rule/1234/5678` | Tak   |
| `/anotherredirect-rule/1234/5678`  | Tak   |

Reguły ponownego pisania `^rewrite-rule/(\d+)/(\d+)`, tylko dopasowuje ścieżek, jeśli zaczyna `rewrite-rule/`. Należy zauważyć różnicę w dopasowywania między poniższe reguły ponownego pisania i reguła przekierowania powyżej.

| Ścieżka                              | Dopasowanie |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Tak   |
| `/my-cool-rewrite-rule/1234/5678` | Nie    |
| `/anotherrewrite-rule/1234/5678`  | Nie    |

Następujące `^rewrite-rule/` część wyrażenia, istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`. `\d` Oznacza *dopasowania cyfrę (numer)*. Znak plus (`+`) oznacza, że *zgodne z co najmniej jeden znak poprzedzający*. W związku z tym adres URL musi zawierać liczbę następuje ukośnikiem do przodu następuje inny numer. Przechwytywanie, te grupy są wstrzykiwane do nowych adresu URL jako `$1` i `$2`. Ciąg zastępujący reguły ponownego zapisywania umieszcza przechwyconych grupach w zmiennej querystring. Żądana ścieżka `/rewrite-rule/1234/5678` jest przepisany można uzyskać zasobu w `/rewritten?var1=1234&var2=5678`. Jeśli ciąg zapytania jest obecna na oryginalne żądanie, są zachowywane, gdy adres URL jest przepisany.

Nie ma żadnych obie strony do serwera można uzyskać zasobu. Jeśli zasób istnieje, ma pobrać i zwracany do klienta z kodem stanu 200 (OK). Ponieważ klient nie jest przekierowany, nie powoduje zmiany adresu URL w pasku adresu przeglądarki. O ile dotyczy to klient, nigdy nie operacji ponownego zapisywania adresu URL wystąpił.

> [!NOTE]
> Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguł dopasowania jest procesem kosztowne i skraca czas odpowiedzi aplikacji. Najszybszy odpowiedzi aplikacji:
> * Kolejność reguły ponownego zapisywania z najczęściej dopasowane reguły do najmniej często dopasowane reguły.
> * Pomiń przetwarzanie pozostałych reguł, gdy pasują do przetwarzania nie dodatkowe reguły jest wymagana.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Zastosuj reguły mod_rewrite Apache z `AddApacheModRewrite`. Upewnij się, że plik reguł jest wdrażany z aplikacją. Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

::: moniker range=">= aspnetcore-2.0"

A `StreamReader` służy do odczytywania reguły z *ApacheModRewrite.txt* pliku reguł.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pierwszy parametr przyjmuje `IFileProvider`, która jest oferowana w ramach [wstrzykiwanie zależności](dependency-injection.md). `IHostingEnvironment` Są wstrzykiwane do zapewnienia `ContentRootFileProvider`. Drugi parametr jest ścieżka do pliku reguł, który jest *ApacheModRewrite.txt* w przykładowej aplikacji.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`. Kod stanu odpowiedzi to 302 (Found).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Oryginalne żądanie: `/apache-mod-rules-redirect/1234`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

Oprogramowanie pośredniczące obsługuje następujące zmienne serwera Apache mod_rewrite:

* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* PROTOKÓŁ IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* CZAS
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Reguły moduł ponowne zapisywanie adresów URL usług IIS

Aby użyć reguł, które dotyczą moduł ponowne zapisywanie adresów URL usług IIS, należy użyć `AddIISUrlRewrite`. Upewnij się, że plik reguł jest wdrażany z aplikacją. Nie bezpośrednie oprogramowania pośredniczącego do zastosowania usługi *web.config* pliku podczas uruchamiania w systemie Windows Server w usługach IIS. Za pomocą programu IIS, reguły te powinny być przechowywane poza swojej *web.config* w celu uniknięcia konfliktów z modułem ponownego zapisywania usług IIS. Aby uzyskać więcej informacji i przykłady reguł moduł ponowne zapisywanie adresów URL usług IIS, zobacz [przy użyciu adresu Url Nadpisz modułu 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [odwołanie konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

::: moniker range=">= aspnetcore-2.0"

A `StreamReader` służy do odczytywania reguły z *IISUrlRewrite.xml* pliku reguł.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pierwszy parametr przyjmuje `IFileProvider`, podczas gdy drugi parametr jest ścieżka do pliku zasady XML, który jest *IISUrlRewrite.xml* w przykładowej aplikacji.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

Przykładowa aplikacja ponownie zapisuje żądań z `/iis-rules-rewrite/(.*)` do `/rewritten?id=$1`. Odpowiedź jest wysyłana do klienta z kodem stanu 200 (OK).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Oryginalne żądanie: `/iis-rules-rewrite/1234`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

Jeśli masz aktywne moduł ponowne zapisywanie adresów usług IIS przy użyciu skonfigurowanych reguł poziom serwera, które mogło mieć wpływ na aplikację w sposób niepożądane, możesz wyłączyć moduł ponowne zapisywanie adresów usług IIS dla aplikacji. Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nieobsługiwane funkcje

::: moniker range=">= aspnetcore-2.0"

Oprogramowanie pośredniczące zwolnione z platformą ASP.NET Core 2.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:

* Reguły ruchu wychodzącego
* Zmienne serwera niestandardowego
* Symboli wieloznacznych
* LogRewrittenUrl

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Oprogramowanie pośredniczące zwolnione z platformą ASP.NET Core 1.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:

* Globalne reguły
* Reguły ruchu wychodzącego
* Map ponownego zapisywania
* Akcja CustomResponse
* Zmienne serwera niestandardowego
* Symboli wieloznacznych
* Akcja: CustomResponse
* LogRewrittenUrl

::: moniker-end

#### <a name="supported-server-variables"></a>Zmienne serwera obsługiwane

Oprogramowanie pośredniczące obsługuje następujące zmienne serwera moduł ponowne zapisywanie adresów URL usług IIS:

* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> Możesz również uzyskać `IFileProvider` za pośrednictwem `PhysicalFileProvider`. Takie podejście może dostarczyć większą elastyczność dla lokalizacji usługi ponownego zapisywania plików reguły. Upewnij się, że pliki reguły ponownego zapisywania są wdrażane do serwera w ścieżce, których udzielasz.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Reguły oparte na metodzie

Użyj `Add(Action<RewriteContext> applyRule)` do zaimplementowania własnej logiki reguły w metodzie. `RewriteContext` Udostępnia `HttpContext` do użycia w metodzie. `context.Result` Określa, jak dodatkowe potok przetwarzania jest obsługiwane.

| context.Result                       | Akcja                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (ustawienie domyślne) | Kontynuować stosowanie reguł                                         |
| `RuleResult.EndResponse`             | Zatrzymaj stosowanie reguł i wysłania odpowiedzi                       |
| `RuleResult.SkipRemainingRules`      | Zaprzestanie stosowania reguły i wysyłać kontekstu do następnego oprogramowania pośredniczącego |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

Przykładowa aplikacja pokazuje metody, która przekierowuje żądania dla ścieżek, które kończy się *.xml*. Jeśli wykonasz żądanie typu `/file.xml`, nastąpi przekierowanie do `/xmlfiles/file.xml`. Kod stanu jest równa 301 (trwale przeniesiona). Dla przekierowania Musisz jawnie ustawić kod stanu odpowiedzi; w przeciwnym razie zwracany jest kod stanu 200 (OK) i Przekierowanie nie zostanie wykonana na komputerze klienckim.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Oryginalne żądanie: `/file.xml`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Na podstawie irule na podstawie reguł

Użyj `Add(IRule)` do zaimplementowania własnej logiki reguły w klasie, która pochodzi od klasy `IRule`. Za pomocą `IRule` zapewnia większą elastyczność na podejście oparte na metodzie reguły. Klasy pochodne mogą obejmować konstruktora, w którym możesz przekazać parametry `ApplyRule` metody.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

Wartości parametrów w przykładowej aplikacji dla `extension` i `newPath` są sprawdzane w celu spełnienia kilku warunków. `extension` Musi zawierać wartość, a wartość musi być *.png*, *.jpg*, lub *.gif*. Jeśli `newPath` nie jest prawidłowy, `ArgumentException` zgłaszany. Jeśli wykonasz żądanie typu *image.png*, nastąpi przekierowanie do `/png-images/image.png`. Jeśli wykonasz żądanie typu *image.jpg*, nastąpi przekierowanie do `/jpg-images/image.jpg`. Kod stanu jest równa 301 (przeniesione trwale), a `context.Result` jest ustawiona, aby zatrzymać przetwarzanie reguł i wysłania odpowiedzi.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Oryginalne żądanie: `/image.png`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla image.png](url-rewriting/_static/add_redirect_png_requests.png)

Oryginalne żądanie: `/image.jpg`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Przykłady wyrażeń regularnych

| Cel | Wyrażenie regularne ciągu &<br>Przykład dopasowania | Ciąg zastępujący &<br>Przykład danych wyjściowych |
| ---- | :-----------------------------: | :------------------------------------: |
| Zmodyfikuj ścieżkę do querystring | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Usuń ukośnika. | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Wymuszanie ukośnika | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Należy unikać ponownego zapisywania adresów określone żądania | `^(.*)(?<!\.axd)$` lub `^(?!.*\.axd$)(.*)$`<br>Tak: `/resource.htm`<br>Nie: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Rozmieszczanie segmenty adresu URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Zastąp segment adresu URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uruchamianie aplikacji](startup.md)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Wyrażenia regularne w .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Język wyrażeń regularnych — podręczny wykaz](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Za pomocą moduł ponowne zapisywanie adresów Url w wersji 2.0 (dla usług IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Odwołania do konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Forum moduł ponowne zapisywanie adresów URL usług IIS](https://forums.iis.net/1152.aspx)
* [Zachowaj proste Struktura adresu URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 ponownego zapisywania adresów URL porady i wskazówki](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Slash — lub nie ukośnika](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
