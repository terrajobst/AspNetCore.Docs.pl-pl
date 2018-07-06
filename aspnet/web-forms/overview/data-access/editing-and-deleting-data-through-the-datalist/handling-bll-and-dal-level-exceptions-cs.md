---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Obsługa wyjątków LOGIKI i poziom warstwy DAL (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku opisano jak przeczucie obsługiwać wyjątki zgłoszone podczas aktualizowania przepływu pracy można edytować DataList.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: f83293f7b8482c2a24c3d825e271582edaadec59
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812083"
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>Obsługa wyjątków LOGIKI i poziom warstwy DAL (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) lub [Pobierz plik PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> W tym samouczku opisano jak przeczucie obsługiwać wyjątki zgłoszone podczas aktualizowania przepływu pracy można edytować DataList.


## <a name="introduction"></a>Wprowadzenie

W [omówienie edytowania i usuwania danych w kontrolce DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) samouczku utworzyliśmy DataList, oferowane proste edytowanie i usuwanie możliwości. Podczas, gdy jest to w pełni funkcjonalny, było bardzo trudno czytelne jak jakikolwiek błąd, który wystąpił podczas edycji lub usuwania procesu spowodowała nieobsługiwany wyjątek. Na przykład, pomijając nazwę produktu s lub edytując produktu, wprowadzenie wartości cen bardzo przystępne cenowo!, zgłasza wyjątek. Ponieważ ten wyjątek nie zostanie przechwycony w kodzie, rozpropagowany środowiska uruchomieniowego programu ASP.NET, który następnie wyświetla szczegóły wyjątku s na stronie sieci web.

Jak widzieliśmy w [obsługi LOGIKI i wyjątki DAL na poziomie strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczek, jeśli wyjątek jest zgłaszany z głębokości logiki biznesowej i warstwy dostępu do danych, szczegóły wyjątku są zwracane do kontrolki ObjectDataSource i następnie do widoku GridView. Firma Microsoft pokazaliśmy, jak bezpiecznie obsługiwać te wyjątki, tworząc `Updated` lub `RowUpdated` programy obsługi zdarzeń dla elementu ObjectDataSource lub GridView, sprawdzanie, dla wyjątku i wskazującą, czy wyjątek został obsłużony.

DataList samouczków, jednak t konto za pomocą kontrolki ObjectDataSource, aktualizowania i usuwania danych. Zamiast tego pracujemy bezpośrednio w odniesieniu do LOGIKI. W celu wykrycia wyjątki pochodzące z LOGIKI lub warstwy DAL, musimy zaimplementować obsługę kodu w ramach kodem naszą stronę ASP.NET wyjątków. W tym samouczku opisano jak bardziej przeczucie obsługiwać wyjątki zgłoszone podczas edycji s DataList aktualizowanie przepływu pracy.

> [!NOTE]
> W *omówienie edytowania i usuwania danych w kontrolce DataList* samouczku Omówiliśmy różne techniki edytowania i usuwania danych w kontrolce DataList kilka technik bierze udziału za pomocą kontrolki ObjectDataSource aktualizacji i usuwanie. Jeśli te techniki zostanie zastosowana, może obsługiwać wyjątki od LOGIKI lub warstwy DAL za pośrednictwem ObjectDataSource s `Updated` lub `Deleted` procedury obsługi zdarzeń.


## <a name="step-1-creating-an-editable-datalist"></a>Krok 1: Tworzenie można edytować DataList

Zanim będziemy zajmować obsługi wyjątków, które występują podczas aktualizowania przepływu pracy, chętnie s, należy najpierw utworzyć DataList można edytować. Otwórz `ErrorHandling.aspx` strony w `EditDeleteDataList` folderu, Dodaj kontrolką DataList do projektanta, ustaw jego `ID` właściwości `Products`, i dodać nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLL` klasy s `GetProducts()` Metoda służąca do wybierania rekordów; Ustawianie list rozwijanych w INSERT, UPDATE i usuwanie kart (Brak).


[![Zwraca informacje o produkcie przy użyciu metody GetProducts()](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Rysunek 1**: zwraca informacje o produkt za pomocą `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


Po zakończeniu pracy Kreatora ObjectDataSource programu Visual Studio będzie automatycznie tworzył `ItemTemplate` dla kontrolki DataList. Zastąp tę wartość ciągiem `ItemTemplate` , wyświetla każdy produkt s nazwy i ceny i zawiera przycisk Edytuj. Następnie należy utworzyć `EditItemTemplate` za pomocą kontrolki TextBox w sieci Web dla nazwy i ceny i przyciski aktualizacji i Anuluj. Wreszcie, ustaw DataList s `RepeatColumns` właściwość 2.

Po wprowadzeniu tych zmian znaczniki deklaratywne s strony powinien wyglądać podobnie do poniższej. Dokładnie, czy upewnij się, że edytowanie, anulowanie, i aktualizacji przyciski powinny mieć ich `CommandName` właściwości ustawione do edytowania, Anuluj i zaktualizuj odpowiednio.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> W tym samouczku kontrolki DataList stan widoku s musi być włączona.


Poświęć chwilę, aby wyświetlić postępach za pośrednictwem przeglądarki (patrz rysunek 2).


[![Każdy produkt zawiera przycisk Edytuj](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Rysunek 2**: każdy produkt zawiera przycisk Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


Obecnie przycisk Edytuj tylko powoduje odświeżenie strony jej t jeszcze sprawiają, że można edytować. Aby umożliwić edytowanie, musimy utworzyć procedury obsługi zdarzeń dla kontrolek DataList s `EditCommand`, `CancelCommand`, i `UpdateCommand` zdarzenia. `EditCommand` i `CancelCommand` zdarzenia po prostu zaktualizuj DataList s `EditItemIndex` właściwość i ponowne wiązanie danych do kontrolki DataList:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

`UpdateCommand` Procedura obsługi zdarzeń jest nieco bardziej skomplikowane. Musi odczytać edytowanego produktu s `ProductID` z `DataKeys` kolekcji wraz z nazwą produktu s i ceny na podstawie pola tekstowe w `EditItemTemplate`, a następnie wywołać `ProductsBLL` klasy s `UpdateProduct` metoda przed zwróceniem kontrolki DataList Stan wstępnie edycji.

Na razie umożliwiają s wystarczy użyć dokładnie tego samego kodu z `UpdateCommand` programu obsługi zdarzeń w *omówienie edytowania i usuwania danych w kontrolce DataList* samouczka. Dodamy kod, aby bezpiecznie obsługiwać wyjątki w kroku 2.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

Nieprawidłowe dane wejściowe w przypadku której może być w formie cena jednostkowa sformatowany, wartością cena jednostki niedozwolony następująco — $5.00 lub pominięcie Nazwa s produktu, który będzie zgłaszany wyjątek. Ponieważ `UpdateCommand` program obsługi zdarzeń nie zawiera każdy wyjątek, kod obsługi w tym momencie, wyjątek zostanie będą się pojawiać do środowiska uruchomieniowego programu ASP.NET, której będzie on wyświetlana użytkownikowi końcowemu (zobacz rysunek 3).


![Po wystąpieniu nieobsługiwanego wyjątku, użytkownik końcowy widzi stronę błędu](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Rysunek 3**: po wystąpieniu nieobsługiwanego wyjątku, użytkownik końcowy widzi stronę błędu


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Krok 2: Bez problemu zmieniała obsługi wyjątków w obsłudze zdarzeń elementu UpdateCommand

Podczas aktualizowania przepływu pracy, mogą wystąpić wyjątki w `UpdateCommand` programu obsługi zdarzeń, LOGIKI lub warstwy DAL. Na przykład, jeśli użytkownik wprowadzi cenie zbyt kosztowne, `Decimal.Parse` instrukcji w `UpdateCommand` zgłosi programu obsługi zdarzeń `FormatException` wyjątku. Jeśli użytkownik pomija Nazwa s produktu lub cena ma wartość ujemną, warstwy DAL zgłosi wyjątek.

Gdy wystąpi wyjątek, chcemy wyświetlić komunikat informacyjny w samej strony. Dodaj etykietę w sieci Web formant do strony, którego `ID` ustawiono `ExceptionDetails`. Skonfiguruj tekst etykiety s do wyświetlenia w kolorze czerwonym, bardzo duży, pogrubionego oraz pochylonego czcionki, przypisując jej `CssClass` właściwości `Warning` klasy CSS, która jest zdefiniowana w `Styles.css` pliku.

Gdy wystąpi błąd, ma być uruchamiany tylko etykiety, które mają być wyświetlane na raz. Oznacza to, że na kolejne ogłaszania zwrotnego, komunikat ostrzegawczy etykiety s powinien zniknąć. Można to osiągnąć przez albo wyczyszczenie się etykiety s `Text` właściwości lub ustawienia jej `Visible` właściwości `False` w `Page_Load` programu obsługi zdarzeń (jak robiliśmy ponownie [obsługi LOGIKI i poziom warstwy DAL wyjątków w ASP Strony .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczka) lub przez wyłączenie obsługi stanu widoku etykiety s. Pozwól s tę druga opcję.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Gdy wyjątek jest zgłaszany, przypiszemy szczegółów wyjątku do `ExceptionDetails` etykiety formantu s `Text` właściwości. Ponieważ swój stan widoku jest wyłączona, kolejne ogłaszania zwrotnego `Text` właściwości s programowe zmiany zostaną utracone, przywrócenie domyślny tekst (ciągiem pustym), a tym samym ukrywanie komunikat ostrzegawczy.

Aby określić, gdy zgłoszono błąd w celu wyświetlania komunikatu przydatne na stronie, musimy dodać `Try ... Catch` za pomocą bloku `UpdateCommand` programu obsługi zdarzeń. `Try` Część zawiera kod, który może prowadzić do wyjątku, gdy `Catch` blok zawiera kod, który jest wykonywany w przypadku wyjątku. Zapoznaj się z [podstawowe informacje dotyczące obsługi wyjątków](https://msdn.microsoft.com/library/2w8f0bss.aspx) sekcji w dokumentacji programu .NET Framework, aby uzyskać więcej informacji na `Try ... Catch` bloku.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Kiedy jest zgłaszany wyjątek dowolnego typu przez kod w ramach `Try` bloku, `Catch` kodu w bloku s rozpocznie się wykonywanie. Typ wyjątku, który jest generowany `DbException`, `NoNullAllowedException`, `ArgumentException`i tak dalej, zależy, co dokładnie wytrącane błędu w pierwszej kolejności. Jeśli tam s problemem na poziomie bazy danych, `DbException` zostanie zgłoszony. Jeśli niedozwoloną wartość jest wprowadzana do `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, lub `ReorderLevel` pól `ArgumentException` zostanie zgłoszony, ponieważ dodaliśmy kod do sprawdzania poprawności tych wartości pól w `ProductsDataTable` klasy (zobacz [ Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md) samouczka).

Firma Microsoft zapewnia bardziej użyteczne wyjaśnienie użytkownikowi końcowemu użycie tekst komunikatu w typie Wystąpił wyjątek. Poniższy kod, który został użyty w postaci niemal identyczne w [obsługi LOGIKI i wyjątki DAL na poziomie strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) samouczek zawiera taki poziom szczegółowości:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Do ukończenia tego samouczka, wystarczy wywołać `DisplayExceptionDetails` metody z `Catch` bloku, przekazując przechwycony `Exception` wystąpienia (`ex`).

Za pomocą `Try ... Catch` blokowanie w miejscu, użytkownicy są przedstawione przy użyciu bardziej szczegółowy komunikat o błędzie jako rysunki 4 i 5 show. Należy zauważyć, że w przypadku wyjątku kontrolki DataList pozostaje w trybie edycji. To dlatego, gdy wystąpi wyjątek, przepływ sterowania od razu zostanie przekierowana do `Catch` bloku, pomijając kod, który zwraca kontrolki DataList stan wstępnie edycji.


[![Komunikat o błędzie jest wyświetlany, jeśli użytkownik pomija pole wymagane](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Rysunek 4**: komunikat o błędzie jest wyświetlany, jeśli użytkownik pomija wymagane pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![Komunikat o błędzie jest wyświetlany podczas wprowadzania ceny](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Rysunek 5**: komunikat o błędzie jest wyświetlany podczas wprowadzania ujemna cena ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>Podsumowanie

GridView i kontrolki ObjectDataSource zapewniają obsługi zdarzeń po poziomu, które zawierają informacje o wszelkich wyjątków, które zostały zgłoszone podczas aktualizowania i usuwania przepływu pracy, a także właściwości, które można ustawić, aby wskazać, czy wyjątek został obsługiwane. Te funkcje, jednak są niedostępne podczas pracy z kontrolki DataList i bezpośrednio przy użyciu LOGIKI. Zamiast tego odpowiadamy za Implementowanie obsługi wyjątków.

W tym samouczku widzieliśmy sposób dodawania obsługi wyjątków, aby można edytować s DataList aktualizowanie przepływu pracy, dodając `Try ... Catch` za pomocą bloku `UpdateCommand` programu obsługi zdarzeń. Jeśli wyjątek jest zgłaszany podczas aktualizowania przepływu pracy, `Catch` bloku s Kod jest wykonywany, wyświetlania informacji pomocnych w `ExceptionDetails` etykiety.

W tym momencie kontrolki DataList nie podejmuje żadnych działań zapobiegające wyjątki od dzieje się w pierwszej kolejności. Mimo że wiemy, że ceny spowodują wyjątek, że firma Microsoft t dodać jeszcze żadnej funkcji, aby aktywnie uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowe dane wejściowe. W naszym samouczku dalej zobaczymy, jak zmniejszyć wyjątki spowodowane przez nieprawidłowy użytkownik danych wejściowych przez dodawanie kontrolek weryfikacji w `EditItemTemplate`.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Wyjątki — zalecenia dotyczące projektowania](https://msdn.microsoft.com/library/ms298399.aspx)
- [Moduły rejestrowania błędów i obsługi (ELMAH)](http://workspaces.gotdotnet.com/elmah) (biblioteka typu open source dla rejestrowanie błędów)
- [Biblioteka przedsiębiorstwa dla programu .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (w tym bloku aplikacji zarządzania wyjątków)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Ken Pespisa. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](performing-batch-updates-cs.md)
> [dalej](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
