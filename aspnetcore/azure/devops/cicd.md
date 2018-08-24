---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure | Ciągła integracja i ciągłe wdrażanie
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: scaddie
ms.date: 08/17/2018
uid: azure/devops/cicd
ms.openlocfilehash: e084a6115dc7e176c17b2b318233b7a003b39a83
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2018
ms.locfileid: "41754247"
---
# <a name="continuous-integration-and-deployment"></a>Ciągła integracja i ciągłe wdrażanie

W poprzednim rozdziale utworzono lokalnego repozytorium Git dla aplikacji proste czytnik źródła danych. W tym rozdziale możesz opublikować ten kod z repozytorium GitHub i utworzyć potok DevOps programu Visual Studio Team Services (VSTS). Potok umożliwia ciągłe kompilacje i wdrożenia aplikacji. Każdego zatwierdzenia do repozytorium GitHub wyzwala kompilację i wdrażania do miejsca przejściowego aplikacji sieci Web platformy Azure.

W tej sekcji zostaną wykonane następujące zadania:

* Publikowanie kodu aplikacji w usłudze GitHub
* Odłącz lokalne wdrożenie narzędzia Git
* Utwórz konto usługi VSTS
* Utwórz projekt zespołowy w usłudze VSTS
* Utwórz definicję kompilacji
* Tworzenie potoku tworzenia wersji
* Zatwierdź zmiany w usłudze GitHub i automatycznie wdrażać na platformie Azure
* Sprawdź z potokiem metodyki DevOps w usłudze VSTS

## <a name="publish-the-apps-code-to-github"></a>Publikowanie kodu aplikacji w usłudze GitHub

1. Otwórz okno przeglądarki i przejdź do `https://github.com`.
1. Kliknij przycisk **+** listy rozwijanej w nagłówku, a następnie wybierz pozycję **nowe repozytorium**:

    ![Opcja nowego repozytorium usługi GitHub](media/cicd/github-new-repo.png)

1. Wybierz swoje konto **właściciela** listy rozwijanej i wprowadź *prosty kanału informacyjnego czytników* w **nazwę repozytorium** pola tekstowego.
1. Kliknij przycisk **tworzenia repozytorium** przycisku.
1. Otwórz powłokę poleceń komputer lokalny. Przejdź do katalogu, w którym *prosty kanału informacyjnego czytników* jest przechowywany w repozytorium Git.
1. Zmień nazwę istniejącego *pochodzenia* zdalnie *nadrzędnego*. Wykonaj następujące polecenie:
    ```console
    git remote rename origin upstream
    ```
1. Dodaj nową *pochodzenia* zdalnego wskazuje swoją kopię repozytorium w witrynie GitHub. Wykonaj następujące polecenie:
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. Publikowanie lokalnego repozytorium Git do nowo utworzonego repozytorium GitHub. Wykonaj następujące polecenie:
    ```console
    git push -u origin master
    ```
1. Otwórz okno przeglądarki i przejdź do `https://github.com/<GitHub_username>/simple-feed-reader/`. Sprawdź, czy kod jest wyświetlana w repozytorium GitHub.

## <a name="disconnect-local-git-deployment"></a>Odłącz lokalne wdrożenie narzędzia Git

Usuń lokalne wdrożenie narzędzia Git wykonując następujące kroki. Usługa VSTS zastępuje i rozszerzają funkcjonalność.

1. Otwórz [witryny Azure portal](https://portal.azure.com/)i przejdź do *przemieszczania (mywebapp\<unique_number\>/przemieszczania)* aplikacji sieci Web. Aplikacji sieci Web mogą szybko znajdować, wprowadzając *przemieszczania* w polu wyszukiwania w witrynie portal:

    ![tymczasową aplikację sieci Web wyszukiwany termin](media/cicd/portal-search-box.png)

1. Kliknij przycisk **opcje wdrażania**. Zostanie wyświetlony nowy panel. Kliknij przycisk **rozłączenia** można usunąć lokalnej dodaną w poprzednim rozdziale konfiguracji kontroli źródła Git. Potwierdź operację usunięcia, klikając przycisk **tak** przycisku.
1. Przejdź do *mywebapp < unique_number >* usługi App Service. Przypominamy pole wyszukiwania portalu można szybko zlokalizować usługi App Service.
1. Kliknij przycisk **opcje wdrażania**. Zostanie wyświetlony nowy panel. Kliknij przycisk **rozłączenia** można usunąć lokalnej dodaną w poprzednim rozdziale konfiguracji kontroli źródła Git. Potwierdź operację usunięcia, klikając przycisk **tak** przycisku.

## <a name="create-a-vsts-account"></a>Utwórz konto usługi VSTS

1. Otwórz przeglądarkę i przejdź do [strony tworzenia konta usługi VSTS](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Wpisz unikatową nazwę do **wybierz łatwą do zapamiętania nazwę** pole tekstowe w celu utworzenia adresu URL do uzyskiwania dostępu do konta usługi VSTS.
1. Wybierz **Git** przycisk radiowy, ponieważ kod znajduje się w repozytorium GitHub.
1. Kliknij przycisk **Kontynuuj** przycisku. Po krótkim czasie oczekiwania, konta i projektu zespołowego o nazwie *MyFirstProject*, są tworzone.

    ![Strona tworzenia konta usługi VSTS](media/cicd/vsts-account-creation.png)

1. Otwórz potwierdzenie e-mail wskazujące, że konto usługi VSTS i projekt gotowy do użycia. Kliknij przycisk **Rozpocznij swój projekt** przycisku:

    ![Uruchom projekt, przycisk](media/cicd/vsts-start-project.png)

1. W przeglądarce zostanie otwarty  *\<account_name\>. visualstudio.com*. Kliknij przycisk *MyFirstProject* link, aby rozpocząć konfigurowanie projektu DevOps potoku.

## <a name="configure-the-devops-pipeline"></a>Konfigurowanie potoku metodyki DevOps

Istnieją trzy różne kroki, aby zakończyć. Wykonując kroki w wynikach następujące trzy sekcje w operacyjnej potoku metodyki DevOps.

### <a name="grant-vsts-access-to-the-github-repository"></a>Udzielanie dostępu usługi VSTS do repozytorium GitHub

1. Rozwiń **lub kompilowania kodu z repozytorium zewnętrznego** właściwości accordion. Kliknij przycisk **konfiguracji kompilacji** przycisku:

    ![Konfigurowanie przycisku kompilacji](media/cicd/vsts-setup-build.png)

1. Wybierz **GitHub** opcję **wybierz źródło** sekcji:

    ![Wybierz źródło - GitHub](media/cicd/vsts-select-source.png)

1. Autoryzacja jest wymagana, zanim usługa VSTS mogą uzyskiwać dostęp do repozytorium GitHub. Wprowadź *< GitHub_username > połączenie usługi GitHub* w **nazwa połączenia** pola tekstowego. Na przykład:

    ![Nazwa połączenia usługi GitHub](media/cicd/vsts-repo-authz.png)

1. Jeśli uwierzytelnianie dwuskładnikowe jest włączone na koncie usługi GitHub, osobisty token dostępu jest wymagany. W takim przypadku kliknij **autoryzacji za pomocą osobisty token dostępu GitHub** łącza. Zobacz [oficjalnych instrukcji tworzenia tokenu osobistego dostępu GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) Aby uzyskać pomoc. Tylko *repozytorium* zakres uprawnień jest wymagane. W przeciwnym razie kliknij przycisk **autoryzacji przy użyciu protokołu OAuth** przycisku.
1. Po wyświetleniu monitu zaloguj się do konta usługi GitHub. Następnie wybierz polecenie Autoryzuj, aby udzielić dostępu do konta usługi VSTS. Jeśli to się powiedzie, jest tworzony nowy punkt końcowy usługi.
1. Kliknij przycisk wielokropka obok **repozytorium** przycisku. Wybierz *< GitHub_username > / prosty kanału informacyjnego czytników* repozytorium z listy. Kliknij przycisk **wybierz** przycisku.
1. Wybierz *wzorca* gałęzi od **domyślna gałąź dla kompilacji ręcznej i zaplanowane** listy rozwijanej. Kliknij przycisk **Kontynuuj** przycisku. Zostanie wyświetlona strona wybór szablonu.

### <a name="create-the-build-definition"></a>Utwórz definicję kompilacji

1. Na stronie Wybór szablonu wprowadź *platformy ASP.NET Core* w polu wyszukiwania:

    ![Wyszukiwanie platformy ASP.NET Core na stronie szablonu](media/cicd/vsts-template-selection.png)

1. Szablon wyniki wyszukiwania są wyświetlane. Umieść kursor nad **platformy ASP.NET Core** szablonu, a następnie kliknij przycisk **Zastosuj** przycisku.
1. **Zadania** zostanie wyświetlona karta definicji kompilacji. Kliknij przycisk **wyzwalaczy** kartę.
1. Sprawdź **Włącz ciągłą integrację** pole. W obszarze **filtry gałęzi** sekcji, upewnij się, że **typu** listy rozwijanej jest równa *Include*. Ustaw **gałęzi specyfikacji** menu rozwijane *wzorca*.

    ![Włączanie ustawień ciągłej integracji](media/cicd/vsts-enable-ci.png)

    Te ustawienia powodują kompilacji do wyzwalania wypchnięcie wszelkie zmiany do *wzorca* gałęzi w repozytorium GitHub. Ciągła Integracja jest sprawdzana w [Zatwierdź zmiany w usłudze GitHub i automatycznie wdrażać na platformie Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) sekcji.

1. Kliknij przycisk **Zapisz k & olejką** przycisk, a następnie wybierz pozycję **Zapisz** opcji:

    ![Przycisk Zapisz](media/cicd/vsts-save-build.png)

1. Zostanie wyświetlony następujący modalne okno dialogowe:

    ![Zapisz definicję kompilacji - modalne okno dialogowe](media/cicd/vsts-save-modal.png)

    Użyj domyślnego folderu z *\\* i kliknij przycisk **Zapisz** przycisku.

### <a name="create-the-release-pipeline"></a>Twórz potoki wydania

1. Kliknij przycisk **wersji** kartę projektu zespołowego. Kliknij przycisk **nowy potok** przycisku.

    ![Karta — nowy przycisk definicji wydania](media/cicd/vsts-new-release-definition.png)

    Zostanie wyświetlone okienko wyboru szablonu.

1. Na stronie Wybór szablonu wprowadź *usługi App Service* w polu wyszukiwania:

    ![Pole wyszukiwania szablonu potok wydania](media/cicd/vsts-release-template-search.png)

1. Szablon wyniki wyszukiwania są wyświetlane. Umieść kursor nad **wdrożenie usługi aplikacji Azure z gniazdem** szablonu, a następnie kliknij przycisk **Zastosuj** przycisku. **Potoku** zostanie wyświetlona karta potoku tworzenia wersji.

    ![Potok wydań kartę potoku](media/cicd/vsts-release-definition-pipeline.png)

1. Kliknij przycisk **Dodaj** znajdujący się w **artefaktów** pole. **Dodawanie artefaktu** zostanie wyświetlony panel:

    ![Potok wydań — Dodawanie panelu artefaktu](media/cicd/vsts-release-add-artifact.png)

1. Wybierz **kompilacji** Kafelek z **typ źródła** sekcji. Tego typu umożliwia łączenie potoku tworzenia wersji do definicji kompilacji.
1. Wybierz *MyFirstProject* z **projektu** listy rozwijanej.
1. Wybierz nazwę definicji kompilacji *MyFirstProject ASP.NET Core-CI*, z **źródło (definicja kompilacji)** listy rozwijanej.
1. Wybierz *najnowsze* z **domyślną wersję** listy rozwijanej. Ta opcja tworzy artefaktów generowane przez działanie najnowszych definicji kompilacji.
1. Zastąp tekst umieszczony w **alias źródła** pole tekstowe z *porzucić*.
1. Kliknij przycisk **Dodaj** przycisku. **Artefaktów** sekcji aktualizacje, aby wyświetlić zmiany.
1. Kliknij ikonę pioruna umożliwiające ciągłych wdrożeń:

    ![Potok tworzenia wersji artefaktów - ikonę pioruna](media/cicd/vsts-artifacts-lightning-bolt.png)

    Po włączeniu tej opcji wdrożenia wystąpienia każdorazowo, gdy dostępna jest nowa kompilacja.
1. A **wyzwalacz ciągłego wdrażania** po prawej stronie zostanie wyświetlony panel. Kliknij przycisk przełączania, aby włączyć tę funkcję. Nie jest wymagane do włączenia **wyzwalacza żądania ściągnięcia**.
1. Kliknij przycisk **Dodaj** listy rozwijanej w **tworzyć filtry gałęzi** sekcji. Wybierz **gałęzi domyślnej Build Definition** opcji. Ten filtr powoduje, że wydania do wyzwalania tylko w przypadku kompilacji z repozytorium GitHub *wzorca* gałęzi.
1. Kliknij przycisk **Zapisz** przycisku. Kliknij przycisk **OK** przycisku w wynikowym **Zapisz** modalne okno dialogowe.
1. Kliknij przycisk **środowisko 1** pole. **Środowiska** po prawej stronie zostanie wyświetlony panel. Zmiana *środowisko 1* tekstu w **Nazwa środowiska** polu tekstowym *produkcji*.

   ![Potok wydań — pole tekstowe Nazwa środowiska](media/cicd/vsts-environment-name-textbox.png)

1. Kliknij przycisk **fazy 1, 2 zadania** łącze w **produkcji** pola:

    ![Potok wydań — link.png środowiska produkcyjnego](media/cicd/vsts-production-link.png)

    **Zadania** pojawi się karta środowiska.
1. Kliknij przycisk **wdrożenia usługi Azure App Service do gniazda** zadania. Jego ustawienia są wyświetlane w panelu po prawej stronie.
1. Wybierz subskrypcję platformy Azure skojarzone z usługą App **subskrypcji platformy Azure** listy rozwijanej. Po wybraniu kliknij **Autoryzuj** przycisku.
1. Wybierz *aplikacji sieci Web* z **typ aplikacji** listy rozwijanej.
1. Wybierz *mywebapp / < unique_number / >* z **nazwa usługi App service** listy rozwijanej.
1. Wybierz *AzureTutorial* z **grupy zasobów** listy rozwijanej.
1. Wybierz *przemieszczania* z **miejsca** listy rozwijanej.
1. Kliknij przycisk **Zapisz** przycisku.
1. Umieść kursor nad domyślna nazwa potoku wydania. Kliknij ikonę ołówka, aby go edytować. Użyj *MyFirstProject ASP.NET Core-CD* jako nazwę.

    ![Nazwa potoku wydania](media/cicd/vsts-release-definition-name.png)

1. Kliknij przycisk **Zapisz** przycisku.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Zatwierdź zmiany w usłudze GitHub i automatycznie wdrażać na platformie Azure

1. Otwórz *SimpleFeedReader.sln* w programie Visual Studio.
1. W Eksploratorze rozwiązań Otwórz *Pages\Index.cshtml*. Zmiana `<h2>Simple Feed Reader - V3</h2>` do `<h2>Simple Feed Reader - V4</h2>`.
1. Naciśnij klawisz **Ctrl**+**Shift**+**B** do skompilowania aplikacji.
1. Przekaż plik z repozytorium GitHub. Użyj jednej **zmiany** strony w programie Visual Studio *Team Explorer* tab lub wykonaj następujące czynności za pomocą powłoki poleceń na komputerze lokalnym:

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. Wypychanie zmiany *wzorca* gałęzi do *pochodzenia* zdalnego repozytorium GitHub:

    ```console
    git push origin master
    ```

    Zatwierdzenie pojawia się w repozytorium GitHub *wzorca* gałęzi:

    ![GitHub zatwierdzenia w gałęzi głównej](media/cicd/github-commit.png)

    Kompilacja zostaje wyzwolona, ponieważ integracja ciągła jest włączone w definicji kompilacji **wyzwalaczy** karty:

    ![Włącz ciągłą integrację](media/cicd/enable-ci.png)

1. Przejdź do **kolejce** karcie **kompilowania i wydawania** > **kompilacje** strony usługi VSTS. Kompilacja w kolejce pokazuje gałęzi i zatwierdzeń, które wywołały kompilację:

    ![kolejki kompilacji](media/cicd/build-queued.png)

1. Gdy kompilacja zakończy się powodzeniem, odbywa się wdrażanie na platformie Azure. Przejdź do aplikacji w przeglądarce. Zwróć uwagę, że tekst "4" jest wyświetlany w nagłówku:

    ![zaktualizowana aplikacja](media/cicd/updated-app-v4.png)

## <a name="examine-the-vsts-devops-pipeline"></a>Sprawdź z potokiem metodyki DevOps w usłudze VSTS

### <a name="build-definition"></a>Definicja kompilacji

Utworzono definicję kompilacji o nazwie *MyFirstProject ASP.NET Core-CI*. Po zakończeniu kompilacji tworzy *zip* zasoby, które mają zostać opublikowane w tym pliku. Potok wydania służy do wdrażania tych zasobów na platformie Azure.

Definicja kompilacji **zadania** karcie wyświetlane są poszczególne kroki, które są używane. Istnieje pięć zadań kompilacji.

![Definicja zadania kompilacji](media/cicd/build-definition-tasks.png)

1. **Przywróć** &mdash; Executes `dotnet restore` polecenia w celu przywrócenia pakietów NuGet aplikacji. Domyślny pakiet źródła danych, używany jest adres nuget.org.
1. **Tworzenie** &mdash; Executes `dotnet build --configuration release` polecenie, aby skompilować kod aplikacji. To `--configuration` opcja jest używana w celu wygenerowania zoptymalizowanych wersję kodu, który jest odpowiedni dla wdrożenia w środowisku produkcyjnym. Modyfikowanie *BuildConfiguration* zmiennej w definicji kompilacji **zmienne** kartę, jeśli na przykład konfiguracji debugowania nie jest konieczne.
1. **Test** &mdash; Executes `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` polecenie, aby uruchomić testy jednostkowe aplikacji. Testy jednostkowe są wykonywane w ramach dowolnego języka C# projekt dopasowania `**/*Tests/*.csproj` glob wzorca. Wyniki testu są zapisywane w *.trx* pliku w lokalizacji określonej przez `--results-directory` opcji. Jeśli żadne testy nie powiodą się, kompilacja nie powiedzie się i nie jest wdrożona.

    > [!NOTE]
    > Aby sprawdzić, czy pracy testów jednostkowych, zmodyfikuj *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* celowo przerwanie jedno z badań. Na przykład zmienić `Assert.True(result.Count > 0);` do `Assert.False(result.Count > 0);` w `Returns_News_Stories_Given_Valid_Uri` metody. Zatwierdź i Wypchnij zmiany do usługi GitHub. Kompilacja zostanie wyzwolony i kończy się niepowodzeniem. Stan potoku kompilacji zmienia się na **nie powiodło się**. Cofnąć zmiany, zatwierdzać i wypychać ponownie. Kompilacja zakończy się pomyślnie.

1. **Publikowanie** &mdash; Executes `dotnet publish --configuration release --output <local_path_on_build_agent>` polecenia w celu utworzenia *zip* pliku z artefaktami, które mają zostać wdrożone. `--output` Opcja określa lokalizację publikowania *zip* pliku. Czy lokalizacja jest określona przez przekazanie [uprzednio zdefiniowanej zmiennej](https://docs.microsoft.com/vsts/pipelines/build/variables) o nazwie `$(build.artifactstagingdirectory)`. Tej zmiennej rozwija ścieżkę lokalną, taką jak *c:\agent\_work\1\a*, w agencie kompilacji.
1. **Publikowanie artefaktów** &mdash; Publishes *zip* za pomocą **Publikuj** zadania. Zadanie akceptuje *zip* lokalizacja jako parametr, który jest uprzednio zdefiniowanej zmiennej pliku `$(build.artifactstagingdirectory)`. *Zip* plik jest publikowany jako folder o nazwie *porzucić*.

Kliknij przycisk definicji kompilacji **Podsumowanie** link, aby wyświetlić historię kompilacji z definicji:

![Historia definicji kompilacji](media/cicd/build-definition-summary.png)

Na wynikowej stronie kliknij link odpowiadający numerowi unikatowy kompilacji:

![Strona podsumowania definicji kompilacji](media/cicd/build-definition-completed.png)

Zostanie wyświetlone podsumowanie tej określonej kompilacji. Kliknij przycisk **artefaktów** kartę i zwróć uwagę, *porzucić* znajduje się folder utworzony przez kompilację:

![Tworzenie definicji artefaktów - folder do wrzucania](media/cicd/build-definition-artifacts.png)

Użyj **Pobierz** i **Eksploruj** łącza, aby sprawdzić opublikowanych artefaktów.

### <a name="release-pipeline"></a>Potok wydania

Potok tworzenia wersji została utworzona przy użyciu nazwy *MyFirstProject ASP.NET Core-CD*:

![Omówienie potok wydania](media/cicd/release-definition-overview.png)

Są dwa główne składniki potoku tworzenia wersji **artefaktów** i **środowisk**. Kliknij pole w **artefaktów** sekcji, co spowoduje wyświetlenie następujących panelu:

![artefaktów potok wydania](media/cicd/release-definition-artifacts.png)

**Źródło (definicja kompilacji)** wartość reprezentuje definicję kompilacji, z którą jest połączony ten potok wersji. *Zip* znajduje się plik utworzony przez pomyślny przebieg definicję kompilacji do *produkcji* środowisko do wdrażania na platformie Azure. Kliknij przycisk *fazy 1, 2 zadania* łącze w *produkcji* pole środowiska, aby wyświetlić zadania potoku wydania:

![zadania potoku wydania](media/cicd/release-definition-tasks.png)

Potok wersji składa się z dwóch zadań: *wdrożenia usługi Azure App Service do gniazda* i *zarządzania usługi Azure App Service — gniazda wymiany*. Kliknięcie pierwszego zadania, co spowoduje wyświetlenie następującej konfiguracji zadania:

![Zadanie wdrażania potoku tworzenia wersji](media/cicd/release-definition-task1.png)

Subskrypcja platformy Azure, typ usługi, nazwa aplikacji sieci web, grupy zasobów i miejsce wdrożenia są definiowane w zadania wdrażania. **Pakietu lub folderu** zawiera pole tekstowe *zip* ścieżka pliku do wyodrębniane i wdrożyć *przemieszczania* gniazda *mywebapp\<unikatowy _number\>*  aplikacji sieci web.

Kliknięcie zadania zamiany gniazda, co spowoduje wyświetlenie następującej konfiguracji zadania:

![Zwolnij zadanie zamiany gniazda potoku](media/cicd/release-definition-task2.png)

Subskrypcja, grupa zasobów, typ usługi, nazwa aplikacji sieci web i szczegóły miejsca wdrożenia znajdują się. **Wymiany z produkcją** zaznaczono pole wyboru. W związku z tym, wdrożone usługi bits *przemieszczania* gniazda zostały zamienione w środowisku produkcyjnym.

## <a name="additional-reading"></a>Materiały uzupełniające

* [Tworzenie aplikacji platformy ASP.NET Core](https://docs.microsoft.com/vsts/build-release/apps/aspnet/build-aspnet-core)
* [Tworzenie i wdrażanie aplikacji sieci Web platformy Azure](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp)
* [Zdefiniuj proces kompilacji ciągłej integracji do repozytorium GitHub](https://docs.microsoft.com/vsts/pipelines/build/ci-build-github)
