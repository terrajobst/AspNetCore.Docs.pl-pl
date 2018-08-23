---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: Adresy URL na stronach wzorcowych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Rozwiązuje, jak adresy URL na stronie głównej może przerwać ze względu na plik strony głównej, są w innym katalogu względne niż strony zawartość. Analizuje zmienianie bazy...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f22159c4e70beeb590039ea0d4b8126c5424bd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755072"
---
<a name="urls-in-master-pages-c"></a>Adresy URL na stronach wzorcowych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Rozwiązuje, jak adresy URL na stronie głównej może przerwać ze względu na plik strony głównej, są w innym katalogu względne niż strony zawartość. Patrzy na zmienianie bazy adresy URL za pośrednictwem ~ w składni deklaratywnej i programowo przy użyciu ResolveUrl i ResolveClientUrl. (Także zapoznać się


## <a name="introduction"></a>Wprowadzenie

We wszystkich przykładach widzieliśmy tej pory, strony głównej i zawartości znajdowała się w tym samym folderze (folder główny witryny sieci Web). Ale nie ma powodu, dlaczego strony wzorcowej i zawartości musi znajdować się w tym samym folderze. Bez obaw można utworzyć strony z zawartością w podfolderach. Podobnie, można utworzyć `~/MasterPages/` folderu, gdzie umieścić strony wzorcowe witryny.

Jeden potencjalny problem ze złożeniem strony wzorcowej i zawartości w różnych folderach obejmuje uszkodzone adresów URL. Jeśli strona główna zawiera względnych adresów URL hiperłącza, obrazy lub inne elementy, link będzie nieprawidłowa dla stron zawartości, które znajdują się w innym folderze. W tym samouczku omówiony źródła ten problem, a także rozwiązania problemu.

## <a name="the-problem-with-relative-urls"></a>Problem z względnych adresów URL

Adres URL, na stronie sieci web jest nazywany *względnym adresem URL* Jeśli lokalizację zasobu wskazuje jest określana względem lokalizacji strony sieci web w strukturze folderów witrynie sieci Web. Dowolny adres URL, który rozpoczyna się od wiodącego ukośnika (`/`) lub protokół (takie jak `http://`) jest względna, ponieważ został rozwiązany przez przeglądarkę, w oparciu o lokalizację strony sieci web, która zawiera adres URL.

Na przykład naszej witryny sieci Web ma `~/Images/` folderu z plikiem pojedynczy obraz `PoweredByASPNET.gif`. Plik strony głównej `Site.master` ma `<img>` element `footerContent` region następującym kodem:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

`src` Wartość atrybutu `<img>` element jest względny adres URL, ponieważ nie zaczyna się od `/` lub `http://`. Krótko mówiąc `src` informuje przeglądarkę do przeszukania, wartość atrybutu `Images` podfolder dla pliku o nazwie `PoweredByASPNET.gif`.

Podczas wyświetlania strony zawartości, powyżej znaczników są wysyłane bezpośrednio do przeglądarki. Poświęć chwilę, aby odwiedzić `About.aspx` i wyświetlić źródło HTML wysyłany do przeglądarki. Można zauważyć, że dokładnie ten sam kod znaczników na stronie głównej był wysyłany do przeglądarki.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Jeśli strona zawartości znajduje się w folderze głównym (ponieważ jest `About.aspx`) wszystko działa zgodnie z oczekiwaniami, ponieważ nie istnieje `Images` podfolder pokrewny folderu głównego. Jednak elementy podział Jeśli strona zawartości znajduje się w innym folderze niż strony wzorcowej. Na przykład utwórz podfolder o nazwie `Admin`. Następnie dodaj stronę zawartości o nazwie `Default.aspx` do `Admin` folderu, upewniając się powiązać nową stronę do `Site.master` strony wzorcowej.

> [!NOTE]
> W [ *Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) samouczek tworzenia niestandardowej strony podstawowej klasy o nazwie `BasePage` , automatycznie Ustaw tytuł strony zawartości (jeśli go nie została jawnie przypisana). Nie należy zapominać nowo utworzony strony osobna klasa kodu pochodzić od `BasePage` tak, aby go mogą korzystać z tej funkcji.


Po utworzeniu tej strony zawartości, Eksploratorze rozwiązań powinien wyglądać podobnie do rysunku 1.


![Nowy Folder i strona platformy ASP.NET zostały dodane do projektu](urls-in-master-pages-cs/_static/image1.png)

**Rysunek 01**: Nowy Folder i strona platformy ASP.NET zostały dodane do projektu


Następnie zaktualizuj `Web.sitemap` pliku, aby uwzględnić nowe `<siteMapNode>` wpis dla tej lekcji. Następujący kody XML pokazuje pełną `Web.sitemap` znaczników, który teraz zawiera dodaniu trzeciego `<siteMapNode>` elementu.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Nowo utworzony `Default.aspx` strona powinna mieć cztery formanty zawartości odpowiadający cztery kontrolek ContentPlaceHolder w `Site.master`. Dodaj jakiś tekst do kontrolki zawartości odwołujące się do `MainContent` ContentPlaceHolder, a następnie odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, przeglądarka nie może odnaleźć `PoweredByASPNET.gif` pliku obrazu. Co się dzieje w tym miejscu?

`~/Admin/Default.aspx` Strony zawartości są wysyłane na ten sam kod HTML `footerContent` region, ponieważ był `About.aspx` strony:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Ponieważ `<img>` elementu `src` atrybut jest względny adres URL, przeglądarka prób do wyszukania `Images` folderu względem lokalizacji folderu strony sieci web. Innymi słowy, przeglądarka szuka pliku obrazu `Admin/Images/PoweredByASPNET.gif`.


[![Nie można odnaleźć pliku obrazu PoweredByASPNET.gif](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Rysunek 02**: `PoweredByASPNET.gif` obraz nie można odnaleźć pliku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Zastępowanie względnych adresów URL przy użyciu bezwzględnych adresów URL

Jest to odwrotność względnym adresem URL *bezwzględny adres URL*, który jest taki, który rozpoczyna się od ukośnika (`/`) lub protokołu, takie jak `http://`. Ponieważ bezwzględny adres URL określa lokalizację zasobu ze znanego punktu stały, tym samym bezwzględny adres URL jest prawidłowy w dowolnej strony sieci web, niezależnie od lokalizacji strony sieci web w strukturze folderów witrynie sieci Web.

Aby rozwiązać problem, uszkodzony obraz pokazano na rysunku 2, musimy zaktualizować `<img>` elementu `src` atrybutu, tak aby używał bezwzględnego adresu URL zamiast względnych. Aby określić prawidłowy bezwzględny adres URL, odwiedź jedną ze stron sieci web w witrynie sieci Web i zbadać na pasku adresu. Jak pokazano na pasku adresu na rysunku 2, to w pełni kwalifikowana ścieżka do aplikacji sieci web `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. W związku z tym, firma Microsoft może aktualizować `<img>` elementu `src` atrybutu do jednej z następujących dwóch bezwzględne adresy URL:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Poświęć chwilę, aby zaktualizować `<img>` elementu `src` atrybutu bezwzględny adres URL przy użyciu jednej formy pokazanych powyżej, a następnie odwiedź `~/Admin/Default.aspx` strony za pośrednictwem przeglądarki. Tym razem przeglądarki zostaną prawidłowo znaleźć i wyświetlić `PoweredByASPNET.gif` pliku obrazu (zobacz rysunek 3).


[![Obraz PoweredByASPNET.gif jest teraz wyświetlana](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Rysunek 03**: `PoweredByASPNET.gif` obraz jest teraz wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](urls-in-master-pages-cs/_static/image7.png))


Natomiast twardych kodowanie przy użyciu programu bezwzględny adres URL działa ściśle couples HTML do serwera witryny sieci Web i lokalizacji folderu, która może ulec zmianie. Za pomocą bezwzględny adres URL formularza `http://localhost:3908/...` jest elementem wrażliwym ponieważ poprzedni numer portu `localhost` jest automatycznie wybierany każdorazowo programu Visual Studio wbudowanych ASP.NET serwera wdrożeniowego sieci Web jest uruchomiona. Podobnie `http://localhost` część jest prawidłowy tylko podczas testowania lokalnie. Gdy kod jest wdrażany na serwerze produkcyjnym, podstawowy adres URL zmieni się czymś innym, takie jak `http://www.yourserver.com`. Bezwzględny adres URL w postaci `/ASPNET_MasterPages_Tutorial_04_CS/...` również odczuwa kruchości na tym samym, ponieważ często tej ścieżki aplikacji różni się między serwerami deweloperskim i produkcyjnym.

Dobra wiadomość jest, że program ASP.NET zapewnia metody do generowania prawidłowym względnym adresem URL w czasie wykonywania.

## <a name="usingandresolveclienturl"></a>Za pomocą`~`i`ResolveClientUrl`

Raczej niż ciężko kod bezwzględny adres URL, ASP.NET umożliwia strony użycie tyldy (`~`) do wskazania katalogu głównym aplikacji sieci web. Na przykład wcześniej w tym samouczku używany notacji `~/Admin/Default.aspx` w tekście do odwoływania się do `Default.aspx` stronie `Admin` folderu. `~` Wskazuje, że `Admin` folderu jest podfolder katalog główny aplikacji sieci web.

`Control` Klasy [ `ResolveClientUrl` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) przyjmuje adres URL i modyfikuje je do odpowiednich dla strony sieci web, na którym znajduje się kontrolka względnym adresem URL. Na przykład, wywołanie `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` z `About.aspx` zwraca `Images/PoweredByASPNET.gif`. Podczas wywoływania go z `~/Admin/Default.aspx`, jednak zwraca `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Ponieważ dziedziczyć wszystkie formanty serwera ASP.NET `Control` klasy, wszystkie formanty serwera mają dostęp do `ResolveClientUrl` metody. Nawet `Page` klasa pochodzi od `Control` klasy, co oznacza, że można użyć tej metody, bezpośrednio z klasy CodeBehind strony ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Za pomocą`~`w oznaczeniu deklaracyjnym

Kilka formantów ASP.NET sieci Web obejmują właściwości dotyczące adresu URL: kontrolkę Hiperlinku `NavigateUrl` właściwości; obraz kontrolkę `ImageUrl` właściwości; i tak dalej. Po zakończeniu renderowania tych kontrolek przekazania wartości właściwości związane z adresu URL, aby `ResolveClientUrl`. W związku z tym jeśli zawierają te właściwości `~` wskazujący katalog główny aplikacji sieci web, adres URL zostaną zmodyfikowane i prawidłowym względnym adresem URL.

Należy pamiętać o tym, tylko formanty serwera ASP.NET przekształcający `~` w ich właściwości związane z adresu URL. Jeśli `~` pojawia się w statycznych kod znaczników HTML, takich jak `<img src="~/Images/PoweredByASPNET.gif" />`, wysyła aparatu ASP.NET `~` do przeglądarki i resztę zawartość HTML. Przeglądarka założono, że `~` jest częścią adresu URL. Na przykład, jeśli przeglądarka odbiera znaczników `<img src="~/Images/PoweredByASPNET.gif" />` przyjęto założenie, że istnieje podfolder o nazwie `~` podfolder `Images` zawierający plik obrazu `PoweredByASPNET.gif`.

Aby rozwiązać problem z kodu znaczników obrazu w `Site.master`, Zastąp istniejące `<img>` elementu z kontrolką obrazu platformy ASP.NET w sieci Web. Ustawianie kontroli obrazu w sieci Web `ID` do `PoweredByImage`, jego `ImageUrl` właściwości `~/Images/PoweredByASPNET.gif`i jego `AlternateText` właściwość "Powered by ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Po wprowadzeniu tej zmiany strony wzorcowej, ponownie `~/Admin/Default.aspx` strony. Tym razem `PoweredByASPNET.gif` plik obrazu, który pojawia się na stronie (zobacz rysunek 3). Gdy obraz sieci Web, formant jest czyniło je wykorzystuje `ResolveClientUrl` metodę, aby rozpoznać jego `ImageUrl` wartości właściwości. W `~/Admin/Default.aspx` `ImageUrl` jest konwertowana na odpowiednie względny adres URL, jak poniższy fragment przedstawia źródła HTML:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Oprócz używany we właściwościach formantu opartego na adresach URL sieci Web, `~` może również służyć podczas wywoływania `Response.Redirect` i `Server.MapPath` metod, między innymi. Ponadto `ResolveClientUrl` metody mogą być wywoływane bezpośrednio z programu ASP.NET lub oznaczeniu deklaracyjnym strony wzorcowej, jeśli to konieczne; zobacz [przenikanie Fritz](https://www.pluralsight.com/blogs/fritz/)firmy wpis w blogu [Using `ResolveClientUrl` w znaczniku](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Naprawianie strony wzorcowej pozostałe względnych adresów URL

Oprócz `<img>` element `footerContent` czy po prostu naprawiliśmy, strona główna zawiera jeden bardziej względny adres URL, który wymaga naszej uwagi. `topContent` Region zawiera łącze "Master strony samouczki," wskazujący `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Ponieważ ten adres URL jest względny, wyśle użytkownikowi `Default.aspx` strony w folderze odwiedzają stronę zawartość. Do tego linku, zawsze wskazywać `Default.aspx` w folderze głównym, należy zastąpić `<a>` element z sieci Web hiperłącze sterowania, co możemy użyć `~` notacji.

Usuń `<a>` znaczników i Dodaj kontrolkę Hiperlinku w tym miejscu. Ustaw hiperłącze `ID` do `lnkHome`, jego `NavigateUrl` właściwości `~/Default.aspx`i jego `Text` właściwość "Samouczki stron wzorca".


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

To wszystko! W tym momencie wszystkie adresy URL z naszej strony wzorcowej prawidłowo opierają się podczas renderowania przez strony zawartości, niezależnie od tego, jakie foldery stronę wzorcową i stronę zawartości znajdują się w.

### <a name="automatic-url-resolution-in-theheadsection"></a>Rozpoznawanie adresu URL automatycznych w`<head>`sekcji

W [ *tworzenia całej witryny układu za pomocą stron wzorcowych* ](creating-a-site-wide-layout-using-master-pages-cs.md) samouczek dodaliśmy `<link>` do `Styles.css` w pliku `<head>` regionu:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Gdy `<link>` elementu `href` atrybut jest względna, jest automatycznie konwertowany na odpowiednią ścieżkę w czasie wykonywania. Tak jak Omówiliśmy to w [ *Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) samouczku `<head>` region jest faktycznie kontrolkę po stronie serwera i umożliwia modyfikowanie zawartość jego wewnętrzny formantów, gdy jest on renderowany.

Aby to sprawdzić, ponownie `~/Admin/Default.aspx` strony i wyświetlić źródło HTML wysyłany do przeglądarki. Tak jak pokazano w poniższym fragmencie kodu, `<link>` elementu `href` atrybut została automatycznie zmodyfikowana, aby odpowiednie względny adres URL, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Podsumowanie

Strony wzorcowe bardzo często zawierają linków, obrazów i innych zasobów zewnętrznych, które musi być określona za pomocą adresu URL. Ponieważ strony wzorcowej oraz strony z zawartością nie istnieje w tym samym folderze, koniecznie angażuje się przy użyciu względnych adresów URL. Chociaż jest możliwe użycie zakodowany bezwzględne adresy URL, więc ściśle wykonując couples bezwzględny adres URL do aplikacji sieci web. Bezwzględny adres URL zmienia - jak często podczas przenoszenia lub wdrażanie aplikacji sieci web — musisz Pamiętaj, aby wrócić i zaktualizować bezwzględne adresy URL.

Idealnym rozwiązaniem jest użycie tyldy (`~`) do wskazania katalogu głównego aplikacji. Mapy formantów sieci Web platformy ASP.NET, które zawierają właściwości powiązanych z adresu URL `~` do katalogu głównego aplikacji w czasie wykonywania. Wewnętrznie, użyj formantów sieci Web `Control` klasy `ResolveClientUrl` metodę w celu wygenerowania prawidłowym względnym adresem URL. Ta metoda jest publiczne i dostępne z każdego formantu serwera (w tym `Page` klasy), aby można było używać go programowo z klas związanym z kodem, jeśli to konieczne.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Strony wzorcowe na platformie ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Adres URL zmienianie bazy w stronę wzorcową](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Za pomocą `ResolveClientUrl` w znacznikach](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [dalej](control-id-naming-in-content-pages-cs.md)
