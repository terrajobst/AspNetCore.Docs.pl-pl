---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Użytkownicy i role w produkcyjnej witrynie internetowej (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET witryny sieci Web Administracja narzędzia (WSAT) udostępnia interfejs użytkownika oparty na sieci web do konfigurowania ustawień członkostwo i role oraz do tworzenia, edytowania,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b2a5ea84c5c16ad5bf4e3041d31ad29660d965f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380379"
---
<a name="users-and-roles-on-the-production-website-vb"></a>Użytkownicy i role w produkcyjnej witrynie internetowej (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> ASP.NET witryny sieci Web Administracja narzędzia (WSAT) zapewnia interfejs użytkownika oparty na sieci web, do konfigurowania ustawień członkostwo i role oraz tworzenie, edytowanie i usuwanie użytkowników i ról. Niestety WSAT działa tylko po odwiedzeniu z hostem lokalnym, co oznacza, że narzędzie administracyjne w produkcyjnej witrynie internetowej nie może się połączyć za pośrednictwem przeglądarki. Dobra wiadomość jest istnieją obejścia, które umożliwiają zarządzanie użytkownikami i rolami w środowisku produkcyjnym. W tym samouczku patrzy na tego rodzaju obejścia i innym osobom.


## <a name="introduction"></a>Wprowadzenie

Program ASP.NET 2.0 wprowadzono szereg *usługi aplikacji*, zestawu bloków konstrukcyjnych usługi, które można dodać do aplikacji sieci web, które są. Dodaliśmy usługi członkostwa i ról do witryny sieci Web przeglądy książki z powrotem w [ *Konfigurowanie witryny sieci Web, korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md). Usługą członkostwa ułatwia tworzenie i zarządzanie nimi kont użytkowników. usługi ról udostępnia interfejs API na potrzeby kategoryzowania użytkowników w grupach. Witryna przeglądy książki ma trzy konta użytkowników — Scott, Jisun i Alicja — i jedną rolę administratora, Scott i Jisun w roli administratora.

ASP. Usługi aplikacji w sieci nie są związane z konkretnej implementacji. Zamiast tego należy wydać polecenie usługi aplikacji, aby użyć określonego *dostawcy*, i że dostawcy implementuje usługę za pomocą określonej technologii. Skonfigurowaliśmy przeglądy książki aplikacji sieci web, użyj `SqlMembershipProvider` i `SqlRoleProvider` dostawcami usług członkostwa i ról. Tych dwóch dostawców przechowywania informacji o kontach i roli użytkownika w bazie danych programu SQL Server i są najczęściej używane dostawców internetowych aplikacji sieci web hostowanych w sieci web firma zapewniająca hosting.

Typowe wyzwanie dla deweloperów korzystających z usług członkostwa i ról jest zarządzany, użytkownicy i role w środowisku produkcyjnym. Jak można usunąć konto użytkownika z witryny sieci Web produkcji, dodać nową rolę lub dodać istniejącego użytkownika do istniejącej roli? W tym samouczku przedstawiono różnych technik do zarządzania użytkownikami i rolami w produkcyjnej witrynie internetowej.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Za pomocą narzędzia do administrowania witryną sieci Web platformy ASP.NET

Program ASP.NET zawiera [narzędzie Administratorskie witryny sieci Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) ułatwia tworzenie i zarządzanie nimi kont użytkowników i ról oraz do określania reguł autoryzacji użytkowników i opartą na roli. Aby użyć WSAT, kliknij ikonę Konfiguracja ASP.NET w Eksploratorze rozwiązań lub przejść do menu witryny sieci Web lub projektu i wybierz opcję Konfiguracja platformy ASP.NET. Każda z tych metod otworzy w przeglądarce sieci web i wskazuje on WSAT pod adresem, takich jak: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT jest podzielony na trzy sekcje:

- **Zabezpieczenia** — Zarządzanie użytkownikami, ról i reguł autoryzacji.
- **ApplicationConfiguration** — zarządzanie &lt;appSettings&gt; i ustawień SMTP, w tym miejscu. Użytkownik może również nastavit aplikaci offline i zarządzanie debugowania i śledzenia ustawienia w tym miejscu, a także określić domyślną stronę błędu niestandardowego.
- **ProviderConfiguration** — Konfigurowanie dostawców dla usług aplikacji.

Sekcja zabezpieczeń (objętego **rysunek 1**) zawiera linki do tworzenia nowych użytkowników, zarządzanie użytkownikami, tworzenie i zarządzanie rolami oraz tworzenie i zarządzanie regułami dostępu. W tym miejscu można dodać nową rolę systemu, usuwać użytkowników, lub Dodaj lub Usuń role z określonego konta użytkownika.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Rysunek 1**: sekcja zabezpieczeń WSAT zawiera opcje do zarządzania użytkownikami i rolami  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image3.png))

Niestety WSAT jest dostępny tylko lokalnie. Nie można znaleźć WSAT w witrynie internetowej zdalnego w środowisku produkcyjnym; w przypadku odwiedzenia `www.yoursite.com/asp.netwebadminfiles/default.aspx` otrzymasz odpowiedź 404 Nie znaleziono. Kod, który obsługuje używa WSAT `Membership` i `Roles` klasy w .NET Framework do tworzenia, edytowania i usuwania użytkowników i ról. Informacje o konfiguracji aplikacji sieci web, aby określić, jakie dostawcy, które mają być używane; zapoznaj się z tych klas ponownie [ *Konfigurowanie witryny sieci Web, korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md) będziemy konfigurować książki, przeglądy witryny sieci Web do użycia `SqlMembershipProvider` i `SqlRoleProvider` dostawców. To tytułu, dodając `<membership>` i `<roleManager>` sekcje do `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Należy pamiętać, że `<membership>` i `<roleManager>` sekcje odwołanie `SqlMembershipProvider` i `SqlRoleProvider` dostawców w ich `type` atrybutu, odpowiednio. Ci dostawcy przechowywać użytkownika oraz informacje o rolach w określonej bazie danych programu SQL Server. Bazy danych używanej przez takich dostawców jest określona przez `connectionStringName` atrybutu `ReviewsConnectionString`, który jest zdefiniowany w `~/ConfigSections/databaseConnectionStrings.config` pliku. Pamiętamy `databaseConnectionStrings.config` plik w środowisku programistycznym zawiera parametry połączenia w celu tworzenia bazy danych, natomiast `databaseConnectionStrings.config` plik w środowisku produkcyjnym zawiera parametry połączenia do produkcyjnej bazy danych.

Mówiąc, WSAT muszą być dostępne lokalnie za pośrednictwem środowiska programistycznego i działa przy użyciu informacji użytkownika i roli bazy danych określonej w `databaseConnectionStrings.config` pliku. W związku z tym jeśli wprowadzimy zmiany w ciągu połączenia w `databaseConnectionStrings.config` plików w środowisku deweloperskim możemy użyć WSAT lokalnie do zarządzania użytkownikami i rolami w środowisku produkcyjnym.

Aby zilustrować tę funkcję, otwórz `databaseConnectionStrings.config` pliku w programie Visual Studio w środowisku deweloperskim i Zastąp parametry połączenia bazy danych rozwoju parametry połączenia bazy danych w środowisku produkcyjnym. Następnie uruchom WSAT, przejdź na kartę Zabezpieczenia i dodać nowego użytkownika o nazwie Sam przy użyciu hasła "password"! (mniej znaków cudzysłowu). **Rysunek 2** wyświetlony ekran WSAT, podczas tworzenia tego konta.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Rysunek 2**: Tworzenie nowego użytkownika o nazwie Sam w środowisku produkcyjnym  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image6.png))

Ponieważ zmiana parametrów połączenia w `databaseConnectionStrings.config` wskaż serwer produkcyjny w bazie danych Sam został dodany jako użytkownika w środowisku produkcyjnym. Aby to sprawdzić, należy zmienić parametry połączenia w `databaseConnectionStrings.config` plik rozwoju bazy danych, a następnie odwiedź `Login.aspx` strony w środowisku programistycznym. Spróbuj zalogować się jako Sam (zobacz **rysunek 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Rysunek 3**: nie można zalogować się jako Sam w środowisku programistycznym  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image9.png))

Nie możesz zalogować jako Sam w środowisku programistycznym, ponieważ informacje o koncie użytkownika nie istnieje w lokalnej bazie danych. Jest raczej, została dodana do produkcyjnej bazy danych. Aby to sprawdzić wyświetlanie zawartości `aspnet_Users` tabeli w bazach danych środowisk deweloperskich i produkcyjnych. W środowisku programistycznym powinien istnieć tylko dla trzech rekordów dla użytkowników, Scott, Jisun i Alicji. Jednak `aspnet_Users` tabela w produkcyjnej bazy danych ma cztery rekordy: Scott, Jisun, Alice i Sam. W związku z tym Sam zalogować się za pośrednictwem witryny internetowej w środowisku produkcyjnym, ale nie przy użyciu środowiska programistycznego.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Rysunek 4**: Sam zalogować się w produkcyjnej witrynie internetowej  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> Nie zapomnij zmienić parametry połączenia w `databaseConnectionStrings.config` pliku w bazie danych rozwoju przez ciąg połączenia po zakończeniu pracy z WSAT, w przeciwnym razie będziesz pracować z danymi produkcyjnymi podczas testowania witryny za pomocą programowania środowisko. Należy pamiętać, że gdy technika, którą właśnie Omówiliśmy pozwala nam korzystać WSAT do zdalnego zarządzania użytkownikami i rolami, zmiany do dowolnych innych WSAT konfiguracji opcji (reguły dostępu, SMTP ustawienia, debugowanie oraz śledzenie ustawień i tak dalej) modyfikować `Web.config` pliku. W związku z tym wszelkie zmiany wprowadzone w ustawieniach się do środowiska deweloperskiego, a nie w środowisku produkcyjnym.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Tworzenie niestandardowych użytkowników i stron sieci Web zarządzania ról

WSAT zawiera poza pole systemu zarządzania użytkownikami i rolami, ale można uruchamiać tylko lokalnie i wymaga wprowadzania zmian w informacje o parametrach połączenia, aby można było zarządzać użytkownicy i role w środowisku produkcyjnym. Większość witryn sieci Web, które obsługują konta użytkowników są także pewną liczbę użytkowników i stron sieci web administracji roli, które umożliwiają administratorom zarządzanie użytkownikami i rolami na stronach w obrębie witryny. Tego rodzaju strony administracji opartej na sieci web znacznie ułatwiają zarządzanie użytkownikami i rolami i mają zasadnicze znaczenie dla witryn w przypadku, gdy może istnieć wiele Administratorzy i Administratorzy, którzy nie mają dostępu do lub wiedzę techniczną do uruchomienia WSAT przy użyciu programu Visual Studio.

Program ASP.NET zawiera szereg wbudowanych formantów skojarzone z logowaniem w sieci Web, wchodzące w implementacji wiele z tych administracyjne stron sieci web, tak łatwe przeciąganie i upuszczanie. Na przykład można utworzyć strony dla administratorów utworzyć nowe konto użytkownika, przeciągając kontrolki CreateUserWizard na stronę i ustawiając kilka właściwości. W rzeczywistości, strona tworzenia użytkowników w WSAT objętego **na rysunku 2** korzysta z tej samej kontrolki CreateUserWizard, które można dodać do stron sieci. Ponadto, funkcje usługi członkostwa i ról są dostępne programowo za pośrednictwem `Membership` i `Roles` klas w programie .NET Framework. Z tych klas można napisać kod do tworzenia, edytowania i usuwania użytkowników i ról, a także dodawać lub usuwać użytkowników z ról, aby ustalić, jakie użytkownicy znajdują się w jaki ról i wykonywać inne zadania związane z użytkownikiem i do roli.

W [ *Konfigurowanie witryny sieci Web, korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md) dodano stronę, aby `Admin` folder o nazwie `CreateAccount.aspx`. Ta strona umożliwia administratorowi dodawanie nowego konta użytkownika do witryny i określ, czy nowo utworzony użytkownik ma rolę administratora (zobacz **rysunek 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Rysunek 5**: Administratorzy mogą tworzyć nowych kont użytkowników  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](users-and-roles-on-the-production-website-vb/_static/image15.png))

Aby uzyskać bardziej szczegółowy widok tworzenia użytkownika i roli stron administracyjnych, wraz z instrukcjami krok po kroku na temat korzystania z `Membership` i `Roles` klasy i formanty związane z logowaniem sieci Web programu ASP.NET, należy przeczytać Moje [bezpieczeństwa witryny sieci Web Samouczki](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Znajduje się tam wskazówki na temat tworzenia stron sieci web do tworzenia nowych kont, tworzenie i zarządzanie rolami, przypisywanie użytkowników do ról i innych typowych zadań administracyjnych.

Do zaimplementowania funkcjonalności przypominającej WSAT w produkcyjnej witrynie internetowej można zawsze tworzyć własne szeregu stron sieci web, które implementują funkcje WSAT. Aby ułatwić rozpoczęcie pracy, zapoznaj się z kodu źródłowego WSAT, który znajduje się w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Innym rozwiązaniem jest użycie zamiast WSAT Dan Clem, który opowiada w swoim artykule [stopniowe Twojej własnej witryny sieci Web narzędzie administracyjne](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). DaN przedstawiono czytelnicy przez proces tworzenia niestandardowego narzędzia WSAT podobne, kod źródłowy swoich aplikacji do pobrania (w języku C#) i zawiera instrukcje krok po kroku dotyczące dodawania jego WSAT niestandardowych do witryny sieci Web hostowanej.

## <a name="summary"></a>Podsumowanie

ASP.NET witryny sieci Web Administracja narzędzia (WSAT) może służyć w połączeniu z usługi aplikacji członkostwa i ról sekcją do zarządzania informacjami o użytkownika i roli w swojej witryny sieci Web. Niestety WSAT jest dostępna tylko lokalnie, a nie odwiedził w produkcyjnej witrynie internetowej. Jednakże, zmieniając parametry połączenia w trakcie opracowywania środowiska, aby wskazywał produkcyjną bazę danych można użyć WSAT do zarządzania użytkownikami i role w produkcyjnej witrynie internetowej.

Chociaż podejście WSAT zapewnia szybki i łatwy sposób zarządzania użytkownikami i rolami, będzie wymagało uruchamianie WSAT z programu Visual Studio, a także tymczasowej zmiany do informacji o parametrach połączenia. WSAT zapewnia szybki sposób zarządzania użytkownikami i rolami w środowisku produkcyjnym, ale jest kłopotliwe i nie działa dobrze w przypadku witryn sieci Web z wielu administratorów lub administratorów, którzy nie mają lub nie jest zaznajomiony z programu Visual Studio i WSAT. Z tego względu większość witryn sieci Web, które obsługują konta użytkowników zawierają zestaw stron administracyjnych sieci web. Zestaw stron sieci web eliminuje potrzebę stosowania WSAT i używane przez różnych użytkowników administracyjnych z dowolnego komputera.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Badanie ASP. Członkostwo w sieci, ról i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Stopniowe własne narzędzia do administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Omówienie narzędzia do administrowania witryny sieci Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Poprzednie](precompiling-your-website-vb.md)
