---
title: Ponowne zapisywanie przez adres URL oprogramowania pośredniczącego w ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej na temat zapisywania i przekierowywania adresów URL przy użyciu oprogramowania pośredniczącego do ponownego zapisywania adresów URL w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/16/2019
uid: fundamentals/url-rewriting
ms.openlocfilehash: 7d63cf381f1d8a19ed4fb789348e36f94304ad63
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666468"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Ponowne zapisywanie przez adres URL oprogramowania pośredniczącego w ASP.NET Core

Autor [Mikael Mengistu](https://github.com/mikaelm12)

::: moniker range=">= aspnetcore-3.0"

Ten dokument zawiera wprowadzenie do ponownego zapisywania adresów URL z instrukcjami dotyczącymi sposobu korzystania z programu pośredniczącego do ponownego zapisywania adresów URL w aplikacjach ASP.NET Core.

Ponowne zapisywanie adresu URL to czynność modyfikacji adresów URL żądań na podstawie co najmniej jednej wstępnie zdefiniowanej reguły. Ponowne zapisywanie adresów URL powoduje utworzenie abstrakcji między lokalizacjami zasobów a ich adresami, dzięki czemu lokalizacje i adresy nie są ściśle połączone. Ponowne zapisywanie adresów URL jest cenne w kilku scenariuszach:

* Przenoszenie lub zastępowanie zasobów serwera tymczasowo lub trwale i utrzymuje stałe lokalizatory dla tych zasobów.
* Podziel przetwarzanie żądań między różne aplikacje lub w obszarach jednej aplikacji.
* Usuwanie, Dodawanie lub organizowanie segmentów adresów URL w żądaniach przychodzących.
* Optymalizuj publiczne adresy URL dla optymalizacji aparatu wyszukiwania (wyszukiwarka).
* Zezwól na używanie przyjaznych publicznych adresów URL, aby ułatwić odwiedzającym zapowiadanie zawartości zwróconej przez żądanie zasobu.
* Przekieruj niezabezpieczone żądania do bezpiecznych punktów końcowych.
* Zapobiegaj hotlinking, gdzie lokacja zewnętrzna używa hostowanego zasobu statycznego w innej lokacji przez połączenie elementu zawartości z własną zawartością.

> [!NOTE]
> Ponowne zapisywanie adresów URL może zmniejszyć wydajność aplikacji. Jeśli to możliwe, należy ograniczyć liczbę i złożoność reguł.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="url-redirect-and-url-rewrite"></a>Przekierowywanie adresów URL i ponowne zapisywanie adresów URL

Różnica między *przekierowaniami adresów URL* i *przepisaniem adresów URL* jest subtelna, ale ma ważne konsekwencje dla udostępniania zasobów klientom. Ponowne zapisywanie oprogramowania pośredniczącego w usłudze ASP.NET Core jest w stanie sprostać potrzebom obu tych elementów.

*Przekierowanie adresu URL* obejmuje operację po stronie klienta, w której klient otrzymuje dostęp do zasobu pod innym adresem niż pierwotnie żądany klient. Wymaga to przeprowadzenia rundy na serwerze. Adres URL przekierowania zwracany do klienta pojawia się na pasku adresu przeglądarki, gdy klient wysyła nowe żądanie dla zasobu.

Jeśli `/resource` zostanie *przekierowana* do `/different-resource`, serwer odpowie, że klient powinien uzyskać zasób w `/different-resource` przy użyciu kodu stanu wskazującego, że przekierowanie jest tymczasowe lub trwałe.

![Punkt końcowy usługi WebAPI został tymczasowo zmieniony z wersji 1 (v1) do wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w ścieżce w wersji 1/v1/API. Serwer wysyła odpowiedź 302 (znalezioną) z nową ścieżką tymczasową dla usługi w wersji 2/v2/API. Klient wysyła drugie żądanie do usługi przy użyciu adresu URL przekierowania. Serwer reaguje na kod stanu 200 (OK).](url-rewriting/_static/url_redirect.png)

Podczas przekierowywania żądań do innego adresu URL wskaż, czy przekierowanie jest trwałe, czy tymczasowe, określając kod stanu z odpowiedzią:

* *301-przesunięty* kod stanu jest używany, gdy zasób ma nowy, stały adres URL i chcesz wydać klientowi, że wszystkie przyszłe żądania dla zasobu powinny używać nowego adresu URL. *Klient może buforować i ponownie używać odpowiedzi po odebraniu kodu stanu 301.*

* Kod stanu *znaleziony przez 302* jest używany, gdy przekierowywanie jest tymczasowe lub zwykle może ulec zmianie. Kod stanu 302 wskazuje na klienta, aby nie przechowywać adresu URL i używać go w przyszłości.

Aby uzyskać więcej informacji na temat kodów stanu, zobacz [RFC 2616: definicje kodów stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Ponowne *Zapisywanie adresów URL* jest operacją po stronie serwera, która dostarcza zasób z innego adresu zasobu niż żądany klient. Ponowne zapisywanie adresu URL nie wymaga przejazdu na serwer. Zwrotny adres URL nie jest zwracany do klienta i nie jest wyświetlany na pasku adresu przeglądarki.

Jeśli `/resource` jest *zapisywana* w `/different-resource`, serwer *wewnętrznie* pobiera i zwraca zasób w `/different-resource`.

Mimo że klient może być w stanie pobrać zasób przy zapisywanym adresie URL, klient nie ma informacji o tym, że zasób istnieje pod adresem URL, gdy wysyła żądanie i otrzymuje odpowiedź.

![Punkt końcowy usługi WebAPI został zmieniony z wersji 1 (v1) na wersję 2 (v2) na serwerze. Klient wysyła żądanie do usługi w ścieżce w wersji 1/v1/API. Adres URL żądania zostanie ponownie zapisany w celu uzyskania dostępu do usługi w ścieżce w wersji 2/v2/API. Usługa reaguje na klienta z kodem stanu 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Przykładowa aplikacja do ponownego zapisywania adresów URL

Możesz zapoznać się z funkcjami zapisywania oprogramowania pośredniczącego za pomocą [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Aplikacja stosuje reguły przekierowania i ponownego zapisywania oraz pokazuje przekierowany lub ponownie zapisany adres URL dla kilku scenariuszy.

## <a name="when-to-use-url-rewriting-middleware"></a>Kiedy używać oprogramowania pośredniczącego ponownego zapisywania adresów URL

Używaj ponownego zapisywania adresów URL, gdy nie możesz użyć następujących metod:

* [Moduł ponownego zapisywania adresów URL z usługami IIS w systemie Windows Server](https://www.iis.net/downloads/microsoft/url-rewrite)
* [Moduł Apache mod_rewrite na serwerze Apache](https://httpd.apache.org/docs/2.4/rewrite/)
* [Ponowne zapisywanie adresów URL w witrynie Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

Należy również użyć oprogramowania pośredniczącego, gdy aplikacja jest hostowana na [serwerze HTTP. sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanej webListener).

Główne przyczyny używania technologii zapisywania adresów URL opartych na serwerze w usługach IIS, Apache i Nginx są następujące:

* Oprogramowanie pośredniczące nie obsługuje pełnych funkcji tych modułów.

  Niektóre funkcje modułów serwera nie współpracują z projektami ASP.NET Core, takimi jak `IsFile` i `IsDirectory` ograniczenia modułu ponownego zapisywania usług IIS. W tych scenariuszach zamiast tego należy użyć oprogramowania pośredniczącego.
* Wydajność oprogramowania pośredniczącego prawdopodobnie nie jest zgodna z tymi modułami.

  Testy porównawcze są jedynym sposobem, aby wiedzieć, że metoda obniża wydajność w najbardziej lub niewielkim stopniu wydajności.

## <a name="package"></a>Pakiet

Oprogramowanie pośredniczące ponownego zapisywania adresów URL jest dostarczane przez pakiet [Microsoft. AspNetCore. Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) , który jest niejawnie uwzględniony w aplikacjach ASP.NET Core.

## <a name="extension-and-options"></a>Rozszerzenie i opcje

Ustanów reguły ponownego zapisywania i przekierowywania adresów URL, tworząc wystąpienie klasy [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) z metodami rozszerzającymi dla każdej z reguł ponownego zapisywania. Łączenie wielu reguł w kolejności, w jakiej mają być przetwarzane. `RewriteOptions` są przenoszone do adresu URL ponownego zapisywania oprogramowania pośredniczącego, które zostało dodane do potoku żądania z <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a>Przekieruj do sieci Web inne niż www

Trzy opcje umożliwiają aplikacji Przekierowywanie żądań innych niż`www` do `www`:

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; trwale przekierować żądanie do domeny podrzędnej `www`, jeśli żądanie jest inne niż`www`. Przekierowuje kod stanu [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) .

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; przekierować żądanie do domeny podrzędnej `www`, jeśli żądanie przychodzące nie jest`www`. Przekierowuje kod stanu [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) . Przeciążenie umożliwia podanie kodu stanu odpowiedzi. Użyj pola klasy <xref:Microsoft.AspNetCore.Http.StatusCodes>, aby przypisać kod stanu.

### <a name="url-redirect"></a>Przekierowywanie adresów URL

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*>, aby przekierować żądania. Pierwszy parametr zawiera wyrażenie regularne do dopasowania na ścieżce przychodzącego adresu URL. Drugi parametr jest ciągiem zamiennym. Trzeci parametr, jeśli obecny, określa kod stanu. Jeśli nie określisz kodu stanu, kod stanu zostanie zmieniony na *302-znaleziono*, co oznacza, że zasób jest tymczasowo przenoszony lub zastępowany.

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

W przeglądarce z włączonymi narzędziami deweloperskimi wykonaj żądanie do przykładowej aplikacji z ścieżką `/redirect-rule/1234/5678`. Wyrażenie regularne jest zgodne ze ścieżką żądania w `redirect-rule/(.*)`, a ścieżka jest zastępowana `/redirected/1234/5678`. Adres URL przekierowania jest wysyłany z powrotem do klienta z kodem stanu *znalezionym przez 302* . Przeglądarka tworzy nowe żądanie w adresie URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki. Ponieważ w adresie URL przekierowania nie ma reguł pasujących do przykładowej aplikacji:

* Drugie żądanie odbiera odpowiedź *200-OK* z aplikacji.
* Treść odpowiedzi zawiera adres URL przekierowania.

Po *przekierowaniu*adresu URL do serwera zostanie przeprowadzona runda.

> [!WARNING]
> Należy zachować ostrożność podczas ustanawiania reguł przekierowań. Reguły przekierowania są oceniane dla każdego żądania do aplikacji, w tym po przekierowaniu. Można łatwo przypadkowo utworzyć *pętlę nieskończonych przekierowań*.

Oryginalne żądanie: `/redirect-rule/1234/5678`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

Część wyrażenia zawartego w nawiasach jest nazywana *grupą przechwytywania*. Kropka (`.`) wyrażenia oznacza *dopasowanie dowolnego znaku*. Gwiazdka (`*`) oznacza *Dopasowanie znaku poprzedzającego zero lub więcej razy*. W związku z tym, ostatnie dwa segmenty ścieżki adresu URL, `1234/5678`, są przechwytywane przez `(.*)`grupy przechwytywania. Każda wartość podaną w adresie URL żądania po `redirect-rule/` zostanie przechwycona przez tę pojedynczą grupę przechwytywania.

W ciągu zamiennym przechwycone grupy są wstawiane do ciągu z znakiem dolara (`$`), a następnie numerem sekwencyjnym przechwytywania. Pierwsza wartość grupy przechwytywania jest uzyskiwana z `$1`, drugi z `$2`i kontynuuje sekwencję dla grup przechwytywania w wyrażeniach regularnych. W aplikacji przykładowej można wykonać tylko jedną przechwyconą grupę w postaci wyrażenia regularnego z regułą przekierowania, więc w ciągu zamiennym występuje tylko jedna grupa wstrzykiwana, która jest `$1`. Gdy reguła zostanie zastosowana, adres URL zostanie `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Przekierowanie adresu URL do bezpiecznego punktu końcowego

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*>, aby przekierować żądania HTTP do tego samego hosta i ścieżki przy użyciu protokołu HTTPS. Jeśli nie podano kodu stanu, oprogramowanie pośredniczące domyślnie zostanie *znalezione na 302*. Jeśli port nie jest podany:

* Ustawienia domyślne oprogramowania pośredniczącego `null`.
* Schemat zostanie zmieniony na `https` (protokół HTTPS), a klient uzyskuje dostęp do zasobu na porcie 443.

Poniższy przykład pokazuje, jak ustawić kod stanu na *301 — trwale przeniesiony* i zmienić port na 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*>, aby przekierować żądania niezabezpieczone do tego samego hosta i ścieżki z bezpiecznym protokołem HTTPS na porcie 443. Oprogramowanie pośredniczące ustawia kod stanu na *301 — trwale przeniesiony*.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Podczas przekierowywania do bezpiecznego punktu końcowego bez wymagania dla dodatkowych reguł przekierowywania zalecamy używanie oprogramowania pośredniczącego do przekierowania protokołu HTTPS. Aby uzyskać więcej informacji, zobacz temat [Wymuś https](xref:security/enforcing-ssl#require-https) .

Przykładowa aplikacja jest w stanie demonstrować, jak używać `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`. Dodaj metodę rozszerzenia do `RewriteOptions`. Wprowadź niezabezpieczone żądanie do aplikacji pod dowolnym adresem URL. Odrzuć ostrzeżenie o zabezpieczeniach przeglądarki, że certyfikat z podpisem własnym jest niezaufany lub Utwórz wyjątek, aby zaufać certyfikatowi.

Oryginalne żądanie przy użyciu `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

Oryginalne żądanie przy użyciu `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Regenerowanie adresów URL

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*>, aby utworzyć regułę ponownego zapisywania adresów URL. Pierwszy parametr zawiera wyrażenie regularne do dopasowania w przychodzącej ścieżce adresu URL. Drugi parametr jest ciągiem zamiennym. Trzeci parametr, `skipRemainingRules: {true|false}`, wskazuje na oprogramowanie pośredniczące, niezależnie od tego, czy pominięcia dodatkowych reguł ponownego zapisywania w przypadku zastosowania bieżącej reguły.

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

Oryginalne żądanie: `/rewrite-rule/1234/5678`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądania i odpowiedzi](url-rewriting/_static/add_rewrite.png)

Daszek (`^`) na początku wyrażenia oznacza, że dopasowanie rozpoczyna się na początku ścieżki URL.

W poprzednim przykładzie z regułą przekierowania `redirect-rule/(.*)`nie ma daszka (`^`) na początku wyrażenia regularnego. W związku z tym wszystkie znaki mogą poprzedzać `redirect-rule/` w ścieżce, aby pomyślnie dopasować.

| Ścieżka                               | Dopasowanie |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Yes   |
| `/my-cool-redirect-rule/1234/5678` | Yes   |
| `/anotherredirect-rule/1234/5678`  | Yes   |

Reguła ponownego zapisywania `^rewrite-rule/(\d+)/(\d+)`, jeśli zaczyna się od `rewrite-rule/`, dopasowuje tylko ścieżki. W poniższej tabeli należy zwrócić uwagę na różnicę.

| Ścieżka                              | Dopasowanie |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Yes   |
| `/my-cool-rewrite-rule/1234/5678` | Nie    |
| `/anotherrewrite-rule/1234/5678`  | Nie    |

Po `^rewrite-rule/` części wyrażenia istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`. `\d` oznacza *dopasowanie cyfry (Number)* . Znak plusa (`+`) oznacza *dopasowanie co najmniej jednego znaku poprzedzającego*. W związku z tym adres URL musi zawierać numer, po którym następuje ukośnik, po którym następuje kolejny numer. Te grupy przechwytywania są wstrzykiwane do zarejestrowanego adresu URL jako `$1` i `$2`. Ciąg zastępczy reguły ponownego zapisu umieszcza przechwycone grupy w ciągu zapytania. Żądana ścieżka `/rewrite-rule/1234/5678` została zapisywana w celu uzyskania zasobu w `/rewritten?var1=1234&var2=5678`. Jeśli ciąg zapytania jest obecny w oryginalnym żądaniu, jest zachowywany, gdy adres URL zostanie ponownie zapisany.

Serwer nie może uzyskać dostępu do zasobów. Jeśli zasób istnieje, jest pobierany i zwracany do klienta przy użyciu kodu stanu *200-OK* . Ponieważ klient nie jest przekierowywany, adres URL na pasku adresu przeglądarki nie jest zmieniany. Klienci nie mogą wykryć, czy na serwerze wystąpiła operacja ponownego zapisywania adresu URL.

> [!NOTE]
> Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguły uzgadniania są wyliczane i zwiększają czas odpowiedzi aplikacji. Dla najszybszej odpowiedzi aplikacji:
>
> * Zamów reguły ponownego zapisu z najczęściej dopasowanej reguły do najmniej często dopasowanej reguły.
> * Pomiń przetwarzanie pozostałych reguł w przypadku wystąpienia dopasowania i nie jest wymagane żadne dodatkowe przetwarzanie reguły.

### <a name="apache-mod_rewrite"></a>Mod_rewrite Apache

Zastosuj reguły mod_rewrite Apache z <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>. Upewnij się, że plik reguł został wdrożony razem z aplikacją. Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

<xref:System.IO.StreamReader> jest używany do odczytywania reguł z pliku reguł *ApacheModRewrite. txt* :

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`. Kod stanu odpowiedzi to *302 — znaleziono*.

[!code[](url-rewriting/samples/3.x/SampleApp/ApacheModRewrite.txt)]

Oryginalne żądanie: `/apache-mod-rules-redirect/1234`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

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
* IPV6
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

### <a name="iis-url-rewrite-module-rules"></a>Reguły modułu ponownego zapisywania adresów URL usług IIS

Aby użyć tego samego zestawu reguł, który ma zastosowanie do modułu ponowne zapisywanie adresów URL usług IIS, użyj <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>. Upewnij się, że plik reguł został wdrożony razem z aplikacją. Nie należy kierować oprogramowanie pośredniczące do korzystania z pliku *Web. config* aplikacji podczas uruchamiania w usługach IIS systemu Windows Server. W przypadku usług IIS te reguły powinny być przechowywane poza plikiem *Web. config* aplikacji, aby uniknąć konfliktów z modułem ponownego zapisywania usług IIS. Aby uzyskać więcej informacji i przykłady reguł modułu ponownego zapisywania adresów URL usług IIS, zobacz [using URL Rewrite module 2,0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [informacje konfiguracyjne modułu ponownego zapisywania adresu URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

<xref:System.IO.StreamReader> jest używany do odczytywania reguł z pliku reguł *IISUrlRewrite. XML* :

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

Przykładowa aplikacja ponownie zapisuje żądania z `/iis-rules-rewrite/(.*)`, aby `/rewritten?id=$1`. Odpowiedź jest wysyłana do klienta z kodem stanu *200-OK* .

[!code-xml[](url-rewriting/samples/3.x/SampleApp/IISUrlRewrite.xml)]

Oryginalne żądanie: `/iis-rules-rewrite/1234`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądania i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

Jeśli masz aktywny moduł ponownego zapisywania usług IIS z skonfigurowanymi regułami na poziomie serwera, które byłyby wpływać na aplikację na niepożądane sposoby, możesz wyłączyć moduł ponownego zapisywania usług IIS dla aplikacji. Aby uzyskać więcej informacji, zobacz [wyłączanie modułów IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nieobsługiwane funkcje

Oprogramowanie pośredniczące wydane z ASP.NET Core 2. x nie obsługuje następujących funkcji modułu ponownego zapisywania adresów URL usług IIS:

* Reguły ruchu wychodzącego
* Niestandardowe zmienne serwera
* Symbole wieloznaczne
* LogRewrittenUrl

#### <a name="supported-server-variables"></a>Obsługiwane Zmienne serwera

Oprogramowanie pośredniczące obsługuje następujące zmienne serwera modułu ponownego zapisywania adresów URL usług IIS:

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
> Możesz również uzyskać <xref:Microsoft.Extensions.FileProviders.IFileProvider> za pośrednictwem <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>. Takie podejście może zapewnić większą elastyczność lokalizacji plików reguł ponownego zapisywania. Upewnij się, że pliki reguł ponownego zapisywania są wdrożone na serwerze pod podaną ścieżką.
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Reguła oparta na metodzie

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*>, aby zaimplementować własną logikę reguł w metodzie. `Add` uwidacznia <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, które udostępnia <xref:Microsoft.AspNetCore.Http.HttpContext> do użycia w metodzie. [RewriteContext. Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) określa sposób obsługi dodatkowego przetwarzania potoku. Ustaw wartość na jedno z <xref:Microsoft.AspNetCore.Rewrite.RuleResult> pól opisanych w poniższej tabeli.

| `RewriteContext.Result`              | Akcja                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| `RuleResult.ContinueRules` (wartość domyślna) | Kontynuuj stosowanie reguł.                                         |
| `RuleResult.EndResponse`             | Zatrzymaj stosowanie reguł i Wyślij odpowiedź.                       |
| `RuleResult.SkipRemainingRules`      | Zatrzymaj stosowanie reguł i Wyślij kontekst do następnego oprogramowania pośredniczącego. |

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

Przykładowa aplikacja przedstawia metodę, która przekierowuje żądania dla ścieżek kończących się na *. XML*. Jeśli zostanie wysłane żądanie `/file.xml`, żądanie jest przekierowywane do `/xmlfiles/file.xml`. Kod stanu jest ustawiony na *301 — trwale przeniesiony*. Gdy przeglądarka wykonuje nowe żądanie dla */XmlFiles/File.XML*, oprogramowanie pośredniczące plików statycznych zachowuje ten plik na kliencie z folderu *wwwroot/XmlFiles* . W przypadku przekierowania jawnie ustaw kod stanu odpowiedzi. W przeciwnym razie zwracany jest kod stanu *200-OK* i przekierowanie nie wystąpi na kliencie.

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

Takie podejście może również ponownie zapisywać żądania. Przykładowa aplikacja pokazuje, jak ponownie napisać ścieżkę do dowolnego żądania pliku tekstowego, aby zapewnić obsługę pliku tekstowego *pliku. txt* z folderu *wwwroot* . Oprogramowanie pośredniczące plików statycznych obsługuje plik na podstawie zaktualizowanej ścieżki żądania:

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a>Reguła oparta na IRule

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*>, aby użyć logiki reguły w klasie implementującej interfejs <xref:Microsoft.AspNetCore.Rewrite.IRule>. `IRule` zapewnia większą elastyczność w porównaniu z użyciem metody opartej na metodzie. Klasa implementacji może zawierać konstruktora, który umożliwia przekazywanie parametrów dla metody <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*>.

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

Wartości parametrów w przykładowej aplikacji dla `extension` i `newPath` są sprawdzane pod kątem spełnienia kilku warunków. `extension` musi zawierać wartość, a wartość musi mieć *Format PNG*, *jpg*lub *GIF*. Jeśli `newPath` jest nieprawidłowy, zostanie zgłoszony <xref:System.ArgumentException>. Jeśli zostanie wysłane żądanie pliku *Image. png*, żądanie jest przekierowywane do `/png-images/image.png`. Jeśli żądanie jest tworzone dla *obrazu. jpg*, żądanie jest przekierowywane do `/jpg-images/image.jpg`. Kod stanu jest ustawiony na *301 — trwale przesunięty*, a `context.Result` jest ustawiony tak, aby zatrzymać przetwarzanie reguł i wysłać odpowiedź.

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

Oryginalne żądanie: `/image.png`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi dla pliku Image. png](url-rewriting/_static/add_redirect_png_requests.png)

Oryginalne żądanie: `/image.jpg`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi dla obrazu. jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Przykłady wyrażeń regularnych

| Cel | Ciąg wyrażenia regularnego &<br>Przykład dopasowania | & Ciągu zamiennego<br>Przykład danych wyjściowych |
| ---- | ------------------------------- | -------------------------------------- |
| Zapisz ścieżkę do ciągu QueryString | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Ukośnik końcowy na pasku | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Wymuszaj końcowy ukośnik | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Unikaj ponownego zapisywania określonych żądań | `^(.*)(?<!\.axd)$` lub `^(?!.*\.axd$)(.*)$`<br>Tak: `/resource.htm`<br>Nie: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Zmień rozmieszczenie segmentów adresu URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Zastępowanie segmentu adresu URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ten dokument zawiera wprowadzenie do ponownego zapisywania adresów URL z instrukcjami dotyczącymi sposobu korzystania z programu pośredniczącego do ponownego zapisywania adresów URL w aplikacjach ASP.NET Core.

Ponowne zapisywanie adresu URL to czynność modyfikacji adresów URL żądań na podstawie co najmniej jednej wstępnie zdefiniowanej reguły. Ponowne zapisywanie adresów URL powoduje utworzenie abstrakcji między lokalizacjami zasobów a ich adresami, dzięki czemu lokalizacje i adresy nie są ściśle połączone. Ponowne zapisywanie adresów URL jest cenne w kilku scenariuszach:

* Przenoszenie lub zastępowanie zasobów serwera tymczasowo lub trwale i utrzymuje stałe lokalizatory dla tych zasobów.
* Podziel przetwarzanie żądań między różne aplikacje lub w obszarach jednej aplikacji.
* Usuwanie, Dodawanie lub organizowanie segmentów adresów URL w żądaniach przychodzących.
* Optymalizuj publiczne adresy URL dla optymalizacji aparatu wyszukiwania (wyszukiwarka).
* Zezwól na używanie przyjaznych publicznych adresów URL, aby ułatwić odwiedzającym zapowiadanie zawartości zwróconej przez żądanie zasobu.
* Przekieruj niezabezpieczone żądania do bezpiecznych punktów końcowych.
* Zapobiegaj hotlinking, gdzie lokacja zewnętrzna używa hostowanego zasobu statycznego w innej lokacji przez połączenie elementu zawartości z własną zawartością.

> [!NOTE]
> Ponowne zapisywanie adresów URL może zmniejszyć wydajność aplikacji. Jeśli to możliwe, należy ograniczyć liczbę i złożoność reguł.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="url-redirect-and-url-rewrite"></a>Przekierowywanie adresów URL i ponowne zapisywanie adresów URL

Różnica między *przekierowaniami adresów URL* i *przepisaniem adresów URL* jest subtelna, ale ma ważne konsekwencje dla udostępniania zasobów klientom. Ponowne zapisywanie oprogramowania pośredniczącego w usłudze ASP.NET Core jest w stanie sprostać potrzebom obu tych elementów.

*Przekierowanie adresu URL* obejmuje operację po stronie klienta, w której klient otrzymuje dostęp do zasobu pod innym adresem niż pierwotnie żądany klient. Wymaga to przeprowadzenia rundy na serwerze. Adres URL przekierowania zwracany do klienta pojawia się na pasku adresu przeglądarki, gdy klient wysyła nowe żądanie dla zasobu.

Jeśli `/resource` zostanie *przekierowana* do `/different-resource`, serwer odpowie, że klient powinien uzyskać zasób w `/different-resource` przy użyciu kodu stanu wskazującego, że przekierowanie jest tymczasowe lub trwałe.

![Punkt końcowy usługi WebAPI został tymczasowo zmieniony z wersji 1 (v1) do wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w ścieżce w wersji 1/v1/API. Serwer wysyła odpowiedź 302 (znalezioną) z nową ścieżką tymczasową dla usługi w wersji 2/v2/API. Klient wysyła drugie żądanie do usługi przy użyciu adresu URL przekierowania. Serwer reaguje na kod stanu 200 (OK).](url-rewriting/_static/url_redirect.png)

Podczas przekierowywania żądań do innego adresu URL wskaż, czy przekierowanie jest trwałe, czy tymczasowe, określając kod stanu z odpowiedzią:

* *301-przesunięty* kod stanu jest używany, gdy zasób ma nowy, stały adres URL i chcesz wydać klientowi, że wszystkie przyszłe żądania dla zasobu powinny używać nowego adresu URL. *Klient może buforować i ponownie używać odpowiedzi po odebraniu kodu stanu 301.*

* Kod stanu *znaleziony przez 302* jest używany, gdy przekierowywanie jest tymczasowe lub zwykle może ulec zmianie. Kod stanu 302 wskazuje na klienta, aby nie przechowywać adresu URL i używać go w przyszłości.

Aby uzyskać więcej informacji na temat kodów stanu, zobacz [RFC 2616: definicje kodów stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Ponowne *Zapisywanie adresów URL* jest operacją po stronie serwera, która dostarcza zasób z innego adresu zasobu niż żądany klient. Ponowne zapisywanie adresu URL nie wymaga przejazdu na serwer. Zwrotny adres URL nie jest zwracany do klienta i nie jest wyświetlany na pasku adresu przeglądarki.

Jeśli `/resource` jest *zapisywana* w `/different-resource`, serwer *wewnętrznie* pobiera i zwraca zasób w `/different-resource`.

Mimo że klient może być w stanie pobrać zasób przy zapisywanym adresie URL, klient nie ma informacji o tym, że zasób istnieje pod adresem URL, gdy wysyła żądanie i otrzymuje odpowiedź.

![Punkt końcowy usługi WebAPI został zmieniony z wersji 1 (v1) na wersję 2 (v2) na serwerze. Klient wysyła żądanie do usługi w ścieżce w wersji 1/v1/API. Adres URL żądania zostanie ponownie zapisany w celu uzyskania dostępu do usługi w ścieżce w wersji 2/v2/API. Usługa reaguje na klienta z kodem stanu 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Przykładowa aplikacja do ponownego zapisywania adresów URL

Możesz zapoznać się z funkcjami zapisywania oprogramowania pośredniczącego za pomocą [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Aplikacja stosuje reguły przekierowania i ponownego zapisywania oraz pokazuje przekierowany lub ponownie zapisany adres URL dla kilku scenariuszy.

## <a name="when-to-use-url-rewriting-middleware"></a>Kiedy używać oprogramowania pośredniczącego ponownego zapisywania adresów URL

Używaj ponownego zapisywania adresów URL, gdy nie możesz użyć następujących metod:

* [Moduł ponownego zapisywania adresów URL z usługami IIS w systemie Windows Server](https://www.iis.net/downloads/microsoft/url-rewrite)
* [Moduł Apache mod_rewrite na serwerze Apache](https://httpd.apache.org/docs/2.4/rewrite/)
* [Ponowne zapisywanie adresów URL w witrynie Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

Należy również użyć oprogramowania pośredniczącego, gdy aplikacja jest hostowana na [serwerze HTTP. sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanej webListener).

Główne przyczyny używania technologii zapisywania adresów URL opartych na serwerze w usługach IIS, Apache i Nginx są następujące:

* Oprogramowanie pośredniczące nie obsługuje pełnych funkcji tych modułów.

  Niektóre funkcje modułów serwera nie współpracują z projektami ASP.NET Core, takimi jak `IsFile` i `IsDirectory` ograniczenia modułu ponownego zapisywania usług IIS. W tych scenariuszach zamiast tego należy użyć oprogramowania pośredniczącego.
* Wydajność oprogramowania pośredniczącego prawdopodobnie nie jest zgodna z tymi modułami.

  Testy porównawcze są jedynym sposobem, aby wiedzieć, że metoda obniża wydajność w najbardziej lub niewielkim stopniu wydajności.

## <a name="package"></a>Pakiet

Aby uwzględnić oprogramowanie pośredniczące w projekcie, Dodaj odwołanie do pakietu do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu, który zawiera pakiet [Microsoft. AspNetCore. Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) .

Gdy nie korzystasz z pakietu `Microsoft.AspNetCore.App`, Dodaj odwołanie do projektu do pakietu `Microsoft.AspNetCore.Rewrite`.

## <a name="extension-and-options"></a>Rozszerzenie i opcje

Ustanów reguły ponownego zapisywania i przekierowywania adresów URL, tworząc wystąpienie klasy [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) z metodami rozszerzającymi dla każdej z reguł ponownego zapisywania. Łączenie wielu reguł w kolejności, w jakiej mają być przetwarzane. `RewriteOptions` są przenoszone do adresu URL ponownego zapisywania oprogramowania pośredniczącego, które zostało dodane do potoku żądania z <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a>Przekieruj do sieci Web inne niż www

Trzy opcje umożliwiają aplikacji Przekierowywanie żądań innych niż`www` do `www`:

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; trwale przekierować żądanie do domeny podrzędnej `www`, jeśli żądanie jest inne niż`www`. Przekierowuje kod stanu [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) .

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; przekierować żądanie do domeny podrzędnej `www`, jeśli żądanie przychodzące nie jest`www`. Przekierowuje kod stanu [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) . Przeciążenie umożliwia podanie kodu stanu odpowiedzi. Użyj pola klasy <xref:Microsoft.AspNetCore.Http.StatusCodes>, aby przypisać kod stanu.

### <a name="url-redirect"></a>Przekierowywanie adresów URL

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*>, aby przekierować żądania. Pierwszy parametr zawiera wyrażenie regularne do dopasowania na ścieżce przychodzącego adresu URL. Drugi parametr jest ciągiem zamiennym. Trzeci parametr, jeśli obecny, określa kod stanu. Jeśli nie określisz kodu stanu, kod stanu zostanie zmieniony na *302-znaleziono*, co oznacza, że zasób jest tymczasowo przenoszony lub zastępowany.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

W przeglądarce z włączonymi narzędziami deweloperskimi wykonaj żądanie do przykładowej aplikacji z ścieżką `/redirect-rule/1234/5678`. Wyrażenie regularne jest zgodne ze ścieżką żądania w `redirect-rule/(.*)`, a ścieżka jest zastępowana `/redirected/1234/5678`. Adres URL przekierowania jest wysyłany z powrotem do klienta z kodem stanu *znalezionym przez 302* . Przeglądarka tworzy nowe żądanie w adresie URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki. Ponieważ w adresie URL przekierowania nie ma reguł pasujących do przykładowej aplikacji:

* Drugie żądanie odbiera odpowiedź *200-OK* z aplikacji.
* Treść odpowiedzi zawiera adres URL przekierowania.

Po *przekierowaniu*adresu URL do serwera zostanie przeprowadzona runda.

> [!WARNING]
> Należy zachować ostrożność podczas ustanawiania reguł przekierowań. Reguły przekierowania są oceniane dla każdego żądania do aplikacji, w tym po przekierowaniu. Można łatwo przypadkowo utworzyć *pętlę nieskończonych przekierowań*.

Oryginalne żądanie: `/redirect-rule/1234/5678`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

Część wyrażenia zawartego w nawiasach jest nazywana *grupą przechwytywania*. Kropka (`.`) wyrażenia oznacza *dopasowanie dowolnego znaku*. Gwiazdka (`*`) oznacza *Dopasowanie znaku poprzedzającego zero lub więcej razy*. W związku z tym, ostatnie dwa segmenty ścieżki adresu URL, `1234/5678`, są przechwytywane przez `(.*)`grupy przechwytywania. Każda wartość podaną w adresie URL żądania po `redirect-rule/` zostanie przechwycona przez tę pojedynczą grupę przechwytywania.

W ciągu zamiennym przechwycone grupy są wstawiane do ciągu z znakiem dolara (`$`), a następnie numerem sekwencyjnym przechwytywania. Pierwsza wartość grupy przechwytywania jest uzyskiwana z `$1`, drugi z `$2`i kontynuuje sekwencję dla grup przechwytywania w wyrażeniach regularnych. W aplikacji przykładowej można wykonać tylko jedną przechwyconą grupę w postaci wyrażenia regularnego z regułą przekierowania, więc w ciągu zamiennym występuje tylko jedna grupa wstrzykiwana, która jest `$1`. Gdy reguła zostanie zastosowana, adres URL zostanie `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Przekierowanie adresu URL do bezpiecznego punktu końcowego

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*>, aby przekierować żądania HTTP do tego samego hosta i ścieżki przy użyciu protokołu HTTPS. Jeśli nie podano kodu stanu, oprogramowanie pośredniczące domyślnie zostanie *znalezione na 302*. Jeśli port nie jest podany:

* Ustawienia domyślne oprogramowania pośredniczącego `null`.
* Schemat zostanie zmieniony na `https` (protokół HTTPS), a klient uzyskuje dostęp do zasobu na porcie 443.

Poniższy przykład pokazuje, jak ustawić kod stanu na *301 — trwale przeniesiony* i zmienić port na 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*>, aby przekierować żądania niezabezpieczone do tego samego hosta i ścieżki z bezpiecznym protokołem HTTPS na porcie 443. Oprogramowanie pośredniczące ustawia kod stanu na *301 — trwale przeniesiony*.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Podczas przekierowywania do bezpiecznego punktu końcowego bez wymagania dla dodatkowych reguł przekierowywania zalecamy używanie oprogramowania pośredniczącego do przekierowania protokołu HTTPS. Aby uzyskać więcej informacji, zobacz temat [Wymuś https](xref:security/enforcing-ssl#require-https) .

Przykładowa aplikacja jest w stanie demonstrować, jak używać `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`. Dodaj metodę rozszerzenia do `RewriteOptions`. Wprowadź niezabezpieczone żądanie do aplikacji pod dowolnym adresem URL. Odrzuć ostrzeżenie o zabezpieczeniach przeglądarki, że certyfikat z podpisem własnym jest niezaufany lub Utwórz wyjątek, aby zaufać certyfikatowi.

Oryginalne żądanie przy użyciu `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

Oryginalne żądanie przy użyciu `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Regenerowanie adresów URL

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*>, aby utworzyć regułę ponownego zapisywania adresów URL. Pierwszy parametr zawiera wyrażenie regularne do dopasowania w przychodzącej ścieżce adresu URL. Drugi parametr jest ciągiem zamiennym. Trzeci parametr, `skipRemainingRules: {true|false}`, wskazuje na oprogramowanie pośredniczące, niezależnie od tego, czy pominięcia dodatkowych reguł ponownego zapisywania w przypadku zastosowania bieżącej reguły.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

Oryginalne żądanie: `/rewrite-rule/1234/5678`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądania i odpowiedzi](url-rewriting/_static/add_rewrite.png)

Daszek (`^`) na początku wyrażenia oznacza, że dopasowanie rozpoczyna się na początku ścieżki URL.

W poprzednim przykładzie z regułą przekierowania `redirect-rule/(.*)`nie ma daszka (`^`) na początku wyrażenia regularnego. W związku z tym wszystkie znaki mogą poprzedzać `redirect-rule/` w ścieżce, aby pomyślnie dopasować.

| Ścieżka                               | Dopasowanie |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Yes   |
| `/my-cool-redirect-rule/1234/5678` | Yes   |
| `/anotherredirect-rule/1234/5678`  | Yes   |

Reguła ponownego zapisywania `^rewrite-rule/(\d+)/(\d+)`, jeśli zaczyna się od `rewrite-rule/`, dopasowuje tylko ścieżki. W poniższej tabeli należy zwrócić uwagę na różnicę.

| Ścieżka                              | Dopasowanie |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Yes   |
| `/my-cool-rewrite-rule/1234/5678` | Nie    |
| `/anotherrewrite-rule/1234/5678`  | Nie    |

Po `^rewrite-rule/` części wyrażenia istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`. `\d` oznacza *dopasowanie cyfry (Number)* . Znak plusa (`+`) oznacza *dopasowanie co najmniej jednego znaku poprzedzającego*. W związku z tym adres URL musi zawierać numer, po którym następuje ukośnik, po którym następuje kolejny numer. Te grupy przechwytywania są wstrzykiwane do zarejestrowanego adresu URL jako `$1` i `$2`. Ciąg zastępczy reguły ponownego zapisu umieszcza przechwycone grupy w ciągu zapytania. Żądana ścieżka `/rewrite-rule/1234/5678` została zapisywana w celu uzyskania zasobu w `/rewritten?var1=1234&var2=5678`. Jeśli ciąg zapytania jest obecny w oryginalnym żądaniu, jest zachowywany, gdy adres URL zostanie ponownie zapisany.

Serwer nie może uzyskać dostępu do zasobów. Jeśli zasób istnieje, jest pobierany i zwracany do klienta przy użyciu kodu stanu *200-OK* . Ponieważ klient nie jest przekierowywany, adres URL na pasku adresu przeglądarki nie jest zmieniany. Klienci nie mogą wykryć, czy na serwerze wystąpiła operacja ponownego zapisywania adresu URL.

> [!NOTE]
> Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguły uzgadniania są wyliczane i zwiększają czas odpowiedzi aplikacji. Dla najszybszej odpowiedzi aplikacji:
>
> * Zamów reguły ponownego zapisu z najczęściej dopasowanej reguły do najmniej często dopasowanej reguły.
> * Pomiń przetwarzanie pozostałych reguł w przypadku wystąpienia dopasowania i nie jest wymagane żadne dodatkowe przetwarzanie reguły.

### <a name="apache-mod_rewrite"></a>Mod_rewrite Apache

Zastosuj reguły mod_rewrite Apache z <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>. Upewnij się, że plik reguł został wdrożony razem z aplikacją. Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

<xref:System.IO.StreamReader> jest używany do odczytywania reguł z pliku reguł *ApacheModRewrite. txt* :

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`. Kod stanu odpowiedzi to *302 — znaleziono*.

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

Oryginalne żądanie: `/apache-mod-rules-redirect/1234`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

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
* IPV6
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

### <a name="iis-url-rewrite-module-rules"></a>Reguły modułu ponownego zapisywania adresów URL usług IIS

Aby użyć tego samego zestawu reguł, który ma zastosowanie do modułu ponowne zapisywanie adresów URL usług IIS, użyj <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>. Upewnij się, że plik reguł został wdrożony razem z aplikacją. Nie należy kierować oprogramowanie pośredniczące do korzystania z pliku *Web. config* aplikacji podczas uruchamiania w usługach IIS systemu Windows Server. W przypadku usług IIS te reguły powinny być przechowywane poza plikiem *Web. config* aplikacji, aby uniknąć konfliktów z modułem ponownego zapisywania usług IIS. Aby uzyskać więcej informacji i przykłady reguł modułu ponownego zapisywania adresów URL usług IIS, zobacz [using URL Rewrite module 2,0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [informacje konfiguracyjne modułu ponownego zapisywania adresu URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

<xref:System.IO.StreamReader> jest używany do odczytywania reguł z pliku reguł *IISUrlRewrite. XML* :

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

Przykładowa aplikacja ponownie zapisuje żądania z `/iis-rules-rewrite/(.*)`, aby `/rewritten?id=$1`. Odpowiedź jest wysyłana do klienta z kodem stanu *200-OK* .

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

Oryginalne żądanie: `/iis-rules-rewrite/1234`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądania i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

Jeśli masz aktywny moduł ponownego zapisywania usług IIS z skonfigurowanymi regułami na poziomie serwera, które byłyby wpływać na aplikację na niepożądane sposoby, możesz wyłączyć moduł ponownego zapisywania usług IIS dla aplikacji. Aby uzyskać więcej informacji, zobacz [wyłączanie modułów IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nieobsługiwane funkcje

Oprogramowanie pośredniczące wydane z ASP.NET Core 2. x nie obsługuje następujących funkcji modułu ponownego zapisywania adresów URL usług IIS:

* Reguły ruchu wychodzącego
* Niestandardowe zmienne serwera
* Symbole wieloznaczne
* LogRewrittenUrl

#### <a name="supported-server-variables"></a>Obsługiwane Zmienne serwera

Oprogramowanie pośredniczące obsługuje następujące zmienne serwera modułu ponownego zapisywania adresów URL usług IIS:

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
> Możesz również uzyskać <xref:Microsoft.Extensions.FileProviders.IFileProvider> za pośrednictwem <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>. Takie podejście może zapewnić większą elastyczność lokalizacji plików reguł ponownego zapisywania. Upewnij się, że pliki reguł ponownego zapisywania są wdrożone na serwerze pod podaną ścieżką.
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Reguła oparta na metodzie

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*>, aby zaimplementować własną logikę reguł w metodzie. `Add` uwidacznia <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, które udostępnia <xref:Microsoft.AspNetCore.Http.HttpContext> do użycia w metodzie. [RewriteContext. Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) określa sposób obsługi dodatkowego przetwarzania potoku. Ustaw wartość na jedno z <xref:Microsoft.AspNetCore.Rewrite.RuleResult> pól opisanych w poniższej tabeli.

| `RewriteContext.Result`              | Akcja                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| `RuleResult.ContinueRules` (wartość domyślna) | Kontynuuj stosowanie reguł.                                         |
| `RuleResult.EndResponse`             | Zatrzymaj stosowanie reguł i Wyślij odpowiedź.                       |
| `RuleResult.SkipRemainingRules`      | Zatrzymaj stosowanie reguł i Wyślij kontekst do następnego oprogramowania pośredniczącego. |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

Przykładowa aplikacja przedstawia metodę, która przekierowuje żądania dla ścieżek kończących się na *. XML*. Jeśli zostanie wysłane żądanie `/file.xml`, żądanie jest przekierowywane do `/xmlfiles/file.xml`. Kod stanu jest ustawiony na *301 — trwale przeniesiony*. Gdy przeglądarka wykonuje nowe żądanie dla */XmlFiles/File.XML*, oprogramowanie pośredniczące plików statycznych zachowuje ten plik na kliencie z folderu *wwwroot/XmlFiles* . W przypadku przekierowania jawnie ustaw kod stanu odpowiedzi. W przeciwnym razie zwracany jest kod stanu *200-OK* i przekierowanie nie wystąpi na kliencie.

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

Takie podejście może również ponownie zapisywać żądania. Przykładowa aplikacja pokazuje, jak ponownie napisać ścieżkę do dowolnego żądania pliku tekstowego, aby zapewnić obsługę pliku tekstowego *pliku. txt* z folderu *wwwroot* . Oprogramowanie pośredniczące plików statycznych obsługuje plik na podstawie zaktualizowanej ścieżki żądania:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a>Reguła oparta na IRule

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*>, aby użyć logiki reguły w klasie implementującej interfejs <xref:Microsoft.AspNetCore.Rewrite.IRule>. `IRule` zapewnia większą elastyczność w porównaniu z użyciem metody opartej na metodzie. Klasa implementacji może zawierać konstruktora, który umożliwia przekazywanie parametrów dla metody <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*>.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

Wartości parametrów w przykładowej aplikacji dla `extension` i `newPath` są sprawdzane pod kątem spełnienia kilku warunków. `extension` musi zawierać wartość, a wartość musi mieć *Format PNG*, *jpg*lub *GIF*. Jeśli `newPath` jest nieprawidłowy, zostanie zgłoszony <xref:System.ArgumentException>. Jeśli zostanie wysłane żądanie pliku *Image. png*, żądanie jest przekierowywane do `/png-images/image.png`. Jeśli żądanie jest tworzone dla *obrazu. jpg*, żądanie jest przekierowywane do `/jpg-images/image.jpg`. Kod stanu jest ustawiony na *301 — trwale przesunięty*, a `context.Result` jest ustawiony tak, aby zatrzymać przetwarzanie reguł i wysłać odpowiedź.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

Oryginalne żądanie: `/image.png`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi dla pliku Image. png](url-rewriting/_static/add_redirect_png_requests.png)

Oryginalne żądanie: `/image.jpg`

![Okno przeglądarki z Narzędzia deweloperskie śledzenia żądań i odpowiedzi dla obrazu. jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Przykłady wyrażeń regularnych

| Cel | Ciąg wyrażenia regularnego &<br>Przykład dopasowania | & Ciągu zamiennego<br>Przykład danych wyjściowych |
| ---- | ------------------------------- | -------------------------------------- |
| Zapisz ścieżkę do ciągu QueryString | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Ukośnik końcowy na pasku | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Wymuszaj końcowy ukośnik | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Unikaj ponownego zapisywania określonych żądań | `^(.*)(?<!\.axd)$` lub `^(?!.*\.axd$)(.*)$`<br>Tak: `/resource.htm`<br>Nie: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Zmień rozmieszczenie segmentów adresu URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Zastępowanie segmentu adresu URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Wyrażenia regularne w programie .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Język wyrażeń regularnych — Krótki przewodnik](/dotnet/articles/standard/base-types/quick-ref)
* [Mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/)
* [Używanie modułu ponownego zapisywania adresu URL 2,0 (dla usług IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Odwołanie do konfiguracji modułu ponownego zapisywania adresu URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Forum ponownego zapisywania adresów URL usług IIS](https://forums.iis.net/1152.aspx)
* [Zachowaj prostą strukturę adresu URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 — porady i wskazówki dotyczące ponownego zapisywania adresów URL](https://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Na ukośnik lub nie do ukośnika](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
