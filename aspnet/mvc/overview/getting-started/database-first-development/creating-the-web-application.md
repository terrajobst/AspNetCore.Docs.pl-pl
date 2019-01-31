---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Samouczek: Tworzenie aplikacji sieci Web i modeli danych na platformie EF bazy danych pierwszy z platformą ASP.NET MVC'
description: Ten samouczek koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych, w oparciu o tabelach bazy danych.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236370"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Samouczek: Tworzenie aplikacji sieci Web i modeli danych na platformie EF bazy danych pierwszy z platformą ASP.NET MVC

 Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.

Ten samouczek koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych, w oparciu o tabelach bazy danych.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci web ASP.NET
> * Generowanie modelu

## <a name="prerequisites"></a>Wymagania wstępne

* [Wprowadzenie do First w programie Entity Framework 6 bazy danych za pomocą MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Tworzenie aplikacji sieci web ASP.NET

W nowym rozwiązaniu lub tego samego rozwiązania co projekt bazy danych, Utwórz nowy projekt w programie Visual Studio i wybierz **aplikacji sieci Web ASP.NET** szablonu. Nadaj projektowi nazwę **ContosoSite**.

![Tworzenie projektu](creating-the-web-application/_static/image1.png)

Kliknij przycisk **OK**.

W oknie Nowy projekt ASP.NET wybierz **MVC** szablonu. Możesz wyczyścić **Hostuj w chmurze** opcji teraz, ponieważ wdroży aplikację w chmurze później. Kliknij przycisk **OK** do tworzenia aplikacji.

Projekt zostanie utworzony przy użyciu domyślnych plików i folderów.

W tym samouczku użyjesz programu Entity Framework 6. W projekcie za pomocą okna Zarządzanie pakietami NuGet, należy dokładnie wersją programu Entity Framework. Jeśli to konieczne, zaktualizuj swoją wersję programu Entity Framework.

![Pokaż wersję](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generowanie modelu

Teraz utworzysz modeli Entity Framework z tabel bazy danych. Te modele są klas, które będą używane do pracy z danymi. Każdy model odzwierciedla tabeli w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.

Kliknij prawym przyciskiem myszy **modeli** folder, a następnie wybierz **Dodaj** i **nowy element**.

W oknie Dodaj nowy element, wybierz **danych** w okienku po lewej stronie i **ADO.NET Entity Data Model** spośród opcji w środkowym okienku. Nadaj nowemu plikowi modelu **ContosoModel**.

Kliknij przycisk **Dodaj**.

W kreatorze modelu danych jednostki, wybierz **projektancie platformy EF z bazy danych**.

Kliknij przycisk **Dalej**.

Jeśli masz połączenia z bazą danych zdefiniowanych w środowisku deweloperskim, może zostać wyświetlony jeden z tych połączeń wstępnie wybrana. Jednak należy utworzyć nowe połączenie z bazą danych utworzoną w pierwszej części tego samouczka. Kliknij przycisk **nowe połączenie** przycisku.

W oknie dialogowym właściwości połączenia należy podać nazwę serwera lokalnego, w którym utworzono bazę danych (w tym przypadku **\Projects13 (localdb)**). Po podaniu nazwy serwera, należy wybrać ContosoUniversityData z dostępnych baz danych.

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

Kliknij przycisk **OK**.

Właściwości połączenia poprawne są teraz wyświetlane. Można użyć domyślnej nazwy dla połączenia w pliku Web.Config.

Kliknij przycisk **Dalej**.

Wybierz najnowszą wersję programu Entity Framework.

Kliknij przycisk **Dalej**.

Wybierz **tabel** do generowania modeli wszystkie trzy tabele.

Kliknij przycisk **Zakończ**.

Jeśli pojawi się ostrzeżenie o zabezpieczeniach, wybierz opcję **OK** kontynuowanie działania w szablonie.

Modele są generowane na podstawie tabel bazy danych i wyświetlony diagram pokazuje właściwości i relacje między tabelami.

![Diagram modelu](creating-the-web-application/_static/image11.png)

Folder modeli teraz zawiera wiele nowych plików związanych z modeli, które zostały wygenerowane z bazy danych.

**ContosoModel.Context.cs** plik zawiera klasę dziedziczącą po **DbContext** klasy, a także właściwości dla każdej klasy modelu, który odnosi się do tabeli bazy danych. **Course.cs**, **Enrollment.cs**, i **Student.cs** pliki zawierają klasy modeli, które reprezentują tabele bazy danych. Użyjesz klasy modelu i klasy kontekstu podczas pracy z funkcją szkieletów.

Przed kontynuowaniem za pomocą tego samouczka, skompiluj projekt. W następnej sekcji spowoduje wygenerowanie kodu opartego na modeli danych, ale ta sekcja nie będzie działać, jeśli projekt nie został zbudowany.

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utworzona aplikacja internetowa platformy ASP.NET
> * Wygenerowany modele

Przejdź do następnego samouczka, aby dowiedzieć się, jak utworzyć wygenerowanie kodu opartego na modeli danych.
> [!div class="nextstepaction"]
> [Generowanie widoków](generating-views.md)