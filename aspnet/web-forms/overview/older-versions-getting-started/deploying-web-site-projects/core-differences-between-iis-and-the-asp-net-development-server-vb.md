---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Podstawowe różnice między usługami IIS a ASP.NET Development Server (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas testowania aplikacji ASP.NET aplikację lokalnie, prawdopodobnie używasz sieci Web ASP.NET Development Server. Jednak produkcji witryny sieci Web jest najprawdopodobniej pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 47b1959f9b92d161da0476b274c8154333ad80dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887551"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Podstawowe różnice między usługami IIS a ASP.NET Development Server (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Podczas testowania aplikacji ASP.NET aplikację lokalnie, prawdopodobnie używasz sieci Web ASP.NET Development Server. Jednak produkcji witryny sieci Web jest najprawdopodobniej zasilanego usług IIS. Istnieją pewne różnice między jak te serwery sieci web obsługuje żądania, a różnice te mogą mieć konsekwencje ważne. To w tym samouczku przedstawiono niektóre różnice więcej istotnego znaczenia.


## <a name="introduction"></a>Wprowadzenie

Zawsze, gdy użytkownik odwiedza aplikacji ASP.NET jego przeglądarki wysyła żądanie do witryny sieci Web. To żądanie jest pobrana przez oprogramowanie serwera sieci web, która koordynuje ze środowiskiem uruchomieniowym ASP.NET do generowania i zwrócić zawartość do żądanego zasobu. [**I** ezpośredniego **I** informacje o wersji **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) są zestawem usługi, które zapewniają często używane funkcje internetowych dla Serwery z systemem Windows. Program IIS jest serwerem sieci web najczęściej używane dla aplikacji ASP.NET w środowiskach produkcyjnych; jest to najprawdopodobniej oprogramowanie serwera sieci web, które są używane przez dostawcę hosta sieci web do obsługi aplikacji ASP.NET. Usługi IIS można również jako oprogramowanie serwera sieci web w środowisku programistycznym, chociaż to pociąga za sobą instalacji usług IIS i prawidłowego skonfigurowania go.


ASP.NET Development Server jest opcją serwera sieci web alternatywny dla środowiska programowania; jest dostarczany z, a jest zintegrowany z programu Visual Studio. Chyba, że aplikacja sieci web została skonfigurowana do używania serwera IIS, ASP.NET Development Server jest uruchomiona i automatycznie używany jako serwer sieci web podczas pierwszego odwiedzenia strony sieci web z poziomu programu Visual Studio. Aplikacje sieci web pokaz utworzyliśmy w [ *określania co pliki potrzebne do wdrożenia* ](determining-what-files-need-to-be-deployed-vb.md) samouczka zostały obydwu aplikacji sieci web opartych na systemie plików, które nie są skonfigurowane do używania serwera IIS. W związku z tym podczas odwiedzania jednej z tych witryn sieci Web z poziomu programu Visual Studio ASP.NET Development Server jest używany.

W idealnych środowisk projektowania i produkcji jest identyczne. Jednakże jak wspomniano w poprzednim samouczek nie jest nietypowe dla środowisk do różnych ustawień konfiguracji. Przy użyciu oprogramowania server innej witryny sieci web w środowiskach dodaje innej zmiennej, która musi być brana pod uwagę podczas wdrażania aplikacji. W tym samouczku przedstawiono podstawowe różnice między usługami IIS a ASP.NET Development Server. Z powodu różnic istnieją scenariusze, w którym kodu uruchamianego w środowisku programistycznym flawlessly zgłasza wyjątek lub zachowuje się inaczej, gdy wykonywana w środowisku produkcyjnym.

## <a name="security-context-differences"></a>Różnice w kontekście zabezpieczeń

Zawsze, gdy oprogramowanie serwera sieci web obsługuje żądanie przychodzące kojarzy z kontekstem zabezpieczeń tego żądania. Te informacje kontekstu zabezpieczeń jest używany przez system operacyjny, aby ustalić, jakie akcje są dopuszczalne przez żądanie. Na przykład strony ASP.NET mogą obejmować kod, który rejestruje niektórych komunikatów w pliku na dysku. Aby ta strona ASP.NET można wykonać bez błędów kontekst zabezpieczeń musi mieć odpowiednie uprawnienia na poziomie systemu plików, to znaczy zapis uprawnienia do tego pliku.

ASP.NET Development Server kojarzy przychodzące żądania w kontekście zabezpieczeń zalogowanego użytkownika. Jeśli użytkownik jest zalogowany pulpicie jako administrator, strony ASP.NET, obsługiwanych przez program ASP.NET Development Server mają te same prawa dostępu z uprawnieniami administratora. Jednak żądania ASP.NET obsługiwane przez usługi IIS są skojarzone z kontem określonej maszyny. Domyślnie konto komputera usługi sieciowej jest używany przez usługi IIS w wersji 6 i 7, mimo że dostawcy usługi hosta sieci web skonfigurowane konto unikatowy dla każdego klienta. Co więcej dostawcy usługi hosta sieci web, prawdopodobnie została podana ograniczone uprawnienia do tego konta komputera. Wynikiem jest, że masz stron sieci web, które zostanie wykonane bez błędów w środowisku programistycznym, ale Generowanie związanych z wyjątkami, podczas udostępniania w środowisku produkcyjnym.

Aby wyświetlić tego typu błędu w akcji utworzono stronę w witrynie sieci Web przeglądami książki, który tworzy plik na dysku, który przechowuje najbardziej aktualną datę i godzinę, ktoś wyświetlić *nauczyć się ASP.NET 3.5 w ciągu 24 godzin* Przejrzyj. Aby z niego skorzystać, otwórz `~/Tech/TYASP35.aspx` strony i Dodaj następujący kod do `Page_Load` obsługi zdarzeń:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> [ `File.WriteAllText` Metody](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) tworzy nowy plik, jeśli nie istnieje, a następnie zapisuje określoną zawartość do niego. Jeśli plik już istnieje, jej istniejąca zawartość zostanie zastąpiony.


Następnie odwiedź *nauczyć się ASP.NET 3.5 w ciągu 24 godzin* stronę przeglądu książki w środowisku programistycznym przy użyciu serwera projektowego ASP.NET. Przy założeniu, że użytkownik jest zalogowany do komputera przy użyciu konta, które ma odpowiednie uprawnienia do tworzenia i modyfikowania pliku tekstowego w sieci web katalogu głównego aplikacji przejrzyj książki pojawi się taka sama jak przed, ale zawsze, gdy strona jest odwiedzi daty i godziny oraz użytkownika  Adres IP jest przechowywany w `LastTYASP35Access.txt` pliku. Wskazać w przeglądarce tego pliku. powinien zostać wyświetlony komunikat podobny do przedstawionego na rysunku 1.


[![Plik tekstowy zawierający Data i godzina ostatniej przeglądu książki został odwiedzony&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Rysunek 1**: plik tekstowy zawierający Data i godzina ostatniej przeglądu książki został odwiedzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Wdrażanie aplikacji sieci web w środowisku produkcyjnym, a następnie odwiedź hostowanej *nauczyć się ASP.NET 3.5 w ciągu 24 godzin* książki stronę przeglądu. W tym momencie albo powinna zostać wyświetlona strona przeglądu książki jako normalny lub komunikat o błędzie pokazany na rysunku 2. Niektóre dostawców usług hosta sieci web Udziel uprawnień zapisu do anonimowego konta komputera platformy ASP.NET, w których przypadku strony będzie działać bez błędów. Jeśli jednak dostawcy usługi hosta sieci web nie zezwala na dostęp do zapisu dla anonimowego konta, a następnie [ `UnauthorizedAccessException` wyjątek](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) jest wywoływane, gdy `TYASP35.aspx` strona próbuje zapisać bieżącą datę i godzinę do `LastTYASP35Access.txt` pliku.


[![Domyślne konto komputera używany przez usługi IIS nie ma uprawnień do zapisu w systemie plików](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Rysunek 2**: domyślna maszyny konto używane przez usługi IIS jest nie ma uprawnień do zapisu w systemie plików ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


Dobre wieści jest, że większość dostawców usług hosta sieci web ma jakieś narzędzie uprawnienia, które można określić uprawnienia systemu plików w witrynie sieci Web. Udzielanie anonimowego dostępu konta ASP.NET zapisu do katalogu głównego i następnie ponowne uzyskiwanie dostępu do strony przeglądu. (Jeśli to konieczne, skontaktuj się z dostawcą hosta sieci web Pomocy dotyczące udzielania uprawnień zapisu do domyślnego konta ASP.NET.) Teraz strony powinny zostać załadowane bez błędów i `LastTYASP35Access.txt` plik powinien być pomyślnie utworzony.

Podejmij optymalizacji tutaj ponieważ ASP.NET Development Server działa w kontekście zabezpieczeń inną niż IIS, jest możliwe, że strony ASP.NET, które odczytu lub zapisu w systemie plików odczytu lub zapisu w dzienniku zdarzeń systemu Windows, lub do odczytu lub zapisu systemu Windows  rejestru będzie działać zgodnie z oczekiwaniami na projektowanie, ale generowania wyjątków, gdy w środowisku produkcyjnym. Podczas tworzenia aplikacji sieci web, który będzie wdrażany w udostępnionym środowisku hosta sieci web, nie do odczytu lub zapisu dziennika zdarzeń lub rejestru systemu Windows. Również pod uwagę stron ASP.NET, dla których odczytywanie i zapisywanie w systemie plików, ponieważ może być konieczne przyznanie odczytu i zapisu uprawnień na odpowiednie foldery w środowisku produkcyjnym.

## <a name="differences-on-serving-static-content"></a>Różnice w obsługę zawartości statycznej

Inna core różnica między usługami IIS a ASP.NET Development Server jest sposób obsługują żądania dotyczące zawartości statycznej. Każde żądanie, który wchodzi w ASP.NET Development Server dla strony platformy ASP.NET, obrazu lub plik JavaScript jest przetwarzany przez moduł wykonawczy platformy ASP.NET. Domyślnie usługi IIS tylko wywołuje środowiska uruchomieniowego ASP.NET, gdy nadejdzie żądanie zasobu ASP.NET, takich jak strony sieci web platformy ASP.NET, usług sieci Web i tak dalej. Żądania dotyczące zawartości statycznej - obrazów, plików CSS, pliki JavaScript, plików PDF, pliki ZIP i podobnych — są pobierane przez usługi IIS bez udziału środowiska uruchomieniowego ASP.NET. (Go można nakazać programowi IIS pracę ze środowiskiem uruchomieniowym ASP.NET przy wysyłaniu zawartości statycznej; zapoznaj się w sekcji "Uwierzytelnianie oparte na formularzach wykonywanie i adres URL uwierzytelniania na pliki statyczne z programem IIS 7" w tym samouczku, aby uzyskać więcej informacji.)

Środowisko uruchomieniowe programu ASP.NET wykonuje szereg kroki, aby wygenerować żądanej zawartości, w tym uwierzytelniania (identyfikowanie żądającego) i autoryzacji (określenie, czy obiekt żądający ma uprawnienia do wyświetlania żądanej zawartości). Popularne formy uwierzytelniania jest *uwierzytelniania opartego na formularzach*, w którym użytkownik jest identyfikowany przez wprowadzić swoje poświadczenia — zwykle nazwę użytkownika i hasło — do pól tekstowych na stronie sieci web. Podczas sprawdzania poprawności poświadczeń, witryna sieci Web przechowuje *biletu uwierzytelniania* plików cookie w przeglądarce użytkownika, który jest wysyłany z wszystkich kolejnych żądań do witryny sieci Web i jest używana do uwierzytelniania użytkownika. Ponadto istnieje możliwość określenia *Autoryzacja adresów URL* reguł, które jest wymagane przez co użytkownicy mogą lub nie można uzyskać dostępu do określonego folderu. Wiele witryn sieci Web ASP.NET, Użyj uwierzytelniania i autoryzacji adresów URL do obsługi kont użytkowników i określenie obszarów dostępnych tylko dla uwierzytelnionych użytkowników lub użytkowników, którzy należą do określonych ról.

> [!NOTE]
> Do zbadania programu ASP. Uwierzytelnianie oparte na formularzach w sieci, Autoryzacja adresów URL i inne funkcje z związanych z kontem użytkownika, należy koniecznie zapoznaj się z mojej [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Należy wziąć pod uwagę witryny sieci Web, który obsługuje kont użytkowników przy użyciu autoryzacji na podstawie formularzy i ma folder, który za pomocą Autoryzacja adresów URL jest skonfigurowane i umożliwiają tylko użytkownicy uwierzytelnieni. Załóżmy, że ten folder zawiera stron ASP.NET i plików PDF i że celem jest, że tylko użytkownicy uwierzytelnieni mogą wyświetlać pliki PDF.

Co się stanie, jeśli użytkownik próbuje wyświetlić jeden z tych plików PDF przy użyciu adresu URL bezpośrednio w pasku adresu przeglądarki jego? Aby dowiedzieć się, umożliwia Utwórz nowy folder w witrynie przeglądami książki, dodać niektóre pliki PDF i skonfigurować witrynę na potrzeby autoryzacji adresów URL zabrania użytkowników anonimowych odwiedzający tego folderu. Jeśli pobieranie aplikacji demonstracyjnej zobaczysz, który został utworzony folder o nazwie `PrivateDocs` i dodać PDF z mojej [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (jak dopasowane!). `PrivateDocs` Folder zawiera także `Web.config` pliku, który określa reguł autoryzacji adresów URL, aby odmówić anonimowych użytkowników:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Na koniec, po skonfigurowaniu aplikacji sieci web do używania uwierzytelniania opartego na formularzach, aktualizując plik Web.config w katalogu głównym, zastępując:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Z:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Za pomocą programu ASP.NET Development Server, odwiedź witrynę i wprowadź bezpośrednio adres URL do jednego z plików PDF na pasku adresu przeglądarki. Jeśli pobrano witryny sieci Web skojarzony z tym samouczku adres URL powinien wyglądać jak: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Wprowadzanie ten adres URL na pasku adresu powoduje, że przeglądarka Wyślij żądanie do pliku ASP.NET Development Server. Ręce ASP.NET Development Server poza środowiskiem uruchomieniowym ASP.NET w celu przetworzenia żądania. Ponieważ firma Microsoft nie jeszcze zalogował się i `Web.config` w `PrivateDocs` jest skonfigurowany do odmowy dostępu anonimowego, środowiska uruchomieniowego ASP.NET automatyczne przekierowanie nam na stronę logowania `Login.aspx` (patrz rysunek 3). Jeśli przekierowanie użytkownika do strony logowania, program ASP.NET zawiera `ReturnUrl` parametr querystring, która wskazuje stronę użytkownik próbował wyświetlić. Po pomyślnym zalogowaniu użytkownika może być zwracany do tej strony.


[![Nieautoryzowani użytkownicy są automatycznie przekierowany do strony logowania](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Rysunek 3**: nieautoryzowani użytkownicy są automatycznie przekierowany do strony logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Teraz zobaczmy, jak to działa w środowisku produkcyjnym. Wdrażanie aplikacji i wprowadź bezpośrednio adres URL do jednego z plików PDF w `PrivateDocs` folderu w środowisku produkcyjnym. Powoduje wyświetlenie monitu o przeglądarce wysyłanie żądań usług IIS dla pliku. Ponieważ zażądano plik statyczny IIS pobiera i zwraca plik bez wywoływania środowiska uruchomieniowego ASP.NET. W związku z tym nie było żadnych operacji; sprawdzania autoryzacji adresu URL Zawartość prawdopodobnie prywatnej PDF są dostępne dla wszystkich osób, który zna bezpośredni adres URL do pliku.


[![Użytkownicy anonimowi mogą pobierać pliki PDF prywatnego wprowadzając bezpośredni adres URL do pliku](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Rysunek 4**: anonimowe użytkownicy mogą pobrać prywatnej PDF plików przez wprowadzanie bezpośredni adres URL do pliku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Wykonywanie uwierzytelnianie oparte na formularzach i uwierzytelnianie adres URL na pliki statyczne z usług IIS 7

Istnieje kilka metod, które służy do ochrony zawartości statycznej przed nieautoryzowanymi użytkownikami. Wprowadzone w usługach IIS 7 *zintegrowanego potoku*, który posiadającego przepływu pracy przez usługi IIS w przepływie pracy środowiska uruchomieniowego ASP.NET. Krótko mówiąc, poinstruuj usług IIS do wywołania środowiska uruchomieniowego ASP.NET uwierzytelniania i autoryzacji modułów wszystkich żądań przychodzących (w tym zawartości statycznej, takich jak pliki PDF). Skontaktuj się z dostawcą hosta sieci web, aby dowiedzieć się, jak skonfigurować witryny sieci Web do użycia zintegrowanego potoku.

Po usług IIS został skonfigurowany do używania zintegrowanego potoku Dodaj następujący kod do `Web.config` pliku w katalogu głównym:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Ten kod znaczników nakazuje IIS 7 do użycia moduły uwierzytelniania i autoryzacji oparty na programie ASP.NET. Ponownie wdrożyć aplikację i ponownie można znaleźć w pliku PDF. Tym razem, gdy usługi IIS obsługuje żądanie udostępnia logikę uwierzytelniania i autoryzacji środowiska uruchomieniowego ASP.NET możliwość sprawdzić żądanie. Ponieważ tylko uwierzytelnieni użytkownicy mogą wyświetlać zawartość w `PrivateDocs` folderu, użytkownik anonimowy jest automatycznie przekierowywane do strony logowania (odwołują się do rysunku 3).

> [!NOTE]
> Dostawcy usługi hosta sieci web jest w dalszym ciągu używają programu IIS 6 nie można użyć funkcji zintegrowanego potoku. Jeden obejście jest umieszczenie prywatne dokumenty w folderze, który uniemożliwia dostęp HTTP (takie jak `App_Data`), a następnie utworzyć strony do obsługi tych dokumentów. Ta strona może zostać wywołana `GetPDF.aspx`, a jest przekazywana nazwy pliku PDF za pośrednictwem parametru querystring. `GetPDF.aspx` Czy najpierw sprawdź, czy użytkownik ma uprawnienia do wyświetlania zawartości pliku i jeśli tak, użyj [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) metody do odesłania do klienta zawartość żądany plik PDF. Ta technika również będzie działać dla usług IIS 7, jeśli nie ma włączyć zintegrowanego potoku.


## <a name="summary"></a>Podsumowanie

Aplikacje sieci Web w środowisku produkcyjnym znajdują się za pomocą oprogramowania serwera sieci web usług IIS do firmy Microsoft. W środowisku programistycznym jednak aplikacja może być obsługiwana za pomocą usług IIS lub ASP.NET Development Server. W idealnym przypadku tego samego oprogramowania serwera sieci web powinien można użyć w obu środowiskach, ponieważ za pomocą innego oprogramowania dodaje innej zmiennej w zestawie. Jednak łatwość użycia programu ASP.NET Development Server ułatwia on atrakcyjny rozwiązaniem w środowisku programistycznym. Dobre wieści jest tylko kilka podstawowych różnic między usługami IIS a ASP.NET Development Server, czy jeśli poznać różnice te można wykonać kroki w celu zapewnienia, że aplikacja działa i czy działa tak samo, jak niezależnie od tego, środowisko.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Integracja platformy ASP.NET z usług IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Korzystanie z uwierzytelniania forach platformy ASP.NET z wszystkich typów zawartości w usługach IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (klip wideo)
- [Serwery sieci Web w programie Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Poprzednie](common-configuration-differences-between-development-and-production-vb.md)
> [dalej](deploying-a-database-vb.md)
