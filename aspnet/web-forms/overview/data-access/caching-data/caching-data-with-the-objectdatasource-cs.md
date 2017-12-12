---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Buforowanie danych z elementu ObjectDataSource (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Buforowanie oznacza różnicę między wolne i szybkie aplikacji sieci Web. W tym samouczku jest pierwszy czterech, który szczegółowe Spójrz na buforowanie w programie ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ce0bd1d3302ee68c9c65584686172a07143e4a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-with-the-objectdatasource-c"></a>Buforowanie danych z elementu ObjectDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) lub [pobierania plików PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Buforowanie oznacza różnicę między wolne i szybkie aplikacji sieci Web. W tym samouczku jest pierwszy czterech, który szczegółowe Spójrz na buforowanie w programie ASP.NET. Dowiedz się, najważniejsze pojęcia związane z buforowania i sposób stosowania buforowanie, aby za pomocą kontrolki ObjectDataSource warstwy prezentacji.


## <a name="introduction"></a>Wprowadzenie

W nauce komputera *buforowanie* to proces pobierania danych i informacji, która jest kosztowna uzyskać i przechowywanie kopii w lokalizacji, która jest szybsze do dostępu. W przypadku aplikacji opartej na danych dużych i złożonych kwerend zwykle zużywa większość czas wykonywania aplikacji s. Takie s wydajność aplikacji, następnie często można zwiększyć przez przechowywanie wyników kwerendy kosztowne bazy danych w pamięci s aplikacji.

Platforma ASP.NET 2.0 oferuje szeroką gamę opcji buforowania. Całą stronę sieci web lub znacznik s renderowania formantu użytkownika mogą być buforowane za pośrednictwem *buforowanie danych wyjściowych*. Kontrolki ObjectDataSource i SqlDataSource zapewniają możliwości buforowania oraz, w tym samym, dzięki czemu dane pamięci podręcznej na poziomie formantu. I ASP.NET s *pamięci podręcznej danych* udostępnia bogate buforowania interfejs API, który umożliwia deweloperom strony programowo obiektów w pamięci podręcznej. W tym samouczku i dalej trzech, które zajmiemy się przy użyciu ObjectDataSource s buforowanie funkcje, a także pamięci podręcznej danych. Firma Microsoft będzie także Poznaj sposób dane całej aplikacji przy uruchamianiu z pamięci podręcznej i przechowywać dane w pamięci podręcznej świeże przy użyciu zależności buforu SQL. Te samouczki nie Poznaj buforowanie danych wyjściowych. Aby uzyskać szczegółowy widok buforowanie danych wyjściowych, zobacz [buforowanie danych wyjściowych programu ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Buforowanie można zastosować w dowolnym miejscu w architekturze z Warstwa dostępu do danych się przez warstwę prezentacji. W ramach tego samouczka przyjrzymy stosowania buforowania do warstwy prezentacji za pomocą kontrolki ObjectDataSource. W następnym samouczku, które zostaną omówione buforowanie danych w warstwie logiki biznesowej.

## <a name="key-caching-concepts"></a>Pojęcia dotyczące buforowanie kluczy

Buforowanie może znacznie ulepszyć s aplikacji ogólnej wydajności i skalowalności przez pobranie danych, która jest kosztowna do generowania i przechowywania kopii w lokalizacji dostępnej dla bardziej wydajne. Ponieważ pamięć podręczna zawiera tylko kopii danych rzeczywistych, podstawowego, może być nieaktualna, lub *starych*, w przypadku zmiany danych. Zwalczania to, Projektant strony może wskazywać kryteria, według których będzie elementu pamięci podręcznej *wykluczaniu* z pamięci podręcznej, za pomocą:

- **Kryteria czasu na podstawie** element może być dodany do pamięci podręcznej bezwzględną lub przedłużanie czas trwania. Na przykład deweloper strony może wskazywać, czas trwania, powiedzieć 60 sekund. Z bezwzględny czas trwania elementu pamięci podręcznej zostanie usunięty 60 sekund po została dodana do pamięci podręcznej, niezależnie od tego, jak często uzyskano. Na metodzie przesuwanego okres element pamięci podręcznej zostanie usunięty 60 sekund od ostatniego dostępu.
- **Kryteria na podstawie zależności** zależność może być skojarzony z elementem po dodaniu do pamięci podręcznej. Po zmianie zależności element s zostanie usunięty z pamięci podręcznej. Zależność może być pliku, inny element pamięci podręcznej lub kombinację obu. Platforma ASP.NET 2.0 umożliwia także zależności buforu SQL, które umożliwiają deweloperom dodania elementu do pamięci podręcznej i został usunięty po zmianie danych bazy danych. Omówione zostaną zależności buforu SQL w nadchodzących [przy użyciu zależności buforu SQL](using-sql-cache-dependencies-cs.md) samouczka.

Niezależnie od określone kryteria wykluczenia elementu w pamięci podręcznej może być *oczyszczany* przed kryterium opartego na czasie lub na podstawie zależności zostały spełnione. Jeśli pamięć podręczna osiągnęła, należy usunąć istniejące elementy przed dodaniem nowych. W rezultacie pracując programowo z danych z pamięci podręcznej go s istotne, że należy zawsze przyjęto założenie, że buforowane dane mogą nie występować. Przyjrzymy wzorzec do użycia podczas uzyskiwania dostępu do danych z pamięci podręcznej programowo w naszym samouczku dalej *buforowania danych w architekturze*.

Buforowanie zapewnia ekonomiczny sposób ściskanie większą wydajność aplikacji. Jako [Steven Smith](http://aspadvice.com/blogs/ssmith/) articulates w jego artykule [ASP.NET buforowanie: technik i najlepszych rozwiązań](https://msdn.microsoft.com/en-us/library/aa478965.aspx):

Buforowanie może być dobrym sposobem uzyskania dobrej dostateczną wydajność bez konieczności dużo czasu i analizy. Ilość pamięci jest tanie, więc jeśli można uzyskać wydajności przez buforowanie danych wyjściowych przez 30 sekund zamiast wydatków dnia lub tygodnia próby zoptymalizować z kodu lub bazy danych, wykonaj buforowania rozwiązania (przy założeniu, 30 - starych sekundę danych jest ok) i przenieść. Po pewnym czasie niską projekt będzie prawdopodobnie nadążyć, więc oczywiście należy poprawnie projektowania aplikacji. Jednak jeśli wystarczy pobrać dostateczną wydajność dzisiaj, buforowanie może być znakomity [Metoda], kupowanie czasu Refaktoryzuj aplikacji w późniejszym terminie, jeśli masz czas, aby to zrobić.

Gdy buforowanie zapewniają wydajność znaczne ulepszenia, nie jest stosowane we wszystkich przypadkach, takich jak z aplikacjami, które używają danych w czasie rzeczywistym, często aktualizowania lub nawet krótko żyła starych danych w przypadku nadmiernego. Jednak w przypadku większości aplikacji, buforowanie należy użyć. Więcej tła na buforowanie w programie ASP.NET 2.0, można znaleźć w temacie [buforowanie wydajności](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) sekcji [samouczków szybkiego startu programu ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Krok 1: Tworzenie pamięci podręcznej stron sieci Web

Przed Rozpoczniemy naszych eksploracji funkcji buforowania s ObjectDataSource umożliwiają s najpierw Poświęć chwilę, aby utworzyć w naszym projekt witryny sieci Web, które będą potrzebne dla tego samouczka i dalej trzy stron ASP.NET. Rozpocznij od dodania nowy folder o nazwie `Caching`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Dodawanie stron ASP.NET samouczki dotyczące buforowania](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki dotyczące buforowania


Podobnie jak w innych folderach `Default.aspx` w `Caching` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Rysunek 2: Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Rysunek 2**: na rysunku 2: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Na koniec Dodaj te strony jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po zakończeniu pracy z danymi binarnymi `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy samouczki buforowania.


![Mapy witryny zawiera teraz wpisy samouczki buforowania](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Rysunek 3**: mapy witryny zawiera teraz wpisy samouczki buforowania


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Krok 2: Wyświetlanie listy produktów na stronie sieci Web

W tym samouczku opisuje sposób użycia ObjectDataSource s wbudowanych buforowania funkcje kontroli. Zanim można przyjrzymy te funkcje, jednak najpierw musimy strony do pracy z. Let s utworzyć stronę sieci web, która używa widoku GridView do listy informacji o produkcie pobierane przez element ObjectDataSource z `ProductsBLL` klasy.

Uruchamianie przez otwarcie `ObjectDataSource.aspx` strony `Caching` folderu. Przeciągnij element GridView z przybornika do projektanta, ustaw jej `ID` właściwości `Products`oraz od ich tagów inteligentnych, wybierz powiązać nowe kontrolki ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj ObjectDataSource do pracy z `ProductsBLL` klasy.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Na tej stronie umożliwiają s utworzyć GridView edytowalny, dzięki czemu można sprawdzić, co się dzieje podczas modyfikacji danych w pamięci podręcznej w elemencie ObjectDataSource za pośrednictwem interfejsu s widoku GridView. Pozostaw listy rozwijanej w karcie Wybierz wartości domyślne, `GetProducts()`, ale Zmień na karcie aktualizacji do wybranego elementu `UpdateProduct` przeciążenia, które akceptuje `productName`, `unitPrice`, i `productID` jako jego parametrów wejściowych.


[![Ustaw listy rozwijanej s kartę aktualizacji do przeciążenia UpdateProduct odpowiednie](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Rysunek 5**: wartość s kartę aktualizacji listy rozwijanej zastosowanie `UpdateProduct` przeciążenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Na koniec zestawu list rozwijanych w kartach INSERT i DELETE na (Brak) i kliknij przycisk Zakończ. Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio ustawia ObjectDataSource s `OldValuesParameterFormatString` właściwości `original_{0}`. Zgodnie z opisem w [omówienie Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczek, tej właściwości musi być usunięte z składni deklaratywnej lub ustawioną wartość domyślną `{0}`, aby przepływu pracy przez naszych aktualizacji Kontynuuj bez błędów.

Ponadto po zakończeniu działania kreatora programu Visual Studio dodaje pole do widoku GridView dla każdego pola danych produktu. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, i `UnitPrice` BoundFields. Następnie zaktualizuj `HeaderText` właściwości każdej z tych BoundFields do produktu, kategorii i cen, odpowiednio. Ponieważ `ProductName` pole jest wymagane, przekonwertować na pole TemplateField elementu BoundField i dodać RequiredFieldValidator do `EditItemTemplate`. Podobnie, przekonwertować `UnitPrice` elementu BoundField na pole TemplateField i Dodaj CompareValidator do zapewnienia, że wartości wprowadzonej przez użytkownika jest prawidłowy waluty wartość s, tym większa lub równa zero. Oprócz tych zmian, możesz wykonać wszelkie estetycznych zmiany, takie jak Wyrównywanie prawej `UnitPrice` wartość lub określanie formatowanie `UnitPrice` w interfejsach tylko do odczytu i edycji tekstu.

Należy widoku GridView można edytować, zaznaczając pole wyboru Włącz edytowanie w tagu s widoku GridView. Sprawdź również włączyć stronicowanie i Włącz sortowanie pola wyboru.

> [!NOTE]
> Potrzebujesz Przegląd sposobu dostosowywania widoku GridView interfejsu edycji s? Jeśli tak, odwołaj się do [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczka.


[![Włącz obsługę widoku GridView do edycji, sortowania i stronicowania](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Rysunek 6**: Włączanie widoku GridView obsługi edycji, sortowania i stronicowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Po wprowadzeniu tych zmian w widoku GridView, znaczników deklaratywne s GridView i ObjectDataSource powinien wyglądać podobny do następującego:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Jak pokazano na rysunku 7, można edytować widoku GridView wymieniono nazwa, kategoria i ceny każdego produktu w bazie danych. Poświęć chwilę, aby przetestować sortowania funkcji strony wyników, przejrzyj i edytować rekord.


[![Każdego produktu s nazwa, kategoria i cen znajduje się na sortowanie, Pageable, można edytować w widoku GridView](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Rysunek 7**: s każdego produktu, nazwa, kategoria i cen znajduje się na sortowanie, Pageable, można edytować w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Krok 3: Badanie po elemencie ObjectDataSource jest żąda danych

`Products` GridView pobiera jego danych do wyświetlenia za pomocą `Select` metody `ProductsDataSource` ObjectDataSource. Ten element ObjectDataSource tworzy wystąpienie s warstwy logiki biznesowej `ProductsBLL` klasy i wywołania jego `GetProducts()` metodę, która z kolei wywołuje s Warstwa dostępu do danych `ProductsTableAdapter` s `GetProducts()` metody. Metoda DAL nawiązuje połączenie z bazą danych Northwind i wystawia skonfigurowanego `SELECT` zapytania. Te dane jest następnie zwracany do warstwy DAL pakiety się w programie `NorthwindDataTable`. Obiekt DataTable jest zwracana do logiki warstwy Biznesowej, które zwraca go do elementu ObjectDataSource, zwraca go do widoku GridView. Następnie tworzy widoku GridView `GridViewRow` obiekt dla każdego `DataRow` w elemencie DataTable i każdego `GridViewRow` ostatecznie jest renderowany w kodzie HTML, która jest zwracana do klienta i wyświetlana w przeglądarce odwiedzający s.

Ta sekwencja zdarzeń odbywa się każdym razem, gdy widoku GridView należy powiązać z jej odpowiednie dane. Ma to miejsce, gdy jest najpierw odwiedzoną stronę, podczas przenoszenia z jednej strony danych do innego, podczas sortowania w widoku GridView lub modyfikowanie danych s GridView za pomocą wbudowanych edytowanie lub usuwanie interfejsów. Jeśli stan widoku GridView s jest wyłączone, widoku GridView będzie odbitych również każdej strony. Widoku GridView może również być jawnie odbitych do jego danych przez wywołanie jego `DataBind()` metody.

Zrozumienie pełni częstotliwość, z jaką dane są pobierane z bazy danych, umożliwiają s wyświetlony komunikat wskazujący, gdy dane są pobierane ponownie. Dodawanie formantu etykiety Web powyżej widoku GridView o nazwie `ODSEvents`. Czyści jej `Text` właściwości i zestaw jej `EnableViewState` właściwości `false`. Poniżej etykiety, Dodaj formant sieci Web przycisk i ustaw jej `Text` właściwości ogłaszania zwrotnego.


[![Dodaj etykietę i przycisk do strony powyżej widoku GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Rysunek 8**: dodać etykiety i przycisk do strony powyżej widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Podczas przepływu pracy dostępu do danych, ObjectDataSource s `Selecting` zdarzeń uruchamiany przed utworzeniem obiektu źródłowego i jego skonfigurowana metoda wywołana. Utwórz program obsługi zdarzeń dla tego zdarzenia i Dodaj następujący kod:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Zawsze, gdy element ObjectDataSource wysyła żądanie do architektury w przypadku danych etykiety zostaną wyświetlone zdarzenia Selecting tekst uruchamiany.

Odwiedź stronę tej strony w przeglądarce. Gdy najpierw odwiedzenia strony, zdarzenia Selecting tekstu, uruchamiane są widoczne. Kliknij przycisk odświeżania strony i należy pamiętać, że tekst znika (przy założeniu, że GridView s `EnableViewState` właściwość jest ustawiona na `true`, wartość domyślna). Jest tak dlatego, strony, widoku GridView jest odtworzone z swój stan widoku i w związku z tym t przejdź do elementu ObjectDataSource danych. Sortowanie, stronicowania lub edytowanie danych, jednak powoduje widoku GridView ponownie powiązać ze swoim źródłem danych i w związku z tym zdarzenia Selecting uruchamiany pojawi się tekst.


[![Przy każdym widoku GridView jest odbitych ze swoim źródłem danych, zdarzenia Selecting uruchamiany jest wyświetlana](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Rysunek 9**: przy każdym widoku GridView jest odbitych ze swoim źródłem danych, wyświetlania zdarzenia Selecting uruchamiany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Kliknięcie przycisku powoduje odświeżenie strony przycisku do można odtworzyć z swój stan widoku GridView](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Na rysunku nr 10**: kliknięcie przycisku odświeżania strony powoduje, że do można odtworzyć z swój stan widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Może wydawać się niepotrzebne można pobrać za każdym razem danych jest stronicowana za pośrednictwem lub sortowania danych w bazie danych. Po wszystkich od możemy re przy użyciu domyślnego stronicowania ObjectDataSource ma pobrać wszystkie rekordy podczas wyświetlania pierwszej strony. Nawet jeśli nie ma widoku GridView, sortowanie i stronicowanie pomocy technicznej, musi można pobrać danych z bazy danych zawsze, gdy strona jest najpierw odwiedzona przez dowolnego użytkownika (i w każdym ogłaszania zwrotnego, jeśli stan widoku jest wyłączone). Ale jeśli widoku GridView jest wyświetlany te same dane dla wszystkich użytkowników, zbędny są tych żądań dodatkowych bazy danych. Dlaczego nie pamięci podręcznej wyniki zwrócone z `GetProducts()` — metoda i powiązania GridView tych buforowane wyniki?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Krok 4: Buforowanie danych za pomocą elementu ObjectDataSource

Wystarczy wybrać ustawienie kilka właściwości, można skonfigurować elementu ObjectDataSource automatycznie buforowania jego pobrane dane w pamięci podręcznej danych ASP.NET. Poniższa lista zawiera podsumowanie właściwości związanych z pamięci podręcznej elementu ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) musi mieć ustawioną `true` Aby włączyć buforowanie. Wartość domyślna to `false`.
- [CacheDuration](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) czas w sekundach, które dane są buforowane. Wartość domyślna to 0. Element ObjectDataSource będą tylko dane z pamięci podręcznej Jeśli `EnableCaching` jest `true` i `CacheDuration` jest ustawiona na wartość większą niż zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) może być ustawiony na `Absolute` lub `Sliding`. Jeśli `Absolute`, ObjectDataSource buforuje jego pobrane dane w celu `CacheDuration` sekundy; Jeśli `Sliding`, dane wygasną tylko wtedy, gdy nie uzyska dostępu dla `CacheDuration` sekund. Wartość domyślna to `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) tej właściwości należy użyć do skojarzenia wpisów pamięci podręcznej s ObjectDataSource z istniejących zależności pamięci podręcznej. Wpisy danych s ObjectDataSource może przedwcześnie wykluczony z pamięci podręcznej poprzez wygaszenie skojarzone `CacheKeyDependency`. Ta właściwość jest najczęściej używana do skojarzenia z pamięci podręcznej s ObjectDataSource zależności bufora SQL, temat firma Microsoft będzie Eksplorowanie w przyszłości [przy użyciu zależności buforu SQL](using-sql-cache-dependencies-cs.md) samouczka.

Let s skonfigurować `ProductsDataSource` ObjectDataSource do jego dane z pamięci podręcznej przez 30 sekund na skalę bezwzględną. Ustaw element ObjectDataSource s `EnableCaching` właściwości `true` i jego `CacheDuration` właściwości do 30. Pozostaw `CacheExpirationPolicy` właściwości wartości domyślne, `Absolute`.


[![Skonfiguruj ObjectDataSource do buforowania danych dla 30 sekund](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Rysunek 11**: Konfigurowanie ObjectDataSource do buforowania danych dla 30 sekund ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Zapisz zmiany i ponownie tę stronę w przeglądarce. Zaznaczanie tekstu zdarzenia wywoływane po pojawi najpierw odwiedź stronę, jak początkowo dane nie są w pamięci podręcznej. Choć nie kolejnych ogłaszania zwrotnego wyzwalane przez kliknięcie przycisku odświeżania strony, sortowania, stronicowania lub przycisków edycji lub Anuluj *nie* ponowne wyświetlanie zdarzenia Selecting uruchamiany tekstu. Jest to spowodowane `Selecting` zdarzenia generowane tylko, gdy element ObjectDataSource pobiera dane z jego obiekt źródłowy; `Selecting` zdarzeń nie jest wyzwalana, gdy dane są pobierane z pamięci podręcznej danych.

Po 30 sekund zostanie wykluczona dane z pamięci podręcznej. Dane również zostanie usunięty z pamięci podręcznej, jeśli element ObjectDataSource s `Insert`, `Update`, lub `Delete` metody są wywoływane. W związku z tym, po upływie 30 sekund lub przycisku Aktualizuj zostanie kliknięta, sortowania, stronicowania, lub przycisków edycji lub Anuluj spowoduje, że element ObjectDataSource można pobrać danych z jego obiekt źródłowy, wyświetlanie zdarzenia Selecting uruchamiany tekstu po `Selecting` generowane zdarzenie. Te zwracane wyniki są umieszczane do pamięci podręcznej danych.

> [!NOTE]
> Jeśli widzisz Zaznaczanie tekstu zdarzenia wywoływane często, nawet jeśli oczekujesz ObjectDataSource działa z pamięci podręcznej danych może być z powodu ograniczeń pamięci. Jeśli nie jest wystarczająca ilość wolnej pamięci, danych dodanych do pamięci podręcznej przez element ObjectDataSource może zostać oczyścił. Jeśli element ObjectDataSource t wydają się być poprawnie buforowanie danych lub tylko pamięci podręcznej danych sporadycznie, Zamknij niektóre aplikacje, aby zwolnić pamięć i spróbuj ponownie.


Rysunek 12 przedstawiono s ObjectDataSource buforowanie przepływu pracy. Podczas zdarzenia Selecting uruchamiany tekst jest wyświetlany na ekranie, ponieważ dane nie była w pamięci podręcznej i musiały być pobierana z obiektu źródłowego. Jeśli brakuje ten tekst jednak go s, ponieważ dane były dostępne z pamięci podręcznej. Gdy dane są zwracane z pamięci podręcznej istnieje s wykonywać Brak wywołania do obiektu źródłowego i w związku z tym bez określenia zapytania bazy danych.


![Element ObjectDataSource magazynów i pobiera dane z pamięci podręcznej danych](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Rysunek 12**: element ObjectDataSource przechowuje i pobiera dane z pamięci podręcznej danych


Każda aplikacja ASP.NET ma własne wystąpienie tego s współużytkowana przez wszystkie strony i gości pamięci podręcznej danych. Oznacza to, że dane przechowywane w pamięci podręcznej danych przez element ObjectDataSource podobnie jest współużytkowana przez wszystkich użytkowników, którzy odwiedź stronę. Aby to sprawdzić, należy otworzyć `ObjectDataSource.aspx` strony w przeglądarce. Podczas odwiedzania najpierw strony, Zaznaczanie tekstu zdarzenia wywoływane pojawi się (przy założeniu, że dane dodanych do pamięci podręcznej przez powyższych testów w obecnie został wykluczony). Otwórz drugie wystąpienie przeglądarki i skopiuj i wklej adres URL z pierwszego wystąpienia przeglądarki do drugiego. W drugim wystąpieniu przeglądarki, Zaznaczanie tekstu zdarzenia wywoływane jest nie wyświetlany, ponieważ jego s korzystającej z tego samego buforowane dane jako pierwszy.

Podczas wstawiania danych pobrane do pamięci podręcznej, ObjectDataSource korzysta z wartości klucza pamięci podręcznej, który obejmuje: `CacheDuration` i `CacheExpirationPolicy` wartości właściwości; Typ obiektu podstawowego firm używany przez element ObjectDataSource, które jest określone za pomocą [ `TypeName` właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, w tym przykładzie); wartość `SelectMethod` właściwości oraz nazwy i wartości parametrów w `SelectParameters` kolekcji; i wartości jego `StartRowIndex`i `MaximumRows` właściwości, które są używane podczas implementowania [stronicowania niestandardowego.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Obsługuje tworzenie pamięci podręcznej wartość klucza jako połączenie tych właściwości zapewnia wpis pamięci podręcznej unikatowy po zmianie tych wartości. Na przykład w ciągu ostatnich samouczkach możemy kolejnych przeglądał przy użyciu `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)`, która zwraca wszystkich produktów dla określonej kategorii. Jeden użytkownik może się napoje strony i widoku, który ma `CategoryID` 1. Jeśli element ObjectDataSource buforowane wyniki bez względu na `SelectParameters` wartości, gdy inny użytkownik dołączone do strony do wyświetlenia przyprawy podczas produktów napoje znajdowały się w pamięci podręcznej, d zobaczy produktów napoju pamięci podręcznej, a nie przyprawy. Przez zróżnicowanie klucz pamięci podręcznej przez te właściwości, które obejmują wartości `SelectParameters`, ObjectDataSource przechowuje wpis w pamięci podręcznej osobne dla napojów i przypraw.

## <a name="stale-data-concerns"></a>Dotyczy starych danych

Element ObjectDataSource wyklucza mogą automatycznie jego elementy z pamięci podręcznej, gdy jeden z jego `Insert`, `Update`, lub `Delete` wywołaniu metody. Pozwala to chronić przed stare dane czyszcząc się wpisy w pamięci podręcznej modyfikacji danych za pośrednictwem strony. Jednak jest możliwe w dla elementu ObjectDataSource nadal wyświetlany stare dane w formacie buforowania. Ogólnie rzecz biorąc można go z powodu zmiany bezpośrednio w bazie danych. Być może administrator bazy danych właśnie uruchomiono skrypt, który modyfikuje niektóre rekordy w bazie danych.

W tym scenariuszu można również ujawniać w sposób bardziej niewielkie. Element ObjectDataSource wyklucza mogą jej elementów z pamięci podręcznej, gdy jeden z jego metody modyfikacji danych jest wywoływana, usunąć buforowane elementy są dla elementu ObjectDataSource s kombinacji wartości właściwości (`CacheDuration`, `TypeName`, `SelectMethod`, i tak dalej). Jeśli masz dwie ObjectDataSources, które używają różnych `SelectMethods` lub `SelectParameters`, ale nadal można zaktualizować te same dane, jeden element ObjectDataSource może zaktualizowania wiersza i unieważnić własną wpisy w pamięci podręcznej, ale odpowiednim dla drugiego elementu ObjectDataSource nadal będzie można zrealizować z pamięci podręcznej. I zachęca do tworzenia stron, które mają działać tej funkcji. Utwórz strona, wyświetlająca można edytować pobierający jego dane z ObjectDataSource, która korzysta z pamięci podręcznej i jest skonfigurowany do pobierania danych z widoku GridView `ProductsBLL` klasy s `GetProducts()` metody. Dodaj inny można edytować widoku GridView i ObjectDataSource do tej strony (lub innego), ale dla tego drugiego elementu ObjectDataSource mają go używać `GetProductsByCategoryID(categoryID)` metody. Od dwóch ObjectDataSources `SelectMethod` właściwości różnią się one ll każdego mają własne wartości pamięci podręcznej. Po zmodyfikowaniu produktu w siatce jeden, przy następnym powiązać dane do innych siatki (przez stronicowania, sortowanie i tak dalej), spowoduje to nadal obsługiwać stare, buforowane dane i uwzględniają zmiany wprowadzone od innych siatki.

Krótko mówiąc używać tylko na podstawie czasu expiries Jeśli zgadzasz się potencjalnie starych danych i Użyj krótszej expiries w scenariuszach, których aktualność danych jest ważna. Jeśli stare dane nie są dopuszczalne, zrezygnujesz z buforowania albo użyj zależności buforu SQL (zakładając, że są to dane z bazy danych należy re buforowania). Firma Microsoft będzie Poznaj zależności buforu SQL w przyszłości samouczka.

## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy zbadać ObjectDataSource s wbudowanych możliwości buforowania. Wystarczy wybrać ustawienie kilka właściwości, możemy poinstruować ObjectDataSource buforować wyniki zwrócone z określonego `SelectMethod` do pamięci podręcznej danych ASP.NET. `CacheDuration` i `CacheExpirationPolicy` właściwości wskazują czas trwania elementu jest buforowany i czy jest bezwzględnym czy przedłużanie ważności. `CacheKeyDependency` Właściwość kojarzy wszystkich wpisów pamięci podręcznej s ObjectDataSource z istniejących zależności pamięci podręcznej. Może być używany do wykluczenia wpisy s ObjectDataSource z pamięci podręcznej, zanim na podstawie czasu wygaśnięcia osiągnięciu i zwykle jest używana z zależności buforu SQL.

Ponieważ ObjectDataSource buforuje po prostu wartości do pamięci podręcznej danych, firma Microsoft może replikować ObjectDataSource s wbudowanej funkcji programowo. Go t ma sensu w tym celu w warstwie prezentacji, ponieważ element ObjectDataSource zapewnia tej funkcji bez, ale możemy wdrożyć buforowania w oddzielnych warstwy architektury. Aby to zrobić, potrzebujemy Powtórz tej samej logiki używany przez element ObjectDataSource. Firma Microsoft będzie Poznaj programowo pracy z pamięci podręcznej danych z wewnątrz architektury w naszym samouczku dalej.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [ASP.NET buforowanie: Technik i najlepszych rozwiązań](https://msdn.microsoft.com/en-us/library/aa478965.aspx)
- [Przewodnik dotyczący architektury buforowania dla aplikacji .NET Framework](https://msdn.microsoft.com/en-us/library/ee817645.aspx)
- [Buforowanie wyjściowe w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Dalej](caching-data-in-the-architecture-cs.md)
