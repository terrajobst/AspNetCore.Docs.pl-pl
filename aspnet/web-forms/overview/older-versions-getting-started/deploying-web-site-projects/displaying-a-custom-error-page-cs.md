---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Wyświetlanie niestandardowej strony błędu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Co to użytkownik widział gdy wystąpi błąd środowiska uruchomieniowego w aplikacji sieci web ASP.NET? Odpowiedź zależy od jak witryny sieci Web &lt;customErrors&gt; konfiguracji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f2e9165acf86ce62d51e7c8d67205684d68e214
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362946"
---
<a name="displaying-a-custom-error-page-c"></a>Wyświetlanie niestandardowej strony błędu (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Co to użytkownik widział gdy wystąpi błąd środowiska uruchomieniowego w aplikacji sieci web ASP.NET? Odpowiedź zależy od jak witryny sieci Web &lt;customErrors&gt; konfiguracji. Domyślnie użytkownicy otrzymują ciemniejszego ekranu żółty, proclaiming, że wystąpił błąd w czasie wykonywania. W tym samouczku pokazano, jak dostosować te ustawienia mają być aesthetically atrakcyjne Wyświetlanie niestandardowej strony błędu odpowiadający Twojej witryny wygląd i działanie.


## <a name="introduction"></a>Wprowadzenie

W idealnych byłoby bez błędów czasu wykonywania. Programiści napisać kod, używając n-argumentowości usterkę i walidacji danych wejściowych użytkownika niezawodne i zewnętrznych zasobów, takich jak serwery baz danych i serwerów poczty e-mail czy nigdy nie przejdą w tryb offline. Oczywiście w rzeczywistości błędy są nieuniknione. Klasy w .NET Framework zasygnalizować błąd, zostanie zgłoszony wyjątek. Na przykład element SqlConnection podczas wywoływania metody Open obiektu nawiąże połączenie z określona przez ciąg połączenia bazy danych. Jednakże, jeśli baza danych znajduje się w dół lub poświadczenia w parametrach połączenia są nieprawidłowe następnie metodę Open zgłasza `SqlException`. Wyjątki mogą być obsługiwane przy użyciu `try/catch/finally` bloków. Jeśli kod w ramach `try` bloku zgłasza wyjątek, kontrola jest przekazywana do bloku catch odpowiednie, gdy deweloper może próbować odzyskać sprawność po błędzie. Jeśli wystąpi nie pasującego bloku catch, lub jeśli kod, który wygenerował wyjątek nie jest w bloku try, wyjątek percolates górę stosu wywołań search z `try/catch/finally` bloków.

Jeśli wyjątek zostanie rozpropagowany aż środowiska uruchomieniowego programu ASP.NET nie jest obsługiwany, [ `HttpApplication` klasy](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)firmy [ `Error` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) jest wywoływane, a także sprawdzić skonfigurowaną *stronę błędu*  jest wyświetlana. Domyślnie platforma ASP.NET wyświetla stronę błędu, który affectionately nazywa się [żółty ekranu śmierci](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Istnieją dwie wersje YSOD: Pokazuje szczegóły wyjątku, ślad stosu i inne informacje pomocne deweloperom debugowanie aplikacji (zobacz **rys.1**); Stany innych po prostu, że wystąpił błąd czasu wykonywania (patrz  **Rysunek 2**).

Szczegóły wyjątku YSOD jest bardzo przydatne w przypadku deweloperom debugowanie aplikacji, ale wyświetlanie YSOD dla użytkowników końcowych jest tacky i nieprofesjonalnie. Zamiast tego użytkownicy końcowi należy podjąć w celu strona błędu, która obsługuje witryny wygląd i działanie przy użyciu prose bardziej przyjazny dla użytkownika zawierająca opis sytuacji. Dobra wiadomość jest taka, jest całkiem proste tworzenie strony błędu niestandardowego. Ten samouczek rozpoczyna się od przyjrzeć się ASP. Strony błędów różnych w sieci. Go następnie pokazano, jak skonfigurować aplikację sieci web, aby wyświetlić użytkowników niestandardowej strony błędu w przypadku błędu.

### <a name="examining-the-three-types-of-error-pages"></a>Badanie trzy typy stron błędów

Kiedy nieobsłużony wyjątek pojawia się w aplikacji ASP.NET w jednym z trzech typów stron błędów, które są wyświetlane:

- Strona błędu wyjątku szczegóły żółty ekranem śmierci,
- Strony błędów środowiska uruchomieniowego błąd żółty ekranem śmierci, lub
- Strona błędu niestandardowego

Deweloperzy strony błędu są najbardziej znanych jest YSOD szczegóły wyjątków. Domyślnie ta strona będzie wyświetlana użytkownikom, którzy odwiedzają lokalnie i w związku z tym jest strona, która zostanie wyświetlony, gdy wystąpi błąd podczas testowania lokacji w środowisku programistycznym. Jak sugeruje jej nazwa, YSOD szczegóły wyjątków zawiera szczegółowe informacje o wyjątku — typ, wiadomości i ślad stosu. Co więcej jeśli wyjątek został zgłoszony przez kod w klasie CodeBehind strony ASP.NET, a aplikacja jest skonfigurowana do debugowania następnie YSOD szczegóły dotyczące wyjątków zostaną wyświetlone również ten wiersz kodu (i kilka wierszy kodu powyżej i poniżej).

**Rysunek 1** stronę YSOD szczegóły wyjątków. Zanotuj adres URL w przeglądarce adres: `http://localhost:62275/Genre.aspx?ID=foo`. Pamiętamy `Genre.aspx` stronie znajduje się lista przeglądów książki określonego rodzaju. Wymaga, aby `GenreId` wartość ( `uniqueidentifier`) przekazany ciąg zapytania; na przykład używając odpowiedniego adresu URL, aby wyświetlić te przeglądy fikcja jest `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Jeśli innej niż`uniqueidentifier` wartość jest przekazywana w za pośrednictwem ciąg zapytania (na przykład "foo") jest zgłaszany wyjątek.

> [!NOTE]
> Aby odtworzyć ten błąd wystąpił w demonstracyjnej aplikacji internetowej dostępne do pobrania można albo odwiedź `Genre.aspx?ID=foo` bezpośrednio lub kliknij łącze "Generowanie błąd środowiska uruchomieniowego" w `Default.aspx`.


Należy pamiętać, informacje o wyjątku, przedstawione **rys.1**. Komunikat o wyjątku, "Konwersja nie powiodła się podczas konwertowania ciągu znaków na wartość uniqueidentifier" jest obecny w górnej części strony. Typ wyjątku, `System.Data.SqlClient.SqlException`, znajduje się, jak również. Również znajduje się ślad stosu.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Rysunek 1**: Szczegóły wyjątku YSOD zawiera informacje o wyjątku  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image3.png))

Inny rodzaj YSOD jest YSOD błąd środowiska uruchomieniowego i jest wyświetlany w **na rysunku 2**. YSOD błąd środowiska uruchomieniowego informuje obiekt odwiedzający, który wystąpił błąd w czasie wykonywania, ale nie zawiera żadnych informacji o wyjątku, który został zgłoszony. (Jednak zawierają instrukcje dotyczącymi szczegóły błędu, modyfikując widoczne `Web.config` pliku, który jest częścią co sprawia, że takie YSOD, Szukaj nieprofesjonalnie.)

Domyślnie YSOD błąd środowiska uruchomieniowego jest wyświetlana dla użytkowników odwiedzających zdalnie (za pośrednictwem http://www.yoursite.com), zgodnie z dowodem na adres URL w pasku adresu przeglądarki w **na rysunku 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Dwa różne ekrany YSOD istnieje, ponieważ deweloperzy mają chcą wiedzieć, szczegóły błędu, ale informacje te nie mają być wyświetlane na aktywnej witryny, ponieważ może spowodować ujawnienie potencjalnych luk w zabezpieczeniach lub innych informacji poufnych dla wszystkich osób odwiedzających usługi witryna.

> [!NOTE]
> Jeśli postępujesz zgodnie z i używają DiscountASP.NET jako hosta sieci web, można zauważyć, że YSOD błąd środowiska uruchomieniowego nie są wyświetlane podczas przeglądania witryny na żywo. Jest to spowodowane DiscountASP.NET ma swoje serwery domyślnie skonfigurowana do wyświetlenia YSOD szczegóły wyjątków. Dobra wiadomość jest taka, możesz to zachowanie domyślne można przesłonić, dodając `<customErrors>` sekcję usługi `Web.config` pliku. Sprawdza, czy w sekcji "Konfigurowanie którego strony zostanie wyświetlony błąd" `<customErrors>` sekcji szczegółowo.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Rysunek 2**: błąd w czasie wykonywania YSOD nie zawiera żadnych szczegółów błędu  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image6.png))

Trzeci typ strony błędu jest stronę błędu niestandardowego, czyli strony sieci web, którą tworzysz. Zaletą korzystania z niestandardowej strony błędu jest, że masz pełną kontrolę nad informacje, które jest wyświetlane użytkownikowi, wraz ze strony wygląd i działanie; Strona błędu niestandardowego, można użyć tej samej strony wzorcowej i style jako stron sieci. W sekcji "Za pomocą niestandardowej strony błędu" przeprowadzi Tworzenie niestandardowej strony błędu i konfigurując go do wyświetlenia w przypadku nieobsługiwanego wyjątku. **Rysunek 3** udostępnia informacje szczytu tej strony błędu niestandardowego. Jak widać, to wygląd i działanie strony błędu, znacznie więcej profesjonalnych niż albo żółty ekrany śmierci przedstawione na rysunkach 1 i 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Rysunek 3**: niestandardowej strony błędu oferuje lepiej dopasowane wygląd i działanie  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image9.png))

Poświęć chwilę, aby sprawdzić paska adresu w przeglądarce w **rysunek 3**. Należy pamiętać, że na pasku adresu zawiera adres URL strony błędu niestandardowego (`/ErrorPages/Oops.aspx`). Na rysunkach 1 i 2 ekrany żółty śmierci są wyświetlane w tej samej stronie, błąd pochodzących z (`Genre.aspx`). Strona błędu niestandardowego jest przekazany adres URL strony, w którym wystąpił błąd przy użyciu `aspxerrorpath` parametr querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Konfigurowanie którego strony błędu jest wyświetlana.

Co trzy strony możliwy błąd ma być wyświetlane opiera się na dwóch zmiennych:

- Informacje o konfiguracji w `<customErrors>` sekcji i
- Czy użytkownik jest następującej witrynie, lokalnie lub zdalnie.

[ `<customErrors>` Sekcji](https://msdn.microsoft.com/library/h0hfz6fc.aspx) w `Web.config` ma dwa atrybuty, które mają wpływ na stronę błędu, jakie jest wyświetlany: `defaultRedirect` i `mode`. `defaultRedirect` Atrybut jest opcjonalny. Jeśli nie dostarczono, określa adres URL strony błędu niestandardowego i wskazuje, że zamiast YSOD błąd środowiska uruchomieniowego powinna być wyświetlana strona błędu niestandardowego. `mode` Atrybut jest wymagany i przyjmuje jedną z trzech wartości: `On`, `Off`, lub `RemoteOnly`. Te wartości mogą być następujące zachowanie:

- `On` -wskazuje stronę błędu niestandardowego lub YSOD błąd środowiska uruchomieniowego jest wyświetlany dla wszystkich osób odwiedzających, niezależnie od tego, czy są one lokalnym lub zdalnym.
- `Off` -Określa, czy wyjątek YSOD szczegóły są wyświetlane dla wszystkich osób odwiedzających, niezależnie od tego, czy są one lokalnym lub zdalnym.
- `RemoteOnly` — Wskazuje, że strony błędów niestandardowych lub YSOD błąd środowiska uruchomieniowego jest wyświetlany osobom odwiedzającym zdalnego podczas YSOD szczegółów wyjątku jest wyświetlany osobom odwiedzającym lokalnego.

O ile nie określono inaczej, platformy ASP.NET działa tak, jakby atrybut tryb zostały ustawione `RemoteOnly` i nie podał `defaultRedirect` wartość. Oznacza to domyślnym zachowaniem jest, że podczas YSOD błąd środowiska uruchomieniowego jest wyświetlany osobom odwiedzającym zdalnego YSOD szczegółów wyjątku jest wyświetlany osobom odwiedzającym lokalnego. To zachowanie domyślne można przesłonić, dodając `<customErrors>` sekcji do aplikacji sieci web `Web.config file.`

## <a name="using-a-custom-error-page"></a>Za pomocą strony błędu niestandardowego

Każda aplikacja sieci web powinien mieć niestandardowej strony błędu. Zapewnia bardziej profesjonalnych zamiast YSOD błąd środowiska uruchomieniowego, ułatwia tworzenie i konfigurowanie aplikacji do korzystania z niestandardowej strony błędu zajmuje tylko chwilę. Pierwszym krokiem jest utworzenie niestandardowej strony błędu. Nowy folder zostały dodane do aplikacji przeglądy książki, o nazwie `ErrorPages` i dodane do, że nowa strona programu ASP.NET o nazwie `Oops.aspx`. Jeszcze strony, użyj tej samej strony wzorcowej jako pozostałe strony w witrynie tak, aby automatycznie dziedziczy tego samego wygląd i działanie.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Rysunek 4**: Tworzenie niestandardowej strony błędu

Następnie Poświęć kilka minut, tworzenie zawartości strony błędu. Zamiast prostych niestandardowej strony błędu został utworzony za pomocą komunikat wskazujący, że wystąpił nieoczekiwany błąd i łącze z powrotem do strony głównej witryny.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Rysunek 5**: Projektowanie strony błędu niestandardowego  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image14.png))

Za pomocą strony błędu, zakończone należy skonfigurować aplikację sieci web, aby użyć niestandardowej strony błędu audytów YSOD błąd środowiska uruchomieniowego. Jest to osiągnąć, określając adres URL strony błędu w `<customErrors>` sekcji `defaultRedirect` atrybutu. Dodaj następujący kod do swojej aplikacji `Web.config` pliku:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Powyższe znaczników konfiguruje stosowanie przez aplikację do przedstawienia YSOD szczegółów wyjątku użytkowników odwiedzających lokalnie, podczas korzystania z niestandardowej strony błędu Oops.aspx dla tych użytkowników, odwiedzając zdalnie. Aby to zobaczyć w działaniu, wdrażanie witryny sieci Web w środowisku produkcyjnym, a następnie odwiedź stronę Genre.aspx w aktywnej witrynie z wartością nieprawidłowy ciąg zapytania. Powinna zostać wyświetlona strona błędu niestandardowego (odnoszą się do **rysunek 3**).

Aby sprawdzić, stronę błędu niestandardowego jest wyświetlana tylko do użytkowników zdalnych, odwiedź stronę `Genre.aspx` strony nieprawidłowy ciąg zapytania ze środowiska projektowego. Nadal powinien zostać wyświetlony YSOD szczegółów wyjątku (odnoszą się do **rys.1**). `RemoteOnly` Ustawienie zapewni, że w środowisku produkcyjnym, odwiedzając witrynę widzą strony błędu niestandardowego podczas deweloperów pracujących lokalnie w dalszym ciągu widzieć szczegóły wyjątku.

## <a name="notifying-developers-and-logging-error-details"></a>Powiadamianie deweloperów i rejestrowanie szczegółów błędu

Błędy występujące w środowisku programistycznym były spowodowane przez dewelopera, dostępne na swoim komputerze. Ona przedstawiono informacje o wyjątku w YSOD szczegółów wyjątku i wie, jakie kroki ona wykonywała w momencie wystąpienia błędu. Jednak po wystąpieniu błędu w środowisku produkcyjnym, deweloper nie ma informacji, chyba że użytkownik końcowy w witrynie zajmuje czasu, aby zgłosić błąd wystąpił błąd. A nawet wtedy, gdy użytkownik wykracza poza sposób alert zespołem programistycznym, który wystąpił błąd, nie wiedząc o tym, typ wyjątku, komunikat i ślad stosu, może być trudne przyczynę błędu, nie mówiąc go naprawić.

Z tych powodów, dla których najważniejsze, że wszelkie błędy w środowisku produkcyjnym zostanie zarejestrowany niektóre trwałego magazynu (np. bazy danych), a deweloperzy są alerty wystąpienia tego błędu. Strona błędu niestandardowego może się wydawać dobrym miejscem do śledzenia tego rejestrowania i powiadomień. Niestety stronę błędu niestandardowego nie ma dostępu do szczegóły błędu i nie można używać do logowania się te informacje. Dobra wiadomość jest taka, istnieje kilka sposobów przechwytywać szczegóły błędu i je rejestrować i zapoznaj się z trzech kolejnych samouczków w tym temacie bardziej szczegółowo.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Za pomocą różnych błędów niestandardowych stron dla stanów różnych błędów HTTP

Gdy wyjątek jest generowany przez strony ASP.NET i nie jest obsługiwany, wyjątek percolates do środowiska uruchomieniowego programu ASP.NET, która zostanie wyświetlona strona błędu skonfigurowany. Żądanie trafia do aparatu programu ASP.NET, ale nie można przetworzyć jakiegoś powodu — prawdopodobnie żądany plik jest nie można odnaleźć lub odczytać uprawnienia zostały wyłączone dla pliku — a następnie wywołuje aparat ASP.NET `HttpException`. Ten wyjątek, takich jak wyjątki wywoływane ze stron ASP.NET rozpropagowany do środowiska uruchomieniowego, powodując stronę odpowiedni komunikat o błędzie do wyświetlenia.

Co to oznacza dla aplikacji sieci web w środowisku produkcyjnym jest to, że jeśli użytkownik zażąda strona, która nie zostanie znaleziony, a następnie ich zostanie wyświetlona strona błędu niestandardowego. **Rysunek 6** przedstawiono przykład takiego. Ponieważ żądanie dotyczy nieistniejącej strony (`NoSuchPage.aspx`), `HttpException` jest generowany i zostanie wyświetlona strona błędu niestandardowego (należy pamiętać, odwołanie do `NoSuchPage.aspx` w `aspxerrorpath` parametr querystring).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Rysunek 6**: środowisko uruchomieniowe ASP.NET Wyświetla skonfigurowane strony w odpowiedzi na błąd nieprawidłowe żądanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image17.png))

Domyślnie wszystkie typy błędów spowodować, że ten sam niestandardowej strony błędu mają być wyświetlane. Można jednak określić różne niestandardowej strony błędu dla określonych HTTP status code za pomocą `<error>` elementy podrzędne w ramach `<customErrors>` sekcji. Na przykład strona inny błąd wyświetlana w przypadku strony błąd, mającej kod stanu HTTP 404 — Nie znaleziono aktualizacji `<customErrors>` sekcji, aby uwzględnić następujące znaczniki:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Dzięki tej zmianie w miejscu zawsze wtedy, gdy użytkownik odwiedzający zdalnie zażąda zasób programu ASP.NET, który nie istnieje, zostanie przekierowany do `404.aspx` niestandardowej strony błędu zamiast `Oops.aspx`. Jako **rysunek 7** ilustruje, `404.aspx` strony może zawierać bardziej szczegółowy komunikat o błędzie niż ogólna niestandardowej strony błędu.

> [!NOTE]
> Zapoznaj się z [404 stron błędów, jeden raz z bardziej](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) wskazówki dotyczące tworzenia skutecznych stron powodujących błąd 404.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Rysunek 7**: strony błędu niestandardowego 404 wyświetla komunikat bardziej precyzyjnie niż `Oops.aspx`  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image20.png)) 

Ponieważ wiadomo, że `404.aspx` strony jest osiągany dopiero po użytkownik wysyła żądanie dla strony, który nie został znaleziony, można zwiększyć tę stronę błędu niestandardowego, aby oferować takie funkcje, aby pomóc użytkownikowi adresu określonego typu błędu. Na przykład można tworzenie map znane nieprawidłowe adresy URL to dobry adresów URL tabeli bazy danych, a następnie `404.aspx` niestandardowej strony błędu uruchom zapytanie dla tabeli, która zasugerować stron, które użytkownik może próbować dotrzeć do.

> [!NOTE]
> Tylko zostanie wyświetlona strona błędu niestandardowego, po wysłaniu żądania do zasobu, obsługiwane przez aparat programu ASP.NET. Tak jak Omówiliśmy to w [podstawowe różnice między usług IIS i programem ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) samouczków, serwer sieci web mogą obsługiwać niektórych żądań sam. Domyślnie usługi IIS żądania sieci web serwera procesów dla zawartości statycznej, takich jak obrazy i pliki HTML bez wywoływania aparatu programu ASP.NET. W związku z tym jeśli użytkownik zażąda pliku obrazu nieistniejącej otrzymają one ponownie komunikat błędu 404 domyślny usług IIS, a nie ASP. Strona błędu skonfigurowany w sieci.


## <a name="summary"></a>Podsumowanie

Po wystąpieniu nieobsługiwanego wyjątku w aplikacji ASP.NET, użytkownik jest wyświetlany jeden z trzech stron błędów: wyjątek szczegóły żółty ekranem śmierci; Błąd w czasie wykonywania, żółte ekran; lub niestandardowej strony błędu. Zostanie wyświetlona strona błędu, który zależy od aplikacji `<customErrors>` konfiguracji i tego, czy użytkownik odwiedził lokalnie lub zdalnie. Zachowanie domyślne jest do przedstawienia YSOD szczegółów wyjątku do lokalnego odwiedzających i YSOD błąd środowiska uruchomieniowego odwiedzających zdalnego.

Gdy YSOD błąd środowiska uruchomieniowego ukrywa informacje o błędzie potencjalnie poufnych przez użytkownika w witrynie, przerywa z witryny wygląd i działanie i sprawia, że wyglądu buggy Twojej aplikacji. Lepszym rozwiązaniem jest użycie niestandardowej strony błędu, co pociąga za sobą tworzenia i projektowania strony błędu niestandardowego i określania jej adresu URL w `<customErrors>` sekcji `defaultRedirect` atrybutu. Można nawet mieć wiele stron błędów niestandardowych dla różnych stanów błąd HTTP.

Strona błędu niestandardowego jest pierwszym krokiem omyłkowo kompleksowej obsługi strategii dla witryny sieci Web w środowisku produkcyjnym. Alerty dla deweloperów błędu i rejestrowanie szczegółowe informacje są także ważne czynności. Techniki powiadomienia o błędzie i rejestrowania zapoznaj się z trzech kolejnych samouczków.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Strony błędów, jeszcze raz](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Wyjątki — zalecenia dotyczące projektowania](https://msdn.microsoft.com/library/ms229014.aspx)
- [Strony błędów przyjazny dla użytkownika](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Obsługa i zgłaszanie wyjątków](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Prawidłowo przy użyciu strony błędów niestandardowych w programie ASP.NET:](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Poprzednie](strategies-for-database-development-and-deployment-cs.md)
> [dalej](processing-unhandled-exceptions-cs.md)
