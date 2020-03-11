---
title: Ciągła integracja i wdrażanie — metodyki DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Ciągła integracja i wdrażanie w infrastrukturze DevOps za pomocą platformy ASP.NET Core i platformy Azure
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: mvc, seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 5fdf52235b49119503885f92c370dc588e809ffe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655835"
---
# <a name="continuous-integration-and-deployment"></a>Ciągła integracja i ciągłe wdrażanie

W poprzednim rozdziale utworzono lokalnego repozytorium Git dla aplikacji proste czytnik źródła danych. W tym rozdziale możesz opublikować ten kod z repozytorium GitHub i budowy potoku usługom DevOps platformy Azure przy użyciu potoków usługi Azure. Potok umożliwia ciągłe kompilacje i wdrożenia aplikacji. Każdego zatwierdzenia do repozytorium GitHub wyzwala kompilację i wdrażania do miejsca przejściowego aplikacji sieci Web platformy Azure.

W tej sekcji zostaną wykonane następujące zadania:

* Publikowanie kodu aplikacji w usłudze GitHub
* Odłącz lokalne wdrożenie narzędzia Git
* Utwórz organizację DevOps platformy Azure
* Utwórz projekt zespołowy w usłudze Azure Services metodyki DevOps
* Tworzenie definicji kompilacji
* Tworzenie potoku wydania
* Zatwierdzanie zmian w usłudze GitHub i automatyczne wdrażanie na platformie Azure
* Sprawdź potoku potoki usługi Azure

## <a name="publish-the-apps-code-to-github"></a>Publikowanie kodu aplikacji w usłudze GitHub

1. Otwórz okno przeglądarki i przejdź do `https://github.com`.
1. Kliknij listę rozwijaną **+** w nagłówku, a następnie wybierz pozycję **nowe repozytorium**:

    ![Opcja nowego repozytorium usługi GitHub](media/cicd/github-new-repo.png)

1. Wybierz swoje konto z listy rozwijanej **właściciel** , a *następnie wprowadź tekst* w polu tekstowym **Nazwa repozytorium** .
1. Kliknij przycisk **Utwórz repozytorium** .
1. Otwórz powłokę poleceń komputer lokalny. Przejdź do katalogu, w którym jest przechowywane repozytorium git programu *Simple-Reader* .
1. Zmień nazwę istniejącego *pochodzenia* zdalnego na *nadrzędny*. Wykonaj następujące polecenie:

    ```console
    git remote rename origin upstream
    ```

1. Dodaj nowe *Źródło* zdalne wskazujące swoją kopię repozytorium w serwisie GitHub. Wykonaj następujące polecenie:

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. Publikowanie lokalnego repozytorium Git do nowo utworzonego repozytorium GitHub. Wykonaj następujące polecenie:

    ```console
    git push -u origin master
    ```

1. Otwórz okno przeglądarki i przejdź do `https://github.com/<GitHub_username>/simple-feed-reader/`. Sprawdź, czy kod jest wyświetlana w repozytorium GitHub.

## <a name="disconnect-local-git-deployment"></a>Odłącz lokalne wdrożenie narzędzia Git

Usuń lokalne wdrożenie narzędzia Git wykonując następujące kroki. Potoki usługi Azure (usługa DevOps platformy Azure) zastępuje i rozszerzają funkcjonalność.

1. Otwórz [Azure Portal](https://portal.azure.com/)i przejdź do aplikacji sieci Web *(mywebapp\<unique_number\>/Staging)* . Aplikację sieci Web można szybko zlokalizować *, wprowadzając w* polu wyszukiwania portalu:

    ![tymczasową aplikację sieci Web wyszukiwany termin](media/cicd/portal-search-box.png)

1. Kliknij pozycję **centrum wdrażania**. Zostanie wyświetlony nowy panel. Kliknij przycisk **Rozłącz** , aby usunąć konfigurację lokalnej kontroli źródła git, która została dodana w poprzednim rozdziale. Potwierdź operację usuwania, klikając przycisk **tak** .
1. Przejdź do *mywebapp < unique_number >* App Service. Przypominamy pole wyszukiwania portalu można szybko zlokalizować usługi App Service.
1. Kliknij pozycję **centrum wdrażania**. Zostanie wyświetlony nowy panel. Kliknij przycisk **Rozłącz** , aby usunąć konfigurację lokalnej kontroli źródła git, która została dodana w poprzednim rozdziale. Potwierdź operację usuwania, klikając przycisk **tak** .

## <a name="create-an-azure-devops-organization"></a>Utwórz organizację DevOps platformy Azure

1. Otwórz przeglądarkę i przejdź do [strony tworzenie organizacji usługi Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Wpisz unikatową nazwę w polu tekstowym **Wybierz zapamiętaną nazwę** , aby utworzyć adres URL do uzyskiwania dostępu do organizacji usługi Azure DevOps.
1. Wybierz przycisk radiowy **git** , ponieważ kod jest hostowany w repozytorium GitHub.
1. Kliknij przycisk **Kontynuuj**. Po krótkim czasie oczekiwania tworzone jest konto i projekt zespołowy o nazwie *MyFirstProject*.

    ![Strona tworzenia organizacji w usłudze Azure DevOps](media/cicd/vsts-account-creation.png)

1. Otwórz potwierdzenie e-mail wskazujące, że organizacja DevOps platformy Azure i projektu gotowy do użycia. Kliknij przycisk **Rozpocznij projekt** :

    ![Uruchom projekt, przycisk](media/cicd/vsts-start-project.png)

1. Zostanie otwarta przeglądarka *\<account_name\>. VisualStudio.com*. Kliknij link *MyFirstProject* , aby rozpocząć konfigurowanie potoku DevOps projektu.

## <a name="configure-the-azure-pipelines-pipeline"></a>Konfigurowanie potoku potoki usługi Azure

Istnieją trzy różne kroki, aby zakończyć. Wykonując kroki w wynikach następujące trzy sekcje w operacyjnej potoku metodyki DevOps.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>DevOps platformy Azure udzielanie dostępu do repozytorium GitHub

1. Rozwiń **lub Skompiluj kod z repozytorium zewnętrznego** . Kliknij przycisk **kompilacja instalacji** :

    ![Konfigurowanie przycisku kompilacji](media/cicd/vsts-setup-build.png)

1. Wybierz opcję **GitHub** z sekcji **Wybierz źródło** :

    ![Wybierz źródło - GitHub](media/cicd/vsts-select-source.png)

1. Autoryzacja jest wymagana, zanim DevOps platformy Azure mogą uzyskiwać dostęp do repozytorium GitHub. Wprowadź *< GitHub_username > połączenia GitHub* w polu tekstowym **Nazwa połączenia** . Na przykład:

    ![Nazwa połączenia usługi GitHub](media/cicd/vsts-repo-authz.png)

1. Jeśli uwierzytelnianie dwuskładnikowe jest włączone na koncie usługi GitHub, osobisty token dostępu jest wymagany. W takim przypadku kliknij link **Autoryzuj przy użyciu osobistego tokenu dostępu usługi GitHub** . Zobacz [oficjalne instrukcje tworzenia tokenu dostępu osobistego usługi GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) , aby uzyskać pomoc. Wymagany jest tylko zakres uprawnień do *repozytorium* . W przeciwnym razie kliknij przycisk **Autoryzuj przy użyciu protokołu OAuth** .
1. Po wyświetleniu monitu zaloguj się do konta usługi GitHub. Następnie wybierz polecenie Autoryzuj, aby udzielić dostępu do Twojej organizacji DevOps platformy Azure. Jeśli to się powiedzie, jest tworzony nowy punkt końcowy usługi.
1. Kliknij przycisk wielokropka obok przycisku **repozytorium** . Wybierz z listy *< GitHub_username > repozytorium/Simple-Feed-Reader* . Kliknij przycisk **Wybierz** .
1. Wybierz gałąź *główną* z **gałęzi domyślnej dla listy rozwijanej ręczne i zaplanowane kompilacje** . Kliknij przycisk **Kontynuuj**. Zostanie wyświetlona strona wybór szablonu.

### <a name="create-the-build-definition"></a>Utwórz definicję kompilacji

1. Na stronie Wybór szablonu wprowadź *ASP.NET Core* w polu wyszukiwania:

    ![Wyszukiwanie platformy ASP.NET Core na stronie szablonu](media/cicd/vsts-template-selection.png)

1. Szablon wyniki wyszukiwania są wyświetlane. Umieść kursor nad szablonem **ASP.NET Core** i kliknij przycisk **Zastosuj** .
1. Zostanie wyświetlona karta **zadania** w definicji kompilacji. Kliknij kartę **wyzwalacze** .
1. Zaznacz pole wyboru **Włącz ciągłą integrację** . W sekcji **filtry gałęzi** upewnij się, że na liście rozwijanej **typ** jest ustawiona wartość *include*. Ustaw listę rozwijaną **Specyfikacja gałęzi** z *główną*.

    ![Włączanie ustawień ciągłej integracji](media/cicd/vsts-enable-ci.png)

    Te ustawienia powodują, że kompilacja jest wyzwalana, gdy jakakolwiek zmiana jest wypychana do *głównej* gałęzi repozytorium GitHub. Ciągła integracja jest testowana w usłudze [GitHub i automatycznie wdrażana](#commit-changes-to-github-and-automatically-deploy-to-azure) w usłudze Azure.

1. Kliknij przycisk **zapisz & kolejkę** i wybierz opcję **Zapisz** :

    ![Przycisk Save (Zapisz)](media/cicd/vsts-save-build.png)

1. Zostanie wyświetlony następujący modalne okno dialogowe:

    ![Zapisz definicję kompilacji - modalne okno dialogowe](media/cicd/vsts-save-modal.png)

    Użyj domyślnego folderu *\\* i kliknij przycisk **Zapisz** .

### <a name="create-the-release-pipeline"></a>Twórz potoki wydania

1. Kliknij kartę **wydania** w projekcie zespołowym. Kliknij przycisk **nowe potoku** .

    ![Karta — nowy przycisk definicji wydania](media/cicd/vsts-new-release-definition.png)

    Zostanie wyświetlone okienko wyboru szablonu.

1. Na stronie Wybór szablonu wprowadź *App Service* w polu wyszukiwania:

    ![Pole wyszukiwania szablonu potok wydania](media/cicd/vsts-release-template-search.png)

1. Szablon wyniki wyszukiwania są wyświetlane. Umieść kursor nad **wdrożeniem Azure App Service z** szablonem miejsca, a następnie kliknij przycisk **Zastosuj** . Zostanie wyświetlona karta **potok** w potoku wydania.

    ![Potok wydań kartę potoku](media/cicd/vsts-release-definition-pipeline.png)

1. Kliknij przycisk **Dodaj** w polu **artefakty** . Zostanie wyświetlony panel **Dodaj artefakt** :

    ![Potok wydań — Dodawanie panelu artefaktu](media/cicd/vsts-release-add-artifact.png)

1. Wybierz kafelek **kompilacja** z sekcji **Typ źródła** . Tego typu umożliwia łączenie potoku tworzenia wersji do definicji kompilacji.
1. Wybierz pozycję *MyFirstProject* z listy rozwijanej **projekt** .
1. Wybierz nazwę definicji kompilacji, *MyFirstProject-ASP.NET Core-Ci*, z listy rozwijanej **Źródło (definicja kompilacji)** .
1. Wybierz pozycję *Najnowsza* z listy rozwijanej **wersja domyślna** . Ta opcja tworzy artefaktów generowane przez działanie najnowszych definicji kompilacji.
1. Zastąp tekst w polu tekstowym **aliasu źródła** obiektem *Drop*.
1. Kliknij przycisk **Dodaj**. Sekcja **artefakty** jest aktualizowana w celu wyświetlenia zmian.
1. Kliknij ikonę pioruna umożliwiające ciągłych wdrożeń:

    ![Potok tworzenia wersji artefaktów - ikonę pioruna](media/cicd/vsts-artifacts-lightning-bolt.png)

    Po włączeniu tej opcji wdrożenia wystąpienia każdorazowo, gdy dostępna jest nowa kompilacja.
1. Po prawej stronie zostanie wyświetlony panel **wyzwalacz ciągłego wdrażania** . Kliknij przycisk przełączania, aby włączyć tę funkcję. Nie jest konieczne włączenie **wyzwalacza żądania ściągnięcia**.
1. Kliknij przycisk **Dodaj** listę rozwijaną w sekcji **filtry gałęzi kompilacji** . Wybierz opcję **domyślne rozgałęzienie definicji kompilacji** . Ten filtr powoduje, że wersja jest wyzwalana tylko dla kompilacji z *głównej* gałęzi repozytorium GitHub.
1. Kliknij przycisk **Zapisz**. Kliknij przycisk **OK** w oknie dialogowym **zapisuje** modalne okno dialogowe.
1. Kliknij pole **środowisko 1** . Panel **środowiska** pojawia się po prawej stronie. Zmień tekst *środowiska 1* w polu tekstowym **Nazwa środowiska** na środowisko *produkcyjne*.

   ![Potok wydań — pole tekstowe Nazwa środowiska](media/cicd/vsts-environment-name-textbox.png)

1. Kliknij link **1 fazy, 2 zadania** w polu **produkcja** :

    ![Potok wydań — link.png środowiska produkcyjnego](media/cicd/vsts-production-link.png)

    Zostanie wyświetlona karta **zadania** środowiska.
1. Kliknij zadanie **wdróż Azure App Service w miejscu** . Jego ustawienia są wyświetlane w panelu po prawej stronie.
1. Wybierz subskrypcję platformy Azure skojarzoną z App Serviceą z listy rozwijanej **subskrypcja platformy Azure** . Po wybraniu kliknij przycisk **Autoryzuj** .
1. Wybierz pozycję *aplikacja sieci Web* z listy rozwijanej **Typ aplikacji** .
1. Wybierz pozycję *mywebapp/< unique_number/>* z listy rozwijanej **nazwa usługi App Service** .
1. Wybierz pozycję *AzureTutorial* z listy rozwijanej **Grupa zasobów** .
1. Wybierz pozycję *przemieszczanie* z listy rozwijanej **miejsce** .
1. Kliknij przycisk **Zapisz**.
1. Umieść kursor nad domyślna nazwa potoku wydania. Kliknij ikonę ołówka, aby go edytować. Użyj *MyFirstProject-ASP.NET Core-CD* jako nazwy.

    ![Nazwa potoku wydania](media/cicd/vsts-release-definition-name.png)

1. Kliknij przycisk **Zapisz**.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Zatwierdzanie zmian w usłudze GitHub i automatyczne wdrażanie na platformie Azure

1. Otwórz *SimpleFeedReader. sln* w programie Visual Studio.
1. W Eksplorator rozwiązań Otwórz *Pages\Index.cshtml*. Zmień `<h2>Simple Feed Reader - V3</h2>`, aby `<h2>Simple Feed Reader - V4</h2>`.
1. Naciśnij klawisz **Ctrl**+**SHIFT**+**B** , aby skompilować aplikację.
1. Przekaż plik z repozytorium GitHub. Użyj strony **zmiany** w karcie *Team Explorer* programu Visual Studio lub wykonaj następujące czynności przy użyciu powłoki poleceń komputera lokalnego:

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. Wypchnij zmiany w gałęzi *głównej* do lokalizacji zdalnej *źródła* repozytorium GitHub:

    ```console
    git push origin master
    ```

    Zatwierdzenie pojawi się w gałęzi *głównej* repozytorium GitHub:

    ![GitHub zatwierdzenia w gałęzi głównej](media/cicd/github-commit.png)

    Kompilacja jest wyzwalana, ponieważ na karcie **wyzwalacze** definicji kompilacji jest włączona Integracja ciągła:

    ![Włącz ciągłą integrację](media/cicd/enable-ci.png)

1. Przejdź do karty z **kolejką** na stronie **kompilacje** > **Azure Pipelines** w Azure DevOps Services. Kompilacja w kolejce pokazuje gałęzi i zatwierdzeń, które wywołały kompilację:

    ![kolejki kompilacji](media/cicd/build-queued.png)

1. Gdy kompilacja zakończy się powodzeniem, odbywa się wdrażanie na platformie Azure. Przejdź do aplikacji w przeglądarce. Zwróć uwagę, że tekst "4" jest wyświetlany w nagłówku:

    ![zaktualizowana aplikacja](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Sprawdź potoku potoki usługi Azure

### <a name="build-definition"></a>Definicja kompilacji

Definicja kompilacji została utworzona przy użyciu nazwy *MyFirstProject-ASP.NET Core-Ci*. Po zakończeniu kompilacja tworzy plik *zip* , w tym zasoby do opublikowania. Potok wydania służy do wdrażania tych zasobów na platformie Azure.

Na karcie **zadania** definicji kompilacji są wyświetlane poszczególne etapy, które są używane. Istnieje pięć zadań kompilacji.

![Definicja zadania kompilacji](media/cicd/build-definition-tasks.png)

1. **Instrukcja restore** &mdash; wykonuje polecenie `dotnet restore` w celu przywrócenia pakietów NuGet aplikacji. Domyślny pakiet źródła danych, używany jest adres nuget.org.
1. **Kompilacja** &mdash; wykonuje polecenie `dotnet build --configuration release`, aby skompilować kod aplikacji. Ta `--configuration` opcja służy do tworzenia zoptymalizowanej wersji kodu, która jest odpowiednia do wdrożenia w środowisku produkcyjnym. Zmodyfikuj zmienną *BuildConfiguration* na karcie **zmienne** definicji kompilacji, jeśli na przykład wymagana jest Konfiguracja debugowania.
1. &mdash; **testowe** wykonuje polecenie `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`, aby uruchomić testy jednostkowe aplikacji. Testy jednostkowe są wykonywane w C# dowolnym projekcie zgodnym ze wzorcem `**/*Tests/*.csproj` globalizowania. Wyniki testu są zapisywane w pliku *. TRX* w lokalizacji określonej przez opcję `--results-directory`. Jeśli żadne testy nie powiodą się, kompilacja nie powiedzie się i nie jest wdrożona.

    > [!NOTE]
    > Aby sprawdzić, czy testy jednostkowe działają, zmodyfikuj *SimpleFeedReader. Tests\Services\NewsServiceTests.cs* na celowo całkowicie Przerwij jeden z testów. Na przykład zmień `Assert.True(result.Count > 0);` na `Assert.False(result.Count > 0);` w metodzie `Returns_News_Stories_Given_Valid_Uri`. Zatwierdź i Wypchnij zmiany do usługi GitHub. Kompilacja zostanie wyzwolony i kończy się niepowodzeniem. Stan potoku kompilacji zmieni się na **Niepowodzenie**. Cofnąć zmiany, zatwierdzać i wypychać ponownie. Kompilacja zakończy się pomyślnie.

1. &mdash; **publikowania** wykonuje polecenie `dotnet publish --configuration release --output <local_path_on_build_agent>`, aby utworzyć plik *zip* z artefaktami do wdrożenia. Opcja `--output` określa lokalizację publikacji pliku *zip* . Ta lokalizacja jest określona przez przekazanie [wstępnie zdefiniowanej zmiennej](/azure/devops/pipelines/build/variables) o nazwie `$(build.artifactstagingdirectory)`. Ta zmienna powiększa się do ścieżki lokalnej, takiej jak *c:\agent\_work\1\a*, na agencie kompilacji.
1. **Publikowanie artefaktu** &mdash; publikowanie pliku *zip* utworzonego przez zadanie **publikowania** . Zadanie przyjmuje lokalizację pliku *. zip* jako parametr, który jest wstępnie zdefiniowaną zmienną `$(build.artifactstagingdirectory)`. Plik *zip* jest publikowany jako folder o nazwie *Drop*.

Kliknij link **podsumowania** definicji kompilacji, aby wyświetlić historię kompilacji z definicją:

![Zrzut ekranu przedstawiający kompilacji definicji historii](media/cicd/build-definition-summary.png)

Na wynikowej stronie kliknij link odpowiadający numerowi unikatowy kompilacji:

![Strona podsumowania definicji zrzut ekranu przedstawiający kompilacji](media/cicd/build-definition-completed.png)

Zostanie wyświetlone podsumowanie tej określonej kompilacji. Kliknij kartę **artefakty** i zwróć uwagę na to, że folder *upuszczania* utworzony przez kompilację zostanie wyświetlony na liście:

![Zrzut ekranu przedstawiający artefaktów w definicji kompilacji - folder do wrzucania](media/cicd/build-definition-artifacts.png)

Użyj linków **pobierania** i **eksplorowania** , aby sprawdzić opublikowane artefakty.

### <a name="release-pipeline"></a>Potok wydania

Potok wydania został utworzony przy użyciu nazwy *MyFirstProject-ASP.NET Core-CD*:

![Zrzut ekranu przedstawiający wersji potoku Przegląd](media/cicd/release-definition-overview.png)

Dwa główne składniki potoku wydania to **artefakty** i **środowiska**. Kliknięcie pola w sekcji **artefakty** ujawnia następujący panel:

![Zrzut ekranu przedstawiający wersji potoku artefaktów](media/cicd/release-definition-artifacts.png)

Wartość **źródłowa (definicji kompilacji)** reprezentuje definicję kompilacji, z którą połączony jest potok wersji. Plik *zip* utworzony przez pomyślne uruchomienie definicji kompilacji jest dostarczany do środowiska *produkcyjnego* w celu wdrożenia na platformie Azure. Kliknij link *1 fazy, 2 zadania* w polu środowisko *produkcyjne* , aby wyświetlić zadania potoku wydania:

![Zrzut ekranu przedstawiający wersji potok zadań](media/cicd/release-definition-tasks.png)

Potok wersji składa się z dwóch zadań: *wdróż Azure App Service w gnieździe* i *Zarządzaj wymianą między gniazdami Azure App Service*. Kliknięcie pierwszego zadania, co spowoduje wyświetlenie następującej konfiguracji zadania:

![Zadanie wdrażania zrzut ekranu przedstawiający wersji potoku](media/cicd/release-definition-task1.png)

Subskrypcja platformy Azure, typ usługi, nazwa aplikacji sieci web, grupy zasobów i miejsce wdrożenia są definiowane w zadania wdrażania. Pole tekstowe **pakiet lub folder** zawiera ścieżkę pliku *. zip* , która ma zostać wyodrębniona i wdrożona w miejscu *przejściowym* *mywebapp\<unique_number\>* aplikacji sieci Web.

Kliknięcie zadania zamiany gniazda, co spowoduje wyświetlenie następującej konfiguracji zadania:

![Zrzut ekranu przedstawiający zwolnienia potoku gniazda wymiany zadania](media/cicd/release-definition-task2.png)

Subskrypcja, grupa zasobów, typ usługi, nazwa aplikacji sieci web i szczegóły miejsca wdrożenia znajdują się. Pole wyboru **Zamień z produkcją** jest zaznaczone. W związku z tym bity wdrożone w miejscu *przejściowym* są wymieniane w środowisku produkcyjnym.

## <a name="additional-reading"></a>Materiały uzupełniające

* [Tworzenie pierwszego potoku za pomocą Azure Pipelines](/azure/devops/pipelines/get-started-yaml)
* [Kompilacja i projekt .NET Core](/azure/devops/pipelines/languages/dotnet-core)
* [Wdrażanie aplikacji sieci Web za pomocą Azure Pipelines](/azure/devops/pipelines/targets/webapp)
