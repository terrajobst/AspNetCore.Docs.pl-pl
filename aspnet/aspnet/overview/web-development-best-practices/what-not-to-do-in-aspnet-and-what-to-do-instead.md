---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Co nie zrobić w programie ASP.NET i co należy zrobić w zamian | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym temacie opisano kilka typowych pomyłek przez osoby w ramach projektów sieci web ASP.NET. Zapewnia zalecenia dotyczące co należy zrobić, aby uniknąć tych commo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 24c6a35a6b663ebb0f8d0e3e7988322fa5d9018c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Co nie zrobić w programie ASP.NET i co zrobić, zamiast niego
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie opisano kilka typowych pomyłek przez osoby w ramach projektów sieci web ASP.NET. Zapewnia zalecenia dotyczące co należy zrobić, aby uniknąć tych typowych pomyłek. Jest on oparty na [prezentacji](http://vimeo.com/68390507) przez **Dyszkiewicz Damianowi** na norweski konferencji deweloperów.


## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

W tym temacie nie ma służyć jako kompletny przewodnik po aby upewnić się, że aplikacja jest bezpieczny i skuteczny. Nadal należy stosować najlepsze rozwiązania dotyczące zabezpieczeń i wydajności, które nie zostały opisane w tym temacie. Go tylko sugeruje, jak można uniknąć typowych błędów związanych z klasy .NET i procesów.

## <a name="overview"></a>Omówienie

Ten temat zawiera następujące sekcje:

- [Zgodność ze standardami](#standards)

    - [Formantu karty](#adapters)
    - [Właściwości stylu dla formantów](#styleprop)
    - [Strony i kontrolki wywołań zwrotnych](#callback)
    - [Wykrywania możliwości przeglądarki](#browsercap)
- [Zabezpieczeń](#security)

    - [Sprawdzanie poprawności żądań](#validation)
    - [Uwierzytelnianie formularzy bez plików cookie i sesji](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Średnia zaufania](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Niezawodność i wydajność](#performance)

    - [PreSendRequestHeaders i PreSendRequestContext](#presend)
    - [Zdarzenia asynchroniczne strony formularzy sieci Web](#asyncevents)
    - [Fire i zapomnij pracy](#fire)
    - [Treści jednostki żądania](#requestentity)
    - [Response.Redirect i Response.End](#redirect)
    - [EnableViewState i ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Długie żądania uruchamiania (> 110 w sekundach)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Zgodność ze standardami

<a id="adapters"></a>

### <a name="control-adapters"></a>Formantu karty

Zalecenie: Zatrzymaj przy użyciu kart kontrolki do renderowania adaptacyjną, a zamiast tego użyć zapytaniami multimediów CSS i zgodny ze standardami HTML.

Formanty karty wprowadzono w programie .NET 2.0 do renderowania kodu prezentacji, który został dostosowany do różnych urządzeń i środowiska. Teraz ten adaptacyjną renderowania można osiągnąć HTML i CSS. Należy zatrzymać za pomocą formantu karty i przekonwertować żadnych istniejących kart CSS i HTML.

Aby uzyskać więcej informacji, zobacz [zapytaniami multimediów](http://www.w3.org/TR/css3-mediaqueries/) i [jak: Dodaj strony Mobile Your formularzy sieci Web ASP.NET / aplikacji MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Właściwości stylu dla formantów

Zatrzymaj zalecenie: Ustawianie stylów wartości w znaczniku kontroli, a zamiast tego ustawienia formatowania wartości w arkusze stylów CSS.

Formanty serwera sieci Web zawiera dziesiątki właściwości, które mogą być używane do ustawiania właściwości stylu w tekście. Na przykład właściwości ForeColor Ustawia kolor tekstu dla formantu. Można osiągnąć ten sam efekt wydajniej za pośrednictwem arkusze stylów CSS. Arkusze stylów umożliwiają scentralizowanie wartości stylu i unikaj ustawiania tych wartości w całej aplikacji.

W poniższym przykładzie przedstawiono klasę CSS ustawia tekst na czerwony.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Kolejnym przykładzie pokazano, jak dynamicznie zastosować klasę CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Strony i kontrolki wywołań zwrotnych

Zalecenie: Zatrzymaj przy użyciu wywołania zwrotne strony i kontrolki, a zamiast tego użyć dowolnego z następujących: AJAX, element UpdatePanel, metod akcji MVC, interfejsu API sieci Web lub SignalR.

We wcześniejszych wersjach programu ASP.NET metody wywołania zwrotnego strony i kontrolki włączone zaktualizować część strony sieci web bez odświeżania całej strony. Można teraz wykonać aktualizacje stron częściowych za pośrednictwem [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [interfejsu API sieci Web](../../../web-api/index.md) lub [SignalR](../../../signalr/index.md). Należy zatrzymać za pomocą metody wywołania zwrotnego, ponieważ mogą one powodować problemy przyjazne adresy URL i routing. Domyślnie przez formanty nie należy włączać metody wywołania zwrotnego, ale włączenie tej funkcji w formancie, należy wyłączyć je.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Wykrywania możliwości przeglądarki

Zalecenie: Zatrzymaj przy użyciu wykrywania możliwości przeglądarki statyczne i zamiast tego użyć funkcji dynamicznego wykrywania.

We wcześniejszych wersjach programu ASP.NET obsługiwane funkcje dla każdej przeglądarki były przechowywane w pliku XML. Wykrywanie obsługi różnych funkcji za pomocą statycznego wyszukiwania nie jest najlepszym rozwiązaniem. Teraz można dynamicznie wykryć można funkcji przeglądarki przy użyciu funkcji wykrywania framework, takich jak [Modernizr](http://modernizr.com/). Funkcja wykrywania określa Obsługa podjęto próbę użycia metody lub właściwości, a następnie sprawdzania, jeśli przeglądarka wyprodukowanych pożądany wynik. Domyślnie Modernizr jest zawarte w szablonach aplikacji sieci Web.

<a id="security"></a>

## <a name="security"></a>Zabezpieczenia

<a id="validation"></a>

### <a name="request-validation"></a>Sprawdzanie poprawności żądań

Zalecenie: Sprawdzanie poprawności danych wejściowych użytkownika, a zakodować dane wyjściowe ze strony użytkowników.

Weryfikacja żądania jest funkcją programu ASP.NET, która sprawdza każde żądanie i zatrzymuje żądania, jeśli zostanie znaleziony potencjalnych zagrożeń. Nie zależą od weryfikacji żądania zabezpieczania aplikacji przed atakami skryptów między witrynami. Należy sprawdzić poprawność wszystkich danych wejściowych od użytkowników i kodowania danych wyjściowych. W ograniczonych przypadkach można użyć wyrażeń regularnych do sprawdzania poprawności danych wejściowych, ale w przypadku bardziej skomplikowanych, które należy sprawdzić, czy dane wejściowe użytkownika przy użyciu klasy .NET, które sprawdza, czy wartość jest zgodna dozwolone wartości.

Poniższy przykład przedstawia użycie metody statycznej klasy identyfikator Uri do ustalenia, czy identyfikator Uri podanego przez użytkownika jest nieprawidłowa.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Jednak aby wystarczająco sprawdzić identyfikator Uri, należy także sprawdzić aby upewnić się, ponieważ określa on `http` lub `https`. W poniższym przykładzie użyto metody wystąpienia, aby sprawdzić poprawność identyfikatora Uri.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Przed dane wejściowe użytkownika w formacie HTML do renderowania lub dane wejściowe użytkownika w tym w zapytaniu SQL, kodowania wartości do upewnij się, że nie jest dołączana złośliwego kodu.

Można HTML Koduj wartość w znaczniku z &lt;%: %&gt; składni, jak pokazano poniżej.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

W składni Razor, można też HTML kodowania z @, jak pokazano poniżej.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

W następnym przykładzie pokazano sposób do formatu HTML kodowania wartości związane z kodem.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Aby bezpiecznie Koduj wartość poleceń SQL, użyj parametrów polecenia takiego jak [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Uwierzytelnianie formularzy bez plików cookie i sesji

Zalecenia: Wymagaj plików cookie.

Przekazywanie informacji o uwierzytelnianiu w ciągu zapytania nie jest bezpieczne. W związku z tym wymagają plików cookie, jeśli aplikacja zawiera uwierzytelniania. Jeśli Twoje pliki cookie są przechowywane poufne informacje, należy rozważyć wymaganie protokołu SSL dla pliku cookie.

Poniższy przykład pokazuje, jak można określić w pliku Web.config, że uwierzytelnianie formularzy wymaga pliku cookie, które są przesyłane za pośrednictwem protokołu SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Zalecenie: Nigdy nie ma wartość false.

Domyślnie EnbableViewStateMac jest ustawiona na true. Nawet jeśli aplikacja nie używa stanu widoku, nie należy ustawiać EnableViewStateMac na wartość false. Ustawienie wartości false spowoduje, że aplikacja podatne na wykonywanie skryptów między witrynami.

Począwszy od platformy ASP.NET 4.5.2, wymusza środowiska uruchomieniowego **EnableViewStateMac = true**. Nawet jeśli zostanie ustawiona na wartość false, środowiska uruchomieniowego ignoruje tę wartość i kontynuuje ustaw wartość true. Aby uzyskać więcej informacji, zobacz [ASP.NET 4.5.2 i EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Poniższy przykład pokazuje, jak ustawić EnableViewStateMac na wartość true. Nie trzeba faktycznie Ustaw tę wartość na true, ponieważ jest on domyślnie true. Jednak jeśli ustawiono go na wartość false na stronach w aplikacji, możesz od razu poprawić tę wartość.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Średnia zaufania

Zalecenie: Nie zależą od zaufania średni (lub na innym poziomie zaufania) funkcję granicy zabezpieczeń.

Częściowej relacji zaufania nie chroni odpowiednio aplikacji i nie powinna być używana. Zamiast tego użyj pełnego zaufania i izolowanie niezaufanych aplikacji w osobnych pulach aplikacji. Ponadto uruchamiania każdego unikatową tożsamość puli aplikacji. Aby uzyskać więcej informacji, zobacz [częściowego zaufania programu ASP.NET nie gwarantuje izolacji aplikacji](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Zalecenie: Nie należy wyłączać ustawienia zabezpieczeń w &lt;appSettings&gt; elementu.

AppSettings element zawierającej wiele wartości, które są wymagane dla aktualizacji zabezpieczeń. Nie należy zmieniać ani wyłączyć te wartości. Jeśli podczas wdrażania aktualizacji, należy wyłączyć te wartości, natychmiast ponownie włączyć po zakończeniu wdrożenia.

Aby uzyskać więcej informacji, zobacz [ASP.NET appSettings elementu](https://msdn.microsoft.com/en-us/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Zalecenie: Użyj [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) zamiast tego.

Metoda UrlPathEncode został dodany do programu .NET Framework, aby rozwiązać problem ze zgodnością bardzo konkretnej przeglądarki. Nie można ją było właściwie przeprowadza kodowania adresu URL, a nie chroni aplikację przed skryptów między witrynami. Należy nigdy używać w aplikacji. Zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).

Poniższy przykład przedstawia sposób przekazywania adresu URL zakodowanym jako parametr ciągu zapytania kontrolki hiperlinku.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Niezawodność i wydajność

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontext"></a>PreSendRequestHeaders i PreSendRequestContext

Zalecenie: Nie należy używać tych zdarzeń z modułów zarządzanych. Zamiast tego należy zapisać moduł macierzysty usług IIS w celu wykonania wymaganych zadań. Zobacz [tworzenie moduły HTTP kodu natywnego](https://msdn.microsoft.com/en-us/library/ms693629.aspx).

Można użyć [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) i [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) zdarzenia z modułami macierzystymi usług IIS, ale nie należy używać z modułów zarządzanych, w których zaimplementowano elementu IHttpModule. Ustawienie tych właściwości mogą powodować problemy z żądań asynchronicznych.

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Zdarzenia asynchroniczne strony formularzy sieci Web

Zalecenie: W formularzach sieci Web unikać pisania async void metody zdarzenia cyklu życia strony, a zamiast tego użyć [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) dla asynchronicznego kodu.

Po zaznaczeniu zdarzeniem strony z **async** i **void**, nie można określić podczas asynchronicznego kodu zostało zakończone. W zamian użyj Page.RegisterAsyncTask, aby uruchomić kod asynchronicznych w taki sposób, który umożliwia śledzenie jego zakończenia.

W poniższym przykładzie pokazano, a przycisk kliknij program obsługi, który zawiera kod asynchroniczny. W tym przykładzie dołączono odczytywania wartości ciągu asynchronicznie, znajdujący się tylko jako uproszczony przykład zadanie asynchroniczne, a nie zalecanym rozwiązaniem.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Jeśli używasz zadania asynchroniczne, należy ustawić platformę docelową środowiska uruchomieniowego Http 4.5 w pliku Web.config. Ustawienia platformy docelowej do 4.5 włącza na nowy kontekst synchronizacji został dodany w programie .NET 4.5. Ta wartość jest ustawiana domyślnie w nowych projektach w programie Visual Studio 2012, ale nie można ustawić podczas pracy z istniejącego projektu.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Fire i zapomnij pracy

Zalecenie: Podczas przetwarzania żądania w programie ASP.NET, należy unikać uruchamiania fire i zapomnij pracy (takie wywołanie metody ThreadPool.QueueUserWorkItem lub tworzenia czasomierza wielokrotnie wywołuje delegata).

Jeśli aplikacja ma fire i zapomnij pracy uruchamiany w ASP.NET, aplikacja może zsynchronizowane. W dowolnym momencie domena aplikacji mogą zostać zniszczone co oznacza, że Twoje ciągły proces mogą nie odpowiadać bieżący stan aplikacji.

Należy przenieść ten typ pracy poza ASP.NET. Można użyć zadania sieci Web, usług systemu Windows lub roli proces roboczy na platformie Azure do wykonywania pracy w toku i uruchomić kod z innego procesu.

Jeśli konieczne jest przeprowadzenie prac w programie ASP.NET, można dodać pakiet Nuget o nazwie [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) do uruchomienia kodu.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Treści jednostki żądania

Zalecenie: Unikaj odczytu Request.Form lub Request.InputStream przed programu obsługi zdarzeń.

Najwcześniejsza którymi należy zapoznać się z kolekcji Request.Form lub Request.InputStream jest podczas obsługi wykonać zdarzeń. W nazwie wzorca MVC program obsługi jest kontrolerem i zdarzenie execute jest uruchomienie metody akcji. W formularzach sieci Web strona jest programem obsługi i zdarzenie execute jest po zdarzeniu Page.Init. Jeśli wcześniej niż zdarzeń execute przeczytanie treści jednostki żądania, zakłócać jest przetwarzania żądania.

Jeśli zachodzi potrzeba odczytania treści jednostki żądania przed zdarzeniem execute, użyj jednej [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) lub [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx). Używasz GetBufferlessInputStream uzyskać raw strumienia z żądania, a na siebie odpowiedzialność za przetwarzanie całego żądania. Po wywołaniu metody GetBufferlessInputStream, Request.Form i Request.InputStream są niedostępne, ponieważ nie zostały one wypełnione przez platformę ASP.NET. Gdy używasz GetBufferedInputStream otrzymasz kopiowania strumienia z żądania. Request.Form i Request.InputStream są nadal dostępne w dalszej części żądania, ponieważ ASP.NET wypełnia pozostałe kopie.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect i Response.End

Zalecenie: Należy pamiętać o różnice w sposób obsługi wątku po wywołaniu [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).

[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) metoda wywołuje metodę Response.End. Podczas synchronicznego wywoływania Request.Redirect powoduje, że bieżący wątek natychmiast przerwania. Jednak w proces asynchroniczny, wywoływanie metody Response.Redirect nie przerwać bieżącego wątku, więc kontynuuje wykonywanie kodu dla żądania. W procesie asynchroniczne musi zwracać zadanie z metody, aby zatrzymać wykonanie kodu.

W projekcie MVC nie powinny wywoływać Response.Redirect. Zamiast tego należy zwracać RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState i ViewStateMode

Zalecenie: ViewStateMode Użyj zamiast EnableViewState, aby zapewnić kontrolę służącym formanty korzystanie ze stanu widoku.

Gdy EnableViewState jest ustawiona na wartość false w dyrektywie Page, stan widoku jest wyłączona dla wszystkich kontrolek na stronie i nie można włączyć. Jeśli chcesz włączyć tylko niektóre formanty na stronie stanu widoku, wartość ViewStateMode wyłączone dla strony.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Następnie ustaw ViewStateMode włączone w formantach faktycznie wymagające stan widoku.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Przez włączenie stanu widoku tylko formanty, które go potrzebują, można zmniejszyć rozmiar stan widoku dla stron sieci web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Zalecenie: Użyj dostawców uniwersalnych.

W bieżącym szablony projektów, została zastąpiona SqlMembershipProvider [dostawców uniwersalnych ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), która jest dostępna jako pakietu NuGet. Jeśli używasz SqlMembershipProvider w projekcie, który został utworzony we wcześniejszej wersji szablonów, należy przełączyć się do dostawców uniwersalnych. Dostawców uniwersalnych współpracować z wszystkich baz danych, które są obsługiwane przez program Entity Framework.

Aby uzyskać więcej informacji, zobacz [wprowadzenie dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Długotrwałe żądania (> 110 w sekundach)

Zalecenie: Użyj [Websocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) lub [SignalR](../../../signalr/index.md) dla połączonych klientów i użyj asynchronicznej operacji We/Wy.

Długotrwałe żądania może spowodować nieprzewidywalne skutki i pogorszenie wydajności w aplikacji sieci web. Domyślne ustawienie limitu czasu dla żądania jest 110 sekund. Jeśli używasz stanu sesji z żądaniem długotrwałe, ASP.NET spowoduje zwolnienie blokady obiektu Session 110 sekund. Jednak aplikacja może znajdować się w środku operację na obiekcie sesji po zwolnieniu blokady, a operacja nie może zakończyć się pomyślnie. Drugie żądanie od użytkownika zostało zablokowane podczas pierwszego żądania, drugie żądanie mogą uzyskiwać dostęp do obiektu Session w niespójnym stanie.

Jeśli aplikacja zawiera blokowania (lub synchroniczne) operacji We/Wy, że aplikacja będzie odpowiadać.

Aby zwiększyć wydajność, należy użyć asynchronicznej operacji We/Wy w programie .NET Framework. Należy także użyć Websocket lub SignalR dla klientów nawiązujących połączenie z serwerem. Te funkcje są przeznaczone do efektywnej obsługi żądań długotrwałe.
