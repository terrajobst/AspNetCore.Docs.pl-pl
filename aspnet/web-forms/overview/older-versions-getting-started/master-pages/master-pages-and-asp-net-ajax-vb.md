---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: Strony wzorcowe i ASP.NET AJAX (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym artykule omówiono opcje przy użyciu rozszerzeń ASP.NET AJAX i stron wzorcowych. Analizuje przy użyciu klasy serwera proxy menedżera skryptów; w tym artykule omówiono, jak różne pliki JS są ładowane dependi...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: aa511b8bd2f4d739cbe1f04b2a9cf03bf6928182
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756184"
---
<a name="master-pages-and-aspnet-ajax-vb"></a>Strony wzorcowe i ASP.NET AJAX (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> W tym artykule omówiono opcje przy użyciu rozszerzeń ASP.NET AJAX i stron wzorcowych. Analizuje przy użyciu klasy serwera proxy menedżera skryptów; w tym artykule omówiono, jak różne pliki JS są ładowane w zależności od tego, czy funkcja ScriptManager jest używany w bazie danych Master lub strony zawartości.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku lat deweloperzy coraz więcej ma zostały tworzenia aplikacji sieci web z włączoną obsługą technologii AJAX. Witryny sieci Web obsługujące technologie AJAX korzysta z wielu technologii pokrewnych w sieci web oferuje bardziej dynamiczne środowisko użytkownika. Tworzenie aplikacji platformy ASP.NET z włączoną obsługą technologii AJAX jest niezwykle proste dzięki strukturą ASP.NET AJAX firmy Microsoft. ASP.NET AJAX jest wbudowana w programie ASP.NET 3.5 i Visual Studio 2008; jest również dostępna do pobrania osobno dla aplikacji programu ASP.NET 2.0.

Podczas kompilowania z włączoną obsługą technologii AJAX stron sieci web za pomocą programu ASP.NET AJAX framework, należy dodać do każdej strony, która wykorzystuje platformę dokładnie jednego formantu ScriptManager. Jak sugeruje jej nazwa, funkcja ScriptManager zarządza skryptu po stronie klienta, używane na stronach sieci web z włączoną obsługą technologii AJAX. Jako minimum funkcja ScriptManager emituje kod HTML, który przekazuje przeglądarce instrukcje pobrania plików JavaScript tego korzeń biblioteki klienckiej AJAX programu ASP.NET. Może również służyć do zarejestrowania niestandardowych plików JavaScript, usług sieci web obsługujących przez skrypt i funkcjonalności usług aplikacji niestandardowych.

Jeśli Twoja witryna używa stron wzorcowych (zgodnie z oczekiwaniami), z nie koniecznie trzeba dodać formantu ScriptManager do każdej pojedynczej strony zawartości; przeciwnie można dodać formantu ScriptManager strony wzorcowej. W tym samouczku przedstawiono sposób dodawania formantu ScriptManager strony wzorcowej. Monitoruje również sposób zarejestrować niestandardowych skryptów i usług skryptu w określonej strony zawartości przy użyciu kontrolki serwera proxy menedżera skryptów.

> [!NOTE]
> W tym samouczku nie eksplorować, projektowania i tworzenia aplikacji sieci web z włączoną obsługą technologii AJAX przy użyciu framework ASP.NET AJAX. Aby uzyskać więcej informacji na temat korzystania z technologii AJAX zapoznaj się z ASP.NET AJAX filmy wideo i samouczki, a także zasobów wymienionych w sekcji dalsze informacje na końcu tego samouczka.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Badanie znaczników wyemitowane przez formantu ScriptManager

Formantu ScriptManager emituje kod znaczników, który powoduje, że przeglądarki, aby pobrać pliki JavaScript tego korzeń biblioteki klienckiej AJAX programu ASP.NET. Dodaje także znacznej liczby liniowy JavaScript do strony, która inicjuje tej biblioteki. Następujący kod przedstawia zawartość, która jest dodawana do wyniku renderowania strony, która obejmuje formantu ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

`<script src="url"></script>` Tagi poinstruować przeglądarki, aby pobrać i uruchomić plik JavaScript na *adresu url*. Funkcja ScriptManager emituje trzech takich znaczników; odwołuje się do jednego pliku `WebResource.axd`, a pozostałe dwa odwołania pliku `ScriptResource.axd`. Te pliki nie istnieją faktycznie jako pliki w witrynie sieci Web. Po odebraniu żądania dla dowolnej z tych plików na serwerze sieci web, aparatu ASP.NET sprawdza, czy ciąg zapytania i zwraca odpowiednią zawartość JavaScript. Skryptu podane przez te trzy zewnętrzne pliki JavaScript tworzą biblioteki klienta platformę ASP.NET AJAX. Druga `<script>` tagi wyemitowane przez funkcja ScriptManager zawierają wbudowany skrypt, który inicjuje tej biblioteki.

Odwołania do skryptu zewnętrznego i wyemitowane przez funkcja ScriptManager wykonanie wbudowanego skryptu są istotne dla strony, która wykorzystuje platformę ASP.NET AJAX, ale nie jest wymagany dla stron, które nie używają programu framework. W związku z tym przyczyny, jest idealnym rozwiązaniem do Menedżera skryptów należy dodawać tylko do tych stron, które korzystaj z platformy ASP.NET AJAX. Jest to wystarczające, a jeśli masz wiele stron, które korzystają z programu framework będzie znajdą Dodawanie formantu ScriptManager do wszystkich stron — powtarzających się zadań, powiedzieć najmniejszej. Alternatywnie możesz dodać ScriptManager strony wzorcowej następnie wprowadza ten skrypt niezbędne do wszystkich stron zawartości. W przypadku tej metody nie trzeba pamiętać dodać element ScriptManager do nowej strony, która używa platformę ASP.NET AJAX, ponieważ jest już uwzględniony przez stronę wzorcową. Krok 1 przeszukiwania przez proces dodawania ScriptManager strony wzorcowej.

> [!NOTE]
> Jeśli planujesz w tym funkcji interfejsu AJAX w interfejsie użytkownika strony wzorcowej, możesz nie mieć wyboru w sprawie — funkcja ScriptManager musi zawierać na stronie głównej.


Jedną wadą Dodawanie funkcja ScriptManager strony wzorcowej jest, że powyższy skrypt jest emitowane w *co* stronę, niezależnie od tego, czy jego potrzebne. Prowadzi to wyraźnie nieużywanego przepustowości dla tych stron, które ScriptManager uwzględniony (przy użyciu strony wzorcowej), ale nie należy używać żadnych funkcji platformę ASP.NET AJAX. Ale dwunasty zostanie zmarnowane przepustowości?

- Rzeczywista zawartość wyemitowane przez ScriptManager (jak pokazano powyżej) sumuje nieco ponad 1KB.
- Trzy pliki skryptu zewnętrznego odwołuje się `<script>` elementu, obejmuje jednak około 450 KB danych bez kompresji; w witrynie internetowej, która używa kompresji gzip, można zmniejszyć tego całkowitej przepustowości prawie 100 KB. Jednak te pliki skryptów są buforowane przez przeglądarkę dla jednego roku, co oznacza, że muszą zostać pobrane raz, a następnie można będzie wykorzystać ponownie w innych stron w witrynie.

W najlepszy przypadek następnie, kiedy pliki skryptów są buforowane, całkowity koszt jest 1KB, co jest niewielki. W najgorszym przypadku jednak — czyli po pliki skryptów nie została jeszcze pobrana i serwera sieci web jest nie za pomocą dowolnej formie kompresji — trafień przepustowości jest około 450KB, który można dodać w dowolnym miejscu z sekundy lub dwóch za pośrednictwem połączenia szerokopasmowego maksymalnie minutę dla  użytkowników za pośrednictwem modemów. Dobra wiadomość jest taka, ponieważ pliki skryptu zewnętrznego są buforowane przez przeglądarkę, scenariusz najgorszego przypadku występuje rzadko.

> [!NOTE]
> Jeśli nadal niepewne, umieszczając formantu ScriptManager w stronę wzorcową, należy wziąć pod uwagę formularz sieci Web ( `<form runat="server">` znaczników na stronie głównej). Każdej strony ASP.NET, która używa modelu zwrotu musi zawierać dokładnie jeden formularz sieci Web. Dodawanie formularza sieci Web dodaje dodatkową zawartość: liczba pól formularza, `<form>` tag, i w razie potrzeby JavaScript funkcji inicjowania zwrotu ze skryptu. Ten kod znaczników jest wykorzystywana dla stron, które nie ogłaszanie zwrotne. Ten kod nadmiarowe znaczników można wyeliminować, usuwając formularz sieci Web z strony wzorcowej i ręczne dodanie do każdej strony zawartości, które tego wymagają. Jednak korzyści formularz sieci Web na stronie głównej przeważają wady trzeba dodać niepotrzebnie do niektórych stron zawartości.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Krok 1: Dodawanie formantu ScriptManager do strony wzorcowej

Każdej strony sieci web, który używa struktury ASP.NET AJAX musi zawierać dokładnie jeden formantu ScriptManager. Ze względu na to wymaganie zazwyczaj warto umieścić jednego formantu ScriptManager na stronie wzorcowej tak, aby wszystkie strony zawartości mają formantu ScriptManager automatycznie dołączane. Ponadto funkcja ScriptManager musi występować przed każdą formanty serwera ASP.NET AJAX, takich jak kontrolki UpdatePanel i UpdateProgress. Dlatego najlepiej umieścić ScriptManager przed wszystkimi formantami ContentPlaceHolder w obrębie formularza sieci Web.

Otwórz `Site.master` strony wzorcowej i dodawanie formantu ScriptManager do strony w formularzu sieci Web, lecz przed `<div id="topContent">` — element (patrz rysunek 1). Jeśli używasz programu Visual Web Developer 2008 lub Visual Studio 2008 formantu ScriptManager znajduje się w przyborniku, na karcie rozszerzenia AJAX. Jeśli używasz programu Visual Studio 2005, należy najpierw zainstalować platformę ASP.NET AJAX i dodać formanty do przybornika. Odwiedź stronę pobierania ASP.NET AJAX, aby uzyskać platformę ASP.NET w wersji 2.0.

Po dodaniu funkcja ScriptManager do strony, należy zmienić jego `ID` z `ScriptManager1` do `MyManager`.


[![Dodaj funkcja ScriptManager do strony wzorcowej](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Rysunek 01**: Dodaj funkcja ScriptManager do strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Krok 2: Za pomocą struktury ASP.NET AJAX, ze strony zawartości

Za pomocą formantu ScriptManager dodał do strony głównej można teraz dodamy funkcje struktury ASP.NET AJAX do dowolnej strony zawartości. Utwórzmy nowej strony programu ASP.NET, która wyświetla wybrany produkt z bazy danych Northwind. Użyjemy kontrolka czasomierza platformę ASP.NET AJAX do aktualizacji na tym ekranie co 15 sekund, przedstawiający nowy produkt.

Rozpocznij od utworzenia nowej strony w katalogu głównym o nazwie `ShowRandomProduct.aspx`. Nie zapomnij o tej nowej strony, aby powiązać `Site.master` strony wzorcowej.


[![Dodawanie nowej strony programu ASP.NET do witryny sieci Web](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Rysunek 02**: Dodawanie nowej strony programu ASP.NET do witryny internetowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image6.png))


Pamiętamy Określanie tytułu, tagów Meta i innych nagłówków HTML, w tym samouczku strony wzorcowej [SKM1] utworzyliśmy niestandardowe strony podstawowej klasy o nazwie `BasePage` , generowane tytuł strony, jeśli nie została jawnie ustawiona. Przejdź do `ShowRandomProduct.aspx` kodem strony klasy i jest pochodną `BasePage` (zamiast z `System.Web.UI.Page`).

Na koniec zaktualizuj `Web.sitemap` plik, aby dołączyć wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` wzorca do interakcji z zawartością strony lekcji:


[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

Dodanie tego `<siteMapNode>` element jest widoczny w lekcji listy (zobacz rysunek 5).

### <a name="displaying-a-randomly-selected-product"></a>Wyświetlanie losowo wybrany produkt

Wróć do `ShowRandomProduct.aspx`. Z projektanta, przeciągnij kontrolki UpdatePanel z przybornika do `MainContent` formantu zawartości i ustaw jego `ID` właściwość `ProductPanel`. Kontrolki UpdatePanel reprezentuje region na ekranie, które mogą być aktualizowane asynchronicznie za pośrednictwem ogłaszania zwrotnego strony częściowe.

Naszym pierwszym zadaniem jest aby wyświetlić informacje o losowo wybranych produktów w obrębie kontrolki UpdatePanel. Rozpocznij, przeciągając kontrolki widoku szczegółów do kontrolki UpdatePanel. Ustaw kontrolce DetailsView `ID` właściwości `ProductInfo` i wyczyszczenie jego `Height` i `Width` właściwości. Rozwiń DetailsView tagu inteligentnego, a następnie z listy rozwijanej wybierz źródło danych, wybierz powiązać DetailsView z kontrolką SqlDataSource o nazwie `RandomProductDataSource`.


[![Powiązywanie DetailsView nowej kontrolki SqlDataSource](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Rysunek 03**: powiązywanie DetailsView nowej kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image9.png))


Konfigurowanie kontrolki SqlDataSource nawiązywania połączenia z bazą danych Northwind za pośrednictwem `NorthwindConnectionString` (które utworzyliśmy w interakcja ze stroną wzorcową z samouczka strony zawartości [SKM2]). Podczas konfigurowania instrukcji select chce określić niestandardową instrukcję SQL, a następnie wprowadź następujące zapytanie:


[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

`TOP 1` — Słowo kluczowe w `SELECT` klauzula zwraca tylko pierwszy rekord zwróconych przez zapytanie. `NEWID()` Funkcja generuje nową wartość Unikatowy identyfikator globalny (GUID) i mogą być używane w `ORDER BY` klauzuli, która zwraca rekordy w tabeli w kolejności losowej.


[![Konfigurowanie SqlDataSource do zwrócenia pojedynczego, wybrany rekord](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Rysunek 04**: Konfigurowanie SqlDataSource zwracać jeden losowo wybrany rekord ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image12.png))


Po zakończeniu działania kreatora programu Visual Studio tworzy elementu BoundField dla dwóch kolumn będących jej zwróconych przez zapytanie powyżej. W tym momencie oznaczeniu deklaracyjnym strony powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Rysunek 5. pokazuje `ShowRandomProduct.aspx` stronie podczas przeglądania za pośrednictwem przeglądarki. Kliknij przycisk Odśwież w przeglądarce, aby ponownie załadować stronę; powinien zostać wyświetlony `ProductName` i `UnitPrice` wartości dla nowego rekordu losowo wybrany.


[![Jest wyświetlana nazwa i cena losowe produktu](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Rysunek 05**: Nazwa i cena produktu losowe jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automatyczne wyświetlanie nowego produktu co 15 sekund

Struktura ASP.NET AJAX obejmuje kontrolka czasomierza, który wykonuje ogłaszania zwrotnego o określonej godzinie; na ogłaszanie zwrotne czasomierza `Tick` zdarzenie jest wywoływane. Kontrolka czasomierza jest umieszczony w obrębie UpdatePanel wyzwala strona częściowa odświeżenie strony, w którym możemy ponownie powiązać dane DetailsView, aby wyświetlić nowy, losowo wybranego produktu.

Aby to zrobić, przeciągnij czasomierz z przybornika i upuść go w UpdatePanel. Zmień czasomierza `ID` z `Timer1` do `ProductTimer` i jego `Interval` właściwość 60000 15000. `Interval` Właściwość wskazuje liczbę milisekund między ogłoszeniami wstecznymi; ustawienie jej 15000 powoduje, że czasomierza wyzwolić zwrotu strona częściowa co 15 sekund. W tym momencie usługi czasomierza oznaczeniu deklaracyjnym powinien wyglądać podobny do następującego:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Utwórz procedurę obsługi zdarzeń dla czasomierza `Tick` zdarzeń. W tej obsługi zdarzeń, będziemy musieli ponownie powiązać dane DetailsView przez wywołanie metody DetailsView `DataBind` metody. To powoduje, że DetailsView ponownie pobrać dane z kontroli źródła danych, co spowoduje zaznaczyć i wyświetlić nowy, losowo wybrany rekord (podobnie jak podczas ponownego ładowania strony przez kliknięcie przycisku Odśwież w przeglądarce).


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

To wszystko jest do niego! Ponownie stronę za pośrednictwem przeglądarki. Początkowo wyświetlane są informacje losowe produktu. Możesz obejrzeć cierpliwie ekranu można zauważyć, że po 15 sekundach informacji na temat nowego produktu magiczny sposób zastępuje istniejące wyświetlania.

Aby lepiej widzieć, co się dzieje, Dodajmy kontrolkę typu etykieta do kontrolki UpdatePanel wyświetlającą czas, wyświetlanie ostatniej aktualizacji. Dodaj kontrolkę etykiety w sieci Web w obrębie kontrolki UpdatePanel, ustaw jego `ID` do `LastUpdateTime`i usuń zaznaczenie jej `Text` właściwości. Następnie należy utworzyć procedurę obsługi zdarzeń dla kontrolki UpdatePanel `Load` zdarzeń i wyświetlanie bieżący czas w etykiecie. (UpdatePanel `Load` zdarzenie jest wywoływane na każdy ogłaszania zwrotnego strony pełnej lub częściowej.)


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Dzięki tej zmianie pełna strona zawiera czas wyświetlonego produktu została załadowana. Rysunek 6 przedstawia stronę po raz pierwszy odwiedzony. Rysunek 7 przedstawia stronę 15 sekund później po kontrolka czasomierza ma "wpisać" i kontrolki UpdatePanel zostały odświeżone do wyświetlania informacji na temat nowych produktów.


[![Losowo wybrany produkt jest wyświetlany podczas ładowania strony](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Rysunek 06**: losowo wybrany produkt jest wyświetlany podczas ładowania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image18.png))


[![Co 15 sekund, wyświetlany jest nowy losowo wybrany produkt](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Rysunek 07**: co 15 sekund jest wyświetlany nowy losowo wybrany produkt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Krok 3: Używanie kontrolki serwera proxy menedżera skryptów

Funkcja ScriptManager wraz z tym konieczne skryptu dla platformy ASP.NET AJAX biblioteki klienta, również zarejestrować niestandardowych plików JavaScript odwołań do usług sieci Web z obsługą skryptów i niestandardowego uwierzytelniania, autoryzacji i usług profilu. Zwykle takie dostosowania są specyficzne dla konkretnej strony. Jednakże jeśli pliki niestandardowego skryptu, odwołania do usługi sieci Web, lub uwierzytelniania, autoryzacji lub profilu usługi odwołuje się funkcja ScriptManager na stronie głównej następnie są one uwzględnione na wszystkich stronach w witrynie sieci Web.

Aby dodać powiązane ScriptManager dostosowania dla strony strona formant serwera proxy menedżera skryptów. Można dodać serwera proxy menedżera skryptów do strony zawartości, a następnie Zarejestruj niestandardowy plik JavaScript, odwołanie do usługi sieci Web, lub uwierzytelniania, autoryzacji lub usługi profilu z serwera proxy menedżera skryptów; ma to wpływ rejestrowania tych usług do określonej strony zawartości.

> [!NOTE]
> Strony ASP.NET może zawierać tylko co najwyżej jeden formantu ScriptManager obecne. W związku z tym nie można dodać formantu ScriptManager do strony zawartości, jeśli formantu ScriptManager już jest zdefiniowana na stronie głównej. Wyłącznie do celów serwera proxy menedżera skryptów jest sposób deweloperzy mogą zdefiniować funkcja ScriptManager na stronie głównej, ale nadal mieć możliwość dodawania ScriptManager dostosowań na podstawie strony strona.


Aby wyświetlić formant serwera proxy menedżera skryptów w działaniu, możemy rozszerzyć UpdatePanel w `ShowRandomProduct.aspx` obejmujący przycisk, który używa skryptów po stronie klienta, aby wstrzymać lub wznowić kontrolka czasomierza. Kontrolka czasomierza ma trzy metody po stronie klienta, które możemy użyć, aby osiągnąć ten odpowiednich funkcji:

- `_startTimer()` -rozpoczyna się kontrolka czasomierza
- `_raiseTick()` -powoduje, że kontrolka czasomierza, aby "znaczników", a tym samym odświeżania i wywoływanie zdarzenie taktu cyklicznie na serwerze
- `_stopTimer()` -Zatrzymuje kontrolka czasomierza

Utwórz plik języka JavaScript przy użyciu zmiennej o nazwie `timerEnabled` i funkcję o nazwie `ToggleTimer`. `timerEnabled` Zmiennej wskazuje, czy kontrolka czasomierza jest aktualnie włączone lub wyłączone; jego wartość domyślna to true. `ToggleTimer` Funkcja przyjmuje dwa parametry wejściowe: odwołanie do przycisku wstrzymywanie i wznawianie i po stronie klienta `id` wartość kontrolki czasomierza. Funkcja ta wartość Włącza/wyłącza `timerEnabled`, pobiera odwołanie do formantu czasomierza, uruchomienia lub zatrzymania czasomierza (w zależności od wartości `timerEnabled`) i aktualizuje tekst wyświetlany na przycisku "Wstrzymaj" lub "Resume". Ta funkcja będzie wywoływana zawsze wtedy, gdy kliknięto przycisk wstrzymywanie i wznawianie.

Najpierw utwórz nowy folder w witrynie sieci Web o nazwie `Scripts`. Następnie dodaj nowy plik do folderu skryptów o nazwie `TimerScript.js` typu plik języka JScript.


[![Dodaj nowy plik JavaScript na Folder skryptów](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Rysunek 08**: Dodaj nowy plik JavaScript do `Scripts` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image24.png))


[![Nowy plik JavaScript został dodany do witryny sieci Web](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Rysunek 09**: nowy plik JavaScript został dodany do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image27.png))


Następnie dodaj następujący skrypt służy do `TimerScript.js` pliku:


[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

Teraz musisz zarejestrować tego niestandardowego pliku JavaScript w `ShowRandomProduct.aspx`. Wróć do `ShowRandomProduct.aspx` i dodać kontrolki serwera proxy menedżera skryptów do strony; ustaw jego `ID` do `MyManagerProxy`. Aby zarejestrować niestandardowych skryptów JavaScript plik wybierz formant serwera proxy menedżera skryptów w Projektancie i przejdź do okna właściwości. Jedna z właściwości jest zatytułowana skryptów. Wybranie tej właściwości powoduje wyświetlenie edytora kolekcji ScriptReference pokazano na rysunku nr 10. Kliknij przycisk Dodaj, aby uwzględnić nowe odwołania do skryptu, a następnie wprowadź ścieżkę do pliku skryptu we właściwości ścieżki: `~/Scripts/TimerScript.js`.


[![Dodaj odwołanie skryptu do kontrolki serwera proxy menedżera skryptów](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Na rysunku nr 10**: Dodaj odwołanie skryptu do kontrolki serwera proxy menedżera skryptów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image30.png))


Po Dodawanie odwołania do skryptu serwera proxy menedżera skryptów kontrolki użytkownika deklaratywne znaczników została zaktualizowana do `<Scripts>` kolekcji za pomocą jednego `ScriptReference` wpisu jako następujący fragment kodu znaczników przedstawia:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

`ScriptReference` Wpisu powoduje, że serwera proxy menedżera skryptów, które mają zostać objęte odwołanie do pliku JavaScript jego renderowanego kodu znaczników. Oznacza to, rejestrując niestandardowy skrypt w serwera proxy menedżera skryptów `ShowRandomProduct.aspx` wyniku renderowania strony zawiera teraz innego `<script src="url"></script>` tagu: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Teraz możemy wywołać `ToggleTimer` funkcję zdefiniowaną w `TimerScript.js` ze skryptem klienta w `ShowRandomProduct.aspx` strony. Dodaj poniższy kod HTML w ramach kontrolki UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Spowoduje to wyświetlenie przycisk z tekstem "Wstrzymaj". Zawsze, gdy kliknięciu, funkcja JavaScript `ToggleTimer` jest wywoływana, przekazując odwołanie do przycisku i `id` wartość kontrolki czasomierza (`ProductTimer`). Należy pamiętać, składnia uzyskania `id` wartość kontrolki czasomierza. `<%=ProductTimer.ClientID%>` emituje wartość `ProductTimer` kontrolka czasomierza `ClientID` właściwości. W nazwach Identyfikator kontrolki w samouczku strony z zawartością [SKM3] omówiono różnice między po stronie serwera `ID` wartość i wynikowy klienta `id` wartości i w jaki sposób `ClientID` zwraca po stronie klienta `id`.

Na ilustracji 11 pokazano tej strony, gdy pierwszy odwiedzony za pośrednictwem przeglądarki. Czasomierz są uruchomione i aktualizuje informacje wyświetlane produktu co 15 sekund. Rysunek 12 pokazuje ekran, po kliknięciu przycisk Wstrzymaj. Klikając przycisk Wstrzymaj zatrzymuje Timer i aktualizuje tekst przycisku "Resume". Informacje o produkcie odświeżania (i kontynuować odświeżanie co 15 sekund) po użytkownik kliknie przycisk Wznów.


[![Kliknij przycisk Wstrzymaj, aby zatrzymać kontrolka czasomierza](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Rysunek 11**: kliknij przycisk Wstrzymaj, aby zatrzymać kontrolka czasomierza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image33.png))


[![Kliknij przycisk Wznów, aby ponownie uruchomić czasomierza](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Rysunek 12**: kliknij przycisk Wznów, aby ponownie uruchomić czasomierza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image36.png))


## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci web z włączoną obsługą technologii AJAX przy użyciu platformy ASP.NET AJAX jest konieczne, że każdej strony sieci web z włączoną obsługą technologii AJAX obejmują formantu ScriptManager. Aby ułatwić ten proces, możemy dodać Menedżera skryptów do strony wzorcowej, niż musieć go Pamiętaj, aby dodać element ScriptManager do każdej strony zawartość. Krok 1 pokazano, jak dodać funkcja ScriptManager strony wzorcowej podczas kroku 2 przyjrzano się implementowania funkcji interfejsu AJAX na stronie zawartości.

Jeśli zachodzi konieczność dodania skryptów niestandardowych, odwołania do usług sieci Web z obsługą skryptów lub niestandardowe uwierzytelnianie, autoryzacja lub usług profilu do określonej strony zawartości, Dodaj formant serwera proxy menedżera skryptów do strony zawartości, a następnie skonfiguruj Brak dostosowań. Krok 3 zbadać, jak zarejestrować niestandardowy plik JavaScript w określonej strony zawartości za pomocą serwera proxy menedżera skryptów.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Struktury programu ASP.NET AJAX](../../../../ajax/index.md)
- [Samouczki AJAX programu ASP.NET](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Klipy wideo programu ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Tworzenie interakcyjny interfejs użytkownika za pomocą platformy Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Za pomocą NEWID losowo sortowanie rekordów](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Używanie formantu czasomierza](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](interacting-with-the-content-page-from-the-master-page-vb.md)
> [dalej](specifying-the-master-page-programmatically-vb.md)
