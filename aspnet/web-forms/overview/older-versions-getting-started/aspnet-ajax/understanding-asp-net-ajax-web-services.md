---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Omówienie usług sieci Web programu ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Usługi sieci Web są integralną częścią programu .NET framework, które stanowią rozwiązanie dla wielu platform wymiany danych między systemami rozproszonymi. Mimo że w sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: c3874e19ef55ffd4949f9bc280d32a3da7a00271
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376388"
---
<a name="understanding-aspnet-ajax-web-services"></a>Omówienie usług sieci Web programu ASP.NET AJAX
====================
przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Usługi sieci Web są integralną częścią programu .NET framework, które stanowią rozwiązanie dla wielu platform wymiany danych między systemami rozproszonymi. Mimo, że usługi sieci Web są zwykle używane do różnych systemów operacyjnych, modele obiektów i języków programowania może wysyłać i odbierać dane, mogą też można dynamicznie Wstawianie danych do strony ASP.NET AJAX, lub wysłać dane ze strony do systemu zaplecza. Wszystko to może odbywać się bez konieczności uciekania się operacji odświeżania.


## <a name="calling-web-services-with-aspnet-ajax"></a>Wywołanie usługi sieci Web przy użyciu rozszerzeń ASP.NET AJAX

DaN Wahlin

Usługi sieci Web są integralną częścią programu .NET framework, które stanowią rozwiązanie dla wielu platform wymiany danych między systemami rozproszonymi. Mimo, że usługi sieci Web są zwykle używane do różnych systemów operacyjnych, modele obiektów i języków programowania może wysyłać i odbierać dane, mogą też można dynamicznie Wstawianie danych do strony ASP.NET AJAX, lub wysłać dane ze strony do systemu zaplecza. Wszystko to może odbywać się bez konieczności uciekania się operacji odświeżania.

Gdy kontrola kontrolki UpdatePanel ASP.NET AJAX zapewnia prosty sposób AJAX Włącz dowolnej strony ASP.NET, mogą wystąpić sytuacje, gdy potrzebujesz dynamicznie dostępu do danych na serwerze bez korzystania z UpdatePanel. W tym artykule pokazano, jak można to osiągnąć, tworząc i korzystanie z usług sieci Web w technologii ASP.NET AJAX stron.

Ten artykuł koncentruje się na funkcje dostępne w rozszerzenia AJAX programu ASP.NET core, a także kontrolki włączona usługa sieci Web w zestawie narzędzi programu ASP.NET AJAX o nazwie AutoCompleteExtender. Omówione zostały między innymi zdefiniowanie usług sieci Web z włączoną obsługą technologii AJAX, tworzenie serwerów proxy klienta i wywoływania usługi sieci Web za pomocą języka JavaScript. Zobaczysz również, jak usługa sieci Web wywołań bezpośrednio do metody strony ASP.NET.

## <a name="web-services-configuration"></a>Konfiguracja usługi sieci Web

Po utworzeniu nowego projektu witryny sieci Web za pomocą programu Visual Studio 2008 plik web.config zawiera szereg nowości, które mogą być nieznane użytkownicy poprzednich wersji programu Visual Studio. Prefiks "asp" niektóre z tych zmian mapy do kontrolek ASP.NET AJAX, dzięki czemu mogą być używane na stronach, ale inne zdefiniuj wymagane HttpHandlers i elementy HttpModule. Wyświetlanie listy 1 zawiera modyfikacje wprowadzone do `<httpHandlers>` elementu w pliku web.config, który wpływa na wywołania usługi sieci Web. Domyślne HttpHandler używani do przetwarzania wywołań .asmx została usunięta i zastąpiona przy użyciu klasy ScriptHandlerFactory znajduje się w zestawie System.Web.Extensions.dll. System.Web.Extensions.dll zawiera wszystkie najważniejsze funkcje używane przez program ASP.NET AJAX.

**Wyświetlanie listy 1. Konfiguracji obsługi dla usługi sieci Web platformy ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Ta zastępowania HttpHandler składa się w celu umożliwienia wywołania języka JavaScript Object Notation (JSON) ma zostać wykonane na stronach ASP.NET AJAX do usług sieci Web platformy .NET przy użyciu serwera proxy usługi sieci Web języka JavaScript. ASP.NET AJAX wysyła komunikaty JSON do usług sieci Web, w przeciwieństwie do standardowych wywołań proste obiektu dostępu protokołu (SOAP) zazwyczaj są skojarzone z usługami sieci Web. Skutkuje to mniejsze żądania i ogólną wiadomości odpowiedzi. Umożliwia ona również bardziej wydajne przetwarzanie danych po stronie klienta, ponieważ biblioteka ASP.NET AJAX JavaScript jest zoptymalizowana pod kątem pracy z obiektami JSON. Wyświetlanie listy 2 i 3 wyświetlania listy przedstawiają przykłady komunikatów żądań i odpowiedzi usługi sieci Web serializowany do formatu JSON. Komunikat żądania, pokazane w ofercie 2 przekazuje parametr kraju z wartością "Belgia", gdy komunikat odpowiedzi w ofercie 3 przekazuje tablicę obiektów klienta i skojarzonych z nimi właściwości.

**Wyświetlanie listy 2. Komunikat żądania usługi sieci Web serializować do notacji JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] Nazwa operacji jest zdefiniowany jako część adresu URL usługi sieci web; Ponadto komunikatów żądania nie są zawsze przesyłane za pośrednictwem JSON. Usługi sieci Web mogą korzystać z atrybutu ScriptMethod z parametrem UseHttpGet ustawionym na wartość true, co powoduje, że parametry, które zostaną przekazane za pośrednictwem parametrów ciągu zapytania.*


**Wyświetlanie listy 3. Komunikat odpowiedzi usługi sieci Web serializować do notacji JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

W następnej sekcji pokazano, jak utworzyć zdolne do obsługi komunikatów żądania JSON i odpowiada za pomocą proste i złożone typy usług sieci Web.

## <a name="creating-ajax-enabled-web-services"></a>Tworzenie usługi sieci Web z włączoną obsługą technologii AJAX

Struktura ASP.NET AJAX zapewnia kilka różnych sposobów w celu wywołania usługi sieci Web. Można użyć formantu AutoCompleteExtender (dostępne w zestawie narzędzi programu ASP.NET AJAX) lub JavaScript. Jednak przed wywołaniem usługi musisz AJAX — Włączanie go tak, aby go może być wywoływany przez kod skryptu klienta.

Czy jesteś nowym użytkownikiem usługi sieci Web platformy ASP.NET, znajdują się bardzo proste tworzenie i usługi z obsługą technologii AJAX. Program .NET framework jest obsługiwany tworzenia usług sieci Web ASP.NET od początkowego wydania w 2002 roku i rozszerzenia AJAX programu ASP.NET zapewnia dodatkowe funkcje AJAX, która opiera się na domyślny zestaw funkcji programu .NET framework. Visual Studio .NET 2008 Beta 2 ma wbudowaną obsługę tworzenia plików usługi sieci Web asmx i automatycznie pochodzi od klasy System.Web.Services.WebService klasy skojarzonego kodu obok klasy. W miarę dodawania metody do klasy, należy zastosować atrybut WebMethod, w kolejności ich wywołania przez klientów usługi sieci Web.

Wyświetlanie listy 4 przedstawiono przykład zastosowania atrybutu WebMethod do metodę o nazwie GetCustomersByCountry().

**Wyświetlanie listy 4. W usłudze sieci Web za pomocą atrybutu WebMethod**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Metoda GetCustomersByCountry() akceptuje parametr kraju i zwraca tablicę obiektów klienta. Wartość kraju, przekazana do metody jest przekazywany do klasy warstwy biznesowej, który z kolei wywołuje klasy warstwy danych, aby pobrać dane z bazy danych, wypełnij właściwości obiektu klienta przy użyciu danych i zwracają tablicy.

## <a name="using-the-scriptservice-attribute"></a>Za pomocą atrybutu ScriptService

Podczas dodawania WebMethod atrybut umożliwia metoda GetCustomersByCountry() ma zostać wywołana przez klientów wysyłających standardowe komunikaty protokołu SOAP z usługą sieci Web, on nie zezwala na wywołania JSON ma zostać wykonane z poziomu aplikacji ASP.NET AJAX gotowe. Aby zezwolić na wywołania JSON ma zostać wykonane, masz w celu zastosowania rozszerzenia AJAX programu ASP.NET `ScriptService` atrybutów do klasy usługi sieci Web. To pozwala usłudze sieci Web do wysyłania wiadomości odpowiedzi, sformatowany przy użyciu formatu JSON oraz umożliwia skryptu po stronie klienta do wywołania usługi przez wysyłanie komunikatów JSON.

Wyświetlanie listy 5 pokazano przykład zastosowania atrybutu ScriptService do klasy usługi sieci Web o nazwie CustomersService.

**Wyświetlanie listy 5. Za pomocą atrybutu ScriptService AJAX — Włączanie usługi sieci Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

Atrybut ScriptService działa jako znacznik, który wskazuje, że może ona zostać wywołana z kod skryptu AJAX. Faktycznie nie obsługuje JSON serializacji lub deserializacji zadań, które występują w tle. ScriptHandlerFactory (skonfigurowane w pliku web.config) oraz innych powiązanych klas do zbiorczego przetwarzania JSON.

## <a name="using-the-scriptmethod-attribute"></a>Za pomocą atrybutu ScriptMethod

Atrybut ScriptService jest to jedyny atrybut ASP.NET AJAX, który ma być zdefiniowany w usłudze sieci Web platformy .NET w celu użycia go do użycia przez strony ASP.NET AJAX. Jednak innego atrybutu o nazwie ScriptMethod mogą być stosowane bezpośrednio do metody sieci Web w usłudze. ScriptMethod definiuje trzy właściwości, w tym `UseHttpGet`, `ResponseFormat` i `XmlSerializeString`. Zmiana wartości tych właściwości może być przydatne w sytuacjach, w którym typ żądania akceptowane przez metody sieci Web musi zostać zmienione GET, gdy metoda sieci Web musi zwracać pierwotne dane XML w formie `XmlDocument` lub `XmlElement` obiekt lub gdy dane są zwracane z  usługi zawsze powinien zostać Zserializowany jako XML zamiast JSON.

UseHttpGet, właściwości mogą być używane, gdy metoda sieci Web powinna obsługiwać pobieranie żądań, w przeciwieństwie do żądania POST. Żądania wysyłane za pomocą adresu URL z parametrami wejściowymi metody sieci Web przeniesiona do parametry QueryString. UseHttpGet właściwości wartość domyślna to false i tylko powinna być równa `true` podczas operacji są znane jako bezpieczne i kiedy dane poufne nie zostanie przekazany do usługi sieci Web. Wyświetlanie listy 6 przedstawia przykład za pomocą atrybutu ScriptMethod z właściwością UseHttpGet.

**Wyświetlanie listy 6. Za pomocą atrybutu ScriptMethod z właściwością UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Przykładem nagłówki wysyłane, gdy wywoływana jest metoda sieci Web HttpGetEcho, które są wyświetlane w ofercie 6 są wyświetlane dalej:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Oprócz umożliwienia metody sieci Web w celu umożliwienia akceptowania żądań HTTP GET, atrybut ScriptMethod można również gdy odpowiedzi XML muszą być zwracane przez usługę zamiast JSON. Na przykład usługi sieci Web może pobrać źródła danych RSS z lokacją zdalną i zwracanie go jako obiektu XmlDocument lub atrybutu XmlElement. Przetwarzanie pliku XML danych następnie może wystąpić na komputerze klienckim.

Wyświetlanie listy 7 przedstawiono przykład przy użyciu właściwości Format odpowiedzi, aby określić, czy dane XML ma zostać zwrócone z metody sieci Web.

**Wyświetlanie listy 7. Za pomocą atrybutu ScriptMethod z właściwością format odpowiedzi.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

Właściwość Format odpowiedzi może również wraz z właściwością XmlSerializeString. Właściwość XmlSerializeString ma domyślną wartość false, co oznacza, że zwraca wszystkie typy z wyjątkiem ciągi, zwrócona z metody sieci Web są serializowane jako kod XML podczas `ResponseFormat` właściwość jest ustawiona na `ResponseFormat.Xml`. Gdy `XmlSerializeString` ustawiono `true`, wszystkie typy zwrócone z metody sieci Web są zserializowanym w formacie XML, w tym typów parametrów. Jeśli właściwość Format odpowiedzi ma wartość `ResponseFormat.Json` XmlSerializeString właściwość jest ignorowana.

Wyświetlanie listy 8 przedstawia przykład przy użyciu właściwości XmlSerializeString, aby wymusić ciągów w celu serializowana jako XML.

**Wyświetlanie listy 8. Za pomocą atrybutu ScriptMethod z właściwością XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Wartość zwracana z wywołania metody sieci Web GetXmlString przedstawione w ofercie 8 pokazano dalej:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Mimo że domyślnym formacie JSON minimalizuje całkowity rozmiar komunikatów żądań i odpowiedzi i łatwiej jest używany przez klientów technologii ASP.NET AJAX w sposób obsługiwania wielu przeglądarek, format odpowiedzi i XmlSerializeString właściwości może być wykorzystywana w przypadku klienta aplikacje, takie jak program Internet Explorer 5 lub nowszej oczekiwać, że dane XML mają być zwracane przez metody sieci Web.

## <a name="working-with-complex-types"></a>Praca z typów złożonych

Lista 5 pokazano przykład zwracająca typ złożony, o nazwie klienta z usługi sieci Web. Klasa odbiorcy definiuje kilka różnych typów prostych wewnętrznie jako właściwości, takie jak imię i nazwisko. Złożone typy używane jako parametr wejściowy lub zwracany typ metody sieci Web z włączoną obsługą technologii AJAX automatycznie są szeregowane w formacie JSON przed wysłaniem po stronie klienta. Jednak zagnieżdżonych typów złożonych (identyczne ze zdefiniowanymi wewnętrznie w ramach innego typu) nie są udostępniane klientowi jako w przypadku obiektów autonomicznych domyślnie.

W przypadkach, gdy zagnieżdżony typ złożony, używane przez usługę sieci Web musi też być używane w strony klienta można dodać atrybut GenerateScriptType AJAX programu ASP.NET z usługą sieci Web. Na przykład klasa CustomerDetails wyświetlane w ofercie 9 zawiera właściwości adresu i płeć który ***reprezentują zagnieżdżonych typów złożonych.***

**Wyświetlanie listy 9. Klasa CustomerDetails, pokazano poniżej zawiera dwa zagnieżdżonych typów złożonych.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Adres i płeć obiekty zdefiniowane w obrębie klasy CustomerDetails wyświetlane w ofercie 9 nie będzie automatycznie udostępnione do użycia po stronie klienta przy użyciu języka JavaScript, ponieważ są one zagnieżdżone typy (adres jest klasą i płeć jest wyliczeniem). W sytuacjach, gdy typem zagnieżdżonym używane w ramach usługi sieci Web musi być dostępny po stronie klienta, GenerateScriptType wspomniano wcześniej może zostać użyty atrybut (patrz lista 10). Ten atrybut można dodać wiele razy w przypadkach, w której zwracane są różnych typów złożonych zagnieżdżonych przy użyciu usługi. Mogą być stosowane, bezpośrednio klasa usługi sieci Web lub jego nowszą określonej metody sieci Web.

**Wyświetlanie listy 10. Za pomocą atrybutu GenerateScriptService, aby zdefiniować typy zagnieżdżone, które powinny być dostępne dla klienta.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Stosując `GenerateScriptType` atrybutu na typy usługi sieci Web, adres i płeć automatycznie będą dostępne do użytku przez kod ASP.NET AJAX JavaScript po stronie klienta. Przykładowy kod JavaScript, który jest automatycznie wygenerowane i wysłane do klienta przez dodanie atrybutu GenerateScriptType usługi sieci Web jest wyświetlany w ofercie 11. Pokazano, jak używać zagnieżdżonych typów złożonych w dalszej części tego artykułu.

**Wyświetlanie listy 11. Zagnieżdżone typy złożone udostępniane na stronie ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Teraz, gdy wiesz jak tworzyć usługi sieci Web i udostępnić je do stron ASP.NET AJAX, Spójrzmy na sposób tworzenia i używania serwery proxy JavaScript, tak aby dane mogą być pobierane lub wysyłane do usługi sieci Web.

## <a name="creating-javascript-proxies"></a>Tworzenie serwery proxy JavaScript

Zazwyczaj wywołanie standardowe usługi sieci Web (.NET lub innej platformy) obejmuje utworzenie obiektu serwera proxy, który możesz ochronnym od złożoności wysyłanie komunikatów żądań i odpowiedzi protokołu SOAP. Za pomocą wywołania usługi sieci Web programu ASP.NET AJAX serwery proxy JavaScript można tworzyć i pozwala łatwo wywoływać usługi bez martwienia się o serializacji i deserializacji komunikatów JSON. Serwery proxy JavaScript mogą być generowane automatycznie za pomocą formantu ScriptManager AJAX programu ASP.NET.

Tworzenie serwera proxy JavaScript, który można wywoływać usług sieci Web odbywa się za pomocą właściwości usługi ScriptManager. Ta właściwość umożliwia zdefiniowanie jednego lub więcej usług, które strony ASP.NET AJAX można wywołać asynchronicznie w celu wysyłania lub odbierania danych bez konieczności zwrotu operacji. W celu zdefiniowania usługi przy użyciu rozszerzeń ASP.NET AJAX `ServiceReference` kontroli i przypisywanie adresu URL usługi sieci Web z formantem `Path` właściwości. Wyświetlanie listy 12 przedstawiono przykład odwołuje się do usługi, o nazwie CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Wyświetlanie listy 12. Definiowanie usługi sieci Web używane na stronie ASP.NET AJAX.**

Dodawanie odwołania do CustomersService.asmx za pomocą formantu ScriptManager powoduje, że serwer proxy JavaScript, aby dynamicznie generowana i przywoływany przez stronę. Serwer proxy jest osadzony przy użyciu &lt;skryptu&gt; oznaczać i dynamicznie ładowany przez wywołanie pliku CustomersService.asmx i dołączanie /js na końcu. Poniższy przykład pokazuje, jak serwer proxy JavaScript jest osadzony na stronie, gdy debugowanie jest wyłączone w pliku web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Jeśli chcesz zobaczyć rzeczywisty kod serwera proxy JavaScript, który jest generowany możesz wpisać adres URL do żądanej usługi sieci Web platformy .NET w polu adresu programu Internet Explorer i Dołącz /js na końcu.*


Jeśli debugowanie jest włączone w pliku web.config, którą wersję debugowania serwera proxy JavaScript zostanie osadzony w stronę jako pokazano dalej:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Serwera proxy JavaScript utworzone przez funkcja ScriptManager można również osadzać bezpośrednio do strony, a nie za pomocą &lt;skryptu&gt; atrybut src znacznika. Można to zrobić, ustawiając właściwość InlineScript kontroli ServiceReference na wartość true (wartość domyślna to false). Może to być przydatne, gdy serwer proxy nie jest udostępniana na wielu stronach i chcesz zmniejszyć liczbę wywołań sieci do serwera. Nie można buforować kiedy InlineScript jest ustawiona na wartość true, skryptu serwera proxy przez przeglądarkę, więc wartość domyślna FALSE jest zalecane w sytuacjach, w którym serwer proxy jest używany przez wielu stronach w aplikacji ASP.NET AJAX. Przykład użycia właściwości InlineScript pokazano dalej:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Za pomocą serwery proxy JavaScript

Gdy usługa sieci Web odwołuje się na stronie ASP.NET AJAX za pomocą formantu ScriptManager, wywołanie może się z usługą sieci Web i zwrócone dane mogą być obsługiwane za pomocą funkcji wywołania zwrotnego. Usługa sieci Web jest wywoływana przez odwoływanie się do jego przestrzeń nazw (jeśli istnieje), nazwę klasy i nazwę metody sieci Web. Można zdefiniować parametry przekazywane do usługi sieci Web oraz funkcję wywołania zwrotnego, która obsługuje zwracanych danych.

Przykład użycia serwera proxy JavaScript do wywołania metody sieci Web o nazwie GetCustomersByCountry() jest wyświetlany w ofercie 13. Funkcja GetCustomersByCountry() jest wywoływana, gdy użytkownik końcowy kliknie przycisk na stronie.

**Wyświetlanie listy 13. Wywoływanie usługi sieci Web z serwerem proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

To wywołanie odwołuje się do przestrzeni nazw InterfaceTraining, CustomersService klasy i metody sieci Web GetCustomersByCountry zdefiniowane w usłudze. Przekazuje kraju uzyskaną z pola tekstowego, a także funkcję wywołania zwrotnego o nazwie OnWSRequestComplete, które powinny być używane, gdy zwraca asynchroniczne wywołanie usługi sieci Web. OnWSRequestComplete obsługuje tablicy obiektów klienta zwrócony przez usługę i konwertuje je do tabeli, która jest wyświetlana na stronie. Dane wyjściowe generowane z wywołania przedstawiono na rysunku 1.


[![Powiązanie danych uzyskanych, wprowadzając asynchroniczne wywołanie AJAX do usługi sieci Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Rysunek 1**: wiązanie danych uzyskanych, wprowadzając asynchroniczne wywołanie AJAX do usługi sieci Web.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image3.png))


Serwery proxy JavaScript można również ustawić jednokierunkowe wywołania usługi sieci Web, w przypadku gdy powinna być wywoływana metoda sieci Web, ale serwer proxy nie należy czekać na odpowiedź. Na przykład można wywołać usługę sieci Web w taki sposób, aby uruchomić proces, takich jak przepływ pracy, ale nie czeka na wartość zwracaną z usługi. W przypadkach, w którym wywołanie jednokierunkowe wymaga można nawiązać połączenia z usługą można po prostu pominąć funkcji wywołania zwrotnego, pokazane w ofercie 13. Ponieważ żadnej funkcji wywołania zwrotnego jest zdefiniowany obiekt serwera proxy nie będzie czekać przez usługę sieci Web zwrócić dane.

## <a name="handling-errors"></a>Obsługa błędów

Asynchroniczne wywołania zwrotne do usług sieci Web mogą wystąpić różne typy błędów, takie jak sieci są w dół, niedostępnością usługi sieci Web lub wyjątek zwracanego. Na szczęście usługa serwera proxy JavaScript — obiekty wygenerowane przez funkcja ScriptManager zezwala na wiele wywołań zwrotnych, należy zdefiniować do obsługi błędów i niepowodzeń, oprócz Powodzenie wywołania zwrotnego, przedstawionej wcześniej. Błąd funkcji wywołania zwrotnego można zdefiniować natychmiast po funkcji standardowego wywołania zwrotnego w wywołaniu metody sieci Web, jak pokazano w ofercie 14.

**Wyświetlanie listy 14. Definiowanie funkcji wywołania zwrotnego błędu i wyświetlanie błędów.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Błędy występujące podczas wywoływania usługi sieci Web spowoduje wyzwolenie funkcji wywołania zwrotnego OnWSRequestFailed() do wywołania, które przyjmuje obiekt reprezentujący błąd jako parametr. Obiekt błędu udostępnia kilka różnych funkcji w celu ustalenia przyczyny błędu również informację, czy upłynął limit czasu wywołania. Wyświetlanie listy 14 pokazano przykład za pomocą funkcji inny błąd i na rysunku 2 przedstawiono przykład dane wyjściowe generowane przez funkcje.


[![Dane wyjściowe generowane przez wywołanie funkcji błąd ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Rysunek 2**: dane wyjściowe generowane przez wywołanie funkcji błąd ASP.NET AJAX.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Obsługa danych XML zwrócony z usługi sieci Web

Wcześniej pokazano, jak metoda sieci Web może zwrócić pierwotne dane XML za pomocą atrybutu ScriptMethod wraz z jego właściwość Format odpowiedzi. Gdy ustawiono format odpowiedzi ResponseFormat.Xml, dane zwrócone przez usługę sieci Web jest serializowana jako XML, a nie w formacie JSON. Może to być przydatne, gdy dane XML, należy przekazać bezpośrednio do klienta dla przetwarzania przy użyciu języka JavaScript lub XSLT. Obecnie Internet Explorer 5 lub nowszej zapewnia najlepszy model obiektów po stronie klienta do analizowania i filtrowania danych XML z powodu jego wbudowaną obsługę programu MSXML.

Pobieranie danych XML przy użyciu usługi sieci Web nie różni się od pobierania pozostałych typów. Uruchom za pomocą serwera proxy JavaScript do wywołania funkcji odpowiednie i zdefiniuj funkcji wywołania zwrotnego. Po wywołaniu zwraca można przetwarzać dane w funkcji wywołania zwrotnego.

Wyświetlanie listy 15 pokazano przykład wywołania metody sieci Web o nazwie GetRssFeed(), która zwraca obiekt atrybutu XmlElement. GetRssFeed() przyjmuje jeden parametr reprezentujący adres URL dla źródła danych do pobrania RSS.

**Wyświetlanie listy 15. Praca z danymi XML zwracanymi przez usługi sieci Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Ten przykład przekazuje adres URL do źródła danych RSS i przetwarza zwracanych danych XML w funkcji OnWSRequestComplete(). OnWSRequestComplete() najpierw sprawdza, czy za pomocą przeglądarki Internet Explorer, aby dowiedzieć się, czy MSXML parser jest dostępna. Jeśli tak jest, instrukcję języka XPath jest używana do lokalizowania wszystkie &lt;elementu&gt; tagów w kanale informacyjnym RSS. Każdy element jest następnie postanowiliśmy za pośrednictwem oraz skojarzonych z nimi &lt;tytuł&gt; i &lt;łącze&gt; tagi są się i przetwarzane w celu wyświetlenia każdego elementu danych. Rysunek 3 przedstawia przykład danych wyjściowych wygenerowany na podstawie co wywołanie za pośrednictwem serwera proxy JavaScript, do metody sieci Web GetRssFeed() rozszerzeń ASP.NET AJAX.

## <a name="handling-complex-types"></a>Obsługa typów złożonych

Typy złożone zaakceptowane lub zwracane przez usługę sieci Web automatycznie są udostępniane za pośrednictwem serwera proxy JavaScript. Jednakże zagnieżdżone typy złożone nie są bezpośrednio dostępne po stronie klienta, chyba że GenerateScriptType jest stosowany do usługi zgodnie z wcześniejszym opisem. Dlaczego czy chcesz użyć zagnieżdżony typ złożony po stronie klienta?

Aby odpowiedzieć na to pytanie, założono, że na stronie ASP.NET AJAX dane klienta są wyświetlane i umożliwia użytkownikom końcowym zaktualizować adres odbiorcy. Jeśli usługa sieci Web określa, czy typ adresu (złożonych typ zdefiniowany w klasie CustomerDetails) mogą być wysyłane do klienta proces aktualizacji można podzielić na funkcji w celu lepszego ponownego użycia kodu.


[![Dane wyjściowe tworzenia wywołanie usługi sieci Web, która zwraca dane RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Rysunek 3**: dane wyjściowe tworzenia wywołanie usługi sieci Web, która zwraca dane RSS.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image9.png))


Lista 16 zawiera przykładowy kod po stronie klienta, który wywołuje obiekt adresów zdefiniowanych w modelu obszaru nazw, wypełnia go przy użyciu zaktualizowanych danych i przypisuje go do obiektu CustomerDetails właściwość Address. Obiekt CustomerDetails jest następnie przekazywany do usługi sieci Web do przetworzenia.

**Wyświetlanie listy 16. Używanie zagnieżdżonych typów złożonych**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Tworzenie i używanie metod strony

Usługi sieci Web zapewnia to doskonały sposób, aby udostępnić recyklingowi usługi różnego rodzaju klientów, w tym stron ASP.NET AJAX. Jednak może być przypadki, gdzie strony musi pobrać dane, które nigdy nie być używane lub udostępnione przez innych stron. W takim przypadku na plik .asmx Zezwalaj na stronę, aby uzyskać dostęp do danych może się wydawać obszerne, ponieważ usługa jest używana tylko przez pojedynczą stronę.

ASP.NET AJAX zapewnia inny mechanizm wywołań podobne do usługi sieci Web bez tworzenia plików .asmx autonomicznych. Odbywa się przy użyciu techniki określane jako "strony metody". Strona przedstawiono statyczny (udostępniony w VB.NET) metody, które osadzić bezpośrednio w pliku strony lub obok kodu, które mają atrybut WebMethod do nich. Za pomocą atrybutu WebMethod one może być wywoływany przy użyciu specjalnych obiekt JavaScript o nazwie PageMethods tworzony dynamicznie w czasie wykonywania. Obiekt PageMethods działa jako serwer proxy, który chroni możesz z procesem serializacji/deserializacji JSON. Należy pamiętać, że aby można było używać obiektu PageMethods należy ustawić ScriptManager EnablePageMethods właściwość na wartość true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Wyświetlanie listy 17 pokazano przykład zdefiniowanie dwóch metod strony ASP.NET obok kodu klasy. Te metody pobierania danych z klasy warstwy biznesowej znajduje się w aplikacji\_katalogu z kodem w witrynie sieci Web.

**Wyświetlanie listy 17. Definiowanie metody strony.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Gdy ScriptManager wykryje obecność metody sieci Web, na stronie generuje dynamiczne odwołanie do obiektu PageMethods wspomnianych wcześniej. Wywołanie metody sieci Web jest realizowane przez utworzenie odwołań do klasy PageMethods następuje nazwa metody i wszelkich danych wymaganych parametrów, który powinien zostać przekazany. Lista 18 pokazano przykłady wywoływania dwie metody strony przedstawionej wcześniej.

**Wyświetlanie listy 18. Wywoływanie metod strony z obiektem PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Za pomocą obiektu PageMethods jest bardzo podobne do obiektu serwera proxy JavaScript. Należy najpierw określić wszystkie dane parametru, który powinien być przekazywany do metody strony, a następnie zdefiniować funkcję wywołania zwrotnego, która powinna być wywoływana, gdy zwraca wywołania asynchronicznego. Można także określić wywołanie zwrotne błędu (na przykład obsługa błędów Zobacz listę 14).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender i zestawu narzędzi AJAX Toolkit platformy ASP.NET

Zestawu narzędzi AJAX Toolkit platformy ASP.NET (dostępnym [ http://ajax.asp.net ](http://ajax.asp.net)) oferuje kilka formantów, które mogą służyć do dostępu do usług sieci Web. W szczególności zestaw narzędzi zawiera przydatne formantu o nazwie `AutoCompleteExtender` który może służyć do wywołania usługi sieci Web i wyświetlanie danych na stronach bez pisania żadnego kodu JavaScript w ogóle.

Kontrolka AutoCompleteExtender może służyć do rozszerzania istniejących funkcji pole tekstowe i ułatwiają użytkownikom łatwy dostęp do danych, które szukają. Podczas ich wpisywania w polu tekstowym formant może służyć do kwerendy usługi sieci Web i dynamicznie przedstawia wyniki poniżej w polu tekstowym. Rysunek 4 przedstawia przykład za pomocą kontroli AutoCompleteExtender, aby wyświetlić identyfikatory klienta dla aplikacji pomocy technicznej. Jako użytkownik wpisuje różne znaki w polu tekstowym, różne elementy pojawią się pod nią na podstawie danych wejściowych. Użytkownicy mogą wybrać identyfikator żądanego klienta.

Korzystanie z AutoCompleteExtender strony ASP.NET AJAX wymaga, że zestaw AjaxControlToolkit.dll dodany do folderu bin witryny sieci Web. Po dodaniu zestawu narzędzi można odwołać się do niego w pliku web.config, aby formantów, które zawiera, są dostępne dla wszystkich stron w aplikacji. Można to zrobić, dodając następujący tag w z pliku web.config &lt;formantów&gt; tag:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

W przypadkach, w których konieczne jest korzystanie z kontrolki w określonej strony możesz odwoływać się do niej przez dodanie dyrektywy odwołania do górnej części strony, jak pokazano w następnym zamiast aktualizowania pliku web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Używanie formantu AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Rysunek 4**: za pomocą kontroli AutoCompleteExtender.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-web-services/_static/image12.png))


Po witrynie sieci Web została skonfigurowana do użycia z zestawu narzędzi AJAX programu ASP.NET, formant AutoCompleteExtender mogą być dodawane do strony znacznie takich, jak należy dodać regularne formant serwera ASP.NET. Wyświetlanie listy 19 pokazano przykład wywołać usługę sieci Web za pomocą formantu.

**Wyświetlanie listy 19. Używanie formantu AutoCompleteExtender zestawu narzędzi AJAX programu ASP.NET.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender ma kilka różnych właściwości, w tym standardowy identyfikator i runat właściwości formantów serwera. Oprócz wspomnianych umożliwia zdefiniowanie liczby znaków, które typy użytkownika końcowego przed uruchomieniem usługi sieci Web zostaje przesłane zapytanie danych. Właściwość MinimumPrefixLength wyświetlane w ofercie 19 powoduje, że usługa jest wywoływana za każdym razem, który znak jest wpisane w polu tekstowym. Będziesz chciał należy zachować ostrożność, że ustawienie tej wartości, ponieważ zawsze użytkownik wpisuje znak usługi sieci Web zostanie wywołana w celu wyszukania wartości, które dopasowuje znaki w polu tekstowym. Usługa sieci Web do wywołania, a także docelowym metody sieci Web są definiowane przy użyciu właściwości ServicePath i ServiceMethod odpowiednio. Na koniec właściwość TargetControlID identyfikuje textbox, które można dołączyć kontrolki AutoCompleteExtender.

Usługa sieci Web, wywoływana muszą mieć atrybut ScriptService stosowane zgodnie z wcześniejszym opisem i docelowy metody sieci Web muszą zaakceptować dwóch parametrów o nazwie prefixText i liczba. Parametr prefixText reprezentuje znaki wpisane przez użytkownika końcowego, a parametr liczby liczbę elementów do zwrócenia (wartość domyślna wynosi 10). Wyświetlanie listy 20 pokazano przykład metody sieci Web GetCustomerIDs, które są wywoływane przez formant AutoCompleteExtender przedstawionej w ofercie 19. Metoda sieci Web wywołuje metodę warstwy biznesowej, z kolei wywołania danych w warstwie metoda, która obsługuje filtrowanie danych i zwracanie pasujących wyników. Kod dla metody Warstwa danych jest wyświetlany w ofercie 21.

**Wyświetlanie listy 20. Filtrowanie danych wysyłanych z formantu AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Wyświetlanie listy 21. Filtrowanie wyników na podstawie danych wejściowych użytkownika końcowego.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Wniosek

ASP.NET AJAX oferuje niezrównane możliwości obsługi wywoływania usługi sieci Web bez konieczności pisania mnóstwo niestandardowy kod JavaScript do obsługi komunikatów żądań i odpowiedzi. W tym artykule przedstawiono sposób usługi sieci Web platformy .NET z obsługą technologii AJAX je włączyć do przetwarzania komunikatów JSON oraz sposób definiowania serwery proxy JavaScript za pomocą formantu ScriptManager. Wiesz też, jak JavaScript, serwery proxy może służyć do wywołania usługi sieci Web obsługi proste i złożone typy i postępowania z błędami. Na koniec wiesz, jak można używać metod strony w celu uproszczenia procesu tworzenia i wykonywanie wywołań usługi sieci Web i jak kontrolka AutoCompleteExtender może zapewnić pomoc dla użytkowników końcowych, podczas ich wpisywania. Mimo że kontrolę nad wyborem dla wielu programistów AJAX ze względu na prostotę jego UpdatePanel dostępne w technologii ASP.NET AJAX z pewnością, wiedząc sposób wywołania usług sieci Web za pośrednictwem serwery proxy JavaScript może być przydatne w wielu aplikacjach.

## <a name="bio"></a>Biografia

DaN Wahlin (Microsoft Most Valuable Professional platformy ASP.NET i usługi XML sieci Web) jest .NET development przez instruktorów i architektura Konsultant w szkoleniu technicznym interfejsu ([http://www.interfacett.com](http://www.interfacett.com)). DaN założona XML dla witryny sieci Web deweloperów platformy ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), znajduje się na biura osoby mówiącej INETA i występuje na konferencjach kilka. DaN wspólnie utworzone DNA Windows Professional (Wrox), platformy ASP.NET: wskazówki, samouczki i kodu (Sams), platformy ASP.NET 1.1 niejawnego programu testów rozwiązania, Professional ASP.NET 2.0 AJAX (Wrox), uzyskuje dostęp do platformy ASP.NET w wersji 2.0 programu MVP i XML utworzone dla deweloperów platformy ASP.NET (Sams). Jeśli on nie pisania kodu, artykuły lub książek, Dan cieszy się pisania i rejestrowanie utworów muzycznych i odtwarzanie golf i przeciwna swoją żoną i dzieci.

Scott Cate pracował nad przy użyciu technologii Microsoft Web od 1997 r i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacji koncentruje się na rozwiązania programowe wiedzy opartych na. Scott można się skontaktować za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blog znajduje się na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-localization.md)
> [dalej](understanding-asp-net-ajax-debugging-capabilities.md)
