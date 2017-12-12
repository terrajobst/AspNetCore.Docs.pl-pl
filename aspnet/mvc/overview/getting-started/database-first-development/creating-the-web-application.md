---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Baza danych EF najpierw o platformie ASP.NET MVC: tworzenie modeli danych i aplikacji sieci Web | Dokumentacja firmy Microsoft'
author: tfitzmac
description: "Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: f495bfa3aa5332e4ca3e44c2ffbfb760fbbeafc8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>Baza danych EF najpierw o platformie ASP.NET MVC: tworzenie modeli danych i aplikacji sieci Web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych oparte na tabelach bazy danych.


## <a name="create-a-new-aspnet-web-application"></a>Tworzenie nowej aplikacji sieci Web ASP.NET

Nowe rozwiązanie lub tego samego rozwiązania co projekt bazy danych, Utwórz nowy projekt w programie Visual Studio i wybierz **aplikacji sieci Web ASP.NET** szablonu. Nazwij projekt **ContosoSite**.

![Tworzenie projektu](creating-the-web-application/_static/image1.png)

Kliknij przycisk **OK**.

W oknie Nowy projekt ASP.NET, wybierz **MVC** szablonu. Możesz wyczyścić **Hostuj w chmurze** opcję teraz, ponieważ wdrożysz aplikację w chmurze później. Kliknij przycisk **OK** do tworzenia aplikacji.

![Wybierz szablon mvc](creating-the-web-application/_static/image2.png)

Projekt jest tworzony z domyślnych plików i folderów.

W tym samouczku użyjesz Entity Framework 6. Sprawdź z wersji programu Entity Framework, w projekcie za pośrednictwem okna Zarządzanie pakietami NuGet. W razie potrzeby zaktualizuj swoją wersję programu Entity Framework.

![Pokaż wersji](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generowanie modeli

Teraz utworzysz modeli Entity Framework tabel bazy danych. Te modele są klasy, które będą używane do pracy z danymi. Każdy model odzwierciedla tabeli w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.

Kliknij prawym przyciskiem myszy **modele** folder, a następnie wybierz **Dodaj** i **nowy element**.

![Dodaj nowy element](creating-the-web-application/_static/image4.png)

W oknie Dodaj nowy element, wybierz **danych** w okienku po lewej stronie i **modelu danych jednostki ADO.NET** opcje w środkowym okienku. Nazwa nowego pliku modelu **ContosoModel**.

![Tworzenie modelu](creating-the-web-application/_static/image5.png)

Kliknij przycisk **Dodaj**.

W kreatorze modelu danych jednostki, wybierz **EF Designer z bazy danych**.

![Generowanie na podstawie bazy danych](creating-the-web-application/_static/image6.png)

Kliknij przycisk **Dalej**.

Jeśli masz połączenia z bazą danych zdefiniowany w ramach środowiska deweloperskiego, może zostać wyświetlony jeden z tych połączeń wstępnie zaznaczona. Jednak użytkownik chce utworzyć nowe połączenie do bazy danych utworzonej w pierwszej części tego samouczka. Kliknij przycisk **nowe połączenie** przycisku.

![łączenia z bazą danych](creating-the-web-application/_static/image7.png)

W oknie właściwości połączenia należy podać nazwę serwera lokalnego, w którym utworzono bazę danych (w tym przypadku **\ProjectsV12 (localdb)**). Po podaniu nazwy serwera, wybierz ContosoUniversityData z dostępnych baz danych.

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

Kliknij przycisk **OK**.

Właściwości połączenia poprawne są teraz wyświetlane. Można użyć domyślna nazwa połączenia w pliku Web.Config

![Ustawienia połączenia](creating-the-web-application/_static/image9.png)

Kliknij przycisk **Dalej**.

Wybierz **tabel** do generowania modeli wszystkie trzy tabele.

![Wybierz tabele](creating-the-web-application/_static/image10.png)

Kliknij przycisk **Zakończ**.

Jeśli pojawi się ostrzeżenie zabezpieczeń, wybierz **OK** kontynuowanie działania w szablonie.

Modele są generowane na podstawie tabel bazy danych i wyświetlony diagramie przedstawiono relacje między tabelami i właściwości.

![Diagram modelu](creating-the-web-application/_static/image11.png)

Folder modeli teraz zawiera wiele nowych plików związanych z modelami, które zostały wygenerowane z bazy danych.

![Pokaż nowych plików modelu](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs** plik zawiera klasę pochodzącą z **DbContext** klasy, a także właściwością dla każdej klasy modelu, która odnosi się do tabeli bazy danych. **Course.cs**, **Enrollment.cs**, i **Student.cs** pliki zawierają klasy modeli, które reprezentują tabel bazy danych. W klasie kontekstu i klasy modelu będzie używany podczas pracy z funkcją szkieletów.

Przed wykonaniem tego samouczka należy skompilować projekt. W następnej sekcji spowoduje wygenerowanie kodu opartego na modeli danych, ale ta sekcja nie będzie działać, jeśli projekt nie został skompilowany.

>[!div class="step-by-step"]
[Poprzednie](setting-up-database.md)
[dalej](generating-views.md)
