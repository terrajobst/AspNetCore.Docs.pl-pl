---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Model 2.0 strony ASP.NET | Dokumentacja firmy Microsoft
author: microsoft
description: W programie ASP.NET 1.x, deweloperzy ma wybór między kodu wbudowanego i modelu kodu związane z kodem. Związane z kodem można zaimplementować za pomocą Src attr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891298"
---
<a name="the-aspnet-20-page-model"></a>Model 2.0 strony ASP.NET
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W programie ASP.NET 1.x, deweloperzy ma wybór między kodu wbudowanego i modelu kodu związane z kodem. Związane z kodem można zaimplementować za pomocą atrybutu Src lub atrybut plik CodeBehind @Page dyrektywy. W programie ASP.NET 2.0 deweloperzy nadal mają wybór między kodu wbudowanego i związane z kodem, ale było istotne ulepszenia modelu związane z kodem.


W programie ASP.NET 1.x, deweloperzy ma wybór między kodu wbudowanego i modelu kodu związane z kodem. Związane z kodem można zaimplementować za pomocą atrybutu Src lub atrybut plik CodeBehind @Page dyrektywy. W programie ASP.NET 2.0 deweloperzy nadal mają wybór między kodu wbudowanego i związane z kodem, ale było istotne ulepszenia modelu związane z kodem.

## <a name="improvements-in-the-code-behind-model"></a>Ulepszenia w modelu związane z kodem

Aby lepiej zrozumieć zmian w modelu kodu powiązanego w programie ASP.NET 2.0, zaleca się szybko przejrzeć modelu w postaci, w jakiej były dostępne w programie ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Model kodu powiązanego w programie ASP.NET 1.x

W programie ASP.NET 1.x, modelu CodeBehind składa się z plikiem ASPX (formularza sieci Web) i plik CodeBehind kodem programowania. Dwa pliki zostały połączone za pomocą @Page dyrektywy w pliku ASPX. Na stronie ASPX każdego formantu ma deklarację odpowiednie w pliku CodeBehind jako zmienna wystąpienia. Plik CodeBehind również zawiera kod dla wiązania zdarzeń i wygenerowanego kodu niezbędne do projektanta programu Visual Studio. Ten model stosunkowo dobrze działa, ale ponieważ ASP.NET, co element na stronie ASPX wymagane odpowiedni kod w pliku związanym z kodem, nie było żadnych true oddzielenie kodu i zawartości. Na przykład jeśli projektant dodany nowy formant serwera z plikiem ASPX poza środowiska IDE programu Visual Studio, aplikacji spowoduje przerwanie z powodu braku deklaracji dla tego formantu w pliku CodeBehind.

## <a name="the-code-behind-model-in-aspnet-20"></a>Model kodu powiązanego w programie ASP.NET 2.0

Platforma ASP.NET 2.0 znacznie poprawia tego modelu. W programie ASP.NET 2.0, związane z kodem jest implementowane za pomocą nowej *klasy częściowe* dostępnych w programie ASP.NET 2.0. Klasy związane z kodem w programie ASP.NET 2.0 jest definiowane jako częściowej klasy, co oznacza, że zawiera on tylko część definicji klasy. Pozostała część definicji klasy dynamicznie jest generowany przez ASP.NET 2.0 za pomocą strony ASPX, w czasie wykonywania, lub gdy witryna sieci Web została wstępnie skompilowana. Skojarzenia między plik CodeBehind i strony ASPX nadal jest określana za pomocą dyrektywy @ Page. Jednak zamiast atrybutu plik CodeBehind lub Src, ASP.NET 2.0 teraz używa atrybut CodeFile. Atrybut Inherits umożliwia również określić nazwę klasy dla strony.

Typowy dyrektywy @ Page może wyglądać następująco:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Definicję typowej klasy w pliku CodeBehind ASP.NET 2.0 może wyglądać następująco:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# i Visual Basic są tylko języki zarządzanych, które obsługuje obecnie klas częściowych. W związku z tym deweloperzy przy użyciu J# nie będzie mógł używać modelu kodu powiązanego w programie ASP.NET 2.0.


Nowy model zwiększa modelu związane z kodem, ponieważ deweloperzy mają teraz pliki kodu, które zawierają tylko kod, który zostały one utworzone. Zapewnia także true oddzielenie kodu i zawartości, ponieważ nie ma żadnych wystąpienia deklaracji zmiennych w pliku CodeBehind.

> [!NOTE]
> Ponieważ częściowej klasy strony ASPX Jeśli powiązanie zdarzeń ma miejsce, deweloperów języka Visual Basic osiągnąć wzrost wydajności nieznaczne przy użyciu słowa kluczowego dojść w związane z kodem powiązać zdarzenia. C# ma nie równoważne — słowo kluczowe.


## <a name="new--page-directive-attributes"></a>Nowe atrybuty @ Page — dyrektywa

Platforma ASP.NET 2.0 dodaje wiele nowych atrybutów do dyrektywy @ Page. Następujące atrybuty są nowe w programie ASP.NET 2.0.

## <a name="async"></a>Async

Atrybut Async pozwala na skonfigurowanie strony, aby być wykonywane asynchronicznie. Obejmuje również asynchroniczne stron w dalszej części tego modułu.

## <a name="asynctimeout"></a>AsyncTimeout

Określony limit czasu asynchronicznego stron. Wartość domyślna wynosi 45 sekund.

## <a name="codefile"></a>CodeFile

Atrybut CodeFile zastępuje atrybutu plik CodeBehind w programie Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Atrybut CodeFileBaseClass jest używany w przypadkach, w którym ma wiele stron pochodzić z jednej klasy podstawowej. Z powodu wykonania klas częściowych w ASP.NET, bez tego atrybutu klasę podstawową używaną do odwołują się do formantów zadeklarowany w strony ASPX współdzielonych pól wspólnej nie będzie działać prawidłowo ponieważ ASP. Aparat kompilacji sieci automatycznie utworzy nowe elementy członkowskie oparte na formanty na stronie. W związku z tym jeśli chcesz wspólna klasa podstawowa dla dwóch lub więcej stron w programie ASP.NET, należy zdefiniować Określ klasy podstawowej w atrybucie CodeFileBaseClass, a następnie pochodzi każdej klasy stron od tej klasy podstawowej. Atrybut CodeFile jest również wymagany, gdy jest używany ten atrybut.

## <a name="compilationmode"></a>właściwość compilationMode

Ten atrybut umożliwia należy ustawić właściwość CompilationMode strony ASPX. Właściwość CompilationMode jest wyliczenie zawierające wartości **zawsze**, **automatycznie**, i **nigdy**. Wartość domyślna to **zawsze**. **Automatycznie** ustawienie uniemożliwi dynamicznie, jeśli to możliwe kompilowania strony ASP.NET. Zwiększa wydajność, z wyłączeniem stron z kompilacji dynamicznej. Jednak jeśli strony, który jest wykluczony zawiera ten kod, który musi być skompilowany, będzie można zgłoszony błąd podczas przeglądania strony.

## <a name="enableeventvalidation"></a>EnableEventValidation

Ten atrybut określa, czy są weryfikowane zdarzenia odświeżania strony i wywołania zwrotnego. Gdy ta opcja jest włączona, argumenty odświeżania lub wywołania zwrotnego zdarzenia są sprawdzane w celu zapewnienia, że pochodzą z formantu serwera, który pierwotnie je odwzorował.

## <a name="enabletheming"></a>Właściwość EnableTheming

Ten atrybut określa, czy kompozycji ASP.NET są używane na stronie. Wartość domyślna to **false**. Motywy ASP.NET są objęte [10 modułu](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Ten atrybut określa, czy dyrektywy pragma linii powinny zostać dodane podczas kompilacji. Wyrażenia pragma wiersza są opcje używane przez debugery, aby oznaczyć określonych sekcji kodu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Ten atrybut określa, czy JavaScript jest wstrzykiwane do strony w celu utrzymania pozycji przewijania między odświeżeniami. Ten atrybut jest **false** domyślnie.

Jeśli ten atrybut jest **true**, doda ASP.NET &lt;skryptu&gt; bloku odświeżania strony, która wygląda następująco:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Należy pamiętać, że src tego bloku skryptu WebResource.axd. Ten zasób nie jest ścieżką fizyczną. Po zażądaniu tego skryptu ASP.NET dynamicznie tworzy skrypt.

### <a name="masterpagefile"></a>MasterPageFile

Ten atrybut określa plik strony głównej dla bieżącej strony. Ścieżka może być względna lub bezwzględna. Strony główne są objęte [modułu 4](master-pages.md).

## <a name="stylesheettheme"></a>Element StyleSheetTheme

Ten atrybut umożliwia zastąpienie interfejsu użytkownika wygląd właściwości zdefiniowane przez motyw programu ASP.NET 2.0. Motywy są objęte [10 modułu](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Motyw

Określa motyw dla strony. Jeśli nie określono wartości dla atrybutu element StyleSheetTheme, atrybutu motywu zastępuje wszystkie style stosowana do formantów na stronie.

## <a name="title"></a>Tytuł

Ustawia tytuł strony. Wartość określona w tym miejscu będą widoczne w &lt;tytuł&gt; element renderowanej strony.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ustawia wartość wyliczenia ViewStateEncryptionMode. Dostępne wartości to **zawsze**, **automatycznie**, i **nigdy**. Wartość domyślna to **automatycznie**. Jeśli ten atrybut ma ustawioną wartość **automatycznie**, są szyfrowane, stan widoku jest formant żądania przez wywołanie metody **RegisterRequiresViewStateEncryption** — metoda.

## <a name="setting-public-property-values-via-the--page-directive"></a>Ustawianie wartości właściwości publicznej za pośrednictwem @ Page — dyrektywa

Kolejna nowa możliwość dyrektywy @ Page, w programie ASP.NET 2.0 jest możliwość określenia początkowej wartości właściwości publicznej klasy podstawowej. Załóżmy na przykład, że masz właściwości publicznej o nazwie **SomeText** w klasie podstawowej i d podoba Ci się zostać zainicjowany do **Hello** po załadowaniu strony. Można to zrobić po prostu ustawiając wartość w dyrektywy @ Page w następujący sposób:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** atrybutu dyrektywy @ Page ustawia początkowa wartość właściwości SomeText w klasie podstawowej, aby *Witaj!*. Poniższy klip wideo jest wskazówki ustawienia początkowej wartości właściwości publicznej w klasie podstawowej za pomocą dyrektywy @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Otwórz wideo na pełnym ekranie](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nowe właściwości publicznej klasy strony

Następujące właściwości publiczne są nowe w programie ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Zwraca ścieżkę względem aplikacji do strona lub kontrolka. Na przykład dla strony znajduje się w http://app/folder/page.aspx, zwraca właściwość ~ / folderu.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Zwraca ścieżka względna katalogu wirtualnego do strony lub formantu. Na przykład dla strony znajduje się w http://app/folder/page.aspx, zwraca właściwość ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Pobiera lub ustawia limit czasu używany do obsługi asynchroniczne strony. (Asynchroniczne strony zostaną uwzględnione w dalszej części tego modułu.)

## <a name="clientquerystring"></a>ClientQueryString

Właściwość tylko do odczytu, która zwraca część ciągu zapytania żądanego adresu URL. Ta wartość jest zakodowane w adresie URL. Metoda UrlDecode klasy HttpServerUtility umożliwia zdekodować.

## <a name="clientscript"></a>ClientScript

Ta właściwość zwraca obiekt ClientScriptManager, który może służyć do zarządzania emisji ASP.NETs skryptu po stronie klienta. (Klasa ClientScriptManager jest uwzględnione w dalszej części tego modułu).

## <a name="enableeventvalidation"></a>EnableEventValidation

Ta właściwość określa, czy sprawdzanie poprawności zdarzenia jest włączony dla zdarzeń ogłaszania zwrotnego i wywołania zwrotnego. Po włączeniu argumentów odświeżania lub wywołania zwrotnego zdarzenia są weryfikowane i upewnij się, że pochodzą z formantu serwera, który pierwotnie je odwzorował.

## <a name="enabletheming"></a>Właściwość EnableTheming

Ta właściwość pobiera lub ustawia wartość logiczna określająca, czy motyw programu ASP.NET 2.0 jest stosowany do strony.

## <a name="form"></a>Formularz

Ta właściwość zwraca formularza HTML na stronie ASPX jako obiekt HtmlForm.

## <a name="header"></a>nagłówek

Ta właściwość zwraca odwołanie do obiektu HtmlHead, który zawiera nagłówek strony. Zwrócony obiekt HtmlHead służy do pobierania/ustawiania arkusze stylów, tagi Meta itp.

## <a name="idseparator"></a>IdSeparator

Ta właściwość tylko do odczytu pobiera znak, który jest używany do oddzielania identyfikatory kontroli, gdy program ASP.NET tworzy unikatowy identyfikator dla formantów na stronie. Nie jest on przeznaczony do użycia bezpośrednio w kodzie.

## <a name="isasync"></a>IsAsync

Ta właściwość umożliwia asynchroniczne stron. Asynchroniczne stron zostały omówione w dalszej części tego modułu.

## <a name="iscallback"></a>IsCallback

Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest wynikiem wywołania zwrotnego. Wywołań zwrotnych zostały omówione w dalszej części tego modułu.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest częścią odświeżania strony między stronami. Między stronami ogłaszania zwrotnego są uwzględnione w dalszej części tego modułu.

## <a name="items"></a>Elementy

Zwraca odwołanie do wystąpienia IDictionary, który zawiera wszystkie obiekty przechowywane w kontekście strony. Można dodać elementy do tego obiektu IDictionary i będą one dostępne w okresie istnienia w kontekście.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Ta właściwość określa, czy program ASP.NET emituje JavaScript, który utrzymuje się, że strony przewiń pozycji w przeglądarce po wystąpieniu odświeżania strony. (Szczegóły tej właściwości zostały opisanych wcześniej w tym module).

## <a name="master"></a>wzorzec

Ta właściwość tylko do odczytu zwraca odwołanie do wystąpienia formantu MasterPage dla strony, do którego zastosowano strony wzorcowej.

## <a name="masterpagefile"></a>MasterPageFile

Pobiera lub ustawia nazwę pliku strony wzorcowej dla strony. Tej właściwości można ustawić tylko w metodzie PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Ta właściwość pobiera lub ustawia maksymalną długość stanu stron w bajtach. Jeśli właściwość jest ustawiona na wartość dodatnią, stan widoku strony będzie być dzielone na wiele ukryte pola tak, aby nie przekraczała liczba bajtów określona. Jeśli właściwość jest liczbą ujemną, stan widoku nie będzie dzielony na fragmenty.

## <a name="pageadapter"></a>PageAdapter

Zwraca odwołanie do obiektu PageAdapter, który modyfikuje strony do przeglądarki.

## <a name="previouspage"></a>PreviousPage

Zwraca odwołanie do poprzedniej strony w przypadku metody Server.Transfer lub odświeżenie strony między stronami.

## <a name="skinid"></a>SkinID

Określa karnacji ASP.NET 2.0 do zastosowania do strony.

## <a name="stylesheettheme"></a>Element StyleSheetTheme

Ta właściwość pobiera lub ustawia arkusza stylów, który jest stosowany do strony.

## <a name="templatecontrol"></a>TemplateControl

Zwraca odwołanie do formantu zawierającego strony.

## <a name="theme"></a>Motyw

Pobiera lub ustawia nazwę motywu ASP.NET 2.0 zastosowana do strony. Ta wartość musi być ustawiony przed metodą PreInit.

## <a name="title"></a>Tytuł

Ta właściwość pobiera lub ustawia tytuł strony uzyskany w nagłówku strony.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Pobiera lub ustawia ViewStateEncryptionMode strony. Zobacz szczegółowe omówienie tej właściwości wcześniej w tym module.

## <a name="new-protected-properties-of-the-page-class"></a>Nowe właściwości chronionego klasy strony

Poniżej przedstawiono nowe właściwości chronionego klasy strony w programie ASP.NET 2.0.

## <a name="adapter"></a>Adapter

Zwraca odwołanie do ControlAdapter, renderujący stronę na urządzeniu, które go zażądało.

## <a name="asyncmode"></a>AsyncMode

Ta właściwość wskazuje, czy strona jest wykonywane asynchronicznie. Służy do użytku przez środowisko uruchomieniowe i nie są bezpośrednio w kodzie.

## <a name="clientidseparator"></a>ClientIDSeparator

Ta właściwość zwraca znak używany jako separator podczas tworzenia unikatowych klientów identyfikatorów dla formantów. Służy do użytku przez środowisko uruchomieniowe i nie są bezpośrednio w kodzie.

## <a name="pagestatepersister"></a>PageStatePersister

Ta właściwość zwraca obiekt PageStatePersister dla strony. Ta właściwość jest używana głównie przez deweloperów platformy ASP.NET formantu.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Ta właściwość Zwraca unikatowy suffic, dołączany do ścieżki pliku dla pamięci podręcznej przeglądarki. Wartość domyślna to \_ \_ufps = i 6-cyfrowy numer.

## <a name="new-public-methods-for-the-page-class"></a>Nowych metod publicznych klasy strony

Następujących metod publicznych dopiero zaczynasz korzystać z klasy strony w programie ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Ta metoda rejestruje delegatów obsługi zdarzeń do wykonywania asynchronicznych strony. Asynchroniczne stron zostały omówione w dalszej części tego modułu.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Zastosowanie właściwości w arkuszu stylów strony do strony.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Ta metoda istot zadanie asynchroniczne.

### <a name="getvalidators"></a>GetValidators

Zwraca kolekcję moduły weryfikacji dla grupy sprawdzania poprawności określonego lub domyślnego sprawdzania poprawności, jeśli nie jest określona.

## <a name="registerasynctask"></a>RegisterAsyncTask

Ta metoda rejestruje nowe zadanie asynchroniczne. Asynchroniczne strony są uwzględnione w dalszej części tego modułu.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Ta metoda określa, że ASP.NET musi zostać utrwalony stanu kontroli stron.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Ta metoda informuje ASP.NET, czy stan wyświetlania stron wymaga szyfrowania.

## <a name="resolveclienturl"></a>ResolveClientUrl

Zwraca względny adres URL, który może służyć do obsługi żądań klientów dla obrazów itd.

## <a name="setfocus"></a>Funkcja SetFocus

Ta metoda ustawi fokus do formantu, który jest określony, podczas ładowania strony do.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Ta metoda wyrejestruje formant, który jest przekazywany do niego jako już wymagające trwałość stanu formantu.

## <a name="changes-to-the-page-lifecycle"></a>Zmiany w cyklu życia strony

Cyklu życia strony w programie ASP.NET 2.0 nie zmieniła się znacznie, ale istnieją pewne nowych metod, które należy zwrócić uwagę. Cyklu życia strony ASP.NET 2.0 jest opisane poniżej.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (Nowość w programie ASP.NET 2.0)

Zdarzenie PreInit jest najwcześniejszym etapie w cyklu życia, w których deweloper może uzyskać dostęp. Dodanie to zdarzenie umożliwia programowo zmienić kompozycji ASP.NET 2.0, stron wzorcowych, dostępu do właściwości profilu platformy ASP.NET 2.0 itp. Jeśli jesteś w stanie odświeżania strony, jego zdawać sobie sprawę, że stan widoku nie ma jeszcze zastosowane do formantów w tym momencie w cyklu życia. W związku z tym jeśli dewelopera właściwości formantu zmieni się na tym etapie, zostanie prawdopodobnie on zastąpiony później w cyklu życia strony.

## <a name="init"></a>Init

Zdarzeniu Init nie zmienił się z ASP.NET 1.x. Jest to miejscu do odczytu lub zainicjować właściwości formantów na stronie. Ten etap, stron wzorcowych, kompozycje itp. już są stosowane do strony.

## <a name="initcomplete-new-in-20"></a>InitComplete (Nowość w wersji 2.0)

Zdarzenie InitComplete jest wywoływane po zakończeniu etapu inicjowania strony. W tym momencie cały cykl życia, dostępne kontrolki na stronie, ale nie została jeszcze wypełniona ich stan.

## <a name="preload-new-in-20"></a>Wstępne ładowanie (Nowość w wersji 2.0)

To zdarzenie jest wywoływane po zastosowaniu wszystkich danych odświeżania strony i wcześniejszy niż strony\_obciążenia.

## <a name="load"></a>Ładowanie

Zdarzenie obciążenia nie zmienił się z ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (Nowość w wersji 2.0)

Zdarzenie LoadComplete jest ostatnie zdarzenie w fazie ładowania stron. Na tym etapie wszystkie dane ogłaszania zwrotnego i stanu wyświetlania zostały zastosowane do strony.

## <a name="prerender"></a>PreRender

Jeśli chcesz viewstate prawidłowo utrzymania dla formantów, które są dodawane dynamicznie do strony, zdarzenie PreRender jest to ostatnia możliwość je dodać.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (Nowość w wersji 2.0)

Na etapie PreRenderComplete zostały dodane wszystkie formanty do strony i jest gotowy do renderowania strony. Zdarzenie PreRenderComplete jest ostatnie zdarzenie wywoływane przed zapisaniem stanu widoku strony.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (Nowość w wersji 2.0)

Zdarzenie SaveStateComplete jest wywoływana natychmiast po zapisaniu wszystkie strony viewstate i kontroli stanu. Jest to ostatnie zdarzenie przed wyświetleniem strony faktycznie w przeglądarce.

## <a name="render"></a>Renderowanie

Metoda renderowania nie zmienił się od ASP.NET 1.x. Jest to której zainicjowano HtmlTextWriter i realizacją strony w przeglądarce.

## <a name="cross-page-postback-in-aspnet-20"></a>Ogłaszania zwrotnego między stronami w programie ASP.NET 2.0

W programie ASP.NET 1.x, ogłaszania zwrotnego były wymagane do wysłania do tej samej stronie. Między stronami ogłaszania zwrotnego są niedozwolone. Platforma ASP.NET 2.0 dodaje możliwość ogłosić innej strony za pomocą interfejsu IButtonControl. Żadnego formantu, który implementuje interfejs IButtonControl nowe (przycisk, LinkButton i ImageButton oprócz innych firm formantów niestandardowych) można korzystać z tej nowej funkcji przez użycie atrybutu PostBackUrl. Poniższy kod przedstawia kontrolkę przycisku, który dokonuje ogłoszenia drugiej stronie.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Jeśli strona jest przesyłana z powrotem, który inicjuje ogłaszania zwrotnego strony jest dostępny za pośrednictwem właściwości PreviousPage na drugiej stronie. Ta funkcja jest implementowany za pośrednictwem nowego formularza sieci Web\_DoPostBackWithOptions po stronie klienta funkcji, która renderuje ASP.NET 2.0 do strony, gdy formant dokonuje ogłoszenia innej strony. Ta funkcja JavaScript jest dostarczana przez nowy program obsługi WebResource.axd, który emituje skryptu do klienta.

Poniższy klip wideo jest przewodnik odświeżania strony między stronami.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Otwórz wideo na pełnym ekranie](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Więcej informacji na temat ogłaszania zwrotnego strony między

### <a name="viewstate"></a>Stan widoku

Może zażądano samodzielnie już o co stanie się stan od pierwszej strony w scenariuszu odświeżania strony między stronami. Po wszystkich żadnego formantu, który nie implementuje element IPostBackDataHandler pozostanie stanu za pośrednictwem elementu viewstate, więc dostęp do właściwości tego formantu na drugiej stronie odświeżania strony między stronami, musisz mieć dostęp do stanu widoku strony. Platforma ASP.NET 2.0 zajmuje się ten scenariusz przy użyciu nowego pola ukryte na drugiej stronie o nazwie \_ \_PREVIOUSPAGE. \_ \_PREVIOUSPAGE pole formularza zawiera stan dla pierwszej strony, dzięki czemu mają dostęp do właściwości wszystkich kontrolek na drugiej stronie.

### <a name="circumventing-findcontrol"></a>Obejście metody FindControl

Przewodnik wideo odświeżania strony między stronami I użyć metody metody FindControl Aby pobrać odwołanie do formantu TextBox na pierwszej stronie. Ta metoda sprawdza się w przypadku tego celu, ale metody FindControl jest kosztowne i wymaga zapisywania dodatkowy kod. Na szczęście program ASP.NET 2.0 stanowi alternatywę dla metody FindControl dla tego celu, które będą działać w wielu scenariuszach. Dyrektywa PreviousPageType umożliwia zawierają jednoznacznie odwołanie do poprzedniej strony za pomocą TypeName lub VirtualPath atrybutu. Atrybut TypeName umożliwia określenie typu poprzedniej strony, gdy atrybut VirtualPath służy do odwoływania się do poprzedniej strony za pomocą ścieżki wirtualnej. Po ustawieniu dyrektywy PreviousPageType następnie musi ujawniać formantów, itp., do której chcesz zezwolić na dostęp za pomocą właściwości publiczne.

## <a name="lab-1-cross-page-postback"></a>Ogłaszania zwrotnego strony między laboratorium 1

W tym laboratorium utworzysz aplikację, która korzysta z nowych funkcji odświeżania strony między stronami ASP.NET 2.0.

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web programu ASP.NET.
2. Dodaj nowy formularz sieci Web o nazwie page2.aspx.
3. Otwórz plik Default.aspx w widoku projektu i Dodaj kontrolkę przycisku i kontrolki pola tekstowego. 

    1. Nadaj identyfikator formantu przycisku **SubmitButton** i identyfikator dla formantu pole tekstowe **UserName**.
    2. Ustaw właściwość PostBackUrl przycisku do page2.aspx.
4. Otwórz page2.aspx w widoku źródła.
5. Dodaj dyrektywy @ PreviousPageType, jak pokazano poniżej:
6. Dodaj następujący kod do strony\_obciążenia na page2.aspx kodem: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Skompiluj projekt, klikając w menu kompilacji podczas kompilacji.
8. Dodaj następujący kod do kodem Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Zmień stronę\_obciążenia w page2.aspx do następującego: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Skompiluj projekt.
11. Uruchom projekt.
12. Wprowadź nazwę w polu tekstowym i kliknij przycisk.
13. Co to jest wynik?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchroniczne stron w programie ASP.NET 2.0

Wiele rywalizacji problemów w programie ASP.NET są spowodowane opóźnienia połączeniami zewnętrznymi (np. wywołań usługi lub bazy danych w sieci Web), opóźnienia we/wy pliku itp. Po wysłaniu żądania w odniesieniu do aplikacji ASP.NET, program ASP.NET korzysta z jednego z jego wątków roboczych do obsłużenia tego żądania. To żądanie jest właścicielem tego wątku zakończenie żądanie i odpowiedź została wysłana. Platforma ASP.NET 2.0 ma na celu rozwiązanie problemów opóźnienia z tego typu problemów przez dodanie możliwość wykonywania stron asynchronicznie. Oznacza to, że wątku roboczego można uruchomić żądania i następnie przekazują wykonywania dodatkowych do innego wątku, w tym samym zwracać do puli wątków dostępnych szybko. Po ukończeniu operacji We/Wy pliku, wywołania bazy danych itp., nowego wątku są uzyskiwane z puli wątków, aby zakończyć żądania.

Pierwszym krokiem w podejmowaniu stronę asynchroniczne jest skonfigurowanie **Async** atrybutu dyrektywy strony w następujący sposób:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Ten atrybut informuje program ASP.NET do zaimplementowania IHttpAsyncHandler dla strony.

Następnym krokiem jest, aby wywołać metodę AddOnPreRenderCompleteAsync w punkcie w cyklu życia strony przed zdarzeniem PreRender. (Ta metoda jest zazwyczaj wywoływana na stronie\_obciążenia.) Metoda AddOnPreRenderCompleteAsync przyjmuje dwa parametry; BeginEventHandler i EndEventHandler. BeginEventHandler zwraca obiekt IAsyncResult, które są następnie przekazywane jako parametr EndEventHandler.

Obraz poniżej jest wskazówki żądania asynchroniczne strony.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Otwórz wideo na pełnym ekranie](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Na stronie asynchronicznej nie renderowania w przeglądarce dopiero po ukończeniu EndEventHandler. Bez wątpienia, ale niektórzy deweloperzy będzie traktować żądań asynchronicznych jako zbliżone do asynchronicznego wywołania zwrotne. Należy koniecznie należy pamiętać, że nie są one. Korzyści żądań asynchronicznych jest, czy pierwszy wątku roboczego może być zwracany w puli wątków do obsługi nowych żądań, skracając rywalizacji z powodu jest powiązany we/wy, np.


## <a name="script-callbacks-in-aspnet-20"></a>Wywołania zwrotne skryptu w programie ASP.NET 2.0

Sposób można zapobiec migotanie skojarzonych z wywołaniem zwrotnym zawsze sprawdzono deweloperów sieci Web. W programie ASP.NET 1.x, SmartNavigation był najbardziej typowa metoda uniknięcia migotanie, ale SmartNavigation przyczyną problemów dla deweloperów, niektóre ze względu na złożoność jej wdrożenia na kliencie. Platforma ASP.NET 2.0 rozwiązuje ten problem z wywołania zwrotne skryptu. Wywołania zwrotne skryptu wykorzystywać XMLHttp na wysyłanie żądań na serwerze sieci Web za pośrednictwem kodu JavaScript. Żądanie XMLHttp zwraca dane XML, który następnie może manipulować za pośrednictwem przeglądarki modelu DOM. Kod XMLHttp jest ukryty przez użytkownika przez nowy program obsługi WebResource.axd.

Istnieje kilka czynności, które są niezbędne w celu skonfigurowania wywołanie zwrotne skryptu w programie ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Krok 1: Implementowanie interfejsu modułu ICallbackEventHandler

Aby ASP.NET rozpoznał strony jako należący do wywołania zwrotnego skryptu musi implementować interfejs modułu ICallbackEventHandler. Można to zrobić w pliku CodeBehind w następujący sposób:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Można też to zrobić przy użyciu podobnych dyrektywy @ implementuje tak:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Dyrektywy @ Implements można skorzystać zazwyczaj, gdy przy użyciu kodu wbudowanego ASP.NET.

## <a name="step-2--call-getcallbackeventreference"></a>Krok 2: Wywołanie GetCallbackEventReference

Jak wspomniano wcześniej, wywołania XMLHttp jest hermetyzowany obsługi WebResource.axd. Podczas renderowania strony ASP.NET będzie Dodaj wywołanie do formularza sieci Web\_DoCallback, skrypt klientów zapewnianej przez WebResource.axd. Formularz sieci Web\_DoCallback funkcja zastępuje \_ \_doPostBack funkcja wywołania zwrotnego. Należy pamiętać, że \_ \_doPostBack programowo przesyła formularz na stronie. W przypadku wywołania zwrotnego, chcesz uniemożliwić odświeżenie strony, dlatego \_ \_doPostBack nie wystarczy.

> [!NOTE]
> \_\_doPostBack jest nadal renderowany do strony w przypadku wywołania zwrotnego skryptu klienta. Nie jest on jednak używany dla wywołania zwrotnego.


Argumenty dla formularza sieci Web\_DoCallback funkcji po stronie klienta są udostępniane za pośrednictwem funkcji po stronie serwera GetCallbackEventReference, która będzie wywoływana zwykle na stronie\_obciążenia. Typowe wywołania GetCallbackEventReference może wyglądać następująco:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> W takim przypadku cm jest wystąpieniem ClientScriptManager. Klasa ClientScriptManager zostanie omówiona w dalszej części tego modułu.


Istnieje kilka wersji przeciążone GetCallbackEventReference. W takim przypadku argumenty są następujące:

`this`

Odwołanie do formantu, gdy jest wywoływana GetCallbackEventReference. W takim przypadku jest samej strony.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argument ciągu, który zostanie przekazany z kodu po stronie klienta do zdarzenia po stronie serwera. W takim przypadku wiadomości błyskawicznych, przekazując wartość z listy rozwijanej o nazwie ddlCompany.

`ShowCompanyName`

Nazwa funkcji po stronie klienta, która przyjmuje wartość zwracaną (jako ciąg) ze zdarzenia wywołania zwrotnego po stronie serwera. Tę funkcję można wywołać tylko po pomyślnym zakończeniu operacji wywołania zwrotnego po stronie serwera. W związku z tym dla niezawodności, ogólnie zaleca się użycie przeciążonej wersji GetCallbackEventReference, który przyjmuje argument dodatkowy ciąg określający nazwę funkcji do wykonania w przypadku wystąpienia błędu po stronie klienta.

`null`

Ciąg reprezentujący funkcji po stronie klienta, która zainicjowane przed wywołania zwrotnego na serwerze. W takim przypadku nie żadnego skryptu więc argument ma wartość null.

`true`

Wartość logiczna określająca, czy należy przeprowadzić asynchronicznie metodę wywołania zwrotnego.

Wywołanie formularza sieci Web\_DoCallback na kliencie zostanie przekazany tych argumentów. W związku z tym, gdy ta strona jest renderowany na kliencie, ten kod będzie wyglądać w następujący sposób:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Należy zauważyć, że podpis funkcji na kliencie jest nieco inny. Funkcja klienta przekazuje ciągów 5 i wartość logiczną. Dodatkowy ciąg znaków (czyli null w powyższym przykładzie) zawiera funkcję po stronie klienta, która będzie obsługiwać błędy zwrotne po stronie serwera.

## <a name="step-3--hook-the-client-side-control-event"></a>Krok 3: Utworzenie punktu zaczepienia zdarzenia formantu po stronie klienta

Należy zauważyć, że wartość zwracana GetCallbackEventReference powyżej został przypisany do zmiennej ciągu. Ten ciąg jest używany do utworzenie punktu zaczepienia zdarzenie po stronie klienta dla formantu, który inicjuje wywołania zwrotnego. W tym przykładzie, wywołanie zwrotne jest inicjowane przez listy rozwijanej na stronie tak, chcę utworzenie punktu zaczepienia *OnChange* zdarzeń.

Aby przyłączyć zdarzeń po stronie klienta, po prostu Dodaj program obsługi do znacznika po stronie klienta w następujący sposób:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Odwołania, który *cbRef* jest wartość zwrotna z wywołania GetCallbackEventReference. Zawiera wywołanie formularza sieci Web\_DoCallback, który został przedstawionych powyżej.

## <a name="step-4--register-the-client-side-script"></a>Krok 4: Zarejestruj skryptu po stronie klienta

Odwołania, które wywołanie GetCallbackEventReference określone, czy skrypt po stronie klienta jest nazywany **ShowCompanyName** może być wykonywana w przypadku pomyślnego wywołania zwrotnego po stronie serwera. Czy skrypt musi zostać dodany do strony przy użyciu wystąpienia ClientScriptManager. (W klasie ClientScriptManager będzie dicussed w dalszej części tego modułu). Tak zrobisz tego typu:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Krok 5: Wywoływać metod interfejsu modułu ICallbackEventHandler

Modułu ICallbackEventHandler zawiera dwie metody, które są potrzebne do zaimplementowania w kodzie. Są one **RaiseCallbackEvent** i **GetCallbackEvent**.

**RaiseCallbackEvent** jako argument ciąg znaków i nie zwraca żadnego. Argument ciągu jest przekazywany z wywołania po stronie klienta do formularza sieci Web\_DoCallback. W takim przypadku ta wartość jest *wartość* atrybutu o nazwie ddlCompany listy rozwijanej. W metodzie RaiseCallbackEvent powinna zostać umieszczona kodu po stronie serwera. Na przykład jeśli wywołanie zwrotne jest WebRequest przed zasób zewnętrzny, ten kod powinna zostać umieszczona w RaiseCallbackEvent.

**GetCallbackEvent** jest odpowiedzialna za przetwarzanie zwracany wywołania zwrotnego do klienta. Go nie przyjmuje żadnych argumentów i zwraca wartość typu ciąg. Ciąg, który zwraca zostaną przekazane jako argument do funkcji po stronie klienta, w tym przypadku *ShowCompanyName*.

Po wykonaniu powyższych czynności można przystąpić do wykonania wywołania zwrotnego skryptu w programie ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Otwórz wideo na pełnym ekranie](the-asp-net-2-0-page-model/_static/callback1.wmv)


Wywołania zwrotne skryptu w programie ASP.NET są obsługiwane w dowolnej przeglądarki obsługującej wprowadzania wywołania XMLHttp. Która obejmuje wszystkie nowoczesne przeglądarki w użyciu dzisiaj. Program Internet Explorer używa obiektu XMLHttp ActiveX, podczas gdy obiekt wewnętrzny XMLHttp innych nowoczesnymi przeglądarkami (w tym nadchodzących IE 7). Aby określić programowo, jeśli przeglądarka obsługuje wywołań zwrotnych, możesz użyć **Request.Browser.SupportCallback** właściwości. Ta właściwość będzie zwracać **true** Jeśli klienta obsługuje wywołania zwrotne skryptu.

## <a name="working-with-client-script-in-aspnet-20"></a>Praca z skrypt po stronie klienta w programie ASP.NET 2.0

Skrypty klienta w programie ASP.NET 2.0 są zarządzane przez użycie klasy ClientScriptManager. Klasa ClientScriptManager przechowuje informacje o skrypty klienta przy użyciu typu i nazwy. Zapobiega to programowo wstawiane na stronie więcej niż jeden raz przez tego samego skryptu.

> [!NOTE]
> Po skrypt został pomyślnie zarejestrowany na stronie, kolejne próby zarejestrowania tego samego skryptu, po prostu spowoduje skryptu nie jest zarejestrowany na drugim. Żadnych zduplikowanych skryptów są dodawane i wystąpi żaden wyjątek. Aby uniknąć niepotrzebnych obliczeń, istnieją metody, których można użyć w celu ustalenia, czy skrypt jest już zarejestrowany, tak aby nie należy go zarejestrować więcej niż raz.


Metody ClientScriptManager powinny być znane wszystkie bieżące deweloperów platformy ASP.NET:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Ta metoda dodaje skrypt do góry renderowanej strony. Jest to przydatne w przypadku dodawania funkcji, które będą wywoływane jawnie na kliencie.

Istnieją dwie wersje przeciążenia tej metody. Trzy z czterech argumentów występują między nimi. Są to:

`type (string)`

***Typu*** argument określa typ skryptu. Zazwyczaj jest dobrym pomysłem jest typ strony, (this. GetType()) dla typu.

`key (string)`

***Klucza*** argument jest zdefiniowane przez użytkownika klucza dla skryptu. Powinny to być unikatowe dla każdego skryptu. Jeśli próbujesz dodać skrypt za pomocą tego samego klucza i typ skryptu już dodany, nie można dodać.

`script (string)`

***Skryptu*** argument jest ciąg zawierający faktyczny skrypt do dodania. Zaleca się używać StringBuilder utworzyć skrypt, a następnie użyć metodę ToString() na StringBuilder można przypisać ***skryptu*** argumentu.

Użycie przeciążonej RegisterClientScriptBlock, który przyjmuje tylko trzech argumentów, musi zawierać elementy skryptu (&lt;skryptu&gt; i &lt;/script&gt;) w skrypcie.

Możesz użyć przeciążenia RegisterClientScriptBlock, który pobiera czwarty argument. Czwarty argument jest wartość logiczna określająca, czy ASP.NET, należy dodać elementy skryptu dla Ciebie. Jeśli ten argument jest **true**, skrypt nie może zawierać elementy skryptu jawnie.

Metoda IsClientScriptBlockRegistered ustalenie, jeśli skrypt został już zarejestrowany. Dzięki temu można uniknąć próba ponownego zarejestrowania skrypt, który został już zarejestrowany.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (Nowość w wersji 2.0)

RegisterClientScriptInclude tag tworzy blok skryptu, który stanowi łącze do zewnętrznego pliku skryptu. Ma on dwa przeciążenia. Wyższy klucza i adres URL. Drugi dodaje trzeci argument określenie typu.

Na przykład następujący kod generuje blok skryptu zawierającego łącze do jsfunctions.js w głównym folderze skryptów aplikacji:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Ten kod tworzy następujący kod na renderowanej stronie:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Blok skryptu jest renderowany w dolnej części strony.


Metoda IsClientScriptIncludeRegistered ustalenie, jeśli skrypt został już zarejestrowany. Dzięki temu można uniknąć próba ponownego zarejestrowania skryptu.

## <a name="registerstartupscript"></a>RegisterStartupScript

Metoda RegisterStartupScript przyjmuje te same argumenty co metoda RegisterClientScriptBlock. Skrypt w zarejestrowany RegisterStartupScript wykonuje po pobraniu strony, ale przed zdarzeniem OnLoad po stronie klienta. W 1.X, skrypty w zarejestrowany RegisterStartupScript zostały umieszczone bezpośrednio przed zamknięciem &lt;/form&gt; tagów, podczas gdy skryptów w zarejestrowany RegisterClientScriptBlock były umieszczane bezpośrednio po otwarciu &lt;formularza&gt; tagu. W programie ASP.NET 2.0, zarówno są umieszczane bezpośrednio przed tagiem zamykającym &lt;/form&gt; tagu.

> [!NOTE]
> Jeśli zarejestrowane przez funkcję RegisterStartupScript, ta funkcja nie zostanie wykonany, dopóki nie można jawnie wywołać w kodzie po stronie klienta.


Użyj metody IsStartupScriptRegistered, aby ustalić, jeśli skrypt został już zarejestrowany i uniknąć próba ponownego zarejestrowania skryptu.

## <a name="other-clientscriptmanager-methods"></a>Inne metody ClientScriptManager

Oto niektóre przydatne metody klasy ClientScriptManager.


|  <strong>GetCallbackEventReference</strong>   |                                                 Zobacz wywołania zwrotne skryptu wcześniej w tym module.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Pobiera odwołanie JavaScript (javascript:&lt;wywołać&gt;) można post z zdarzeń po stronie klienta.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Pobiera ciąg, który może służyć do zainicjowania post przez klienta.                                    |
|      <strong>GetWebResourceUrl</strong>       | Zwraca adres URL do zasobu, który jest osadzony w zestawie. Należy używać w połączeniu z <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Rejestruje zasobu sieci Web ze stroną. Są to zasoby osadzone w zestawie i obsługiwane przez nowy program obsługi WebResource.axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Rejestruje ukryte pole formularza ze stroną.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Rejestruje kod po stronie klienta, który jest wykonywany po przesłaniu formularza HTML.                                   |

