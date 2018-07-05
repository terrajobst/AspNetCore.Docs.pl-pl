---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: tworzenie modeli danych i aplikacji sieci Web | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: a1c4e3365d320ee54d378de33a77666558a854d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398248"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF bazy danych, najpierw z platformą ASP.NET MVC: tworzenie modeli danych i aplikacji sieci Web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych, w oparciu o tabelach bazy danych.


## <a name="create-a-new-aspnet-web-application"></a>Utwórz nową aplikację sieci Web platformy ASP.NET

W nowym rozwiązaniu lub tego samego rozwiązania co projekt bazy danych, Utwórz nowy projekt w programie Visual Studio i wybierz **aplikacji sieci Web ASP.NET** szablonu. Nadaj projektowi nazwę **ContosoSite**.

![Tworzenie projektu](creating-the-web-application/_static/image1.png)

Kliknij przycisk **OK**.

W oknie Nowy projekt ASP.NET wybierz **MVC** szablonu. Możesz wyczyścić **Hostuj w chmurze** opcji teraz, ponieważ wdroży aplikację w chmurze później. Kliknij przycisk **OK** do tworzenia aplikacji.

![Wybierz szablon mvc](creating-the-web-application/_static/image2.png)

Projekt zostanie utworzony przy użyciu domyślnych plików i folderów.

W tym samouczku użyjesz programu Entity Framework 6. W projekcie za pomocą okna Zarządzanie pakietami NuGet, należy dokładnie wersją programu Entity Framework. Jeśli to konieczne, zaktualizuj swoją wersję programu Entity Framework.

![Pokaż wersję](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generowanie modelu

Teraz utworzysz modeli Entity Framework z tabel bazy danych. Te modele są klas, które będą używane do pracy z danymi. Każdy model odzwierciedla tabeli w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.

Kliknij prawym przyciskiem myszy **modeli** folder, a następnie wybierz **Dodaj** i **nowy element**.

![Dodaj nowy element](creating-the-web-application/_static/image4.png)

W oknie Dodaj nowy element, wybierz **danych** w okienku po lewej stronie i **ADO.NET Entity Data Model** spośród opcji w środkowym okienku. Nadaj nowemu plikowi modelu **ContosoModel**.

![Tworzenie modelu](creating-the-web-application/_static/image5.png)

Kliknij przycisk **Dodaj**.

W kreatorze modelu danych jednostki, wybierz **projektancie platformy EF z bazy danych**.

![Generuj z bazy danych](creating-the-web-application/_static/image6.png)

Kliknij przycisk **Dalej**.

Jeśli masz połączenia z bazą danych zdefiniowanych w środowisku deweloperskim, może zostać wyświetlony jeden z tych połączeń wstępnie wybrana. Jednak należy utworzyć nowe połączenie z bazą danych utworzoną w pierwszej części tego samouczka. Kliknij przycisk **nowe połączenie** przycisku.

![nawiązać połączenie z bazą danych](creating-the-web-application/_static/image7.png)

W oknie dialogowym właściwości połączenia należy podać nazwę serwera lokalnego, w którym utworzono bazę danych (w tym przypadku **\ProjectsV12 (localdb)**). Po podaniu nazwy serwera, należy wybrać ContosoUniversityData z dostępnych baz danych.

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

Kliknij przycisk **OK**.

Właściwości połączenia poprawne są teraz wyświetlane. Można użyć domyślnej nazwy dla połączenia w pliku Web.Config

![Ustawienia połączenia](creating-the-web-application/_static/image9.png)

Kliknij przycisk **Dalej**.

Wybierz **tabel** do generowania modeli wszystkie trzy tabele.

![Wybierz tabele](creating-the-web-application/_static/image10.png)

Kliknij przycisk **Zakończ**.

Jeśli pojawi się ostrzeżenie o zabezpieczeniach, wybierz opcję **OK** kontynuowanie działania w szablonie.

Modele są generowane na podstawie tabel bazy danych i wyświetlony diagram pokazuje właściwości i relacje między tabelami.

![Diagram modelu](creating-the-web-application/_static/image11.png)

Folder modeli teraz zawiera wiele nowych plików związanych z modeli, które zostały wygenerowane z bazy danych.

![Pokaż nowe pliki modelu](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs** plik zawiera klasę dziedziczącą po **DbContext** klasy, a także właściwości dla każdej klasy modelu, który odnosi się do tabeli bazy danych. **Course.cs**, **Enrollment.cs**, i **Student.cs** pliki zawierają klasy modeli, które reprezentują tabele bazy danych. Użyjesz klasy modelu i klasy kontekstu podczas pracy z funkcją szkieletów.

Przed kontynuowaniem za pomocą tego samouczka, skompiluj projekt. W następnej sekcji spowoduje wygenerowanie kodu opartego na modeli danych, ale ta sekcja nie będzie działać, jeśli projekt nie został zbudowany.

> [!div class="step-by-step"]
> [Poprzednie](setting-up-database.md)
> [dalej](generating-views.md)
