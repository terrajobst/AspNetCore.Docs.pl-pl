---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Dostosowywanie elementu DataList do edycji interfejsu (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku utworzymy bardziej zaawansowane funkcje interfejsu edycji DataList, zawierającej DropDownLists i pola wyboru.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fbdb4687a23604b9f505bbf782d59a8c78e5a02
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Dostosowywanie interfejsu edycji DataList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) lub [pobierania plików PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> W tym samouczku utworzymy bardziej zaawansowane funkcje interfejsu edycji DataList, zawierającej DropDownLists i pola wyboru.


## <a name="introduction"></a>Wprowadzenie

Znaczniki i kontrolki sieci Web w DataList s `EditItemTemplate` zdefiniować jego interfejs można edytować. We wszystkich edytowalnych przykłady DataList możemy stawienia zbadać pory, można edytować interfejs ma zostały składa się z kontrolki TextBox w sieci Web. W [poprzedniego samouczek](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) możemy udoskonalone środowisko użytkownika w czasie edycji przez dodawanie formantów sprawdzania poprawności.

`EditItemTemplate` Można rozwinąć, aby uwzględnić formantów sieci Web innego niż pole tekstowe, takie jak DropDownLists, RadioButtonLists, kalendarzy i tak dalej. Podobnie jak w przypadku pól tekstowych w przypadku dostosowywania edycji interfejsu, który ma zawierać inne formanty sieci Web, należy wykonać następujące czynności:

1. Dodaj formant sieci Web `EditItemTemplate`.
2. Aby przypisać odpowiednie wartości pola danych do odpowiedniej właściwości, należy użyć składni wiązania z danymi.
3. W `UpdateCommand` program obsługi zdarzeń, programowego dostępu do sieci Web kontrolować wartość i przekaż go do odpowiedniej metody logiki warstwy Biznesowej.

W tym samouczku utworzymy bardziej zaawansowane funkcje interfejsu edycji DataList, zawierającej DropDownLists i pola wyboru. W szczególności, utworzymy DataList, który zawiera listę informacji o produkcie i pozwala na nazwę produktu s, dostawca, kategorii i wycofane stan aktualizacji (zobacz rysunek 1).


[![Interfejs edycji zawiera pole tekstowe, dwa DropDownLists i pola wyboru](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Rysunek 1**: interfejs edycji zawiera pole tekstowe, dwa DropDownLists i pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Krok 1: Wyświetlanie informacji o produkcie

Zanim można utworzyć interfejsu można edytować s DataList, musimy najpierw utworzyć interfejs tylko do odczytu. Uruchamianie przez otwarcie `CustomizedUI.aspx` strony z `EditDeleteDataList` folderu i ustawienia na stronie, Dodaj DataList przy użyciu projektanta, jego `ID` właściwości `Products`. Utworzyć nowy element ObjectDataSource tagów inteligentnych s DataList. Nazwa tego nowego elementu ObjectDataSource `ProductsDataSource` i skonfigurować go, aby pobrać dane z `ProductsBLL` klasy s `GetProducts` metody. Jako z poprzedniej edycji samouczki DataList będziemy informować informacji s edytowanych produktu, przejdź bezpośrednio do warstwy logiki biznesowej. W związku z tym zestawu list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak).


[![Ustawianie list rozwijanych UPDATE, INSERT i DELETE karty na (Brak)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Rysunek 2**: Ustaw AKTUALIZOWANIA, WSTAWIANIA i Usuń z listy rozwijane karty na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Po skonfigurowaniu ObjectDataSource, Visual Studio utworzy domyślną `ItemTemplate` dla zwrócił DataList, która zawiera nazwę i wartość dla każdego pola danych. Modyfikowanie `ItemTemplate` tak, aby szablon Wyświetla nazwę produktu w `<h4>` element wraz z nazwa kategorii, nazwę dostawcy, ceny i stan wycofane. Ponadto Dodaj przycisk Edytuj, upewniając się, że jego `CommandName` właściwość jest ustawiona na edycję. Deklaracyjne kod znaczników dla moich `ItemTemplate` następuje:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Powyżej znaczników wychodzi poza informacji produkt za pomocą &lt;h4&gt; nagłówek dla nazwy produktu s i cztery kolumny `<table>` w pozostałych polach. `ProductPropertyLabel` i `ProductPropertyValue` klas CSS, zdefiniowanych w `Styles.css`, zostały omówione w poprzednich samouczki. Rysunek 3 przedstawia naszych postępu podczas wyświetlania za pośrednictwem przeglądarki.


[![Nazwa, dostawca, kategoria, zaprzestać stanu i ceny każdego produktu są wyświetlane](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Rysunek 3**: Name, dostawca, kategoria, zaprzestać stanu i ceny każdego produktu są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Krok 2: Dodawanie formantów sieci Web do edycji interfejsu

Pierwszym krokiem tworzenia dostosowanego interfejsu edycji jest dodanie wymaganych formantów sieci Web do elementu DataList `EditItemTemplate`. W szczególności potrzebujemy DropDownList dla kategorii, jedno dla dostawcy i wyboru wycofane stanu. Ponieważ nie można edytować w tym przykładzie cena s produktu, może w dalszym ciągu wyświetlania za pomocą formantu etykiety w sieci Web.

Aby dostosować interfejs edytowania, kliknij łącze Edytuj szablony z tagów inteligentnych DataList s, a następnie wybierz pozycję `EditItemTemplate` opcję z listy rozwijanej. Dodaj DropDownList do `EditItemTemplate` i ustawić jej `ID` do `Categories`.


[![Dodaj DropDownList dla kategorii](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Rysunek 4**: Dodaj DropDownList dla kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Następnie z tagów inteligentnych s DropDownList opcję Wybierz źródło danych i utworzyć nowy element ObjectDataSource o nazwie `CategoriesDataSource`. Skonfiguruj ten element ObjectDataSource do użycia `CategoriesBLL` klasy s `GetCategories()` — metoda (patrz rysunek 5). Następnie DropDownList s Kreator konfiguracji źródła danych wyświetla monit dotyczący pola danych, które będą używane dla każdego `ListItem` s `Text` i `Value` właściwości. Wyświetlone DropDownList `CategoryName` pola danych i użyj `CategoryID` jako wartość, jak pokazano na rysunku 6.


[![Utwórz nowy element ObjectDataSource o nazwie CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Rysunek 5**: Utwórz nowy składnik o nazwie ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Konfigurowanie wyświetlania s DropDownList i wartość pola](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Rysunek 6**: Konfigurowanie s DropDownList wyświetlania i pola wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Powtórz następujące kroki, aby utworzyć DropDownList dla dostawcy. Ustaw `ID` dla tego DropDownList do `Suppliers` i nazwy jej ObjectDataSource `SuppliersDataSource`.

Po dodaniu dwóch DropDownLists, Dodaj pole wyboru dla stanu wycofane i pole tekstowe nazwy s produktu. Ustaw `ID` s dla pola wyboru i pola tekstowego do `Discontinued` i `ProductName`odpowiednio. Dodaj RequiredFieldValidator, aby upewnić się, że użytkownik zawiera wartość dla nazwy produktu s.

Na koniec Dodaj przyciski aktualizacji i Anuluj. Należy pamiętać, że te dwa przycisków należy bezwzględnie który ich `CommandName` właściwości są ustawione, aby zaktualizować i anulować odpowiednio.

Możesz także do określania układu interfejs edytowania, jednak chcesz. I Zapisz postanowił używać tego samego cztery kolumny `<table>` układu w interfejsie tylko do odczytu jako następującej składni deklaratywnej i zrzut ekranu przedstawia:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Edytowanie interfejsu jest ustalić limit, takich jak interfejs tylko do odczytu](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Rysunek 7**: interfejs edycji jest ustalić limit, takich jak interfejs tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Krok 3: Tworzenie EditCommand i procedury obsługi zdarzeń CancelCommand

Obecnie, nie istnieje żadna składnia wiązania z danymi w `EditItemTemplate` (z wyjątkiem `UnitPriceLabel`, która została skopiowana przez verbatim z `ItemTemplate`). Dodamy składnia wiązania z danymi na chwilę, ale pierwszym let s tworzenie obsługi zdarzeń dla elementu DataList s `EditCommand` i `CancelCommand` zdarzenia. Odwołania, który jest obowiązkiem `EditCommand` procedura obsługi zdarzeń jest do renderowania interfejs edytowania elementu DataList kliknięto przycisk Edytuj, którego, podczas gdy `CancelCommand` jest zadanie s, aby przywrócić stan wstępnie edycji elementu DataList.

Tworzenie obsługi tych dwóch zdarzeń oraz ich użyć poniższego kodu:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Z tych dwóch obsługi zdarzeń w miejscu, klikając przycisk Edytuj wyświetla interfejs edytowania i kliknięcie przycisku Anuluj zwraca edytowany element do trybu tylko do odczytu. Rysunek nr 8 przedstawia elementu DataList po kliknięto przycisk Edytuj Jacka Chef s Gumbo mieszanego. Ponieważ firma Microsoft kolejnych jeszcze, aby dodać wszystkie składnia wiązania z danymi do edycji interfejsu `ProductName` pole tekstowe jest puste, `Discontinued` niezaznaczone pole wyboru, a pierwszym elementy wybrane z `Categories` i `Suppliers` DropDownLists.


[![Kliknięcie przycisku przedstawia przycisk Edytuj edycji interfejsu](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Rysunek 8**: kliknięcie przycisku Edytuj wyświetla interfejs edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Krok 4: Dodawanie składnia wiązania z danymi do edycji interfejsu

Aby interfejs edytowania wyświetlane bieżące wartości s produktu, musimy można przypisać wartości pola danych odpowiednie wartości formantu sieci Web ze składnią wiązania z danymi. Składnia wiązania z danymi można zastosować za pomocą projektanta, przechodząc do ekranu Edytuj szablony i kliknąć łącze edycji powiązania danych w sieci Web kontroluje tagów inteligentnych. Alternatywnie można dodać bezpośrednio do deklaratywne znaczników składnia wiązania z danymi.

Przypisz `ProductName` wartości pola danych `ProductName` s pole tekstowe `Text` właściwości, `CategoryID` i `SupplierID` wartości pola danych `Categories` i `Suppliers` DropDownLists `SelectedValue` właściwości oraz `Discontinued` wartości pola danych `Discontinued` s wyboru `Checked` właściwości. Po wprowadzeniu tych zmian, za pomocą projektanta, lub bezpośrednio za pomocą znacznika deklaratywne, ponownie strony za pośrednictwem przeglądarki, a następnie kliknij przycisk Edytuj Jacka Chef s Gumbo mieszanego. Jak pokazano na rysunku nr 9, składnia wiązania danych został dodany bieżące wartości do pola tekstowego, DropDownLists i pola wyboru.


[![Kliknięcie przycisku przedstawia przycisk Edytuj edycji interfejsu](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Rysunek 9**: kliknięcie przycisku Edytuj wyświetla interfejs edycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Krok 5: Zapisywanie zmian s użytkownika w obsłudze zdarzeń UpdateCommand

Gdy użytkownik edytuje produktu i kliknie przycisk Aktualizuj występuje odświeżania strony i DataList s `UpdateCommand` generowane zdarzenie. W przypadku obsługi, należy odczytać wartości z formantów sieci Web w `EditItemTemplate` i interfejs z logiki warstwy Biznesowej w celu zaktualizowania produktu w bazie danych. Jak możemy kolejnych w poprzedniej samouczki `ProductID` zaktualizowane produktu jest dostępny za pośrednictwem `DataKeys` kolekcji. Pola wprowadzonych przez użytkownika są dostępne dla programowo odwołujące się do formantów sieci Web przy użyciu `FindControl("controlID")`, jak pokazano w następującym kodem:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Kod rozpoczyna się przez konsultacji `Page.IsValid` właściwości, aby upewnić się, że wszystkie formanty walidacji na tej stronie są prawidłowe. Jeśli `Page.IsValid` jest `True`, edytowanych produktu s `ProductID` odczytać z wartości `DataKeys` kolekcji i wprowadzania danych formantów sieci Web w `EditItemTemplate` odwołuje się programowo. Następnie wartości z tych kontrolek sieci Web są odczytywane do zmiennych, które są następnie przekazywane do odpowiedniej `UpdateProduct` przeciążenia. Po zaktualizowaniu danych, zwracany jest stanu wstępnie edycji elementu DataList.

> [!NOTE]
> I Zapisz pominięcia logiki dodany do programu obsługi wyjątków [obsługi logiki warstwy Biznesowej i wyjątków na poziomie warstwy DAL](handling-bll-and-dal-level-exceptions-vb.md) samouczek, aby zapewnić kod i w tym przykładzie fokus. Ten samouczek wykonywania dodanie tej funkcji.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Krok 6: Obsługa wartości IDDostawcy i PUSTYM identyfikatorem kategorii

Umożliwia bazy danych Northwind `NULL` wartości `Products` tabeli s `CategoryID` i `SupplierID` kolumn. Jednak obecnie obsłużyć naszych edycji t interfejsu `NULL` wartości. Firma Microsoft podejmie próbę edycji produkt, który ma `NULL` wartość albo jego `CategoryID` lub `SupplierID` kolumn, możemy uzyskać `ArgumentOutOfRangeException` komunikat o błędzie podobny do: *"Kategorie" ma SelectedValue, co jest nieprawidłowe, ponieważ uzyskuje ona nie istnieje na liście elementów.* Ponadto s występują obecnie nie można zmienić kategorii produktów s lub dostawcę wartość z innej`NULL` do wartości `NULL` jeden.

Do obsługi `NULL` wartości kategorii i dostawcy DropDownLists, należy dodać kolejny `ListItem`. I Zapisz wybranego do użycia (Brak) jako `Text` wartość to `ListItem`, ale można zmienić na inny (na przykład ciąg pusty) Jeśli d. Na koniec należy pamiętać o ustawieniu DropDownLists `AppendDataBoundItems` do `True`; Jeśli zapomnisz to zrobić, kategorie i dostawców powiązany DropDownList spowoduje zastąpienie-dodawane statycznie `ListItem`.

Po wprowadzeniu tych zmian, znaczników DropDownLists w DataList s `EditItemTemplate` powinien wyglądać podobnie do następującego:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statyczne `ListItem` s mogą być dodawane do DropDownList poprzez projektanta lub bezpośrednio za pomocą składni deklaratywnej. Podczas dodawania elementu DropDownList do reprezentowania bazy danych `NULL` wartości, należy dodać `ListItem` za pomocą składni deklaratywnej. Jeśli używasz `ListItem` edytora kolekcji w projektancie, zostaną pominięte w wygenerowanym składni deklaratywnej `Value` ustawienie całkowicie kiedy przypisane ciąg pusty deklaratywne znaczników, takich jak tworzenie: `<asp:ListItem>(None)</asp:ListItem>`. Gdy to wygląda nieszkodliwe, brakujący `Value` powoduje, że lista DropDownList do użycia `Text` wartości właściwości w tym miejscu. Oznacza to, że jeśli to `NULL` `ListItem` jest zaznaczone, wartość (Brak) nastąpi próba można przypisać do pola danych produktu (`CategoryID` lub `SupplierID`, w tym samouczku), co spowoduje Wystąpił wyjątek. Jawnie ustawiając `Value=""`, `NULL` produktu będzie można przypisać wartości pola danych, gdy `NULL` `ListItem` jest zaznaczone.


Poświęć chwilę, aby wyświetlić postęp naszych za pośrednictwem przeglądarki. Podczas edytowania produkt, należy pamiętać, że `Categories` i `Suppliers` DropDownLists zarówno ma (Brak) opcja na początku DropDownList.


[![Kategorie i dostawców DropDownLists obejmują (Brak) opcja](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Na rysunku nr 10**: `Categories` i `Suppliers` DropDownLists obejmują (Brak) opcja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Można zapisać opcji (Brak) jako bazy danych `NULL` wartości, należy wrócić do `UpdateCommand` obsługi zdarzeń. Zmień `categoryIDValue` i `supplierIDValue` zmienne, które mają być liczbami całkowitymi wartości null i przypisać je wartości innych niż `Nothing` tylko wtedy, gdy DropDownList s `SelectedValue` nie jest pusty ciąg:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Dzięki tej zmianie wartości `Nothing` zostaną przekazane do `UpdateProduct` opcję logiki warstwy Biznesowej metodę, jeśli użytkownik wybrał (Brak) przy użyciu dowolnego z listy rozwijanej, które odpowiada `NULL` bazy danych wartości.

## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy tworzenie bardziej złożonych edycji interfejs DataList, który zawiera trzy różne kontrolki wejściowe sieci Web, pole tekstowe, DropDownLists dwóch i wyboru wraz z formanty walidacji. Podczas konstruowania interfejsu edycji, kroki są takie same, niezależnie od formantów sieci Web, używana: Rozpocznij od dodania formantów sieci Web do DataList s `EditItemTemplate`; użyj składni wiązania z danymi, aby przypisać odpowiednie wartości pola danych z odpowiednią siecią Web właściwości formantów; i w `UpdateCommand` obsługi zdarzeń uzyskania programowego dostępu do formantów sieci Web oraz odpowiednie właściwości, przekazanie wartości do logiki warstwy Biznesowej.

Podczas tworzenia interfejsu edycji, czy jego s składa się tylko pól tekstowych lub zbiór inne formanty sieci Web, należy poprawnie obsługiwać bazy danych `NULL` wartości. Gdy dla `NULL` s, jest konieczne, aby tylko prawidłowo wyświetlana istniejące `NULL` wartość edycji interfejsu, ale które oferują oznacza znakowania wartość jako `NULL`. Dla DropDownLists w DataLists, zwykle oznacza to dodawanie statycznego `ListItem` których `Value` właściwość jest jawnie ustawiona na pusty ciąg (`Value=""`) i dodać kodu do `UpdateCommand` obsługi zdarzeń, aby określić, czy `NULL``ListItem` został wybrany.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały firmy Dennis Patterson Suru Dominik i Randy Schmidt. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
