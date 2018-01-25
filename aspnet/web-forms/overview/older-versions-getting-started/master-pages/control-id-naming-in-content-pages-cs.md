---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: "Kontrolowanie Identyfikatora nazwy w strony z zawartością (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Przedstawia sposób kontrolki elementu ContentPlaceHolder służyć jako kontenera nazewnictwa i w związku z tym należy programowo Praca z formantem trudne (za pośrednictwem FindConrol)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c0db7fd76a7a486ff45085329ef7c77b0af5ebe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="control-id-naming-in-content-pages-c"></a>Identyfikator formantu nazewnictwa na stronach zawartości (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Przedstawiono sposób kontrolki elementu ContentPlaceHolder służyć jako kontenera nazewnictwa i w związku z tym należy programowo Praca z formantem trudne (za pośrednictwem FindConrol). Analizuje tego problemu i jego rozwiązania. Omówiono także sposób programowego dostępu wynikowej wartości identyfikatora klienta.


## <a name="introduction"></a>Wprowadzenie

Obejmują wszystkie kontrolek serwera ASP.NET `ID` właściwość, która unikatowo identyfikuje kontrolki i sposób za pomocą której formant programowo jest dostępny w klasie związanej z kodem. Podobnie, może zawierać elementów w dokumencie HTML `id` atrybut, który unikatowo identyfikuje element; te `id` wartości są często używane w skryptu po stronie klienta do programowego odwołuje się do określonego elementu HTML. Biorąc pod uwagę to, użytkownik może przyjęto założenie, że kontrolka serwerowa ASP.NET jest renderowany w kodzie HTML, jego `ID` wartość jest używana jako `id` wartość renderowanego elementu HTML. Nie jest wielkość liter, ponieważ w niektórych sytuacjach jeden kontrolować za pomocą jednej `ID` wartość mogą pojawić się wiele razy w renderowanego kodu znaczników. Należy wziąć pod uwagę kontrolce GridView zawierający pole TemplateField za pomocą formantu etykiety sieci Web z `ID` wartość ProductName. Widoku GridView jest powiązany ze swoim źródłem danych w czasie wykonywania, etykieta jest powtarzany raz dla każdego wiersza w widoku GridView. Każdy renderowane potrzeb etykiety unikatowego `id` wartość.

Do obsługi takich scenariuszy, ASP.NET umożliwia niektórych formantów być oznaczona jako nazw kontenerów. Kontener nazewnictwa służy jako nowy `ID` przestrzeni nazw. Wszystkie kontrolki serwera, które pojawiają się w kontenerze nazewnictwa ma ich renderowanych `id` wartość prefiks `ID` formantu kontenera nazewnictwa. Na przykład `GridView` i `GridViewRow` klasy są oba kontenery nazewnictwa. W rezultacie Etykieta zdefiniowana w widoku GridView TemplateField z `ID` ProductName podano renderowanych `id` wartość `GridViewID_GridViewRowID_ProductName`. Ponieważ *GridViewRowID* jest unikatowy dla każdego wiersza w widoku GridView, powstałe w ten sposób `id` wartości są unikatowe.

> [!NOTE]
> [ `INamingContainer` Interfejsu](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) służy do wskazania, że określonego formantu serwera ASP.NET powinny działać jako kontenera nazewnictwa. `INamingContainer` Żadnych metod, które kontrolki serwera musi implementować interfejs nie pełnych; zamiast jest używany jako znacznik. Podczas generowania renderowanego kodu znaczników, jeśli formant implementuje ten interfejs następnie aparatu ASP.NET automatycznie prefiksy jego `ID` renderowania wartości jego elementów podrzędnych `id` wartości atrybutów. Ten proces jest omówiona bardziej szczegółowo w kroku 2.


Kontenery nazewnictwa nie tylko zmienić renderowanych `id` wartość atrybutu, ale również wpływa na sposób kontrolki odwołania mogą dotyczyć programowo z klasy związane z kodem strony ASP.NET. `FindControl("controlID")` Metody jest najczęściej używany do programowego odwołania formantu sieci Web. Jednak `FindControl` nie spenetrowanie za pomocą nazw kontenerów. W rezultacie nie można bezpośrednio używać `Page.FindControl` metodę odwołują się do formantów w widoku GridView lub innych kontenera nazewnictwa.

Ponieważ użytkownik może mieć surmised, stron wzorcowych i Elementy ContentPlaceHolders są oba zaimplementowane jako nazw kontenerów. W tym samouczku omówione jak główny element HTML wpływają na stronach `id` wartości i sposoby programowo odwołują się do formantów sieci Web w obrębie zawartości strony przy użyciu `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Krok 1: Dodawanie nowej strony ASP.NET

Aby zademonstrować omówione w tym samouczku, Dodajmy nowej strony ASP.NET do naszej witryny sieci Web. Utwórz nową stronę zawartości o nazwie `IDIssues.aspx` w folderze głównym powiązania go do `Site.master` strony wzorcowej.


![Dodawanie zawartości IDIssues.aspx strony do folderu głównego](control-id-naming-in-content-pages-cs/_static/image1.png)

**Rysunek 01**: Dodaj stronę zawartości `IDIssues.aspx` do folderu głównego


Visual Studio automatycznie tworzy kontrolkę zawartości dla każdego z czterech Elementy ContentPlaceHolders strony wzorcowej. Zgodnie z opisem w [ *wielu Elementy ContentPlaceHolders i zawartości domyślnej* ](multiple-contentplaceholders-and-default-content-cs.md) samouczek, jeśli nie ma zawartości formantu zamiast tego emitowanego zawartości elementu ContentPlaceHolder domyślnej strony wzorcowej. Ponieważ `QuickLoginUI` i `LeftColumnContent` Elementy ContentPlaceHolders zawierają znaczników odpowiedniej wartości domyślnej dla tej strony, przejdź dalej i usuń odpowiednie kontrolki zawartości z `IDIssues.aspx`. W tym momencie deklaratywne znaczników strony zawartości powinien wyglądać następująco:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

W [ *określenie tytułu, tagi Meta i inne nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) samouczek utworzyliśmy klasy niestandardowej strony podstawowej (`BasePage`) który konfiguruje automatycznie tytuł strony w przypadku nie jest jawnie ustawione. Aby uzyskać `IDIssues.aspx` strony, aby zastosować tę funkcję, strony klasę kodu musi pochodzić od `BasePage` klasy (zamiast `System.Web.UI.Page`). Należy zmodyfikować definicję klasy związane z kodem, tak, aby go wygląda następująco:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Na koniec zaktualizuj `Web.sitemap` pliku, aby uwzględnić wpis dla tej nowej lekcji. Dodaj `<siteMapNode>` element i ustaw jej `title` i `url` atrybuty "Problemów nazewnictwa Identyfikatora formantu" i `~/IDIssues.aspx`odpowiednio. Po wprowadzeniu to dodawanie użytkownika `Web.sitemap` pliku znaczników powinien wyglądać podobnie do następującego:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Jak pokazano na rysunku 2, nowy wpis mapy witryny w `Web.sitemap` jest natychmiast odzwierciedlone w sekcji lekcje w lewej kolumnie.


![Sekcja — lekcje zawiera teraz łącze do &quot;nazewnictwa problemów identyfikator formantu&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Rysunek 02**: sekcja — lekcje teraz łącze umożliwiające "Identyfikatora formantu nazewnictwa problemów"


## <a name="step-2-examining-the-renderedidchanges"></a>Krok 2: Sprawdzenie renderowanych`ID`zmiany

Aby lepiej zrozumieć modyfikacje ASP.NET aparatowi do renderowanej `id` kontrolki wartości serwera, możemy dodać kilka formantów sieci Web do `IDIssues.aspx` strony, a następnie Wyświetl renderowanego kodu znaczników wysyłany do przeglądarki. W szczególności, wpisz tekst "Wprowadź swój wiek:" następuje formantu sieci Web pola tekstowego. Dodatkowo w dół na stronie Dodaj formantu przycisku sieci Web i formantu etykiety w sieci Web. Ustaw pole tekstowe `ID` i `Columns` właściwości `Age` i 3, odpowiednio. Ustaw przycisku `Text` i `ID` właściwości "Zatwierdź" i `SubmitButton`. Czyści etykiety `Text` właściwości i zestaw jej `ID` do `Results`.

W tym momencie znaczników deklaratywne formantu zawartości powinien wyglądać podobnie do następującego:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Rysunek 3 przedstawia stronie podczas przeglądania za pomocą projektanta programu Visual Studio.


[![Strona zawiera trzy formantów sieci Web: pole tekstowe, przycisków i etykiet](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Rysunek 03**: strona zawiera trzy formantów sieci Web: pole tekstowe, przycisków i etykiet ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image5.png))


Odwiedź stronę za pośrednictwem przeglądarki, a następnie wyświetlić źródło HTML. Jako kod znaczników, poniżej przedstawiono `id` wartości elementów HTML dla kontrolki pola tekstowego, przycisk i Web etykiety są kombinacją `ID` wartości formantów sieci Web i `ID` wartości nazw kontenerów na stronie.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Jak wspomniano wcześniej w tym samouczku, zarówno strony wzorcowej i jego elementy ContentPlaceHolders stanowią nazewnictwa kontenerów. W rezultacie współtworzenia zarówno renderowanych `ID` wartości ich zagnieżdżonych kontrolek. Pole tekstowe zająć `id` atrybutu, na przykład: `ctl00_MainContent_Age`. Odwołania, który formantu TextBox `ID` wartość `Age`. Przedrostek jego kontrolą ContentPlaceHolder `ID` wartość `MainContent`. Ponadto ta wartość jest prefiksem strony wzorcowej `ID` wartość `ctl00`. Net efekt jest `id` wartość atrybutu składające się z `ID` wartości strony wzorcowej, kontrolki elementu ContentPlaceHolder i pole tekstowe samej siebie.

Rysunek 4 przedstawia tego zachowania. Aby określić renderowanych `id` z `Age` pole tekstowe, rozpoczyna się od `ID` wartość formantu TextBox `Age`. Następnie przechodzić w górę hierarchii formantu. W każdym kontenera nazewnictwa (tych węzłów brzoskwini kolorem), prefiks bieżącego renderowane `id` z kontenera nazewnictwa `id`.


![Atrybuty identyfikatora Rendered są oparte na wartości identyfikatorów nazw kontenerów](control-id-naming-in-content-pages-cs/_static/image6.png)

**Rysunek 04**: renderowane `id` atrybuty są oparte na `ID` wartości nazw kontenerów


> [!NOTE]
> Jak wspomniano, `ctl00` część renderowanych `id` stanowi atrybutu `ID` wartość strony wzorcowej, ale być może zastanawiasz się jak ta `ID` wartość pochodzi. Firma Microsoft nie określono jego dowolne miejsce w naszej strony wzorcowej lub zawartości. Większość kontrolki serwera strony ASP.NET są dodawane jawnie za pośrednictwem strony deklaratywne znaczników. `MainContent` Formantu elementu ContentPlaceHolder jawnie został określony w znaczniku danego `Site.master`; `Age` pole tekstowe został zdefiniowany `IDIssues.aspx`dla znacznika. Można określić `ID` wartości dla tych typów formantów w oknie właściwości lub składni deklaratywnej. Inne formanty, takie jak strony wzorcowej, nie są zdefiniowane w znaczniku deklaratywne. W rezultacie ich `ID` wartości musi być generowane automatycznie dla nas. ASP.NET ustawia aparat `ID` wartości w czasie wykonywania dla tych kontrolek, których identyfikatory nie zostały jawnie ustawione. Używa wzorca nazewnictwa `ctlXX`, gdzie *XX* jest sekwencyjnie zwiększa wartość całkowitą.


Ponieważ wzorzec stronie służy jako kontener nazewnictwa, formantów sieci Web zdefiniowane na stronie głównej również zmieniono renderowanych `id` wartości atrybutów. Na przykład `DisplayDate` etykiety dodaliśmy strony wzorcowej w [ *tworzenie układu całej lokacji za pomocą stron wzorcowych* ](creating-a-site-wide-layout-using-master-pages-cs.md) samouczek zawiera następujące renderowany kod znaczników:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Należy pamiętać, że `id` atrybutu obejmuje zarówno strony wzorcowej `ID` wartość (`ctl00`) i `ID` wartość formantu etykiety w sieci Web (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Krok 3: Programowane odwołania do formantów sieci Web za pomocą`FindControl`

Kontrolka serwerowa ASP.NET, co obejmuje `FindControl("controlID")` metody, która wyszukuje elementów podrzędnych formantu dla formantu o nazwie *controlID*. Jeśli zostanie znaleziony taki formant, jest zwracany; Jeśli brak pasującego formantu zostanie znaleziony, `FindControl` zwraca `null`.

`FindControl`jest przydatne w sytuacji, gdy trzeba kontroli dostępu, ale nie masz bezpośrednie odwołanie do niej. Podczas pracy z danymi formantów sieci Web, takich jak na przykład GridView formantów w widoku GridView pola są definiowane raz w składni deklaratywnej, ale w czasie wykonywania jest tworzone wystąpienie formantu dla każdego wiersza w widoku GridView. W rezultacie istnieją formanty generowane w czasie wykonywania, ale nie ma bezpośredniego odwołania dostępne w klasie związanej z kodem. W związku z tym należy użyć `FindControl` programowo pracować z określonego formantu w obrębie pola w widoku GridView. (Aby uzyskać więcej informacji na temat używania `FindControl` Aby uzyskać dostęp do formantów w szablonach formantów sieci Web danych, zobacz [niestandardowe formatowanie oparte na danych](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) W tym samym scenariuszu występuje podczas dynamicznego dodawania formantów sieci Web do formularza sieci Web, temat omówione w [tworzenie dynamicznych danych wpisu interfejsy użytkownika](https://msdn.microsoft.com/library/aa479330.aspx).

Aby zilustrować przy użyciu `FindControl` metody do wyszukiwania formantów na stronie zawartości, utworzyć programu obsługi zdarzeń dla `SubmitButton`w `Click` zdarzeń. W obsłudze zdarzeń, Dodaj następujący kod, który odwołuje się programowo `Age` pole tekstowe i `Results` etykietę przy użyciu `FindControl` — metoda, a następnie wyświetla komunikat w `Results` oparte na danych wejściowych użytkownika.

> [!NOTE]
> Oczywiście nie należy używać `FindControl` do odwołują się do formantów etykiety i pola tekstowego, w tym przykładzie. Firma Microsoft może odwoływać się je bezpośrednio za pomocą ich `ID` wartości właściwości. Można użyć `FindControl` tutaj, aby zilustrować, co się stanie w przypadku korzystania z `FindControl` ze strony zawartość.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Podczas składnię wykorzystywaną do wywołania `FindControl` metoda różni się nieco pierwsze dwa wiersze `SubmitButton_Click`, są one semantycznie równoważne. Odwołania, który obejmuje wszystkich kontrolek serwera ASP.NET `FindControl` metody. Dotyczy to również `Page` klasy, z których wszystkie platformy ASP.NET klasy związane z kodem muszą pochodzić od. W związku z tym wywołaniem `FindControl("controlID")` jest odpowiednikiem wywołania `Page.FindControl("controlID")`, zakładając, że nie zostały zastąpione `FindControl` metody w klasie CodeBehind lub w niestandardowej klasy podstawowej.

Po wprowadzeniu tego kodu, odwiedź stronę `IDIssues.aspx` strony za pośrednictwem przeglądarki, wprowadź swój wiek, a następnie kliknij przycisk "Zatwierdź". Po kliknięciu przycisku "Zatwierdź" `NullReferenceException` jest wywoływane (patrz rysunek 5).


[![NullReferenceException jest wywoływane.](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Rysunek 05**: A `NullReferenceException` jest wywoływane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image9.png))


Jeśli ustawisz punkt przerwania `SubmitButton_Click` przekonasz się, że oba wywołań obsługi zdarzeń `FindControl` zwracać `null` wartość. `NullReferenceException` Jest wywoływane, gdy firma Microsoft próba uzyskania dostępu do `Age` w pole tekstowe `Text` właściwości.

Problem jest to, że `Control.FindControl` tylko wyszukuje *kontroli*jego elementów podrzędnych, które są *w tym samym kontenerze nazewnictwa*. Ponieważ strony wzorcowej stanowi nowy kontener nazewnictwa wywołanie `Page.FindControl("controlID")` nigdy nie permeates obiekt strony wzorcowej `ctl00`. (Odwołują się do 4 rysunek, aby wyświetlić hierarchii kontrolki, która zawiera `Page` obiektu jako element nadrzędny obiekt strony wzorcowej `ctl00`.) W związku z tym `Results` etykiety i `Age` nie znaleziono pola tekstowego i `ResultsLabel` i `AgeTextBox` są przypisane wartości `null`.

Istnieją dwa obejścia do tego żądania: Firma Microsoft może przejść do szczegółów jeden kontener nazewnictwa w czasie, aby właściwej opcji kontroli; lub można utworzyć własne `FindControl` metodę, która permeates kontenery nazewnictwa. Przeanalizujmy każdej z tych opcji.

### <a name="drilling-into-the-appropriate-naming-container"></a>Wchodzenia w kontenerze nazewnictwa odpowiednie

Aby użyć `FindControl` do odwołania `Results` etykiety lub `Age` pole tekstowe, należy wywołać `FindControl` z formantu nadrzędnego w tym samym kontenerze nazewnictwa. Jak pokazano na rysunku 4, `MainContent` ContentPlaceHolder formant jest jedynym elementem nadrzędnym `Results` lub `Age` to w ramach tego samego kontenera nazewnictwa. Innymi słowy, wywoływania `FindControl` metody z `MainContent` kontroli, jak pokazano poniżej, fragment kodu poprawnie zwraca odwołanie do `Results` lub `Age` kontrolki.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Jednak firma Microsoft nie może działać z `MainContent` ContentPlaceHolder z naszej strony zawartości klasę kodu przy użyciu powyższej składni, ponieważ zdefiniowano elementu ContentPlaceHolder na stronie głównej. Zamiast tego mamy użyj `FindControl` można pobrać odwołania do `MainContent`. Zastąp kod w `SubmitButton_Click` obsługi zdarzeń z następującymi zmianami:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

W przypadku odwiedzenia strony za pośrednictwem przeglądarki, wprowadź swój wiek i kliknij przycisk "Zatwierdź" `NullReferenceException` jest wywoływane. Jeśli ustawisz punkt przerwania `SubmitButton_Click` przekonasz się, że ten wyjątek występuje, gdy próba wywołania programu obsługi zdarzeń `MainContent` obiektu `FindControl` metody. `MainContent` Obiekt jest `null` ponieważ `FindControl` metody nie można zlokalizować obiektu o nazwie "Znacznika". Przyczyny źródłowej jest taka sama jak z `Results` etykiety i `Age` kontrolki TextBox: `FindControl` rozpoczyna się wyszukiwanie od góry hierarchii kontroli i nie spenetrowanie nazw kontenerów, ale `MainContent` jest element ContentPlaceHolder na stronie głównej, która jest kontenera nazewnictwa.

Przed możemy użyć `FindControl` można pobrać odwołania do `MainContent`, najpierw musimy odwołanie do formantu strony wzorcowej. Gdy będziemy mieć odwołanie do strony wzorcowej możemy pobrać odwołanie do `MainContent` ContentPlaceHolder za pośrednictwem `FindControl` i z tego miejsca, odwołuje się do `Results` etykiety i `Age` pola tekstowego (ponownie, za pośrednictwem przy użyciu `FindControl`). Jednak jak możemy pobrać odwołania do strony wzorcowej? Sprawdzając `id` atrybutów w renderowanego kodu znaczników jest oczywiste, które strony wzorcowej `ID` wartość jest `ctl00`. W związku z tym można używamy `Page.FindControl("ctl00")` Aby odwołać się do strony głównej, aby pobrać odwołanie do użyć obiektu `MainContent`i tak dalej. Poniższy fragment kodu przedstawia tę logikę:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Gdy ten kod będzie działać na pewno, jego przy założeniu, że wygenerowany automatycznie strony wzorcowej `ID` zawsze będzie `ctl00`. Nigdy nie jest dobrym pomysłem jest zakładają wartości wygenerowana automatycznie.

Na szczęście odwołania do strony wzorcowej jest dostępna za pośrednictwem `Page` klasy `Master` właściwości. Dlatego zamiast `FindControl("ctl00")` Aby pobrać odwołanie strony głównej w celu uzyskania dostępu do `MainContent` ContentPlaceHolder, możemy użyć `Page.Master.FindControl("MainContent")`. Aktualizacja `SubmitButton_Click` obsługi zdarzeń z następującym kodem:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Teraz, odwiedzając stronę za pośrednictwem przeglądarki, wprowadzić swój wiek, a następnie klikając przycisk "Zatwierdź" wyświetlany jest komunikat w `Results` etykietę, zgodnie z oczekiwaniami.


[![Wiek użytkownika jest wyświetlany w etykiecie](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Rysunek 06**: wiek użytkownika jest wyświetlany w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rekursywnie wyszukiwanie za pomocą nazw kontenerów

Przyczyna w poprzednim przykładzie kodu, do których odwołuje się `MainContent` formantu elementu ContentPlaceHolder na stronie głównej, a następnie `Results` etykiety i `Age` kontrolki pola tekstowego z `MainContent`, ponieważ `Control.FindControl` tylko wyszukuje — metoda w ramach *kontroli*kontener nazw obiektu. O `FindControl` Pozostań w kontenerze nazewnictwa ma sens w większości przypadków ponieważ dwóch formantów w dwóch różnych kontenerów nazewnictwa może mieć taki sam `ID` wartości. Należy wziąć pod uwagę w przypadku widoku GridView, który definiuje formantu etykiety sieci Web o nazwie `ProductName` w jednej z jego TemplateFields. Gdy dane jest powiązana z widoku GridView w czasie wykonywania, `ProductName` etykiety jest tworzony dla każdego wiersza w widoku GridView. Jeśli `FindControl` przeszukiwane przy użyciu nazw wszystkie kontenery i firma Microsoft o nazwie `Page.FindControl("ProductName")`, jakie wystąpienie etykieta powinna `FindControl` zwracać? `ProductName` Etykiet w pierwszym wierszu widoku GridView? Co w ostatnim wierszu?

Dlatego o `Control.FindControl` wyszukiwanie tylko *kontroli*jego nazwy kontenera ma sens w większości przypadków. Istnieją inne przypadkach, takich jak jeden skierowany us, gdy będziemy mieć unikatową `ID` we wszystkich nazwach kontenerów i aby uniknąć konieczności złudzenia odwołania każdego kontenera nazewnictwa w hierarchii formantu do kontroli dostępu. O `FindControl` variant, który zbyt rekursywnie wyszukuje sprawia, że wszystkie kontenery nazewnictwa znaczeniu. Niestety .NET Framework nie obejmuje taka metoda.

Szczęście jest, że firma Microsoft może tworzyć własne `FindControl` metoda tego rekursywnie wyszukuje wszystkie kontenery nazewnictwa. W rzeczywistości przy użyciu *metody rozszerzenia* firma Microsoft może kat `FindControlRecursive` metody `Control` klasy towarzyszące istniejące `FindControl` metody.

> [!NOTE]
> Metody rozszerzenia są funkcją nowych C# 3.0 i Visual Basic 9, które języki, które są dostarczane z .NET Framework w wersji 3.5 i Visual Studio 2008. Krótko mówiąc metody rozszerzenia umożliwiają deweloperom utworzenie nowej metody dla istniejącego typu klasy przy użyciu specjalnej składni. Aby uzyskać więcej informacji na temat tej funkcji przydatne odwoływać się do mojej artykułu [rozszerzanie funkcjonalności typ podstawowy z metody rozszerzenia](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Aby utworzyć metody rozszerzenia, Dodaj nowy plik do `App_Code` folder o nazwie `PageExtensionMethods.cs`. Dodaj metodę rozszerzenia o nazwie `FindControlRecursive` który przyjmuje jako dane wejściowe `string` parametru o nazwie `controlID`. Dla metody rozszerzenia działało poprawnie, jest niezbędne, można oznaczyć samej klasy i metody rozszerzenia `static`. Ponadto musisz zaakceptować wszystkie metody rozszerzenia, zgodnie z ich pierwszym parametrem obiekt typu, do którego jest stosowana metoda rozszerzenia, a ten parametr wejściowy musi być poprzedzony ze słowem kluczowym `this`.

Dodaj następujący kod do `PageExtensionMethods.cs` pliku klasy, aby określić tej klasy i `FindControlRecursive` — metoda rozszerzenia:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Z tego kodu w miejscu, wróć do `IDIssues.aspx` klasę kodu i komentarz dla bieżącej strony `FindControl` wywołania metody. Zamień wywołania `Page.FindControlRecursive("controlID")`. Porządek o metody rozszerzenia jest to, czy są wyświetlane bezpośrednio z poziomu listy rozwijane IntelliSense. Jak pokazano na rysunku 7, gdy użytkownik typ strony, a następnie naciśnij okres, `FindControlRecursive` metody jest uwzględniona w listy rozwijanej oraz innych funkcji IntelliSense `Control` metody klasy.


[![Metody rozszerzenia są uwzględniane w IntelliSense listy rozwijane](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Rysunek 07**: metody rozszerzenia są uwzględniane w IntelliSense listach rozwijanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image15.png))


Wprowadź następujący kod do `SubmitButton_Click` obsługi zdarzeń, a następnie przetestować, odwiedzając stronę, wprowadzając Twój wiek i klikając przycisk "Zatwierdź". Jak pokazano wstecz na rysunku 6, dane wyjściowe będzie wiadomości, "Są wieku lat!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Ponieważ metody rozszerzenia są nowe 3.0 C# i Visual Basic 9, jeśli używasz programu Visual Studio 2005 nie można używać metod rozszerzenia. Zamiast tego należy zaimplementować `FindControlRecursive` metodę w klasie pomocnika. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) zawiera takie przykład w jego wpis w blogu, [strony maserowy ASP.NET i `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Krok 4: Przy użyciu prawidłowego`id`atrybutu wartość skryptu po stronie klienta

Zgodnie z opisem w tym samouczku wprowadzenie, formantu sieci Web do renderowania `id` atrybutu jest często używane w skryptu po stronie klienta do programowego odwołuje się do określonego elementu HTML. Na przykład następujący kod JavaScript odwołuje się element HTML przez jego `id` , a następnie wyświetla jego wartość w polu komunikatów modalnych:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Odwołania, który w programie ASP.NET stron, które nie zawierają nazwy kontenera, renderowanego elementu HTML `id` atrybut jest taki sam jak formant sieci Web `ID` wartości właściwości. W związku z tym jest kuszące twardych kod w `id` wartości atrybutów do kodu JavaScript. Oznacza to, czy wiesz, ma dostęp do `Age` kontrolki TextBox w sieci Web za pomocą skryptu po stronie klienta to zrobić za pomocą wywołania `document.getElementById("Age")`.

Tego podejścia przy rozwiązywaniu problemu jest fakt, że za pomocą stron wzorcowych (lub innych mechanizmów kontroli kontenera nazewnictwa) HTML renderowanych `id` nie jest tożsame z formantu sieci Web `ID` właściwości. Twoje pierwsze nachylenia może być odwiedź stronę za pośrednictwem przeglądarki i Wyświetl źródło, aby określić rzeczywiste `id` atrybutu. Jeśli znasz już renderowanych `id` wartości, możesz wkleić go do wywołania `getElementById` na dostęp do elementu HTML, potrzebujesz do pracy z za pomocą skryptu po stronie klienta. Takie podejście jest mniejsza niż nadaje się doskonale, ponieważ pewnych zmian wprowadzonych na stronie kontrolować hierarchii lub zmiany w `ID` właściwości formantów nazewnictwa zmieni powstałe w ten sposób `id` atrybutu, co spowodowało uszkodzenie kodu JavaScript.

Dobre wieści jest to, że `id` wartość atrybutu, który jest renderowany jest dostępny w kodzie po stronie serwera za pomocą formantu sieci Web [ `ClientID` właściwości](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Należy używać tej właściwości, aby określić `id` atrybutu wartość używana przez skrypt po stronie klienta. Na przykład, aby dodać funkcję JavaScript do strony, która po wywołaniu, wyświetlana jest wartość `Age` TextBox w polu komunikatów modalnych, Dodaj następujący kod do `Page_Load` obsługi zdarzeń:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Powyższy kod injects wartość `Age` właściwości ClientID w pole tekstowe do wywołania języka JavaScript `getElementById`. Jeśli strony za pośrednictwem przeglądarki i wyświetlić źródło HTML, można znaleźć następujący kod JavaScript:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Powiadomienie jak poprawny `id` wartość atrybutu `ctl00_MainContent_Age`, pojawi się w wywołaniu `getElementById`. Ponieważ ta wartość jest obliczana w czasie wykonywania, działa niezależnie od późniejsze zmiany hierarchia formantów strony.

> [!NOTE]
> W tym przykładzie JavaScript jedynie przedstawiono sposób dodawania funkcji JavaScript, która poprawnie odwołuje się do elementu HTML renderowane przez kontrolki serwera. Aby użyć tej funkcji należy tworzyć dodatkowe JavaScript do wywołania tej funkcji, podczas ładowania dokumentu lub techniczną niektóre akcje określonego użytkownika. Aby uzyskać więcej informacji na temat tych i materiały pokrewne, przeczytaj [pracy przy użyciu skryptu po stronie klienta](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Podsumowanie

Niektórych kontrolek serwera ASP.NET działają jak kontenery nazewnictwa, co ma wpływ na renderowanych `id` atrybutu wartości ich formantów podrzędnych, a także zakres canvassed przez formanty `FindControl` metody. W odniesieniu do strony wzorcowe zarówno stronie wzorcowej i jego formantów elementu ContentPlaceHolder nadawania nazw kontenerów. W związku z tym, trzeba umieścić określonymi nieco więcej pracy programowo odwołują się do formantów w na stronie zawartości za pomocą `FindControl`. W tym samouczku będziemy zbadać dwie metody: wiertnicze do formantu elementu ContentPlaceHolder i wywoływania jego `FindControl` metoda; i stopniowych własnej `FindControl` implementacji tego rekursywnie wyszukuje za pośrednictwem wszystkich kontenerów nazewnictwa.

Oprócz problemów po stronie serwera, kontenery nazewnictwa wprowadzenie w odniesieniu do odwołujące się do formantów sieci Web są również problemy po stronie klienta. W przypadku braku nazw kontenerów, kontrolka sieci Web firmy `ID` wartość właściwości i renderowane `id` wartość atrybutu są w taki sam. Z dodatkową kontenera nazewnictwa renderowanych `id` atrybutu obejmuje zarówno `ID` wartości formantu sieci Web i nazewnictwa kontenerów w swojej hierarchii kontroli pochodzenia. Te problemy nazewnictwa są bez problemu, tak długo, jak używać formantu sieci Web `ClientID` właściwości w celu określenia renderowanych `id` atrybutu wartość za pomocą skryptu po stronie klienta.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Strony wzorcowe ASP.NET i`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Tworzenie interfejsów użytkownika wpisu danych dynamicznych](https://msdn.microsoft.com/library/aa479330.aspx)
- [Rozszerzanie funkcjonalności typu podstawowego z metody rozszerzenia](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Porady: Odwołania zawartość strony ASP.NET wzorca](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Sprawa strony: Porady, wskazówki i pułapek](http://www.odetocode.com/articles/450.aspx)
- [Praca z skryptu po stronie klienta](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Kowalski Zack i Suchi Barnerjee. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Poprzednie](urls-in-master-pages-cs.md)
[dalej](interacting-with-the-master-page-from-the-content-page-cs.md)
