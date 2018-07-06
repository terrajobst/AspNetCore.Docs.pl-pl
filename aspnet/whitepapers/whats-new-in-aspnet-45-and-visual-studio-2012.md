---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: What's New in ASP.NET 4.5 i programu Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis nowych funkcji i ulepszeń wprowadzonych w ASP.NET 4.5. Opisano również usprawnienia do tworzenia aplikacji sieci web...
ms.author: aspnetcontent
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: cfe9b1a7f05b43d5eb638c8fa7cb581d1edac9d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835816"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>What's New in ASP.NET 4.5 i programu Visual Studio 2012
====================
> Ten dokument zawiera opis nowych funkcji i ulepszeń wprowadzonych w ASP.NET 4.5. Omówiono także usprawnienia do tworzenia aplikacji internetowych w programie Visual Studio 2012. Ten dokument został pierwotnie opublikowany 29 lutego 2012 r.


- [Środowisko uruchomieniowe programu ASP.NET Core i struktury](#_Toc318097372)

    - [Asynchronicznego odczytywania i zapisywania żądań i odpowiedzi HTTP](#_Toc318097373)
    - [Ulepszenia obsługi HttpRequest](#_Toc318097374)
    - [Asynchronicznie opróżnianie odpowiedzi](#_Toc318097375)
    - [Obsługa *await* i *zadań*— na podstawie modułów asynchronicznych i programów obsługi](#_Toc318097376)
    - [Asynchroniczne moduły HTTP](#_Toc318097377)
    - [Asynchroniczne funkcje obsługi protokołu HTTP](#_Toc318097378)
    - [Nowe funkcje sprawdzania poprawności żądania programu ASP.NET](#_Toc318097379)
    - [Odroczone sprawdzanie poprawności żądań ("z opóźnieniem")](#_Toc318097380)
    - [Obsługa żądań niezweryfikowane](#_Toc318097381)
    - [Biblioteki AntiXSS](#_Toc318097382)
    - [Obsługa protokołu WebSockets](#_Toc318097383)
    - [Tworzenie pakietów i minifikacja](#_Toc318097384)
    - [Ulepszenia wydajności dotyczące hostingu sieci Web](#_Toc_perf)

        - [Czynnikami Wydajnościowymi klucza](#_Toc_perf_1)
        - [Wymagania dotyczące nowych funkcji wydajności](#_Toc_perf_2)
        - [Udostępnianie zestawów wspólnych](#_Toc_perf_3)
        - [Szybsze uruchamianie przy użyciu wielordzeniowych kompilacja JIT](#_Toc_perf_4)
        - [Dostrajanie wyrzucania elementów bezużytecznych zoptymalizowane pod kątem pamięci](#_Toc_perf_5)
        - [Trwa pobieranie z wyprzedzeniem dla aplikacji sieci web](#_Toc_perf_6)
- [ASP.NET Web Forms](#_Toc318097385)

    - [Silnie typizowane kontrolki danych](#_Toc318097386)
    - [Wiązanie modelu](#_Toc318097387)

        - [Wybór danych](#_Toc318097388)
        - [Dostawców wartości](#_Toc318097389)
        - [Filtrowanie według wartości w kontrolce](#_Toc318097390)
    - [Wyrażenia wiązania danych kodowania HTML](#_Toc318097391)
    - [Sprawdzania poprawności dyskretnego kodu](#_Toc318097392)
    - [Aktualizacje HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 w wersji Release Candidate](#_Toc318097396)

    - [Projekt do udostępniania między Visual Studio 2010 i Visual Studio 2012 w wersji Release Candidate (tryb zgodności projektu)](#project-compatibility)
    - [Zmiany konfiguracji w szablonach 4.5 witryny sieci Web platformy ASP.NET](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Macierzysta obsługa w usługach IIS 7 dla routingu platformy ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Edytor HTML](#_Toc318097397)

        - [Inteligentne zadań](#_Toc318097398)
        - [Poczeka ARIA pomocy technicznej](#_Toc318097399)
        - [Nowe fragmenty HTML5](#_Toc318097400)
        - [Wyodrębnij do kontrolki użytkownika](#_Toc318097401)
        - [Funkcja IntelliSense dla kodu nuggets w atrybutach](#_Toc318097402)
        - [Automatyczna zmiana nazwy pasujących tagów, po zmianie nazwy opening lub tag zamykający](#_Toc318097403)
        - [Generowanie programów obsługi zdarzeń](#_Toc318097404)
        - [Inteligentne wcięcie](#_Toc318097405)
        - [Zmniejsz automatyczne uzupełnianie instrukcji](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Zwijanie kodu](#_Toc318097408)
        - [Parowanie nawiasów klamrowych](#_Toc318097409)
        - [Przejdź do definicji](#_Toc318097410)
        - [Obsługa ECMAScript5](#_Toc318097411)
        - [Funkcja IntelliSense w modelu DOM](#_Toc318097412)
        - [VSDOC sygnatury przeciążenia](#_Toc318097413)
        - [Niejawne odwołania](#_Toc318097414)
    - [Edytor CSS](#_Toc318097415)

        - [Zmniejsz automatyczne uzupełnianie instrukcji](#_Toc318097416)
        - [Wcięcie hierarchiczne.](#_Toc318097417)
        - [CSS uzyskuje dostęp do pomocy technicznej](#_Toc318097418)
        - [Schematy określonego dostawcy (- moz-, - webkit)](#_Toc318097419)
        - [Komentowania i Trwa usuwanie komentarza do pomocy technicznej](#_Toc318097420)
        - [Selektor kolorów](#_Toc318097421)
        - [Wstawki kodu](#_Toc318097422)
        - [Niestandardowe regionów](#_Toc318097423)
    - [Narzędzie Page Inspector](#_Toc318097424)
    - [Publikowanie](#_Toc318097425)

        - [Profile publikowania](#_Toc318097426)
        - [Wstępnej kompilacji platformy ASP.NET i scalania](#_Toc318097427)
- [Usługi IIS Express](#_Toc318097428)
- [Zrzeczenie odpowiedzialności](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Środowisko uruchomieniowe programu ASP.NET Core i struktury

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchronicznego odczytywania i zapisywania żądań i odpowiedzi HTTP

ASP.NET 4 wprowadzono możliwość odczytu jednostki żądania HTTP jako strumienia z wykorzystaniem *HttpRequest.GetBufferlessInputStream* metody. Ta metoda podana przesyłania strumieniowego dostęp do jednostki żądania. Jednak ono wykonywane synchronicznie, który blokowana wątku na czas trwania żądania.

Program ASP.NET 4.5 obsługuje możliwość odczytu strumieni asynchronicznie dla jednostki żądania HTTP, a także możliwość opróżniania asynchronicznie. Program ASP.NET 4.5 daje również możliwość podwójnego buforowania jednostki żądania HTTP, oferujący kontrolerów ułatwia integrację z podrzędnym funkcje obsługi protokołu HTTP, takich jak programy obsługi strony .aspx i platformy ASP.NET MVC.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Ulepszenia obsługi HttpRequest

Stream Odwołanie zwrócone przez funkcję ASP.NET 4.5 z *HttpRequest.GetBufferlessInputStream* obsługuje synchroniczne i asynchroniczne metody odczytu. *Stream* obiekt zwracany z *GetBufferlessInputStream* teraz implementuje BeginRead i EndRead można wywołać metody. Asynchroniczną *Stream* metody umożliwiają asynchronicznego odczytania jednostka żądania we fragmentach, podczas gdy ASP.NET zwalnia bieżącego wątku między każdej iteracji pętli odczytu asynchronicznego.

Program ASP.NET 4.5 dodano również metody pomocnika do odczytywania jednostka żądania w sposób buforowanego: *HttpRequest.GetBufferedInputStream*. To przeciążenie nowe działa jak *GetBufferlessInputStream*, obsługa synchroniczne i asynchroniczne operacje odczytu. Jednakże, jak odczytuje, *GetBufferedInputStream* również kopiuje bajtów jednostki do buforów wewnętrznego programu ASP.NET, tak, aby moduły podrzędne i programy obsługi nadal mogą uzyskiwać dostęp do jednostki żądania. Na przykład, jeśli niektóre nadrzędne kodu w potoku ma już odczytu przy użyciu jednostek żądania *GetBufferedInputStream*, można nadal używać *HttpRequest.Form* lub *HttpRequest.Files*. Dzięki temu można wykonywać asynchronicznego przetwarzania żądania (na przykład przesyłania strumieniowego przekazywanie dużych plików, do bazy danych), ale strony aspx nadal wykonywania i kontrolerów platformy ASP.NET MVC później.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchronicznie opróżnianie odpowiedzi

Wysyłania odpowiedzi do klienta HTTP może zająć znaczną ilość czasu, gdy klient jest daleko, lub z połączeniem o niskiej przepustowości. Zwykle ASP.NET buforuje bajtów odpowiedzi tworzonych przez aplikację. ASP.NET następnie wykonuje operację wysyłania pojedynczego naliczone buforów na końcu procesu przetwarzania żądania.

Jeśli buforowaną odpowiedź jest duży (na przykład, przesyłanie strumieniowe dużych plików do klienta), należy okresowo wywoływać *HttpResponse.Flush* wysyłanie buforowanych wyników do klienta i Zachowaj pamięć pod kontrolą. Jednak ponieważ *opróżniania* jest wywołanie synchroniczne wywołanie iteracyjne *opróżniania* nadal zużywa wątku na czas trwania potencjalnie długotrwałych żądań.

Program ASP.NET 4.5 alokowanej dla wykonywania opróżnia asynchronicznie za pomocą *BeginFlush* i *EndFlush* metody *HttpResponse* klasy. Przy użyciu tych metod, można utworzyć modułów asynchronicznych i obsługę asynchroniczną, których przyrostowo przesyłać dane do klienta bez zajmowania wątków systemu operacyjnego. Między *BeginFlush* i *EndFlush* wywołań, ASP.NET zwalnia bieżącego wątku. Zmniejsza to znacznie całkowita liczba aktywnych wątków, które są wymagane w celu zapewnienia obsługi długotrwałych HTTP pliki do pobrania.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Obsługa *await* i *zadań* — na podstawie modułów asynchronicznych i programów obsługi

.NET Framework 4 wprowadzono asynchronicznego koncepcji programowania, nazywane *zadań*. Zadania są reprezentowane przez *zadań* typu i powiązanych typów *System.Threading.Tasks* przestrzeni nazw. .NET Framework 4.5 opiera się na to ulepszenia kompilatora, które ułatwić pracę z *zadań* proste obiekty. W .NET Framework 4.5, kompilatory obsługi dwóch nowych słów kluczowych: *await* i *async*. *Await* — słowo kluczowe jest syntaktycznych skrótem wskazująca, że fragment kodu asynchronicznego powinien zaczekać na inne części kodu. *Async* — słowo kluczowe reprezentuje wskazówkę, która służy do oznaczania metod jako opartego na zadaniach asynchronicznej metody.

Kombinacja *await*, *async*i *zadań* obiektu ułatwia pisanie kodu asynchronicznego w .NET 4.5. Program ASP.NET 4.5 obsługuje te definiowaniu przy użyciu nowych interfejsów API, które pozwalają na zapis asynchroniczny moduły HTTP i asynchroniczne funkcje obsługi protokołu HTTP przy użyciu nowe ulepszenia kompilatora.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchroniczne moduły HTTP

Załóżmy, że chcesz wykonać pracę asynchroniczną w metodzie, która zwraca *zadań* obiektu. Poniższy przykład kodu przedstawia metodę asynchroniczną, która sprawia, że wywołanie asynchroniczne do pobrania na stronie głównej firmy Microsoft. Zwróć uwagę na *async* słowa kluczowego w podpisie metody i *await* wywołanie *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

To wszystko, trzeba napisać — .NET Framework będzie automatycznie obsługiwać odwijanie stosu wywołań podczas oczekiwania na zakończenie pobierania, a także automatyczne przywrócenie stos wywołań, po zakończeniu pobierania.

Teraz załóżmy, że chcesz użyć tej metody asynchronicznej w asynchronicznej moduł HTTP programu ASP.NET. Program ASP.NET 4.5 zawiera metody pomocnika (*EventHandlerTaskAsyncHelper*) oraz nowy typ delegata (*TaskEventHandler*) służące do integracji opartego na zadaniach asynchronicznej metody przy użyciu starszej wersji model programowania asynchronicznego udostępnianych przez potok HTTP platformy ASP.NET. Ten przykład pokazuje, jak:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchroniczne funkcje obsługi protokołu HTTP

Tradycyjne podejście do pisania obsługę asynchroniczną w programie ASP.NET jest zaimplementowanie *IHttpAsyncHandler* interfejsu. Program ASP.NET 4.5 wprowadza *HttpTaskAsyncHandler* asynchronicznego typu podstawowego, który może pochodzić z lokalizacji, która znacznie ułatwia pisanie obsługę asynchroniczną.

*HttpTaskAsyncHandler* typ jest abstrakcyjny i wymaga zastąpienia *ProcessRequestAsync* metody. Wewnętrznie ASP.NET dba o integracji zwracany podpis ( *zadań* obiektu) z *ProcessRequestAsync* przy użyciu starszych asynchronicznego modelu programowania, używany przez potoku platformy ASP.NET.

Poniższy przykład pokazuje, jak można użyć *zadań* i *await* jako część wykonania asynchronicznej programu obsługi HTTP:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nowe funkcje sprawdzania poprawności żądania programu ASP.NET

Domyślnie program ASP.NET sprawdza poprawność żądania — sprawdza, czy żądania, aby wyszukać znaczników lub skrypt w polach, nagłówki, pliki cookie i tak dalej. Jeśli dowolny zostanie wykryte, program ASP.NET zgłasza wyjątek. Jest to zabezpieczenie jako pierwsza linia obrony przed potencjalnymi ataki z użyciem skryptów między witrynami.

Program ASP.NET 4.5 ułatwia selektywnie odczytać dane niezweryfikowanych żądania. Program ASP.NET 4.5 jest zintegrowana z biblioteki AntiXSS popularnych wcześniejszemu zewnętrznej biblioteki.

Deweloperzy mają często zadawane możliwość selektywnego wyłączyć weryfikację żądań dla swoich aplikacji. Na przykład aplikacji to oprogramowanie, forum, można zezwolić użytkownikom na przesyłanie wpisy na forum w formacie HTML i komentarze, ale nadal upewnij się, czy sprawdzania poprawności żądania wszystko inne.

Program ASP.NET 4.5 wprowadza dwie funkcje, które ułatwiają selektywnie pracować niezweryfikowane dane wejściowe: odroczono Weryfikacja żądania ("z opóźnieniem") i dostęp do niezweryfikowanych żądania danych.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Odroczone sprawdzanie poprawności żądań ("z opóźnieniem")

W programie ASP.NET 4.5 domyślnie wszystkie dane żądania podlega weryfikacji żądań. Można jednak skonfigurować aplikacji, które mają być odroczone sprawdzanie poprawności żądań, dopóki faktycznie dostęp do danych żądania. (To jest czasami określane jako weryfikacji żądań z opóźnieniem, w oparciu o terminy, takie jak powolne ładowanie niektórych scenariuszy danych.) Można skonfigurować aplikację do użycia odroczone sprawdzanie poprawności w pliku Web.config, ustawiając *requestValidationMode* atrybutu 4.5 w *httpRUntime* elementu, jak w poniższym przykładzie:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Podczas żądania weryfikacji jest tryb 4.5, weryfikacja żądania jest wyzwalany tylko na potrzeby wartości określonego żądania, i tylko wtedy, gdy kod uzyskuje dostęp do tej wartości. Na przykład, jeśli kod uzyskuje wartość Request.Form["forum\_wpis"], weryfikacja żądania jest wywoływana tylko dla tego elementu w kolekcji formularza. Brak elementów w *formularza* kolekcji są prawidłowe. W poprzednich wersjach programu ASP.NET Weryfikacja żądania została wyzwolona dla żądania całej kolekcji, gdy uzyskano dostęp do dowolnego elementu w kolekcji. Nowe zachowanie ułatwia składników innej aplikacji do wzięcia pod różne dane żądania bez powodowania weryfikacji żądań na innych elementach.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Obsługa żądań niezweryfikowane

Weryfikacja żądania odroczonego samodzielnie nie rozwiąże problemu z selektywnie pomijanie weryfikacji żądań. Wywołanie Request.Form["forum\_wpis"] nadal wyzwalaczy żądania weryfikacji dla tej wartości określonego żądania. Można uzyskać dostęp do tego pola bez wyzwalania sprawdzania poprawności, ponieważ chcesz umożliwić znacznik, w tym polu.

Aby to umożliwić, ASP.NET 4.5 obsługuje teraz niezweryfikowanych dostęp do danych żądania. Program ASP.NET 4.5 zawiera nową *Unvalidated* właściwość kolekcji w *HttpRequest* klasy. Ta kolekcja zapewnia dostęp do wszystkich wartości typowych danych żądania, jak *formularza*, *QueryString*, *plików cookie*, i *adresu Url*.

Korzystając z przykładu forum, aby można było odczytać dane niezweryfikowanych żądania, najpierw musisz skonfigurować aplikację do korzystania z nowego trybu weryfikacji żądania:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Następnie można użyć *HttpRequest.Unvalidated* właściwości można odczytać wartości niezweryfikowanych formularza:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Zabezpieczenia — *żądania niezweryfikowanych danych należy używać ostrożnie!* Program ASP.NET 4.5 dodane właściwości niezweryfikowanych żądania i kolekcji, aby ułatwić dostęp do bardzo konkretnego żądania niezweryfikowanych danych. Nadal należy jednak wykonać niestandardowego sprawdzania poprawności na dane pierwotne żądania, aby upewnić się, czy niebezpiecznych tekstu nie są odtwarzane dla użytkowników.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteki AntiXSS

Z powodu popularności Microsoft AntiXSS Library ASP.NET 4.5 zawiera podstawowe procedury kodowania z biblioteki w wersji 4.0.

Kodowanie procedury są wykonywane przez *AntiXssEncoder* typu w nowym *System.Web.Security.AntiXss* przestrzeni nazw. Możesz użyć *AntiXssEncoder* typu bezpośrednio przez wywołanie dowolnej metody kodowania statyczne, które są implementowane w typie. Jednak jest to najłatwiejsza metoda dotyczące korzystania z nowej procedury anti-XSS skonfigurować aplikację ASP.NET do korzystania *AntiXssEncoder* klasy domyślnie. Aby to zrobić, Dodaj następujący atrybut w pliku Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Gdy *encoderType* atrybutu jest skonfigurowany do używania *AntiXssEncoder* typu, wszystkie dane wyjściowe kodowania w programie ASP.NET: automatycznie używa nowej procedury kodowania.

Oto części zewnętrznej biblioteki AntiXSS, które zostały włączone do ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, i *HtmlAttributeEncode*
- *XmlAttributeEncode* i *XmlEncode*
- *UrlEncode* i *UrlPathEncode* (nowy)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Obsługa protokołu WebSockets

Protokół Websocket jest protokołem sieci opartych na standardach, który określa, jak nawiązać bezpieczne i w czasie rzeczywistym dwukierunkową komunikację między klientem a serwerem za pośrednictwem protokołu HTTP. Firma Microsoft podjęła współpracę z IETF i W3C organami standardów, aby uprościć określenie protokołu. Protokół Websocket jest obsługiwany przez klienta (nie tylko przeglądarki) inwestowanie znaczne zasoby obsługujące protokół Websocket zarówno klient, jak i systemach operacyjnych urządzeń przenośnych firmy Microsoft.

Protokół Websocket sprawia, że znacznie łatwiej tworzyć długoterminowych transfery między klientem a serwerem. Na przykład pisania aplikacji rozmów jest znacznie łatwiejsze, ponieważ może nawiązać true połączenie długotrwałych między klientem a serwerem. Nie masz odwołać się do rozwiązania problemu, takich jak okresowe sondowanie lub HTTP długiego sondowania aby symulować gniazda.

ASP.NET 4.5 i usługi IIS 8 zapewnia obsługę niskiego poziomu funkcji WebSockets, dzięki czemu deweloperzy platformy ASP.NET mogą używać zarządzanych interfejsów API do asynchronicznego odczytywania i zapisywania ciągów i danych binarnych dla obiektu Websocket. Przez funkcję ASP.NET 4.5 jest dostępny nowy *System.Web.WebSockets* przestrzeni nazw, która zawiera typy do pracy za pomocą protokołu Websocket.

Klient przeglądarki ustanawia połączenie Websocket, tworząc DOM *WebSocket* obiektu, który wskazuje na adres URL w aplikacji ASP.NET, jak w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Możesz utworzyć WebSockets punktów końcowych w aplikacji ASP.NET przy użyciu dowolnego rodzaju moduł ani Obsługa. W poprzednim przykładzie ashx użyto pliku, ponieważ pliki ashx są szybkie utworzenie programu obsługi.

Zgodnie z protokołu Websocket aplikacji ASP.NET akceptuje żądania WebSockets klienta, wskazując żądania powinny zostać uaktualnione z żądania HTTP GET do żądania funkcji WebSockets. Oto przykład:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest* metoda przyjmuje delegata funkcji, ponieważ ASP.NET rozwija bieżącego żądania HTTP, a następnie przekazuje sterowanie do delegata funkcji. Koncepcyjnie to podejście jest podobne do wykorzystania *System.Threading.Thread*, gdzie należy zdefiniować delegata rozpoczęcia wątku w tle, która jest wykonywana praca.

Po ASP.NET i klienta została pomyślnie ukończona uzgadniania WebSockets, ASP.NET wywołuje delegata usługi i uruchamiania aplikacji funkcji WebSockets. Poniższy przykład kodu pokazuje aplikacji proste echo, która korzysta z wbudowanej obsługi funkcji WebSockets w programie ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Obsługa w .NET 4.5 dla *await* — słowo kluczowe i opartego na zadaniach operacji asynchronicznych jest naturalnym wyborem w przypadku tworzenia aplikacji funkcji WebSockets. W przykładzie kodu pokazano żądania WebSockets jest uruchamiane w programie ASP.NET całkowicie asynchronicznie. Aplikacji asynchronicznie czeka na komunikat do wysłania w kliencie przez wywołanie metody *await gniazda. ReceiveAsync*. Podobnie, wysyłanie komunikatów asynchronicznych do klienta przez wywołanie metody *await gniazda. SendAsync*.

W przeglądarce, aplikacja odbiera komunikaty Websocket za pośrednictwem *onmessage* funkcji. Aby wysłać komunikat z przeglądarki, należy wywołać *wysyłania* metody *WebSocket* typu modelu DOM, tak jak pokazano w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

W przyszłości firma Microsoft może aktualizacje do tej funkcjonalności, że abstrakcyjne natychmiast niektóre niskiego poziomu kodowania oznacza to wymagane w tej wersji dla funkcji WebSockets aplikacji.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Tworzenie pakietów pozwala łączyć pojedyncze pliki JavaScript i CSS w pakiet, który może być traktowana jak pojedynczy plik. Minimalizowanie zmniejsza objętość plików JavaScript i CSS, usuwając odstępów i innych znaków, które nie są wymagane. Te funkcje działają z formularzy sieci Web, ASP.NET MVC i stron sieci Web.

Pakiety są tworzone przy użyciu klasy pakietu lub jednej z jej klas podrzędnych ScriptBundle i StyleBundle. Po skonfigurowaniu wystąpienie pakietu, pakietu jest udostępniane na przychodzące żądania po prostu dodając go do globalnego wystąpienia BundleCollection. W domyślnych szablonach konfiguracji pakietu odbywa się w pliku BundleConfig. Konfiguracja domyślna tworzy pakiety dla wszystkich podstawowych skryptów i plików css używany przez Szablony.

Pakiety są wywoływane przy użyciu jednej z kilku metod pomocniczych możliwe z w obrębie widoków. Aby zapewnić obsługę renderowania różny kod znaczników dla pakietu w przypadku debugowania, a tryb wersji, ScriptBundle StyleBundle mają i metody pomocnika, renderowania. Podczas pracy w trybie debugowania, renderowania wygeneruje kod znaczników dla każdego zasobu w pakiecie. W trybie wersji renderowania wygeneruje element znaczników jednego dla całego pakietu. Przełączanie między debugowaniem i wydawaniem trybu można osiągnąć, modyfikując atrybut debugowania elementu kompilacji, w pliku web.config, jak pokazano poniżej:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Ponadto włączenie lub wyłączenie optymalizacji można ustawić bezpośrednio za pomocą właściwości BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Gdy pliki są powiązane, są one najpierw posortowane alfabetycznie (sposób, w jakiej są wyświetlane w **Eksploratora rozwiązań**). Następnie są organizowane, aby znanych bibliotek i ich rozszerzenia niestandardowe (np. jQuery MooTools i Dojo) są najpierw ładowane. Na przykład będzie końcowego zamówienia dla uwzględniała folder skryptów, jak pokazano powyżej:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Pliki CSS także sortowana alfabetycznie i następnie zreorganizować tak, aby reset.css i normalize.css występować przed każdy inny plik. Końcowe sortowania uwzględniała folder style powyżej będzie to:

1. Reset.css
2. Content.css
3. Forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Ulepszenia wydajności dotyczące hostingu sieci Web

.NET Framework 4.5 i Windows 8 wprowadzone funkcje, które mogą pomóc Ci osiągnąć boost istotnie poprawiającą wydajność w przypadku obciążeń serwera sieci web. Obejmuje to zmniejszenie (maksymalnie 35%) w obu czasu uruchamiania i ilość pamięci zajmowaną przez hostingu witryn, które za pomocą programu ASP.NET w sieci web.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Czynnikami wydajnościowymi klucza

W idealnym przypadku wszystkie witryny sieci Web powinien być aktywny, a w pamięci, aby zapewnić szybką odpowiedź dla następnego żądania, gdy chodzi. Czynniki, które mogą wpływać na czas odpowiedzi witryny obejmują:

- Czas trwania dla lokacji w celu ponownego uruchomienia po pula aplikacji jest odtwarzana. Jest to czas potrzebny do uruchomienia procesu serwera sieci web, witryny, zestawy lokacji nie są już w pamięci. (Zestawy platformy są nadal w pamięci, ponieważ są one używane przez innych witryn). Ta sytuacja jest określany jako "zimnej lokacji i uruchamiania ciepło framework" lub po prostu "zimnej lokacji uruchamiania."
- Ile pamięci zajmuje lokacji. Postanowienia dotyczące to są "zużycie pamięci dla lokacji" lub "anulować zestaw roboczy".

Nowe ulepszenia wydajności skoncentrować się na oba te czynniki.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Wymagania dotyczące nowych funkcji wydajności

Wymagania dotyczące nowych funkcji mogą być podzielone na następujące kategorie:

- Ulepszenia, które działają w .NET Framework 4.
- Ulepszenia, które wymagają programu .NET Framework 4.5, ale można uruchomić w dowolnej wersji systemu Windows.
- Ulepszenia, które są dostępne tylko z systemem Windows 8 program .NET Framework 4.5.

Zwiększona wydajność przy czym każdy poziom zabezpieczeń, które można włączyć.

Niektóre usprawnienia programu .NET Framework 4.5 zalet szersze funkcje wydajności, które dotyczą również innych scenariuszy.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Udostępnianie zestawów wspólnych

**Wymaganie**: .NET Framework 4 i Visual Studio 11 Developer Preview SDK

Różne lokacje na serwerze często używają tych samych zestawów pomocnika (na przykład zestawy z starter kit lub przykładowej aplikacji). Każda lokacja ma własną kopię tych zestawów w katalogu Bin. Mimo że kod obiektowy dla zestawów jest identyczny, są fizycznie oddzielnymi zestawów, więc każdego zestawu nie można odczytać oddzielnie podczas uruchamiania zimnej lokacji i od siebie oddzielone w pamięci.

Nowe funkcje interning rozwiązuje to nieefektywne podejście, a także ogranicza wymagania dotyczące pamięci RAM i obciążenia czasu. Wewnętrzne przygotowanie umożliwia Windows zachować pojedynczej kopii każdego zestawu w systemie plików, a poszczególne zestawy w folderach Bin witryny są zastępowane linki symboliczne do pojedynczej kopii. Jeśli danej witryny wymaga różnych wersji zestawu, łącza symbolicznego zostaje zastąpiona przez nową wersję zestawu i dotyczy tylko tej lokacji.

Udostępnianie zestawów przy użyciu łącza symbolicznego wymaga nowego narzędzia o nazwie aspnet\_intern.exe, która pozwala na tworzenie i zarządzanie nimi w magazynie interned zestawów. Jest ona udostępniana jako część programu Visual Studio 11 Developer Preview SDK. (Jednak będą działać w systemie, który zawiera tylko .NET Framework 4 zainstalowany, zakładając, że zainstalowano najnowszy [aktualizacji](https://support.microsoft.com/kb/2468871).)

Aby upewnić się, wszystkie kwalifikujące się zestawy zostały interned., należy uruchomić aspnet\_intern.exe okresowo (na przykład raz w tygodniu jako zaplanowane zadanie). Typowym zastosowaniem jest następująca:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Aby wyświetlić wszystkie opcje, należy uruchomić narzędzie bez argumentów.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Szybsze uruchamianie przy użyciu wielordzeniowych kompilacja JIT

**Wymaganie**: .NET Framework 4.5

Do uruchomienia zimnej lokacji nie tylko zestawy muszą być odczytane z dysku, ale lokacji musi być kompilowany dokładnie na czas. W przypadku złożonych witryny to dodanie znaczne opóźnienia. Nowe techniką ogólnego przeznaczenia w programie .NET Framework 4.5 zmniejsza te opóźnienia przez rozłożenie kompilacja JIT na dostępne rdzenie. Dzieje się tak dużo i tak szybko, jak to możliwe, korzystając z informacji zebranych podczas poprzedniego uruchamia witryny. Ta funkcja implementowana przez [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) metody.

JIT — kompilowanie przy użyciu wiele rdzeni jest domyślnie włączona w programie ASP.NET, dzięki czemu nie trzeba nic robić, aby skorzystać z tej funkcji. Jeśli chcesz wyłączyć tę funkcję, należy podjąć następujące ustawienie w pliku Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Dostrajanie wyrzucania elementów bezużytecznych zoptymalizowane pod kątem pamięci

**Wymaganie**: .NET Framework 4.5

Po uruchomieniu witryny można jej użycie sterty modułu odśmiecania (GC) może być ważnym czynnikiem zużycie pamięci. Podobnie jak dowolny moduł odśmiecania pamięci .NET Framework GC sprawia, że kompromis między czasu procesora CPU (częstotliwość i znaczenia kolekcji) i zużycie pamięci (dodatkowego miejsca jest używany dla nowych, zwolnione lub bezpłatne możliwością obiektów). Poprzednie wersje zostały zamieszczone wskazówki dotyczące sposobu konfigurowania GC, aby osiągnąć równowagę (na przykład zobacz [ASP.NET 2.0/3,5 hostingu konfiguracji udostępnionej](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

.NET Framework 4.5, zamiast wiele ustawień autonomicznych, ustawienia konfiguracji zdefiniowane obciążenia jest dostępny, który umożliwia wszystkie wcześniej zalecanych ustawień GC także nowe dostrajania, które zapewnia dodatkową wydajność dla poszczególnych witryn Zestaw roboczy.

Aby włączyć dostrajania pamięci GC, Dodaj następujące ustawienie do pliku Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Jeśli znasz wcześniejszych wskazówkach dla zmian do aspnet.config, weź pod uwagę, że ustawienie to zastępuje stare ustawienia — na przykład, nie ma potrzeby ustawić gcserver — gcconcurrent —, itp. Nie trzeba usunąć stare ustawienia.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Trwa pobieranie z wyprzedzeniem dla aplikacji sieci web

**Wymaganie**: .NET Framework 4.5, uruchomiony w systemie Windows 8

Dla kilku wersji Windows dostępnych technologii, znane jako [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , która ogranicza koszt odczytu z dysku uruchomienia aplikacji. Ponieważ zimnego uruchamiania jest to problem głównie dla aplikacji klienckich, ta technologia nie została uwzględniona w systemie Windows Server, która zawiera tylko te składniki, które są niezbędne do serwera. Trwa pobieranie z wyprzedzeniem jest teraz dostępna w najnowszej wersji systemu Windows Server, w którym można zoptymalizować uruchomienia poszczególnych witrynach sieci Web.

W systemie Windows Server prefetcher nie jest domyślnie włączone. Aby włączyć i skonfigurować prefetcher dla hostingu w sieci web o wysokiej gęstości, uruchom następujący zestaw poleceń w wierszu polecenia:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Następnie Aby zintegrować prefetcher z aplikacjami ASP.NET, Dodaj następujący element do pliku Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Silnie Typizowane kontrolki danych

W programie ASP.NET 4.5 Web Forms zawiera kilka ulepszeń do pracy z danymi. Poprawa pierwszy jest silnie typizowane kontrolki danych. Dla formantów formularzy sieci Web w poprzednich wersjach programu ASP.NET, możesz wyświetlić, wartość powiązanych z danymi za pomocą *Eval* i wyrażenia wiązania danych:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Powiązanie dwukierunkowe danych, możesz użyć *powiązać*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

W czasie wykonywania te wywołania umożliwia odbicia odczytana wartość określonego elementu członkowskiego, a następnie Wyświetl wynik w znaczniku. Takie podejście ułatwia do tworzenia powiązań danych względem dowolnego unshaped danych.

Jednak wyrażenia wiązania danych, takich jak to nie obsługują funkcji, takich jak technologia IntelliSense dla nazwy elementów członkowskich, nawigacji (np. Przejdź do definicji) lub czasie kompilacji sprawdzania dla tych nazw.

Aby rozwiązać ten problem, ASP.NET 4.5 dodaje możliwość zadeklarować typu danych, danych, które formantem. Można to zrobić za pomocą nowego *ItemType* właściwości. Po ustawieniu tej właściwości w zakresie wyrażenia wiązania danych są dostępne dwa nowe wpisane zmienne: *elementu* i *BindItem*. Ponieważ zmienne są silnie typizowane, uzyskasz pełny zalety środowisko programistyczne programu Visual Studio.


Dwukierunkowe wyrażenia wiązania danych, można użyć *BindItem* zmiennej:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Większość formantów w ramach formularzy sieci Web ASP.NET, które obsługuje powiązanie danych zostały zaktualizowane do obsługi *ItemType* właściwości.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Wiązanie modelu

Wiązanie modelu rozszerza powiązanie danych w kontrolkach formularzy sieci Web ASP.NET do pracy z dostępem do danych skoncentrowane na kodzie. Zawiera pojęcia z *ObjectDataSource* kontroli i z wiązania modelu we wzorcu ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Wybór danych

Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu *metody SelectMethod* właściwość na nazwę metody w kodzie strony. Kontrolki danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych. Nie trzeba jawnie wywołać *elementu DataBind* metody.

W poniższym przykładzie *GridView* kontroli jest skonfigurowany do używania metodę o nazwie *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Możesz utworzyć *GetCategories* metody w kodzie strony. Dla prostych operacji wyboru metody musi żadnych parametrów i powinien zwrócić *IEnumerable* lub *IQueryable* obiektu. Jeśli nowy *ItemType* właściwość jest ustawiona (umożliwia silnie typizowane wyrażenia wiązania danych, jak wyjaśniono w [silnie Typizowane kontrolki danych](#_Toc318097386) wcześniej), ogólny wersje tych interfejsów powinien nastąpić powrót — *IEnumerable&lt;T&gt;*  lub *IQueryable&lt;T&gt;*, za pomocą *T* Parametr odpowiadającą typowi *ItemType* właściwości (na przykład *IQueryable&lt;kategorii&gt;*).

Poniższy przykład przedstawia kod dla *GetCategories* metody. Ten przykład używa modelu Entity Framework Code First z przykładową bazą danych Northwind. Kod upewnia się, że zapytanie zwraca szczegółowe informacje o powiązanych z nimi produktów dla każdej kategorii za *Include* metody. (To zapewnia, że *TemplateField* elementem w znacznikach Wyświetla liczbę produktów z każdej kategorii bez konieczności [n + 1 Zaznacz](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Po uruchomieniu strony, *GridView* kontrolować wywołania *GetCategories* metoda automatycznie i renderuje zwracanych danych przy użyciu skonfigurowanego pól:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Ponieważ metody select zwraca *IQueryable* obiektu *GridView* kontroli dalsze można manipulować zapytanie przed wykonaniem jego. Na przykład *GridView* formantu może dodać wyrażenia zapytań do sortowania i stronicowania do zwracanego *IQueryable* obiekt przed jego wykonaniem, tak, aby te operacje są wykonywane przez podstawowe Dostawca LINQ. W tym przypadku Entity Framework pewność, że te operacje są wykonywane w bazie danych.

W poniższym przykładzie przedstawiono *GridView* kontroli zmodyfikowane w celu umożliwienia sortowania i stronicowania:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Teraz po uruchomieniu na stronie, kontrolki można upewnij się, że wyświetlane jest tylko bieżącej strony i porządkowania według wybranej kolumny:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Aby filtrować dane zwracane, parametry muszą być dodane do metody wybierz. Te parametry zostaną wypełnione przez powiązanie modelu, w czasie wykonywania i można je zmienić zapytanie przed zwróceniem danych.

Załóżmy na przykład, chcesz umożliwić użytkownikom filtrowania produktów przez wprowadzenie słowa kluczowego w ciągu zapytania. Można dodać parametr do metody i zaktualizować kod, aby użyć wartości parametrów:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Ten kod zawiera *gdzie* wyrażenia, jeśli wartość jest podana dla *— słowo kluczowe* , a następnie zwraca wyniki zapytania.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Dostawców wartości

Poprzedni przykład nie jest określone, o tym, gdzie wartość *— słowo kluczowe* parametr został pochodzące z. Aby wskazać, te informacje, można użyć atrybutu parametru. Na przykład można użyć *QueryStringAttribute* klasę, która znajduje się w *System.Web.ModelBinding* przestrzeni nazw:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

To powoduje, że wiązanie modelu do wypróbowania można powiązać wartości z ciągu zapytania do *— słowo kluczowe* parametru w czasie wykonywania. (Może to obejmować wykonanie konwersji typów, chociaż w tym przypadku nie.) Jeśli nie można podać wartość, a typem wartości null, jest wyjątek.

Źródła wartości dla tych metod są określane jako dostawców wartości, a atrybuty parametrów, wskazujące, której dostawca wartości do użycia są określane jako atrybuty dostawcy wartości. Formularze sieci Web obejmuje dostawców wartości i odpowiednie atrybuty wszystkie typowe źródła danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, plików cookie, wartości formularza, formanty, stan widoku, stan sesji i właściwości profilu. Można także napisać dostawców wartości niestandardowych.

Domyślnie nazwa parametru jest używany jako klucz do znajdowania wartości w kolekcja dostawców wartości. W przykładzie kodu będzie wyglądać wartość ciągu zapytania o nazwie — słowo kluczowe (na przykład ~ / default.aspx?keyword=chef). Można określić klucz niestandardowy przez przekazanie jej jako argumentu do parametru atrybutu. Na przykład aby użyć wartości zmiennej ciągu zapytania o nazwie funkcji pytania i odpowiedzi, należy można to zrobić:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

W przypadku tej metody w kodzie strony, użytkownicy można filtrować wyniki, przekazując — słowo kluczowe, za pomocą ciągu zapytania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Wiązanie modelu wykonuje wiele zadań, które w przeciwnym razie byłoby trzeba kodować ręcznie: odczytu wartości, sprawdzanie wartości null, próbując konwertować go do odpowiedniego typu, sprawdzanie, czy konwersja powiodła się i na koniec za pomocą wartości w Zapytanie. Powiązanie modelu, wyniki w znacznie mniejszej ilości kodu i możliwość ponownego użycia funkcji w całej aplikacji.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrowanie według wartości w kontrolce

Załóżmy, że chcesz rozszerzyć przykładu tak, aby zezwolić użytkownikowi na wybranie wartość filtru z listy rozwijanej. Dodaj następujące listy rozwijanej znaczników i skonfiguruj ją, aby uzyskać dane z innego przy użyciu metody *metody SelectMethod* właściwości:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Zazwyczaj należy również dodać *EmptyDataTemplate* elementu *GridView* sterowania formantu zostanie wyświetlony komunikat, jeśli nie zostaną znalezione dopasowania produktów:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

W kodzie strony Dodaj nowy wybór metody dla listy rozwijanej:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Na koniec zaktualizuj *GetProducts* wybierz metodę, aby móc nowy parametr, który zawiera identyfikator wybraną kategorię z listy rozwijanej:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Teraz po uruchomieniu na stronie, użytkownicy mogą wybrać kategorię z listy rozwijanej i *GridView* formant jest automatycznie ponownie została powiązana z Pokaż odfiltrowane dane. Jest to możliwe, ponieważ powiązanie modelu śledzi wartości parametrów dla metod, wybierz i wykrywa, czy wszystkie wartości parametru uległy zmianie po odświeżeniu strony. Jeśli tak, wiązanie modelu wymusza kontroli powiązane dane, aby ponownie powiązać dane.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Wyrażenia wiązania danych kodowania HTML

Możesz to zrobić teraz kodowanie HTML wynik wyrażenia wiązania danych. Dodaj dwukropek (:) na końcu &lt;% # prefiks, który oznacza wyrażenia wiązania danych:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Sprawdzania poprawności dyskretnego kodu

Teraz można skonfigurować kontrolki wbudowanej modułu sprawdzania poprawności dyskretnego kodu JavaScript na potrzeby logiki weryfikacji po stronie klienta. To znacznie zmniejsza ilość kodu JavaScript renderowane w tekście w znaczniku strony i zmniejsza całkowity rozmiar strony. Dyskretny kod JavaScript dla kontrolek weryfikacji można skonfigurować w dowolnej z następujących sposobów:

- Globalnie, dodając następujące ustawienie, aby *&lt;appSettings&gt;* elementu w pliku Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalnie przez ustawienie statycznego *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* właściwości *UnobtrusiveValidationMode.WebForms* (zwykle w *aplikacji \_Start* metody w pliku Global.asax).
- Osobno dla strony, ustawiając nową *UnobtrusiveValidationMode* właściwość *strony* klasy *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aktualizacje HTML5

Niektóre udoskonalenia formularzy sieci Web formanty serwera, aby móc korzystać z nowych funkcji języków HTML5:

- *Tryb tekstowy* właściwość *TextBox* formant został zaktualizowany do obsługi nowych typów wejściowych HTML5, takie jak *e-mail*, *daty/godziny*, i itd.
- *FileUpload* sterowanie teraz obsługuje operacje przekazywania wielu plików z przeglądarki, które obsługują tę funkcję HTML5.
- Moduł sprawdzania poprawności steruje teraz obsługi sprawdzania poprawności HTML5 elementów wejściowych.
- Nowe elementy HTML5, które mają atrybuty, które reprezentują adresu URL teraz obsługuje runat = "server". Co w efekcie można używać konwencji platformy ASP.NET w ścieżki adresu URL, takie jak ~ operatora do reprezentowania katalog główny aplikacji (na przykład &lt;wideo runat = "server" src="~/myVideo.wmv" /&gt;).
- *UpdatePanel* formant został rozwiązany do obsługi pól wejściowych ogłaszania HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 w wersji Beta jest teraz dołączone do programu Visual Studio 11 Beta. ASP.NET MVC to architektura służąca do tworzenia aplikacji sieci Web w szerokim zakresie testować i łatwego w utrzymaniu dzięki wykorzystaniu wzorca Model-View-Controller (MVC). ASP.NET MVC 4 ułatwia tworzenie aplikacji dla urządzeń przenośnych sieci Web i zawiera interfejs API sieci Web platformy ASP.NET, który umożliwia tworzenie usług HTTP, które mają dostęp do dowolnego urządzenia. Aby uzyskać więcej informacji, zobacz [platformy ASP.NET MVC 4 — informacje o wersji](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

Nowe funkcje:

- Szablony witryn nowe i zaktualizowane.
- Dodawanie po stronie serwera i walidacji po stronie klienta przy użyciu *weryfikacji* pomocnika.
- Możliwość zarejestrowania skryptów przy użyciu Menedżera zasobów.
- Włączanie logowania do usług Facebook i innych lokacji za pomocą protokołu OAuth i OpenID.
- Dodawanie map, za pomocą *mapuje* pomocnika.
- Uruchamianie stron sieci Web aplikacji side-by-side.
- Renderowanie stron dla urządzeń przenośnych.

Aby uzyskać więcej informacji o tych funkcjach i przykłady kodu całej strony, zobacz [funkcji Top w wersji Beta programu Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Ta sekcja zawiera informacje na temat usprawnień do tworzenia aplikacji internetowych w wersji Beta programu Visual Web Developer 11 i Visual Studio 2012 w wersji Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projekt do udostępniania między Visual Studio 2010 i Visual Studio 2012 w wersji Release Candidate (tryb zgodności projektu)

Do programu Visual Studio 2012 w wersji Release Candidate otworzyć istniejący projekt w nowszej wersji programu Visual Studio uruchomiony Kreator konwersji. Uaktualnienie zawartości (zasobów) projektu i rozwiązania to wymuszone nowe formaty, które nie były zgodne z poprzednimi wersjami. Dlatego po konwersji można nie można otworzyć projektu w starszej wersji programu Visual Studio.

Wielu klientów ma Otrzymaliśmy informację, że to nie jest właściwe podejście. W programie Visual Studio 11 Beta jest teraz obsługiwana udostępnianie projektów i rozwiązań za pomocą programu Visual Studio 2010 z dodatkiem SP1. Oznacza to, że po otwarciu projektu 2010 w programie Visual Studio 2012 w wersji Release Candidate, nadal będzie można otworzyć projektu w Visual Studio 2010 SP1.

> [!NOTE]
> Kilka typów projektów nie mogą być współdzielone w Visual Studio 2010 z dodatkiem SP1 i Visual Studio 2012 w wersji Release Candidate. Obejmują one niektórych starszych projektów (np. projektów ASP.NET MVC 2) lub projektów do celów specjalnych (na przykład projektów instalacji).

Po otwarciu projektu sieci Web programu Visual Studio 2010 SP1 po raz pierwszy w programie Visual Studio 11 Beta następujące właściwości są dodawane do pliku projektu:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation i OldToolsVersion są używane przez proces, który uaktualnia plik projektu. Nie mają one wpływu na pracę z projektem w programie Visual Studio 2010.

VisualStudioVersion jest nową właściwość używane przez program MSBuild 4.5, który wskazuje wersję programu Visual Studio dla bieżącego projektu. Ponieważ ta właściwość nie istnieje w MSBuild 4.0 (wersja programu MSBuild, który używa programu Visual Studio 2010 z dodatkiem SP1), firma Microsoft wstawić wartość domyślną do pliku projektu.

Właściwość VSToolsPath jest używana określić poprawne .targets pliku do zaimportowania ze ścieżki, reprezentowane przez ustawienie MSBuildExtensionsPath32.

Istnieją również pewne zmiany związane z elementy importu. Te zmiany są wymagane w celu obsługi zgodność między obie wersje programu Visual Studio.

> [!NOTE]
> Jeśli projekt są udostępniane między Visual Studio 2010 z dodatkiem SP1 i Visual Studio 11 Beta na dwóch różnych komputerach, a projekt zawiera lokalnej bazy danych w aplikacji\_folderu danych należy się upewnić, że jest wersja programu SQL Server używany przez bazę danych zainstalowana na obu komputerach.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Zmiany konfiguracji w szablonach 4.5 witryny sieci Web platformy ASP.NET

Następujące zmiany zostały dokonane do domyślnego *Web.config* plików dla lokacji, które są tworzone przy użyciu szablonów sieci Web w programie Visual Studio 2012 w wersji Release Candidate:

- W `<httpRuntime>` elementu `encoderType` atrybutu jest równa domyślnie korzystają z typów elementu AntiXSS, które zostały dodane do platformy ASP.NET. Aby uzyskać więcej informacji, zobacz [biblioteki AntiXSS](#_Toc318097382).
- Również w `<httpRuntime>` elementu `requestValidationMode` "4.5" ma ustawioną wartość atrybutu. Oznacza to, że domyślnie Weryfikacja żądania jest skonfigurowany do używania odroczonego weryfikacji ("z opóźnieniem"). Aby uzyskać więcej informacji, zobacz [nowe żądanie weryfikacji programu ASP.NET](#_Toc318097379).
- `<modules>` Elementu `<system.webServer>` nie zawiera sekcji `runAllManagedModulesForAllRequests` atrybutu. (Wartość domyślna to false). Oznacza to, że jeśli używasz wersji usług IIS 7, który nie został zaktualizowany do wersji SP1 może być problemy z routingiem w nowej lokacji. Aby uzyskać więcej informacji, zobacz [macierzystą obsługę w usługach IIS 7 dla routingu platformy ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Te zmiany nie wpłyną na istniejące aplikacje. Jednak może być reprezentują one różnicy w zachowaniu istniejących witryn sieci Web i nowych witryn sieci Web utworzonej przez funkcję ASP.NET 4.5, za pomocą nowych szablonów.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Macierzysta obsługa w usługach IIS 7 dla routingu platformy ASP.NET

Nie jest zmiany do platformy ASP.NET jako takie, ale zmiany w szablonach dla nowych projektów witryny sieci Web, które mogą wpłynąć na możesz pracy wersję usług IIS 7, który miał nie zastosowano aktualizacji z dodatkiem SP1.

W programie ASP.NET można dodać następujące ustawienia konfiguracji dla aplikacji w celu obsługi routingu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Podczas **runAllManagedModulesForAllRequests** jest true, takie jak adres URL `http://mysite/myapp/home` przechodzi do platformy ASP.NET, nawet jeśli istnieje nie *.aspx*, *MVC*, lub podobne rozszerzenie na ADRES URL.

Sprawia, że aktualizacja, która została wprowadzona w usługach IIS 7 **runAllManagedModulesForAllRequests** ustawienie niepotrzebne i obsługuje natywnie proces routingu platformy ASP.NET. (Aby uzyskać informacje dotyczące aktualizacji, zobacz artykuł Microsoft Support [aktualizacja jest dostępna, że umożliwia niektórych obsługi usług IIS 7.0 lub 7.5 usług IIS do obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).)

Jeśli witryny sieci Web jest uruchomiona w usługach IIS 7, a jeśli program IIS został zaktualizowany, nie należy ustawić **runAllManagedModulesForAllRequests** na wartość true. W rzeczywistości ustawieniem dla niego wartość true nie jest zalecane, ponieważ jego zwiększa niepotrzebne obciążenie przetwarzaniem żądania. Gdy to ustawienie ma wartość true, wszystkie żądania, w tym przypadku *.htm*, *.jpg*, i inne pliki statyczne również przechodzą przez potok żądań ASP.NET.

Jeśli tworzysz nowy ASP.NET 4.5 witryny sieci Web przy użyciu szablonów, które znajdują się w programie Visual Studio 2012 RC, konfiguracja witryny sieci Web nie obejmuje **runAllManagedModulesForAllRequests** ustawienie. Oznacza to, że domyślnie to ustawienie ma wartość false.

Następnie należy uruchomić witryny sieci Web na Windows 7 bez zainstalowany dodatek SP1, usługi IIS 7 nie będzie zawierać wymagana aktualizacja. W konsekwencji routingu nie będą działać i zakończy się wystąpieniem błędów. Jeśli masz problem gdzie routingu nie działa, możesz wykonać albo następujące czynności:

- Zaktualizuj Windows 7 z dodatkiem SP1, która doda je do usług IIS 7.
- Zainstaluj aktualizację opisaną w artykule firmy Microsoft Support wymienionych powyżej.
- Ustaw **runAllManagedModulesForAllRequests** na wartość true w pliku Web.config tej witryny sieci Web. Należy pamiętać, że spowoduje to dodanie pewien narzut na żądania.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Edytor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Inteligentne zadań

W widoku Projekt złożonych właściwości formantów serwera często mają skojarzone kreatorów, aby ułatwić ich ustawiania i okien dialogowych. Na przykład umożliwia specjalne okno dialogowe Dodaj źródło danych w celu *Repeater* kontrolować lub dodawanie kolumn do *GridView* kontroli.

Jednak tego rodzaju pomocy interfejsu użytkownika dla złożonych właściwości nie był dostępny w widoku źródła. W związku z tym programu Visual Studio 11 wprowadza inteligentnego dla widoku źródła. Inteligentne zadania są oparte na kontekście skróty dla często używanych funkcji edytorów języka C# i Visual Basic.

Dla formantów formularzy sieci Web ASP.NET inteligentne zadania są wyświetlane w tagach serwera jako małych symbol gdy punkt wstawiania znajduje się wewnątrz elementu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Inteligentne zadań rozszerza się po kliknięciu symbolu lub naciśnij klawisze CTRL +. (kropka), podobnie jak w przypadku edytory kodu. Następnie wyświetla skróty, które są podobne do inteligentnego w widoku Projekt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Na przykład inteligentne zadań na poprzedniej ilustracji przedstawiono opcje zadania GridView. Jeśli wybierzesz Edytowanie kolumn, zostanie wyświetlone następujące okno dialogowe:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Wypełnianie zestawy okno dialogowe te same właściwości można ustawić w widoku Projekt. Po kliknięciu przycisku OK kodu znaczników kontrolki jest aktualizowany z nowymi ustawieniami:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Poczeka ARIA pomocy technicznej

Zapisywanie dostępny witryn sieci Web staje się coraz ważniejsze. [ARIA Poczeka dostępność standardowych](http://www.w3.org/WAI/intro/aria) definiuje, jak deweloperzy powinien zapisać dostępny witryn sieci Web. Ten standard jest teraz w pełni obsługiwane w programie Visual Studio.

Na przykład *roli* atrybut ma teraz pełną obsługą technologii IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Standardowa Poczeka ARIA wprowadza również atrybuty, które mają prefiks *aria -* umożliwiające dodawanie semantyki do dokumentu HTML5. Program Visual Studio obsługuje również w pełni tych *aria -* atrybuty:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png)![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nowe fragmenty HTML5

Aby umożliwić szybsze i prostsze do zapisywania często używanych znaczników języka HTML5, program Visual Studio zawiera wiele fragmentów. Przykładem jest wideo fragmentu kodu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Aby wywołać fragment kodu, naciśnij klawisz Tab dwa razy, po wybraniu elementu w technologii IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Daje to fragment kodu, który można dostosować.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Wyodrębnij do kontrolki użytkownika

W dużych stron sieci web może być dobry pomysł, aby przenieść poszczególnych elementów do kontrolki użytkownika. Ta forma Refaktoryzacja może pomóc zwiększyć czytelność strony i można uproszczenie strukturę strony.

Aby to ułatwić, podczas edycji stron formularzy sieci Web w widoku źródła, można teraz wybrać tekstu na stronie, kliknij go prawym przyciskiem myszy, a następnie wybierz Wyodrębnij do kontrolki użytkownika:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>Funkcja IntelliSense dla kodu nuggets w atrybutach

Visual Studio zawsze oferowała IntelliSense dla nuggets kodu po stronie serwera w dowolnej strony lub formant. Teraz Visual Studio zawiera funkcję IntelliSense dla kodu nuggets w atrybutach HTML, jak również.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Ułatwia to tworzenie wyrażenia wiązania danych:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Automatyczna zmiana nazwy pasujących tagów, po zmianie nazwy opening lub tag zamykający

W przypadku zmiany nazwy elementu HTML (na przykład zmienić *div* tag, aby być *nagłówka* tag), odpowiednich otwierających ani zamykających nawiasów tag zmienia się również w czasie rzeczywistym.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Dzięki temu można uniknąć tego błędu, w którym zapomnij zmienić tag zamykający lub niewłaściwy.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generowanie programów obsługi zdarzeń

Program Visual Studio zawiera teraz funkcje w widoku źródła, aby ułatwić pisanie programów obsługi zdarzeń i powiązać je ręcznie. Jeśli edytujesz nazwę zdarzenia w widoku źródła, IntelliSense wyświetla &lt;Utwórz nowe zdarzenie&gt;, co spowoduje utworzenie programu obsługi zdarzeń w kodzie strony, ma prawo podpis:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Domyślnie program obsługi zdarzeń użyje identyfikator formantu Nazwa metody obsługi zdarzeń:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Wynikowy programu obsługi zdarzeń będzie wyglądać następująco (w tym przypadku w języku C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Inteligentne wcięcie

Po naciśnięciu klawisza Enter, gdy są połączeni z pustego elementu HTML, edytorze umieści punkt wstawiania w odpowiednim miejscu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Jeśli użytkownik naciśnie klawisz Enter w tej lokalizacji, tag zamykający jest przenieść w dół i wcięcia pasuje do tagu początkowego. Również tworzone jest wcięcie punkt wstawiania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Zmniejsz automatyczne uzupełnianie instrukcji

Lista funkcji IntelliSense w Visual Studio teraz filtry oparte na typ tak, aby wyświetlał tylko odpowiednie opcje:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

Funkcja IntelliSense także filtry oparte na wielkość liter tytuł poszczególnych wyrazów na liście funkcji IntelliSense. Na przykład jeśli wpiszesz "dl", zarówno w przypadku listy dystrybucyjnej, jak i asp: DataList są wyświetlane:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Ta funkcja umożliwia szybsze uzyskanie uzupełnianie składni dla znanych elementów.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript — Edytor

Edytor kodu JavaScript w programie Visual Studio 2012 w wersji Release Candidate jest całkowicie nowych i znacznie poprawia środowisko pracy za pomocą języka JavaScript w programie Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Zwijanie kodu

Regiony konspektu teraz są tworzone automatycznie dla wszystkich funkcji, umożliwiając Zwiń części pliku, które nie są odpowiednie do Twojej bieżący fokus.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Parowanie nawiasów klamrowych

Po umieszczeniu kursora na otwarcie lub zamykającego nawiasu klamrowego Edytor wyróżnia dopasowania.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Przejdź do definicji

Przejdź do definicji polecenia pozwala przejść do źródła dla funkcji lub zmiennej.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Obsługa ECMAScript5

Edytor obsługuje nową składnię i interfejsów API w ECMAScript5, najnowszej wersji standardowa opisujący języka JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>Funkcja IntelliSense w modelu DOM

Ulepszono funkcję IntelliSense dla interfejsów API modelu DOM, z obsługą wielu nowych interfejsów API języków HTML5, w tym *querySelector*, DOM magazynu dla wielu dokumentów, wiadomości, a *kanwy*. DOM IntelliSense teraz jest uzyskiwana przez jednego prostego pliku JavaScript, a nie zgodnie z definicją biblioteki typu natywnego. Dzięki temu można łatwo rozszerzać lub zastępować.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC sygnatury przeciążenia

Szczegółowe komentarze IntelliSense mogą być deklarowane teraz dla oddzielnych przeciążenia funkcji języka JavaScript za pomocą nowego *&lt;podpisu&gt;* elementu, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Niejawne odwołania

Pliki JavaScript można teraz dodać do listy centralnej, która zostanie niejawnie umieszczona na liście plików, danego języka JavaScript pliku lub bloku odwołań, co oznacza Przedstawiamy IntelliSense jego zawartość. Na przykład można dodać pliki jQuery centralną listę plików, a otrzymasz technologii IntelliSense dla funkcji jQuery w dowolnym bloku kodu JavaScript w pliku, czy odwołania jawnie (przy użyciu / / / &lt;odwołania /&gt;) czy nie.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Edytor CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Zmniejsz automatyczne uzupełnianie instrukcji

Lista IntelliSense CSS teraz filtrów na na podstawie właściwości CSS i wartości obsługiwanych przez wybrany schemat.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

Technologia IntelliSense obsługuje również tytuł przypadku wyszukiwania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Wcięcie hierarchiczne

Edytor CSS używa wcięcia do wyświetlania hierarchiczne reguły, co daje Przegląd sposobu logicznie organizacji zasady kaskadowych. W poniższym przykładzie #list selektora jest elementem podrzędnym kaskadowe listy i w związku z tym będą wcięte.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Poniższy przykład przedstawia bardziej złożone dziedziczenie:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Wcięcie reguły jest określana na podstawie reguł nadrzędnej. Wcięcie hierarchiczna jest domyślnie włączona, ale można ją wyłączyć, okno dialogowe Opcje (narzędzia, opcje na pasku menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS uzyskuje dostęp do pomocy technicznej

Analiza setek plików CSS rzeczywistych pokazuje, że hakerskie CSS są bardzo popularne, i Visual Studio obsługuje teraz te najczęściej używanych. Ta obsługa obejmuje technologii IntelliSense i weryfikacji gwiazdka (\*) i podkreślenia (\_) hakerskie właściwości:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Selektor typowe hakerskie są również obsługiwane utrzymywanie wcięcie hierarchiczna nawet wtedy, gdy są one stosowane. Hack typowe selektor, używany do obiektu docelowego programu Internet Explorer 7 jest dołączana selektora z  *\*: pierwszy podrzędny + html*. Przy użyciu tej reguły będzie utrzymywać wcięcie hierarchiczna:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schematy określonego dostawcy (- moz-- webkit)

CSS3 wprowadza wiele właściwości, które zostały wdrożone w różnych przeglądarkach w różnym czasie. Deweloperom tworzenia kodu dla przeglądarki zostanie wcześniej wymuszone za pomocą składni specyficzne dla dostawcy. Te właściwości specyficzne dla przeglądarki, teraz znajdują się w technologii IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Komentowania i Trwa usuwanie komentarza do pomocy technicznej

Można teraz dodać komentarz i usuń znaczniki komentarza reguły CSS przy użyciu tych samych skrótów, używanych w edytorze kodu (Ctrl + K, C, komentarz i Ctrl + K, umożliwia usuń znaczniki komentarza).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selektor kolorów

W poprzednich wersjach programu Visual Studio technologii IntelliSense dla atrybuty dotyczące kolorów składa się z listy rozwijanej wartości kolor nazwany. Ta lista została zastąpiona selektor kolorów w pełni funkcjonalne.

Po wprowadzeniu wartości koloru, selektor kolorów jest wyświetlany automatycznie i wyświetla listę wcześniej używanych kolorów, następuje z domyślnej palety kolorów. Można wybrać kolor przy użyciu myszy lub klawiatury.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Listy można rozszerzyć do próbnika kolorów ukończone. Selektor umożliwia sterowanie kanał alfa konwertując automatycznie dowolny kolor do RGBA podczas przesuwania suwaka nieprzezroczystość:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmenty kodu

Fragmenty kodu w edytorze CSS umożliwiają łatwiejsze i szybsze tworzenie stylów obsługiwania wielu przeglądarek. Wiele właściwości CSS3, które wymagają ustawienia specyficzne dla przeglądarki teraz zostały wycofane do fragmentów kodu.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Fragmenty kodu CSS wspierać zaawansowane scenariusze, (na przykład zapytaniami multimediów CSS3), wpisując na symbol (@), który wyświetla listę funkcji IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Po wybraniu @media wartość i naciśnij klawisz Tab, Edytor CSS wstawia poniższy fragment kodu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Podobnie jak w przypadku fragmentów kodu, możesz utworzyć własne fragmenty kodu CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Niestandardowe regionów

O nazwie regiony kodu, które są już dostępne w edytorze kodu, są teraz dostępne do edycji CSS. Dzięki temu można łatwo grupie style powiązane bloki.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Gdy region jest zwinięty Wyświetla nazwę regionu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspektor strony

Narzędzie Page Inspector to narzędzie, które renderuje stronę sieci web (HTML, formularze sieci Web, ASP.NET MVC lub strony sieci Web) w środowisku IDE programu Visual Studio i pozwala na badanie zarówno kod źródłowy, jak i dane wyjściowe. Stron ASP.NET narzędzie Page Inspector umożliwia określenie, które kodu po stronie serwera tworzył kod znaczników HTML, który jest renderowany w przeglądarce.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Aby uzyskać więcej informacji na temat narzędzia Page Inspector zobacz następujące samouczki:

- Za pomocą narzędzia Page Inspector w [platformy ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Za pomocą narzędzia Page Inspector w [formularzy sieci Web ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publikowanie

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profile publikowania

W programie Visual Studio 2010 publikowanie informacji dla projektów aplikacji sieci Web nie są przechowywane w systemie kontroli wersji i nie jest przeznaczona do udostępniania innym osobom. W programie Visual Studio 2012 w wersji Release Candidate został zmieniony format profilu publikowania. Dokonano artefaktu zespołu i jest teraz można łatwo korzystać z kompilacji w oparciu o program MSBuild. Informacje o konfiguracji kompilacji jest w oknie dialogowym publikowania, dzięki czemu można łatwo przełączać konfiguracje kompilacji przed opublikowaniem.

Publikowanie profilów są przechowywane w folderze PublishProfiles. Zależy od lokalizacji folderu używasz jakiego języka programowania:

- C#: Properties\PublishProfiles
- Visual Basic: Moje Project\PublishProfiles

Każdy profil jest plikiem programu MSBuild. Podczas publikowania, ten plik jest importowany do projektu programu MSBuild pliku. W programie Visual Studio 2010, jeśli chcesz wprowadzić zmiany w procesie publikowania lub pakietu, należy umieścić dostosowania w pliku o nazwie **ProjectName**. wpp.targets. Ta funkcja jest nadal obsługiwana, ale możesz teraz umieścić dostosowania w profilu publikowania, sam. W ten sposób dostosowania będą używane tylko w przypadku tego profilu.

Możesz teraz korzystać publikować profile z programu MSBuild. Podczas kompilowania projektu, aby to zrobić, użyj następującego polecenia:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Wartość project.csproj to ścieżka projektu, a Nazwa_profilu jest nazwą profilu publikowania. Alternatywnie, zamiast przekazywać nazwę profilu *PublishProfile* właściwości, możesz przekazać pełną ścieżkę profilu publikowania.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Wstępnej kompilacji platformy ASP.NET i scalania

W przypadku projektów aplikacji sieci Web Visual Studio 2012 w wersji Release Candidate dodaje opcję na stronie właściwości Pakuj/Publikuj w sieci Web, umożliwiający wstępnej kompilacji i scalanie zawartości witryny sieci podczas publikowania lub pakietu projektu. Aby wyświetlić te opcje, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz polecenie Właściwości, a następnie wybierz na stronie właściwości Pakuj/Publikuj w sieci Web. Na poniższej ilustracji przedstawiono Precompile tej aplikacji, przed opublikowaniem opcji.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Gdy ta opcja jest zaznaczona, Visual Studio wstępnie kompiluje aplikacji zawsze wtedy, gdy opublikujesz lub pakietu aplikacji sieci web. Jeśli chcesz kontrolować, jak witryna jest wstępnie skompilowany lub jak zestawy są scalane, kliknij przycisk Zaawansowane, aby skonfigurować te opcje.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Domyślny serwer sieci web dla testowania projektów sieci web w programie Visual Studio jest teraz usługi IIS Express. Serwera wdrożeniowego programu Visual Studio nadal jest opcję lokalnego serwera internetowego podczas projektowania, ale usługi IIS Express jest teraz serwerem zalecane. Środowisko korzystania z usług IIS Express w Visual Studio 11 Beta jest bardzo podobny do korzystania z niego w programie Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może ulec znacznym zmianom przed ostatecznym wydaniem komercyjnym oprogramowania opisanych tutaj materiałach.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok firmy Microsoft Corporation na problemy omówione w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI W TYM DOKUMENCIE.

Zgodnie z obowiązującymi przepisami prawa autorskiego jest odpowiedzialny za użytkownika. Bez ograniczenia, praw autorskich, żadna część tego dokumentu może być odtworzyć, przechowywane w lub wprowadzone do systemu wyszukiwania lub przekazywanych w jakiejkolwiek formie lub za pomocą jakichkolwiek środków (elektronicznych, mechanicznych, fotokopiowania, nagrywania lub w przeciwnym razie) lub w jakimkolwiek celu, bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może być patentów, wniosków patentowych, znaków towarowych, praw autorskich lub innych praw własności intelektualnej dotyczące elementów zawartych w tym dokumencie. Wyraźnie określone w umowach licencyjnych firmy Microsoft, udostępnienie tego dokumentu nie mu żadnych licencji dotyczących tych patentów, znaków towarowych, praw autorskich i innej własności intelektualnej.

Jeśli nie określono inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, adres e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2012 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Firma Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami przez ich właścicieli.
