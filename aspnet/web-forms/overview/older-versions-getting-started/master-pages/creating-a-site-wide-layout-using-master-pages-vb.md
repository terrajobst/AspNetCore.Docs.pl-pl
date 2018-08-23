---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Tworzenie układu dla całej witryny za pomocą stron wzorcowych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek przedstawia podstawowe informacje dotyczące strony wzorcowej. To znaczy, jakie są strony wzorcowe, jak działa jeden Tworzenie strony wzorcowej, co to są posiadacze miejscu zawartości, jak działa jeden cr...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: feb04c19092101bb019883c8b72b40ceb9afc015
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752305"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Tworzenie układu dla całej witryny za pomocą stron wzorcowych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Ten samouczek przedstawia podstawowe informacje dotyczące strony wzorcowej. To znaczy, co to są strony wzorcowe, jak jeden tworzy stronę wzorcową, jakie są symbole zastępcze zawartości, jak jeden tworzy strony ASP.NET, która korzysta z strony wzorcowej, w jaki sposób modyfikowania strony wzorcowej jest automatycznie odzwierciedlana we skojarzone strony z zawartością i tak dalej.


## <a name="introduction"></a>Wprowadzenie

Jeden atrybut dobrze zaprojektowanego witryny sieci Web jest układ spójne strony całej lokacji. Przyjmować www.asp.net witryny sieci Web, na przykład. W momencie pisania tego dokumentu każdej strony ma taką samą zawartość, u góry i u dołu strony. Jak pokazano na rysunku 1, bardzo u góry każdej strony wyświetli szarym pasku listę Communities firmy Microsoft. Poniżej to znaczy logo witryny, listy języków, w których został przetłumaczony lokacji i sekcje core: Strona główna, wprowadzenie, Dowiedz się więcej, pliki do pobrania i tak dalej. Podobnie dolnej części strony zawiera informacje o reklamie na www.asp.net, informacje o prawach autorskich oraz łącze do zasad zachowania poufności.


[![Witryny sieci Web www.asp.net stosuje spójny wygląd i zachowanie na wszystkich stronach](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Rysunek 01</strong>: www.asp.net witryny sieci Web używającego spójny wygląd i możesz ją we wszystkich stron ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Inny atrybut dobrze zaprojektowanego lokacji jest łatwe za pomocą którego można zmienić wygląd witryny. Rysunek 1 pokazuje strony głównej www.asp.net począwszy od marca 2008, ale od chwili opublikowania tego samouczka zostały zmienione wyglądu i działania. Być może elementy menu u góry rozwinie do uwzględnienia dodania nowej sekcji dla platformy MVC. Lub może być znacznie nowy projekt za pomocą różnych kolorów, czcionek i układ będzie nieobsługujące. Stosowanie tych zmian w całej witrynie powinna być szybki i prosty proces, który nie wymaga modyfikowania tysięcy stron witryny sieci web, które tworzą witryny.

Tworzenie szablonu normalnej strony w programie ASP.NET: jest to możliwe za pośrednictwem *strony wzorcowe*. Mówiąc, strony wzorcowej jest specjalnym typem strony ASP.NET, która definiuje znaczniki, które są wspólne dla wszystkich *stron zawartości* oraz regionów, które można dostosować na podstawie zawartości strony zawartości strony. (Strony zawartości jest powiązany z strony wzorcowej strony ASP.NET). Po każdej zmianie układ strony wzorcowej lub formatowania, wszystkich jej zawartości strony w danych wyjściowych jest również natychmiast aktualizowane, co sprawia, że stosowanie zmian wyglądu normalnej tak łatwe jak aktualizowanie i wdrażanie pojedynczego pliku (to znaczy, strony głównej).

To jest pierwszy samouczek z serii samouczków dowiesz się, za pomocą stron wzorcowych. W trakcie tej serii samouczków firma Microsoft:

- Sprawdź, tworzenie stron wzorcowych i skojarzonych z nimi stron zawartości,
- Omówiono szereg porady, sztuczki i pułapki,
- Identyfikowanie typowych pułapek stronę wzorcową i obejścia problemu, zapoznaj się z
- Zobacz, jak uzyskać dostęp do strony wzorcowej z zawartości strony i na odwrót,
- Dowiedz się, jak określić strony zawartości strony wzorcowej w czasie wykonywania, a
- Inne zaawansowane tematy strony wzorcowej.

Te samouczki są dostosowane do być zwięzłe i zapewnić instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie przeprowadzi Cię przez proces. Każdy samouczek jest dostępny w C# i Visual Basic wersji i obejmuje pobieranie kompletny kod używany.

W tym samouczku inauguracyjnym rozpoczyna się od poznać podstawy strony wzorcowej. Firma Microsoft omówiono, jak strony wzorcowe pracy, Przyjrzyj się tworzenie strony wzorcowej i skojarzonych stron zawartości przy użyciu programu Visual Web Developer i zobacz, jak zmiany na stronie wzorcowej są natychmiast odzwierciedlane na swoich stronach zawartości. Zaczynajmy!

## <a name="understanding-how-master-pages-work"></a>Zrozumienie, jak strony wzorcowe pracy

Tworzenie witryny sieci Web z układem spójne strony całej lokacji wymaga, że każdej strony sieci web emitować typowych znaczników formatowania, oprócz swojej zawartości niestandardowej. Na przykład podczas każdego wpisu samouczka lub forum na www.asp.net mają własne unikatowe zawartość, każda z tych stron również renderowanie szereg typowych `<div>` elementy wyświetlające łącza sekcji najwyższego poziomu: Strona główna, wprowadzenie, Dowiedz się więcej i tak dalej.

Istnieją różne techniki tworzenia stron sieci web za pomocą spójny wygląd i zachowanie. To podejście naiwni jest po prostu skopiuj i Wklej typowych znaczników układu do wszystkich stron sieci web, ale takie podejście ma wiele wad. Po pierwsze za każdym razem, gdy tworzona jest nowa strona, należy pamiętać, aby skopiować i wkleić zawartość udostępnioną do strony. Takie kopiowanie i wklejanie działań są dojrzałe do błędu przypadkowo kopiujesz tylko podzbiór udostępnionego znaczników do nowej strony. I do pierwszych go, to podejście sprawia, że zastępując istniejący wygląd całej witryny nową ból rzeczywistych, ponieważ należy edytować każdej pojedynczej strony w witrynie, aby korzystać z nowego wyglądu i działania.

Przed platformę ASP.NET w wersji 2.0 stronie deweloperzy często umieszcza wspólnego kodu znaczników w [kontrolki użytkownika](https://msdn.microsoft.com/library/y6wb1a0e.aspx) i następnie dodać te kontrolki użytkownika do każdej strony. Takie podejście wymaga developer strony Pamiętaj, aby ręcznie dodać kontrolki użytkownika do każdej strony, ale można łatwiej modyfikacji całej lokacji, ponieważ podczas aktualizowania typowych znaczników tylko formanty użytkownika wymagane do zmodyfikowania. Niestety Visual Studio .NET 2002 i 2003 — wersje programu Visual Studio będzie używany do tworzenia aplikacji 1.x ASP.NET — w renderowaniu w widoku Projekt szare pola kontrolki użytkownika. W związku z tym programistom stron przy użyciu tej metody nie posiada WYSIWYG środowisku czasu projektowania.

Braków za pomocą kontrolki użytkownika zostały rozwiązane w platformę ASP.NET w wersji 2.0 i Visual Studio 2005 wraz z wprowadzeniem *strony wzorcowe*. Strona wzorcowa jest specjalnym typem strony ASP.NET, który definiuje znaczniki całej witryny i *regionów* w przypadku, gdy skojarzone *stron zawartości* definiowanie ich niestandardowych znaczników. Jak widać w kroku 1, te regiony są definiowane przez kontrolek ContentPlaceHolder. Kontrolka ContentPlaceHolder oznacza po prostu pozycji w hierarchii kontroli strony wzorcowej, w którym niestandardowej zawartości można wstrzyknięte przez strony zawartości.

> [!NOTE]
> Podstawowych pojęć i funkcji strony wzorcowe, nie zmienił się od platformę ASP.NET w wersji 2.0. Jednak program Visual Studio 2008 oferuje obsługi w czasie projektowania dla zagnieżdżone strony wzorcowe, funkcja, która brakowało w programie Visual Studio 2005. Przyjrzymy się przy użyciu zagnieżdżone strony wzorcowe przyszłych samouczka.


Na rysunku 2 przedstawiono, jak może wyglądać stronę wzorcową dla www.asp.net. Należy pamiętać, że strony wzorcowej definiuje typowe układu dla całej witryny - znaczników w górnej, dolnej i prawej strony na każdej stronie, - oraz ContentPlaceHolder w środku — po lewej stronie, gdzie znajduje się zawartość unikatowy dla poszczególnych stron sieci web.


![Strona wzorcowa definiuje układu dla całej witryny i regionów, które można edytować na podstawie zawartości strony zawartości strony](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Rysunek 02**: strona wzorcowa definiuje układu dla całej witryny i regionów, które można edytować na podstawie zawartości strony zawartości strony


Po zdefiniowaniu strony wzorcowej może być powiązana z nowych stron ASP.NET za pomocą znaczników pola wyboru. Te strony ASP.NET, - o nazwie strony z zawartością — obejmują formantu zawartości dla wszystkich kontrolek ContentPlaceHolder strony wzorcowej. W przypadku odwiedzenia strony zawartości za pośrednictwem przeglądarki aparat ASP.NET tworzy stronę wzorcową hierarchii kontroli i wprowadza hierarchii kontroli poziomu strony zawartości w odpowiednich miejscach. Ta hierarchia połączonych kontroli jest renderowany i wynikowy HTML jest zwracany do przeglądarki użytkownika końcowego. W związku z tym poziomu strony zawartości emituje typowych znaczników, zdefiniowane w jego strony głównej poza kontrolek ContentPlaceHolder i znaczników dla strony, zdefiniowanych w jego własnej zawartości kontrolki. Rysunek 3 ilustruje tę koncepcję.


[![Znaczniki strony wymagane jest zespolone do strony wzorcowej](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Rysunek 03**: żądanie strony znaczników jest zespolone do strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Teraz, gdy Omówiliśmy się, jak strony wzorcowe pracy, Spójrzmy na tworzenie strony wzorcowej i skojarzonych stron zawartości przy użyciu programu Visual Web Developer.

> [!NOTE]
> Aby osiągnąć najszerszych możliwych odbiorców, witryny sieci Web platformy ASP.NET jest wbudowana w całej tej serii samouczków zostanie utworzona z użyciem ASP.NET 3.5 z firmy Microsoft w bezpłatnej wersji usługi Visual Studio 2008 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Jeśli nie zostały jeszcze uaktualnione do programu ASP.NET 3.5, nie martw się — kwestie omówione w tych samouczkach pracy równie dobrze z programem ASP.NET 2.0 i Visual Studio 2005. Jednak niektóre aplikacje pokaz mogą używać nowych funkcji programu .NET Framework w wersji 3.5; w przypadku używania funkcji specyficznych dla 3.5 czy mogę dołączyć należy pamiętać, że w tym artykule omówiono sposób implementacji podobne funkcje w wersji 2.0. Należy pamiętać, dostępne dla aplikacji demonstracyjnych pobierane każdego samouczek obiektu docelowego .NET Framework w wersji 3.5, co skutkuje `Web.config` pliku, który zawiera elementy konfiguracji specyficznej dla 3.5. Długi tekst krótki, jeśli masz jeszcze do zainstalowania programu .NET 3.5 na komputerze, następnie do pobrania aplikacji sieci web programu nie będzie działać bez usuwania znacznika specyficzne dla 3.5 z `Web.config`. Zobacz [analiza ASP.NET w wersji 3.5 firmy `Web.config` pliku](http://www.4guysfromrolla.com/articles/121207-1.aspx) Aby uzyskać więcej informacji na ten temat.


## <a name="step-1-creating-a-master-page"></a>Krok 1: Tworzenie strony wzorcowej

Zanim omówimy tworzenia i używania stron głównych i zawartości, najpierw należy w witrynie sieci Web platformy ASP.NET. Rozpocznij od utworzenia nowego pliku w oparciu o system ASP.NET witryny sieci Web. Aby to zrobić, uruchom Visual Web Developer a następnie przejdź do menu Plik i wybierz nową witrynę sieci Web w celu wyświetlania okna dialogowego nową witrynę sieci Web (zobacz rysunek 4). Wybierz szablon witryny sieci Web platformy ASP.NET, zmień wartość na liście rozwijanej lokalizacji w systemie plików, wybierz folder do umieszczenia w witrynie sieci web i ustawienie języka Visual Basic. Spowoduje to utworzenie nowej witryny sieci web za pomocą `Default.aspx` strony ASP.NET `App_Data` folder, a `Web.config` pliku.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektami: projektów witryny sieci Web i projektów aplikacji sieci Web. Projektów witryny sieci Web brakuje pliku projektu, a Web Application Projects naśladować architektury projektu w Visual Studio .NET 2002/2003 — Dołącz plik projektu i skompilować kod źródłowy projektu do jednego zestawu, który jest umieszczony w `/bin` folder. Visual Studio 2005 projektów początkowo tylko obsługiwane witryny sieci Web, mimo że modelu projektu aplikacji sieci Web została przywrócona z dodatkiem Service Pack 1; Program Visual Studio 2008 oferuje oba modele projektu. Visual Web Developer 2005 i wersji 2008, ale obsługują tylko projektów witryny sieci Web. Czy mogę przy użyciu modelu projektu witryny sieci Web dla mojego pokazy w tej serii samouczków. Jeśli używasz wersji non-Express i chcesz użyć [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) zamiast tego możesz to zrobić, ale należy pamiętać, że może istnieć pewne rozbieżności między wyświetlanych na ekranie oraz czynności należy wykonać w porównaniu z zrzuty ekranu pokazano, jak i instrukcje podane w tych samouczkach.


[![Tworzenie nowego pliku w oparciu o System witryny sieci Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Rysunek 04**: tworzenie witryny sieci Web New File System-Based ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Następnie dodaj stronę wzorcową witryny w katalogu głównym, klikając prawym przyciskiem myszy nazwę projektu, wybierając Dodaj nowy element i wybierając szablon strony wzorcowej. Należy pamiętać, że strony wzorcowe kończyć się rozszerzeniem `.master`. Nazwa tej nowej strony wzorcowej `Site.master` i kliknij przycisk Dodaj.


[![Dodaj stronę wzorcową o nazwie Site.master do witryny sieci Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Rysunek 05**: Dodaj nazwanych strony główne `Site.master` do witryny internetowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Dodawanie nowego pliku strony głównej przy użyciu programu Visual Web Developer tworzy stronę wzorcową z następujących oznaczeniu deklaracyjnym:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

Pierwszy wiersz w oznaczeniu deklaracyjnym jest [ `@Master` dyrektywy](https://msdn.microsoft.com/library/ms228176.aspx). `@Master` Dyrektywa jest podobny do [ `@Page` dyrektywy](https://msdn.microsoft.com/library/ydy4x04a.aspx) wyświetlany na stronach ASP.NET. Definiuje języka po stronie serwera (VB) i informacje o lokalizacji i dziedziczenia klasy CodeBehind strony wzorcowej.

`DOCTYPE` i oznaczeniu deklaracyjnym strony wyświetlany pod `@Master` dyrektywy. Strona zawiera statyczny kod HTML oraz cztery formanty po stronie serwera:

- **Formularz sieci Web ( `<form runat="server">`)** — ponieważ wszystkie strony ASP.NET, zwykle zawiera formularz sieci Web — i ponieważ strona główna może zawierać kontrolki sieci Web, które musi znajdować się w obrębie formularza sieci Web — należy dodać formularz sieci Web do swojej strony wzorcowej (zamiast dodawać formularza sieci Web do e stacje strony zawartości).
- **Formant ContentPlaceHolder o nazwie `ContentPlaceHolder1`**  -tej kontrolki ContentPlaceHolder pojawia się w obrębie formularza sieci Web i służy jako region dla interfejsu użytkownika strony zawartość.
- **Po stronie serwera `<head>` elementu** — `<head>` element ma `runat="server"` atrybutu, dzięki czemu dostępne za pośrednictwem kodu po stronie serwera. `<head>` Element jest zaimplementowana w ten sposób, aby tytuł strony i inne `<head>`-powiązane znaczników, które mogą być dodawane lub programowo zmienić. Na przykład ustawienie na stronie ASP.NET `Title` zmiany właściwości `<title>` element renderowany przez `<head>` kontrolki serwera.
- **Formant ContentPlaceHolder o nazwie `head`**  -tej kontrolki ContentPlaceHolder pojawia się w obrębie `<head>` serwera kontroli i może służyć do deklaratywne dodawania zawartości do `<head>` elementu.

Ta domyślna strona wzorcowa oznaczeniu deklaracyjnym służy jako punkt wyjścia do projektowania stron wzorcowych. Możesz edytować kod HTML lub dodać dodatkowe kontrolki sieci Web lub kontrolek ContentPlaceHolder strony wzorcowej.

> [!NOTE]
> Podczas projektowania strony wzorcowej upewnij się, że strona główna zawiera formularz sieci Web i że co najmniej jedną kontrolkę ContentPlaceHolder pojawia się w obrębie tego formularza sieci Web.


### <a name="creating-a-simple-site-layout"></a>Tworzenie układu prostą witrynę

Możemy rozwinąć `Site.master`w oznaczeniu deklaracyjnym domyślnej do utworzenia układu witryny, którym udostępnianie wszystkich stron: typowego nagłówka; w lewej kolumnie nawigacji, wiadomości i innej zawartości w całej lokacji; i stopka, która jest wyświetlana ikona "Obsługiwane przez program Microsoft ASP.NET". Rysunek 6 przedstawia wynik końcowy strony głównej, gdy jeden z jej zawartość strony zostanie wyświetlony za pośrednictwem przeglądarki sieci. Trwa odwiedzoną stronę dotyczy czerwonym kółku regionu na rysunku 6 (`Default.aspx`); inne zawartość jest zdefiniowana na stronie głównej i w związku z tym spójny na wszystkich stronach zawartości.


[![Strona wzorcowa definiuje znaczniki dla góry po lewej stronie, a jej dolnej części](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Rysunek 06**: strona wzorcowa definiuje znaczniki dla góry po lewej stronie, a jej dolnej części ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Uzyskanie pokazano na rysunku 6 układu witryny, należy uruchomić, aktualizując `Site.master` strony wzorcowej, tak aby zawierała następujące oznaczeniu deklaracyjnym:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Układ strony wzorcowej jest definiowana za pomocą szeregu `<div>` elementów HTML. `topContent` `<div>` Zawiera kod znaczników, który pojawia się u góry każdej strony, podczas gdy `mainContent`, `leftContent`, i `footerContent` `<div>` s są używane do wyświetlania zawartości strony, lewej kolumnie i "obsługiwane przez firmy Microsoft Ikona programu ASP.NET", odpowiednio. Oprócz dodawania tych `<div>` elementy, można również zmienić nazwy `ID` właściwości kontrolki ContentPlaceHolder podstawowej z `ContentPlaceHolder1` do `MainContent`.

Formatowanie i układ zasady dla tych asortymentach `<div>` elementy są zapisane w [kaskadowych arkuszy stylów (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) pliku `Styles.css`, który jest określony za pośrednictwem `<link>` elementu w zestawie `<head>`elementu. Te różne reguły zdefiniować wygląd i działanie każdego `<div>` element przedstawionych powyżej. Na przykład `topContent` `<div>` element, który jest wyświetlany tekst "Samouczki stron wzorca" i łącza, ma swoją reguły formatowania, określone w `Styles.css` w następujący sposób:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Jeśli wykonujesz Twojego komputera, konieczne będzie pobierania w tym samouczku towarzyszący kodu i Dodaj `Styles.css` plik do projektu. Podobnie, należy również utworzyć folder o nazwie `Images` i skopiowania ikonę "Obsługiwane przez program Microsoft ASP.NET" pokaz pobranej witryny sieci Web do projektu.

> [!NOTE]
> Dyskusję na temat CSS i formatowanie strony sieci web wykracza poza zakres tego artykułu. Aby uzyskać więcej informacji na temat CSS, zapoznaj się z [samouczki CSS](http://www.w3schools.com/css/default.asp) na [W3Schools.com](http://www.w3schools.com/). Również zachęcam Cię do pobierania kodu, w tym samouczku towarzyszący i Poeksperymentuj z ustawieniami CSS `Styles.css` aby zobaczyć skutki różne reguły formatowania.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Tworzenie przy użyciu istniejącego szablonu projektu strony wzorcowej

W ciągu lat I dołączeniu wiele aplikacji sieci web ASP.NET w małych — do średnich firmach. Niektóre z moich klientów ma istniejącego układu witryny, które chcieli korzystać; inne zatrudniane projektanta grafiki właściwe. Kilka powierzone mi, aby zaprojektować układ witryny sieci Web. Jak można stwierdzić, rysunek 6, zadań programisty, aby zaprojektować układ witryny sieci Web jest zwykle jako jako posiadające swoje księgowy wykonać open-heart surgery, gdy Twoje lekarzem jest z podatków.

Na szczęście innumerous witryn sieci Web oferuje bezpłatne szablony projektu HTML — Google zwróciło więcej niż 6 milionów wyników dla wyszukiwanego terminu "templates bezpłatne witryny sieci Web". Jedna z moich ulubionych z nich jest [OpenDesigns.org](http://opendesigns.org/). Po odnalezieniu szablonu witryny sieci Web, którą chcesz dodać pliki CSS i obrazów do projektu witryny sieci Web i integrowanie szablon HTML na stronie głównej.

> [!NOTE]
> Firma Microsoft oferuje także szereg [bezpłatne szablony Start zestawu w programie ASP.NET projekt](https://msdn.microsoft.com/asp.net/aa336613.aspx) integrowane w oknie dialogowym nowej witryny sieci Web w programie Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Krok 2: Tworzenie skojarzonej zawartości strony

Ze stroną wzorcową utworzone jesteśmy gotowi rozpocząć tworzenie strony ASP.NET, które są powiązane z strony wzorcowej. Tego rodzaju strony są określane jako *stron zawartości*.

Umożliwia dodawanie nowej strony programu ASP.NET do projektu, który należy powiązać `Site.master` strony wzorcowej. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz opcję Dodaj nowy element. Wybierz szablon formularza sieci Web, wprowadź nazwę `About.aspx`, a następnie zaznacz pole wyboru "Wybierz stronę wzorcową", jak pokazano na rysunku 7. Ten sposób wyświetli wybierz stronę wzorcową okno dialogowe pola (zobacz rysunek 8), z którym można wybrać strony wzorcowej do użycia.

> [!NOTE]
> Jeśli utworzono witryny sieci Web ASP.NET przy użyciu modelu projektu aplikacji sieci Web zamiast modelu projektu witryny sieci Web nie zobaczą pola wyboru "Wybierz stronę wzorcową" w oknie dialogowym Dodaj nowy element pokazano na rysunku 7. Do tworzenia zawartości strony, gdy za pomocą projektu aplikacji sieci Web możesz modelować musisz wybrać szablon formularz zawartości sieci Web zamiast szablonu formularza sieci Web. Po wybierając szablon formularz zawartości sieci Web, a następnie klikając przycisk Dodaj, taka sama wybierz stronę wzorcową, zostanie wyświetlone okno dialogowe pokazano na rysunku 8.


[![Dodaj nową stronę zawartości](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Rysunek 07**: Dodawanie nowej strony zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Wybierz na stronie wzorcowej Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Rysunek 08**: Wybierz `Site.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Jak pokazano na poniższym oznaczeniu deklaracyjnym, Nowa strona zawartości zawiera `@Page` dyrektywy, że wskazuje z powrotem do jego główny strony i kontrolki zawartości dla wszystkich kontrolek ContentPlaceHolder strony wzorcowej.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> W sekcji "Tworzenie prostego układu witryny" w kroku 1 zmieniono `ContentPlaceHolder1` do `MainContent`. Nie w przypadku zmiany nazwy tej kontrolki ContentPlaceHolder `ID` tak samo jak oznaczeniu deklaracyjnym strony zawartości będą różnić się nieco od znaczników przedstawionych powyżej. To znaczy, drugi zawartości formantu `ContentPlaceHolderID` będzie odzwierciedlać `ID` odpowiedniego elementu ContentPlaceHolder formant na stronie głównej.


Podczas renderowania strony zawartości, aparat programu ASP.NET należy Łączenie strony zawartości kontrolki za pomocą kontrolek ContentPlaceHolder jego strony wzorcowej. Aparat platformy ASP.NET określa strony wzorcowej strony zawartości z `@Page` dyrektywy `MasterPageFile` atrybutu. Jak pokazano na powyższym znaczników, ta strona zawartości jest powiązany z `~/Site.master`.

Ponieważ dwóch kontrolek ContentPlaceHolder — strona główna `head` i `MainContent` — Visual Web Developer generowane dwa formanty zawartości. Każdy formant zawartości odwołuje się do określonego ContentPlaceHolder za pośrednictwem jego `ContentPlaceHolderID` właściwości.

Gdzie strony wzorcowe przykuć uwagę przez poprzednie techniki szablonu normalnej jest ich obsługi w czasie projektowania. Nr 9 przedstawiono `About.aspx` strony zawartości, po wyświetleniu za pośrednictwem widoku projektu programu Visual Web Developer's. Należy pamiętać, że w trakcie widoczna zawartość strony wzorcowej jest wyszarzona i nie może być modyfikowany. Formanty zawartości odpowiadający kontrolek ContentPlaceHolder strony wzorcowej są, jednak można edytować. I tak samo, jak za pomocą dowolnej innej strony ASP.NET, można utworzyć interfejsu strony zawartości, dodając kontrolki sieci Web za pośrednictwem widoków źródła lub projektu.


[![Widok projektu zawartości strony wyświetla zawartość strony dla strony i główny](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Rysunek 09**: zawartości strony projektu widoku wyświetla zarówno dla strony i zawartości strony główne ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Dodawanie znaczników i formantów sieci Web do zawartości strony

Poświęć chwilę, aby utworzyć odpowiednią zawartość do `About.aspx` strony. Jak widać na rysunku nr 10 wprowadzone "O autorze" Nagłówek i kilka akapitów tekstu, a także możesz dodać kontrolki sieci Web za. Po utworzeniu tego interfejsu, odwiedź stronę `About.aspx` strony za pośrednictwem przeglądarki.


[![Odwiedź stronę About.aspx za pośrednictwem przeglądarki](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Na rysunku nr 10**: odwiedź `About.aspx` strony za pomocą przeglądarki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


Jest ważne dowiedzieć się, że żądana strona zawartości strony głównej skojarzonej zespolone i są renderowane jako całość wyłącznie na serwerze sieci web. Przeglądarki przez użytkownika końcowego jest następnie wysyłana wynikowe, z kolei HTML. Aby to sprawdzić, Wyświetl HTML odebranych przez przeglądarki, przechodząc do menu widoku i Wybieranie źródła. Należy pamiętać, że istnieją Brak ramek lub innych technik wyspecjalizowane wyświetlania dwóch różnych stron sieci web w jednym oknie.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Wiązanie strony wzorcowej do istniejącej strony ASP.NET

Jak widzieliśmy w tym kroku dodawania nowej strony zawartości do aplikacji sieci web ASP.NET jest równie proste jak zaznaczając pole wyboru "Wybierz stronę wzorcową" i pobrania strony wzorcowej. Niestety Konwertowanie istniejącej strony ASP.NET na stronie wzorcowej są bardzo proste.

Można powiązać strony wzorcowej do istniejącej strony ASP.NET należy wykonać następujące czynności:

1. Dodaj `MasterPageFile` atrybutu do strony ASP.NET `@Page` dyrektywy, wskazując je do odpowiedniej strony wzorcowej.
2. Dodawanie formantów zawartości dla wszystkich kontrolek ContentPlaceHolder na stronie głównej.
3. Selektywnie Wytnij i Wklej zawartość istniejącej strony ASP.NET do odpowiednich kontroli w zawartości. Kliknę opcję "selektywnie" w tym miejscu, ponieważ ASP.NET stronie prawdopodobnie zawiera kod znaczników, który już jest wyrażona przez stronę wzorcową, na przykład `DOCTYPE`, `<html>` elementu, a formularz sieci Web.

Aby uzyskać instrukcje krok po kroku na temat tego procesu, wraz z zrzuty ekranu, zapoznaj się [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy [za pomocą stron wzorcowych i nawigacja w witrynie](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) samouczka. "Aktualizacja `Default.aspx` i `DataSample.aspx` do użycia na stronie wzorcowej" sekcji opisano szczegółowo następujące kroki.

Ponieważ jest to znacznie ułatwia tworzenie nowych stron zawartości, nie można przekonwertować istniejącej strony ASP.NET na stronach zawartości I zaleca się że zawsze podczas tworzenia nowej witryny sieci Web platformy ASP.NET dodać strony wzorcowej do lokacji. Powiąż wszystkie nowe strony programu ASP.NET do tej strony wzorcowej. Nie martw się, jeśli początkowej strony wzorcowej jest bardzo proste lub zwykły; można później zaktualizować strony wzorcowej.

> [!NOTE]
> Podczas tworzenia nowej aplikacji platformy ASP.NET, Visual Web Developer dodaje `Default.aspx` strona, która nie jest związany ze stroną wzorcową. Chcąc rozwiązanie polegające na konwersji istniejącej strony ASP.NET do strony zawartości, przejdź dalej i zrobić z `Default.aspx`. Alternatywnie możesz usunąć `Default.aspx` , a następnie ponownie dodać, ale tym razem, zaznaczając pole wyboru "Wybierz stronę wzorcową".


## <a name="step-3-updating-the-master-pages-markup"></a>Krok 3: Aktualizowanie znaczników strony wzorcowej

Jest jedną z podstawowych zalet strony wzorcowe, że jednej strony wzorcowej mogą służyć do definiowania układu ogólną na wielu stronach w witrynie. W związku z tym aktualizowanie witryny wygląd i działanie wymaga aktualizacji pojedynczy plik — strona główna.

Aby zilustrować ten problem, zaktualizuj nasze strony wzorcowej do uwzględnienia bieżącą datę w górnej części kolumny po lewej stronie. Dodaj etykietę o nazwie `DateDisplay` do `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Następnie należy utworzyć `Page_Load` program obsługi zdarzeń dla wzorca strony, a następnie dodaj następujący kod:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Powyższy kod ustawia etykiety `Text` właściwość bieżącą datę i godzinę w formacie dzień tygodnia, nazwy miesiąca i dnia dwóch cyfr (zobacz rysunek 11). Dzięki tej zmianie ponownie jednej strony z zawartością. Jak pokazano na ilustracji 11, wynikowy znaczników jest natychmiast zaktualizowano zmiany strony wzorcowej.


[![Zmiany strony wzorcowej są widoczne podczas przeglądania zawartości strony](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Rysunek 11**: zmiany na stronie wzorcowej są widoczne podczas przeglądania strony zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Tak jak pokazano w tym przykładzie, stron wzorcowych może zawierać kontrolki sieci Web po stronie serwera, kod i procedury obsługi zdarzeń.


## <a name="summary"></a>Podsumowanie

Strony wzorcowe umożliwiają deweloperom ASP.NET do projektowania spójnego układu w całej lokacji, który można łatwo aktualizować. Tworzenie stron wzorcowych i skojarzonych z nimi stron zawartości jest tak proste, jak tworzenie standardowego stron ASP.NET, zgodnie z Visual Web Developer oferuje rozbudowane obsługi w czasie projektowania.

Przykład strony wzorcowej, utworzonego w ramach tego samouczka było dwóch kontrolek ContentPlaceHolder head i MainContent. Określony tylko kodu znaczników kontrolki MainContent ContentPlaceHolder w naszej strony zawartości, jednak. W następnym samouczku przyjrzymy się przy użyciu wielu zawartości kontrolki na stronie zawartości. Widzimy również, jak zdefiniować domyślny kod znaczników dla zawartości kontrolki w obrębie strony wzorcowej oraz jak przełączać się między przy użyciu domyślnego znaczników zdefiniowanych w bazie danych master strony i zapewnianie niestandardowych znaczników ze strony zawartość.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [ASP.NET dla projektantów: bezpłatne szablonów projektu i wskazówek dotyczących tworzenia witryn sieci Web platformy ASP.NET przy użyciu standardów sieci Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Omówienie stron wzorcową platformy ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Samouczki usługi kaskadowych arkuszy stylów (CSS)](http://www.w3schools.com/css/default.asp)
- [Dynamiczne Ustawianie tytułu strony](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Strony wzorcowe na platformie ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Strony wzorcowe samouczki Szybki Start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](nested-master-pages-cs.md)
> [dalej](multiple-contentplaceholders-and-default-content-vb.md)
