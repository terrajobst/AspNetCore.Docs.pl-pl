---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie aktualizacji bazy danych serwera SQL - 11, 12 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 898259885da8a089db296bd0f400ee8863877d08
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie aktualizacji bazy danych serwera SQL - 11, 12
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która pokazuje, jak wdrożyć do systemu Windows Azure Web Sites, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

Ten samouczek przedstawia sposób wdrażania aktualizacji bazy danych do pełnej bazy danych programu SQL Server. Ponieważ migracje Code First wykonuje całą pracę aktualizowania bazy danych, proces jest niemal identyczny jak dla programu SQL Server Compact w [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) samouczka.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Dodawanie nowej kolumny do tabeli

W tej części samouczka należy podjąć zmienić bazę danych i odpowiednie zmiany kodu, testowanie ich w programie Visual Studio w ramach przygotowania do wdrażania ich do środowiska testowego i produkcyjnego. Zmiana obejmuje dodanie `OfficeHours` kolumny `Instructor` jednostki i wyświetlanie nowych informacji **instruktorów** strony sieci web.

W projekcie ContosoUniversity.DAL Otwórz *Instructor.cs* i dodaj następującą właściwość między `HireDate` i `Courses` właściwości:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Klasa inicjatora należy zaktualizować tak, aby go nasiona nową kolumnę z danych testowych. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następujący blok kodu, która obejmuje nową kolumnę:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

W projekcie ContosoUniversity Otwórz *Instructors.aspx* i Dodaj nowe pole szablonu dla godzinami tuż przed zamknięciem `</Columns>` tag w pierwszym `GridView` sterowania:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Skompiluj rozwiązanie.

Otwórz **Konsola Menedżera pakietów** okna, a następnie wybierz opcję ContosoUniversity.DAL jako **domyślny projekt**.

Wprowadź następujące polecenia:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Uruchom aplikację i wybierz **instruktorów** strony. Strona ma nieco dłużej niż zwykle załadować, ponieważ ponownie utworzy bazę danych programu Entity Framework, a nasiona go z danych testowych.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska testowego

Użycie migracje Code First, metoda wdrażania zmianę w bazie danych programu SQL Server jest taka sama, jak w przypadku programu SQL Server Compact. Niemniej jednak, należy zmienić testu profil publikowania, ponieważ jest nadal wybrana do migracji z programu SQL Server Compact do programu SQL Server.

Pierwszym krokiem jest usunąć utworzone w poprzednim samouczek przekształceń ciągu połączenia. Te nie są już potrzebne ponieważ określisz przekształcenia ciągu połączenia w profilu publikowania, tak samo jak przed skonfigurowano **Pakuj/Publikuj SQL** kartę do migracji do programu SQL Server.

Otwórz *Web.Test.config* pliku i usuwania `connectionStrings` elementu. Tylko pozostałe transformacji w *Web.Test.config* pliku `Environment` wartość w `appSettings` elementu.

Należy teraz zaktualizować profil publikowania i opublikuj do środowiska testowego.

Otwórz **publikowanie w sieci Web** kreatora, a następnie przejdź do **profilu** kartę.

Wybierz **testu** profilu publikowania.

Wybierz **ustawienia** kartę.

Kliknij przycisk **włączyć nowe ulepszenia publikowania bazy danych**.

W polu Parametry połączenia dla **SchoolContext**, wprowadzić tę samą wartość, które było używane w *Web.Test.config* plik przekształcenia w poprzednich instrukcji:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Wybierz **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**. (W wersji programu Visual Studio, może być oznaczony jako pole wyboru **zastosować migracje Code First**.)

W polu Parametry połączenia dla **połączenia DefaultConnection**, wprowadzić tę samą wartość, które było używane w *Web.Test.config* plik przekształcenia w poprzednich instrukcji:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Pozostaw **aktualizacji bazy danych** wyczyszczone.

Kliknij przycisk **publikowania**.

Visual Studio wdraża zmiany kodu do środowiska testowego i otwiera przeglądarkę na stronę główną Contoso University.

Wybierz stronę instruktorów.

Gdy ta strona jest uruchomiona aplikacja próbuje dostęp do bazy danych. Migracje Code First sprawdza, czy baza danych jest aktualny i stwierdzi, że najnowsze migracji nie zastosowano jeszcze. Migracje Code First zastosowanie najnowszego migracji, uruchamia `Seed` — metoda i strona działa normalnie. Zostanie wyświetlony nową kolumnę godzinami pracy z wprowadzonych danych.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska produkcyjnego

Należy również zmienić profil publikowania dla środowiska produkcyjnego. W takim przypadku zostanie Usuń istniejący profil i utworzyć nową przez zaimportowanie pliku .publishsettings zaktualizowane. Zaktualizowany plik będzie zawierać ciąg połączenia dla bazy danych programu SQL Server w Cytanium.

Jak widać, podczas wdrażania w środowisku testowym, nie są już potrzebne transformacje ciągu połączenia w *Web.Production.config* plik przekształcenia. Otwarcie tego pliku, a następnie usuń `connectionStrings` elementu. Pozostałe przekształceń są przeznaczone dla `Environment` wartość w `appSettings` elementu i `location` element, który ogranicza dostęp do raportów o błędach Elmah.

Przed utworzeniem nowego profilu publikowania do produkcji, Pobierz plik .publishsettings zaktualizowane tak samo jak wcześniej w [wdrażania w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka. (W Panelu sterowania Cytanium kliknij **witryn sieci Web**, a następnie kliknij przycisk **contosouniversity.com** witryny sieci Web. Wybierz **publikowania w sieci Web** , a następnie kliknij pozycję **Pobierz profil publikowania dla tej witryny sieci web**.) Przyczyny, dla której robią to jest aby pobrać parametry połączenia bazy danych w pliku .publishsettings. Parametry połączenia nie była dostępna pobrany plik, ponieważ nadal używa programu SQL Server Compact, a nie utworzono bazy danych programu SQL Server na Cytanium jeszcze po raz pierwszy.

Należy teraz zaktualizować profil publikowania i opublikuj do środowiska produkcyjnego.

Otwórz **publikowanie w sieci Web** kreatora, a następnie przejdź do **profilu** kartę.

Kliknij przycisk **zarządzania profilami**, a następnie usuń profil produkcji.

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.

Otwórz **publikowanie w sieci Web** ponownie kreatora, a następnie kliknij przycisk **importu**.

Na **połączenia** Zmień **docelowy adres URL** na odpowiednią wartość, jeśli używasz tymczasowego adresu URL.

Kliknij przycisk **Dalej**.

Na **ustawienia** , kliknij pozycję **włączyć nowe ulepszenia publikowania bazy danych**.

Na liście rozwijanej ciągu połączenia dla **SchoolContext**, wybierz Cytanium parametry połączenia.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Wybierz **migracje wykonania Code First (wywoływane po uruchomieniu aplikacji)**.

Na liście rozwijanej ciągu połączenia dla **połączenia DefaultConnection**, wybierz Cytanium parametry połączenia.

Wybierz **profilu** , kliknij pozycję **zarządzania profilami**i Zmień nazwę profilu z "contosouniversity.com — narzędzie Web Deploy" na "Production".

Zamknij profil publikowania, aby zapisać wprowadzone zmiany, a następnie otwórz go ponownie.

Kliknij przycisk **publikowania**. (Dla witryny sieci Web rzeczywistej produkcji, należy skopiować *aplikacji\_offline.htm* do miejsca produkcyjnego i umieść go w folderze projektu przed opublikowaniem, następnie usunięcia go po zakończeniu wdrożenia.)

Visual Studio wdraża zmiany kodu do środowiska testowego i otwiera przeglądarkę na stronę główną Contoso University.

Wybierz stronę instruktorów.

Migracje Code First aktualizuje bazę danych w taki sam sposób jak w środowisku testowym. Zostanie wyświetlony nową kolumnę godzinami pracy z wprowadzonych danych.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Teraz pomyślnie wdrożono aktualizacji aplikacji, która obejmuje zmianę bazy danych, korzystanie z bazy danych programu SQL Server.

## <a name="more-information"></a>Więcej informacji

Na tym kończy się tej serii samouczków dotyczących wdrażania aplikacji sieci web ASP.NET do innego dostawcy hostingu. Aby uzyskać więcej informacji o tych tematów objęte te samouczki, zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/en-us/library/bb386521(v=vs.110).aspx) w witrynie MSDN.

## <a name="acknowledgements"></a>Potwierdzeń

Chcę Dziękujemy następujących osób istotny wkład do zawartości z tego samouczka serii:

- [Alberto Poblacion, MVP &amp; MCT, (Hiszpania)](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, dane aplikacji na wiele Platform MVP, Stany Zjednoczone
- Ostrym Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Jan Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Włochy](http://www.iamraf.net/)
- [Rick Anderson firmy Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott myśliwego, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Poprzednie](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[dalej](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
