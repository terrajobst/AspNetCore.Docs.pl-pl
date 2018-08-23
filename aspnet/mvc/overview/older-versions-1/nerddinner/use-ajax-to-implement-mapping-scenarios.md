---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Korzystanie z technologii AJAX w celu implementacji scenariuszy mapowania | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 11 pokazano, jak zintegrować wsparcie mapowania interfejsu AJAX do naszej aplikacji NerdDinner, pozwalając na użytkowników, którzy są tworzenia, przeglądania lub edytowania kolacji, aby zobaczyć l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f7de23ca46e6dc00fe8075e28068a8b3f95d02cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755772"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Korzystanie z technologii AJAX w celu implementacji scenariuszy mapowania
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 11 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 11 pokazano, jak zintegrować wsparcie mapowania interfejsu AJAX do naszej aplikacji NerdDinner, pozwalając na użytkowników, którzy są tworzenia, przeglądania lub edytowania kolacji, aby wyświetlić lokalizację dinner graficznie.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner krok 11: Integrowanie mapą AJAX

Teraz wprowadzimy naszej aplikacji nieco bardziej ekscytujące integrując Obsługa mapowania interfejsu AJAX. Umożliwi to użytkownicy, którzy są tworzenia, przeglądania lub edytowania kolacji, aby wyświetlić lokalizację dinner graficznie.

### <a name="creating-a-map-partial-view"></a>Tworzenie widoku częściowego mapy

Firma Microsoft zamierza korzystać z funkcji mapowania w kilku miejscach w naszej aplikacji. Aby zachować naszego kodu PRÓBNEGO umieściliśmy typowych funkcji mapy w ramach pojedynczego częściowy szablon, który możemy ponownie użyć w wielu akcji kontrolera i widoki. Utworzymy nazwę tego widoku częściowego "map.ascx" i utwórz go w katalogu \Views\Dinners.

Możemy utworzyć map.ascx częściowe, klikając prawym przyciskiem myszy w katalogu \Views\Dinners i wybierając Add -&gt;wyświetlać polecenia menu. Firma Microsoft będzie nazwy widoku "Map.ascx", sprawdź go jako widok częściowy i wskazują, że zamierzamy udostępnić silnie typizowane "obiad" klasy modelu:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Po kliknięciu przycisku "Dodaj" nasze częściowy szablon zostanie utworzony. Następnie dodamy pliku Map.ascx, aby zawierała następującej zawartości:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Pierwszy &lt;skryptu&gt; biblioteki mapowania wirtualnego 6.2 ziemi Microsoft punktów odniesienia. Drugi &lt;skryptu&gt; map.js pliku, który wkrótce utworzymy której będzie hermetyzację naszych wspólnej logiki mapowania Javascript punktów odniesienia. &lt;Identyfikatora DZIEL = "theMap"&gt; element jest kontenerem HTML, który Virtual Earth będzie używany do hostowania mapy.

Następnie mamy osadzone &lt;skryptu&gt; blok, który zawiera dwie funkcje JavaScript specyficzne dla tego widoku. Pierwsza funkcja używa jQuery do funkcji, który jest wykonywany, gdy strona jest gotowy do uruchomienia skryptu po stronie klienta w górę o komunikacji sieciowej. Wywołuje funkcję pomocnika LoadMap() zdefiniujemy w ramach naszego pliku skryptu Map.js można załadować kontrolki mapy programu virtual earth. Druga funkcja jest program obsługi zdarzeń wywołania zwrotnego, który dodaje pinezkę dla map, który identyfikuje lokalizację.

Zwróć uwagę, jak używamy po stronie serwera &lt;% = %&gt; bloku, w bloku skryptu po stronie klienta, aby osadzić współrzędne geograficzne obiad chcemy do mapowania na kod JavaScript. To jest przydatną techniką, służący do wypełniania wyjściowego wartości dynamicznych, które mogą być używane przez skrypt po stronie klienta (bez konieczności oddzielne wywołanie AJAX do serwera można pobrać wartości — przyspieszająca). &lt;% = %&gt; Bloki będą wykonywane podczas renderowania widoku na serwerze — i dlatego dane wyjściowe HTML po prostu zakończą się wartościami osadzonego kodu JavaScript (na przykład: szerokość var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Tworzenie biblioteki narzędzie Map.js

Teraz utworzymy plik Map.js, w którym firma Microsoft może używać do hermetyzacji funkcje języka JavaScript dla naszych mapy (i implementować metody LoadMap i LoadPin powyżej). Możemy to zrobić, klikając prawym przyciskiem myszy katalog \Scripts w projekcie, a następnie wybierz "Add -&gt;nowy element" polecenie menu, wybierz element języka JScript i nadaj jej nazwę na "Map.js".

Poniżej znajduje się kod JavaScript, dodamy do pliku Map.js, który wchodzi w interakcje z programu Virtual Earth do wyświetlania nasza Mapa i dodać lokalizacje kodów PIN do niego dla naszych kolacji:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integracja mapy z tworzenie i Edycja formularzy

Firma Microsoft będzie teraz zintegrować wsparcie mapy ze scenariuszami tworzyć i edytować istniejące. Dobra wiadomość jest taka, jest to dość łatwe do wykonania i nie wymagać zmienić dowolne z naszego kodu kontrolera przez firmę Microsoft. Ponieważ naszych tworzyć i edytować widoki mają wspólne "DinnerForm" widoku częściowego do zaimplementowania formularza obiad interfejsu użytkownika, firma Microsoft można dodać mapy w jednym miejscu i zostały oba nasz tworzyć i edytować scenariusze go używać.

Wszystkie potrzebne do wykonania jest powoduje ono otwarcie widoku częściowego \Views\Dinners\DinnerForm.ascx i zaktualizować je, aby uwzględnić naszej nowej mapy częściowe. Poniżej przedstawiono wygląd DinnerForm zaktualizowane po dodaniu mapy (Uwaga: elementy formularza HTML zostały pominięte w wstawkę kodu poniżej w celu skrócenia programu):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Częściowa powyższe DinnerForm przyjmuje obiektu typu "DinnerFormViewModel" jako typu modelu (ponieważ wymaga zarówno obiekt obiad, a także SelectList do wypełnienia kontrolki dropdownlist krajów). Nasza Mapa częściowej musi jedynie obiektu typu "Obiad" jak jego typ modelu, a więc gdy firma Microsoft renderowania mapy częściowe możemy kończy się sukcesem po prostu obiad podrzędnych właściwość DinnerFormViewModel do niego:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Funkcja języka JavaScript do dodaliśmy jQuery częściowe używa do dołączania zdarzeń "rozmycie" do pola tekstowego "Address" HTML. Prawdopodobnie słyszałeś już zdarzenia "zespół", które są aktywowane, gdy użytkownik kliknie lub kart do pola tekstowego. Odwrotny jest zdarzenie "rozmycie", który jest uruchamiany po użytkownik opuszcza pole tekstowe. Powyższej procedury obsługi zdarzeń czyści wartości długości i szerokości geograficznej w polu tekstowym, gdy to się dzieje, a następnie drukuje nowej lokalizacji adresu na naszej mapie. Program obsługi zdarzeń wywołania zwrotnego zdefiniowanego w pliku map.js następnie zaktualizuje pola tekstowe długości i szerokości geograficznej na naszego formularza przy użyciu wartości zwróconych z programu virtual earth na podstawie adresu, w których firma Microsoft udostępniła je.

Teraz i kiedy możemy uruchomić ponownie aplikację na karcie "Hosta obiad" zobaczymy domyślnego mapowania wyświetlane wraz z naszego standardowego elementów formularza obiad:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Gdy firma Microsoft wpisz adres, a następnie kartę natychmiast, mapy spowoduje dynamiczne zaktualizowanie, aby wyświetlić lokalizację, a nasz program obsługi zdarzeń spowoduje wypełnienie pól tekstowych szerokości/długości geograficznej przy użyciu wartości lokalizacji:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Jeśli firma Microsoft Zapisz nowe obiad, a następnie otwórz go ponownie do edycji, możemy znaleźć, czy lokalizacji na mapie są wyświetlane po załadowaniu strony:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Za każdym razem, gdy pole adres jest zmieniane, mapy i współrzędne szerokości/długości geograficznej zostanie zaktualizowana.

Teraz, że mapa zawiera lokalizację obiad, możemy zmienić pola formularza i szerokości geograficznej miałyby widocznych pól tekstowych zamiast być ukryte elementy (ponieważ mapy jest automatycznie aktualizuje je każdym razem, gdy wprowadzono adres). Do wykonania to firma Microsoft będzie przełączyć się z przy użyciu Pomocnika Html.TextBox() HTML przy użyciu metody pomocnika Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

A teraz naszych formularzy są nieco bardziej przyjazny dla użytkownika i zapobiec wyświetlaniu pierwotne szerokości/długości geograficznej (jednocześnie nadal ich przechowywania z każdego obiad w bazie danych):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integracja mapy z widoku szczegółów

Teraz, gdy mamy już mapy zintegrowane ze scenariuszami tworzyć i edytować, możemy ją także zintegrować z naszym scenariuszu szczegółowe informacje. Wszystkie potrzebne do wykonania jest wywołanie &lt;% Html.RenderPartial("map"); %&gt; w widoku szczegółów.

Poniżej znajduje się kod źródłowy w celu pełnego widoku szczegółów (za pomocą integracji mapy) wygląda następująco:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

A teraz, gdy użytkownik przejdzie do /Dinners/szczegóły / [id] adres URL zobaczą szczegółowe informacje o lokalizacji obiad na mapie w firmy dinner (wraz z pinezki że po umieszczeniu wskaźnika myszy nad Wyświetla tytuł dinner oraz na adres), i ma połączenie AJAX RSVP fo r go:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementowanie wyszukiwania lokalizacji w naszej bazie danych i repozytorium

Aby zakończyć, wyłącz naszej implementacji interfejsu AJAX, Dodajmy mapę do strony głównej aplikacji, która umożliwia użytkownikom graficznie Wyszukaj kolacji niemal je.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Rozpocznie się poprzez implementację obsługi w ramach naszych warstwy bazy danych i danych repozytorium efektywnego wykonywania wyszukiwania na podstawie lokalizacji usługi radius kolacji. Firma Microsoft może korzystać z nowych [funkcje geoprzestrzenne, programu SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementacji, lub też używamy podejście funkcji SQL, które dzień Gary należy omówionych w artykule w tym miejscu: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) i Rob Conery zakładkach o korzystaniu z LINQ to SQL, w tym miejscu: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Do zaimplementowania tej techniki, firma Microsoft będzie Otwórz "Eksploratora serwera" w programie Visual Studio, wybierz bazę danych NerdDinner i kliknij prawym przyciskiem myszy węzeł podrzędny "funkcji" w nim i wybrać opcję utworzenia nowej "skalarne funkcji":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Firma Microsoft będzie następnie wklej następującą funkcję DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Następnie utworzymy nową funkcję z wartościami przechowywanymi w tabeli w programie SQL Server, który będzie nazywamy "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Ta funkcja tabeli "NearestDinners" używa funkcji pomocnika DistanceBetween do zwrócenia wszystkich kolacji w obrębie 100 mil Latitude i długości geograficznej będziemy podawać go:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Aby wywołać tę funkcję, najpierw otworzymy się LINQ to SQL projektanta przez dwukrotne kliknięcie pliku NerdDinner.dbml naszego katalogu \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Firma Microsoft będzie następnie przeciągnąć funkcji NearestDinners i DistanceBetween na LINQ do projektanta SQL, co spowoduje ich ma zostać dodany jako metody na naszych LINQ do klas SQL NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Następnie możemy uwidocznić metoda zapytania "FindByLocation" na naszych klasy DinnerRepository, która używa funkcji NearestDinner do zwracania nadchodzących kolacji znajdujących się w obrębie 100 mil od określonej lokalizacji:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementacja metody akcji wyszukiwania opartych na formacie JSON AJAX

Firma Microsoft będzie teraz wdrożyć metody akcji kontrolera, która korzysta z zalet nowej metody FindByLocation() w repozytorium i zwróci listę danych obiad, który może służyć do wypełniania mapy. Firma Microsoft będziesz mieć tą metodą akcji, które zwróci dane obiad w formacie JSON (JavaScript Object Notation), dzięki czemu można łatwo manipulować, przy użyciu języka JavaScript na komputerze klienckim.

Aby wdrożyć to rozwiązanie, utworzymy nową klasę "SearchController", kliknij prawym przyciskiem myszy w katalogu \Controllers i wybierając Add -&gt;polecenia menu kontrolera. Firma Microsoft będzie następnie zaimplementować metodę akcji "SearchByLocation" w ramach nowej klasy SearchController podobnie jak poniżej:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metody akcji SearchByLocation SearchController wewnętrznie wywołuje metodę FindByLocation DinnerRespository można pobrać listy kolacji w pobliżu. Zamiast zwracać obiekty obiad bezpośrednio do klienta, jednak zamiast tego zwraca JsonDinner obiektów. Klasa JsonDinner udostępnia podzbiór właściwości obiad (na przykład: ze względów bezpieczeństwa nie ujawnić, nazwy użytkowników, którzy mają RSVP'd na obiad). Zawiera również właściwość RSVPCount, która nie istnieje na obiad — i które dynamicznie jest obliczana przez liczenie obiektów RSVP skojarzonych z określonym obiad.

Następnie używamy metody pomocnika Json() w klasie bazowej kontrolera w celu zwrócenia sekwencji kolacji przy użyciu formatu przewodowy opartych na formacie JSON. JSON to format standardowy tekst reprezentujący proste struktur danych. Poniżej przedstawiono przykład jak listę dwóch obiektów JsonDinner sformatowanego JSON wygląda gdy zwracany z naszych metody akcji:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Wywołanie metody AJAX opartych na formacie JSON przy użyciu jQuery

Teraz możemy przystąpić do aktualizacji strony głównej aplikacji NerdDinner przy użyciu metody akcji SearchByLocation SearchController. Do wykonania, firma Microsoft będzie otworzyć szablon widoku /Views/Home/Index.aspx i zaktualizować je, aby mieć pole tekstowe, przycisk Wyszukaj, nasza Mapa i &lt;div&gt; elementu o nazwie dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Firma Microsoft można dodać dwie funkcje języka JavaScript do strony:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Pierwsza funkcja JavaScript ładuje mapę, po pierwszym załadowaniu strony. Drugi przewodów funkcji JavaScript się JavaScript kliknoj obsługę zdarzeń przycisk wyszukiwania. Po naciśnięciu przycisku wywołuje funkcję FindDinnersGivenLocation() JavaScript, które dodamy do naszego pliku Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Ta funkcja FindDinnersGivenLocation() wywołuje mapy. Find() na wirtualna kontrolka ziemi, aby wyśrodkować jego wprowadzić lokalizacji. Gdy usługi programu virtual earth mapy zwraca, mapy. Metoda Find() wywołuje metodę wywołania zwrotnego callbackUpdateMapDinners, które możemy przekazać jako ostatni argument.

Metoda callbackUpdateMapDinners() polega na tym, gdzie odbywa się faktyczną pracę. Używa metody pomocnika $.post() jQuery firmy do wykonania wywołania AJAX do metody akcji SearchByLocation() naszych SearchController — przekazanie jej szerokość i długość geograficzna nowo wyśrodkowany mapy. Definiuje wbudowanej funkcji, która zostanie wywołana po ukończeniu metody pomocnika $.post() i zwracane wyniki w formacie JSON obiad z SearchByLocation() metody akcji zostaną przekazane za pomocą zmiennej o nazwie "kolacji". Następnie obsługuje instrukcji foreach za pośrednictwem każdego zwróconego obiad i używa obiad szerokości i długości geograficznej, a także inne właściwości, aby dodać nowy numer pin na mapie. Dodaje także wpis obiad do listy HTML kolacji po prawej stronie mapy. Go następnie przewodów telefoniczny zdarzeń po wskazaniu wskaźnikiem, zarówno dla pinezki i lista HTML tak, aby szczegółowe informacje o dinner są wyświetlane, gdy użytkownik przesuwa się nad nimi:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

I teraz podczas możemy uruchomić aplikację i odwiedź stronę główną których firma Microsoft zostaną wyświetlone z mapą. Gdy firma Microsoft wprowadza nazwę miasta mapy spowoduje wyświetlenie nadchodzących kolacji obok niego:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Kursor obiad spowoduje wyświetlenie szczegółowych informacji o nim.

Kliknięcie tytułu obiad w bąbelka lub po prawej stronie na liście HTML spowoduje przejście nam do firmy dinner — firma Microsoft może następnie opcjonalnie Odpowiedz na zaproszenie dla:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Następny krok

Teraz Zaimplementowaliśmy wszystkie funkcje aplikacji naszej aplikacji NerdDinner. Przejdźmy teraz przyjrzeć się jak firma Microsoft może umożliwić automatyczne jednostki testowania go.

> [!div class="step-by-step"]
> [Poprzednie](use-ajax-to-deliver-dynamic-updates.md)
> [dalej](enable-automated-unit-testing.md)
