---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Modelu programu ASP.NET 2.0 strony | Dokumentacja firmy Microsoft
author: microsoft
description: 'W programie ASP.NET: 1.x, deweloperzy ma wybór między wbudowany model kodu i model kodu związanego z kodem. Związane z kodem można zaimplementować przy użyciu Src attr...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379890"
---
<a name="the-aspnet-20-page-model"></a>Modelu programu ASP.NET 2.0 strony
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W programie ASP.NET: 1.x, deweloperzy ma wybór między wbudowany model kodu i model kodu związanego z kodem. Związane z kodem można zaimplementować przy użyciu atrybutu Src lub atrybut CodeBehind @Page dyrektywy. W programie ASP.NET 2.0 deweloperzy nadal mają możliwość wyboru między wbudowany kod i związane z kodem, ale zostały znaczne ulepszenia do modelu związanym z kodem.


W programie ASP.NET: 1.x, deweloperzy ma wybór między wbudowany model kodu i model kodu związanego z kodem. Związane z kodem można zaimplementować przy użyciu atrybutu Src lub atrybut CodeBehind @Page dyrektywy. W programie ASP.NET 2.0 deweloperzy nadal mają możliwość wyboru między wbudowany kod i związane z kodem, ale zostały znaczne ulepszenia do modelu związanym z kodem.

## <a name="improvements-in-the-code-behind-model"></a>Ulepszenia w modelu związanym z kodem

Aby można było w pełni zapoznać się ze zmianami w modelu związanym z kodem w programie ASP.NET 2.0, jego najlepiej, aby szybko zapoznać się z modelu, ponieważ istniał w programie ASP.NET: 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Model związanym z kodem w programie ASP.NET: 1.x

W programie ASP.NET: 1.x, model związanym z kodem składa się z plikiem ASPX (formularz sieci Web) i pliku związanego z kodem, kodem programowania. Dwa pliki zostały połączone za pomocą @Page dyrektywy w pliku ASPX. Każdej kontrolki na stronie ASPX miały odpowiednich deklaracji w pliku związanym z kodem jako zmienna wystąpienia. Plik związany z kodem również zawiera kod dla powiązania zdarzeń i wygenerowanego kodu niezbędne do projektanta programu Visual Studio. Ten model jest dość dobrze działa, ale ponieważ wymaganego przez każdy element ASP.NET strony ASPX odpowiedni kod w pliku związanym z kodem wystąpił bez separacji true kodu i zawartości. Na przykład jeśli projektant dodany nowy formant serwera do pliku ASPX poza programem Visual Studio IDE, aplikacja zaburzyłaby z powodu braku deklarację dla tego formantu w pliku związanym z kodem.

## <a name="the-code-behind-model-in-aspnet-20"></a>Model związanym z kodem w programie ASP.NET 2.0

Program ASP.NET 2.0 znacznie poprawia przy tym modelu. W programie ASP.NET 2.0 związanym z kodem jest implementowany przy użyciu nowego *klas częściowych* dostarczane w programie ASP.NET 2.0. Klasy związane z kodem w programie ASP.NET 2.0 jest definiowane jako klasę częściową, co oznacza, że zawiera on tylko część definicji klasy. Pozostała część definicji klasy jest generowana dynamicznie przy ASP.NET 2.0 przy użyciu strony ASPX, w czasie wykonywania lub witryny sieci Web są wstępnie skompilowane. Łącze między plikiem CodeBehind i strony ASPX nadal zostanie nawiązane, przy użyciu dyrektywy @ Page. Jednak zamiast atrybutu CodeBehind lub Src ASP.NET 2.0 używa teraz atrybut CodeFile. Atrybut Inherits umożliwia również określić nazwę klasy dla strony.

Typowe dyrektywy @ Page może wyglądać następująco:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Definicję typowej klasy w pliku związanym z kodem programu ASP.NET 2.0 może wyglądać następująco:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# i Visual Basic są tylko języki zarządzane, obsługujące klas częściowych. W związku z tym deweloperzy korzystający z języka J# nie będzie do korzystania z modelu związanym z kodem w programie ASP.NET 2.0.


Nowy model zwiększa modelu związanym z kodem, ponieważ deweloperzy będą teraz mieć plików kodu, które zawierają tylko kod, który zostały one utworzone. Zapewnia także true separacja kodu i zawartości, ponieważ nie ma żadnych wystąpienia deklaracji zmiennych w pliku związanym z kodem.

> [!NOTE]
> Częściowe klasy dla strony ASPX jest, gdzie odbywa się powiązanie zdarzeń, deweloperów programu Visual Basic można weź pod uwagę wzrost niewielkim wzroście wydajności przy użyciu słowa kluczowego uchwytów w związanym z kodem można powiązać zdarzenia. C# ma nie równoważne słowa kluczowego.


## <a name="new--page-directive-attributes"></a>Nowe atrybuty @ Page — dyrektywa

Program ASP.NET 2.0 dodaje wiele nowych atrybutów do dyrektywy @ Page. Następujące atrybuty są nowością w programie ASP.NET 2.0.

## <a name="async"></a>Async

Atrybut Async pozwala skonfigurować stronę, aby być wykonywany asynchronicznie. Obejmuje również asynchronicznego stron w dalszej części tego modułu.

## <a name="asynctimeout"></a>AsyncTimeout

Określony limit czasu dla asynchronicznego stron. Wartość domyślna to 45 sekund.

## <a name="codefile"></a>CodeFile

Atrybut CodeFile jest zastępczych dla atrybutu pliku CodeBehind w programie Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Atrybut CodeFileBaseClass jest używany w przypadkach, w którym ma się wiele stron pochodzić z jednej klasy bazowej. Ze względu na implementację klas częściowych w ASP.NET, bez tego atrybutu klasy bazowej, korzystającą z udostępnionych wspólnych pól w celu odwołują się do formantów zadeklarowanych w strony ASPX nie będzie działać poprawnie ponieważ ASP. Aparat kompilacji sieci automatycznie utworzy nowych elementów członkowskich na podstawie formantów na stronie. W związku z tym, jeśli mają wspólną klasę bazową dla dwóch lub więcej stron w programie ASP.NET: należy zdefiniować Określ klasę bazową w atrybucie CodeFileBaseClass, a następnie dziedziczyć po każdej klasy strony tej klasy bazowej. Atrybut CodeFile jest wymagany również w przypadku, gdy ten atrybut jest używany.

## <a name="compilationmode"></a>CompilationMode

Ten atrybut umożliwia ustawienie właściwości CompilationMode strony ASPX. Właściwość CompilationMode jest wyliczeniem, zawierające wartości **zawsze**, **automatycznie**, i **nigdy**. Wartość domyślna to **zawsze**. **Automatycznie** ustawienie uniemożliwi ASP.NET dynamicznie kompilowania strony, jeśli jest to możliwe. Wyłączanie stron z kompilacji dynamicznej zwiększa wydajność. Jednakże, jeśli strona, która jest wykluczona zawiera ten kod, który musi być skompilowany, zostanie zwrócony błąd podczas przeglądania strony.

## <a name="enableeventvalidation"></a>EnableEventValidation

Ten atrybut określa, czy zdarzenia odświeżenie strony i wywołań zwrotnych do sprawdzania poprawności. Gdy ta opcja jest włączona, argumenty odświeżania lub zdarzenia wywołania zwrotnego jest sprawdzany w celu upewnij się, że pochodzą formant serwera, który pierwotnie ich renderowania.

## <a name="enabletheming"></a>EnableTheming

Ten atrybut określa, czy motywów programu ASP.NET są używane na stronie. Wartość domyślna to **false**. Motywy platformy ASP.NET zostały omówione w [10 modułu](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Ten atrybut określa, czy podczas kompilacji należy dodać wyrażenia pragma wiersza. Wyrażenia pragma wiersza są opcje używane przez debugery, aby oznaczyć określonych sekcji kodu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Ten atrybut określa, czy języka JavaScript są wstrzykiwane do strony, aby zachować położenie przewijania między ogłoszeniami wstecznymi. Ten atrybut jest **false** domyślnie.

Jeśli ten atrybut jest **true**, ASP.NET spowoduje to dodanie &lt;skryptu&gt; bloku na odświeżenie strony, która wygląda w następujący sposób:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Należy pamiętać, że src dla tego bloku skryptu WebResource.axd. Ten zasób nie jest ścieżką fizyczną. Zleconą ten skrypt ASP.NET dynamicznie tworzy skrypt.

### <a name="masterpagefile"></a>MasterPageFile

Ten atrybut określa plik strony głównej dla bieżącej strony. Ścieżka może być względna lub bezwzględna. Strony wzorcowe są objęte [modułu 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Ten atrybut umożliwia zastąpienie właściwości wygląd interfejsu użytkownika zdefiniowane przez motywów programu ASP.NET 2.0. Motywy są objęte [10 modułu](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Motyw

Określa motywu strony. Jeśli nie określono wartości dla atrybutu StyleSheetTheme, atrybut motywu zastępuje wszystkie style, które dotyczą kontrolki na stronie.

## <a name="title"></a>Tytuł

Ustawia tytuł strony. Wartość określona w tym miejscu będą wyświetlane w &lt;tytuł&gt; element renderowanej strony.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ustawia wartość wyliczenia ViewStateEncryptionMode. Dostępne wartości to **zawsze**, **automatycznie**, i **nigdy**. Wartość domyślna to **automatycznie**. Gdy ten atrybut jest ustawiony na wartość **automatycznie**, stan widoku jest szyfrowany jest kontrolki go przez wywołanie metody **RegisterRequiresViewStateEncryption** metody.

## <a name="setting-public-property-values-via-the--page-directive"></a>Ustawianie wartości właściwości publicznej za pośrednictwem @ Page — dyrektywa

Kolejna nowa możliwość dyrektywy @ Page w programie ASP.NET 2.0 jest możliwość ustawiania początkowa wartość właściwości publiczne klasy bazowej. Załóżmy na przykład, że masz właściwość publiczną o nazwie **SomeText** w swojej klasy bazowej i d podoba Ci się zostać zainicjowana do **Hello** po załadowaniu strony. Można to zrobić, po prostu ustawiając wartość w dyrektywy @ Page w następujący sposób:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** atrybutu dyrektywy @ Page ustawia początkowe wartości właściwości SomeText w klasie bazowej, aby *Hello!*. Poniższy klip wideo jest wskazówki dotyczące ustawiania wartości początkowej właściwość publiczna w klasie bazowej, przy użyciu dyrektywy @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Otwórz wideo pełnego ekranu](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nowe właściwości publicznej klasy strony

Następujące właściwości publiczne są nowością w programie ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Zwraca ścieżkę względem aplikacji do strony lub formant. Na przykład strony znajduje się w http://app/folder/page.aspx, zwraca właściwości ~ / folder /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Zwraca ścieżkę względną katalogu wirtualnego do strona lub kontrolka. Na przykład w przypadku strony znajduje się w http://app/folder/page.aspx, zwraca właściwości ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Pobiera lub ustawia limit czasu, używane do obsługi asynchronicznego strony. (Asynchronicznego strony będzie uwzględnione w dalszej części tego modułu).

## <a name="clientquerystring"></a>ClientQueryString

Właściwość tylko do odczytu, która zwraca część ciągu zapytania żądanego adresu URL. Ta wartość jest zakodowane w adresie URL. Metoda UrlDecode klasy HttpServerUtility służy do zdekodowania go.

## <a name="clientscript"></a>ClientScript

Ta właściwość zwraca obiekt ClientScriptManager, który może służyć do zarządzania emisji ASP.NETs skryptu po stronie klienta. (Klasy ClientScriptManager zostało opisane w dalszej części tego modułu).

## <a name="enableeventvalidation"></a>EnableEventValidation

Ta właściwość określa, czy sprawdzanie poprawności zdarzenia jest włączony dla zdarzeń odświeżenie strony i wywołania zwrotnego. Po włączeniu argumentów odświeżania lub zdarzenia wywołania zwrotnego są weryfikowane i upewnij się, że pochodzą formant serwera, który pierwotnie ich renderowania.

## <a name="enabletheming"></a>EnableTheming

Tej właściwości pobiera lub ustawia wartość logiczna określająca, czy motywów programu ASP.NET 2.0 jest stosowany do strony.

## <a name="form"></a>Formularz

Ta właściwość zwraca formularza HTML strony ASPX jako obiekt HtmlForm.

## <a name="header"></a>nagłówek

Ta właściwość zwraca odwołanie do obiektu HtmlHead, która zawiera nagłówek strony. Zwrócony obiekt HtmlHead służy do pobierania/ustawiania, arkusze stylów, tagów Meta itp.

## <a name="idseparator"></a>IdSeparator

Ta właściwość tylko do odczytu pobiera lub ustawia znak używany do oddzielania identyfikatory kontroli, kompilując ASP.NET jest unikatowy identyfikator dla formantów na stronie. Nie jest on przeznaczony do użycia bezpośrednio w kodzie.

## <a name="isasync"></a>IsAsync

Ta właściwość umożliwia asynchroniczne stron. Strony asynchroniczne zostały omówione w dalszej części tego modułu.

## <a name="iscallback"></a>IsCallback

Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest wynikiem wywołania zwrotnego. Oddzwanianie zostały omówione w dalszej części tego modułu.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest częścią ogłaszania zwrotnego między stronami. Ogłaszania zwrotnego między stronami zostały omówione w dalszej części tego modułu.

## <a name="items"></a>Elementy

Zwraca odwołanie do wystąpienia element IDictionary zawierający wszystkie obiekty przechowywane w kontekście strony. Można dodać elementy do tego obiektu IDictionary i będą one dostępne w okresie istnienia w kontekście.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Ta właściwość określa, czy program ASP.NET emituje języka JavaScript, który utrzymuje stron przewiń pozycji w przeglądarce, po wystąpieniu ogłaszania zwrotnego. (Szczegóły tej właściwości zostały omówione wcześniej w tym module).

## <a name="master"></a>Wzorzec

Ta właściwość tylko do odczytu zwraca odwołanie do wystąpienia na nich dla strony, do którego zastosowano strony wzorcowej.

## <a name="masterpagefile"></a>MasterPageFile

Pobiera lub ustawia nazwę pliku strony wzorcowej strony. Tę właściwość można ustawić tylko w metodzie PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Tej właściwości pobiera lub ustawia maksymalną długość stanu stron w bajtach. Jeśli właściwość jest ustawiona na wartość dodatnią, stan widoku strony będzie być dzielone na wiele ukryte pola tak, aby nie przekracza rozmiaru liczbę bajtów określoną. Jeśli właściwość jest liczbą ujemną, stan widoku nie można podzielić na fragmenty.

## <a name="pageadapter"></a>PageAdapter

Zwraca odwołanie do obiektu PageAdapter, który modyfikuje strony do przeglądarki.

## <a name="previouspage"></a>PreviousPage

Zwraca odwołanie do poprzedniej strony w przypadku Server.Transfer lub ogłaszania zwrotnego między stronami.

## <a name="skinid"></a>SkinID

Określa skórki ASP.NET 2.0 do zastosowania do strony.

## <a name="stylesheettheme"></a>StyleSheetTheme

Tej właściwości pobiera lub ustawia arkusza stylów, która jest stosowana do strony.

## <a name="templatecontrol"></a>TemplateControl

Zwraca odwołanie do formantu zawierającego dla strony.

## <a name="theme"></a>Motyw

Pobiera lub ustawia nazwę motywu ASP.NET 2.0 zastosowana do strony. Ta wartość musi być ustawiony przed metoda PreInit.

## <a name="title"></a>Tytuł

Tej właściwości pobiera lub ustawia tytuł strony, uzyskany w nagłówku strony.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Pobiera lub ustawia ViewStateEncryptionMode strony. Zobacz szczegółowe omówienie tej właściwości we wcześniejszej części tego modułu.

## <a name="new-protected-properties-of-the-page-class"></a>Nowe właściwości chronionego klasy strony

Poniżej przedstawiono nowe właściwości chronionego klasy strony programu ASP.NET 2.0.

## <a name="adapter"></a>Adapter

Zwraca odwołanie do ControlAdapter, który powoduje wyświetlenie strony na urządzeniu żądane go.

## <a name="asyncmode"></a>AsyncMode

Ta właściwość wskazuje, czy strony są przetwarzane asynchronicznie. Jest przeznaczony do użytku przez środowisko wykonawcze, a nie bezpośrednio w kodzie.

## <a name="clientidseparator"></a>ClientIDSeparator

Ta właściwość zwraca znak używany jako separator, podczas tworzenia unikatowych klientów identyfikatory dla formantów. Jest przeznaczony do użytku przez środowisko wykonawcze, a nie bezpośrednio w kodzie.

## <a name="pagestatepersister"></a>PageStatePersister

Ta właściwość zwraca obiekt PageStatePersister dla strony. Ta właściwość jest głównie używana przez deweloperów kontroli platformy ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Ta właściwość Zwraca unikatowy suffic, która jest dołączana do ścieżki pliku do pamięci podręcznej przeglądarki. Wartość domyślna to \_ \_ufps = 6-cyfrowy numer.

## <a name="new-public-methods-for-the-page-class"></a>Nowych metod publicznych klasy strony

Jesteś nowym użytkownikiem klasy strony programu ASP.NET 2.0 są następujące metody publiczne.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Ta metoda rejestruje delegatów obsługi zdarzeń do wykonania asynchronicznej strony. Strony asynchroniczne zostały omówione w dalszej części tego modułu.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Zastosowanie właściwości w arkuszu stylów strony do strony.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Ta metoda żywych zadanie asynchroniczne.

### <a name="getvalidators"></a>GetValidators

Zwraca kolekcję moduły weryfikacji dla grupy sprawdzania poprawności określonego lub sprawdzania poprawności domyślnej grupy, jeśli nie określono.

## <a name="registerasynctask"></a>RegisterAsyncTask

Ta metoda rejestruje nowe zadanie asynchroniczne. Strony asynchroniczne zostały omówione w dalszej części tego modułu.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Ta metoda informuje ASP.NET stan formantu strony musi zostać utrwalone.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Ta metoda informuje ASP.NET, czy stan wyświetlania stron wymaga szyfrowania.

## <a name="resolveclienturl"></a>ResolveClientUrl

Zwraca względny adres URL, który może służyć do obsługi żądań klientów, obrazy itp.

## <a name="setfocus"></a>Funkcja SetFocus

Ta metoda ustawi fokus do formantu, który jest określana podczas wczytywania strony.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Ta metoda wyrejestruje formant, który jest przekazywany do niego jako już konieczności utrwalanie stanu kontrolki.

## <a name="changes-to-the-page-lifecycle"></a>Zmiany w cyklu życia strony

Cyklu życia strony programu ASP.NET 2.0 nie zmieniło się drastycznie, ale istnieje kilka nowych metod, których należy wiedzieć. Cyklu życia strony ASP.NET 2.0 jest opisane poniżej.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (Nowość w ASP.NET 2.0)

Zdarzenie PreInit jest najwcześniejszym etapie w cyklu życia, których deweloper może uzyskać dostęp. Dodanie to zdarzenie umożliwia programowe Zmienianie motywów programu ASP.NET 2.0, strony wzorcowe, uzyskiwanie dostępu do właściwości profilu platformy ASP.NET 2.0 itp. Jeśli jesteś w stanie ogłaszania wstecznego, istotne jest, aby należy pamiętać, że stan widoku nie ma jeszcze zastosowane do kontrolek na tym etapie w cyklu życia. W związku z tym jeśli deweloper zmienia właściwość kontrolki na tym etapie, zostanie prawdopodobnie on zastąpiony w dalszej części cyklu życia strony.

## <a name="init"></a>Init

Zdarzenie inicjowania nie zmienił się z ASP.NET 1.x. Jest to miejscu do odczytu lub zainicjować właściwości formantów na stronie. Na tym etapie, strony wzorcowe, motywy itp., są już stosowane do strony.

## <a name="initcomplete-new-in-20"></a>InitComplete (Nowość w wersji 2.0)

Zdarzenie InitComplete jest wywoływane na końcu etapie inicjowania strony. Na tym etapie w cyklu życia, możesz uzyskać dostęp formantów na stronie, ale nie została jeszcze wypełniona ich stanu.

## <a name="preload-new-in-20"></a>Wstępne ładowanie (Nowość w wersji 2.0)

To zdarzenie jest wywoływane po zastosowaniu wszystkich danych ogłaszania wstecznego i tuż przed strony\_obciążenia.

## <a name="load"></a>Ładowanie

Załadowane zdarzenie nie zmienił się z ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (Nowość w wersji 2.0)

Zdarzenie LoadComplete jest ostatnie zdarzenie w fazie ładowania stron. Na tym etapie wszystkie dane ogłaszania zwrotnego i stanu wyświetlania zostały doliczone do strony.

## <a name="prerender"></a>PreRender

Jeśli chcesz uzyskać viewstate prawidłowo utrzymanie dla formantów, które są dodawane dynamicznie do strony, zdarzenie PreRender jest to ostatnia możliwość je dodać.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (Nowość w wersji 2.0)

Na etapie PreRenderComplete wszystkich kontrolek zostały dodane do strony, a strona jest gotowy do renderowania. Zdarzenie PreRenderComplete jest ostatnie zdarzenie zgłoszone przed zapisaniem stanu widoku strony.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (Nowość w wersji 2.0)

Zdarzenie SaveStateComplete nosi nazwę natychmiast, po zapisaniu wszystkich stanów strony viewstate i kontroli. Jest to ostatnie zdarzenie przed wyświetleniem strony faktycznie w przeglądarce.

## <a name="render"></a>Renderowanie

Metody renderowania nie zmienił się od ASP.NET 1.x. Jest to, gdzie HtmlTextWriter jest inicjowany i wyrenderowaniu strony do przeglądarki.

## <a name="cross-page-postback-in-aspnet-20"></a>Ogłaszania zwrotnego między stronami, w programie ASP.NET 2.0

W programie ASP.NET: 1.x, ogłaszania zwrotnego były wymagane do wysłania do tej samej stronie. Ogłaszania zwrotnego między stronami nie były dozwolone. Program ASP.NET 2.0 dodaje możliwość Opublikuj innej strony za pomocą interfejsu IButtonControl. Żadnego formantu, który implementuje nowy interfejs IButtonControl (Button, element LinkButton i ImageButton oprócz niestandardowe formanty innych firm) mogą korzystać z tej nowej funkcji przez użycie atrybutu PostBackUrl. Poniższy kod przedstawia formant przycisku, który publikuje drugiej strony.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Gdy strona jest przesyłana z powrotem, Strona która inicjuje odświeżenie strony są dostępne przez właściwość PreviousPage na drugiej stronie. Ta funkcja jest zaimplementowana za pomocą nowego formularza sieci Web\_DoPostBackWithOptions po stronie klienta funkcji, która ASP.NET 2.0 renderuje do strony, gdy formant publikuje do innej strony. Ta funkcja JavaScript jest zapewniana przez nowy program obsługi WebResource.axd, który emituje skryptu do klienta.

Poniższy klip wideo jest przewodnik ogłaszania zwrotnego między stronami.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Otwórz wideo pełnego ekranu](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Szczegółowe informacje na temat ogłaszania zwrotnego między stronami

### <a name="viewstate"></a>Stan widoku

Może mieć monit samodzielnie już o co się dzieje z viewstate od pierwszej strony w przypadku zwrotu między stronami. Gdy wszystkie żadnego formantu, który nie implementuje element IPostBackDataHandler utrwali stanie za pośrednictwem stanu widoku, dlatego mają mieć dostęp do właściwości tej kontrolki na drugiej stronie ogłaszania zwrotnego między stronami, musi mieć dostęp do stanu widoku strony. Program ASP.NET 2.0 zajmuje się ten scenariusz przy użyciu nowego pola ukryte na drugiej stronie o nazwie \_ \_PREVIOUSPAGE. \_ \_PREVIOUSPAGE pole formularza zawiera viewstate dla pierwszej strony, dzięki czemu mają dostęp do właściwości wszystkich kontrolek na drugiej stronie.

### <a name="circumventing-findcontrol"></a>Obejście FindControl

Przewodnik wideo ogłaszania zwrotnego między stronami czy używana metoda FindControl można pobrać odwołania do kontrolki pola tekstowego na pierwszej stronie. Ta metoda sprawdza się w przypadku tego celu, ale FindControl jest kosztowne i wymaga on tworzenia dodatkowego kodu. Na szczęście ASP.NET 2.0 stanowi alternatywę dla FindControl w tym celu, który będzie działać w wielu scenariuszach. Dyrektywa PreviousPageType umożliwia silnie typizowane odwoływałby się do poprzedniej strony za pomocą TypeName lub atrybut VirtualPath. Atrybut TypeName umożliwia określenie typu poprzedniej strony, gdy atrybut VirtualPath zezwala na odnoszenie się do poprzedniej strony, przy użyciu ścieżki wirtualnej. Po ustawieniu dyrektywy PreviousPageType, należy następnie udostępnić formantów, itp., do której chcesz zezwolić na dostęp przy użyciu właściwości publiczne.

## <a name="lab-1-cross-page-postback"></a>Ogłaszania zwrotnego między stronami laboratorium 1

W tym środowisku laboratoryjnym utworzysz aplikację, która korzysta z nowych funkcji zwrotu między stronami ASP.NET 2.0.

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web platformy ASP.NET.
2. Dodaj nowy formularz sieci Web o nazwie page2.aspx.
3. Otwórz Default.aspx w widoku Projekt i Dodaj formant przycisku i formant pola tekstowego. 

    1. Nadaj formant przycisku o identyfikatorze **SubmitButton** i pole tekstowe kontrolować identyfikator **UserName**.
    2. Ustaw właściwość PostBackUrl przycisku page2.aspx.
4. Otwórz page2.aspx w widoku źródła.
5. Dodaj dyrektywy @ PreviousPageType, jak pokazano poniżej:
6. Dodaj następujący kod do strony\_obciążenia firmy page2.aspx związane z kodem: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Skompiluj projekt, klikając menu kompilacji podczas kompilacji.
8. Dodaj następujący kod do kodem Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Zmiany strony\_obciążenia w page2.aspx do następującego: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Skompiluj projekt.
11. Uruchom projekt.
12. Wprowadź nazwę w polu tekstowym, a następnie kliknij przycisk.
13. Co to jest działanie?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchroniczne stron w programie ASP.NET 2.0

Wiele problemów rywalizacji o zasoby na platformie ASP.NET są spowodowane przez opóźnienia połączeniami zewnętrznymi (np. wywołań usługi lub bazy danych w sieci Web), czas oczekiwania operacji We/Wy plików itp. Po wysłaniu żądania w odniesieniu do aplikacji platformy ASP.NET, ASP.NET używa jednego z jego wątków roboczych do obsługi tego żądania. To żądanie jest właścicielem tego wątku zakończenie żądanie i odpowiedź została wysłana. Program ASP.NET 2.0 ma na celu rozwiązania problemów z opóźnieniem przy użyciu tego rodzaju problemy, dodając możliwość strony są wykonywane asynchronicznie. Oznacza to, czy wątku roboczego można uruchomić żądania, a następnie przekazać dodatkowe wykonywania do innego wątku, w tym samym szybko zwracać do puli dostępnych wątków. Po ukończeniu operacji We/Wy w pliku, wywołanie bazy danych itp. nowy wątek jest uzyskiwana z puli wątków, aby zakończyć żądania.

Pierwszym krokiem w podejmowaniu strony są wykonywane asynchronicznie jest można ustawić **Async** atrybut strony dyrektyw w następujący sposób:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Ten atrybut informuje platformę ASP.NET w celu zaimplementowania IHttpAsyncHandler dla strony.

Następnym krokiem jest wywołać metodę AddOnPreRenderCompleteAsync w punkcie w cyklu życia strony przed PreRender. (Ta metoda jest zwykle nazywany na stronie\_obciążenia.) Metoda AddOnPreRenderCompleteAsync przyjmuje dwa parametry; BeginEventHandler i EndEventHandler. BeginEventHandler zwraca obiekt IAsyncResult, który jest następnie przekazywany jako parametr EndEventHandler.

Poniższy film jest przewodnik żądania asynchroniczne strony.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Otwórz wideo pełnego ekranu](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Na stronie async nie renderowane w przeglądarce, dopóki nie zakończy się EndEventHandler. Bez wątpienia, ale niektórzy deweloperzy będą traktować żądań asynchronicznych jako zbliżone do wywołania zwrotne async. Należy weź pod uwagę, że nie są one. Korzyścią dla żądań asynchronicznych jest, że pierwszym wątku roboczego mogą być zwrócone do puli wątków do obsługi nowych żądań, zmniejszając w ten sposób rywalizacji o zasoby z powodu trwa operacji We/Wy, powiązany, etc.


## <a name="script-callbacks-in-aspnet-20"></a>Wywołania zwrotne skryptu, na platformie ASP.NET 2.0

Deweloperzy sieci Web zawsze sprawdzono, sposobów zapobiec migotanie skojarzonych z wywołaniem zwrotnym. W programie ASP.NET: 1.x, SmartNavigation był najbardziej typowa metoda unikanie migotanie, ale SmartNavigation przyczyną problemów dla deweloperów, niektóre ze względu na złożoność implementacji na komputerze klienckim. Program ASP.NET 2.0 rozwiązuje ten problem za pomocą wywołania zwrotne skryptu. Wywołania zwrotne skryptu wykorzystywać XMLHttp żądań na serwerze sieci Web przy użyciu języka JavaScript. Żądanie XMLHttp zwraca dane XML, które następnie mogą być zmieniane za pomocą przeglądarki modelu DOM. Kod XMLHttp są ukryte przed użytkownikiem przez nowy program obsługi WebResource.axd.

Istnieje kilka kroków, które są niezbędne w celu skonfigurowania wywołanie zwrotne skryptu w programie ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Krok 1: Implementowanie interfejsu ICallbackEventHandler

Aby ASP.NET rozpoznaje strony jako udział w wywołaniu zwrotnym skryptu musi implementować interfejs ICallbackEventHandler. Można to zrobić w pliku związanym z kodem w następujący sposób:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Możesz też to zrobić przy użyciu notacji dyrektywy @ implementuje, więc:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Zazwyczaj używasz dyrektywa @ Implements, korzystając z wbudowanego kodu platformy ASP.NET.

## <a name="step-2--call-getcallbackeventreference"></a>Krok 2: Wywołanie GetCallbackEventReference

Jak wspomniano wcześniej, wywołanie XMLHttp jest hermetyzowany w obsłudze WebResource.axd. Po wyrenderowaniu strony ASP.NET spowoduje to dodanie wywołanie formularz sieci Web\_DoCallback, dostarczone przez WebResource.axd skryptu klienta. Formularz sieci Web\_DoCallBack — funkcja zastępuje \_ \_doPostBack funkcji wywołania zwrotnego. Należy pamiętać, że \_ \_doPostBack programowo przesyła formularz na stronie. W przypadku wywołania zwrotnego chcesz uniemożliwić odświeżenie strony, dlatego \_ \_doPostBack nie jest wystarczające.

> [!NOTE]
> \_\_doPostBack była nadal renderowana do strony w przypadku wywołania zwrotnego skryptu klienta. Nie jest on jednak używany wywołania zwrotnego.


Argumenty dla formularza sieci Web\_DoCallback, funkcja po stronie klienta są udostępniane za pośrednictwem funkcji po stronie serwera GetCallbackEventReference, która zwykle będzie wywoływana na stronie\_obciążenia. Typowe wywołanie GetCallbackEventReference może wyglądać następująco:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> W tym przypadku cm jest wystąpieniem ClientScriptManager. Klasa ClientScriptManager zostały omówione w dalszej części tego modułu.


Istnieje kilka przeciążone wersje GetCallbackEventReference. W tym przypadku argumenty są następujące:

`this`

Odwołanie do formantu, gdzie jest wywoływana GetCallbackEventReference. W tym przypadku jest sama strona.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argument ciągu, który zostanie przekazany z kodu po stronie klienta do zdarzenia po stronie serwera. W tym przypadku wiadomości błyskawicznych, przekazując wartość z listy rozwijanej o nazwie ddlCompany.

`ShowCompanyName`

Nazwa funkcji po stronie klienta, która będzie akceptować wartości zwracanej (jako ciąg) ze zdarzenia wywołania zwrotnego po stronie serwera. Ta funkcja będzie wywoływana tylko, po pomyślnym zakończeniu operacji wywołania zwrotnego po stronie serwera. W związku ze względu na niezawodność, ogólnie zaleca się korzystać z przeciążoną wersją GetCallbackEventReference, która przyjmuje argument dodatkowy ciąg określający nazwę funkcję po stronie klienta do wykonania w przypadku wystąpienia błędu.

`null`

Ciąg reprezentujący funkcji po stronie klienta, który je zainicjował przed wywołania zwrotnego do serwera. W tym przypadku nie jest bez skryptu, argument ma wartość null.

`true`

Wartość logiczna określająca, czy należy przeprowadzić asynchroniczne wywołanie zwrotne.

Wywołanie metody formularza sieci Web\_DoCallback na komputerze klienckim przekazują tych argumentów. W związku z tym, gdy ta strona jest wyświetlana na komputerze klienckim, ten kod będzie wyglądać w następujący sposób:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Należy zauważyć, że podpis funkcji na komputerze klienckim jest nieco inna. Funkcja klienta przekazuje 5 parametrów i wartość logiczną. Dodatkowe parametry, (który ma wartość null w powyższym przykładzie) zawiera funkcję po stronie klienta, która będzie obsługiwać ewentualne błędy z wywołania zwrotnego po stronie serwera.

## <a name="step-3--hook-the-client-side-control-event"></a>Krok 3: Obsługiwać zdarzenia formantu po stronie klienta

Należy zauważyć, że wartość zwracana przez GetCallbackEventReference powyżej został przypisany do zmiennej ciągu. Ten ciąg jest używany do podłączania zdarzeń po stronie klienta, dla formantu, który inicjuje wywołanie zwrotne. W tym przykładzie wywołanie zwrotne jest inicjowane przez listę rozwijaną na stronie tak, chcę dołączyć *OnChange* zdarzeń.

Aby obsługiwać zdarzenia po stronie klienta, po prostu Dodaj program obsługi znaczników po stronie klienta w następujący sposób:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Pamiętamy *cbRef* jest wartością zwracaną z wywołania GetCallbackEventReference. Zawiera wywołanie metody formularza sieci Web\_DoCallback, który został wyświetlony powyżej.

## <a name="step-4--register-the-client-side-script"></a>Krok 4: Zarejestruj skryptu po stronie klienta

Odwołania, które wywołanie GetCallbackEventReference określone, czy skrypt po stronie klienta o nazwie **ShowCompanyName** będzie wykonywany po pomyślnym zakończeniu wywołania zwrotnego po stronie serwera. Czy skrypt musi zostać dodane do strony, przy użyciu wystąpienia ClientScriptManager. (W klasie ClientScriptManager będzie dicussed w dalszej części tego modułu). Dlatego należy wykonać podobne do tego:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Krok 5: Wywołanie metody interfejsu ICallbackEventHandler

ICallbackEventHandler zawiera dwie metody, które są potrzebne do zaimplementowania w kodzie. Są one **RaiseCallbackEvent** i **GetCallbackEvent**.

**RaiseCallbackEvent** przyjmuje ciąg jako argument i zwraca nothing. Argument ciągu jest przekazywany z wywołania po stronie klienta, aby formularz sieci Web\_DoCallback. W takim przypadku ta wartość jest *wartość* atrybutu z listy rozwijanej o nazwie ddlCompany. Kod po stronie serwera, należy umieścić w metodzie RaiseCallbackEvent. Na przykład jeśli wywołanie zwrotne WebRequest względem zewnętrznego zasobu, ten kod będzie umieszczona w RaiseCallbackEvent.

**GetCallbackEvent** jest odpowiedzialna za przetwarzanie zwracana wywołania zwrotnego do klienta. Go nie przyjmuje żadnych argumentów i zwraca wartość typu ciąg. Ciąg, który zwraca zostaną przekazane jako argument do funkcji po stronie klienta, w tym przypadku *ShowCompanyName*.

Po wykonaniu powyższych czynności można przystąpić do wykonania skryptu wywołania zwrotnego w programie ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Otwórz wideo pełnego ekranu](the-asp-net-2-0-page-model/_static/callback1.wmv)


Wywołania zwrotne skryptu, na platformie ASP.NET są obsługiwane w dowolnej przeglądarki obsługującej wprowadzania wywołania XMLHttp. Obejmuje to wszystkie nowoczesne przeglądarki używana już dziś. Program Internet Explorer używa obiektu XMLHttp ActiveX, podczas gdy inne nowoczesnych przeglądarek (w tym nadchodzących programu Internet Explorer 7) używają wewnętrzne obiektu XMLHttp. Aby programowo określić, jeśli przeglądarka obsługuje wywołania zwrotne, możesz użyć **Request.Browser.SupportCallback** właściwości. Właściwość ta zwróci **true** Jeśli żądanie klienta obsługuje wywołania zwrotne skryptu.

## <a name="working-with-client-script-in-aspnet-20"></a>Praca ze skryptem klienta w programie ASP.NET 2.0

Skrypty klienta w programie ASP.NET 2.0 są zarządzane przez użycie klasy ClientScriptManager. Klasa ClientScriptManager przechowuje informacje o skryptów klienta przy użyciu typu i nazwy. Zapobiega to programowo wstawiane na stronie więcej niż jeden raz przez tego samego skryptu.

> [!NOTE]
> Po skrypt został pomyślnie zarejestrowany na stronie, żadne kolejne próby, aby zarejestrować ten sam skrypt powoduje po prostu skrypt nie jest zarejestrowany po raz drugi. Nie zduplikowane skryptów są dodawane, a nie wystąpi wyjątek. Aby uniknąć niepotrzebnych obliczeń, istnieją metody, które można użyć w celu ustalenia, czy skrypt został już zarejestrowany, tak aby nie należy go zarejestrować więcej niż jeden raz.


Metody ClientScriptManager powinny być znane wszystkie bieżące deweloperów platformy ASP.NET:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Metoda ta umożliwia dodanie skryptu na początku renderowanej strony. Jest to przydatne, aby dodać funkcje, które będą wywoływane jawnie na komputerze klienckim.

Istnieją dwie przeciążone wersje tej metody. Trzy z czterech argumentów występują między nimi. Są to:

`type (string)`

***Typu*** argument określa typ skryptu. Zazwyczaj jest to dobry pomysł, aby użyć typu strony, (to. GetType()) dla typu.

`key (string)`

***Klucz*** argument jest zdefiniowane przez użytkownika klucza dla skryptu. Powinna to być unikatowy dla każdego skryptu. Jeśli spróbujesz dodać skrypt za pomocą tego samego klucza i typ skryptu dodane wcześniej, nie można dodać.

`script (string)`

***Skryptu*** argument jest ciąg zawierający rzeczywiste skryptu, aby dodać. Zalecane jest, użyj klasy StringBuilder można utworzyć skrypt, a następnie zastosować metodę ToString() StringBuilder można przypisać ***skryptu*** argumentu.

Jeśli używasz RegisterClientScriptBlock przeciążona, który tylko przyjmuje trzy argumenty, musi zawierać elementy skryptu (&lt;skryptu&gt; i &lt;/script&gt;) w skrypcie.

Można użyć przeciążenia RegisterClientScriptBlock, która przyjmuje czwarty argument. Czwarty argument jest wartość logiczna określająca, czy ASP.NET, należy dodać elementy skryptu dla Ciebie. Jeśli ten argument jest **true**, skrypt nie może zawierać elementy skryptu jawnie.

Należy użyć metody IsClientScriptBlockRegistered, aby określić, jeśli skrypt został już zarejestrowany. Dzięki temu można uniknąć próba ponownego zarejestrowania skrypt, który został już zarejestrowany.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (Nowość w wersji 2.0)

RegisterClientScriptInclude tag tworzy blok skryptu, który stanowi łącze do zewnętrznego pliku skryptu. Ma dwa przeciążenia. Przyjmuje jeden klucz i adres URL. Drugi dodaje trzeci argument określenie typu.

Na przykład, poniższy kod generuje blok skryptu zawierającego łącze do jsfunctions.js w katalogu głównym folderu skryptów aplikacji:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Ten kod tworzy następujący kod na renderowanej stronie:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Blok skryptu jest renderowany w dolnej części strony.


Należy użyć metody IsClientScriptIncludeRegistered, aby określić, jeśli skrypt został już zarejestrowany. Dzięki temu można uniknąć próba ponownego zarejestrowania skrypt.

## <a name="registerstartupscript"></a>RegisterStartupScript

Metoda RegisterStartupScript przyjmuje te same argumenty co metoda RegisterClientScriptBlock. Zarejestrowane w usłudze RegisterStartupScript skrypt wykonuje po załadowaniu strony, ale przed zdarzeniem OnLoad po stronie klienta. W 1.X, zarejestrowane w usłudze RegisterStartupScript skrypty zostały umieszczone bezpośrednio przed zamykającym &lt;/form&gt; tagów podczas zarejestrowane w usłudze RegisterClientScriptBlock skrypty zostały umieszczone bezpośrednio po otwarciu &lt;formularza&gt; tagu. W programie ASP.NET 2.0, oba są umieszczane bezpośrednio przed tagiem zamykającym &lt;/form&gt; tagu.

> [!NOTE]
> Jeśli zarejestrowane przez funkcję RegisterStartupScript, tej funkcji nie będą wykonywane, dopóki nie zostanie jawnie wywołana w kodzie po stronie klienta.


Aby określić, jeśli skrypt został już zarejestrowany i uniknąć próba ponownego zarejestrowania skrypt, należy użyć metody IsStartupScriptRegistered.

## <a name="other-clientscriptmanager-methods"></a>Inne metody ClientScriptManager

Poniżej przedstawiono niektóre inne użyteczne metody klasy ClientScriptManager.


|  <strong>GetCallbackEventReference</strong>   |                                                 Zobacz wywołania zwrotne skryptu, we wcześniejszej części tego modułu.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Pobiera odniesienie JavaScript (javascript:&lt;wywołania&gt;) który może służyć do opublikowania powrót po awarii z zdarzenia po stronie klienta.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Pobiera ciąg, który może służyć do inicjowania wpis przez klienta.                                    |
|      <strong>GetWebResourceUrl</strong>       | Zwraca adres URL do zasobu, który jest osadzony w zestawie. Należy używać w połączeniu z <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Rejestruje zasobu sieci Web ze stroną. Są to zasoby osadzone w zestawie i obsługiwane przez nowy program obsługi WebResource.axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Rejestruje ukryte pole formularza ze stroną.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Rejestruje kod po stronie klienta, który jest wykonywany po przesłaniu formularza HTML.                                   |

