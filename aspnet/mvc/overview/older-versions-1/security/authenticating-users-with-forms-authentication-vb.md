---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Uwierzytelniania użytkowników przy użyciu formularzy uwierzytelniania (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak za pomocą atrybutu [Authorize] hasło ochrony określonej strony w aplikacji MVC. Możesz dowiedzieć się, jak Administracja witryny sieci Web za pomocą...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ff425a4c9728de2eec3d0c94e76cb51a15de487
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Uwierzytelnianie użytkowników za pomocą uwierzytelniania formularzy (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak za pomocą atrybutu [Authorize] hasło ochrony określonej strony w aplikacji MVC. Jak używać narzędzia administrowania witryną sieci Web do tworzenia i zarządzania użytkownikami i rolami. Można również informacje o sposobie konfigurowania przechowywania informacji o kontach i roli użytkownika.


Celem tego samouczka jest wyjaśniają, jak formularzy można używać uwierzytelniania hasła ochrona widoków w aplikacjach ASP.NET MVC. Jak używać narzędzia administrowania witryną sieci Web do tworzenia użytkowników i ról. Możesz również sposób zapobiec nieautoryzowanemu wywoływania akcji kontrolera. Na koniec zostanie przedstawiony sposób skonfigurowania, gdzie są przechowywane nazwy użytkowników i hasła.

#### <a name="using-the-web-site-administration-tool"></a>Za pomocą narzędzia administrowania witryną sieci Web

Zanim przejdziemy inaczej możemy należy zacząć od tworzenie niektórych użytkowników i ról. Najprostszym sposobem tworzenia nowych użytkowników i ról ma korzystać z narzędzia administrowania witryną sieci Web 2008 w usłudze Visual Studio. To narzędzie można uruchomić po wybraniu opcji menu **projektu, konfiguracji ASP.NET**. Alternatywnie możesz uruchomić narzędzie do administrowania witryną sieci Web, klikając ikonę (nieco przerażenie) młota naciśnięcie na świecie, który jest wyświetlany w górnej części okna Eksploratora rozwiązań (zobacz rysunek 1).

**Rysunek 1 — Uruchamianie narzędzia administrowania witryną sieci Web**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

W ramach narzędzia administrowania witryną sieci Web tworzenia nowych użytkowników i role, wybierając kartę Zabezpieczenia. Kliknij przycisk **tworzenia użytkownika** łącze, aby utworzyć nowego użytkownika o nazwie Stephen (patrz rysunek 2). Podaj użytkownika Stephen dowolnym hasłem, które mają (na przykład *klucz tajny*).

**Rysunek 2 — Tworzenie nowego użytkownika**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Pierwszy Włączanie ról i definiowanie co najmniej jedną rolę do tworzenia nowych ról. Włącz role, klikając **Włącz role** łącza. Następnie należy utworzyć rolę o nazwie *Administratorzy* klikając **Utwórz role lub zarządzaj nimi** link (patrz rysunek 3).

**Rysunek 3 — Tworzenie nowej roli**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Na koniec Utwórz nowego użytkownika o nazwie Zosi i skojarz Zosi z rolą Administratorzy klikając łącza tworzenia użytkownika i wybierając administratorów, podczas tworzenia Zosi (patrz rysunek 4).

**Rysunek 4 — Dodawanie użytkownika do roli**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Gdy wszystkie jest nazywany i gotowe, powinien mieć dwóch nowych użytkowników o nazwie Stephen i Zosi. Nowa rola o nazwie Administratorzy powinni również mieć. Zosi jest członkiem roli Administratorzy i Stephen nie jest.

#### <a name="requiring-authorization"></a>Wymaganie autoryzacji

Możesz wymagać użytkownika do uwierzytelnienia użytkownika wywołuje akcji kontrolera, dodając atrybut [Authorize] do akcji. Atrybut [Authorize] można zastosować do akcji kontrolera poszczególnych lub ten atrybut można stosować do klasy całego kontrolera.

Na przykład kontrolera 1 lista przedstawia akcji o nazwie CompanySecrets(). Ponieważ ta akcja ma atrybut [Authorize], chyba że użytkownik jest uwierzytelniany nie można wywołać tej akcji.

**Wyświetlanie listy 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Jeśli wywołanie akcji CompanySecrets(), wprowadzając adres URL /Home/CompanySecrets na pasku adresu przeglądarki, a nie jesteś uwierzytelniony użytkownik, a następnie nastąpi przekierowanie do widoku logowania automatycznie (patrz rysunek 5).

**Rysunek 5 — widoku logowania**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Widok logowania umożliwia wprowadź nazwę użytkownika i hasło. Jeśli nie jesteś zarejestrowanym użytkownikiem, a następnie kliknięcie **zarejestrować** łącze, aby przejść do rejestru wyświetlania (patrz rysunek 6). Widok rejestru umożliwia utworzenie nowego konta użytkownika.

**Rysunek 6 — widok rejestru**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Po pomyślnym zalogowaniu można wyświetlić CompanySecrets wyświetlania (patrz rysunek 7). Domyślnie możesz zalogować się do czasu zamknięcia okna przeglądarki.

**Rysunek 7 — widok CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autoryzowanie według nazwy użytkownika lub roli użytkownika

Aby ograniczyć dostęp do akcji kontrolera do określonego zestawu użytkowników lub zestaw ról użytkownika, można użyć atrybutu [Authorize]. Na przykład zmodyfikowanych kontrolera głównej wyświetlania 2 zawiera dwa nowe akcje o nazwie StephenSecrets() i AdministratorSecrets().

**Wyświetlanie listy 2 — Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Tylko użytkownik z nazwą użytkownika Stephen może wywołać akcję StephenSecrets(). Przekierowanie do widoku logowania wszystkich innych użytkowników. Właściwość użytkowników akceptuje rozdzielana przecinkami lista nazw kont użytkowników.

Tylko użytkownicy należący do roli Administratorzy mogą wywoływać Akcja AdministratorSecrets(). Na przykład ponieważ Zosi jest członkiem grupy Administratorzy, klika wywołanie akcji AdministratorSecrets(). Przekierowanie do widoku logowania wszystkich innych użytkowników. Właściwość role akceptuje rozdzielana przecinkami lista nazw ról.

#### <a name="configuring-authentication"></a>Konfigurowanie uwierzytelniania

W tym momencie możesz może się zastanawiać, gdzie jest przechowywana informacji o kontach i roli użytkownika. Domyślnie informacje są przechowywane w bazie danych (RANU) programu SQL Express nazwane ASPNETDB.mdf znajduje się w aplikacji MVC aplikacji\_folderem danych. Ta baza danych jest generowany przez platformę ASP.NET automatycznie podczas uruchamiania przy użyciu członkostwa.

Aby wyświetlić ASPNETDB.mdf bazy danych w oknie Eksploratora rozwiązań, należy najpierw wybierz opcję menu projektu, Pokaż wszystkie pliki.

Przy użyciu bazy danych SQL Express domyślny jest poprawnie przy tworzeniu aplikacji. Prawdopodobnie jednak nie chcesz użyć ASPNETDB.mdf domyślnej bazy danych w przypadku aplikacji produkcyjnej. W takim przypadku można zmienić przechowywania informacji o koncie użytkownika, wykonując następujące dwa kroki:

1. Dodawanie obiektów bazy danych usług aplikacji do produkcyjnej bazy danych — zmienić parametry połączenia aplikacji wskaż produkcyjnej bazy danych

Pierwszym krokiem jest dodanie wszystkie niezbędne obiekty bazy danych (tabele i procedury składowane) do produkcyjnej bazy danych. Najprostszym sposobem, aby dodać te obiekty do nowej bazy danych jest wykorzystać Kreator konfiguracji ASP.NET SQL Server (patrz rysunek 8). To narzędzie można uruchomić, należy otworzyć wiersz polecenia 2008 programu Visual Studio z grupy programu Microsoft Visual Studio 2008 i wykonywania w wierszu polecenia następujące polecenie:

aspnet\_regsql

**Rysunek 8 — Kreator konfiguracji ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

Kreator konfiguracji ASP.NET SQL Server umożliwia wybierz bazę danych programu SQL Server w sieci i zainstalować wszystkie obiekty bazy danych wymaganych przez usługi aplikacji ASP.NET. Serwer bazy danych nie jest wymagane znajdować się na komputerze lokalnym.

> [!NOTE]
> Jeśli nie chcesz używać Kreator konfiguracji ASP.NET SQL Server można znaleźć skrypty SQL dodawania obiektów bazy danych usług aplikacji w następującym folderze:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


Po utworzeniu niezbędne obiekty bazy danych, należy zmodyfikować połączenie z bazą danych używanych przez aplikację MVC. Zmodyfikuj ApplicationServices parametry połączenia w pliku konfiguracyjnym (web.config) w sieci web tak, aby wskazywało w produkcyjnej bazie danych. Na przykład modyfikacji połączenia do wyświetlania 3 wskazuje na bazie danych o nazwie MyProductionDB (oryginalny ciąg połączenia ApplicationServices ma zostały oznaczone komentarzami).

**Wyświetlanie listy 3 — pliku Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurowanie uprawnień do bazy danych

Jeśli używasz zintegrowane zabezpieczenia do połączenia z bazą danych następnie musisz dodać prawidłowe konto użytkownika systemu Windows jako identyfikatora logowania do bazy danych. Poprawne konto zależy od tego, czy używasz ASP.NET Development Server albo IIS jako serwera sieci web. Konto użytkownika zależy również od systemu operacyjnego.

Jeśli używasz ASP.NET Development Server (serwer sieci web domyślne używane przez program Visual Studio) aplikacji będzie wykonywany w kontekście konta użytkownika systemu Windows. W takim przypadku należy dodać konto użytkownika systemu Windows jako logowanie do serwera bazy danych.

Alternatywnie Jeśli używasz Internetowe usługi informacyjne należy dodać konto ASPNET lub konta NT urzędu/NETWORK SERVICE jako logowanie do serwera bazy danych. Jeśli używasz systemu Windows XP Dodaj konto ASPNET jako identyfikatora logowania do bazy danych. Jeśli używane są nowsze systemu operacyjnego, takich jak Windows Vista lub Windows Server 2008, Dodaj konto usługi NT urzędu i sieci, jako nazwy logowania bazy danych.

Można dodać nowego konta użytkownika do bazy danych przy użyciu programu Microsoft SQL Server Management Studio (patrz rysunek 9).

**Rysunek 9 — Tworzenie nowego logowania programu Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Po utworzeniu wymagane logowania, należy do mapowania nazwy logowania użytkownika bazy danych z rolami właściwej bazy danych. Kliknij dwukrotnie nazwy logowania, a następnie wybierz kartę mapowania użytkowników. Wybierz co najmniej jedną rolę bazy danych usług aplikacji. Na przykład w celu uwierzytelniania użytkowników, należy włączyć aspnet\_członkostwa\_BasicAccess roli bazy danych. Aby można było utworzyć nowych użytkowników, musisz włączyć aspnet\_członkostwa\_FullAccess roli bazy danych (zobacz rysunek 10).

**Rysunek 10 — Dodawanie ról bazy danych usług aplikacji**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób użycia uwierzytelniania formularzy, podczas tworzenia aplikacji platformy ASP.NET MVC. Po pierwsze przedstawiono sposób tworzenia nowych użytkowników i role, korzystając z narzędzia administrowania witryną sieci Web. Następnie przedstawiono sposób użycia atrybutu [Authorize], aby zapobiec nieautoryzowanemu wywoływania akcji kontrolera. Ponadto przedstawiono sposób konfigurowania aplikacji MVC do użytkownika oraz informacje o rolach są przechowywane w produkcyjnej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](preventing-javascript-injection-attacks-cs.md)
> [dalej](authenticating-users-with-windows-authentication-vb.md)
