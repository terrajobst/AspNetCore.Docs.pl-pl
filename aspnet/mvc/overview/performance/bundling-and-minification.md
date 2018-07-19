---
uid: mvc/overview/performance/bundling-and-minification
title: Tworzenie pakietów i minimalizowanie | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Tworzenie pakietów i minimalizowanie są dwie metody można użyć w programie ASP.NET 4.5, aby poprawić czas ładowania żądań. Tworzenie pakietów i minimalizowanie poprawia czas ładowania przez reducin...
ms.author: aspnetcontent
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 4e72804593c07318af8cc577f9d43ab96be4de05
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123791"
---
<a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Tworzenie pakietów i minimalizowanie są dwie metody można użyć w programie ASP.NET 4.5, aby poprawić czas ładowania żądań. Tworzenie pakietów i minimalizowanie poprawia czas ładowania dzięki zmniejszeniu liczby żądań do serwera oraz redukcję rozmiaru żądanych zasobów (na przykład CSS i JavaScript.)


Większość bieżącego ważniejszymi przeglądarkami ograniczyć liczbę [równoczesnych połączeń](http://www.browserscope.org/?category=network) dla każdej nazwy hosta na 6. Oznacza to, że podczas przetwarzania żądania sześciu dodatkowych żądań zasobów na hoście zostanie umieszczona w kolejce przez przeglądarkę. Na poniższej ilustracji kart sieciowych narzędzia dla deweloperów F12 w przeglądarce IE pokazuje chronometraż zasoby wymagane przez widok informacje przykładowej aplikacji.

![B/M](bundling-and-minification/_static/image1.png)

Szara słupki pokazują czas żądania jest umieszczane w kolejce przez przeglądarkę, oczekiwanie na limit sześć połączeń. Żółty pasek jest czas żądania do pierwszego bajtu, oznacza to, że czas potrzebny na wysłanie żądania i odbierania pierwszej odpowiedzi z serwera. Niebieski słupki pokazują czas potrzebny na odebranie dane odpowiedzi z serwera. Możesz dwukrotnie kliknąć na zasobu, aby uzyskać szczegółowe informacje o czasie. Na przykład na poniższej ilustracji przedstawiono szczegółów chronometrażu dla ładowania */Scripts/MyScripts/JavaScript6.js* pliku.

![](bundling-and-minification/_static/image2.png)

Poprzedni obraz przedstawia **Start** zdarzenia, które daje czas żądanie zostało umieszczone w kolejce z powodu przeglądarki Ogranicz liczbę równoczesnych połączeń. W tym przypadku żądanie zostało umieszczone w kolejce przez 46 milisekund oczekiwania na zakończenie innego żądania.

## <a name="bundling"></a>Tworzenie pakietów

Tworzenie pakietów to nowa funkcja w programie ASP.NET 4.5, który można łatwo połączyć lub wiele plików pakietu w jednym pliku. Możesz utworzyć CSS, JavaScript i innych pakietów. Mniej plików oznacza, że mniejszej liczby żądań HTTP i który może zwiększyć pierwszy wydajność ładowania strony.

Na poniższej ilustracji przedstawiono ten sam widok chronometrażu w widoku informacje wyświetlane wcześniej, ale tym razem z tworzenie pakietów i minimalizowanie włączone.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimalizowanie

Minimalizowanie wykonuje różne optymalizacje inny kod skryptów lub css, takie jak usuwanie niepotrzebnych białych znaków i komentarze i skrócenie nazwy zmiennych, jeden znak. Należy wziąć pod uwagę następujące funkcja języka JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Po gotowy do przeprowadzania minifikacji funkcja jest ograniczone do następujących:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Oprócz usuwanie komentarzy i niepotrzebnych odstępów, następujących parametrów i nazwy zmiennych zostały zmienione (skrócona) w następujący sposób:

| **Oryginał** | **Zmieniono nazwę** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | mogę |

## <a name="impact-of-bundling-and-minification"></a>Wpływ tworzenia pakietów i minimalizowanie

W poniższej tabeli przedstawiono kilka istotnych różnic między ofercie wszystkie zasoby pojedynczo i tworzenie pakietów i minimalizowanie (B/M) w programie próbki.

|  | **Za pomocą B/M** | **Bez B/M** | **Zmiany** |
| --- | --- | --- | --- |
| **Żądań plików** | 9 | 34 | 256% |
| **KB Sent** | 3.26 | 11.92 | 266% |
| **Odebrano KB** | 388.51 | 530 | 36% |
| **Czas ładowania** | 510 MS | 780 MS | 53% |

Wysłane bajty ma znacznego zmniejszenia za pomocą tworzenia pakietów, ponieważ przeglądarki są dość pełne przy użyciu nagłówków HTTP, które stosują w odpowiedzi na żądania. Redukcja odebranych bajtów nie jest tak duży, ponieważ największych plików (*skrypty\\jquery-ui-1.8.11.min.js* i *skrypty\\jquery 1.7.1.min.js*) są już zminimalizowany . Uwaga: Chronometrażu w przykładowy program używany [Fiddler](http://www.fiddler2.com/fiddler2/) narzędzie, aby symulować wolną sieć. (Z programu Fiddler **reguły** menu, wybierz opcję **wydajności** następnie **symulować szybkości modemu**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debugowanie powiązane i zminimalizowania JavaScript

Łatwo debugował Twój kod JavaScript w środowisku programistycznym (gdzie [kompilacji elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* pliku jest ustawiona na `debug="true"` ), ponieważ pliki JavaScript nie są powiązane lub zminimalizowany. Można również debugować kompilację wydania, w których powiązane i zminimalizowania pliki JavaScript. Przy użyciu narzędzi deweloperskich F12 w przeglądarce IE, debugowania funkcji języka JavaScript zawartych w pakiecie zminimalizowany, przy użyciu następujących podejść:

1. Wybierz **skryptu** , a następnie wybierz pozycję **Rozpocznij debugowanie** przycisku.
2. Wybierz pakiet zawierający funkcja języka JavaScript, który chcesz debugować za pomocą przycisku zasoby.  
    ![](bundling-and-minification/_static/image4.png)
3. Formatowanie zminimalizowanego JavaScript, wybierając **przycisk Konfiguracja** ![](bundling-and-minification/_static/image5.png), a następnie wybierając **formatu JavaScript**.
4. W **skryptu wyszukiwania** pole wprowadzania, wybierz nazwę funkcji, który chcesz debugować. Na poniższej ilustracji **AddAltToImg** została wprowadzona w **skryptu wyszukiwania** pola wejściowego.  
    ![](bundling-and-minification/_static/image6.png)

Aby uzyskać więcej informacji na temat debugowania przy użyciu narzędzi deweloperskich F12, zobacz artykuł w witrynie MSDN [przy użyciu narzędzi deweloperskich F12, aby debugować błędy JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Kontrolowanie tworzenie pakietów i minimalizowanie

Tworzenie pakietów i minimalizowanie jest włączone lub wyłączone, ustawiając wartość atrybutu debugowania w [kompilacji elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* pliku. Następujący kod XML `debug` jest ustawiona na wartość true, dlatego tworzenie pakietów i minimalizowanie jest wyłączona.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Aby włączyć tworzenie pakietów i minimalizowanie, ustaw `debug` wartość "false". Można zastąpić *Web.config* ustawienie z `EnableOptimizations` właściwość `BundleTable` klasy. Poniższy kod umożliwia tworzenie pakietów i minimalizowanie i zastępuje wszystkie ustawienia w *Web.config* pliku.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Chyba że `EnableOptimizations` jest `true` lub atrybutu debugowania w [kompilacji elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* pliku jest ustawiona na `false`, pliki nie zostaną powiązane lub zminimalizowany. Ponadto .min wersję plików, nie będzie używany, zostaną wybrane wersje do debugowania pełne. `EnableOptimizations` przesłania atrybut debugowania w [kompilacji elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* pliku


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Za pomocą tworzenia pakietów i minimalizowanie przy użyciu wzorca ASP.NET Web Forms i stron sieci Web

- Dla stron sieci Web, zobacz wpis na blogu [Dodawanie optymalizacji sieci Web do witryny sieci Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Dla formularzy sieci Web, zobacz wpis na blogu [Dodawanie tworzenie pakietów i minimalizowanie do formularzy sieci Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Za pomocą tworzenia pakietów i minimalizowanie ze wzorca ASP.NET MVC

W tej sekcji utworzymy platformy ASP.NET MVC projekt, aby sprawdzić, tworzenie pakietów i minimalizowanie. Najpierw utwórz nowy projekt ASP.NET MVC, internetowe o nazwie **MvcBM** bez wprowadzania zmian w dowolnej wartości domyślne.

Otwórz *aplikacji\\\_Start\\BundleConfig.cs* plików i zbadaj `RegisterBundles` metodę, która umożliwia tworzenie, rejestrowanie i konfigurowanie pakiety. Poniższy kod ilustruje część `RegisterBundles` metody.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Powyższy kod powoduje utworzenie nowego pakietu języka JavaScript o nazwie *~/bundles/jquery* zawierającą wszystkie odpowiednie (które debugowania lub zminimalizowany, ale nie. *vsdoc*) pliki *skrypty* folder, który pasuje do ciągu z symbolami wieloznacznymi "js ~/Scripts/jquery-{version}". Dla platformy ASP.NET MVC 4, oznacza to, z konfiguracji debugowania, plik *jquery 1.7.1.js* zostaną dodane do pakietu. W konfiguracji wydania *jquery 1.7.1.min.js* zostaną dodane. Tworzenie pakietu framework zgodna z konwencjami kilka typowych takich jak:

- Wybieranie pliku ".min" dla wersji, gdy *FileX.min.js* i *FileX.js* istnieje.
- Wybieranie wersji niż ".min" do debugowania.
- Ignorowanie "-vsdoc" plików (takich jak *jquery-1.7.1-vsdoc.js*), które są używane tylko przez technologię IntelliSense.

`{version}` Symbol wieloznaczny dopasowania powyżej jest używane do automatycznego tworzenia pakietu jQuery z odpowiednią wersją jQuery w swojej *skrypty* folderu. W tym przykładzie za pomocą symbolu wieloznacznego zapewnia następujące korzyści:

- Pozwala użyć NuGet, aby zaktualizować do nowszej wersji jQuery bez wprowadzania zmian w powyższy kod tworzenia pakietów lub odwołania jQuery strony widoku.
- Automatycznie wybiera pełną wersję dla konfiguracji debugowania i wersji ".min" dla wersji kompilacji.

## <a name="using-a-cdn"></a>Korzystanie z sieci CDN

 Kod postępuj zgodnie z zastępuje pakiet lokalny jQuery pakiet jQuery sieci CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

W powyższym kodzie jQuery zostanie zażądany z sieci CDN, natomiast w wersji tryb i wersję debugowania jQuery będą pobierane lokalnie w trybie debugowania. Podczas korzystania z sieci CDN, powinny mieć mechanizm rezerwowy, w przypadku, gdy żądanie usługi CDN nie powiedzie się. Następujące znaczniki fragment od końca skryptu przedstawia plik układu dodane do żądania jQuery powinien kończyć się niepowodzeniem usługi CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Tworzenie pakietu

[Pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) klasy `Include` metoda przyjmuje tablicę ciągów, gdzie każdy ciąg jest ścieżką wirtualną do zasobu. Poniższy kod z `RegisterBundles` method in Class metoda *aplikacji\\\_Start\\BundleConfig.cs* plik pokazuje, jak wiele plików są dodawane do pakietu:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[Pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) klasy `IncludeDirectory` metoda znajduje się dodać wszystkie pliki w katalogu (i opcjonalnie wszystkie podkatalogi), które pasują do wzorca wyszukiwania. [Pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) klasy `IncludeDirectory` interfejsu API jest pokazany poniżej:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pakiety są określone w widokach przy użyciu metody renderowania (`Styles.Render` CSS i `Scripts.Render` dla języka JavaScript). Następujące znaczniki z *widoków\\Shared\\\_Layout.cshtml* plik pokazuje, jak domyślne widoki projektów internetowych ASP.NET odwoływać się do pakietów CSS i JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Zwróć uwagę, metody renderowania przyjmuje tablicę ciągów, dzięki czemu można dodać kilka pakietów w jednym wierszu kodu. Ogólnie można używać metod renderowania, które tworzą niezbędne HTML, aby odwoływać się do elementu zawartości. Możesz użyć `Url` metoda generuje adres URL do zasobu bez znaczników musiał odwoływać się do elementu zawartości. Załóżmy, że chcesz używać nowego języka HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atrybutu. Poniższy kod pokazuje, jak odwoływać się za pomocą modernizr `Url` metody.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Za pomocą "\*" wieloznaczny, aby wybrać pliki

Ścieżce wirtualnej określonej w `Include` metody i wyszukiwanie wzorca w `IncludeDirectory` metoda może obsługiwać jeden "\*" wieloznacznego jako prefiks lub sufiks w ostatnim segmencie ścieżki. Ciąg wyszukiwania jest uwzględniana wielkość liter. `IncludeDirectory` Metoda ma opcję wyszukiwania podkatalogów.

Za pomocą następujących plików JavaScript, należy wziąć pod uwagę projekt:

- *Skrypty\\typowe\\AddAltToImg.js*
- *Skrypty\\typowe\\ToggleDiv.js*
- *Skrypty\\typowe\\ToggleImg.js*
- *Skrypty\\typowe\\Sub1\\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

W poniższej tabeli przedstawiono pliki dodane do pakietu za pomocą symbolu wieloznacznego, jak pokazano:

| **Call** | **Pliki dodane lub zgłoszony wyjątek** |
| --- | --- |
| Obejmują ("~/Scripts/Common/\*js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Nieprawidłowy wzorzec wyjątek. Symbol wieloznaczny jest dozwolona tylko na prefiksu lub sufiksu. |
| Obejmują ("~/Scripts/Common/\*og.\*") | Nieprawidłowy wzorzec wyjątek. Dozwolone jest tylko jeden symbol wieloznaczny. |
| Obejmują ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Obejmują ("~/Scripts/Common/\*") | Nieprawidłowy wzorzec wyjątek. Segment czystego symbolu wieloznacznego nie jest prawidłowy. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Jawne Dodawanie każdego pliku do pakietu jest ogólnie metoda preferowana nad ładowania symboli wieloznacznych plików z następujących powodów:

- Dodawanie skryptów przez domyślne ustawienia symboli wieloznacznych, na potrzeby ładowania je w kolejności alfabetycznej, który jest zwykle nie chcemy. Pliki CSS i JavaScript często muszą zostać dodane w określonej kolejności (inne niż alfanumeryczne). Można zmniejszyć to zagrożenie, dodając niestandardową [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementacji, ale jawne dodanie każdego pliku jest mniej podatne. Na przykład można dodać nowe zasoby do folderu w przyszłości, które mogą wymagać zmodyfikowania swoje [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementacji.
- Wyświetl określone pliki dodane do katalogu przy użyciu ładowania symbol wieloznaczny mogą być dołączane we wszystkich widokach odwołuje się do tego pakietu. Jeśli określonego skryptu w widoku jest dodawany do pakietu, może wystąpić błąd JavaScript na inne widoki, które odwołują się pakietu.
- Importowanie innych plików, pliki CSS doprowadzić do importowanych plików, które są ładowane dwa razy. Na przykład poniższy kod tworzy pakiet z większością pliki CSS motyw interfejsu użytkownika jQuery ładowane dwa razy. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Selektor symbol wieloznaczny "\*CSS" łączy w każdym pliku CSS w folderze, w tym *zawartości\\motywy\\podstawowy\\jquery.ui.all.css* pliku. *Jquery.ui.all.css* plik importuje inne pliki CSS.

## <a name="bundle-caching"></a>Pakietu, buforowaniu

Pakiety ustawić nagłówek HTTP wygasa rok od po utworzeniu pakietu. Po przejściu na stronę poprzednio czytanego pokazuje Fiddler programu Internet Explorer nie powoduje warunkowego żądania dla pakietu, oznacza to, że istnieją żadne żądania HTTP GET z programu Internet Explorer dla pakiety i HTTP 304 odpowiedzi z serwera. Można wymusić programu Internet Explorer, aby utworzyć żądanie warunkowe dla każdego pakietu przy użyciu klawisza F5 (co w odpowiedzi HTTP 304 dla każdego pakietu). Możesz wymusić odświeżanie pełne, za pomocą ^ F5 (co w odpowiedzi HTTP 200 dla każdego pakietu).

Na poniższej ilustracji przedstawiono **Caching** karty okienku odpowiedzi programu Fiddler:

![Obraz pamięci podręcznej programu fiddler](bundling-and-minification/_static/image8.png)

Żądanie   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 dotyczy pakietu **AllMyScripts** i zawiera pary ciągu zapytania **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Ciąg zapytania **v** ma wartość tokenu oznacza to unikatowy identyfikator używany do buforowania. Tak długo, jak pakietu nie zmienia się, aplikacja ASP.NET będzie żądać **AllMyScripts** pakietu przy użyciu tego tokenu. Jeśli zmieni się dowolny plik w pakiecie, struktura optymalizacji ASP.NET spowoduje wygenerowanie nowego tokenu, gwarantując, że żądania przeglądarki dla pakietu otrzyma najnowsze pakietu.

Jeśli uruchomienie narzędzia programistyczne F12 programem IE9 lub nowszym, przejdź do strony wcześniej załadowanych IE niepoprawnie pokazuje warunkowego żądania GET do każdego pakietu i serwera, zwracając HTTP 304. Może odczytywać, dlaczego programem IE9 lub nowszym ma problemy z określania, czy żądanie warunkowego została wprowadzona w wpis w blogu [przy użyciu usługi CDN i wygasa, aby zwiększyć wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MNIEJ CoffeeScript, SCSS, Sass, tworzenie pakietów.

Tworzenie pakietów i minimalizowanie kryptograficznymi zapewnia mechanizm do przetworzenia pośrednich języków takich jak [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [mniej](http://www.dotlesscss.org/) lub [Coffeescript ](http://coffeescript.org/)i Zastosuj transformacje, takie jak minimalizację do pakietu wynikowego. Na przykład, aby dodać [.less](http://www.dotlesscss.org/) pliki do projektu MVC 4:

1. Utwórz folder dla mniej zawartości. W poniższym przykładzie użyto *zawartości\\MyLess* folderu.
2. Dodaj [.less](http://www.dotlesscss.org/) pakietu NuGet **bez kropek** do projektu.  
    ![Bez kropek install NuGet](bundling-and-minification/_static/image9.png)
3. Dodaj klasę, która implementuje [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interfejsu. Przekształcenia .less Dodaj następujący kod do projektu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Tworzenie pakietu mniej plików za pomocą `LessTransform` i [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) przekształcania. Dodaj następujący kod do `RegisterBundles` method in Class metoda *aplikacji\\_uruchom\\BundleConfig.cs* pliku.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Dodaj następujący kod, do których odwołuje się mniej pakietu widoków.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Zagadnienia dotyczące pakietu

Jest dobrą Konwencji, które należy wykonać podczas tworzenia pakietów do uwzględnienia "pakietu" jako prefiksu nazwy pakietu. Dzięki temu można zapobiec możliwe [routingu konflikt](https://forums.asp.net/post/5012037.aspx).

Po aktualizacji jeden plik w pakiecie nowy token jest generowany dla parametru ciągu zapytania pakietu, i pełnego pakietu muszą zostać pobrane przy następnym klient żąda strony zawierającej pakietu. W tradycyjnych znaczników, gdzie każdy element zawartości będzie widoczny indywidualnie będzie można pobrać tylko zmienionego pliku. Zasoby, które zmieniają się często, może nie być dobrymi kandydatami do tworzenia pakietów.

Tworzenie pakietów i minimalizowanie przede wszystkim poprawić strony żądań obciążenia po raz pierwszy. Gdy wysłano żądanie strony sieci Web, przeglądarka buforuje zasoby (JavaScript, CSS i obrazów), tworzenie pakietów i minimalizowanie niemożliwe jest wszystkie zwiększeniu wydajności podczas żądania tej samej stronie lub strony w tej samej lokacji, żądanie tych samych zasobów. Jeśli nie ustawisz wygasa nagłówek poprawnie na Twoje zasoby i nie używaj tworzenie pakietów i minimalizowanie, Algorytm heurystyczny świeżości przeglądarek spowoduje oznaczenie zasoby starych po upływie kilku dni i przeglądarka będzie wymagać żądanie weryfikacji dla każdego zasobu. W tym przypadku tworzenie pakietów i minimalizowanie zapewniają wzrost wydajności po pierwszym żądaniu strony. Aby uzyskać więcej informacji, zobacz w blogu [przy użyciu usługi CDN i wygasa, aby zwiększyć wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Ograniczenie przeglądarki sześć jednoczesnych połączeń dla każdej nazwy hosta, można zminimalizować przy użyciu [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Ponieważ usługa CDN będzie miał inną nazwę hosta niż hostingu witryny, żądania zawartości z usługi CDN nie wliczają sześć równoczesnych połączeń dozwolona środowisku macierzystym. Sieci CDN oferuje również typowe buforowania pakietów i urządzeniami brzegowymi, buforowanie korzyści.

Zestawy powinny być dzielone według stron, które ich potrzebują. Na przykład domyślnego szablonu platformy ASP.NET MVC dla aplikacji internetowych tworzy zbiór weryfikacji jQuery oddzielnie od jQuery. Widoki tworzone mają nie dane wejściowe, a nie publikowały wartości, dlatego nie obejmują one pakietu sprawdzania poprawności.

`System.Web.Optimization` Przestrzeni nazw jest zaimplementowana w *System.Web.Optimization.dll*. Jest przeprowadzana z zastosowaniem biblioteki WebGrease (*WebGrease.dll*) minimalizacji możliwości, który z kolei używa *Antlr3.Runtime.dll*.

*Używam Twitter utworzyć wpisy szybki i udostępnić łącza. Moje uchwyt Twitter jest*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Dodatkowe zasoby

- Wideo:[tworzenie pakietów i optymalizowanie](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) przez [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Dodawanie optymalizacji sieci Web do witryny sieci Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Dodawanie tworzenia pakietów i minimalizowanie do formularzy sieci Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Wpływ na wydajność tworzenia pakietów i minimalizowanie na przeglądanie sieci Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) przez [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Przy użyciu usługi CDN i wygasa, aby zwiększyć wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) autorstwa Ricka Andersona [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimalizuj RTT (czasami opóźnienia)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Współautorzy

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
