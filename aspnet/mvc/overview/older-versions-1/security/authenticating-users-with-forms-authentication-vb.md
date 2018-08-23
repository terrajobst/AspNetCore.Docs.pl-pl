---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Uwierzytelnianie użytkowników za pomocą formularzy uwierzytelniania (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak używać atrybutu [Authorize] do hasła ochrony konkretnych stron w aplikacji MVC. Dowiesz się, jak Administracja witryny sieci Web za pomocą...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: af91ae24cae505125dc237adfaa11b0ea4d60922
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753873"
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Uwierzytelnianie użytkowników za pomocą uwierzytelniania formularzy (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak używać atrybutu [Authorize] do hasła ochrony konkretnych stron w aplikacji MVC. Dowiesz się, jak używać narzędzia do administrowania witryną sieci Web do tworzenia i zarządzania użytkownikami i rolami. Poznasz również sposób konfigurowania przechowywania informacji o kontach i roli użytkownika.


Celem tego samouczka jest wyjaśniają, jak można użyć formy uwierzytelniania do hasła ochrony widoków w aplikacjach ASP.NET MVC. Dowiesz się, jak używać narzędzia do administrowania witryną sieci Web do tworzenia użytkowników i ról. Poznasz również sposób zapobiegania nieautoryzowanemu dostępowi wywołanie akcji kontrolera. Na koniec dowiesz się, jak skonfigurować, gdzie są przechowywane nazwy użytkowników i hasła.

#### <a name="using-the-web-site-administration-tool"></a>Za pomocą narzędzia do administrowania witryną sieci Web

Zanim przejdziemy czymkolwiek, powinien Zaczniemy, tworząc niektórych użytkowników i ról. Najprostszym sposobem utworzenia nowych użytkowników i ról jest korzystanie z zalet narzędzie Administratorskie witryny sieci Web 2008 w usłudze Visual Studio. To narzędzie można uruchomić, wybierając opcję menu **projektu, konfiguracja ASP.NET**. Alternatywnie, możesz uruchomić narzędzie Administratorskie witryny sieci Web, klikając ikonę (nieco scary) z młot osiągnięcia w świecie, która pojawia się w górnej części okna Eksploratora rozwiązań (patrz rysunek 1).

**Rysunek 1 — Uruchamianie narzędzia do administrowania witryną sieci Web**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

W ramach narzędzia do administrowania witryną sieci Web należy utworzyć nowych użytkowników i ról, wybierając kartę zabezpieczeń. Kliknij przycisk **Utwórz użytkownika** link, aby utworzyć nowego użytkownika o nazwie Autor: Stephen (patrz rysunek 2). Podaj użytkownika Autor: Stephen dowolnym hasłem, który ma (na przykład *klucz tajny*).

**Rysunek 2 — Tworzenie nowego użytkownika**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Możesz utworzyć nowe role pierwszy włączając ról i definiując co najmniej jedną rolę. Povolit role, klikając **Włącz role** łącza. Następnie Utwórz rolę o nazwie *Administratorzy* , klikając **tworzyć ani nimi zarządzać rolami** link (zobacz rysunek 3).

**Rysunek 3 — Tworzenie nowej roli**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Na koniec Utwórz nowego użytkownika o nazwie Zosi i skojarz Zosi z rolą Administratorzy, klikając link Create User i wybierając Administratorzy, tworząc Zosi (zobacz rysunek 4).

**Rysunek 4 — Dodawanie użytkownika do roli**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Gdy wszystkie jest nazywany i gotowe, powinny mieć dwóch nowych użytkowników o nazwie Autor: Stephen i Zosi. Należy również nową rolę o nazwie Administratorzy. Zosi jest elementem członkowskim roli Administratorzy programu i Autor: Stephen nie.

#### <a name="requiring-authorization"></a>Wymaganie autoryzacji

Może wymagać uwierzytelnienia użytkownik wywoła akcji kontrolera, dodając atrybut [Authorize] do akcji użytkownika. Można zastosować atrybutu [Authorize] do akcji kontrolera poszczególnych lub ten atrybut można zastosować do klasy całego kontrolera.

Na przykład kontroler w ofercie 1 udostępnia akcję o nazwie CompanySecrets(). Ponieważ ta akcja zostanie nadany atrybut [Authorize], nie można wywołać tej akcji, chyba że użytkownik jest uwierzytelniany.

**Wyświetlanie listy 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Jeśli możesz wywołać akcję CompanySecrets(), wprowadzając adres URL /Home/CompanySecrets na pasku adresu przeglądarki i nie jesteś uwierzytelniony użytkownik, a następnie nastąpi przekierowanie do widoku logowania automatycznie (patrz rysunek 5).

**Rysunek 5 — widok logowania**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Widok logowania umożliwia wprowadź nazwę użytkownika i hasło. Jeśli nie jesteś zarejestrowanym użytkownikiem, a następnie kliknięcie **zarejestrować** link, aby przejść do kasy wyświetlenia (patrz rysunek 6). Widok rejestru można użyć do utworzenia nowego konta użytkownika.

**Rysunek 6 — widok rejestru**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Po pomyślnym zalogowaniu zobaczysz CompanySecrets wyświetlenia (patrz rysunek 7). Domyślnie będziesz się zalogować, dopóki nie zamkniesz okna przeglądarki.

**Rysunek 7 — widok CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Uwierzytelnianie przy użyciu nazwy użytkownika lub roli użytkownika

Można użyć atrybutu [Authorize] do ograniczania dostępu do akcji kontrolera do określonego zestawu użytkowników lub określonego zestawu ról użytkownika. Na przykład zmodyfikowane kontrolera głównego w ofercie 2 zawiera dwie nowe akcje o nazwie StephenSecrets() i AdministratorSecrets().

**Wyświetlanie listy 2 — Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Tylko użytkownik z nazwą użytkownika Autor: Stephen wywołać akcję StephenSecrets(). Wszyscy pozostali użytkownicy uzyskać przekierowanie do widoku logowania. Właściwości użytkowników akceptuje listę rozdzielonych przecinkami nazw kont użytkowników.

Tylko użytkownicy należący do roli Administratorzy programu mogą wywołać akcję AdministratorSecrets(). Na przykład ponieważ Zosi jest członkiem grupy Administratorzy, ona wywołać akcję AdministratorSecrets(). Wszyscy pozostali użytkownicy uzyskać przekierowanie do widoku logowania. Właściwości ról akceptuje rozdzielana przecinkami lista nazw ról.

#### <a name="configuring-authentication"></a>Konfigurowanie uwierzytelniania

W tym momencie możesz się zastanawiać gdzie są przechowywane informacji o kontach i roli użytkownika. Domyślnie, informacje są przechowywane w bazie danych (RANU) programu SQL Express nazwane ASPNETDB.mdf znajduje się w aplikacji MVC App\_folderu danych. Ta baza danych jest generowany przez platformę ASP.NET automatycznie po uruchomieniu przy użyciu członkostwa.

Aby ASPNETDB.mdf baza danych zostanie wyświetlona w oknie Eksploratora rozwiązań, należy najpierw wybierz opcję menu projektu, Pokaż wszystkie pliki.

Przy użyciu bazy danych programu SQL Express domyślny jest dobrym rozwiązaniem, podczas tworzenia aplikacji. Najprawdopodobniej jednak nie chcesz użyć ASPNETDB.mdf domyślnej bazy danych dla aplikacji produkcyjnych. W takim przypadku można zmienić sposób przechowywania informacji o koncie użytkownika, wykonując następujące dwa kroki:

1. Dodaj obiekty bazy danych usługi aplikacji do produkcyjnej bazy danych — zmienić parametry połączenia aplikacji wskaż produkcyjnej bazy danych

Pierwszym krokiem jest dodanie wszystkie niezbędne obiekty bazy danych (tabel i procedur składowanych) do produkcyjnej bazy danych. Jest najprostszym sposobem dodania tych obiektów do nowej bazy danych, aby móc korzystać z Kreatora instalacji serwera SQL platformy ASP.NET (zobacz rysunek 8). To narzędzie można uruchomić, otwierając Visual Studio 2008 Command Prompt z grupy programów Microsoft Visual Studio 2008 i wykonując następujące polecenie w wierszu polecenia:

aspnet\_regsql

**Rysunek 8 — Kreator konfiguracji serwera SQL platformy ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

Kreator instalacji serwera SQL programu ASP.NET umożliwia wybierz bazę danych programu SQL Server w sieci i zainstaluj wszystkie obiekty bazy danych wymaganych przez usługi aplikacji ASP.NET. Serwer bazy danych nie jest wymagany znajdować się na komputerze lokalnym.

> [!NOTE]
> Jeśli nie chcesz używać Kreatora instalacji serwera SQL programu ASP.NET, można znaleźć skrypty SQL, dodawania obiektów bazy danych usług aplikacji w następującym folderze:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


Po utworzeniu niezbędne obiekty bazy danych, musisz zmodyfikować połączenie z bazą danych używanych przez aplikację MVC. Zmodyfikuj ApplicationServices parametry połączenia w pliku konfiguracji (web.config) w sieci web, które wskazuje w produkcyjnej bazie danych. Na przykład połączenie zmodyfikowane w ofercie 3 wskazuje bazę danych o nazwie MyProductionDB (oryginalny ciąg połączenia ApplicationServices została ujęta w poziomie).

**Wyświetlanie listy 3 — pliku Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurowanie uprawnień do bazy danych

Jeśli używasz zintegrowanych zabezpieczeń do połączenia z bazą danych należy dodać prawidłowe konto użytkownika Windows jako identyfikatora logowania do bazy danych. Prawidłowe konto zależy od tego, czy używasz ASP.NET Development Server albo Internet Information Services jako serwer sieci web. Konto użytkownika zależy również od systemu operacyjnego.

Jeśli używasz ASP.NET Development Server (domyślny serwer sieci web używane przez program Visual Studio) aplikacja wykonuje w kontekście konta użytkownika Windows. W takim przypadku należy dodać konto użytkownika Windows podczas logowania do serwera bazy danych.

Alternatywnie Jeśli używasz programu Internet Information Services następnie należy dodać konto ASPNET lub konta NT urzędu/NETWORK SERVICE podczas logowania do serwera bazy danych. Jeśli w systemie Windows XP, Dodaj konto ASPNET jako identyfikatora logowania do bazy danych. Jeśli używasz nowszego systemu operacyjnego, takich jak Windows Vista lub Windows Server 2008, Dodaj konta NT urzędu/NETWORK SERVICE podczas logowania do bazy danych.

Można dodać nowego konta użytkownika do bazy danych za pomocą programu Microsoft SQL Server Management Studio (patrz rysunek 9).

**Rysunek 9 — Tworzenie nowego logowania programu Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Po utworzeniu wymagane do zalogowania, należy do mapowania nazwy logowania użytkownika bazy danych przy użyciu ról właściwej bazy danych. Kliknij dwukrotnie logowania, a następnie wybierz kartę mapowania użytkowników. Wybierz jedną lub więcej aplikacji, usług ról bazy danych. Na przykład w celu uwierzytelniania użytkowników, należy włączyć aspnet\_członkostwa\_BasicAccess roli bazy danych. Aby można było utworzyć nowych użytkowników, należy włączyć aspnet\_członkostwa\_FullAccess roli bazy danych (zobacz rysunek 10).

**Na rysunku nr 10 — Dodawanie ról bazy danych usługi aplikacji**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób użycia uwierzytelniania formularzy, podczas tworzenia aplikacji ASP.NET MVC. Najpierw pokazaliśmy ci, jak utworzyć nowych użytkowników i ról, wykorzystując narzędzia do administrowania witryną sieci Web. Następnie pokazaliśmy ci, jak używać atrybutu [Authorize] do zapobiegania nieautoryzowanemu dostępowi wywołanie akcji kontrolera. Na koniec pokazaliśmy ci, jak skonfigurować aplikację MVC do przechowywania użytkownika oraz informacje o rolach w produkcyjnej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](preventing-javascript-injection-attacks-cs.md)
> [dalej](authenticating-users-with-windows-authentication-vb.md)
