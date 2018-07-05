---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: wprowadzenie | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji sieci web do usługi Azure App Service Web Apps lub dostawcy hostingu innych firm, dzięki użyciu V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: ad06639db9c5de78fb9926c7ab00e348bcbb20cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382561"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: wprowadzenie
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji sieci web do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu, przez przy użyciu programu Visual Studio 2012 w zestawie Azure SDK dla platformy .NET. Większość procedur są podobne dla programu Visual Studio 2013.
> 
> Możesz tworzyć aplikacji sieci web w taki sposób, aby udostępnić go przez Internet. Ale samouczki programowania dla sieci web zwykle zatrzymuje po prawej, po one już wyświetlane jak coś działa na komputerze deweloperskim. W tej serii samouczków rozpoczyna się, gdzie pozostawić inne wyłączone: zostały skompilowane z aplikacją internetową, przetestowane, i jest gotowa do użytku. Jaka jest przyszłość? Tych samouczkach przedstawiono sposób wdrażania najpierw w usługach IIS na lokalnym komputerze deweloperskim do testowania, a następnie na platformie Azure lub dostawcy hostingu innych firm, dla środowisk przejściowych i produkcyjnych. Przykładowa aplikacja, który będzie wdrażany jest projekt aplikacji sieci web, który używa programu Entity Framework, programu SQL Server i systemu członkostwa programu ASP.NET. Przykładowa aplikacja używa wzorca ASP.NET Web Forms, ale z procedurami przedstawionymi dotyczą również programu ASP.NET MVC i interfejs API sieci Web.
> 
> Te samouczki przyjęto założenie, że wiesz, jak pracować z programem ASP.NET w programie Visual Studio. Jeśli nie istnieje, jest dobrym miejscem do rozpoczęcia [podstawowego samouczka formularzy sieci Web platformy ASP.NET](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) lub [podstawowego samouczka MVC ASP.NET](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) lub [StackOverflow](http://stackoverflow.com).
> 
> Ta zawartość jest również dostępna jako bezpłatny e-book, w [galerii TechNet E-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Omówienie

Te Samouczki pomagają w wdrażanie aplikacji sieci web platformy ASP.NET, która obejmuje bazy danych programu SQL Server. Poniżej przedstawiono wdrażania najpierw w usługach IIS na lokalnym komputerze deweloperskim do testowania, a następnie do aplikacji sieci Web w usłudze Azure App Service i Azure SQL Database dla środowisk przejściowych i produkcyjnych. Pokazano, jak wdrożyć przy użyciu programu Visual Studio publikowanie jednym kliknięciem i pokazano, jak wdrożyć przy użyciu wiersza polecenia.

Liczba samouczki może uniemożliwić procesu wdrażania wydają się czasochłonnym zadaniem. W rzeczywistości ale podstawowe procedury są proste. Jednak w rzeczywistych warunkach często konieczne wdrożenie dodatkowych zadań — na przykład, ustawianie uprawnień do folderów na serwerze docelowym. Firma Microsoft zostały przedstawione niektóre z tych dodatkowych zadań, w nadziei, że samouczków nie opuszczaj informacje, które mogą uniemożliwić pomyślne wdrożenie rzeczywistej aplikacji.

Samouczki są zaprojektowane do uruchamiania w sekwencji, a każda część kompilacji w poprzedniej części. Można pominąć części, które nie są odpowiednie do sytuacji, ale następnie może być konieczne dostosowanie procedury opisane w kolejnych samouczkach.

## <a name="intended-audience"></a>Odbiorców

Samouczki mają na celu deweloperów platformy ASP.NET, którzy pracują w środowiskach, w których:

- W środowisku produkcyjnym jest usługi Azure App Service Web Apps lub dostawcy hostingu innych firm.
- Wdrożenie nie jest ograniczona do procesu ciągłej integracji, ale może odbywać się bezpośrednio z programu Visual Studio.

Wdrażanie z [kontroli źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) przy użyciu [ciągłe dostarczanie](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) procesu nie została uwzględniona w tych samouczków, z wyjątkiem jednego samouczek, w którym przedstawiono sposób wdrażania z poziomu wiersza polecenia. Dla informacji o ciągłe dostarczanie zobacz następujące zasoby:

- [Ciągła integracja i ciągłe dostarczanie (tworzenie rzeczywistych aplikacji w chmurze przy użyciu platformy Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Wdrażanie aplikacji sieci web w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (starszy zestaw samouczki napisany dla programu Visual Studio 2010, która nadal zawiera informacje przydatne w środowiskach przedsiębiorstw.)

## <a name="using-a-third-party-hosting-provider"></a>Za pomocą dostawcy hostingu innych firm

Samouczki prowadzą użytkownika przez proces konfigurowania konta platformy Azure i wdrażanie aplikacji do aplikacji sieci Web w usłudze Azure App Service dla środowisk przejściowych i produkcyjnych. Jednak można użyć tych samych procedur podstawowych do wdrożenia u dostawcy hostingu innych firm wybranych przez użytkownika. W przypadku, gdy samouczków przechodzi przez procesy unikatowa na platformie Azure, wyjaśniono, że i poinformowania różnic, jakie można spodziewać się u dostawcy hostingu innych firm.

## <a name="deploying-web-app-projects"></a>Wdrażanie projektów aplikacji sieci web

Przykładowej aplikacji, możesz pobrać i wdrożyć na potrzeby tych samouczków jest projektu aplikacji sieci web programu Visual Studio. Jednak po zainstalowaniu najnowszej [Web publikowanie aktualizacji dla programu Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), można użyć tej samej metody wdrażania i narzędzia dla projektów aplikacji sieci web.

## <a name="deploying-aspnet-mvc-projects"></a>Wdrażanie projektów platformy ASP.NET MVC

Przykładowa aplikacja jest projektu programu ASP.NET Web Forms, ale wszystko dowiesz się, jak to zrobić stosuje się do platformy ASP.NET MVC oraz. Projekt programu Visual Studio MVC jest po prostu innej formy projektu aplikacji sieci web. Jedyną różnicą jest to, że jeśli wdrażasz do dostawcy usług hosta, który nie obsługuje platformy ASP.NET MVC lub wersji docelowej ją, należy się upewnić, że masz zainstalowaną odpowiednią ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) lub [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) pakietu NuGet w projekcie.

## <a name="programming-language"></a>Język programowania

Przykładowa aplikacja używa języka C#, ale samouczków nie wymagają znajomości języka C# i techniki wdrażania, wyświetlane według samouczków nie są specyficzne dla języka.

## <a name="database-deployment-methods"></a>Metody wdrażania bazy danych

Istnieją trzy sposoby wdrożyć bazę danych programu SQL Server oraz wdrażania w Internecie w programie Visual Studio:

- Migracje Code First Framework jednostki
- Dostawcy dbDacFx Web Deploy
- Dostawca Narzędzia Web Deploy dbFullSql

W tym samouczku użyjesz pierwsze dwa z tych metod. Dostawca Narzędzia Web Deploy dbFullSql jest metodą starszej wersji, która nie jest już zalecany z wyjątkiem niektórych określonych scenariuszach, takich jak migracja z programu SQL Server Compact do programu SQL Server.

Metody przedstawiona w tym samouczku są dla baz danych programu SQL Server, nie programu SQL Server Compact. Aby uzyskać informacje o tym, jak wdrożyć bazę danych programu SQL Server Compact, zobacz [Visual Studio Web wdrożenia przy użyciu programu SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Metody przedstawiona w tym samouczku wymagają użycia narzędzia Web Deploy metody publikowania. Jeśli wolisz inny publikowania metody, takiej jak FTP, System plików lub FPSE, zobacz [wdrażania bazy danych niezależnie od wdrażania aplikacji sieci web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) mapy zawartości wdrażania sieci Web dla programu Visual Studio i platformy ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migracje Code First Framework jednostki

W programu Entity Framework w wersji 4.3 Firma Microsoft wprowadziła migracje Code First. Migracje Code First pozwala zautomatyzować proces wprowadzania zmian przyrostowych w modelu danych i propagowanie tych zmian w bazie danych. We wcześniejszych wersjach Code First zwykle umożliwiają Entity Framework, Porzuć i ponownie utworzyć bazę danych po każdej zmianie modelu danych. Nie jest problemem w rozwoju, ponieważ dane testowe jest łatwo ponownie utworzyć, ale w środowisku produkcyjnym zazwyczaj chcesz zaktualizować schemat bazy danych bez usuwania bazy danych. Funkcja migracji umożliwia Code First aktualizacji bazy danych bez porzucenie i ponowne utworzenie go. Można pozwolić, aby Code First automatycznie podjęcie decyzji o sposobie wprowadzania zmian wymagane schematu lub można napisać kod, który dostosowuje zmiany. Aby zapoznać się z wprowadzeniem do migracje Code First, zobacz [migracje Code First](https://msdn.microsoft.com/library/hh770484.aspx).

W przypadku wdrażania projektu sieci web programu Visual Studio można zautomatyzować proces wdrażania bazy danych, który jest zarządzany przez migracje Code First. Podczas tworzenia profilu publikowania jest zaznacz pole wyboru, która jest oznaczona etykietą wykonaj migracje Code First (uruchomieniu aplikacji). To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować plik Web.config na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.

Program Visual Studio nie działa z bazą danych w procesie wdrażania. Gdy wdrożonej aplikacji uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First automatycznie tworzy bazę danych lub aktualizuje schemat bazy danych do najnowszej wersji. Jeśli aplikacja implementuje metodę migracji inicjatora, metoda jest uruchamiana po utworzeniu bazy danych lub schemat jest aktualizowany.

W tym samouczku użyjesz migracje Code First wdrożyć bazę danych aplikacji.

### <a name="the-dbdacfx-web-deploy-provider"></a>Dostawcy dbDacFx Web Deploy

Dla bazy danych do programu SQL Server, który nie jest zarządzany przez Entity Framework Code First można wybrać pole wyboru, która jest oznaczona etykietą Aktualizuj bazę danych, podczas konfigurowania profilu publikowania. Podczas początkowego wdrożenia dostawcy dbDacFx tworzy tabele i inne obiekty bazy danych w docelowej bazie danych, aby dopasować źródłowej bazy danych. W kolejnych wdrożeń dostawcę Określa, jakie są różnice między bazami danych źródłowych i docelowych i aktualizuje schemat docelowej bazy danych, aby dopasować źródłowej bazy danych. Domyślnie dostawca nie będzie wprowadzać żadnych zmian, powodujące utraty danych, np. Jeśli tabela lub kolumna jest porzucany.

Ta metoda nie automatyzację wdrażania danych w tabelach bazy danych, ale można tworzyć skrypty, aby to zrobić i konfigurowanie programu Visual Studio, aby uruchomić je podczas wdrażania. To kolejny powód, aby uruchamiać skrypty podczas wdrażania zmian schematu, które nie może być wykonane automatycznie, ponieważ spowodowałoby utratę danych.

W tym samouczku użyjesz dostawcy dbDacFx do wdrożenia bazy danych członkostwa ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Rozwiązywanie problemów w ramach tego samouczka

Po wystąpieniu błędu podczas wdrażania lub witrynę wdrożoną nie działa poprawnie, komunikaty o błędach nie zawsze podawać to oczywiste rozwiązanie. Do udzielenia odpowiedzi na kilka typowych scenariuszy problem [Rozwiązywanie problemów z strona referencyjna](troubleshooting.md) jest dostępna. Jeśli otrzymasz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczków, pamiętaj sprawdzić stronę rozwiązywania problemów.

## <a name="comments-welcome"></a>Uwagi-Zapraszamy!

Komentarze dotyczące samouczków są powitalnej, a po zaktualizowaniu samouczka wszelkich starań, będzie nawiązywane w przypadku uwzględnienia poprawki konta lub sugestie dotyczące ulepszeń, które znajdują się w samouczku komentarzy.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek został napisany dla następujących produktów:

- System Windows 8 lub Windows 7.
- Visual Studio 2012 lub Visual Studio 2012 Express for Web z [najnowszą aktualizację](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Wykonać kroki samouczka przy użyciu programu Visual Studio 2010 z dodatkiem SP1 lub programu Visual Studio 2013, ale niektóre zrzuty ekranu będzie się różnił i niektóre funkcje będą inne.

Jeśli używasz programu Visual Studio 2013, należy zainstalować [zestawu Azure SDK dla programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Jeśli używasz programu Visual Studio 2010 z dodatkiem SP1, należy zainstalować następujące oprogramowanie:

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

W zależności od liczby składników zależnych zestawu SDK już istnieje na komputerze Instalowanie zestawu Azure SDK może zająć dużo czasu, od kilku minut do pół godziny lub dłużej. Zestaw Azure SDK jest konieczne, nawet, jeśli zamierzasz publikować firm u dostawcy hostingu zamiast na platformie Azure, ponieważ zestaw SDK zawiera najnowsze aktualizacje programu Visual Studio web publikowanie funkcji.

> [!NOTE]
> Ten samouczek został napisany z zestawu SDK platformy Azure w wersji 1.8.1. Od tego czasu wydano nowsze wersje oraz dodatkowe funkcje. Samouczki zostały zaktualizowanie, aby wspomnieć o tych funkcji i łącza do zasobów, które zawierają więcej informacji o nich.


Instrukcje i zrzuty ekranu są oparte na systemie Windows 8, ale samouczków wyjaśniono różnice, Windows 7.

Niektóre inne oprogramowanie jest wymagane w celu ukończenia tego samouczka, ale nie masz jeszcze zainstalowanego mieć. Samouczek przeprowadzi Cię kroki instalacji, gdy ich potrzebujesz.

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

Aplikacja, która będzie wdrażana o nazwie Contoso University i został już utworzony dla Ciebie. Jest uproszczoną wersję university witryny sieci web, luźno na podstawie Contoso University aplikacji opisane w [samouczki platformy Entity Framework w witrynie programu ASP.NET](https://asp.net/entity-framework/tutorials).

Jeśli masz zainstalowane warunki wstępne, Pobierz [aplikacji sieci web firmy Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). *Zip* plik zawiera wiele wersji projektu. Do pracy, kolejne kroki tego samouczka, rozpoczynać projektu, znajdujący się w folderze C#. Aby zobaczyć, jak wygląda projektu na końcu samouczków, otwórz projekt w folderze ContosoUniversity-End.

Aby przygotować projekt do pracy nad kroki samouczka, wykonaj następujące czynności:

1. Zapisz pliki rozwiązania ContosoUniversity z folderu języka C# w folderze o nazwie ContosoUniversity w folderze, niezależnie od użycia podczas pracy z projektami Visual Studio.

    Domyślnie jest to następujący folder programu Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Do zrzutów ekranu w tym samouczku, folderu projektu znajduje się w katalogu głównym na `C`: dysk.)
2. Uruchom program Visual Studio i Otwórz projekt.
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i kliknij przycisk **Przywracanie pakietów EnableNuGet**.
4. Skompiluj rozwiązanie.
5. Jeśli występują błędy kompilacji, należy ręcznie przywrócić pakiety NuGet:

    1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Zarządzaj pakietami NuGet dla rozwiązania**.
    2. W górnej części **Zarządzaj pakietami NuGet** zostanie wyświetlone okno dialogowe **pakiety NuGet niektóre brakuje tego rozwiązania. Kliknij, aby przywrócić.** Kliknij przycisk **przywrócić** przycisku.
    3. Ponownie skompiluj rozwiązanie.
6. Naciśnij kombinację klawiszy CTRL-F5, aby uruchomić aplikację.

    Aplikacja zostanie otwarta do strony głównej firmy Contoso University.

    ![Strona główna Dev](introduction/_static/image1.png)

    (Może być czas oczekiwania, podczas gdy program Visual Studio uruchamia wystąpienie programu SQL Server Express LocalDB, i możesz otrzymać błąd upływu limitu czasu Jeśli, proces trwa zbyt długo. W takim przypadku po prostu uruchom ponownie projekt.)

Na stronach witryny sieci Web są dostępne z paska menu i umożliwiają wykonywanie następujących funkcji:

- Wyświetlanie statystyk dla uczniów (strona informacje).
- Wyświetlanie, edytowanie, usuwanie i dodanie uczniów.
- Wyświetlanie i edytowanie kursów.
- Wyświetlanie i edytowanie instruktorów.
- Wyświetlanie i edytowanie działów.

Poniżej przedstawiono zrzuty ekranu na kilku stronach językiem.

![Studenci stronie deweloperów](introduction/_static/image2.png)

![Dodaj studentów, deweloperów strony](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Przejrzyj funkcje aplikacji, które mają wpływ na wdrożenie

Następujące funkcje aplikacji wpływa na sposób wdrażania lub co musisz zrobić, aby wdrożyć go. Każdy z nich jest omówione bardziej szczegółowo w następujących samouczkach z tej serii.

- Uniwersytet contoso używa bazy danych programu SQL Server do przechowywania danych aplikacji, takich jak nazwy dla uczniów i instruktora. Baza danych zawiera zarówno dane testowe i danymi produkcyjnymi, a podczas wdrażania w środowisku produkcyjnym należy wykluczyć dane testowe.
- Aplikacja korzysta z systemu członkostwa programu ASP.NET, która przechowuje informacje o koncie użytkownika w bazie danych programu SQL Server. Aplikacja definiuje użytkownika administracyjnego, który ma dostęp do niektórych z chronionych informacji. Należy wdrożyć w członkowskiej bazie danych bez konta testowego, ale przy użyciu konta administratora.
- Aplikacja używa błąd innej firmy, rejestrowania i raportowania narzędzia. To narzędzie znajduje się w zestawie, który musi zostać wdrożony z aplikacją.
- Błąd narzędzia rejestrowania zapisuje informacje o błędzie w plikach XML w folderze plików. Masz upewnij się, że ASP.NET działa w witrynę wdrożoną konto ma uprawnienia do zapisu do tego folderu i należy wykluczyć ten folder z wdrożenia. (W przeciwnym razie błąd dane dzienników ze środowiska testowego, może być wdrażana w środowisku produkcyjnym i/lub plików dziennika błędów produkcyjnych może zostać usunięty.)
- Aplikacja zawiera niektóre ustawienia, które musi zostać zmienione w wdrożonych *Web.config* pliku, w zależności od środowiska docelowego (testowych, przejściowych lub produkcyjnych) i inne ustawienia, które musi zostać zmienione w zależności od kompilacji Konfiguracja (debugowanie lub wydanie).
- Rozwiązanie programu Visual Studio zawiera projekt biblioteki klas. Powinny być wdrażane tylko zestaw, który generuje ten projekt, nie projekt.

## <a name="summary"></a>Podsumowanie

W ramach tego pierwszego samouczka z tej serii pobrać przykładowy projekt programu Visual Studio i przeglądane funkcje witryny, które wpływają na sposób wdrażania aplikacji. W ramach następujących samouczków przygotowanie się do wdrożenia, konfigurując niektóre z tych czynności, które mają być obsługiwane automatycznie. Inne, które należy zadbać o ręcznie.

> [!div class="step-by-step"]
> [Next](preparing-databases.md)
