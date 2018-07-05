---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Konfigurowanie właściwości projektu - 4 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa1e55f21e74ae8da67d6d13c35a8de5693ec3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396275"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Konfigurowanie właściwości projektu - 4 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Niektóre opcje wdrażania są skonfigurowane we właściwościach projektu, które są przechowywane w pliku projektu ( *.csproj* lub *.vbproj* pliku). W większości przypadków są wartości domyślne tych ustawień, co ma, ale można użyć **właściwości projektu** interfejsu użytkownika utworzone w programie Visual Studio do pracy z tymi ustawieniami, jeśli trzeba je zmienić. W ramach tego samouczka zapoznaj się z ustawienia wdrażania **właściwości projektu**. Możesz również utworzyć plik symbolu zastępczego, który powoduje, że pusty folder, który można wdrożyć.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurowanie ustawień wdrażania w oknie dialogowym właściwości projektu

Większość ustawień wpływających na to, co się dzieje podczas wdrożenia znajdują się w profilu publikowania, jak można zauważyć w ramach następujących samouczków. Kilka ustawień, których należy pamiętać o znajdują się w **Pakuj/Publikuj** kart **właściwości projektu** okna. Te ustawienia są określone dla każdej konfiguracji kompilacji — oznacza to, nie należy do kompilacji debugowanej może mieć różne ustawienia dla kompilacji wydania.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, wybierz opcję **właściwości**, a następnie wybierz pozycję **pakowaniu/publikowaniu Web**kartę.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Gdy okno jest wyświetlane, domyślnie przedstawiająca ustawienia dla jednego z tych konfiguracji kompilacji jest aktualnie aktywnego rozwiązania. Jeśli **konfiguracji** okno nie wskazuje **aktywny (wersja)**, wybierz opcję **wersji** w celu wyświetlenia ustawień dla konfiguracji kompilacji wydania. Poniżej przedstawiono wdrażania kompilacji wydania w środowiskach testowych i produkcyjnych.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Za pomocą **aktywny (wersja)** lub **wersji** wybrana, możesz zobaczyć wartości, które są efektywne w przypadku wdrażania przy użyciu konfiguracji kompilacji wydania:

- W **elementów, aby wdrożyć** polu **tylko pliki potrzebne do uruchomienia aplikacji** jest zaznaczone. Inne opcje są **wszystkie pliki w tym projekcie** lub **wszystkie pliki w tym folderze projektu**. Pozostawić wybór domyślny, bez zmian można uniknąć wdrażania plików kodu źródłowego, na przykład. To ustawienie jest powód, dlaczego ma foldery zawierające pliki binarne programu SQL Server Compact do uwzględnienia w projekcie. Aby uzyskać więcej informacji na temat tego ustawienia, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET Web Application projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx).
- **Wyklucz generowane symbole debugowania** jest zaznaczone. Nie możesz debugowanie podczas używania tej konfiguracji kompilacji.
- **Wyklucz pliki z poziomu aplikacji\_folderu danych** nie jest zaznaczone. SQL Server Compact pliku bazy danych członkostwa jest w tym folderze i należy ją wdrożyć. Podczas wdrażania aktualizacji, które nie zawierają zmiany w bazie danych, należy wybrać to pole wyboru.
- **Prekompilowanie tej aplikacji, przed opublikowaniem** nie jest zaznaczone. W większości przypadków nie ma potrzeby wstępnej kompilacji projektów aplikacji sieci web. Aby uzyskać więcej informacji na temat tej opcji, zobacz [Pakuj/Publikuj kartę sieci Web, właściwości projektu](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) i [zaawansowane Prekompilowanie okno dialogowe Ustawienia](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Obejmują wszystkie bazy danych skonfigurowano na karcie pakowanie/publikowanie SQL** jest zaznaczone, ale tej opcji ma teraz żadnego efektu, ponieważ nie są konfigurowanie **Pakuj/Publikuj SQL** kartę. Tę kartę, jest metoda wdrażania starszej bazy danych, które były jedyną opcją wdrażania baz danych programu SQL Server. Użyjesz **Pakuj/Publikuj SQL** karcie [migracji do programu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) samouczka.
- **Ustawienia pakietu wdrażania sieci Web** sekcji nie ma zastosowania, ponieważ używasz jednym kliknięciem publikować w tych samouczkach.

Zmiana **konfiguracji** pole listy rozwijanej do debugowania, aby wyświetlić ustawienia domyślne kompilacji debugowania. Wartości są takie same, z wyjątkiem **wykluczyć generowane symbole debugowania** jest wyczyszczone, dzięki czemu można debugować po wdrożeniu kompilacji debugowania.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Co się, że Folder biblioteki Elmah zostanie wdrożona

Jak pokazano w poprzednim samouczku [pakiet NuGet biblioteki Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) zapewnia funkcje rejestrowania i raportowania błędów. W aplikacji Contoso University Elmah został skonfigurowany, aby zapisywać szczegóły błędu w folderze o nazwie *Elmah*:

![Folder biblioteki Elmah](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Wykluczenie określonych plików lub folderów z wdrożenia jest typowym wymogiem; innym przykładem może być folderem, który użytkownicy mogą przekazywać pliki do. Nie chcesz, aby pliki dziennika lub przekazane pliki, które zostały utworzone w środowisku projektowym do wdrożenia do środowiska produkcyjnego. A jeśli wdrażasz aktualizację do środowiska produkcyjnego nie chcesz, aby proces wdrażania, aby usunąć pliki, które istnieją w środowisku produkcyjnym. (W zależności od tego, jak ustawisz opcję wdrożenia, jeśli plik istnieje w lokacji docelowej, ale nie w lokacji źródłowej, podczas wdrażania, narzędzie Web Deploy usuwa go z miejsca docelowego.)

Jak przedstawiono wcześniej w tym samouczku **elementów, aby wdrożyć** opcji **pakowaniu/publikowaniu Web** karta jest ustawiona na **tylko pliki potrzebne do uruchomienia tej aplikacji**. W rezultacie plików dziennika, które są tworzone przez Elmah w trakcie opracowywania nie zostanie wdrożony, czyli, które mają zostać wykonane. (Wdrożony, będą mieć do uwzględnienia w projekcie i ich **Build Action** właściwość musi być równa **zawartości**. Aby uzyskać więcej informacji, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET Web Application projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx)). Jednak narzędzie Web Deploy nie utworzy folder w lokacji docelowej, chyba że istnieje co najmniej jeden plik do skopiowania do niego. W związku z tym, jaki dodasz *.txt* plik do folderu do działania jako symbol zastępczy, tak aby folderze zostaną skopiowane.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz **Dodaj nowy element**i Utwórz plik o nazwie *Placeholder.txt*. Umieść następujący tekst w niej: "To jest plik symbolu zastępczego, aby upewnić się, że folder zostanie wdrożona." i Zapisz plik. To wszystko, należy wykonać, aby upewnić się, że program Visual Studio wdroży ten plik i folder, jest on, ponieważ **Build Action** właściwość *.txt* plików jest ustawiona na **zawartości**domyślnie.

Ukończono wszystkie zadania konfiguracji wdrożenia. W następnym samouczku możesz wdrożyć witrynę Contoso University do środowiska testowego i przetestować.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
