---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 i Visual Studio 2010 w sieci Web Development — omówienie | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera omówienie wiele nowych funkcji dla platformy ASP.NET, które są uwzględnione w ramach platformy.NET Framework 4 i w programie Visual Studio 2010.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 775286df610df9040cbf04125b1742b6befa055b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753409"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 i omówienie projektowania aplikacji internetowych programu Visual Studio 2010
====================
> Ten dokument zawiera omówienie wiele nowych funkcji dla platformy ASP.NET, które są uwzględnione w ramach platformy.NET Framework 4 i w programie Visual Studio 2010.
> 
> [Pobierz ten oficjalny dokument](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Zawartość**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Plik Web.config, Refaktoryzacja](#0.2__Toc253429239 "_Toc253429239")  
[Buforowanie danych wyjściowych Extensible](#0.2__Toc253429240 "_Toc253429240")  
[Auto-Start aplikacji sieci Web](#0.2__Toc253429241 "_Toc253429241")  
[Trwałe przekierowanie strony](#0.2__Toc253429242 "_Toc253429242")  
[Zmniejszanie stanu sesji](#0.2__Toc253429243 "_Toc253429243")  
[Rozszerzenie zakresu dozwolone adresy URL](#0.2__Toc253429244 "_Toc253429244")  
[Weryfikacja żądania Extensible](#0.2__Toc253429245 "_Toc253429245")  
[Obiekt, buforowanie i obiektu pamięci podręcznej rozszerzeń](#0.2__Toc253429246 "_Toc253429246")  
[Rozszerzalne HTML, adres URL i kodowanie nagłówka HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitorowanie wydajności dla poszczególnych aplikacji w procesie roboczym pojedynczego](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[Uwzględnione za pomocą MVC i formularzy sieci Web w technologii jQuery](#0.2__Toc253429251 "_Toc253429251")  
[Obsługa sieci dostarczania zawartości](#0.2__Toc253429252 "_Toc253429252")  
[Skrypty jawne ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Ustawianie tagów Meta Page.MetaKeywords i właściwości Page.MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Włączanie stanu widoku dla poszczególnych formantów](#0.2__Toc253429258 "_Toc253429258")  
[Zmienia się na możliwości przeglądarki](#0.2__Toc253429259 "_Toc253429259")  
[Routing na platformie ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Ustawienie identyfikatorami klientów](#0.2__Toc253429261 "_Toc253429261")  
[Trwały wybór wiersza w formantach danych](#0.2__Toc253429262 "_Toc253429262")  
[Formant wykresu ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrowanie danych za pomocą kontrolki QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[HTML zakodowane kodu wyrażeń](#0.2__Toc253429265 "_Toc253429265")  
[Zmiany szablonu projektu](#0.2__Toc253429266 "_Toc253429266")  
[Ulepszenia CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ukrywanie div elementy wokół ukryte pola](#0.2__Toc253429268 "_Toc253429268")  
[Renderowanie tabeli zewnętrznej dla kontrolki z szablonami](#0.2__Toc253429269 "_Toc253429269")  
[Ulepszenia formantów ListView](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList i ulepszenia kontroli RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Ulepszenia kontroli menu](#0.2__Toc253429272 "_Toc253429272")  
[Kreator i formanty CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Obsługa obszarów](#0.2__Toc253429275 "_Toc253429275")  
[Obsługa sprawdzania poprawności atrybutów adnotacji danych](#0.2__Toc253429276 "_Toc253429276")  
[Pomocników szablonu](#0.2__Toc253429277 "_Toc253429277")

**[Dane dynamiczne](#0.2__Toc253429278 "_Toc253429278")**  
[Włączanie danych dynamicznych w przypadku istniejących projektów](#0.2__Toc253429279 "_Toc253429279")  
[Składnia deklaratywne kontrolki Menedżera danych dynamicznych](#0.2__Toc253429280 "_Toc253429280")  
[Szablony jednostki](#0.2__Toc253429281 "_Toc253429281")  
[Nowe szablony pola dla adresów URL i adresów E-mail](#0.2__Toc253429282 "_Toc253429282")  
[Tworzenie łączy główny za pomocą kontrolki DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Obsługa dziedziczenia w modelu danych](#0.2__Toc253429284 "_Toc253429284")  
[Obsługa relacji wiele do wielu (tylko platformy Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nowe atrybuty do wyświetlenia sterowania i pomocy technicznej wyliczenia](#0.2__Toc253429286 "_Toc253429286")  
[Rozszerzona obsługa filtrów](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 w sieci Web ulepszeń dotyczących programowania](#0.2__Toc253429288 "_Toc253429288")**  
[Zwiększona zgodność CSS](#0.2__Toc253429289 "_Toc253429289")  
[HTML i JavaScript fragmenty](#0.2__Toc253429290 "_Toc253429290")  
[Ulepszenia funkcji JavaScript IntelliSense](#0.2__Toc253429291 "_Toc253429291")

**[Wdrażanie aplikacji za pomocą programu Visual Studio 2010 w sieci Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Web.config Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Wdrożenie bazy danych](#0.2__Toc253429295 "_Toc253429295")  
[Publikowanie jednym kliknięciem dla aplikacji sieci Web](#0.2__Toc253429296 "_Toc253429296")  
[Zasoby](#0.2__Toc253429297 "_Toc253429297")

**[Zastrzeżenie](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Podstawowych usług

ASP.NET 4 wprowadzono szereg funkcji, które zwiększają usług ASP.NET core, takich jak buforowanie danych wyjściowych i przechowywanie stanu sesji.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Plik Web.config, Refaktoryzacja

`Web.config` Pliku, który zawiera konfigurację dla aplikacji sieci Web, która wzrosła znacznie za pośrednictwem kilku poprzednich wersjach programu .NET Framework jako zostały dodane nowe funkcje, takie jak Ajax, routingu i integracji z programem IIS 7. To ma stało się jeszcze trudniejsze do skonfigurowania lub Rozpocznij nowe aplikacje sieci Web bez narzędzia takiego jak Visual Studio. W .the .NET Framework 4 elementy główne konfiguracji zostały przeniesione do ikony `machine.config` plików i aplikacji teraz te ustawienia były dziedziczone. Dzięki temu `Web.config` pliku w aplikacji platformy ASP.NET 4, być pusta lub zawierać tylko następujące wiersze, które określ dla programu Visual Studio, jakie wersje framework aplikacji jest przeznaczony dla:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Buforowanie danych wyjściowych rozszerzalny

Od czasu wydania programu ASP.NET w wersji 1.0 buforowanie danych wyjściowych włączył deweloperom przechowywanie wygenerowanych danych wyjściowych strony, kontrolek i odpowiedzi HTTP w pamięci. Podczas kolejnych żądań sieci Web ASP.NET może obsługiwać zawartość szybciej pobierając wygenerowanych danych wyjściowych z pamięci zamiast ponownego generowania danych wyjściowych od podstaw. Jednak to podejście ma ograniczenie — zawartość jest generowana musi zawsze być przechowywane w pamięci, a na serwerach, na których wystąpiły duży ruch pamięci używane przez buforowanie danych wyjściowych mogą konkurować z zapotrzebowaniem na zasoby pamięci z innych części aplikacji sieci Web.

ASP.NET 4 dodaje punktem rozszerzalności do buforowania danych wyjściowych, który pozwala na skonfigurowanie co najmniej jeden niestandardowych dostawców pamięci podręcznej danych wyjściowych. Dostawcy wyjściowej pamięci podręcznej mogą wykorzystywać każdy mechanizm magazynu, aby zachować zawartość HTML. Umożliwia to tworzenie niestandardowych dostawców pamięci podręcznej danych wyjściowych dla trwałości różnych mechanizmów, które mogą obejmować dysków lokalnych lub zdalnych, Magazyn w chmurze i rozproszonej pamięci podręcznej silników.

Tworzenie niestandardowego dostawcy wyjściowej pamięci podręcznej jako klasę pochodzącą z nowym *System.Web.Caching.OutputCacheProvider* typu. Następnie możesz skonfigurować dostawcę w `Web.config` plików za pomocą nowego *dostawców* podsekcję *outputCache* elementu, jak pokazano w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample2.xml)]

Domyślnie w platformie ASP.NET 4, wszystkie odpowiedzi HTTP, renderowania stron i kontrolek przy użyciu pamięci podręcznej danych wyjściowych w pamięci, jak pokazano w poprzednim przykładzie, gdzie *właściwości defaultProvider* AspNetInternalProvider ma ustawioną wartość atrybutu. Możesz zmienić domyślny dostawca wyjściowej pamięci podręcznej używane dla aplikacji sieci Web, określając nazwę innego dostawcy *właściwości defaultProvider*.

Ponadto można wybrać różnych dostawców pamięci podręcznej danych wyjściowych dla każdego formantu i na żądanie. Najprostszym sposobem wybierz innego dostawcę wyjściowej pamięci podręcznej w przypadku różnych kontrolek użytkownika sieci Web jest tak w sposób deklaratywny za pomocą nowego *providerName* atrybutu w dyrektywie kontroli, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Określanie dostawcy pamięci podręcznej danych wyjściowych inne niż dla żądania HTTP wymaga nieco więcej pracy. Zamiast określania deklaratywne dostawcy, możesz zastąpić nowym *GetOuputCacheProviderName* method in Class metoda `Global.asax` plik, aby programowo określić który dostawca do użycia dla określonego żądania. W przykładzie poniżej pokazano, jak to zrobić.

[!code-csharp[Main](overview/samples/sample4.cs)]

Z dodatkiem rozszerzalności dostawca wyjściowej pamięci podręcznej ASP.NET 4 można teraz dążyć skuteczniejsze i bardziej inteligentne i strategii buforowania danych wyjściowych witryn sieci Web. Na przykład obecnie istnieje możliwość buforowania strony "Top 10" w lokacji w pamięci, podczas buforowania stron, które niższe ruchu na dysku. Możesz też każdej różnią się w relacji do renderowanej strony w pamięci podręcznej, ale tak, aby zmniejszenie zużycia pamięci jest odciążany z serwerów frontonu sieci Web przy użyciu rozproszonej pamięci podręcznej.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Automatyczne uruchamianie aplikacji sieci Web

Niektóre aplikacje sieci Web musi załadować dużych ilości danych lub wykonywania kosztownych inicjowanie przetwarzania przed rozpoczęciem pierwszego żądania obsługi. We wcześniejszych wersjach programu ASP.NET w takich sytuacjach konieczne było opracowanie niestandardowej metody "wznawiania" aplikacji ASP.NET, a następnie uruchom kod inicjujący podczas *aplikacji\_obciążenia* method in Class metoda `Global.asax` plik.

Nowa funkcja skalowania o nazwie *auto-start* , bezpośrednio adresy ten scenariusz jest dostępny po platformy ASP.NET 4 działa usług IIS 7.5 w systemie Windows Server 2008 R2. Funkcja automatycznego uruchamiania zapewnia kontrolowanego podejście do uruchamiania puli aplikacji, inicjowanie aplikacji ASP.NET i następnie akceptując żądania HTTP.

> [!NOTE] 
> 
> Moduł rozgrzewania aplikacji usług IIS dla usług IIS 7.5
> 
> Zespół usługi IIS wydała pierwszą wersję testu wersji beta modułu zwiększanie gotowości aplikacji dla usług IIS 7.5. To sprawia, że Trwa rozgrzewanie aplikacji jeszcze łatwiejsze niż opisany wcześniej. Zamiast pisać kod niestandardowy, należy określić adresy URL zasobów do wykonania przed aplikacji sieci Web akceptuje żądania z sieci. Ta rozgrzewania występuje podczas uruchamiania usługi IIS (Jeśli skonfigurowano puli aplikacji usług IIS jako *AlwaysRunning*) i podczas odtwarzania procesu roboczego programu IIS. Podczas odtwarzania stare proces roboczy usług IIS w dalszym ciągu wykonywać żądania do momentu nowo zduplikowanych proces jest całkowicie przygotowaniu, tak, aby aplikacje środowiska nie przerw w pracy lub inne problemy z powodu unprimed pamięci podręcznych. Należy pamiętać, że ten moduł działa z dowolną wersją programu ASP.NET, począwszy od wersji 2.0.
> 
> Aby uzyskać więcej informacji, zobacz [zwiększanie gotowości aplikacji](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) witrynie IIS.net w sieci Web. Aby uzyskać wskazówki, który ilustruje sposób użycia funkcji rozgrzewania, zobacz [rozpoczęcie korzystania z modułu rozgrzewania aplikacji usług IIS 7.5](https://www.iis.net/learn/manage) witrynie IIS.net w sieci Web.


Aby korzystać z funkcji automatycznego uruchamiania, administrator usług IIS Ustawia pulę aplikacji w IIS 7.5 ma zostać automatycznie uruchomiony przy użyciu następującej konfiguracji w `applicationHost.config` pliku:

[!code-xml[Main](overview/samples/sample5.xml)]

Jednej puli aplikacji może zawierać wiele aplikacji, podaj poszczególne aplikacje, które ma zostać automatycznie uruchomiony przy użyciu następującej konfiguracji w `applicationHost.config` pliku:

[!code-xml[Main](overview/samples/sample6.xml)]

Gdy na serwerze usług IIS 7.5 jest uruchomiona zimnego lub jednej puli aplikacji zostanie odtworzona, IIS 7.5 używa tych informacji w `applicationHost.config` plik, aby określić potrzeby aplikacji sieci Web uruchomiony automatycznie. Dla każdej aplikacji, który jest oznaczony do automatycznego uruchamiania IIS 7.5 wysyła żądanie do programu ASP.NET 4, aby uruchomić aplikację w stanie, w którym aplikacja tymczasowo nie akceptuje żądań HTTP. Gdy jest to w tym stanie, program ASP.NET tworzy wystąpienie z typem zdefiniowanym przez *serviceAutoStartProvider* atrybutów (jak pokazano w poprzednim przykładzie) i wywołuje jego punktu wejścia publicznych.

Tworzenie typu zarządzanego auto-start z punktem wejścia konieczne zaimplementowanie *IProcessHostPreloadClient* interfejsu, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample7.cs)]

Po inicjalizacji kod działa na platformie *Preload* metody, a także metoda zwróci wartość, aplikacja ASP.NET jest gotowa do przetwarzania żądań.

Z dodatkiem Autostart.5 usług IIS i platformy ASP.NET 4 jest teraz dostępna dobrze zdefiniowanych podejście do wykonywania inicjowanie kosztowne aplikacji przed rozpoczęciem przetwarzania pierwszego żądania HTTP. Na przykład można użyć nowej funkcji automatycznego uruchamiania na inicjowanie aplikacji, a następnie sygnał równoważenia obciążenia, czy aplikacja została zainicjowana i gotowy do akceptowania ruchu HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Trwałe przekierowanie strony

Jest to powszechną praktyką w aplikacjach sieci Web można przenieść strony i innej zawartości wokół wraz z upływem czasu, co może prowadzić do nagromadzenia starych łącza w wyszukiwarkach. W programie ASP.NET: deweloperzy mają obsługiwane tradycyjnie żądania do starych adresów URL przy użyciu *Response.Redirect* metodę, aby przekazywać żądania do nowego adresu URL. Jednak *przekierowania* metoda wysyła odpowiedź HTTP 302 znaleźć (Przekierowanie tymczasowe), co skutkuje dodatkowe HTTP obiegu gdy użytkownicy próbują uzyskać dostęp ze starych adresów URL.

ASP.NET 4 dodaje nowy *RedirectPermanent* metody pomocnika, która ułatwia problem HTTP 301 trwale przeniesiona odpowiedzi, jak w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample8.cs)]

Aparaty wyszukiwania i innych agentów użytkownika, które rozpoznaje stałe przekierowuje zapisze nowy adres URL, który jest skojarzony z zawartości, co eliminuje niepotrzebne obiegu przez przeglądarkę dla przekierowania tymczasowego.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Zmniejszanie stanu sesji

Program ASP.NET oferuje dwie opcje domyślne do przechowywania stanu sesji w ramach farmy sieci Web: Dostawca stanu sesji, który wywołuje serwer stanu sesji poza procesem, a dostawca stanu sesji, która przechowuje dane w bazie danych programu Microsoft SQL Server. Obie opcje obejmują przechowywanie informacji o stanie spoza procesu roboczego aplikacji sieci Web, stan sesji musi być serializowana przed wysłaniem go do magazynu zdalnego. W zależności od ilości informacji dewelopera są zapisywane w stan sesji rozmiar serializowane dane można powiększać dość duży.

ASP.NET 4 wprowadzono nową opcję kompresji dla obu rodzajów dostawcy stanu sesji poza procesem. Gdy *compressionEnabled* opcji konfiguracji pokazano w poniższym przykładzie ustawiono *true*, ASP.NET będzie kompresji (i dekompresji) stanu sesji serializacji przy użyciu programu .NET Framework  *System.IO.Compression.GZipStream* klasy.

[!code-xml[Main](overview/samples/sample9.xml)]

Za pomocą proste dodanie nowego atrybutu do `Web.config` plików, aplikacji o wolnym cykle procesora CPU na serwerach sieci Web może realizować znaczną redukcję rozmiaru serializowane dane stanu sesji.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Rozszerzenie zakresu dozwolone adresy URL

ASP.NET 4 wprowadzono nowe opcje dotyczące rozszerzania rozmiaru adresu URL aplikacji. Poprzednie wersje programu ASP.NET ograniczone długości ścieżki adresu URL do 260 znaków, oparte na limit ścieżka pliku systemu plików NTFS. ASP.NET 4, masz opcję, aby zwiększyć (lub zmniejszyć) ten limit, zgodnie z potrzebami dla Twoich aplikacji, używając dwóch nowych *httpRuntime* atrybuty konfiguracji. Poniższy przykład przedstawia te nowe atrybuty.

[!code-xml[Main](overview/samples/sample10.xml)]

Aby zezwolić na dłuższy lub krótszy ścieżki (część adresu URL, który nie obejmuje protokołu, nazwy serwera i ciągu zapytania), należy zmodyfikować *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* atrybutu. Aby zezwolić na ciągi zapytań dłuższy lub krótszy, zmodyfikuj wartość *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* atrybutu.

ASP.NET 4 umożliwia również konfigurowanie znaki, które są używane przez sprawdzenie znaku adresu URL. Jeśli program ASP.NET znajdzie nieprawidłowy znak w część ścieżki adresu URL, odrzuca żądanie i zgłasza błąd HTTP 400. W poprzednich wersjach programu ASP.NET sprawdza znak adresu URL były ograniczone do stały zestaw znaków. ASP.NET 4, można dostosować zbiór prawidłowych znaków za pomocą nowego *requestPathInvalidChars* atrybutu *httpRuntime* element konfiguracji, jak pokazano w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample11.xml)]

Domyślnie <em>requestPathInvalidChars</em> atrybut definiuje osiem znaków jako nieprawidłowe. (W ciągu, która jest przypisana do <em>requestPathInvalidChars</em> domyślnie<em>,</em>mniej niż (&lt;), większe niż (&gt;) i handlowe "i" (&amp;) znaków zakodowane, ponieważ `Web.config` plik jest plikiem XML.) Można dostosować zestaw nieprawidłowych znaków, zgodnie z potrzebami.

> [!NOTE]
> Należy pamiętać, platformy ASP.NET 4 zawsze odrzuca ścieżki adresu URL, zawierających znaki w zakresie ASCII od 0x00 do 0x1F, ponieważ te są nieprawidłowe znaki adresu URL, zgodnie z definicją w dokumencie RFC 2396 IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). W wersjach systemu Windows Server z systemem usług IIS 6 lub nowszego, sterownik http.sys protokołu urządzenia automatycznie odrzuca adresy URL z tych znaków.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Sprawdzanie poprawności Extensible żądań

Weryfikacja żądania ASP.NET wyszukuje dane przychodzące żądania HTTP dla ciągów, które są często używane w atakami skryptów między witrynami (XSS). Jeśli zostaną znalezione potencjalne ciągi XSS, weryfikacja żądania flagi podejrzanych ciąg i zwraca błąd. Weryfikacja żądania wbudowanych zwraca błąd, tylko wtedy, gdy znajdzie najbardziej typowych ciągów wykorzystanych w atakom XSS. Poprzednie próby się sprawdzanie poprawności XSS wyższe w wyniku zbyt wiele fałszywych alarmów. Jednak klienci powinni weryfikacji żądań, wyższe albo z drugiej strony może być celowo złagodzenie XSS kontroli dla określonej strony lub dla określonych typów żądań.

ASP.NET 4 funkcję weryfikacji żądania dokonano extensible tak, aby można było używać logikę niestandardowego sprawdzania poprawności żądania. Aby rozszerzyć weryfikację żądań, możesz utworzyć klasę, która pochodzi od nowej *System.Web.Util.RequestValidator* typu, na które należy skonfigurować aplikację (w *httpRuntime* sekcji `Web.config`plików) do użycia na typ niestandardowy. Poniższy przykład pokazuje, jak skonfigurować klasy niestandardowego sprawdzania poprawności żądania:

[!code-xml[Main](overview/samples/sample12.xml)]

Nowy *requestValidationType* atrybut wymaga standardowy ciąg do identyfikatora typu .NET Framework, który określa klasę, która zapewnia weryfikację żądań niestandardowych. Dla każdego żądania ASP.NET wywołuje typ niestandardowy w celu przetworzenia każdego z nich dane przychodzące żądania HTTP. Przychodzącego adresu URL, wszystkie nagłówki HTTP (pliki cookie i nagłówki niestandardowe) i treść jednostki są wszystkie dostępne w celu przeprowadzenia inspekcji przez klasę sprawdzania poprawności żądanie niestandardowe, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample13.cs)]

W przypadkach, w których nie chcesz sprawdzić element przychodzących danych protokołu HTTP, klasa weryfikacji żądań może zostać przywrócone umożliwiające weryfikacji żądań domyślne ASP.NET uruchamiane przez wywołanie po prostu *podstawowej. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Obiekt, buforowanie i obiektu pamięci podręcznej rozszerzalności

Od czasu pierwszej wersji platformy ASP.NET dołączyła pamięci podręcznej zaawansowane obiektu w pamięci (*System.Web.Caching.Cache*). Implementacja pamięci podręcznej zostało tak popularne, że został on użyty w innych aplikacji. Jednak jest niewygodna dla aplikacji Windows Forms i WPF obejmujący odwołaniem do `System.Web.dll` wystarczy, aby można było korzystać z pamięci podręcznej obiektu ASP.NET.

Aby udostępnić buforowania dla wszystkich aplikacji, .NET Framework 4 wprowadzono nowego zestawu, nowej przestrzeni nazw, niektóre typy podstawowe i konkretny implementacji buforowania. Nowy `System.Runtime.Caching.dll` zestaw zawiera nowy interfejs API pamięci podręcznej w *System.Runtime.Caching* przestrzeni nazw. Przestrzeń nazw zawiera dwa podstawowe zestawy klasy:

- Typy abstrakcyjne, które stanowią podstawę do tworzenia dowolnego typu implementacji niestandardowych pamięci podręcznej.
- Implementacja pamięci podręcznej konkretny obiekt w pamięci ( *System.Runtime.Caching.MemoryCache* klasy).

Nowy *MemoryCache* klasy jest formowana ściśle w pamięci podręcznej platformy ASP.NET i współużytkuje znaczną część logiki aparatu wewnętrzną pamięć podręczną za pomocą platformy ASP.NET. Mimo że publicznych interfejsów API pamięci podręcznej w *System.Runtime.Caching* zostały zaktualizowanie, aby pozwalała na projektowanie niestandardowych pamięciami podręcznymi, jeśli używasz platformy ASP.NET *pamięci podręcznej* obiektu, znajdują się znane pojęcia związane z nowe interfejsy API.

Szczegółowe omówienie nowej *MemoryCache* klasy i obsługa podstawowych interfejsów API wymagałoby całego dokumentu. Natomiast poniższy przykład daje wyobrażenie działania nową pamięć podręczną interfejsu API. Przykład został napisany dla aplikacji Windows Forms, bez żadnych zależności na `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Rozszerzalne HTML, adres URL i kodowanie nagłówka HTTP

W platformie ASP.NET 4 możesz utworzyć niestandardowe procedury kodowania dla następujących typowych zadań kodowania tekstu:

- Kodowanie HTML.
- Kodowanie adresu URL.
- Kodowanie atrybutu HTML.
- Szyfrowanie ruchu wychodzącego nagłówków HTTP.

Możesz utworzyć niestandardowego kodera, wynikające z nowym *System.Web.Util.HttpEncoder* typu, a następnie konfigurując ASP.NET do użycia na typ niestandardowy w *httpRuntime* części `Web.config` plik jako pokazano w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample15.xml)]

Po skonfigurowaniu niestandardowego kodera ASP.NET automatycznie wywołuje implementację niestandardową kodowania zawsze, gdy jest to publiczny, kodowanie metod *System.Web.HttpUtility* lub *System.Web.HttpServerUtility* noszą nazwę klasy. Dzięki temu jednej części zespół deweloperów sieci Web, tworzenia niestandardowego kodera, który implementuje kodowania znaków agresywne, podczas gdy pozostałe zespół deweloperów sieci Web w dalszym ciągu używa publicznego kodowanie interfejsów API platformy ASP.NET. Konfigurując centralnie niestandardowego kodera w *httpRuntime* elementu, ma gwarancji, że wszystkie wywołania kodowania tekstu z publicznej platformy ASP.NET, kodowanie interfejsy API są przesyłane za pośrednictwem niestandardowego kodera.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitorowanie wydajności dla poszczególnych aplikacji w procesie pojedynczego procesu roboczego

Aby zwiększyć liczbę witryn sieci Web, które mogą być hostowane na jednym serwerze, wielu dostawców usług hostingowych uruchamiać wiele aplikacji programu ASP.NET w procesie pojedynczego procesu roboczego. Jednak wiele aplikacji, użycie jednego, udostępnionego proces, jest trudne dla administratorów serwerów do identyfikowania poszczególnych aplikacji, które występują problemy.

ASP.NET 4 korzysta z nowych funkcji monitorowania zasobów wprowadzone przez środowisko CLR. Aby włączyć tę funkcję, należy dodać następujący fragment kodu konfiguracji XML do `aspnet.config` pliku konfiguracji.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Uwaga `aspnet.config` pliku znajduje się w katalogu, w którym jest zainstalowany .NET Framework. Nie jest `Web.config` pliku.


Gdy *appdomainresourcemonitoring —* funkcja została włączona, dwie nowe liczniki wydajności są dostępne w kategorii wydajności "Aplikacji ASP.NET": *% czasu procesora zarządzane* i  *Zarządzana pamięć używana*. Nowa funkcja zarządzania zasobów domen aplikacji CLR oba te liczniki wydajności służy do śledzenia, szacowany czas procesora CPU i użycie pamięci zarządzanej przez poszczególne aplikacje platformy ASP.NET. W rezultacie za pomocą programu ASP.NET 4 Administratorzy mają teraz bardziej szczegółowy wgląd w zużycia zasobów przez poszczególne aplikacje uruchomione w procesie pojedynczego procesu roboczego.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Wielowersyjność kodu

Można utworzyć aplikację, który jest przeznaczony dla określonej wersji programu .NET Framework. ASP.NET 4, nowy atrybut w *kompilacji* elementu `Web.config` pliku umożliwia skierowane do .NET Framework 4 lub nowszy. Jeśli jawnie docelowych programu .NET Framework 4 i obejmują elementy opcjonalne w `Web.config` plików, takie jak wpisy dla *system.codedom*, te elementy muszą być poprawne dla programu .NET Framework 4. (Jeśli nie jawnie programem .NET Framework 4, platforma docelowa jest wnioskowany z braku wpis w `Web.config` pliku.)

Poniższy przykład pokazuje użycie *targetFramework* atrybutu w *kompilacji* elementu `Web.config` pliku.

[!code-xml[Main](overview/samples/sample17.xml)]

Należy pamiętać o następujących dotyczących przeznaczonych dla określonej wersji programu .NET Framework:

- W puli aplikacji programu .NET Framework 4, systemu kompilacji platformy ASP.NET przyjęto założenie, .NET Framework 4 jako obiekt docelowy Jeśli `Web.config` plik nie zawiera *targetFramework* atrybutu lub jeśli `Web.config` plik nie istnieje. (Być może trzeba wprowadzać zmian kodowania do aplikacji w taki sposób, aby stał się działać w ramach programu .NET Framework 4).
- Jeśli dołączysz *targetFramework* atrybut Jeśli *system.codeDom* element jest zdefiniowany w `Web.config` pliku, ten plik musi zawierać prawidłowe wpisy dla programu .NET Framework 4.
- Jeśli używasz *aspnet\_kompilatora* polecenia wstępnej kompilacji aplikacji (na przykład w środowisku kompilacji), należy użyć odpowiedniej wersji *aspnet\_kompilatora* polecenie dla platformy docelowej. Za pomocą kompilatora, dostarczanej programu .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) do kompilowania dla platformy .NET Framework 3.5 i wcześniejszymi wersjami. Użyj kompilatora, który jest dostarczany za pomocą programu .NET Framework 4 skompilować aplikacje utworzone przy użyciu tej struktury lub nowszy.
- W czasie wykonywania, kompilator używa najnowsze zestawy framework, które są zainstalowane na komputerze (i w związku z tym w GAC). Jeśli aktualizacja zostanie podjęta później platformę (na przykład hipotetyczny wersji 4.1 jest zainstalowany), będzie można korzystać z funkcji w nowszej wersji Framework, nawet jeśli *targetFramework* atrybut jest przeznaczony dla starszej wersji (na przykład 4.0). (Jednak w czasie projektowania w programie Visual Studio 2010 lub jeśli używasz *aspnet\_kompilatora* polecenia przy użyciu nowszych funkcji Framework będą powodować błędy kompilatora).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery uwzględniony przy użyciu platformy MVC i formularzy sieci Web

Szablony programu Visual Studio dla platformy MVC i formularzy sieci Web obejmują biblioteki jQuery typu open source. Podczas tworzenia nowej witryny sieci Web lub projektu, tworzony jest folder skryptów, zawierające następujące pliki z 3:

- jQuery 1.4.1.js — czytelny dla człowieka, unminified wersję biblioteki jQuery.
- jQuery-14.1.min.js — zminimalizowany wersję biblioteki jQuery.
- jQuery-1.4.1-vsdoc.js — plik dokumentacji funkcji Intellisense dla biblioteki jQuery.

Uwzględnij unminified wersję jQuery podczas tworzenia aplikacji. Obejmują zminimalizowany wersję jQuery dla aplikacji produkcyjnych.

Na przykład następujące strony formularzy sieci Web ilustruje, jak można użyć jQuery, aby zmienić kolor tła kontrolki ASP.NET TextBox żółty, gdy fokus.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Obsługa usługi Content Delivery Network

Microsoft Ajax Content Delivery Network (CDN) umożliwia łatwe dodawanie do aplikacji sieci Web ASP.NET Ajax i jQuery skryptów. Na przykład, można uruchomić przy użyciu biblioteki jQuery poprzez dodanie `<script>` tag do strony, który wskazuje na Ajax.microsoft.com następująco:

[!code-html[Main](overview/samples/sample19.html)]

Dzięki wykorzystaniu Microsoft Ajax CDN może znacznie poprawić wydajność aplikacji Ajax. Zawartość Microsoft Ajax CDN są buforowane na serwerach znajdujących się na całym świecie. Ponadto Microsoft Ajax CDN umożliwia ponowne użycie pamięci podręcznej plików JavaScript dla witryn sieci Web, które znajdują się w różnych domenach w różnych przeglądarkach.

W przypadku, gdy potrzebujesz do obsługi strony sieci web przy użyciu protokołu Secure Sockets Layer, firmy Microsoft Ajax Content Delivery Network obsługuje protokołu SSL (HTTPS).

Implementowanie rezerwowe, gdy sieć CDN jest niedostępny. Testuj plan awaryjny.

Aby dowiedzieć się więcej na temat Microsoft Ajax CDN, odwiedź następującą witrynę:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Funkcja ScriptManager ASP.NET obsługuje Microsoft Ajax CDN. Po prostu przez ustawienie jedną właściwość, właściwości EnableCdn można pobrać wszystkich plików JavaScript framework ASP.NET z sieci CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Po ustawieniu właściwości EnableCdn wartość true, środowiska ASP.NET framework pobierze wszystkie pliki JavaScript framework platformy ASP.NET z sieci CDN, w tym wszystkie pliki JavaScript, używany do sprawdzania poprawności i kontrolki UpdatePanel. Ustawienie tej właściwości co może mieć znaczący wpływ na wydajność aplikacji sieci web.

Ścieżka CDN można ustawić dla plików JavaScript za pomocą atrybutu widok. Nowe właściwości CdnPath Określa ścieżkę do usługi CDN, używany gdy właściwość EnableCdn jest ustawiona na wartość true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Skrypty jawne Menedżera skryptów

W przeszłości Jeśli użyto ASP.NET ScriptManger następnie zostały należy załadować całej biblioteki monolityczne Ajax programu ASP.NET. Dzięki wykorzystaniu nową właściwość ScriptManager.AjaxFrameworkMode, można kontrolować, dokładnie składniki biblioteki ASP.NET Ajax są ładowane i załadować tylko składniki biblioteki ASP.NET Ajax, potrzebne.

Właściwość ScriptManager.AjaxFrameworkMode można ustawić następujące wartości:

- Włączone — Określa, że formantu ScriptManager automatycznie dołącza plik skryptu MicrosoftAjax.js jest pliku skryptu połączone każdego skryptu framework core (starsze zachowanie).
- Wyłączone — Określa wyłączenia wszystkich funkcji skryptów Microsoft Ajax i że formantu ScriptManager nie odwołuje się wszystkie skrypty automatycznie.
- Explicit — Określa, zostanie jawnie dołączyć odwołania do skryptu do pliku skryptu core framework poszczególnych wymagającego strony i obejmie odwołań do zależności, które wymaga każdego pliku skryptu.

Na przykład jeśli właściwość AjaxFrameworkMode jest ustawiona na wartość Explicit następnie można określić skrypty określonego składnika ASP.NET Ajax, wymagane:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Formularze sieci Web

Formularze sieci Web została funkcji na platformie ASP.NET core od czasu wydania programu ASP.NET w wersji 1.0. Wiele udoskonaleń zostały w tym obszarze dla platformy ASP.NET 4, takie jak następujące:

- Możliwość ustawiania *meta* tagów.
- Większa kontrola nad stan widoku.
- Łatwiejsze sposoby pracy z możliwości przeglądarki.
- Obsługa przy użyciu routingu platformy ASP.NET przy użyciu formularzy sieci Web.
- Większa kontrola nad generowanych identyfikatorów.
- Możliwość utrzymania wybranych wierszy w formantach danych.
- Większa kontrola nad renderowany kod HTML w *FormView* i *ListView* kontrolki.
- Obsługa filtrowania kontrolki źródła danych.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Ustawianie tagów Meta Page.MetaKeywords i Page.MetaDescription właściwości

ASP.NET 4 dodaje dwie właściwości do *strony* klasy *MetaKeywords* i *MetaDescription*. Te dwie właściwości reprezentują odpowiadającego *meta* tagów na stronie, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Te dwie właściwości działać tak samo jak strony *tytuł* jest właściwość. One należy wykonać następujące czynności:

1. W przypadku nie *meta* tagów w *head* element, który pasuje do nazwy właściwości (oznacza to, że nazwa = "słowa kluczowe" dla *Page.MetaKeywords* i nazwa = "description", aby uzyskać  *Page.MetaDescription*, co oznacza, że te właściwości nie zostały ustawione), *meta* tagi zostanie dodany do strony podczas jego renderowania.
2. Jeśli istnieją już *meta* tagi o tych nazwach, te właściwości pełnić rolę pobieranie i ustawianie metody dla zawartości istniejące tagi.

Te właściwości można ustawić w czasie wykonywania, które pozwala uzyskać zawartość z bazy danych lub innego źródła, a które pozwala ustawić tagi dynamicznie, aby Opisz, co dotyczy określonej strony.

Można również ustawić *słowa kluczowe* i *opis* właściwości w *@ Page* dyrektywę w górnej części znaczników strony formularzy sieci Web, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Spowoduje to zastąpienie *meta* zawartość (jeśli istnieje) już zadeklarowany na stronie tagu.

Zawartość opis *meta* tag służą do poprawy wyszukiwanie, wyświetlanie podglądów w firmie Google. (Aby uzyskać więcej informacji, zobacz [poprawić fragmenty kodu z zapamiętają Opis meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) na blogu Google webmastera centralnej.) Google i Windows Live Search nie korzystać z zawartości słowa kluczowe dla wszystkich elementów, ale może być inne aparaty wyszukiwania. Aby uzyskać więcej informacji, zobacz [porad słowa kluczowe Meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) w witrynie sieci Web przewodnik aparatu wyszukiwania.

Nowe właściwości są funkcją proste, ale możesz zapisać z wymaganie, aby dodać je ręcznie lub pisania własnego kodu, aby utworzyć *meta* tagów.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Włączanie stanu widoku dla pojedynczych formantów

Domyślnie stan widoku jest włączony dla tej strony, z wynikiem, że każdej kontrolki na stronie potencjalnie przechowuje stan widoku, nawet jeśli nie jest wymagane dla aplikacji. Dane stanu widoku znajduje się w znaczniku, że strona generuje i zwiększa ilość czasu potrzebnego do wysyłania strony klientowi i opublikuj go ponownie. Przechowywanie więcej stan widoku, niż jest to konieczne, może powodować znaczne pogorszenie wydajności. We wcześniejszych wersjach programu ASP.NET deweloperzy można wyłączyć stan widoku dla pojedynczych formantów w celu zmniejszenia rozmiaru strony, ale musiałem zrobić jednoznacznie dla pojedynczych formantów. W programie ASP.NET 4 to formanty serwera sieci Web *ViewStateMode* właściwość, która umożliwia powodująca domyślne wyłączenie stan widoku, a następnie włączyć ją tylko w przypadku kontrolek, które tego wymagają na stronie.

*ViewStateMode* właściwość przyjmuje wyliczenie, które ma trzy wartości: *włączone*, *wyłączone*, i *Dziedzicz*. *Włączone* umożliwia wyświetlanie stanu dla tej kontrolki i formanty podrzędne, które są ustawione na *Dziedzicz* lub że nic nie ustawione. *Wyłączone* wyłącza wyświetlanie stanu, a *Dziedzicz* Określa, że kontrolka używa *ViewStateMode* ustawienie z kontrolki nadrzędnej.

W poniższym przykładzie pokazano sposób, w jaki *ViewStateMode* działa właściwość. Znaczników i kodu dla formantów na następującej stronie zawiera wartości *ViewStateMode* właściwości:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Jak widać, kod wyłącza stan widoku dla kontrolki PlaceHolder1. Kontrolka label1 podrzędna dziedziczy wartości tej właściwości (*Dziedzicz* jest wartością domyślną dla *ViewStateMode* dla formantów.) i w związku z tym zapisuje bez stanu widoku. W kontrolce PlaceHolder2 *ViewStateMode* ustawiono *włączone*, więc etykiety 2 dziedziczy tę właściwość, a następnie zapisuje stanu widoku. Po pierwszym załadowaniu strony *tekstu* właściwość obu *etykiety* kontrolki jest ustawiony na ciąg "[DynamicValue]".

Te ustawienia powoduje, że jeśli strona ładuje się po raz pierwszy, następujące dane wyjściowe są wyświetlane w przeglądarce:

Wyłączone `: [DynamicValue]`

Włączone:`[DynamicValue]`

Po odświeżeniu strony jednak następujące dane wyjściowe są wyświetlane:

Wyłączone `: [DeclaredValue]`

Włączone:`[DynamicValue]`

Kontrolka label1 (których *ViewStateMode* wartość jest równa *wyłączone*) nie została zachowana wartość, która została ustawiona w kodzie. Jednak sterować etykiety 2 (których *ViewStateMode* wartość jest równa *włączone*) została zachowana swojego stanu.

Można również ustawić *ViewStateMode* w *@ Page* dyrektywy, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample26.aspx)]

*Strony* klasy jest po prostu inną kontrolkę; działa ona jako formant nadrzędny dla wszystkich formantów na stronie. Wartość domyślna *ViewStateMode* jest *włączone* dla wystąpienia elementu *strony*. Ponieważ formanty domyślnie *Dziedzicz*, kontrolek będą dziedziczyć *włączone* wartość właściwości, chyba że *ViewStateMode* na poziomie strony lub formant.

Wartość *ViewStateMode* Określa właściwość, jeśli stan widoku jest obsługiwane tylko wtedy, gdy *EnableViewState* właściwość jest ustawiona na *true*. Jeśli *EnableViewState* właściwość jest ustawiona na *false*, stan widoku nie zostaną zachowane nawet wtedy, gdy *ViewStateMode* ustawiono *włączone*.

Dobrze Wykorzystaj dla tej funkcji jest *ContentPlaceHolder* kontrolek w stronach wzorcowych, w którym można ustawić *ViewStateMode* do *wyłączone* wzorca strony, a następnie włączyć ją osobno dla *ContentPlaceHolder* formantów, które z kolei zawierają kontrolki, które wymagają stanu widoku.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Zmiany możliwości przeglądarki

ASP.NET określa możliwości przeglądarki, który użytkownik używa do przeglądania witryny przy użyciu funkcji o nazwie *możliwości przeglądarki*. Funkcje przeglądarki są reprezentowane przez *HttpBrowserCapabilities* obiektu (udostępniany przez *Request.Browser* właściwości). Na przykład, można użyć *HttpBrowserCapabilities* obiekt, aby sprawdzić, czy typ i wersja bieżąca przeglądarka obsługuje określoną wersję języka JavaScript. Alternatywnie można użyć *HttpBrowserCapabilities* obiektu w celu ustalenia, czy żądanie pochodzi z urządzenia przenośnego.

*HttpBrowserCapabilities* obiektu jest wymuszany przez zestaw plików definicji przeglądarki. Te pliki zawierają informacje na temat możliwości określonego przeglądarki. ASP.NET 4 te pliki definicji przeglądarki zostały zaktualizowane do zawierają informacje o wprowadzonych niedawno przeglądarek i urządzeń, takich jak Google Chrome, badania w ruchu BlackBerry smartfonów i iPhone firmy Apple.

Na poniższej liście przedstawiono Nowa przeglądarka plików definicji:

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Przy użyciu dostawców możliwości przeglądarki

W programie ASP.NET w wersji 3.5 z dodatkiem Service Pack 1, można zdefiniować możliwości, które ma przeglądarkę, w następujący sposób:

- Na poziomie komputera, Utwórz lub zaktualizuj `.browser` pliku XML w następującym folderze:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Po zdefiniowaniu możliwości przeglądarki, możesz uruchom następujące polecenie z wiersza polecenia Visual Studio aby odbudować zestaw możliwości przeglądarki i dodaj go do globalnej pamięci podręcznej zestawów:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Dla poszczególnych aplikacji, możesz utworzyć `.browser` pliku w aplikacji `App_Browsers` folderu.

Tych metod może powodować konieczność zmiany plików XML i zmiany na poziomie komputera, należy ponownie uruchomić aplikację po uruchomieniu aspnet\_regbrowsers.exe procesu.

ASP.NET 4 zawiera funkcję nazywane *dostawców możliwości przeglądarki*. Jak sugeruje nazwa, dzięki temu deweloper może utworzyć dostawcę, który z kolei pozwala użyć własnego kodu do określania możliwości przeglądarki.

W praktyce deweloperzy często nie określają funkcje niestandardowe przeglądarki. Przeglądarka plików są trudne do aktualizacji procesu dla ich uaktualnienia jest dość skomplikowane i składni XML `.browser` pliki mogą być złożone, aby użyć i zdefiniować. Co będzie sprawia, że ten proces znacznie łatwiej jest gdyby typowej składni definicji przeglądarki lub w bazie danych, która zawiera definicje aktualne przeglądarki lub nawet usługi sieci Web dla bazy danych. Nowa funkcja dostawców możliwości przeglądarki sprawia, że te scenariusze możliwe i praktyczne dla deweloperów innych firm.

Dostępne są dwie główne opcje dotyczące korzystania z nowej funkcji dostawcy możliwości przeglądarki programu ASP.NET 4: rozszerzanie możliwości przeglądarki ASP.NET definicji funkcji lub zastępowania go. Poniżej opisano najpierw jak zamienić funkcjonalność i rozszerzać go.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Zastępowanie funkcji możliwości przeglądarki programu ASP.NET

Aby zastąpić całkowicie funkcjonalności definicji możliwości przeglądarki programu ASP.NET, wykonaj następujące kroki:

1. Utwórz klasę dostawcy, która jest pochodną *HttpCapabilitiesProvider* i który zastępuje *GetBrowserCapabilities* metody, jak w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Kod w tym przykładzie tworzy nową *HttpBrowserCapabilities* obiektu, określając tylko funkcja o nazwie przeglądarka i ustawienie MyCustomBrowser taką możliwość.
2. Zarejestruj dostawcę z aplikacją. 

    Aby można było korzystać z dostawcy z aplikacją, należy dodać *dostawcy* atrybutu *browserCaps* sekcji `Web.config` lub `Machine.config` plików. (Można również zdefiniować atrybuty dostawcy w *lokalizacji* elementu dla określonych katalogów w aplikacji, takich jak folder dla określonego urządzenia przenośnego.) Poniższy przykład pokazuje, jak ustawić *dostawcy* atrybutu w pliku konfiguracji:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Zarejestruj nową definicję możliwości przeglądarki na innym sposobem jest użycie kodu, jak pokazano w poniższym przykładzie:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Ten kod, musisz uruchomić w *aplikacji\_Start* zdarzenia `Global.asax` pliku. Wszelkie zmiany do *BrowserCapabilitiesProvider* klasy musi wystąpić przed wykonaniem jakiegokolwiek kodu w aplikacji, aby upewnić się, że pamięć podręczna pozostanie w nieprawidłowym stanie dla rozwiązania *HttpCapabilitiesBase* obiektu.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Buforowanie obiektu HttpBrowserCapabilities

Poprzedni przykład jest jednym z problemów, który jest, że kod będzie Uruchom za każdym razem, niestandardowego dostawcy jest wywoływana w celu uzyskania *HttpBrowserCapabilities* obiektu. Przyczyną może być wielokrotnie podczas każdego żądania. W przykładzie kodu dla dostawcy nie wykonuje większość. Jednakże jeśli kod w niestandardowego dostawcy wykonuje dużo pracy w celu uzyskania *HttpBrowserCapabilities* obiektu, może to wpłynąć na wydajność. Aby temu zapobiec, można buforować *HttpBrowserCapabilities* obiektu. Wykonaj następujące kroki:

1. Utwórz klasę, która pochodzi od klasy *HttpCapabilitiesProvider*, takie jak w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    W tym przykładzie kod generuje klucz pamięci podręcznej przez wywołanie niestandardowej metody BuildCacheKey i pobiera czas do pamięci podręcznej przez wywołanie niestandardowej metody GetCacheTime. Następnie ten kod dodaje rozpoznać *HttpBrowserCapabilities* obiektu w pamięci podręcznej. Obiekt może być pobrane z pamięci podręcznej i ponownie używane w kolejnych żądań, które korzystanie z niestandardowego dostawcy.
2. Zarejestruj dostawcę w aplikacji, zgodnie z opisem w poprzedniej procedurze.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Rozszerzanie funkcjonalności możliwości przeglądarki programu ASP.NET

W poprzedniej sekcji opisano sposób tworzenia nowego *HttpBrowserCapabilities* obiektu w programie ASP.NET 4. Można rozszerzać funkcjonalność możliwości przeglądarki programu ASP.NET, dodając nowe definicje możliwości przeglądarki do tych, które znajdują się już na platformie ASP.NET. Można to zrobić bez korzystania z definicji XML w przeglądarce. Poniższej procedury jak.

1. Utwórz klasę, która pochodzi od klasy *HttpCapabilitiesEvaluator* i który zastępuje *GetBrowserCapabilities* metodzie, jak pokazano w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Ten kod używa najpierw funkcji możliwości przeglądarki, aby spróbować zidentyfikować przeglądarki. Jednakże, gdy przeglądarka nie zostanie zidentyfikowany na podstawie informacji zdefiniowane w żądaniu (to znaczy, jeśli *przeglądarki* właściwość *HttpBrowserCapabilities* obiekt jest ciąg "Nieznane"), kod wywołuje niestandardowy dostawca (MyBrowserCapabilitiesEvaluator), aby zidentyfikować przeglądarki.
2. Zarejestruj dostawcę w aplikacji, zgodnie z opisem w poprzednim przykładzie.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Rozszerzanie funkcjonalności możliwości przeglądarki, dodając nowe możliwości do istniejących definicji funkcji

Oprócz tworzenia Przeglądarka niestandardowa definicja dostawcy i dynamiczne utworzenie nowych definicji przeglądarki można rozszerzyć istniejące definicje przeglądarki z dodatkowymi możliwościami. Dzięki temu można używać definicji, który znajduje się w pobliżu co chcesz zrobić, ale nie zawiera tylko kilka funkcji. Aby to zrobić, wykonaj następujące kroki.

1. Utwórz klasę, która pochodzi od klasy *HttpCapabilitiesEvaluator* i który zastępuje *GetBrowserCapabilities* metodzie, jak pokazano w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Przykładowy kod rozszerza istniejący ASP.NET *HttpCapabilitiesEvaluator* klasy i pobiera *HttpBrowserCapabilities* obiektu, który odpowiada bieżącej definicji żądania przy użyciu następującego kodu :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kod następnie można dodać lub zmodyfikować możliwości w tej przeglądarce. Istnieją dwa sposoby, aby określić nowe możliwości przeglądarki:

    - Dodaj parę klucz wartość do *IDictionary* obiektu uwidocznionego przez *możliwości* właściwość *HttpCapabilitiesBase* obiektu. W poprzednim przykładzie, ten kod dodaje możliwości o wartości o nazwie MultiTouch *true*.
    - Ustaw właściwości istniejących *HttpCapabilitiesBase* obiektu. W poprzednim przykładzie, kod ustawia *ramek* właściwości *true*. Ta właściwość jest po prostu akcesora *IDictionary* obiektu uwidocznionego przez *możliwości* właściwości. 

        > [!NOTE]
        > Należy zauważyć ten model ma zastosowanie do żadnej właściwości *HttpBrowserCapabilities*, łącznie z adapterów kontrolek.
2. Zarejestruj dostawcę w aplikacji, zgodnie z opisem we wcześniejszej procedurze.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing na platformie ASP.NET 4

ASP.NET 4 dodaje wbudowaną obsługę przy użyciu routingu przy użyciu formularzy sieci Web. Routing umożliwia skonfigurowanie aplikacji do akceptowania żądań adresów URL, które nie są mapowane na pliki fizyczne. Zamiast tego służy routingu do definiowania adresów URL, które są zrozumiałe dla użytkowników i które może pomóc w optymalizacji dla aparatów wyszukiwania (SEO) dla aplikacji. Na przykład adres URL strony, która przedstawia kategorie produktów w istniejącej aplikacji może wyglądać następująco:

[!code-console[Main](overview/samples/sample36.cmd)]

Przy użyciu routingu, można skonfigurować aplikację do akceptowania następujący adres URL do renderowania te same informacje:

[!code-console[Main](overview/samples/sample37.cmd)]

Routing została dostępnych w programie ASP.NET 3.5 z dodatkiem SP1. (Aby uzyskać przykład sposobu użycia routingu w ASP.NET 3.5 z dodatkiem SP1, zobacz wpis [przy użyciu routingu przy użyciu formularzy sieci Web](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "tytułu tego wpisu.") na phila haacka.) ASP.NET 4 zawiera jednak niektóre funkcje, które ułatwiają używają routingu, takie jak następujące:

- *PageRouteHandler* klasy, która jest prosty program obsługi HTTP, używanego podczas definiowania trasy. Klasa przekazuje dane do strony, która żądanie jest kierowane do.
- Nowe właściwości *HttpRequest.RequestContext* i *Page.RouteData* (czyli serwer proxy dla *HttpRequest.RequestContext.RouteData* obiektu). Właściwości te ułatwiają uzyskiwanie dostępu do informacji, który jest przekazywany z trasy.
- Następujące konstruktorów wyrażeń nowe, które są zdefiniowane w *System.Web.Compilation.RouteUrlExpressionBuilder* i *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, który zapewnia prosty sposób utworzyć adres URL, który odpowiada adresowi URL trasy w ramach formant serwera ASP.NET.
- *RouteValue*, który zapewnia prosty sposób wyodrębniania informacji z *RouteContext* obiektu.
- *RouteParameter* klasy, która ułatwia przekazać dane zawarte w *RouteContext* obiektu do zapytania do kontroli źródła danych (podobnie jak [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing dla stron formularzy sieci Web

Poniższy przykład pokazuje jak zdefiniować trasę formularzy sieci Web za pomocą nowego *MapPageRoute* metody *trasy* klasy:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 wprowadzono *MapPageRoute* metody. Poniższy przykład jest równoważny z definicją SearchRoute pokazano w poprzednim przykładzie, ale używa *PageRouteHandler* klasy.

[!code-csharp[Main](overview/samples/sample39.cs)]

Kod w przykładzie mapuje trasę do strony fizycznej (w pierwsza trasa do `~/search.aspx`). Pierwszy definicję trasy określa również, że parametr o nazwie searchterm powinien być wyodrębnione z adresu URL i przekazane do strony.

*MapPageRoute* metoda obsługuje następujące przeciążenia metody:

- *MapPageRoute (routeName ciągu routeUrl ciąg, ciąg %{physicalfile/, bool checkPhysicalUrlAccess)*
- *MapPageRoute (routeName ciągu routeUrl ciąg, ciąg %{physicalfile/, bool checkPhysicalUrlAccess, RouteValueDictionary wartości domyślne)*
- *MapPageRoute (routeName ciągu routeUrl ciąg, ciąg %{physicalfile/, bool checkPhysicalUrlAccess, RouteValueDictionary wartości domyślne, RouteValueDictionary ograniczenia)*

*CheckPhysicalUrlAccess* parametr określa, czy trasa należy sprawdzić uprawnień zabezpieczeń dla strony fizycznej, jest kierowany do (w tym przypadku search.aspx) i uprawnienia dotyczące przychodzącego adresu URL (w tym przypadku wyszukiwania / {searchterm}). Jeśli wartość *checkPhysicalUrlAccess* jest *false*, tylko uprawnienia przychodzącego adresu URL, który zostanie sprawdzony. Te uprawnienia są definiowane w `Web.config` plików przy użyciu ustawień, takich jak następujące:

[!code-xml[Main](overview/samples/sample40.xml)]

W przykładzie konfiguracji, odmowa dostępu do strony fizycznej `search.aspx` dla wszystkich użytkowników z wyjątkiem tych, którzy znajdują się w roli administratora. Gdy *checkPhysicalUrlAccess* parametr ma wartość *true* (który jest jego wartość domyślna), tylko administrator użytkownicy mają możliwość dostępu do adresu URL /search/ {searchterm}, ponieważ search.aspx strony fizycznej ograniczony do użytkowników w tej roli. Jeśli *checkPhysicalUrlAccess* ustawiono *false* i lokację skonfigurowano tak jak pokazano w poprzednim przykładzie, mogą wszystkim uwierzytelnionym użytkownikom dostępu do adresu URL /search/ {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Odczytywanie informacji o routingu w stronie formularzy sieci Web

W kodzie strony fizycznej formularzy sieci Web, można uzyskać dostęp do informacji, który routingu został wyodrębniony z adresu URL (lub inne informacje, które inny obiekt został dodany do *RouteData* obiektów) przy użyciu dwie nowe właściwości:  *HttpRequest.RequestContext* i *Page.RouteData*. (*Page.RouteData* opakowuje *HttpRequest.RequestContext.RouteData*.) Poniższy przykład pokazuje, jak używać *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Kod wyodrębnia wartość, która została przekazana dla parametru searchterm, zgodnie z definicją w trasie przykład wcześniej. Należy wziąć pod uwagę następujący adres URL żądania:

[!code-console[Main](overview/samples/sample42.cmd)]

Nawiązaniem tym żądaniem, słowa "scott" może być renderowany w `search.aspx` strony.

#### <a name="accessing-routing-information-in-markup"></a>Uzyskiwanie dostępu do informacji o routingu w znacznikach

Metody opisanej w poprzedniej sekcji pokazano, jak można pobrać danych trasy w kodzie w strony formularzy sieci Web. Można również użyć wyrażeń w znaczniku, które umożliwiają dostęp do tych samych informacji. Konstruktorzy wyrażenia są wydajne i w prosty sposób pracy z kod deklaratywny. (Aby uzyskać więcej informacji, zobacz wpis [Express samodzielnie za pomocą niestandardowych wyrażeń Konstruktorzy](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) na phila haacka.)

ASP.NET 4 zawiera dwa nowe konstruktorów wyrażeń routingu formularzy sieci Web. Poniższy przykład pokazuje, jak z nich korzystać.

[!code-aspx[Main](overview/samples/sample43.aspx)]

W tym przykładzie *RouteUrl* wyrażenie jest używany do definiowania adres URL, który jest oparty na parametru trasy. Pozwala uniknąć konieczności trwale kodować pełny adres URL do znaczników i pozwala później zmienić strukturę adresu URL bez konieczności zmiany do tego łącza.

Na podstawie trasy zdefiniowane wcześniej, ten kod znaczników generuje następujący adres URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET działa automatycznie poprawnej trasy (czyli generuje on poprawny adres URL) na podstawie parametrów wejściowych. Można także nazwy trasy w wyrażeniu, które umożliwia określenie trasy do użycia.

Poniższy przykład pokazuje, jak używać *RouteValue* wyrażenia.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Po uruchomieniu strony, która zawiera tę kontrolkę, wartość "scott" jest wyświetlany w etykiecie.

*RouteValue* wyrażenie sprawia, że prosta w obsłudze danych trasy w znacznikach i pomaga uniknąć konieczności współpracy z bardziej złożonych Page.RouteData["x"] składni w znacznikach.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Korzystanie z danych trasy dla parametrów kontroli źródła danych

*RouteParameter* klasy pozwala określić dane trasy jako wartość parametru zapytania w kontroli źródła danych. Jego [działa podobnie jak](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) klasy, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample46.aspx)]

W tym przypadku zostanie użyta wartość searchterm parametru trasy dla @companyname parametru w <em>wybierz</em> instrukcji.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Ustawienie identyfikatory klienta

Nowy *ClientIDMode* właściwość adresy długotrwałych problem w programie ASP.NET to znaczy, jak formanty utworzyć *identyfikator* atrybutu dla elementów, które są one renderowane. Znajomość *identyfikator* atrybut renderowanych elementów jest ważne, jeśli aplikacja zawiera skrypt klienta, który odwołuje się do tych elementów.

*Identyfikator* atrybutu w formacie HTML, który jest renderowany formantów serwera sieci Web jest generowany na podstawie *ClientID* właściwości formantu. Do platformy ASP.NET 4, algorytm generuje *identyfikator* atrybut z *ClientID* właściwość została do łączenia kontenera nazewnictwa (jeśli istnieje) z Identyfikatorem, a w przypadku kontrolek wielokrotnego (podobnie jak w formanty danych), aby dodać prefiks i numeru sekwencyjnego. Gdy ma to zawsze gwarancję, że identyfikatorów kontrolek na stronie są unikatowe, algorytm spowodowało kontrolować identyfikatorów, które nie były przewidywalne, a w związku z tym były trudne do odwołania w skrypt po stronie klienta.

Nowy *ClientIDMode* właściwość umożliwia określenie, bardziej precyzyjne, jak identyfikator klienta jest generowany dla formantów. Możesz ustawić *ClientIDMode* właściwości dla każdego formantu, w tym dla strony. Ustawienia możliwe są następujące:

- *Identyfikator automatyczny* — jest to równoważne algorytmu do generowania *ClientID* wartości właściwości, które zostało użyte we wcześniejszych wersjach programu ASP.NET.
- *Statyczne* — oznacza, że *ClientID* wartość będzie taki sam jak identyfikator, bez łączenia identyfikatory nadrzędne nazewnictwa kontenerów. Może to być przydatne w przypadku kontrolek użytkownika sieci Web. Ponieważ kontrolka użytkownika sieci Web może znajdować na różnych stronach i w kontrolkach innego kontenera, może być trudne do pisania skrypt po stronie klienta dla formantów, które używają *identyfikator automatyczny* algorytmu, ponieważ nie można przewidzieć, co spowoduje wartości Identyfikatora .
- *Przewidywalne* — ta opcja jest głównie do użytku w formantach danych, które używają powtarzającymi się szablonami. Łączy ona właściwości Identyfikatora formantu nazewnictwa kontenerów, ale wygenerowanego *ClientID* wartości nie zawierają ciągi, takie jak "ctlxxx". To ustawienie działa w połączeniu z *ClientIDRowSuffix* właściwości formantu. Możesz ustawić *ClientIDRowSuffix* na nazwę pola danych i wartość tego pola jest używana jako sufiks dla wygenerowanej *ClientID* wartości. Zazwyczaj używasz rekord danych jako klucz podstawowy *ClientIDRowSuffix* wartości.
- *Dziedziczenie* — to ustawienie jest to domyślne zachowanie dla kontrolek; określa generowanie Identyfikator kontrolki jest taka sama jak jego elementu nadrzędnego.

Możesz ustawić *ClientIDMode* właściwości na poziomie strony. Definiuje domyślną *ClientIDMode* wartości dla wszystkich kontrolek na bieżącej stronie.

Wartość domyślna *ClientIDMode* wartość na poziomie strony jest *identyfikator automatyczny*i domyślnie *ClientIDMode* wartość na poziomie kontroli *Dziedzicz*. W rezultacie, jeśli nie ustawisz tę właściwość dowolnym miejscu w kodzie, wszystkie formanty będą domyślnie *identyfikator automatyczny* algorytmu.

Wartość zostanie ustawiona na poziomie strony *@ Page* dyrektywy, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Można również ustawić *ClientIDMode* wartości w pliku konfiguracji, na poziomie komputera (komputer) lub na poziomie aplikacji. Definiuje domyślną *ClientIDMode* ustawienie dla wszystkich kontrolek na wszystkich stronach w aplikacji. Jeśli ustawisz wartość na poziomie komputera definiuje domyślną *ClientIDMode* ustawienia dla wszystkich witryn sieci Web na tym komputerze. W poniższym przykładzie przedstawiono *ClientIDMode* ustawienia w pliku konfiguracji:

[!code-xml[Main](overview/samples/sample48.xml)]

Jak wspomniano wcześniej, wartość *ClientID* właściwość jest tworzony na podstawie kontenera nazewnictwa dla kontrolki nadrzędnej. W niektórych scenariuszach, na przykład w przypadku używania stron wzorcowych kontroli może wystąpić identyfikatory podobnie jak w poniższym renderowania kodu HTML:

[!code-html[Main](overview/samples/sample49.html)]

Mimo że *wejściowych* widocznego w znaczniku (z *TextBox* kontroli) jest tylko dwa kontenery nazewnictwa szczegółowego na stronie (zagnieżdżonego *ContentPlaceholder* formantów), ze względu na sposób, w jaki są przetwarzane strony wzorcowe efekt jest identyfikator kontrolki, jak pokazano poniżej:

[!code-console[Main](overview/samples/sample50.cmd)]

Ten identyfikator jest musi być unikatowy w stronę, ale jest niepotrzebnie długo w większości przypadków. Załóżmy, że mają zmniejszyć długość Identyfikatora renderowany i uzyskać większą kontrolę nad jak jest generowany identyfikator. (Na przykład, chcesz wyeliminować prefiksy "ctlxxx".) Najprostszym sposobem osiągnięcia tego jest ustawiając *ClientIDMode* właściwości, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample51.aspx)]

W tym przykładzie *ClientIDMode* właściwość jest ustawiona na *statyczne* dla najbardziej zewnętrzna *NamingPanel* elementu, a ustawienie *przewidywalny* Aby uzyskać wewnętrzny *NamingControl* elementu. Te ustawienia powodują w niej następujące znaczniki (pozostałej części strony i strony wzorcowej zakłada się, że takie same jak w poprzednim przykładzie):

[!code-html[Main](overview/samples/sample52.html)]

*Statyczne* ustawienie obowiązuje Resetowanie nazewnictwa hierarchii dla wszystkich formantów w najbardziej zewnętrznej *NamingPanel* elementu i eliminowania *ContentPlaceHolder* i *na nich* identyfikatory z wygenerowanego identyfikatora. ( *Nazwa* atrybutów elementów renderowanych pozostanie nienaruszona, tak normalnej funkcjonalności programu ASP.NET został zachowany na potrzeby zdarzeń, wyświetlanie stanu itd.) Resetowanie hierarchii nazewnictwa efektem jest to, że nawet wtedy, gdy przeniesiesz kod znaczników dla *NamingPanel* elementy z innym *ContentPlaceholder* kontrolki, identyfikatory klienta renderowanych pozostają takie same.

> [!NOTE]
> Należy pamiętać, że do Ciebie, aby upewnić się, że identyfikatory renderowanych kontrolki są unikatowe. Jeśli nie są one żadnej funkcji, która wymaga unikatowych identyfikatorów dla poszczególnych elementów kodu HTML, takich jak klient może przerwać *document.getElementById* funkcji.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Tworzenie przewidywalne identyfikatory klienta w formantach powiązanych z danymi

*ClientID* wartości, które są generowane dla kontrolek w kontrolce listy powiązanych z danymi, starszego algorytmu może być długi i nie są naprawdę przewidywalne. *ClientIDMode* funkcjonalność może pomóc w mieć większą kontrolę nad jak te identyfikatory są generowane.

Znaczniki w poniższym przykładzie zawiera *ListView* sterowania:

[!code-aspx[Main](overview/samples/sample53.aspx)]

W poprzednim przykładzie *ClientIDMode* i *RowClientIDRowSuffix* właściwości są ustawiane w znacznikach. *ClientIDRowSuffix* właściwość może być używana tylko w formantach powiązanych z danymi, a jego zachowanie różni się w zależności od tego, które określają, którego używasz. Różnice są one:

- *GridView* kontroli — można określić nazwę co najmniej jednej kolumny w źródle danych, które są łączone w czasie wykonywania można utworzyć klienta identyfikatorów. Na przykład jeśli ustawisz *RowClientIDRowSuffix* do "ProductName, ProductId", kontrolować identyfikatory dla wyrenderowane elementy będą miały format podobnie do poniższego:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* kontroli — można określić pojedynczej kolumny w źródle danych, która jest dołączana do identyfikatora klienta. Na przykład jeśli ustawisz *ClientIDRowSuffix* "ProductName" renderowanych kontroli identyfikatorów, będzie miała następujący format:

- [!code-console[Main](overview/samples/sample55.cmd)]

- W tym przypadku końcowe 1 jest tworzony na podstawie identyfikator produktu dla bieżącego elementu danych.

- *Elementu powtarzanego* kontroli — nie obsługuje tej kontrolki *ClientIDRowSuffix* właściwości. W *Repeater* kontrolki, indeks bieżącego wiersza jest używany. Kiedy używasz ClientIDMode = "Przewidywalny" with *elementu powtarzanego* kontrolować klienta identyfikatory są generowane, który ma następujący format:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Końcowych 0 jest indeks bieżącego wiersza.

*FormView* i *DetailsView* formanty są wyświetlane wiele wierszy, więc nie obsługują one *ClientIDRowSuffix* właściwości.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Trwały wybór wiersza w formantach danych

*GridView* i *ListView* formantów pozwolić użytkownikom, wybierz wiersz. W poprzednich wersjach programu ASP.NET wybór opiera się na indeks wiersza na stronie. Na przykład jeśli Wybierz trzeci element na stronie 1, a następnie przejść do strony 2, trzeci element na tej stronie jest zaznaczone.

Wybór utrwalonych początkowo była obsługiwana tylko w projektach dane dynamiczne w .NET Framework 3.5 SP1. Po włączeniu tej funkcji bieżącego wybranego elementu jest oparty na klucz danych dla elementu. Oznacza to, że jeśli wybierzesz trzeciego wiersza na stronie 1 i przejść do strony 2, nic nie jest zaznaczone na stronie 2. Jeśli można przenieść z powrotem do strony 1, nadal zaznaczone trzeciego wiersza. Wybór utrwalonych jest teraz obsługiwana dla *GridView* i *ListView* kontrolki we wszystkich projektach przy użyciu *EnablePersistedSelection* właściwości, jak pokazano w Poniższy przykład:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Formant wykresu programu ASP.NET

ASP.NET *wykresu* kontroli rozwija ofert wizualizacji danych w programie .NET Framework. Za pomocą *wykresu* kontrolki, można utworzyć strony ASP.NET, które jest intuicyjna i wizualnie atrakcyjne wykresów do kompleksowej analizy statystycznej lub finansowych. ASP.NET *wykresu* kontrolki została wprowadzona jako dodatek do wersji .NET Framework w wersji 3.5 z dodatkiem SP1 i jest częścią wersji .NET Framework 4.

Kontrolka zawiera następujące funkcje:

- 35 typy wykresów distinct.
- Nieograniczoną liczbę obszarów wykresu, tytuły, legendy i adnotacji.
- Szerokiej gamy ustawienia wyglądu dla wszystkich elementów wykresu.
- 3-obsługa większości typów wykresów.
- Etykiety inteligentne danych, które można automatycznie mieściła wokół punktów danych.
- Paski podziałów skali i logarytmicznej.
- Więcej niż 50 finansowych formuły do analizowania danych i transformacji, statystycznych.
- Proste powiązanie i modyfikowanie danych wykresu.
- Pomoc techniczna dla typowych formatów danych, takich jak daty, godziny i waluty.
- Obsługa interakcyjność i dostosowywania oparte na zdarzeniach, łącznie z klienta kliknij zdarzeń za pomocą interfejsu Ajax.
- Zarządzanie stanem.
- Przesyłanie strumieniowe binarnego.

Poniższe rysunki pokazują przykłady programu finansowego wykresów, które są produkowane przez formant wykresu programu ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Rysunek 2: Przykłady formantów ASP.NET wykresu

Aby więcej przykładów dotyczących sposobów używania formantu wykresu programu ASP.NET, Pobierz przykładowy kod na [przykłady środowiska do kontrolek wykresów Microsoft](https://go.microsoft.com/fwlink/?LinkId=128300) strony w witrynie MSDN w sieci Web. Więcej przykładów społeczności można znaleźć zawartości w [Forum formant wykresu](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Dodawanie kontrolki wykresu do strony ASP.NET

Poniższy przykład pokazuje, jak dodać *wykresu* kontrolki na stronie ASP.NET za pomocą znaczników. W tym przykładzie *wykresu* kontroli tworzy wykres kolumnowy dla punktów danych statycznych.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Korzystanie z wykresów 3W

*Wykresu* kontrolka zawiera *ChartAreas* kolekcji, która może zawierać *obszaru wykresu* obiekty, które określają właściwości obszarów wykresu. Na przykład, aby użyć 3W obszaru wykresu, należy użyć *Area3DStyle* właściwości, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Na poniższym rysunku przedstawiono wykresu 3D za pomocą czterech serii *pasek* typ wykresu.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Rysunek 3: 3-wykres słupkowy

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Przy użyciu podziałów skali i skali logarytmicznej

Podziały skali i skal logarytmicznych są dwa inne sposoby dodawania wiedzy do wykresu. Te funkcje są specyficzne dla każdej z osi obszaru wykresu. Na przykład aby użyć tych funkcji na podstawowej osi Y obszaru wykresu, należy użyć *AxisY.IsLogarithmic* i *ScaleBreakStyle* właściwości w *obszaru wykresu* obiektu. Poniższy fragment kodu pokazuje, jak na korzystanie z podziałów skali osi Y podstawowej.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Na poniższym rysunku przedstawiono osi Y z podziałów skali włączone.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Rysunek 4: Podziałów skali

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrowanie danych za pomocą kontrolki QueryExtender

Jest bardzo popularne zadania dla deweloperów, którzy tworzą stron sieci Web opartej na danych do filtrowania danych. To tradycyjnie została wykonana, tworząc *gdzie* klauzule w danych źródłowych kontrolek. Ta metoda może być skomplikowane, a w niektórych przypadkach *gdzie* składni nie zezwala na korzystanie z zalet pełnej funkcjonalności podstawowej bazy danych.

Aby łatwiej, filtrowanie nową *QueryExtender* formant został dodany w programie ASP.NET 4. Ten formant mogą być dodawane do *EntityDataSource* lub *LinqDataSource* formantów, aby filtrować dane zwracane przez te kontrolki. Ponieważ *QueryExtender* formant opiera się na LINQ, filtr jest stosowany na serwerze bazy danych przed wysłaniem danych do strony, co skutkuje bardzo wysoką wydajność operacji.

*QueryExtender* kontroli obsługuje szereg opcji filtrowania. W poniższych sekcjach opisano te opcje i przykłady ich zastosowania.

#### <a name="search"></a>Wyszukaj

Dla opcji wyszukiwania *QueryExtender* formant wykonuje wyszukiwanie w określonych dziedzinach. W poniższym przykładzie kontrolka używa tekst, który jest wprowadzana w kontroli TextBoxSearch i wyszukuje jego zawartość w `ProductName` i `Supplier.CompanyName` kolumny danych, który jest zwracany z *LinqDataSource* formant.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Zakres

Opcja zakresu jest podobna do opcji wyszukiwania, ale Określa pary wartości, aby zdefiniować zakres. W poniższym przykładzie *QueryExtender* kontrolować wyszukiwania `UnitPrice` kolumny w danych zwracanych przez *LinqDataSource* kontroli. Zakres są odczytywane z TextBoxFrom i TextBoxTo formantów na stronie.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Opcja wyrażenie właściwości pozwala zdefiniować porównanie wartości właściwości. Jeśli wyrażenie ma *true*, jest zwracany w danych, które jest sprawdzane. W poniższym przykładzie *QueryExtender* kontroli filtruje dane, porównując dane w `Discontinued` kolumny na wartość z formantu CheckBoxDiscontinued na stronie.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Ponadto można określić niestandardowe wyrażenie za pomocą *QueryExtender* kontroli. Ta opcja pozwala wywołać funkcję na stronie, na którym jest zdefiniowana logika filtr niestandardowy. Poniższy przykład pokazuje, jak deklaratywne określenie wyrażenia niestandardowego w *QueryExtender* kontroli.

[!code-aspx[Main](overview/samples/sample64.aspx)]

W poniższym przykładzie pokazano funkcję niestandardową, która jest wywoływana przez *QueryExtender* kontroli. W tym przypadku, zamiast korzystać z zapytania bazy danych, które obejmuje *gdzie* klauzuli kod używa zapytania LINQ do filtrowania danych.

[!code-csharp[Main](overview/samples/sample65.cs)]

W poniższych przykładach pokazano tylko jedno wyrażenie, które są używane w *QueryExtender* formantu w czasie. Jednak może zawierać wiele wyrażeń wewnątrz *QueryExtender* kontroli.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>HTML zakodowane kodu wyrażeń

Niektóre witryny ASP.NET (w szczególności z ASP.NET MVC) w dużym stopniu polegają na temat korzystania z `<%` =  `expression %>` składni (często nazywane "code nuggets") można zapisać jakiś tekst do odpowiedzi. Należy użyć wyrażeń kodu, jest można łatwo zapomnieć o — kodowanie HTML wprowadzania tekstu, jeśli tekst pochodzi od użytkownika, jego może zamykaj strony do ataku XSS (skryptów krzyżowych).

ASP.NET 4 wprowadzono następujące nowe składni wyrażeń kodu:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Ta składnia używa kodowanie HTML domyślnie, podczas zapisywania odpowiedzi. To nowe wyrażenie skutecznie tłumaczy do następujących:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Na przykład &lt;%: % żądania ["UserInput"]&gt; wykonuje kodowanie HTML na wartość *żądania ["UserInput"]*.

Celem tej funkcji jest umożliwia Zastąp wszystkie wystąpienia zmiennej stara składnia za pomocą nowej składni, dzięki czemu nie muszą podjąć decyzję co do użycia na każdym etapie. Istnieją przypadki, w których tekst będący danych wyjściowych jest należy traktować jako HTML lub jest już zaszyfrowana, w którym to przypadku może to spowodować podwójne kodowania.

Dla tych przypadków, platformy ASP.NET 4 wprowadzono nowy interfejs *IHtmlString*, wraz z konkretną implementację *HtmlString*. Wystąpienia te typy umożliwiają wskazują, że wartość zwracana jest już poprawnie zaszyfrowana (lub badany w przeciwnym razie) do wyświetlania w postaci kodu HTML i, dlatego wartość nie powinna być kodowany w formacie HTML ponownie. Na przykład, następujący nie powinny być (i nie jest) kodowania HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Metody pomocników platformy ASP.NET MVC 2 zostały zaktualizowane do pracy z tej nowej składni, tak, aby nie są one double zakodowane, ale tylko po uruchomieniu programu ASP.NET 4. Ta nowa składnia nie działa podczas uruchamiania aplikacji przy użyciu platformy ASP.NET 3.5 z dodatkiem SP1.

Należy pamiętać, że nie gwarantuje ochronę przed atakami XSS. Na przykład HTML, który używa wartości atrybutów, które nie znajdują się w znaki cudzysłowu mogą zawierać dane wejściowe użytkownika, które są nadal podatne. Należy pamiętać, że dane wyjściowe kontrolek ASP.NET i pomocników platformy ASP.NET MVC zawsze zawiera wartości atrybutów w cudzysłowie, co jest zalecane podejście.

Podobnie ta składnia nie kodowanie JavaScript, takie jak podczas tworzenia ciągu języka JavaScript, na podstawie danych wejściowych użytkownika.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Zmiany szablonu projektu

We wcześniejszych wersjach programu ASP.NET, przy użyciu programu Visual Studio Utwórz nowy projekt witryny sieci Web lub projekt aplikacji sieci Web, wynikowy projekty zawierają tylko strony Default.aspx, domyślny `Web.config` pliku, a `App_Data` folderu, jak pokazano w poniższym Ilustracja:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Program Visual Studio obsługuje również pusta witryna sieci Web typu projektu, który nie zawiera plików na wszystkich, jak pokazano na poniższej ilustracji:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Wynik jest, że dla początkujących, jest bardzo mało wskazówki na temat tworzenia aplikacji sieci Web w środowisku produkcyjnym. W związku z tym platformy ASP.NET 4 wprowadza trzy nowe szablony, jeden dla pusty projekt aplikacji sieci Web i jeden dla projektu witryny sieci Web i aplikacji sieci Web.

#### <a name="empty-web-application-template"></a>Pustego szablonu sieci Web aplikacji

Jak sugeruje nazwa, szablon pusta aplikacja sieci Web jest stripped-down projekt aplikacji sieci Web. Możesz wybierać tego szablonu projektu okno dialogowe nowego projektu programu Visual Studio, jak pokazano na poniższej ilustracji:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image8.png))

Kiedy tworzysz pustą aplikację sieci Web platformy ASP.NET, program Visual Studio tworzy następujące układu foldera:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Jest to podobne do układu pusta witryna internetowa z wcześniejszych wersji programu ASP.NET, z jednym wyjątkiem. W programie Visual Studio 2010, pusta aplikacja sieci Web i pusta witryna internetowa projekty zawierają następujące minimalne `Web.config` pliku, który zawiera informacje używane przez program Visual Studio do identyfikowania umożliwiająca będącego celem projektu:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Jeśli ta usługa nie *targetFramework* właściwość, wartością domyślną jest Visual Studio w celu zachowania zgodności podczas otwierania starsze aplikacje przeznaczone dla programu .NET Framework 2.0.

#### <a name="web-application-and-web-site-project-templates"></a>Szablony projektów witryny sieci Web i aplikacji sieci Web

Pozostałe dwie nowe szablony projektów, które są dostarczane z programem Visual Studio 2010 zawierają istotne zmiany. Na poniższej ilustracji przedstawiono układ projektu, który jest tworzony podczas tworzenia nowego projektu aplikacji sieci Web. (Układ dla projektu witryny sieci Web jest niemal identyczne).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projekt zawiera wiele plików, które nie zostały utworzone we wcześniejszych wersjach. Ponadto nowy projekt aplikacji sieci Web skonfigurowano funkcję podstawowego członkostwa, która pozwala szybko rozpocząć pracę w zabezpieczaniu dostępu do nowej aplikacji. Ze względu na to włączenie `Web.config` pliku dla nowego projektu zawiera wpisy, które są używane do konfigurowania członkostwa, ról i profilów. W poniższym przykładzie przedstawiono `Web.config` pliku dla nowego projektu aplikacji sieci Web. (W tym przypadku *roleManager* jest wyłączone.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image14.png))

Projekt zawiera również sekundy `Web.config` w pliku `Account` katalogu. Drugi plik konfiguracji umożliwia bezpieczny dostęp do strony ChangePassword.aspx niezarejestrowanej wśród użytkowników. W poniższym przykładzie pokazano zawartość drugiego `Web.config` pliku.

![](overview/_static/image15.png)

Strony tworzone domyślnie w nowe szablony projektów również zawierać więcej zawartości niż w poprzednich wersjach. Projekt zawiera domyślną stronę wzorcową i pliku CSS, a domyślna strona (Default.aspx) jest domyślnie skonfigurowana do użycia strony wzorcowej. Wynik jest po uruchomieniu aplikacji sieci Web lub witryny sieci Web po raz pierwszy, domyślna strona (macierzysty) już funkcjonalności. W rzeczywistości jest podobny do domyślnej strony widoczny po uruchomieniu nowej aplikacji platformy MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image18.png))

Te zmiany w szablonach projektu jest przedstawienie wskazówek dotyczących sposobu rozpoczęcia tworzenia nowej aplikacji sieci Web. Za pomocą semantycznie poprawny, ścisłe XHTML 1.0 zgodne kod znaczników i układ, który jest określony przy użyciu CSS stron w szablonach reprezentują najlepsze rozwiązania dotyczące tworzenia aplikacji sieci Web platformy ASP.NET 4. Domyślne strony mają także układ dwie kolumny, który można łatwo dostosować.

Załóżmy, że dla nowej aplikacji sieci Web chcesz zmienić niektóre kolory i Wstaw logo firmy zamiast logo Moja aplikacja platformy ASP.NET. Aby to zrobić, Utwórz nowy katalog w obszarze `Content` do przechowywania obrazu logo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Aby dodać obraz do strony, możesz następnie otworzyć `Site.Master` pliku, gdzie tekst Moja aplikacja platformy ASP.NET jest zdefiniowana i zastąp go wartością *obraz* elementu którego *src* atrybut jest ustawiony na nowe logo obraz, jak w poniższym przykładzie:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image22.png))

Możesz następnie przejść do pliku Site.css i modyfikować definicje klas CSS, aby zmienić kolor tła strony, a także nagłówka.

Wynikiem tych zmian jest, że można wyświetlić dostosowanej strony głównej przy niewielkim nakładzie pracy:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Ulepszenia CSS

Jedną z głównych obszarów roboczych w programie ASP.NET 4 został celu renderowania elementów HTML, który jest zgodny z najnowszymi standardami HTML. Obejmuje to zmiany jak formanty serwera sieci Web platformy ASP.NET używaj stylów CSS.

#### <a name="compatibility-setting-for-rendering"></a>Ustawienie zgodności w celu renderowania

Domyślnie, gdy aplikacja sieci Web lub witryny sieci Web jest przeznaczony dla .NET Framework 4 *controlRenderingCompatibilityVersion* atrybutu *stron* element jest ustawiony na wartość "4.0". Ten element jest zdefiniowana na poziomie maszyny `Web.config` pliku i domyślnie ma zastosowanie do wszystkich aplikacji platformy ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Wartość *controlRenderingCompatibility* jest ciąg, który umożliwia potencjalnych nowych definicji wersji w przyszłych wersjach. W bieżącej wersji obsługiwane są następujące wartości dla tej właściwości:

- "3.5". To ustawienie wskazuje starszych renderowania i znaczników. Znaczników renderowania kontrolki jest zgodne z poprzednimi wersjami 100%, a ustawienie *xhtmlConformance* właściwość zostanie uznane.
- "4.0". Jeśli właściwość ma to ustawienie, formanty serwera sieci Web programu ASP.NET, wykonaj następujące czynności:
- *XhtmlConformance* właściwości jest zawsze traktowany jako "Strict". W rezultacie formantów renderowania kodu znaczników XHTML 1.0 Strict.
- Wyłączenie formantów bez danych wejściowych nie jest już renderuje nieprawidłowy style.
- *DIV* elementy wokół ukryte pola mają różne teraz, dzięki czemu nie zakłócają reguły CSS utworzone przez użytkownika.
- Formanty menu renderowania kodu znaczników semantycznie prawidłowe i zgodne z wytyczne dotyczące ułatwień dostępu.
- Formanty sprawdzania poprawności nie są renderowane style wbudowane.
- Określa, który wcześniej renderowany border = "0" (formantów, które wynikają z platformy ASP.NET *tabeli* kontroli i platformy ASP.NET *obrazu* kontroli) nie jest już renderowania tego atrybutu.

#### <a name="disabling-controls"></a>Wyłączenie formantów

W ASP.NET 3.5 z dodatkiem SP1 i wcześniejszymi wersjami, struktura renderuje *wyłączone* atrybutu w kod znaczników HTML, aby każdy formant, którego *włączone* właściwością *false*. Jednak zgodnie ze specyfikacją HTML 4.01, tylko *wejściowych* elementy powinny mieć tego atrybutu.

W ramach platformy ASP.NET 4, możesz ustawić *controlRenderingCompatabilityVersion* właściwość "3.5", jak w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample70.xml)]

Możesz utworzyć kod znaczników dla *etykiety* kontroli podobnie do poniższego, co spowoduje wyłączenie kontrolki:

[!code-aspx[Main](overview/samples/sample71.aspx)]

*Etykiety* formant będzie renderować poniższy kod HTML:

[!code-html[Main](overview/samples/sample72.html)]

W ramach platformy ASP.NET 4, możesz ustawić *controlRenderingCompatabilityVersion* "4.0". W takim przypadku czynność reguluje tylko tym renderowania *wejściowych* elementy będą renderowane *wyłączone* atrybutu, gdy formantu *włączone* właściwość jest ustawiona na *false* . Formanty, które nie są renderowane HTML *wejściowych* zamiast renderowania elementów *klasy* atrybut, który odwołuje się do klasy CSS używanej do definiowania wyłączone wyglądu kontrolki. Na przykład *etykiety* kontrolki wyświetlane we wcześniejszym przykładzie wygeneruje następujący kod:

[!code-html[Main](overview/samples/sample73.html)]

Wartość domyślna dla klasy, która jest określona dla tej kontrolki jest "aspNetDisabled". Jednak można zmienić tej wartości domyślnej przez ustawienie statycznego *DisabledCssClass* właściwość statyczna *WebControl* klasy. Dla deweloperów kontroli zachowania do użycia dla określonej kontrolki można także definiować przy użyciu *SupportsDisabledAttribute* właściwości.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ukrywanie div elementy wokół ukryte pola

ASP.NET 2.0 i nowszych wersjach Renderuj ukryte pola specyficzne dla systemu (takich jak *ukryte* element używany do przechowywania informacji o stanie widoku) wewnątrz *div* element, aby można było zgodne z normą XHTML. Jednak może to spowodować problem podczas reguły CSS ma wpływ na *div* elementów na stronie. Na przykład może to skutkować linię jednego piksela znajdujących się na stronie około ukryte *div* elementów. W technologii ASP.NET 4 *div* elementy, które należy ująć ukryte pola wygenerowane przez platformę ASP.NET Dodaj odwołanie do klasy CSS, jak w poniższym przykładzie:

[!code-html[Main](overview/samples/sample74.html)]

Można zdefiniować klasę CSS, która ma zastosowanie tylko do *ukryte* elementy, które są generowane przez platformę ASP.NET, jak w poniższym przykładzie:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Renderowanie tabeli zewnętrznej dla kontrolki z szablonami

Domyślnie następujące formanty serwera sieci Web platformy ASP.NET, które obsługują szablonów automatycznie zostaną opakowane w tabeli zewnętrznej, która umożliwia zastosowanie style wbudowane:

- *FormView*
- *Zaloguj się*
- *PasswordRecovery*
- *ChangePassword*
- *Kreator*
- *CreateUserWizard*

Nową właściwość o nazwie *RenderOuterTable* został dodany do tych kontrolek, które umożliwia tabeli zewnętrznej do usunięcia z kodu znaczników. Na przykład rozważmy następujący przykład *FormView* sterowania:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Ten kod znaczników renderuje następujące dane wyjściowe do strony, która zawiera tabelę HTML:

[!code-html[Main](overview/samples/sample77.html)]

Aby zapobiec są renderowane w tabeli, możesz ustawić *FormView* kontrolki *RenderOuterTable* właściwości, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Poprzedni przykład renderuje następujące dane wyjściowe bez *tabeli*, *tr*, i *td* elementy:

> Zawartość


To ulepszenie może ułatwić styl zawartość kontrolki z CSS, ponieważ brak tagów nieoczekiwany są renderowane przez kontrolkę.

> [!NOTE]
> Uwaga Ta zmiana wyłącza obsługę funkcji automatycznego formatowania w Projektancie Visual Studio 2010, ponieważ nie ma już *tabeli* element, który może obsługiwać atrybuty stylu, które są generowane przez opcję automatycznego formatowania.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Ulepszenia formantów ListView

*ListView* kontroli jest łatwiejsze do użycia w ASP.NET 4. Starszą wersję kontrolki wymaga, aby określić szablon układu, który zawiera formant serwera za pomocą znanego identyfikatora. Poniższy kod przedstawia typowym przykładem przedstawiającym sposób użycia *ListView* sterowania w programie ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

W technologii ASP.NET 4 *ListView* formantu nie wymaga szablon układu. Znaczniki w poprzednim przykładzie można zastąpić za pomocą następujących znaczników:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList i ulepszenia RadioButtonList formantów

W programie ASP.NET 3.5, można określić układ *CheckBoxList* i *RadioButtonList* przy użyciu dwóch następujących ustawień:

- *Przepływ*. Kontrolka renderuje *span* elementów do jego zawartością.
- *Tabela*. Kontrolka renderuje *tabeli* elementu do jego zawartością.

Poniższy przykład pokazuje kod znaczników dla każdego z tych kontrolek.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Domyślnie przez formanty renderowania elementów HTML podobny do następującego:

[!code-html[Main](overview/samples/sample82.html)]

Ponieważ te kontrolki zawiera listę elementów do renderowania elementów HTML semantycznie poprawne, powinny one zostać wyrenderowane ich zawartość przy użyciu listy HTML (*li*) elementów. To ułatwia użytkownikom, którzy odczyt stron sieci Web przy użyciu technologii pomocniczej i ułatwia kontrolki stylu przy użyciu CSS.

W technologii ASP.NET 4 *CheckBoxList* i *RadioButtonList* formanty obsługują następujące nowe wartości dla *RepeatLayout* właściwości:

- *OrderedList* — zawartość jest wyświetlana jako *li* elementów w obrębie *ol* elementu.
- *UnorderedList* — zawartość jest wyświetlana jako *li* elementów w obrębie *ul* elementu.

Poniższy przykład przedstawia sposób używania tych nowych wartości.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Poprzedni kod znaczników generuje poniższy kod HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Należy pamiętać, jeśli ustawisz *RepeatLayout* do *OrderedList* lub *UnorderedList*, *RepeatDirection* właściwości nie można używać i będzie Jeśli właściwość została ustawiona w ramach Twojej znaczników lub innego kodu, należy zgłosić wyjątek w czasie wykonywania. Właściwość będzie mieć żadnej wartości, ponieważ układ wizualizacji tych formantów jest zdefiniowany w zamian CSS.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Ulepszenia kontroli menu

Przed platformy ASP.NET 4 *Menu* kontroli renderowane serii tabel HTML. Utrudniało na stosowanie stylów CSS poza ustawienia właściwości wbudowanych i nie jest również ze standardami ułatwień dostępu.

ASP.NET 4 kontrolka renderuje HTML za pomocą semantyki kod znaczników, który składa się z nieuporządkowaną listę i elementy listy. W poniższym przykładzie pokazano znaczników strony ASP.NET dla *Menu* kontroli.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Gdy powoduje wyświetlenie strony, formant generuje poniższy kod HTML ( *onclick* kodu została pominięta dla jasności):

[!code-html[Main](overview/samples/sample86.html)]

Oprócz ulepszeń renderowania została ulepszona nawigacji za pomocą klawiatury menu przy użyciu przystawki Zarządzanie fokus. Gdy *Menu* formant uzyskuje fokus, możesz użyć klawiszy strzałek, można przejść elementów. *Menu* kontroli teraz również dołącza dostępny bogaty role aplikacji (ARIA) internet i atrybuty następującymi[następującą](http://www.w3.org/TR/wai-aria-practices/#menu "wytycznych Menu ARIA")na lepsze ułatwienia dostępu.

Style formantu menu — zostaną zrenderowane w bloku stylu w górnej części strony, a nie zgodnie ze renderowanych elementów HTML. Jeśli chcesz wykonać pełną kontrolę nad stylu dla formantu, możesz ustawić nowy *IncludeStyleBlock* właściwości *false*, w którym to przypadku nie jest emitowane bloku stylu. Jednym ze sposobów, aby używać tej właściwości jest używać funkcji automatycznego formatowania w projektanta programu Visual Studio, aby ustawić wygląd menu. Można następnie uruchomienia strony, oprogramowanie typu open source strony i następnie skopiuj bloku stylu renderowanych do zewnętrznego pliku CSS. W programie Visual Studio, Cofnij zestawu i stylów *IncludeStyleBlock* do *false*. Powoduje to, że wygląd menu jest definiowana za pomocą stylów w zewnętrznym arkuszu stylów.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Kreator i CreateUserWizard formantów

ASP.NET *kreatora* i *CreateUserWizard* formantów obsługuje szablonów, które pozwalają zdefiniować HTML, że są one renderowane. (*CreateUserWizard* pochodzi od klasy *kreatora*.) W poniższym przykładzie pokazano znaczniki dla w pełni szablonem *CreateUserWizard* sterowania:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Kontrolka renderuje HTML podobny do następującego:

[!code-html[Main](overview/samples/sample88.html)]

W programie ASP.NET 3.5 z dodatkiem SP1, chociaż można zmienić zawartość szablonu nadal masz ograniczone kontroli nad sposobem *kreatora* kontroli. W platformie ASP.NET 4, możesz utworzyć *LayoutTemplate* szablonu i Wstaw *symbolu zastępczego* kontroluje (przy użyciu nazw zastrzeżonych), aby określić, jak chcesz, aby *formantu kreatora* do renderowania. Poniższy przykład przedstawia to:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Przykład zawiera następujące nazwane symbole zastępcze w wywołaniach *LayoutTemplate* elementu:

- *headerPlaceholder* — w czasie wykonywania, zostanie zastąpiony przez zawartość *HeaderTemplate* elementu.
- *sideBarPlaceholder* — w czasie wykonywania, zostanie zastąpiony przez zawartość *SideBarTemplate* elementu.
- *wizardStepPlaceHolder* — w czasie wykonywania, zostanie zastąpiony przez zawartość *WizardStepTemplate* elementu.
- *navigationPlaceholder* — w czasie wykonywania, to zastępuje żadnych szablonów nawigacji, które zostały zdefiniowane.

Znaczniki w przykładzie, który używa symbole zastępcze Renderuje poniższy kod HTML (bez zawartość faktycznie są zdefiniowane w szablonach):

[!code-html[Main](overview/samples/sample90.html)]

Tylko kod HTML, który jest teraz nie zdefiniowane przez użytkownika jest *span* elementu. (Który w przyszłości zamierzamy wersji, nawet *span* element nie będzie renderowana.) To teraz daje pełną kontrolę nad praktycznie całość zawartości, który jest generowany przez *kreatora* kontroli.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC została wprowadzona jako platforma dodatek do programu ASP.NET 3.5 z dodatkiem SP1 w marca 2009 roku. Program Visual Studio 2010 obejmuje platformy ASP.NET MVC 2, która zawiera nowe funkcje i możliwości.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Obszary pomocy technicznej

Obszary umożliwiają widoków i kontrolerów grupy na sekcje dużych aplikacji względna od siebie odizolowane od innych sekcji. Każdy z tych obszarów można zaimplementować jako osobne projekt platformy ASP.NET MVC, który można odwoływać się do aplikacji głównej. To pomaga w zarządzaniu złożonością podczas kompilowania dużych aplikacji i ułatwia wiele zespołów musi współpracować ze sobą na pojedynczej aplikacji.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Obsługa sprawdzania poprawności atrybutów adnotacji danych

*DataAnnotations* atrybutów umożliwiają dołączanie logikę walidacji do modelu przy użyciu atrybutów metadanych. *DataAnnotations* atrybuty zostały wprowadzone w programie ASP.NET Dynamic Data w ASP.NET 3.5 z dodatkiem SP1. Te atrybuty są integrowane domyślnego integratora modelu i stanowić sposób opartą na metadanych, aby sprawdzić poprawność danych wejściowych użytkownika.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Pomocników szablonu

Pomocników szablonu pozwalają na automatyczne kojarzenie Edytuj i Wyświetl szablony z typami danych. Na przykład, można użyć pomocnika szablonu do określenia automatycznie renderowania elementu selektora daty interfejsu użytkownika dla *System.DateTime* wartość. Jest to podobne do szablonów pól w ASP.NET Dynamic Data.

*Html.EditorFor* i *Html.DisplayFor* metody pomocnika ma wbudowaną obsługę dla Renderowanie danych w warstwie standardowa typów obiektów również tak złożonego z wieloma właściwościami. Mogą również dostosować renderowania, umożliwiając stosowanie adnotacji danych atrybutów, takich jak *DisplayName* i *ScaffoldColumn* do *ViewModel* obiektu.

Często chcesz dostosować dane wyjściowe z jeszcze bardziej wątków interfejsu użytkownika i mają pełną kontrolę nad czym jest generowany. *Html.EditorFor* i *Html.DisplayFor* metody pomocnika to umożliwić przy użyciu mechanizmu szablonów, które umożliwiają definiowanie Szablony zewnętrzne, które można przesłonić i kontroli danych wyjściowych renderowane. Szablony mogą być renderowane indywidualnie dla klasy.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dane dynamiczne

Dane dynamiczne została wprowadzona w wersji .NET Framework 3.5 SP1 w połowie 2008. Ta funkcja zawiera wiele udoskonaleń do tworzenia aplikacji opartych na danych, w tym następujące czynności:

- RAD środowisko do szybkiego tworzenia witryny sieci Web opartej na danych.
- Automatyczna Walidacja opiera się na ograniczenia zdefiniowane w modelu danych.
- Możliwość łatwego zmiany znaczników, który jest generowany dla pól w *GridView* i *DetailsView* kontrolek przy użyciu szablonów pól, które są częścią projektu danych dynamicznych.

> [!NOTE]
> Należy pamiętać, aby uzyskać więcej informacji, zobacz [dokumentacji dane dynamiczne](https://msdn.microsoft.com/library/cc488545.aspx) w bibliotece MSDN.


Dla platformy ASP.NET 4 danych dynamicznych zostało rozszerzone, aby zaoferować deweloperom jeszcze większe możliwości szybkiego tworzenia witryn internetowych opartych na danych.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Włączanie danych dynamicznych dla istniejących projektów

Funkcje dane dynamiczne, które w .NET Framework 3.5 SP1 wprowadzone nowe funkcje, takie jak następujące:

- Szablony pola — te zapewniają szablony na podstawie typu danych dla formantów powiązanych z danymi. Pole Szablony zapewniają prostszy sposób, aby dostosować wygląd kontrolki danych niż przy użyciu pól szablonu dla każdego pola.
- Sprawdzanie poprawności — dane dynamiczne umożliwia Użyj atrybutów na klas danych, aby określić sprawdzania poprawności dla typowych scenariuszy, takich jak wymagane pola, sprawdzanie zakresu, kontrola typów, wzorzec dopasowywania, za pomocą wyrażeń regularnych i Walidacja niestandardowa. Sprawdzanie poprawności jest wymuszana przez formanty danych.

Jednak te funkcje miały następujące wymagania:

- Warstwa dostępu do danych było opierać się na platformy Entity Framework x lub LINQ to SQL.
- Jedyne dane źródła kontrolki obsługiwane w przypadku te funkcje zostały *EntityDataSource* lub *LinqDataSource* kontrolki.
- Projekt sieci Web, które zostały utworzone przy użyciu danych dynamicznych lub szablonów dynamiczne jednostek danych, aby mogła mieć wszystkie pliki, które są wymagane do obsługi funkcji wymagane funkcje.

Główne celem dane dynamiczne obsługi w technologii ASP.NET 4 jest włączyć nowych funkcji danych dynamicznych dla dowolnej aplikacji platformy ASP.NET. Poniższy przykład pokazuje kod znaczników dla formantów, które można wykorzystać dane dynamiczne funkcje w istniejącej strony.

[!code-aspx[Main](overview/samples/sample91.aspx)]

W kodzie strony musi dodać poniższego kodu, aby włączyć obsługę danych dynamicznych dla tych formantów:

[!code-csharp[Main](overview/samples/sample92.cs)]

Gdy *GridView* kontrolka jest w trybie edycji, dane dynamiczne automatycznie sprawdza, czy wprowadzone dane jest w nieprawidłowym formacie. Jeśli nie jest dostępne, jest wyświetlany komunikat o błędzie.

Ta funkcja zapewnia także inne korzyści, takie jak możliwość określenia domyślnego trybu wstawiania wartości. Bez danych dynamicznych do zaimplementowania wartość domyślną dla pola, należy dołączyć do zdarzenia, zlokalizuj kontrolkę (przy użyciu *FindControl*) i określanie jego wartości. W technologii ASP.NET 4 *EnableDynamicData* wywołanie obsługuje drugi parametr, który umożliwia przekazywanie wartości domyślne dla dowolnego pola w obiekcie, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Składnia deklaratywne kontrolki Menedżera danych dynamicznych

*Menedżera danych dynamicznych* formant został rozszerzony tak, aby można go skonfigurować w sposób deklaratywny, podobnie jak w przypadku większości kontrolek w ASP.NET, a nie tylko w kodzie. Kod znaczników dla *Menedżera danych dynamicznych* kontrolka wygląda następująco:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Ten kod znaczników umożliwia zachowanie danych dynamicznych dla formantu GridView1, do którego odwołuje się do *DataControls* części *Menedżera danych dynamicznych* kontroli.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Szablony jednostki

Szablony jednostki oferują nowy sposób, aby dostosować układ danych bez konieczności tworzenia niestandardowej strony. Stronie Szablony *FormView* kontroli (zamiast *DetailsView* kontrolować, jak używane w szablony stron w starszych wersjach danych dynamicznych) i *DynamicEntity* formant do renderowania szablonów jednostki. Zapewnia większą kontrolę nad znacznikami, który jest renderowany przez dane dynamiczne.

Na poniższej liście przedstawiono nowy układ katalogu projektu, który zawiera szablony jednostki:

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate` Katalogu zawiera szablony służące do wyświetlania obiekty modelu danych. Domyślnie obiekty są renderowane przy użyciu `Default.ascx` szablonu, który zawiera kod znaczników, który wygląda podobnie jak znaczników utworzone przez *DetailsView* formant używany przez dynamicznych danych w programie ASP.NET 3.5 z dodatkiem SP1. W poniższym przykładzie pokazano kod znaczników dla `Default.ascx` sterowania:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Szablony domyślne można edytować w taki sposób, aby zmienić wygląd i działanie dla całej lokacji. Istnieją szablony do wyświetlania, edytowania i operacje wstawiania. Nowe szablony można dodać na podstawie nazwy obiektu danych w celu zmiany wyglądu i działania tylko jeden typ obiektu. Na przykład można dodać następującego szablonu:

[!code-console[Main](overview/samples/sample97.cmd)]

Szablon może zawierać następujące znaczniki:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Nowe szablony jednostki są wyświetlane na stronie za pomocą nowego *DynamicEntity* kontroli. W czasie wykonywania ten formant jest zastępowany zawartość szablonu jednostki. Ilustruje poniższy kod znaczników *FormView* w kontrolce `Detail.aspx` szablon strona, która używa tego szablonu jednostki. Zwróć uwagę *DynamicEntity* elementem w znacznikach.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nowe szablony pola dla adresów URL i adresów E-mail

ASP.NET 4 wprowadzono dwa nowe szablony wbudowane pole `EmailAddress.ascx` i `Url.ascx`. Te szablony są używane dla pól, które są oznaczone jako *EmailAddress* lub *adresu Url* z *DataType* atrybutu. Aby uzyskać *EmailAddress* obiektów, pole jest wyświetlane jako hiperłącze, która jest tworzona przy użyciu *mailto:* protokołu. Gdy użytkownik kliknie link, otwiera klienta poczty e-mail użytkownika i tworzy szkielet wiadomość. Obiekty wpisanych w formie *adresu Url* są wyświetlane jako zwykły hiperłącza.

Poniższy przykład pokazuje, jak pola będą oznaczone.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Tworzenie łączy z formantem DynamicHyperLink

Dane dynamiczne używają nowej funkcji routingu, który został dodany do programu .NET Framework 3.5 SP1 w celu kontrolowania adresów URL, które użytkownicy końcowi widzą podczas uzyskiwania dostępu do witryny sieci Web. Nowy *DynamicHyperLink* kontroli można łatwo tworzyć łącza do stron w witryna danych dynamicznych. Poniższy przykład pokazuje, jak używać *DynamicHyperLink* sterowania:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Ten kod znaczników tworzy łącze, które wskazuje na stronę listy `Products` tabela oparta na trasach, które są zdefiniowane w `Global.asax` pliku. Formant automatycznie używa domyślnej nazwy tabeli, na podstawie strony danych dynamicznych.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Obsługa dziedziczenia w modelu danych

Entity Framework i LINQ to SQL obsługuje dziedziczenia w swoich modelach danych. Na przykład może być bazy danych zawierającej `InsurancePolicy` tabeli. Może także zawierać `CarPolicy` i `HousePolicy` tabele, które mają te same pola jako `InsurancePolicy` , a następnie dodać więcej pól. Dane dynamiczne został zmodyfikowany, aby zrozumieć dziedziczonych obiektów w modelu danych i obsługę tworzenia szkieletów tabel dziedziczonych.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Obsługa relacji wiele do wielu (tylko platformy Entity Framework)

Entity Framework zawiera szeroką obsługę relacji wiele do wielu między tabelami, które jest implementowany przez udostępnianie relację jako kolekcja w *jednostki* obiektu. Nowe `ManyToMany.ascx` i `ManyToMany_Edit.ascx` szablony pola zostały dodane do zapewnienia obsługi wyświetlania i edytowania danych, który jest używany w relacji wiele do wielu.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nowe atrybuty do wyświetlenia sterowania i pomocy technicznej wyliczenia

*DisplayAttribute* została dodana do zapewniają dodatkową kontrolę nad jak pola są wyświetlane. *DisplayName* atrybut we wcześniejszych wersjach programu danych dynamicznych, które pozwala zmienić nazwę, która jest używana jako podpis dla pola. Nowy *DisplayAttribute* klasy pozwala określić dodatkowe opcje wyświetlania pola, na przykład kolejność, w których pole jest wyświetlane i tego, czy pole będzie służyć jako filtr. Ten atrybut również można kontrolować niezależnie od nazwy używanego do etykiet w *GridView* kontrolować nazwę używaną w *DetailsView* kontrolować, tekst pomocy dla pola i znak wodny używane na potrzeby pole (Jeżeli pole przyjmuje tekst wejściowy).

*EnumDataTypeAttribute* klasa została dodana do umożliwiają mapowanie pól do wyliczenia. Po zastosowaniu tego atrybutu do pola, określasz typ wyliczenia. Dane dynamiczne używają nowej `Enumeration.ascx` pola szablonu do tworzenia interfejsu użytkownika do wyświetlania i edytowania wartości wyliczenia. Szablon mapuje wartości z bazy danych, które mają nazwy w wyliczeniu.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Rozszerzona obsługa filtry

Dynamiczne 1.0 dane są dostarczane z programem wbudowane filtry logiczne kolumny i kolumny klucza obcego. Filtry nie zezwolił na określenie, czy zostały one wyświetlane, czy w jakiej kolejności były wyświetlane. Nowy *DisplayAttribute* atrybut adresy oba te problemy, umożliwiając kontrolę tego, czy kolumna jest wyświetlana jako filtr i w jakiej kolejności będą wyświetlane.

Dodatkowe ulepszenie jest, że Obsługa filtrowania został ponownie[zapisywane do używania nowych](#0.2__QueryExtender "_QueryExtender") funkcji formularzy sieci Web. Dzięki temu można tworzyć filtry bez konieczności znajomości kontroli źródła danych, który filtry, które będą używane z. Wraz z tych rozszerzeń filtry mają również zostały przekształcane w szablonu kontrolki, co pozwala na dodawanie nowych. Na koniec *DisplayAttribute* klasy wymienione wcześniej umożliwia domyślny filtr do zastąpienia w taki sam sposób *UIHint* umożliwia domyślnego szablonu pola kolumna, która ma zostać zastąpiona.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 w sieci Web ulepszeń dotyczących programowania

Web development w Visual Studio 2010 został rozszerzony o większą zgodność CSS, zwiększenia produktywności przy użyciu fragmentów kodu znaczników HTML i ASP.NET oraz nowe dynamiczne IntelliSense JavaScript.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Zgodność ulepszone arkusza CSS

Projektant Visual Web Developer w Visual Studio 2010 został zaktualizowany tak, aby zwiększyć zgodność ze standardami CSS 2.1. Projektant lepiej zachowuje spójności źródła HTML i jest bardziej niezawodna niż w poprzednich wersjach programu Visual Studio. Kulisy architektury również wprowadzono ulepszenia tego spowoduje lepszego włączania przyszłe rozszerzenia będą miały renderowania, układ i użytkowanie.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML i JavaScript fragmentów kodu

W edytorze HTML IntelliSense automatyczne uzupełnianie nazwy tagów. Funkcja fragmenty kodu IntelliSense automatyczne uzupełnianie całych tagów i innych. W programie Visual Studio 2010 fragmenty kodu IntelliSense są obsługiwane dla języka JavaScript, wraz z C# i Visual Basic, które były obsługiwane we wcześniejszych wersjach programu Visual Studio.

Program Visual Studio 2010 zawiera ponad 200 fragmentami kodu, które ułatwiają Autouzupełnianie wspólnego platformy ASP.NET i HTML tagów, łącznie z wymaganych atrybutów (takie jak runat = "server") i wspólne atrybuty specyficzne dla tagu (takie jak *identyfikator*,  *Identyfikator DataSourceID*, *ControlToValidate elementu*, i *tekstu*).

Możesz pobrać dodatkowe fragmenty, lub można napisać własne fragmenty kodu, hermetyzują bloki konstrukcyjne kod znaczników, który Ty lub Twój zespół używać do wykonywania typowych zadań.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Ulepszenia funkcji JavaScript IntelliSense

W 2010 r. wizualizacji aby zapewnić jeszcze większe doświadczenie w edytowaniu został przeprojektowany JavaScript IntelliSense. Funkcja IntelliSense rozpoznaje teraz obiekty, które zostały dynamicznie wygenerowane przez metody takie jak *registerNamespace* i podobne techniki używane przez inne struktury języka JavaScript. Do analizowania dużych bibliotek skryptu i wyświetlić IntelliSense z opóźnieniem przetwarzania niewielkiego lub żadnego została ulepszona wydajności. Zgodność została znacznie zwiększona do obsługi praktycznie wszystkich bibliotek innych firm oraz do obsługi różnych stylów kodowania. Komentarze dokumentacji są teraz w sytuacji, w trakcie wpisywania i od razu są wykorzystywane przez funkcję IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Wdrażanie aplikacji sieci Web w programie Visual Studio 2010

Deweloperów platformy ASP.NET wdrażania aplikacji sieci Web, są często znaleźć to, że napotykają problemy, takie jak następujące:

- Wdrażanie do hostingu witryny sieci udostępnionej wymaga technologii, takich jak FTP, który może działać powoli. Ponadto należy ręcznie wykonać zadania, takie jak uruchamianie skryptów SQL, aby skonfigurować bazę danych i należy zmienić ustawienia usług IIS, takie jak konfigurowanie folder katalogu wirtualnego jako aplikacja.
- W środowisku przedsiębiorstwa poza wdrożeniem pliki aplikacji sieci Web administratorzy często muszą modyfikować pliki konfiguracji programu ASP.NET i ustawień usług IIS. Administratorzy baz danych należy uruchomić szereg skrypty SQL, aby pobrać bazę danych aplikacji, uruchomione. Takie urządzenia są intensywnie korzystających z pracy, często prowadzą godzin, a dokładnie dokumentowane.

Program Visual Studio 2010 obejmuje technologie, które rozwiązania tych problemów i umożliwiające bezproblemowe wdrażanie aplikacji sieci Web. Jedną z tych technologii jest narzędzie wdrażania sieci Web usług IIS (MsDeploy.exe).

Funkcje wdrażania sieci Web w programie Visual Studio 2010 następujące główne obszary:

- Tworzenie pakietów w sieci Web
- Przekształcenia pliku Web.config
- Wdrożenie bazy danych
- Publikowanie jednym kliknięciem dla aplikacji sieci Web

Poniższe sekcje zawierają szczegółowe informacje o tych funkcjach.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Tworzenie pakietów w sieci Web

Programu Visual Studio 2010 przy użyciu narzędzia MSDeploy, aby utworzyć plik skompresowany (.zip) dla aplikacji, które są określone jako *pakiet Web*. Plik pakietu zawiera metadane dotyczące aplikacji, a także następującej zawartości:

- Ustawienia usług IIS, które zawiera ustawienia puli aplikacji, ustawień stron błędów i tak dalej.
- Rzeczywista zawartość sieci Web, która zawiera strony sieci Web, formanty użytkownika, zawartość statyczna (obrazy i pliki HTML) i tak dalej.
- Schematy bazy danych programu SQL Server i danych.
- Certyfikaty zabezpieczeń składniki do zainstalowania w pamięci podręcznej GAC, ustawień rejestru i tak dalej.

Pakiet sieci Web można skopiować do dowolnego serwera i następnie instalowane ręcznie przy użyciu Menedżera usług IIS. Alternatywnie dla automatycznego wdrażania pakietu można zainstalować przy użyciu poleceń wiersza polecenia lub przy użyciu interfejsów API wdrożenia.

Program Visual Studio 2010 udostępnia wbudowane zadania MSBuild i elementy docelowe do tworzenia pakietów w sieci Web. Aby uzyskać więcej informacji, zobacz [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [10 + 20 przyczyny, dlaczego należy utworzyć pakiet sieci Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Przekształcenia pliku Web.config

Dla wdrożenia aplikacji sieci Web, program Visual Studio 2010 wprowadzono [przekształcić dokumentu XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), która to funkcja, która umożliwia przekształcenie `Web.config` pliku z ustawieniami środowiska deweloperskiego w środowiskach produkcyjnych. Ustawienia przekształcenia są określone w pliki transformacji o nazwie `web.debug.config`, `web.release.config`i tak dalej. (Nazwy tych plików są zgodne konfiguracje MSBuild). Plik transformacji obejmuje tylko zmiany, które należy wprowadzić do wdrożony `Web.config` pliku. Należy określić zmiany za pomocą prostą składnię.

W poniższym przykładzie pokazano część `web.release.config` pliku, który może być generowany dla wdrożenia swojej konfiguracji wydania. Zastąp słowo kluczowe w przykładzie określa, że podczas wdrażania *connectionString* w węźle `Web.config` pliku zostaną zastąpione wartości, które są wymienione w przykładzie.

[!code-xml[Main](overview/samples/sample102.xml)]

Aby uzyskać więcej informacji, zobacz [składni przekształcania Web.config wdrażania projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) w witrynie MSDN <a id="0.2_a"> </a> witryny sieci Web i[wdrażanie w Internecie: transformacji pliku Web.Config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)na blogu Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Wdrożenie bazy danych

Pakiet wdrożeniowy programu Visual Studio 2010 mogą zawierać zależności dla baz danych programu SQL Server. Jako część definicji pakietu możesz wprowadzić parametry połączenia dla źródłowej bazy danych. Podczas tworzenia pakietu sieci Web programu Visual Studio 2010 tworzy skrypty SQL dla schematu bazy danych i opcjonalnie dla danych, a następnie dodaje je do pakietu. Możesz również podać niestandardowe skrypty SQL i Określ sekwencję, w którym powinno być ono uruchomione na serwerze. W czasie wdrażania Podaj parametry połączenia, który jest odpowiedni dla serwera docelowego; proces wdrażania następnie używa tych parametrów połączenia do uruchamiania skryptów, które umożliwiają tworzenie schematu bazy danych i dodać dane.

Ponadto, za pomocą jednego kliknięcia publikowania, można skonfigurować wdrożenie do publikowania bazy danych bezpośrednio, gdy aplikacja została opublikowana z lokacją zdalną hostingu udostępnionej. Aby uzyskać więcej informacji, zobacz [jak: wdrożyć bazę danych z projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [wdrożenie bazy danych z VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publikowanie jednym kliknięciem dla aplikacji sieci Web

Program Visual Studio 2010 umożliwia także używać usługa zdalnego zarządzania usługami IIS, aby opublikować aplikację sieci Web na serwerze zdalnym. Dla konta usługi hostingu lub serwery testowe lub przejściowe serwerów, można utworzyć profilu publikowania. Każdy profil można bezpiecznie zapisać odpowiednie poświadczenia. Następnie można wdrożyć do dowolnego elementu docelowego pasek narzędzi publikacja w serwerach za pomocą jednego kliknięcia przy użyciu jednego kliknięcia w sieci Web. Za pomocą programu Visual Studio 2010 można również opublikować przy użyciu wiersza polecenia programu MSBuild. Dzięki temu można skonfigurować środowiska kompilacji zespołu, które mają zostać objęte Publikowanie modelu ciągłej integracji.

Aby uzyskać więcej informacji, zobacz [porady: Wdrażanie aplikacji publikowania projektu sieci Web za pomocą jednego kliknięcia i narzędzia Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [sieci Web 1 kliknięciu publikowania z VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) na blogu Vishal Joshi. Aby wyświetlić prezentacje wideo dotyczące wdrażania aplikacji sieci Web w programie Visual Studio 2010, zobacz [VS 2010 dla deweloperów Podgląd w sieci Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Resources

Następujące witryny sieci Web zawierają dodatkowe informacje o platformie ASP.NET 4 i Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — oficjalnej dokumentacji dla platformy ASP.NET 4 w witrynie MSDN w sieci Web.
- [https://www.asp.net/](https://www.asp.net/) — ASP.NET witryny sieci Web dla zespołu.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) i [dynamiczna Mapa zawartości platformy ASP.NET dane](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — zasoby Online w witrynie zespołu programu ASP.NET i w oficjalnej dokumentacji dla danych dynamicznych ASP.NET.
- [https://www.asp.net/ajax/](../../ajax/index.md) — Głównego zasobu sieci Web do tworzenia aplikacji ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Blog zespołu Visual dla deweloperów sieci Web, który zawiera informacje o funkcjach w programie Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — głównego zasobów sieci Web dla wersji zapoznawczych programu ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może ulec znacznym zmianom przed ostatecznym wydaniem komercyjnym oprogramowania opisanych tutaj materiałach.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok firmy Microsoft Corporation na problemy omówione w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI W TYM DOKUMENCIE.

Zgodnie z obowiązującymi przepisami prawa autorskiego jest odpowiedzialny za użytkownika. Bez ograniczenia, praw autorskich, żadna część tego dokumentu może być odtworzyć, przechowywane w lub wprowadzone do systemu wyszukiwania lub przekazywanych w jakiejkolwiek formie lub za pomocą jakichkolwiek środków (elektronicznych, mechanicznych, fotokopiowania, nagrywania lub w przeciwnym razie) lub w jakimkolwiek celu, bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może być patentów, wniosków patentowych, znaków towarowych, praw autorskich lub innych praw własności intelektualnej dotyczące elementów zawartych w tym dokumencie. Wyraźnie określone w umowach licencyjnych firmy Microsoft, udostępnienie tego dokumentu nie mu żadnych licencji dotyczących tych patentów, znaków towarowych, praw autorskich i innej własności intelektualnej.

Jeśli nie określono inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, adres e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2009 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Firma Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami przez ich właścicieli.
