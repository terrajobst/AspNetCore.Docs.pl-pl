---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET Hosting opcje (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Aplikacji sieci web ASP.NET zwykle są zaprojektowane, utworzony i przetestować w środowisku projektowym lokalnych i muszą zostać wdrożone do o środowisku produkcyjnym..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 66431cadac6011bbf247b24a08b3aacec928a715
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-hosting-options-c"></a>Opcje hostingu ASP.NET (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Aplikacje sieci web ASP.NET zwykle są zaprojektowane, tworzone i testowane w lokalne Środowisko deweloperskie i potrzeb dotyczących można wdrożyć w środowisku produkcyjnym, gdy jest ono gotowe do wydania. Ten samouczek zawiera omówienie procesu wdrażania i stanowi wprowadzenie do tego samouczka serii.


## <a name="introduction"></a>Wprowadzenie

Aplikacje sieci Web zwykle są zaprojektowane, tworzone i testowane w środowisku projektowania, który jest dostępny tylko dla programistów pracy w witrynie. Gdy aplikacja jest gotowa do wydania, jest przenoszony do środowiska produkcyjnego gdzie lokacji są dostępne dla wszystkich użytkowników Internetu. Ten proces wdrażania wprowadzono wiele wyzwań:

- Do środowiska produkcyjnego musi istnieć i należy prawidłowo skonfigurować przed wdrożeniem aplikacji ASP.NET; Ponadto środowisko produkcji muszą być przechowywane na bieżąco najnowsze poprawki zabezpieczeń.
- Poprawny zestaw plików znaczników, pliki kodu i pliki obsługi musi być kopiowane z Środowisko deweloperskie do środowiska produkcyjnego. W przypadku aplikacji opartej na danych może to wymagać kopiowanie schematu bazy danych i/lub danych, jak również.
- Może to być konfiguracji różnice między dwoma środowiskami. Serwer ciągu lub e-mail połączenia bazy danych używane w środowisku programistycznym będzie prawdopodobnie inny niż w środowisku produkcyjnym. Co więcej zachowanie aplikacji może zależeć od środowiska. Na przykład po wystąpieniu błędu w rozwoju szczegóły błędu mogą być wyświetlane na ekranie, ale po wystąpieniu błędu w środowisku produkcyjnym, zamiast tego powinna być wyświetlana strona błędu przyjazną dla użytkownika, a szczegóły błędu pocztą e-mail do deweloperów.

W celu uniknięcia pierwszego wyzwania - konfigurowania i konserwacji do środowiska produkcyjnego — wiele osób i firm zewnętrzny środowisk produkcyjnych do *dostawców hostingu w sieci web*. Dostawcy usług hosta sieci web to firma, która zarządza środowiska produkcyjnego w Twoim imieniu. Brak niezliczonych sieci web dostawców hosta z różnymi ceny i poziomów usługi; w sekcji "Wyszukiwanie w sieci Web hosta dostawcy" porady na temat lokalizowania dostawcy usług.

Jest to pierwszy z serii samouczków, z którymi przyjrzeć się czynności związanych z wdrażaniem aplikacji sieci web ASP.NET w środowisku produkcyjnym, zarządzane przez dostawcę hosta sieci web. W trakcie tego samouczka, zostaną omówione:

- Co pliki potrzebne do wdrożenia dla dostawcy hosta sieci web.
- Narzędzia do usprawnienie procesu wdrażania.
- Jak wdrożyć bazę danych.
- Wskazówki dotyczące wdrażania bazy danych, która używa [dostawcy SQL na podstawie członkostwa oraz ról](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), oraz sposoby naśladować narzędzia administracyjnego witryny sieci Web w środowisku produkcyjnym.
- Strategie sprawnie aktualizowania bazy danych w środowisku produkcyjnym ze zmianami wprowadzonymi w czasie projektowania.
- Techniki rejestrowania błędów, które występują w produkcji i sposobów, aby powiadomić deweloperzy po wystąpieniu błędu.

Te samouczki są dostosowane zwięzły i zawierają instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie prowadzą użytkownika przez proces. W tym samouczku inauguracyjnym zawiera omówienie procesu wdrażania platformy ASP.NET i porad na wyszukiwanie dostawcy usług hosta sieci web. Dzieła!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Omówienie procesu wdrażania platformy ASP.NET

Mówiąc wdrażanie aplikacji ASP.NET obejmuje następujące trzy kroki:

1. Konfigurowanie aplikacji sieci web, serwer sieci web i bazy danych w środowisku produkcyjnym.
2. Synchronizuj stron ASP.NET, kod plików zestawów w `Bin` folder i pliki pomocnicze związane z HTML, takich jak pliki CSS i JavaScript.
3. Synchronizuj schematu bazy danych i/lub danych.

Informacje o konfiguracji dla aplikacji sieci web znajduje się w `Web.config` pliku i zawiera parametry połączenia bazy danych, Obsługa kryteriów, reguły ponowne zapisywanie adresów URL, błędów i informacji o serwerze poczty e-mail. Te informacje są często różnych aplikacji do rozwoju i tej samej aplikacji w środowisku produkcyjnym. Na przykład przy tworzeniu aplikacji jest najlepiej użyć projektowej bazie danych, dzięki czemu nie testujesz przed produkcyjną bazę danych. W związku z tym parametry połączenia bazy danych różnią się zazwyczaj między aplikacjami rozwoju i produkcji. Z powodu różnic część wdrożenia konieczne jest wprowadzenie zmian do informacji o konfiguracji aplikacji sieci web.

Oprócz zmian w konfiguracji aplikacji sieci web krok 1 również może pociągać za sobą konfiguracji serwera sieci web i bazy danych. Na przykład jeśli strony ASP.NET umożliwia tworzenie lub usuwanie plików z katalogu na serwerze sieci web serwera sieci web trzeba skonfigurować tak, aby umożliwić modyfikacje systemu plików. Podobnie może być uprawnienia lub uwierzytelniania ustawień, które muszą zostać wprowadzone w bazie danych.


Krok 2 polega na synchronizacji zestawu niezbędne stron ASP.NET i pliki obsługi między środowisk projektowania i produkcji. Określonego zestawu ASP. Plików związanych z sieci, które muszą być synchronizowane między dwoma środowiskami zależy od typu projektu utworzone w programie Visual Studio i jest dyskusji w następnym samouczku [ *określania co pliki potrzebne do wdrożenia*](determining-what-files-need-to-be-deployed-cs.md). Samouczki trzeci i czwarty - [ *wdrażanie Your lokacji przy użyciu FTP* ](deploying-your-site-using-an-ftp-client-cs.md) i [ *wdrażanie Your lokacji za pomocą programu Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -Sprawdź różnych narzędzi i technik do synchronizowania tych plików.

Podczas tworzenia aplikacji opartych na danych są zwykle dwóch baz danych używane: jeden dla rozwoju i jeden w środowisku produkcyjnym. Podczas tworzenia schemat programowanie bazy danych mogą być modyfikowane w celu uwzględnienia nowych tabel, kolumn procedur składowanych i wyzwalaczy lub mogą zostać zmodyfikowane w celu usuń lub zmień istniejące obiekty bazy danych. Między czas tych zmian i czasu, w których aplikacja jest wdrażana w środowisku produkcyjnym projektowanie i produkcyjne bazy danych nie są zsynchronizowane. To asynchrony musi być stały podczas procesu wdrażania. Te problemy zostaną sprawdzone w przyszłości samouczki.

## <a name="finding-a-web-host-provider"></a>Wyszukiwanie dostawcy hosta sieci Web

Można wdrożyć aplikacji ASP.NET do dowolnego serwera sieci web, który ma .NET Framework i Internet Information Services (IIS). Może udostępniać lokacji z komputerem, przy założeniu, że ma połączenie szerokopasmowe z Internetem i wiedzieć, jak skonfigurować router tak, aby umożliwić przychodzących żądań sieci web. Może również udostępnić lokacji z komputera w sieci intranet, jak wiele firm. Fokus te samouczki, jednak jest hostem witryny sieci Web z dostawcą hosta sieci web.

> [!NOTE]
> [Usługi IIS](https://www.iis.net/) to serwer sieci web klasy przedsiębiorstwa firmy Microsoft. Ten składnik jest dostarczany z wersji głównej z systemem innym niż Windows, takich jak Windows Server 2008 i niektóre wersje systemu Windows Vista. Nie trzeba zainstalować usługi IIS do obsługi aplikacji programu ASP.NET w środowisku projektowania, tylko Visual Studio zawiera sieci Web ASP.NET Development Server. Jednak sieci Web ASP.NET Development Server akceptuje tylko połączeń lokalnych i w związku z tym nie można używać w środowisku produkcyjnym.


Przed wdrożeniem witryny dostawcy hosta sieci web należy najpierw wybrać jakie firmy do prowadzenia działalności z. Brak niezliczonych hostingu firmy w witrynie marketplace; Wyszukaj "hostingu w sieci web firmy" zwraca więcej niż pięć milionów wyniki. Jak można znaleźć ten, który jest dla Ciebie odpowiednia? Aparat wyszukiwania ulubionych jest dobrym miejscem początkowy, jak witryn sieci Web, takich jak [TopHosts](http://www.tophosts.com/) i [HostCritique](http://www.hostcritique.net/), który porównania i kontrastu różnych usług hostingu. I poinformować współpracowników i współpracowników jest proszony o podanie żadnych rekomendacji do przedstawienia; Możesz również poprosić zaleceń w [Hosting Otwórz Forum](https://forums.asp.net/158.aspx) tutaj w [fora ASP.NET](https://forums.asp.net/).

Firm świadczących usługi hostingu sieci Web zwykle oferować udostępnionego plany obsługi i plany obsługi w wersji dedykowanej. Udostępniane hosting jednej sieci hostów serwerów dziesiątek, jeśli nie setki różnych witryn sieci Web. Z dedykowanym hosting dzierżawy jest komputer z firmy, która obsługuje witryny i witryny sieci Web. Plan hostingu udostępnionego mogą obejmować obsługi stron ASP.NET, możliwość korzystania z bazy danych programu Microsoft Access, 5 GB miejsca na dysku i 100 GB, przepustowości ruchu dla $9,95 miesięcznie. Innego udostępnionego plan hostingu może obejmować obsługi stron ASP.NET, dostęp do serwera bazy danych programu Microsoft SQL Server 2008, 10 GB miejsca na dysku i 250 GB, przepustowości ruchu cenie od 19,95 USD miesięcznie. Plany obsługi w wersji dedykowanej są zwykle znacznie droższe, kosztów kilka kwoty kilkuset na miesiąc, ale także zapewniają lepszą wydajność i lepszą kontrolę niż udostępnionego hosting opcje. Jakie planu, możesz wybrać zależy budżetu, ilość ruchu odbiera witryny sieci Web, i potrzebne funkcje, należy przewidzieć.

Dwa istotne zagadnienia dotyczące wybierania dostawcy hosta sieci web są obsługi klienta i jakości usług. Jeśli masz pytania lub problem z konfiguracją, jak długo trwa przesyłanie problem do działu pomocy technicznej hosta sieci web, do momentu uzyskania odpowiedzi? Jak niezawodnej są usługami firmy? Czy często mają awarie bazy danych? Jak często serwer e-mail Przejdź w trybie offline? Należy zawsze Pytaj firmy zawierają szczegółowe informacje o ich czas pracy i uzyskiwanie informacji o zasadach usługi ich klienta, ale jest bardziej surefire sposób w celu uzyskania informacji zwrotnych dotyczących bieżących i starszych klientów, można to zrobić za pomocą online fora, grupy dyskusyjne i poczty e-mail listservs.

> [!NOTE]
> Niektóre firm świadczących usługi hostingu sieci web firmy skupić się na stosie określonej technologii, takich jak .NET lub [światła](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, i **P** HP), dlatego należy upewnić się, że firmy, należy wybrać obsługuje aplikacje ASP.NET. Ponadto sprawdź obsługuje wersję programu ASP.NET, które są używane do tworzenia aplikacji. A jeśli tworzysz aplikację opartych na danych, upewnij się, że host sieci web oferuje ten sam serwer bazy danych i wersji, którego używasz.


## <a name="summary"></a>Podsumowanie

Aplikacji sieci web ASP.NET zwykle są zaprojektowane, tworzone i testowane w środowisku lokalnym programowania. Gdy wersja jest gotowe do wydania, jest przenoszony do środowiska produkcyjnego. Mimo że jest możliwa do hosta ASP.NET witryn sieci Web na komputerze osobistym, lub na serwerach w firmie, wiele firm i osób wybierz zewnętrzny hosting dostawcy hosta sieci web.

Ten samouczek serii sprawdza kroki dotyczące wdrażania aplikacji ASP.NET do dostawcy hosta sieci web, eksploracji najczęstsze wyzwania. W tym samouczku oferowane ogólne omówienie procesu wdrażania platformy ASP.NET i nadać porady do znajdowania dostawcy hosta odpowiedniego sieci web. Następny samouczek analizuje pliki związane z ASP.NET muszą ma zostać skopiowany do środowiska produkcyjnego, wdrażając witryny sieci Web.

Programowanie przyjemność!

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Dalej](determining-what-files-need-to-be-deployed-cs.md)
