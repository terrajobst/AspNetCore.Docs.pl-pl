---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "Włączanie żądań Cross-Origin w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Przedstawia sposób obsługi udostępniania zasobów między źródłami (CORS) w interfejsie API sieci Web ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Włączanie żądań Cross-Origin w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Poziom zabezpieczeń przeglądarki uniemożliwia wprowadzanie żądania AJAX do innej domeny przez stronę sieci web. To ograniczenie jest nazywany *zasad samego pochodzenia*i zapobiega złośliwa witryna odczytywanie danych poufnych z innej lokacji. Jednak czasami może mają mieć możliwość innych witryn wywołania interfejsu API sieci web.
> 
> [Krzyżowe udostępniania zasobów pochodzenia](http://www.w3.org/TR/cors/) (CORS) jest standardem W3C, dzięki której serwer złagodzenie zasad tego samego źródła. Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin podczas odrzucenia innych użytkowników. CORS jest bezpieczniejsze i bardziej elastyczne niż wcześniejszych metod takich jak [JSONP](http://en.wikipedia.org/wiki/JSONP). Ten samouczek pokazuje, jak włączyć mechanizm CORS w aplikacji interfejsu API sieci Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - 2.2 interfejsu API sieci Web


<a id="intro"></a>
## <a name="introduction"></a>Wprowadzenie

W tym samouczku przedstawiono Obsługa mechanizmu CORS interfejsu API sieci Web ASP.NET. Zaczniemy przez utworzenie dwóch projektów ASP.NET — jedną o nazwie "Usługa sieci Web", obsługująca kontroler Web API i innych o nazwie "WebClient", która wywołuje Usługa sieci Web. Ponieważ aplikacje są przechowywane w różnych domenach, żądanie AJAX z WebClient do usługi sieci Web jest żądaniem cross-origin.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Co to jest "Tego samego źródła"?

Dwa adresy URL mają tego samego źródła, gdy mają identyczne schematy, hostów i portów. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Te dwa adresy URL są tego samego źródła:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Tych adresów URL mają różne źródła niż poprzedniej dwóch:

- `http://example.net`-Innej domeny
- `http://example.com:9000/foo.html`-Różnych portów:
- `https://example.com/foo.html`-Inny schemat
- `http://www.example.com/foo.html`-Różnych poddomeny

> [!NOTE]
> Program Internet Explorer nie należy wziąć pod uwagę portu podczas porównywania źródeł.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Tworzenie projektu usługi sieci Web

> [!NOTE]
> W tej sekcji założono, że już wiesz, jak tworzyć projekty interfejsu API sieci Web. Jeśli nie, zobacz [wprowadzenie do składnika ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Uruchom program Visual Studio i utworzyć nową **aplikacji sieci Web ASP.NET** projektu. Wybierz **pusty** szablonu projektu. W obszarze ". Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** wyboru. Opcjonalnie wybierz opcję "Hostuj w chmurze", aby wdrożyć aplikację pakietu Microsoft Azure. Firma Microsoft oferuje usługi hostingu sieci web wolne dla maksymalnie 10 witryn internetowych w [bezpłatnego konta wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Dodawanie kontrolera interfejsu API sieci Web o nazwie `TestController` z następującym kodem:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Możesz uruchomić aplikację lokalnie lub wdrożyć na platformie Azure. (Zrzuty ekranu w tym samouczku I wdrożenie aplikacji sieci Web usługi aplikacji Azure.) Aby sprawdzić, czy działa interfejsu API sieci web, przejdź do `http://hostname/api/test/`, gdzie *hostname* domenę, w której została wdrożona aplikacja. Tekst odpowiedzi powinna zostać wyświetlona &quot;pobrać: wiadomości testowej&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Utwórz projekt WebClient

Utwórz innego projektu aplikacji sieci Web ASP.NET i wybierz **MVC** szablonu projektu. Opcjonalnie wybierz **Zmień uwierzytelnianie** > **bez uwierzytelniania**. Uwierzytelnianie nie jest wymagany na potrzeby tego samouczka.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

W Eksploratorze rozwiązań Otwórz plik Views/Home/Index.cshtml. Zastąp kod w tym pliku z następujących czynności:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Aby uzyskać *serviceUrl* zmiennej, użyj identyfikatora URI aplikacji usługi sieci Web. Teraz uruchom aplikację WebClient lokalnie lub opublikować go do innej witryny sieci Web.

Kliknięcie przycisku "Spróbuj on" przesyła żądanie AJAX do aplikacji usługi sieci Web, przy użyciu metody HTTP wymienione w polu listy rozwijanej (GET, POST i PUT). Pozwala to uzyskać zbadania różnych żądań cross-origin. Teraz, aplikacji usługi sieci Web nie obsługuje mechanizmu CORS, więc po kliknięciu przycisku spowoduje błąd.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Jeśli Obejrzyj ruchu HTTP w narzędziu, takich jak [Fiddler](http://www.telerik.com/fiddler), zostanie wyświetlony w przeglądarce wysyłania żądania GET i żądanie zakończy się powodzeniem, że wywołanie AJAX zwraca błąd. Ważne jest, aby zrozumieć zasady samego pochodzenia nie zapobiega przeglądarki z *wysyłania* żądania. Zamiast tego zapobiega aplikacji wyświetlania *odpowiedzi*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Włączanie mechanizmu CORS

Teraz załóżmy Włączanie mechanizmu CORS w aplikacji usługi sieci Web. Najpierw Dodaj pakiet CORS NuGet. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

To polecenie instaluje najnowszy pakiet i aktualizuje wszystkie zależności, w tym podstawowe biblioteki interfejsu API sieci Web. Użytkownik flagi wersji pod kątem określonej wersji. Pakiet CORS wymaga składnika Web API w wersji 2.0 lub nowszej.

Otwórz plik aplikacji\_Start/WebApiConfig.cs. Dodaj następujący kod do **WebApiConfig.Register** metody.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Następnie dodaj **[EnableCors]** atrybutu `TestController` klasy:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Aby uzyskać *źródeł* parametru, użyj identyfikatora URI, do której została wdrożona aplikacja WebClient. Dzięki temu żądań cross-origin z WebClient, podczas nadal brak zezwolenia wszystkich innych żądaniach między domenami. Później będzie opisano parametry **[EnableCors]** bardziej szczegółowo.

Nie dołączaj ukośnikiem na końcu *źródeł* adresu URL.

Należy ponownie wdrożyć zaktualizowane aplikacji usługi sieci Web. Nie trzeba zaktualizować WebClient. Teraz żądania AJAX z WebClient powinna zakończyć się pomyślnie. Metody GET i PUT, POST wszystkie dozwolone.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Jak działa CORS

W tej sekcji opisano, co się stanie w żądanie CORS na poziomie wiadomości HTTP. Ważne jest, aby zrozumieć, jak działa CORS, dzięki czemu można skonfigurować **[EnableCors]** atrybutu poprawnie i rozwiązywanie problemów, jeśli elementy nie działają zgodnie z oczekiwaniami.

Specyfikacja CORS wprowadzono kilka nowych nagłówków HTTP, które Włączanie żądań cross-origin. Jeśli przeglądarka obsługuje mechanizm CORS, ustawia automatycznie tych nagłówków żądań cross-origin; nie trzeba wykonywać żadnych czynności specjalne w kodzie JavaScript.

Oto przykład żądań cross-origin. Nagłówek "Origin" daje domeny witryny, do której wysłano żądanie.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Jeśli serwer zezwala na żądanie, ustawia dla nagłówka Access-Control-Allow-Origin. Wartość tego nagłówka odpowiada nagłówka źródła albo ma wartość symbolu wieloznacznego "\*", co oznacza, że każde źródło jest dozwolone.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiodło się. W szczególności przeglądarki nie zezwala na żądanie. Nawet wtedy, gdy serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnia odpowiedzi aplikacji klienckiej.

**Żądania wstępnego**

Dla niektórych żądań CORS przeglądarce wysyła żądanie dodatkowych, o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu.

Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:

- Metoda żądania jest GET, HEAD lub POST, *i*
- Aplikacja nie określa żadnych nagłówków żądania innego niż Akceptuj, Accept-Language, Content-Language, Content-Type lub Last-zdarzenia-ID *i*
- Nagłówek Content-Type (Jeśli ustawiona) jest jednym z następujących czynności: 

    - Application/x--www-form-urlencoded
    - dane multipart/formularza
    - zwykły tekst

Reguła o nagłówków żądań ma zastosowanie do nagłówki, które ustawia aplikacji przez wywołanie metody **setRequestHeader** na **XMLHttpRequest** obiektu. (Specyfikacja CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówków *przeglądarki* można ustawić, takie jak agenta użytkownika, hosta lub Content-Length.

Oto przykład żądania wstępnego:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Żądania wstępnego transmitowane używa metody OPTIONS protokołu HTTP. Zawiera dwa nagłówki specjalne:

- Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.
- Access-Control-Request-Headers: Lista nagłówków żądania który *aplikacji* ustawiać rzeczywistego żądania. (Ponownie, ta nie obejmuje nagłówki, które ustawia przeglądarki).

Oto przykład odpowiedzi, przy założeniu, że serwer zezwala na żądanie:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Odpowiedź zawiera nagłówek dostępu-formant-Allow-Methods, który wyświetla listę dozwolonych metod i opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolone nagłówki. Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Reguły zakresów dla [EnableCors]

Mechanizm CORS można włączyć każdej akcji, każdy kontroler lub globalnie do wszystkich kontrolerów interfejsu API sieci Web w aplikacji.

**Każdej akcji**

Aby włączyć mechanizm CORS dla jednej akcji, należy ustawić **[EnableCors]** atrybutu w metodzie akcji. Poniższy przykład umożliwia włączenie mechanizmu CORS dla `GetItem` tylko metody.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Każdy kontroler**

Jeśli ustawisz **[EnableCors]** klasy kontrolera, stosuje do wszystkich akcji w kontrolerze. Aby wyłączyć CORS dla akcji, Dodaj **[DisableCors]** atrybutu w celu wykonania akcji. Poniższy przykład umożliwia CORS dla każdej metody z wyjątkiem `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalny**

Aby włączyć mechanizm CORS dla wszystkich kontrolerów interfejsu API sieci Web w aplikacji, należy przekazać **EnableCorsAttribute** wystąpienie do **EnableCors** metody:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Jeśli ten atrybut zostanie ustawiony na więcej niż jednego zakresu, kolejność pierwszeństwa jest:

1. Akcja
2. Kontrolera
3. Globalne

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Ustaw dozwolonych źródeł

*Źródeł* parametr **[EnableCors]** atrybut określa, które źródła są dozwolone w celu dostępu do zasobu. Wartość jest rozdzielana przecinkami lista dozwolonych źródeł.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Możesz także użyć wartości symbolu wieloznacznego "\*" Aby zezwolić na żądania z dowolnego źródła.

Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła. Oznacza to, że dosłownie w dowolnej witrynie sieci Web można wywołań AJAX do interfejsu API sieci web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Ustaw metody HTTP dozwolonych

*Metody* parametr **[EnableCors]** atrybut określa, które metody HTTP będą mogli uzyskać dostępu do zasobu. Aby zezwolić na wszystkie metody, należy użyć wartości symbolu wieloznacznego "\*". Poniższy przykład umożliwia tylko żądania GET i POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Ustawia nagłówki żądania dozwolone

Wcześniej I opisano sposób żądania wstępnego może obejmować nagłówka Access-Control-Request-Headers lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania"). *Nagłówki* parametr **[EnableCors]** atrybut określa, które autor nagłówki żądania są dozwolone. Aby zezwolić na wszystkie nagłówki, *nagłówki* do "\*". Do listy dozwolonych adresów IP określonych nagłówków, ustaw *nagłówki* do rozdzielana przecinkami lista dozwolone nagłówki:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Jednak przeglądarki nie są całkowicie zgodne, w konfiguracji do programu Access-Control-Request-Headers. Na przykład Chrome zawiera obecnie "origin"; gdy FireFox nie obejmuje standardowych nagłówków, takie jak "Akceptuj", nawet wtedy, gdy aplikacja ustawia je w skrypcie.

Jeśli ustawisz *nagłówki* do żadnych innych niż "\*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type", "origin", a także niestandardowe nagłówki, które mają być obsługiwane.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Ustawianie nagłówków odpowiedzi dozwolone

Domyślnie przeglądarka nie ujawnia wszystkich nagłówków odpowiedzi do aplikacji. Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:

- Cache-Control
- Język zawartości
- Typ zawartości
- Wygasa
- Ostatniej modyfikacji
- Wartość dyrektywy pragma

Specyfikacja CORS wywołuje te [nagłówki odpowiedzi proste](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Aby udostępnić inne nagłówki do aplikacji, ustaw *exposedHeaders* parametr **[EnableCors]**.

W poniższym przykładzie kontrolera w `Get` metoda ustawia niestandardowy nagłówek o nazwie "X-niestandardowego nagłówka". Domyślnie przeglądarka nie powoduje to udostępnienie tego nagłówka w żądaniu cross-origin. Aby udostępnić nagłówka, Dołącz "X-niestandardowego nagłówka" *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Przekazywanie poświadczeń w żądań Cross-Origin

Poświadczenia wymagają specjalnej obsługi żądania CORS. Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin. Poświadczenia obejmują pliki cookie, a także schematy uwierzytelniania HTTP. Aby wysłać poświadczeń z żądaniem cross-origin, klient musi być ustawiona **XMLHttpRequest.withCredentials** na wartość true.

Przy użyciu **XMLHttpRequest** bezpośrednio:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

W jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Ponadto poświadczenia muszą zezwalać na serwerze. Aby zezwolić poświadczenia cross-origin w interfejsie API sieci Web, **SupportsCredentials** właściwości na wartość true na **[EnableCors]** atrybutu:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Jeśli ta właściwość ma wartość true, odpowiedź HTTP będzie zawierać nagłówka dostępu-formant-Allow-Credentials. Ten nagłówek informuje przeglądarkę, że serwer umożliwia poświadczenia dla żądań cross-origin.

Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka dostępu-formant-Allow-Credentials, przeglądarka nie powoduje to udostępnienie odpowiedzi do aplikacji i żądanie AJAX nie powiedzie się.

Należy ostrożnie ustawienie **SupportsCredentials** na wartość true, ponieważ oznacza witryny sieci Web w innej domenie można wysłać poświadczeń zalogowanego użytkownika do interfejsu API sieci Web w imieniu użytkownika bez wiedzy użytkownika. Specyfikacja CORS stany również ustawienie *źródeł* do &quot; \* &quot; jest nieprawidłowa Jeśli **SupportsCredentials** ma wartość true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Dostawców zasad CORS niestandardowych

**[EnableCors]** atrybutu implementuje **ICorsPolicyProvider** interfejsu. Możesz podać własne implementacji, tworząc klasę, która jest pochodną **atrybutu** i implementuje **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Teraz można zastosować atrybutu miejsce czy przełączyć **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Na przykład niestandardowe Dostawca zasad CORS można odczytać ustawień z pliku konfiguracji.

Zamiast przy użyciu atrybutów, możesz zarejestrować **ICorsPolicyProviderFactory** obiekt, który tworzy **ICorsPolicyProvider** obiektów.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Aby ustawić **ICorsPolicyProviderFactory**, wywołaj **SetCorsPolicyProviderFactory** — metoda rozszerzenia przy uruchamianiu w następujący sposób:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Obsługa przeglądarek

Pakiet mechanizmu CORS interfejsu API sieci Web to technologia po stronie serwera. Przeglądarka użytkownika również musi obsługiwać CORS. Na szczęście obejmują bieżące wersje wszystkie główne przeglądarki [obsługę mechanizmu CORS](http://caniuse.com/cors).

Program Internet Explorer 8 oraz Internet Explorer 9 ma częściowe obsługę mechanizmu CORS, za pomocą starszej wersji obiektu XDomainRequest a XMLHttpRequest. Aby uzyskać więcej informacji, zobacz [XDomainRequest - ograniczeń, ograniczenia i obejścia](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
