---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: "Zagnieżdżone strony wzorcowe (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Pokazuje, jak można zagnieżdżać jednej strony wzorcowej w innym."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9059e358311cc80b6a64aa3ee1168f4ffcd4e94c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="nested-master-pages-vb"></a>Zagnieżdżone strony wzorcowe (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Pokazuje, jak można zagnieżdżać jednej strony wzorcowej w innym.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich samouczki dziewięć zaobserwowano implementowania układ całej lokacji przy użyciu stron wzorcowych. Mówiąc stron wzorcowych możemy, developer strony, aby zdefiniować markup wspólnego na stronie głównej wraz z określonych regionów, które mogą być dostosowywane na podstawie zawartości strony zawartości strony. Formanty elementu ContentPlaceHolder na stronie głównej wskazać dostosowania regionów; dostosowane znaczników dla formantów elementu ContentPlaceHolder są definiowane na stronie zawartości za pomocą formantów zawartości.

Techniki strony wzorcowej, które firma Microsoft zostały przedstawione dotychczasowych jest wspaniała, jeśli mieć pojedynczy układ używane w całej lokacji. Jednak wielu dużych witryn sieci Web ma układu witryny, dostosowane w poszczególnych sekcjach. Rozważmy na przykład aplikacji opieki zdrowotnej, używane przez pracowników szpital Twoich do zarządzania informacji pacjenta, działań i rozliczeń. Może to być trzy typy stron sieci web w tej aplikacji:

- Personel specyficzne dla elementu członkowskiego stron, którym pracownicy mogą aktualizować dostępności, wyświetlać harmonogramy, lub zwróć urlop.
- Strony pacjenta specyficzne, gdzie pracownicy Wyświetl lub Edytuj informacje dotyczące określonego pacjenta.
- Gdzie księgowym Przejrzyj bieżącej strony dotyczące rozliczeń oświadczeń stanów i raporty finansowe.

Każdej strony może udostępnić układ wspólne, takie jak menu na górze i serii często używanych łączy wzdłuż dolnej. Ale pracowników, pacjenta i stron związanych z rozliczeniami może być konieczne dostosowanie ten Układ ogólny. Na przykład możliwe, że wszystkie strony specyficzne dla personelu powinna zawierać kalendarz i zadania listę zawierającą informacje o dostępności i harmonogramu dziennego aktualnie zalogowanego użytkownika. Możliwe, że wszystkie strony pacjenta specyficzne chcesz pokazać nazwisko, adres i informacji dotyczących pacjenta, którego informacje jest edytowany ubezpieczenia.

Istnieje możliwość utworzenia takich układów niestandardowych za pomocą *zagnieżdżone strony wzorcowe*. Do zaimplementowania scenariusza powyżej, firma Microsoft będzie Rozpocznij od utworzenia zdefiniowana zawartości układ, menu i stopki całej lokacji, z Definiowanie regionów dostosowywalne Elementy ContentPlaceHolders strony wzorcowej. Następnie możemy utworzyć trzy zagnieżdżone strony wzorcowe, po jednej dla każdego typu strony sieci web. Każdy osadzonej stronie głównej zdefiniowane zawartości między typem zawartości stron, które używają strony wzorcowej. Innymi słowy zagnieżdżone strony wzorcowej dla stron zawartości określonego pacjenta obejmuje znaczników i logiki do wyświetlania informacji na temat pacjenta edytowany. Podczas tworzenia nowej strony specyficzne dla pacjenta firma Microsoft może powiązać go z tego osadzonej stronie głównej.

W tym samouczku rozpoczyna się od wyróżnianie zalet zagnieżdżone strony wzorcowe. Następnie widoczny jest sposób tworzenia i używania zagnieżdżone strony wzorcowe.

> [!NOTE]
> Zagnieżdżone strony wzorcowe zostały możliwe od programu .NET Framework w wersji 2.0. Visual Studio 2005 nie zawiera jednak obsługi zagnieżdżone strony wzorcowe w czasie projektowania. Dobre wieści jest, że program Visual Studio 2008 oferuje zaawansowane środowisko czasu projektowania dla zagnieżdżone strony wzorcowe. Jeśli chcesz użyć zagnieżdżone strony wzorcowe, ale nadal przy użyciu programu Visual Studio 2005, zapoznaj się [Scott Guthrie](https://weblogs.asp.net/scottgu/)jego wpis w blogu [porady dotyczące zagnieżdżone strony wzorcowe w czasie projektowania 2005 VS](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Zalety zagnieżdżone strony wzorcowe

Wiele witryn sieci Web ma lokacja nadrzędna, a także bardziej dostosowanego projektów specyficzne dla określonych rodzajów stron. Na przykład w naszej aplikacji sieci web pokaz utworzono podstawowe sekcji Administracja (strony `~/Admin` folderu). Obecnie stron sieci web w `~/Admin` folderu używać tej samej strony wzorcowej na tych stronach nie w sekcji Administracja (to znaczy, `Site.master` lub `Alternate.master`, w zależności od wybranej przez użytkownika).

> [!NOTE]
> Na razie podawać czy naszej witrynie ma tylko jedną stronę wzorcową `Site.master`. Będzie można rozwiązać przy użyciu zagnieżdżone strony wzorcowe z dwóch (lub więcej) strony wzorcowe, począwszy od "Przy użyciu wzorca strony dla administracji sekcji zagnieżdżonych" w dalszej części tego samouczka.


Załóżmy, że firma Microsoft zostały monit dostosować układ strony administracji do zawierają dodatkowe informacje i linki, które nie w przeciwnym razie jest obecny w innych stron w witrynie. Istnieją cztery technik w celu wykonania tego wymagania:

1. Ręcznie Dodaj informacje specyficzne dla Administracja i łącza do każdej strony zawartości `~/Admin` folderu.
2. Aktualizacja `Site.master` strony wzorcowej do uwzględnienia informacje specyficzne dla sekcji Administracja i łącza, a następnie dodaj kod do strony głównej, aby pokazać lub ukryć następujące sekcje w oparciu czy jednej strony Administracja jest odwiedzana.
3. Tworzenie nowej strony wzorcowej specjalnie z myślą o sekcji Administracja kopiowane znaczników z `Site.master`, Dodaj informacje specyficzne dla sekcji Administracja i łącza, a następnie zaktualizuj strony zawartości w `~/Admin` folderu, aby użyć tej nowej wzorca Strona.
4. Tworzenie osadzonej stronie głównej, która jest powiązana z `Site.master` i zawartości stron `~/Admin` użycie folderu nowy zagnieżdżone strony wzorcowej. Ta osadzonej stronie głównej to po prostu dodatkowe informacje i linki specyficzne dla stron administracyjnych i nie jest wymagane Powtórz znacznika już zdefiniowana w `Site.master`.

Pierwsza opcja jest najmniej palatable. Całego miejsca przy użyciu stron wzorcowych jest przenieść poza konieczności ręcznie skopiuj i Wklej markup wspólnego do nowych stron ASP.NET. Drugą opcją jest dopuszczalna, ale sprawia, że aplikacja jest mniej łatwy w obsłudze, zgodnie z jego bulks się stron wzorcowych z kod znaczników, który jest wyświetlany tylko od czasu do czasu i wymaga deweloperzy edycji strony głównej w celu obejścia tego znacznika i do zapamiętania, kiedy dokładnie i jest wyświetlany niektórych znaczników, gdy jest on ukryty. Ta metoda jest mniej firmy tenable jako dostosowania z coraz więcej typów stron sieci web, konieczne są spełnione przez ten pojedynczej strony wzorcowej.

Trzecia opcja powoduje usunięcie niepotrzebnych i złożoność wystawia surfaced z drugą opcją. Jednak opcja trzy przez główną wadą jest wymaganie firmie Microsoft w celu kopiowania i wklejania wspólnej układu z `Site.master` do nowej strony wzorcowej specyficzne dla sekcji administracji. Jeśli zdecydujesz się firma Microsoft później zmienić układ całej lokacji, musimy Pamiętaj, aby go zmienić w dwóch miejscach.

Opcji czwarty zagnieżdżone strony wzorcowe, Przekaż nam najlepsze opcje drugiego i trzeciego. Informacje o układzie całej lokacji jest obsługiwany w jednym pliku - najwyższego poziomu strony wzorcowej — podczas specyficzne dla poszczególnych regionów zawartości jest dzielony różne pliki.

W tym samouczku rozpoczyna się od spojrzeć na tworzenie i korzystanie z prostych osadzonej stronie głównej. Utworzymy całkowicie nowe strony wzorcowej najwyższego poziomu, dwa zagnieżdżone strony wzorcowe i dwie strony z zawartością. Począwszy od przy użyciu zagnieżdżone wzorca strony dla administracji, zobacz sekcję"przyjrzymy się aktualizowanie obejmują użycie zagnieżdżone strony wzorcowe naszych istniejącej architektury strony wzorcowej. W szczególności, możemy utworzyć osadzonej stronie głównej i przy jego użyciu obejmować dodatkową zawartość niestandardowe dla stron zawartości w `~/Admin` folderu.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Krok 1: Tworzenie prostego najwyższego poziomu strony wzorcowej

Tworzenie master zagnieżdżone na podstawie jednej z istniejącego wzorca stron i następnie aktualizacja istniejącej zawartości strony, aby używać tej nowej strony wzorcowej zagnieżdżonych zamiast najwyższego poziomu strony wzorcowej pociąga za sobą niektórych złożoności, ponieważ niektóre oczekuje już istniejących stron zawartości Formanty ContentPlaceHolder zdefiniowane na stronie głównej najwyższego poziomu. W związku z tym osadzonej stronie głównej muszą także tych samych kontroli ContentPlaceHolder pod tą samą nazwą. Ponadto naszej aplikacji demonstracyjnej określonego ma dwie strony wzorcowej (`Site.master` i `Alternate.master`) dynamicznie przypisane do strony zawartości zgodnie z preferencjami użytkownika, którego dalsze dodaje do tego stopnia złożoności. Zajmiemy aktualizowania istniejących aplikacji do użycia w dalszej części tego samouczka zagnieżdżone strony wzorcowe, ale umożliwia pierwszy fokus na prostej zagnieżdżone strony wzorcowe przykład.

Utwórz nowy folder o nazwie `NestedMasterPages` , a następnie dodaj nowy plik strony głównej do tego folderu o nazwie `Simple.master`. (Zobacz rysunek 1 dla zrzut ekranu Eksploratora rozwiązań po dodaniu tego folderu i pliku). Przeciągnij `AlternateStyles.css` pliku arkusza stylów z Eksploratora rozwiązań do projektanta. Spowoduje to dodanie `<link>` elementu do pliku arkusza stylów w `<head>` elementu, po upływie którego strony wzorcowej `<head>` znacznika elementu powinien wyglądać następująco:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Następnie dodaj następujący kod w formularzu sieci Web `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Ten kod znaczników zawiera łącze zatytułowany "Zagnieżdżone strony wzorcowe (proste)" w górnej części strony z użyciem dużej czcionki białe granatowym tła. Pod który jest `MainContent` ContentPlaceHolder. Rysunek 1 pokazuje `Simple.master` strony głównej po załadowaniu w projektancie programu Visual Studio.


[![Zagnieżdżone strony wzorcowej definiuje określonej zawartości do stron w sekcji Administracja](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Rysunek 01**: zagnieżdżone wzorca strony definiuje zawartości specyficzne dla stron w sekcji Administracja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Krok 2: Tworzenie prostego zagnieżdżone strony wzorcowej

`Simple.master`zawiera dwa formanty ContentPlaceHolder: `MainContent` ContentPlaceHolder dodaliśmy w formularzu sieci Web, wraz z `head` elementu ContentPlaceHolder na `<head>` elementu. Czy możemy do tworzenia strony zawartości, który należy powiązać `Simple.master` musi dwóch formantów zawartości odwołujące się do dwóch Elementy ContentPlaceHolders strony zawartość. Podobnie możemy utworzyć osadzonej stronie głównej i powiązać `Simple.master` osadzonej stronie głównej będzie mieć dwa formanty zawartości.

Możemy dodać do nowej strony wzorcowej zagnieżdżonych `NestedMasterPages` folder o nazwie `SimpleNested.master`. Kliknij prawym przyciskiem myszy `NestedMasterPages` folder i wybierz polecenie Dodaj nowy element. Spowoduje to wyświetlenie okna dialogowego Dodaj nowy element pokazany na rysunku 2. Wybierz typ szablonu strony wzorcowej i wpisz nazwę nowej strony wzorcowej. Aby wskazać, że nowe strony wzorcowej powinny być osadzonej stronie głównej, zaznacz pole wyboru "Wybierz strony głównej".

Następnie kliknij przycisk Dodaj. Spowoduje to wyświetlenie tego samego wybierz okno dialogowe strony wzorcowej, zostanie wyświetlony podczas wiązania strony zawartości strony wzorcowej (patrz rysunek 3). Wybierz `Simple.master` strony wzorcowej `NestedMasterPages` folder i kliknij przycisk OK.

> [!NOTE]
> Jeśli utworzono witryny sieci Web ASP.NET przy użyciu modelu projektu aplikacji sieci Web zamiast modelu projekt witryny sieci Web nie będą widzieć pole wyboru "Wybierz strony wzorcowej" w oknie dialogowym Dodawanie nowego elementu pokazany na rysunku 2. Można utworzyć osadzonej stronie głównej przy użyciu modelu projektu aplikacji sieci Web, musisz wybrać szablon zagnieżdżony strony wzorcowej (zamiast szablonu strony wzorcowej). Po wybór szablonu zagnieżdżone strony wzorcowej i klikając przycisk Dodaj, wybierz taki sam strony wzorcowej, zostanie wyświetlone okno dialogowe pokazany na rysunku 3.


[![Sprawdź &quot;wybierz strony wzorcowej&quot; pole wyboru, aby dodać zagnieżdżone strony wzorcowej](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Rysunek 02**: zaznacz pole wyboru "Wybierz strony wzorcowej", aby dodać zagnieżdżone strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image6.png))


[![Strona wzorcowa Simple.master powiązać zagnieżdżone strony wzorcowej](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Rysunek 03**: strona wzorcowa zagnieżdżone, aby powiązać `Simple.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image9.png))


Osadzonej stronie głównej deklaratywne znaczników, pokazano poniżej, zawiera dwa formanty zawartości odwołujące się do dwóch formantów elementu ContentPlaceHolder najwyższego poziomu strony wzorcowej.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Z wyjątkiem `<%@ Master %>` dyrektywy, osadzonej stronie głównej początkowej deklaratywne znaczników jest identyczna jak znaczników, generowany podczas wiązania strony zawartości z tej samej strony wzorcowej najwyższego poziomu. Strony zawartości, takich jak `<%@ Page %>` dyrektywy `<%@ Master %>` tutaj dyrektywa zawiera `MasterPageFile` atrybut określający, strony głównej nadrzędnej osadzonej stronie głównej. Podstawowa różnica między osadzonej stronie głównej i strony zawartości, która jest powiązana z tej samej strony wzorcowej najwyższego poziomu jest, że osadzonej stronie głównej może zawierać elementu ContentPlaceHolder kontrolki. Formanty ContentPlaceHolder osadzonej stronie głównej zdefiniuj regionach, gdzie strony zawartości można dostosować znaczników.

Zaktualizowanie tego osadzonej stronie głównej, tak aby zawiera tekst "Hello, z SimpleNested!" w formancie zawartości, która odpowiada `MainContent` ContentPlaceHolder formantu.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Po wprowadzeniu to dodawanie, zapisać osadzonej stronie głównej, a następnie dodaj nową stronę zawartości do `NestedMasterPages` folder o nazwie `Default.aspx`oraz powiązać `SimpleNested.master` strony wzorcowej. Podczas dodawania tej strony może być zaskoczeniem zobaczyć, czy zawiera on żadnych formantów zawartości (patrz rysunek 4)! Zawartość strony ma dostęp tylko do jego *nadrzędnej* Elementy ContentPlaceHolders strony wzorcowej. `SimpleNested.master`nie zawiera żadnych formantów elementu ContentPlaceHolder; w związku z tym wszystkie strony zawartości, powiązane z tej strony wzorcowej nie może zawierać wszystkie formanty zawartości.


[![Nowa strona zawartości zawiera nie formanty zawartości](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Rysunek 04**: nowej zawartości strony zawiera nr formantów zawartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image12.png))


Co należy zrobić to Aktualizuj osadzonej stronie głównej (`SimpleNested.master`) do uwzględnienia ContentPlaceHolder formantów. Zazwyczaj należy zagnieżdżone strony wzorcowe uwzględnienie ContentPlaceHolder dla każdego elementu ContentPlaceHolder wynika z jego nadrzędny strony wzorcowej, umożliwiając jego podrzędnych strony wzorcowej lub zawartości strony do pracy z dowolnego elementu ContentPlaceHolder najwyższego poziomu strony wzorcowej formanty.

Aktualizacja `SimpleNested.master` strony wzorcowej do dołączenia elementu ContentPlaceHolder jego dwóch formantów zawartości. Nadaj kontrolki elementu ContentPlaceHolder taką samą nazwę jak ich formantu zawartości odwołuje się do formantu elementu ContentPlaceHolder. Oznacza to, Dodaj formant ContentPlaceHolder o nazwie `MainContent` zawartości formantu w `SimpleNested.master` , która odwołuje się `MainContent` elementu ContentPlaceHolder na `Simple.master`. Tak samo postąpić w formancie zawartości, który odwołuje się do `head` ContentPlaceHolder.

> [!NOTE]
> Gdy najlepiej nazewnictwa kontrolki elementu ContentPlaceHolder na stronie głównej zagnieżdżonych taka sama jak elementy ContentPlaceHolders na stronie głównej najwyższego poziomu, to nazewnictwa symetrycznego nie jest wymagane. Możesz zapewnić kontrolki elementu ContentPlaceHolder na stronie głównej zagnieżdżonych dowolną nazwę, którą chcesz. Jednak I łatwiejsze do zapamiętania Elementy ContentPlaceHolders odpowiada z jakich regionach strony, jeśli mojego najwyższego poziomu strony wzorcowej i zagnieżdżone strony wzorcowe używają tych samych nazw.


Po wprowadzeniu tych dodatków programu `SimpleNested.master` deklaratywne znaczników strony wzorcowej powinien wyglądać podobnie do następującego:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Usuń `Default.aspx` strona właśnie utworzyliśmy zawartości i następnie dodaj go ponownie, powiązanie go do `SimpleNested.master` strony wzorcowej. Teraz program Visual Studio dodaje dwa formanty zawartości `Default.aspx`, odwołuje się do Elementy ContentPlaceHolders teraz zdefiniowany w `SimpleNested.master` (patrz rysunek 6). Dodaj tekst "Hello, z Default.aspx!" w zawartości sterować tym, do którego odwołanie `MainContent`.

Rysunek 5. pokazuje trzy jednostki zaangażowane w tym miejscu - `Simple.master`, `SimpleNested.master`, i `Default.aspx` — i w jaki sposób są powiązane ze sobą. Jak pokazano na diagramie, osadzonej stronie głównej implementuje formantów zawartości dla elementu ContentPlaceHolder jego elementu nadrzędnego. Jeśli tych regionów muszą być dostępne na stronie zawartości, osadzonej stronie głównej należy dodać własne elementy ContentPlaceHolders z formantami zawartości.


[![Strony wzorcowe najwyższego poziomu i zagnieżdżonych dyktowania układ zawartości strony](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Rysunek 05**: układ strony zawartości jest wymagane przez najwyższego poziomu i zagnieżdżone strony wzorcowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image15.png))


To zachowanie przedstawiono, jak strony zawartości lub strony wzorcowej jest tylko świadome jego nadrzędny strony wzorcowej. To zachowanie jest także określane przez projektanta programu Visual Studio. Rysunek 6 przedstawia Projektant `Default.aspx`. Gdy Projektant jasno wskazuje, które regiony są edytowalne ze strony zawartość i nie są fragmenty, nie usunąć niejednoznaczność, nie można edytować regiony są z osadzonej stronie głównej i regiony są na stronie głównej najwyższego poziomu.


[![Obecnie strony zawartości zawiera formanty zawartości dla strony wzorcowej zagnieżdżone elementy ContentPlaceHolders](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Rysunek 06**: zawartości strony teraz zawiera formanty zawartości dla Elementy ContentPlaceHolders zagnieżdżone wzorca strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Krok 3: Dodawanie drugiego proste zagnieżdżone strony wzorcowej

Zaletą zagnieżdżone strony wzorcowe jest bardziej widoczny, gdy istnieje wiele zagnieżdżone strony wzorcowe. Aby zilustrować ten korzyści, należy utworzyć inny osadzonej stronie głównej w `NestedMasterPages` folderu; nazwa nowy zagnieżdżone strony wzorcowej `SimpleNestedAlternate.master` , który należy powiązać `Simple.master` strony wzorcowej. Dodaj formanty ContentPlaceHolder w dwóch formantów zawartości osadzonej stronie głównej jak robiliśmy w kroku 2. Ponadto Dodaj tekst "Hello, z SimpleNestedAlternate!" w zawartości formantu, który odpowiada najwyższego poziomu strony wzorcowej `MainContent` ContentPlaceHolder. Po wprowadzeniu tych zmian nowej zagnieżdżone strony wzorcowej dla znaczników deklaratywne powinien wyglądać podobny do następującego:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Utwórz stronę zawartości o nazwie `Alternate.aspx` w `NestedMasterPages` folderu, który należy powiązać `SimpleNestedAlternate.master` osadzonej stronie głównej. Dodaj tekst "Hello, z alternatywnym!" w formancie zawartości, która odpowiada `MainContent`. Rysunek nr 7 przedstawia `Alternate.aspx` oglądany przez projektanta programu Visual Studio.


[![Alternate.aspx jest powiązany z strony wzorcowej SimpleNestedAlternate.master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Rysunek 07**: `Alternate.aspx` jest powiązany z `SimpleNestedAlternate.master` strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image21.png))


Porównaj projektanta na rysunku 7 do projektanta na rysunku 6. Obie strony z zawartością udziału ten sam układ zdefiniowane na stronie głównej najwyższego poziomu (`Simple.master`), czyli tytuł "Zagnieżdżone wzorca stron samouczek (proste)". Jeszcze mają różne zawartości zdefiniowanej w ich nadrzędnej strony wzorcowe - tekst "Hello, z SimpleNested!" Rysunek 6 i "Hello, z SimpleNestedAlternate!" na rysunku 7. Udzieleniu tych różnic w tym miejscu są trivial, ale można rozszerzyć w tym przykładzie, aby uwzględnić bardziej znaczące różnice. Na przykład `SimpleNested.master` strony może obejmować menu z opcjami określone na stronach zawartości, podczas gdy `SimpleNestedAlternate.master` może być informacje dotyczące strony zawartości, które wiążą się do niego.

Teraz załóżmy, że niezbędna do zmiany nadrzędna układu witryny. Załóżmy, że trzeba dodać listę typowych łączy do wszystkich stron zawartości. W tym modyfikacjom najwyższego poziomu strony wzorcowej `Simple.master`. Zmiany są natychmiast odzwierciedlane w jego zagnieżdżone strony wzorcowe i przez rozszerzenie, ich strony z zawartością.

Aby zademonstrować łatwość, z którym możemy zmienić układ lokacja nadrzędna, otwórz `Simple.master` strony wzorcowej i Dodaj następujący kod między `topContent` i `mainContent` `<div>` elementy:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

To dodaje dwa łącza na początku każdej strony, która jest powiązana z `Simple.master`, `SimpleNested.master`, lub `SimpleNestedAlternate.master`; te zmiany dotyczą wszystkich zagnieżdżonych stron wzorcowych i stron zawartości natychmiast. Rysunek nr 8 przedstawia `Alternate.aspx` podczas wyświetlania za pośrednictwem przeglądarki. Należy pamiętać, dodawania łączy w górnej części strony (w porównaniu z rysunku 7).


[![Zmieniony na najwyższym poziomie strony wzorcowej są natychmiast odzwierciedlone w jego zagnieżdżone strony wzorcowe i ich zawartości strony](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Rysunek 08**: zmieniony na najwyższym poziomie strony wzorcowej są natychmiast odzwierciedlone w jego zagnieżdżone strony wzorcowe i ich strony z zawartością ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Za pomocą zagnieżdżone strony wzorcowej dla sekcji Administracja

Na tym etapie nasz sprawdzono zalet master zagnieżdżone strony i ma przedstawiono sposób tworzenia i używania ich w aplikacji ASP.NET. Przykłady w krokach 1, 2 i 3, jednak obejmuje tworzenie nowej strony wzorcowej najwyższego poziomu, nowe zagnieżdżone strony wzorcowe i nowych stron zawartości. Jak dodawanie nowego osadzonej stronie głównej witryny sieci Web z istniejącej najwyższego poziomu strony wzorcowej oraz strony z zawartością?

Integrowanie osadzonej stronie głównej istniejącej witryny sieci Web i kojarzenie go z istniejących stron zawartości wymaga nieco więcej wysiłku niż od podstaw. Kroki 4, 5, 6 i 7 Eksploruj te problemy, jak firma Microsoft rozszerzyć naszej aplikacji demonstracyjnej, aby uwzględnić nowe osadzonej stronie głównej o nazwie `AdminNested.master` który zawiera instrukcje dla administratora i jest używany przez strony ASP.NET `~/Admin` folderu.

Integrowanie osadzonej stronie głównej aplikacji demonstracyjnej wprowadza następujące przeszkód:

- Istniejąca zawartość strony w `~/Admin` folder ma niektórych oczekiwań ze strony wzorcowej. Po pierwsze oczekiwane niektórych formantów elementu ContentPlaceHolder obecności. Ponadto `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` strony wywoływać publicznego strony wzorcowej `RefreshRecentProductsGrid` metody, ustaw jej `GridMessageText` właściwości, lub program obsługi zdarzeń dla jego `PricesDoubled` zdarzeń. W rezultacie naszych osadzonej stronie głównej podać tego samego Elementy ContentPlaceHolders i publiczne elementy członkowskie.
- W poprzednim samouczek firma Microsoft enhanced `BasePage` klasę, aby ustawić dynamicznie `Page` obiektu `MasterPageFile` na podstawie właściwości w zmiennej sesji. Jak do obsługujemy dynamiczne strony głównej przy użyciu zagnieżdżone strony wzorcowe?

Te dwa problemy będą powierzchni jak możemy utworzyć osadzonej stronie głównej i używać go z istniejących stron zawartości. Firma Microsoft będzie badanie i rozwiązanie tych problemów, zanim jeszcze wystąpią.

## <a name="step-4-creating-the-nested-master-page"></a>Krok 4: Tworzenie zagnieżdżone strony wzorcowej

Naszym pierwszym zadaniem jest można utworzyć osadzonej stronie głównej używanego przez stron w sekcji administracji. Jako widzieliśmy w kroku 2, podczas dodawania nowego zagnieżdżone strony wzorcowej należy określić strony wzorcowej nadrzędnego osadzonej stronie głównej. Ale mamy dwie strony wzorcowej najwyższego poziomu: `Site.master` i `Alternate.master`. Odwołania, który utworzyliśmy `Alternate.master` w poprzednim samouczek i zapisane w kodzie `BasePage` klasy zestawu `Page` obiektu `MasterPageFile` właściwości w czasie wykonywania jednej `Site.master` lub `Alternate.master` w zależności od wartości `MyMasterPage` Zmienna sesji.

W jaki sposób możemy skonfigurować naszych osadzonej stronie głównej, tak aby były używane odpowiednie najwyższego poziomu strony wzorcowej? Firma Microsoft są dostępne dwie opcje:

- Utwórz dwa zagnieżdżone strony wzorcowe, `AdminNestedSite.master` i `AdminNestedAlternate.master`oraz powiązać najwyższego poziomu strony wzorcowe `Site.master` i `Alternate.master`odpowiednio. W `BasePage`, następnie ustawimy usługę· `Page` obiektu `MasterPageFile` odpowiednie zagnieżdżone strony wzorcowej.
- Utwórz pojedynczy osadzonej stronie głównej i mieć strony zawartości, użyj tej konkretnej strony wzorcowej. Następnie, w czasie wykonywania, czy musimy ustawić osadzonej stronie głównej `MasterPageFile` właściwości do odpowiedniej strony wzorcowej najwyższego poziomu w czasie wykonywania. (Użytkownik może znalezienia przez teraz, stron wzorcowych również mają `MasterPageFile` właściwości.)

Użyjmy drugiej opcji. Utwórz pojedynczy zagnieżdżonych pliku strony głównej w `~/Admin` folder o nazwie `AdminNested.master`. Ponieważ oba `Site.master` i `Alternate.master` mają ten sam zestaw kontrolek elementu ContentPlaceHolder, nie ma znaczenia, jakie strony wzorcowej możesz powiązać ją, chociaż I zachęca do powiązać `Site.master` dla sake dla spójności.


[![Dodaj stronę wzorcową zagnieżdżonych do folderu ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Rysunek 09**: Dodaj stronę wzorcową zagnieżdżonych do `~/Admin` folderu. ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image27.png))


Ponieważ osadzonej stronie głównej jest powiązany z strony wzorcowej z cztery kontrolki elementu ContentPlaceHolder, Visual Studio dodaje cztery formanty do nowego pliku osadzonej stronie głównej znacznika początkowego zawartości. Jak robiliśmy kroki 2 i 3, Dodaj formant ContentPlaceHolder w każdej zawartości kontrolki nadanie mu taką samą nazwę jak kontrolki elementu ContentPlaceHolder najwyższego poziomu strony wzorcowej. Również Dodaj następujący kod do zawartości formantu, który odpowiada `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Następnie określ `instructions` klasy CSS w `Styles.css` i `AlternateStyles.css` pliki CSS. Następujące reguły CSS spowodować elementów HTML stylem `instructions` klasy, które mają być wyświetlane z jasny żółty kolor tła i obramowania czarny, stałe:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Ponieważ tego znacznika został dodany do osadzonej stronie głównej, zostanie wyświetlony tylko na tych stron, które używają osadzonej stronie głównej (to znaczy, strony w sekcji Administracja).

Po wprowadzeniu tych dodatków do strony głównej zagnieżdżonych, jego deklaratywne znaczników powinien wyglądać podobny do następującego:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Należy zauważyć, że każdy formant zawartości ma formantu elementu ContentPlaceHolder oraz że formantów elementu ContentPlaceHolder `ID` właściwości są przypisane takie same wartości jak odpowiednie kontrolki elementu ContentPlaceHolder na stronie głównej najwyższego poziomu. Ponadto znaczników specyficzne dla sekcji Administracja zostanie wyświetlona w `MainContent` ContentPlaceHolder.

Rysunek nr 10 przedstawia `AdminNested.master` osadzonej stronie głównej oglądany przez projektanta programu Visual Studio. Można zapoznaj się z instrukcjami w polu żółty w górnej części `MainContent` zawartości formantu.


[![Zagnieżdżone strony wzorcowej rozszerza strony wzorcowej najwyższego poziomu, aby dołączyć instrukcje dla administratora.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Na rysunku nr 10**: zagnieżdżone strony wzorcowej rozszerza strony wzorcowej najwyższego poziomu, aby dołączyć instrukcje dla administratora. ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Krok 5: Aktualizowanie istniejących stron zawartości do użycia nowego zagnieżdżone strony wzorcowej

W dowolnym momencie dodania nowej strony zawartości do sekcji Administracja należy powiązać `AdminNested.master` właśnie utworzyliśmy strony wzorcowej. Ale co istniejącej zawartości strony? Obecnie wszystkie zawartości strony w witrynie pochodzi od `BasePage` klasy, która programowo ustawia strony wzorcowej zawartości strony w czasie wykonywania. To nie jest zachowanie interesujące dla stron zawartości w sekcji administracji. Zamiast tego chcemy te strony zawartości, aby zawsze była używana `AdminNested.master` strony. Będzie ona odpowiedzialność osadzonej stronie głównej wybierz prawo najwyższego poziomu strony zawartości w czasie wykonywania.

Aby najlepszy sposób, aby osiągnąć to konieczne, zachowanie jest utworzenie nowej klasy niestandardowej strony podstawowej o nazwie `AdminBasePage` który rozszerza `BasePage` klasy. `AdminBasePage`następnie można zastąpić `SetMasterPageFile` i ustaw `Page` obiektu `MasterPageFile` wartość ustalony "~ / Admin/AdminNested.master". W ten sposób dowolną stronę która pochodzi z `AdminBasePage` użyje `AdminNested.master`, podczas gdy dowolną stronę, która pochodzi z `BasePage` będzie jego `MasterPageFile` właściwość ustawić dynamicznie "~ / Site.master" lub "~ / Alternate.master" na podstawie wartości z `MyMasterPage` Zmiennej sesji.

Rozpocznij od dodania nowego pliku klasy `App_Code` folder o nazwie `AdminBasePage.vb`. Ma `AdminBasePage` rozszerzyć `BasePage` , a następnie zastąpić `SetMasterPageFile` metody. W tej metodzie przypisać `MasterPageFile` wartość "~ / Admin/AdminNested.master". Po wprowadzeniu tych zmian na klasę pliku powinien wyglądać podobnie do poniższej:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Teraz musisz mieć istniejących stron zawartości w administracji sekcji pochodzi od `AdminBasePage` zamiast `BasePage`. Przejdź do pliku CodeBehind klasy dla każdej strony zawartości `~/Admin` folderu i wprowadzić tę zmianę. Na przykład w `~/Admin/Default.aspx` zmienia się z deklaracji klasy związane z kodem:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Do:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Rysunek 11 przedstawia sposób najwyższego poziomu strony wzorcowej (`Site.master` lub `Alternate.master`), osadzonej stronie głównej (`AdminNested.master`), i stron zawartości sekcji Administracja odnoszą się do siebie.


[![Zagnieżdżone strony wzorcowej definiuje określonej zawartości do stron w sekcji Administracja](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Rysunek 11**: zagnieżdżone wzorca strony definiuje zawartości specyficzne dla stron w sekcji Administracja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Krok 6: Dublowania publicznej metody i właściwości strony wzorcowej

Odwołania, który `~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stron interakcji programowej z strony wzorcowej: `~/Admin/AddProduct.aspx` wywołań strony wzorcowej do publicznego `RefreshRecentProductsGrid` — metoda i ustawia jej `GridMessageText` właściwości; `~/Admin/Products.aspx` ma obsługi zdarzeń dla `PricesDoubled` zdarzeń. W poprzednim samouczek utworzyliśmy `MustInherit` `BaseMasterPage` klasy, która zdefiniowana te publiczne elementy członkowskie.

`~/Admin/AddProduct.aspx` i `~/Admin/Products.aspx` stron założono, że ich strony wzorcowej pochodną `BaseMasterPage` klasy. `AdminNested.master` Strony, jednak obecnie rozszerza `System.Web.UI.MasterPage` klasy. Dzięki temu podczas odwiedzania `~/Admin/Products.aspx` `InvalidCastException` jest generowany komunikat o: "nie można rzutować obiektu typu" ASP.admin\_adminnested\_wzorca "na typ"BaseMasterPage"."

Aby naprawić to musimy mieć `AdminNested.master` rozszerzyć klasę kodu `BaseMasterPage`. Aktualizacja deklarację klasy związane z kodem osadzonej stronie głównej:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Do:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Nie skończymy jeszcze. Należy zastąpić elementy członkowskie oznaczone jako `MustOverride`, to znaczy `RefreshRecentProductsGrid` i `GridMessageText`. Elementy te są używane przez strony wzorcowe najwyższego poziomu można zaktualizować swoje interfejsy użytkownika. (W rzeczywistości tylko `Site.master` strony wzorcowej używa tych metod, mimo że obu stron wzorcowych najwyższego poziomu wdrożenia tych metod, ponieważ zarówno rozszerzyć `BaseMasterPage`.)

Gdy musimy zaimplementować te elementy członkowskie w `AdminNested.master`, wszystkie te implementacji konieczne jest po prostu wywołać elementu członkowskiego na stronie głównej najwyższego poziomu używane przez osadzonej stronie głównej. Na przykład gdy zawartości strony w sekcji Administracja wywołuje osadzonej stronie głównej `RefreshRecentProductsGrid` metody, wszystkie zagnieżdżone strony wzorcowej musi wykonać, z kolei, wywołaj `Site.master` lub `Alternate.master`w `RefreshRecentProductsGrid` metody.

Aby to osiągnąć, Rozpocznij od dodania następujących `@MasterType` dyrektywy na początku `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Odwołania, który `@MasterType` dyrektywy dodaje właściwość silnie typizowaną do klasy związane z kodem o nazwie `Master`. Następnie zastąp `RefreshRecentProductsGrid` i `GridMessageText` elementy członkowskie i po prostu delegowanie wywołanie `Master`i odpowiadający jej metodę:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Z tego kodu w miejscu należy odwiedzić i używać zawartości strony w sekcji Administracja. Przedstawia rysunek 12 `~/Admin/Products.aspx` strony podczas wyświetlania za pośrednictwem przeglądarki. Jak widać, strona zawiera pole instrukcje administracji, które jest zdefiniowany w osadzonej stronie głównej.


[![Strony zawartości, w sekcji Administracja instrukcje na początku każdej strony](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Rysunek 12**: strony z zawartością w administracji sekcji zawierają instrukcje na początku każdej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Krok 7: Przy użyciu odpowiedniej strony wzorcowej najwyższego poziomu w czasie wykonywania

Gdy wszystkie zawartości strony w sekcji Administracja są w pełni funkcjonalne, używają tej samej strony wzorcowej najwyższego poziomu i Ignoruj strony wzorcowej wybrane przez użytkownika w `ChooseMasterPage.aspx`. To zachowanie to z faktu, że ma osadzonej stronie głównej jego `MasterPageFile` statycznie ustawioną właściwość `Site.master` w jego `<%@ Master %>` dyrektywy.

Aby użyć najwyższego poziomu strony wzorcowej wybrane przez użytkownika końcowego, należy ustawić `AdminNested.master`w `MasterPageFile` właściwości do wartości w `MyMasterPage` zmiennej sesji. Ponieważ firma Microsoft strony zawartości `MasterPageFile` właściwości w `BasePage`, może uważasz, że ustawimy usługę· osadzonej stronie głównej `MasterPageFile` właściwości w `BaseMasterPage` lub `AdminNested.master`w klasie związanej z kodem. To nie będzie działać, jednak ponieważ musimy ustawiono `MasterPageFile` właściwości do końca etap PreInit. Najwcześniejsza czas, który firma Microsoft może programowo nacisnąć w cyklu życia strony ze strony wzorcowej jest na etapie Init, (realizowany po fazie PreInit).

W związku z tym należy ustawić osadzonej stronie głównej `MasterPageFile` właściwość ze strony z zawartością. Tylko zawartość strony używające `AdminNested.master` strony wzorcowej pochodzi od `AdminBasePage`. Firma Microsoft w związku z tym umieścić tę logikę. W kroku 5 możemy overrode `SetMasterPageFile` metody ustawienie obiekt strony `MasterPageFile` dla właściwości "~ / Admin/AdminNested.master". Aktualizacja `SetMasterPageFile` można również ustawić strony wzorcowej `MasterPageFile` właściwości do wyniku przechowywanego w sesji:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession` Metody, które dodano do `BasePage` klasy w poprzednim samouczek, zwraca ścieżkę pliku strony głównej odpowiednie oparta na wartości zmiennej sesji.

Dzięki tej zmianie w miejscu wybranej strony wzorcowej przez użytkownika są przenoszone do sekcji Administracja. Rysunek 13 przedstawiono tej samej stronie co rysunek 12, ale po zmianie ich zaznaczenie strony wzorcowej do użytkownika `Alternate.master`.


[![Strony Administracja zagnieżdżonych używa najwyższego poziomu strony wzorcowej wybrane przez użytkownika](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Rysunek 13**: zagnieżdżone strony Administracja używa najwyższego poziomu głównego strony wybranego przez użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Podsumowanie

Wiele podobnych jak zawartości strony można powiązać z strony wzorcowej, jest możliwość utworzenia zagnieżdżone strony wzorcowe przez posiadanie strony wzorcowej podrzędnych powiązać nadrzędny strony wzorcowej. Strony wzorcowej podrzędnych może zdefiniować formantów zawartości dla każdego z jego elementu nadrzędnego Elementy ContentPlaceHolders; go można następnie dodać własne kontrolki elementu ContentPlaceHolder (a także innych znaczników) do tych kontrolek zawartości. Zagnieżdżone strony wzorcowe są bardzo przydatne w aplikacji sieci web dużych, gdzie wszystkie strony Udostępnianie nadrzędna wyglądu i działania, ale określone sekcje lokacji wymagają unikatowych dostosowania.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zagnieżdżone strony wzorcowe ASP.NET](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Porady dotyczące zagnieżdżone strony wzorcowe i czasu projektowania 2005 VS](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Obsługa strony wzorca zagnieżdżone VS 2008](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](specifying-the-master-page-programmatically-vb.md)
