---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Obsługa wyjątków LOGIKI i warstwy DAL poziomu strony ASP.NET (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku widzimy sposób wyświetlania przyjazny szczegółowy komunikat o błędzie, wystąpi wyjątek podczas wstawiania, aktualizacji lub operacji usuwania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c971b69605055e587aefe92fb4fc755176e6b53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393115"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Obsługa wyjątków LOGIKI i warstwy DAL poziomu strony ASP.NET (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) lub [Pobierz plik PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> W tym samouczku zobaczymy sposób wyświetlania przyjazny szczegółowy komunikat o błędzie, wystąpi wyjątek podczas wstawiania, aktualizacji lub operacja usuwania danych ASP.NET formantu sieci Web.


## <a name="introduction"></a>Wprowadzenie

Praca z danymi z aplikacji sieci web ASP.NET przy użyciu architektury aplikacji warstwowych obejmuje następujące trzy kroki ogólne:

1. Ustal, jakiej metody warstwy logiki biznesowej musi być wywoływany i jakie parametru wartości jego przekazania. Wartość parametru może być zakodowany, programowo przypisane lub danych wejściowych wprowadzonych przez użytkownika.
2. Wywołanie metody.
3. Przetwarzanie wyników. Podczas wywoływania metody LOGIKI, która zwraca dane, może to obejmować wiązanie danych z danymi formantu sieci Web. W przypadku metod LOGIKI, które modyfikują dane może to obejmować wykonywania pewnych akcji na podstawie wartości zwracanej i bez problemu zmieniała obsługi każdego wyjątku, który powstał w kroku 2.

Jak widzieliśmy w [poprzedniego samouczka](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), zarówno w kontrolki ObjectDataSource, jak i w kontrolkach internetowych danych zapewniają punkty rozszerzeń krokach 1 i 3. GridView, na przykład generowane jego `RowUpdating` zdarzeń przed przypisaniem wartości pól do jego kontrolki ObjectDataSource `UpdateParameters` kolekcji; jej `RowUpdated` zdarzenie jest wywoływane po kontrolki ObjectDataSource ukończył operację.

Firma Microsoft została już zbadać zdarzenia, które zostanie wyzwolony w kroku 1 i Zauważyliśmy już, jak one można dostosować parametry wejściowe lub anulować operację. W tym samouczku firma Microsoft będzie włączyć naszej uwagi na zdarzenia, które uruchamiają się po ukończeniu tej operacji. Z tych programów obsługi zdarzeń po poziomu, które możemy, między innymi określić, czy wyjątek wystąpił podczas operacji i go bezpiecznie obsłużyć, wyświetlanie przyjazny szczegółowy komunikat o błędzie na ekranie, zamiast przyjęty standardowe platformy ASP.NET Strona wyjątku.

Aby zilustrować pracę z tych zdarzeń po poziomu, Utwórzmy strona, która zawiera listę produktów w edycji kontrolki GridView. Podczas aktualizacji produktu, jeśli wyjątek jest zgłaszany naszej platformy ASP.NET strony wyświetli krótką wiadomość powyżej GridView wyjaśniające, że wystąpił problem. Zaczynajmy!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Krok 1: Tworzenie można edytować GridView produktów

W poprzednim samouczku utworzyliśmy GridView można edytować za pomocą zaledwie dwóch pól `ProductName` i `UnitPrice`. To wymagane, tworzenie dodatkowych przeciążenie dla elementu `ProductsBLL` klasy `UpdateProduct` metody, który jest akceptowany tylko trzy parametry wejściowe (produktu nazwę, cenę jednostkową i identyfikator) alfanumeryczną parametr dla każdego produktu. W tym samouczku teraz praktyczne tej techniki, tworząc GridView można edytować, który zawiera nazwę produktu, ilość na jednostkę, cenę jednostkową i jednostek w magazynie, ale zezwala tylko nazwę, cenę jednostkową i jednostek w magazynie do edycji.

Aby uwzględnić w tym scenariuszu będziemy potrzebować innego przeciążenia metody `UpdateProduct` metody, która przyjmuje cztery parametry: Nazwa produktu, cena jednostkowa, jednostek w magazynie i identyfikator. Dodaj następującą metodę do `ProductsBLL` klasy:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Przy użyciu tej metody jest pełna możemy przystąpić do tworzenia strony ASP.NET, która umożliwia edytowanie te cztery pola określonego produktu. Otwórz `ErrorHandling.aspx` stronie `EditInsertDelete` folderze i Dodaj GridView na strony za pomocą projektanta. Powiązać widoku GridView nowe kontrolki ObjectDataSource, mapowanie `Select()` metodę, aby `ProductsBLL` klasy `GetProducts()` metody i `Update()` metody `UpdateProduct` przeciążenia, które właśnie utworzony.


[![Użyj przeciążenia metody UpdateProduct, które przyjmuje cztery parametry wejściowe](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Rysunek 1**: Użyj `UpdateProduct` metoda przeciążenia, przyjmuje cztery parametry wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Spowoduje to utworzenie elementu ObjectDataSource z `UpdateParameters` kolekcji za pomocą czterech parametrów i GridView z polem dla każdego z pól produktu. Przypisuje ObjectDataSource oznaczeniu deklaracyjnym `OldValuesParameterFormatString` właściwości wartość `original_{0}`, co spowoduje wyjątek, ponieważ klasy Nasze LOGIKI oczekują, że parametr wejściowy o nazwie `original_productID` przekazać. Nie zapomnij usunąć tego ustawienia całkowicie, od składni deklaratywnej (lub ustaw go na wartość domyślną `{0}`).

Następnie okrojenia GridView w celu uwzględnienia tylko `ProductName`, `QuantityPerUnit`, `UnitPrice`, i `UnitsInStock` BoundFields. Również możesz zastosować formatowanie na poziomie pola można uznać za niezbędne (takie jak zmienianie docelowej `HeaderText` właściwości).

W poprzednim samouczku zobaczyliśmy, jak sformatować `UnitPrice` elementu BoundField jako waluta, zarówno w trybie tylko do odczytu i w trybie edycji. Wykonamy teraz zadania z tym samym miejscu. Pamiętaj, że to ustawienie wymagane elementu BoundField `DataFormatString` właściwości `{0:c}`, jego `HtmlEncode` właściwości `false`, a jego `ApplyFormatInEditMode` do `true`, jak pokazano na rysunku 2.


[![Konfigurowanie elementu UnitPrice BoundField do wyświetlenia jako walutę](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Rysunek 2**: Konfigurowanie `UnitPrice` elementu BoundField do wyświetlenia jako walutę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Formatowanie `UnitPrice` jako walutę w interfejsie edycji wymaga tworzenie obsługi zdarzeń dla GridView `RowUpdating` zdarzeń, która analizuje ciąg w formacie waluty w `decimal` wartość. Pamiętamy `RowUpdating` programu obsługi zdarzeń z ostatnich samouczka również sprawdzany w celu upewnij się, że podane przez użytkownika `UnitPrice` wartość. Jednak w tym samouczku zezwolimy na użytkownika pominąć ceny.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Obejmuje nasze GridView `QuantityPerUnit` elementu BoundField, ale tego elementu BoundField powinny być tylko do wyświetlania i nie powinny być edytowalny przez użytkownika. W tym po prostu ustaw BoundFields `ReadOnly` właściwość `true`.


[![Utworzyć elementu QuantityPerUnit BoundField tylko do odczytu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Rysunek 3**: wprowadzić `QuantityPerUnit` elementu BoundField tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Koniec sprawdź, czy pole wyboru Włącz edytowanie, z GridView tagu inteligentnego. Po wykonaniu tych kroków `ErrorHandling.aspx` strony projektanta powinien wyglądać podobnie do rysunek 4.


[![Usuń wszystkie oprócz potrzebną BoundFields i sprawdź, czy włączyć edytowanie pola wyboru](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Rysunek 4**: Usuń wszystkie elementy oprócz potrzebne BoundFields i sprawdź, czy pole Włącz edycję ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


W tym momencie mamy listę wszystkich produktów `ProductName`, `QuantityPerUnit`, `UnitPrice`, i `UnitsInStock` pola; jednak tylko `ProductName`, `UnitPrice`, i `UnitsInStock` można edytować pola.


[![Użytkownicy teraz można łatwo edytować nazwy produktów, ceny i jednostek standardowych pól](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Rysunek 5**: użytkownicy mogą teraz łatwo edytować produktów nazwy, ceny i jednostek w magazynie pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Krok 2: Bez problemu zmieniała obsługi wyjątków na poziomie warstwy DAL

Natomiast naszych można edytować kontrolki GridView bajeczną działa, gdy użytkownicy wprowadzają wartości prawne na nazwę edytowanego produktu, ceny i jednostek w magazynie, wprowadzanie wartości niedozwolony powoduje wyjątek. Na przykład, pomijając `ProductName` wartość powoduje, że [nonullallowedexception —](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) zostanie wygenerowany od `ProductName` właściwość `ProdcutsRow` klasa ma jego `AllowDBNull` właściwością `false`; Jeśli Baza danych nie działa, `SqlException` zostaną zgłoszone przez TableAdapter, podczas próby połączenia z bazą danych. Bez podejmowania żadnych działań, tych wyjątków się pojawiać z warstwy dostępu do danych do warstwy logiki biznesowej, a następnie na stronie ASP.NET, a na koniec do środowiska uruchomieniowego programu ASP.NET.

W zależności od sposobu skonfigurowania aplikacji sieci web i czy odwiedzasz aplikacji z `localhost`, nieobsługiwanego wyjątku może spowodować strony ogólny błąd serwera, raport szczegółowy komunikat o błędzie lub strony sieci web przyjazna dla użytkownika. Zobacz [obsługę błędów w programie Web aplikacji na platformie ASP.NET](http://www.15seconds.com/issue/030102.htm) i [customErrors Element](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) więcej informacji na temat sposobu na nieprzechwycony wyjątek środowiska uruchomieniowego programu ASP.NET.

Rysunek 6 przedstawia ekranu podczas próby aktualizacji produktu bez określania `ProductName` wartość. Jest to opcja domyślna raport szczegółowy komunikat o błędzie wyświetlany, gdy przechodzącego przez `localhost`.


[![Pominięcie szczegóły wyjątku będzie wyświetlana nazwa produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Rysunek 6**: pomijanie szczegóły wyjątku wyświetlana będzie nazwa produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Takie szczegóły wyjątku są przydatne podczas testowania aplikacji, prezentacja użytkownik końcowy ekran w przypadku wyjątku jest mniej niż idealne rozwiązanie. Użytkownik końcowy prawdopodobnie nie wiedzieli, czego `NoNullAllowedException` jest i dlaczego został spowodowany. Lepszym rozwiązaniem jest przedstawić użytkownikowi bardziej przyjazny dla użytkownika komunikat wyjaśniający, że wystąpiły problemy podczas próby zaktualizowania produktu.

Jeśli wystąpi wyjątek podczas wykonywania operacji, zdarzenia po poziomu zarówno kontrolki ObjectDataSource, jak i kontroli danych w sieci Web pozwalają wykryć je i anulować wyjątek z Propagacja do środowiska uruchomieniowego programu ASP.NET. W tym przykładzie utworzymy program obsługi zdarzeń dla GridView `RowUpdated` zdarzenia, które określa, jeśli wyjątek jest uruchamiany i jeśli tak, Wyświetla szczegóły wyjątku w kontrolce etykiety w sieci Web.

Najpierw dodaj etykietę do strony ASP.NET, ustawiając jego `ID` właściwości `ExceptionDetails` i wyczyszczenie jego `Text` właściwości. Aby narysować oka użytkownika do tej wiadomości, ustaw jego `CssClass` właściwości `Warning`, czyli klasy CSS, dodaliśmy do `Styles.css` pliku w poprzednim samouczku. Pamiętaj, że ta klasa CSS powoduje, że tekst etykiety ma być wyświetlany czcionką czerwony, pogrubienie, kursywa bardzo duże.


[![Na stronie Dodaj kontrolkę typu etykieta w sieci Web](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Rysunek 7**: na stronie Dodaj kontrolkę typu etykieta w sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Ponieważ chcemy, aby ten formant etykiety w sieci Web był widoczny tylko od razu po Wystąpił wyjątek, ustaw jego `Visible` właściwości na wartość false w `Page_Load` programu obsługi zdarzeń:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

Przy użyciu tego kodu, w pierwszej wizyty strony i kolejne ogłaszania zwrotnego `ExceptionDetails` kontrolka będzie miała jego `Visible` właściwością `false`. W przypadku wyjątku poziom warstwy DAL lub LOGIKI możemy wykryć, czy w kontrolce GridView `RowUpdated` programu obsługi zdarzeń, firma Microsoft ustawi `ExceptionDetails` kontrolki `Visible` właściwości na wartość true. Ponieważ programy obsługi zdarzeń kontrolki sieci Web wystąpić po `Page_Load` programu obsługi zdarzeń w cyklu życia strony etykiety będą wyświetlane. Jednakże w przypadku zwrotu dalej `Page_Load` programu obsługi zdarzeń zostaną przywrócone `Visible` właściwości z powrotem do `false`, ukrywając je ponownie z widoku.

> [!NOTE]
> Alternatywnie, można usunąć konieczność ustawienie `ExceptionDetails` kontrolki `Visible` właściwości w `Page_Load` , przypisując jej `Visible` właściwość `false` w składni deklaratywnej i wyłączanie swój stan widoku (ustawienie jego `EnableViewState` właściwość `false`). Użyjemy tego alternatywne podejście w przyszłości zapoznać się z samouczkiem.


Za pomocą formantu etykiety dodawane, naszym kolejnym krokiem będzie można utworzyć programu obsługi zdarzeń dla GridView `RowUpdated` zdarzeń. Wybierz kontrolki GridView w projektancie, przejdź do okna właściwości, a następnie kliknij ikonę pioruna, wyświetlanie zdarzeń w widoku GridView. Powinien już istnieć wpis dla kontrolki GridView `RowUpdating` zdarzeń, jak utworzyliśmy program obsługi zdarzeń dla tego zdarzenia we wcześniejszej części tego samouczka. Utwórz procedurę obsługi zdarzeń dla `RowUpdated` także zdarzenia.


![Tworzenie procedury obsługi zdarzeń dla zdarzenia RowUpdated GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Rysunek 8**: Tworzenie procedury obsługi zdarzeń dla GridView `RowUpdated` zdarzeń


> [!NOTE]
> Można również utworzyć programu obsługi zdarzeń za pomocą listy rozwijanej w górnej części pliku klasy CodeBehind. Z listy rozwijanej, po lewej stronie wybierz widoku GridView i `RowUpdated` zdarzeń od początku po prawej stronie.


Ta procedura obsługi zdarzeń tworzenia Dodaj następujący kod do klasy CodeBehind strony ASP.NET:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Drugi parametr wejściowy tej obsługi zdarzeń jest obiektem typu [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), która ma trzy właściwości znaczenie w odniesieniu do obsługi wyjątków:

- `Exception` Odwołanie do zgłoszenia wyjątku; Jeśli żaden wyjątek został zgłoszony, ta właściwość będzie miała wartość `null`
- `ExceptionHandled` wartość logiczna, która wskazuje, czy wyjątek został obsłużony w `RowUpdated` programu obsługi zdarzeń; Jeśli `false` (ustawienie domyślne), wyjątek jest ponownie zgłoszony wypływająca do środowiska uruchomieniowego platformy ASP.NET
- `KeepInEditMode` Jeśli ustawiono `true` edytowanych wiersza w widoku GridView pozostaje w trybie edycji; Jeśli `false` (ustawienie domyślne), wiersz GridView powróci do trybu tylko do odczytu

Naszym kodzie następnie należy sprawdzić, czy `Exception` nie `null`, co oznacza, że wyjątek został zgłoszony podczas wykonywania operacji. Jeśli jest to możliwe, chcemy:

- Wyświetla przyjazny dla użytkownika komunikat w `ExceptionDetails` etykiety
- Wskazuje, czy wyjątek został obsłużony.
- Zachowaj wiersza w widoku GridView w trybie edycji

Ten poniższy kod w ramach tych celów:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Ta procedura obsługi zdarzeń rozpoczyna się od sprawdzenia, czy `e.Exception` jest `null`. Jeśli nie jest dostępna, `ExceptionDetails` etykiety `Visible` właściwość jest ustawiona na `true` i jego `Text` właściwość "Wystąpił problem podczas aktualizacji produktu." Szczegółowe informacje o faktyczny wyjątek, który został zgłoszony znajdują się w `e.Exception` obiektu `InnerException` właściwości. Ten wyjątek wewnętrzny jest badany i, jeśli jest określonego typu, komunikat o dodatkowe, pomocne jest dołączany do `ExceptionDetails` etykiety `Text` właściwości. Na koniec `ExceptionHandled` i `KeepInEditMode` właściwości są ustawione na `true`.

Gdy nazwa produktu; pomijanie rysunku nr 9 przedstawiono zrzut ekranu strony Na rysunku nr 10 przedstawiono wyniki, wprowadzając niedozwolony `UnitPrice` wartość (-50).


[![Elementu ProductName BoundField musi zawierać wartość](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Rysunek 9**: `ProductName` elementu BoundField musi zawierać wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![Wartości ujemne UnitPrice są niedozwolone](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Na rysunku nr 10**: ujemna `UnitPrice` wartości są niedozwolone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Ustawiając `e.ExceptionHandled` właściwości `true`, `RowUpdated` program obsługi zdarzeń wskazuje, że została zapewniona obsługa wyjątku. W związku z tym wyjątek nie będzie propagowane do środowiska uruchomieniowego programu ASP.NET.

> [!NOTE]
> Rysunki 9 i 10 Pokaż łagodne sposób obsługi wyjątków wyzwalanymi z powodu nieprawidłowego użytkownika danych wejściowych. Najlepiej, jeśli jednak takie nieprawidłowe dane wejściowe nigdy nie będzie zasięg warstwy logiki biznesowej w pierwszej kolejności, jak strony ASP.NET należy upewnić się, że dane wejściowe użytkownika są prawidłowe przed wywołaniem `ProductsBLL` klasy `UpdateProduct` metody. W naszym następnego samouczka, zobaczymy, jak dodawanie kontrolek weryfikacji do interfejsów edycji i wstawianie do upewnij się, że dane przesłane do warstwy logiki biznesowej jest zgodny z reguł biznesowych. Formanty sprawdzania poprawności nie tylko przeszkodzi w wywołaniu `UpdateProduct` metody do momentu dane dostarczone przez użytkownika jest prawidłowy, ale również zapewniają większą przyjazność środowisko użytkownika do identyfikowania problemów zapisu danych.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Krok 3: Bez problemu zmieniała obsługi wyjątków na poziomie LOGIKI

Podczas wstawiania, aktualizowania lub usuwania danych, warstwy dostępu do danych może zgłosić wyjątek w przypadku błędów związanych z danymi. Baza danych może być w trybie offline, kolumnie tabeli wymaganej bazy danych nie może być mogła mieć wartość określoną lub ograniczenie poziomu tabeli zostały naruszone. Oprócz wyjątków ściśle powiązane z danymi warstwy logiki biznesowej można używać wyjątków sygnalizującego, kiedy reguły biznesowe zostały naruszone. W [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md) samouczek, na przykład, dodaliśmy wyboru reguły biznesowe do oryginalnego `UpdateProduct` przeciążenia. Ściślej mówiąc Jeśli użytkownik został oznaczanie produktu, jak wycofana, wprowadziliśmy produktu będzie nie być jedyną dostarczone przez dostawcę. Jeśli ten warunek został naruszony, zwiększyłaby, `ApplicationException` został zgłoszony.

Dla `UpdateProduct` przeciążenia utworzonych w tym samouczku, możemy dodać regułę biznesową, która zakazuje `UnitPrice` pola z ustawiana na nową wartość, która jest więcej niż dwa razy, oryginalnym `UnitPrice` wartość. Aby to osiągnąć, należy dostosować `UpdateProduct` przeciążenia, który wykonuje to sprawdzenie i zgłasza `ApplicationException` Jeśli zostanie naruszona zasada. Zaktualizowana metoda następująco:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Dzięki tej zmianie spowoduje, że każda aktualizacja cenę, która ceną bezpieczeństwa jest więcej niż dwa razy istniejących `ApplicationException` zostanie wygenerowany. Podobnie jak w przypadku wyjątek zgłoszony z warstwy DAL, w tym zgłoszone LOGIKI `ApplicationException` mogą być wykryte i obsługiwane w GridView `RowUpdated` programu obsługi zdarzeń. W rzeczywistości `RowUpdated` kod obsługi zdarzeń w poprawnie wykryje ten wyjątek i wyświetli `ApplicationException`firmy `Message` wartości właściwości. Na ilustracji 11 pokazano zrzutu, gdy użytkownik próbuje zaktualizować cena Chai 50,00 USD, czyli więcej niż double jego bieżąca cena cenie od 19,95 USD ekranu.


[![Reguły biznesowe nie zezwalaj na wzrost cen, które ponad dwukrotnie cena produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Rysunek 11**: firmy reguł wzrostów nie zezwalaj, które ponad dwukrotnie cena produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> Najlepiej logiki reguły biznesowe może być refaktoryzowany poza `UpdateProduct` przeciążenia metody do wspólnej metody. To pole pozostanie w charakterze ćwiczenia dla czytnika.


## <a name="summary"></a>Podsumowanie

Podczas wstawiania, aktualizowania i usuwania działań kontrolki sieci Web danych i kontrolki ObjectDataSource zaangażowane wyzwalać przed i po poziomu zdarzeń tego zakładkę bieżącej operacji. Jak widzieliśmy w tym samouczku i mieszanego, pracując z GridView można edytować w widoku GridView `RowUpdating` generowane zdarzenie, a następnie ObjectDataSource `Updating` zdarzeń, w tym momencie ObjectDataSource wprowadzania polecenia update obiekt źródłowy. Po ukończeniu tej operacji, ObjectDataSource `Updated` aktywowaniu zdarzenia następuje GridView `RowUpdated` zdarzeń.

Można utworzyć procedury obsługi zdarzeń dla zdarzenia wstępnie poziomu, aby dostosować parametry wejściowe lub po utworzeniu zdarzenia na poziomie w celu inspekcji i reagować na wyniki operacji. Programy obsługi zdarzeń po poziomu są najczęściej używane do wykrywania, czy wystąpił wyjątek podczas operacji. W przypadku wyjątku te programy obsługi zdarzeń po poziom opcjonalnie może obsłużyć wyjątek własnych. W tym samouczku widzieliśmy sposób obsługi takiego wyjątku, wyświetlając przyjazny komunikat o błędzie.

W następnym samouczku zobaczymy, jak zmniejszyć prawdopodobieństwo wyjątki powstających problemów formatowania danych (np. wprowadzanie ujemnych `UnitPrice`). W szczególności omówimy sposób dodawania kontrolek weryfikacji do interfejsów edycji i wstawianie.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Liz Shulok. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [dalej](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
