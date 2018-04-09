---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Dodawanie potwierdzenie po stronie klienta podczas usuwania (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W interfejsach, którego dotychczasowy utworzyliśmy użytkownik może przypadkowo usunąć dane, klikając przycisk Usuń, gdy są one przeznaczone do kliknij przycisk Edytuj. W tym t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 72b15d498e45cc519a14ecfe39111b224db88c30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Dodawanie potwierdzenie po stronie klienta podczas usuwania (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) lub [pobierania plików PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> W interfejsach, którego dotychczasowy utworzyliśmy użytkownik może przypadkowo usunąć dane, klikając przycisk Usuń, gdy są one przeznaczone do kliknij przycisk Edytuj. W tym samouczku dodamy okno dialogowe potwierdzenia po stronie klienta, który jest wyświetlany, gdy zostanie kliknięty przycisk Usuń.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku samouczki możemy kolejnych przedstawiono sposób wysyłać Nasza architektura aplikacji, ObjectDataSource i danych formantów sieci Web w porozumieniu z Wstawianie, edytowanie i usuwanie funkcji. Usuwanie interfejsy możemy stawienia zbadać dotychczasowych zostały złożony z usunięciem przycisku, gdy kliknięty, powoduje odświeżenie strony i wywołuje ObjectDataSource s `Delete()` — metoda. `Delete()` Metody następnie wywołuje metodę skonfigurowany z warstwy logiki biznesowej, która propaguje wywołania do warstwy dostępu do danych, wystawiania rzeczywiste `DELETE` instrukcji w bazie danych.

Gdy ten interfejs użytkownika umożliwia odwiedzający do usuwania rekordów za pomocą kontrolki GridView, widoku DetailsView lub FormView, brakuje dowolny rodzaj potwierdzenie gdy użytkownik kliknie przycisk Usuń. Jeśli użytkownik przypadkowo kliknie przycisk Usuń, gdy są przeznaczone do kliknij przycisk Edytuj, zamiast tego zostanie usunięty rekord, które są przeznaczone do zaktualizowania. Aby temu zapobiec, w tym samouczku dodamy okno dialogowe potwierdzenia po stronie klienta, który jest wyświetlany, gdy zostanie kliknięty przycisk Usuń.

Kod JavaScript `confirm(string)` funkcja wyświetla jej parametr wejściowy ciąg tekst wewnątrz modalne okno dialogowe jest dostarczany z dwóch przycisków - OK i Anuluj (zobacz rysunek 1). `confirm(string)` Funkcja zwraca wartość logiczną, w zależności od tego, jakie przycisku (`true`, gdy użytkownik kliknie przycisk OK, a `false` po kliknięciu przycisku Anuluj).


![Confirm(string) JavaScript metoda Wyświetla modalne, Messagebox po stronie klienta](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Rysunek 1**: JavaScript `confirm(string)` metoda Wyświetla element Messagebox modalne, po stronie klienta


Podczas przesyłania formularza, jeśli wartość `false` jest zwracana z obsługi zdarzeń po stronie klienta, a następnie przesłanie formularza zostało anulowane. Za pomocą tej funkcji, będziemy mieć Usuń przycisk s po stronie klienta `onclick` obsługi zdarzeń zwrócić wartość wywołania `confirm("Are you sure you want to delete this product?")`. Jeśli użytkownik kliknie przycisk Anuluj, `confirm(string)` zwróci wartość false, co powoduje przesyłanie formularza, aby anulować. Z nie ogłaszania zwrotnego można usunąć produkt został kliknięty przycisk Usuń, którego won t. Jeśli jednak użytkownik kliknie przycisk OK w oknie dialogowym potwierdzenia, ogłaszania zwrotnego będą nadal nieobniżone i produktu zostaną usunięte. Zapoznaj się [s przy użyciu języka JavaScript `confirm()` metody przesyłania formularza kontroli](http://www.webreference.com/programming/javascript/confirm/) Aby uzyskać więcej informacji na temat tej techniki.

Dodawanie niezbędne skryptu po stronie klienta różni się nieco Jeśli za pomocą szablonów niż przy użyciu CommandField. W związku z tym w tym samouczku przedstawiono przykład zarówno FormView i widoku GridView.

> [!NOTE]
> Przy użyciu technik potwierdzenie po stronie klienta, takich jak te omówione w tym samouczku założono odwiedzają użytkownicy przeglądarek, które obsługuje języka JavaScript i mają włączoną obsługę języka JavaScript. Jeśli jeden z tych założeń nie są spełnione dla danego użytkownika, klikając przycisk Usuń natychmiast spowoduje odświeżenie strony (nie są wyświetlane messagebox Potwierdź).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Krok 1: Tworzenie FormView obsługującego usunięcia

Rozpocznij od dodania FormView do `ConfirmationOnDelete.aspx` w obszarze `EditInsertDelete` folderu powiązania jej z nowego elementu ObjectDataSource wstecz ściągający informacji o produkcie za pośrednictwem `ProductsBLL` klasy s `GetProducts()` metody. Również skonfigurować ObjectDataSource, aby `ProductsBLL` klasy s `DeleteProduct(productID)` metody jest mapowany na ObjectDataSource s `Delete()` metody; upewnij się, że karty INSERT i UPDATE list rozwijanych są ustawione na (Brak). Na koniec pole wyboru Włącz stronicowanie w widoku FormView tag inteligentny s.

Po wykonaniu tych kroków nowego znacznika deklaratywne s ObjectDataSource będzie wyglądać następująco:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Jak w naszych przykładach ostatnich, które nie użyto optymistycznej współbieżności, Poświęć chwilę, aby umożliwić wyczyszczenie ObjectDataSource s `OldValuesParameterFormatString` właściwości.

Ponieważ powiązano kontrolki ObjectDataSource, który obsługuje tylko usunięcie FormView s `ItemTemplate` oferuje tylko przycisk Usuń, nie zawiera przyciski nowy i aktualizacji. Jednak znaczników deklaratywne s FormView obejmuje zbędny `EditItemTemplate` i `InsertItemTemplate`, które mogą zostać usunięte. Poświęć chwilę, aby dostosować `ItemTemplate` dlatego oznacza to pokazuje tylko podzbiór produktu pola danych. I Zapisz skonfigurowane analizę, aby wyświetlić nazwę produktu s w `<h3>` nagłówek powyżej nazwy kategorii i dostawcy (wraz z przycisku Usuń).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Wprowadzone zmiany zostały funkcjonalnej strony sieci web, który umożliwia użytkownikowi przełączać się między jeden produktów w czasie, umożliwia usuwanie produktu, klikając przycisk Usuń. Na rysunku 2 przedstawiono zrzut ekranu: naszych postępu dotychczasowych widzianego za pośrednictwem przeglądarki.


[![FormView przedstawia informacje o jednym produkcie](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Rysunek 2**: FormView zawiera informacje o jeden produkt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Krok 2: Wywoływanie confirm(string) funkcji z onclick usuń przyciski klienta zdarzeń

Z FormView utworzone, ostatnim krokiem jest skonfigurowanie przycisk Usuń takie że w przypadku jego s kliknięty przez obiekt odwiedzający JavaScript `confirm(string)` funkcja jest wywoływana. Dodawanie skryptu po stronie klienta do przycisku, LinkButton lub ImageButton s po stronie klienta `onclick` zdarzeń można osiągnąć za pośrednictwem `OnClientClick property`, który jest nowym składnikiem programu ASP.NET 2.0. Ponieważ chcemy mieć wartość `confirm(string)` zwrócona przez funkcję, wystarczy ustawić tę właściwość na: `return confirm('Are you certain that you want to delete this product?');`

Po tej zmianie składni deklaratywnej s LinkButton usunąć powinien wyglądać jak:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Wszystkie dostępne tego s jest! Rysunek 3 przedstawia zrzut ekranu: potwierdzenie tej akcji. Kliknięcie przycisku Usuń powoduje wyświetlenie okna dialogowego Potwierdź. Jeśli użytkownik kliknie przycisk Anuluj, ogłaszania zwrotnego zostało anulowane i produktu nie zostanie usunięta. Kontynuuje ogłaszania zwrotnego, jeśli jednak użytkownik kliknie przycisk OK, a ObjectDataSource s `Delete()` wywoływana jest metoda, skutkując usunięciem rekordów bazy danych.

> [!NOTE]
> Ciąg przekazany do `confirm(string)` funkcji JavaScript jest oddzielany za pomocą apostrofów (zamiast znaków cudzysłowu). W języku JavaScript można ograniczać ciągów za pomocą obu znaków. Używamy apostrofów tutaj, aby Ogranicznik ciągu przekazany do `confirm(string)` powstanie niejednoznaczności ogranicznikami używany do `OnClientClick` wartości właściwości.


[![Potwierdzenie jest teraz wyświetlany po kliknięciu przycisku Usuń.](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Rysunek 3**: A potwierdzenia jest teraz wyświetlany po kliknięciu przycisku przycisk Usuń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Krok 3: Konfigurowanie właściwość OnClientClick przycisk Usuń w elemencie CommandField

Podczas pracy z przycisku, LinkButton lub ImageButton bezpośrednio w szablonie, okno dialogowe potwierdzenia może być skojarzony z nim po prostu konfigurując jego `OnClientClick` właściwości zwracają wyniki skryptu JavaScript `confirm(string)` funkcji. Jednak nie ma CommandField, - dodająca pola usuń przyciski widoku GridView lub widoku DetailsView - `OnClientClick` właściwości, które można ustawić deklaratywnie. Zamiast tego, firma Microsoft musi programowo odwoływać się w widoku GridView lub widoku DetailsView s odpowiedni przycisk Usuń `DataBound` obsługi zdarzeń, a następnie ustawić jego `OnClientClick` właściwości.

> [!NOTE]
> Podczas ustawiania przycisk Usuń s `OnClientClick` właściwości w odpowiedniej `DataBound` program obsługi zdarzeń, będziemy mieć dostęp do danych został powiązany z bieżącym rekordem. Oznacza to, że firma Microsoft mogą rozszerzać komunikat potwierdzenia do zawierają szczegółowe informacje dotyczące konkretnego rekordu, takie jak "Czy na pewno chcesz usunąć produktu Chai?" Takie dostosowanie jest również w szablonach przy użyciu składni wiązania z danymi.


Ustawienie praktyki `OnClientClick` na stronie właściwości przycisku(-ów) usuwania w elemencie CommandField, pozwalają s Dodaj element GridView. Skonfiguruj tego widoku GridView używać tej samej kontrolki ObjectDataSource, która używa FormView. Także ograniczać s GridView BoundFields obejmujący tylko nazwa s produktu, kategorii i dostawcy. Ponadto pole wyboru Włącz usuwanie z tagów inteligentnych s widoku GridView. Spowoduje to dodanie CommandField s GridView `Columns` kolekcji z jego `ShowDeleteButton` ustawioną właściwość `true`.

Po wprowadzeniu tych zmian, deklaratywne GridView znaczników w s powinna wyglądać następująco:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField zawiera pojedyncze wystąpienie usunąć LinkButton, które można uzyskać programistycznie z widoku GridView s `RowDataBound` program obsługi zdarzeń. Gdy odwołuje się do, firma Microsoft można ustawić jej `OnClientClick` właściwości odpowiednio. Tworzenie procedury obsługi zdarzeń dla `RowDataBound` zdarzeń przy użyciu następującego kodu:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Ten program obsługi zdarzeń współpracuje z wierszy danych, (te, które będą miały przycisk Usuń) i rozpocznie się programowo, odwołując przycisk Usuń. W ogólności należy użyć następującego wzorca:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*Właściwość ButtonType* jest typ używany przez CommandField - przycisku, LinkButton lub ImageButton przycisku. Używa domyślnie LinkButtons CommandField, ale to można dostosować za pomocą CommandField s `ButtonType property`. *CommandFieldIndex* jest indeksem porządkowym CommandField w widoku GridView s `Columns` kolekcji, podczas gdy *controlIndex* jest indeksem przycisk Usuń w elemencie CommandField s `Controls` kolekcji. *ControlIndex* wartość zależy od pozycji przycisku s względem innych przycisków w elemencie CommandField. Na przykład jeśli tylko wyświetlany w elemencie CommandField przycisk przycisk Usuń, użyj indeks 0. Jeśli jednak występują przycisk edytowania poprzedzający przycisk Usuń używać indeks 2. Przyczyna jest używany indeks 2 jest ponieważ dwóch formantów są dodawane przez CommandField przed przycisk Usuń: przycisk Edytuj i LiteralControl tego s służy do dodawania niektórych odstępów między przyciskami edytowanie i usuwanie.

W naszym przykładzie określonego CommandField używa LinkButtons i Trwa pole lewej ma *commandFieldIndex* 0. Ponieważ ma nie innych przycisków, ale w elemencie CommandField przycisk Usuń, używamy *controlIndex* 0.

Po odwołujące się do przycisku Usuń w elemencie CommandField, możemy obok pobrania informacji o produkcie powiązany z bieżącego wiersza w widoku GridView. Na koniec ustawiliśmy na przycisku Usuń s `OnClientClick` właściwości do odpowiednich JavaScript, która zawiera nazwę produktu s. Ponieważ przekazany ciąg JavaScript `confirm(string)` funkcja rozdzielana przy użyciu apostrofów możemy musi wprowadzić żadnych apostrofów, które pojawiają się w nazwie produktu s. W szczególności wszelkie apostrofów w nazwie produktu s będą miały zmienione znaczenie z "`\'`".

Wprowadzone zmiany ukończone klikając przycisk Usuń, w oknie widoku GridView są wyświetlane okno dialogowe potwierdzenia dostosowanego pola (patrz rysunek 4). Jako z messagebox potwierdzenia z FormView, gdy użytkownik kliknie przycisk Anuluj ogłaszania zwrotnego zostało anulowane, zapobiegając w ten sposób usunięcia wystąpienia.

> [!NOTE]
> Ta technika mogą służyć do uzyskania programowego dostępu do przycisku Usuń w elemencie CommandField w widoku DetailsView. Dla widoku DetailsView, jednak d utworzyć programu obsługi zdarzeń dla `DataBound` zdarzenie, ponieważ nie ma widoku DetailsView `RowDataBound` zdarzeń.


[![Kliknięcie przycisku Usuń s GridView Wyświetla okno dialogowe potwierdzenia dostosowane](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Rysunek 4**: kliknięcie s GridView przycisk Usuń Wyświetla okno dialogowe potwierdzenia dostosowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Przy użyciu TemplateFields

Jedną z wad CommandField jest, że jej przyciski muszą być dostępne za pośrednictwem indeksowanie i że wynikowy obiekt musi być rzutowany na typ odpowiedni przycisk (przycisk, LinkButton lub ImageButton). Za pomocą "Magia" i ustalony typy zaprasza problemów, które nie może odnaleźć do środowiska wykonawczego. Na przykład jeśli użytkownik lub innego projektanta dodaje nowe przyciski do CommandField w pewnym momencie w przyszłości (np. przycisk Edytuj) lub zmiany `ButtonType` właściwości istniejącego kodu będzie nadal kompilować bez błędów, ale odwiedzania strony może spowodować wyjątek lub nieoczekiwanego zachowania, w zależności od tego, jak zostało zapisane kodu i jakie zmiany zostały wprowadzone.

Informacje o innym podejściu jest aby przekonwertować GridView i widoku DetailsView s CommandFields TemplateFields. Spowoduje to wygenerowanie TemplateField z `ItemTemplate` mający LinkButton (lub przycisku lub ImageButton) dla każdego przycisku w elemencie CommandField. Tych przycisków `OnClientClick` właściwości można przypisać deklaratywnie, jak możemy był wyświetlany z FormView lub programowo dostępnych w odpowiedniej `DataBound` obsługi zdarzeń przy użyciu następującego wzorca:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Gdzie *controlID* jest wartość przycisku s `ID` właściwości. Podczas tego wzorca nadal wymaga typu ustalony rzutowanie, eliminuje konieczność indeksowania, co pozwala na układu zmienić powodowała błąd w czasie wykonywania.

## <a name="summary"></a>Podsumowanie

Kod JavaScript `confirm(string)` funkcja jest często używane techniki kontroli przepływu pracy przesyłania formularza. Podczas wykonywania funkcji Wyświetla okno dialogowe modalne, po stronie klienta, którego obejmuje dwa przyciski OK i Anuluj. Jeśli użytkownik kliknie przycisk OK, `confirm(string)` funkcja zwraca `true`; kliknięcie przycisku Anuluj zwraca `false`. Ta funkcja jest połączone z zachowanie przeglądarki s, aby anulować przesyłania formularza, jeśli program obsługi zdarzeń podczas procesu przesyłania zwraca `false`, można użyć do wyświetlenia messagebox potwierdzenia podczas usuwania rekordu.

`confirm(string)` Funkcja może być skojarzony z sieci Web przycisk formantu s po stronie klienta `onclick` obsługi zdarzeń za pomocą formantu s `OnClientClick` właściwości. Podczas pracy z przycisk Usuń, w szablonie — albo w jednym z szablonów s FormView lub TemplateField w widoku DetailsView lub widoku GridView — tej właściwości można ustawić deklaratywnie lub programowo, jak widzieliśmy w tym samouczku.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](implementing-optimistic-concurrency-cs.md)
> [dalej](limiting-data-modification-functionality-based-on-the-user-cs.md)
