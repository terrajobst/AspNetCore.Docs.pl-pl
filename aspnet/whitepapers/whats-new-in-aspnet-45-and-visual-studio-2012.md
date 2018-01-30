---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: What's New in ASP.NET 4.5 i programu Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Ten dokument zawiera opis nowych funkcji i ulepszeń wprowadzonych w programie ASP.NET 4.5. Opisano również ulepszeniami do tworzenia aplikacji sieci web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>What's New in ASP.NET 4.5 i programu Visual Studio 2012
====================
> Ten dokument zawiera opis nowych funkcji i ulepszeń wprowadzonych w programie ASP.NET 4.5. Omówiono także ulepszeniami do tworzenia aplikacji sieci web w programie Visual Studio 2012. Ten dokument został pierwotnie opublikowane 29 lutego 2012.


- [Środowisko uruchomieniowe platformy ASP.NET Core i platforma](#_Toc318097372)

    - [Asynchronicznie odczytuje i zapisuje żądań i odpowiedzi HTTP](#_Toc318097373)
    - [Ulepszenia obsługi HttpRequest](#_Toc318097374)
    - [Asynchronicznie opróżnianie odpowiedzi](#_Toc318097375)
    - [Obsługa *await* i *zadań*— na podstawie asynchronicznego moduły i obsługi](#_Toc318097376)
    - [Asynchroniczne moduły HTTP](#_Toc318097377)
    - [Asynchroniczne programów obsługi HTTP](#_Toc318097378)
    - [Nowe funkcje sprawdzania poprawności żądania programu ASP.NET](#_Toc318097379)
    - [Odroczone sprawdzanie poprawności żądań ("opóźnieniem")](#_Toc318097380)
    - [Obsługa niezweryfikowanych żądań](#_Toc318097381)
    - [Biblioteka elementu AntiXSS](#_Toc318097382)
    - [Obsługę protokołu WebSockets](#_Toc318097383)
    - [Tworzenie pakietów i minifikacja](#_Toc318097384)
    - [Ulepszenia wydajności usługi hostingu sieci Web](#_Toc_perf)

        - [Czynniki wydajności](#_Toc_perf_1)
        - [Wymagania dotyczące nowych funkcji wydajności](#_Toc_perf_2)
        - [Udostępnianie zestawów wspólnych](#_Toc_perf_3)
        - [W celu przyspieszenia uruchamiania przy użyciu wielordzeniowych kompilacja JIT](#_Toc_perf_4)
        - [Dostrajanie wyrzucanie elementów bezużytecznych w celu optymalizacji pamięci](#_Toc_perf_5)
        - [Odczyt z wyprzedzeniem dla aplikacji sieci web](#_Toc_perf_6)
- [Formularze sieci Web ASP.NET](#_Toc318097385)

    - [Silnie typizowane kontrolki danych](#_Toc318097386)
    - [Wiązanie modelu](#_Toc318097387)

        - [Wybór danych](#_Toc318097388)
        - [Dostawców wartości](#_Toc318097389)
        - [Filtrowanie według wartości z formantu](#_Toc318097390)
    - [Wyrażenia wiązania danych kodowania HTML](#_Toc318097391)
    - [Sprawdzania poprawności dyskretnego kodu](#_Toc318097392)
    - [Aktualizacje HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Strony ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projekt udostępnianie między Visual Studio 2010 i Visual Studio 2012 Release Candidate (zgodność z projektu)](#project-compatibility)
    - [Zmiany konfiguracji w szablonach 4.5 witryny sieci Web ASP.NET](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Macierzysty mechanizm obsługi w usługach IIS 7 routingu platformy ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML Editor](#_Toc318097397)

        - [Inteligentne zadań](#_Toc318097398)
        - [Obsługa programu ARIA WAI](#_Toc318097399)
        - [Nowe HTML5 wstawki kodu programu](#_Toc318097400)
        - [Wyodrębnij do kontrolki użytkownika](#_Toc318097401)
        - [IntelliSense dla nuggets kodu w atrybutach](#_Toc318097402)
        - [Zmiana nazwy automatycznego dopasowania tagu po zmianie nazwy otwierający lub zamykający tag](#_Toc318097403)
        - [Generowanie obsługi zdarzeń](#_Toc318097404)
        - [Inteligentne wcięcie](#_Toc318097405)
        - [Zmniejsz automatyczne uzupełnianie](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Zwijanie kodu](#_Toc318097408)
        - [Parowanie nawiasów klamrowych](#_Toc318097409)
        - [Przejdź do definicji](#_Toc318097410)
        - [Obsługa ECMAScript5](#_Toc318097411)
        - [IntelliSense modelu DOM](#_Toc318097412)
        - [VSDOC sygnatury przeciążenia](#_Toc318097413)
        - [Niejawne odwołania](#_Toc318097414)
    - [Edytor CSS](#_Toc318097415)

        - [Zmniejsz automatyczne uzupełnianie](#_Toc318097416)
        - [Hierarchiczna wcięcia.](#_Toc318097417)
        - [CSS uzyskuje dostęp do pomocy technicznej](#_Toc318097418)
        - [Schematy określonego dostawcy (- moz-- webkit)](#_Toc318097419)
        - [Obsługa komentowania i Trwa usuwanie komentarza](#_Toc318097420)
        - [Selektor kolorów](#_Toc318097421)
        - [Wstawki kodu](#_Toc318097422)
        - [Niestandardowe regionów](#_Toc318097423)
    - [Narzędzie Page Inspector](#_Toc318097424)
    - [Publikowanie](#_Toc318097425)

        - [Profilów publikowania](#_Toc318097426)
        - [Wstępnej kompilacji platformy ASP.NET i dokonać scalania](#_Toc318097427)
- [Usługi IIS Express](#_Toc318097428)
- [Zrzeczenie odpowiedzialności](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Środowisko uruchomieniowe platformy ASP.NET Core i platforma

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchronicznie odczytuje i zapisuje żądań i odpowiedzi HTTP

ASP.NET 4 wprowadzono możliwość odczytu jednostki żądania HTTP jako przy użyciu strumienia *HttpRequest.GetBufferlessInputStream* metody. Ta metoda podać przesyłania strumieniowego dostęp do jednostki żądania. Jednak wykonania synchronicznie, który blokowana wątek na czas trwania żądania.

Program ASP.NET 4.5 obsługuje możliwości odczytu strumieni asynchronicznie w jednostce żądania HTTP i możliwość opróżnić asynchronicznie. Program ASP.NET 4.5 daje również możliwość podwójnego buforowania jednostki żądania HTTP, który zapewnia łatwiejsze integrację z podrzędne programów obsługi HTTP, takie jak programy obsługi strony aspx i ASP.NET MVC kontrolerów.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Ulepszenia obsługi HttpRequest

Odwołanie strumienia zwrócone przez funkcję ASP.NET 4.5 z *HttpRequest.GetBufferlessInputStream* obsługuje synchroniczne i asynchroniczne metody odczytu. *Strumienia* obiektu zwróconego z *GetBufferlessInputStream* teraz implementuje BeginRead i EndRead można wywołać metody. Asynchroniczną *strumienia* metody umożliwiają asynchronicznie odczytu jednostka żądania w fragmentów, gdy bieżący wątek między każdej iteracji pętli odczytu asynchronicznego zwalnia ASP.NET.

Program ASP.NET 4.5 dodano również metody pomocnika do odczytywania jednostka żądania w sposób buforowanego: *metody HttpRequest.GetBufferedInputStream*. To przeciążenie nowe działa jak *GetBufferlessInputStream*, obsługa odczyty synchroniczne i asynchroniczne. Jednakże, ponieważ jest odczytywana, *GetBufferedInputStream* również kopiuje bajtów jednostki do bufory wewnętrzne ASP.NET, umożliwiając moduły podrzędne i obsługi nadal dostęp do jednostki żądania. Na przykład, jeśli niektóre od kodu w potoku ma już odczytu przy użyciu jednostek żądania *GetBufferedInputStream*, można nadal używać *HttpRequest.Form* lub *HttpRequest.Files*. Dzięki temu można wykonać przetwarzania asynchronicznego na żądanie (na przykład, przesyłania strumieniowego przekazywania dużych pliku do bazy danych), ale strony .aspx nadal wykonywania i kontrolery MVC ASP.NET później.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchronicznie opróżnianie odpowiedzi

Wysyłania odpowiedzi do klienta HTTP może zająć wiele czasu, gdy klient jest daleko lub z połączeniem o niskiej przepustowości. Zwykle ASP.NET buforuje bajtów odpowiedzi tworzonych przez aplikację. ASP.NET następnie wykonuje operację wysyłania pojedynczego naliczone buforów na końcu procesu przetwarzania żądania.

Jeśli buforowanej odpowiedzi jest duży (na przykład, przesyłania strumieniowego dużego pliku do klienta), należy okresowo wywoływać *HttpResponse.Flush* wysyłanie buforowanych wyników do klienta i zachować pamięć pod kontrolą. Jednak ponieważ *opróżnić* jest wywołania synchronicznego wywoływania wielokrotnie powtarzane *opróżnić* nadal zużywa wątek na czas trwania żądania potencjalnie długotrwałe.

Program ASP.NET 4.5 dodaje obsługę dla wykonywania opróżnia asynchronicznie za pomocą *BeginFlush* i *EndFlush* metody *HttpResponse* klasy. Za pomocą następujących metod, można utworzyć modułów asynchronicznych i obsługi asynchroniczne, które przyrostowo wysyłanie danych do klienta bez angażowania wątków systemu operacyjnego. Między *BeginFlush* i *EndFlush* wywołań, ASP.NET zwalnia bieżącego wątku. Pozwala to znacznie ograniczyć łączna liczba aktywnych wątków, które są wymagane w celu zapewnienia obsługi HTTP długotrwałe pliki do pobrania.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Obsługa *await* i *zadań* — na podstawie asynchronicznego moduły i obsługi

.NET Framework 4 wprowadzono asynchroniczne koncepcji programowania nazywane *zadań*. Zadania są reprezentowane przez *zadań* typu i powiązanych typów z *System.Threading.Tasks* przestrzeni nazw. .NET Framework 4.5 opiera się na to kompilatora ulepszeń, które sprawiają, że praca z *zadań* prosty obiektów. W programie .NET Framework 4.5 kompilatory obsługi dwóch nowych słów kluczowych: *await* i *async*. *Await* — słowo kluczowe jest syntaktycznych skrócona forma wskazujący, który fragment kodu asynchronicznie ma oczekiwać na innych części kodu. *Async* — słowo kluczowe reprezentuje wskazówkę, która służy do oznaczania metod jako opartego na zadaniach asynchronicznej metody.

Kombinacja *await*, *async*i *zadań* obiektu ułatwia do pisania kodu asynchronicznego w programie .NET 4.5. Program ASP.NET 4.5 obsługuje te uproszczenia z nowych interfejsów API, które umożliwiają zapisu asynchronicznego moduły HTTP i asynchronicznych za pomocą nowe ulepszenia kompilatora programów obsługi HTTP.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Asynchroniczne moduły HTTP

Załóżmy, że chcesz wykonać zadanie asynchroniczne w metodę, która zwraca *zadań* obiektu. Poniższy przykładowy kod definiuje metodę asynchroniczną, która wykonuje asynchroniczne wywołanie do pobrania na stronie głównej firmy Microsoft. Zwróć uwagę na *async* — słowo kluczowe w podpisie metody i *await* wywołanie *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

To wszystko, trzeba napisać — programu .NET Framework będzie automatycznie obsługiwał odwijanie stosu wywołań podczas oczekiwania na pobieranie, aby zakończyć, a także automatycznie przywracanie stos wywołań, po ukończeniu pobierania.

Teraz załóżmy, że chcesz użyć tej metody asynchronicznej asynchronicznego modułu HTTP programu ASP.NET. Program ASP.NET 4.5 zawiera metody pomocnika (*EventHandlerTaskAsyncHelper*), a nowy typ delegata (*TaskEventHandler*) można zintegrować opartego na zadaniach asynchronicznej metody za pomocą starszego model programowania asynchroniczne udostępnianych przez potoku HTTP programu ASP.NET. W tym przykładzie pokazano sposób:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Asynchroniczne programów obsługi HTTP

W tradycyjnym podejściu zapisu asynchronicznego obsługi w programie ASP.NET jest zaimplementowanie *IHttpAsyncHandler* interfejsu. Wprowadza ASP.NET 4.5 *HttpTaskAsyncHandler* asynchroniczne typ podstawowy służący może pochodzić od, co pozwala na znacznie łatwiejsze do pisania obsług asynchronicznego.

*HttpTaskAsyncHandler* typ jest abstrakcyjny i wymagane jest zastąpienie *ProcessRequestAsync* metody. Wewnętrznie ASP.NET zajmuje się integrowanie zwracany podpis ( *zadań* obiektu) z *ProcessRequestAsync* z starsze modelu programowania asynchronicznego używane przez potok ASP.NET.

W poniższym przykładzie pokazano, jak używasz *zadań* i *await* jako część wdrożenia asynchronicznego programu obsługi HTTP:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nowe funkcje sprawdzania poprawności żądania programu ASP.NET

Domyślnie program ASP.NET sprawdza poprawność żądania — sprawdza, czy żądania, aby wyszukać znaczników lub skrypt pola, nagłówki plików cookie i tak dalej. W przypadku wykrycia żadnego ASP.NET zgłasza wyjątek. Stanowi to pierwszy wiersz obrony przed potencjalnych ataków skryptów między witrynami.

Program ASP.NET 4.5 ułatwia selektywnie odczytać żądania niezweryfikowanych danych. Program ASP.NET 4.5 integruje się również popularnych biblioteki elementu AntiXSS, który był wcześniej biblioteki zewnętrznej.

Deweloperzy mają często zadawane dla możliwość selektywnie wyłączyć weryfikację żądań dla swoich aplikacji. Na przykład aplikacji to oprogramowanie forum, można umożliwić użytkownikom przesyłanie postach formacie HTML i komentarze, ale nadal upewnij się, że sprawdzania poprawności żądania wszystkich pozostałych.

Program ASP.NET 4.5 wprowadza dwie funkcje, które można selektywnie pracować przy użyciu niezweryfikowane dane wejściowe: odroczone sprawdzanie poprawności żądań ("opóźnieniem") i dostęp do niezweryfikowanych żądania danych.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Odroczone sprawdzanie poprawności żądań ("opóźnieniem")

W programie ASP.NET 4.5 domyślnie wszystkie dane żądania podlega weryfikacji żądań. Można jednak skonfigurować aplikacji, które mają być odroczone sprawdzanie poprawności żądań do momentu rzeczywistości dostęp do danych żądania. (To jest czasami nazywane weryfikacji żądań opóźnieniem, na podstawie warunków, takich jak opóźnionego ładowania niektórych scenariuszy danych.) Można skonfigurować aplikację do użycia odroczone sprawdzanie poprawności w pliku Web.config przez ustawienie *requestValidationMode* atrybutu 4.5 w *httpRUntime* element, jak w poniższym przykładzie:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Gdy tryb weryfikacji żądania ustawiono 4.5, sprawdzanie poprawności żądań zostanie wywołany tylko dla wartości określonego żądania i tylko wtedy, gdy kod uzyskuje dostęp do tej wartości. Na przykład, jeśli kod pobiera wartość Request.Form["forum\_post"], weryfikacja żądania jest wywoływane tylko dla tego elementu w kolekcji formularza. Żaden z elementów w *formularza* kolekcji są weryfikowane. W poprzednich wersjach programu ASP.NET Weryfikacja żądania zostało wyzwolone dla kolekcji request całego dostępie dowolny element w kolekcji. Nowe zachowanie ułatwia przyjrzeć się różne dane żądania bez wyzwalania sprawdzanie poprawności żądań na inne składniki inną aplikację.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Obsługa niezweryfikowanych żądań

Weryfikacja żądania odroczonego wyłącznie nie rozwiąże problem selektywnego pomijanie sprawdzania poprawności żądania. Wywołanie Request.Form["forum\_post"] nadal wyzwalaczy żądania sprawdzania poprawności dla tej wartości określonego żądania. Jednak można uzyskać dostępu do tego pola bez wyzwalania weryfikacji, ponieważ chcesz umożliwić znacznik, w tym polu.

W tym celu ASP.NET 4.5 obsługują teraz mają dostęp do danych żądania. Program ASP.NET 4.5 zawiera nową *Unvalidated* właściwości kolekcji w *HttpRequest* klasy. Ta kolekcja zapewnia dostęp do wszystkich typowych wartości danych żądania tak samo, jak *formularza*, *QueryString*, *plików cookie*, i *adres Url*.

W przykładzie forum, aby można było odczytać żądania niezweryfikowanych danych, należy najpierw skonfigurować aplikację do używania trybu weryfikacji żądania:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Następnie można użyć *HttpRequest.Unvalidated* właściwości można odczytać wartości niezweryfikowanych formularza:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Zabezpieczenia — *używać niezweryfikowanych żądanie danych z rozwagą!* Program ASP.NET 4.5 dodać właściwości niezweryfikowanych żądania i kolekcji, aby ułatwić dostęp do bardzo szczegółowej żądania niezweryfikowanych danych. Jednak należy nadal wykonać niestandardowego sprawdzania poprawności na dane żądanie raw, aby upewnić się, niebezpiecznych tekstu nie są odtwarzane użytkownikom.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteka elementu AntiXSS

Z powodu popularne Microsoft AntiXSS Library ASP.NET 4.5 zawiera podstawowe kodowania procedury z tej biblioteki w wersji 4.0.

Procedury kodowania są implementowane przez *AntiXssEncoder* typu w nowym *System.Web.Security.AntiXss* przestrzeni nazw. Można użyć *AntiXssEncoder* typu bezpośrednio przez wywołanie żadnego kodowania metod statycznych, które są wdrażane w typie. Najłatwiejszy dla przy użyciu nowej procedury anti-XSS jest jednak skonfigurować aplikację ASP.NET do korzystania *AntiXssEncoder* klasy domyślnie. Aby to zrobić, Dodaj następujący atrybut w pliku Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Gdy *encoderType* atrybutu jest skonfigurowany do używania *AntiXssEncoder* typu, wszystkie dane wyjściowe kodowanie automatycznie w programie ASP.NET używa nowe procedury kodowania.

Są to części zewnętrznej biblioteki elementu AntiXSS, które zostały włączone do ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, i *HtmlAttributeEncode*
- *XmlAttributeEncode* i *XmlEncode*
- *UrlEncode* i *UrlPathEncode* (nowy)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Obsługę protokołu WebSockets

Protokół Websocket jest oparta na standardach protokół sieciowy, który definiuje metodę ustanawiania bezpiecznego, w czasie rzeczywistym dwukierunkowego komunikacji między klientem serwerem za pośrednictwem protokołu HTTP. Firma Microsoft podjęła współpracę z treściami standardów IETF i W3C z myślą o określeniu protokołu. Protokół Websocket jest obsługiwany przez klienta (nie tylko przeglądarki) z firmą Microsoft, najlepiej zainwestować znaczne zasoby obsługi protokołu WebSockets zarówno klient, jak i przenośnych systemów operacyjnych.

Protokół Websocket ułatwia utworzyć transferów danych długotrwałe między klientem serwerem. Na przykład pisania aplikacji czatu jest znacznie łatwiejsze, ponieważ może nawiązać połączenie długotrwałe true między klientem serwerem. Nie masz odwołać się do rozwiązania, takie jak okresowo sondowanie lub HTTP long sondowania, aby symulować gniazda.

Program ASP.NET 4.5 i IIS 8 obsługują niskiego poziomu Websocket, umożliwiają deweloperom ASP.NET przy użyciu zarządzanych interfejsów API do asynchronicznego odczytywania i zapisywania zarówno ciąg, jak i dane binarne dla obiektu Websocket. Dla platformy ASP.NET 4.5, to nowa *System.Web.WebSockets* przestrzeni nazw, która zawiera typy do pracy z protokołu WebSockets.

Klient przeglądarki ustanawia połączenie Websocket, tworząc DOM *WebSocket* obiektu, który wskazuje na adres URL w aplikacji ASP.NET, jak w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Możesz utworzyć Websocket punktów końcowych w aplikacji ASP.NET przy użyciu dowolnego rodzaju moduł ani Obsługa. W poprzednim przykładzie .ashx użyto pliku, ponieważ pliki ashx są szybko utworzyć programu obsługi.

Zgodnie z protokołem Websocket aplikacji ASP.NET akceptuje żądania Websocket klienta wskazując żądania powinny zostać uaktualnione z żądania HTTP GET na żądanie protokołu WebSockets. Oto przykład:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest* metoda przyjmuje delegata funkcji, ponieważ ASP.NET cofa bieżącego żądania HTTP, a następnie przekazuje sterowanie do delegata funkcji. Koncepcyjnie takie podejście jest podobny do sposobu korzystania *System.Threading.Thread*, gdzie został zdefiniowany delegata uruchomienia wątku w tle, które praca jest wykonywana.

Po zakończeniu ASP.NET i klient pomyślnie uzgadniania Websocket, ASP.NET wymaga pełnomocnika i uruchamiania aplikacji Websocket. Poniższy przykład kodu pokazuje aplikacji echo proste, która używa wbudowanych funkcji Websocket w programie ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Obsługa w programie .NET 4.5 dla *await* — słowo kluczowe i oparty na zadaniach operacji asynchronicznych jest fizyczną nadające się do pisania aplikacji Websocket. Przykład kodu pokazuje całkowicie asynchronicznie uruchamia żądanie Websocket w programie ASP.NET. Aplikacja asynchronicznie oczekuje na komunikat do wysłania w kliencie przez wywołanie metody *await gniazda. Metody ReceiveAsync*. Podobnie, wysyłanie komunikatów asynchronicznych do klienta przez wywołanie metody *await gniazda. SendAsync*.

W przeglądarce, aplikacja odbiera komunikaty Websocket za pośrednictwem *onmessage* funkcji. Aby wysłać wiadomość z przeglądarki, należy wywołać *wysyłania* metody *WebSocket* typu modelu DOM, tak jak w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

W przyszłości firma Microsoft może aktualizacje do tej funkcji, czy abstrakcyjny optymalizacji niektórych niskiego poziomu kodowania czyli wymaganych w tej wersji protokołu WebSockets aplikacji.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Tworzenie pakietów umożliwia łączenie pojedynczych plików JavaScript i CSS w pakiet, który może być traktowana jak pojedynczy plik. Minimalizowanie pozwala zapisać pliki JavaScript i CSS przez usunięcie odstępy i znaki, które nie są wymagane. Te funkcje działają z formularzy sieci Web, ASP.NET MVC i stron sieci Web.

Pakiety są tworzone przy użyciu klasy pakietu lub jednej z jej klas podrzędnych ScriptBundle i StyleBundle. Po skonfigurowaniu wystąpienia pakietu, po prostu dodając go do globalnego wystąpienia BundleCollection ma zostać udostępnione na przychodzące żądania pakietu. W szablonach domyślnych konfiguracji pakietu odbywa się w pliku BundleConfig. Konfiguracja domyślna tworzy pakiety dla wszystkich rdzeni skryptów i plików css używany przez Szablony.

Pakiety odwołuje się od w obrębie widoków przy użyciu jednej z kilku metod pomocniczych możliwości. Aby zapewnić obsługę renderowania różny kod znaczników dla pakietu w przypadku debugowania, a tryb wersji, klas ScriptBundle i StyleBundle ma metodę pomocnika renderowania. W trybie debugowania, renderowanie generuje kod znaczników dla każdego zasobu w pakiecie. W trybie wersji renderowania wygeneruje element jednego znacznika dla cały pakiet. Przełączanie między debug i release tryb można osiągnąć, modyfikując atrybut debugowania elementu kompilacji w pliku web.config, jak pokazano poniżej:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Ponadto Włączanie lub wyłączanie optymalizacji można ustawić bezpośrednio za pomocą właściwości BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Gdy pliki są powiązane, są one najpierw sortowane alfabetycznie (są one wyświetlane w sposób **Eksploratora rozwiązań**). Następnie są zorganizowane tak, aby znane bibliotek i ich rozszerzenia niestandardowe (np. jQuery, MooTools i Dojo) są ładowane najpierw. Na przykład będzie końcowego kolejność paczki folderze skryptów, jak pokazano powyżej:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Pliki CSS również sortowana alfabetycznie i następnie zreorganizować tak, aby reset.css i normalize.css występować przed dowolnego innego pliku. Końcowe sortowania paczki folderu stylów pokazanym powyżej będzie to:

1. Reset.css
2. content.css
3. Forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Ulepszenia wydajności usługi hostingu sieci Web

.NET Framework 4.5 i Windows 8 przedstawiono funkcje, które mogą pomóc w osiągnięciu podniesienie znaczących wydajności dla obciążeń serwera sieci web. Obejmuje to zmniejszenie (do 35%) w obu czas uruchamiania i zużycie pamięci w sieci Web, witrynami, które używają programu ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Czynniki wydajności

Najlepiej, jeśli wszystkie witryny sieci Web powinien być aktywny i w pamięci, aby zapewnić szybkie odpowiedzi dla następnego żądania, gdy nastąpi. Czynniki, które mogą wpłynąć na czas odpowiedzi lokacji obejmują:

- Czas trwania dla lokacji w celu ponownego uruchomienia puli aplikacji jest odtwarzana. Jest to czas potrzebny do uruchomienia procesu serwera sieci web dla witryny, gdy zestawy lokacji nie są już w pamięci. (Zestawów platformy są nadal w pamięci, ponieważ są one używane przez inne lokacje). Taka sytuacja jest określany jako "zimnych lokacji i uruchamiania ciepłych framework" lub po prostu "chłodni uruchamianie witryny."
- Ilość pamięci zajmuje lokacji. Warunki dla tego są "zużycie pamięci dla poszczególnych witryn" lub "cofnięto zestaw roboczy".

Nowe ulepszenia wydajności skupić się na oba te czynniki.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Wymagania dotyczące nowych funkcji wydajności

Wymagania dotyczące nowych funkcji można podzielić na następujące kategorie:

- Ulepszenia działających w .NET Framework 4.
- Ulepszenia, które wymagają programu .NET Framework 4.5, ale można uruchomić w dowolnej wersji systemu Windows.
- Ulepszenia, które są dostępne tylko z systemem Windows 8 program .NET Framework 4.5.

Zwiększona wydajność przy czym każdy poziom zabezpieczeń, które można włączyć.

Niektóre z ulepszeń .NET Framework 4.5 zalet ogólniejszej funkcji wydajności, które dotyczą również inne scenariusze.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Udostępnianie zestawów wspólnych

**Wymaganie**: program .NET Framework 4 i Visual Studio 11 Developer Preview zestawu SDK

Różnych lokacji na serwerze często używają tego samego zestawy pomocy (na przykład zestawy z starter kit lub przykładowej aplikacji). Każda lokacja ma własną kopię tych zestawów w jego katalogu Bin. Mimo że kod obiektu zestawy jest taki sam, jest fizycznie oddzielona zestawy, więc każdego zestawu, nie można odczytać oddzielnie podczas uruchamiania zimnych lokacji i oddzielnie przechowywany w pamięci.

Nowe funkcje interning rozwiązuje to nieefektywne podejście i zmniejsza wymagania dotyczące pamięci RAM, jak i obciążenia czasu. Interning umożliwia Windows zachować pojedynczej kopii każdego zestawu, w systemie plików, a pojedyncze zestawy w folderach Bin lokacji zostają zastąpione łącza symbolicznego do pojedynczej kopii. Jeśli indywidualnej witryny musi różnych wersji zestawu, łącza symbolicznego zostało zastąpione przez nową wersję zestawu i dotyczy tylko w tej lokacji.

Udostępnianie zestawów przy użyciu łącza symbolicznego wymaga nowe narzędzie o nazwie aspnet\_intern.exe, która pozwala na tworzenie i zarządzanie nimi w magazynie zestawów interned. Jest podana jako część programu Visual Studio 11 Developer Preview SDK. (Jednak będą działać w systemie, który zawiera tylko programu .NET Framework 4 zainstalowany, zakładając, że zainstalowano najnowszą [aktualizacji](https://support.microsoft.com/kb/2468871).)

Aby upewnić się, wszystkie zestawy kwalifikujących się zostały interned, należy uruchomić aspnet\_intern.exe okresowo (na przykład raz w tygodniu jako zaplanowane zadanie). Typowy sposób użycia jest następująca:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Aby wyświetlić wszystkie opcje, uruchom narzędzie bez argumentów.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>W celu przyspieszenia uruchamiania przy użyciu wielordzeniowych kompilacja JIT

**Wymaganie**: program .NET Framework 4.5

Do uruchomienia zimnych lokacji nie tylko zestawy muszą być odczytane z dysku, ale lokacji musi być skompilowany JIT. W przypadku złożonych witryny to dodanie znaczne opóźnienia. Nowe techniki ogólnego przeznaczenia w programie .NET Framework 4.5 zmniejsza te opóźnienia przez rozłożenie dostępne rdzenie kompilacji JIT. Dzieje się tak dużo i jak to możliwe, korzystając z informacji zebranych podczas poprzedniego uruchamia witryny. Ta funkcja implementowane przez [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) metody.

JIT — kompilowanie przy użyciu wiele rdzeni jest domyślnie włączona w programie ASP.NET, dzięki czemu nie trzeba wykonywać żadnych czynności, aby móc korzystać z tej funkcji. Jeśli chcesz wyłączyć tę funkcję, należy wprowadzić następujące ustawienie w pliku Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Dostrajanie wyrzucanie elementów bezużytecznych w celu optymalizacji pamięci

**Wymaganie**: program .NET Framework 4.5

Po uruchomieniu lokacji, jego użycie stercie modułu zbierającego elementy bezużyteczne (GC) może być ważnym czynnikiem umożliwiającym jego zużycie pamięci. Podobnie jak wszelkie modułu zbierającego elementy bezużyteczne .NET Framework GC sprawia, że wady i zalety między czasu procesora CPU (częstotliwości i znaczenie kolekcje) i zmniejszenie zużycia pamięci (dodatkowego miejsca używanego dla nowych, zwolnionych lub zwolnij możliwością obiektów). W poprzednich wersjach, firma Microsoft umieściła wskazówki dotyczące sposobu konfigurowania wykazu Globalnego, aby osiągnąć odpowiednią równowagę między (na przykład, zobacz [ASP.NET 2.0/3.5 Hosting konfiguracji udostępnionej](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

.NET Framework 4.5, a wiele ustawień autonomiczny, ustawienia konfiguracji zdefiniowane przez obciążenie jest dostępne, umożliwia wszystkie wcześniej zalecanych ustawień GC, a także dostrajania nowych, który zapewnia wyższą wydajność dla poszczególnych witryn Zestaw roboczy.

Aby włączyć dostrajanie pamięci GC, Dodaj następujące ustawienie do pliku Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Jeśli znasz poprzedniej wskazówki dotyczące zmiany do konfigurację aspnet.config, należy pamiętać, że to ustawienie zastępuje stare ustawienia — na przykład jest niepotrzebna można ustawić gcserver — gcconcurrent —, itp. Nie trzeba usunąć stare ustawienia.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Odczyt z wyprzedzeniem dla aplikacji sieci web

**Wymaganie**: .NET Framework 4.5 uruchomiony w systemie Windows 8

Dla kilku wersji systemu Windows zostało uwzględnione to technologia, znany jako [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) obniżającą koszty odczytu z dysku uruchamiania aplikacji. Ponieważ zimnych uruchamiania problem głównie dla aplikacji klienckich, ta technologia nie zostało uwzględnione w systemie Windows Server, który zawiera tylko te składniki, które są niezbędne do serwera. Odczyt z wyprzedzeniem jest teraz dostępna w najnowszej wersji systemu Windows Server, gdzie może zoptymalizować uruchamiania poszczególnych witryn sieci Web.

W systemie Windows Server prefetcher nie jest domyślnie włączona. Aby włączyć i skonfigurować prefetcher usługi hostingu sieci web o wysokiej gęstości, uruchom następujący zestaw poleceń w wierszu polecenia:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Następnie Aby zintegrować prefetcher z aplikacji programu ASP.NET, dodaj następującą wartość do pliku Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formularze sieci Web ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Formanty danych silnie Typizowanego

W programie ASP.NET 4.5 formularzy sieci Web zawiera niektóre ulepszenia do pracy z danymi. Poprawa pierwszy jest formantów jednoznacznie danych. Dla formantów formularzy sieci Web w poprzednich wersjach programu ASP.NET, wyświetlania wartości z danymi przy użyciu *Eval* i wyrażenia wiązania danych:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Aby powiązanie dwukierunkowe danych, można użyć *powiązać*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

W czasie wykonywania tych wywołań umożliwia odbicia odczytano wartość określonego elementu członkowskiego, a następnie Wyświetl wynik w znaczniku. Takie podejście ułatwia do tworzenia powiązań danych z dowolnego, unshaped danych.

Jednak wyrażenia wiązania danych podobny do tego nie obsługują funkcje, takie jak IntelliSense dla nazwy elementów członkowskich, nawigacji (np. Przejdź do definicji) lub kompilacji sprawdzanie te nazwy.

Aby rozwiązać ten problem, ASP.NET 4.5 dodaje możliwość deklarować typu danych powiązanej z formantu danych. Można to zrobić przy użyciu nowego *ItemType* właściwości. Po ustawieniu tej właściwości w zakresie wyrażenia wiązania danych są dostępne dwie nowe zmienne typu: *elementu* i *metody BindItem*. Ponieważ zmienne są silnie typizowane, otrzymasz pełne korzyści środowisko programistyczne Visual Studio.


Dwukierunkowe wyrażenia wiązania danych, można użyć *metody BindItem* zmienną:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Większość formantów w ramach formularzy sieci Web ASP.NET, które obsługuje powiązanie danych zostały zaktualizowane do obsługi *ItemType* właściwości.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Wiązanie modelu

Wiązanie modelu rozszerza wiązania z danymi w formantach formularzy sieci Web ASP.NET do pracy z dostępem do danych fokus kodu. Zawiera pojęcia dotyczące z *ObjectDataSource* kontroli i z wiązania modelu w programie ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Wybór danych

Aby skonfigurować formantu danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu *SelectMethod* właściwość na nazwę metody w kodzie strony. Formant danych wywołuje metodę we właściwym czasie w cyklu życia strony i automatycznie wiąże zwróconych danych. Nie istnieje potrzeba aby jawnie wywołać *DataBind* metody.

W poniższym przykładzie *GridView* kontroli jest skonfigurowana do używania metodę o nazwie *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Możesz utworzyć *GetCategories* w kodzie strony. Prostych operacji select, metoda wymaga parametrów i powinien zwrócić *IEnumerable* lub *IQueryable* obiektu. Jeśli nowy *ItemType* właściwość jest ustawiona (co pozwoli silnie typizowane wyrażenia wiązania danych, jak wyjaśniono w [silnie Typizowanej formantów danych](#_Toc318097386) wcześniej), ogólny wersje tych interfejsów powinny być zwracane — *IEnumerable&lt;T&gt;*  lub *IQueryable&lt;T&gt;*, z *T* parametru dopasowania typ *ItemType* właściwości (na przykład *IQueryable&lt;kategorii&gt;*).

W poniższym przykładzie pokazano kod *GetCategories* metody. W tym przykładzie używa modelu Entity Framework Code First z przykładowej bazy danych Northwind. Kod upewnia się, że zapytanie zwraca szczegółowe informacje o pokrewnych produktów dla każdej kategorii poprzez *Include* metody. (Dzięki temu *TemplateField* elementu w znaczniku przedstawia liczbę produktów w każdej kategorii bez konieczności [n + 1 Zaznacz](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Po uruchomieniu strony, *GridView* kontrolować wywołania *GetCategories* — metoda automatycznie i renderuje zwrócone dane przy użyciu pól skonfigurowane:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Ponieważ metoda select zwraca *IQueryable* obiektu, *GridView* kontroli dalsze można manipulować zapytanie przed jej wykonanie. Na przykład *GridView* formantu można dodać zapytania wyrażenia sortowania i stronicowania, aby zwracana *IQueryable* obiekt przed jego wykonaniem, tak aby te operacje są wykonywane przez podstawową Dostawcy LINQ. W takim przypadku Entity Framework zagwarantować, że te operacje są wykonywane w bazie danych.

W poniższym przykładzie przedstawiono *GridView* kontroli zmodyfikowane w celu umożliwienia sortowania i stronicowania:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Teraz po uruchomieniu na stronie, formantu można upewnij się, że jest wyświetlana tylko bieżąca strona i porządkowania według wybranych kolumn:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Aby filtrować dane zwrotne, parametry muszą być dodane do metody select. Parametry te zostaną wypełnione przez powiązanie modelu w czasie wykonywania i można je zmodyfikować zapytanie przed zwróceniem danych.

Załóżmy na przykład, że mają mieć możliwość produktów filtr użytkowników wpisując słowo kluczowe w ciągu zapytania. Możesz dodać parametr do metody i zaktualizuj kod, aby użyć wartości parametru:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Ten kod zawiera *gdzie* wyrażenia, jeśli wartość jest dostarczany *— słowo kluczowe* , a następnie zwraca wyniki zapytania.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Dostawców wartości

Poprzedni przykład nie jest określonym o tym, gdzie wartość *— słowo kluczowe* parametr został pochodzące z. Aby wskazać, te informacje, można użyć atrybutu parametru. Na przykład można użyć *QueryStringAttribute* klasy, która znajduje się w *System.Web.ModelBinding* przestrzeni nazw:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

To powoduje, że wiązanie modelu próby powiązać wartości z ciągu zapytania do *— słowo kluczowe* parametru w czasie wykonywania. (To może obejmować wykonywania konwersji typu, mimo że w takim przypadku nie.) Jeśli nie można podać wartości, a typem wartości null, jest zwracany wyjątek.

Źródła wartości te metody są określane jako dostawców wartości i wskazujący, który dostawca wartości do użycia atrybuty parametru są określane jako atrybuty dostawcy wartości. Formularze sieci Web zawiera dostawców wartości i atrybuty odpowiednie dla wszystkich typowych źródeł danych wejściowych użytkownika w aplikacji formularzy sieci Web, na przykład ciąg zapytania, plików cookie, wartości formularza, kontrolek, stan widoku, stan sesji i właściwości profilu. Można również napisać dostawców wartości niestandardowych.

Domyślnie nazwa parametru jest używany jako klucz znaleźć wartości w dostawcy wartości kolekcji. W tym przykładzie kodu będzie szukać wartość ciągu kwerendy o nazwie — słowo kluczowe (na przykład ~ / default.aspx?keyword=chef). Można określić klucz niestandardowy, przekazując go jako argument atrybutu parametru. Na przykład aby użyć wartości zmiennej ciągu zapytania o nazwie q, można w tym:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

W przypadku tej metody w kodzie strony, użytkownicy mogą filtrować wyniki przez przekazanie słowa kluczowego przy użyciu ciągu zapytania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Wiele zadań, które w przeciwnym razie trzeba kodować ręcznie wykonuje wiązanie modelu: odczytywanie wartości, sprawdzanie wartości null próby przekonwertować go do odpowiedniego typu, sprawdzanie, czy konwersja powiodła się i na koniec przy użyciu wartości w Zapytanie. Model powiązanie wyniki w znacznie mniej kodu i możliwość ponownego użycia funkcji w całej aplikacji.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrowanie według wartości z formantu

Załóżmy, że chcesz rozszerzenia tego przykładu, aby umożliwić użytkownikowi, wybierz wartość z listy rozwijanej. Dodaj następujące listy rozwijanej znaczników i skonfiguruj go można pobrać danych z innego przy użyciu metody *SelectMethod* właściwości:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Zazwyczaj należy również dodać *elementu EmptyDataTemplate* elementu *GridView* , aby formantu zostanie wyświetlony komunikat, jeśli znaleziono żadnych zgodnych produktów:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

W kodzie strony Dodaj nowe wybrać metodę do listy rozwijanej:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Na koniec zaktualizuj *GetProducts* wybierz metodę, aby wykonać nowy parametr, który zawiera identyfikator wybranej kategorii z listy rozwijanej:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Teraz po uruchomieniu na stronie, użytkownicy mogą wybrać kategorię z listy rozwijanej i *GridView* formant jest automatycznie ponownie powiązane, aby pokazać to odfiltrowane dane. Jest to możliwe, ponieważ powiązanie modelu śledzi wartości parametrów dla metody select i wykrywa, czy wszystkie wartości parametru został zmieniony po odświeżeniu strony. Jeśli tak, tworzenia powiązania modelu wymusza kontroli skojarzone dane ponownie powiązać dane.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Wyrażenia wiązania danych kodowania HTML

Możesz teraz kodowanie HTML wynik wyrażenia wiązania danych. Dodaj średnikiem (;) na końcu &lt;% # prefiks, który oznacza wyrażenia wiązania danych:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Sprawdzania poprawności dyskretnego kodu

Można teraz skonfigurować kontrolki wbudowany moduł sprawdzania poprawności dyskretnego kodu JavaScript na potrzeby logikę weryfikacji po stronie klienta. To znacznie zmniejsza ilość kodu JavaScript renderowane w tekście w znaczniku strony i zmniejsza całkowity rozmiar strony. Dyskretny kod JavaScript dla formantów modułu sprawdzania poprawności można skonfigurować w dowolnej z następujących sposobów:

- Globalnie, dodając następujące ustawienie do  *&lt;appSettings&gt;*  elementu w pliku Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalnie, ustawiając statycznych *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* właściwości *UnobtrusiveValidationMode.WebForms* (zwykle w *aplikacji \_Start* metody w pliku Global.asax).
- Osobno dla strony przez ustawienie nowego *właściwość unobtrusivevalidationmode obiektu* właściwość *strony* klasy do *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aktualizacje HTML5

Niektóre ulepszenia do formularzy sieci Web kontrolki serwera mógł korzystać z nowych funkcji HTML5:

- *Tryb tekstowy* właściwość *pole tekstowe* kontrolki została zaktualizowana do obsługi nowych typów wejściowych HTML5, takich jak *e-mail*, *datetime*, i itd.
- *Przekazywaniem plików* sterowanie teraz obsługuje przekazywania wielu plików z przeglądarek, które obsługują tę funkcję HTML5.
- Moduł sprawdzania poprawności steruje teraz obsługi sprawdzania poprawności HTML5 elementów wejściowych.
- Nowych elementów HTML5, które mają atrybuty, które reprezentują adresu URL teraz obsługuje runat = "server". W związku z tym można użyć programu ASP.NET Konwencji w ścieżki adresu URL, tak samo, jak ~ operatora, aby reprezentować katalog główny aplikacji (na przykład &lt;wideo runat = "server" src="~/myVideo.wmv" /&gt;).
- *UpdatePanel* formant został rozwiązany do obsługi publikowanie HTML5 pól wejściowych.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 w wersji Beta jest teraz dołączone do programu Visual Studio 11 Beta. ASP.NET MVC to struktura służąca do tworzenia aplikacji sieci Web wysokiej zakresie testować i obsługiwać dzięki wykorzystaniu wzorca Model-widok-kontroler (MVC). ASP.NET MVC 4 ułatwia tworzenie aplikacji dla sieci Web urządzeń przenośnych i zawiera interfejsu API sieci Web ASP.NET, która ułatwia tworzenie usług HTTP, które może nawiązać połączenie z dowolnego urządzenia. Aby uzyskać więcej informacji, zobacz [informacje o wersji platformy ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

Następujące nowe funkcje:

- Szablony stron nowe i zaktualizowane.
- Dodawanie po stronie serwera i weryfikacji po stronie klienta przy użyciu *weryfikacji* pomocnika.
- Możliwość zarejestrowania skryptów za pomocą Menedżera zasobów.
- Włączanie logowania z usługi Facebook i innych lokacji za pomocą protokołu OAuth i OpenID.
- Dodawanie mapy, przy użyciu *mapy* pomocnika.
- Uruchomiona stron sieci Web aplikacji side-by-side.
- Renderowanie stron dla urządzeń przenośnych.

Aby uzyskać więcej informacji o tych funkcjach i przykładów kodu całej strony, zobacz [funkcji Top w wersji Beta 2 stron sieci Web](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Ta sekcja zawiera informacje o usprawnieniach do tworzenia aplikacji sieci web w wersji Beta programu Visual Web Developer 11 i Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projekt udostępnianie między Visual Studio 2010 i Visual Studio 2012 Release Candidate (zgodność z projektu)

Do programu Visual Studio 2012 w wersji Release Candidate otwierania istniejącego projektu w nowszej wersji programu Visual Studio uruchomiony Kreator konwersji. Uaktualnienie zawartości (zasobów) projektu i rozwiązania to wymuszone nowych formatów, które nie były zgodne z poprzednimi wersjami. W związku z tym po konwersji musisz nie można otworzyć projektu w starszej wersji programu Visual Studio.

Wielu klientów zwracali naszą uwagę, że nie był podejście. W programie Visual Studio 11 Beta obsługujemy teraz udostępniania projektów i rozwiązań w programie Visual Studio 2010 z dodatkiem SP1. Oznacza to, że po otwarciu projektu 2010 w programie Visual Studio 2012 Release Candidate nadal będzie można otworzyć projektu w Visual Studio 2010 z dodatkiem SP1.

> [!NOTE]
> Kilka typów projektów nie mogą być współdzielone w Visual Studio 2010 z dodatkiem SP1 i programu Visual Studio 2012 Release Candidate. Obejmują one niektóre starsze projekty (np. projekty programu ASP.NET MVC 2) lub projektów do celów specjalne (takie jak projekty Instalatora).

Po otwarciu projektu sieci Web programu Visual Studio 2010 z dodatkiem SP1 po raz pierwszy w programie Visual Studio 11 Beta następujące właściwości są dodawane do pliku projektu:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation i OldToolsVersion są używane przez proces uaktualnienia pliku projektu. Że nie wpływają na temat pracy z projektu w programie Visual Studio 2010.

VisualStudioVersion jest nową właściwość używane przez program MSBuild 4.5, która wskazuje wersję programu Visual Studio dla bieżącego projektu. Ponieważ ta właściwość nie istnieje w programie MSBuild 4.0 (wersji programu MSBuild, który używa programu Visual Studio 2010 z dodatkiem SP1), używamy wartość domyślną do pliku projektu.

Właściwość VSToolsPath jest używana do określenia .targets poprawny plik do importu ze ścieżki reprezentowany przez ustawienie MSBuildExtensionsPath32.

Istnieją pewne zmiany związane z elementy importu. Te zmiany są wymagane w celu zapewnienia obsługi zgodność obie wersje programu Visual Studio.

> [!NOTE]
> Jeśli projekt jest udostępniana między Visual Studio 2010 z dodatkiem SP1 i programu Visual Studio 11 Beta na dwóch różnych komputerach, a jeśli projekt zawiera lokalnej bazy danych w aplikacji\_folderu danych, należy się upewnić, że jest wersja programu SQL Server używany przez bazę danych zainstalowana na obu komputerach.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Zmiany konfiguracji w szablonach 4.5 witryny sieci Web ASP.NET

Następujące zmiany zostały wprowadzone do domyślnej *Web.config* plików dla lokacji, które są tworzone przy użyciu szablonów sieci Web w programie Visual Studio 2012 Release Candidate:

- W `<httpRuntime>` elementu `encoderType` atrybut ma teraz wartość domyślnie używać typów elementu AntiXSS, które zostały dodane do programu ASP.NET. Aby uzyskać więcej informacji, zobacz [elementu AntiXSS biblioteki](#_Toc318097382).
- Również w `<httpRuntime>` elementu `requestValidationMode` atrybut jest ustawiony jako "4.5". Oznacza to, że domyślnie Weryfikacja żądania jest skonfigurowany do używania odroczonego sprawdzania poprawności ("opóźnieniem"). Aby uzyskać więcej informacji, zobacz [nowego żądania sprawdzania poprawności programu ASP.NET](#_Toc318097379).
- `<modules>` Elementu `<system.webServer>` nie zawiera sekcji `runAllManagedModulesForAllRequests` atrybutu. (Jego wartość domyślna to false). Oznacza to, że jeśli używasz wersji IIS 7, który nie został jeszcze zaktualizowany do wersji SP1 może mieć problemy z routingiem w nowej lokacji. Aby uzyskać więcej informacji, zobacz [macierzystą obsługę w usługach IIS 7 routingu platformy ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Te zmiany nie wpłyną na istniejące aplikacje. Jednak może reprezentują różnice w zachowaniu między istniejących witryn sieci Web oraz nowych witryn sieci Web utworzonej przez funkcję ASP.NET 4.5 przy użyciu nowych szablonów.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Macierzysty mechanizm obsługi w usługach IIS 7 routingu platformy ASP.NET

Nie jest jako takie zmiany do programu ASP.NET, ale zmiany w szablonach dla nowych projektów witryny sieci Web, które mogą wpłynąć na możesz pracy wersji IIS 7, który nie miał zastosowanej aktualizacji z dodatkiem SP1.

W programie ASP.NET można dodać następujące ustawienia konfiguracji do aplikacji w celu zapewnienia obsługi routingu:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Gdy **runAllManagedModulesForAllRequests** jest PRAWDA, takie jak adres URL `http://mysite/myapp/home` przechodzi do platformy ASP.NET, nawet jeśli jest nie *.aspx*, *MVC*, lub podobne rozszerzenia na ADRES URL.

Sprawia, że aktualizacja została wprowadzona w usługach IIS 7 **runAllManagedModulesForAllRequests** ustawienie niepotrzebnych i obsługuje natywnie proces routingu platformy ASP.NET. (Aby uzyskać informacje o aktualizacji, zobacz artykuł Microsoft Support [aktualizacja jest dostępna, że umożliwia niektórych obsługi usług IIS 7.0 lub 7.5 usług IIS do obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).)

Jeśli działa witryny sieci Web w usługach IIS 7 i jeśli usługi IIS zostały zaktualizowane, nie trzeba ustawić **runAllManagedModulesForAllRequests** na wartość true. W rzeczywistości ustawieniem dla niego wartość true nie jest zalecane, ponieważ dodaje przetwarzania niepotrzebnych na żądanie. Jeśli to ustawienie ma wartość true, wszystkie żądania, w tym w zakresie *.htm*, *.jpg*, i inne pliki statyczne, również przejść przez potok żądań ASP.NET.

W przypadku utworzenia nowej witryny internetowej ASP.NET 4.5 przy użyciu szablonów, które znajdują się w programie Visual Studio 2012 RC, nie ma konfiguracji witryny sieci Web **runAllManagedModulesForAllRequests** ustawienie. Oznacza to, że ustawienie domyślnie ma wartość false.

Następnie uruchom witrynę sieci Web w systemie Windows 7 bez dodatku SP1 jest zainstalowana, usługi IIS 7 nie będzie zawierał wymaganej aktualizacji. W konsekwencji routingu nie będzie działać i występuje błąd. Jeśli masz problem, gdzie routingu nie działa, możesz to zrobić albo następujące:

- Aktualizacji systemu Windows 7 z dodatkiem SP1, który doda aktualizację do wersji IIS 7.
- Zainstaluj aktualizację opisaną w artykule firmy Microsoft Support wymienionych powyżej.
- Ustaw **runAllManagedModulesForAllRequests** na wartość true w pliku Web.config tej witryny sieci Web. Należy pamiętać, że pewne nadmiarowe obciążenie zostaną dodane do żądania.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Edytor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Inteligentne zadań

W widoku Projekt właściwości złożonych kontrolek serwera często skojarzony okien dialogowych i kreatorów, aby ułatwić ich. Na przykład można specjalne, okno dialogowe Dodawanie źródła danych do *elementu powtarzanego* kontroli lub dodać kolumny *GridView* formantu.

Ten rodzaj pomocy interfejsu użytkownika dla właściwości złożonych nie został jednak dostępne w widoku źródła. W związku z tym Visual Studio 11 wprowadza panelu inteligentnego dla widoku źródła. Inteligentne zadania są obsługujący kontekst klawiaturowe dla często używane funkcje w edytorach C# i Visual Basic.

Dla formantów formularzy sieci Web ASP.NET, inteligentne zadania są wyświetlane w tagach serwera jako symbolu małych gdy punkt wstawiania znajduje się wewnątrz elementu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Inteligentne zadań rozszerza się po kliknięciu symbolu lub naciśnij klawisze CTRL +. (kropka), tak jak w przypadku edytory kodu. Wyświetla skróty, które są podobne do panelu inteligentnego w widoku Projekt.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Na przykład inteligentne zadań na poprzedniej ilustracji przedstawiono opcje zadania w widoku GridView. Jeśli wybierzesz Edytowanie kolumn, zostanie wyświetlone następujące okno dialogowe:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Wypełnianie zestawów — okno dialogowe takie same właściwości można ustawić w widoku Projekt. Po kliknięciu przycisku OK znaczników dla formantu jest aktualizowany przy użyciu nowych ustawień:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Obsługa programu ARIA WAI

Pisanie dostępny witryn sieci Web staje się coraz bardziej ważne. [Ułatwień dostępu programu ARIA WAI standardowe](http://www.w3.org/WAI/intro/aria) definiuje sposób deweloperzy należy zapisać dostępny witryn sieci Web. Ten standard jest teraz w pełni obsługiwana w programie Visual Studio.

Na przykład *roli* atrybut ma teraz pełną IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Standard programu ARIA WAI wprowadza również atrybuty, które mają przedrostek *programu aria -* umożliwiające dodawanie semantyki do dokumentu HTML5. Program Visual Studio także w pełni obsługuje te *programu aria -* atrybuty:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png)![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nowe HTML5 wstawki kodu programu

Aby szybsze i łatwiejsze do zapisywania często używanych znaczników HTML5, Visual Studio zawiera szereg fragmentów. Przykładem jest fragment wideo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Aby wywołać fragment kodu, naciśnij klawisz Tab, dwa razy, po wybraniu elementu w IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Daje to fragment kodu, który można dostosować.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Wyodrębnij do kontrolki użytkownika

W dużych stron sieci web może być dobrym pomysłem przenoszenia pojedynczych do kontrolki użytkownika. Ta forma refaktoryzacji może pomóc zwiększyć czytelność strony i uprościć strukturę strony.

Aby ułatwić, podczas edytowania stron formularzy sieci Web w widoku źródła, można teraz wybrać tekstu na stronie, kliknij go prawym przyciskiem myszy, a następnie wybierz Wyodrębnij do kontrolki użytkownika:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense dla nuggets kodu w atrybutach

Visual Studio zawsze udostępnił IntelliSense dla nuggets kod po stronie serwera w dowolnej strony lub kontrolki. Visual Studio zawiera teraz IntelliSense dla nuggets kodu oraz atrybutów HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Ułatwia to tworzenie wyrażenia wiązania danych:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Zmiana nazwy automatycznego dopasowania tagu po zmianie nazwy otwierający lub zamykający tag

W przypadku zmiany nazwy elementu HTML (na przykład zmienić *div* tag, aby być *nagłówka* tagu), odpowiednich otwarcie lub tagu zamykającego zmieniają się także w czasie rzeczywistym.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Dzięki temu można uniknąć tego błędu, gdy zapomnisz zmienić tagu zamykającego lub niewłaściwy.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generowanie obsługi zdarzeń

Visual Studio teraz obejmuje funkcje w widoku źródła, aby zapisać procedury obsługi zdarzeń i powiązać je ręcznie. Jeśli edytujesz nazwę zdarzenia w widoku źródła, IntelliSense wyświetla &lt;utworzyć nowe zdarzenie&gt;, co spowoduje utworzenie program obsługi zdarzeń w kodzie strony który zawiera odpowiedniego podpisu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Domyślnie program obsługi zdarzeń użyje identyfikator formantu nazwą metody obsługi zdarzeń:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Wynikowa programu obsługi zdarzeń będzie wyglądać następująco (w tym przypadku w języku C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Inteligentne wcięcie

Po naciśnięciu klawisza Enter znajduje się wewnątrz pustego elementu HTML, Edytor będzie umieść punkt wstawiania w odpowiednim miejscu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Po naciśnięciu klawisza Enter w tej lokalizacji, tag zamykający jest przenieść w dół i wcięcia pasuje do tagu początkowego. Również tworzone jest wcięcie punkt wstawiania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Zmniejsz automatyczne uzupełnianie

Lista IntelliSense w Visual Studio teraz filtry oparte na typ, aby wyświetla tylko odpowiednie opcje:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense również filtry oparte na tytuł w przypadku poszczególnych wyrazów na liście IntelliSense. Na przykład jeśli wpiszesz "dl" zarówno dl i asp: DataList są wyświetlane:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Ta funkcja umożliwia szybsze uzyskanie uzupełniania dla znanych elementów.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript — Edytor

Edytora kodu JavaScript w programie Visual Studio 2012 Release Candidate jest całkowicie nowa i znacznie skraca doświadczenia w pracy z JavaScript w programie Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Zwijanie kodu

Regiony zwijania teraz są tworzone automatycznie dla wszystkich funkcji, umożliwiając zwijanie części pliku, które nie są odpowiednie do bieżącego fokus.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Parowanie nawiasów klamrowych

Po umieszczeniu kursora na otwierający lub zamykający nawias klamrowy w edytorze wyróżnia dopasowania.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Przejdź do definicji

Przejdź do definicji — polecenie umożliwia przejście do źródła dla funkcji lub zmienna.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Obsługa ECMAScript5

Edytor obsługuje nowej składni i interfejsów API w ECMAScript5, najnowszej wersji standard, który opisuje języka JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense modelu DOM

Udoskonalono IntelliSense dla interfejsów API modelu DOM z obsługą wielu nowych interfejsów API HTML5 w tym *querySelector*, magazynu modelu DOM, do obsługi komunikatów, wielu dokumentów i *kanwy*. DOM IntelliSense obecnie jest wymuszany przez pojedynczy plik JavaScript proste, a nie przez definicję biblioteki typu macierzystego. Ułatwia to rozszerzać lub zastępować.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC sygnatury przeciążenia

Szczegółowe komentarze IntelliSense teraz mogą być deklarowane dla oddzielnych przeciążeń funkcji JavaScript przy użyciu nowej  *&lt;podpisu&gt;*  element, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Niejawne odwołania

Pliki JavaScript można teraz dodać do listy centralnej, która zostanie niejawnie uwzględniona na liście plików czy danego języka JavaScript pliku lub bloku odwołań, co oznacza otrzymają IntelliSense jego zawartość. Na przykład możesz dodać pliki jQuery do centralnej listę plików i otrzymają IntelliSense funkcje dostępne w bloku kodu JavaScript pliku, czy odwołania jawnie (przy użyciu lokalizacje &lt;odwołania /&gt;) czy nie.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Edytor CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Zmniejsz automatyczne uzupełnianie

Lista IntelliSense dla filtrów teraz CSS na podstawie właściwości CSS i wartości obsługiwany przez wybrany schemat.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense obsługuje również tytuł przypadków wyszukiwania:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hierarchiczna wcięcie

Edytor CSS używa wcięcia do wyświetlania hierarchiczna reguły, który zawiera omówienie sposobu logicznie organizacji zasad kaskadowych. W poniższym przykładzie #list selektora jest elementem podrzędnym kaskadowych listy i tworzone jest wcięcie w związku z tym.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

W poniższym przykładzie przedstawiono bardziej złożonych dziedziczenia:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Wcięcie reguły jest określany przez zasady jej nadrzędnej. Hierarchiczna wcięcia jest domyślnie włączona, ale można ją wyłączyć, okno dialogowe Opcje (narzędzia, opcje na pasku menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS uzyskuje dostęp do pomocy technicznej

Analizy setki pliki CSS rzeczywistych pokazuje, że CSS hacks są bardzo często używane, i teraz program Visual Studio obsługuje te najczęściej używana. Ta obsługa obejmuje IntelliSense i sprawdzanie poprawności gwiazdy (\*) i znak podkreślenia (\_) hacks właściwości:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Typowy selektora hacks są również obsługiwane utrzymywanie hierarchiczna wcięcia nawet wtedy, gdy są one stosowane. Hack typowe selektora, używany do docelowej programu Internet Explorer 7 jest dołączana selektora z  *\*: pierwszy podrzędny + html*. Przy użyciu tej reguły będzie odpowiadać za utrzymanie hierarchiczna wcięcie:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schematy określonego dostawcy (- moz-- webkit)

CSS3 wprowadzono wiele właściwości, które są zaimplementowane w różnych przeglądarkach w różnym czasie. To wcześniej wymuszone deweloperom kodu dla przeglądarki przy użyciu składni specyficznych dla dostawcy. Te właściwości specyficzne dla przeglądarki są teraz zawarta w funkcji IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Obsługa komentowania i Trwa usuwanie komentarza

Można teraz dodać komentarz i Usuń komentarz reguły CSS przy użyciu tego samego skrótów używanych w edytorze kodu (Ctrl + K, C, aby komentarz i Ctrl + K, można usuń znaczniki komentarza).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selektor kolorów

W poprzednich wersjach programu Visual Studio IntelliSense dla atrybutów dotyczące kolorów składa się z listy rozwijanej wartości koloru. Selektor koloru kompletne została zastąpiona tej listy.

Po wprowadzeniu wartości koloru próbnika kolorów są wyświetlane automatycznie i wyświetla listę wcześniej używanych kolorów następuje domyślnej palety kolorów. Można wybrać kolor za pomocą myszy lub klawiatury.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Listy można rozszerzyć do selektora pełny kolor. Selektor umożliwia sterowanie kanału alfa za pomocą automatycznej konwersji kolorów do RGBA, gdy suwak Przezroczystość:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Wstawki kodu programu

Fragmenty kodu w edytorze CSS umożliwiają łatwiejsze i szybsze tworzenie stylów w różnych przeglądarkach. Teraz zostały wycofane wiele właściwości CSS3, które wymagają ustawienia dotyczące przeglądarki do fragmentów.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Fragmenty kodu CSS obsługuje zaawansowanych scenariuszy (takich jak zapytaniami multimediów CSS3), wpisując w symbolu (@), który wskazuje na liście IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Po wybraniu @media wartość i naciśnij klawisz Tab, Edytor CSS wstawia następujący fragment kodu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Podobnie jak w przypadku fragmenty kodu, możesz utworzyć własne fragmenty kodu CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Niestandardowe regionów

O nazwie regionach kodu, które są już dostępne w edytorze kodu, są teraz dostępne do edycji CSS. Dzięki temu można łatwo grupy bloki style pokrewne.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Po zwinięciu region zawiera nazwę regionu:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspektor strony

Narzędzie Page Inspector to narzędzie, które umożliwia Sprawdź, czy zarówno kod źródłowy, jak i dane wyjściowe i renderuje stronę sieci web (HTML, formularzy sieci Web, ASP.NET MVC lub strony sieci Web) w programie Visual Studio IDE. Stron ASP.NET narzędzie Page Inspector umożliwia określenie, które kodu po stronie serwera został utworzony kod znaczników HTML, który jest renderowany w przeglądarce.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Aby uzyskać więcej informacji na temat narzędzia Page Inspector zobacz następujące samouczki:

- Za pomocą narzędzia Page Inspector w [platformy ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Za pomocą narzędzia Page Inspector w [formularzy sieci Web ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publikowanie

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profilów publikowania

W programie Visual Studio 2010 publikowanie informacji o projektów aplikacji sieci Web nie są przechowywane w kontroli wersji i nie jest przeznaczony do udostępniania innym użytkownikom. W programie Visual Studio 2012 w wersji Release Candidate został zmieniony format profilu publikowania. Dokonano artefaktu zespołu i jest teraz łatwo korzystać z kompilacji w oparciu MSBuild. Informacje o konfiguracji kompilacji jest w oknie dialogowym Publikowanie, dzięki czemu można łatwo przełączać konfiguracje kompilacji przed opublikowaniem.

Publikowanie profile są przechowywane w folderze PublishProfiles. Zależy od lokalizacji folderu, w jakim języku programowania używasz:

- C#: Properties\PublishProfiles
- Visual Basic: Moje Project\PublishProfiles

Każdy profil jest plik programu MSBuild. Podczas publikowania, ten plik jest importowany do pliku projektu MSBuild. W programie Visual Studio 2010, jeśli chcesz wprowadzić zmiany w procesie publikowania lub pakietu należy umieścić dostosowania w pliku o nazwie **ProjectName**. wpp.targets. Nadal jest to obsługiwane, ale dostosowania teraz można umieścić w profil publikowania. Dzięki temu dostosowania będzie używany tylko dla tego profilu.

Można teraz również wykorzystać publikować profile z programu MSBuild. Podczas tworzenia projektu, aby to zrobić, użyj następującego polecenia:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Wartość project.csproj jest ścieżkę projektu i ProfileName jest nazwą profilu publikowania. Możesz też zamiast przekazywanie nazwę profilu *PublishProfile* właściwości, można przekazać pełną ścieżkę do profilu publikowania.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Wstępnej kompilacji platformy ASP.NET i dokonać scalania

Projekty aplikacji sieci Web programu Visual Studio 2012 Release Candidate dodaje opcję na stronie właściwości Pakuj/Publikuj Web umożliwiający wstępnej kompilacji i scalanie zawartości witryny sieci podczas publikowania lub pakiet projektu. Aby wyświetlić te opcje, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz polecenie Właściwości, a następnie wybierz stronie właściwości Pakuj/Publikuj Web. Na poniższej ilustracji przedstawiono Precompile tej aplikacji przed opublikowaniem opcji.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Gdy ta opcja jest zaznaczona, Visual Studio precompiles aplikacji zawsze, gdy opublikujesz lub pakietu aplikacji sieci web. Jeśli chcesz kontrolować sposób została wstępnie skompilowana lokacji lub jak zestawy są scalane, kliknij przycisk Zaawansowane, aby skonfigurować te opcje.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>Usługi IIS Express

Domyślnego serwera sieci web do testowania projektów sieci web w programie Visual Studio jest teraz program IIS Express. Serwera wdrożeniowego programu Visual Studio jest wciąż opcję Serwer sieci web w lokalnej podczas tworzenia, ale usług IIS Express jest teraz serwerem zalecane. Korzystania z usług IIS Express w Visual Studio 11 Beta jest bardzo podobny do używania go w programie Visual Studio 2010 z dodatkiem SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może znacznie zmienione przed końcowej wersji oprogramowania opisanych tutaj.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok elementu firmy Microsoft Corporation na zagadnień omówionych w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft i firma Microsoft nie może zagwarantować dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH LUB USTAWOWYCH, ODNOŚNIE DO INFORMACJI W TYM DOKUMENCIE.

Zgodne z wszystkich stosownych praw autorskich jest odpowiedzialny za użytkownika. Bez ograniczenia praw autorskich, żadnej części tego dokumentu może być odtworzyć, przechowywane w wprowadzane do systemu pobierania lub przekazywać w jakimkolwiek formularzu za pomocą jakichkolwiek metod (elektronicznych, mechanicznych, postaci kopii, nagrań lub innych) w jakimkolwiek celu, bez pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej będących przedmiotem tego dokumentu. Z wyjątkiem wyraźnie określonych w umowach licencyjnych firmy Microsoft, otrzymanie tego dokumentu nie oznacza udzielenia licencji na te patenty, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej.

Jeśli nie podano inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia opisywane w przykładach są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, poczty e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2012 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami towarowymi ich prawnych właścicieli.
