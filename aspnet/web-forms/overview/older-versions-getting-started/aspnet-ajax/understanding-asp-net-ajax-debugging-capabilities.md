---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Opis funkcji debugowania kodu ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Możliwość debugowania kodu jest umiejętności, które każdy Deweloper powinien mieć w ich Arsenał, niezależnie od użytej technologii używane. Wielu programistów są...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 9d420a41f50d06541d04a1dd3cb78a2e6beaaa9a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813831"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Opis funkcji debugowania kodu ASP.NET AJAX
====================
przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Możliwość debugowania kodu jest umiejętności, które każdy Deweloper powinien mieć w ich Arsenał, niezależnie od użytej technologii używane. Wielu programistów są przyzwyczajeni do debugowania aplikacji ASP.NET, które używają kodu VB.NET lub C# za pomocą programu Visual Studio .NET lub Web Developer Express, nie są świadomi, że możliwe jest też bardzo przydatne w przypadku debugowania kodu po stronie klienta, takich jak JavaScript. Ten sam typ techniki debugowania aplikacji .NET można również będą stosowane do aplikacji z włączoną obsługą technologii AJAX i dokładniej aplikacji ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Debugowanie aplikacji ASP.NET AJAX

DaN Wahlin

Możliwość debugowania kodu jest umiejętności, które każdy Deweloper powinien mieć w ich Arsenał, niezależnie od użytej technologii używane. Oczywiste, zrozumienie, które są dostępne różne opcje debugowania można zapisać ogromną ilość czasu na projekt i wręcz Fatalne kilka problemów. Wielu programistów są przyzwyczajeni do debugowania aplikacji ASP.NET, które używają kodu VB.NET lub C# za pomocą programu Visual Studio .NET lub Web Developer Express, nie są świadomi, że możliwe jest też bardzo przydatne w przypadku debugowania kodu po stronie klienta, takich jak JavaScript. Ten sam typ techniki debugowania aplikacji .NET można również będą stosowane do aplikacji z włączoną obsługą technologii AJAX i dokładniej aplikacji ASP.NET AJAX.

W tym artykule zobaczysz, jak program Visual Studio 2008 i kilka innych narzędzi może służyć do debugowania aplikacji ASP.NET AJAX odnajdywanie usterki i inne problemy. Tej dyskusji, będzie zawierać informacje na temat włączania programu Internet Explorer 6 lub nowszej dla debugowania, za pomocą programu Visual Studio 2008 i Eksplorator skryptów, aby śledzić wykonywanie kodu, a także przy użyciu innych bezpłatnych narzędzi, takich jak Pomocnik programowanie sieci Web. Pokażemy ci również sposób debugowania aplikacji ASP.NET AJAX w programie Firefox przy użyciu rozszerzenia o nazwie Firebug pozwalającej krok po kroku przez kod języka JavaScript bezpośrednio w przeglądarce, bez jakichkolwiek innych narzędzi. Na koniec będzie można wprowadzić do klas w bibliotece programu ASP.NET AJAX pomocne przy użyciu różnych zadań debugowania, takie jak śledzenie i instrukcje asercji kodu.

Przed próbą debugowania wyświetlane w programie Internet Explorer, istnieje kilka podstawowych czynności, które należy wykonać, aby ją włączyć debugowanie stron. Przyjrzyjmy się pewne wymagania podstawowe ustawienia, które należy wykonać, aby rozpocząć pracę.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurowanie programu Internet Explorer do debugowania

Większość osób nie jest zainteresowany widzisz problemy z językiem JavaScript napotkała w witrynie sieci Web, wyświetlane w programie Internet Explorer. W rzeczywistości średni użytkownik jeszcze nie wiadomo co zrobić, jeśli zobaczył, komunikat o błędzie. W rezultacie opcje debugowania są domyślnie wyłączone w przeglądarce. Jednak jest bardzo proste włączyć debugowanie i umieść go do użycia podczas tworzenia nowych aplikacji w technologii AJAX.

Aby włączyć funkcję debugowania, przejdź do opcji internetowych narzędzia w menu programu Internet Explorer, a następnie wybierz kartę Zaawansowane. W sekcji przeglądanie upewnij się, że nie zaznaczone są następujące elementy:

- Wyłącz debugowanie skryptu (Internet Explorer)
- Wyłącz debugowanie skryptu (inne)

Chociaż nie jest wymagane, jeśli próbujesz debugować aplikację, prawdopodobnie zaistnieje błędy JavaScript na stronie do nastąpić od razu widoczne, jak i oczywiste. Można wymusić wszystkie błędy, które mają być wyświetlane za pomocą okno komunikatu, zaznaczając pole wyboru "Display powiadomienie o każdym błędzie skryptu". Gdy jest to doskonałe rozwiązanie, aby włączyć funkcję, gdy tworzysz aplikację, może szybko stać się irytujące, jeśli masz tylko perusing innych witryn sieci Web, ponieważ szanse wystąpią błędy JavaScript są całkiem Niezłe rozwiązanie.

Rysunek 1 przedstawia jakie programu Internet Explorer okno dialogowe Zaawansowane powinien wyglądać po została prawidłowo skonfigurowana do debugowania.


[![Konfigurowanie programu Internet Explorer do debugowania.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Rysunek 1**: Konfigurowanie programu Internet Explorer do debugowania.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Po debugowanie zostało włączone, zostanie wyświetlony nowy element menu są wyświetlane w menu Widok o nazwie debugera skryptów. Posiada dwie opcje dostępne w tym Open i podziału na następną instrukcję. Po wybraniu Open zostanie wyświetlony monit do debugowania na stronie w programie Visual Studio 2008 (należy zauważyć, że Visual Web Developer Express może również służyć do debugowania). Visual Studio .NET jest obecnie uruchomiona. Możesz użyć tego wystąpienia lub Utwórz nowe wystąpienie. Po wybraniu podziału w następnej instrukcji zostanie wyświetlony monit do debugowania na stronie, gdy kod JavaScript jest wykonywana. Jeśli kod JavaScript jest wykonywana zdarzenia onLoad strony możesz odświeżyć stronę, aby wyzwolić sesji debugowania. Jeśli kod JavaScript jest uruchamiane po kliknięciu przycisku debuger będzie działać natychmiast, po kliknięciu przycisku.

> *> [!NOTE] Jeśli są uruchomione w systemie Windows Vista z dostępu do kontroli użytkownika (UAC) włączone, a masz program Visual Studio 2008 do uruchamiania jako administrator, programu Visual Studio nie będzie można dołączyć do procesu, po wyświetleniu monitu, aby dołączyć. Aby obejść ten problem, najpierw uruchom program Visual Studio i debugowanie przy użyciu tego wystąpienia.*


Mimo że w następnej sekcji pokazano, jak można debugować strony ASP.NET AJAX, bezpośrednio z w ramach programu Visual Studio 2008, przy użyciu opcji debugera skryptów programu Internet Explorer jest przydatne, gdy strona jest już otwarty, a chcesz dokładniej go sprawdzić.

## <a name="debugging-with-visual-studio-2008"></a>Debugowanie za pomocą programu Visual Studio 2008

Program Visual Studio 2008 zapewnia funkcji debugowania, które deweloperów na całym świecie korzystają z codziennie do debugowania aplikacji .NET. Wbudowany debuger można przejść przez kod, służy do wyświetlania danych obiektów, obserwowanie określonych zmiennych, monitorowanie, stos wywołań, a także wiele innych. Oprócz debugowania kodu VB.NET lub C#, Debuger jest również przydatne podczas debugowania aplikacji ASP.NET AJAX i będzie można przejść przez kod JavaScript wiersz po wierszu. Szczegółowe informacje, które należy wykonać fokus na techniki, które mogą służyć do debugowania plików skryptów po stronie klienta, a nie dostarczanie discourse w całym procesie debugowanie aplikacji przy użyciu programu Visual Studio 2008.

Proces debugowania na stronie w programie Visual Studio 2008, można uruchomić na kilka różnych sposobów. Po pierwsze można użyć opcji debugera skryptów programu Internet Explorer, opisane w poprzedniej sekcji. To działa dobrze w przypadku, gdy strona jest już załadowany w przeglądarce i chcesz rozpocząć debugowanie. Można też, kliknij prawym przyciskiem myszy na stronie .aspx w Eksploratorze rozwiązań i wybierz pozycję Ustaw jako stronę startową, z menu. Jeśli jesteś przyzwyczajony debugowanie stron ASP.NET następnie prawdopodobnie wykonano tego wcześniej. Po naciśnięciu klawisza F5 strony mogą być debugowane. Jednak gdy ogólnie można ustawić punktu przerwania dowolnym chcesz na VB.NET lub C# w kodzie, który nie jest zawsze w przypadku języka JavaScript jako pojawi się obok.

*Osadzony w porównaniu z zewnętrznych skryptów*

Debuger programu Visual Studio 2008 traktuje JavaScript osadzony na stronie, które są inne niż zewnętrzne pliki JavaScript. Przy użyciu plików skryptu zewnętrznego można otworzyć pliku i ustaw punkt przerwania w dowolnym wierszu, który wybierzesz. Można ustawić punktów przerwania, klikając w obszarze na szarym pasku po lewej stronie okna edytora kodu. Gdy JavaScript jest osadzony bezpośrednio do strony przy użyciu `<script>` tagu, ustawienie punktu przerwania, klikając w obszarze na szarym pasku nie jest dostępną opcją. Próbuje ustawić punkt przerwania w wierszu osadzonych skryptów spowoduje ostrzeżenie z informacją "Nie jest to prawidłową lokalizacją punktu przerwania".

Można obejść ten problem, przenosząc kod do pliku js zewnętrznych i odwoływania się do niego przy użyciu atrybutu src &lt;skryptu&gt; tag:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Co zrobić, jeśli przenoszenia kodu do zewnętrznego pliku nie jest dostępną opcją lub wymaga więcej pracy niż jest wart? Chociaż nie można ustawić punktu przerwania, za pomocą edytora, możesz dodać debugger — instrukcja bezpośrednio do kodu, w którym chcesz rozpocząć debugowanie. Umożliwia także klasy Sys.Debug dostępne w bibliotece programu ASP.NET AJAX do wymuszenia, debugowanie, aby rozpocząć. Dowiesz się więcej na temat klasy Sys.Debug w dalszej części tego artykułu.

Przykład użycia `debugger` — słowo kluczowe jest wyświetlany w ofercie 1. W tym przykładzie wymusza debuger przerywa prawo przed wykonaniem wywołania funkcji update.

**Wyświetlanie listy 1. Wymusić aby przerwać debuger programu Visual Studio .NET przy użyciu słowa kluczowego debugera.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Po osiągnięciu instrukcji debugera zostanie wyświetlony monit o debugowania stronę korzystając z programu Visual Studio .NET i rozpocząć krokowe wykonywanie kodu. Podczas wykonywania to Ty, może wystąpić problem z uzyskiwaniem dostępu do plików skryptów biblioteki ASP.NET AJAX użyte na stronie, więc Przyjrzyj się przy użyciu programu Visual Studio. Eksplorator skryptów w sieci.

## <a name="using-visual-studio-net-windows-to-debug"></a>Używanie programu Visual Studio .NET Windows do debugowania

Po uruchomieniu sesji debugowania i rozpoczęciem Instruktaż kodu przy użyciu domyślnego klucza F11, może się pojawić okno dialogowe błędu wyświetlane w zobacz rysunek 2, chyba że wszystkie pliki skryptów użyty na stronie są otwarte i dostępne do debugowania.


[![Okno dialogowe błędu jest wyświetlany, gdy brak kodu źródłowego jest dostępna do debugowania.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Rysunek 2**: okno dialogowe błędu wyświetlany, gdy brak kodu źródłowego jest dostępna do debugowania.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


To okno dialogowe jest wyświetlane, ponieważ program Visual Studio .NET nie ma pewności, jak uzyskać dostęp do kodu źródłowego w niektórych skryptów odwołuje się na stronie. Chociaż może to być całkiem irytujące na początku jest proste poprawki. Po rozpoczął sesję debugowania i Traf punkt przerwania, przejdź do okna programu debugowania Eksplorator skryptów Windows z menu programu Visual Studio 2008, lub użyj skrótu Ctrl + Alt + N.

> *> [!NOTE] Jeśli nie widzisz menu Eksplorator skryptów, na liście, przejdź do pozycji narzędzia* *Dostosuj* *poleceń w menu programu Visual Studio .NET. Zlokalizuj wpis debugowania w sekcji kategorie, a następnie kliknij go, aby wyświetlić wszystkie wpisy dostępne menu. Na liście poleceń, przewiń w dół, Eksplorator skryptów, a następnie przeciągnij go do debugowania* *Windows menu wspomnianych wcześniej. W ten sposób będzie udostępniać wpis menu Eksplorator skryptów każdorazowo, gdy uruchamiasz program Visual Studio .NET.*


Eksplorator skryptów może służyć do wyświetlania wszystkich skryptów na stronie i otwórz je w edytorze kodu. Po otwarciu Eksplorator skryptów, kliknij dwukrotnie strony .aspx aktualnie debugowanych aby otworzyć go w oknie edytora kodu. Wykonaj tę samą akcję dla wszystkich innych skryptów wyświetlane w Eksploratorze skryptów. Gdy wszystkie skrypty są otwarte w oknie kodu, możesz naciśnij klawisz F11 (i użyj innych klawisze dostępu debugowania), aby przejść przez kod. Rysunek 3 przedstawia przykład Eksplorator skryptów. Wyświetla listę bieżącego pliku debugowany (Demo.aspx) oraz dwa skrypty niestandardowe i dwa skrypty wstrzykuje dynamiczne strony przez Menedżera skryptów AJAX programu ASP.NET.


[![Eksplorator skryptów zapewnia łatwy dostęp do skryptów na stronie.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Rysunek 3**. Eksplorator skryptów zapewnia łatwy dostęp do skryptów na stronie.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Kilka innych systemu windows mogą również zawierają przydatne informacje, jak krok po kroku przez kod na stronie. Na przykład umożliwia okno zmiennych lokalnych zobaczyć wartości różne zmienne używane na stronie okna bezpośredniego, aby ocenić określonych zmiennych lub warunków i wyświetlić dane wyjściowe. Aby wyświetlić instrukcje śledzenia napisanych przy użyciu funkcji Sys.Debug.trace, (które zostały omówione w dalszej części tego artykułu) lub Internet Explorer Debug.writeln — funkcja umożliwia także w oknie danych wyjściowych.

Podczas wykonywania kroków za pomocą kodu za pomocą debugera przesuniesz wskaźnik myszy nad zmiennych w kodzie, aby wyświetlić wartość, które są przypisane. Jednak debuger skryptów od czasu do czasu nic nie wyświetla jako wskaźnik myszy nad danym zmiennej JavaScript. Aby wyświetlić wartość, zaznacz instrukcji lub zmiennej, którą próbujesz wyświetlić w oknie edytora kodu i następnie wskaźnik myszy nad nim. Mimo że ta technika nie działa w każdej sytuacji, wiele razy będzie mógł wyświetlić wartość, bez konieczności wyszukiwania w oknie różnych debugowania, takich jak okno zmiennych lokalnych.

Samouczek wideo pokazujące niektóre funkcje omówione w tym miejscu mogą być wyświetlane w [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debugowanie przy użyciu Pomocnika programowanie sieci Web

Mimo że program Visual Studio 2008 (i Visual Web Developer Express 2008) sprzętowi bardzo narzędzia debugowania, istnieją dodatkowe opcje, które może służyć także, które są bardziej lekki. Jednym z najnowszych narzędzi mogą być wprowadzane jest pomocnika programowanie sieci Web. Nikhil Kothari firmy Microsoft, (jeden z kluczy architektów ASP.NET AJAX w firmie Microsoft) napisał to doskonałe narzędzie, które można wykonywać wiele różnych zadań, od prostych debugowania, tak jak w przypadku wyświetlania komunikatów żądań i odpowiedzi HTTP. Pomocnik programowanie sieci Web można pobrać [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Pomocnik programowanie sieci Web należy używać bezpośrednio z programu Internet Explorer, co pozwala na łatwe w użyciu. Jej ponownym uruchomieniu, wybierając pozycję Narzędzia sieci Web Development pomocnika z menu programu Internet Explorer. Zostanie otwarte narzędzie w dolnej części przeglądarki, która to dobre rozwiązanie, ponieważ nie masz pozostawić przeglądarkę, aby wykonać kilka zadań, takich jak rejestrowanie komunikatów żądań i odpowiedzi HTTP. Rysunek 4 pokazuje, jak wygląda pomocnika programowanie sieci Web w działaniu.


[![Pomocnik programowanie sieci Web](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Rysunek 4**: pomocnika programowanie sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Pomocnik programowanie sieci Web nie jest narzędziem użyjesz przechodzić przez kodu wiersz po wierszu jako za pomocą programu Visual Studio 2008. Jednak może służyć do wyświetlania danych wyjściowych śledzenia, łatwo oceny zmiennych w skrypcie lub eksplorować dane znajdują się wewnątrz obiektu JSON. Jest również bardzo przydatny do wyświetlania danych, które są przekazywane do i z na stronie ASP.NET AJAX i serwera.

Kiedy pomocnika programowanie sieci Web zostanie otwarte w programie Internet Explorer, debugowanie skryptu musi być włączona, wybierając skryptu Włączanie debugowania skryptu z menu pomocnika programowania dla sieci Web jak pokazano wcześniej na rysunku 4. Umożliwia to narzędzie, aby intercept o błędach podczas uruchamiania strony. Umożliwia także łatwy dostęp do komunikatów śledzenia, które są danymi wyjściowymi na stronie. Aby wyświetlić informacje o śledzeniu lub wykonania polecenia skryptu, aby przetestować różne funkcje na stronie, wybierz skrypt Pokaż skrypt konsolę z menu pomocnika programowanie sieci Web. Dzięki temu dostęp do okna polecenia i proste okno bezpośrednie.

*Wyświetlanie komunikatów śledzenia i danych obiektu JSON*

W oknie bezpośrednim może służyć do wykonywania poleceń skryptu, lub nawet załadowania lub zapisania skrypty, które są używane do testowania różnych funkcji na stronie. W oknie polecenia zostaną wyświetlone komunikaty śledzenia i debugowania, określanych na wyświetlanej stronie. Wyświetlanie listy 2 pokazuje, jak napisać komunikat śledzenia przy użyciu programu Internet Explorer Debug.writeln — funkcja.

**Wyświetlanie listy 2. Wypisywanie komunikat śledzenia po stronie klienta przy użyciu klasy Debug.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Jeśli właściwość nazwisko zawiera wartość Kowalski, Pomocnik programowanie sieci Web wyświetli komunikat o "nazwisko osoby: Doe" w oknie polecenia konsoli skryptu (przy założeniu, że włączono debugowanie). Pomocnik programowanie sieci Web również dodaje obiekt debugService najwyższego poziomu do stron, które mogą służyć do zapisywania informacji śledzenia lub wyświetlanie zawartości obiektów JSON. Wyświetlanie listy 3 przedstawiono przykład przy użyciu funkcji śledzenia klasy debugService.

**Wyświetlanie listy 3. Za pomocą klasy debugService pomocnika programowanie sieci Web do zapisywania komunikatów śledzenia.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Nieuprzywilejowany funkcji klasy debugService to, że będzie ona działać nawet, jeśli debugowanie nie jest włączone w programie Internet Explorer, co ułatwia zawsze dostęp do danych śledzenia, gdy pomocnika programowanie sieci Web jest uruchomiony. Jeśli narzędzie nie jest używana do debugowania stroną, instrukcji śledzenia zostanie zignorowany, ponieważ wywołanie window.debugService zwróci wartość false.

Klasa debugService umożliwia także danych obiektu JSON, aby ją wyświetlić za pomocą okna Inspektor sieci Web Development pomocnika. Wyświetlanie listy 4 tworzy prosty obiekt JSON zawierający dane osoby. Po utworzeniu obiektu wykonywane jest wywołanie, aby debugService klasy sprawdzić funkcję, aby zezwolić na obiekt JSON wizualnie podlegających inspekcji.

**Wyświetlanie listy 4. Przy użyciu funkcji debugService.inspect, aby wyświetlić dane z obiektu JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Na stronie lub w oknie bezpośrednim podczas wywoływania funkcji GetPerson() spowoduje w oknie dialogowym Inspektor obiektów znajdujących się, jak pokazano na rysunku 5. Właściwości w obiekcie można zmienić dynamicznie przez wyróżnianie, zmieniając wartość pokazana w pole tekstowe z wartością, a następnie klikając łącze aktualizacji. Przy użyciu Inspektora obiektu sprawia, że prosta do wyświetlania danych obiektu JSON i eksperymentować z stosowanie różnych wartości właściwości.

*Debugowanie błędów*

Oprócz umożliwienia danych śledzenia i obiektami JSON, który będzie wyświetlany, pomocnika programowania dla sieci Web można również pomocy w Debugowanie błędów na stronie. W przypadku napotkania błędu użytkownik zostanie wyświetlony monit kontynuować do następnego wiersza kodu lub debugowanie skryptu (patrz rysunek 6). Na okno dialogowe błąd skryptu przedstawiono kompletne wywołanie stosu oraz numery wierszy, dzięki czemu można łatwo zidentyfikować, których problemów w ramach skryptów.


[![Korzystanie z okna Inspektor obiektów, aby wyświetlić obiekt JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Rysunek 5**: Korzystanie z okna Inspektor obiektów do wyświetlenia obiektu JSON.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Wybranie opcji debugowania umożliwia wykonywanie instrukcji skryptu bezpośrednio w bezpośrednim pomocnika programowanie sieci Web, aby wyświetlić wartości zmiennych, zapisać obiekty JSON, a także więcej. Jeśli ta sama akcja, która wyzwoliła błąd odbywa się ponownie i programu Visual Studio 2008 jest dostępny na komputerze, użytkownik zostanie wyświetlony monit rozpocząć sesję debugowania, tak aby można było przechodzić przez kodu wiersz po wierszu zgodnie z opisem w poprzedniej sekcji.


[![Okno dialogowe błąd skryptu pomocnika programowanie sieci Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Rysunek 6**: okno dialogowe błąd skryptu pomocnika programowanie sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Sprawdzanie żądań i odpowiedzi wiadomości*

Podczas debugowania kodu ASP.NET AJAX stron często jest przydatne wyświetlić komunikaty żądań i odpowiedzi wysyłane między stroną i serwera. Wyświetlanie zawartości w wiadomościach umożliwia zobaczenie, jeśli odpowiednie dane jest przekazywana oraz rozmiaru wiadomości. Pomocnika programowanie sieci Web oferuje doskonałą HTTP komunikat rejestratora funkcję ułatwiająca do wyświetlania danych jako nieprzetworzony tekst lub w bardziej czytelnym formacie.

Aby wyświetlić komunikaty żądań i odpowiedzi AJAX programu ASP.NET, rejestratora HTTP musi być włączona, wybierając pozycję Włącz HTTP Rejestrowanie HTTP z menu pomocnika programowanie sieci Web. Po włączeniu wszystkich komunikatów wysłanych z bieżącej strony można wyświetlić w przeglądarce dzienników HTTP, do którego dostęp można uzyskać, wybierając pozycję Dzienniki HTTP Pokaż protokołu HTTP.

Mimo że wyświetlanie nieprzetworzony tekst wysyłanych w każdym komunikacie żądania/odpowiedzi przydaje się bez obaw i opcję w Pomocniku programowanie sieci Web, często jest łatwiejsze do wyświetlania danych wiadomości w postaci bardziej graficznej. Gdy włączono rejestrowanie HTTP, i zostały zarejestrowane komunikaty, można wyświetlić danych komunikatu przez dwukrotne kliknięcie komunikatu w przeglądarce dzienników HTTP. W ten sposób pozwala wyświetlić wszystkie nagłówki skojarzone z wiadomości, a także komunikat rzeczywiste zawartość. Rysunek 7 przedstawia przykładowy komunikat żądania, a komunikat odpowiedzi wyświetlane w oknie przeglądarka dzienników HTTP.


[![Aby wyświetlić dane komunikatów żądań i odpowiedzi, przy użyciu przeglądarka dzienników HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Rysunek 7**: za pomocą podglądu dziennika HTTP, aby wyświetlić dane komunikatów żądań i odpowiedzi.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Podgląd dziennika HTTP automatycznie analizuje obiekty JSON i wyświetla je przy użyciu widoku drzewa, ułatwiając szybkie i łatwe do wyświetlania danych właściwości obiektu. Podgląd dzieli każdej części wiadomości na części, jak pokazano na rysunku 8, gdy UpdatePanel jest używane na stronie ASP.NET AJAX. Jest to doskonałe funkcja, która sprawia, że znacznie łatwiej zobaczyć i zrozumieć, co znajduje się w wiadomości porównaniu wyświetlanie danych pierwotnych wiadomości.


[![Komunikat odpowiedzi UpdatePanel przeglądać za pomocą podglądu dziennika HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Rysunek 8**: komunikat odpowiedzi UpdatePanel wyświetlane przy użyciu przeglądarki plików dziennika HTTP.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Istnieją inne narzędzia, które mogą służyć do wyświetlania komunikatów żądań i odpowiedzi, oprócz pomocnika programowanie sieci Web. Innym dobrym rozwiązaniem jest Fiddler, która jest dostępna bezpłatnie w [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Mimo że nie Fiddler zostanie tutaj omówiona, jest również dobrym rozwiązaniem gdy należy dokładnie sprawdzić nagłówki komunikatów i danych.

## <a name="debugging-with-firefox-and-firebug"></a>Debugowanie za pomocą przeglądarki Firefox i Firebug

Program Internet Explorer nadal jest najczęściej używana przeglądarka, innych przeglądarek, takich jak Firefox stają się bardzo popularny i są coraz częściej używane. W rezultacie będziesz chciał wyświetlić i debugowanie stron ASP.NET AJAX w Firefox oraz Internet Explorer Aby upewnić się, że Twoje aplikacje działają poprawnie. Mimo że Firefox nie można powiązać bezpośrednio do programu Visual Studio 2008 do debugowania, ma rozszerzenie o nazwie Firebug, który może służyć do debugowania stron. Firebug można pobrać za darmo, przechodząc do [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug zapewnia w pełni funkcjonalne środowisko debugowania, które pozwala przejść przez kod wiersz po wierszu, dostęp wszystkie skrypty używane w obrębie strony, przeglądania struktur modelu DOM, wyświetlanie stylów CSS i nawet śledzeniu zdarzeń występujących w stronę. Po zainstalowaniu Firebug jest możliwy, wybierając pozycję Narzędzia Firebug Otwórz Firebug menu przeglądarki Firefox. Podobnie jak pomocnika programowanie sieci Web Firebug służy bezpośrednio w przeglądarce, chociaż może również służyć jako aplikacji autonomicznej.

Gdy Firebug jest uruchomiona, można ustawić punktów przerwania w dowolnym wierszu plik JavaScript o czy skrypt jest osadzony na stronie, czy nie. Aby ustawić punkt przerwania, najpierw załadować odpowiedniej strony, który chcesz debugować w programie Firefox. Po załadowaniu strony wybierz skrypt do debugowania z listy rozwijanej skrypty Firebug firmy. Zostaną wyświetlone wszystkie skrypty używane przez stronę. Punkt przerwania jest ustawiony, klikając na terenie Firebug firmy na szarym pasku w wierszu gdzie musi tak samo, jak w programie Visual Studio 2008 punkt przerwania.

Gdy punkt przerwania został ustawiony w Firebug można wykonywać czynności do wykonania skryptu, który ma być debugowany, takie jak kliknięcie przycisku lub odświeżyć przeglądarkę, aby wyzwolić zdarzenie onLoad. Wykonywanie zostanie automatycznie zatrzymany na wiersz zawierający punkt przerwania. Rysunek 9 przedstawiono przykład punkcie przerwania, który został wyzwolony w Firebug.


[![Obsługa punktów przerwania w Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Rysunek 9**: Obsługa punkty przerwania w Firebug.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Po osiągnięciu punktu przerwania można wkroczyć do, Przekrocz nad lub wychodzenia z kodu za pomocą przycisków strzałek. Podczas wykonywania kroków za pomocą kodu zmienne skryptu są wyświetlane po prawej stronie część debugera, dzięki czemu możesz zobaczyć wartości i przechodzenie do szczegółów w obiektach. Firebug obejmuje również stos wywołań listy rozwijanej, aby zapoznać się z procedurą wykonywanie skryptu, które doprowadziły do bieżącego wiersza debugowany.

Firebug obejmuje również okna konsoli, który może służyć do testowania instrukcji inny skrypt, oceny zmiennych i wyświetlania danych wyjściowych śledzenia. Jest on dostępny, klikając kartę konsoli w górnej części okna Firebug. Strona debugowania można również "poddawane" Aby wyświetlić jego strukturę modelu DOM i zawartość, klikając kartę Sprawdź. Podczas zostanie wyróżniony myszą różne elementy modelu DOM, wyświetlane w oknie Inspektora odpowiednie części strony, dzięki czemu można łatwo sprawdzić, gdzie element jest używany na stronie. Można zmienić wartości atrybutu skojarzone z danym elementem "live", aby eksperymentować z stosowanie różnych szerokości, style, itp. z elementem. Jest to dobre rozwiązanie funkcja, która pozwala uniknąć konieczności stale przełączać się między Edytor kodu źródłowego i przeglądarki Firefox, aby wyświetlić prosty zmiany mogą wpłynąć na stronie.

Na rysunku nr 10 przedstawiono przykład przy użyciu Inspektora DOM do zlokalizowania pole tekstowe o nazwie txtCountry na stronie. Inspektor Firebug można również wyświetlić style CSS używany w stronę, a także zdarzenia, takie jak śledzenie ruchów myszy, kliknięć przycisków, a także więcej.


[![Przy użyciu Inspektora DOM Firebug firmy.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Na rysunku nr 10**: inspector modelu DOM przy użyciu Firebug.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug zapewnia sposób lekki szybko Debuguj strony bezpośrednio w przeglądarce Firefox, jak również doskonałe narzędzie do sprawdzania różnych elementów na stronie.

## <a name="debugging-support-in-aspnet-ajax"></a>Obsługa debugowania w technologii ASP.NET AJAX

Biblioteka rozszerzeń ASP.NET AJAX obejmuje wiele różnych klas, których można użyć w celu uproszczenia procesu elementu Dodawanie funkcji AJAX w witrynie sieci Web. Lokalizowanie elementów w obrębie strony i manipulować nimi, dodawanie nowych formantów, wywołanie usługi sieci Web i nawet obsługi zdarzeń, można użyć tych klas. Biblioteka rozszerzeń ASP.NET AJAX również zawiera klasy, których można użyć, aby ulepszyć proces debugowania stron. W tej sekcji zostanie wprowadzona do klasy Sys.Debug i zobacz, jak mogą być używane w aplikacji.

*Używanie klasy Sys.Debug*

Sys.Debug klasą (języka JavaScript znajdujących się w przestrzeni nazw Sys) może służyć do wykonywania kilku różnych funkcji, m.in. Zapisywanie danych wyjściowych śledzenia wykonywania kodu potwierdzenia i wymuszanie kod, aby zakończyć się niepowodzeniem, dzięki czemu mogą być debugowane. Jest ono często w pliki debugowania biblioteki ASP.NET AJAX (instalowana domyślnie na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) do wykonywania testów warunkowych ( wywoływana potwierdzenia), upewnij się, parametry są przekazywane odpowiednio do funkcji, że obiekty zawierają oczekiwanych danych i do pisania instrukcji śledzenia.

Klasa Sys.Debug udostępnia kilka różnych funkcji, które mogą służyć do obsługi śledzenia, kod potwierdzenia lub błędów, jak pokazano w tabeli 1.

**Tabela 1. Funkcje klasy Sys.Debug.**

| **Nazwa funkcji** | **Opis** |
| --- | --- |
| Assert (warunek, wiadomości, displayCaller) | Potwierdza, że parametr warunek ma wartość true. Jeśli testowany warunek ma wartość false, okno komunikatu będzie służyć do wyświetlania wartości parametru wiadomości. Jeśli parametr displayCaller ma wartość true, metoda wyświetla również informacje o obiekcie wywołującym. |
| clearTrace() | Usuwa danych wyjściowych instrukcji śledzenia operacji. |
| Fail(Message) | Powoduje, że program zatrzymać wykonywanie i przerwać debugowanie. Parametr komunikatu można podać przyczynę niepowodzenia. |
| trace(Message) | Zapisuje parametr komunikat do wyjściowych danych śledzenia. |
| traceDump (object, nazwa) | Wysyła dane obiektu w czytelnym formacie. Parametr name może służyć do Podaj etykietę dla zrzutu śledzenia. Wszystkie obiekty podrzędne w ramach obiektu jest zrzucany będą zapisywane domyślnie. |

Śledzenia po stronie klienta może służyć w podobny sposób jak dostępne w programie ASP.NET: funkcja śledzenia. Umożliwia ona różne komunikaty łatwo zauważyć bez przerywania przepływu w aplikacji. Wyświetlanie listy 5 przedstawiono przykład przy użyciu funkcji Sys.Debug.trace do zapisu w dzienniku śledzenia. Ta funkcja po prostu pobiera komunikat, który powinien być zapisany jako parametr.

**Wyświetlanie listy 5. Przy użyciu funkcji Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Jeśli wykonanie kodu pokazany w ofercie 5 nie zobaczysz żadnych danych wyjściowych śledzenia na stronie. Jedynym sposobem, aby zobaczyć, jak to jest dostępne w programie Visual Studio .NET, pomocnika programowanie sieci Web lub Firebug oknie konsoli. Jeśli chcesz zobaczyć wyniki śledzenia na stronie następnie musisz dodać TextArea tag i nadaj mu identyfikator elementu TraceConsole, jak pokazano dalej:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

TextArea elementu TraceConsole zostanie zapisany dowolnej instrukcji Sys.Debug.trace strony.

W przypadkach, w którym chcesz wyświetlić dane zawarte w obiekcie JSON można użyć klasy Sys.Debug traceDump funkcji. Ta funkcja przyjmuje dwa parametry w tym obiekt, który powinien być zrzucany konsoli śledzenia i nazwę, która może służyć do identyfikowania obiektów w danych wyjściowych śledzenia. Wyświetlanie listy 6 przedstawia przykład przy użyciu funkcji traceDump.

**Wyświetlanie listy 6. Przy użyciu funkcji Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Na ilustracji 11 pokazano dane wyjściowe z wywołaniem funkcji Sys.Debug.traceDump. Należy zauważyć, że oprócz wypisywanie danych obiektu osoba, usługa również zapisuje się adres sub-danych obiektu.

Oprócz śledzenia, klasa Sys.Debug można również przeprowadzić kod potwierdzenia. Potwierdzenia są używane do testowania, czy są spełnione określone warunki, gdy aplikacja jest uruchomiona. Wersja do debugowania skryptów biblioteki ASP.NET AJAX zawiera kilka assert instrukcje do testowania różnych warunków.

Wyświetlanie listy 7 przedstawiono przykład przy użyciu funkcji Sys.Debug.assert, aby przetestować warunku. Kod sprawdza, czy obiekt adres ma wartość null, przed zaktualizowaniem obiektu osoba.


[![Dane wyjściowe funkcji Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Rysunek 11**: dane wyjściowe funkcji Sys.Debug.traceDump.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Wyświetlanie listy 7. Przy użyciu funkcji debug.assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Trzy parametry są przekazywane w tym warunek do oceny, komunikat wyświetlany, jeśli potwierdzenie zwraca wartość FAŁSZ i określa, czy informacje o obiekcie wywołującym powinien być wyświetlany. W przypadkach, w którym potwierdzenie nie powiedzie się komunikat pojawi się oraz informacje o wywołującym Jeśli trzeci parametr ma wartość true. Rysunek 12 przedstawiono przykład okno dialogowe błędu, które pojawia się, jeśli potwierdzenie wyświetlane w ofercie 7 kończy się niepowodzeniem.

Końcowe funkcję, aby obejmować jest Sys.Debug.fail. Jeśli chcesz wymusić kod nie powiedzie się w określonej linii w skrypcie można dodać wywołania Sys.Debug.fail zamiast instrukcji debuger zwykle używanych w aplikacji JavaScript. Funkcja Sys.Debug.fail akceptuje parametr pojedynczy ciąg, który reprezentuje przyczynę niepowodzenia, jak pokazano dalej:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Komunikat o błędzie Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Rysunek 12**: komunikat o błędzie Sys.Debug.assert.  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


W przypadku instrukcji Sys.Debug.fail podczas wykonywania skryptu wartość parametru komunikat pojawi się w konsoli application debugowania, takich jak program Visual Studio 2008 i zostanie wyświetlony monit, aby debugować aplikację. Jeden przypadek, gdzie może to być bardzo przydatne jest, gdy nie można ustawić punktu przerwania w programie Visual Studio 2008 na wykonanie wbudowanego skryptu, ale chcesz kod, aby zatrzymać na określony wiersz, dzięki czemu można sprawdzić wartości zmiennych.

*Informacje o formancie ScriptManager ScriptMode właściwości*

Biblioteka rozszerzeń ASP.NET AJAX obejmuje debugowania i wydania wersji skryptu, które są instalowane na C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 domyślnie. Debugowania skryptów są dobrze sformatowana, łatwe do odczytania i ma kilka wywołań Sys.Debug.assert rozproszone w nich skryptów wersji mają białe znaki wycięte i korzystanie z klasy Sys.Debug rzadko, aby zminimalizować ich całkowity rozmiar.

Formantu ScriptManager dodane do stron ASP.NET AJAX odczytuje element kompilacji debugowania atrybutu w pliku web.config, aby określić, które wersje biblioteki skryptów w celu załadowania. Jednakże, możesz kontrolować, czy Debuguj lub Uwolnij skrypty są załadowane (skrypty biblioteki lub własnych skryptów niestandardowych), zmieniając właściwość ScriptMode. ScriptMode akceptuje wyliczenie ScriptMode, w których elementy Członkowskie zawierają Auto, debugowania, wersji i dziedziczenia.

ScriptMode przyjmuje domyślnie wartość Auto, co oznacza, że funkcja ScriptManager sprawdzi atrybut debugowania w pliku web.config. Podczas debugowania ma wartość false, funkcja ScriptManager załaduje wersji skrypty biblioteki ASP.NET AJAX. Podczas debugowania ma wartość true zostanie załadowana wersja do debugowania skryptów. Zmiana właściwości ScriptMode do wydania lub debugowania wymusi ScriptManager załadować odpowiedniej skrypty niezależnie od tego, jaka wartość atrybutu debugowania ma w pliku web.config. Wyświetlanie listy 8 przedstawia przykład obciążenia debugowania skryptów z biblioteki ASP.NET AJAX za pomocą formantu ScriptManager.

**Wyświetlanie listy 8. Ładowanie debugowania skryptów przy użyciu funkcja ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Można również załadować różne wersje własnych skryptów niestandardowych (debugowanie lub wydanie) przy użyciu ScriptManager właściwość skryptów oraz składnik ScriptReference, jak pokazano w ofercie 9.

**Wyświetlanie listy 9. Ładowanie skrypty niestandardowe korzystające z funkcja ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Jeśli ładowania niestandardowych skryptów za pomocą składnika ScriptReference funkcja ScriptManager musi powiadomić, po zakończeniu działania skryptu ładowania, dodając następujący kod w dolnej części skryptu:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Kod przedstawiony w ofercie 9 informuje menedżera skryptów do wyszukania wersję debugowania skryptu osoby, dzięki czemu będzie ona automatycznie wyszukiwał Person.debug.js zamiast Person.js. Jeśli nie znaleziono pliku Person.debug.js, że zostanie wygenerowany błąd.

W przypadkach, jeśli chcesz, debugowania i wersji skryptu niestandardowego do załadowania na podstawie wartości ustawionej w formancie ScriptManager właściwości ScriptMode można ustawić właściwości ScriptMode kontroli ScriptReference do dziedziczenia. To spowoduje, że odpowiednia wersja niestandardowego skryptu do załadowania na podstawie właściwości ScriptMode ScriptManager, jak pokazano w ofercie 10. Ponieważ właściwość ScriptMode formantu ScriptManager debugowania skryptu Person.debug.js zostanie załadowany i użyty na stronie.

**Wyświetlanie listy 10. Dziedziczenie ScriptMode z ScriptManager dla skryptów niestandardowych.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Za pomocą właściwości ScriptMode odpowiednio można łatwiej debugować aplikacje i uprościć całego procesu. Skrypty wersji biblioteki ASP.NET AJAX trudno jest raczej krok po kroku i odczytu, ponieważ formatowanie kodu został usunięty podczas debugowania skryptów są sformatowane specjalnie na potrzeby debugowania.

## <a name="conclusion"></a>Wniosek

Technologii ASP.NET AJAX firmy Microsoft zapewnia solidną podstawę do tworzenia aplikacji z włączoną obsługą technologii AJAX zwiększające atrakcyjniejsze środowisko pracy użytkownika końcowego. Jednak jako technologią programowania, usterki i inne problemy z aplikacji bez obaw pojawią się. Wiedząc o różnych dostępnych opcjach debugowania można zapisać mnóstwo czasu i wynik w produkcie bardziej stabilne.

W tym artykule służącą do kilku różnych technik debugowanie stron ASP.NET AJAX, w tym program Internet Explorer przy użyciu programu Visual Studio 2008, pomocnika programowanie sieci Web i Firebug. Te narzędzia można uprościć ogólny proces debugowania, ponieważ mogą uzyskiwać dostęp do danych zmiennej, przeprowadzenie kodu wiersz po wierszu i wyświetlić instrukcji śledzenia. Oprócz różnych narzędzia debugowania omówione również pokazano, jak używać klasy Sys.Debug biblioteki ASP.NET AJAX w aplikacji i jak klasa ScriptManager może być używana w taki sposób, aby załadować debugowania lub wydania wersji skryptów.

## <a name="bio"></a>Biografia

DaN Wahlin (Microsoft Most Valuable Professional platformy ASP.NET i usługi XML sieci Web) jest .NET development przez instruktorów i architektura Konsultant w szkoleniu technicznym interfejsu ([www.interfacett.com)](http://www.interfacett.com). DaN założona XML dla witryny sieci Web deweloperów platformy ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), znajduje się na biura osoby mówiącej INETA i występuje na konferencjach kilka. DaN wspólnie utworzone DNA Windows Professional (Wrox), platformy ASP.NET: wskazówki, samouczki i kodu (Sams), platformy ASP.NET 1.1 niejawnego programu testów rozwiązania, Professional ASP.NET 2.0 AJAX (Wrox), uzyskuje dostęp do platformy ASP.NET w wersji 2.0 programu MVP i XML utworzone dla deweloperów platformy ASP.NET (Sams). Jeśli on nie pisania kodu, artykuły lub książek, Dan cieszy się pisania i rejestrowanie utworów muzycznych i odtwarzanie golf i przeciwna swoją żoną i dzieci.

Scott Cate pracował nad przy użyciu technologii Microsoft Web od 1997 r i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacji koncentruje się na rozwiązania programowe wiedzy opartych na. Scott można się skontaktować za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blog znajduje się na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-web-services.md)
