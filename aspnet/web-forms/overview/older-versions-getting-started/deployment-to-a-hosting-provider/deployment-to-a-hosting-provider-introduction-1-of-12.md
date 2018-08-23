---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio: wprowadzenie - 1 z 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9dacafaacdab12b8005cb6073647ae526cefcfb4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753318"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio: wprowadzenie - 1 z 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web.
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Te Samouczki pomagają kroków wdrażania najpierw w usługach IIS na lokalnym komputerze deweloperskim do testowania, a następnie do dostawcy hostingu innych firm. Aplikacja, która będzie wdrażana korzysta z bazy danych aplikacji i bazy danych członkostwa ASP.NET. Uruchom przy użyciu programu SQL Server Compact i wdrożenie programu SQL Server Compact i kolejnych samouczkach dowiesz się, jak wdrażać zmiany w bazie danych i jak przeprowadzić migrację do programu SQL Server.
> 
> Samouczki przyjęto założenie, że wiesz, jak pracować z programem ASP.NET w programie Visual Studio. Jeśli nie istnieje, jest dobrym miejscem do rozpoczęcia [podstawowego samouczka formularzy sieci Web platformy ASP.NET](../tailspin-spyworks/tailspin-spyworks-part-1.md) lub [podstawowego samouczka MVC ASP.NET](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Omówienie

Te Samouczki pomagają kroków wdrażania najpierw w usługach IIS na lokalnym komputerze deweloperskim do testowania, a następnie do dostawcy hostingu innych firm. Aplikacja, która będzie wdrażana korzysta z bazy danych aplikacji i bazy danych członkostwa ASP.NET. Uruchom przy użyciu programu SQL Server Compact i wdrożenie programu SQL Server Compact i kolejnych samouczkach dowiesz się, jak wdrażać zmiany w bazie danych i jak przeprowadzić migrację do programu SQL Server.

Liczba samouczków — 11 we wszystkich oraz stronę rozwiązywania problemów — może uniemożliwić procesu wdrażania wydają się czasochłonnym zadaniem. W rzeczywistości ale podstawowe procedury dotyczące wdrażania witryny tworzą stosunkowo niewielką część zestawie samouczków. Jednak w rzeczywistych warunkach często konieczne informacje dotyczące pewnych aspektów dodatkowe małą ale ważną wdrożenia — na przykład, ustawianie uprawnień do folderów na serwerze docelowym. Wprowadziliśmy wiele z tych dodatkowych metod w samouczkach, za pomocą nadzieję, że informacje, które mogą uniemożliwić pomyślne wdrożenie rzeczywistej aplikacji nie może pozostać samouczków.

Samouczki są zaprojektowane do uruchamiania w sekwencji, a każda część kompilacji w poprzedniej części. Jednakże, możesz pominąć części, które nie są odpowiednie do swojej sytuacji. (Pomijanie części może wymagać dostosowania procedury opisane w kolejnych samouczkach.)

## <a name="intended-audience"></a>Odbiorców

Samouczki mają na celu deweloperów platformy ASP.NET, którzy pracują w małych organizacjach lub w innych środowiskach gdzie:

- Proces ciągłej integracji (zautomatyzowane kompilacje i wdrożenia) nie jest używany.
- W środowisku produkcyjnym jest dostawcą hostingu innych firm.
- Jedna osoba zwykle wypełnia wiele ról (ta sama osoba odpowiedzialny za rozwój, testy i wdraża).

W środowiskach przedsiębiorstw jest bardziej typowego w celu zaimplementowania procesów ciągłej integracji, a w środowisku produkcyjnym jest ono zwykle hostowane przez serwery firmy. Różne osoby również zwykle wykonują różne role. Aby uzyskać informacji na temat wdrażania w przedsiębiorstwie, zobacz [wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organizacje o dowolnym rozmiarze, można także wdrożyć aplikacje sieci web na platformie Azure, a większość z procedurami przedstawionymi w tych samouczkach mają również zastosowanie do aplikacji sieci Web usługi aplikacji Azure. Wprowadzenie do platformy Azure, zobacz [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Dostawca hostingu wyświetlane w ramach samouczków

Samouczki prowadzą użytkownika przez proces konfigurowania konta z firmy hostingowej i wdrażanie aplikacji do tego dostawcy hostingu. Określone firmę hostingową został wybrany tak, aby samouczków można zilustrować kompleksowe środowisko wdrażania w aktywnej witrynie sieci Web. Każda firma hostingu zapewnia różne funkcje i środowisko wdrażania serwerów różni się nieco; jednak procesu opisanego w tym samouczku jest typowe dla całego procesu.

Dostawcy hostingu używane w tym samouczku Cytanium.com, jest jednym z wielu, które są dostępne, a jej użycie w ramach tego samouczka nie stanowi poręczenia lub zalecenia.

## <a name="deploying-web-site-projects"></a>Wdrażanie projektów witryn internetowych

Uniwersytet firmy Contoso jest projektu aplikacji sieci web programu Visual Studio. Większość metod wdrażania i narzędzia, przedstawione w tym samouczku nie dotyczą [projektów witryny sieci Web](https://msdn.microsoft.com/library/dd547590.aspx). Aby uzyskać informacje o sposobie wdrażania projektów witryny sieci web, zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Wdrażanie projektów platformy ASP.NET MVC

W tym samouczku wdrożyć projekt formularzy sieci Web ASP.NET, ale wszystko dowiesz się, jak to zrobić stosuje się do platformy ASP.NET MVC oraz. Projekt programu Visual Studio MVC jest po prostu innej formy projektu aplikacji sieci web. Jedyną różnicą jest to, że jeśli wdrażasz do dostawcy usług hosta, który nie obsługuje platformy ASP.NET MVC lub wersji docelowej ją, należy się upewnić, że masz zainstalowaną odpowiednią ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) lub [MVC 4](http://nuget.org/packages/aspnetmvc)) Pakietu NuGet w projekcie.

## <a name="programming-language"></a>Język programowania

Przykładowa aplikacja używa języka C#, ale samouczków nie wymagają znajomości języka C# i techniki wdrażania, wyświetlane według samouczków nie są specyficzne dla języka.

## <a name="troubleshooting-during-this-tutorial"></a>Rozwiązywanie problemów w ramach tego samouczka

Po wystąpieniu błędu podczas wdrażania lub witrynę wdrożoną nie działa poprawnie, komunikaty o błędach nie zawsze stanowią rozwiązanie. Do udzielenia odpowiedzi na kilka typowych scenariuszy problem [Rozwiązywanie problemów z strona referencyjna](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) jest dostępna. Jeśli otrzymasz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczków, pamiętaj sprawdzić stronę rozwiązywania problemów.

## <a name="comments-welcome"></a>Uwagi — Zapraszamy!

Komentarze dotyczące samouczków są powitalnej, a po zaktualizowaniu samouczka wszelkich starań, będzie nawiązywane w przypadku uwzględnienia poprawki konta lub sugestie dotyczące ulepszeń, które znajdują się w samouczku komentarzy.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz Windows 7 lub nowszy, a jeden z następujących produktów zainstalowany na komputerze:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 z dodatkiem SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Jeśli masz program Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer Express 2010 z dodatkiem SP1, zainstaluj również następujące produkty:

- [Zestaw Azure SDK dla platformy .NET (VS 2010 z dodatkiem SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (w tym aktualizacji publikowania w sieci Web)
- [Microsoft Visual Studio 2010 SP1 Tools dla programu SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Niektóre inne oprogramowanie jest wymagane w celu ukończenia tego samouczka, ale nie trzeba tego jeszcze załadowany. Samouczek przeprowadzi Cię kroki instalacji, gdy ich potrzebujesz.

## <a name="downloading-the-sample-application"></a>Pobieranie przykładowej aplikacji

W przypadku wdrażania aplikacji o nazwie Contoso University i został już utworzony dla Ciebie. Jest uproszczoną wersję university witryny sieci web, luźno na podstawie Contoso University aplikacji opisane w [samouczki platformy Entity Framework w witrynie programu ASP.NET](https://asp.net/entity-framework/tutorials).

Jeśli masz zainstalowane warunki wstępne, Pobierz [aplikacji sieci web firmy Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). *Zip* plik zawiera wiele wersji projektu i pliku PDF, która zawiera wszystkie samouczki 12. Do pracy, kolejne kroki tego samouczka, rozpoczynać się ContosoUniversity Begin. Aby zobaczyć, jak wygląda projektu na końcu samouczków, otwórz ContosoUniversity-End. Aby zobaczyć, jak wygląda projektu przed migracją do pełnej wersji programu SQL Server w ramach samouczka 10, otwórz ContosoUniversity AfterTutorial09.

Aby przygotować się do pracy za pośrednictwem kroki samouczka, należy zapisać ContosoUniversity Begin do folderu, niezależnie od użycia podczas pracy z projektami Visual Studio. Domyślnie jest to następujący folder:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Do zrzutów ekranu w tym samouczku, folderu projektu znajduje się w katalogu głównym na `C`: dysk.)

Uruchom program Visual Studio, otwórz projekt, a następnie naciśnij klawisz CTRL-F5, aby go uruchomić.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Na stronach witryny sieci Web są dostępne z paska menu i umożliwiają wykonywanie następujących funkcji:

- Wyświetlanie statystyk dla uczniów (strona informacje).
- Wyświetlanie, edytowanie, usuwanie i dodanie uczniów.
- Wyświetlanie i edytowanie kursów.
- Wyświetlanie i edytowanie instruktorów.
- Wyświetlanie i edytowanie działów.

Poniżej przedstawiono zrzuty ekranu na kilku stronach językiem.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Przegląd funkcji aplikacji, które mają wpływ na wdrożenie

Następujące funkcje aplikacji wpływa na sposób wdrażania lub co musisz zrobić, aby wdrożyć go. Każdy z nich jest omówione bardziej szczegółowo w następujących samouczkach z tej serii.

- Uniwersytet contoso używa bazę danych programu SQL Server Compact do przechowywania danych aplikacji, takich jak nazwy dla uczniów i instruktora. Baza danych zawiera zarówno dane testowe i danymi produkcyjnymi, a podczas wdrażania w środowisku produkcyjnym należy wykluczyć dane testowe. W dalszej części tej serii samouczka będzie migracji z programu SQL Server Compact do programu SQL Server.
- Aplikacja korzysta z systemu członkostwa programu ASP.NET, która przechowuje informacje o koncie użytkownika w bazie danych programu SQL Server Compact. Aplikacja definiuje użytkownika administracyjnego, który ma dostęp do niektórych z chronionych informacji. Należy wdrożyć w członkowskiej bazie danych bez konta testowego, ale jedno konto administratora.
- Ponieważ bazy danych aplikacji i bazie danych członkostwa użyć programu SQL Server Compact jako aparat bazy danych, musisz wdrożyć aparatu bazy danych dostawcy usług hosta, a także samych bazach danych.
- Aplikacja korzysta z dostawców uniwersalnych członkostwa ASP.NET, aby system członkostwa może przechowywać swoje dane w bazie danych programu SQL Server Compact. Zestaw, który zawiera dostawców uniwersalnych członkostwa musi zostać wdrożony z aplikacją.
- Aplikacja korzysta z programu Entity Framework 5.0 dostępu do danych w bazie danych aplikacji. Zestaw, który zawiera program Entity Framework 5.0 musi zostać wdrożony z aplikacją.
- Aplikacja używa błąd innej firmy, rejestrowania i raportowania narzędzia. To narzędzie znajduje się w zestawie, który musi zostać wdrożony z aplikacją.
- Błąd narzędzia rejestrowania zapisuje informacje o błędzie w plikach XML w folderze plików. Masz upewnij się, że ASP.NET działa w witrynę wdrożoną konto ma uprawnienia do zapisu do tego folderu i należy wykluczyć ten folder z wdrożenia. (W przeciwnym razie błąd dane dzienników ze środowiska testowego, może być wdrażana w środowisku produkcyjnym i/lub plików dziennika błędów produkcyjnych może zostać usunięty.)
- Aplikacja zawiera niektóre ustawienia, które musi zostać zmienione w wdrożonych *Web.config* pliku, w zależności od środowiska docelowego (testowym lub produkcyjnym) i inne ustawienia, które musi zostać zmienione w zależności od kompilacji Konfiguracja (debugowanie lub wydanie).
- Rozwiązanie programu Visual Studio zawiera projekt biblioteki klas. Powinny być wdrażane tylko zestaw, który generuje ten projekt, nie projekt.

W ramach tego pierwszego samouczka z tej serii pobrać przykładowy projekt programu Visual Studio i przeglądane funkcje witryny, które wpływają na sposób wdrażania aplikacji. W ramach następujących samouczków przygotowanie się do wdrożenia, konfigurując niektóre z tych czynności, które mają być obsługiwane automatycznie. Inne, które należy zadbać o ręcznie.

> [!div class="step-by-step"]
> [Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
