---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Programowe określanie strony wzorcowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Patrzy na ustawienie poziomu strony zawartości strony wzorcowej programowo za pośrednictwem programu obsługi zdarzeń PreInit.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: a89964749ce8e127207ada6944a3d2ba513d3547
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754582"
---
<a name="specifying-the-master-page-programmatically-vb"></a>Programowe określanie strony wzorcowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Patrzy na ustawienie poziomu strony zawartości strony wzorcowej programowo za pośrednictwem programu obsługi zdarzeń PreInit.


## <a name="introduction"></a>Wprowadzenie

Ponieważ przykład inauguracyjnym w [ *tworzenia całej witryny układu za pomocą stron wzorcowych*](creating-a-site-wide-layout-using-master-pages-vb.md), cała zawartość strony ma odwołanie do ich strony wzorcowej w sposób deklaratywny za pośrednictwem `MasterPageFile` atrybutu w `@Page`dyrektywy. Na przykład następująca `@Page` dyrektywy łączy strony zawartości strony wzorcowej `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

[ `Page` Klasy](https://msdn.microsoft.com/library/system.web.ui.page.aspx) w `System.Web.UI` przestrzeń nazw zawiera [ `MasterPageFile` właściwość](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) , zwraca ścieżkę do strony wzorcowej strony zawartości; jest to właściwość, która została ustawiona przez `@Page` dyrektywy. Ta właściwość może również programowe określanie strony wzorcowej strony zawartość. Takie podejście jest przydatne, jeśli chcesz dynamiczne przypisywanie na podstawie zewnętrznych czynników, takich jak użytkownik, odwiedzając stronę strony wzorcowej.

W tym samouczku będziemy Dodaj drugą stronę wzorcową naszą witrynę internetową i dynamicznie zdecydować, które strony wzorcowej do użycia w czasie wykonywania.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Krok 1: Przyjrzeć się cyklu życia strony

Zawsze, gdy żądanie dociera do serwera sieci web dla strony ASP.NET, która jest używana jako strona zawartości, aparat programu ASP.NET należy Łączenie strony zawartości, kontrolek do strony głównej i odpowiadający jej kontrolek ContentPlaceHolder. Ta fusion tworzy hierarchii jeden formant, które następnie mogą przejść w całym cyklu życia typowej strony.

Rysunek 1 przedstawia ten fusion. Krok 1 na rysunku 1 przedstawiono oryginalnej zawartości i hierarchii kontroli strony wzorcowej. Na końcu tail etapu PreInit zawartość kontrolki na stronie są dodawane do odpowiednich kontrolek ContentPlaceHolder na stronie głównej (krok 2). Po tym fusion strony wzorcowej służy jako katalog główny hierarchii kolei kontroli. Tego zespolone kontroli jest następnie dodawana do strony Aby wygenerować hierarchii kontroli ukończone (krok 3). Wynikiem jest, że hierarchii kontroli strony zawiera hierarchii kolei kontroli.


[![Strona wzorcowa i hierarchii kontroli zawartości strony są zespolone ze sobą, na etapie PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Rysunek 01**: Hierarchie kontroli stronę wzorcową i stronę zawartości są zespolone razem na etapie PreInit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Krok 2: Ustawienie`MasterPageFile`właściwości z kodu

Jakie strony wzorcowej partakes w tym łączenia zależy od wartości `Page` obiektu `MasterPageFile` właściwości. Ustawienie `MasterPageFile` atrybutu w `@Page` dyrektywy efektem netto przypisywanie `Page`firmy `MasterPageFile` właściwości na etapie inicjowania, który jest pierwszy etap cyklu życia strony. Możemy też tę właściwość można ustawić programowo. Jednak należy bezwzględnie czy tej właściwości można ustawić przed rozpoczęciem fusion na rysunku 1.

Na początku tego etapu PreInit `Page` obiektu wywołuje jego [ `PreInit` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) i wywołuje jego [ `OnPreInit` metoda](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Aby programowo ustawić stronę wzorcową, następnie możemy utworzyć program obsługi zdarzeń dla `PreInit` zdarzenia lub zastąpienie `OnPreInit` metody. Przyjrzyjmy się oba podejścia.

Zacznij od otwarcia `Default.aspx.vb`, plik klasy CodeBehind strony głównej w naszej witrynie. Dodawanie obsługi zdarzeń dla strony `PreInit` zdarzeń, wpisując polecenie w następującym kodzie:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

W tym miejscu możemy ustawić `MasterPageFile` właściwości. Aktualizowanie kodu, tak aby przypisuje wartość "~ / Site.master" Aby `MasterPageFile` właściwości.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Jeśli ustawisz punkt przerwania i Rozpocznij debugowanie można będzie wyświetlana po każdym `Default.aspx` odwiedzoną stronę lub zawsze, gdy występuje zwrot do tej strony `Page_PreInit` wykonuje program obsługi zdarzeń i `MasterPageFile` właściwości jest przypisany do "~ / Site.master".

Alternatywnie możesz zastąpić `Page` klasy `OnPreInit` metody i ustaw `MasterPageFile` istnieje właściwość. W tym przykładzie załóżmy Nieustawione strony wzorcowej w określonej strony, ale raczej z `BasePage`. Pamiętaj, że firma Microsoft utworzone klasy niestandardowej strony podstawowej (`BasePage`) w [ *Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) samouczka. Obecnie `BasePage` zastępuje `Page` klasy `OnLoadComplete` metody, w którym ustawia strony `Title` właściwość oparte na danych mapy witryny. Zaktualizujmy `BasePage` można także Przesłoń `OnPreInit` metody programowe określanie strony wzorcowej.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Ponieważ naszych stron zawartości pochodzić od `BasePage`, wszystkie mają teraz ich strony wzorcowej programowo przypisane. W tym momencie `PreInit` programu obsługi zdarzeń w `Default.aspx.vb` jest zbędny; możesz je usunąć.

### <a name="what-about-thepagedirective"></a>Co`@Page`dyrektywy?

Jakie mogą być nieco mylące jest fakt, że stron zawartości `MasterPageFile` właściwości są teraz są określane w dwóch miejscach: programowo w `BasePage` klasy `OnPreInit` metoda również za pośrednictwem `MasterPageFile` atrybutu w każdej strony zawartości `@Page` dyrektywy.

Pierwszy etap w cyklu życia strony jest na etapie inicjowania. Na tym etapie `Page` obiektu `MasterPageFile` właściwość jest przypisywana wartość `MasterPageFile` atrybutu w `@Page` — dyrektywa (jeśli jest ona udostępniana). Na etapie PreInit następuje na etapie inicjowania, a tutaj jest gdzie możemy programowo ustawić `Page` obiektu `MasterPageFile` właściwości, w tym samym, zastępując wartość przypisana z `@Page` dyrektywy. Ponieważ ustawiamy `Page` obiektu `MasterPageFile` właściwość programowo, firma Microsoft może usunąć `MasterPageFile` atrybut z `@Page` dyrektywy bez wywierania wpływu na środowisko użytkownika końcowego. Samodzielnie to przekonanie, przejdź dalej i Usuń `MasterPageFile` atrybut z `@Page` dyrektywy w `Default.aspx` , a następnie odwiedź stronę za pośrednictwem przeglądarki. Jak można oczekiwać, dane wyjściowe jest taka sama jak przed atrybut został usunięty.

Czy `MasterPageFile` zostaje ustalona za pośrednictwem `@Page` dyrektywy lub programowo wpływu na środowisko użytkownika końcowego. Jednak `MasterPageFile` atrybutu w `@Page` dyrektywa jest używany przez program Visual Studio w czasie projektowania do produkcji WYSIWYG widok w projektancie. Po powrocie do `Default.aspx` w programie Visual Studio i przejdź do projektanta, zostanie wyświetlony komunikat "Błąd strony wzorcowej: strona zawiera formanty, które wymagają odwołania do strony wzorcowej, ale nie określono żadnego" (patrz rysunek 2).

Krótko mówiąc, należy pozostawić `MasterPageFile` atrybutu w `@Page` dyrektywy zaawansowane środowisko czasu projektowania w programie Visual Studio.


[![Program Visual Studio używa @Page atrybut MasterPageFile dyrektywy do renderowania widoku projektu](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Rysunek 02**: Visual Studio korzysta z `@Page` dyrektywy `MasterPageFile` atrybutu do renderowania widoku projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Krok 3: Tworzenie strony wzorcowej alternatywna

Ponieważ strony wzorcowej strony zawartości można programowo ustawić w czasie wykonywania, który można załadować dynamicznie konkretnej strony wzorcowej, na podstawie pewnych kryteriów zewnętrznych. Ta funkcja może być przydatne w sytuacjach, gdy układu witryny musi się różnić zależnie od użytkownika. Na przykład aplikacji sieci web aparatu blogu może umożliwiają użytkownikom wybierz układ jego blogu, gdzie każdy układ jest skojarzony z innej strony wzorcowej. W czasie wykonywania wyświetlając obiekt odwiedzający jest blogu użytkownika aplikacji sieci web należy określić układ blogu i dynamicznie przypisywać odpowiadającej mu stronie wzorcowej ze stroną zawartości.

Przeanalizujmy dynamicznie ładowanie strony wzorcowej w czasie wykonywania na podstawie pewnych kryteriów zewnętrznych. Nasza witryna obecnie zawiera tylko jedną stronę wzorcową (`Site.master`). Potrzebujemy innej strony wzorcowej, aby zilustrować, wybierając stronę wzorcową w czasie wykonywania. W tym kroku koncentruje się na temat tworzenia i konfigurowania nowej strony wzorcowej. Krok 4 patrzy na określenie, jakie strony wzorcowej do użycia w czasie wykonywania.

Tworzenie nowej strony wzorcowej w folderze głównym o nazwie `Alternate.master`. Również dodać nowy arkusz stylów do witryny sieci Web o nazwie `AlternateStyles.css`.


[![Dodaj kolejną stronę wzorcową i arkusze CSS plik do witryny sieci Web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Rysunek 03**: Dodaj innej strony wzorcowej i plik CSS do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image9.png))


Czy mogę zaprojektowania `Alternate.master` stronę wzorcową, która znajduje się tytuł wyświetlany u góry strony, a ich tematyka i granatowym tła. I zostały pominięte w kolumnie po lewej stronie i przenieść tę zawartość pod `MainContent` ContentPlaceHolder formant, który teraz obejmuje całą szerokość strony. Ponadto nixed listę nieuporządkowaną — lekcje i zastąpiliśmy go z powyższej listy poziomej `MainContent`. Zaktualizowano również czcionek i kolorów używanych przez strony wzorcowej (i przy użyciu rozszerzenia, na stronach zawartości). Rysunek 4 przedstawia `Default.aspx` przy użyciu `Alternate.master` strony wzorcowej.

> [!NOTE]
> Program ASP.NET zawiera możliwość definiowania *motywy*. Motyw jest kolekcją, obrazy, pliki CSS i stylu związane sieci Web kontroli ustawień właściwości, które mogą być stosowane do strony w czasie wykonywania. Motywy to sposób Jeśli układy witryny różnią się tylko w przypadku obrazów wyświetlanych i ich reguły CSS. Jeśli układów różnią się w bardziej znacznie, takiej jak inne kontrolki sieci Web lub mieć znacznie inny układ, następnie konieczne będzie używać osobnych stron wzorcowych. Na końcu tego samouczka, aby uzyskać więcej informacji dotyczących tematów można znaleźć w sekcji dalsze informacje.


[![Nasze strony zawartości można teraz używać nowego wyglądu i działania](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Rysunek 04**: stron zawartości można teraz używać nowego wyglądu i sposobu działania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image12.png))


Gdy wzorzec i znaczników stron zawartości są zespolone, `MasterPage` klasy kontrole, aby upewnić się, że zawartość każdej kontrolki na stronie zawartości odwołuje się do ContentPlaceHolder na stronie głównej. Wyjątek jest generowany, jeśli zostanie znaleziony w formancie zawartości, który odwołuje się do nieistniejącej ContentPlaceHolder. Innymi słowy, konieczne jest przypisywane do strony zawartości strony wzorcowej że ContentPlaceHolder dla każdego formantu na stronie zawartości zawartości.

`Site.master` Strona główna zawiera cztery kontrolki ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Niektóre zawartości stron w naszej witryny sieci Web to tylko jeden lub dwa formanty zawartości; dla każdego z dostępnych kontrolek ContentPlaceHolder inne zawierają kontrolki zawartości. Jeśli nasza nowa strona wzorca (`Alternate.master`) nigdy nie mogą być przypisane do tych stron zawartości, zawierających formanty zawartości dla wszystkich kontrolek ContentPlaceHolder w `Site.master` , a następnie istotne jest, który `Alternate.master` również obejmować tych samych kontrolek ContentPlaceHolder jako `Site.master`.

Aby uzyskać swoje `Alternate.master` strony wzorcowej wyglądają podobnie, aby analizować (zobacz rysunek 4), Rozpocznij od zdefiniowania style strony wzorcowej w `AlternateStyles.css` arkusza stylów. Dodaj następujące reguły do `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Następnie dodaj poniższe oznaczeniu deklaracyjnym do `Alternate.master`. Jak widać, `Alternate.master` zawiera cztery kontrolki ContentPlaceHolder o takiej samej `ID` wartości co w przypadku kontrolek ContentPlaceHolder w `Site.master`. Ponadto zawiera ona formantu ScriptManager, co jest niezbędne do tych stron w naszej witryny sieci Web, które korzystaj z platformy ASP.NET AJAX.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testowanie nowej strony wzorcowej

Do testowania tej aktualizacji strony wzorcowej `BasePage` klasy `OnPreInit` metody, aby `MasterPageFile` właściwość jest przypisywana wartość `"~/Alternate.maser"` , a następnie odwiedź witryny sieci Web. Każdej strony powinny działać bez błędów, z wyjątkiem dwóch: `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx`. Dodawanie produktu do DetailsView w `~/Admin/AddProduct.aspx` skutkuje `NullReferenceException` z wiersza kodu, który podejmuje próbę ustawienia strony wzorcowej `GridMessageText` właściwości. Podczas odwiedzania `~/Admin/Products.aspx` `InvalidCastException` jest zgłaszany po załadowaniu strony z komunikatem: "nie można rzutować obiektu typu" ASP.alternate\_master "na typ" ASP.site\_master "."

Te błędy, ponieważ `Site.master` osobna klasa kodu obejmuje zdarzenia publiczne, właściwości i metody, które nie są zdefiniowane w `Alternate.master`. Fragment kodu znaczników te dwie strony mają `@MasterType` dyrektywę, który odwołuje się do `Site.master` strony wzorcowej.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Ponadto DetailsView `ItemInserted` programu obsługi zdarzeń w `~/Admin/AddProduct.aspx` zawiera kod, który rzutuje typowaniem luźnym `Page.Master` właściwość do obiektu typu `Site`. `@MasterType` — Dyrektywa (używana w ten sposób) i rzutowanie w `ItemInserted` programu obsługi zdarzeń ściśle pozamałżeńskie `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` strony do `Site.master` strony wzorcowej.

Aby przerwać to ścisłego sprzężenia, firma Microsoft może mieć `Site.master` i `Alternate.master` dziedziczyć wspólna klasa bazowa, który zawiera definicje publicznych składowych. Poniżej, firma Microsoft może aktualizować `@MasterType` dyrektywy odwołania do tego wspólnego typu podstawowego.

### <a name="creating-a-custom-base-master-page-class"></a>Tworzenie klasy strony niestandardowe podstawowy wzorzec

Dodaj nowy plik klasy do `App_Code` folder o nazwie `BaseMasterPage.vb` potem z łatwością dziedziczyć `System.Web.UI.MasterPage`. Należy zdefiniować `RefreshRecentProductsGrid` metody i `GridMessageText` właściwości w `BaseMasterPage`, ale nie możemy po prostu przenieść je tam `Site.master` ponieważ te elementy członkowskie pracować kontrolki sieci Web, które są specyficzne dla `Site.master` strony wzorcowej ( `RecentProducts` GridView i `GridMessage` etykiety).

Co należy zrobić to skonfigurować `BaseMasterPage` w taki sposób, te elementy członkowskie zdefiniowano, że faktycznie są implementowane przez `BaseMasterPage`w klasach pochodnych (`Site.master` i `Alternate.master`). Tego rodzaju dziedziczenia jest możliwe, oznaczając klasy jako `MustInherit` i jej elementów członkowskich jako `MustOverride`. Krótko mówiąc, dodawanie tych słów kluczowych do klasy i składowych dwa ogłasza, `BaseMasterPage` nie zaimplementowana `RefreshRecentProductsGrid` i `GridMessageText`, ale ta będzie jej klas pochodnych.

Należy również zdefiniować `PricesDoubled` zdarzenia w `BaseMasterPage` i zapewniają środki przez klasy pochodne, aby zgłosić zdarzenie. Wzorzec używany w programie .NET Framework w celu ułatwienia zachowania jest utworzenie publiczne zdarzenie w klasie bazowej, a następnie dodać chronionych, którą można przesłonić metodę o nazwie `OnEventName`. Klasy pochodne następnie wywołać tę metodę, aby wygenerować zdarzenie, lub można zmienić, aby wykonać kod bezpośrednio przed lub po wywołaniu zdarzenia.

Aktualizacja usługi `BaseMasterPage` klasy tak, aby zawierała następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Następnie przejdź do `Site.master` związanym z kodem klasy i jest pochodną `BaseMasterPage`. Ponieważ `BaseMasterPage` zawiera składowe oznaczone `MustOverride` należy zastąpić te elementy członkowskie w tym miejscu `Site.master`. Dodaj `Overrides` słowa kluczowego definicje metod i właściwości. Również zaktualizować kod, który wywołuje `PricesDoubled` zdarzenia w `DoublePrice` przycisku `Click` programu obsługi zdarzeń przy użyciu wywołania do klasy bazowej `OnPricesDoubled` metody.

Po tych zmian `Site.master` z kodem klasę powinna zawierać następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Musimy też zaktualizować `Alternate.master`firmy pochodzi od klasy związane z kodem `BaseMasterPage` i zastąpić dwa `MustOverride` elementów członkowskich. Ale ponieważ `Alternate.master` nie zawiera GridView, wyświetla najnowsze produkty ani etykietę, która wyświetla komunikat po nowego produktu nie zostanie dodany do bazy danych, te metody nie trzeba podejmować żadnych działań.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Odwołanie do klasy strony podstawowy wzorzec

Teraz, gdy ukończyliśmy `BaseMasterPage` klasy i mieć nasze dwie strony wzorcowe, rozszerzając go, ostatnią czynnością jest zaktualizowanie `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stron do odwoływania się do tego typu wspólnego. Rozpocznij od zmiany `@MasterType` dyrektywy w obie strony z:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

Do:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Zamiast odwołujące się do ścieżki pliku, `@MasterType` właściwość teraz odwołuje się typ podstawowy (`BaseMasterPage`). W związku z tym, że wpisane pogrubieniem `Master` właściwość używana w klasach związanym z kodem w obie strony jest teraz typu `BaseMasterPage` (zamiast typu `Site`). Dzięki tej zmianie w miejscu ponownie `~/Admin/Products.aspx`. Wcześniej, to spowodowało błąd rzutowania ponieważ strony jest skonfigurowany do używania `Alternate.master` stronę wzorcową, ale `@MasterType` dyrektywy odwołania `Site.master` pliku. Ale teraz renderuje stronę bez błędów. Jest to spowodowane `Alternate.master` strony wzorcowej mogą być rzutowane na obiekt typu `BaseMasterPage` (ponieważ rozszerza on).

Istnieje jeden niewielką zmianę, który ma nastąpić w `~/Admin/AddProduct.aspx`. Kontrolce DetailsView `ItemInserted` programu obsługi zdarzeń korzysta z obu silnie typizowaną `Master` właściwość i typowaniem luźnym `Page.Master` właściwości. Usunięto odwołanie silnie typizowane, gdy Zaktualizowaliśmy `@MasterType` dyrektywy, ale nadal konieczne zaktualizowanie typowaniem luźnym odwołania. Zastąp następujący wiersz kodu:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Z następujących pozycji, której rzutuje `Page.Master` do typu podstawowego:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Krok 4: Określająca jakie strony wzorcowej do wiązania zawartości strony

Nasze `BasePage` klasy Ustawia aktualnie wszystkie zawartości stron `MasterPageFile` właściwości ustaloną wartość w PreInit etapie cyklu życia strony. Aktualizujemy ten kod, aby utworzyć stronę wzorcową na niektóre czynniki zewnętrzne. Być może strony wzorcowej do załadowania zależy od preferencji aktualnie zalogowanego użytkownika. W takiej sytuacji firma Microsoft musiałaby pisanie kodu w `OnPreInit` method in Class metoda `BasePage` , wyszukuje Preferencje strony wzorcowej obecnie zaproszonych użytkowników.

Utwórz stronę sieci web, która umożliwia użytkownikowi określenie, które strony wzorcowej do użycia — `Site.master` lub `Alternate.master` — i Zapisz ten wybór w zmiennej sesji. Rozpocznij od utworzenia nowej strony sieci web w katalogu głównym o nazwie `ChooseMasterPage.aspx`. Podczas tworzenia tej strony (lub inne zawartości strony od tej pory) nie musisz powiązać go do strony wzorcowej programowe Ustawianie strony wzorcowej `BasePage`. Jednak jeśli nie powiążesz nowej strony do strony wzorcowej następnie nową stronę domyślną oznaczeniu deklaracyjnym zawiera formularz sieci Web i innej zawartości, dostarczone przez strony wzorcowej. Należy ręcznie Zastąp ten kod znaczników odpowiednie formanty zawartości. Z tego powodu I łatwiejsze do nowej strony programu ASP.NET należy powiązać strony wzorcowej.

> [!NOTE]
> Ponieważ `Site.master` i `Alternate.master` mają ten sam zestaw kontrolek ContentPlaceHolder nieważne, jakiego stronę wzorcową, możesz wybrać, tworząc nową stronę zawartości. W celu zapewnienia spójności I moglibyśmy przy użyciu `Site.master`.


[![Dodaj nową stronę zawartości do witryny sieci Web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Rysunek 05**: Dodaj nową stronę zawartości do witryny internetowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image15.png))


Aktualizacja `Web.sitemap` plik, aby dołączyć wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` dla strony wzorcowe i ASP.NET AJAX lekcji:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Przed dodaniem żadnej zawartości do `ChooseMasterPage.aspx` strony Poświęć chwilę, aby zaktualizować strony osobna klasa kodu tak, aby pochodzi od klasy `BasePage` (zamiast `System.Web.UI.Page`). Następnie dodaj formant DropDownList do strony, ustaw jego `ID` właściwości `MasterPageChoice`, i dodaj dwa ListItems z `Text` wartości "~ / Site.master" i "~ / Alternate.master".

Dodawanie kontrolki przycisku w sieci Web do strony i ustaw jego `ID` i `Text` właściwości `SaveLayout` i "Zapisywanie układu", odpowiednio. W tym momencie oznaczeniu deklaracyjnym strony powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Po pierwsze odwiedzenia strony należy wyświetlić wybór strony wzorcowej aktualnie wybranego użytkownika. Utwórz `Page_Load` program obsługi zdarzeń i Dodaj następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Powyższy kod wykonuje tylko na pierwszej wizyty strony (a nie kolejnych ogłaszania zwrotnego). Najpierw sprawdza, czy zmienna sesji `MyMasterPage` istnieje. Jeśli tak jest, próbuje odnaleźć zgodnego elementu listy w `MasterPageChoice` DropDownList. Jeśli zostanie znaleziony pasującego elementu listy, jego `Selected` właściwość jest ustawiona na `True`.

Należy również kod, który zapisuje wybrany przez użytkownika do `MyMasterPage` zmiennej sesji. Utwórz procedurę obsługi zdarzeń dla `SaveLayout` przycisku `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Do czasu `Click` program obsługi zdarzeń jest wykonywana na odświeżenie strony, strony wzorcowej został już wybrany. W związku z tym opcji wyboru z listy rozwijanej użytkownika nie będą obowiązywać, dopóki nie można znaleźć w następnej strony. `Response.Redirect` Wymusza przeglądarkę, aby ponownie `ChooseMasterPage.aspx`.


Za pomocą `ChooseMasterPage.aspx` strona kompletna naszych ostatnim zadaniem jest zapewnienie `BasePage` przypisać `MasterPageFile` właściwości na podstawie wartości z `MyMasterPage` zmiennej sesji. Jeśli nie ustawiono zmiennej sesji `BasePage` domyślnie `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Czy mogę przenieść kod, który przypisuje `Page` obiektu `MasterPageFile` właściwości z `OnPreInit` program obsługi zdarzeń do dwóch oddzielnych metod. Ta pierwsza metoda `SetMasterPageFile`, przypisuje `MasterPageFile` właściwości do wartości zwracanej przez druga metoda `GetMasterPageFileFromSession`. Po oznaczeniu `SetMasterPageFile` metoda `Overridable` tak, aby w przyszłości klas, które rozszerzają `BasePage` można opcjonalnie zastąpić, aby zaimplementować logikę niestandardową, w razie potrzeby. Zajmiemy się tym przykładem zastępowanie `BasePage`firmy `SetMasterPageFile` właściwość w następnym samouczku.


Przy użyciu tego kodu w miejscu, odwiedź stronę `ChooseMasterPage.aspx` strony. Początkowo `Site.master` strony wzorcowej jest wybrane (patrz rysunek 6), ale użytkownik może wybrać innej strony wzorcowej, z listy rozwijanej.


[![Zawartość strony są wyświetlane przy użyciu strony wzorcowej Site.master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Rysunek 06**: stron zawartości są wyświetlane przy użyciu `Site.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Strony z zawartością są teraz wyświetlane przy użyciu strony wzorcowej Alternate.master](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Rysunek 07**: stron zawartości są teraz wyświetlane przy użyciu `Alternate.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Podsumowanie

W przypadku odwiedzenia strony zawartości, jej kontrolek zawartości są połączone z kontrolek ContentPlaceHolder jego strony wzorcowej funkcjami. Strona zawartości strony wzorcowej jest wskazywane przez `Page` klasy `MasterPageFile` właściwość, która jest przypisana do `@Page` dyrektywy `MasterPageFile` atrybutu na etapie inicjowania. Jak w tym samouczku pokazano, firma Microsoft można przypisać wartości do `MasterPageFile` tak długo, jak możemy to zrobić przed zakończeniem etapu PreInit właściwości. Możliwość programowe określanie strony wzorcowej otwiera drzwi dla bardziej zaawansowanych scenariuszy, takich jak dynamiczne powiązanie zawartości strony na stronie wzorcowej, na podstawie czynników zewnętrznych.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Diagram cyklu życia strony ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Przegląd cyklu życia strony ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Omówienie skórek i motywów programu ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Strony wzorcowe: Porady, wskazówki i pułapki](http://www.odetocode.com/articles/450.aspx)
- [Motywy w programie ASP.NET:](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Suchi Banerjee. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-pages-and-asp-net-ajax-vb.md)
> [dalej](nested-master-pages-vb.md)
