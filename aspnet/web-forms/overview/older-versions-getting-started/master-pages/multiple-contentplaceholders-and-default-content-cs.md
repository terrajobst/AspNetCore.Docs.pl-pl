---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Wiele kontrolek ContentPlaceHolder i zawartość domyślna (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sprawdza, czy sposób dodawania wielu posiadaczy zawartości miejsce na stronę wzorcową, a także sposobu określania domyślnej zawartości w posiadaczy miejscu zawartości.
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: 8e3a9146b22557691899bbfe299bc44682d1efae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804161"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>Wiele kontrolek ContentPlaceHolder i zawartość domyślna (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Sprawdza, czy sposób dodawania wielu posiadaczy zawartości miejsce na stronę wzorcową, a także sposobu określania domyślnej zawartości w posiadaczy miejscu zawartości.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku zbadaliśmy, jak strony wzorcowe Włącz deweloperów platformy ASP.NET do tworzenia spójnego układu dla całej witryny. Strony wzorcowe zdefiniować znaczniki, które są wspólne dla wszystkich stronach zawartości i regionów, które można dostosować na podstawie strony strona. W poprzednim samouczku utworzyliśmy proste strony wzorcowej (`Site.master`) i zawartości dwóch stronach (`Default.aspx` i `About.aspx`). Nasze strony wzorcowej składa się z dwóch kontrolek ContentPlaceHolder o nazwie `head` i `MainContent`, które znajdowały się w `<head>` elementu i formularz sieci Web, odpowiednio. Gdy stron zawartości ma dwa formanty zawartości, określony tylko znaczników dla nich odpowiadający `MainContent`.

Dowodem dwóch kontrolek ContentPlaceHolder w `Site.master`, strona wzorcowa może zawierać wiele kontrolek ContentPlaceHolder. Co więcej strona główna może określić domyślne znaczników dla kontrolek ContentPlaceHolder. Strony zawartości, a następnie, można opcjonalnie określić własny kod znaczników lub znaczniki domyślne. W tym samouczku będziemy Spójrz na używanie wielu kontrolek zawartości na stronie głównej i dowiedzieć się, jak zdefiniować domyślne znaczników do kontrolek ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Krok 1: Dodawanie kontrolek ContentPlaceHolder dodatkowe do strony wzorcowej

Wiele projektów witryny sieci Web zawiera kilka obszarów na ekranie, które są dostosowywane na podstawie strony strona. `Site.master`, strony wzorcowej utworzonego w poprzednim samouczku zawiera pojedynczy ContentPlaceHolder w obrębie formularza sieci Web o nazwie `MainContent`. W szczególności ten ContentPlaceHolder znajduje się w obrębie `mainContent` `<div>` elementu.

Rysunek 1 pokazuje `Default.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Region zakreślony na czerwono jest specyficzne dla strony kodu znaczników odpowiadający `MainContent`.


[![Region kółku zawiera obszar obecnie można dostosować na podstawie strony strona](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Rysunek 01**: Circled Region pokazuje obszaru aktualnie modyfikowalny na podstawie strony strona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


Wyobraź sobie, że oprócz regionów, w przedstawionej na rysunku 1, należy również dodać elementy specyficzne dla strony do lewej kolumnie pod — lekcje i wiadomości sekcje. Aby to osiągnąć, można dodać kolejną kontrolkę ContentPlaceHolder strony wzorcowej. Aby z niego skorzystać, otwórz `Site.master` master strony w Visual Web Developer, a następnie przeciągnij formant ContentPlaceHolder z przybornika w Projektancie po sekcji wiadomości. Ustaw ContentPlaceHolder `ID` do `LeftColumnContent`.


[![Dodawanie kontrolki ContentPlaceHolder do lewej kolumnie strony wzorcowej](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Rysunek 02**: Dodawanie formantu ContentPlaceHolder kolumnę po lewej stronie strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


Z dodatkiem `LeftColumnContent` ContentPlaceHolder strony wzorcowej, firma Microsoft można zdefiniować zawartość dla tego obszaru na podstawie strony Strona, w tym zawartości formant na stronie, którego `ContentPlaceHolderID` ustawiono `LeftColumnContent`. Sprawdzamy, ten proces w kroku 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Krok 2: Definiowanie zawartości dla nowych ContentPlaceHolder na stronach zawartości

Podczas dodawania nowej strony zawartości do witryny sieci Web, Visual Web Developer automatycznie tworzy zawartość kontrolki na stronie dla każdego elementu ContentPlaceHolder na wybranej strony wzorcowej. Posiadanie dodano `LeftColumnContent` ContentPlaceHolder do strony głównej w kroku 1. nowy ASP.NET stron będą teraz mieć trzy kontrolki zawartości.

Na przykład Dodaj nową stronę zawartości o nazwie w katalogu głównym `MultipleContentPlaceHolders.aspx` , jest powiązany z `Site.master` strony wzorcowej. Visual Web Developer tworzy tę stronę przy użyciu następujących oznaczeniu deklaracyjnym:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Wprowadź odpowiednią zawartość do formantu zawartości odwołujące się do `MainContent` kontrolek ContentPlaceHolder (`Content2`). Następnie dodaj następujący kod do `Content3` formantu zawartości (która odwołuje się do `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Po dodaniu ten kod znaczników, odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 3, znaczniki są umieszczane w `Content3` formant zawartości jest wyświetlany w lewej kolumnie poniżej sekcji wiadomości (zakreślony na czerwono). Znaczniki są umieszczane w `Content2` jest wyświetlany w prawej części strony (w kółkach w kolorze niebieskim).


[![Lewa kolumna zawiera teraz zawartości dla strony poniżej sekcji wiadomości](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Rysunek 03**: po lewej stronie kolumny teraz obejmuje specyficzne dla strony zawartości poniżej sekcji wiadomości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definiowanie zawartości w istniejących stron zawartości

Automatyczne tworzenie nowej strony zawartości zawiera kontrolki ContentPlaceHolder zapoczątkowany w kroku 1. Ale dwóch istniejących stron zawartości — `About.aspx` i `Default.aspx` — nie ma zawartości kontrolki dla `LeftColumnContent` ContentPlaceHolder. Aby określić zawartość dla tego elementu ContentPlaceHolder na tych dwóch istniejących stron, musimy dodać kontrolkę zawartości, określić główną przyczynę.

W przeciwieństwie do większości formantów sieci Web platformy ASP.NET Visual przybornika dla deweloperów sieci Web nie ma element zawartości formantu. Firma Microsoft ręcznie wpisać w oznaczeniu deklaracyjnym formantu zawartości w widoku źródła, ale łatwiej i szybciej podejściem jest użycie widoku projektu. Otwórz `About.aspx` strony, a następnie przełączyć do widoku projektu. Jak rysunek 4 przedstawia, `LeftColumnContent` ContentPlaceHolder pojawia się w widoku Projekt; Jeśli przesuniesz wskaźnik myszy nad nią tytuł widoczny odczytuje: "LeftColumnContent (Master)." Włączenie gałęzią "główną" w tytule oznacza, że są nie formantu zawartości, które zostały zdefiniowane na stronie dla tego elementu ContentPlaceHolder. Jeśli istnieje kontrolkę zawartości dla ContentPlaceHolder, tak jak w przypadku `MainContent`, tytuł będzie odczytywał element: "*ContentPlaceHolderID* (niestandardowy)."

Aby dodać kontrolkę zawartości dla `LeftColumnContent` ContentPlaceHolder do `About.aspx`, rozwiń tagu inteligentnego ContentPlaceHolder i kliknij link, Utwórz niestandardowe zawartość.


[![Widok projektu About.aspx pokazuje LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Rysunek 04**: widok projektu `About.aspx` pokazuje `LeftColumnContent` ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


Kliknięcie linku Utwórz niestandardowy zawartość generuje niezbędne zawartości kontrolki na stronie i ustawia jego `ContentPlaceHolderID` właściwość ContentPlaceHolder `ID`. Na przykład kliknięcie linku do tworzenia niestandardowych zawartości `LeftColumnContent` regionu w `About.aspx` dodaje następujące oznaczeniu deklaracyjnym strony:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Pominięcie formanty zawartości

Program ASP.NET nie wymaga, że wszystkie strony zawartości zawierają formanty zawartości dla każdego elementu ContentPlaceHolder zdefiniowane na stronie głównej. Jeśli kontrolki zawartości zostanie pominięty, aparat platformy ASP.NET używa znaczników zdefiniowane ContentPlaceHolder na stronie głównej. Ten kod znaczników jest określany jako ContentPlaceHolder *domyślnej zawartości* co jest przydatne w scenariuszach, w którym zawartość dla niektórych region jest wspólne dla większości stron, ale musi można dostosować dla niewielkiej liczby stron. Krok 3 przedstawiono Określanie domyślnej zawartości na stronie głównej.

Obecnie `Default.aspx` zawiera dwie kontrolki zawartości dla `head` i `MainContent` kontrolek ContentPlaceHolder; nie ma zawartości kontrolki dla `LeftColumnContent`. W związku z tym, kiedy `Default.aspx` jest renderowany `LeftColumnContent` firmy ContentPlaceHolder i zawartość domyślna jest używana. Ponieważ mamy do definiowania zawartości domyślny dla tego elementu ContentPlaceHolder jeszcze efektem sieciowym jest czy żadnych znaczników jest emitowane dla tego regionu. Aby sprawdzić, czy ten problem, odwiedź stronę `Default.aspx` za pośrednictwem przeglądarki. Jak pokazano na rysunku 5, żadnych znaczników jest emitowane w lewej kolumnie poniżej sekcji wiadomości.


[![Żadna zawartość nie jest renderowany LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Rysunek 05**: Brak zawartości nie jest renderowany `LeftColumnContent` ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Krok 3: Określanie zawartości domyślnej stronie wzorcowej

Niektóre projekty witryny sieci Web obejmują regionu, w których zawartość jest taka sama dla wszystkich stron w witrynie, z wyjątkiem co najmniej dwa wyjątki. Należy wziąć pod uwagę witryny sieci Web, która obsługuje konta użytkowników. Witryny sieci wymaga strony logowania, w którym osoby odwiedzające mogą wprowadzać poświadczeń do logowania do witryny. Aby przyspieszyć proces logowania, projektantom witryn sieci Web mogą zawierać pola tekstowe nazwy użytkownika i hasła w lewym górnym rogu każdej strony, aby zezwolić użytkownikom na logowanie bez konieczności jawnego znajduje się na stronie logowania. Te pola tekstowe nazwy użytkownika i hasła są przydatne w większości stron, są one nadmiarowe na stronie logowania, która już zawiera pola tekstowe, aby uzyskać poświadczenia użytkownika.

Aby zaimplementować ten projekt, można utworzyć kontrolki ContentPlaceHolder w lewym górnym rogu strony wzorcowej. Każda strona wymagana w celu wyświetlenia pola tekstowe nazwy użytkownika i hasła w jego lewym górnym rogu będzie utworzyć kontrolkę zawartości dla tego elementu ContentPlaceHolder, a następnie Dodaj interfejs niezbędne. Strony logowania, z drugiej strony, albo może pominąć Dodawanie formantu zawartości dla tego elementu ContentPlaceHolder lub utworzyć zawartość kontrolki z żadnych znaczników zdefiniowane. Wadą tego podejścia jest to, że mamy Pamiętaj, aby dodać pola tekstowe nazwy użytkownika i hasła na każdej stronie, które możemy dodać do lokacji (z wyjątkiem na stronie logowania). Jest to pytanie o problemy. Jesteśmy może zapomniano dodać te pola tekstowe do strony lub dwóch, lub co gorsza, firma Microsoft może nie implementuje interfejsu poprawnie (prawdopodobnie dodanie tylko jednego pola tekstowego zamiast dwóch).

Lepszym rozwiązaniem jest zdefiniowanie pola tekstowe nazwy użytkownika i hasła jako ContentPlaceHolder domyślnej zawartości. Dzięki temu tylko należy zastąpić zawartości tej domyślnej tych kilku stronach, które nie są wyświetlane pola tekstowe nazwy użytkownika i hasła (logowanie strona, na przykład). Aby zilustrować Określanie domyślnej zawartości kontrolki ContentPlaceHolder, Przyjrzyjmy implementować scenariusza omówione tylko.

> [!NOTE]
> W pozostałej części tego samouczka aktualizuje naszej witryny sieci Web, aby uwzględnić interfejs logowania w lewej kolumnie dla wszystkich stron, ale do strony logowania. Jednak w tym samouczku nie analizuje sposób konfigurowania witryny sieci Web do obsługi kont użytkowników. Aby uzyskać więcej informacji na ten temat, zobacz mój [formy uwierzytelniania, autoryzacji, konta użytkowników i ról](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) samouczków.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Dodawanie ContentPlaceHolder i określanie jego domyślnej zawartości

Otwórz `Site.master` strony wzorcowej i Dodaj następujący kod do lewej kolumnie między `DateDisplay` etykiety i lekcje sekcji:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Po dodaniu ten kod znaczników widoku projektu strony wzorcowej powinien wyglądać podobnie jak rysunek 6.


[![Strona wzorcowa zawiera kontrolki logowania](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Rysunek 06**: strona wzorcowa zawiera kontrolkę logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


Ta ContentPlaceHolder `QuickLoginUI`, zawiera formant Web zaloguj się jako jego domyślnej zawartości. Kontrolka Login wyświetla interfejsu użytkownika, który monituje użytkownika dla nazwy użytkownika i hasła oraz przycisk Zaloguj. Po kliknięciu przycisk Zaloguj, kontrolka Login wewnętrznie weryfikuje poświadczenia użytkownika przy użyciu interfejsu API członkostwa. Aby użyć tej kontrolki logowania w praktyce, następnie należy skonfigurować witrynę w taki sposób, aby użyć członkostwa. W tym temacie wykracza poza zakres tego samouczka; można znaleźć Moje [formy uwierzytelniania, autoryzacji, konta użytkowników i ról](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) samouczki, aby uzyskać więcej informacji na temat tworzenia aplikacji sieci web, która obsługuje konta użytkowników.

Możesz dostosować zachowania i wyglądu kontrolki logowania. Ustawiam dwie właściwości: `TitleText` i `FailureAction`. `TitleText` Wartości właściwości, która domyślnie "Log In", jest wyświetlany w górnej części kontrolki interfejsu użytkownika. Tak, aby wyświetlała tekst "Sign In" jako mają jest ustawić tę właściwość `<h3>` elementu. `FailureAction` Właściwość wskazuje co należy zrobić, jeśli poświadczenia użytkownika są nieprawidłowe. Domyślnie wartość `Refresh`, która pozostawia użytkownika na tej samej stronie i wyświetla komunikat o błędzie w ramach kontrolka Login. Po zmianie jego `RedirectToLoginPage`, która wysyła do strony logowania w przypadku nieprawidłowe poświadczenia użytkownika. Chcę wysłać użytkownika do strony logowania, gdy użytkownik próbuje się zalogować z innej strony, ale kończy się niepowodzeniem, ponieważ na stronie logowania może zawierać dodatkowe informacje i opcje, które nie będzie łatwo mieści się w lewej kolumnie. Na przykład na stronie logowania może obejmować opcje, aby odzyskać zapomniane hasło lub Utwórz nowe konto.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Tworzenie strony logowania i zastąpienie domyślnej zawartości

Za pomocą strony wzorcowej pełną naszym kolejnym krokiem jest utworzenie strony logowania. Dodawanie strony ASP.NET do katalogu głównego witryny o nazwie `Login.aspx`, wiążące go do `Site.master` strony wzorcowej. To spowoduje utworzenie strony z cztery formanty zawartości, jednej dla każdego z kontrolek ContentPlaceHolder zdefiniowane w `Site.master`.

Dodawanie kontrolki logowania do `MainContent` formantu zawartości. Podobnie, możesz dodać dowolną zawartość do `LeftColumnContent` regionu. Jednak pamiętaj pozostawić formantu zawartości dla `QuickLoginUI` ContentPlaceHolder puste. Pozwoli to zagwarantować, logowania, formant nie jest wyświetlany w lewej kolumnie strony logowania.

Po zdefiniowaniu zawartość `MainContent` i `LeftColumnContent` regionów, w oznaczeniu deklaracyjnym strony logowania powinien wyglądać podobnie do następującego:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

Rysunek nr 7 przedstawia tej strony, podczas wyświetlania za pośrednictwem przeglądarki. Ponieważ ta strona określa kontrolkę zawartości dla `QuickLoginUI` ContentPlaceHolder, zastępuje ona domyślnej zawartości określonego na stronie głównej. Efektem sieciowym jest czy kontrolka Login wyświetlane na liście projektu strony wzorcowej (patrz rysunek 6) nie renderowania widoku na tej stronie.


[![Na stronie logowania Represses QuickLoginUI ContentPlaceHolder domyślnej zawartości](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Rysunek 07**: Represses strony logowania `QuickLoginUI` firmy ContentPlaceHolder domyślnej zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Przy użyciu domyślnej zawartości w nowych stron

Chcemy wyświetlić kontrolki logowania w lewej kolumnie dla wszystkich stron z wyjątkiem strony logowania. Aby to osiągnąć, stron zawartości z wyjątkiem strony logowania należy pominąć znak kontrolkę zawartości dla `QuickLoginUI` ContentPlaceHolder. Pomijając kontrolkę zawartości, zawartość domyślna ContentPlaceHolder w zamian zostanie użyta.

Istniejących stron zawartości — `Default.aspx`, `About.aspx`, i `MultipleContentPlaceHolders.aspx` — nie zawierają kontrolki zawartości dla `QuickLoginUI` ponieważ zostały utworzone przed dodaliśmy tę kontrolkę ContentPlaceHolder strony wzorcowej. Dlatego te istniejących stron nie są do zaktualizowania. Jednak nowe strony dodane do witryny sieci Web obejmują formantu zawartości dla `QuickLoginUI` ContentPlaceHolder domyślnie. Możemy więc Pamiętaj, aby usunąć te kontrolki zawartości zawsze możemy dodać nową stronę zawartości (chyba że chcemy zastąpić ContentPlaceHolder domyślnej zawartości, jak w przypadku na stronie logowania).

Aby usunąć formant zawartości, można ręcznie usunąć jego oznaczeniu deklaracyjnym z widoku źródła lub, w widoku Projekt, wybierz domyślną linku zawartości głównego w tagu inteligentnego. Każda z tych metod usuwa formantu zawartości ze strony i tworzy net taki sam efekt.

Rysunek 8 przedstawia `Default.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Pamiętamy `Default.aspx` ma tylko dwie kontrolki zawartości określone w oznaczeniu deklaracyjnym — jeden dla `head` i jeden dla `MainContent`. W wyniku zawartości dla domyślnego `LeftColumnContent` i `QuickLoginUI` kontrolek ContentPlaceHolder są wyświetlane.


[![Domyślnie zawartość LeftColumnContent i kontrolek ContentPlaceHolder QuickLoginUI są wyświetlane](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Rysunek 08**: domyślna zawartości dla `LeftColumnContent` i `QuickLoginUI` kontrolek ContentPlaceHolder są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>Podsumowanie

Model stronę wzorcową platformy ASP.NET pozwala na dowolną liczbę kontrolek ContentPlaceHolder na stronie głównej. Co to jest więcej, kontrolek ContentPlaceHolder obejmują zawartość domyślna, która jest emitowane w przypadku, że istnieje bez odpowiedniej zawartości kontrolki na stronie zawartości. W tym samouczku widzieliśmy, jak dołączać dodatkowe elementy sterujące ContentPlaceHolder na stronie głównej i jak zdefiniować kontrolek zawartości do tych nowych kontrolek ContentPlaceHolder w istniejących i nowych stron ASP.NET. Również przyjrzeliśmy się określanie ustawień domyślnych zawartość ContentPlaceHolder, co jest przydatne w scenariuszach, gdzie tylko mniejszości stron w zakresie dostosowywania, w przeciwnym razie standaryzowane zawartości w danym regionie.

W następnym samouczku zajmiemy się `head` ContentPlaceHolder bardziej szczegółowo, widzisz jak sposób deklaratywny i programowy zdefiniować tytułu, tagów meta i innych nagłówków HTML na podstawie strony strona.

Wszystkiego najlepszego programowania!

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Suchi Banerjee. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-site-wide-layout-using-master-pages-cs.md)
> [dalej](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
