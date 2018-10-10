---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Warsztaty: Azure websites w warstwie łatwego w utrzymaniu: Zarządzanie zmianami i skalowaniem | Dokumentacja firmy Microsoft'
author: rick-anderson
description: W tym środowisku laboratoryjnym Dowiedz się, jak Microsoft Azure ułatwia tworzenie i wdrażanie witryn sieci Web w środowisku produkcyjnym.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: bc6de2f0c8b2cd958c198abb90fc4ad97613e973
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913310"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Warsztaty: Azure websites w warstwie łatwego w utrzymaniu: Zarządzanie zmianami i skalowaniem
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](http://aka.ms/webcamps-training-kit)

> Microsoft Azure ułatwia tworzenie i wdrażanie witryn sieci Web w środowisku produkcyjnym. Ale wszystko nie będzie gotowe, gdy aplikacja jest aktywna, po prostu rozpoczynasz pracę! Konieczne będzie obsługiwać zmiany wymagań, aktualizacji bazy danych, skalowania i więcej. Na szczęście usługa Azure App Service zapewnia pełne wsparcie, z dużą ilością funkcje ułatwiające zachowują te witryny działających normalnie.
>
> Platforma Azure oferuje bezpieczne i elastyczne programowania, wdrażania i skalowania opcje dla aplikacji sieci web dowolnej wielkości. Korzystaj z istniejących narzędzi do tworzenia i wdrażania aplikacji bez konieczności zawracania sobie głowy zarządzaniem infrastrukturą.
>
> Aprowizowanie aplikacji sieci web w środowisku produkcyjnym samodzielnie w ciągu kilku minut przez łatwe wdrożenie zawartości utworzonej przy użyciu ulubionego narzędzia deweloperskiego. Możesz wdrożyć istniejącą witrynę bezpośrednio z kontroli źródła z obsługą **Git**, **GitHub**, **Bitbucket**, **TFS**, a nawet  **DropBox**. Wdróż bezpośrednio z ulubionego środowiska IDE lub ze skryptów, używając **PowerShell** w Windows lub **interfejsu wiersza polecenia** narzędzia uruchamianie w dowolnym systemie operacyjnym. Po wdrożeniu zachowują te witryny stale aktualnych dzięki obsłudze ciągłego wdrażania.
>
> System Azure oferuje skalowalne i niezawodne magazyn w chmurze, kopii zapasowej i rozwiązania w zakresie odzyskiwania danych i małymi ilościami. W przypadku wdrażania aplikacji dla środowiska produkcyjnego, usługi magazynu, takie jak tabele, obiekty BLOB i bazy danych SQL, ułatwić skalowanie aplikacji w chmurze.
>
> Z bazami danych SQL ważne jest zapewnić aktualność wydajność bazy danych podczas wdrażania nowej wersji aplikacji. Dzięki **migracje Code First Framework jednostki**, tworzenia i wdrażania modelu danych został uproszczony, aby zaktualizować swoje środowiska w ciągu kilku minut. To ćwiczenie praktyczne pokazują różne tematy, które mogą wystąpić podczas wdrażania aplikacji sieci web na środowisko produkcyjne w systemie Microsoft Azure.
>
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
>
> Aby uzyskać więcej szczegółowe informacje dotyczące tego tematu, zobacz [tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Włącz migracją architektury jednostek przy użyciu istniejącego modelu
- Aktualizowanie modelu obiektów i odpowiednio przy użyciu migracją architektury jednostek bazy danych
- Wdrażanie w usłudze Azure App Service przy użyciu narzędzia Git
- Wycofywanie do poprzedniego wdrożenia za pomocą portalu zarządzania platformy Azure
- Umożliwia skalowanie aplikacji sieci web usługi Azure Storage
- Konfigurowanie automatycznego skalowania dla aplikacji sieci web za pomocą portalu zarządzania systemu Azure
- Tworzenie i konfigurowanie projektu testów obciążeniowych w programie Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej
- [Zestaw Azure SDK dla platformy .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [System kontroli wersji GIT](http://git-scm.com/download)
- Subskrypcja Microsoft Azure

    - Zaloguj się w celu [bezpłatnej wersji próbnej](http://aka.ms/watk-freetrial)
    - Jeśli masz program Visual Studio Professional, Test Professional, Premium lub Ultimate z subskrypcją MSDN lub platform MSDN, aktywować swoje [korzyść MSDN](http://aka.ms/watk-msdn) teraz, aby rozpocząć tworzenie i testowanie na platformie Azure
    - [BizSpark](http://aka.ms/watk-bizspark) członków automatycznie otrzymują Azure korzyści za pomocą Visual Studio Ultimate z subskrypcją MSDN
    - Elementy członkowskie [sieci Microsoft Partner Network](http://aka.ms/watk-mpn) programu Cloud Essentials otrzymywać miesięczne środki na korzystanie z platformy Azure bez dodatkowych opłat

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.

1. Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.
3. Jeśli zostanie wyświetlone okno Kontrola konta użytkownika, upewnij się, działania, aby kontynuować.

> [!NOTE]
> Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Za pomocą fragmentów kodu

W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu. Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania. Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Korzystanie z migracją architektury jednostek](#Exercise1)
2. [Wdrażanie aplikacji internetowej w środowisku przejściowym](#Exercise2)
3. [Wykonywanie wycofywania wdrożenia w środowisku produkcyjnym](#Exercise3)
4. [Skalowanie przy użyciu usługi Azure Storage](#Exercise4)
5. [Przy użyciu automatycznego skalowania dla aplikacji sieci Web](#Exercise5) (opcjonalne dla programu Visual Studio 2013 Ultimate edition)

Szacowany czas do ukończenia tego laboratorium: **75 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień. Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Ćwiczenie 1: Przy użyciu migracją architektury jednostek

Opracowując aplikację, modelu danych mogą ulec zmianie wraz z upływem czasu. Te zmiany mogą mieć wpływ na istniejący model w bazie danych (Jeśli tworzysz nową wersję) i ważne jest, aby zachować aktualne, aby zapobiec wystąpieniu błędów bazy danych.

Aby uprościć monitorowanie tych zmian w modelu, **migracje Code First Framework jednostki** automatycznie Wykryj zmiany porównanie modelu ze schematem bazy danych i generuje konkretnego kodu, aby zaktualizować bazę danych, Tworzenie nowego *wersji* bazy danych.

To ćwiczenie dowiesz się, jak włączyć **migracje** swojej aplikacji i jak można łatwo wykryć i generowanie zmiany można zaktualizować bazy danych.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Zadanie 1 — Włączanie migracji

W tym zadaniu, konieczne będzie przejście przez kroki umożliwiające **migracje Code First Framework jednostki** do **Quiz maniaków komputerowych** baza danych, zmiana modelu i zrozumienie, jak te zmiany zostaną uwzględnione w Baza danych.

1. Otwórz program Visual Studio i Otwórz **GeekQuiz.sln** plikiem rozwiązania z **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Skompiluj rozwiązanie, aby pobrać i zainstalować **NuGet** pakietów zależności. Aby to zrobić, kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Kompiluj rozwiązanie** lub naciśnij **Ctrl + Shift + B**.
3. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżera pakietów NuGet**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.
4. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**. Początkowej migracji, na podstawie istniejącego modelu zostanie utworzony.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Włączanie migracji](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Włączanie migracji")

    *Włączanie migracji*

    > [!NOTE]
    > To polecenie dodaje **migracje** folderze Quiz maniaków komputerowych projektu, który zawiera plik o nazwie **Configuration.cs**. **Konfiguracji** klasy można skonfigurować sposób działania migracji w Twoim kontekście.
5. Za pomocą migracji jest włączona, należy zaktualizować **konfiguracji** klasy do wypełniania bazy danych przy użyciu danych początkowych, **Quiz maniaków komputerowych** wymaga. W obszarze **migracje**, Zastąp **Configuration.cs** plik z elementem zlokalizowany w **Source\Assets** folder w tym laboratorium.

    > [!NOTE]
    > Ponieważ **migracje** wywoła **inicjatora** metody przy każdej aktualizacji bazy danych, należy się upewnić, że rekordy nie są duplikowane w bazie danych. **AddOrUpdate** metoda pomoże zapobiec zduplikowanych danych.
6. Aby dodać początkowej migracji, wpisz następujące polecenie, a następnie naciśnij klawisz **Enter**.

    > [!NOTE]
    > Upewnij się, że nie istnieje żadna baza danych o nazwie &quot;GeekQuizProd&quot; wystąpienia LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Dodawanie migracji schematu podstawowego](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Dodawanie schemat bazowy migracji")

    *Dodawanie migracji schematu podstawowego*

    > [!NOTE]
    > **Dodaj migracji** będzie tworzenia szkieletu dalej migracji, w oparciu o zmiany wprowadzone do modelu od chwili utworzenia ostatniej migracji. W takim przypadku po wykonaniu pierwszej migracji projektu, doda skryptów do tworzenia wszystkich tabel, które są zdefiniowane w **TriviaContext** klasy.
7. Wykonaj migrację do aktualizacji bazy danych, uruchamiając następujące polecenie. Dla tego polecenia należy określić **pełne** flagę, aby wyświetlić instrukcje SQL są stosowane do docelowej bazy danych.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Tworzenie początkowej bazy danych](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "tworzenie początkowej bazy danych")

    *Tworzenie początkowej bazy danych*

    > [!NOTE]
    > **Update-Database** Zastosuj wszelkie oczekujące migracji do bazy danych. W tym przypadku utworzy bazę danych przy użyciu parametrów połączenia, zdefiniowane w pliku web.config.
8. Przejdź do **widoku** menu i Otwórz **Eksplorator obiektów SQL Server**.

    ![Otwórz w Eksploratorze obiektów SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Otwórz w Eksploratorze obiektów SQL Server")

    *Otwórz w Eksploratorze obiektów SQL Server*
9. W **Eksplorator obiektów SQL Server** okna, nawiąż połączenie z wystąpieniem usługi LocalDB, klikając prawym przyciskiem myszy **programu SQL Server** węzła i wybierając polecenie **dodawania serwera SQL...**  opcji.

    ![Dodawanie wystąpienia programu SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "dodanie wystąpienia programu SQL Server")

    *Dodawanie wystąpienia programu SQL Server do Eksplorator obiektów SQL Server*
10. Ustaw **nazwy serwera** do *(localdb) \v11.0* i pozostawić **uwierzytelniania Windows** z trybem uwierzytelniania. Kliknij przycisk **Connect** aby kontynuować.

    ![Nawiązywanie połączenia z LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "nawiązywania połączenia z LocalDB")

    *Nawiązywanie połączenia z LocalDB*
11. Otwórz **GeekQuizProd** bazy danych, a następnie rozwiń węzeł **tabel** węzła. Jak widać, **Update-Database** polecenia wygenerowany wszystkich tabel, które są zdefiniowane w **TriviaContext** klasy. Znajdź **dbo. TriviaQuestions** tabeli, a następnie otwórz węzeł kolumny. W ramach następnego zadania będzie dodać nową kolumnę do tej tabeli i zaktualizować bazę danych za pomocą **migracje**.

    ![Elementy towarzyszące składni pytania kolumn](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "elementy towarzyszące składni pytania kolumn")

    *Elementy towarzyszące składni pytania kolumn*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Zadanie 2 — Aktualizowanie schematu bazy danych przy użyciu migracji

W tym zadaniu zostanie użyty **migracje Code First Framework jednostki** wykrywa zmian w modelu i generowania kodu niezbędne do aktualizacji bazy danych. Spowoduje zaktualizowanie **TriviaQuestions** jednostki, dodając nowe właściwości do niego. Następnie uruchom polecenia, aby utworzyć nową migrację, aby uwzględnić nową kolumnę w tabeli.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **TriviaQuestion.cs** plik znajdujący się wewnątrz **modeli** folderu.
2. Dodaj nową właściwość o nazwie **wskazówka**, jak pokazano w poniższym fragmencie kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**. Nowej migracji zostanie utworzony, odzwierciedlając zmiany w naszym modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Dodaj migracji](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Dodaj migracji")

    *Dodaj migracji*

    > [!NOTE]
    > Plik migracji składa się z dwóch metod **się** i **dół**.
    >
    > - **Się** metoda będzie używana do określenia, jakie zmiany bieżącej wersji naszych potrzeb aplikacji, aby zastosować do bazy danych.
    > - **Dół** służy do odwrócić zmiany dodanych do **się** metody.
    >
    > Podczas migracji bazy danych aktualizuje bazę danych, będzie ona uruchamiana wszystkich migracji w kolejności sygnatura czasowa, a tylko te, które nie były używane od czasu ostatniej aktualizacji ( \_tabeli MigrationHistory śledzi informacje o migracji, które zostały zastosowane). **Się** metoda wszystkich migracji zostanie wywołana i dokona zmian została określona w bazie danych. Jeśli firma chce przejść z powrotem do poprzedniej migracji **dół** metoda zostanie wywołana w celu ponownie dokonać zmian w odwrotnej kolejności.
4. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Dane wyjściowe **Update-Database** polecenia wygenerowany **instrukcji Alter Table** instrukcję SQL w celu dodania nowej kolumny do **TriviaQuestions** tabeli, jak pokazano na poniższej ilustracji.

    ![Dodaj instrukcję SQL kolumny generowane](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Dodaj instrukcję SQL kolumny wygenerowany")

    *Dodaj instrukcję SQL kolumny wygenerowany*
6. W **Eksplorator obiektów SQL Server**, Odśwież **dbo. TriviaQuestions** tabeli i sprawdź, czy nowy **wskazówka** kolumna jest wyświetlana.

    ![Wyświetlanie nową kolumnę wskazówka](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "przedstawiający nową kolumnę wskazówka")

    *Wyświetlanie nową kolumnę wskazówka*
7. Ponownie **TriviaQuestion.cs** edytorze Dodaj **StringLength** ograniczenie *wskazówka* właściwości, jak pokazano w poniższym fragmencie kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. W **Konsola Menedżera pakietów**, wprowadź następujące polecenie i naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Dane wyjściowe **Update-Database** polecenia wygenerowany **instrukcji Alter Table** instrukcję SQL, aby zaktualizować *wskazówka* typ kolumny **TriviaQuestions** tabeli, jak pokazano na poniższej ilustracji.

    ![Instrukcja SQL kolumny generowane ALTER](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter wygenerowano instrukcji SQL kolumny")

    *Instrukcja ALTER wygenerowano instrukcji SQL kolumny*
11. W **Eksplorator obiektów SQL Server**, Odśwież **dbo. TriviaQuestions** tabeli i sprawdź, czy **wskazówka** typ kolumny jest **nvarchar(150)**.

    ![Wyświetlanie nowego ograniczenia](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "przedstawiający nowe ograniczenie")

    *Wyświetlane nowe ograniczenie*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Ćwiczenie 2: Wdrażanie aplikacji internetowej w środowisku przejściowym

**Aplikacji sieci Web w usłudze Azure App Service** umożliwia publikowanie etapowe. Publikowanie etapowe tworzy gniazdo tymczasowej witryny dla każdej domyślnej witryny produkcyjnej i umożliwia zastępowanie tych gniazd bez przerw. Jest to bardzo przydatne do sprawdzenia poprawności zmian przed zwolnieniem publicznie, przyrostowo zintegrować zawartość witryny i wdrożyć ponownie, jeśli zmiany nie działają zgodnie z oczekiwaniami.

W tym ćwiczeniu wdrożysz **Quiz maniaków komputerowych** aplikacji do środowiska pomostowego aplikacji sieci web przy użyciu kontroli źródła Git. Aby to zrobić, będzie tworzenie aplikacji sieci web i zainicjować obsługę administracyjną wymaganych składników w portalu zarządzania, skonfiguruj **Git** repozytorium i wypychanie aplikacji źródłowej kod z komputera lokalnego do miejsca przejściowego. Zostanie również zaktualizować produkcyjnej bazy danych za pomocą **migracje Code First** utworzonego w poprzednim ćwiczeniu. Następnie wykona aplikacji w tym środowisku testowym, aby sprawdzić jego działanie. Testy okażą się satysfakcjonujące, że działa zgodnie z Twoim oczekiwaniom, spowoduje podwyższenie poziomu aplikacji do środowiska produkcyjnego.

> [!NOTE]
> Aby włączyć publikowanie etapowe, aplikacji sieci web musi należeć do **Tryb standardowy**. Należy pamiętać, że dodatkowe opłaty będą naliczane w przypadku aplikacji sieci web zmiany trybu na standardowy. Aby uzyskać więcej informacji o cenach, zobacz [App Service — ceny](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Zadanie 1 — Tworzenie aplikacji sieci Web w usłudze Azure App Service

W tym zadaniu utworzysz aplikację internetową w **usługi Azure App Service** w portalu zarządzania. Skonfigurujesz również **bazy danych SQL** do utrwalenia danych aplikacji i konfigurowanie lokalnego repozytorium Git do kontroli źródła.

1. Przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Microsoft skojarzonego z Twoją subskrypcją.

    ![Zaloguj się do portalu zarządzania systemu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Zaloguj się do portalu zarządzania systemu Azure*
2. Kliknij przycisk **New** na pasku poleceń u dołu strony.

    ![Tworzenie nowej aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Tworzenie nowej aplikacji sieci web")

    *Tworzenie nowej aplikacji sieci web*
3. Kliknij przycisk **obliczenia**, **witryny sieci Web** i następnie **tworzenie niestandardowe**.

    ![Tworzenie nowej aplikacji sieci web przy użyciu tworzenie niestandardowe](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "utworzenie nowej aplikacji sieci web przy użyciu tworzenie niestandardowe")

    *Tworzenie nowej aplikacji sieci web przy użyciu tworzenie niestandardowe*
4. W **nowa witryna — tworzenie niestandardowe** okna dialogowego wprowadź dostępne **adresu URL** (np. *maniaków komputerowych quiz*), wybierz lokalizację, w **Region** listy rozwijanej i wybierz pozycję **Tworzenie nowej bazy danych SQL** w **bazy danych** listy rozwijanej. Na koniec wybierz pozycję **publikowanie z kontroli źródła** pole wyboru i kliknij przycisk **dalej**.

    ![Dostosowywanie nowej aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Dostosowywanie nowej aplikacji sieci web*
5. Określ następujące informacje dotyczące ustawień bazy danych:

   - W **nazwa** tekstu wprowadź nazwę bazy danych (np. *geekquiz\_db*)
   - Na serwerze **listy rozwijanej** listy wybierz **serwer bazy danych SQL nowe**. Alternatywnie można wybrać istniejący serwer.
   - W **użytkownika bazy danych** i **hasło bazy danych** wprowadź nazwę użytkownika administratora i hasło dla serwera bazy danych SQL. W przypadku wybrania opcji serwera została już utworzona, zostanie wyświetlony monit o hasło.

     ![Określanie ustawień bazy danych](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Określanie ustawień bazy danych*
6. Kliknij przycisk **Dalej** , aby kontynuować.
7. Wybierz **lokalnym repozytorium Git** do kontroli źródła, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Może pojawić się prośba o poświadczenia wdrożenia (nazwa użytkownika i hasło).

    ![Tworzenie repozytorium Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Tworzenie repozytorium Git*
8. Zaczekaj, aż utworzeniu nowej aplikacji sieci web.

    > [!NOTE]
    > Domyślnie platforma Azure udostępnia domen w *azurewebsites.net* , ale też zapewnia możliwość ustawienia domen niestandardowych przy użyciu portalu zarządzania systemu Azure. Jednakże można zarządzać tylko domen niestandardowych korzystania z niektórych tryby w usłudze Azure App Service.
    >
    > Usługa Azure App Service jest dostępna w wersjach bezpłatna, współdzielona, podstawowa, standardowa i Premium. W trybie bezpłatna i współdzielona wszystkie aplikacje sieci web działają w środowisku wielodostępnym i mają limity przydziału użycia procesora CPU, pamięci i sieci. Maksymalna liczba bezpłatnych aplikacji mogą się różnić z planem. W trybie standardowym możesz wybrać aplikacje, które Uruchom na dedykowanych maszynach wirtualnych, które odpowiadają Azure standard zasobów obliczeniowych. Można znaleźć konfiguracji trybu aplikacji internetowej w **skalowania** menu aplikacji sieci web.
    >
    > ![Usługa Azure App Service tryby](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "tryby w usłudze Azure App Service")
    >
    > Jeśli używasz **Shared** lub **standardowa** trybu, można zarządzać domen niestandardowych dla aplikacji sieci web, przechodząc do swojej aplikacji **Konfiguruj** menu i klikając **Zarządzanie domenami** w obszarze *nazw domen*.
    >
    > ![Zarządzanie domenami](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Zarządzanie domenami")
    >
    > ![Zarządzanie domenami niestandardowymi](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Zarządzanie domenami niestandardowymi")
9. Po utworzeniu aplikacji internetowej kliknij link w obszarze **adresu URL** kolumny, aby sprawdzić, czy nowa aplikacja sieci web jest uruchomiona.

    ![Przejście do nowej aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Przejście do nowej aplikacji sieci web*

    ![Aplikacja sieci Web uruchomiona](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *Aplikacja sieci Web uruchomiona*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Zadanie 2 — Tworzenie produkcyjnej bazy danych SQL

W tym zadaniu zostanie użyty **migracje Code First Framework jednostki** do utworzenia bazy danych, przeznaczonych dla **usługi Azure SQL Database** wystąpienia utworzone w poprzednim zadaniu.

1. W portalu zarządzania, przejdź do aplikacji internetowej utworzonej w poprzednim zadaniu, a następnie przejdź do jej **pulpit nawigacyjny**.
2. W **pulpit nawigacyjny** kliknij **Wyświetl parametry połączeń** łącze w obszarze **Przegląd** sekcji.

    ![Wyświetl parametry połączeń](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Wyświetl parametry połączeń")

    *Wyświetl parametry połączenia*
3. Kopiuj **parametry połączenia** wartości i zamknąć okno dialogowe.

    ![Parametry połączenia w portalu zarządzania systemu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "parametry połączenia w portalu zarządzania systemu Azure")

    *Parametry połączenia w portalu zarządzania systemu Azure*
4. Kliknij przycisk **baz danych SQL** Aby wyświetlić listę baz danych SQL na platformie Azure

    ![Menu bazy danych SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "menu bazy danych SQL")

    *Menu bazy danych SQL*
5. Znajdź bazy danych utworzonej w poprzednim zadaniu, a następnie kliknij polecenie na serwerze.

    ![Serwer usługi SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "bazy danych programu SQL server")

    *Serwer usługi SQL Database*
6. W **— Szybki Start** strony serwera, kliknij pozycję **Konfiguruj**.

    ![Konfiguruj menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Konfigurowanie menu")

    *Konfigurowanie menu*
7. W **adresy IP mogą** sekcji, kliknij pozycję **Dodaj dozwolone adresy IP** link, aby włączyć Twój adres IP nawiązać połączenie z serwerem usługi SQL Database.

    ![Dozwolone adresy IP](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "adresy IP dozwolone")

    *Dozwolone adresy IP*
8. Kliknij przycisk **Zapisz** w dolnej części strony Aby ukończyć etap tworzenia.
9. Przełącz się do programu Visual Studio.
10. W **Konsola Menedżera pakietów**, wykonaj następujące polecenie zastępowanie *[YOUR-CONNECTION-STRING]* symbolu zastępczego parametrami połączenia skopiowane z platformy Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualizuj bazę danych, przeznaczonych dla systemu Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aktualizacji bazy danych, przeznaczonych dla systemu Windows Azure SQL Database")

    *Aktualizuj bazę danych, przeznaczonych dla usługi Azure SQL Database*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Zadanie 3 — wdrażanie Quiz maniaków komputerowych do wdrażania przejściowego, przy użyciu narzędzia Git

W ramach tego zadania spowoduje włączenie publikowania etapowego w aplikacji sieci web. Następnie użyje Git do publikowania aplikacji maniaków komputerowych Quiz, bezpośrednio z komputera lokalnego do środowiska pomostowego aplikacji sieci web.

1. Wróć do portalu i kliknij nazwę aplikacji sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania aplikacji sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Otwieranie strony zarządzania aplikacji sieci web*
2. Przejdź do **skalowania** strony. W obszarze **ogólne** zaznacz **standardowa** konfiguracji, a następnie kliknij przycisk **Zapisz** na pasku poleceń.

    > [!NOTE]
    > Do uruchomienia wszystkich aplikacji sieci web w bieżącym regionie i subskrypcji w **standardowa** tryb, pozostaw **Zaznacz wszystko** zaznaczone pole wyboru w **wybierz Lokacje** konfiguracji. W przeciwnym razie wyczyść **Zaznacz wszystko** pole wyboru.

    ![Uaktualnianie aplikacji sieci web trybu na standardowy](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "uaktualnianie aplikacji sieci web trybu na standardowy")

    *Uaktualnianie aplikacji sieci Web trybu na standardowy*
3. Kliknij przycisk **tak** aby potwierdzić zmiany.

    ![Potwierdzanie zmiany trybu na standardowy](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "kontynuować zmianę trybu aplikacji sieci web")

    *Potwierdzanie zmiany trybu na standardowy*
4. Przejdź do **pulpit nawigacyjny** strony, a następnie kliknij przycisk **włączyć publikowanie etapowe** w obszarze **Przegląd** sekcji.

    ![Włączanie publikowania etapowego](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "włączenie publikowanie etapowe")

    *Włączanie publikowania etapowego*
5. Kliknij przycisk **tak** umożliwiające publikowanie etapowe.

    ![Publikowanie etapowe w potwierdzeniu](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "kliknięcie przycisku Tak, aby umożliwić publikowanie etapowe")

    *Trwa Potwierdzanie publikowanie etapowe*
6. Na liście aplikacji sieci web rozwiń znak z lewej strony nazwy aplikacji sieci web do wyświetlenia miejsce na tymczasową witrynę. Ta kolumna ma nazwę aplikacji sieci web, a następnie ***(przemieszczania)***. Kliknij lokację tymczasowej, aby przejść do strony zarządzania.

    ![Przejdź do tymczasowej webapp](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "przechodzenia do tymczasowej webapp")

    *Przejdź do aplikacji przemieszczania*
7. Zwróć uwagę, że strona zarządzania tym he wygląda podobnie jak strony zarządzania żadnych innych aplikacji sieci web. Przejdź do **wdrożeń** strony i skopiuj **adres URL Git** wartość. Użyjesz go w dalszej części tego ćwiczenia.

    ![Kopiowanie wartości adresu URL usługi Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopiowanie wartości adresu URL usługi Git*
8. Otwórz nowy **powłoki Git Bash** konsoli i wykonaj następujące polecenia. Aktualizacja *[YOUR-aplikacji-PATH]* symbolu zastępczego na ścieżkę do **GeekQuiz** rozwiązanie, znajduje się w **Source\Ex1 DeployingWebSiteToStaging\Begin** folderu w tym laboratorium.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicjowanie usługi Git i pierwszego zatwierdzenia](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicjowanie usługi Git i pierwszego zatwierdzenia*
9. Uruchom następujące polecenie, aby wypchnąć aplikacji sieci web do zdalnego **Git** repozytorium. Zastąp symbol zastępczy adres URL, które zostały uzyskane z portalu zarządzania. Użytkownik jest monitowany o podanie hasła wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Wypychanie do usługi Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Wypychanie do platformy Azure*

    > [!NOTE]
    > Podczas wdrażania zawartości do hosta FTP lub repozytorium GIT aplikacji sieci web, należy uwierzytelnić za pomocą **poświadczenia wdrożenia** utworzony przy użyciu aplikacji sieci web **— Szybki Start** lub **pulpitu nawigacyjnego**  strony zarządzania. Jeśli nie znasz poświadczenia wdrożenia można łatwo można zresetować je za pomocą portalu zarządzania. Otwórz aplikację sieci web **pulpit nawigacyjny** strony, a następnie kliknij przycisk **resetowanie poświadczeń wdrażania** łącza. Podaj nowe hasło, a następnie kliknij przycisk **OK**. Poświadczenia wdrożenia są prawidłowa do stosowania z wszystkich aplikacji sieci web skojarzony z subskrypcją.
10. Aby sprawdzić, czy aplikacja sieci web została pomyślnie wypchnięto do platformy Azure, wróć do portalu zarządzania, a następnie kliknij przycisk **witryn sieci Web**.
11. Wyszukaj aplikację sieci web, a następnie rozwiń pozycję, aby wyświetlić miejsce na tymczasową witrynę. Kliknij jego **nazwa** aby przejść do strony zarządzania.
12. Kliknij przycisk **wdrożeń** się **historii wdrażania**. Sprawdź, czy jest **aktywnego wdrożenia** za pomocą usługi  *&quot;początkowe zatwierdzenie&quot;*.

    ![Aktywne wdrożenie](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktywne wdrożenie*
13. Na koniec kliknij **Przeglądaj** na pasku poleceń, aby przejść do aplikacji sieci web.

    ![Przeglądaj aplikację sieci web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Przeglądaj aplikację sieci web*
14. Jeśli aplikacja zostanie pomyślnie wdrożona, zostanie wyświetlona strona logowania Quiz maniaków komputerowych.

    > [!NOTE]
    > Adres URL wdrożonej aplikacji zawiera nazwę aplikacji sieci web, a następnie *-przemieszczania*.

    ![Aplikacja uruchomiona w środowisku przejściowym](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplikacja uruchomiona w środowisku przejściowym*
15. Jeśli chcesz zapoznać się z aplikacji, kliknij przycisk **zarejestrować** można zarejestrować nowego użytkownika. Wypełnij szczegóły konta, wprowadzając nazwę użytkownika, adres e-mail i hasło. Następnie aplikacja przedstawiono pierwszego pytania quizu. Odpowiedz na kilka pytań, aby upewnić się, że działa zgodnie z oczekiwaniami.

    ![Aplikacja jest gotowa do użycia](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplikacja jest gotowa do użycia*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Zadanie 4 — podwyższania poziomu aplikacji sieci Web w środowisku produkcyjnym

Po upewnieniu się, że aplikacja sieci web działa poprawnie w środowisku przejściowym, można przystąpić do promowania go do środowiska produkcyjnego. W tym zadaniu zostanie zamienić miejsce na tymczasową witrynę z miejscem produkcyjnym w lokacji.

1. Wróć do portalu zarządzania, a następnie wybierz miejsce na tymczasową witrynę. Kliknij przycisk **wymiany** na pasku poleceń.

    ![Przechodź do środowiska produkcyjnego](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Przechodź do środowiska produkcyjnego*
2. Kliknij przycisk **tak** w oknie dialogowym potwierdzenia, aby kontynuować operację wymiany. Azure zostanie natychmiast zamienić zawartość witryny produkcyjnej o zawartości przejściowej lokacji.

    > [!NOTE]
    > Niektóre ustawienia z przygotowaną wersję automatycznie zostaną skopiowane do wersji produkcyjnej (np. parametrów połączenia zastąpienia mapowania programu obsługi, itp.), ale nie spowoduje zmiany innych ustawień, (np. punkty końcowe DNS, powiązania SSL itp.).

    ![Potwierdzanie operacji zamiany](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Potwierdzanie operacji zamiany*
3. Po zakończeniu wymiany, wybierz z miejscem produkcyjnym i kliknij przycisk **Przeglądaj** na pasku poleceń, aby otworzyć witrynę w środowisku produkcyjnym. Zwróć uwagę, adres URL w pasku adresu.

    > [!NOTE]
    > Może być konieczne Odśwież przeglądarkę, aby wyczyścić pamięć podręczną. W programie Internet Explorer można to zrobić, naciskając klawisz **CTRL + R**.

    ![Aplikacja sieci Web działające w środowisku produkcyjnym](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. W **GitBash** konsoli, zaktualizuj zdalny adres URL dla lokalnego repozytorium Git pod kątem miejscem produkcyjnym. Aby to zrobić, uruchom następujące polecenie, zastępując symbole zastępcze swoją nazwę użytkownika wdrożenia i nazwę aplikacji sieci web.

    > [!NOTE]
    > W poniższym ćwiczeniach będzie Wypchnij zmiany do witryny produkcyjnej, zamiast przemieszczania tylko dla uproszczenia w laboratorium. W rzeczywistych scenariuszy zalecane jest aby zweryfikować zmiany w środowisku przejściowym przed podniesieniem poziomu do środowiska produkcyjnego.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Ćwiczenie 3: Wykonywanie wycofywania wdrożenia w środowisku produkcyjnym

Istnieją scenariusze, w których nie masz gniazdo tymczasowej do wykonania wymiany na gorąco między środowiskiem przejściowym i produkcyjnym, na przykład, jeśli pracujesz z **bezpłatna** lub **Shared** trybu. W tych przypadkach należy przetestować aplikację w środowisku testowym — lokalnie lub w lokacji zdalnej — przed wdrożeniem w środowisku produkcyjnym. Jednak jest możliwe, że problem nie są wykrywane podczas fazy testowania mogą pojawiać się w lokacji produkcyjnej. W tym przypadku jest posiadanie mechanizmu łatwo przełączyć się na poprzedni i bardziej stabilną wersję aplikacji tak szybko, jak to możliwe.

W **usługi Azure App Service**, ciągłego wdrażania z kontroli źródła sprawia, że to możliwe dzięki możliwościom **ponownie wdrożyć** akcji dostępnych w portalu zarządzania. Azure śledzi wdrożenia skojarzone z zatwierdzeniami wypchnięty do repozytorium i zapewnia możliwość ponownego wdrażania aplikacji przy użyciu dowolnej z poprzedniego wdrożenia, w dowolnym momencie.

W tym ćwiczeniu zostaną przeprowadzone zmiany do kodu w **Quiz maniaków komputerowych** aplikacji, które celowo wprowadza *usterki*. Wdroży aplikację do środowiska produkcyjnego, aby zobaczyć ten błąd, a następnie skorzystasz z funkcji ponownego wdrażania, aby powrócić do poprzedniego stanu.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Zadanie 1 — aktualizowanie aplikacji Quiz maniaków komputerowych

W tym zadaniu zostanie Refaktoryzuj niewielkim fragmentem kodu **TriviaController** klasy, aby wyodrębnić część logiki, która pobiera opcji wybrany test z bazy danych do nowej metody.

1. Przełącz się do wystąpienia programu Visual Studio za pomocą **GeekQuiz** rozwiązania z poprzednim ćwiczeniu.
2. W **Eksploratora rozwiązań**, otwórz **TriviaController.cs** pliku wewnątrz **kontrolerów** folderu.
3. Znajdź **StoreAsync** i wybierz opcję Kod wyróżniony na poniższej ilustracji.

    ![Wybierając kod](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Wybierając kod*
4. Kliknij prawym przyciskiem myszy wybrany kod, rozwiń **Refaktoryzuj** menu, a następnie wybierz **Wyodrębnij metodę...** .

    ![Trwa wyodrębnianie kod jako nową metodę](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Wybieranie metody wyodrębniania*
5. W **Wyodrębnij metodę** okno dialogowe, nazwę nowej metody *MatchesOption* i kliknij przycisk **OK**.

    ![Określanie nazwy — metoda](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Określenie nazwy dla wyodrębnionych — metoda*
6. Zaznaczony kod jest następnie wyodrębniany do **MatchesOption** metody. Następnie kod wynikowy jest wyświetlany w poniższym fragmencie kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Zadanie 2 — ponownego wdrożenia aplikacji Quiz maniaków komputerowych

Teraz będzie wypychać zmiany wprowadzone w poprzednim zadaniu repozytorium, które wyzwolą nowe wdrożenie w środowisku produkcyjnym. Następnie zostaną rozwiązane problemy problemu za pomocą **narzędzia programistyczne F12** dostarczane przez program Internet Explorer, a następnie wykonać wycofanie do poprzedniego wdrożenia, w portalu zarządzania platformy Azure.

1. Otwórz nowy **powłoki Git Bash** konsoli, aby wdrożyć zaktualizowaną aplikację w usłudze Azure App Service.
2. Wykonaj następujące polecenia, aby wypchnąć zmiany do platformy Azure. Aktualizacja *[YOUR-aplikacji-PATH]* symbolu zastępczego na ścieżkę do **GeekQuiz** rozwiązania. Użytkownik jest monitowany o podanie hasła wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Wypychanie refaktoryzować kod na platformie Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Wypychanie refaktoryzować kod na platformie Azure*
3. Otwórz program Internet Explorer i przejdź do aplikacji sieci web (np. `http://<your-web-site>.azurewebsites.net`). Zaloguj się przy użyciu poświadczeń utworzonego wcześniej.
4. Naciśnij klawisz **F12** można uruchomić narzędzia programistyczne, wybierz **sieci** kartę, a następnie kliknij przycisk **Odtwórz** przycisk, aby rozpocząć nagrywanie.

    ![Uruchomienie rejestrowania sieci](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "uruchomienie rejestrowania sieci")

    *Uruchomienie rejestrowania sieci*
5. Wybierz dowolną opcję quizu. Zobaczysz, że nic się nie dzieje.
6. W **F12** okno, zawiera wpis odpowiadający na żądania POST HTTP HTTP **500** wynik.

    ![HTTP 500 błędów](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 błędów*
7. Wybierz **konsoli** kartę. Ze szczegółami przyczyny zostanie zarejestrowany błąd.

    ![Błąd zarejestrowane](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Błąd zarejestrowane*
8. Znajdź części szczegółów błędu. Wyraźnie widać ten błąd jest spowodowany przez kod, refaktoryzacji, zostanie zatwierdzone w poprzednich krokach.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Nie zamykaj przeglądarki.
10. W nowym wystąpieniu przeglądarki przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Microsoft skojarzonego z Twoją subskrypcją.
11. Wybierz **witryn sieci Web** i kliknij aplikację internetową utworzoną w wersji 2 wykonywania.
12. Przejdź do **wdrożeń** strony. Należy zauważyć, że wszystkie zatwierdzenia, wykonywane są wymienione w historii wdrażania.

    ![Lista istniejących wdrożeń](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Lista istniejących wdrożeń*
13. Wybierz poprzednie zatwierdzenie, a następnie kliknij przycisk **ponownie wdrożyć** na pasku poleceń.

    ![Ponowne wdrażanie poprzednie zatwierdzenie](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Ponowne wdrażanie poprzednie zatwierdzenie*
14. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **tak**.

    ![Potwierdzanie ponownego wdrażania](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Po zakończeniu wdrożenia przejdź z powrotem do wystąpienia przeglądarki z aplikacji sieci web i naciśnij klawisz **klawiszy CTRL + F5**.
16. Kliknij przycisk Opcje. Przerzuć animacji teraz zajmie się miejsce i wynik (*Popraw nieprawidłowe*) będą wyświetlane.
17. (Opcjonalnie) Przełącz się do **powłoki Git Bash** konsoli i wykonaj następujące polecenia, aby powrócić do poprzedniego zatwierdzenia.

    > [!NOTE]
    > Te polecenia powodują utworzenie nowego zatwierdzenia, która cofa wszystkich zmian w repozytorium Git, które zostały wprowadzone w zły zatwierdzenia. Azure następnie wdroży aplikację, używając nowego zatwierdzenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Ćwiczenie 4: Skalowanie przy użyciu usługi Azure Storage

**Obiekty BLOB** to najprostszy sposób przechowywania dużych ilości pozbawionych struktury danych tekstowych i binarnych, takich jak wideo, audio i obrazy. Przenoszenie zawartości statycznej w aplikacji do magazynu, pozwala skalować swoją aplikację przez obsługiwanie obrazów i dokumentów bezpośrednio w przeglądarce.

W tym ćwiczeniu przeniesie zawartości statycznej w aplikacji do kontenera obiektów Blob. Następnie skonfigurujesz aplikację, aby dodać **reguły ponownego zapisywania adresów URL platformy ASP.NET** w **Web.config** do przekierowania zawartość kontenerów obiektów Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Zadanie 1 — Tworzenie konta usługi Azure Storage

W tym zadaniu dowiesz się, jak utworzyć nowe konto magazynu przy użyciu portalu zarządzania.

1. Przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konta Microsoft skojarzonego z Twoją subskrypcją.
2. Wybierz **nowe | Data Services | Magazyn | Szybkie tworzenie** aby rozpocząć tworzenie nowego konta magazynu. Wprowadź unikatową nazwę konta i wybierz **Region** z listy. Kliknij przycisk **Utwórz konto magazynu** aby kontynuować.

    ![Tworzenie nowego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "tworzenia nowego konta magazynu")

    *Tworzenie nowego konta magazynu*
3. W **magazynu** sekcji, poczekaj, aż stan nowego konta magazynu zmieni się na *Online* celu wykonaj poniższe czynności.

    ![Konto magazynu utworzone](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "utworzono konto magazynu")

    *Utworzono konto magazynu*
4. Kliknij nazwę konta magazynu, a następnie kliknij przycisk **pulpit nawigacyjny** widocznego u góry strony. **Pulpit nawigacyjny** strona zawiera informacje o stanie konta i punktów końcowych usługi, które mogą być używane w aplikacjach.

    ![Wyświetlanie pulpitu nawigacyjnego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "wyświetlanie pulpitu nawigacyjnego konta magazynu")

    *Wyświetlanie pulpitu nawigacyjnego konta magazynu*
5. Kliknij przycisk **Zarządzaj kluczami dostępu** przycisk na pasku nawigacyjnym.

    ![Zarządzanie kluczami dostępu przycisk](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "przycisk Zarządzaj kluczami dostępu")

    *Zarządzanie przycisk klucze dostępu*
6. W **Zarządzaj kluczami dostępu** okno dialogowe, kopia **nazwa konta magazynu** i **podstawowy klucz dostępu** jak będą one potrzebne w poniższym ćwiczeniu. Następnie zamknij okno dialogowe.

    ![Okno dialogowe klucz dostępu zarządzanie](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "okno dialogowe Zarządzanie klucz dostępu")

    *Klucz dostępu, okno dialogowe Zarządzanie*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Zadanie 2 — przekazywanie elementu zawartości do usługi Azure Blob Storage

W tym zadaniu użyjesz okno Eksploratora serwera w programie Visual Studio do łączenia się z kontem magazynu. Następnie utworzysz kontener obiektów blob i przekaż plik z logo Quiz maniaków komputerowych do kontenera.

1. Przełącz się do wystąpienia programu Visual Studio za pomocą **GeekQuiz** rozwiązania z poprzednim ćwiczeniu.
2. Na pasku menu wybierz **widoku** a następnie kliknij przycisk **Eksploratora serwera**.
3. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **Azure** a następnie wybierz węzeł **Połącz z platformą Azure...** . Zaloguj się przy użyciu konta Microsoft skojarzonego z Twoją subskrypcją.

    ![Łączenie z platformą Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Łączenie z platformą Azure*
4. Rozwiń **Azure** węzła, kliknij prawym przyciskiem myszy **magazynu** i wybierz **dołączanie zewnętrznej usługi Storage...** .
5. W **dodać nowe konto magazynu** okna dialogowego wprowadź **nazwa konta** i **klucz konta** uzyskanego w poprzednim zadaniu i kliknij **OK**.

    ![Dodaj nowe konto magazynu, okno dialogowe](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Dodaj nowe konto magazynu, okno dialogowe*
6. Konta magazynu powinna zostać wyświetlona w obszarze **magazynu** węzła. Rozwiń swoje konto magazynu, kliknij prawym przyciskiem myszy **obiektów blob** i wybierz **Utwórz kontener obiektów Blob...** .

    ![Tworzenie kontenera obiektów Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "kontener obiektów Blob")

    *Tworzenie kontenera obiektów Blob*
7. W **Utwórz kontener obiektów Blob** okna dialogowego pole, wprowadź nazwę kontenera obiektów blob i kliknij przycisk **OK**.

    ![Okno dialogowe Tworzenie kontenera obiektów Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "okno dialogowe Utwórz kontener obiektów Blob")

    *Tworzenie kontenera obiektów Blob, okno dialogowe*
8. Nowy kontener obiektów blob, powinny zostać dodane do **obiektów blob** węzła. Zmień uprawnienia dostępu w kontenerze do publicznego kontenera. Aby to zrobić, kliknij prawym przyciskiem myszy **obrazów** kontenera, a następnie wybierz pozycję **właściwości**.

    ![obrazy kontenera właściwości](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "obrazy właściwości kontenera")

    *Właściwości kontenera obrazów*
9. W **właściwości** oknie **publicznego dostępu do odczytu** do **kontenera**.

    ![Zmiana właściwości publicznego dostępu do odczytu](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "zmiana właściwości publicznego dostępu do odczytu")

    *Zmiana właściwości publicznego dostępu do odczytu*
10. Po wyświetleniu monitu, jeśli masz pewność, którą chcesz zmienić właściwość publicznego dostępu, kliknij przycisk **tak**.

    ![Ostrzeżenie programu Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "ostrzeżenie programu Microsoft Visual Studio")

    *Ostrzeżenie programu Microsoft Visual Studio*
11. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **obrazów** kontenera obiektów blob, a następnie wybierz pozycję **kontenera obiektów Blob widoku**.

    ![Wyświetl kontener obiektów Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Wyświetl kontener obiektów Blob")

    *Widok kontenera obiektów Blob*
12. Kontener obrazów powinna zostać otwarta w nowym oknie, a legenda o żadnych wpisów, które mają być wyświetlane. Kliknij przycisk **przekazywanie** ikonę, aby przekazać plik do kontenera obiektów blob.

    ![Kontener obrazów za pomocą żadnych wpisów](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "obrazy kontenera z żadnych wpisów")

    *Kontener obrazów za pomocą żadnych wpisów*
13. W **przekazywanie obiektu Blob** okno dialogowe, przejdź do **zasoby** folderu laboratorium. Wybierz **logo big.png** plik i kliknij przycisk **Otwórz**.
14. Zaczekaj, aż zostanie przekazany plik. Po zakończeniu przekazywania pliku powinny figurować w kontenerze obrazów. Kliknij prawym przyciskiem myszy wpis w pliku, a następnie wybierz pozycję **URL kopii**.

    ![Skopiuj adres URL obiektu blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "skopiuj adres URL pliku obiektu blob")

    *Skopiuj adres URL obiektu blob*
15. Otwórz program Internet Explorer i wklej adres URL. Poniższy obraz powinien być wyświetlany w przeglądarce.

    ![Obraz logo big.png z magazynu obiektów Blob Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "obrazu logo big.png z magazynu")

    *Obraz logo big.png z usługi Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Zadanie 3 — Aktualizowanie rozwiązania do korzystania z zawartości statycznej z magazynu obiektów Blob platformy Azure

W tym zadaniu skonfigurujesz **GeekQuiz** rozwiązania, aby używać obraz przekazany do usługi Azure Blob Storage (zamiast obrazu znajduje się w aplikacji sieci web) przez dodanie reguły ponownego zapisywania adresu URL platformy ASP.NET w **web.config**pliku.

1. W programie Visual Studio, otwórz **Web.config** pliku wewnątrz **GeekQuiz** projektu, a następnie zlokalizuj **&lt;system.webServer&gt;** elementu.
2. Dodaj następujący kod, aby dodać reguły, ponownego zapisywania adresu URL aktualizowania symbol zastępczy nazwą konta magazynu.

    (Code Snippet — *UrlRewriteRule WebSitesInProduction - Ex4 -*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Ponownego zapisywania adresów URL jest proces przechwytuje przychodzące żądanie sieci Web i przekierowywać żądania do innego zasobu. Adres URL poprawiania reguły informuje aparat ponownego zapisywania adresów, gdy żądanie musi zostać przekierowane i którym powinny one zostać przekierowane. Reguły ponownego zapisywania adresów składa się z dwóch ciągów: wzorzec do wyszukania w żądany adres URL (zazwyczaj przy użyciu wyrażeń regularnych), a ciągu do wzorca, Zastąp, jeśli znaleziono. Aby uzyskać więcej informacji, zobacz [ponownego zapisywania adresów URL w programie ASP.NET:](https://msdn.microsoft.com/library/ms972974.aspx).
3. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
4. Otwórz nowy **powłoki Git Bash** konsoli, aby wdrożyć zaktualizowaną aplikację w usłudze Azure App Service.
5. Wykonaj następujące polecenia, aby wypchnąć zmiany do platformy Azure. Aktualizacja *[YOUR-aplikacji-PATH]* symbolu zastępczego na ścieżkę do **GeekQuiz** rozwiązania. Użytkownik jest monitowany o podanie hasła wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Wdrażanie aktualizacji na platformie Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Wdrażanie aktualizacji na platformie Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Zadanie 4 — weryfikacja

W tym zadaniu użyjesz **programu Internet Explorer** do przeglądania **Quiz maniaków komputerowych** aplikacji i sprawdź, czy reguła działa obrazów i ponownego zapisywania adresu URL, nastąpi przekierowanie do obrazu w serwisie **obiektów Blob platformy Azure Magazyn**.

1. Otwórz program Internet Explorer i przejdź do aplikacji sieci web (np. `http://<your-web-site>.azurewebsites.net`). Zaloguj się przy użyciu poświadczeń utworzonego wcześniej.

    ![Wyświetlanie aplikacji sieci web Quiz maniaków komputerowych z obrazem](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "przedstawiający aplikacji sieci web Quiz maniaków komputerowych z obrazem")

    *Wyświetlanie aplikacji sieci web Quiz maniaków komputerowych z obrazem*
2. Naciśnij klawisz **F12** można uruchomić narzędzia programistyczne, wybierz **sieci** kartę i Rozpocznij nagrywanie.

    ![Uruchomienie rejestrowania sieci](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "uruchomienie rejestrowania sieci")

    *Uruchomienie rejestrowania sieci*
3. Naciśnij klawisz **klawiszy CTRL + F5** odświeżenie strony sieci web.
4. Po zakończeniu ładowania strony powinna zostać wyświetlona żądania HTTP dla **/img/logo-big.png** adresu URL za pośrednictwem protokołu HTTP **301** wyników (przekierowanie) i inne żądanie `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adresu URL za pośrednictwem protokołu HTTP **200** wynik.

    ![Weryfikowanie adresu URL przekierowania](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "przedstawiający przekierowania w narzędzia programistyczne")

    *Weryfikowanie adresu URL przekierowania*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Ćwiczenie 5: Korzystania ze skalowania automatycznego dla aplikacji sieci Web

> [!NOTE]
> Jest to opcjonalne, ponieważ wymaga obsługi obciążenia sieci Web &amp; testowanie wydajności, która jest dostępna tylko dla **Visual Studio 2013 Ultimate Edition**. Aby uzyskać więcej informacji na temat określonych funkcji programu Visual Studio 2013, porównaj wersje [tutaj](https://www.microsoft.com/visualstudio/eng/products/compare).


**Usługa Azure App Service Web Apps** udostępnia funkcję automatycznego skalowania dla aplikacji sieci web działających w **Tryb standardowy**. Automatyczne skalowanie pozwala automatycznie skalować liczbę wystąpień aplikacji sieci web w taki sposób, w zależności od obciążenia platformy Azure. Po włączeniu automatycznego skalowania Azure sprawdza, czy Procesor aplikacji sieci web co pięć minut i dodaje wystąpień, zgodnie z potrzebami w danym momencie. Jeśli użycie procesora CPU jest niska, Azure spowoduje usunięcie wystąpienia co dwie godziny, aby upewnić się, że wydajność aplikacji sieci web nie ma obniżoną wydajność.

W tym ćwiczeniu, konieczne będzie przejście przez kroki wymagane do skonfigurowania **skalowania automatycznego** funkcji **Quiz maniaków komputerowych** aplikacji sieci web. Ta funkcja zostanie zweryfikowana przez uruchomienie testu obciążenia programu Visual Studio można wygenerować za mało obciążenie procesora CPU w aplikacji, aby wyzwolić uaktualnienie wystąpienia.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Zadanie 1 — Konfigurowanie skalowania automatycznego w oparciu metryki użycia Procesora

W tym zadaniu użyjesz portalu zarządzania systemu Azure można włączyć funkcję automatycznego skalowania dla aplikacji sieci web, utworzony w ćwiczeniu 2.

1. W [portalu zarządzania systemu Azure](https://manage.windowsazure.com/), wybierz opcję **witryn sieci Web** i kliknij aplikację internetową utworzoną w wersji 2 wykonywania.
2. Przejdź do **skalowania** strony. W obszarze **pojemności** zaznacz **Procesora** dla **skalowanie według metryki** konfiguracji.

    > [!NOTE]
    > Podczas skalowania w najbardziej obciążających procesor CPU, Azure dynamicznie dostosowuje liczbę wystąpień, w których korzysta aplikacja Jeśli zmieni się użycie procesora CPU.

    ![Wybranie opcji skalowania najbardziej obciążających procesor CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Wybieranie Procesora metryki automatycznego skalowania")

    *Wybranie opcji skalowania najbardziej obciążających procesor CPU*
3. Zmiana **Procesora docelowego** konfiguracji **20**-**40** procent.

    > [!NOTE]
    > Ten zakres reprezentuje średniego użycia procesora CPU dla aplikacji sieci web. Azure spowoduje dodanie lub usunięcie wystąpień, aby zachować swoją aplikację sieci web, w tym zakresie. Minimalna i maksymalna liczba wystąpień używanych do skalowania jest określony w **liczba wystąpień** konfiguracji. Azure nigdy nie zaczną się powyżej lub po przekroczeniu tego limitu.
    >
    > Wartość domyślna **Procesora docelowego** wartości są modyfikowane tylko na potrzeby tego laboratorium. Konfigurując zakres procesora CPU z małymi wartościami, zwiększasz szanse na wyzwalacza automatycznego skalowania podczas umiarkowany obciążenie aplikacji.

    ![Zmiana docelowej Procesora należeć do zakresu od 20 do 40 procent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "zmiana docelowy adres CPU należeć do zakresu od 20 do 40 procent")

    *Zmiana Procesor docelowy należeć do zakresu od 20 do 40 procent*
4. Kliknij przycisk **Zapisz** na pasku poleceń, aby zapisać zmiany.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Zadanie 2 — obciążenia, testowanie za pomocą programu Visual Studio

Teraz, gdy **skalowania automatycznego** została skonfigurowana, zostanie utworzony **załadować projekt testu wydajności sieci Web i** w programie Visual Studio, aby wygenerować obciążenie procesora CPU w aplikacji sieci web.

1. Otwórz **Visual Studio Ultimate 2013** i wybierz **pliku | Nowe | Projekt...**  można uruchomić nowego rozwiązania.

    ![Tworzenie nowego projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "tworzenia nowego projektu")

    *Tworzenie nowego projektu*
2. W **nowy projekt** okno dialogowe, wybierz opcję **projekt testu obciążenia i wydajności sieci Web** w obszarze **Visual C# | Test** kartę. Upewnij się, że **.NET Framework 4.5** jest zaznaczone, nadaj projektowi nazwę *WebAndLoadTestProject*, wybierz **lokalizacji** i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu sieci Web i testu obciążeniowego](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "tworzenia nowego projektu sieci Web i testu obciążenia")

    *Tworzenie nowego projektu sieci Web i testu obciążenia*
3. W **WebTest1.webtest** kliknij prawym przyciskiem myszy **WebTest1** węzła i kliknij przycisk **Dodaj żądania**.

    ![Dodawanie żądania do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "dodając żądania do WebTest1")

    *Dodawanie żądania do WebTest1*
4. W **właściwości** okno nowego węzła żądanie aktualizacji **adresu Url** właściwości, aby wskazywała na adres URL aplikacji sieci web (np. *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Zmiana właściwości adresu Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "zmianę właściwości adresu Url")

    *Zmiana właściwości adresu Url*
5. W **WebTest1.webtest** okna, kliknij prawym przyciskiem myszy **WebTest1** i kliknij przycisk **Dodaj pętlę...** .

    ![Dodawanie pętli do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Dodawanie pętli do WebTest1")

    *Dodawanie pętli do WebTest1*
6. W **Dodaj regułę warunkową i elementy do pętli** okno dialogowe, wybierz opcję **pętli For** reguły i zmodyfikować następujące właściwości.

   1. **Kończenie wartość:** 1000
   2. **Nazwa parametru kontekstu:** iteratora
   3. **Wartość przyrostu:** 1

      ![Wybierając reguły dla pętli i aktualizowanie właściwości](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "wybierając reguły dla pętli i aktualizowanie właściwości")

      *Wybierając reguły dla pętli i aktualizowanie właściwości*
7. W obszarze **elementy w pętli** wybierz żądania, który został wcześniej utworzony jako pierwszy i ostatni element dla pętli. Kliknij przycisk **OK** aby kontynuować.

    ![Zaznaczając pierwszy i ostatni element dla pętli](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "zaznaczając pierwszy i ostatni element dla pętli")

    *Zaznaczając pierwszy i ostatni element dla pętli*
8. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebAndLoadTestProject** projektu, rozwiń węzeł **Dodaj** menu, a następnie wybierz **testu obciążeniowego...** .

    ![Dodawanie testu obciążeniowego do projektów WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Dodawanie testu obciążeniowego do projektów WebAndLoadTestProject")

    *Dodawanie testu obciążeniowego do projektów WebAndLoadTestProject*
9. W **nowego kreatora testu obciążenia** okno dialogowe, kliknij przycisk **dalej**.

    ![Nowy Kreator testu obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "nowego kreatora testu obciążenia")

    *Nowy Kreator testu obciążenia*
10. W **scenariusza** wybierz opcję **nie używaj czasów namysłu** i kliknij przycisk **dalej**.

    ![Wybranie pozycji nie należy używać czasów reakcji](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "wybranie nie należy używać czasów reakcji")

    *Zaznaczenie nie należy używać czasów reakcji*
11. W **wzorca obciążenia** strony, upewnij się, że **stałego obciążenia** opcja jest zaznaczona. Zmiana **liczba użytkowników** ustawienie **250** użytkowników i kliknij przycisk **dalej**.

    ![Zmiana liczby użytkowników do 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "zmiana liczby użytkowników do 250")

    *Zmiana liczby użytkowników do 250*
12. W **Model testu mieszanego** wybierz opcję **oparty na sekwencyjnej kolejności testów** i kliknij przycisk **dalej**.

    ![Wybieranie modelu testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "wybierając model testu mieszanego")

    *Wybieranie modelu testu mieszanego*
13. W **Model testu mieszanego** kliknij **Dodaj...**  można dodać testu do mieszanki.

    ![Dodawanie testu do mieszanki testów](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Dodawanie testów do testu mieszanego")

    *Dodawanie testu do mieszanki testów*
14. W **Dodaj testy** okno dialogowe, kliknij dwukrotnie **WebTest1** test, aby dodać **wybrane testy** listy. Kliknij przycisk **OK** aby kontynuować.

    ![Dodawanie testu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Dodawanie testu WebTest1")

    *Dodawanie testu WebTest1*
15. Ponownie **Test mieszany** kliknij **dalej**.

    ![Kończenie pracy na stronie testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "zakończeniu strony Test mieszany")

    *Kończenie pracy na stronie testu mieszanego*
16. W **mieszany profil sieciowy** kliknij **dalej**.

    ![Kliknięcie przycisku Dalej na stronie mieszany profil sieciowy](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "kliknięcie przycisku Dalej na stronie mieszany profil sieciowy")

    *Kliknięcie przycisku Dalej na stronie mieszany profil sieciowy*
17. W **mieszaną przeglądarkę** wybierz opcję **Internet Explorer 10.0** jako typ przeglądarki i kliknij przycisk **dalej**.

    ![Wybierając typ przeglądarki](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "wybierając typ przeglądarki")

    *Wybierając typ przeglądarki*
18. W **zbiorów liczników** kliknij **dalej**.

    ![Jeśli klikniesz pozycję dalej na stronie zbiorów liczników](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "kliknięcie przycisku Dalej na stronie zbiory liczników")

    *Kliknięcie przycisku Dalej na stronie zbiory liczników*
19. W **parametrów uruchomieniowych** ustaw **czas trwania testu obciążenia** do **5 minut** i kliknij przycisk **Zakończ**.

    ![Ustawianie czas trwania testu obciążenia na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "ustawienie czas trwania testu obciążenia na 5 minut")

    *Ustawianie czas trwania testu obciążenia na 5 minut*
20. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Local.settings** plik, aby eksplorować ustawienia testu. Domyślnie program Visual Studio używa komputera lokalnego do uruchamiania testów.

    > [!NOTE]
    > Alternatywnie, można skonfigurować projektu testowego do uruchamiania testów obciążenia w chmurze za pomocą **plany testów Azure**. Plany testów platformy Azure zapewnia oparte na chmurze obciążenia, testowanie usługi, która symuluje bardziej realistycznego obciążenia, unikając ograniczenia środowiska lokalnego, takich jak wydajność procesora CPU, pamięci i przepustowość sieci. Aby uzyskać więcej informacji o korzystaniu z planów testowych platformy Azure do uruchamiania testów obciążenia, zobacz [scenariuszy testowania obciążenia](/azure/devops/test/load-test/overview?view=vsts).

    ![Ustawienia testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Zadanie 3 — Weryfikacja automatycznego skalowania

Teraz spowoduje wykonanie testu obciążenia, który został utworzony w poprzednim zadaniu i zobacz, jak Twoja aplikacja sieci web zachowuje się pod obciążeniem.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **testy LoadTest1.loadtest** do Otwórz test obciążenia.

    ![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")

    *Opening LoadTest1.loadtest*
2. W **testy LoadTest1.loadtest** okna, kliknij pierwszy przycisk w przyborniku, aby uruchomić test obciążenia.

    ![Uruchamianie testu obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Uruchamianie testu obciążenia")

    *Uruchamianie testu obciążenia*
3. Zaczekaj, aż do zakończenia testu obciążeniowego.

    > [!NOTE]
    > Zasymulowano obciążenie krokowe wielu użytkowników, które jednocześnie wysyłać żądania do aplikacji sieci web. Gdy test jest uruchomiony, możesz monitorować dostępne liczniki wykryć wszelkie błędy, ostrzeżenia lub inne informacje związane z przebiegiem testu obciążenia.

    ![Uruchomiony test obciążeniowy](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "oczekiwanie, aż do zakończenia testu obciążeniowego")

    *Test obciążenia*
4. Po zakończeniu testu, wróć do portalu zarządzania i przejdź do **skalowania** strony aplikacji sieci web. W obszarze **pojemności** sekcji, powinien zostać wyświetlony na wykresie automatycznie wdrożony nowe wystąpienie.

    ![Nowe wystąpienie automatycznie wdrożone:](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nowe wystąpienie automatycznie wdrożone:*

    > [!NOTE]
    > Może upłynąć kilka minut, zanim będą one widoczne na wykresie (naciśnij klawisz **klawiszy CTRL + F5** okresowo, aby odświeżyć stronę). Jeśli nie ma żadnych zmian, możesz wypróbować następujące czynności:
    >
    > - Zwiększ czas trwania testu obciążeniowego (np. do **10 minut**)
    > - Zmniejszenie wartości maksymalne i minimalne **Procesora docelowego** zakres w konfiguracji skalowania automatycznego w aplikacji sieci web
    > - Uruchom test obciążenia w chmurze przy użyciu **plany testów Azure**. Więcej informacji na [tutaj](/azure/devops/test/load-test/index?view=vsts)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego pokazaliśmy ci, jak skonfigurować i wdrożyć aplikację do produkcyjnych aplikacji sieci web na platformie Azure. Uruchomione przez wykrywanie i aktualizowanie baz danych przy użyciu **migracje Code First Framework jednostki**, następnie kontynuowane przez wdrożenie nowych wersji przy użyciu witryny **Git** i cofnięcia do wykonywania Najnowsza stabilna wersja lokacji. Ponadto pokazaliśmy ci, jak skalować aplikację przy użyciu magazynu, aby przenieść Twojej zawartości statycznej do kontenera obiektów Blob.
