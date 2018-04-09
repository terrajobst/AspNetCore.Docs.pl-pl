---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Logowanie jednokrotne (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Logowanie jednokrotne (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Istnieje wiele problemów z zabezpieczeniami można traktować Jeśli projektujesz aplikacji w chmurze, ale dla tej serii firma Microsoft będzie skupić się na co najmniej jeden: logowanie jednokrotne. Często dotyczy osób zapytania to: "I używam przede wszystkim tworzenia aplikacji dla pracowników firmy; sposób obsługi tych aplikacji w chmurze i nadal włączyć je do korzystania z tego samego modelu zabezpieczeń, który Moje pracowników i używanych w środowisku lokalnym, gdy te są uruchomione aplikacje który znajdują się wewnątrz niej?" Jednym ze sposobów włączyć obsługi tego scenariusza jest nazywany usługi Azure Active Directory (Azure AD). Usługi Azure AD pozwala udostępnić enterprise — biznesowych (LOB) aplikacji w Internecie, i umożliwia udostępnia swoim partnerom tych aplikacji.

## <a name="introduction-to-azure-ad"></a>Wprowadzenie do usługi Azure AD

[Usługi Azure AD](https://docs.microsoft.com/azure/active-directory/) zapewnia [usługi Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) w chmurze. Najważniejsze funkcje obejmują:

- Umożliwia integrację z lokalnej usługi Active Directory.
- Umożliwia logowanie jednokrotne z aplikacjami.
- Obsługuje on otwarty standardy, takie jak [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), i [OAuth 2.0](http://oauth.net/2/).
- Obsługuje ona Enterprise [interfejsu API REST wykresu](https://msdn.microsoft.com/library/hh974476.aspx).

Załóżmy, że masz środowiska usługi Active Directory systemu Windows Server lokalnego, który umożliwia pracownikom zalogować się do aplikacji w sieci Intranet:

![](single-sign-on/_static/image1.png)

Co usługi Azure AD umożliwia wykonanie jest tworzenie katalogu w chmurze. Jest funkcją wolnego i łatwo skonfigurować.

Może być całkowicie niezależna od lokalnej usługi Active Directory; Możesz też zaznaczyć każda osoba, która ma w niej i uwierzytelniania je w aplikacjach internetowych.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Lub zintegrować ją z lokalnej usługi AD.

![Usługi AD i integracja z drewna](single-sign-on/_static/image3.png)

Teraz wszystkich pracowników, którzy mogą uwierzytelniać lokalnymi mogą również uwierzytelniać za pośrednictwem Internetu — bez konieczności otwarcie zapory lub wdrażanie nowych serwerów w centrum danych. Można nadal korzystać z wszystkich istniejącego środowiska usługi Active Directory, który znasz i użyj już dziś, aby nadać jednokrotnego Twojej aplikacji wewnętrznych funkcję.

Po wprowadzeniu tego połączenia między AD i Azure AD, można również włączyć aplikacji sieci web i urządzeniach przenośnych, aby uwierzytelniać pracowników w chmurze i można włączyć aplikacji innych firm, takich jak aplikacje pakietu Office 365, witryny SalesForce.com lub Google, aby zaakceptować użytkownika poświadczenia pracowników. Jeśli używasz usługi Office 365, użytkownik jest już skonfigurowane z usługą Azure AD ponieważ usługi Office 365 używa usługi Azure AD do uwierzytelniania i autoryzacji.

![aplikacje firm 3](single-sign-on/_static/image4.png)

Zaletą tej metody jest dowolnym momencie organizacji dodaje lub usuwa użytkownika, lub użytkownik zmieni hasło, użyj tego samego procesu, używaną obecnie w środowisku lokalnym. Wszystkie z lokalnej usługi AD zmiany są automatycznie propagowane do środowiska chmury.

Jeśli firma jest przy użyciu lub przechodzenia do usługi Office 365 szczęście jest to, że masz usługi Azure AD — konfiguracja automatycznie, ponieważ usługi Office 365 do uwierzytelniania używa usługi Azure AD. Aby można było łatwo korzystać w własnych aplikacji tego samego uwierzytelniania, który korzysta z usługi Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Konfigurowanie dzierżawa usługi Azure AD

katalog usługi Azure AD jest nazywany usługi Azure AD [dzierżawy](https://technet.microsoft.com/library/jj573650.aspx), i skonfigurowanie dzierżawcy jest bardzo prosty. Poniżej opisano jak jest wykonywane w portalu zarządzania Azure w celu zilustrowania koncepcji, ale oczywiście podobnie jak inne funkcje portalu również należy go za pomocą skryptów lub interfejsu API zarządzania.

W portalu zarządzania na karcie usługi Active Directory.

![DREWNA w portalu](single-sign-on/_static/image5.png)

Automatycznie mieć jednej dzierżawy usługi Azure AD dla konta platformy Azure, a kliknięcie **Dodaj** znajdujący się u dołu strony, aby utworzyć dodatkowe katalogi. Na przykład może być jeden dla środowiska testowego i jeden do produkcji. Należy dobrze przemyśleć co nazwa nowego katalogu. Jeśli używasz nazwę katalogu, a następnie użyty nazwę ponownie dla użytkowników, które mogą być mylące.

![Dodaj katalog](single-sign-on/_static/image6.png)

Portal ma pełną obsługę tworzenia, usuwania i zarządzanie użytkownikami, w tym środowisku. Na przykład, aby dodać użytkownika wybierz **użytkowników** i kliknij polecenie **Dodaj użytkownika** przycisku.

![Przycisk Dodaj użytkownika](single-sign-on/_static/image7.png)

![Dodaj użytkownika okna dialogowego](single-sign-on/_static/image8.png)

Można utworzyć nowego użytkownika, który istnieje tylko w tym katalogu, lub możesz zarejestrować Account Microsoft jako użytkownik w tym katalogu lub rejestru lub z innego katalogu usługi Azure AD jako użytkownik w tym katalogu. (W katalogu prawdziwe, domena domyślna będzie ContosoTest.onmicrosoft.com. Umożliwia także domeny wybranej przez użytkownika, takie jak contoso.com.)

![Typy użytkownika](single-sign-on/_static/image9.png)

![Dodaj użytkownika okna dialogowego](single-sign-on/_static/image10.png)

Użytkownika można przypisać do roli.

![Profil użytkownika](single-sign-on/_static/image11.png)

I, konto zostanie utworzone hasło tymczasowe.

![Hasło tymczasowe](single-sign-on/_static/image12.png)

Użytkownicy w ten sposób można utworzyć natychmiast zalogować się do aplikacji sieci web przy użyciu tego katalogu w chmurze.

Co to jest doskonały dla przedsiębiorstwa logowania jednokrotnego, jednak jest **integracji katalogów** karty:

![Karta Integracja katalogu](single-sign-on/_static/image13.png)

Po włączeniu Integracja katalogu i [Pobierz narzędzie](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), można zsynchronizować ten katalog chmury z istniejącej lokalnej usługi Active Directory już używany wewnątrz organizacji. Następnie wszystkich użytkowników przechowywanych w katalogu zostaną wyświetlone w tym katalogu w chmurze. Aplikacje w chmurze może teraz uwierzytelniać wszystkich pracowników przy użyciu swoich istniejących poświadczeń usługi Active Directory. Oraz o wolnym wszystko, co to jest — narzędzie synchronizacji i usługi Azure AD sama.

Narzędzie to jest kreatora, który jest łatwy w użyciu, jak widać w te zrzuty ekranu. Nie są kompletne instrukcje, tylko przykładem zastosowania podstawowy proces. Aby uzyskać szczegółowe informacje o how-do-it, zobacz linki w [zasobów](#resources) sekcji na końcu działu.

![Kreator konfiguracji narzędzia synchronizacji drewna](single-sign-on/_static/image14.png)

Kliknij przycisk **dalej**, a następnie wprowadź swoje poświadczenia usługi Azure Active Directory.

![Kreator konfiguracji narzędzia synchronizacji drewna](single-sign-on/_static/image15.png)

Kliknij przycisk **dalej**, a następnie wprowadź lokalnej poświadczeń AD.

![Kreator konfiguracji narzędzia synchronizacji drewna](single-sign-on/_static/image16.png)

Kliknij przycisk **dalej**, a następnie wskaż, jeśli chcesz Przechowywanie skrótów haseł usługi AD w chmurze.

![Kreator konfiguracji narzędzia synchronizacji drewna](single-sign-on/_static/image17.png)

Wartość skrótu hasła, które mogą być przechowywane w chmurze jest skrót jednokierunkowy; rzeczywiste hasła nigdy nie są przechowywane w usłudze Azure AD. Jeśli zdecydujesz się na przechowywanie skrótów w chmurze, musisz użyć [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (AD FS). Dostępne są także [inne czynniki, które należy wziąć pod uwagę podczas wybierania, czy należy używać usług AD FS](https://technet.microsoft.com/library/jj573653.aspx). Opcja usług AD FS wymaga kilka dodatkowych czynności konfiguracyjnych.

Jeśli zdecydujesz się przechowywać skróty w chmurze, wszystko gotowe i uruchamia narzędzie synchronizacji katalogów po kliknięciu **dalej**.

![Kreator konfiguracji narzędzia synchronizacji drewna](single-sign-on/_static/image18.png)

I w ciągu kilku minut wszystko będzie gotowe.

![Kreator konfiguracji narzędzia synchronizacji drewna](single-sign-on/_static/image19.png)

Trzeba uruchomić na jeden kontroler domeny w organizacji, w systemie Windows 2003 lub nowszy. I ponownego uruchamiania. Gdy wszystko będzie gotowe, wszyscy użytkownicy są w chmurze i mogą wykonywać rejestracji jednokrotnej z sieci web lub aplikacji mobilnej, przy użyciu SAML, OAuth lub WS Fed.

Czasami uzyskać możemy zadawane, o jak bezpieczne jest — Microsoft używa go do ich własnych poufnych danych? I tak, nie. Na przykład, jeśli przejdziesz do wewnętrznej witryny Microsoft SharePoint na [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), zostanie wyświetlony monit, aby się zalogować.

![Logowania usługi Office 365](single-sign-on/_static/image20.png)

Microsoft ma włączoną obsługę usług AD FS, więc po wprowadzeniu Identyfikatora Microsoft następuje przekierowanie do strony logowania usług AD FS.

![Logowania usług AD FS](single-sign-on/_static/image21.png)

I po wprowadzeniu poświadczeń przechowywanych w wewnętrzne konto Microsoft AD mają dostęp do tej aplikacji wewnętrznych.

![Witryna MS SharePoint](single-sign-on/_static/image22.png)

Używamy serwera logowanie AD głównie, ponieważ już było ustawić przed usługi Azure AD stały się dostępne, ale przechodzi przez katalog usługi Azure AD w chmurze procesu logowania usług AD FS. Możemy umieścić naszych ważnych dokumentów, kontroli źródła, pliki zarządzania wydajności, raporty ze sprzedaży i więcej, w chmurze i przy użyciu tego dokładnie tego samego rozwiązania do zabezpieczenia.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Tworzenie aplikacji platformy ASP.NET, która używa usługi Azure AD dla rejestracji jednokrotnej

Visual Studio ułatwia naprawdę do tworzenia aplikacji, która używa usługi Azure AD dla logowania jednokrotnego, jak widać w kilku zrzutów ekranu.

Podczas tworzenia nowej aplikacji ASP.NET, MVC i formularzy sieci Web, domyślną metodą uwierzytelniania jest tożsamości ASP.NET. Aby zmienić do usługi Azure AD, możesz kliknąć przycisk **Zmień uwierzytelnianie** przycisku.

![Zmienianie uwierzytelniania](single-sign-on/_static/image23.png)

Wybierz konta organizacyjne, wpisz nazwę domeny, a następnie wybierz rejestracji jednokrotnej.

![Skonfiguruj dialog uwierzytelniania](single-sign-on/_static/image24.png)

Możesz też udzielić aplikacji odczytu i odczytu/zapisu uprawnień do katalogu danych. Jeśli możesz to zrobić, można użyć [interfejsu API REST wykres Azure](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) aby wyszukać użytkowników numer telefonu, dowiedzieć się, jeśli są one w pakiecie office, podczas ich ostatniego logowania na itp.

To wszystko, co trzeba zrobić — Visual Studio poprosi o podanie poświadczeń administratora dzierżawy usługi Azure AD, a następnie konfiguruje zarówno projektu i dzierżawy usługi Azure AD dla nowej aplikacji.

Po uruchomieniu projektu, zostanie wyświetlona strona logowania i aby móc zalogować się przy użyciu poświadczeń użytkownika w katalogu usługi Azure AD.

![Logowanie konta organizacji](single-sign-on/_static/image25.png)

![Zalogowany](single-sign-on/_static/image26.png)

Podczas wdrażania aplikacji na platformie Azure, wszystkie wystarczy, że jest zaznaczona **Włączanie uwierzytelniania organizacyjnego** pole wyboru, a następnie ponownie Visual Studio odpowiedzialna za całą konfigurację za Ciebie.

![Publikowanie w sieci Web](single-sign-on/_static/image27.png)

Te zrzuty ekranu pochodzą z pełną samouczek krok po kroku, pokazujący sposób tworzenia aplikacji korzystającej z uwierzytelniania usługi Azure AD: [tworzenie aplikacji ASP.NET w usłudze Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Podsumowanie

W tym rozdziale widać, że usługi Azure Active Directory, Visual Studio i ASP.NET, ułatwiają konfigurowanie logowania jednokrotnego w aplikacjach internetowych dla użytkowników w organizacji. Użytkownicy mogą logowania w aplikacjach internetowych przy użyciu tych samych poświadczeń, których używają do logowania przy użyciu usługi Active Directory w sieci wewnętrznej.

[Następnego rozdziału](data-storage-options.md) analizuje opcje magazynowania danych dostępnych dla aplikacji w chmurze.

<a id="resources"></a>
## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Dokumentacja usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Strona Portal dokumentacji usługi Azure AD w witrynie windowsazure.com. Samouczki krok po kroku, zobacz **opracowanie** sekcji.
- [Usługa Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Strona Portal dokumentacji na temat uwierzytelniania wieloskładnikowego na platformie Azure.
- [Opcje uwierzytelniania konto organizacyjne](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Wyjaśnienie opcje uwierzytelniania usługi Azure AD w oknie dialogowym Nowy projekt programu Visual Studio 2013.
- [Microsoft Patterns and Practices — wzorzec tożsamości federacyjnych](https://msdn.microsoft.com/library/dn589790.aspx).
- [Porada: Zainstaluj narzędzie do synchronizacji usługi Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 Mapa zawartości](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Łącza do dokumentacji usług AD FS 2.0.
- [Autoryzacji opartej na rolach i na podstawie listy ACL w aplikacji systemu Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Przykładowa aplikacja.
- [Blog Azure Active Directory interfejsu API Graph](https://blogs.msdn.com/b/aadgraphteam/).
- [Kontrola dostępu w modelu BYOD i integracji katalogu hybrydowej infrastruktury tożsamości](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Techniczna Ed 2014 sesji wideo przez Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Poprzednie](web-development-best-practices.md)
> [dalej](data-storage-options.md)
