---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: "Użytkownicy i role w środowisku produkcyjnym w sieci Web (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Narzędzie administracyjnej witryny sieci Web platformy ASP.NET (WSAT) udostępnia interfejs użytkownika sieci web, do konfigurowania ustawień członkostwo i role oraz do tworzenia, edytowania,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: ced68b633eb34d1ea75671ac4ffe7f512e911a0d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="users-and-roles-on-the-production-website-vb"></a>Użytkownicy i role w środowisku produkcyjnym w sieci Web (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> Narzędzie administrowania witryny sieci Web platformy ASP.NET (WSAT) zapewnia interfejsu użytkownika opartego na sieci web, do konfigurowania ustawień członkostwo i role oraz tworzenie, edytowanie i usuwanie użytkowników i ról. Niestety WSAT działa tylko po odwiedzeniu z localhost, co oznacza, że można nawiązać połączenia narzędzia administracyjnego witryny produkcji za pośrednictwem przeglądarki. Dobre wieści zakłada, że rozwiązania, które umożliwiają zarządzanie użytkownikami i rolami w środowisku produkcyjnym. W tym samouczku analizuje obejścia tych i innych użytkowników.


## <a name="introduction"></a>Wprowadzenie

Platforma ASP.NET 2.0 wprowadzono szereg *usług aplikacji*, zestaw modułów konstrukcyjnych usług, które można dodać do aplikacji sieci web, które są. Dodaliśmy, członkostwo i role usługi do witryny sieci Web przeglądami książki ponownie [ *Konfigurowanie witryny sieci Web czy korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md). Usługa członkostwa ułatwia tworzenie i obsługa kont użytkowników. usługi ról oferuje interfejs API do klasyfikacji użytkowników w grupach. Lokacja przeglądami książki ma trzy konta użytkowników — Scott, Jisun i Alicja - i jedną rolę administratora Scott i Jisun w roli administratora.

ŚRODOWISKO ASP. Usługi aplikacji w sieci nie są związane z konkretnej implementacji. Zamiast tego należy poinstruować usług aplikacji, aby użyć określonego *dostawcy*, i usługi za pomocą technologii określonego implementuje tego dostawcy. Został skonfigurowany przeglądami książki aplikacji sieci web do używania `SqlMembershipProvider` i `SqlRoleProvider` dostawców członkostwa i ról usług. Tych dwóch dostawców na przechowywanie informacji o kontach i roli użytkownika w bazie danych programu SQL Server i są najczęściej używane dostawców dla internetowego aplikacji sieci web hostowanej w lokalizacji hostingu firmy.

Zarządza wspólnym wyzwaniem dla deweloperów korzystających z usług członkostwa i ról użytkowników i role w środowisku produkcyjnym. Jak można usunąć konto użytkownika z witryny sieci Web produkcji, Dodaj nową rolę lub Dodaj istniejącego użytkownika do istniejącej roli? W tym samouczku Eksploruje różnych technik do zarządzania użytkownikami i rolami w witrynie sieci Web produkcji.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Za pomocą narzędzia administrowania witryną sieci Web ASP.NET

Program ASP.NET zawiera [narzędzie do administrowania witryną sieci Web](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx) (WSAT) ułatwia tworzenie i zarządzanie nimi kont użytkowników i ról i umożliwia określenie reguł autoryzacji na podstawie użytkowników i ról. Aby używać WSAT, kliknij ikonę konfiguracja platformy ASP.NET w Eksploratorze rozwiązań lub przejdź do menu witryny sieci Web lub projektu i wybierz opcję Konfiguracja programu ASP.NET. Albo podejście spowoduje uruchomienie przeglądarki sieci web i wskazaniu WSAT pod adresem, takich jak:`http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT jest podzielone na trzy części:

- **Zabezpieczenia** — Zarządzanie użytkownikami, ról i reguł autoryzacji.
- **ApplicationConfiguration** — zarządzanie &lt;appSettings&gt; i ustawienia SMTP w tym miejscu. Możesz można również przejdź do trybu offline i zarządzanie debugowanie i śledzenie ustawień w tym miejscu, a także określić domyślną stronę błędu niestandardowego.
- **ProviderConfiguration** — Skonfiguruj dostawców dla usług aplikacji.

W sekcji zabezpieczeń (pokazano **rysunek 1**) zawiera łącza do tworzenia nowych użytkowników, zarządzanie użytkownikami, tworzenie i zarządzanie rolami oraz tworzenie i zarządzanie regułami dostępu. W tym miejscu można dodać nową rolę do systemu, usunąć istniejącego użytkownika, lub dodawanie lub usuwanie ról określonego konta użytkownika.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Rysunek 1**: sekcja zabezpieczeń WSAT zawiera opcje do zarządzania użytkownikami i rolami  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image3.png))

Niestety WSAT jest dostępny tylko lokalnie. Nie można znaleźć WSAT w produkcji zdalnej witryny sieci Web; w przypadku odwiedzenia `www.yoursite.com/asp.netwebadminfiles/default.aspx` otrzymasz odpowiedź 404 Nie znaleziono. Kod, który obsługuje używa WSAT `Membership` i `Roles` klas w programie .NET Framework do tworzenia, edytowania i usuwania użytkowników i ról. Te klasy przeczytaj informacje o konfiguracji aplikacji sieci web do określenia dostawcy, jakie mają być używane; w [ *Konfigurowanie witryny sieci Web czy korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md) będziemy konfigurować przeglądami książki witryny sieci Web do używania `SqlMembershipProvider` i `SqlRoleProvider` dostawców. To wiązało się dodanie `<membership>` i `<roleManager>` sekcje do `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Należy pamiętać, że `<membership>` i `<roleManager>` sekcje odwołania `SqlMembershipProvider` i `SqlRoleProvider` dostawców w ich `type` atrybutu odpowiednio. Tych dostawców przechowywać informacje o rolach użytkowników i w określonej bazy danych SQL Server. Bazy danych używanej przez tych dostawców jest określona przez `connectionStringName` atrybutu `ReviewsConnectionString`, która jest zdefiniowana w `~/ConfigSections/databaseConnectionStrings.config` pliku. Odwołania, który `databaseConnectionStrings.config` plik w środowisku programistycznym zawiera parametry połączenia z bazą danych programowanie, podczas gdy `databaseConnectionStrings.config` plik produkcyjnych zawiera parametry połączenia w produkcyjnej bazie danych.

Mówiąc, WSAT muszą być dostępne lokalnie przez środowisko deweloperskie i działa z danych użytkownika i roli w bazie danych określona w `databaseConnectionStrings.config` pliku. W związku z tym informacje o parametrach połączenia w każdej zmianie `databaseConnectionStrings.config` plików w środowisku programistycznym możemy użyć WSAT lokalnie do zarządzania użytkownikami i rolami w środowisku produkcyjnym.

Aby zilustrować tę funkcję, otwórz `databaseConnectionStrings.config` plików w programie Visual Studio na środowisko deweloperskie i Zastąp ciąg połączenia bazy danych programowanie parametry połączenia bazy danych w środowisku produkcyjnym. Następnie uruchom WSAT, przejdź na kartę Zabezpieczenia i Dodaj nowego użytkownika o nazwie Sam hasłem "password"! (mniej znaków cudzysłowu). **Rysunek 2** pokazuje ekranu WSAT podczas tworzenia tego konta.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Rysunek 2**: Tworzenie nowego użytkownika o nazwie Sam w środowisku produkcyjnym  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image6.png))

Ponieważ firma Microsoft zmienił się w ciągu połączenia `databaseConnectionStrings.config` aby wskazywały serwer bazy danych produkcyjnej, Sam został dodany jako użytkownik w środowisku produkcyjnym. Aby to sprawdzić, zmiany w ciągu połączenia `databaseConnectionStrings.config` pliku projektowej bazie danych, a następnie odwiedź `Login.aspx` strony w środowisku programistycznym. Podejmą próbę zalogowania się jako Sam (zobacz **rysunek 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Rysunek 3**: nie można zalogować się jako Sam w środowisku programistycznym  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image9.png))

Nie możesz się zarejestrować jako Sam w środowisku programistycznym, ponieważ informacje o koncie użytkownika nie istnieje w lokalnej bazie danych. Jest raczej, został dodany w produkcyjnej bazie danych. Aby to sprawdzić, Wyświetl zawartość `aspnet_Users` tabeli w rozwoju i produkcyjnych bazach danych. W środowisku programistycznym powinna być tylko trzy rekordów użytkowników Scott Jisun i Alicji. Jednak `aspnet_Users` tabeli w produkcyjnej bazie danych ma cztery rekordy: Scott, Jisun Alicja i Sam. W rezultacie Sam zalogować się za pośrednictwem witryny sieci Web w środowisku produkcyjnym, a nie przez środowisko deweloperskie.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Rysunek 4**: Sam mogą zalogować się środowisku produkcyjnym witryny sieci Web  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> Nie zapomnij zmienić parametry połączenia w `databaseConnectionStrings.config` pliku w bazie danych programowanie przez ciąg połączenia po zakończeniu pracy z WSAT w przeciwnym razie zostanie pracy z danymi produkcyjnymi podczas testowania lokacji za pośrednictwem rozwoju środowisko. Należy również pamiętać, że podczas technika, którą właśnie Rozmawialiśmy pozwala używać WSAT do zdalnego zarządzania użytkownikami i rolami, zmiany do żadnych innych WSAT opcji konfiguracji (reguły dostępu, SMTP ustawienia debugowania i śledzenia ustawienia i tak dalej) zmodyfikować `Web.config` pliku. W związku z tym wszelkie zmiany wprowadzone do ustawienia mają zastosowanie do środowiska projektowego, a nie do środowiska produkcyjnego.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Tworzenie niestandardowych użytkowników i stron sieci Web zarządzania ról

WSAT zawiera poza okno systemu zarządzania użytkownikami i rolami, ale może zostać uruchomiona tylko lokalnie i wymaga wprowadzania zmian w ciągu połączenia, aby zarządzać użytkownikami i rolami w środowisku produkcyjnym. Większość witryn sieci Web, które obsługują kont użytkowników także pewną liczbę użytkowników i stron sieci web administracji roli, które umożliwiają administratorom zarządzanie użytkownikami i rolami ze stron w witrynie. Takie strony administracji opartej na sieci web była znacznie ułatwia zarządzanie użytkownikami i rolami i są niezbędne dla witryn, gdzie może występować wiele administratorów lub które nie mają dostępu do lub informacje techniczne, aby uruchomić WSAT za pomocą programu Visual Studio.

Program ASP.NET zawiera szereg formantów wbudowanych związane z logowaniem sieci Web powodujących, że wdrożenie wielu administracyjne należycie łatwym przeciągnij i upuść strony sieci web. Na przykład można utworzyć strony dla administratorów utworzyć nowe konto użytkownika przeciągając formancie CreateUserWizard na stronę i ustawiając kilka właściwości. W rzeczywistości stronę do tworzenia użytkowników w WSAT pokazano **na rysunku 2** używa tego samego formancie CreateUserWizard, które można dodać do stron. Ponadto usługi członkostwo i role, funkcje są dostępne programowo `Membership` i `Roles` klas w programie .NET Framework. Z tych klas można napisać kod do tworzenia, edytowania i usuwania, użytkownicy i role, a także aby dodawać i usuwać użytkowników do ról, aby ustalić, jakie użytkownicy są role oraz wykonywać inne zadania dotyczące użytkowników i ról.

W [ *Konfigurowanie witryny sieci Web czy korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md) dodano stronę, aby `Admin` folder o nazwie `CreateAccount.aspx`. Ta strona umożliwia administratorem, aby dodać nowe konto użytkownika do witryny i określ, czy nowo utworzony użytkownik jest w roli administratora (zobacz **rysunek 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Rysunek 5**: Administratorzy mogą tworzyć nowych kont użytkowników  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image15.png))

Aby bardziej szczegółowe przyjrzeć się tworzenie użytkownika i roli administracyjną stron, wraz z instrukcjami krok po kroku na temat używania `Membership` i `Roles` klas i formanty związane z logowaniem sieci Web ASP.NET, należy przeczytać Mój [bezpieczeństwa witryny sieci Web Samouczki](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Znajdują się wskazówki dotyczące tworzenia stron sieci web do tworzenia nowych kont, tworzenie i zarządzanie rolami, przypisywania użytkowników do ról i innych typowych zadań administracyjnych.

Do zaimplementowania WSAT podobne funkcje w witrynie sieci Web produkcji zawsze można tworzyć serie stron sieci web, które implementuje funkcje WSAT. Aby rozpocząć, zapoznaj się z kodu źródłowego WSAT, który znajduje się w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Inną opcją jest użycie zamiast WSAT Dan Clem, który udostępnia on w jego artykułu, [stopniowych Twoje własne witryny sieci Web narzędzia administracyjnego](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). DaN przeprowadzi czytników przez proces tworzenia niestandardowego narzędzia WSAT przypominającej obejmuje instalację tej aplikacji kod źródłowy dla pobierania (w języku C#) i zawiera instrukcje krok po kroku dotyczące Dodawanie jego WSAT niestandardowych do witryny sieci Web hostowanej.

## <a name="summary"></a>Podsumowanie

Narzędzia administracji witryny sieci Web platformy ASP.NET (WSAT) może służyć w połączeniu z usługi aplikacji członkostwa i ról do zarządzania informacjami o użytkownika i roli dla witryny sieci Web. Niestety WSAT jest dostępny tylko lokalnie, a nie z witryny sieci Web produkcji odwiedzin. Zmieniając parametry połączenia do tworzenia środowiska, aby wskazywał produkcyjną bazę danych można jednak użyć WSAT do zarządzania użytkownikami i rolami w witrynie sieci Web produkcji.

Gdy podejście WSAT zapewnia szybki i łatwy sposób zarządzać użytkownikami i rolami, wymaga uruchamianie WSAT z programu Visual Studio, a także tymczasowej zmiany do ciągu połączenia. WSAT pozwala szybko do zarządzania użytkownikami i rolami w środowisku produkcyjnym, ale jest skomplikowane i nie działa dobrze w przypadku witryn sieci Web z wielu administratorów lub administratorów, którzy nie mają lub nie są jeszcze znane z programu Visual Studio i WSAT. W przypadku z tego względu większość witryn sieci Web, które obsługują kont użytkowników zawiera zestaw stron administracyjnych sieci web. Zestaw stron sieci web eliminuje potrzebę stosowania WSAT i używane przez różnych użytkowników administracyjnych z dowolnego komputera.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Badanie ASP. Członkostwo w sieci, ról i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Wycofanie własne narzędzia administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Omówienie narzędzia do administrowania witryny sieci Web](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx)
- [Samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[Poprzednie](precompiling-your-website-vb.md)
