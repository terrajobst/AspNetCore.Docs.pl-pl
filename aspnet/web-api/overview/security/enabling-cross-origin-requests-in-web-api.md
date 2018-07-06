---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Włączanie żądań Cross-Origin we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Pokazuje sposób obsługi udostępniania zasobów między źródłami (CORS) w interfejsie API sieci Web platformy ASP.NET.
ms.author: aspnetcontent
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7ac6158c2365aa324cefe97db044f568a1a43795
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805415"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Włączanie żądań Cross-Origin we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Poziom zabezpieczeń przeglądarki uniemożliwia strony sieci web wprowadzanie wysyłanie żądań AJAX do innej domeny. To ograniczenie jest nazywany *zasadami tego samego źródła*i zapobiega złośliwych witryn odczytywanie poufnych danych z innej lokacji. Jednak czasami możesz chcieć umożliwić innych witryn, wywołania interfejsu API sieci web.
> 
> [Krzyżowe współużytkowanie zasobów](http://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne. CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej techniki takie jak [JSONP](http://en.wikipedia.org/wiki/JSONP). W tym samouczku pokazano, jak włączyć mechanizm CORS w aplikacji interfejsu API sieci Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Składnik Web API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Wprowadzenie

Ten samouczek pokazuje, że obsługa mechanizmu CORS w Web API platformy ASP.NET. Zaczniemy od utworzenia dwóch projektów platformy ASP.NET — jedną o nazwie "Usługa sieci Web", który hostuje kontrolera interfejsu API sieci Web, a inne o nazwie "Usługa WebClient", która wywołuje usługę sieci Web. Ponieważ dwie aplikacje są przechowywane w różnych domenach, żądanie AJAX z WebClient Usługa sieci Web jest żądaniem cross-origin.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Co to jest "Tego samego Origin"?

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


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Utwórz projekt usługi sieci Web

> [!NOTE]
> W tej sekcji założono, że użytkownik wie już, jak tworzyć projekty interfejsu API sieci Web. Jeśli nie, zobacz [wprowadzenie do interfejsu API sieci Web platformy ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Uruchom program Visual Studio i Utwórz nowy **aplikacji sieci Web ASP.NET** projektu. Wybierz **pusty** szablonu projektu. W obszarze "Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** pola wyboru. Opcjonalnie wybierz opcję "Hostuj w chmurze", aby wdrożyć aplikację pakietu Microsoft Azure. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci Web w [bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Dodawanie kontrolera interfejsu API sieci Web o nazwie `TestController` następującym kodem:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Można uruchomić aplikację lokalnie, lub Wdróż na platformie Azure. (Zrzuty ekranu, w tym samouczku, czy wdrożenie w usłudze Azure App Service Web Apps.) Aby sprawdzić, czy działa interfejs API sieci web, przejdź do `http://hostname/api/test/`, gdzie *hostname* jest domeną, w których wdrożono aplikację. Powinien zostać wyświetlony tekst odpowiedzi &quot;Uzyskaj: wiadomość testową&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Utwórz projekt WebClient

Utwórz inny projekt aplikacji sieci Web ASP.NET i wybierz **MVC** szablonu projektu. Opcjonalnie można zaznaczyć **Zmień uwierzytelnianie** > **bez uwierzytelniania**. Nie musisz uwierzytelniania na potrzeby tego samouczka.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

W Eksploratorze rozwiązań Otwórz plik Views/Home/Index.cshtml. Zastąp kod w tym pliku następujących czynności:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Aby uzyskać *serviceUrl* zmiennej, użyj identyfikatora URI aplikacji usługi sieci Web. Teraz uruchom aplikację modułu WebClient lokalnie lub opublikować ją do innej witryny sieci Web.

Kliknięcie przycisku "Try It" przesyła żądanie AJAX do aplikacji usługi sieci Web, za pomocą metody HTTP wymienionych w polu listy rozwijanej (GET, POST lub PUT). Pozwala to sprawdzić różne żądań cross-origin. W tej chwili, aplikacja usługi sieci Web nie obsługuje mechanizm CORS, więc jeśli klikniesz przycisk, otrzymają komunikat o błędzie.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Jeśli możesz obejrzeć ruch HTTP w narzędziu [Fiddler](http://www.telerik.com/fiddler), zostanie wyświetlony w przeglądarce wysyłania żądania GET i żądanie zakończy się powodzeniem, że wywołanie AJAX zwraca błąd. Jest ważne zrozumieć, zasadami jednego źródła, nie uniemożliwia przeglądarki z *wysyłania* żądania. Zamiast tego należy go zapobiega aplikację widzisz *odpowiedzi*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Włączanie mechanizmu CORS

Teraz możemy włączyć mechanizm CORS w aplikacji usługi sieci Web. Najpierw Dodaj pakiet NuGet mechanizmu CORS. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

To polecenie instaluje najnowszy pakiet i aktualizuje wszystkie zależności, w tym podstawowe biblioteki interfejsu API sieci Web. Użytkownik flagi wersji, pod kątem określonej wersji. Pakiet CORS wymaga internetowego interfejsu API w wersji 2.0 lub nowszej.

Otwórz plik App\_Start/WebApiConfig.cs. Dodaj następujący kod do **WebApiConfig.Register** metody.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Następnie dodaj **[EnableCors]** atrybutu `TestController` klasy:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Aby uzyskać *źródła* parametru, użyj identyfikatora URI, do której została wdrożona aplikacja klient sieci Web. Dzięki temu żądań cross-origin z WebClient, podczas nadal nie można przydzielać wszystkich innych żądaniach między domenami. Później będziesz opisano parametry **[EnableCors]** bardziej szczegółowo.

Nie dołączaj ukośnika na końcu *źródła* adresu URL.

Ponownie wdróż zaktualizowaną aplikację usługi sieci Web. Nie musisz zaktualizować WebClient. Teraz żądanie AJAX z WebClient powinna zakończyć się pomyślnie. Metody GET, PUT i POST wszystkie dozwolone.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
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

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Zakres reguły [EnableCors]

W aplikacji, można włączyć mechanizm CORS każdej akcji, każdy kontroler lub globalnie dla wszystkich kontrolerów składnika Web API.

**Za akcję**

Aby włączyć mechanizm CORS dla jednej akcji, należy ustawić **[EnableCors]** atrybutu metody akcji. Poniższy przykład umożliwia włączenie mechanizmu CORS dla `GetItem` tylko metody.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Każdy kontroler**

Jeśli ustawisz **[EnableCors]** klasy kontrolera ma zastosowanie do wszystkich akcji w kontrolerze. Aby wyłączyć CORS dla akcji, należy dodać **[DisableCors]** atrybutu akcji. Poniższy przykład umożliwia CORS dla każdej metody, z wyjątkiem `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalnie**

Aby włączyć mechanizm CORS dla wszystkich kontrolerów składnika Web API w aplikacji, należy przekazać **EnableCorsAttribute** wystąpienia do **EnableCors** metody:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Jeśli ten atrybut zostanie ustawiony na więcej niż jednego zakresu, jest kolejność według priorytetu:

1. Akcja
2. Kontroler
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Ustaw dozwolone źródła

*Źródła* parametru **[EnableCors]** atrybut określa, które źródła są dozwolone dostępu do zasobu. Wartość jest rozdzielaną przecinkami listę dozwolonych źródeł.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Możesz także użyć wartości symbolu wieloznacznego "\*" Aby zezwolić na żądania z dowolnego źródła.

Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła. Oznacza to, że dosłownie w dowolnej witrynie sieci Web może wykonywać wywołania AJAX, do interfejsu API sieci web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Ustaw metody HTTP dozwolonych

*Metody* parametru **[EnableCors]** atrybut określa metody HTTP, które mogą uzyskać dostępu do zasobu. Aby zezwolić na wszystkie metody, należy użyć wartości symbolu wieloznacznego "\*". Poniższy przykład umożliwia tylko żądania GET i POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

Wcześniej I opisano sposób żądania wstępnego mogą obejmować nagłówka Access-Control-Request-Headers lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania"). *Nagłówki* parametru **[EnableCors]** atrybut określa, które autor nagłówki żądania są dozwolone. Aby zezwolić na wszystkie nagłówki, *nagłówki* do "\*". Aby nagłówki określonej listy dozwolonych adresów, należy ustawić *nagłówki* do listy dozwolone nagłówki oddzielone przecinkami:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Jednak przeglądarki nie są całkowicie zgodne, w jaki ustawiają Access-Control-Request-Headers. Na przykład w przeglądarce Chrome zawiera obecnie "origin"; gdy FireFox nie ma standardowych nagłówków, takie jak "Akceptuj", nawet wtedy, gdy aplikacja ustawia je w skrypcie.

Jeśli ustawisz *nagłówki* do żadnego elementu innego niż "\*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type" i "origin", a także niestandardowe nagłówki, które mają być obsługiwane.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Ustaw dozwolonych nagłówków odpowiedzi

Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji. Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

- Cache-Control
- Język zawartości
- Typ zawartości
- Wygasa
- Last-Modified
- Dyrektywy pragma

Specyfikacja CORS wywołuje te [nagłówki odpowiedzi proste](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Aby udostępnić innych nagłówków do aplikacji, należy ustawić *exposedHeaders* parametru **[EnableCors]**.

W poniższym przykładzie, kontroler firmy `Get` metoda ustawia niestandardowy nagłówek o nazwie "X-Custom-Header". Domyślnie przeglądarka nie udostępni tego nagłówka w żądaniu cross-origin. Aby udostępnić nagłówka, należy dołączyć "X-Custom-Header" *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Przekazywanie poświadczeń w żądań Cross-Origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin. Poświadczenia obejmują pliki cookie, a także schematów uwierzytelniania HTTP. Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona **XMLHttpRequest.withCredentials** na wartość true.

Za pomocą **XMLHttpRequest** bezpośrednio:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

W technologii jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Ponadto serwer musi zezwalać na poświadczenia. Aby zezwolić na poświadczenia cross-origin w interfejsie API sieci Web, należy ustawić **SupportsCredentials** właściwości na wartość TRUE **[EnableCors]** atrybutu:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Jeśli ta właściwość ma wartość true, odpowiedź HTTP będzie zawierać nagłówek dostępu — kontrola-Allow-Credentials. Ten nagłówek informuje przeglądarkę o serwer zezwala na poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-kontroli-Allow-Credentials, przeglądarka nie będzie ujawniać odpowiedź do aplikacji, a żądanie AJAX nie powiodło się.

Należy zachować ostrożność bardzo ustawienie **SupportsCredentials** na wartość true, ponieważ oznacza witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do internetowego interfejsu API w imieniu użytkownika, bez angażowania użytkownika. Specyfikacja CORS również stany to ustawienie *źródła* do &quot; \* &quot; jest nieprawidłowa Jeśli **SupportsCredentials** ma wartość true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Dostawcy zasad niestandardowego mechanizmu CORS

**[EnableCors]** atrybutu implementuje **ICorsPolicyProvider** interfejsu. Możesz podać Twojej własnej implementacji, tworząc klasę, która pochodzi od klasy **atrybut** i implementuje **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Teraz można zastosować atrybut, w dowolnym miejscu, że możesz umieścić **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Na przykład niestandardowy Dostawca zasad CORS można odczytać ustawień z pliku konfiguracji.

Jako alternatywa przy użyciu atrybutów, możesz zarejestrować się **ICorsPolicyProviderFactory** obiektu, który tworzy **ICorsPolicyProvider** obiektów.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Aby ustawić **ICorsPolicyProviderFactory**, wywołania **SetCorsPolicyProviderFactory** — metoda rozszerzenia przy uruchamianiu w następujący sposób:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Obsługa przeglądarek

Pakietów mechanizmu CORS interfejsu API sieci Web to technologia po stronie serwera. Przeglądarka musi także obsługę mechanizmu CORS. Na szczęście zawierają aktualne wersje wszystkie popularne przeglądarki [Obsługa mechanizmu CORS](http://caniuse.com/cors).

Program Internet Explorer 8 oraz Internet Explorer 9 mają częściowa Obsługa mechanizmu CORS za pomocą starszej wersji obiektu XDomainRequest zamiast XMLHttpRequest. Aby uzyskać więcej informacji, zobacz [XDomainRequest — ograniczenia, ograniczeń i obejścia](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
