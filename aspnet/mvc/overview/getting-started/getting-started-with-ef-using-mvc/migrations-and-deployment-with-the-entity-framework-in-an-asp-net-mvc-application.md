---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Kod najpierw migracje i wdrażanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 836711b05599087b540ad16bb5f462dd6baa287f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400221"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Kod najpierw migracje i wdrażanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić rzeczywistej aplikacji innym osobom korzystanie przez Internet, należy wdrożyć ją do dostawcy usług hosta sieci web. W tym samouczku wdrożysz aplikacji Contoso University do chmury na platformie Azure.

Samouczek zawiera następujące sekcje:

- Włącz migracje Code First. Funkcja migracji umożliwia Zmień model danych, a następnie wdrożyć swoje zmiany do środowiska produkcyjnego, aktualizowanie schematu bazy danych bez konieczności porzucenia i ponownego utworzenia bazy danych.
- Wdrażanie na platformie Azure. Ten krok jest opcjonalny; Możesz kontynuować pozostałych samouczków, bez konieczności wdrożony projekt.

Zaleca się, że można korzystać z procesu ciągłej integracji z kontrolą źródła do wdrożenia, ale w tym samouczku nie pokrywa się z tymi tematami. Aby uzyskać więcej informacji, zobacz [kontroli źródła](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) i [ciągłej integracji](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) rozdziały [tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) książki elektronicznej.

## <a name="enable-code-first-migrations"></a>Włącz migracje Code First

Podczas tworzenia nowej aplikacji swój model danych zmienia często i za każdym razem zmiany modelu, otrzymuje zsynchronizowana z bazą danych. Skonfigurowano programu Entity Framework, aby automatycznie porzucenia i ponownego utworzenia bazy danych po każdej zmianie modelu danych. Gdy możesz dodać, usunąć, lub zmienić klas jednostek lub zmień swoje `DbContext` klasy przy następnym uruchomieniu aplikacji automatycznie usuwa istniejącą bazę danych, utworzony zostaje nowy indeks, który odpowiada modelu i inicjowania inicjuje ją z danymi.

Ta metoda synchronizacja bazy danych z modelem danych działa poprawnie, dopóki nie możesz wdrożyć aplikację do środowiska produkcyjnego. Gdy aplikacja jest uruchomiona w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które mają być przechowywane i nie chcesz stracić wszystko, każdym razem, gdy zostaną wprowadzone zmiany, takie jak dodawanie nowej kolumny. [Migracje Code First](https://msdn.microsoft.com/data/jj591621) funkcja rozwiązuje ten problem, włączając Code First zaktualizować schemat bazy danych zamiast porzucenie i ponowne utworzenie bazy danych. W tym samouczku wdrożysz aplikację, a do Państwu w przygotowaniu się do tego umożliwi migracji.

1. Wyłącz inicjatora, skonfigurowanej wcześniej zakomentowując lub usuwając `contexts` element, który został dodany do pliku Web.config aplikacji.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Również w aplikacji *Web.config* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Ta zmiana konfiguruje projekt tak, aby pierwszej migracji utworzy nową bazę danych. Nie jest to wymagane, ale zostanie wyświetlony później dlaczego jest dobrym pomysłem.
3. Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** i następnie **Konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. W `PM>` wierszu polecenia wprowadź następujące polecenia:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![polecenia enable-migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations` Polecenie tworzy *migracje* folderu w projekcie ContosoUniversity i umieszcza w tym folderze *Configuration.cs* pliku, który można edytować w celu konfigurowania migracji.

    (Jeśli Ci się przeoczyć poprzednim kroku, który kieruje użytkownika do zmiany nazwy bazy danych, migracje znajdzie istniejącą bazę danych i automatycznie `add-migration` polecenia. To normalne, po prostu oznacza to, że nie będzie uruchomienie testu kod migracji, przed przystąpieniem do wdrażania bazy danych. Nowsze, po uruchomieniu `update-database` polecenia nic się nie stanie, ponieważ ta baza danych będzie już istnieje.)

    ![Folder migracji](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Klasa inicjatora, która wcześniej, takich jak `Configuration` klasa zawiera `Seed` metody.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Celem [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodą jest umożliwienie wstawiania lub aktualizacji danych testowych po Code First tworzy lub aktualizuje bazę danych. Metoda jest wywoływana po utworzeniu bazy danych i za każdym razem, gdy schemat bazy danych jest aktualizowana po zmianie modelu danych.

### <a name="set-up-the-seed-method"></a>Konfigurowanie Seed — metoda

Kiedy są porzucenie i ponowne utworzenie bazy danych dla każdej zmiany w modelu danych, możesz użyć klasy inicjatora `Seed` metodę, aby wstawić dane testowe, ponieważ po każdej zmianie modelu bazy danych jest porzucany i wszystkie dane z badań zostaną utracone. Migracje Code First, test, dane są zachowywane po wprowadzeniu zmian w bazie danych, więc łącznie danych testowych w [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) zazwyczaj metoda nie jest konieczne. W rzeczywistości nie chcesz `Seed` metodę, aby wstawić dane testowe, jeśli będziesz korzystać z migracji do wdrażania bazy danych w środowisku produkcyjnym, ponieważ `Seed` metoda jest uruchamiana w środowisku produkcyjnym. W takim przypadku chcesz `Seed` metody do wstawienia do bazy danych tylko potrzebne dane w środowisku produkcyjnym. Na przykład można znaleźć nazwy rzeczywistych działów w w bazie danych `Department` tabeli, gdy aplikacja staje się dostępna w środowisku produkcyjnym.

W tym samouczku będziesz korzystać z migracji do wdrożenia, Twoja `Seed` metody powoduje wstawienie danych testowych mimo to ułatwi można zobaczyć, jak działa funkcji aplikacji bez konieczności ręcznego Wstaw dużej ilości danych.

1. Zastąp zawartość *Configuration.cs* pliku następującym kodem, który załaduje dane z badań do nowej bazy danych. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [Inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda przyjmuje obiekt kontekstu bazy danych jako parametr wejściowy, a kod w metodzie używa tego obiektu, aby dodać nowe jednostki w bazie danych. Dla każdego typu jednostki, ten kod tworzy kolekcję nowe jednostki, dodaje je do odpowiednich [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) właściwości, a następnie zapisuje zmiany w bazie danych. Nie trzeba wywoływać [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody po każdej grupie jednostek, tak jak to zrobić w tym miejscu, ale sposób, który pomoże Ci Znajdź źródło problemu, jeśli wystąpi wyjątek podczas pisania kodu w bazie danych.

    Niektóre instrukcje, które wstawiają dane wykorzystują [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody, które można wykonać operacji "upsert". Ponieważ `Seed` metoda jest uruchamiana za każdym razem, gdy wykonaniu `update-database` poleceń, zwykle po każdym migracji danych, nie można wstawić po prostu, ponieważ wierszy próbujesz dodać będą już dostępne po pierwszej migracji, który tworzy bazę danych. Operacja "upsert" uniemożliwia błędów, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, ale ***zastępuje*** wszelkie zmiany w danych, które mogły zostać wprowadzone podczas testowania aplikacji. Zawierającej dane testowe w niektórych tabel nie można było to możliwe: w niektórych przypadkach po zmianie danych podczas testowania mają zmiany po aktualizacji bazy danych. W takim przypadku chcesz zrobić to operacja wstawiania warunkowego: Wstaw wiersz, tylko wtedy, gdy jeszcze nie istnieje. Seed — metoda korzysta z obu metod.

    Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć, aby sprawdzić, czy wiersz już istnieje. Dla danych student testu, które są podawane `LastName` właściwość może być używana w tym celu, ponieważ każdy nazwisko na liście jest unikatowa:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Ten kod zakłada, że ostatni nazwy są unikatowe. Jeśli ręcznie dodać uczniów o zduplikowanej nazwie ostatniego, otrzymasz następujący wyjątek podczas następnego przeprowadzić migrację.

    Sekwencja zawiera więcej niż jeden element

    Aby dowiedzieć się, jak obsługiwać nadmiarowych danych, takich jak dwóm studentom/uczniom o nazwie "Alexander Carson", zobacz [wstępnego wypełniania i baz danych debugowania Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona. Aby uzyskać więcej informacji na temat `AddOrUpdate` metody, zobacz [powinien zachować ostrożność przy użyciu metody AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kod, który tworzy `Enrollment` jednostek przyjęto założenie, możesz `ID` wartości w jednostkach w `students` kolekcji, chociaż nie ustawisz tę właściwość w kodzie, który tworzy kolekcję.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Możesz użyć `ID` tutaj właściwość ponieważ `ID` wartość jest ustawiana podczas wywoływania `SaveChanges` dla `students` kolekcji. EF automatycznie pobiera wartość klucza podstawowego, gdy jednostka do wstawienia do bazy danych i aktualizuje `ID` właściwości obiektu w pamięci.

    Kod, który dodaje `Enrollment` jednostki do `Enrollments` nie korzysta z zestawu jednostek `AddOrUpdate` metody. Sprawdza, czy jednostka już istnieje i wstawi jednostkę, jeśli nie istnieje. To podejście zachowa zmiany wprowadzone do rejestracji klasy korporacyjnej przy użyciu interfejsu użytkownika aplikacji. Kod w pętli każdy element członkowski `Enrollment` [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) i jeśli rejestracja nie zostanie znaleziony w bazie danych, dodaje rejestracji z bazą danych. Po raz pierwszy można zaktualizować bazy danych, bazy danych jest pusta, więc spowoduje to dodanie każdej rejestracji.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Skompiluj projekt.

### <a name="execute-the-first-migration"></a>Wykonanie pierwszej migracji

Kiedy wykonać `add-migration` polecenia migracji wygenerowany kod, który może utworzyć bazę danych od podstaw. Ten kod jest również w *migracje* folderu, w pliku o nazwie  *&lt;sygnatura czasowa&gt;\_InitialCreate.cs*. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych i `Down` metoda usuwa je.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji. Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody.

Jest to początkowej migracji, który został utworzony po wprowadzeniu `add-migration InitialCreate` polecenia. Parametr (`InitialCreate` w przykładzie) służy do pliku nazwy i może być dowolnie; zazwyczaj wybierz wyraz lub frazę, który podsumowuje, co to jest wykonywana w procesie migracji. Na przykład można nazwać późniejszej migracji &quot;AddDepartmentTable&quot;.

Jeśli utworzono początkowej migracji bazy danych już istnieje, kod tworzenia bazy danych jest generowany, ale nie ma działać, ponieważ baza danych już jest zgodny z modelem danych. Podczas wdrażania aplikacji do innego środowiska, w którym baza danych nie istnieje jeszcze uruchamiane ten kod, aby utworzyć bazę danych, więc to dobry pomysł, aby przetestować go najpierw. Dlatego właśnie zmieniono nazwę bazy danych w parametrach połączenia wcześniej — tak aby migracji można utworzyć nową od podstaw.

1. W **Konsola Menedżera pakietów** okna, wprowadź następujące polecenie:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database` Polecenia `Up` uruchamia metodę w celu utworzenia bazy danych, a następnie go `Seed` metodę do wypełniania bazy danych. Ten sam proces zostanie uruchomiony automatycznie w środowisku produkcyjnym po wdrożeniu aplikacji, ponieważ znajduje się w poniższej sekcji.
2. Użyj **Eksploratora serwera** inspekcji bazy danych, tak jak w pierwszym samouczku i uruchomić aplikację, aby zweryfikować, że wszystko nadal działa tak samo jak wcześniej.

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

Do tej pory aplikacja była uruchomiona lokalnie w usługach IIS Express na komputerze deweloperskim. Aby udostępnić innym osobom korzystanie przez Internet, należy je wdrożyć do dostawcy usług hosta sieci web. W tej części samouczka będziesz jej wdrażanie na platformie Azure. Ta sekcja jest opcjonalna. Możesz pominąć to i przejdź do następującego samouczka lub możesz dostosować instrukcje w tej sekcji dla innego dostawcy hostingu wybranych przez użytkownika.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Wdrażanie bazy danych przy użyciu migracje Code First

Aby wdrożyć bazy danych będzie użyć migracje Code First. Gdy tworzysz profil publikowania, który umożliwia konfigurowanie ustawień dla wdrażania z programu Visual Studio, należy wybrać pole wyboru z etykietą **Aktualizuj bazę danych**. To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować aplikację *Web.config* plików na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.

Program Visual Studio nie wykonuje żadnych czynności z bazą danych w procesie wdrażania podczas kopiuje projektu na serwerze docelowym. Po uruchomieniu wdrożonej aplikacji, i uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First sprawdza, czy baza danych zgodne z modelem danych. W przypadku niezgodności Code First automatycznie tworzy bazę danych (Jeśli nie istnieje jeszcze) lub aktualizuje schemat bazy danych do najnowszej wersji (Jeśli baza danych istnieje, ale nie jest zgodny z modelem). Jeśli aplikacja implementuje migracje `Seed` metody, metoda uruchomienia po utworzeniu bazy danych lub schemat jest aktualizowany.

Migracji `Seed` metoda wstawia dane z badań. Jeśli były wdrażane w środowisku produkcyjnym, musisz zmienić `Seed` metodę, tak aby tylko wstawia dane, który ma zostać wstawiony do produkcyjnej bazy danych. Na przykład w bieżącym modelu danych warto mieć rzeczywistych kursów, ale fikcyjnej studentów w bazie danych rozwoju. Można napisać `Seed` metodę, aby załadować zarówno w rozwoju, a następnie przekształcić w komentarz fikcyjnej studentów przed wdrożeniem w środowisku produkcyjnym. Lub możesz napisać `Seed` metodę, aby załadować tylko kursów, a następnie wprowadź fikcyjnej studentów w bazie danych testu ręcznie przy użyciu interfejsu użytkownika aplikacji.

### <a name="get-an-azure-account"></a>Uzyskaj konto platformy Azure

Będziesz potrzebować konta platformy Azure. Jeśli nie masz jeszcze jeden, ale masz subskrypcję programu Visual Studio, możesz to zrobić [Aktywuj korzyści z subskrypcji](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). W przeciwnym razie można utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Tworzenie witryny sieci web i bazy danych SQL na platformie Azure

Aplikacji sieci web na platformie Azure jest uruchamiana w środowisku współdzielonym hostingu, co oznacza, że działa na maszynach wirtualnych (VM), które są współużytkowane z innymi klientami platformy Azure. Udostępnione Środowisko hostingu jest sposób niskie koszty, aby rozpocząć pracę w chmurze. Później Jeśli powoduje zwiększenie ruchu w sieci web, aplikację można skalować do spełnia potrzeby, uruchamiając na dedykowanych maszynach wirtualnych. Aby dowiedzieć się więcej na temat opcji cen dla usługi Azure App Service, zapoznaj się dokumentacją dotyczącą [Azure Docs](https://azure.microsoft.com/pricing/details/app-service/)

Poniżej przedstawiono wdrażania bazy danych do usługi Azure SQL Database. SQL Database to usługa relacyjnej bazy danych opartej na chmurze, która jest oparta na technologiach programu SQL Server. Narzędzia i aplikacje, które działają z programem SQL Server również działać z usługą SQL Database.

1. W [portalu zarządzania systemu Azure](https://portal.azure.com), kliknij przycisk **nowy** na karcie po lewej stronie kliknij **holograficznych** w nowym bloku, a następnie kliknij **aplikacji sieci Web i SQL** w **Web** sekcji i w końcu **Utwórz**.

    ![Przycisk Nowy w portalu zarządzania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **Nowej aplikacji sieci Web i SQL - Utwórz** zostanie otwarty Kreator.

2. W bloku, wprowadź ciąg w **nazwy aplikacji** pole do użycia jako unikatowy adres URL aplikacji. Pełny adres URL będzie obejmować co należy wprowadzić w tym miejscu oraz domyślnej domeny usługi Azure App Services (. azurewebsites.net). Jeśli **nazwy aplikacji** jest już zajęta, Kreator zostanie wyświetlone powiadomienie o tym za pomocą czerwonego *Nazwa aplikacji jest niedostępna* wiadomości. Jeśli **nazwy aplikacji** jest dostępna, zostanie wyświetlony na zielony znacznik wyboru.

    ![Tworzenie za pomocą łącza bazy danych w portalu zarządzania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. W **subskrypcji** listy rozwijanej wybierz subskrypcję Azure, w którym chcesz **usługi App Service** się znajdować.

4. W **grupy zasobów** pola tekstowego, wybierz grupę zasobów lub Utwórz nową. To ustawienie określa, które centrum danych witryny sieci web jest uruchamiana w. Aby uzyskać więcej informacji na temat grup zasobów, zapoznaj się dokumentacją dotyczącą [Azure Docs](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Utwórz nową **Plan usługi App Service** , klikając *sekcji usługi App Service*, **Utwórz nowy**, a następnie wypełnij **planu usługi App Service** (może mieć takiej samej nazwie jak Usługa App Service), **lokalizacji**, i **warstwa cenowa** (jest bezpłatną opcją).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Kliknij przycisk **bazy danych SQL**i wybierz polecenie *Utwórz nowy* lub wybierz istniejącą bazę danych

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. W **nazwa** wprowadź nazwę bazy danych.
8. Kliknij przycisk **serwer docelowy** wybierz opcję **Tworzenie nowego serwera**. Alternatywnie wcześniej utworzonego serwera, można wybrać tego serwera z listy dostępnych serwerów.
9. Wybierz **warstwa cenowa** wybierz pozycję *bezpłatna*. Jeśli potrzebne są dodatkowe zasoby, bazy danych można skalować w w dowolnym momencie. Aby dowiedzieć się więcej na temat cen usługi Azure SQL, zapoznaj się dokumentacją dotyczącą [Azure Docs](https://azure.microsoft.com/pricing/details/sql-database/).
10. Modyfikowanie [sortowania](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) zgodnie z potrzebami.
11. Wprowadź administrator **nazwa użytkownika administratora SQL** i **hasło administratora SQL**. W przypadku wybrania **nowy serwer SQL Database**nie wprowadzasz istniejącej nazwy i hasła w tym miejscu, podajesz nową nazwę i hasło, które definiujesz teraz do użycia w przyszłości, gdy uzyskujesz dostęp do bazy danych. Jeśli został wybrany serwer, który został utworzony wcześniej, wprowadzisz poświadczenia dla tego serwera.
12. Zbieranie danych telemetrycznych można włączyć dla usługi App Service za pomocą usługi Application Insights. Application Insights przy użyciu konfiguracji nieco gromadzi cenne zdarzeń, wyjątków, zależności, żądania i informacje o śledzeniu. Aby dowiedzieć się więcej na temat usługi Application Insights, Rozpocznij pracę w [Azure Docs](https://azure.microsoft.com/services/application-insights/).
13. Kliknij przycisk **Utwórz** w dolnej części bloku aby wskazać, że jesteś gotowy.
  
    Portal zarządzania zwróci do strony pulpitów nawigacyjnych i **powiadomienia** bloku, w górnej części strony pokazuje, że witryna jest tworzona. Po chwili (zazwyczaj mniej niż minutę) będą powiadomienie o pomyślnym wdrożeniu. Na pasku nawigacyjnym po lewej stronie nowe **usługi App Service** pojawia się w *App Services* sekcji, a nowe **bazy danych SQL** pojawia się w *baz danych SQL*  sekcji.

### <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj** z menu kontekstowego.
  
    ![Opublikuj w menu kontekstowego projektu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. W **profilu** karcie **publikowanie w sieci Web** kreatora, kliknij przycisk **Microsoft Azure App Service**.
  
    ![Importowanie ustawień publikowania](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Jeśli Twoja subskrypcja platformy Azure nie zostały wcześniej dodane w programie Visual Studio, wykonaj kroki na ekranie. Kroki te włączają Visual Studio, aby nawiązać połączenie z subskrypcją platformy Azure tak, na liście **App Services** będzie zawierać witryny sieci web.
 
4. Wybierz **subskrypcji** możesz dodawać usługi App Service, następnie **planu usługi App Service** folder usługi App Service jest częścią programu, a na koniec **usługi App Service** sam następuje **Ok**.

    ![Wybierz usługi App Service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Po skonfigurowaniu profilu **połączenia** kartę będą wyświetlane. Kliknij przycisk **Waliduj połączenie** aby upewnić się, że ustawienia są poprawne

    ![Weryfikowanie połączenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Po zweryfikowaniu połączenia, zielony znacznik wyboru jest wyświetlany obok pozycji **Waliduj połączenie** przycisku. Kliknij przycisk **Dalej**.
  
    ![Pomyślnie zweryfikowane połączenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Otwórz **parametry połączenia zdalnego** listy rozwijanej w obszarze **SchoolContext** i wybierz parametry połączenia dla bazy danych został utworzony.
8. Wybierz **Aktualizuj bazę danych**.

    ![Karta Ustawienia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    To ustawienie powoduje, że proces wdrażania automatycznie skonfigurować aplikację *Web.config* plików na serwerze docelowym, aby używał Code First `MigrateDatabaseToLatestVersion` klasy inicjatora.
9. Kliknij przycisk **Dalej**.
10. W **Podgląd** kliknij pozycję **Uruchom Podgląd**.
  
    ![Przycisk StartPreview w karcie podglądu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Karta zawiera listę plików, które zostaną skopiowane na serwer. Wyświetlanie korzystania z wersji zapoznawczej nie jest wymagane, aby opublikować aplikację, ale jest przydatna funkcja wiedzieć. W takim przypadku nie trzeba podejmować żadnych działań, z listy plików, który jest wyświetlany. Przy następnym wdrożeniem tej aplikacji będzie tylko te pliki, które zostały zmienione na tej liście.
    ![Dane wyjściowe pliku StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Kliknij przycisk **publikowania**.
    Program Visual Studio rozpoczyna proces kopiowania plików na serwer platformy Azure.
12. **Dane wyjściowe** okno pokazuje, jakie akcje wdrażania zostały wykonane i raporty pomyślne ukończenie wdrożenia.
  
    ![Okno danych wyjściowych potwierdzającym pomyślne ukończenie wdrożenia](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Po pomyślnym wdrożeniu przeglądarka domyślna automatycznie otwiera adres URL witryny sieci web, wdrożone.
    Teraz działa utworzonej aplikacji w chmurze. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

W tym momencie usługi *SchoolContext* bazy danych została utworzona w usłudze Azure SQL Database, ponieważ wybrano **wykonaj migracje Code First (uruchomieniu aplikacji)**. *Web.config* plik w witrynie sieci web wdrożonej został zmieniony tak, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjator jest uruchamiany po raz pierwszy kod odczytuje lub zapisuje dane w bazie danych (co się stało Po wybraniu **studentów** kartę):

![](https://asp.net/media/4367421/mig.png)

Proces wdrażania jest tworzona w nowe parametry połączenia *(SchoolContext\_DatabasePublish*) dla migracje Code First na potrzeby Aktualizowanie schematu bazy danych i wstępne wypełnianie bazy danych.

![Parametry połączenia Database_Publish](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Można znaleźć wdrożonej wersji pliku Web.config na komputerze w *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dostęp do wdrożonych *Web.config* samego pliku przy użyciu protokołu FTP. Aby uzyskać instrukcje, zobacz [wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Postępuj zgodnie z instrukcjami, które zaczyna się "Aby użyć narzędzia FTP, potrzebne są trzy rzeczy: adres URL protokołu FTP, nazwę użytkownika i hasło."

> [!NOTE]
> Aplikacja sieci web nie implementuje zabezpieczeń, dzięki czemu każda osoba, która znajdzie adres URL można zmieniać dane. Aby uzyskać instrukcje na temat sposobu zabezpieczenie witryny sieci web, zobacz [wdrażanie bezpiecznej aplikacji ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Możesz uniemożliwić innym osobom za pomocą witryny przy użyciu portalu zarządzania systemu Azure lub **Eksploratora serwera** w programie Visual Studio, aby zatrzymać witrynę.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Migracje zaawansowane scenariusze

Jeśli wdrażanie bazy danych, uruchamiając migracji automatycznie, jak pokazano w tym samouczku wdrażasz witryny sieci web, która działa na wielu serwerach, można pobrać wiele serwerów próby Uruchom migracje, w tym samym czasie. Migracje są niepodzielne, więc jeśli dwa serwery podjęta próba uruchomienia tej samej migracji, jeden zakończy się powodzeniem i innych zakończy się niepowodzeniem (przy założeniu, że operacje nie można wykonać dwa razy). W tym scenariuszu jeśli chcesz uniknąć tych problemów, można ręcznie wywołać migracje i skonfigurować własny kod, aby go odbywa się tylko raz. Aby uzyskać więcej informacji, zobacz [działa i skryptów migracji z kodu](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) na blogu Rowan Miller i [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (w przypadku wykonywania migracji z wiersza polecenia) w witrynie MSDN.

Aby uzyskać informacji dotyczących innych scenariuszy migracji, zobacz [serii zrzut ekranu migracje](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Inicjatory pierwszy kodu

W sekcji Wdrażanie dostrzegła [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicjatora używane. Kod najpierw zapewnia innych inicjatorów, w tym [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (ustawienie domyślne), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (które było wcześniej używane) i [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicjatora może być przydatne w przypadku konfigurowania warunki dla testów jednostkowych. Można także napisać własny inicjatory i można wywołać inicjatora jawnie, jeśli nie chcesz czekać aż aplikacja odczytuje lub zapisuje w bazie danych. Od momentu, w tym samouczku jest zapisywana w listopadzie 2013 r., można używać tylko tworzenie i DropCreate inicjatory przed włączeniem migracji. Zespół platformy Entity Framework pracuje się nad tymi inicjatory można używać z także migracje.

Aby uzyskać więcej informacji na temat inicjatory, zobacz [opis inicjatory bazy danych w Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) i rozdział 6 książki [programowania programu Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) przez Julie Lerman i Rowan Miller.

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazaliśmy już, jak włączanie funkcji migracje i wdrażania aplikacji. W następnym samouczku rozpocznie się spojrzenie na bardziej zaawansowanych tematów, rozwijając modelu danych.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Poprzednie](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [dalej](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
