---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Kod najpierw migracje i wdrażanie za pomocą programu Entity Framework w aplikacji platformy ASP.NET MVC | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2294f2aba3f765d7849d1f407e85f424dc8b2518
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Kod najpierw migracje i wdrażanie za pomocą programu Entity Framework w aplikacji platformy ASP.NET MVC
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić rzeczywistej aplikacji dla innych osób do użycia w Internecie, należy wdrożyć ją do usługi hosta sieci web. W tym samouczku aplikacja Contoso University w chmurze na platformie Azure zostanie wdrożona.

Samouczek zawiera następujące sekcje:

- Włącz migracje Code First. Funkcja migracji umożliwia zmienić modelu na model danych i wdrażanie zmiany w środowisku produkcyjnym, aktualizując schemat bazy danych bez konieczności porzucenia i ponownego utworzenia bazy danych.
- Wdrażanie na platformie Azure. Ten krok jest opcjonalny; Możesz kontynuować pozostałych samouczków, bez konieczności wdrożony projekt.

Firma Microsoft zaleca, użyj procesu ciągłej integracji z kontroli źródła dla wdrożenia, że w tym samouczku nie obejmuje tych tematów. Aby uzyskać więcej informacji, zobacz [kontroli źródła](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) i [ciągłej integracji](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) rozdziałów [tworzenia rzeczywistych aplikacji w chmurze platformy Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) Książka elektroniczna.

## <a name="enable-code-first-migrations"></a>Włącz migracje Code First

Podczas opracowywania nowej aplikacji modelu danych zmienia często i zawsze zmian modelu, pobiera zsynchronizowane z bazą danych. Skonfigurowano programu Entity Framework, aby automatycznie Porzuć i ponownie utworzyć bazę danych w każdej zmianie modelu danych. Gdy dodać, usunąć, lub zmień klas jednostek lub zmienić Twojego `DbContext` klasy, przy następnym uruchomieniu aplikacji automatycznie usuwa istniejącą bazę danych, tworzy nową, zgodny z modelem, którą nasiona go z danych testowych.

Synchronizacja bazy danych w modelu danych z tej metody działa poprawnie, dopóki możesz wdrożyć aplikację w środowisku produkcyjnym. Gdy aplikacja działa w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które chcesz zachować, a nie chcesz utracić wszystkie zawsze, gdy zostaną wprowadzone zmiany, takie jak dodawanie nowej kolumny. [Migracje Code First](https://msdn.microsoft.com/data/jj591621) funkcji rozwiązuje ten problem, należy włączyć Code First zaktualizować schemat bazy danych zamiast usunięcie i ponowne utworzenie bazy danych. W tym samouczku aplikacja zostanie wdrożona, a do przygotowania tego włączona migracji.

1. Wyłącz inicjatora skonfigurowane wcześniej komentowania lub usuwając `contexts` element, który został dodany do pliku Web.config aplikacji.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Również w aplikacji *Web.config* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Ta zmiana skonfigurowanie projektu, aby pierwszy migracji spowoduje utworzenie nowej bazy danych. Nie jest to wymagane, ale pojawi się później dlaczego jest dobrym rozwiązaniem.
3. Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** , a następnie **Konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. W `PM>` monitu wprowadź następujące polecenia:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![polecenia enable-migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations` Polecenie tworzy *migracje* folderu projektu ContosoUniversity i umieszcza w tym folderze *Configuration.cs* pliku, który można edytować w celu konfigurowania migracji.

    (Jeśli pominięte powyżej krok, który kieruje użytkownika do zmiany nazwy bazy danych migracji znajdzie istniejącą bazę danych i automatycznie `add-migration` polecenia. To normalne, po prostu oznacza to, że nie będzie uruchomienie testu kodu migracji, przed wdrożeniem bazy danych. Nowsze, po uruchomieniu `update-database` polecenia nic się nie stanie, ponieważ baza danych będzie już istnieje.)

    ![Migrations folder](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Klasa inicjatora, który był wyświetlany poprzednio, takich jak `Configuration` klasa zawiera `Seed` metody.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Celem [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metody jest umożliwienie można wstawić ani zaktualizować dane testowe, po pierwszym kod tworzy lub aktualizuje bazę danych. Metoda jest wywoływana po utworzeniu bazy danych, a za każdym razem, gdy schemat bazy danych jest aktualizowany po zmianie modelu danych.

### <a name="set-up-the-seed-method"></a>Konfigurowanie Seed — metoda

Gdy są porzucenie i ponowne utworzenie bazy danych dla każdej zmiany modelu danych, należy użyć klasy inicjatora `Seed` do wstawienia danych testowych, ponieważ po każdej zmianie modelu bazy danych zostanie porzucony i wszystkich danych testowych zostaną utracone. Migracje Code First, test dane zostaną zachowane po wprowadzeniu zmian w bazie danych, więc tym dane testowe w [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) zwykle — metoda nie jest konieczne. W rzeczywistości nie chcesz `Seed` do wstawienia danych testowych, jeśli należy używać migracji do wdrażania bazy danych do środowiska produkcyjnego, ponieważ `Seed` metody zostanie uruchomiony w środowisku produkcyjnym. W takim przypadku ma `Seed` do wstawienia do bazy danych tylko dane potrzebne w środowisku produkcyjnym. Na przykład można znaleźć w bazie danych działu rzeczywistej nazwy w `Department` tabeli po udostępnieniu aplikacji, w środowisku produkcyjnym.

W tym samouczku, należy używać migracje dla wdrożenia, ale Twoje `Seed` metody powoduje wstawienie danych testowych mimo to w celu ułatwienia zobaczyć, jak działa funkcji aplikacji bez konieczności wstawiać ręcznie partii danych.

1. Zastąp zawartość *Configuration.cs* pliku z następującym kodem, który będzie ładowanie danych testowych do nowej bazy danych. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [Inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda przyjmuje jako parametr wejściowy obiekt kontekstu bazy danych i kod w metodzie używa tego obiektu, aby dodać nowe jednostki w bazie danych. Dla poszczególnych typów jednostek kodu tworzy kolekcję nowych jednostek, dodaje je do odpowiednich [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) właściwości, a następnie zapisuje zmiany w bazie danych. Nie trzeba wywołać [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody po każdej grupie jednostek, jak odbywa się w tym miejscu, ale które pomaga Znajdź źródło problemu w przypadku wystąpienia wyjątku, gdy kod jest zapisywania do bazy danych.

    Niektóre z oświadczeń, które wstawiają dane użycia [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodę można wykonać operacji "upsert". Ponieważ `Seed` metoda jest uruchamiana za każdym razem, gdy będzie wykonanie `update-database` polecenia zwykle po każdym migracji, po prostu nie można wstawić danych, ponieważ wierszy chcesz dodać już będą dostępne po pierwszej migracji, które utworzy bazę danych. Operacja "upsert" uniemożliwia błędów, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, ale ***zastępuje*** wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji. Z danych testowych w niektórych tabel nie można się zdarzyć, że: w niektórych przypadkach po zmianie danych podczas testowania ma zmiany po aktualizacji bazy danych. W takim przypadku chcesz wykonać operację wstawiania warunkowe: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje. Seed — metoda korzysta z obu podejść.

    Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć do sprawdzenia, czy wiersz już istnieje. Dla danych uczniów testów, które udostępniasz `LastName` właściwość może być używana w tym celu, ponieważ każdy nazwisko na liście jest unikatowa:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Ten kod przyjęto założenie, że ostatni nazwy są unikatowe. Ręcznie Dodaj uczniów ze zduplikowaną nazwą ostatniego, otrzymasz następujący wyjątek podczas następnego przeprowadzenie migracji.

    Sekwencja zawiera więcej niż jeden element

    Aby uzyskać informacje dotyczące obsługi nadmiarowych danych, takich jak dwóm studentom / o nazwie "Alexander Carson", zobacz [wstępnego wypełniania i bazami danych debugowanie Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona. Aby uzyskać więcej informacji na temat `AddOrUpdate` metody, zobacz [zajmie się za pomocą metody AddOrUpdate EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kod, który tworzy `Enrollment` jednostek zakłada masz `ID` wartość jednostek w `students` kolekcji, chociaż nie zostały ustawione właściwości w kodzie, który tworzy kolekcję.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Można użyć `ID` właściwości tutaj ponieważ `ID` wartość jest ustawiana po wywołaniu `SaveChanges` dla `students` kolekcji. Gdy jednostka do wstawienia do bazy danych i aktualizuje EF automatycznie pobiera wartość klucza podstawowego `ID` właściwości jednostki w pamięci.

    Kod, który dodaje `Enrollment` jednostki do `Enrollments` nie korzysta z zestawu jednostek `AddOrUpdate` metody. Sprawdza, czy jednostka już istnieje i wstawi jednostkę, jeśli nie istnieje. Takie podejście zostanie zachować zmiany wprowadzone do klasy rejestracji przy użyciu interfejsu użytkownika aplikacji. Kod w pętli każdy element członkowski `Enrollment` [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) i jeśli rejestracja nie zostanie znaleziony w bazie danych, dodaje rejestracji z bazą danych. Po raz pierwszy aktualizacji bazy danych, bazy danych jest pusta, tak spowoduje dodanie każdego rejestracji.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Skompiluj projekt.

### <a name="execute-the-first-migration"></a>Wykonanie pierwszej migracji

Kiedy wykonać `add-migration` polecenia migracji wygenerowanego kodu, które mogą utworzyć bazy danych od podstaw. Ten kod jest również w *migracje* folder, w pliku o nazwie  *&lt;sygnatury czasowej&gt;\_InitialCreate.cs*. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych, i `Down` metoda usuwa je.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji. Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.

Jest to początkowej migracji, który został utworzony po wprowadzeniu `add-migration InitialCreate` polecenia. Parametr (`InitialCreate` w przykładzie) jest używane dla pliku nazwy i może być dowolne; zazwyczaj wybierz słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji. Na przykład możesz nazwać nowsze migracji &quot;AddDepartmentTable&quot;.

Jeśli utworzono początkowej migracji, gdy baza danych już istnieje, jest generowany kod tworzenia bazy danych, ale nie ma działać, ponieważ w bazie danych jest już zgodny z modelem danych. Gdy wdrażasz aplikację do innego środowiska, w których bazy danych nie istnieje jeszcze tego kodu zostaną uruchomione, aby utworzyć bazę danych, dlatego jest dobrym rozwiązaniem jest przetestowanie go. Dlatego zmienić nazwę bazy danych w parametrach połączenia wcześniej — tak, aby migracji można utworzyć nową od początku.

1. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenie:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database` Polecenia `Up` metodę w celu utworzenia bazy danych, a następnie go uruchamia `Seed` metodę, aby Wypełnianie bazy danych. Ten sam proces zostanie uruchomiony automatycznie w środowisku produkcyjnym po wdrożeniu aplikacji, jak można zauważyć w następnej sekcji.
- Użyj **Eksploratora serwera** inspekcji bazy danych, tak jak w pierwszym samouczku i uruchom aplikację, aby sprawdzić, czy wszystkie elementy nadal działa tak samo, jak poprzednio.

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić innym użytkownikom za pośrednictwem Internetu, należy wdrożyć ją do usługi hosta sieci web. W tej części samouczka będziesz ją wdrożyć na platformie Azure. Ta sekcja jest opcjonalna. można pominąć to i kontynuować samouczek następujące lub dostosować instrukcje w tej sekcji innego dostawcy hostingu wybranych przez użytkownika.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Wdrażanie bazy danych za pomocą migracje Code First

Aby wdrożyć bazy danych będzie wykonaj migracje Code First. Gdy tworzysz profil publikowania, który służy do konfigurowania ustawień wdrażania w programie Visual Studio, należy wybrać pole wyboru z etykietą **aktualizacji bazy danych**. To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować aplikację *Web.config* plików na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.

Visual Studio nie są wykonywane w bazie danych podczas procesu wdrażania podczas kopiuje projektu na serwerze docelowym. Po uruchomieniu wdrożonej aplikacji i uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First sprawdza, czy bazy danych jest zgodny z modelem danych. Jeśli występuje niezgodność, Code First automatycznie utworzy bazę danych (Jeśli nie istnieje jeszcze) lub aktualizuje schemat bazy danych do najnowszej wersji (jeśli istnieje bazy danych, ale nie jest zgodny z modelem). Jeśli aplikacja korzysta migracje `Seed` metody, uruchamia metody po utworzeniu bazy danych lub schemat jest aktualizowany.

Migracji `Seed` metody wstawia dane testowe. Jeśli są wdrażane w środowisku produkcyjnym, należy zmienić `Seed` metodę, tak że tylko wstawia dane, które ma zostać wstawiony do bazy danych produkcyjnych. Na przykład w bieżącym modelu danych możesz mieć rzeczywistych kursów, ale fikcyjnej studentów w projektowej bazie danych. Można napisać `Seed` metodę, aby załadować zarówno w rozwoju, a następnie Oznacz jako komentarz fikcyjnej studentów przed wdrożeniem w środowisku produkcyjnym. Lub może zapisywać `Seed` metodę, aby załadować tylko kursy, a następnie wprowadź fikcyjnej studentów w bazie danych testu ręcznie przy użyciu interfejsu użytkownika aplikacji.

### <a name="get-an-azure-account"></a>Uzyskaj konto platformy Azure

Konieczne będzie konto platformy Azure. Jeśli nie masz już konto, ale ma subskrypcji programu Visual Studio, możesz [Aktywuj swoje korzyści z subskrypcji](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). W przeciwnym razie możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Tworzenie witryny sieci web i bazy danych SQL na platformie Azure

Aplikacja sieci web na platformie Azure zostanie uruchomiony w środowisku macierzystym udostępnionego, co oznacza, że jest uruchamiany na maszynach wirtualnych (VM), które są współużytkowane z innymi klientami Azure. Udostępniony środowisko macierzyste to sposób ekonomicznych wprowadzenie w chmurze. Później Jeśli powoduje zwiększenie ruchu w sieci web, aplikację można skalować spełnia potrzeby, uruchamiając na dedykowanych maszynach wirtualnych. Aby dowiedzieć się więcej o opcjach ceny dla usługi Azure App Service, zapoznaj się z dokumentacją na [Azure Docs](https://azure.microsoft.com/pricing/details/app-service/)

Poniżej przedstawiono wdrażania bazy danych do bazy danych SQL Azure. Baza danych SQL jest oparta na chmurze usługą relacyjnych baz danych, który jest oparty na technologii programu SQL Server. Narzędzia i aplikacje, które współpracują z programem SQL Server również współpracować z bazy danych SQL.

1. W [portalu zarządzania Azure](https://portal.azure.com), kliknij przycisk **nowy** na karcie po lewej stronie kliknij **zobaczyć wszystkie** w nowym bloku, a następnie kliknij przycisk **aplikacji sieci Web i SQL** w **Web** sekcji i w końcu **Utwórz**.

    ![Przycisk Nowy w portalu zarządzania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

 **Nowej aplikacji sieci Web & SQL — tworzenie** zostanie otwarty Kreator.

2. W bloku, wprowadź ciąg w **Nazwa aplikacji** pole ma być używana jako unikatowy adres URL aplikacji. Pełny adres URL będzie zawierał co wprowadzony w tym miejscu i domyślną domenę usługi aplikacji Azure (. azurewebsites.net). Jeśli **Nazwa aplikacji** jest już zajęty, Kreator wyświetli powiadomienie tego czerwonym znakiem *Nazwa aplikacji jest niedostępna* wiadomości. Jeśli **Nazwa aplikacji** jest dostępny, zostanie wyświetlony na zielonym znacznikiem wyboru.

    ![Utwórz łącze bazy danych w portalu zarządzania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. W **subskrypcji** listy rozwijanej wybierz subskrypcję Azure, w którym mają **usługi aplikacji** mogą teraz być przechowywane.

4. W **grupy zasobów** pole tekstowe, wybierz grupę zasobów lub Utwórz nową. To ustawienie określa centrum danych, które witryny sieci web jest uruchamiana w. Aby uzyskać więcej informacji na temat grup zasobów, zapoznaj się z dokumentacją na [Azure Docs](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Utwórz nową **Plan usługi aplikacji** klikając *sekcji usługi aplikacji*, **Utwórz nowy**i wypełnij **planu usługi aplikacji** (może mieć tej samej nazwie jak Usługi aplikacji), **lokalizacji**, i **warstwa cenowa** (jest bezpłatną opcją).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Kliknij przycisk **bazy danych SQL**i wybierz polecenie *Utwórz nowy* lub wybierz istniejącą bazę danych

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. W **nazwa** wprowadź nazwę bazy danych.
8. Kliknij przycisk **serwer docelowy** wybierz opcję **Utwórz nowy serwer**. Alternatywnie wcześniej utworzonego serwera, można wybrać tego serwera z listy dostępnych serwerów.
9. Wybierz **warstwa cenowa** wybierz *wolne*. Jeśli potrzebne są dodatkowe zasoby, bazy danych można skalować w w dowolnym momencie. Aby dowiedzieć się więcej o cenach SQL Azure, zapoznaj się z dokumentacją na [Azure Docs](https://azure.microsoft.com/pricing/details/sql-database/).
10. Modyfikowanie [sortowania](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) zgodnie z potrzebami.
11. Wprowadź administrator **nazwa użytkownika administratora SQL** i **hasło administratora SQL**. W przypadku wybrania **nowej bazy danych SQL server**, nie wprowadzasz istniejącej nazwy i hasła w tym miejscu, podajesz nową nazwę i hasło definiowane w tej chwili do użycia w przyszłości podczas dostępu do bazy danych. W przypadku wybrania wcześniej utworzonego serwera zostanie wprowadź poświadczenia dla tego serwera.
12. Kolekcja danych telemetrycznych można włączyć dla aplikacji za pomocą usługi Application Insights. Usługi Application Insights z małego konfiguracją zbiera dane zdarzeń cenne, wyjątków, zależności, żądania i informacje o śledzeniu. Aby dowiedzieć się więcej na temat usługi Application Insights, wprowadzenie [Azure Docs](https://azure.microsoft.com/services/application-insights/).
12. Kliknij przycisk **Utwórz** w dolnej części bloku, aby wskazać, że wszystko jest gotowe.
  
 Portal zarządzania zwróci do strony pulpitów nawigacyjnych i **powiadomienia** zawiera blok, w górnej części strony witryny jest tworzona. Po chwili (zazwyczaj mniej niż minutę) będzie powiadomienie, że wdrożenie zakończyło się pomyślnie. Na pasku nawigacyjnym po lewej stronie nowe **usługi aplikacji** pojawia się w *usługi aplikacji* sekcji i nowych **bazy danych SQL** pojawia się w *bazy danych SQL*  sekcji.

### <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **publikowania** z menu kontekstowego.
  
    ![Opublikuj w menu kontekstowego projektu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. W **profilu** karcie **publikowanie w sieci Web** kreatora, kliknij przycisk **Microsoft Azure App Service**.
  
    ![Importowanie ustawień publikowania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Jeśli nie zostały wcześniej dodane subskrypcji platformy Azure w programie Visual Studio, wykonaj kroki na ekranie. Kroki te włączają Visual Studio, aby połączyć się z subskrypcją platformy Azure tak że listę **usługi aplikacji** uwzględni witryny sieci web.
 
4. Wybierz **subskrypcji** możesz dodawać usługi App Service, następnie **planu usługi App Service** folderu usługi aplikacji jest częścią, a na końcu **usługi aplikacji** sam następuje **Ok**.

    ![Wybierz usługę aplikacji](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Po skonfigurowaniu profilu **połączenia** kartę będą wyświetlane. Kliknij przycisk **sprawdzania poprawności połączenia** aby upewnić się, że ustawienia są poprawne

    ![Weryfikacja połączenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
7. Po sprawdzeniu poprawności połączenia zielony znacznik wyboru jest wyświetlany obok pozycji **sprawdzania poprawności połączenia** przycisku. Kliknij przycisk **Dalej**.
  
    ![Pomyślnie zweryfikowane połączenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
8. Otwórz **ciąg połączenia zdalnego** listy rozwijanej w obszarze **SchoolContext** i wybierz parametry połączenia dla bazy danych został utworzony.
9. Wybierz **aktualizacji bazy danych**.

    ![Karta Ustawienia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować aplikację *Web.config* plików na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.
10. Kliknij przycisk **Dalej**.
11. W **Podgląd** , kliknij pozycję **Uruchom Podgląd**.
  
    ![Przycisk StartPreview w karcie podglądu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
 Karcie zostanie wyświetlona lista plików, które zostaną skopiowane na serwer. Wyświetlanie podglądu nie jest wymagane, aby opublikować aplikację, ale jest to funkcja przydatna pod uwagę. W takim przypadku nie trzeba wykonywać żadnych czynności z listy plików, który jest wyświetlany. Przy następnym wdrażania tej aplikacji będzie tylko te pliki, które zostały zmienione na tej liście.
    ![Dane wyjściowe pliku StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

12. Kliknij przycisk **publikowania**.
 Visual Studio rozpoczyna proces kopiowania plików na serwer platformy Azure.
13. **Dane wyjściowe** okna pokazuje, jakie akcje wdrażania zostały pobrane i raporty pomyślnego wdrożenia.
  
    ![Okno danych wyjściowych raportowania pomyślnego wdrożenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
14. Po pomyślnym wdrożeniu przeglądarka domyślna automatycznie otwiera adres URL wdrożonej witryny sieci web.
 Utworzoną aplikację jest teraz uruchomiona w chmurze. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

W tym momencie z *SchoolContext* utworzono bazę danych w bazie danych SQL Azure, ponieważ wybrano **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**. *Web.config* plik wdrożonej witryny sieci web został zmieniony, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjator jest uruchamiana po raz pierwszy kod odczytuje i zapisuje dane w bazie danych (co się stało Jeśli wybrano **studentów** kartę):

![](https://asp.net/media/4367421/mig.png)

Proces wdrażania również utworzyć nowe parametry połączenia *(SchoolContext\_DatabasePublish*) dla migracje Code First na potrzeby aktualizacji schematu bazy danych i wstępne wypełnianie bazy danych.

![Parametry połączenia Database_Publish](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Można znaleźć wdrożoną wersję pliku Web.config na komputerze w *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dostęp do wdrożonych *Web.config* samego pliku przy użyciu protokołu FTP. Aby uzyskać instrukcje, zobacz [wdrożenia sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Postępuj zgodnie z instrukcjami rozpoczynających się od "Aby użyć narzędzia FTP, niezbędne są trzy elementy: adres URL FTP, nazwę użytkownika i hasło."

> [!NOTE]
> Aplikacja sieci web nie zawiera implementacji zabezpieczeń, więc każda osoba, która znajdzie adres URL Zmień dane. Aby uzyskać instrukcje na temat sposobu zabezpieczenie witryny sieci web, zobacz [wdrażanie aplikacji bezpiecznego ASP.NET MVC z członkostwa, OAuth i bazy danych SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Można zapobiec użyciu lokacji za pomocą portalu zarządzania Azure przez inne osoby lub **Eksploratora serwera** w programie Visual Studio można zatrzymać witryny.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Scenariusze migracji zaawansowane

Jeśli wdrożyć bazę danych, uruchamiając migracji automatycznie, jak pokazano w tym samouczku, a następnie wdrażasz do witryny sieci web, która działa na wielu serwerach, można pobrać wielu serwerów w trakcie uruchamiania migracji w tym samym czasie. Migracje są atomic, dwa serwery próby uruchomienia tej samej migracji, co powiedzie się i innych zakończy się niepowodzeniem (przy założeniu, że dwa razy nie można wykonać operacji). W tym scenariuszu jeśli chcesz uniknąć tych problemów, można wywołać ręcznie migracji i skonfigurować własny kod, aby go odbywa się tylko raz. Aby uzyskać więcej informacji, zobacz [pracy i skrypty migracji z kodu](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) na blogu Tomaszewski Rowan i [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (w przypadku wykonywania migracji z wiersza polecenia) w witrynie MSDN.

Aby uzyskać informacje o innych scenariuszy migracji, zobacz [serii Screencast migracje](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Inicjatory pierwszy kodu

W sekcji wdrożenia widać [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjatora używane. Kod najpierw zawiera również inne inicjatory, w tym [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (ustawienie domyślne), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (który użyto wcześniej) i [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicjatora może być przydatne w przypadku konfigurowania warunki dla testów jednostkowych. Można również napisać własny inicjatory i można wywołać inicjatora jawnie, jeśli nie chcesz czekać, aż do aplikacji odczytuje z lub zapisuje w bazie danych. Z chwili utworzenia tego samouczka jest zapisywana w listopadzie 2013 można używać tylko inicjatory tworzenie i DropCreate przed włączeniem migracji. Zespół programu Entity Framework pracuje nad wprowadzania tych inicjatory można używać z także migracji.

Aby uzyskać więcej informacji na temat inicjatory, zobacz [opis inicjatory bazy danych w Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) i rozdział 6 książki [programowania Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) przez Julie Lerman i Tomaszewski Rowan.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób włączyć migracje i wdrażania aplikacji. W następnym samouczku zostaną patrzeć bardziej zaawansowanych tematów, rozwijając modelu danych.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](xref:whitepapers/aspnet-data-access-content-map).

>[!div class="step-by-step"]
[Poprzednie](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
[dalej](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
