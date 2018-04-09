---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Wiele Elementy ContentPlaceHolders i zawartości domyślnej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sprawdza, czy sposób dodawania wielu posiadaczy miejscu zawartości do strony głównej, a także sposobu określania domyślnej zawartości w posiadaczy miejscu zawartości.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd1d8f34dba52a04c0d9f6a1961df7b97405b42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Wiele Elementy ContentPlaceHolders i zawartości domyślnej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Sprawdza, czy sposób dodawania wielu posiadaczy miejscu zawartości do strony głównej, a także sposobu określania domyślnej zawartości w posiadaczy miejscu zawartości.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku będziemy zbadane, jak strony wzorcowe Włącz deweloperów platformy ASP.NET, aby utworzyć spójnego układu całej lokacji. Strony wzorcowe zdefiniuj zarówno kod znaczników, który jest wspólny dla wszystkich jej strony zawartości, jak i regionów, które można dostosowywać na podstawie strony strona. W poprzednich instrukcji utworzyliśmy proste strony wzorcowej (`Site.master`) i dwa zawartości strony (`Default.aspx` i `About.aspx`). Nasze strony głównej zawiera dwa elementy ContentPlaceHolders o nazwie `head` i `MainContent`, które znajdowały się w `<head>` elementu i formularza sieci Web, odpowiednio. Gdy strony zawartości istniało dwóch formantów zawartości, określony tylko znaczników dla nich odpowiadający `MainContent`.

O dwóch formantów elementu ContentPlaceHolder w `Site.master`, strona wzorcowa może zawierać wiele Elementy ContentPlaceHolders. Co więcej strony wzorcowej może określić domyślnego znacznika elementu ContentPlaceHolder formantów. Strony zawartości, a następnie można opcjonalnie określić własne znaczników lub domyślnego znacznika. W tym samouczku będziemy przyjrzeć się przy użyciu wielu formantów zawartości na stronie głównej i zobacz, jak do definiowania domyślnego znacznika w formantach ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Krok 1: Dodawanie formantów ContentPlaceHolder dodatkowe do strony wzorcowej

Wiele projektów witryny sieci Web zawiera kilka obszarów na ekranie dostosowanych na podstawie strony strona. `Site.master`, strony wzorcowej utworzyliśmy w poprzednim samouczek zawiera pojedynczy element ContentPlaceHolder w formularzu sieci Web o nazwie `MainContent`. W szczególności ten element ContentPlaceHolder znajduje się w `mainContent` `<div>` elementu.

Rysunek 1 pokazuje `Default.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Region w czerwonym kółku jest znaczników specyficznym dla strony odpowiadający `MainContent`.


[![Region kółku zawiera obszar obecnie można dostosowywać na podstawie strony strona](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Rysunek 01**: Circled Region zawiera obszar obecnie modyfikowalny na podstawie strony strona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Załóżmy, że oprócz region pokazany na rysunku 1, również musimy dodać elementy specyficzne dla strony do lewej kolumnie poniżej lekcje i wiadomości sekcje. W tym celu możemy dodać inny formant ContentPlaceHolder strony wzorcowej. Aby z niego skorzystać, otwórz `Site.master` głównej stronie Visual Web Developer, a następnie przeciągnij formant ContentPlaceHolder z przybornika do projektanta po sekcji wiadomości. Ustaw element ContentPlaceHolder `ID` do `LeftColumnContent`.


[![Dodawanie formantu elementu ContentPlaceHolder do lewej kolumnie strony wzorcowej](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Rysunek 02**: Dodawanie formantu elementu ContentPlaceHolder kolumnę po lewej stronie wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Z dodatkiem `LeftColumnContent` elementu ContentPlaceHolder na stronie wzorcowej, firma Microsoft można zdefiniować zawartość dla tego obszaru na podstawie strony strona zawartości w tym kontrolować na stronie, których `ContentPlaceHolderID` ma ustawioną wartość `LeftColumnContent`. Firma Microsoft bada ten proces w kroku 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Krok 2: Definiowanie zawartości dla nowego elementu ContentPlaceHolder na stronach zawartości

Podczas dodawania nowej strony zawartości witryny sieci Web, Visual Web Developer automatycznie tworzy zawartość kontrolki na stronie dla każdego elementu ContentPlaceHolder na wybranej strony wzorcowej. Posiadanie dodane `LeftColumnContent` ContentPlaceHolder do strony głównej w kroku 1, nowe ASP.NET strony będzie teraz ma trzy formanty zawartości.

Na przykład Dodaj nową stronę zawartości do katalogu głównego o nazwie `MultipleContentPlaceHolders.aspx` który jest powiązany `Site.master` strony wzorcowej. Visual Web Developer tworzy tę stronę z następujących deklaratywne kod znaczników:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Wprowadź część zawartości do formantu zawartości odwołujące się do `MainContent` Elementy ContentPlaceHolders (`Content2`). Następnie dodaj następujący kod do `Content3` zawartości formantu (które odwołania `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Po dodaniu tego znacznika, odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 3, znaczników umieszczone w `Content3` zawartości formant jest wyświetlany w lewej kolumnie poniżej sekcji wiadomości (w czerwonym kółku). Kod znaczników umieszczone w `Content2` jest wyświetlany w prawej części strony (zaznaczona kółkiem na niebiesko).


[![Lewa kolumna zawiera teraz specyficznym dla strony zawartości poniżej sekcji wiadomości](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Rysunek 03**: po lewej kolumnie teraz obejmuje specyficznym dla strony zawartości poniżej sekcji wiadomości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definiowanie zawartości w istniejących stron zawartości

Automatyczne tworzenie nowej strony zawartości zawiera kontrolki elementu ContentPlaceHolder dodane w kroku 1. Ale dwóch istniejących stron zawartości - `About.aspx` i `Default.aspx` — nie zawiera formantu zawartości dla `LeftColumnContent` ContentPlaceHolder. Aby określić zawartości dla tego elementu ContentPlaceHolder na tych dwóch istniejących stron, należy dodać zawartości kontroli nad.

W przeciwieństwie do większości formantów sieci Web ASP.NET Visual przybornika deweloperów sieci Web nie zawiera elementu kontroli zawartości. Firma Microsoft ręcznie wpisać w znaczniku deklaratywne kontrolki zawartości w widoku źródła, ale łatwiejsze i szybsze rozwiązaniem jest użycie widoku Projekt. Otwórz `About.aspx` strony i przejdź do widoku projektu. Jak pokazano na rysunku 4, `LeftColumnContent` ContentPlaceHolder pojawia się w widoku Projekt; Jeśli myszy nad nim wyświetlany tytuł odczytuje: "LeftColumnContent (Master)". Włączenie "Master" w tytule wskazuje brak kontroli zawartości zdefiniowana dla tego elementu ContentPlaceHolder na stronie. Jeśli istnieje formantu zawartości dla elementu ContentPlaceHolder, tak jak w przypadku `MainContent`, odczyta tytuł: "*ContentPlaceHolderID* (niestandardowy)."

Aby dodać kontrolkę zawartości dla `LeftColumnContent` elementu ContentPlaceHolder na `About.aspx`, rozwiń element ContentPlaceHolder tagów inteligentnych i kliknij łącze, Utwórz niestandardowe zawartość.


[![Widok projektu dla About.aspx zawiera element LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Rysunek 04**: widok projektu dla `About.aspx` pokazuje `LeftColumnContent` ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Utwórz niestandardowe zawartość łącze generuje niezbędnych zawartości kontrolki na stronie i ustawia jej `ContentPlaceHolderID` właściwości do elementu ContentPlaceHolder `ID`. Na przykład kliknięcie linku do tworzenia niestandardowych zawartości `LeftColumnContent` regionu w `About.aspx` dodaje następujące deklaratywne znaczników do strony:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Pominięcie formanty zawartości

ASP.NET nie jest wymagane, że wszystkie strony z zawartością obejmują formantów zawartości dla każdego elementu ContentPlaceHolder zdefiniowane na stronie głównej. W przypadku pominięcia zawartości formantu aparatu ASP.NET używa znacznika zdefiniowany w ramach elementu ContentPlaceHolder na stronie głównej. Ten kod znaczników jest określany jako elementu ContentPlaceHolder *domyślne zawartości* co jest przydatne w sytuacjach, gdy zawartość dla niektórych region jest wspólny dla większości stron, ale musi można dostosować dla niewielkiej liczby stron. Krok 3 Eksploruje określania domyślnej zawartości na stronie głównej.

Obecnie `Default.aspx` zawiera dwa formantów zawartości do `head` i `MainContent` Elementy ContentPlaceHolders; nie ma zawartości formantu dla `LeftColumnContent`. W związku z tym, kiedy `Default.aspx` jest renderowany `LeftColumnContent` jest używany przez element ContentPlaceHolder domyślnej zawartości. Ponieważ mamy jeszcze do zdefiniowania żadnej zawartości domyślnej dla tego elementu ContentPlaceHolder, net powoduje, że dla tego regionu jest emitowany nie znaczników. Aby sprawdzić to zachowanie, odwiedź stronę `Default.aspx` za pośrednictwem przeglądarki. Jak pokazano na rysunku nr 5, znaczników nie są emitowane w lewej kolumnie poniżej sekcji wiadomości.


[![Brak zawartości nie jest renderowany element LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Rysunek 05**: zawartości nie jest renderowany `LeftColumnContent` ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Krok 3: Określanie zawartości domyślnej w strony wzorcowej

Niektóre projekty witryny sieci Web obejmują regionu, w których zawartość jest taka sama dla wszystkich stron w witrynie, z wyjątkiem jednego lub dwóch wyjątków. Należy wziąć pod uwagę witryny sieci Web, która obsługuje konta użytkowników. Witryny sieci wymaga strony logowania, gdy osoby odwiedzające mogą wprowadzić swoje poświadczenia logowania do witryny. Aby przyspieszyć proces logowania, projektantom witryn sieci Web może zawierać nazwy użytkownika i hasła pól tekstowych w lewym górnym rogu każdej strony, aby umożliwić użytkownikom logowania się bez konieczności jawnie odwiedź stronę logowania. Te pola tekstowe Nazwa użytkownika i hasło są przydatne w większości stron, są one nadmiarowe na stronie logowania, która zawiera już pól tekstowych dla poświadczeń użytkownika.

Aby zaimplementować ten projekt, można utworzyć formantu elementu ContentPlaceHolder w lewym górnym rogu strony wzorcowej. Każdej strony, który wymagany do wyświetlania pola tekstowe nazwy użytkownika i hasła w ich lewym górnym rogu czy tworzenie formantu zawartości dla tego elementu ContentPlaceHolder, a następnie Dodaj interfejs niezbędne. Strona logowania z drugiej strony, albo czy pominąć Dodawanie formantu zawartości dla tego elementu ContentPlaceHolder lub utworzyć zawartość formantu o nie znaczników zdefiniowane. Wadą interfejsu tego podejścia jest, że mamy Pamiętaj, aby dodać pola tekstowe nazwy użytkownika i hasła na każdej stronie, które możemy dodać do lokacji (z wyjątkiem strony logowania). To jest podanie problemy. Jesteśmy mogących dodanie tych pól tekstowych do strony lub dwóch, lub gorsza, firma Microsoft może nie implementuje interfejsu poprawnie (prawdopodobnie dodanie tylko jednego pola tekstowego zamiast dwóch).

Lepszym rozwiązaniem jest określenie pola tekstowe Nazwa użytkownika i hasło jako elementu ContentPlaceHolder domyślnej zawartości. Dzięki temu tylko należy zastąpić zawartości tej domyślnej w tych kilka stron, które nie są wyświetlane pola tekstowe Nazwa użytkownika i hasło (logowanie strona, na przykład). Aby zilustrować określania domyślnej zawartości kontrolki elementu ContentPlaceHolder, możemy wdrożyć scenariusz dla omówione wystarczy.

> [!NOTE]
> W pozostałej części tego samouczka aktualizuje naszą witrynę sieci Web, aby uwzględnić interfejs logowania w lewej kolumnie dla wszystkich stron, ale strony logowania. Jednak w tym samouczku nie bada sposobu konfigurowania witryny sieci Web do obsługi kont użytkowników. Aby uzyskać więcej informacji na ten temat, zapoznaj się Moje [formy uwierzytelniania, autoryzacji, kont użytkowników i ról](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) samouczki.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Dodawanie elementu ContentPlaceHolder i określanie jego zawartości domyślnej

Otwórz `Site.master` strony wzorcowej i Dodaj następujący kod do lewej kolumnie między `DateDisplay` etykiety i lekcje sekcji:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Po dodaniu tego znacznika strony wzorcowej widoku Projekt powinien być podobny do rysunek 6.


[![Strona wzorcowa obejmuje formantu logowania](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Rysunek 06**: strona wzorcowa obejmuje formantu logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Ten element ContentPlaceHolder, `QuickLoginUI`, zawiera formant sieci Web logowania jako jego zawartości domyślnej. Kontrolka logowania wyświetla interfejsu użytkownika, które monituje użytkownika o swojej nazwy użytkownika i hasła oraz przycisk Zaloguj. Po kliknięciu przycisku Zaloguj formantu logowania wewnętrznie weryfikuje poświadczenia użytkownika do interfejsu API członkostwa. Aby użyć tej kontrolki nazwy logowania w praktyce, następnie należy skonfigurować witrynę tak, aby używać członkostwa. W tym temacie wykracza poza zakres tego samouczka; Zapoznaj się Moje [formy uwierzytelniania, autoryzacji, kont użytkowników i ról](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) samouczki, aby uzyskać więcej informacji na temat tworzenia aplikacji sieci web, która obsługuje konta użytkowników.

Możesz dostosować zachowanie lub wygląd formantu logowania. Ustawię dwie właściwości: `TitleText` i `FailureAction`. `TitleText` Wartość właściwości, która domyślnie określa "W dzienniku", jest wyświetlany w górnej części formantu interfejsu użytkownika. Ta właściwość ma ustawić tak, czy tekst "Zaloguj" jest wyświetlany jako `<h3>` elementu. `FailureAction` Właściwość wskazuje co zrobić, jeśli poświadczenia użytkownika są nieprawidłowe. Domyślne wartości `Refresh`, co pozostawia użytkownika na tej samej stronie i wyświetla komunikat o błędzie w formancie logowania. Po zmianie jego `RedirectToLoginPage`, który wysyła do strony logowania, w przypadku nieprawidłowe poświadczenia użytkownika. Chcę wysłać do użytkownika do strony logowania, gdy użytkownik próbuje się zalogować z innej strony, ale nie powiedzie się, ponieważ strony logowania może zawierać dodatkowe informacje i opcje, które nie łatwo mieści się w lewej kolumnie. Na przykład strony logowania może obejmować opcje, aby odzyskać zapomniane hasło lub utworzyć nowe konto.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Tworzenie strony logowania i zastępowanie zawartości domyślnej

Ze stroną wzorcową pełną naszych następnym krokiem jest do utworzenia strony logowania. Dodaj stronę ASP.NET do katalogu głównego witryny o nazwie `Login.aspx`, powiązanie go do `Site.master` strony wzorcowej. W ten sposób spowoduje utworzenie stronę z czterech formantów zawartości, po jednej dla każdego z Elementy ContentPlaceHolders zdefiniowane w `Site.master`.

Dodawanie do formantu logowania `MainContent` zawartości formantu. Podobnie, możesz także dodać zawartość do `LeftColumnContent` regionu. Jednakże, upewnij się, że pozostaw formantu zawartości dla `QuickLoginUI` ContentPlaceHolder pusta. To spowoduje upewnij się, że logowanie formant nie jest wyświetlany w lewej kolumnie strony logowania.

Po zdefiniowaniu zawartość `MainContent` i `LeftColumnContent` regionach, znaczników deklaratywne stronę logowania powinien wyglądać podobnie do następującego:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Rysunek nr 7 przedstawia tę stronę, podczas wyświetlania za pośrednictwem przeglądarki. Ponieważ ta strona Określa formant zawartości dla `QuickLoginUI` ContentPlaceHolder, przesłania zawartości domyślnej określone na stronie głównej. Net powoduje, że formant logowania wyświetlany w projekcie strony wzorcowej widoku (patrz rysunek 6) nie są odtwarzane na tej stronie.


[![Element QuickLoginUI ContentPlaceHolder domyślnej zawartości Represses strony logowania](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Rysunek 07**: Represses strony logowania `QuickLoginUI` elementu ContentPlaceHolder na domyślne zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Przy użyciu domyślnej zawartości w nowych stron

Chęć pokazania formantu logowania w lewej kolumnie dla wszystkich stron z wyjątkiem strony logowania. Aby to osiągnąć, strony zawartości z wyjątkiem strony logowania, musi pominąć formantu zawartości dla `QuickLoginUI` ContentPlaceHolder. Pomijając formantu zawartości, zawartość domyślnego elementu ContentPlaceHolder zostanie użyty.

Istniejących stron zawartości - `Default.aspx`, `About.aspx`, i `MultipleContentPlaceHolders.aspx` — nie dołączaj formantu zawartości dla `QuickLoginUI` ponieważ zostały utworzone przed dodaliśmy tego formantu elementu ContentPlaceHolder na stronie głównej. W związku z tym te istniejących stron zbędna, nie można zaktualizować. Jednak nowych stron dodane do witryny sieci Web obejmują formantu zawartości dla `QuickLoginUI` elementu ContentPlaceHolder domyślnie. W związku z tym mamy Pamiętaj, aby usunąć te elementy zawartości kontrolki zawsze możemy Dodaj nową stronę zawartości (o ile nie chcemy zastąpić element ContentPlaceHolder domyślnej zawartości, jak w przypadku strony logowania).

Aby usunąć formant zawartości, można ręcznie usunąć jego deklaratywne znaczników z widoku źródła lub, w widoku Projekt, wybierz domyślne do wzorca zawartości łącza z jego tagów inteligentnych. Podejście albo usuwa formantu zawartości ze strony i tworzy net taki sam efekt.

Rysunek nr 8 przedstawia `Default.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Odwołania, który `Default.aspx` ma tylko dwie formantów zawartości określony w znaczniku deklaratywne — jeden dla `head` i jeden dla `MainContent`. W rezultacie domyślne zawartości dla `LeftColumnContent` i `QuickLoginUI` Elementy ContentPlaceHolders są wyświetlane.


[![Domyślna zawartość LeftColumnContent i Elementy QuickLoginUI ContentPlaceHolders są wyświetlane](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Rysunek 08**: domyślna zawartość `LeftColumnContent` i `QuickLoginUI` wyświetlane są elementy ContentPlaceHolders ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Podsumowanie

Model strony wzorcowej ASP.NET pozwala na dowolnej liczby Elementy ContentPlaceHolders na stronie głównej. Co to jest więcej, Elementy ContentPlaceHolders obejmują domyślną zawartość, która jest emitowany w przypadku istnieje nieprawidłowość bez odpowiedniej zawartości kontrolki na stronie zawartości. W tym samouczku widzieliśmy sposób obejmują dodatkowe funkcje kontroli elementu ContentPlaceHolder na stronie głównej i zdefiniuj formantów zawartości dla te nowe elementy ContentPlaceHolders w istniejących i nowych stron ASP.NET. Również analizujemy określania wartości domyślnej zawartości w ContentPlaceHolder, co jest przydatne w scenariuszach, w których tylko mniejszości strony należy dostosować, w przeciwnym razie standaryzowane zawartości w danym regionie.

W następnym samouczku zajmiemy `head` ContentPlaceHolder bardziej szczegółowo w niewidoczny sposób deklaratywnego i programowo zdefiniować tytuł, tagi meta i innych nagłówków HTML dla strony strona.

Programowanie przyjemność!

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Suchi Banerjee. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-site-wide-layout-using-master-pages-vb.md)
> [dalej](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
