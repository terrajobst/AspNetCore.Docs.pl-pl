---
uid: mvc/overview/performance/bundling-and-minification
title: Tworzenie pakietów i minimalizowanie | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Tworzenie pakietów i minimalizowanie są dwie metody można użyć w programie ASP.NET 4.5 do zwiększenia czas ładowania żądania. Tworzenie pakietów i minimalizowanie poprawia czas ładowania przez reducin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Tworzenie pakietów i minimalizowanie są dwie metody można użyć w programie ASP.NET 4.5 do zwiększenia czas ładowania żądania. Tworzenie pakietów i minimalizowanie poprawia czas ładowania przez zmniejszenie liczby żądań do serwera oraz redukcję rozmiaru żądanych zasobów (takich jak CSS i JavaScript.)


Większość bieżącego główne przeglądarki ograniczyć liczbę [równoczesnych połączeń](http://www.browserscope.org/?category=network) dla każdej nazwy hosta do 6. Oznacza to, że podczas przetwarzania żądania sześciu dodatkowych żądań do zasobów na hoście zostaną umieszczone w kolejce przez przeglądarkę. Na poniższej ilustracji kartach sieci narzędzia developer F12 programu Internet Explorer zawiera chronometraż zasoby wymagane przez widok informacje przykładowej aplikacji.

![B/M](bundling-and-minification/_static/image1.png)

Szare paski Pokaż żądanie jest przechowywane w kolejce w przeglądarce oczekiwanie na granicy sześciu połączenia czas. Czas żądania do pierwszego bajtu jest żółty pasek oznacza to, że czas potrzebny do wysyłania żądania i odbierania pierwszej odpowiedzi z serwera. Niebieski paski Pokaż czas potrzebny na odebranie dane odpowiedzi z serwera. Możesz kliknąć dwukrotnie na zasób można uzyskać informacji o chronometrażu. Na przykład na poniższej ilustracji przedstawiono szczegółów chronometrażu dla ładowania */Scripts/MyScripts/JavaScript6.js* pliku.

![](bundling-and-minification/_static/image2.png)

W poprzednim przedstawiono obrazu **Start** zdarzenia, które powoduje czasu żądania zostało umieszczone w kolejce z powodu przeglądarki ograniczyć liczbę równoczesnych połączeń. W takim przypadku żądanie zostało umieszczone w kolejce przez 46 milisekund oczekiwania na zakończenie innego żądania.

## <a name="bundling"></a>Tworzenie pakietów

Tworzenie pakietów jest nową funkcją w programie ASP.NET 4.5, który ułatwia łączenie lub wielu plików pakietu w jednym pliku. Można utworzyć CSS, JavaScript i inne pakiety. Mniej plików oznacza, że mniej żądania HTTP, może poprawić wydajność obciążenia w usłudze pierwszej strony.

Na poniższej ilustracji przedstawiono tego samego widoku chronometrażu widoku informacje pokazano wcześniej, ale tym razem z tworzenie pakietów i minimalizowanie włączone.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimalizowanie

Minimalizowanie wykonuje różne optymalizacje inny kod do skryptów lub css, takie jak usunięcie niepotrzebnych biały znak i komentarze i skrócić nazwy zmiennych o jeden znak. Należy wziąć pod uwagę następujące funkcji JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Po minimalizowanie funkcji, zostanie zmniejszona do następującego:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Oprócz usuwanie komentarzy i niepotrzebne odstępu, następujących parametrów i nazwy zmiennych zmieniono (skrócony) w następujący sposób:

| **Original** | **Zmieniono jego nazwę** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | mogę |

## <a name="impact-of-bundling-and-minification"></a>Wpływ na tworzenie pakietów i minimalizowanie

W poniższej tabeli przedstawiono kilka istotnych różnic między listę wszystkich zasobów pojedynczo i tworzenie pakietów i minimalizowanie (B/M) w programie próbki.

|  | **Przy użyciu B/M** | **Bez B/M** | **Change** |
| --- | --- | --- | --- |
| **Żądań plików** | 9 | 34 | 256% |
| **KB Sent** | 3.26 | 11.92 | 266% |
| **Odebrano KB** | 388.51 | 530 | 36% |
| **Czas ładowania** | 510 MS | 780 MS | 53% |

Wysłane bajty miał znaczne obniżenie zużycia z udane przeglądarki są dość verbose z nagłówków HTTP, które mają one zastosowanie do żądań. Redukcja odebranych bajtów nie jest tak duży, ponieważ plików największy (*Scripts\jquery-ui-1.8.11.min.js* i *Scripts\jquery-1.7.1.min.js*) są już zminimalizowany. Uwaga: Chronometraż przykładowy program używany [Fiddler](http://www.fiddler2.com/fiddler2/) narzędzia, aby symulować wolnej sieci. (Z Fiddler **reguły** menu, wybierz opcję **wydajności** następnie **symulować szybkości modemu**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debugowanie powiązane i zminimalizowane JavaScript

Łatwo można debugować skrypt JavaScript w środowisku projektowym (gdzie [compilation Element](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* plik ma ustawioną wartość `debug="true"` ), ponieważ nie są powiązane pliki JavaScript lub zminimalizowany. Można również debugowanie kompilacji wydania którym powiązane pliki JavaScript i zminimalizowany. Korzystając z narzędzi deweloperskich programu Internet Explorer F12, debugowania funkcji JavaScript, zawartych w pakiecie zminimalizowany, w następujący sposób:

1. Wybierz **skryptu** karcie, a następnie wybierz **Rozpocznij debugowanie** przycisku.
2. Wybierz pakiet zawierający funkcji JavaScript, do której chcesz debugować przy użyciu przycisku zasoby.  
    ![](bundling-and-minification/_static/image4.png)
3. Format zminimalizowany JavaScript, wybierając **przycisk Konfiguracja** ![](bundling-and-minification/_static/image5.png), a następnie wybierając **formacie JavaScript**.
4. W **skrypt służy wyszukiwania** t pole wejściowe, wybierz nazwę funkcji chcesz debugować. Na poniższej ilustracji **AddAltToImg** została wprowadzona w **skrypt służy wyszukiwania** pole wprowadzania t.  
    ![](bundling-and-minification/_static/image6.png)

Aby uzyskać więcej informacji dotyczących debugowania za pomocą narzędzi deweloperskich F12, zobacz artykuł w witrynie MSDN [korzystania z narzędzi deweloperskich F12, aby debugować JavaScript — błędy](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Kontrolowanie tworzenie pakietów i minimalizowanie

Tworzenie pakietów i minimalizowanie włączyć lub wyłączyć, ustawiając wartość atrybutu debugowania w [compilation Element](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* pliku. W poniższych XML `debug` jest ustawiona na wartość true, dlatego tworzenie pakietów i minimalizowanie jest wyłączona.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Aby włączyć tworzenie pakietów i minimalizowanie, ustaw `debug` wartość "false". Można zastąpić *Web.config* ustawienie `EnableOptimizations` właściwość `BundleTable` klasy. Poniższy kod umożliwia tworzenie pakietów i minimalizowanie i zastępuje wszystkie ustawienia w *Web.config* pliku.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> O ile `EnableOptimizations` jest `true` lub atrybutu debugowania w [compilation Element](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* plik ma ustawioną wartość `false`, pliki nie zostaną powiązane lub zminimalizowany. Ponadto wersji .min plików nie będą używane, zostanie wybrany debugowania pełnej wersji. `EnableOptimizations` zastępuje atrybut debugowania w [compilation Element](https://msdn.microsoft.com/library/s10awwz0.aspx) w *Web.config* pliku


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Przy użyciu tworzenie pakietów i minimalizowanie z formularzami sieci Web ASP.NET i stron sieci Web

- Dla stron sieci Web, zobacz wpis w blogu [Dodawanie optymalizacji sieci Web do witryny sieci Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- W przypadku formularzy sieci Web, zobacz wpis w blogu [Dodawanie tworzenie pakietów i minimalizowanie do formularzy sieci Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Przy użyciu tworzenie pakietów i minimalizowanie z platformą ASP.NET MVC

W tej sekcji utworzymy ASP.NET MVC projektu, aby sprawdzić, tworzenie pakietów i minimalizowanie. Najpierw utwórz nowy projekt ASP.NET MVC internet o nazwie **MvcBM** bez zmiana któregoś z ustawień domyślnych.

Otwórz *aplikacji\_Start\BundleConfig.cs* pliku i sprawdź, czy `RegisterBundles` metodę, która służy do tworzenia, Zarejestruj i skonfiguruj pakietów. Poniższy kod przedstawia część `RegisterBundles` metody.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Poprzedni kod tworzy nowy pakiet JavaScript o nazwie *~/bundles/jquery* zawierającą wszystkie odpowiednie (które debugowania lub zminimalizowane, ale nie. *vsdoc*) plików *skryptów* folderu, który jest zgodny z ciągiem symbol wieloznaczny "{{wersja} ~/Scripts/jquery-.js". Dla programu ASP.NET MVC 4, oznacza to, z konfiguracji debugowania, plik *jquery 1.7.1.js* zostaną dodane do pakietu. W konfiguracji wydania *jquery 1.7.1.min.js* zostanie dodany. Wiązanie framework zgodna z konwencjami kilka typowych takich jak:

- Wybierając plik ".min" w wersji, gdy istnieją "FileX.min.js" i "FileX.js".
- Wybierz wersję z systemem innym niż ".min" do debugowania.
- Ignorowanie "-vsdoc" (np. jquery-1.7.1-vsdoc.js), plików, które są używane tylko przez funkcję IntelliSense.

`{version}` Wieloznaczny dopasowania pokazanym powyżej jest używane do automatycznego tworzenia pakiet jQuery z odpowiednią wersją jQuery w Twojej *skryptów* folderu. W tym przykładzie przy użyciu symbolu wieloznacznego zapewnia następujące korzyści:

- Umożliwia użycie narzędzia NuGet można zaktualizować do nowszej wersji jQuery bez zmieniania poprzedni kod wiązanie lub jQuery odwołuje się do strony widoku.
- Automatycznie wybiera pełną wersję dla konfiguracji debugowania i wersji ".min" w wersji kompilacji.

## <a name="using-a-cdn"></a>Korzystanie z nazwy CDN

 Kod wykonaj zastępuje pakiet jQuery lokalnego pakiet jQuery sieci CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

W powyższym kodzie jQuery będzie wymagane z sieci CDN, podczas gdy w wersji trybu i wersja do debugowania z jQuery będą pobierane lokalnie w trybie debugowania. Podczas korzystania z usługi CDN, musisz mieć mechanizm rezerwowy w przypadku nieudanego żądania CDN. Fragmentu następujący kod na końcu skryptu pokazuje pliku układu dodane do żądania jQuery powinien kończyć się niepowodzeniem CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Tworzenie pakietu

[Pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) klasy `Include` — metoda pobiera tablicę ciągów, których każdy ciąg jest ścieżką wirtualną do zasobu. Następujący kod w metodzie RegisterBundles w *aplikacji\_Start\BundleConfig.cs* pliku pokazuje, jak pliki zostaną dodane do pakietu:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[Pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) klasy `IncludeDirectory` — metoda jest dostępne, aby dodać wszystkie pliki w katalogu (i opcjonalnie wszystkie podkatalogi), które pasują do wzorca wyszukiwania. [Pakietu](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) klasy `IncludeDirectory` interfejsu API jest pokazany poniżej:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pakiety są przywoływane w widokach przy użyciu metody renderowania ( `Styles.Render` CSS i `Scripts.Render` JavaScript). Następujący kod z *Views\Shared\\_Layout.cshtml* pliku pokazuje, jak widoki projektu ASP.NET, internetowe odwołania pakiety CSS i JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Zwróć uwagę, metody renderowania pobiera tablicę ciągów, dzięki czemu można dodać kilka pakietów w jednym wierszu kodu. Zazwyczaj można użyć metody renderowania, które utworzyć niezbędne HTML do odwołania elementu zawartości. Można użyć `Url` do generowania adresu URL do elementu zawartości bez znaczników potrzebne do odwołania elementu zawartości. Załóżmy, że chcesz używać nowego HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atrybutu. Poniższy kod przedstawia sposób odwoływać się przy użyciu modernizr `Url` metody.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Za pomocą "\*" wieloznaczny, aby wybrać pliki

Ścieżki wirtualnej określona w `Include` — metoda i wyszukiwanie wzorca w `IncludeDirectory` metody można zaakceptować jedną "\*" wieloznacznego jako prefiksu lub sufiksu do ostatni segment ścieżki. Ciąg wyszukiwania jest uwzględniana wielkość liter. `IncludeDirectory` Metoda ma możliwość wyszukiwania podkatalogów.

Rozważ następujące pliki JavaScript projektu:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

W poniższej tabeli przedstawiono pliki dodane do pakietu przy użyciu symbolu wieloznacznego, jak pokazano:

| **Wywołania** | **Pliki dodane lub wystąpił wyjątek** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Nieprawidłowy wzorzec wyjątek. Symbol wieloznaczny jest dozwolona tylko w prefiksu lub sufiksu. |
| Obejmują ("~/Scripts/Common/\*og.\*") | Nieprawidłowy wzorzec wyjątek. Dozwolone jest tylko jeden symbol wieloznaczny. |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | Nieprawidłowy wzorzec wyjątek. Segment wieloznaczny czysty jest nieprawidłowy. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Jawne Dodawanie każdego pliku do pakietu jest zwykle preferowany nad ładowania symboli wieloznacznych plików z następujących powodów:

- Dodawanie skryptów przez domyślne symboli wieloznacznych do ładowania je w porządku alfabetycznym, która zazwyczaj nie chcesz. Pliki CSS i JavaScript często muszą zostać dodane, w określonej kolejności (inne niż alfanumeryczne). Można zmniejszyć to zagrożenie, dodając niestandardowe [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementacji, ale jawnie Dodawanie każdego pliku jest mniej podatne błędu. Na przykład można dodać nowe zasoby do folderu w przyszłości, które mogą wymagać zmodyfikowania Twojej [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementacji.
- Wyświetlanie określonych plików dodawane do katalogu przy użyciu ładowania symbol wieloznaczny może obejmować we wszystkich widokach odwołuje się do tego pakietu. Jeśli określonego skryptu w widoku jest dodawany do pakietu, może wystąpić błąd kodu JavaScript w innych widoków, które odwołują się do pakietu.
- Pliki CSS, które importują innych plików powoduje w importowanych plikach załadować dwa razy. Na przykład poniższy kod tworzy pakiet z większość plików CSS motywu interfejsu użytkownika jQuery załadować dwa razy. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Selektor symbol wieloznaczny "\*.css" w przypadku każdego pliku CSS w folderze, w tym *Content\themes\base\jquery.ui.all.css* pliku. *Jquery.ui.all.css* pliku Importuje inne pliki CSS.

## <a name="bundle-caching"></a>Pakietu buforowanie

Pakiety wartość nagłówka HTTP wygasa jednego roku z po utworzeniu pakietu. Jeśli przejdź na stronę poprzednio czytanego pokazuje Fiddler IE nie powoduje warunkowego żądania dla pakietu, który nie istnieją żadne żądania HTTP GET z programu Internet Explorer dla pakiety i odpowiedzi HTTP 304 z serwera. Można wymusić programu Internet Explorer, aby utworzyć żądanie warunkowego dla każdego pakietu z kluczem F5 (co spowoduje odpowiedź HTTP 304 dla każdego pakietu). Możesz wymusić pełne odświeżenie przy użyciu ^ F5 (co spowoduje odpowiedź 200 protokołu HTTP, dla każdego pakietu).

Poniższy obraz przedstawia **buforowanie** Fiddler okienka odpowiedzi:

![Obraz buforowania fiddler](bundling-and-minification/_static/image8.png)

Żądanie   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 jest przeznaczony dla pakietu **AllMyScripts** i zawiera pary ciągu zapytania **v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Ciąg zapytania **v** ma wartość, czyli token Unikatowy identyfikator używany do buforowania. Tak długo, jak pakietu nie zmienia, aplikacja ASP.NET będzie żądać **AllMyScripts** pakietu przy użyciu tego tokenu. W przypadku zmiany dowolnego pliku w pakiecie, struktura optymalizacji ASP.NET wygeneruje nowy token, gwarantując, że żądania przeglądarki dla pakietu pobierze najnowszą pakietu.

Uruchamianie narzędzi deweloperskich IE9 F12, przejdź do strony wcześniej załadowany IE niepoprawnie zawiera warunkowego żądania GET wysyłane do każdego pakietu i serwer zwracanie HTTP 304. Możesz przeczytać, dlaczego IE9 ma problemy z określenie, czy żądania warunkowego została wprowadzona w wpis w blogu [CDN przy użyciu i wygasa, aby poprawić wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MNIEJ CoffeeScript, SCSS, Sass, tworzenie pakietów.

Tworzenie pakietów i minimalizowanie framework zapewnia mechanizm do przetwarzania pośredniego języków, takich jak [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [mniej](http://www.dotlesscss.org/) lub [Coffeescript ](http://coffeescript.org/)i dotyczą transformacje, takich jak minimalizację wynikowy pakietu. Na przykład, aby dodać [.less](http://www.dotlesscss.org/) plików do projektu MVC 4:

1. Utwórz folder mniej zawartości. W poniższym przykładzie użyto *Content\MyLess* folderu.
2. Dodaj [.less](http://www.dotlesscss.org/) pakietu NuGet **bez kropki** do projektu.  
    ![Bez kropki install NuGet](bundling-and-minification/_static/image9.png)
3. Dodaj klasę, która implementuje [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interfejsu. Przekształcenia .less Dodaj następujący kod do projektu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Utwórz pakiet zawierający mniej plików z `LessTransform` i [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformacji. Dodaj następujący kod do `RegisterBundles` metody w *aplikacji\_Start\BundleConfig.cs* pliku.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Dodaj następujący kod, do których odwołuje się do mniej pakietu widoków.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Zagadnienia dotyczące pakietu

Dotyczącej podczas tworzenia pakietów ma obejmować "pakietu" jako prefiksu nazwy pakietu. Uniemożliwi to potencjalnie [routingu konflikt](https://forums.asp.net/post/5012037.aspx).

Po zaktualizowaniu jeden plik w pakiecie nowy token jest generowane dla parametru ciągu zapytania pakietu i pełny pakiet musi zostać pobrana przy następnym klient zażąda strony zawierającej pakietu. W znaczniku tradycyjnych, gdy poszczególnych trwałych znajduje się indywidualnie czy pobrane tylko zmienionych plików. Zasoby, które zmieniają się często może nie być odpowiednimi obiektami do grupowania.

Tworzenie pakietów i minimalizowanie głównie zwiększyć czas ładowania pierwsze żądanie strony. Po zażądaniu strony sieci Web przeglądarce buforuje zasoby (JavaScript, CSS i obrazy), tworzenie pakietów i minimalizowanie nie będzie zapewniać żadnych wzrost wydajności podczas żądania strony lub strony w tej samej lokacji, żąda tego samego zasoby. Jeśli nie ustawisz nagłówek poprawnie na zasobów, wygaśnięcia i nie używaj tworzenie pakietów i minimalizowanie, heurystyki świeżości przeglądarki oznaczy zasoby starych po upływie kilku dni i przeglądarki będzie wymagać żądanie sprawdzania poprawności dla każdego zasobu. W takim przypadku tworzenie pakietów i minimalizowanie zapewnić wzrost wydajności po pierwszym żądaniu strony. Aby uzyskać więcej informacji, zobacz blog [CDN przy użyciu i wygasa, aby poprawić wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Ograniczenia przeglądarki sześciu równoczesnych połączeń dla każdej nazwy hosta można zminimalizować przy użyciu [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Ponieważ sieć CDN będzie mieć inną nazwę hosta niż hostingu witryny, żądania zasobów z sieci CDN nie odliczona sześciu limit równoczesnych połączeń środowisku macierzystym. CDN oferuje również wspólnej buforowanie pakietu i buforowanie zalety krawędzi.

Powinien być dzielony na partycje pakietów przez strony, które tego wymagają. Na przykład domyślnego szablonu platformy ASP.NET MVC dla aplikacji internetowej tworzy pakiet weryfikacji jQuery oddzielony od jQuery. Ponieważ widoki tworzone nie dane wejściowe i nie po wartości, nie obejmują one pakietu sprawdzania poprawności.

`System.Web.Optimization` Przestrzeni nazw jest zaimplementowana w System.Web.Optimization.DLL. Jest przeprowadzana z zastosowaniem biblioteki WebGrease (WebGrease.dll) minimalizacji możliwości, który z kolei używa Antlr3.Runtime.dll.

*Używam Twitter wpisów szybki i udostępniać łącza. Dojście do mojego Twitter*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Dodatkowe zasoby

- Wideo:[tworzenie pakietów i optymalizowanie](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) przez [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Dodawanie optymalizacji sieci Web do witryny sieci Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Dodawanie tworzenie pakietów i minimalizowanie do formularzy sieci Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Wpływ na wydajność programu tworzenie pakietów i minimalizowanie na przeglądanie sieci Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) przez [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Przy użyciu CDN i wygasa, aby zwiększyć wydajność witryny sieci Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) autorstwa Ricka Andersona [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimalizowanie RTT (czasami opóźnienia)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Współautorzy

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
