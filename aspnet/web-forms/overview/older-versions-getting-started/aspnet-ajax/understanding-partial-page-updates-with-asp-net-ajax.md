---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: "Opis strony częściowej aktualizacji z ASP.NET AJAX | Dokumentacja firmy Microsoft"
author: scottcate
description: "Prawdopodobnie najbardziej widocznej funkcji rozszerzeń AJAX ASP.NET jest możliwość czy aktualizacje częściowe lub przyrostowych strony nie robiąc pełne odświeżania strony t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 1d8d3009df0a264e466d3f7decfb65978d8ae7a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Strona częściowa Opis aktualizacji z ASP.NET AJAX
====================
przez [Scott kategorii](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Prawdopodobnie najbardziej widocznej funkcji rozszerzeń AJAX ASP.NET jest możliwość czy aktualizacje częściowe lub przyrostowych strony nie robiąc pełne ogłaszania zwrotnego na serwerze bez zmiany kodu i znaczników minimalne zmiany. Zalety są rozległe — niezmieniona stanu z multimediami (takie jak Adobe Flash lub nośnik z systemem Windows), są zmniejszenie kosztów przepustowości i klienta nie występują migotania związanych zazwyczaj ze odświeżania strony.


## <a name="introduction"></a>Wprowadzenie

Technologia ASP.NET firmy Microsoft zapewnia model programowania zorientowane obiektowo i sterowane zdarzeniami i unites go z zalet skompilowanego kodu. Jednak jego model przetwarzania po stronie serwera ma kilka wady związane z technologii:

- Strona aktualizacje wymagają przesłania danych do serwera, który wymaga odświeżania strony.
- Operacji nie zostaną utrwalone efekty generowane przez Javascript lub innych technologii po stronie klienta (takie jak Adobe Flash)
- Podczas odświeżania strony przeglądarki innej niż Microsoft Internet Explorer nie obsługują automatyczne przywrócenie jego położenie przewijania. A nawet w programie Internet Explorer jest nadal migotanie odświeżeniu strony.
- Ogłaszania zwrotnego może wymagać dużej ilości przepustowości jako \_ \_stan WYŚWIETLANIA pola formularza może przekroczyć, szczególnie w przypadku zajmujących się formanty, takie jak kontrolki GridView lub wzmacniaki.
- Nie zostanie ujednoliconego wzór do uzyskiwania dostępu do usługi sieci Web przy użyciu języka JavaScript lub innych technologii po stronie klienta.

Wprowadź rozszerzenia ASP.NET AJAX firmy Microsoft. AJAX, który oznacza **A** synchroniczne **J** avaScript **A** nd **X** ML, to struktura zintegrowane umożliwiające przyrostowe strony aktualizacje za pomocą języka JavaScript i platform, składa się z kodu po stronie serwera składającej się z Microsoft AJAX Framework i składnik skryptu o nazwie Biblioteka skryptów Microsoft AJAX. Rozszerzenia kodu ASP.NET AJAX udostępniają Obsługa platform do uzyskiwania dostępu do usług sieci Web ASP.NET za pośrednictwem kodu JavaScript.

Strona częściowa funkcji Aktualizacje rozszerzeń AJAX ASP.NET, który obejmuje składnika ScriptManager formantu UpdatePanel i kontrolki postępu aktualizacji i uwzględnia scenariusze, w których powinien lub nie powinien być sprawdza, czy ten dokument użyte.

Ten dokument jest oparty na wersji Beta 2 programu Visual Studio 2008 i .NET Framework 3.5, która integruje rozszerzeń programu ASP.NET AJAX Biblioteka klasy podstawowej (gdzie wcześniej był dostępny dla programu ASP.NET 2.0 dodatkowy składnik). Ten dokument założono również, że korzystasz z programu Visual Studio 2008 i nie Visual Web Developer Express Edition; Niektóre szablony projektów, do których istnieją odwołania nie mogą być dostępne dla użytkowników programu Visual Web Developer Express.

## <a name="partial-page-updates"></a>Strona częściowa aktualizacji

Prawdopodobnie najbardziej widocznej funkcji rozszerzeń AJAX ASP.NET jest możliwość czy aktualizacje częściowe lub przyrostowych strony nie robiąc pełne ogłaszania zwrotnego na serwerze bez zmiany kodu i znaczników minimalne zmiany. Zalety są rozległe — niezmieniona stanu z multimediami (takie jak Adobe Flash lub nośnik z systemem Windows), są zmniejszenie kosztów przepustowości i klienta nie występują migotania związanych zazwyczaj ze odświeżania strony.

Możliwość integracji renderowania stron częściowych jest zintegrowany ASP.NET przy minimalnych zmianach w swoim projekcie.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Wskazówki: Integrowanie częściowe renderowanie istniejącego projektu


1. W programie Microsoft Visual Studio 2008, Utwórz nowy projekt witryny sieci Web ASP.NET, przechodząc do *pliku*  *- &gt; nowy*  *- &gt; witryny sieci Web* i wybierając w oknie dialogowym witryny sieci Web ASP.NET. Można określić nazwę dowolne i może zainstalować ją do systemu plików lub do programu Internet Information Services (IIS).
2. Zostaną wyświetlone z puste domyślną stronę z podstawowych znaczników ASP.NET (formularza po stronie serwera i `@Page` dyrektywy). Upuść etykietę o nazwie `Label1` i przycisk o nazwie `Button1` na stronie w obrębie elementu form. Co chcesz mogą ustawić ich właściwości tekstu.
3. W widoku Projekt, kliknij dwukrotnie `Button1` do generowania obsługi zdarzenia związane z kodem. W ramach tej obsługi zdarzeń, skonfigurować `Label1.Text` do kliknięto przycisk! .

**Wyświetlanie 1: Kod znaczników dla default.aspx przed włączeniem częściowe renderowanie**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Wyświetlanie 2: Plik Codebehind (obcięte) default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Naciśnij klawisz F5, aby uruchomić witryny sieci web. Visual Studio spowoduje wyświetlenie monitu o dodanie pliku web.config, aby włączyć debugowanie; to zrobić. Po kliknięciu przycisku zauważyć, że odświeżona zmiany tekstu w etykiecie, a istnieje krótki migotania strona jest odświeżana.
2. Po zamknięciu okna przeglądarki, wróć do programu Visual Studio i na stronie znaczników. Przewiń w dół w przyborniku Visual Studio i znaleźć kartę rozszerzenia AJAX. (Jeśli nie masz na tej karcie, ponieważ używasz starszej wersji rozszerzenia AJAX lub Atlas, odwoływać się do wskazówki rejestrowania elementy przybornika rozszerzenia AJAX w dalszej części tego dokumentu, lub zainstaluj bieżącą wersję za pomocą Instalatora systemu Windows do pobrania Witryna sieci Web).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. *Znany problem:*po zainstalowaniu programu Visual Studio 2008 na komputerze, na którym jest już zainstalowana z rozszerzeniami AJAX ASP.NET 2.0 programu Visual Studio 2005, Visual Studio 2008 zostaną zaimportowane elementy przybornika rozszerzenia AJAX. Można określić, czy jest to możliwe, sprawdzając etykietka narzędzia składników; powinny przekazać komunikat wersji 3.5.0.0. Jeśli podają wersją 2.0.0.0, następnie zaimportowano starych elementów przybornika i będzie musiał ręcznie zaimportować je za pomocą okna dialogowego Wybierz elementy przybornika programu Visual Studio. Nie można do dodawania formantów w wersji 2 za pomocą projektanta.

1. Przed `<asp:Label>` rozpoczyna tag, utworzyć linię odstępem, a następnie kliknij dwukrotnie na formantu UpdatePanel w przyborniku. Należy pamiętać, że nowy `@Register` dyrektywy znajduje się w górnej części strony, wskazującą, czy formanty w przestrzeni nazw System.Web.UI powinny być importowane przy użyciu `asp:` prefiks.
2. Przeciągnij zamknięcia `</asp:UpdatePanel>` tag poza koniec elementu przycisk Tak, aby element jest poprawnie sformułowanym z formantami etykiety i przycisk opakowana.
3. Po otwarciu `<asp:UpdatePanel>` tagów, Rozpocznij nowe tagu początkowego. Należy pamiętać, że funkcja IntelliSense wyświetla monit dwie opcje. W takim przypadku utworzyć `<ContentTemplate>` tagu. Pamiętaj zawijać ten tag wokół etykiety, a przycisk Tak, aby kod znaczników jest poprawnie sformułowany.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. Dowolne miejsce w `<form>` elementu obejmują formantu ScriptManager przez dwukrotne kliknięcie `ScriptManager` element przybornika.
2. Edytuj `<asp:ScriptManager>` tag, tak aby zawiera ona atrybut `EnablePartialRendering= true`.

**Wyświetlanie 3: Kod znaczników dla default.aspx z włączoną funkcją renderowania częściowe**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Otwórz plik web.config. Zwróć uwagę, że Visual Studio automatycznie dodał odwołanie kompilacji do System.Web.Extensions.dll.

1. What's New in Visual Studio 2008: plik web.config, automatycznie dołączony szablony projektów witryny sieci Web ASP.NET wszystkich niezbędnych odwołań do rozszerzeń programu ASP.NET AJAX i zawierają sekcje informacji konfiguracyjnych, które mogą być Cofanie komentarze włączyć dodatkowe funkcje. Visual Studio 2005 mają podobne szablony instalowania rozszerzeń AJAX programu ASP.NET 2.0. Jednak w programie Visual Studio 2008, rozszerzenia interfejsu AJAX są Wypisz domyślnie (oznacza to, że ich odwołuje się domyślnie, ale można usunąć, ponieważ odwołuje się do).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Naciśnij klawisz F5, aby uruchomić witryny sieci Web. Należy zwrócić uwagę, jak zmiany w kodzie źródłowym nie były wymagane do obsługi częściowe renderowanie — tylko znaczników została zmieniona.

Podczas uruchamiania witryny sieci Web powinny być widoczne czy częściowe renderowanie jest teraz włączone, ponieważ po kliknięciu przycisku nie zostanie nie migotanie, ani nie zostaną wprowadzone zmiany w położeniu przewijania strony (w tym przykładzie nie Wykaż, że). Gdyby przyjrzeć się renderowanych źródło strony po kliknięciu przycisku potwierdzi w rzeczywistości nie przeprowadzono ogłaszania — oryginalny tekst etykiety nadal jest częścią kod źródłowy i etykiety zmienił się za pomocą skryptu JavaScript.

Program Visual Studio 2008 nie dostarczane z wstępnie zdefiniowanego szablonu dla witryny sieci web z włączoną obsługą technologii AJAX platformy ASP.NET. Jednak takie szablonu było dostępne w programie Visual Studio 2005, jeśli programu Visual Studio 2005 i rozszerzenia programu ASP.NET 2.0 AJAX zostały zainstalowane. W związku z tym konfigurowanie witryny sieci web i uruchamianie przy użyciu szablonu witryny sieci Web AJAX-Enabled będzie prawdopodobnie jeszcze łatwiejsze jako szablon powinien zawierać plik web.config w pełni skonfigurowana (Obsługa wszystkich rozszerzeń ASP.NET AJAX, łącznie z dostępem do usługi sieci Web i serializacji JSON - JavaScript Object Notation) i domyślnie zawiera element UpdatePanel i elementu ContentTemplate w głównej strony formularzy sieci Web. Włączenie częściowe renderowanie z tą stroną domyślny jest tak proste, jak ponowne odwiedzanie kroku 10 tego przewodnika i upuszczanie formantów na stronę.

## <a name="the-scriptmanager-control"></a>Kontrolki Menedżera skryptów

## <a name="scriptmanager-control-reference"></a>Odwołanie do formantu ScriptManager

Włączone znaczników właściwości:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AllowCustomErrors przekierowania | wartość logiczna | Określa, czy sekcja błędu niestandardowego pliku web.config służy do obsługi błędów. |
| AsyncPostBackError — komunikat | String | Pobiera lub ustawia komunikat o błędzie wysłane do klienta, jeśli występuje błąd. |
| Limit czasu AsyncPostBack | Int32 | Pobiera lub ustawia domyślny czas, jaki klient ma oczekiwać na żądania asynchronicznego zakończyć. |
| Globalizacja EnableScript | wartość logiczna | Pobiera lub ustawia, czy włączono skryptów globalizacji. |
| Lokalizacja EnableScript | wartość logiczna | Pobiera lub ustawia, czy lokalizacja skryptu jest włączona. |
| ScriptLoadTimeout | Int32 | Określa liczbę sekund, dozwolone w przypadku ładowania skryptów do klienta |
| ScriptMode | Wyliczenia (Auto, debugowanie, wersji, dziedziczą) | Pobiera lub ustawia, czy renderowanie wersji skryptów |
| scriptPath | String | Pobiera lub ustawia ścieżkę katalogu głównego do lokalizacji plików skryptów przeznaczonych do wysłania do klienta. |

Właściwości tylko do kodu:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AuthenticationService | Menedżer AuthenticationService | Pobiera szczegóły serwera proxy usługi uwierzytelniania ASP.NET, które zostaną wysłane do klienta. |
| IsDebuggingEnabled | wartość logiczna | Pobiera czy skrypt i debugowanie kodu jest włączone. |
| IsInAsyncPostback | wartość logiczna | Pobiera informacje, czy strona jest obecnie w asynchroniczne żądanie post Wstecz. |
| Elementu ProfileService | Menedżer elementu ProfileService | Pobiera szczegóły serwera proxy usługi profilowania ASP.NET, które zostaną wysłane do klienta. |
| Skrypty | Kolekcja&lt;odwołania do skryptu&gt; | Pobiera kolekcję odwołań do skryptów, które zostaną wysłane do klienta. |
| Usługi | Kolekcja&lt;odwołania do usługi&gt; | Pobiera kolekcję odwołań do serwera proxy usługi sieci Web, które zostaną wysłane do klienta. |
| SupportsPartialRendering | wartość logiczna | Pobiera informacje, czy bieżący klient obsługuje renderowanie częściowej. Jeśli ta właściwość zwraca **false**, wszystkie żądania strony będzie standardowy ogłaszania zwrotnego. |

Metody publiczne kodu:

| **Nazwa metody** | **Typ** | **Opis** |
| --- | --- | --- |
| SetFocus(string) | Void | Ustawia fokus klienta do określonego formantu, gdy żądanie zostało ukończone. |

Elementy podrzędne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;AuthenticationService&gt; | Zawiera szczegółowe informacje o serwerze proxy do usługi uwierzytelniania ASP.NET. |
| &lt;Elementu ProfileService&gt; | Zawiera szczegółowe informacje dotyczące serwera proxy do profilowania usługi ASP.NET. |
| &lt;Skrypty&gt; | Zawiera dodatkowy skrypt służący odwołania. |
| &lt;ASP: ScriptReference&gt; | Określa odwołania do określonego skryptu. |
| &lt;Usługi&gt; | Zawiera dodatkowe informacje usługi sieci Web, których wygenerowane klasy serwera proxy. |
| &lt;ASP: servicereference kierowany&gt; | Określa określone odwołanie do usługi sieci Web. |

Formantu ScriptManager jest istotne core dla rozszerzeń ASP.NET AJAX. Go zapewnia dostęp do biblioteki skryptu (w tym system typów szeroką gamę skryptu po stronie klienta), obsługuje częściowe renderowanie i zapewnia zaawansowaną obsługę dodatkowych usług platformy ASP.NET (takich jak uwierzytelnianie i profilowania, ale również inne usługi sieci Web). Formantu ScriptManager znajdują się również lokalizacja i globalizacja obsługę skryptów klienta.

## <a name="providing-alterative-and-supplemental-scripts"></a>Zapewnianie skryptów alternatywny i uzupełniające

Rozszerzenia Microsoft ASP.NET 2.0 AJAX zawiera kod cały skrypt w obu debugowania i wydania wersji jako zasoby osadzone w zestawów występujących w odwołaniach, deweloperzy mogą przekierować ScriptManager do plików skryptów niestandardowych, jak również zarejestrować dodatkowe wymagane skrypty.

Aby zastąpić powiązania domyślnego dla skryptów zwykle uwzględnione (takich jak te, które obsługuje niestandardowe systemu pisania i przestrzeni nazw Sys.WebForms), możesz zarejestrować się w `ResolveScriptReference` zdarzenia w klasie ScriptManager. Gdy ta metoda jest wywoływana, program obsługi zdarzeń ma możliwość zmienić ścieżkę do pliku skryptu w danym; Menedżer skryptów prześle kopię skrypty różnych lub dostosowane do klienta.

Ponadto skryptu odwołań (reprezentowane przez `ScriptReference` klasy) można dołączać programowo lub za pomocą znacznika. Aby to zrobić, programowo zmodyfikuj `ScriptManager.Scripts` kolekcji, lub obejmują `<asp:ScriptReference>` tagów w obszarze `<Scripts>` tagu, który jest elementem podrzędnym pierwszego stopnia formantu ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Niestandardową obsługę błędów na UpdatePanels

Mimo że aktualizacje będą obsługiwane przez określony przez element UpdatePanel formanty wyzwalaczy, do obsługi Obsługa błędów i niestandardowych komunikatów o błędach jest obsługiwany przez wystąpienie formantu ScriptManager na stronie. Jest to realizowane przez udostępnianie zdarzeń `AsyncPostBackError`, do strony, który można następnie podaj niestandardowej logiki obsługi wyjątków.

Przez zdarzenia AsyncPostBackError, można określić `AsyncPostBackErrorMessage` właściwość, która następnie powoduje, że pole alertu do wywołania po zakończeniu wywołania zwrotnego.

Dostosowywanie po stronie klienta jest także możliwe, zamiast pola alertu domyślne; na przykład można wyświetlić dostosowany `<div>` elementu zamiast domyślnej przeglądarki modalne okno dialogowe. W takim przypadku może obsłużyć błąd w skrypcie klienta:

**Wyświetlanie 5: Skryptu po stronie klienta, aby wyświetlić błędy niestandardowe**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Bardzo łatwo powyższy skrypt rejestruje wywołanie zwrotne środowiska wykonawczego AJAX po stronie klienta dla po wykonaniu żądania asynchronicznego. Następnie sprawdza, czy został zgłoszony błąd i jeśli tak, przetwarza dane, koniec wskazując do środowiska wykonawczego że błąd został obsłużony w skryptu niestandardowego.

## <a name="globalization-and-localization-support"></a>Globalizacja i lokalizacja pomocy technicznej

Formantu ScriptManager zapewnia zaawansowaną obsługę lokalizacja ciągi skryptów i składniki interfejsu użytkownika; Ten temat jest jednak poza zakres tego dokumentu. Aby uzyskać więcej informacji zobacz oficjalny dokument, obsługa globalizacji w ASP.NET AJAX rozszerzenia.

## <a name="the-updatepanel-control"></a>Formantu UpdatePanel

## <a name="updatepanel-control-reference"></a>Odwołanie do formantu UpdatePanel

Włączone znaczników właściwości:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| Dla właściwości ChildrenAsTriggers | bool | Określa, czy formanty podrzędne automatycznie wywołanie odświeżania strony. |
| RenderMode | wyliczenia (bloku, wbudowany) | Określa sposób zawartość zostanie wyświetlone wizualnie. |
| Właściwość UpdateMode | wyliczenia (zawsze, warunkowego) | Określa, czy element UpdatePanel zawsze są odświeżane podczas renderowania częściowe lub gdy tylko jest odświeżany po wybraniu wyzwalacza. |

Właściwości tylko do kodu:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| IsInPartialRendering | bool | Pobiera informacje, czy element UpdatePanel obsługuje częściowe renderowanie dla bieżącego żądania. |
| Elementu ContentTemplate | ITemplate | Pobiera szablon znaczników dla żądania aktualizacji. |
| ContentTemplateContainer | Formant | Pobiera szablon programowe dla żądania aktualizacji. |
| Wyzwalacze | Element UpdatePanel - TriggerCollection | Pobiera listę wyzwalacz skojarzony z bieżącego elementu UpdatePanel. |

Metody publiczne kodu:

| **Nazwa metody** | **Typ** | **Opis** |
| --- | --- | --- |
| Metody Update() | Void | Aktualizuje określony element UpdatePanel programowo. Umożliwia wyzwalanie częściowe renderowanie elementu UpdatePanel w przeciwnym razie untriggered żądania serwera. |

Elementy podrzędne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;Elementu ContentTemplate&gt; | Określa znaczników, który ma być używany do renderowania częściowe renderowanie wyniku. Element podrzędny &lt;asp: UpdatePanel&gt;. |
| &lt;Wyzwalacze&gt; | Określa kolekcję  *n*  formanty powiązane z aktualizacji tego elementu UpdatePanel. Element podrzędny &lt;asp: UpdatePanel&gt;. |
| &lt;ASP: AsyncPostBackTrigger&gt; | Określa wyzwalacz, który wywołuje renderowania strona częściowa dla danego elementu UpdatePanel. To może lub nie może być formant jako element podrzędny elementu UpdatePanel zagrożona. Szczegółowe na nazwę zdarzenia. Element podrzędny &lt;wyzwalaczy&gt;. |
| &lt;ASP: PostBackTrigger&gt; | Określa formant, który powoduje, że cały stronę, aby odświeżyć. To może lub nie może być formant jako element podrzędny elementu UpdatePanel zagrożona. Szczegółowe do obiektu. Element podrzędny &lt;wyzwalaczy&gt;. |

`UpdatePanel` Formant jest formantem ograniczającego zawartości po stronie serwera, która będzie uczestniczył w działaniu częściowe renderowanie rozszerzeń AJAX. Nie ma żadnego limitu liczby UpdatePanel formantów, które mogą się znajdować na stronie i mogą być zagnieżdżone. Każdy element UpdatePanel jest izolowanym, dzięki czemu każdy może pracować wielel osób (może mieć dwóch UpdatePanels uruchomione w tym samym czasie, renderowanie niezależnie od ogłaszania zwrotnego strony różnych części strony).

Element UpdatePanel kontrolować głównie transakcje z wyzwalaczy kontroli — domyślnie wszystkie formanty zawarte w elemencie UpdatePanel `ContentTemplate` tworzącą odświeżania strony jest zarejestrowany jako wyzwalacz dla UpdatePanel. Oznacza to, że element UpdatePanel może współpracować z formantów powiązanych z danymi domyślnej (na przykład w widoku GridView), kontrolek użytkownika i mogą być programowane w skrypcie.

Domyślnie po wyzwoleniu renderowania strona częściowa wszystkie formanty UpdatePanel na stronie zostanie odświeżony, czy element UpdatePanel steruje zdefiniowane wyzwalacze takiego działania. Na przykład jeśli jeden element UpdatePanel definiuje kontrolkę przycisku tego formantu przycisk zostanie kliknięty, domyślnie zostanie odświeżona wszystkie formanty UpdatePanel na tej stronie. Wynika to z faktu, domyślnie `UpdateMode` ma ustawioną właściwość elementu UpdatePanel `Always`. Może też ustawić właściwość UpdateMode na `Conditional`, co oznacza, że UpdatePanel będzie można odświeżyć tylko jeśli określonego wyzwalacza.

## <a name="custom-control-notes"></a>Uwagi dotyczące formantu niestandardowego

Element UpdatePanel można dodać kontrolkę użytkownika lub kontrolek niestandardowych; jednak strony, na którym są uwzględnione tych kontrolek musi także obejmować formantu ScriptManager razem z właściwością ustawioną właściwość EnablePartialRendering **true**.

Jednokierunkowej w którym użytkownik może to uwzględniać podczas za pomocą kontrolki niestandardowe sieci Web jest zastąpienie chronionej `CreateChildControls()` metody `CompositeControl` klasy. W ten sposób można wstrzyknąć UpdatePanel między podrzędnych formantu i publicznie, jeśli okaże się, że strona obsługuje częściowe renderowanie; w przeciwnym razie po prostu można warstwy formantów podrzędnych w kontenerze `Control` wystąpienia.

## <a name="updatepanel-considerations"></a>Zagadnienia dotyczące UpdatePanel

Element UpdatePanel działa jako pola czarne, zawijania ogłaszania zwrotnego ASP.NET w kontekście JavaScript XMLHttpRequest. Istnieją jednak zagadnienia dotyczące wydajności znaczących pamiętać, w zakresie zachowania i szybkości. Aby zrozumieć, jak działa UpdatePanel tak, aby najlepiej zdecydować, kiedy jej użycie jest odpowiednie, należy sprawdzić AJAX exchange. W poniższym przykładzie użyto istniejącej lokacji i, Mozilla Firefox z rozszerzeniem Firebug (Firebug przechwytuje dane o XMLHttpRequest).

Należy wziąć pod uwagę formularza zawierającego, między innymi, kod pocztowy pola tekstowego, który powinien wypełnić pole Miejscowość i województwo na formularz lub formant. Ten formularz ostatecznie zbiera informacje o członkostwie, w tym nazwę użytkownika, adresu i informacji kontaktowych. Istnieje wiele uwagi dotyczące projektowania wziąć pod uwagę zależnie od wymagań danego projektu.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


W oryginalnym iteracji tej aplikacji formantu został skompilowany sprzężone całości danych rejestracji użytkownika, łącznie z kodem pocztowym, miejscowość i województwo. Cały formant został zawinięty w elemencie UpdatePanel i upuszczone na formularza sieci Web. Po wprowadzeniu kodu pocztowego przez użytkownika, element UpdatePanel wykrywa zdarzenia (odpowiedniego TextChanged zdarzenie w zaplecza, określając wyzwalaczy lub za pomocą właściwości dla właściwości ChildrenAsTriggers ustawioną wartość true). Zapisuje AJAX, wszystkie pola w elemencie UpdatePanel, jak przechwycone przez FireBug (zobacz diagramu po prawej stronie).

Jak Przechwytywanie ekranu wskazuje, są dostępne w ramach wartości z każdego formantu UpdatePanel (w tym przypadku są wszystkie puste), a także pole stanu widoku. Wszystkie told ponad 9kb danych jest wysyłane, gdy w rzeczywistości były potrzebne tylko pięć bajtów danych do zgłoszenia tego konkretnego żądania. Odpowiedź jest bardziej przeglądarek: łącznie 57kb są wysyłane do klienta, wystarczy, aby zaktualizować pole tekstowe, pole listy rozwijanej.

Może to być również przydatne, aby zobaczyć, jak ASP.NET AJAX aktualizuje prezentacji. Odpowiedź część elementu UpdatePanel żądanie aktualizacji jest wyświetlany w obszarze Firebug ekranu konsoli po lewej stronie; jest specjalnie sformułowane rozdzielany potoku ciąg, który jest na skrypt po stronie klienta, a następnie ponownie złożone, na stronie. W szczególności ustawia ASP.NET AJAX *innerHTML* właściwości elementu HTML na kliencie, który reprezentuje z elementu UpdatePanel. Jak przeglądarka generuje ponownie modelu DOM, występuje niewielkie opóźnienie, w zależności od ilości informacji, który ma zostać przetworzony.

Ponowne generowanie modelu DOM wyzwala liczba dodatkowe problemy:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- W przypadku ukierunkowanych elementu HTML w elemencie UpdatePanel, utraci fokus. Tak dla użytkowników, którzy naciśnięty klawisz Tab, aby zamknąć okno tekstu kod pocztowy, ich przeznaczenia dalej byłby Miasto pola tekstowego. Jednak po UpdatePanel odświeżyć ekran, formularz już nie miałoby fokus i naciśnięcie klawisza Tab będzie uruchomiony wyróżniania elementów zespołu (takie jak łącza).
- Jeśli dowolnego typu niestandardowego skryptu po stronie klienta jest w użyciu, którym uzyskuje dostęp do elementów modelu DOM, odwołań utrwalonego za pomocą funkcji może stać się unieczynnienia po częściowej ogłaszania zwrotnego.

UpdatePanels nie mają być wychwytywania rozwiązania. Zamiast stanowią szybkie rozwiązanie dla niektórych sytuacjach, na przykład tworzenie prototypów, aktualizacje małych kontroli i zapewniają znanego interfejsu deweloperów platformy ASP.NET, które mogą być zapoznać się z modelu obiektów programu .NET, ale dzięki do mniej z modelu DOM. Istnieje kilka rozwiązań alternatywnych, które mogą zapewnia lepszą wydajność, w zależności od scenariusza aplikacji:

- Należy rozważyć użycie PageMethods i JSON (JavaScript Object Notation) umożliwia deweloperom wywołanie metody statyczne na stronie tak, jakby wywołano wywołanie usługi sieci web. Ponieważ te metody są statyczne, stan nie jest konieczne; obiekt wywołujący skryptu dostarcza parametry, a wynik zostanie zwrócony asynchronicznie.
- Należy rozważyć użycie usługi sieci Web i JSON, jeśli jeden formant musi być używany w kilku miejscach w aplikacji. Ponownie to wymaga bardzo małego wysiłku specjalne i działa w sposób asynchroniczny.

Dołączanie funkcji za pośrednictwem usług sieci Web lub metody strony ma również wady. Najpierw i wszystkim deweloperów platformy ASP.NET zwykle zazwyczaj tworzenie małych składników funkcji do kontrolki użytkownika (plikach ascx). Strona metody nie może być umieszczona w tych plikach; muszą one obsługiwane w obrębie klasy strony .aspx rzeczywiste. Podobnie, usług sieci Web musi być obsługiwana w ramach klasy .asmx. W zależności od aplikacji tej architektury mogą naruszyć jednej zasady odpowiedzialności, w tym funkcje jeden składnik jest teraz rozłożyć na co najmniej dwa składniki fizyczne, które mogą mieć żadnych ties spójności.

Na koniec Jeśli aplikacja wymaga, aby UpdatePanels są używane, rozwiązywanie problemów i konserwacji powinna pomagać poniższe wskazówki.

- **Jak najmniejszy możliwy UpdatePanels gniazda nie tylko w obrębie jednostek, ale także na jednostek kodu.** Na przykład UpdatePanel na stronie, która opakowuje formantu, gdy ten formant zawiera również element UpdatePanel, który zawiera inny formant, który zawiera element UpdatePanel, jest zagnieżdżanie między jednostki. Dzięki temu Wyczyść elementy, które powinny być odświeżanie i zapobiega odświeżania nieoczekiwany element podrzędny UpdatePanels.
- **Zachowaj *dla właściwości ChildrenAsTriggers* właściwość ma wartość false, a jawnie ustawiona wyzwolenie zdarzenia.** Przy użyciu `<Triggers>` kolekcji to znacznie lepszy sposób obsługi zdarzeń i może uniemożliwić nieoczekiwanego zachowania przy zadania konserwacji i wymuszanie dewelopera w zdarzenie.
- **Najmniejsza możliwa jednostka umożliwia uzyskanie funkcji.** Zgodnie z opisem w dyskusji usługę kod pocztowy zawijania, że tylko minimalne czynności skraca czas serwera przetwarzania całkowitej i rozmiaru klient serwer exchange, zwiększając wydajność.

## <a name="the-updateprogress-control"></a>Kontrolki postępu aktualizacji

## <a name="updateprogress-control-reference"></a>Odwołanie do formantu postępu aktualizacji

Włączone znaczników właściwości:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AssociatedUpdate PanelID | String | Określa identyfikator elementu UpdatePanel, że ta wartość UpdateProgress należy sporządzić raport na. |
| Wartość DisplayAfter | int | Określa limit czasu w milisekundach, przed wyświetleniem tego formantu, po rozpoczęciu żądania asynchronicznego. |
| DynamicLayout | bool | Określa, czy postępu jest renderowany dynamicznie. |

Elementy podrzędne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;Element ProgressTemplate&gt; | Zawiera szablon formantu, ustaw dla zawartości, która będzie wyświetlana z tym formantem. |

Kontrolki postępu aktualizacji umożliwia miary opinię, aby zachować zainteresowania użytkowników podczas wykonywania pracy niezbędne do transportu do serwera. Może to ułatwić użytkowników, czy robimy coś mimo że może nie być oczywista, szczególnie w przypadku, ponieważ większość użytkowników są używane do strony odświeżanie i wyświetlanie na pasku stanu, zaznacz.

Kontrolki postępu aktualizacji można jako Uwaga występować w dowolnym miejscu w hierarchii strony. Jednak w przypadkach, w których częściowe ogłaszania zwrotnego jest inicjowana z elementem podrzędnym elementu UpdatePanel (gdzie element UpdatePanel jest zagnieżdżony w innym elemencie UpdatePanel) postbacks tego wyzwalacza elementów podrzędnych elementu UpdatePanel spowoduje, że szablony postępu aktualizacji, który będzie wyświetlany dla obiekt podrzędny Element UpdatePanel, jak i element nadrzędny elementu UpdatePanel. Jednak jeśli wyzwalacza jest element podrzędny elementu nadrzędnego elementu UpdatePanel, a następnie wyświetli tylko szablony postępu aktualizacji skojarzonego z nadrzędnym.

## <a name="summary"></a>Podsumowanie

Rozszerzenia Microsoft ASP.NET AJAX są zaawansowane produkty ułatwiających udostępnianie zawartości sieci web i zapewnia bardziej rozbudowane środowisko użytkownika do aplikacji sieci web. Jako część rozszerzenia ASP.NET AJAX, formantów strony częściowe renderowanie, tym formantach ScriptManager, element UpdatePanel i postępu aktualizacji są niektóre z najczęściej widoczne składników zestawu narzędzi.

Składnika ScriptManager integruje się zapewnienie klienta języka JavaScript dla rozszerzeń, a także umożliwia różnych składników serwera i klienta, współpraca przy minimalnym programowanie inwestycji.

Formantu UpdatePanel jest widoczne pole magic — znaczników w elemencie UpdatePanel może mieć plik Codebehind po stronie serwera i wyzwalają odświeżania strony. Formanty UpdatePanel mogą być zagnieżdżane i mogą być zależne od kontrolek w inne UpdatePanels. Domyślnie UpdatePanels obsługiwać żadnych ogłaszania zwrotnego wywoływane przez ich formanty podrzędne, mimo że ta funkcja precyzyjne dostrajania, deklaratywnego lub programowo.

Korzystając z formantu UpdatePanel, deweloperzy należy zwrócić uwagę ich wpływ na wydajność potencjalnie może wystąpić. Potencjalne alternatywne metody obejmują usługi sieci web i metody strony, chociaż należy rozważyć podczas projektowania aplikacji.

Postępu aktualizacji, który kontroli umożliwia użytkownikowi wiedzieć, że użytkownik lub osoba nie jest ignorowany i że wewnętrznych żądania trwa podczas strony nie jest wykonywanie żadnych czynności, aby odpowiedzieć na dane wejściowe użytkownika. Zawiera także możliwość przerwania wyniki częściowe renderowanie.

Te narzędzia pomocy ze sobą, utworzyć środowisko użytkowników zaawansowanych i łatwego działają mniej oczywista użytkownikowi i mniej przerywania tego przepływu pracy.

## <a name="bio"></a>Mnie

Scott IE pracuje z technologii Microsoft Web od 1997 i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacje oparte na systemie koncentruje się na rozwiązania w zakresie oprogramowania bazy wiedzy Knowledge Base. Scott można nawiązać połączenie za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blogu w [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Dalej](understanding-asp-net-ajax-updatepanel-triggers.md)
