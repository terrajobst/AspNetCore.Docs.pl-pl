---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio
author: rick-anderson
description: Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 7fc3644df3dcb957f2537538aaa9506c6b38a480
ms.sourcegitcommit: 79850db9e79b1705b89f466c6f2c961ff15485de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/07/2020
ms.locfileid: "75693976"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a>Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)
::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

::: moniker-end


Zobacz [publikowanie aplikacji sieci Web, aby Azure App Service przy użyciu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/publish-app-svc?view=vsmac-2019) , jeśli pracujesz na macOS.

Aby rozwiązać problem wdrożenia usługi App Service, zobacz <xref:test/troubleshoot-azure-iis>.

## <a name="set-up"></a>Konfigurowanie

* Otwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/dotnet/) Jeśli nie masz. 

## <a name="create-a-web-app"></a>tworzenie aplikacji internetowej

W programie Visual Studio strony początkowej, wybierz **Plik > Nowy > Projekt...**

![menu Plik](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Wykonaj **nowy projekt** okno dialogowe:

* W okienku po lewej stronie wybierz **platformy .NET Core**.
* W środkowym okienku wybierz **aplikacji sieci Web programu ASP.NET Core**.
* Wybierz **OK**.

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

W **Nowa aplikacja internetowa ASP.NET Core** okno dialogowe:

* Wybierz **aplikacji sieci Web**.
* Wybierz **Zmień uwierzytelnianie**.

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

**Zmień uwierzytelnianie** zostanie wyświetlone okno dialogowe. 

* Wybierz **indywidualne konta użytkowników**.
* Wybierz **OK** aby powrócić do **Nowa aplikacja internetowa ASP.NET Core**, a następnie wybierz **OK** ponownie.

![Nowe okno dialogowe uwierzytelniania sieci Web platformy ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Program Visual Studio tworzy rozwiązanie.

## <a name="run-the-app"></a>Uruchamianie aplikacji

* Naciśnij klawisze CTRL + F5, aby uruchomić projekt.
* Test **o** i **skontaktuj się z pomocą** łącza.

![Aplikacja sieci Web otwórz w programie Microsoft Edge na hoście lokalnym](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a>Rejestrowanie użytkownika

* Wybierz **zarejestrować** i rejestrowanie nowego użytkownika. Można użyć adresu e-mail fikcyjne. Podczas przesyłania, zostanie wyświetlona strona następujący błąd:

    *"Wewnętrzny błąd serwera: operacja bazy danych nie powiodła się podczas przetwarzania żądania. Wyjątek SQL: nie można otworzyć bazy danych. Zastosowanie istniejących migracji dla kontekstu bazy danych aplikacji może rozwiązać ten problem.*
* Wybierz **zastosować migracje** i, po aktualizacji strony, Odśwież stronę.

![Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania. Wyjątek SQL: nie można otworzyć bazy danych. Stosowanie migracji istniejących w kontekście bazy danych aplikacji może rozwiązać ten problem.](publish-to-azure-webapp-using-vs/_static/mig.png)

Aplikacja wyświetla adres e-mail używany do rejestrowania nowych użytkowników i **Wyloguj** łącza.

![Otwórz aplikację sieci Web w programie Microsoft Edge. Link Zarejestruj jest zastępowana przez tekst powitania email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Wdrażanie aplikacji na platformie Azure

Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania...** .

![Menu kontekstowe jest otwarte z wyróżnionym linkiem publikowania](publish-to-azure-webapp-using-vs/_static/pub.png)

W **Publikuj** okno dialogowe:

* Wybierz **platformy Microsoft Azure App Service**.
* Wybierz ikonę koła zębatego, a następnie wybierz pozycję **Utwórz profil**.
* Wybierz **Utwórz profil**.

![Okno dialogowe publikowanie](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a>Tworzenie zasobów platformy Azure

**Tworzenie usługi App Service** zostanie wyświetlone okno dialogowe:

* Wprowadź swoją subskrypcję.
* **Nazwy aplikacji**, **grupy zasobów**, i **planu usługi App Service** zostaną wypełnione pola wprowadzania. Można zachować te nazwy lub je zmienić.

![Okno dialogowe usługi App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Wybierz **usług** kartę, aby utworzyć nową bazę danych.

* Zaznacz zielony **+** ikonę, aby utworzyć nową bazę danych SQL

![Nowa baza danych SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* Wybierz **nowy...**  na **Konfigurowanie bazy danych SQL** okno dialogowe, aby utworzyć nową bazę danych.

![Nowe bazy danych SQL Database i serwera](publish-to-azure-webapp-using-vs/_static/conf.png)

**Konfiguruj serwer SQL** zostanie wyświetlone okno dialogowe.

* Wprowadź nazwę użytkownika administratora i hasło, a następnie wybierz **OK**. Domyślne można zachować **nazwy serwera**. 

> [!NOTE]
> "admin" nie jest dozwolona jako nazwa użytkownika administratora.

![Konfigurowanie programu SQL Server w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Wybierz **OK**.

Program Visual Studio zwraca **Tworzenie usługi App Service** okna dialogowego.

* Wybierz **Utwórz** na **Tworzenie usługi App Service** okna dialogowego.

![Konfigurowanie okna dialogowego baza danych SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

Visual Studio tworzy aplikację sieci Web i SQL Server na platformie Azure. Może to potrwać kilka minut. Aby uzyskać informacji na temat tworzenia zasobów, zobacz [dodatkowe zasoby](#additional-resources).

Po zakończeniu wdrażania wybierz **ustawienia**:

![Konfigurowanie programu SQL Server w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/set.png)

Na **ustawienia** strony **Publikuj** okno dialogowe:

* Rozwiń **baz danych** i sprawdź **Użyj tych parametrów połączenia w czasie wykonywania**.
* Rozwiń **migracją architektury jednostek** i sprawdź **Zastosuj tej migracji na publikowanie**.

* Wybierz pozycję **Zapisz**. Program Visual Studio zwraca **Publikuj** okna dialogowego. 

![Okno dialogowe publikowanie: panel ustawień](publish-to-azure-webapp-using-vs/_static/pubs.png)

Kliknij przycisk **publikowania**. Program Visual Studio publikuje aplikację na platformie Azure. Po zakończeniu wdrażania aplikacji jest otwarty w przeglądarce.

### <a name="test-your-app-in-azure"></a>Przetestuj swoją aplikację na platformie Azure

* Test **o** i **skontaktuj się z pomocą** łącza

* Rejestrowanie nowego użytkownika

![Aplikacja sieci Web otwarte w programie Microsoft Edge w usłudze Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Aktualizowanie aplikacji

* Edytuj *Pages/About.cshtml* Razor strony i zmienić jego zawartość. Na przykład można zmodyfikować akapit, aby powiedzieć "Hello ASP.NET Core!":

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Kliknij prawym przyciskiem myszy projekt i wybierz pozycję **publikowania...**  ponownie.

![Menu kontekstowe jest otwarte z wyróżnionym linkiem publikowania](publish-to-azure-webapp-using-vs/_static/pub.png)

* Po opublikowaniu aplikacji, sprawdź, czy dokonane zmiany są dostępne na platformie Azure.

![Sprawdź, czy zadanie zostało ukończone](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Czyszczenie

Po zakończeniu testowania aplikacji, przejdź do [witryny Azure portal](https://portal.azure.com/) i usunąć aplikację.

* Wybierz **grup zasobów**, następnie wybierz utworzoną grupę zasobów.

![Witrynie Azure Portal: Grup zasobów w menu bocznym](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* W **grup zasobów** wybierz opcję **Usuń**.

![Witrynie Azure Portal: Strony grup zasobów](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Wprowadź nazwę grupy zasobów, a następnie wybierz pozycję **Usuń**. Aplikacja i inne zasoby utworzone w ramach tego samouczka, teraz są usuwane z usługi Azure.

### <a name="next-steps"></a>Następne kroki

* <xref:host-and-deploy/azure-apps/azure-continuous-deployment>

## <a name="additional-resources"></a>Dodatkowe zasoby

* Aby uzyskać Visual Studio Code, zobacz temat [Publikowanie profilów](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).
* [Usługa Azure App Service](/azure/app-service/app-service-web-overview)
* [Grupy zasobów platformy Azure](/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [Azure SQL Database](/azure/sql-database/)
* <xref:host-and-deploy/visual-studio-publish-profiles>
* <xref:test/troubleshoot-azure-iis>
