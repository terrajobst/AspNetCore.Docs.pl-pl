---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Opis aktualizacji stron częściowych przy użyciu rozszerzeń ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Może być najbardziej widoczne funkcji rozszerzenia AJAX programu ASP.NET jest zdolność przeprowadzania aktualizacji strony częściowego lub przyrostowe bez wykonywania pełnego odświeżania do t...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 8ec4df5ffeaab4490eaea0f0093d543d517bd5f4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805275"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Objaśnienie aktualizacji stron częściowych przy użyciu rozszerzeń ASP.NET AJAX
====================
przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Może być najbardziej widoczne funkcji rozszerzenia AJAX programu ASP.NET jest zdolność przeprowadzania aktualizacji strony częściowego lub przyrostowe bez wykonania tej czynności pełny zwrot do serwera, bez wprowadzania zmian w kodzie i zmianami znaczników minimalny. Zalety są obszerne — stan multimediach (takich jak Adobe Flash lub Windows Media) pozostaje niezmieniony, są obniżone koszty przepustowości i klienta nie występują migotania związanych zazwyczaj ze ogłaszania zwrotnego.


## <a name="introduction"></a>Wprowadzenie

Firmy Microsoft technologii ASP.NET oferuje zorientowane obiektowo i oparte na zdarzeniach model programowania i łączy w sobie go z korzyściami skompilowanego kodu. Jednak jego przetwarzania po stronie serwera modelu ma kilka wady związane z technologii:

- Strona aktualizacje wymagają przesłania danych do serwera, który wymaga odświeżenia strony.
- Rund nie są zachowywane efekty, który został wygenerowany przez programy Javascript lub innej technologii po stronie klienta (takich jak Adobe Flash)
- Podczas odświeżania przeglądarki innej niż Microsoft Internet Explorer nie obsługują automatyczne przywrócenie jego położenie przewijania. I jeszcze w programie Internet Explorer istnieje migotanie jako strona zostanie odświeżona.
- Ogłaszania zwrotnego może obejmować dużej ilości przepustowości jako \_ \_VIEWSTATE pola formularza może rosnąć, szczególnie gdy chodzi o kontrolek, takich jak kontrolki GridView lub wzmacniaki.
- Nie ma żadnych zunifikowany model przeznaczony do uzyskiwania dostępu do usług sieci Web za pomocą języka JavaScript lub innej technologii po stronie klienta.

Wprowadź rozszerzeń ASP.NET AJAX firmy Microsoft. AJAX, który oznacza **A** synchroniczne **"j"** avaScript **A** nd **X** uczenie Maszynowe, to zintegrowane umożliwiająca dostarczanie przyrostowe strony aktualizacje przy użyciu języka JavaScript dla wielu platform, składające się z kodu po stronie serwera Microsoft Framework w technologii AJAX i składnik skryptu o nazwie biblioteki skryptów AJAX firmy Microsoft. Rozszerzeń ASP.NET AJAX również zapewniają obsługę wielu platform do uzyskiwania dostępu do usług sieci Web ASP.NET przy użyciu języka JavaScript.

Strona częściowa aktualizacje funkcji rozszerzenia AJAX programu ASP.NET, który obejmuje składnika ScriptManager, kontrolki UpdatePanel i kontrolki UpdateProgress i uwzględnia scenariusze, w których powinien lub nie powinien być sprawdza, czy ten oficjalny dokument wykorzystywane.

Ten oficjalny dokument jest oparty na wersji Beta 2 programu Visual Studio 2008 i programu .NET Framework 3.5, którą można zintegrować rozszerzenia AJAX programu ASP.NET do Biblioteka klasy podstawowej (której wcześniej był dodatkowy składnik dostępne dla programu ASP.NET 2.0). Tym oficjalnym dokumencie założono, że używasz programu Visual Studio 2008 i nie Visual Web Developer Express Edition; Niektóre szablony projektów, których istnieją odwołania może nie być dostępne dla użytkowników programu Visual Web Developer Express.

## <a name="partial-page-updates"></a>Aktualizacji stron częściowych

Może być najbardziej widoczne funkcji rozszerzenia AJAX programu ASP.NET jest zdolność przeprowadzania aktualizacji strony częściowego lub przyrostowe bez wykonania tej czynności pełny zwrot do serwera, bez wprowadzania zmian w kodzie i zmianami znaczników minimalny. Zalety są obszerne — stan multimediach (takich jak Adobe Flash lub Windows Media) pozostaje niezmieniony, są obniżone koszty przepustowości i klienta nie występują migotania związanych zazwyczaj ze ogłaszania zwrotnego.

Możliwość integracji renderowania stron częściowych jest integrowany ASP.NET przy minimalnych zmianach w projekcie.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Wskazówki: Integrowanie częściowe renderowanie istniejący projekt


1. W programie Microsoft Visual Studio 2008, należy utworzyć nowy projekt witryny sieci Web platformy ASP.NET, przechodząc do <em>pliku</em>  <em>- &gt; New</em>  <em>- &gt; witryny sieci Web</em> i wybierając pozycję witryny sieci Web platformy ASP.NET z poziomu okna dialogowego. Można określić nazwę wedle uznania i może zainstalować ją w systemie plików lub do programu Internet Information Services (IIS).
2. Zostaną wyświetlone z pustą domyślną stroną z podstawowych znaczników ASP.NET (formularza po stronie serwera i `@Page` dyrektywy). Usunąć etykietę o nazwie `Label1` oraz przycisk o nazwie `Button1` na stronę w elemencie formularza. Może ustawiać ich właściwości tekstu, wedle uznania.
3. W widoku Projekt, kliknij dwukrotnie `Button1` do generowania obsługi zdarzenia związanego z kodem. W ramach tego programu obsługi zdarzeń, należy ustawić `Label1.Text` do kliknięto przycisk! .

**Wyświetlanie 1: Kod znaczników dla default.aspx przed włączeniem częściowe renderowanie**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Wyświetlanie 2: Codebehind (spacje) default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Naciśnij klawisz F5, aby uruchomić witryny sieci web. Program Visual Studio wyświetli monit o dodanie pliku web.config w celu włączenia debugowania. to zrobić. Po kliknięciu przycisku, zwróć uwagę, że odświeżona do zmiany tekstu w etykiecie, a ma krótki migotania odświeżeniu strony.
2. Po zamknięciu okna przeglądarki, wróć do programu Visual Studio, jak i do strony znaczników. Przewiń w dół w przyborniku programu Visual Studio i Znajdź kartę rozszerzenia AJAX. (Jeśli nie masz na tej karcie, ponieważ używasz starszej wersji rozszerzeń AJAX lub Atlas, zapoznaj się przewodnik rejestracji rozszerzenia AJAX elementów przybornika w dalszej części tego dokumentu, lub zainstaluj bieżącą wersję za pomocą Instalatora Windows do pobrania z witryny sieci Web).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Znany problem:</em>po zainstalowaniu programu Visual Studio 2008 na komputerze, na którym jest już zainstalowany za pomocą rozszerzenia AJAX programu ASP.NET w wersji 2.0 programu Visual Studio 2005, Visual Studio 2008 zostaną zaimportowane elementy przybornika rozszerzenia AJAX. Można określić, czy jest to możliwe, sprawdzając etykietka narzędzia składników; powinny one Załóżmy, że wersja 3.5.0.0. Jeśli podają wersji 2.0.0.0 lub nowszej, następnie zaimportowano starych elementów przybornika i należy ręcznie zaimportować je za pomocą okna dialogowego Wybierz elementy paska narzędzi w programie Visual Studio. Nie można dodać kontrolki w wersji 2 przy użyciu narzędzia Projektant.

2. Przed `<asp:Label>` rozpoczyna się tag, Utwórz wiersz białe znaki i kliknij dwukrotnie kontrolki UpdatePanel z przybornika. Należy pamiętać, że nowy `@Register` dyrektywa jest uwzględniane w górnej części strony, wskazujący, że formanty w przestrzeni nazw System.Web.UI powinny być importowane przy użyciu `asp:` prefiks.
3. Przeciągnij zamykającym `</asp:UpdatePanel>` tag poza końcem Button element tak, aby element jest poprawnie sformułowanym z formantami etykiet i przycisk opakowana.
4. Po otwarciu `<asp:UpdatePanel>` tag, rozpoczęcie nowego tagu początkowego. Należy pamiętać, że funkcja IntelliSense wyświetli monit o dwie opcje. W takim przypadku utworzyć `<ContentTemplate>` tagu. Pamiętaj zawinięte ten tag wokół Twoje etykiety i przycisk znaczników jest poprawnie sformułowany.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. W dowolnym miejscu `<form>` elementu, obejmują formantu ScriptManager przez dwukrotne kliknięcie `ScriptManager` w przyborniku.
2. Edytuj `<asp:ScriptManager>` tag, aby obejmowała atrybut `EnablePartialRendering= true`.

**Wyświetlanie 3: Znaczników dla default.aspx z włączoną funkcją renderowania częściowe**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Otwórz plik web.config. Należy zauważyć, że program Visual Studio automatycznie dodał odwołanie kompilacji do System.Web.Extensions.dll.

1. What's New in Visual Studio 2008: plik web.config, dostarczanego z szablonami projektu witryny sieci Web platformy ASP.NET, automatycznie obejmuje wszystkie niezbędne odwołania do rozszerzenia AJAX programu ASP.NET i zawiera sekcje informacji konfiguracyjnych, które mogą być niezaznaczone komentarzem włączyć dodatkowe funkcje. Visual Studio 2005 wcześniejszego podobne szablony zostały zainstalowane rozszerzenia AJAX programu ASP.NET 2.0. W programie Visual Studio 2008, rozszerzenia AJAX są jednak zrezygnować domyślnie (oznacza to, że ich są określone przez domyślny, ale można usuwać odwołania).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Naciśnij klawisz F5, aby uruchomić witryny sieci Web. Należy zauważyć, jak zmiany kodu źródłowego, nie były wymagane do obsługi częściowe renderowanie — tylko znaczników został zmieniony.

Po uruchomieniu witryny sieci Web, powinien zostać wyświetlony, że częściowe renderowanie jest teraz włączone, ponieważ po kliknięciu przycisku, nie będzie żadnych migotania, ani nie zostaną wprowadzone żadne zmiany jego położenie przewijania strony (w tym przykładzie nie wykazują, że). Gdyby Przyjrzyj się renderowany źródło strony po kliknięciu przycisku potwierdzi w rzeczywistości nie przeprowadzono wstecz po — oryginalny tekst etykiety jest nadal częścią kod źródłowy i etykiety zmienił się za pomocą języka JavaScript.

Program Visual Studio 2008 nie ma dołączone wstępnie zdefiniowanym szablonie dostępnym dla witryny sieci web z włączoną obsługą technologii AJAX programu ASP.NET. Jednak taki szablon był dostępny w programie Visual Studio 2005, jeśli były zainstalowane Visual Studio 2005 i rozszerzenia AJAX programu ASP.NET 2.0. W związku z tym konfigurowanie witryny sieci web i uruchamianie za pomocą szablonu witryny sieci Web AJAX-Enabled będzie prawdopodobnie jeszcze łatwiejsze szablon powinien zawierać plik web.config w pełni skonfigurowana (Obsługa wszystkie rozszerzenia AJAX programu ASP.NET, w tym dostęp do usług sieci Web a serializacja kodu JSON — JavaScript Object Notation) i domyślnie zawiera kontrolki UpdatePanel i ContentTemplate na głównej stronie formularzy sieci Web. Włączenie częściowe renderowanie z tą stroną domyślny jest tak proste, jak ponowne spojrzenie na 10 kroku tego przewodnika i upuszczając formantów na stronę.

## <a name="the-scriptmanager-control"></a>Formantu ScriptManager

## <a name="scriptmanager-control-reference"></a>Odwołanie do formantu ScriptManager

Włączone znaczników właściwości:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | wartość logiczna | Określa, czy sekcja błędu niestandardowego pliku web.config służy do obsługi błędów. |
| AsyncPostBackError — komunikat | String | Pobiera lub ustawia komunikat o błędzie, wysłane do klienta, jeśli występuje błąd. |
| AsyncPostBack-Timeout | Int32 | Pobiera lub ustawia domyślny czas, który klient ma oczekiwać na zakończenie żądania asynchronicznego. |
| EnableScript-Globalization | wartość logiczna | Pobiera lub ustawia informację, czy włączono skryptów globalizacji. |
| EnableScript-Localization | wartość logiczna | Pobiera lub ustawia informację, czy lokalizacja skryptu jest włączona. |
| ScriptLoadTimeout | Int32 | Określa liczbę sekund, dozwolony dla ładowania skryptów do klienta |
| ScriptMode | Wyliczenia (Auto, debugowania, wersji, dziedziczą) | Pobiera lub ustawia, czy renderowanie wersji skryptów |
| ScriptPath | String | Pobiera lub ustawia ścieżkę katalogu głównego do lokalizacji plików skryptów przeznaczonych do wysłania do klienta. |

Właściwości tylko do kodu:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Pobiera szczegółowe informacje o serwerze proxy usługi uwierzytelniania platformy ASP.NET, który zostanie wysłany do klienta. |
| IsDebuggingEnabled | wartość logiczna | Czy skrypt pobiera i debugowania kodu jest włączona. |
| IsInAsyncPostback | wartość logiczna | Pobiera informacje, czy strona jest obecnie w żądaniu post wstecz asynchronicznym. |
| ProfileService | ProfileService-Manager | Pobiera szczegółowe informacje o serwerze proxy usługi profilowania ASP.NET, który zostanie wysłany do klienta. |
| Skrypty | Kolekcja&lt;odwołanie do skryptu&gt; | Pobiera kolekcję odwołania do skryptu, które zostaną wysłane do klienta. |
| Usługi | Kolekcja&lt;odwołanie do usługi&gt; | Pobiera kolekcję odwołań do serwera proxy usługi sieci Web, które zostaną wysłane do klienta. |
| SupportsPartialRendering | wartość logiczna | Pobiera informacje, czy bieżące klienta obsługuje częściowe renderowanie. Jeśli ta właściwość zwraca **false**, wszystkie żądania strony będą standardowa ogłaszania zwrotnego. |

Metody publiczne kodu:

| **Nazwa metody** | **Typ** | **Opis** |
| --- | --- | --- |
| SetFocus(string) | Void | Ustawia fokus klienta w określonego formantu żądania zostało ukończone. |

Elementy podrzędne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;AuthenticationService&gt; | Zawiera szczegółowe informacje o serwerze proxy do usługi uwierzytelniania platformy ASP.NET. |
| &lt;ProfileService&gt; | Zawiera szczegółowe informacje dotyczące serwera proxy do profilowania usługi ASP.NET. |
| &lt;Skrypty&gt; | Zawiera odwołania do skryptu dodatkowe. |
| &lt;ASP: ScriptReference&gt; | Wskazuje, że odwołanie do określonego skryptu. |
| &lt;Usługa&gt; | Zawiera dodatkowe informacje usługi sieci Web, zainstalowanym klasy serwera proxy generowany. |
| &lt;ASP: ServiceReference&gt; | Wskazuje, że określone odwołanie do usługi sieci Web. |

Formantu ScriptManager stanowi podstawę niezbędne dla rozszerzenia AJAX programu ASP.NET. Jego zapewnia dostęp do biblioteki skryptów (w tym system typów rozbudowane skryptu po stronie klienta), obsługuje częściowe renderowanie i zapewnia zaawansowaną obsługę dodatkowych usług platformy ASP.NET (takich jak uwierzytelnianie i profilowania, ale również inne usługi sieci Web). Formantu ScriptManager znajdują się również lokalizacja i globalizacja Obsługa skryptów klienta.

## <a name="providing-alterative-and-supplemental-scripts"></a>Zapewnianie skrypty alternatywny i uzupełniające

Rozszerzenia AJAX programu Microsoft ASP.NET 2.0 zawiera kod cały skrypt w obu debugowania i wydania wersji jako zasoby osadzone w przywoływanych zestawach, deweloperzy mogą przekierować funkcja ScriptManager do plików skryptów niestandardowych, a także rejestrowanie dodatkowe wymagane skrypty.

Aby zastąpić domyślne powiązanie zazwyczaj uwzględniony skryptów (takich jak te, które obsługują Sys.WebForms przestrzeni nazw i niestandardowego systemu Pisownia), możesz zarejestrować `ResolveScriptReference` zdarzeń klasy ScriptManager. Gdy ta metoda jest wywoływana, program obsługi zdarzeń ma szansy sprzedaży, aby zmienić ścieżkę do pliku skryptu w danym; Menedżer skryptu prześle kopię skrypty różnych lub dostosowanych do klienta.

Ponadto skryptu odwołania (reprezentowane przez `ScriptReference` klasy) może zostać dołączony programowo lub za pomocą znaczników. Aby to zrobić, programowo zmodyfikuj `ScriptManager.Scripts` kolekcji, lub Uwzględnij `<asp:ScriptReference>` tagów w obszarze `<Scripts>` znacznik, który jest elementem podrzędnym pierwszego poziomu formantu ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Niestandardowej obsługi błędów dla UpdatePanels

Mimo że aktualizacje są obsługiwane przez wyzwalacze określony przez formantów UpdatePanel, pomocy technicznej do obsługi błędów i niestandardowe komunikaty o błędach jest obsługiwany przez wystąpienia formantu ScriptManager strony. Jest to realizowane przez udostępnianie zdarzeń `AsyncPostBackError`, do strony, który można następnie zapewnić logikę niestandardową obsługę wyjątków.

Za korzystanie z zdarzeń AsyncPostBackError, można określić `AsyncPostBackErrorMessage` właściwość, która następnie powoduje, że okno dialogowe alertu do wywołania po ukończeniu wywołania zwrotnego.

Dostosowywanie po stronie klienta jest również możliwe, zamiast przy użyciu domyślnego pola alertu; na przykład możesz chcieć wyświetlić dostosowany `<div>` elementu zamiast domyślnej przeglądarki modalne okno dialogowe. W takim przypadku może obsłużyć błąd w skrypcie klienta:

**Wyświetlanie 5: Skrypt po stronie klienta, aby wyświetlić błędy niestandardowe**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Bardzo łatwo powyższy skrypt rejestruje wywołanie zwrotne ze środowiskiem uruchomieniowym AJAX po stronie klienta dla po ukończeniu żądania asynchronicznego. Następnie sprawdza, czy błąd został zgłoszony i jeśli tak, przetwarza szczegółowe informacje, na koniec wskazujący do środowiska uruchomieniowego czy błąd został obsłużony w skryptu niestandardowego.

## <a name="globalization-and-localization-support"></a>Globalizacja i lokalizacja pomocy technicznej

Formantu ScriptManager oferuje zaawansowaną obsługę dla lokalizacji ciągi skryptu oraz składników interfejsu użytkownika; jednak tego tematu znajduje się poza zakres tego dokumentu. Aby uzyskać więcej informacji zobacz oficjalny dokument, obsługa globalizacji w rozszerzenia AJAX programu ASP.NET.

## <a name="the-updatepanel-control"></a>Kontrolki UpdatePanel

## <a name="updatepanel-control-reference"></a>Odwołanie do kontrolki UpdatePanel

Włączone znaczników właściwości:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Określa, czy formanty podrzędne automatycznie wywołują odświeżanie zwrotu. |
| RenderMode | wyliczenia (bloku, wbudowane) | Określa, że jej zawartość zostanie wyświetlona wizualnie. |
| UpdateMode | wyliczenia (zawsze, warunkowego) | Określa, czy zawsze odświeżania kontrolki UpdatePanel podczas renderowania częściowego lub jeśli jest on tylko odświeżany po trafieniu wyzwalacza. |

Właściwości tylko do kodu:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| IsInPartialRendering | bool | Pobiera informacje, czy kontrolki UpdatePanel obsługuje częściowe renderowanie dla bieżącego żądania. |
| ContentTemplate | ITemplate | Pobiera szablon znaczników dla żądania aktualizacji. |
| ContentTemplateContainer | Formant | Pobiera szablon programistyczny dla żądania aktualizacji. |
| Wyzwalacze | Kontrolki UpdatePanel - TriggerCollection | Pobiera listę wyzwalacz skojarzony z bieżącym UpdatePanel. |

Metody publiczne kodu:

| **Nazwa metody** | **Typ** | **Opis** |
| --- | --- | --- |
| Update() | Void | Aktualizuje określony UpdatePanel programowo. Zezwala na żądania serwera wyzwolić częściowe renderowanie prvku UpdatePanel w przeciwnym razie untriggered. |

Elementy podrzędne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;ContentTemplate&gt; | Kod znaczników określa ten ma być używany do renderowania wyniki częściowe renderowanie. Element podrzędny elementu &lt;asp: UpdatePanel&gt;. |
| &lt;Wyzwalacze&gt; | Określa kolekcję elementów *n* kontrolki skojarzone z aktualizowaniem tej kontrolki UpdatePanel. Element podrzędny elementu &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Określa wyzwalacz, który wywołuje renderowania stron częściowych dla danej kontrolki UpdatePanel. To może być lub może nie być formantu jako element podrzędny w danym UpdatePanel. Szczegółową nazwę zdarzenia. Element podrzędny elementu &lt;wyzwalaczy&gt;. |
| &lt;asp:PostBackTrigger&gt; | Określa formant, który powoduje, że całej strony odświeżyć. To może być lub może nie być formantu jako element podrzędny w danym UpdatePanel. Szczegółową do obiektu. Element podrzędny elementu &lt;wyzwalaczy&gt;. |

`UpdatePanel` Formant jest formantem, ograniczającego zawartości po stronie serwera, który będzie uczestniczył w funkcji częściowe renderowanie rozszerzeń AJAX. Nie ma żadnego limitu liczby formantów UpdatePanel, które mogą się znajdować na stronie, a można zagnieżdżać. Każdej kontrolki UpdatePanel zostanie wyizolowana, tak aby każdy może działać niezależnie (może mieć dwóch UpdatePanels uruchomione w tym samym czasie, renderowanie różnych części strony, niezależnie od ogłaszania zwrotnego strony).

Kontrolki UpdatePanel kontrolować przede wszystkim ofertami z wyzwalaczami kontroli — domyślnie dowolną kontrolkę zawartych w UpdatePanel `ContentTemplate` tworząca ogłaszania zwrotnego jest zarejestrowany jako wyzwalacza dla kontrolki UpdatePanel. Oznacza to, że kontrolki UpdatePanel można pracować z formantów powiązanych z danymi domyślne (na przykład GridView), kontrolek użytkownika, i mogą one być zaprogramowane w skrypcie.

Domyślnie po wyzwoleniu renderowania strony częściowej wszystkich formantów UpdatePanel na stronie zostanie odświeżona, czy kontrolki UpdatePanel zdefiniowane wyzwalacze takiego działania. Na przykład jeśli jeden UpdatePanel definiuje formant przycisku i kliknięciu tego formantu przycisku, domyślnie zostanie odświeżona wszystkich formantów UpdatePanel na tej stronie. Jest to spowodowane, domyślnie `UpdateMode` kontrolki UpdatePanel zostaje ustalona `Always`. Alternatywnie możesz ustawić właściwość UpdateMode `Conditional`, co oznacza, że kontrolki UpdatePanel będzie można odświeżyć tylko wtedy określonego wyzwalacza tych limitów zostanie osiągnięty.

## <a name="custom-control-notes"></a>Uwagi dotyczące kontrolek niestandardowych

Można dodać do dowolnej kontrolki użytkownika lub formant niestandardowy; UpdatePanel jednak strony, na którym są uwzględnione te kontrolki należy również uwzględnić formantu ScriptManager z właściwością równa EnablePartialRendering **true**.

Jednym ze sposobów, w którym użytkownik może to uwzględniać podczas używania Kontrolki niestandardowe sieci Web jest zastąpienie chronionego `CreateChildControls()` metody `CompositeControl` klasy. W ten sposób może wprowadzać UpdatePanel między elementów podrzędnych kontrolki a światem zewnętrznym, jeśli okaże się, że strona obsługuje częściowe renderowanie; w przeciwnym razie po prostu umożliwia zastosowanie warstwy formantów podrzędnych w kontenerze `Control` wystąpienia.

## <a name="updatepanel-considerations"></a>Zagadnienia dotyczące kontrolki UpdatePanel

Kontrolki UpdatePanel działa jako coś czarnej, zawijanie ogłaszania zwrotnego ASP.NET, w kontekście XMLHttpRequest języka JavaScript. Istnieją jednak zagadnienia istotnie poprawiającą wydajność, należy pamiętać, zarówno pod względem zachowanie i szybkości. Aby zrozumieć, jak działa UpdatePanel tak, aby najlepiej zdecyduj, kiedy jego użycie jest odpowiednie, należy sprawdzić AJAX exchange. W poniższym przykładzie użyto istniejącej lokacji i, Mozilla Firefox z rozszerzeniem Firebug (Firebug przechwytuje dane o XMLHttpRequest).

Należy wziąć pod uwagę zawierającej, między innymi, kod pocztowy pola tekstowego, który ma wypełnić pole Miejscowość i województwo formularza lub formantu formularza. Ten formularz, ostatecznie zbiera informacje o członkostwie, w tym nazwę, adres i informacje kontaktowe. Istnieje wiele zagadnienia dotyczące projektowania, aby wziąć pod uwagę w oparciu o wymagania określonego projektu.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


W oryginalnym iteracji tej aplikacji kontrolki została opracowana sprzężone materiałami dane rejestracyjne użytkownika, w tym kod pocztowy, miasta i stanu. Opakowane w ramach UpdatePanel i upuszczone na formularz sieci Web całego kontrolki. Po wprowadzeniu przez użytkownika kod pocztowy UpdatePanel wykrywa zdarzenia (odpowiednie zdarzenie textchanged w zaplecza, określając wyzwalaczy lub przy użyciu zestawu ChildrenAsTriggers właściwości na wartość true). AJAX publikuje wszystkie pola w ramach kontrolki UpdatePanel, jak przechwycone przez FireBug (zobacz diagram po prawej stronie).

Jak wskazuje przechwytywania ekranu, są dostarczane wartości z każdej kontrolki UpdatePanel (w tym przypadku są puste), a także pole stanu widoku. Wszystkie told ponad 9kb danych są wysyłane, gdy w rzeczywistości tylko pięć bajtów danych były potrzebne do wykonania tego konkretnego żądania. Odpowiedź jest jeszcze bardziej przeglądarek: w sumie 57kb są wysyłane do klienta, wystarczy, aby zaktualizować pole tekstowe i pole listy rozwijanej.

Może to być również interesujące, aby zobaczyć, jak ASP.NET AJAX aktualizuje prezentacji. Część odpowiedzi UpdatePanel żądanie aktualizacji jest wyświetlany w oknie konsoli Firebug po lewej stronie; jest specjalnie formułować rozdzielonych potoku ciąg, który jest podzielony przez skrypt po stronie klienta, a następnie ponownie na stronie. W szczególności ustawia ASP.NET AJAX *innerHTML* właściwość elementu HTML na kliencie, który reprezentuje swoje UpdatePanel. Jak ponownie powodują generowanie modelu DOM, występuje niewielkie opóźnienie, w zależności od ilości informacji, które ma zostać przetworzony.

Ponowne generowanie modelu DOM wyzwala szereg dodatkowych problemów:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Jeśli w UpdatePanel wąsko zdefiniowany element HTML, utraci fokus. Dlatego dla użytkowników, którzy naciśnięty klawisz Tab aby zamknąć pole tekstowe kod pocztowy, miejsca docelowego następnego byłby pole tekstowe miasta. Jednak po odświeżeniu wyświetlania kontrolki UpdatePanel formularza nie jest już musiałaby fokus i klawisza Tab czy uruchomiono wyróżniania elementów fokus (np. linków).
- Jeśli dowolnego typu niestandardowego skryptu po stronie klienta jest używany, uzyskuje dostęp do elementów DOM, odwołania utrwalone przez funkcje może przestać unieczynnienia po odświeżeniu strony częściowe.

UpdatePanels nie mają być wychwytywania rozwiązania. Przeciwnie zapewniają szybkie rozwiązanie w przypadku niektórych sytuacjach, w tym prototypy, aktualizacje małych kontroli i zapewnić znany interfejs deweloperów platformy ASP.NET, które mogą być dobrze znanych z modelem obiektów platformy .NET, ale mniej, dzięki czemu z DOM. Istnieje kilka rozwiązań alternatywnych, które mogą spowodować lepszą wydajność, w zależności od scenariusza aplikacji:

- Należy rozważyć użycie PageMethods i JSON (JavaScript Object Notation) umożliwia deweloperom wywołanie metody statyczne, na stronie tak, jakby wywołano wywołanie usługi sieci web. Ponieważ te metody są statyczne, stan nie jest konieczne; obiekt wywołujący skrypt zawiera parametry, a wynik jest zwracany asynchronicznie.
- Należy rozważyć użycie usługi sieci Web i JSON, jeśli jeden formant musi być używana w kilku miejscach w całej aplikacji. To ponownie wymaga bardzo małego wysiłku specjalnych i działa w sposób asynchroniczny.

Dołączanie funkcje za pośrednictwem usług sieci Web lub strony metody ma również wady. Najpierw deweloperów platformy ASP.NET zazwyczaj mają tendencję do tworzenia składników małych funkcji do kontrolki użytkownika (plikach ascx). Strona metody nie mogą być hostowane w tych plikach; one muszą być obsługiwane w ramach klasy strony .aspx rzeczywistych. Podobnie, usług sieci Web musi być hostowany w obrębie klasy .asmx. W zależności od aplikacji Ta architektura może naruszać jednej zasady odpowiedzialności, w tym funkcje dla pojedynczego składnika teraz rozkłada się na dwóch lub więcej składników fizycznych, które mogą mieć niewielkiego lub żadnego spójnego ties.

Ponadto jeśli aplikacja wymaga, że UpdatePanels są używane, poniższe wskazówki powinien potrzeby rozwiązywania problemów i konserwacji.

- **Jak najmniejszy UpdatePanels zagnieżdżanie nie tylko w ramach jednostek, ale także na jednostki kodu.** Na przykład UpdatePanel na stronie, która otacza kontrolkę, a tej kontrolki zawiera też kontrolki UpdatePanel zawiera inna kontrolka, która zawiera UpdatePanel, jest zagnieżdżanie dla wielu jednostek. Pomaga to zachować Wyczyść elementy, które powinny być odświeżanie i zapobiega nieoczekiwany odświeżania do elementu podrzędnego UpdatePanels.
- **Zachowaj *ChildrenAsTriggers* właściwość ustawiony na wartość false i jawnie ustawić wyzwalanie zdarzeń.** Przy użyciu `<Triggers>` kolekcja jest znacznie bardziej zrozumiały sposób obsługi zdarzeń i może uniemożliwić nieoczekiwane zachowanie, obniżając równocześnie za pomocą zadania konserwacji i wymuszanie dewelopera do uczestnictwa w wydarzeniu.
- **Użyj najmniejszego możliwe, aby uzyskać funkcjonalność.** Jak wspomniano w dyskusji usługi kod pocztowy, zawijanie, że tylko absolutnego minimum skraca czas do serwera, łączna liczba przetwarzania i zasięgowi wymiana klient serwer, zwiększając wydajność.

## <a name="the-updateprogress-control"></a>Kontrolki UpdateProgress

## <a name="updateprogress-control-reference"></a>Kontrolki UpdateProgress odwołania

Włączone znaczników właściwości:

| **Nazwa właściwości** | **Typ** | **Opis** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | Określa identyfikator kontrolki UpdatePanel, który to UpdateProgress należy zgłaszać na. |
| DisplayAfter | int | Określa limit czasu w milisekundach, zanim ten formant jest wyświetlany po rozpoczęciu żądania asynchronicznego. |
| DynamicLayout | bool | Określa, czy postęp jest dynamicznie renderowane. |

Elementy podrzędne znaczników:

| **Tag** | **Opis** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Zawiera szablon kontrolki, ustaw dla zawartości, który będzie wyświetlany za pomocą tej kontrolki. |

Kontrolki UpdateProgress zapewnia środek opinię, aby zachować zainteresowania użytkowników podczas wykonywania wymaganych pracy do transportu do serwera. Może to pomóc użytkownikom dowiedzieć się, że wykonujesz coś mimo, że może nie być oczywista, szczególnie w przypadku, ponieważ większość użytkowników są używane do strony, odświeżanie i sprawdzając, zaznacz pasek stanu.

Jako notatki kontrolki UpdateProgress może znajdować się w dowolnym miejscu na hierarchię strony. Jednak w sytuacjach, w których zwrotu częściowe są inicjowane z elementu podrzędnego kontrolki UpdatePanel (gdzie UpdatePanel jest zagnieżdżona w innej kontrolki UpdatePanel) postbacks ten wyzwalacz elementu podrzędnego kontrolki UpdatePanel spowoduje UpdateProgress szablony mają być wyświetlane dla elementu podrzędnego Kontrolki UpdatePanel, jak i element nadrzędny kontrolki UpdatePanel. Jednak jeśli wyzwalacz jest element podrzędny elementu nadrzędnego kontrolki UpdatePanel, a następnie zostaną wyświetlone tylko szablony UpdateProgress skojarzony z obiektem nadrzędnym.

## <a name="summary"></a>Podsumowanie

Rozszerzenia Microsoft ASP.NET AJAX są zaawansowane produkty ułatwiających udostępnianie zawartości sieci web i aby zapewnić rozbudowane środowisko użytkownika do aplikacji sieci web. Jako część rozszerzenia AJAX programu ASP.NET, strona częściowa renderowanie kontrolek, łącznie z kontrolki Menedżera skryptów, UpdatePanel i UpdateProgress są niektóre z najbardziej widoczne składników zestawu narzędzi.

Składnika ScriptManager integruje się zapewnienie klienta JavaScript dla rozszerzenia, a także umożliwia różnych składników serwera i klienta współdziałały inwestycje związane z programowaniem minimalny.

Kontrolki UpdatePanel jest widoczne pole magic — znaczników w ramach kontrolki UpdatePanel mogą mieć Codebehind po stronie serwera i nie wyzwalać odświeżanie strony. Formantów UpdatePanel mogą być zagnieżdżane i może być zależny od kontrolek w inne UpdatePanels. Domyślnie UpdatePanels obsługiwać żadnych ogłaszania zwrotnego wywoływana przez ich elementów podrzędnych kontrolki, mimo że ta funkcja może być dostrojony, deklaratywne lub programowo.

Przy użyciu kontrolki UpdatePanel, deweloperzy należy pamiętać o jego wpływ na wydajność, które potencjalnie mogą wystąpić. Potencjalne alternatywne metody obejmują usługi sieci web i metody strony, jeśli projekt aplikacji należy rozważyć.

UpdateProgress, które kontrolki pozwala użytkownikowi na wiedzieć, że użytkownik lub użytkownik nie jest ignorowany i tle żądania powinien się dzieje podczas strona nie jest wykonywania żadnych czynności, aby odpowiedzieć na dane wejściowe użytkownika. Obejmuje również możliwość przerwać wyniki częściowe renderowanie.

Razem te narzędzia pomagają tworzenia bogatych i płynnych komfortu pracy serwera mniej widoczny dla użytkownika i mniej przerywania przepływu pracy.

## <a name="bio"></a>Biografia

Scott Cate pracował nad przy użyciu technologii Microsoft Web od 1997 r i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacji koncentruje się na rozwiązania programowe wiedzy opartych na. Scott można się skontaktować za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blog znajduje się na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
