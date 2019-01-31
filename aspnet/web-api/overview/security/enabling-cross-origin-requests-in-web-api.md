---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Włączanie żądań Cross-Origin we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Pokazuje sposób obsługi udostępniania zasobów między źródłami (CORS) w interfejsie API sieci Web platformy ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97a0027194b019b09e220493dcb593e682027fe3
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428450"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Włączanie żądań cross-origin w programie ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Zabezpieczenia przeglądarki uniemożliwiają stronie internetowej wysyłanie żądań AJAX do innej domeny. To ograniczenie jest nazywany *zasadami tego samego źródła*i zapobiega złośliwych witryn odczytywanie poufnych danych z innej lokacji. Jednak czasami możesz chcieć umożliwić innych witryn, wywołania interfejsu API sieci web.
>
> [Krzyżowe współużytkowanie zasobów](http://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne. CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej techniki takie jak [JSONP](http://en.wikipedia.org/wiki/JSONP). W tym samouczku pokazano, jak włączyć mechanizm CORS w aplikacji interfejsu API sieci Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Oprogramowania używanego w tym samouczku
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Składnik Web API 2.2

## <a name="introduction"></a>Wprowadzenie

Ten samouczek pokazuje, że obsługa mechanizmu CORS w Web API platformy ASP.NET. Zaczniemy od utworzenia dwóch projektów platformy ASP.NET — jedną o nazwie "Usługa sieci Web", który hostuje kontrolera interfejsu API sieci Web, a inne o nazwie "Usługa WebClient", która wywołuje usługę sieci Web. Ponieważ dwie aplikacje są przechowywane w różnych domenach, żądanie AJAX z WebClient Usługa sieci Web jest żądaniem cross-origin.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Co to jest "tego samego origin"?

Dwa adresy URL mają tego samego źródła, jeśli mają identyczne schematy, hosty i portów. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Te dwa adresy URL są tego samego źródła:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Te adresy URL są źródła innego niż poprzednie dwa:

- `http://example.net` -Innej domeny
- `http://example.com:9000/foo.html` -Innego portu
- `https://example.com/foo.html` -Innego schematu
- `http://www.example.com/foo.html` — Różne domeny podrzędnej

> [!NOTE]
> Program Internet Explorer nie należy wziąć pod uwagę portu podczas porównywania źródeł.

## <a name="create-the-webservice-project"></a>Utwórz projekt usługi sieci Web

> [!NOTE]
> W tej sekcji założono, że użytkownik wie już, jak tworzyć projekty interfejsu API sieci Web. Jeśli nie, zobacz [wprowadzenie do interfejsu API sieci Web platformy ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Uruchom program Visual Studio i Utwórz nowy **aplikacji sieci Web platformy ASP.NET (.NET Framework)** projektu.
2. W **Nowa aplikacja internetowa ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu projektu. W obszarze **. Dodaj foldery i podstawowe odwołania dla**, wybierz opcję **interfejsu API sieci Web** pola wyboru.

   ![ASP.NET okna dialogowego Nowy projekt w programie Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Dodawanie kontrolera interfejsu API sieci Web o nazwie `TestController` następującym kodem:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Można uruchomić aplikację lokalnie, lub Wdróż na platformie Azure. (Zrzuty ekranu w tym samouczku aplikacja wdrażanie w usłudze Azure App Service Web Apps.) Aby sprawdzić, czy działa interfejs API sieci web, przejdź do `http://hostname/api/test/`, gdzie *hostname* jest domeną, w których wdrożono aplikację. Powinien zostać wyświetlony tekst odpowiedzi &quot;UZYSKAĆ: Wiadomości testowe&quot;.

   ![Wiadomość testowa przedstawiający przeglądarkę sieci Web](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Utwórz projekt WebClient

1. Utworzyć kolejną **aplikacji sieci Web platformy ASP.NET (.NET Framework)** projektu, a następnie wybierz **MVC** szablonu projektu. Opcjonalnie można zaznaczyć **Zmień uwierzytelnianie** > **bez uwierzytelniania**. Nie musisz uwierzytelniania na potrzeby tego samouczka.

   ![Szablon MVC w oknie dialogowym Nowy projekt ASP.NET, w programie Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. W **Eksploratora rozwiązań**, otwórz plik *Views/Home/Index.cshtml*. Zastąp kod w tym pliku zgodnie z poniższym przykładem:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Aby uzyskać *serviceUrl* zmiennej, użyj identyfikatora URI aplikacji usługi sieci Web.

3. Lokalne uruchamianie aplikacji WebClient lub opublikować ją do innej witryny sieci Web.

Po kliknięciu przycisku "Try It" żądanie AJAX jest przesyłany do aplikacji usługi sieci Web za pomocą metody HTTP wymienionych w polu listy rozwijanej (GET, POST lub PUT). Dzięki temu możesz sprawdzić różne żądań cross-origin. Aplikacja usługi sieci Web nie obsługuje obecnie mechanizmu CORS, więc jeśli klikniesz przycisk otrzymasz błąd.

![Błąd "Wypróbuj" w przeglądarce](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Jeśli możesz obejrzeć ruch HTTP w narzędziu [Fiddler](https://www.telerik.com/fiddler), zostanie wyświetlony w przeglądarce wysyłania żądania GET i żądanie zakończy się powodzeniem, że wywołanie AJAX zwraca błąd. Jest ważne zrozumieć, zasadami jednego źródła, nie uniemożliwia przeglądarki z *wysyłania* żądania. Zamiast tego należy go zapobiega aplikację widzisz *odpowiedzi*.

![Debuger sieci web programu fiddler przedstawiający żądania sieci web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Włączanie mechanizmu CORS

Teraz możemy włączyć mechanizm CORS w aplikacji usługi sieci Web. Najpierw Dodaj pakiet NuGet mechanizmu CORS. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

To polecenie instaluje najnowszy pakiet i aktualizuje wszystkie zależności, w tym podstawowe biblioteki interfejsu API sieci Web. Użyj `-Version` flagi pod kątem określonej wersji. Pakiet CORS wymaga internetowego interfejsu API w wersji 2.0 lub nowszej.

Otwórz plik *aplikacji\_Start/WebApiConfig.cs*. Dodaj następujący kod do **WebApiConfig.Register** metody:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Następnie dodaj **[EnableCors]** atrybutu `TestController` klasy:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Aby uzyskać *źródła* parametru, użyj identyfikatora URI, do której została wdrożona aplikacja klient sieci Web. Dzięki temu żądań cross-origin z WebClient, podczas nadal nie można przydzielać wszystkich innych żądaniach między domenami. Później będziesz opisano parametry **[EnableCors]** bardziej szczegółowo.

Nie dołączaj ukośnika na końcu *źródła* adresu URL.

Ponownie wdróż zaktualizowaną aplikację usługi sieci Web. Nie musisz zaktualizować WebClient. Teraz żądanie AJAX z WebClient powinna zakończyć się pomyślnie. Metody GET, PUT i POST wszystkie dozwolone.

![Komunikat pomyślnego testowego przedstawiający przeglądarkę sieci Web](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Jak działa mechanizmu CORS

W tej sekcji opisano, co się dzieje w żądaniu CORS, na poziomie wiadomości HTTP. Jest ważne dowiedzieć się, jak działa mechanizm CORS, dzięki czemu można skonfigurować **[EnableCors]** atrybutu prawidłowo i rozwiązywanie problemów, jeśli elementy nie działają zgodnie z oczekiwaniami.

Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin; nie trzeba wykonywać żadnych specjalnych w kodzie JavaScript czynności.

Oto przykład żądania między źródłami. Nagłówek "Origin" zapewnia domeny lokacji, który wysłał żądanie.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Jeśli serwer zezwala na żądanie, ustawia dla nagłówka Access-Control-Allow-Origin. Wartość tego nagłówka stanowi odpowiada nagłówek źródła albo ma wartość symbolu wieloznacznego "\*", co oznacza, że wszystkie źródła jest dozwolone.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiodło się. W szczególności przeglądarki nie zezwalają na żądanie. Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnić odpowiedź do aplikacji klienckiej.

**Żądania wstępnego**

W przypadku niektórych żądań CORPS przeglądarki przesyła wysłanie dodatkowego żądania o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu.

Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:

- Metoda żądania jest GET, HEAD lub WPIS, *i*
- Aplikacja nie ustawia wszystkie nagłówki żądania innego niż zawartości Accept, Accept-Language-Language, Content-Type lub ostatni-Event-ID *i*
- Nagłówek Content-Type (jeśli ustawić) jest jednym z następujących czynności:

    - application/x-www-form-urlencoded
    - multipart/formularza data
    - zwykły tekst

Reguła o nagłówków żądań ma zastosowanie do nagłówków, które aplikacja ustawia przez wywołanie metody **setRequestHeader** na **XMLHttpRequest** obiektu. (Specyfikacji CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówków *przeglądarki* można ustawić, np. Agent użytkownika, Host lub Content-Length.

Oto przykład żądania wstępnego:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP. Zawiera ona dwie specjalnych nagłówków:

- Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.
- Access-Control-Request-Headers: Listę nagłówków żądań, *aplikacji* nastavit rzeczywistego żądania. (Ponownie, to nie zawiera nagłówki, które ustawia przeglądarki.)

Poniżej przedstawiono przykładową odpowiedź, przy założeniu, że serwer zezwala na żądanie:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Odpowiedź zawiera nagłówek dostępu — kontrola-Allow-Methods, zawierającego dozwolone metody i, opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolonych nagłówków. Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.

Narzędzia często używane do testowania punktów końcowych przy użyciu żądania wstępnego OPTIONS (na przykład [Fiddler](https://www.telerik.com/fiddler) i [Postman](https://www.getpostman.com/)) nie wymagane nagłówki opcje będą domyślnie wysyłane. Upewnij się, że `Access-Control-Request-Method` i `Access-Control-Request-Headers` nagłówki są wysyłane z żądania i opcje nagłówki skontaktować się z aplikacją za pośrednictwem usług IIS.

Aby skonfigurować serwer IIS zezwala na aplikację ASP.NET w celu odbierania i obsługi żądań opcji, dodaj następującą konfigurację w aplikacji *web.config* w pliku `<system.webServer><handlers>` sekcji:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

Usunięcie `OPTIONSVerbHandler` zapobiega obsługi opcji żądań usług IIS. Zastąpienie `ExtensionlessUrlHandler-Integrated-4.0` umożliwia żądania OPTIONS skontaktować się z aplikacją, ponieważ rejestrację modułu domyślne zezwala tylko żądania GET, HEAD, POST i debugowania przy użyciu adresów URL bez rozszerzenia.

## <a name="scope-rules-for-enablecors"></a>Zakres reguły [EnableCors]

W aplikacji, można włączyć mechanizm CORS każdej akcji, każdy kontroler lub globalnie dla wszystkich kontrolerów składnika Web API.

**Za akcję**

Aby włączyć mechanizm CORS dla jednej akcji, należy ustawić **[EnableCors]** atrybutu metody akcji. Poniższy przykład umożliwia włączenie mechanizmu CORS dla `GetItem` tylko metody.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Per Controller**

Jeśli ustawisz **[EnableCors]** klasy kontrolera ma zastosowanie do wszystkich akcji w kontrolerze. Aby wyłączyć CORS dla akcji, należy dodać **[DisableCors]** atrybutu akcji. Poniższy przykład umożliwia CORS dla każdej metody, z wyjątkiem `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globally**

Aby włączyć mechanizm CORS dla wszystkich kontrolerów składnika Web API w aplikacji, należy przekazać **EnableCorsAttribute** wystąpienia do **EnableCors** metody:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Jeśli ten atrybut zostanie ustawiony na więcej niż jednego zakresu, jest kolejność według priorytetu:

1. Akcja
2. Kontroler
3. Global

## <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

*Źródła* parametru **[EnableCors]** atrybut określa, które źródła są dozwolone dostępu do zasobu. Wartość jest rozdzielaną przecinkami listę dozwolonych źródeł.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Możesz także użyć wartości symbolu wieloznacznego "\*" Aby zezwolić na żądania z dowolnego źródła.

Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła. Oznacza to, że dosłownie w dowolnej witrynie sieci Web może wykonywać wywołania AJAX, do interfejsu API sieci web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Ustaw dozwolone metody HTTP

*Metody* parametru **[EnableCors]** atrybut określa metody HTTP, które mogą uzyskać dostępu do zasobu. Aby zezwolić na wszystkie metody, należy użyć wartości symbolu wieloznacznego "\*". Poniższy przykład umożliwia tylko żądania GET i POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

W tym artykule opisano wcześniej, jak żądania wstępnego mogą obejmować nagłówka Access-Control-Request-Headers, lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania"). *Nagłówki* parametru **[EnableCors]** atrybut określa, które autor nagłówki żądania są dozwolone. Aby zezwolić na wszystkie nagłówki, *nagłówki* do "\*". Aby nagłówki określonej listy dozwolonych adresów, należy ustawić *nagłówki* do listy dozwolone nagłówki oddzielone przecinkami:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Jednak przeglądarki nie są całkowicie zgodne, w jaki ustawiają Access-Control-Request-Headers. Na przykład dla programu Chrome obecnie dotyczy to "origin". FireFox nie ma standardowych nagłówków, takie jak "Akceptuj", nawet wtedy, gdy aplikacja ustawia je w skrypcie.

Jeśli ustawisz *nagłówki* do żadnego elementu innego niż "\*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type" i "origin", a także niestandardowe nagłówki, które mają być obsługiwane.

## <a name="set-the-allowed-response-headers"></a>Ustaw dozwolonych nagłówków odpowiedzi

Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji. Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

- Cache-Control
- Język zawartości
- Typ zawartości
- Wygasa
- Last-Modified
- Pragma

Specyfikacja CORS wywołuje te [nagłówki odpowiedzi proste](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Aby udostępnić innych nagłówków do aplikacji, należy ustawić *exposedHeaders* parametru **[EnableCors]**.

W poniższym przykładzie, kontroler firmy `Get` metoda ustawia niestandardowy nagłówek o nazwie "X-Custom-Header". Domyślnie przeglądarka nie udostępni tego nagłówka w żądaniu cross-origin. Aby udostępnić nagłówka, należy dołączyć "X-Custom-Header" *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Przekazywanie poświadczeń żądań cross-origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin. Poświadczenia obejmują pliki cookie, a także schematów uwierzytelniania HTTP. Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona **XMLHttpRequest.withCredentials** na wartość true.

Za pomocą **XMLHttpRequest** bezpośrednio:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

W technologii jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Ponadto serwer musi zezwalać na poświadczenia. Aby zezwolić na poświadczenia cross-origin w interfejsie API sieci Web, należy ustawić **SupportsCredentials** właściwości na wartość TRUE **[EnableCors]** atrybutu:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Jeśli ta właściwość ma wartość true, odpowiedź HTTP będzie zawierać nagłówek dostępu — kontrola-Allow-Credentials. Ten nagłówek informuje przeglądarkę o serwer zezwala na poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-kontroli-Allow-Credentials, przeglądarka nie będzie ujawniać odpowiedź do aplikacji, a żądanie AJAX nie powiodło się.

Należy zachować ostrożność ustawienie **SupportsCredentials** na wartość true, ponieważ oznacza witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do internetowego interfejsu API w imieniu użytkownika, bez angażowania użytkownika. Specyfikacja CORS również stany to ustawienie *źródła* do &quot; \* &quot; jest nieprawidłowa Jeśli **SupportsCredentials** ma wartość true.

## <a name="custom-cors-policy-providers"></a>Niestandardowi dostawcy zasad CORS

**[EnableCors]** atrybutu implementuje **ICorsPolicyProvider** interfejsu. Możesz podać Twojej własnej implementacji, tworząc klasę, która pochodzi od klasy **atrybut** i implementuje **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Teraz można zastosować atrybut, w dowolnym miejscu, że możesz umieścić **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Na przykład niestandardowy Dostawca zasad CORS można odczytać ustawień z pliku konfiguracji.

Jako alternatywa przy użyciu atrybutów, możesz zarejestrować się **ICorsPolicyProviderFactory** obiektu, który tworzy **ICorsPolicyProvider** obiektów.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Aby ustawić **ICorsPolicyProviderFactory**, wywołania **SetCorsPolicyProviderFactory** — metoda rozszerzenia przy uruchamianiu w następujący sposób:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Obsługa przeglądarek

Pakietów mechanizmu CORS interfejsu API sieci Web to technologia po stronie serwera. Przeglądarka musi także obsługę mechanizmu CORS. Na szczęście zawierają aktualne wersje wszystkie popularne przeglądarki [Obsługa mechanizmu CORS](http://caniuse.com/cors).
