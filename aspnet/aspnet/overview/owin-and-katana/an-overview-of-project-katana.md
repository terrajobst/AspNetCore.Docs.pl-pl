---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Omówienie projektu Katana | Dokumentacja firmy Microsoft
author: howarddierking
description: Środowiska ASP.NET Framework została wokół ponad dziesięć lat, a platformę włączył rozwoju niezliczonych witryn sieci Web i usług. Jako aplikacji sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 212594b503bfc17d3faa6df03e01402d2862522f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390191"
---
<a name="an-overview-of-project-katana"></a>Omówienie projektu Katana
====================
przez [Howard Dierking](https://github.com/howarddierking)

> Środowiska ASP.NET Framework została wokół ponad dziesięć lat, a platformę włączył rozwoju niezliczonych witryn sieci Web i usług. Jak usprawniły strategie programowania aplikacji sieci Web, struktura została może podlegać ewolucji w kroku przy użyciu technologii, takich jak ASP.NET MVC i Web API platformy ASP.NET. Jak opracowywanie aplikacji sieci Web ma ewolucyjny następnego kroku w świecie chmury obliczeniowej, projekt [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) zawiera podstawowy zestaw składników do aplikacji ASP.NET, co pozwala na elastyczne, przenośne, lekkie i zapewnić lepszą wydajność — innymi słowy, projektów [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) chmury optymalizuje aplikacji ASP.NET.


## <a name="why-katana--why-now"></a>Dlaczego Katana — Dlaczego teraz?

 Niezależnie od tego, czy jest jeden Omawiając framework lub przez użytkownika końcowego produktu dla deweloperów, ważne jest zrozumienie podstawowych zresztą tworzenia produktu — i części, która obejmuje znajomość produktu została utworzona dla. ASP.NET został pierwotnie utworzony za pomocą dwóch klientów na uwadze.   
  
**Pierwszej grupy klientów była programistów classic ASP.** W tym czasie ASP była jednym z podstawowe technologie do tworzenia dynamicznych, opartych na danych witryn sieci Web i aplikacje, łącząc znaczniki i skrypt po stronie serwera. Środowisko uruchomieniowe ASP dostarczony skrypt po stronie serwera za pomocą zestawu obiektów, które wyodrębnione podstawowych aspektów podstawowego protokołu HTTP i serwera sieci Web pod warunkiem dostęp do dodatkowych usług zarządzania stanem folderu takich sesji i aplikacji w pamięci podręcznej itp. Gdy jest to wydajne, klasyczne aplikacje ASP stało się wezwanie do zarządzania jako zwiększył się rozmiar i złożoność. Jest to głównie ze względu na brak struktury w skryptów środowiska, w połączeniu z powielania kodu wynikające z z przeplotem kodu i znaczników. Aby można było, możesz wykorzystać zalety klasyczne środowisko ASP jednocześnie spełnia wymagania, niektóre jej wyzwania, ASP.NET skorzystała z organizacji kodu, dostarczone przez języki zorientowane obiektowo, programu .NET Framework, zachowując przy tym również model programowania po stronie serwera do której klasyczne środowisko ASP deweloperów miał wzrosła przyzwyczajeni.

**Druga grupa klientów docelowych dla platformy ASP.NET został deweloperzy aplikacji biznesowych Windows.** W odróżnieniu od klasycznego ASP deweloperów, którzy są przyzwyczajeni do pisania kodu znaczników HTML i kod można wygenerować więcej kod znaczników HTML, WinForms deweloperów (np. deweloperzy VB6 poprzedzających) zostały przyzwyczajeni do środowiska czasu projektowania, które uwzględnione kanwy i bogatego zestawu użytkownika Formanty interfejsu. Pierwsza wersja ASP.NET — znany także jako "Formularzy sieci Web" udostępniane podobne środowiska czasu projektowania wraz z modelem zdarzenia po stronie serwera dla składników interfejsu użytkownika oraz zestaw funkcji infrastruktury (na przykład ViewState) Utwórz sprawne środowisko deweloperskie między klientem i programowanie po stronie serwera. Formularze sieci Web hid skutecznie charakter bezstanowe sieci Web w ramach modelu stanowych zdarzeń, który został znane deweloperom WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Problemy zgłoszone przez modelu historycznym

**Wynikiem był dojrzała, bogate runtime i model programowania dla deweloperów.** Korzystając z niego bogactwo funkcji dołączone kilka godne uwagi problemy. Po pierwsze, struktura została **monolityczne**, przy użyciu jednostek logicznie różnych funkcji są ściśle powiązane z tego samego zestawu System.Web.dll (na przykład obiekty podstawowe HTTP przy użyciu środowiska formularzy sieci Web). Po drugie, ASP.NET został dołączony jako część większych .NET Framework, co oznaczało, że **okresami pomiędzy wydaniami został rzędu kilku latach.** To wprowadzone trudne dla zmieniającego się wszystkie zmiany w szybko rozwijających się programowania dla sieci Web ASP.NET. Na koniec System.Web.dll, sama zostało połączone kilka różnych sposobów określonych opcji hostingu w sieci Web: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Ewolucyjny kroków: ASP.NET MVC i Web API platformy ASP.NET

I wiele zmian działo przy projektowaniu aplikacji internetowych. Aplikacje sieci Web zostały coraz bardziej opracowywane jako serię małe, skupia się składniki, a nie w dużych środowisk. Liczba składników, a także częstotliwość, z której zostały wydane zwiększał z szybkością coraz szybciej. Stało się jasne, że zachowanie tempie z sieci Web wymaga struktur, które można pobrać mniejsze, oddzielne i bardziej ukierunkowaną zamiast większych i bardziej bogate, w związku z tym **zespół ASP.NET trwało kilka ewolucyjny kroki, aby włączyć platformę ASP.NET jako rodzina podłączane składników sieci Web, a nie pojedynczego framework**.

Jeden z początku zmiany był wzrost popularności dobrze znanych model-view-controller (MVC) wzorca projektowego dzięki struktur projektowania sieci Web, takich jak Ruby on Rails. Ten styl tworzenia aplikacji sieci Web udostępniła Deweloper większą kontrolę nad znacznikami swojej aplikacji, zachowując przy tym nadal rozdzielenie znaczników i logiki biznesowej, którego była jedna początkowej punktów sprzedaży dla platformy ASP.NET. Aby spełnić wymagania tego stylu programowania aplikacji sieci Web, Microsoft miał okazję do samej pozycji lepiej w przyszłości przez **tworzenie platformy ASP.NET MVC poza pasmem** (i nie uwzględniać go w programie .NET Framework). ASP.NET MVC została wydana jako niezależne do pobrania. Zespół inżynierów to zapewniła elastyczność, której do dostarczenia aktualizacji znacznie częściej niż był wcześniej możliwe.

Inną główną shift podczas tworzenia aplikacji sieci Web został shift z dynamicznych, generowane przez serwer strony sieci Web, aby statyczne znaczników początkowej za pomocą dynamicznego części strony wygenerowany na podstawie skryptu po stronie klienta podczas komunikacji **przy użyciu interfejsów API sieci Web zaplecza za pośrednictwem Wysyłanie żądań AJAX**. Tej architektury shift pomogła napędzania rośnie interfejsów API sieci Web i rozwoju struktury ASP.NET Web API. Jak w przypadku platformy ASP.NET MVC w wersji interfejsu API sieci Web platformy ASP.NET pod warunkiem możliwość dalszego jako bardziej modularna struktura rozwój platformy ASP.NET. Zespół inżynierów skorzystała szansy sprzedaży i **wbudowany interfejs API sieci Web platformy ASP.NET, tak, aby miał on żadnych zależności na żadnym z typów framework core znalezionych w System.Web.dll**. Ta opcja włączona dwie rzeczy: najpierw oznaczało to, że interfejs API sieci Web platformy ASP.NET można rozwijać w sposób całkowicie niezależna (i nadal można szybko iterować, ponieważ jest dostarczane za pośrednictwem pakietu NuGet). Po drugie ponieważ nie wystąpiły żadne zależności zewnętrzne System.Web.dll, a w związku z tym, bez zależności w usługach IIS, ASP.NET Web API uwzględnione możliwość uruchamiania w niestandardowego hosta (na przykład aplikacji konsoli, usługa Windows, itp.)

### <a name="the-future-a-nimble-framework"></a>Przyszłość: Elastyczna platforma

Oddzielenie składników framework od siebie nawzajem, a następnie zwalniając je, dla narzędzia NuGet, struktur, można teraz **iteracji bardziej niezależnie i szybciej**. Ponadto zaawansowane funkcje i elastyczność możliwości samodzielnie hostingu internetowego interfejsu API okazały się bardzo atrakcyjne dla deweloperów, którzy chcieli **hosta małe, lekkie** dla swoich usług. Było tak atrakcyjnym, w rzeczywistości oznacza innych struktur chce również tej funkcji i udostępniane nowe wezwanie, że każdego szablonu został uruchomiony w swoim własnym procesie hosta na własny adres podstawowy, a także wymagane do zarządzania (uruchomiona, zatrzymana, itp.) niezależnie od siebie. Nowoczesnych aplikacji sieci Web zazwyczaj obsługuje dostarczanie plików statycznych, generowania strony dynamicznej, interfejs API sieci Web i więcej ostatnio rzeczywistym-jednorazowe/powiadomień wypychanych. Oczekiwano, że każda z tych usług powinien być uruchamiania i zarządzać w sposób niezależny nie po prostu realistyczne.

Co było wymagane był pojedynczego abstrakcji hostingu, będzie włączyć deweloperów do tworzenia aplikacji z wielu różnych składników i platform, a następnie uruchom tę aplikację na hoście pomocnicze.

## <a name="the-open-web-interface-for-net-owin"></a>Interfejsu Open Web dla platformy .NET (OWIN)

 ZAINSPIROWANE korzyści osiągnięte przez [stojak](http://rack.github.io/) we Wspólnocie dopisków fonetycznych kilku członków społeczności platformy .NET ustalono utworzyć abstrakcję między serwerami sieci Web i składników struktury. Dwa cele projektu do pozyskiwania OWIN były był prosty i zajęło możliwych najmniejszą liczbą zależności na inne typy framework. Te dwa cele zapewnić:

- Nowe składniki można łatwiej opracowane i używane.
- Aplikacje można łatwiej przenoszone między hostami i potencjalnie całego operacyjnych platform systemów.

Wynikowy abstrakcji składa się z dwóch podstawowych elementów. Pierwszy jest słownik środowiska. Ta struktura danych jest odpowiedzialny za przechowywanie wszystkich niezbędnych do przetwarzania żądania HTTP i odpowiedzi, a także wszelkie stanie odpowiedniego serwera stanu. Słownik środowiska jest zdefiniowana w następujący sposób:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Zgodny z OWIN serwer sieci Web jest odpowiedzialny za wypełnianie słownik środowiska z danymi, takie jak strumienie treści i kolekcje nagłówków żądań HTTP i odpowiedzi. Następnie jest odpowiedzialny za składników aplikacji lub framework, aby wypełnić lub zaktualizuj słownik za pomocą dodatkowych wartości i zapisać w strumieniu treści odpowiedzi.

Oprócz określenia typu słownik środowiska, specyfikacja OWIN definiuje listę podstawowych słownik par kluczy i wartości. Na przykład w poniższej tabeli przedstawiono klucze słownikowe wymagane dla żądania HTTP:

| Nazwa klucza | Wartość Opis |
| --- | --- |
| `"owin.RequestBody"` | Stream z treści żądania, jeśli istnieje. Stream.Null może służyć jako symbol zastępczy, jeśli nie treści żądania. Zobacz [treść żądania](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` Nagłówków żądania. Zobacz [nagłówki](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` zawierający metoda żądania HTTP żądania (np. `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | A `string` zawierającą ścieżkę żądania. Ścieżka musi być określona względem "root" delegata aplikacji; zobacz [ścieżki](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` zawierająca część ścieżki żądania odpowiadający "root" delegata aplikacji; zobacz [ścieżki](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` zawierającą nazwę protokołu i wersji (np. `"HTTP/1.0"` lub `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | A `string` zawierająca składnik ciągu zapytania żądania HTTP identyfikatora URI, znaczeniem bez "?" (np. `"foo=bar&baz=quux"`). Wartość może być ciągiem pustym. |
| `"owin.RequestScheme"` | A `string` zawierającego schemat identyfikatora URI użyta dla żądania (np. `"http"`, `"https"`); zobacz [schemat identyfikatora URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Drugi element klucza OWIN jest delegat aplikacji. Jest to sygnatura funkcji, która służy jako podstawowy interfejs między wszystkimi składnikami w aplikacji OWIN. Definicja delegata aplikacji jest następujący:

`Func<IDictionary<string, object>, Task>;`

Delegat aplikacji jest następnie po prostu implementację typu delegata Func, gdzie funkcja akceptuje słownik środowiska jako dane wejściowe i zwraca zadanie. Ten projekt ma kilka skutki dla deweloperów:

- Istnieje bardzo mała liczba zależności dotyczące typu wymagane w celu zapisu składniki OWIN. To znacznie zwiększa dostępność OWIN dla deweloperów.
- Asynchroniczne projektu umożliwia abstrakcji to efektywne z jego obsługa zasobów obliczeniowych, szczególnie w kolejnych operacjach intensywnie korzystających z operacji We/Wy.
- Ponieważ delegata aplikacji jest pojedynczej Atomowej jednostki wykonywania, a ponieważ słownik środowiska jest przenoszone jako parametr w delegacie, składniki OWIN można łatwo łączyć ze sobą, aby utworzyć złożone HTTP potoków przetwarzania.

Z perspektywy wdrożenia OWIN jest specyfikacją ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Jego celem jest nie należy go dalej struktury sieci Web, ale raczej specyfikacja sposobu interakcji struktury sieci Web i serwery sieci Web.

Jeśli po zbadaniu [OWIN](http://owin.org/) lub [Katana](https://github.com/aspnet/AspNetKatana/wiki), należy również zauważyć [pakietu Owin NuGet](http://nuget.org/packages/Owin) i Owin.dll. Ta biblioteka zawiera jeden interfejs [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), który rozmieszczony i kodyfikuje sekwencja uruchamiania opisanego w [sekcji 4](http://owin.org/html/owin.html#4-application-startup) specyfikacji OWIN. Chociaż nie jest wymagane do kompilowania serwerów OWIN [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interfejs zapewnia punkt odniesienia konkretnych i jest on używany przez Katana składników projektów.

## <a name="project-katana"></a>Projektu Katana

Natomiast zarówno [OWIN](http://owin.org/html/owin.html) specyfikacji i *Owin.dll* należące do społeczności i społeczności, uruchom wysiłków "open source", [Katana](https://github.com/aspnet/AspNetKatana/wiki) projektu reprezentuje zestaw OWIN składniki, które podczas nadal open source są wbudowane i wydane przez firmę Microsoft. Te składniki obejmują zarówno składników infrastruktury, takich jak hostów i serwerów, jak i funkcjonalności składników, takich jak składniki uwierzytelniania i powiązania z platform takich jak [SignalR](../../../signalr/index.md) i [sieci Web platformy ASP.NET Interfejs API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Projekt zawiera następujące trzy cele wysokiego poziomu: 

- **Przenośne** — składniki powinny mieć możliwość łatwego zamieniony na nowe składniki po ich udostępnieniu. Dotyczy to wszystkich typów składników platformy do serwera i hosta. Implikacje ten cel jest, czy innej struktury bezproblemowo można uruchomić na serwerach Microsoft, natomiast struktur firmy Microsoft mogą potencjalnie uruchamiane na hostach i serwerów stron trzecich.
- **Modułowy/elastyczne**— w przeciwieństwie do wielu struktur, które obejmują wiele funkcji, które są domyślnie włączone, Katana składników projektu powinna być niewielkiego i skupionego projektu, dając kontrolę do deweloperów aplikacji w określeniu, które składniki, aby Używanie w swojej aplikacji.
- **Uproszczona i wydajne/skalowalnych** — poprzez rozbijanie tradycyjnych pojęcie struktury na zestaw małych, skupia się składniki, które są dodawane jawnie przez dewelopera aplikacji powstałą aplikację Katana może zużywać mniej przetwarzania danych zasoby, a w rezultacie obsługiwać większe obciążenie niż przy użyciu innych typów serwerów i struktur. Jak wymagań aplikacji wymagają większej liczby funkcji z podstawowej infrastruktury, te mogą być dodawane do potoku OWIN, ale powinny zostać wyraźnej decyzji przez dewelopera aplikacji. Ponadto zastąpienia niższym poziomie składników oznacza, że po ich udostępnieniu nowych serwerów o wysokiej wydajności można bezproblemowo można wprowadzone, aby poprawić wydajność aplikacji OWIN bez przerywania tych aplikacji.

## <a name="getting-started-with-katana-components"></a>Wprowadzenie do biblioteki Katana składników

Po raz pierwszy wprowadzona, jeden z aspektów [Node.js](http://nodejs.org/) umożliwiająca natychmiast narysowane uwagi osób został prostotę, za pomocą którego można jeden tworzenie i uruchamianie serwera sieci Web. Jeśli cele Katana zostały sformułowane w świetle właściwości [Node.js](http://nodejs.org/), jeden może je Sumuj według informujący o tym, czy Katana oferuje wiele korzyści zapewnianych przez [Node.js](http://nodejs.org/) (i środowisk, takich jak go) bez wymuszania dla deweloperów, aby zgłosić wszystko, czego wie o projektowaniu aplikacji sieci Web ASP.NET. Dla tej instrukcji pomieścić true, wprowadzenie do projektu Katana powinien być równie prosty w charakterze [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Tworzenie "Hello World!"

Jeden znacząca różnica między scalaniem rozwoju języka JavaScript i platformy .NET jest obecność (lub brak) kompilatora. W efekcie punkt początkowy dla prostego serwera Katana jest projekt programu Visual Studio. Jednak firma Microsoft można uruchomić za pomocą najbardziej minimalny typach projektów: pusta aplikacja sieci Web platformy ASP.NET.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Następnie zostanie zainstalowany [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) pakiet NuGet do projektu. Ten pakiet zawiera serwer OWIN, który jest uruchamiany w Potok żądań ASP.NET. Znajduje się na [galerii pakietów NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) i można je zainstalować przy użyciu konsoli Menedżera pakietów lub okno Menedżera pakietów programu Visual Studio za pomocą następującego polecenia:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalowanie `Microsoft.Owin.Host.SystemWeb` pakiet zainstaluje kilka dodatkowych pakietów jako zależności. Jedną z tych zależności jest `Microsoft.Owin`, bibliotekę, która zawiera kilka pomocnicze typy i metody do tworzenia aplikacji OWIN. Możemy użyć tych typów, można szybko zapisać następującego serwera "hello world".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

To bardzo prosty serwer sieci Web, mogą być uruchamiane przy użyciu programu Visual Studio **F5** polecenia i zapewnia pełną obsługę debugowania.

## <a name="switching-hosts"></a>Przełączanie hostów

Domyślnie w poprzednim przykładzie "hello world" jest uruchamiane w potoku żądania programu ASP.NET używa System.Web w kontekście usług IIS. To samodzielnie dodać ogromne korzyści, ponieważ dzięki temu możemy korzystać z elastyczności i możliwości tworzenia potoku OWIN za pomocą funkcji zarządzania i ogólną dojrzałości usług IIS. Jednak może być przypadkach korzyści udostępniane przez usługi IIS nie są wymagane i pragnienie jest mniejsze, uproszczone hosta. Co jest potrzebne, następnie, aby uruchomić nasz prosty serwer sieci Web poza usług IIS i System.Web?

Aby zilustrować celem przenośności, przejście z hosta serwera sieci Web do hosta wiersza polecenia wymaga po prostu dodając nowe zależności serwera i hosta do folderu wyjściowego projektu, a następnie uruchomić hosta. W tym przykładzie będziemy hostować naszych serwera sieci Web, na hoście Katana o nazwie `OwinHost.exe` i będzie korzystać z serwera na podstawie Katana HttpListener. Podobnie do innych składników Katana te będzie można uzyskać z pakietu NuGet, używając następującego polecenia:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

W wierszu polecenia, firma Microsoft następnie przejdź do folderu głównego projektu i po prostu uruchomić `OwinHost.exe` (który został zainstalowany w folderze narzędzi jego odpowiedniego pakietu NuGet). Domyślnie `OwinHost.exe` jest skonfigurowany do wyszukiwania serwera opartego na HttpListener i dlatego dodatkowa konfiguracja nie jest potrzebna. Przechodząc w przeglądarce sieci Web w taki sposób, aby `http://localhost:5000/` przedstawiono aplikację teraz uruchomione za pośrednictwem konsoli.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architektura Katana

 Architektura składników Katana dzieli aplikację na cztery warstwy logiczne, jak przedstawiono poniżej: *hosta, server, oprogramowanie pośredniczące,* i *aplikacji*. Architektura składników jest uwzględniona w taki sposób, że implementacji tych warstw można łatwo zastąpić, w wielu przypadkach, bez konieczności ponownej kompilacji aplikacji.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 Host jest odpowiedzialny za:

- Zarządzanie bazowego procesu.
- Organizowanie przepływów pracy, które powoduje wybór serwera oraz do budowy potoku OWIN za pośrednictwem żądań, które będą obsługiwane.

  Obecnie istnieją 3 podstawowe opcje hostingu aplikacji opartych na Katana:  
  
**IIS/ASP.NET**: przy użyciu standardowych typów HttpModule i HttpHandler, potoki OWIN można uruchomić w usługach IIS jako część przepływu żądania programu ASP.NET. Obsługa hostingu platformy ASP.NET jest włączona, instalując pakiet Microsoft.AspNet.Host.SystemWeb NuGet do projektu aplikacji sieci Web. Ponadto ponieważ programu IIS działa jako zarówno hosta, jak i serwera, w tym pakiecie NuGet, co oznacza, że przy użyciu SystemWeb host, Projektant nie może zastąpić implementację alternatywny serwer jest conflated rozróżnienie hosta/serwera OWIN.  
  
**Niestandardowy Host**: Katana składników pakietu umożliwia dewelopera do hostowania aplikacji w jej własny niestandardowy proces, czy to aplikacja konsoli, usługa Windows itp. Ta funkcja jest podobny do samodzielnego hostowania możliwości udostępniane przez interfejs API sieci Web. Poniższy przykład przedstawia niestandardowego hosta kodu internetowego interfejsu API:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Konfiguracja hosta samodzielnego aplikacji Katana jest podobne:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Jeden znacząca różnica między scalaniem interfejsu API sieci Web i Katana przykłady hosta samodzielnego jest brak przykład hosta samodzielnego Katana kodu konfiguracji interfejsu API sieci Web. Aby włączyć mobilność i możliwości tworzenia, Katana oddziela kod, który uruchamia serwer z kodu, który konfiguruje potok przetwarzania żądania. Kod, który umożliwia skonfigurowanie interfejsu API sieci Web, a następnie jest zawarty w klasie startu, a ponadto jest określony jako parametr typu w WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Klasa początkowa zostanie omówiona bardziej szczegółowo w dalszej części tego artykułu. Jednak kod wymaganą do uruchomienia Katana, proces hosta samodzielnego przypomina właśnie kod, który być może korzystasz już dziś w aplikacjach hosta samodzielnego ASP.NET Web API.

**OwinHost.exe**: gdy niektóre spowoduje chcesz napisać niestandardowy proces do uruchamiania aplikacji sieci Katana Web, wiele woleliby po prostu uruchomić plik wykonywalny, wstępnie skompilowane, który można uruchomić serwera i uruchamianie ich aplikacji. W przypadku tego scenariusza zawiera zestaw komponentów Katana `OwinHost.exe`. Gdy uruchamiać z poziomu katalogu głównego projektu, tego pliku wykonywalnego uruchomić serwer (HttpListener serwer używany domyślnie) i używać konwencji znajdowania i uruchamiania klasy Autostart użytkownika. Aby uzyskać większą kontrolę plik wykonywalny oferuje pewną liczbę parametrów wiersza polecenia.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Serwer

 Gdy host jest odpowiedzialny za uruchamianie i utrzymywanie proces, w którym jest uruchomiona aplikacja, odpowiedzialność za serwer jest otworzyć gniazda sieci, nasłuchiwanie żądań oraz wysyłać je za pośrednictwem potoku OWIN składniki określony przez użytkownika (jak może być już zauważyć, że ten potok jest określona w klasie uruchamiania Deweloper aplikacji). Obecnie projektu Katana obejmuje dwie implementacje serwera: 

- **Microsoft.Owin.Host.SystemWeb**: jak wcześniej wspomniano, usług IIS w połączeniu z potoku platformy ASP.NET działa jako host i serwer. W związku z tym wybierając to opcja hostingu, usługi IIS zarządza uwagi na poziomie hosta, takie jak proces aktywacji i nasłuchuje żądań HTTP. Dla aplikacji sieci Web platformy ASP.NET następnie wysyła żądania do potoku ASP.NET. Host Katana SystemWeb rejestruje ASP.NET HttpModule i HttpHandler Przechwytywanie żądań, jak długo będą przepływać za pośrednictwem potoku HTTP i wysyłać je za pośrednictwem potoku OWIN określonych przez użytkownika.
- **Microsoft.Owin.Host.HttpListener**: jak jego nazwa wskazuje, ten serwer Katana używa klasy HttpListener .NET Framework, otworzyć gniazda do wysyłania żądań do potoku OWIN określony dla deweloperów. Ta funkcja jest obecnie domyślny wybór serwera dla hosta samodzielnego Katana w interfejsu API i OwinHost.exe.

## <a name="middlewareframework"></a>Oprogramowanie pośredniczące/platformy

 Jak wspomniano wcześniej gdy serwer zaakceptuje żądanie od klienta, jest odpowiedzialny za przekazanie jej za pośrednictwem potoku OWIN składników, które są określone przez kod startowy dla deweloperów. Te składniki potoku są nazywane oprogramowania pośredniczącego.  
 Na poziomie wykraczającego poza podstawowe składnik oprogramowania pośredniczącego OWIN po prostu musi zaimplementować delegata aplikacji OWIN, tak aby był możliwy do wywołania.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Jednak aby uprościć rozwój i kompozycji składników oprogramowania pośredniczącego, Katana obsługuje aspekty konwencje i typy pomocnika dla składników oprogramowania pośredniczącego. Najczęściej z nich jest `OwinMiddleware` klasy. Składnik oprogramowania pośredniczącego niestandardowe utworzone przy użyciu tej klasy będzie wyglądać podobnie do poniższej: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Ta klasa jest pochodną `OwinMiddleware`, implementuje konstruktora, który akceptuje wystąpienie następne oprogramowanie pośredniczące w potoku jako jeden z argumentów, a następnie przekazuje go do konstruktora podstawowego. Dodatkowe argumenty używane do konfiguracji oprogramowania pośredniczącego są również deklarowane jako parametry konstruktora, po następnym parametr oprogramowania pośredniczącego.   
  
W czasie wykonywania, oprogramowanie pośredniczące jest wykonywane za pośrednictwem zastąpione `Invoke` metody. Ta metoda przyjmuje pojedynczy argument typu `OwinContext`. Ten obiekt kontekstu jest dostarczany przez `Microsoft.Owin` pakietu NuGet opisany wcześniej i udostępnia silnie typizowane słownika żądania, odpowiedzi i środowisku, wraz z kilku typów dodatkowej pomocy.   
  
Klasa oprogramowania pośredniczącego mogą być łatwo dodawane do potoku OWIN w kodzie uruchamiania aplikacji w następujący sposób:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Ponieważ infrastruktura Katana po prostu tworzy potok składników oprogramowania pośredniczącego OWIN i składniki po prostu musi obsługiwać delegata aplikacji do wzięcia udziału w potoku, składników oprogramowania pośredniczącego można dostosować w zakresie rozwiązania od prostego rejestratory do całego środowiska, takie jak ASP.NET, w przypadku interfejsu API sieci Web lub [SignalR](../../../signalr/index.md). Dodawanie interfejsu API sieci Web platformy ASP.NET do potoku OWIN poprzedniego wymaga na przykład, dodając następujący kod startowy:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Infrastruktura Katana utworzy potoku składników oprogramowania pośredniczącego w kolejności, w jakiej zostały dodane do obiektu elementu IAppBuilder tej metody konfiguracji. Następnie, w tym przykładzie LoggerMiddleware może obsłużyć wszystkie żądania, które będą działać przez potok, niezależnie od tego, jak ostatecznie te żądania są obsługiwane. To pozwala na zaawansowane scenariusze, w którym składnik oprogramowania pośredniczącego (np. Składnik uwierzytelniania) może przetwarzać żądania dla potoku, który zawiera wiele składników i struktury (np. interfejsu API sieci Web platformy ASP.NET SignalR i serwera plików statycznych).
 
## <a name="applications"></a>Aplikacje

Zgodnie z opisami w poprzednich przykładach, OWIN i Katana projektu powinna nie można traktować jako nowy model programowania aplikacji, ale raczej jako abstrakcję oddzielenie modeli programowania aplikacji i platform z serwera i infrastruktury hostingu. Na przykład podczas tworzenia aplikacji interfejsu API sieci Web, developer framework będzie używać interfejsu API sieci Web platformy ASP.NET framework, niezależnie od tego, czy aplikacja będzie działać w potoku OWIN za pomocą składników z projektu Katana. Jednym miejscu, gdzie kodu związanego z OWIN będą widoczne dla deweloperów aplikacji będzie kodu uruchamiania aplikacji, których Deweloper Redaguj potoku OWIN. Kod startowy dewelopera zarejestruje serię instrukcji UseXx, zazwyczaj jedną dla poszczególnych składników oprogramowania pośredniczącego, który będzie przetwarzał żądania przychodzące. To środowisko będzie mieć taki sam skutek jak rejestrowanie modułów HTTP w bieżącym środowisku System.Web. Zazwyczaj większych framework oprogramowanie pośredniczące, takich jak Web API platformy ASP.NET lub [SignalR](../../../signalr/index.md) zostanie zarejestrowany na końcu potoku. Kompleksowych składników oprogramowania pośredniczącego, takie jak te dla uwierzytelniania lub buforowania, zazwyczaj są rejestrowane w kierunku początku potoku tak, aby ich będzie przetwarzać żądania dla wszystkich platform i składniki zarejestrowane później w potoku. Rozdzielanie składników oprogramowania pośredniczącego z sobą i z podstawowych składników infrastruktury włącza składniki podlegać ewolucji w różnych ilościach, przy jednoczesnym zapewnieniu, że cały system jest trwała.

## <a name="components--nuget-packages"></a>Składniki — pakietów NuGet

Podobnie jak wiele bieżących bibliotek i struktur Katana składników projektów są dostarczane jako zestaw pakietów NuGet. Mającą się ukazać wersję 2.0 wykres zależności pakietu Katana wygląda następująco. (Kliknij obraz większym widoku).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Prawie każdy pakiet w projekcie Katana zależy, bezpośrednio lub pośrednio, pakietów Owin. Być może pamiętasz, czy to jest pakiet, który zawiera interfejs IAppBuilder, który udostępnia konkretną implementację sekwencja uruchamiania aplikacji, które są opisane w sekcji 4 specyfikacji OWIN. Ponadto wiele pakietów zależą od Microsoft.Owin, który zawiera zestaw typów pomocnika do pracy z żądania i odpowiedzi HTTP. W pozostałej części pakietu mogą być klasyfikowane jako hostingu pakietów infrastruktury (serwerów lub hostów) lub oprogramowania pośredniczącego. Pakiety i zależności, które są zewnętrzne w stosunku do projektu Katana są wyświetlane w kolorze pomarańczowym.

Infrastruktury hostingu dla wersji biblioteki Katana 2.0 obejmuje zarówno SystemWeb i serwerami z systemem HttpListener, pakiet OwinHost do uruchamiania aplikacji OWIN za pomocą OwinHost.exe i pakiet Microsoft.Owin.Hosting dla hostingu samodzielnego aplikacji OWIN w niestandardowy host (np. aplikację konsolową, usługa Windows itp.)

2.0 biblioteki Katana składników oprogramowania pośredniczącego przede wszystkim koncentrują się na zapewnieniu inną metodę uwierzytelniania. Jeden składnik dodatkowe oprogramowanie pośredniczące dla diagnostyki zostanie podany, która umożliwia obsługę rozpoczęcia i błędu strony. Wraz z rozwojem w dziedzinie abstrakcji hostingu OWIN ekosystem składników oprogramowania pośredniczącego, oba te opracowany przez firmę Microsoft i innych firm również rośnie liczba.

## <a name="conclusion"></a>Wniosek

 Od jego początku celem projektu Katana nie był do tworzenia i tym samym wymusić deweloperów, aby dowiedzieć się kolejny struktury sieci Web. Zamiast celem została utworzyć abstrakcję, aby zaoferować deweloperom aplikacji sieci Web platformy .NET większy wybór, niż było wcześniej możliwe. Przez podzielenie warstwy logiczne typowe stosu aplikacji sieci Web na zestaw składników wymienne, projektu Katana umożliwia składników w całym stosie, aby poprawić w według stawki ma sens dla tych składników. Tworząc wszystkie składniki wokół proste abstrakcji OWIN, Katana umożliwia platformy i aplikacji korzystających z nich będzie działał na wielu różnych serwery i hosty. Umieszczając Deweloper w formancie stosu, Katana gwarantuje, że deweloper sprawia, że wybór ultimate o jak uproszczone lub jak bogate powinny być jej stosu sieci Web.  
  

## <a name="for-more-information-about-katana"></a>Aby uzyskać więcej informacji na temat Katana

- Projektu Katana w usłudze GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Wideo: [projektu Katana — OWIN dla platformy ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), przez Howard Dierking.

## <a name="acknowledgements"></a>Potwierdzanie

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick jest starszym pisarzem programowania dla firmy Microsoft, koncentrując się na platformie Azure i MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jan Galloway'em](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
