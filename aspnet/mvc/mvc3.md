---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: "(obejmuje kwietnia 2011 aktualizacji narzędzi) ASP.NET MVC 3 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach przy użyciu wzorca projektowego ustalonym..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(obejmuje kwietnia 2011 aktualizacji narzędzi)*
> 
> ASP.NET MVC 3 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach za pomocą często używanych wzorów projektów oraz funkcji platform ASP.NET i .NET Framework.
> 
> Instaluje side-by-side z platformy ASP.NET MVC 2, aby rozpocząć dzisiaj przy użyciu!
> 
> Pobierz [Instalator w tym miejscu](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Najważniejsze funkcje

- Zintegrowany system szkieletów rozszerzalny za pośrednictwem pakietu NuGet
- Szablony projektu włączone 5 HTML
- Obszerne widoków, w tym aparat widoku Razor
- Zaawansowane punkty zaczepienia z iniekcji zależności i globalne filtry akcji
- Obsługa języka JavaScript sformatowanego z dyskretnego kodu JavaScript, jQuery sprawdzania poprawności i powiązanie JSON
- *Zapoznać się z listą wszystkich funkcji [poniżej](#overview)*

## <a name="top-links"></a>Górny łącza

Co to jest nowe w programie ASP.NET MVC 3

- Phil Haack: [platformy ASP.NET MVC 3 wydane](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC 3, program WebMatrix, NuGet, usługi IIS Express i Orchard wydane — wydanie sieci Web Microsoft stycznia w kontekście](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [o wersji programu ASP.NET MVC 3, IIS Express, SQL CE 4, struktura farmy sieci Web, Orchard, programu WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Informacje o wersji dla platformy ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalacja i pomocy

- Instalowane przy użyciu platformy ASP.NET MVC 3 [Instalatora platformy sieci Web (zalecane)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instalowane przy użyciu platformy ASP.NET MVC 3 [pliku wykonywalnego Instalatora](https://go.microsoft.com/fwlink/?LinkID=208140)
- Zainstaluj [platformy ASP.NET MVC 3 dla programu Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Odczyt [wprowadzenie do samouczka platformy ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Uzyskiwanie pomocy i przedyskutować ASP.NET MVC 3 w [fora](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Przegląd platformy ASP.NET MVC 3

ASP.NET MVC 3 opiera się na platformy ASP.NET MVC 1 i 2, dodawanie zalety obu uprościć kod i umożliwić obszerniejsze rozszerzalności. Ten temat zawiera omówienie wiele nowych funkcji, które znajdują się w tej wersji, podzielone na następujące sekcje:

- [Rozszerzalne szkieletów z integracją MvcScaffold](#BM_MvcScaffolding)
- [Szablony projektu włączone 5 HTML](#BM_HTML5)
- [Aparat widoku Razor](#BM_TheRazorViewEngine)
- [Obsługa wielu aparatów widoków](#BM_Support_for_Multiple_View_Engines)
- [Ulepszenia kontrolera](#BM_Controller_Improvements)
- [JavaScript oraz Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Ulepszenia walidacji modelu](#BM_Model_Validation_Improvements)
- [Ulepszenia iniekcji zależności](#BM_Dependency_Injection_Improvements)
- [Inne nowe funkcje](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Rozszerzalne szkieletów z integracją MvcScaffold

Nowy system szkieletów ułatwia na odebranie i rozpocząć korzystanie z efektywnie całkowicie nowych do struktury i automatyzowania typowych zadań związanych z projektowaniem, jeśli masz doświadczenie i znanych wykonywanych czynności.

Jest to obsługiwane przez nowe NuGet *szkieletów* pakietu o nazwie **MvcScaffolding**. Termin "Tworzenia szkieletu" jest używana przez wiele technologii oprogramowania oznacza "szybko generowania podstawowe konturu oprogramowanie, które można edytować i dostosować". Pakiet szkieletów tworzymy dla platformy ASP.NET MVC jest bardzo przydatne w kilku scenariuszach:

- **Jeśli program ASP.NET MVC zapoznawania się po raz pierwszy**, ponieważ umożliwia szybki sposób, aby uzyskać przydatne kod pracy, który można edytować i dostosować zgodnie z potrzebami. Wymaga od urazem patrzeć pustej strony, których nie wiadomo jak zacząć!
- **Należy dobrze znana ASP.NET MVC i są teraz eksploracji niektórych nowej technologii dodatek** takie jak relacyjna obiektu mapowania, aparat widoku testowania biblioteki, itp., ponieważ twórca tej technologii może również utworzono pakiet szkieletów dla niego.
- **Jeśli pracy obejmuje wielokrotnie tworzenia podobnych klasy lub pliki jakiegoś**, ponieważ można tworzyć niestandardowe scaffolders, które udostępniają testów osprzętu, skryptów wdrażania lub dowolny inny należy. Wszyscy członkowie zespołu za Użyj Twoje niestandardowe scaffolders.

Inne funkcje w MvcScaffolding:

- Pomoc techniczna dla projektów C# i VB
- Obsługa Razor i ASPX widok aparaty
- Obsługuje tworzenia szkieletu na obszary ASP.NET MVC i używa widok niestandardowy układów/wzorców
- Dane wyjściowe można łatwo dostosować, edytując szablony T4
- Możesz dodać całkowicie nowa scaffolders przy użyciu niestandardowej logiki środowiska PowerShell i niestandardowych szablonów T4. Te (i wszelkie niestandardowe parametry, został przez Ciebie udzielony) są automatycznie wyświetlane na liście uzupełniania po naciśnięciu tabulatora konsoli.
- Możesz uzyskać pakietów NuGet zawierający dodatkowe scaffolders dla różnych technologii (np. jest z koncepcji jeden dla składnika LINQ to SQL teraz) i mieszać i dopasowywać je razem

Aktualizacji narzędzi programu ASP.NET MVC 3 obsługuje bardzo Visual Studio dla tego systemu funkcja szkieletów, takich jak:

- Dodaj okna dialogowego kontroler obsługuje teraz szkieletów pełnej automatycznego tworzenia, odczytu, aktualizacji i usuwania działań kontrolera i odpowiednie widoki. Domyślnie to rusztowania kod dostępu do danych za pomocą funkcji EF Code First.
- Dodaj okna dialogowego kontroler obsługuje *extensible rusztowania* za pośrednictwem pakietu NuGet takie jak pakiety *MvcScaffolding*. Umożliwia to podłączenie rusztowania niestandardowych do okna dialogowego, który pozwoli utworzyć rusztowania dla innych technologii dostępu do danych, takich jak NHibernate lub nawet JET z ODBCDirect, jeśli jest to pochylone!

Aby uzyskać więcej informacji na temat szkieletów w ASP.NET MVC 3 zobacz następujące zasoby:

- Steve Sanderson post serii, w tym: 

    1. [Wprowadzenie: Tworzenie szkieletu z pakietem MvcScaffolding projektu programu ASP.NET MVC 3](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Użycie standardowych: typowe zastosowania i opcje](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relacje jeden do wielu](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Akcje szkieletów i testów jednostkowych](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Zastępowanie szablony T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Ten wpis: Tworzenie niestandardowego scaffolders](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman post z jego sesji kontrolera PDC 2010 [tworzenie blogu z firmą Microsoft "Bez nazwy pakietu z sieci Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Szablony projektów 5 HTML

Okno dialogowe Nowy projekt zawiera pole wyboru Włącz HTML 5 wersje szablony projektów. Te szablony wykorzystać 1.7 Modernizr, aby zapewnić obsługę zgodności HTML 5 i CSS 3 w przeglądarkach niższego poziomu.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Aparat widoku Razor

ASP.NET MVC 3 zawiera nowy aparat widoków o nazwie Razor, która oferuje następujące korzyści:

- Składnia razor jest czysty i zwięzłe, wymagających minimalna liczba naciśnięć klawiszy.
- Razor jest proste dowiedzieć się, w części, ponieważ jest on oparty na istniejących w językach C# i Visual Basic.
- Visual Studio zawiera IntelliSense i kod kolorowania składni Razor.
- Widokami razor można jednostki przetestowane bez konieczności uruchamiania aplikacji, lub uruchomić serwera sieci web.

Niektóre nowe funkcje Razor są następujące:

- `@model`Składnia określająca typ są przekazywane do widoku.
- `@* *@`składnia komentarza.
- Umożliwia określenie wartości domyślnych (takie jak `layoutpage`) raz dla całej witryny.
- `Html.Raw` Metody do wyświetlania tekstu bez kodowania HTML go.
- Obsługa udostępniania kodu wśród wielu widoków (*\_viewstart.cshtml* lub  *\_viewstart.vbhtml* plików).

Razor zawiera również nowe pomocników HTML, takie jak następujące:

- `Chart`. Renderuje wykresu, oferuje te same funkcje co formant wykresu w ASP.NET 4.
- `WebGrid`. Renderuje siatkę danych, wraz z funkcji stronicowania i sortowania.
- `Crypto`. Używa algorytmów, aby prawidłowo utworzyć mieszania ciągu inicjującego i wartość skrótu hasła.
- `WebImage`. Renderuje obraz.
- `WebMail`. Wysyła wiadomość e-mail.

Aby uzyskać więcej informacji na temat Razor zobacz następujące zasoby:

- [Scott Guthrie wpis w blogu introducing Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie blogu post wprowadzenie @model — słowo kluczowe](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie w blogu introducing układów Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Odwołanie API szybkie razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Obsługa wielu aparatów widoków

**Dodaj widok** okno dialogowe w programie ASP.NET MVC 3 pozwala wybrać aparat widoku, której chcesz używać, i **nowy projekt** okno dialogowe umożliwia określenie domyślny aparat widoku w projekcie. Można wybrać aparat widoku formularzy sieci Web (ASPX), Razor lub aparat widoku open source, takie jak [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), lub [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Ulepszenia kontrolera

### <a name="global-action-filters"></a>Globalne filtry akcji

Czasami chcesz przeprowadzić logiki przed uruchomieniem metody akcji lub po wykonaniu metody akcji. Aby to obsłużyć, ASP.NET MVC 2 podać filtrów akcji. Filtry akcji są deklaratywne umożliwiają dodanie akcji przed i po zachowania do metody akcji kontrolera określone atrybuty niestandardowe. Jednak w niektórych przypadkach można określić zachowanie akcji przed lub po akcji, która dotyczy wszystkich metod akcji. MVC 3 umożliwia określenie filtry globalne przez dodanie ich do `GlobalFilters` kolekcji. Aby uzyskać więcej informacji na temat globalne filtry akcji zobacz następujące zasoby:

- [Scott Guthrie blogu w wersji zapoznawczej 3 MVC](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrowanie na platformie ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nowe właściwości "Obiekt ViewBag."

Obsługa kontrolerów MVC 2 `ViewData` właściwość, która umożliwia przekazywanie danych do szablonu widoku przy użyciu interfejsu API słownik późnym wiązaniem. W MVC 3, można nieco prostsze składni `ViewBag` właściwości do wykonania tych samych celów. Na przykład zamiast zapisywania `ViewData["Message"]="text"`, można napisać `ViewBag.Message="text"`. Nie trzeba zdefiniować żadnych jednoznacznie klasy `ViewBag` właściwości. Właściwości dynamiczne, dlatego zamiast tego można po prostu pobrać lub ustaw właściwości i zostanie rozwiązany ich dynamicznie w czasie wykonywania. Wewnętrznie `ViewBag` właściwości są przechowywane jako pary nazwa/wartość w `ViewData` słownika. (Uwaga: w większości wersji wstępnych programu MVC 3 `ViewBag` nazwę `ViewModel` właściwości.)

### <a name="new-actionresult-types"></a>Nowe typy "ActionResult"

Następujące `ActionResult` typy i metody pomocnicze odpowiednie są nowe i udoskonalone w MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Zwraca kod stanu HTTP 404 do klienta.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Zwraca Przekierowanie tymczasowe (kod stanu HTTP 302) lub przekierowanie trwałe (kod stanu HTTP 301), w zależności od parametrów typu Boolean. W połączeniu z tą zmianą [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) klasa ma teraz trzech metod wykonywania przekierowania stałych: `RedirectPermanent`, `RedirectToRoutePermanent`, i `RedirectToActionPermanent`. Te metody zwracają wystąpienia `RedirectResult` z `Permanent` ustawioną właściwość `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Zwraca kod stanu HTTP określone przez użytkownika.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Ulepszenia Ajax i JavaScript

Domyślnie pomocników Ajax i weryfikacja w MVC 3 używają podejście dyskretnego kodu JavaScript. Dyskretny kod JavaScript pozwala uniknąć wstrzyknięcie wbudowanego JavaScript w kodzie HTML. Dzięki HTML mniejsze i czytelność i ułatwia wymienić lub Dostosuj biblioteki języka JavaScript. Pomocnicy weryfikacji w MVC 3 również użyć `jQueryValidate` wtyczki domyślnie. Jeśli chcesz zachowanie MVC 2, można wyłączyć dyskretnego kodu JavaScript przy użyciu *web.config* pliku ustawień. Aby uzyskać więcej informacji o usprawnieniach JavaScript oraz Ajax zobacz następujące zasoby:

- [Wstęp do dyskretnego kodu JavaScript w witrynie Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson dyskretnego kodu JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Post sprawdzania poprawności dyskretnego kodu JavaScript Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Tworzenie aplikacji programu MVC 3 z Razor i dyskretny kod JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (samouczek w witrynie programu ASP.NET)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Domyślnie włączona weryfikacja po stronie klienta

We wcześniejszych wersjach programu MVC, musisz jawnie wywołać `Html.EnableClientValidation` metody z widoku, aby włączyć weryfikację po stronie klienta. W MVC 3 to nie jest już wymagane ponieważ weryfikacji po stronie klienta jest domyślnie włączone. (Możesz wyłączyć to ustawienie w *web.config* pliku.)

Aby weryfikacji po stronie klienta do pracy należy nadal odwoływać się odpowiednie jQuery i jQuery weryfikacji biblioteki w witrynie. Możesz udostępniać tych bibliotek na danym serwerze lub odwoływać się do nich z sieci dostarczania zawartości (CDN), takie jak CDN od firmy Microsoft lub Google.

### <a name="remote-validator"></a>Zdalnego modułu weryfikacji

ASP.NET MVC 3 obsługuje nowe [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) klasy, która pozwala na wykorzystanie jQuery weryfikacji w plug na obsługę zdalnego modułu weryfikacji. Dzięki temu biblioteki weryfikacji po stronie klienta, umożliwiająca automatyczne wywoływanie metody niestandardowej, zdefiniowanego na serwerze w celu wykonywania logiki sprawdzania poprawności, które jest możliwe tylko po stronie serwera.

W poniższym przykładzie `Remote` atrybut określa, czy sprawdzanie poprawności klienta wywoła akcję o nazwie `UserNameAvailable` na `UsersController` klasy do weryfikacji `UserName` pola.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

W poniższym przykładzie przedstawiono odpowiedniego kontrolera.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Aby uzyskać więcej informacji o sposobie używania `Remote` atrybutów, zobacz [porady: Implementowanie zdalnej weryfikacji w aplikacji ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) w bibliotece MSDN.

### <a name="json-binding-support"></a>Obsługa powiązanie JSON

ASP.NET MVC 3 zawiera wbudowana obsługa powiązanie JSON umożliwia metod akcji odbierać dane zakodowane w formacie JSON oraz wiązanie modeli go do parametrów metody akcji. Ta funkcja jest przydatna w scenariuszach obejmujących szablony klienta i powiązania danych. (Klienta Szablony umożliwiają formatowania i wyświetlania danych pojedynczy element lub zbiór elementów danych przy użyciu szablonów, które są wykonywane na kliencie.) MVC 3 pozwala łatwo połączyć szablony klienta z metody akcji na serwerze, które wysyłać i odbierać dane JSON. Aby uzyskać więcej informacji o obsłudze powiązanie JSON, zobacz **JavaScript oraz AJAX ulepszenia** sekcji [MVC 3 Podgląd Scott Guthrie wpis w blogu](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Ulepszenia walidacji modelu

### <a name="dataannotations-metadata-attributes"></a>Atrybuty metadanych "DataAnnotations"

Obsługuje platformy ASP.NET MVC 3 `DataAnnotations` metadanych atrybutów, takich jak `DisplayAttribute`.

### <a name="validationattribute-class"></a>Klasa "ValidationAttribute"

`ValidationAttribute` Klasy zostało ulepszone w .NET Framework 4 do obsługi nowej `IsValid` przeciążenia, które zawiera więcej informacji o bieżącym kontekście weryfikacji, takie jak jak obiekt jest sprawdzana. Umożliwia to bardziej rozbudowane scenariuszy, w którym można sprawdzić bieżącą wartość na podstawie innej właściwości modelu. Na przykład nowy `CompareAttribute` atrybutu umożliwia porównanie wartości z dwóch właściwości modelu. W poniższym przykładzie `ComparePassword` musi odpowiadać właściwości `Password` pola, aby był prawidłowy.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Sprawdzanie poprawności interfejsów

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interfejs umożliwia przeprowadzanie weryfikacji na poziomie modelu i umożliwia udostępnienia weryfikacji komunikaty o błędach, które są specyficzne dla stan ogólny modelu lub między dwoma właściwości w modelu . MVC 3 teraz pobiera błędów z `IValidatableObject` interfejsu podczas wiązania modelu i automatycznie flagi lub prezentuje dotyczy pól w widoku przy użyciu wbudowanych pomocników formularza HTML.

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfejs umożliwia platformie ASP.NET MVC wykrywanie w czasie wykonywania czy moduł weryfikacji obsługuje weryfikację klienta. Ten interfejs został zaprojektowany tak, aby można zintegrować z różnych platform sprawdzania poprawności.

Aby uzyskać więcej informacji o weryfikacji interfejsów, temacie **ulepszenia sprawdzania poprawności modelu** sekcji [MVC 3 Podgląd Scott Guthrie wpis w blogu](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Jednak pamiętaj, że odwołanie do "IValidateObject" w blogu powinien być "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Ulepszenia iniekcji zależności

ASP.NET MVC 3 zapewnia lepszą obsługę stosowania zależności Iniekcja oraz integrowanie z kontenerami iniekcji zależności lub Inwersja kontroli (IOC). Dodano obsługę dla Podpisane w następujących obszarach:

- Kontrolery (rejestrowanie i wstrzyknięcie fabryki kontrolerów, zostanie wstrzyknięta kontrolerów).
- Widoki (rejestrowanie i wstrzyknięcie aparatów widoków, wstrzykiwania zależności do widoku strony).
- Filtry akcji (lokalizuje i wstrzyknięcie filtrów).
- Integratorów modeli (rejestrowanie i wstrzyknięcie).
- Dostawców weryfikacji modelu (rejestrowanie i wstrzyknięcie).
- Dostawcy metadanych modelu (rejestrowanie i wstrzyknięcie).
- Wartość dostawcy (rejestrowanie i wstrzyknięcie).

Obsługuje MVC 3 [wspólnego lokalizatora usług](https://github.com/unitycontainer/commonservicelocator) biblioteki i wszystkie Podpisane kontener, który obsługuje tej biblioteki `IServiceLocator` interfejsu. Obsługuje ona również nową `IDependencyResolver` interfejs, który ułatwia integrowanie struktur Podpisane.

Aby uzyskać więcej informacji na temat Podpisane w MVC 3 zobacz następujące zasoby:

- [Brad Wilson serii wpisy na blogu w lokalizacji usługi](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Inne nowe funkcje

### <a name="nuget-integration"></a>Integracji z programem NuGet

ASP.NET MVC 3 automatycznie instaluje i włącza NuGet w ramach jego instalacji. NuGet jest Menedżer wolnego pakietu open source, która ułatwia znajdowanie, instalowania i używania bibliotek .NET i narzędzia w projektach. Działa ze wszystkimi typami projektu Visual Studio (w tym programu ASP.NET MVC i formularzy sieci Web programu ASP.NET).

NuGet umożliwia deweloperom, którzy obsługują projektów typu open source (na przykład projekty jak Moq, NHibernate Ninject, StructureMap, NUnit, Windsor, RhinoMocks i Elmah) do pakietu ich bibliotek i zarejestrować je w galerii online. Następnie to proste dla deweloperów platformy .NET, które chcą korzystać z jednej z tych bibliotek można znaleźć pakietu i zainstaluj go w projektach, które działają na.

Przy użyciu narzędzia aktualizacji 3 ASP.NET szablony projektów obejmują JavaScript bibliotek wstępnie zainstalowane pakiety NuGet, dzięki czemu są one nadaje się do aktualizacji za pośrednictwem pakietu NuGet. Entity Framework Code First jest również wstępnie zainstalowane jako pakietu NuGet.

Aby uzyskać więcej informacji na temat narzędzia NuGet, zobacz [dokumentacji NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Stron częściowych wyjściowej pamięci podręcznej

ASP.NET MVC ma obsługiwane buforowanie danych wyjściowych strony pełnej odpowiedzi od wersji 1. MVC 3 obsługuje również buforowanie danych wyjściowych stron częściowych, co pozwala łatwo regiony pamięci podręcznej lub fragmenty odpowiedzi. Aby uzyskać więcej informacji na temat buforowania, zobacz **częściowe buforowanie danych wyjściowych strony** sekcji [Scott Guthrie wpis w blogu w wersji release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) i **podrzędnych akcji buforowanie danych wyjściowych** sekcji [informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Szczegółową kontrolę nad weryfikacji żądań

ASP.NET MVC ma weryfikacji żądań wbudowanych, który automatycznie pomaga w ochronie przed atakami polegającymi na wprowadzeniu XSS i HTML. Jednak czasami chcą jawnie wyłącz żądania weryfikacji, np. Jeśli chcesz umożliwić użytkownikom wysyłanie zawartości (na przykład w wpisy blogu lub zawartość CMS) HTML. Można teraz dodawać [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) atrybut modele lub wyświetl modele, aby wyłączyć sprawdzanie poprawności żądań na podstawie dla właściwości podczas wiązania modelu. Aby uzyskać więcej informacji o weryfikacji żądań zobacz następujące zasoby:

- **Dyskretny kod JavaScript i weryfikacja** sekcji [Scott Guthrie wpis w blogu w wersji release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Rozszerzalne "nowego projektu" — okno dialogowe

ASP.NET MVC 3 można dodać szablony projektów, aparatów widoków i struktury projektu do testu jednostkowego **nowy projekt** okno dialogowe.

### <a name="template-scaffolding-improvements"></a>Ulepszenia szkieletów szablonu

Szablony szkieletów platformy ASP.NET MVC 3 stanie dokładniej identyfikowania właściwości klucza podstawowego w modelach i obsługa je odpowiednio niż we wcześniejszych wersjach programu MVC. (Na przykład szablony szkieletów teraz upewnij się, że klucz podstawowy nie jest szkieletu jako pola formularza można edytować.)

Domyślnie używają teraz rusztowania tworzenie i edytowanie `Html.EditorFor` pomocnika zamiast `Html.TextBoxFor` pomocnika. Zwiększa to obsługę metadanych modelu w postaci danych adnotacji atrybutów, kiedy **Dodaj widok** okno dialogowe generuje widoku.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nowe przeciążenia dla "Html.LabelFor" i "Html.LabelForModel"

Dodano nowe przeciążenia metody `LabelFor` i `LabelForModel` metody pomocnicze. Nowe przeciążeń umożliwiają określ lub Zastąp tekst etykiety.

### <a name="sessionless-controller-support"></a>Obsługa Bezsesyjne kontrolera

ASP.NET MVC 3, można określić, czy klasa kontrolera, do korzystania ze stanu sesji, a jeśli tak, czy stan sesji powinien być odczytu/zapisu lub tylko do odczytu. Aby uzyskać więcej informacji na temat obsługi Bezsesyjne kontrolera, zobacz [informacje o wersji platformy MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nowa klasa "AdditionalMetadataAttribute"

Można użyć [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atrybutu, aby wypełnić `ModelMetadata.AdditionalValues` słownika właściwości modelu. Na przykład jeśli model widoku ma właściwość, która powinna być wyświetlana tylko dla administratora, użytkownik może dodawać adnotacje do tej właściwości jak pokazano w poniższym przykładzie:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Te metadane były dostępne dla dowolnego szablonu ekranu lub edytorze podczas renderowania widoku modelu produktu. Jest należy interpretować informacji o metadanych.

### <a name="accountcontroller-improvements"></a>Ulepszenia elementu AccountController

Elementu AccountController w szablonie projektu Internet została znacznie ulepszona.

### <a name="new-intranet-project-template"></a>Nowy szablon projektu sieci Intranet

Szablon projektu sieci Intranet jest uwzględniony co Włącza uwierzytelnianie systemu Windows i usuwa elementu AccountController.
