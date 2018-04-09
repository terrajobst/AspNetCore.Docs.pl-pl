---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: Konfigurowanie witryny sieci Web, która korzysta z usługi aplikacji (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET w wersji 2.0 wprowadzono szereg usług aplikacji, które są częścią programu .NET Framework i służyć jako zestaw modułów konstrukcyjnych usług yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: da4ef328e3461e96fbb0cdca156ce1b9a076748f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-website-that-uses-application-services-c"></a>Konfigurowanie witryny sieci Web, która korzysta z usługi aplikacji (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> ASP.NET w wersji 2.0 wprowadzono szereg usług aplikacji, które są częścią programu .NET Framework i służyć jako zestaw usług bloków konstrukcyjnych, które można dodać zaawansowane funkcje do aplikacji sieci web. W tym samouczku opisuje sposób konfigurowania witryny sieci Web w środowisku produkcyjnym w celu korzystania z aplikacji, usług i opisano często występujące problemy z zarządzaniem kont użytkowników i role w środowisku produkcyjnym.


## <a name="introduction"></a>Wprowadzenie

ASP.NET w wersji 2.0 wprowadzono szereg *usług aplikacji*, które są częścią programu .NET Framework i służyć jako zestaw modułów konstrukcyjnych usług, które umożliwia dodanie rozbudowane funkcje do aplikacji sieci web. Usługi aplikacji obejmują:

- **Członkostwo** — interfejs API do tworzenia i zarządzania kontami użytkowników.
- **Role** — interfejs API do klasyfikacji użytkowników w grupach.
- **Profil** — interfejs API do przechowywania zawartości niestandardowych, specyficzne dla użytkownika.
- **Mapa witryny** — interfejs API do definiowania struktury logicznej lokacji s w postaci hierarchii, które można wyświetlić za pomocą kontrolki, takich jak menu i nawigacji.
- **Personalizacja** — interfejs API do obsługi preferencje, najczęściej używana z [ *składników Web Part*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitorowanie kondycji** — interfejs API do monitorowania wydajności, zabezpieczeń, błędy i innych metryk kondycji systemu dla działającej aplikacji sieci web.
  

Interfejsy API usług aplikacji nie są związane z konkretnej implementacji. Zamiast tego należy poinstruować usług aplikacji, aby użyć określonego *dostawcy*, i usługi za pomocą technologii określonego implementuje tego dostawcy. Najczęściej używane dostawców dla internetowego aplikacji sieci web hostowanej w lokalizacji hostingu firmy są dostawców używających implementacji bazy danych programu SQL Server. Na przykład `SqlMembershipProvider` jest dostawcą członkostwa interfejsu API, która przechowuje informacje o koncie użytkownika w bazie danych programu Microsoft SQL Server.

Przy użyciu dostawcy programu SQL Server i usług aplikacji dodaje niektóre wyzwania, podczas wdrażania aplikacji. Po pierwsze obiekty bazy danych usług aplikacji należy właściwie utworzone w rozwoju i produkcyjnych bazach danych i prawidłowo zainicjowane. Brak ważnych ustawień konfiguracji, które należy wprowadzić.

> [!NOTE]
> Interfejsy API zostały zaprojektowane przy użyciu usług aplikacji [ *modelu dostawcy*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), wzorzec projektowania, który pozwala na interfejs API s szczegółów implementacji w czasie wykonywania. .NET Framework jest dostarczany z dostawców usług aplikacji, które mogą być używane, takich jak `SqlMembershipProvider` i `SqlRoleProvider`, dostawcy dla członkostwa, które są i interfejsów API ról, które korzystają z serwera SQL bazy danych wdrażania. Można również tworzyć i wtyczki niestandardowego dostawcy. W rzeczywistości przeglądami książki aplikacji sieci web już zawiera niestandardowego dostawcy dla interfejsu API mapy witryny (`ReviewSiteMapProvider`), który konstruuje mapy witryny z danych w `Genres` i `Books` tabele w bazie danych.


W tym samouczku rozpoczyna się od przyjrzeć się jak mogę rozszerzyć przeglądami książki aplikacji sieci web do użycia, członkostwo i role interfejsów API. Następnie przedstawiono wdrażanie aplikacji sieci web, która korzysta z usługi aplikacji z implementacją bazy danych programu SQL Server i nie zawiera umożliwi Udokumentowanie typowych problemów z zarządzaniem kont użytkowników i role w środowisku produkcyjnym.

## <a name="updates-to-the-book-reviews-application"></a>Aktualizacje aplikacji przeglądami książki

W ciągu ostatnich kilku samouczków, z którymi aplikacja sieci web sprawdza książki została zaktualizowana z statycznej witryny sieci Web do aplikacji sieci web dynamicznych, opartych na danych wykonaj z zestawem stron administracyjnych związanych z zarządzaniem genres i recenzje. Jednak w tej sekcji administracyjnej nie jest zabezpieczony — każdy użytkownik, który zna (lub liczba prób) adres URL strony Administracja można waltz w i tworzyć, edytować lub usunąć przeglądy w naszej witrynie. Jest to często stosowana metoda ochrony niektórych części witryny sieci Web do implementowania kont użytkowników, a następnie użyć reguł autoryzacji adresów URL, aby ograniczyć dostęp do określonych użytkowników lub ról. Dostępny do pobrania z tego samouczka aplikacji sieci web przeglądami książki obsługuje kont użytkowników i ról. Ma on jeden definicja roli o nazwie administratora i tylko użytkownicy w tej roli mają dostęp do stron administracyjnych.

> [!NOTE]
> I Zapisz utworzone trzy konta użytkowników w aplikacji sieci web przeglądami książki: Scott Jisun i Alicji. Wszystkie trzy użytkownicy mają tego samego hasła: **hasło!** Scott i Jisun znajdują się w roli administratora, Alicja nie jest. Na stronach witryny s-administration są nadal dostępne dla użytkowników anonimowych. Oznacza to, że nie trzeba zalogować się do witryny, chyba że chcesz administrować, w którym to przypadku należy zalogować się jako użytkownik w roli administratora.


Aby dołączyć inny interfejs dla użytkowników uwierzytelnionych i anonimowych została zaktualizowana strony wzorcowej s przeglądami książki aplikacji. Jeśli użytkownik anonimowy odwiedza witrynę widzi łącze logowania w prawym górnym rogu. Uwierzytelniony użytkownik zobaczy następujący komunikat, "Witamy z powrotem, *username*!" i link do wylogowania. Dostępne są również strony logowania s (`~/Login.aspx`), zawierający formant sieci Web logowania, który udostępnia logiki i interfejsu użytkownika dla uwierzytelniania obiekt odwiedzający. Tylko administratorzy mogą tworzyć nowych kont. (Brak stron do tworzenia i zarządzania kontami użytkowników w `~/Admin` folderu.)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurowanie członkostwa i ról interfejsów API

Przeglądy książki aplikacji sieci web używa członkostwo i role interfejsów API do obsługi kont użytkowników i grup użytkowników do ról (to znaczy, rola administratora). `SqlMembershipProvider` i `SqlRoleProvider` klasy dostawcy są używane, ponieważ chcemy do przechowywania informacji o kontach i roli w bazie danych programu SQL Server.

> [!NOTE]
> W tym samouczku nie mają być szczegółowej analizy na konfigurowanie aplikacji sieci web do obsługi członkostwa i ról interfejsów API. Dokładne przyjrzeć się te interfejsy API i kroki należy wykonać w celu skonfigurowania witryny sieci Web z nich korzystać, przeczytaj Mój [ *samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Do korzystania z usługi aplikacji z bazą danych programu SQL Server, należy najpierw dodać obiekty bazy danych używane przez tych dostawców do bazy danych, w którym ma konto użytkownika i przechowywane informacje o rolach. Te obiekty bazy danych wymagane obejmują szereg tabel, widoków i procedur składowanych. O ile nie określono inaczej, `SqlMembershipProvider` i `SqlRoleProvider` klasy dostawców korzystania z bazy danych programu SQL Server Express Edition o nazwie `ASPNETDB` znajduje się w aplikacji s `App_Data` folderu; Jeśli baza danych nie istnieje, jest tworzony automatycznie z wymaganych obiektów bazy danych przez tych dostawców w czasie wykonywania.

Możliwe jest, i zazwyczaj jest to idealne utworzyć aplikację usługi obiektów bazy danych w tej samej bazy danych, których są przechowywane dane specyficzne dla aplikacji s witryny sieci Web. .NET Framework jest dostarczany z narzędziem o nazwie `aspnet_regsql.exe` instalujące obiektów bazy danych określonej bazy danych. Mam usunięty z wyprzedzeniem i użyć tego narzędzia, aby dodać te obiekty do `Reviews.mdf` bazy danych w `App_Data` folder (projektowej bazie danych). Firma Microsoft będzie Zobacz, jak to narzędzie później w tym samouczku przy wdrażaniu tych obiektów w produkcyjnej bazie danych.

Jeśli dodasz usługami obiektów bazy danych do bazy danych innej niż `ASPNETDB` trzeba będzie dostosować `SqlMembershipProvider` i `SqlRoleProvider` dostawcy klas konfiguracji, tak aby używały odpowiednią bazę danych. Aby dostosować dodać dostawcę członkostwa [  *&lt;członkostwa&gt; elementu* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) w `<system.web>` sekcji `Web.config`; użyj [  *&lt;roleManager&gt; elementu* ](https://msdn.microsoft.com/library/ms164660.aspx) do skonfigurowania dostawcy ról. Poniższy fragment jest pobierana z s aplikacji przeglądami książki `Web.config` oraz konfigurowanie ustawień członkostwa i ról interfejsów API. Należy pamiętać, że oba zarejestrować nowego dostawcę - `ReviewMembership` i `ReviewRole` -używające `SqlMembershipProvider` i `SqlRoleProvider` dostawców, odpowiednio.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

`Web.config` Pliku s `<authentication>` element także został skonfigurowany do obsługi uwierzytelniania opartego na formularzach.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Ograniczanie dostępu do stron administracyjnych

ASP.NET ułatwia Udziel lub Odmów dostępu do określonego pliku lub folderu przez użytkownika lub roli przy użyciu jego *Autoryzacja adresów URL* funkcji. (Omówiono pokrótce Autoryzacja adresów URL w *podstawowe różnice między usług IIS i ASP.NET Development Server* samouczka i pokazano, jak usług IIS i ASP.NET Development Server stosowane reguł autoryzacji adresów URL na statyczne w zależności od zawartości dynamicznej.) Ponieważ chcemy, aby zabronić dostępu do `~/Admin` folderu z wyjątkiem tych użytkowników w roli administratora, należy dodać reguły autoryzacji adresów URL do tego folderu. W szczególności reguł autoryzacji adresów URL, należy zezwolić użytkownikom w roli administratora i odmawiać go innym użytkownikom. Jest to osiągane przez dodanie `Web.config` pliku `~/Admin` folder z następującą zawartość:

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

Dla więcej informacji o funkcji autoryzacji adresu URL s ASP.NET i jak z niego korzystać do pełnych reguł autoryzacji dla użytkowników i ról, należy przeczytać [ *autoryzacji na podstawie użytkownika* ](../../older-versions-security/membership/user-based-authorization-cs.md) i [ *Autoryzacji opartej na rolach* ](../../older-versions-security/roles/role-based-authorization-cs.md) samouczki z mojej [ *samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Wdrażanie aplikacji sieci Web, która korzysta z usługi aplikacji

W przypadku wdrażania witryny sieci Web, który korzysta z usługi aplikacji i dostawcy, która przechowuje informacje o usługach aplikacji w bazie danych, konieczne jest utworzony w produkcyjnej bazie danych obiektów bazy danych wymaganych przez usługi aplikacji. Początkowo w produkcyjnej bazie danych nie zawiera tych obiektów, to podczas wdrażania aplikacji (lub po jej wdrożeniu po raz pierwszy po dodaniu usługi aplikacji), należy wykonać dodatkowe kroki, aby pobrać te obiekty wymagania bazy danych w produkcyjnej bazie danych.

Inne wyzwanie mogą wystąpić podczas wdrażania witryny sieci Web, który korzysta z usług aplikacji, jeśli chcesz replikować kont użytkowników utworzonych w środowisku programistycznym do środowiska produkcyjnego. W zależności od konfiguracji członkostwo i role istnieje możliwość, że nawet jeśli kopiujesz pomyślnie kont użytkowników, które zostały utworzone w środowisku programistycznym w produkcyjnej bazie danych, ci użytkownicy nie może zalogować się do aplikacji sieci web w środowisku produkcyjnym. Firma Microsoft będzie Spójrz na przyczyny tego problemu i omówiono sposób zapobiec go.

Program ASP.NET jest dostarczany z nieuprzywilejowany [ *narzędzie do administrowania witryny sieci Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) który można uruchomić z programu Visual Studio i umożliwia użytkownikowi konta, ról i autoryzacji reguł, które mają być zarządzane za pośrednictwem sieci web interfejs. Niestety WSAT działa tylko dla lokalnych witryn sieci Web, co oznacza, że nie może służyć do zdalnego zarządzania kontami użytkowników, ról i reguł autoryzacji dla aplikacji sieci web w środowisku produkcyjnym. Przyjrzymy implementuje zachowanie WSAT przypominającej z witryny sieci Web produkcji na różne sposoby.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Dodawanie przy użyciu obiektów bazy danych aspnet\_regsql.exe

*Wdrażania bazy danych* samouczku przedstawiono sposób kopiowania tabele i danych z bazy danych Programowanie w produkcyjnej bazie danych, a te techniki na pewno może służyć do kopiowania obiektów bazy danych usług aplikacji do w produkcyjnej bazie danych. Innym rozwiązaniem jest `aspnet_regsql.exe` narzędzia, które dodaje lub usuwa z bazy danych obiektów bazy danych usług aplikacji.

> [!NOTE]
> `aspnet_regsql.exe` Narzędzie tworzy obiekty baz danych na określonej bazy danych. Nie są migrowane dane w tych obiektach bazy danych z bazy danych Programowanie w produkcyjnej bazie danych. Jeśli chcesz skopiować informacji o kontach i roli użytkownika w bazie danych Programowanie w produkcyjnej bazie danych korzystać z metod opisanych w *wdrażania bazy danych* samouczka.


Let s, zobacz, jak dodać obiekty bazy danych przy użyciu bazy danych produkcyjnej `aspnet_regsql.exe` narzędzia. Rozpocznij od otwierając Eksploratora Windows i przechodząc do katalogu .NET Framework w wersji 2.0 na komputerze %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Możesz znaleźć `aspnet_regsql.exe` narzędzia. Można to narzędzie z wiersza polecenia, ale zawiera także graficznego interfejsu użytkownika; Kliknij dwukrotnie `aspnet_regsql.exe` plików można uruchomić jej graficznego składnika.

Uruchamia narzędzie za pomocą wyświetlania ekranu powitalnego, wyjaśniający jej celem. Kliknij przycisk Dalej przejście do ekranu "Wybierz opcję instalacji", co przedstawiono na rysunku 1. W tym miejscu można dodać usługi aplikacji obiekty bazy danych lub usuń je z bazy danych. Ponieważ chcemy dodać te obiekty w produkcyjnej bazie danych, wybierz opcję "Konfiguruj SQL Server dla usług aplikacji", a następnie kliknij przycisk Dalej.


[![Wybierz konfigurowania programu SQL Server dla usług aplikacji](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**Rysunek 1**: Wybierz do konfigurowania programu SQL Server dla usług aplikacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


W "Wybierz serwer i baza danych" ekranu monituje o podanie informacji do połączenia z bazą danych. Wprowadź serwer bazy danych, poświadczenia zabezpieczeń i nazwę bazy danych, udostępniane przez firmy hostingu w sieci web, a następnie kliknij przycisk Dalej.

> [!NOTE]
> Po wprowadzeniu serwera bazy danych i poświadczeń może uzyskać wystąpił błąd podczas rozwijania listy rozwijanej bazy danych. `aspnet_regsql.exe` Narzędzia kwerendy `sysdatabases` tabeli systemowej można pobrać listy baz danych na serwerze, ale niektóre hostingu przedsiębiorstwa blokowanie ich serwery baz danych, aby te informacje nie są publicznie dostępne w sieci web. Jeśli ten błąd możesz wpisać nazwę bazy danych bezpośrednio na liście rozwijanej.


[![Podaj narzędzia z informacjami o połączeniu s bazy danych](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**Rysunek 2**: Podaj s narzędzie z Twojej bazy danych informacji o połączeniu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


Kolejne ekranu znajduje się podsumowanie akcje, które mają mieć to miejsce, które obiekty bazy danych usług aplikacji są mają zostać dodane do określonej bazy danych. Kliknij przycisk Dalej, aby ukończyć tę akcję. Po kilku chwilach końcowego ekran jest wyświetlany, biorąc pod uwagę, że obiekty bazy danych zostały dodane (patrz rysunek 3).


[![Powodzenie! Obiekty bazy danych usług aplikacji zostały dodane do produkcyjnej bazy danych](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**Rysunek 3**: sukcesu! Aplikacja usługi bazy danych obiekty zostały dodane w produkcyjnej bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


Aby sprawdzić, czy obiekty bazy danych usług aplikacji zostały pomyślnie dodane do produkcyjnej bazy danych, Otwórz program SQL Server Management Studio i połączenia z bazą danych w środowisku produkcyjnym. Jak pokazano na rysunku 4, powinien zostać wyświetlony tabel bazy danych usług aplikacji, w bazie danych, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, itd.


[![Upewnij się, że obiekty bazy danych zostały dodane w produkcyjnej bazie danych](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**Rysunek 4**: Upewnij się, że obiekty bazy danych zostały dodane do produkcyjnej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


Tylko musisz użyć `aspnet_regsql.exe` narzędzia w przypadku wdrażania aplikacji sieci web po raz pierwszy lub po raz pierwszy po rozpoczęciu korzystania z usług aplikacji. Po zatwierdzeniu te obiekty bazy danych w bazie danych produkcyjnych one won t trzeba ponownie dodane lub zmodyfikowane.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopiowanie konta użytkowników od projektowania do produkcji

Korzystając z `SqlMembershipProvider` i `SqlRoleProvider` klasy dostawców do przechowywania informacji o usługach aplikacji w bazie danych programu SQL Server, informacji o kontach i roli użytkownika jest przechowywane w różnych tabel bazy danych, łącznie z `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, i `aspnet_UsersInRoles`, między innymi. Podczas tworzenia kont użytkowników w przypadku utworzenia w środowisku programistycznym może replikować tych kont użytkowników w środowisku produkcyjnym przez skopiowanie odpowiednie rekordy z tabel odpowiednie bazy danych. Jeśli Kreator publikowania bazy danych jest używane do wdrażania obiektów bazy danych usług aplikacji może również wybrano kopiowanie rekordów, co spowodowałoby kont użytkowników utworzone w rozwoju się również w środowisku produkcyjnym. Jednak w zależności od ustawień konfiguracji może się okazać, czy nie można zalogować się w witrynie produkcji tych użytkowników, których konta zostały utworzone w rozwoju i skopiowane do środowiska produkcyjnego. Co daje?

`SqlMembershipProvider` i `SqlRoleProvider` klasy dostawców zostały zaprojektowane w taki sposób, że pojedyncza baza danych może służyć jako magazynu użytkowników dla wielu aplikacji, w którym każda aplikacja może teoretycznie mają użytkownicy z nakładającą się nazwami użytkowników i role o takiej samej nazwie. Aby zezwolić na tego rodzaju elastyczności, bazy danych przechowuje listę aplikacji w `aspnet_Applications` tabeli, a każdy użytkownik jest skojarzony z jedną z tych aplikacji. W szczególności `aspnet_Users` tabela ma `ApplicationId` kolumny, która wiąże każdy użytkownik z rekordem w `aspnet_Applications` tabeli.

Oprócz `ApplicationId` kolumny, `aspnet_Applications` tabeli znajdują się także `ApplicationName` kolumny, która zapewnia bardziej człowieka przyjazna nazwa dla aplikacji. Próba uzyskania witryny sieci Web pracować przy użyciu konta użytkownika, takich jak sprawdzanie poprawności poświadczeń użytkownika s, na stronie logowania, musi ona informować `SqlMembershipProvider` jakie aplikacji do pracy z klasy. Zazwyczaj jest to przez podanie nazwy aplikacji i jego wartość pochodzi z konfiguracji dostawcy s w `Web.config` — w szczególności za pośrednictwem `applicationName` atrybutu.

Ale co się stanie, jeśli `applicationName` atrybut nie jest określony w `Web.config`? W takim przypadku członkostwo system używa ścieżki katalogu głównego aplikacji jako `applicationName` wartość. Jeśli `applicationName` atrybut nie jest jawnie ustawiona `Web.config`, następnie, istnieje możliwość Środowisko deweloperskie i środowiska produkcyjnego używają głównych różnych aplikacji i w związku z tym zostanie skojarzona z inną aplikację nazwy w usługach aplikacji. W przypadku takich niezgodność tych użytkowników utworzone w środowisku programistycznym będzie mieć `ApplicationId` wartość, która nie jest zgodny z `ApplicationId` wartość w środowisku produkcyjnym. Wynikiem jest zalogowanie się użytkowników won t.

> [!NOTE]
> Jeśli okaże się, że w tej sytuacji — z kontami użytkowników skopiowane do środowiska produkcyjnego niedopasowanych `ApplicationId` wartość — można napisać zapytanie, aby zaktualizować te nieprawidłowe `ApplicationId` wartości do `ApplicationId` używane w środowisku produkcyjnym. Po zaktualizowaniu użytkowników, których konta zostały utworzone w środowisku programistycznym teraz będzie mógł zalogować się do aplikacji sieci web w środowisku produkcyjnym.


Dobre wieści jest, że jest prosty krok należy wykonać, aby upewnić się, że dwóch środowisk używać tego samego `ApplicationId` — jawnie ustawione `applicationName` atrybutu w `Web.config` dla wszystkich dostawców usług aplikacji. Jawnie ustawiona `applicationName` atrybutu "BookReviews" w `<membership>` i `<roleManager>` elementy, co ta Wstawka kodu z `Web.config` zawiera.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

Aby poznać ustawienie `applicationName` atrybut i uzasadnienie, zapoznaj się [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s wpis w blogu, [ *zawsze ustawić właściwość applicationName Właściwość podczas konfigurowania członkostwa ASP.NET i innych dostawców*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Zarządzanie kontami użytkowników w środowisku produkcyjnym

Narzędzia administracji witryny sieci Web platformy ASP.NET (WSAT) ułatwia tworzenie i zarządzanie kontami użytkowników, zdefiniuj zastosować ról i pełnych reguły autoryzacji na podstawie użytkowników i ról. Możesz uruchomić WSAT z programu Visual Studio, przechodząc do Eksploratora rozwiązań i klikając ikonę Konfiguracja programu ASP.NET lub przechodząc do witryny sieci Web lub projektu w menu i wybraniu elementu menu konfiguracja platformy ASP.NET. Niestety WSAT może pracować tylko z lokalnej witryny sieci Web. W związku z tym nie można użyć WSAT ze stacji roboczej do zarządzania witryny sieci Web w środowisku produkcyjnym.

Dobre wieści jest wszystkie funkcje udostępniane podał WSAT jest dostępnych programowo za pośrednictwem członkostwa i ról interfejsów API; Ponadto wiele ekranów WSAT używanie standardowych kontrolek związane z logowaniem ASP.NET. Krótko mówiąc można dodać strony ASP.NET do witryny sieci Web oferuje możliwości zarządzania niezbędne.

Odwołaj, że samouczek wcześniejszych zaktualizowany przeglądami książki aplikacji sieci web obejmują `~/Admin` folderu, a ten folder został skonfigurowany, aby umożliwić tylko przez użytkowników w roli administratora. Po dodaniu strony do tego folderu o nazwie `CreateAccount.aspx` z którym administrator może utworzyć nowego konta użytkownika. Ta strona używa formancie CreateUserWizard do wyświetlenia logikę interfejsu i wewnętrznej bazy danych użytkownika do tworzenia nowego konta użytkownika. Jakie więcej, s I dostosowane formantu ma zawierać pola wyboru, które monituje czy również należy dodać nowego użytkownika do roli administratora (patrz rysunek 5). Z niewielki pracy można utworzyć niestandardowy zestaw stron implementujący użytkowników i ról związanych z zarządzaniem zadań, które w przeciwnym razie zostanie dostarczone przez WSAT.

> [!NOTE]
> Dla więcej informacji na temat przy użyciu członkostwa i ról interfejsów API oraz kontroli związane z logowaniem sieci Web ASP.NET, należy przeczytać Mój [ *samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Aby uzyskać więcej informacji na temat dostosowywania formancie CreateUserWizard zobacz [ *tworzenia kont użytkowników* ](../../older-versions-security/membership/creating-user-accounts-cs.md) i [ *przechowywania dodatkowych informacji użytkownika* ](../../older-versions-security/membership/storing-additional-user-information-cs.md) samouczki lub wyewidencjonowanie [ *Erich Peterson* ](http://www.erichpeterson.com/) artykułu s [ *Dostosowywanie formancie CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Administratorzy mogą tworzyć nowych kont użytkowników](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**Rysunek 5**: Administratorzy mogą tworzyć nowych kont użytkowników ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


Jeśli potrzebujesz pełną funkcjonalnością WSAT wyewidencjonowanie [ *stopniowych swój własny sieci Web narzędzie do administrowania witryną*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), w których autor Dan Clem przeprowadzi Cię przez proces tworzenia niestandardowego narzędzia przypominającej WSAT. DaN udziały jego s kodu aplikacji (w języku C#) i instrukcje krok po kroku dodanie go do witryny sieci Web hostowanej.

## <a name="summary"></a>Podsumowanie

W przypadku wdrażania aplikacji sieci web, która używa implementacja bazy danych usług aplikacji musi najpierw sprawdź, czy w produkcyjnej bazie danych ma obiektów wymagania bazy danych. Te obiekty można dodać przy użyciu techniki opisane w *wdrażania bazy danych* samouczek; Alternatywnie, można zastosować `aspnet_regsql.exe` narzędzia, ponieważ widzieliśmy w tym samouczku. Inne problemy, firma Microsoft dotknięciu Centrum wokół synchronizowanie nazwę aplikacji, używane w środowisk projektowania i produkcji, (co jest ważne, jeśli użytkownicy i role utworzone w środowisku programistycznym, jest nieprawidłowy w środowisku produkcyjnym) i metody Zarządzanie użytkownikami i rolami w środowisku produkcyjnym.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [*ASP.NET SQL Server Registration Tool (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Tworzenie bazy danych usług aplikacji dla programu SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Tworzenie schematu członkostwa w programie SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*Badanie członkostwa ASP.NET s, ról i profilu*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Wycofanie własne narzędzia administrowania witryną sieci Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Samouczki dotyczące zabezpieczeń witryny sieci Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Omówienie narzędzia do administrowania witryny sieci Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Poprzednie](configuring-the-production-web-application-to-use-the-production-database-cs.md)
> [dalej](strategies-for-database-development-and-deployment-cs.md)
