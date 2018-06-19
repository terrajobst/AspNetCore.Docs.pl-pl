---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Obsługa wyjątków logiki warstwy Biznesowej i warstwy DAL poziom strony ASP.NET (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przedstawiono sposób wyświetlania komunikat o błędzie przyjaznych, informacyjny powinna wystąpić Wystąpił wyjątek podczas wstawiania, aktualizacji lub operacji usuwania programu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b76554b6e8c00dbe3b33de8158b925d7314afb72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886839"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Obsługa wyjątków logiki warstwy Biznesowej i warstwy DAL poziom strony ASP.NET (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) lub [pobierania plików PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> W tym samouczku przedstawiono sposób wyświetlania komunikat o błędzie przyjaznych, informacyjny powinien wystąpić wyjątek podczas wstawiania, aktualizacji lub operacja usuwania danych ASP.NET kontrolka sieci Web.


## <a name="introduction"></a>Wprowadzenie

Pracę z danymi z aplikacji sieci web ASP.NET przy użyciu architektury aplikacji warstwowych obejmuje trzy następujące ogólne czynności:

1. Określ metodę, warstwy logiki biznesowej musi być wywoływane i wartości jakie parametr przekazywany. Wartości parametrów, które mogą być zakodowany, programowo przypisane lub danych wejściowych wprowadzony przez użytkownika.
2. Wywołanie metody.
3. Przetworzyć wyników. Podczas wywoływania metody logiki warstwy Biznesowej, która zwraca dane, może to obejmować powiązanie danych z danych formantu sieci Web. Dla metod logiki warstwy Biznesowej, które modyfikują dane może to obejmować wykonywanie niektórych akcji na podstawie wartości zwracane lub bezpiecznie obsługi wszelkie wyjątki, które powstały w kroku 2.

Jak widzieliśmy w [poprzedniego samouczek](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), zarówno w elemencie ObjectDataSource, jak i dane formantów sieci Web podanie punkty rozszerzeń kroki 1 i 3. Widoku GridView, na przykład generowane jego `RowUpdating` zdarzeń przed przypisanie jej wartości pól do jego ObjectDataSource `UpdateParameters` kolekcji; jego `RowUpdated` zdarzenie jest wywoływane po elemencie ObjectDataSource ukończył operację.

Firma Microsoft już zostały zbadane zdarzeń, które wyzwalać w kroku 1 i jak już wspomniano jak one używane do dostosowywania parametrów wejściowych, lub Anuluj operację. W tym samouczku firma Microsoft będzie Włącz naszych uwagę na zdarzenia, które wyzwalać po ukończeniu tej operacji. Z te programy obsługi zdarzeń po poziom, którego możemy, między innymi określić, czy wystąpił wyjątek podczas operacji i go bezpiecznie obsłużyć, wyświetlanie komunikat o błędzie przyjaznych, informacyjny na ekranie, a nie przyjęty standardowe ASP.NET Strona wyjątku.

Aby zilustrować pracy z tych zdarzeń po poziomu, Utwórzmy strona, która zawiera listę produktów w można edytować widoku GridView. Podczas aktualizowania produktu, jeśli wyjątek jest wywoływane naszych ASP.NET będą wyświetlane krótką wiadomość powyżej widoku GridView wyjaśniający, że wystąpił problem. Dzieła!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Krok 1: Tworzenie można edytować GridView produktów

W poprzednich instrukcji utworzyliśmy edytowalny Element GridView z dwóch pól `ProductName` i `UnitPrice`. To wymagane dodatkowe przeciążenia dla tworzenia `ProductsBLL` klasy `UpdateProduct` metoda, która akceptowany tylko trzy parametry wejściowe (produktu nazwa cenie jednostkowej i identyfikator) nazwą parametru dla każdego pola produktu. W tym samouczku umożliwia rozwiązaniem, ta technika, tworzenie można edytować widoku GridView, wyświetlana nazwa produktu, ilość na jednostkę, cenie jednostkowej oraz jednostki w magazynie, ale zezwala tylko nazwę, cenie jednostkowej i jednostki w magazynie do edycji.

Aby zmieścił się w tym scenariuszu poprosimy Cię o innego przeciążenia metody `UpdateProduct` metoda, która przyjmuje cztery parametry: Nazwa produktu, cenie jednostkowej, jednostki w magazynie i identyfikator. Dodaj następującą metodę do `ProductsBLL` klasy:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Ta metoda pełną jest gotowy do utworzenia strony ASP.NET, która służy do edytowania czterech pól danego produktu. Otwórz `ErrorHandling.aspx` strony `EditInsertDelete` folderu i Dodaj element GridView do strony przy użyciu projektanta. Powiązać widoku GridView nowy element ObjectDataSource, mapowanie `Select()` metodę `ProductsBLL` klasy `GetProducts()` — metoda i `Update()` metodę `UpdateProduct` przeciążenia właśnie utworzony.


[![Użyj przeciążenia metody UpdateProduct, które przyjmuje cztery parametry wejściowe](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Rysunek 1**: Użyj `UpdateProduct` metoda przeciążenia czy przyjmuje cztery parametry wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Spowoduje to utworzenie elementu ObjectDataSource z `UpdateParameters` kolekcji z cztery parametry i GridView z polem dla każdego pola produktu. Przypisuje znaczników deklaracyjne element ObjectDataSource `OldValuesParameterFormatString` właściwości wartość `original_{0}`, który spowoduje, że wyjątek od klasy Nasze logiki warstwy Biznesowej oczekują, że parametr wejściowy o nazwie `original_productID` , należy przesłać. Nie zapomnij Usuń ustawienie to całkowicie z składni deklaratywnej (lub ustaw ją na wartość domyślną `{0}`).

Następnie nawiasu dół widoku GridView, aby uwzględnić tylko `ProductName`, `QuantityPerUnit`, `UnitPrice`, i `UnitsInStock` BoundFields. Również możesz zastosować na poziomie pola formatowania danych konieczne (np. zmiana `HeaderText` właściwości).

W poprzednich instrukcji Analizujemy sposób formatowania `UnitPrice` elementu BoundField jako walutę zarówno w trybie tylko do odczytu i w trybie edycji. Poznajmy tej samej lokalizacji. Odwołaj to wymagane ustawienie elementu BoundField `DataFormatString` właściwości `{0:c}`, jego `HtmlEncode` właściwości `false`, a jego `ApplyFormatInEditMode` do `true`, jak pokazano na rysunku 2.


[![Konfigurowanie elementu UnitPrice BoundField do wyświetlania jako walutę](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Rysunek 2**: Konfigurowanie `UnitPrice` elementu BoundField do wyświetlania jako walutę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Formatowanie `UnitPrice` jako walutę w interfejsie edycji wymaga tworzenie obsługi zdarzeń dla w widoku GridView `RowUpdating` zdarzeń, która analizuje ciąg w formacie waluty w `decimal` wartość. Odwołania, który `RowUpdating` obsługi zdarzeń od ostatniego samouczek również sprawdzane pod kątem upewnij się, że podane przez użytkownika `UnitPrice` wartość. Jednak w tym samouczku umożliwia Zezwalaj użytkownikowi na pominięcie cenę.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Nasze GridView obejmuje `QuantityPerUnit` elementu BoundField, ale tego elementu BoundField powinny być tylko do wyświetlania i nie może być edytowany przez użytkownika. W tym wystarczy ustawić BoundFields `ReadOnly` właściwości `true`.


[![Nadawanie elementu QuantityPerUnit BoundField tylko do odczytu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Rysunek 3**: należy `QuantityPerUnit` elementu BoundField tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Na koniec pole wyboru Włącz edytowanie z tagów inteligentnych w widoku GridView. Po wykonaniu tych czynności `ErrorHandling.aspx` strony projektanta powinien wyglądać podobnie do rysunek 4.


[![Usuń wszystkie oprócz wymagane BoundFields i zaznacz pole wyboru Włącz edytowanie wyboru](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Rysunek 4**: Usuń wszystkie elementy oprócz potrzebne BoundFields i zaznacz pole wyboru Włącz edytowanie pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


W tym momencie mamy listę wszystkich produktów `ProductName`, `QuantityPerUnit`, `UnitPrice`, i `UnitsInStock` pola; jednak tylko `ProductName`, `UnitPrice`, i `UnitsInStock` pola można edytowane.


[![Użytkownicy teraz można łatwo edytować, nazwy produktów, ceny i jednostki w standardowych pól](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Rysunek 5**: nazwy użytkowników mogą teraz łatwo edytować produktów, ceny i pól jednostki w magazynie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Krok 2: Bezpiecznie obsługi wyjątków na poziomie warstwy DAL

Gdy naszych można edytować widoku GridView działa bajeczną podczas wprowadzania wartości nazwy produktu edytowany, ceny i jednostki w magazynie, wprowadzanie wartości niedozwolony powoduje wygenerowanie wyjątku. Na przykład, pomijając `ProductName` wartość przyczyny [nonullallowedexception —](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) zostanie wygenerowany od `ProductName` właściwości w `ProdcutsRow` klasa ma jego `AllowDBNull` ustawioną właściwość `false`; Jeśli Baza danych nie działa, `SqlException` zgłoszony przez TableAdapter podczas próby nawiązania połączenia z bazą danych. Bez podejmowania żadnych działań, te wyjątki bąbelkowy się z warstwy dostępu do danych do warstwy logiki biznesowej, a następnie do strony ASP.NET i w końcu do środowiska wykonawczego programu ASP.NET.

W zależności od konfiguracji aplikacji sieci web i czy odwiedzasz aplikacji z `localhost`, nieobsługiwany wyjątek może spowodować strony ogólny błąd serwera, raport szczegóły błędu lub strony sieci web przyjaznych dla użytkownika. Zobacz [sieci Web obsługi błędu aplikacji, w programie ASP.NET](http://www.15seconds.com/issue/030102.htm) i [customErrors Element](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) uzyskać więcej informacji na temat sposobu środowiska uruchomieniowego ASP.NET odpowiada nieprzechwycony wyjątek.

Rysunek 6 przedstawia ekranu napotkanych podczas próby zaktualizowania produktu bez określania `ProductName` wartość. Jest to domyślny raport szczegółowe informacje o błędzie wyświetlany po wznowieniu za pośrednictwem `localhost`.


[![Pominięcie szczegóły wyjątku będzie wyświetlana nazwa produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Rysunek 6**: pominięcie szczegóły wyjątku wyświetlana będzie nazwa produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Takie szczegóły wyjątku są przydatne podczas testowania aplikacji, prezentacja użytkownik końcowy ekran w wypadku wyjątków jest mniejsza niż idealne. Prawdopodobnie użytkownik końcowy nie może ustalić, jakie `NoNullAllowedException` jest i dlaczego został spowodowany. Lepszym rozwiązaniem jest do zaoferowania użytkownikowi bardziej przyjazny komunikat wyjaśniający, że wystąpiły problemy podczas próby aktualizacji produktu.

Jeśli wystąpi wyjątek podczas wykonywania operacji, po poziomu zdarzeń w elemencie ObjectDataSource i kontroli danych w sieci Web pozwalają wykryć i anulować wyjątku z propagacji do środowiska wykonawczego programu ASP.NET. W naszym przykładzie załóżmy tworzenie obsługi zdarzeń dla w widoku GridView `RowUpdated` zdarzenie, które określa, jeśli wyjątek jest uruchamiany i jeśli tak, Wyświetla szczegóły wyjątku w formancie etykiety w sieci Web.

Rozpocznij od dodania etykietę do strony ASP.NET, ustawienie jej `ID` właściwości `ExceptionDetails` i wyczyszczenie jego `Text` właściwości. Aby narysować oka użytkownika do tej wiadomości, ustaw jej `CssClass` właściwości `Warning`, który jest dodana do klasy CSS `Styles.css` plików w poprzednich instrukcji. Odwołaj się, że ta klasa CSS powoduje, że tekst etykiety będzie wyświetlana w czcionce czerwony, pogrubienie, kursywa bardzo duża.


[![Na stronie Dodaj formant etykiety sieci Web](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Rysunek 7**: Dodawanie formantu etykiety sieci Web do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Ponieważ chcemy ten formant etykiety w sieci Web, aby były widoczne tylko natychmiast po Wystąpił wyjątek, ustaw jej `Visible` właściwości na wartość false w `Page_Load` obsługi zdarzeń:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Z tego kodu, w pierwszej wizyty strony i kolejne ogłaszania zwrotnego `ExceptionDetails` kontrolka będzie miała jego `Visible` ustawioną właściwość `false`. W wypadku wyjątek poziomie DAL lub logiki warstwy Biznesowej, które firma Microsoft może wykryć, że w widoku GridView `RowUpdated` program obsługi zdarzeń, możemy ustawi `ExceptionDetails` formantu `Visible` właściwości na wartość true. Ponieważ programy obsługi zdarzeń formantu sieci Web wystąpić po `Page_Load` program obsługi zdarzeń w cyklu życia strony, będzie można wyświetlić etykiety. Jednak na następny ogłaszania zwrotnego `Page_Load` obsługi zdarzeń zostaną przywrócone `Visible` właściwości z powrotem do `false`, ukrywając go ponownie z widoku.

> [!NOTE]
> Alternatywnie można usunąć konieczność ustawienie `ExceptionDetails` formantu `Visible` właściwości w `Page_Load` przez przypisanie jej `Visible` właściwości `false` w składni deklaratywnej i wyłączanie swój stan widoku (ustawienie jej `EnableViewState` właściwości `false`). Użyjemy tego podejścia alternatywnego w przyszłości samouczka.


Za pomocą formantu etykiety dodane, naszych następnym krokiem jest tworzenie obsługi zdarzeń dla w widoku GridView `RowUpdated` zdarzeń. Wybierz widoku GridView w projektancie, przejdź do okna właściwości i kliknij ikonę bolt, wyświetlanie zdarzeń w widoku GridView. Powinna już istnieć wpis istnieje w widoku GridView `RowUpdating` zdarzenie, ponieważ utworzono we wcześniejszej części tego samouczka program obsługi zdarzeń dla tego zdarzenia. Tworzenie procedury obsługi zdarzeń dla `RowUpdated` również zdarzeń.


![Tworzenie procedury obsługi zdarzeń dla zdarzenia RowUpdated w widoku GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Rysunek 8**: Tworzenie procedury obsługi zdarzeń dla w widoku GridView `RowUpdated` zdarzeń


> [!NOTE]
> Można również utworzyć programu obsługi zdarzeń za pomocą listy rozwijanej w górnej części pliku klasy związane z kodem. Wybierz z listy rozwijanej w lewym widoku GridView i `RowUpdated` zdarzenia z elementem po prawej stronie.


Tworzenie ten program obsługi zdarzeń Dodaj następujący kod do klasy związane z kodem strony ASP.NET:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Drugi parametr wejściowy tej obsługi zdarzeń jest obiektem typu [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), która zawiera trzy właściwości istotnych dla obsługi wyjątków:

- `Exception` Odwołanie do zgłoszenia wyjątku; Jeśli żaden wyjątek nie były zgłaszane, ta właściwość będzie mieć wartość `null`
- `ExceptionHandled` wartość logiczna, która wskazuje, czy wyjątek został obsłużony w `RowUpdated` obsługi zdarzeń; Jeśli `false` (ustawienie domyślne), wyjątek jest zgłoszony ponownie wypływająca do środowiska wykonawczego programu ASP.NET
- `KeepInEditMode` Jeśli ustawiono `true` edytowanych wiersza elementu GridView pozostaje w trybie edycji; Jeśli `false` (domyślna), wiersza elementu GridView powróci do trybu tylko do odczytu

Naszego kodu, następnie należy sprawdzić, czy `Exception` nie jest `null`, co oznacza, że wystąpił wyjątek podczas wykonywania operacji. Jeśli tak jest, chcemy się:

- Wyświetla przyjazną dla użytkownika komunikat w `ExceptionDetails` etykiety
- Wskazuje, czy wyjątek został obsłużony.
- Zachowaj wiersza elementu GridView w trybie edycji

Ten kod w następujących wykonuje następujące cele:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Ten program obsługi zdarzeń rozpoczyna się od sprawdzenia, czy `e.Exception` jest `null`. Jeśli nie, `ExceptionDetails` etykiety `Visible` właściwość jest ustawiona na `true` i jego `Text` dla właściwości "Wystąpił problem podczas aktualizowania produktu." Szczegóły faktyczny wyjątek, który został zgłoszony znajdują się w `e.Exception` obiektu `InnerException` właściwości. Ten wyjątek wewnętrzny się zbadana i, jeśli jest określonego typu, komunikat dodatkowe, pomocne jest dołączany do `ExceptionDetails` etykiety `Text` właściwości. Ponadto `ExceptionHandled` i `KeepInEditMode` właściwości są ustawione na `true`.

Na rysunku nr 9 przedstawiono zrzut ekranu strony podczas pomijania nazwa produktu; Rysunek nr 10 przedstawia wyniki, wprowadzając niedozwolony `UnitPrice` wartość (-50).


[![Pole ProductName BoundField musi zawierać wartość](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Rysunek 9**: `ProductName` elementu BoundField musi zawierać wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![Wartości ujemne UnitPrice są niedozwolone](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Na rysunku nr 10**: ujemna `UnitPrice` wartości są niedozwolone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


Przez ustawienie `e.ExceptionHandled` właściwości `true`, `RowUpdated` program obsługi zdarzeń wskazuje, że obsłużyła wyjątek. W związku z tym wyjątku nie są propagowane do środowiska wykonawczego programu ASP.NET.

> [!NOTE]
> Rysunki 9 i 10 Pokaż łagodne sposób obsługi wyjątków zgłoszonych z powodu nieprawidłowego użytkownika w danych wejściowych. Najlepiej, jeśli jednak takie nieprawidłowe dane wejściowe nigdy nie będzie reach warstwy logiki biznesowej w pierwszej kolejności, jak strony ASP.NET należy upewnij się, czy dane wejściowe użytkownika są prawidłowe przed wywołaniem `ProductsBLL` klasy `UpdateProduct` metody. W naszym samouczku dalej zajmiemy się tym, jak dodać formanty walidacji do edycji i wstawianie interfejsów upewnij się, że dane przesłane do warstwy logiki biznesowej jest zgodna z reguły biznesowe. Formanty walidacji nie tylko zapobiec wywołanie `UpdateProduct` metody do momentu dane dostarczone przez użytkownika jest prawidłowy, ale również zapewnia większą przyjazność pracę użytkownika identyfikowanie problemów zapisu danych.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Krok 3: Bezpiecznie obsługi wyjątków na poziomie logiki warstwy Biznesowej

Podczas wstawiania, aktualizowania lub usuwania danych, Warstwa dostępu do danych może zgłosić wyjątek w wypadku błędów związanych z danymi. Baza danych może być w trybie offline, kolumnie tabeli wymaganej bazy danych nie mógł zawierać określoną wartość lub ograniczenie poziomu tabeli zostały naruszone. Oprócz wyjątków ściśle związanych z danymi warstwy logiki biznesowej umożliwia wyjątki sygnalizującego, kiedy reguły biznesowe zostały naruszone. W [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md) samouczek, na przykład dodaliśmy wyboru reguła biznesowa oryginalnej `UpdateProduct` przeciążenia. W szczególności jeśli użytkownik został oznaczenie produktu, jak zaprzestać, wprowadziliśmy czy produktu można nie tylko jeden dostarczone przez dostawcę. Jeśli ten stan zostało naruszone, `ApplicationException` został zgłoszony.

Dla `UpdateProduct` przeciążenia utworzonej w tym samouczku, możemy dodać regułę biznesową, która uniemożliwia `UnitPrice` pola na nową wartość, która jest więcej niż dwa razy oryginalnej `UnitPrice` wartość. Aby to zrobić, należy dostosować `UpdateProduct` przeciążenia, który wykonuje sprawdzanie i zgłasza `ApplicationException` Jeśli naruszenia reguły. Zaktualizowana metoda następująco:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Dzięki tej zmianie spowoduje, że każda Aktualizacja cen, która jest więcej niż dwa razy istniejących cen `ApplicationException` zostanie wygenerowany. Podobnie jak wyjątek zgłoszony z warstwy DAL, w tym wywoływane logiki warstwy Biznesowej `ApplicationException` wykrywane i obsługiwane w widoku GridView `RowUpdated` obsługi zdarzeń. W rzeczywistości `RowUpdated` kod obsługi zdarzeń, jak podano, poprawnie wykryje tego wyjątku i wyświetli `ApplicationException`w `Message` wartości właściwości. Rysunek 11 zawiera zrzut, gdy użytkownik próbuje zaktualizować ceny Chai 50,00 USD, czyli więcej niż double jego bieżąca cena cenie od 19,95 USD ekranu.


[![Reguły biznesowe nie zezwalaj na wzrostów, które więcej niż dwukrotnie cena produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Rysunek 11**: nie zezwalaj wzrostów, które więcej niż dwukrotnie cena produktu reguły biznesowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> W idealnym przypadku naszej reguł logiki biznesowej czy refaktoryzowane poza `UpdateProduct` przeciążenia metody i do wspólnej metody. To pole pozostanie jako wykonywania dla czytnika.


## <a name="summary"></a>Podsumowanie

Podczas wstawiania, aktualizowania i usuwania działań formantu danych sieci Web i ObjectDataSource związane wyzwalać zdarzeń przed i po poziomu tego zakładkę bieżącej operacji. Jak widzieliśmy w tym samouczku i mieszanego, podczas pracy z GridView można edytować w widoku GridView `RowUpdating` generowane zdarzenie, a następnie przez element ObjectDataSource `Updating` zdarzeń w takim przypadku polecenie aktualizacji jest wprowadzone w elemencie ObjectDataSource obiekt źródłowy. Po ukończeniu tej operacji, ObjectDataSource `Updated` generowane zdarzenie, a następnie w widoku GridView `RowUpdated` zdarzeń.

Można utworzyć procedury obsługi zdarzeń dla zdarzenia wstępnie poziomu, aby dostosować parametry wejściowe lub po poziomu zdarzeń, aby sprawdzić i reagowanie na wyniki operacji. Programy obsługi zdarzeń po poziomu są najczęściej używane do wykrywania, czy wystąpił wyjątek podczas operacji. Wystąpił wyjątek w wypadku te programy obsługi zdarzeń po poziom opcjonalnie może obsłużyć wyjątek samodzielnie. W tym samouczku widzieliśmy sposób obsługi takiego wyjątku, wyświetlając przyjazne komunikaty o błędach.

W następnym samouczku zajmiemy się tym, jak zmniejszyć prawdopodobieństwo wyjątki wynikające z danych formatowania problemów (takich jak wprowadzenie ujemny `UnitPrice`). W szczególności wyjaśniono, jak dodać formanty walidacji do edycji i wstawianie interfejsów.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Liz Shulok. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [dalej](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
