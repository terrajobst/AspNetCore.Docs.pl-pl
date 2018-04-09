---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: Adresy URL w stron wzorcowych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Rozwiązuje, jak adresy URL na stronie głównej mogą być dzielone ze względu na plik strony głównej w katalogu względną innego niż ze strony zawartość. Analizuje zmienianie bazy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e1d4b2d66bedfb5f3d7d8c61265944a82605e77e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="urls-in-master-pages-vb"></a>Adresy URL w stron wzorcowych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Rozwiązuje, jak adresy URL na stronie głównej mogą być dzielone ze względu na plik strony głównej w katalogu względną innego niż ze strony zawartość. Analizuje zmienianie bazy adresów URL za pośrednictwem ~ w składni deklaratywnej i programowo przy użyciu ResolveUrl i ResolveClientUrl. (Też przyjrzeć


## <a name="introduction"></a>Wprowadzenie

We wszystkich przykładach możemy przedstawiono pory, strony głównej i zawartości znajdowała się w tym samym folderze (folder główny witryny sieci Web). Ale nie ma powodu Dlaczego strony wzorcowej i zawartości musi być w tym samym folderze. Oczywiście można utworzyć strony z zawartością w podfolderach. Podobnie można utworzyć `~/MasterPages/` folder, gdzie umieścić stron wzorcowych witryny.

Jeden potencjalny problem z umieszczenie strony wzorcowej i zawartości w innych folderach obejmuje przerwane adresów URL. Jeśli strony wzorcowej zawiera względnych adresów URL w hiperłącza, obrazy i innych elementów, link będzie nieprawidłowe dla stron zawartości, które znajdują się w innym folderze. Źródło tego problemu, a także rozwiązania omówione w tym samouczku.

## <a name="the-problem-with-relative-urls"></a>Problem z względnych adresów URL

Adres URL na stronie sieci web jest określany jako *względny adres URL* w przypadku lokalizacji zasobów wskazuje lokalizacji strony sieci web w strukturze folderu witryny sieci Web. Każdy adres URL, który nie rozpoczyna się od wiodącego ukośnika (`/`) lub protokołu (takie jak `http://`) jest względna, ponieważ został rozwiązany przez przeglądarkę na podstawie lokalizacji strony sieci web, który zawiera adres URL.

Na przykład naszą witrynę sieci Web ma `~/Images/` folder z jednym plikiem obrazu, `PoweredByASPNET.gif`. Plik strony głównej `Site.master` ma `<img>` element `footerContent` region następujący kod:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

`src` Wartość w atrybutu `<img>` element jest względnym adresem URL, ponieważ nie można uruchomić z `/` lub `http://`. Krótko mówiąc `src` informuje przeglądarkę do przeszukania, wartość atrybutu `Images` podfolder dla pliku o nazwie `PoweredByASPNET.gif`.

Podczas odwiedzania strony zawartości, znaczników powyżej są wysyłane bezpośrednio do przeglądarki. Poświęć chwilę, aby odwiedzić `About.aspx` i wyświetlić źródło HTML wysyłany do przeglądarki. Można zauważyć, że dokładnie tego samego znaczników na stronie głównej był wysyłany do przeglądarki.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Jeśli strony zawartości znajduje się w folderze głównym (ponieważ jest `About.aspx`) wszystko działa zgodnie z oczekiwaniami, ponieważ istnieje `Images` podfolder pokrewny folderu głównego. Jednak czynności podział Jeśli strony zawartości znajduje się w innym folderze niż strony wzorcowej. Na przykład utworzyć podfolder o nazwie `Admin`. Następnie dodaj strony zawartości o nazwie `Default.aspx` do `Admin` folderu, upewniając się powiązać nową stronę do `Site.master` strony wzorcowej.

> [!NOTE]
> W [ *określenie tytułu, tagi Meta i inne nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) samouczek utworzyliśmy klasy niestandardowej strony podstawowej o nazwie `BasePage` ustawiany automatycznie tytuł strony zawartości (jeśli go nie została jawnie przypisana). Nie zapomnij mają pochodzić od klasy związane z kodem nowo utworzonej strony `BasePage` tak, aby go może korzystać z tej funkcji.


Po utworzeniu tej strony zawartości z Eksploratora rozwiązań powinien wyglądać podobnie do rysunek 1.


![Nowy Folder i stron ASP.NET zostały dodane do projektu](urls-in-master-pages-vb/_static/image1.png)

**Rysunek 01**: Nowy Folder i stron ASP.NET zostały dodane do projektu


Następnie zaktualizuj `Web.sitemap` pliku, aby uwzględnić nowe `<siteMapNode>` wpis dla tej lekcji. Następujący kod XML zawiera pełną `Web.sitemap` znaczników, który zawiera teraz dodanie innej `<siteMapNode>` elementu.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

Nowo utworzony `Default.aspx` strony powinny mieć cztery formantów zawartości odpowiadający cztery elementy ContentPlaceHolders w `Site.master`. Dodaj tekst do formantu zawartości odwołujące się do `MainContent` ContentPlaceHolder, a następnie odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, przeglądarka nie może odnaleźć `PoweredByASPNET.gif` pliku obrazu. Co się dzieje w tym miejscu?

`~/Admin/Default.aspx` Wysłaniem tego samego kodu HTML strony zawartości `footerContent` regionu jako podano `About.aspx` strony:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Ponieważ `<img>` elementu `src` atrybut jest względny adres URL, przeglądarka próbuje do wyszukania `Images` folderu względem strony sieci web w lokalizacji folderu. Innymi słowy, przeglądarki jest szuka pliku obrazu `Admin/Images/PoweredByASPNET.gif`.


[![Nie można odnaleźć pliku obrazu PoweredByASPNET.gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Rysunek 02**: `PoweredByASPNET.gif` obrazu nie można odnaleźć pliku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Zastępowanie względnych adresów URL bezwzględnych adresów URL

Odwrotny niż w przypadku względnego adresu URL jest *bezwzględnego adresu URL*, który jest taki, który rozpoczyna się od ukośnika (`/`) lub protokołu, takich jak `http://`. Ponieważ bezwzględny adres URL określa lokalizację zasobów ze znanych punktu stały, ten sam bezwzględny adres URL jest prawidłowy w dowolnej strony sieci web, niezależnie od lokalizacji strony sieci web w strukturze folderu witryny sieci Web.

Aby rozwiązać uszkodzony obraz pokazany na rysunku 2, należy zaktualizować `<img>` elementu `src` atrybutu, tak aby były używane zamiast względną bezwzględnego adresu URL. Aby określić prawidłowy bezwzględny adres URL, odwiedź jedną stron sieci web w witrynie sieci Web i sprawdź, czy na pasku adresu. Jak pokazano na rysunku 2 paska adresu, w pełni kwalifikowana ścieżka do aplikacji sieci web jest `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. W związku z tym można modyfikacjom `<img>` elementu `src` atrybutu do jednej z następujących dwóch bezwzględnych adresów URL:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Poświęć chwilę, aby zaktualizować `<img>` elementu `src` atrybutu bezwzględny adres URL przy użyciu jednej formy przedstawionych powyżej, a następnie odwiedź `~/Admin/Default.aspx` strony za pośrednictwem przeglądarki. Teraz przeglądarki zostaną poprawnie znaleźć i wyświetlić `PoweredByASPNET.gif` pliku obrazu (patrz rysunek 3).


[![Obraz PoweredByASPNET.gif jest teraz wyświetlany](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Rysunek 03**: `PoweredByASPNET.gif` obraz jest teraz wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](urls-in-master-pages-vb/_static/image7.png))


Gdy twardych kodowania w bezwzględnego adresu URL działa, ściśle couples HTML do serwera witryny sieci Web i lokalizacji folderu, która może ulec zmianie. Przy użyciu bezwzględny adres URL w postaci `http://localhost:3908/...` jest łamliwe, ponieważ numer portu poprzedzających localhost jest wybierana automatycznie każdego uruchomienia programu Visual Studio wbudowanego ASP.NET Development serwera sieci Web. Podobnie `http://localhost` części jest prawidłowy tylko w przypadku testowania lokalnego. Po wdrożeniu kodu na serwerze produkcyjnym baza adresów URL zmieni się na inną, tak samo, jak `http://www.yourserver.com`. Bezwzględny adres URL w postaci `/ASPNET_MasterPages_Tutorial_04_VB/...` również odczuwa kruchości na tym samym, ponieważ często tej ścieżki aplikacji różni się między serwerami rozwoju i produkcji.

Dobre wieści jest, że program ASP.NET ma metodę generowania prawidłowy względny adres URL w czasie wykonywania.

## <a name="usingandresolveclienturl"></a>Przy użyciu`~`i`ResolveClientUrl`

Zamiast niż twardego kod bezwzględny adres URL, ASP.NET umożliwia deweloperom strony użyj tylda (`~`) wskazująca, katalog główny aplikacji sieci web. Na przykład we wcześniejszej części tego samouczka, można użyć notacji `~/Admin/Default.aspx` w tekście do odwoływania się do `Default.aspx` strony `Admin` folderu. `~` Oznacza to, że `Admin` folder jest podfolderem katalog główny aplikacji sieci web.

`Control` Klasy [ `ResolveClientUrl` metoda](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) pobiera adres URL i modyfikuje je do odpowiednich dla strony sieci web, na którym znajduje się kontrolka względnym adresem URL. Na przykład wywołanie elementu `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` z `About.aspx` zwraca `Images/PoweredByASPNET.gif`. Wywoływanie z `~/Admin/Default.aspx`, jednak zwraca `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Ponieważ wszystkich kontrolek serwera ASP.NET, pochodzi z `Control` klasy, wszystkie formanty serwera ma dostęp do `ResolveClientUrl` metody. Nawet `Page` pochodną klasy `Control` klasy, co oznacza, że można użyć tej metody bezpośrednio z klasy związane z kodem stron ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Przy użyciu`~`w znaczniku deklaratywne

Kilka formantów sieci Web ASP.NET obejmują właściwości dotyczące adresu URL: kontrolkę HyperLink `NavigateUrl` właściwości; obrazu formant ma `ImageUrl` właściwości; i tak dalej. Podczas renderowania tych kontrolek przekazania wartości właściwości związanych z adresu URL, aby `ResolveClientUrl`. W związku z tym jeśli te właściwości zawierają `~` wskaż, katalog główny aplikacji sieci web, adres URL zostaną zmodyfikowane i prawidłowe względnego adresu URL.

Należy pamiętać, że przekształcenie tylko kontrolek serwera ASP.NET `~` w ich właściwości związanych z adresu URL. Jeśli `~` jest wyświetlany w statycznej kod znaczników HTML, takie jak `<img src="~/Images/PoweredByASPNET.gif" />`, wysyła aparatu ASP.NET `~` w przeglądarce wraz z resztą zawartość HTML. Przeglądarka, przy założeniu, że `~` to część adresu URL. Na przykład, jeśli przeglądarka odbiera kod znaczników `<img src="~/Images/PoweredByASPNET.gif" />` przyjęto założenie, że istnieje podfolder o nazwie `~` podfolder `Images` zawierający plik obrazu `PoweredByASPNET.gif`.

Aby naprawić znacznika obrazu w `Site.master`, zastąpić istniejącą `<img>` element zawierający formant sieci Web ASP.NET obrazu. Ustawianie formantu obrazu w sieci Web `ID` do `PoweredByImage`, jego `ImageUrl` właściwości `~/Images/PoweredByASPNET.gif`, a jego `AlternateText` dla właściwości "Obsługiwane przez program ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Po wprowadzeniu tej zmiany do strony głównej, należy ponownie `~/Admin/Default.aspx` strony ponownie. Teraz `PoweredByASPNET.gif` plik obrazu jest wyświetlane na stronie (patrz rysunek 3). Gdy Web obrazu formant jest renderowany go używa `ResolveClientUrl` metodą rozwiązania jego `ImageUrl` wartości właściwości. W `~/Admin/Default.aspx` `ImageUrl` jest konwertowany na odpowiednie względny adres URL, jako następujący fragment kodu HTML źródło programów:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Oprócz używane w sieci Web opartych na adres URL właściwości formantu, `~` również może być używana przy wywoływaniu `Response.Redirect` i `Server.MapPath` metod, między innymi. Ponadto `ResolveClientUrl` może można wywołać metody bezpośrednio z platformy ASP.NET lub strony wzorcowej deklaratywne znaczników, jeśli to konieczne, zobacz [przenikanie Fritz](https://www.pluralsight.com/blogs/fritz/)jego wpis w blogu [Using `ResolveClientUrl` w znaczniku](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Ustalania strony wzorcowej pozostałych względnych adresów URL

Oprócz `<img>` element `footerContent` ten właśnie stała, strony głównej zawiera jeden więcej względny adres URL, wymagającego wymagające uwagi. `topContent` Region zawiera link "Wzorzec stron samouczki," wskazujący `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Ponieważ ten adres URL jest względny, będzie wysyłać użytkownikowi `Default.aspx` strony w folderze odwiedzają strony zawartość. Ma to łącze zawsze wskazywać `Default.aspx` w folderze głównym, należy zastąpić `<a>` element sieci Web HyperLink sterowania, aby firma Microsoft może używać `~` notacji.

Usuń `<a>` znaczników element i Dodaj formant hiperłącza w jego miejscu. Ustaw hiperłącza `ID` do `lnkHome`, jego `NavigateUrl` właściwości `~/Default.aspx`i jego `Text` dla właściwości "Samouczki stron Master".


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

To już wszystko! W tym momencie wszystkie adresy URL w naszej strony wzorcowej prawidłowo są oparte na podczas renderowania przez strony zawartości, niezależnie od tego, jakie foldery strony wzorcowej oraz strony zawartości znajdują się w.

### <a name="automatic-url-resolution-in-theheadsection"></a>Adres URL automatycznego rozpoznawania w`<head>`sekcji

W [ *tworzenia całej lokacji układu przy użyciu stron wzorcowych* ](creating-a-site-wide-layout-using-master-pages-vb.md) samouczek dodaliśmy `<link>` do `Styles.css` w pliku `<head>` regionu:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Gdy `<link>` elementu `href` atrybut jest względną, jest automatycznie konwertowany do odpowiednią ścieżkę w czasie wykonywania. Jak wspomniano w [ *określenie tytułu, tagi Meta i inne nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) samouczka `<head>` region jest rzeczywiście formantu po stronie serwera i umożliwia modyfikowanie zawartość jego wewnętrzny formantów, gdy jest on renderowany.

Aby to sprawdzić, należy ponownie `~/Admin/Default.aspx` i Wyświetl źródło HTML wysyłany do przeglądarki. Jak pokazano w poniższy fragment, `<link>` elementu `href` atrybut został automatycznie zmodyfikowany w celu odpowiedniego względny adres URL, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Podsumowanie

Strony główne bardzo często zawierają łącza, obrazy i innych zasobów zewnętrznych, które musi być określona za pomocą adresu URL. Ponieważ strony głównej i strony z zawartością nie istnieje w tym samym folderze, koniecznie angażuje się przy użyciu względnych adresów URL. Jest możliwe użycie zakodowany bezwzględnych adresów URL, więc ściśle czynności couples bezwzględny adres URL aplikacji sieci web. W przypadku zmiany bezwzględny adres URL — jak często ma podczas przenoszenia lub wdrażania aplikacji sieci web - masz Pamiętaj, aby wrócić i zaktualizować bezwzględnych adresów URL.

Nadaje się doskonale rozwiązaniem jest użycie tyldy (`~`) wskazująca, katalog główny aplikacji. Mapy formantów sieci Web ASP.NET, które zawierają właściwości związanych z adresu URL `~` do katalogu głównego aplikacji w czasie wykonywania. Wewnętrznie, użyj formantów sieci Web `Control` klasy `ResolveClientUrl` metodę, aby wygenerować prawidłowy względnego adresu URL. Ta metoda jest publiczna i dostępne z każdego serwera formantu (w tym `Page` klasy), aby można było ich użyć programowo z klas związane z kodem, w razie potrzeby.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Stron wzorcowych w programie ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Adres URL zmienianie bazy w strony wzorcowej](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Przy użyciu `ResolveClientUrl` w znaczniku](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [dalej](control-id-naming-in-content-pages-vb.md)
