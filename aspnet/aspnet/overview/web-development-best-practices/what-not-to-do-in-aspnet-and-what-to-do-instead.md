---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Czego nie robić na platformie ASP.NET i co zrobić zamiast tego | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym temacie opisano kilka typowych pomyłek, których wiele osób wprowadza w projektach programu ASP.NET w sieci web. Zawiera on zalecenia dotyczące co należy zrobić, aby uniknąć tych commo...
ms.author: aspnetcontent
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 9e9b192126995ac8a461b15bce69b60d57ca9ba1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806021"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Czego nie robić na platformie ASP.NET i co zrobić zamiast tego
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym temacie opisano kilka typowych pomyłek, których wiele osób wprowadza w projektach programu ASP.NET w sieci web. Zawiera on zalecenia dotyczące co należy zrobić, aby uniknąć tych typowych pomyłek. Jest on oparty na [prezentacji](http://vimeo.com/68390507) przez **Damianem Edwardsem** na norweskiej konferencji deweloperów.


## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

W tym temacie nie ma służyć jako kompletny przewodnik, aby upewnić się, że aplikacja jest bezpieczny i skuteczny. Nadal należy stosować najlepsze rozwiązania dotyczące zabezpieczeń i wydajności, które nie zostały opisane w tym temacie. Sugerują one tylko sposoby unikania typowych błędów związanych z klas .NET i procesów.

## <a name="overview"></a>Omówienie

Ten temat zawiera następujące sekcje:

- [Zgodność ze standardami](#standards)

    - [Kontrolki karty](#adapters)
    - [Właściwości stylu dla formantów](#styleprop)
    - [Strony i wywołań zwrotnych kontroli](#callback)
    - [Wykrywanie możliwości przeglądarki](#browsercap)
- [Zabezpieczenia](#security)

    - [Żądanie weryfikacji](#validation)
    - [Uwierzytelnianie formularzy cookieless i sesji](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Trybie średniego zaufania](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Niezawodność i wydajność](#performance)

    - [PreSendRequestHeaders i PreSendRequestContent](#presend)
    - [Zdarzenia asynchroniczne strony za pomocą formularzy sieci Web](#asyncevents)
    - [Ogień i zapominać pracy](#fire)
    - [Treść jednostki żądania](#requestentity)
    - [Response.Redirect i Response.End](#redirect)
    - [EnableViewState i ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Długie żądania uruchomione (> 110 w sekundach)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Zgodność ze standardami

<a id="adapters"></a>

### <a name="control-adapters"></a>Kontrolki karty

Zalecenie: Zatrzymaj renderowanie adaptacyjne za pomocą adapterów kontrolek, a zamiast tego użyć zapytaniami multimediów CSS i HTML zgodnych ze standardami.

Formanty karty zostały wprowadzone w .NET 2.0 do renderowania kodu prezentacji, który został dostosowany do różnych urządzeń i środowisk. Teraz można osiągnąć ten adaptacyjne renderowania przy użyciu CSS i HTML. Należy zaprzestać korzystania z adapterów kontrolek i przekonwertować żadnych istniejących kart CSS i HTML.

Aby uzyskać więcej informacji, zobacz [zapytaniami multimediów](http://www.w3.org/TR/css3-mediaqueries/) i [instrukcje: Dodawanie stron Mobile Your wzorca ASP.NET Web Forms / aplikacji MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Właściwości stylu dla formantów

Zalecenie: Zatrzymaj, ustawienie wartości stylu w znacznikach kontroli, a zamiast tego ustawienia formatowania wartości w arkusze stylów CSS.

Formanty serwera sieci Web zawierają dziesiątek, jak właściwości, które mogą być używane do ustawiania właściwości stylu w tekście. Na przykład ForeColor właściwość ustawia kolor tekstu dla formantu. Można osiągnąć ten sam efekt, bardziej wydajnie za pomocą arkuszy stylów CSS. Arkusze stylów umożliwiają scentralizowanie wartości stylu i zapobiec ustawianiu tych wartości w całej aplikacji.

Poniższy przykład przedstawia klasę CSS tekstu zestawy na czerwony.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Następny przykład pokazuje, jak dynamicznie zastosować klasę CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Strony i wywołań zwrotnych kontroli

Zalecenie: Zatrzymaj, za pomocą wywołania zwrotne strony i kontrolki, a zamiast tego użyć dowolnej z następujących czynności: AJAX, UpdatePanel, metod akcji MVC, interfejs API sieci Web lub SignalR.

We wcześniejszych wersjach programu ASP.NET metody wywołania zwrotnego strony i kontrolki włączone aktualizację części strony sieci web bez odświeżania całej strony. Teraz można wykonać aktualizacji stron częściowych przy użyciu [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [interfejsu API sieci Web](../../../web-api/index.md) lub [SignalR](../../../signalr/index.md). Należy zatrzymać przy użyciu metod wywołania zwrotnego, ponieważ mogą one spowodować problemy z przyjaznymi adresami URL i routing. Domyślnie formanty, nie należy włączać metody wywołania zwrotnego, ale jeśli włączono tę funkcję w kontrolce, należy wyłączyć je.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Wykrywanie możliwości przeglądarki

Zalecenie: Zatrzymaj, przy użyciu przeglądarki statyczne możliwości wykrywania, a zamiast tego użyj funkcji dynamicznej wykrywania.

We wcześniejszych wersjach programu ASP.NET obsługiwane funkcje w każdej przeglądarce są przechowywane w pliku XML. Wykrywanie obsługi różnych funkcji za pomocą statycznych wyszukiwania nie jest najlepszym rozwiązaniem. Teraz można dynamicznie wykryć przeglądarki użytkownika obsługiwane funkcje przy użyciu funkcji wykrywania struktury, takich jak [Modernizr](http://modernizr.com/). Funkcja wykrywania określa pomocy technicznej podjęto próbę użycia metody lub właściwości, a następnie zaznaczając, aby zobaczyć, jeśli przeglądarki generowane oczekiwany rezultat. Domyślnie Modernizr znajduje się w szablonach aplikacji sieci Web.

<a id="security"></a>

## <a name="security"></a>Zabezpieczenia

<a id="validation"></a>

### <a name="request-validation"></a>Żądanie weryfikacji

Zalecenie: Sprawdzanie poprawności danych wejściowych użytkownika i kodowanie danych wyjściowych z użytkowników.

Weryfikacja żądania jest funkcją programu ASP.NET, która sprawdza każde żądanie i zatrzymuje żądania, jeśli zostanie znaleziony potencjalnych zagrożeń. Nie są zależne od weryfikację żądań dla zabezpieczania aplikacji przed atakami skryptów między witrynami. Zamiast tego należy sprawdzić poprawność wszystkich danych wejściowych od użytkowników i kodowanie danych wyjściowych. W niektórych przypadkach ograniczonych wyrażeń regularnych można użyć, aby sprawdzić poprawność danych wejściowych, ale w przypadku bardziej skomplikowane, które należy sprawdzić, czy dane wejściowe użytkownika za pomocą klas platformy .NET, które sprawdza, czy wartość jest zgodna, dozwolone wartości.

Poniższy przykład przedstawia sposób użycia metody statycznej w klasie identyfikatora Uri do ustalenia, czy identyfikator Uri, podane przez użytkownika jest nieprawidłowa.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Jednak aby wystarczająco sprawdzić identyfikator Uri, należy także sprawdzić się upewnić, że Określa ona `http` lub `https`. W poniższym przykładzie użyto metody wystąpienia, aby sprawdzić, czy identyfikator Uri jest prawidłowa.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Przed Renderowanie danych wejściowych użytkownika w formacie HTML, lub danych wejściowych użytkownika, np. w zapytaniu SQL, należy zakodować wartości, aby upewnić się, że złośliwy kod nie jest uwzględniony.

Możesz HTML Koduj wartość w znacznikach z &lt;%: %&gt; składni, jak pokazano poniżej.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Lub, w składni Razor HTML kodowanie za pomocą @, jak pokazano poniżej.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

W następnym przykładzie pokazano sposób w formacie HTML zakodować wartości w związanym z kodem.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Bezpiecznie zakodować wartości poleceń SQL, użyj parametrów polecenia takie jak [parametr SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Uwierzytelnianie formularzy cookieless i sesji

Zalecenie: Wymagaj plików cookie.

Przekazywanie informacji uwierzytelniania w ciągu zapytania nie jest bezpieczne. Dlatego wymaga plików cookie, jeśli aplikacja zawiera uwierzytelniania. Jeśli Twojego pliku cookie są przechowywane poufne informacje, należy rozważyć wymaganie protokołu SSL dla pliku cookie.

Poniższy przykład pokazuje, jak określić w pliku Web.config, że uwierzytelnianie formularzy wymaga pliku cookie, które są przesyłane za pośrednictwem protokołu SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Zalecenie: Nigdy nie ma wartość false.

Domyślnie EnbableViewStateMac jest ustawiona na wartość true. Nawet wtedy, gdy aplikacja nie używa stanu widoku, nie należy ustawiać EnableViewStateMac na wartość false. Ustawienie wartości FALSE spowoduje, że aplikacja narażone na wykonywanie skryptów między witrynami.

Począwszy od platformy ASP.NET 4.5.2, środowisko wykonawcze wymusza **EnableViewStateMac = true**. Nawet wtedy, gdy zostanie ustawiona na wartość false, środowisko uruchomieniowe ignoruje tę wartość i będzie kontynuowane z ustawioną wartość true. Aby uzyskać więcej informacji, zobacz [ASP.NET 4.5.2 i EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Poniższy przykład pokazuje, jak ustawić EnableViewStateMac na wartość true. Nie trzeba faktycznie Ustaw tę wartość na wartość true, ponieważ jest prawdą, domyślnie. Jednakże jeśli ustawiono go na wartość false, na dowolnej stronie w aplikacji, należy poprawić natychmiast tę wartość.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Trybie średniego zaufania

Zalecenie: Nie są zależne od zaufania Medium (lub inne poziom zaufania) pełnią funkcję granicy zabezpieczeń.

Częściowej relacji zaufania nie chronią odpowiednio aplikacji i nie powinna być używana. Zamiast tego należy używać pełnego zaufania i izolowania niezaufanych aplikacji w osobnych pulach aplikacji. Ponadto należy uruchomić każdy unikatową tożsamość puli aplikacji. Aby uzyskać więcej informacji, zobacz [częściowego zaufania programu ASP.NET nie gwarantuje izolacji aplikacji](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Zalecenie: Nie należy wyłączać ustawienia zabezpieczeń w &lt;appSettings&gt; elementu.

AppSettings element zawiera wiele wartości, które są wymagane dla aktualizacji zabezpieczeń. Nie należy zmienić lub wyłączyć te wartości. Jeśli konieczne jest wyłączenie tych wartości podczas wdrażania aktualizacji, natychmiast ponownie włączyć po ukończeniu wdrażania.

Aby uzyskać więcej informacji, zobacz [Element appSettings platformy ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Zalecenie: Użyj [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) zamiast tego.

Metoda UrlPathEncode został dodany do programu .NET Framework, aby rozwiązać problem ze zgodnością bardzo przeglądarki. Odpowiednio nie przeprowadza kodowania adresu URL, a nie chroni aplikację przed skryptów między witrynami. Powinno nigdy nie używaj go w aplikacji. Zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Poniższy przykład pokazuje, jak przekazać adres URL zakodowany jako parametr ciągu zapytania kontrolki hiperlinku.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Niezawodność i wydajność

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders i PreSendRequestContent

Zalecenie: Nie należy używać tych zdarzeń z modułów zarządzanych. Zamiast tego należy zapisać moduł macierzysty usług IIS w celu wykonania wymaganych zadań. Zobacz [tworzenie moduły HTTP kodu natywnego](https://msdn.microsoft.com/library/ms693629.aspx).

Możesz użyć [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) i [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) zdarzeń z modułów natywnych usług IIS.
> [!WARNING]
> Nie używaj `PreSendRequestHeaders` i `PreSendRequestContent` z modułów zarządzanych, które implementują `IHttpModule`. Ustawienie tych właściwości mogą powodować problemy z żądań asynchronicznych. Kombinacja żądane Routing aplikacji (ARR) i technologia websockets może prowadzić do wyjątki naruszenie zasad dostępu, które może spowodować, że w3wp ulega awarii. Na przykład iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 w iiscore.dll spowodowała wyjątek naruszenie zasad dostępu (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Zdarzenia asynchroniczne strony za pomocą formularzy sieci Web

Zalecenie: W formularzach sieci Web należy unikać pisania asynchronicznej metody void zdarzenia cyklu życia strony, a zamiast tego użyć [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) dla kodu asynchronicznego.

Po oznaczeniu zdarzeń strony, przy użyciu **async** i **void**, nie można określić, kiedy kod asynchroniczny zostało zakończone. Zamiast tego należy użyć Page.RegisterAsyncTask do uruchomienia kodu asynchronicznego w sposób, który pozwala na śledzenie jego zakończenia.

W poniższym przykładzie pokazano, a przycisk kliknij program obsługi, który zawiera kod asynchroniczny. W tym przykładzie zawiera wartość ciągu do czytania asynchronicznie, inaczej niż tylko jako uproszczony przykład asynchroniczne zadanie, a nie zalecanym rozwiązaniem.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Jeśli używasz zadań asynchronicznych ustawić platformę docelową środowiska wykonawczego protokołu Http do wersji 4.5 w pliku Web.config. Ustawienia platformy docelowej do 4.5 włącza na nowy kontekst synchronizacji został dodany w .NET 4.5. Ta wartość jest ustawiana domyślnie w nowych projektach programu Visual Studio 2012, ale nie można ustawić, jeśli pracujesz z istniejącego projektu.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Ogień i zapominać pracy

Zalecenie: Podczas obsługi żądania w programie ASP.NET, należy unikać uruchamiania pożarowego i zapominać pracy (takie wywołanie metody ThreadPool.QueueUserWorkItem lub tworzenia czasomierza, który wielokrotnie wywołuje delegata).

Jeśli aplikacja ma pożarowego i zapominać pracy jest uruchamiany w ramach programu ASP.NET, aplikację można uzyskać zsynchronizowane. W dowolnym momencie domeny aplikacji może zostać zniszczone co oznacza, że proces ciągłego mogą nie odpowiadać bieżący stan aplikacji.

Przeniesienie tego typu pracy poza programem ASP.NET. Możesz użyć zadania Web Job, usługa Windows lub roli procesu roboczego na platformie Azure do wykonywania pracy w toku i uruchomić kod z innego procesu.

Jeśli konieczne jest wykonanie tej pracy w programie ASP.NET, można dodać pakietu Nuget o nazwie [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) do uruchomienia kodu.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Treść jednostki żądania

Zalecenie: Unikaj czytania Request.Form Request.InputStream przed programu obsługi zdarzeń.

Najwcześniejsza którymi należy zapoznać się z Request.Form lub Request.InputStream jest podczas obsługi wykonywania zdarzeń. W przypadku platformy MVC kontroler jest programem obsługi i zdarzenie wykonania jest po uruchomieniu metody akcji. W formularzach sieci Web strona jest program obsługi, a zdarzenie wykonania jest gdy zostanie wyzwolony zdarzeń Page.Init. Jeśli treść jednostki żądania możesz odczytać starszych niż zdarzenie wykonania, kolidować z przetwarzaniem żądania.

Jeśli zachodzi potrzeba odczytania treści jednostki żądania przed zdarzeniem execute, użyj jednej [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) lub [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Użycie opcji GetBufferlessInputStream, uzyskiwanie pierwotne strumienia żądania, a na siebie odpowiedzialność za przetwarzanie całego żądania. Po wywołaniu GetBufferlessInputStream, Request.Form i Request.InputStream są niedostępne, ponieważ nie ma została wypełniona przez platformę ASP.NET. Gdy używasz GetBufferedInputStream, otrzymasz kopię strumienia z żądania. Request.Form i Request.InputStream będą nadal dostępne w dalszej części żądania, ponieważ ASP.NET wypełnia drugi egzemplarz.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect i Response.End

Zalecenie: Należy pamiętać o różnice w sposób obsługi wątków po wywołaniu [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metoda wywołuje metodę Response.End. W procesie synchronicznym wywołanie Request.Redirect powoduje, że bieżący wątek natychmiastowe przerwanie. Jednak w procesie asynchronicznym, wywołanie Response.Redirect nie przerwać bieżącego wątku, więc kontynuuje wykonywanie kodu dla żądania. W procesie asynchronicznym musi zwracać zadanie z metody, aby zatrzymać wykonywanie kodu.

W projekcie MVC nie powinien wywoływać Response.Redirect. Zamiast tego zwracają RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState i ViewStateMode

Zalecenie: ViewStateMode Użyj zamiast EnableViewState, aby zapewnić kontrolę nad tym, którzy kontrolki używać stan widoku.

Gdy EnableViewState jest ustawiona na wartość false w dyrektywie Page, stan widoku jest wyłączona dla wszystkich kontrolek w obrębie strony i nie można włączyć. Jeśli chcesz włączyć tylko niektóre formanty na stronie stanu widoku, ustaw ViewStateMode wyłączone dla strony.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Następnie ustaw ViewStateMode włączone na tylko formanty, które jest potrzebna stan widoku.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Włączając stan widoku dla formantów, które go potrzebują, można zmniejszyć rozmiar danych stanu widoku dla stron sieci web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Zalecenie: Korzystanie z dostawców uniwersalnych.

W bieżącym szablony projektu została zastąpiona SqlMembershipProvider [dostawców uniwersalnych ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), która jest dostępna jako pakiet NuGet. Jeśli używasz SqlMembershipProvider w projekcie, który został zbudowany przy użyciu starszej wersji szablony, powinien Przełącz się do dostawców uniwersalnych. Dostawców uniwersalnych współpracować z wszystkich baz danych, które są obsługiwane przez program Entity Framework.

Aby uzyskać więcej informacji, zobacz [wprowadzenie do dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Długotrwałych żądań (> 110 w sekundach)

Zalecenie: Użyj [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) lub [SignalR](../../../signalr/index.md) dla połączonych klientów i użyj asynchroniczne operacje We/Wy.

Długotrwałych żądań może spowodować nieprzewidywalne skutki i niską wydajnością w aplikacji sieci web. Domyślne ustawienie limitu czasu żądania wynosi 110 sekund. Jeśli używasz stanu sesji przy użyciu żądania długotrwałych, ASP.NET zwolni blokadę obiektu sesji po 110 sekundach. Jednak aplikacja może być w trakcie wykonywania operacji na obiekcie sesji, blokada jest zwalniana, gdy operacja może zakończyć się niepowodzeniem. Jeśli drugie żądanie od użytkownika jest zablokowane podczas pierwszego żądania, drugie żądanie mogą uzyskiwać dostęp do obiektu Session w niespójnym stanie.

Jeśli aplikacja zawiera operacje We/Wy blokowania (lub synchronicznego), aplikacja będzie odpowiadać.

Aby zwiększyć wydajność, należy użyć operacji asynchronicznych operacji We/Wy w .NET Framework. Ponadto na użytek funkcji WebSockets lub SignalR klientów nawiązujących połączenie z serwerem. Te funkcje są przeznaczone do efektywnej obsługi długotrwałych żądań.
