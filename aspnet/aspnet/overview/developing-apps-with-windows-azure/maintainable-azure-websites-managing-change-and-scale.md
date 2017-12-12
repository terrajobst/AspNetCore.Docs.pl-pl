---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "Przekazuje w laboratorium: łatwy w obsłudze witryn sieci Web Azure: Zarządzanie zmianami i skali | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Microsoft Azure ułatwia do tworzenia i wdrażania witryn sieci Web w środowisku produkcyjnym. Ale nie wszystko gotowe, gdy aplikacja jest na żywo, można dopiero rozpoczynasz pracę! Możesz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 1d6d9265d93fbd32e2d9c22e2ac3db9b5ffd9776
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Przekazuje w laboratorium: łatwy w obsłudze witryn sieci Web Azure: Zarządzanie zmianami i skali
====================
przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](http://aka.ms/webcamps-training-kit)

> Microsoft Azure ułatwia do tworzenia i wdrażania witryn sieci Web w środowisku produkcyjnym. Ale nie wszystko gotowe, gdy aplikacja jest na żywo, można dopiero rozpoczynasz pracę! Konieczne będzie obsługiwać zmieniających się wymagań, aktualizacje bazy danych, skalę i inne. Na szczęście usługa Azure App Service ma możesz objętych, wiele funkcji, aby zabezpieczać sprawne lokacje.
> 
> System Azure oferuje bezpieczne i elastyczne programowanie, wdrożenia i opcje dla dowolnej aplikacji sieci web rozmiar skalowania. Korzystaj z istniejących narzędzi do tworzenia i wdrażania aplikacji bez konieczności używania wielu zarządzania infrastrukturą.
> 
> Udostępnij aplikacji sieci web w środowisku produkcyjnym samodzielnie w minutach łatwe wdrażanie zawartości utworzone za pomocą narzędzia do rozwoju ulubionych. Można wdrożyć istniejącą witrynę bezpośrednio z kontroli źródła z obsługą **Git**, **GitHub**, **Bitbucket**, **TFS**, a nawet  **DropBox**. Wdrażanie bezpośrednio z Twoje ulubione IDE lub skrypty przy użyciu **PowerShell** w systemie Windows lub **CLI** narzędzia uruchomiony na systemie operacyjnym. Po wdrożeniu aktualizowanie lokacji stale z obsługą ciągłego wdrażania.
> 
> Platforma Azure udostępnia magazynu w chmurze skalowalne, trwałe, tworzenia kopii zapasowej i odzyskiwania rozwiązania dla dowolnych danych duże lub małe. W przypadku wdrażania aplikacji dla środowiska produkcyjnego, usług magazynu, takich jak tabele, obiekty BLOB i bazy danych SQL, ułatwić skalowanie aplikacji w chmurze.
> 
> W przypadku baz danych SQL ważne jest zapewnić aktualność wydajność bazy danych w przypadku wdrażania nowych wersji aplikacji. Podziękowania dla **migracje Code First Framework jednostki**, projektowania i wdrażania modelu danych uproszczoną tak, aby zaktualizować środowiska w minutach. To laboratorium praktycznego Pokaż różne tematy, które może wystąpić podczas wdrażania aplikacji sieci web w środowisku produkcyjnym na platformie Microsoft Azure.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).
> 
> Aby uzyskać więcej szczegółowe informacje dotyczące tego tematu, zobacz [budynku praktyczne aplikacje w chmurze z Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Włącz Entity Framework migracji z istniejącego modelu
- Aktualizuj model obiektów i odpowiednio przy użyciu Entity Framework migracji bazy danych
- Wdrażanie w usłudze Azure App Service przy użyciu narzędzia Git
- Przywrócenie poprzedniego wdrożenia za pomocą portalu zarządzania Azure
- Użyj usługi Azure Storage można skalować aplikacji sieci web
- Skonfiguruj automatyczne skalowanie aplikacji sieci web za pomocą portalu zarządzania Azure
- Tworzenie i konfigurowanie projektu testu obciążenia w programie Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej
- [Zestaw Azure SDK dla platformy .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [System kontroli wersji GIT](http://git-scm.com/download)
- Subskrypcja Microsoft Azure 

    - Zaloguj się do [bezpłatnej wersji próbnej](http://aka.ms/watk-freetrial)
    - Jeśli program Visual Studio Professional, Test Professional, Premium lub Ultimate z subskrybenta MSDN lub platformy MSDN, aktywacji programu [korzyści MSDN](http://aka.ms/watk-msdn) teraz, aby rozpocząć tworzenia i testowania na platformie Azure
    - [BizSpark](http://aka.ms/watk-bizspark) członków automatycznie otrzymują Azure korzyści za pośrednictwem ich programu Visual Studio Ultimate z subskrypcją MSDN
    - Elementy członkowskie [sieci Microsoft Partner Network](http://aka.ms/watk-mpn) program Cloud Essentials odbiera Azure kredyty miesięczne bez dodatkowych opłat

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.

1. Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.
3. Jeśli jest wyświetlane okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.

> [!NOTE]
> Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Korzystania z wstawek kodu

W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu. Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania. W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Korzystania z programu Entity Framework migracji](#Exercise1)
2. [Wdrażanie aplikacji sieci Web do etapu przemieszczania](#Exercise2)
3. [Wykonywanie wycofywania wdrożenia w środowisku produkcyjnym](#Exercise3)
4. [Odbierającej za pomocą usługi Azure Storage](#Exercise4)
5. [Przy użyciu automatycznego skalowania dla aplikacji sieci Web](#Exercise5) (opcjonalne dla programu Visual Studio 2013 Ultimate edition)

Szacowany czas trwania tego laboratorium: **75 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji. Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Ćwiczenie 1: Użycie Entity Framework migracji

Gdy tworzysz aplikację modelu danych może zmieniać wraz z upływem czasu. Te zmiany mogą wpłynąć na istniejącego modelu w bazie danych (Jeśli tworzysz nową wersję) i ważne jest, aby zachować aktualne w celu uniknięcia błędów bazy danych.

Aby uprościć śledzenia tych zmian w modelu, **migracje Code First Framework jednostki** automatycznie wykrywa zmian porównanie modelu za pomocą schematu bazy danych i generuje kod określonych w celu aktualizacji bazy danych, Tworzenie nowego *wersji* bazy danych.

Tego ćwiczenia pokazuje, jak włączyć **migracje** dla aplikacji i jak można łatwo wykryć i generowanie zmian do zaktualizowania baz danych.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Zadanie 1 — włączenie migracji

W tym zadaniu przechodzą przez kolejne kroki umożliwiające **migracje Code First Framework jednostki** do **Geek quizu** bazy danych, zmiana modelu i zrozumienie, jak te zmiany zostaną odzwierciedlone w Baza danych.

1. Otwórz program Visual Studio i Otwórz **GeekQuiz.sln** plik rozwiązania z **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Skompiluj rozwiązanie, aby pobrać i zainstalować **NuGet** zależności pakietów. Aby to zrobić, kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Kompiluj rozwiązanie** lub naciśnij klawisz **Ctrl + Shift + B**.
3. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.
4. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**. Zostanie utworzona początkowa migracji na podstawie istniejącego modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Włączanie migracje](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "włączenie migracji")

    *Włączanie migracji*

    > [!NOTE]
    > To polecenie dodaje **migracje** folder do quizu Geek projektu, który zawiera plik o nazwie **Configuration.cs**. **Konfiguracji** klasy można skonfigurować zachowanie migracji dla kontekstu.
5. Przy migracji włączone, musisz zaktualizować **konfiguracji** klasę, aby wypełnić bazę danych pierwotnych danych który **Geek quizu** wymaga. W obszarze **migracje**, Zastąp **Configuration.cs** plik z elementem zlokalizowany w **Source\Assets** folder tego laboratorium.

    > [!NOTE]
    > Ponieważ **migracje** wywoła **inicjatora** metody z każdej aktualizacji bazy danych, należy się upewnić, że rekordy nie są duplikowane w bazie danych. **AddOrUpdate** metody pomoże uniknąć zduplikowanych danych.
6. Aby dodać początkowej migracji, wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**.

    > [!NOTE]
    > Upewnij się, że nie jest brak baza danych o nazwie &quot;GeekQuizProd&quot; w wystąpieniu bazy danych LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Dodawanie migracji schematu podstawowego](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Dodawanie schematu podstawowego migracji")

    *Dodawanie schematu podstawowego migracji*

    > [!NOTE]
    > **Dodaj migracji** będzie szkieletu następnej migracji, w oparciu o zmiany zostały wprowadzone do modelu, od chwili utworzenia ostatniej migracji. W takim przypadku po wykonaniu pierwszej migracji projektu, spowoduje to dodanie skrypty w celu utworzenia wszystkie tabele określone w **TriviaContext** klasy.
7. Wykonanie migracji do aktualizacji bazy danych, uruchamiając następujące polecenie. To polecenie będzie określać **pełne** flagę, aby wyświetlić instrukcje SQL stosowane do docelowej bazy danych.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Tworzenie początkowej bazy danych](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "tworzenie początkowej bazy danych")

    *Tworzenie początkowej bazy danych*

    > [!NOTE]
    > **Update-Database** Zastosuj wszelkie oczekujące migracje do bazy danych. W takim przypadku utworzy bazę danych przy użyciu parametrów połączenia określonych w pliku web.config.
8. Przejdź do **widoku** i otwórz menu **Eksplorator obiektów SQL Server**.

    ![Otwórz w Eksploratorze obiektów SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Otwórz w Eksploratorze obiektów SQL Server")

    *Otwórz w Eksploratorze obiektów SQL Server*
9. W **Eksplorator obiektów SQL Server** okna, nawiązać połączenia z wystąpieniem LocalDB przez kliknięcie prawym przyciskiem myszy **programu SQL Server** węzła i wybierając **dodawania serwera SQL...**  opcji.

    ![Dodawanie wystąpienia programu SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "dodanie wystąpienia programu SQL Server")

    *Dodanie wystąpienia programu SQL Server do Eksplorator obiektów SQL Server*
10. Ustaw **nazwy serwera** do *(localdb) \v11.0* i pozostawić **uwierzytelniania systemu Windows** jako tryb uwierzytelniania. Kliknij przycisk **Connect** aby kontynuować.

    ![Łączenie z LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "połączenie do LocalDB")

    *Łączenie do LocalDB*
11. Otwórz **GeekQuizProd** bazy danych, a następnie rozwiń węzeł **tabel** węzła. Jak widać, **Update-Database** polecenia wygenerowany wszystkie tabele określone w **TriviaContext** klasy. Zlokalizuj **dbo. TriviaQuestions** tabeli, a następnie otwórz węzeł kolumny. W następnym zadaniem będzie Dodaj nową kolumnę do tej tabeli i Aktualizuj używając bazy danych **migracje**.

    ![Elementy towarzyszące składni pytania kolumn](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "elementy towarzyszące składni pytania kolumn")

    *Elementy towarzyszące składni pytania kolumn*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Zadanie 2 — aktualizacja schematu bazy danych za pomocą migracji

W tym zadaniu użyjesz **migracje Code First Framework jednostki** wykrywa zmian w modelu i generowania kodu niezbędne do aktualizacji bazy danych. Spowoduje zaktualizowanie **TriviaQuestions** jednostki przez dodanie nowych właściwości do niego. Następnie należy uruchomić poleceń, aby utworzyć nowy migrację, aby uwzględnić nowe kolumny w tabeli.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **TriviaQuestion.cs** znajdujących się w pliku **modele** folderu.
2. Dodaj nową właściwość o nazwie **wskazówka**, jak pokazano w poniższy fragment kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**. Nowych migracji zostanie utworzony w czasie wykonywania odbicia zmiany w modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Dodaj migracji](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "dodać migracji")

    *Dodaj migracji*

    > [!NOTE]
    > Plik migracji składa się z dwóch metod **się** i **dół**.
    > 
    > - **Się** metoda będzie używana do określenia, jakie zmiany bieżącej wersji naszych konieczność stosowania do bazy danych aplikacji.
    > - **Dół** służy do wycofania zmian dodano do **się** metody.
    > 
    > Podczas migracji bazy danych aktualizuje bazę danych, zostanie ona uruchomiona wszystkich migracji w kolejności znaczników czasu i tylko te, które nie zostały użyte od ostatniej aktualizacji ( \_tabeli MigrationHistory przechowuje informacje o migracji, które zostały zastosowane). **Się** metoda wszystkich migracji zostanie wywołana i dokona zmian została określona w bazie danych. Jeśli firma zdecyduje się wróć do poprzedniej migracji, **dół** metoda zostanie wywołana w celu ponownie dokonać zmian w odwrotnej kolejności.
4. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Dane wyjściowe **Update-Database** generowane polecenia **instrukcji Alter Table** instrukcję SQL, aby dodać nową kolumnę **TriviaQuestions** tabeli, jak pokazano na poniższej ilustracji.

    ![Dodaj instrukcję SQL kolumny wygenerowany](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Dodaj instrukcję SQL kolumny wygenerowany")

    *Dodaj instrukcję SQL kolumny wygenerowany*
6. W **Eksplorator obiektów SQL Server**, Odśwież **dbo. TriviaQuestions** tabeli i sprawdź, czy nowy **wskazówka** kolumna jest wyświetlana.

    ![Wyświetlanie nowej kolumny, która wskazówka](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "przedstawiający nową kolumnę wskazówki")

    *Wyświetlanie w nowej kolumnie wskazówki*
7. W **TriviaQuestion.cs** edytora, Dodaj **StringLength** ograniczenie do *wskazówka* właściwości, jak pokazano w poniższy fragment kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Dane wyjściowe **Update-Database** generowane polecenia **instrukcji Alter Table** instrukcję SQL, aby zaktualizować *wskazówka* typ kolumny **TriviaQuestions** tabeli, jak pokazano na poniższej ilustracji.

    ![Instrukcja SQL kolumny wygenerowany ALTER](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter wygenerowano instrukcji SQL kolumny")

    *Instrukcja SQL kolumny wygenerowany ALTER*
11. W **Eksplorator obiektów SQL Server**, Odśwież **dbo. TriviaQuestions** tabeli i sprawdź, czy **wskazówka** typem kolumny jest **nvarchar(150)**.

    ![Nowe ograniczenie przedstawiający](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "przedstawiający nowe ograniczenie")

    *Wyświetlanie nowe ograniczenie*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Ćwiczenie 2: Wdrażanie aplikacji sieci Web do etapu przemieszczania

**Web Apps w usłudze Azure App Service** umożliwia publikowanie przemieszczane. Publikowanie przemieszczanego tworzy przemieszczania miejsca lokacji dla każdej lokacji w środowisku produkcyjnym domyślne i umożliwia zamienić te miejsc bez przestoju. Jest to naprawdę przydatne zweryfikować zmiany przed zwolnieniem publicznie, przyrostowo integracji zawartości witryny i przywracania, jeśli zmiany nie działają zgodnie z oczekiwaniami.

W tym ćwiczeniu zostanie wdrożona **Geek quizu** aplikacji do środowiska pomostowego aplikacji sieci web przy użyciu kontroli wersji Git. Aby to zrobić, zostanie tworzenie aplikacji sieci web i obsługi administracyjnej wymaganych składników w portalu zarządzania, skonfiguruj **Git** repozytorium i wypychania aplikacji kodu z komputera lokalnego do tymczasowej miejsca źródłowego. Zostanie również zaktualizować produkcyjnej bazy danych za pomocą **migracje Code First** utworzonego w poprzednim ćwiczeniu. Następnie wykona aplikacji w środowisku testowym, to aby sprawdzić jej działanie. Po zakończeniu jego działa zgodnie z oczekiwaniami, propaguje aplikacji do środowiska produkcyjnego.

> [!NOTE]
> Aby umożliwić publikowanie przemieszczanego, aplikacji sieci web musi być w **Tryb standardowy**. Należy pamiętać, że dodatkowe opłaty zostaną naliczone w przypadku zmiany Tryb standardowy aplikacji sieci web. Aby uzyskać więcej informacji o cenach, zobacz [App Service — ceny](https://azure.microsoft.com/en-us/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Zadanie 1 — Tworzenie aplikacji sieci Web w usłudze aplikacji Azure

W ramach tego zadania spowoduje utworzenie aplikacji sieci web w **usłudze Azure App Service** z portalu zarządzania. Zostanie również skonfigurować **bazy danych SQL** do utrwalenia danych aplikacji i skonfigurować lokalne repozytorium Git do kontroli źródła.

1. Przejdź do [portalu zarządzania Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Microsoft skojarzonego z subskrypcją.

    ![Zaloguj się do portalu zarządzania platformy Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Zaloguj się do portalu zarządzania platformy Azure*
2. Kliknij przycisk **nowy** na pasku poleceń u dołu strony.

    ![Tworzenie nowej aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Tworzenie nowej aplikacji sieci web")

    *Tworzenie nowej aplikacji sieci web*
3. Kliknij przycisk **obliczeniowe**, **witryny sieci Web** , a następnie **Utwórz niestandardowy**.

    ![Tworzenie nowej aplikacji sieci web przy użyciu Utwórz niestandardowy](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Tworzenie nowej aplikacji sieci web przy użyciu Utwórz niestandardowy")

    *Tworzenie nowej aplikacji sieci web przy użyciu Utwórz niestandardowy*
4. W **nowej witryny sieci Web — Utwórz niestandardowy** okna dialogowego podaj dostępny **adres URL** (np. *kwizu geek*), wybierz lokalizację, w **Region** listy rozwijanej, a następnie wybierz **utworzyć nową bazę danych SQL** w **bazy danych** listy rozwijanej. Na koniec wybierz **publikowania z kontroli źródła** pole wyboru i kliknij przycisk **dalej**.

    ![Dostosowywanie nowej aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Dostosowywanie nowej aplikacji sieci web*
5. Określ następujące informacje dotyczące ustawień bazy danych:

    - W **nazwa** tekst Wprowadź nazwę bazy danych (np. *geekquiz\_db*)
    - Na serwerze **listy rozwijanej** listy, wybierz **serwera bazy danych SQL nowych**. Alternatywnie można wybrać istniejący serwer.
    - W **użytkownika bazy danych** i **hasła bazy danych** wprowadź nazwę administratora użytkownika i hasło dla serwera bazy danych SQL. Jeśli wybierzesz serwer zostały już utworzone, zostanie wyświetlony monit o hasło.

    ![Określanie ustawień bazy danych](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    *Określanie ustawień bazy danych*
6. Kliknij przycisk **Dalej** , aby kontynuować.
7. Wybierz **repozytorium Git lokalnego** do kontroli źródła, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Może zostać wyświetlony monit o poświadczenia wdrożenia (nazwa użytkownika i hasło).

    ![Tworzenie repozytorium Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Tworzenie repozytorium Git*
8. Zaczekaj, aż zostanie utworzona nowa aplikacja sieci web.

    > [!NOTE]
    > Domyślnie platforma Azure udostępnia domen w *azurewebsites.net* , ale także daje możliwość ustawić domenami niestandardowymi za pomocą portalu zarządzania Azure. Tylko może jednak zarządzać domen niestandardowych, jeśli używasz niektórych tryby usługi Azure App Service.
    > 
    > Usługa aplikacji Azure jest niedostępne w wersjach wolne, współużytkowane, podstawowa, standardowa i Premium. W trybie wolnym i udostępnione wszystkie aplikacje sieci web uruchamiany w środowisku wielodostępnym i ma przydziały na potrzeby użycia procesora CPU, pamięci i sieci. Maksymalna liczba bezpłatnych aplikacji może się różnić w planie. W trybie standardowym dostępne zasoby obliczeniowe aplikacje uruchomione na dedykowanych maszyn wirtualnych, które odpowiadają na standardowe platformy Azure. Można znaleźć konfiguracji trybu aplikacji sieci web w **skali** menu aplikacji sieci web.
    > 
    > ![Usługa aplikacji Azure tryby](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "tryby usługi aplikacji Azure")
    > 
    > Jeśli używasz **Shared** lub **standardowe** tryb, będzie można nimi zarządzać domen niestandardowych dla aplikacji sieci web, przechodząc do swojej aplikacji **Konfiguruj** menu i klikając **Zarządzanie domenami** w obszarze *nazwy domen*.
    > 
    > ![Zarządzanie domenami](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Zarządzanie domenami")
    > 
    > ![Zarządzanie domenami niestandardowymi](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Zarządzanie domenami niestandardowymi")
9. Po utworzeniu aplikacji sieci web, kliknij łącze w obszarze **adres URL** kolumny, aby sprawdzić, czy jest uruchomiona w nowej aplikacji sieci web.

    ![Przeglądanie nowej aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Przeglądanie nowej aplikacji sieci web*

    ![Aplikacja sieci Web uruchomiona](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *Aplikacja sieci Web uruchomiona*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Zadanie 2 — Tworzenie produkcyjną bazę danych SQL

W tym zadaniu użyjesz **migracje Code First Framework jednostki** można utworzyć wartości docelowej bazy danych **bazy danych SQL Azure** wystąpienia utworzone w poprzednim zadaniu.

1. W portalu zarządzania, przejdź do aplikacji sieci web utworzony w poprzednim zadaniu i przejdź do jego **pulpitu nawigacyjnego**.
2. W **pulpitu nawigacyjnego** kliknij przycisk **Wyświetl parametry połączenia** łącze w obszarze **szybkiego dostępu** sekcji.

    ![Wyświetl parametry połączenia](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Wyświetl parametry połączenia")

    *Wyświetl parametry połączenia*
3. Kopiuj **ciąg połączenia** wartości i zamknąć okno dialogowe.

    ![Parametry połączenia w portalu zarządzania Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "parametry połączenia w portalu zarządzania Azure")

    *Parametry połączenia w portalu zarządzania Azure*
4. Kliknij przycisk **baz danych SQL** lista baz danych na platformie Azure

    ![Baza danych SQL menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "menu bazy danych SQL")

    *Menu bazy danych SQL*
5. Zlokalizuj bazę danych, który został utworzony w poprzednim zadaniu, a następnie kliknij polecenie na serwerze.

    ![Baza danych SQL server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "serwera bazy danych SQL")

    *Serwer bazy danych SQL*
6. W **Szybki Start** strony serwera, kliknij **Konfiguruj**.

    ![Konfiguruj menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Konfiguruj menu")

    *Konfigurowanie menu*
7. W **adresy IP mogą** kliknij na **dodać do dozwolonych adresów IP** łącze, aby włączyć IP do łączenia się z serwerem bazy danych SQL.

    ![Dozwolone adresy IP](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "adresów IP dozwolone")

    *Dozwolonych adresów IP*
8. Kliknij przycisk **zapisać** w dolnej części strony, aby wykonać krok.
9. Przełącz się do programu Visual Studio.
10. W **Konsola Menedżera pakietów**, wykonaj następujące polecenie zastępowanie *[YOUR-połączenia-STRING]* symbol zastępczy parametrów połączenia skopiowane z platformy Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualizacja bazy danych przeznaczonych dla systemu Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aktualizacji bazy danych przeznaczonych dla systemu Windows Azure SQL Database")

    *Aktualizacja bazy danych przeznaczonych dla bazy danych SQL Azure*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Zadanie 3 — wdrażanie kwizu Geek do etapu przemieszczania przy użyciu narzędzia Git

To zadanie spowoduje włączenie publikowania przemieszczanego w aplikacji sieci web. Następnie użyje Git do publikowania aplikacji quizu Geek bezpośrednio z komputera lokalnego do środowiska pomostowego aplikacji sieci web.

1. Wróć do portalu i kliknij nazwę aplikacji sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron zarządzania aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Otwieranie stron zarządzania aplikacji sieci web*
2. Przejdź do **skali** strony. W obszarze **ogólne** zaznacz **standardowe** dla konfigurację i kliknij przycisk **zapisać** na pasku poleceń.

    > [!NOTE]
    > Aby uruchamiać wszystkie aplikacje sieci web w bieżącym obszarze i subskrypcji w **standardowe** tryb, pozostaw **Zaznacz wszystko** zaznaczone pole wyboru w **wybierz Lokacje** konfiguracji. W przeciwnym razie wyczyść **Zaznacz wszystko** pole wyboru.

    ![Uaktualnianie aplikacji sieci web, tryb standardowy](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "uaktualniania aplikacji sieci web, tryb standardowy")

    *Uaktualnianie aplikacji sieci Web, tryb standardowy*
3. Kliknij przycisk **tak** o potwierdzenie zmian.

    ![Potwierdzenie, Zmień tryb standardowy](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "kontynuowaniem zmianę trybu aplikacji sieci web")

    *Potwierdzenie, Zmień tryb standardowy*
4. Przejdź do **pulpitu nawigacyjnego** i kliknij przycisk **Włącz przemieszczane publikowanie** w obszarze **szybkiego dostępu** sekcji.

    ![Włączanie publikowania przemieszczanego](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Włączanie przemieszczane publikowania")

    *Włączanie publikowania przemieszczanego*
5. Kliknij przycisk **tak** aby umożliwić publikowanie przemieszczane.

    ![Potwierdzenie przemieszczane publikowanie](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "kliknięcie przycisku Tak, aby umożliwić publikowanie przemieszczanego")

    *Potwierdzenie przemieszczanego publikowania*
6. Na liście aplikacji sieci web rozwiń węzeł znak z lewej strony nazwę aplikacji sieci web do wyświetlenia miejsca przemieszczania lokacji. Ta kolumna ma nazwę aplikacji sieci web, a następnie ***(przemieszczania)***. Kliknij przycisk przemieszczania lokacji, aby przejść do strony zarządzania.

    ![Nawigowanie do tymczasowej aplikacjami sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "nawigowania do tymczasowej aplikacjami sieci web")

    *Nawigowanie do tymczasowej aplikacjami*
7. Należy zauważyć, że tej strony zarządzania he wygląda na stronę Zarządzanie żadnych innych aplikacji sieci web. Przejdź do **wdrożeń** strony i skopiuj **adres URL Git** wartość. Zostanie użyty później w tym ćwiczeniu.

    ![Kopiowanie wartość adresu URL Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopiowanie wartość adresu URL Git*
8. Otwórz nowe **Git Bash** konsoli i uruchom następujące polecenia. Aktualizacja *[YOUR-aplikacji-PATH]* symbolu zastępczego ze ścieżką do **GeekQuiz** rozwiązania, znajdujących się w **Source\Ex1 DeployingWebSiteToStaging\Begin** folderu w tym laboratorium.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicjowanie Git i pierwszego zatwierdzenia](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicjowanie Git i pierwszego zatwierdzenia*
9. Uruchom następujące polecenie, aby wypychać aplikacji sieci web do zdalnej **Git** repozytorium. Zastąp symbol zastępczy z adresem URL uzyskane z portalu zarządzania. Pojawi się monit o podanie hasła wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Wypychanie do systemu Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Wypychanie do platformy Azure*

    > [!NOTE]
    > Podczas wdrażania zawartości do hosta FTP lub repozytorium GIT aplikacji sieci web, należy uwierzytelnić przy użyciu **poświadczenia wdrażania** utworzony przy użyciu aplikacji sieci web **Szybki Start** lub **pulpitu nawigacyjnego**  strony zarządzania. Jeśli nie znasz poświadczenia wdrażania można łatwo można zresetować je za pomocą portalu zarządzania. Otwórz aplikację sieci web **pulpitu nawigacyjnego** i kliknij przycisk **resetowanie poświadczeń wdrażania** łącza. Wprowadź nowe hasło, a następnie kliknij przycisk **OK**. Wdrożenie poświadczenia są prawidłowe do użytku z wszystkich aplikacji sieci web skojarzonych z Twoją subskrypcją.
10. Aby można było zweryfikować aplikacji sieci web został pomyślnie przypisany do platformy Azure, wróć do portalu zarządzania, a następnie kliknij przycisk **witryn sieci Web**.
11. Znajdź aplikację sieci web, a następnie rozwiń pozycję, aby wyświetlić miejsce przemieszczania lokacji. Kliknij jego **nazwa** aby przejść do strony zarządzania.
12. Kliknij przycisk **wdrożeń** wyświetlić **historii wdrożenia**. Sprawdź, czy jest **aktywnych wdrożeń** z Twojej  *&quot;początkowej zatwierdzić&quot;*.

    ![Wdrażanie usługi Active](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Wdrażanie usługi Active*
13. Na koniec kliknij **Przeglądaj** na pasku poleceń, aby przejść do aplikacji sieci web.

    ![Przeglądanie aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Przeglądanie aplikacji sieci web*
14. Jeśli aplikacja zostanie pomyślnie wdrożona, zostanie wyświetlona strona logowania quizu Geek.

    > [!NOTE]
    > Adres URL wdrożonej aplikacji zawiera nazwę aplikacji sieci web, a następnie *-przemieszczania*.

    ![Aplikacja uruchomiona w środowisku przemieszczania](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplikacja uruchomiona w środowisku przemieszczania*
15. Jeśli chcesz zapoznać się z aplikacji, kliknij przycisk **zarejestrować** zarejestrować nowego użytkownika. Wszystkie szczegóły konta, wprowadzając nazwę użytkownika, adres e-mail i hasło. Następnie aplikacja zawiera pierwsze pytanie kwizu. Odpowiedz na kilka pytań, aby upewnić się, że działa zgodnie z oczekiwaniami.

    ![Aplikacja jest gotowa do użycia](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplikacja jest gotowa do użycia*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Zadanie 4 — podwyższania poziomu aplikacji sieci Web w środowisku produkcyjnym

Po upewnieniu się, że aplikacja sieci web działa poprawnie w środowisku przemieszczania, można przystąpić do Podwyższ go do produkcji. W tym zadaniu zostanie Zamień miejsce wystawiania lokacji z lokacji miejsca produkcji.

1. Wróć do portalu zarządzania i wybierz miejsce przemieszczania lokacji. Kliknij przycisk **wymiany** na pasku poleceń.

    ![Przechodź do środowiska produkcyjnego](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Przechodź do środowiska produkcyjnego*
2. Kliknij przycisk **tak** w oknie dialogowym potwierdzenia, aby kontynuować operację wymiany. Azure zostanie natychmiast wymiany zawartości witryny z zawartością przemieszczania lokacji.

    > [!NOTE]
    > Niektóre ustawienia z wersji przemieszczanego automatycznie zostanie skopiowany do wersji produkcyjnej (np. parametrów połączenia zastąpienia mapowania programu obsługi, itp.), ale inne ustawienia nie spowoduje zmiany (np. punkty końcowe DNS powiązania SSL, itp.).

    ![Potwierdzenie operacji wymiany](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Potwierdzenie operacji wymiany*
3. Po zakończeniu wymiany wybierz gniazda produkcyjnego i kliknij przycisk **Przeglądaj** na pasku poleceń, aby otworzyć miejsca produkcji. Zwróć uwagę, adres URL na pasku adresu.

    > [!NOTE]
    > Może być konieczne odświeżenie przeglądarkę, aby wyczyścić pamięć podręczną. W programie Internet Explorer, można to zrobić przez naciśnięcie przycisku **CTRL + R**.

    ![Aplikacja sieci Web w środowisku produkcyjnym](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. W **GitBash** konsoli, zaktualizuj zdalnego adresu URL dla lokalnego repozytorium Git do miejsca produkcji. Aby to zrobić, uruchom następujące polecenie, zastępując symbole zastępcze wdrożenia nazwy użytkownika i nazwę aplikacji sieci web.

    > [!NOTE]
    > W następujących ćwiczeniach będzie Wypchnij zmiany do miejsca produkcji zamiast przemieszczania tylko dla uproszczenia laboratorium. W przypadku rzeczywistych zalecane jest aby zweryfikować zmiany w środowisku przemieszczania przed podniesieniem do środowiska produkcyjnego.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Ćwiczenie 3: Wykonywanie wycofywania wdrożenia w środowisku produkcyjnym

Istnieją scenariusze, w którym nie ma miejsce przemieszczania do wykonania dynamicznej wymiany między przemieszczania i produkcji, na przykład jeśli pracujesz z **wolne** lub **Shared** tryb. W tych scenariuszach należy testować aplikację w środowisku testowym — lokalnie lub w lokacji zdalnej — przed wdrożeniem w środowisku produkcyjnym. Jednak jest możliwe, że problem nie został wykryty podczas fazy badania mogą pojawiać się w lokacji produkcyjnej. W takim przypadku należy mieć mechanizm można łatwo przełączyć się poprzedni i bardziej stabilną wersję aplikacji tak szybko jak to możliwe.

W **usłudze Azure App Service**, ciągłego wdrażania od kontroli źródła sprawia, że to możliwe dzięki **ponownie wdrożyć** akcji dostępnych w portalu zarządzania. Azure śledzi wdrożenia skojarzone z zatwierdzeń do repozytorium i zapewnia możliwość ponownego wdrażania aplikacji przy użyciu dowolnej z poprzednich wdrożeń, w dowolnym momencie.

W tym ćwiczeniu będzie wykonywać zmiana kodu w **quizu Geek** aplikacji, które celowo injects *usterki*. Zostanie wdrożona aplikacji do środowiska produkcyjnego, aby wyświetlić ten błąd, a następnie możesz będzie korzystać z funkcji ponownego wdrażania, aby powrócić do poprzedniego stanu.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Zadanie 1 — aktualizacja aplikacji kwizu Geek

W tym zadaniu zostanie Refaktoryzuj małego fragmentu kodu **TriviaController** klasę, aby wyodrębnić część logiki pobierający opcja wybranego testu z bazy danych do nowej metody.

1. Przełącz się do wystąpienia programu Visual Studio z **GeekQuiz** rozwiązania z poprzednim ćwiczeniu.
2. W **Eksploratora rozwiązań**, otwórz **TriviaController.cs** pliku wewnątrz **kontrolerów** folderu.
3. Zlokalizuj **StoreAsync** metodę i wybierz kod wyróżnione na poniższym rysunku.

    ![Wybranie kodu](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Wybranie kodu*
4. Kliknij prawym przyciskiem myszy zaznaczony kod, a następnie rozwiń **Refaktoryzuj** menu i wybierz **Wyodrębnij metodę...** .

    ![Wyodrębnianie kodu jako nowej metody](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Wybieranie metody wyodrębniania*
5. W **Wyodrębnij metodę** okno dialogowe, nazwę nowej metody *MatchesOption* i kliknij przycisk **OK**.

    ![Określanie nazwy — metoda](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Określanie nazwy dla wyodrębnionego — metoda*
6. Zaznaczony kod następnie jest wyodrębniany do **MatchesOption** metody. Poniższy fragment kodu pokazano wynikowy kod.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Zadanie 2 — ponownego wdrażania aplikacji kwizu Geek

Teraz zostanie Wypchnij zmiany wprowadzone w poprzednim zadaniu repozytorium, które wyzwoli nowe wdrożenie do środowiska produkcyjnego. Następnie zostaną rozwiązane problemy problem przy użyciu **narzędzi programistycznych F12** programu Internet Explorer, a następnie wykonać wycofanie do poprzedniego wdrożenia z portalu zarządzania Azure.

1. Otwórz nowe **Git Bash** konsoli do wdrażania zaktualizowaną aplikację w usłudze Azure App Service.
2. Wykonaj następujące polecenia, aby wypchnąć zmiany do platformy Azure. Aktualizacja *[YOUR-aplikacji-PATH]* symbolu zastępczego ze ścieżką do **GeekQuiz** rozwiązania. Pojawi się monit o podanie hasła wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Wypychanie refaktoryzowane kodu na platformie Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Wypychanie refaktoryzowane kodu na platformie Azure*
3. Otwórz program Internet Explorer i przejdź do aplikacji sieci web (np. `http://<your-web-site>.azurewebsites.net`). Zaloguj się za pomocą utworzonego wcześniej poświadczenia.
4. Naciśnij klawisz **F12** można uruchomić narzędzia deweloperskie, wybierz **sieci** i kliknij polecenie **odtwarzanie** przycisk, aby rozpocząć nagrywanie.

    ![Uruchomienie rejestrowania sieci](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "uruchomienie rejestrowania sieci")

    *Uruchomienie rejestrowania sieci*
5. Wybierz dowolną opcję kwizu. Zobaczysz, że nic się nie dzieje.
6. W **F12** okno, zawiera wpis odpowiadający na żądania POST HTTP protokołu HTTP **500** wynik.

    ![HTTP 500 Błąd](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 Błąd*
7. Wybierz **konsoli** kartę. Szczegółowe informacje o przyczynie jest rejestrowany błąd.

    ![Błąd w zarejestrowany](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Błąd w zarejestrowany*
8. Znajdź części szczegółów błędu. Wyraźnie widać ten błąd jest spowodowany przez kod refaktoryzacji zostanie zatwierdzone w poprzednich krokach.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Nie zamykaj przeglądarki.
10. W nowym wystąpieniu przeglądarki, przejdź do [portalu zarządzania Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Microsoft skojarzonego z subskrypcją.
11. Wybierz **witryn sieci Web** i kliknij aplikację sieci web utworzone w ćwiczenie 2.
12. Przejdź do **wdrożeń** strony. Należy zauważyć, że wszystkie zatwierdzenia wykonywane są wyświetlane w historii wdrożenia.

    ![Lista istniejących wdrożeń](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Lista istniejących wdrożeń*
13. Wybierz poprzednie zatwierdzenie, a następnie kliknij przycisk **ponownie wdrożyć** na pasku poleceń.

    ![Ponowne rozmieszczanie poprzednie zatwierdzenie](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Ponowne rozmieszczanie poprzednie zatwierdzenie*
14. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **tak**.

    ![Potwierdzenie ponownego wdrożenia komputera](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Po zakończeniu wdrożenia, przełączyć się do wystąpienia przeglądarki z aplikacji sieci web i naciśnij klawisz **CTRL + F5**.
16. Kliknij przycisk Opcje. Przerzuć animacji teraz będą miały miejsce i wynik (*Popraw nieprawidłowe*) będą wyświetlane.
17. (Opcjonalnie) Przełącz się do **Git Bash** konsoli i wykonaj następujące polecenia, aby powrócić do poprzedniej zatwierdzenia.

    > [!NOTE]
    > Te polecenia Utwórz nowe zatwierdzenie, które spowoduje cofnięcie wszystkich wprowadzonych w zatwierdzonym zły zmian w repozytorium Git. Azure następnie należy ponownie wdrożyć aplikację, używając nowego zatwierdzenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Ćwiczenie 4: Skalowanie przy użyciu usługi Azure Storage

**Obiekty BLOB** to najprostszy sposób przechowywania dużych ilości nieustrukturyzowanych tekstu lub danych binarnych, takich jak wideo, audio i obrazów. Przenoszenie zawartości statycznej aplikacji do magazynu, pomaga do skalowania aplikacji za pomocą dostarczenia obrazów i dokumentów bezpośrednio w przeglądarce.

W tym ćwiczeniu zostanie przesunięty w statycznej zawartości aplikacji do kontenera obiektów Blob. A następnie skonfiguruje aplikacji w taki sposób, aby dodać **reguły ponowne zapisywanie adresów ASP.NET URL** w **Web.config** przekierować zawartości do kontenera obiektów Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Zadanie 1 — Tworzenie konta magazynu platformy Azure

W tym zadaniu dowiesz się, jak utworzyć nowe konto magazynu przy użyciu portalu zarządzania.

1. Przejdź do [portalu zarządzania Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Microsoft skojarzonego z subskrypcją.
2. Wybierz **nowych | Usługi danych | Magazyn | Szybkie tworzenie** aby rozpocząć tworzenie nowego konta magazynu. Wprowadź unikatową nazwę konta i wybierz **Region** z listy. Kliknij przycisk **Utwórz konto magazynu** aby kontynuować.

    ![Tworzenie nowego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Tworzenie nowego konta magazynu")

    *Tworzenie nowego konta magazynu*
3. W **magazynu** sekcji, poczekaj, aż zmiany stanu nowe konto magazynu na *Online* aby można było wykonać następujący krok.

    ![Utworzono konto magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "utworzono konto magazynu")

    *Utworzono konto magazynu*
4. Kliknij nazwę konta magazynu, a następnie kliknij przycisk **pulpitu nawigacyjnego** łącze w górnej części strony. **Pulpitu nawigacyjnego** strona zawiera informacje o stanie konta i punktów końcowych usługi, które mogą być używane w aplikacjach.

    ![Wyświetlanie pulpitu nawigacyjnego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "wyświetlanie pulpitu nawigacyjnego konta magazynu")

    *Wyświetlanie pulpitu nawigacyjnego konta magazynu*
5. Kliknij przycisk **Zarządzaj kluczami dostępu** na pasku nawigacji.

    ![Zarządzaj kluczami dostępu przycisk](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "przycisku Zarządzaj kluczami dostępu")

    *Zarządzanie przycisk klawisze dostępu*
6. W **Zarządzaj kluczami dostępu** okno dialogowe, kopiowania **nazwy konta magazynu** i **podstawowy klucz dostępu** jako będą potrzebne w poniższym ćwiczeniu. Zamknij okno dialogowe.

    ![Okno dialogowe klucz dostępu Zarządzaj](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "okno dialogowe Zarządzanie klucza dostępu")

    *Klucz dostępu, okno dialogowe Zarządzanie*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Zadanie 2 — przekazywania zasobów do magazynu obiektów Blob platformy Azure

W tym zadaniu zostanie okno Eksploratora serwera z programu Visual Studio do łączenia się z kontem magazynu. Następnie utworzysz kontenera obiektów blob i przekazać plik z logo quizu Geek do kontenera.

1. Przełącz się do wystąpienia programu Visual Studio z **GeekQuiz** rozwiązania z poprzednim ćwiczeniu.
2. Na pasku menu wybierz **widoku** , a następnie kliknij przycisk **Eksploratora serwera**.
3. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **Azure** a następnie wybierz węzeł **Connect Azure...** . Zaloguj się przy użyciu konta Microsoft skojarzonego z subskrypcją.

    ![Z usługą Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Połączenia z platformą Azure*
4. Rozwiń węzeł **Azure** węzła, kliknij prawym przyciskiem myszy **magazynu** i wybierz **Dołącz zewnętrzną usługę Storage...** .
5. W **Dodaj nowe konto magazynu** okna dialogowego wprowadź **nazwa konta** i **klucz konta** uzyskanych w poprzednim zadaniu i kliknij przycisk **OK**.

    ![Dodaj nowe konto magazynu — okno dialogowe](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Dodaj nowe konto magazynu — okno dialogowe*
6. Konta magazynu powinien zostać wyświetlony w obszarze **magazynu** węzła. Rozwiń konta magazynu, kliknij prawym przyciskiem myszy **obiekty BLOB** i wybierz **Tworzenie kontenera obiektów Blob...** .

    ![Tworzenie kontenera obiektów Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Tworzenie kontenera obiektów Blob")

    *Tworzenie kontenera obiektów Blob*
7. W **Tworzenie kontenera obiektów Blob** okna dialogowego wprowadź nazwę dla blobcontainer a kliknij **OK**.

    ![Okno dialogowe Tworzenie kontenera obiektów Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "okno dialogowe Tworzenie kontenera obiektów Blob")

    *Tworzenie kontenera obiektów Blob, okno dialogowe*
8. Nowy kontener obiektów blob, powinny zostać dodane do **obiekty BLOB** węzła. Zmień uprawnienia dostępu w kontenerze do publicznego kontenera. Aby to zrobić, kliknij prawym przyciskiem myszy **obrazów** kontenera i wybierz **właściwości**.

    ![obrazy właściwości kontenera](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "obrazy właściwości kontenera")

    *Obrazy właściwości kontenera*
9. W **właściwości** ustaw **publiczny dostęp do odczytu** do **kontenera**.

    ![Zmiana właściwości publiczny dostęp do odczytu](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "zmiana właściwości publiczny dostęp do odczytu")

    *Zmiana właściwości publiczny dostęp do odczytu*
10. Po wyświetleniu monitu, gdy chcesz zmienić właściwości dostępu publicznego, kliknij przycisk **tak**.

    ![Ostrzeżenie programu Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "ostrzeżenie programu Microsoft Visual Studio")

    *Ostrzeżenie programu Microsoft Visual Studio*
11. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **obrazów** kontenera obiektów blob i wybierz **kontenera obiektów Blob widoku**.

    ![Wyświetl kontenera obiektów Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "wyświetlić kontenera obiektów Blob")

    *Widok kontenera obiektów Blob*
12. Kontener obrazów należy otworzyć nowe okno i legenda o żadnych wpisów, które mają być wyświetlane. Kliknij przycisk **przekazać** ikonę, aby przekazać plik do kontenera obiektów blob.

    ![Kontener obrazów z żadnych wpisów](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "kontener obrazów z żadnych wpisów")

    *Kontener obrazów z żadnych wpisów*
13. W **przekazania obiektu Blob** oknie dialogowym wybierz opcję **zasoby** folderu laboratorium. Wybierz **logo big.png** plik i kliknij przycisk **Otwórz**.
14. Zaczekaj, aż przekazać pliku. Po zakończeniu przekazywania, plik powinien być wyświetlany w kontenerze obrazów. Kliknij prawym przyciskiem myszy wpis pliku, a następnie wybierz **URL kopii**.

    ![Skopiuj adres URL obiektu blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "skopiuj adres URL pliku obiektu blob")

    *Skopiuj adres URL obiektu blob*
15. Otwórz program Internet Explorer i wklej adres URL. Poniższy obraz powinien być wyświetlany w przeglądarce.

    ![Obraz logo big.png z magazynu obiektów Blob systemu Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo big.png obrazu z magazynu")

    *Obraz logo big.png z magazynu obiektów Blob platformy Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Zadanie 3 — Aktualizowanie rozwiązania do pracy z zawartość statyczną z magazynu obiektów Blob platformy Azure

W tym zadaniu zostanie skonfigurowana **GeekQuiz** rozwiązanie, aby korzystać z obrazu przekazany do magazynu obiektów Blob Azure (zamiast obrazu, znajdującego się w aplikacji sieci web), dodając reguły ponowne zapisywanie adresów URL platformy ASP.NET w **web.config**pliku.

1. W programie Visual Studio Otwórz **Web.config** pliku wewnątrz **GeekQuiz** projektu i Znajdź  **&lt;system.webServer&gt;**  elementu.
2. Dodaj następujący kod, aby dodać ponowne zapisywanie adresów URL reguły, aktualizowanie symbol zastępczy nazwą konta magazynu.

    (Fragment - kodu *UrlRewriteRule WebSitesInProduction - Ex4 -*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Ponowne zapisywanie adresów URL jest proces przechwytywaniu przychodzącego żądania sieci Web i Przekierowanie żądania do innego zasobu. Adres URL ponowne zapisywanie reguł informuje przebudowywania aparatu, gdy żądanie musi zostać przekierowane i którym powinny one zostać przekierowane. Reguła przebudowywania składa się z dwóch ciągów: wzorzec do wyszukania w żądanym adresie URL (zazwyczaj za pomocą wyrażeń regularnych), i Zastąp wzorzec, jeśli ciąg. Aby uzyskać więcej informacji, zobacz [w programie ASP.NET ponowne zapisywanie adresów URL](https://msdn.microsoft.com/en-us/library/ms972974.aspx).
3. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
4. Otwórz nowe **Git Bash** konsoli do wdrażania zaktualizowaną aplikację w usłudze Azure App Service.
5. Wykonaj następujące polecenia, aby wypchnąć zmiany do platformy Azure. Aktualizacja *[YOUR-aplikacji-PATH]* symbolu zastępczego ze ścieżką do **GeekQuiz** rozwiązania. Pojawi się monit o podanie hasła wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Wdrażanie aktualizacji na platformie Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Wdrażanie aktualizacji na platformie Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Zadanie 4 — weryfikacja

W tym zadaniu użyjesz **programu Internet Explorer** do przeglądania **quizu Geek** aplikacji i sprawdź, czy adres URL edycji reguły dla działania obrazów i są przekierowywane do obrazu hostowanych na **obiektów Blob platformy Azure Magazyn**.

1. Otwórz program Internet Explorer i przejdź do aplikacji sieci web (np. `http://<your-web-site>.azurewebsites.net`). Zaloguj się za pomocą utworzonego wcześniej poświadczenia.

    ![Wyświetlanie aplikacji sieci web quizu Geek z obrazem](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "przedstawiający aplikacji sieci web quizu Geek z obrazem")

    *Wyświetlanie aplikacji sieci web quizu Geek z obrazem*
2. Naciśnij klawisz **F12** można uruchomić narzędzia deweloperskie, wybierz **sieci** karcie i rozpocząć nagrywanie.

    ![Uruchomienie rejestrowania sieci](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "uruchomienie rejestrowania sieci")

    *Uruchomienie rejestrowania sieci*
3. Naciśnij klawisz **CTRL + F5** odświeżenie strony sieci web.
4. Po zakończeniu ładowania strony powinny pojawić się żądanie HTTP dla **/img/logo-big.png** adres URL protokołu HTTP z **301** wynik (przekierowanie) i inne żądanie `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adresu URL HTTP **200** wynik.

    ![Weryfikowanie adresu URL przekierowania](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "przedstawiający przekierowania w narzędzia dla deweloperów")

    *Weryfikowanie adresu URL przekierowania*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Ćwiczenie 5: Przy użyciu automatycznego skalowania dla aplikacji sieci Web

> [!NOTE]
> Jest to opcjonalne, ponieważ wymaga obsługi obciążenia sieci Web &amp; test wydajności, która jest dostępna tylko dla **Visual Studio 2013 Ultimate Edition**. Aby uzyskać więcej informacji na temat określonych funkcji programu Visual Studio 2013 Porównaj wersje [tutaj](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** udostępnia funkcję automatycznego skalowania dla aplikacji sieci web działających **Tryb standardowy**. Funkcja automatycznego skalowania umożliwia automatyczne skalowanie liczby wystąpień aplikacji sieci web, w zależności od obciążenia Azure. Włączenie automatycznego skalowania Azure sprawdza Procesora aplikacji sieci web co pięć minut i dodaje wystąpień, zgodnie z potrzebami w danym momencie. Jeśli wykorzystanie Procesora jest niskie, Azure spowoduje usunięcie wystąpienia co dwie godziny aby upewnić się, że nie ma obniżoną wydajność aplikacji sieci web.

W tym ćwiczeniu przechodzą przez kolejne kroki wymagane do skonfigurowania **skalowania automatycznego** funkcji **Geek quizu** aplikacji sieci web. Ta funkcja sprawdzi przez uruchomienie testu obciążenia programu Visual Studio, aby wygenerować obciążenie za mało procesora CPU w aplikacji w celu wyzwolenia aktualizacji wystąpienia.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Zadanie 1 — Konfigurowanie skalowania automatycznego oparciu metryki Procesora

W tym zadaniu użyjesz portalu zarządzania Azure można włączyć funkcję automatycznego skalowania dla aplikacji sieci web utworzonej w ćwiczenie 2.

1. W [portalu zarządzania Azure](https://manage.windowsazure.com/), wybierz pozycję **witryn sieci Web** i kliknij aplikację sieci web utworzone w ćwiczenie 2.
2. Przejdź do **skali** strony. W obszarze **pojemności** zaznacz **Procesora** dla **skali według metryki** konfiguracji.

    > [!NOTE]
    > Podczas skalowania przez procesor CPU, Azure dynamicznie dostosowuje liczbę wystąpień używane przez aplikację w przypadku zmiany użycia procesora CPU.

    ![Wybranie opcji skalowania przez procesor CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "wybranie Procesora metryki automatycznego skalowania")

    *Wybranie opcji skalowania przez procesor CPU*
3. Zmień **Procesora docelowej** konfigurację, aby **20**-**40** procent.

    > [!NOTE]
    > Ten zakres reprezentuje średniego użycia procesora CPU dla aplikacji sieci web. Azure spowoduje dodanie lub usunięcie wystąpień do zachowania aplikacji sieci web w tym zakresie. Określono minimalną i maksymalną liczbę wystąpień używanych na potrzeby skalowania w **liczba wystąpień** konfiguracji. Azure nigdy nie przejdzie powyżej lub przekroczenie tego limitu.
    > 
    > Wartość domyślna **Procesora docelowej** wartości są modyfikowane tylko do celów tego laboratorium. Konfigurując zakres Procesora wartościami małych, są zwiększając szanse do wyzwalania automatycznego skalowania, jeśli średniego obciążenia znajduje się w aplikacji.

    ![Zmiana docelowy może wynosić 20-40 procent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "zmiana docelowy Procesora należeć do przedziału od 20 i 40 procent")

    *Zmiana Procesora docelowy należeć do przedziału od 20 i 40 procent*
4. Kliknij przycisk **zapisać** na pasku poleceń, aby zapisać zmiany.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Zadanie 2 — obciążenia testowanie za pomocą programu Visual Studio

Teraz, gdy **skalowania automatycznego** została skonfigurowana, zostanie utworzona **wydajności sieci Web i załadować projekt testowy** w programie Visual Studio, aby wygenerować niektórych obciążenie procesora CPU w aplikacji sieci web.

1. Otwórz **programu Visual Studio Ultimate 2013** i wybierz **pliku | Nowy | Projekt...**  można uruchomić nowego rozwiązania.

    ![Tworzenie nowego projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "tworzenia nowego projektu")

    *Tworzenie nowego projektu*
2. W **nowy projekt** okno dialogowe, wybierz opcję **wydajności sieci Web i projektu testu obciążenia** w obszarze **Visual C# | Test** kartę. Upewnij się, że **.NET Framework 4.5** jest zaznaczone, nazwa projektu *WebAndLoadTestProject*, wybierz **lokalizacji** i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu sieci Web i testów obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "tworzenia nowego projektu sieci Web i testów obciążenia")

    *Tworzenie nowego projektu sieci Web i testów obciążenia*
3. W **WebTest1.webtest** kliknij prawym przyciskiem myszy **WebTest1** węzeł i kliknij przycisk **Dodaj żądania**.

    ![Dodawanie żądanie do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Dodawanie żądanie do WebTest1")

    *Dodawanie żądanie do WebTest1*
4. W **właściwości** okno nowego węzła żądanie aktualizacji **adres Url** właściwości, aby wskazywał adres URL aplikacji sieci web (np.  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Zmiana właściwości adresu Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "zmianę właściwości adresu Url")

    *Zmiana właściwości adresu Url*
5. W **WebTest1.webtest** okna, kliknij prawym przyciskiem myszy **WebTest1** i kliknij przycisk **Dodawanie pętli...** .

    ![Dodawanie pętli do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Dodawanie pętli do WebTest1")

    *Dodawanie pętli do WebTest1*
6. W **Dodaj regułę warunkową i elementy do pętli** okno dialogowe, wybierz opcję **pętli For** reguły i zmodyfikuj następujące właściwości.

    1. **Przerywanie wartość:** 1000
    2. **Nazwa parametru kontekstu:** iteratora
    3. **Wartość przyrostu:** 1

    ![Wybranie reguły dla pętli i aktualizowanie właściwości](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "zaznaczając regułę dla pętli i aktualizowanie właściwości")

    *Wybranie reguły dla pętli i aktualizowanie właściwości*
7. W obszarze **elementów w pętli** wybierz wcześniej utworzony jako pierwszy i ostatni element dla pętli żądania. Kliknij przycisk **OK** aby kontynuować.

    ![Wybieranie pierwszy i ostatni element dla pętli](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "wybranie pierwszy i ostatni element dla pętli")

    *Wybieranie pierwszy i ostatni element dla pętli*
8. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebAndLoadTestProject** projektu, rozwiń **Dodaj** menu i wybierz **testu obciążenia...** .

    ![Dodawanie testu obciążenia do projektu WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "dodanie do projektu WebAndLoadTestProject testu obciążenia")

    *Dodawanie do projektu WebAndLoadTestProject testu obciążenia*
9. W **nowy Kreator testu obciążenia** okno dialogowe, kliknij przycisk **dalej**.

    ![Nowy Kreator testu obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "nowy Kreator testu obciążenia")

    *Nowy Kreator testu obciążenia*
10. W **scenariusza** wybierz pozycję **nie używaj czasów namysłu** i kliknij przycisk **dalej**.

    ![Wybranie opcji, aby nie używaj czasów namysłu](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "wybranie opcji, aby nie używaj czasów namysłu")

    *Wybranie opcji nie używaj czasów namysłu*
11. W **wzorca obciążenia** strony, upewnij się, że **stałej obciążenia** opcja jest zaznaczona. Zmień **liczba użytkowników** ustawienie **250** użytkowników i kliknij przycisk **dalej**.

    ![Zmiana liczby użytkowników na 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "zmiana liczby użytkowników na 250")

    *Zmiana liczby użytkowników na 250*
12. W **Model mieszanki testów** wybierz pozycję **oparty na sekwencyjnej kolejności testów** i kliknij przycisk **dalej**.

    ![Wybieranie model testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "wybranie model testu mieszanego")

    *Wybieranie model testu mieszanego*
13. W **Model testu mieszanego** kliknij przycisk **Dodaj...**  można dodać testu z różnymi.

    ![Dodawanie do testu mieszanego testu](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "dodanie do testu mieszanego testu")

    *Dodawanie do testu mieszanego testu*
14. W **Dodaj testy** okno dialogowe, kliknij dwukrotnie **WebTest1** test, aby dodać **wybrane testy** listy. Kliknij przycisk **OK** aby kontynuować.

    ![Dodawanie testu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Dodawanie testu WebTest1")

    *Dodawanie testu WebTest1*
15. W **testu mieszanego** kliknij przycisk **dalej**.

    ![Kończenie strony testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "zakończeniu strony mieszanki testów")

    *Kończenie strony mieszanki testów*
16. W **mieszany profil sieciowy** kliknij przycisk **dalej**.

    ![Kliknięcie przycisku Dalej na stronie mieszany profil sieciowy](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "kliknięcie przycisku Dalej na stronie mieszany profil sieciowy")

    *Kliknięcie przycisku Dalej na stronie mieszany profil sieciowy*
17. W **rozkład przeglądarek** wybierz pozycję **Internet Explorer 10.0** jako typ przeglądarki i kliknij przycisk **dalej**.

    ![Wybierając typ przeglądarki](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "wybierając typ przeglądarki")

    *Wybierając typ przeglądarki*
18. W **zbiorów liczników** kliknij przycisk **dalej**.

    ![Kliknięcie przycisku Dalej na stronie zbiorów liczników](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "kliknięcie przycisku Dalej na stronie zestawy liczników")

    *Kliknięcie przycisku Dalej na stronie zestawy liczników*
19. W **parametrów uruchomieniowych** ustaw **czas trwania testu obciążenia** do **5 minut** i kliknij przycisk **Zakończ**.

    ![Ustawienie czas trwania testu obciążenia na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "ustawienie czas trwania testu obciążenia na 5 minut")

    *Ustawienie czas trwania testu obciążenia na 5 minut*
20. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Local.settings** plik, aby eksplorować ustawienia testu. Domyślnie program Visual Studio będzie korzystać komputera lokalnego do uruchamiania testów.

    > [!NOTE]
    > Alternatywnie można skonfigurować projektu testowego do uruchamiania testów obciążenia w chmurze przy użyciu **programu Visual Studio Online (VSO)**. Usługi VSO zapewnia obciążenia opartego na chmurze testowania usługi, która symuluje bardziej realistyczne obciążenia, unikając lokalnego środowiska ograniczenia, takie jak wydajność procesora CPU, pamięci i przepustowości sieci. Aby uzyskać więcej informacji na temat uruchamiania testów obciążenia przy użyciu usługi VSO zobacz [w tym artykule](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Ustawienia testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Zadanie 3 — Weryfikacja skalowania automatycznego

Można teraz wykonania testu obciążenia utworzony w poprzednim zadaniu i zobacz, jak aplikacji sieci web ma zachowywać się pod obciążeniem.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **LoadTest1.loadtest** otworzyć testu obciążenia.

    ![Otwieranie LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "otwierania LoadTest1.loadtest")

    *Otwieranie LoadTest1.loadtest*
2. W **LoadTest1.loadtest** okna, kliknij pierwszy przycisk w przyborniku, aby uruchomić test obciążenia.

    ![Uruchamianie testu obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "wykonywania testu obciążenia")

    *Uruchamianie testu obciążenia*
3. Poczekaj na zakończenie testu obciążenia.

    > [!NOTE]
    > Test obciążenia symuluje wielu użytkowników, którzy jednocześnie wysyłać żądania do aplikacji sieci web. Gdy test jest uruchomiony, można monitorować dostępne liczniki, aby wykryć ewentualne błędy, ostrzeżenia lub inne informacje powiązane z testu obciążenia.

    ![Uruchomiony test obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "oczekiwanie, aż do zakończenia testu obciążenia")

    *Uruchomiony test obciążenia*
4. Po zakończeniu testu, wróć do portalu zarządzania i przejdź do **skali** strony aplikacji sieci web. W obszarze **pojemności** sekcji, powinny być widoczne na wykresie automatycznie wdrożono nowego wystąpienia.

    ![Nowe wystąpienie automatycznie wdrożona](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nowe wystąpienie automatycznie wdrożona*

    > [!NOTE]
    > Może upłynąć kilka minut, aby zmiany są wyświetlane na wykresie (naciśnij klawisz **CTRL + F5** okresowo, aby odświeżyć stronę). Jeśli nie ma żadnych zmian, można spróbować wykonać następujące czynności:
    > 
    > - Zwiększ czas trwania testu obciążenia (np. **10 minut**)
    > - Zmniejszyć maksymalne i minimalne wartości **Procesora docelowej** zakresu w konfiguracji automatycznego skalowania aplikacji sieci web
    > - Uruchamianie testu obciążenia w chmurze za pomocą **Visual Studio Online**. Więcej informacji [tutaj](https://www.visualstudio.com/en-us/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego wiesz, jak skonfigurować i wdrożyć aplikację do produkcji aplikacji sieci web na platformie Azure. Uruchomione przez wykrywanie i aktualizowanie baz danych przy użyciu **migracje Code First Framework jednostki**, następnie kontynuowane przez wdrożenie nowej wersji przy użyciu witryny **Git** i cofnięcia do wykonywania Najnowsza stabilna wersja lokacji. Ponadto przedstawiono sposób Skaluj aplikację przy użyciu magazynu można przenieść zawartości statycznej do kontenera obiektów Blob.
