---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Ulepszenia w programie Visual Studio 2005 | Dokumentacja firmy Microsoft
author: microsoft
description: "Visual Studio 2005 oferuje deweloperom długą listę i ulepszeń do projektów sieci Web w aplikacji sieci Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 2c1f9a7291d8eab675bac3e1c37d6922131e3761
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="improvements-in-visual-studio-2005"></a>Ulepszenia w programie Visual Studio 2005
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Visual Studio 2005 oferuje deweloperom długą listę i ulepszeń do projektów sieci Web w aplikacji sieci Web.


Visual Studio 2005 oferuje deweloperom długą listę i ulepszeń do projektów sieci Web w aplikacji sieci Web. Wydajne jak Visual Studio .NET 2002 i 2003, znaleziono wiele utrudnień w taki sposób, że projekty sieci Web zostały obsłużone. Visual Studio 2005 dodaje wprowadzono wiele nowych funkcji w celu rozwiązania tych utrudnień. Dla tych, którzy preferowany sposób obsługi Kompilacja aplikacji sieci Web programu Visual Studio .NET 2003, zobacz [projekty aplikacji sieci Web](https://go.microsoft.com/fwlink/?LinkId=57870).

W tym module również obejmować ulepszenia tworzenia projektu sieci Web, zarządzania i programowania. W module nowsze również obejmować poprawę tworzenia projektów sieci Web i ich wdrażania.

## <a name="frontpage-server-extensions"></a>Rozszerzenia serwera FrontPage

Visual Studio .NET 2002 i 2003 wymagane rozszerzenia serwera FrontPage pole, aby można było utworzyć lub kompilacji projekty sieci Web. Deweloperzy miał wybór między dwa tryby dostępu do innego (tryb dostępu rozszerzenia serwera FrontPage lub pliku), rozszerzenia serwera FrontPage są używane do wykonywania zadań, takich jak ustawienie katalog główny aplikacji w usługach IIS, itp.

Visual Studio 2005 usuwa zależność od rozszerzeń serwera FrontPage dla projektów lokalnego. Teraz program Visual Studio 2005 uzyskuje dostęp do metabazy usług IIS, zamiast bezpośrednio za pomocą rozszerzeń serwera FrontPage. Visual Studio 2005 również dodaje obsługę FTP, dzięki czemu dostępu zdalnego projektu bez konieczności rozszerzeń serwera FrontPage.

Dla deweloperów, które chcą korzystać z rozszerzeń serwera FrontPage w swoich projektach ta opcja jest nadal dostępny. Jednak oparte na silne opinii społeczności deweloperów platformy ASP.NET, nie jest wymagane.

> [!NOTE]
> Rozszerzenia serwera FrontPage są nadal wymagane dla projektu zdalnego tworzenia, otwierania itp.


## <a name="aspnet-development-server"></a>ASP.NET Development Server

Visual Studio 2005 jest dostarczany z serwerem sieci Web o nazwie ASP.NET Development Server. (Ten serwer sieci Web była wcześniej znana jako Cassini).

Istnieje kilka zalet ASP.NET Development Server.

- Teraz istnieje możliwość dla użytkowników niebędących administratorami do opracowywania i debugowania dla serwera sieci Web.
- ASP.NET Development Server dynamicznie mapuje katalogów wirtualnych znajdujących się w dowolnej lokalizacji w systemie plików, co pozwala na elastyczne projektu lokalizacji.
- Użytkowników, którzy już używają programu IIS w systemie Windows XP Professional będzie teraz można utworzyć nowej aplikacji sieci Web, które nie mają wpływ na strukturze plik lub folder z ich domyślna witryna sieci Web w usługach IIS.

Aby móc korzystać z ASP.NET Development Server jest wymagana żadna konfiguracja specjalnych. Podczas debugowania projektu sieci Web, która jest obsługiwana w systemie plików lub przeglądać, Visual Studio 2005 zostaną automatycznie uruchomione wystąpienie ASP.NET Development Server losowy port do obsługi żądania.

Więcej informacji zostały omówione w ASP.NET Development Server później w tym module.

## <a name="improved-file-management"></a>Zarządzanie plikami ulepszone

W programie Visual Studio 2002 i 2003 pliku projektu (vbproj dla VB.NET) i .csproj w języku C# przechowywane informacje we wszystkich plikach w aplikacji sieci Web. Wyświetlanie w Eksploratorze rozwiązań jest oparty na informacje o pliku w pliku projektu. W związku z tym Eksploratora rozwiązań często umożliwią wyświetlanie nieprawidłowych informacji w przypadkach, w którym użyto edytory zewnętrzne. Visual Studio 2002 i 2003 będzie często Zastąp zmiany pliku lub wyświetla najnowszą wersję plików.

Visual Studio 2005 jest niedostępny z pliku projektu. Zamiast tego odczytuje informacje plików i folderów bezpośrednio z dysku, co powoduje dokładnego wyświetlania plików w projekcie. Ponieważ folder odwołań w Visual Studio 2002 i 2003 nie stanowią rzeczywistej folderu w aplikacji sieci Web, programu Visual Studio 2005 spowoduje również usunięcie folderu odwołania w Eksploratorze rozwiązań. Aby uzyskać dostęp do odwołań projektu programu Visual Studio 2005, należy użyć strony właściwości dla projektu.

## <a name="creating-web-projects"></a>Tworzenie projektów sieci Web

Deweloperzy sieci Web oferują wiele opcji nowe dostępne na potrzeby tworzenia projektu programu Visual Studio 2005. Witryny sieci Web teraz można tworzyć dowolne miejsce w systemie plików i następnie może być debugowany lub przeglądać przy użyciu nowego ASP.NET Development Server. Deweloperzy mogą również tworzyć nowych witryn sieci Web za pomocą protokołu FTP.

Kliknij tutaj, aby wyświetlić Przewodnik wideo dotyczący tworzenia projektów sieci Web programu Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Otwórz wideo na pełnym ekranie](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Projekty systemu plików

Jak przedstawiono w przewodnik wideo, można tworzyć witryny sieci Web w systemie plików na komputerze lokalnym lub w zdalnej lokalizacji za pośrednictwem udziału plików. Witryn sieci Web, które są tworzone w systemie plików jest przeglądany i debugować za pomocą programu ASP.NET Development Server.

> [!NOTE]
> ASP.NET Development Server mogą powodować dezorientację dla klientów. Jeśli projekt sieci Web jest tworzona w systemie plików w strukturze katalogów IISs (tj. c:\inetpub\wwwroot), nadal można przeglądać witryny sieci Web za pomocą programu ASP.NET Development Server, gdy jest uruchamiany z poziomu programu Visual Studio 2005. W związku z tym konfigurowania usług IIS (tj. metody uwierzytelniania) nie ma zastosowania.


Domyślny projekt sieci web spowoduje również usunięcie znacznie narzutów przez zawiera tylko stronę Default.aspx, default.cs plików i aplikacji\_folderem danych. Plik web.config i foldery specjalne (np. aplikacji\_kodu) są dodawane, gdy są potrzebne. Projekt sieci web zawiera tylko pliki i foldery, które są potrzebne.

### <a name="http-projects"></a>Projekty HTTP

Projekty HTTP może być projektów, które są tworzone w lokalnej witrynie sieci Web usług IIS lub zdalnej witryny sieci Web. Domyślna lokalizacja projektu `http://localhost`. Jeśli klikniesz przycisk Przeglądaj, dostępne są dwie opcje HTTP: IIS lokalnej i zdalnej witrynie. Główną różnicą w tych dwóch opcji to metoda, w którym informacji o witrynie sieci web jest wyświetlany w oknie dialogowym Wybierz lokalizację, jak i w jaki sposób pliki są kopiowane na serwer sieci Web.

Opcja lokalne usługi IIS odczytuje informacje o lokacji w metabazie na komputerze lokalnym i pliki są kopiowane przy użyciu systemu plików. Opcja lokacji zdalnej używa rozszerzeń serwera FrontPage i informacje o lokacji i pliki są kopiowane przy użyciu protokołu HTTP i wywołania RPC rozszerzenia serwera FrontPage.

> [!NOTE]
> Vs ###\_tmp.htm plików i get\_aspx\_ver.aspx nie są używane do określania informacji o wersji.


Domyślna opcja HTTP jest lokalne usługi IIS. Ta opcja odczytuje metabazy usług IIS, aby określić, które witryny są dostępne i lokalizację, w której do tworzenia zawartości. Można wybrać inny folder lub katalogu wirtualnego, wybierając go w widoku drzewa. Użytkownik może również Utwórz nowy katalog wirtualny, oznaczyć foldery jako aplikacje, a także usunąć istniejące katalogi wirtualne w tym oknie dialogowym.


![Wybierz lokalizację w oknie dialogowym](improvements-in-visual-studio-2005/_static/image1.gif)

**Rysunek 1**: Wybierz lokalizację w oknie dialogowym


W odróżnieniu od we wcześniejszych wersjach programu Visual Studio, jeśli zaznaczysz **Użyj protokołu Secure Sockets Layer** wyboru i certyfikat SSL nie jest zgodna adres URL przeglądania, zostanie wyświetlone okno dialogowe Alert zabezpieczeń pytaniem, jeśli będzie Czy chcesz kontynuować. Jeśli certyfikat nie jest zgodną jeden, za pomocą programu Visual Studio .NET 2003, tworzenie projektu nie powiedzie się.


![Certyfikat SSL dotyczące alertów zabezpieczeń](improvements-in-visual-studio-2005/_static/image2.gif)

**Rysunek 2**: Alert zabezpieczeń dotyczące certyfikatu SSL


### <a name="note-on-host-headers"></a>Należy pamiętać o nagłówkach hostów

W przypadku tworzenia aplikacji sieci Web w lokacji powiązane z określonego adresu IP, należy upewnić się, że skonfigurowano nagłówek hosta. W przeciwnym razie program Visual Studio utworzy lokacji na `http://localhost`, ale adres IP nie rozpozna poprawnie podczas przeglądania w witrynie lub debugowany od w środowisku IDE.

Jeśli wybrano opcję lokacji zdalnej, okno dialogowe zmiany umożliwiają wprowadzanie docelowy adres URL dla nowej witryny sieci Web. Ten adres URL musi być na serwerze, który ma rozszerzenia serwera FrontPage, które są włączone. Jeśli chcesz pracować z lokalnym serwerem sieci Web używa rozszerzeń serwera FrontPage, można użyć opcji lokacji zdalnej i określić lokalny adres URL.


![Tworzenie witryny sieci Web na serwerze zdalnym](improvements-in-visual-studio-2005/_static/image1.jpg)

**Rysunek 3**: tworzenie witryny sieci Web na serwerze zdalnym


Podczas tworzenia aplikacji w witrynie zdalnej za pośrednictwem protokołu SSL, jeśli certyfikat SSL nie jest zgodny, okno dialogowe potwierdzenia są nieco inne niż okno dialogowe wyświetlane przy użyciu opcji lokalnego usług IIS.


![Alert zabezpieczeń lokacji zdalnej](improvements-in-visual-studio-2005/_static/image3.gif)

**Rysunek 4**: Alert zabezpieczeń lokacji zdalnej


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 wprowadza opcję tworzenia witryn sieci Web za pośrednictwem protokołu FTP. Tej opcji IDE lokalnie tworzy pliki w folderze tymczasowym użytkowników, a następnie używa FTP do przenoszenia plików do lokalizacji FTP.

> [!NOTE]
> Lokalizacja folderu tymczasowego znajduje się c:\Documents and Settings\&lt; Użytkownik&gt;\Local Settings\Temp\VWDWebCache\&lt; Serwer&gt;\_&lt;Nazwa aplikacji&gt;


Korzystając z opcji FTP, zostanie wyświetlone okno dialogowe Wybieranie lokalizacji. Wprowadzeniu wymaganych informacji o połączeniu FTP do tego okna dialogowego, jak pokazano poniżej.


![Wybierz okno dialogowe lokalizacji FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Rysunek 5**: Wybierz okno dialogowe lokalizacji FTP


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorium: Konfiguracja FTP lokacji i tworzenie projektu

Poniższe kroki konfigurowania witryny FTP tak, aby użytkownik miał lokalizację tylko przekazywać do za pośrednictwem protokołu FTP.

### <a name="install-the-ftp-service"></a>Zainstaluj usługę FTP

1. Otwórz w aplecie Dodaj Usuń programy, wybierz opcję Dodaj/Usuń składniki systemu Windows
2. Wybierz Internetowe usługi informacyjne (serwer aplikacji w systemie Windows 2003), a następnie kliknij przycisk **szczegóły**.
3. Sprawdź **usługi protokołu transferu plików (FTP)** i kliknij przycisk **OK**.
4. Kliknij przycisk **dalej** ma zostać zainstalowana usługa FTP.

### <a name="create-a-new-folder-for-content"></a>Utwórz nowy Folder zawartości

1. W Eksploratorze Windows, Utwórz nowy folder o nazwie **Użytkownik1** wewnątrz c:\inetpub\wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Konfigurowanie uprawnień i foldery w folderach.

1. Otwórz przystawkę internetowych usług informacyjnych w menu Narzędzia administracyjne. Konieczne będzie teraz folder witryny FTP, w węźle nazwy komputera.
2. Rozwiń węzeł **witryn FTP**.
3. Kliknij prawym przyciskiem myszy **domyślne witryny FTP**, wybierz pozycję **nowy**, następnie **katalogu wirtualnego**, następnie kliknij przycisk **dalej**.
4. Wprowadź **Użytkownik1** nazwę katalogu wirtualnego i kliknij **dalej**.
5. Wprowadź **c:\inetpub\wwwroot\User1** ścieżkę i kliknij **dalej**.
6. Kliknij przycisk **dalej** , a następnie **Zakończ** aby zakończyć pracę kreatora.
7. Kliknij prawym przyciskiem myszy **Użytkownik1** do katalogu wirtualnego domyślne witryny FTP i wybierz **właściwości**.
8. Sprawdź **zapisu** wyboru i kliknij przycisk **OK** aby zamknąć okno dialogowe.
9. Kliknij prawym przyciskiem myszy **domyślne witryny FTP** i wybierz **właściwości**.
10. Na **kont zabezpieczeń** karcie, usuń zaznaczenie pola wyboru **Zezwalaj na połączenia anonimowe**.
11. Kliknij przycisk **tak** w oknie dialogowym z pytaniem, czy chcesz kontynuować.
12. Kliknij przycisk **OK** aby zamknąć okno dialogowe.
13. Rozwiń węzeł **domyślna witryna sieci Web** w obszarze **witryn sieci Web** węzła.
14. Kliknij prawym przyciskiem myszy **Użytkownik1** katalogu i zaznacz **właściwości**
15. W **ustawienia aplikacji** , kliknij przycisk **Utwórz** do oznaczania folderze co aplikacja.
16. Kliknij przycisk **OK** aby zamknąć okno dialogowe.
17. Zamknij przystawkę Internetowe usługi informacyjne.

### <a name="create-web-project"></a>Tworzenie projektu sieci web

1. Otwórz program Visual Studio 2005.
2. Z **pliku** menu, wybierz opcję **nowej witryny sieci Web**.
3. W **lokalizacji** listy rozwijanej wybierz **FTP**.
4. Kliknij przycisk **Przeglądaj**.
5. Wprowadź **localhost** w **serwera** pola tekstowego.
6. Wprowadź **Użytkownik1** w polu tekstowym katalogu.
7. Kliknij przycisk **Otwórz**. Lokalizacji FTP zostaną wprowadzone w oknie dialogowym nowej witryny sieci Web.
8. Kliknij przycisk **OK**.
9. Usuń zaznaczenie pola wyboru **anonimowego logowania** w dziennika FTP w oknie dialogowym, wprowadź swoje poświadczenia, a następnie kliknij przycisk **OK**.
10. Co to jest adres URL projektu? (Adres URL projektu będzie wyświetlana w Eksploratorze rozwiązań).
11. Z **kompilacji** menu, wybierz opcję **kompilacji witryny sieci Web** lub **Kompiluj rozwiązanie**.
12. Kliknij prawym przyciskiem myszy plik Default.aspx w Eksploratorze rozwiązań i wybierz **Wyświetl w przeglądarce**.
13. W oknie dialogowym wymagany adres URL witryny sieci Web wprowadź `http://localhost/user1` adresu URL i kliknij **OK**.

> [!NOTE]
> Jeśli błąd wskazujący brak możliwości można załadować typu \_Default, upewnij się, że są uruchomione ASP.NET 2.0 na witrynie sieci Web i nie starszej wersji. Możesz to zrobić na karcie ASP.NET w usługach IIS.


## <a name="opening-web-projects"></a>Otwierania projektów sieci Web

Otwierania projektów sieci Web jest podobny do tworzenia projektów. Poniższe sekcje wyróżnienia obszarów, aby śledzić limit dla podczas pracy w środowisku IDE. Obejmuje ona również korzystanie z projektów sieci Web przy użyciu protokołu HTTP i FTP.

Aby otworzyć projekt sieci Web, wybierz Otwórz witrynę sieci Web z menu Plik. Pojawi się w tym samym oknie dialogowym Wybierz lokalizację objętych wcześniej i mieć tego samego dostępne opcje cztery: System plików, lokalne usługi IIS, FTP i witryny zdalnego.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>System plików

Jak wspomniano wcześniej, w tym module, Visual Studio nie jest już używany jest plik projektu. Dlatego jeśli wybierzesz Otwórz witrynę sieci Web z systemu plików, faktycznie trzeba wybrać dowolnego folderu, który ma, nawet jeśli nie utworzono folder wybrany jako projekt sieci Web początkowo w programie Visual Studio. Na przykład można wybrać otworzyć folder Moje dokumenty jako witryny sieci Web i Visual Studio będzie happily ją otworzyć i wyświetlić pliki, jak pokazano poniżej.


![Moje dokumenty otwarty jako witryny sieci Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Rysunek 6**: *Moje dokumenty* otwarty jako witryny sieci Web


Ponieważ program Visual Studio tworzy tylko dodatkowe pliki i foldery, gdy jest to konieczne, nie dodatkowe pliki i foldery są dodawane do lokalizacji, które będą otwierane. Efektem ubocznym takiej architektury jest to, że uniemożliwia zagnieżdżania witryn sieci Web w systemie plików. Rozważmy na przykład następującą strukturę katalogów.

Projekt sieci Web w C:\MyWebSite

Inny projekt sieci web w C:\MyWebSite\Nested

Po otwarciu witrynę sieci Web pod c:\MyWebSite folderu zagnieżdżone pojawi się podfolder tej aplikacji.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Podczas otwierania witryn sieci Web za pośrednictwem protokołu HTTP, ustawienia są odczytywane z metabazy usług IIS (lokalne usługi IIS) lub przy użyciu rozszerzeń serwera FrontPage (lokacji zdalnej). W przypadku aplikacji sieci web zagnieżdżonych te są wyświetlane również ikonę identyfikację je jako aplikacji. Jeśli znasz pracy z aplikacjami sieci web FrontPage, przypomina zachowanie programu Visual Studio 2005.

Mimo że Wyświetla ikonę dla aplikacji, które zagnieżdżonych aplikacji, która jest obecnie otwarty w środowisku IDE programu Visual Studio, nie umożliwia można rozszerzyć je, aby wyświetlić ich zawartość. Można jednak kliknij dwukrotnie je, aby je otworzyć. Po wykonaniu, zostanie wyświetlone okno dialogowe z monitem o albo otwórz aplikacji sieci web (i Zastąp aktualnie otwarte rozwiązanie) lub Dodaj aplikację sieci Web do bieżącego rozwiązania.


![Dwukrotne kliknięcie ikony aplikacji zagnieżdżonych oferuje to okno dialogowe](improvements-in-visual-studio-2005/_static/image4.jpg)

**Rysunek 7**: dwukrotne kliknięcie ikony aplikacji zagnieżdżonych oferuje to okno dialogowe


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Witryny FTP

Po otwarciu lokacji za pośrednictwem protokołu FTP są wszystkie skopiować plików lokalnie do folderu tymczasowego. Pełna ścieżka do lokalizacji magazynu lokalnego jest wyświetlany w okienku właściwości dla projektu i jest tworzony w następującym formacie.

C:\Documents and Settings\&lt; Użytkownik&gt;\Local Settings\Temp\VWDWebCache\&lt; Serwer&gt;\_&lt;Nazwa aplikacji&gt;

W przypadku używania FTP programu Visual Studio musi określić podstawowy adres URL dla projektu, w którym można go przejść, jak pokazano poniżej. Jeśli nie określisz podstawowy adres URL, Visual Studio zapyta dla niego przeglądania strony w witrynie sieci Web po raz pierwszy.


![Określanie podstawowego adresu URL dla witryny FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Rysunek 8**: Określanie podstawowego adresu URL dla witryny FTP


## <a name="improvements-in-compilation"></a>Ulepszenia kompilacji

Praca z aplikacji sieci Web programu Visual Studio 2005 jest znacznie szybsze niż w poprzednich wersjach. Jest to wymagane na zmiany w architekturze kompilacji bez małe części.

W programie Visual Studio 2002 i 2003 aplikacji sieci Web zostały skompilowane w jeden podstawowy zestaw znajdującej się w folderze/bin. W programie Visual Studio 2005, aplikację\_folder kod został dodany. Klasy i innego kodu bez interfejsu użytkownika są dodawane do aplikacji\_katalogu z kodem. Podczas kompilacji projektu, wszystkie pliki w aplikacji w Visual Studio\_katalogu z kodem są skompilowane w jednej aplikacji\_Code.dll pliku. W wyniku tej zmiany jest znacznie szybsze niż w poprzednich wersjach kolejnych kompilacjach.

> [!NOTE]
> Narzędzia wiersza polecenia programu MSBuild mogą służyć do tworzenia aplikacji sieci Web ASP.NET. Tego narzędzia zostanie omówiona w module 9.


Inny ulepszenie kompilacji jest nowa opcja kompilacji strony w menu kompilacji. Ta funkcja umożliwia deweloperom odbudować tylko bieżącej strony (wraz z programem, kursu i zależności), dzięki czemu zmiany mogą być kompilowane szybciej. Ponieważ C# nie oferuje tła kompilacji dla celów aktualizacji IntelliSense, itp., one będą korzystać być z tej funkcji, ponieważ umożliwi IntelliSense zaktualizowania szybko za pomocą po prostu ponownie skompilować z pojedynczą stroną.

Właściwości kompilacji projektu pozwalają skonfigurować typ kompilacji, która występuje przed wykonaniem strony początkowej. Deweloperzy można tworzyć tylko na bieżącej stronie tak, aby uruchomić program Visual Studio debugowanie aplikacji szybciej po wprowadzeniu zmian w kodzie.


![Akcji uruchamiania strony kompilacji](improvements-in-visual-studio-2005/_static/image6.jpg)

**Rysunek 9**: akcji uruchamiania strony kompilacji


Innym doskonałym rozszerzenie architektura programu ASP.NET i Visual Studio znajduje się w obszarze Edytuj i Kontynuuj. W programie Visual Studio 2005 deweloperzy można rozpocząć debugowania projektu i wprowadzać zmiany kodu w projekcie bez odłączania debugera. W rzeczywistości jako literału można uruchomić debugowania projektu, Dodaj nową klasę, Dodaj kod do klasy, Dodaj kod do strony, która tworzy nowe wystąpienie tej klasy i wykonania metody klasy, bez odłączania debugera. Wykonywanie nowy kod jest dosłownie tak proste, jak odświeżyć przeglądarkę!

Kliknij tutaj, aby wyświetlić Przewodnik wideo edycji i kontynuować funkcji w programie Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Otwórz wideo na pełnym ekranie](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Niezawodna Edytuj i Kontynuuj funkcjonalność w programie ASP.NET 2.0 i programu Visual Studio 2005 jest z powodu zmiany architektury dla aplikacji ASP.NET. W programie ASP.NET 1.x, aplikacje utworzone w programie Visual Studio 2002/2003 zostały skompilowane do podstawowego zestawu, które były przechowywane w folderze/bin. Wszystkie klasy, stron, itp. dla aplikacji zostały skompilowane w tym jednej biblioteki DLL. W czasie wykonywania, ASP.NET może następnie Kompiluj wszystkie formanty, znaczników i kodu platformy ASP.NET w ramach strony i skopiować te biblioteki dll do folderu tymczasowego program ASP.NET.

W programie Visual Studio 2005 przy użyciu składnika ASP.NET 2.0, kompilacja dwa modele konspektu powyżej (jeden dla programu Visual Studio) i jeden dla platformy ASP.NET w czasie wykonywania zostały scalone w jednym wspólnym modelu kompilacji. Oznacza to, że wszystkie problemy kompilacji teraz są przechwytywane zamiast na etapie Programowanie w czasie wykonywania. Umożliwia także projektanta i obsługę funkcji IntelliSense na funkcje, takie jak kontrolek użytkownika i stron wzorcowych.

Kliknij tutaj, aby zobaczyć przewodnik wideo projektanta obsługi w przypadku kontrolek użytkownika.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Otwórz wideo na pełnym ekranie](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Gdy kontrola użytkownika zostanie usunięta ze strony @Register dyrektywy pozostaje w znaczniku i powinna zostać usunięta ręcznie, aby uniknąć błędów analizatora, jeśli formant użytkownika zostanie usunięty z witryny sieci Web.


Inny poprawy modelu kompilacji programu Visual Studio jest funkcji publikowania witryny sieci Web. Ponieważ funkcja publikowania precompiles witryny sieci Web, deweloperzy mogą korzystać dodano wydajność nie ma potrzeby skompilować niczego na żądanie. Również precompiles całego kodu źródłowego w aplikacji\_kodu folder do biblioteki DLL, aby żaden kod źródłowy ma zostać wdrożony.


![Okno dialogowe publikowania witryny sieci Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Na rysunku nr 10**: okno dialogowe publikowania witryny sieci Web


> [!NOTE]
> Aspnet\_compile.exe narzędzia można również wstępnie skompilować aplikację sieci Web programu ASP.NET. Tego narzędzia zostanie omówiona w module 9.


Po opublikowaniu witryny sieci Web, wstępnie skompilowanych plików są przechowywane w folderze Temporary ASP.NET Files jak pokazano poniżej. Pliki z *.compiled* rozszerzenie pliku to pliki XML, które Definiowanie zależności dla określonej biblioteki dll. Wszystkie formanty formularza sieci Web lub użytkownika są skompilowane w losowej bibliotek DLL, które zaczynają się od *aplikacji\_Web\_*.

Jeśli opuścisz *zezwolić tej witryny wstępnie skompilowanej aktualizacja była możliwa* zaznaczone pole wyboru, znaczników wewnątrz formantów formularzy sieci Web i użytkownik nie będzie wstępnie skompilowanym do biblioteki DLL, dzięki czemu można wprowadzać zmiany po wdrożeniu. Jeśli chcesz użyć do blokowania kod znaczników, aby zmiany do wdrożonej zawartości nie są dozwolone, usuń zaznaczenie tego pola.

*Użyj stałego nazewnictwa i jednej strony zestawy* wyboru pozwala wyłączyć kompilację masową, dzięki czemu każdy strona ma być kompilowana w ramach zestawu o nazwie stałej. Jeśli to pole pozostanie niezaznaczone umożliwia można wykorzystać kompilacji wsadów.

*Włącz silne nazwy na wstępnie skompilowana zestawy* wyboru pozwala na silnej nazwy ponownie zestawy wstępnie skompilowana.

> [!NOTE]
> W programie ASP.NET 1.x, zestawy o silnych nazwach musiały być zainstalowany w globalnej pamięci podręcznej zestawów (GAC). W programie ASP.NET 2.0 nie jest wymagane do zainstalowania zestawy o silnych nazwach w pamięci GAC.


![Pliki wstępnie skompilowanym aplikacji ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Rysunek 11**: plików wstępnie skompilowanej aplikacji ASP.NET


> [!NOTE]
> W powyższym aplikacji nie było żadnych pliku web.config. Jeśli nastąpiła, jego może zostać wywołany *PrecompiledApp.config* po publikowanie w sieci Web witryny procesu.


## <a name="improvements-in-deployment"></a>Ulepszenia we wdrożeniu

Jako programu Visual Studio 2002 i 2003, Visual Studio 2005 oferuje funkcję kopii projektu. Jednak funkcja została beefed w programie Visual Studio 2005 i nosi teraz kopiowanie witryny sieci Web.

Okno dialogowe Kopiowanie witryny sieci Web jest podzielony na ramki lewej i prawej ramce. Po lewej stronie ramki jest nazywana źródłowej witryny sieci Web i prawa nosi nazwę witryny sieci Web. Rzecz, która może mylić niektórzy deweloperzy jest czy lokacji wyświetlany w prawej ramce nie zawsze lokacji zdalnej. Może to być lokacji, lokalnego systemu plików lub w lokalnym wystąpieniu usług IIS. Ponadto witryna wyświetlonego w ramce po lewej stronie nie jest zawsze źródłowej witryny sieci Web ponieważ okno dialogowe umożliwia publikowanie ze zdalnej witryny sieci Web *do* źródłowej witryny sieci Web.

Jeśli projekt jest kopiowany do zdalnej witryny sieci Web, tej lokacji musi mieć zainstalowane rozszerzenia serwera FrontPage. Jeśli nie, należy połączyć za pomocą protokołu FTP. Z drugiej strony Jeśli projekt jest kopiowany do lokalnego wystąpienia usług IIS, rozszerzenia serwera FrontPage nie są wymagane.

> [!NOTE]
> Jeśli próbujesz utworzyć nową witrynę sieci Web w lokalnym wystąpieniu usług IIS i rozszerzenia serwera FrontPage 2002 są zainstalowane, otrzymasz komunikat o błędzie informujący, że tworzenie witryn sieci Web nie jest obsługiwane na serwerze programu SharePoint. W takim przypadku istnieje możliwość rozszerzenia serwera FrontPage 2000 instalowanie lub usuwanie rozszerzeń serwera FrontPage.


Kliknij tutaj, aby przewodnik wideo funkcji Kopiowanie witryny sieci Web.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Otwórz wideo na pełnym ekranie](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Ulepszenia debugowania

Istnieją cztery klucza ulepszenia debugowania w programie Visual Studio 2005.

- Debugowanie lokalnie z systemem innym niż administratora jest możliwe bez.
- Atrybut debugowania dla elementu kompilacji jest teraz domyślnie false.
- Zdalne debugowanie instalacja i konfiguracja jest łatwiejsze niż przedtem.
- Teraz można debugować witryny sieci Web otworzyć za pomocą lokalizacji FTP.

## <a name="debugging-as-a-non-administrator"></a>Debugowanie z systemem innym niż administratora

Dodanie ASP.NET Development Server umożliwia użytkowników niebędących administratorami łatwe debugowanie aplikacji ASP.NET dodatkowych zabiegów. Podczas debugowania aplikacji ASP.NET uruchomionej w lokalnym systemie plików programu Visual Studio uruchamia ASP.NET Development Server w kontekście zalogowanego użytkownika. Ten użytkownik może następnie Debuguj aplikację bez przeprowadzania dodatkowej konfiguracji.

## <a name="debug-is-false-by-default"></a>Debugowanie jest wartość FAŁSZ domyślnie

W programie ASP.NET 1.x, *debugowania* atrybutu w *kompilacji* ustawiono element pliku web.config *true* domyślnie. Zawsze zostały zaleca czy deweloperzy ustawioną tego atrybutu *false* przed wdrożeniem aplikacji do środowiska produkcyjnego, ale ponieważ większość deweloperów nie pełni zrozumieć skutków pozostawienia atrybut debugowania ustawiony na wartość true, wystarczy ją jako — jest.

Najbardziej poważny problem z mających atrybut debugowania ma wartość true jest wyłączany ASP.NETs partii kompilacji modelu. W związku z tym każdy strona ma być kompilowana w oddzielnych bibliotekach DLL. Jeśli sieci Web, który aplikacja składa się z tysiącami stron (nie liczyć listę w jakikolwiek sposób), która oznacza kilka tysięcy małych bibliotek dll zostanie utworzony przez tę aplikację. Mimo że te biblioteki DLL jest mały rozmiar, nie są ładowane do dowolnej lokalizacji określonej w pamięci. W związku z tym spowodować fragmentacji pamięci systemowej i może przyczynić się do wystąpienia OutOfMemoryException.

W programie ASP.NET 2.0 atrybut debugowania ma ustawioną wartość false domyślnie. Jak już już wspomniano, gdy projektant debuguje aplikacji ASP.NET w programie Visual Studio 2005, są monitowani o dodanie pliku web.config z włączonym debugowaniem. To wiąże się z tym samym wady, które były obecne w programie ASP.NET 1.x, ale teraz dewelopera jest wyraźnie ostrzeżenie, że powinni resetować atrybut na wartość false przed przeniesieniem aplikacji do środowiska produkcyjnego.

## <a name="remote-debugging-setup-and-configuration"></a>Zdalnego debugowania i Konfiguracja

W programie Visual Studio 2002/2003, zdalnego debugowania zależał od proces vs7jit.exe i Menedżer debugowania komputera (mdm.exe). Z tego powodu rozwiązywania problemów debugowania zdalnego został często czarne pole dla klientów i często nie był znacznie poprawia dla pomocy technicznej.

Visual Studio 2005 usuwa zależność od mdm.exe i vs7jit.exe procesów. Zamiast tego teraz używa usługi monitora zdalnego debugowania (msvsmon.exe).

Wymagania dla zdalnego debugowania programu Visual Studio 2005 jest bardzo proste. Musisz uruchomić msvsmon.exe na serwerze zdalnym przed debugowaniem. Monitor zdalnego debugowania można zainstalować z dysku CD programu Visual Studio lub po prostu uruchomić msvsmon.exe z udziału bez instalowania niczego w ogóle na serwerze sieci Web.

Po uruchomieniu msvsmon.exe, prawdopodobnie będzie skarżą się o portach blokowany jest dostęp do zdalnego debugowania. Na szczęście można łatwo odblokujesz portów z prawej strony, w oknie dialogowym ostrzeżenia jak pokazano poniżej.


![To powiadomienie, że Zapora systemu Windows blokuje debugowanie zdalne](improvements-in-visual-studio-2005/_static/image9.jpg)

**Rysunek 12**: to powiadomienie czy Zapora systemu Windows blokuje debugowanie zdalne


Po ma odblokowany porty niezbędne do debugowania, zostanie wyświetlona Monitor debugera zdalnego, jak pokazano poniżej. W tym interfejsie można monitorować połączenia i zmienić łatwe debugowanie uprawnień.


![Monitor debugera zdalnego](improvements-in-visual-studio-2005/_static/image10.jpg)

**Rysunek 13**: Monitor debugera zdalnego


Istnieje również możliwość zdalne debugowanie aplikacji sieci Web otwarty za pośrednictwem protokołu FTP. Te kroki są takie same jak wcześniej objętych usługą. Należy jednak określić podstawowy adres URL do przeglądania projektu FTP, zgodnie z opisem we wcześniejszej części tego modułu.

## <a name="lab-2"></a>Laboratorium 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Debugowanie zdalne z programu Visual Studio 2005

W tym laboratorium przeprowadzi użytkownika zdalnego debugowania w programie Visual Studio 2005.

Kliknij tutaj, aby przewodnik wideo dotyczący tego laboratorium.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Otwórz wideo na pełnym ekranie](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


W tym laboratorium wymaga dwóch maszyn jednej uruchomionej Visual Studio 2005 i inne uruchomione usługi IIS 5 lub nowszego.

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web na serwerze zdalnym.

> [!NOTE]
> Można utworzyć witrynę sieci Web, na zdalnym wystąpieniu programu IIS lub za pośrednictwem protokołu FTP.


1. Zdalny serwer sieci Web zlokalizuj msvsmon.exe na komputerze deweloperskim przy użyciu ścieżki UNC i wykonaj go.  
 Domyślna lokalizacja dla msvsmon.exe \\server\c$ \Program Files\Microsoft Debugger\x86 8\Common7\IDE\Remote programu Visual Studio.
2. Jeśli zostanie wyświetlony monit, aby odblokować portów do zdalnego debugowania, należy to zrobić.
3. Na komputerze deweloperskim Otwórz kodem Default.aspx i ustaw punkt przerwania na stronie\_załadować metody.
4. Uruchom profilowanie z na komputerze deweloperskim.

Powinien trafienie punktu przerwania, zgodnie z oczekiwaniami.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Jako weve już omówione Visual Studio 2005 jest dostarczany z serwera sieci Web o nazwie ASP.NET Development Server. (ASP.NET Development Server jest czasami nazywany Cassini.) Ten serwer sieci Web jest wygodny sposób przeglądania i debugowanie aplikacji sieci Web działających w systemie plików.

ASP.NET Development Server jest ograniczony serwera sieci Web. Zezwala na połączenia zdalne, nie zezwala żądań z dowolnego konta użytkownika niż użytkownika, który uruchomił serwera sieci Web. Ponadto nie ma możliwości obsługę stron ASP. Obsługiwane są tylko ASP.NET zasobów i HTML (w tym obrazów, plików CSS itp.).

ASP.NET Development Server można uruchomić z wiersza polecenia, uruchamiając plik WebDev.WebServer.exe znajdujący się w c:\Windows\Microsoft.NET\Framework\v2.0. \*\*\*\*\*. To okno dialogowe wyświetla parametry, które są dostępne.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Rysunek 14**


> [!NOTE]
> ASP.NET Development Server nie jest obsługiwana, gdy uruchamiana jawnie za pomocą wiersza polecenia.
