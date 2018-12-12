---
title: Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git na platformie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio, a następnie wdrożyć ją w usłudze Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: e12c2ee0b78db105b431770e8644e7d19d915765
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284446"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git na platformie ASP.NET Core

przez [Erik Reitan](https://github.com/Erikre)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Ten samouczek pokazuje, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio i wdrożyć ją w programie Visual Studio w usłudze Azure App Service za pomocą ciągłego wdrażania.

Zobacz też [Tworzenie pierwszego potoku za pomocą potoków usługi Azure](/azure/devops/pipelines/get-started-yaml), który pokazuje, jak skonfigurować ciągłe dostarczanie (CD) przepływ pracy dla [usługi Azure App Service](/azure/app-service/app-service-web-overview) przy użyciu usługi DevOps platformy Azure. Potoki usługi Azure (usługa usługom DevOps platformy Azure) upraszcza konfigurowanie niezawodnego potoku wdrażania aktualizacji dla aplikacji hostowanych w usłudze Azure App Service. Potok można skonfigurować w witrynie Azure portal do kompilacji, uruchom testy, wdrożyć w miejscu przejściowym i następnie wdrażania w środowisku produkcyjnym.

> [!NOTE]
> Do ukończenia tego samouczka, wymagane jest konto Microsoft Azure. Aby utworzyć konta [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku przyjęto założenie, że zainstalowano następujące oprogramowanie:

* [Visual Studio](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://git-scm.com/downloads) dla Windows

## <a name="create-an-aspnet-core-web-app"></a>Tworzenie aplikacji internetowej platformy ASP.NET Core

1. Uruchom program Visual Studio.

1. Z **pliku** menu, wybierz opcję **New** > **projektu**.

1. Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu projektu. Wygląda na to, w obszarze **zainstalowane** > **szablony** > **Visual C#** > **platformy .NET Core**. Nadaj projektowi nazwę `SampleWebAppDemo`. Wybierz **Utwórz nowe repozytorium Git** opcji, a następnie kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu](azure-continuous-deployment/_static/01-new-project.png)

1. W **nowy projekt programu ASP.NET Core** okno dialogowe, wybierz pozycję ASP.NET Core **pusty** szablonu, następnie kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu programu ASP.NET Core](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> Najbardziej aktualną wersją platformy .NET Core jest w wersji 2.0.

### <a name="running-the-web-app-locally"></a>Uruchamianie aplikacji sieci web lokalnie

1. Po zakończeniu tworzenia aplikacji programu Visual Studio Uruchom aplikację, wybierając **debugowania** > **Rozpocznij debugowanie**. Alternatywnie, naciśnij klawisz **F5**.

   Może potrwać do zainicjowania programu Visual Studio i nową aplikację. Po zakończeniu przeglądarce pokazuje uruchomionej aplikacji.

   ![Wyświetlanie okna przeglądarki uruchomiona aplikacja, która wyświetla "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Po zapoznaniu się z działającą aplikacją sieci Web, zamknij przeglądarkę, a następnie wybierz ikonę "Zatrzymaj debugowanie" na pasku narzędzi programu Visual Studio, aby zatrzymać aplikację.

## <a name="create-a-web-app-in-the-azure-portal"></a>Tworzenie aplikacji internetowej w witrynie Azure Portal

Poniższe kroki umożliwiają utworzenie aplikacji sieci web w witrynie Azure Portal:

1. Zaloguj się do [witryny Azure Portal](https://portal.azure.com).

1. Wybierz **NEW** w lewym górnym rogu portalu interfejsu.

1. Wybierz **sieci Web i mobilność** > **aplikacja sieci Web**.

   ![Portal Microsoft Azure: Nowy przycisk: Sieć Web i mobilność w portalu Marketplace: Przycisk aplikacji sieci Web w obszarze polecane aplikacje](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. W **aplikacji sieci Web** bloku, należy wprowadzić unikatową wartość dla **nazwa usługi App Service**.

   ![Bloku aplikacji sieci Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **Nazwa usługi App Service** nazwa musi być unikatowa. Portal wymusza tę regułę, gdy została podana nazwa. Jeśli podając inną wartość, Zastąp tę wartość dla każdego wystąpienia **SampleWebAppDemo** w ramach tego samouczka.

   Również w **aplikacji sieci Web** bloku, wybierz istniejącą **Plan App Service/lokalizacja** lub utworzyć nowy. W przypadku tworzenia nowego planu, wybierz warstwę cenową, lokalizacja i inne opcje. Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [szczegółowe omówienie planów usługi Azure App Service](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Wybierz pozycję **Utwórz**. Platforma Azure będzie aprowizować i uruchomić aplikacji sieci web.

   ![Witryna Azure Portal: Blok podstawy 01 wersji demonstracyjnej aplikacji sieci Web próbki](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Włączanie publikowania usługi Git dla nowej aplikacji sieci web

Git to Rozproszony system kontroli wersji, który może służyć do wdrażania aplikacji sieci web usługi Azure App Service. Kod aplikacji internetowej jest przechowywany w lokalnym repozytorium Git, a kod jest wdrażany na platformie Azure przez wypchnięcie do repozytorium zdalnego.

1. Zaloguj się do [witryny Azure Portal](https://portal.azure.com).

1. Wybierz **App Services** Aby wyświetlić listę usług aplikacji skojarzonych z subskrypcją platformy Azure.

1. Wybierz aplikację internetową utworzoną w poprzedniej sekcji tego samouczka.

1. W **wdrożenia** bloku wybierz **opcje wdrażania** > **wybierz źródło** > **lokalnego repozytorium Git**.

   ![Blok ustawień: Blok źródła wdrożenia: Wybierz blok źródła](azure-continuous-deployment/_static/deployment-options.png)

1. Kliknij przycisk **OK**.

1. Jeśli poświadczenia wdrażania na potrzeby publikowania aplikacji sieci web lub innych aplikacji usługi app Service nie zostało jeszcze skonfigurowane, skonfiguruj je teraz:

   * Wybierz **ustawienia** > **poświadczenia wdrożenia**. **Ustaw poświadczenia wdrażania** zostanie wyświetlony blok.
   * Utwórz nazwę użytkownika i hasło. Zapisz hasło do późniejszego użycia podczas konfigurowania usługi Git.
   * Wybierz pozycję **Zapisz**.

1. W **aplikacji sieci Web** bloku wybierz **ustawienia** > **właściwości**. Adres URL zdalnego repozytorium Git do wdrożenia jest wyświetlany w obszarze **adres URL GIT**.

1. Kopiuj **adres URL GIT** wartości do późniejszego użycia w tym samouczku.

   ![Witrynie Azure Portal: blok właściwości aplikacji](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Publikowanie aplikacji sieci web w usłudze Azure App Service

W tej sekcji należy utworzyć lokalne repozytorium Git przy użyciu programu Visual Studio i wypychania z tego repozytorium na platformie Azure do wdrożenia aplikacji sieci web. Następujące etapy:

* Dodaj ustawienie repozytorium zdalnego przy użyciu wartości adres URL usługi GIT, aby można było wdrożyć lokalne repozytorium na platformie Azure.
* Zatwierdź zmiany w projekcie.
* Wypchnij zmiany projektu z repozytorium lokalnego do repozytorium zdalnego na platformie Azure.

1. W **Eksploratora rozwiązań** kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**. **Team Explorer** jest wyświetlana.

   ![Team Explorer, Połącz kartę](azure-continuous-deployment/_static/10-team-explorer.png)

1. W **Team Explorer**, wybierz opcję **macierzystego** (ikona macierzystego) > **ustawienia** > **ustawienia repozytorium**.

1. W **źródłami zdalnymi** części **ustawienia repozytorium**, wybierz opcję **Dodaj**. **Dodawanie elementu zdalnego** zostanie wyświetlone okno dialogowe.

1. Ustaw **nazwa** zdalnego do **Przykładowa aplikacja Azure**.

1. Ustaw wartość **pobrać** do **adres URL Git** , skopiowane z platformy Azure we wcześniejszej części tego samouczka. Należy pamiętać, że ten adres URL, który kończy się **.git**.

   ![Zdalne okno dialogowe Edycja](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Jako alternatywę, można określić zdalnego repozytorium z **okna polecenia** , otwierając **okna polecenia**zmiany do katalogu projektu i wprowadzając polecenie. Przykład:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Wybierz **macierzystego** (ikona macierzystego) > **ustawienia** > **ustawienia globalne**. Upewnij się, że nazwa i adres e-mail są ustawione. Wybierz **aktualizacji** w razie potrzeby.

1. Wybierz **Home** > **zmiany** aby powrócić do **zmiany** widoku.

1. Wprowadź wiadomość dotyczącą zatwierdzenia, taką jak **początkowej wypychania nr 1** i wybierz **zatwierdzenia**. Ta akcja powoduje utworzenie *zatwierdzenia* lokalnie.

   ![Team Explorer, Połącz kartę](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Jako alternatywę zatwierdzenia zmieni się z **okna polecenia** , otwierając **okna polecenia**zmiany do katalogu projektu i wprowadzania poleceń usługi git. Przykład:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Wybierz **Home** > **synchronizacji** > **akcje** > **Otwórz wiersz polecenia**. Wiersz polecenia otwiera się w katalogu projektu.

1. Wprowadź następujące polecenie w oknie wiersza polecenia:

   `git push -u Azure-SampleApp master`

1. Wprowadź Azure **poświadczenia wdrożenia** hasło utworzone wcześniej na platformie Azure.

   To polecenie uruchamia proces wypychania pliki projektu lokalnych na platformę Azure. Dane wyjściowe z powyższego polecenia kończy się z komunikatem, że wdrożenie zakończyło się pomyślnie.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Jeśli wymagana jest współpracy nad projektem, należy wziąć pod uwagę Wypychanie do [GitHub](https://github.com) przed wypchnięciem do platformy Azure.
 
### <a name="verify-the-active-deployment"></a>Sprawdź aktywne wdrożenie

Sprawdź, czy powiodła się przesyłanie aplikacji sieci web ze środowiska lokalnego do platformy Azure.

W [witryny Azure Portal](https://portal.azure.com), wybierz aplikację sieci web. Wybierz **wdrożenia** > **opcje wdrażania**.

![Witryna Azure Portal: Blok ustawień: Pomyślne wdrożenie przedstawiający bloku wdrożenia](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Uruchom aplikację na platformie Azure

Teraz, gdy aplikacja sieci web jest wdrażana na platformie Azure, uruchom aplikację.

Można to zrobić na dwa sposoby:

* W witrynie Azure Portal zlokalizuj bloku aplikacji sieci web dla aplikacji sieci web. Wybierz **Przeglądaj** do wyświetlania aplikacji w domyślnej przeglądarce.
* Otwórz przeglądarkę i wprowadź adres URL aplikacji sieci web. Przykład: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Aktualizowanie aplikacji internetowej i ponowne opublikowanie

Po wprowadzeniu zmian w kodzie lokalnych należy opublikować ponownie:

1. W **Eksploratora rozwiązań** programu Visual Studio, otwórz *Startup.cs* pliku.

1. W `Configure` metody, zmodyfikuj `Response.WriteAsync` metody, aby była ona wyświetlana w następujący sposób:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Czy zapisać zmiany *Startup.cs*.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **rozwiązania "SampleWebAppDemo"** i wybierz **zatwierdzić**. **Team Explorer** jest wyświetlana.

1. Wprowadź wiadomość dotyczącą zatwierdzenia, taką jak `Update #2`.

1. Naciśnij klawisz **zatwierdzenia** przycisk, aby zatwierdzić zmiany projektu.

1. Wybierz **Home** > **synchronizacji** > **akcje** > **wypychania**.

> [!NOTE]
> Jako alternatywę, Wypchnij zmiany z **okna polecenia** , otwierając **okna polecenia**zmiany do katalogu projektu i wprowadzając polecenie git. Przykład:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Wyświetlanie zaktualizowanej aplikacji internetowej na platformie Azure

Wyświetlanie zaktualizowanej aplikacji internetowej, wybierając **Przeglądaj** z bloku aplikacji sieci web w witrynie Azure Portal lub otwierając przeglądarkę i wprowadź adres URL aplikacji sieci web. Przykład: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Tworzenie pierwszego potoku za pomocą potoków usługi Azure](/azure/devops/pipelines/get-started-yaml)
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
* <xref:host-and-deploy/visual-studio-publish-profiles>
