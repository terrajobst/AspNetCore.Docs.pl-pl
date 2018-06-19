---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Do utworzenia układu całej lokacji za pomocą stron wzorcowych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek przedstawia podstawy strony wzorcowej. To znaczy, jakie są strony wzorcowe, jak jeden tworzenia strony wzorcowej, jakie są posiadacze miejscu zawartości, jak jeden cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: d18993af7159de552db0c622fbef58e814e36ebb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890882"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Do utworzenia układu całej lokacji za pomocą stron wzorcowych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Ten samouczek przedstawia podstawy strony wzorcowej. Czyli, co to są strony wzorcowe, jak jedną tworzy strony wzorcowej, jakie są posiadacze miejscu zawartości, jak jeden tworzy strony platformy ASP.NET, który używa strony wzorcowej sposobu modyfikowania strony wzorcowej automatycznie znajduje odzwierciedlenie w jego skojarzony strony z zawartością i tak dalej.


## <a name="introduction"></a>Wprowadzenie

Jeden atrybut dobrze zaprojektowanego witryny sieci Web jest układ spójne strony całej lokacji. Przyjmować www.asp.net witryny sieci Web, na przykład. W momencie pisania tego dokumentu co strona ma tę samą zawartość u góry i u dołu strony. Jak pokazano na rysunku 1, bardzo początku każdej strony wyświetla szarym pasku listę Communities firmy Microsoft. Poniżej oznacza to logo lokacji, listę języków, w których został przetłumaczony lokacji i w sekcjach rdzeni: Home, wprowadzenie, Dowiedz się więcej, pliki do pobrania i tak dalej. Podobnie w dolnej części strony zawiera informacje na temat reklamy na www.asp.net, informację o prawach autorskich oraz link do zachowania poufności informacji.


[![Witryny sieci Web www.asp.net wykorzystuje spójny wygląd i zachowanie na wszystkich stronach](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Rysunek 01</strong>: www.asp.net witryny sieci Web jest stosowana w spójny wygląd i dostępny we wszystkich stron ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Inny atrybut dobrze zaprojektowanego witryny jest łatwość, z którym można zmienić wygląd witryny. Rysunek 1 pokazuje strony głównej www.asp.net z marca 2008, ale od chwili opublikowania tego samouczka zostały zmienione wyglądu i działania. Być może elementów menu u góry zostanie rozwinięty w celu pomieszczenia nową sekcję dla platformy MVC. Lub może znacząco nowy projekt z różne kolory, czcionki i układu będą nieobsługujące. Zastosowanie tych zmian do całej witryny powinna być szybki i prosty proces, który nie wymaga modyfikowania tysiące stron sieci web, które tworzą witryny.

Tworzenie szablonu strony całej lokacji w programie ASP.NET jest możliwe za pośrednictwem *strony wzorcowe*. Mówiąc, strony głównej jest specjalnym rodzajem strony ASP.NET, który definiuje znaczniki, które są wspólne dla wszystkich *zawartości strony* oraz obszarów, które można dostosować na podstawie zawartości strony zawartości strony. (Strony zawartości jest powiązany z strony wzorcowej strony ASP.NET). Po każdej zmianie układ strony wzorcowej lub formatowania wszystkie dane wyjściowe jej strony zawartości jest również od razu aktualizowany, co czyni stosowanie zmian wygląd całej lokacji, wystarczy aktualizacji i wdrażanie pojedynczego pliku (to znaczy, strony wzorcowej).

To jest pierwszy samouczek z serii samouczków, z którymi eksplorowania przy użyciu stron wzorcowych. W trakcie tego samouczka serii firma Microsoft:

- Sprawdź tworzenie stron wzorcowych i skojarzonych stron zawartości
- Omówiono szereg porady, wskazówki i pułapek,
- Identyfikowanie typowych problemów strony wzorcowej i Poznaj rozwiązania,
- Zapoznanie się z strony zawartości, a także na na odwrót, dostęp do strony wzorcowej
- Dowiedz się, jak określić strony zawartości strony wzorcowej w czasie wykonywania i
- Inne zaawansowane tematy strony wzorcowej.

Te samouczki są dostosowane do zwięzły i zawierają instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie prowadzą użytkownika przez proces. Każdego samouczka jest dostępna w C# i Visual Basic wersji i obejmuje pobieranie pełny kod używany.

W tym samouczku inauguracyjnym rozpoczyna się od przyjrzeć się podstawy strony wzorcowej. Firma Microsoft omówiono sposób pracy strony wzorcowe, przyjrzeć się tworzenie strony wzorcowej i skojarzonych stron zawartości przy użyciu programu Visual Web Developer i zobacz, jak zmiany strony głównej są natychmiast odzwierciedlone w jej zawartości strony. Dzieła!

## <a name="understanding-how-master-pages-work"></a>Opis sposobu pracy stron wzorcowych

Tworzenia witryny sieci Web z układem spójne strony całej lokacji wymaga czy każdej strony sieci web Emituj typowych znaczników formatowania oprócz jego niestandardowej zawartości. Na przykład podczas każdego samouczek lub forum ogłoszenia na www.asp.net mieć własną unikatową zawartość, każdy z tych stron również renderowania serii wspólnych `<div>` elementy wyświetlające łącza sekcji najwyższego poziomu: Home, wprowadzenie, Dowiedz się więcej i tak dalej.

Istnieją różne metody tworzenia stron sieci web z spójny wygląd i zachowanie. Prostym rozwiązaniem jest po prostu skopiuj i Wklej typowych znaczników układu do wszystkich stron sieci web, ale takie podejście charakteryzuje się liczba downsides. Po pierwsze za każdym razem, gdy jest tworzona nowa strona, należy pamiętać skopiować i wkleić do strony zawartości udostępnionej. Takie kopiowanie i wklejanie działań są dojrzałe błędu, ponieważ mogą przypadkowo skopiować tylko podzestaw znacznika udostępnionego do nowej strony. I do góry go, ta metoda powoduje, że zastępując istniejący wygląd całej witryny nową słabe rzeczywistych, ponieważ należy edytować aby można było używać nowego wyglądu i działania każdej pojedynczej strony w witrynie.

Przed platformę ASP.NET w wersji 2.0, strony programistów często umieszcza markup wspólnego w [kontrolek użytkownika](https://msdn.microsoft.com/library/y6wb1a0e.aspx) , a następnie dodać tych kontrolek użytkownika do każdej strony. Takie podejście wymagane, pamiętaj, aby ręcznie dodaj formanty użytkownika do każdej strony projektanta strony, ale można łatwiej modyfikacji całej lokacji, ponieważ podczas aktualizowania markup wspólnego tylko formanty użytkownika wymagane do zmodyfikowania. Niestety program Visual Studio .NET 2002 i 2003 — wersje programu Visual Studio będzie używany do tworzenia aplikacji 1.x ASP.NET — jest renderowana kontrolek użytkownika w widoku Projekt szare pola. W związku z tym deweloperzy strony przy użyciu tej metody nie posiada WYSIWYG środowiska czasu projektowania.

Niedociągnięć za pomocą formantów użytkownika zostały rozwiązane w ASP.NET w wersji 2.0 i programu Visual Studio 2005 wraz z wprowadzeniem *strony wzorcowe*. Strona wzorcowa jest specjalnym rodzajem strony ASP.NET, który definiuje znaczników całej lokacji oraz *regionów* w przypadku, gdy skojarzone *zawartości strony* zdefiniować ich niestandardowych znaczników. Zostanie wyświetlone w kroku 1, tych regionów są określone przez element ContentPlaceHolder formanty. Formant ContentPlaceHolder oznacza po prostu pozycji w hierarchii formantu strony wzorcowej, gdzie niestandardowej zawartości mogą zostać dodane przez strony zawartości.

> [!NOTE]
> Podstawowych pojęć i funkcji strony wzorcowe, nie zmienił się od platformę ASP.NET w wersji 2.0. Jednak program Visual Studio 2008 oferuje czasu projektowania obsługę zagnieżdżone strony wzorcowe, funkcja, która brakowało w programie Visual Studio 2005. Firma Microsoft będzie wyglądać na przy użyciu zagnieżdżone strony wzorcowe w przyszłości samouczka.


Na rysunku 2 przedstawiono, jak może wyglądać strony wzorcowej dla www.asp.net. Należy pamiętać, że strony wzorcowej definiuje wspólne układ całej lokacji — kod znaczników u góry, dolnej i prawej każdej strony - oraz ContentPlaceHolder w środku lewej, gdzie znajduje się zawartość unikatowy dla poszczególnych stron sieci web.


![Strona wzorcowa definiuje układ całej lokacji i regiony można edytować na podstawie zawartości strony zawartości strony](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Rysunek 02**: strona wzorcowa definiuje układ całej lokacji i regiony można edytować na podstawie zawartości strony zawartości strony


Po zdefiniowaniu strony wzorcowej może być powiązana do nowych stron ASP.NET przy użyciu znaczników wyboru. Te strony ASP.NET - o nazwie strony z zawartością — obejmują formantu zawartości dla każdej kontrolki elementu ContentPlaceHolder strony wzorcowej. W przypadku odwiedzenia strony zawartości za pośrednictwem przeglądarki aparatu ASP.NET tworzy hierarchia formantów strony wzorcowej i injects hierarchia formantów zawartości strony w odpowiednich miejscach. Ta hierarchia połączonych kontroli jest renderowany i wynikowy HTML jest zwracany do przeglądarki użytkownika końcowego. W rezultacie strony zawartości emituje zarówno znaczników wspólnej zdefiniowane w jego strony wzorcowej poza kontrolki elementu ContentPlaceHolder i znaczników specyficznym dla strony zdefiniowany w ramach własnej formantów zawartości. Rysunek 3 ilustruje tę koncepcję.


[![Znaczniki strony żądanie jest zespolone do strony wzorcowej](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Rysunek 03**: żądana w znaczniku strony jest zespolone do strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Teraz, gdy Omówiliśmy jak strony wzorcowe pracy Spójrzmy na tworzenie strony wzorcowej i skojarzonych stron zawartości przy użyciu programu Visual Web Developer.

> [!NOTE]
> Aby można było uzyskać dostęp do najszerszych możliwych odbiorców, witryny sieci Web ASP.NET mamy utworzyć w tej serii samouczka zostanie utworzona z użyciem ASP.NET 3.5 z bezpłatnej wersji Visual Studio 2008 firmy Microsoft [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Jeśli nie zostały jeszcze uaktualnione do programu ASP.NET 3.5, nie martw się — omówione w pracy te samouczki jednakowo oraz z programem ASP.NET w wersji 2.0 i programu Visual Studio 2005. Jednak niektóre aplikacje demo mogą używać nowych funkcji programu .NET Framework w wersji 3.5; w przypadku używania funkcji specyficznych dla 3.5 dołączyć należy pamiętać, że w tym artykule omówiono sposób implementacji podobne funkcje w wersji 2.0. Należy pamiętać, dostępna dla aplikacji demonstracyjnej pobierane z każdym obiekcie docelowym samouczek .NET Framework w wersji 3.5, co powoduje `Web.config` pliku, który zawiera elementy konfiguracyjne 3.5. Długie wątku, short, jeśli masz jeszcze do zainstalowania .NET 3.5 na komputerze następnie aplikacji sieci web do pobrania nie będzie działać bez usuwania pierwszego znacznika specyficzne dla 3.5 z `Web.config`. Zobacz [pincety ASP.NET w wersji 3.5 w `Web.config` pliku](http://www.4guysfromrolla.com/articles/121207-1.aspx) Aby uzyskać więcej informacji na ten temat.


## <a name="step-1-creating-a-master-page"></a>Krok 1: Tworzenie strony wzorcowej

Zanim firma Microsoft może zapoznać się z tworzenia i używania strony wzorcowej i zawartości, należy najpierw witryny sieci Web platformy ASP.NET. Rozpocznij od utworzenia nowego pliku oparte na systemie ASP.NET witryny sieci Web. Aby to zrobić, uruchom Visual Web Developer a następnie przejdź do menu Plik i wybierz nową witrynę sieci Web, wyświetlanie okna dialogowego nowej witryny sieci Web (zobacz rysunek 4). Wybierz szablon witryny sieci Web ASP.NET, wartość na liście rozwijanej lokalizacji do systemu plików, wybierz folder, który można umieścić witrynę sieci web i ustawić język Visual Basic. Spowoduje to utworzenie nowej witryny sieci web z `Default.aspx` strony ASP.NET `App_Data` folderu, a `Web.config` pliku.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektem: witryny sieci Web i projektów aplikacji sieci Web. Projektów witryny sieci Web brakuje pliku projektu, podczas gdy projekty aplikacji sieci Web naśladować architektura projektu w Visual Studio .NET 2002/2003 — dołączenie pliku projektu i kompilowanie kodu źródłowego projektu w jednym zestawie, który jest umieszczony w `/bin` folder. Visual Studio 2005 projektów początkowo tylko obsługiwane witryny sieci Web, mimo że projektu aplikacji sieci Web modelu została przywrócona z dodatkiem Service Pack 1; Program Visual Studio 2008 oferuje oba modele projektu. Visual 2005 Developer sieci Web i wersje 2008, jednak obsługują tylko projektów witryny sieci Web. Model projekt witryny sieci Web można używać do mojego pokazy w tym samouczku. Jeśli używasz wersji-Express i chcesz użyć [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) zamiast tego możesz to zrobić, ale należy pamiętać, że może zostać pewne rozbieżności między informacje wyświetlane na ekranie i kroki należy wykonać w porównaniu z zrzuty ekranu pokazano i instrukcje podane w tych samouczkach.


[![Tworzenie nowego pliku oparte na systemie witryny sieci Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Rysunek 04**: tworzenie witryny sieci Web New File System-Based ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Następnie dodaj stronę wzorcową do lokacji w katalogu głównym przez kliknięcie prawym przyciskiem myszy nazwę projektu, wybierając pozycję Dodaj nowy element i wybór szablonu strony wzorcowej. Należy pamiętać, że strony wzorcowe kończy się rozszerzeniem `.master`. Nazwa tej nowej strony wzorcowej `Site.master` i kliknij przycisk Dodaj.


[![Dodaj stronę wzorcową o nazwie Site.master witryny sieci Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Rysunek 05**: Dodaj nazwane strony wzorca `Site.master` witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Dodawanie nowego pliku strony głównej przy użyciu programu Visual Web Developer tworzy stronę wzorcową z następujących deklaratywne znaczników:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

Pierwszy wiersz w znaczniku deklaratywne [ `@Master` dyrektywy](https://msdn.microsoft.com/library/ms228176.aspx). `@Master` Dyrektywa jest podobny do [ `@Page` dyrektywy](https://msdn.microsoft.com/library/ydy4x04a.aspx) wyświetlonym w stron ASP.NET. Definiuje język po stronie serwera (VB) i informacje o lokalizacji i dziedziczenia klasy związane z kodem strony wzorcowej.

`DOCTYPE` a declarative znaczników strony zostanie wyświetlone poniżej `@Master` dyrektywy. Strona zawiera statyczny HTML oraz cztery kontroli po stronie serwera:

- **Formularz sieci Web ( `<form runat="server">`)** — ponieważ zwykle mają formularza sieci Web - wszystkich stron ASP.NET i dodanie do strony wzorcowej (zamiast dodawanie formularzy sieci Web do e formularza sieci Web upewnij się, ponieważ strony wzorcowej może zawierać formantów sieci Web, które muszą znajdować się w formularzu sieci Web — stacje strony zawartości).
- **Formant ContentPlaceHolder o nazwie `ContentPlaceHolder1`**  — ten formant ContentPlaceHolder pojawia się w formularzu sieci Web i służy jako regionu dla interfejsu użytkownika strony zawartość.
- **Strona serwera `<head>` elementu** - `<head>` element ma `runat="server"` atrybut, dzięki czemu dostępne przez kod po stronie serwera. `<head>` Element jest zaimplementowana w ten sposób, aby tytuł strony i inne `<head>`-powiązane znaczników mogą być dodane lub dostosowana programowo. Na przykład ustawienia strony ASP.NET `Title` zmiany właściwości `<title>` element renderowany przez `<head>` kontrolki serwera.
- **Formant ContentPlaceHolder o nazwie `head`**  -tego formantu elementu ContentPlaceHolder pojawia się w obrębie `<head>` serwera kontroli i może służyć do deklaratywnie Dodawanie zawartości do `<head>` elementu.

Znaczników deklaratywne strony wzorcowej domyślna służy jako punkt początkowy projektowania stron wzorcowych. Możesz także, aby edytować HTML lub aby dodać dodatkowe formantów sieci Web lub Elementy ContentPlaceHolders strony wzorcowej.

> [!NOTE]
> Podczas projektowania strony wzorcowej upewnij się, że strony wzorcowej zawiera formularza sieci Web i że co najmniej jeden formant ContentPlaceHolder pojawia się w obrębie tego formularza sieci Web.


### <a name="creating-a-simple-site-layout"></a>Tworzenie układu witryny

Umożliwia rozwijanie `Site.master`w domyślnej deklaratywne znaczników do utworzenia układu witryny, których mają wszystkich stron: typowe nagłówka; kolumnę po lewej stronie z nawigacji, wiadomości i innej zawartości w całej lokacji; i stopce strony, która wyświetla ikonę "Obsługiwane przez program Microsoft ASP.NET". Rysunek 6 przedstawia wynik końcowy strony wzorcowej, gdy jeden z jej strony zawartości jest wyświetlany za pośrednictwem przeglądarki. Trwa odwiedzoną stronę dotyczy czerwonym kółku region na rysunku 6 (`Default.aspx`); innej zawartości jest zdefiniowane na stronie głównej i w związku z tym spójna na wszystkich stronach zawartości.


[![Strona wzorcowa definiuje znaczniki dla Top, w lewo i w dolnej części](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Rysunek 06**: strona wzorcowa definiuje znaczniki dla Top, w lewo i w dolnej części ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Uzyskanie układu witryny przedstawiono na rysunku 6, Rozpocznij od aktualizowanie `Site.master` strony wzorcowej tak, aby zawierał następujące deklaratywne kod znaczników:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Układ strony wzorcowej jest definiowana za pomocą serii `<div>` elementów HTML. `topContent` `<div>` Zawiera kod znaczników, który pojawi się na początku każdej stronie, gdy `mainContent`, `leftContent`, i `footerContent` `<div>` s są używane do wyświetlania zawartości strony, kolumnie po lewej stronie i "obsługiwane przez firmy Microsoft Ikona programu ASP.NET"odpowiednio. Oprócz dodania tych `<div>` elementów, również zmieniono `ID` właściwości formantu podstawowego elementu ContentPlaceHolder od `ContentPlaceHolder1` do `MainContent`.

Formatowanie i układ zasady te asortymentach `<div>` elementów jest zapisane w [kaskadowych arkuszy stylów (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) pliku `Styles.css`, który jest określony za pomocą `<link>` elementu na stronie głównej firmy `<head>`elementu. Te reguły różnych definiują wyglądu i działania każdej `<div>` elementów wymienionych powyżej. Na przykład `topContent` `<div>` element, który zawiera tekst "Główny stron samouczki" i łącza, ma jego formatowania reguły określone w `Styles.css` w następujący sposób:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Jeśli wykonujesz na komputerze, należy pobrać kod towarzyszący tego samouczka i dodać `Styles.css` plik do projektu. Podobnie, należy również utworzyć folder o nazwie `Images` i skopiować ikonę "Obsługiwane przez program Microsoft ASP.NET" z pobranego pokaz witryny sieci Web do projektu.

> [!NOTE]
> Omówienie CSS i formatowanie strony sieci web wykracza poza zakres tego artykułu. Aby uzyskać więcej informacji na temat CSS, zapoznaj się z [samouczki CSS](http://www.w3schools.com/css/default.asp) w [W3Schools.com](http://www.w3schools.com/). Można również zachęca do pobierania w tym samouczku towarzyszący kodu i odtworzenie z ustawieniami CSS w `Styles.css` aby zobaczyć efekty różnych reguły formatowania.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Tworzenie przy użyciu istniejącego szablonu projektu strony wzorcowej

Całościowo I został skompilowany wiele aplikacji sieci web platformy ASP.NET dla małych — do średnich firm. Niektórzy klienci Moje ma istniejącego układu witryny, których chcieli korzystać; inne dzierżawione projektanta grafiki właściwe. Kilka odpowiedzialna mnie do zaprojektowania układu witryny sieci Web. Jak widać na rysunku 6 zadań programisty do zaprojektowania układu witryny sieci Web jest zwykle jako jako mający Twojej księgowość wykonać open-heart surgery, gdy Twoje lekarza jest Twoje podatki.

Na szczęście innumerous witryn sieci Web oferuje bezpłatne szablony projektów HTML — Google zwróciło więcej niż sześciu milionów wyników dla wyszukiwanego terminu "Szablony bezpłatne witryny sieci Web". Jednym z moich ulubionych z nich jest [OpenDesigns.org](http://opendesigns.org/). Po znalezieniu szablonu witryny sieci Web, którą chcesz dodać pliki CSS i obrazy do projektu witryny sieci Web i integracji w szablonie HTML na stronie głównej.

> [!NOTE]
> Firma Microsoft oferuje także szereg [wolne ASP.NET uruchomić zestaw szablony](https://msdn.microsoft.com/asp.net/aa336613.aspx) który zintegrować okno dialogowe nowej witryny sieci Web w programie Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Krok 2: Tworzenie skojarzone strony z zawartością

Ze stroną wzorcową utworzone możemy gotowe do rozpoczęcia tworzenia stron ASP.NET, które są powiązane z strony wzorcowej. Takie strony są określane jako *zawartości strony*.

Umożliwia dodawanie nowej strony ASP.NET do projektu, który należy powiązać `Site.master` strony wzorcowej. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz opcję Dodaj nowy element. Wybierz szablon formularza sieci Web, wprowadź nazwę `About.aspx`, a następnie zaznacz pole wyboru "Wybierz strony wzorcowej", jak pokazano na rysunku 7. W ten sposób spowoduje wyświetlenie wybierz okno dialogowe strony wzorcowej polu (patrz rysunek 8), z którym można wybrać strony wzorcowej do użycia.

> [!NOTE]
> Jeśli utworzono witryny sieci Web ASP.NET przy użyciu modelu projektu aplikacji sieci Web zamiast modelu projekt witryny sieci Web nie będą widzieć pole wyboru "Wybierz strony wzorcowej" w oknie dialogowym Dodawanie nowego elementu pokazano na rysunku 7. Do utworzenia zawartości strony, gdy model za pomocą projektu aplikacji sieci Web, możesz wybrać szablon formularza zawartości sieci Web zamiast szablonu formularza sieci Web. Po wybór szablonu formularza zawartości sieci Web, a następnie klikając przycisk Dodaj, wybierz taki sam strony wzorcowej, zostanie wyświetlone okno dialogowe pokazano na rysunku 8.


[![Dodaj nową stronę zawartości](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Rysunek 07**: Dodaj nową stronę zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Wybierz strony wzorcowej Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Rysunek 08**: Wybierz `Site.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Ilustruje następujący kod deklaratywne nowej strony zawartości zawiera `@Page` dyrektywy czy wskazuje z powrotem do wzorca strony i kontrolki zawartości dla każdej kontrolki elementu ContentPlaceHolder strony wzorcowej.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> W sekcji "Tworzenie prostego układu witryny" w kroku 1 zmieniono `ContentPlaceHolder1` do `MainContent`. Nie w przypadku zmiany nazwy tego formantu elementu ContentPlaceHolder `ID` w taki sam sposób, deklaratywne znaczników strony zawartości będzie różnić się nieznacznie od znacznika przedstawionych powyżej. To znaczy, drugi zawartości formantu `ContentPlaceHolderID` będzie odzwierciedlać `ID` odpowiedniego elementu ContentPlaceHolder formant na stronie głównej.


Podczas renderowania strony zawartości, aparatu ASP.NET musi Łączenie strony zawartości kontrolki z formantami ContentPlaceHolder jego strony wzorcowej. Aparat programu ASP.NET określa strony zawartości strony wzorcowej z `@Page` dyrektywy `MasterPageFile` atrybutu. Jak pokazano na powyższym znaczników, ta strona zawartości jest powiązany z `~/Site.master`.

Ponieważ strony wzorcowej ma dwa formanty ContentPlaceHolder - `head` i `MainContent` -Visual Web Developer wygenerowany dwóch formantów zawartości. Każdy formant zawartości odwołuje się do określonego elementu ContentPlaceHolder za pośrednictwem jego `ContentPlaceHolderID` właściwości.

Gdzie stron wzorcowych zaprezentować przez poprzednie techniki szablonu całej lokacji jest ich obsługi w czasie projektowania. Na rysunku nr 9 przedstawiono `About.aspx` strony zawartości, gdy wyświetlany w widoku projektu programu Visual Web Developer's. Należy pamiętać, że zawartość strony wzorcowej jest widoczny, jest niedostępny i nie może być modyfikowany. Formanty zawartości odpowiadający Elementy ContentPlaceHolders strony wzorcowej są, jednak można edytować. I tak samo jak z innej strony ASP.NET, można utworzyć interfejsu strony zawartości przez dodawanie formantów sieci Web za pośrednictwem widoków źródła lub projektu.


[![Widok projektu strony zawartości zawiera zawartość strony głównej i strony specyficzne](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Rysunek 09**: zawartości strony projekt widoku wyświetla zarówno konkretnych stron i zawartość strony wzorca ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Dodawanie znaczników i formantów sieci Web do zawartości strony

Poświęć chwilę, aby utworzyć zawartość dla `About.aspx` strony. Jak widać na rysunku nr 10 wprowadzono nagłówek "O autorze" i kilka akapitów tekstu, a działanie swobodę zbyt Dodaj formanty sieci Web. Po utworzeniu tego interfejsu, odwiedź stronę `About.aspx` strony za pośrednictwem przeglądarki.


[![Odwiedź stronę About.aspx przy użyciu przeglądarki](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Na rysunku nr 10**: odwiedź `About.aspx` strony za pomocą przeglądarki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


Należy zrozumieć, że żądanej strony zawartości strony wzorcowej skojarzonej zespolone i są renderowane jako całość wyłącznie na serwerze sieci web. Przeglądarki przez użytkownika końcowego jest następnie wysyłana wynikowy, kolei HTML. Aby to sprawdzić, Wyświetl odebranych przez przeglądarkę, przechodząc do menu Widok i Wybieranie źródła HTML. Należy pamiętać o tym, czy ma żadnych ramek lub innych technik specjalne wyświetlania dwóch różnych stron sieci web w jednym oknie.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Powiązanie strony wzorcowej do istniejącej strony ASP.NET

Jak widzieliśmy w tym kroku dodawania nowej strony zawartości do aplikacji sieci web ASP.NET jest tak proste, jak zaznaczając pole wyboru "Wybierz strony wzorcowej" i pobrania strony wzorcowej. Niestety Konwertowanie istniejącej strony ASP.NET do strony głównej nie jest równie proste.

Powiązać strony wzorcowej do istniejącej strony ASP.NET należy wykonać następujące czynności:

1. Dodaj `MasterPageFile` atrybutu do strony ASP.NET `@Page` dyrektywy, wskazując go do odpowiedniej strony wzorcowej.
2. Dodaj formanty zawartości dla każdego Elementy ContentPlaceHolders na stronie głównej.
3. Selektywnie wyciąć i wkleić strony ASP.NET istniejącej zawartości do odpowiednich formantów zawartości. Mogę powiedzieć "selektywne" w tym miejscu, ponieważ ASP.NET prawdopodobnie strona zawiera kod znaczników, który jest już wyrażona strony wzorcowej, takich jak `DOCTYPE`, `<html>` elementu i formularza sieci Web.

Aby uzyskać instrukcje dotyczące tego procesu oraz zrzuty ekranu, zapoznaj się z [Scott Guthrie](https://weblogs.asp.net/scottgu/)w [przy użyciu stron wzorcowych i nawigacji w witrynie](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) samouczka. "Aktualizacja `Default.aspx` i `DataSample.aspx` do używania strony wzorcowej" szczegółowo opisano następujące kroki.

Ponieważ to znacznie ułatwia tworzenie nowych stron zawartości, niż można przekonwertować istniejących stron ASP.NET do strony z zawartością, najlepiej czy podczas tworzenia nowej witryny sieci Web ASP.NET dodawania strony wzorcowej do tej lokacji. Wszystkie nowe strony ASP.NET należy powiązać tej strony wzorcowej. Nie martw się, jeśli początkowej strony wzorcowej jest bardzo proste lub niezaszyfrowanym; Później możesz zaktualizować strony wzorcowej.

> [!NOTE]
> Podczas tworzenia nowej aplikacji ASP.NET, Visual Web Developer dodaje `Default.aspx` strony, który nie jest powiązany z strony wzorcowej. Jeśli chcesz praktyki konwersji istniejącej strony ASP.NET na stronie zawartości Przejdź dalej i zrobić z `Default.aspx`. Alternatywnie możesz usunąć `Default.aspx` , a następnie ponownie dodaj go, lecz tym razem, zaznaczając pole wyboru "Wybierz strony głównej".


## <a name="step-3-updating-the-master-pages-markup"></a>Krok 3: Aktualizowanie znaczników strony wzorcowej

Jest jedną z zalet głównej strony wzorcowe, że jednej strony wzorcowej może służyć do zdefiniowania ogólny układ wiele stron w witrynie. W związku z tym aktualizowania witryny wygląd i działanie wymaga aktualizacji pojedynczego pliku - strony wzorcowej.

Aby zilustrować ten problem, zaktualizuj umożliwia naszej strony wzorcowej uwzględnienie bieżącą datę w u góry po lewej stronie kolumny. Dodaj etykietę o nazwie `DateDisplay` do `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Następnie należy utworzyć `Page_Load` programu obsługi zdarzeń dla wzorca strony i Dodaj następujący kod:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Powyższy kod ustawia etykietę `Text` właściwości bieżącą datę i godzinę w formacie dzień tygodnia, nazwy miesiąca i dnia dwucyfrowe (patrz rysunek 11). Dzięki tej zmianie ponowne przeanalizowanie zawartości stron. Jak pokazano na rysunku nr 11, wynikowy kod znaczników jest natychmiast zaktualizowano w celu uwzględnienia zmiany strony wzorcowej.


[![Zmiany w strony wzorcowej są uwzględniane podczas wyświetlania zawartości strony](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Rysunek 11**: zmiany strony wzorcowej są uwzględniane podczas wyświetlania zawartości strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Jak pokazano w poniższym przykładzie, stron wzorcowych mogą zawierać formantów sieci Web po stronie serwera, kodu i procedury obsługi zdarzeń.


## <a name="summary"></a>Podsumowanie

Strony główne umożliwiają deweloperom ASP.NET projektowania spójnego układu całej lokacji, który można łatwo aktualizować. Tworzenie stron wzorcowych i skojarzonych z nimi stron zawartości jest tak proste, jak tworzenie standardowych stron ASP.NET, jako Visual Web Developer oferuje rozbudowane obsługi w czasie projektowania.

Przykład strony wzorcowej, utworzone w tym samouczku ma dwa formanty ContentPlaceHolder head i znacznika. Określony tylko znacznika kontrolki elementu ContentPlaceHolder znacznika w naszej strony zawartości, jednak. W następnym samouczku przyjrzymy się przy użyciu wielu zawartość kontrolki na stronie zawartości. Przedstawiono również sposób definiowania domyślny kod znaczników dla zawartości kontrolki na stronie głównej także jak przełączać się między przy użyciu domyślnego znacznika zdefiniowanych we wzorcu strony i dostarczanie niestandardowych znaczników ze strony zawartość.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [ASP.NET dla projektantów: Zwolnij szablonów projektu i wskazówki na tworzeniu witryn sieci Web ASP.NET przy użyciu standardów sieci Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Omówienie stron ASP.NET wzorca](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Samouczki usługi kaskadowych arkuszy stylów (CSS)](http://www.w3schools.com/css/default.asp)
- [Dynamicznie ustawienie tytuł strony](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Stron wzorcowych w programie ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Strony wzorcowe samouczków szybkiego startu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](nested-master-pages-cs.md)
> [dalej](multiple-contentplaceholders-and-default-content-vb.md)
