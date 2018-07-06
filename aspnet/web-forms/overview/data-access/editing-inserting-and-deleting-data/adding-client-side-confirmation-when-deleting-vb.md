---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Dodawanie potwierdzenia po stronie klienta podczas usuwania (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W interfejsach, którą utworzyliśmy, do tej pory użytkownik może przypadkowo usunąć dane, klikając przycisk Usuń, gdy są one przeznaczone do kliknij przycisk Edytuj. W tym t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 2318018b20841038ebb7a6e4900cf9849057d941
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802500"
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>Dodawanie potwierdzenia po stronie klienta podczas usuwania (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) lub [Pobierz plik PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> W interfejsach, którą utworzyliśmy, do tej pory użytkownik może przypadkowo usunąć dane, klikając przycisk Usuń, gdy są one przeznaczone do kliknij przycisk Edytuj. W tym samouczku dodamy okno dialogowe potwierdzenia po stronie klienta, który pojawia się po kliknięciu przycisku usuwania.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku samouczków firma ve przedstawiono sposób korzystanie z naszej architektury aplikacji, danych kontrolki sieci Web i kontrolki ObjectDataSource z optymalizacją umożliwia wstawianie, edytowanie i usuwanie możliwości. Usuwanie interfejsy możemy ve zbadanie tej pory były składa się z usunięciem przycisku, który kliknięto, powoduje odświeżenie strony i wywołuje s kontrolki ObjectDataSource `Delete()` metody. `Delete()` Metoda następnie wywołuje metodę skonfigurowany z warstwy logiki biznesowej, która propaguje wywołania do warstwy dostępu do danych wystawianie rzeczywiste `DELETE` instrukcji w bazie danych.

Podczas tego interfejsu użytkownika umożliwia gości do usuwania rekordów za pomocą kontrolki GridView, DetailsView lub FormView, mu dowolny rodzaj potwierdzenie, gdy użytkownik kliknie przycisk Usuń. Jeśli użytkownik przypadkowo kliknie przycisk usuwania, gdy są przeznaczone do kliknij przycisk Edytuj, zamiast tego zostanie usunięty rekord, który one przeznaczone do zaktualizowania. Aby temu zapobiec, w tym samouczku dodamy okno dialogowe potwierdzenia po stronie klienta, który pojawia się po kliknięciu przycisku usuwania.

Kod JavaScript `confirm(string)` funkcja wyświetla własny parametr wejściowy ciągu jako tekst wewnątrz pola modalne okno dialogowe, który jest dostarczany za pomocą dwóch przycisków — OK i anulowania (patrz rysunek 1). `confirm(string)` Funkcja zwraca wartość logiczną, w zależności od tego, jakie przycisku (`true`, jeśli użytkownik kliknie przycisk OK, a `false` po kliknięciu przycisku Anuluj).


![Confirm(string) JavaScript, metoda Wyświetla modalny Messagebox po stronie klienta](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Rysunek 1**: kod JavaScript `confirm(string)` metoda wyświetla komunikat Messagebox modalne, po stronie klienta


Podczas przesyłania formularza, jeśli wartość `false` jest zwracana z obsługi zdarzenia po stronie klienta, a następnie przesyłania formularza zostanie anulowana. Dzięki tej funkcji można przygotować Usuń przycisk s po stronie klienta `onclick` programu obsługi zdarzeń, zwróć wartość wywołania `confirm("Are you sure you want to delete this product?")`. Jeśli użytkownik kliknie przycisk Anuluj, `confirm(string)` zwróci wartość false, co powoduje przesłanie formularza anulować. Za pomocą nie zwrotu można usunąć produkt został kliknięty przycisk Usuń, którego wygrał t. Jeśli jednak użytkownik kliknie przycisk OK w oknie dialogowym potwierdzenia, ogłaszania zwrotnego będzie nadal nieobniżone i produktu zostaną usunięte. Zapoznaj się z [s przy użyciu języka JavaScript `confirm()` metodę przesyłania formularza kontrolki](http://www.webreference.com/programming/javascript/confirm/) Aby uzyskać więcej informacji na temat tej techniki.

Dodanie niezbędnych skryptu po stronie klienta różni się nieco Jeśli za pomocą szablonów niż przy użyciu CommandField. W związku z tym w tym samouczku przyjrzymy się przykład FormView i GridView.

> [!NOTE]
> Przy użyciu technik potwierdzenia po stronie klienta, jak w powyższym omówionych w tym samouczku przyjęto założenie, odwiedzają użytkownicy z przeglądarki, które obsługuje języka JavaScript i mają włączoną obsługę języka JavaScript. Jeśli nie są jedną z tych założeń są spełnione dla danego użytkownika, klikając przycisk Usuń natychmiast spowoduje odświeżenie strony (nie są wyświetlane messagebox Potwierdź).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Krok 1: Tworzenie kontrolce FormView, który obsługuje usuwania

Rozpocznij od dodania FormView do `ConfirmationOnDelete.aspx` strony w `EditInsertDelete` folderu, wiążące go do nowego elementu ObjectDataSource, ponownie pobierający informacje o produkcie za pośrednictwem `ProductsBLL` klasy s `GetProducts()` metody. Również skonfigurować kontrolki ObjectDataSource tak, aby `ProductsBLL` klasy s `DeleteProduct(productID)` metody jest mapowany na ObjectDataSource s `Delete()` metody; upewnij się, że karty INSERT i UPDATE, listy rozwijane są ustawione na (Brak). Koniec sprawdź, czy pole wyboru Włącz stronicowania w tagu inteligentnego s FormView.

Po wykonaniu tych kroków nowe oznaczeniu deklaracyjnym s ObjectDataSource będzie wyglądać następująco:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Jak w naszych przykładach poprzednich, które nie używały optymistycznej współbieżności, Poświęć chwilę na wyczyszczenie ObjectDataSource s `OldValuesParameterFormatString` właściwości.

Ponieważ powiązano kontrolka ObjectDataSource, który obsługuje tylko usunięcie FormView s `ItemTemplate` oferuje tylko przycisk Usuń, zawiera przyciski New i Update. Jednak FormView s oznaczeniu deklaracyjnym, obejmuje zbędny `EditItemTemplate` i `InsertItemTemplate`, który może zostać usunięty. Poświęć chwilę, aby dostosować `ItemTemplate` tak oznacza to pokazuje tylko podzbiór produktu pola danych. Czy mogę ve skonfigurowane, min, aby wyświetlić nazwę produktu s w `<h3>` nagłówka powyżej jego nazwy dostawcy i kategorii (wraz z przycisku Usuń).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Za pomocą tych zmian zostały w pełni funkcjonalne strony sieci web, który umożliwia użytkownikowi przełączać się między produktami, co w czasie, z możliwością Usuń produkt, wystarczy kliknąć przycisk Usuń. Na rysunku 2 przedstawiono zrzut ekranu: nasz postęp tej pory, podczas wyświetlania za pośrednictwem przeglądarki.


[![FormView informacjami na temat jednego produktu](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Rysunek 2**: FormView zawiera informacje dotyczące jednego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Krok 2: Wywoływanie confirm(string) funkcji z onclick usuń przyciski klienta zdarzeń

Za pomocą FormView utworzone, ostatnim krokiem jest skonfigurować przycisk Usuń takie że w przypadku jego s kliknięty przez obiekt odwiedzający JavaScript `confirm(string)` wywołaniu funkcji. Dodawanie skryptu po stronie klienta do przycisku, element LinkButton lub ImageButton s klienta `onclick` zdarzenia może się odbywać za pośrednictwem `OnClientClick property`, który jest nowym składnikiem programu ASP.NET 2.0. Ponieważ chcemy wartość `confirm(string)` zwrócona przez funkcję, wystarczy ustawić dla tej właściwości: `return confirm('Are you certain that you want to delete this product?');`

Po tej zmianie składni deklaratywnej s usunąć element LinkButton powinien wyglądać następująco:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Wszystkie dostępne tego s jest! Rysunek 3 przedstawia zrzut ekranu to potwierdzenie w działaniu. Kliknięcie przycisku Usuń otwiera okno dialogowe Potwierdź. Jeśli użytkownik kliknie przycisk Anuluj, zwrot zostanie anulowane i produktu nie zostanie usunięta. Jeśli jednak użytkownik kliknie przycisk OK, nadal zwrotu i ObjectDataSource s `Delete()` metoda jest wywoływana, skutkując usunięciem rekordu bazy danych.

> [!NOTE]
> Ciąg przekazany do `confirm(string)` rozdzielana funkcji języka JavaScript przy użyciu apostrofy (zamiast znaki cudzysłowu). W języku JavaScript można ograniczać ciągi przy użyciu dowolnego znaku. Możemy użyć apostrofów tutaj, aby ograniczników ciągu przekazany do `confirm(string)` powstanie niejednoznaczności z ogranicznikami umożliwiający `OnClientClick` wartości właściwości.


[![Potwierdzenie jest teraz wyświetlany po kliknięciu przycisku przycisk Usuń](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Rysunek 3**: potwierdzenia jest teraz wyświetlany po kliknięciu przycisku przycisk Usuń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Krok 3: Konfigurowanie właściwość OnClientClick przycisk Usuń w CommandField

Podczas pracy z przycisku, element LinkButton lub ImageButton bezpośrednio w szablonie, okno dialogowe potwierdzenia można skojarzyć z nim, po prostu konfigurując jego `OnClientClick` właściwości zwracają kod JavaScript `confirm(string)` funkcji. Jednak CommandField, -, który dodaje pole Usuń przycisków do kontrolki GridView lub DetailsView — nie ma `OnClientClick` właściwość, która może być ustawiona w sposób deklaratywny. Zamiast tego należy programowo odwołujemy się przycisk Usuń w odpowiednich s GridView lub DetailsView `DataBound` programu obsługi zdarzeń, a następnie ustaw jego `OnClientClick` istnieje właściwość.

> [!NOTE]
> Podczas ustawiania przycisk Usuń s `OnClientClick` właściwości w odpowiedniej `DataBound` procedura obsługi zdarzeń, mamy dostęp do danych została powiązana z bieżącym rekordem. Oznacza to, że teraz możemy rozszerzać komunikat potwierdzający obejmujący szczegółowe informacje dotyczące konkretnego rekordu, takie jak "Czy na pewno chcesz usunąć produkt Chai?" Takie dostosowanie może się również w szablonach przy użyciu składni wiązania danych.


Rozwiązaniem jest ustawienie `OnClientClick` właściwość przycisku(-ów) Delete w CommandField s pozwalają na stronie Dodaj GridView. Skonfiguruj tego GridView, aby użyć tego samego kontrolka ObjectDataSource, który używa FormView. Także ograniczać to s GridView BoundFields obejmujący tylko nazwa s produktu, kategorii i dostawcy. Na koniec zaznacz pole wyboru Włącz usuwanie z tagu inteligentnego s GridView. Spowoduje to dodanie CommandField s GridView `Columns` kolekcji z jego `ShowDeleteButton` właściwością `true`.

Po wprowadzeniu tych zmian, znaczników deklaratywne s GridView powinien wyglądać następująco:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

CommandField zawiera pojedyncze wystąpienie LinkButton usuwania, które są dostępne programowo z GridView s `RowDataBound` programu obsługi zdarzeń. Gdy przywoływane, możemy ustawić jej `OnClientClick` właściwość odpowiednio. Utwórz procedurę obsługi zdarzeń dla `RowDataBound` zdarzeń, używając następującego kodu:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Ta procedura obsługi zdarzeń współpracuje z wierszy danych, (te, które będą miały przycisk Usuń) i rozpoczyna się od programowe odwoływanie się do przycisk Usuń. Ogólnie rzecz biorąc należy użyć następującego wzorca:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*Właściwości ButtonType* jest typ używany przez CommandField — przycisk, element LinkButton lub ImageButton przycisku. Domyślnie CommandField używa LinkButtons, ale to może być dostosowane za pośrednictwem CommandField s `ButtonType property`. *CommandFieldIndex* jest indeksem porządkowym CommandField w ramach GridView s `Columns` kolekcji, natomiast *controlIndex* jest indeksem przycisk Usuń w ramach CommandField s `Controls` kolekcji. *ControlIndex* wartość zależy od pozycji przycisku s względem innych przycisków w CommandField. Na przykład jeśli przycisk tylko wyświetlane w CommandField znajduje się przycisk Usuń, należy użyć indeks 0. Jeśli jednak s tam przycisk edycji, który poprzedza przycisk Usuń użyć indeksu 2. Przyczyną indeks 2 jest używany jest CommandField przed przycisk Usuń dodaje dwie kontrolki: przycisk Edytuj i LiteralControl tego s używane do dodawania odstępem pomiędzy przyciski edytowania i usuwania.

W naszym przykładzie określonego CommandField używa LinkButtons i zawiera on pole skrajnie po lewej *commandFieldIndex* 0. Ponieważ istnieją inne przyciski, ale przycisk Usuń w CommandField, używamy *controlIndex* 0.

Po odwołujące się do przycisk Usuń w CommandField, firma Microsoft następnie Pobierz informacje o produkcie powiązane z bieżącego wiersza GridView. Na koniec możemy ustawić przycisk Usuń s `OnClientClick` właściwość do odpowiedniego kodu JavaScript, który zawiera nazwę produktu s. Ponieważ ciąg JavaScript przekazany do `confirm(string)` funkcji jest rozdzielany, przy użyciu apostrofy możemy przed nimi znak ucieczki wszelkie apostrofy, które są wyświetlane w nazwie produktu s. W szczególności wszelkich apostrofy w nazwie produktu s będą miały zmienione znaczenie za pomocą "`\'`".

Za pomocą tych zmian ukończyć, klikając przycisk Usuń w Wyświetla GridView, okno dialogowe potwierdzenia niestandardowe pole (zobacz rysunek 4). Jako z messagebox potwierdzenia z FormView, jeśli użytkownik kliknie przycisk Anuluj zwrotu zostało anulowane, zapobiegając w ten sposób usuwania występowaniu.

> [!NOTE]
> Tej techniki można również uzyskać programowy dostęp do przycisk Usuń w CommandField w DetailsView. Dla DetailsView, jednak d utworzyć program obsługi zdarzeń dla `DataBound` zdarzenie, ponieważ nie ma DetailsView `RowDataBound` zdarzeń.


[![Kliknięcie przycisku Usuń s GridView wyświetlane jest okno dialogowe potwierdzenia niestandardowe](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Rysunek 4**: klikając s GridView przycisk Usuń przedstawia dostosowane okno dialogowe potwierdzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>Używanie kontrolek TemplateField

Jedną z wad CommandField jest, że jej przyciski muszą być dostępne za pośrednictwem indeksowania i że wynikowy obiekt musi być rzutowany na typ odpowiedni przycisk (przycisk, element LinkButton lub ImageButton). Za pomocą "Magia" i ustaloną typy zaprasza problemy, które nie można odnaleźć aż do czasu. Na przykład jeśli użytkownik lub innemu deweloperowi, dodaje nowe przyciski do CommandField w pewnym momencie w przyszłości (np. przycisk Edytuj) lub zmiany `ButtonType` właściwości istniejącego kodu nadal zostanie skompilowana bez błędów, ale odwiedzając stronę może spowodować wyjątek lub nieoczekiwanego zachowania, w zależności od tego, jak został napisany kod, i jakie zmiany zostały wprowadzone.

Alternatywnym podejściem jest aby przekonwertować s kontrolkami GridView i DetailsView CommandFields kontrolek TemplateField. Spowoduje to wygenerowanie TemplateField z `ItemTemplate` zawierający element LinkButton (lub przycisku lub ImageButton) dla każdego przycisku w CommandField. Przyciski te `OnClientClick` właściwości można przypisać w sposób deklaratywny, jak firma dostrzegła przy użyciu widoku FormView lub programowo jest możliwy w odpowiedniej `DataBound` obsługi zdarzeń przy użyciu następującego wzorca:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Gdzie *controlID* jest wartością przycisk s `ID` właściwości. Podczas tego wzorca nadal wymaga typu ustaloną rzutowania, jej eliminuje potrzebę indeksowania, umożliwiając układu można zmienić bez spowodowało błąd w czasie wykonywania.

## <a name="summary"></a>Podsumowanie

Kod JavaScript `confirm(string)` funkcji to technika często używane do kontrolowania przepływu pracy przesyłania formularza. Po wykonaniu funkcji Wyświetla okno dialogowe modalne, po stronie klienta, która zawiera dwa przyciski o OK i Anuluj. Jeśli użytkownik kliknie przycisk OK, `confirm(string)` funkcja zwraca `true`; kliknięcie przycisku Anuluj zwraca `false`. Ta funkcja jest sprzężona z zachowanie przeglądarki s, aby anulować przesłanie formularza, jeśli zwraca program obsługi zdarzeń w procesie przesyłania `false`, może służyć do wyświetlania messagebox potwierdzenie podczas usuwania rekordu.

`confirm(string)` Funkcja może być skojarzony z sieci Web przycisku sterowania s po stronie klienta `onclick` programu obsługi zdarzeń za pomocą formantu s `OnClientClick` właściwości. Podczas pracy z przycisk usuwania szablonu — w jednym z szablonów s FormView lub w TemplateField w DetailsView lub GridView - tę właściwość można ustawić deklaratywne lub programowo, zgodnie z widzieliśmy w ramach tego samouczka.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](implementing-optimistic-concurrency-vb.md)
> [dalej](limiting-data-modification-functionality-based-on-the-user-vb.md)
