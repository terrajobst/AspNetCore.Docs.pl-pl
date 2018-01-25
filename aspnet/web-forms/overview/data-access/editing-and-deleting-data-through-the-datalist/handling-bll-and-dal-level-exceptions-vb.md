---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: "Obsługa wyjątków na poziomie logiki warstwy Biznesowej i warstwy DAL (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku przedstawiono będzie sposób obsługi przeczucie wyjątki zgłoszone podczas aktualizowania przepływu pracy można edytować DataList."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bd2ccb13c44d104e8945840705a21738d8abd5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Obsługa wyjątków na poziomie logiki warstwy Biznesowej i warstwy DAL (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) lub [pobierania plików PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> W tym samouczku przedstawiono będzie sposób obsługi przeczucie wyjątki zgłoszone podczas aktualizowania przepływu pracy można edytować DataList.


## <a name="introduction"></a>Wprowadzenie

W [omówienie edytowanie i usuwanie danych elementu DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) samouczek, utworzyliśmy DataList, oferowane proste edytowanie i usuwanie funkcji. Podczas pełnej funkcjonalności było bardzo trudno najłatwiejsze w użyciu, jakiegokolwiek błędu, który wystąpił podczas edycji lub usuwania procesu spowodowała nieobsługiwany wyjątek. Na przykład, pomijając nazwę produktu s lub podczas edycji produktu, wprowadzenie wartości cen bardzo przystępnej!, zgłasza wyjątek. Ponieważ ten wyjątek nie zostanie przechwycony w kodzie, propaguje go do środowiska uruchomieniowego ASP.NET, którego następnie wyświetla szczegóły wyjątku s na stronie sieci web.

Jak widzieliśmy w [obsługi logiki warstwy Biznesowej i wyjątków DAL na poziomie strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczek, jeśli wyjątek zgłoszony z głębokości logiki biznesowej i warstwy dostępu do danych, szczegóły wyjątku są zwracane do elementu ObjectDataSource i następnie do widoku GridView. Widzieliśmy jak obsługiwać te wyjątki, tworząc `Updated` lub `RowUpdated` programy obsługi zdarzeń dla elementu ObjectDataSource lub widoku GridView sprawdzanie wyjątku i wskazującą, czy wyjątek został obsłużony.

Nasze DataList samouczki, jednak nie są t przy użyciu elementu ObjectDataSource aktualizowania i usuwania danych. Pracujemy bezpośrednio przed logiki warstwy Biznesowej. W celu wykrycia wyjątki pochodzące z logiki warstwy Biznesowej lub DAL, należy wdrożyć kod w kodem naszą stronę ASP.NET do obsługi wyjątków. W tym samouczku przedstawiono będzie sposób obsługi bardziej przeczucie wyjątki zgłoszone podczas edycji s DataList aktualizowanie przepływu pracy.

> [!NOTE]
> W *omówienie edytowanie i usuwanie danych elementu DataList* samouczku omówiono różne techniki edytowanie i usuwanie danych z elementu DataList niektóre techniki wykonywane przy użyciu elementu ObjectDataSource aktualizacji i usuwanie. Jeśli te techniki zostanie zastosowana, może obsługiwać wyjątki od logiki warstwy Biznesowej lub DAL przez element ObjectDataSource s `Updated` lub `Deleted` procedury obsługi zdarzeń.


## <a name="step-1-creating-an-editable-datalist"></a>Krok 1: Tworzenie można edytować DataList.

Zanim firma Microsoft martwić Obsługa wyjątków występujących podczas aktualizowania przepływu pracy, chętnie s najpierw utworzyć można edytować elementu DataList. Otwórz `ErrorHandling.aspx` strony `EditDeleteDataList` folderu, Dodaj DataList do projektanta, ustaw jej `ID` właściwości `Products`, i Dodaj nowy element ObjectDataSource o nazwie `ProductsDataSource`. Element ObjectDataSource umożliwia konfigurowanie `ProductsBLL` klasy s `GetProducts()` rejestruje metodę wyboru; Ustaw list rozwijanych w INSERT, UPDATE i usuwanie kart na (Brak).


[![Zwraca informacje produktu przy użyciu metody GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Rysunek 1**: zwraca informacje o produkt za pomocą `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Po zakończeniu pracy Kreatora ObjectDataSource Visual Studio automatycznie utworzy `ItemTemplate` dla elementu DataList. Zastąp to z `ItemTemplate` która wyświetla każdy element name produktu s i ceny i zawiera przycisk Edytuj. Następnie należy utworzyć `EditItemTemplate` za pomocą formantu TextBox w sieci Web dla nazwy i ceny i przyciski aktualizacji i Anuluj. Wreszcie, ustaw DataList s `RepeatColumns` właściwości do 2.

Po wprowadzeniu tych zmian znaczniki deklaratywne s strony powinien wyglądać podobnie do poniższej. Dokładnie, czy upewnij się, że Edycja, Anuluj, i mieć aktualizacji przycisków ich `CommandName` do edytowania, Anuluj i zaktualizować odpowiednio ustawić właściwości.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> W tym samouczku elementu DataList musi być włączony stan widoku s.


Poświęć chwilę, aby wyświetlić postęp naszych za pośrednictwem przeglądarki (patrz rysunek 2).


[![Każdy produkt zawiera przycisk Edytuj](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Rysunek 2**: każdego produktu zawiera przycisk Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


Obecnie przycisk Edytuj tylko powoduje odświeżenie strony go t jeszcze sprawiają, że produkt edytowalnych. Aby umożliwić edytowanie, należy utworzyć procedury obsługi zdarzeń dla elementu DataList s `EditCommand`, `CancelCommand`, i `UpdateCommand` zdarzenia. `EditCommand` i `CancelCommand` zdarzenia wystarczy zaktualizować DataList s `EditItemIndex` właściwości i ponownie Utwórz wiązanie danych do elementu DataList:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

`UpdateCommand` Procedura obsługi zdarzeń jest nieco bardziej skomplikowane. Należy go odczytać w produkcie edytowanych s `ProductID` z `DataKeys` kolekcji wraz z nazwą produktu s i cen z pól tekstowych w `EditItemTemplate`, a następnie wywołać `ProductsBLL` klasy s `UpdateProduct` metoda przed zwróceniem elementu DataList Stan wstępnie edycji.

Na razie let s wystarczy użyć dokładnie tego samego kodu z `UpdateCommand` obsługi zdarzeń w *omówienie edytowanie i usuwanie danych elementu DataList* samouczka. Dodamy kod, aby bezpiecznie obsługi wyjątków w kroku 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

Nieprawidłowe dane wejściowe w wypadku której może być w formie cenie jednostkowej niewłaściwie sformatowany, cenę niedozwolony jednostki, takich jak-$5.00 lub pominięcie nazwy s produktu, który zostanie wygenerowany wyjątek. Ponieważ `UpdateCommand` program obsługi zdarzeń nie zawiera żadnych wyjątków kod obsługi w tym momencie, wyjątek zostanie bąbelkowy do środowiska uruchomieniowego ASP.NET, gdzie będzie wyświetlana dla użytkownika końcowego (patrz rysunek 3).


![Gdy wystąpi nieobsługiwany wyjątek, użytkownik końcowy widzi stronę błędu](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Rysunek 3**: gdy wystąpi nieobsługiwany wyjątek, użytkownik końcowy widzi stronę błędu


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Krok 2: Bezpiecznie Obsługa wyjątków w obsłudze zdarzeń UpdateCommand

Podczas aktualizacji przepływu pracy, wyjątki mogą występować w `UpdateCommand` obsługi zdarzeń logiki warstwy Biznesowej i warstwy DAL. Na przykład, jeśli użytkownik wprowadzi cena za kosztowne, `Decimal.Parse` instrukcji w `UpdateCommand` zgłosi obsługi zdarzeń `FormatException` wyjątku. Jeśli użytkownik pomija nazwy s produktu lub jeśli cena ma wartość ujemną, warstwy DAL zgłosi wyjątek.

Po wystąpieniu wyjątku, chcemy wyświetlić komunikat informacyjny w samej strony. Dodawanie sieci Web etykiety kontroli do strony, którego `ID` ma ustawioną wartość `ExceptionDetails`. Skonfiguruj tekst etykiety s do wyświetlenia na czerwono, bardzo duży pogrubionego oraz pochylonego czcionki, przypisując jego `CssClass` właściwości `Warning` klasy CSS, która jest zdefiniowana w `Styles.css` pliku.

Gdy wystąpi błąd, chcemy tylko etykiety, które mają być wyświetlane raz. Oznacza to na kolejnych ogłaszania zwrotnego, komunikat ostrzegawczy etykiety s powinien zniknąć. Można to zrobić przez albo wyczyszczenie limit etykiety s `Text` właściwości lub ustawienia jej `Visible` właściwości `False` w `Page_Load` obsługi zdarzeń (jak robiliśmy w [obsługi logiki warstwy Biznesowej i warstwy DAL poziomie wyjątków w ASP Strona .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) samouczek) lub przez wyłączenie obsługi stanu widoku etykiety s. Poinformuj tę druga opcję s.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Zgłoszony wyjątek, firma Microsoft będzie przypisać szczegóły wyjątku do `ExceptionDetails` etykiety formantu s `Text` właściwości. Ponieważ swój stan widoku jest wyłączone, kolejne ogłaszania zwrotnego `Text` właściwości s programowe zmiany zostaną utracone, przywrócenie domyślny tekst (ciąg pusty), a tym samym ukrywanie komunikat ostrzegawczy.

Aby określić, kiedy został zgłoszony błąd w celu wyświetlania przydatne wiadomości na stronie, musimy dodać `Try ... Catch` za pomocą bloku `UpdateCommand` obsługi zdarzeń. `Try` Części zawiera kod, który może prowadzić do wyjątku podczas `Catch` blok zawiera kod, który jest wykonywany w wypadku wyjątków. Zapoznaj się z [podstawowe informacje dotyczące obsługi wyjątków](https://msdn.microsoft.com/library/2w8f0bss.aspx) sekcji w dokumentacji programu .NET Framework, aby uzyskać więcej informacji na `Try ... Catch` bloku.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Gdy dowolnego typu, jest zwracany wyjątek przez kod w `Try` bloku, `Catch` kod bloku s rozpocznie się wykonywanie. Typ wyjątku, który jest zgłaszany `DbException`, `NoNullAllowedException`, `ArgumentException`i tak dalej zależy, jakie dokładnie, w pierwszej kolejności wytrącane błędu. Jeśli istnieje s problem na poziomie bazy danych, `DbException` zostanie zgłoszony. Jeśli wprowadzono jest niedozwolona wartość `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, lub `ReorderLevel` pól, `ArgumentException` będą zgłaszane, możemy dodać kod do sprawdzania poprawności te wartości pól w `ProductsDataTable` klasy (zobacz [ Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md) samouczka).

Firma Microsoft może zapewnić bardziej użyteczne informacje użytkownikowi końcowemu tworzony tekst komunikatu na typ Przechwycono wyjątek. Następujący kod, który został użyty w postaci niemal identyczny jak w [obsługi logiki warstwy Biznesowej i wyjątków DAL na poziomie strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) samouczek zawiera ten poziom szczegółowości:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Do ukończenia tego samouczka, po prostu Wywołaj `DisplayExceptionDetails` metody z `Catch` bloku, przekazując zgłoszony `Exception` wystąpienia (`ex`).

Z `Try ... Catch` bloków w miejscu, użytkownicy wyświetlana jako rysunki 4 i 5 Pokaż bardziej szczegółowy komunikat o błędzie. Należy pamiętać, że w wypadku wyjątek elementu DataList pozostaje w trybie edycji. Wynika to z faktu w momencie wyjątek przepływu sterowania natychmiast jest przekierowywany do `Catch` bloku, pomijając kod, który zwraca stan wstępnie edycji elementu DataList.


[![Jeśli użytkownik pomija pola wymagane jest wyświetlany komunikat o błędzie](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Rysunek 4**: komunikat o błędzie jest wyświetlany, jeśli użytkownik pomija wymagane pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Komunikat o błędzie jest wyświetlany podczas wprowadzania ceny](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Rysunek 5**: komunikat o błędzie jest wyświetlany podczas wprowadzania ceny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Podsumowanie

Element GridView i ObjectDataSource zapewniają obsługi zdarzeń po poziomu, które zawierają informacje o żadnych wyjątków, które zostały zgłoszone podczas aktualizowania i usuwania przepływu pracy, a także właściwości, które można ustawić, aby wskazać, czy wyjątek został obsługiwane. Te funkcje, jednak są niedostępne podczas pracy z elementu DataList i bezpośrednio przy użyciu logiki warstwy Biznesowej. Zamiast tego firma Microsoft są zobowiązani do implementowania obsługi wyjątków.

W tym samouczku widzieliśmy sposób dodawania obsługi wyjątków do edycji s DataList aktualizowanie przepływu pracy, dodając `Try ... Catch` za pomocą bloku `UpdateCommand` obsługi zdarzeń. Jeśli wystąpił wyjątek podczas aktualizowania przepływu pracy, `Catch` wykonuje kod w bloku s, wyświetlanie informacje pomocne w `ExceptionDetails` etykiety.

W tym momencie elementu DataList sprawia, że nie starań, aby zapobiec wyjątki od sytuacji, w pierwszej kolejności. Mimo że wiemy, że cen ujemnej spowoduje wyjątek, możemy informacjami o klientach t dodać jeszcze żadnych funkcji do aktywnego uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowe dane wejściowe. W naszym samouczku dalej zajmiemy się tym, jak ograniczyć wyjątki spowodowane przez nieprawidłowy użytkownikiem, dodając formanty walidacji w `EditItemTemplate`.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Wyjątki — zalecenia dotyczące projektowania](https://msdn.microsoft.com/library/ms298399.aspx)
- [Moduły rejestrowania błędów i obsługi (ELMAH)](http://workspaces.gotdotnet.com/elmah) (Biblioteka open source rejestrowania błędów)
- [Biblioteka przedsiębiorstwa dla programu .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (w tym bloku aplikacji zarządzania wyjątków)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Ken Pespisa. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](performing-batch-updates-vb.md)
[dalej](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
