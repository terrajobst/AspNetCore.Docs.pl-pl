---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Ponowne używanie interfejsu użytkownika za pomocą stron wzorcowych i częściowych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 7 analizuje sposób możemy zastosować zasady susz w ramach naszych szablonów widok, aby wyeliminować zduplikowania kodu, przy użyciu szablonów widok częściowy i stron wzorcowych.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 961378d6d710c678a0cd223a5c31544f014ace7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839597"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Ponowne używanie interfejsu użytkownika za pomocą stron wzorcowych i częściowych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 7 bezpłatne [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 7 analizuje sposób możemy zastosować "Zasadą susz" w ramach naszych szablonów widok, aby wyeliminować zduplikowania kodu, przy użyciu szablonów widok częściowy i stron wzorcowych.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner krok 7: Częściowe i stronami wzorcowymi

Jeden filozofię projekt, który korzysta z platformy ASP.NET MVC jest zasada "Czy nie Powtórz samodzielnie" (powszechnie znane jako "Suchych"). Projekt susz, pomaga wyeliminować sytuację duplikowania kodu i logiki, co ostatecznie sprawia, że aplikacje szybciej tworzyć i łatwiejsze w utrzymaniu.

Widzieliśmy już susz zasady stosowane w kilku scenariuszy NerdDinner. Kilka przykładów: naszych logikę weryfikacji jest zaimplementowana w naszej warstwie modelu umożliwia wymuszane dla obu edytowanie i tworzenie scenariuszy w kontrolera; ponownie używamy "NotFound" Wyświetl szablon różnych metod akcji Edytuj, szczegóły i Delete; użyto konwencji nazewnictwa wzorzec za pomocą naszych szablonów widoku, które eliminuje potrzebę jawnie określić nazwę, jeśli możemy wywołać metody pomocnika View(); i możemy ponowne wykorzystanie klasy DinnerFormViewModel do obu edycji i tworzenia akcji scenariuszy.

Teraz Spójrzmy na sposób możemy zastosować "Zasadą susz" w ramach naszych szablonów widok, aby wyeliminować zduplikowania kodu, ma także.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Ponownego odwiedzania naszej edycji i tworzenie szablonów widoków

Obecnie używamy dwa szablony inny widok — "Edit.aspx" i "Create.aspx" — do wyświetlania formularza obiad interfejsu użytkownika. Szybkie visual porównania ich wyróżnia, podobnie jak są one. Poniżej znajduje się formularz Utwórz wygląda następująco:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

A Oto formularza "Edit" wygląda następująco:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Nie ma wiele różnic jest? Inne niż tekst tytułu i nagłówek kontrolek formularza układ i dane wejściowe są identyczne.

Otwieramy "Edit.aspx" i "Create.aspx" Przeglądanie szablonów, które firma Microsoft znajdziesz, które zawierają kod sterowania układ i dane wejściowe formularza identyczne. Ta duplikacja oznacza, że firma Microsoft znajdą się konieczności wprowadzania zmian, dwa razy w dowolnym możemy wprowadzić lub zmienić nową właściwość obiad — który nie jest dobra.

### <a name="using-partial-view-templates"></a>Za pomocą szablonów widoku częściowego

ASP.NET MVC obsługuje możliwość definiowania szablonów "widoku częściowego", które może być użyty do hermetyzacji logiki renderowania widoku podrzędnego części strony. "Częściowych" wygodny sposób, aby zdefiniować logikę do renderowania widoku raz, a następnie użyć go ponownie w wielu miejscach w aplikacji.

Aby ułatwić "PRÓBNEGO w górę" nasze Edit.aspx i Duplikowanie szablonu widoku Create.aspx, możemy utworzyć szablon widoku częściowego o nazwie "DinnerForm.ascx", który hermetyzuje układu formularza i elementów wejściowych, które są wspólne dla obu. Możemy to zrobić, klikając prawym przyciskiem myszy na naszych/widoków lub kolacji katalogu i wybierając pozycję "Add -&gt;widok" polecenie menu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Spowoduje to wyświetlenie okna dialogowego "Dodaj widok". Firma Microsoft będzie nazwa nowy widok, firma Microsoft do utworzenia "DinnerForm", zaznacz pole wyboru "Utwórz widok częściowy", w oknie dialogowym i wskazuje, czy będzie przekazywany jej klasę DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Kliknięcie przycisku "Dodaj", programu Visual Studio Utwórz nowy szablon widoku "DinnerForm.ascx" dla nas w katalogu "\Views\Dinners".

Firma Microsoft może następnie kopiowania/wklejania układu formularza zduplikowane / wprowadzić kod sterowania z naszych szablonów widoku Edit.aspx/ Create.aspx do naszego nowego szablonu widoku częściowego "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Firma Microsoft jest następnie zaktualizuj nasze szablony widoku Edit and Create wywołać DinnerForm częściowy szablon i wyeliminować duplikaty, w formularzu. Możemy to zrobić przez wywołującego Html.RenderPartial("DinnerForm") w ramach naszych szablonów widoku:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Można jawnie kwalifikujesz się do ścieżki częściowy szablon chcesz, aby podczas wywoływania Html.RenderPartial (na przykład: ~ Views/Dinners/DinnerForm.ascx "). W naszym kodzie powyżej, jesteśmy zalet oparty na konwencji nazewnictwa wzorcu w ramach platformy ASP.NET MVC i po prostu określenie "DinnerForm" jako nazwę częściowego do renderowania. W takim przypadku to platformy ASP.NET MVC będzie szukać pierwszy w katalogu views oparty na Konwencji (będzie to/widoków/kolacji DinnersController). Jeśli nie znajdzie częściowy szablon ma ona będzie szukać go w katalogu /Views/Shared.

Gdy Html.RenderPartial() jest wywoływana za pomocą tylko nazwę widoku częściowego, platformy ASP.NET MVC zostanie przekazany do widoku częściowego do tego samego modelu ViewData słownika obiektów i używane przez wywołującego szablon widoku. Alternatywnie są przeciążone wersje Html.RenderPartial() umożliwiające przekazać alternatywne obiekt modelu i/lub słownika ViewData widoku częściowego do użycia. Jest to przydatne w scenariuszach, gdzie tylko chcesz przekazać podzestaw pełny Model/ViewModel.

| **Temat po stronie: Dlaczego &lt;%%&gt; zamiast &lt;% = %&gt;?** |
| --- |
| Jedną z rzeczy subtelne, być może Zauważyłeś, za pomocą powyższego kodu jest, że &lt;%%&gt; block zamiast &lt;% = %&gt; blokowania podczas wywoływania Html.RenderPartial(). &lt;% = %&gt; bloków na platformie ASP.NET wskazują, że deweloper chce renderowania określona wartość (na przykład: &lt;% = "Hello" %&gt; będzie renderować "Hello"). &lt;%%&gt; bloki zamiast tego wskazują, że deweloper chce wykonać kod, i że dowolne renderowania danych wyjściowych w ramach ich musi odbywać się jawnie (na przykład: &lt;Response.Write("Hello") %&gt;. Dlatego używamy &lt;%%&gt; jest blok dzięki naszemu kodowi Html.RenderPartial powyżej, ponieważ metoda Html.RenderPartial() nie zwraca ciąg, a zamiast tego Wyświetla zawartość bezpośrednio do wywoływania szablon widoku danych wyjściowych strumienia. Robi to wydajność ze względu na wydajność i wykonując, aby uniknąć konieczności tworzenia obiektu string tymczasowego (potencjalnie bardzo duże). Zmniejsza użycie pamięci i zwiększa przepływność cała aplikacja. Jeden powszechnym podczas używania Html.RenderPartial() jest dodanie średnika na końcu wywołania, gdy znajduje się w &lt;%%&gt; bloku. Na przykład, ten kod powoduje błąd kompilatora: &lt;Html.RenderPartial("DinnerForm") %&gt; zamiast tego trzeba było pisać: &lt;% Html.RenderPartial("DinnerForm"); %&gt; jest to spowodowane &lt;%%&gt; bloki są instrukcje kodu niezależna i korzystając z instrukcji, które muszą być zakończone znakiem średnika kodu C#. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Za pomocą szablonów widoku częściowego, aby wyjaśnić, kod

Utworzyliśmy szablon widoku częściowego "DinnerForm", aby uniknąć duplikowania logiki renderowania widoku w kilku miejscach. To jest Najczęstszym powodem tworzenia szablonów widoku częściowego.

Czasami nadal warto tworzyć widoki częściowe, nawet wtedy, gdy są one wywoływana tylko w jednym miejscu. Szablony widoku bardzo skomplikowane często może stać się znacznie łatwiejsze do odczytania podczas logikę renderowania widoku jest wyodrębniony i podzielona na partycje w jednym lub więcej również o nazwie częściowe szablonów.

Rozważmy na przykład poniżej fragment kodu z pliku Site.master w projekcie (które firma Microsoft będzie sprawdzane wkrótce). Kod jest względnie proste — można odczytać — częściowo, ponieważ logika do wyświetlenia logowania/wylogowania link u góry po prawej stronie ekranu jest hermetyzowane w części "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Zawsze, gdy okaże się, że pobieranie mylić próby zrozumieć kod html/znaczników w ramach szablonu widoku, należy wziąć pod uwagę, czy w takich sytuacjach przydałaby się bardziej zrozumiałe, jeśli część z nich został wyodrębniony i zrefaktoryzowany do dobrze nazwane widoki częściowe.

### <a name="master-pages"></a>Strony wzorcowe

Oprócz obsługi widoków częściowych, ASP.NET MVC obsługuje również możliwość tworzenia szablonów "strony wzorcowej", które mogą służyć do definiowania układu typowe i html najwyższego poziomu lokacji. Symbol zastępczy, który następnie można dodać formanty do strony wzorcowej do identyfikowania regionów wymienne, które mogą być zastąpione, lub "wypełnione" przy użyciu funkcji widoków zawartości. Zapewnia to bardzo efektywny (i susz) sposób stosowania wspólnego układu dla aplikacji.

Domyślnie nowe projekty ASP.NET MVC ma szablonu strony wzorcowej automatycznie dodawane do nich. Ta strona wzorcowa nosi nazwę "Site.master" i życie w folderze \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Domyślny plik Site.master wygląda jak poniżej. Definiuje zewnętrzne html witryny, wraz z menu nawigacji u góry. Zawiera dwie kontrolki wymienne symbolu zastępczego zawartości — jeden dla tytułu i innych, dla której powinna zostać zastąpiona głównej zawartość strony:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Wszystkie szablony widoku, którą utworzyliśmy dla naszej aplikacji NerdDinner ("List", "Szczegóły", "Edytuj", "Utwórz", "NotFound" itp.) mają został oparty na tym szablonie Site.master. Jest to wskazywane przez atrybut "MasterPageFile", który został dodany domyślnie u góry &lt;% @ % strony&gt; dyrektywy wtedy stworzyliśmy naszych widoków, korzystając z okna dialogowego "Dodaj widok":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Oznacza to, że możemy zmienić zawartość Site.master i mają zmiany automatycznie stosowane i używany, gdy firma Microsoft renderowania jednego z naszych szablonów widoku.

Zaktualizujmy naszych Site.master nagłówka sekcji, aby nagłówek nasza aplikacja jest "NerdDinner" zamiast "Moja aplikacja MVC". Również zaktualizujmy nasze menu nawigacji, aby pierwsza karta jest "Znajdź obiad" (obsługiwane przez metodę akcji indeks() HomeController) i możemy dodać nową kartę o nazwie "Host obiad" (obsługiwane przez metodę akcji Create() DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Podczas oszczędzamy plik Site.master i odświeżania przeglądarki zobaczymy nasz nagłówek zmiany show we wszystkich widokach w ramach naszej aplikacji. Na przykład:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

I */Dinners/Edit / [id]* adresu URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Następny krok

Częściowe i stron wzorcowych zapewnia bardzo elastyczne opcje, które umożliwiają organizowanie nie pozostawia żadnych śladów widoków. Znajdziesz się, że pomagają uniknąć duplikowania widok zawartości / kodu i ułatwiają odczytywanie i Obsługa szablonów widoku.

Przejdźmy teraz ponownie scenariusza listy, który wcześniej został skompilowany i włączyć obsługę stronicowania skalowalne.

> [!div class="step-by-step"]
> [Poprzednie](use-viewdata-and-implement-viewmodel-classes.md)
> [dalej](implement-efficient-data-paging.md)
