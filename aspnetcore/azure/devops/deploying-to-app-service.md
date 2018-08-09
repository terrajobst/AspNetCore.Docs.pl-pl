---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure | Wdrażanie aplikacji w usłudze App Service
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: abd7167b313e131dc8b7ea6a49b774e14ae53bb9
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722676"
---
# <a name="deploy-an-app-to-app-service"></a>Wdrażanie aplikacji w usłudze App Service

[Usługa Azure App Service](https://docs.microsoft.com/azure/app-service/) jest Azure platforma sieci web hostingu. Wdrażanie aplikacji sieci web w usłudze Azure App Service może odbywać się ręcznie lub za pomocą zautomatyzowanego procesu. W tej sekcji przewodnika omówiono metod wdrażania, które mogą być wyzwalane ręcznie lub za pomocą skryptu przy użyciu wiersza polecenia lub wyzwalane ręcznie przy użyciu programu Visual Studio.

W tej sekcji opisano następujące zadania:

* Pobierz i skompiluj aplikację przykładową.
* Tworzenie usługi Azure App Service Web Apps przy użyciu usługi Azure Cloud Shell.
* Wdróż przykładową aplikację na platformie Azure przy użyciu narzędzia Git.
* Wdrażanie zmiany w aplikacji przy użyciu programu Visual Studio.
* Dodaj miejsce na tymczasową do aplikacji sieci web.
* Wdróż aktualizację w miejscu przejściowym.
* Zamienić gniazd środowisk przejściowych i produkcyjnych.

## <a name="download-and-test-the-app"></a>Pobieranie i testowanie aplikacji

Aplikacja używana w tym przewodniku jest wstępnie skompilowanych aplikacji ASP.NET Core, [proste czytnik źródła danych](https://github.com/Azure-Samples/simple-feed-reader/). Jest to aplikacja stron Razor, który używa `Microsoft.SyndicationFeed.ReaderWriter` interfejsu API, aby pobrać źródła danych RSS/Atom i wyświetlanie elementów wiadomości na liście.

Możesz przejrzeć kod, ale jest ważne dowiedzieć się, że nie ma nic specjalnego o tej aplikacji. Po prostu prostą aplikację platformy ASP.NET Core jest celach ilustracyjnych.

Z powłoki poleceń należy pobrać kod, skompilować projekt i uruchomić go w następujący sposób.

> *Uwaga: Użytkownicy systemu Linux/macOS należy wprowadzenie odpowiednich zmian dla ścieżek, np. przy użyciu ukośnika (`/`) zamiast ukośnika (`\`).*

1. Klonowanie kodu do folderu na komputerze lokalnym.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Zmienić folder roboczy do *prosty kanału informacyjnego czytników* folder, który został utworzony.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Przywróć pakiety i Skompiluj rozwiązanie.

    ```console
    dotnet build
    ```

4. Uruchom aplikację.

    ```console
    dotnet run
    ```

    ![Polecenia dotnet, uruchom polecenie zakończy się pomyślnie](./media/deploying-to-app-service/dotnet-run.png)

5. Otwórz przeglądarkę i przejdź do `http://localhost:5000`. Aplikacja pozwala wpisać lub wkleić zespolonego adres URL źródła danych i wyświetlanie listy elementów wiadomości.

     ![Aplikacja wyświetlania zawartości ze źródła danych RSS](./media/deploying-to-app-service/app-in-browser.png)

6. Po zakończeniu aplikacja działa prawidłowo, zamknij go, naciskając klawisz **Ctrl**+**C** w powłoce poleceń.

## <a name="create-the-azure-app-service-web-app"></a>Tworzenie aplikacji sieci Web w usłudze Azure App Service

Aby wdrożyć aplikację, musisz utworzyć usługi App Service [aplikacji sieci Web](https://docs.microsoft.com/azure/app-service/app-service-web-overview). Po utworzeniu aplikacji sieci Web będzie można wdrożyć na go z komputera lokalnego przy użyciu narzędzia Git.

1. Zaloguj się do [usługi Azure Cloud Shell](https://shell.azure.com/bash). Uwaga: Po zalogowaniu się po raz pierwszy Cloud Shell monituje o utworzenie konta magazynu dla plików konfiguracyjnych. Zaakceptuj wartości domyślne lub Podaj unikatową nazwę.

2. Użyliśmy usługi Cloud Shell dla następujących kroków.

    a. Zadeklaruj zmienną do przechowywania nazwy aplikacji sieci web. Nazwa musi być unikatowa, ma być używany w domyślnego adresu URL. Za pomocą `$RANDOM` funkcję Bash, aby skonstruować nazwę gwarantuje unikatowość i wyniki w formacie `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Utwórz grupę zasobów. Grupy zasobów zapewniają środki do agregowania zasobów platformy Azure, które mają być zarządzane jako grupa.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    `az` Olecenie wywołuje [wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/). Interfejs wiersza polecenia, które mogą być uruchamiane lokalnie, ale jej używanie w usłudze Cloud Shell oszczędza czas i konfiguracji.

    c. Utwórz plan usługi App Service w warstwie S1. Plan usługi App Service to grupa aplikacji sieci web, które udostępnianie tej samej warstwie cenowej. W warstwie S1 jest bezpłatne, ale jest to wymagane dla funkcji miejsca przejściowego.

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. Utwórz zasób aplikacji sieci web przy użyciu planu usługi App Service w tej samej grupie zasobów.

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. Ustaw poświadczenia wdrażania. Te poświadczenia wdrożenia dotyczą wszystkich aplikacji sieci web w ramach subskrypcji. Nie należy używać znaków specjalnych w nazwie użytkownika.

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. Konfigurowanie aplikacji sieci web, aby zaakceptować wdrożenia z poziomu lokalnego narzędzia Git i wyświetlanie *adres URL wdrożenia Git*. **Należy pamiętać, ten adres URL dla odwołania później**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Wyświetlanie *adres URL aplikacji sieci web*. Przejdź do tego adresu URL, aby zobaczyć aplikację internetową puste. **Należy pamiętać, ten adres URL dla odwołania później**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Przy użyciu powłoki poleceń na komputerze lokalnym, przejdź do folderu projektu aplikacji sieci web (na przykład `.\simple-feed-reader\SimpleFeedReader`). Wykonaj następujące polecenia, aby konfiguracja repozytorium Git do wypychania na adres URL wdrożenia:

    a. Dodaj zdalny adres URL do lokalnego repozytorium.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Wypychanie lokalnej *wzorca* gałęzi do *produkcyjne azure* firmy zdalne *wzorca* gałęzi.

    ```console
    git push azure-prod master
    ```

    Zostanie wyświetlony monit o poświadczenia wdrażania, która została utworzona wcześniej. Sprawdzanie danych wyjściowych, w powłoce poleceń. Azure tworzy aplikację ASP.NET Core zdalnie.

4. W przeglądarce przejdź do *adres URL aplikacji sieci Web* i zwróć uwagę, aplikacja została skompilowana i wdrożona. Dodatkowe zmiany mogą zostać zatwierdzone do lokalnego repozytorium Git z `git commit`. Zmiany te są wypychane na platformie Azure z poprzednim okresem `git push` polecenia.

## <a name="deployment-with-visual-studio"></a>Wdrażanie za pomocą programu Visual Studio

> *Uwaga: Ta sekcja dotyczy Windows tylko. Użytkownicy systemu Linux i macOS wprowadzić zmiany, opisanej w kroku 2 poniżej. Zapisz plik i zatwierdź zmianę w repozytorium lokalnym za pomocą `git commit`. Na koniec wypchnąć zmiany z `git push`, jak w pierwszej sekcji.*

Aplikacja została już wdrożona z powłoki poleceń. Utwórzmy wdrażanie aktualizacji w aplikacji za pomocą zintegrowanych narzędzi programu Visual Studio. W tle programu Visual Studio w ramach tak samo, jak narzędzie wiersza polecenia, ale w obrębie znajomy interfejs użytkownika programu Visual Studio.

1. Otwórz *SimpleFeedReader.sln* w programie Visual Studio.
2. W Eksploratorze rozwiązań Otwórz *Pages\Index.cshtml*. Zmiana `<h2>Simple Feed Reader</h2>` do `<h2>Simple Feed Reader - V2</h2>`.
3. Naciśnij klawisz **Ctrl**+**Shift**+**B** do skompilowania aplikacji.
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nad projektem, a następnie kliknij przycisk **Publikuj**.

    ![Kliknij prawym przyciskiem myszy, publikowanie](./media/deploying-to-app-service/publish.png)
5. Visual Studio można utworzyć nowy zasób usługi App Service, ale ta aktualizacja zostanie opublikowana przez istniejące wdrożenie. W **wybierz lokalizację docelową publikowania** okno dialogowe, wybierz opcję **usługi App Service** z listy po lewej stronie, a następnie wybierz pozycję **wybierz istniejącą**. Kliknij przycisk **publikowania**.
6. W **usługi App Service** okna dialogowego, upewnij się, że firmy Microsoft lub konto organizacyjne użyte do utworzenia subskrypcji platformy Azure jest wyświetlana w prawym górnym rogu. Jeśli nie, kliknij listę rozwijaną i dodaj ją.
7. Upewnij się, że poprawne Azure **subskrypcji** jest zaznaczone. Aby uzyskać **widoku**, wybierz opcję **grupy zasobów**. Rozwiń **AzureTutorial** grupy zasobów, a następnie wybierz istniejącą aplikację sieci web. Kliknij przycisk **OK**.

    ![Publikowanie w oknie dialogowym App Service](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio tworzy i wdraża aplikację na platformie Azure. Przejdź do adresu URL aplikacji sieci web. Sprawdzić, czy `<h2>` modyfikacji elementu jest aktywna.

![Aplikacja z tytułem zmienione](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Miejsca wdrożenia

Miejsca wdrożenia obsługuje wdrażanie przejściowe zmiany, bez wywierania wpływu na aplikacji działających w środowisku produkcyjnym. Po zweryfikowaniu przygotowaną wersję aplikacji przez zespół zapewnienia jakości można wymieniać produkcyjne oraz przejściowe miejsce. Aplikacji w środowisku tymczasowym jest podwyższany do środowiska produkcyjnego w ten sposób. Poniższe kroki tworzenia miejsca przejściowego, wdrażanie w niej kilka zmian i zamienić miejsce przejściowe z środowisku produkcyjnym po zakończeniu weryfikacji.

1. Zaloguj się do [usługi Azure Cloud Shell](https://shell.azure.com/bash), jeśli nie zostało to zrobione.
2. Utwórz miejsca przejściowego.

    a. Utwórz miejsce wdrożenia o nazwie *przemieszczania*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Konfigurowanie miejsca przejściowego, przy użyciu wdrażania z lokalnej usługi Git i get **przemieszczania** adres URL wdrożenia. **Należy pamiętać, ten adres URL dla odwołania później**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Wyświetlanie adresów URL w miejscu przejściowym. Przejdź do adresu URL, aby wyświetlić puste miejsce na tymczasową. **Należy pamiętać, ten adres URL dla odwołania później**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. W edytorze tekstu lub programu Visual Studio, należy zmodyfikować *Pages/Index.cshtml* ponownie, aby `<h2>` odczytuje element `<h2>Simple Feed Reader - V3</h2>` i Zapisz plik.

4. Przekazać plik do lokalnego repozytorium Git, za pomocą **zmiany** strony w programie Visual Studio *Team Explorer* kartę, lub przez wprowadzenie następujących przy użyciu powłoki poleceń na komputerze lokalnym:

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. Korzystając z powłoki poleceń na komputerze lokalnym, Dodaj przejściowym adresie URL wdrożenia jako element zdalny narzędzia Git i Wypchnij zatwierdzone zmiany:

    a. Dodaj zdalny adres URL na potrzeby wdrażania przejściowego do lokalnego repozytorium Git.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Wypychanie lokalnej *wzorca* gałęzi do *przemieszczania azure* firmy zdalne *wzorca* gałęzi.

    ```console
    git push azure-staging master
    ```

    Zaczekaj, aż Azure zostanie skompilowana i wdrożona aplikacja.

6. Aby sprawdzić, czy V3 został pomyślnie wdrożony do miejsca przejściowego, Otwórz dwa okna przeglądarki. W jednym oknie przejdź do oryginalnego adresu URL aplikacji sieci web. W innym oknie przejdź do tymczasowej adresu URL aplikacji sieci web. Produkcja — adres URL służy aplikacji w wersji 2. Przejściowym adresie URL służy aplikacji w wersji 3.

    ![Porównywanie okna przeglądarki](./media/deploying-to-app-service/ready-to-swap.png)

7. W usłudze Cloud Shell można zamienić miejsca przejściowego zweryfikować/przygotowaniu up w środowisku produkcyjnym.

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. Sprawdź wymiany, które wystąpiły, odświeżając okna przeglądarki dwa.

    ![Porównywanie okna przeglądarki po wymiany](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>Podsumowanie

W tej sekcji wykonano następujące zadania:

* Pobrano i zbudowane dla przykładowej aplikacji.
* Utworzony usługi Azure App Service Web Apps przy użyciu usługi Azure Cloud Shell.
* Wdrożyć aplikację przykładową na platformie Azure przy użyciu narzędzia Git.
* Wdrożona zmiana w aplikacji przy użyciu programu Visual Studio.
* Dodaje gniazdo tymczasowej do aplikacji sieci web.
* Należy wdrożyć aktualizację do miejsca przejściowego.
* Zamienione miejscami środowisk przejściowych i produkcyjnych.

W następnej sekcji dowiesz się, jak utworzyć potok DevOps z platformą Azure i programu Visual Studio Team Services.

## <a name="additional-reading"></a>Materiały uzupełniające

* [Przegląd usługi Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Skonfiguruj poświadczenia wdrożenia dla usługi Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-deployment-credentials)
* [Konfigurowanie środowisk przejściowych w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing)
