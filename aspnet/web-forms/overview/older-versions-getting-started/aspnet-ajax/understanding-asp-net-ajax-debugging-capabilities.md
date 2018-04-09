---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Opis możliwości debugowania kodu ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Możliwość debugowania kodu jest umiejętności, w której każdy Deweloper powinien mieć w ich Arsenał niezależnie od technologii, które korzysta. Gdy są wielu deweloperów...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: f082e2206f5e691579670e42634f30b57e3b3593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Opis możliwości debugowania kodu ASP.NET AJAX
====================
przez [Scott kategorii](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Możliwość debugowania kodu jest umiejętności, w której każdy Deweloper powinien mieć w ich Arsenał niezależnie od technologii, które korzysta. Gdy wielu deweloperów są przystosowane do debugowania aplikacji ASP.NET, Kod VB.NET lub C# za pomocą programu Visual Studio .NET lub Web Developer Express, nie są pamiętać, że jest również bardzo przydatne w przypadku debugowania kodu po stronie klienta, takich jak JavaScript. Ten sam typ techniki stosowane do debugowania aplikacji .NET można również będą stosowane do aplikacji z włączoną obsługą technologii AJAX i dokładniej aplikacji ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Debugowanie aplikacji ASP.NET AJAX

DaN Wahlin

Możliwość debugowania kodu jest umiejętności, w której każdy Deweloper powinien mieć w ich Arsenał niezależnie od technologii, które korzysta. Oczywiste czy opis różnych opcji debugowania, które są dostępne można zapisać ogromne ilości czasu na projekt, a czasami nawet kilka problemów. Gdy wielu deweloperów są przystosowane do debugowania aplikacji ASP.NET, Kod VB.NET lub C# za pomocą programu Visual Studio .NET lub Web Developer Express, nie są pamiętać, że jest również bardzo przydatne w przypadku debugowania kodu po stronie klienta, takich jak JavaScript. Ten sam typ techniki stosowane do debugowania aplikacji .NET można również będą stosowane do aplikacji z włączoną obsługą technologii AJAX i dokładniej aplikacji ASP.NET AJAX.

W tym artykule zobaczysz, jak Visual Studio 2008 i kilka innych narzędzi może służyć do debugowania aplikacji ASP.NET AJAX odnajdywanie usterki i inne problemy. Rozważania dotyczą informacje na temat włączania programu Internet Explorer 6 lub nowszy, debugowanie, przy użyciu programu Visual Studio 2008 i Explorer skryptu do wykonania kroków opisanych kodu, a także przy użyciu innych narzędzi wolnego, takich jak Web Development pomocnika. Ponadto dowiesz się, jak do debugowania aplikacji ASP.NET AJAX w programie Firefox przy użyciu rozszerzenie o nazwie Firebug pozwalającej krokowo kodu JavaScript bezpośrednio w przeglądarce bez innych narzędzi. Ponadto przedstawiono do klas w biblioteki AJAX ASP.NET, która może pomóc w różnych debugowania zadań, takich jak i śledzenie błędów instrukcji potwierdzania kodu.

Przed próbą debugowania wyświetlane w programie Internet Explorer, istnieje kilka podstawowych czynności, które należy wykonać, aby ją włączyć debugowanie stron. Spójrzmy na niektóre wymagania podstawowe ustawienia, które należy wykonać, aby rozpocząć pracę.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurowanie programu Internet Explorer do debugowania

Większość użytkowników nie są wyświetlenie problemów JavaScript napotkała z programem Internet Explorer do przeglądania witryny sieci Web. W rzeczywistości średnia użytkownika nie wiedzieć, co należy zrobić, jeśli ich był wyświetlany komunikat o błędzie. W związku z tym opcje debugowania są domyślnie wyłączone w przeglądarce. Jednak jest bardzo proste włączyć debugowanie i umieścić go do użycia podczas tworzenia nowej aplikacji interfejsu AJAX.

Aby włączyć funkcję debugowania, przejdź do opcji internetowych narzędzi w menu programu Internet Explorer, a następnie wybierz kartę Zaawansowane. W sekcji przeglądanie upewnij się, że zaznaczenie opcji są następujące elementy:

- Wyłącz debugowanie skryptu (Internet Explorer)
- Wyłącz debugowanie skryptu (inne)

Chociaż nie jest wymagane, jeśli próbujesz debugowania aplikacji prawdopodobnie należy błędy JavaScript w stronę, aby być od razu widoczne i oczywiste. Można wymusić wszystkie błędy, które mają być wyświetlane z okno komunikatu, zaznaczając pole wyboru "Wyświetl powiadomienie o każdym błędzie skryptu". Jest to doskonałe rozwiązanie, aby włączyć funkcję, natomiast w przypadku tworzenia aplikacji, jego może szybko stać się irytujące, jeśli tylko w przypadku perusing innych witryn sieci Web, ponieważ szanse występowania błędów JavaScript są dość dobre.

Rysunek 1 pokazuje jakie programu Internet Explorer okno dialogowe Zaawansowane powinien wyglądać po została prawidłowo skonfigurowana do debugowania.


[![Konfigurowanie programu Internet Explorer do debugowania.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Rysunek 1**: Konfigurowanie programu Internet Explorer do debugowania.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Gdy debugowanie zostało włączone, zostanie wyświetlony nowy element menu są wyświetlane w menu Widok o nazwie Script Debugger. Ma dwie opcje dostępne w tym otwarte i podziału na następnej instrukcji. W przypadku wybrania Otwórz zostanie wyświetlony monit debugowanie stron w programie Visual Studio 2008 (należy pamiętać, że Visual Web Developer Express może także służyć do debugowania). Visual Studio .NET jest obecnie uruchomiona można użyć tego wystąpienia lub Utwórz nowe wystąpienie. W przypadku wybrania podziału w następnej instrukcji zostanie wyświetlony monit do debugowania na stronie, gdy wykonywany jest kod JavaScript. Wykonuje kod JavaScript w zdarzeniu onLoad strony po odświeżeniu strony, aby wyzwolić sesji debugowania. Jeśli kod JavaScript jest uruchamiany po kliknięciu przycisku debugera zostanie uruchomiony, natychmiast po kliknięciu przycisku.

> *> [!NOTE] Jeśli są uruchomione w systemie Windows Vista z dostępu kontroli użytkownika (UAC) włączone, a masz program Visual Studio 2008 ustawiona do uruchamiania jako administrator, programu Visual Studio nie będzie można dołączyć do procesu, gdy zostanie wyświetlony monit o dołączyć. Aby obejść ten problem, najpierw uruchom program Visual Studio i debugowanie przy użyciu tego wystąpienia.*


Chociaż w następnej sekcji przedstawiono sposób debugowania strony ASP.NET AJAX bezpośrednio w programie Visual Studio 2008, przy użyciu opcji debugera skryptów programu Internet Explorer jest przydatne, gdy strona jest już otwarty i chcesz do dokładniejszego zbadania go.

## <a name="debugging-with-visual-studio-2008"></a>Debugowanie za pomocą programu Visual Studio 2008

Program Visual Studio 2008 udostępnia funkcje debugowania i deweloperów na całym świecie polegać na codziennie do debugowania aplikacji .NET. Wbudowany debuger pozwala wykonywać krokowo kodu, służy do wyświetlania danych obiektów, obserwowanie określonych zmiennych, monitorować stosu wywołań i wiele innych. Oprócz debugowanie kodu VB.NET lub C#, Debuger jest również przydatne podczas debugowania aplikacji ASP.NET AJAX i będzie można wykonywać krokowo kodu JavaScript wiersz po wierszu. Szczegóły, które należy wykonać fokus na techniki, które mogą służyć do debugowania plików skryptu po stronie klienta, a nie udostępnia discourse w całym procesie debugowanie aplikacji przy użyciu programu Visual Studio 2008.

Proces debugowanie stron w programie Visual Studio 2008 można uruchomić na kilka różnych sposobów. Po pierwsze można użyć opcji programu Internet Explorer Script Debugger wspomniano w poprzedniej sekcji. To działanie jest również w sytuacji, gdy strona jest już załadowany w przeglądarce i chcesz rozpocząć debugowanie go. Można też, kliknij prawym przyciskiem myszy na strony .aspx w Eksploratorze rozwiązań i Ustaw jako stronę startową, wybierz z menu. Jeśli jesteś przyzwyczajony do debugowanie stron ASP.NET następnie prawdopodobnie wykonaniu tego wcześniej. Po naciśnięciu F5 strony może być debugowany. Jednak gdy ogólnie można ustawić punktu przerwania dowolnym chcesz w kodzie VB.NET lub C#, który nie jest zawsze w przypadku JavaScript jako pojawi się obok.

*Osadzony w porównaniu z zewnętrznych skryptów*

Debuger programu Visual Studio 2008 traktuje JavaScript osadzonego w stronie inne niż zewnętrzne pliki JavaScript. Z plikami skryptu zewnętrznego można otworzyć pliku i ustaw punkt przerwania w dowolnym wierszu, możesz wybrać. Można ustawić punktów przerwania, klikając w obszarze szare na pasku zadań z lewej strony okna edytora kodu. Gdy JavaScript jest osadzony bezpośrednio do strony przy użyciu `<script>` tag ustawienie punkt przerwania, klikając w obszarze szare na pasku zadań nie jest opcją. Próbuje ustawić punkt przerwania w wierszu osadzony skrypt spowoduje ostrzeżenie, że Stany "Nie jest to prawidłową lokalizacją punktu przerwania".

Można uzyskać rozwiązać ten problem, przenosząc kod do pliku zewnętrznego js i za pomocą atrybutu src odwołujące się do &lt;skryptu&gt; tagu:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Co w przypadku przenoszenia kod do pliku zewnętrznego nie jest opcją lub wymaga więcej pracy niż warto? Gdy nie można ustawić punktu przerwania za pomocą edytora, możesz dodać instrukcja debugera bezpośrednio do kodu, w którym ma się rozpocząć debugowania. Umożliwia także klasy Sys.Debug dostępne w bibliotece programu ASP.NET AJAX do wymuszenia, debugowanie, aby rozpocząć. Dowiesz się więcej na temat klasy Sys.Debug w dalszej części tego artykułu.

Przykład użycia `debugger` — słowo kluczowe jest wyświetlana lista 1. W tym przykładzie wymusza debugera tak, aby przerwać prawo przed dokonaniem wywołanie funkcji aktualizacji.

**Wyświetlanie listy 1. Aby wymusić debugera programu Visual Studio .NET można przerwać za pomocą słowa kluczowego debugera.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Po instrukcji debugera jest trafień zostanie wyświetlony monit o debugowania strony za pomocą programu Visual Studio .NET i rozpocząć krokowe wykonywanie kodu. Gdy ten użytkownik ten może wystąpić problem z dostępem do plików skryptów biblioteki ASP.NET AJAX używanych na stronie więc warto Przyjrzyjmy się przy użyciu programu Visual Studio. Eksplorator skryptu w sieci.

## <a name="using-visual-studio-net-windows-to-debug"></a>Za pomocą programu Visual Studio .NET w systemie Windows do debugowania

Po uruchomieniu sesji debugowania i rozpoczęciem Instruktaż kodu za pomocą domyślny klucz F11, mogą wystąpić okna dialogowego błędu, pokazano patrz rysunek 2, chyba że wszystkie pliki skryptów używane na stronie są otwarte i dostępne do debugowania.


[![Okno dialogowe błędu jest wyświetlany, jeśli żaden kod źródłowy nie jest dostępny do debugowania.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Rysunek 2**: okno dialogowe błędu wyświetlany, jeśli żaden kod źródłowy nie jest dostępny do debugowania.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


To okno dialogowe jest wyświetlany, ponieważ program Visual Studio .NET nie ma pewności, jak uzyskać dostęp do kodu źródłowego niektórych skryptów odwołuje się na stronie. Mogą to być dość irytujące na początku, jest prosty poprawki. Po rozpoczął sesję debugowania i trafiony punkt przerwania, przejdź do okna debugowania Eksploratora skryptu w menu programu Visual Studio 2008, lub użyj skrótu Ctrl + Alt + N.

> *> [!NOTE] Jeśli nie widać menu programu Script Explorer wymienione, przejdź do lokalizacji narzędzia* *Dostosuj* *polecenia menu programu Visual Studio .NET. Znajdź wpis debugowania w sekcji kategorie i kliknij go, aby wyświetlić wszystkie elementy menu dostępne. Na liście polecenia przewiń w dół do Eksploratora skryptu i przeciągnij go na debugowania* *menu systemu Windows w wymienionych poniżej. W ten sposób udostępni wpisu menu programu Script Explorer każdorazowo po uruchomieniu programu Visual Studio .NET.*


W Eksploratorze skryptu może służyć do wyświetlania wszystkich skryptów na stronie i otworzyć je w edytorze kodu. Po otwarciu Eksploratora skrypt, kliknij dwukrotnie strony .aspx aktualnie debugowanych aby otworzyć go w oknie edytora kodu. Wykonaj tę samą akcję dla wszystkich innych skryptów wyświetlany w Eksploratorze skryptu. Gdy wszystkie skrypty są otwarte w oknie kodu można naciśnij F11 (i użycia innych klawisze dostępu debugowania) do wykonania kroków opisanych w kodzie. Rysunek 3 przedstawia przykład Eksploratora skryptu. Wyświetla listę bieżącego pliku debugowany (Demo.aspx) oraz dwa skrypty niestandardowe i dwa skrypty dynamicznie wstrzykiwane do strony przez element ScriptManager AJAX ASP.NET.


[![W Eksploratorze skryptu zapewnia łatwy dostęp do skryptów na stronie.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Rysunek 3**. W Eksploratorze skryptu zapewnia łatwy dostęp do skryptów na stronie.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Kilka innych systemu windows mogą również służyć do dostarczające przydatnych informacji, jak krokowo kodu na stronie. Na przykład służy okno zmiennych lokalnych wartości z innej zmienne używane w oknie bezpośrednim oceny określonych zmiennych lub warunków i wyświetlić dane wyjściowe strony. Aby wyświetlić instrukcje śledzenia zapisany przy użyciu funkcji Sys.Debug.trace (które zostaną uwzględnione w dalszej części tego artykułu) lub program Internet Explorer Debug.writeln — funkcja umożliwia także w oknie danych wyjściowych.

Jak można wykonywać krokowo kodu za pomocą debugera można myszą zmiennych w kodzie do wyświetlania wartości, które są przypisane. Jednak debuger skryptu czasami nic nie wyświetla zgodnie z myszą danego zmiennej JavaScript. Aby wyświetlić wartość, zaznacz instrukcji lub zmiennej, którą próbujesz wyświetlić w oknie edytora kodu i następnie myszy nad nim. Mimo że ta metoda nie działa w każdej sytuacji, wiele razy będzie widoczna wartość bez konieczności wyszukiwania w oknie różnych debugowania, takich jak okno zmiennych lokalnych.

Samouczek wideo pokazujące niektóre funkcje omówione w tym miejscu można wyświetlić w [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debugowanie za pomocą pomocnika Projektowanie sieci Web

Mimo że program Visual Studio 2008 (i Visual Web Developer Express 2008) mogą bardzo narzędzia debugowania, istnieją dodatkowe opcje, które może służyć także, które są bardziej lekki. Jednym z najnowsze narzędzia umożliwiające zwalniane jest pomocnika Projektowanie sieci Web. Nikhil Kothari firmy Microsoft, (jeden z kluczy architektów środowiska ASP.NET AJAX w firmie Microsoft) zapisano to doskonałe narzędzie, które można wykonywać wiele różnych zadań z prostego debugowania do wyświetlania komunikatów żądań i odpowiedzi HTTP. Pomocnik tworzenia sieci Web można pobrać [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Pomocnik tworzenia sieci Web można bezpośrednio z programu Internet Explorer, co pozwala na łatwe w użyciu. Jest on uruchomiony przez wybranie narzędzi Web Development pomocnika z menu programu Internet Explorer. Zostanie otwarte narzędzie w dolnej części przeglądarki, która jest dobry, ponieważ nie masz pozostaw przeglądarkę, aby wykonać kilka zadań, takich jak rejestrowanie komunikatów żądań i odpowiedzi HTTP. Na rysunku 4 przedstawiono, jak wygląda pomocnika Projektowanie sieci Web w akcji.


[![Pomocnik tworzenia sieci Web](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Rysunek 4**: pomocnika Projektowanie sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Pomocnik tworzenia sieci Web nie jest to narzędzie służy do kroku przez kod wiersz po wierszu jako program Visual Studio 2008. Jednak może służyć do wyświetlania danych wyjściowych śledzenia, łatwo oceny zmiennych w skrypcie i eksplorować dane są wewnątrz obiekt JSON. Jest również przydatne do wyświetlania danych, które są przekazywane do i z strony ASP.NET AJAX i serwera.

Po pomocnika Projektowanie sieci Web jest otwarty w programie Internet Explorer, debugowanie skryptu musi być włączony, wybierając skrypt włączyć debugowanie skryptu z menu pomocnika projektowanie witryn sieci Web jak pokazano wcześniej na rysunku 4. Umożliwia to narzędzie w celu przechwycenia o błędach jako strona jest uruchomienie. Umożliwia także łatwy dostęp do śledzenia komunikatów, które są wyświetlane na stronie. Aby wyświetlić informacje o śledzeniu, lub wykonaj polecenia skryptu, aby przetestować różne funkcje na stronie, wybierz skrypt Pokaż skrypt konsoli z menu pomocnika Projektowanie sieci Web. Zapewnia to dostęp do okna polecenia i proste oknie bezpośrednim.

*Wyświetlanie komunikatów śledzenia i danych z obiektu JSON*

W oknie bezpośrednim może służyć do wykonywania polecenia skryptu lub nawet obciążenia lub zapisać skrypty, które są używane do testowania różne funkcje na stronie. Okno polecenia wyświetla komunikaty śledzenia i debugowania zapisywane przez na wyświetlanej stronie. Wyświetlanie listy 2 przedstawia sposób zapisania wiadomości śledzenia przy użyciu programu Internet Explorer Debug.writeln — funkcja.

**Wyświetlanie listy 2. Wypisywanie wiadomości śledzenia po stronie klienta przy użyciu klasy debugowania.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Jeśli właściwość nazwisko zawiera wartość Nowak, pomocnika Projektowanie sieci Web zostanie wyświetlony komunikat "nazwisko osoby: Doe" w oknie polecenia konsoli skryptu (przy założeniu, że włączono debugowanie). Pomocnik tworzenia sieci Web również dodaje obiekt debugService najwyższego poziomu do stron, które mogą służyć do zapisania informacji śledzenia lub wyświetlanie zawartości obiektów JSON. Wyświetlanie listy 3 przedstawiono przykład przy użyciu funkcji śledzenia klasy debugService.

**Wyświetlanie listy 3. Używanie klasy debugService Web Development pomocnika do zapisywania komunikatów śledzenia.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Funkcja nieuprzywilejowany klasy debugService jest, że będzie ona działać nawet, jeśli debugowanie nie jest włączone w programie Internet Explorer, co ułatwia zawsze dostęp do danych śledzenia, gdy pomocnika Projektowanie sieci Web jest uruchomiony. Gdy narzędzie nie jest używana do debugowania strony, instrukcje śledzenia zostanie zignorowany, ponieważ wywołanie window.debugService zwróci wartość false.

Klasa debugService umożliwia również JSON obiektu danych można wyświetlić przy użyciu okna Inspektor sieci Web Development pomocnika. Wyświetlanie listy 4 tworzy prosty obiekt JSON zawierający dane osoby. Po utworzeniu obiektu jest nawiązane połączenie do debugService klasy sprawdzić funkcja umożliwia wizualnie poddanych obiekt JSON.

**Wyświetlanie listy 4. Przy użyciu funkcji debugService.inspect, aby wyświetlić dane obiektów JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Na stronie lub w oknie bezpośrednim podczas wywoływania funkcji GetPerson() spowoduje w oknie dialogowym Inspektor obiektów znajdujących się, jak pokazano na rysunku 5. Właściwości wewnątrz obiektu można zmieniać dynamicznie przez wyróżnianie, zmiana wartości wyświetlane w polu tekstowym wartość, a następnie klikając łącze aktualizacji. Za pomocą Inspektora obiektu umożliwia proste do wyświetlania danych obiektu JSON i eksperymentować stosowanie różnych wartości do właściwości.

*Debugowanie błędów*

Oprócz umożliwienia danych śledzenia i obiektów JSON, który będzie wyświetlany, pomocnika projektowanie witryn sieci Web również może pomóc w debugowaniu błędy na stronie. Jeśli wystąpi błąd, pojawi się monit do następnego wiersza kodu w dalszym ciągu lub debugowanie skryptu (patrz rysunek 6). Na okno dialogowe błąd skryptu przedstawiono ukończenia wywołania stosu oraz numery wierszy, aby łatwo zidentyfikować, gdzie są problemy w skrypcie.


[![Korzystanie z okna Inspektor obiektów, aby wyświetlić obiekt JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Rysunek 5**: Korzystanie z okna Inspektor obiektów do wyświetlenia obiekt JSON.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Wybranie opcji debugowania służy do wykonywania instrukcji skryptu bezpośrednio w oknie bezpośrednim pomocnika Projektowanie sieci Web do wyświetlania wartości zmiennych, zapisać obiektów JSON, a także więcej. Jeśli ta sama akcja, która wyzwoliła błędu jest wykonywane ponownie i Visual Studio 2008 jest dostępna na komputerze, pojawi się monit, aby rozpocząć sesję debugowania, dzięki czemu można przejrzeć kod wiersz po wierszu zgodnie z opisem w poprzedniej sekcji.


[![Okno dialogowe błąd skryptu pomocnika Projektowanie sieci Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Rysunek 6**: okno dialogowe błąd skryptu pomocnika Projektowanie sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Sprawdzanie żądań i odpowiedzi, wiadomości*

Podczas debugowania kodu ASP.NET AJAX stron często jest przydatne wyświetlić żądanie i odpowiedź komunikaty wysyłane między stroną i serwera. Wyświetlanie zawartości w wiadomości umożliwia można zobaczyć, czy odpowiednie dane jest przekazywany oraz rozmiaru komunikatów. Pomocnik programowanie sieci Web zapewnia doskonałą HTTP komunikat rejestratora funkcja, która ułatwia wyświetlać dane jako tekst raw lub w bardziej czytelnym formacie.

Aby wyświetlić komunikatów żądań i odpowiedzi ASP.NET AJAX, rejestratora HTTP musi być włączony, wybierając polecenie HTTP włączyć rejestrowanie HTTP z menu pomocnika Projektowanie sieci Web. Po włączeniu wszystkich wiadomości wysłanych z bieżącej strony można wyświetlić w przeglądarce dzienników HTTP, którego dostęp można uzyskać, wybierając dzienników HTTP Pokaż HTTP.

Mimo że wyświetlanie nieprzetworzony tekst w każdej wiadomości żądania/odpowiedzi przydaje się na pewno (i opcję w sieci Web Development pomocnika), często jest łatwiej przeglądać dane wiadomości w postaci graficznej więcej. Gdy włączono rejestrowanie HTTP i zostały zarejestrowane komunikaty, można wyświetlić dane wiadomości przez dwukrotne kliknięcie komunikatu w przeglądarce dzienników HTTP. W ten sposób umożliwia wyświetlenie wszystkich nagłówki skojarzone z komunikatu, jak również rzeczywista komunikat zawartości. Rysunek nr 7 przedstawia przykład komunikatu żądania i odpowiedzi komunikat wyświetlany w oknie przeglądarka dzienników HTTP.


[![Za pomocą podglądu dziennika HTTP, aby wyświetlić dane komunikatów żądań i odpowiedzi.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Rysunek 7**: za pomocą podglądu dziennika HTTP, aby wyświetlić dane komunikatów żądań i odpowiedzi.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Przeglądarka dzienników HTTP automatycznie analizuje obiektów JSON i wyświetla je za pomocą widoku drzewa, ułatwiając szybkie i łatwe do wyświetlania danych właściwości obiektu. Po elemencie UpdatePanel jest używany na stronie ASP.NET AJAX, Podgląd komunikatów o dzieli się każda część komunikatu na części, jak pokazano na rysunku 8. To doskonały funkcja, która ułatwia wyświetlić i poznać nowości w komunikacie w porównaniu do przeglądania danych pierwotnych wiadomości.


[![Komunikat odpowiedzi UpdatePanel wyświetlać za pomocą podglądu dziennika HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Rysunek 8**: komunikat odpowiedzi UpdatePanel wyświetlać za pomocą podglądu dziennika HTTP.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Istnieje kilka innych narzędzi, które mogą służyć do wyświetlania komunikatów żądań i odpowiedzi oprócz pomocnika Projektowanie sieci Web. Innym dobrym rozwiązaniem jest Fiddler, który jest dostępny bezpłatnie w [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Mimo że Fiddler nie zostanie tutaj dokładnie omówione, jest również dobrym rozwiązaniem gdy należy dokładnie sprawdzić nagłówki wiadomości i danych.

## <a name="debugging-with-firefox-and-firebug"></a>Debugowanie za pomocą przeglądarki Firefox i Firebug

Program Internet Explorer nadal jest najczęściej używana przeglądarka, inne przeglądarki, takich jak program Firefox stają się bardzo popularny i są bardziej używane. W związku z tym należy wyświetlić i debugowanie stron ASP.NET AJAX w Firefox, a także programu Internet Explorer, aby sprawdzić, czy aplikacje działają prawidłowo. Mimo że Firefox nie można powiązać bezpośrednio do programu Visual Studio 2008 debugowania, ma rozszerzenie o nazwie Firebug, który może służyć do debugowania stron. Firebug można bezpłatnie pobrać, przechodząc do [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug udostępnia środowisko debugowania kompletne używany do krokowo kodu wiersz po wierszu, dostęp wszystkie skrypty używane na stronie, wyświetlanie struktury modelu DOM, wyświetlanie stylów CSS i nawet zdarzeń śledzenia w stronę. Po zakończeniu instalacji Firebug są dostępne, wybierając narzędzia Firebug Otwórz Firebug z Firefox menu. Jak pomocnika programowanie dla sieci Web Firebug jest używany bezpośrednio w przeglądarce, chociaż może również służyć jako aplikacja autonomiczna.

Po uruchomieniu Firebug, można ustawić punktów przerwania w dowolnym wierszu plik JavaScript o czy skrypt jest osadzony w stronę, czy nie. Aby ustawić punkt przerwania, najpierw załadować odpowiedniej strony, którą chcesz debugowania w programie Firefox. Po załadowaniu stronie Wybierz skrypt do debugowania z listy rozwijanej Firebug przez skrypty. Zostaną wyświetlone wszystkie skrypty używane przez stronę. Punkt przerwania jest ustawiony, klikając w Firebug szare na pasku zadań obszarze w wierszu gdzie musi tak jak w programie Visual Studio 2008, należy wykonać punktu przerwania.

Gdy punkt przerwania został ustawiony w Firebug można wykonać akcji do wykonania skryptu, który wymaga debugowanego takie jak kliknięcie przycisku lub odświeżyć przeglądarkę, aby wyzwolić zdarzenie onLoad wymagane. Wykonanie zatrzyma się automatycznie w wiersz zawierający punkt przerwania. Na rysunku nr 9 przedstawiono przykład punktu przerwania, które zostało wyzwolone w Firebug.


[![Obsługa punktów przerwania w Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Rysunek 9**: Obsługa punktów przerwania w Firebug.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Gdy punkt przerwania zostaje trafiony można Wkrocz, Przekrocz nad lub wychodzenia z kodu przy użyciu przycisków strzałek. Podczas wykonywania kroków za pomocą kodu skryptu zmienne są wyświetlane w prawej części debugera, umożliwiając zobaczyć wartości i przechodzenia do obiektów. Firebug także listy rozwijanej stos wywołań, aby wyświetlić kroki wykonywania skryptów, które doprowadziły do bieżącego wiersza debugowany.

Firebug także okna konsoli, który może służyć do testowania instrukcje inny skrypt, oceny zmiennych i wyświetlania danych wyjściowych śledzenia. Jest on dostępny, klikając kartę konsoli w górnej części okna Firebug. Strona debugowany można również "poddawane" Aby wyświetlić jej zawartości i struktury modelu DOM, klikając kartę inspekcję. Podczas myszą różnych elementów modelu DOM wyświetlany w oknie Inspektora odpowiedniej części strony zostanie podświetlony ułatwiając Zobacz, gdy element jest używany na stronie. Można zmienić wartości atrybutu skojarzone z danym elementem "live" eksperymentować stosowanie różnych szerokości, style, itp. do elementu. Jest to dobry funkcja, która pozwala uniknąć konieczności stale przełączać się między Edytor kodu źródłowego i przeglądarki Firefox, aby wyświetlić prosty sposób zmiany wpływają na stronę.

Na rysunku nr 10 przedstawiono przykład Inspektor modelu DOM i odszukaj pole tekstowe o nazwie txtCountry na stronie. Inspektor Firebug można również wyświetlić stylu CSS w stronę, a także zdarzenia, takie jak śledzenie ruchów myszy, kliknięć przycisków, a także więcej.


[![Za pomocą Firebug w modelu DOM inspektora.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Na rysunku nr 10**: inspector modelu DOM przy użyciu Firebug.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug zapewnia sposób lekki szybko debugowania stronę bezpośrednio w programie Firefox, a także doskonałe narzędzie do sprawdzania różnych elementów na stronie.

## <a name="debugging-support-in-aspnet-ajax"></a>Obsługę debugowania ASP.NET AJAX

Biblioteka ASP.NET AJAX zawiera wiele różnych klas, których można użyć w celu uproszczenia procesu dodawania możliwości technologii AJAX w witrynie sieci Web. Lokalizowanie elementów na stronie i manipulowania nimi, Dodaj nowe kontrolki, wywołują usługi sieci Web i nawet obsługi zdarzeń, można użyć tych klas. Biblioteka ASP.NET AJAX zawiera również klasy, których można użyć w celu ułatwienia procesu debugowania stron. W tej sekcji przedstawiono klasy Sys.Debug i zobacz, jak można w aplikacji.

*Za pomocą klasy Sys.Debug*

Klasa Sys.Debug (klasy JavaScript znajdujących się w przestrzeni nazw Sys) mogą posłużyć do wykonania kilku różnych funkcji w tym zapisywania wyjściowych danych śledzenia, wykonywanie kodu potwierdzenia i wymuszanie kod, aby zakończyć się niepowodzeniem, tak aby było możliwe. Jest on często w biblioteki ASP.NET AJAX pliki debugowania (domyślnie instalowana na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) do wykonywania testów warunkowego ( wywołuje potwierdzenia) zapewniające parametry są przekazywane poprawnie do funkcji, że obiekty zawierają oczekiwane dane i pisanie instrukcji śledzenia.

Klasa Sys.Debug udostępnia kilka różnych funkcji, które mogą służyć do obsługi śledzenia, kod potwierdzenia lub błędów, jak pokazano w tabeli 1.

**Tabela 1. Funkcje klasy Sys.Debug.**

| **Nazwa funkcji** | **Opis** |
| --- | --- |
| Assert (warunek, wiadomości, displayCaller) | Potwierdza, że parametr warunku jest PRAWDA. Jeśli testowana jest spełniony warunek, okno komunikatu będzie używany do wyświetlania wartości parametru wiadomości. Jeśli parametr displayCaller ma wartość true, metoda wyświetla również informacje o elemencie wywołującym. |
| clearTrace() | Usuwa dane wyjściowe instrukcje operacji śledzenia. |
| Fail(Message) | Powoduje, że program zatrzymać wykonanie i przerwanie w debugerze. Parametr komunikatu może służyć do zapewnienia przyczynę błędu. |
| trace(Message) | Zapisuje komunikat parametr danych wyjściowych śledzenia. |
| traceDump (obiektu, nazwa) | Generuje dane obiektu w czytelnym formacie. Parametr name może służyć do zapewnienia etykietę dla zrzutu śledzenia. Wszystkie obiekty podrzędne w obiekcie trwa utworzyć zrzutu zostanie zapisany domyślnie. |

Śledzenie po stronie klienta można znacznie samo jak funkcja śledzenia dostępnych w programie ASP.NET. Umożliwia on inne komunikaty do łatwo widoczne bez przerywania przepływu aplikacji. Wyświetlanie listy 5 przedstawiono przykład przy użyciu funkcji Sys.Debug.trace do zapisu w dzienniku śledzenia. Ta funkcja przyjmuje po prostu komunikat, który powinien być zapisywane jako parametr.

**Wyświetlanie listy 5. Przy użyciu funkcji Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Jeśli wykonanie kodu na listę 5 nie zobaczysz żadnych danych wyjściowych śledzenia na stronie. Jedynym sposobem, aby go wyświetlić ma korzystać z dostępnych w Visual Studio .NET, Web Development pomocnika lub Firebug okna konsoli. Jeśli chcesz zobaczyć wyniki śledzenia na stronie, następnie należy dodać znacznik TextArea i nadaj mu identyfikator TraceConsole, jak pokazano w następnym:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Wszystkie instrukcje Sys.Debug.trace na stronie zostanie zapisany TraceConsole TextArea.

W przypadkach, w którym chcesz wyświetlić dane zawarte w obiekcie JSON można użyć funkcji traceDump klasy Sys.Debug. Ta funkcja przyjmuje dwa parametry, łącznie z obiektu, który należy utworzyć zrzutu do konsoli śledzenia i nazwy, które mogą służyć do identyfikowania obiektu w danych wyjściowych śledzenia. Wyświetlanie listy 6 przedstawiono przykład przy użyciu funkcji traceDump.

**Wyświetlanie listy 6. Przy użyciu funkcji Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Rysunek 11 zawiera wywołanie funkcji Sys.Debug.traceDump dane wyjściowe. Należy zauważyć, że oprócz zapisywania danych obiektu osoby, również zapisuje dane adres sub obiektu.

Oprócz śledzenia, klasa Sys.Debug można również wykonać kod potwierdzenia. Potwierdzenia są wykorzystywane do testowania, gdy aplikacja jest uruchomiona spełnienia określonych warunków. Wersja do debugowania skryptów biblioteki ASP.NET AJAX zawiera kilka assert instrukcje, aby przetestować różnych warunków.

Wyświetlanie listy 7 przedstawiono przykład przy użyciu funkcji Sys.Debug.assert do testowania warunku. Kod sprawdzenie, czy obiekt adres ma wartość null przed zaktualizowaniem obiektu osoby.


[![Dane wyjściowe funkcji Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Rysunek 11**: dane wyjściowe funkcji Sys.Debug.traceDump.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Wyświetlanie listy 7. Przy użyciu funkcji debug.assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Trzy parametry są przekazywane w tym warunku, aby ocenić, komunikat wyświetlany, jeśli potwierdzenie zwraca wartość FAŁSZ i określa, czy informacje o elemencie wywołującym powinna być wyświetlana. W przypadkach, gdy nie potwierdzenie wiadomości będą wyświetlane oraz informacje o wywołującym, jeśli trzeci parametr ma wartość true. Rysunek 12 przedstawiono przykład okna dialogowego błędu, wyświetlanego w przypadku potwierdzenia pokazano na wyświetlanie 7 kończy się niepowodzeniem.

Funkcja końcowego tak, aby pokrywał jest Sys.Debug.fail. Gdy użytkownik chce wymusić niepowodzenie na określonej linii w skrypcie kodu można dodać wywołanie Sys.Debug.fail zamiast instrukcji debuger zwykle używanych w aplikacji JavaScript. Funkcja Sys.Debug.fail przyjmuje parametr będący pojedynczym ciągiem, który reprezentuje przyczynę błędu, jak pokazano w następnym:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Komunikat o błędzie Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Rysunek 12**: komunikat o błędzie A Sys.Debug.assert.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


W przypadku instrukcji Sys.Debug.fail podczas skrypt jest wykonywany, wartość parametru komunikat będzie wyświetlana w konsoli debugowania aplikacji takich jak program Visual Studio 2008 i zostanie wyświetlony monit do debugowania aplikacji. Jeden przypadek, w którym może to być bardzo przydatne jest, gdy nie można ustawić punktu przerwania w programie Visual Studio 2008 na wykonanie wbudowanego skryptu, ale chcesz kod może zatrzymać się na określonej linii, więc możesz sprawdzić wartości zmiennych.

*Opis właściwości formantu ScriptManager ScriptMode*

Biblioteka ASP.NET AJAX zawiera debugowania i wydania wersji skryptów, które są domyślnie instalowane na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0. Debugowania skryptów są dobrze sformatowana, łatwy do odczytania i ma kilka wywołań Sys.Debug.assert rozrzucone po całej ich podczas skryptów wersji ma odstępu wycięte i użyj klasy Sys.Debug oszczędnie, aby zminimalizować ich łącznego rozmiaru.

Dodane do stron ASP.NET AJAX formantu ScriptManager odczytuje atrybutu debugowania elementu kompilacji w pliku web.config, aby ustalić, które wersje, biblioteki skryptów do załadowania. Jednak można kontrolować, jeśli debugowanie lub zwolnij skrypty są załadowane (skrypty biblioteki lub skryptów niestandardowych), zmieniając właściwość ScriptMode. ScriptMode akceptuje wyliczenie ScriptMode, której członkowie obejmują automatyczne, debugowania i wydania dziedziczenia.

Domyślnie ScriptMode wartość Auto, co oznacza, że element ScriptManager sprawdzi atrybutu debugowania w pliku web.config. Podczas debugowania ma wartość false ScriptManager załaduje wydanej wersji programu ASP.NET AJAX skrypty biblioteki. Podczas debugowania ma wartość true zostanie załadowana wersja do debugowania skryptów. Zmiana właściwości ScriptMode do debugowania czy zwykłe wymusi ScriptManager ładowanie skryptów odpowiednie, niezależnie od tego, jakie korzyści ma atrybut debugowania w pliku web.config. Wyświetlanie listy 8 przedstawiono przykład za pomocą formantu ScriptManager ładowanie skryptów debugowania z biblioteki ASP.NET AJAX.

**Wyświetlanie listy 8. Ładowanie skryptów debugowania przy użyciu ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Można również ładować różne wersje własnych skryptów niestandardowych (debugowanie czy wydanie) przy użyciu właściwości skryptów element ScriptManager wraz ze składnika ScriptReference, jak pokazano na wyświetlanie 9.

**Wyświetlanie listy 9. Ładowanie skryptów niestandardowych przy użyciu ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> W przypadku ładowania niestandardowych skryptów za pomocą składnika ScriptReference należy powiadomić ScriptManager po zakończeniu ładowanie skryptu przez dodanie poniższego kodu w dolnej części skryptu:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Kodem przedstawionym w wyświetlania 9 informuje ScriptManager do wyszukania wersję do debugowania skryptu, osoby, automatycznie będzie szukać Person.debug.js zamiast Person.js. Jeśli nie znaleziono pliku Person.debug.js, że zostanie wygenerowany błąd.

W przypadkach, gdy chcesz debugowania lub wydanej wersji skryptu niestandardowego do załadowania na podstawie wartości ustawionej w formancie ScriptManager właściwości ScriptMode można ustawić właściwości ScriptMode formantu ScriptReference dziedziczenia. Spowoduje to odpowiedniej wersji skryptu niestandardowego do załadowania ustalane na podstawie właściwości ScriptMode element ScriptManager pokazane na listę 10. Ponieważ właściwość ScriptMode formantu ScriptManager debugowania skryptu Person.debug.js zostanie załadowana i użyty na stronie.

**Wyświetlanie listy 10. Dziedziczenie ScriptMode z ScriptManager niestandardowych skryptów.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Za pomocą właściwości ScriptMode odpowiednio można łatwiej debugowania aplikacji i uprościć całego procesu. Skrypty wersji biblioteki ASP.NET AJAX trudno jest raczej kroków i odczytu, ponieważ formatowanie kodu został usunięty podczas debugowania skryptów są sformatowane specjalnie na potrzeby debugowania.

## <a name="conclusion"></a>Wniosek

Technologia ASP.NET AJAX firmy Microsoft zapewnia solidną podstawę do tworzenia aplikacji z włączoną obsługą technologii AJAX zwiększające środowisko pracy użytkownika końcowego. Jednak jak w przypadku innych technologii programowania, błędy i problemy z innych aplikacji na pewno wystąpienia. Uzyskiwanie informacji o różnych dostępnych opcjach debugowania można zapisać dużo czasu i wynik stabilniejsze produktu.

W tym artykule w niniejszym artykule przedstawiono kilka różnych technik debugowanie stron ASP.NET AJAX w tym programu Internet Explorer przy użyciu programu Visual Studio 2008, pomocnika Projektowanie sieci Web i Firebug. Te narzędzia można uprościć całego procesu debugowania, ponieważ można uzyskać dostępu do danych zmiennej, przeprowadzenie kodu wiersz po wierszu i wyświetlać instrukcji śledzenia. Oprócz różnych narzędzi debugowania omówione przedstawiono także sposób można klasy Sys.Debug biblioteki ASP.NET AJAX w aplikacji i jak klasa ScriptManager może służyć do załadowania debugowania i wydania wersji skryptów.

## <a name="bio"></a>Mnie

daN Wahlin (Microsoft najbardziej wartościowych Professional dla platformy ASP.NET i usługi XML sieci Web) jest .NET development instruktora i architektura konsultanta na szkolenia technicznego interfejsu ([www.interfacett.com)](http://www.interfacett.com). DaN założona XML dla witryny sieci Web deweloperów platformy ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), znajduje się w biurze prelegenta INETA i mówi w kilku konferencji. DaN wspólnie utworzone Professional DNS systemu Windows (Wrox), platformy ASP.NET: porady, samouczki i kodu (Sams), ASP.NET 1.1 niejawnego rozwiązania, Professional ASP.NET 2.0 AJAX (Wrox), uzyskuje dostęp do programu ASP.NET 2.0 MVP i utworzone XML dla deweloperów platformy ASP.NET (Sams). Jeśli on nie zapisuje kodu, artykułów lub książek, zapisywania i rejestrowanie muzyka i odtwarzanie pól i koszykówki swoją żona i dzieci będą zadowoleni Dan.

Scott IE pracuje z technologii Microsoft Web od 1997 i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacje oparte na systemie koncentruje się na rozwiązania w zakresie oprogramowania bazy wiedzy Knowledge Base. Scott można nawiązać połączenie za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blogu w [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-web-services.md)
