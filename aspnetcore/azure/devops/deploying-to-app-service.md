---
title: Wdrażanie aplikacji w usłudze App Service — metodyki DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Wdrażanie aplikacji ASP.NET Core na usłudze Azure App Service, pierwszym krokiem dla metodyki DevOps z platformą ASP.NET Core i platformy Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657746"
---
# <a name="deploy-an-app-to-app-service"></a>Wdrażanie aplikacji w usłudze App Service

[Azure App Service](/azure/app-service/) to platforma hostingu w sieci Web platformy Azure. Wdrażanie aplikacji sieci web w usłudze Azure App Service może odbywać się ręcznie lub za pomocą zautomatyzowanego procesu. W tej sekcji przewodnika omówiono metod wdrażania, które mogą być wyzwalane ręcznie lub za pomocą skryptu przy użyciu wiersza polecenia lub wyzwalane ręcznie przy użyciu programu Visual Studio.

W tej sekcji opisano następujące zadania:

* Pobierz i skompiluj aplikację przykładową.
* Tworzenie usługi Azure App Service Web Apps przy użyciu usługi Azure Cloud Shell.
* Wdróż przykładową aplikację na platformie Azure przy użyciu narzędzia Git.
* Wdrażanie zmiany w aplikacji przy użyciu programu Visual Studio.
* Dodaj miejsce na tymczasową do aplikacji sieci web.
* Wdróż aktualizację w miejscu przejściowym.
* Zamienić gniazd środowisk przejściowych i produkcyjnych.

## <a name="download-and-test-the-app"></a>Pobieranie i testowanie aplikacji

Aplikacja używana w tym przewodniku jest wstępnie zbudowaną aplikacją ASP.NET Core, [prostym czytnikiem strumieniowego źródła danych](https://github.com/Azure-Samples/simple-feed-reader/). Jest to aplikacja Razor Pages, która korzysta z interfejsu API `Microsoft.SyndicationFeed.ReaderWriter` do pobierania źródła danych RSS/Atom i wyświetlania na liście elementów wiadomości.

Możesz przejrzeć kod, ale jest ważne dowiedzieć się, że nie ma nic specjalnego o tej aplikacji. Po prostu prostą aplikację platformy ASP.NET Core jest celach ilustracyjnych.

Z powłoki poleceń należy pobrać kod, skompilować projekt i uruchomić go w następujący sposób.

> *Uwaga: Użytkownicy systemu Linux/macOS powinni wprowadzać odpowiednie zmiany w ścieżkach, np. przy użyciu ukośnika (`/`), a nie ukośnika odwrotnego (`\`).*

1. Klonowanie kodu do folderu na komputerze lokalnym.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Zmień folder roboczy na utworzony folder *czytnika źródła danych* .

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Przywróć pakiety i Skompiluj rozwiązanie.

    ```dotnetcli
    dotnet build
    ```

4. Uruchom aplikację.

    ```dotnetcli
    dotnet run
    ```

    ![Polecenia dotnet, uruchom polecenie zakończy się pomyślnie](./media/deploying-to-app-service/dotnet-run.png)

5. Otwórz przeglądarkę i przejdź pod adres `http://localhost:5000`. Aplikacja pozwala wpisać lub wkleić zespolonego adres URL źródła danych i wyświetlanie listy elementów wiadomości.

     ![Aplikacja wyświetlania zawartości ze źródła danych RSS](./media/deploying-to-app-service/app-in-browser.png)

6. Po upewnieniu się, że aplikacja działa prawidłowo, zamknij ją, naciskając klawisz **Ctrl**+**C** w powłoce poleceń.

## <a name="create-the-azure-app-service-web-app"></a>Tworzenie aplikacji sieci Web w usłudze Azure App Service

Aby wdrożyć aplikację, musisz utworzyć App Service [aplikację sieci Web](/azure/app-service/app-service-web-overview). Po utworzeniu aplikacji sieci Web będzie można wdrożyć na go z komputera lokalnego przy użyciu narzędzia Git.

1. Zaloguj się do usługi [Azure Cloud Shell](https://shell.azure.com/bash). Uwaga: Po zalogowaniu się po raz pierwszy Cloud Shell monituje o utworzenie konta magazynu dla plików konfiguracyjnych. Zaakceptuj wartości domyślne lub Podaj unikatową nazwę.

2. Użyliśmy usługi Cloud Shell dla następujących kroków.

    a. Zadeklaruj zmienną do przechowywania nazwy aplikacji sieci web. Nazwa musi być unikatowa, ma być używany w domyślnego adresu URL. Używanie `$RANDOM` funkcji bash do konstruowania nazwy gwarantuje unikatowość i wyniki w formacie `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Utwórz grupę zasobów. Grupy zasobów zapewniają środki do agregowania zasobów platformy Azure, które mają być zarządzane jako grupa.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    `az` polecenie wywołuje [interfejs wiersza polecenia platformy Azure](/cli/azure/). Interfejs wiersza polecenia, które mogą być uruchamiane lokalnie, ale jej używanie w usłudze Cloud Shell oszczędza czas i konfiguracji.

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

    f. Skonfiguruj aplikację sieci Web tak, aby akceptowała wdrożenia z lokalnego narzędzia Git i wyświetlała *adres URL wdrożenia narzędzia Git*. **Zanotuj ten adres URL później**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Wyświetl *adres URL aplikacji sieci Web*. Przejdź do tego adresu URL, aby zobaczyć aplikację internetową puste. **Zanotuj ten adres URL później**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Przy użyciu powłoki poleceń na komputerze lokalnym przejdź do folderu projektu aplikacji sieci Web (na przykład `.\simple-feed-reader\SimpleFeedReader`). Wykonaj następujące polecenia, aby konfiguracja repozytorium Git do wypychania na adres URL wdrożenia:

    a. Dodaj zdalny adres URL do lokalnego repozytorium.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Wypchnij lokalną gałąź *główną* do *głównej* gałęzi *Azure-prod* zdalnego.

    ```console
    git push azure-prod master
    ```

    Zostanie wyświetlony monit o poświadczenia wdrażania, która została utworzona wcześniej. Sprawdzanie danych wyjściowych, w powłoce poleceń. Azure tworzy aplikację ASP.NET Core zdalnie.

4. W przeglądarce przejdź do *adresu URL aplikacji sieci Web* i zanotuj, że aplikacja została skompilowana i wdrożona. Dodatkowe zmiany można zatwierdzić w lokalnym repozytorium git przy użyciu `git commit`. Te zmiany są przekazywane do platformy Azure przy użyciu poprzedniego polecenia `git push`.

## <a name="deployment-with-visual-studio"></a>Wdrażanie za pomocą programu Visual Studio

> *Uwaga: Ta sekcja dotyczy tylko systemu Windows. Użytkownicy systemów Linux i macOS powinni wprowadzić zmiany opisane w kroku 2 poniżej. Zapisz plik i zatwierdź zmianę w repozytorium lokalnym przy użyciu `git commit`. Na koniec wypchnij zmianę o `git push`, jak w pierwszej sekcji.*

Aplikacja została już wdrożona z powłoki poleceń. Utwórzmy wdrażanie aktualizacji w aplikacji za pomocą zintegrowanych narzędzi programu Visual Studio. W tle programu Visual Studio w ramach tak samo, jak narzędzie wiersza polecenia, ale w obrębie znajomy interfejs użytkownika programu Visual Studio.

1. Otwórz *SimpleFeedReader. sln* w programie Visual Studio.
2. W Eksplorator rozwiązań Otwórz *Pages\Index.cshtml*. Zmień `<h2>Simple Feed Reader</h2>`, aby `<h2>Simple Feed Reader - V2</h2>`.
3. Naciśnij klawisz **Ctrl**+**SHIFT**+**B** , aby skompilować aplikację.
4. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie kliknij pozycję **Publikuj**.

    ![Zrzut ekranu przedstawiający kliknij prawym przyciskiem myszy i publikowania](./media/deploying-to-app-service/publish.png)
5. Visual Studio można utworzyć nowy zasób usługi App Service, ale ta aktualizacja zostanie opublikowana przez istniejące wdrożenie. W oknie dialogowym **Wybieranie elementu docelowego publikowania** wybierz pozycję **App Service** z listy po lewej stronie, a następnie wybierz pozycję **Wybierz istniejące**. Kliknij przycisk **Opublikuj**.
6. W oknie dialogowym **App Service** upewnij się, że konto Microsoft lub organizacyjne używane do tworzenia subskrypcji platformy Azure jest wyświetlane w prawym górnym rogu. Jeśli nie, kliknij listę rozwijaną i dodaj ją.
7. Upewnij się, że wybrano poprawną **subskrypcję** platformy Azure. W obszarze **Widok**wybierz pozycję **Grupa zasobów**. Rozwiń grupę zasobów **AzureTutorial** , a następnie wybierz istniejącą aplikację sieci Web. Kliknij przycisk **OK**.

    ![Zrzut ekranu przedstawiający okno dialogowe z publikowania usługi aplikacji](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio tworzy i wdraża aplikację na platformie Azure. Przejdź do adresu URL aplikacji sieci web. Sprawdź, czy `<h2>` modyfikacji elementu jest na żywo.

![Aplikacja z tytułem zmienione](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Miejsca wdrożenia

Miejsca wdrożenia obsługuje wdrażanie przejściowe zmiany, bez wywierania wpływu na aplikacji działających w środowisku produkcyjnym. Po zweryfikowaniu przygotowaną wersję aplikacji przez zespół zapewnienia jakości można wymieniać produkcyjne oraz przejściowe miejsce. Aplikacji w środowisku tymczasowym jest podwyższany do środowiska produkcyjnego w ten sposób. Poniższe kroki tworzenia miejsca przejściowego, wdrażanie w niej kilka zmian i zamienić miejsce przejściowe z środowisku produkcyjnym po zakończeniu weryfikacji.

1. Zaloguj się do [Azure Cloud Shell](https://shell.azure.com/bash), jeśli jeszcze nie jest zalogowany.
2. Utwórz miejsca przejściowego.

    a. Utwórz miejsce wdrożenia o nazwie *tymczasowej*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Skonfiguruj miejsce przejściowe tak, aby korzystało z wdrożenia z lokalnego narzędzia Git i uzyskać adres URL wdrożenia **przemieszczania** . **Zanotuj ten adres URL później**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Wyświetlanie adresów URL w miejscu przejściowym. Przejdź do adresu URL, aby wyświetlić puste miejsce na tymczasową. **Zanotuj ten adres URL później**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. W edytorze tekstu lub programie Visual Studio zmodyfikuj ponownie *strony/index. cshtml* , aby element `<h2>` odczytuje `<h2>Simple Feed Reader - V3</h2>` i zapisać plik.

4. Zatwierdź plik w lokalnym repozytorium git, używając strony **zmiany** w karcie *Team Explorer* w programie Visual Studio lub wprowadzając następujące polecenie przy użyciu powłoki poleceń komputera lokalnego:

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. Korzystając z powłoki poleceń na komputerze lokalnym, Dodaj przejściowym adresie URL wdrożenia jako element zdalny narzędzia Git i Wypchnij zatwierdzone zmiany:

    a. Dodaj zdalny adres URL na potrzeby wdrażania przejściowego do lokalnego repozytorium Git.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Wypchnij lokalną gałąź *główną* do *głównej* gałęzi zdalnej *platformy Azure* .

    ```console
    git push azure-staging master
    ```

    Zaczekaj, aż Azure zostanie skompilowana i wdrożona aplikacja.

6. Aby sprawdzić, czy V3 został pomyślnie wdrożony do miejsca przejściowego, Otwórz dwa okna przeglądarki. W jednym oknie przejdź do oryginalnego adresu URL aplikacji sieci web. W innym oknie przejdź do tymczasowej adresu URL aplikacji sieci web. Produkcja — adres URL służy aplikacji w wersji 2. Przejściowym adresie URL służy aplikacji w wersji 3.

    ![Zrzut ekranu okna przeglądarki porównanie](./media/deploying-to-app-service/ready-to-swap.png)

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

W następnej sekcji dowiesz się, jak utworzyć potok DevOps za pomocą potoków usługi Azure.

## <a name="additional-reading"></a>Materiały uzupełniające

* [Przegląd usługi Web Apps](/azure/app-service/app-service-web-overview)
* [Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w programie Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Skonfiguruj poświadczenia wdrażania dla Azure App Service](/azure/app-service/app-service-deployment-credentials)
* [Konfigurowanie środowisk przejściowych w usłudze Azure App Service](/azure/app-service/web-sites-staged-publishing)
