---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: "Przegląd projektu Katana | Dokumentacja firmy Microsoft"
author: howarddierking
description: "Struktury programu ASP.NET została wokół ponad 10 lat, a platformy ma włączoną obsługę programowania niezliczonych witryn sieci Web i usług. Jako aplikacji sieci Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: ceb7d3a7d1cb1685c0f1e62698f508c9a73e77c2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="an-overview-of-project-katana"></a>Przegląd projektu Katana
====================
przez [Howard Dierking](https://github.com/howarddierking)

> Struktury programu ASP.NET została wokół ponad 10 lat, a platformy ma włączoną obsługę programowania niezliczonych witryn sieci Web i usług. Jak usprawnionych strategii rozwoju aplikacji sieci Web, platformę mógł rozwijać w kroku z technologii, takich jak ASP.NET MVC i Web API platformy ASP.NET. Jak projektowanie aplikacji sieci Web uwzględnia następnego kroku ewolucyjny świecie przetwarzania w chmurze, projektu [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) zawiera podstawowy zestaw składników do aplikacji ASP.NET, umożliwiając im elastyczne, przenośne, lekkie i zapewnić lepszą wydajność — innymi słowy, projektu [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) chmury optymalizuje aplikacji ASP.NET.


## <a name="why-katana--why-now"></a>Dlaczego Katana — Dlaczego teraz?

 Niezależnie od tego, czy zawiera jeden Deweloper framework lub przez użytkownika końcowego produktu, ważne jest zrozumienie podstawowej motywacji tworzenia produktu —, który jest częścią obejmuje znajomość produktu został utworzony. ASP.NET został utworzony z dwóch klientów pod uwagę.   
  
**Pierwsza grupa odbiorców został classic ASP deweloperów.** W tym czasie ASP była jednym technologii podstawowy służący do tworzenia dynamicznych, opartych na danych witryn sieci Web i aplikacji przez łącząc znaczniki i skrypt po stronie serwera. Środowisko uruchomieniowe ASP dostarczona skryptu po stronie serwera przy użyciu zestawu obiektów, które pobieranej podstawowych aspektów podstawowy protokół HTTP i serwer sieci Web i pod warunkiem dostęp do dodatkowych usług takich sesji i aplikacji Zarządzanie stanem, pamięci podręcznej itp. Gdy wydajne, klasyczne aplikacje ASP stał się żądanie Zarządzanie zwiększył się rozmiar i złożoność. To przede wszystkim ze względu na brak znaleziono skrypty środowiska, w połączeniu z duplikatów kod wyniku naprzemiennego wykonywania kodu i znaczników struktury. Aby wielką literą sile klasyczne środowisko ASP podczas adresowania niektóre z jego wyzwania, ASP.NET trwało zaletą organizacji kodu pochodzącymi zorientowane obiektowo języków platformy .NET przy zachowaniu również model programowania po stronie serwera do których klasyczne środowisko ASP miał rozszerzony przyzwyczajony deweloperów.

**Drugiej grupy klientów docelowych dla platformy ASP.NET został deweloperzy aplikacji biznesowych systemu Windows.** W odróżnieniu od klasycznego ASP deweloperzy, którzy przyzwyczajony do pisania kodu znaczników HTML i kod, aby wygenerować kod znaczników HTML więcej deweloperzy WinForms (podobnie jak deweloperzy VB6 poprzedzających) są przystosowane do obsługi czasu projektowania, zawierającego obszaru roboczego i bogaty zestaw użytkownika Formanty interfejsu. Pierwszą wersję platformy ASP.NET — znanej także jako "Formularzy sieci Web" podany podobne możliwości czasu projektowania oraz model zdarzeń po stronie serwera dla składników interfejsu użytkownika i zestaw funkcji infrastruktury (na przykład stan wyświetlania) można utworzyć środowisko bezproblemowe developer między klientem a programowanie po stronie serwera. Formularze sieci Web skutecznie hid charakter bezstanowych sieci Web w modelu stanowe zdarzeń, które były znane deweloperom WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Problemy zgłoszone przez modelu historycznym

**Wynikiem był dojrzałe, bogate środowisko uruchomieniowe i model programowania developer.** Z tym funkcja siłę pochodzi kilka istotnych wyzwania. Po pierwsze, została **wbudowanymi**, z jednostkami logicznie różnych funkcji, które są ściśle powiązane w tym samym zestawie System.Web.dll (na przykład obiektów podstawowych HTTP Framework formularzy sieci Web). Po drugie, ASP.NET został uwzględniony w ramach większych .NET Framework, co oznaczało, że **czas między wersjami był rzędu lata.** To wprowadzone trudne dla platformy ASP.NET wszystkie zmiany wykonywane w szybko rozwijających się projektowanie witryn sieci Web. Na koniec System.Web.dll sam zostało połączone kilka różnych sposobów do określonych opcji obsługi sieci Web: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Kroki ewolucyjny: ASP.NET MVC i Web API platformy ASP.NET

I wiele zmian zostało wykonywane w aplikacji sieci Web! Aplikacje sieci Web zostały coraz rozwijane jako serię małych fokus składniki zamiast dużej struktury. Liczba składników, a także częstotliwość, z której zostały wydane wzrastała szybkością coraz szybsze. Jasno, że zachowanie tempie z sieci Web wymaga platformy, aby uzyskać mniejszy, rozdzielonymi i bardziej ukierunkowaną zamiast większych i oferującego więcej funkcji, w związku z tym **zespołu ASP.NET trwało kilka ewolucyjny kroki, aby włączyć platformę ASP.NET, jak to rodzina podłączane składniki sieci Web zamiast pojedynczego framework**.

Wzrost popularności dobrze znanego model-view-controller (MVC) wzorcu dzięki środowiska deweloperskie sieci Web, takich jak Ruby szyny jeden wczesne zmian jest. Ten styl tworzenia aplikacji sieci Web nadać dewelopera większą kontrolę nad znaczników swojej aplikacji nadal zachowaniem separacji znaczników i logiki biznesowej, które jest jednym z początkowej punktów sprzedaży dla platformy ASP.NET. Aby spełnić wymagania tego stylu tworzenia aplikacji sieci Web, Microsoft miał możliwość umieść sam lepsze w przyszłości przez **tworzenie ASP.NET MVC poza pasmem** (i nie zawiera go w programie .NET Framework). ASP.NET MVC został wydany jako niezależne do pobrania. To udzielił zespołu inżynieryjnego elastyczność do dostarczania aktualizacji częściej niż był wcześniej możliwe.

Inny shift głównych w tworzenie aplikacji sieci Web został shift z dynamicznego, generowane przez serwer strony sieci Web do statycznych znacznika początkowego dynamiczne sekcje strony wygenerowany komunikację skryptu po stronie klienta **z zapleczem interfejsów API sieci Web za pomocą Żądania AJAX**. Tej architektury shift pomogła napędzania wzrost interfejsów API sieci Web i rozwoju platformy ASP.NET Web API. Jak w przypadku platformy ASP.NET MVC wersji składnika ASP.NET Web API udostępniane innym możliwość rozwijać dalsze jako bardziej modularna platforma ASP.NET. Zaletą możliwość trwało zespołu inżynieryjnego i **wbudowane interfejsu API sieci Web platformy ASP.NET, tak, aby miał żadnych zależności na żadnym z typów framework core w System.Web.dll**. Ta opcja włączona dwie czynności: najpierw przeznaczone który interfejsu API sieci Web platformy ASP.NET można rozwijać w sposób całkowicie niezależna (i nadal można szybko przejść, ponieważ jest dostarczane za pośrednictwem pakietu NuGet). Po drugie ponieważ nie było żadnych zależności zewnętrzne do System.Web.dll i w związku z tym bez zależności w usługach IIS, ASP.NET Web API uwzględnione możliwości do uruchamiania w niestandardowych hosta (na przykład aplikacji konsoli, usługa systemu Windows, itp.)

### <a name="the-future-a-nimble-framework"></a>Przyszłe: Firma struktury

Oddzielenie składników framework od siebie i udostępnia je na NuGet, struktury, można teraz **iteracyjne więcej niezależnie i szybciej**. Ponadto możliwościach i elastyczności możliwości własnym hostingu API sieci Web potwierdza, że bardzo atrakcyjny dla deweloperów, którzy chciał **hosta małe, lekkie** dla swoich usług. Tak atrakcyjne udowodnić, w rzeczywistości to innych platform, chcę teraz tej możliwości i udostępnić nowe żądanie, w tym każdego framework były uruchamiane w swoim własnym procesie hosta podstawowego adresu i wymagane do zarządzania (uruchomiona, zatrzymana, itp.) niezależnie. Nowoczesnych aplikacji sieci Web obsługuje zazwyczaj dostarczanie plików statycznych, generowania strony dynamicznego interfejsu API sieci Web i bardziej ostatnio real-jednorazowej/powiadomień wypychanych. Oczekiwano, że należy każdy z tych usług uruchom i zarządzane niezależnie nie wystarczy realistyczne.

Co było wymagane było pojedynczego abstrakcji hostingu, czy włączyć deweloperów do tworzenia aplikacji z wielu różnych składników i platform, a następnie uruchom aplikację na hoście obsługi.

## <a name="the-open-web-interface-for-net-owin"></a>Interfejsu Open Web dla platformy .NET (OWIN)

 Inspirowana korzyści osiągane dzięki [stojak](http://rack.github.io/) społeczności dopisków fonetycznych, określone kilku członków społeczności usług .NET do tworzenia abstrakcję między serwerami sieci Web i komponentów środowiska. Dwa cele projektowania dla abstrakcji OWIN zostały był prosty i zajęło najmniejszej możliwych zależności na inne typy framework. Tych dwóch celów zapewnienia:

- Nowe składniki można łatwiej opracowany i używane.
- Aplikacje można łatwiej przenoszone między hostami oraz potencjalnie całego operacyjnych platform systemów.

Wynikowa abstrakcji składa się z dwóch podstawowych elementów. Pierwsza to słownik środowiska. Ta struktura danych jest odpowiedzialny za przechowywania wszystkich niezbędnych do przetwarzania żądań HTTP i odpowiedzi, a także wszelkie stanie odpowiedniego serwera stanu. Słownik środowiska jest zdefiniowane w następujący sposób:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Serwer sieci Web zgodnej OWIN jest odpowiedzialny za wypełnianie słownik środowiska z danymi, takie jak strumienie treści i kolekcje nagłówków HTTP żądania i odpowiedzi. Następnie jest odpowiedzialny za składników aplikacji lub platformy, aby wypełnić lub zaktualizować słownik o dodatkowe wartości i zapisać w strumieniu treści odpowiedzi.

Oprócz określenia typu słownika środowiska, specyfikacja OWIN definiuje listę par wartości klucza słownika core. Na przykład w poniższej tabeli przedstawiono klucze słownika wymagany dla żądania HTTP:

| Nazwa klucza | Opis wartości |
| --- | --- |
| `"owin.RequestBody"` | Strumień treści żądania, jeśli istnieje. Stream.Null może być używany jako symbol zastępczy, jeśli brak treści żądania. Zobacz [treść żądania](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` Nagłówków żądania. Zobacz [nagłówki](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` zawierający metoda żądania HTTP żądania (np. `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | A `string` zawierający ścieżki żądania. Ścieżka musi być określona względem "root" delegata aplikacji; zobacz [ścieżki](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` zawierający część ścieżki żądania odpowiadającej "root" delegata aplikacji; zobacz [ścieżki](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` zawierający nazwę protokołu i wersji (np. `"HTTP/1.0"` lub `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | A `string` zawierającą składnik ciągu zapytania żądania HTTP identyfikatora URI, bez znaku "?" (np. `"foo=bar&baz=quux"`). Wartość może być pustym ciągiem. |
| `"owin.RequestScheme"` | A `string` zawierający schemat identyfikatora URI użytej w żądaniu (np. `"http"`, `"https"`); zobacz [schemat identyfikatora URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Drugi element klucza OWIN jest delegat aplikacji. Jest to sygnatura funkcji, która służy jako podstawowy interfejs między wszystkimi składnikami w aplikacji OWIN. Definicja delegata aplikacji jest następujący:

`Func<IDictionary<string, object>, Task>;`

Delegat aplikacji jest następnie po prostu implementacja typu delegata Func, gdy funkcja akceptuje słownik środowiska jako dane wejściowe i zwraca zadanie. W tym projekcie ma wpływ na kilku dla deweloperów:

- Istnieje bardzo małą liczbę zależności typu wymagane w celu zapisania składniki OWIN. Znacznie zwiększa to dostępność OWIN dla deweloperów.
- Asynchroniczne projektu umożliwia abstrakcji za skuteczny z jego obsługę zasoby przetwarzania danych, szczególnie w kolejnych operacjach intensywne operacje We/Wy.
- Ponieważ delegat aplikacji jest Atomowej jednostki wykonywania, a ponieważ słownik środowiska odbywa się jako parametr w delegacie, składniki OWIN można łatwo połączyć ze sobą w celu utworzenia złożonego HTTP potoków przetwarzania.

Z perspektywy wdrożenia OWIN jest specyfikacją ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Jego celem jest nie można dalej struktura sieci Web, ale raczej Specyfikacja interakcje platformy sieci Web i serwery sieci Web.

Jeśli zostały zbadane [OWIN](http://owin.org/) lub [Katana](https://github.com/aspnet/AspNetKatana/wiki), należy także zauważyć [pakietu Owin NuGet](http://nuget.org/packages/Owin) i Owin.dll. Ta biblioteka zawiera jeden interfejs, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), który rozmieszczony i określa zasady uruchamiania opisanego w [sekcji 4](http://owin.org/html/owin.html#4-application-startup) specyfikacji OWIN. Gdy nie są wymagane do zbudowania serwerów OWIN [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interfejs zapewnia punkt odniesienia konkretnych i jest używany przez składniki Katana projektu.

## <a name="project-katana"></a>Katana projektu

Natomiast zarówno [OWIN](http://owin.org/html/owin.html) specyfikacji i *Owin.dll* należące do społeczności i społeczności Uruchom wysiłków typu open source, [Katana](https://github.com/aspnet/AspNetKatana/wiki) projektu reprezentuje zestaw OWIN składniki, które podczas nadal open source są wbudowane i udostępnionych przez firmę Microsoft. Te składniki zawierają zarówno składników infrastruktury, takich jak hosty i serwerów, jak i funkcjonalności składniki, takie jak składniki uwierzytelniania i powiązania z platform takich jak [SignalR](../../../signalr/index.md) i [sieci Web ASP.NET Interfejs API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Projekt zawiera następujące trzy cele wysokiego poziomu: 

- **Przenośne** — składniki powinny mieć możliwość można łatwo zastępowane dla nowych składników staną się one dostępne. Dotyczy to wszystkich typów składników, w ramach do serwera i hosta. Implikacji ten cel jest, że struktur innej bezproblemowo może działać na serwerach Microsoft podczas struktury Microsoft potencjalnie można uruchamiać na serwerach innych firm i hostów.
- **Moduły/elastyczne**— w odróżnieniu od wielu struktur, które obejmują większą liczbą funkcji, które są domyślnie włączone, składniki projektu Katana powinny być niewielkiego i skupionego projektu, co daje kontrolę dla dewelopera aplikacji w określeniu, które składniki do Użyj w swojej aplikacji.
- **Lightweight/wydajność/skalowalne** — dzięki pozbyciu tradycyjnych pojęcie RAM do zestawu z małych fokus składniki, które są dodawane jawnie przez dewelopera aplikacji wynikowej aplikacji Katana, jaką może wykorzystać mniej obliczeniowych zasoby i w związku z tym obsłużyć obciążenia więcej niż z innych typów serwerów i platform. Zgodnie z wymaganiami aplikacji wymaga więcej funkcji z podstawowej infrastruktury, te mogą być dodawane do potoku OWIN, ale powinny zostać jawne decyzji przez dewelopera aplikacji. Ponadto zastąpienia niższym poziomie składników oznacza, że staną się one dostępne, nowe serwery wysokiej wydajności można bezproblemowo można mające na celu poprawę wydajności aplikacji OWIN bez przerywania tych aplikacji.

## <a name="getting-started-with-katana-components"></a>Wprowadzenie do składników Katana

Gdy najpierw została wprowadzona, jednym aspekcie [Node.js](http://nodejs.org/) framework natychmiast zwrócił uwagę osób został prostotę, z którym jeden można tworzyć i uruchomić serwera sieci Web. Jeśli cele Katana były w ramce świetle z [Node.js](http://nodejs.org/), co może je podsumowania przez informujący o tym, że Katana oferuje wiele zalet [Node.js](http://nodejs.org/) (i platform, takich jak jej) bez wymuszania deweloperowi porzucanie wszystko, co użytkownik wie, informacje o tworzeniu aplikacji sieci Web ASP.NET. W tej instrukcji do przechowywania true wprowadzenie projektu Katana powinna być równie proste, w charakterze [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Tworzenie "Witaj świecie!"

Jeden znacząca różnica między JavaScript i .NET development jest obecności lub braku kompilatora. W efekcie punkt początkowy prosty serwer Katana jest projektu programu Visual Studio. Jednak możemy rozpocząć z najbardziej minimalnego typów projektów: pusta aplikacja sieci Web platformy ASP.NET.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Następnie zostanie zainstalowany [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) pakiet NuGet do projektu. Ten pakiet zawiera serwer OWIN, który jest uruchamiany w potoku żądania ASP.NET. Znaleziono go na [galerii NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) które można zainstalować przy użyciu okna dialogowego Menedżera pakietu Visual Studio lub konsoli Menedżera pakietów przy użyciu następującego polecenia:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalowanie `Microsoft.Owin.Host.SystemWeb` pakietu zainstaluje kilka dodatkowych pakietów jako zależności. Jeden z tych zależności jest `Microsoft.Owin`, bibliotekę, która udostępnia kilka pomocnicze typy i metody do tworzenia aplikacji OWIN. Możemy użyć tych typów do szybkiego zapisu następujący serwer "hello world".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Ten bardzo prosty serwer sieci Web może zostać uruchomiony przy użyciu programu Visual Studio **F5** poleceń i zapewnia pełną obsługę debugowania.

## <a name="switching-hosts"></a>Przełączanie hostów

Domyślnie poprzedniego przykładu "hello world" działa w potoku żądania programu ASP.NET, który używa System.Web w kontekście usług IIS. To może samodzielnie Dodaj ogromne wartość jako pozwala korzystać z elastyczność i możliwości potok OWIN z możliwości zarządzania i ogólną dojrzałości usług IIS. Jednak może być przypadkach korzyści udostępniane przez usługi IIS nie są wymagane i desire jest przeznaczony dla hosta mniejsze, bardziej lightweight. Co jest potrzebne, następnie, aby uruchomić proste serwera sieci Web poza usług IIS i System.Web?

Aby zilustrować celem przenośność, przenoszenie z serwera sieci Web hosta do hosta wiersza polecenia wymaga wystarczy dodać nowe zależności serwera i hosta do folderu wyjściowego projektu, a następnie uruchomić hosta. W tym przykładzie mamy hosta serwera sieci Web w kolekcji Katana hosta o nazwie `OwinHost.exe` i korzysta z serwera opartego na Katana HttpListener. Podobnie do innych składników Katana te będzie można uzyskać z pakietu NuGet, za pomocą następującego polecenia:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Z poziomu wiersza polecenia, możemy następnie przejdź do folderu głównego projektu i wystarczy uruchomić `OwinHost.exe` (który został zainstalowany w folderze Narzędzia jego odpowiedniego pakietu NuGet). Domyślnie `OwinHost.exe` jest skonfigurowany do wyszukiwania na podstawie HttpListener serwera, a więc nie jest wymagane żadne dodatkowe czynności konfiguracyjne. Nawigacja w przeglądarce sieci Web, aby `http://localhost:5000/` przedstawiono aplikację teraz uruchomiony za pośrednictwem konsoli.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architektura Katana

 Architektura składników Katana dzieli aplikację na cztery warstwy logicznych, jak pokazano poniżej: *hosta, serwera, oprogramowanie pośredniczące,* i *aplikacji*. Architektura składników jest brana pod uwagę w taki sposób, że implementacje tych warstw można łatwo zastąpić, w wielu przypadkach, bez konieczności ponownej kompilacji aplikacji.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 Host jest odpowiedzialny za:

- Zarządzanie w podstawowym procesie.
- Organizowanie przepływu pracy, który powoduje wybór serwera i budowy potoku OWIN za pośrednictwem których żądań obsługi.

 Obecnie istnieją 3 podstawowe opcje hostingu dla aplikacji opartych na Katana:  
  
**IIS/ASP.NET**: używanie standardowych typów HttpModule i HttpHandler, potoki OWIN można uruchomić na serwerze IIS jako część przepływu żądania ASP.NET. Hosting pomocy technicznej platformy ASP.NET jest włączony, instalując pakiet Microsoft.AspNet.Host.SystemWeb NuGet do projektu aplikacji sieci Web. Ponadto ponieważ usługi IIS działa jako serwer i hostów, w tym pakiecie NuGet, co oznacza, że używany SystemWeb host, Projektant nie może zastąpić implementację alternatywny serwer jest conflated rozróżnienie hosta serwera/OWIN.  
  
**Niestandardowy Host**: zestaw komponentów Katana umożliwia dewelopera umożliwia obsługę aplikacji własne niestandardowe procesu, czy to aplikacji konsoli, usługa systemu Windows itd. Ta funkcja wygląda podobnie do hosta samodzielnego możliwości udostępniony przez interfejs API sieci Web. W poniższym przykładzie przedstawiono hosta niestandardowego kodu interfejsu API sieci Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Host samodzielny Instalator dla aplikacji Katana jest podobne:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Jeden znacząca różnica między interfejsu API sieci Web i Katana przykłady hosta samodzielnego jest brak przykład hosta samodzielnego Katana kod konfiguracji interfejsu API sieci Web. Aby włączyć zarówno mobilność i możliwości, Katana oddziela kod, który uruchamia serwer z kodu, który konfiguruje żądanie przetwarzania potoku. Kod konfiguruje interfejs API sieci Web, a następnie jest zawarty w klasie startu, a ponadto jest określony jako parametr typu w WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Klasa początkowa zostanie omówiona bardziej szczegółowo w dalszej części tego artykułu. Jednak kod wymaganych do uruchomienia Katana, proces hosta samodzielnego przypomina właśnie kod może być za pomocą którego obecnie w aplikacji hosta samodzielnego interfejsu API sieci Web platformy ASP.NET.

**OwinHost.exe**: podczas niektórych będzie mają zostać zapisane niestandardowy proces do uruchamiania aplikacji sieci Katana Web, wiele wolisz wystarczy uruchomić wcześniej utworzonego pliku wykonywalnego, który można uruchomić serwera i uruchamianie ich aplikacji. W tym scenariuszu zawiera zestaw składników Katana `OwinHost.exe`. Gdy uruchamiane w katalogu głównym projektu, tego pliku wykonywalnego uruchomi serwer (używa serwera HttpListener domyślnie) i umożliwia konwencje Znajdź i uruchom klasa startowa użytkownika. Do zapewnienia jeszcze bardziej precyzyjnej kontroli pliku wykonywalnego zawiera wiele parametrów wiersza polecenia.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Serwer

 Gdy host jest odpowiedzialna za uruchamianie i utrzymywanie proces, w którym jest uruchomiona aplikacja, odpowiedzialność serwera jest otworzyć gniazda sieci nasłuchiwać żądań i wysyłać je za pośrednictwem potoku OWIN składniki określone przez użytkownika (jak już zauważalne, tego potoku jest określona w klasie uruchamiania Deweloper aplikacji). Obecnie projekt Katana zawiera dwa implementacji serwera: 

- **Microsoft.Owin.Host.SystemWeb**: jak wcześniej wspomniano, usług IIS w połączeniu z potoku ASP.NET działa jako hostem i serwerem. W związku z tym, wybierając to opcji obsługi, usługi IIS zarówno zarządza na poziomie hosta problemy, takie jak proces aktywacji i nasłuchuje żądań HTTP. Dla aplikacji sieci Web ASP.NET następnie wysyła żądania do potoku ASP.NET. Host Katana SystemWeb rejestruje ASP.NET HttpModule i HttpHandler przechwycenia żądań przepływać przez potoku HTTP i wysyłać je za pośrednictwem potoku OWIN określone przez użytkownika.
- **Microsoft.Owin.Host.HttpListener**: jak jego nazwa wskazuje, ten serwer Katana używa klasy HttpListener .NET Framework do otwierania gniazda i wysyłania żądań do potoku OWIN określony przez dewelopera. Ta funkcja jest obecnie domyślny wybór serwera dla interfejsu API Katana hosta samodzielnego i OwinHost.exe.

## <a name="middlewareframework"></a>Oprogramowanie pośredniczące/framework

 Jak wcześniej wspomniano gdy serwer akceptuje żądania w kliencie jest odpowiedzialny za przekazywanie jej przez potok OWIN składników, które są określone przez kod startowy dewelopera. Te składniki potoku są nazywane oprogramowania pośredniczącego.  
 Na poziomie bardzo proste składnik oprogramowania pośredniczącego OWIN po prostu należy zaimplementować delegata aplikacji OWIN tak, aby można wywołać.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Aby ułatwić projektowanie i kompozycji składników oprogramowania pośredniczącego, Katana obsługuje jednak kilka konwencje i typy pomocnika dla składników oprogramowania pośredniczącego. Najbardziej typowe tych jest `OwinMiddleware` klasy. Składnik oprogramowania pośredniczącego niestandardowych utworzony za pomocą tej klasy będzie wyglądać podobnie do poniższej: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Ta klasa pochodzi od `OwinMiddleware`, implementuje konstruktora, który akceptuje wystąpienia następne oprogramowanie pośredniczące w potoku jako jeden z jego argumentów, a następnie przekazuje do konstruktora podstawowego. Dodatkowe argumenty używane do konfigurowania oprogramowania pośredniczącego również są deklarowane jako parametry konstruktora po następnym parametr oprogramowania pośredniczącego.   
  
W czasie wykonywania, oprogramowanie pośredniczące jest wykonywana za pośrednictwem przesłoniętych `Invoke` metody. Ta metoda przyjmuje jeden argument typu `OwinContext`. Ten obiekt kontekstu jest zapewniana przez `Microsoft.Owin` pakietu NuGet opisanych wcześniej i udostępnia silnie typizowane słownika żądania, odpowiedzi i środowiska, wraz z kilku typów dodatkowego pomocy.   
  
Klasa oprogramowanie pośredniczące mogą być łatwo dodawane do potoku OWIN w kodzie uruchamiania aplikacji w następujący sposób:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Ponieważ infrastruktura Katana po prostu tworzy potoku składników oprogramowania pośredniczącego OWIN i składniki po prostu musi obsługiwać delegata aplikacji do udziału w potoku, składników oprogramowania pośredniczącego można dostosować w zakresie rozwiązania od prostego rejestratory do całej struktury, takich jak ASP.NET, interfejsu API sieci Web, lub [SignalR](../../../signalr/index.md). Dodawanie interfejsu API sieci Web platformy ASP.NET do potoku OWIN poprzedniego wymaga na przykład, dodając następujący kod uruchomienia:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Infrastruktura Katana utworzy potoku składników oprogramowania pośredniczącego na podstawie kolejności, w jakiej zostały dodane do obiektu IAppBuilder metody konfiguracji. Następnie, w tym przykładzie LoggerMiddleware może obsługiwać wszystkie żądania, które wpływają za pośrednictwem potoku, niezależnie od tego, jak te żądania ostatecznie są obsługiwane. Dzięki temu zaawansowanych scenariuszy, w którym składnik oprogramowania pośredniczącego (np. Składnik uwierzytelniania) może przetwarzać żądania potok, który obejmuje wiele składników i struktur (np. interfejsu API sieci Web platformy ASP.NET SignalR i serwera plików statycznych).
 
## <a name="applications"></a>Aplikacje

Zgodnie z opisami w poprzednich przykładach, OWIN i projektu Katana powinien nie można traktować jako nowy model programowania aplikacji, ale raczej jako abstrakcję rozdzielenie modele programowania aplikacji i platform z serwera i infrastruktury hostingu. Na przykład podczas tworzenia aplikacji interfejsu API sieci Web, w ramach developer będzie za pomocą architektury interfejsu API sieci Web platformy ASP.NET, niezależnie od tego, czy aplikacja działa w potoku OWIN za pomocą składników z projektu Katana. Jednym miejscu, gdzie związanych z OWIN kodu będą widoczne dla projektanta aplikacji będzie kodu uruchamiania aplikacji, gdzie dewelopera Redaguj potoku OWIN. W kodzie uruchomienia projektanta zarejestruje serię instrukcji UseXx, zwykle po jednej dla każdego składnika oprogramowania pośredniczącego, która będzie przetwarzać żądań przychodzących. To środowisko będzie mieć ten sam efekt co rejestrowanie modułów HTTP w bieżącym środowisku System.Web. Zazwyczaj większych framework oprogramowanie pośredniczące, np. interfejsu API sieci Web ASP.NET lub [SignalR](../../../signalr/index.md) zostanie zarejestrowany na końcu potoku. Kompleksowymi składników oprogramowania pośredniczącego, takich jak te dotyczące uwierzytelniania lub buforowania, zazwyczaj są zarejestrowane na początku potoku, tak aby ich będzie przetwarzać żądania dla wszystkich platform i składniki w zarejestrowany później w potoku. Ta separacja składników oprogramowania pośredniczącego z sobą oraz z podstawowych składników infrastruktury umożliwia składniki podlegać ewolucji w różnych prędkości przy jednoczesnym zapewnieniu, że cały system jest trwały.

## <a name="components--nuget-packages"></a>Składniki — pakietów NuGet

Podobnie jak wiele bieżącego bibliotek i platform Katana składniki projektu są dostarczane jako zestaw pakietów NuGet. Nadchodzącej wersji 2.0 na wykresie zależności pakietu Katana wygląda następująco. (Kliknij obraz dla widoku większy).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Niemal każdy pakiet w projekcie Katana zależy, bezpośrednio lub pośrednio, pakiet Owin. Może należy pamiętać, że jest to pakiet zawierający interfejsu IAppBuilder, który udostępnia konkretną implementację sekwencja uruchamiania aplikacji opisane w sekcji 4 specyfikacji OWIN. Ponadto wiele pakietów zależą od Microsoft.Owin, który zapewnia zbiór pomocnika typy do pracy z żądań i odpowiedzi HTTP. W pozostałej części pakietu mogą być klasyfikowane jako hostingu pakietów infrastruktury (serwerów lub hostów) lub oprogramowania pośredniczącego. Pakietów i zależności zewnętrznych dla projektu Katana są wyświetlane w kolorze pomarańczowym.

Hostingu infrastruktura Katana 2.0 obejmuje zarówno SystemWeb oraz serwerów opartych na HttpListener, pakiet OwinHost dla aplikacji OWIN za pomocą OwinHost.exe i pakietu Microsoft.Owin.Hosting dla samodzielnej obsługi aplikacji OWIN w niestandardowy host (np. aplikacji konsoli usługi systemu Windows, itp.)

2.0 Katana składników oprogramowania pośredniczącego przede wszystkim jest ukierunkowana na zapewnienie inną metodę uwierzytelniania. Dostępne są jeden składnik dodatkowe oprogramowanie pośredniczące dla diagnostyki, który umożliwia obsługę stronę początkową i błędów. Wraz z rozwojem OWIN do faktycznego abstrakcji hostingu, ekosystem składników oprogramowania pośredniczącego, oba te opracowane przez firmę Microsoft i stron trzecich również wzrośnie numer.

## <a name="conclusion"></a>Wniosek

 Od jej początku celem projektu Katana nie był do tworzenia i tym samym wymusić deweloperów, aby dowiedzieć się jeszcze innej platformy sieci Web. Zamiast została celem można utworzyć abstrakcję umożliwiają deweloperom aplikacji sieci Web .NET wybór więcej niż wcześniej było możliwe. Przez podzielenie warstw logicznych typowe stosu aplikacji sieci Web na zestaw składników wymienne, projektu Katana umożliwia składniki w stosie, aby poprawić na niezależnie od szybkości ma sens dla tych składników. Tworząc wszystkie składniki wokół proste abstrakcji OWIN Katana umożliwia struktury i aplikacje oparty na nich być przenośne różnych różne serwery i hosty. Umieszczając dewelopera w formancie stosu, Katana gwarantuje, że deweloper sprawia, że wybór ultimate o jak lekkie lub w sposób bogate jej stos sieci Web.  
  

## <a name="for-more-information-about-katana"></a>Aby uzyskać więcej informacji na temat Katana

- Projekt Katana w witrynie GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Wideo: [projekt Katana — OWIN dla platformy ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), przez Dierking Howarda.

## <a name="acknowledgements"></a>Potwierdzeń

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick jest starszy programowania modułu zapisującego dla Microsoft koncentrujących się na platformie Azure i MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jan Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
