---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Podstawowe różnice między usługami IIS a programem ASP.NET Development Server (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas testowania aplikacji programu ASP.NET w środowisku lokalnym, jest szansa, że używasz serwera sieci Web programu ASP.NET Development. Jednak w produkcyjnej witrynie internetowej jest najprawdopodobniej pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 19ca40374f97d59cac4f1677f886f3e48eab7b67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753725"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>Podstawowe różnice między usługami IIS a programem ASP.NET Development Server (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Podczas testowania aplikacji programu ASP.NET w środowisku lokalnym, jest szansa, że używasz serwera sieci Web programu ASP.NET Development. Jednak produkcyjnej witrynie internetowej jest najprawdopodobniej zasilania usług IIS. Istnieją pewne różnice między jak te serwery sieci web obsługi żądań, a te różnice mogą mieć konsekwencje ważne. W tym samouczku przedstawiono niektóre różnice, bardziej istotnego.


## <a name="introduction"></a>Wprowadzenie

Zawsze, gdy użytkownik odwiedzi aplikacji ASP.NET jego przeglądarce wysyła żądanie do witryny sieci Web. To żądanie jest pobierana przez oprogramowanie serwera sieci web, która koordynuje ze środowiskiem uruchomieniowym ASP.NET, aby wygenerować i zwrócić zawartość dla żądanego zasobu. [**I** ezpośredniego **I** informacje o wersji **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) to pakiet usług, które zapewniają typowych funkcji internetowych dla Serwery Windows. Program IIS jest serwer sieci web najczęściej używane dla aplikacji ASP.NET w środowisku produkcyjnym; jest to najprawdopodobniej oprogramowanie serwera sieci web, które są używane przez dostawcę hosta sieci web do obsługi aplikacji programu ASP.NET. Usługi IIS można również jako oprogramowanie serwera sieci web w środowisku deweloperskim, mimo że to pociąga za sobą Instalowanie usług IIS i odpowiednio konfigurując go.


ASP.NET Development Server jest opcją serwera sieci web alternatywny w środowisku deweloperskim; on dostarczany z programem i jest zintegrowana w programie Visual Studio. Chyba że aplikacja sieci web została skonfigurowana do używania usług IIS, ASP.NET Development Server jest uruchomiony i automatycznie używany jako serwer sieci web, odwiedź stronę sieci web z poziomu programu Visual Studio po raz pierwszy. Aplikacje sieci web pokaz utworzyliśmy w [ *określająca, które pliki muszą zostać wdrożone* ](determining-what-files-need-to-be-deployed-cs.md) samouczka zostały obie aplikacje sieci web opartych na systemie plików, które nie zostały skonfigurowane do używania usług IIS. W związku z tym gdy użytkownik odwiedzi jednej z tych witryn sieci Web z poziomu programu Visual Studio ASP.NET Development Server jest używany.

W idealnych środowisk deweloperskich i produkcyjnych może być taka sama. Jednakże tak jak Omówiliśmy to w poprzednim samouczku nie jest niczym niezwykłym środowiska do różnych ustawień konfiguracji. Przy użyciu oprogramowania server innej witryny sieci web w środowiskach dodaje innej zmiennej, które należy brać pod uwagę podczas wdrażania aplikacji. W tym samouczku opisano podstawowe różnice między usługami IIS a programem ASP.NET Development Server. Ze względu na różnice te są scenariusze, w którym kodu uruchamianego w środowisku programistycznym bezproblemowo zgłasza wyjątek lub zachowuje się inaczej, gdy wykonywane w środowisku produkcyjnym.

## <a name="security-context-differences"></a>Różnice w kontekście zabezpieczeń

Zawsze wtedy, gdy oprogramowanie serwera sieci web obsługuje przychodzące żądanie kojarzy tego żądania z kontekstu zabezpieczeń. Te informacje kontekstu zabezpieczeń jest używany przez system operacyjny do określenia działania, jakie są dozwolone w żądaniu. Na przykład strony ASP.NET może zawierać kod, który rejestruje komunikat w pliku na dysku. Aby ta strona ASP.NET, można wykonać bez błędów kontekstu zabezpieczeń musi mieć odpowiednie uprawnienia na poziomie systemu plików, a mianowicie zapis uprawnienia do tego pliku.

ASP.NET Development Server kojarzy przychodzące żądania w kontekście zabezpieczeń zalogowanego użytkownika. Jeśli użytkownik jest zalogowany komputerze jako administrator, strony ASP.NET obsługiwanych przez program ASP.NET Development Server mają te same prawa dostępu jako administrator. Jednak żądania programu ASP.NET, obsługiwane przez usługi IIS są skojarzone z kontem określonego komputera. Domyślnie konto Usługa sieciowa maszyny jest używany przez usługi IIS w wersji 6 i 7, mimo że dostawcą hosta sieci web skonfigurowane konto unikatowy dla każdego klienta. Co więcej Twój dostawca hosta sieci web prawdopodobnie dało ograniczonymi uprawnieniami, aby konto tego komputera. Wynikiem jest, że masz stron sieci web, które wykonane bez błędów w środowisku deweloperskim, ale generowanie wyjątków związanych z autoryzacją w przypadku hostowania w środowisku produkcyjnym.

Do wyświetlenia tego typu błędu w działaniu w przeglądach książki witryny sieci Web, która tworzy plik na dysku, który przechowuje, najbardziej aktualną datę i godzinę, ktoś utworzono strony wyświetli *uczyć się ASP.NET 3.5 w ciągu 24 godzin* przeglądu. Aby z niego skorzystać, otwórz `~/Tech/TYASP35.aspx` strony, a następnie dodaj następujący kod do `Page_Load` program obsługi zdarzeń:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> [ `File.WriteAllText` Metoda](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) tworzy nowy plik, jeśli nie istnieje, a następnie zapisuje określoną zawartość do niego. Jeśli plik już istnieje, jego istniejąca zawartość zostanie zastąpiony.


Następnie odwiedź *uczyć się ASP.NET 3.5 w ciągu 24 godzin* strony przeglądu książki w środowisku programistycznym, przy użyciu serwera projektowego ASP.NET. Przy założeniu, że zalogowano się do komputera przy użyciu konta, które ma odpowiednie uprawnienia do tworzenia i modyfikowania pliku tekstowego w sieci web katalogu głównego aplikacji przejrzyj książki pojawi się taka sama jak przed, ale każdorazowo, gdy strona jest odwiedzone daty i godziny oraz użytkownika  Adres IP jest przechowywany w `LastTYASP35Access.txt` pliku. Wskazać w przeglądarce do tego pliku; powinien zostać wyświetlony komunikat podobny do przedstawionego na rysunku 1.


[![Plik tekstowy zawiera Data i godzina ostatniej odwiedzono przeglądu książki](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Rysunek 1**: plik tekstowy zawiera Data i godzina ostatniej odwiedzono przeglądu książki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


Wdrażanie aplikacji sieci web w środowisku produkcyjnym, a następnie odwiedź hostowanej *uczyć się ASP.NET 3.5 w ciągu 24 godzin* strony przeglądu książki. W tym momencie w albo powinna zostać wyświetlona strona przeglądu książki, jako normalny lub komunikat o błędzie pokazano na rysunku 2. Niektórych dostawców usług hosta sieci web przyznać uprawnienia do zapisu do anonimowych ASP.NET konta komputera w którym przypadku strony będzie działać bez błędów. Jeśli jednak dostawcą hosta sieci web nie zezwala na dostęp do zapisu dla konta anonimowego, a następnie [ `UnauthorizedAccessException` wyjątek](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) jest wywoływane, gdy `TYASP35.aspx` strona próbuje zapisać bieżącą datę i czas `LastTYASP35Access.txt` pliku.


[![Domyślne konto komputera, używanego przez usługi IIS nie ma uprawnień do zapisu w systemie plików](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Rysunek 2**: domyślna maszyny konto używane przez usługi IIS jest nie mieć uprawnień do zapisu w systemie plików ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


Dobra wiadomość jest większość dostawców usług hosta sieci web jakieś narzędzie uprawnienia, które umożliwia określenie uprawnień systemu plików w witrynie sieci Web. Udzielanie anonimowego dostępu konta ASP.NET zapisu do katalogu głównego, a następnie ponownie na stronie przeglądu księgi. (Jeśli to konieczne, skontaktuj się z dostawcą sieci web hosta uzyskać pomoc na temat sposobu przyznawania uprawnień zapisu do domyślnego konta ASP.NET.) Tym razem strona, powinny zostać załadowane bez błędów i `LastTYASP35Access.txt` powinien on być utworzony pomyślnie.

Wypełnij natychmiast tutaj ponieważ ASP.NET Development Server działa w kontekście zabezpieczeń inną niż IIS, jest możliwe, że Twoje strony ASP.NET, które odczytu lub zapisu w systemie plików, odczytu z lub zapisu w dzienniku zdarzeń Windows, lub odczytu lub zapisu w Windows  rejestru będzie działać zgodnie z oczekiwaniami na temat opracowywania rozwiązań, ale generują wyjątki, gdy w środowisku produkcyjnym. Podczas tworzenia aplikacji sieci web, która zostanie wdrożona w udostępnionym środowisku hostingu w sieci web, nie odczytu lub zapisu w dzienniku zdarzeń lub rejestru Windows. Ponadto Zwróć uwagę na wszystkich stron ASP.NET, które odczytać lub zapisać do systemu plików, jak udzielić odczytu i zapisu uprawnień na odpowiednich folderów w środowisku produkcyjnym może być konieczne.

## <a name="differences-on-serving-static-content"></a>Różnice w obsługująca zawartość statyczną

Inny różnica core między usługami IIS a programem ASP.NET Development Server polega na tym, jak mogą obsługiwać żądania dotyczące zawartości statycznej. Każde żądanie, dostarczanego w ASP.NET Development Server dla strony ASP.NET, obrazu lub pliku JavaScript, są przetwarzane przez środowisko uruchomieniowe programu ASP.NET. Domyślnie usługi IIS tylko wywołuje środowiska uruchomieniowego programu ASP.NET, gdy nadejdzie żądanie dla zasobu platformy ASP.NET, takich jak strony sieci web ASP.NET, usługi sieci Web i tak dalej. Żądania dotyczące zawartości statycznej — obrazy, pliki CSS, plików JavaScript, plików PDF, plików ZIP i podobne — są pobierane przez usługi IIS bez udziału środowiska uruchomieniowego programu ASP.NET. (Go można nakazać usług IIS do pracy ze środowiskiem uruchomieniowym platformy ASP.NET, gdy obsługująca zawartość statyczną, zapoznaj się z sekcją "Przeprowadzanie uwierzytelniania opartego na formularzach i adres URL uwierzytelniania na pliki statyczne z usługami IIS 7" w ramach tego samouczka, aby uzyskać więcej informacji.)

Środowisko uruchomieniowe ASP.NET wykonuje kilka kroków, aby wygenerować żądanej zawartości, w tym uwierzytelniania (identyfikowania osoby zgłaszającej żądanie) i autoryzacji (określania, czy obiekt żądający ma uprawnienia do wyświetlania żądanej zawartości). Popularne formy uwierzytelniania jest *uwierzytelniania opartego na formularzach*, w której jest identyfikowany przez użytkownika, wprowadzając swoje poświadczenia — zwykle o nazwę użytkownika i hasło — do pól tekstowych na stronie sieci web. Podczas sprawdzania poprawności poświadczeń, są przechowywane w witrynie sieci Web *biletu uwierzytelniania* plików cookie w przeglądarce, co jest wysyłane każde kolejne żądanie do witryny internetowej i jest używany do uwierzytelniania użytkownika. Ponadto, istnieje możliwość określenia *Autoryzacja adresów URL* reguł, które określają, jakie użytkownicy mogą lub nie można uzyskać dostęp do określonego folderu. Wiele witryn w sieci Web platformy ASP.NET korzystania z uwierzytelniania opartego na formularzach i Autoryzacja adresów URL do obsługi kont użytkowników i w celu określenia części lokacji dostępnych tylko dla uwierzytelnionych użytkowników lub użytkowników, którzy należą do określonych ról.

> [!NOTE]
> Aby uzyskać dokładne badanie ASP. Uwierzytelnianie oparte na formularzach sieci firmy, Autoryzacja adresów URL i inne funkcje z związanych z kontem użytkownika, koniecznie zapoznaj się z moich [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Należy wziąć pod uwagę witryny sieci Web, która obsługuje konta użytkowników przy użyciu autoryzacji opartej na formularzach i zawiera folder, który za pomocą Autoryzacja adresów URL jest skonfigurowane i umożliwiają tylko uwierzytelnionym użytkownikom. Wyobraź sobie, że ten folder zawiera strony ASP.NET i pliki PDF i że celem jest, że tylko uwierzytelnieni użytkownicy mogą wyświetlać te pliki PDF.

Co się stanie, jeśli użytkownik próbuje wyświetlić jeden z tych plików PDF, wprowadzając adres URL bezpośrednio z paska adresu w swojej przeglądarce? Aby dowiedzieć się, Dodajmy Utwórz nowy folder w witrynie przeglądy książki, niektóre pliki PDF i skonfigurować witrynę na potrzeby autoryzacji adresów URL anonimowych użytkowników ze Stanów Zjednoczonych zabraniają odwiedzania tego folderu. Jeśli pobieranie aplikacji demonstracyjnej zobaczysz, że utworzono folder o nazwie `PrivateDocs` i dodać plik PDF z moich [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (jak dopasowane!). `PrivateDocs` Folder zawiera także `Web.config` pliku, który określa reguły autoryzacji adresów URL, aby uniemożliwić użytkownikom anonimowym:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Na koniec, po skonfigurowaniu aplikacji sieci web do korzystania z uwierzytelniania opartego na formularzach, aktualizując `Web.config` pliku w katalogu głównym, zastępując:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

Za pomocą:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

Przy użyciu serwera projektowego ASP.NET, odwiedź witrynę, a następnie wprowadź bezpośredni adres URL do jednego z plików PDF, na pasku adresu przeglądarki. Jeśli nie pobrano witrynę internetową skojarzoną z tym samouczkiem, w których adres URL powinien wyglądać następująco: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Wprowadź ten adres URL w pasku adresu powoduje, że przeglądarkę, aby wysłać żądanie do ASP.NET Development Server dla pliku. Sesje hands ASP.NET Development Server wniosek do środowiska uruchomieniowego programu ASP.NET do przetwarzania. Ponieważ firma Microsoft nie jeszcze logowali się i `Web.config` w `PrivateDocs` folderu skonfigurowano odmowę dostępu anonimowego, środowisko uruchomieniowe ASP.NET automatycznie przekierowuje nam do strony logowania `Login.aspx` (zobacz rysunek 3). Jeśli przekierowanie użytkownika do strony logowania, program ASP.NET zawiera `ReturnUrl` parametr querystring, która wskazuje stronę użytkownik próbował wyświetlić. Po pomyślnym zalogowaniu się użytkownika mogą być zwrócone do tej strony.


[![Nieautoryzowani użytkownicy są automatycznie przekierowywane do strony logowania](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Rysunek 3**: nieautoryzowani użytkownicy są automatycznie przekierowywane do strony logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


Teraz zobaczmy, jak to działa w środowisku produkcyjnym. Wdrażanie aplikacji, a następnie wprowadź bezpośredni adres URL do jednego z plików PDF w `PrivateDocs` folder w środowisku produkcyjnym. Wyświetla monit o przeglądarce wysyłanie żądań usług IIS dla pliku. Ponieważ pliku statycznego jest wymagane, usług IIS umożliwia pobranie i zwraca go bez wywoływania środowiska uruchomieniowego programu ASP.NET. W rezultacie wystąpił nie adres URL autoryzacji operacji sprawdzania; zawartość PDF funkcji rzekomo prywatne są dostępne dla każdego, kto zna bezpośredni adres URL do pliku.


[![Użytkownicy anonimowi mogą pobierać pliki PDF prywatnego przy użyciu bezpośredniego adresu URL do pliku](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Rysunek 4**: anonimowe użytkownicy mogą pobrać prywatnej PDF plików przez wprowadzanie bezpośredni adres URL do pliku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Wykonywanie uwierzytelniania opartego na formularzach i adres URL uwierzytelniania na pliki statyczne z usługami IIS 7

Istnieje kilka technik, których można użyć do ochrony zawartości statycznej przed nieautoryzowanymi użytkownikami. Wprowadzone w usługach IIS 7 *zintegrowanego potoku*, który posiadającego workflow usług IIS za pomocą przepływu pracy środowiska uruchomieniowego programu ASP.NET. Mówiąc możesz wydać polecenie usług IIS do wywołania środowiska uruchomieniowego programu ASP.NET, uwierzytelniania i autoryzacji modułów wszystkich żądań przychodzących (w tym zawartości statycznej, takich jak pliki PDF). Skontaktuj się z dostawcą hosta sieci web, aby dowiedzieć się, jak skonfigurować witryny sieci Web do użycia w zintegrowanym potoku.

Po skonfigurowaniu usług IIS, aby użyć zintegrowanego potoku Dodaj następujący kod do `Web.config` pliku w katalogu głównym:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Ten kod znaczników powoduje, że usługi IIS 7 używania modułów uwierzytelniania i autoryzacji opartych na programie ASP.NET. Ponownie wdróż aplikację i ponownie można znaleźć w pliku PDF. Tym razem, gdy usługi IIS obsługuje żądanie daje ona logiki uwierzytelniania i autoryzacji, środowisko uruchomieniowe ASP.NET możliwość Zbadaj żądanie. Ponieważ tylko uwierzytelnionym użytkownikom uprawnienia do wyświetlenia zawartości `PrivateDocs` folderu, anonimowe osoby odwiedzającej jest automatycznie przekierowywane do strony logowania (odnoszą się do rysunek 3).

> [!NOTE]
> Jeśli Twój dostawca hosta sieci web nadal używa usług IIS 6 nie można użyć funkcji zintegrowanego potoku. Obejście jeden polega na umieszczeniu Twoje prywatne dokumenty w folderze, który uniemożliwia dostęp HTTP (taki jak `App_Data`), a następnie utworzyć stronę, aby obsługiwać te dokumenty. Ta strona może być wywoływana `GetPDF.aspx`i jest przekazywana nazwa pliku PDF, za pomocą parametru querystring. `GetPDF.aspx` Strony może najpierw sprawdzić, czy użytkownik ma uprawnienia do wyświetlania pliku, a jeśli tak, użyj [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) metodę, aby wysłać zawartość żądany plik PDF do klienta wysyłającego żądanie. Ta technika również będzie działać dla usług IIS 7, jeśli nie ma włączone w zintegrowanym potoku.


## <a name="summary"></a>Podsumowanie

Aplikacje sieci Web w środowisku produkcyjnym są hostowane przy użyciu oprogramowania serwera sieci web usług IIS przez firmę Microsoft. W środowisku deweloperskim jednak aplikacja może być obsługiwana za pomocą usług IIS lub programem ASP.NET Development Server. W idealnym przypadku oprogramowanie tego samego serwera sieci web powinna używane w obu środowiskach, ponieważ używane oprogramowanie różni się dodaje innej zmiennej w zestawie. Jednakże łatwość użycia oprogramowania ASP.NET Development Server ułatwia on atrakcyjny rozwiązaniem w środowisku programistycznym. Dobra wiadomość jest, że istnieje tylko kilka podstawowych różnic między usługami IIS a programem ASP.NET Development Server, a jeśli masz świadomość tych różnic możesz wykonać kroki, aby mieć pewność, że aplikacja działa i działa tak samo, sposób niezależnie od tego, środowisko.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Integracja platformy ASP.NET w usługach IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Korzystanie z uwierzytelniania fora platformy ASP.NET z wszystkich typów zawartości w usługach IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (wideo)
- [Serwery sieci Web w Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Poprzednie](common-configuration-differences-between-development-and-production-cs.md)
> [dalej](deploying-a-database-cs.md)
