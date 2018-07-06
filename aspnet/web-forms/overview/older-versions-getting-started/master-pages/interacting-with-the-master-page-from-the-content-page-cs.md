---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: Interakcja ze stroną wzorcową z poziomu strony zawartości (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zbadamy, jak wywoływać metody, ustaw właściwości, itp. strona wzorcowa z kodu na stronie zawartości.
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2aa2ba4443a2bad248b0d2cd2a140dc828fd7747
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842879"
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>Interakcja ze stroną wzorcową z poziomu strony zawartości (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Zbadamy, jak wywoływać metody, ustaw właściwości, itp. strona wzorcowa z kodu na stronie zawartości.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich pięciu samouczków, które mają zobaczyliśmy, jak utworzyć stronę wzorcową Zdefiniuj obszary zawartości powiązać strony ASP.NET na stronie wzorcowej i definiowania zawartości dla strony. Gdy użytkownik zażąda określonej strony zawartości, zawartość i strony wzorcowe markup są połączone funkcjami w czasie wykonywania, co w przypadku renderowania hierarchii kontroli ujednoliconego. W związku z tym, już widzieliśmy taki sposób, który strony wzorcowej i jeden z jego strony z zawartością mogą wchodzić w interakcje: strony zawartości opisujemy znaczników transfuse do kontrolek ContentPlaceHolder strony wzorcowej.

Co mamy jeszcze do sprawdzenia jest jak strony wzorcowej oraz strony zawartości można wchodzić w interakcje programowo. Oprócz definiowania znaczników dla kontrolek ContentPlaceHolder strony wzorcowej, strony zawartości można przypisać wartości do właściwości publiczne jej stronę wzorcową i wywoływania jego metody publiczne. Podobnie strony wzorcowej mogą wchodzić w interakcje z jego strony z zawartością. W trakcie rzadziej niż interakcji między ich deklaratywne znaczników interakcji programistycznych między strony wzorcowej i zawartości istnieje wiele scenariuszy, w których wymagane jest takie interakcji programistycznych.

W tym samouczku sprawdzamy, jak strony zawartości mogą programowo współdziałać z jego strony głównej; w następnym samouczku przyjrzymy się sposobu strony wzorcowej podobnie interakcji z jego strony z zawartością.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Przykłady programowe interakcji między strony zawartości i jej strona wzorcowa

Gdy w danym regionie strony musi być skonfigurowane dla strony Strona, używamy kontrolki ContentPlaceHolder. Ale co sytuacji, w których większość stron muszą emitować niektórych danych wyjściowych, ale niewielką liczbę stron trzeba dostosować go do wyświetlenia coś innego? Przykładem takiego, który zbadaliśmy w [ *wiele kontrolek ContentPlaceHolder i zawartość domyślna* ](multiple-contentplaceholders-and-default-content-cs.md) samouczku obejmuje wyświetlanie interfejs logowania na każdej stronie. Chociaż większość strony powinna zawierać interfejs logowania, powinien zostać pominięty na kilka stron, takich jak: na stronie głównej logowania (`Login.aspx`); na stronie tworzenia konta; oraz innych stron, które są dostępne do uwierzytelnionych użytkowników jedynie w. [ *Wiele kontrolek ContentPlaceHolder i zawartość domyślna* ](multiple-contentplaceholders-and-default-content-cs.md) samouczku przedstawiono sposób definiowania domyślnej zawartości dla ContentPlaceHolder na stronie głównej, a następnie jak je zastąpić w tych stron where nie Chcieliśmy domyślnej zawartości.

Innym rozwiązaniem jest tworzenie publiczna właściwość lub metoda w obrębie strony wzorcowej, która wskazuje, czy pokazać lub ukryć interfejs logowania. Na przykład strony wzorcowej może obejmować o nazwie właściwości publicznej `ShowLoginUI` użyto których wartość można ustawić `Visible` właściwości kontrolki logowania na stronie głównej. Tych stron zawartości, gdy interfejs użytkownika logowania ma być pomijana. Następnie można programowo ustawić `ShowLoginUI` właściwość `false`.

Może być najbardziej typowym przykładem zawartości i interakcji z strony wzorcowej występuje, gdy dane wyświetlane na potrzeby strony wzorcowej zostanie odświeżona po niektóre akcje, upłynął czas na stronie zawartości. Należy wziąć pod uwagę, że strony wzorcowej, która obejmuje GridView wyświetlający pięć ostatnio dodane rekordy z tabeli określonej bazy danych, i że jedna z jego strony z zawartością zawiera interfejs służący do dodawania nowych rekordów do tej samej tabeli.

Gdy użytkownik odwiedzi stronę, aby dodać nowy rekord, stwierdza, że pięć ostatnio dodany rekordów wyświetlanych na stronie głównej. Po wypełnieniu pól wartości w kolumnach nowy rekord, użytkownik wyśle formularz. Przy założeniu, że ma GridView na stronie głównej jego `EnableViewState` właściwość jest ustawiona na wartość true (ustawienie domyślne), jego zawartość zostanie ponownie załadowana z widoku stanu i, w związku z tym, pięć rekordów tej samej są wyświetlane, nawet jeśli nowszy rekord został dodany do bazy danych. Może to być niezrozumiałe użytkownika.

> [!NOTE]
> Nawet jeśli wyłączysz stan widoku GridView tak, aby go rebinds do jego źródle danych, na każdym zwrotu, nadal nie będzie widoczna właśnie dodany rekord ponieważ danych jest powiązany z GridView we wcześniejszej części cyklu życia strony niż po dodaniu nowego rekordu do datab środowisko ASE.


Aby rozwiązać ten problem, tak aby po prostu dodać rekord jest wyświetlany na stronie głównej firmy GridView na odświeżenie strony, należy wydać polecenie GridView, aby ponownie powiązać ze swoim źródłem danych *po* nowy rekord został dodany do bazy danych. Wymaga interakcji między zawartością, a strony wzorcowe, ponieważ interfejs do dodawania nowego rekordu (i jego programy obsługi zdarzeń) znajdują się w zawartości strony, ale GridView, który musi zostać odświeżona strony wzorcowej.

Odświeżanie strony wzorcowej widoku z programu obsługi zdarzeń w zawartości strony jest jednym z najbardziej typowych potrzeb dla zawartości i interakcji z strony wzorcowej, Przyjrzyjmy się w tym temacie bardziej szczegółowo. Pobierania w tym samouczku obejmuje bazę danych Microsoft SQL Server 2005 Express Edition, o nazwie `NORTHWIND.MDF` w witrynie internetowej `App_Data` folderu. Bazy danych Northwind przechowuje produktu, pracowników i informacji o sprzedaży dla fikcyjnej firmy Northwind Traders.

Krok 1 przeszukiwania poprzez wyświetlanie pięć ostatnio dodane produktów w GridView na stronie głównej. Krok 2 tworzy stronę zawartości do dodawania nowych produktów. Krok 3 pokazano, jak utworzyć publiczny właściwości i metody w stronę wzorcową i kroku 4 pokazano, jak programowo współpracować z tych właściwości i metod ze strony zawartość.

> [!NOTE]
> W tym samouczku nie delve w szczegółowe informacje na temat pracy z danymi w programie ASP.NET. Kroki konfigurowania strony wzorcowej do wyświetlania danych i strony zawartości do wstawiania danych są pełne, jeszcze breezy. Aby uzyskać więcej informacji na temat przyjrzeć wyświetlania i wstawianie danych przy użyciu kontrolki SqlDataSource i GridView zapoznaj się zasoby przedstawione w sekcji dalsze odczyty na końcu tego samouczka.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Krok 1: Wyświetlanie pięć ostatnio dodane produktów na stronie wzorcowej

Otwórz `Site.master` strony wzorcowej, a następnie dodaj etykietę i kontrolki widoku siatki do `leftContent` `<div>`. Czyści etykiety `Text` właściwość, ustaw jego `EnableViewState` właściwości na wartość false i jego `ID` właściwości `GridMessage`; Ustaw GridView `ID` właściwość `RecentProducts`. Następnie przy użyciu projektanta, rozwiń GridView tagu inteligentnego i wybierz opcję powiązać go z nowego źródła danych. Spowoduje to uruchomienie Kreatora konfiguracji źródła danych. Ponieważ bazy danych Northwind `App_Data` folderu jest bazą danych programu Microsoft SQL Server Utwórz SqlDataSource, wybierając (patrz rysunek 1); nazwa SqlDataSource `RecentProductsDataSource`.


[![Powiąż widoku GridView z kontrolką SqlDataSource o nazwie RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Rysunek 01**: powiązywanie kontrolki SqlDataSource o nazwie GridView `RecentProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


Następnym krokiem pyta, czy NAS, aby określić, co bazy danych, aby nawiązać połączenie. Wybierz `NORTHWIND.MDF` pliku z listy rozwijanej bazy danych, a następnie kliknij przycisk Dalej. Ponieważ ta baza danych była używana po raz pierwszy, Kreator będzie oferować przechowywać parametry połączenia w `Web.config`. Jest przechowywanie parametrów połączenia przy użyciu nazwy `NorthwindConnectionString`.


[![Połączenia z bazą danych Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Rysunek 02**: łączenie z bazą danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


W Kreatorze konfigurowania źródła danych zawiera dwa oznacza, że za pomocą których można określić zapytania służące do pobierania danych:

- Określając niestandardową instrukcję SQL lub procedury składowanej, lub
- Wybieranie tabeli lub widoku, a następnie określając kolumny do zwrócenia

Ponieważ chcemy zwracać tylko pięć ostatnio dodany produktów, należy określić niestandardową instrukcję SQL. Użyj następującego zapytania wybierz:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5` — Słowo kluczowe zwraca pierwsze pięć rekordów z zapytania. `Products` Klucza podstawowego tabeli, `ProductID`, jest `IDENTITY` kolumny, która zapewnia nam, że każdy nowy produkt dodawane do tabeli wartości większej niż poprzedniej pozycji. W związku z tym, sortowanie wyników według `ProductID` zwraca produkty, począwszy od ostatniego utworzonymi w w kolejności malejącej.


[![Zwróć pięciu produktach ostatnio dodane](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Rysunek 03**: zwraca pięć najbardziej niedawno dodano produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


Po ukończeniu kreatora, program Visual Studio generuje dwa BoundFields widoku GridView wyświetlić `ProductName` i `UnitPrice` pola zwrócony z bazy danych. W tym momencie oznaczeniu deklaracyjnym strony wzorcowej powinien zawierać kod znaczników podobny do następującego:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Jak widać, zawiera znaczniki: formant etykiety w sieci Web (`GridMessage`); widoku GridView `RecentProducts`z dwóch BoundFields; i kontrolki SqlDataSource, które zwraca 5 ostatnio dodany produktów.

Dzięki temu GridView utworzone i skonfigurowane, jego użyciu kontrolki SqlDataSource, odwiedź witrynę sieci Web za pośrednictwem przeglądarki. Jak pokazano na rysunku 4, zobaczysz, że siatki w lewym dolnym rogu, który zawiera pięć ostatnio dodany produktów.


[![Kontrolki GridView Wyświetla pięć ostatnio dodane produktów](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Rysunek 04**: widoku GridView przedstawia pięć najbardziej niedawno dodano produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> Możesz wyczyścić wyglądu kontrolki GridView. Kilka sugestii obejmują formatowanie wyświetlana `UnitPrice` wartość jako walutę i przy użyciu czcionki i kolory tła, aby poprawić wygląd siatki.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Krok 2: Tworzenie strony zawartości, aby dodać nowe produkty

Naszym kolejnym krokiem jest utworzenie strony zawartości, w którym użytkownik może dodać nowy produkt do `Products` tabeli. Dodaj nową stronę zawartości do `Admin` folder o nazwie `AddProduct.aspx`i powiązać `Site.master` strony wzorcowej. Rysunek 5. Pokazuje Eksplorator rozwiązań po tej strony została dodana do witryny sieci Web.


[![Dodawanie nowej strony programu ASP.NET do folderu administratora](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Rysunek 05**: Dodawanie nowej strony programu ASP.NET do `Admin` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


Pamiętaj, że w [ *Określanie tytułu, tagów Meta i innych nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) samouczek tworzenia niestandardowej strony podstawowej klasy o nazwie `BasePage` , generowane tytuł strony, jeśli było nie jest jawnie ustawione. Przejdź do `AddProduct.aspx` kodem strony klasy i jest pochodną `BasePage` (zamiast z `System.Web.UI.Page`).

Na koniec zaktualizuj `Web.sitemap` plik, aby dołączyć wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` dla lekcji problemów nazewnictwa identyfikator formantu:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Jak pokazano na rysunku 6, to dodanie `<siteMapNode>` element jest odzwierciedlana na liście lekcje.

Wróć do `AddProduct.aspx`. W formancie zawartości dla `MainContent` ContentPlaceHolder, Dodaj kontrolce DetailsView i nadaj mu nazwę `NewProduct`. Powiąż DetailsView z kontrolką SqlDataSource o nazwie `NewProductDataSource`. Np. z kontrolką SqlDataSource w kroku 1, skonfiguruj kreatora, tak aby używał bazy danych Northwind i określić niestandardową instrukcję SQL. Ponieważ DetailsView będzie służyć do dodawania elementów do bazy danych, należy podać obydwie wartości `SELECT` instrukcji i `INSERT` instrukcji. Należy użyć następującego `SELECT` kwerendy:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Z karty Wstawianie, dodaj następującą `INSERT` instrukcji:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Po zakończeniu pracy kreatora przejdź do DetailsView tagu inteligentnego, a następnie zaznacz pole wyboru "Włącz wstawianie". Spowoduje to dodanie CommandField DetailsView z jego `ShowInsertButton` właściwość ma wartość true. Ponieważ ta DetailsView będzie służyć wyłącznie do wstawiania danych, ustaw DetailsView `DefaultMode` właściwość `Insert`.

To wszystko jest do niego! Umożliwia testowanie tej strony. Odwiedź stronę `AddProduct.aspx` za pośrednictwem przeglądarki, wprowadź nazwę i cena (patrz rysunek 6).


[![Dodaj nowy produkt do bazy danych](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Rysunek 06**: Dodaj nowy produkt do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Po wpisaniu nazwy i ceny na nowy produkt, kliknij przycisk Wstaw. To powoduje, że formularz odświeżania. Na odświeżenie strony, użyciu kontrolki SqlDataSource firmy `INSERT` zostaje wykonana instrukcja; jego dwa parametry są wypełniane przy użyciu wartości wprowadzonych przez użytkownika w DetailsView dwóch pól tekstowych. Niestety nie ma żadnych wizualną opinię, która jest przeprowadzana w instrukcji insert. Byłoby miły komunikat zawierający, potwierdzenie, że dodano nowy rekord. Można pozostawić to w charakterze ćwiczenia dla czytnika. Ponadto po dodaniu nowego rekordu z DetailsView GridView na stronie głównej nadal pokazuje tych samych pięciu rekordy jako przed; nie ma rekordu po prostu dodanego przez producenta. Zajmiemy się jak rozwiązać ten problem w kolejnych krokach.

> [!NOTE]
> Oprócz dodawania pewnego rodzaju wizualną opinię, która Wstaw zakończyła się pomyślnie, czy zachęcam Cię do również zaktualizować DetailsView Wstawianie interfejsu do uwzględnienia sprawdzania poprawności. Obecnie nie ma możliwości weryfikacji. Jeśli użytkownik wprowadzi nieprawidłową wartością dla `UnitPrice` pola, takie jak "jest zbyt kosztowne," wyjątek zostanie zgłoszony na odświeżenie strony, gdy system próbuje przekonwertować ciąg na ułamek dziesiętny. Aby uzyskać więcej informacji na temat dostosowywania, wstawianie interfejsu, zapoznaj się [ *Dostosowywanie interfejsu modyfikacji danych* samouczek](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) z moich [Praca z serii samouczków danych](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Krok 3: Tworzenie właściwości publiczne i metod na stronie wzorcowej

W kroku 1 dodaliśmy formant etykiety w sieci Web o nazwie `GridMessage` powyżej GridView na stronie głównej. Ta etykieta jest przeznaczona do opcjonalnie wyświetlić wiadomość. Na przykład, po dodaniu nowego rekordu do `Products` tabeli może być chcemy wyświetlić wiadomość, która odczytuje: "*ProductName* został dodany do bazy danych." Zamiast trwale kodować tekstu dla tej etykiety na stronie głównej firma Microsoft może być komunikat, aby mieć możliwość dostosowania przez strony zawartości.

Ponieważ formant etykiety jest implementowany jako zmienna chroniony element członkowski w obrębie strony wzorcowej nie są dostępne bezpośrednio z poziomu strony zawartości. Aby móc pracować z etykietą w obrębie strony wzorcowej, z poziomu strony zawartości (lub służącego dowolnej kontrolki sieci Web, na stronie głównej) należy utworzyć właściwość publiczną na stronie głównej, który udostępnia kontrolki sieci Web lub służy jako serwer proxy, za pomocą którego można jednej z jego właściwości  dostępne. Dodaj następującą składnię do strony wzorcowej osobna klasa kodu do udostępnienia etykiety `Text` właściwości:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Po dodaniu nowego rekordu do `Products` tabelę z poziomu strony zawartości `RecentProducts` GridView na stronie głównej wymaga ponownie powiązać do źródła danych. Aby ponownie powiązać wywołanie GridView jego `DataBind` metody. Ponieważ GridView na stronie głównej nie jest dostępne do stron zawartości, firma Microsoft, należy utworzyć publiczną metodę na stronie głównej, gdy zostanie wywołana, rebinds dane do widoku GridView. Dodaj następującą metodę do klasy CodeBehind strony wzorcowej:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

Za pomocą `GridMessageText` właściwości i `RefreshRecentProductsGrid` metody w miejscu, dowolnej strony zawartości można programowo ustawić lub odczytać wartość `GridMessage` etykiety `Text` właściwości lub ponownie powiązać dane `RecentProducts` GridView. Krok 4 sprawdza uzyskiwania dostępu do strony wzorcowej publicznej właściwości i metod ze strony zawartość.

> [!NOTE]
> Nie należy zapominać oznaczyć właściwości i metod jako stronę wzorcową `public`. Jeśli użytkownik jawnie określa tych właściwości i metod jako `public`, nie będzie dostępny z poziomu strony zawartości.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Krok 4: Wywoływania strony wzorcowej publiczne elementy członkowskie z poziomu strony zawartości

Teraz, że strona główna zawiera niezbędne publicznej właściwości i metod, jesteśmy gotowi do wywołania tych właściwości i metod z `AddProduct.aspx` strony zawartość. W szczególności należy ustawić strony wzorcowej `GridMessageText` właściwości i wywołania jego `RefreshRecentProductsGrid` metoda po dodaniu nowego produktu w bazie danych. Wszystkie platformy ASP.NET kontrolkach internetowych danych wyzwalać zdarzeń bezpośrednio przed i po wykonaniu różnych zadań, które ułatwiają programistom stron wykonania określonego działania programistyczne przed lub po zadania. Na przykład, gdy użytkownik końcowy kliknie przycisk Wstaw DetailsView, na ogłaszanie zwrotne zgłasza DetailsView jego `ItemInserting` zdarzeń przed rozpoczęciem Wstawianie przepływu pracy. Następnie wstawia rekord do bazy danych. Poniżej DetailsView wywołuje jego `ItemInserted` zdarzeń. W związku z tym, aby można było pracować z strony wzorcowej, po dodaniu nowego produktu, utworzyć program obsługi zdarzeń DetailsView `ItemInserted` zdarzeń.

Istnieją dwa sposoby, które strony zawartości programowo może współpracować z jego strony głównej:

- Za pomocą `Page.Master` właściwość, która zwraca odwołanie typowaniem luźnym strony wzorcowej, lub
- Wpisz ścieżkę strony strona wzorcowa typu lub pliku za pośrednictwem `@MasterType` dyrektywę; to automatycznie dodaje właściwość silnie typizowane do strony o nazwie `Master`.

Przeanalizujmy oba podejścia.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Korzystanie z typowaniem luźnym`Page.Master`właściwości

Wszystkie strony sieci web platformy ASP.NET muszą pochodzić od `Page` klasy, która znajduje się w `System.Web.UI` przestrzeni nazw. `Page` Klasa zawiera [ `Master` właściwość](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) , zwraca odwołanie do strony głównej. Jeśli strona nie jest stroną wzorcową `Master` zwraca `null`.

`Master` Właściwość zwraca obiekt typu [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (również znajduje się w `System.Web.UI` przestrzeni nazw) czyli typ podstawowy, z którego dziedziczyć wszystkie strony wzorcowej. W związku z tym, do użycia właściwości publiczne lub metody zdefiniowane w naszej witryny sieci Web stronę wzorcową, firma Microsoft rzutować `MasterPage` obiekt zwracany z `Master` właściwości odpowiedniego typu. Ponieważ firma Microsoft o nazwie nasz plik strony głównej `Site.master`, nosiła nazwę klasy CodeBehind `Site`. W związku z tym, poniższy kod rzutowania `Page.Master` właściwości wystąpienia klasy lokacji.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Skoro mamy już rzutować typowaniem luźnym `Page.Master` właściwość `Site` typu Firma Microsoft może odwoływać się do właściwości i metody specyficzne dla lokacji. Jak pokazano na rysunku 7, właściwość publiczna `GridMessageText` pojawia się na liście rozwijanej funkcji IntelliSense.


[![Funkcja IntelliSense wyświetla właściwości publiczne i metod nasze strony wzorcowej](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Rysunek 07**: funkcja IntelliSense wyświetla właściwości publiczne i metod nasze strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Jeśli nazwany plik strony głównej `MasterPage.master` , a następnie nazwa klasy CodeBehind strony wzorcowej jest `MasterPage`. Może to prowadzić do kodu niejednoznaczne, gdy rzutowania z typu `System.Web.UI.MasterPage` do Twojej `MasterPage` klasy. Krótko mówiąc należy do pełnej kwalifikacji typu, które są rzutowania, który może być trudna przy użyciu modelu projektu witryny sieci Web. Moja sugestia jest albo upewnij się, że po utworzeniu stronę wzorcową przypadku nadania jej nazwy innej niż `MasterPage.master` lub jeszcze lepiej utworzyć silnie typizowane odwołania do strony wzorcowej.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Tworzenie odwołanie silnie Typizowane`@MasterType`— dyrektywa

Można spojrzeć ściśle możesz zobaczyć, że na stronie ASP.NET osobna klasa kodu jest klasę częściową (Uwaga `partial` — słowo kluczowe w definicji klasy). Klasy częściowe zostały wprowadzone w języku C# i Visual Basic with.NET Framework 2.0, a mówiąc, umożliwiające składowych należy zdefiniować na wiele plików. Plik klasy CodeBehind - `AddProduct.aspx.cs`, na przykład — zawiera kod, który, deweloper strony utworzymy. Oprócz naszego kodu aparatu ASP.NET automatycznie tworzy plik osobnej klasy z właściwości i procedury obsługi zdarzeń w tym tłumaczenie oznaczeniu deklaracyjnym strony hierarchii klas.

Generowanie kodu automatyczne, który występuje zawsze, gdy odwiedzenia strony ASP.NET drogę do wprowadzenia dla niektórych możliwości dość interesujące i przydatne. W przypadku stron wzorcowych, jeśli o aparatu ASP.NET, w jaki strony wzorcowej jest on używany przez naszą stronę zawartości generuje silnie typizowanego `Master` właściwości dla nas.

Użyj [ `@MasterType` dyrektywy](https://msdn.microsoft.com/library/ms228274.aspx) poinformować aparatu ASP.NET typu strony wzorcowej strony zawartość. `@MasterType` Dyrektywy może zaakceptować, wpisz nazwę strony wzorcowej lub ścieżki pliku. Aby określić, że `AddProduct.aspx` stronie używa `Site.master` jako jego stronę wzorcową, dodaj następującą dyrektywę na początku `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Ta dyrektywa powoduje, że aparat platformy ASP.NET można dodać odwołania do strony wzorcowej za pośrednictwem właściwości o nazwie silnie typizowane `Master`. Za pomocą `@MasterType` dyrektywy w miejscu, możemy wywołać `Site.master` głównym właściwości publiczne i metod za pomocą strony `Master` właściwość bez żadnych rzutowania.

> [!NOTE]
> Jeżeli pominięto `@MasterType` dyrektywy, składnia `Page.Master` i `Master` zwracać ten sam efekt: obiekt typowaniem luźnym stronę wzorcową. Jeśli dołączysz `@MasterType` następnie dyrektywy `Master` zwraca silnie typizowane odwołanie do określonej strony wzorcowej. `Page.Master`, jednak nadal zwraca odwołanie z typowaniem luźnym. Dla bardziej szczegółowego przyjrzeć się Dlaczego tak jest oraz sposób, w jaki `Master` właściwość jest tworzony podczas `@MasterType` dyrektywa jest dołączony, zobacz [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)firmy wpis w blogu [ `@MasterType` w programie ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualizowanie strony wzorcowej po dodaniu nowego produktu

Teraz, gdy wiemy, jak wywołać publicznej właściwości i metod ze strony zawartości strony wzorcowej, możemy przystąpić do aktualizacji `AddProduct.aspx` strony, czemu wzorca strona zostanie odświeżona po dodaniu nowego produktu. Na początku kroku 4, utworzyliśmy program obsługi zdarzeń dla formantu DetailsView `ItemInserting` zdarzenie, które wykonuje natychmiast, po został dodany nowy produkt do bazy danych. Dodaj następujący kod do tego programu obsługi zdarzeń:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Powyższy kod korzysta z obu typowaniem luźnym `Page.Master` właściwość i że wpisane pogrubieniem `Master` właściwości. Należy pamiętać, że `GridMessageText` właściwość jest ustawiona na "*ProductName* dodane do siatki..." Wartości produktu po prostu dodać są dostępne za pośrednictwem `e.Values` kolekcji; jak widać, po prostu dodane `ProductName` wartość są dostępne za pośrednictwem `e.Values["ProductName"]`.

Rysunek 8 przedstawia `AddProduct.aspx` strony natychmiast po nowych produktów — Scotta Soda — został dodany do bazy danych. Należy pamiętać, że nazwa produktu po prostu dodać została przedstawiona w Etykieta strony wzorcowej i odświeżeniu widoku GridView obejmujący produktu, a jego ceną.


[![Etykieta i Pokaż GridView produktu po prostu dodać strony wzorcowej](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Rysunek 08**: Etykieta strony wzorcowej i GridView Pokaż produktu Just-Added ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Podsumowanie

W idealnym przypadku strony wzorcowej i na stronach zawartości są całkowicie niezależne od siebie nawzajem i wymagają żaden poziom interakcji. Gdy stron wzorcowych i stronach zawartości należy tak zaprojektować ten cel, pamiętając, istnieje szereg typowych scenariuszy, w których strony zawartości muszą współpracować z jego strony wzorcowej. Jedną z najbardziej typowych przyczyn koncentruje się wokół aktualizowania konkretnego fragmentu wyświetlaną stronę wzorcową, oparte na pewne akcje, które odwzorowanie na stronie zawartości.

Dobra wiadomość jest stosunkowo prosta programowo współdziałać z jego strony wzorcowej strony zawartości. Rozpocznij od utworzenia na stronie głównej, która hermetyzują funkcji, który ma być wywoływany przez strony zawartości publicznej właściwości lub metody. Następnie na stronie zawartości dostęp strony wzorcowej właściwości i metod za pośrednictwem typowaniem luźnym `Page.Master` właściwości lub użyj `@MasterType` dyrektywy, aby utworzyć silnie typizowane odwołania do strony wzorcowej.

W następnym samouczku omówiony jak programowo współdziałać z jednej z jego strony z zawartością strony wzorcowej.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu do i aktualizowanie danych w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Stron wzorcowych platformy ASP.NET: Porady, wskazówki i pułapek](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` w programie ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Przekazywanie informacji między zawartości i stronami wzorcowymi](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Praca z danymi w samouczki platformy ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Zack Jones. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](control-id-naming-in-content-pages-cs.md)
> [dalej](interacting-with-the-content-page-from-the-master-page-cs.md)
