---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfejs użytkownika i nawigacja | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: d2d4101455a85c53e016e567c0cf1337642f1863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="ui-and-navigation"></a>Interfejs użytkownika i nawigacja
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


W tym samouczku należy zmodyfikować Interfejsie domyślnej aplikacji sieci Web do obsługi funkcji aplikacji frontonu magazynu Wingtip Toys. Ponadto doda proste i nawigacji powiązany z danymi. W tym samouczku jest oparty na poprzednich samouczek "Tworzenie Warstwa dostępu do danych" i jest częścią serii samouczek Wingtip Toys.

## <a name="what-youll-learn"></a>Zawartość:

- Jak zmienić interfejsu użytkownika do obsługi funkcji aplikacji frontonu magazynu Wingtip Toys.
- Jak skonfigurować element HTML5 uwzględnienie nawigacji strony.
- Jak utworzyć opartych na danych formantu można przejść do określonego produktu danych.
- Jak wyświetlać dane z bazy danych utworzonej przy użyciu programu Entity Framework Code First.

Formularze sieci Web programu ASP.NET umożliwiają tworzenie dynamicznej zawartości dla aplikacji sieci Web. Każdej strony sieci Web programu ASP.NET jest tworzony w sposób podobny do statycznego HTML strony (strona nie ma na serwerze przetwarzania), ale strona sieci Web platformy ASP.NET zawiera dodatkowe elementy, które platforma ASP.NET rozpoznaje i przetwarza do generowania kodu HTML, po uruchomieniu na stronie.

Za pomocą statycznych strony HTML (*.htm* lub *.html* pliku), serwer spełnia `Web` żądanie odczytu pliku i wysłanie go jako — jest w przeglądarce. Z kolei, gdy ktoś żąda strony sieci Web platformy ASP.NET (*.aspx* pliku), strona działa jako program na serwerze sieci Web. Po uruchomieniu strony może przeprowadzać wszystkie zadania, które wymaga witryny sieci Web, takie jak obliczanie wartości, odczytywania lub zapisywania informacji o bazie danych lub wywołaniem innych programów. Jako dane wyjściowe strony dynamicznie tworzy znaczników (takich jak elementy w formacie HTML) i wysyła ten dynamicznych danych wyjściowych do przeglądarki.

## <a name="modifying-the-ui"></a>Modyfikowanie interfejsu użytkownika

Ten samouczek serii nastąpi przejście, modyfikując *Default.aspx* strony. Należy zmodyfikować interfejsu użytkownika, który jest już ustanowiona przez domyślny szablon używany do tworzenia aplikacji. Typ zmiany, które należy wykonać są typowe podczas tworzenia żadnych aplikacji formularzy sieci Web. Należy to zrobić to zmiana nazwy, zastąpienie części zawartości i usuwając zbędne domyślnej zawartości.

1. Otwieranie programu lub przełączanie do *Default.aspx* strony.
2. Jeśli strona jest wyświetlana w **projekt** wyświetlić, przełącz się do **źródła** widoku.
3. W górnej części strony w `@Page` dyrektywy, zmień `Title` atrybutu "Witaj", jak pokazano wyróżnionych kolorem żółtym poniżej. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Ponadto na *Default.aspx* strony, Zamień wszystkie zawarte w domyślnych zawartości `<asp:Content>` tag, tak aby znaczników jest wyświetlany jako poniżej. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Zapisz *Default.aspx* strony, wybierając **zapisać Default.aspx** z **pliku** menu.

   Powstałe w ten sposób *Default.aspx* strony będzie wyglądać następująco: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

W tym przykładzie ustawiono `Title` atrybutu `@Page` dyrektywy. Gdy HTML jest wyświetlany w przeglądarce, kod serwera `<%: Page.Title %>` jest rozpoznawana jako zawartość w `Title` atrybutu.

Przykładowa strona zawiera podstawowe elementy, które stanowią strony sieci Web ASP.NET. Strona zawiera tekst statyczny, które masz na stronie HTML, wraz z elementów, które są specyficzne dla platformy ASP.NET. Zawartość w *Default.aspx* zintegrowania strony z zawartością strony wzorcowej, które opisano w dalszej części tego samouczka.

### <a name="page-directive"></a>@Page Dyrektywy

Formularze sieci Web ASP.NET zazwyczaj zawiera dyrektywy, które umożliwiają określenie stronę właściwości i informacje konfiguracyjne dla strony. Dyrektywy są używane przez program ASP.NET jako instrukcje dotyczące sposobu przetwarzania strony, ale nie są renderowane jako część kod znaczników, który jest wysyłany do przeglądarki.

Dyrektywa najczęściej używane jest `@Page` dyrektywy, dzięki czemu można określić wiele opcji konfiguracji dla strony, takie jak:

1. Serwer programowania języka dla kodu na stronie, takich jak C#.
2. Określa, czy jest strona z kodu serwera bezpośrednio na stronie, której zostanie wywołana strona pojedynczego pliku, lub czy jest to strona kodu z pliku osobnej klasy, nosi nazwę strony związane z kodem.
3. Określa, czy strona ma skojarzone strony wzorcowej i dlatego powinien być traktowany jako strony zawartość.
4. Debugowanie i śledzenie opcje.

Jeśli nie zostanie uwzględniony `@Page` dyrektywy na stronie, lub jeśli dyrektywa nie zawiera określonego ustawienia, ustawienia będą dziedziczone z *Web.config* pliku konfiguracji lub z *Machine.config* pliku konfiguracji. *Machine.config* pliku udostępnia dodatkowe ustawienia konfiguracji do wszystkich aplikacji uruchomionych na komputerze.

> [!NOTE] 
> 
> *Machine.config* także szczegółowe informacje na temat wszystkich możliwych konfiguracji ustawień.


### <a name="web-server-controls"></a>Formanty serwera sieci Web

W większości aplikacji formularzy sieci Web ASP.NET należy dodać formanty, które umożliwiają użytkownikom interakcję ze stroną, takie jak przyciski, pola tekstowe, listy i tak dalej. Te kontrolki serwera sieci Web są podobne do przycisków HTML i elementów wejściowych. Jednak są przetwarzane na serwerze, dzięki czemu można użyć kodu serwera w celu ustawienia ich właściwości. Formanty też wiązać się zdarzenia, które może obsłużyć w kodzie serwera.

Formanty serwera należy użyć składni specjalne, które ASP.NET rozpoznaje po uruchomieniu na stronie. Nazwa tagu kontrolek serwera ASP.NET, który rozpoczyna się od `asp:` prefiks. Dzięki temu ASP.NET rozpoznaje i przetworzenie tych kontrolek serwera. Prefiks mogą się różnić, jeśli formant nie jest częścią programu .NET Framework. Oprócz `asp:` prefiksu, kontrolek serwera ASP.NET również obejmować `runat="server"` atrybutu i `ID` której można odwoływać się do formantu w kodzie serwera.

Po uruchomieniu strony ASP.NET identyfikuje kontrolki serwera i uruchamia kod, który jest skojarzony z tych kontrolek. Wiele formantów renderować kod HTML lub innych znaczników na stronie wyświetlanym w przeglądarce.

### <a name="server-code"></a>Kod serwera

Większość aplikacji składnika ASP.NET Web Forms obejmują kod, który działa na serwerze podczas przetwarzania strony. Jak wspomniano powyżej, kod serwera można wykonywać różne czynności, takich jak dodawanie danych do formantu ListView. Obsługuje wiele języków do uruchomienia na serwerze, w tym C#, Visual Basic, J# i inne.

Program ASP.NET obsługuje dwa modele dotyczące pisania kodu serwera dla strony sieci Web. W modelu pojedynczego pliku kodu strony jest w elemencie skryptu, gdy zawiera otwierający tag `runat="server"` atrybutu. Można również utworzyć kod dla strony w pliku osobnej klasy, nosi nazwę modelu związane z kodem. W takim przypadku strony formularzy sieci Web ASP.NET zazwyczaj nie zawiera serwera kodu. Zamiast tego `@Page` dyrektywy zawiera informacje, które łączy *.aspx* strony z plikiem jego skojarzony kodem.

`CodeBehind` Zawartych w atrybucie `@Page` dyrektywa określa nazwę pliku osobnej klasy i `Inherits` atrybut określa nazwę klasy w pliku CodeBehind, który odpowiada strony.

### <a name="updating-the-master-page"></a>Aktualizowanie strony wzorcowej

W formularzach sieci Web ASP.NET stron wzorcowych pozwala na tworzenie spójnego układu dla strony w aplikacji. Jednej strony wzorcowej definiuje wygląd i działanie i standardowe zachowanie dla wszystkich stron (lub grupę stron) w aplikacji. Następnie możesz utworzyć pojedynczych stron z zawartością z zawartością, który chcesz wyświetlić, jak opisano powyżej. Gdy użytkownik zażąda strony z zawartością, ASP.NET scala te strony wzorcowej do generowania danych wyjściowych, łączącą układ strony wzorcowej z zawartością ze strony zawartość.

Nowa lokacja musi mieć pojedynczy logo, które mają być wyświetlane na każdej stronie. Aby dodać to logo, można zmodyfikować HTML na stronie głównej.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie **Site.Master** strony.
2. Jeśli strona jest w **projekt** wyświetlić, przełącz się do **źródła** widoku.
3. Zaktualizuj strony wzorcowej przez **zmodyfikowanie lub dodanie** znaczników wyróżnione kolorem żółtym: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Kod HTML będzie wyświetlana obrazu o nazwie *logo.jpg* z *obrazów* folderu aplikacji sieci Web, który zostanie dodany później. Po wyświetleniu strony, która używa strony wzorcowej w przeglądarce pojawi się logo. Gdy użytkownik kliknie logo, użytkownik będzie Przejdź wstecz do *Default.aspx* strony. Tag kotwicy HTML `<a>` opakowuje formantu obrazu serwera i umożliwia obraz, który ma być dołączane jako część łącza. `href` Atrybutu tag kotwicy określa główny "`~/`" witryny sieci Web jako lokalizacji łącza. Domyślnie *Default.aspx* strona jest wyświetlana, gdy użytkownik przechodzi do katalogu głównego witryny sieci Web. **Obrazu** `<asp:Image>` kontrolki serwera obejmuje dodanie właściwości, takie jak `BorderStyle`, który renderowane jako HTML podczas wyświetlania w przeglądarce.

### <a name="master-pages"></a>Strony wzorcowej

Strona wzorcowa jest plikiem ASP.NET z rozszerzenie .master (na przykład *Site.Master*) z układem wstępnie zdefiniowane, zawierających tekst statyczny, elementów HTML i kontrolki serwera. Strony wzorcowej jest identyfikowany przez specjalnego `@Master` dyrektywy, które zastępuje `@Page` dyrektywy służący do zwykłych *.aspx* stron.

Oprócz `@Master` dyrektywy, strony wzorcowej zawiera również wszystkich elementów najwyższego poziomu HTML strony, takich jak `html`, `head`, i `form`. Na przykład na stronie głównej dodanych wcześniej, musisz użyć kodu HTML `table` dla układu, `img` element firmowego logo, tekst statyczny i kontrolki serwera do obsługi wspólnej członkostwa dla witryny. Kod HTML i elementy programu ASP.NET można użyć jako części strony wzorcowej.

Oprócz tekst statyczny oraz formantów, które pojawią się na wszystkie strony, strony wzorcowej również zawiera co najmniej jedną **ContentPlaceHolder** kontrolki. Te kontrolki symbolu zastępczego zdefiniuj regionach, gdzie replaceable zawartość będzie wyświetlana. Z kolei replaceable zawartości jest zdefiniowany w strony zawartości, takich jak *Default.aspx*za pomocą **zawartości** kontrolki serwera.

#### <a name="adding-image-files"></a>Dodawanie plików obrazów

Obraz logo, do którego odwołuje się powyżej, oraz wszystkie obrazy produktu, należy dodać do aplikacji sieci Web tak, aby można je przeglądać, gdy projekt jest wyświetlany w przeglądarce.

#### <a name="download-from-msdn-samples-site"></a>Pobrać z witryny MSDN próbek:

[Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Pobieranie obejmuje zasoby w *zasoby WingtipToys* folderu, które są używane do tworzenia przykładowej aplikacji.

1. Jeśli nie zostało to jeszcze zrobione, Pobierz przykładowe skompresowane pliki przy użyciu powyższego łącza z witryny MSDN próbek.
2. Po pobraniu, otwórz plik zip i skopiuj zawartość do folderu lokalnego na tym komputerze.
3. Znajdowanie i otwieranie *zasoby WingtipToys* folderu.
4. Przeciąganie i upuszczanie, skopiuj *katalogu* folderu z folderu lokalnego do katalogu głównego projektu aplikacji sieci Web w **Eksploratora rozwiązań** programu Visual Studio. 

    ![Interfejs użytkownika i nawigacja — kopiowanie plików](ui_and_navigation/_static/image1.png)
5. Następnie utwórz nowy folder o nazwie *obrazów* przez kliknięcie prawym przyciskiem myszy **WingtipToys** projektu w **Eksploratora rozwiązań** i wybierając **Dodaj**  - &gt; **Nowy Folder**.
6. Kopiuj *logo.jpg* plik z *WingtipToys zasoby* folderu w **Eksploratora plików** do *obrazów* folderu aplikacji sieci Web Projekt w **Eksploratora rozwiązań** programu Visual Studio.
7. Kliknij przycisk **Pokaż wszystkie pliki** opcji w górnej części **Eksploratora rozwiązań** zaktualizować listę plików, jeśli nie widzisz nowych plików.  
  
    **Eksplorator rozwiązań** zawiera obecnie pliki zaktualizowany projekt. 

    ![Interfejs użytkownika i nawigacja - Eksploratora rozwiązań](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Dodawanie stron

Przed dodaniem nawigacji do aplikacji sieci Web, zostanie najpierw dodać dwie nowe strony, które będzie przejdź do. W dalszej części tego samouczka serii będzie wyświetlać produktów i szczegółowe informacje na tych nowych stron.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.   
 **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie wybierz opcję **formularza sieci Web ze stroną wzorcową** ze środka listy i nadaj mu nazwę *ProductList.aspx*. 

    ![Interfejs użytkownika i nawigacja — Dodaj okno dialogowe nowego elementu](ui_and_navigation/_static/image3.png)
3. Wybierz **Site.Master** dołączyć strony wzorcowej do nowo utworzony *.aspx* strony. 

    ![Interfejs użytkownika i Nawigacja — wybierz strony wzorcowej](ui_and_navigation/_static/image4.png)
4. Dodaj dodatkowe stronę o nazwie *ProductDetails.aspx* wykonując te same czynności.

### <a name="updating-bootstrap"></a>Aktualizowanie ładowania początkowego

Szablony projektu Visual Studio 2013 użyj [Bootstrap](http://getbootstrap.com/), ramy układ i motywów utworzonych przez usługi Twitter. Bootstrap używa CSS3, aby zapewnić elastyczny projekt, co oznacza, że układów dynamicznie można dostosować do rozmiarów okna innej przeglądarki. Funkcja tworzenia motywów ładowania początkowego na umożliwia również łatwe wpływ zmian w aplikacji wyglądu i działania. Domyślnie szablonu aplikacji sieci Web ASP.NET w programie Visual Studio 2013 obejmują Bootstrap jako pakietu NuGet.

W tym samouczku wygląd i działanie aplikacji Wingtip Toys ulegnie zmianie, zastępując pliki CSS ładowania początkowego.

1. W **Eksploratora rozwiązań**, otwórz *zawartości* folderu.
2. Kliknij prawym przyciskiem myszy *bootstrap.css* plików i zmienić jego nazwę na *bootstrap original.css*.
3. Zmień nazwę *bootstrap.min.css* do *bootstrap original.min.css*.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *zawartości* i wybierz polecenie **Otwórz Folder w Eksploratorze plików**.  
   Zostanie wyświetlony w Eksploratorze plików. Pobranych plików CSS bootstrap zapisze do tej lokalizacji.
5. W przeglądarce przejdź do [ http://Bootswatch.com ](http://bootswatch.com/).
6. Przewiń okno przeglądarki Cerulean motywu. 

    ![Interfejs użytkownika i nawigacja - Cerulean motywu](ui_and_navigation/_static/image5.png)
7. Pobierz oba *bootstrap.css* pliku i *bootstrap.min.css* pliku *zawartości* folderu. Użyj ścieżki do folderu zawartości, która jest wyświetlana w **Eksploratora plików** okna, który został uprzednio otwarty.
8. W **programu Visual Studio** w górnej części **Eksploratora rozwiązań**, wybierz pozycję **Pokaż wszystkie pliki** opcję, aby wyświetlić nowe pliki w folderze zawartości. 

    ![Interfejs użytkownika i nawigacja - Eksploratora rozwiązań](ui_and_navigation/_static/image6.png)

   Zobaczysz dwa nowe pliki CSS w **zawartości** folder, ale należy zauważyć, że jest wyszarzona ikona obok nazwy każdego pliku. Oznacza to, że plik nie ma jeszcze dodane do projektu.
9. Kliknij prawym przyciskiem myszy *bootstrap.css* i *bootstrap.min.css* pliki i wybierz **załącz do projektu**.   
   Po uruchomieniu aplikacji Wingtip Toys w dalszej części tego samouczka, pojawi się nowy interfejs użytkownika.

> [!NOTE] 
> 
> Szablon aplikacji sieci Web platformy ASP.NET używa *Bundle.config* pliku w katalogu głównym projektu do przechowywania ścieżkę do plików CSS ładowania początkowego.


### <a name="modifying-the-default-navigation"></a>Modyfikowanie domyślnego nawigacji

Nawigacja domyślne dla każdej strony w aplikacji można modyfikować, zmieniając element listę nieuporządkowaną nawigacji, który znajduje się w *Site.Master* strony.

1. W **Eksploratora rozwiązań**, Znajdź i Otwórz *Site.Master* strony.
2. Dodaj łącze nawigacyjne wyróżnione kolorem żółtym do listy nieuporządkowane pokazano poniżej:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Jak widać w powyższym kodzie HTML, zmodyfikować każdej pozycji `<li>` zawierający tag kotwicy `<a>` z łączem `href` atrybutu. Każdy `href` wskazuje stronę w aplikacji sieci Web. W przeglądarce, gdy użytkownik kliknie na jednym z tych łączy (takich jak **produktów**), przejściu do strony zawarte w `href` (takich jak **ProductList.aspx**). Należy uruchomić aplikację na końcu tego samouczka.

> [!NOTE] 
> 
> Tyldy (`~`) jest używany znak, aby określić, że `href` ścieżka rozpoczyna się w katalogu głównym projektu.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Dodawanie formantu danych do wyświetlania danych nawigacji

Następnie będzie Dodaj formant do wyświetlenia wszystkich kategorii z bazy danych. Każda kategoria będą działać jako łącze do *ProductList.aspx* strony. Gdy użytkownik kliknie łącze kategorii w przeglądarce, zostanie przejdź do strony produktów i zobaczyć tylko te produkty, które są skojarzone z wybraną kategorię.

Użyjesz **ListView** kontrolka do wyświetlenia wszystkich kategorii zawartych w bazie danych. Aby dodać **ListView** formantu do strony głównej:

1. W *Site.Master* strony, Dodaj następujący wyróżniony `<div>` elementu **po** `<div>` zawierający element `id="TitleContent"` dodanego wcześniej:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Ten kod spowoduje wyświetlenie wszystkich kategorii z bazy danych. **ListView** kontroli Wyświetla nazwę każdej kategorii jako tekst łącza i zawiera łącze do *ProductList.aspx* strony z zawierający wartość ciągu kwerendy `ID` kategorii. Przez ustawienie `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępne w ramach `ItemTemplate` węzeł i formantu staje się silnie typizowane. Możesz wybrać szczegóły `Item` przy użyciu funkcji IntelliSense, takich jak określanie `CategoryName`. Ten kod znajduje się w kontenerze `<%#: %>` wskazujący wyrażenia wiązania danych. Dodając (:) na końcu `<%#` prefiks, wynik wyrażenia wiązania danych jest kodowany w formacie HTML. Jeśli wynik jest kodowany w formacie HTML, aplikacja jest lepiej chroniony przed krzyżowych skrypt (XSS iniekcji) i ataków iniekcji kodu HTML.

> [!NOTE] 
> 
> **Porada**
> 
> Po dodaniu kod, wpisując podczas tworzenia może mieć pewność, że prawidłowym elementem członkowskim obiektu znajduje się ponieważ silnie typizowane formantów danych zawierają dostępni członkowie oparte na IntelliSense. IntelliSense oferuje opcje odpowiednie kontekst kodu podczas pisania kodu, na przykład obiektów, metod i właściwości.


W następnym kroku zostaną zaimplementowane `GetCategories` metody do pobierania danych.

### <a name="linking-the-data-control-to-the-database"></a>Łączenie z bazą danych formantu danych

Można było wyświetlać dane w formancie danych, należy połączyć kontroli danych w bazie danych. Aby łącze, można zmodyfikować kod związany z *Site.Master.cs* pliku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Site.Master* a następnie kliknij przycisk **kod widoku**. *Site.Master.cs* plik jest otwarty w edytorze.
2. Początek w pobliżu *Site.Master.cs* plików, Dodaj dwie dodatkowe przestrzenie nazw, tak aby wszystkie przestrzenie nazw uwzględniane wyglądają następująco:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Dodaj wyróżnione `GetCategories` metody po `Page_Load` obsługi zdarzeń w następujący sposób:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Powyższy kod jest wykonywany po załadowaniu w przeglądarce dowolną stronę, która używa strony wzorcowej. `ListView` Formantu (o nazwie "categoryList"), który dodane wcześniej w tym samouczku używane powiązanie modelu, aby wybrać dane. W znaczniku danego `ListView` sterowania Ustaw formantu `SelectMethod` właściwości `GetCategories` metody przedstawionych powyżej. `ListView` Kontrolować wywołania `GetCategories` metody w odpowiednim czasie życia strony cyklu i automatycznie wiąże zwróconych danych. Dowiesz się więcej na temat wiązania danych w następnym samouczku.

### <a name="running-the-application-and-creating-the-database"></a>Uruchamianie aplikacji i tworzenia bazy danych

Wcześniej w tym samouczku zostały utworzone klasy inicjatora (o nazwie "ProductDatabaseInitializer") i określone tej klasy w *global.asax.cs* pliku. Entity Framework wygeneruje bazy danych, gdy aplikacja jest uruchamiana po raz pierwszy, ponieważ `Application_Start` metody zawarte w *global.asax.cs* pliku wywoła klasa inicjatora. Klasa inicjatora użyje klasy modelu (`Category` i `Product`) dodano we wcześniejszej części tego samouczka serii utworzyć bazę danych.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybrać opcję **Ustaw jako stronę startową**.
2. W programie Visual Studio naciśnij klawisz **F5**.   
 Upłynąć trochę czasu na skonfigurowanie wszystko podczas tego pierwszego uruchomienia.   
    ![Interfejs użytkownika i nawigacja - okna przeglądarki](ui_and_navigation/_static/image7.png)  
 Po uruchomieniu aplikacji zostanie skompilowany, aplikacji i bazy danych o nazwie *wingtiptoys.mdf* zostaną utworzone w *aplikacji\_danych* folderu. W przeglądarce zobaczysz menu nawigacji kategorii. W tym menu został wygenerowany przez pobranie kategorii z bazy danych. W następnym samouczku zaimplementowaniem nawigacji.
3. Zamknij przeglądarkę, aby zatrzymać działającej aplikacji.

### <a name="reviewing-the-database"></a>Przegląd bazy danych

Otwórz *Web.config* plik i przyjrzyj się w sekcji ciągu połączenia. Można stwierdzić, że `AttachDbFilename` wskazuje wartość w parametrach połączenia `DataDirectory` dla projektu aplikacji sieci Web. Wartość `|DataDirectory|` jest wartością zastrzeżoną, który reprezentuje *aplikacji\_danych* folderu w projekcie. Ten folder jest, gdzie znajduje się baza danych, który został utworzony z klas jednostek.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Jeśli *aplikacji\_danych* nie jest widoczny folder lub folder jest pusty, wybierz opcję **Odśwież** ikonę, a następnie **Pokaż wszystkie pliki** w górnym **Eksploratora rozwiązań** okna. Rozszerzanie szerokość **Eksploratora rozwiązań** systemu windows może być konieczne, aby wyświetlić wszystkie dostępne ikony.


Teraz możesz sprawdzić dane zawarte w *wingtiptoys.mdf* pliku bazy danych przy użyciu **Eksploratora serwera** okna.

1. Rozwiń węzeł *aplikacji\_danych* folderu. Jeśli *aplikacji\_danych* folder nie jest widoczne, zobacz uwagi powyżej.
2. Jeśli *wingtiptoys.mdf* pliku bazy danych nie jest widoczny, wybierz pozycję **Odśwież** ikonę, a następnie **Pokaż wszystkie pliki** ikony na początku **Eksploratora rozwiązań**  okna.
3. Kliknij prawym przyciskiem myszy *wingtiptoys.mdf* pliku bazy danych i wybierz **Otwórz**.  
    **W Eksploratorze serwera** jest wyświetlany. 

    ![Interfejs użytkownika i nawigacja - Eksploratora serwera](ui_and_navigation/_static/image8.png)
4. Rozwiń węzeł *tabel* folderu.
5. Kliknij prawym przyciskiem myszy **produktów**tabeli i wybierz **Pokaż dane tabeli**.  
 **Produktów** tabela jest wyświetlana. 

    ![Interfejs użytkownika i nawigacja - tabeli Produkty](ui_and_navigation/_static/image9.png)
6. Ten widok pozwala wyświetlić i zmodyfikować danych **produktów** tabeli ręcznie.
7. Zamknij **produktów** okna tabeli.
8. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **produktów** tabeli ponownie, a następnie wybierz **Otwórz definicję tabeli**.  
 Dane projektować pod kątem **produktów** tabela jest wyświetlana. 

    ![Interfejs użytkownika i nawigacja - projekt produktów](ui_and_navigation/_static/image10.png)
9. W **T-SQL** kartę zobaczysz instrukcji SQL DDL, który został użyty do utworzenia tabeli. Można również użyć interfejsu użytkownika w **projekt** kartę do modyfikacji schematu.
10. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **WingtipToys** bazy danych, a następnie wybierz **bliskie połączenie**.   
 Schemat bazy danych przez odłączenia bazy danych z programu Visual Studio, będzie można zmodyfikować w dalszej części tego samouczka serii.
11. Wróć do **Eksploratora rozwiązań**wybierając **Eksploratora rozwiązań** u dołu **Eksploratora serwera** okna.

## <a name="summary"></a>Podsumowanie

W tym samouczku serii dodano podstawowy interfejs użytkownika, grafiki, stron i nawigacji. Ponadto uruchomienia aplikacji sieci Web, który utworzył bazę danych z klas danych, które dodanym w poprzednim samouczka. Możesz również wyświetlić zawartość *produktów* tabeli bazy danych, wyświetlając bezpośrednio bazy danych. W następnym samouczku możesz wyświetlić elementy danych i informacji z bazy danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wprowadzenie do programowania stron sieci Web ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Omówienie kontrolki serwera sieci Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Samouczek CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Poprzednie](create_the_data_access_layer.md)
> [dalej](display_data_items_and_details.md)
