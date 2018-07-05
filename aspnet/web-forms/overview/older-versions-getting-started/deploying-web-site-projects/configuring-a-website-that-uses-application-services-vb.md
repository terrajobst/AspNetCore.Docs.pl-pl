---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Konfigurowanie witryny sieci Web, która korzysta z usług aplikacji (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Verze technologie ASP.NET 2.0 wprowadzono szereg usług aplikacji, które są częścią programu .NET Framework i będzie służyć jako zestaw bloków konstrukcyjnych usługi, których yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 69c8bb4061c2ed7fcc44038aa3bf0268ac86c9ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387497"
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurowanie witryny sieci Web, która korzysta z usług aplikacji (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> Verze technologie ASP.NET 2.0 wprowadzono szereg usług aplikacji, które są częścią programu .NET Framework i służyć jako zestaw usług bloków konstrukcyjnych, których można użyć, aby dodać zaawansowane funkcje do aplikacji sieci web. W tym samouczku przedstawiono sposób konfigurowania witryny sieci Web w środowisku produkcyjnym w celu korzystania z usług aplikacji i opisano często występujące problemy z zarządzaniem kont użytkowników i ról w środowisku produkcyjnym.


## <a name="introduction"></a>Wprowadzenie

Verze technologie ASP.NET 2.0 wprowadzono szereg *usługi aplikacji*, które są częścią programu .NET Framework i będzie służyć jako zestaw bloków konstrukcyjnych usługi, które służy do dodawania zaawansowane funkcje do aplikacji sieci web. Usługi aplikacji obejmują:

- **Członkostwo** — interfejs API do tworzenia i zarządzania kontami użytkowników.
- **Role** — interfejs API na potrzeby kategoryzowania użytkowników w grupach.
- **Profil** — interfejs API do przechowywania zawartości niestandardowych, specyficzne dla użytkownika.
- **Mapa witryny** — interfejs API do definiowania struktury logicznej lokacji s w postaci hierarchii, które można wyświetlić za pomocą kontrolki, takie jak menu i linki do stron nadrzędnych.
- **Personalizacja** — interfejs API na potrzeby utrzymywania preferencje, najczęściej używana z [ *składników Web Part*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitorowanie kondycji** — interfejs API do monitorowania wydajności, bezpieczeństwa, błędy i inne metryki kondycji systemu dla działającej aplikacji sieci web.
  

Usługi aplikacji interfejsów API nie są związane z konkretnej implementacji. Zamiast tego należy wydać polecenie usługi aplikacji, aby użyć określonego *dostawcy*, i że dostawcy implementuje usługę za pomocą określonej technologii. Dostawcy najczęściej używanych internetowych aplikacji sieci web hostowanych w sieci web firma zapewniająca hosting są dostawców korzystających z implementacją bazy danych programu SQL Server. Na przykład `SqlMembershipProvider` jest dostawcą członkostwa interfejsu API, która przechowuje informacje o koncie użytkownika w bazie danych programu Microsoft SQL Server.

Przy użyciu usług aplikacji i programu SQL Server dostawców dodaje niektóre wyzwania, podczas wdrażania aplikacji. Po pierwsze obiekty bazy danych usług aplikacji muszą być prawidłowo tworzone dla środowisk deweloperskich i produkcyjnych baz danych i prawidłowo zainicjowana. Dostępne są także ważnych ustawień konfiguracyjnych, które należy podjąć.

> [!NOTE]
> Usługi aplikacji, interfejsy API zostały zaprojektowane, za pomocą [ *modelu dostawca*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), szablon projektu, który umożliwia interfejsu API s szczegóły implementacji, które mają być dostarczane w czasie wykonywania. .NET Framework, który jest dostarczany z wielu dostawców usług aplikacji, które mogą być używane, takich jak `SqlMembershipProvider` i `SqlRoleProvider`dostawców o członkostwo, które są i interfejsów API ról, które używają programu SQL Server, bazy danych wdrożenia. Można również tworzyć i wtyczka niestandardowego dostawcy. W rzeczywistości, przeglądy książki aplikacji sieci web już zawiera niestandardowego dostawcy dla interfejsu API mapy witryny (`ReviewSiteMapProvider`), który tworzy mapy witryny z danych w `Genres` i `Books` tabele w bazie danych.


Ten samouczek rozpoczyna się od przyjrzeć się jak mogę rozszerzyć przeglądy książki aplikacji sieci web, członkostwo i role interfejsów API. Następnie przeprowadzi wdrażanie aplikacji sieci web, korzysta z usług aplikacji z implementacją bazy danych programu SQL Server, która stwierdza, odnoszący się typowe problemy związane z zarządzaniem kont użytkowników i rolami w środowisku produkcyjnym.

## <a name="updates-to-the-book-reviews-application"></a>Aktualizacje aplikacji przeglądy książki

W ciągu ostatnich kilku samouczków, z którymi aplikacji sieci web przeglądy książka została zaktualizowana z statycznej witryny sieci Web do aplikacji sieci web opartej na danych, dynamiczne Wykonaj zestaw stron administracyjnych związanych z zarządzaniem gatunki i recenzje. Jednak w tej sekcji Administracja obecnie nie jest chroniony — każdy użytkownik, który zna (lub próby odgadnięcia) adres URL strony administracyjnej można waltz w i tworzyć, edytować lub usuwać przeglądy w naszej witrynie. Typowym sposobem chronić niektóre obszary witryny sieci Web jest implementowanie kont użytkowników, a następnie użyć reguł autoryzacji adresów URL do ograniczania dostępu do niektórych użytkowników lub ról. Dostępne do pobrania za pomocą tego samouczka aplikacji sieci web przeglądy książki obsługuje konta użytkowników i ról. Ma on jeden zdefiniowaną rolę administratora i jest tylko użytkownicy w tej roli mają dostęp do stron administracji.

> [!NOTE]
> Czy mogę ve utworzyć trzy konta użytkowników w aplikacji sieci web przeglądy książki: Scott, Jisun i Alicji. Wszystkie trzy użytkownicy mają tego samego hasła: **hasło!** Scott i Jisun znajdują się w roli administratora, Alicja nie znajduje się. Strony inne niż administracyjne s witryny są nadal dostępne dla użytkowników anonimowych. Oznacza to, że nie trzeba zalogować się do witryny, chyba że chcesz administrować, w którym to przypadku musisz zarejestrować się jako użytkownik w roli administratora.


Przeglądy książki s stronie wzorcowej aplikacji został zaktualizowany do uwzględnienia interfejsu użytkownika różnych użytkowników uwierzytelnionych i anonimowych. Jeśli użytkownik anonimowy odwiedza witryny zauważenia łącze Zaloguj się w prawym górnym rogu. Uwierzytelniony użytkownik zobaczy następujący komunikat, "Witaj ponownie, *username*!" a także łącze do wylogowania. Tam s również strony logowania (`~/Login.aspx`), który zawiera formant logowanie w sieci Web, który zapewnia interfejs użytkownika i logikę w celu uwierzytelniania obiekt odwiedzający. Tylko administratorzy mogą tworzyć nowych kont. (Brak stron do tworzenia i zarządzania kontami użytkowników w `~/Admin` folderu.)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurowanie członkostwa i ról interfejsów API

Przeglądy książki aplikacji sieci web używa członkostwa i ról interfejsów API do obsługi kont użytkowników i grup, tych użytkowników do ról (to znaczy, rola administratora). `SqlMembershipProvider` i `SqlRoleProvider` klasy dostawcy są używane, ponieważ chcemy do przechowywania informacji o kontach i roli w bazie danych programu SQL Server.

> [!NOTE]
> Ten samouczek nie jest przeznaczony do szczegółowej analizy na konfigurowanie aplikacji sieci web do obsługi członkostwa i ról interfejsów API. Dokładne przyjrzeć się te interfejsy API i kroki należy wykonać w celu skonfigurowania witryny sieci Web z nich korzystać, należy przeczytać Moje [ *samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Aby korzystać z usług aplikacji z bazą danych programu SQL Server, należy najpierw dodać obiekty bazy danych używane przez tych dostawców w bazie danych, którego konto użytkownika i przechowywane informacje o rolach. Te obiekty wymagania bazy danych obejmują szereg tabel, widoków i procedur składowanych. Chyba że określono inaczej, `SqlMembershipProvider` i `SqlRoleProvider` klasy dostawców używają bazy danych programu SQL Server Express Edition, o nazwie `ASPNETDB` s aplikacji na terenie `App_Data` folderu; Jeśli baza danych nie istnieje, jest ona tworzona automatycznie z wymaganych obiektów bazy danych przez tych dostawców, w czasie wykonywania.

Możliwe jest, i zazwyczaj idealne rozwiązanie do tworzenia aplikacji usług obiektów bazy danych w tej samej bazy danych, gdzie są przechowywane dane specyficzne dla aplikacji witryny sieci Web s. .NET Framework, który jest dostarczany za pomocą narzędzia o nazwie `aspnet_regsql.exe` instalowane obiekty bazy danych określonej bazy danych. Mam stała się dalej i używać tego narzędzia można dodać te obiekty do `Reviews.mdf` bazy danych w `App_Data` folder (Tworzenie bazy danych,). Zobaczymy, jak używać tego narzędzia w dalszej części tego samouczka, po dodaniu tych obiektów do produkcyjnej bazy danych.

Jeśli dodasz usługami obiekty bazy danych do bazy danych innej niż `ASPNETDB` trzeba będzie dostosować `SqlMembershipProvider` i `SqlRoleProvider` dostawcy klasy konfiguracji, tak aby używały odpowiednią bazą danych. Dostosowywanie dodawania dostawcy członkostwa [  *&lt;członkostwa&gt; elementu* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) w ramach `<system.web>` sekcji `Web.config`; użyj [  *&lt;roleManager&gt; elementu* ](https://msdn.microsoft.com/library/ms164660.aspx) do skonfigurowania dostawcy ról. Poniższy fragment kodu jest pobierana z s aplikacji przeglądy książki `Web.config` oraz konfigurowanie ustawień członkostwa i ról interfejsów API. Należy pamiętać, zarówno zarejestrować nowego dostawcę — `ReviewMembership` i `ReviewRole` -używające `SqlMembershipProvider` i `SqlRoleProvider` dostawców, odpowiednio.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

`Web.config` Pliku s `<authentication>` element także został skonfigurowany do obsługi uwierzytelniania opartego na formularzach.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Ograniczanie dostępu do stron administracji

ASP.NET można łatwo udzielić lub odmówić dostępu do określonego pliku lub folderu przez użytkownika lub roli przy użyciu jego *Autoryzacja adresów URL* funkcji. (Omówiliśmy pokrótce Autoryzacja adresów URL w *podstawowe różnice między usług IIS i programem ASP.NET Development Server* samouczek i pokazano, jak usługi IIS i programem ASP.NET Development Server stosowane reguły autoryzacji adresów URL na statyczne w zależności od zawartości dynamicznej.) Ponieważ chcemy uniemożliwić dostęp do `~/Admin` folderu z wyjątkiem tych użytkowników w roli administratora, należy dodać reguły autoryzacji adresów URL do tego folderu. W szczególności reguł autoryzacji adresów URL należy umożliwić użytkownikom w roli administratora i odmawiać go innym użytkownikom. Jest to osiągane przez dodanie `Web.config` plik `~/Admin` folder o następującej zawartości:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Aby więcej informacji na temat funkcji autoryzacji programu ASP.NET s adresu URL i jak z niej korzystać sprecyzowania reguł autoryzacji dla użytkowników i ról, należy przeczytać [ *autoryzacja na podstawie użytkownika* ](../../older-versions-security/membership/user-based-authorization-vb.md) i [ *Autoryzacji opartej na rolach* ](../../older-versions-security/roles/role-based-authorization-vb.md) samouczki z moich [ *samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Wdrażanie aplikacji sieci Web, która korzysta z usług aplikacji

W przypadku wdrażania witryny sieci Web, która korzysta z usługi aplikacji i dostawcy, która przechowuje informacje o usługach aplikacji w bazie danych, konieczne jest utworzony obiekty bazy danych wymaganych przez usługi aplikacji w produkcyjnej bazie danych. Początkowo w produkcyjnej bazie danych nie zawiera tych obiektów, więc po wdrożeniu aplikacji (lub po jego wdrożeniu po raz pierwszy po dodaniu usługi aplikacji), należy wykonać dodatkowe kroki, aby pobrać te obiekty bazy danych wymaganego produkcyjnej bazy danych.

Inne wyzwanie mogą wystąpić podczas wdrażania witryny sieci Web, która korzysta z usług aplikacji, jeśli zamierzasz replikować kont użytkowników utworzonych w środowisku programistycznym do środowiska produkcyjnego. W zależności od konfiguracji członkostwa i ról jest możliwe, że nawet wtedy, gdy konta użytkowników, które zostały utworzone w środowisku programistycznym do produkcyjnej bazy danych pomyślnie kopiowania, ci użytkownicy nie może zalogować się do aplikacji sieci web w środowisku produkcyjnym. Utworzymy Przyjrzyj się przyczynę tego problemu i omawiać sposób zapobiec sytuacji.

ASP.NET, który jest dostarczany z nieuprzywilejowany [ *narzędzie Administracja witryny sieci Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , można uruchomić z programu Visual Studio i umożliwia użytkownikowi konta, ról i autoryzacji reguł, które mają być zarządzane za pośrednictwem sieci web interfejs. Niestety WSAT działa tylko dla lokalnych witryn sieci Web, co oznacza, że nie można zdalnie zarządzać kontami użytkowników, ról i reguł autoryzacji dla aplikacji sieci web w środowisku produkcyjnym. Omówimy różne sposoby, aby zaimplementować WSAT przypominające zachowanie w produkcyjnej witrynie internetowej.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Dodawanie bazy danych przy użyciu obiektów aspnet\_regsql.exe

*Wdrażania bazy danych* samouczku pokazano, jak kopiowanie tabel i danych z bazy danych rozwoju w produkcyjnej bazie danych, a te techniki bez obaw można kopiować obiekty bazy danych usług aplikacji do produkcyjnej bazy danych. Innym rozwiązaniem jest `aspnet_regsql.exe` narzędzia, które dodaje lub usuwa obiekty bazy danych usług aplikacji z bazy danych.

> [!NOTE]
> `aspnet_regsql.exe` Narzędzie tworzy obiekty baz danych w określonej bazie danych. Nie są migrowane dane w tych obiektach bazy danych z bazy danych rozwoju w produkcyjnej bazie danych. Jeśli chcesz skopiować informacji o kontach i roli użytkownika w bazie danych rozwoju w produkcyjnej bazie danych Użyj technik omówione w *wdrażania bazy danych* samouczka.


Pozwól s, zobacz, jak dodać obiekty z bazy danych do bazy danych produkcyjnych przy użyciu `aspnet_regsql.exe` narzędzia. Uruchom po otwarciu Eksploratora Windows i przejdź do katalogu .NET Framework w wersji 2.0 na komputerze %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Możesz znaleźć `aspnet_regsql.exe` narzędzia. To narzędzie może służyć z wiersza polecenia, ale obejmuje także graficzny interfejs użytkownika; Kliknij dwukrotnie `aspnet_regsql.exe` plików, które można uruchomić jej składowe graficznego.

Uruchamia narzędzie, wyświetlając ekran powitalny wyjaśniające, jego przeznaczenie. Kliknij obok Zaawansowane na ekranie "Wybieranie opcji konfiguracji", który jest pokazany na rysunku 1. W tym miejscu można dodać usługi aplikacji obiektów bazy danych lub usunięcie ich z bazy danych. Ponieważ chcemy dodać te obiekty do produkcyjnej bazy danych, wybierz opcję "Configure SQL Server dla usług aplikacji", a następnie kliknij przycisk Dalej.


[![Wybierz opcję konfigurowania programu SQL Server dla usług aplikacji](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Rysunek 1**: możliwość konfigurowania programu SQL Server dla usług aplikacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


W "Wybierz serwer i baza danych" ekranu monituje o podanie informacji do łączenia z bazą danych. Wprowadź serwera bazy danych, poświadczenia zabezpieczeń i nazwa bazy danych, udostępniane przez firmy hostingu w sieci web, a następnie kliknij przycisk Dalej.

> [!NOTE]
> Po wprowadzeniu serwera bazy danych i poświadczeń może wystąpi błąd podczas rozwijania listy rozwijanej bazy danych. `aspnet_regsql.exe` Narzędzia zapytań `sysdatabases` tabeli systemowej można pobrać listy baz danych na serwerze, ale niektóre hostingu firmy blokowanie ich serwerów baz danych, aby te informacje nie są publicznie dostępne w sieci web. Jeśli ten błąd można wpisać nazwę bazy danych bezpośrednio na liście rozwijanej.


[![Podaj narzędzia z informacjami o połączeniu s bazy danych](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Rysunek 2**: Podaj s Narzędzie za pomocą usługi bazy danych informacji o połączeniu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


Kolejne ekranu zawiera podsumowanie działań, które mają zostać wykonane, a mianowicie który obiekty bazy danych usług aplikacji ma zostać dodany do określonej bazy danych. Kliknij przycisk Dalej, aby ukończyć tę akcję. Po kilku chwilach ekran końcowy jest wyświetlany, biorąc pod uwagę, że obiekty bazy danych zostały dodane (zobacz rysunek 3).


[![SUKCES! Obiekty bazy danych usług aplikacji zostały dodane do produkcyjnej bazy danych](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Rysunek 3**: Sukces! Aplikacja usługi bazy danych obiekty zostały dodane do produkcyjnej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Aby sprawdzić, czy obiekty bazy danych usług aplikacji zostały pomyślnie dodane do produkcyjnej bazy danych, Otwórz program SQL Server Management Studio i połączyć do produkcyjnej bazy danych. Jak pokazano na rysunku 4, powinien zostać wyświetlony tabel bazy danych usług aplikacji w bazie danych, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, i tak dalej.


[![Upewnij się, że obiekty bazy danych zostały dodane do produkcyjnej bazy danych](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Rysunek 4**: Upewnij się, że obiekty bazy danych zostały dodane do produkcyjnej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Tylko wtedy należy użyć `aspnet_regsql.exe` narzędzia podczas wdrażania aplikacji sieci web po raz pierwszy lub po raz pierwszy po uruchomieniu przy użyciu usług aplikacji. Gdy te obiekty bazy danych znajdują się w produkcyjnej bazie danych są wygrał t trzeba być ponownie dodane lub zmodyfikowane.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopiowanie kont użytkowników od projektowania do produkcji

Korzystając z `SqlMembershipProvider` i `SqlRoleProvider` klasy dostawców do przechowywania informacji o usługach aplikacji w bazie danych programu SQL Server, informacji o kontach i roli użytkownika są przechowywane w różnych tabelach bazy danych, w tym `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, i `aspnet_UsersInRoles`, między innymi. Jeśli podczas tworzenia konta zostaną utworzone w środowisku programistycznym można replikować tych kont użytkowników w środowisku produkcyjnym przez skopiowanie odpowiednich rekordów w tabelach odpowiednie bazy danych. Jeśli Kreator publikowania bazy danych jest używane do wdrażania obiektów bazy danych usług aplikacji, mogą również wybrano opcję Kopiuj rekordy, które mogłyby spowodować konta użytkowników utworzone w trakcie opracowywania również znajdować się w środowisku produkcyjnym. Jednak w zależności od ustawień konfiguracji, może się okazać, że tych użytkowników, których konta zostały utworzone w trakcie opracowywania i kopiowane do środowiska produkcyjnego są w stanie się zalogować w produkcyjnej witrynie internetowej. Co zapewnia?

`SqlMembershipProvider` i `SqlRoleProvider` klasy dostawców zostały zaprojektowane w taki sposób, że pojedyncza baza danych może służyć jako magazyn użytkownika, dla wielu aplikacji, w którym każda aplikacja może teoretycznie ma użytkowników z nakładającymi się nazwy użytkowników i role o takiej samej nazwie. Aby umożliwić tej elastyczności, baza danych przechowuje listę aplikacji w `aspnet_Applications` tabeli, a każdy użytkownik jest skojarzony z jedną z tych aplikacji. W szczególności `aspnet_Users` tabela ma `ApplicationId` kolumny, która każdy użytkownik z rekordem w `aspnet_Applications` tabeli.

Oprócz `ApplicationId` kolumnie `aspnet_Applications` tabeli znajdują się także `ApplicationName` kolumny, która zawiera nazwę aplikacji do bardziej przyjaznego dla człowieka. Próba uzyskania witryny sieci Web pracować przy użyciu konta użytkownika, takich jak sprawdzanie poprawności poświadczeń użytkownika s, na stronie logowania, musi ona informować `SqlMembershipProvider` klasy aplikacji, jakie chcesz pracować. Zwykle robi to poprzez dostarczenie nazwę aplikacji i jego wartość pochodzi z konfiguracją dostawcy s w programie `Web.config` — w szczególności za pośrednictwem `applicationName` atrybutu.

Ale co się stanie, jeśli `applicationName` atrybut nie jest określony w `Web.config`? W takim przypadku członkostwa system używa ścieżka katalogu głównego aplikacji jako `applicationName` wartość. Jeśli `applicationName` atrybut nie jest jawnie określona `Web.config`, następnie istnieje możliwość, że środowisko deweloperskie i środowisko produkcyjne Użyj katalog główny aplikacji jest inny i w związku z tym zostanie skojarzona z inną aplikacją nazwy w usługi aplikacji. Sytuacji takich niezgodności, a następnie będzie miał tych użytkowników utworzonych w środowisku deweloperskim `ApplicationId` wartość, która nie jest zgodna z `ApplicationId` wartość w środowisku produkcyjnym. Wynikiem jest tych użytkowników, których t i można się zalogować.

> [!NOTE]
> Jeśli okaże się, w takiej sytuacji — z kontami użytkowników skopiowane do środowiska produkcyjnego niezgodny `ApplicationId` wartość — można napisać zapytanie, aby zaktualizować te nieprawidłowe `ApplicationId` wartości `ApplicationId` używane w środowisku produkcyjnym. Po zaktualizowaniu użytkowników, których konta zostały utworzone w środowisku programistycznym będą teraz mogli logować się do aplikacji sieci web w środowisku produkcyjnym.


Dobra wiadomość jest prostym kroku może podjąć w celu zapewnienia, że dwóch środowisk używać tego samego `ApplicationId` — jawnie ustaw `applicationName` atrybutu w `Web.config` dla wszystkich dostawców usług w Twojej aplikacji. Jawnie ustawić `applicationName` atrybutu "BookReviews" w `<membership>` i `<roleManager>` elementy jako ten fragment kodu z `Web.config` pokazuje.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Więcej dyskusji na temat ustawień dotyczących `applicationName` atrybut wraz z uzasadnieniem, można znaleźć [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) wpis w blogu s [ *zawsze ustawić applicationName właściwości podczas konfigurowania członkostwa ASP.NET i innych dostawców*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Zarządzanie kontami użytkowników w środowisku produkcyjnym

ASP.NET witryny sieci Web Administracja narzędzia (WSAT) ułatwia tworzenie i zarządzanie kontami użytkowników, zdefiniuj stosować i zawierają bardziej reguł autoryzacji użytkowników i opartą na roli. Można uruchomić WSAT z programu Visual Studio, przechodząc do Eksploratora rozwiązań i klikając ikonę konfiguracja platformy ASP.NET lub przechodząc do witryny sieci Web lub projektu w menu i wybierając element menu konfiguracji platformy ASP.NET. Niestety WSAT może pracować tylko z lokalnych witryn sieci Web. W związku z tym nie możesz użyć WSAT ze swojej stacji roboczej do zarządzania witryny sieci Web w środowisku produkcyjnym.

Dobra wiadomość jest wszystkie funkcje udostępniane dostarczone przez WSAT są dostępne programowo za pośrednictwem członkostwa i ról interfejsów API; Ponadto wiele ekranów WSAT używanie standardowych kontrolek usługi związane z logowaniem ASP.NET. Krótko mówiąc można dodać strony ASP.NET do swojej witryny sieci Web, która oferuje funkcje zarządzania wymagane.

Pamiętaj, że wcześniej samouczek zaktualizowane książki przeglądy aplikacji sieci web do uwzględnienia `~/Admin` folder, a ten folder został skonfigurowany do Zezwalaj tylko na użytkowników w roli administratora. Po dodaniu strony do tego folderu o nazwie `CreateAccount.aspx` z którym administrator może utworzyć nowego konta użytkownika. Ta strona używa kontrolki CreateUserWizard, aby wyświetlić logikę interfejsu i zaplecza użytkownika do tworzenia nowego konta użytkownika. Jakie s więcej, mogę dostosować formant aby uwzględnić pole wyboru, który monituje czy również należy dodać nowego użytkownika do roli administratora (zobacz rysunek 5). Za pomocą trochę pracy można utworzyć niestandardowy zestaw stron, który implementuje użytkowników i ról związanych z zarządzaniem zadania, które w przeciwnym razie zostanie dostarczone przez WSAT.

> [!NOTE]
> Aby więcej informacji na temat przy użyciu członkostwa i ról interfejsów API do formantów sieci Web platformy ASP.NET skojarzone z logowaniem, pamiętaj o przeczytaniu Moje [ *samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Aby uzyskać więcej informacji na dostosowywanie formantu CreateUserWizard, zobacz [ *tworzenie kont użytkowników* ](../../older-versions-security/membership/creating-user-accounts-vb.md) i [ *przechowywanie dodatkowych informacji użytkownika* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) samouczków lub wyewidencjonowanie [ *Erich Peterson* ](http://www.erichpeterson.com/) artykułu s [ *Dostosowywanie formantu CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Administratorzy mogą tworzyć nowych kont użytkowników](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Rysunek 5**: Administratorzy mogą tworzyć nowych kont użytkowników ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Jeśli potrzebujesz pełnej funkcjonalności WSAT wyewidencjonowanie [ *stopniowe Twojej własnej witryny sieci Web narzędzie administracyjne*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), w których autor Dan Clem przedstawiono proces tworzenia niestandardowego narzędzia WSAT podobne. DaN udostępnia jego kod źródłowy aplikacji s (w języku C#) i zawiera szczegółowe instrukcje dotyczące dodawania go do witryny sieci Web hostowanej.

## <a name="summary"></a>Podsumowanie

Podczas wdrażania aplikacji sieci web korzystającą z implementacji bazy danych usług aplikacji najpierw upewnij się, że produkcyjnej bazy danych ma obiekty wymagania bazy danych. Te obiekty można dodać za pomocą techniki opisane w *wdrażania bazy danych* samouczek; Alternatywnie, można użyć `aspnet_regsql.exe` narzędzia, ponieważ widzieliśmy w ramach tego samouczka. Inne wyzwania firma Microsoft korzysta z Centrum wokół synchronizowanie nazwę aplikacji używane w środowiskach deweloperskich i produkcyjnych, (które jest ważne, jeśli chcesz, aby użytkownicy i role utworzone w środowisku programistycznym, był prawidłowy w środowisku produkcyjnym) i techniki Zarządzanie użytkownikami i rolami, w środowisku produkcyjnym.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [*ASP.NET SQL Server Registration Tool (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Tworzenie bazy danych z usługi aplikacji dla programu SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Tworzenie schematu członkostwa w programie SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Badanie ASP.NET s członkostwo, role i profilu*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Stopniowe własne narzędzia do administrowania witryną sieci Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Omówienie narzędzia do administrowania witryny sieci Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Poprzednie](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [dalej](strategies-for-database-development-and-deployment-vb.md)
