---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: "Dodawanie kolumny widoku GridView przycisków radiowych (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku analizuje jak dodać kolumnę przycisków radiowych do kontrolki widoku siatki, aby przyznać użytkownikowi bardziej intuicyjne sposób wybierania pojedynczy wiersz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: a9a470efc9e9416cd06fd4268f4e9505393dbed3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Dodawanie przycisków radiowych (VB) kolumny widoku GridView
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) lub [pobierania plików PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> W tym samouczku analizuje jak dodać kolumnę przycisków radiowych do kontrolki widoku siatki, aby przyznać użytkownikowi bardziej intuicyjne sposób wybierania pojedynczy wiersz GridView.


## <a name="introduction"></a>Wprowadzenie

Formant widoku GridView oferuje wiele wbudowanych funkcji. Zawiera szereg różnych pól do wyświetlania tekstu, obrazów, hiperłączy i przycisków. Obsługuje ona szablonów dla dalszego dostosowania. Za pomocą kilku kliknięć myszą jego s może być GridView, w którym można wybrać każdego wiersza za pośrednictwem przycisku lub umożliwienie edytowania lub usuwania możliwości. Pomimo nadmiarem podana funkcje często będzie sytuacje, w których dodatkowe funkcje nieobsługiwane będzie musi zostać dodany. W tym samouczku i dwie zajmiemy się, jak zwiększyć funkcjonalność s GridView dołączenie dodatkowych funkcji.

W tym samouczku i kolejnego skupić się na Udoskonalanie procesu zaznaczenie wiersza. Jak zbadać [wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), CommandField można dodać do widoku GridView, która zawiera przycisk Wybierz. Po kliknięciu odświeżania strony ensues i GridView s `SelectedIndex` właściwości zostaną zmienione na indeks wiersza, którego wybierz przycisk został kliknięty. W [wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) samouczka widzieliśmy sposób użyć tej funkcji, aby wyświetlić szczegóły wybranego wiersza elementu GridView.

Gdy przycisku Wybierz działa w wielu sytuacjach, może nie działać również dla innych użytkowników. Zamiast za pomocą przycisku, dwa inne elementy interfejsu użytkownika są często używane do wyboru: przycisku radiowego i pól wyboru. Firma Microsoft może rozszerzyć widoku GridView tak, aby zamiast przycisku Wybierz, każdy wiersz zawiera przycisk radiowy lub pole wyboru. W scenariuszach, w którym użytkownika można wybrać tylko jeden z rekordów widoku GridView przycisku radiowego może mieć pierwszeństwo przycisku Wybierz. W sytuacjach, w którym użytkownik może wybrać potencjalnie wiele rekordów, takich jak w aplikacji opartych na sieci web poczty e-mail, której użytkownik może chcieć zaznaczyć wiele wiadomości, aby usunąć pole wyboru oferuje funkcje, które nie są dostępne z przycisk wyboru lub przycisku radiowego interfejsy użytkownika.

W tym samouczku analizuje jak dodać kolumnę przycisków radiowych do widoku GridView. Postępowania w tym samouczku przedstawiono za pomocą pola wyboru.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Krok 1: Tworzenie, rozszerzanie stron sieci Web w widoku GridView

Przed rozpoczęciem firma Microsoft udoskonalanie widoku GridView, aby zawierała kolumnę przycisków radiowych, umożliwiają s najpierw Poświęć chwilę, aby utworzyć w naszym projekt witryny sieci Web, które będą potrzebne dla tego samouczka i dwie strony ASP.NET. Rozpocznij od dodania nowy folder o nazwie `EnhancedGridView`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Dodawanie stron ASP.NET samouczki dotyczące SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki dotyczące SqlDataSource


Podobnie jak w innych folderach `Default.aspx` w `EnhancedGridView` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Na koniec Dodaj następujące cztery strony jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po używanie kontroli SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczki.


![Mapy witryny zawiera teraz wpisy wzmocnienia samouczki widoku GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpisy wzmocnienia samouczki widoku GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Krok 2: Wyświetlanie dostawców w widoku GridView

Dla tego samouczka umożliwiają s kompilacji Element GridView z listą dostawców z USA, do każdego wiersza w widoku GridView dostarczanie przycisku radiowego. Po wybraniu dostawcy za pomocą przycisku radiowego, użytkownik może przeglądać produktów s dostawcy przez kliknięcie przycisku. Podczas tego zadania może dźwiękowej trivial, istnieje szereg precyzyjnie wchodzące szczególnie istotne znaczenie. Zanim firma Microsoft delve do tych precyzyjnie, chętnie s najpierw pobrać Element GridView listę dostawców.

Uruchamianie przez otwarcie `RadioButtonField.aspx` strony `EnhancedGridView` folderu przeciągnąć element GridView z przybornika do projektanta. Ustaw GridView s `ID` do `Suppliers` i z jego tagów inteligentnych chce utworzyć nowe źródło danych. W szczególności utworzyć ObjectDataSource o nazwie `SuppliersDataSource` która pobiera dane z `SuppliersBLL` obiektu.


[![Utwórz nowy element ObjectDataSource o nazwie SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Rysunek 4**: Utwórz nowy składnik o nazwie ObjectDataSource `SuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Skonfiguruj ObjectDataSource do użycia klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Rysunek 5**: Konfigurowanie ObjectDataSource użyć `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Ponieważ chcemy tylko listę tych dostawców w Stanach Zjednoczonych, wybierz `GetSuppliersByCountry(country)` metodę z listy rozwijanej wybierz kartę.


[![Skonfiguruj ObjectDataSource do użycia klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Rysunek 6**: Konfigurowanie ObjectDataSource użyć `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


Z aktualizacji karty, wybierz (Brak) opcję i kliknij przycisk Dalej.


[![Skonfiguruj ObjectDataSource do użycia klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Rysunek 7**: Konfigurowanie ObjectDataSource użyć `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Ponieważ `GetSuppliersByCountry(country)` metoda przyjmuje parametr, skonfiguruj źródło danych, Kreator wyświetli monit nam dla źródła parametru. Aby określić wartość zakodowany (USA, w tym przykładzie), pozostaw parametr listy rozwijanej źródła wartość None i wprowadź wartość domyślną w polu tekstowym. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Użyj USA jako wartość domyślną dla parametru kraju](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Rysunek 8**: USA używana jako domyślna wartość dla `country` parametr ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Po zakończeniu działania kreatora widoku GridView będzie zawierać elementu BoundField dla każdego pola danych dostawcy. Usuń wszystkie elementy oprócz `CompanyName`, `City`, i `Country` BoundFields i Zmień nazwę `CompanyName` BoundFields `HeaderText` właściwości dostawcy. Po wykonaniu tej czynności, składni deklaratywnej GridView i element ObjectDataSource powinien wyglądać podobnie do poniższej.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

W tym samouczku umożliwiają s Zezwalaj użytkownikom na wyświetlanie wybranego dostawcy produktów s, na tej samej stronie jako listy dostawcy lub na innej stronie. Aby to umożliwić, należy dodać dwa kontrolki przycisku w sieci Web do strony. I Zapisz zestaw `ID` s tych dwóch przycisków, aby `ListProducts` i `SendToProducts`, z myślą że w przypadku `ListProducts` zostanie kliknięty nastąpi odświeżenie strony i produktów wybranego dostawcy s wyświetlą się na tej samej stronie, ale `SendToProducts` zostanie kliknięty użytkownik zostanie whisked do innej strony, który zawiera listę produktów.

Na rysunku nr 9 przedstawiono `Suppliers` GridView i dwie sieci Web przycisku kontrolki widzianego za pośrednictwem przeglądarki.


[![Tych dostawców z USA ma ich nazwy, miasta i wyświetlane informacje o kraju](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Rysunek 9**: tych dostawców z USA ma ich nazwy, miasta i kraju informacje wymienione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Krok 3: Dodawanie kolumny przycisków radiowych

W tym momencie `Suppliers` GridView ma trzy BoundFields wyświetlanie nazwę firmy, miasta i kraju każdego z dostawców w USA. Nadal są jednak niedostateczne kolumnie przycisków radiowych. Niestety t GridView obejmują RadioButtonField wbudowanych, w przeciwnym razie firma Microsoft może wystarczy dodać go do siatki i wykonać. Zamiast tego, możemy dodać pole TemplateField i skonfigurować jej `ItemTemplate` do renderowania przycisku radiowego, co powoduje przycisku radiowego dla każdego wiersza w widoku GridView.

Początkowo może przyjęto założenie, że interfejs odpowiedniego użytkownika może być zaimplementowany przez dodawanie do formantu RadioButton sieci Web `ItemTemplate` z TemplateField. Do każdego wiersza w widoku GridView zostanie dodany w rzeczywistości przycisku radiowego pojedynczego, przyciski radiowe nie mogą być grupowane i w związku z tym nie wykluczają. Użytkownik końcowy jest możliwe wybranie wielokrotnych przycisków radiowych jednocześnie z widoku GridView.

Mimo że za pomocą TemplateField formantów RadioButton sieci Web nie oferuje funkcje potrzebujemy, umożliwiają s zaimplementować takie podejście, ponieważ s wyodrębnienie zbadać, dlaczego nie są grupowane wynikowy przycisków radiowych. Rozpocznij od dodania TemplateField do widoku GridView dostawców, dzięki czemu po lewej stronie pola. Następnie z widoku GridView s tagu, kliknij łącze Edytuj szablony i przeciągnij formant sieci RadioButton Web z przybornika do TemplateField s `ItemTemplate` (zobacz rysunek 10). Ustaw RadioButton s `ID` właściwości `RowSelector` i `GroupName` właściwości `SuppliersGroup`.


[![Dodawanie formantu RadioButton sieci Web do właściwości ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Na rysunku nr 10**: Dodawanie formantu RadioButton sieci Web do `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Po wprowadzeniu tych dodatków poprzez projektanta, znaczników s z widoku GridView powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

RadioButton s [ `GroupName` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) co to jest używana do grupowania serii przycisków radiowych. Wszystkich kontrolek RadioButton o takim samym `GroupName` wartości są traktowane jako pogrupowane; można wybrać tylko jedną opcję z grupy naraz. `GroupName` Właściwość określa wartość dla przycisku radiowego renderowanych s `name` atrybutu. Przeglądarka sprawdza przycisków radiowych `name` atrybutów radia przycisk grupowania.

Za pomocą formantu RadioButton sieci Web dodany do `ItemTemplate`, odwiedź stronę tej strony za pośrednictwem przeglądarki i kliknij pozycję przycisków radiowych w wierszami siatki s. Powiadomienia, jak przyciski radiowe nie są grupowane i umożliwiając wybierz wszystkie wiersze, jako rysunek 11 zawiera.


[![Przyciski radiowe s GridView są zgrupowane nie](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Rysunek 11**: przycisków radiowych s widok GridView są zgrupowane nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


Jest przyczyna przycisków radiowych nie są grupowane, ponieważ ich renderowanych `name` atrybuty są różne, pomimo o tych samych `GroupName` ustawienie właściwości. Aby wyświetlić te różnice, czy widok/źródła z przeglądarki i sprawdź, czy znaczników przycisku radiowego:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Powiadomienie jak zarówno `name` i `id` atrybuty nie są dokładne wartości określone w oknie właściwości, ale jest dołączany na początku o liczbie innych `ID` wartości. Dodatkowy `ID` wartości dodane na początku renderowanych `id` i `name` atrybuty `ID` s radia przycisków kontrolki nadrzędnej `GridViewRow` s `ID` s, GridView s `ID`, Zawartość formantu s `ID`i s formularza sieci Web `ID`. Te `ID` s są dodawane tak, aby każdy renderowania formantu sieci Web w widoku GridView ma unikatową `id` i `name` wartości.

Każdy renderowania formantu musi innej `name` i `id` ponieważ jest to, jak przeglądarka unikatowo identyfikuje każdego formantu w po stronie klienta i jak identyfikuje do serwera sieci web akcję lub zostało zmienione na ogłaszania zwrotnego. Na przykład załóżmy, że możemy do uruchomienia kodu po stronie serwera, gdy RadioButton s wyewidencjonowywany uległ zmianie stan. Firma Microsoft może to zrobić przez ustawienie RadioButton s `AutoPostBack` właściwości `True` i tworzenie obsługi zdarzeń dla `CheckChanged` zdarzeń. Jednak jeśli renderowanych `name` i `id` wartości dla wszystkich przycisków radiowych były takie same na ogłaszanie firma Microsoft nie może określić, jakich kliknięto przycisk radiowy.

Short jego jest, że nie można utworzyć kolumny przycisków radiowych w widoku GridView za pomocą formantu RadioButton sieci Web. Zamiast tego należy używamy raczej archaicznym techniki aby upewnić się, że odpowiedni kod znaczników jest wstrzykiwane do każdego wiersza w widoku GridView.

> [!NOTE]
> Jak formantu RadioButton sieci Web, przycisk radiowy kontrolka HTML, po dodaniu do szablonu, będzie zawierać unikatowy `name` atrybutu, co w siatce rozgrupować przycisków radiowych. Jeśli nie masz doświadczenia z kontrolek HTML, możesz pominąć ten Uwaga kontrolek HTML są rzadko używane, szczególnie w programie ASP.NET 2.0. Ale jeśli chcesz dowiedzieć się więcej, zobacz [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) wpis w blogu s [formantów sieci Web i kontrolek HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Używanie formantu literału iniekcję znaczników przycisku radiowego

Aby można było poprawnie pogrupować wszystkie przycisków radiowych w widoku GridView, należy ręcznie wprowadzić kod znaczników przycisków radiowych do `ItemTemplate`. Każdy przycisk radiowy musi taka sama `name` atrybutu, ale powinny mieć unikatową `id` atrybutu (w przypadku, gdy chcemy, aby uzyskać dostęp przycisku radiowego za pomocą skryptu po stronie klienta). Inny użytkownik wybierze przycisk radiowy i ogłoszeń kopii strony, przeglądarka będzie odesłania wartość wybranego przycisku radiowego s `value` atrybutu. W związku z tym każdy przycisk radiowy należy unikatowego `value` atrybutu. Na koniec na ogłaszania zwrotnego musimy upewnij się dodać `checked` atrybut do przycisku jednej opcji wybrany, w przeciwnym razie po użytkownika powoduje, że zaznaczenie i ponownie wpisów, przycisków radiowych wróci do stanu domyślnego (wszystkie niezaznaczone).

Istnieją dwie metody, które można podjąć w celu iniekcję niskiego poziomu znaczników do szablonu. Jeden jest zarówno znaczników i wywołania metod zdefiniowany w klasie związanej z kodem formatowania. Ta technika najpierw został omówiony w [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) samouczka. W tym przypadku go może wyglądać mniej więcej tak:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

W tym miejscu `GetUniqueRadioButton` i `GetRadioButtonValue` jest zdefiniowany w klasie związanej z kodem, który zwrócony odpowiedni metody `id` i `value` atrybutu wartości dla każdego przycisku radiowego. Ta metoda sprawdza się w przypadku przypisywania `id` i `value` atrybuty, ale znajduje się krótki po określeniu `checked` wartość atrybutu, ponieważ składnia wiązania z danymi jest wykonywane tylko, gdy dane najpierw jest powiązana z widoku GridView. W związku z tym, jeśli w widoku GridView jest włączony stan widoku, metod formatowania tylko uruchomią podczas ładowania strony do (lub gdy widoku GridView jest jawnie odbitych do źródła danych) i w związku z tym funkcja, która ustawia `checked` można wywołać atrybutu won t ogłaszania zwrotnego. Go s raczej niewielkie problem i nieco poza zakres tego artykułu, więc I pozostanie w tej. Czy, jednak zachęca spróbuj użyć powyższe podejście i działa ona za pośrednictwem do punktu, w którym będzie można zostać zablokowane. Gdy t wykonywania won uzyskać żadnych bliżej do wersji roboczych, może pomóc sprzyjać głębsze zrozumienie widoku GridView i cyklem życia wiązania z danymi.

Inne podejścia iniekcję niestandardowy, niskiego poziomu znacznika w szablonie i metody, która będzie używana w tym samouczku jest dodanie [formancie Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) do szablonu. Następnie, w widoku GridView s `RowCreated` lub `RowDataBound` obsługi zdarzeń w formancie Literal mogą uzyskiwać programowo i jego `Text` właściwość znaczników emisji.

Uruchom przez usunięcie RadioButton z TemplateField s `ItemTemplate`, zastępując formancie Literal. Ustawianie formantu literału s `ID` do `RadioButtonMarkup`.


[![Dodawanie formantu literału do właściwości ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Rysunek 12**: Dodawanie formantu literału do `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Następnie należy utworzyć programu obsługi zdarzeń dla widoku GridView s `RowCreated` zdarzeń. `RowCreated` Zdarzenia generowane, gdy dla każdego wiersza dodane, czy dane jest trwa odbitych do widoku GridView. Oznacza to, że nawet w przypadku odświeżania strony po załadowaniu danych ze stanu widoku `RowCreated` nadal generowane zdarzenie i jest to powód korzystamy, zamiast `RowDataBound` (który generowane tylko po jawnie powiązania danych z danymi formantu sieci Web).

W tej obsłudze zdarzeń tylko chcemy kontynuować, jeśli firma Microsoft re postępowania z wierszem danych. Dla każdego wiersza danych chcemy odwołać programowo `RadioButtonMarkup` formancie Literal i zestaw jej `Text` właściwość znaczników do wysyłania. Poniższy kod ilustruje znaczników wysyłanego tworzy radia przycisk, którego `name` atrybut ma ustawioną `SuppliersGroup`, których `id` atrybut ma ustawioną `RowSelectorX`, gdzie *X* jest indeks wiersza elementu GridView i którego `value` atrybutu jest ustawiona na indeks wiersza elementu GridView.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Gdy zostanie wybrany wiersz widoku GridView i odświeżenie strony, Dbamy o `SupplierID` wybranego dostawcy. W związku z tym, co traktować że wartość opcji powinna być rzeczywiste `SupplierID` (zamiast indeks wiersza elementu GridView). Gdy to może działać w pewnych okolicznościach, będzie stanowić ryzyko dla bezpieczeństwa ślepo przyjmował i przetwarzał `SupplierID`. Na przykład, naszych GridView wyświetla tylko dostawcy w USA. Jednak jeśli `SupplierID` są przekazywane bezpośrednio z przycisk radiowy, jakie s, aby zatrzymać mischievous użytkownika z manipulowanie `SupplierID` wartość wysyłane do ogłaszania zwrotnego? Za pomocą indeks wiersza jako `value`, a następnie konfigurowania `SupplierID` odświeżania strony z `DataKeys` kolekcji, możemy zagwarantować, że użytkownik jest tylko przy użyciu jednej z `SupplierID` wartości skojarzone z jednego z wierszy w widoku GridView.

Po dodaniu tego kod obsługi zdarzeń, zabrać kilka minut na przetestowanie strony w przeglądarce. Najpierw należy pamiętać, że opcji tylko jeden przycisk w siatce można wybrać w czasie. Jednak gdy wybranie przycisku radiowego i kliknij jeden z przycisków, odświeżenie strony występuje i przywrócić wszystkie przycisków radiowych do stanu początkowego (czyli, strony, wybranego przycisku radiowego jest już zaznaczone). Aby rozwiązać ten problem, należy rozszerzyć `RowCreated` obsługi zdarzeń, tak że bada indeksu przycisk opcji wybranych wysyłane z ogłaszania zwrotnego i dodaje `checked="checked"` atrybutu emitowany znaczników dopasowań indeks wiersza.

Podczas odświeżania strony wystąpienia przeglądarki odsyła `name` i `value` z wybranego przycisku radiowego. Wartość można programowo pobrać przy użyciu `Request.Form("name")`. [ `Request.Form` Właściwości](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) zapewnia [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) reprezentujący zmiennych formularza. Zmiennych formularza są nazwy i wartości pola formularza na stronie sieci web i są wysyłane przez przeglądarki sieci web, przy każdym ensues odświeżania strony. Ponieważ renderowanych `name` atrybut przycisków radiowych w widoku GridView jest `SuppliersGroup`, gdy strony sieci web jest przesyłana z powrotem wyśle przeglądarki `SuppliersGroup=valueOfSelectedRadioButton` do serwera sieci web (wraz z innych pól formularza). Następnie te informacje są dostępne z `Request.Form` przy użyciu właściwości: `Request.Form("SuppliersGroup")`.

Ponieważ firma Microsoft będzie konieczne ustalenie wybranego przycisku radiowego indeksu nie tylko w `RowCreated` program obsługi zdarzeń, ale w `Click` dodać let s obsługi zdarzeń dla formantów sieci Web przycisku, `SuppliersSelectedIndex` właściwości do klasy związane z kodem, które zwraca `-1`zaznaczenie ma przycisku radiowego i wybranego indeksu, jeśli wybrano jeden z przycisków radiowych.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Z tą właściwością dodane wiemy, aby dodać `checked="checked"` znaczników w `RowCreated` obsługi zdarzeń podczas `SuppliersSelectedIndex` jest równe `e.Row.RowIndex`. Aktualizacja programu obsługi zdarzeń, aby uwzględnić tę logikę:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Dzięki tej zmianie wybranego przycisku radiowego pozostaje zaznaczone po odświeżeniu strony. Teraz, gdy mamy możliwość określenia, jakie przycisk radiowy zostanie wybrany można możemy zmienić zachowanie tak, że gdy najpierw została odwiedzoną stronę, wybrano przycisk radiowy pierwszy wiersz s GridView (a nie o nie przycisków radiowych domyślnie zaznaczone, która jest bieżącą zachowanie). Aby pierwszy przycisk radiowy domyślnie zaznaczone, po prostu zmienić `If SuppliersSelectedIndex = e.Row.RowIndex Then` instrukcji do następującego: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

W tym momencie w widoku GridView umożliwiający pojedynczy wiersz widoku GridView i zapamiętanych między ogłaszania zwrotnego dodaliśmy kolumnie przycisków radiowych grupowanych. Nasze następne kroki są do wyświetlenia produktów dostarczone przez wybranego dostawcę. W kroku 4 zajmiemy się tym, jak przekierowanie użytkownika do innej strony wysyłania wzdłuż wybranego `SupplierID`. W kroku 5 Firma Microsoft będzie widoczny sposób wyświetlania produktów s wybranego dostawcy w widoku GridView na tej samej stronie.

> [!NOTE]
> Zamiast przy użyciu TemplateField (fokus to długie krok 3), można utworzyć niestandardowy `DataControlField` klasy, która renderuje interfejs odpowiedniego użytkownika i funkcje. [ `DataControlField` Klasy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) jest klasą podstawową, z którego pochodzi elementu BoundField, CheckBoxField TemplateField i innych pól wbudowanych GridView i widoku DetailsView. Tworzenie niestandardowego `DataControlField` klasy oznaczałoby mógł zostać dodany tylko za pomocą składni deklaratywnej kolumnie przycisków radiowych, a także spowodowałoby replikowanie funkcji na innych stron sieci web i innych aplikacji sieci web znacznie łatwiejsze.


Jeśli był tworzenia niestandardowych, skompilowany formantów w ASP.NET, jednak znasz czy to tak wymaga odpowiedniej ilości legwork i niesie ze sobą hosta precyzyjnie oraz przypadków krawędzi, musi być starannie obsługiwane. W związku z tym firma Microsoft będzie zrezygnujesz z implementacji kolumnie przycisków radiowych jako niestandardowego `DataControlField` klasy teraz i przestrzegaj opcja TemplateField. Prawdopodobnie będziesz mieć możliwość tworzenia, przy użyciu i wdrażanie niestandardowych `DataControlField` klas w przyszłości samouczek!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Krok 4: Wyświetlanie produktów s wybranego dostawcy w osobnej stronie

Po wybraniu użytkownika wiersza elementu GridView musimy Pokaż wybranego dostawcy produktów s. W niektórych sytuacjach może chcemy do wyświetlenia tych produktów w osobnej stronie, a w innych łączyli możemy w tej samej stronie. Let s, najpierw należy przejrzeć sposób wyświetlania produktów w osobnej stronie; w kroku 5 przyjrzymy Dodawanie widoku GridView do `RadioButtonField.aspx` do wyświetlenia produktów wybranego dostawcy s.

Obecnie dostępne są dwa kontrolki przycisku w sieci Web na stronie `ListProducts` i `SendToProducts`. Gdy `SendToProducts` przycisku, chcemy wysłać użytkownikowi `~/Filtering/ProductsForSupplierDetails.aspx`. Ta strona została utworzona w [wzorzec/szczegół filtrowania między dwoma stronami](../masterdetail/master-detail-filtering-across-two-pages-vb.md) samouczka i wyświetla produktów dla dostawcy których `SupplierID` jest przekazywana do pola ciągu kwerendy o nazwie `SupplierID`.

Do tej funkcji, należy utworzyć programu obsługi zdarzeń dla `SendToProducts` przycisk s `Click` zdarzeń. W kroku 3 dodaliśmy `SuppliersSelectedIndex` właściwość, która zwraca indeks wiersza, którego przycisk radiowy jest zaznaczony. Odpowiednie `SupplierID` mogą być pobierane z widoku GridView s `DataKeys` kolekcji, a użytkownik może następnie wysyłane do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` przy użyciu `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Ten kod działa bajeczną tak długo, jak wybierany jest jeden z przycisków radiowych z widoku GridView. Jeśli początkowo widoku GridView nie ma żadnych przycisków radiowych zaznaczone, a użytkownik kliknie `SendToProducts` przycisku `SuppliersSelectedIndex` będzie `-1`, co spowoduje wyświetlanie wyjątków od `-1` jest poza zakresem indeksu `DataKeys`kolekcji. Jednak jest nie niepożądane, jeśli chcesz zaktualizować `RowCreated` obsługi zdarzeń, zgodnie z opisem w kroku 3 tak, aby mieć pierwszego przycisku radiowego w widoku GridView początkowo zaznaczone.

Aby uwzględnić `SuppliersSelectedIndex` wartość `-1`, Dodaj formant etykiety sieci Web ze stroną powyżej widoku GridView. Ustaw jego `ID` właściwości `ChooseSupplierMsg`, jego `CssClass` właściwości `Warning`, jego `EnableViewState` i `Visible` właściwości `False`i jego `Text` właściwości należy wybrać dostawcę z siatki. Klasa CSS `Warning` tekst jest wyświetlany czerwony, pogrubienie, kursywa dużej czcionki i jest zdefiniowany w `Styles.css`. Przez ustawienie `EnableViewState` i `Visible` właściwości `False`, etykiety nie są odtwarzane z wyjątkiem dla tylko te postbacks where kontroli s `Visible` programowo ustawioną właściwość `True`.


[![Dodawanie formantu etykiety Web powyżej widoku GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Rysunek 13**: Dodaj etykietę sieci Web kontroli nad widok GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Następnie należy rozszerzyć `Click` obsługi zdarzeń do wyświetlenia `ChooseSupplierMsg` etykiet, jeśli `SuppliersSelectedIndex` jest mniejsza od zera i przekieruje użytkownika do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` w przeciwnym razie wartość.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Odwiedź stronę w przeglądarce i kliknij przycisk `SendToProducts` przycisk przed wybraniem dostawcy z widoku GridView. Jak pokazano na rysunku 14, spowoduje to wyświetlenie `ChooseSupplierMsg` etykiety. Następnie wybierz dostawcę i kliknij przycisk `SendToProducts` przycisku. Spowoduje to whisk stronę, która zawiera listę produktów dostarczanych przez wybranego dostawcę. Przedstawia rysunek 15 `ProductsForSupplierDetails.aspx` strony, gdy wybrano dostawcę browarów Bigfoot.


[![Etykieta ChooseSupplierMsg jest wyświetlana, jeśli dostawca nie jest zaznaczone.](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Rysunek 14**: `ChooseSupplierMsg` Jeśli wybrano dostawcę nie jest wyświetlana etykieta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Wybrany dostawca produktów s są wyświetlane w ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Rysunek 15**: wybrany dostawca s produktów są wyświetlane w `ProductsForSupplierDetails.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Krok 5: Wyświetlanie produktów s wybranego dostawcy w tej samej stronie

W kroku 4 widzieliśmy jak wysłać do użytkownika do innej strony sieci web do wyświetlania wybranego dostawcy produktów s. Alternatywnie produktów s wybranego dostawcy mogą być wyświetlane na tej samej stronie. Na przykład dodamy innego widoku GridView do `RadioButtonField.aspx` do wyświetlenia produktów wybranego dostawcy s.

Ponieważ chcemy tylko tego widoku GridView produktów do wyświetlenia po wybraniu dostawcy, Dodaj formant Panel sieci Web pod `Suppliers` GridView ustawienie jej `ID` do `ProductsBySupplierPanel` i jego `Visible` właściwości `False`. W panelu, Dodaj tekst produktów dla dostawcy wybrane następuje Element GridView o nazwie `ProductsBySupplier`. Z widoku GridView s tagu, wybierz powiązać nowy element ObjectDataSource o nazwie `ProductsBySupplierDataSource`.


[![Nowy element ObjectDataSource powiązać ProductsBySupplier widoku GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Rysunek 16**: powiązać `ProductsBySupplier` GridView do nowego elementu ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Skonfiguruj ObjectDataSource do użycia `ProductsBLL` klasy. Ponieważ chcemy pobrać te produkty dostarczone przez wybranego dostawcę, określ, czy należy wywołać element ObjectDataSource `GetProductsBySupplierID(supplierID)` metoda pobierania danych. (Brak) wybierz z listy rozwijanej w UPDATE, INSERT i usuwanie kart.


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Rysunek 17**: Konfigurowanie ObjectDataSource użyć `GetProductsBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Ustaw listy rozwijanej (Brak) w UPDATE, INSERT i usuwanie kart](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Rysunek 18**: Ustaw listy rozwijanej (Brak) w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Po skonfigurowaniu wyboru, aktualizacji, wstawianie i usuwanie kart, kliknij przycisk Dalej. Ponieważ `GetProductsBySupplierID(supplierID)` metoda oczekuje parametru wejściowego, Utwórz źródło danych, Kreator wyświetli monit nam określić źródło wartości parametru s.

Mamy kilka opcji tutaj w określanie źródło wartości parametru s. Firma Microsoft może używać domyślnego parametru obiektu i programowo przypisać wartość `SuppliersSelectedIndex` właściwości do parametru s `DefaultValue` właściwości w elemencie ObjectDataSource s `Selecting` program obsługi zdarzeń. Odwołaj się do [programowane Ustawianie wartości parametrów elementu ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) samouczka dla odświeżacza na programowo przypisywania wartości parametrów elementu ObjectDataSource s.

Możemy również użyć parametrze ControlParameter i zapoznaj się z `Suppliers` GridView s [ `SelectedValue` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (patrz rysunek 19). GridView s `SelectedValue` zwraca `DataKey` wartość odpowiadającą [ `SelectedIndex` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Aby ta opcja działała, należy ustawić programowo s widoku GridView `SelectedIndex` właściwości wybranego wiersza, kiedy `ListProducts` kliknięciu przycisku. Jako dodatkowa korzyść, ustawiając `SelectedIndex`, będzie miał wybranego rekordu `SelectedRowStyle` zdefiniowane w `DataWebControls` motywu (żółty tła).


[![Umożliwia określenie SelectedValue s widoku GridView, jako źródło parametru w parametrze ControlParameter](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Rysunek 19**: umożliwia określenie GridView s SelectedValue, jako źródło parametru w parametrze ControlParameter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Po zakończeniu działania kreatora programu Visual Studio automatycznie doda pola dla pola danych s produktu. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, i `UnitPrice` BoundFields i zmień `HeaderText` właściwości do produktu, kategorii i cenę. Skonfiguruj `UnitPrice` elementu BoundField tak, aby jego wartość jest w formacie waluty. Po wprowadzeniu tych zmian, znaczników deklaratywne s panelu, GridView i ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Do ukończenia tego ćwiczenia, musimy ustawić GridView s `SelectedIndex` właściwości `SelectedSuppliersIndex` i `ProductsBySupplierPanel` panelu s `Visible` właściwości `True` podczas `ListProducts` przycisk zostanie kliknięty. W tym celu należy utworzyć programu obsługi zdarzeń dla `ListProducts` kontrolki przycisku w sieci Web s `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Jeśli nie wybrano dostawcę z widoku GridView, `ChooseSupplierMsg` jest wyświetlana etykieta i `ProductsBySupplierPanel` panelu ukryte. W przeciwnym razie, jeśli wybrano dostawcę, `ProductsBySupplierPanel` wyświetleniem i GridView s `SelectedIndex` właściwość jest aktualizowana.

20 rysunek pokazuje wyniki po wybrano dostawcę browarów Bigfoot i produktów Pokaż przycisk Strona zostanie kliknięta.


[![Produkty dostarczane przez browarów Bigfoot są wyświetlane na tej samej stronie](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Rysunek 20**: produkty dostarczane przez browarów Bigfoot są wyświetlane na tej samej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Podsumowanie

Zgodnie z opisem w [wzorca/Detail, przy użyciu wybieranych GridView wzorca z DetailView szczegóły](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) samouczka można wybrać rekordów z widoku GridView przy użyciu CommandField których `ShowSelectButton` właściwość jest ustawiona na `True`. Ale CommandField jako regularne przycisków, linki lub obrazów są wyświetlane przyciski jej. Interfejs użytkownika alternatywnych zaznaczenie wiersza jest zapewnienie przycisku radiowego lub pole wyboru w każdym wierszu widoku GridView. W tym samouczku będziemy zbadać jak dodać kolumnę przycisków radiowych.

Niestety należy dodać kolumnę t przycisków radiowych jako prostego lub prostego jako jeden może spodziewać się. Nie istnieje żadne wbudowane RadioButtonField dodaną na kliknięcie przycisku i za pomocą formantu RadioButton Web TemplateField wprowadza własny zestaw problemów. W celu zapewnienie taki interfejs albo mamy utworzyć niestandardowy `DataControlField` klasy lub możliwości na iniekcję odpowiednie HTML na pole TemplateField podczas `RowCreated` zdarzeń.

O zbadane, jak dodać kolumnę przycisków radiowych poinformować nas, Włącz wymagające uwagi, aby dodać kolumnę pola wyboru. Z kolumną zawierającą pola wyboru użytkownik może wybrać co najmniej jeden wiersz widoku GridView i następnie wykonać pewne operacje na wszystkie zaznaczone wiersze (na przykład wybrać zestaw wiadomości e-mail opartych na sieci web klienta poczty e-mail, a następnie wybierając można usunąć wszystkie wybrane wiadomości e-mail). W następnym samouczku będzie przedstawiono sposób dodawania takie kolumny.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Suru Dominika. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
[dalej](adding-a-gridview-column-of-checkboxes-vb.md)
