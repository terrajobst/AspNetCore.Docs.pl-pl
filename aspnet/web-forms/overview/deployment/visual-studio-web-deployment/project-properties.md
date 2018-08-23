---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: właściwości projektu | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 85b2aa982c56f030c2de0f3ac2ad0dca780f3f38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753922"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: właściwości projektu
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Niektóre opcje wdrażania są skonfigurowane we właściwościach projektu, które są przechowywane w pliku projektu ( *.csproj* lub *.vbproj* pliku). W większości przypadków są wartości domyślne tych ustawień, co ma, ale można użyć **właściwości projektu** interfejsu użytkownika utworzone w programie Visual Studio do pracy z tymi ustawieniami, jeśli trzeba je zmienić. W ramach tego samouczka zapoznaj się z ustawienia wdrażania **właściwości projektu**. Możesz również utworzyć plik symbolu zastępczego, który powoduje, że pusty folder, który można wdrożyć.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfiguruj ustawienia wdrażania w oknie dialogowym właściwości projektu

Większość ustawień wpływających na to, co się dzieje podczas wdrożenia znajdują się w profilu publikowania, jak można zauważyć w ramach następujących samouczków. Kilka ustawień, których należy pamiętać o znajdują się w **Pakuj/Publikuj** kart **właściwości projektu** okna. Te ustawienia są określone dla każdej konfiguracji kompilacji — oznacza to, nie należy do kompilacji debugowanej może mieć różne ustawienia dla kompilacji wydania.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, wybierz opcję **właściwości**, a następnie wybierz pozycję **pakowaniu/publikowaniu Web**kartę.

![Karcie pakowanie/publikowanie w sieci Web](project-properties/_static/image1.png)

Gdy okno jest wyświetlane, domyślnie przedstawiająca ustawienia dla jednego z tych konfiguracji kompilacji jest aktualnie aktywnego rozwiązania. Jeśli **konfiguracji** okno nie wskazuje **aktywny (wersja)**, wybierz opcję **wersji** w celu wyświetlenia ustawień dla konfiguracji kompilacji wydania. Poniżej przedstawiono wdrażania kompilacji wydania w środowiskach testowych i produkcyjnych.

![Wybieranie konfiguracji kompilacji wydania](project-properties/_static/image2.png)

Za pomocą **aktywny (wersja)** lub **wersji** wybrana, możesz zobaczyć wartości, które są efektywne w przypadku wdrażania przy użyciu konfiguracji kompilacji wydania:

- W **elementów, aby wdrożyć** polu **tylko pliki potrzebne do uruchomienia aplikacji** jest zaznaczone. Inne opcje są **wszystkie pliki w tym projekcie** lub **wszystkie pliki w tym folderze projektu**. Pozostawić wybór domyślny, bez zmian można uniknąć wdrażania plików kodu źródłowego, na przykład. To ustawienie jest powód, dlaczego ma foldery zawierające pliki binarne programu SQL Server Compact do uwzględnienia w projekcie. Aby uzyskać więcej informacji na temat tego ustawienia, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET Web Application projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx).
- **Wyklucz generowane symbole debugowania** jest zaznaczone. Nie możesz debugowanie podczas używania tej konfiguracji kompilacji.
- **Obejmują wszystkie bazy danych skonfigurowano na karcie pakowanie/publikowanie SQL** jest zaznaczone. Określa, czy program Visual Studio wdroży baz danych, a także pliki. Mimo że pole wyboru etykietę tylko wzmianki **Pakuj/Publikuj SQL** kartę, czyszczenie tego pola wyboru spowoduje także wyłączenie wdrożenie bazy danych, który jest skonfigurowany w profilu publikowania. Można będzie się robi, później, pole wyboru musi pozostać zaznaczona. **Pakuj/Publikuj SQL** karta jest używana dla starszej wersji bazy danych publikacji metodę, która nie będzie używany w tych samouczkach.
- **Ustawienia pakietu wdrażania sieci Web** sekcji nie ma zastosowania, ponieważ używasz jednym kliknięciem publikować w tych samouczkach.

Zmiana **konfiguracji** pole listy rozwijanej do debugowania, aby wyświetlić ustawienia domyślne kompilacji debugowania. Wartości są takie same, z wyjątkiem **wykluczyć generowane symbole debugowania** jest wyczyszczone, dzięki czemu można debugować po wdrożeniu kompilacji debugowania.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Upewnij się, że folder biblioteki Elmah zostanie wdrożona

Jak pokazano w poprzednim samouczku [pakiet NuGet biblioteki Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) zapewnia funkcje rejestrowania i raportowania błędów. W aplikacji Contoso University Elmah został skonfigurowany, aby zapisywać szczegóły błędu w folderze o nazwie *Elmah*:

![Folder biblioteki Elmah](project-properties/_static/image3.png)

Wykluczenie określonych plików lub folderów z wdrożenia jest typowym wymogiem; innym przykładem może być folderem, który użytkownicy mogą przekazywać pliki do. Nie chcesz, aby pliki dziennika lub przekazane pliki, które zostały utworzone w środowisku projektowym do wdrożenia do środowiska produkcyjnego. A jeśli wdrażasz aktualizację do środowiska produkcyjnego nie chcesz, aby proces wdrażania, aby usunąć pliki, które istnieją w środowisku produkcyjnym. (W zależności od tego, jak ustawisz opcję wdrożenia, jeśli plik istnieje w lokacji docelowej, ale nie w lokacji źródłowej, podczas wdrażania, narzędzie Web Deploy usuwa go z miejsca docelowego.)

Jak przedstawiono wcześniej w tym samouczku **elementów, aby wdrożyć** opcji **pakowaniu/publikowaniu Web** karta jest ustawiona na **tylko pliki potrzebne do uruchomienia tej aplikacji**. W rezultacie plików dziennika, które są tworzone przez Elmah w trakcie opracowywania nie zostanie wdrożony, czyli, które mają zostać wykonane. (Wdrożony, będą mieć do uwzględnienia w projekcie i ich **Build Action** właściwość musi być równa **zawartości**. Aby uzyskać więcej informacji, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET Web Application projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx)). Jednak narzędzie Web Deploy nie utworzy folder w lokacji docelowej, chyba że istnieje co najmniej jeden plik do skopiowania do niego. W związku z tym, jaki dodasz *.txt* plik do folderu do działania jako symbol zastępczy, tak aby folderze zostaną skopiowane.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz **Dodaj nowy element**i Utwórz plik tekstowy o nazwie *Placeholder.txt*. Umieść następujący tekst w niej: "To jest plik symbolu zastępczego, aby upewnić się, że folder zostanie wdrożona." i Zapisz plik. To wszystko, należy wykonać, aby upewnić się, że program Visual Studio wdroży ten plik i folder, jest on, ponieważ **Build Action** właściwość *.txt* plików jest ustawiona na **zawartości**domyślnie.

## <a name="summary"></a>Podsumowanie

Ukończono wszystkie zadania konfiguracji wdrożenia. W następnym samouczku możesz wdrożyć witrynę Contoso University do środowiska testowego i przetestować.

> [!div class="step-by-step"]
> [Poprzednie](web-config-transformations.md)
> [dalej](deploying-to-iis.md)
