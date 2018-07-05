---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfejs użytkownika i nawigacja | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: f8570942c094edc0a2825613be634fbfb447b13c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394281"
---
<a name="ui-and-navigation"></a>Interfejs użytkownika i nawigacja
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku zmodyfikujesz Interfejsie domyślnej aplikacji sieci Web do obsługi funkcji aplikacji frontonu magazynu o nazwie Wingtip Toys. Ponadto należy dodać prosty i nawigacji powiązany z danymi. Ten samouczek opiera się na poprzednim samouczku "Tworzenie warstwy dostępu do danych" i jest częścią serii samouczków firmy Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak zmienić interfejs użytkownika do obsługi funkcji aplikacji frontonu magazynu o nazwie Wingtip Toys.
- Jak skonfigurować element HTML5 do uwzględnienia nawigacji na stronie.
- Jak utworzyć formant opartych na danych, aby przejść do określonego produktu danych.
- Jak wyświetlić dane z bazy danych utworzone za pomocą programu Entity Framework Code First.

Formularze sieci Web programu ASP.NET umożliwiają tworzenie dynamicznej zawartości dla aplikacji sieci Web. Każda strona sieci Web platformy ASP.NET jest tworzony w sposób podobny do statyczną stronę HTML w sieci Web (strona nie ma na serwerze przetwarzania), ale strona sieci Web platformy ASP.NET zawiera dodatkowe elementy, które platforma ASP.NET rozpoznaje i przetwarza do generowania kodu HTML, po uruchomieniu na stronie.

Przy użyciu statycznej strony HTML (*.htm* lub *.html* pliku), serwer spełnia `Web` żądania, podczas odczytywania pliku, a następnie wysyłając je jako — jest w przeglądarce. Natomiast gdy ktoś żąda na stronie sieci Web platformy ASP.NET (*.aspx* pliku), strona działa jako program na serwerze sieci Web. Gdy strona jest uruchomiona, można wykonywać wszystkie zadania, które wymaga witryny sieci Web, takich jak obliczanie wartości, odczytywania lub zapisywania informacji o bazie danych lub wywoływania innych programów. Jako dane wyjściowe strony dynamicznie tworzy znaczniki (na przykład elementy w formacie HTML) i wysyła te dynamiczne dane wyjściowe do przeglądarki.

## <a name="modifying-the-ui"></a>Modyfikowanie interfejsu użytkownika

Nadal będzie w tej serii samouczków, modyfikując *Default.aspx* strony. Należy zmodyfikować interfejs użytkownika, który jest już ustanowione przez domyślny szablon używany do tworzenia aplikacji. Typ zmiany, które należy wykonać są typowe podczas tworzenia dowolnej aplikacji formularzy sieci Web. Możesz to zrobić poprzez Zmienianie tytułu, zastępując części zawartości i usuwanie niepotrzebnych domyślnej zawartości.

1. Otwieranie programu lub przełączanie na *Default.aspx* strony.
2. Jeśli zostanie wyświetlona w **projektowania** wyświetlić, przejdź do **źródła** widoku.
3. W górnej części strony w `@Page` dyrektywy, zmiana `Title` atrybutu "Witaj", jak pokazano wyróżnione na żółto poniżej. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Ponadto na *Default.aspx* strony, Zamień wszystkie zawarte w domyślnej zawartości `<asp:Content>` tag, aby znaczniki pojawia się jako poniżej. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Zapisz *Default.aspx* strony, wybierając **Zapisz Default.aspx** z **pliku** menu.

   Wartość wynikowa *Default.aspx* zostanie wyświetlona następująca strona: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

W tym przykładzie ustawiono `Title` atrybutu `@Page` dyrektywy. Gdy kod HTML zostanie wyświetlona w przeglądarce, kod serwera `<%: Page.Title %>` jest rozpoznawana jako zawartość zawarte w `Title` atrybutu.

Przykładowa strona zawiera podstawowe elementy, składających się na stronie sieci Web platformy ASP.NET. Strona zawiera tekst statyczny, jak w przypadku korzystania na stronie HTML, wraz z elementów, które są specyficzne dla platformy ASP.NET. Zawartość zawarte w *Default.aspx* strony zostanie ona również zintegrowana z zawartością strony wzorcowej, które zostaną wyjaśnione w dalszej części tego samouczka.

### <a name="page-directive"></a>@Page — Dyrektywa

ASP.NET Web Forms zawierają zwykle dyrektyw, które pozwalają na określenie strony właściwości i informacje dotyczące konfiguracji dla strony. Dyrektywy są używane przez program ASP.NET jako instrukcje dotyczące sposobu przetwarzania strony, ale nie są renderowane jako część kod znaczników, który jest wysyłany do przeglądarki.

Najczęściej używane dyrektywy jest `@Page` dyrektywy, dzięki czemu można określić wiele opcji konfiguracji dla tej strony, w tym następujące:

1. Serwer języka programowania dla kodu w stronę, na przykład C#.
2. Czy jest strona z kodu serwera bezpośrednio w stronę, zostanie wywołana strona pojedynczy plik, lub czy jest to strona, przy użyciu kodu w pliku osobnej klasy, która nosi nazwę strony związanym z kodem.
3. Czy strona ma skojarzone stronę wzorcową i dlatego powinien być traktowany jako strony zawartości.
4. Debugowanie i śledzenie opcje.

Jeśli nie zostanie uwzględniony `@Page` dyrektywy na stronie, lub jeśli dyrektywa nie zawiera określonego ustawienia, ustawienia zostaną odziedziczone z *Web.config* pliku konfiguracji lub *Machine.config* pliku konfiguracji. *Machine.config* plik zawiera ustawienia dodatkowe czynności konfiguracyjne, aby wszystkie aplikacje uruchomione na maszynie.

> [!NOTE] 
> 
> *Machine.config* zawiera także szczegółowe informacje dotyczące wszystkich możliwych konfiguracji ustawień.


### <a name="web-server-controls"></a>Formanty serwera sieci Web

W większości aplikacji formularzy sieci Web ASP.NET należy dodać formanty, które umożliwiają użytkownikowi interakcję ze strony, takie jak przyciski, pola tekstowe, listy i tak dalej. Te formanty serwera sieci Web są podobne do przycisków HTML i elementów wejściowych. Jednakże są przetwarzane na serwerze, dzięki czemu możesz ustawiać ich właściwości za pomocą kodu serwera. Te kontrolki także wywoływać zdarzenia, które może obsłużyć w kodzie serwera.

Formanty serwera użycia specjalnej składni, która ASP.NET rozpoznaje po uruchomieniu na stronie. Nazwa tagu dla formantów serwera ASP.NET, który rozpoczyna się od `asp:` prefiks. Dzięki temu ASP.NET rozpoznaje i przetwarzać te formanty serwera. Prefiks może się różnić, jeśli formant nie jest częścią programu .NET Framework. Oprócz `asp:` również obejmować prefiks, formanty serwera ASP.NET `runat="server"` atrybutu i `ID` której można odwoływać się do kontrolki w kodzie serwera.

Po uruchomieniu strony ASP.NET identyfikuje formantów serwera i uruchamia kod, który jest skojarzony z tych kontrolek. Wiele kontrolek Renderuj kod HTML lub innych znaczników do strony, gdy jest on wyświetlany w przeglądarce.

### <a name="server-code"></a>Kod serwera

Większość aplikacji formularzy sieci Web ASP.NET zawiera kod, który działa na serwerze podczas przetwarzania strony. Jak wspomniano powyżej, kod serwera można wykonywać różne czynności, takich jak dodawanie danych do kontrolki ListView. Program ASP.NET obsługuje wiele języków do uruchomienia na serwerze, w tym C#, Visual Basic, J# i innych.

Program ASP.NET obsługuje dwa modele do pisania kodu serwera dla strony sieci Web. W modelu pojedynczego pliku kod strony znajduje się w elemencie script uwzględniającym otwierający tag `runat="server"` atrybutu. Alternatywnie można utworzyć kod dla strony w pliku osobnej klasy jest określany jako model związanym z kodem. W tym przypadku strony formularzy sieci Web platformy ASP.NET ogólnie nie zawiera serwera kodu. Zamiast tego `@Page` dyrektywy zawierają informacje, które łączy *.aspx* stronę z jego pliku skojarzone związanym z kodem.

`CodeBehind` Zawartych w atrybucie `@Page` dyrektywa określa nazwę pliku osobnej klasy i `Inherits` atrybut określa nazwę klasy w pliku związanym z kodem, który odnosi się do strony.

### <a name="updating-the-master-page"></a>Aktualizowanie strony wzorcowej

W programie ASP.NET Web Forms strony wzorcowe umożliwiają tworzenie spójnego układu dla strony w aplikacji. Jednej strony wzorcowej definiuje wygląd i zachowanie standardowe dla wszystkich stron (lub grupy stron) w aplikacji. Następnie można utworzyć poszczególne strony zawartości, które zawierają zawartość, którą chcesz wyświetlić, jak wyjaśniono powyżej. Jeśli użytkownicy zażądają zawartości stron, ASP.NET scala je ze stroną wzorcową w celu wygenerowania danych wyjściowych, który łączy układ strony wzorcowej z zawartości z poziomu strony zawartości.

Nowa witryna wymaga pojedynczego logo do wyświetlenia na każdej stronie. Aby dodać tego logo, można zmodyfikować HTML na stronie wzorcowej.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie **Site.Master** strony.
2. Jeśli strona znajduje się w **projektowania** wyświetlić, przejdź do **źródła** widoku.
3. Zaktualizuj strony wzorcowej przez **zmodyfikowanie lub dodanie** znaczników wyróżniony na żółto: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Kod HTML wyświetli obraz o nazwie *logo.jpg* z *obrazów* folder aplikacji sieci Web należy dodać później. Gdy strona, która korzysta z strony wzorcowej jest wyświetlana w przeglądarce, pojawi się logo. Jeśli użytkownik kliknie logo, użytkownik będzie przejdź z powrotem do *Default.aspx* strony. Tag kotwicy HTML `<a>` opakowuje formant serwera obrazu i umożliwia obraz, który ma być dołączane jako część łącza. `href` Atrybutu dla tagu kotwicy określa główny "`~/`" witryny sieci Web jako lokalizacja linku. Domyślnie *Default.aspx* strona jest wyświetlana, gdy użytkownik przechodzi do katalogu głównego witryny sieci Web. **Obraz** `<asp:Image>` kontrolki serwera obejmuje dodatkowe właściwości, takie jak `BorderStyle`, który renderowane jako HTML podczas wyświetlania w przeglądarce.

### <a name="master-pages"></a>Strony wzorcowe

Strony wzorcowej jest plikiem programu ASP.NET przy użyciu rozszerzenia master (na przykład *Site.Master*) przy użyciu wstępnie zdefiniowanego układu, zawierających tekst statyczny, elementy HTML i formantów serwera. Strona główna jest identyfikowany przez specjalny `@Master` dyrektywy, która zastępuje `@Page` dyrektywę, który jest używany dla zwykłych *.aspx* stron.

Oprócz `@Master` dyrektywy, strony wzorcowej również zawiera wszystkie najwyższego poziomu elementy HTML strony, takich jak `html`, `head`, i `form`. Na przykład na stronie wzorcowej dodanych wcześniej, możesz użyć kodu HTML `table` układu, `img` element logo firmy, tekst statyczny i formantów serwera do obsługi wspólnych członkostwa dla danej witryny. Kod HTML i elementy programu ASP.NET można użyć jako część strony wzorcowej.

Oprócz tekstu statycznego i kontrolek, które pojawią się na wszystkich stronach strony wzorcowej również zawiera co najmniej jedną **ContentPlaceHolder** kontrolki. Te kontrolki symbolu zastępczego zdefiniować regionach, gdzie pojawią się wymienne zawartości. Z kolei wymienne zawartości jest zdefiniowany na stronach zawartości, takich jak *Default.aspx*przy użyciu **zawartości** kontrolki serwera.

#### <a name="adding-image-files"></a>Trwa dodawanie plików obrazu

Obraz logo, do którego odwołuje się powyżej, wraz z wszystkie obrazy produktów, należy dodać do aplikacji sieci Web tak, aby można je przeglądać, gdy projekt jest wyświetlany w przeglądarce.

#### <a name="download-from-msdn-samples-site"></a>Pobierz z witryny MSDN próbek:

[Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Pliki do pobrania zawiera zasoby w *zasoby WingtipToys* folderu, które są używane do tworzenia przykładowej aplikacji.

1. Jeśli nie zostało to jeszcze zrobione, Pobierz przykładowe skompresowane pliki przy użyciu powyższego linku z witryny MSDN przykładów.
2. Po pobraniu, otwórz plik zip, a następnie skopiuj zawartość do folderu lokalnego na komputerze.
3. Znajdowanie i otwieranie *zasoby WingtipToys* folderu.
4. Przeciągając i upuszczając, skopiuj *katalogu* folderu w lokalnym folderze w katalogu głównym projektu aplikacji sieci Web w **Eksploratora rozwiązań** programu Visual Studio. 

    ![Interfejs użytkownika i nawigacja — kopiowanie plików](ui_and_navigation/_static/image1.png)
5. Następnie utwórz nowy folder o nazwie *obrazów* przez kliknięcie prawym przyciskiem myszy **WingtipToys** projektu w **Eksploratora rozwiązań** i wybierając polecenie **Dodaj**  - &gt; **Nowy Folder**.
6. Kopiuj *logo.jpg* plik wchodzącej w skład *WingtipToys zasoby* folderu w **Eksploratora plików** do *obrazów* folder aplikacji sieci Web Projekt w **Eksploratora rozwiązań** programu Visual Studio.
7. Kliknij przycisk **Pokaż wszystkie pliki** opcji w górnej części **Eksploratora rozwiązań** można zaktualizować listy plików, jeśli nie widzisz nowe pliki.  
  
    **Eksplorator rozwiązań** pojawią się pliki zaktualizowanego projektu. 

    ![Interfejs użytkownika i nawigacja — Eksplorator rozwiązań](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Dodawanie stron

Przed dodaniem nawigacji do aplikacji sieci Web, należy najpierw dodać dwóch nowych stron, które będzie przejdź do. W dalszej części tej serii samouczków będą wyświetlane na tych stronach nowych produktów i szczegółowe informacje.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.   
 **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie wybierz **formularz sieci Web ze stroną wzorcową** ze środka listy i nadaj mu nazwę *ProductList.aspx*. 

    ![Interfejs użytkownika i nawigacja — Dodaj okno dialogowe nowego elementu](ui_and_navigation/_static/image3.png)
3. Wybierz **Site.Master** można dołączyć strony wzorcowej do nowo utworzonego *.aspx* strony. 

    ![Interfejs użytkownika i Nawigacja — wybierz stronę wzorcową](ui_and_navigation/_static/image4.png)
4. Dodawanie dodatkowych strony o nazwie *ProductDetails.aspx* wykonując te same kroki.

### <a name="updating-bootstrap"></a>Aktualizacja ładowania początkowego

Szablony projektu Visual Studio 2013 korzystają [Bootstrap](http://getbootstrap.com/), układ i motywów framework, utworzone przez usługi Twitter. Usługa ładowania początkowego używa CSS3, aby zapewnić elastyczne, co oznacza, że układy dynamicznie dostosowują się do innej przeglądarki rozmiary okna. Można również użyć funkcji motywów Bootstrap firmy, można łatwo dokonać zmian w aplikacji wyglądu i działania. Domyślnie szablon aplikacji sieci Web ASP.NET w programie Visual Studio 2013 zawiera narzędzia Bootstrap jako pakiet NuGet.

W tym samouczku zostanie zmienić wygląd i działanie aplikacji Wingtip Toys, zastępując pliki CSS szablonu Bootstrap.

1. W **Eksploratora rozwiązań**, otwórz *zawartości* folderu.
2. Kliknij prawym przyciskiem myszy *bootstrap.css* pliku i zmień jej nazwę na *bootstrap original.css*.
3. Zmień nazwę *bootstrap.min.css* do *bootstrap original.min.css*.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *zawartości* i wybierz polecenie **Otwórz Folder w Eksploratorze plików**.  
   Wyświetlany jest Eksplorator plików. Pobrane pliki CSS bootstrap zostanie zapisane w tej lokalizacji.
5. W przeglądarce przejdź do [ http://Bootswatch.com ](http://bootswatch.com/).
6. Przewiń okno przeglądarki, aż zobaczysz Cerulean motywu. 

    ![Interfejs użytkownika i nawigacja — Cerulean motywu](ui_and_navigation/_static/image5.png)
7. Pobierz zarówno *bootstrap.css* pliku i *bootstrap.min.css* plik *zawartości* folderu. Użyj ścieżki do folderu zawartości, która jest wyświetlana w **Eksploratora plików** okna, który został wcześniej otwarty.
8. W **programu Visual Studio** w górnej części **Eksploratora rozwiązań**, wybierz opcję **Pokaż wszystkie pliki** opcję, aby wyświetlić nowe pliki w folderze zawartości. 

    ![Interfejs użytkownika i nawigacja — Eksplorator rozwiązań](ui_and_navigation/_static/image6.png)

   Zobaczysz dwa nowe pliki CSS w **zawartości** folder, ale należy zauważyć, że ikona obok nazwy każdego pliku jest wyszarzona. Oznacza to, że plik nie został jeszcze dodany do projektu.
9. Kliknij prawym przyciskiem myszy *bootstrap.css* i *bootstrap.min.css* pliki i wybierz pozycję **załącz do projektu**.   
   Po uruchomieniu aplikacji Wingtip Toys w dalszej części tego samouczka, pojawi się nowy interfejs użytkownika.

> [!NOTE] 
> 
> Szablon aplikacji sieci Web platformy ASP.NET używa *Bundle.config* pliku w katalogu głównym projektu do przechowywania ścieżki plików CSS szablonu Bootstrap.


### <a name="modifying-the-default-navigation"></a>Modyfikowanie nawigacji domyślne

Nawigacja domyślne dla każdej strony w aplikacji można modyfikować, zmieniając element listy nieuporządkowane nawigacji, który znajduje się w *Site.Master* strony.

1. W **Eksploratora rozwiązań**Znajdź i Otwórz *Site.Master* strony.
2. Dodaj łącze nawigacyjne wyróżniony na żółto do listy nieuporządkowane, pokazano poniżej:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Jak widać w powyższym kodzie HTML, został zmodyfikowany każdego elementu wiersza `<li>` zawierający tag kotwicy `<a>` z linkiem umożliwiającym `href` atrybutu. Każdy `href` wskazuje stronę w aplikacji sieci Web. W przeglądarce, gdy użytkownik kliknie na jednym z poniższych linków (takie jak **produktów**), ich spowoduje przejście do strony zawarte w `href` (takie jak **ProductList.aspx**). Uruchom aplikację na końcu tego samouczka.

> [!NOTE] 
> 
> Tyldy (`~`) znak jest używany do określenia, że `href` ścieżka rozpoczyna się w katalogu głównym projektu.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Dodawanie kontrolki danych do wyświetlania danych nawigacji

Następnie dodasz formant, aby wyświetlić wszystkie kategorie z bazy danych. Każda kategoria będzie działał jako link do *ProductList.aspx* strony. Gdy użytkownik kliknie łącze kategorii w przeglądarce, spowoduje przejdź do strony produktów i wyświetlić tylko produkty, które są skojarzone z wybraną kategorię.

Użyjesz **ListView** formantu, aby wyświetlić wszystkie kategorie, które są zawarte w bazie danych. Aby dodać **ListView** kontrolki na stronie głównej:

1. W *Site.Master* strony, Dodaj następujący wyróżniony `<div>` elementu **po** `<div>` zawierający element `id="TitleContent"` dodanego wcześniej:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Ten kod będą wyświetlane wszystkie kategorie z bazy danych. **ListView** kontroli Wyświetla nazwę każdej kategorii jako tekst łącza i zawiera łącza do *ProductList.aspx* strony zawierający wartość ciągu zapytania `ID` kategorii. Ustawiając `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępna w ramach `ItemTemplate` węzła i kontrolka staje się silnie typizowane. Możesz wybrać szczegóły `Item` przy użyciu technologii IntelliSense, takich jak określanie `CategoryName`. Ten kod znajduje się w kontenerze `<%#: %>` , który oznacza wyrażenie powiązanie danych. Dodając (:) na końcu `<%#` prefiks, wynik wyrażenia wiązania danych jest zakodowany w formacie HTML. Gdy wynik jest zakodowany w formacie HTML, aplikacja jest lepiej chroniony przed między lokacjami skrypt (XSS iniekcji) i ataki przez iniekcję kodu HTML.

> [!NOTE] 
> 
> **Porada**
> 
> Po dodaniu kodu, wpisując podczas projektowania może mieć pewność, że prawidłowym elementem członkowskim obiektu zostanie znaleziony ponieważ silnie typizowane kontrolki danych pokazać dostępne elementy członkowskie, oparte na technologii IntelliSense. Funkcja IntelliSense daje szeroki wybór odpowiednich kontekst kodu podczas pisania kodu, takie jak właściwości, metod i obiektów.


W następnym kroku zostaną zaimplementowane `GetCategories` metody do pobierania danych.

### <a name="linking-the-data-control-to-the-database"></a>Łączenie kontrolki danych do bazy danych

Można było wyświetlać dane w formancie danych, musisz utworzyć link kontrolki danych w bazie danych. Aby utworzyć łącze, można zmodyfikować kod związany z *Site.Master.cs* pliku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Site.Master* strony, a następnie kliknij przycisk **Wyświetl kod**. *Site.Master.cs* plik jest otwarty w edytorze.
2. W pobliżu początku *Site.Master.cs* Dodaj dwa dodatkowe przestrzenie nazw, dzięki czemu wszystkie przestrzenie nazw uwzględniane są wyświetlane w następujący sposób:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Dodaj wyróżnione `GetCategories` metody `Page_Load` program obsługi zdarzeń w następujący sposób:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Powyższy kod jest wykonywany po załadowaniu każdej strony, używający strony wzorcowej w przeglądarce. `ListView` Formantu (o nazwie "categoryList"), który dodano wcześniej w tym samouczku użyto powiązanie modelu, aby wybrać dane. W znaczniku elementu `ListView` kontrolki ustawisz formantu `SelectMethod` właściwość `GetCategories` metody przedstawionych powyżej. `ListView` Kontrolować wywołania `GetCategories` metody w odpowiednim czasie życia strony cyklu i automatycznie wiąże zwracanych danych. Więcej informacji na temat wiązania danych w następnym samouczku przedstawiono.

### <a name="running-the-application-and-creating-the-database"></a>Uruchamianie aplikacji i utworzeniu bazy danych

We wcześniejszej części tej serii samouczków zostały utworzone klasy inicjatora (o nazwie "ProductDatabaseInitializer") i określone tej klasy w *global.asax.cs* pliku. Entity Framework wygeneruje bazy danych, gdy aplikacja jest uruchamiana po raz pierwszy, ponieważ `Application_Start` metoda zawarte w *global.asax.cs* pliku będzie wywoływać klasy inicjatora. Klasa inicjatora będzie używać klasy modelu (`Category` i `Product`) dodano we wcześniejszej części tej serii samouczków, aby utworzyć bazę danych.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* strony i wybierz **Ustaw jako stronę startową**.
2. W programie Visual Studio naciśnij klawisz **F5**.   
 Upłynąć trochę czasu, aby skonfigurować wszystko w tym pierwszego uruchomienia.   
    ![Interfejs użytkownika i nawigacja — Windows przeglądarki](ui_and_navigation/_static/image7.png)  
 Po uruchomieniu aplikacji, aplikacja zostanie skompilowany i bazy danych o nazwie *wingtiptoys.mdf* zostaną utworzone w *aplikacji\_danych* folderu. W przeglądarce pojawi się menu nawigacji kategorii. To menu został wygenerowany przez pobranie kategorii z bazy danych. W następnym samouczku wdroży nawigacji.
3. Zamknij przeglądarkę, aby zatrzymać działającą aplikację.

### <a name="reviewing-the-database"></a>Przegląd bazy danych

Otwórz *Web.config* plik i przyjrzyj się w sekcji parametrów połączenia. Możesz zobaczyć, że `AttachDbFilename` wskazuje wartość w parametrach połączenia `DataDirectory` dla projektu aplikacji sieci Web. Wartość `|DataDirectory|` jest zastrzeżony wartość, która reprezentuje *aplikacji\_danych* folderu w projekcie. Ten folder jest, gdzie znajduje się baza danych, który został utworzony z klas jednostek.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Jeśli *aplikacji\_danych* nie jest widoczny folder lub folder jest pusty, wybierz opcję **Odśwież** ikonę i następnie **Pokaż wszystkie pliki** ikonę u góry **Eksploratora rozwiązań** okna. Rozwijanie szerokość **Eksploratora rozwiązań** systemu windows może być wymagane do wyświetlenia wszystkich dostępnych ikon.


Teraz możesz sprawdzić dane zawarte w *wingtiptoys.mdf* plik bazy danych przy użyciu **Eksploratora serwera** okna.

1. Rozwiń *aplikacji\_danych* folderu. Jeśli *aplikacji\_danych* folder nie jest widoczne, zobacz uwagi powyżej.
2. Jeśli *wingtiptoys.mdf* plik bazy danych nie jest widoczny, wybierz opcję **Odśwież** ikonę i następnie **Pokaż wszystkie pliki** ikony w górnej części **Eksploratora rozwiązań**  okna.
3. Kliknij prawym przyciskiem myszy *wingtiptoys.mdf* plik bazy danych i wybierz pozycję **Otwórz**.  
    **Eksplorator serwera** jest wyświetlana. 

    ![Interfejs użytkownika i nawigacja — Eksploratora serwera](ui_and_navigation/_static/image8.png)
4. Rozwiń *tabel* folderu.
5. Kliknij prawym przyciskiem myszy **produktów**tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.  
 **Produktów** tabela jest wyświetlana. 

    ![Interfejs użytkownika i nawigacja — Tabela produktów](ui_and_navigation/_static/image9.png)
6. Ten widok umożliwia wyświetlić i modyfikować dane w **produktów** tabeli ręcznie.
7. Zamknij **produktów** okna tabeli.
8. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **produktów** tabeli ponownie, a następnie wybierz pozycję **Otwórz definicję tabeli**.  
 Dane projektu dla **produktów** tabela jest wyświetlana. 

    ![Interfejs użytkownika i nawigacja — projektowanie produktów](ui_and_navigation/_static/image10.png)
9. W **języka T-SQL** kartę zobaczysz instrukcji SQL DDL, który został użyty do utworzenia tabeli. Możesz również użyć interfejsu użytkownika w **projektowania** kartę do modyfikacji schematu.
10. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **WingtipToys** bazy danych, a następnie wybierz pozycję **bliskie połączenie**.   
 Schemat bazy danych odłączając bazę danych z programu Visual Studio, będzie można zmodyfikować w dalszej części tej serii samouczków.
11. Wróć do **Eksploratora rozwiązań**, wybierając **Eksploratora rozwiązań** karta w dolnej części **Eksploratora serwera** okna.

## <a name="summary"></a>Podsumowanie

W tym samouczku tej serii dodano podstawowy interfejs użytkownika, grafiki, strony i nawigacji. Ponadto uruchomiono aplikację sieci Web, która tworzenia bazy danych z klas danych, które zostały dodane w poprzednim samouczku. Możesz również wyświetlić zawartość *produktów* tabeli bazy danych, wyświetlając bazy danych bezpośrednio. W następnym samouczku możesz wyświetlić elementy danych i uzyskać szczegółowe informacje z bazy danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wprowadzenie do programowania stron ASP.NET Web Pages](https://msdn.microsoft.com/library/ms178125.aspx)   
[Serwer sieci Web ASP.NET kontrolki — omówienie](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Samouczek CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Poprzednie](create_the_data_access_layer.md)
> [dalej](display_data_items_and_details.md)
