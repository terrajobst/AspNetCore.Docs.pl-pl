---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Opis usługi sieci Web ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Usługi sieci Web są integralną częścią programu .NET framework, które stanowią rozwiązanie i platform wymiany danych między systemów rozproszonych. Mimo że w sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-web-services"></a>Opis usługi sieci Web ASP.NET AJAX
====================
przez [Scott kategorii](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Usługi sieci Web są integralną częścią programu .NET framework, które stanowią rozwiązanie i platform wymiany danych między systemów rozproszonych. Mimo że usługi sieci Web są zwykle używane do różnych systemów operacyjnych, modeli obiektów i języków programowania, aby wysyłać i odbierać dane, ich może również służyć do dynamicznie iniekcję danych do strony ASP.NET AJAX lub wysyłanie danych ze strony systemu zaplecza. Wszystkie te mogą być przeprowadzane bez konieczności ogłaszanie operacji.


## <a name="calling-web-services-with-aspnet-ajax"></a>Wywołanie usługi sieci Web ASP.NET AJAX

DaN Wahlin

Usługi sieci Web są integralną częścią programu .NET framework, które stanowią rozwiązanie i platform wymiany danych między systemów rozproszonych. Mimo że usługi sieci Web są zwykle używane do różnych systemów operacyjnych, modeli obiektów i języków programowania, aby wysyłać i odbierać dane, ich może również służyć do dynamicznie iniekcję danych do strony ASP.NET AJAX lub wysyłanie danych ze strony systemu zaplecza. Wszystkie te mogą być przeprowadzane bez konieczności ogłaszanie operacji.

Kontrolki ASP.NET AJAX w elemencie UpdatePanel zapewnia prosty sposób AJAX jednocześnie włączyć dowolnej strony ASP.NET, mogą być potrzebne dynamicznie dostępu do danych na serwerze bez użycia elementu UpdatePanel. W tym artykule pojawi się, jak to zrobić, tworząc i korzystanie z usług sieci Web w technologii ASP.NET AJAX stron.

Ten artykuł koncentruje się na funkcje dostępne w rozszerzenia AJAX platformy ASP.NET core, a także formantu w zestawie narzędzi programu ASP.NET AJAX o nazwie AutoCompleteExtender włączona usługa sieci Web. Tematy obejmują definiowanie usług sieci Web z włączoną obsługą technologii AJAX, tworzenia serwerów proxy klienta i wywoływanie usługi sieci Web z użyciem języka JavaScript. Możesz również sprawdzić, jak bezpośrednio do metody stron ASP.NET, w którym można było wykonywać wywołania usługi sieci Web.

## <a name="web-services-configuration"></a>Konfiguracja usług sieci Web

Po utworzeniu nowego projektu witryny sieci Web z programu Visual Studio 2008 plik web.config zawiera szereg nowych dodatków, które mogą być nieznane użytkownicy poprzednich wersji programu Visual Studio. Prefiks "asp" niektóre z tych zmian mapowane na formanty ASP.NET AJAX, dzięki mogą być używane na stronach, podczas gdy inne zdefiniuj wymagane HttpHandlers i HttpModules. Wyświetlanie listy 1 przedstawiono modyfikacje `<httpHandlers>` elementu w pliku web.config, który może mieć wpływ wywołania usługi sieci Web. Używane do przetwarzania wywołania .asmx HttpHandler domyślne jest usunięte i zastąpione klasą ScriptHandlerFactory znajduje się w zestawie System.Web.Extensions.dll. System.Web.Extensions.dll znajdują się podstawowe funkcje używane przez program ASP.NET AJAX.

**Wyświetlanie listy 1. Konfiguracja obsługi usługi sieci Web AJAX ASP.NET**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

To zastąpienie HttpHandler następuje w celu zapewnienia obsługi wywołań JavaScript Object Notation (JSON) ma zostać wykonane z ASP.NET AJAX stron do usług sieci Web platformy .NET przy użyciu serwera proxy usługi sieci Web JavaScript. ASP.NET AJAX wysyła komunikaty JSON do usług sieci Web, w przeciwieństwie do standardowych wywołań protokołu dynamicznej konfiguracji hosta (SOAP, Simple Object Access Protocol), zazwyczaj związanych z usługami sieci Web. Powoduje to mniejsze żądania i ogólną wiadomości odpowiedzi. Pozwala także wydajniejsze po stronie klienta przetwarzanie danych, ponieważ biblioteka ASP.NET AJAX JavaScript jest zoptymalizowana pod kątem pracy z obiektami JSON. Wyświetlanie listy 2 i 3 wyświetlanie listy przedstawiają przykłady komunikatów żądań i odpowiedzi usługi sieci Web do formatu JSON. Komunikat żądania zostało przedstawione na wyświetlanie 2 przekazuje kraju parametru o wartości "Belgia" podczas przekazuje komunikat odpowiedzi w 3 wyświetlania tablicę obiektów klientów i ich właściwości.

**Wyświetlanie listy 2. Komunikat żądania usługi sieci Web serializować do notacji JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] Nazwa operacji jest zdefiniowany jako część adresu URL usługi sieci web; Ponadto komunikaty żądania nie są zawsze przesłane za pomocą formatu JSON. Usługi sieci Web może korzystać z atrybutu ScriptMethod z parametrem UseHttpGet wartość true, co powoduje, że parametry do przekazania za pomocą parametrów ciągu zapytania.*


**Wyświetlanie listy 3. Komunikat odpowiedzi usługi sieci Web serializować do notacji JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

W następnej sekcji zostanie wyświetlone sposobu tworzenia usług sieci Web do obsługi komunikatów żądania JSON i odpowiadać zarówno proste i złożone typy.

## <a name="creating-ajax-enabled-web-services"></a>Tworzenie usług sieci Web obsługujących technologię AJAX

ASP.NET AJAX framework oferuje kilka różnych metod do wywołania usługi sieci Web. Można użyć formantu AutoCompleteExtender (dostępne w zestawie narzędzi programu ASP.NET AJAX) lub JavaScript. Jednak przed wywołaniem usługi należy włączyć AJAX go tak, że może ona zostać wywołana przez kod skryptu klienta.

Czy jesteś nowym użytkownikiem usługi sieci Web programu ASP.NET, znajdują się ona bezpośrednie utworzyć i Włącz AJAX usług. .NET framework jest obsługiwane tworzenie usług sieci Web ASP.NET początkowego wydania 2002 i rozszerzenia AJAX ASP.NET zapewniają dodatkowe funkcje AJAX bazując na domyślny zestaw funkcji programu .NET framework. Visual Studio .NET 2008 w wersji Beta 2 ma wbudowaną obsługę tworzenia plików usługi sieci Web asmx i automatycznie pochodną skojarzonego kodu obok klasy klasy System.Web.Services.WebService. Podczas dodawania metod w klasie należy zastosować atrybut WebMethod je do wywołania przez konsumentów usługi sieci Web.

Wyświetlanie listy 4 przedstawiono przykład zastosowania atrybutu WebMethod metodę o nazwie GetCustomersByCountry().

**Wyświetlanie listy 4. Usługi sieci Web przy użyciu atrybutu WebMethod**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Metoda GetCustomersByCountry() przyjmuje parametr kraju i zwraca tablicę obiektów klienta. Wartość kraju przekazany do metody jest przekazywany do klasy warstwy biznesowej, który z kolei wywołania klasy warstwy danych do pobierania danych z bazy danych, wypełnienie właściwości obiektu klienta z danymi i zwracać tablicy.

## <a name="using-the-scriptservice-attribute"></a>Za pomocą atrybutu ScriptService

Podczas dodawania WebMethod atrybutu zapewnia metody GetCustomersByCountry() ma zostać wywołana przez klientów wysyłających standardowych komunikatów SOAP z usługą sieci Web, jego nie zezwala na wywołania JSON z aplikacji ASP.NET AJAX poza pole. Aby umożliwić wywołania JSON dokonywane trzeba stosować rozszerzenie ASP.NET AJAX `ScriptService` atrybutu klasy usługi sieci Web. To umożliwia usługi sieci Web do wysyłania wiadomości odpowiedzi, sformatowany przy użyciu formatu JSON oraz umożliwia skryptu po stronie klienta do wywołania usługi przez wysłanie wiadomości w formacie JSON.

Wyświetlanie listy 5 przedstawiono przykład zastosowania atrybutu ScriptService do klasy usługi sieci Web o nazwie CustomersService.

**Wyświetlanie listy 5. Za pomocą atrybutu ScriptService AJAX-Włącz usługi sieci Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

Atrybut ScriptService działa jako znacznik, który wskazuje, że może ona zostać wywołana z kodu skryptów AJAX. Faktycznie nie obsługuje żadnego JSON serializacji lub deserializacji powtarzających się zadań w tle. ScriptHandlerFactory (skonfigurowany w pliku web.config) i innych klas pokrewnych nie zbiorczego przetwarzania JSON.

## <a name="using-the-scriptmethod-attribute"></a>Za pomocą atrybutu ScriptMethod

Atrybut ScriptService jest tylko atrybut ASP.NET AJAX, który musi być zdefiniowane w usłudze sieci Web .NET w celu użycia go do użycia przez stron ASP.NET AJAX. Jednak inny atrybut o nazwie ScriptMethod można również będą stosowane bezpośrednio do metody sieci Web w usłudze. ScriptMethod definiuje trzy właściwości w tym `UseHttpGet`, `ResponseFormat` i `XmlSerializeString`. Zmiana wartości właściwości mogą być przydatne w sytuacjach, gdy typ żądania akceptowane przez metody sieci Web musi zostać zmienione GET, gdy metoda sieci Web musi zwracać nieprzetworzone dane XML w formie `XmlDocument` lub `XmlElement` obiekt lub gdy dane są zwracane z  Usługa zawsze powinny być serializowane w formacie XML zamiast JSON.

UseHttpGet, właściwość może być używane, gdy metoda sieci Web powinna obsługiwać pobrać żądań w przeciwieństwie do żądania POST. Żądania wysyłane z parametrów wejściowych metody sieci Web przy użyciu adresu URL przekształcić na parametry QueryString. UseHttpGet właściwość domyślną jest false i tylko powinien mieć ustawioną `true` gdy operacje są znane jako bezpieczne i kiedy dane poufne nie zostanie przekazany do usługi sieci Web. Wyświetlanie listy 6 przedstawiono przykład za pomocą atrybutu ScriptMethod razem z właściwością UseHttpGet.

**Wyświetlanie listy 6. Za pomocą atrybutu ScriptMethod razem z właściwością UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Przykład nagłówków wysyłany, gdy wywoływana jest metoda sieci Web HttpGetEcho przedstawiono na wyświetlanie 6 są wyświetlane dalej:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Oprócz umożliwienia metody sieci Web akceptował żądania HTTP GET, atrybut ScriptMethod można także jeśli XML odpowiedzi muszą zostać zwrócone z usługi zamiast JSON. Na przykład usługi sieci Web może pobrać źródła danych RSS z lokacją zdalną i przywrócić go jako obiekt XmlDocument lub atrybutu XmlElement. Przetwarzanie pliku XML danych następnie może wystąpić na kliencie.

Wyświetlanie listy 7 przedstawiono przykład za pomocą właściwości Format odpowiedzi, aby określić, że dane XML ma zostać zwrócony z metody sieci Web.

**Wyświetlanie listy 7. Za pomocą atrybutu ScriptMethod razem z właściwością format odpowiedzi.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

Właściwość Format odpowiedzi może również wraz z właściwością XmlSerializeString. Właściwość XmlSerializeString ma wartość domyślną, FALSE, która oznacza wszystkie zwracać typów, z wyjątkiem ciągów zwrócony z metody sieci Web są zserializowanym w formacie XML podczas `ResponseFormat` właściwość jest ustawiona na `ResponseFormat.Xml`. Gdy `XmlSerializeString` ma ustawioną wartość `true`, wszystkie typy zwrócony z metody sieci Web są zserializowanym w formacie XML, włącznie z typami ciągu. Jeśli format odpowiedzi właściwość ma wartość `ResponseFormat.Json` XmlSerializeString właściwość jest ignorowana.

Wyświetlanie listy 8 przedstawiono przykład za pomocą właściwości XmlSerializeString, aby wymusić ciągi do zserializowanym w formacie XML.

**Wyświetlanie listy 8. Za pomocą atrybutu ScriptMethod razem z właściwością XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Wartość zwracana z wywołania metody sieci Web GetXmlString pokazano na wyświetlanie 8 przedstawiono dalej:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Domyślny format JSON minimalizuje całkowitego rozmiaru komunikatów żądań i odpowiedzi i łatwiej jest używany przez klientów ASP.NET AJAX w sposób różnych przeglądarkach, format odpowiedzi i XmlSerializeString właściwości mogą być używane, kiedy klient aplikacje, takie jak program Internet Explorer 5 lub nowszej oczekiwać, że dane XML mają być zwracane przez metody sieci Web.

## <a name="working-with-complex-types"></a>Praca z typów złożonych

Wyświetlanie listy 5 pokazano przykład przekazujących typu złożonego o nazwie klienta z usługi sieci Web. Klasa odbiorcy definiuje kilka różnych typów prostych wewnętrznie jako właściwości, takie jak imię i nazwisko. Złożone typy używane jako parametr wejściowy lub typ zwracany metody sieci Web z włączoną obsługą technologii AJAX automatycznie są serializowane w formacie JSON przed wysłaniem do po stronie klienta. Jednak zagnieżdżonych typów złożonych (określone wewnętrznie w ramach innego typu) nie są udostępniane do klienta jako w przypadku obiektów autonomicznych domyślnie.

W przypadkach, gdy zagnieżdżony typ złożony używane przez usługę sieci Web należy użyć strony klienta ASP.NET AJAX GenerateScriptType atrybutu można dodać do usługi sieci Web. Na przykład klasa CustomerDetails pokazano na wyświetlanie 9 zawiera właściwości adresu i płci którego ***reprezentują zagnieżdżonych typów złożonych.***

**Wyświetlanie listy 9. Klasa CustomerDetails pokazane zawiera dwa zagnieżdżonych typów złożonych.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Adres i płci obiektów zdefiniowana w klasie CustomerDetails pokazano na wyświetlanie 9 nie będzie automatycznie udostępnione do użycia po stronie klienta za pośrednictwem kodu JavaScript ponieważ są one zagnieżdżone typy (adres jest klasą i płci jest wyliczeniem). W sytuacjach, gdy typem zagnieżdżonym używane w ramach usługi sieci Web muszą być dostępne po stronie klienta, można użyć atrybutu GenerateScriptType wymienionych poniżej (patrz lista 10). Ten atrybut można dodać wiele razy w przypadkach, w którym różne zagnieżdżone typy złożone są zwracane z usługi. Można je stosować bezpośrednio klasa usługi sieci Web lub wcześniej określone metody sieci Web.

**Wyświetlanie listy 10. Za pomocą atrybutu GenerateScriptService do definiowania zagnieżdżone typy, które powinny być dostępne dla klienta.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Stosując `GenerateScriptType` atrybutu typom usługi sieci Web, adres i płci automatycznie będą dostępne do użytku przez kod ASP.NET AJAX JavaScript po stronie klienta. Przykład JavaScript, który jest automatycznie wygenerowane i wysłane do klienta przez dodanie atrybutu GenerateScriptType na usługi sieci Web jest wyświetlany w wyświetlania 11. Zobaczysz jak używać zagnieżdżonych typów złożonych w dalszej części tego artykułu.

**Wyświetlanie listy 11. Zagnieżdżone typy złożone udostępniane na stronie ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Teraz, przedstawiono sposób tworzenia usług sieci Web i udostępnić stron ASP.NET AJAX, Spójrzmy na sposób tworzenia i używania serwery proxy JavaScript, aby dane można pobrać lub wysyłane do usługi sieci Web.

## <a name="creating-javascript-proxies"></a>Tworzenie serwery proxy JavaScript

Zazwyczaj wywołanie standardowe usługi sieci Web (.NET lub innej platformie) obejmuje tworzenie obiekt serwera proxy, który chroni należy od złożoności wysyłanie komunikatów żądań i odpowiedzi protokołu SOAP. Z wywołania usługi sieci Web ASP.NET AJAX serwery proxy JavaScript można tworzyć i umożliwia łatwe wywołują usługi bez obaw o serializację i deserializację formatu JSON wiadomości. Serwery proxy JavaScript mogą być generowane automatycznie za pomocą formantu ASP.NET AJAX ScriptManager.

Tworzenie serwera proxy JavaScript, który można wywołać usługi sieci Web odbywa się przy użyciu właściwości usług elementu ScriptManager. Tej właściwości można zdefiniować co najmniej jednej usługi, które strony ASP.NET AJAX może wywoływać asynchronicznie do wysyłania i odbierania danych bez konieczności operacji odświeżania strony. W celu zdefiniowania usługi za pomocą kodu ASP.NET AJAX `ServiceReference` kontroli i przypisywanie adresu URL usługi sieci Web do formantu `Path` właściwości. Wyświetlanie listy 12 przedstawiono przykład odwołuje się do usługi o nazwie CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Wyświetlanie listy 12. Definiowanie usługi sieci Web używany na stronie ASP.NET AJAX.**

Dodawanie odwołania do CustomersService.asmx za pomocą formantu ScriptManager powoduje, że serwer proxy JavaScript do dynamicznego wygenerowany i odwołuje się strony. Serwer proxy jest osadzony przy użyciu &lt;skryptu&gt; tagu i załadować dynamicznie przez wywołanie pliku CustomersService.asmx i dołączenie /js na końcu. W poniższym przykładzie pokazano, jak serwer proxy JavaScript jest osadzony na stronie, gdy debugowanie jest wyłączone w pliku web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Jeśli chcesz zobaczyć rzeczywisty kod JavaScript proxy generowany jest można wpisz adres URL do żądanej usługi sieci Web .NET w polu adresu programu Internet Explorer i Dołącz /js na końcu.*


Jeśli w pliku web.config, który zostanie osadzony wersji serwera proxy JavaScript do debugowania na stronie jako włączone jest debugowanie wyświetlane dalej:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Proxy JavaScript utworzone przez element ScriptManager można również osadzać bezpośrednio do strony, a nie za pomocą &lt;skryptu&gt; atrybutu src znacznika. Można to zrobić przez ustawienie właściwości formantu servicereference kierowany InlineScript na true (wartość domyślna to false). Może to być przydatne, gdy serwer proxy nie jest udostępniane na wielu stronach i chcesz zmniejszyć liczbę wywołań sieci do serwera. Nie można buforować gdy InlineScript jest ustawiona na true skryptu serwera proxy przez przeglądarkę, dlatego zaleca się domyślną wartość false w przypadku, gdy serwer proxy jest używany przez wiele stron w aplikacji ASP.NET AJAX. Przykład użycia właściwość InlineScript pokazano dalej:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Przy użyciu serwery proxy JavaScript

Po stronie ASP.NET AJAX przy użyciu formantu ScriptManager odwołuje się usługi sieci Web, wywołanie może się z usługą sieci Web i zwrócone dane są obsługiwane przy użyciu funkcji wywołania zwrotnego. Usługi sieci Web jest wywoływana przez odwołanie do jego przestrzeni nazw (jeśli istnieje), nazwę klasy i nazwę metody sieci Web. Parametry przekazane do usługi sieci Web można zdefiniować wraz z funkcji wywołania zwrotnego, która obsługuje zwróconych danych.

Przykład użycia serwera proxy JavaScript do wywołania metody sieci Web o nazwie GetCustomersByCountry() jest wyświetlany w wyświetlania 13. Funkcja GetCustomersByCountry() jest wywoływana, gdy użytkownik końcowy kliknie przycisk na stronie.

**Wyświetlanie listy 13. Wywoływanie usługi sieci Web z serwera proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

To wywołanie odwołuje się do przestrzeni nazw InterfaceTraining, CustomersService klasy i metody sieci Web GetCustomersByCountry zdefiniowane w usłudze. Przekazuje ono wartość kraju uzyskiwane z pola tekstowego, a także funkcję wywołania zwrotnego o nazwie OnWSRequestComplete, która powinna być wywoływana po powrocie asynchroniczne wywołanie usługi sieci Web. OnWSRequestComplete obsługuje tablicę obiektów klienta z usługi zwrócono i konwertuje je na tabelę, która jest wyświetlana na stronie. Dane wyjściowe, generowane przez wywołanie jest pokazany na rysunku 1.


[![Powiązanie danych uzyskanych dzięki asynchroniczne wywołanie AJAX do usługi sieci Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Rysunek 1**: wiązanie danych uzyskanych dzięki asynchroniczne wywołanie AJAX do usługi sieci Web.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image3.png))


Serwery proxy JavaScript można również ustawić jednokierunkowe wywołania usługi sieci Web, w przypadku gdy powinna być wywoływana metoda sieci Web, ale nie powinien oczekiwania na odpowiedź serwera proxy. Na przykład można wywołać usługi sieci Web w taki sposób, aby uruchomić proces, takich jak przepływ pracy, ale nie oczekuje dla wartości zwracanej z usługi. W sytuacjach, gdy wywołanie jednokierunkowe musi się do usługi po prostu można pominąć funkcja wywołania zwrotnego pokazano wyświetlania 13. Ponieważ funkcja wywołania zwrotnego, nie jest zdefiniowany dla usługi sieci Web zwrócić dane nie będzie czekać obiekt serwera proxy.

## <a name="handling-errors"></a>Obsługa błędów

Asynchroniczne wywołania zwrotne do usług sieci Web może wystąpić różnych typów błędów, takie jak sieci jest wciśnięty niedostępności usługi sieci Web lub wyjątek zwracanych. Na szczęście obiektów pośredniczących JavaScript wygenerowanych przez element ScriptManager umożliwiają wielu wywołań zwrotnych do zdefiniowania do obsługi błędów i niepowodzeń oprócz wywołania zwrotnego Powodzenie przedstawiona wcześniej. Funkcja wywołania zwrotnego błędu można zdefiniować natychmiast po funkcji standardowe wywołania zwrotnego w wywołaniu metody sieci Web pokazane na wyświetlanie 14.

**Wyświetlanie listy 14. Definiowanie funkcji wywołania zwrotnego błędu i wyświetlanie błędów.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Wszystkie błędy, gdy jest wywoływana przez usługę sieci Web spowoduje wyzwolenie OnWSRequestFailed() funkcja wywołania zwrotnego wywoływana, która przyjmuje obiekt reprezentujący błąd jako parametr. Obiekt błąd udostępnia kilka różnych funkcji w celu ustalenia przyczyny błędu również, czy połączenie przekroczyło limit czasu. Wyświetlanie listy 14 przedstawiono przykład przy użyciu funkcji inny błąd i na rysunku 2 przedstawiono przykład dane wyjściowe, generowane przez funkcje.


[![Dane wyjściowe generowane przez wywoływanie funkcji błąd ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Rysunek 2**: dane wyjściowe generowane przez wywoływanie funkcji błąd ASP.NET AJAX.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Obsługa danych XML zwrócony z usługi sieci Web

Widać wcześniej jak metoda sieci Web może zwrócić nieprzetworzone dane XML przy użyciu atrybutu ScriptMethod wraz z jego właściwość Format odpowiedzi. Jeśli format odpowiedzi jest ustawiony na ResponseFormat.Xml, dane zwrócone przez usługę sieci Web jest szeregowana jako XML, a nie w formacie JSON. Może to być przydatne, gdy dane XML musi być przekazywane bezpośrednio do klienta do przetwarzania, przy użyciu języka JavaScript i XSLT. W danym momencie Internet Explorer 5 lub nowszej zapewnia najlepsze modelu obiektowego po stronie klienta dla analizowanie i filtrowanie danych XML z powodu jego wbudowaną obsługę programu MSXML.

Nie różni się od innych typów danych pobierania się pobieranie danych XML z usługi sieci Web. Uruchom za pomocą serwera proxy JavaScript do wywołania funkcji odpowiednie i zdefiniować funkcję wywołania zwrotnego. Po wywołaniu zwraca można przetwarzać dane w funkcji wywołania zwrotnego.

Wyświetlanie listy 15 przedstawiono przykład wywołanie metody sieci Web o nazwie GetRssFeed(), która zwraca obiekt XmlElement. GetRssFeed() przyjmuje jeden parametr reprezentujący adres URL dla danych RSS do pobrania.

**Wyświetlanie listy 15. Praca z danymi XML zwracanymi przez usługi sieci Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

W tym przykładzie przekazuje adres URL do źródła danych RSS i przetwarza zwrócone dane XML w funkcji OnWSRequestComplete(). OnWSRequestComplete() najpierw sprawdza, czy przeglądarka jest wiedzieć, czy MSXML parser jest dostępna w programie Internet Explorer. Jeśli tak jest, aby zlokalizować wszystkie używana jest instrukcja XPath &lt;elementu&gt; tagów w kanału informacyjnego RSS. Każdy element jest następnie powtórzyć za pośrednictwem oraz skojarzonych z nimi &lt;tytuł&gt; i &lt;łącze&gt; znaczniki są znajduje się i przetwarzane w celu wyświetlania każdego elementu danych. Rysunek 3 przedstawia przykład danych wyjściowych wygenerowanych z wprowadzeniem wywołanie za pośrednictwem serwera proxy JavaScript do metody sieci Web GetRssFeed() kodu ASP.NET AJAX.

## <a name="handling-complex-types"></a>Obsługa typów złożonych

Typy złożone akceptowane lub zwrócony przez usługę sieci Web są automatycznie widoczne za pośrednictwem serwera proxy JavaScript. Jednak złożone typy zagnieżdżone nie ma bezpośredniego połączenia po stronie klienta, chyba że atrybut GenerateScriptType jest stosowany do usługi, zgodnie z wcześniejszym opisem. Dlaczego czy chcesz użyć typu złożonego zagnieżdżonych po stronie klienta?

Aby odpowiedzieć na to pytanie, założono, że strony ASP.NET AJAX wyświetla dane klienta i umożliwia użytkownikom końcowym zaktualizować adres odbiorcy. Jeśli usługa sieci Web określa, czy typ adresu (typ złożony zdefiniowany w klasie CustomerDetails) mogą być wysyłane do klienta następnie proces aktualizacji można podzielić na odrębne funkcje do lepszego kodu ponownego użycia.


[![Tworzenie z wywołaniem usługi sieci Web, która zwraca dane RSS danych wyjściowych.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Rysunek 3**: Output tworzenia z wywołaniem usługi sieci Web, która zwraca dane RSS.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image9.png))


Wyświetlanie listy 16 zawiera przykładowy kod po stronie klienta, który wywołuje obiekt adres zdefiniowany w przestrzeni nazw modelu, wypełnia ją przy użyciu zaktualizowanych danych i przypisuje go do obiektu CustomerDetails właściwość Address. Obiekt CustomerDetails są następnie przekazywane do usługi sieci Web do przetworzenia.

**Wyświetlanie listy 16. Używanie zagnieżdżonych typów złożonych**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Tworzenie i korzystanie z metody stron

Usługi sieci Web zapewnić doskonałą sposób udostępnienia recyklingowi usług do różnych klientów, w tym stron ASP.NET AJAX. Jednak może być sytuacjach, gdy strona musi pobrać dane, które nigdy nie będzie można używane lub współużytkowany przez innych stron. W takim przypadku na plik .asmx Zezwalaj na stronę, aby uzyskać dostęp do danych może się wydawać zbyt obszerne ponieważ usługa jest używana tylko przez jedną stronę.

ASP.NET AJAX zapewnia inny mechanizm wywołania przypominającej usługi sieci Web bez tworzenia plików .asmx autonomicznych. Odbywa się przy użyciu techniki określane jako "strony metody". Strona metody to metody statyczne (udostępnionych w VB.NET) osadzone bezpośrednio w pliku strony lub kodu obok, mających atrybut WebMethod zastosowanych do nich. Stosując atrybutu WebMethod one można wywołać za pomocą specjalnych obiektu JavaScript o nazwie PageMethods, który pobiera tworzone dynamicznie w czasie wykonywania. Obiekt PageMethods działa jako serwer proxy, który chroni możesz z procesu serializacji/deserializacji JSON. Należy pamiętać, że było użyć obiektu PageMethods należy ustawić właściwość EnablePageMethods elementu ScriptManager na wartość true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Wyświetlanie listy 17 przedstawiono przykład zdefiniowanie dwóch metod strony ASP.NET kodu obok klasy. Te metody pobierania danych z klasy warstwie business znajduje się w aplikacji\_kod folderu witryny sieci Web.

**Wyświetlanie listy 17. Definiowanie metod strony.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Wykrycie ScriptManager obecności metody sieci Web na stronie generuje dynamiczne odwołanie do obiektu PageMethods wymienionych poniżej. Wywołanie metody sieci Web odbywa się za pomocą odwołań do klasy PageMethods następuje nazwa metody oraz wszystkie dane wymaganego parametru, który powinien zostać przekazany. Wyświetlanie listy 18 przedstawiono przykłady wywołanie dwóch metod strony przedstawiona wcześniej.

**Wyświetlanie listy 18. Wywołanie metod strony z obiektem PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Przy użyciu obiektu PageMethods jest bardzo podobny do sposobu używania obiekt serwera proxy JavaScript. Najpierw określ wszystkie dane parametru, który powinien zostać przekazany do metody strony, a następnie definiuje funkcję wywołania zwrotnego, która powinna być wywoływana podczas wywołania asynchronicznego zwraca. Można także określić wywołanie zwrotne błędu (na przykład obsługi błędów Zobacz listę 14).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender i zestawie narzędzi programu ASP.NET AJAX

Zestaw narzędzi platformy ASP.NET AJAX (dostępnej w sklepie [ http://ajax.asp.net ](http://ajax.asp.net)) oferuje kilka formantów, które mogą służyć do dostępu do usług sieci Web. W szczególności zestaw narzędzi zawiera przydatne formantu o nazwie `AutoCompleteExtender` który może służyć do wywołania usługi sieci Web w celu wyświetlenia danych na stronach bez pisania żadnego kodu JavaScript w ogóle.

Formant AutoCompleteExtender można rozszerzyć istniejące funkcje pole tekstowe i ułatwić użytkownikom łatwe Odszukiwanie danych, których szukają więcej. Wpisywania do pola tekstowego kontrolki może służyć do badania usługi sieci Web i dynamicznie pokazuje wyniki poniżej pola tekstowego. Na rysunku 4 przedstawiono przykład za pomocą kontroli AutoCompleteExtender do wyświetlenia identyfikatorów użytkowników dla aplikacji pomocy technicznej. Jako użytkownik wpisze różnych znaków w polu tekstowym, różne elementy będą wyświetlane poniżej ustalane na podstawie danych wejściowych. Użytkownicy mogą następnie wybrać identyfikator żądanego klienta.

Przy użyciu AutoCompleteExtender strony ASP.NET AJAX wymaga dodania zestawu AjaxControlToolkit.dll do folderu bin witryny sieci Web. Po dodaniu zestawu narzędzi, chcesz odwołać w pliku web.config, dzięki czemu formantów, które zawiera są dostępne dla wszystkich stron w aplikacji. Można to zrobić, dodając następujące znacznika w pliku web.config na &lt;formanty&gt; tagu:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

W przypadkach, gdy konieczne jest użycie kontroli w określonej strony można odwoływać się do niej przez dodanie dyrektywy odwołania do górnej części strony, jak pokazano w następnym zamiast aktualizowania pliku web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Używanie formantu AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Rysunek 4**: używanie AutoCompleteExtender formantu.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image12.png))


Gdy witryna sieci Web została skonfigurowana do używania w zestawie narzędzi programu ASP.NET AJAX, kontrolkę AutoCompleteExtender mogą być dodawane do strony znacznie tak, należy dodać regularne kontrolka serwerowa ASP.NET. Wyświetlanie listy 19 przedstawiono przykład do wywoływania usługi sieci Web za pomocą formantu.

**Wyświetlanie listy 19. Używanie formantu AutoCompleteExtender zestawu narzędzi programu ASP.NET AJAX.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender ma kilka różnych właściwości, w tym standardowe właściwości identyfikator i runat znalezionych na kontrolki serwera. Oprócz powyższych go można zdefiniować liczbę znaków typy użytkownika przed uruchomieniem usługi sieci Web zostanie zapytany o danych. Właściwość MinimumPrefixLength pokazano wyświetlania 19 powoduje, że usługa ma być wywoływana zawsze, gdy znak jest wpisane w polu tekstowym. Należy zachować ostrożność, że ustawienie tej wartości, ponieważ zawsze użytkownik wpisze znak usługi sieci Web zostanie wywołana w celu wyszukania wartości, które odpowiadają znaków w polu tekstowym. Usługa sieci Web do wywołania, a także docelowy metody sieci Web są definiowane przy użyciu właściwości ServicePath i ServiceMethod odpowiednio. Na koniec właściwość targetcontrolid równa identyfikuje które textbox na połączeniu AutoCompleteExtender formant.

Wywoływana usługi sieci Web musi mieć atrybut ScriptService stosowane zgodnie z wcześniejszym opisem i element docelowy metody sieci Web musi akceptuje dwa parametry o nazwie prefixText i liczba. Parametr prefixText reprezentuje znaków wpisana przez użytkownika końcowego, a parametr liczba liczbę elementów do zwrócenia (wartość domyślna wynosi 10). Wyświetlanie listy 20 przedstawiono przykład GetCustomerIDs metody sieci Web o nazwie przez formant AutoCompleteExtender przedstawionej w 19 wyświetlania. Metoda sieci Web wywołuje metodę warstwy biznesowej czy z kolei wywołuje danych warstwy metoda, która obsługuje filtrowanie danych i zwracanie pasujących wyników. Wyświetlania 21 pokazano kod dla metody warstwy danych.

**Wyświetlanie listy 20. Filtrowanie danych wysyłanych z formantu AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Wyświetlanie listy 21. Filtrowanie wyników ustalane na podstawie danych wejściowych użytkownika końcowego.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Wniosek

ASP.NET AJAX zapewnia obsługę znakomity wywoływania usługi sieci Web bez konieczności pisania partii niestandardowego kodu JavaScript do obsługi komunikatów żądań i odpowiedzi. W tym artykule przedstawiono sposób usług sieci Web .NET AJAX włącz je włączyć do przetwarzania komunikatów JSON oraz sposób definiowania serwery proxy JavaScript za pomocą formantu ScriptManager. Zapoznaniu się również, jak JavaScript, serwery proxy może służyć do wywoływania usługi sieci Web obsługi proste i złożone typy i postępowania z błędami. Ponadto przedstawiono sposób metody strony może służyć do uproszczenia procesu tworzenia i wywołania usługi sieci Web i jak kontroli AutoCompleteExtender może zapewniać pomoc dla użytkowników końcowych, jak typ. Mimo że element UpdatePanel dostępne w technologii ASP.NET AJAX z pewnością kontrolę nad wyborem w przypadku wielu programistów AJAX z powodu jego prostotę, wiedząc, sposób wywoływania usługi sieci Web za pośrednictwem serwerów proxy JavaScript mogą być przydatne w wielu aplikacjach.

## <a name="bio"></a>Mnie

daN Wahlin (Microsoft najbardziej wartościowych Professional dla platformy ASP.NET i usługi XML sieci Web) jest .NET development instruktora i architektura konsultanta na szkolenia technicznego interfejsu ([http://www.interfacett.com](http://www.interfacett.com)). DaN założona XML dla witryny sieci Web deweloperów platformy ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), znajduje się w biurze prelegenta INETA i mówi w kilku konferencji. DaN wspólnie utworzone Professional DNS systemu Windows (Wrox), platformy ASP.NET: porady, samouczki i kodu (Sams), ASP.NET 1.1 niejawnego rozwiązania, Professional ASP.NET 2.0 AJAX (Wrox), uzyskuje dostęp do programu ASP.NET 2.0 MVP i utworzone XML dla deweloperów platformy ASP.NET (Sams). Jeśli on nie zapisuje kodu, artykułów lub książek, zapisywania i rejestrowanie muzyka i odtwarzanie pól i koszykówki swoją żona i dzieci będą zadowoleni Dan.

Scott IE pracuje z technologii Microsoft Web od 1997 i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacje oparte na systemie koncentruje się na rozwiązania w zakresie oprogramowania bazy wiedzy Knowledge Base. Scott można nawiązać połączenie za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blogu w [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-localization.md)
> [dalej](understanding-asp-net-ajax-debugging-capabilities.md)
