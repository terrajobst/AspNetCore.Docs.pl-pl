---
title: Adres URL ponowne zapisanie oprogramowania pośredniczącego w platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat adresu URL ponowne zapisywanie i przekierowywania z pośredniczącym ponowne zapisywanie adresów URL w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d3484e222c4412a427d086c1b71a12b81095ba72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276350"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Adres URL ponowne zapisanie oprogramowania pośredniczącego w platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Mikael Mengistu](https://github.com/mikaelm12)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Ponowne zapisywanie adresów URL jest czynnością modyfikowania żądania, których adresy URL na podstawie co najmniej jeden wstępnie zdefiniowanych reguł. Ponowne zapisywanie adresów URL powoduje abstrakcję między lokalizacje zasobów i ich adresów, tak aby lokalizacje i adresy nie są ściśle powiązane. Istnieje kilka scenariuszy, w których jest przydatna ponowne zapisywanie adresów URL:

* Przenoszenie lub zastępowanie tymczasowo lub trwale zasoby serwera przy zachowaniu stabilna lokalizatorów dla tych zasobów.
* Dzielenie żądania przetwarzania różnych aplikacji lub obszarów jedną aplikację.
* Usuwanie, dodając lub reorganizacja segmenty adresu URL na żądań przychodzących.
* Optymalizacja publiczne adresy URL do optymalizacji aparatu wyszukiwania (SEO).
* Pozwalające na używanie przyjaznych adresów URL publiczne, ułatwiające osobom prognozowania zawartość, którą znajdą, wykonując następujące łącze.
* Przekierowywanie żądań niezabezpieczonego do bezpiecznego punktów końcowych.
* Zapobieganie hotlinking obrazu.

Można zdefiniować reguły zmiana adresu URL na kilka sposobów, łącznie z wyrażenia regularnego Apache mod_rewrite modułu zasad, zasady przepisywania moduł usług IIS i przy użyciu reguły niestandardowej logiki. Ten dokument zawiera instrukcje dotyczące sposobu używania pośredniczącym ponowne zapisywanie adresów URL w aplikacji platformy ASP.NET Core ponowne zapisywanie adresów URL.

> [!NOTE]
> Ponowne zapisywanie adresów URL może zmniejszyć wydajność aplikacji. W przypadku, gdy jest to możliwe, należy ograniczyć liczba i złożoność reguł.

## <a name="url-redirect-and-url-rewrite"></a>Ponowne zapisywanie adresów URL przekierowania i adres URL

Różnica w treść między *adres URL przekierowania* i *ponowne zapisywanie adresów URL* może wydawać się niewielkie w pierwszym, ale ma istotny wpływ na zapewnianie zasobów na klientach. Ponowne zapisywanie adresów URL platformy ASP.NET Core w oprogramowaniu pośredniczącym jest w stanie spełniających potrzeby dla obu.

A *adres URL przekierowania* jest operacją po stronie klienta, w którym klient otrzymuje instrukcję do uzyskania dostępu do zasobu na inny adres. To wymaga przesłania danych do serwera. Adres URL przekierowania do klienta zwracany jest wyświetlany w pasku adresu przeglądarki, gdy klient wysyła żądanie nowego zasobu. 

Jeśli `/resource` jest *przekierowanie* do `/different-resource`, żądań klientów `/resource`. Serwer odpowiada, że klient Zażądaj zasobu pod adresem `/different-resource` z stan kodu wskazujący, że przekierowanie tymczasowych lub trwałych. Klient wykonuje żądanie nowego zasobu pod adresem URL przekierowania.

![Punkt końcowy usługi WebAPI tymczasowo zmieniono z wersji 1 (wersja 1) w wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w wersji 1 /v1/api ścieżki. Serwer wysyła ponownie odpowiedzi 302 (Found) z nowego, tymczasowego źródła ścieżkę dla usługi w wersji 2 /v2/api. Klient wysyła drugie żądanie do usługi pod adresem URL przekierowania. Serwer odpowiada, z kodem stanu 200 (OK).](url-rewriting/_static/url_redirect.png)

Przekierowywanie żądań do innego adresu URL, wskazuje, czy przekierowanie stałych lub tymczasowych. 301 (trwale przeniesiona) kod stanu jest używany gdzie zasób ma adres URL nowego, stałe i chcesz nakazać klienta, że wszystkie przyszłe żądania dla zasobu powinien używać nowego adresu URL. *Po odebraniu kod stanu 301, klient może buforować odpowiedzi.* Kod stanu 302 (Found) jest używany gdzie przekierowania jest tymczasowe lub ogólnie podmiotu można zmienić w taki sposób, że klient nie należy przechowywać i ponowne użycie adres URL przekierowania w przyszłości. Aby uzyskać więcej informacji, zobacz [RFC 2616: definicje kod stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *ponowne zapisywanie adresów URL* jest operacją po stronie serwera w celu zapewnienia zasobów z adresu innego zasobu. Ponowne zapisywanie adresów URL nie wymaga przesłania danych do serwera. Ponownie zapisane adres URL nie jest zwracana do klienta i nie będą wyświetlane na pasku adresu przeglądarki. Gdy `/resource` jest *ulegną* do `/different-resource`, żądań klientów `/resource`, a serwerem *wewnętrznie* pobiera zasobu pod adresem `/different-resource`. Chociaż przez klienta może być w stanie pobrać zasobu pod adresem URL ponownie zapisane, klient nie będzie informacja, czy zasób istnieje pod adresem URL ponownie zapisane wysyła żądania i odpowiedzi.

![Punkt końcowy usługi WebAPI zmianie z wersji 1 (wersja 1) w wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w wersji 1 /v1/api ścieżki. Adres URL żądania jest napisany od nowa do uzyskania dostępu do usługi w wersji 2 /v2/api ścieżki. Usługa odpowiada na kliencie z kodem stanu 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Adres URL przebudowywania przykładowej aplikacji

Można eksplorować funkcje pośredniczącym ponowne zapisywanie adresów URL z [adres URL przebudowywania Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Aplikacja stosuje ponownego zapisywania i Przekierowanie zasady i przedstawia nowych lub przekierowany adres URL.

## <a name="when-to-use-url-rewriting-middleware"></a>Kiedy należy używać pośredniczącym ponowne zapisywanie adresów URL

Użyj ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym, gdy nie można użyć [moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) z usługami IIS w systemie Windows Server [modułu mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) na serwerze Apache [na NginxponownezapisywanieadresówURL](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), lub aplikacji znajduje się na [serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)). Główne powody do użycia na serwerze adresu URL przebudowywania technologii w usługach IIS, Apache lub Nginx są czy oprogramowanie pośredniczące nie obsługuje wszystkich funkcji tych modułów i wydajności oprogramowania pośredniczącego prawdopodobnie nie będzie zgodny z modułów. Istnieją jednak niektóre funkcje modułów serwera, które nie działają z projektów platformy ASP.NET Core, takich jak `IsFile` i `IsDirectory` ograniczeń modułu ponownego zapisywania usług IIS. W tych scenariuszach w zamian użyj oprogramowania pośredniczącego.

## <a name="package"></a>Package

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pakietu. Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.

## <a name="extension-and-options"></a>Opcje i rozszerzenia

Ustanowić Twojej ponowne zapisywanie adresów URL i Przekierowanie reguł przez utworzenie wystąpienia `RewriteOptions` klasy z metody rozszerzenia dla poszczególnych reguł. Łańcucha wielu reguł w kolejności, że chcesz je przetworzyć. `RewriteOptions` Są przekazywane do oprogramowania pośredniczącego ponowne zapisywanie adresów URL, który jest dodawany do potoku żądania z `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

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

---

### <a name="url-redirect"></a>Adres URL przekierowania

Użyj `AddRedirect` Przekierowywanie żądań. Pierwszy parametr zawiera z wyrażenia regularnego do dopasowania w ścieżce przychodzącego adresu URL. Drugi parametr jest ciąg zastępczy. Trzeci parametr, jeśli jest obecny, określa kod stanu. Jeśli kod stanu nie jest określony, domyślnie 302 (Found), który wskazuje, że zasób jest tymczasowo przeniesiony lub zastąpione.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

W przeglądarce za pomocą narzędzia dla deweloperów włączone, Wyślij żądanie do przykładowej aplikacji ze ścieżką `/redirect-rule/1234/5678`. Wyrażenia regularnego zgodna ze ścieżką żądania na `redirect-rule/(.*)`, a ścieżka została zastąpiona `/redirected/1234/5678`. Przekieruj adres URL jest wysyłany do klienta z kodem stanu 302 (Found). Przeglądarka sprawia, że nowe żądanie pod adresem URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki. Ponieważ brak reguł w przykładowej aplikacji odpowiada na adres URL przekierowania, drugie żądanie odbiera odpowiedź 200 (OK) z aplikacji i treść odpowiedzi zawiera adres URL przekierowania. Obie strony jest kierowane do serwera, gdy adres URL jest *przekierowanie*.

> [!WARNING]
> Należy zachować ostrożność podczas ustanawiania reguł przekierowania. Reguły przekierowania są oceniane na każde żądanie do aplikacji, w tym nastąpiło przekierowanie. Łatwo jest przypadkowo pętlę nieskończoną przekierowania.

Oryginalne żądanie: `/redirect-rule/1234/5678`

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

Część wyrażenia umieszczone w nawiasach jest nazywana *grupy przechwytywania*. Kropka (`.`) wyrażenia oznacza *dopasowuje dowolny znak*. Gwiazdka (`*`) wskazuje *Dopasowuje poprzedni znak zero lub więcej razy*. W związku z tym segmenty dwa ostatnie ścieżki adresu URL, `1234/5678`, są przechwytywane przez grupę przechwytywania `(.*)`. Wartości podane w adresie URL żądania po `redirect-rule/` są przechwytywane przez tę grupę pojedynczego przechwytywania.

W ciągu zastępowania przechwyconej grupy są wstrzykiwane do ciągu z znak dolara (`$`) wraz z numerem sekwencji przechwytywania. Pierwsza wartość grupy przechwytywania są uzyskiwane z `$1`, druga z `$2`, i kontynuują w sekwencji dla grup przechwytywania w Twojej wyrażenia regularnego. Tylko jedna grupa przechwyconych regex reguły przekierowania w jest przykładowej aplikacji, tak aby tylko jedna grupa wprowadzony w ciągu zastępowania, która jest `$1`. Reguła jest stosowana, adres URL staje się `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Adres URL przekierowania do bezpiecznego punktu końcowego

Użyj `AddRedirectToHttps` przekierowywania żądań HTTP do tego samego hosta i ścieżkę przy użyciu protokołu HTTPS (`https://`). Jeśli kod stanu nie jest podany, oprogramowanie pośredniczące domyślnie 302 (Found). Jeśli port nie jest podany, oprogramowanie pośredniczące domyślnie `null`, co oznacza, że protokół zmienia się na `https://` i klient uzyskuje dostęp do zasobów na porcie 443. W przykładzie przedstawiono sposób ustawić kod stanu 301 (trwale przeniesiona) i zmień numer portu na 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Użyj `AddRedirectToHttpsPermanent` przekierowywania żądań niezabezpieczonych do tego samego hosta i ścieżki z bezpiecznego protokołu HTTPS (`https://` na porcie 443). Oprogramowanie pośredniczące ustawia kod stanu 301 (trwale przeniesiona).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS. Aby uzyskać więcej informacji, zobacz [wymusić HTTPS](xref:security/enforcing-ssl#require-https) tematu.

Przykładowa aplikacja jest w stanie pokazuje sposób użycia `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`. Dodaj metodę rozszerzenie do `RewriteOptions`. Wprowadź niezabezpieczonego żądania do aplikacji na dowolny adres URL. Odrzucić zabezpieczeń przeglądarki ostrzeżenie niezaufany certyfikat z podpisem własnym lub utworzyć wyjątek dotyczący ufać certyfikatowi.

Oryginalnego żądania przy użyciu `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

Oryginalnego żądania przy użyciu `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Ponowne zapisywanie adresów URL

Użyj `AddRewrite` Aby utworzyć regułę dla ponowne zapisywanie adresów URL. Pierwszy parametr zawiera z wyrażenia regularnego do dopasowania na ścieżkę przychodzącego adresu URL. Drugi parametr jest ciąg zastępczy. Trzeci parametr `skipRemainingRules: {true|false}`, wskazuje, aby oprogramowanie pośredniczące umożliwia określenie, czy pominąć reguły ponownego zapisywania dodatkowe, jeśli zastosowano bieżącej regule.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

Oryginalne żądanie: `/rewrite-rule/1234/5678`

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_rewrite.png)

Pierwszą rzeczą, którą można zauważyć, że w wyrażenia regularnego jest karatach (`^`) na początku wyrażenia. Oznacza to, że pasujące zaczyna się na początku ścieżki adresu URL.

W przypadku poprzedniego przykładu z regułą przekierowania `redirect-rule/(.*)`, na początku wyrażenia regularnego nie istnieją żadne karatach — w związku z tym może poprzedzać wszystkie znaki `redirect-rule/` w ścieżce dla pomyślnego dopasowania.

| Ścieżka                               | Dopasowanie |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Tak   |
| `/my-cool-redirect-rule/1234/5678` | Tak   |
| `/anotherredirect-rule/1234/5678`  | Tak   |

Reguła ponownego zapisywania `^rewrite-rule/(\d+)/(\d+)`, ścieżki zgodny tylko, jeśli zaczynają `rewrite-rule/`. Zwróć uwagę, różnica dopasowywania między poniżej reguły ponownego zapisywania i zasadę przekierowania powyżej.

| Ścieżka                              | Dopasowanie |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Tak   |
| `/my-cool-rewrite-rule/1234/5678` | Nie    |
| `/anotherrewrite-rule/1234/5678`  | Nie    |

Po `^rewrite-rule/` część wyrażenia, istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`. `\d` Oznacza *odpowiada cyfrę (numer)*. Znak plus (`+`) oznacza *zgodne z co najmniej jednego znaku poprzedzającego*. W związku z tym adres URL musi zawierać liczbę następuje następuje inny numer kreskami ukośnymi. Te przechwytywania grup są wstrzykiwane do nowych adresu URL jako `$1` i `$2`. Ciąg zastępczy reguły ponownego zapisywania umieszczenie grup przechwyconych w ciąg zapytania. Żądana ścieżka `/rewrite-rule/1234/5678` jest napisany od nowa można uzyskać zasobu pod adresem `/rewritten?var1=1234&var2=5678`. Jeśli ciąg zapytania jest obecna na oryginalne żądanie, jest to zachowane, gdy adres URL jest napisany od nowa.

Nie ma żadnych obie strony do serwera w celu uzyskania zasobu. Jeśli zasób istnieje, ma pobrane i zwracany do klienta z kodem 200 stanu (OK). Ponieważ klient nie jest przekierowany, nie powoduje zmiany adresu URL na pasku adresu przeglądarki. Ile dotyczy to klient, wystąpił nigdy nie operacji ponowne zapisywanie adresów URL.

> [!NOTE]
> Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguł dopasowywania jest procesem kosztowne i skraca czas odpowiedzi aplikacji. Najszybszym odpowiedzi aplikacji:
> * Kolejność reguły ponownego zapisywania z reguły najczęściej dopasowane do najmniej często dopasowane reguły.
> * Pomiń przetwarzanie pozostałych reguł, gdy pasują do i nie dodatkowe reguły przetwarzania jest wymagane.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Zastosuj reguły mod_rewrite Apache z `AddApacheModRewrite`. Upewnij się, że plik reguł jest wdrażany z aplikacją. Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

A `StreamReader` jest używany do odczytu reguły z *ApacheModRewrite.txt* pliku reguł.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Pierwszy parametr przyjmuje `IFileProvider`, które zostaną przekazane za pośrednictwem [iniekcji zależności](dependency-injection.md). `IHostingEnvironment` Jest wprowadzonym w celu zapewnienia `ContentRootFileProvider`. Drugi parametr jest ścieżka do pliku reguł, który jest *ApacheModRewrite.txt* w przykładowej aplikacji.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`. Kod stanu odpowiedzi jest 302 (Found).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Oryginalne żądanie: `/apache-mod-rules-redirect/1234`

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Zmienne serwera obsługiwanych

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

Aby użyć reguł, które dotyczą moduł ponowne zapisywanie adresów URL usług IIS, użyj `AddIISUrlRewrite`. Upewnij się, że plik reguł jest wdrażany z aplikacją. Nie bezpośrednie oprogramowaniu pośredniczącym, aby korzystać z *web.config* pliku podczas uruchamiania w systemie Windows Server IIS. Z programem IIS, zasady te powinny być przechowywane poza Twojej *web.config* w celu uniknięcia konfliktów z modułem ponownego zapisywania usług IIS. Aby uzyskać więcej informacji i przykłady reguł moduł ponowne zapisywanie adresów URL usług IIS, zobacz [przy użyciu adresu Url przepisywania moduł 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [odwołania konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

A `StreamReader` jest używany do odczytu reguły z *IISUrlRewrite.xml* pliku reguł.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Pierwszy parametr przyjmuje `IFileProvider`, a drugi parametr jest ścieżka do pliku reguł XML, który jest *IISUrlRewrite.xml* w przykładowej aplikacji.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

Przykładowa aplikacja ponownie zapisuje żądań z `/iis-rules-rewrite/(.*)` do `/rewritten?id=$1`. Odpowiedź jest wysyłana do klienta z kodem stanu 200 (OK).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Oryginalne żądanie: `/iis-rules-rewrite/1234`

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

Jeśli masz aktywnego modułu ponownego zapisywania usług IIS skonfigurowanych reguł poziomu serwera, które będzie mieć wpływ aplikacji w sposób niepożądanych można wyłączyć moduł ponowne zapisywanie adresów usług IIS dla aplikacji. Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nieobsługiwane funkcje

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Oprogramowanie pośredniczące zwolnione z platformy ASP.NET Core 2.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:

* Reguły ruchu wychodzącego
* Zmienne serwera niestandardowego
* Symbole wieloznaczne
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Oprogramowanie pośredniczące zwolnione z platformy ASP.NET Core 1.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:

* Globalne zasady
* Reguły ruchu wychodzącego
* Ponownego zapisywania map
* Akcja CustomResponse
* Zmienne serwera niestandardowego
* Symbole wieloznaczne
* Akcja: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Zmienne serwera obsługiwanych

Oprogramowanie pośredniczące obsługuje następujące zmienne serwera moduł ponowne zapisywanie adresów URL usług IIS:

* CONTENT_LENGTH
* TYP_ZAWARTOŚCI
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
> Możesz również uzyskać `IFileProvider` za pośrednictwem `PhysicalFileProvider`. Takie podejście może udostępnić większą elastyczność lokalizacji Twojej ponownego zapisywania plików reguł. Upewnij się, że reguły ponownego zapisywania plików są wdrażane do serwera w ścieżce podane przez użytkownika.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Reguły na podstawie — metoda

Użyj `Add(Action<RewriteContext> applyRule)` implementacji logiki reguły w metodzie. `RewriteContext` Przedstawia `HttpContext` do użycia w metodę. `context.Result` Określa, jak dodatkowe potoku przetwarzania jest obsługiwane.

| context.Result                       | Akcja                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (ustawienie domyślne) | Kontynuować stosowanie reguł                                         |
| `RuleResult.EndResponse`             | Zaprzestanie stosowania reguły i wysyłania odpowiedzi                       |
| `RuleResult.SkipRemainingRules`      | Zaprzestanie stosowania reguły i wysyłać kontekstu do następnego oprogramowania pośredniczącego |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

Przykładowa aplikacja przedstawiono metodę, która przekierowuje żądania dla ścieżek czy kończą się *.xml*. Jeśli żądanie `/file.xml`, nastąpi przekierowanie do `/xmlfiles/file.xml`. Kod stanu jest ustawiona na 301 (trwale przeniesiona). Dla przekierowania Musisz jawnie ustawić kod stanu odpowiedzi; w przeciwnym razie zwracany jest kod stanu 200 (OK) i Przekierowanie nie występują na komputerze klienckim.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Oryginalne żądanie: `/file.xml`

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla file.xml narzędzia dla deweloperów](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Reguły na podstawie IRule

Użyj `Add(IRule)` implementacji logiki reguły w klasie, która jest pochodną `IRule`. Przy użyciu `IRule` zapewnia większą elastyczność w przypadku metody oparte na metodzie reguły. Klasy pochodne mogą obejmować konstruktora, w którym można przekazać w parametrach `ApplyRule` metody.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

Wartości parametrów w Przykładowa aplikacja dla `extension` i `newPath` są sprawdzane w celu spełnienia kilku warunków. `extension` Musi zawierać wartość, a wartość musi być *.png*, *.jpg*, lub *.gif*. Jeśli `newPath` nie jest prawidłowy, `ArgumentException` jest generowany. Jeśli żądanie *image.png*, nastąpi przekierowanie do `/png-images/image.png`. Jeśli żądanie *image.jpg*, nastąpi przekierowanie do `/jpg-images/image.jpg`. Kod stanu ma ustawioną wartość 301 (przenieść trwale) i `context.Result` ustawiono Zatrzymaj przetwarzanie reguł i wysyłania odpowiedzi.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Oryginalne żądanie: `/image.png`

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla image.png narzędzia dla deweloperów](url-rewriting/_static/add_redirect_png_requests.png)

Oryginalne żądanie: `/image.jpg`

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla image.jpg narzędzia dla deweloperów](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Przykłady wyrażeń regularnych

| Cel | Ciąg wyrażenia regularnego &<br>Przykład dopasowania | Ciąg zastępczy &<br>Przykład danych wyjściowych |
| ---- | :-----------------------------: | :------------------------------------: |
| Ścieżka Napisz ponownie w ciągu kwerendy | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Usuwanie ukośnika | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Wymuszanie ukośnika | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Unikaj ponowne zapisywanie określone żądania | `^(.*)(?<!\.axd)$` lub `^(?!.*\.axd$)(.*)$`<br>Tak: `/resource.htm`<br>Nie: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Rozmieszczanie segmenty adresu URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Zastąp segment adresu URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uruchamianie aplikacji](startup.md)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Wyrażenia regularne w .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Język wyrażeń regularnych — podręczny wykaz](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Za pomocą modułu ponowne zapisywanie adresów Url 2.0 (dla usług IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Odwołania do konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Forum moduł ponowne zapisywanie adresów URL usług IIS](https://forums.iis.net/1152.aspx)
* [Zachowaj prostą strukturę adresu URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [Ponowne zapisywanie adresów URL 10 porady i wskazówki](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Ukośnika lub ukośnika](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
