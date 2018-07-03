---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji bazy danych serwera SQL - 11, 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 866c871026709a7cccee5fc1c0937120a031d997
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370947"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji bazy danych serwera SQL - 11, 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć do Windows Azure Web Sites, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób wdrażania aktualizacji bazy danych do pełnego bazy danych programu SQL Server. Ponieważ migracje Code First wykonuje całą pracę aktualizowania bazy danych, proces jest niemal identyczny jak dla programu SQL Server Compact w [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) samouczka.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Dodawanie nowej kolumny do tabeli

W tej części samouczka wprowadzisz zmiany bazy danych i odpowiednich zmian w kodzie, Testuj je w programie Visual Studio w ramach przygotowania do wdrażania ich w środowiskach testowych i produkcyjnych. Zmiana obejmuje dodanie `OfficeHours` kolumny `Instructor` jednostki i wyświetlanie nowych informacji w **Instruktorzy** strony sieci web.

W projekcie ContosoUniversity.DAL Otwórz *Instructor.cs* i dodaj następującą właściwość między `HireDate` i `Courses` właściwości:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Klasa inicjatora należy zaktualizować tak, aby go inicjowania inicjuje nową kolumnę z danymi. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z poniższy blok kodu, który zawiera nową kolumnę:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

W projekcie ContosoUniversity Otwórz *Instructors.aspx* i dodać nowe pole szablonu dla godzin pracy zaraz przed zamknięciem `</Columns>` tagu w pierwszym `GridView` sterowania:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Skompiluj rozwiązanie.

Otwórz **Konsola Menedżera pakietów** okna, a następnie wybierz opcję ContosoUniversity.DAL jako **projekt domyślny**.

Wprowadź następujące polecenia:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Uruchom aplikację, a następnie wybierz **Instruktorzy** strony. Strona trwa nieco dłużej niż zwykle załadować, ponieważ platforma Entity Framework ponownie tworzy bazę danych i inicjowania inicjuje ją z danymi.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska testowego

Możesz użyć migracje Code First, metodę wdrażania zmianę w bazie danych programu SQL Server jest takie same jak dla programu SQL Server Compact. Jednak trzeba zmienić testu profil publikowania, ponieważ jest nadal wybrana do migracji z programu SQL Server Compact do programu SQL Server.

Pierwszym krokiem jest do usunięcia przekształcenia ciągu połączenia, które zostały utworzone w poprzednim samouczku. Te nie są już potrzebne, ponieważ w profilu publikowania, należy podać przekształcenia parametry połączenia tak jak przed skonfigurowano **Pakuj/Publikuj SQL** kartę pod kątem migracji do programu SQL Server.

Otwórz *Web.Test.config* pliku i usuwania `connectionStrings` elementu. Tylko transformacji pozostałe w *Web.Test.config* plik jest przeznaczony dla `Environment` wartość w `appSettings` elementu.

Teraz można zaktualizować profilu publikowania i Publikuj do środowiska testowego.

Otwórz **publikowanie w sieci Web** kreatora, a następnie przejdź do **profilu** kartę.

Wybierz **testu** profilu publikowania.

Wybierz **ustawienia** kartę.

Kliknij przycisk **Włącz nowe ulepszenia dotyczące publikowania bazy danych**.

W polu ciągu połączenia dla **SchoolContext**, wprowadź tę samą wartość, która została użyta w *Web.Test.config* plik przekształcenia w poprzednim samouczku:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Wybierz **wykonaj migracje Code First (uruchomieniu aplikacji)**. (W używanej wersji programu Visual Studio może być oznaczony jako pole wyboru **zastosować migracje Code First**.)

W polu ciągu połączenia dla **DefaultConnection**, wprowadź tę samą wartość, która została użyta w *Web.Test.config* plik przekształcenia w poprzednim samouczku:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Pozostaw **Aktualizuj bazę danych** wyczyszczone.

Kliknij przycisk **publikowania**.

Visual Studio wdroży zmian kodu do środowiska testowego i otwiera przeglądarkę na stronę główną University firmy Contoso.

Wybierz stronę instruktorów.

Gdy aplikacja zostanie uruchomiona na tej stronie, próbuje dostęp do bazy danych. Migracje Code First sprawdza, czy baza danych jest aktualny i znajduje się, że najnowsze migracji nie została zastosowana jeszcze. Migracje Code First dotyczy najnowszej migracji, uruchamia `Seed` metody, a następnie stronę działa normalnie. Zobaczysz nową kolumnę godzinami pracy z wprowadzonych danych.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska produkcyjnego

Należy również zmienić profil publikowania dla środowiska produkcyjnego. W tym przypadku będzie Usuń istniejący profil i Utwórz nową, importując plik .publishsettings zaktualizowane. Zaktualizowany plik będzie zawierać parametry połączenia dla bazy danych programu SQL Server w Cytanium.

Jak podczas wdrażania do środowiska testowego, nie potrzebujesz już przekształcenia parametrów połączenia w *Web.Production.config* plik przekształcenia. Otwarcie tego pliku, a następnie usuń `connectionStrings` elementu. Pozostałe przekształcenia są dla `Environment` wartość w `appSettings` elementu i `location` element, który ogranicza dostęp do raportów o błędach Elmah.

Przed utworzeniem nowego profilu publikowania w środowisku produkcyjnym należy pobrać plik .publishsettings zaktualizowane tak samo jak wcześniej w [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka. (W Panelu sterowania Cytanium kliknij **witryn sieci Web**, a następnie kliknij przycisk **contosouniversity.com** witryny sieci Web. Wybierz **publikowania w sieci Web** kartę, a następnie kliknij przycisk **Pobierz profil publikowania dla tej witryny sieci web**.) Powodów, dla którego to robią to aby pobrać parametry połączenia bazy danych w pliku .publishsettings. Parametry połączenia nie była dostępna został pobrany plik, ponieważ był używany program SQL Server Compact i nie zostało jeszcze utworzone bazy danych programu SQL Server w Cytanium po raz pierwszy.

Teraz można zaktualizować profilu publikowania i Publikuj do środowiska produkcyjnego.

Otwórz **publikowanie w sieci Web** kreatora, a następnie przejdź do **profilu** kartę.

Kliknij przycisk **spravovat profily**, a następnie usuń profil produkcji.

Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.

Otwórz **publikowanie w sieci Web** kreatora ponownie, a następnie kliknij przycisk **importu**.

Na **połączenia** kartę, zmień **docelowy adres URL** odpowiednią wartość, korzystając z tymczasowego adresu URL.

Kliknij przycisk **Dalej**.

Na **ustawienia** kliknij pozycję **Włącz nowe ulepszenia dotyczące publikowania bazy danych**.

Na liście rozwijanej ciąg połączenia dla **SchoolContext**, wybierz Cytanium parametry połączenia.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Wybierz **migracje wykonania Code First (uruchomieniu aplikacji)**.

Na liście rozwijanej ciąg połączenia dla **DefaultConnection**, wybierz Cytanium parametry połączenia.

Wybierz **profilu** kliknij pozycję **spravovat profily**i Zmień nazwę profilu z "contosouniversity.com — narzędzie Web Deploy" na "Produkcyjne".

Zamknij profil publikowania, aby zapisać wprowadzone zmiany, a następnie otwórz go ponownie.

Kliknij przycisk **publikowania**. (Rzeczywistej produkcji witryny sieci Web może spowodować skopiowanie *aplikacji\_offline.htm* do produkcji i umieść go w folderze projektu przed opublikowaniem, następnie usunięcia go po zakończeniu wdrożenia.)

Visual Studio wdroży zmian kodu do środowiska testowego i otwiera przeglądarkę na stronę główną University firmy Contoso.

Wybierz stronę instruktorów.

Migracje Code First aktualizuje bazę danych w taki sam sposób jak w środowisku testowym. Zobaczysz nową kolumnę godzinami pracy z wprowadzonych danych.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Teraz pomyślnie wdrożono aktualizację aplikacji, która obejmuje zmianę bazy danych przy użyciu bazy danych programu SQL Server.

## <a name="more-information"></a>Więcej informacji

Na tym kończy się w tej serii samouczków na temat wdrażania aplikacji sieci web ASP.NET u dostawcy hostingu innych firm. Aby uzyskać więcej informacji na temat dowolny z tematów omówione w tych samouczkach zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) w witrynie MSDN.

## <a name="acknowledgements"></a>Potwierdzanie

Chcę otrzymywać podziękować następujących osób, które istotny wkład w zawartości w tej serii samouczków:

- [Alberto Poblacion, MVP &amp; MCT, Hiszpania](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, programowanie na platformie danych MVP, Stany Zjednoczone
- Ostrym Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Włochy](http://www.iamraf.net/)
- [Rick Anderson firmy Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [dalej](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
