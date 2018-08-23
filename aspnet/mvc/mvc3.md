---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (obejmuje kwietnia 2011 narzędzia aktualizacji) ASP.NET MVC 3 to architektura służąca do tworzenia skalowalnych, oparte na standardach aplikacji sieci web przy użyciu wzorca projektowego sprawdzone...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 82d18865815568c5df9768fd9dd403f11ebd1714
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754367"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(obejmuje kwietnia 2011 narzędzia aktualizacji)*
> 
> ASP.NET MVC 3 to architektura służąca do tworzenia skalowalnych, oparte na standardach aplikacji sieci web przy użyciu wzorców projektowych sprawdzone oraz dzięki możliwościom platformy ASP.NET i .NET Framework.
> 
> Instaluje side-by-side przy użyciu wzorca ASP.NET MVC 2, więc Zacznij dzisiaj za pomocą!
> 
> Pobierz [Instalatora w tym miejscu](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Najważniejsze funkcje

- Zintegrowany system Scaffoldingu rozszerzalny za pośrednictwem pakietu NuGet
- HTML włączone szablony projektów 5
- Widoki ekspresyjny, łącznie z nowego aparatu widoku Razor
- Zaawansowane punkty zaczepienia przy użyciu iniekcji zależności i globalne filtry akcji
- Pomoc techniczna zaawansowane JavaScript z dyskretnego kodu JavaScript, jQuery sprawdzania poprawności i powiązania JSON
- *Zapoznać się z listą funkcji pełnego [poniżej](#overview)*

## <a name="top-links"></a>Najważniejsze linki

Co nowego we wzorcu ASP.NET MVC 3

- Phil haack o: [platformy ASP.NET MVC 3 wydania](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [wzorca ASP.NET MVC 3, WebMatrix, NuGet, usługi IIS Express i Orchard wydane - stycznia firmy Microsoft w sieci Web wersji w kontekście](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [ogłaszamy wydanie programu ASP.NET MVC 3, usług IIS Express, SQL CE 4, struktura farmy sieci Web, witryny systemu Orchard, programu WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Informacje o wersji dla platformy ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalacja i pomocy

- Instalowane przy użyciu platformy ASP.NET MVC 3 [Instalatora platformy sieci Web (zalecane)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instalowane przy użyciu platformy ASP.NET MVC 3 [Instalatora wykonywalnego](https://go.microsoft.com/fwlink/?LinkID=208140)
- Zainstaluj [ASP.NET MVC 3 dla programu Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Odczyt [wprowadzenie do samouczka platformy ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Uzyskaj Pomoc i omawiania ASP.NET MVC 3 w [forów](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Omówienie platformy ASP.NET MVC 3

ASP.NET MVC 3 opiera się na platformy ASP.NET MVC 1 i 2, dodawanie wspaniałe funkcje zarówno uprościć kod i umożliwić bardziej rozszerzalności. W tym temacie omówiono wiele nowych funkcji, które znajdują się w tej wersji, podzielone na następujące sekcje:

- [Rozszerzalne szkieletu z integracją MvcScaffold](#BM_MvcScaffolding)
- [HTML włączone szablony projektów 5](#BM_HTML5)
- [Aparat widoku Razor](#BM_TheRazorViewEngine)
- [Obsługa wielu aparatów widoku](#BM_Support_for_Multiple_View_Engines)
- [Ulepszenia kontrolera](#BM_Controller_Improvements)
- [JavaScript oraz Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Ulepszenia sprawdzania poprawności modelu](#BM_Model_Validation_Improvements)
- [Ulepszenia wstrzykiwania zależności](#BM_Dependency_Injection_Improvements)
- [Inne nowe funkcje](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Rozszerzalne szkieletu z integracją MvcScaffold

Nowy system Scaffoldingu ułatwia przejmą i rozpocząć korzystanie z wydajnie całkowicie nowych do struktury, jak i do automatyzacji typowych zadań programistycznych, jeśli jesteś doświadczonym i już wiesz, co robisz.

Jest to obsługiwane przez nowe NuGet *tworzenia szkieletów* pakiet o nazwie **MvcScaffolding**. Termin "Tworzenia szkieletów" jest używany przez wiele technologii oprogramowania oznacza "szybkie generowanie podstawowy zarys swoje oprogramowanie, które można następnie edytowanie i dostosowywanie". Pakietu tworzenia szkieletów, które tworzymy dla platformy ASP.NET MVC jest bardzo korzystne w kilku scenariuszach:

- **W przypadku platformy ASP.NET MVC zapoznawania się po raz pierwszy**, ponieważ zapewnia szybki sposób uzyskać niektóre przydatne kod pracy, w którym można edytować i dostosować zgodnie z potrzebami. Zapisuje możesz z urazem spojrzenie na pustej strony, których nie wiadomo jak zacząć!
- **Jeśli dobrze znana platformy ASP.NET MVC, a teraz analizowania niektóre nowej technologii dodatek** takich jak maper obiektowo relacyjny, aparat widoku, bibliotekę testów itp., ponieważ twórca technologia ta może również utworzony pakiet tworzenia szkieletów dla niego.
- **Jeśli pracy polega na wielokrotne tworzenie podobnych klas lub pliki jakieś**, ponieważ można utworzyć generatory niestandardowych, które zwracają świetlnymi testów, skryptów wdrażania lub czegokolwiek innego potrzebujesz. Każdy członek zespołu można użyć Twojego niestandardowego generatory zbyt.

Inne funkcje w MvcScaffolding:

- Pomoc techniczna dla projektów C# i VB
- Pomoc techniczna dotycząca Razor i ASPX widok aparaty
- Obsługuje tworzenia szkieletów w obszarach platformy ASP.NET MVC i używa widoku niestandardowego układy/wzorców
- Dane wyjściowe można łatwo dostosować, edytując szablony T4
- Możesz dodać całkowicie nowe generatory za pomocą niestandardowej logiki programu PowerShell i niestandardowe szablony T4. Te (oraz wszelkie parametry niestandardowe, został przez Ciebie udzielony) automatycznie są wyświetlane na liście uzupełniania po naciśnięciu tabulatora konsoli.
- Możesz uzyskać pakietach NuGet zawierających dodatkowe generatory dla różnych technologii (np. jest z koncepcji jeden dla programu LINQ to SQL teraz) i mieszać je oraz je ze sobą zgodne

ASP.NET MVC 3 Tools Update obsługuje doskonałe programu Visual Studio dla tego systemu tworzenia szkieletów, takich jak:

- Dodaj okno dialogowe kontroler obsługuje teraz szkieletu pełnej automatycznego tworzenia, odczytu, aktualizacji i usuwania działań kontrolera i odpowiednie widoki. Domyślnie to szkielety mechanizmów kod dostępu do danych za pomocą funkcji EF Code First.
- Dodaj okno dialogowe kontroler obsługuje *extensible szkielety mechanizmów* za pośrednictwem pakietu NuGet takie jak pakiety *MvcScaffolding*. Umożliwia to podłączenie niestandardowe szkielety mechanizmów do okna dialogowego, które pozwoli na tworzenie szkielety mechanizmów dla innych technologii dostępu do danych, takich jak NHibernate lub nawet JET przy użyciu ODBCDirect, jeśli jest więc pochylone!

Aby uzyskać więcej informacji na temat tworzenia szkieletów w ASP.NET MVC 3 zobacz następujące zasoby:

- Steve sanderson o po serii, w tym: 

    1. [Wprowadzenie: Tworzenia szkieletu z pakietem MvcScaffolding projektu ASP.NET MVC 3](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Użycie standardowych: typowe przypadki użycia i opcji](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relacje jeden do wielu](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Akcje tworzenia szkieletu i testów jednostkowych](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Zastępowanie szablony T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Ten wpis: Tworzenie niestandardowego generatory](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman wpis sesja podstawowego kontrolera domeny 2010 [tworzenia blogu z firmą Microsoft "Bez nazwy pakietu z sieci Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Szablony projektów 5 HTML

Okna dialogowego Nowy projekt zawiera pole wyboru Włącz HTML 5 wersje szablonów projektu. Te szablony wykorzystać Modernizr 1.7, aby zapewnić zgodność obsługę języków HTML 5 i CSS 3 w przeglądarkach niskiego poziomu.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Aparat widoku Razor

ASP.NET MVC 3 obejmuje nowy aparat widoków o nazwie Razor, który zapewnia następujące korzyści:

- Składnia razor jest czyste i zwięzłe, wymaganie minimalnej liczby naciśnięć klawiszy.
- Razor jest łatwe dowiedzieć się, w części, ponieważ jest on oparty na istniejących w językach C# i Visual Basic.
- Visual Studio zawiera funkcję IntelliSense i kod kolorowanie składni Razor.
- Widokami razor można jednostki przetestowane bez konieczności uruchamiania aplikacji, lub uruchomić serwera sieci web.

Niektóre nowe funkcje aparatu Razor są następujące:

- `@model` Składnia określająca typ są przekazywane do widoku.
- `@* *@` składnia komentarza.
- Możliwość określenia wartości domyślnych (takie jak `layoutpage`) jeden raz dla całej witryny.
- `Html.Raw` Metody do wyświetlania tekstu bez kodowania HTML go.
- Obsługa udostępniania kodu między wiele widoków (*\_viewstart.cshtml* lub  *\_viewstart.vbhtml* plików).

Razor zawiera również nowe pomocników HTML, takie jak następujące:

- `Chart`. Renderowanie wykresu, oferuje te same funkcje jak formant wykresu w programie ASP.NET 4.
- `WebGrid`. Wyświetla siatkę danych, wraz z funkcjonalności stronicowania i sortowania.
- `Crypto`. Algorytmy, aby utworzyć poprawnie wyznaczania wartości skrótu ciągu inicjującego i skrótu hasła.
- `WebImage`. Renderuje obraz.
- `WebMail`. Wysyła wiadomość e-mail.

Aby uzyskać więcej informacji na temat Razor zobacz następujące zasoby:

- [Wprowadzenie do aparatu Razor wpis w blogu Scotta Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Wprowadzenie do wpis w blogu Scotta guthrie'a zatytułowany @model — słowo kluczowe](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Wprowadzenie do układów Razor wpis w blogu Scotta Guthrie](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Odwołanie API szybkie razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Obsługa wielu aparatów widoku

**Dodaj widok** okno dialogowe w ASP.NET MVC 3 pozwala wybrać aparat widoku chcesz pracować, i **nowy projekt** okno dialogowe pozwala określić domyślny aparat widoku w projekcie. Możesz wybrać aparat widoku formularzy sieci Web (ASPX), Razor lub aparat typu open-source view takich jak [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), lub [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Ulepszenia kontrolera

### <a name="global-action-filters"></a>Globalne filtry akcji

Czasami chcesz wykonać logikę, przed uruchomieniem metody akcji lub po uruchomieniu metody akcji. Aby to umożliwić, ASP.NET MVC 2 podana filtrów akcji. Filtry akcji są atrybutów niestandardowych, które zapewniają deklaracyjne oznacza, że można dodać zachowanie akcja przed i po działaniu do metody akcji kontrolera określonej. Jednak w niektórych przypadkach można określić zachowanie akcja przed lub po akcji, która ma zastosowanie do wszystkich metod akcji. MVC 3 umożliwia określenie filtry globalne przez dodanie ich do `GlobalFilters` kolekcji. Aby uzyskać więcej informacji na temat globalne filtry akcji zobacz następujące zasoby:

- [Blog Scotta Guthrie Podgląd MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrowanie na platformie ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nowa właściwość "Obiekt ViewBag."

Obsługa kontrolerów MVC 2 `ViewData` właściwość, która umożliwia przekazywanie danych do szablonu widoku przy użyciu interfejsu API z późnym wiązaniem słownika. W MVC 3, można również użyć składni nieco prostsza `ViewBag` właściwości, aby osiągnąć ten sam cel. Na przykład zamiast zapisywania `ViewData["Message"]="text"`, można napisać `ViewBag.Message="text"`. Nie musisz zdefiniować silnie typizowanych klas do użycia `ViewBag` właściwości. Ponieważ właściwość dynamiczna, zamiast tego można po prostu pobrać lub ustawić właściwości i zostanie rozwiązany je dynamicznie w czasie wykonywania. Wewnętrznie `ViewBag` właściwości są przechowywane jako pary nazwa/wartość w `ViewData` słownika. (Uwaga: w większości wersji wstępnych programu MVC 3 `ViewBag` nosiła nazwę właściwości `ViewModel` właściwości.)

### <a name="new-actionresult-types"></a>Nowe typy "ActionResult"

Następujące `ActionResult` typy i odpowiedniej metody pomocnicze są nowe lub udoskonalone w MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Zwraca kod stanu 404 protokołu HTTP do klienta.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Zwraca Przekierowanie tymczasowe (kod stanu HTTP 302) lub trwałe przekierowanie (kod stanu HTTP 301), w zależności od parametrów typu Boolean. W połączeniu z tą zmianą [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) klasa ma teraz trzy metody służące do wykonywania przekierowania stałych: `RedirectPermanent`, `RedirectToRoutePermanent`, i `RedirectToActionPermanent`. Te metody zwracają wystąpienie `RedirectResult` z `Permanent` właściwością `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Zwraca kod stanu HTTP określonych przez użytkownika.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Ulepszenia technologii Ajax i JavaScript

Domyślnie obiekty pomocnicze technologii Ajax i sprawdzania poprawności w MVC 3 używają podejście dyskretnego kodu JavaScript. Pozwala uniknąć dyskretny kod JavaScript, wprowadza liniowy JavaScript w kodzie HTML. To sprawia, że HTML mniejsze i mniej zatłoczoną i ułatwia wymienić lub dostosować bibliotek JavaScript. Pomocnicy sprawdzania poprawności w MVC 3 również użyć `jQueryValidate` wtyczki domyślnie. Jeśli chcesz, aby zachowanie MVC 2, można wyłączyć dyskretnego kodu JavaScript przy użyciu *web.config* pliku ustawień. Aby uzyskać więcej informacji na temat ulepszenia języka JavaScript oraz Ajax zobacz następujące zasoby:

- [Wstęp do dyskretnego kodu JavaScript w witrynie Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson dyskretnego kodu JavaScript wpis](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Wpis sprawdzania poprawności dyskretnego kodu JavaScript Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Tworzenie aplikacji MVC 3 ze składnią Razor i dyskretny kod JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (samouczek w witrynie programu ASP.NET)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Weryfikacja po stronie klienta, domyślnie włączone

We wcześniejszych wersjach programu MVC, należy jawnie wywołać `Html.EnableClientValidation` metody z widoku w celu włączenia weryfikacji po stronie klienta. W MVC 3 to nie jest już wymagane, ponieważ Weryfikacja po stronie klienta jest domyślnie włączona. (Tę opcję można wyłączyć przy użyciu ustawień w *web.config* pliku.)

Aby weryfikacji po stronie klienta do pracy nadal należy odwoływać się do odpowiednich technologii jQuery i bibliotek weryfikacji jQuery w Twojej lokacji. Można udostępniać tych bibliotek na własnym serwerze lub odwoływać się do nich z sieci dostarczania zawartości (CDN), takich jak sieci CDN, od firmy Microsoft lub Google.

### <a name="remote-validator"></a>Zdalnego modułu weryfikacji

ASP.NET MVC 3 obsługuje nową [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) klasę, która umożliwia korzystanie z zalet jQuery weryfikacji dodatków plug-in jego obsługa zdalnego modułu weryfikacji. Dzięki temu biblioteki weryfikacji po stronie klienta i automatycznie wywoływać metodę niestandardową, definiujące na serwerze, aby można było wykonać logikę weryfikacji, który jest możliwe tylko po stronie serwera.

W poniższym przykładzie `Remote` atrybut określa, czy sprawdzanie poprawności klienta wywoła akcję o nazwie `UserNameAvailable` na `UsersController` klasy w celu zweryfikowania `UserName` pola.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Poniższy przykład pokazuje odpowiedniego kontrolera.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Aby uzyskać więcej informacji o sposobie używania `Remote` atrybutów, zobacz [porady: Implementowanie zdalnej weryfikacji we wzorcu ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) w bibliotece MSDN.

### <a name="json-binding-support"></a>Obsługa powiązań JSON

ASP.NET MVC 3 ma wbudowaną funkcję obsługi powiązania JSON, umożliwiająca metody akcji odbierać dane zakodowane w formacie JSON oraz wiązanie modeli go do parametrów metody akcji. Ta możliwość jest przydatne w scenariuszach związanych z szablonów klienta i powiązanie danych. (Szablony klienta umożliwiają formatowania i wyświetlania danych pojedynczy element lub zbiór elementów danych przy użyciu szablonów, które są wykonywane na komputerze klienckim.) MVC 3 pozwala na łatwe łączenie szablonów klienta za pomocą metod akcji na serwerze, które wysyłać i odbierać dane JSON. Aby uzyskać więcej informacji na temat obsługi powiązania JSON, zobacz **JavaScript oraz AJAX ulepszenia** części [MVC 3 w wersji zapoznawczej w blogu Scotta guthrie'a zatytułowany](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Ulepszenia sprawdzania poprawności modelu

### <a name="dataannotations-metadata-attributes"></a>Atrybuty metadanych na "DataAnnotations"

Obsługuje platformy ASP.NET MVC 3 `DataAnnotations` metadanych atrybutów, takich jak `DisplayAttribute`.

### <a name="validationattribute-class"></a>Klasa "ValidationAttribute"

`ValidationAttribute` Klasy poprawiła w .NET Framework 4, aby obsługiwać nowe `IsValid` przeciążenia, które zawiera więcej informacji na temat bieżącego kontekstu sprawdzania poprawności, takich jak jak obiekt jest weryfikowany. Dzięki temu bogatsze scenariusze, w którym można sprawdzić bieżącą wartość na podstawie innej właściwości w modelu. Na przykład nowy `CompareAttribute` atrybut ten umożliwia porównanie wartości dwóch właściwości modelu. W poniższym przykładzie `ComparePassword` właściwość musi być zgodna `Password` pola, aby był prawidłowy.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Sprawdzanie poprawności interfejsów

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interfejs umożliwia wykonywanie walidacji na poziomie modelu i pozwala ona na celu udostępnienia weryfikacji komunikaty o błędach, które są specyficzne dla stanu ogólnym modelu lub między nimi dwie właściwości w modelu . MVC 3 pobiera błędy z `IValidatableObject` interfejs podczas wiązania modelu i automatycznie flagi najważniejsze funkcje wpływ na co najmniej pól w widoku, używając wbudowanych pomocników HTML w formularzu.

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfejs umożliwia platformy ASP.NET MVC w celu odnalezienia w czasie wykonywania, czy moduł weryfikacji obsługuje weryfikację klienta. Ten interfejs został zaprojektowany tak, aby go można zintegrować z różnych platform sprawdzania poprawności.

Aby uzyskać więcej informacji o interfejsach weryfikacji, zobacz **ulepszenia sprawdzania poprawności modelu** części [MVC 3 w wersji zapoznawczej w blogu Scotta guthrie'a zatytułowany](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Pamiętaj jednak, że odwołania do "IValidateObject" w blogu powinny być "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Ulepszenia wstrzykiwania zależności

ASP.NET MVC 3 oferuje lepszą obsługę stosowania wstrzykiwanie zależności (DI) oraz integracji z usługą kontenerów wstrzykiwanie zależności lub Inwersja kontroli (IOC). Obsługa DI został dodany w następujących obszarach:

- Kontrolery (rejestrowanie i wprowadza fabryki kontrolerów, wprowadza kontrolerów).
- Widoki (rejestrowanie i wprowadza aparatów widoku, wstrzykiwania zależności do widoku strony).
- Filtry akcji (lokalizowanie i wprowadza filtry).
- Integratorów modelu (rejestrowanie i wprowadza).
- Dostawców weryfikacji modelu (rejestrowanie i wprowadza).
- Dostawcy metadanych modelu (rejestrowanie i wprowadza).
- Dostawców wartości (rejestrowanie i wprowadza).

MVC 3 obsługuje [wspólnego lokalizatora usług](https://github.com/unitycontainer/commonservicelocator) biblioteki i dowolnego kontenera DI, która obsługuje tej biblioteki `IServiceLocator` interfejsu. Obsługuje ona również nową `IDependencyResolver` interfejsu, który ułatwia integrowanie DI struktur.

Aby uzyskać więcej informacji na temat DI w MVC 3 zobacz następujące zasoby:

- [Brad Wilson serię wpisów w blogu usługi lokalizacji](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Inne nowe funkcje

### <a name="nuget-integration"></a>Integracja z NuGet

ASP.NET MVC 3 automatycznie instaluje i włącza NuGet jako część jego instalacji. NuGet to Menedżer bezpłatny pakiet typu open source, który można łatwo znaleźć, instalowanie i używanie bibliotek platformy .NET i narzędzi w swoich projektach. Działa z typami projektu Visual Studio (w tym ASP.NET Web Forms i ASP.NET MVC).

NuGet umożliwia deweloperom, którzy obsługują projektów typu open source (na przykład, projektów, takich jak Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks i Elmah) spakować bibliotek i zarejestrować je w galerii w trybie online. Następnie jest dla deweloperów platformy .NET, którzy chcą korzystać z jednej z tych bibliotek można znaleźć pakietu i zainstaluj go w projektach, które działają na jest łatwe.

Za pomocą programu ASP.NET 3 Tools Update szablony projektów obejmują JavaScript bibliotek wstępnie zainstalowanych pakietów NuGet, dzięki czemu są one można aktualizować za pośrednictwem pakietu NuGet. Entity Framework Code First jest również wstępnie zainstalowany jako pakiet NuGet.

Aby uzyskać więcej informacji na temat programu NuGet, zobacz [dokumentacja programu NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Buforowaniu danych wyjściowych stron częściowych

ASP.NET MVC ma obsługiwane buforowania danych wyjściowych pełnej strony odpowiedzi od wersji 1. MVC 3 obsługuje również buforowania danych wyjściowych części strony, która pozwala na łatwe regionów pamięci podręcznej lub fragmenty odpowiedzi. Aby uzyskać więcej informacji na temat buforowania, zobacz **częściowe buforowania danych wyjściowych strony** części [wpis w blogu Scotta guthrie'a zatytułowany w wersji Release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) i **podrzędne działania buforowania danych wyjściowych** części [MVC 3 — informacje o wersji](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Ścisła kontrola nad weryfikacji żądań

ASP.NET MVC jest weryfikacja żądań wbudowanych, który automatycznie zapewnia lepszą ochronę przed atakami polegającymi na iniekcji XSS, jak i HTML. Jednak czasami trzeba jawnie wyłącz żądania weryfikacji, np. Jeśli chcesz umożliwić użytkownikom wysyłanie HTML zawartości (na przykład w wpisy w blogu lub zawartości CMS). Teraz możesz dodać [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) atrybut do modeli lub wyświetl modele, aby wyłączyć sprawdzanie poprawności żądań na podstawie poszczególnych właściwości podczas wiązania modelu. Aby uzyskać więcej informacji na temat weryfikacji żądań zobacz następujące zasoby:

- **Dyskretnego kodu JavaScript i sprawdzanie poprawności** sekcji [wpis w blogu Scotta guthrie'a zatytułowany w wersji Release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Rozszerzalne "nowego projektu" okno dialogowe

W ASP.NET MVC 3, można dodać szablony projektów, aparatów widoków i struktury projektu do testów jednostkowych **nowy projekt** okno dialogowe.

### <a name="template-scaffolding-improvements"></a>Ulepszenia tworzenia szkieletów szablonu

Szablony tworzenia szkieletu ASP.NET MVC 3 wykonywanie lepszej pracy identyfikujące właściwości klucza podstawowego w modelach i całościowo odpowiednio niż we wcześniejszych wersjach programu MVC. (Na przykład szablony tworzenia szkieletów teraz upewnij się, że klucz podstawowy nie jest szkieletu jako pole edytowalnego formularza.)

Domyślnie używają teraz tworzyć i edytować szkielety mechanizmów `Html.EditorFor` pomocnika zamiast `Html.TextBoxFor` pomocnika. Zwiększa to obsługę metadanych modelu w postaci danych adnotacji atrybutów, kiedy **Dodaj widok** okno dialogowe generuje widoku.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nowe przeciążenia dla "Html.LabelFor" i "Html.LabelForModel"

Dodano nowe przeciążenia metody `LabelFor` i `LabelForModel` metody pomocnika. Nowe przeciążenia pozwalają na określenie lub Zastąp tekst etykiety.

### <a name="sessionless-controller-support"></a>Obsługa Bezsesyjne kontrolera

W ASP.NET MVC 3, można wskazać, czy klasa kontrolera, aby używać stanu sesji, a jeśli tak, czy stan sesji powinien być odczytu/zapisu lub tylko do odczytu. Aby uzyskać więcej informacji na temat obsługi kontrolera Bezsesyjne zobacz [MVC 3 — informacje o wersji](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nowa klasa "AdditionalMetadataAttribute"

Możesz użyć [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atrybutu, aby wypełnić `ModelMetadata.AdditionalValues` słownik właściwości modelu. Na przykład jeśli model widoku ma właściwość, która powinna być wyświetlana tylko dla administratora, można dodać adnotacje tę właściwość zgodnie z poniższym przykładem:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Te metadane ma zostać udostępnione dowolnego szablonu ekranu lub edytorze, po wyrenderowaniu modelu widoku produktu. Jest do interpretacji informacji o metadanych.

### <a name="accountcontroller-improvements"></a>Ulepszenia elementu AccountController

Elementu AccountController w szablonie projektu internetowego została znacznie ulepszona.

### <a name="new-intranet-project-template"></a>Nowy szablon projektu sieci Intranet

Nowy szablon projektu sieci Intranet jest uwzględniany który umożliwia użycie uwierzytelniania Windows i spowoduje usunięcie elementu AccountController.
