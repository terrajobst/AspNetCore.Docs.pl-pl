---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Użyj widoków i kontrolerów, aby zaimplementować interfejs użytkownika lista/szczegóły | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 4 pokazano, jak dodać kontroler do aplikacji, która korzysta z zalet nasz model, aby zapewnić użytkownikom środowisko nawigacji lista/szczegóły danych...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 203a12473f79f38f7162d360d2179ca7c4a30303
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753185"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Użyj widoków i kontrolerów, aby zaimplementować interfejs użytkownika lista/szczegóły
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 4 bezpłatne [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 4 przedstawiono sposób dodawania kontrolera do aplikacji, która korzysta z zalet nasz model, aby zapewnić użytkownikom danych lista/szczegóły środowisko nawigacji dla kolacji w naszej witrynie NerdDinner.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner krok 4 kontrolery i widoki

Za pomocą platform tradycyjnej sieci web (klasyczne środowisko ASP, PHP, ASP.NET Web Forms itp.) przychodzących adresów URL są zwykle mapowane na pliki na dysku. Na przykład: żądania dla adresu URL, takich jak "/ Products.aspx" lub "/ Products.php" mogą być przetwarzane przez plik "Products.aspx" lub "Products.php".

Oparte na sieci Web platformy MVC adresy URL są mapowane na kod serwera w nieco inny sposób. Zamiast mapowania przychodzących adresów URL do plików, zamiast tego mapowania ich adresy URL do metod w klasach. Te klasy są nazywane "Kontrolerów" i są one odpowiedzialne za przetwarzanie przychodzących żądań HTTP, Obsługa danych wejściowych użytkownika, pobierania i zapisywania danych i określania odpowiedź do wysłania z powrotem do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny Adres URL itp.).

Teraz, gdy zostały utworzyliśmy podstawowy model dla naszej aplikacji NerdDinner, naszym kolejnym krokiem będzie dodać kontroler do aplikacji, która wykorzystuje ona użytkownikom danych lista/szczegóły środowisko nawigacji dla kolacji w naszej witrynie.

### <a name="adding-a-dinnerscontroller-controller"></a>Dodawanie kontrolera DinnersController

Firma Microsoft będzie zacząć, klikając prawym przyciskiem myszy w folderze "Kontrolerów" w projekcie sieci web, a następnie wybierz **Add -&gt;kontrolera** polecenia menu (można również wykonać tego polecenia, wpisując Ctrl-M, Ctrl + C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj kontroler":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Firma Microsoft będzie Nazwij nowy kontroler o nazwie "DinnersController" i kliknij przycisk "Dodaj". Program Visual Studio spowoduje dodanie pliku DinnersController.cs naszego katalogu \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Otworzy się również się nową klasę DinnersController w edytorze kodu.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Dodawanie indeks() i metod akcji Details() do klasy DinnersController

Chcemy włączyć odwiedzających, przeglądając listę nadchodzących kolacji i zezwolić im na kliknięcie dowolnej obiad na liście, aby wyświetlić szczegółowe informacje na ten temat za pomocą naszej aplikacji. Możemy to zrobić poprzez publikowanie następujące adresy URL z naszej aplikacji:

| **ADRES URL** | **Cel** |
| --- | --- |
| */Dinners/* | Wyświetl listę nadchodzących kolacji HTML |
| */Dinners/szczegóły / [id]* | Wyświetl szczegółowe informacje o określonych obiad wskazywany przez parametr "id" osadzone w adresie URL — która będzie odpowiadała DinnerID systemu obiad w bazie danych. Na przykład: /Dinners/Details/2 spowoduje wyświetlenia strony HTML ze szczegółowymi informacjami o obiad, którego wartość DinnerID to 2. |

Opublikujemy początkowe implementacje tych adresów URL, dodając dwa publiczne "metod akcji" do klasy Nasze DinnersController podobnie jak poniżej:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Następnie utworzymy uruchamianie aplikacji NerdDinner i wywoływać je przy użyciu naszego przeglądarki. Pisanie w *"/ kolacji /"* spowoduje, że adres URL naszych *indeks()* metody wykonywania i wyśle ponownie następującą odpowiedź:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Pisanie w *"/ kolacji/szczegóły/2"* spowoduje, że adres URL naszych *Details()* metodę, aby uruchomić i odsyłania następującą odpowiedź:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Możesz się zastanawiać — jak platformy ASP.NET MVC wiedzieć, aby utworzyć klasy Nasze DinnersController i wywoływanie tych metod? Aby zrozumieć, że możemy przyjrzeć szybkiego działa jak routingu.

### <a name="understanding-aspnet-mvc-routing"></a>Omówienie platformy ASP.NET MVC routingu

Platforma ASP.NET MVC zawiera zaawansowany aparat routingu adresów URL, który zapewnia dużą elastyczność w kontrolowaniu, jak adresy URL są mapowane do klas kontrolera. Pozwala dostosować sposób wybiera której klasy kontrolera, aby utworzyć, którą metodę należy wywoływać w nim, a także skonfigurować różne sposoby zmiennych można automatycznie pochodzącą z adresu URL/Querystring analizy i przekazywany do metody jako parametr w ASP.NET MVC argumenty. System ten zapewnia elastyczność całkowicie optymalizacji witryny pod kątem Wyszukiwarek (optymalizacji dla aparatów wyszukiwania), a także publikowanie dowolnego Struktura adresu URL, którą chcemy z aplikacji.

Domyślnie nowe projekty ASP.NET MVC pochodzą ze zbiorem wstępnie skonfigurowanych reguł routingu adresów URL już zarejestrowany. Pozwala na łatwe wprowadzenie do aplikacji bez konieczności jawnego konfigurowania niczego. Rejestracje reguły routingu domyślnego można znaleźć w klasie "Aplikacja" z naszych projektów — które firma Microsoft można otworzyć przez dwukrotne kliknięcie pliku "Global.asax" w katalogu głównym w projekcie:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Domyślne reguły routingu platformy ASP.NET MVC są rejestrowane w metodzie "RegisterRoutes" tej klasy:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Trasy. MapRoute() "wywołania metody powyżej rejestruje domyślną regułę routingu, która mapuje przychodzących adresów URL do klas kontrolera, używając formatu adresu URL:" / {controller} / {action} / {id} "— w przypadku, gdy"controller"jest nazwą klasy kontrolera do utworzenia wystąpienia,"action"to nazwa publiczne metody do wywołania na i "id" jest parametrem opcjonalnym osadzone w adresie URL, który może być przekazywany jako argument do metody. Trzeci parametr przekazany do wywołania metody "MapRoute()" to zbiór wartości domyślne, aby używać wartości identyfikatora kontroler/akcji w przypadku, gdy nie są obecne w adresie URL (kontroler = "Home", Akcja = "Index", Id = "").

Poniżej znajduje się tabela, która pokazuje, jak różne adresy URL są zamapowane przy użyciu domyślnego "<em>/ {kontrolerów} / {action} / {id}"</em>reguły trasy:

| **ADRES URL** | **Klasa kontrolera** | **Metody akcji** | **Parametry przekazane** |
| --- | --- | --- | --- |
| */ Kolacji/szczegóły/2* | DinnersController | Details(ID) | id=2 |
| */ Kolacji/Edit/5* | DinnersController | Edit(ID) | id=5 |
| */Dinners/Create* | DinnersController | Create() | Brak |
| */ Kolacji* | DinnersController | Index() | Brak |
| *Domowych* | HomeController | Index() | Brak |
| */* | HomeController | Index() | Brak |

Ostatnie trzy wiersze Pokaż wartości domyślne (kontroler = domu, akcja = indeks, identyfikator = "") używana. Ponieważ metoda "Index" jest zarejestrowany jako domyślnej nazwy akcji, jeśli nie jest określony, "/ kolacji" i "/ Home" Przyczyna adresy URL indeks() metody akcji do wywołania na ich klasy kontrolera. Ponieważ kontroler "Home" jest zarejestrowany jako domyślny kontroler, jeśli nie jest określony, adres URL "/" powoduje, że HomeController ma zostać utworzony i metody akcji indeks() go do wywołania.

Jeśli nie potrzebujesz tych reguł routingu adresów URL domyślnego dobra wiadomość jest czy można łatwo zmienić — po prostu je edytować w metodzie RegisterRoutes powyżej. Dla naszej aplikacji NerdDinner, użyjemy nie zmienia żadnych reguł routingu adresów URL domyślnego — zamiast tego po prostu użyjemy ich jako-to.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Za pomocą DinnerRepository z naszych DinnersController

Teraz Przyjrzyjmy zastąpić naszych bieżąca implementacja parametru DinnersController indeks() i Details() metody akcji przy użyciu implementacji, które używają nasz model.

Użyjemy klasy DinnerRepository wcześniej skompilowany Aby zaimplementować to zachowanie. Firma Microsoft będzie rozpocząć przez dodanie instrukcji "using", który odwołuje się do przestrzeni nazw "NerdDinner.Models", a następnie Zadeklaruj wystąpienie naszych DinnerRepository jako pole w klasie naszych DinnerController.

Później w tym rozdziale utworzymy wprowadzono koncepcję "Wstrzykiwanie zależności" i pokazują nasze kontrolerów, aby uzyskać odwołanie do DinnerRepository, która umożliwia lepsze jednostki w inny sposób testowania — ale po prawej stronie teraz po prostu utworzymy wystąpienie naszych DinnerRepository wbudowane, podobnie jak poniżej.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Teraz możemy przystąpić do generowania odpowiedzi HTML, ponownie za pomocą naszych obiekty modelu pobrane dane.

### <a name="using-views-with-our-controller"></a>Korzystanie z widoków z kontrolera

Choć jest możliwe do pisania kodu w ramach naszych metody akcji, aby złożyć HTML, a następnie użyj *Response.Write()* metody pomocnika do wysłania do klienta, że podejście staje się niewydolna dość szybko. Znacznie lepszym rozwiązaniem jest dla nas do wykonywania tylko aplikacji i danych logiki wewnątrz naszych DinnersController metody akcji, a następnie przekazać dane potrzebne do renderowania odpowiedzi HTML do szablonu oddzielnych "view", który jest odpowiedzialny za podawania reprezentacji w formacie HTML z niej. Jak zobaczymy za chwilę "view" szablonu jest plik tekstowy, który zawiera zazwyczaj kombinacją kod znaczników HTML i renderowania osadzonego kodu.

Oddzielenie logiki naszych kontrolera od naszych renderowania widoku zapewnia kilka korzyści big Data. W szczególności ułatwia wymuszanie "separacji" między kodem aplikacji i kodem formatowanie renderowania interfejsu użytkownika. To ułatwia znacznie test jednostkowy logiki aplikacji w izolacji od logiki renderowania interfejsu użytkownika. Ułatwia on później zmodyfikować szablony renderowania interfejsu użytkownika, bez konieczności wprowadzania zmian w kodzie aplikacji. I jej może ułatwić dla deweloperów i projektantów, współpracę nad projektami.

Aktualizujemy nasze klasy DinnersController, aby wskazać, że chcemy użyć szablonu widoku do odesłania odpowiedzi interfejsu użytkownika HTML, zmieniając podpisy metod naszych metod akcji dwóch miał typ zwracany "void", aby zamiast tego zwracany typ "ActionResult". Następnie możemy wywołać *View()* metody pomocniczej dla klasy bazowej kontrolera do zwrócenia z powrotem do obiektu "ViewResult" takich jak:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Podpis *View()* metodę pomocnika używamy powyżej wygląda jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Pierwszy parametr *View()* metody pomocnika to nazwa pliku szablonu widoku, firma Microsoft ma być używany do renderowania odpowiedzi HTML. Drugi parametr jest obiektu modelu, który zawiera dane, które szablon widoku wymaga do renderowania odpowiedzi HTML.

W ramach naszych metody akcji indeks() dzwonimy *View()* metody pomocnika i wskazujący, że chcemy renderować HTML listę kolacji za pomocą szablonu usługi "Index" w widoku. Firma Microsoft kończy się sukcesem szablon widoku sekwencji obiektów obiad do generowania listy od:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

W ramach naszych Details() metody akcji, kolejna próba pobrania obiektu obiad przy użyciu identyfikatora podana w adresie URL. Jeśli zostanie znaleziony prawidłowy obiad, nazywamy *View()* metody pomocnika, wskazujący, chcemy renderowania pobrano obiekt obiad przy użyciu szablonu widoku "Szczegóły". Jeśli wymagana jest nieprawidłowa obiad, firma Microsoft renderowania przydatne komunikat o błędzie oznacza, że Dinner nie istnieje, przy użyciu szablonu widoku "NotFound" (i przeciążoną wersję *View()* metody pomocnika, która po prostu przyjmuje nazwę szablonu ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Przejdźmy teraz wdrożyć Przeglądanie szablonów "NotFound", "Szczegóły" i "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementowanie "NotFound" Wyświetl szablon

Rozpocznie się dzięki wdrożeniu Wyświetl szablon "NotFound" — która wyświetla komunikat z przyjazną błąd wskazujący, że nie można znaleźć żądanej firmy dinner.

Utworzymy nowy szablon widoku, ustawiając kursor naszych tekstu w obrębie metody akcji kontrolera, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie menu "Dodaj widok" (Firma Microsoft może również wykonywania tego polecenia, wpisując Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Zostanie wyświetlone okno dialogowe "Dodaj widok", takich jak poniżej. Domyślnie okno dialogowe zostanie wstępnie wprowadzona nazwa widoku, aby utworzyć dopasowany do nazwy metody akcji kursor w momencie tworzenia okna dialogowego została uruchomiona (w tym przypadku "Szczegóły"). Ponieważ chcemy najpierw zaimplementować w szablonie "NotFound", utworzymy przesłaniania tej nazwy widoku i ustawić go jako zamiast "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Kliknięcie przycisku "Dodaj", programu Visual Studio Utwórz nowy szablon widoku "NotFound.aspx" dla nas w katalogu "\Views\Dinners" (co spowoduje również utworzenie, jeśli katalog nie istnieje):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Otworzy się również się nasz nowy szablon widoku "NotFound.aspx" w edytorze kodu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Wyświetl szablony domyślne mają dwa "zawartości regiony" gdzie możemy dodać treści i kodu. Pierwsza z nich umożliwia nam dostosować "title" strony HTML wysyłanych z powrotem. Drugi pozwala nam dostosować "głównego zawartość" strony HTML wysyłanych z powrotem.

Do zaimplementowania naszych "NotFound" Wyświetl szablon dodamy niektóre podstawowe elementy:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Firma Microsoft mogą następnie korzystać z niego w przeglądarce. W tym celu możemy żądania *"/ kolacji/szczegóły/9999"* adresu URL. Ten będzie odnosił się do obiad, obecnie nie istnieje w bazie danych, która spowoduje, że nasze metody akcji DinnersController.Details() do renderowania naszych "NotFound" Wyświetl szablon:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Jedyną operacją, której można zauważyć na zrzucie ekranu powyżej jest, że szablon widoku podstawowym odziedziczył wiele kod HTML, który otacza głównej zawartości na ekranie. Jest to spowodowane nasz szablon widoku używa szablonu "strony wzorcowej", która umożliwia nam zastosowanie spójnego układu we wszystkich widokach w witrynie. Omówimy, jak strony wzorcowe pracy więcej w dalszej części tego samouczka.

### <a name="implementing-the-details-view-template"></a>Implementowanie "Szczegóły" Wyświetl szablon

Przejdźmy teraz wdrożyć szablonie widok "Szczegóły" — zostanie generują kod HTML dla pojedynczego modelu obiad.

Firma Microsoft będzie to zrobić, ustawiając kursor naszych tekstu w obrębie szczegóły metody akcji, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie "Dodaj widok" (lub naciśnij klawisze Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Zostanie wyświetlone okno dialogowe "Dodaj widok". Zachowamy nazwę widoku domyślnego ("szczegóły"). Firma Microsoft będzie również zaznaczyć pole wyboru "Utwórz widok silnie typizowane" w oknie dialogowym i wybierz (przy użyciu listy rozwijanej pola kombi) nazwę typu modelu, który możemy przechodzi z kontrolera do widoku. Dla tego widoku możemy przekazanie obiektu obiad (w pełni kwalifikowana nazwa dla tego typu to: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

W przeciwieństwie do poprzednich szablonu, gdzie wybrano utworzyć "Pusty widok", tym razem, którymi firma Microsoft będzie automatycznie "tworzenia szkieletu" widok przy użyciu szablonu "Szczegóły". Firma Microsoft może wskazywać, zmieniając "Wyświetl zawartość" listy rozwijanej w oknie dialogowym powyżej.

"Tworzenia szkieletów" spowoduje wygenerowanie początkowej wdrożenia na podstawie obiektu obiad, firma Microsoft jest przekazanie do niego szablon widoku szczegółów. Zapewnia to prosty sposób nam na szybkie rozpoczęcie pracy w naszej implementacji szablonu widoku.

Kliknięcie przycisku "Dodaj", Visual Studio utworzy nowy plik szablonu widoku "Details.aspx" nam naszym katalogu "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Otworzy się również się nasz nowy szablon widoku "Details.aspx" w edytorze kodu. Będzie zawierać implementację szkieletu początkowa widoku szczegółów, oparte na modelu obiad. Aparat tworzenia szkieletów używa odbicia .NET, aby wyświetlić właściwości publicznej, widoczne w klasie przekazano go, a spowoduje to dodanie odpowiedniej zawartości na podstawie każdego typu, który odnajdzie:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Firma Microsoft może zażądać *"/ 1/szczegóły/kolacji"* adresu URL, aby zobaczyć, jak wygląda tej implementacji szkieletu "szczegóły" w przeglądarce. Przy użyciu tego adresu URL, zostanie wyświetlony jeden z kolacji, zapoczątkowany ręcznie naszej bazie danych wtedy stworzyliśmy najpierw:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Pobiera nam szybko uruchamiać wdrożenia i zapewnia nam za pomocą początkowego stosowania naszych widok Details.aspx. Firma Microsoft następnie go i dostosować go dostosować interfejsu użytkownika, zgodnie z wymaganiami naszych.

Gdy spojrzymy na szablon Details.aspx dokładniej, firma Microsoft znajdują się aby go statyczny kod HTML zawiera także osadzone w kodzie. &lt;%%&gt; nuggets kodu wykonanie kodu, gdy szablon widoku renderuje, i &lt;% = %&gt; nuggets kodu wykonywania kodu zawartych w nich i następnie renderowania wynik w strumieniu wyjściowym szablonu.

W naszym widoku, który uzyskuje dostęp do obiektu modelu "Obiad", który został przekazany z naszych kontrolera, używając właściwości "Model" silnie typizowane, możemy napisać kod. Program Visual Studio zapewnia pełny kod funkcji intellisense podczas uzyskiwania dostępu do tej właściwości "Model" w edytorze:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Upewnijmy się dostosowaliśmy, tak aby źródła dla naszych ostateczny szablon widoku szczegółów wygląda jak poniżej:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Podczas dostępu do *"/ 1/szczegóły/kolacji"* adresu URL ponownie będzie teraz renderowania, takich jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementowanie "Index" Wyświetl szablon

Przejdźmy teraz wdrożyć szablon widoku "Index" — która generuje listę nadchodzących kolacji. Zadanie do wykonania, to firma Microsoft będzie umieść kursor naszych tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy kliknij polecenie menu "Dodaj widok" (lub naciśnij klawisze Ctrl-M, Ctrl-V).

W oknie dialogowym "Dodaj widok" Firma Microsoft nadal Wyświetl szablon o nazwie "Index" i zaznacz pole wyboru "Utwórz widok silnie typizowane". Tym razem wybierzemy automatycznie Generuj szablon widoku "List" i wybierz pozycję "NerdDinner.Models.Dinner" jako typ modelu przekazywane do widoku (który ponieważ został wskazany, tworzymy "List" szkieletu spowoduje, że okno dialogowe Dodaj widok założył, firma Microsoft przekazywanie sekwencji obiektów obiad z kontrolera do widoku):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Kliknięcie przycisku "Dodaj", Visual Studio utworzy nowy plik szablonu "Index.aspx" Widok nam naszym katalogu "\Views\Dinners". Jej będzie "tworzenia szkieletu" początkowej implementacji znajdujący się w nim zapewniająca HTML zapoznać się z dostępnymi kolacji przekażemy do widoku.

Firma Microsoft uruchamiania aplikacji i dostępu *"/ kolacji /"* renderowanie zostanie przeprowadzone w naszej listy kolacji adresu URL w następujący sposób:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Rozwiązanie tabeli powyżej daje nam układu siatki w naszych danych obiad — nie jest dość chcemy dla naszych klientów, połączonego z listy obiad. Firma Microsoft może zaktualizować szablon widoku Index.aspx i zmodyfikuj je listy mniejszą liczbę kolumn danych, a następnie użyć &lt;ul&gt; element do renderowania je zamiast tabeli za pomocą poniższego kodu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Używamy słowa kluczowego "var" w ramach powyższych instrukcji foreach, ponieważ jest używana pętla nad każdym obiad w naszym modelu. Te zaznajomiony z języka C# 3.0 wydaje się, że za pomocą "var" oznacza, że obiekt obiad z późnym wiązaniem. Zamiast tego oznacza, że kompilator używa wnioskowanie o typie względem silnie typizowane właściwości "Model" (czyli typu "IEnumerable&lt;obiad&gt;") i kompilowanie zmiennej lokalnej "obiad" jako typu Dinner — oznacza, że uzyskujemy pełną funkcja IntelliSense i kompilacji, szukanie wewnątrz bloków kodu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Gdy odświeżymy */Dinners* adresu URL w przeglądarce, nasze naszych wygląda teraz zaktualizować widok, takich jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

To wygląda lepiej —, ale nie jest całkowicie istnieje jeszcze. Nasze ostatnim krokiem jest umożliwienie użytkownikom końcowym kliknij poszczególne kolacji na liście i zobaczyć szczegółowe informacje o nich. Firma Microsoft będzie implementowana przez renderowaniu elementów HTML hiperłącze, które łącze do metody akcji szczegółowe informacje na naszym DinnersController.

Firma Microsoft może generować te hiperłącza w naszym widoku indeksu w jednej z dwóch sposobów. Pierwsza to ręczne tworzenie HTML &lt;&gt; elementów, takich jak poniżej, gdzie wbudowaliśmy &lt;%%&gt; blokuje w ramach &lt;&gt; elementu HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Inną metodą, możemy użyć jest z zalet wbudowane metody pomocnika "Html.ActionLink()" w ramach platformy ASP.NET MVC, który obsługuje programowe tworzenie kodu HTML &lt;&gt; element, który stanowi łącze do innej metody akcji w Kontroler:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Pierwszy parametr do metody pomocnika Html.ActionLink() jest tekst łącza do wyświetlenia (w tym przypadku tytuł dinner), drugi parametr jest nazwa akcji kontrolera, chcemy, aby wygenerować łącze do (w tym przypadku metoda szczegóły), a trzeci parametr jest zestaw parametrów do wysłania do akcji (zaimplementowane jako anonimowy typ o wartości nazwy właściwości). W tym przypadku jest określenie parametru "id" obiad możemy chcesz się połączyć, a ponieważ domyślny routing adresów URL reguły we wzorcu ASP.NET MVC jest "{Controller} / {Action} / {id}" metody pomocnika Html.ActionLink() wygeneruje następujące wyniki:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Nasze widoku Index.aspx firma Microsoft podejście metody pomocnika Html.ActionLink() i mieć każdego obiad łącza listy odpowiednie szczegóły adres URL:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

A teraz, gdy firma Microsoft trafiony */Dinners* adres URL listy obiad wygląda jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Po kliknięciu dowolnego kolacji na liście przejdziemy szczegółowe informacje na ten temat:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Oparty na konwencji nazewnictwa i struktury katalogów \Views

Aplikacje platformy ASP.NET MVC domyślnie za katalog oparty na Konwencji strukturę nadawania nazw podczas rozpoznawania Przeglądanie szablonów. Umożliwia to deweloperom uniknąć konieczności pełnej kwalifikacji ścieżka lokalizacji podczas odwoływania się do widoków z w obrębie klasy kontrolera. Domyślnie platformy ASP.NET MVC będzie szukać pliku szablonu widoku w * \Views\[ControllerName]\* katalogu poniżej aplikacji.

Na przykład, mamy pracowano klasy DinnersController — która jawnie odwoływać się do trzy szablony widoku: "Index", "Szczegóły" i "NotFound". ASP.NET MVC domyślnie wyszuka tych widoków w obrębie *\Views\Dinners* katalogu poniżej nasz katalog główny aplikacji:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Zwróć uwagę, powyżej jak tam są obecnie dostępne trzy klasy kontrolera w projekcie (DinnersController, HomeController i elementu AccountController — ostatnie dwa dodano domyślnie podczas tworzenia projektu), istnieją trzy podkatalogi (po jednym dla każdego Kontroler) w katalogu \Views.

Widoki odwoływać się z główną i kont kontrolerów automatycznie rozwiąże ich Przeglądanie szablonów z odpowiednich *\Views\Home* i *\Views\Account* katalogów. *\Views\Shared* podkatalogu zapewnia sposób przechowywania Przeglądanie szablonów, które są ponownie używane w obrębie wielu kontrolerów w aplikacji. Gdy platformy ASP.NET MVC próbuje rozpoznać szablonu widoku, najpierw będzie sprawdzać w ramach *\Views\[kontrolera]* określonego katalogu i nie można znaleźć Wyświetl szablon zostanie ona wyszukana w ramach *\Views\ Udostępnione* katalogu.

Jeśli chodzi o nazw poszczególnych Przeglądanie szablonów, zalecane wskazówki jest Wyświetl szablon ma taką samą nazwę jak metody akcji, który spowodował jego renderowania. Na przykład powyżej naszych "Index" metody akcji jest przy użyciu widoku "Index" do renderowania wynik widoku, a metody akcji "Szczegóły" jest w widoku "Szczegóły" do renderowania wyniki. Ułatwia to szybko wyświetlić szablon, który jest skojarzony z każdej akcji.

Deweloperzy nie muszą jawnie określ nazwę szablonu widoku, gdy szablon widoku ma taką samą nazwę jak metoda akcji wywoływanej na kontrolerze. Możemy zamiast tego po prostu przekazać obiekt modelu do metody pomocnika "View()" (bez określenia nazwy widoku) i platformy ASP.NET MVC zostanie automatycznie wywnioskować, że chcemy użyć *\Views\[ControllerName]\[ActionName]* Wyświetl szablon na dysku, aby go renderować.

Pozwoli to nam trochę wyczyścić naszego kodu kontrolera i uniknąć duplikowania nazwa dwa razy w naszym kodzie:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Powyższy kod to wszystkie, które jest niezbędne do zaimplementowania nieuprzywilejowany lista obiad/szczegóły środowiska dla witryny.

#### <a name="next-step"></a>Następny krok

W efekcie powstał obiad dobre rozwiązanie, przeglądania Internetu skompilowane.

Przejdźmy teraz włączyć obsługę edytowania formularza danych CRUD (tworzenia, odczytu, Update, Delete).

> [!div class="step-by-step"]
> [Poprzednie](build-a-model-with-business-rule-validations.md)
> [dalej](provide-crud-create-read-update-delete-data-form-entry-support.md)
