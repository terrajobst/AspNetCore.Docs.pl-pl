---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: "Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: wprowadzenie | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji sieci web do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu za pomocą V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: e14f3bed001592c85bdbba868f51141bc52a9470
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: wprowadzenie
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji sieci web do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu przez przy użyciu programu Visual Studio 2012 z zestawem Azure SDK dla platformy .NET. Większość procedur są podobne dla programu Visual Studio 2013.
> 
> Można utworzyć aplikacji sieci web w taki sposób, aby udostępnić go przez Internet. Ale web podręczniki programowania zwykle zatrzymuje tuż po ich zostały pokazano sposób coś pracy na komputerze deweloperskim. Tej serii samouczków rozpoczyna się, gdy inne pozostawić wyłączone: został utworzony w aplikacji sieci web, przetestować, i jest gotowe. Co to jest dalej? Te samouczki wyjaśniają, jak wdrożyć najpierw do usług IIS na komputerze deweloperskim lokalnego do testowania, a następnie do platformy Azure lub innego dostawcy hostingu dla tymczasowych i produkcyjnych. Przykładowej aplikacji, które będą wdrażane jest projekt aplikacji sieci web, który używa programu Entity Framework, SQL Server i systemu członkostwa programu ASP.NET. Przykładowa aplikacja korzysta z formularzy sieci Web ASP.NET, ale zgodnie z procedurami przedstawionymi dotyczą również programu ASP.NET MVC i interfejsu API sieci Web.
> 
> Te samouczki przyjęto założenie, że wiesz, jak pracować z programem ASP.NET w programie Visual Studio. Jeśli użytkownik nie jest dobrym miejscem do rozpoczęcia [podstawowy samouczek formularzy sieci Web ASP.NET](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) lub [basic ASP.NET MVC samouczek](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) lub [StackOverflow](http://stackoverflow.com).
> 
> Ta zawartość jest również dostępny jako wolne e-book w [galerii TechNet Książka elektroniczna](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Omówienie

Te samouczki pomocne przy wdrażaniu aplikacji sieci web ASP.NET, że zawiera baz danych programu SQL Server. Poniżej przedstawiono wdrażania najpierw do usług IIS na komputerze deweloperskim lokalnego do testowania, a następnie do aplikacji sieci Web w usłudze Azure App Service i bazą danych SQL Azure dla tymczasowych i produkcyjnych. Zostanie wyświetlona wdrażanie przy użyciu programu Visual Studio publikowania jednym kliknięciem, i zobaczysz wdrażanie przy użyciu wiersza polecenia.

Liczba samouczki może uniemożliwić procesu wdrażania wydaje się być czasochłonnym zadaniem. W rzeczywistości ale podstawowe procedury są proste. Jednak w sytuacjach rzeczywistych często należy wykonać zadania wdrażania dodatkowych — na przykład ustawienie uprawnień do folderu na serwerze docelowym. Firma Microsoft zostały przedstawione niektóre z tych dodatkowych zadań, nadzieję, że nie należy pozostawiać samouczków informacje, które mogą uniemożliwiać pomyślne wdrożenie rzeczywistej aplikacji.

Samouczki są przeznaczone do uruchamiania w sekwencji, a każda część kompilacje w poprzedniej części. Można pominąć części, które nie są odpowiednie do sytuacji, ale następnie może być konieczne dostosowanie procedury przedstawione w kolejnych samouczkach.

## <a name="intended-audience"></a>Docelowa grupa odbiorców

Samouczków mają na celu deweloperów platformy ASP.NET, którzy pracują w środowiskach, w których:

- Środowiska produkcyjnego to Azure App Service Web Apps lub innego dostawcy hostingu.
- Wdrożenie nie jest ograniczona do procesu ciągłej integracji, ale może odbywać się bezpośrednio z programu Visual Studio.

Wdrożenia z [kontroli źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) przy użyciu [ciągłego dostarczania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) w tych samouczkach z wyjątkiem jednego samouczek, który pokazuje, jak wdrożyć z wiersza polecenia nie pasuje do procesu. Aby uzyskać informacje na temat ciągłego dostarczania zobacz następujące zasoby:

- [Ciągłej integracji i ciągłego dostarczania (Tworzenie chmury rzeczywistych aplikacji z systemu Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Wdrażanie aplikacji sieci web w usłudze Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-deploy/)
- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (starsze zestaw samouczki przeznaczone dla programu Visual Studio 2010, który nadal zawiera informacje przydatne dla przedsiębiorstw.)

## <a name="using-a-third-party-hosting-provider"></a>Przy użyciu innego dostawcy hostingu

Samouczków prowadzi użytkownika przez proces konfigurowania konta platformy Azure i wdrażanie aplikacji do aplikacji sieci Web w usłudze Azure App Service dla tymczasowych i produkcyjnych. Jednak można użyć tych samych procedur podstawowe w przypadku wdrożenia w wybranym dostawcy hostingu innych firm. Jeżeli samouczków za pośrednictwem procesów unikatowa na platformie Azure, wyjaśnić, że i poinformować różnice, jakie można spodziewać się u dostawcy hostingu innych firm.

## <a name="deploying-web-app-projects"></a>Wdrażanie projektów aplikacji sieci web

Przykładowej aplikacji, którą można pobrać i zainstalować te samouczki jest projektu aplikacji sieci web programu Visual Studio. Jednak po zainstalowaniu najnowszej [aktualizacji publikowania w sieci Web dla programu Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), można użyć tej samej metody wdrażania i narzędzi dla projektów aplikacji sieci web.

## <a name="deploying-aspnet-mvc-projects"></a>Wdrażanie projekty składnika ASP.NET MVC

Przykładowa aplikacja jest projektem formularzy sieci Web ASP.NET, ale wszystko poznać sposoby wykonywania ma zastosowanie do platformy ASP.NET MVC również. Projekt programu Visual Studio MVC jest po prostu inną formę projektu aplikacji sieci web. Jedyną różnicą jest to, że jeśli wdrażasz do dostawcy usług hosta, który nie obsługuje platformy ASP.NET MVC ani Twojej wersji docelowej, należy się upewnić, że zainstalowano odpowiedni ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) lub [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) pakietu NuGet w projekcie.

## <a name="programming-language"></a>Język programowania

Przykładowa aplikacja korzysta z C#, ale samouczków nie wymaga znajomości języka C# i techniki wdrażania wynika z samouczkami dotyczącymi nie są specyficzne dla języka.

## <a name="database-deployment-methods"></a>Metody wdrażania bazy danych

Istnieją trzy sposoby wdrożenie bazy danych SQL Server, wraz z wdrożenia w sieci web w programie Visual Studio:

- Migracje Code First Framework jednostki
- Dostawcy dbDacFx narzędzia Web Deploy
- Dostawca Narzędzia Web Deploy dbFullSql

W tym samouczku użyjesz dwa pierwsze z tych metod. Dostawca Narzędzia Web Deploy dbFullSql jest starszej wersji metody, która nie jest zalecane z wyjątkiem niektórych konkretnych scenariuszy, takich jak migracja z programu SQL Server Compact do programu SQL Server.

Metody przedstawiona w tym samouczku dotyczą baz danych programu SQL Server, nie programu SQL Server Compact. Aby uzyskać informacje o sposobie wdrażania bazy danych programu SQL Server Compact, zobacz [programu Visual Studio Web wdrażania z programu SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Metody przedstawiona w tym samouczku wymagają użycia narzędzia Web Deploy metody publikowania. Jeśli wolisz publikowania innej metody, takiej jak FTP, System plików lub FPSE, zobacz [wdrażania bazy danych niezależnie od wdrażania aplikacji sieci web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) w Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio.

### <a name="entity-framework-code-first-migrations"></a>Migracje Code First Framework jednostki

W programu Entity Framework w wersji 4.3 Firma Microsoft wprowadziła migracje Code First. Migracje Code First automatyzuje proces zmiany przyrostowe do modelu danych i propagowanie te zmiany w bazie danych. We wcześniejszych wersjach programu Code First zwykle umożliwiają Entity Framework, Porzuć i ponownie utworzyć bazę danych w każdej zmianie modelu danych. To nie jest problem w rozwoju, ponieważ dane testowe jest łatwo ponownego utworzenia, ale w środowisku produkcyjnym zwykle chcesz zaktualizować schemat bazy danych bez usuwania bazy danych. Funkcja migracji umożliwia Code First zaktualizować bazy danych bez usunięcie i ponowne utworzenie. Możesz pozwolić, aby Code First automatycznie decyzję dotyczącą sposobu wprowadzania zmian schematu wymagane lub można napisać kod, który dostosowuje zmiany. Aby obejrzeć wprowadzenie do migracje Code First, zobacz [migracje Code First](https://msdn.microsoft.com/library/hh770484.aspx).

W przypadku wdrażania projektu sieci web programu Visual Studio można zautomatyzować proces wdrażania bazy danych, który jest zarządzany przez migracje Code First. Podczas tworzenia profilu publikowania zaznaczeniu pola wyboru o nazwie wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji). To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować plik Web.config na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.

Program Visual Studio nie ma wpływu z bazą danych podczas procesu wdrażania. W przypadku wdrożonej aplikacji uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First automatycznie utworzy bazę danych lub aktualizuje schemat bazy danych do najnowszej wersji. Jeśli aplikacji implementuje metody migracje inicjatora, metoda uruchamiane po utworzeniu bazy danych lub schemat jest aktualizowany.

W tym samouczku użyjesz migracje Code First wdrażania bazy danych aplikacji.

### <a name="the-dbdacfx-web-deploy-provider"></a>Dostawcy dbDacFx narzędzia Web Deploy

Dla bazy danych do programu SQL Server, który nie jest zarządzana przez Entity Framework Code First można wybrać pola wyboru o nazwie aktualizacji bazy danych podczas konfigurowania profilu publikowania. Podczas początkowego rozmieszczania w docelowej bazie danych odpowiadające źródłowej bazy danych dostawcy dbDacFx narzędzia tworzy tabele i inne obiekty bazy danych. W kolejnych wdrożeń określa dostawcę, co różni się między bazami danych źródłowych i docelowych i aktualizuje schematu docelowej bazy danych odpowiadające źródłowej bazy danych. Domyślnie dostawca nie będzie zmiany powodujące utratę danych, takich jak po upuszczeniu tabeli lub kolumny.

Ta metoda nie automatyzacji wdrażania dane w tabelach bazy danych, ale można tworzyć skrypty, aby to zrobić i konfigurowanie programu Visual Studio, aby uruchomić potrzebne podczas wdrażania. Kolejny powód do uruchamiania skryptów podczas wdrażania jest wprowadzania zmian schematu, których nie można wykonać automatycznie, ponieważ ich może spowodować utratę danych.

W tym samouczku użyjesz dostawcy dbDacFx narzędzia do wdrażania bazy danych członkostwa programu ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Rozwiązywanie problemów podczas tego samouczka

Po wystąpieniu błędu podczas wdrażania lub wdrożonej witrynie nie działa poprawnie, komunikaty o błędach nie zawsze stanowią rozwiązanie oczywiste. Aby ułatwić niektórych typowych scenariuszy problem, [Rozwiązywanie problemów z strona referencyjna](troubleshooting.md) jest dostępna. Jeśli coś nie działa podczas wykonywania kroków samouczków wyświetlony komunikat o błędzie, należy sprawdzić stronę rozwiązywania problemów.

## <a name="comments-welcome"></a>Komentarze-Zapraszamy!

Komentarze dotyczące samouczków są-Zapraszamy!, a po zaktualizowaniu samouczka wszelkich starań, zostaną wprowadzone należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszenia, które znajdują się w samouczku komentarze.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku został napisany dla następujących produktów:

- Windows 8 lub Windows 7.
- Visual Studio 2012 lub Visual Studio 2012 Express for Web z [najnowszą aktualizację](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Zestaw Azure SDK dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Samouczka można wykonać za pomocą programu Visual Studio 2010 z dodatkiem SP1 lub programu Visual Studio 2013, ale niektóre zrzuty ekranu mogą być inne i niektóre funkcje mogą być inne.

Jeśli używasz programu Visual Studio 2013, zainstaluj [zestawu Azure SDK dla programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Jeśli używasz programu Visual Studio 2010 z dodatkiem SP1, należy zainstalować następujące oprogramowanie:

- [Zestaw Azure SDK dla programu Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB.](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/en-us/library/hh500335.aspx).

W zależności od tego, jak wiele zależności zestawu SDK już istnieje na tym komputerze Instalowanie zestawu SDK usługi Azure może zająć dużo czasu, od kilku minut do pół godziny lub dłużej. Należy zestawu Azure SDK, nawet wtedy, gdy planujesz publikować do innych firm dostawcy obsługi zamiast na platformie Azure, ponieważ zestaw SDK zawiera najnowsze aktualizacje programu Visual Studio web publikowanie funkcji.

> [!NOTE]
> W tym samouczku został zapisany z wersją 1.8.1 zestawu SDK platformy Azure. Od tego momentu zostały wydane nowszych wersji z dodatkowych funkcji. Samouczków zostały zaktualizowane do tych funkcji i łącze do zasobów, które mają więcej informacji o nich.


Instrukcje i zrzuty ekranu są oparte na systemie Windows 8, ale samouczków opisano różnice w systemie Windows 7.

Niektóre inne oprogramowanie jest wymagane w celu ukończenia tego samouczka, ale nie trzeba mieć jeszcze zainstalowana. Samouczek przeprowadzi Cię przez kroki instalacji, gdy są potrzebne.

## <a name="download-the-sample-application"></a>Pobierz aplikację przykładową

W przypadku wdrażania aplikacji o nazwie Contoso University i został już utworzony dla Ciebie. Jest to wersja uproszczony university witryny sieci web, luźno na podstawie Contoso University aplikacji opisane w [Samouczki programu Entity Framework w witrynie platformy ASP.NET](https://asp.net/entity-framework/tutorials).

Jeśli masz zainstalowane warunki wstępne, Pobierz [aplikacji sieci web firmy Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). *Zip* plik zawiera wiele wersji projektu. Aby pracować kroki tego samouczka, rozpoczynać projektu znajdujące się w folderze C#. Aby zobaczyć, jak wygląda projektu na końcu samouczków, otwórz projekt w folderze ContosoUniversity-End.

Aby przygotować projektu do pracy kroków samouczka, wykonaj następujące czynności:

1. Zapisywanie plików rozwiązania ContosoUniversity z folderu C# w folderze o nazwie ContosoUniversity w folderze, niezależnie od używać do pracy z projektów Visual Studio.

    Domyślnie jest to następujący folder dla programu Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Dla zrzutów ekranu, w tym samouczku, folder projektu znajduje się w katalogu głównym na `C`: dysk.)
2. Uruchom program Visual Studio i otworzyć projekt.
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i kliknij przycisk **przywracania pakietów EnableNuGet**.
4. Skompiluj rozwiązanie.
5. Jeśli występują błędy kompilacji, należy ręcznie przywrócić pakietów NuGet:

    1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Zarządzaj pakietami NuGet dla rozwiązania**.
    2. W górnej części **Zarządzaj pakietami NuGet** pojawi się okno dialogowe **tym rozwiązaniu brakuje niektórych NuGet pakietów. Kliknij, aby przywrócić.** Kliknij przycisk **przywrócić** przycisku.
    3. Ponownie skompiluj rozwiązanie.
6. Naciśnij klawisze CTRL-F5, aby uruchomić aplikację.

    Aplikacja zostanie otwarty do strony głównej Contoso University.

    ![Strona główna deweloperów](introduction/_static/image1.png)

    (Może być czas oczekiwania, podczas uruchamiania programu Visual Studio wystąpienie programu SQL Server Express LocalDB, i można uzyskać błąd upływu limitu czasu Jeśli który proces trwa zbyt długo. W takim przypadku wystarczy uruchomić projekt ponownie.)

Strony sieci Web są dostępne z paska menu i umożliwiają wykonywanie następujących funkcji:

- Wyświetlanie statystyk uczniów (strona informacje).
- Wyświetlanie, edytowanie usunąć i dodać studentów.
- Wyświetl i Edytuj szkolenia.
- Wyświetl i Edytuj instruktorów.
- Wyświetl i Edytuj działów.

Poniżej przedstawiono zrzuty ekranu kilka stron reprezentatywny.

![Dev strony uczniów lub studentów](introduction/_static/image2.png)

![Dodaj deweloperów strony uczniów lub studentów](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Przejrzyj funkcje aplikacji, które mają wpływ na wdrożenie

Poniższe funkcje aplikacji wpływa na sposób wdrażania lub masz celu jej wdrożenia. Każdy z tych jest co omówiono bardziej szczegółowo w następujących samouczkach z tej serii.

- Contoso University korzysta z bazy danych programu SQL Server do przechowywania danych aplikacji, takich jak nazwy dla użytkowników domowych i instruktora. Baza danych zawiera zarówno dane testowe i danymi produkcyjnymi, a podczas wdrażania w środowisku produkcyjnym należy wyłączyć danych testowych.
- Aplikacja używa systemu członkostwa programu ASP.NET, w której są przechowywane informacje o koncie użytkownika w bazie danych programu SQL Server. Aplikacja definiuje użytkownika administracyjnego, który ma dostęp do niektórych chronionych informacji. Należy wdrożyć w bazie danych członkostwa bez testowe konta, ale przy użyciu konta administratora.
- Błąd innej firmy, rejestrowania i raportowania narzędzie używane przez aplikację. To narzędzie jest dostępne w zestawie, do którego należy wdrożyć w aplikacji.
- Narzędzia rejestrowania błędów zapisuje informacje o błędzie w plikach XML do folderu plików. Należy upewnić się, czy konto, które program ASP.NET jest uruchamiana w witrynie wdrożonej ma uprawnienie zapisu do tego folderu, i czy masz do wykluczenia z wdrożenia tego folderu. (W przeciwnym razie błąd dane dzienników ze środowiska testowego mógł zostać wdrożony w środowisku produkcyjnym i/lub plików dziennika błędów produkcji może zostać usunięty.)
- Aplikacja zawiera pewne ustawienia, które można zmienić w programie w wdrożone *Web.config* pliku, w zależności od środowiska docelowego (test przemieszczania i produkcji) i inne ustawienia, które musi zostać zmienione w zależności od kompilacji Konfiguracja (debugowanie czy wydanie).
- Rozwiązanie programu Visual Studio zawiera projektu biblioteki klas. Powinny być wdrażane tylko zestaw, który generuje ten projekt, nie projektu.

## <a name="summary"></a>Podsumowanie

W tym samouczku pierwszy z serii pobraniu przykładowy projekt programu Visual Studio i przejrzeć funkcji witryny, które mają wpływ na sposób wdrażania aplikacji. W następujących samouczkach przygotować się do wdrożenia, konfigurując niektóre z nich mają zostać obsłużone automatycznie. Inne rozwiązywane ręcznie.

>[!div class="step-by-step"]
[Dalej](preparing-databases.md)
