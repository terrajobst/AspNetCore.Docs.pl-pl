---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Ulepszenia w programie Visual Studio 2005 | Dokumentacja firmy Microsoft
author: microsoft
description: Visual Studio 2005 zawiera twórcom długą listę usprawnień i ulepszeń do projektów sieci Web w aplikacji sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 35ebb6e94c3da105b802c843e36f193f81a45782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397642"
---
<a name="improvements-in-visual-studio-2005"></a>Ulepszenia w programie Visual Studio 2005
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Visual Studio 2005 zawiera twórcom długą listę usprawnień i ulepszeń do projektów sieci Web w aplikacji sieci Web.


Visual Studio 2005 zawiera twórcom długą listę usprawnień i ulepszeń do projektów sieci Web w aplikacji sieci Web. Tak wydajna jak Visual Studio .NET 2002 i 2003 wystąpiły wiele skarg w taki sposób, że projekty sieci Web zostały obsłużone. Visual Studio 2005 dodaje szereg istotnych nowe funkcje, aby rozwiązać te skargi. Dla tych, którzy wolą sposób obsługi kompilacji aplikacji sieci Web w Visual Studio .NET 2003, zobacz [projektów aplikacji sieci Web](https://go.microsoft.com/fwlink/?LinkId=57870).

W tym module również obejmować ulepszenia tworzenia projektu sieci Web, zarządzania i programowania. W nowszym module również obejmować ulepszenia tworzenia projektów sieci Web i wdrażanie ich.

## <a name="frontpage-server-extensions"></a>Rozszerzenia serwera FrontPage

Visual Studio .NET 2002 i 2003 wymagane rozszerzenia serwera FrontPage w polu, aby można było utworzyć lub tworzenie projektów sieci Web. Deweloperzy miał wyboru między dwoma trybami różny dostęp (tryb dostępu do pliku lub rozszerzenia serwera FrontPage), używane rozszerzenia serwera FrontPage do wykonania zadania, takie jak ustawienie katalog główny aplikacji w usługach IIS itp.

Visual Studio 2005 usuwa zależność od rozszerzenia serwera FrontPage, dla lokalnych projektów. Programu Visual Studio 2005 uzyskuje teraz dostęp do metabazy usług IIS bezpośrednio zamiast przy użyciu rozszerzenia serwera FrontPage. Visual Studio 2005 dodają także obsługę protokołu FTP, która pozwala na potrzeby dostępu zdalnego projektu bez rozszerzenia serwera FrontPage.

Dla deweloperów, którzy chcą korzystać z rozszerzeń serwera FrontPage w swoich projektach opcja będzie nadal dostępna. Jednak na podstawie silne opinii od społeczności deweloperów programu ASP.NET, nie jest to wymagane.

> [!NOTE]
> Rozszerzenia serwera FrontPage są nadal wymagane dla projektu zdalnego tworzenia, otwierania itp.


## <a name="aspnet-development-server"></a>ASP.NET Development Server

Visual Studio 2005 jest dostarczany z serwerem sieci Web o nazwie ASP.NET Development Server. (Ten serwer sieci Web była wcześniej znana jako Cassini).

Ma kilka zalet ASP.NET Development Server.

- Teraz jest możliwa dla użytkowników niebędących administratorami na programowanie i debugowanie dla serwera sieci Web.
- ASP.NET Development Server dynamicznie mapuje katalogów wirtualnych w dowolnej lokalizacji w systemie plików, umożliwiając dla lokalizacji projektu elastyczne.
- Użytkowników w systemie Windows XP Professional, którzy już używają programu IIS będą teraz mogli tworzyć nowe aplikacje sieci Web, które nie ma wpływu na strukturze pliku lub folderu, z ich domyślna witryna sieci Web w usługach IIS.

Aby móc korzystać z ASP.NET Development Server jest wymagana żadna konfiguracja specjalne. Gdy projekt sieci Web, która jest hostowana w systemie plików jest debugowania lub przeglądania, Visual Studio 2005 automatycznie uruchomić wystąpienie ASP.NET Development Server na losowy port do obsługi żądania.

Więcej informacji zostanie omówione w ASP.NET Development Server, w dalszej części tego modułu.

## <a name="improved-file-management"></a>Zarządzanie plikami ulepszone

W Visual Studio 2002 i 2003 plik projektu (.vbproj dla VB.NET) i pliku csproj dla języka C# przechowywane informacje we wszystkich plikach w aplikacji sieci Web. Wyświetlanie w Eksploratorze rozwiązań opiera się na podstawie informacji z pliku w pliku projektu. W związku z tym Eksploratora rozwiązań często umożliwią wyświetlanie nieprawidłowych informacji w przypadkach, w których były używane edytory zewnętrzne. Visual Studio 2002 i 2003 będzie często zastąpić zmiany pliku lub wyświetla najnowszą wersję plików.

Visual Studio 2005 natychmiast odbywa się przy użyciu pliku projektu. Zamiast tego odczytuje informacje plików i folderów bezpośrednio z dysku, wynikiem dokładnego wyświetlania plików w projekcie. Ponieważ w folderze odwołania w Visual Studio 2002 i 2003 nie odzwierciedla rzeczywistego folderu w aplikacji sieci Web, programu Visual Studio 2005 spowoduje również usunięcie w folderze odwołania w Eksploratorze rozwiązań. Aby uzyskać dostęp do odwołania do projektu w programie Visual Studio 2005, należy użyć strony właściwości dla projektu.

## <a name="creating-web-projects"></a>Tworzenie projektów sieci Web

Deweloperzy sieci Web mają wiele nowych opcji dostępnych do utworzenia projektu w programie Visual Studio 2005. Witryny sieci Web można teraz tworzyć dowolne miejsce w systemie plików, a następnie można debugować lub przeglądać przy użyciu nowego oprogramowania ASP.NET Development Server. Deweloperzy mogą również tworzyć nowych witryn sieci Web przy użyciu protokołu FTP.

Kliknij tutaj, aby wyświetlić Przewodnik wideo dotyczący tworzenia projektów sieci Web w programie Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Otwórz wideo pełnego ekranu](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Projekty systemu plików

Jak przedstawiono w przewodnik wideo można utworzyć witryny sieci Web w systemie plików na komputerze lokalnym lub w zdalnej lokalizacji za pośrednictwem udziału plików. Witryny sieci Web, które są tworzone w systemie plików są przeglądane i debugowania przy użyciu serwera projektowego ASP.NET.

> [!NOTE]
> ASP.NET Development Server może być niejasne dla klientów. Jeśli projekt sieci Web została utworzona w systemie plików w strukturze katalogów o IISs (czyli c:/Lokalizacja inetpub/wwwroot), nadal można przeglądać witryny sieci Web za pomocą programu ASP.NET Development Server, gdy uruchamiana z poziomu programu Visual Studio 2005. W związku z tym nie dotyczy konfigurowania usług IIS (np. metody uwierzytelniania).


Domyślny projekt sieci web spowoduje również usunięcie znacznie obciążenie przez tylko obejmuje strony Default.aspx, default.cs plików i folderów aplikacji/_dane. Plik web.config i foldery specjalne (czyli aplikacji/_fragmenty) są dodawane, ponieważ są one potrzebne. Projekt sieci web zawiera tylko pliki i foldery, które są potrzebne.

### <a name="http-projects"></a>Projekty HTTP

Projekty HTTP może być projektów, które zostały utworzone w lokalnej witrynie sieci Web usług IIS lub na zdalnej witrynie sieci Web. Domyślna lokalizacja projektu jest `http://localhost`. Jeśli klikniesz przycisk przeglądania, dostępne są dwie opcje HTTP: lokalne usługi IIS i zdalnej witrynie. Główną różnicą w tych dwóch opcji jest to metoda, w którym dane witryny sieci web jest wyświetlany w oknie dialogowym Wybierz lokalizację, jak i w jaki sposób pliki są kopiowane do serwera sieci Web.

Opcja lokalne usługi IIS odczytuje informacje o lokacji w metabazie na komputerze lokalnym i pliki są kopiowane w systemie plików. Opcja lokacji zdalnej przy użyciu rozszerzenia serwera FrontPage i informacje o lokacji i pliki są kopiowane, przy użyciu protokołu HTTP i wywołań RPC rozszerzeń serwera FrontPage.

> [!NOTE]
> Plik vs###/_tmp.htm i get/_aspx/_ver.aspx nie są już używane do określenia informacje o wersji.


Domyślna opcja HTTP jest lokalne usługi IIS. Ta opcja odczytuje metabazy usług IIS, aby określić, które witryny są dostępne i lokalizację, w której ma zostać utworzona zawartość. Można wybrać inny folder lub katalog wirtualny, wybierając go w widoku drzewa. Użytkownik może również Utwórz nowy katalog wirtualny, oznacz foldery jako aplikacje, a także usuwania istniejących katalogów wirtualnych w tym oknie dialogowym.


![Wybierz lokalizację w oknie dialogowym](improvements-in-visual-studio-2005/_static/image1.gif)

**Rysunek 1**: Wybierz lokalizację w oknie dialogowym


W odróżnieniu od we wcześniejszych wersjach programu Visual Studio, jeśli zaznaczysz **Użyj protokołu Secure Sockets Layer** pole wyboru i certyfikat SSL jest niezgodny adres URL przeglądania, zostanie wyświetlone okno dialogowe Alert zabezpieczeń pytaniem, jeśli będzie np. kontynuować. Przy użyciu programu Visual Studio .NET 2003, jeśli certyfikat nie jest zgodne jeden, tworzenia projektu zakończy się niepowodzeniem.


![Certyfikat SSL dotyczący alertów zabezpieczeń](improvements-in-visual-studio-2005/_static/image2.gif)

**Rysunek 2**: Alert zabezpieczeń dotyczące certyfikatu SSL


### <a name="note-on-host-headers"></a>Uwaga dotycząca nagłówki hosta

W przypadku tworzenia aplikacji sieci Web w lokacji powiązane z określonego adresu IP, należy upewnić się, że skonfigurowano nagłówka hosta. W przeciwnym razie program Visual Studio utworzy witrynie `http://localhost`, ale adres IP nie zostanie rozwiązany poprawnie, gdy witryna jest przeglądania lub debugowania z poziomu środowiska IDE programu.

Jeśli zostanie wybrana opcja lokacji zdalnej, okno dialogowe zmieni się na umożliwiają wprowadzanie docelowy adres URL dla nowej witryny sieci Web. Ten adres URL musi być na serwerze, który ma rozszerzenia serwera FrontPage, które są włączone. Jeśli użytkownik chce pracować z lokalnego serwera sieci Web za pomocą rozszerzenia serwera FrontPage, należy użyć opcji lokacji zdalnej i określić lokalny adres URL.


![Tworzenie witryny sieci Web na serwerze zdalnym](improvements-in-visual-studio-2005/_static/image1.jpg)

**Rysunek 3**: tworzenie witryny sieci Web na serwerze zdalnym


Podczas tworzenia aplikacji w lokalizacji zdalnej za pomocą protokołu SSL, jeśli certyfikat SSL nie jest zgodny, okno dialogowe potwierdzenia jest nieco inna niż okno dialogowe wyświetlane podczas korzystania z opcji lokalnych usług IIS.


![Alert zabezpieczeń lokacji zdalnej](improvements-in-visual-studio-2005/_static/image3.gif)

**Rysunek 4**: Alert zabezpieczeń lokacji zdalnej


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 wprowadza możliwość utworzenia witryny sieci Web za pośrednictwem protokołu FTP. Tej opcji IDE tworzy pliki lokalnie w folderze temp użytkowników, a następnie używa protokołu FTP można przenieść pliki do lokalizacji FTP.

> [!NOTE]
> Lokalizacja folderu tymczasowego to c:/Documents and Settings /&lt;użytkownika&gt;/Local ustawienia/Temp/VWDWebCache/&lt;serwera&gt;/_&lt;nazwy aplikacji&gt;


Korzystając z opcji FTP, zostanie wyświetlone okno dialogowe Wybierz lokalizację. Wprowadzeniu wymaganych informacji o połączeniu FTP do tego okna dialogowego, jak pokazano poniżej.


![Wybierz okno dialogowe lokalizacji FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Rysunek 5**: Wybierz okno dialogowe lokalizacji FTP


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorium: Konfiguracja, FTP lokacji i Utwórz projekt

Poniższe kroki konfigurowania witryny FTP, tak, aby użytkownik miał lokalizacji, która tylko mogą przekazać do za pośrednictwem protokołu FTP.

### <a name="install-the-ftp-service"></a>Zainstaluj usługę FTP

1. Otwórz Dodaj/Usuń programy, wybierz opcję Dodaj/Usuń składniki Windows
2. Wybierz Internet Information Services (serwera aplikacji w systemie Windows 2003), a następnie kliknij przycisk **szczegóły**.
3. Sprawdź **usługi protokołu FTP (File Transfer)** i kliknij przycisk **OK**.
4. Kliknij przycisk **dalej** zainstalować usługę FTP.

### <a name="create-a-new-folder-for-content"></a>Utwórz nowy Folder zawartości

1. W Eksploratorze Windows, Utwórz nowy folder o nazwie **Użytkownik1** wewnątrz inetpub/c:/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Skonfiguruj uprawnienia i foldery w folderach.

1. Otwórz przystawkę Internetowe usługi informacyjne, w menu Narzędzia administracyjne. Teraz masz folder witryny FTP w węźle nazwy komputera.
2. Rozwiń **witryn FTP**.
3. Kliknij prawym przyciskiem myszy **domyślne witryny FTP**, wybierz opcję **New**, następnie **katalog wirtualny**, następnie kliknij przycisk **dalej**.
4. Wprowadź **Użytkownik1** nazwą katalogu wirtualnego i kliknij przycisk **dalej**.
5. Wprowadź **c:/inetpub/wwwroot/Użytkownik1** ścieżkę, a następnie kliknij przycisk **dalej**.
6. Kliknij przycisk **dalej** i następnie **Zakończ** aby zakończyć działanie kreatora.
7. Kliknij prawym przyciskiem myszy **Użytkownik1** katalogu wirtualnego w obszarze Domyślna witryna FTP, a następnie wybierz pozycję **właściwości**.
8. Sprawdź **zapisu** pole wyboru i kliknij przycisk **OK** aby zamknąć okno dialogowe.
9. Kliknij prawym przyciskiem myszy **domyślne witryny FTP** i wybierz **właściwości**.
10. Na **kont zabezpieczeń** karcie, usuń zaznaczenie pola wyboru **Zezwalaj na połączenia anonimowe**.
11. Kliknij przycisk **tak** w oknie dialogowym z pytaniem, jeśli chcesz kontynuować.
12. Kliknij przycisk **OK** aby zamknąć okno dialogowe.
13. Rozwiń **domyślna witryna sieci Web** w obszarze **witryn sieci Web** węzła.
14. Kliknij prawym przyciskiem myszy **Użytkownik1** katalogu i zaznacz **właściwości**
15. W **ustawienia aplikacji** kliknij **Utwórz** do oznaczania w folderze, co aplikacja.
16. Kliknij przycisk **OK** aby zamknąć okno dialogowe.
17. Zamknij przystawkę Internetowe usługi informacyjne.

### <a name="create-web-project"></a>Tworzenie projektu sieci web

1. Otwórz okno programu Visual Studio 2005.
2. Z **pliku** menu, wybierz opcję **nową witrynę sieci Web**.
3. W **lokalizacji** listy rozwijanej wybierz **FTP**.
4. Kliknij przycisk **Przeglądaj**.
5. Wprowadź **localhost** w **serwera** pola tekstowego.
6. Wprowadź **Użytkownik1** w polu tekstowym katalogu.
7. Kliknij przycisk **Otwórz**. W oknie dialogowym nowej witryny sieci Web należy wprowadzić lokalizację FTP.
8. Kliknij przycisk **OK**.
9. Usuń zaznaczenie pola wyboru **zalogować się do niego anonimowy** w dzienniku FTP w oknie dialogowym Wprowadź swoje poświadczenia, a następnie kliknij przycisk **OK**.
10. Co to jest adres URL projektu? (Adres URL projektu zostaną wyświetlone w oknie Solution Explorer).
11. Z **kompilacji** menu, wybierz opcję **kompilacji witryny sieci Web** lub **Kompiluj rozwiązanie**.
12. Kliknij prawym przyciskiem myszy na Default.aspx w Eksploratorze rozwiązań i wybierz **Pokaż w przeglądarce**.
13. W oknie dialogowym wymagany adres URL witryny sieci Web, wprowadź `http://localhost/user1` adresu URL, a następnie kliknij przycisk **OK**.

> [!NOTE]
> Jeśli błąd wskazujący brakiem, aby załadować /_Default typu, upewnij się, że działają ASP.NET 2.0 w witrynie sieci Web i nie starszej wersji. Możesz to zrobić na karcie ASP.NET w programie Internet Information Services.


## <a name="opening-web-projects"></a>Otwierania projektów sieci Web

Otwierania projektów sieci Web jest podobny do tworzenia projektów. Poniższe sekcje wyróżnienia obszarów, aby zorientować się, aby uzyskać podczas pracy w środowisku IDE. Obejmuje ona również korzystanie z projektów sieci Web przy użyciu protokołu HTTP i FTP.

Aby otworzyć projekt sieci Web, wybierz Otwórz witrynę sieci Web za pomocą menu Plik. Zostanie wyświetlony monit z tego samego okna dialogowego Wybierz lokalizację omówione wcześniej i ma ten sam cztery opcje dostępne dla użytkownika: System plików, lokalnych usług IIS, FTP i lokacji zdalnej.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>System plików

Jak wskazano wcześniej, w tym module, Visual Studio nie jest już używa pliku projektu. W związku z tym jeśli użytkownik chce otworzyć witrynę sieci Web w systemie plików, faktycznie masz wybór wszystkich folderach, które chcesz, nawet jeśli nie utworzono folder, który wybierzesz jako projekt sieci Web początkowo w programie Visual Studio. Na przykład można wybrać otworzyć folder Moje dokumenty, jako witryny sieci Web i Visual Studio trafem korzysta go otworzyć i wyświetlić pliki, jak pokazano poniżej.


![Moje dokumenty, otwarty jako witryny sieci Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Rysunek 6**: *Moje dokumenty* otwarty jako witryny sieci Web


Ponieważ program Visual Studio tworzy tylko dodatkowe pliki i foldery, gdy jest to konieczne, żadne dodatkowe pliki lub foldery są dodawane do lokalizacji, w której możesz otworzyć. Efekt uboczny tej architektury jest to, że uniemożliwia zagnieżdżania witryn sieci Web w systemie plików. Na przykład rozważmy następującą strukturę katalogów.

Projekt sieci Web w C:/MojaWitrynaSieciWeb

Inny projekt sieci web w C:/MojaWitrynaSieciWeb/zagnieżdżone

Po otwarciu witryny sieci Web w c:/MojaWitrynaSieciWeb, folder zagnieżdżone będą wyświetlane jako podfolder danej aplikacji.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Podczas otwierania witryn sieci Web za pośrednictwem protokołu HTTP, ustawienia są odczytywane z metabazy usług IIS (lokalne usługi IIS) lub za pomocą rozszerzenia serwera FrontPage (lokacji zdalnej). W przypadku aplikacji sieci web zagnieżdżone, są one widoczne również ikoną identyfikuje je jako aplikację. Osoby zaznajomione z pracy z aplikacjami sieci web programu FrontPage przypomina zachowanie w programie Visual Studio 2005.

Mimo że program Visual Studio wyświetli ikonę dla aplikacji, które zagnieżdżonych aplikacji, która jest obecnie otwarty w środowisku IDE, nie umożliwia można rozszerzyć je, aby wyświetlić jego zawartość. Można jednak kliknij dwukrotnie je, aby je otworzyć. Po wykonaniu czynności zostanie wyświetlone okno dialogowe monitowania o albo otworzyć aplikację sieci web (i zastąpić aktualnie otwarte rozwiązanie) lub dodać aplikację sieci Web do bieżącego rozwiązania.


![Dwukrotne kliknięcie ikony aplikacji zagnieżdżonych przedstawia to okno dialogowe](improvements-in-visual-studio-2005/_static/image4.jpg)

**Rysunek 7**: dwukrotne kliknięcie ikony aplikacji zagnieżdżonych przedstawia to okno dialogowe


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Witryna FTP

Po otwarciu witryny przy użyciu protokołu FTP, pliki są wszystkie kopiowane lokalnie do folderu tymczasowego. Pełną ścieżkę do lokalizacji magazynu lokalnego jest wyświetlany w okienku właściwości dla projektu i jest tworzona przy użyciu następującego formatu.

C:/Documents and Settings /&lt;użytkownika&gt;/Local ustawienia/Temp/VWDWebCache/&lt;serwera&gt;/_&lt;nazwy aplikacji&gt;

Gdy za pomocą protokołu FTP, Visual Studio należy określić podstawowy adres URL dla projektu, możesz je przeglądać, jak pokazano poniżej. Jeśli podstawowy adres URL nie jest określony, program Visual Studio wyświetli monit dla niej próba przejść na stronę w witrynie sieci Web po raz pierwszy.


![Określanie podstawowego adresu URL dla witryny FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Rysunek 8**: Określanie podstawowego adresu URL dla witryny FTP


## <a name="improvements-in-compilation"></a>Ulepszenia kompilacji

Praca z aplikacji sieci Web w programie Visual Studio 2005 jest znacznie krótszy niż w starszych wersjach. Jest to termin w małej części na zmiany w architekturze kompilacji.

W programie Visual Studio 2002 i 2003 aplikacji sieci Web zostały skompilowane do jednego zestawu podstawowej usługi znajdujące się w folderze/bin. W programie Visual Studio 2005 folder aplikacji/_fragmenty został dodany. Klasy i innego kodu, bez interfejsu użytkownika są dodawane do folderu aplikacji/_fragmenty. Gdy program Visual Studio tworzy projekt, wszystkie pliki w folderze aplikacji/_fragmenty są kompilowane do pojedynczego pliku App/_Code.dll. W wyniku tej zmiany jest, że kolejne kompilacje są znacznie szybsze niż w poprzednich wersjach.

> [!NOTE]
> Narzędzie wiersza polecenia programu MSBuild można również tworzyć aplikacje sieci Web platformy ASP.NET. Tego narzędzia zostały omówione w module 9.


Innym usprawnieniem kompilacji jest nowa opcja Stroka kompilacji w menu Kompiluj. Ta funkcja umożliwia zatem programistą, aby ponownie skompilować tylko bieżącej strony (wraz z programem, kursu i zależności) tak, aby zmiany może być kompilowane szybciej. Ponieważ języka C# nie oferuje kompilacja tła na potrzeby aktualizowania funkcji IntelliSense, itp., ich będą mogli korzystać bardzo z tej funkcji, ponieważ pozwoli to na technologię IntelliSense, aby zaktualizować szybko po prostu ponownie skompilować z pojedynczą stroną.

Właściwości kompilacji dla projektu umożliwiają skonfigurowanie typ kompilacji, który występuje, zanim zostanie wykonany stronę startową. Deweloperzy mogą być tylko kompilacji na bieżącej stronie, aby program Visual Studio można uruchomić, szybsze debugowanie aplikacji po zmianie kodu.


![Akcja kompilacji początek strony](improvements-in-visual-studio-2005/_static/image6.jpg)

**Rysunek 9**: akcji strony uruchamiania kompilacji


Innym usprawnieniem doskonałe architektura platformy ASP.NET i programu Visual Studio znajduje się w obszarze Edytuj i Kontynuuj. W programie Visual Studio 2005 deweloperzy mogą rozpocząć debugowanie projektu i wprowadzać zmiany kodu w projekcie bez odłączania debugera. W rzeczywistości dosłownie rozpocząć debugowanie projektu, Dodaj nową klasę, Dodaj kod dla tej klasy, Dodaj kod do strony, która tworzy nowe wystąpienie klasy i wykonać metodę do klasy bez odłączania debugera. Wykonywanie nowy kod jest dosłownie bardzo proste — wystarczy odświeżeniu przeglądarki!

Kliknij tutaj, aby zobaczyć przewodnik wideo, edycji i kontynuować funkcji w programie Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Otwórz wideo pełnego ekranu](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Niezawodne Edytuj i Kontynuuj funkcji w programie ASP.NET 2.0 i Visual Studio 2005 jest z powodu zmiany architektury dla aplikacji ASP.NET. W programie ASP.NET: 1.x, aplikacje utworzone w programie Visual Studio 2002/2003 zostały skompilowane do podstawowego zestawu, który był przechowywany w folderze/bin. Wszystkie klasy, stron, itp. dla aplikacji zostały skompilowane w tym jedną bibliotekę DLL. W czasie wykonywania, ASP.NET może następnie skompilować wszystkie kontrolki, znaczników i kodu platformy ASP.NET na stronach i skopiuj tych bibliotek DLL do folderu tymczasowego środowiska ASP.NET.

W programie Visual Studio 2005 za pomocą programu ASP.NET 2.0, modele dwóch kompilacji konturu powyżej (po jednym dla programu Visual Studio) i jeden dla platformy ASP.NET w środowisku uruchomieniowym zostały scalone w jeden wspólny model kompilacji. Oznacza to, że wszystkie problemy kompilacji teraz są przechwytywane zamiast na etapie projektowania w czasie wykonywania. Umożliwia także projektanta i obsługę funkcji IntelliSense na funkcje, takie jak kontrolki użytkownika i stron wzorcowych.

Kliknij tutaj, aby zobaczyć przewodnik wideo dotyczący wsparcie dla kontrolki użytkownika.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Otwórz wideo pełnego ekranu](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Po usunięciu kontrolki użytkownika na stronie @Register dyrektywy pozostaje w znaczniku i powinny zostać usunięte ręcznie, aby uniknąć błędów analizatora, jeśli formant użytkownika zostanie usunięta z witryny sieci Web.


Poprawa innego modelu kompilacji Visual Studio to funkcja Publikowanie witryny sieci Web. Ponieważ funkcja Publikuj wstępnie kompiluje witryny sieci Web, deweloperzy mogą korzystać dodano wydajność nie ma potrzeby Kompiluj wszystko na żądanie. On również wstępnie kompiluje całego kodu źródłowego w folderze aplikacji/_fragmenty do biblioteki DLL tak, aby żaden kod źródłowy ma zostać wdrożone.


![Okno dialogowe publikowania witryny sieci Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Na rysunku nr 10**: okno dialogowe publikowania witryny sieci Web


> [!NOTE]
> Narzędzie aspnet/_compile.exe można również wstępnie skompilować aplikację sieci Web ASP.NET. Tego narzędzia zostały omówione w module 9.


Gdy Publikuj witryny sieci Web, wstępnie skompilowane pliki są przechowywane w folderze plików tymczasowych ASP.NET jak pokazano poniżej. Pliki z *.compiled* rozszerzenie pliku są plikami XML, które Definiowanie zależności dla określonej biblioteki dll. Wszystkie formanty formularza sieci Web lub użytkownika są kompilowane do losowych bibliotek DLL, które zaczynają się od *aplikacji /_sieci Web /_*.

Jeśli pozostawisz *tej prekompilowanej witryny zezwalać na aktualizacje* zaznaczone pole wyboru, znaczników wewnątrz formantów formularzy sieci Web i użytkownik nie będzie wstępnie skompilowanym do biblioteki DLL, umożliwiając wprowadzanie zmian po wdrożeniu. Jeśli chcesz użyć do blokowania języka znaczników, więc, że zmiany do wdrożonej zawartości nie są dozwolone, usuń zaznaczenie tego pola.

*Użyj stałego nazewnictwa i jednym zestawy strony* pola wyboru pozwala wyłączyć kompilację masową, tak aby każda strona jest skompilowany w zestawie o nazwie stałej. Pozostawienie to pole niezaznaczone pozwala na korzystanie z zalet kompilacji wsadowej.

*Włącz zewnictwo w prekompilowanych zestawach* pola wyboru pozwala na silnej nazwy wstępnie skompilowanych zestawów.

> [!NOTE]
> W programie ASP.NET: 1.x, zestawy o silnych nazwach musiały być zainstalowany w globalnej pamięci podręcznej zestawów (GAC). W programie ASP.NET 2.0 nie jest wymagane do instalowania zestawów o silnych nazwach w GAC.


![Pliki wstępnie skompilowanych aplikacji ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Rysunek 11**: pliki wstępnie skompilowanych aplikacji ASP.NET


> [!NOTE]
> W powyższym aplikacji wystąpił nie pliku web.config. Jeśli nastąpiła, jego może zostać wywołany *PrecompiledApp.config* po publikowanie w sieci Web site procesu.


## <a name="improvements-in-deployment"></a>Ulepszenia we wdrożeniu

Jako za pomocą programu Visual Studio 2002 i 2003, Visual Studio 2005 oferuje funkcję kopiowania projektu. Jednak funkcja ta została beefed w programie Visual Studio 2005 i jest teraz nazywana Kopiuj witrynę sieci Web.

Okno dialogowe Kopiowanie witryny sieci Web jest dzielony na ramkę po lewej i prawej ramce. Po lewej stronie ramki nosi nazwę źródłowej witryny sieci Web i prawa nosi nazwę witryny sieci Web. Jeden element, który może mylić niektórzy deweloperzy jest witryny wyświetlane w prawej ramce, nie zawsze lokacji zdalnej. Może to być lokacja w lokalnym systemie plików lub w lokalnym wystąpieniu usług IIS. Ponadto witryna wyświetlonego w ramce po lewej stronie nie jest koniecznie źródłowej witryny sieci Web ponieważ okno dialogowe umożliwia publikowanie ze zdalnej witryny sieci Web *do* źródłowej witryny sieci Web.

Jeśli projekt jest kopiowany do zdalnej witryny sieci Web, tej lokacji musi mieć na nim zainstalowane rozszerzenia serwera FrontPage. Jeśli nie, należy nawiązać połączenie przy użyciu protokołu FTP. Z drugiej strony Jeśli projekt jest kopiowany do lokalnego wystąpienia usług IIS, rozszerzenia serwera FrontPage nie są wymagane.

> [!NOTE]
> Jeśli spróbujesz utworzyć nową witrynę sieci Web w lokalnym wystąpieniu usług IIS i zainstalowanych rozszerzeń serwera FrontPage 2002, zostanie wyświetlony komunikat o błędzie informujący o tym, że tworzenie witryn sieci Web nie jest obsługiwany na serwerze programu SharePoint. W takiej sytuacji istnieje możliwość rozszerzenia serwera FrontPage 2000 instalowania lub usuwania rozszerzenia serwera FrontPage.


Kliknij tutaj, aby uzyskać przewodnik wideo dotyczący funkcji kopiowania witryny sieci Web.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Otwórz wideo pełnego ekranu](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Ulepszenia debugowania

Istnieją cztery kluczowe ulepszenia debugowania w programie Visual Studio 2005.

- Możliwe gotowych jest debugowania lokalnego jako użytkowników niebędących administratorami.
- Atrybut debugowania dla elementu kompilacji jest teraz false domyślnie.
- Zdalne debugowanie, instalacja i konfiguracja jest łatwiejsze niż przedtem.
- Można teraz debugować witrynę sieci Web, otwarty z lokalizacji FTP.

## <a name="debugging-as-a-non-administrator"></a>Debugowanie jako użytkowników niebędących administratorami

Dodanie ASP.NET Development Server umożliwia użytkowników niebędących administratorami łatwe debugowanie aplikacji ASP.NET, gotową do. Podczas debugowania aplikacji ASP.NET uruchomionej w lokalnym systemie plików programu Visual Studio uruchamia ASP.NET Development Server, w kontekście zalogowanego użytkownika. Ten użytkownik następnie można debugować aplikację bez przeprowadzania dodatkowej konfiguracji.

## <a name="debug-is-false-by-default"></a>Debugowanie jest wartość FAŁSZ domyślnie

W programie ASP.NET: 1.x, *debugowania* atrybutu w *kompilacji* element w pliku web.config został ustawiony na wartość *true* domyślnie. Zawsze zostały zaleca się że deweloperzy Ustaw ten atrybut na *false* przed wdrożeniem aplikacji do środowiska produkcyjnego, ale ponieważ większość programistów nie w pełni rozumieją konsekwencje opuszczania atrybut debugowania, ustaw wartość true, one po prostu, że pozostała jako-to.

Najbardziej poważny problem wymaga atrybutu debugowania jest wyłącza ASP.NETs partii kompilacji modelu ma wartość true. W związku z tym każda strona jest skompilowana do oddzielnych bibliotek DLL. Jeśli dany internetowy aplikacja składa się z tysięcy stron (nie liczyć części za pomocą jakichkolwiek środków), oznacza to kilka tysięcy małych bibliotek DLL zostaną utworzone przez tę aplikację. Mimo że te biblioteki DLL mały rozmiar, nie są ładowane do dowolnej określonej lokalizacji w pamięci. Dlatego spowodować fragmentacji pamięci systemowej i może przyczynić się do wystąpienia OutOfMemoryException.

W programie ASP.NET 2.0 atrybut debugowania jest domyślnie wartość false. Jak już wiesz, gdy deweloper debuguje aplikację ASP.NET w programie Visual Studio 2005, są monitowani o dodanie pliku web.config z włączonym debugowaniem. To wiąże się tego samego wady, które znajdowały się w programie ASP.NET: 1.x, ale teraz deweloper jest wyraźnie ostrzegany atrybut powinien być resetowany na wartość false przed przeniesieniem aplikacji do środowiska produkcyjnego.

## <a name="remote-debugging-setup-and-configuration"></a>Konfiguracja debugowania zdalnego i Konfiguracja

W programie Visual Studio 2002/2003, zdalne debugowanie polegać proces vs7jit.exe i Menedżer debugowania komputera (mdm.exe). Z tego powodu rozwiązywania problemów debugowania zdalnego z jego utrzymania często był czarne pole dla klientów i często nie był znacznie lepiej pomocy technicznej.

Visual Studio 2005 usuwa zależność od mdm.exe i vs7jit.exe procesów. Zamiast tego używa teraz usługi Monitor zdalnego debugowania (msvsmon.exe).

Wymagania dla zdalnego debugowania w programie Visual Studio 2005 jest bardzo proste. Musisz uruchomić msvsmon.exe na serwerze zdalnym przed debugowania. Monitor zdalnego debugowania można zainstalować z dysku CD programu Visual Studio, lub możesz po prostu msvsmon.exe można uruchamiać z udziału bez konieczności instalowania nic w ogóle na serwerze sieci Web.

Po uruchomieniu msvsmon.exe, prawdopodobnie będzie zgłaszać błąd o portach blokowane dla zdalnego debugowania. Łatwo można szczęście odblokować portów z prawej strony w oknie dialogowym ostrzeżenia, jak pokazano poniżej.


![Powiadamianie zapory Windows blokuje debugowanie zdalne](improvements-in-visual-studio-2005/_static/image9.jpg)

**Rysunek 12**: powiadomienie, że Zapora Windows jest blokuje debugowanie zdalne


Po ma odblokowany porty niezbędne do debugowania, zostanie wyświetlony Monitor zdalnego debugowania, jak pokazano poniżej. W tym interfejsie można monitorować połączenia i zmienić uprawnień debugowania, łatwo.


![Monitor debugera zdalnego](improvements-in-visual-studio-2005/_static/image10.jpg)

**Rysunek 13**: Monitor debugera zdalnego


Jest również możliwe zdalne debugowanie aplikacji sieci Web otwarte przy użyciu protokołu FTP. Kroki są takie same, jak omówione wcześniej. Należy określić podstawowy adres URL do przeglądania projektu FTP, zgodnie z opisem we wcześniejszej części tego modułu.

## <a name="lab-2"></a>Laboratorium 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Zdalne debugowanie w programie Visual Studio 2005

W tym laboratorium przeprowadzi zdalnego debugowania w programie Visual Studio 2005.

Kliknij tutaj, aby przewodnik wideo dotyczący tego laboratorium.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Otwórz wideo pełnego ekranu](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


W tym laboratorium musisz mieć dwie maszyny, co uruchamianie programu Visual Studio 2005 i inne uruchomione usługi IIS 5 lub nowszego.

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web na serwerze zdalnym.

> [!NOTE]
> Można utworzyć witryny sieci Web, na zdalnym wystąpieniu programu IIS lub za pośrednictwem protokołu FTP.


1. Z serwerem sieci Web zlokalizuj msvsmon.exe na komputerze deweloperskim, przy użyciu ścieżki UNC, a następnie uruchomić go.  
 Domyślną lokalizacją msvsmon.exe jest //server/c$/Program plików/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Jeśli zostanie wyświetlony monit o odblokowanie portów dla zdalnego debugowania, należy to zrobić.
3. Z na komputerze deweloperskim kodem Default.aspx i ustaw punkt przerwania w metodzie _usługa/strony.
4. Uruchom debugowanie z na komputerze deweloperskim.

Należy trafiony punkt przerwania, zgodnie z oczekiwaniami.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Jako weve już omówione Visual Studio 2005 jest dostarczany z serwera sieci Web o nazwie ASP.NET Development Server. (ASP.NET Development Server jest czasami określane jako Cassini.) Ten serwer sieci Web jest wygodny sposób przeglądania i debugowania aplikacji sieci Web uruchomionych w systemie plików.

ASP.NET Development Server jest ograniczony serwera sieci Web. Nie zezwalaj na połączenia zdalne, go nie zezwala na wszystkie żądania z dowolnego konta użytkownika niż użytkownika, który uruchomił serwer sieci Web. Ponadto nie ma możliwości obsługi stron ASP. Obsługiwane są tylko ASP.NET zasobów i HTML (w tym obrazy, pliki CSS, itp.).

ASP.NET Development Server można uruchomić z poziomu wiersza polecenia, uruchamiając plik WebDev.WebServer.exe znajdujący się w c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. Następujące okno dialogowe wyświetla parametry, które są dostępne.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Rysunek 14**


> [!NOTE]
> ASP.NET Development Server nie jest obsługiwana w przypadku uruchomienia jawnie za pomocą wiersza polecenia.
