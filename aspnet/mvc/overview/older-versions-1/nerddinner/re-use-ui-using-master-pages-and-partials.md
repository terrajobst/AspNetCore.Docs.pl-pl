---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: "Ponownie użyj interfejsu użytkownika przy użyciu stron wzorcowych i częściowe | Dokumentacja firmy Microsoft"
author: microsoft
description: "Krok 7 analizuje sposoby można zastosować zasady suchej w naszym szablony widoku wyeliminować zduplikowania kodu, za pomocą szablonów widok częściowy i stron wzorcowych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: c42cd6aca40b08a9f8461532fbfd0589901b64ad
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Ponownie użyj interfejsu użytkownika przy użyciu stron wzorcowych i częściowe
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 7 bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 7 analizuje sposoby stosujemy "Zasady suchej" w ramach naszych szablonów widoku, aby wyeliminować zduplikowania kodu, za pomocą szablonów widok częściowy i stron wzorcowych.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner krok 7: Częściowe i stron wzorcowych

Jedną z filozofiami projektu, który obejmuje ASP.NET MVC jest zasada "Czy nie powtarzaj samodzielnie" (często określana jako "Suchej"). Projekt suchej pomaga wyeliminować dublowanie kodu i logikę, dzięki czemu ostatecznie aplikacji szybsze tworzenie i łatwiejsze w obsłudze.

Firma Microsoft już przedstawiono suchej zasady stosowane w kilku scenariuszami NerdDinner. Kilka przykładów: logiki sprawdzania poprawności zostanie wdrożony w naszym warstwy modelu, która umożliwia być wymuszany dla obu edycji i tworzyć scenariusze w kontrolera; Szablon widoku "NotFound" ponownie jest używany przez edytowanie, szczegóły i usuwanie metody akcji. używamy konwencji nazewnictwa wzorca naszym szablony widoku, co eliminuje konieczność jawnie określić nazwę, gdy nazywamy metody pomocnika View(); i firma Microsoft ponowne wykorzystanie klasy DinnerFormViewModel dla obu edycji i tworzyć scenariusze akcji.

Teraz Przyjrzyjmy się sposoby stosujemy "Zasady suchej" w naszym szablony widok również wyeliminować zduplikowania kodu.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Ponownie odwiedzając naszych edycji i tworzenie widoku szablonów

Obecnie dwa szablony inny widok — "Edit.aspx" i "Create.aspx" — jest używany do wyświetlania naszego formularza obiad interfejsu użytkownika. Szybkie visual porównanie ich prezentuje są podobne jak. Poniżej znajduje się formularzu Utwórz wygląda następująco:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

A Oto nasze formularza "Edytuj" wygląda następująco:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Nie znacznie różnica jest? Inne niż tekst tytułu i nagłówek kontrolek formularza układ i dane wejściowe są identyczne.

Jeśli firma Microsoft otwarcia "Edit.aspx" i "Create.aspx" Wyświetl szablony, które będzie znaleźliśmy, które zawierają kod sterowania układ i dane wejściowe formularza identyczne. Tego duplikowania oznacza, że firma Microsoft przechodzili konieczności zmiany dwukrotnie przy każdym możemy wprowadzić lub zmienić nową właściwość obiad — nie jest dobra.

### <a name="using-partial-view-templates"></a>Za pomocą szablonów widok częściowy

ASP.NET MVC obsługuje możliwość definiowania szablonów "widoku częściowego", których można użyć w celu hermetyzacji logiki renderowania widoku podrzędnego części strony. "Częściowe" wygodny sposób na raz zdefiniować logiki renderowania widoku, a następnie użyć go ponownie w wielu miejscach w aplikacji.

Aby "Suchej up" naszych Edit.aspx i Duplikowanie szablonu widoku Create.aspx, można utworzyć szablon widoku częściowego o nazwie "DinnerForm.ascx", który hermetyzuje układu formularza i wspólne dla obu elementów wejściowych. Firma Microsoft będzie w tym celu naszych/widoków lub kolacji katalogu prawym przyciskiem myszy i wybierając polecenie "Add -&gt;widok" polecenie:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Spowoduje to wyświetlenie okna dialogowego "Dodaj widok". Firma Microsoft będzie nazwa nowego widoku możemy chcesz utworzyć "DinnerForm", zaznacz pole wyboru "Utwórz widok częściowy" w oknie dialogowym i wskazywać, że firma Microsoft będzie przekazywany klasy DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Kliknięcie przycisku "Dodaj", Visual Studio utworzy nowy szablon widoku "DinnerForm.ascx" firmie Microsoft w ramach katalogu "\Views\Dinners".

Firma Microsoft może następnie kopiowania/wklejania układu duplikacie / danych wejściowych kontroli kodu z naszym szablony widoku Edit.aspx/ Create.aspx naszego nowego szablonu widoku częściowego "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Firma Microsoft może następnie zaktualizuj naszych szablonów edycji i tworzenie widoku do wywołania szablon częściowej DinnerForm i eliminowania duplikatów formularza. Firma Microsoft można to zrobić przez wywołanie Html.RenderPartial("DinnerForm") w naszym szablony widoku:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Jawnie kwalifikują się ścieżka częściowa szablonu ma podczas wywoływania metody Html.RenderPartial (na przykład: ~ Views/Dinners/DinnerForm.ascx "). W naszym kodzie powyżej, firma Microsoft wykorzystaniu opartych na konwencjach wzorzec nazewnictwa w programie ASP.NET MVC i po prostu określenie "DinnerForm" jako nazwy częściowego do renderowania. W takim przypadku to ASP.NET MVC sprawdza pierwszy w katalogu widoków opartych na konwencjach (DinnersController to/widoków/kolacji). Jeśli nie znajdzie z częściowa szablon zostanie następnie wygląda go w katalogu /Views/Shared.

Html.RenderPartial() wywołanego na nazwę widoku częściowego ASP.NET MVC zostanie przekazany do widoku częściowego do tego samego modelu ViewData słownika obiektów i używane przez wywołanie szablonu widoku. Istnieją również przeciążone wersje Html.RenderPartial() umożliwiające przekazać alternatywny obiekt modelu i/lub słownik ViewData widoku częściowego do użycia. Jest to przydatne w scenariuszach, w której chcesz tylko przekazać podzestaw pełnego modelu/ViewModel.

| **Temat po stronie: Dlaczego &lt;%%&gt; zamiast &lt;% = %&gt;?** |
| --- |
| Jest jednym z elementów niewielkie, można zauważyć z powyższym kodzie, czy użyto &lt;%%&gt; zablokować zamiast &lt;% = %&gt; zablokować podczas wywoływania metody Html.RenderPartial(). &lt;% = %&gt; bloków w programie ASP.NET wskazują, że deweloper chce renderowania określona wartość (na przykład: &lt;% = "Hello" %&gt; będzie renderować "tekst Hello"). &lt;%%&gt; bloki zamiast tego wskazują, że deweloper chce wykonać kod, i że renderowana wszelkie dane wyjściowe w nich należy jawnie (na przykład: &lt;Response.Write("Hello") %&gt;. Przyczyna używamy &lt;%%&gt; jest blok z naszego kodu Html.RenderPartial powyżej, ponieważ metoda Html.RenderPartial() nie zwrócił ciągu i zamiast tego wyprowadza strumień wyjściowy zawartość bezpośrednio do wywoływania szablonu widoku. Robi to wydajność ze względu na wydajność i wykonując tak takie rozwiązanie pomaga uniknąć konieczności tworzenia obiekt ciągu tymczasowe (potencjalnie bardzo duża liczba godzin). Zmniejsza zużycie pamięci i zwiększa przepustowość ogólną aplikacji. Jeden powszechnym błędem, gdy przy użyciu Html.RenderPartial() jest dodanie średnikiem na końcu wywołania, gdy znajduje się w &lt;%%&gt; bloku. Na przykład ten kod spowoduje błąd kompilatora: &lt;Html.RenderPartial("DinnerForm") %&gt; zamiast tego należy wpisać: &lt;% % Html.RenderPartial("DinnerForm");&gt; jest to spowodowane &lt;%%&gt; bloki są instrukcje niezależne kodu i korzystając z instrukcji musi być zakończona średnikiem kodu C#. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Za pomocą szablonów widoku częściowego wyjaśnienie kodu

Utworzono szablon widoku częściowego "DinnerForm", aby uniknąć duplikowania logiki renderowania widoku w kilku miejscach. Jest to najbardziej typowe przyczyny do tworzenia szablonów widoku częściowego.

Czasami nadal warto tworzyć widoki częściowe, nawet wtedy, gdy są one wywoływany tylko w jednym miejscu. Wyświetlanie bardzo skomplikowane szablonów często może stać się znacznie łatwiejsze do odczytu, jeśli ich logiki renderowania widoku jest wyodrębnione i podzielona na partycje w jeden lub więcej również o nazwie częściowe szablonów.

Rozważmy na przykład poniżej fragment kodu z pliku Site.master w naszym projektu (który firma Microsoft będzie patrzeć wkrótce). Kod jest względnie proste — można odczytać — częściowo, ponieważ logiki, aby wyświetlić logowania/wylogowania łącze u góry rogu ekranu jest hermetyzowany w części "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Zawsze, gdy okaże się, że pierwsze pomylić próby zrozumieć kod znaczników html/kod w ramach szablonu widoku, rozważ, czy nie będą jaśniejszy, jeśli część z nich został wyodrębniony, zrefaktoryzowany do przekazującego widoki częściowe.

### <a name="master-pages"></a>Strony wzorcowej

Oprócz obsługi widoki częściowe, ASP.NET MVC obsługuje również możliwość tworzenia szablonów "strony wzorcowej", które mogą służyć do definiowania układu typowe i html najwyższego poziomu w lokacji. Symbol zastępczy, który można następnie dodać formantów strony wzorcowej do identyfikowania replaceable regionów, które mogą zostać zastąpione lub "wypełnione" przy użyciu funkcji widoków zawartości. Dzięki temu bardzo skuteczne (i suchej) do zastosowania wspólnej układu wewnątrz aplikacji.

Domyślnie nowe projekty składnika ASP.NET MVC ma automatycznie dodawane do nich szablonu strony wzorcowej. Ta strona wzorcowa nosi nazwę "Site.master" i życie znajdujących się w folderze \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Domyślny plik Site.master wygląda jak poniżej. Definiuje zewnętrzne html witryny, wraz z menu nawigacji u góry. Ten przewodnik zawiera dwa formanty replaceable symbolu zastępczego zawartości — jeden dla nazwy i innych, dla której głównej zawartość strony powinna zostać zastąpiona:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Wszystkie szablony widoku, który utworzyliśmy dla aplikacji NerdDinner ("List", "Szczegóły", "Edytuj", "Utwórz", "NotFound", itp.) ma zostać na podstawie tego Site.master szablonu. To jest wskazywana przez atrybut "MasterPageFile", który został dodany domyślnie do góry &lt;% @ strony %&gt; dyrektywy podczas utworzyliśmy naszych widoków, korzystając z okna dialogowego "Dodaj widok":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Co oznacza że można zmienić zawartości Site.master i mieć zmiany można zastosować i automatycznie używany, gdy firma Microsoft świadczą naszym szablony widoku.

Teraz należy zaktualizować naszych Site.master nagłówka sekcji, tak aby nagłówek naszej aplikacji jest "NerdDinner" zamiast "Moja aplikacja MVC". Umożliwia również zaktualizować naszych menu nawigacji, aby pierwszej karcie jest "Znajdź obiad" (obsługiwane przez metodę akcji indeks() HomeController), a teraz dodać nową kartę o nazwie "Host obiad" (obsługiwane przez metodę akcji Create() DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Firma Microsoft zapisywania pliku Site.master i Odśwież naszej przeglądarki zajmiemy się tym naszej nagłówka zmiany Pokaż we wszystkich widokach w naszej aplikacji. Na przykład:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

I */Dinners/Edit / [id]* adresu URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Następny krok

Częściowe i stron wzorcowych zapewniają bardzo elastyczne opcje, które umożliwiają prawidłowo organizowanie widoków. Znajdują się one uniknąć duplikowania widok zawartości / kodu czy ułatwiają odczytu i Obsługa szablonów widoku.

Umożliwia teraz ponownie scenariusza listy, który wcześniej budujemy i włączyć obsługę stronicowania skalowalności.

>[!div class="step-by-step"]
[Poprzednie](use-viewdata-and-implement-viewmodel-classes.md)
[dalej](implement-efficient-data-paging.md)
