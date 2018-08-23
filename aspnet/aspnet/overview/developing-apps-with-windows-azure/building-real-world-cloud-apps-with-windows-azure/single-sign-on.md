---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Logowanie jednokrotne (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: feace28c373a767453b1b984f6fdc5a3d97a9217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753077"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Logowanie jednokrotne (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Istnieje wiele problemów z zabezpieczeniami zastanowić się, gdy tworzysz aplikację w chmurze, ale w tej serii samouczków skupimy się na co najmniej jeden: logowanie jednokrotne. Osoby pytania często pytają, czy to: "czy jestem przede wszystkim tworzenia aplikacji dla pracowników firmy; jak obsługiwać te aplikacje w chmurze i włączyć je do korzystania z tego samego modelu zabezpieczeń, która Moje pracowników znasz i użyj w środowisku lokalnym, jeśli są one uruchamiane aplikacje, są hostowane wewnątrz zapory?" Jednym ze sposobów, możemy włączyć ten scenariusz jest nazywany usługi Azure Active Directory (Azure AD). Usługa Azure AD umożliwia udostępnienia enterprise line of business (LOB) aplikacji w Internecie i umożliwia udostępnienie tych aplikacji swoim partnerom.

## <a name="introduction-to-azure-ad"></a>Wprowadzenie do usługi Azure AD

[Usługa Azure AD](https://docs.microsoft.com/azure/active-directory/) zapewnia [usługi Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) w chmurze. Najważniejsze funkcje obejmują następujące czynności:

- Integruje się z usługą Active Directory w środowisku lokalnym.
- Umożliwia logowanie jednokrotne z aplikacjami.
- Obsługuje on otwartych standardów, takich jak [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), i [OAuth 2.0](http://oauth.net/2/).
- Obsługuje ona Enterprise [interfejsu API REST programu Graph](https://msdn.microsoft.com/library/hh974476.aspx).

Załóżmy, że istnieje w lokalnym środowisku usługi Active Directory systemu Windows Server, która umożliwia pracownikom zalogować się do aplikacji intranetowych:

![](single-sign-on/_static/image1.png)

Usługa Azure AD umożliwia wykonywanie jest Utwórz katalog w chmurze. Jest bezpłatną funkcją i są łatwe do skonfigurowania.

Może być całkowicie niezależna od swojej lokalnej usługi Active Directory; Możesz umieścić każdy użytkownik ma w nim i uwierzytelniaj je w aplikacjach internetowych.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Możesz też zintegrować przy użyciu lokalnej usługi AD.

![Usługi AD i integracji WAAD](single-sign-on/_static/image3.png)

Teraz wszyscy pracownicy, którzy mogą uwierzytelniać się w środowisku lokalnym może także uwierzytelnić się za pośrednictwem Internetu — bez konieczności otwierania zapory lub wdrażanie nowych serwerów w centrum danych. Można nadal korzystać z wszystkich istniejącego środowiska usługi Active Directory, który znasz i użyj już dziś, aby nadać usługi logowania jednokrotnego aplikacji wewnętrznych na możliwości.

Po dokonaniu to połączenie między AD i Azure AD, można również włączyć aplikacji sieci web i urządzeń przenośnych do uwierzytelnienia pracowników w chmurze i można włączyć aplikacji innych firm, takich jak aplikacje usługi Office 365 i SalesForce.com, Google, aby zaakceptować Twoje poświadczenia pracowników. Jeśli używasz usługi Office 365, możesz już skonfigurowano Cię z usługą Azure AD, ponieważ usługi Office 365 używa usługi Azure AD do uwierzytelniania i autoryzacji.

![aplikacje firm 3](single-sign-on/_static/image4.png)

Zaletą tego podejścia jest ilekroć organizacji dodaje lub usuwa użytkownika, lub użytkownik zmieni hasło, użyj tego samego procesu używanego obecnie w środowisku w środowisku lokalnym. Wszystkie z lokalnej usługi AD zmiany są automatycznie propagowane do środowiska chmury.

Jeśli firma korzysta z lub przechodzenia do usługi Office 365, dobra wiadomość jest fakt, że masz skonfigurowane automatycznie, ponieważ usługi Office 365 używa usługi Azure AD do uwierzytelniania z usługi Azure AD. Aby można było łatwo używać w swoich własnych aplikacji tego samego uwierzytelniania, która korzysta z usługi Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Konfigurowanie dzierżawę usługi Azure AD

katalog usługi Azure AD jest nazywany moduł usługi Azure AD [dzierżawy](https://technet.microsoft.com/library/jj573650.aspx), i konfigurowania dzierżawy jest całkiem proste. Pokażemy ci jak jest wykonywane w portalu zarządzania systemu Azure w celu zilustrowania koncepcji podanych, ale oczywiście podobnie jak inne funkcje portalu możesz także zrobić go za pomocą skryptu lub interfejsu API zarządzania.

W portalu zarządzania, kliknij kartę usługi Active Directory.

![WAAD w portalu](single-sign-on/_static/image5.png)

Automatycznie ma jedną dzierżawę usługi Azure AD dla konta platformy Azure i możesz kliknąć pozycję **Dodaj** znajdujący się u dołu strony Aby utworzyć dodatkowe katalogi. Możesz chcieć jedno dla środowiska testowego i jedno dla środowiska produkcyjnego, na przykład. Należy dobrze przemyśleć co nazwa nowego katalogu. Jeśli używasz nazwę katalogu, a następnie użyć swoją nazwę ponownie dla jednego z użytkowników, które mogą być mylące.

![Dodaj katalog](single-sign-on/_static/image6.png)

Portal udostępnia pełną obsługę tworzenia, usuwania i zarządzanie użytkownikami, w tym środowisku. Na przykład, aby dodać użytkownika przejdź do **użytkowników** kartę, a następnie kliknij przycisk **Dodaj użytkownika** przycisku.

![Przycisk Dodaj użytkownika](single-sign-on/_static/image7.png)

![Dodaj okno dialogowe użytkownik](single-sign-on/_static/image8.png)

Można utworzyć nowego użytkownika, który istnieje tylko w tym katalogu lub zarejestrowaniem Account Microsoft jako użytkownika w tym katalogu lub Zarejestruj się lub użytkownik z innego katalogu usługi Azure AD jako użytkownik w tym katalogu. (W rzeczywistych katalogu, domyślna domena jest ContosoTest.onmicrosoft.com. Umożliwia także domeny wybranej przez użytkownika, np. contoso.com.)

![Typy użytkownika](single-sign-on/_static/image9.png)

![Dodaj okno dialogowe użytkownik](single-sign-on/_static/image10.png)

Można przypisać użytkownika do roli.

![Profil użytkownika](single-sign-on/_static/image11.png)

A konto zostanie utworzone hasło tymczasowe.

![Hasło tymczasowe](single-sign-on/_static/image12.png)

Użytkownicy, tworzysz w ten sposób natychmiast zalogować się do aplikacji sieci web za pomocą tego katalogu w chmurze.

Co to jest bardzo przydatne na potrzeby logowania jednokrotnego dla firm, jednak jest **integracji katalogu** karty:

![Karta Integracja katalogu](single-sign-on/_static/image13.png)

Po włączeniu Integracja katalogu i [Pobierz narzędzie](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), można zsynchronizować tego katalogu w chmurze przy użyciu istniejącej lokalnej usługi Active Directory, już używasz wewnątrz organizacji. Następnie wszystkich użytkowników, przechowywane w katalogu pojawią się tego katalogu w chmurze. Aplikacje w chmurze może teraz uwierzytelniać wszystkich pracowników za pomocą istniejących poświadczeń usługi Active Directory. I wszystko, co to jest bezpłatne — narzędzia do synchronizacji i samej usługi Azure AD.

To narzędzie jest Kreator, który jest łatwy w użyciu, jak widać na te zrzuty ekranu. Nie są pełne instrukcje, przykładowe zawierająca podstawowy proces. Aby uzyskać szczegółowe informacje jak-to-it, skorzystaj z łączy w [zasobów](#resources) sekcji na końcu rozdziale.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image14.png)

Kliknij przycisk **dalej**, a następnie wprowadź swoje poświadczenia usługi Azure Active Directory.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image15.png)

Kliknij przycisk **dalej**, a następnie wprowadź lokalne poświadczenia usługi AD.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image16.png)

Kliknij przycisk **dalej**, a następnie wskaż, jeśli chcesz Przechowywanie skrótów haseł w usłudze AD w chmurze.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image17.png)

Wartość skrótu hasła, które mogą być przechowywane w chmurze to jednokierunkowe skróty; bieżące hasła nigdy nie są przechowywane w usłudze Azure AD. Jeśli zdecydujesz się na przechowywanie skrótów w chmurze, musisz użyć [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Dostępne są także [inne czynniki do rozważenia podczas wybierania, czy należy użyć usług AD FS](https://technet.microsoft.com/library/jj573653.aspx). Opcja usług AD FS wymaga kilku dodatkowych czynności konfiguracyjnych.

Jeśli zdecydujesz się przechowywać skrótów w chmurze, wszystko będzie gotowe i rozpoczyna synchronizację katalogów, po kliknięciu **dalej**.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image18.png)

I w ciągu kilku minut wszystko będzie gotowe.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image19.png)

Musisz uruchomić na jednym kontrolerze domeny w organizacji, w systemie Windows 2003 lub nowszy. I to bez konieczności ponownego uruchomienia. Gdy wszystko będzie gotowe, wszyscy użytkownicy znajdują się w chmurze i mogą wykonywać logowanie jednokrotne z sieci web lub aplikacji mobilnej, za pomocą protokołu SAML, OAuth i WS-Fed.

Czasami firma Microsoft pytanie o jak bezpieczne jest — Microsoft używa go dla ich własnych poufnych danych biznesowych? I tak, jaką robimy. Na przykład, jeśli przejdziesz do wewnętrznej witryny SharePoint firmy Microsoft w [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), zostanie wyświetlony monit, aby zalogować się.

![Logowanie w usłudze Office 365](single-sign-on/_static/image20.png)

Microsoft włączył usług AD FS, więc po wprowadzeniu Identyfikatora firmy Microsoft, możesz pobrać przekierowanie do strony logowania usług AD FS.

![Logowania usług AD FS](single-sign-on/_static/image21.png)

I po wprowadzeniu poświadczeń przechowywanych na koncie Microsoft AD wewnętrznego, masz dostęp do tej aplikacji wewnętrznych.

![Witryna MS SharePoint](single-sign-on/_static/image22.png)

Używamy serwera logowania usługi AD, przede wszystkim, ponieważ Musieliśmy usług AD FS, skonfigurować usługę Azure AD stały się dostępne, ale proces logowania przechodzi katalog usługi Azure AD w chmurze. Firma Microsoft może umieścić naszych ważnych dokumentów, kontroli źródła, plików zarządzania wydajnością, raporty dotyczące sprzedaży i więcej w chmurze, a następnie za pomocą tego dokładnie tego samego rozwiązania je zabezpieczyć.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Tworzenie aplikacji platformy ASP.NET używająca usługi Azure AD na potrzeby logowania jednokrotnego

Program Visual Studio sprawia, że bardzo łatwo jest tworzyć aplikację, która używa usługi Azure AD na potrzeby logowania jednokrotnego, jak widać na kilka zrzutów ekranu.

Podczas tworzenia nowej aplikacji platformy ASP.NET MVC i formularzy sieci Web, domyślną metodą uwierzytelniania jest tożsamości ASP.NET. Aby zmienić do usługi Azure AD, możesz kliknąć przycisk **Zmień uwierzytelnianie** przycisku.

![Zmień uwierzytelnianie](single-sign-on/_static/image23.png)

Wybierz konta organizacyjne, wprowadź nazwę domeny, a następnie wybierz logowanie jednokrotne.

![Konfigurowanie okna dialogowego uwierzytelniania](single-sign-on/_static/image24.png)

Można również nadać odczytu aplikacji lub uprawnienia dla katalogu danych odczytu i zapisu. Jeśli to zrobisz, można użyć [interfejsu API REST usługi Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) aby wyszukać użytkowników, numer telefonu, Dowiedz się, jeśli są one w pakiecie office, kiedy się logowali się ostatnio na itp.

To wszystko, co należy zrobić — Visual Studio Wyświetla prośbę o poświadczenia administratora dla dzierżawy usługi Azure AD, a następnie skonfiguruje projekt i dzierżawy usługi Azure AD dla nowej aplikacji.

Kiedy uruchamiasz projekt, zostanie wyświetlona strona logowania, a następnie możesz zalogować się przy użyciu poświadczeń użytkownika katalogu usługi Azure AD.

![Logowanie konta organizacji](single-sign-on/_static/image25.png)

![Zalogowany](single-sign-on/_static/image26.png)

Podczas wdrażania aplikacji na platformie Azure, musisz wykonać wystarczy wybierz **Włączanie uwierzytelniania organizacyjnego** pole wyboru, a następnie ponownie program Visual Studio zajmuje się całą konfigurację za Ciebie.

![Publikowanie w sieci Web](single-sign-on/_static/image27.png)

Te zrzuty ekranu pochodzą kompletny samouczek krok po kroku, który pokazuje, jak utworzyć aplikację, która korzysta z uwierzytelniania usługi Azure AD: [opracowywanie aplikacji ASP.NET w usłudze Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Podsumowanie

W tym rozdziale pokazano, że usługi Azure Active Directory, program Visual Studio i platformy ASP.NET, ułatwiają konfigurowanie logowania jednokrotnego w aplikacjach internetowych dla użytkowników w organizacji. Użytkownicy zalogować się aplikacje internetowe przy użyciu tych samych poświadczeń, których używają do logowania się na korzystanie z usługi Active Directory w sieci wewnętrznej.

[Następny rozdział](data-storage-options.md) porównaniem opcji magazynowania danych dostępnych dla aplikacji w chmurze.

<a id="resources"></a>
## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Dokumentacja usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Strona Portal dokumentacji usługi Azure AD w witrynie w witrynie windowsazure.com. Samouczki krok po kroku, zobacz **programowanie** sekcji.
- [Usługa Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Strona portalu dokumentację dotyczącą usługi uwierzytelnianie wieloskładnikowe na platformie Azure.
- [Opcje uwierzytelniania konto organizacyjne](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Wyjaśnienie opcje uwierzytelniania usługi Azure AD, w oknie dialogowym Nowy projekt programu Visual Studio 2013.
- [Microsoft Patterns and Practices - federacyjne wzorzec tożsamości](https://msdn.microsoft.com/library/dn589790.aspx).
- [Porady: Instalowanie narzędzi usługi Azure Active Directory Sync](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [2.0 Mapa zawartości usług federacyjnych Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Zawiera łącza do dokumentacji usług AD FS 2.0.
- [Autoryzacja oparta na rolach i na podstawie listy ACL w aplikacji Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Przykładowa aplikacja.
- [Blog usługi Azure Active Directory interfejsu API programu Graph](https://blogs.msdn.com/b/aadgraphteam/).
- [Kontrola dostępu w modelu BYOD i Integracja katalogu w hybrydowej infrastruktury tożsamości](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 sesji wideo przez Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Poprzednie](web-development-best-practices.md)
> [dalej](data-storage-options.md)
