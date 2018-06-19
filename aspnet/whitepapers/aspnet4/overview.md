---
uid: whitepapers/aspnet4/overview
title: Program ASP.NET 4 i omówienie tworzenia sieci Web programu Visual Studio 2010 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera omówienie wiele nowych funkcji programu ASP.NET, które znajdują się w ramach platformy.NET Framework 4 i w programie Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 6ce52c387ff835eda46bc1882b8b974889e2d4af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
ms.locfileid: "30899046"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Program ASP.NET 4 i omówienie tworzenia sieci Web programu Visual Studio 2010
====================
> Ten dokument zawiera omówienie wiele nowych funkcji programu ASP.NET, które znajdują się w ramach platformy.NET Framework 4 i w programie Visual Studio 2010.
> 
> [Pobierz ten dokument](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Zawartość**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Plik Web.config refaktoryzacji](#0.2__Toc253429239 "_Toc253429239")  
[Buforowanie danych wyjściowych Extensible](#0.2__Toc253429240 "_Toc253429240")  
[Automatyczne uruchamianie aplikacji sieci Web](#0.2__Toc253429241 "_Toc253429241")  
[Stałe przekierowanie strony](#0.2__Toc253429242 "_Toc253429242")  
[Zmniejszanie stanu sesji](#0.2__Toc253429243 "_Toc253429243")  
[Rozszerzanie zakresu adresów URL dopuszczalny](#0.2__Toc253429244 "_Toc253429244")  
[Sprawdzanie poprawności żądań Extensible](#0.2__Toc253429245 "_Toc253429245")  
[Obiekt buforowania i obiektu pamięci podręcznej rozszerzeń](#0.2__Toc253429246 "_Toc253429246")  
[Rozszerzalne HTML, adres URL i kodowanie nagłówka HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitorowanie wydajności dla poszczególnych aplikacji w procesie roboczym pojedynczego](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery uwzględnione z MVC i formularzy sieci Web](#0.2__Toc253429251 "_Toc253429251")  
[Obsługa sieci dostarczania zawartości](#0.2__Toc253429252 "_Toc253429252")  
[Skrypty jawne ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Sieci Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Ustawienie metatagów Page.MetaKeywords i właściwości Page.MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Włączanie stan widoku dla pojedynczych formantów](#0.2__Toc253429258 "_Toc253429258")  
[Zmiany w funkcji przeglądarki](#0.2__Toc253429259 "_Toc253429259")  
[Routing w programie ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Ustawienia klienta identyfikatorów](#0.2__Toc253429261 "_Toc253429261")  
[Utrwalanie zaznaczenie wiersza w formantach danych](#0.2__Toc253429262 "_Toc253429262")  
[Formant wykresu ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrowanie danych za pomocą formantu klasą QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Wyrażenia kodu kodowania HTML](#0.2__Toc253429265 "_Toc253429265")  
[Zmiany szablonu projektu](#0.2__Toc253429266 "_Toc253429266")  
[Ulepszenia CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ukrywanie div elementy wokół ukryte pola](#0.2__Toc253429268 "_Toc253429268")  
[Renderowanie tabeli zewnętrznej dla formantów opartego na szablonie](#0.2__Toc253429269 "_Toc253429269")  
[Ulepszenia formantów ListView](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList i RadioButtonList kontrolki rozszerzeniach](#0.2__Toc253429271 "_Toc253429271")  
[Ulepszenia formant menu](#0.2__Toc253429272 "_Toc253429272")  
[Kreator i formanty CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Obsługa obszarów](#0.2__Toc253429275 "_Toc253429275")  
[Obsługa sprawdzania poprawności atrybutów adnotacji danych](#0.2__Toc253429276 "_Toc253429276")  
[Pomocników szablonu](#0.2__Toc253429277 "_Toc253429277")

**[Dane dynamiczne](#0.2__Toc253429278 "_Toc253429278")**  
[Włączanie obsługi danych dynamicznych dla istniejących projektów](#0.2__Toc253429279 "_Toc253429279")  
[Składnia deklaratywne kontrolki Menedżera danych dynamicznych](#0.2__Toc253429280 "_Toc253429280")  
[Szablony jednostki](#0.2__Toc253429281 "_Toc253429281")  
[Nowe szablony pola adresy URL i adresów E-mail](#0.2__Toc253429282 "_Toc253429282")  
[Tworzenie łączy z formantem DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Obsługa dziedziczenia w modelu danych](#0.2__Toc253429284 "_Toc253429284")  
[Obsługa relacje wiele do wielu (tylko Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nowe atrybuty do wyświetlenia sterowania i pomocy technicznej wyliczenia](#0.2__Toc253429286 "_Toc253429286")  
[Rozszerzona obsługa filtry](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web Development ulepszenia](#0.2__Toc253429288 "_Toc253429288")**  
[Ulepszone zgodności CSS](#0.2__Toc253429289 "_Toc253429289")  
[HTML i JavaScript wstawki](#0.2__Toc253429290 "_Toc253429290")  
[Ulepszenia IntelliSense dla JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Wdrożenie aplikacji z programu Visual Studio 2010 w sieci Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Web.config Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Wdrożenie bazy danych](#0.2__Toc253429295 "_Toc253429295")  
[One-Click Publish dla aplikacji sieci Web](#0.2__Toc253429296 "_Toc253429296")  
[Zasoby](#0.2__Toc253429297 "_Toc253429297")

**[Zastrzeżenie](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Podstawowe usługi

ASP.NET 4 wprowadzono szereg funkcji zwiększających usług platformy ASP.NET core, takich jak buforowanie danych wyjściowych i przechowywanie stanu sesji.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refaktoryzacja pliku Web.config

`Web.config` Plik zawierający konfigurację dla aplikacji sieci Web zwiększył się znacznie w poprzednich wersjach kilka programu .NET Framework jako zostały dodane nowe funkcje, takie jak Ajax, routing i integracja z usługami IIS 7. To jest utrudniało jego konfigurowania lub rozpocząć nowej aplikacji sieci Web bez narzędzia, takiego jak Visual Studio. W. wartość NET Framework 4, elementy konfiguracji główne zostały przeniesione do `machine.config` plików i aplikacji teraz te ustawienia były dziedziczone. Dzięki temu `Web.config` plik w aplikacji platformy ASP.NET 4 muszą być puste ani zawierać tylko następujące wiersze, które określ dla programu Visual Studio, jakiej wersji framework jest element docelowy aplikacji:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Buforowanie danych wyjściowych rozszerzonego

Od czasu wydania programu ASP.NET w wersji 1.0 buforowanie danych wyjściowych włączył deweloperom przechowywanie wygenerowanych danych wyjściowych stron, kontrolek i odpowiedzi HTTP w pamięci. Kolejne żądania sieci Web ASP.NET można udostępniać zawartość szybciej pobierając wygenerowanych danych wyjściowych z pamięci zamiast ponownego generowania danych wyjściowych od początku. Jednak takie podejście charakteryzuje się ograniczenie — wygenerowana zawartość zawsze ma być przechowywany w pamięci i na serwerach, które występują duży ruch pamięci używane przez buforowanie danych wyjściowych może konkurować o zapotrzebowaniu pamięci z innych części aplikacji sieci Web.

ASP.NET 4 dodaje punktem rozszerzalności do wyjściowej pamięci podręcznej, który umożliwia skonfigurowanie co najmniej jednego niestandardowego dostawcy pamięci podręcznej danych wyjściowych. Dostawcy pamięci podręcznej danych wyjściowych umożliwia dowolny mechanizm magazynu utrwal zawartość HTML. Umożliwia tworzenie niestandardowych dostawców pamięci podręcznej danych wyjściowych dla różnych trwałości mechanizmy, które mogą obejmować dysków lokalnych lub zdalnych, w chmurze, magazynu i rozproszonej pamięci podręcznej aparaty.

Tworzenie niestandardowego dostawcy pamięci podręcznej danych wyjściowych jako klasa, która pochodzi z nowego *System.Web.Caching.OutputCacheProvider* typu. Następnie można skonfigurować dostawcę w `Web.config` pliku przy użyciu nowej *dostawców* podsekcji z *outputCache* element, jak pokazano w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample2.xml)]

Domyślnie wszystkie odpowiedzi HTTP, ASP.NET 4 renderowania stron i kontrolek przy użyciu pamięci podręcznej danych wyjściowych w pamięci, jak pokazano w poprzednim przykładzie, gdzie *dostawcę defaultProvider* atrybut ma ustawioną AspNetInternalProvider. Można zmienić domyślny dostawca pamięci podręcznej danych wyjściowych używane dla aplikacji sieci Web, określając nazwę innego dostawcę *dostawcę defaultProvider*.

Ponadto można wybrać innego dostawcy pamięci podręcznej danych wyjściowych dla każdego formantu i na żądanie. Najprostszym sposobem wybierz innego dostawcę pamięci podręcznej danych wyjściowych w przypadku różnych kontrolek użytkownika sieci Web jest tak deklaratywnie przy użyciu nowej *providerName* atrybutu w dyrektywie kontroli, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Określenie dostawcy różnych wyjściowej pamięci podręcznej dla żądania HTTP wymaga nieco więcej wysiłku. Zamiast określania deklaratywnie dostawcy, można zastąpić nowy *GetOuputCacheProviderName* metody w `Global.asax` pliku programowo Określ, który dostawca do użycia na potrzeby określonego żądania. W przykładzie poniżej pokazano, jak to zrobić.

[!code-csharp[Main](overview/samples/sample4.cs)]

Z dodatkiem rozszerzalności dostawcy pamięci podręcznej danych wyjściowych programu ASP.NET 4 można teraz wykonywać strategii skuteczniejsze i bardziej inteligentne buforowanie danych wyjściowych witryn sieci Web. Na przykład obecnie istnieje możliwość buforowania na stronach "Najważniejsze 10" lokacji w pamięci, podczas buforowania stron pobierające niższe ruchu na dysku. Alternatywnie możesz pamięci podręcznej każdej kombinacji różnią się przez dla renderowanej strony, ale użyj rozproszonej pamięci podręcznej, dzięki czemu zużycie pamięci jest przekazywane z serwerów frontonu sieci Web.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Automatyczne uruchamianie aplikacji sieci Web

Niektóre aplikacje sieci Web trzeba ładowania dużych ilości danych lub zainicjować kosztowne przetwarzania przed pierwszym żądania przez obsługujący. We wcześniejszych wersjach programu ASP.NET w takich sytuacjach konieczne było opracować niestandardowy podejścia do "wznawiania" aplikacji ASP.NET, a następnie uruchom kod inicjujący podczas *aplikacji\_obciążenia* metody w `Global.asax` plik.

Nowa funkcja skalowalność *auto-start* czy bezpośrednio adresy ten scenariusz jest dostępne po programu ASP.NET 4 działa na IIS 7.5 w systemie Windows Server 2008 R2. Funkcja automatycznego uruchamiania zapewnia kontrolowany sposób uruchamiania puli aplikacji, inicjowanie aplikacji ASP.NET i następnie akceptować żądania HTTP.

> [!NOTE] 
> 
> Moduł zwiększanie gotowości aplikacji usług IIS dla usług IIS 7.5
> 
> Zespół usługi IIS wydała pierwszą wersję beta testu modułu zwiększanie gotowości aplikacji dla usług IIS 7.5. Dzięki temu rozgrzewanie łatwiejsze niż opisane wcześniej aplikacji. Zamiast pisanie kodu niestandardowego, należy określić adresy URL zasobów do wykonania przed aplikacji sieci Web akceptuje żądania z sieci. Ten rozgrzewania występuje podczas uruchamiania usług IIS (Jeśli skonfigurowano puli aplikacji usług IIS jako *AlwaysRunning*) i podczas odtwarzania procesu roboczego programu IIS. Podczas odtwarzania stary proces roboczy usług IIS nadal wykonywać żądania, dopóki proces roboczy nowo uruchomionego jest w pełni przygotowaniu miejsca, dzięki czemu aplikacje doświadczeniem nie przerw w działaniu lub inne problemy z powodu unprimed pamięci podręcznych. Należy pamiętać, że moduł ten współdziała z wszystkich wersji platformy ASP.NET, począwszy od wersji 2.0.
> 
> Aby uzyskać więcej informacji, zobacz [zwiększanie gotowości aplikacji](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) w witrynie sieci IIS.net Web. Aby uzyskać wskazówki, która ilustruje sposób korzystania z funkcji rozgrzewania, zobacz [wprowadzenie do korzystania z modułu zwiększanie gotowości aplikacji usług IIS 7.5](https://www.iis.net/learn/manage) w witrynie sieci IIS.net Web.


Aby użyć funkcji automatycznego uruchamiania, administrator usług IIS ustawia puli aplikacji IIS 7.5 ma zostać automatycznie uruchomiony przy użyciu następującej konfiguracji w `applicationHost.config` pliku:

[!code-xml[Main](overview/samples/sample5.xml)]

Ponieważ jednej puli aplikacji może zawierać wielu aplikacji, określ poszczególnych aplikacji, które ma zostać automatycznie uruchomiony przy użyciu następującej konfiguracji w `applicationHost.config` pliku:

[!code-xml[Main](overview/samples/sample6.xml)]

W przypadku serwera IIS 7.5 uruchomiona chłodni lub podczas odtwarzania puli aplikacji usług IIS 7.5 używa tych informacji w `applicationHost.config` pliku, aby określić, które wymagają aplikacji sieci Web ma być uruchamiana automatycznie. Dla każdej aplikacji, która jest oznaczony do automatycznego uruchamiania IIS 7.5 wysyła żądanie do programu ASP.NET 4, aby uruchomić aplikację w stanie, w którym aplikacja tymczasowo nie akceptuje żądań HTTP. Gdy jest on w tym stanie, program ASP.NET tworzy typ zdefiniowany przez *serviceAutoStartProvider* atrybut (jak pokazano w poprzednim przykładzie) i wywołuje jego punktu wejścia publicznego.

Tworzenie typu zarządzanego automatycznego uruchamiania z punktu wejścia niezbędne zaimplementowanie *IProcessHostPreloadClient* interfejsu, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample7.cs)]

Po zainicjowaniu Twojej kod działa na platformie *wstępnego ładowania* — metoda i metoda zwróci wartość, aplikacja ASP.NET jest gotowy do przetwarzania żądań.

Z dodatkiem auto-start.5 usług IIS i platformy ASP.NET 4 został dobrze zdefiniowany podejście do wykonywania inicjowanie kosztowne aplikacji przed przetworzeniem pierwszego żądania HTTP. Na przykład korzystając z nowej funkcji automatycznego uruchamiania można Inicjowanie aplikacji, a następnie sygnału równoważenia obciążenia, czy aplikacja została zainicjowana i gotowa do akceptowania ruchu HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Stałe przekierowanie strony

Jest typowym rozwiązaniem w aplikacji sieci Web można przenieść strony i innej zawartości wokół wraz z upływem czasu, co może prowadzić do gromadzenia starych łączy aparaty wyszukiwania. W programie ASP.NET, deweloperzy mają obsługiwane tradycyjnie żądań stare adresy URL przy użyciu *Response.Redirect* metodę, aby przesłać żądanie do nowego adresu URL. Jednak *przekierowania* metody wystawia odpowiedź HTTP 302 znaleziono (Przekierowanie tymczasowe), co powoduje dodatkowe HTTP Rundy, gdy użytkownicy próbują uzyskać dostęp stare adresy URL.

Dodaje nowy ASP.NET 4 *RedirectPermanent* metody pomocniczej, która ułatwia problem HTTP 301 trwale przeniesiona odpowiedzi, jak w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample8.cs)]

Aparaty wyszukiwania i innych agentów użytkownika, które rozpoznają stałe przekierowuje zapisze nowy adres URL jest powiązane z zawartością, która eliminuje niepotrzebne obiegu wprowadzone przez przeglądarkę dla przekierowania tymczasowego.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Zmniejszanie stanu sesji

Program ASP.NET udostępnia dwa domyślne opcje do przechowywania stanu sesji na farmie sieci Web: dostawcy stanu sesji, który wywołuje serwer stanu sesji poza procesem, a dostawca stanu sesji, która przechowuje dane w bazie danych programu Microsoft SQL Server. Ponieważ obie te opcje wymagają przechowywania informacji o stanie poza procesu roboczego aplikacji sieci Web, stan sesji ma być serializowana przed wysłaniem do magazynu zdalnego. W zależności od tego, ile informacji dewelopera zapisuje stan sesji dość duży maksymalny rozmiar danych serializacji.

ASP.NET 4 wprowadza nową opcję kompresji dla obu typów dostawców stanu sesji poza procesem. Gdy *compressionEnabled* ustawiono opcję konfiguracji pokazano w poniższym przykładzie *true*, ASP.NET skompresuje (i dekompresja) stanu sesji serializacji przy użyciu programu .NET Framework  *System.IO.Compression.GZipStream* klasy.

[!code-xml[Main](overview/samples/sample9.xml)]

Dodając proste nowy atrybut do `Web.config` plików, aplikacji za pomocą zapasowe cykle procesora CPU na serwerach sieci Web można uzyskać znaczne obniżenie rozmiar danych stanu sesji serializacji.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Rozszerzanie zakresie dozwolonym adresów URL

ASP.NET 4 wprowadza nowe opcje dotyczące rozszerzania rozmiaru adresu URL aplikacji. Poprzednie wersje programu ASP.NET ograniczenia długości ścieżki adresu URL do 260 znaków, oparte na granicy ścieżka pliku systemu plików NTFS. W technologii ASP.NET 4 masz opcję, aby zwiększyć (lub zmniejszyć) ten limit na potrzeby aplikacji, używając dwóch nowych *httpRuntime* atrybuty konfiguracji. W poniższym przykładzie przedstawiono te nowe atrybuty.

[!code-xml[Main](overview/samples/sample10.xml)]

Aby umożliwić dłuższy lub krótszy ścieżki (część adresu URL, który nie obejmuje protokołu, nazwy serwera i ciągu zapytania), zmodyfikuj *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* atrybutu. Aby umożliwić ciągi zapytań dłuższy lub krótszy, zmodyfikuj wartość *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* atrybutu.

ASP.NET 4 umożliwia również skonfigurowanie znaki, które są używane przez wyboru znak adresu URL. Jeśli program ASP.NET znajdzie nieprawidłowy znak w części ścieżki adresu URL, odrzuca żądanie i generuje błąd HTTP 400. W poprzednich wersjach programu ASP.NET sprawdza znak adresu URL były ograniczone do stałego zestawu znaków. W przypadku programu ASP.NET 4, można dostosować zbiór prawidłowych znaków przy użyciu nowego *requestPathInvalidChars* atrybutu *httpRuntime* element konfiguracji, jak pokazano w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample11.xml)]

Domyślnie <em>requestPathInvalidChars</em> atrybut definiuje osiem znaków jako nieprawidłowe. (W ciągu, który jest przypisany do <em>requestPathInvalidChars</em> domyślnie<em>,</em>mniej niż (&lt;), jest większa niż (&gt;) i handlowego "i" (&amp;) znaków zakodowany, ponieważ `Web.config` plik jest plikiem XML.) W razie potrzeby można dostosować zestaw nieprawidłowe znaki.

> [!NOTE]
> Uwaga programu ASP.NET 4 zawsze odrzuca ścieżki adresu URL, zawierających znaki w zakresie ASCII 0x00 do 0x1F, ponieważ są to nieprawidłowe znaki adres URL, zgodnie z definicją w dokumencie RFC 2396 IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). W wersjach systemu Windows Server z systemem usług IIS 6 lub nowszego, sterownik http.sys protokołu urządzenia automatycznie odrzuca adresów URL z tych znaków.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Sprawdzanie poprawności rozszerzonego żądań

Weryfikacja żądania ASP.NET przeszukuje przychodzące dane żądania HTTP dla ciągów, które są często używane w atakami skryptów między witrynami (XSS). W przypadku odnalezienia potencjalnych ciągi XSS weryfikacji żądań flagi podejrzane ciągu i zwraca błąd. Weryfikacja żądań wbudowanych zwraca błąd tylko wtedy, gdy znajdzie ciągi najczęściej używane w atakom XSS. W wyniku poprzednich prób wprowadzenia weryfikacji XSS skuteczniejsze zbyt wiele fałszywych alarmów. Jednak klienci może okazać się, że weryfikacja żądań, które jest bardziej agresywną lub odwrotnie może być celowo złagodzenie XSS sprawdza dla określonych stron lub dla określonych typów żądań.

W technologii ASP.NET 4 funkcji sprawdzania poprawności żądań wprowadzono extensible tak, aby można było używać logiki niestandardowej weryfikacji żądań. Aby rozszerzyć weryfikację żądań, Utwórz klasę pochodzącą od nowa *System.Web.Util.RequestValidator* typu i konfigurowanie aplikacji (w *httpRuntime* sekcji `Web.config`pliku) Aby użyć typu niestandardowego. Poniższy przykład przedstawia sposób konfigurowania klasy niestandardowej weryfikacji żądań:

[!code-xml[Main](overview/samples/sample12.xml)]

Nowy *requestValidationType* atrybut wymaga standardowe ciąg do identyfikator typu .NET Framework, który określa klasę, która udostępnia weryfikację żądanie niestandardowe. Dla każdego żądania ASP.NET wywołuje typu niestandardowego do przetwarzania każdego z nich dane przychodzące żądania HTTP. Przychodzącego adresu URL, wszystkie nagłówki HTTP (pliki cookie i niestandardowe nagłówki) i treści jednostki są wszystkie dostępne do wglądu klasy do sprawdzania poprawności żądania niestandardowych, jak który pokazano w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample13.cs)]

W przypadkach, gdy nie chcesz sprawdzić element przychodzących danych protokołu HTTP, klasa weryfikacji żądań przełączają się na let weryfikacji żądania domyślnego platformy ASP.NET, po prostu wywołując *podstawowej. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Obiekt buforowania i obiektu pamięci podręcznej rozszerzeń

Od czasu jej pierwszego wydania ASP.NET była dostępna pamięć podręczną zaawansowanych obiektu w pamięci (*System.Web.Caching.Cache*). Implementacja pamięci podręcznej została tak popularna, że został on użyty w aplikacjach sieci Web. Jest jednak nieodpowiednich załączenie odwołania do aplikacji formularzy systemu Windows lub programu WPF `System.Web.dll` tak, aby można było używać pamięci podręcznej programu ASP.NET obiektu.

Aby udostępnić buforowania dla wszystkich aplikacji, programu .NET Framework 4 wprowadza nowego zestawu, nowej przestrzeni nazw niektórych typów podstawowych i konkretnych, buforowanie implementacji. Nowy `System.Runtime.Caching.dll` zestaw zawiera nowy interfejs API buforowania w *system.Runtime.Caching —* przestrzeni nazw. Przestrzeń nazw zawiera dwa zestawy podstawowe klasy:

- Typy abstrakcyjne, które stanowią podstawę tworzenia dowolnego typu implementacji niestandardowych pamięci podręcznej.
- Implementacja pamięci podręcznej konkretnego obiektu w pamięci ( *System.Runtime.Caching.MemoryCache* klasy).

Nowy *MemoryCache* klasy został uformowany ściśle pamięci podręcznej programu ASP.NET i udostępnia znacznie logiki aparatu wewnętrznej pamięci podręcznej z programem ASP.NET. Mimo że publicznego buforowania interfejsów API w *system.Runtime.Caching —* zostały zaktualizowane do obsługi tworzenia niestandardowych pamięci podręcznych, użycie programu ASP.NET *pamięci podręcznej* obiektu, można znaleźć znane pojęcia związane z nowe interfejsy API.

Szczegółowe omówienie nowej *MemoryCache* klasy i obsługa interfejsów API podstawowej wymagają całego dokumentu. Jednak poniższy przykład umożliwia pogląd, jak działa nową pamięć podręczną interfejsu API. Przykład został napisany dla aplikacji formularzy systemu Windows, bez żadnych zależności na `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Rozszerzalne HTML, adres URL i kodowanie nagłówka HTTP

W ASP.NET 4 można tworzyć niestandardowe procedury kodowania dla następujące typowe zadania kodowania tekstu:

- Kodowanie HTML.
- Kodowanie adresu URL.
- Kodowanie atrybutu HTML.
- Kodowanie wychodzącego nagłówków HTTP.

Można utworzyć niestandardowego kodera wynikających z nowego *System.Web.Util.HttpEncoder* typu, a następnie konfigurując ASP.NET do użycia niestandardowego typu w *httpRuntime* sekcji `Web.config` pliku, jako pokazano w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample15.xml)]

Po skonfigurowaniu niestandardowego kodera ASP.NET automatycznie wywołuje implementacja niestandardowa kodowania zawsze, gdy jest to kodowanie metod publicznych *System.Web.HttpUtility* lub *System.Web.HttpServerUtility* noszą nazwę klasy. Dzięki temu jednej części zespół deweloperów sieci Web tworzenie niestandardowego kodera, który implementuje kodowania znaków aktywnego, podczas gdy pozostałe zespół deweloperów sieci Web w dalszym ciągu używa publicznego kodowanie interfejsów API platformy ASP.NET. Konfigurując centralnie niestandardowego kodera w *httpRuntime* elementu, masz gwarancję że wywołania kodowanie tekstu z publicznego kodowanie interfejsów API platformy ASP.NET są kierowane za pomocą niestandardowego kodera.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitorowanie wydajności dla poszczególnych aplikacji w procesie pojedynczego procesu roboczego

Aby zwiększyć liczbę witryn sieci Web, które mogą być hostowane na jednym serwerze, wielu dostawców usług hostingowych uruchamiać wiele aplikacji programu ASP.NET w procesie pojedynczego procesu roboczego. Jednak użycie jednego udostępnionego procesu roboczego procesu, wiele aplikacji jest trudne dla administratorów serwerów do identyfikowania poszczególnych aplikacji, która ma problemy.

ASP.NET 4 korzysta z nowych funkcji monitorowania zasobów, wprowadzonych przez środowisko CLR. Aby włączyć tę funkcję, należy dodać poniższy fragment XML w konfiguracji, aby `aspnet.config` pliku konfiguracji.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Uwaga `aspnet.config` pliku znajduje się w katalogu, w których zainstalowano programu .NET Framework. Nie jest `Web.config` pliku.


Gdy *appdomainresourcemonitoring —* funkcja została włączona, dwa nowe liczniki wydajności są dostępne w kategorii wydajności "Aplikacji ASP.NET": *% czasu procesora zarządzane* i  *Zarządzane używanej pamięci*. Nowa funkcja zarządzania zasobów domen aplikacji CLR oba te liczniki wydajności służy do śledzenia szacowany czas procesora CPU i użycie pamięci zarządzanej przez poszczególnych aplikacji ASP.NET. W związku z tym z platformy ASP.NET 4 Administratorzy już bardziej szczegółowego widoku do zużycia zasobów poszczególnych aplikacji uruchomionych w procesie pojedynczego procesu roboczego.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Wielowersyjność kodu

Można utworzyć aplikację, przeznaczonego dla określonej wersji programu .NET Framework. W przypadku programu ASP.NET 4, nowy atrybut w *kompilacji* elementu `Web.config` plik umożliwia dla środowiska .NET Framework 4 lub nowszy. Jawnie są przeznaczone dla platformy .NET Framework 4, a obejmują elementy opcjonalne w `Web.config` plików, takich jak wpisy *system.codedom*, te elementy muszą być poprawne dla programu .NET Framework 4. (Jeśli nie są jawnie kierowane programu .NET Framework 4, platforma docelowa jest wywnioskowany na podstawie Brak wpisu w `Web.config` pliku.)

W poniższym przykładzie przedstawiono użycie *targetFramework* atrybutu w *kompilacji* elementu `Web.config` pliku.

[!code-xml[Main](overview/samples/sample17.xml)]

Należy pamiętać, że o przeznaczonych dla określonej wersji programu .NET Framework:

- W puli aplikacji programu .NET Framework 4, system kompilacji ASP.NET zakłada programu .NET Framework 4 jako miejsce docelowe Jeśli `Web.config` plik nie zawiera *targetFramework* atrybutu lub jeśli `Web.config` brakuje pliku. (Być może trzeba wprowadzać zmian kodowania do aplikacji, aby była uruchamiana z programu .NET Framework 4).
- Jeśli dołączysz *targetFramework* atrybutu i w razie *system.codeDom* w jest zdefiniowany element `Web.config` pliku, ten plik musi zawierać prawidłowe wpisy dla programu .NET Framework 4.
- Jeśli używasz *aspnet\_kompilatora* polecenia wstępnej kompilacji aplikacji (na przykład w środowisku kompilacji), należy użyć poprawnej wersji *aspnet\_kompilatora* polecenie dla struktury docelowej. Za pomocą kompilatora, która dostarczona programu .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) do kompilowania dla platformy .NET Framework 3.5 i wcześniejszymi wersjami. Użyj kompilator jest dostarczany z programu .NET Framework 4 skompilować aplikacje utworzone przy użyciu tego framework lub nowszy.
- W czasie wykonywania, kompilator używa najnowszych zestawów platformy, które są zainstalowane na komputerze (i w związku z tym w pamięci GAC). Jeśli aktualizacja ma zostać później do struktury (na przykład hipotetyczny wersji 4.1 jest instalowana), można używać funkcji w nowszej wersji platformy, nawet jeśli *targetFramework* atrybut jest przeznaczony dla starszej wersji (na przykład 4.0). (Jednak w czasie projektowania w Visual Studio 2010 lub jeśli używasz *aspnet\_kompilatora* polecenia przy użyciu nowsze funkcje platformy spowoduje, że błędy kompilatora).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery uwzględnione z MVC i formularzy sieci Web

Szablony Visual Studio dla platformy MVC i formularzy sieci Web obejmują biblioteki jQuery open source. Podczas tworzenia nowej witryny sieci Web lub projektu, jest tworzony w folderze skryptów znajdują się następujące pliki 3:

- jQuery 1.4.1.js — czytelny dla człowieka, unminified wersji biblioteki jQuery.
- jQuery — 14.1.min.js — zminimalizowany wersji biblioteki jQuery.
- jQuery-1.4.1-vsdoc.js — plik dokumentacji Intellisense w bibliotece jQuery.

Uwzględnij unminified wersji jQuery podczas tworzenia aplikacji. Zawierają zminimalizowany wersję jQuery przez aplikacje produkcyjne.

Na przykład następujące strony formularzy sieci Web przedstawiono, jak można użyć jQuery, aby zmienić kolor tła formantów ASP.NET TextBox się żółty po uzyskaniu fokusu.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Obsługa sieci dostarczania zawartości

Microsoft Ajax sieci dostarczania zawartości (CDN) pozwala na łatwe dodawanie do aplikacji sieci Web ASP.NET Ajax i jQuery skryptów. Na przykład można uruchomić za pomocą biblioteki jQuery po prostu dodając `<script>` tag do strony wskazujące Ajax.microsoft.com następująco:

[!code-html[Main](overview/samples/sample19.html)]

Korzystając z usługi Microsoft Ajax CDN, może znacznie poprawić wydajność aplikacji Ajax. Zawartość Microsoft Ajax CDN są buforowane na serwerów na całym świecie. Ponadto usługa Microsoft Ajax CDN umożliwia przeglądarki dla witryn sieci Web, które znajdują się w różnych domenach ponowne użycie pamięci podręcznej plików JavaScript.

Microsoft Ajax Content Delivery Network obsługuje protokół SSL (HTTPS), w razie potrzeby do obsługi strony sieci web przy użyciu protokołu SSL.

Zaimplementuj rezerwowe, gdy element CDN jest niedostępny. Przetestuj powrotu.

Aby dowiedzieć się więcej na temat usługi Microsoft Ajax CDN, odwiedź następującą witrynę sieci Web:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Element ScriptManager ASP.NET obsługuje Microsoft Ajax CDN. Po prostu przez ustawienie jedną właściwość właściwości EnableCdn można pobrać wszystkich plików JavaScript framework ASP.NET z sieci CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Po ustawieniu właściwości EnableCdn wartość true, struktury programu ASP.NET pobierze wszystkie pliki JavaScript framework ASP.NET z sieci CDN w tym wszystkie pliki JavaScript używany do sprawdzania poprawności i UpdatePanel. Ustawienie tej właściwości co może mieć znaczący wpływ na wydajność aplikacji sieci web.

Ścieżka CDN dla plików JavaScript można ustawić za pomocą atrybutu widok. Nowe właściwości CdnPath Określa ścieżkę do CDN używany, gdy wartość właściwości EnableCdn na wartość true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>ScriptManager Explicit Scripts

W przeszłości Jeśli użyto ASP.NET ScriptManger następnie zostały trzeba załadować całej biblioteki wbudowanymi Ajax ASP.NET. Dzięki wykorzystaniu nowych właściwości ScriptManager.AjaxFrameworkMode, można kontrolować dokładnie są ładowane składniki biblioteki ASP.NET Ajax i załadować tylko składniki biblioteki ASP.NET Ajax, potrzebne.

Właściwość ScriptManager.AjaxFrameworkMode można ustawić następujące wartości:

- Włączone — Określa, czy formant ScriptManager automatycznie zawiera MicrosoftAjax.js pliku skryptu, który jest plik skryptu Scalonej każdego skryptu framework core (starsze zachowanie).
- Wyłączone — Określa, że wyłączono wszystkich funkcji skryptów Microsoft Ajax i że formantu ScriptManager nie odwołuje się wszystkie skrypty automatycznie.
- Jawne — Określa jawnie dołączysz odwołań do skryptów do poszczególnych framework core skryptu pliku, który wymaga ze strony i uwzględni odwołań do zależności, które wymaga każdego pliku skryptu.

Na przykład jeśli właściwość AjaxFrameworkMode jest ustawiona na wartość Explicit następnie można określić skrypty określonego składnika ASP.NET Ajax, które należy:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Formularze sieci Web

Formularze sieci Web została podstawowych funkcji w programie ASP.NET od czasu wydania programu ASP.NET w wersji 1.0. Wiele udoskonaleń zostały w tym obszarze dla programu ASP.NET 4, takie jak następujące:

- Możliwość określenia *meta* tagów.
- Większa kontrola nad stan widoku.
- Łatwiejsze sposoby pracy z możliwości przeglądarki.
- Obsługa przy użyciu routingu platformy ASP.NET z formularzy sieci Web.
- Większa kontrola nad wygenerowanych identyfikatorów.
- Możliwość utrzymania zaznaczone wiersze w formantach danych.
- Większa kontrola nad HTML renderowanych w *FormView* i *ListView* kontrolki.
- Filtrowanie obsługę kontrolki źródła danych.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Ustawienie metatagów Page.MetaKeywords i Page.MetaDescription właściwości

ASP.NET 4 dodaje dwie właściwości do *strony* klasy *MetaKeywords* i *MetaDescription*. Te dwie właściwości reprezentują odpowiadającego *meta* znaczników na stronie, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Te dwie właściwości zachowuje się tak samo jak robi strony *tytuł* jest właściwość. One wykonać następujące czynności:

1. Jeśli istnieją nie *meta* tagów w *head* element pasuje do nazwy właściwości (oznacza to, że nazwa = "słów kluczowych" *Page.MetaKeywords* i nazwa = "opis"  *Page.MetaDescription*, co oznacza, że nie ustawiono tych właściwości), *meta* tagi zostanie dodany do strony, gdy jest on renderowany.
2. Jeśli istnieje już *meta* tagi o tych nazwach, te właściwości pełnić rolę metody dla zawartości, istniejący tagów get i set.

Te właściwości można ustawić w czasie wykonywania, które pozwala uzyskać zawartość z bazą danych lub innego źródła, a które pozwala ustawić tagów dynamicznie Opisz, co dotyczy określonej strony.

Można również ustawić *słowa kluczowe* i *opis* właściwości w *@ Page* dyrektywy w górnej części w znaczniku strony formularzy sieci Web, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Spowoduje to zastąpienie *meta* zawartość (jeśli istnieje) już zadeklarowany w stronie tagu.

Zawartość opis *meta* tag służą do poprawy wyszukiwania, wyświetlania podglądu w usłudze Google. (Aby uzyskać więcej informacji, zobacz [poprawy wstawki z makeover Opis meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) na blogu Google Webmaster centralnej.) Google i wyszukiwania usługi Windows Live nie używaj zawartość słowa kluczowe dla wszystkich elementów, ale może innych aparatów wyszukiwania. Aby uzyskać więcej informacji, zobacz [porady słowa kluczowe Meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) w witrynie sieci Web przewodnik aparatu wyszukiwania.

Nowe właściwości są funkcją prostego, ale należy zapisać z konieczności je ręcznie dodać lub pisania kodu do utworzenia *meta* tagów.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Włączanie stan widoku dla pojedynczych formantów

Domyślnie stan widoku jest włączony dla tej strony, w wyniku, że każdej kontrolki na stronie potencjalnie przechowuje stan widoku, nawet jeśli nie jest wymagane dla aplikacji. Widok stanu danych znajduje się w znaczników, że strona generuje i wydłuża czas potrzebny do wysyłania strony do klienta i opublikuj go ponownie. Przechowywanie więcej stan widoku, niż jest to konieczne może być przyczyną znacznego obniżenia wydajności. We wcześniejszych wersjach programu ASP.NET deweloperzy można wyłączyć stan widoku dla pojedynczych formantów, aby zmniejszyć rozmiar strony, ale ma robić jednoznacznie pojedynczych formantów. W programie ASP.NET 4 kontrolki serwera sieci Web obejmują *ViewStateMode* właściwość, która umożliwia powodująca domyślne wyłączenie stan widoku, a następnie włączyć ją tylko w przypadku kontrolek, które wymagają go na stronie.

*ViewStateMode* właściwość przyjmuje wyliczenia, która ma trzy wartości: *włączone*, *wyłączone*, i *Dziedzicz*. *Włączone* umożliwia wyświetlanie stanu dla tego formantu i formanty podrzędne, które są ustawione na *Dziedzicz* lub że nic ustawione. *Wyłączone* wyłącza wyświetlanie stanu, i *Dziedzicz* Określa, że kontrolka używa *ViewStateMode* z kontrolki nadrzędnej.

W poniższym przykładzie przedstawiono sposób *ViewStateMode* właściwości działania. Znaczników i kodu dla formantów na następującej stronie zawiera wartości *ViewStateMode* właściwości:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Jak widać, kod powoduje wyłączenie stan widoku formantu PlaceHolder1. Formant podrzędny label1 dziedziczy wartość tej właściwości (*Dziedzicz* jest wartością domyślną dla *ViewStateMode* dla formantów.) i w związku z tym zapisuje bez stanu widoku. W formancie PlaceHolder2 *ViewStateMode* ustawiono *włączone*, więc label2 dziedziczy tej właściwości, oraz zapisywanie stanu widoku. Po załadowaniu strony *tekst* właściwość obu *etykiety* formantów ma ustawioną wartość ciągu "[wartość dynamiczną]".

Te ustawienia powoduje, że podczas ładowania strony po raz pierwszy, następujące dane wyjściowe są wyświetlane w przeglądarce:

wyłączone `: [DynamicValue]`

Włączone:`[DynamicValue]`

Po odświeżeniu strony jednak następujące dane wyjściowe są wyświetlane:

wyłączone `: [DeclaredValue]`

Włączone:`[DynamicValue]`

Formant label1 (których *ViewStateMode* ma wartość *wyłączone*) nie została zachowana wartość, która była ustawiona na w kodzie. Jednak sterować label2 (których *ViewStateMode* ma wartość *włączone*) została zachowana jego stanu.

Można również ustawić *ViewStateMode* w *@ Page* dyrektywy, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample26.aspx)]

*Strony* klasy jest tylko inny formant; działa jako formant nadrzędny dla innych formantów na stronie. Wartość domyślna *ViewStateMode* jest *włączone* dla wystąpień *strony*. Ponieważ domyślnie formanty *Dziedzicz*, formanty będzie dziedziczyć *włączone* wartość właściwości, chyba że zostanie ustawiony *ViewStateMode* na poziomie strona lub kontrolka.

Wartość *ViewStateMode* właściwość określa, czy stan widoku jest obsługiwany tylko wtedy, gdy *EnableViewState* właściwość jest ustawiona na *true*. Jeśli *EnableViewState* właściwość jest ustawiona na *false*, stan widoku nie zostaną zachowane nawet wtedy, gdy *ViewStateMode* ustawiono *włączone*.

Dobre wykorzystanie dla tej funkcji jest z *ContentPlaceHolder* formantów strony wzorcowe, w którym można ustawić *ViewStateMode* do *wyłączone* wzorca strony, a następnie włącz go osobno dla *ContentPlaceHolder* kontrolek, które z kolei zawierają formanty, które wymagają stanu widoku.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Zmiany funkcji przeglądarki

ASP.NET określa możliwości przeglądarki, używanym do przeglądania witryny za pomocą funkcji o nazwie użytkownika *możliwości przeglądarki*. Możliwości przeglądarki są reprezentowane przez *HttpBrowserCapabilities* obiektu (udostępnione przez *Request.Browser* właściwości). Na przykład można użyć *HttpBrowserCapabilities* obiektem, aby określić, czy typ i wersja bieżąca przeglądarka obsługuje konkretnej wersji języka JavaScript. Możesz też użyć *HttpBrowserCapabilities* obiektem, aby określić, czy żądanie pochodzi z urządzenia przenośnego.

*HttpBrowserCapabilities* obiektu jest wymuszany przez zestaw plików definicji przeglądarki. Te pliki zawierają informacje na temat możliwości określonego przeglądarek. W przypadku programu ASP.NET 4 te pliki definicji przeglądarki zostały zaktualizowane w usłudze zawierają informacje o ostatnio wprowadzone przeglądarek i urządzeń, takich jak Google Chrome, badawczych smartfony BlackBerry ruchu i Apple iPhone.

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

W programie ASP.NET w wersji 3.5 z dodatkiem Service Pack 1, można zdefiniować możliwości, które ma przeglądarki w następujący sposób:

- Na poziomie komputera, Utwórz lub zaktualizuj `.browser` pliku XML w następującym folderze:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Po zdefiniowaniu możliwości przeglądarki, możesz uruchom następujące polecenie z programu Visual Studio wiersza polecenia w celu odbudowania zestaw funkcji przeglądarki i dodaj go do pamięci podręcznej GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Dla poszczególnych aplikacji, należy utworzyć `.browser` plik w aplikacji `App_Browsers` folderu.

Tych metod wymaga zmiany plików XML, a zmiany na poziomie komputera, należy ponownie uruchomić aplikację po uruchomieniu aspnet\_regbrowsers.exe procesu.

Program ASP.NET 4 zawiera funkcję określaną jako *dostawców możliwości przeglądarki*. Zgodnie z sugestią, nazwa, ta umożliwia tworzenie dostawcy, który z kolei umożliwia użyć własnego kodu do określania możliwości przeglądarki.

W praktyce deweloperzy często nie definiują możliwości przeglądarki niestandardowych. Przeglądarka plików są trudne do aktualizacji, proces ich uaktualnienia jest dość złożone i składnia XML `.browser` plików może być skomplikowane i zdefiniować. Co spowodowałoby, ten proces ułatwi to gdyby wspólnej Składnia definicji przeglądarki lub bazy danych, która zawiera definicje aktualne przeglądarki lub nawet usługi sieci Web dla bazy danych. Nowa funkcja dostawców możliwości przeglądarki sprawia, że te scenariusze możliwe i praktyczne dla deweloperów innych firm.

Istnieją dwie metody głównej dla przy użyciu nowej funkcji dostawcy możliwości przeglądarki ASP.NET 4: rozszerzanie możliwości przeglądarki ASP.NET definicji funkcji lub całkowicie zamienienie go. W poniższych sekcjach opisano najpierw sposobu zastępują funkcjonalność i jego rozszerzenia.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Zastępowanie funkcji możliwości przeglądarki ASP.NET

Aby całkowicie zastąpić funkcjonalności definicji możliwości przeglądarki ASP.NET, wykonaj następujące kroki:

1. Tworzenie klasy dostawcy, która jest pochodną *HttpCapabilitiesProvider* i który zastępuje *GetBrowserCapabilities* metody, jak w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Tworzy nowy kod w tym przykładzie *HttpBrowserCapabilities* obiektów, określając możliwości przeglądarki i ustawienie MyCustomBrowser taką możliwość.
2. Zarejestruj dostawcę z aplikacją. 

    Aby można było użyć dostawcy z aplikacji, należy dodać *dostawcy* atrybutu *browserCaps* sekcji `Web.config` lub `Machine.config` plików. (Można również zdefiniować atrybuty dostawcy w *lokalizacji* element do katalogów określonych w aplikacji, takie jak folder dla określonego urządzenia przenośnego.) Poniższy przykład przedstawia sposób ustawiania *dostawcy* atrybutu w pliku konfiguracji:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Zarejestrować nową definicję możliwości przeglądarki na innym sposobem jest użycie kodu, jak pokazano w poniższym przykładzie:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Ten kod muszą działać w *aplikacji\_Start* zdarzenie `Global.asax` pliku. Wszelkie zmiany do *BrowserCapabilitiesProvider* klasy musi wystąpić przed wykonaniem jakiegokolwiek kodu aplikacji, aby mieć pewność, że pamięci podręcznej pozostaje w nieprawidłowym stanie dla rozpoznać *HttpCapabilitiesBase* obiektu.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Buforowanie obiektu HttpBrowserCapabilities

Powyższy przykład jest jednym z problemów, które będą działać kod zawsze niestandardowego dostawcy jest wywoływane, aby uzyskać *HttpBrowserCapabilities* obiektu. Przyczyną może być kilka razy podczas każdego żądania. W tym przykładzie kodu dla dostawcy zrobić wiele. Jednak jeśli kodu niestandardowego dostawcy wykonuje wiele pracy w celu uzyskania *HttpBrowserCapabilities* obiektów, to może wpłynąć na wydajność. Aby temu zapobiec, można buforować *HttpBrowserCapabilities* obiektu. Wykonaj następujące kroki:

1. Utwórz klasę pochodną *HttpCapabilitiesProvider*, takich jak w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    W tym przykładzie kodu generuje klucz pamięci podręcznej przez wywołanie niestandardowej metody BuildCacheKey i pobiera czas do pamięci podręcznej przez wywołanie metody GetCacheTime niestandardowej. Następnie kod dodaje rozpoznać *HttpBrowserCapabilities* obiektu w pamięci podręcznej. Obiekt można pobrać z pamięci podręcznej i ponownie wykorzystać na kolejnych żądań, które należy użyć niestandardowego dostawcy.
2. Zarejestruj dostawcę z aplikacją, zgodnie z opisem w poprzedniej procedurze.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Rozszerzanie funkcjonalności możliwości przeglądarki ASP.NET

Poprzedniej sekcji opisano sposób tworzenia nowego *HttpBrowserCapabilities* obiektu w ASP.NET 4. Funkcje możliwości przeglądarki ASP.NET można rozszerzać przez dodawanie nowych definicji możliwości przeglądarki do tych, które znajdują się już w programie ASP.NET. Można to zrobić bez użycia XML definicji przeglądarki. Poniżej przedstawiono procedurę sposób.

1. Utwórz klasę pochodną *HttpCapabilitiesEvaluator* i który zastępuje *GetBrowserCapabilities* metody, jak pokazano w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Ten kod najpierw używa funkcji możliwości przeglądarki ASP.NET celu zidentyfikowania w przeglądarce. Jednak jeśli przeglądarka nie została zidentyfikowana na podstawie informacje zdefiniowane w żądaniu (to znaczy, jeśli *przeglądarki* właściwość *HttpBrowserCapabilities* obiekt jest ciąg "Nieznany"), kod wywołuje niestandardowy dostawca (MyBrowserCapabilitiesEvaluator), aby zidentyfikować w przeglądarce.
2. Zarejestruj dostawcę z aplikacją, zgodnie z opisem w poprzednim przykładzie.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Rozszerzanie funkcjonalności możliwości przeglądarki przez dodawanie nowych funkcji do istniejącej definicji funkcji

Oprócz tworzenia dostawcy definicji przeglądarki niestandardowych i dynamicznego tworzenia nowych definicji przeglądarki można rozszerzyć istniejące definicje przeglądarki dodatkowe funkcje. Dzięki temu można użyć definicji, który znajduje się w pobliżu co ma, ale nie ma tylko kilka możliwości. Aby to zrobić, wykonaj następujące kroki.

1. Utwórz klasę pochodną *HttpCapabilitiesEvaluator* i który zastępuje *GetBrowserCapabilities* metody, jak pokazano w poniższym przykładzie: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Przykładowy kod rozszerza istniejącą platformę ASP.NET *HttpCapabilitiesEvaluator* klasy i pobiera *HttpBrowserCapabilities* obiekt, który odpowiada bieżącej definicji żądania przy użyciu poniższego kodu :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kod można następnie dodać lub zmodyfikować możliwości tej przeglądarki. Istnieją dwa sposoby określania nowej możliwości przeglądarki:

    - Dodaj parę klucz wartość do *IDictionary* obiektu uwidocznionego przez *możliwości* właściwość *HttpCapabilitiesBase* obiektu. W poprzednim przykładzie, ten kod dodaje możliwości o nazwie MultiTouch o wartości *true*.
    - Ustawianie właściwości istniejącego *HttpCapabilitiesBase* obiektu. W poprzednim przykładzie, ustawia kod *ramki* właściwości *true*. Ta właściwość jest po prostu metody dostępu dla *IDictionary* obiektu uwidocznionego przez *możliwości* właściwości. 

        > [!NOTE]
        > Należy zauważyć ten model ma zastosowanie do żadnej właściwości *HttpBrowserCapabilities*, w tym kart kontrolki.
2. Zarejestruj dostawcę z aplikacją, zgodnie z opisem w procedurze wcześniej.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing w programie ASP.NET 4

ASP.NET 4 dodaje wbudowaną obsługę przy użyciu routingu z formularzy sieci Web. Routingu umożliwia skonfigurowanie aplikacji do akceptowania żądań adresów URL, które nie są mapowane na pliki fizyczne. Można zamiast tego należy używać routingu do definiowania adresów URL, które są przydatne dla użytkowników i które mogą pomóc z optymalizacji dla aparatów wyszukiwania (SEO) dla aplikacji. Na przykład adres URL strony, która przedstawia kategorie produktów w istniejącej aplikacji może wyglądać następująco:

[!code-console[Main](overview/samples/sample36.cmd)]

Przy użyciu routingu, można skonfigurować aplikację do akceptowania następujący adres URL do renderowania tych samych informacji:

[!code-console[Main](overview/samples/sample37.cmd)]

Routing została dostępnych w programie ASP.NET 3.5 z dodatkiem SP1. (Na przykład jak korzystać z routingu platformy ASP.NET 3.5 z dodatkiem SP1, zobacz wpis [przy użyciu routingu z WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "tytuł tego wpisu.") w blogu Phila Haacka.) Jednak programu ASP.NET 4 zawiera niektóre funkcje, które ułatwiają korzystać z routingu, takie jak następujące:

- *PageRouteHandler* klasy, która jest prosty program obsługi HTTP, który jest używany podczas definiowania tras. Klasa przekazuje dane do strony żądanie jest kierowane do.
- Nowe właściwości *HttpRequest.RequestContext* i *Page.RouteData* (która jest serwer proxy dla *HttpRequest.RequestContext.RouteData* obiektu). Te właściwości ułatwić dostęp do informacji przekazywanych z trasy.
- Następujące konstruktorów wyrażeń nowe, które są zdefiniowane w *System.Web.Compilation.RouteUrlExpressionBuilder* i *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, który zapewnia prosty sposób tworzenia adresu URL, który odpowiada adresowi URL trasy w formancie serwera ASP.NET.
- *RouteValue*, która umożliwia w prosty sposób wyodrębnić informacji z *RouteContext* obiektu.
- *RouteParameter* klasy, która ułatwia przekazywania danych zawartych w *RouteContext* obiektu do zapytania do kontroli źródła danych (podobnie jak [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing dla stron formularzy sieci Web

Poniższy przykład przedstawia sposób definiowania tras formularzy sieci Web przy użyciu nowej *MapPageRoute* metody *trasy* klasy:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 wprowadza *MapPageRoute* metody. W poniższym przykładzie jest odpowiednikiem definicji SearchRoute w poprzednim przykładzie, ale używa *PageRouteHandler* klasy.

[!code-csharp[Main](overview/samples/sample39.cs)]

Kod w przykładzie mapuje trasę do strony fizycznej (w pierwszym trasy do `~/search.aspx`). Pierwszy definicję trasy określa również, że wyodrębniony z adresu URL i przekazane do strony parametr o nazwie searchterm.

*MapPageRoute* metoda obsługuje następujące przeciążenia metody:

- *MapPageRoute (ciąg routeName, routeUrl ciąg, ciąg funkcji, bool checkPhysicalUrlAccess)*
- *MapPageRoute (ciąg routeName, routeUrl ciąg, ciąg funkcji, bool checkPhysicalUrlAccess, RouteValueDictionary domyślne)*
- *MapPageRoute (routeName ciągu, routeUrl ciąg, ciąg funkcji, bool checkPhysicalUrlAccess, RouteValueDictionary wartości domyślne, ograniczenia RouteValueDictionary)*

*CheckPhysicalUrlAccess* parametr określa, czy trasa należy sprawdzić uprawnień zabezpieczeń dla strony fizycznej rozsyłane do (w tym przypadku search.aspx) i uprawnienia dotyczące przychodzącego adresu URL (w tym przypadku wyszukiwania / {searchterm}). Jeśli wartość *checkPhysicalUrlAccess* jest *false*, tylko uprawnienia przychodzącego adresu URL, zostanie sprawdzony. Te uprawnienia są zdefiniowane w `Web.config` plików przy użyciu ustawień, takich jak następujące:

[!code-xml[Main](overview/samples/sample40.xml)]

W konfiguracji przykład odmowa dostępu do strony fizycznej `search.aspx` dla wszystkich użytkowników z wyjątkiem tych, którzy są w roli administratora. Gdy *checkPhysicalUrlAccess* ustawiono parametr *true* (która jest jego wartość domyślna), tylko administrator użytkownicy będą mogli uzyskać dostępu do adresu URL /search/ {searchterm}, ponieważ jest search.aspx strony fizycznej ograniczony do użytkowników w tej roli. Jeśli *checkPhysicalUrlAccess* ustawiono *false* i lokację skonfigurowano tak jak pokazano w poprzednim przykładzie, mogą uzyskać dostępu do adresu URL /search/ {searchterm} wszystkim uwierzytelnionym użytkownikom.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Odczytywanie informacji o routingu w strony formularzy sieci Web

W kodzie strony fizycznej formularzy sieci Web, ma dostęp do informacji, który routingu został wyodrębniony z adresu URL (lub inne informacje, które dodane do innego obiektu *RouteData* obiektu) przy użyciu dwóch nowych właściwości:  *HttpRequest.RequestContext* i *Page.RouteData*. (*Page.RouteData* opakowuje *HttpRequest.RequestContext.RouteData*.) Poniższy przykład przedstawia użycie *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Kod pobiera wartość, która została przekazana dla parametru searchterm zgodnie z definicją w trasie przykład wcześniej. Rozważmy następujący adres URL żądania:

[!code-console[Main](overview/samples/sample42.cmd)]

Nawiązaniem tego żądania, wyraz "scott" może być renderowane w `search.aspx` strony.

#### <a name="accessing-routing-information-in-markup"></a>Uzyskiwanie dostępu do informacji o routingu w znaczniku

Metody opisanej w poprzedniej sekcji pokazano, jak można pobrać danych trasy w kodzie strony formularzy sieci Web. Umożliwia także wyrażeń w znaczniku, które umożliwiają dostęp do tych samych informacji. Konstruktorów wyrażeń są wydajne i elegancki sposób pracy z deklaratywne kodu. (Aby uzyskać więcej informacji, zobacz wpis [Express sobie z niestandardowych konstruktorów wyrażeń](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) na blogu Phila Haacka.)

Program ASP.NET 4 zawiera dwa nowe konstruktorów wyrażeń dla routingu formularzy sieci Web. Poniższy przykład przedstawia sposób ich użycia.

[!code-aspx[Main](overview/samples/sample43.aspx)]

W tym przykładzie *RouteUrl* wyrażenie służy do definiowania adres URL, który jest oparty na parametru trasy. Trzeba być zakodowane pełny adres URL do znaczników i pozwala później zmienić strukturą adresów URL bez konieczności zmiany tego łącza.

Na podstawie trasy zdefiniowanego wcześniej, to znaczników generuje następujący adres URL:

[!code-console[Main](overview/samples/sample44.cmd)]

Program ASP.NET jest automatycznie działa limit poprawnej trasy (to znaczy generuje poprawny adres URL) na podstawie parametrów wejściowych. Możesz również uwzględnić nazwę trasy w wyrażeniu, która umożliwia określenie trasy do użycia.

Poniższy przykład przedstawia użycie *RouteValue* wyrażenia.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Po uruchomieniu na stronie, zawierający ten formant, wartość "scott" jest wyświetlana w etykiecie.

*RouteValue* wyrażenie ułatwia użyć danych trasy w znaczniku i jego pozwala uniknąć konieczności pracować z bardziej złożonych Page.RouteData["x"] składni w znaczniku.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Przy użyciu danych trasy dla parametrów kontroli źródła danych

*RouteParameter* klasy pozwala określić dane trasy jako wartość parametru zapytania w kontroli źródła danych. On [działa podobnie jak](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) klasy, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample46.aspx)]

W takim przypadku zostanie użyta wartość searchterm parametru trasy dla @companyname parametru w <em>wybierz</em> instrukcji.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Ustawienia klienta identyfikatorów

Nowy *ClientIDMode* właściwość dotyczy długotrwałego problemu w programie ASP.NET, a mianowicie formanty tworzenia *identyfikator* atrybutu dla elementów, które renderują. Znajomość *identyfikator* atrybut elementów renderowanych jest ważne, jeśli aplikacja zawiera skrypt klienta, który odwołuje się do tych elementów.

*Identyfikator* atrybutu HTML, który jest renderowany kontrolek serwera sieci Web jest generowany na podstawie *ClientID* właściwości formantu. Do programu ASP.NET 4, algorytm generuje *identyfikator* atrybutu z *ClientID* właściwość została do łączenia kontenera nazewnictwa (jeśli istnieją) z Identyfikatorem, a w przypadku wielokrotnego formantów (podobnie jak w formanty danych), aby dodać prefiks i numer kolejny. Gdy ma to zawsze gwarancję, że identyfikatorów kontrolki na stronie są unikatowe, algorytm sprawia, że formant identyfikatorów, które nie były przewidywalną i w związku z tym były trudne do odwołania w skrypt po stronie klienta.

Nowy *ClientIDMode* właściwość umożliwia określenie, bardziej precyzyjne, jak identyfikator klienta jest generowany dla formantów. Można ustawić *ClientIDMode* właściwości dla każdego formantu, w tym dla strony. Ustawienia możliwe są następujące:

- *Identyfikator automatyczny* — jest to równoważne algorytmu generowania *ClientID* wartości właściwości, które zostało użyte we wcześniejszych wersjach programu ASP.NET.
- *Statyczne* — to oznacza, że *ClientID* wartość będzie taki sam jak identyfikator bez łączenie identyfikatorów nazw kontenery elementu nadrzędnego. Może to być przydatne w przypadku kontrolek użytkownika sieci Web. Kontrolka użytkownika sieci Web mogą być znajduje się na różnych stronach i w różnych kontenerów formantów, może być trudne do zapisywania skryptu klienta dla formantów, które używają *identyfikator automatyczny* algorytmu, ponieważ nie można przewidzieć wartości identyfikatorów, jakie będą .
- *Przewidywalna* — ta opcja jest głównie do użytku w formantów danych, które używają szablonów powtarzających się. Łączy jego właściwości ID formantu nazw kontenerów, ale wygenerowanego *ClientID* wartości nie zawierają parametrów, takich jak "ctlxxx". To ustawienie działa w połączeniu z *ClientIDRowSuffix* właściwości formantu. Możesz ustawić *ClientIDRowSuffix* nazwę pola danych i wartość tego pola jest używana jako sufiks dla wygenerowanej *ClientID* wartości. Zwykle użyje rekord danych jako klucz podstawowy *ClientIDRowSuffix* wartości.
- *Dziedzicz* — to ustawienie jest domyślnie formantach; Określa, czy Generowanie Identyfikatora formantu jest taka sama jak jego obiekt nadrzędny.

Można ustawić *ClientIDMode* właściwości na poziomie strony. Definiuje domyślny *ClientIDMode* wartości dla wszystkich kontrolek w bieżącej strony.

Wartość domyślna *ClientIDMode* wartość na poziomie strony jest *identyfikator automatyczny*, wartością domyślną *ClientIDMode* wartość na poziomie formantu jest *Dziedzicz*. W związku z tym, jeśli ta właściwość nie ustawiona dowolne miejsce w kodzie, wszystkie formanty domyślnie zostanie ustawiona *identyfikator automatyczny* algorytmu.

Ustaw wartość na poziomie strony *@ Page* dyrektywy, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Można również ustawić *ClientIDMode* wartości w pliku konfiguracji na poziomie komputera (komputer) lub na poziomie aplikacji. Definiuje domyślny *ClientIDMode* ustawienie dla wszystkich kontrolek na wszystkich stronach w aplikacji. Jeśli wartość jest ustawiona na poziomie komputera, określa ona domyślnie *ClientIDMode* ustawienie dla wszystkich witryn sieci Web na tym komputerze. W poniższym przykładzie przedstawiono *ClientIDMode* ustawienia w pliku konfiguracji:

[!code-xml[Main](overview/samples/sample48.xml)]

Jak wspomniano wcześniej, wartość *ClientID* właściwości jest pochodną elementu nadrzędnego formantu kontenera nazewnictwa. W niektórych scenariuszach, na przykład w przypadku używania stron wzorcowych formantów może zakończyć z identyfikatorami podobnie jak w poniższym renderowania kodu HTML:

[!code-html[Main](overview/samples/sample49.html)]

Mimo że *wejściowych* wyświetlany w znaczniku elementu (z *pole tekstowe* kontroli) jest tylko dwa kontenery nazewnictwa głębokość na stronie (zagnieżdżone *ContentPlaceholder* formantów), ze względu na sposób, w jaki są przetwarzane strony wzorcowe wynik końcowy jest identyfikator formantu, takie jak następujące:

[!code-console[Main](overview/samples/sample50.cmd)]

Ten identyfikator jest musi być unikatowy w strony, ale jest niepotrzebnie długo w większości przypadków. Załóżmy, że chcesz zmniejszyć długość Identyfikatora renderowanych i większą kontrolę nad jak jest generowany identyfikator. (Na przykład chcesz wyeliminować prefiksy "ctlxxx".) Najprostszym sposobem, aby to osiągnąć, jest przez ustawienie *ClientIDMode* właściwości, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample51.aspx)]

W tym przykładzie *ClientIDMode* właściwość jest ustawiona na *statycznych* dla zewnętrznych *NamingPanel* elementu i ustawiono *przewidywalny* dla wewnętrznej *NamingControl* elementu. Te ustawienia powodują następujący kod znaczników (pozostałe strony i strony wzorcowej zakłada się, że takie same jak w poprzednim przykładzie):

[!code-html[Main](overview/samples/sample52.html)]

*Statycznych* ustawienie ma wpływ Resetowanie nazewnictwa hierarchii dla wszystkich formantów w najbardziej zewnętrznej *NamingPanel* elementu i eliminowania *ContentPlaceHolder* i *MasterPage* identyfikatory z wygenerowanego identyfikatora. ( *Nazwa* nie wpływa na atrybut renderowanych elementów, dlatego normalną funkcjonalność platformy ASP.NET został zachowany na potrzeby zdarzeń, wyświetlić stan i itd.) Efektem ubocznym Resetowanie hierarchii nazewnictwa jest to, że nawet jeśli przeniesiesz kod znaczników dla *NamingPanel* elementy z innym *ContentPlaceholder* kontroli, identyfikatory renderowanych klienta pozostają takie same.

> [!NOTE]
> Należy pamiętać, że jest od użytkownika, aby upewnić się, że identyfikatory renderowanych sterowania są unikatowe. Jeśli nie, może spowodować nieprawidłowe żadnej funkcji, która wymaga unikatowych identyfikatorów dla poszczególnych elementów HTML, takich jak klient *document.getElementById* funkcji.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Tworzenie identyfikatorów przewidywalną klienta w formantach powiązanych z danymi

*ClientID* wartości, które są generowane dla formantów w formancie listy powiązane z danymi przez algorytm starszej wersji może być długa i nie są naprawdę przewidywalne. *ClientIDMode* funkcje ułatwiają mieć większą kontrolę nad jak te identyfikatory są generowane.

Znaczniki w poniższym przykładzie obejmuje *ListView* sterowania:

[!code-aspx[Main](overview/samples/sample53.aspx)]

W poprzednim przykładzie *ClientIDMode* i *RowClientIDRowSuffix* właściwości są ustawione w znaczniku. *ClientIDRowSuffix* właściwości można używać tylko w formantach powiązanych z danymi i jego zachowanie różni się w zależności od kontroli, których używasz. Różnice są one:

- *Element GridView* kontroli — można określić nazwę co najmniej jedną kolumnę w źródle danych, które są łączone w czasie wykonywania można utworzyć klienta identyfikatorów. Na przykład jeśli ustawisz *RowClientIDRowSuffix* do "ProductName, ProductId", kontrolować identyfikatory elementów renderowanych będzie miał format podobnie do następującej:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *Element ListView* kontroli — można określić pojedynczej kolumny w źródle danych, która jest dołączana do identyfikator klienta. Na przykład jeśli ustawisz *ClientIDRowSuffix* "ProductName" renderowanych kontroli identyfikatory, będzie miała następujący format:

- [!code-console[Main](overview/samples/sample55.cmd)]

- W takim przypadku końcowe 1 jest pochodną identyfikator produktu bieżącego elementu danych.

- *Elementu powtarzanego* kontroli — ten formant nie obsługuje *ClientIDRowSuffix* właściwości. W *elementu powtarzanego* służy kontroli, indeks bieżącego wiersza. Jeśli używasz ClientIDMode = "Przewidywalny" with *elementu powtarzanego* kontroli klienta są generowane, identyfikatory, który ma następujący format:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Końcowych 0 jest indeks bieżącego wiersza.

*FormView* i *widoku DetailsView* formanty nie są wyświetlane wiele wierszy, które nie obsługują *ClientIDRowSuffix* właściwości.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Utrwalanie zaznaczenie wiersza w formantach danych

*GridView* i *ListView* formantów pozwolić użytkownikom na wybór wiersza. W poprzednich wersjach programu ASP.NET została oparta na indeks wiersza na stronie wyboru. Na przykład jeśli Wybierz trzeci element na stronie 1, a następnie przejść do strony 2, trzeci element na tej stronie jest zaznaczony.

Wybór trwały początkowo była obsługiwana tylko w projektach dane dynamiczne w .NET Framework 3.5 SP1. Ta funkcja jest włączona, bieżący wybrany element jest oparty na danych klucza dla elementu. Oznacza to, że jeśli wybierz trzeciego wiersza na stronie 1 i przejść do strony 2, nic nie zostanie wybrane na stronie 2. W przypadku przenoszenia powrót do strony 1 trzeciego wiersza nadal jest zaznaczone. Wybór trwały jest teraz obsługiwana przez *GridView* i *ListView* formanty we wszystkich projektach przy użyciu *EnablePersistedSelection* właściwości, jak pokazano w Poniższy przykład:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Formant wykresu ASP.NET

ASP.NET *wykresu* kontroli rozszerza ofert wizualizacji danych w programie .NET Framework. Przy użyciu *wykresu* formantu, można utworzyć stron ASP.NET, które mają intuicyjne i wizualny wykresy złożone analizy statystycznej lub finansowych. ASP.NET *wykresu* kontroli została wprowadzona jako dodatek do wersji .NET Framework w wersji 3.5 z dodatkiem SP1 i jest częścią wersji .NET Framework 4.

Formant zawiera następujące funkcje:

- 35 typów wykresów distinct.
- Nieograniczoną liczbę obszarów wykresu, tytułów legendy i adnotacje.
- Szerokiej gamy wygląd ustawienia dla wszystkich elementów wykresu.
- Obsługa 3-w przypadku większości typów wykresów.
- Etykiety inteligentne danych, które można automatycznie Dopasuj wokół punktów danych.
- Pasków podziałów skali i logarytmicznej.
- Więcej niż 50 finansowych i statystycznych formuł do analizowania danych i przekształcania.
- Proste powiązanie i modyfikowanie danych wykresu.
- Obsługa wspólnej formatów danych, takich jak daty, godziny i waluty.
- Obsługa interakcyjności i dostosowanie sterowane zdarzeniami, w tym klienta kliknij zdarzeń za pomocą interfejsu Ajax.
- Stan zarządzania.
- Binarny przesyłania strumieniowego.

Poniższe rysunki pokazują przykładowe wykresy finansowych, które są tworzone przez formant wykresu ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Rysunek 2: Przykłady formantów wykresu ASP.NET

Dla więcej przykładów sposobu użycia formantu wykresu programu ASP.NET, Pobierz przykładowy kod na [przykłady środowisko do kontrolek wykresów Microsoft](https://go.microsoft.com/fwlink/?LinkId=128300) stronę w witrynie MSDN. Więcej przykładów społeczności można znaleźć zawartości w [Forum formantu wykresu](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Dodawanie formantu wykresu strony ASP.NET

Poniższy przykład przedstawia sposób dodawania *wykresu* formantu strony ASP.NET przy użyciu znaczników. W tym przykładzie *wykresu* kontroli tworzy wykres kolumnowy dla punktów danych statycznych.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Przy użyciu wykresów 3-w

*Wykresu* formant zawiera *ChartAreas* kolekcji, która może zawierać *obszar wykresu* obiekty, które określają właściwości obszarów wykresu. Na przykład, aby użyć 3W obszaru wykresu, należy użyć *Area3DStyle* właściwości, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Poniższy rysunek przedstawia wykres 3-z czterech serii *paska* typ wykresu.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Rysunek 3: wykres słupkowy 3-

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Przy użyciu podziałów skali i skali logarytmicznej

Podziały skali i skal logarytmicznych są dwa inne sposoby dodawania wiedzy do wykresu. Te funkcje są specyficzne dla poszczególnych osi w obszarze wykresu. Na przykład, aby używać tych funkcji podstawowej osi Y obszaru wykresu, należy użyć *AxisY.IsLogarithmic* i *ScaleBreakStyle* właściwości w *obszar wykresu* obiektu. Poniższy fragment kodu przedstawia sposób użycia podziałów skali na osi Y podstawowej.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Na poniższym rysunku przedstawiono osi Y, z włączoną podziałów skali.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Rysunek 4: Podziałów skali

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrowanie danych za pomocą formantu klasą QueryExtender

Jest bardzo typowych zadań dla deweloperów, którzy tworzenia stron sieci Web opartych na danych do filtrowania danych. To tradycyjnie ma zostać wykonane przez utworzenie *gdzie* klauzul w danych źródłowych kontrolek. Ta metoda może być skomplikowane, a w niektórych przypadkach *gdzie* składni nie pozwala na korzystanie z pełnej funkcjonalności podstawowej bazy danych.

Aby łatwiej, filtrowania nowy *klasą QueryExtender* formant został dodany w programie ASP.NET 4. Ten formant można dodać do *obiektu EntityDataSource* lub *LinqDataSource* służy do filtrowania danych zwróconych przez tych kontrolek. Ponieważ *klasą QueryExtender* kontroli zależy od LINQ, przed wysłaniem danych do strony, co powoduje w bardzo wydajny operacji na serwerze bazy danych jest stosowany filtr.

*Klasą QueryExtender* formantu obsługuje wiele różnych opcji filtrowania. W poniższych sekcjach opisano te opcje i zawierają przykłady sposobu ich używania.

#### <a name="search"></a>Wyszukaj

W przypadku opcji wyszukiwania *klasą QueryExtender* kontroli wykonuje wyszukiwanie w określonych polach. W poniższym przykładzie formantu używa tekst, który jest wprowadzana w kontroli TextBoxSearch i wyszukuje jego zawartość w `ProductName` i `Supplier.CompanyName` kolumny danych, która jest zwracana z *LinqDataSource* formant.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Zakres

Opcja zakresu działa podobnie do opcji wyszukiwania, ale Określa pary wartości, aby zdefiniować zakres. W poniższym przykładzie *klasą QueryExtender* kontrolować wyszukiwania `UnitPrice` kolumny z danymi zwróconymi z *LinqDataSource* formantu. Zakresem są odczytywane z TextBoxFrom i TextBoxTo kontrolki na stronie.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Opcja wyrażenie właściwości pozwala zdefiniować porównanie wartości właściwości. Jeśli wyrażenie ma *true*, jest zwracany w danych, który jest kontrolowany. W poniższym przykładzie *klasą QueryExtender* kontroli filtry danych na podstawie porównania ilości danych w `Discontinued` kolumny, która ma wartość z formantu CheckBoxDiscontinued na stronie.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Ponadto można określić wyrażenia niestandardowego do użycia z *klasą QueryExtender* formantu. Ta opcja umożliwia wywołania funkcji w strony, który definiuje logiki filtru niestandardowego. Poniższy przykład przedstawia sposób deklaratywnego określić niestandardowe wyrażenie w *klasą QueryExtender* formantu.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Poniższy przykład przedstawia funkcję niestandardową, który jest wywoływany przez *klasą QueryExtender* formantu. W takim przypadku zamiast zapytania bazy danych, które obejmuje *gdzie* klauzuli kod używa zapytania LINQ do filtrowania danych.

[!code-csharp[Main](overview/samples/sample65.cs)]

Tylko jedno wyrażenie używane w tych przykładach *klasą QueryExtender* formantu w czasie. Jednak może zawierać wiele wyrażeń wewnątrz *klasą QueryExtender* formantu.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Html Encoded Code Expressions

Niektóre witryny ASP.NET (zwłaszcza z platformą ASP.NET MVC) silnie zależy od usługi `<%` =  `expression %>` składni (często nazywane "code nuggets") do zapisania fragment tekstu w odpowiedzi. Użycie wyrażenia kodu, jest proste zapomnieć do zakodowania w formacie HTML wprowadzania tekstu, jeśli tekst pochodzi od użytkownika, jego można pozostawić strony Otwórz na ataki XSS (skryptów krzyżowych).

ASP.NET 4 wprowadza następujące nowe składni wyrażeń kodu:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Ta składnia używa kodowanie HTML domyślnie podczas zapisywania odpowiedzi. To nowe wyrażenie skutecznie tłumaczy do następującego:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Na przykład &lt;%: żądanie ["UserInput"] %&gt; wykonuje kodowanie HTML na wartość *żądania ["UserInput"]*.

Pozwala zastąpić wszystkie wystąpienia stara składnia z nowej składni, dzięki czemu nie będą obowiązkowo przenoszone do określania, w każdym kroku, który jest celem tej funkcji. Istnieją przypadki, w których tekst będący dane wyjściowe ma być HTML lub już został zakodowany w takim przypadku może prowadzić do kodowania dwa razy.

W tych przypadkach programu ASP.NET 4 wprowadzono nowy interfejs *IHtmlString*, wraz z konkretną implementację *HtmlString*. Wystąpienia typów, te umożliwiają wskazują, że zwracana wartość jest już poprawnie zaszyfrowana (lub w przeciwnym razie zbadane) do wyświetlania w formacie HTML oraz że dlatego wartość nie powinna być zakodowany w formacie HTML ponownie. Na przykład następujący nie powinien być (i nie jest) kodowania HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Metody pomocnicze ASP.NET MVC 2 zostały zaktualizowane do pracy z tej nowej składni tak, aby nie były dwa razy, zakodowany, ale tylko po uruchomieniu programu ASP.NET 4. Tej nowej składni nie działa podczas uruchamiania aplikacji przy użyciu programu ASP.NET 3.5 z dodatkiem SP1.

Należy pamiętać, że nie gwarantuje to ochronę przed atakami XSS. Na przykład HTML, który używa wartości atrybutów, które nie znajdują się w znaki cudzysłowu może zawierać danych wejściowych użytkownika, które są nadal podatne. Należy pamiętać, że dane wyjściowe kontrolek ASP.NET i pomocników platformy ASP.NET MVC zawsze zawiera wartości atrybutów w cudzysłowie, która jest zalecanym podejściem.

Podobnie ta składnia nie przeprowadza kodowania JavaScript, takie jak podczas tworzenia ciąg JavaScript w zależności od danych wejściowych użytkownika.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Zmiany szablonu projektu

We wcześniejszych wersjach programu ASP.NET, podczas tworzenia nowego projektu witryny sieci Web lub projektu aplikacji sieci Web za pomocą programu Visual Studio wynikowy projekty zawierają tylko Default.aspx stronę, domyślny `Web.config` pliku i `App_Data` folderu, jak pokazano w poniższym Ilustracja:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Program Visual Studio obsługuje również typ projektu pusta witryna sieci Web, który nie zawiera plików na wszystkich, jak pokazano na poniższej ilustracji:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Wynik jest, że dla początkujących, istnieje bardzo mało wskazówki na temat tworzenia aplikacji sieci Web w środowisku produkcyjnym. W związku z tym ASP.NET 4 przedstawiono trzy nowe szablony, jedno dla pusty projekt aplikacji sieci Web i jeden dla projektu aplikacji sieci Web i witryny sieci Web.

#### <a name="empty-web-application-template"></a>Szablon aplikacji sieci Web pusty

Jak wynika z nazwy, szablon pustej aplikacji sieci Web jest stripped-down projekt aplikacji sieci Web. Ten szablon projektu możesz wybierz w oknie dialogowym Nowy projekt programu Visual Studio, jak pokazano na poniższej ilustracji:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image8.png))

Gdy utworzysz pustą aplikację sieci Web ASP.NET, Visual Studio tworzy układ następujący folder:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Efekt jest podobny do układu pusta witryna sieci Web z wcześniejszych wersji programu ASP.NET, z jednym wyjątkiem. W programie Visual Studio 2010, projekty pustą aplikację sieci Web i pusta witryna sieci Web zawierają następujące minimalne `Web.config` pliku, który zawiera informacje używane przez program Visual Studio, aby zidentyfikować framework, który projekt jest docelowo:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Jeśli ta usługa nie *targetFramework* właściwości, Visual Studio, wartością domyślną będzie przeznaczonych dla programu .NET Framework 2.0 w celu zachowania zgodności podczas otwierania starsze aplikacje.

#### <a name="web-application-and-web-site-project-templates"></a>Szablony projektów witryny sieci Web i aplikacji sieci Web

Inne dwóch nowych szablonów projektu dostarczonych z programem Visual Studio 2010 zawiera istotne zmiany. Na poniższej ilustracji przedstawiono układ projektu, który jest tworzony podczas tworzenia nowego projektu aplikacji sieci Web. (Układu dla projektu witryny sieci Web jest praktycznie identyczny).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projekt zawiera wiele plików, które nie zostały utworzone we wcześniejszych wersjach. Ponadto nowy projekt aplikacji sieci Web skonfigurowano funkcję członkostwo podstawowe, która umożliwia szybkie rozpoczęcie pracy w zabezpieczaniu dostępu do nowej aplikacji. Ze względu na to włączenie `Web.config` pliku dla nowego projektu zawiera wpisów, które są używane do konfigurowania członkostwa, ról i profilów. W poniższym przykładzie przedstawiono `Web.config` plików dla nowego projektu aplikacji sieci Web. (W tym przypadku *roleManager* jest wyłączona.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image14.png))

Projekt zawiera także drugi `Web.config` w pliku `Account` katalogu. Drugi plik konfiguracji umożliwia zapewnienie dostępu do strony ChangePassword.aspx z systemem innym niż zalogowany użytkowników. W poniższym przykładzie przedstawiono zawartość drugiego `Web.config` pliku.

![](overview/_static/image15.png)

Stron tworzonych domyślnie w nowych szablonów projektu również zawierać więcej zawartości niż w poprzednich wersjach. Projekt nie zawiera domyślnej strony wzorcowej i pliku CSS, a domyślna strona (Default.aspx) jest domyślnie konfigurowana do używania strony wzorcowej. Wynik jest uruchomienie witryny sieci Web lub aplikacji sieci Web po raz pierwszy, domyślna strona (macierzysty) już funkcjonalności. W rzeczywistości jest podobny do domyślnej strony widocznej przy uruchamianiu nowej aplikacji MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image18.png))

Tych zmian do szablonów projektu jest zapewnienie wskazówki na temat rozpocząć tworzenie nowej aplikacji sieci Web. Z semantycznie poprawny, ograniczeniami XHTML 1.0 zgodne kod znaczników i układu, który jest określany przy użyciu CSS stron w szablonach reprezentują najlepsze rozwiązania dotyczące tworzenia aplikacji sieci Web programu ASP.NET 4. Domyślne strony ma także układu dwie kolumny, które można łatwo dostosować.

Załóżmy na przykład dla nowej aplikacji sieci Web chcesz zmienić niektóre kolory i Wstaw logo firmy, zamiast logo Moja aplikacja platformy ASP.NET. W tym celu należy utworzyć nowy katalog w obszarze `Content` do przechowywania obrazu logo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Aby dodać obraz do strony, należy następnie otworzyć `Site.Master` plików, których zdefiniowano tekst Moja aplikacja platformy ASP.NET i go zastąpić *obrazu* element których *src* atrybut ma ustawioną nowe logo obraz, jak w poniższym przykładzie:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image22.png))

Następnie można przejść do pliku Site.css i modyfikować definicje klas CSS, aby zmienić kolor tła strony, a także nagłówka.

Wynikiem tych zmian jest, że można wyświetlić dostosowanej strony głównej przy bardzo małego wysiłku:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Ulepszenia CSS

Jedną z głównych obszarów roboczych w programie ASP.NET 4 została aby renderować kod HTML, który jest zgodny z najnowszych standardów HTML. Obejmuje to zmian w sposób kontrolki serwera sieci Web ASP.NET używaj stylów CSS.

#### <a name="compatibility-setting-for-rendering"></a>Ustawienia zgodności do renderowania

Domyślnie, gdy witryna sieci Web lub aplikacji sieci Web jest przeznaczony dla programu .NET Framework 4 *controlRenderingCompatibilityVersion* atrybutu *stron* element jest ustawiony na wartość "4.0". Ten element jest zdefiniowana na poziomie maszyny `Web.config` plików i domyślnie ma zastosowanie do wszystkich aplikacji programu ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Wartość *controlRenderingCompatibility* jest ciągiem, dzięki czemu potencjalnych nowych wersji definicji w przyszłych wersjach. W bieżącej wersji dla tej właściwości, obsługiwane są następujące wartości:

- "3.5". To ustawienie wskazuje renderowanie starsze i znaczników. Znaczników renderowania formantów jest zgodne z poprzednimi wersjami 100%, a ustawienie *xhtmlConformance* jest honorowane właściwości.
- "4.0". Jeśli właściwość ma to ustawienie, kontrolki serwera sieci Web ASP.NET, wykonaj następujące czynności:
- *XhtmlConformance* właściwość zawsze jest traktowana jako "Strict". W związku z tym formanty renderowania kodu znaczników XHTML 1.0 Strict.
- Wyłączenie formantów z systemem innym niż dane wejściowe nie jest już renderuje nieprawidłowy style.
- *DIV* elementy wokół ukryte pola są teraz stylem, więc nie zakłócają reguły CSS utworzonych przez użytkownika.
- Formanty menu renderować kod znaczników, który jest semantycznie prawidłowe i zgodne z wytycznymi ułatwień dostępu.
- Formanty walidacji nie są wbudowane style.
- Określa, który wcześniej renderowany border = "0" (formantów, które pochodzą z platformy ASP.NET *tabeli* kontroli i platformy ASP.NET *obrazu* kontroli) już renderowania tego atrybutu.

#### <a name="disabling-controls"></a>Wyłączenie formantów

W programie ASP.NET 3.5 z dodatkiem SP1 i wcześniejszych wersjach renderuje platformę *wyłączone* atrybutu w kod znaczników HTML, wszelkie kontroli, których *włączone* ustawioną właściwość *false*. Jednak zgodnie ze specyfikacją HTML 4.01 tylko *wejściowych* elementy powinny mieć tego atrybutu.

W przypadku programu ASP.NET 4, można ustawić *controlRenderingCompatabilityVersion* dla właściwości "3.5", jak w poniższym przykładzie:

[!code-xml[Main](overview/samples/sample70.xml)]

Można utworzyć kod znaczników dla *etykiety* kontroli podobnie do poniższego, która wyłącza formant:

[!code-aspx[Main](overview/samples/sample71.aspx)]

*Etykiety* formant będzie renderować poniższy kod HTML:

[!code-html[Main](overview/samples/sample72.html)]

W przypadku programu ASP.NET 4, można ustawić *controlRenderingCompatabilityVersion* "4.0". W takim przypadku tylko steruje tym renderowania *wejściowych* elementy będą zawierały *wyłączone* atrybutu, gdy formantu *włączone* właściwość jest ustawiona na *false* . Formanty, które nie są HTML *wejściowych* zamiast renderowania elementów *klasy* atrybut, który odwołuje się do klasy CSS, która służy do definiowania wyłączone wygląd formantu. Na przykład *etykiety* kontroli pokazano w przykładzie wcześniejszych wygenerowanie następujący kod:

[!code-html[Main](overview/samples/sample73.html)]

Wartość domyślna dla klasy, która określona dla tego formantu to "aspNetDisabled". Jednak można zmienić tę wartość domyślną, ustawiając statycznych *DisabledCssClass* statycznej właściwości *WebControl* klasy. Dla deweloperów kontroli zachowania do użycia na potrzeby określonego formantu można zdefiniować przy użyciu *SupportsDisabledAttribute* właściwości.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ukrywanie div elementy wokół ukryte pola

ASP.NET 2.0 i nowszych wersjach Renderuj ukryte pola specyficzne dla systemu (takich jak *ukryte* element używany do przechowywania informacji o stanie widoku) wewnątrz *div* element, aby można było zgodne z normą XHTML. Jednak może to spowodować problem podczas regułę CSS ma wpływ na *div* elementów na stronie. Na przykład może to skutkować linii jednego piksela znajdujących się na stronie około ukryte *div* elementów. W przypadku programu ASP.NET 4 *div* elementy, które należy ująć ukryte pola wygenerowane przez platformę ASP.NET dodać odwołania do klasy CSS, jak w poniższym przykładzie:

[!code-html[Main](overview/samples/sample74.html)]

Można zdefiniować klasę CSS, która ma zastosowanie tylko do *ukryte* elementy, które są generowane przez platformę ASP.NET, jak w poniższym przykładzie:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Renderowanie tabeli zewnętrznej dla formantów opartego na szablonie

Domyślnie następujące kontrolki serwera sieci Web ASP.NET, które obsługują szablony są automatycznie ujęte w tabeli zewnętrznej, która jest używana do stosowania style wbudowane:

- *FormView*
- *Logowanie*
- *PasswordRecovery*
- *ChangePassword*
- *Kreator*
- *CreateUserWizard*

Nowe właściwości o nazwie *właściwość RenderOuterTable* został dodany do tych kontrolek, które umożliwia Tabela zewnętrzna ma zostać usunięty z kod znaczników. Rozważmy na przykład poniższy przykład przedstawia *FormView* sterowania:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Ten kod znaczników renderuje następujące dane wyjściowe do strony, która obejmuje tabeli HTML:

[!code-html[Main](overview/samples/sample77.html)]

Aby zapobiec renderowany w tabeli, można ustawić *FormView* formantu *właściwość RenderOuterTable* właściwości, jak w poniższym przykładzie:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Poprzedni przykład renderuje następujące wyniki bez *tabeli*, *tr*, i *td* elementy:

> Zawartość


To rozszerzenie można ułatwić styl zawartość kontrolki z CSS, ponieważ nie nieoczekiwany tagi są renderowane przez formant.

> [!NOTE]
> Ta zmiana wyłącza obsługę funkcji automatycznego formatowania w projektancie programu Visual Studio 2010, ponieważ istnieje już Uwaga *tabeli* element, który może obsługiwać atrybuty stylu, które są generowane przez opcję automatycznego formatowania.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Rozszerzenia formantu ListView

*ListView* kontroli jest łatwiejsze do użycia w ASP.NET 4. Starszą wersję formantu wymaga, aby określić szablon układu, który zawiera kontrolki serwera, ze znanym identyfikatorem. Następujący kod przedstawia typowym przykładem przedstawiającym sposób użycia *ListView* sterowania w programie ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

W przypadku programu ASP.NET 4 *ListView* formantu nie wymaga szablon układu. Można zastąpić następujący kod znaczników w poprzednim przykładzie:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList i ulepszenia RadioButtonList formantów

W programie ASP.NET 3.5, można określić układ *CheckBoxList* i *RadioButtonList* przy użyciu następujące dwa ustawienia:

- *Przepływ*. Renderowanie formantu *span* elementy do jego zawartości.
- *Tabela*. Renderowanie formantu *tabeli* elementu do jego zawartości.

W poniższym przykładzie pokazano kod znaczników dla każdego z tych kontrolek.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Domyślnie przez formanty renderować kod HTML podobny do następującego:

[!code-html[Main](overview/samples/sample82.html)]

Ponieważ te formanty zawierają listę elementów do renderowania elementów HTML semantycznie poprawne renderują powinny ich zawartość przy użyciu listy HTML (*li*) elementów. Ułatwia użytkownikom, którzy odczyt stron sieci Web przy użyciu technologii pomocniczej i ułatwia kontrolki do stylu przy użyciu CSS.

W przypadku programu ASP.NET 4 *CheckBoxList* i *RadioButtonList* formanty obsługują następujące nowe wartości *RepeatLayout* właściwości:

- *OrderedList* — jest renderowana zawartość *li* elementów w obrębie *ol* elementu.
- *UnorderedList* — jest renderowana zawartość *li* elementów w obrębie *ul* elementu.

Poniższy przykład przedstawia sposób użycia tych nowych wartości.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Poprzedni kod znaczników generuje poniższy kod HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Należy pamiętać, należy ustawić *RepeatLayout* do *OrderedList* lub *UnorderedList*, *RepeatDirection* właściwości nie mogą być używane i będzie Zgłoś wyjątek w czasie wykonywania, jeśli właściwość została ustawiona w znaczników lub kodu. Właściwość musi żadnej wartości, ponieważ zdefiniowano układu wizualnego tych kontrolek, zamiast niego CSS.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Ulepszenia formant menu

Przed programu ASP.NET 4 *Menu* kontroli renderowane serii tabel HTML. Utrudniało dotyczyć style CSS poza wbudowanej właściwości, a nie był również zgodne ze standardami ułatwień dostępu.

W programie ASP.NET 4 formantu renderuje HTML za pomocą semantyki kod znaczników, który składa się z nieuporządkowaną listę i elementy listy. W poniższym przykładzie pokazano kod znaczników strony ASP.NET dla *Menu* formantu.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Gdy renderuje stronę, formantu tworzy poniższy kod HTML ( *onclick* kodu zostało pominięte dla jasności):

[!code-html[Main](overview/samples/sample86.html)]

Oprócz renderowania usprawnień nawigacji klawiatury menu udoskonalono za pomocą zarządzania fokus. Gdy *Menu* formant uzyska fokus, możesz użyć klawiszy strzałek, aby przejść elementów. *Menu* kontroli teraz również dołącza dostępny bogaty ról aplikacji (programu ARIA) internet i atrybuty usta[następującą](http://www.w3.org/TR/wai-aria-practices/#menu "wytyczne Menu programu ARIA")na lepsze ułatwienia dostępu.

Style formantu menu są renderowane w bloku stylu w górnej części strony, a nie z renderowanych elementów HTML. Jeśli chcesz wykonać pełną kontrolę nad stylów formantu, możesz ustawić nowy *IncludeStyleBlock* właściwości *false*, w tym przypadku nie jest wysyłanego bloku stylu. Sposób użycia tej właściwości jest Użyj funkcji automatycznego formatowania w projektancie programu Visual Studio, aby określić wygląd menu. Można następnie uruchomić stronę, Otwórz źródło strony, a następnie skopiuj bloku stylu renderowanych do zewnętrznego pliku CSS. W programie Visual Studio, Cofnij stylami i zestaw *IncludeStyleBlock* do *false*. Wynik jest, że wygląd menu jest definiowana za pomocą stylów w zewnętrznego arkusza stylów.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Kreator i CreateUserWizard formantów

ASP.NET *kreatora* i *CreateUserWizard* formanty obsługuje szablonów, które umożliwiają definiowanie kodu HTML, które renderują. (*CreateUserWizard* pochodną *kreatora*.) W poniższym przykładzie pokazano kod znaczników dla pełni szablonem *CreateUserWizard* sterowania:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Formant renderuje HTML podobny do następującego:

[!code-html[Main](overview/samples/sample88.html)]

W programie ASP.NET 3.5 z dodatkiem SP1, mimo że można zmienić zawartość szablonu możesz nadal mają ograniczoną kontrolę nad dane wyjściowe *kreatora* formantu. W przypadku programu ASP.NET 4, można utworzyć *LayoutTemplate* szablon i Wstaw *symbolu zastępczego* kontrolki (przy użyciu nazw zastrzeżonych), aby określić sposób *formantu kreatora* do renderowania. W poniższym przykładzie pokazano to:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Przykład zawiera następujące o nazwie symbole zastępcze w *LayoutTemplate* elementu:

- *headerPlaceholder* — w czasie wykonywania, to zastępuje zawartość *elementu HeaderTemplate* elementu.
- *sideBarPlaceholder* — w czasie wykonywania, to zastępuje zawartość *SideBarTemplate* elementu.
- *wizardStepPlaceHolder* — w czasie wykonywania, to zastępuje zawartość *WizardStepTemplate* elementu.
- *navigationPlaceholder* — w czasie wykonywania, to zastępuje wszystkie szablony nawigacji, które zostały określone.

Kod znaczników, w tym przykładzie, który używa symbole zastępcze Renderuje poniższy kod HTML (bez zawartości, w rzeczywistości zdefiniowane w szablonach):

[!code-html[Main](overview/samples/sample90.html)]

Jest tylko kod HTML, który jest teraz nie użytkownika *span* elementu. (Przewidujemy, że w przyszłych wersjach nawet *span* element nie będzie renderowany.) To teraz umożliwia pełną kontrolę nad niemal wszystkie zawartością, który jest generowany przez *kreatora* formantu.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC została wprowadzona jako dodatek platforma ASP.NET 3.5 z dodatkiem SP1 w marca 2009 roku. Program Visual Studio 2010 obejmuje ASP.NET MVC 2, który zawiera nowe funkcje i możliwości.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Obszary pomocy technicznej

Obszary umożliwiają grupy kontrolery i widoki sekcje dużych aplikacji w izolacji względnej z innej sekcji. Każdy obszar można zaimplementować jako oddzielny projekt platformy ASP.NET MVC, który można odwoływać się do aplikacji głównej. Pomaga w zarządzaniu złożonością podczas tworzenia dużych aplikacji i ułatwia tworzenie zespołów wielu współdziałają ze sobą w pojedynczej aplikacji.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Obsługa sprawdzania poprawności atrybutów adnotacji danych

*DataAnnotations* atrybutów umożliwiają dołączanie logikę weryfikacji modelu przy użyciu atrybutów metadanych. *DataAnnotations* atrybuty zostały wprowadzone w danych dynamicznych platformy ASP.NET w programie ASP.NET 3.5 z dodatkiem SP1. Te atrybuty są integrowane domyślnego integratora modelu i umożliwiają oparte na metadanych do sprawdzania poprawności danych wejściowych użytkownika.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Pomocników szablonu

Pomocników szablonu umożliwiają automatyczne kojarzenie Edytuj i Wyświetl szablony przy użyciu typów danych. Na przykład pomocnika szablonu umożliwia określenie automatycznie renderowania elementu selektora daty interfejsu użytkownika dla *System.DateTime* wartość. Efekt jest podobny do szablonów pola w danych dynamicznych platformy ASP.NET.

*Html.EditorFor* i *Html.DisplayFor* metody pomocnicze ma wbudowaną obsługę dla renderowania danych standardowych typów obiektów również jako złożonych z wielu właściwości. Można zastosować atrybutów adnotacji danych, takich jak one również dostosować renderowania *DisplayName* i *ScaffoldColumn* do *ViewModel* obiektu.

Często chcesz dostosować dane wyjściowe z jeszcze bardziej wątków interfejsu użytkownika i mieć pełną kontrolę nad co to jest generowany. *Html.EditorFor* i *Html.DisplayFor* metody pomocnika obsługują, to przy użyciu mechanizmu tworzenia szablonów, które pozwala zdefiniować zewnętrznych szablonów, które można zastąpić i kontroli danych wyjściowych renderowane. Szablony może być renderowane indywidualnie dla klasy.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Danych dynamicznych

Danych dynamicznych została wprowadzona w wersji programu .NET Framework 3.5 z dodatkiem SP1 w połowie 2008. Ta funkcja zapewnia wiele rozszerzeń do tworzenia aplikacji opartych na danych, takie jak następujące:

- RAD środowisko szybkiego tworzenia witryny sieci Web opartej na danych.
- Automatyczne sprawdzanie poprawności opartego na ograniczeń zdefiniowanych w modelu danych.
- Możliwość łatwo zmienić kod znaczników, który zostanie wygenerowany dla pola w *GridView* i *widoku DetailsView* formantów przy użyciu szablonów pól, które są częścią projektu danych dynamicznych.

> [!NOTE]
> Należy pamiętać, aby uzyskać więcej informacji, zobacz [dokumentacji danych dynamicznych](https://msdn.microsoft.com/library/cc488545.aspx) w bibliotece MSDN.


Dla programu ASP.NET 4 danych dynamicznych została rozszerzona umożliwiają deweloperom nawet więcej możliwości szybkiego tworzenia witryn sieci Web opartych na danych.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Włączanie obsługi danych dynamicznych dla istniejących projektów

Funkcje dane dynamiczne, które zostały wydane w .NET Framework 3.5 SP1 wprowadzone nowe funkcje, takie jak następujące:

- Szablony pola — te zapewniają szablony na podstawie typu danych dla formantów powiązanych z danymi. Pole Szablony zapewniają prostszy sposób, aby dostosować wygląd formantów danych niż przy użyciu szablonu pól dla każdego pola.
- Sprawdzanie poprawności — dane dynamiczne umożliwia Użyj atrybuty dla klas danych w celu określenia sprawdzania poprawności dla typowych scenariuszy, takich jak wymagane pola, sprawdzanie zakresu, sprawdzanie typu wzorzec dopasowany za pomocą wyrażeń regularnych, a niestandardowego sprawdzania poprawności. Sprawdzanie poprawności jest wymuszana przez formanty danych.

Jednak te funkcje ma następujące wymagania:

- Warstwa dostępu do danych mają być oparte na Entity Framework lub LINQ do SQL.
- Formanty w przypadku tych funkcji były obsługiwane źródła danych tylko *obiektu EntityDataSource* lub *LinqDataSource* kontrolki.
- Projekt sieci Web, które zostały utworzone przy użyciu danych dynamicznych lub szablony obiektów danych dynamicznych Aby przypisać wszystkie pliki, które są wymagane do obsługi funkcji wymagane funkcje.

Główne celem obsługę danych dynamicznych w ASP.NET 4 jest umożliwienie nowych funkcji danych dynamicznych dla dowolnej aplikacji ASP.NET. W poniższym przykładzie pokazano kod znaczników dla kontrolki, których można korzystać z funkcji dynamicznego danych w istniejącej strony.

[!code-aspx[Main](overview/samples/sample91.aspx)]

W kodzie strony muszą być dodane następujący kod, aby włączyć obsługę danych dynamicznych dla tych kontrolek:

[!code-csharp[Main](overview/samples/sample92.cs)]

Gdy *GridView* formant jest w trybie edycji, danych dynamicznych automatycznie sprawdza, czy dane wprowadzone w nieprawidłowym formacie. Jeśli nie jest dostępne, zostanie wyświetlony komunikat o błędzie.

Ta funkcja zapewnia również inne korzyści, takich jak możliwość określenia domyślnej wartości tryb wstawiania. Bez danych dynamicznych do zaimplementowania wartości domyślnej dla pola, należy dołączyć do zdarzenia, zlokalizować formantu (przy użyciu *metody FindControl*) i ustaw jej wartość. W przypadku programu ASP.NET 4 *EnableDynamicData* wywołania obsługuje drugi parametr, który pozwala przekazać wartości domyślne dla dowolnego pola w obiekcie, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Składnia deklaratywne kontrolki Menedżera danych dynamicznych

*Obiektu DynamicDataManager* kontrolki została rozszerzona, aby można go skonfigurować deklaratywnie, tak jak w przypadku większości kontroli w ASP.NET, a nie tylko w kodzie. Kod znaczników dla *obiektu DynamicDataManager* kontroli wygląda następująco:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Ten kod znaczników włącza zachowanie danych dynamicznych dla formantu GridView1, do którego odwołuje się do *DataControls* sekcji *obiektu DynamicDataManager* formantu.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Szablony jednostki

Szablony jednostki oferują nowy sposób, aby dostosować układ danych bez konieczności tworzenia niestandardowej strony. Strona szablonach *FormView* formantu (zamiast *widoku DetailsView* kontroli stosowane w szablony stron w starszych wersjach danych dynamicznych) i *Dynamiccontrol* formant do renderowania szablonów jednostki. Zapewnia większą kontrolę nad kod znaczników, który jest renderowany przez dane dynamiczne.

Na poniższej liście przedstawiono nowy układ katalogu projektu zawiera szablony jednostki:

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate` Katalog zawiera szablony wyświetlania obiekty modelu danych. Domyślnie obiekty mają być renderowane przy użyciu `Default.ascx` szablonu, który zawiera kod znaczników, który wygląda podobnie jak znaczników utworzone przez *widoku DetailsView* formant używany przez danych dynamicznych ASP.NET 3.5 z dodatkiem SP1. W poniższym przykładzie pokazano kod znaczników dla `Default.ascx` sterowania:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Aby zmienić wygląd i działanie dla całej lokacji można edytować domyślnych szablonów. Brak szablonów do wyświetlania, edytowania i operacje wstawiania. Nowe szablony do dodania na podstawie nazwy obiektu danych, aby zmienić wygląd i działanie tylko jeden typ obiektu. Na przykład można dodać następującego szablonu:

[!code-console[Main](overview/samples/sample97.cmd)]

Szablon może zawierać następujący kod:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Nowe szablony jednostki są wyświetlane na stronie przy użyciu nowej *Dynamiccontrol* formantu. W czasie wykonywania ten formant jest zastępowany zawartość szablonu jednostki. Poniżej przedstawiono znaczników *FormView* kontroli w `Detail.aspx` strony szablonu, który używa szablonu jednostki. Powiadomienie *Dynamiccontrol* elementu w znaczniku.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nowe szablony pola adresy URL i adresów E-mail

ASP.NET 4 wprowadza dwa nowe szablony wbudowane pole `EmailAddress.ascx` i `Url.ascx`. Te szablony służą do pola, które są oznaczone jako *EmailAddress* lub *adres Url* z *DataType* atrybutu. Aby uzyskać *EmailAddress* obiekty, w polu jest wyświetlana jako hiperłącze, która jest tworzona przy użyciu *mailto:* protokołu. Gdy użytkownik kliknie łącze, otwiera klienta poczty e-mail użytkownika i tworzy komunikat szkielet. Obiekty typu *adres Url* są wyświetlane jako zwykłej hiperłącza.

W poniższym przykładzie pokazano, jak będzie oznaczony pól.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Tworzenie łączy z formantem DynamicHyperLink

Dane dynamiczne korzysta z nowej funkcji routingu, który został dodany w programie .NET Framework 3.5 SP1 w celu kontrolowania adresów URL, które użytkownicy widzą przy uzyskiwaniu dostępu do witryny sieci Web. Nowy *DynamicHyperLink* kontroli ułatwia tworzenie łączy do stron w witrynie danych dynamicznych. Poniższy przykład przedstawia użycie *DynamicHyperLink* sterowania:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Ten kod znaczników tworzy łącze, które wskazuje na stronie listy `Products` tabeli oparte na trasach zdefiniowanych w `Global.asax` pliku. Formant automatycznie używa nazwy tabeli domyślnego, na podstawie strony danych dynamicznych.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Obsługa dziedziczenia w modelu danych

Zarówno programu Entity Framework, jak i LINQ do SQL obsługuje dziedziczenia w ich modeli danych. Na przykład może być bazy danych, która ma `InsurancePolicy` tabeli. Może ona także zawierać `CarPolicy` i `HousePolicy` tabel, które mają te same pola jako `InsurancePolicy` , a następnie dodać więcej pól. Aby zrozumieć dziedziczonych obiektów w modelu danych i obsługę szkieletów dziedziczone tabel danych dynamicznych została zmodyfikowana.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Obsługa relacje wiele do wielu (tylko Entity Framework)

Entity Framework ma szeroką obsługę relacje wiele do wielu między tabelami, w których jest implementowany przez udostępnianie relacji w postaci zbioru na *jednostki* obiektu. Nowy `ManyToMany.ascx` i `ManyToMany_Edit.ascx` Szablony pól zostały dodane do obsługi do wyświetlania i edytowania danych, który uczestniczy w relacji wiele do wielu.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nowe atrybuty do wyświetlenia sterowania i pomocy technicznej wyliczenia

*DisplayAttribute* został dodany do zapewniają dodatkową kontrolę nad wyświetlania pól. *DisplayName* atrybutu we wcześniejszych wersjach można zmienić nazwę, która jest używana jako podpis dla pola danych dynamicznych. Nowy *DisplayAttribute* klasy pozwala określić dodatkowe opcje dotyczące wyświetlania pola, takie jak kolejność, w polu jest wyświetlana i określa, czy pole będzie służyć jako filtr. Ten atrybut zapewnia również niezależne kontrolę nad nazwę używaną dla etykiet w *GridView* kontrolować nazwę używaną w *widoku DetailsView* kontrolować tekst pomocy dla pola i znak wodny używane do pole (Jeśli pole akceptuje pola tekstowego).

*EnumDataTypeAttribute* klasy został dodany do umożliwiają mapowanie pól do wyliczenia. Po zastosowaniu tego atrybutu do pola, można określić typem wyliczenia. Dane dynamiczne używa nowego `Enumeration.ascx` pola szablonu w celu utworzenia interfejsu użytkownika do wyświetlania i edytowania wartości wyliczenia. Szablon mapuje wartości z bazy danych do nazwy w wyliczeniu.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Rozszerzona obsługa filtrów

Dynamiczne 1.0 dane dostarczone z wbudowanych filtrów logiczna kolumny i kolumny klucza obcego. Filtry nie zezwala na określanie, czy zostały one wyświetlane lub w jakiej kolejności były wyświetlane. Nowy *DisplayAttribute* atrybutu adresy oba te problemy, umożliwiając kontrolę nad Określa, czy kolumna jest wyświetlana jako filtr i w kolejność będą wyświetlane.

Dodatkowe ulepszenie jest, czy filtrowanie został[zapisywane do używania nowych](#0.2__QueryExtender "_QueryExtender") funkcji formularzy sieci Web. Dzięki temu można tworzyć filtry bez konieczności znajomości formantu źródła danych, które będą używane filtry z. Wraz z tymi rozszerzeniami filtry również zostanie spełniony w formantach szablonu, co pozwala na dodawanie nowych. Na koniec *DisplayAttribute* klasa wymienionych poniżej umożliwia domyślny filtr do zastąpienia, w tym samym jak robi *UIHint* umożliwia domyślny szablon pola kolumny do zastąpienia.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web Development ulepszenia

Projektowanie witryn sieci Web w programie Visual Studio 2010 została rozszerzona większa zgodność CSS, zwiększenie produktywności za pośrednictwem wstawki kodu znaczników HTML i ASP.NET i nowe dynamiczne IntelliSense JavaScript.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Ulepszone CSS zgodności

Aby zwiększyć zgodność ze standardami CSS 2.1 został zaktualizowany projektanta Visual Web Developer w Visual Studio 2010. Projektant lepiej zachowuje spójności źródła HTML i jest bardziej niezawodna niż w poprzednich wersjach programu Visual Studio. Pod maską architektury także udoskonalono tego zostanie umożliwiają lepsze przyszłe rozszerzenia renderowania, układu i użytkowanie.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML i JavaScript wstawki kodu programu

W edytorze HTML IntelliSense auto kończy nazwy tagów. Funkcja wstawki kodu programu IntelliSense auto kończy całych tagów i inne. W programie Visual Studio 2010 wstawki kodu programu IntelliSense są obsługiwane dla języka JavaScript, obok C# i Visual Basic, które są obsługiwane we wcześniejszych wersjach programu Visual Studio.

Program Visual Studio 2010 zawiera ponad 200 fragmentów, ułatwiające automatycznego zakończenia wspólnej ASP.NET i HTML tagów, łącznie z wymaganych atrybutów (takie jak runat = "server") i atrybuty wspólne, które są specyficzne dla znacznika (takich jak *identyfikator*,  *Identyfikator DataSourceID*, *ControlToValidate*, i *tekst*).

Można pobrać dodatkowe fragmenty kodu, lub można pisać własne fragmentów, które hermetyzują bloków znaczników używanego do wykonywania typowych zadań lub Twojego zespołu.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Ulepszenia IntelliSense dla JavaScript

W języku Visual 2010 IntelliSense dla JavaScript został zaprojektowany do zapewnienia jeszcze bardziej rozbudowane środowisko edycji. IntelliSense rozpoznaje teraz obiektów, które zostały dynamicznie wygenerowane za pomocą metod takich jak *registerNamespace* i podobne techniki stosowane przez innych platform, JavaScript. Wydajność została zwiększona do analizowania dużych bibliotek skryptu i do wyświetlenia IntelliSense z opóźnieniem przetwarzania niewielkiego lub żadnego. Zgodność znacząco zwiększono do obsługi niemal wszystkich innych firm bibliotek i do obsługi różnych stylów kodowania. Komentarze dokumentacji są teraz analizowana jako typ i natychmiast są wykorzystywane przez funkcję IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Wdrażanie aplikacji sieci Web z programu Visual Studio 2010

Deweloperów platformy ASP.NET wdrażania aplikacji sieci Web, często znajduje się pracującym następujące kwestie:

- Wdrażanie udostępnionego lokacji hostingu wymaga technologii, takich jak FTP, który może działać powoli. Ponadto należy ręcznie wykonać zadania, takie jak uruchamianie skryptów SQL do konfigurowania bazy danych i zmieniać ustawienia usług IIS, takie jak konfigurowanie folder katalogu wirtualnego jako aplikacji.
- W środowisku firmowym, oprócz wdrażania plików aplikacji sieci Web administratorzy często zmodyfikować ustawienia usług IIS i plików konfiguracji ASP.NET. Administratorzy baz danych, należy uruchomić szereg skrypty SQL, można pobrać z bazy danych aplikacji. Takie urządzenia są pracy znacznym, często zająć godzin, a dokładnie dokumentowane.

Program Visual Studio 2010 zawiera technologie, które rozwiązania tych problemów i umożliwiające łatwe wdrażanie aplikacji sieci Web. Jedną z tych technologii jest narzędzia wdrażania Web usług IIS (MsDeploy.exe).

Funkcje wdrażania sieci Web w programie Visual Studio 2010 następujących głównych obszarów:

- Tworzenie pakietów w sieci Web
- Transformacji pliku Web.config
- Wdrożenie bazy danych
- Jednym kliknięciem publikowania aplikacji sieci Web

Poniższe sekcje zawierają szczegółowe informacje o tych funkcjach.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Tworzenie pakietów w sieci Web

Program Visual Studio 2010 korzysta z narzędzia MSDeploy utworzyć plik skompresowany (.zip) dla aplikacji, które są określone jako *pakietu sieci Web*. Pakiet zawierający metadane dotyczące aplikacji oraz zawartość następujących:

- Ustawienia usług IIS, które zawiera ustawienia puli aplikacji, ustawienia strony błędów i tak dalej.
- Rzeczywista zawartość sieci Web, w tym stron sieci Web, kontrolek użytkownika, zawartość statyczną (obrazów i plików HTML) i tak dalej.
- Schematy bazy danych programu SQL Server i danych.
- Certyfikaty zabezpieczeń, składniki do zainstalowania w pamięci podręcznej GAC, ustawienia rejestru i tak dalej.

Pakiet sieci Web można kopiować do dowolnego serwera i następnie instalowane ręcznie przy użyciu Menedżera usług IIS. Alternatywnie dla automatycznego wdrażania pakietu można zainstalować przy użyciu poleceń wiersza polecenia lub przy użyciu wdrażania interfejsów API.

Program Visual Studio 2010 udostępnia wbudowane zadania programu MSBuild i obiekty docelowe do tworzenia pakietów w sieci Web. Aby uzyskać więcej informacji, zobacz [omówienie wdrażania projektu aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [10 + 20 powodów, dlaczego należy utworzyć pakiet sieci Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config Transformation

Wdrożenia aplikacji sieci Web programu Visual Studio 2010 wprowadza [transformacji dokumentów XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), która to funkcja, która pozwala na przekształcanie `Web.config` plik z ustawienia środowiska deweloperskiego do ustawienia w środowisku produkcyjnym. Ustawienia przekształcania są określone w pliki transformacji o nazwie `web.debug.config`, `web.release.config`i tak dalej. (Nazwy tych plików są zgodne z konfiguracji programu MSBuild.) Plik przekształcenia obejmuje tylko zmiany należy wprowadzić wdrożony `Web.config` pliku. Określasz zmiany przy użyciu prostego składni.

W poniższym przykładzie przedstawiono część `web.release.config` pliku, który może zostać utworzony dla wdrożenia konfiguracji wydania. Słowo kluczowe Zamień w przykładzie określa, że podczas wdrażania *connectionString* w węźle `Web.config` pliku zostaną zastąpione wartości, które są wymienione w tym przykładzie.

[!code-xml[Main](overview/samples/sample102.xml)]

Aby uzyskać więcej informacji, zobacz [składni transformacji pliku Web.config dla wdrożenia projektu aplikacji sieci Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) w witrynie MSDN <a id="0.2_a"> </a> witryny sieci Web i[Web Deployment: transformacji pliku Web.Config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)na blogu Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Wdrożenie bazy danych

Pakiet wdrożeniowy programu Visual Studio 2010 mogą zawierać zależności od bazy danych programu SQL Server. W ramach definicji pakietu należy podać parametry połączenia dla źródłowej bazy danych. Podczas tworzenia pakietu sieci Web programu Visual Studio 2010 tworzy skrypty SQL dla schematu bazy danych i opcjonalnie danych, a następnie dodaje je do pakietu. Może również udostępniać niestandardowe skrypty SQL i Określ sekwencję, w którym powinno być ono uruchomione na serwerze. W czasie wdrażania Podaj parametry połączenia, które jest odpowiednie dla serwera docelowego; proces wdrażania następnie używa tego ciągu połączenia na uruchamianie skryptów, które tworzą schemat bazy danych i dodać dane.

Ponadto, za pomocą jednego kliknięcia publikowania, można skonfigurować wdrożenie w celu publikowania bazy danych bezpośrednio po opublikowaniu aplikacji z lokacją zdalną hostingu udostępnionego. Aby uzyskać więcej informacji, zobacz [porady: Wdróż projekt bazy danych z sieci Web aplikacji](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [wdrożenie bazy danych z VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>One-Click Publish dla aplikacji sieci Web

Visual Studio 2010 umożliwia także publikowanie aplikacji sieci Web na serwerze zdalnym za pomocą usługi zdalnego zarządzania usługami IIS. Można utworzyć profil publikowania, konta usług hosta lub do testowania serwerów lub serwerów na potrzeby przemieszczania. Każdy profil można bezpiecznie zapisać odpowiednie poświadczenia. Następnie można wdrożyć dla każdego obiektu docelowego pasek narzędzi publikacja serwerów jednym kliknięciem, przy użyciu sieci Web jednym kliknięciem. Z programu Visual Studio 2010 można również opublikować przy użyciu wiersza polecenia programu MSBuild. Dzięki temu można skonfigurować środowisko kompilacji team uwzględnienie publikowanie w modelu ciągłej integracji.

Aby uzyskać więcej informacji, zobacz [porady: Wdrażanie aplikacji publikowania projektu sieci Web za pomocą jednego kliknięcia i Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) w witrynie MSDN w sieci Web i [sieci Web publikowanie kliknij 1 z VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) na blogu Vishal Joshi. Aby wyświetlić wideo prezentacji na temat wdrażania aplikacji sieci Web w programie Visual Studio 2010, zobacz [VS 2010 dotyczącym wersji zapoznawczych platformy sieci Web Developer](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) na blogu Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Zasoby

Następujące witryny sieci Web zawierają dodatkowe informacje dotyczące programu ASP.NET 4 i programu Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — oficjalnej dokumentacji dla programu ASP.NET 4 w witrynie MSDN.
- [https://www.asp.net/](https://www.asp.net/) — ASP.NET witryny sieci Web przez zespół.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) i [dynamiczna Mapa zawartości platformy ASP.NET danych](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — Online zasobów w witrynie zespołu programu ASP.NET w oficjalnej dokumentacji dla danych dynamicznych ASP.NET.
- [https://www.asp.net/ajax/](../../ajax/index.md) — Główny zasobu sieci Web do tworzenia aplikacji ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Web Developer Team blog, który zawiera informacje o funkcji w programie Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — główny zasobu sieci Web dla wersji preview programu ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może znacznie zmienione przed końcowej wersji oprogramowania opisanych tutaj.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok elementu firmy Microsoft Corporation na zagadnień omówionych w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft i firma Microsoft nie może zagwarantować dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH LUB USTAWOWYCH, ODNOŚNIE DO INFORMACJI W TYM DOKUMENCIE.

Zgodne z wszystkich stosownych praw autorskich jest odpowiedzialny za użytkownika. Bez ograniczenia praw autorskich, żadnej części tego dokumentu może być odtworzyć, przechowywane w wprowadzane do systemu pobierania lub przekazywać w jakimkolwiek formularzu za pomocą jakichkolwiek metod (elektronicznych, mechanicznych, postaci kopii, nagrań lub innych) w jakimkolwiek celu, bez pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej będących przedmiotem tego dokumentu. Z wyjątkiem wyraźnie określonych w umowach licencyjnych firmy Microsoft, otrzymanie tego dokumentu nie oznacza udzielenia licencji na te patenty, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej.

Jeśli nie podano inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia opisywane w przykładach są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, poczty e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2009 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami towarowymi ich prawnych właścicieli.
