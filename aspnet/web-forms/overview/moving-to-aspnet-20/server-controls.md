---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Formanty serwera | Dokumentacja firmy Microsoft
author: microsoft
description: "Platforma ASP.NET 2.0 zwiększa kontrolki serwera na wiele sposobów. W tym module omówione zostaną następujące czynności niektóre zmiany w architekturze sposób ASP.NET 2.0 i programu Visual Studio 200..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="server-controls"></a>Formanty serwera
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Platforma ASP.NET 2.0 zwiększa kontrolki serwera na wiele sposobów. W tym module omówione zostaną następujące czynności niektóre zmiany w architekturze sposób ASP.NET 2.0 i programu Visual Studio 2005 podchodzi do kontrolki serwera.


Platforma ASP.NET 2.0 zwiększa kontrolki serwera na wiele sposobów. W tym module omówione zostaną następujące czynności niektóre zmiany w architekturze sposób ASP.NET 2.0 i programu Visual Studio 2005 podchodzi do kontrolki serwera.

## <a name="view-state"></a>Stan widoku

Podstawowej zmiany w widoku stanu w programie ASP.NET 2.0 jest znacznego zmniejszenia rozmiaru. Należy wziąć pod uwagę stronę tylko formant kalendarza na nim. W tym miejscu jest w stanie widoku w ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Teraz w tym miejscu jest stanu widoku strony identyczne w programie ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Jest bardzo istotne zmiany i określania, czy stan widoku jest przenoszona i z powrotem przez sieć, ta zmiana umożliwia deweloperom do znacznego zwiększenia wydajności. Zmniejszenie rozmiaru stan widoku jest przede wszystkim ze względu na sposób możemy obsłużyć wewnętrznie. Należy pamiętać, ciąg zakodowany w widoku stanu jest Base64. Aby lepiej zrozumieć zmiany w widoku stanu w programie ASP.NET 2.0, ma teraz przyjrzeć się dekodowane wartości w powyższych przykładach.

Oto stan widoku 1.1 zdekodować:

[!code-css[Main](server-controls/samples/sample3.css)]

To może wyglądać nieco informacje, ale brak wzorca w tym miejscu. W programie ASP.NET 1.x, możemy pojedynczy znaki umożliwiający określenie typów danych i rozdzielane przy użyciu wartości &lt; &gt; znaków. "T" w przykładzie stan widoku powyżej reprezentuje Trzykolumnowa. Trzykolumnowa zawiera pary ArrayLists ("l" reprezentuje element ArrayList). Jeden z tych ArrayLists zawiera Int32 ("i") o wartości 1, a drugi zawiera inny Trzykolumnowa. Trzykolumnowa zawiera pary ArrayLists itp. Ważne jest, aby pamiętać jest używamy Triplets, które zawierają pary, nazywamy typy danych za pomocą litery i używamy &lt; i &gt; znaków jako ograniczników.

W programie ASP.NET 2.0 stan widoku dekodowane wygląda nieco inaczej.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Należy zauważyć ogromnych zmiana wygląd dekodowane widoku stanu. Ta zmiana ma kilka underpinnings architektury. Wyświetl stan w programie ASP.NET 1.x służący do serializowania danych LosFormatter. Nowa klasa ObjectStateFormatter używamy w 2.0. Ta klasa został opracowany z myślą ułatwiających serializacji i deserializacji stan widoku oraz stan formantu. (Stan formantu zostanie omówiona w następnej sekcji). Dostępne są następujące zmiany metody, za pomocą którego została wykonana serializacja i deserializacja wiele korzyści. Jednym z najbardziej znaczący jest fakt, że w przeciwieństwie do LosFormatter, który używa TextWriter, ObjectStateFormatter używa BinaryWriter. Dzięki temu ASP.NET 2.0 do przechowywania stanu widoku serię bajtów zamiast ciągów. Podjąć, na przykład, liczbą całkowitą. W ASP.NET 1.1 całkowitą wymagane 4 bajty stan widoku. W programie ASP.NET 2.0 tej samej liczby całkowitej wymaga tylko 1 bajt. Inne ulepszenia wprowadzono zmniejszyć ilość stan widoku, który jest przechowywany. Wartości daty/godziny, na przykład znajdują się teraz za pomocą TickCount zamiast ciągu.

Tak, jakby to wszystko nie ma wystarczającej ilości został zwrócić szczególną uwagę na fakt, jedną z największych konsumentów stan widoku w 1.x został DataGrid i podobnych kontrolek. Główną wadą formanty, takie jak DataGrid, których dotyczy stan widoku jest, że często zawierają duże ilości danych powtórzony. W programie ASP.NET 1.x, powtarzające informacji po prostu przechowywane przez i za pośrednictwem ponownie co w stanie widoku przeglądarek. W programie ASP.NET 2.0 używamy nowej klasy IndexedString do przechowywania tych danych. Jeśli ciąg jest powtarzany, po prostu przechowujemy token IndexedString i indeks w tabeli uruchomionego IndexedString obiektów.

## <a name="control-state"></a>Stan kontrolki

Jeden z głównych gripes, do których zastosowano deweloperów o stan widoku jest rozmiar dodać go do ładunku HTTP. Jak wcześniej wspomniano, jest jedną z największych konsumentów stan widoku formantu DataGrid. Aby uniknąć ogromnych ilości generowanych przez DataGrid stan widoku, wielu deweloperów wyłączona po prostu stan widoku dla tego formantu. Niestety tego rozwiązania nie zawsze dobrym pomysłem. Wyświetl stan w programie ASP.NET 1.x zawiera nie tylko dane niezbędne do funkcji poprawne formantu. Zawiera również informacje dotyczące stanu formantu interfejsu użytkownika. Oznacza to, że jeśli chcesz umożliwić podział na strony DataGrid, należy włączyć stan widoku, nawet jeśli nie potrzebujesz wszystkich danych interfejsu użytkownika, ten widok zawiera stan. Jest to all-or-nothing scenariusz.

W programie ASP.NET 2.0 stan kontrolki rozwiązuje problem dobrze za pośrednictwem wprowadzenie stan formantu. Stan kontrolki zawiera dane, które jest bezwzględnie konieczne, aby zapewnić prawidłowe funkcjonowanie formantu. W odróżnieniu od stan widoku nie można wyłączyć stan formantu. Dlatego jest ważne, że dane są przechowywane w stanie kontroli dokładnie jest kontrolowany.

> [!NOTE]
> Stan kontrolki jest trwały oraz stan widoku w \_ \_VIEWSTATE ukryte pole formularza.


Ten film jest przewodnik stan widoku oraz stan formantu.


![](server-controls/_static/image1.png)


[Otwórz wideo na pełnym ekranie](server-controls/_static/state1.wmv)


Kontrolki serwera do odczytu i zapisu do kontrolowania stanu, należy wykonać trzy czynności.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Krok 1: Wywołaj metodę RegisterRequiresControlState

Metoda RegisterRequiresControlState informuje o ASP.NET, czy formant ma zostać zachowany stan formantu. Trwa jeden argument typu formantu, który jest formant, który jest rejestrowany.

Należy pamiętać rejestracji nie jest trwały do innego żądania. W związku z tym ta metoda musi zostać wywołana na każde żądanie, jeśli formant ma utrwalić stanu formantu. Zaleca się, że w OnInit można wywołać metody.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Krok 2: Zastąp SaveControlState

Metoda SaveControlState zapisuje kontroli zmian stanu formantu od czasu ostatniego ogłaszanie zwrotne. Zwraca obiekt reprezentujący stan formantu.

## <a name="step-3-override-loadcontrolstate"></a>Krok 3: Zastąp LoadControlState

Metoda LoadControlState ładuje zapisany stan w formancie. Metoda przyjmuje jeden argument typu obiektu, który przechowuje zapisany stan formantu.

## <a name="full-xhtml-compliance"></a>XHTML pełna zgodność

Każdy deweloper sieci Web wie znaczenie standardów w aplikacji sieci Web. Aby zachować Środowisko deweloperskie opartych na standardach, ASP.NET 2.0 jest w pełni zgodne XHTML. W związku z tym wszystkie tagi są odtwarzane według standardów XHTML w przeglądarkach obsługujących język HTML 4.0 lub nowszej.

Definicja typu dokumentu w ASP.NET 1.1 został w następujący sposób:

[!code-html[Main](server-controls/samples/sample6.html)]

W programie ASP.NET 2.0 definicja typu dokumentu domyślnego, jest następujący:

[!code-html[Main](server-controls/samples/sample7.html)]

Jeśli wybierzesz, można zmienić zgodności XHML domyślny za pomocą węzła xhtmlConformance w pliku konfiguracji. Na przykład następujący węzeł w pliku web.config spowoduje zmianę zgodności XHTML na XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Jeśli wybierzesz, można również skonfigurować platformę ASP.NET do korzystania z konfiguracji starszych używane w programie ASP.NET 1.x w następujący sposób:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptacyjną renderowania przy użyciu karty

W programie ASP.NET 1.x, plik konfiguracji zawiera &lt;browserCaps&gt; sekcja, która wypełnione obiektu HttpBrowserCapabilities. Ten obiekt może dewelopera ustalić, jakiego urządzenia jest wprowadzenie poszczególnych żądań i odpowiednio renderowania kodu. W programie ASP.NET 2.0 model została ulepszona i używa teraz nową klasę ControlAdapter. Klasa ControlAdapter zastępuje zdarzenia w cyklu życia formantu i steruje renderowaniem formanty oparte na możliwości agenta użytkownika. Możliwości określonego agenta użytkownika są zdefiniowane w pliku definicji przeglądarki (plik z rozszerzeniem .browser) przechowywane w c:\windows\microsoft.net\framework\v2.0. \* \* \* \*\CONFIG\Browsers folderu.

> [!NOTE]
> Klasa ControlAdapter jest klasą abstrakcyjną.


Podobnie jak &lt;browserCaps&gt; części 1.x, pliku definicji przeglądarki używa wyrażenia regularnego, aby przeanalizować ciąg agenta użytkownika w celu identyfikacji przeglądarki. Go je definiuje możliwości określonego agenta użytkownika. ControlAdapter renderuje formantu za pomocą metody renderowania. W związku z tym jeśli przesłonięcia metody renderowanie, nie powinny wywoływać renderowania dla klasy podstawowej. W ten sposób może spowodować renderowania występuje dwa razy, raz dla karty i raz dla samego formantu.

## <a name="developing-a-custom-adapter"></a>Tworzenie kart niestandardowych

Możesz utworzyć własne niestandardowe karty przez dziedziczenie z ControlAdapter. Ponadto może dziedziczyć z klasy abstrakcyjnej PageAdapter w przypadkach, gdy karta jest potrzebne dla strony. Mapowanie kontrolek niestandardowych karty odbywa się za pośrednictwem &lt;controlAdapters&gt; elementu w pliku definicji przeglądarki. Na przykład następujący kod XML z pliku definicji przeglądarki mapuje klasy MenuAdapter formant Menu:

[!code-html[Main](server-controls/samples/sample10.html)]

Użycie tego modelu, staje się bardzo łatwe deweloperowi kontroli rozpoczęta dla określonego urządzenia lub przeglądarki. Jest również bardzo proste dla deweloperów mają pełną kontrolę nad sposób renderowania stron na każdym urządzeniu.

## <a name="per-device-rendering"></a>Renderowanie na urządzenie

Właściwości formantów serwera w programie ASP.NET 2.0 może być określony na urządzenie przy użyciu prefiksu specyficzne dla przeglądarki. Na przykład poniższy kod zmieni się tekst etykiety, w zależności od urządzenia, które są używane do przeglądania strony.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Podczas przeglądania strony zawierających tę etykietę z programu Internet Explorer, etykiety wyświetli tekst informujący o tym "Przeglądania z programu Internet Explorer." Podczas przeglądania strony z przeglądarki Firefox, etykiety wyświetli tekst "Przeglądania z Firefox." Podczas przeglądania strony z innych urządzeń, wyświetli "Przeglądania z nieznane urządzenie." Można określić dowolną właściwość przy użyciu tej składni specjalnych.

## <a name="setting-focus"></a>Ustawianie fokusu

Deweloperzy 1.x ASP.NET — często zadawane ustawiania początkowy fokus na określonego formantu. Na przykład na stronie logowania, warto mieć pole tekstowe nazwy użytkownika, uzyskiwanie fokusu po pierwszym załadowaniu strony. W programie ASP.NET wymagane 1.x w ten sposób zapisywania niektórych skryptu po stronie klienta. Mimo że takiego skryptu jest prosta zadań, nie jest już konieczne w programie ASP.NET 2.0 dzięki użyciu metoda SetFocus. Metoda SetFocus przyjmuje jeden argument wskazującą formant, który powinien zostać wyświetlony fokus. Ten argument może być identyfikator klienta formantu w postaci ciągu lub nazwa formantu serwera, jak dla obiektu formantu. Na przykład, aby ustawić początkowy fokus do formantu TextBox o nazwie txtUserID po pierwszym załadowaniu strony, Dodaj następujący kod do strony\_obciążenia:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--lub

[!code-csharp[Main](server-controls/samples/sample13.cs)]

Platforma ASP.NET 2.0 używa moduł obsługi Webresource.axd (omówionych wcześniej) do renderowania po stronie klienta funkcji, która ustawia fokus. Nazwa funkcji po stronie klienta jest formularz sieci Web\_AutoFocus, jak pokazano poniżej:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternatywnie można metodę fokus dla formantu ustaw początkowy fokus do formantu. Metoda fokus jest pochodną klasy formantu i jest dostępny dla wszystkich kontrolek ASP.NET 2.0. Istnieje również możliwość ustawić fokus do określonego formantu, gdy wystąpi błąd sprawdzania poprawności. Która zostanie omówiona w późniejszym modułu.

## <a name="new-server-controls-in-aspnet-20"></a>Nowe kontrolki serwera w programie ASP.NET 2.0

Poniżej przedstawiono nowe kontrolki serwera w programie ASP.NET 2.0. Firma Microsoft zostaną umieszczone w bardziej szczegółowo na niektórych z nich w późniejszym modułów.

## <a name="imagemap-control"></a>Formant ImageMap

Formant ImageMap umożliwia dodanie hotspotami do obrazu, który można zainicjować ogłaszanie zwrotne lub przejdź do adresu URL. Istnieją trzy typy punktów dostępowych dostępne; CircleHotSpot RectangleHotSpot i PolygonHotSpot. Punkty aktywne są dodawane za pomocą edytora kolekcji, w programie Visual Studio lub programowo w kodzie. Nie ma interfejsu użytkownika dostępnej dla rysowania punkty aktywne na obrazie. Deklaratywnie należy określić współrzędne i rozmiaru lub radius punktu aktywnego. Istnieje również nie wizualną reprezentację punkt aktywny w projektancie. Jeśli punkt aktywny jest skonfigurowany do przejdź do adresu URL, adres URL jest określony przez właściwość NavigateUrl elementu hotspot. W przypadku post kopii punkt aktywny, PostBackValue Właściwość pozwala przekazać ciąg w ogłaszanie zwrotne, które mogą być pobierane w kodzie po stronie serwera.


![Edytor kolekcji punkt aktywny w programie Visual Studio](server-controls/_static/image1.jpg)

**Rysunek 1**: Edytor kolekcji punkt aktywny w programie Visual Studio


## <a name="bulletedlist-control"></a>Formant listy BulletedList

Formant listy BulletedList znajduje się lista punktowana, które łatwo mogą być powiązane z danymi. Lista może zostać określona (numerowana) lub nieuporządkowaną za pomocą właściwości BulletStyle. Każdy element na liście jest reprezentowana przez obiekt elementu listy.


![Formant listy BulletedList w programie Visual Studio](server-controls/_static/image1.gif)

**Rysunek 2**: listy BulletedList sterowania w programie Visual Studio


## <a name="hiddenfield-control"></a>Formant pole ukryte

Formant pole ukryte dodaje ukryte pole formularza do strony, którego wartość jest dostępna w kodzie po stronie serwera. Wartość ukryte pole formularza zazwyczaj oczekuje się, zmieniają się między tworzy kopię post. Istnieje jednak możliwość złośliwy użytkownik może zmienić przed wartość publikowania Wstecz. W takim przypadku formantu pole ukryte będzie wywołaj zdarzenie ValueChanged. Jeśli masz poufnych informacji w formancie pole ukryte i chcesz upewnij się, że zostanie zmienione, powinna obsługiwać zdarzenia ValueChanged w kodzie.

## <a name="fileupload-control"></a>Formant przekazywaniem plików

Formant przekazywaniem plików w programie ASP.NET 2.0 umożliwia przekazywanie plików do serwera sieci Web za pośrednictwem strony ASP.NET. Ten formant jest podobna do klasy HtmlInputFile 1.x ASP.NET z kilkoma wyjątkami. W programie ASP.NET 1.x, zalecono sprawdzany PostedFile właściwości dla wartości null w celu ustalenia, jeśli były dobre pliku. Formant przekazywaniem plików w programie ASP.NET 2.0 dodaje nową właściwość HasFile służy do tego celu, a jest bardziej wydajne.

Właściwość PostedFile jest wciąż dostępna na potrzeby dostępu do obiektu HttpPostedFile, ale niektóre funkcje HttpPostedFile jest teraz dostępna leżą za pomocą formantu przekazywaniem plików. Na przykład, aby zapisać przekazany plik w programie ASP.NET 1.x, należy wywołać metodę SaveAs dla obiekt HttpPostedFile. Używanie formantu przekazywaniem plików w programie ASP.NET 2.0, spowodowałoby wywołanie Metoda SaveAs w samej kontrolce przekazywaniem plików.

Innej znaczące zmiany zachowania 2.0 (i prawdopodobnie najbardziej znaczących zmian) nie jest już konieczne załadowanie całego pliku przekazanego do pamięci przed zapisaniem. W 1.x, jest zapisywana każdego pliku, który został przekazany w całości do pamięci przed zapisaniem na dysku. Taka architektura zapobiega przekazywania dużych plików.

W programie ASP.NET 2.0, atrybut requestLengthDiskThreshold elementu httpRuntime można skonfigurować liczbę kilobajtów są przechowywane w buforze w pamięci przed zapisywana na dysku.

**WAŻNE**: dokumentacji MSDN (i w innym miejscu dokumentacji) określa, czy ta wartość jest w bajtach (nie w kilobajtach) i że wartość domyślna to 256. Wartość w rzeczywistości jest określone w kilobajtach, a wartość domyślna to 80. Dzięki użyciu wartości domyślnej równej 80 KB, Upewniamy się, że rozmiar buforu nie kończy na sterty dużego obiektu.

## <a name="wizard-control"></a>Kreator formantu

Jest dość często mogą wystąpić klienci z Próba zebrania informacji w serii "stron" paneli lub przekazując strony deweloperów platformy ASP.NET. Więcej często niż nie pozwala jest irytujące i jest czasochłonne. Kreator nowego formantu rozwiązuje problemy w celu umożliwienia liniowej i inne czynności za pomocą Kreatora interfejsu, który użytkownicy zapoznali się z. Kreator formantu przedstawia zwykłe formularze w serii kroków. Każdy krok jest określonego typu określonego przez właściwość StepType formantu. Typy kroku dostępne są następujące:

| **Typ kroku** | **Wyjaśnienie** |
| --- | --- |
| Auto | Kreator automatycznie określa typ kroku oparte na jej położenie w ramach hierarchii kroku. |
| Uruchamianie | Pierwszym krokiem, często używany do wyświetlania zawartości instrukcji wstępne. |
| Krok | Normalne kroku. |
| Zakończ | Ostatni krok, zwykle używana do prezentowania przycisk, aby zakończyć pracę kreatora. |
| Zakończenie | Wyświetla komunikat komunikacji powodzenie lub niepowodzenie. |

> [!NOTE]
> Kreator formantu przechowuje informacje o jego stan za pomocą stan kontrolki ASP.NET. W związku z tym EnableViewState właściwości można ustawić na wartość false, bez żadnych szkody.


Ten film jest wskazówki formantu kreatora.


![](server-controls/_static/image2.png)


[Otwórz wideo na pełnym ekranie](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Lokalizowanie formantu

Formant Lokalizuj przypomina formancie Literal. Jednak formant Lokalizuj ma **tryb** właściwość, która określa sposób renderowania kodu znaczników, które zostaną dodane do niego. Właściwość Mode obsługuje następujące wartości:

| **Tryb** | **Wyjaśnienie** |
| --- | --- |
| Transformacja | Kod znaczników jest przekształcana zgodnie z protokołem przeglądarki zgłoszenia żądania. |
| Przekazywanie | Kod znaczników jest renderowana — jest. |
| Kodowanie | Kod znaczników, który zostanie dodany do formantu jest zakodowany przy użyciu HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Element multiView i kontrolki widoku

W formancie MultiView działa jako kontener dla kontrolki widoku i formantu widoku pełni funkcję kontenera (podobnie jak w Panelu sterowania) dla innych formantów. Każdy widok w formancie MultiView jest reprezentowany przez jeden formant widoku. Kontrolki widoku na pierwszym MultiView jest widokiem 0, drugi widok 1, itp. Można przełączać widoki, określając wartość ActiveViewIndex formancie MultiView.

## <a name="substitution-control"></a>Kontrolki zastępczej

Kontrolki zastępczej jest używany w połączeniu z pamięci podręcznej programu ASP.NET. W przypadku, gdy chcesz korzystać z pamięci podręcznej, ale masz części strony, który musi zostać zaktualizowany na każde żądanie (innymi słowy, części strony, które są wykluczone z pamięci podręcznej) podstawienia stanowi doskonałe rozwiązanie. Formant nie faktycznie renderowania żadnych danych wyjściowych samodzielnie. Zamiast tego jest on powiązany z metody w kodzie po stronie serwera. Po zażądaniu strony ma zostać wywołana metoda i zwracany kod znaczników jest renderowany zamiast kontrolki zastępczej.

Powiązane kontrolki zastępczej metody jest określona za pomocą **MethodName** właściwości. Ta metoda musi spełniać następujące kryteria:

- Musi być metodą static (udostępnionych w VB).
- Przyjmuje jeden parametr typu Element HttpContext.
- Zwraca ciąg reprezentujący kod znaczników, który należy zastąpić kontrolki na stronie.

Kontrolki zastępczej nie ma uprawnienia do modyfikowania inne kontrolki na stronie, ale ma dostęp do bieżący element HttpContext za pośrednictwem jej parametr.

## <a name="gridview-control"></a>Kontrolki widoku siatki

Formant widoku GridView jest zastąpienie dla formantu DataGrid. Ten formant zostanie omówiona bardziej szczegółowo w późniejszym modułu.

## <a name="detailsview-control"></a>Formant widoku DetailsView

Kontrolki widoku szczegółów umożliwia do wyświetlania pojedynczego rekordu ze źródła danych i edytować lub usunąć. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="formview-control"></a>Formant FormView

Formant FormView służy do wyświetlenia interfejsu można skonfigurować pojedynczy rekord z źródła danych. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="accessdatasource-control"></a>Formant AccessDataSource

Formant AccessDataSource służy do bazy danych programu Access powiązania danych. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="objectdatasource-control"></a>Kontrolki ObjectDataSource

Kontrolki ObjectDataSource jest używana do obsługi architektury trójwarstwowej, tak aby formanty może być powiązany z danymi obiektem biznesowym warstwy środkowej w przeciwieństwie do modelu dwuwarstwowej gdzie wiązania kontrolek bezpośrednio ze źródłem danych. Będzie można omówiono bardziej szczegółowo w późniejszym modułu.

## <a name="xmldatasource-control"></a>Formantu XmlDataSource

Formantu XmlDataSource służy do tworzenia powiązań danych źródła danych XML. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="sitemapdatasource-control"></a>Formant SiteMapDataSource

Kontrola SiteMapDataSource zapewnia powiązania danych dla lokacji kontrolki oparte na mapy witryny sieci Web. Będzie można omówiono bardziej szczegółowo w późniejszym modułu.

## <a name="sitemappath-control"></a>SiteMapPath Control

Kontrolki ścieżki mapy witryny prezentuje serię zwanymi powszechnie nawigacyjnej łączy nawigacji. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="menu-control"></a>Formant menu

Formant Menu wyświetla menu dynamiczne przy użyciu DHTML. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="treeview-control"></a>TreeView — Kontrolka

TreeView — kontrolka służy do wyświetlania danych w widoku drzewa hierarchicznej. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="login-control"></a>Formant logowania

Kontrolka logowania udostępnia mechanizm do zalogowania się do witryny sieci Web. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="loginview-control"></a>Kontrolki widoku logowania

Formantu LoginView umożliwia wyświetlanie różnych szablonów ustalane na podstawie stanu logowania użytkownika. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="passwordrecovery-control"></a>Formant PasswordRecovery

Formant PasswordRecovery służy do pobierania zapomniane hasła przez użytkowników aplikacji ASP.NET. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="loginstatus"></a>LoginStatus

Kontrolki stanu logowania Wyświetla stan logowania użytkownika. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="loginname"></a>LoginName

Kontrolki nazwy logowania Wyświetla nazwy użytkownika, po którego jest się zalogowanym aplikacji ASP.NET. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard jest można skonfigurować kreatora, który daje użytkownikom możliwość tworzenia konta członkostwa ASP.NET do użycia w aplikacji ASP.NET. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="changepassword"></a>ChangePassword

Element ChangePassword formantu umożliwia użytkownikom zmianę hasła dla aplikacji ASP.NET. Jest on opisane bardziej szczegółowo w późniejszym modułu.

## <a name="various-webparts"></a>Różne składniki Web Part

Platforma ASP.NET 2.0 jest dostarczany z różnych składników Web Part. Te zostanie omówiona szczegółowo w późniejszym modułu.
