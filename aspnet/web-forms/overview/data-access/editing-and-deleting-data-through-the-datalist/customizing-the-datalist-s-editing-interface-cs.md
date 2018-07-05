---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Dostosowywanie kontrolki DataList przez edytowanie interfejsu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku utworzymy bogatsze interfejsu edycji kontrolki DataList, zawierającej kontrolek DROPDOWNLIST i pola wyboru.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f1af8086fd65f09df3bdb8b5547f7aa779c1f984
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388675"
---
<a name="customizing-the-datalists-editing-interface-c"></a>Dostosowywanie interfejsu edycji DataList (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) lub [Pobierz plik PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> W tym samouczku utworzymy bogatsze interfejsu edycji kontrolki DataList, zawierającej kontrolek DROPDOWNLIST i pola wyboru.


## <a name="introduction"></a>Wprowadzenie

Znaczniki i kontrolki sieci Web w elemencie DataList s `EditItemTemplate` zdefiniowanie własnego interfejsu można edytować. We wszystkich można edytować przykłady DataList możemy ve zbadanie tej pory, można edytować interfejsu ma został złożony z formantów sieci Web w polu tekstowym. W [poprzedni Samouczek](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) poprawiliśmy środowisko użytkownika w czasie edycji przez dodawanie kontrolek weryfikacji.

`EditItemTemplate` Można rozwinąć, aby uwzględnić kontrolki sieci Web innych niż pola tekstowego, takich jak kontrolek DropDownList, RadioButtonLists, kalendarze i tak dalej. Podobnie jak w przypadku pól tekstowych, gdy Dostosowywanie interfejsu edycji, aby uwzględnić inne kontrolki sieci Web, należy stosować następujące czynności:

1. Dodawanie kontrolki sieci Web do `EditItemTemplate`.
2. Aby przypisać odpowiednie wartości pola danych do odpowiedniej właściwości, należy użyć składni wiązania danych.
3. W `UpdateCommand` programu obsługi zdarzeń, programowego dostępu do sieci Web kontroli wartość i przekaż go do odpowiedniej metody LOGIKI.

W tym samouczku utworzymy bogatsze interfejsu edycji kontrolki DataList, zawierającej kontrolek DROPDOWNLIST i pola wyboru. W szczególności, utworzymy DataList, który zawiera informacje o produkcie i zezwala na nazwy produktu s, dostawca, kategoria i nieobsługiwane stan do zaktualizowania (patrz rysunek 1).


[![Interfejs edytowania zawiera pole tekstowe, dwóch kontrolek DROPDOWNLIST i pola wyboru](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Rysunek 1**: interfejs edycji zawiera pole tekstowe, dwóch kontrolek DROPDOWNLIST i pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Krok 1: Wyświetlanie informacji o produkcie

Zanim będziemy mogli utworzyć interfejsu można edytować s DataList, najpierw należy tworzyć interfejsu tylko do odczytu. Zacznij od otwarcia `CustomizedUI.aspx` strony `EditDeleteDataList` folder i ustawienia na stronie, Dodaj kontrolką DataList przy użyciu projektanta, jego `ID` właściwość `Products`. W tagu inteligentnego s DataList należy utworzyć nowe kontrolki ObjectDataSource. Nazwa tej nowej kontrolki ObjectDataSource `ProductsDataSource` i skonfiguruj ją, aby pobrać dane z `ProductsBLL` klasy s `GetProducts` metody. Jak z poprzednich samouczków DataList można edytować, zaktualizujemy informacje o s edytowanych produktu, przechodząc bezpośrednio do warstwy logiki biznesowej. W związku z tym zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Ustawianie list rozwijanych AKTUALIZOWANIA, WSTAWIANIA i usuwania karty (Brak)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Rysunek 2**: Ustaw AKTUALIZOWANIA, WSTAWIANIA i usuwania listy rozwijane karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))


Po skonfigurowaniu kontrolki ObjectDataSource, Visual Studio utworzy domyślną `ItemTemplate` dla zwrócił DataList, zawierający listę nazw i wartości dla każdego pola danych. Modyfikowanie `ItemTemplate` tak, aby ten szablon zawiera nazwy produktu w `<h4>` element wraz z nazwa kategorii, nazwa dostawcy, ceny i stan nieobsługiwane. Ponadto, Dodaj przycisk edycji, upewniając się, że jego `CommandName` właściwość jest ustawiona na edycji. Oznaczeniu deklaracyjnym dla mojego `ItemTemplate` poniżej:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Powyższe znaczników wychodzi poza przy użyciu informacji o produkcie &lt;h4&gt; nagłówka dla nazwy produktu s i cztery kolumny `<table>` dla pozostałych pól. `ProductPropertyLabel` i `ProductPropertyValue` klas CSS, zdefiniowanych w `Styles.css`, zostały omówione w poprzednich samouczkach. Rysunek 3 pokazuje nasz postęp, podczas wyświetlania za pośrednictwem przeglądarki.


[![Jest wyświetlana nazwa, dostawca, kategorii, przerywane, stan i cena każdego produktu](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Rysunek 3**: nazwę, dostawca, kategoria, przerywane, stan i cena każdego produktu zostaną wyświetlone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Krok 2: Dodawanie formantów sieci Web do interfejsu edycji

Pierwszym krokiem tworzenia dostosowanego interfejsu edycji to dodanie wymaganych formantów sieci Web do kontrolki DataList `EditItemTemplate`. W szczególności należy kontrolki DropDownList dla kategorii, drugi dla dostawcy, a pola wyboru dla stanu nieobsługiwane. Ponieważ cena s produktu nie można edytować w tym przykładzie, można w dalszym ciągu wyświetlania go za pomocą formantu sieci Web etykiety.

Dostosowywanie interfejsu edycji, kliknij link Edytuj szablony z tagu inteligentnego DataList s, a następnie wybierz `EditItemTemplate` opcję z listy rozwijanej. Dodawanie do kontrolki DropDownList `EditItemTemplate` i ustaw jego `ID` do `Categories`.


[![Dodawanie kontrolki DropDownList dla kategorii](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Rysunek 4**: Dodawanie kontrolki DropDownList dla kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))


Następnie za pomocą tagu inteligentnego s DropDownList opcję Wybierz źródło danych i utworzyć nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource`. Konfigurowanie tej kontrolki ObjectDataSource używać `CategoriesBLL` klasy s `GetCategories()` — metoda (zobacz rysunek 5). Następnie DropDownList s Data Source Configuration Wizard wyświetla monit dotyczący pola danych, które będą używane dla każdego `ListItem` s `Text` i `Value` właściwości. Ma ekran DropDownList `CategoryName` pola danych i użyj `CategoryID` jako wartość, jak pokazano na rysunku 6.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Rysunek 5**: Utwórz nowy o nazwie elementu ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))


[![Konfigurowanie wyświetlania s DropDownList i wartości pól](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Rysunek 6**: Konfigurowanie s DropDownList wyświetlania i pola wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))


Powtórz kroki umożliwiające utworzenie kontrolki DropDownList dla dostawców w tej serii. Ustaw `ID` dla tej metody DropDownList do `Suppliers` i nadaj jej ObjectDataSource `SuppliersDataSource`.

Po dodaniu dwóch kontrolek DropDownList, Dodaj pole wyboru dla stanu nieobsługiwane i pole tekstowe Nazwa s produktu. Ustaw `ID` pole wyboru i w polu tekstowym pod kątem `Discontinued` i `ProductName`, odpowiednio. Dodaj RequiredFieldValidator, aby upewnić się, że użytkownik udostępnia wartość dla nazwy produktu s.

Na koniec Dodaj przyciski aktualizacji i Anuluj. Należy pamiętać, że dla tych dwóch przycisków należy bezwzględnie, ich `CommandName` właściwości są ustawione na aktualizowanie i anulować, odpowiednio.

Możesz określić układ edycji interfejs w dowolny sposób. Czy mogę ve zgody użyto tych samych cztery kolumny `<table>` układu przy użyciu interfejsu tylko do odczytu, jak poniższej składni deklaratywnej i odpowiednio zmienić zrzut ekranu przedstawia:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![Edytowanie interfejsu jest określone w poziomie, takich jak interfejs tylko do odczytu](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Rysunek 7**: interfejsu edycji kontrolki jest określone w poziomie, takich jak interfejs tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Krok 3: Tworzenie EditCommand i procedury obsługi zdarzeń CancelCommand

Obecnie nie ma powiązań danych składni w `EditItemTemplate` (z wyjątkiem `UnitPriceLabel`, który został skopiowany przez verbatim z `ItemTemplate`). Chwilowo dodamy składnia wiązania danych, ale pierwszym umożliwiają s tworzenie obsługi zdarzeń dla DataList s `EditCommand` i `CancelCommand` zdarzenia. Pamiętamy odpowiedzialność `EditCommand` procedura obsługi zdarzeń jest do renderowania interfejsu edycji elementu DataList został kliknięty przycisk Edytuj, którego, natomiast `CancelCommand` s zadaniem jest przywrócić stan wstępnie edycji kontrolki DataList.

Tworzenie dwóch programów obsługi i je, użyj następującego kodu:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Za pomocą tych dwóch programów obsługi zdarzeń w miejscu, klikając przycisk Edytuj umożliwia wyświetlanie interfejsu edycji, a następnie kliknięcie przycisku Anuluj zwraca edytowany element do trybu tylko do odczytu. Rysunek 8 zawiera kontrolki DataList po kliknięciu przycisk Edytuj s Jacka Chef Gumbo mieszanego. Ponieważ firma Microsoft ve jeszcze, aby dodać wszelkie składnia wiązania danych do interfejsu edycji `ProductName` pole tekstowe jest puste, `Discontinued` pole wyboru jest nie zaznaczone, a pierwsze elementy wybrana w zaufanym `Categories` i `Suppliers` kontrolek DROPDOWNLIST.


[![Kliknięcie przycisku Wyświetla przycisk Edytuj interfejsu edycji](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Rysunek 8**: kliknięcie przycisku Edytuj wyświetla interfejsu edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Krok 4: Dodanie składnia wiązania danych do interfejsu edycji

Aby interfejs edytowania wyświetlić bieżące wartości s produktu, musimy można przypisać wartości pól danych odpowiednie wartości kontrolki sieci Web ze składnią wiązania danych. Składnia wiązania danych mogą być stosowane za pomocą projektanta, przechodząc do ekranu edytowania szablonów i wybierając link Edytuj powiązania danych z sieci Web formanty tagów inteligentnych. Alternatywnie można dodać bezpośrednio w oznaczeniu deklaracyjnym składnia wiązania danych.

Przypisz `ProductName` wartość do pola danych `ProductName` TextBox s `Text` właściwości `CategoryID` i `SupplierID` wartości do pola danych `Categories` i `Suppliers` kontrolek DROPDOWNLIST `SelectedValue` właściwości i `Discontinued` wartość do pola danych `Discontinued` s wyboru `Checked` właściwości. Po wprowadzeniu tych zmian za pomocą projektanta lub bezpośrednio w oznaczeniu deklaracyjnym ponownie stronę za pośrednictwem przeglądarki, a następnie kliknij przycisk Edytuj s Jacka Chef Gumbo mieszanego. Jak pokazano na rysunku nr 9, składnia wiązania danych został dodany bieżące wartości w polu tekstowym, kontrolek DROPDOWNLIST i zaznacz pole wyboru.


[![Kliknięcie przycisku Wyświetla przycisk Edytuj interfejsu edycji](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Rysunek 9**: kliknięcie przycisku Edytuj wyświetla interfejsu edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Krok 5: Zapisywanie zmian s użytkownika w obsłudze zdarzeń elementu UpdateCommand

Gdy użytkownik edytuje produktu i kliknie przycisk Aktualizuj występuje odświeżenie strony i kontrolki DataList s `UpdateCommand` generowane zdarzenie. W przypadku obsługi, musimy odczytywania wartości z kontrolek w sieci Web w `EditItemTemplate` i interfejs za pomocą LOGIKI, aby zaktualizować produkt w bazie danych. Ponieważ ve w poprzednich samouczkach `ProductID` zaktualizowanego produktu jest dostępny za pośrednictwem `DataKeys` kolekcji. Pól wprowadzonych przez użytkownika są dostępne programowo odwołujące się do formantów sieci Web przy użyciu `FindControl("controlID")`, jak pokazano w poniższym kodzie:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

Kod uruchamia przez consulting `Page.IsValid` właściwości, aby upewnić się, że wszystkie formanty sprawdzania poprawności na tej stronie są prawidłowe. Jeśli `Page.IsValid` jest `True`, edytowany produkt s `ProductID` wartości są odczytywane z `DataKeys` kolekcji i wprowadzanie danych, sieci Web kontrolki w `EditItemTemplate` programowo odwołują. Następnie wartości z tych formantów sieci Web są odczytywane w zmiennych, które są następnie przekazywane do odpowiedniego `UpdateProduct` przeciążenia. Po zaktualizowaniu danych, kontrolki DataList jest zwracany stan wstępnie edycji.

> [!NOTE]
> Czy mogę ve pominięcia logiki dodany do programu obsługi wyjątków [obsługi LOGIKI i wyjątków na poziomie warstwy DAL](handling-bll-and-dal-level-exceptions-cs.md) samouczka, aby zachować kod i ten przykład koncentruje się. W charakterze ćwiczenia należy dodać tę funkcjonalność po ukończeniu tego samouczka.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Krok 6: Obsługa CategoryID o wartości NULL i wartości IDDostawcy

Umożliwia bazy danych Northwind `NULL` wartości `Products` tabeli s `CategoryID` i `SupplierID` kolumn. Jednak obecnie obsłużyć nasz edycji t interfejsu `NULL` wartości. Jeśli firma Microsoft podejmie próbę edytowania produktu, który ma `NULL` wartość albo jego `CategoryID` lub `SupplierID` kolumn, przejdziemy `ArgumentOutOfRangeException` komunikat o błędzie podobny do: *"Kategorie" ma SelectedValue, która jest nieprawidłowa, ponieważ uzyskuje ona nie istnieje na liście elementów.* Ponadto s występują obecnie nie możliwości zmiany Kategoria produktu s lub dostawcę z wartości innej niż`NULL` wartość `NULL` jeden.

Aby obsługiwać `NULL` wartości kategorii i dostawcy kontrolek DropDownList, należy dodać dodatkowy `ListItem`. Czy mogę ve wybranego do użycia (Brak) jako `Text` wartość to `ListItem`, ale można zmienić ją na coś innego (na przykład ciągiem pustym) po d chcesz. Na koniec należy pamiętać o ustawieniu kontrolek DROPDOWNLIST `AppendDataBoundItems` do `True`; Jeśli zapomnisz to zrobić, kategorie i dostawców powiązany z metody DropDownList spowoduje zastąpienie-dodawane statycznie `ListItem`.

Po wprowadzeniu tych zmian, znaczniki kontrolek DROPDOWNLIST w elemencie DataList s `EditItemTemplate` powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Statyczne `ListItem` s mogą być dodawane do kontrolki DropDownList za pomocą projektanta lub bezpośrednio za pomocą składni deklaratywnej. Podczas dodawania elementu DropDownList do bazy danych do reprezentowania `NULL` wartość, należy je dodać `ListItem` za pomocą składni deklaratywnej. Jeśli używasz `ListItem` Edytor kolekcji w projektancie, wygenerowany składni deklaratywnej zostaną pominięte `Value` całkowicie ustawienie, jeśli przypisany ciąg pusty, tworzenie oznaczeniu deklaracyjnym, takich jak: `<asp:ListItem>(None)</asp:ListItem>`. Chociaż może to wyglądać nieszkodliwe, brakujący `Value` powoduje, że metody DropDownList używać `Text` wartości właściwości w tym miejscu. Oznacza to, że jeśli to `NULL` `ListItem` jest zaznaczone, wartość (Brak) zostanie podjęta próba ma być przypisane do pola danych produktu (`CategoryID` lub `SupplierID`, w tym samouczku), co spowoduje wyjątek. Poprzez jawne ustawienie `Value=""`, `NULL` produktu zostanie przypisana wartość pola danych, gdy `NULL` `ListItem` jest zaznaczone.


Poświęć chwilę, aby wyświetlić postępach za pośrednictwem przeglądarki. Podczas edytowania produktu, należy pamiętać, że `Categories` i `Suppliers` kontrolek DROPDOWNLIST zarówno mają (Brak) opcja na początku metody DropDownList.


[![Kategorie i dostawców kontrolek DROPDOWNLIST (Brak) opcja obejmują](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Na rysunku nr 10**: `Categories` i `Suppliers` kontrolek DROPDOWNLIST obejmują (Brak) opcja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))


Można zapisać opcji (Brak) jako bazy danych `NULL` wartości, należy powrócić do `UpdateCommand` programu obsługi zdarzeń. Zmiana `categoryIDValue` i `supplierIDValue` zmiennych całkowitych dopuszcza wartości null i przypisać im wartości innych niż `Nothing` tylko wtedy, gdy DropDownList s `SelectedValue` nie jest pusty ciąg:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Dzięki tej zmianie wartości `Nothing` zostaną przekazane do `UpdateProduct` LOGIKI metody, jeśli użytkownik wybrał (Brak) opcję z dowolnej z list rozwijanych, które odpowiada `NULL` bazy danych wartości.

## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy pokazaliśmy, jak utworzyć bardziej złożone DataList interfejs edycji, który zawierał trzech różnych danych wejściowych kontrolki sieci Web, pole tekstowe, dwóch kontrolek DROPDOWNLIST i pola wyboru wraz z kontrolkami walidacji. Podczas kompilowania interfejsu edycji, kroki są takie same, niezależnie od tego, kontrolki sieci Web używane: Rozpocznij, dodając kontrolki sieci Web do DataList s `EditItemTemplate`; użyj składni wiązania danych, aby przypisać odpowiednie wartości pól danych z odpowiednią siecią Web właściwości kontrolek; Ponadto w `UpdateCommand` programu obsługi zdarzeń programowy dostęp do formantów sieci Web i ich odpowiednie właściwości i przekazanie wartości do LOGIKI.

Podczas tworzenia interfejsu edycji tego, czy jego s składa się tylko pól tekstowych lub zbiór innej kontrolki sieci Web, należy poprawnie obsługiwać bazy danych `NULL` wartości. Gdy dla `NULL` s, jest konieczne, aby tylko prawidłowo wyświetlana istniejące `NULL` wartość w interfejsie edycji, ale również tej oferty oznacza, że do oznaczania wartości jako `NULL`. Dla kontrolek DROPDOWNLIST w DataLists, zwykle oznacza to, dodawanie statycznego `ListItem` którego `Value` jest jawnie ustawioną na pusty ciąg (`Value=""`) i aby dodać kod do `UpdateCommand` program obsługi zdarzeń, aby określić, czy `NULL``ListItem` został wybrany.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Dennis Patterson, David Suru i Randy Schmidt. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [dalej](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
