---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Konfigurowanie właściwości projektu - 4 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: b8352c1832ffc79db93b6324dd673afaff6b0d74
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30887203"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Konfigurowanie właściwości projektu - 4 12
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Niektóre opcje wdrażania są skonfigurowane we właściwościach projektu, które są przechowywane w pliku projektu ( *.csproj* lub *vbproj* pliku). W większości przypadków domyślne wartości tych ustawień są, co ma, ale może użyć **właściwości projektu** wbudowane interfejsu użytkownika w Visual Studio, aby pracować przy użyciu tych ustawień, jeśli trzeba je zmienić. W tym samouczku można przejrzeć ustawienia wdrażania **właściwości projektu**. Można również utworzyć pliku symbolu zastępczego, który powoduje, że pusty folder, który można wdrożyć.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurowanie ustawień wdrażania w oknie właściwości projektu

Większość ustawień, które mają wpływ na co się dzieje podczas wdrażania znajdują się w profilu publikowania, jak można zauważyć w następujących samouczkach. Kilka ustawień, które należy zwrócić uwagę znajdują się w **pakowania/publikowania** karty **właściwości projektu** okna. Te ustawienia są określone dla każdej konfiguracji kompilacji — to znaczy może mieć różne ustawienia dla kompilacji wydania niż liczba kupionych dla kompilacji debugowania.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, zaznacz **właściwości**, a następnie wybierz **pakowaniu/publikowaniu Web**kartę.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Po wyświetleniu okna domyślnie wyświetlane ustawienia dla jednego z tych konfiguracji kompilacji jest obecnie aktywna dla rozwiązania. Jeśli **konfiguracji** pole nie wskazuje **aktywny (wersja)**, wybierz pozycję **wersji** Aby wyświetlić ustawienia konfiguracji kompilacji wydania. Poniżej przedstawiono wdrażania kompilacjami wydania ze środowiskami testowego i produkcyjnego.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Z **aktywny (wersja)** lub **wersji** zaznaczone, zobacz wartości, które są efektywne w przypadku wdrażania przy użyciu konfiguracji kompilacji wydania:

- W **elementy do wdrożenia** okno, **tylko pliki potrzebne do uruchomienia aplikacji** jest zaznaczone. Inne opcje są **wszystkie pliki w tym projekcie** lub **wszystkie pliki w tym folderze projektu**. Pozostawić wybór domyślny, bez zmian można uniknąć, wdrażanie plików kodu źródłowego, np. To ustawienie jest powód, dlaczego foldery zawierające pliki binarne programu SQL Server Compact musiały być dołączony do projektu. Aby uzyskać więcej informacji o tym ustawieniu, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET sieci Web aplikacji projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx).
- **Symbole debugowania Wyklucz wygenerowany** jest zaznaczone. Nie można debugowania używania tej konfiguracji kompilacji.
- **Wyklucz pliki z aplikacji\_folderu danych** nie jest zaznaczone. Plik programu SQL Server Compact członkostwa bazy danych jest w tym folderze i należy go wdrożyć. Podczas wdrażania aktualizacji, które nie zawierają zmiany dotyczące bazy danych, należy wybrać to pole wyboru.
- **Prekompilowanie tej aplikacji przed opublikowaniem** nie jest zaznaczone. W większości przypadków jest niepotrzebna wstępnej kompilacji projekty aplikacji sieci web. Aby uzyskać więcej informacji na temat tej opcji, zobacz [pakowania/publikowania kartę sieci Web, właściwości projektu](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) i [dialogowe Ustawienia zaawansowane Prekompilowanie](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Obejmują wszystkie baz danych skonfigurowanych na karcie pakowanie/publikowanie SQL** jest zaznaczona, ale tę opcję, jest teraz nieskuteczne, ponieważ nie są konfigurowanie **Pakuj/Publikuj SQL** kartę. Tej karcie jest przeznaczony dla starszej wersji bazy danych metody wdrażania, które były tylko opcja wdrażania baz danych programu SQL Server. Użyjesz **Pakuj/Publikuj SQL** karcie [migracji do programu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) samouczka.
- **Ustawienia pakietu wdrażania Web** sekcja nie ma zastosowania, ponieważ używasz jednym kliknięciem publikowania w tych samouczkach.

Zmień **konfiguracji** pole listy rozwijanej do debugowania, aby wyświetlić ustawienia domyślne dla kompilacji debugowania. Wartości są takie same, z wyjątkiem **Wyklucz wygenerowano symbole debugowania** jest wyczyszczone, dzięki czemu można debugować po wdrożeniu kompilacji debugowania.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Tworzenie Sure, że Elmah Folder zostanie wdrożona

Jak przedstawiono w samouczku poprzedniej [pakietu Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) zapewnia funkcje Błąd rejestrowania i raportowania. W aplikacji Contoso University Elmah został skonfigurowany do przechowywania szczegóły błędu w folderze o nazwie *Elmah*:

![Elmah folder](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Wykluczenie określonych plików lub folderów z wdrożenia jest typowym wymaganiem; innym przykładem może być folderu, w którym użytkownicy mogą przekazać pliki do. Nie ma plików dziennika lub przekazać pliki, które zostały utworzone w środowisku projektowania wdrożenia do środowiska produkcyjnego. A jeśli wdrażasz aktualizacji w środowisku produkcyjnym nie chcesz, aby proces wdrażania, aby usunąć pliki, które istnieją w środowisku produkcyjnym. (W zależności od tego, jak ustawisz opcję wdrażania, jeśli istnieje plik w lokacji docelowej, ale nie w lokacji źródłowej, podczas wdrażania, narzędzie Web Deploy spowoduje usunięcie go z docelowego).

Jak przedstawiono wcześniej w tym samouczku **elementy do wdrożenia** opcji **pakowaniu/publikowaniu Web** ustawiono kartę **tylko pliki potrzebne do uruchomienia tej aplikacji**. W związku z tym plików dziennika, które są tworzone przez Elmah rozwijany nie zostanie wdrożony, czyli ma nastąpić. (Zostać wdrożony, zostałyby do uwzględnienia w projekcie i ich **Akcja kompilacji** musi mieć ustawioną właściwość **zawartości**. Aby uzyskać więcej informacji, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET sieci Web aplikacji projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx)). Jednak narzędzia Web Deploy nie utworzy folder w lokacji docelowej, chyba że istnieje co najmniej jeden plik, aby skopiować na nią. W związku z tym należy dodać *.txt* plik do folderu na działanie jako symbolu zastępczego, dzięki czemu będzie można skopiować folderu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz opcję **Dodaj nowy element**i Utwórz plik o nazwie *Placeholder.txt*. Umieść w nim następujący tekst: "Jest to plik symbolu zastępczego, aby upewnić się, że folder zostanie wdrożona". I Zapisz plik. To wszystko, musisz wykonać, aby mieć pewność, że program Visual Studio wdroży tego pliku i folderu w, ponieważ **Akcja kompilacji** właściwość *.txt* plików ma ustawioną wartość **zawartości**domyślnie.

Ukończono wszystkie zadania konfiguracji wdrożenia. W następnym samouczku możesz wdrożyć witrynę Contoso University do środowiska testowego i przetestować go brak.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
