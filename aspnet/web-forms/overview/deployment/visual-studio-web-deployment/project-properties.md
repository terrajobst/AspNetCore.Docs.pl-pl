---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: właściwości projektu | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: fba3f089bf1693eec873b08b4bc50e3accba06ee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: właściwości projektu
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

Niektóre opcje wdrażania są skonfigurowane we właściwościach projektu, które są przechowywane w pliku projektu ( *.csproj* lub *vbproj* pliku). W większości przypadków domyślne wartości tych ustawień są, co ma, ale może użyć **właściwości projektu** wbudowane interfejsu użytkownika w Visual Studio, aby pracować przy użyciu tych ustawień, jeśli trzeba je zmienić. W tym samouczku można przejrzeć ustawienia wdrażania **właściwości projektu**. Można również utworzyć pliku symbolu zastępczego, który powoduje, że pusty folder, który można wdrożyć.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurowanie ustawień wdrażania w oknie właściwości projektu

Większość ustawień, które mają wpływ na co się dzieje podczas wdrażania znajdują się w profilu publikowania, jak można zauważyć w następujących samouczkach. Kilka ustawień, które należy zwrócić uwagę znajdują się w **pakowania/publikowania** karty **właściwości projektu** okna. Te ustawienia są określone dla każdej konfiguracji kompilacji — to znaczy może mieć różne ustawienia dla kompilacji wydania niż liczba kupionych dla kompilacji debugowania.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **ContosoUniversity** projektu, zaznacz **właściwości**, a następnie wybierz **pakowaniu/publikowaniu Web**kartę.

![Karta pakowaniu/publikowaniu Web](project-properties/_static/image1.png)

Po wyświetleniu okna domyślnie wyświetlane ustawienia dla jednego z tych konfiguracji kompilacji jest obecnie aktywna dla rozwiązania. Jeśli **konfiguracji** pole nie wskazuje **aktywny (wersja)**, wybierz pozycję **wersji** Aby wyświetlić ustawienia konfiguracji kompilacji wydania. Poniżej przedstawiono wdrażania kompilacjami wydania ze środowiskami testowego i produkcyjnego.

![Wybieranie konfiguracji kompilacji wydania](project-properties/_static/image2.png)

Z **aktywny (wersja)** lub **wersji** zaznaczone, zobacz wartości, które są efektywne w przypadku wdrażania przy użyciu konfiguracji kompilacji wydania:

- W **elementy do wdrożenia** okno, **tylko pliki potrzebne do uruchomienia aplikacji** jest zaznaczone. Inne opcje są **wszystkie pliki w tym projekcie** lub **wszystkie pliki w tym folderze projektu**. Pozostawić wybór domyślny, bez zmian można uniknąć, wdrażanie plików kodu źródłowego, np. To ustawienie jest powód, dlaczego foldery zawierające pliki binarne programu SQL Server Compact musiały być dołączony do projektu. Aby uzyskać więcej informacji o tym ustawieniu, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET sieci Web aplikacji projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx).
- **Symbole debugowania Wyklucz wygenerowany** jest zaznaczone. Nie można debugowania używania tej konfiguracji kompilacji.
- **Obejmują wszystkie baz danych skonfigurowanych na karcie pakowanie/publikowanie SQL** jest zaznaczone. Określa, czy program Visual Studio wdroży baz danych, a także pliki. Mimo że pole wyboru etykiety uwagi tylko **Pakuj/Publikuj SQL** kartę, czyszczenie tego pola wyboru spowoduje również wyłączenie wdrażania bazy danych, skonfigurowanego w profilu publikowania. Użytkownik będzie można grozi to który później, pole wyboru musi pozostać zaznaczona. **Pakuj/Publikuj SQL** karcie jest używana do publikowania metodę, która nie będzie używany w tych samouczkach starszej wersji bazy danych.
- **Ustawienia pakietu wdrażania Web** sekcja nie ma zastosowania, ponieważ używasz jednym kliknięciem publikowania w tych samouczkach.

Zmień **konfiguracji** pole listy rozwijanej do debugowania, aby wyświetlić ustawienia domyślne dla kompilacji debugowania. Wartości są takie same, z wyjątkiem **Wyklucz wygenerowano symbole debugowania** jest wyczyszczone, dzięki czemu można debugować po wdrożeniu kompilacji debugowania.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Upewnij się, że Elmah folder zostanie wdrożona

Jak przedstawiono w samouczku poprzedniej [pakietu Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) zapewnia funkcje Błąd rejestrowania i raportowania. W aplikacji Contoso University Elmah został skonfigurowany do przechowywania szczegóły błędu w folderze o nazwie *Elmah*:

![Elmah folder](project-properties/_static/image3.png)

Wykluczenie określonych plików lub folderów z wdrożenia jest typowym wymaganiem; innym przykładem może być folderu, w którym użytkownicy mogą przekazać pliki do. Nie ma plików dziennika lub przekazać pliki, które zostały utworzone w środowisku projektowania wdrożenia do środowiska produkcyjnego. A jeśli wdrażasz aktualizacji w środowisku produkcyjnym nie chcesz, aby proces wdrażania, aby usunąć pliki, które istnieją w środowisku produkcyjnym. (W zależności od tego, jak ustawisz opcję wdrażania, jeśli istnieje plik w lokacji docelowej, ale nie w lokacji źródłowej, podczas wdrażania, narzędzie Web Deploy spowoduje usunięcie go z docelowego).

Jak przedstawiono wcześniej w tym samouczku **elementy do wdrożenia** opcji **pakowaniu/publikowaniu Web** ustawiono kartę **tylko pliki potrzebne do uruchomienia tej aplikacji**. W związku z tym plików dziennika, które są tworzone przez Elmah rozwijany nie zostanie wdrożony, czyli ma nastąpić. (Zostać wdrożony, zostałyby do uwzględnienia w projekcie i ich **Akcja kompilacji** musi mieć ustawioną właściwość **zawartości**. Aby uzyskać więcej informacji, zobacz **Dlaczego nie wszystkie pliki w folderze projektu wdrożony?** w [ASP.NET sieci Web aplikacji projektu wdrożenia — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx)). Jednak narzędzia Web Deploy nie utworzy folder w lokacji docelowej, chyba że istnieje co najmniej jeden plik, aby skopiować na nią. W związku z tym należy dodać *.txt* plik do folderu na działanie jako symbolu zastępczego, dzięki czemu będzie można skopiować folderu.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Elmah* folderu, wybierz opcję **Dodaj nowy element**i Utwórz plik tekstowy o nazwie *Placeholder.txt*. Umieść w nim następujący tekst: "Jest to plik symbolu zastępczego, aby upewnić się, że folder zostanie wdrożona". I Zapisz plik. To wszystko, musisz wykonać, aby mieć pewność, że program Visual Studio wdroży tego pliku i folderu w, ponieważ **Akcja kompilacji** właściwość *.txt* plików ma ustawioną wartość **zawartości**domyślnie.

## <a name="summary"></a>Podsumowanie

Ukończono wszystkie zadania konfiguracji wdrożenia. W następnym samouczku możesz wdrożyć witrynę Contoso University do środowiska testowego i przetestować go brak.

> [!div class="step-by-step"]
> [Poprzednie](web-config-transformations.md)
> [dalej](deploying-to-iis.md)
