---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Formanty serwera | Dokumentacja firmy Microsoft
author: microsoft
description: Program ASP.NET 2.0 zwiększa formantów serwera na wiele sposobów. W tym module omówimy niektóre zmiany w architekturze sposób ASP.NET 2.0 i Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: ecf99fa894c1f662542aa8a613195b828bf2c67b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756302"
---
<a name="server-controls"></a>Formanty serwera
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Program ASP.NET 2.0 zwiększa formantów serwera na wiele sposobów. W tym module omówimy niektóre architektury zmiany w sposobie ASP.NET 2.0 i Visual Studio 2005 zajmuje się formantów serwera.


Program ASP.NET 2.0 zwiększa formantów serwera na wiele sposobów. W tym module omówimy niektóre architektury zmiany w sposobie ASP.NET 2.0 i Visual Studio 2005 zajmuje się formantów serwera.

## <a name="view-state"></a>Wyświetl stan

Podstawowej zmiany w widoku stanu w programie ASP.NET 2.0 jest znaczną redukcję rozmiaru. Należy wziąć pod uwagę strony za pomocą tylko kontrolki kalendarza na nim. Oto stan widoku w ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Teraz Oto stanu widoku na identyczna strona programu ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

To bardzo istotna zmiana, i biorąc pod uwagę, że stan widoku jest przenoszony i z powrotem przez sieć, ta zmiana umożliwiają deweloperom do znacznego zwiększenia wydajności. Zmniejszenie rozmiaru stan widoku jest głównie ze względu na sposób, w jaki firma Microsoft obsłużyć wewnętrznie. Należy pamiętać, widok stanu jest w formacie Base64 ciąg zakodowany. Aby lepiej zrozumieć zmiany w widoku stanu w programie ASP.NET 2.0, Przyjrzyjmy się przyjrzeć się dekodowane wartości z powyższych przykładach.

Poniżej przedstawiono stan widoku 1.1 zdekodowany:

[!code-css[Main](server-controls/samples/sample3.css)]

To mogą wyglądać nieco dziwny, ale występuje wzorzec, w tym miejscu. W programie ASP.NET: 1.x, firma Microsoft umożliwia identyfikowanie typów danych przez pojedyncze znaki i lista wartości, korzystając z &lt; &gt; znaków. W powyższym przykładzie stan widoku "t" reprezentuje trójkę. Trójkę zawiera parę ArrayLists ("g" reprezentuje element ArrayList). Jeden z tych ArrayLists zawiera wartości Int32 ("i") o wartości 1, a drugi zawiera inny trójkę. Trójkę zawiera parę ArrayLists itp. Ważne jest, aby pamiętać jest używamy Triplets, które zawierają pary, możemy zidentyfikować typy danych za pomocą litery i używamy &lt; i &gt; znaków w dwukropki.

W programie ASP.NET 2.0 stan widoku zdekodowany wygląda nieco inna.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Powinno być widocznych ogromna zmiana wyglądu stan zdekodowany widoku. Ta zmiana ma kilka underpinnings architektury. Wyświetl stan w programie ASP.NET: 1.x służący do serializowania danych LosFormatter. W wersji 2.0 używamy nową klasę ObjectStateFormatter. Ta klasa została zaprojektowana specjalnie o ułatwiające serializacji i deserializacji Wyświetl stan i stan kontrolki. (Stan formantu zostanie omówione w następnej sekcji). Istnieje wiele korzyści uzyskiwane dzięki zmiana formy, za pomocą którego serializacja i deserializacja mieć miejsce. Jednym z najbardziej znaczący jest fakt, że w przeciwieństwie do LosFormatter, który używa element TextWriter, ObjectStateFormatter używa BinaryWriter. Dzięki temu ASP.NET 2.0 do przechowywania stanu widoku serię bajtów zamiast ciągów. Podjąć, na przykład liczba całkowita. W ASP.NET 1.1 całkowitą wymagane 4 bajty stan widoku. W programie ASP.NET 2.0 w tej samej liczby całkowitej wymaga tylko 1 bajt. Inne ulepszenia wprowadzono może zmniejszyć ilość stan widoku, który jest przechowywany. Wartości daty/godziny, na przykład, są teraz przechowywane przy użyciu TickCount zamiast ciągu.

Tak, jakby to wszystko nie będzie wystarczająco dużo, został zwrócić szczególną uwagę na fakt, jeden z największych konsumentów stanu widoku w 1.x został DataGrid i podobnych kontrolek. Główną wadą kontrolek, takich jak DataGrid, których dotyczy stan widoku jest, że często zawierają duże ilości danych wielokrotnego. W programie ASP.NET: 1.x, powtarzające informacji po prostu została zapisana, nad oraz nad ponownie skutkuje stanu widoku przeglądarek. W programie ASP.NET 2.0 używamy nową klasę IndexedString do przechowywania tych danych. Jeśli ciąg jest powtarzany, po prostu przechowujemy token IndexedString i indeks w tabeli uruchomionego IndexedString obiektów.

## <a name="control-state"></a>Stan formantu

Jedną z głównych gripes, których deweloperzy o stanie widoku był rozmiar dodaną do ładunku HTTP. Jak wcześniej wspomniano, jedną z największych konsumentów stan widoku jest formant DataGrid. Aby uniknąć bardzo duże ilości stan widoku, generowane przez DataGrid, wielu deweloperów po prostu widok stanu wyłączonego dla tej kontrolki. Niestety tego rozwiązania nie zawsze dobrym. Wyświetl stan w programie ASP.NET: 1.x zawiera nie tylko danych niezbędnych poprawne działanie formantu. Zawiera także informacje dotyczące stanu kontrolki interfejsu użytkownika. Oznacza to, że aby umożliwić dzielenia na strony w elemencie DataGrid, należy włączyć stan widoku, nawet jeśli nie potrzebujesz wszystkich danych interfejsu użytkownika, ten widok zawiera stan. Jest to sztywnego scenariusz.

W programie ASP.NET 2.0 stan formantu rozwiązuje ten problem, dobrze za pośrednictwem wprowadzenie stan formantu. Stan formantu zawiera dane, które jest bezwzględnie konieczne, aby zapewnić prawidłowe funkcjonowanie formantu. W przeciwieństwie do stanu widoku nie można wyłączyć stan kontrolki. Dlatego jest ważne, że dane są przechowywane w stan formantu jest starannie kontrolowany.

> [!NOTE]
> Stan formantu jest trwały oraz stanu widoku w \_ \_VIEWSTATE ukrytego pola formularza.


To wideo jest przewodnik Wyświetl stan i stan kontrolki.


![](server-controls/_static/image1.png)


[Otwórz wideo pełnego ekranu](server-controls/_static/state1.wmv)


Dla kontrolki serwera do odczytu i zapisu do kontrolowania stanu, należy wykonać trzy czynności.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Krok 1: Wywołaj metodę RegisterRequiresControlState

Metoda RegisterRequiresControlState informuje ASP.NET, czy formant ma zostać zachowany stan formantu. Trwa jeden argument typu formantu, co jest formantem, który jest rejestrowany.

Należy pamiętać, rejestracja nie jest trwały do innego żądania. W związku z tym ta metoda musi zostać wywołana na każde żądanie, jeśli formant znajduje się na utrwalanie stanu kontrolki. Zaleca się, że w metody OnInit można wywołać metody.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Krok 2: Przesłoń SaveControlState

Metoda SaveControlState zapisuje kontroli zmian stanu kontrolki od ostatni wpis ponownie. Zwraca obiekt reprezentujący stan formantu.

## <a name="step-3-override-loadcontrolstate"></a>Krok 3: Przesłoń LoadControlState

Metoda LoadControlState ładuje zapisanego stanu w formancie. Ta metoda przyjmuje jeden argument typu obiektu, który przechowuje zapisany stan kontrolki.

## <a name="full-xhtml-compliance"></a>XHTML pełnej zgodności

Każdy deweloper sieci Web wie znaczenie standardów w aplikacjach sieci Web. Aby zachować środowisko programistyczne oparte na standardach, ASP.NET 2.0 jest w pełni zgodne XHTML. Dlatego wszystkie tagi są renderowanych zgodnie ze standardami XHTML w przeglądarkach obsługujących język HTML 4.0 lub nowszej.

Definicja typu dokumentu w ASP.NET 1.1 zakończyło się w następujący sposób:

[!code-html[Main](server-controls/samples/sample6.html)]

W programie ASP.NET 2.0 definicji typu dokumentu domyślnego, jest następujący:

[!code-html[Main](server-controls/samples/sample7.html)]

Jeśli wybierzesz, można zmienić domyślne XHML zgodności za pośrednictwem węzła xhtmlConformance w pliku konfiguracji. Na przykład następujący węzeł w pliku web.config będzie Zmień zgodności XHTML XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Jeśli wybierzesz, możesz również skonfigurować ASP.NET do korzystania z konfiguracji starszej wersji, używane w programie ASP.NET: 1.x w następujący sposób:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptacyjne, renderowanie przy użyciu karty

W programie ASP.NET 1.x i plik konfiguracji zawiera &lt;browserCaps&gt; wypełnione obiektu HttpBrowserCapabilities sekcji. Ten obiekt może zatem programistą, aby określić, jakiego urządzenia osiąga poszczególnych żądań i renderować kod odpowiednio. W programie ASP.NET 2.0 model tworzony i udoskonalany i używa teraz nową klasę ControlAdapter. Klasa ControlAdapter przesłania zdarzenia w cyklu życia formantu i steruje renderowaniem kontrolek, w zależności od możliwości agenta użytkownika. Możliwości określonego agenta użytkownika są definiowane przez plik definicji przeglądarki (plik z rozszerzeniem pliku .browser) przechowywane w c:\windows\microsoft.net\framework\v2.0. \* \* \* \*\CONFIG\Browsers folderu.

> [!NOTE]
> Klasa ControlAdapter jest klasą abstrakcyjną.


Podobnie jak &lt;browserCaps&gt; sekcji 1.x, plik definicji przeglądarki korzysta z wyrażenia regularnego, aby przeanalizować ciąg agenta użytkownika w celu zidentyfikowania przeglądarki. Jego ich definiuje konkretnymi możliwościami dla tego agenta użytkownika. ControlAdapter renderuje kontrolkę za pomocą metody renderowania. W związku z tym jeśli zastąpienie metody renderowania, nie powinien wywoływać renderowania w klasie bazowej. Może to spowodować renderowania występuje dwa razy, jeden raz dla karty i jeden raz dla samego formantu.

## <a name="developing-a-custom-adapter"></a>Tworzenie karty niestandardowej

Aby opracować niestandardowe karty, dziedziczenie z ControlAdapter. Ponadto może dziedziczyć z klasy abstrakcyjnej PageAdapter w przypadkach, gdy potrzebne są karty strony. Mapowanie kontrolki do karty niestandardowej odbywa się za pośrednictwem &lt;controlAdapters&gt; elementu w pliku definicji przeglądarki. Na przykład następujący kod XML z pliku definicji przeglądarki mapuje klasy MenuAdapter formant Menu:

[!code-html[Main](server-controls/samples/sample10.html)]

Przy użyciu tego modelu, staje się bardzo łatwe deweloperowi kontroli pod kątem określonego urządzenia lub przeglądarki. Jest również bardzo proste, deweloper ma pełną kontrolę nad jak renderowania stron na każdym urządzeniu.

## <a name="per-device-rendering"></a>Renderowanie na urządzenie

Właściwości kontrolek serwera ASP.NET w wersji 2.0 może być określony na poziomie urządzenia przy użyciu prefiksu specyficzne dla przeglądarki. Na przykład poniższy kod zmieni tekst etykiety, w zależności od urządzenia, które jest używane do przeglądania strony.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Podczas przeglądania strony zawierających tę etykietę z programu Internet Explorer, etykiety będą wyświetlane tekst informujący o tym, "Po przejściu z programu Internet Explorer." Podczas przeglądania strony w przeglądarce Firefox, etykiety będą wyświetlane tekst "Po przejściu w przeglądarce Firefox." Podczas przeglądania strony za pomocą innego urządzenia, wyświetli "Po przejściu z nieznanego urządzenia." Można określić dowolną właściwość przy użyciu tej składni specjalne.

## <a name="setting-focus"></a>Ustawienie fokusu

Deweloperów 1.x platformy ASP.NET — często zadawane jak ustawić początkowy fokus określonego formantu. Na przykład na stronie logowania, warto mieć ważniejsze, jeśli strona ładuje się najpierw polu tekstowym identyfikator użytkownika. W programie ASP.NET: 1.x, spowoduje to wymagane, zapisywania niektórych skryptu po stronie klienta. Mimo że utworzenia takiego skryptu jest prostym zadaniem, nie jest już konieczne w programie ASP.NET 2.0, dzięki metody SetFocus. Metoda SetFocus przyjmuje jeden argument wskazujący formant, który powinien zostać wyświetlony fokus. Ten argument może być identyfikator klienta kontrolki w postaci ciągu lub nazwę kontrolki serwera jako obiekt formantu. Na przykład, aby ustawić początkowy fokus do kontrolki TextBox wywołać txtUserID, jeśli strona ładuje się najpierw, Dodaj następujący kod do strony\_obciążenia:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--lub

[!code-csharp[Main](server-controls/samples/sample13.cs)]

Platformy ASP.NET 2.0 używa obsługi Webresource.axd (omówionych wcześniej) do renderowania po stronie klienta — funkcja, która ustawia fokus. Nazwa funkcji po stronie klienta jest formularz sieci Web\_AutoFocus, jak pokazano poniżej:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternatywnie można użyć metody koncentracji uwagi dla formantu można ustawić początkowy fokus do tego formantu. Metoda fokus pochodzi z klasy kontrolek i jest dostępny dla wszystkich kontrolek ASP.NET 2.0. Istnieje również możliwość ustawić fokus na określonej kontrolki, gdy wystąpi błąd sprawdzania poprawności. Które zostały omówione w nowszym module.

## <a name="new-server-controls-in-aspnet-20"></a>Nowe formanty serwera w programie ASP.NET 2.0

Poniżej przedstawiono nowe formanty serwera ASP.NET w wersji 2.0. Firma Microsoft zaczną się bardziej szczegółowo na niektórych z nich w późniejszym modułów.

## <a name="imagemap-control"></a>Kontrolka ImageMap

Kontrolka ImageMap zezwala na dodawanie obszarów nadmiernej aktywności do obrazu, który można zainicjować post ponownie lub przejdź do adresu URL. Istnieją trzy typy obszarów nadmiernej aktywności dostępne; CircleHotSpot RectangleHotSpot i PolygonHotSpot. Punkty aktywne są dodawane przy użyciu edytora kolekcji, w programie Visual Studio lub programowo w kodzie. Brak dostępnej bez interfejsu użytkownika dla rysowania hotspotami obrazu. Deklaratywne należy podać współrzędne i rozmiar lub radius punktem największej aktywności. Istnieje również nie wizualnej reprezentacji punkt aktywny w projektancie. Jeśli punkt aktywny jest skonfigurowany do przejdź do adresu URL, adres URL jest określony za pomocą właściwości NavigateUrl punktu aktywnego. W przypadku wpis Utwórz kopię informacji o hotspotach, PostBackValue Właściwość pozwala przekazać ciąg we wpisie Wstecz, który można pobrać w kodzie po stronie serwera.


![Punkt aktywny Edytor kolekcji w programie Visual Studio](server-controls/_static/image1.jpg)

**Rysunek 1**: punkt aktywny Edytor kolekcji w programie Visual Studio


## <a name="bulletedlist-control"></a>Kontrolka BulletedList

Kontrolka BulletedList jest listę punktowaną, które mogą być łatwo powiązane z danymi. Lista może zostać określona (numerowana) lub nieuporządkowane za pomocą właściwości BulletStyle. Każdy element na liście jest reprezentowany przez obiekt elementu listy.


![BulletedList sterowania w programie Visual Studio](server-controls/_static/image1.gif)

**Rysunek 2**: BulletedList sterowania w programie Visual Studio


## <a name="hiddenfield-control"></a>Formant HiddenField kontroli

Kontrolka formant HiddenField dodaje ukryte pole formularza do strony, którego wartość jest dostępna w kodzie po stronie serwera. Wartość ukrytego pola formularza zazwyczaj powinien nie ulega zmianie między post tworzy kopię. Jednak jest możliwe, że złośliwy użytkownik może zmienić przed wartość do opublikowania Wstecz. W takim przypadku kontrolka formant HiddenField zgłosi zdarzeń ValueChanged. Jeśli masz poufnych informacji w formancie pole ukryte, i chce mieć pewność, że pozostaje niezmieniony, powinna obsługiwać zdarzenia ValueChanged w kodzie.

## <a name="fileupload-control"></a>Kontrolka fileUpload

Kontrolka FileUpload w programie ASP.NET 2.0 umożliwia przekazywanie plików na serwerze sieci Web za pośrednictwem strony ASP.NET. Ten formant jest podobna do klasy HtmlInputFile 1.x platformy ASP.NET z pewnymi wyjątkami. W programie ASP.NET: 1.x, zalecano, że właściwość PostedFile można sprawdzić wartość null w celu ustalenia, czy istniały odpowiednie dobre pliku. Kontrolka FileUpload w programie ASP.NET 2.0 dodaje nową właściwość HasFile, można użyć w tym samym celu i nieco bardziej wydajne.

Właściwość PostedFile jest wciąż dostępna na potrzeby dostępu do obiektu HttpPostedFile, ale niektóre funkcje HttpPostedFile jest teraz dostępna wewnętrznie za pomocą kontrolki FileUpload. Na przykład, aby zapisać przekazanego pliku w programie ASP.NET: 1.x, wywołujesz metody Zapisz jako obiektu HttpPostedFile. Za pomocą kontrolki FileUpload programu ASP.NET 2.0, może wywołać metodę Zapisz jako w samej kontrolce FileUpload.

Inna ważna różnica w zachowanie 2.0 (i prawdopodobnie najbardziej znaczące zmiany) to, że nie jest już konieczne załadowanie całego pliku przekazanego do pamięci przed zapisaniem zmian. W 1.x, dowolny plik, który został przekazany zostanie zapisany w całości w pamięci przed zapisywana na dysku. Ta architektura zapobiega przekazywanie dużych plików.

W programie ASP.NET 2.0 atrybut requestLengthDiskThreshold elementu httpRuntime można skonfigurować liczbę kilobajtów są przechowywane w buforze pamięci przed zapisywana na dysku.

**WAŻNE**: Dokumentacja MSDN (i dokumentacji w innym miejscu) określa, że ta wartość jest w bajtach (nie w kilobajtach) i że wartość domyślna to 256. Wartość jest faktycznie określić w kilobajtach, a wartość domyślna to 80. Dzięki k 80 wartości domyślnej, Upewniamy się, że bufor nie znajdą się na stosie dużego obiektu.

## <a name="wizard-control"></a>Kreator kontrolki

Jest to dość często występują deweloperów platformy ASP.NET się próby zebrania informacji z serii "strony" przy użyciu paneli, lub przez przeniesienie między stronami. Więcej często niż nie pozwala jest irytujące i jest czasochłonne. Nowy formant kreatora rozwiązuje problemy, umożliwiając dla liniowych i nieliniowych kroków interfejs kreatora, który zna użytkowników. Formant Kreator przedstawia wejściowych formularzy w serii kroków. Każdy krok jest określonego typu określonego przez właściwość StepType formantu. Typy kroku dostępne są następujące:

| **Typ kroku** | **Wyjaśnienie** |
| --- | --- |
| Auto | Kreator automatycznie określi typ kroku, w zależności od jego pozycja w ramach hierarchii kroku. |
| Uruchamianie | Pierwszym krokiem, często używany do wyświetlania zawartości instrukcji wprowadzających. |
| Krok | Normalne krok. |
| Zakończ | Ostatnim krokiem, zwykle używany do wyświetlania przycisku, aby zakończyć pracę kreatora. |
| Wykonaj | Przedstawia informacje o wiadomości, komunikacja powodzenia lub niepowodzenia. |

> [!NOTE]
> Formant kreatora przechowuje informacje o stanie przy użyciu stanu kontrolki ASP.NET. W związku z tym można ustawić właściwości EnableViewState na wartość false, bez żadnych szczegółów.


To wideo jest przewodnik formantu kreatora.


![](server-controls/_static/image2.png)


[Otwórz wideo pełnego ekranu](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Lokalizowanie formantu

Kontrolka Lokalizuj przypomina formancie Literal. Jednak formant Lokalizuj ma **tryb** właściwość, która określa sposób renderowania kodu znaczników, który jest dodawany do niego. Właściwość trybu obsługuje następujące wartości:

| **Tryb** | **Wyjaśnienie** |
| --- | --- |
| Transformacja | Kod znaczników jest przekształcane zgodnie z protokołem przeglądarki wysyłającego żądanie. |
| Przekazywanie | Kod znaczników jest renderowane jako-to. |
| Kodowanie | Kod znaczników, który jest dodawany do kontrolki są kodowane przy użyciu HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView i kontrolki widoku

Kontrolki widoku wielu elementów działa jako kontener dla kontrolki widoku, a kontrolka widoku działa jako kontener (podobnie jak w Panelu sterowania) dla innych kontrolek. Każdy widok w kontrolki widoku wielu elementów jest reprezentowany przez jeden formant widoku. Na pierwszy formant MultiView widok jest widokiem 0, druga widoku 1, itp. Możesz przełączać widoki, określając wartość ActiveViewIndex kontrolki widoku wielu elementów.

## <a name="substitution-control"></a>Kontrolki zastępczej

Kontrolki zastępczej jest używany w połączeniu z pamięci podręcznej programu ASP.NET. W przypadkach, gdy chcesz móc korzystać z pamięci podręcznej, ale masz części strony, które muszą być aktualizowane na każde żądanie (innymi słowy, części strony, którzy są wykluczeni z pamięci podręcznej) składnik podstawienia zapewnia doskonałe rozwiązanie. Kontrolka faktycznie nie renderuje żadnych danych wyjściowych samodzielnie. Zamiast tego jest on powiązany z metody w kodzie po stronie serwera. Gdy strona jest pobierana, metoda jest wywoływana i zwracany kod znaczników jest renderowany zamiast kontrolki zastępczej.

Metoda, z którym powiązany jest kontrolki zastępczej jest określony za pośrednictwem **MethodName** właściwości. Ta metoda musi spełniać następujące kryteria:

- Musi być metodą statyczną (udostępniony w języku VB).
- Przyjmuje jeden parametr typu HttpContext.
- Zwraca ciąg reprezentujący kod znaczników, który należy zastąpić formantu na stronie.

Kontrolki zastępczej nie ma uprawnienia do modyfikowania dowolnej innej kontrolki na stronie, ale on mieć dostęp do bieżącego obiektu HttpContext przez jego parametr.

## <a name="gridview-control"></a>Kontrolki GridView

W kontrolce GridView jest zastępczych dla formantu DataGrid. Ten formant zostały omówione bardziej szczegółowo w nowszym module.

## <a name="detailsview-control"></a>Kontrolce DetailsView

W kontrolce DetailsView pozwala wyświetlić pojedynczy rekord ze źródła danych, a także edytować lub usunąć. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="formview-control"></a>Formant widoku FormView

Formant widoku FormView jest używany w pojedynczy rekord ze źródła danych można skonfigurować interfejsu. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="accessdatasource-control"></a>Kontrolka AccessDataSource

Kontrolka AccessDataSource jest używany do bazy danych programu Access powiązanie danych. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="objectdatasource-control"></a>Kontrolka ObjectDataSource

Kontrolka ObjectDataSource jest używana do obsługi architekturze trójwarstwowej, tak aby formantów może być powiązany z danymi obiektu biznesowego warstwy środkowej, w przeciwieństwie do modelu dwuwarstwowym gdzie wiązania kontrolek bezpośrednio ze źródłem danych. Jego zostanie dokładnie omówione bardziej szczegółowo w nowszym module.

## <a name="xmldatasource-control"></a>Kontrolki elementu XmlDataSource

Kontrolki elementu XmlDataSource służy do tworzenia powiązań danych źródła danych XML. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="sitemapdatasource-control"></a>Kontrolka SiteMapDataSource

Formant SiteMapDataSource zapewnia wiązania danych w witrynie nawigacji bazując na mapy witryny sieci Web. Jego zostanie dokładnie omówione bardziej szczegółowo w nowszym module.

## <a name="sitemappath-control"></a>SiteMapPath Control

Kontrolki ścieżki mapy witryny prezentuje serię łączy nawigacji, często określana jako linki do stron nadrzędnych. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="menu-control"></a>Formant menu

Formant Menu wyświetla menu dynamiczne, za pomocą DHTML. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="treeview-control"></a>TreeView — Kontrolka

TreeView — kontrolka jest używana do wyświetlania hierarchicznym widoku drzewa danych. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="login-control"></a>Kontrolka Login

Kontrolka Login udostępnia mechanizm do zalogowania się do witryny sieci Web. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="loginview-control"></a>Kontrolki widoku logowania

Kontrolki widoku logowania umożliwia wyświetlanie różne szablony, w zależności od stanu logowania użytkownika. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="passwordrecovery-control"></a>Kontrolka PasswordRecovery

Kontrolka PasswordRecovery służy do pobierania zapomniane hasła przez użytkowników aplikacji platformy ASP.NET. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="loginstatus"></a>Stanu logowania

Kontrolki stanu logowania Wyświetla stan logowania użytkownika. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="loginname"></a>LoginName

Kontrolki nazwy logowania Wyświetla nazwy użytkownika, po którego jest się zalogowanym aplikacji ASP.NET. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard jest można skonfigurować kreatora, który zapewnia użytkownikom możliwość tworzenia konta usługi członkostwa ASP.NET, do użycia w aplikacji ASP.NET. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="changepassword"></a>ChangePassword

Kontrolka ChangePassword umożliwia użytkownikom zmianę hasła dla aplikacji ASP.NET. Jego zostało opisane bardziej szczegółowo w nowszym module.

## <a name="various-webparts"></a>Różne składniki Web Part

Program ASP.NET 2.0 jest dostarczany z różnych składników Web Part. Te będzie można zostały szczegółowo opisane w nowszym module.
