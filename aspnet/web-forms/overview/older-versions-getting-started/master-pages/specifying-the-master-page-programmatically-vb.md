---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Określanie strony wzorcowej programowo (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Analizuje ustawienia strony zawartości strony wzorcowej programowo przy użyciu programu obsługi zdarzeń PreInit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ba2981e627199da89a25b0b59840f66521f2e78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="specifying-the-master-page-programmatically-vb"></a>Określanie strony wzorcowej programowo (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Analizuje ustawienia strony zawartości strony wzorcowej programowo przy użyciu programu obsługi zdarzeń PreInit.


## <a name="introduction"></a>Wprowadzenie

Od inauguracyjnym przykład [ *tworzenia całej lokacji układu przy użyciu stron wzorcowych*](creating-a-site-wide-layout-using-master-pages-vb.md), cała zawartość strony ma odwołanie do ich strony wzorcowej deklaratywnie za pośrednictwem `MasterPageFile` atrybutu w `@Page`dyrektywy. Na przykład następująca `@Page` dyrektywy łączy strony zawartości strony wzorcowej `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

[ `Page` Klasy](https://msdn.microsoft.com/library/system.web.ui.page.aspx) w `System.Web.UI` przestrzeń nazw zawiera [ `MasterPageFile` właściwości](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) która zwraca ścieżkę do strony głównej strony zawartości, lecz tej właściwości, który jest uporządkowany według `@Page` dyrektywy. Ta właściwość umożliwia również programowo Określ strony zawartości strony wzorcowej. Ta metoda jest przydatna, jeśli chcesz przypisać dynamicznie na podstawie czynników zewnętrznych, takich jak użytkownika, odwiedzając stronę strony wzorcowej.

W tym samouczku będziemy Dodaj drugą stronę wzorcową do naszej witryny sieci Web i dynamicznie zdecydować, które strony wzorcowej do użycia w czasie wykonywania.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Krok 1: Sprawdź w cyklu życia strony

Zawsze, gdy żądanie dociera do serwera sieci web dla strony platformy ASP.NET, który jest strony zawartości, aparatu ASP.NET musi Łączenie strony zawartości, kontrolek do strony głównej i odpowiadający jej element ContentPlaceHolder formantów. Ta fusion tworzy hierarchii pojedynczego formantu, które następnie mogą przejść przez cały cykl życia typowej strony.

Rysunek 1 pokazuje to fusion. Krok 1 na rysunku 1 przedstawiono oryginalnej zawartości i hierarchie formantu strony wzorcowej. Na końcu tail etap PreInit zawartość kontrolki na stronie są dodawane do odpowiednich Elementy ContentPlaceHolders na stronie głównej (krok 2). Po tym fusion strony wzorcowej służy jako elementem głównym hierarchii kolei formantu. To zespolone kontroli jest następnie dodawana do strony, aby utworzyć hierarchię ukończone formantu (krok 3). Wynikiem jest to, że hierarchia formantów strony zawiera hierarchię kolei formantu.


[![Strony wzorcowej i hierarchie kontroli zawartości strony są zespolone razem na etapie PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Rysunek 01**: Hierarchie formantu strony wzorcowej oraz strony zawartości są zespolone razem na etapie PreInit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Krok 2: Ustawienie`MasterPageFile`właściwości z kodu

Jakie strony wzorcowej partakes w tym fusion zależy od wartości `Page` obiektu `MasterPageFile` właściwości. Ustawienie `MasterPageFile` atrybutu w `@Page` dyrektywy skutkuje net przypisywanie `Page`w `MasterPageFile` właściwości etapie inicjalizacji jest pierwszego etap cyklu życia strony. Możemy również ustawić tę właściwość programowo. Jednak jest konieczne, że można ustawić tę właściwość przed dokonaniem fusion na rysunku 1.

Na początku na etapie PreInit `Page` obiekt zgłasza jego [ `PreInit` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) i wywołuje jego [ `OnPreInit` metody](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Aby ustawić strony wzorcowej programowo, następnie, możemy utworzyć programu obsługi zdarzeń dla `PreInit` zdarzenia lub zastąpienie `OnPreInit` metody. Przyjrzyjmy się obu podejść.

Uruchamianie przez otwarcie `Default.aspx.vb`, plik CodeBehind klasy naszej witrynie strony głównej. Dodaj program obsługi zdarzeń dla strony `PreInit` zdarzeń, wpisując polecenie w następującym kodzie:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

W tym miejscu możemy ustawić `MasterPageFile` właściwości. Zaktualizuj kod, tak aby przypisuje wartość "~ / Site.master" do `MasterPageFile` właściwości.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Jeśli Ustaw punkt przerwania i rozpoczynanie debugowania można będzie wyświetlana zawsze, gdy `Default.aspx` odwiedzoną stronę lub zawsze, gdy istnieje ogłaszania zwrotnego do tej strony `Page_PreInit` wykonuje program obsługi zdarzeń i `MasterPageFile` właściwości jest przypisany do "~ / Site.master".

Alternatywnie można zastąpić `Page` klasy `OnPreInit` — metoda i zestaw `MasterPageFile` właściwości. Na przykład załóżmy nie ustawienie strony wzorcowej w określonej strony, ale raczej z `BasePage`. Odwołaj się, że utworzyliśmy klasy niestandardowej strony podstawowej (`BasePage`) w [ *określenie tytułu, tagi Meta i inne nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) samouczka. Obecnie `BasePage` zastępuje `Page` klasy `OnLoadComplete` metody, gdzie ustawia strony `Title` właściwości oparte na danych mapy witryny. Ta funkcja pozwala zaktualizować `BasePage` również zastąpić `OnPreInit` metodę, aby określić programowo strony wzorcowej.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Ponieważ zawartości stronach pochodzi od `BasePage`, wszystkie mają teraz ich strony wzorcowej programowo przypisane. W tym momencie `PreInit` obsługi zdarzeń w `Default.aspx.vb` jest zbędny; możesz go usunąć.

### <a name="what-about-thepagedirective"></a>Jak`@Page`dyrektywy?

Co może być mylące nieco jest to, że strony zawartości `MasterPageFile` właściwości są teraz określany w dwóch miejscach: programowane w `BasePage` klasy `OnPreInit` — metoda również jako za pomocą `MasterPageFile` atrybutu w każdej strony zawartości `@Page` dyrektywy.

Pierwszym etapem w cyklu życia strony jest na etapie inicjalizacji. Na tym etapie `Page` obiektu `MasterPageFile` właściwości jest przypisywana wartość `MasterPageFile` atrybutu w `@Page` — dyrektywa (Jeśli podano). Na etapie PreInit następuje fazie inicjowania, a tutaj jest gdzie możemy programowane Ustawianie `Page` obiektu `MasterPageFile` właściwości, zastępując tym samym wartość przypisaną z `@Page` dyrektywy. Ponieważ konfigurujemy ustawienia `Page` obiektu `MasterPageFile` właściwości programowo, można usunąć `MasterPageFile` atrybutu z `@Page` dyrektywy bez wpływu na środowisko użytkownika końcowego. Aby przekonać użytkownika o tym, przejdź dalej i Usuń `MasterPageFile` atrybutu z `@Page` dyrektywę `Default.aspx` , a następnie odwiedź stronę za pośrednictwem przeglądarki. Jak można oczekiwać, dane wyjściowe jest taka sama jak przed ten atrybut został usunięty.

Czy `MasterPageFile` właściwości ustawione za pośrednictwem `@Page` dyrektywy lub programowo wpływu na środowisko użytkownika końcowego. Jednak `MasterPageFile` atrybutu w `@Page` dyrektywa jest używany przez program Visual Studio w czasie projektowania do produkcji WYSIWYG widoku w projektancie. Jeśli `Default.aspx` w programie Visual Studio i przejdź do projektanta zostanie wyświetlony komunikat "Błąd strony wzorcowej: strona ma formantów, które wymagają odwołania do strony wzorcowej, ale nie określono żadnego" (patrz rysunek 2).

Krótko mówiąc, należy pozostawić `MasterPageFile` atrybutu w `@Page` dyrektywy bogate środowisko czasu projektowania w programie Visual Studio.


[![Używa programu Visual Studio @Page atrybutu MasterPageFile dyrektywy do renderowania widoku Projekt](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Rysunek 02**: Visual Studio będzie korzystać z `@Page` dyrektywy `MasterPageFile` atrybutu do renderowania widoku Projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Krok 3: Tworzenie strony wzorcowej alternatywnej

Ponieważ strony wzorcowej strony zawartości można ustawić programowo w czasie wykonywania jest można dynamicznie załadować określonej strony wzorcowej na podstawie niektórych kryteriów zewnętrznych. Ta funkcja może być przydatne w sytuacjach, gdy układ witryny musi się różnić na podstawie użytkownika. Dla wystąpienia aplikacji sieci web aparat blogu może umożliwiają użytkownikom wybierz układ ich blog, gdzie każdy układ jest skojarzony z innej strony wzorcowej. W czasie wykonywania wyświetlając obiekt odwiedzający jest blog użytkownika aplikacji sieci web należy określić układ we wpisie i dynamicznie Skojarz odpowiedniej strony wzorcowej ze strony zawartość.

Przeanalizujmy sposobu dynamicznie ładowania strony wzorcowej podczas pracy zależnie od określonych kryteriów zewnętrznych. Nasze witryny sieci Web zawiera obecnie tylko jednej strony wzorcowej (`Site.master`). Potrzebujemy innej strony wzorcowej, aby zilustrować, wybierając stronę wzorcową w czasie wykonywania. Ten krok dotyczy przede wszystkim tworzenia i konfigurowania nowej strony wzorcowej. Krok 4 analizuje określania, jakie strony wzorcowej do użycia w czasie wykonywania.

Tworzenie nowej strony wzorcowej w folderze głównym o nazwie `Alternate.master`. Również dodać nowy arkusz stylów do witryny sieci Web o nazwie `AlternateStyles.css`.


[![Dodać kolejne strony wzorcowej i CSS plików do witryny sieci Web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Rysunek 03**: Dodaj innej strony wzorcowej i pliku CSS do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image9.png))


Zostało zaprojektowanych `Alternate.master` wyświetlany u góry strony wyśrodkowany i na tle granatowym tytuł strony wzorcowej. Zostały pominięte w lewej kolumnie, a przenieść zawartość poniżej `MainContent` formantu elementu ContentPlaceHolder teraz obejmuje całą szerokości strony. Ponadto nixed nieuporządkowaną listę — lekcje i zastąpione poziome powyższej listy `MainContent`. Zaktualizowano również czcionki i kolory używane przez strony wzorcowej (i przez rozszerzenie, na stronach zawartości). Na rysunku 4 przedstawiono `Default.aspx` przy użyciu `Alternate.master` strony wzorcowej.

> [!NOTE]
> Program ASP.NET zawiera możliwość definiowania *motywy*. Motyw jest kolekcją obrazów, plików CSS i powiązane stylu sieci Web kontroli ustawień właściwości, które może odnosić się do strony w czasie wykonywania. Motywy to sposób przejść układów witryny różnią się tylko w przypadku obrazów wyświetlanych i ich reguły CSS. Jeśli układów różnią się bardziej znacznie, takiej jak inne formanty sieci Web lub o układzie znacząco różnych, następnie należy używać oddzielnych stron wzorcowych. Na końcu tego samouczka, aby uzyskać więcej informacji dotyczących tematów można znaleźć w sekcji dalsze informacje.


[![Stron zawartości można teraz używać nowego wyglądu i działania](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Rysunek 04**: stron zawartości można teraz używać nowego wyglądu i działania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image12.png))


Gdy są zespolone wzorca i znaczników strony zawartości, `MasterPage` klasy sprawdza, upewnij się, że każdy zawartość kontrolki na stronie zawartości odwołuje się do elementu ContentPlaceHolder na stronie głównej. Jeśli formant zawartości, który odwołuje się do nieistniejącego elementu ContentPlaceHolder zostanie znaleziony, jest zgłaszany wyjątek. Innymi słowy, konieczne jest przypisywane do strony zawartości strony wzorcowej że ContentPlaceHolder dla każdego zawartości kontrolki na stronie zawartości.

`Site.master` Strony głównej zawiera cztery kontrolki elementu ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Niektóre strony zawartości w naszej witrynie sieci Web są tylko jeden lub dwa formanty zawartości; dla każdego z dostępnych Elementy ContentPlaceHolders inne zawierają zawartości formantu. Jeśli naszej nowej strony wzorcowej (`Alternate.master`) kiedykolwiek mogły zostać przypisane do tych stron zawartości, które zawierają wszystkie elementy ContentPlaceHolders w formanty zawartości `Site.master` , a następnie ważne jest, że `Alternate.master` również obejmować tych samych kontroli ContentPlaceHolder jako `Site.master`.

Aby uzyskać Twoje `Alternate.master` strony wzorcowej wyglądać podobnie do min (patrz rysunek 4), Rozpocznij od zdefiniowania stylów strony wzorcowej `AlternateStyles.css` arkusza stylów. Dodaj następujące reguły do `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Następnie dodaj następujący znacznik deklaratywne `Alternate.master`. Jak widać, `Alternate.master` zawiera cztery kontrolki elementu ContentPlaceHolder o takim samym `ID` wartości jako formantów elementu ContentPlaceHolder `Site.master`. Ponadto zawiera kontrolki Menedżera skryptów, które są niezbędne dla tych stron w naszej witrynie sieci Web, które używają programu ASP.NET AJAX framework.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testowanie nowej strony wzorcowej

Do testowania tej nowej aktualizacji strony wzorcowej `BasePage` klasy `OnPreInit` metody, aby `MasterPageFile` właściwości jest przypisywana wartość `"~/Alternate.maser"` , a następnie odwiedź witrynę sieci Web. Każdej strony powinny działać bez błędów, z wyjątkiem dwóch: `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx`. Dodawanie produktu do widoku DetailsView w `~/Admin/AddProduct.aspx` powoduje `NullReferenceException` z poziomu wiersza kodu, który podejmuje próbę ustawienia strony wzorcowej `GridMessageText` właściwości. Podczas odwiedzania `~/Admin/Products.aspx` `InvalidCastException` jest zgłaszany na załadowanie strony z komunikatem: "nie można rzutować obiektu typu" ASP.alternate\_wzorca "na typ" ASP.site\_master "."

Te błędy, ponieważ `Site.master` klasę kodu zawiera zdarzenia publiczne, właściwości i metody, które nie są zdefiniowane w `Alternate.master`. Część znacznika te dwie strony ma `@MasterType` dyrektywy, który odwołuje się do `Site.master` strony wzorcowej.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Ponadto DetailsView `ItemInserted` obsługi zdarzeń w `~/Admin/AddProduct.aspx` zawiera kod, który rzutuje typowaniem luźnym `Page.Master` dla właściwości typu obiektu `Site`. `@MasterType` — Dyrektywa (używane w ten sposób) i rzutowanie w `ItemInserted` obsługi zdarzeń ściśle pozamałżeńskie `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` strony do `Site.master` strony wzorcowej.

Aby przerwać ten ścisłej sprzężenia, firma Microsoft może mieć `Site.master` i `Alternate.master` pochodzi od klasy podstawowej wspólne, który zawiera definicje dla publicznych elementów członkowskich. Następujące, możemy zaktualizować `@MasterType` dyrektywy do odwołania tego wspólnego typu podstawowego.

### <a name="creating-a-custom-base-master-page-class"></a>Tworzenie klasy strony wzorca Base niestandardowe

Dodaj nowy plik klasy `App_Code` folder o nazwie `BaseMasterPage.vb` i została ona pochodzić od `System.Web.UI.MasterPage`. Musimy zdefiniować `RefreshRecentProductsGrid` — metoda i `GridMessageText` właściwości w `BaseMasterPage`, ale firma Microsoft nie może po prostu istnieje z `Site.master` ponieważ tych elementów członkowskich pracować z formantów sieci Web, które są specyficzne dla `Site.master` strony wzorcowej ( `RecentProducts` Element GridView i `GridMessage` etykiety).

Co należy zrobić to skonfigurować `BaseMasterPage` w taki sposób, zdefiniowano tych elementów członkowskich, ale faktycznie są implementowane przez `BaseMasterPage`do klas pochodnych (`Site.master` i `Alternate.master`). Ten rodzaj dziedziczenia jest możliwe przez oznaczenie klasy jako `MustInherit` i jej elementów członkowskich jako `MustOverride`. Krótko mówiąc, Dodawanie słowa kluczowe do klasy i jej elementów członkowskich w dwóch ogłasza który `BaseMasterPage` nie zaimplementowano `RefreshRecentProductsGrid` i `GridMessageText`, ale ten będzie jej klas pochodnych.

Należy również do definiowania `PricesDoubled` zdarzenia w `BaseMasterPage` i umożliwiają klasy pochodne, aby wywołać zdarzenie. Wzorzec używany w programie .NET Framework w celu umożliwienia to zachowanie jest utworzenie zdarzenie publiczne w klasie podstawowej i dodać chronionych, którą można przesłonić metodę o nazwie `OnEventName`. Klasy pochodne mogą wywoływać tej metody, aby zgłosić zdarzenie lub można ją wyłączyć, aby wykonać kod bezpośrednio przed lub po wywołaniu zdarzenia.

Aktualizacja Twojego `BaseMasterPage` klasy tak, aby zawierał następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Następnie należy przejść do `Site.master` CodeBehind klasy i została ona pochodzić od `BaseMasterPage`. Ponieważ `BaseMasterPage` zawiera elementy członkowskie oznaczone `MustOverride` należy zastąpić te elementy członkowskie tutaj `Site.master`. Dodaj `Overrides` — słowo kluczowe z definicjami metod i właściwości. Również zaktualizuj kod, który wywołuje `PricesDoubled` zdarzenia w `DoublePrice` przycisku `Click` obsługi zdarzeń przy użyciu wywołania do klasy podstawowej `OnPricesDoubled` metody.

Po modyfikacje `Site.master` klasę kodu powinna zawierać następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Również należy zaktualizować `Alternate.master`w klasie związanej z kodem pochodzić z `BaseMasterPage` i zastąpić dwa `MustOverride` elementów członkowskich. Jednak ponieważ `Alternate.master` nie zawiera element GridView, że listy najnowsze produkty ani etykietę, która wyświetla komunikat po nowym produktem jest dodawane do bazy danych, te metody nie trzeba wykonywać żadnych czynności.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Odwołanie do klasy strony podstawowej wzorca

Teraz, gdy firma Microsoft zakończył `BaseMasterPage` klasy i mieć naszych dwie strony wzorcowe, rozszerzając ją, ostatnią czynnością jest aktualizacja `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stron do odwoływania się do tego wspólnego typu. Najpierw zmienić `@MasterType` na obu stronach z dyrektywą:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

Do:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Zamiast odwołujące się do ścieżki pliku, `@MasterType` właściwości teraz odwołuje się do typu podstawowego (`BaseMasterPage`). W rezultacie jednoznacznie `Master` używany w klasach CodeBehind obie strony właściwości jest teraz typu `BaseMasterPage` (zamiast typu `Site`). Z tą zmianą w miejscu ponownie `~/Admin/Products.aspx`. Wcześniej, to spowodowała błąd rzutowanie ponieważ strony jest skonfigurowana do używania `Alternate.master` strony wzorcowej, ale `@MasterType` dyrektywy odwołania do `Site.master` pliku. Ale teraz renderuje stronę bez błędów. Jest to spowodowane `Alternate.master` strony wzorcowej mogą być rzutowane na obiekt typu `BaseMasterPage` (ponieważ rozszerza on).

Istnieje jeden niewielkie zmiany, który musi być wprowadzona w `~/Admin/AddProduct.aspx`. Kontrolki widoku szczegółów `ItemInserted` obsługi zdarzeń korzysta z obu jednoznacznie `Master` właściwości i typowaniem luźnym `Page.Master` właściwości. Usunięto odwołanie jednoznacznie podczas Zaktualizowaliśmy `@MasterType` dyrektywy, ale nadal konieczne zaktualizowanie typowaniem luźnym odwołania. Zastąp następujący wiersz kodu:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Z następujących czynności, które rzutuje `Page.Master` do typu podstawowego:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Krok 4: Określanie jakie strony wzorcowej do tworzenia powiązań zawartości strony

Nasze `BasePage` klasy ustawia obecnie wszystkie strony z zawartością `MasterPageFile` właściwości na wartość ustalony w etapie PreInit cyklu życia strony. Firma Microsoft może aktualizować ten kod, aby utworzyć strony wzorcowej na pewne czynniki zewnętrzne. Być może na załadowanie strony wzorcowej zależy od preferencje aktualnie zalogowanego użytkownika. W takim przypadku czy musimy pisania kodu `OnPreInit` metody w `BasePage` który wyszukuje Preferencje strony wzorcowej obecnie zaproszonych użytkowników.

Utwórzmy strona sieci web pozwala użytkownikowi na wybranie wzorca stronę, która ma być użyj - `Site.master` lub `Alternate.master` - i Zapisz ten wybór w zmiennej sesji. Rozpocznij od utworzenia nowej strony sieci web w katalogu głównym o nazwie `ChooseMasterPage.aspx`. Podczas tworzenia tej strony (lub innych zawartości stron odtąd) nie należy powiązać go do strony głównej strony wzorcowej ustawiono programowo `BasePage`. Jednak jeśli nie powiąże nowej strony do strony wzorcowej następnie nową stronę domyślną deklaratywne znaczników zawiera formularza sieci Web i innej zawartości dostarczonych przez strony wzorcowej. Należy ręcznie zastąpić ten kod znaczników odpowiednie formanty zawartości. Z tego powodu I łatwiejsze do nowej strony ASP.NET należy powiązać strony wzorcowej.

> [!NOTE]
> Ponieważ `Site.master` i `Alternate.master` mają ten sam zestaw kontrolek elementu ContentPlaceHolder nie ma znaczenia, jakie strony wzorcowej można wybrać podczas tworzenia nowej zawartości strony. Zgodność I sugerować przy użyciu `Site.master`.


[![Dodaj nową stronę zawartości witryny sieci Web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Rysunek 05**: Dodaj nową stronę zawartości witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image15.png))


Aktualizacja `Web.sitemap` pliku, aby uwzględnić wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` dla stron wzorcowych i ASP.NET AJAX lekcji:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Przed dodaniem żadnej zawartości do `ChooseMasterPage.aspx` strony Poświęć chwilę, aby zaktualizować klasy związane z kodem strony, tak aby dziedziczy `BasePage` (zamiast `System.Web.UI.Page`). Następnie dodaj formant DropDownList do strony, ustaw jej `ID` właściwości `MasterPageChoice`, i dodaj dwa elementy ListItems z `Text` wartości "~ / Site.master" i "~ / Alternate.master".

Dodawanie formantu przycisku sieci Web ze stroną i ustaw jego `ID` i `Text` właściwości `SaveLayout` i "Zapisywanie układu wyboru" odpowiednio. W tym momencie znaczników deklaratywne ze strony powinien wyglądać podobnie do następującego:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Po pierwsze odwiedzenia strony należy wyświetlić wybór użytkownika aktualnie wybranej strony wzorcowej. Utwórz `Page_Load` obsługi zdarzeń i Dodaj następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Powyższy kod wykonuje tylko na pierwszej wizyty strony (a nie kolejnych ogłaszania zwrotnego). Najpierw sprawdza, czy zmiennej sesji `MyMasterPage` istnieje. Jeśli tak, próbuje odnaleźć zgodnego elementu listy w `MasterPageChoice` DropDownList. Jeśli zostanie znaleziony pasującego elementu listy, jego `Selected` właściwość jest ustawiona na `True`.

Potrzebujemy kodu, który zapisuje wybór użytkownika do `MyMasterPage` zmiennej sesji. Tworzenie procedury obsługi zdarzeń dla `SaveLayout` przycisku `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> W czasie `Click` wykonuje obsługi zdarzenia odświeżania strony, strony wzorcowej została już wybrana. W związku z tym opcji wyboru z listy rozwijanej użytkownika nie będzie obowiązywać do momentu następnej strony można znaleźć. `Response.Redirect` Wymusza przeglądarkę, aby ponownie `ChooseMasterPage.aspx`.


Z `ChooseMasterPage.aspx` strona kompletna naszych ostatnim zadaniem jest `BasePage` przypisać `MasterPageFile` na podstawie właściwości na wartość `MyMasterPage` zmiennej sesji. Jeśli nie ustawiono zmiennej sesji `BasePage` domyślnie `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Po przeniesieniu kod, który przypisuje `Page` obiektu `MasterPageFile` właściwości poza `OnPreInit` obsługi zdarzeń i do dwóch oddzielnych metod. Pierwsza metoda `SetMasterPageFile`, przypisuje `MasterPageFile` właściwości do wartości zwracanej przez metodę drugi `GetMasterPageFileFromSession`. Po oznaczeniu `SetMasterPageFile` metody `Overridable` tak, aby w przyszłości klasy rozszerzyć `BasePage` można opcjonalnie zastąpić, aby zaimplementować niestandardowej logiki w razie potrzeby. Zajmiemy się tym przykładem zastępowanie `BasePage`w `SetMasterPageFile` właściwości w następnym samouczku.


Z tego kodu w miejscu, odwiedź stronę `ChooseMasterPage.aspx` strony. Początkowo `Site.master` strony wzorcowej jest wybrane (patrz rysunek 6), ale użytkownik może wybrać z listy rozwijanej innej strony wzorcowej.


[![Strony zawartości są wyświetlane przy użyciu strony wzorcowej Site.master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Rysunek 06**: strony zawartości są wyświetlane przy użyciu `Site.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Strony z zawartością są teraz wyświetlane przy użyciu strony wzorcowej Alternate.master](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Rysunek 07**: strony z zawartością jest teraz wyświetlany za pomocą `Alternate.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Podsumowanie

W przypadku odwiedzenia strony zawartości, jego formanty zawartości są zespolone z formantami ContentPlaceHolder jego strony wzorcowej. Strona zawartości strony wzorcowej jest oznaczona `Page` klasy `MasterPageFile` właściwość, która jest przypisana do `@Page` dyrektywy `MasterPageFile` atrybutu na etapie inicjalizacji. W samouczku pokazano, firma Microsoft można przypisać wartości do `MasterPageFile` tak długo, jak firma Microsoft zrobić przed końcem etap PreInit właściwości. Możliwość programowo Określ strony wzorcowej otwiera drzwi bardziej zaawansowanych scenariuszy, takich jak dynamiczne wiązanie strony zawartości do strony głównej na podstawie czynników zewnętrznych.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Diagram cyklu życia strony ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Przegląd cyklu życia strony ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Omówienie skórek i kompozycji ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Stron wzorcowych: Wskazówki dotyczące, wskazówki i pułapek](http://www.odetocode.com/articles/450.aspx)
- [Motywy w programie ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Suchi Banerjee. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-pages-and-asp-net-ajax-vb.md)
> [dalej](nested-master-pages-vb.md)
