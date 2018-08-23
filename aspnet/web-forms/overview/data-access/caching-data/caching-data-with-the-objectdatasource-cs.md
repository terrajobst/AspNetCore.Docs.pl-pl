---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Buforowanie danych za pomocą kontrolki ObjectDataSource (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Buforowanie może oznaczać różnicę między wolnej i szybkie aplikacji sieci Web. W tym samouczku jest to pierwsza z czterema, które Przyjrzyj się szczegółowe buforowanie na platformie ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 770b1ae92db8384a0c0b7f038b1ea994ae4c1b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754475"
---
<a name="caching-data-with-the-objectdatasource-c"></a>Buforowanie danych za pomocą kontrolki ObjectDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) lub [Pobierz plik PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Buforowanie może oznaczać różnicę między wolnej i szybkie aplikacji sieci Web. Ten samouczek to pierwsza z czterema, które Przyjrzyj się szczegółowe buforowania w programie ASP.NET. Dowiedz się, kluczowe założenia buforowania i jak stosować buforowania do warstwy prezentacji za pomocą kontrolki ObjectDataSource.


## <a name="introduction"></a>Wprowadzenie

Informatyki *buforowania* to proces przyjmowania danych lub informacji, która jest kosztowna uzyskać i przechowywaniu jej kopię w lokalizacji, która jest szybsze, aby uzyskać dostęp do. W przypadku aplikacji opartych na danych dużych i złożonych zapytań często zużywa większość czasu wykonywania aplikacji s. Takie s wydajności aplikacji, w efekcie często można zwiększyć za przechowywanie wyników zapytania kosztowne bazy danych w pamięci s aplikacji.

Program ASP.NET 2.0 oferuje różnorodne opcje buforowania. Cała strona sieci web lub znaczników s renderowania kontrolki użytkownika mogą być buforowane za pośrednictwem *buforowania danych wyjściowych*. Kontrolki ObjectDataSource i użyciu kontrolki SqlDataSource możliwości buforowania także, w tym samym, dzięki czemu dane pamięci podręcznej na poziomie kontroli. I ASP.NET s *pamięci podręcznej danych* zapewnia rozbudowane buforowania interfejsu API, który umożliwia deweloperom strony programowo obiektów w pamięci podręcznej. W tym samouczku i dalej trzech, które zajmiemy się przy użyciu ObjectDataSource s buforowania funkcji, jak również w pamięci podręcznej danych. Ponadto przedstawimy sposobu buforowania danych całej aplikacji, podczas uruchamiania i przechowywać dane w pamięci podręcznej od nowa za pomocą zależności pamięci podręcznej SQL. Te samouczki nie zapoznaj się z buforowania danych wyjściowych. Aby uzyskać szczegółowy widok buforowania danych wyjściowych, zobacz [buforowania danych wyjściowych programu ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Buforowanie można stosować w dowolnym miejscu w architekturze z warstwy dostępu do danych się za pośrednictwem warstwy prezentacji. W tym samouczku Zapoznamy się stosowanie pamięci podręcznej do warstwy prezentacji za pomocą kontrolki ObjectDataSource. W następnym samouczku, który zajmiemy się buforowanie danych przy warstwy logiki biznesowej.

## <a name="key-caching-concepts"></a>Klucz pamięci podręcznej pojęcia

Buforowanie może znacznie poprawić s aplikacja ogólna wydajność i skalowalność przez pobranie danych, która jest kosztowna wygenerować i przechowywanie kopii w lokalizacji dostępnej dla bardziej efektywnie. Ponieważ pamięć podręczna przechowuje tylko kopii rzeczywistych danych podstawowych, może stać się nieaktualna, lub *starych*, w przypadku zmiany danych bazowych. Aby walczyć z tym, deweloper strony można wskazać kryteria, według których element pamięci podręcznej będzie *wykluczona* z pamięci podręcznej, za pomocą:

- **Na podstawie czasu kryteria** elementu mogą być dodawane do pamięci podręcznej bezwzględną lub przedłużanie czasu trwania. Na przykład deweloper strony może wskazywać czas trwania, powiedz, 60 sekund. Bezwzględny czas trwania element pamięci podręcznej jest wykluczony 60 sekund po zostało dodane do pamięci podręcznej, niezależnie od tego, jak często została otwarta. Z czasem przewijania element pamięci podręcznej jest wykluczony 60 sekund od ostatniego dostępu.
- **Kryteria na podstawie zależności** zależności mogą być skojarzone z elementem, gdy jest dodawane do pamięci podręcznej. Po zmianie zależności element s wartość zostanie usunięty z pamięci podręcznej. Zależność może być plikiem, inny element pamięci podręcznej lub kombinacji obu. Program ASP.NET 2.0 umożliwia także zależności pamięci podręcznej SQL, które umożliwia deweloperom Dodawanie elementu do pamięci podręcznej i jest wykluczona po zmianie danych bazowych bazy danych. Będziemy sprawdzać zależności pamięci podręcznej SQL w przyszłych [przy użyciu zależności pamięci podręcznej SQL](using-sql-cache-dependencies-cs.md) samouczka.

Niezależnie od tego, określone kryteria wykluczenia elementu w pamięci podręcznej może być *oczyszczany* przed kryteriów opartych na czasie lub na podstawie zależności zostały spełnione. Jeśli pamięć podręczna osiągnął limit pojemności, należy usunąć istniejące elementy, przed dodaniem nowych. W związku z tym pracując programowo przy użyciu danych z pamięci podręcznej go s istotne, że należy zawsze przyjęto założenie, że dane w pamięci podręcznej mogą nie występować. Omówimy wzorzec używany podczas uzyskiwania dostępu do danych z pamięci podręcznej programowo w naszym samouczku dalej *buforowania danych w architekturze*.

Pamięć podręczna zapewnia ekonomiczny sposób ściskanie lepszą wydajność aplikacji. Jako [Steven Smith](http://aspadvice.com/blogs/ssmith/) articulates w swoim artykule [buforowania platformy ASP.NET: technik i najlepszych rozwiązań](https://msdn.microsoft.com/library/aa478965.aspx):

Pamięć podręczna może być dobrym sposobem uzyskać dobrą dostateczną wydajność bez konieczności mnóstwo czasu i analizy. Pamięć jest tanie, dlatego można uzyskać wydajność przez buforowanie danych wyjściowych przez 30 sekund, nie trzeba spędzać dnia lub tygodnia, próby optymalizacji kodu lub bazy danych, wykonaj rozwiązanie pamięci podręcznej (przy założeniu 30 - starych sekundę danych jest ok) i przejść. Po pewnym czasie słabym projektem będzie prawdopodobnie nadążyć, więc kursu, należy starać się prawidłowo zaprojektować aplikacje. Ale jeśli potrzebujesz uzyskać już dzisiaj dostateczną wydajność, buforowanie może być doskonałym [Metoda], zakupy, czas do refaktoryzacji aplikacji w późniejszym terminie, gdy masz czas, aby to zrobić.

Gdy buforowanie zapewniają ulepszenia wydajności znaczące, nie ma on zastosowania we wszystkich sytuacjach, takich jak za pomocą aplikacji, które korzystają z danych w czasie rzeczywistym, często aktualizowania lub gdzie nawet krótko znajdowały się nieaktualnych danych stanie się nieakceptowalna. Jednak w przypadku większości aplikacji, buforowanie należy użyć. Aby uzyskać więcej ogólnych informacji na temat buforowania programu ASP.NET 2.0, zobacz [buforowania dla wydajności](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) części [samouczki szybkiego startu programu ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Krok 1: Tworzenie pamięci podręcznej stron sieci Web

Zanim zaczniemy naszych eksploracji funkcji buforowania s ObjectDataSource, umożliwiają s najpierw Poświęć chwilę, do tworzenia stron ASP.NET w naszym projektu witryny sieci Web, który będziemy potrzebować dla tego samouczka i trzy dalej. Rozpocznij od dodania nowy folder o nazwie `Caching`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Dodawanie stron ASP.NET samouczki dotyczące buforowania](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki dotyczące buforowania


Podobnie jak w przypadku innych folderów `Default.aspx` w `Caching` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Rysunek 2: Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Rysunek 2**: rysunek 2: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Wreszcie, Dodaj te strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po zakończeniu pracy z danymi binarnymi `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy samouczki buforowania.


![Mapa witryny zawiera teraz wpisy pamięci podręcznej samouczki](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Rysunek 3**: mapy witryny zawiera teraz wpisy pamięci podręcznej samouczki


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Krok 2: Wyświetlanie listy produktów na stronie sieci Web

W tym samouczku przedstawiono sposób użycia ObjectDataSource kontroli s wbudowanych buforowania funkcji. Aby umożliwić wyświetlanie tych funkcji, jednak najpierw należy strony, aby działało z. Umożliwiają s utworzyć stronę sieci web, która korzysta z GridView do listy informacji o produkcie pobierane przez kontrolki ObjectDataSource z `ProductsBLL` klasy.

Zacznij od otwarcia `ObjectDataSource.aspx` stronie `Caching` folderu. Przeciągnij GridView z przybornika w projektancie, ustaw jego `ID` właściwości `Products`i w tagu inteligentnego, chcesz powiązać kontrolkę ObjectDataSource o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource do pracy z `ProductsBLL` klasy.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Na tej stronie umożliwiają s Utwórz GridView można edytować, aby można sprawdzić, co się stanie po zmodyfikowaniu dane buforowane w kontrolki ObjectDataSource za pośrednictwem interfejsu s GridView. Należy pozostawić listy rozwijanej wybierz OPCJĘ karty ustawiona na wartość domyślną `GetProducts()`, ale zmienić wybrany element na karcie aktualizacji `UpdateProduct` przeciążenie, które akceptuje `productName`, `unitPrice`, i `productID` jako jego parametry wejściowe.


[![Ustawianie listy rozwijanej aktualizacji karty s przeciążenia UpdateProduct odpowiednie](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Rysunek 5**: Ustaw s kartę aktualizacji listy rozwijanej zastosowanie `UpdateProduct` przeciążenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Na koniec Ustaw list rozwijanych w kartach INSERT i DELETE (Brak) i kliknij przycisk Zakończ. Po ukończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio ustawia ObjectDataSource s `OldValuesParameterFormatString` właściwość `original_{0}`. Zgodnie z opisem w [Przegląd Wstawianie, aktualizowanie i usuwanie danych](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) samouczek, ta właściwość musi zostać usunięte z składni deklaratywnej albo powraca do wartości domyślnej, `{0}`, w kolejności dla przepływu pracy przez naszych aktualizacji Kontynuuj bez błędów.

Ponadto po zakończeniu działania kreatora programu Visual Studio dodaje pole do kontrolki GridView dla każdego pola danych produktu. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, i `UnitPrice` BoundFields. Następnie zaktualizuj `HeaderText` właściwości każdego z tych BoundFields produkt, kategoria i ceny, odpowiednio. Ponieważ `ProductName` pole jest wymagane, Konwertowanie elementu BoundField TemplateField i dodać RequiredFieldValidator do `EditItemTemplate`. Podobnie, przekonwertować `UnitPrice` elementu BoundField do TemplateField i Dodaj CompareValidator nad zapewnieniem wpisana przez użytkownika wartość waluty prawidłową wartość s, tym większa lub równa zero. Oprócz tych zmian, możesz wykonać wszelkie estetycznych zmiany, takie jak wyrównanie do prawej `UnitPrice` wartość lub Określanie formatowania `UnitPrice` tekstu w interfejsach tylko do odczytu i edycji.

Należy widoku GridView można edytować, zaznaczając pole wyboru Włącz edytowanie w tagu inteligentnego s GridView. Sprawdź także włączone stronicowanie i sortowanie Włącz pola wyboru.

> [!NOTE]
> Potrzebujesz przeglądu sposobu dostosowywania interfejsu edycji kontrolki GridView s? Jeśli tak, odwołaj się do [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczka.


[![Włącz obsługę GridView do edycji, sortowania i stronicowania](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Rysunek 6**: Włącz GridView obsługę edycji, sortowania i stronicowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Po wprowadzeniu tych zmian GridView, kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Jak pokazano na rysunku 7, wyświetla listę GridView można edytować nazwy, kategorii i cenę każdego z produktów w bazie danych. Poświęć chwilę, w celu przetestowania sortowania funkcji strony s wyniki strony za pomocą ich i edytować rekord.


[![Każdy produkt s nazwy, kategorii i cena znajduje się w sortowanie, Pageable, można edytować kontrolki GridView](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Rysunek 7**: s każdego produktu, nazwa, kategoria i cena znajduje się w sortowanie, Pageable, można edytować kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Krok 3: Sprawdzenie podczas kontrolki ObjectDataSource jest żądanie danych

`Products` GridView pobiera jego danych do wyświetlenia, wywołując `Select` metody `ProductsDataSource` ObjectDataSource. Tego elementu ObjectDataSource tworzy wystąpienie klasy s warstwy logiki biznesowej `ProductsBLL` klasy i wywołuje jego `GetProducts()` metody, która z kolei wywołuje s warstwy dostępu do danych `ProductsTableAdapter` s `GetProducts()` metody. Metoda DAL nawiązuje połączenie z bazą danych Northwind i wystawia skonfigurowanych `SELECT` zapytania. Tych danych jest zwracany z warstwą dal pakietów ją w górę w `NorthwindDataTable`. Obiekt DataTable, jest zwracana do LOGIKI, która zwraca go do elementu ObjectDataSource, zwraca go do widoku GridView. Następnie tworzy widoku GridView `GridViewRow` obiekt dla każdego `DataRow` w elemencie DataTable i każdy `GridViewRow` ostatecznie jest renderowany w kodzie HTML, która jest zwracana do klienta i wyświetlane w przeglądarce odwiedzający s.

Następująca sekwencja zdarzeń odbywa się z każdym razem, gdy widoku GridView należy powiązać z jego danych źródłowych. Tak się stanie, gdy jest najpierw odwiedzoną stronę, podczas przenoszenia z jednej strony danych do innego, podczas sortowania widoku GridView lub podczas modyfikowania danych s GridView przy użyciu jego wbudowanych, edytowanie lub usuwanie interfejsów. Jeśli stan widoku GridView s jest wyłączona, widoku GridView będzie odbitych również każdej strony. Kontrolki GridView może również być jawnie odbitych do jego danych przez wywołanie jego `DataBind()` metody.

Aby w pełni docenić częstotliwość, z jaką dane są pobierane z bazy danych, umożliwiają s wyświetlić komunikat wskazujący, gdy dane są ponownie pobierane. Dodaj kontrolkę etykieta Web powyżej GridView o nazwie `ODSEvents`. Czyści jej `Text` właściwości i ustaw jego `EnableViewState` właściwość `false`. Poniżej etykiety, Dodaj kontrolkę przycisku w sieci Web, a następnie ustaw jego `Text` właściwość zwrotu.


[![Dodaj etykietę i przycisk do strony powyżej widoku GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Rysunek 8**: Dodaj etykietę i przycisk do strony powyżej kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Podczas przepływ dostępu do danych, ObjectDataSource s `Selecting` uruchamiany zdarzeń przed utworzeniem obiektu źródłowego i jego skonfigurowana metoda wywoływana. Utwórz procedurę obsługi zdarzeń dla tego zdarzenia, a następnie dodaj następujący kod:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Każdorazowo, gdy kontrolki ObjectDataSource kieruje żądanie do architektury danych, etykiety będą wyświetlane zdarzenie wybranie tekstu, które są wywoływane.

Odwiedź tę stronę w przeglądarce. Po pierwsze odwiedzenia strony, zdarzenie Zaznaczanie tekstu, uruchamiane jest wyświetlany. Kliknij przycisk ogłaszania wstecznego i należy pamiętać, że tekst znika (przy założeniu, że GridView s `EnableViewState` właściwość jest ustawiona na `true`, wartość domyślna). To dlatego, na odświeżenie strony, widoku GridView jest odtworzone swój stan widoku i w związku z tym t sięgają ObjectDataSource dla swoich danych. Sortowanie, stronicowanie i edytowanie danych, jednak powoduje, że GridView ponownie powiązać ze swoim źródłem danych i w związku z tym zdarzenie wybranie uruchamiane wyświetleniu tekstu.


[![Po każdym widoku GridView jest odbitych ze swoim źródłem danych, wybranie event uruchamiany jest wyświetlana](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Rysunek 9**: zawsze, gdy widok GridView jest odbitych ze swoim źródłem danych, wybranie event uruchamiany jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Kliknięcie powoduje odświeżenie strony przycisk GridView, aby zostać odtworzone swój stan widoku](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Na rysunku nr 10**: kliknięcie przycisku zwrotu powoduje, że GridView zostać odtworzone swój stan widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Może się to wydawać marnotrawstwa można pobrać za każdym razem danych jest stronicowanej za pośrednictwem lub posortowanych danych w bazie danych. Ponieważ możemy ponownie przy użyciu domyślnego stronicowania kontrolki ObjectDataSource pobierze wszystkie rekordy przy wyświetlaniu pierwszej strony. Nawet wtedy, gdy widoku GridView nie dostarcza, sortowanie i stronicowanie pomocy technicznej, dane muszą pobierane z bazy danych, każdym razem, gdy strony najpierw jest kontrolowane przez żadnego użytkownika (i na każdy zwrotu, jeśli stan widoku jest wyłączone). Ale jeśli widoku GridView są wyświetlane te same dane dla wszystkich użytkowników, tych żądań dodatkowych bazy danych są zbędne. Dlaczego nie pamięci podręcznej wyniki zwrócone z `GetProducts()` metody i powiązania GridView do tych buforowanych wyników?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Krok 4: Buforowanie danych za pomocą kontrolki ObjectDataSource

Wystarczy ustawić kilka właściwości, można skonfigurować kontrolki ObjectDataSource automatycznie pamięci podręcznej pobrane dane w pamięci podręcznej danych ASP.NET. Poniższa lista zawiera podsumowanie właściwości kontrolki ObjectDataSource związane z pamięci podręcznej:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) musi być równa `true` Aby włączyć buforowanie. Wartość domyślna to `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) czas w sekundach, dane są buforowane. Wartość domyślna to 0. Kontrolki ObjectDataSource tylko buforowania danych, jeśli `EnableCaching` jest `true` i `CacheDuration` jest ustawiona na wartość większą niż zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) można ustawić `Absolute` lub `Sliding`. Jeśli `Absolute`, kontrolki ObjectDataSource buforuje jego pobrane dane w celu `CacheDuration` (w sekundach); Jeśli `Sliding`, dane wygasną tylko wtedy, gdy nie jest on dostępny dla `CacheDuration` sekund. Wartość domyślna to `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) tej właściwości należy użyć do skojarzenia z wpisy pamięci podręcznej s ObjectDataSource z istniejących zależności pamięci podręcznej. Wpisy ObjectDataSource s danych może zostać przedwcześnie wykluczony z pamięci podręcznej przez wygasa związanych z nią `CacheKeyDependency`. Ta właściwość jest najczęściej używana do kojarzenia zależności pamięci podręcznej SQL za pomocą kontrolki ObjectDataSource cache s, tematu pokażemy w przyszłości [przy użyciu zależności pamięci podręcznej SQL](using-sql-cache-dependencies-cs.md) samouczka.

Umożliwiają konfigurowanie s `ProductsDataSource` ObjectDataSource do jego dane z pamięci podręcznej przez 30 sekund na skalę bezwzględną. Ustaw ObjectDataSource s `EnableCaching` właściwości `true` i jego `CacheDuration` właściwości do 30. Pozostaw `CacheExpirationPolicy` właściwość ustawioną na wartość domyślną `Absolute`.


[![Konfigurowanie kontrolki ObjectDataSource buforować dane przez 30 sekund](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Rysunek 11**: Konfigurowanie kontrolki ObjectDataSource buforować dane przez 30 sekund ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Zapisz zmiany i ponownie tę stronę w przeglądarce. Zaznaczanie tekstu zdarzenia wywoływane będą wyświetlane, gdy najpierw odwiedź stronę, jako początkowo dane nie są w pamięci podręcznej. Ale kolejne ogłaszania zwrotnego wyzwolone przez kliknięcie przycisku zwrotu, sortowanie, stronicowanie lub naciskać przycisków edycji, lub przycisk Anuluj *nie* ponowne wyświetlanie zdarzeń wybranie wyzwolone tekstu. Jest to spowodowane `Selecting` zdarzenia generowane tylko, gdy kontrolki ObjectDataSource dane są pobierane z jego obiektu podstawowego; `Selecting` zdarzeń nie jest wyzwalana, jeśli dane są pobierane z pamięci podręcznej danych.

Po 30 sekundach danych zostanie wykluczona z pamięci podręcznej. Dane również zostanie wykluczona z pamięci podręcznej, jeśli ObjectDataSource s `Insert`, `Update`, lub `Delete` metody są wywoływane. W związku z tym, po 30 sekund lub przycisk Aktualizuj kliknięto, sortowanie, stronicowanie, przycisków Edytuj lub przycisk Anuluj, spowoduje, że ObjectDataSource można pobrać danych z jego obiekt źródłowy, wyświetlanie zdarzeń wybranie wyzwolone tekstu po `Selecting` generowane zdarzenie. Te zwracane wyniki są umieszczane Wstecz w pamięci podręcznej danych.

> [!NOTE]
> Jeśli widzisz często Zaznaczanie tekstu zdarzenia wywoływane, nawet w przypadku, gdy spodziewasz się ObjectDataSource z dane w pamięci podręcznej, może to być spowodowane ograniczenia pamięci. Jeśli nie jest wystarczająca ilość wolnej pamięci, dane dodane do pamięci podręcznej przez kontrolki ObjectDataSource może został oczyszczana. Jeśli t ObjectDataSource wydają się być poprawnie buforowania danych lub tylko pamięci podręczne danych sporadycznie, Zamknij niektóre aplikacje, aby zwolnić pamięć i spróbuj ponownie.


Rysunek 12 przedstawiono s ObjectDataSource buforowania przepływu pracy. Gdy zdarzenie wybranie uruchamiane tekst jest wyświetlany na ekranie, ponieważ dane nie znajdowała się w pamięci podręcznej i musiały być pobierane z obiektu źródłowego. Jeśli brakuje ten tekst jednak go s, ponieważ dane były dostępne z pamięci podręcznej. Gdy dane są zwracane z pamięci podręcznej miejsca s wykonywać Brak wywołania do bazowego obiektu i w związku z tym, bez określenia zapytania bazy danych.


![Magazyny ObjectDataSource i pobiera dane z pamięci podręcznej danych](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Rysunek 12**: kontrolki ObjectDataSource przechowuje oraz pobiera dane z pamięci podręcznej danych


Każda aplikacja ASP.NET ma swoje własne wystąpienie tego s współużytkowane przez wszystkie strony i osoby odwiedzające pamięci podręcznej danych. Oznacza to, że dane przechowywane w pamięci podręcznej danych przez kontrolki ObjectDataSource podobnie jest współużytkowany przez wszystkich użytkowników, którzy znajduje się na stronie. Aby to sprawdzić, otwórz `ObjectDataSource.aspx` strony w przeglądarce. Po pierwsze, odwiedzając stronę, Zaznaczanie tekstu zdarzenia wywoływane pojawi się (przy założeniu, że dane dodane do pamięci podręcznej przez poprzednie testy w razie został wykluczony). Otwórz drugie wystąpienie przeglądarki i skopiuj i wklej adres URL z pierwszego wystąpienia przeglądarki do drugiego. W drugim wystąpieniu przeglądarki Zaznaczanie tekstu zdarzenia wywoływane jest nie wyświetlany, ponieważ jego s, korzystając z tych samych danych, jak pierwsza w pamięci podręcznej.

Podczas wstawiania jego pobrane dane w pamięci podręcznej, kontrolki ObjectDataSource korzysta z pamięci podręcznej wartości klucza, który obejmuje: `CacheDuration` i `CacheExpirationPolicy` wartości właściwości; Typ obiektu firm bazowego, używany przez kontrolki ObjectDataSource, która została określona za pomocą [ `TypeName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, w tym przykładzie); wartość `SelectMethod` właściwości, jak również nazwy i wartości parametrów w `SelectParameters` kolekcji i wartości jego `StartRowIndex`i `MaximumRows` właściwości, które są używane podczas implementowania [stronicowania niestandardowego.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Umożliwiają utworzenie dobrze dopasowanego wartość klucza pamięci podręcznej pod postacią połączenia tych właściwości zapewnia wpis pamięci podręcznej unikatowy, po zmianie tych wartości. Na przykład w poprzednich samouczkach możemy ve przyjrzano się przy użyciu `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)`, która zwraca wszystkich produktów dla określonej kategorii. Jeden użytkownik może się beverages strony i widoku, który ma `CategoryID` 1. Jeśli kontrolki ObjectDataSource pamięci podręcznej, jego wyniki, bez względu na `SelectParameters` wartości, gdy inny użytkownik pochodzi ze stroną do wyświetlania condiments podczas produktów beverages znajdowały się w pamięci podręcznej, d zobaczy produktów spożywczy pamięci podręcznej, a nie condiments. Przez zróżnicowanie klucz pamięci podręcznej według tych właściwości, które zawierają wartości `SelectParameters`, kontrolki ObjectDataSource zachowuje oddzielny pamięci podręcznej objęcia beverages condiments.

## <a name="stale-data-concerns"></a>Dotyczy nieaktualnych danych

Kontrolki ObjectDataSource automatycznie wyklucza mogą jej elementy z pamięci podręcznej, gdy jeden z jego `Insert`, `Update`, lub `Delete` metody są wywoływane. Pozwala to chronić przed nieaktualnych danych przez wyczyszczenie się wpisy w pamięci podręcznej danych jest modyfikowany przez stronę. Jednak jest możliwe dla elementu ObjectDataSource użycia pamięci podręcznej, aby wyświetlić nadal nieaktualnych danych. W najprostszym przypadku może być z powodu danych zmiany bezpośrednio w bazie danych. Być może administrator bazy danych po prostu uruchomiono skrypt, który modyfikuje niektóre rekordy w bazie danych.

W tym scenariuszu można również ujawniać w bardziej subtelny sposób. Kontrolki ObjectDataSource wyklucza mogą jego elementów z pamięci podręcznej, gdy wywoływana jest jeden z jego metod modyfikacji danych, pamięci podręcznej usunięto następującą liczbę elementów są kontrolki ObjectDataSource s określonej kombinacji wartości właściwości (`CacheDuration`, `TypeName`, `SelectMethod`, i tak dalej). Jeśli masz dwa ObjectDataSources, które używają różnych `SelectMethods` lub `SelectParameters`, ale nadal można zaktualizować te same dane, a jeden ObjectDataSource może zaktualizuj wiersz unieważnienie swój własny wpisy w pamięci podręcznej, ale odpowiedni wiersz dla drugiego elementu ObjectDataSource będą one nadal obsługiwane z pamięci podręcznej. Zachęcam Cię do tworzenia stron, które mają wykazują tej funkcji. Utwórz strona, wyświetlająca GridView można edytować, która ściąga dane z kontrolki ObjectDataSource, która korzysta z pamięci podręcznej i jest skonfigurowany do pobierania danych `ProductsBLL` klasy s `GetProducts()` metody. Dodaj kolejną edytowalne GridView ObjectDataSource do tej strony (lub innej), ale w przypadku tej drugiej kontrolki ObjectDataSource się go użyć `GetProductsByCategoryID(categoryID)` metody. Od dwóch ObjectDataSources `SelectMethod` właściwości różnią się one ll każda ma własne wartości z pamięci podręcznej. Jeśli edytujesz produktu w jednej siatce przy następnym powiązanie danych do innych siatki (przez stronicowania, sortowania i tak dalej), spowoduje to nadal obsługiwać stare, buforowane dane i nie odzwierciedlać zmiany, który został wykonany z innych siatki.

Krótko mówiąc składać się tylko na podstawie czasu expiries Jeśli zgadzasz się potencjalnie nieaktualnych danych i używać krótszy expiries dla scenariuszy, w których jest ważna aktualność danych. Jeśli nieaktualnych danych nie jest dopuszczalne, zrezygnujesz z pamięci podręcznej albo użyj zależności pamięci podręcznej SQL (zakładając, że są to dane z bazy danych należy ponownie buforowania). W przyszłości samouczku przyjrzymy się zależności pamięci podręcznej SQL.

## <a name="summary"></a>Podsumowanie

W tym samouczku zbadaliśmy ObjectDataSource s wbudowanych funkcji buforowania. Wystarczy ustawić kilka właściwości, firma Microsoft może wydać ObjectDataSource buforowanie wyników zwróconych z określonego `SelectMethod` w pamięci podręcznej danych ASP.NET. `CacheDuration` i `CacheExpirationPolicy` właściwości wskazują, czas trwania elementu są buforowane i czy jest bezwzględną lub przedłużanie ważności. `CacheKeyDependency` Właściwość kojarzy wszystkie wpisy pamięci podręcznej s ObjectDataSource z istniejących zależności pamięci podręcznej. Może to służyć do wykluczenia wpisy s ObjectDataSource z pamięci podręcznej, zanim na podstawie czasu wygaśnięcia zostanie osiągnięty i jest zwykle używany z zależności pamięci podręcznej SQL.

Ponieważ kontrolki ObjectDataSource buforuje po prostu jego wartości w pamięci podręcznej danych, firma Microsoft może replikować ObjectDataSource s wbudowanej funkcji programowo. Go t sens, w tym celu w warstwie prezentacji, ponieważ kontrolki ObjectDataSource oferuje gotowych tej funkcji, ale firma Microsoft zaimplementowania funkcji buforowania w oddzielne warstwy architektury. Aby to zrobić, należy powtórzyć tę samą logikę, które są używane przez kontrolki ObjectDataSource. Pokażemy, jak programowo pracować z pamięci podręcznej danych z w ramach architektury w naszym samouczku dalej.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Program ASP.NET buforowanie: Technik i najlepszych rozwiązań](https://msdn.microsoft.com/library/aa478965.aspx)
- [Buforowanie w Przewodniku dotyczącym architektury dla aplikacji .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Dane wyjściowe pamięci podręcznej na platformie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](caching-data-in-the-architecture-cs.md)
