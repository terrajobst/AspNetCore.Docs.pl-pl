---
title: Adres URL ponowne napisanie oprogramowania pośredniczącego w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z adresem URL ponownego zapisywania adresów i przekierowywania z oprogramowanie pośredniczące ponownego zapisywania adresów URL w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/url-rewriting
ms.openlocfilehash: 72d5b2e902a95442ccffb7a149b917c50373775b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902203"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Adres URL ponowne napisanie oprogramowania pośredniczącego w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Mikael Mengistu](https://github.com/mikaelm12)

::: moniker range="<= aspnetcore-1.1"

Dla wersji 1.1 w tym temacie, Pobierz [oprogramowanie pośredniczące ponownego zapisywania adresów URL w programie ASP.NET Core (w wersji 1.1, plików PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf).

::: moniker-end

Ten dokument wprowadza instrukcje dotyczące sposobu używania oprogramowanie pośredniczące ponownego zapisywania adresów URL w aplikacji platformy ASP.NET Core ponownego zapisywania adresów URL.

Ponownego zapisywania adresów URL jest działaniem zmiany żądania, których adresy URL na podstawie co najmniej jeden wstępnie zdefiniowanych reguł. Ponownego zapisywania adresów URL tworzy abstrakcję między lokalizacje zasobów i ich adresów, tak, aby lokalizacje i adresy nie są ściśle połączony. Ponownego zapisywania adresów URL jest przydatne w kilku scenariuszach do:

* Przenieś lub Zastąp zasoby serwera, tymczasowo lub trwale i utrzymać stabilną lokalizatorów dla tych zasobów.
* Podziel przetwarzanie żądania w różnych aplikacjach lub w wielu obszarach jednej aplikacji.
* Usuwanie, dodawanie lub zreorganizować segmenty adresu URL na żądań przychodzących.
* Optymalizuj publiczne adresy URL do optymalizacji aparatu wyszukiwania (SEO).
* Zezwala na użycie przyjaznych adresów URL z publiczny ułatwia odwiedzających przewidzieć zawartość zwróconą przez żądanie zasobu.
* Przekieruj niezabezpieczone żądania do bezpieczne punkty końcowe.
* Zapobiegaj hotlinking, w których witryny zewnętrznej używa hostowanej zawartości statycznej w innej lokacji, łącząc elementu zawartości do własnej zawartości.

> [!NOTE]
> Ponownego zapisywania adresów URL może zmniejszyć wydajność aplikacji. Jeśli jest to możliwe, należy ograniczyć liczbę i złożoność reguł.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="url-redirect-and-url-rewrite"></a>Ponowne zapisywanie adresów URL przekierowania i adres URL

Różnica w treść między *adres URL przekierowania* i *ponowne zapisywanie adresów URL* jest subtelne, ale ma istotny wpływ na zapewniania zasobów dla klientów. Oprogramowanie pośredniczące ponownego zapisywania adresów URL platformy ASP.NET Core jest w stanie spełniające potrzeby dla obu.

A *adres URL przekierowania* obejmuje operacji po stronie klienta, w której klient jest zobowiązany do uzyskania dostępu do zasobu pod adresem innego niż dany klient pierwotnie żądaną. Ta migracja wymaga rund do serwera. Adres URL przekierowania zwracana do klienta pojawia się w pasku adresu przeglądarki, gdy klient wysyła nowe żądanie dla zasobu.

Jeśli `/resource` jest *przekierowanie* do `/different-resource`, serwer odpowiada, że klient powinien uzyskać zasób w `/different-resource` z kodem stanu wskazującym, przekierowania, który jest tymczasowy lub stały.

![Punkt końcowy usługi WebAPI tymczasowo zmieniono z wersją 1 (v1) do wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w wersji 1 /v1/api ścieżki. Serwer odsyła 302 odpowiedź (Found) z nowego, tymczasowego ścieżkę dla usługi w wersji 2 /v2/api. Klient wysyła drugie żądanie do usługi pod adresem URL przekierowania. Serwer odpowiada, zwracając kod stanu 200 (OK).](url-rewriting/_static/url_redirect.png)

Podczas przekierowywania żądań do innego adresu URL, informujące o przekierowania stałych lub tymczasowych, określając kod stanu odpowiedzi:

* *301 - trwale przeniesiona* kod stanu jest używany w którym zasób ma nowy, stały adres URL i chcesz celu poinstruowania klienta o tym, że wszystkie przyszłe żądania dotyczące zasobów powinien używać nowego adresu URL. *Klient może w pamięci podręcznej i ponowne użycie odpowiedzi, po odebraniu kodu 301 stanu.*

* *302 — znaleziono* kod stanu jest używany w przypadku, gdy przekierowania jest tymczasowy lub ogólnie może ulec zmianie. Kod stanu 302 wskazuje klientowi adresu URL Sklepu i używać go w przyszłości.

Aby uzyskać więcej informacji na temat kodów stanu, zobacz [dokumencie RFC 2616: Definicje kodów stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *ponowne zapisywanie adresów URL* jest operacją po stronie serwera, który udostępnia zasób z adresu innego zasobu niż dany klient zażądał. Ponownego zapisywania adresów URL nie wymaga komunikacji dwustronnej z serwerem. Nowych adres URL nie jest zwracana do klienta i nie będzie wyświetlany w pasku adresu przeglądarki.

Jeśli `/resource` jest *przepisany* do `/different-resource`, serwer *wewnętrznie* pobiera i zwraca zasób w `/different-resource`.

Mimo że klienta może mieć możliwość pobrania zasobu pod adresem URL nowych, klient nie jest informacja, że zasób istnieje pod adresem URL nowych sprawia, że jego żądanie i odbiera odpowiedź.

![Punkt końcowy usługi WebAPI została zmieniona z wersją 1 (v1) do wersji 2 (v2) na serwerze. Klient wysyła żądanie do usługi w wersji 1 /v1/api ścieżki. Adres URL żądania jest przepisane, aby uzyskać dostęp do usługi w wersji 2 /v2/api ścieżki. Usługa odnosi się do klienta z kodem stanu 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Przykładowa aplikacja ponownego zapisywania adresów URL

Możesz eksplorować funkcje pośredniczącym ponownego zapisywania adresów URL przy użyciu [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Aplikacja ma zastosowanie przekierowania i ponowne zapisywanie adresów reguł i zawiera adres URL przekierowanego lub nowych dla kilku scenariuszy.

## <a name="when-to-use-url-rewriting-middleware"></a>Kiedy należy używać oprogramowanie pośredniczące ponownego zapisywania adresów URL

Oprogramowanie pośredniczące ponownego zapisywania adresów URL należy użyć, jeśli nie można użyć następujących metod:

* [Moduł ponowne zapisywanie adresów URL z usługami IIS w systemie Windows Server](https://www.iis.net/downloads/microsoft/url-rewrite)
* [Moduł mod_rewrite Apache na serwerze Apache](https://httpd.apache.org/docs/2.4/rewrite/)
* [Adres URL ponownego zapisywania adresów na serwera Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

Ponadto korzystania z oprogramowania pośredniczącego, gdy aplikacja jest hostowana na [serwera HTTP.sys](xref:fundamentals/servers/httpsys) (dawniej nazywanych WebListener).

Główne przyczyny używał adresu URL na serwerze, ponowne napisanie technologii dostępnych w usługach IIS, Apache i Nginx są następujące:

* Oprogramowanie pośredniczące nie obsługuje funkcji pełnego tych modułów.

  Niektóre funkcje modułów serwera nie działają z projektów ASP.NET Core, takich jak `IsFile` i `IsDirectory` ograniczenia moduł ponowne zapisywanie adresów IIS. W tych scenariuszach Użyj oprogramowania pośredniczącego.
* Wydajność oprogramowania pośredniczącego prawdopodobnie nie jest zgodna z modułów.

  Analiza porównawcza jest jedynym sposobem, aby upewnić się, które rozwiązanie najlepiej obniża wydajność lub jeśli pogorszenie wydajności jest niewielka.

## <a name="package"></a>Package

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołania do pakietu do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) w pliku projektu, który zawiera [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) pakietu.

Bez korzystania z `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj odwołanie do `Microsoft.AspNetCore.Rewrite` pakietu.

## <a name="extension-and-options"></a>Opcje i rozszerzenia

Nawiązać ponowne zapisywanie adresów URL i przekierować reguły przez utworzenie wystąpienia [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) klasy przy użyciu metody rozszerzenia dla poszczególnych reguł ponownego zapisywania. Utworzyć łańcuch wielu reguł w kolejności, że chcesz je przetworzyć. `RewriteOptions` Są przekazywane do oprogramowanie pośredniczące ponownego zapisywania adresów URL jest dodawany do potoku żądania za pomocą <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a>Przekierowanie nie www do www

Trzy opcje umożliwiają aplikacji, aby przekierować non -`www` żądania `www`:

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; Trwałe przekierowanie żądania do `www` poddomeny, jeśli żądanie ma wartość inną niż`www`. Przekierowuje z [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) kod stanu.

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Przekieruj żądania do `www` poddomeny, jeśli żądanie przychodzące ma wartość inną niż`www`. Przekierowuje z [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) kod stanu. Przeciążenie umożliwia podawanie kodu stanu odpowiedzi. Użyj pola <xref:Microsoft.AspNetCore.Http.StatusCodes> klasy przydziału kodu stanu.

### <a name="url-redirect"></a>Adres URL przekierowania

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> Przekierowywanie żądań. Pierwszy parametr zawiera Twoje wyrażenia regularnego do dopasowania w ścieżce przychodzącego adresu URL. Drugi parametr jest ciąg zastępujący. Trzeci parametr, jeśli jest obecny, określa kod stanu. Jeśli nie określisz kod stanu, kod stanu: wartość domyślna to *302 — znaleziono*, co oznacza zasobu jest tymczasowo przenieść lub zastąpiony.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

W przeglądarce za pomocą narzędzi dla deweloperów, włączone, należy wysłać żądanie do przykładowej aplikacji ze ścieżką `/redirect-rule/1234/5678`. Wyrażenie regularne dopasowuje ścieżki żądania w `redirect-rule/(.*)`, a ścieżka została zastąpiona `/redirected/1234/5678`. Przekierowywanie adresu URL jest wysyłane z powrotem do klienta z *302 — znaleziono* kod stanu. Przeglądarka sprawia, że nowe wezwanie pod adresem URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki. Od żadnych reguł dopasowania przykładowej aplikacji na adres URL przekierowania:

* Drugie żądanie odbiera *200 — OK* odpowiedzi z aplikacji.
* Treść odpowiedzi zawiera adres URL przekierowania.

A obiegu jest wysyłane do serwera, gdy adres URL jest *przekierowanie*.

> [!WARNING]
> Należy zachować ostrożność podczas ustanawiania zasad przekierowania. Zasady przekierowania są obliczane na każde żądanie do aplikacji, nawet po przekierowania. Można łatwo utworzyć przypadkowo *pętli nieskończonej przekierowuje*.

Oryginalne żądanie: `/redirect-rule/1234/5678`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

Część numerowanie wyrażenia zawartgoe w nawiasach jest nazywany *grupa przechwytywania*. Kropka (`.`) wyrażenie oznacza, że *dopasowuje dowolny znak*. Gwiazdka (`*`) wskazuje *Dopasuj poprzedni znak zero lub więcej razy*. W związku z tym, segmenty ostatnie dwie ścieżki adresu URL, `1234/5678`, są przechwytywane przez grupę przechwytywania `(.*)`. Dowolna wartość należy podać w adresie URL żądania po `redirect-rule/` są przechwytywane przez tę grupę przechwytywania jednego.

W ciągu zamiennym przechwyconych grupach są wstrzykiwane do ciągu znakiem dolara (`$`) wraz z numerem sekwencji przechwytywania. Wartość pierwszej grupy przechwytywania są uzyskiwane z `$1`, druga z `$2`, i kontynuują w sekwencji dla grup przechwytywania w swojej wyrażenia regularnego. Tylko jedna grupa przechwycone z wyrażeniem regularnym reguła przekierowania w jest przykładowej aplikacji, więc tylko jedna grupa wprowadzonego w ciągu zamiennym, który jest `$1`. Po zastosowaniu reguły staje się adres URL `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Adres URL przekierowania do bezpiecznego punktu końcowego

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> Przekierowywanie żądań HTTP do tego samego hosta i ścieżkę przy użyciu protokołu HTTPS. Jeśli nie został dostarczony kod stanu, oprogramowanie pośredniczące, wartość domyślna to *302 — znaleziono*. Jeśli port nie został dostarczony:

* Wartością domyślną jest oprogramowanie pośredniczące `null`.
* Schemat zmienia się na `https` (protokół HTTPS), a klient uzyskuje dostęp do zasobów na porcie 443.

Poniższy przykład pokazuje, jak ustawić kod stanu *301 - trwale przeniesiona* i zmień port 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> Aby przekierować niezabezpieczone żądania do tego samego hosta i ścieżkę z bezpiecznego protokołu HTTPS na porcie 443. Oprogramowanie pośredniczące ustawia kod stanu *301 - trwale przeniesiona*.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Podczas przekierowywania do bezpiecznego punktu końcowego nie wymaga przekierowania dodatkowych reguł, zaleca się za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS. Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl#require-https) tematu.

Przykładowa aplikacja jest w stanie pokazuje sposób użycia studia `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`. Dodaj metodę rozszerzenia, aby `RewriteOptions`. Przesyłania niezabezpieczone żądania do aplikacji na dowolny adres URL. Odrzuć zabezpieczeń przeglądarki, ostrzeżenie, że nie jest zaufany certyfikat z podpisem własnym, lub Utwórz wyjątek, aby ufać certyfikatowi.

Oryginalne żądanie, używając `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

Oryginalne żądanie, używając `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Ponowne zapisywanie adresów URL

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> można utworzyć regułę ponownego zapisywania adresów URL. Pierwszy parametr zawiera wyrażenie regularne do dopasowania na przychodzące Ścieżka adresu URL. Drugi parametr jest ciąg zastępujący. Trzeci parametr `skipRemainingRules: {true|false}`, wskazuje, aby oprogramowanie pośredniczące umożliwia określenie, czy pominąć reguły ponownego zapisywania dodatkowe, jeśli zastosowano bieżącej reguły.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

Oryginalne żądanie: `/rewrite-rule/1234/5678`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_rewrite.png)

Pojawia się znak daszka (`^`) na początku oznacza wyrażenie tej dopasowywanie rozpoczyna się od początku ścieżki adresu URL.

We wcześniejszym przykładzie z regułą przekierowania `redirect-rule/(.*)`, nie ma żadnych daszka (`^`) na początku wyrażenia regularnego. W związku z tym, mogą poprzedzać wszystkie znaki `redirect-rule/` w ścieżce dla pomyślnego dopasowania.

| Ścieżka                               | Dopasowanie |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Tak   |
| `/my-cool-redirect-rule/1234/5678` | Yes   |
| `/anotherredirect-rule/1234/5678`  | Tak   |

Reguły ponownego pisania `^rewrite-rule/(\d+)/(\d+)`, tylko dopasowuje ścieżek, jeśli zaczyna `rewrite-rule/`. W poniższej tabeli należy zauważyć różnicę w dopasowywanie.

| Ścieżka                              | Dopasowanie |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Yes   |
| `/my-cool-rewrite-rule/1234/5678` | Nie    |
| `/anotherrewrite-rule/1234/5678`  | Nie    |

Następujące `^rewrite-rule/` część wyrażenia, istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`. `\d` Oznacza *dopasowania cyfrę (numer)*. Znak plus (`+`) oznacza, że *zgodne z co najmniej jeden znak poprzedzający*. W związku z tym adres URL musi zawierać liczbę następuje ukośnikiem do przodu następuje inny numer. Przechwytywanie, te grupy są wstrzykiwane do nowych adresu URL jako `$1` i `$2`. Ciąg zastępujący reguły ponownego zapisywania umieszcza przechwyconych grupach w ciągu zapytania. Żądana ścieżka `/rewrite-rule/1234/5678` jest przepisany można uzyskać zasobu w `/rewritten?var1=1234&var2=5678`. Jeśli ciąg zapytania jest obecna na oryginalne żądanie, są zachowywane, gdy adres URL jest przepisany.

Nie ma żadnych komunikacji dwustronnej z serwerem, można uzyskać zasobu. Jeśli zasób istnieje, ma pobrać i zwracany do klienta z *200 — OK* kod stanu. Ponieważ klient nie jest przekierowany, nie powoduje zmiany adresu URL w pasku adresu przeglądarki. Klienci nie może wykryć, że operacja ponownego zapisywania adresu URL wystąpił na serwerze.

> [!NOTE]
> Użyj `skipRemainingRules: true` zawsze, gdy to możliwe ponieważ reguł dopasowania jest obliczeniowo kosztowne i wydłuża czas odpowiedzi aplikacji. Najszybszy odpowiedzi aplikacji:
>
> * Kolejność napisać ponownie reguły z najczęściej dopasowane reguły do najmniej często dopasowane reguły.
> * Pomiń przetwarzanie pozostałych reguł, gdy pasują do przetwarzania nie dodatkowe reguły jest wymagana.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Zastosuj reguły mod_rewrite Apache z <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>. Upewnij się, że plik reguł jest wdrażany z aplikacją. Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

A <xref:System.IO.StreamReader> służy do odczytywania reguły z *ApacheModRewrite.txt* pliku reguł:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`. Kod stanu odpowiedzi to *302 — znaleziono*.

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

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

### <a name="iis-url-rewrite-module-rules"></a>Reguły moduł ponowne zapisywanie adresów URL usług IIS

Aby użyć tego samego zestawu reguł, który ma zastosowanie do moduł ponowne zapisywanie adresów URL usług IIS, należy użyć <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>. Upewnij się, że plik reguł jest wdrażany z aplikacją. Nie bezpośrednie oprogramowania pośredniczącego, aby korzystać z aplikacji *web.config* pliku podczas uruchamiania w systemie Windows Server w usługach IIS. Za pomocą programu IIS, reguły te powinny być przechowywane poza aplikacji *web.config* plik w celu uniknięcia konfliktów z modułem ponownego zapisywania usług IIS. Aby uzyskać więcej informacji i przykłady reguł moduł ponowne zapisywanie adresów URL usług IIS, zobacz [przy użyciu adresu Url Nadpisz modułu 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [odwołanie konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

A <xref:System.IO.StreamReader> służy do odczytywania reguły z *IISUrlRewrite.xml* pliku reguł:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

Przykładowa aplikacja ponownie zapisuje żądań z `/iis-rules-rewrite/(.*)` do `/rewritten?id=$1`. Odpowiedź jest wysyłana do klienta z *200 — OK* kod stanu.

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

Oryginalne żądanie: `/iis-rules-rewrite/1234`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

Jeśli masz aktywne moduł ponowne zapisywanie adresów usług IIS przy użyciu skonfigurowanych reguł poziom serwera, które mogło mieć wpływ na aplikację w sposób niepożądane, możesz wyłączyć moduł ponowne zapisywanie adresów usług IIS dla aplikacji. Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nieobsługiwane funkcje

Oprogramowanie pośredniczące zwolnione z platformą ASP.NET Core 2.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:

* Reguły ruchu wychodzącego
* Zmienne serwera niestandardowego
* Symboli wieloznacznych
* LogRewrittenUrl

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
> Możesz również uzyskać <xref:Microsoft.Extensions.FileProviders.IFileProvider> za pośrednictwem <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>. Takie podejście może dostarczyć większą elastyczność dla lokalizacji usługi ponownego zapisywania plików reguły. Upewnij się, że pliki reguły ponownego zapisywania są wdrażane do serwera w ścieżce, których udzielasz.
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Reguły oparte na metodzie

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> do zaimplementowania własnej logiki reguły w metodzie. `Add` udostępnia <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, co sprawia, że dostępne <xref:Microsoft.AspNetCore.Http.HttpContext> do użycia w metodzie. [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) Określa, jak dodatkowe potok przetwarzania jest obsługiwane. Ustaw wartość na jedną z <xref:Microsoft.AspNetCore.Rewrite.RuleResult> pola opisane w poniższej tabeli.

| `RewriteContext.Result`              | Akcja                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| `RuleResult.ContinueRules` (ustawienie domyślne) | Kontynuuj, stosowanie reguł.                                         |
| `RuleResult.EndResponse`             | Zatrzymaj stosowanie reguł i wysłania odpowiedzi.                       |
| `RuleResult.SkipRemainingRules`      | Zaprzestanie stosowania reguły i wysyłać kontekstu do następnego oprogramowania pośredniczącego. |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

Przykładowa aplikacja pokazuje metody, która przekierowuje żądania dla ścieżek, które kończy się *.xml*. W przypadku żądania dotyczącego `/file.xml`, żądanie jest przekierowywane do `/xmlfiles/file.xml`. Kod stanu jest ustawiona na *301 - trwale przeniesiona*. Jeśli przeglądarka sprawia, że nowe żądanie dla */xmlfiles/file.xml*, oprogramowanie pośredniczące plików statycznych służy pliku do klienta z *wwwroot/xmlfiles* folderu. Dla przekierowania należy jawnie ustawić kod stanu odpowiedzi. W przeciwnym razie *200 — OK* zwróciła kod stanu i przekierowania, który nie występuje na komputerze klienckim.

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

Takie podejście może również ponowne zapisywanie żądań. Przykładowa aplikacja pokazuje ponowne napisanie ścieżkę dla każdego żądania pliku tekstowego do obsługi *plik.txt* plik tekstowy z *wwwroot* folderu. Oprogramowanie pośredniczące plików statycznych służy pliku na podstawie ścieżki zaktualizowane żądanie:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a>Na podstawie irule na podstawie reguł

Użyj <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> do użycia z logiką reguły w klasie, która implementuje <xref:Microsoft.AspNetCore.Rewrite.IRule> interfejsu. `IRule` zapewnia większą elastyczność na podejście oparte na metodzie reguły. Implementacja klasy może zawierać konstruktora, który umożliwia można przekazać parametry <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> metody.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

Wartości parametrów w przykładowej aplikacji dla `extension` i `newPath` są sprawdzane w celu spełnienia kilku warunków. `extension` Musi zawierać wartość, a wartość musi być *.png*, *.jpg*, lub *.gif*. Jeśli `newPath` nie jest prawidłowy, <xref:System.ArgumentException> zgłaszany. W przypadku żądania dotyczącego *image.png*, żądanie jest przekierowywane do `/png-images/image.png`. W przypadku żądania dotyczącego *image.jpg*, żądanie jest przekierowywane do `/jpg-images/image.jpg`. Kod stanu jest ustawiona na *301 - trwale przeniesiona*i `context.Result` jest ustawiona, aby zatrzymać przetwarzanie reguł i wysłania odpowiedzi.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

Oryginalne żądanie: `/image.png`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla image.png](url-rewriting/_static/add_redirect_png_requests.png)

Oryginalne żądanie: `/image.jpg`

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Przykłady wyrażeń regularnych

| Cel | Wyrażenie regularne ciągu &<br>Przykład dopasowania | Ciąg zastępujący &<br>Przykład danych wyjściowych |
| ---- | ------------------------------- | -------------------------------------- |
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
