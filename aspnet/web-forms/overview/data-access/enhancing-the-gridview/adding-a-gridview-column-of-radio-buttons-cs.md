---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Dodawanie kolumny przycisków radiowych (C#) do kontrolki GridView | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku pokazano, jak można dodać kolumny przycisków radiowych do kontrolki GridView, aby przedstawić użytkownikowi bardziej intuicyjny sposób wybierania pojedynczy wiersz...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: e1ac64a3c5023d83259f6e2d6facd8e248768c2d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755886"
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Dodawanie kolumny przycisków radiowych (C#) do kontrolki GridView
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) lub [Pobierz plik PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> W tym samouczku pokazano, jak można dodać kolumny przycisków radiowych do kontrolki GridView, aby przedstawić użytkownikowi bardziej intuicyjny sposób wybierania pojedynczy wiersz GridView.


## <a name="introduction"></a>Wprowadzenie

W kontrolce GridView oferuje wiele wbudowanych funkcji. Obejmuje szereg różnych pól do wyświetlania tekstu, obrazów, hiperłącza i przyciski. Obsługuje ona szablony do dalszego dostosowania. Za pomocą kilku kliknięć myszą jego możliwe, aby w kontrolce GridView, gdzie każdy wiersz można wybrać za pośrednictwem przycisku, lub umożliwiające edytowanie lub usuwanie możliwości s. Pomimo mnóstwo podana funkcje często będą sytuacje, w których dodatkowe funkcje nieobsługiwane będzie konieczne będzie dodanie. W tym samouczku i kolejne dwa zajmiemy się, jak poprawić funkcji s GridView, aby uwzględnić dodatkowe funkcje.

W tym samouczku oraz kolejny skoncentrować się na ulepszanie procesów wybór wiersza. Jak badania w [Master/szczegółów przy użyciu GridView wzorca można wybierać z DetailView szczegółów](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), możemy dodać CommandField do widoku GridView, która zawiera przycisk Wybierz. Po kliknięciu ensues odświeżenie strony i GridView s `SelectedIndex` właściwość zostanie zaktualizowany w celu indeks wiersza, którego wybierz przycisk został kliknięty. W [Master/szczegółów przy użyciu GridView wzorca można wybierać z DetailView szczegółów](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka będziemy pokazaliśmy, jak korzystać z tej funkcji w celu wyświetlenia szczegółów dla wybranego wiersza w widoku GridView.

Natomiast przycisku Wybierz działa w wielu sytuacjach, może nie działać jak również dla innych użytkowników. Zamiast za pomocą przycisku, dwa inne elementy interfejsu użytkownika są często używane do wyboru: przycisk radiowy i pól wyboru. Tak, aby zamiast wybierz przycisk, każdy wiersz zawiera przycisku radiowego lub zaznacz pole wyboru, możemy rozszerzyć widoku GridView. W scenariuszach, w którym użytkownik może wybrać tylko jednego z rekordów GridView przycisku radiowego może mieć pierwszeństwo wybierz przycisk. W sytuacjach, w którym użytkownik może wybrać potencjalnie wiele rekordów, takich jak w aplikacji internetowej poczty e-mail, w którym użytkownik może chcieć zaznaczyć wiele wiadomości, aby usunąć pole wyboru oferuje funkcje, który nie jest dostępny przycisk wyboru lub przycisku radiowego interfejsy użytkownika.

W tym samouczku pokazano, jak można dodać kolumny przycisków radiowych do kontrolki GridView. Postępowanie w tym samouczku przedstawiono za pomocą pola wyboru.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Krok 1: Tworzenie udoskonalanie stron sieci Web widoku GridView

Zanim zaczniemy udoskonalanie widoku GridView, aby zawierała kolumnę przycisków radiowych, chętnie s najpierw Poświęć chwilę, aby utworzyć strony ASP.NET w naszym projektu witryny sieci Web, która należy do tego samouczka i kolejne dwa. Rozpocznij od dodania nowy folder o nazwie `EnhancedGridView`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Dodawanie stron ASP.NET związane z kontrolką SqlDataSource samouczki](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Rysunek 1**: Dodawanie stron ASP.NET związane z kontrolką SqlDataSource samouczki


Podobnie jak w przypadku innych folderów `Default.aspx` w `EnhancedGridView` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


Wreszcie, Dodaj te cztery strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po używanie kontrolki SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczków.


![Mapa witryny zawiera teraz wpisy związane z poprawianiem samouczki GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Rysunek 3**: mapy witryny zawiera teraz wpisy związane z poprawianiem samouczki GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Krok 2: Wyświetlanie dostawców w widoku GridView

Dla tego samouczka umożliwiają s kompilacji GridView, zawierającego dostawców z USA, z każdego wiersza w widoku GridView dostarczanie przycisku radiowego. Po wybraniu dostawcy za pośrednictwem przycisku radiowego, użytkownik może przeglądać te produkty s dostawcy przez kliknięcie przycisku. Gdy zadanie to stwierdzenie może wydawać się prosta, istnieje kilka precyzyjnie, które ułatwiają szczególnie trudne. Zanim firma delve do tych precyzyjnie, chętnie s najpierw uzyskać listę dostawców GridView.

Zacznij od otwarcia `RadioButtonField.aspx` stronie `EnhancedGridView` folderu, przeciągając je z przybornika do projektanta w kontrolce GridView. Ustaw GridView s `ID` do `Suppliers` i w tagu inteligentnego, wybrać opcję utworzenia nowego źródła danych. Dokładniej mówiąc, Utwórz ObjectDataSource o nazwie `SuppliersDataSource` która ściąga dane z `SuppliersBLL` obiektu.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Rysunek 4**: Utwórz nowy o nazwie elementu ObjectDataSource `SuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Rysunek 5**: Konfigurowanie kontrolki ObjectDataSource do użycia `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Ponieważ chcemy wyświetlić listę tych dostawców, w Stanach Zjednoczonych, wybierz `GetSuppliersByCountry(country)` metodę z listy rozwijanej wybierz OPCJĘ karty.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Rysunek 6**: Konfigurowanie kontrolki ObjectDataSource do użycia `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


Z aktualizacji karty, wybierz opcję (Brak) opcję i kliknij przycisk Dalej.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Rysunek 7**: Konfigurowanie kontrolki ObjectDataSource do użycia `SuppliersBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Ponieważ `GetSuppliersByCountry(country)` metoda akceptuje parametr, monituje Kreator konfigurowania źródła danych NAS dla źródła tego parametru. Aby określić wartość zakodowany (USA, w tym przykładzie), pozostaw parametr listy rozwijanej źródła ustawiony na wartość None i wprowadź wartość domyślną w polu tekstowym. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Na użytek USA jako wartości domyślnej parametru kraju](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Rysunek 8**: USA użycia jako wartość domyślna dla `country` parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


Po ukończeniu kreatora, widoku GridView będą zawierać elementu BoundField dla każdego pola danych dostawcy. Usuń wszystkie elementy oprócz `CompanyName`, `City`, i `Country` BoundFields i Zmień nazwę `CompanyName` BoundFields `HeaderText` właściwości dostawcy. Po wykonaniu tej czynności, składni deklaratywnej kontrolkami GridView i kontrolki ObjectDataSource powinien wyglądać podobnie do poniższej.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Na potrzeby tego samouczka należy zezwolić s Zezwalaj użytkownikom na wyświetlanie wybranego dostawcy s produktów na stronie listy dostawcy lub na innej stronie. Aby to umożliwić, należy dodać dwie kontrolki przycisku w sieci Web do strony. Czy mogę zestaw ve `ID` s tych dwóch przycisków, aby `ListProducts` i `SendToProducts`, z myślą że w przypadku `ListProducts` kliknięciu nastąpi odświeżenie strony i wybranego dostawcy s produkty zostaną wyświetlone na tej samej stronie, ale `SendToProducts` po kliknięciu użytkownik będzie whisked do innej strony, która zawiera listę produktów.

Nr 9 przedstawiono `Suppliers` kontrolkami GridView i dwie sieci Web przycisku kontrolki podczas wyświetlania za pośrednictwem przeglądarki.


[![Tych dostawców z USA ma ich nazwy, miasta i kraju informacji](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Rysunek 9**: tych dostawców z USA ma ich nazwy, miasta i kraju informacje wymienione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Krok 3. Dodawanie kolumny przycisków radiowych

W tym momencie `Suppliers` GridView ma trzy BoundFields wyświetlanie nazwę firmy, miasta i kraju każdego z dostawców w USA. Go nadal brakuje kolumny przycisków radiowych, jednak. Niestety t GridView obejmują RadioButtonField wbudowane, w przeciwnym razie firma Microsoft po prostu można dodać do siatki i odbywać się. Zamiast tego, możemy dodać TemplateField i skonfigurować jej `ItemTemplate` do renderowania przycisku radiowego skutkuje przycisku radiowego dla każdego wiersza w widoku GridView.

Początkowo może przyjęto założenie, że interfejs żądanego użytkownika może być implementowany przez dodawanie kontrolki RadioButton w sieci Web do `ItemTemplate` z TemplateField. Chociaż w rzeczywistości spowoduje to dodanie przycisku radiowego jednym do każdego wiersza w widoku GridView, przyciski radiowe nie mogą być grupowane i w związku z tym nie wykluczają. Użytkownik końcowy jest możliwe wybranie wielokrotnych przycisków radiowych równocześnie z widoku GridView.

Mimo że za pomocą TemplateField formantów RadioButton w sieci Web nie oferuje funkcje, potrzebujemy, umożliwiają s zaimplementować to podejście, ponieważ s zwiększonej zbadać, dlaczego nie są grupowane wynikowy przycisków radiowych. Rozpocznij, dodając TemplateField w kontrolce GridView dostawcy, dzięki czemu skrajnie po lewej stronie pola. Następnie za pomocą tagu inteligentnego s GridView kliknij link Edytuj szablony i przeciągnięcie formantu RadioButton w sieci Web z przybornika do TemplateField s `ItemTemplate` (zobacz rysunek 10). Ustaw RadioButton s `ID` właściwości `RowSelector` i `GroupName` właściwość `SuppliersGroup`.


[![Dodawanie formantu RadioButton w sieci Web do właściwości ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Na rysunku nr 10**: Dodawanie formantu RadioButton sieci Web do `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


Po wprowadzeniu tych dodatków przy użyciu narzędzia Projektant, znaczników s GridView powinien wyglądać podobny do następującego:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton s [ `GroupName` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) zostały użyte do grupowania serii przycisków radiowych. Wszystkie kontrolki RadioButton o takiej samej `GroupName` wartości są uznawane za pogrupowane; można wybrać przycisk radiowy tylko jeden z grupy w danym momencie. `GroupName` Właściwości określa wartość dla przycisku radiowego renderowanych s `name` atrybutu. Przeglądarka sprawdza, czy przyciski radiowe `name` atrybutów, aby określić radiowego przycisk grupowania.

Za pomocą formantu RadioButton Web dodany do `ItemTemplate`, odwiedź tę stronę za pośrednictwem przeglądarki i kliknij przyciski radiowe w wierszami siatki s. Zwróć uwagę, jak przyciski radiowe nie są grupowane, dzięki czemu można wybrać wszystkie wiersze, jako rysunek 11 pokazano.


[![Przyciski radiowe s GridView są grupowane nie](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Rysunek 11**: przyciski radiowe s GridView są grupowane nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


Przyczyną przycisków radiowych nie są grupowane jest ich renderowanych `name` atrybuty są różne, niezależnie od posiadania takich samych `GroupName` ustawienie właściwości. Aby wyświetlić te różnice, czy widok/źródło z przeglądarki i sprawdź znaczników przycisk radiowy:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Ogłoszenie jak zarówno `name` i `id` atrybuty nie są dokładne wartości, jak określono w oknie dialogowym właściwości, ale są poprzedzonej ciągiem szereg innych `ID` wartości. Dodatkowe `ID` wartości dodane na początku renderowanych `id` i `name` atrybuty są `ID` s radiowego przyciski kontrolek nadrzędnych `GridViewRow` s `ID` s, GridView s `ID`, Zawartość formantu s `ID`i s formularz sieci Web `ID`. Te `ID` s są dodawane, aby każdy renderowania formantu sieci Web w widoku GridView ma unikatową `id` i `name` wartości.

Każdy renderowane potrzeb kontroli innego `name` i `id` ponieważ jest to, jak przeglądarka jednoznacznie identyfikował każdy formant, po stronie klienta i jak je identyfikuje do serwera sieci web akcję lub zostało zmienione na zwrot. Na przykład Wyobraź sobie, że Chcieliśmy do uruchomienia kodu po stronie serwera, zawsze wtedy, gdy zaznaczone RadioButton s, że stan został zmieniony. Firma Microsoft może to zrobić, ustawiając RadioButton s `AutoPostBack` właściwości `true` i tworzenie programu obsługi zdarzeń dla `CheckChanged` zdarzeń. Jednak jeśli renderowanych `name` i `id` wartości dla wszystkich przycisków radiowych są takie same, na ogłaszanie zwrotne firma Microsoft nie może określić, z jakiego konkretnego został kliknięty RadioButton.

Krótki, jego to, że firma Microsoft nie można utworzyć kolumny przycisków radiowych GridView za pomocą kontrolki RadioButton w sieci Web. Zamiast tego należy używamy raczej niestabilnego technik aby upewnić się, że odpowiedni kod znaczników są wstrzykiwane do każdego wiersza w widoku GridView.

> [!NOTE]
> Jak kontrolki sieci RadioButton Web, kontrolka HTML, po dodaniu do szablonu, przycisk radiowy obejmie to unikatowy `name` atrybutu, dzięki czemu przycisków radiowych w siatce niezgrupowane. Jeśli nie jesteś zaznajomiony z kontrolek HTML, możesz zignorować ta uwaga kontrolek HTML są rzadko używane, zwłaszcza w programie ASP.NET 2.0. Ale jeśli chcesz dowiedzieć się więcej, zobacz [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) wpis w blogu s [kontrolki sieci Web oraz HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Używanie formantu literału iniekcję kodu znaczników przycisku radiowego

Aby poprawnie pogrupować wszystkie przyciski radiowe w widoku GridView, należy ręcznie wprowadzić znaczników przycisków radiowych do `ItemTemplate`. Każdego przycisku radiowego musi takie same `name` atrybutu, ale powinien mieć unikatową `id` atrybutu (w przypadku, gdy chcemy dostęp do przycisku radiowego za pomocą skryptu po stronie klienta). Po użytkownik wybierze przycisk radiowy i wpisów kopii strony, przeglądarka zostanie odesłania wartość wybranego przycisku radiowego s `value` atrybutu. W związku z tym, każdy przycisk radiowy potrzebujesz unikatową `value` atrybutu. Na koniec na zwrot musimy upewnić się, że dodano `checked` atrybut do przycisku radiowego jeden, jest zaznaczone, w przeciwnym razie po użytkownik dokona wyboru i ponownie wpisy, przyciski radiowe wróci do stanu domyślnego (wszystkie niezaznaczone).

Istnieją dwie metody, które mogą zostać podjęte w celu wstrzyknięcia znaczników niskiego poziomu do szablonu. Jeden jest różnych znaczników i wywołania do metody zdefiniowanej w klasie CodeBehind formatowania. Ta technika najpierw został omówiony w [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) samouczka. W naszym przypadku go może wyglądać następująco:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

W tym miejscu `GetUniqueRadioButton` i `GetRadioButtonValue` będzie metody zdefiniowanej w klasie związanym z kodem, który zwrócony odpowiedni `id` i `value` atrybutu wartości dla każdego przycisku radiowego. Ta metoda sprawdza się w przypadku przypisywania `id` i `value` atrybutów, ale znajduje się krótki, jeśli zachodzi potrzeba określenia `checked` wartość atrybutu, ponieważ składnia wiązania danych jest wykonywany tylko, gdy najpierw powiązania danych z widoku GridView. W związku z tym, jeśli widoku GridView ma stan widoku, włączone, metody formatowania tylko nastąpi po pierwszym załadowaniu strony (lub gdy widoku GridView jest jawnie odbitych ze źródłem danych) i w związku z tym funkcja, która ustawia `checked` atrybut wygrał t volat jen tehdy ogłaszanie zwrotne. Jego s zamiast drobny problem i nieco poza zakres tego artykułu, dzięki czemu będzie jest pozostawienie tej. Czy mogę sposób, jednak zachęcamy do wypróbowania przy użyciu powyższej metody i działa go za pośrednictwem do punktu, gdzie można będzie zatrzymywane. Gdy t wykonywania wygrał uzyskać żadnych bliżej do wersji roboczych, pomoże sprzyjają lepiej zrozumieć widoku GridView i cyklem życia wiązania danych.

Inne podejście do iniekcji niestandardowe, niskiego poziomu znaczników w szablonie i podejście, który będzie używany na potrzeby tego samouczka jest dodanie [formancie Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) do szablonu. Następnie, w tym s GridView `RowCreated` lub `RowDataBound` programu obsługi zdarzeń w formancie Literal można programowo uzyskać dostęp i jej `Text` właściwością znaczników do emitowania.

Start, usuwając RadioButton s TemplateField `ItemTemplate`, zastępując formancie Literal. Ustaw s w formancie Literal `ID` do `RadioButtonMarkup`.


[![Dodaj formant literału do właściwości ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Rysunek 12**: Dodawanie formantu literału do `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


Następnie należy utworzyć program obsługi zdarzeń dla GridView s `RowCreated` zdarzeń. `RowCreated` Zdarzenia wyzwalane jest raz dla każdego wiersza, dodano informację, czy dane jest trwa odbitych do kontrolki GridView. Oznacza to, że nawet w przypadku ogłaszania zwrotnego po załadowaniu danych ze stanu widoku `RowCreated` nadal generowane zdarzenie i jest to przyczyna używamy, zamiast `RowDataBound` (która jest uruchamiana tylko jeśli danych jest jawnie powiązane z danymi formantu sieci Web).

W tej obsługi zdarzeń ma być uruchamiany tylko postępowania w przypadku możemy ponownie zajmowanie się wiersz danych. Dla każdego wiersza danych chcemy programowo odwołania `RadioButtonMarkup` formancie Literal i ustaw jego `Text` właściwość znaczników do emitowania. Poniższy kod ilustruje znaczników emitowane tworzy radia przycisk, którego `name` ma ustawioną wartość atrybutu `SuppliersGroup`, którego `id` ma ustawioną wartość atrybutu `RowSelectorX`, gdzie *X* jest indeks wiersza w widoku GridView i którego `value` indeks wiersza w widoku GridView ma ustawioną wartość atrybutu.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Po wierszu elementu GridView i występuje odświeżenie strony, jesteśmy zainteresowani `SupplierID` wybranego dostawcy. W związku z tym, jeden traktować wartość każdego przycisku radiowego należy rzeczywiste `SupplierID` (zamiast indeks wiersza w widoku GridView). Gdy ta może działać w pewnych okolicznościach, byłoby to zagrożenie bezpieczeństwa, aby bezrefleksyjne przyjmował i przetwarzał `SupplierID`. Nasze GridView, na przykład wyświetla tylko dostawcy w USA. Jednak jeśli `SupplierID` jest przekazywana bezpośrednio z przyciskiem radiowym, jakie s, aby zatrzymać mischievous użytkownika z manipulowanie `SupplierID` wartość wysyłana po powrocie na odświeżenie strony? Za pomocą indeks wiersza jako `value`, a następnie konfigurowania `SupplierID` na zwrot z `DataKeys` kolekcji, firma Microsoft może upewnić się, że użytkownik jest wyłącznie przy użyciu jednej z `SupplierID` wartości skojarzone z jednego z wierszy GridView.

Po dodaniu tego kodu programu obsługi zdarzeń, należy poświęcić chwilę na przetestowaniu strony w przeglądarce. Najpierw należy zanotować tej opcji tylko jeden przycisk w siatce można wybrać w danym momencie. Jednak po wybierając przycisk radiowy, a następnie klikając pozycję jeden z przycisków, występuje odświeżenie strony i wszystkie przyciski radiowe przywrócenie ich początkowego stanu (czyli na odświeżenie strony, wybranego przycisku radiowego jest już wybrane). Aby rozwiązać ten problem, należy rozszerzyć `RowCreated` programu obsługi zdarzeń, tak że sprawdza indeksu przycisk wybranych opcji wysyłane z ogłaszania zwrotnego i dodaje `checked="checked"` atrybutu emitowany znaczników dopasowania wierszy indeksu.

Jeśli odświeżenie strony występuje, przeglądarce wysyła z powrotem `name` i `value` z wybranego przycisku radiowego. Wartość może programowo pobrać, przy użyciu `Request.Form["name"]`. [ `Request.Form` Właściwość](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) zapewnia [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) reprezentujący zmiennych formularza. Zmiennych formularza są nazwy i wartości pól formularza na stronie sieci web i są wysyłane przez przeglądarkę sieci web po każdym ensues ogłaszania zwrotnego. Ponieważ renderowanych `name` atrybutu przycisków radiowych w widoku GridView `SuppliersGroup`, gdy strona sieci web zostaje opublikowany ponownie wyśle przeglądarki `SuppliersGroup=valueOfSelectedRadioButton` ponownie do serwera sieci web (wraz z innych pól formularza). Następnie te informacje są dostępne z `Request.Form` przy użyciu właściwości: `Request.Form["SuppliersGroup"]`.

Ponieważ firma Microsoft będzie potrzebne do określenia wybranego przycisku radiowego indeksu nie tylko w `RowCreated` programu obsługi zdarzeń, ale `Click` Dodaj umożliwiają s procedury obsługi zdarzeń kontrolki przycisku w sieci Web, `SuppliersSelectedIndex` właściwości klasy związane z kodem, która zwraca `-1`Jeśli nie została zaznaczona opcja i wybranego indeksu, jeśli wybrano jeden z przycisków radiowych.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Z tą właściwością dodane, wiemy, aby dodać `checked="checked"` znaczników w `RowCreated` programu obsługi zdarzeń podczas `SuppliersSelectedIndex` jest równa `e.Row.RowIndex`. Aktualizacja programu obsługi zdarzeń, aby uwzględnić tę logikę:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Dzięki tej zmianie wybranego przycisku radiowego pozostaje wybrane po odświeżeniu strony. Teraz gdy mamy możliwość określenia, jakie przycisk radiowy zostanie wybrany, można zmienimy zachowanie tak, aby podczas pierwszych odwiedzono stronę, został wybrany pierwszy przycisk radiowy wiersz s GridView (a nie mieć żadnych przycisków radiowych domyślnie zaznaczone, który jest bieżący zachowanie). Aby uzyskać pierwszy przycisk radiowy domyślnie zaznaczone, wystarczy zmienić `if (SuppliersSelectedIndex == e.Row.RowIndex)` instrukcji do następującego: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

W tym momencie w widoku GridView umożliwiającej pojedynczy wiersz GridView i zapamiętanych między ogłaszania zwrotnego dodaliśmy kolumny przycisków radiowych zgrupowane. Nasze następne kroki są do wyświetlania produktów, dostarczone przez wybranego dostawcę. W kroku 4 zobaczymy, jak przekierować użytkownika do innej strony wysyłania wzdłuż wybranego `SupplierID`. W kroku 5 zobaczymy sposób wyświetlania produktów s wybranego dostawcy w GridView na tej samej stronie.

> [!NOTE]
> Zamiast używania TemplateField (główne zagadnienie tego długich krok 3), możemy utworzyć niestandardową `DataControlField` klasy, który renderuje odpowiedni interfejs i funkcje. [ `DataControlField` Klasy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) jest klasą bazową, z której pochodzi elementu BoundField, CheckBoxField, TemplateField i innych pól wbudowanych kontrolkami GridView i DetailsView. Tworzenie niestandardowego `DataControlField` oznaczałoby klasy, może zostać dodany tylko za pomocą składni deklaratywnej kolumny przycisków radiowych, a także czyniłyby replikowanie funkcjonalność w innych stron sieci web i innych aplikacji sieci web jest znacznie łatwiejsze.


Jeśli był kiedykolwiek utworzone niestandardowe, skompilowany formantów na platformie ASP.NET, jednak wiesz, że spowoduje to więc wymaga ilość żmudne zadania i niesie ze sobą hosta precyzyjnie i przypadki brzegowe, które muszą być dokładnie obsługiwane. W związku z tym, będzie możemy zrezygnujesz z wdrażania kolumny przycisków radiowych rolę niestandardową `DataControlField` klasy teraz i trzymaj się opcja TemplateField. Prawdopodobnie będziesz mieć możliwość zapoznaj się z tworzenia, przy użyciu oraz wdrażanie niestandardowych `DataControlField` klas w przyszłości samouczek!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Krok 4: Wyświetlanie produktów s wybranego dostawcy w osobnej stronie

Po użytkownik wybrał wierszu elementu GridView, musimy Pokaż wybranego dostawcy s produktów. W niektórych sytuacjach może chcemy wyświetlić tych produktów w osobnej stronie, a w innych będziemy preferować odbywa się w tej samej stronie. Pozwól najpierw sprawdzić sposób wyświetlania produktów w osobnej stronie; s w kroku 5 przyjrzymy GridView do dodawania `RadioButtonField.aspx` do wyświetlania produktów wybranego dostawcy s.

Obecnie dostępne są dwie kontrolki przycisku w sieci Web, na stronie `ListProducts` i `SendToProducts`. Gdy `SendToProducts` przycisku, chcemy wysłać użytkownikowi `~/Filtering/ProductsForSupplierDetails.aspx`. Ta strona została utworzona w [wzorzec/szczegół filtrowania na dwóch stronach](../masterdetail/master-detail-filtering-across-two-pages-cs.md) samouczek i wyświetla produktów dla dostawcy którego `SupplierID` jest przekazywana do pola ciągu kwerendy o nazwie `SupplierID`.

Do tej funkcji, należy utworzyć program obsługi zdarzeń dla `SendToProducts` przycisk s `Click` zdarzeń. W kroku 3 dodaliśmy `SuppliersSelectedIndex` wybrano właściwość, która zwraca indeks wiersza, którego przycisku radiowego. Odpowiedni `SupplierID` mogą być pobierane z GridView s `DataKeys` kolekcji i użytkownik może następnie wysyłane do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` przy użyciu `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Ten kod działa bajeczną tak długo, jak jeden z przycisków radiowych wybrano w widoku GridView. Jeśli początkowo widoku GridView nie ma żadnych przycisków radiowych zaznaczone i użytkownik klika polecenie `SendToProducts` przycisku `SuppliersSelectedIndex` będzie `-1`, co spowoduje zgłoszenie wyjątku od `-1` jest poza zakresem indeksu `DataKeys`kolekcji. Jest to nie stanowi problemu, jednak, jeśli chcesz zaktualizować `RowCreated` program obsługi zdarzeń, zgodnie z opisem w kroku 3 zastosowania pierwszy przycisk radiowy GridView początkowo zaznaczone.

Aby uwzględnić `SuppliersSelectedIndex` wartość `-1`, Dodaj kontrolkę etykieta w sieci Web do strony powyżej widoku GridView. Ustaw jego `ID` właściwości `ChooseSupplierMsg`, jego `CssClass` właściwości `Warning`, jego `EnableViewState` i `Visible` właściwości w celu `false`i jego `Text` właściwości do najpierw wybierz dostawcę z siatki. Klasa CSS `Warning` tekst jest wyświetlany czcionką czerwony, pogrubienie, kursywa duże i jest definiowany w `Styles.css`. Przez ustawienie `EnableViewState` i `Visible` właściwości w celu `false`, etykiety nie są odtwarzane z wyjątkiem dla tylko te postbacks gdzie formantu s `Visible` programowo ustawiono właściwość `true`.


[![Dodaj kontrolkę etykieta Web powyżej widoku GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Rysunek 13**: Dodaj etykietę w sieci Web kontroli nad GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


Następnie rozszerzyć `Click` programu obsługi zdarzeń, aby wyświetlić `ChooseSupplierMsg` etykiety, jeśli `SuppliersSelectedIndex` jest mniejsza niż zero i przekieruje go do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` inaczej.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Odwiedź stronę w przeglądarce i kliknij przycisk `SendToProducts` przycisk przed wybraniem dostawcy z widoku GridView. Jak pokazano na rysunku 14, spowoduje to wyświetlenie `ChooseSupplierMsg` etykiety. Następnie wybierz dostawcę i kliknij przycisk `SendToProducts` przycisku. Spowoduje to whisk stronę, która zawiera listę produktów, dostarczanych przez wybranego dostawcę. Przedstawia rysunek 15 `ProductsForSupplierDetails.aspx` strony, gdy dostawca browarów Bigfoot został wybrany.


[![Etykieta ChooseSupplierMsg jest wyświetlana, jeśli dostawca nie jest zaznaczone.](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Rysunek 14**: `ChooseSupplierMsg` etykieta jest wyświetlana, jeśli dostawca nie jest zaznaczone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![Wybrany dostawca produktów s są wyświetlane w ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Rysunek 15**: wybrany dostawca s produkty są wyświetlane w `ProductsForSupplierDetails.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Krok 5: Wyświetlanie produktów s wybranego dostawcy w tej samej stronie

Jak wysłać użytkownika do innej strony sieci web, aby wyświetlić wybranego dostawcy s produktów widzieliśmy w kroku 4. Alternatywnie produktów s wybranego dostawcy mogą być wyświetlane na tej samej stronie. Na przykład dodamy GridView innego celu `RadioButtonField.aspx` do wyświetlania produktów wybranego dostawcy s.

Ponieważ chcemy tylko tym GridView produktów do wyświetlenia, gdy dostawca został wybrany, dodać kontrolkę panelu w sieci Web pod `Suppliers` GridView, ustawiając jego `ID` do `ProductsBySupplierPanel` i jego `Visible` właściwość `false`. W panelu, Dodaj tekst produktów dla dostawcy wybrane następuje GridView o nazwie `ProductsBySupplier`. Za pomocą tagu inteligentnego s GridView wybierz powiązać go z nowego elementu ObjectDataSource, o nazwie `ProductsBySupplierDataSource`.


[![Powiązywanie ProductsBySupplier GridView nowe kontrolki ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Rysunek 16**: powiązywanie `ProductsBySupplier` GridView do nowej kontrolki ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


Następnie skonfiguruj ObjectDataSource do użycia `ProductsBLL` klasy. Ponieważ chcemy pobierania tych produktów, dostarczone przez wybranego dostawcę, należy określić, że kontrolki ObjectDataSource powinien wywołuje `GetProductsBySupplierID(supplierID)` metody do pobierania danych. (Brak) wybierz z listy rozwijanej w UPDATE, INSERT i usuwanie kart.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Rysunek 17**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetProductsBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Ustaw list rozwijanych (Brak) w UPDATE, INSERT i usuwanie kart](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Rysunek 18**: Ustaw listy rozwijane (Brak) aktualizacji, WSTAWIANIA i usuwania karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


Po skonfigurowaniu SELECT, aktualizacji, WSTAWIANIA i usuwanie kart, kliknij przycisk Dalej. Ponieważ `GetProductsBySupplierID(supplierID)` metoda oczekuje parametru wejściowego, monituje kreator Utwórz źródło danych, nam określić źródło s wartość tego parametru.

Dostępnych jest kilka opcji, w tym miejscu w określania źródła s wartość tego parametru. Można użyć domyślnego parametru obiektu i programowo przypisze się wartość `SuppliersSelectedIndex` właściwość s parametr `DefaultValue` właściwości w elemencie ObjectDataSource s `Selecting` programu obsługi zdarzeń. Odwołaj się do [programowe Ustawianie wartości parametrów elementu ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) samouczków dla przypomnienia informacji na temat programowego przypisywania wartości parametrów elementu ObjectDataSource s.

Alternatywnie można używane parametrze ControlParameter i mogą odwoływać się do `Suppliers` GridView s [ `SelectedValue` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (zobacz rysunek 19). GridView s `SelectedValue` właściwość zwraca `DataKey` wartość odpowiadającą [ `SelectedIndex` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Aby ta opcja działała, musimy programowo ustawić GridView s `SelectedIndex` właściwości wybranego wiersza, kiedy `ListProducts` przycisku. Jako dodatkowa korzyść, ustawiając `SelectedIndex`, będzie miał wybranego rekordu `SelectedRowStyle` zdefiniowane w `DataWebControls` motyw (żółte tło).


[![Użyj parametrze ControlParameter, aby określić GridView s SelectedValue jako źródło parametru](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Rysunek 19**: Użyj parametrze ControlParameter, aby określić GridView s SelectedValue jako źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


Po ukończeniu kreatora, Visual Studio spowoduje automatyczne dodanie pola dla pola danych s produktu. Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, i `UnitPrice` BoundFields i zmień `HeaderText` właściwości, aby produkt, kategoria i ceny. Konfigurowanie `UnitPrice` elementu BoundField tak, aby jego wartość jest formatowana jako walutę. Po wprowadzeniu tych zmian, Panel, GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Do ukończenia tego ćwiczenia, musimy GridView s `SelectedIndex` właściwości `SelectedSuppliersIndex` i `ProductsBySupplierPanel` panelu s `Visible` właściwości `true` podczas `ListProducts` przycisku. W tym celu należy utworzyć program obsługi zdarzeń dla `ListProducts` formant przycisku w sieci Web s `Click` zdarzeń i Dodaj następujący kod:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Jeśli nie wybrano dostawcę z GridView `ChooseSupplierMsg` jest wyświetlana etykieta i `ProductsBySupplierPanel` panelu ukryte. W przeciwnym razie wybranie dostawcy `ProductsBySupplierPanel` jest wyświetlany i GridView s `SelectedIndex` zaktualizować właściwości.

20 rysunek przedstawia wyniki po został wybrany dostawca browarów Bigfoot i kliknął produktów Pokaż przycisk strony.


[![Produkty dostarczane przez browarów Bigfoot są wymienione na tej samej stronie](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Rysunek 20**: produkty dostarczane przez browarów Bigfoot są wymienione na tej samej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Podsumowanie

Zgodnie z opisem w [Master/szczegółów przy użyciu GridView wzorca można wybierać z DetailView szczegółów](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczek rekordy, które można wybierać z GridView przy użyciu CommandField którego `ShowSelectButton` właściwość jest ustawiona na `true`. Ale CommandField wyświetla jej przyciski jako regularne przycisków, linki lub obrazy. Interfejs użytkownika alternatywnych wybór wiersza jest zapewnienie przycisku radiowego lub zaznacz pole wyboru w każdym wierszu GridView. W tym samouczku zbadaliśmy jak dodawanie kolumny przycisków radiowych.

Niestety Dodawanie kolumny do t nie jest przycisków radiowych jako prostym lub prostą, jak można oczekiwać, że. Nie ma żadnych wbudowanych RadioButtonField dodaną o kliknięcie przycisku, a za pomocą kontrolki RadioButton w sieci Web w obrębie TemplateField wprowadza swój własny zestaw problemów. Na koniec aby zapewnić taki interfejs albo musimy utworzyć niestandardową `DataControlField` klasy lub zwracania się do wstrzykiwania odpowiedni kod HTML do TemplateField podczas `RowCreated` zdarzeń.

Posiadanie zbadano jak dodawanie kolumny przycisków radiowych NAS Włącz naszej uwagi na dodawanie kolumny pól wyboru. Kolumny pól wyboru użytkownik może wybrać co najmniej jeden wiersz GridView i następnie wykonać pewne operacje na wszystkich wybranych wierszy (na przykład wybierając zestaw wiadomości e-mail z klienta e-mail opartego na sieci web, a następnie wybierając można usunąć wszystkie wybrane wiadomości e-mail). W następnym samouczku opisano sposób dodawania takiej kolumny.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został David Suru. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](adding-a-gridview-column-of-checkboxes-cs.md)
