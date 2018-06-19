---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: AJAX stron wzorcowych i platformy ASP.NET (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym artykule omówiono opcje za pomocą kodu ASP.NET AJAX i stron wzorcowych. Analizuje za pomocą klasy ScriptManagerProxy; w tym artykule omówiono sposób różnych plików JS są ładowane dependi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c7d8477d6d9d235749d88d0b657d60454298e53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891142"
---
<a name="master-pages-and-aspnet-ajax-vb"></a>AJAX stron wzorcowych i platformy ASP.NET (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> W tym artykule omówiono opcje za pomocą kodu ASP.NET AJAX i stron wzorcowych. Analizuje za pomocą klasy ScriptManagerProxy; w tym artykule omówiono, jak różne pliki JS są ładowane w zależności od tego, czy używany jest element ScriptManager we wzorcu lub strona zawartości.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku lat deweloperzy coraz więcej ma zostały tworzenia aplikacji sieci web obsługujących technologię AJAX. Witryny sieci Web z obsługą technologii AJAX korzysta wielu technologii pokrewnych w sieci web oferuje bardziej odpowiednie środowisko użytkownika. Tworzenie aplikacji programu ASP.NET z włączoną obsługą technologii AJAX jest zdumiewająco łatwe dzięki firmy Microsoft ASP.NET AJAX framework. ASP.NET AJAX jest wbudowana w programie ASP.NET 3.5 i Visual Studio 2008; jest również dostępny jako osobny plik do pobrania dla aplikacji programu ASP.NET 2.0.

Podczas kompilowania z włączoną obsługą technologii AJAX stron sieci web platformy ASP.NET AJAX framework, należy dodać do każdej strony, która używa w ramach dokładnie jednego formantu ScriptManager. Jak jego nazwa wskazuje, ScriptManager zarządza skryptu po stronie klienta używany na stronach sieci web z włączoną obsługą technologii AJAX. Co najmniej ScriptManager emituje kod HTML, który informuje przeglądarkę, aby pobrać pliki JavaScript tej biblioteki klienckiej ASP.NET AJAX w skład. Można go również służyć do Rejestrowanie niestandardowe pliki JavaScript, usług sieci web obsługujących skryptu i funkcjonalność usługi niestandardowych aplikacji.

Jeśli w Twojej lokacji używa stron wzorcowych (jak powinno), z niekoniecznie zbędna, nie można dodać formantu ScriptManager do każdej pojedynczej strony zawartości; zamiast można dodać formantu ScriptManager strony wzorcowej. W tym samouczku przedstawiono sposób dodawania formantu ScriptManager strony wzorcowej. Analizuje również sposób używania kontroli ScriptManagerProxy można zarejestrować usługi skryptu i skrypty niestandardowe w określonej strony zawartości.

> [!NOTE]
> W tym samouczku nie Poznaj projektowania i tworzenia aplikacji sieci web obsługujących technologię AJAX ASP.NET AJAX Framework. Aby uzyskać więcej informacji na temat używania AJAX zapoznaj się ASP.NET AJAX wideo i samouczków, a także zasobów wymienionych w sekcji dalsze informacje na końcu tego samouczka.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Badanie znaczników emitowane przez formantu ScriptManager

Formantu ScriptManager emituje kod znaczników, który powoduje, że przeglądarka w celu pobrania plików JavaScript tej biblioteki klienckiej ASP.NET AJAX w skład. Dodaje również bit tekście JavaScript do strony, która inicjuje tej biblioteki. Następujący kod przedstawia zawartość, która jest dodawana do renderowanej danych wyjściowych strony, która obejmuje formantu ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

`<script src="url"></script>` Tagi poinstruować przeglądarki, aby pobrać i uruchomić plik JavaScript w *adres url*. Element ScriptManager emituje trzy takie znaczniki; odwołuje się do jednego pliku `WebResource.axd`, a pozostałe dwa odwołanie do pliku `ScriptResource.axd`. Te pliki nie istnieją faktycznie jako pliki w witrynie sieci Web. Po odebraniu żądania albo jeden z tych plików na serwerze sieci web, aparatu ASP.NET sprawdza, czy ciąg zapytania i zwraca odpowiednia zawartość JavaScript. Skrypt dostarczonych przez tych trzech plików JavaScript zewnętrznych stanowi Biblioteka klienta programu ASP.NET AJAX framework. Druga `<script>` tagi emitowane przez element ScriptManager obejmują wbudowanego skryptu, który inicjuje tej biblioteki.

Odwołania do skryptu zewnętrznego i wbudowanego skryptu emitowane przez element ScriptManager są istotne dla strony, która używa środowiska ASP.NET AJAX framework, ale nie jest wymagany dla stron, które nie używają platformę. W związku z tym przyczyny, jest idealny można tylko dodać element ScriptManager do tych stron, które używają programu ASP.NET AJAX framework. To jest wystarczająca, a jeśli masz wiele stron, które używają platformę zostanie wyświetlone okno Dodawanie formantu ScriptManager do wszystkich stron - powtarzających się zadań, znaczy najmniej. Alternatywnie można dodać elementu ScriptManager na stronie głównej, następnie injects ten skrypt niezbędne do wszystkich stron zawartości. Z tej metody nie trzeba Pamiętaj, aby dodać element ScriptManager do nowej strony, która wykorzystuje platformę ASP.NET AJAX, ponieważ jest już uwzględniony przez strony wzorcowej. Krok 1 przeszukiwań przez procedurę dodawania elementu ScriptManager strony wzorcowej.

> [!NOTE]
> Jeśli planujesz na tym funkcjonalności interfejsu AJAX w interfejsie użytkownika strony wzorcowej, możesz nie mają wyboru w sprawie — musi zawierać element ScriptManager na stronie głównej.


Wadą jednego interfejsu dodawania ScriptManager strony wzorcowej jest, że powyższy skrypt jest emitowany w *co* strony, niezależnie od tego, czy jego uruchomienie. Prowadzi to wyraźnie nieużywanego przepustowości dla tych stron, które element ScriptManager uwzględnione (za pośrednictwem strony wzorcowej), ale nie korzystają z funkcji programu ASP.NET AJAX framework. Ale tylko ile przepustowości jest niewykorzystana?

- Rzeczywista zawartość emitowane przez element ScriptManager (pokazanym powyżej) sumy nieco ponad 1KB.
- Trzy pliki skryptu zewnętrznego odwołuje się `<script>` elementu, jednak zawierać około 450 KB danych jako nieskompresowane; w witrynie internetowej, która używa kompresji gzip, można zmniejszyć tego całkowitej przepustowości bliskie 100 KB. Jednak te pliki skryptów są buforowane przez przeglądarkę przez jeden rok, co oznacza, że powinni zostać pobrane raz i następnie mogą być ponownie używane w innych stron w witrynie.

W najlepszym przypadku, gdy pliki skryptów są buforowane, całkowity koszt wówczas 1KB, który jest niewielka. W najgorszym przypadku jednak — kiedy pliki skryptów nie zostały jeszcze pobrane i serwera sieci web nie używa żadnych formularza kompresji — trafień przepustowości jest około 450KB, który można dodać dowolnym z drugiej lub dwóch za pośrednictwem połączenia szerokopasmowego maksymalnie minuty dla  Użytkownicy za pośrednictwem modemów. Dobre wieści jest, że ponieważ plików zewnętrznych skryptów są buforowane przez przeglądarkę, ten scenariusz rzadko występuje.

> [!NOTE]
> Jeśli uważasz, nadal niepewne wprowadzenie do formantu ScriptManager na stronie głównej, należy wziąć pod uwagę formularza sieci Web ( `<form runat="server">` znaczników na stronie głównej). Każdej strony ASP.NET, który korzysta z modelu odświeżania strony musi zawierać dokładnie jeden formularz sieci Web. Dodawanie do formularza sieci Web dodaje dodatkowej zawartości: liczba pól ukrytym `<form>` tagów, i w razie potrzeby JavaScript funkcji inicjowania ogłaszania zwrotnego ze skryptu. Ten kod znaczników jest niepotrzebne dla stron, które nie ogłaszanie. Tego znacznika nadmiarowe może zostać usunięte przez usunięcie formularza sieci Web ze strony wzorcowej i ręczne dodanie do każdej strony zawartości, które tego wymagają. Jednak korzyści formularza sieci Web na stronie głównej przeważają wady z właśnie niepotrzebnie dodane do określonych stron zawartości.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Krok 1: Dodawanie formantu ScriptManager do strony wzorcowej

Każdej strony sieci web, która używa środowiska ASP.NET AJAX framework musi zawierać dokładnie jeden formant ScriptManager. Ze względu na to wymaganie zwykle dobrym rozwiązaniem jest umieszczenie jednego formantu ScriptManager na stronie głównej tak, aby wszystkie strony z zawartością ma formantu ScriptManager automatycznie dołączane. Ponadto element ScriptManager musi występować przed każdą kontrolek serwera ASP.NET AJAX, takie jak formanty UpdatePanel i postępu aktualizacji. W związku z tym najlepiej umieścić ScriptManager przed wszystkimi formantami ContentPlaceHolder w formularzu sieci Web.

Otwórz `Site.master` strony wzorcowej i formantu ScriptManager na stronie Dodaj w formularzu sieci Web, ale przed wysłaniem `<div id="topContent">` — element (zobacz rysunek 1). Jeśli używasz programu Visual Web Developer 2008 lub Visual Studio 2008 formantu ScriptManager znajduje się w przyborniku na karcie rozszerzenia AJAX. Jeśli używasz programu Visual Studio 2005, należy najpierw zainstalować platformę ASP.NET AJAX i Dodaj formanty do przybornika. Odwiedź stronę pobierania kodu ASP.NET AJAX, aby uzyskać platformę ASP.NET 2.0.

Po dodaniu ScriptManager do strony, zmienić jego `ID` z `ScriptManager1` do `MyManager`.


[![Dodaj element ScriptManager do strony wzorcowej](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Rysunek 01**: Dodaj element ScriptManager do strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Krok 2: Za pomocą ASP.NET AJAX Framework ze strony zawartości

Za pomocą formantu ScriptManager dodany do strony głównej można teraz dodać ASP.NET AJAX framework funkcji do dowolnej zawartości strony. Umożliwia tworzenie nowej strony ASP.NET, która wyświetla losowo wybranym produktu z bazy danych Northwind. Aby zaktualizować ten ekran co 15 s przedstawiający nowego produktu użyjemy kontroli czasomierza programu ASP.NET AJAX framework.

Rozpocznij od utworzenia nowej strony w katalogu głównym o nazwie `ShowRandomProduct.aspx`. Nie zapomnij o tej nowej strony, aby powiązać `Site.master` strony wzorcowej.


[![Dodaj nową stronę ASP.NET do witryny sieci Web](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Rysunek 02**: Dodaj nową stronę ASP.NET do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image6.png))


Odwołaj który w określanie tytuł, tagi Meta i inne nagłówków HTML w samouczku strony wzorcowej [SKM1] utworzyliśmy klasy niestandardowej strony podstawowej o nazwie `BasePage` który generowane tytuł strony, jeśli nie została jawnie ustawiona. Przejdź do `ShowRandomProduct.aspx` kodem strony klasy i została ona pochodzić od `BasePage` (a nie z `System.Web.UI.Page`).

Na koniec zaktualizuj `Web.sitemap` pliku, aby uwzględnić wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` wzorca do lekcji interakcji z zawartości strony:


[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

Dodanie tego `<siteMapNode>` element jest widoczny w wnioski liście (patrz rysunek 5).

### <a name="displaying-a-randomly-selected-product"></a>Wyświetlanie losowo wybranym produktu

Wróć do `ShowRandomProduct.aspx`. Przy użyciu projektanta, przeciągnij formantu UpdatePanel z przybornika do `MainContent` zawartości formantu i ustawić jej `ID` właściwości `ProductPanel`. Element UpdatePanel reprezentuje region na ekranie, który może być aktualizowana asynchronicznie za pomocą odświeżania strony częściowe.

Naszym pierwszym zadaniem jest do wyświetlania informacji o produkcie losowo wybranym w elemencie UpdatePanel. Uruchom przeciągając kontrolki widoku szczegółów w elemencie UpdatePanel. Ustawianie formantu widoku DetailsView `ID` właściwości `ProductInfo` i wyczyszczenie jego `Height` i `Width` właściwości. Rozwiń DetailsView tagów inteligentnych, a z listy rozwijanej wybierz źródło danych, wybierz powiązać widoku DetailsView formant SqlDataSource o nazwie `RandomProductDataSource`.


[![Nowy formant SqlDataSource powiązać widoku DetailsView](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Rysunek 03**: powiązać widoku DetailsView nowy formant SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image9.png))


Skonfigurować SqlDataSource formantu do nawiązania połączenia z bazą danych Northwind za pośrednictwem `NorthwindConnectionString` (który utworzone interakcja ze stroną wzorcową z samouczka strony zawartości [SKM2]). Podczas konfigurowania instrukcji select wybierz Określ niestandardową instrukcję SQL, a następnie wprowadzić następujące zapytanie:


[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

`TOP 1` — Słowo kluczowe w `SELECT` klauzula zwraca tylko pierwszy rekord zwróconych przez kwerendę. `NEWID()` Funkcji generuje nową wartość Unikatowy identyfikator globalny (GUID) i mogą być używane w `ORDER BY` klauzuli do zwracania tabeli rekordów w kolejności losowej.


[![Skonfigurować SqlDataSource można przywrócić rekord jednej, losowo wybranych](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Rysunek 04**: skonfigurować SqlDataSource do zwrócenia jeden losowo wybrany rekord ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image12.png))


Po zakończeniu działania kreatora programu Visual Studio tworzy elementu BoundField dla dwóch kolumn będących zwróconych przez zapytanie powyżej. W tym momencie znaczników deklaratywne ze strony powinien wyglądać podobnie do następującego:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Rysunek 5. pokazuje `ShowRandomProduct.aspx` strony podczas wyświetlania za pośrednictwem przeglądarki. Kliknij przycisk Odśwież w przeglądarce, aby ponownie załadować stronę; powinny pojawić się `ProductName` i `UnitPrice` wartości dla nowego rekordu losowo wybrany.


[![Nazwa i cen losowe produktu są wyświetlane](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Rysunek 05**: Nazwa produktu losowe A i cen zostaną wyświetlone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Automatyczne wyświetlanie nowego produktu co 15 s

ASP.NET AJAX framework zawiera formant czasomierza, który wykonuje odświeżania strony o określonej godzinie; na ogłaszanie czasomierza `Tick` zdarzenia. Jeśli formant czasomierza jest umieszczony w elemencie UpdatePanel wyzwala strona częściowa ogłaszania zwrotnego, w którym możemy ponownie powiązać dane do widoku DetailsView do wyświetlenia nowego produktu losowo wybranych.

Aby to zrobić, przeciągnij czasomierza z przybornika i upuść ją na element UpdatePanel. Zmień czasomierza `ID` z `Timer1` do `ProductTimer` i jego `Interval` właściwości z 60000 15000. `Interval` Właściwość wskazuje wyrażony w milisekundach czas między odświeżeniami; ustawieniem dla niego 15000 powoduje czasomierza do wyzwolenia odświeżania strony strona częściowa co 15 s. W tym momencie znaczników deklaratywne Twojej czasomierza powinien wyglądać podobnie do następującego:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Utwórz program obsługi zdarzeń dla czasomierza `Tick` zdarzeń. W tej obsłudze zdarzeń musimy ponownie powiązać dane do widoku DetailsView wywołując DetailsView `DataBind` metody. W ten sposób nakazuje widoku DetailsView ponownie pobrać danych z kontroli źródła danych, który będzie wybierz i wyświetlić nowy, losowo wybrany rekordu (podobnie jak podczas ponownego ładowania strony, klikając przycisk Odśwież w przeglądarce).


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

To wszystko jest do niego! Kontroluj strony za pośrednictwem przeglądarki. Początkowo wyświetlane są informacje losowe produktu. Patiently Obejrzyj ekranu można zauważyć, że po 15 sekund, informacje o nowym produktem magicznie zastępuje istniejące wyświetlania.

Aby lepiej zobaczyć, co dzieje się w tym miejscu, możemy dodać formantu etykiety do elementu UpdatePanel, który wyświetla czas wyświetlania ostatniej aktualizacji. Dodawanie formantu etykiety w sieci Web w obrębie UpdatePanel, ustaw jej `ID` do `LastUpdateTime`i usuń zaznaczenie jej `Text` właściwości. Następnie należy utworzyć program obsługi zdarzeń dla elementu UpdatePanel `Load` zdarzeń i wyświetlić bieżący czas w etykiecie. (Elementu UpdatePanel `Load` zdarzenie jest wywoływane na każdej stronie pełnej lub częściowej ogłaszania zwrotnego.)


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Z tą zmianą pełną strony zawiera czas aktualnie wyświetlanego produkt został załadowany. Rysunek 6 przedstawia strony po odwiedzeniu najpierw. Rysunek nr 7 przedstawia strony później 15 sekund po kontroli czasomierza ma "zaznaczyć" i element UpdatePanel zostały odświeżone, aby wyświetlić informacje o nowym produktem.


[![Losowo wybrany produkt jest wyświetlany na załadowanie strony](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Rysunek 06**: A losowo wybrane produktu jest wyświetlany na stronie obciążenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image18.png))


[![Co 15 sekund wyświetlania nowego losowo wybrane produktu](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Rysunek 07**: co 15 sekund, zostanie wyświetlony nowy losowo wybrany produkt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Krok 3: Używanie formantu ScriptManagerProxy

Wraz z tym konieczne skrypt środowiska ASP.NET AJAX framework biblioteki klienta, ScriptManager można również zarejestrować niestandardowe pliki JavaScript, odwołania do usług sieci Web korzystających z skryptu i niestandardowego uwierzytelniania, autoryzacji i profilu usługi. Zwykle takie dostosowania są specyficzne dla konkretnej strony. Jeśli pliki niestandardowego skryptu, odwołania do usług sieci Web, lub uwierzytelniania, autoryzacji lub profilu usługi odwołuje się element ScriptManager na stronie głównej następnie one jednak są włączone na wszystkich stronach witryny sieci Web.

Aby dodać powiązane ScriptManager dostosowania dla strony strona formant ScriptManagerProxy. Można dodać ScriptManagerProxy do strony zawartości, a następnie Zarejestruj niestandardowy plik JavaScript, odwołania do usługi sieci Web, lub uwierzytelniania, autoryzacji lub profilu usługi z ScriptManagerProxy; skutkuje to rejestrowania tych usług w szczególności strony zawartość.

> [!NOTE]
> Strony ASP.NET mogą mieć tylko nie więcej niż jeden formantu ScriptManager obecny. W związku z tym nie można dodać formantu ScriptManager do strony zawartości, jeśli formantu ScriptManager już jest zdefiniowana na stronie głównej. Wyłącznie do celów ScriptManagerProxy ma umożliwiają deweloperom zdefiniować ScriptManager na stronie głównej, ale nadal mieć możliwość dodawania dostosowania ScriptManager na podstawie strony strona.


Aby wyświetlić kontroli ScriptManagerProxy w akcji, umożliwia rozszerzyć elementu UpdatePanel w `ShowRandomProduct.aspx` uwzględnienie przycisku, który używa skryptu po stronie klienta, aby wstrzymać lub wznowić kontrola czasomierza. Formant czasomierza ma trzy metody po stronie klienta, które możemy użyć do osiągnięcia tej wymaganej funkcjonalności:

- `_startTimer()` -Uruchamia program Regulacja czasomierza
- `_raiseTick()` -powoduje, że formant czasomierza "znaczników", a tym samym odświeżania i wywoływanie zdarzeń jego znaczników na serwerze
- `_stopTimer()` -sterowania czasomierza

Utwórz plik JavaScript za pomocą zmiennych o nazwie `timerEnabled` i funkcji o nazwie `ToggleTimer`. `timerEnabled` Zmiennej wskazuje, czy formant czasomierza jest aktualnie włączone lub wyłączone; jego wartość domyślna to true. `ToggleTimer` Funkcja akceptuje dwa parametry wejściowe: odwołanie do przycisk Wstrzymaj i Wznów i po stronie klienta `id` wartość formantu czasomierza. Ta funkcja przełącza wartość `timerEnabled`, pobiera odwołanie do formantu czasomierza, uruchomienia lub zatrzymania czasomierza (w zależności od wartości `timerEnabled`) i aktualizuje tekst wyświetlany przycisk "Wstrzymaj" lub "Resume". Ta funkcja zostanie wywołana przy każdym kliknięciu przycisku Wstrzymanie/wznowienie.

Rozpocznij od utworzenia nowy folder w witrynie sieci Web o nazwie `Scripts`. Następnie dodaj nowy plik do folderu skryptów o nazwie `TimerScript.js` typu plik języka JScript.


[![Dodaj nowy plik JavaScript w folderze skryptów](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Rysunek 08**: Dodaj nowy plik JavaScript do `Scripts` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image24.png))


[![Nowy plik JavaScript został dodany do witryny sieci Web](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Rysunek 09**: nowy plik JavaScript został dodany do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image27.png))


Następnie dodaj następujący skrypt służy do `TimerScript.js` pliku:


[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

Teraz musisz zarejestrować ten niestandardowy plik JavaScript w `ShowRandomProduct.aspx`. Wróć do `ShowRandomProduct.aspx` i na stronie Dodaj formant ScriptManagerProxy; ustaw jego `ID` do `MyManagerProxy`. Aby zarejestrować niestandardowych skryptów JavaScript plik zaznacz formant ScriptManagerProxy w Projektancie i przejdź do okna właściwości. Tytuł to jedna z właściwości skryptów. Wybranie tej właściwości powoduje wyświetlenie edytora kolekcji ScriptReference pokazany na rysunku 10. Kliknij przycisk Dodaj do uwzględnienia nowego odwołania do skryptu, a następnie wprowadź ścieżkę do pliku skryptu w właściwość Path: `~/Scripts/TimerScript.js`.


[![Dodaj odwołanie do formantu ScriptManagerProxy skryptu](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Na rysunku nr 10**: Dodaj odwołania do skryptu do formantu ScriptManagerProxy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image30.png))


Po deklaratywne Dodawanie odwołania do skryptu kontroli ScriptManagerProxy znaczników została zaktualizowana do `<Scripts>` kolekcji za pomocą jednej `ScriptReference` wpisu, jako poniższy fragment kodu znaczników przedstawiono:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

`ScriptReference` Wpis instruuje ScriptManagerProxy załączanie odwołań do pliku JavaScript w jego renderowanego kodu znaczników. Oznacza to, rejestrując niestandardowego skryptu w ScriptManagerProxy `ShowRandomProduct.aspx` innego zawiera teraz wyjścia renderowanej strony `<script src="url"></script>` tag: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Teraz można nazywa się `ToggleTimer` funkcji zdefiniowanej w `TimerScript.js` ze skryptu klienta w `ShowRandomProduct.aspx` strony. Dodaj poniższy kod HTML w elemencie UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Spowoduje to wyświetlenie element button z tekstem "Wstrzymaj". Po każdej zmianie kliknięciu, funkcja JavaScript `ToggleTimer` jest wywoływana, przekazując odwołanie do przycisku i `id` wartość formantu czasomierza (`ProductTimer`). Należy zwrócić uwagę składni uzyskiwania `id` wartość formantu czasomierza. `<%=ProductTimer.ClientID%>` emituje wartość `ProductTimer` kontroli czasomierza `ClientID` właściwości. W nazwy Identyfikatora formantów w samouczku strony z zawartością [SKM3] Rozmawialiśmy różnice między po stronie serwera `ID` wartość i wynikowy klienta `id` wartości i w jaki sposób `ClientID` zwraca po stronie klienta `id`.

Rysunek 11 przedstawia tę stronę po odwiedzeniu najpierw za pośrednictwem przeglądarki. Czasomierz jest uruchomiony i aktualizuje informacje wyświetlane produktu co 15 s. Rysunek 12 przedstawiono ekranu, po kliknięciu przycisk Wstrzymaj. Klikając przycisk Wstrzymaj zatrzymuje czasomierza i aktualizuje tekst przycisku "Resume". Informacji o produkcie Odśwież (i kontynuować odświeżanie co 15 sekund), gdy użytkownik kliknie przycisk Wznów.


[![Kliknij przycisk Przerwij, aby zatrzymać sterowanie czasomierza](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Rysunek 11**: kliknij przycisk Przerwij, aby zatrzymać czasomierza formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image33.png))


[![Kliknij przycisk Wznów, aby ponownie uruchomić czasomierza](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Rysunek 12**: kliknij przycisk Wznów, aby ponownie uruchomić czasomierza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-pages-and-asp-net-ajax-vb/_static/image36.png))


## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci web obsługujących technologię AJAX za pomocą kodu ASP.NET AJAX framework konieczne jest, że każdej strony sieci web z włączoną obsługą technologii AJAX obejmuje formantu ScriptManager. Aby ułatwić ten proces, możemy dodać element ScriptManager do strony głównej, a nie trzeba pamiętać dodać element ScriptManager do każdej strony zawartości. Krok 1 pokazano, jak dodać element ScriptManager strony wzorcowej podczas kroku 2 przeglądał Implementowanie funkcjonalności interfejsu AJAX na stronie zawartości.

Jeśli trzeba dodać niestandardowe skrypty, odwołania do usług sieci Web korzystających z skryptu lub dostosowanego uwierzytelniania, autoryzacji lub usług profilu do określonej strony zawartości, Dodaj formant ScriptManagerProxy do strony zawartości, a następnie skonfiguruj Brak dostosowań. Krok 3 zbadane, jak używać ScriptManagerProxy rejestrowania niestandardowego pliku JavaScript w określonej strony zawartości.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [ASP.NET AJAX Framework](../../../../ajax/index.md)
- [Samouczki AJAX ASP.NET](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Filmy wideo AJAX ASP.NET](../../../videos/aspnet-ajax/index.md)
- [Kompilowanie interakcyjny interfejs użytkownika z platformy Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Przy użyciu NEWID losowo sortowanie rekordów](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Używanie formantu czasomierza](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](interacting-with-the-content-page-from-the-master-page-vb.md)
> [dalej](specifying-the-master-page-programmatically-vb.md)
