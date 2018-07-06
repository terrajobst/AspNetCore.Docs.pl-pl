---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Kontrolowanie identyfikator nazewnictwa na stronach zawartości (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ilustruje sposób kontrolek ContentPlaceHolder służą jako kontener nazewnictwa i w związku z tym upewnij się, programowo Praca z formantem trudne (za pośrednictwem FindConrol)...
ms.author: aspnetcontent
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b7ae15095b972a456f274e1326221d168f323256
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805878"
---
<a name="control-id-naming-in-content-pages-c"></a>Nazewnictwo identyfikatorów kontrolek na stronach zawartości (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Ilustruje sposób kontrolek ContentPlaceHolder służą jako kontener nazewnictwa i w związku z tym upewnij się, programowo Praca z formantem trudne (za pośrednictwem FindConrol). Analizuje tego problemu i jego rozwiązania. Omówiono również sposób programowego dostępu wynikowej wartości identyfikatora klienta.


## <a name="introduction"></a>Wprowadzenie

Obejmują wszystkie formanty serwera ASP.NET `ID` właściwość, która jednoznacznie identyfikuje formant, a to oznacza, że za pomocą którego kontrolki programowo odbywa się w klasie CodeBehind. Podobnie, może zawierać elementów w dokumencie HTML `id` atrybut, który unikatowo identyfikuje element; te `id` wartości są często używane w skrypcie po stronie klienta do programowego odwołuje się do określonego elementu HTML. Biorąc pod uwagę to, użytkownik może przyjęto założenie, że podczas renderowania do formatu HTML, formant serwera ASP.NET jego `ID` wartość jest używana jako `id` wartość renderowanego elementu HTML. Nie jest tak, ponieważ w pewnych okolicznościach pojedynczy kontrolować za pomocą jednego `ID` wartości mogą pojawić się wiele razy w renderowanego kodu znaczników. Należy wziąć pod uwagę obejmującą TemplateField za pomocą kontrolki etykiety w sieci Web przy użyciu kontrolki GridView `ID` wartość ProductName. Widoku GridView jest powiązany z innym źródłem danych w czasie wykonywania, ta etykieta jest powtarzany jeden raz dla każdego wiersza w widoku GridView. Każdy renderowane potrzeb etykiety unikatową `id` wartość.

Do obsługi takich scenariuszy, ASP.NET umożliwia pewnych formantów zostać oznaczone jako nazewnictwa kontenerów. Kontener nazewnictwa stanowi nową `ID` przestrzeni nazw. Wszystkie formanty serwera, które pojawiają się w kontenerze nazewnictwa ma ich renderowanych `id` wartość prefiks `ID` kontrolki kontenera nazewnictwa. Na przykład `GridView` i `GridViewRow` klasy są oba nazewnictwa kontenerów. W związku z tym, formant etykiety zdefiniowane w GridView TemplateField z `ID` ProductName otrzymuje renderowanych `id` wartość `GridViewID_GridViewRowID_ProductName`. Ponieważ *GridViewRowID* jest unikatowy dla każdego wiersza w widoku GridView, wynikowy `id` wartości są unikatowe.

> [!NOTE]
> [ `INamingContainer` Interfejsu](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) jest używany do wskazania, że określonego formant serwera ASP.NET powinien działać jako kontenera nazewnictwa. `INamingContainer` Interfejsu nie zawierają bardziej dowolnej metody, które należy zaimplementować formant serwera; zamiast jest używany jako znacznik. Podczas generowania renderowanego kodu znaczników, jeśli kontrolka implementuje ten interfejs następnie aparatu ASP.NET automatycznie prefiksy jego `ID` renderowane wartość do jego elementów potomnych `id` wartości atrybutów. Ten proces jest omówiona bardziej szczegółowo w kroku 2.


Nazewnictwa kontenerów nie należy zmieniać tylko renderowanych `id` wartość atrybutu, ale również wpływa na sposób kontrolki odwołania mogą dotyczyć programowo z klasy CodeBehind strony ASP.NET. `FindControl("controlID")` Metoda jest najczęściej używany do programowego przywołać kontrolki sieci Web. Jednak `FindControl` nie przechodzić przez za pośrednictwem nazewnictwa kontenerów. W związku z tym, nie możesz bezpośrednio użyć `Page.FindControl` metodę, aby odwoływać się do kontrolki GridView lub innego kontenera nazewnictwa.

Ponieważ użytkownik może mieć surmised, stron wzorcowych i kontrolek ContentPlaceHolder są zarówno implementowane jako nazewnictwa kontenerów. W tym samouczku sprawdzamy, jak głównego elementu HTML mogą wpłynąć na stronach `id` wartości i informacje o tym, jak programowo odwołują się do formantów sieci Web w obrębie strony zawartości za pomocą `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Krok 1: Dodawanie nowej strony programu ASP.NET

Aby zademonstrować kwestie omówione w tym samouczku, Dodajmy nowej strony programu ASP.NET do naszej witryny sieci Web. Utwórz nową stronę zawartości o nazwie `IDIssues.aspx` w folderze głównym wiążące go do `Site.master` strony wzorcowej.


![Dodawanie zawartości IDIssues.aspx strony do folderu głównego](control-id-naming-in-content-pages-cs/_static/image1.png)

**Rysunek 01**: Dodawanie strony zawartości `IDIssues.aspx` do folderu głównego


Visual Studio automatycznie tworzy kontrolkę zawartości dla każdego z czterech kontrolek ContentPlaceHolder strony wzorcowej. Jak wspomniano w [ *wiele kontrolek ContentPlaceHolder i zawartość domyślna* ](multiple-contentplaceholders-and-default-content-cs.md) samouczków, jeśli nie ma zawartości kontrolki zawartości ContentPlaceHolder domyślnej strony wzorcowej jest emitowane zamiast tego. Ponieważ `QuickLoginUI` i `LeftColumnContent` kontrolek ContentPlaceHolder zawierają znaczników domyślne odpowiednie dla tej strony, przejdź dalej i usuń odpowiednie kontrolki zawartości z `IDIssues.aspx`. W tym momencie oznaczeniu deklaracyjnym strony zawartości powinien wyglądać następująco:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

W [ *Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) samouczku utworzyliśmy klasy niestandardowej strony podstawowej (`BasePage`), zostaną automatycznie skonfigurowane tytuł strony po nie jest jawnie ustawione. Aby uzyskać `IDIssues.aspx` strony mogą wykorzystać tę funkcję, strony osobna klasa kodu musi pochodzić od klasy `BasePage` klasy (zamiast `System.Web.UI.Page`). Zmodyfikuj definicję klasy związane z kodem, tak, aby go wygląda podobnie do poniższego:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Na koniec zaktualizuj `Web.sitemap` plik, aby dołączyć wpis dla tej lekcji nowe. Dodaj `<siteMapNode>` element i ustaw jego `title` i `url` atrybuty dla "Kontrolki identyfikator nazewnictwa problemy z" i `~/IDIssues.aspx`, odpowiednio. Po wprowadzeniu to dodanie Twojej `Web.sitemap` pliku znaczników powinien wyglądać podobnie do poniższego:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Tak jak pokazano na rysunku 2, nowy wpis mapy witryny w `Web.sitemap` jest natychmiast odzwierciedlana w sekcji lekcje w lewej kolumnie.


![Sekcja lekcje zawiera teraz łącza do &quot;nazewnictwa problemów identyfikator formantu&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Rysunek 02**: sekcja lekcje teraz łącze umożliwiające "Problemy z nazewnictwo identyfikatorów kontrolek"


## <a name="step-2-examining-the-renderedidchanges"></a>Krok 2: Badanie renderowanych`ID`zmiany

Aby lepiej zrozumieć modyfikacje ASP.NET aparatowi do renderowanej `id` kontroluje wartości serwera, dodamy kilka formantów sieci Web do `IDIssues.aspx` strony, a następnie Wyświetl renderowanego kodu znaczników, wysyłany do przeglądarki. Ściślej mówiąc, wpisz tekst ", wprowadź swój wiek:" następuje formantu sieci Web w polu tekstowym. Więcej szczegółów na stronie Dodaj formant przycisku w sieci Web i formant etykiety w sieci Web. Ustaw pole tekstowe `ID` i `Columns` właściwości `Age` i 3, odpowiednio. Ustaw właściwość `Text` i `ID` właściwości "Prześlij" i `SubmitButton`. Czyści etykiety `Text` właściwości i ustaw jego `ID` do `Results`.

W tym momencie oznaczeniu deklaracyjnym formantu zawartości powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Rysunek 3 przedstawia stronę oglądany przez projektanta programu Visual Studio.


[![Strona zawiera trzy kontrolki sieci Web: pole tekstowe, przycisków i etykiet](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Rysunek 03**: strona zawiera trzy kontrolki sieci Web: pole tekstowe, przycisków i etykiet ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image5.png))


Odwiedź stronę za pośrednictwem przeglądarki, a następnie Wyświetl źródło HTML. Jako kod znaczników, poniżej przedstawiono `id` wartości elementów kodu HTML dla formantów pola tekstowego, przycisk i etykietę w sieci Web są kombinacją `ID` wartości kontrolki sieci Web i `ID` wartości nazewnictwa kontenerów na stronie.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Jak wspomniano wcześniej w tym samouczku, zarówno strony wzorcowej, jak i jej kontrolek ContentPlaceHolder pełnić rolę nazewnictwa kontenerów. W związku z tym, zarówno współtworzyć renderowanych `ID` wartości ich zagnieżdżonych formantów. Pole tekstowe zająć `id` atrybut, na przykład: `ctl00_MainContent_Age`. Pamiętaj, że formant pola tekstowego `ID` wartość była `Age`. To jest poprzedzony znakiem do jego kontrolki ContentPlaceHolder `ID` wartość `MainContent`. Ponadto, ta wartość jest poprzedzony ze stroną wzorcową `ID` wartość `ctl00`. Efektem sieciowym jest `id` wartość atrybutu składający się z `ID` wartości strony wzorcowej, kontrola ContentPlaceHolder i pole tekstowe, sam.

Rysunek 4 przedstawia tego zachowania. Aby określić renderowanych `id` z `Age` pola tekstowego, rozpoczyna się od `ID` wartość formant pola tekstowego `Age`. Następnie sposobu pracy użytkownika w hierarchii kontroli. Na każdy kontener nazewnictwa (te węzły kolorem brzoskwini), prefiks bieżącego renderowane `id` za pomocą kontenera nazewnictwa `id`.


![Atrybuty identyfikatora Rendered są oparte na wartości Identyfikatora nazewnictwa kontenerów](control-id-naming-in-content-pages-cs/_static/image6.png)

**Rysunek 04**: renderowane `id` atrybuty są na podstawie `ID` wartości nazewnictwa kontenerów


> [!NOTE]
> Tak jak Omówiliśmy, `ctl00` część renderowanych `id` stanowi atrybutu `ID` wartość strony wzorcowej, ale być może zastanawiasz się jak ta `ID` wartość pochodzi. Firma Microsoft nie określa go dowolnym miejscu w naszym głównym lub zawartości na stronie. Większość formantów serwera na stronie ASP.NET są jawnie dodawane w oznaczeniu deklaracyjnym strony. `MainContent` Jawnie określono kontrolkę ContentPlaceHolder w znaczniku elementu `Site.master`; `Age` pola tekstowego została zdefiniowana `IDIssues.aspx`firmy znaczników. Można określić `ID` wartości dla tych typów formantów w oknie właściwości lub składni deklaratywnej. Inne formanty, takie jak strony wzorcowej, nie są zdefiniowane w oznaczeniu deklaracyjnym. W związku z tym ich `ID` wartości muszą być generowane automatycznie dla nas. ASP.NET ustawia aparat `ID` wartości w czasie wykonywania dla tych kontrolek, których identyfikatory nie zostały jawnie ustawione. Używa wzorca nazewnictwa `ctlXX`, gdzie *XX* jest sekwencyjnie zwiększa wartość całkowitą.


Ponieważ wzorzec stronie służy jako kontener nazewnictwa, kontrolki sieci Web zdefiniowany w stronę wzorcową również zmieniają się renderowany `id` wartości atrybutów. Na przykład `DisplayDate` etykiety dodaliśmy strony wzorcowej w [ *tworzenie układu dla całej witryny za pomocą stron wzorcowych* ](creating-a-site-wide-layout-using-master-pages-cs.md) samouczek ma renderowania kodu znaczników:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Należy pamiętać, że `id` atrybut zawiera zarówno strony wzorcowej firmy `ID` wartość (`ctl00`) i `ID` wartość kontrolki etykiety w sieci Web (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Krok 3: Programowe odwołanie do kontrolki sieci Web za pośrednictwem`FindControl`

Każdy formant serwera ASP.NET zawiera `FindControl("controlID")` metodę, która wyszukuje elementy potomne kontrolki, dla formantu o nazwie *controlID*. Jeśli zostanie znaleziony taki formant, jest zwracana; Jeśli nie może kontrolować dopasowania zostanie znaleziony, `FindControl` zwraca `null`.

`FindControl` jest przydatne w sytuacjach, gdy trzeba kontroli dostępu, ale nie ma bezpośredniego odwołania do niego. Podczas pracy z danymi kontrolki sieci Web, takich jak GridView, na przykład kontrolek w obrębie pola GridView zdefiniowano jeden raz w składni deklaratywnej, ale w czasie wykonywania jest tworzone wystąpienie kontrolki, dla każdego wiersza w widoku GridView. W związku z tym istnieją środki generowane w czasie wykonywania, ale nie ma bezpośredniego odwołania dostępne od klasy CodeBehind. Dlatego musimy użyć `FindControl` Aby programowo pracować z określonej kontrolki GridView pola. (Aby uzyskać więcej informacji na temat korzystania z `FindControl` Aby uzyskać dostęp do formantów w szablonach formantów sieci Web danych, zobacz [niestandardowe formatowanie oparte na danych](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) W tym scenariuszu tej samej sytuacji: dynamiczne dodawanie formantów sieci Web do formularza sieci Web, temat omówione w [tworzenie dynamicznych danych wpis interfejsy użytkownika](https://msdn.microsoft.com/library/aa479330.aspx).

Aby zilustrować, za pomocą `FindControl` metoda ma szukać kontrolek w obrębie strony zawartości, utworzyć program obsługi zdarzeń dla `SubmitButton`firmy `Click` zdarzeń. W procedurze obsługi zdarzeń Dodaj następujący kod, który odwołuje się do programowego `Age` pole tekstowe i `Results` etykiety za pomocą `FindControl` metody, a następnie wyświetla komunikat w `Results` na podstawie danych wejściowych użytkownika.

> [!NOTE]
> Oczywiście nie trzeba używać `FindControl` do odwołują się do formantów etykiet i pole tekstowe, w tym przykładzie. Firma Microsoft może odwoływać się do nich bezpośrednio za pośrednictwem ich `ID` wartości właściwości. Czy mogę używać `FindControl` tutaj, aby zilustrować, co się dzieje w przypadku korzystania z `FindControl` ze strony zawartość.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Składnia używana do wywołania podczas `FindControl` metoda różni się nieco pierwsze dwa wiersze `SubmitButton_Click`, są semantycznie równoważne. Odwołania, który zawiera wszystkie formanty serwera ASP.NET `FindControl` metody. Obejmuje to `Page` klasy, z których wszystkie platformy ASP.NET klasy CodeBehind muszą pochodzić od. Dlatego wywołanie `FindControl("controlID")` jest równoważne z wywoływaniem `Page.FindControl("controlID")`, zakładając, że nie zostały zastąpione `FindControl` metodę w klasie CodeBehind lub w niestandardowej klasy bazowej.

Po wprowadzeniu tego kodu, odwiedź stronę `IDIssues.aspx` stronie za pośrednictwem przeglądarki, wprowadź swój wiek, a następnie kliknij przycisk "Przekaż". Po kliknięciu przycisku "Prześlij" `NullReferenceException` jest wywoływane (zobacz rysunek 5).


[![Obiektu NullReferenceException jest wywoływane.](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Rysunek 05**: A `NullReferenceException` jest wywoływane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image9.png))


Jeśli ustawisz punkt przerwania w `SubmitButton_Click` programu obsługi zdarzeń, zobaczysz, że oba wywołania `FindControl` zwracają `null` wartość. `NullReferenceException` Jest zgłaszane w przypadku próby uzyskania dostępu `Age` pola `Text` właściwości.

Problemu jest to, że `Control.FindControl` przeszukuje tylko *kontroli*jego elementy potomne, które są *w tym samym kontenerze nazewnictwa*. Ponieważ strony wzorcowej stanowi nowy kontener nazewnictwa wywołanie `Page.FindControl("controlID")` nigdy nie permeates obiekt strony wzorcowej `ctl00`. (Zobacz rysunek 4, aby wyświetlić hierarchii kontroli, który pokazuje `Page` obiektów jako nadrzędny obiekt strony wzorcowej `ctl00`.) W związku z tym `Results` etykiety i `Age` nie znaleziono pola tekstowego i `ResultsLabel` i `AgeTextBox` przypisanych wartości `null`.

Istnieją dwa obejścia problemu do tego wyzwania: Firma Microsoft może przejść do szczegółów jednego kontenera nazewnictwa naraz, właściwej opcji kontroli; lub możesz stworzyć własną `FindControl` metodę, która permeates nazewnictwa kontenerów. Przeanalizujmy każdej z tych opcji.

### <a name="drilling-into-the-appropriate-naming-container"></a>Przechodzenie do szczegółów do odpowiedniej nazwy kontenera

Do użycia `FindControl` do odwołania `Results` etykiety lub `Age` pole tekstowe, należy wywołać `FindControl` z elementu nadrzędnego kontrolki w tym samym kontenerze nazewnictwa. Jak rysunek 4 wykazało, `MainContent` ContentPlaceHolder formant jest tylko element nadrzędny elementu `Results` lub `Age` to w ramach tego samego kontenera nazewnictwa. Innymi słowy, wywołanie `FindControl` metody z `MainContent` kontroli, jak pokazano w poniższym fragmencie kodu prawidłowo zwraca odwołanie do `Results` lub `Age` kontrolki.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Jednakże, nie będziemy pracować z `MainContent` ContentPlaceHolder z naszej strony zawartości osobna klasa kodu przy użyciu powyższej składni, ponieważ ContentPlaceHolder jest zdefiniowana na stronie głównej. Zamiast tego, musimy użyć `FindControl` można pobrać odwołania do `MainContent`. Zastąp kod w `SubmitButton_Click` programu obsługi zdarzeń z następującymi zmianami:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

W przypadku odwiedzenia strony za pośrednictwem przeglądarki, wprowadź swój wiek i kliknij przycisk "Prześlij" `NullReferenceException` jest wywoływane. Jeśli ustawisz punkt przerwania w `SubmitButton_Click` programu obsługi zdarzeń, zobaczysz, że ten wyjątek występuje podczas próby wywołania `MainContent` obiektu `FindControl` metody. `MainContent` Obiekt jest `null` ponieważ `FindControl` metody nie można zlokalizować obiektu o nazwie "MainContent". Główną przyczynę wysokiej skuteczności jest taka sama jak za pomocą `Results` etykiety i `Age` kontrolki TextBox: `FindControl` rozpoczyna się wyszukiwanie od początku hierarchii kontroli, a nie przechodzić przez nazewnictwa kontenerów, ale `MainContent` jest ContentPlaceHolder w obrębie strony wzorcowej, czyli kontenera nazewnictwa.

Firma Microsoft może korzystać z `FindControl` można pobrać odwołania do `MainContent`, najpierw należy odwołanie do formantu strony wzorcowej. Gdy będziemy już mieć odwołania do strony wzorcowej firma Microsoft może odwołać się do `MainContent` ContentPlaceHolder za pośrednictwem `FindControl` i z tego miejsca odwołania do `Results` etykiety i `Age` pola tekstowego (ponownie za pomocą `FindControl`). Ale jak możemy uzyskać odwołanie do strony głównej? Sprawdzając `id` atrybutów w renderowanego kodu znaczników jest oczywiste, że strony wzorcowej `ID` wartość jest `ctl00`. W związku z tym, moglibyśmy użyć `Page.FindControl("ctl00")` Aby odwołać się do strony głównej, a następnie użyj tego obiektu można pobrać odwołania do `MainContent`i tak dalej. Poniższy fragment kodu ilustruje tę logikę:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Podczas, gdy ten kod bez obaw będzie działać, przyjęto założenie, że wygenerowany automatycznie strony wzorcowej `ID` zawsze będzie `ctl00`. Nigdy nie jest dobry pomysł, aby wprowadzić założeń o wartościach wygenerowany automatycznie.

Na szczęście odwołania do strony wzorcowej jest dostępny za pośrednictwem `Page` klasy `Master` właściwości. Dlatego zamiast `FindControl("ctl00")` można pobrać odwołania do strony wzorcowej, aby uzyskać dostęp do `MainContent` ContentPlaceHolder, można zamiast tego użyć `Page.Master.FindControl("MainContent")`. Aktualizacja `SubmitButton_Click` programu obsługi zdarzeń z następującym kodem:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Tym razem, odwiedzając stronę za pośrednictwem przeglądarki sieci, wprowadzając Twój wiek, a następnie klikając przycisk "Prześlij", wyświetla komunikat w `Results` etykiety, zgodnie z oczekiwaniami.


[![Wieku użytkownika jest wyświetlany w etykiecie](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Rysunek 06**: wieku użytkownika jest wyświetlany w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Rekursywnie przeszukiwania nazewnictwa kontenerów

Przyczyna poprzedniego przykładu kodu, do których odwołuje się `MainContent` ContentPlaceHolder kontroli ze strony głównej, a następnie `Results` etykiety i `Age` formanty TextBox z `MainContent`, ponieważ `Control.FindControl` przeszukuje tylko co — metoda w ramach *kontroli*nazewnictwa kontenerów. Posiadanie `FindControl` pobytu w ramach kontenera nazewnictwa sens, w większości przypadków ponieważ dwóch kontrolek w dwóch różnych kontenerów nazewnictwa może mieć taką samą `ID` wartości. Rozważmy przypadek GridView, który definiuje formant etykiety w sieci Web o nazwie `ProductName` w jednej z jej kontrolek TemplateField. Gdy dane jest powiązana z GridView w czasie wykonywania, `ProductName` zostaje utworzone etykieta dla każdego wiersza w widoku GridView. Jeśli `FindControl` przeszukiwane za pośrednictwem wszystkich nazw i kontenery wywoływane `Page.FindControl("ProductName")`, jakie wystąpienie etykieta powinna `FindControl` zwracają? `ProductName` Etykiet w pierwszym wierszu GridView? Co w ostatnim wierszu?

Dlatego po `Control.FindControl` wyszukiwanie tylko *kontroli*nazewnictwa kontenera ma sens w większości przypadków. Ale w innych przypadkach, takich jak jednego połączonego z nami, gdy będziemy mieć unikatową `ID` we wszystkich nazewnictwa kontenerów i chcesz uniknąć konieczności lubiane odwoływać się do każdego kontenera nazewnictwa w hierarchii kontroli do kontroli dostępu. Posiadanie `FindControl` wariant, który zbyt sens rekursywnie wyszukuje sprawia, że wszystkie kontenery nazewnictwa. Niestety .NET Framework nie ma taką metodę.

Dobra wiadomość jest taka, możemy utworzyć własną `FindControl` metoda tym rekursywnie wyszukuje wszystkie kontenery nazewnictwa. W rzeczywistości przy użyciu *metody rozszerzenia* firma Microsoft może kat `FindControlRecursive` metody `Control` klasy, która ma towarzyszyć istniejące `FindControl` metody.

> [!NOTE]
> Metody rozszerzenia są funkcją nowego języka C# 3.0 i 9 Visual Basic, które są to języki, które są dostarczane z .NET Framework w wersji 3.5 i Visual Studio 2008. Krótko mówiąc metody rozszerzające umożliwiają deweloperem utworzyć nowe metody istniejącego typu klasy przy użyciu specjalnej składni. Aby uzyskać więcej informacji na temat tej funkcji pomocnych się Moje artykułem [rozszerzanie funkcjonalności typ podstawowy za pomocą metody rozszerzenia](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Aby utworzyć metodę rozszerzenia, Dodaj nowy plik do `App_Code` folder o nazwie `PageExtensionMethods.cs`. Dodaj metodę rozszerzenia o nazwie `FindControlRecursive` która przyjmuje jako dane wejściowe `string` parametr o nazwie `controlID`. Dla metody rozszerzenia zapewnić prawidłowe działanie, koniecznie samej klasy i jego metod rozszerzenia oznaczane `static`. Ponadto, należy zaakceptować wszystkie metody rozszerzenia, zgodnie z ich pierwszy parametr, obiekt tego typu, do której stosują się metody rozszerzenia, a ten parametr wejściowy musi być poprzedzona słowem kluczowym `this`.

Dodaj następujący kod do `PageExtensionMethods.cs` plik klasy do definiowania tej klasy i `FindControlRecursive` — metoda rozszerzenia:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Przy użyciu tego kodu w miejscu, wróć do `IDIssues.aspx` z kodem klasę i komentarz dla bieżącej strony `FindControl` wywołania metody. Zastąp je wywołaniami do `Page.FindControlRecursive("controlID")`. Ładnie dotyczących metod rozszerzających jest to, że pojawiają się bezpośrednio z poziomu listy rozwijane IntelliSense. Jak pokazano na rysunku 7, po wpisaniu strony i okres, kliknij przycisk `FindControlRecursive` metoda znajduje się w rozwijanej wraz z innych funkcji IntelliSense `Control` metody klasy.


[![Metody rozszerzenia są uwzględnione w funkcji IntelliSense rozwijanych](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Rysunek 07**: metody rozszerzenia są uwzględnione w funkcji IntelliSense rozwijanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](control-id-naming-in-content-pages-cs/_static/image15.png))


Wprowadź następujący kod do `SubmitButton_Click` program obsługi zdarzeń, a następnie przetestować, odwiedzając stronę, wprowadzając Twój wiek i klikając przycisk "Przekaż". Pokazany ponownie na rysunku 6, dane wyjściowe będą komunikat, "Są wiek lat!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Ponieważ jesteś nowym użytkownikiem języka C# 3.0 i 9 Visual Basic, jeśli używasz programu Visual Studio 2005 metody rozszerzenia nie można używać metod rozszerzenia. Zamiast tego musisz zaimplementować `FindControlRecursive` metodę w klasie pomocnika. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) ma takie przykładem w jego wpis w blogu [strony maserowy ASP.NET i `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Krok 4: Przy użyciu prawidłowego`id`atrybutu wartości w skryptu po stronie klienta

Jak wspomniano w tym samouczku wprowadzenia, firmy renderowania formantu sieci Web `id` atrybut jest często używany w skrypcie po stronie klienta do programowo odwołuje się do określonego elementu HTML. Na przykład następujący kod JavaScript odwołuje się element HTML, jego `id` , a następnie wyświetla jego wartość w polu modalne komunikatu:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Odwołania, że w programie ASP.NET stron, które nie zawierają nazw kontenera, renderowanego elementu HTML `id` atrybutu jest taka sama jak kontrolki sieci Web `ID` wartości właściwości. W związku z tym jest kuszące do twarde kodu w `id` wartości atrybutów w kodzie JavaScript. Oznacza to, jeśli znasz chcesz uzyskać dostęp do `Age` formantu sieci Web w polu tekstowym za pomocą skryptu po stronie klienta, to zrobić za pomocą wywołania `document.getElementById("Age")`.

Problem z tym podejściem jest fakt, że za pomocą stron wzorcowych (lub innych nazw kontrole kontenerów), kod HTML renderowany `id` nie jest równoznaczny z formantu sieci Web `ID` właściwości. Twoje pierwsze nachylenie może być do odwiedzenia strony za pośrednictwem przeglądarki i Wyświetl źródło w celu określenia rzeczywistego `id` atrybutu. Jeśli znasz już renderowanych `id` wartości, możesz wkleić go do wywołania `getElementById` umożliwiają dostęp do elementu HTML, musisz pracować za pośrednictwem skryptu po stronie klienta. Ta metoda jest mniej niż idealne rozwiązanie, ponieważ pewne zmiany na stronę kontroli hierarchii lub zmiany `ID` właściwości formantów nazewnictwa zmieni wartość wynikowa `id` atrybut, co istotne kodu JavaScript.

Dobra wiadomość jest fakt, że `id` wartość atrybutu, który jest renderowany jest dostępny w kodzie po stronie serwera za pomocą kontrolki sieci Web [ `ClientID` właściwość](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Należy używać tej właściwości, aby określić `id` atrybutu wartość używana przez skrypt po stronie klienta. Na przykład, aby dodać funkcję JavaScript do strony, gdy zostanie wywołana, wyświetlana jest wartość `Age` pole tekstowe w oknie komunikatu modalne, Dodaj następujący kod do `Page_Load` program obsługi zdarzeń:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Powyższy kod wprowadza wartość `Age` w polu tekstowym właściwości identyfikatora klienta do wywołania języka JavaScript, aby `getElementById`. Jeśli tę stronę za pośrednictwem przeglądarki i wyświetlić źródło HTML, można znaleźć następujący kod JavaScript:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Zwróć uwagę jak poprawny `id` wartość atrybutu, `ctl00_MainContent_Age`, pojawi się w wywołaniu `getElementById`. Ponieważ ta wartość jest obliczana w czasie wykonywania, działa niezależnie od tego, późniejsze zmiany hierarchii kontroli strony.

> [!NOTE]
> W tym przykładzie JavaScript jedynie przedstawiono sposób dodawania funkcji JavaScript, która poprawnie odwołuje się do elementu HTML renderowany przez formant serwera. Aby użyć tej funkcji należy tworzyć dodatkowe JavaScript do wywołania funkcji podczas ładowania dokumentu lub techniczną niektóre akcje określonego użytkownika. Aby uzyskać więcej informacji na ten temat i materiały pokrewne, przeczytaj [pracy za pomocą skryptu po stronie klienta](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Podsumowanie

Niektóre formanty serwera ASP.NET działają jak kontenery nazewnictwa, co ma wpływ na renderowanych `id` atrybutu wartości swoich formantów podrzędnych, a także zakres formantów canvassed przez `FindControl` metody. W odniesieniu do stron wzorcowych zarówno strony wzorcowej, sam, jak i jej kontrolek ContentPlaceHolder są nazewnictwa kontenerów. W związku z tym, trzeba umieścić ogłoszonym trochę więcej pracy programowo odwołują się do formantów w obrębie strony zawartości, za pomocą `FindControl`. W tym samouczku zbadaliśmy dwie techniki: przechodzenia do szczegółów w formancie ContentPlaceHolder i wywoływania jego `FindControl` metody i wycofania własną `FindControl` implementacji tego rekursywnie wyszukuje za pośrednictwem wszystkich nazewnictwa kontenerów.

Oprócz problemów po stronie serwera, wprowadzenie nazewnictwa kontenerów w odniesieniu do odwołuje się do formantów sieci Web są również problemy po stronie klienta. W przypadku braku nazewnictwa kontenerów, kontrolki sieci Web firmy `ID` wartość właściwości renderowana `id` wartość atrybutu są w taki sam. Z dodatkową kontenera nazewnictwa renderowanych `id` atrybut zawiera zarówno `ID` wartości kontrolki sieci Web i nazewnictwa kontenerów w jego hierarchii kontroli pochodzenie. Te rozważania nazewnictwa są inne niż problem, tak długo, jak używać kontrolki sieci Web `ClientID` właściwości w celu określenia renderowanych `id` atrybutu wartości za pomocą skryptu po stronie klienta.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Stron wzorcowych platformy ASP.NET i `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Tworzenie interfejsów użytkownika wprowadzania danych dynamicznych](https://msdn.microsoft.com/library/aa479330.aspx)
- [Rozszerzanie funkcjonalności typu podstawowego, za pomocą metody rozszerzenia](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Instrukcje: Odwołania zawartość strony wzorca ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Sprawa strony: Porady, wskazówki i pułapek](http://www.odetocode.com/articles/450.aspx)
- [Praca z skryptu po stronie klienta](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Zack Jones i Suchi Barnerjee. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](urls-in-master-pages-cs.md)
> [dalej](interacting-with-the-master-page-from-the-content-page-cs.md)
