---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Zagnieżdżone strony wzorcowe (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Pokazuje, jak można zagnieżdżać jedną stronę wzorcową w innym.
ms.author: aspnetcontent
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e58eef87afce7d1b7e29445bdbf48baf5f65d3e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836265"
---
<a name="nested-master-pages-vb"></a>Zagnieżdżone strony wzorcowe (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Pokazuje, jak można zagnieżdżać jedną stronę wzorcową w innym.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich samouczki dziewięć Zaobserwowaliśmy sposób implementacji układu dla całej witryny za pomocą stron wzorcowych. Mówiąc stron wzorcowych pozwalają, deweloperów strony, zdefiniuj typowych znaczników w strony wzorcowej oraz konkretnych regionów, które można dostosować na podstawie zawartości strony zawartości strony. Kontrolek ContentPlaceHolder na stronę wzorcową wskazują możliwe do dostosowania regiony; dostosowany kod znaczników dla kontrolek ContentPlaceHolder są definiowane w zawartości strony za pomocą formantów zawartości.

Techniki strony wzorcowej, które zostały rozważyliśmy dotychczasowych są znakomite, jeśli masz pojedynczy układ używane w całej lokacji. Jednak wielu dużych witryn sieci Web ma układ lokacji, który został dostosowany w różnych sekcjach. Rozważmy na przykład aplikacja opieki zdrowotnej, używane przez pracowników szpitali do zarządzania informacji o pacjentach, działań i rozliczeń. Może to być trzy typy stron sieci web w tej aplikacji:

- Personel specyficzne dla elementu członkowskiego stron, gdzie członkowie personelu może aktualizować dostępność, wyświetlać harmonogramy lub żądanie urlopu.
- Specyficzne dla pacjenta strony gdzie członków personelu, Wyświetl lub Edytuj informacje dotyczące określonego pacjenta.
- Strony dotyczące rozliczeń, gdzie accountants Przejrzyj bieżące oświadczenia, stanów i raporty finansowe.

Każdej strony może udostępniać typowe układ, takich jak menu na górze i serii często używanych łączy wzdłuż dolnej krawędzi. Ale personelu, pacjentów i stron związanych z rozliczeniami może być konieczne dostosowanie ten ogólny układ. Na przykład prawdopodobnie wszystkich stron specyficzne dla pracowników powinna zawierać kalendarz i zadania listę zawierającą informacje o dostępności i harmonogram dzienny aktualnie zalogowanego użytkownika. Być może wszystkich stron specyficzne dla pacjenta chcesz pokazać nazwę, adres i ubezpieczeniowe informacji dla pacjenta, o których informacje jest edytowany.

Istnieje możliwość utworzyć takie układów niestandardowych za pomocą *zagnieżdżone strony wzorcowe*. Aby zaimplementować scenariusz powyżej, czy Zaczniemy od utworzenia stronę wzorcową, zdefiniowane zawartości układu, menu i stopka obejmujące całą lokację, za pomocą kontrolek ContentPlaceHolder Definiowanie regionów można dostosowywać. Następnie możemy utworzyć trzy zagnieżdżone strony wzorcowe, jeden dla każdego typu strony sieci web. Każda zagnieżdżona strona wzorcowa zdefiniuje zawartości między typem zawartości stron, które korzystają z strony wzorcowej. Innymi słowy zagnieżdżonej strony wzorcowej dla stron zawartości specyficzne dla pacjenta zawierałoby znaczników i logika do wyświetlania informacji na temat pacjenta edytowany. Podczas tworzenia nowej strony specyficzne dla pacjenta, firma Microsoft będzie powiązać go tej zagnieżdżonej strony wzorcowej.

Ten samouczek rozpoczyna się przez wyróżnianie zalety zagnieżdżone strony wzorcowe. Następnie widoczny jest sposób tworzenia i używania zagnieżdżone strony wzorcowe.

> [!NOTE]
> Zagnieżdżone strony wzorcowe były możliwe od programu .NET Framework w wersji 2.0. Jednak program Visual Studio 2005 nie zawiera obsługi zagnieżdżone strony wzorcowe w czasie projektowania. Dobra wiadomość jest, że Visual Studio 2008 oferuje rozbudowane środowisko czasu projektowania dla zagnieżdżone strony wzorcowe. Jeśli interesują Cię przy użyciu zagnieżdżone strony wzorcowe, ale nadal przy użyciu programu Visual Studio 2005, zapoznaj się z [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [porady dotyczące zagnieżdżone strony wzorcowe w programie VS 2005 projektowania](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Zalety zagnieżdżone strony wzorcowe

Wiele witryn sieci Web mają lokacja nadrzędna, a także bardziej dostosowanego projekty specyficzne dla niektórych typów stron. Na przykład naszego demonstracyjnej aplikacji internetowej utworzyliśmy podstawowe sekcji Administracja (stron w `~/Admin` folder). Obecnie strony sieci web w `~/Admin` folderu pełnić tych stron nie w sekcji Administracja tej samej strony wzorcowej (to znaczy, `Site.master` lub `Alternate.master`, w zależności od wybranych przez użytkownika).

> [!NOTE]
> Na razie poudawać czy naszą witrynę ma tylko jedną stronę wzorcową, `Site.master`. Będziemy mogli zaspokoić przy użyciu zagnieżdżone strony wzorcowe, za pomocą dwóch (lub więcej) strony wzorcowe, rozpoczynając od "Przy użyciu zagnieżdżonych Master strony dla sekcji Administracja" w dalszej części tego samouczka.


Wyobraź sobie, że firma Microsoft była monit o dostosowania układu stron administracyjnych, aby uwzględnić dodatkowe informacje i łącza, które nie zostałyby obecne w innych stron w witrynie. Istnieją cztery metody do zaimplementowania tego wymagania:

1. Ręcznie Dodaj informacje specyficzne dla Administracja i łącza do każdej strony zawartości `~/Admin` folderu.
2. Aktualizacja `Site.master` strony wzorcowej do uwzględnienia informacji specyficznych dla sekcji Administracja i łącza, a następnie dodać kod do głównej strony, aby pokazać lub ukryć te sekcje oparte na tego, czy jeden do stron administracji jest odwiedzana.
3. Utwórz nową stronę wzorcową specjalnie dla sekcji Administracja, skopiuj kod znaczników, od `Site.master`, Dodaj informacje specyficzne dla sekcji Administracja i łącza, a następnie zaktualizuj zawartości stron w `~/Admin` folder, aby użyć tego nowego wzorca Strona.
4. Tworzenie zagnieżdżonej strony wzorcowej, która jest powiązywana z `Site.master` i stronach zawartości w `~/Admin` nowy użycie folderu zagnieżdżonej strony wzorcowej. Ta zagnieżdżona strona wzorcowa obejmuje tylko dodatkowe informacje i łącza specyficzne dla stron administracyjnych i nie należy powtórzyć znaczników już zdefiniowana w `Site.master`.

Pierwsza opcja jest najmniej palatable. Celem za pomocą stron wzorcowych jest odbiegać od konieczności ręcznego kopiowania i wklejania typowych znaczników do nowych stron ASP.NET. Drugą opcją jest dopuszczalna, ale sprawia, że aplikacja jest mniej łatwego w utrzymaniu, zgodnie z jego bulks górę strony wzorcowe, za pomocą znaczników, tylko od czasu do czasu jest wyświetlany, i wymaga deweloperom edycji strony głównej, aby obejść ten kod znaczników, jak i do zapamiętania, kiedy dokładnie niektórych znaczników jest wyświetlany i, gdy jest on ukryty. To podejście będzie mniej firmy tenable jako dostosowania z coraz więcej typów stron sieci web, konieczne są spełnione przez pojedynczego strona wzorcowa.

Trzecia opcja usuwa niepotrzebnych danych i złożoność wystawia surfaced z drugą opcją. Jednak opcja trzy firmy główną wadą jest wymaganie, aby nas do kopiowania i wklejania wspólnej układ z `Site.master` do nowej strony wzorcowej specyficzne dla sekcji Administracja. Jeśli firma Microsoft później zdecydujesz się zmienić układ całej lokacji, musimy Pamiętaj, aby ją zmienić w dwóch miejscach.

Dostępny czwarty etap zagnieżdżone strony wzorcowe, Przekaż nam najlepsze opcje drugiego i trzeciego. Informacje układu dla całej witryny są obsługiwane w jednym pliku - najwyższego poziomu strony wzorcowej —, podczas gdy specyficzne dla poszczególnych regionów zawartości jest podzielony różne pliki.

Ten samouczek rozpoczyna się od przyjrzeć się tworzenie i używanie prostych zagnieżdżonej strony wzorcowej. Możemy utworzyć nowego najwyższego poziomu strony wzorcowej, dwa zagnieżdżone strony wzorcowe i dwie strony zawartości. Począwszy od "Przy użyciu zagnieżdżonych Master strony dla sekcji Administracja" przyjrzymy się aktualizowanie naszej istniejącej architektury strony wzorcowej obejmujący użycie zagnieżdżone strony wzorcowe. W szczególności utworzymy zagnieżdżonej strony wzorcowej i użyj go, aby uwzględnić dodatkowe niestandardowej zawartości dla zawartości stron w `~/Admin` folderu.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Krok 1: Tworzenie prostego najwyższego poziomu strony wzorcowej

Tworzenie zagnieżdżonych wzorzec oparty na jednym z istniejącego wzorca strony i następnie aktualizowania istniejącej strony zawartości, aby użyć tej nowej zagnieżdżonej strony wzorcowej zamiast najwyższego poziomu strony wzorcowej pociąga za sobą złożoność, ponieważ niektóre oczekiwać już istniejących stron zawartości Kontrolek ContentPlaceHolder zdefiniowane w najwyższego poziomu strony wzorcowej. W związku z tym zagnieżdżonej strony wzorcowej musi również zawierać tych samych kontrolek ContentPlaceHolder o takich samych nazwach. Ponadto naszej aplikacji pokazowej określonego ma dwie strony wzorcowej (`Site.master` i `Alternate.master`) dynamiczne są przypisane do strony zawartości na podstawie preferencji użytkownika, który dodatkowo dodaje do tę złożoność. Przyjrzymy się aktualizowanie istniejących aplikacji na używanie zagnieżdżone strony wzorcowe w dalszej części tego samouczka, ale umożliwia pierwszy skoncentrować się na prostej zagnieżdżone strony wzorcowe przykład.

Utwórz nowy folder o nazwie `NestedMasterPages` a następnie dodaj nowy plik strony głównej do tego folderu o nazwie `Simple.master`. (Zobacz rysunek 1 dla zrzut ekranu Eksploratora rozwiązań po dodaniu ten folder i plik). Przeciągnij `AlternateStyles.css` pliku arkusza stylów z poziomu Eksploratora rozwiązań do projektanta. Spowoduje to dodanie `<link>` elementu do pliku arkusza stylów w `<head>` elementu, po upływie którego strony wzorcowej `<head>` znaczników elementu powinien wyglądać następująco:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Następnie dodaj następujący kod znaczników w formularzu sieci Web programu `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Ten kod znaczników Wyświetla łącze pod tytułem "Zagnieżdżone strony wzorcowe (proste)" w górnej części strony z użyciem dużej czcionki białe na tle granatowym. Poniżej, jest `MainContent` ContentPlaceHolder. Rysunek 1 pokazuje `Simple.master` strony wzorcowej po załadowaniu w projektancie programu Visual Studio.


[![Zagnieżdżona strona wzorcowa definiuje zawartość określonej strony w sekcji Administracja](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Rysunek 01**: zagnieżdżone Master strony definiuje zawartości specyficzne dla strony w sekcji Administracja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Krok 2: Tworzenie prostego zagnieżdżona strona wzorcowa

`Simple.master` zawiera dwóch kontrolek ContentPlaceHolder: `MainContent` ContentPlaceHolder dodaliśmy w obrębie formularza sieci Web, wraz z `head` ContentPlaceHolder w `<head>` elementu. Gdybyśmy wybrali Tworzenie strony zawartości, który należy powiązać `Simple.master` strony zawartości miałby dwóch kontrolek zawartości odwołujące się do dwóch kontrolek ContentPlaceHolder. Podobnie jeśli firma Microsoft Tworzenie zagnieżdżonej strony wzorcowej, który należy powiązać `Simple.master` , a następnie zagnieżdżonej strony wzorcowej będzie mieć dwa formanty zawartości.

Dodajmy nową stronę wzorcową zagnieżdżonych do `NestedMasterPages` folder o nazwie `SimpleNested.master`. Kliknij prawym przyciskiem myszy `NestedMasterPages` folder i wybierz polecenie Dodaj nowy element. Wyświetlenie okna dialogowego Dodaj nowy element pokazano na rysunku 2. Wybierz typ szablonu stronę wzorcową i wpisz nazwę nowej strony wzorcowej. Aby wskazać, że nowe strony wzorcowej powinny być zagnieżdżona strona wzorcowa, zaznacz pole wyboru "Wybierz stronę wzorcową".

Następnie kliknij przycisk Dodaj. Spowoduje to wyświetlenie tego samego wybierz okno dialogowe strony wzorcowej, zostanie wyświetlony podczas tworzenia wiązania zawartości strony do strony wzorcowej (zobacz rysunek 3). Wybierz `Simple.master` strony wzorcowej w `NestedMasterPages` folder i kliknij przycisk OK.

> [!NOTE]
> Jeśli utworzono witryny sieci Web ASP.NET przy użyciu modelu projektu aplikacji sieci Web zamiast modelu projektu witryny sieci Web nie zobaczą pola wyboru "Wybierz stronę wzorcową" w oknie dialogowym Dodaj nowy element pokazano na rysunku 2. Aby Tworzenie zagnieżdżonej strony wzorcowej, korzystając z modelu projektu aplikacji sieci Web, należy wybrać szablon zagnieżdżona strona wzorcowa (a nie szablon strona wzorcowa). Po wybierając szablon zagnieżdżoną stronę wzorcową i klikając przycisk Dodaj, taka sama wybierz stronę wzorcową, zostanie wyświetlone okno dialogowe pokazany na rysunku 3.


[![Sprawdź &quot;wybierz stronę wzorcową&quot; pole wyboru, aby dodać zagnieżdżoną stronę wzorcową](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Rysunek 02**: zaznacz pole wyboru "Wybierz stronę wzorcową", aby dodać zagnieżdżoną stronę wzorcową ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image6.png))


[![Zagnieżdżona strona wzorcowa należy powiązać strony wzorcowej Simple.master](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Rysunek 03**: powiązywanie zagnieżdżoną stronę wzorcową do `Simple.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image9.png))


Oznaczeniu deklaracyjnym zagnieżdżonej strony wzorcowej, pokazano poniżej, zawiera dwie kontrolki zawartości odwołujące się do dwóch kontrolek ContentPlaceHolder najwyższego poziomu strony wzorcowej.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Z wyjątkiem `<%@ Master %>` dyrektywy, początkowej oznaczeniu deklaracyjnym zagnieżdżonej strony wzorcowej jest taka sama jak znaczników, generowany podczas tworzenia wiązania strony zawartości do tego samego najwyższego poziomu strony wzorcowej. Strony zawartości, takich jak `<%@ Page %>` dyrektywy, `<%@ Master %>` zawiera dyrektywy tutaj `MasterPageFile` atrybut określający stronę wzorcową nadrzędnego zagnieżdżonej strony wzorcowej. Główna różnica między zagnieżdżonej strony wzorcowej oraz strony zawartości, która jest powiązana z tym samym najwyższego poziomu strony wzorcowej jest, że zagnieżdżonej strony wzorcowej mogą obejmować kontrolek ContentPlaceHolder. Kontrolek ContentPlaceHolder zagnieżdżonej strony wzorcowej zdefiniować regionów, w którym zawartości strony można dostosować znaczniki.

Aktualizacja ta zagnieżdżona strona wzorcowa tak, aby wyświetlała tekst "Hello, z SimpleNested!" w formancie zawartości, który odpowiada `MainContent` ContentPlaceHolder kontroli.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Po wprowadzeniu to dodawanie należy zapisać zagnieżdżonej strony wzorcowej, a następnie dodaj nową stronę zawartości w celu `NestedMasterPages` folder o nazwie `Default.aspx`, który należy powiązać `SimpleNested.master` strony wzorcowej. Podczas dodawania tej strony może być zaskoczeniem zobaczyć, czy zawiera on nie kontrolek zawartości (zobacz rysunek 4)! Strony zawartości można uzyskać dostęp tylko do jej *nadrzędnego* kontrolek ContentPlaceHolder strony głównej. `SimpleNested.master` nie zawiera żadnych kontrolek ContentPlaceHolder; wszystkie formanty zawartości nie może zagrażać dowolnej strony zawartości, powiązane z tej strony wzorcowej.


[![Nowa strona zawartości zawiera nie formantów zawartości](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Rysunek 04**: nowej zawartości strony zawiera nie formanty zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image12.png))


Co należy zrobić to aktualizowanie zagnieżdżonej strony wzorcowej (`SimpleNested.master`) do uwzględnienia kontrolek ContentPlaceHolder. Zazwyczaj należy zagnieżdżone strony wzorcowe obejmujący ContentPlaceHolder dla każdego ContentPlaceHolder zdefiniowane przy jego nadrzędny strony wzorcowej, umożliwiając jego strony głównej podrzędny lub zawartości strony, aby pracować z dowolnymi ContentPlaceHolder najwyższego poziomu strony wzorcowej kontrolki.

Aktualizacja `SimpleNested.master` strony wzorcowej, które mają zostać objęte ContentPlaceHolder jego dwóch kontrolek zawartości. Nadaj kontrolek ContentPlaceHolder taką samą nazwę jak formant ContentPlaceHolder, który ich formantu zawartości, który odwołuje się do. Oznacza to, Dodaj formant ContentPlaceHolder o nazwie `MainContent` do zawartości w kontrolce `SimpleNested.master` odwołujący się `MainContent` ContentPlaceHolder w `Simple.master`. Te same czynności wykonasz w formancie zawartości, który odwołuje się do `head` ContentPlaceHolder.

> [!NOTE]
> Gdy polecam używanie nazw kontrolek ContentPlaceHolder w zagnieżdżonej strony wzorcowej, taka sama jak kontrolek ContentPlaceHolder w najwyższego poziomu strony wzorcowej, to nazewnictwa symetrii nie jest wymagana. Kontrolek ContentPlaceHolder można udostępnić z Twojej zagnieżdżona strona wzorcowa dowolną nazwę, jaką chcesz. Jednak I łatwiejsze do zapamiętania kontrolek ContentPlaceHolder odnoszą się do jakich regionach stronę, jeśli Moje najwyższego poziomu strony wzorcowej i zagnieżdżone strony wzorcowe, użyj takich samych nazwach.


Po wprowadzeniu te dodatki swoje `SimpleNested.master` oznaczeniu deklaracyjnym strony wzorcowej powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Usuń `Default.aspx` strona, którą właśnie utworzyliśmy zawartości, a następnie ponownie dodać, wiążące go do `SimpleNested.master` strony wzorcowej. Tym razem program Visual Studio dodaje dwie kontrolki zawartości do `Default.aspx`, odwołuje się do kontrolek ContentPlaceHolder teraz zdefiniowana w `SimpleNested.master` (patrz rysunek 6). Dodaj tekst "Hello, z Default.aspx!" w zawartości kontrolować, do której odwołanie `MainContent`.

Rysunek 5. pokazuje trzy obiekty zaangażowane w tym miejscu — `Simple.master`, `SimpleNested.master`, i `Default.aspx` — i jak odnoszą się do siebie nawzajem. Jak pokazano na diagramie, zagnieżdżonej strony wzorcowej implementuje formanty zawartości dla ContentPlaceHolder jego elementu nadrzędnego. Jeśli tych regionów muszą być dostępne dla strony zawartości, zagnieżdżonej strony wzorcowej, należy dodać swoje własne kontrolek ContentPlaceHolder z kontrolkami zawartości.


[![Strony wzorcowe najwyższego poziomu i zagnieżdżonych dyktowanie układu strony zawartości](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Rysunek 05**: najwyższego poziomu i zagnieżdżone strony wzorcowe dyktowanie układu strony zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image15.png))


To zachowanie ilustruje, jak strony zawartości lub strony wzorcowej jest tylko cognizant jego nadrzędny strony wzorcowej. To zachowanie jest również wskazywany przez projektanta programu Visual Studio. Rysunek 6 przedstawia Projektant `Default.aspx`. Gdy Projektant wyraźnie pokazuje, jakie regiony są edytowalne, z poziomu strony zawartości i nie są fragmenty, nie odróżniania, nie można edytować regiony są z zagnieżdżonej strony wzorcowej i jakie regiony z najwyższego poziomu strony wzorcowej.


[![Zawartości teraz strony zawiera formanty zawartości dla kontrolek ContentPlaceHolder zagnieżdżona strona wzorcowa](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Rysunek 06**: zawartości strony teraz zawiera formanty zawartości dla kontrolek ContentPlaceHolder zagnieżdżoną stronę wzorcową firmy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Krok 3: Dodawanie drugiego proste zagnieżdżonej strony wzorcowej

Zaletą zagnieżdżone strony wzorcowe jest bardziej oczywiste, gdy wiele zagnieżdżone strony wzorcowe. Aby zilustrować tej korzyści, należy utworzyć innej zagnieżdżonej strony wzorcowej w `NestedMasterPages` folderu; ta nowa nazwa zagnieżdżonej strony wzorcowej `SimpleNestedAlternate.master` , który należy powiązać `Simple.master` strony wzorcowej. Dodaj kontrolek ContentPlaceHolder w zagnieżdżonej strony wzorcowej dwóch kontrolek zawartości, takie jak zrobiliśmy w kroku 2. Ponadto Dodaj tekst "Hello, z SimpleNestedAlternate!" w formancie zawartości, który odnosi się do najwyższego poziomu strony wzorcowej `MainContent` ContentPlaceHolder. Po wprowadzeniu tych zmian nowej zagnieżdżonej strony wzorcowej w oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Utwórz stronę zawartości o nazwie `Alternate.aspx` w `NestedMasterPages` folder, który należy powiązać `SimpleNestedAlternate.master` zagnieżdżonej strony wzorcowej. Dodaj tekst "Hello, z alternatywnym!" w formancie zawartości, który odpowiada `MainContent`. Rysunek nr 7 przedstawia `Alternate.aspx` oglądany przez projektanta programu Visual Studio.


[![Alternate.aspx jest powiązany z poziomu strony wzorcowej SimpleNestedAlternate.master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Rysunek 07**: `Alternate.aspx` jest powiązany z `SimpleNestedAlternate.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image21.png))


Porównaj Designer na rysunku 7 programu Designer na rysunku 6. Obie strony z zawartością udostępnianie tego samego układu zdefiniowane w najwyższego poziomu strony wzorcowej (`Simple.master`), a mianowicie tytuł "Zagnieżdżone Master strony samouczek (proste)". Jeszcze mają różne zawartości zdefiniowanej w ich nadrzędnej strony wzorcowe — tekst "Hello, z SimpleNested!" Rysunek 6 i "Hello, z SimpleNestedAlternate!" na rysunku 7. Udzieleniu te różnice w tym miejscu są proste, ale można rozszerzyć tego przykładu, aby uwzględnić bardziej istotne różnice. Na przykład `SimpleNested.master` strony mogą zawierać menu z opcjami, które są określone na stronach zawartości, natomiast `SimpleNestedAlternate.master` może mieć informacji dotyczących stron zawartości, które powiązać rozwiązanie.

Teraz Wyobraź sobie, że Musieliśmy wprowadzić zmiany do nadrzędna układu witryny. Na przykład Wyobraź sobie, że Chcieliśmy, aby dodać listę typowych łączy do wszystkich stron zawartości. W tym firma Microsoft aktualizuje najwyższego poziomu strony wzorcowej `Simple.master`. Wszelkie zmiany są natychmiast odzwierciedlane w jego zagnieżdżone strony wzorcowe, a w konsekwencji, ich strony z zawartością.

Aby zademonstrować prostotę, z którym można zmienić układ lokacja nadrzędna, otwórz `Simple.master` strony wzorcowej i Dodaj następujący kod między `topContent` i `mainContent` `<div>` elementy:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Spowoduje to dodanie dwóch łączy do góry każdej strony, która jest powiązywana z `Simple.master`, `SimpleNested.master`, lub `SimpleNestedAlternate.master`; te zmiany natychmiast zastosowane do wszystkich zagnieżdżone strony wzorcowe i ich zawartości stron. Rysunek 8 przedstawia `Alternate.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Należy pamiętać, dodawanie łączy w górnej części strony (w porównaniu do rysunek 7).


[![Zmieniono na stronie wzorcowej najwyższego poziomu są natychmiast odzwierciedlane w jego zagnieżdżone strony wzorcowe i ich zawartości strony](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Rysunek 08**: zmienić na stronie wzorcowej najwyższego poziomu są natychmiast odzwierciedlane w jego zagnieżdżone strony wzorcowe i ich strony zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Za pomocą zagnieżdżona strona wzorcowa dla sekcji Administracja

W tym momencie możemy sprawdzono zalety wzorzec zagnieżdżone strony i ma już, jak tworzyć i używać ich w aplikacji ASP.NET. Przykłady w krokach 1, 2 i 3, jednak zaangażowani, tworzenie nowej strony wzorcowej najwyższego poziomu, nowe zagnieżdżone strony wzorcowe i nowych stron zawartości. Co sądzisz o Dodawanie nowej zagnieżdżonej strony wzorcowej w witrynie sieci Web przy użyciu istniejącej najwyższego poziomu strony wzorcowej oraz strony z zawartością?

Integrowanie zagnieżdżona strona wzorcowa z istniejącej witryny sieci Web i kojarząc ją z istniejących stron zawartości wymaga nieco więcej wysiłku niż zaczynasz od zera. Kroki od 4, 5, 6 i 7 Eksploruj te wyzwania, jak możemy rozszerzyć nasze demonstracyjnej aplikacji do uwzględnienia nowej zagnieżdżonej strony wzorcowej o nazwie `AdminNested.master` który zawiera instrukcje dla administratora i jest używany przez strony ASP.NET w `~/Admin` folderu.

Integrowanie zagnieżdżona strona wzorcowa naszej aplikacji pokazowej wprowadza następujące progi:

- Istniejąca zawartość stron w `~/Admin` folder ma pewne oczekiwania ze strony głównej. Po pierwsze spełniają oczekiwane niektórych kontrolek ContentPlaceHolder być obecne. Ponadto `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` strony wywoływać publicznego strony wzorcowej `RefreshRecentProductsGrid` metody, ustaw jego `GridMessageText` właściwości lub ma program obsługi zdarzeń dla jego `PricesDoubled` zdarzeń. W związku z tym należy podać naszych zagnieżdżonej strony wzorcowej, tych samych kontrolek ContentPlaceHolder i publiczne elementy członkowskie.
- W poprzednim samouczku Rozszerzyliśmy `BasePage` klasy, aby dynamicznie ustawić `Page` obiektu `MasterPageFile` w zmiennej sesji na podstawie właściwości. Jak się firma Microsoft obsługuje dynamicznej strony wzorcowe po użyciu zagnieżdżone strony wzorcowe?

Te dwa problemy ujawni zgodnie z zagnieżdżonej strony wzorcowej do skompilowania i używać jej z istniejących stron zawartości. Utworzymy badanie i rozwiązanie tych problemów, gdy występują one.

## <a name="step-4-creating-the-nested-master-page"></a>Krok 4: Tworzenie zagnieżdżonej strony wzorcowej

Naszym pierwszym zadaniem jest tworzenie zagnieżdżonej strony wzorcowej do użycia przez strony w sekcji Administracja. Jak widzieliśmy w kroku 2, podczas dodawania nowego zagnieżdżona strona wzorcowa musimy określanie strony wzorcowej nadrzędnego zagnieżdżonej strony wzorcowej. Ale mamy dwie strony wzorcowe najwyższego poziomu: `Site.master` i `Alternate.master`. Odwołania, który utworzyliśmy `Alternate.master` w poprzednim samouczku i wpisano kod `BasePage` klasy tego zestawu `Page` obiektu `MasterPageFile` właściwości w czasie wykonywania do jednej `Site.master` lub `Alternate.master` zależności od wartości `MyMasterPage` Zmiennej sesji.

W jaki sposób możemy skonfigurować nasz zagnieżdżonej strony wzorcowej, tak aby używał odpowiedniego najwyższego poziomu strony wzorcowej? Mamy dwie opcje:

- Utwórz dwa zagnieżdżone strony wzorcowe `AdminNestedSite.master` i `AdminNestedAlternate.master`i wiązania ich z najwyższego poziomu strony wzorcowe `Site.master` i `Alternate.master`, odpowiednio. W `BasePage`, następnie możemy ustawić `Page` obiektu `MasterPageFile` odpowiednie zagnieżdżonej strony wzorcowej.
- Utwórz pojedynczy zagnieżdżonej strony wzorcowej i stron zawartości, użyj tej konkretnej strony wzorcowej. Następnie, w czasie wykonywania, czy musimy zagnieżdżonej strony wzorcowej `MasterPageFile` właściwość do odpowiedniej strony wzorcowej najwyższego poziomu w czasie wykonywania. (Przez teraz, może mieć wybierana, stron wzorcowych również mają `MasterPageFile` właściwości.)

Użyjmy drugiej opcji. Utwórz pojedynczy zagnieżdżonych pliku strony głównej w `~/Admin` folder o nazwie `AdminNested.master`. Ponieważ zarówno `Site.master` i `Alternate.master` mają taki sam zestaw kontrolek ContentPlaceHolder, nie ma znaczenia, jakie strony wzorcowej można powiązać, mimo że zachęcam Cię do powiązać `Site.master` dla sake firmy spójności.


[![Dodaj zagnieżdżona strona wzorcowa do folderu ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Rysunek 09**: zagnieżdżona strona wzorcowa, aby dodać `~/Admin` folderu. ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image27.png))


Ponieważ zagnieżdżonej strony wzorcowej jest powiązany z strony wzorcowej za pomocą czterech kontrolek ContentPlaceHolder, program Visual Studio dodaje cztery formanty do nowego pliku zagnieżdżona strona wzorcowa początkowej znaczników zawartości. Jak Przeprowadziliśmy w krokach 2 i 3, Dodaj kontrolkę ContentPlaceHolder w każdej kontrolce zawartości, nadając mu taką samą nazwę jak kontrolka ContentPlaceHolder najwyższego poziomu strony wzorcowej. Również dodać następujący kod do formantu zawartości, która odpowiada `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Następnie zdefiniuj `instructions` klasy CSS w `Styles.css` i `AlternateStyles.css` pliki CSS. Następujące reguły CSS spowodować, że elementy HTML stylem `instructions` klasy, które mają być wyświetlane za pomocą koloru światła żółte tło i obramowanie czarny, stałe:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Ponieważ ten kod znaczników została dodana do zagnieżdżonej strony wzorcowej, tylko pojawi się na tych stronach, korzystających z zagnieżdżonej strony wzorcowej (to znaczy, strony w sekcji Administracja).

Po wprowadzeniu te dodatki do strony głównej zagnieżdżonych, jego oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Należy zauważyć, że każdy formant zawartości ma formant ContentPlaceHolder i który kontrolek ContentPlaceHolder `ID` właściwości są przypisane te same wartości kontrolki z odpowiedniego ContentPlaceHolder najwyższego poziomu strony wzorcowej. Ponadto znaczników specyficzne dla sekcji Administracja pojawia się w `MainContent` ContentPlaceHolder.

Na rysunku nr 10 przedstawiono `AdminNested.master` zagnieżdżona strona wzorcowa oglądany przez projektanta programu Visual Studio. Widać, zgodnie z instrukcjami w żółte pole w górnej części `MainContent` formantu zawartości.


[![Zagnieżdżona strona wzorcowa rozszerza najwyższego poziomu strony wzorcowej, aby dołączyć instrukcje dla administratora.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Na rysunku nr 10**: zagnieżdżona strona wzorcowa rozszerza najwyższego poziomu strony wzorcowej, aby dołączyć instrukcje dla administratora. ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Krok 5: Aktualizowanie istniejących stron zawartości, aby użyć nowych zagnieżdżona strona wzorcowa

W dowolnym momencie możemy dodać nową stronę zawartości do sekcji Administracja, należy powiązać `AdminNested.master` stronę wzorcową, którą właśnie utworzyliśmy. Ale co istniejących stron zawartości? Obecnie dziedziczyć wszystkie strony zawartości w witrynie `BasePage` klasy, która programowo ustawia stronę wzorcową z poziomu strony zawartości w czasie wykonywania. Nie jest oczekiwane w przypadku zawartości stron w sekcji Administracja zachowanie. Zamiast tego chcemy, aby te zawartości strony, aby zawsze używać `AdminNested.master` strony. Będzie ona odpowiedzialność zagnieżdżonej strony wzorcowej do wyboru bezpośrednio najwyższego poziomu strony zawartości w czasie wykonywania.

Aby najlepszym sposobem osiągnięcia tego żądanego, zachowanie jest utworzenie nowej niestandardowej strony podstawowej klasy o nazwie `AdminBasePage` rozszerzający `BasePage` klasy. `AdminBasePage` następnie można zastąpić `SetMasterPageFile` i ustaw `Page` obiektu `MasterPageFile` ustaloną wartość "~ / Admin/AdminNested.master". W ten sposób dowolną stronę który pochodzi od klasy `AdminBasePage` użyje `AdminNested.master`, podczas gdy dowolnej strony, który pochodzi z `BasePage` będzie miał jego `MasterPageFile` właściwością dynamicznie na "~ / Site.master" lub "~ / Alternate.master" na podstawie wartości `MyMasterPage` Zmiennej sesji.

Najpierw dodaj plik klasy do `App_Code` folder o nazwie `AdminBasePage.vb`. Masz `AdminBasePage` rozszerzyć `BasePage` a następnie zastąpić `SetMasterPageFile` metody. W tej metodzie należy przypisać `MasterPageFile` wartość "~ / Admin/AdminNested.master". Po wprowadzeniu tych zmian na klasę plik powinien wyglądać podobny do następującego:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Teraz musisz mieć istniejących stron zawartości w administracyjnej części pochodzić od `AdminBasePage` zamiast `BasePage`. Przejdź do pliku klasy CodeBehind dla każdej strony zawartości `~/Admin` folder i wprowadzić tę zmianę. Na przykład w `~/Admin/Default.aspx` należy zmienić deklarację klasy związane z kodem z:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Do:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Rysunek 11 przedstawia sposób najwyższego poziomu strony wzorcowej (`Site.master` lub `Alternate.master`), zagnieżdżonej strony wzorcowej (`AdminNested.master`), i stronach zawartości sekcji Administracja odnoszą się do siebie nawzajem.


[![Zagnieżdżona strona wzorcowa definiuje zawartość określonej strony w sekcji Administracja](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Rysunek 11**: zagnieżdżone Master strony definiuje zawartości specyficzne dla strony w sekcji Administracja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Krok 6: Dublowanie metody publiczne i właściwości strony wzorcowej

Pamiętamy `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stronach, interakcji programowej z strony wzorcowej: `~/Admin/AddProduct.aspx` wywołań strony wzorcowej w publicznych `RefreshRecentProductsGrid` metody i ustawia jego `GridMessageText` właściwości `~/Admin/Products.aspx` ma program obsługi zdarzeń dla `PricesDoubled` zdarzeń. W poprzednim samouczku utworzyliśmy `MustInherit` `BaseMasterPage` klasy, która zdefiniowana te publiczne elementy członkowskie.

`~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stron przyjęto założenie, że ich strony wzorcowej jest pochodną `BaseMasterPage` klasy. `AdminNested.master` Strony, jednak obecnie rozszerza `System.Web.UI.MasterPage` klasy. W rezultacie, gdy użytkownik odwiedzi `~/Admin/Products.aspx` `InvalidCastException` generowany jest komunikat o: "nie można rzutować obiektu typu" ASP.admin\_adminnested\_master "na typ"BaseMasterPage"."

Aby rozwiązać ten musimy mieć problem `AdminNested.master` rozszerzyć klasy CodeBehind `BaseMasterPage`. Zaktualizuj deklaracji klasy CodeBehind zagnieżdżonej strony wzorcowej od:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Do:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Jeszcze nie skończyliśmy jeszcze. Musimy Przesłoń składowe oznaczone jako `MustOverride`, to znaczy `RefreshRecentProductsGrid` i `GridMessageText`. Te elementy członkowskie są używane przez najwyższego poziomu strony głównej można zaktualizować ich interfejsów użytkownika. (W rzeczywistości tylko `Site.master` strony wzorcowej używa tych metod, mimo że obu stron wzorca najwyższego poziomu wdrożenia tych metod, ponieważ oba rozszerzenia `BaseMasterPage`.)

Chociaż należy zaimplementować te składowe w `AdminNested.master`, tych implementacji należy zrobić, wystarczy po prostu wywoływanie jednego elementu członkowskiego w najwyższego poziomu strony wzorcowej posługują się zagnieżdżonej strony wzorcowej. Na przykład, kiedy zawartość strony w sekcji Administracja wywołuje zagnieżdżonej strony wzorcowej `RefreshRecentProductsGrid` metody, wszystkie zagnieżdżone strony wzorcowej musi wykonać, z kolei, wywołaj `Site.master` lub `Alternate.master`firmy `RefreshRecentProductsGrid` metody.

Aby to osiągnąć, Rozpocznij, dodając następujące `@MasterType` dyrektywę na początku `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Pamiętamy `@MasterType` dyrektywy dodaje właściwość silnie typizowaną do klasy związane z kodem o nazwie `Master`. Następnie zastąp `RefreshRecentProductsGrid` i `GridMessageText` elementów członkowskich i po prostu delegowanie wywołanie `Master`użytkownika odpowiadającej metody:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Przy użyciu tego kodu w miejscu można znaleźć i używać zawartości stron w sekcji Administracja. Przedstawia rysunek 12 `~/Admin/Products.aspx` stronie podczas przeglądania za pośrednictwem przeglądarki. Jak widać, strona zawiera pole instrukcje administracji, która jest zdefiniowana w zagnieżdżonej strony wzorcowej.


[![Zawartość strony w sekcji Administracja dołączyć instrukcje u góry każdej strony](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Rysunek 12**: strony z zawartością w instrukcjach obejmują sekcji Administracja u góry każdej strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Krok 7: Przy użyciu odpowiednich najwyższego poziomu strony wzorcowej w czasie wykonywania

Mimo że wszystkie strony zawartości w sekcji Administracja w pełni funkcjonalne, używają tej samej strony wzorcowej najwyższego poziomu i Ignoruj strony wzorcowej wybrane przez użytkownika w `ChooseMasterPage.aspx`. To zachowanie jest fakt, że ma zagnieżdżonej strony wzorcowej jego `MasterPageFile` statycznie właściwością `Site.master` w jego `<%@ Master %>` dyrektywy.

Aby użyć najwyższego poziomu strony wzorcowej wybierane przez użytkownika końcowego, należy ustawić `AdminNested.master`firmy `MasterPageFile` właściwość z wartością w `MyMasterPage` zmiennej sesji. Ponieważ ustawiliśmy stron zawartości `MasterPageFile` właściwości w `BasePage`, użytkownik może uznać, że firma Microsoft ustawi zagnieżdżonej strony wzorcowej `MasterPageFile` właściwość `BaseMasterPage` lub `AdminNested.master`firmy z kodem klasę. To nie będzie działać, jednak ponieważ potrzebujemy ustawiono `MasterPageFile` właściwości do końca etapu PreInit. Najwcześniejsza godzina, który firma Microsoft może programowo nacisnąć w cyklu życia strony ze strony wzorcowej jest na etapie inicjowania, (która występuje po etapie PreInit).

Dlatego musimy zagnieżdżonej strony wzorcowej `MasterPageFile` właściwości ze stron zawartości. Tylko zawartość strony, które używają `AdminNested.master` strony wzorcowej pochodzić od `AdminBasePage`. Firma Microsoft może więc tę logikę istnieje. W kroku 5, firma Microsoft overrode `SetMasterPageFile` metody, ustawienie obiektu strony `MasterPageFile` właściwość "~ / Admin/AdminNested.master". Aktualizacja `SetMasterPageFile` można również ustawić strony wzorcowej `MasterPageFile` właściwości do wyników przechowywanych w sesji:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession` Metody, która dodaliśmy do `BasePage` klasy w poprzednim samouczku zwraca ścieżkę pliku odpowiednią stronę wzorcową oparte na wartości zmiennej sesji.

Dzięki tej zmianie w miejscu wybranej strony wzorcowej przez użytkownika są przenoszone do sekcji Administracja. Rysunek 13 pokazuje, jak rysunek 12, ale po użytkownik zmienił ich zaznaczenie strony wzorcowej do tej samej stronie `Alternate.master`.


[![Na stronie Administracja zagnieżdżonych używa najwyższego poziomu strony wzorcowej wybrane przez użytkownika](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Rysunek 13**: zagnieżdżone strony administrowania używa najwyższego poziomu głównego strony wybranego przez użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Podsumowanie

Wiele podobnych jak zawartości strony można powiązać z strony wzorcowej, możliwe jest tworzenie zagnieżdżonej strony wzorcowe, konfigurując stronę wzorcową podrzędnych powiązać nadrzędny strony wzorcowej. Strona wzorcowa podrzędne mogą definiować formanty zawartości dla każdego z jego elementu nadrzędnego kontrolek ContentPlaceHolder; go następnie dodać swój własny kontrolek ContentPlaceHolder (a także innych znaczników) do tych kontrolek zawartości. Zagnieżdżone strony wzorcowe są bardzo przydatne w aplikacjach sieci web w dużych, gdzie wszystkie strony Udostępnianie nadrzędna wyglądu i działania, ale niektóre sekcje witryny wymagają unikatowych dostosowania.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zagnieżdżone strony wzorcowe ASP.NET](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Porady dotyczące zagnieżdżone strony wzorcowe i projektowania dla programu VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 zagnieżdżone pomocy technicznej strony główne](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu ASP/ASP.NET książki i założyciel 4GuysFromRolla.com pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](specifying-the-master-page-programmatically-vb.md)
