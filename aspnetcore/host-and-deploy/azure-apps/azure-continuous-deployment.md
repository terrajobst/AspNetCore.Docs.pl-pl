---
title: "Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i Git"
author: rick-anderson
description: "Informacje o sposobie tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w usłudze Azure App Service przy użyciu usługi Git do ciągłego wdrażania."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 78f4eed188323f2f43fafbb69d3fca9b59129ad2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Ciągłe wdrażanie na platformie Azure dla platformy ASP.NET Core z programu Visual Studio i Git

Przez [Erik Reitan](https://github.com/Erikre)

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w programie Visual Studio w usłudze Azure App Service przy użyciu ciągłego wdrażania.

Zobacz też [VSTS używany do tworzenia i publikowania w ciągłego wdrażania aplikacji sieci Web Azure](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), który wskazuje, jak skonfigurować ciągłego dostarczania (CD) przepływ pracy dla [usłudze Azure App Service](/azure/app-service/app-service-web-overview) przy użyciu programu Visual Studio Team Usługi. Azure ciągłego dostarczania w Team Services upraszcza proces konfigurowania potoku niezawodne wdrożenia publikować aktualizacje dla aplikacji hostowanych w usłudze Azure App Service. Potok można skonfigurować w portalu Azure do kompilacji, uruchom testy wdrożyć miejsce wystawiania i następnie wdrożyć do środowiska produkcyjnego.

> [!NOTE]
> Do ukończenia tego samouczka, wymagane jest konto Microsoft Azure. Aby utworzyć konta [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku przyjęto założenie, że zainstalowano następujące oprogramowanie:

* [Visual Studio](https://www.visualstudio.com)
* [Zestaw SDK programu .NET core](https://www.microsoft.com/net/download/core) (środowisko uruchomieniowe i narzędzi)
* [Git](https://git-scm.com/downloads) dla systemu Windows

## <a name="create-an-aspnet-core-web-app"></a>Tworzenie aplikacji sieci web platformy ASP.NET Core

1. Uruchom program Visual Studio.

1. Z **pliku** menu, wybierz opcję **nowy** > **projektu**.

1. Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu projektu. Wygląda na to, w obszarze **zainstalowana** > **szablony** > **Visual C#** > **.NET Core**. Nazwij projekt `SampleWebAppDemo`. Wybierz **utworzyć nowe repozytorium Git** opcję i kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu](azure-continuous-deployment/_static/01-new-project.png)

1. W **nowy projekt programu ASP.NET Core** okno dialogowe, wybierz platformy ASP.NET Core **pusty** szablonu, następnie kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu platformy ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> Najbardziej aktualną wersją .NET Core jest wersja 2.0.

### <a name="running-the-web-app-locally"></a>Uruchomienie aplikacji sieci web lokalnie

1. Po zakończeniu tworzenia aplikacji programu Visual Studio Uruchom aplikację wybierając **debugowania** > **Rozpocznij debugowanie**. Alternatywnie, naciśnij klawisz **F5**.

   Może potrwać do zainicjowania programu Visual Studio i nowej aplikacji. Po zakończeniu przeglądarki zawiera uruchomionej aplikacji.

   ![Wyświetlanie okna przeglądarki uruchomiona aplikacja, która wyświetla "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Po przejrzeniu uruchomionej aplikacji sieci Web, zamknij przeglądarkę i wybierz ikonę "Zatrzymaj debugowanie" na pasku narzędzi programu Visual Studio, aby zatrzymać aplikację.

## <a name="create-a-web-app-in-the-azure-portal"></a>Tworzenie aplikacji sieci web w portalu Azure

Poniższe kroki tworzenia aplikacji sieci web w portalu Azure:

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

1. Wybierz **nowy** w lewym górnym rogu portalu interfejsu.

1. Wybierz **sieci Web i mobilność** > **sieci Web aplikacji**.

   ![Portalu Microsoft Azure: Przycisk nowego: sieci Web i mobilność w witrynie Marketplace: przycisku aplikacji sieci Web w obszarze polecane aplikacje](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. W **aplikacji sieci Web** bloku, wprowadź unikatową wartość **nazwę usługi aplikacji**.

   ![Bloku aplikacja sieci Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **Nazwę usługi aplikacji** nazwa musi być unikatowa. Portalu wymusza tej reguły, gdy została podana nazwa. Jeśli podając inną wartość, należy zastąpić tę wartość dla każdego wystąpienia **SampleWebAppDemo** w tym samouczku.

   Również w **aplikacji sieci Web** bloku, wybierz istniejący **lokalizacja planu usługi aplikacji** lub Utwórz nową. W przypadku tworzenia nowego planu, wybierz warstwę cenową, lokalizacji i innych opcji. Aby uzyskać więcej informacji na temat planów usługi aplikacji, zobacz [szczegółowe omówienie planów usługi aplikacji Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Wybierz **utworzyć**. Azure obsługi administracyjnej, a następnie uruchomić aplikacji sieci web.

   ![Portalu Azure: Blok Essentials pokaz aplikacji sieci Web przykładowej 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Włączanie publikowania Git dla nowej aplikacji sieci web

Git to system kontroli wersji rozproszonej, który może służyć do wdrażania aplikacji sieci web usługi aplikacji Azure. Kod aplikacji sieci Web jest przechowywany w lokalnym repozytorium Git i kod jest wdrożony na platformie Azure przez wypchnięcie do repozytorium zdalnego.

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

1. Wybierz **usługi aplikacji** Aby wyświetlić listę usług aplikacji skojarzone z subskrypcją platformy Azure.

1. Wybierz utworzony w poprzedniej sekcji w tym samouczku aplikacji sieci web.

1. W **wdrożenia** bloku, wybierz opcję **opcje wdrażania** > **wybierz źródło** > **lokalnego repozytorium Git**.

   ![Blok ustawień: bloku źródła wdrożenia: Wybierz źródło bloku](azure-continuous-deployment/_static/deployment-options.png)

1. Wybierz **OK**.

1. Jeśli wcześniej nie został ustawiony poświadczenia wdrażania dla publikowania aplikacji sieci web lub innych aplikacji usługi app Service, skonfiguruj je teraz:

   * Wybierz **ustawienia** > **poświadczenia wdrażania**. **Ustawić poświadczenia wdrażania** bloku jest wyświetlany.
   * Utwórz nazwę użytkownika i hasło. Zapisz hasło do późniejszego użytku podczas konfigurowania Git.
   * Wybierz **zapisać**.

1. W **aplikacji sieci Web** bloku, wybierz opcję **ustawienia** > **właściwości**. Adres URL zdalnego repozytorium Git do wdrożenia jest wyświetlany w obszarze **adres URL GIT**.

1. Kopiuj **adres URL GIT** wartość do wykorzystania później w samouczku.

   ![Portalu Azure: bloku właściwości aplikacji](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Publikowanie aplikacji sieci web w usłudze Azure App Service

W tej sekcji należy utworzyć lokalnego repozytorium Git przy użyciu programu Visual Studio i wypychania z tego repozytorium na platformie Azure do wdrażania aplikacji sieci web. Następujące etapy:

* Dodaj ustawienie zdalnego repozytorium, używając wartość adresu URL GIT lokalnego repozytorium można wdrożyć na platformie Azure.
* Zatwierdź zmiany projektu.
* Wypchnij zmiany projektu z repozytorium lokalnego do zdalnego repozytorium na platformie Azure.

1. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**. **Team Explorer** jest wyświetlany.

   ![Team Explorer połączyć karty](azure-continuous-deployment/_static/10-team-explorer.png)

1. W **Team Explorer**, wybierz pozycję **macierzystego** (ikona domową) > **ustawienia** > **ustawienia repozytorium**.

1. W **element zdalny** sekcji **ustawienia repozytorium**, wybierz pozycję **Dodaj**. **Dodaj zdalny** zostanie wyświetlone okno dialogowe.

1. Ustaw **nazwa** zdalnego do **aplikacja Azure**.

1. Ustaw wartość **pobrać** do **adres URL Git** który skopiowano Azure we wcześniejszej części tego samouczka. Należy pamiętać, że ten adres URL, który kończy się wyrazem **.git**.

   ![Zdalne okno dialogowe Edycja](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Alternatywnie, można określić zdalnego repozytorium, z **okno polecenia** otwierając **okno polecenia**, zmiana w katalogu projektu i wprowadzając polecenie. Przykład:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Wybierz **macierzystego** (ikona domową) > **ustawienia** > **ustawienia globalne**. Upewnij się, że nazwa i adres e-mail są ustawione. Wybierz **aktualizacji** w razie potrzeby.

1. Wybierz **Home** > **zmiany** aby powrócić do **zmiany** widoku.

1. Wpisz wiadomość, zatwierdzania, takich jak **początkowej Push #1** i wybierz **zatwierdzania**. Ta akcja tworzy *zatwierdzania* lokalnie.

   ![Team Explorer połączyć karty](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Alternatywnie, zatwierdzania zmieni się z **okno polecenia** otwierając **okno polecenia**, zmiana w katalogu projektu i wprowadzania poleceń git. Przykład:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Wybierz **Home** > **synchronizacji** > **akcje** > **Otwórz wiersz polecenia**. Wiersz polecenia otworzy się w katalogu projektu.

1. Wprowadź następujące polecenie w oknie wiersza polecenia:

   `git push -u Azure-SampleApp master`

1. Wprowadź Azure **poświadczenia wdrażania** hasło utworzone wcześniej w usłudze Azure.

   To polecenie uruchamia proces wypychanie pliki projektu lokalnego do platformy Azure. Dane wyjściowe z powyższego polecenia kończy się komunikat o pomyślnym wdrożeniu.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Jeśli wymagana jest współpraca w projekcie, należy wziąć pod uwagę Wypychanie do [GitHub](https://github.com) przed wypchnięciem na platformie Azure.
 
### <a name="verify-the-active-deployment"></a>Sprawdź aktywnych wdrożeń

Sprawdź, czy pomyślnie transfer aplikacji sieci web ze środowiska lokalnego do platformy Azure.

W [Azure Portal](https://portal.azure.com), wybierz aplikację sieci web. Wybierz **wdrożenia** > **opcje wdrażania**.

![Portalu Azure: Blok ustawień: blok wdrożeń pomyślnego wdrożenia](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Uruchom aplikację na platformie Azure

Teraz, gdy aplikacja sieci web jest wdrażana na platformie Azure, uruchom aplikację.

Można to zrobić na dwa sposoby:

* W portalu Azure Znajdź bloku aplikacja sieci web dla aplikacji sieci web. Wybierz **Przeglądaj** do wyświetlania aplikacji w domyślnej przeglądarce.
* Otwórz przeglądarkę i wprowadź adres URL aplikacji sieci web. Przykład:`http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Aktualizowanie aplikacji sieci web i opublikować

Po wprowadzeniu zmian w kodzie lokalnego, należy opublikować:

1. W **Eksploratora rozwiązań** programu Visual Studio Otwórz *Startup.cs* pliku.

1. W `Configure` metody, zmodyfikuj `Response.WriteAsync` metody, dzięki czemu wygląda następująco:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Zapisać zmiany w *Startup.cs*.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**. **Team Explorer** jest wyświetlany.

1. Wpisz wiadomość, zatwierdzania, takich jak `Update #2`.

1. Naciśnij klawisz **zatwierdzania** przycisk, aby zatwierdzić zmiany projektu.

1. Wybierz **Home** > **synchronizacji** > **akcje** > **Push**.

> [!NOTE]
> Alternatywnie, Wypchnij zmiany z **okno polecenia** otwierając **okno polecenia**, zmiana do katalogu projektu i wprowadzając polecenie git. Przykład:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Widok zaktualizowano aplikację sieci web na platformie Azure

Zaktualizowano aplikację sieci web wyświetlić, wybierając **Przeglądaj** z bloku aplikacja sieci web w portalu Azure lub przez otwarcie przeglądarki i wprowadzić adres URL aplikacji sieci web. Przykład:`http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Dodatkowe zasoby

* [VSTS umożliwia tworzenie i publikowanie aplikacji sieci Web platformy Azure przy użyciu ciągłe wdrażanie](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Program Kudu projektu](https://github.com/projectkudu/kudu/wiki)
