---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Użyj AJAX do implementacji mapowania scenariusze | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 11 pokazano, jak zintegrować AJAX mapowania obsługi aplikacji NerdDinner, umożliwiające użytkownikom tworzenie, edytowanie lub przeglądanie kolacji, aby zobaczyć l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Umożliwia Implementowanie scenariuszy mapowania AJAX
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 11 bezpłatnych ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 11 pokazano, jak zintegrować AJAX mapowania obsługi aplikacji NerdDinner, umożliwiające użytkownikom tworzenie, edytowanie lub przeglądanie kolacji, aby zobaczyć lokalizację obiad graficznie.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner krok 11. Mapa AJAX integrowanie

Teraz wybierzemy naszej aplikacji nieco wizualnie ekscytujące dzięki integracji AJAX mapowania obsługi. Umożliwi to użytkownikom tworzenie, edytowanie lub przeglądanie kolacji, aby zobaczyć lokalizację obiad graficznie.

### <a name="creating-a-map-partial-view"></a>Tworzenie widoku częściowego mapy

Firma Microsoft ma być używane mapowanie funkcji w kilku miejscach w naszej aplikacji. Aby zachować naszego kodu suchej umieściliśmy typowe funkcje mapy w ramach jednego szablonu częściowe, można ponownie używana w wielu akcji kontrolera i widoków. Firma Microsoft będzie nazwę tego widoku częściowego "map.ascx" i utwórz go w katalogu \Views\Dinners.

Można utworzyć map.ascx częściowe katalogu \Views\Dinners prawym przyciskiem myszy i wybierając Add -&gt;wyświetlić polecenia menu. Firma Microsoft będzie nazwy widoku "Map.ascx", sprawdź go jako widok częściowy i wskazują, że zamierzamy przekazywany klasę modelu jednoznacznie "obiad":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Gdy firma Microsoft, kliknij przycisk "Dodaj" naszych częściowe szablonu zostanie utworzony. Następnie będziemy informować pliku Map.ascx mieć następującą zawartość:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Pierwszy &lt;skryptu&gt; biblioteki mapowania wirtualnego 6.2 Earth Microsoft punktów odniesienia. Drugi &lt;skryptu&gt; map.js pliku, który wkrótce zostanie utworzony, który będzie Hermetyzowanie wspólnej logiki Javascript mapowania punktów odniesienia. &lt;Div id = "theMap"&gt; element jest kontenerem HTML, używanego do hostowania mapy Virtual Earth.

Następnie mamy osadzonych &lt;skryptu&gt; bloku, który zawiera dwie funkcje JavaScript specyficzne dla tego widoku. Pierwszej funkcji używa jQuery podczas transmisji zapasowe funkcję, która jest wykonywana, gdy strona jest gotowy do uruchomienia skryptu po stronie klienta. Wywołuje funkcję pomocnika LoadMap() zdefiniujemy w naszym Map.js pliku skryptu można załadować formantu mapy programu virtual earth. Funkcja drugi jest obsługi zdarzeń wywołania zwrotnego, który dodaje numeru pin do mapy, który określa lokalizację.

Zwróć uwagę, jak jest używany po stronie serwera &lt;% = %&gt; bloku w bloku skryptu po stronie klienta, aby osadzić współrzędne geograficzne obiad chcemy mapować do skryptu JavaScript. Jest to przydatne metoda zwracać wartości dynamiczne, które mogą być używane przez skrypt po stronie klienta (bez konieczności oddzielnego wywołanie AJAX do serwera można pobrać wartości — co pozwala na szybsze). &lt;% = %&gt; Bloki będą wykonywane podczas renderowania widoku na serwerze — i dlatego danych wyjściowych HTML tylko zakończą się wartościami osadzonego kodu JavaScript (na przykład: szerokość var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Tworzenie biblioteki narzędzie Map.js

Teraz Utwórzmy pliku Map.js, którego możemy Hermetyzowanie funkcji JavaScript dla naszych mapy (i implementować metody LoadMap i LoadPin powyżej). Firma Microsoft to zrobić, klikając prawym przyciskiem myszy w katalogu \Scripts w ramach naszych projektu, a następnie wybierz pozycję "Add -&gt;nowy element" polecenia menu, wybierz element, JScript i nadaj jej nazwę na "Map.js".

Poniżej znajduje się kod JavaScript zostanie dodany do pliku Map.js, który będzie współpracować z programu Virtual Earth do wyświetlania naszych mapy i Dodaj numery PIN lokalizacji do niej dla naszych kolacji:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrowanie mapy z tworzenia i edycji formularzy

Firma Microsoft będzie teraz zintegrować Obsługa mapy z naszych istniejące scenariusze tworzenie i edytowanie. Dobre wieści jest, że jest dość łatwe do wykonania i nie wymaga nam zmienić dowolne z naszego kodu kontrolera. Ponieważ naszych tworzenie i edytowanie widoków udział wspólnej "DinnerForm" widoku częściowego do zaimplementowania formularza obiad interfejsu użytkownika, firma Microsoft można dodać mapy w jednym miejscu i mieć zarówno tworzenie i edytowanie scenariuszami go używać.

Wszystkie potrzebne do wykonania jest Otwórz widok częściowy \Views\Dinners\DinnerForm.ascx i zaktualizować ją do uwzględnienia naszej nowej mapy częściowej. Poniżej przedstawiono, jak będzie wyglądać DinnerForm zaktualizowane po dodaniu mapy (Uwaga: pominięto elementów formularza HTML fragment kodu poniżej w celu jego skrócenia):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Częściowe powyżej DinnerForm przyjmuje obiekt typu "DinnerFormViewModel" jako jego typu modelu (ponieważ wymaga zarówno obiektu obiad, jak i SelectList do wypełnienia dropdownlist krajów). Nasze mapy częściowe wystarczy do obiektu typu "Obiad", jako jego typ modelu, a następnie tak podczas możemy renderowania mapy częściowe możemy przekazywane tylko obiad podrzędne właściwość DinnerFormViewModel do niego:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Funkcja JavaScript po dodaniu do jQuery częściowe używa do dołączania zdarzeń "rozmycia" do pola tekstowego "Address" HTML. Został prawdopodobnie wiesz "fokus" zdarzenia, które uruchamiają się po kliknięciu przez użytkownika lub karty do pola tekstowego. Odwrotny jest zdarzenie "rozmycia", które są generowane, gdy użytkownik opuszcza pole tekstowe. Powyżej programu obsługi zdarzeń czyści wartości tekstowe współrzędne geograficzne, gdy to się stanie, a następnie geograficzne do nowej lokalizacji adresu na naszych mapy. Program obsługi zdarzeń wywołania zwrotnego zdefiniowanego w pliku map.js następnie zaktualizuje długości i szerokości geograficznej pól tekstowych w naszym formularza za pomocą wartości zwróconych z programu virtual earth na podstawie adresu, który możemy nadać mu.

I teraz po możemy uruchomić aplikację ponownie i na karcie "Host obiad" zajmiemy się tym domyślnego mapowania wyświetlona wraz z naszego standardowego elementów formularza obiad:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Podczas możemy wpisz adres, a następnie kartę optymalizacji, mapy dynamicznie zaktualizuje Wyświetla lokalizację i naszych obsługi zdarzeń spowoduje wypełnienie pól tekstowych szerokości geograficznej/geograficzne wartościami lokalizacji:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Czy możemy zapisać nowe obiad, a następnie otwórz go ponownie do edycji, możemy znaleźć, że wyświetlana jest lokalizacja mapy, podczas ładowania strony:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Każdej zmianie pole adresu spowoduje zaktualizowanie mapy i współrzędne geograficzne/szerokości geograficznej.

Teraz, mapy Wyświetla lokalizację obiad, firma Microsoft również zmienić pola formularza współrzędne geograficzne przed widocznych pól tekstowych zamiast być ukryte elementy (ponieważ mapy automatycznie aktualizuje je zawsze, gdy wprowadzono adres). Zadania do wykonania to firma Microsoft będzie zmienić przy użyciu Pomocnika Html.TextBox() HTML przy użyciu metody pomocnika Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

I teraz naszych formularze są nieco bardziej przyjazny dla użytkownika i zapobiec wyświetlaniu raw szerokość/wysokość (podczas nadal przechowywanie ich z każdym obiad w bazie danych):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrowanie mapy z widoku szczegółów

Teraz, gdy mamy mapy zintegrowany ze scenariuszami tworzenie i edytowanie umożliwia też zintegrować ją z naszym scenariuszu szczegóły. Wszystkie potrzebne do wykonania jest wywołanie &lt;% % Html.RenderPartial("map");&gt; w widoku szczegółów.

Poniżej znajduje się kod źródłowy do pełnego widoku szczegółów (dzięki integracji mapy) wygląda następująco:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

A teraz, gdy użytkownik przechodzi do adresu URL/szczegóły / [id] /Dinners one wyświetlone szczegóły obiad, lokalizacja obiad na mapie (wraz z numeru pin wypychania że w przypadku aktywowany przez Wyświetla tytuł obiad i na adres), i mieć połączenie AJAX RSVP fo r go:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementowanie wyszukiwania lokalizacji w naszej bazie danych i repozytorium

Aby zakończyć poza naszych implementacji interfejsu AJAX, Dodajmy mapy do strony głównej aplikacji, która umożliwia użytkownikom graficznie Wyszukaj kolacji obok nich.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Rozpocznie się zaimplementowanie pomocy technicznej w ramach naszych warstwy i bazy danych repozytorium Aby efektywnie kolacji wyszukiwanie na podstawie lokalizacji usługi radius. Firma Microsoft może korzystać z nowych [dane geograficzne funkcji programu SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementacji, lub też używamy podejście funkcji SQL, które dzień Gary należy omówiony w artykule w tym miejscu: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) i Conery Tomasz blogged dotyczące korzystania z LINQ do SQL tutaj: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Aby zaimplementować to technika, firma Microsoft będzie otworzyć "Eksploratora serwera" w programie Visual Studio, wybierz NerdDinner bazy danych i kliknij prawym przyciskiem myszy węzeł podrzędny "funkcji" w nim i chce utworzyć nowe "funkcji skalarnej":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Firma Microsoft będzie następnie wklej w następującej funkcji DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Następnie utworzymy nowe funkcji zwracającej tabelę w programie SQL Server, który będzie nazywamy "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Tej funkcji tabeli "NearestDinners" Funkcja DistanceBetween pomocnika do zwrócenia wszystkich kolacji w obrębie 100 mil szerokości geograficznej i geograficzne dostarczamy go:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Aby wywołać tę funkcję, najpierw otworzymy się LINQ do SQL projektanta przez dwukrotne kliknięcie pliku NerdDinner.dbml w naszym katalogu \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Firma Microsoft będzie następnie przeciągnij funkcji NearestDinners i DistanceBetween LINQ do projektanta SQL, co spowoduje ich ma zostać dodany jako metody w naszym LINQ do SQL NerdDinnerDataContext klasy:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Firma Microsoft może następnie ujawnia metody zapytania "FindByLocation" na naszych DinnerRepository klasy, która funkcja NearestDinner i zwraca nadchodzących kolacji znajdujących się w obrębie 100 mil od określonej lokalizacji:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementacja metody akcji wyszukiwania AJAX opartych na formacie JSON

Firma Microsoft będzie teraz implementuje metody akcji kontrolera, który korzysta z nowej metody repozytorium FindByLocation() ponownie zwraca listę danych obiad, który może służyć do wypełnienia mapy. Firma Microsoft zapewnia tą metodą akcji zwróceniu wstecz obiad w formacie JSON (JavaScript Object Notation), dzięki czemu można łatwo manipulować, przy użyciu języka JavaScript na kliencie.

Aby zaimplementować to, utworzymy nową klasę "SearchController" katalogu \Controllers prawym przyciskiem myszy i wybierając Add -&gt;polecenia menu kontrolera. Firma Microsoft będzie następnie zaimplementować metodę akcji "SearchByLocation" w ramach nowej klasy SearchController podobnie jak poniżej:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metody akcji SearchByLocation SearchController wewnętrznie wywołuje metodę FindByLocation DinnerRespository w celu uzyskania listy kolacji pobliżu. Zamiast zwracać obiekty obiad bezpośrednio do klienta, jednak zamiast zwraca JsonDinner obiektów. Klasa JsonDinner przedstawia podzbiór właściwości obiad (na przykład: ze względów bezpieczeństwa nie ujawniają nazwy użytkowników, którzy mają RSVP'd na obiad). Zawiera także właściwości RSVPCount, który nie istnieje na obiad — i który dynamicznie jest obliczana na podstawie liczby RSVP obiekty skojarzone z określonym obiad.

Następnie użyto metody pomocnika Json() w klasie podstawowej kontrolera do zwrócenia sekwencja kolacji przy użyciu formatu przewodowy opartych na formacie JSON. JSON to format tekstu standardowego reprezentujący proste struktury danych. Poniżej przedstawiono przykładowy listę dwóch obiektów JsonDinner formacie JSON wygląd po zwracane z naszych metody akcji:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Wywołanie metody AJAX opartych na formacie JSON przy użyciu jQuery

Firma Microsoft są teraz gotowe do strony głównej aplikacji NerdDinner przy użyciu metody akcji SearchByLocation SearchController aktualizacji. Zadania do wykonania, firma Microsoft będzie otworzyć szablon widoku /Views/Home/Index.aspx i zaktualizować go ma pole tekstowe, przycisk Wyszukaj, naszych mapy i &lt;div&gt; elementu o nazwie dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Następnie można dodać dwóch funkcji JavaScript do strony:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Pierwszej funkcji JavaScript ładuje mapę po pierwszym załadowaniu strony. Drugi przewodów funkcji JavaScript zapasowej JavaScript kliknij program obsługi zdarzeń przycisk wyszukiwania. Po naciśnięciu przycisku wywołuje funkcję FindDinnersGivenLocation() JavaScript, który zostanie dodany do pliku naszych Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Ta funkcja FindDinnersGivenLocation() wywołuje mapy. Find() w formancie ziemi wirtualnego do środka go w lokalizacji wprowadzona. Gdy usługa programu virtual earth mapy zwraca, mapy. Metoda Find() wywołuje metodę wywołania zwrotnego callbackUpdateMapDinners możemy przekazany jako ostatni argument.

Metoda callbackUpdateMapDinners() jest, gdy rzeczywista praca jest wykonywana. Używa metody pomocnika $.post() w jQuery przeprowadzić wywołanie AJAX do metody akcji SearchByLocation() naszych SearchController — przekazanie jej współrzędne geograficzne nowo wyśrodkowany mapy. Definiuje funkcję śródwierszową, która będzie wywoływana po zakończeniu metody pomocnika $.post() i zwracane wyniki w formacie JSON obiad z SearchByLocation() metody akcji zostaną przekazane za pomocą zmiennej o nazwie "kolacji". Następnie obsługuje foreach za pośrednictwem każdego zwróconego obiad i używa obiad szerokości i długości geograficznej, a inne właściwości, aby dodać nowy numer pin na mapie. Dodaje również wpis obiad do listy HTML kolacji z prawej strony mapy. Go następnie przewodów w górę zdarzenia aktywowanego pinezki — i listy HTML, aby uzyskać szczegółowe informacje o obiad są wyświetlane, gdy użytkownik przesuwa się nad nimi:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

I teraz po możemy uruchomić aplikację i strona główna który mamy zostaną wyświetlone mapy. Gdy firma Microsoft wprowadź nazwę miejscowości mapy spowoduje wyświetlenie nadchodzących kolacji obok niego:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Kursora myszy nad obiad wyświetli szczegółowe informacje o nim.

Klikając tytuł obiad bąbelków lub po prawej stronie listy HTML przejdzie nam do obiad — które możemy następnie można opcjonalnie RSVP dla:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Następny krok

Teraz wdrożyliśmy wszystkie funkcje aplikacji naszej aplikacji NerdDinner. Umożliwia teraz znajduje się w jaki sposób można włączyć obsługę automatycznego jednostki testowania go.

> [!div class="step-by-step"]
> [Poprzednie](use-ajax-to-deliver-dynamic-updates.md)
> [dalej](enable-automated-unit-testing.md)
