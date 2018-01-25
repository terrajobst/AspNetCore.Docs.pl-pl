---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: "Wdrażanie witryny przy użyciu klienta FTP (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Najprostszym sposobem, aby wdrożyć aplikację ASP.NET jest ręcznie skopiuj niezbędne pliki środowiska programowania do środowiska produkcyjnego. Thi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 862f07defafb2d2613fef9f76f13aab0b11c5440
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Wdrażanie witryny przy użyciu klienta FTP (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Najprostszym sposobem, aby wdrożyć aplikację ASP.NET jest ręcznie skopiuj niezbędne pliki środowiska programowania do środowiska produkcyjnego. Ten samouczek przedstawia sposób użycia klient FTP można pobrać plików z pulpitu do dostawcy hosta sieci web.


## <a name="introduction"></a>Wprowadzenie

Samouczek poprzedniej wprowadzone proste aplikacja sieci web ASP.NET przeglądu książki, która składa się z kilku stron ASP.NET, strony głównej, niestandardowe baza `Page` klasy liczbę obrazów, i arkusze stylów CSS trzy. Firma Microsoft są teraz gotowe do wdrożenia tej aplikacji sieci web dostawcy hosta, w którym aplikacja będzie dostępne dla wszystkich użytkowników z połączeniem z Internetem!


Z naszych rozmów w [ *określania co pliki potrzebne do wdrożenia* ](determining-what-files-need-to-be-deployed-vb.md) samouczka wiemy, że pliki potrzebne do skopiowania do dostawcy hosta sieci web. (Czy które pliki są kopiowane zależy od tego, czy aplikacja jest jawnie lub automatycznie skompilowany odwołania). Jednak jak możemy uzyskać pliki ze środowiska projektowego (naszym pulpitu) do środowiska produkcyjnego (zarządzanych przez dostawcę sieci web hosta serwera sieci web)? [ **F** profil **T** przesyłania **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) jest protokołem często używane do kopiowania plików z jednego komputera na inny za pośrednictwem sieci. Innym rozwiązaniem jest rozszerzeń serwera FrontPage (FPSE). Ten samouczek koncentruje się przy użyciu autonomicznej oprogramowanie klienckie FTP do wdrażania niezbędne pliki środowiska programowania do środowiska produkcyjnego.

> [!NOTE]
> Visual Studio zawiera narzędzia do publikowania witryny sieci Web za pomocą protokołu FTP; te narzędzia, a także przeglądać narzędzia, które używają FPSE, znajdują się w następnym samouczku.


Aby skopiować pliki przy użyciu protokołu FTP, potrzebujemy *klient FTP* w środowisku programistycznym. Klient FTP jest aplikacja, która służy do kopiowania plików na komputerze jest zainstalowany na komputerze, na którym uruchomiono *serwera FTP*. (Jeśli dostawcy usługi hosta sieci web obsługuje transferowania plików przy użyciu protokołu FTP, tak jak większość, następnie jest uruchomiony na serwerach sieci web serwera FTP.) Liczba aplikacje klienckie FTP są dostępne. Przeglądarki sieci web można nawet double jako klient FTP. Moje ulubione klient FTP i jeden I będzie używana w tym samouczku jest [FileZilla](http://filezilla-project.org/), klient FTP bezpłatne, open source, który jest dostępna dla systemu Windows, Linux i komputerów Mac. Dowolnego klienta FTP będzie działać, jednak może używać dowolnego klienta potrafisz najbardziej tak działania.

Jeśli wykonujesz wzdłuż można będzie konieczne tworzenia konta u dostawcy hosta sieci web przed można wykonać w tym samouczku lub kolejne. Jak wspomniano w poprzedniej samouczek, istnieją gaggle firm dostawcy hosta sieci web z szerokiego zakresu cen, funkcje i jakości usług. Ten samouczek serii I użyje [Discount ASP.NET](http://discountasp.net) jako Mój hosta sieci web dostawcy, ale można skorzystać z każdego dostawcy hosta sieci web tak długo, jak obsługują wersji programu ASP.NET w witrynie został napisany w. (Te samouczki zostały utworzone za pomocą programu ASP.NET 3.5.) Ponieważ firma Microsoft kopiowania plików do dostawcy hosta sieci web za pomocą protokołu FTP w tym samouczku i w przyszłości tych jest także konieczne, że dostawca hosta sieci web obsługuje FTP dostęp do swoich serwerów sieci web. Ta funkcja oferuje praktycznie wszystkich dostawców usług hosta sieci web, ale należy zapoznać się przed utworzeniem.

## <a name="deploying-the-book-review-web-application-project"></a>Wdrażanie projektu aplikacji sieci Web Przejrzyj książki

Odwołaj, że istnieją dwie wersje aplikacji sieci web Przejrzyj książki: jeden implementowane przy użyciu modelu projektu aplikacji sieci Web (BookReviewsWAP) i innych za pomocą modelu projekt witryny sieci Web (BookReviewsWSP). Typ projektu ma wpływ, czy ma być kompilowana lokacji automatycznie lub jawnie i modelu kompilacji decyduje, co pliki potrzebne do wdrożenia. W rezultacie zostaną omówione wdrażania projektów BookReviewsWAP i BookReviewsWSP oddzielnie, począwszy od BookReviewsWAP. Poświęć chwilę, aby pobrać te dwie aplikacje ASP.NET, jeśli nie zostało to jeszcze zrobione.

Uruchom projekt BookReviewsWAP, przechodząc do `BookReviewsWAP` folder i dwukrotnie `BookReviewsWAP.sln` pliku. Przed wdrożeniem projekt należy do kompilacji, aby upewnić się, że wszystkie zmiany do kodu źródłowego są uwzględniane w skompilowanym zestawie. Aby skompilować projekt przejdź do menu Kompiluj i wybierz opcję menu BookReviewsWAP kompilacji. To kompiluje kod źródłowy w projekcie w ramach jednego zestawu `BookReviewsWAP.dll`, który znajduje się w `Bin` folderu.

Firma Microsoft są teraz gotowe do wdrożenia wymaganych plików! Uruchom klienta FTP i połączyć się z serwerem sieci web u dostawcy usługi hosta sieci web. (Po utworzeniu konta z hostingu firmy będą one e-mail informacji na temat łączenia się z serwerem FTP; dotyczy to również adres serwera FTP oraz nazwę użytkownika i hasło).

Skopiuj następujące pliki z pulpitu w folderze głównym witryny sieci Web u dostawcy usługi hosta sieci web. Jeśli FTP do serwera sieci web w sieci web dostawcy najprawdopodobniej w katalogu głównym witryny sieci Web. Jednak niektóre dostawców usług hosta sieci web ma podfolder o nazwie `www` lub `wwwroot` służy jako folder główny dla plików witryny sieci Web. Na koniec po FTPing plików może być konieczne utworzenie odpowiednich struktury folderów w środowisku produkcyjnym — `Bin` folderu, `Fiction` folderu `Images` folder i tak dalej.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Pełna zawartość `Styles` folderu
- Pełna zawartość `Images` folder (i jego podfolderów `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Rysunek 1 pokazuje FileZilla po skopiowaniu potrzebne pliki. FileZilla Wyświetla pliki na komputerze lokalnym po lewej i plików na komputerze zdalnym po prawej stronie. Jak przedstawiono na rysunku 1, pliki kodu źródłowego ASP.NET, takich jak `About.aspx.vb`, znajdują się na komputerze lokalnym (środowisko rozwoju), ale nie zostały skopiowane do dostawcy hosta sieci web (środowisko produkcyjne), ponieważ nie trzeba można wdrożyć przy użyciu plików kodu kompilację typu Explicit.

> [!NOTE]
> Jak są ignorowane nie powoduje żadnych problemów w mających plików kodu źródłowego na serwerze produkcyjnym. Domyślnie program ASP.NET jest zabronione żądania HTTP do plików kodu źródłowego, tak, że nawet jeśli pliki kodu źródłowego znajdują się na serwerze produkcyjnym są niedostępne dla odwiedzający witrynę sieci Web. (To znaczy, jeśli użytkownik próbuje znaleźć `http://www.yoursite.com/Default.aspx.vb` otrzymuje strony błędu, który objaśnia, w którym tych typów plików - `.vb` plików — jest zabronione.)


[![Klient FTP umożliwia skopiuj niezbędne pliki z pulpitu na serwerze sieci Web u dostawcy hosta sieci Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Rysunek 1**: używany klient FTP do skopiowania Your Desktop pliki niezbędne do serwera sieci Web u dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Po wdrożeniu witryny Poświęć chwilę, aby przetestować lokacji. Jeśli zakupione nazwę domeny i ustawienia DNS skonfigurowane poprawnie, można odwiedzić witrynę, wprowadzając nazwę domeny. Alternatywnie dostawcy usługi hosta sieci web powinien podano możesz przy użyciu adresu URL do witryny, które będą wyglądać podobnie jak *accountname*. *webhostprovider*.com lub *webhostprovider*.com /*accountname*. Na przykład adres URL konta na platformie ASP.NET z rabatem jest: `http://httpruntime.web703.discountasp.net`.

Rysunek 2 przedstawia wdrożonej witrynie przeglądami książki. Należy pamiętać, że wyświetlanej go na Discount ASP. Serwery w sieci, w `http://httpruntime.web703.discountasp.net`. W tym momencie każdy z połączeniem z Internetem można wyświetlać witryny sieci Web! Jak możemy oczekiwać lokacji wyglądu i zachowania tak samo jak podczas testowania go w środowisku programistycznym.

> [!NOTE]
> Jeśli wystąpi błąd podczas wyświetlania aplikacji Poświęć chwilę, aby upewnić się, że wdrożono poprawny zestaw plików. Następnie sprawdź komunikat o błędzie, aby zobaczyć, jeśli jego ujawnia żadnych wskazówek, aby ten problem. Następujące, można włączyć do działu pomocy technicznej firmy hosta sieci web lub opublikuj pytanie na forum odpowiednie w [fora ASP.NET](https://forums.asp.net/).


[![Witryna przeglądami książki jest teraz dostępna dla wszystkich użytkowników z połączeniem internetowym.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Rysunek 2**: lokacji przeglądami książki jest teraz dostępny dla wszystkich użytkowników z połączeniem internetowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Wdrażanie projektu witryny sieci Web Przejrzyj książki

Wdrażanie aplikacji ASP.NET, która używa automatycznego kompilacji, takie jak BookReviewsWSP projekt witryny sieci Web, nie ma żadnych skompilowanego zestawu w `Bin` folderu. W związku z tym plików kodu źródłowego aplikacji sieci web należy wdrożyć w środowisku produkcyjnym. Przejdźmy tego procesu.

Zgodnie z projektu aplikacji sieci Web dobrze jest pierwszym kompilacji aplikacji przed jego wdrożeniem. Podczas kompilowania projektu witryny sieci Web nie powoduje utworzenia zestawu, sprawdź na stronie błędy kompilacji. Lepiej Znajdź teraz te błędy, a nie o obiekt odwiedzający do swojej witryny wykrywać je dla Ciebie!

Po został pomyślnie utworzony projekt, użyj klienta FTP do skopiuj następujące pliki do folderu głównego witryny sieci Web u dostawcy usługi hosta sieci web. Może być konieczne utworzenie odpowiednich struktury folderów w środowisku produkcyjnym.

> [!NOTE]
> Jeśli BookReviewsWAP projektu ale nadal chcesz wypróbować wdrażania projektu BookReviewsWSP już wdrożone, należy najpierw usunąć wszystkie pliki na serwerze sieci web, które zostały przekazane podczas wdrażania BookReviewsWAP, a następnie wdrożyć pliki dla BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Pełna zawartość `Styles` folderu
- Pełna zawartość `Images` folder (i jego podfolderów `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

Rysunek 3 przedstawia FileZilla po skopiowaniu zapasowej wymaganych plików. Jak widać, ASP.NET kodu plików źródłowych, takich jak `About.aspx.vb`, znajdują się na komputerze lokalnym (środowisko rozwoju) i dostawcy hosta sieci web (środowisko produkcyjne), ponieważ pliki kodu muszą zostać wdrożone w przypadku korzystania z automatycznego kompilacji.


[![Używany klient FTP, aby skopiować niezbędne pliki z pulpitu na serwerze sieci Web u dostawcy hosta sieci Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Rysunek 3**: używany klient FTP do skopiowania Your Desktop pliki niezbędne do serwera sieci Web u dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


Środowisko użytkownika nie ma wpływu na modelu kompilacji aplikacji. Tej samej strony ASP.NET są dostępne i wyglądają i działają tak samo, czy witryna sieci Web została utworzona za pomocą modelu projektu aplikacji sieci Web lub modelu projekt witryny sieci Web.

## <a name="updating-a-web-application-on-production"></a>Aktualizowanie aplikacji sieci Web w środowisku produkcyjnym

Tworzenie aplikacji sieci Web i wdrożenia nie są jednorazowego procesu. Na przykład podczas tworzenia witryny sieci Web Przejrzyj książki I wbudowane różne strony i zapisano towarzyszący kodu na tym komputerze osobiste (środowisko rozwoju). Po osiągnięciu niektórych stabilności, wdrożono aplikację tak, aby inne osoby można odwiedzić witrynę i Moje czytamy. Ale wdrożenia nie oznacza koniec Moje Programowanie w tej witrynie. I dodać więcej recenzji książki lub implementowania nowych funkcji, na przykład pozwala Moje odwiedzających szybkość książki lub pozostaw własne komentarze. Takie rozszerzenia mogłyby opracowany na środowisko deweloperskie i po zakończeniu potrzebny do wdrożenia. Opracowywania i wdrażania, dlatego są cykliczne. Możesz utworzyć aplikację, a następnie wdrożyć go. Gdy lokacji znajduje się na żywo w środowisku produkcyjnym, zostaną dodane nowe funkcje i usunięto usterek wraz z upływem czasu, która wymaga ponownego wdrażania aplikacji. I tak dalej i tak dalej.

Jak może oczekujesz, podczas ponownego wdrażania aplikacji sieci web, należy skopiować nowe i zmienione pliki. Nie istnieje potrzeba ponownego wdrażania niezmienione strony lub pliki obsługi serwera lub klienta (ale nie powoduje żadnych problemów w ten sposób).

> [!NOTE]
> Jedyną operacją, należy wziąć pod uwagę, gdy przy użyciu kompilację typu explicit jest, że w dowolnym momencie dodać nową stronę ASP.NET do projektu lub wprowadzić zmiany dotyczące kodu, trzeba ponownie skompiluj projekt, który aktualizuje zestawu w `Bin` folderu. W związku z tym należy skopiować ten zaktualizowany zestaw do produkcji, podczas aktualizacji aplikacji sieci web w środowisku produkcyjnym (wraz z innych nowych i zaktualizowanych zawartości).


Należy również zapoznać się wszelkich zmian do `Web.config` lub pliki w `Bin` directory zatrzymuje i uruchamia ponownie puli aplikacji witryny sieci Web. Jeśli nazwa stanu sesji jest przechowywany przy użyciu `InProc` trybu (ustawienie domyślne), a następnie odwiedzających witryny sieci spowoduje utratę ich stanu sesji modyfikacjach te pliki klucza. Aby uniknąć tej niedogodności, należy wziąć pod uwagę przechowywanie sesji przy użyciu `StateServer` lub `SQLServer` trybów. Aby uzyskać więcej informacji na ten temat odczytu [tryby stanu sesji](https://msdn.microsoft.com/library/ms178586.aspx).

Na koniec należy pamiętać, że ponownego wdrażania aplikacji może potrwać od kilku sekund do kilku minut w zależności od liczby i rozmiaru plików, które mają zostać skopiowane do środowiska produkcyjnego. W tym czasie użytkowników odwiedzających witrynę, mogą wystąpić błędy lub nietypowego zachowania. Można "wyłączyć" całej aplikacji przez dodanie stronę o nazwie `App_Offline.htm` do katalogu głównego aplikacji, który objaśnia, aby użytkownicy czy lokacja nie działa z powodu konserwacji (lub niezależnie od) i będzie można wykonać kopię zapasową wkrótce. Gdy `App_Offline.htm` plik jest obecny, środowiska uruchomieniowego ASP.NET przekierowuje wszystkie żądania przychodzące do tej strony.

## <a name="summary"></a>Podsumowanie

Wdrażanie aplikacji sieci web wymaga kopiowanie niezbędne pliki środowiska programowania do środowiska produkcyjnego. Oznacza najczęściej używane, za pomocą której pliki są przesyłane przez sieć jest protokół transferu plików (FTP), a większość dostawców usług hosta sieci web obsługuje FTP dostęp do swoich serwerów sieci web. W tym samouczku widzieliśmy wdrożenia wymaganych plików do serwera sieci web przy użyciu klienta FTP. Po wdrożeniu witryny sieci Web można odwiedzić wszystkim osobom z połączeniem z Internetem!

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Aplikacja\_Offline.htm i obchodzić funkcji "Przyjazny dla programu Internet Explorer błędy"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Tryb stanu sesji](https://msdn.microsoft.com/library/ms178586.aspx)

>[!div class="step-by-step"]
[Poprzednie](determining-what-files-need-to-be-deployed-vb.md)
[dalej](deploying-your-site-using-visual-studio-vb.md)
