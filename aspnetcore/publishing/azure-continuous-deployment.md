---
title: "Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i Git"
author: rick-anderson
description: "Informacje o sposobie tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w usłudze Azure App Service przy użyciu usługi Git do ciągłego wdrażania."
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Ciągłe wdrażanie na platformie Azure dla platformy ASP.NET Core, z programu Visual Studio i Git

Przez [Erik Reitan](https://github.com/Erikre)

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w programie Visual Studio w usłudze Azure App Service przy użyciu ciągłego wdrażania. 

Zobacz też [VSTS używany do tworzenia i publikowania w ciągłego wdrażania aplikacji sieci Web Azure](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), który wskazuje, jak skonfigurować ciągłego dostarczania (CD) przepływ pracy dla [usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) przy użyciu programu Visual Studio Team Usługi. Azure ciągłego dostarczania w Team Services upraszcza proces konfigurowania potoku niezawodne wdrożenia do publikowania aktualizacji dla aplikacji w usłudze Azure App Service. Potok można skonfigurować w portalu Azure do kompilacji, uruchom testy wdrożyć miejsce wystawiania i następnie wdrożyć do środowiska produkcyjnego.

> [!NOTE]
> Do ukończenia tego samouczka potrzebne jest konto Microsoft Azure. Jeśli nie masz konta, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku przyjęto założenie, że zainstalowano już następujące czynności:

* [Visual Studio](https://www.visualstudio.com)

* [Platformy ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (środowisko uruchomieniowe i narzędzi)

* [Git](https://git-scm.com/downloads) dla systemu Windows

## <a name="create-an-aspnet-core-web-app"></a>Tworzenie aplikacji sieci web platformy ASP.NET Core

1. Uruchom program Visual Studio.

2. Z **pliku** menu, wybierz opcję **nowy** > **projektu**.

3. Wybierz **aplikacji sieci Web ASP.NET** szablonu projektu. Wygląda na to, w obszarze **zainstalowana** > **szablony** > **Visual C#** > **Web**. Nazwij projekt `SampleWebAppDemo`. Wybierz **utworzyć nowe repozytorium Git** opcję i kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu](azure-continuous-deployment/_static/01-new-project.png)

4. W **nowy projekt ASP.NET** okno dialogowe, wybierz platformy ASP.NET Core **pusty** szablonu, następnie kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu platformy ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a>Uruchomienie aplikacji sieci web lokalnie

1. Po zakończeniu tworzenia aplikacji programu Visual Studio Uruchom aplikację wybierając **debugowania** -> **Rozpocznij debugowanie**. Alternatywnie, można nacisnąć klawisz **F5**.

   Może potrwać do zainicjowania programu Visual Studio i nowej aplikacji. Po zakończeniu przeglądarki zostaną wyświetlone uruchomionej aplikacji.

   ![Wyświetlanie okna przeglądarki uruchomiona aplikacja, która wyświetla "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Po przejrzeniu uruchomionej aplikacji sieci Web, zamknij przeglądarkę, a następnie kliknij przycisk "Zatrzymaj debugowanie" ikony na pasku narzędzi programu Visual Studio, aby zatrzymać aplikację.

## <a name="create-a-web-app-in-the-azure-portal"></a>Tworzenie aplikacji sieci web w portalu Azure

Poniższe kroki przeprowadzi Cię przez proces tworzenia aplikacji sieci web w portalu Azure.

1. Zaloguj się do [portalu Azure](https://portal.azure.com)
2. Wybierz **nowy** u góry po lewej portalu
3. Wybierz **sieci Web i mobilność** > **sieci Web aplikacji**

    ![Portalu Microsoft Azure: Przycisk nowego: sieci Web i mobilność w witrynie Marketplace: przycisku aplikacji sieci Web w obszarze polecane aplikacje](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  W **aplikacji sieci Web** bloku, wprowadź unikatową wartość **nazwę usługi aplikacji**.

    ![Bloku aplikacja sieci Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >**Nazwę usługi aplikacji** nazwa musi być unikatowa. Podczas próby wprowadź nazwę, portalu będzie wymuszać tej reguły. Po wprowadzeniu inną wartość, należy zastąpić tę wartość dla każdego wystąpienia **SampleWebAppDemo** widoczny w tym samouczku.

    &nbsp;
    
    Również w **aplikacji sieci Web** bloku, wybierz istniejący **lokalizacja planu usługi aplikacji** lub Utwórz nową. Jeśli tworzysz nowy plan, wybierz warstwę cenową, lokalizacji i innych opcji. Aby uzyskać więcej informacji na temat planów usługi aplikacji [szczegółowe omówienie planów usługi aplikacji Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  Kliknij przycisk **Utwórz**. Azure obsługi administracyjnej, a następnie uruchom aplikację sieci web.

    ![Portalu Azure: Blok Essentials pokaz aplikacji sieci Web przykładowej 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Włączanie publikowania Git dla nowej aplikacji sieci web

Git to system kontroli wersji rozproszonej, który służy do wdrażania aplikacji sieci web w usłudze Azure App Service. Będą przechowywane kod dla aplikacji sieci web w lokalnym repozytorium Git i wdrożony kodu na platformie Azure przez wypchnięcie do repozytorium zdalnego.

1. Zaloguj się do [Azure Portal](https://portal.azure.com), jeśli jeszcze nie jest zalogowany.

2. Kliknij przycisk **Przeglądaj**, który znajduje się w dolnej części okienka nawigacji.

3. Kliknij przycisk **aplikacje sieci Web** Aby wyświetlić listę aplikacji sieci web skojarzony z subskrypcją platformy Azure.

4. Wybierz aplikację sieci web utworzonej w poprzedniej sekcji tego samouczka.

5. Jeśli **ustawienia** bloku nie jest widoczne, wybierz **ustawienia** w **aplikacji sieci Web** bloku.

6. W **ustawienia** bloku, wybierz opcję **źródło wdrożenia** > **wybierz źródło** > **lokalnego repozytorium Git**.

   ![Blok ustawień: bloku źródła wdrożenia: Wybierz źródło bloku](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. Kliknij przycisk **OK**.

8. Jeśli nie ma poświadczeń wdrożenia do publikowania aplikacji sieci web lub innych aplikacji usługi app Service, skonfiguruj je teraz:

   * Kliknij przycisk **ustawienia** > **poświadczenia wdrażania**. **Ustawić poświadczenia wdrażania** zostanie wyświetlony blok.

   * Utwórz nazwę użytkownika i hasło.  To hasło będzie potrzebne później podczas konfigurowania Git.

   * Kliknij przycisk **zapisać**.

9. W **aplikacji sieci Web** bloku, kliknij przycisk **ustawienia** > **właściwości**. Adres URL zdalnego repozytorium Git, który będzie można wdrożyć do jest wyświetlany w obszarze **adres URL GIT**.

10. Kopiuj **adres URL GIT** wartość do wykorzystania później w samouczku.

   ![Portalu Azure: bloku właściwości aplikacji](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Publikowanie aplikacji sieci web w usłudze Azure App Service

W tej sekcji utworzysz lokalne repozytorium Git przy użyciu programu Visual Studio i wypychania z tego repozytorium na platformie Azure do wdrożenia aplikacji sieci web. Następujące etapy:

   * Dodaj odpowiednie ustawienie repozytorium zdalnego przy użyciu wartość adres URL GIT, aby umożliwić wdrażanie lokalnym repozytorium na platformie Azure.

   * Zatwierdź zmiany projektu.

   * Wypchnij zmiany projektu z lokalnym repozytorium do zdalnego repozytorium na platformie Azure.

&nbsp;
   
1.  W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**. **Team Explorer** będą wyświetlane.

    ![Team Explorer połączyć karty](azure-continuous-deployment/_static/10-team-explorer.png)

2.  W **Team Explorer**, wybierz pozycję **macierzystego** (ikona domową) > **ustawienia** > **ustawienia repozytorium**.

3.  W **element zdalny** sekcji **ustawienia repozytorium** wybierz **Dodaj**. **Dodaj zdalny** wyświetli się okno dialogowe.

4.  Ustaw **nazwa** zdalnego do **aplikacja Azure**.

5.  Ustaw wartość **pobrać** do **adres URL Git** skopiowany z platformy Azure we wcześniejszej części tego samouczka. Należy pamiętać, że ten adres URL, który kończy się wyrazem **.git**.

    ![Zdalne okno dialogowe Edycja](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >Alternatywnie, można określić zdalnego repozytorium z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzając polecenie. Na przykład:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Wybierz **macierzystego** (ikona domową) > **ustawienia** > **ustawienia globalne**. Upewnij się, że masz swoją nazwę i adres e-mail, ustaw. Może być również konieczne wybierz **aktualizacji**.

7.  Wybierz **Home** > **zmiany** aby powrócić do **zmiany** widoku.

8.  Wpisz wiadomość, zatwierdzania, takich jak **początkowej Push #1** i kliknij przycisk **zatwierdzania**. Ta akcja spowoduje utworzenie *zatwierdzania* lokalnie. Następnie należy do *synchronizacji* z platformy Azure.

    ![Team Explorer połączyć karty](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >Alternatywnie, można zatwierdzić zmiany z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzania poleceń git. Na przykład:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Wybierz **Home** > **synchronizacji** > **akcje** > **Otwórz wiersz polecenia**. Wiersz polecenia otworzy się w katalogu projektu.

10.  Wprowadź następujące polecenie w oknie wiersza polecenia:

    `git push -u Azure-SampleApp master`

11.  Wprowadź Azure **poświadczenia wdrażania** hasła utworzonego wcześniej w usłudze Azure.

    >[!NOTE]
    >Podczas wprowadzania hasła nie będą widoczne.

    To polecenie spowoduje uruchomienie procesu pchać plików lokalnych projektu na platformie Azure. Dane wyjściowe z powyższego polecenia kończy się komunikat, że wdrożenie powiodło się.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Jeśli potrzebujesz współpracować nad projektem, należy rozważyć Wypychanie do [GitHub](https://github.com) między Wypychanie do platformy Azure.
 
### <a name="verify-the-active-deployment"></a>Sprawdź aktywnych wdrożeń

Aby sprawdzić, czy pomyślnie przeniesiono aplikacji sieci web ze środowiska lokalnego do platformy Azure. Zobaczysz wymienionych pomyślne wdrożenie.

1. W [Azure Portal](https://portal.azure.com), wybierz aplikację sieci web. Następnie wybierz opcję **ustawienia** > **ciągłe wdrażanie**.

   ![Portalu Azure: Blok ustawień: blok wdrożeń pomyślnego wdrożenia](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Uruchom aplikację na platformie Azure

Teraz, wdrożono aplikację sieci web na platformie Azure, możesz uruchomić aplikację.

Można to zrobić na dwa sposoby:

* W portalu Azure Znajdź bloku aplikacja sieci web dla aplikacji sieci web, a następnie kliknij przycisk **Przeglądaj** do wyświetlania aplikacji w domyślnej przeglądarce.

* Otwórz przeglądarkę i wprowadź adres URL aplikacji sieci web. Na przykład:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Aktualizowanie aplikacji sieci web i opublikować

Po wprowadzeniu zmian w kodzie lokalnego można ponownie opublikować.

1.  W **Eksploratora rozwiązań** programu Visual Studio Otwórz *Startup.cs* pliku.

2.  W `Configure` metody, zmodyfikuj `Response.WriteAsync` metody, dzięki czemu wygląda następująco:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Zapisać zmiany w *Startup.cs*.

4.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**. **Team Explorer** będą wyświetlane.

5.  Wprowadź komunikat zatwierdzenia, takich jak:

    ```none
    Update #2
    ```

6.  Naciśnij klawisz **zatwierdzania** przycisk, aby zatwierdzić zmiany projektu.

7.  Wybierz **Home** > **synchronizacji** > **akcje** > **Push**.

>[!NOTE]
>Alternatywnie możesz wypchnąć zmiany z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzając polecenie git. Na przykład:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Widok zaktualizowano aplikację sieci web na platformie Azure

Wyświetlenie aplikacji sieci web zaktualizowane przez wybranie **Przeglądaj** z bloku aplikacja sieci web w portalu Azure lub przez otwarcie przeglądarki i wprowadzić adres URL aplikacji sieci web. Na przykład:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Publikowanie i wdrażanie](index.md)

* [Program Kudu projektu](https://github.com/projectkudu/kudu/wiki)
