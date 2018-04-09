---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Strona błędu niestandardowego (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Co to widoczne dla użytkownika po wystąpił błąd środowiska uruchomieniowego w aplikacji sieci web ASP.NET? Odpowiedź zależy sposób witryny sieci Web &lt;customErrors&gt; konfiguracji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: f01a0f3af3680d53639512d7a86ac1a8645d00e2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-custom-error-page-c"></a>Strona błędu niestandardowego (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Co to widoczne dla użytkownika po wystąpił błąd środowiska uruchomieniowego w aplikacji sieci web ASP.NET? Odpowiedź zależy sposób witryny sieci Web &lt;customErrors&gt; konfiguracji. Domyślnie użytkownicy otrzymują ciemniejszego ekranu żółty proclaiming wystąpił błąd w czasie wykonywania. W tym samouczku pokazano, jak dostosować te ustawienia do wyświetlania aesthetically czytelnych niestandardowej strony błędu odpowiadający witryny wyglądu i działania.


## <a name="introduction"></a>Wprowadzenie

W idealnych nie byłoby bez błędów czasu wykonywania. Programiści zapisać kod z n-argumentowości usterki i z sprawdzania poprawności danych wejściowych użytkownika niezawodne i zewnętrznych zasobów, takich jak serwery baz danych i serwerów poczty e-mail nigdy nie przejdzie w tryb offline. Oczywiście w rzeczywistości błędy są nieuniknione. Klasy w programie .NET Framework sygnał błąd przez Zgłaszanie wyjątku. Na przykład SqlConnection podczas wywoływania metody Open obiektu ustanawia połączenie określona przez ciąg połączenia bazy danych. Jednak jeśli bazy danych jest wyłączony lub poświadczenia w parametrach połączenia są nieprawidłowe następnie otwórz metoda zgłasza `SqlException`. Wyjątki mogą być obsługiwane za pomocą `try/catch/finally` bloków. Jeśli kod w `try` bloku zgłasza wyjątek, sterowanie jest przekazywane do bloku catch odpowiednie gdzie deweloper może próbować odzyskać sprawność po błędzie. Jeśli nie istnieje żadne odpowiadający mu blok catch, lub jeśli kod, który zgłosił wyjątek nie jest w bloku try, wyjątek percolates górę stosu wywołań search z `try/catch/finally` bloków.

Jeśli wyjątek propaguje do środowiska uruchomieniowego ASP.NET nie jest obsługiwane, [ `HttpApplication` klasy](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)w [ `Error` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) jest wywoływane i skonfigurowanego *strony błędu*  jest wyświetlany. Domyślnie platforma ASP.NET wyświetla stronę błędu, który affectionately nazywa się [żółty ekranu śmierci](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Dostępne są dwie wersje YSOD: jeden przedstawia szczegóły wyjątku, ślad stosu i inne informacje przydatne deweloperom debugowanie aplikacji (zobacz **rysunek 1**); Stany innych po prostu, że wystąpił błąd czasu wykonywania (zobacz  **Rysunek 2**).

Szczegóły wyjątku YSOD jest bardzo przydatne deweloperom debugowanie aplikacji, ale wyświetlanie YSOD użytkownikom końcowym jest tacky i szkodzi. Zamiast tego użytkownicy końcowi należy podjąć w celu przechowującą witryny wyglądu i działania z użytkownikom większy komfort prozie opisujące sytuacji stronę błędu. Dobre wieści jest to utworzenie strony błędu niestandardowego prostą. W tym samouczku rozpoczyna się od przyjrzeć się ASP. Strony błędów różnych w sieci. Następnie pokaże konfigurowania aplikacji sieci web do wyświetlenia użytkownikom niestandardowej strony błędu w wypadku wystąpił błąd.

### <a name="examining-the-three-types-of-error-pages"></a>Badanie trzy typy stron błędów

Gdy wystąpił nieobsługiwany wyjątek pojawia się w jednym z trzech typów stron błędów aplikacji ASP.NET jest wyświetlane:

- Strona błędu wyjątek szczegóły żółty ekranem śmierci,
- Strony błędów czasu wykonywania błąd żółty ekranem śmierci, lub
- Strona błędu niestandardowego

Deweloperzy strony błędu są najbardziej znany jest YSOD szczegóły wyjątku. Domyślnie ta strona będzie wyświetlana użytkownikom, którzy odwiedzają lokalnie i w związku z tym jest strona, która zostanie wyświetlona, gdy wystąpi błąd podczas testowania lokacji w środowisku programistycznym. Jak jego nazwa wskazuje, YSOD szczegóły wyjątku zawiera szczegółowe informacje o wyjątku — typ wiadomości i ślad stosu. Co więcej jeśli wyjątek został zgłoszony przez kod w klasie związanej z kodem strony ASP.NET i jeśli aplikacja jest skonfigurowana do debugowania następnie YSOD szczegóły wyjątku także wyświetli ten wiersz kodu (i zaledwie kilku wierszach kodu powyżej i poniżej).

**Rysunek 1** stronę YSOD szczegóły wyjątku. Zanotuj adres URL w przeglądarce adres: `http://localhost:62275/Genre.aspx?ID=foo`. Odwołania, który `Genre.aspx` strona zawiera listę przeglądami książki określonego rodzaju. Wymaga, aby `GenreId` wartość ( `uniqueidentifier`) jest przekazywane querystring; na przykład jest odpowiedni adres URL, aby wyświetlić przegląd fikcja `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Jeśli niż`uniqueidentifier` została przekazana wartość w za pośrednictwem querystring (np. "foo") jest zgłaszany wyjątek.

> [!NOTE]
> Aby odtworzyć ten błąd w aplikacji sieci web pokaz dostępny do pobrania można albo odwiedź `Genre.aspx?ID=foo` bezpośrednio lub kliknij łącze "Generuj błąd środowiska uruchomieniowego" w `Default.aspx`.


Należy zwrócić uwagę informacji przedstawionych w **rysunek 1**. Komunikat o wyjątku "Niepowodzenie konwersji ciągu znaków na unikatowy identyfikator" znajduje się w górnej części strony. Typ wyjątku, `System.Data.SqlClient.SqlException`, jest wyświetlana, jak również. Istnieje także ślad stosu.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Rysunek 1**: Szczegóły wyjątku YSOD zawiera informacje o wyjątku  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image3.png))

Typ YSOD jest YSOD błąd środowiska uruchomieniowego i jest wyświetlany w **na rysunku 2**. YSOD błąd środowiska uruchomieniowego informuje obiekt odwiedzający, który wystąpił błąd w czasie wykonywania, ale nie zawiera żadnych informacji o wyjątku, który został zgłoszony. (Jednak zawierają instrukcje dotyczące sposobu wprowadzania szczegóły błędu można przeglądać, modyfikując `Web.config` pliku, który jest częścią co sprawia, że takie YSOD, Szukaj szkodzi.)

Domyślnie YSOD błąd czasu wykonywania jest wyświetlana dla użytkowników odwiedzających zdalne (za pośrednictwem http://www.yoursite.com), potwierdzonych przez adres URL na pasku adresu przeglądarki w **na rysunku 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Dwóch różnych ekranach YSOD istnieje, ponieważ Deweloperzy są chciał wiedzieć, szczegóły błędu, ale informacje te nie mają być wyświetlane na działającą witrynę jako może ujawnić, potencjalnych luk w zabezpieczeniach lub inne poufne informacje dla wszystkich osób odwiedzających użytkownika lokacja.

> [!NOTE]
> Jeśli po i przy użyciu DiscountASP.NET jako hosta sieci web, możesz zauważyć, że YSOD błąd środowiska uruchomieniowego nie są wyświetlane podczas przeglądania witryny na żywo. Jest to spowodowane DiscountASP.NET zawiera ich serwerów do wyświetlenia YSOD szczegóły wyjątku jest domyślnie konfigurowana. Dobre wieści jest, że można zastąpić to zachowanie domyślne, dodając `<customErrors>` sekcji do Twojej `Web.config` pliku. Sprawdza, czy w sekcji "Konfigurowanie którego strona zostanie wyświetlony błąd" `<customErrors>` sekcji szczegółowo.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Rysunek 2**: błąd w czasie wykonywania YSOD nie zawiera żadnych szczegółów błędu  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image6.png))

Trzeci typ strony błędu jest niestandardowej strony błędu, która jest tworzona strona sieci web. Zaletą stronę błędu niestandardowego jest, że mają pełną kontrolę nad informacje, które jest wyświetlane użytkownikowi wraz ze strony wygląd i działanie; strony błędów niestandardowych mogą używać tej samej strony wzorcowej i style jako stron sieci. W sekcji "Przy użyciu niestandardowej strony błędu" przeprowadzi Cię przez tworzenie niestandardowej strony błędu i konfigurowanie go w celu wyświetlania w przypadku nieobsługiwany wyjątek. **Rysunek 3** oferuje wtedy szczytu tej strony błędu niestandardowego. Jak widać, wygląd i działanie strony błędu jest znacznie więcej profesjonalnych niż albo żółty ekrany śmierci pokazano na rysunkach 1 i 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Rysunek 3**: stronę błędu niestandardowego oferuje bardziej dopasowane wygląd i działanie  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image9.png))

Poświęć chwilę, aby sprawdzić paska adresu przeglądarki w **rysunek 3**. Należy pamiętać, że na pasku adresu zawiera adres URL strony błędu niestandardowego (`/ErrorPages/Oops.aspx`). Na rysunkach 1 i 2 w tej samej stronie zainicjowanej błąd przedstawiono ekrany żółty śmierci (`Genre.aspx`). Strona błędu niestandardowego jest przekazany adres URL strony, w którym wystąpił błąd przy użyciu `aspxerrorpath` parametr querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Konfigurowanie strony błędu który jest wyświetlany

Które trzy strony może zawierać błąd jest wyświetlany jest oparty na dwóch zmiennych:

- Informacje o konfiguracji w `<customErrors>` sekcji i
- Określa, czy użytkownik jest tej witrynie lokalnie lub zdalnie.

[ `<customErrors>` Sekcji](https://msdn.microsoft.com/library/h0hfz6fc.aspx) w `Web.config` ma dwa atrybuty, które mają wpływ na jakie strony błędu jest wyświetlany: `defaultRedirect` i `mode`. `defaultRedirect` Atrybutu jest opcjonalny. Jeśli zostanie podana, określa adres URL strony błędu niestandardowego i wskazuje, że zamiast YSOD błąd środowiska uruchomieniowego powinna być wyświetlana strona błędu niestandardowego. `mode` Atrybut jest wymagany i przyjmuje jeden z trzech wartości: `On`, `Off`, lub `RemoteOnly`. Te wartości są następujące działania:

- `On` — Wskazuje, że strony błędów niestandardowych lub YSOD błąd czasu wykonywania jest pokazywane do wszystkich odwiedzających, niezależnie od tego, czy są one lokalnym lub zdalnym.
- `Off` — Określa, czy YSOD szczegóły wyjątku jest wyświetlana dla wszystkich odwiedzających, niezależnie od tego, czy są one lokalnym lub zdalnym.
- `RemoteOnly` — Wskazuje, że strony błędów niestandardowych lub YSOD błąd czasu wykonywania jest pokazywane zdalnego gościom podczas YSOD szczegóły wyjątku znajduje się do lokalnego odwiedzający.

O ile nie określono inaczej, ASP.NET działa tak, jakby ma ustawiony atrybut tryb `RemoteOnly` i nie podał `defaultRedirect` wartość. Domyślnym zachowaniem jest innymi słowy, że podczas YSOD błąd środowiska uruchomieniowego znajduje się do zdalnego odwiedzających YSOD szczegóły wyjątku jest wyświetlany gościom lokalnego. To zachowanie domyślne można przesłonić przez dodanie `<customErrors>` sekcji do aplikacji sieci web `Web.config file.`

## <a name="using-a-custom-error-page"></a>Za pomocą strony błędu niestandardowego

Każda aplikacja sieci web powinien mieć niestandardowej strony błędu. Zapewnia bardziej profesjonalnych zamiast YSOD błąd środowiska uruchomieniowego, ułatwia tworzenie i konfigurowanie aplikacji do korzystania z niestandardowej strony błędu zajmuje tylko kilka minut. Pierwszym krokiem jest utworzenie niestandardowej strony błędu. Nowy folder zostały dodane do wskazanej aplikacji przeglądami książki `ErrorPages` i dodane do, że nowa strona ASP.NET o nazwie `Oops.aspx`. Ma używać tej samej strony wzorcowej jak innych stron w witrynie, aby automatycznie dziedziczy tej samej wygląd i działanie strony.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Rysunek 4**: Tworzenie strony błędu niestandardowego

Następnie spędzają na kilka minut, tworzenie zawartości dla strony błędu. Zamiast prostego niestandardową stronę błędu został utworzony z komunikat wskazujący, że wystąpił nieoczekiwany błąd i łącze z powrotem do strony głównej witryny.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Rysunek 5**: Projekt strony błędu niestandardowego  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image14.png))

Strona błędu ukończone Konfigurowanie aplikacji sieci web, aby użyć niestandardowej strony błędu zamiast YSOD błąd czasu wykonywania. Jest to osiągnąć, określając adres URL strony błędu w `<customErrors>` sekcji `defaultRedirect` atrybutu. Dodaj następujący kod do aplikacji `Web.config` pliku:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Powyżej znaczników konfiguruje aplikację do wyświetlenia YSOD szczegóły wyjątku do użytkowników odwiedzających lokalnie, używając strony błędu niestandardowego Oops.aspx dla tych użytkowników, odwiedzając zdalnie. Aby wyświetlić to działanie, wdrażanie witryny sieci Web w środowisku produkcyjnym, a następnie odwiedź strony Genre.aspx na stronie z wartością nieprawidłowy ciąg querystring. Powinna zostać wyświetlona strona błędu niestandardowego (odwołują się do **rysunek 3**).

Aby sprawdzić, stronę błędu niestandardowego jest wyświetlana tylko do użytkowników zdalnych, odwiedź stronę `Genre.aspx` strony z nieprawidłowy ciąg querystring środowiska programowania. Powinny pojawić nadal YSOD szczegóły wyjątku (odwołują się do **rysunek 1**). `RemoteOnly` Ustawienie zapewni, że użytkowników odwiedzających witrynę w środowisku produkcyjnym zobacz stronę błędu niestandardowego nadal deweloperów pracujących lokalnie wyświetlić szczegóły wyjątku.

## <a name="notifying-developers-and-logging-error-details"></a>Powiadamianie o tym, deweloperzy i rejestrowanie szczegóły błędu

Błędy występujące w środowisku programistycznym zostały spowodowane przez dewelopera dostępne na swoim komputerze. Użytkownik przedstawiono informacje o wyjątku w YSOD szczegóły wyjątku i wie, jakie kroki ona wykonywała w momencie wystąpienia błędu. Jednak po wystąpieniu błędu w środowisku produkcyjnym, deweloper nie zna, wystąpił błąd, chyba że użytkownik końcowy w witrynie potrzebny jest czas na raport o błędzie. A nawet wtedy, gdy użytkownik przechodzi poza sposobem alertów zespół deweloperów, który wystąpił błąd, bez wiedzy o typ wyjątku, komunikat oraz może być trudne do diagnozowania przyczyny tego błędu nawet rozwiązywanie ślad stosu.

Z tego względu, który jest podstawowym, że niektóre magazynu trwałego (na przykład w bazie danych), a jest rejestrowana błędu w środowisku produkcyjnym Deweloperzy są alerty dotyczące tego błędu. Strona błędu niestandardowego może się wydawać dobrym miejscem do tego rejestrowania i powiadomień. Niestety stronę błędu niestandardowego nie ma dostępu do szczegóły błędu i nie można użyć do logowania się te informacje. Dobre wieści jest istnieją różne sposoby przechwytywać szczegóły błędu i je rejestrować, czy trzech kolejnych samouczkach Eksplorowanie w tym temacie bardziej szczegółowo.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Przy użyciu różnych niestandardowe strony błędów dla błędów HTTP różnych stanów

Jeśli wyjątek jest zgłaszany przez strony ASP.NET i nie jest obsługiwana, wyjątek percolates do środowiska uruchomieniowego ASP.NET, którego zostanie wyświetlona strona błędu skonfigurowany. Jeśli żądanie jest dostarczany do aparatu ASP.NET, ale nie można przetworzyć jakiegoś powodu — możliwe, że żądany plik nie znaleziono lub odczytać uprawnienia zostały wyłączone dla pliku — a następnie aparatu ASP.NET zgłasza `HttpException`. Propaguje tego wyjątku, takie jak wyjątki zgłoszone ze stron ASP.NET do środowiska wykonawczego, powodując strona błędu odpowiednie do wyświetlenia.

Co to znaczy dla aplikacji sieci web w środowisku produkcyjnym to, że jeśli użytkownik zażąda strony, które nie zostało odnalezione, a następnie odbiorcy zobaczą strony błędu niestandardowego. **Rysunek 6** przedstawiono przykład takiego. Ponieważ żądania jest strony nieistniejącą (`NoSuchPage.aspx`), `HttpException` jest zgłaszany i zostanie wyświetlona strona błędu niestandardowego (należy pamiętać, odwołanie do `NoSuchPage.aspx` w `aspxerrorpath` parametr querystring).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Rysunek 6**: środowiska uruchomieniowego ASP.NET Wyświetla skonfigurowane strony w odpowiedzi na błąd nieprawidłowe żądanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image17.png))

Domyślnie wszystkie typy błędów powodują tej samej strony błędu niestandardowego do wyświetlenia. Można jednak określić różne niestandardową stronę błędu dla określonych HTTP status kodu za pomocą `<error>` elementów podrzędnych w obrębie `<customErrors>` sekcji. Na przykład mieć inny błąd strony wyświetlany w przypadku strony, nie można odnaleźć błąd, który ma kod stanu HTTP 404, należy zaktualizować `<customErrors>` sekcji, aby uwzględnić następujący kod:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Dzięki tej zmianie w miejscu zawsze, gdy użytkownik odwiedzający zdalnie zażąda zasobów programu ASP.NET, który nie istnieje, zostanie przekierowany do `404.aspx` niestandardowej strony błędu zamiast `Oops.aspx`. Jako **na rysunku nr 7** ilustruje, `404.aspx` strona może zawierać więcej określonego komunikatu niż ogólna niestandardowej strony błędu.

> [!NOTE]
> Zapoznaj się z [404 stron błędów, jeden raz więcej](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) wskazówki dotyczące tworzenia skuteczne stron błąd 404.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Rysunek 7**: strony błędu niestandardowego 404, zostanie wyświetlony komunikat bardziej precyzyjnie niż `Oops.aspx`  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-custom-error-page-cs/_static/image20.png)) 

Ponieważ wiadomo, że `404.aspx` strony tylko jest osiągana, gdy użytkownik zażąda strony, który nie został znaleziony, można rozszerzyć tę stronę błędu niestandardowego, aby obejmuje funkcjonalności pomóc użytkownikowi adres tego typu określonego błędu. Na przykład można kompilacji mapy znane zły adresy URL do adresów URL dobrej tabeli bazy danych, a następnie `404.aspx` niestandardowej strony błędu Uruchom zapytania dotyczącego tabeli i sugeruje stron, które użytkownik może próba nawiązania połączenia.

> [!NOTE]
> Tylko zostanie wyświetlona strona błędu niestandardowego, po wysłaniu żądania do zasobu obsługiwane przez aparat programu ASP.NET. Jak wspomniano w [podstawowe różnice między usług IIS i ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) samouczek, serwer sieci web mogą obsługiwać niektórych żądań samej siebie. Domyślnie usługi IIS żądania sieci web serwera procesów dla zawartości statycznej, takich jak obrazy i pliki HTML bez wywoływania aparatu ASP.NET. W związku z tym jeśli użytkownik zażąda pliku obrazu nieistniejącą one będzie odzyskać przez usługi IIS domyślnego 404 komunikatu o błędzie zamiast ASP. Strona błędu skonfigurowanych w sieci.


## <a name="summary"></a>Podsumowanie

Gdy wystąpi nieobsługiwany wyjątek w aplikacji ASP.NET, użytkownik jest wyświetlany jeden z trzech stron błędów: wyjątek szczegóły żółty ekranem śmierci; Błąd w czasie wykonywania żółty ekran; lub niestandardową stronę błędu. Zostanie wyświetlona strona błędu, który jest zależny od aplikacji `<customErrors>` konfiguracji i czy użytkownik jest odwiedzający lokalnie lub zdalnie. Domyślnym zachowaniem jest wyświetlenie YSOD szczegóły wyjątku do lokalnego odwiedzających YSOD błąd środowiska uruchomieniowego gościom zdalnego.

Gdy YSOD błąd środowiska uruchomieniowego ukrywa informacje o błędzie potencjalnie poufnych użytkownika, w tej witrynie, dzieli z witryny wyglądu i działania i sprawia, że aplikacja Szukaj buggy. Lepszym rozwiązaniem jest użycie niestandardowej strony błędu, który pociąga za sobą konieczność tworzenia i projektowania strony błędu niestandardowego i określania adresu URL w `<customErrors>` sekcji `defaultRedirect` atrybutu. Można także ustawić wiele stron błędów niestandardowych dla poszczególnych stanów błędu HTTP.

Niestandardowej strony błędu jest to pierwszy etap kompleksowe błąd obsługi strategii dla witryny sieci Web w środowisku produkcyjnym. Alerty developer błędu i rejestrowanie jego szczegóły są również ważne czynności. Samouczki trzech kolejnych Poznaj techniki powiadomienie o błędzie i rejestrowania.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Strony błędów, jeszcze raz](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Wyjątki — zalecenia dotyczące projektowania](https://msdn.microsoft.com/library/ms229014.aspx)
- [Strony błędów przyjazną dla użytkownika](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Obsługa i zgłaszanie wyjątków](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Prawidłowo przy użyciu strony błędów niestandardowych w programie ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Poprzednie](strategies-for-database-development-and-deployment-cs.md)
> [dalej](processing-unhandled-exceptions-cs.md)
