---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: "Użycie kontrolery i widoki do zaimplementowania interfejsu użytkownika listę/szczegółów | Dokumentacja firmy Microsoft"
author: microsoft
description: "Krok 4 przedstawiono sposób dodawania kontrolera do aplikacji, która korzysta z naszego modelu, aby zapewnić użytkownikom przy użyciu środowiska nawigacji listę/szczegóły danych..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 2f9148a2d419863229e2c5a2a0c98984001fcee5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>W celu zaimplementowania interfejsu użytkownika listę/szczegóły użyć kontrolery i widoki
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 4 bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 4 przedstawiono sposób dodawania kontrolera do aplikacji, która korzysta z naszego modelu, aby zapewnić użytkownikom przy użyciu danych listy/szczegóły nawigacji środowiska dla kolacji w naszej witrynie NerdDinner.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner krok 4: Kontrolery i widoki

Web tradycyjnych struktur (klasyczne środowisko ASP, PHP, formularzy sieci Web ASP.NET itp.) przychodzących adresów URL są zwykle mapowane do plików na dysku. Na przykład: żądanie dla danego adresu URL, takich jak "/ Products.aspx" lub "/ Products.php" mogą być przetwarzane przez plik "Products.aspx" lub "Products.php".

Oparte na sieci Web platformy MVC adresy URL są mapowane na kod serwera w nieco inny sposób. Zamiast mapowania przychodzących adresów URL do plików, zamiast tego mapują adresy URL do metody klasy. Klasy te są nazywane "Kontrolerów" i są one odpowiedzialna za przetwarzanie przychodzących żądań HTTP w celu obsługi danych wejściowych użytkownika, pobieranie i zapisywanie danych i określania odpowiedź do wysłania z powrotem do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny Adres URL itp.).

Teraz, nawiązaliśmy model podstawowe dla aplikacji NerdDinner, naszych następnego kroku zostaną dodane kontrolera do aplikacji, która wykorzystuje je do udostępniania użytkownikom przy użyciu danych listy/szczegóły nawigacji środowiska dla kolacji w naszej witrynie.

### <a name="adding-a-dinnerscontroller-controller"></a>Dodawanie kontrolera DinnersController

Firma Microsoft będzie rozpocząć przez kliknięcie prawym przyciskiem myszy folder "Kontrolery" w ramach naszych projektu sieci web, a następnie wybierz **Add -&gt;kontrolera** polecenia menu (możesz również wykonywania tego polecenia, wpisując polecenie Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Zostanie wyświetlone okno dialogowe "Dodaj kontroler":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Firma Microsoft będzie nazwa nowego kontrolera "DinnersController" i kliknij przycisk "Dodaj". Visual Studio spowoduje dodanie pliku DinnersController.cs w naszym katalogu \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Otworzy się również się nowa klasa DinnersController w edytorze kodu.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Dodawanie do klasy DinnersController indeks() i metod akcji Details()

Chcemy włączyć odwiedzających przeglądać listę nadchodzących kolacji i zezwolić im na kliknięcie żadnych obiad na liście, aby wyświetlić szczegółowe informacje o nim za pomocą naszej aplikacji. Firma Microsoft będzie w tym celu publikowania następujące adresy URL z naszej aplikacji:

| **ADRES URL** | **Cel** |
| --- | --- |
| */Dinners/* | Wyświetl listę nadchodzących kolacji HTML |
| */Dinners/szczegóły / [id]* | Wyświetlanie szczegółów dotyczących określonego obiad, wskazane przez parametr "id" osadzone w adresu URL —, który będzie pasował DinnerID z obiad w bazie danych. Na przykład: /Dinners/Details/2 spowoduje wyświetlenia strony HTML ze szczegółami obiad, którego wartość DinnerID jest równa 2. |

Firma Microsoft opublikuje początkowej implementacje tych adresów URL przez dodanie dwóch publicznego "metod akcji" do klasy Nasze DinnersController podobnie jak poniżej:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Firma Microsoft będzie następnie uruchomić aplikację NerdDinner i korzystanie z naszego przeglądarki do wywołania je. Pisanie w *"/ kolacji /"* spowoduje, że adres URL naszych *indeks()* metody do uruchomienia, a będzie odesłania następującą odpowiedź:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Pisanie w *"/ 2-kolacji/szczegółów"* spowoduje, że adres URL naszych *Details()* metodę, aby uruchomić i wysłanie przez następującą odpowiedź:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Może być zastanawiasz się — jak ASP.NET MVC znać, aby utworzyć klasy Nasze DinnersController i wywołania tych metod? Aby zrozumieć, który umożliwia szybkie Spójrz na działa jak routingu.

### <a name="understanding-aspnet-mvc-routing"></a>Opis programu ASP.NET MVC routingu

Platforma ASP.NET MVC zawiera zaawansowany aparat routingu adresów URL, który zapewnia dużą elastyczność w kontroli, jak adresy URL są mapowane do klas kontrolera. Pozwala dostosować sposób ASP.NET MVC wybierze która klasa kontrolera, aby utworzyć, którą metodę należy wywoływać w nim, a także skonfigurować różne sposoby zmienne można automatycznie przeanalizować z adresu URL/na ciąg zapytania i przekazane do metody jako parametr argumenty. Zapewnia to elastyczność całkowicie optymalizacji witryny dla aparatów wyszukiwania (optymalizacji dla aparatów wyszukiwania), a także publikowania żadnej struktury adresu URL, interesujące z aplikacji.

Domyślnie nowe projekty składnika ASP.NET MVC są dostarczane z wstępnie skonfigurowany zestaw reguł routingu adresów URL już zarejestrowany. Pozwala na łatwe Rozpoczynanie pracy z aplikacji bez konieczności jawnego konfigurowania żadnych czynności. Rejestracje reguły routingu domyślne można znaleźć w klasie "Aplikacja" naszych projektów — które możemy otworzyć, klikając dwukrotnie plik "Global.asax" w katalogu głównym projektu naszych:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Domyślne reguły routingu platformy ASP.NET MVC są rejestrowane w metodzie "RegisterRoutes" tej klasy:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Trasy. MapRoute() "powyżej wywołanie metody rejestruje domyślna reguła routingu, która mapuje przychodzących adresów URL do klas kontrolera przy użyciu formatu adresu URL:" / {controller} / {action} / {id} "— w przypadku"controller"Nazwa klasy kontrolera, można utworzyć wystąpienia,"Akcja"jest nazwą publicznej metody do wywołania i "id" jest parametrem opcjonalnym osadzone w adresie URL zawierających mogą zostać przekazane jako argument do metody. Trzeci parametr przekazywany do wywołania metody "MapRoute()" to zestaw wartości domyślne dla wartości identyfikatora kontroler/akcji w przypadku, gdy nie są obecne w adresie URL (kontroler = "Home", Akcja = "Index", Id = "").

Poniżej znajduje się tabela, która ilustruje sposób różnych adresów URL są zamapowane przy użyciu domyślnego "*/ {kontrolerów} / {action} / {id}"*trasy reguły:

| **ADRES URL** | **Klasa kontrolera** | **Metody akcji** | **Parametry przekazane** |
| --- | --- | --- | --- |
| */ Kolacji/szczegóły/2* | DinnersController | Details(ID) | Identyfikator = 2 |
| */ Kolacji/Edit/5* | DinnersController | Edit(ID) | Identyfikator = 5 |
| */ Kolacji/utworzyć* | DinnersController | Create() | Brak |
| */ Kolacji* | DinnersController | Indeks() | Brak |
| *Domowych* | HomeController | Indeks() | Brak |
| */* | HomeController | Indeks() | Brak |

Trzy ostatnie wiersze Pokaż wartości domyślne (kontrolera = Narzędzia główne, akcja = indeks, Id = "") używana. Ponieważ metoda "Index" jest zarejestrowany jako domyślna nazwa akcji, jeśli nie jest określony, "/ kolacji" i "/ Home" Przyczyna adresy URL indeks() metody akcji do wywołania w ich klasy kontrolera. Ponieważ kontroler "Home" jest zarejestrowany jako domyślnego kontrolera, jeśli nie jest określony, adres URL "/" powoduje HomeController do utworzenia i metody akcji indeks() go do wywołania.

Jeśli nie potrzebujesz tych domyślnych reguł routingu adresów URL, szczęście te są łatwo zmienić — wystarczy je edytować w metodzie RegisterRoutes powyżej. Dla aplikacji NerdDinner, jednak nie zostaną zmienić domyślne reguły routingu adresów URL — zamiast tego po prostu użyjemy ich jako — jest.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Przy użyciu DinnerRepository z naszych DinnersController

Umożliwia teraz Zastąp naszych bieżąca implementacja DinnersController indeks() i Details() metod akcji z implementacji, które używają modelu.

Użyjemy klasy DinnerRepository budujemy wcześniej do zaimplementowania zachowania. Firma Microsoft będzie rozpocząć, dodając instrukcję "przy użyciu", który odwołuje się do przestrzeni nazw "NerdDinner.Models", a następnie Zadeklaruj wystąpienie naszych DinnerRepository jako pole w klasie naszych DinnerController.

Później w tym rozdziale firma Microsoft będzie wprowadzenie pojęcia "Iniekcji zależności" i Pokaż naszych kontrolerów uzyskać odwołania do DinnerRepository, która umożliwia lepsze jednostki w inny sposób testowania — ale do prawej teraz utworzymy tylko wystąpienia naszych DinnerRepository śródwierszowe, podobnie jak poniżej.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Teraz możemy przystąpić do generowania odpowiedzi HTML przy użyciu naszych pobrane dane modelu obiektów.

### <a name="using-views-with-our-controller"></a>Korzystanie z widoków z kontrolera

Mimo że jest możliwa do pisania kodu, w ramach naszych metod akcji złóż HTML, a następnie użyć *metody Response.Write()* metody pomocnika do wysłania do klienta, że podejście staje się niewygodna stosunkowo szybko. Dużo lepszym rozwiązaniem jest dla nas do wykonywania tylko aplikacji i logika danych wewnątrz naszego DinnersController metod akcji, a następnie przekazać dane potrzebne do renderowania odpowiedzi HTML szablon oddzielne "Widok", który jest odpowiedzialny za generowanie reprezentacji w formacie HTML go. Jak zajmiemy się za chwilę, szablon "Wyświetl" jest plik tekstowy, który zwykle zawiera kombinację kodu znaczników HTML i renderowania osadzonego kodu.

Oddzielenie logiki kontrolera z naszych renderowania widoku oferuje wiele zalet duży. W szczególności ułatwia wymuszanie wyraźnej "separacji" między kodem aplikacji i kod formatowania renderowania interfejsu użytkownika. Dzięki temu łatwiej testu jednostkowego logiki aplikacji w izolacji od logika renderowania interfejsu użytkownika. Ułatwia on później zmodyfikować szablony renderowania interfejsu użytkownika bez wprowadzania zmian w kodzie aplikacji. I jego może ułatwić dla deweloperów i projektantów ze sobą współpracować nad projektami.

Firma Microsoft może aktualizować klasy Nasze DinnersController, aby wskazać, czy chcemy użyć szablonu widoku do odesłania odpowiedzi interfejsu użytkownika HTML, zmieniając podpisy metod naszych metod akcji dwa z zwracanego typu "void", aby zamiast tego zwracany typ "ActionResult". Firma Microsoft może wywoływać *View()* metody pomocnika dla klasy podstawowej kontrolera do zwrócenia z powrotem, takich jak obiekt "ViewResult" poniżej:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Podpis *View()* metody pomocnika używamy powyżej wygląda jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Pierwszy parametr *View()* metoda pomocnika jest nazwa widoku pliku szablonu, chcemy służące do renderowania odpowiedzi HTML. Drugi parametr jest obiekt modelu, który zawiera dane, które muszą szablon widoku do renderowania odpowiedzi HTML.

W naszym metody akcji indeks() wywołania *View()* metody pomocniczej i wskazujący chęć renderowania HTML lista kolacji przy użyciu szablonu usługi "Index" w widoku. Szablon widoku możemy przekazywane sekwencji obiad obiektów, aby wygenerować listę z:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

W naszym Details() metody akcji, które firma Microsoft próbował pobrać obiekt obiad za pomocą identyfikatora w adresie URL. Jeśli zostanie znaleziony prawidłowy obiad, nazywamy *View()* metody pomocnika wskazujące chcemy użyć szablonu "Szczegóły" widoku do renderowania pobrano obiekt obiad. Jeśli wymagane jest nieprawidłowa obiad, możemy renderowania przydatne komunikat o błędzie oznacza, że obiad nie istnieje, przy użyciu szablonu widoku "NotFound" (i zastąpionej wersji *View()* metody pomocniczej, która po prostu przyjmuje nazwę szablonu ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Implementuje umożliwia teraz wyświetlanie szablonów "NotFound", "Szczegóły" i "Index".

### <a name="implementing-the-notfound-view-template"></a>Wdrażanie szablonu widoku "NotFound"

Rozpocznie się zaimplementowanie "NotFound" Wyświetl szablon — która Wyświetla przyjazną komunikat o błędzie wskazujący nie można odnaleźć żądanego obiad.

Firma Microsoft będzie Utwórz nowy szablon widoku umieszczając kursor naszych tekstu w obrębie metody akcji kontrolera, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie "Dodaj widok", (możemy również wykonywania tego polecenia, wpisując polecenie Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Zostanie wyświetlone okno dialogowe "Dodaj widok", podobnie jak poniżej. Domyślnie okno dialogowe będzie wstępnego wypełniania nazwę widoku, aby utworzyć dopasowana do nazwy metody akcji kursor w momencie okna dialogowego została uruchomiona (w tym przypadku "Szczegóły"). Ponieważ chcemy, aby najpierw wdrożyć szablon "NotFound", firma Microsoft będzie zastąpić nazwę tego widoku i ustaw go jako zamiast "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Kliknięcie przycisku "Dodaj", Visual Studio utworzy nowy szablon widoku "NotFound.aspx" firmie Microsoft w ramach katalogu "\Views\Dinners" (co spowoduje również utworzenie, jeśli katalog nie istnieje):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Otworzy się również się naszego nowego szablonu widoku "NotFound.aspx" w edytorze kodu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Wyświetl szablony domyślnie ma dwie "obszarów zawartości" gdzie możemy dodać zawartość i kod. Pierwszy pozwala nam dostosować "title" strony HTML odesłał. Drugi pozwala nam dostosować "głównego zawartość" strony HTML odesłał.

Do zaimplementowania naszych "NotFound" Wyświetl szablon dodamy niektóre podstawowe elementy:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Firma Microsoft można następnie wypróbuj go w przeglądarce. Aby to zrobić teraz żądania *"/ kolacji/szczegóły/9999"* adresu URL. Ten będzie dotyczyć obiad, obecnie nie istnieje w bazie danych, która spowoduje, że nasze DinnersController.Details() metody akcji do renderowania naszych "NotFound" Wyświetl szablon:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Jedyną operacją, której można zauważyć w zrzut ekranu powyżej jest, że nasze szablon widoku podstawowym odziedziczył licznych HTML wokół zawartości ekranu głównego. Jest to spowodowane naszych szablon widoku jest przy użyciu szablonu "strony wzorcowej", który pozwala na zastosowanie spójnego układu we wszystkich widokach w witrynie. Omówiono sposób więcej w późniejszym części w tym samouczku robocze stron wzorcowych.

### <a name="implementing-the-details-view-template"></a>Wdrażanie szablonu widoku "Szczegóły"

Umożliwia teraz implementuje szablonie widok "Szczegóły" — dla pojedynczego modelu obiad wygeneruje HTML.

Firma Microsoft będzie w tym celu Pozycjonowanie naszych kursor tekstu w metodzie akcji szczegółowe informacje, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie "Dodaj widok" (lub naciśnij klawisze Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Zostanie wyświetlone okno dialogowe "Dodaj widok". Firma Microsoft będzie Zachowaj domyślną nazwę widoku ("szczegóły"). Firma Microsoft będzie także pole wyboru "Utwórz widok jednoznacznie" w oknie dialogowym i wybierz (przy użyciu listy rozwijanej pola kombi combobox) Nazwa typu modelu, który firma Microsoft przekazywane z kontrolera do widoku. Dla tego widoku możemy przekazywanie obiektu obiad (jest w pełni kwalifikowana nazwa tego typu: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

W przeciwieństwie do poprzednich szablonu, gdzie Wybraliśmy utworzyć "Pusty widok", tym razem wybierzemy do automatycznie "szkieletu" widoku przy użyciu szablonu "Szczegóły". Firma Microsoft może wskazywać, zmieniając rozwijanej "Wyświetl zawartość" w oknie dialogowym powyżej.

"Tworzenia szkieletu" wygeneruje początkowego stosowania naszych szablonu widoku szczegółów opartego na obiekt obiad, który mamy przekazywane do niego. To zapewnia prosty sposób firmie Microsoft szybko rozpocząć pracę na naszych widok szablonu wdrożenia.

Kliknięcie przycisku "Dodaj", Visual Studio utworzy nowy plik szablonu widoku "Details.aspx" dla nas w naszej katalogu "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Otworzy się również się naszego nowego szablonu widoku "Details.aspx" w edytorze kodu. Będzie zawierać implementację początkowej szkieletu widoku szczegółów opartą na modelu obiad. Aparat szkieletów używa odbicia .NET aby przyjrzeć się właściwości publiczne ujawnionymi w klasie przekazany go i spowoduje dodanie odpowiedniej zawartości na podstawie każdego typu, który odnajdzie:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Firma Microsoft może zażądać *"/ 1/szczegóły/kolacji"* adres URL, aby zobaczyć, jak wygląda tej implementacji szkieletu "szczegóły" w przeglądarce. Przy użyciu tego adresu URL wyświetli jeden z kolacji, możemy ręcznie dodane do naszej bazie danych podczas utworzyliśmy najpierw:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Pobiera nam szybkiego skonfigurowania i uruchomienia i zapewnia początkowego stosowania naszych widok Details.aspx. Firma Microsoft następnie przejdź i dostosować go do dostosowania interfejsu użytkownika do naszej zadowolenia użytkowników.

Jeśli przyjrzymy się szablon Details.aspx ściślej, możemy znajdziesz czy go zawiera Statycznych, a także osadzonych renderowania kodu. &lt;%%&gt; nuggets kod wykonanie kodu, gdy renderuje widok szablonu, i &lt;% = %&gt; nuggets kod wykonanie kodu zawarte w nich i renderowania wynik do strumienia wyjściowego szablonu.

W naszym widoku, który uzyskuje dostęp do obiektu modelu "Obiad", który został przekazany z kontrolera przy użyciu właściwości "Modelu" jednoznacznie, możemy napisać kod. Visual Studio zapewnia pełne intellisense kodu podczas uzyskiwania dostępu do tej właściwości "Modelu" edytora:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Upewnijmy niektórych ulepszeń, dzięki czemu źródła dla naszych ostateczny szablon widoku szczegółów wygląda jak poniżej:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Kiedy możemy uzyskać dostępu do *"/ 1/szczegóły/kolacji"* adres URL ponownie go zostanie teraz renderowania, takich jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementowanie "Index" Wyświetlanie szablonu

Umożliwia teraz wdrożyć szablon widoku "Index" — co spowoduje, że lista nadchodzących kolacji. Zadania do wykonania, firma Microsoft będzie umieść kursor naszych tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy kliknij przycisk Wybierz polecenie "Dodaj widok" (lub naciśnij klawisze Ctrl-M, Ctrl-V).

W oknie dialogowym "Dodaj widok" Nasz zachować Wyświetl szablon o nazwie "Index" i zaznacz pole wyboru "Utwórz widok jednoznacznie". Tym razem wybierzemy do automatycznego generowania szablonu widoku "List" i wybierz "NerdDinner.Models.Dinner" jako typu modelu przekazywane do widoku (który ponieważ został wskazany, tworzymy "List" szkieletu spowoduje, że okno dialogowe Dodaj widok do wniosku, możemy przekazywanie sekwencji obiad obiektów z kontrolera do widoku):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Kliknięcie przycisku "Dodaj", Visual Studio utworzy nowy plik szablonu widoku "Index.aspx" dla nas w naszej katalogu "\Views\Dinners". Go będzie "szkieletu" początkowej implementację znajdujące się w nim, która zawiera listę tabeli HTML kolacji jest przekazywana do widoku.

Firma Microsoft uruchamiania aplikacji i dostępu *"/ kolacji /"* będą zawierały naszej listy kolacji adresu URL w następujący sposób:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Rozwiązanie tabeli powyżej daje układ siatki naszych danych obiad — nie jest dość co chcemy dla naszych konsumenta ukierunkowane obiad listy. Możemy zaktualizować szablon widoku Index.aspx i zmodyfikuj go do listy mniejszą liczbę kolumn danych, a następnie użyj &lt;ul&gt; element do renderowania je zamiast tabeli za pomocą poniższy kod:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Użyto słowa kluczowego "var" w powyższych instrukcji foreach jako pętla za pośrednictwem każdej obiad w modelu. Te, które znają języka C# 3.0 wydaje się, że za pomocą "var" oznacza, że obiekt obiad późnym wiązaniem. Zamiast tego oznacza, że kompilator używa wnioskowanie o typie przed jednoznacznie właściwości "Modelu" (jest typu "IEnumerable&lt;obiad&gt;") i kompilowanie zmiennej lokalnej "obiad" jako typu obiad — oznacza, że możemy uzyskać pełnej funkcja IntelliSense i sprawdzanie wewnątrz bloków kodu kompilacji:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Podczas odświeżania możemy trafień */Dinners* adres URL do naszej przeglądarki naszych wygląda teraz zaktualizować widok jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Poszukuje lepsze —, ale nie jest całkowicie istnieje jeszcze. Nasze ostatnim krokiem jest umożliwienie użytkownikom końcowym nawiązywanie kliknij poszczególne kolacji na liście i szczegółowe informacje o nich. Firma Microsoft będzie implementuje przez renderowanie elementów hyperlink HTML, które łącze do metody akcji szczegółów na naszych DinnersController.

Firma Microsoft może wygenerować hiperłącza w naszym widoku indeksu w jeden z dwóch sposobów. Pierwsza to ręczne tworzenie HTML &lt;&gt; elementów, takich jak poniżej, gdzie możemy osadzić &lt;%%&gt; blokuje w &lt;&gt; elementu HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Informacje o innym podejściu możemy użyć jest przeprowadzać wbudowana metoda pomocnika "Html.ActionLink()" w ramach platformy ASP.NET MVC, który obsługuje programowe tworzenie kodu HTML &lt;&gt; element, który stanowi łącze do innej metody akcji na Kontroler:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Pierwszy parametr metody pomocnika Html.ActionLink() jest łącze tekst do wyświetlenia (w tym przypadku tytuł obiad), drugi parametr jest nazwa akcji kontrolera, chcemy, aby wygenerować łącze do (w tym przypadku metoda szczegóły), a trzeci parametr jest zestaw parametrów, aby wysłać do akcji (zaimplementowane jako anonimowy typ o wartości nazw właściwości). W takim przypadku jest określenie parametru "id" obiad możemy chcesz połączyć, a ponieważ routing domyślny adres URL reguły na platformie ASP.NET MVC jest "{Controller} / {Action} / {id}" metody pomocnika Html.ActionLink() spowoduje wygenerowanie następujących danych wyjściowych:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Nasze widoku Index.aspx możemy użyć metody metody pomocnika Html.ActionLink() i ma każdego obiad z listy łącza do adresu URL odpowiednie informacje:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

I teraz, kiedy możemy trafień */Dinners* adres URL z naszej listy obiad wygląda jak poniżej:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Po kliknięciu przycisku żadnego kolacji na liście firma Microsoft będzie przejdź do szczegółowe informacje o jego:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Opartych na konwencjach nazewnictwa i struktura katalogów \Views

Aplikacje programu ASP.NET MVC domyślnie używać katalogu opartych na konwencjach nazewnictwa struktury podczas rozpoznawania Wyświetl szablony. Dzięki temu deweloperzy mogą uniknąć konieczności pełnej kwalifikacji ścieżka lokalizacji podczas odwoływania się do widoków z wewnątrz klasy kontrolera. Domyślnie ASP.NET MVC będzie szukał pliku szablonu widoku w ramach * \Views\[ControllerName]\* katalogu poniżej aplikacji.

Na przykład możemy używane w klasie DinnersController — jawnie odwoływać się do trzech szablonów widoku: "Index", "Szczegóły" i "NotFound". ASP.NET MVC domyślnie sprawdza tych widokach w ramach *\Views\Dinners* katalogu poniżej naszych katalog główny aplikacji:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Nad jak występują obecnie są trzy klasy kontrolera w projekcie (DinnersController, HomeController i elementu AccountController — dodano ostatnie dwa domyślnie, gdy utworzono projekt), i czy istnieją trzy podkatalogów (po jednym dla każdego Kontroler) w katalogu \Views.

Widoki odwołuje się do kontrolerów dla użytkowników domowych i kont zostanie automatycznie rozwiązać ich wyświetlanie szablonów z odpowiednich *\Views\Home* i *\Views\Account* katalogów. *\Views\Shared* podkatalogu umożliwia przechowywanie szablonów widoków, które są ponownie używane przez wiele kontrolerów w aplikacji. Gdy ASP.NET MVC próbuje rozpoznać szablonu widoku, najpierw sprawdza w *\Views\[kontrolera]* określonego katalogu, i nie można znaleźć szablonu widoku zostanie ona wyszukana w *\Views\ Udostępnione* katalogu.

Po przejściu do nazw poszczególnych Wyświetl szablony, zalecane wskazówki jest Wyświetl szablon o tej samej nazwie jako metody akcji, który spowodował do renderowania. Na przykład powyżej naszych "Index" metody akcji jest korzystanie z widoku "Index" do renderowania wynik widoku, a metoda akcji "Szczegóły" używa widoku "Szczegóły" do renderowania jej wyników. Ułatwia to szybko wyświetlić szablon, który jest skojarzony z każdego działania.

Deweloperzy nie muszą jawnie określ nazwę szablonu widoku, gdy szablon widoku ma taką samą nazwę jak metody akcji, wywoływana na kontrolerze. Firma Microsoft tylko zamiast tego można przekazać obiekt modelu do metody pomocnika "View()" (bez określenia nazwy widoku) i ASP.NET MVC automatycznie wywnioskuje chcemy użyć *\Views\[ControllerName]\[Nazwa akcji]* szablon widoku na dysku, aby renderować ją.

Pozwala to nieco wyczyścić naszego kodu kontrolera i uniknąć duplikowania nazwa dwa razy w naszym kodzie:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Powyższy kod jest zgłaszać wszystkie, które są niezbędne do zaimplementowania nieuprzywilejowany listę obiad/szczegółów dla witryny.

#### <a name="next-step"></a>Następny krok

Mamy teraz nieuprzywilejowany obiad, przeglądania Internetu skompilowany.

Umożliwia teraz włączyć CRUD (tworzenia, odczytu, aktualizacji, usuwania) dane formularza edycji pomocy technicznej.

>[!div class="step-by-step"]
[Poprzednie](build-a-model-with-business-rule-validations.md)
[dalej](provide-crud-create-read-update-delete-data-form-entry-support.md)
