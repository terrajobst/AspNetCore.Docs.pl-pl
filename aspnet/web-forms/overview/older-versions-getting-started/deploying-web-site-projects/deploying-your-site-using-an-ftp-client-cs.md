---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Wdrażanie witryny przy użyciu klienta FTP (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Najprostszym sposobem wdrażania aplikacji ASP.NET jest ręcznie skopiuj niezbędne pliki ze środowiska projektowego w środowisku produkcyjnym. Ten...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 50e1522a39cb7b8ea2ee4c664f166b45f22ff070
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842286"
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>Wdrażanie witryny przy użyciu klienta FTP (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> Najprostszym sposobem wdrażania aplikacji ASP.NET jest ręcznie skopiuj niezbędne pliki ze środowiska projektowego w środowisku produkcyjnym. W tym samouczku pokazano, jak pobierać pliki z komputera do dostawcy hosta sieci web za pomocą klienta FTP.


## <a name="introduction"></a>Wprowadzenie

Do poprzedniego samouczka wprowadziliśmy prostą aplikację sieci web ASP.NET przeglądu książki, który składa się kilka stron ASP.NET, strony wzorcowej, niestandardowe baza `Page` klasy liczbę obrazów, i arkusze stylów CSS trzy. Teraz jesteśmy gotowi wdrożyć tę aplikację sieci web dostawcy hosta, w tym momencie aplikacji będą dostępne dla wszystkich osób z połączeniem z Internetem!


Z naszych rozmów w [ *określająca, które pliki muszą zostać wdrożone* ](determining-what-files-need-to-be-deployed-cs.md) samouczków, wiemy, że pliki muszą być kopiowane do dostawcy hosta sieci web. (Pamiętaj, że jakie pliki są kopiowane zależy od tego, czy aplikacja jest jawnie lub automatycznie kompilowana). Ale jak możemy doprowadzić pliki ze środowiska projektowego (nasze dla komputerów stacjonarnych) do środowiska produkcyjnego (zarządzane przez dostawcę sieci web hosta serwera sieci web)? [ **F** IK **T** przesyłania **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) jest protokołem często używane do kopiowania plików z jednego komputera do drugiego za pośrednictwem sieci. Innym rozwiązaniem jest rozszerzeń serwera FrontPage (FPSE). Ten samouczek koncentruje się na temat korzystania z autonomicznej oprogramowanie klienckie FTP do wdrażania niezbędne pliki ze środowiska projektowego w środowisku produkcyjnym.

> [!NOTE]
> Program Visual Studio zawiera narzędzia służące do publikowania witryny sieci Web za pośrednictwem protokołu FTP; te narzędzia, a także poznać narzędzia, które używają FPSE, zostały uwzględnione w następnym samouczku.


Aby skopiować pliki przy użyciu protokołu FTP, potrzebujemy *klienta FTP* w środowisku programistycznym. Klient FTP to aplikacja, która jest przeznaczona do kopiowania plików na komputerze jest zainstalowany na komputerze, na którym uruchomiono *serwera FTP*. (Jeśli Twój dostawca hosta sieci web obsługuje transfery plików za pośrednictwem protokołu FTP, tak jak większość, następnie jest uruchomione na serwerach sieci web serwera FTP.) Liczba aplikacje klienckie FTP są dostępne. Przeglądarki sieci web, można nawet dwukrotnie jako klient FTP. Moje ulubione klienta FTP i co mogę używać na potrzeby tego samouczka jest [FileZilla](http://filezilla-project.org/), bezpłatny, open source klienta FTP, który jest dostępny dla Windows, Linux i Mac. Dowolny klient FTP będzie działać, więc możesz użyć dowolnego klienta potrafisz najbardziej.

Jeśli wykonujesz wzdłuż można będzie potrzebne tworzenia konta u dostawcy usług hosta sieci web przed można wykonać w tym samouczku lub kolejne. Jak wspomniano w poprzednim samouczku, są gaggle firm dostawcy hosta sieci web, za pomocą szerokie spektrum ceny, funkcje i jakości usługi. W tej serii samouczków I będzie korzystać z [ASP.NET z rabatami](http://discountasp.net) jako Mój hosta sieci web dostawcy, ale skorzystaj z dowolnego dostawcy hosta sieci web tak długo, jak obsługują verze technologie ASP.NET opracowanym w lokacji. (Te samouczki zostały utworzone przy użyciu ASP.NET 3.5.) Ponieważ firma Microsoft będzie można kopiowanie plików do dostawcy hosta sieci web przy użyciu protokołu FTP, w tym samouczku i w przyszłości takich, które jest także imperatywnego, że Twój dostawca hosta sieci web obsługuje FTP dostęp do swoich serwerów sieci web. Praktycznie wszystkich dostawców usług hosta sieci web oferuje tę funkcję, ale powinien być dokładnie przed utworzeniem nowego.

## <a name="deploying-the-book-review-web-application-project"></a>Wdrażanie projektu aplikacji sieci Web Przejrzyj książki

Pamiętaj, że istnieją dwie wersje książki przeglądanie aplikacji sieci web: jeden implementowane przy użyciu modelu projektu aplikacji sieci Web (BookReviewsWAP), a druga za pomocą modelu projektu witryny sieci Web (BookReviewsWSP). Typ projektu ma wpływ, czy witryna jest kompilowany automatycznie lub jawnie i modelu kompilacji decyduje, które pliki muszą zostać wdrożone. W związku z tym będziemy sprawdzać wdrażania projektów BookReviewsWAP i BookReviewsWSP oddzielnie, począwszy od BookReviewsWAP. Poświęć chwilę, aby pobrać te dwie aplikacje ASP.NET, jeśli nie zostało to jeszcze zrobione.

Uruchom projekt BookReviewsWAP, przechodząc do `BookReviewsWAP` folder i dwukrotne kliknięcie `BookReviewsWAP.sln` pliku. Przed przystąpieniem do wdrażania projektu jest ważne, aby skompiluj go, aby upewnić się, że zmiany do kodu źródłowego są uwzględniane w skompilowanym zestawie. Aby skompilować projekt przejdź do menu kompilacji, a następnie wybierz opcję menu BookReviewsWAP kompilacji. To kompiluje kod źródłowy w projekcie w jednym zestawie `BookReviewsWAP.dll`, który jest umieszczany w `Bin` folderu.

Teraz możemy przystąpić do wdrażania niezbędne pliki! Uruchom klienta FTP i połączyć się z serwerem sieci web u Twojego dostawcy hosta sieci web. (Po zarejestrowaniu się przy użyciu hostingu firmy mogą Ci wiadomość e-mail informacji na temat łączenia się z serwerem FTP; obejmuje to adres serwera FTP oraz nazwy użytkownika i hasło).

Skopiuj następujące pliki z komputera do głównego folderu witryny sieci Web u Twojego dostawcy hosta sieci web. Jeśli FTP do serwera sieci web w sieci web dostawcy prawdopodobnie w katalogu głównym witryny sieci Web. Jednak niektóre dostawców usług hosta sieci web ma podfolder o nazwie `www` lub `wwwroot` służy jako folder główny dla plików witryny sieci Web. Na koniec podczas FTPing plików może być konieczne utworzenie odpowiedniego struktury folderów w środowisku produkcyjnym — `Bin` folderze `Fiction` folderze `Images` folder i tak dalej.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Pełna zawartość `Styles` folderu
- Pełna zawartość `Images` folder (oraz w jego podfolderze `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Rysunek 1 pokazuje FileZilla po niezbędne pliki zostały skopiowane. FileZilla Wyświetla pliki na komputerze lokalnym z lewej strony oraz pliki na komputerze zdalnym po prawej stronie. Jak rysunek 1 pokazuje, w plikach kodu źródłowego programu ASP.NET, takich jak `About.aspx.cs`, znajdują się na komputerze lokalnym (środowisko programistyczne), ale nie zostały skopiowane do dostawcy hosta sieci web (środowisko produkcyjne), ponieważ nie trzeba wdrożyć, korzystając z plików kodu kompilację typu Explicit.

> [!NOTE]
> Jak są one ignorowane nie powoduje żadnych problemów, że w plikach kodu źródłowego na serwerze produkcyjnym. ASP.NET zabrania żądania HTTP do plików kodu źródłowego domyślnie, tak aby nawet, jeśli pliki kodu źródłowego są obecne na serwerze produkcyjnym są niedostępne dla odwiedzający witrynę sieci Web. (To znaczy, jeśli użytkownik próbuje znaleźć `http://www.yoursite.com/Default.aspx.cs` otrzymają one stronę błędu, który objaśnia, że te typy plików — `.cs` plików — jest zabronione.)


[![Klient FTP umożliwia skopiuj niezbędne pliki z komputera z serwerem sieci Web u dostawcy hosta sieci Web](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Rysunek 1**: używany klient FTP do skopiowania niezbędne pliki swój pulpit na serwerze sieci Web u dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


Po wdrożeniu witryny Poświęć chwilę, aby przetestować w witrynie. Jeśli możesz zakupić nazwę domeny i skonfigurowano ustawienia DNS poprawnie, możesz odwiedzić stronę witryny, wprowadzając nazwę domeny. Alternatywnie dostawcą hosta sieci web powinna wprowadzonych w możesz przy użyciu adresu URL do witryny, której będą wyglądać mniej więcej tak *accountname*. *webhostprovider*.com lub *webhostprovider*.com /*accountname*. Na przykład adres URL mojego konta na platformie ASP.NET z rabatu jest: `http://httpruntime.web703.discountasp.net`.

Rysunek 2 przedstawia witrynę wdrożoną przeglądy książki. Należy pamiętać o tym, czy podczas oglądania go na Discount ASP. Serwery w sieci u `http://httpruntime.web703.discountasp.net`. W tym momencie każdy z połączeniem z Internetem może wyświetlać Moja witryna sieci Web! Czy oczekujemy, witryna wygląda i zachowuje się tak samo jak podczas testowania go w środowisku programistycznym.

> [!NOTE]
> Jeśli wystąpi błąd podczas wyświetlania aplikacji Poświęć chwilę, aby upewnić się, że Ci się wdrożyć poprawny zestaw plików. Następnie sprawdź komunikat o błędzie, aby zobaczyć, jeśli go, co spowoduje wyświetlenie wszelkie wskazówki dotyczące problemu. Poniżej można włączyć do działu pomocy technicznej Twojej firmy hosta sieci web lub opublikuj swoje pytanie na forum odpowiednie [fora ASP.NET](https://forums.asp.net/).


[![Witryna przeglądy książki jest teraz dostępny dla każdego, kto połączenie z Internetem](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Rysunek 2**: lokacji przeglądy książki jest teraz dostępny dla każdego, kto połączenie z Internetem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Wdrażanie projektu witryny sieci Web Przejrzyj książki

W przypadku wdrażania aplikacji platformy ASP.NET, która używa automatyczne kompilowanie, takich jak BookReviewsWSP projektu witryny sieci Web, nie ma żadnych skompilowanego zestawu w `Bin` folderu. W rezultacie pliki kodu źródłowego aplikacji sieci web musi zostać wdrożony w środowisku produkcyjnym. Przejdźmy teraz przez ten proces.

Jak za pomocą projektu aplikacji sieci Web jest poddanie pierwsza Kompilacja aplikacji przed jego wdrożeniem. Podczas kompilowania projektu witryny sieci Web nie powoduje utworzenia zestawu, sprawdza pod kątem błędów kompilacji, na stronie. Lepsze można teraz znaleźć te błędy, a nie o odwiedzającą witrynę odnajdywać dla Ciebie!

Po pomyślnym utworzeniu projektu, umożliwia klienta FTP skopiuj następujące pliki do głównego folderu witryny sieci Web u Twojego dostawcy hosta sieci web. Może być konieczne utworzenie odpowiedniego struktury folderów w środowisku produkcyjnym.

> [!NOTE]
> Jeśli wdrożono już BookReviewsWAP projektu ale chcesz wypróbować wdrażanie projektu BookReviewsWSP, najpierw usuń wszystkie pliki na serwerze sieci web, które zostały przekazane podczas wdrażania BookReviewsWAP, a następnie wdrożyć pliki dla BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Pełna zawartość `Styles` folderu
- Pełna zawartość `Images` folder (oraz w jego podfolderze `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

Rysunek 3 przedstawia FileZilla po skopiowaniu zapasowej wymaganych plików. Jak widać, platformy ASP.NET pliki kodów źródłowych, takich jak `About.aspx.cs`, znajdują się na komputerze lokalnym (środowisko programistyczne) i dostawcy hosta sieci web (środowisko produkcyjne), ponieważ pliki kodu, które muszą zostać wdrożone w przypadku korzystania z automatycznego Kompilacja.


[![Klient FTP umożliwia skopiuj niezbędne pliki z komputera z serwerem sieci Web u dostawcy hosta sieci Web](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Rysunek 3**: używany klient FTP do skopiowania niezbędne pliki swój pulpit na serwerze sieci Web u dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


Środowisko użytkownika nie ma wpływu modelu kompilacji aplikacji. Tej samej strony ASP.NET są dostępne, a ich wygląd i działa tak samo, czy witryna sieci Web została utworzona przy użyciu modelu projektu aplikacji sieci Web lub modelu projektu witryny sieci Web.

### <a name="updating-a-web-application-on-production"></a>Aktualizowanie aplikacji sieci Web w środowisku produkcyjnym

Opracowywanie aplikacji sieci Web i wdrażania są jednorazowy proces. Na przykład podczas tworzenia witryny sieci Web Przejrzyj książki I oparta na różnych stronach i napisał towarzyszący kod na Mój komputer osobisty (środowisko programistyczne). Po osiągnięciu niektórych stabilne, wdrożono aplikację tak, aby inne osoby mogą odwiedź witrynę i odczytu Moje recenzje. Ale wdrożenia nie oznacza koniec Moje rozwoju w tej witrynie. Czy mogę dodać więcej recenzji książki lub implementowania nowych funkcji, na przykład pozwala Moje osoby odwiedzające współczynnik książki lub umieszczać swoje własne komentarze. Takie ulepszenia będą opracowywane w środowisku deweloperskim, a po zakończeniu musi zostać wdrożone. Programowania i wdrażania, dlatego są cykliczne. Tworzenie aplikacji, a następnie wdrożyć go. Gdy lokacja znajduje się na żywo i w środowisku produkcyjnym są dodawane nowe funkcje i błędy zostały usunięte wraz z upływem czasu, która wymaga ponownego wdrażania aplikacji. I tak dalej i tak dalej.

Zgodnie z oczekiwaniami, podczas ponownego wdrażania aplikacji sieci web, należy skopiować nowe i zmienione pliki. Nie ma potrzeby ponownie wdrożyć stron bez zmian lub pliki obsługi serwera lub klienta (mimo że nie ma żadnych szkód w ten sposób).

> [!NOTE]
> Warto pamiętać podczas przy użyciu kompilację typu explicit jest dowolnym Dodawanie nowej strony programu ASP.NET do projektu, lub wprowadzić zmiany dotyczące kodu, należy ponownie skompilować projekt, który aktualizuje zestaw na `Bin` folderu. W związku z tym należy skopiować tego zaktualizowanego zestawu do produkcji, podczas aktualizowania aplikacji sieci web na produkcyjne (wraz z innymi nowych i zaktualizowanych zawartości).


Również zrozumienie wszelkich zmian do `Web.config` lub pliki w `Bin` directory zatrzymuje się i ponowne uruchomienie puli aplikacji witryny sieci Web. Jeśli Twoje stan sesji jest przechowywany przy użyciu `InProc` trybu (ustawienie domyślne), a następnie odwiedzających witryny spowoduje utratę ich stanu sesji, zawsze wtedy, gdy te pliki klucza są modyfikowane. Aby uniknąć tej niedogodności, należy rozważyć przechowywanie sesji przy użyciu `StateServer` lub `SQLServer` tryby. Aby uzyskać więcej informacji na ten temat, przeczytaj [tryby stanu sesji](https://msdn.microsoft.com/library/ms178586.aspx).

Na koniec należy pamiętać, że ponownego wdrażania aplikacji może potrwać od kilku sekund do kilku minut w zależności od liczby i rozmiaru plików, które mają zostać skopiowane do środowiska produkcyjnego. W tym czasie użytkowników odwiedzających witrynę, mogą wystąpić błędy lub nietypowego zachowania. Użytkownik może "turn off" całej aplikacji, dodając stronę o nazwie `App_Offline.htm` do katalogu głównego aplikacji, który wyjaśnia użytkownikom czy lokacja nie działa konserwacji (lub cokolwiek innego) i będzie można wykonać kopię zapasową wkrótce. Gdy `App_Offline.htm` pliku, środowisko uruchomieniowe ASP.NET przekierowuje wszystkie żądania przychodzące do tej strony.

## <a name="summary"></a>Podsumowanie

Wdrażanie aplikacji sieci web wymaga kopiowania plików na potrzeby ze środowiska projektowego w środowisku produkcyjnym. Najbardziej typowe oznacza, że za pomocą której pliki są przesyłane za pośrednictwem sieci jest protokół transferu plików (FTP), a większość dostawców usług hosta sieci web pomocy technicznej FTP dostęp do swoich serwerów sieci web. Jak wdrożyć wymagane pliki do serwera sieci web za pomocą klienta FTP widzieliśmy w ramach tego samouczka. Po wdrożeniu witryny sieci Web można kontrolowane przez każdy z połączeniem z Internetem!

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Aplikacja\_Offline.htm i Praca wokół funkcji "Przyjazne IE błędy"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Tryby stanu sesji](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Poprzednie](determining-what-files-need-to-be-deployed-cs.md)
> [dalej](deploying-your-site-using-visual-studio-cs.md)
